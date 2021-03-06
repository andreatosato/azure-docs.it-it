---
title: Comportamento di un dispositivo simulato nella soluzione di simulazione dei dispositivi - Azure | Microsoft Docs
description: Questo articolo descrive come usare JavaScript per definire il comportamento di un dispositivo simulato nella soluzione di simulazione dei dispositivi.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 01/29/2018
ms.topic: conceptual
ms.openlocfilehash: 43edbc653ddbd55aab5e722071de1f2cf4bcd1c4
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/31/2018
ms.locfileid: "43344517"
---
# <a name="implement-the-device-model-behavior"></a>Implementare il comportamento del modello di dispositivo

L'articolo [Understand the device model schema](iot-accelerators-device-simulation-device-schema.md) (Conoscere lo schema del modello di dispositivo) descrive lo schema che definisce un modello di dispositivo simulato. L'articolo fa riferimento a due tipi di file JavaScript che implementano il comportamento di un dispositivo simulato:

- File JavaScript di tipo **State** che vengono eseguiti a intervalli prefissati per aggiornare lo stato interno del dispositivo.
- File JavaScript di tipo **Method** che vengono eseguiti quando la soluzione richiama un metodo nel dispositivo.

In questo articolo viene spiegato come:

>[!div class="checklist"]
> * Controllare lo stato di un dispositivo simulato.
> * Definire come deve rispondere un dispositivo simulato a una chiamata al metodo dall'hub IoT a cui è connesso
> * Eseguire il debug degli script.

[!INCLUDE [iot-accelerators-device-schema](../../includes/iot-accelerators-device-schema.md)]

## <a name="next-steps"></a>Passaggi successivi

Questo articolo ha spiegato come definire il comportamento di un modello di dispositivo simulato personalizzato. Sono state illustrate le procedure per eseguire queste operazioni:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Controllare lo stato di un dispositivo simulato.
> * Definire come deve rispondere un dispositivo simulato a una chiamata al metodo dall'hub IoT a cui è connesso
> * Eseguire il debug degli script.

Ora che si è appreso come specificare il comportamento di un dispositivo simulato, il passaggio successivo consigliato consente di apprendere come [creare un dispositivo simulato](iot-accelerators-device-simulation-create-simulated-device.md).

Per altre informazioni per sviluppatori sulla soluzione di simulazione dei dispositivi, vedere la [guida di riferimento per sviluppatori](https://github.com/Azure/device-simulation-dotnet/wiki/Simulation-Service-Developer-Reference-Guide).
