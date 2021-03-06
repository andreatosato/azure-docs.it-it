---
title: Domande frequenti sulla creazione di report in Azure Active Directory | Documentazione Microsoft
description: Domande frequenti sulla creazione di report in Azure Active Directory.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
ms.assetid: 534da0b1-7858-4167-9986-7a62fbd10439
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: report-monitor
ms.date: 05/10/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: f1683321e23eff82e73dc9bb44941fc390633b8c
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/20/2018
ms.locfileid: "42143247"
---
# <a name="azure-active-directory-reporting-faq"></a>Domande frequenti sulla creazione di report in Azure Active Directory

Questo articolo include risposte alle domande frequenti sulla creazione di report in Azure Active Directory (Azure AD). Per altre informazioni, vedere [Creazione di report in Azure Active Directory](overview-reports.md). 

## <a name="getting-started"></a>Introduzione 

**D: se si usano le API endpoint https://graph.windows.net/&ltnome-tenant&gt;/reports/ per eseguire il pull a livello di codice dei report di controllo e di utilizzo delle applicazioni integrate di Azure AD, a quale API occorre passare?**

**R:** consultare la [documentazione di riferimento sulle API](https://developer.microsoft.com/graph/) per vedere come è possibile usare le nuove API per accedere ai [report attività](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started-azure-portal). Questo endpoint dispone di due report (Controllo e Accessi) che forniscono tutti i dati che si ottengono nell'endpoint delle API precedente. Anche questo nuovo endpoint include un report sugli accessi con la licenza Azure AD Premium, che è possibile usare per ottenere informazioni sull'utilizzo dell'app, l'utilizzo dei dispositivi e gli accessi degli utenti.

--- 

**D: se si usano le API endpoint https://graph.windows.net/&ltnome-tenant&gt;/reports/ per eseguire il pull a livello di codice dei report sulla sicurezza di Azure AD (tipi specifici di rilevamenti, come perdita di credenziali o accessi da indirizzi IP anonimi) nei sistemi di gestione dei report, a quale API occorre passare?**

**R:** è possibile usare l'[API per gli eventi di rischio di protezione dell'identità](../identity-protection/graph-get-started.md) per accedere ai rilevamenti relativi alla sicurezza tramite Microsoft Graph. Questo nuovo formato offre maggiore flessibilità per l'esecuzione di query sui dati, grazie a filtri avanzati, modalità di selezione dei campi e altro, oltre a standardizzare gli eventi di rischio su un solo tipo per una più facile integrazione con gli strumenti per la raccolta di informazioni di sicurezza e gestione degli eventi e altri dati. Poiché i dati sono un formato diverso, non è possibile sostituire le query precedenti con una nuova query. Tuttavia, [la nuova API usa Microsoft Graph](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/identityriskevent), ovvero lo standard Microsoft per API come Office 365 o Azure AD. Saranno quindi necessari interventi per estendere gli investimenti esistenti per Microsoft Graph o per avviare la transizione a questa nuova piattaforma standard.

--- 

**D: Come ottenere una licenza Premium?**

**R:** Per la risposta a questa domanda, vedere [Introduzione ad Azure Active Directory Premium](../fundamentals/active-directory-get-started-premium.md).

---

**D: In quanto tempo è possibile visualizzare le attività dopo aver acquistato una licenza Premium?**

**R:** Se si dispone già di dati sulle attività come licenza gratuita, è possibile visualizzare questi dati. Se non si dispone di dati, saranno necessari uno o due giorni.

---

**D: È possibile visualizzare i dati del mese precedente dopo avere acquistato una licenza Azure AD Premium?**

**R:**: Se di recente si è passati alla versione Premium (anche di valutazione), inizialmente è possibile visualizzare i dati fino a 7 giorni. Quando i dati si accumulano, verranno visualizzati fino a 30 giorni.

---

**D: è necessario essere amministratore globale per visualizzare gli accessi al portale di Azure o per ottenere i dati tramite l'API?**

**R:** No. È necessario avere il **ruolo con autorizzazioni di lettura per la sicurezza** oppure essere un **amministratore della protezione** o un **amministratore globale** per ottenere i dati dei report nel portale di Azure o tramite l'API.

---


## <a name="activity-logs"></a>Log attività


**D: Qual è la conservazione dei dati per i log attività (controllo e accessi) nel portale di Azure?** 

