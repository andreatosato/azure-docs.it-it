---
title: Approvare o rifiutare le richieste per i ruoli delle risorse di Azure in PIM | Microsoft Docs
description: Informazioni su come approvare o rifiutare le richieste per i ruoli delle risorse di Azure in Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: pim
ms.date: 08/31/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: c661f2662f48c5aaece142cb4a2223ab8a6d0853
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/04/2018
ms.locfileid: "43666592"
---
# <a name="approve-or-deny-requests-for-azure-resource-roles-in-pim"></a>Approvare o rifiutare le richieste per i ruoli delle risorse di Azure in PIM

Con Azure AD Privileged Identity Management (PIM) è possibile configurare ruoli per richiedere l’approvazione per l’attivazione e scegliere uno o più utenti o gruppi come responsabili di approvazione con delega. Seguire i passaggi descritti in questo articolo per approvare o rifiutare le richieste per i ruoli delle risorse di Azure.

## <a name="view-pending-requests"></a>Visualizzazione delle richieste in sospeso

In qualità di responsabile approvazione con delega si riceverà una notifica di posta elettronica quando una richiesta è in attesa di approvazione. È possibile visualizzare queste richieste in sospeso in PIM.

1. Accedere al [portale di Azure](https://portal.azure.com/).

1. Aprire **Azure AD Privileged Identity Management**.

1. Fare clic su **Approva richieste**.

    ![Risorse di Azure - Approvare le richieste](./media/pim-resource-roles-approval-workflow/resources-approve-requests.png)

    Nella sezione **Requests for role activations** (Richieste di attivazioni di ruoli) verrà visualizzato un elenco di richieste in attesa di approvazione.

## <a name="approve-requests"></a>Approvare le richieste

1. Trovare e fare clic sulla richiesta che si intende approvare. Viene visualizzato un riquadro di approvazione.

    ![Riquadro Approva richieste](./media/pim-resource-roles-approval-workflow/resources-approve-pane.png)

1. Nel riquadro **Giustificazione** digitare un motivo.

1. Fare clic su **Approve** (Approva).

    Viene visualizzata una notifica con l'approvazione.

    ![Approvare la notifica](./media/pim-resource-roles-approval-workflow/resources-approve-notification.png)

## <a name="deny-requests"></a>Negare le richieste

1. Trovare e fare clic sulla richiesta che si intende negare. Viene visualizzato un riquadro di approvazione.

    ![Riquadro Approva richieste](./media/pim-resource-roles-approval-workflow/resources-approve-pane.png)

1. Nel riquadro **Giustificazione** digitare un motivo.

1. Fare clic su **Nega**.

    Viene visualizzata una notifica con la negazione.

## <a name="workflow-notifications"></a>Notifiche del flusso di lavoro

Ecco alcune informazioni sulle notifiche del flusso di lavoro:

- Tutti i membri dell'elenco di responsabili approvazione ricevono una notifica tramite posta elettronica quando una richiesta per un ruolo è in attesa di revisione. Le notifiche tramite posta elettronica includono un collegamento diretto alla richiesta, dove il responsabile approvazione può approvare o rifiutare.
- Le richieste vengono risolte dal primo membro dell'elenco che approva o rifiuta.
- Quando un responsabile approvazione risponde alla richiesta, tutti i membri dell'elenco di responsabili approvazione ricevono notifica dell'azione.
- Gli amministratori delle risorse ricevono notifica quando un membro approvato diventa attivo nel ruolo.

>[!Note]
>Un amministratore delle risorse che ritiene che un membro approvato non deve essere attivo può rimuovere l'assegnazione di ruolo attivo in PIM. Anche se gli amministratori delle risorse non ricevono notifica delle richieste in sospeso a meno che non siano membri dell'elenco di responsabili approvazione, essi possono visualizzare e annullare le richieste in sospeso di tutti gli utenti visualizzandole in PIM. 

## <a name="next-steps"></a>Passaggi successivi

- [Estendere o rinnovare i ruoli delle risorse di Azure in PIM](pim-resource-roles-renew-extend.md)
- [Notifiche tramite posta elettronica in PIM](pim-email-notifications.md)
- [Approvare o rifiutare le richieste per i ruoli della directory di Azure AD in PIM](azure-ad-pim-approval-workflow.md)
