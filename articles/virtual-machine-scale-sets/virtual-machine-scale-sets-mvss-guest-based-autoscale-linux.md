---
title: Usare la scalabilità automatica di Azure con metriche guest in un modello di set di scalabilità Linux | Microsoft Docs
description: Informazioni su come eseguire la scalabilità automatica usando metriche guest in un modello di set di scalabilità di macchine virtuali Linux
services: virtual-machine-scale-sets
documentationcenter: ''
author: gatneil
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: na
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: negat
ms.openlocfilehash: 8e822d83dd3bafabfea60ad50224c87df226bdc6
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/20/2017
ms.locfileid: "26781429"
---
# <a name="autoscale-using-guest-metrics-in-a-linux-scale-set-template"></a>Eseguire la scalabilità automatica usando metriche guest in un modello di set di scalabilità Linux

In Azure esistono due tipi di metriche raccolte da macchine virtuali e set di scalabilità: alcune provengono dalla macchina virtuale host e altre dalla macchina virtuale guest. In generale, se si usano CPU, disco e metriche di rete standard, le metriche host sono probabilmente ottimali. Se tuttavia è necessaria una scelta di metriche più ampia, le metriche guest rappresentano probabilmente una soluzione migliore. Ecco le differenze tra i due tipi di metriche:

Le metriche host sono più semplici e più affidabili. Non richiedono passaggi di configurazione aggiuntivi perché vengono raccolte dalla macchina virtuale host, mentre le metriche guest richiedono l'installazione dell'[estensione Diagnostica Windows Azure](../virtual-machines/windows/extensions-diagnostics-template.md) o dell'[estensione Diagnostica Linux Azure](../virtual-machines/linux/diagnostic-extension.md) nella macchina virtuale guest. È preferibile usare metriche guest anziché metriche host perché le metriche guest offrono una selezione più ampia di metriche. Le metriche relative al consumo di memoria, disponibili solo tramite metriche guest, ne sono un esempio. Le metriche host supportate sono elencate [qui](../monitoring-and-diagnostics/monitoring-supported-metrics.md), mentre le metriche guest usate comunemente sono elencate [qui](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md). Questo articolo illustra come modificare il [modello di set di scalabilità a validità minima](./virtual-machine-scale-sets-mvss-start.md) per usare le regole di scalabilità automatica in base alle metriche guest per i set di scalabilità Linux.

## <a name="change-the-template-definition"></a>Modificare la definizione del modello

