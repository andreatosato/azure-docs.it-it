---
title: Compilare un'applicazione iOS che si integra con Azure AD per l'accesso e chiama le API protette di Azure AD usando OAuth 2.0 | Microsoft Docs
description: Informazioni sull'accesso utenti e chiamata dell'API Microsoft Graph da un'app iOS.
services: active-directory
documentationcenter: ios
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 42303177-9566-48ed-8abb-279fcf1e6ddb
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: quickstart
ms.date: 09/24/2018
ms.author: celested
ms.custom: aaddev
ms.reviewer: brandwe
ms.openlocfilehash: 89f2a4058006687fbe64ec64d98659e38f93f618
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46980577"
---
# <a name="quickstart-sign-in-users-and-call-the-microsoft-graph-api-from-an-ios-app"></a>Guida introduttiva: Accesso utenti e chiamata dell'API Microsoft Graph da un'app iOS

[!INCLUDE [active-directory-develop-applies-v1-adal](../../../includes/active-directory-develop-applies-v1-adal.md)]

Azure Active Directory (Azure AD) include Active Directory Authentication Library (ADAL) per i client iOS che devono accedere a risorse protette. ADAL semplifica il processo usato dall'applicazione per ottenere i token di accesso. 

In questa guida introduttiva si creerà un'applicazione Objective C to-Do List che:

* Ottiene i token di accesso per chiamare l'API Graph di Azure AD con il protocollo di autenticazione OAuth 2.0
* Cerca in una directory gli utenti con un determinato alias

Per compilare l'applicazione funzionante completa, sarà necessario:

1. Registrare l'applicazione con Azure AD.
1. Installare e configurare ADAL.
1. Usare ADAL per ottenere i token da Azure AD.

## <a name="prerequisites"></a>Prerequisiti

Per iniziare, completare questi prerequisiti:

