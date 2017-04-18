<properties
   pageTitle="Opis usługi Azure Active Directory Manifest aplikacji | Microsoft Azure"
   description="Szczegółowe zakresu oczywiste aplikacji usługi Azure Active Directory, która reprezentuje konfigurację tożsamości aplikacji w dzierżawie usługi Azure AD i ułatwia OAuth autoryzacji, obsługi zgody i innych elementów."
   services="active-directory"
   documentationCenter=""
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/11/2016"
   ms.author="dkershaw;bryanla"/>

# <a name="understanding-the-azure-active-directory-application-manifest"></a>Opis manifest aplikacji usługi Azure Active Directory

Aplikacje, które Integracja z Azure Active Directory (AD) musi być zarejestrowana w dzierżawie usługi Azure AD, dostarczając konfiguracji trwałych tożsamości dla aplikacji. W czasie wykonywania, umożliwiając scenariusze zezwolić aplikacji na zewnętrzny i brokera uwierzytelniania i autoryzacji za pośrednictwem Azure AD konsultacji tej konfiguracji. Aby uzyskać więcej informacji na temat model aplikacji Azure AD, zobacz [Dodawanie, aktualizowanie i usuwanie aplikacji] [ ADD-UPD-RMV-APP] artykuł.

## <a name="updating-an-applications-identity-configuration"></a>Aktualizowanie konfiguracji tożsamości aplikacji

Dostępne są faktyczną wiele opcji aktualizowania właściwości konfiguracji tożsamości aplikacji, które różnią się w funkcjach i stopnie trudności, łącznie z następujących czynności:

- ** [Azure klasyczny portalu] [ AZURE-CLASSIC-PORTAL] interfejs użytkownika sieci Web** umożliwia aktualizowanie najbardziej typowe właściwości aplikacji. Jest to błąd najszybszy i najmniej podatne sposób aktualizowania właściwości aplikacji, ale nie daje pełny dostęp do wszystkich właściwości, takie jak następnych dwóch metod.
- Dla bardziej zaawansowanych scenariuszy, gdy trzeba zaktualizować właściwości, które nie są dostępne w portalu klasyczny Azure możesz zmienić **pojawiają aplikacji**. To jest fokusu w tym artykule i jest szczegółowo uruchamiania w następnej sekcji.
- Możliwe jest również **napisać aplikację, która korzysta z [Interfejsu API wykresu] [ GRAPH-API] ** aktualizacji z aplikacji, która wymaga najbardziej wysiłku. Może to być atrakcyjną opcję, jeśli, czy piszesz oprogramowanie do zarządzania, aby zaktualizować właściwości aplikacji na bieżąco w zautomatyzowany sposób.

## <a name="using-the-application-manifest-to-update-an-applications-identity-configuration"></a>Aby zaktualizować konfiguracji tożsamości aplikacji przy użyciu manifest aplikacji
Za pomocą [portal Azure klasyczny][AZURE-CLASSIC-PORTAL], możesz zarządzać konfiguracji tożsamości aplikacji, pobierając i przekazywanie JSON pliku reprezentacja, nazywany manifest aplikacji. Nie rzeczywisty plik jest przechowywany w katalogu. Manifest aplikacji jest jedynie HTTP GET operacji na Jednostka aplikacji interfejsu API Azure AD wykresu, a przekazywanie jest operację poprawka HTTP Jednostka aplikacji.

W wyniku w celu zrozumienia, formatowanie i właściwości manifest aplikacji, będzie konieczne odwołanie interfejsu API wykres [Jednostka aplikacji] [ APPLICATION-ENTITY] dokumentacji. Przykłady aktualizacje, które mogą być wykonywane, ale aplikacji pojawiają przekazywania:

- **Zakresy uprawnień Declare (oauth2Permissions)** , udostępniane przez interfejsu API usługi sieci web. Zobacz temat "Uwidacznianie aplikacje sieci Web interfejsy API do innych" w [Aplikacjach integrację z usługą Azure Active Directory] [ INTEGRATING-APPLICATIONS-AAD] informacji na temat wykonywania personifikacji użytkownika przy użyciu oauth2Permissions delegowanymi zakres uprawnień. Jak już wspomniano, opisano właściwości obiektu aplikacji w interfejsie API wykresu [Jednostka i typ złożonych] [ APPLICATION-ENTITY] artykułu odwołania, w tym właściwość oauth2Permissions, która jest zbiorem typ [OAuth2Permission][APPLICATION-ENTITY-OAUTH2-PERMISSION].
- **Role aplikacji Declare (appRoles) udostępniane przez aplikację**. Właściwość appRoles Jednostka aplikacji jest to zbiór typ [AppRole][APPLICATION-ENTITY-APP-ROLE]. Zobacz [oparta na rolach kontrola dostępu w chmurze aplikacji przy użyciu Azure AD] [ RBAC-CLOUD-APPS-AZUREAD] artykułu, na przykład implementacji.
- **Declare znane klienta aplikacje (knownClientApplications)**, które umożliwiają logicznie wiążą zgody określonej aplikacji do zasobu i interfejsu API sieci web.
- **Żądanie Azure AD wydania grupy, którą rościć sobie członkostwa** zalogowania użytkownika (groupMembershipClaims).  Może to również skonfigurowane do wykonania roszczeń dotyczących członkostwa ról katalogu użytkownika. Zobacz [autoryzacji w aplikacje w chmurze przy użyciu grup AD] [ AAD-GROUPS-FOR-AUTHORIZATION] artykułu, na przykład implementacji.
- **Udzielanie Zezwalaj aplikacji do obsługi OAuth 2.0 niejawne** przepływów (oauth2AllowImplicitFlow). Ten typ przepływu udzielanie jest używana z osadzonego JavaScript stron sieci web lub pojedynczej strony aplikacji (SPA). Aby uzyskać więcej informacji o udzieleniu niejawne autoryzacji, zobacz [Opis uwierzytelnianie OAuth2 niejawne udzielić przepływ w usługi Azure Active Directory][IMPLICIT-GRANT].
- **Włącz stosowania X509 certyfikaty jako klucz tajny** (keyCredentials). Zobacz [Tworzenie aplikacji usługi i demon w usłudze Office 365] [ O365-SERVICE-DAEMON-APPS] i [Podręcznik dewelopera do uwierzytelniania z interfejsem API Menedżera zasobów Azure] [ DEV-GUIDE-TO-AUTH-WITH-ARM] artykułów Przykłady implementacji.
- **Dodaj nowy identyfikator URI aplikacji** dla aplikacji (identifierURIs[]). Aplikacja identyfikator URI są używane do jednoznacznie identyfikować aplikacji w jego dzierżawy Azure AD (lub w wielu dzierżawami Azure AD dla wielu dzierżawy scenariusze po kwalifikowanym za pośrednictwem zweryfikowanym domeny niestandardowej). Są one używane podczas żądania uprawnień do aplikacji zasobów lub podczas uzyskiwania token dostępu dla aplikacji zasobów. Gdy zaktualizujesz ten element, ta sama aktualizacja następuje do odpowiednich wystawcy usługi servicePrincipalNames [] kolekcji, która znajduje się w dzierżawie głównym aplikacji.

Manifest aplikacji zawiera również umożliwia śledzenie stanu rejestracji aplikacji. Ponieważ jest dostępne w formacie JSON, reprezentacja plik można sprawdzić, do pilota źródła wraz z kodu źródłowego aplikacji.

## <a name="step-by-step-example"></a>Krok według przykładu kroku
Umożliwia teraz wykonać kroki wymagane do zaktualizowania konfiguracji tożsamości aplikacji za pośrednictwem manifest aplikacji. Firma Microsoft wyróżni jedną z poprzednich przykładach przedstawiający, jak zadeklarować nowy zakres uprawnień w aplikacji zasobu:

1. Przejdź do [portalu klasyczny Azure] [ AZURE-CLASSIC-PORTAL] i zaloguj się przy użyciu konta, które ma uprawnienia administratora lub Współtworzenie administratora usługi.

2. Po możesz po uwierzytelnieniu, przewiń w dół i wybierz rozszerzenie Azure "Active Directory" w panelu nawigacji po lewej stronie (1), a następnie wybierz polecenie dzierżawy Azure AD, w której aplikacja jest zarejestrowany (2).

    ![Wybierz pozycję dzierżawy Azure AD][SELECT-AZURE-AD-TENANT]

3. Po stronie katalog pojawi się, kliknij pozycję "Aplikacje" (1) w górnej części strony, aby wyświetlić listę aplikacji zarejestrowane w dzierżawie. Następnie znajdź aplikację, którą chcesz zaktualizować na liście i wybierz polecenie (2).

    ![Wybierz pozycję dzierżawy Azure AD][SELECT-AZURE-AD-APP]