Il modello di set di scalabilità a validità minima è disponibile [qui](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), mentre il modello per la distribuzione del set di scalabilità Linux con scalabilità automatica basata su guest è disponibile [qui](https://raw.githubusercontent.com/gatneil/mvss/guest-based-autoscale-linux/azuredeploy.json). Viene ora esaminato il diff usato per creare questo modello, `git diff minimum-viable-scale-set existing-vnet`, passo per passo:

Per prima cosa, aggiungere i parametri per `storageAccountName` e `storageAccountSasToken`. L'agente di diagnostica archivia i dati di metrica in una [tabella](../cosmos-db/table-storage-how-to-use-dotnet.md) in questo account di archiviazione. A partire dalla versione 3.0 dell'agente di diagnostica Linux, non è più supportato l'uso di una chiave di accesso di archiviazione. Usare invece un [token di firma di accesso condiviso](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

```diff
     },
     "adminPassword": {
       "type": "securestring"
+    },
+    "storageAccountName": {
+      "type": "string"
+    },
+    "storageAccountSasToken": {
+      "type": "securestring"
     }
   },
```

Successivamente, modificare il set di scalabilità `extensionProfile` in modo da includere l'estensione di diagnostica. In questa configurazione specificare l'ID della risorsa del set di scalabilità da cui vengono raccolte le metriche, oltre all'account di archiviazione e al token di firma di accesso condiviso da usare per archiviare le metriche. Specificare la frequenza con cui vengono aggregate le metriche, in questo caso, ogni minuto, e le metriche di cui tenere traccia, in questo caso, la percentuale di memoria usata. Per informazioni più dettagliate su questa configurazione e sulle metriche diverse dalla percentuale di memoria usata, vedere [questa documentazione](../virtual-machines/linux/diagnostic-extension.md).

```diff
                 }
               }
             ]
+          },
+          "extensionProfile": {
+            "extensions": [
+              {
+                "name": "LinuxDiagnosticExtension",
+                "properties": {
+                  "publisher": "Microsoft.Azure.Diagnostics",
+                  "type": "LinuxDiagnostic",
+                  "typeHandlerVersion": "3.0",
+                  "settings": {
+                    "StorageAccount": "[parameters('storageAccountName')]",
+                    "ladCfg": {
+                      "diagnosticMonitorConfiguration": {
+                        "performanceCounters": {
+                          "sinks": "WADMetricJsonBlob",
+                          "performanceCounterConfiguration": [
+                            {
+                              "unit": "percent",
+                              "type": "builtin",
+                              "class": "memory",
+                              "counter": "percentUsedMemory",
+                              "counterSpecifier": "/builtin/memory/percentUsedMemory",
+                              "condition": "IsAggregate=TRUE"
+                            }
+                          ]
+                        },
+                        "metrics": {
+                          "metricAggregation": [
+                            {
+                              "scheduledTransferPeriod": "PT1M"
+                            }
+                          ],
+                          "resourceId": "[resourceId('Microsoft.Compute/virtualMachineScaleSets', 'myScaleSet')]"
+                        }
+                      }
+                    }
+                  },
+                  "protectedSettings": {
+                    "storageAccountName": "[parameters('storageAccountName')]",
+                    "storageAccountSasToken": "[parameters('storageAccountSasToken')]",
+                    "sinksConfig": {
+                      "sink": [
+                        {
+                          "name": "WADMetricJsonBlob",
+                          "type": "JsonBlob"
+                        }
+                      ]
+                    }
+                  }
+                }
+              }
+            ]
           }
         }
       }
```

Aggiungere infine una risorsa `autoscaleSettings` per configurare la scalabilità automatica in base a queste metriche. Questa risorsa include una clausola `dependsOn` che fa riferimento al set di scalabilità per verificarne l'esistenza prima di tentare l'applicazione della scalabilità automatica. Se si sceglie una metrica diversa su cui basare la scalabilità automatica, si dovrebbe usare `counterSpecifier` della configurazione per l'estensione di diagnostica come `metricName` nella configurazione di scalabilità automatica. Per altre informazioni sulla configurazione della scalabilità automatica, vedere [Procedure consigliate per la scalabilità automatica](..//monitoring-and-diagnostics/insights-autoscale-best-practices.md) e la [documentazione di riferimento sulle API REST di monitoraggio di Azure](https://msdn.microsoft.com/library/azure/dn931928.aspx).

```diff
+    },
+    {
+      "type": "Microsoft.Insights/autoscaleSettings",
+      "apiVersion": "2015-04-01",
+      "name": "guestMetricsAutoscale",
+      "location": "[resourceGroup().location]",
+      "dependsOn": [
+        "Microsoft.Compute/virtualMachineScaleSets/myScaleSet"
+      ],
+      "properties": {
+        "name": "guestMetricsAutoscale",
+        "targetResourceUri": "[resourceId('Microsoft.Compute/virtualMachineScaleSets', 'myScaleSet')]",
+        "enabled": true,
+        "profiles": [
+          {
+            "name": "Profile1",
+            "capacity": {
+              "minimum": "1",
+              "maximum": "10",
+              "default": "3"
+            },
+            "rules": [
+              {
+                "metricTrigger": {
+                  "metricName": "/builtin/memory/percentUsedMemory",
+                  "metricNamespace": "",
+                  "metricResourceUri": "[resourceId('Microsoft.Compute/virtualMachineScaleSets', 'myScaleSet')]",
+                  "timeGrain": "PT1M",
+                  "statistic": "Average",
+                  "timeWindow": "PT5M",
+                  "timeAggregation": "Average",
+                  "operator": "GreaterThan",
+                  "threshold": 60
+                },
+                "scaleAction": {
+                  "direction": "Increase",
+                  "type": "ChangeCount",
+                  "value": "1",
+                  "cooldown": "PT1M"
+                }
+              },
+              {
+                "metricTrigger": {
+                  "metricName": "/builtin/memory/percentUsedMemory",
+                  "metricNamespace": "",
+                  "metricResourceUri": "[resourceId('Microsoft.Compute/virtualMachineScaleSets', 'myScaleSet')]",
+                  "timeGrain": "PT1M",
+                  "statistic": "Average",
+                  "timeWindow": "PT5M",
+                  "timeAggregation": "Average",
+                  "operator": "LessThan",
+                  "threshold": 30
+                },
+                "scaleAction": {
+                  "direction": "Decrease",
+                  "type": "ChangeCount",
+                  "value": "1",
+                  "cooldown": "PT1M"
+                }
+              }
+            ]
+          }
+        ]
+      }
     }
   ]
 }
```





## <a name="next-steps"></a>Passaggi successivi

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
