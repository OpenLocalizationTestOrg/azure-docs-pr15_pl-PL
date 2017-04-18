<properties
    pageTitle="Połączenie niestandardowe interfejsu API w aplikacjach warunków logicznych"
    description="Za pomocą swojego niestandardowego interfejsu API hostowana w aplikacji usługi z aplikacjami warunków logicznych"
    authors="stepsic-microsoft-com"
    manager="dwrede"
    editor=""
    services="logic-apps"
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/31/2016"
    ms.author="stepsic"/>

# <a name="using-your-custom-api-hosted-on-app-service-with-logic-apps"></a>Za pomocą swojego niestandardowego interfejsu API hostowana w aplikacji usługi z aplikacjami warunków logicznych

Mimo że aplikacje logika ma bogatego zestawu łączników 40 + dla różnych usług, warto nawiązywanie połączeń ze spotkaniami własne niestandardowe interfejs API, który może zostać uruchomiony własnego kodu. Najłatwiejszym i najbardziej skalowalna sposoby udostępniać własne niestandardowe web interfejsu API jest za pomocą aplikacji usługi. W tym artykule opisano, jak połączenie do dowolnej interfejsu API obsługiwane w aplikacji, w przeglądarce lub aplikacji dla urządzeń przenośnych interfejs API usługi aplikacji sieci web.

Dla informacji na temat tworzenia interfejsów API jako wyzwalacza lub akcji w logiczny aplikacje zapoznaj się z [tego artykułu](app-service-logic-create-api-app.md).

## <a name="deploy-your-web-app"></a>Wdrażanie aplikacji sieci Web