4. Teraz, gdy została zaznaczona pozycja strony głównej aplikacji, zwróć uwagę, funkcja "Zarządzanie pojawiają" na koniec strony (1). Po kliknięciu tego łącza pojawi pobieranie lub przekazywanie pliku manifestu JSON. Kliknij przycisk "Pobierz Manifest" (2). Będzie można bezpośrednio po przy użyciu okna dialogowego potwierdzenia pobierania prośbą o potwierdzenie, klikając pozycję "Pobierz pojawiają" (3), a następnie pozycję Otwórz lub Zapisz plik lokalnie (4).

    ![Zarządzanie manifest, opcja pobierania][MANAGE-MANIFEST-DOWNLOAD]

    ![Pobieranie manifestu][DOWNLOAD-MANIFEST]

5. W tym przykładzie firma Microsoft zapisany plik lokalnie, umożliwiając nam otwarcie w edytorze, wprowadzanie zmian w formacie JSON i ponownie Zapisz. Poniżej przedstawiono, jak wygląda struktury JSON w Edytorze Visual Studio JSON. Uwaga wynika schematu [Jednostka aplikacji] [ APPLICATION-ENTITY] jak wspomniano wcześniej:

    ![Aktualizowanie manifestu JSON][UPDATE-MANIFEST]

    Na przykład przy założeniu, że chcemy wdrożenie/Uwidacznianie nowych uprawnień o nazwie "Employees.Read.All" w naszym aplikacji zasobów (API), możesz po prostu dodać element nowe na sekundę do kolekcji oauth2Permissions ie:

        "oauth2Permissions": [
        {
        "adminConsentDescription": "Allow the application to access MyWebApplication on behalf of the signed-in user.",
        "adminConsentDisplayName": "Access MyWebApplication",
        "id": "aade5b35-ea3e-481c-b38d-cba4c78682a0",
        "isEnabled": true,
        "type": "User",
        "userConsentDescription": "Allow the application to access MyWebApplication on your behalf.",
        "userConsentDisplayName": "Access MyWebApplication",
        "value": "user_impersonation"
        },
        {
        "adminConsentDescription": "Allow the application to have read-only access to all Employee data.",
        "adminConsentDisplayName": "Read-only access to Employee records",
        "id": "2b351394-d7a7-4a84-841e-08a6a17e4cb8",
        "isEnabled": true,
        "type": "User",
        "userConsentDescription": "Allow the application to have read-only access to your Employee data.",
        "userConsentDisplayName": "Read-only access to your Employee records",
        "value": "Employees.Read.All"
        }
        ],

    Wpis musi być unikatowa i w związku z tym należy wygenerować nowy unikatowy identyfikator GUID dla `"id"` właściwości. W tym przypadku ponieważ możemy określony `"type": "User"`, to uprawnienie może być zgodę na przez dowolnego konta uwierzytelnione przez Azure AD dzierżawa rejestracji aplikacji zasobów i interfejsu API. Dzięki temu klient aplikacji uprawnienia do nich dostęp w imieniu konta. Opis i wyświetlania ciągi nazwy są używane podczas zgody i wyświetlania w portalu klasyczny Azure.

6. Po zakończeniu aktualizowania manifest wrócić do strony aplikacji Azure AD w portalu klasyczny Azure, a następnie kliknij przycisk "Zarządzanie pojawiają" funkcję ponownie (1). Ale tym razem wybierz opcję "Przekazywanie pojawiają" (2). Podobnie do pobierania, możesz będzie zostanie praca ponownie przy użyciu drugiego okna dialogowego, monituje o lokalizację pliku JSON. Kliknij przycisk "Przeglądaj w poszukiwaniu pliku..." (3), a następnie zaznacz plik JSON (4) za pomocą okna dialogowego "Wybierz plik do przekazywania" i kliknij przycisk "Otwórz". Gdy okno dialogowe zniknie, zaznacz pole wyboru "OK" (5) i manifeście zostaną przekazane.  

    ![Zarządzanie manifest, opcji przekazywania][MANAGE-MANIFEST-UPLOAD]

    ![Przekazywanie manifestu JSON][UPLOAD-MANIFEST]

    ![Przekazywanie manifestu JSON - potwierdzenia][UPLOAD-MANIFEST-CONFIRM]

Manifest zostanie zapisany, można nadać zarejestrowanego klienta aplikacji dostępu do nowego uprawnienia dodana powyżej. Tym razem zamiast edycji manifest aplikacji klienta można użyć Azure portalu klasyczny interfejs sieci Web:  

