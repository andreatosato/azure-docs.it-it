---
title: File di inclusione
description: File di inclusione
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 09/19/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 903074c78180ab2cd755abcf4207232f2851804e
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47019683"
---
[File di Azure](../articles/storage/files/storage-files-introduction.md) supporta l'autenticazione basata su identità tramite SMB (Server Message Block) (anteprima) per File di Azure con [Azure Active Directory (Azure AD) Domain Services.](../articles/active-directory-domain-services/active-directory-ds-overview.md) Le macchine virtuali di Windows aggiunte a un dominio possono così accedere a condivisioni file di Azure tramite credenziali di [Azure AD](../articles/active-directory/fundamentals/active-directory-whatis.md). 

Azure AD autentica un'identità come un utente, un gruppo o un'entità servizio con il [controllo degli accessi in base al ruolo](../articles/role-based-access-control/overview.md). È possibile definire ruoli personalizzati di controllo degli accessi in base al ruolo che comprendono set comuni di autorizzazioni usate per accedere a File di Azure. Quando si assegna il ruolo personalizzato di controllo degli accessi in base al ruolo a un'identità Azure AD, tale identità può accedere a una condivisione file di Azure in base a tali autorizzazioni.

Nell'ambito dell'anteprima, File di Azure supporta anche la conservazione, l'ereditarietà e l'applicazione di [DACL NTFS](https://technet.microsoft.com/library/2006.01.howitworksntfs.aspx) in tutti i file e in tutte le directory di una condivisione file. Se si copiano i dati da una condivisione file a File di Azure o viceversa, è possibile specificare che i DACL NTFS debbano essere mantenuti. In questo modo è possibile implementare scenari di backup usando File di Azure, preservando i DACL NTFS tra la condivisione file locale e la condivisione file nel cloud. 

> [!NOTE]
> L'autenticazione di Azure AD tramite SMB non è supportata per le macchine virtuali Linux nella versione di anteprima. Sono supportate solo le macchine virtuali Windows Server.
>
> L'autenticazione di Azure AD è disponibile solo per gli account di archiviazione creati dopo il 24 settembre 2018.