**R:** vedere [per quanto tempo rimangono archiviati i dati raccolti?](reference-reports-data-retention.md#q-for-how-long-is-the-collected-data-stored) per una risposta a questa domanda.

--- 

**D: Quanto tempo occorre per la visualizzazione dei dati sull'attività dopo il completamento della stessa?**

**R:** i log dell'attività di controllo hanno una latenza compresa tra 15 minuti e un'ora. Per la visualizzazione dei log delle attività di accesso possono essere necessari da 15 minuti fino a 2 ore per alcuni record.

---


**D: È possibile ottenere informazioni sui log attività di Office 365 tramite il portale di Azure?**

**R:** Sebbene l'attività di Office 365 e i log attività di Azure AD condividano numerose risorse della directory, se si desidera una visualizzazione completa dei log attività di Office 365, è necessario usare l'interfaccia di amministrazione di Office 365 per ottenere informazioni sui log attività di Office 365.

---


**D: Quali API è necessario usare per ottenere informazioni sui log attività di Office 365?**

**R:** Usare le API Gestione di Office 365 per accedere ai [log attività di Office 365 tramite un'API](https://msdn.microsoft.com/office-365/office-365-managment-apis-overview).

---

**D: Quanti record è possibile scaricare dal portale di Azure?**

**R:** Dal portale di Azure è possibile scaricare fino a 5.000 record. Per impostazione predefinita, vengono scaricati gli ultimi 5.000 record, ordinati partendo dai record *più recenti*.

---

## <a name="risky-sign-ins"></a>Accessi a rischio

**D: Esiste un evento di rischio nella protezione dell'identità, ma non vengono visualizzati accessi corrispondenti in tutti gli accessi. È normale?**

**R:** Sì, la protezione dell'identità valuta il rischio per tutti i flussi di autenticazione, sia interattivi che non interattivi. Tuttavia, tutti i report degli accessi mostrano solo gli accessi interattivi.

---

**D: Come è possibile scaricare il report "Utenti contrassegnati per il rischio" nel portale di Azure?**

**R:**: L'opzione per scaricare il report *Utenti contrassegnati per il rischio* verrà aggiunta a breve.

---

**D: Come sapere perché un accesso o un utente è stato contrassegnato come rischioso nel portale di Azure?**

**R:** I clienti dell'edizione Premium possono scoprire altre informazioni sugli eventi di rischio sottostanti facendo clic sull'utente in "Utenti contrassegnati per il rischio" o facendo clic su "Accessi a rischio". I clienti della versione Basic e Free riescono a visualizzare gli utenti e gli accessi a rischio senza le informazioni sull'evento di rischio sottostante.

---

**D: come vengono calcolati gli indirizzi IP nel report degli accessi e degli accessi a rischio?**

**R:** Gli indirizzi IP vengono rilasciati in modo che non esista una connessione certa tra un IP e la posizione in cui si trova fisicamente il computer con tale indirizzo. A ciò si aggiungono fattori come la possibilità che provider di telefonia mobile e VPN rilascino indirizzi IP da pool centrali spesso molto distanti dal luogo in cui viene effettivamente usato il dispositivo client. Per questi motivi, la conversione di un indirizzo IP in una posizione fisica è un'approssimazione basata su tracce, dati del Registro di sistema, ricerche inverse e altre informazioni. 

---

**D: Cosa significa l'evento di rischio "È stato rilevato un accesso con rischi aggiuntivi"?**

**R:** Per offrire informazioni approfondite su tutti gli accessi rischiosi effettuati nell'ambiente, "È stato rilevato un accesso con rischi aggiuntivi" funziona come segnaposto per gli accessi per rilevamenti esclusivi per i sottoscrittori di Azure AD Identity Protection.

---

**D: Cosa significa l'evento di rischio "È stato rilevato un accesso con rischi aggiuntivi"?**

**R:** Per offrire informazioni approfondite su tutti gli accessi rischiosi effettuati nell'ambiente, "È stato rilevato un accesso con rischi aggiuntivi" funziona come segnaposto per gli accessi per rilevamenti esclusivi per i sottoscrittori di Azure AD Identity Protection.

---

## <a name="conditional-access"></a>Accesso condizionale

**D: Quali sono le novità di questa funzionalità?**

**R:** I clienti possono ora risolvere i problemi relativi ai criteri di accesso condizionale tramite i report di tutti gli accessi. I clienti possono esaminare lo stato di accesso condizionale e approfondire i dettagli dei criteri applicati per l'accesso e il risultato per ogni criterio.

**D: Come iniziare?**

**R**: Per iniziare:
    * Andare al report degli accessi nel [portale di Azure](https://portal.azure.com). 
    * Fare clic sull'accesso di cui si desiderano risolvere i problemi.
    * Passare alla scheda **Accesso condizionale**. In questa scheda è possibile visualizzare tutti i criteri che hanno interessato l'accesso e il risultato per ogni criterio. 
    
**D: Quali sono i valori possibili per lo stato di accesso condizionale?**

**R:** Lo stato di accesso condizionale può avere i valori seguenti:
    * **Non applicato**: ciò significa che non è presente alcun criterio di accesso condizionale con l'utente e con l'app previsti. 
    * **Operazione riuscita**: ciò significa che è presente un criterio di accesso condizionale con l'utente e con l'app previsti e i criteri di accesso condizionale sono stati soddisfatti correttamente. 
    * **Operazione non riuscita**: ciò significa che è presente un criterio di accesso condizionale con l'utente e con l'app previsti e i criteri di accesso condizionale non sono stati soddisfatti correttamente. 
    
**D: Quali sono i valori possibili per il risultato dei criteri di accesso condizionale?**

**R:** Un criterio di accesso condizionale può avere i risultati seguenti:
    * **Operazione riuscita**: il criterio è stato soddisfatto correttamente.
    * **Operazione non riuscita**: il criterio non è stato soddisfatto.
    * **Non applicato**: le condizioni dei criteri potrebbero non essere soddisfatte.
    * **Non abilitato**: a causa di criteri nello stato disabilitato. 
    
**D: Il nome del criterio nel report di tutti gli accessi non corrisponde al nome del criterio nell'accesso condizionale. Perché?**

**R:** Il nome del criterio nel report di tutti gli accessi è basato sul nome del criterio di accesso condizionale al momento dell'accesso. Ciò può essere incoerente con il nome del criterio nell'accesso condizionale se è stato aggiornato il nome del criterio in un secondo momento, vale a dire, dopo l'accesso.