* Scaricare la [struttura dell'app](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/skeleton.zip) oppure l'[esempio completato](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).
* Predisporre un tenant di Azure AD in cui poter creare gli utenti e registrare un'applicazione. Se non si ha già un tenant, vedere le [informazioni su come ottenerne uno](quickstart-create-new-tenant.md).

> [!TIP]
> Provare il nuovo [portale per sviluppatori](https://identity.microsoft.com/Docs/iOS) per imparare a usare Azure AD in pochi minuti. Il portale per sviluppatori guida l'utente nel processo di registrazione di un'app e di integrazione di Azure AD nel codice. Al termine si ottiene un'applicazione semplice in grado di autenticare gli utenti nel tenant e un back-end che può accettare i token ed eseguire la convalida.

## <a name="step-1-determine-what-your-redirect-uri-is-for-ios"></a>Passaggio 1: determinare qual è l'URI di reindirizzamento per iOS

Per avviare in modo sicuro le applicazioni in determinati scenari SSO, è necessario creare un *URI di reindirizzamento* in un formato particolare. Un URI di reindirizzamento viene usato per assicurarsi che i token vengano restituiti all'applicazione corretta che li aveva richiesti.

Il formato iOS per l'URI di reindirizzamento è il seguente:

```
<app-scheme>://<bundle-id>
```

* **app-scheme**: è registrato nel progetto XCode e rappresenta il modo in cui altre applicazioni possono chiamare l'utente. È possibile trovare l'app-scheme in **Info.plist -> Tipi di URL -> Identificatore URL**. È consigliabile crear un app-scheme se non ne sono stati configurati uno o più.
* **bundle-id**: è l'identificatore del bundle presente in **identity** nelle impostazioni del progetto in XCode.

Un esempio di questo codice di avvio rapido:

***msquickstart://com.microsoft.azureactivedirectory.samples.graph.QuickStart***

## <a name="step-2-register-the-directorysearcher-application"></a>Passaggio 2: Registrare l'applicazione DirectorySearcher

Per impostare l'app perché ottenga i token, è necessario registrarla nel tenant di Azure AD e concederle l'autorizzazione per accedere all'API Graph di Azure AD.

1. Accedere al [portale di Azure](https://portal.azure.com).
2. Nella barra in alto selezionare il proprio account. Nell'elenco **Directory** scegliere il tenant di Active Directory in cui si vuole registrare l'applicazione.
3. Selezionare **Tutti i servizi** nel riquadro di spostamento a sinistra e quindi selezionare **Azure Active Directory**.
4. Selezionare **Registrazioni per l'app**, quindi scegliere **Aggiungi**.
5. Seguire le istruzioni e creare una nuova applicazione client **nativa**.
    * **Nome** è il nome dell'applicazione e descrive l'applicazione agli utenti finali.
    * L'**URI di reindirizzamento** è una combinazione dello schema e della stringa che Azure AD usa per restituire le risposte dei token. Immettere un valore specifico per l'applicazione in base alle informazioni sull'URI di reindirizzamento sopra riportate.
6. Dopo avere completato la registrazione, Azure AD assegna automaticamente all'app un ID applicazione univoco. Poiché questo valore sarà necessario nelle sezioni successive, copiarlo dalla scheda dell'applicazione.
7. Dalla pagina **Impostazioni**, selezionare **Autorizzazioni necessarie > Aggiungi > Microsoft Graph**, quindi in **Autorizzazioni delegate** aggiungere l'autorizzazione **Leggi i dati della directory**. L'autorizzazione imposta l'applicazione in modo che possa eseguire query nell'API Graph di Azure AD per gli utenti.

## <a name="step-3-install-and-configure-adal"></a>Passaggio 3: Installare e configurare ADAL

Ora che si dispone di un'applicazione in Azure AD, è possibile installare ADAL e scrivere il codice relativo all'identità. Affinché ADAL possa comunicare con Azure AD, è necessario fornirle alcune informazioni sulla registrazione dell'app.

1. Si può iniziare aggiungendo la libreria ADAL al progetto DirectorySearcher usando CocaPods.

    ```
    $ vi Podfile
    ```
1. Aggiungere il codice seguente al podfile:

    ```
    source 'https://github.com/CocoaPods/Specs.git'
    link_with ['QuickStart']
    xcodeproj 'QuickStart'

    pod 'ADALiOS'
    ```

1. Caricare il podfile usando CocoaPods. Verrà creata una nuova area di lavoro XCode che sarà caricata.

    ```
    $ pod install
    ...
    $ open QuickStart.xcworkspace
    ```

1. Nel progetto QuickStart, aprire il file plist `settings.plist`.
1. Sostituire i valori degli elementi nella sezione in modo che usino gli stessi valori immessi nel portale di Azure. Il codice fa riferimento a questi valori ogni volta che usa ADAL.
    * `tenant` è il dominio del tenant di Azure AD, ad esempio contoso.onmicrosoft.com.
    * `clientId` è l'ID client dell'applicazione copiato dal portale.
    * `redirectUri` è l'URL di reindirizzamento registrato nel portale.

## <a name="step-4-use-adal-to-get-tokens-from-azure-ad"></a>Passaggio 4: Usare ADAL per ottenere token da Azure AD

Il principio alla base di ADAL è che l'app, ogni volta che ha bisogno di un token di accesso, deve solo chiamare un CompletionBlock `+(void) getToken : ` e ADAL fa il resto.

1. Nel progetto `QuickStart` aprire `GraphAPICaller.m` e trovare il commento `// TODO: getToken for generic Web API flows. Returns a token with no additional parameters provided.` nella parte superiore,

    dove si passano ad ADAL le coordinate attraverso un CompletionBlock per comunicare con Azure AD e gli si indica come memorizzare i token nella cache.

    ```ObjC
    +(void) getToken : (BOOL) clearCache
               parent:(UIViewController*) parent
    completionHandler:(void (^) (NSString*, NSError*))completionBlock;
    {
        AppData* data = [AppData getInstance];
        if(data.userItem){
            completionBlock(data.userItem.accessToken, nil);
            return;
        }

        ADAuthenticationError *error;
        authContext = [ADAuthenticationContext authenticationContextWithAuthority:data.authority error:&error];
        authContext.parentController = parent;
        NSURL *redirectUri = [[NSURL alloc]initWithString:data.redirectUriString];

        [ADAuthenticationSettings sharedInstance].enableFullScreen = YES;
        [authContext acquireTokenWithResource:data.resourceId
                                     clientId:data.clientId
                                  redirectUri:redirectUri
                               promptBehavior:AD_PROMPT_AUTO
                                       userId:data.userItem.userInformation.userId
                        extraQueryParameters: @"nux=1" // if this strikes you as strange it was legacy to display the correct mobile UX. You most likely won't need it in your code.
                             completionBlock:^(ADAuthenticationResult *result) {

                                  if (result.status != AD_SUCCEEDED)
                                  {
                                     completionBlock(nil, result.error);
                                  }
                                  else
                                  {
                                      data.userItem = result.tokenCacheStoreItem;
                                      completionBlock(result.tokenCacheStoreItem.accessToken, nil);
                                  }
                             }];
    }

    ```

2. È necessario usare questo token per cercare gli utenti nel grafico. Trovare il commento `// TODO: implement SearchUsersList`. Questo metodo invia una richiesta GET all'API Graph di Azure AD per eseguire una query sugli utenti il cui UPN inizia con il termine di ricerca specificato. 

    Per eseguire una query nell'API Graph di Azure AD, è necessario includere un oggetto access_token nell'intestazione `Authorization` della richiesta. È qui che entra in gioco ADAL.

    ```ObjC
    +(void) searchUserList:(NSString*)searchString
                    parent:(UIViewController*) parent
          completionBlock:(void (^) (NSMutableArray* Users, NSError* error)) completionBlock
    {
        if (!loadedApplicationSettings)
       {
            [self readApplicationSettings];
        }
        
        AppData* data = [AppData getInstance];

        NSString *graphURL = [NSString stringWithFormat:@"%@%@/users?api-version=%@&$filter=startswith(userPrincipalName, '%@')", data.taskWebApiUrlString, data.tenant, data.apiversion, searchString];

        [self craftRequest:[self.class trimString:graphURL]
                    parent:parent
         completionHandler:^(NSMutableURLRequest *request, NSError *error) {

             if (error != nil)
             {
                 completionBlock(nil, error);
             }
             else
             {

                 NSOperationQueue *queue = [[NSOperationQueue alloc]init];

                 [NSURLConnection sendAsynchronousRequest:request queue:queue completionHandler:^(NSURLResponse *response, NSData *data, NSError *error) {

                     if (error == nil && data != nil){

                         NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:data options:0 error:nil];

                         // We can grab the JSON node at the top to get our graph data.
                         NSArray *graphDataArray = [dataReturned objectForKey:@"value"];

                         // Don't be thrown off by the key name being "value". It really is the name of the
                         // first node. :-)

                         // Each object is a key value pair
                         NSDictionary *keyValuePairs;
                         NSMutableArray* Users = [[NSMutableArray alloc]init];

                         for(int i =0; i < graphDataArray.count; i++)
                         {
                             keyValuePairs = [graphDataArray objectAtIndex:i];

                             User *s = [[User alloc]init];
                             s.upn = [keyValuePairs valueForKey:@"userPrincipalName"];
                             s.name =[keyValuePairs valueForKey:@"givenName"];

                             [Users addObject:s];
                         }

                         completionBlock(Users, nil);
                     }
                     else
                     {
                         completionBlock(nil, error);
                     }

                }];
             }
         }];

    }

    ```

3. Quando l'app richiede un token mediante la chiamata a `getToken(...)`, ADAL tenta di restituire un token senza chiedere le credenziali all'utente. Se ADAL determina che l'utente deve eseguire l'accesso per ottenere un token, visualizza una finestra di dialogo di accesso, raccoglie le credenziali dell'utente e restituisce un token al termine dell'autenticazione. Se ADAL non può restituire un token per qualsiasi motivo, genera una `AdalException`.

> [!NOTE]
> L'oggetto `AuthenticationResult` contiene un oggetto `tokenCacheStoreItem` che può essere usato per raccogliere informazioni che potrebbero essere richieste dall'app. Nel progetto QuickStart viene usato `tokenCacheStoreItem` per determinare se l'autenticazione è già stata eseguita.

## <a name="step-5-build-and-run-the-application"></a>Passaggio 5: Compilare ed eseguire l'applicazione

Congratulazioni! Si dispone ora di un'applicazione iOS funzionante in grado di autenticare gli utenti, chiamare in modo sicuro le API Web usando OAuth 2.0 e ottenere informazioni di base sull'utente.

Se non si è ancora popolato il tenant con alcuni utenti, ora è possibile farlo.

1. Avviare l'applicazione QuickStart e accedere come uno di questi utenti.
1. Cercare altri utenti in base al relativo UPN.
1. Chiudere l'app e avviarla nuovamente. Si noti che la sessione dell'utente non è stata modificata.

ADAL consente di incorporare facilmente nell'applicazione tutte queste funzionalità comuni relative alle identità. Esegue automaticamente le attività più complesse, ovvero gestione della cache, supporto del protocollo OAuth, visualizzazione all'utente di un'interfaccia utente di accesso e aggiornamento dei token scaduti. Tutto ciò che occorre conoscere è una sola chiamata all'API, `getToken`.

L'esempio completato (senza i valori di configurazione) è disponibile in [GitHub](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip) per riferimento.

## <a name="next-steps"></a>Passaggi successivi

Ora è possibile passare ad altri scenari. È consigliabile provare quanto segue:

* [Proteggere un'API Web Node.js con Azure AD](quickstart-v1-nodejs-webapi.md)
* Ottenere informazioni su [Come abilitare l'accesso Single Sign-On tra app in iOS usando ADAL](howto-v1-enable-sso-ios.md)  