Najpierw musisz wdrażanie usługi interfejsu API jako aplikacji sieci Web w aplikacji usługi. Podstawy wdrażania obejmuje instrukcje tutaj: [Tworzenie aplikacji sieci web programu ASP.NET](../app-service-web/web-sites-dotnet-get-started.md).  Gdy możesz nawiązać połączenie do wszystkich API aplikacji logika, aby uzyskać najlepsze wyniki zalecamy Swagger metadane, aby ułatwić integrację z akcjami aplikacji logika.  Szczegóły można znaleźć na [dodanie swagger](../app-service-api/app-service-api-dotnet-get-started.md#use-swagger-api-metadata-and-ui).

### <a name="api-settings"></a>Ustawienia interfejsu API

Aby projektanta aplikacji logika, aby przeanalizować usługi Swagger należy włączyć CORS i ustawić właściwości APIDefinition aplikacji sieci web.  To jest bardzo łatwo można ustawić w Azure Portal.  Po prostu otwórz karta Ustawienia aplikacji sieci Web, w sekcji interfejsu API Ustawianie "Interfejsu API definicji" do adresu URL pliku swagger.json (zazwyczaj jest to https://{name}.azurewebsites.net/swagger/docs/v1) i dodać zasadę CORS dla "*" umożliwia żądania z aplikacji logika projektanta.

## <a name="calling-into-the-api"></a>Dzwoniąc do API

W ramach portalu aplikacje logiczny, jeśli ustawiono CORS i właściwości definicji interfejsu API powinno być możliwe do łatwo dodawać akcje niestandardowe interfejsu API w ramach usługi przepływu.  W Projektancie umożliwia przeglądanie serwisach subskrypcji, aby wyświetlić listę witryn sieci Web przy użyciu adresu URL swagger zdefiniowane.  Można również użyć HTTP + Swagger akcji do wskazywania swagger i listy dostępnych akcji i danych wejściowych.  Na koniec możesz zawsze utworzyć żądanie przy użyciu akcji HTTP połączenie wszystkich API, nawet tych, które nie mają ani nie uwidaczniają dokumentu swagger.

Jeśli chcesz zabezpieczyć z interfejsu API, następnie istnieje kilka sposobów, w tym:

1. Zmiany nie są kod wymagane - usługi Azure Active Directory może służyć do ochrony usługi interfejsu API bez konieczności zmiany kodu lub ponownego rozmieszczania.
1. Wymusić uwierzytelnianie podstawowe, AAD Auth lub certyfikatu uwierzytelniania w kodzie z interfejsu API.

## <a name="securing-calls-to-your-api-without-a-code-change"></a>Zabezpieczanie połączeń z interfejsu API bez zmiany kodu

W tej sekcji utworzysz dwa aplikacji usługi Azure Active Directory — jedną dla aplikacji logiki i jedną dla aplikacji sieci Web.  Będzie uwierzytelnienia połączenia do aplikacji sieci Web przy użyciu wystawcy usługi (identyfikator klienta i hasła) skojarzone z aplikacją AAD dla aplikacji logicznych. Na koniec będzie zawierać aplikacji identyfikatorami w swojej definicji aplikacji logicznych.

### <a name="part-1-setting-up-an-application-identity-for-your-logic-app"></a>Część 1: Konfigurowanie tożsamości aplikacji dla aplikacji warunków logicznych

To jest aplikacja Logika używa do uwierzytelniania usługi active directory. Tylko *potrzebne* w tym celu raz dla katalogu. Na przykład można używać tej samej tożsamości dla wszystkich aplikacji logiczny, ale możesz również utworzyć unikatowych tożsamości na logiki aplikacji w razie potrzeby. Można to zrobić w interfejsie użytkownika lub za pomocą programu PowerShell.

#### <a name="create-the-application-identity-using-the-azure-classic-portal"></a>Tworzenie tożsamości aplikacji przy użyciu portalu klasyczny Azure

1. Przejdź do [usługi Active directory w portalu klasyczny Azure](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory) i wybierz pozycję katalogu, którego używasz dla aplikacji sieci Web
2. Kliknij kartę **aplikacje**
3. Kliknij przycisk **Dodaj** na pasku poleceń u dołu strony
4. Nadaj nazwę, kliknij strzałkę następnego Twojej tożsamości
5. Umieszczenie w ciągu unikatowe sformatowane jako domenę w dwóch polach, a następnie kliknij znacznik wyboru
6. Kliknij kartę **Konfiguruj** dla tej aplikacji
7. Skopiuj **Identyfikator klienta**, będzie on używany w aplikacji logika
8. W sekcji **klawisze** kliknij pozycję **Wybierz czas trwania** i wybierz rok 1 lub 2 lat
9. Kliknij przycisk **Zapisz** u dołu ekranu (może być konieczne zaczekanie kilka sekund)
10. Teraz należy skopiować klucz w polu. Będzie on również używany w aplikacji warunków logicznych

#### <a name="create-the-application-identity-using-powershell"></a>Tworzenie tożsamości aplikacji przy użyciu programu PowerShell
1. `Switch-AzureMode AzureResourceManager`
2. `Add-AzureAccount`
3. `New-AzureADApplication -DisplayName "MyLogicAppID" -HomePage "http://someranddomain.tld" -IdentifierUris "http://someranddomain.tld" -Password "Pass@word1!"`
4. Pamiętaj skopiować **Identyfikator dzierżawy**, **Identyfikator aplikacji** i hasło, którego użyto

### <a name="part-2-protect-your-web-app-with-an-aad-app-identity"></a>Część 2: Ochrona aplikacji sieci Web przy użyciu tożsamości aplikacji AAD

Jeśli została już rozmieszczona aplikacji sieci Web możesz po prostu włączyć ją w portalu. W przeciwnym razie umożliwia włączenie autoryzacji częścią wdrożenia Menedżera zasobów Azure.

#### <a name="enable-authorization-in-the-azure-portal"></a>Włączanie autoryzacji w portalu Azure

1. Przejdź do aplikacji sieci Web i kliknij przycisk **Ustawienia** na pasku poleceń.
2. Kliknij pozycję **Autoryzacji i uwierzytelniania**.
3. **Włącz go.**

Na tym etapie można automatycznie jest tworzona aplikacja. Gdy jest potrzebny identyfikator klienta tej aplikacji dla część 3, musisz:

1. Przejdź do [usługi Active directory w portalu klasyczny Azure](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory) i wybierz katalogu.
2. Wyszukiwanie aplikacji w polu wyszukiwania
3. Polecenie na liście
4. Kliknij kartę **Konfigurowanie**
5. Powinien zostać wyświetlony **Identyfikator klienta**

#### <a name="deploying-your-web-app-using-an-arm-template"></a>Wdrażanie aplikacji sieci Web przy użyciu szablonu ARM

Najpierw należy utworzyć aplikację dla aplikacji sieci Web. To powinny różnić się od aplikacji, która jest używana dla aplikacji logicznych. Zacznij od powyżej kroki opisane w części 1, ale teraz dla **strony głównej** i **IdentifierUris** stosowania https:// rzeczywisty**adres URL** aplikacji sieci Web.

>[AZURE.NOTE]Podczas tworzenia aplikacji dla aplikacji sieci Web, należy użyć [Azure klasyczny podejście portalu](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory), jak polecenia programu PowerShell nie konfiguruje wymagane uprawnienia, aby zalogować się użytkowników do witryny sieci Web.

Masz jeden raz klienta, ID i ID dzierżawy, obejmują jako zasoby sub aplikacji sieci Web w szablonie wdrażania:

```
"resources" : [
    {
        "apiVersion" : "2015-08-01",
        "name" : "web",
        "type" : "config",
        "dependsOn" : [
            "[concat('Microsoft.Web/sites/','parameters('webAppName'))]"
        ],
        "properties" : {
            "siteAuthEnabled": true,
            "siteAuthSettings": {
              "clientId": "<<clientID>>",
              "issuer": "https://sts.windows.net/<<tenantID>>/",
            }
        }
    }
]
```

Aby uruchomić instalacji automatycznie wdraża pustej aplikacji sieci Web i aplikacji logika razem, którego AAD, przycisk:

[![Wdrażanie Azure](./media/app-service-logic-custom-hosted-api/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-logic-app-custom-api%2Fazuredeploy.json)

Zakończenie szablonu zobacz [Logika aplikacja wywołuje interfejs API niestandardowych hostowanych w aplikacji usługi i chronione AAD](https://github.com/Azure/azure-quickstart-templates/blob/master/201-logic-app-custom-api/azuredeploy.json).


### <a name="part-3-populate-the-authorization-section-in-the-logic-app"></a>Część 3: Wypełnianie sekcji autoryzacji w aplikacji warunków logicznych

W sekcji **autoryzacji** akcji **HTTP** :`{"tenant":"<<tenantId>>", "audience":"<<clientID from Part 2>>", "clientId":"<<clientID from Part 1>>","secret": "<<Password or Key from Part 1>>","type":"ActiveDirectoryOAuth" }`

| Element | Opis |
|---------|-------------|
| Typ | Typ uwierzytelniania. W przypadku uwierzytelniania ActiveDirectoryOAuth wartość jest ActiveDirectoryOAuth. |
| dzierżawy | Identyfikator dzierżawy, używane do identyfikowania dzierżawy AD. |
| grupy odbiorców | Wymagane. Zasób, do którego łączysz się. |
| clientID | Identyfikator klienta na podstawie Azure AD. |
| tajny | Wymagane. Tajny klienta, który żąda tokenu. |

To ustawienie ma już powyższego szablonu, ale jeśli bezpośrednio tworzenia aplikacji logiczny, musisz umieścić sekcję pełny autoryzacji.

## <a name="securing-your-api-in-code"></a>Zabezpieczanie usługi interfejsu API w kodzie

### <a name="certificate-auth"></a>Certyfikat uwierzytelniania

Certyfikaty klientów służy do sprawdzania poprawności przychodzących wezwań do aplikacji sieci Web. Zobacz [Jak do skonfigurowania TLS wzajemnego uwierzytelniania dla aplikacji sieci Web](../app-service-web/app-service-web-configure-tls-mutual-auth.md) dla konfigurowania kodu.

W sekcji *autoryzacji* powinien zawierać: `{"type": "clientcertificate","password": "test","pfx": "long-pfx-key"}`.

| Element | Opis |
|---------|-------------|
| Typ | Wymagane. Typ uwierzytelniania. Certyfikatów SSL klienta wartość musi być ClientCertificate. |
| PFX | Wymagane. Zakodowany Base64 zawartość pliku PFX. |
| hasło | Wymagane. Hasło, aby uzyskać dostęp do pliku PFX. |

### <a name="basic-auth"></a>Uwierzytelnianie podstawowe

Uwierzytelnianie podstawowe (na przykład nazwę użytkownika i hasło) służy do sprawdzania poprawności przychodzących żądań. Uwierzytelnianie podstawowe jest wspólnego wzorca i możliwości w innym języku, który tworzenie aplikacji w.

W sekcji *autoryzacji* powinien zawierać: `{"type": "basic","username": "test","password": "test"}`.

| Element | Opis |
|---------|-------------|
| Typ | Wymagane. Typ uwierzytelniania. W przypadku uwierzytelniania podstawowego wartość musi być Basic. |
| Nazwa użytkownika | Wymagane. Nazwa użytkownika do uwierzytelnienia. |
| hasło | Wymagane. Hasło do uwierzytelniania. |

### <a name="handle-aad-auth-in-code"></a>Uchwyt AAD auth w kodzie

Domyślnie uwierzytelniania usługi Azure Active Directory, która umożliwi w portalu nie szerokiego autoryzacji. Na przykład nie blokuje z interfejsu API do określonego użytkownika lub aplikacji, ale tylko do określonych dzierżawy.

Jeśli chcesz ograniczyć API do właśnie logiki aplikacji, na przykład w kodzie, można wyodrębnić nagłówka, który zawiera JWT i sprawdź, kto rozmówcy odrzucanie żądań, które nie są zgodne.

Przejść dalej, jeśli chcesz ją wdrożyć wyłącznie w własnego kodu, a nie korzystać z funkcji portalu można przeczytać ten artykuł: [Używanie usługi Active Directory do uwierzytelniania w usłudze Azure aplikacji](../app-service-web/web-sites-authentication-authorization.md).

Nadal potrzebujesz wykonaj powyższe kroki w celu utworzenia tożsamości aplikacji dla aplikacji logiki i użyć jej połączenie z interfejsu API.