1. Najpierw przejdź do strony "Konfigurowanie" z aplikacją kliencką, do którego chcesz dodać dostęp do nowego interfejsu API usługi, a następnie kliknij przycisk "Dodaj aplikacji".
2. Następnie zostanie wyświetlona z listy zarejestrowanych zasobów aplikacji (API) w dzierżawie. Kliknij przycisk plus- + symbol obok nazwy aplikacji zasobów, aby go zaznaczyć.  
3. Następnie kliknij znacznik wyboru w prawym dolnym.
4. Po powrocie do sekcja "Dodawanie aplikacji" strony Konfiguracja usługi klienta, zostanie wyświetlona nowa aplikacja zasobów na liście. Po zatrzymaniu wskaźnika myszy nad sekcji "Delegowane uprawnienia" po prawej stronie tego wiersza, pojawi się znaleźć lista rozwijana są wyświetlane. Kliknij listę, a następnie wybierz nowy uprawnienia, aby dodać go do listy wymagane klienta uprawnień. Uwaga: to uprawnienie nowy będą przechowywane w konfiguracji tożsamości z aplikacją kliencką we właściwości zbioru "requiredResourceAccess".

![Uprawnienia do innych aplikacji][PERMS-TO-OTHER-APPS]

![Uprawnienia do innych aplikacji][PERMS-SELECT-APP]

![Uprawnienia do innych aplikacji][PERMS-SELECT-PERMS]

To wszystko. Teraz aplikacji jest wykonywane przy użyciu konfiguracją nowej tożsamości.

## <a name="next-steps"></a>Następne kroki
- Aby uzyskać więcej informacji na relacje między obiekty aplikacji i wystawcy usługi aplikacji, zobacz [aplikacji i usług obiektów kapitału w Azure AD][AAD-APP-OBJECTS].
- Zobacz [Słownik Deweloper Azure AD] [ AAD-DEVELOPER-GLOSSARY] dla definicje niektórych podstawowych koncepcji Deweloper Azure Active Directory (AD).

Użyj sekcji komentarze DISQUS poniżej, aby przesyłanie opinii i Pomóż nam Uściślanie i kształtowanie naszych zawartości.

<!--Image references-->
[DOWNLOAD-MANIFEST]: ./media/active-directory-application-manifest/download-manifest.png
[MANAGE-MANIFEST-DOWNLOAD]: ./media/active-directory-application-manifest/manage-manifest-download.png
[MANAGE-MANIFEST-UPLOAD]: ./media/active-directory-application-manifest/manage-manifest-upload.png
[PERMS-SELECT-APP]: ./media/active-directory-application-manifest/portal-perms-select-app.png
[PERMS-SELECT-PERMS]: ./media/active-directory-application-manifest/portal-perms-select-perms.png
[PERMS-TO-OTHER-APPS]: ./media/active-directory-application-manifest/portal-perms-to-other-apps.png
[SELECT-AZURE-AD-APP]: ./media/active-directory-application-manifest/select-azure-ad-application.png
[SELECT-AZURE-AD-TENANT]: ./media/active-directory-application-manifest/select-azure-ad-tenant.png
[UPDATE-MANIFEST]: ./media/active-directory-application-manifest/update-manifest.png
[UPLOAD-MANIFEST]: ./media/active-directory-application-manifest/upload-manifest.png
[UPLOAD-MANIFEST-CONFIRM]: ./media/active-directory-application-manifest/upload-manifest-confirm.png

<!--article references -->
[AAD-APP-OBJECTS]: active-directory-application-objects.md
[AAD-DEVELOPER-GLOSSARY]: active-directory-dev-glossary.md
[AAD-GROUPS-FOR-AUTHORIZATION]: http://www.dushyantgill.com/blog/2014/12/10/authorization-cloud-applications-using-ad-groups/
[ADD-UPD-RMV-APP]: active-directory-integrating-applications.md
[APPLICATION-ENTITY]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[APPLICATION-ENTITY-APP-ROLE]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type
[APPLICATION-ENTITY-OAUTH2-PERMISSION]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type
[AZURE-CLASSIC-PORTAL]: https://manage.windowsazure.com
[DEV-GUIDE-TO-AUTH-WITH-ARM]: http://www.dushyantgill.com/blog/2015/05/23/developers-guide-to-auth-with-azure-resource-manager-api/
[GRAPH-API]: active-directory-graph-api.md
[IMPLICIT-GRANT]: active-directory-dev-understanding-oauth2-implicit-grant.md
[INTEGRATING-APPLICATIONS-AAD]: https://azure.microsoft.com/documentation/articles/active-directory-integrating-applications/
[O365-PERM-DETAILS]: https://msdn.microsoft.com/office/office365/HowTo/application-manifest
[O365-SERVICE-DAEMON-APPS]: https://msdn.microsoft.com/office/office365/howto/building-service-apps-in-office-365
[RBAC-CLOUD-APPS-AZUREAD]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/

