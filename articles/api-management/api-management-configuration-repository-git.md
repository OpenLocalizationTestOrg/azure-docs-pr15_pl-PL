<properties 
    pageTitle="Zapisywanie i konfigurowaniu konfiguracji usługi zarządzania interfejsu API przy użyciu cyfra" 
    description="Dowiedz się, jak zapisać i konfigurowanie konfiguracji usługi zarządzania interfejsu API przy użyciu cyfra." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>


# <a name="how-to-save-and-configure-your-api-management-service-configuration-using-git"></a>Zapisywanie i konfigurowaniu konfiguracji usługi zarządzania interfejsu API przy użyciu cyfra

>[AZURE.IMPORTANT] Konfiguracja cyfra zarządzania interfejsu API jest obecnie w podglądzie. Funkcjonalny zakończeniu, ale jest na podglądzie, ponieważ firma Microsoft aktywnie świadczonej opinii na temat tej funkcji. Istnieje możliwość, że firma Microsoft może poprawić podziału ulec zmianie w odpowiedzi na opinie klientów, dlatego zalecamy nie w zależności od funkcji do użytku w środowisku produkcyjnym. Jeśli masz pytania lub opinii, napisz nam na `apimgmt@microsoft.com`.

Każde wystąpienie usługi zarządzania interfejsu API obsługuje bazy danych konfiguracji, która zawiera informacje o konfiguracji i metadanych dla wystąpienia usługi. Zmiany mogą zostać wprowadzone w wystąpieniu usług, zmieniając ustawienia w portalu programu publisher, przy użyciu polecenia cmdlet programu PowerShell lub wywołania interfejsu API usługi REST. Oprócz tych metod możesz zarządzać konfiguracji wystąpienia usługi przy użyciu cyfra, takich jak włączanie usług zarządzania scenariusze:

-   Przechowywanie wersji konfiguracji — pobieranie i przechowywanie różnych wersji konfiguracji usługi
-   Zbiorcze zmian w konfiguracji — wprowadzanie zmian w wielu części konfiguracji usługi w lokalnym repozytorium i integracja zmiany na serwerze z jednej operacji
-   Znane toolchain cyfra i przepływu pracy — korzystanie z narzędzia cyfra i przepływów pracy, które znasz już

Na poniższym diagramie przedstawiono przegląd sposobów konfigurowania wystąpienia usługi zarządzania interfejsu API.

![Konfigurowanie cyfra][api-management-git-configure]

Po wprowadzeniu zmian w usłudze przy użyciu portalu programu publisher, poleceń cmdlet środowiska PowerShell lub interfejsu API usługi REST zarządzasz z usług konfiguracji bazy danych za pomocą `https://{name}.management.azure-api.net` punktu końcowego, jak pokazano po prawej stronie diagramu. Po lewej stronie diagramu przedstawiono, jak można zarządzać konfiguracji usługi przy użyciu cyfra i repozytorium cyfra tej usługi znajdujące się w `https://{name}.scm.azure-api.net`.

Poniższe kroki zawierają omówienie zarządzania wystąpienia usługi zarządzania interfejsu API przy użyciu cyfra.

1.  Włączanie dostępu cyfra w usłudze
2.  Zapisywanie bazy danych konfiguracji usługi do repozytorium cyfra
3.  Klonowanie repo cyfra na komputerze lokalnym
4.  Pobrana najnowszą repo w dół do komputera lokalnego i przekazywania i wypychane zmian z powrotem do programu repo
5.  Wdrażanie zmiany wprowadzone przez użytkownika repo do bazy danych konfiguracji usługi

W tym artykule opisano, jak włączyć i zarządzanie konfiguracji usługi przy użyciu cyfra i znajdują się informacje dotyczące plików i folderów w repozytorium cyfra.

## <a name="to-enable-git-access"></a>Aby umożliwić dostęp cyfra

Możesz szybko wyświetlić stan konfiguracji cyfra, wyświetlając ikonę cyfra w prawym górnym rogu programu publisher. W tym przykładzie nie został jeszcze włączony dostęp cyfra.

![Stan cyfra][api-management-git-icon-enable]

Aby wyświetlić i skonfigurować ustawienia konfiguracji cyfra, możesz można albo kliknij ikonę cyfra lub kliknij menu **zabezpieczeń** i przejdź do karty **repozytorium konfiguracji** .

![Włączanie CYFRA][api-management-enable-git]

Aby włączyć dostęp cyfra, zaznacz pole wyboru **Włącz cyfra dostępu** .

Po chwili zmiany są zapisywane i zostanie wyświetlony komunikat z potwierdzeniem. Należy zauważyć, że ikona cyfra zmienił się kolor, aby wskazać, że jest włączony dostęp cyfra i komunikat o stanie teraz oznacza, że istnieją niezapisane zmiany do repozytorium. Jest to spowodowane bazy danych konfiguracji usługi zarządzania interfejsu API nie został jeszcze zapisany do repozytorium.

![Cyfra włączone][api-management-git-enabled]

>[AZURE.IMPORTANT] Wszelkie hasła, które nie zdefiniowano właściwości będą przechowywane w repozytorium i pozostanie w jego historii dopiero po wyłączyć i ponownie włączyć dostęp cyfra. Właściwości zapewniają bezpiecznym miejscu do zarządzania stałe wartości ciągów, w tym hasła, we wszystkich konfiguracji interfejsu API i zasady, dzięki czemu nie trzeba przechowywać je bezpośrednio w instrukcji z zasad. Aby uzyskać więcej informacji zobacz [jak za pomocą właściwości w zasady zarządzania interfejsu API Azure](api-management-howto-properties.md).

Aby uzyskać informacji na temat włączania lub wyłączania dostępu cyfra przy użyciu interfejsu API usługi REST zobacz [Włączanie lub wyłączanie cyfra dostępu za pomocą interfejsu API usługi REST](https://msdn.microsoft.com/library/dn781420.aspx#EnableGit).

## <a name="to-save-the-service-configuration-to-the-git-repository"></a>Aby zapisać konfigurację usługi repozytorium cyfra

Pierwszym krokiem przed klonowanie repozytorium jest zapisywanie bieżącego stanu konfiguracji usługi do repozytorium. Kliknij przycisk **Zapisz konfiguracji do repozytorium**.

![Zapisywanie konfiguracji][api-management-save-configuration]

Wprowadź potrzebne zmiany na ekranie potwierdzenia, a następnie kliknij przycisk **Ok** Aby zapisać.

![Zapisywanie konfiguracji][api-management-save-configuration-confirm]

Po chwili konfiguracja jest zapisywana, i jest wyświetlany stan konfiguracji repozytorium, łącznie z datą i godziną wprowadzenia ostatniej zmiany konfiguracji i ostatniej synchronizacji między konfigurację usługi i repozytorium.

![Stan konfiguracji][api-management-configuration-status]

Po zapisaniu konfiguracji do repozytorium może być klonowane.

Aby uzyskać informacji na temat tej operacji za pomocą interfejsu API usługi REST zobacz [Zatwierdzenie migawki konfiguracji za pomocą interfejsu API usługi REST](https://msdn.microsoft.com/library/dn781420.aspx#CommitSnapshot).

## <a name="to-clone-the-repository-to-your-local-machine"></a>Aby klonowanie repozytorium na komputerze lokalnym

Aby klonowanie repozytorium, potrzebujesz adresu URL do repozytorium, nazwy użytkownika i hasła. Nazwa użytkownika i adres URL są wyświetlane w górnej części na karcie **repozytorium konfiguracji** .

![Klonowanie cyfra][api-management-configuration-git-clone]

Hasło jest generowany u dołu karty **repozytorium konfiguracji** .

![Generowanie hasła][api-management-generate-password]

Aby wygenerować hasło, najpierw upewnij się, że po **upływie** ustawiono wygasania odpowiednią datę i godzinę, a następnie kliknij **Wygenerować tokenu**.

![Hasło][api-management-password]

>[AZURE.IMPORTANT] Zanotuj to hasło. Po opuszczeniu tej strony hasła nie pojawi się ponownie.

W poniższych przykładach użyto narzędzia imprezie cyfra z [Cyfra dla systemu Windows](http://www.git-scm.com/downloads) , ale możesz użyć dowolnego narzędzia cyfra, które znasz.

Otwórz narzędzie cyfra w odpowiednim folderze i uruchom następujące polecenie, aby klonowanie repozytorium cyfra na komputerze lokalnym, za pomocą polecenia dostarczony przez portal programu publisher.

    git clone https://bugbashdev4.scm.azure-api.net/ 

Podaj nazwę użytkownika i hasło po wyświetleniu monitu.

Jeśli zostanie wyświetlony jakiekolwiek błędy, spróbuj zmodyfikować z `git clone` polecenie, aby uwzględnić nazwę użytkownika i hasło, jak pokazano w poniższym przykładzie.

    git clone https://username:password@bugbashdev4.scm.azure-api.net/

Jeśli ten komunikat o błędzie, spróbuj kodowania fragment hasło polecenia URL. Szybkim sposobem wykonaj następujące czynności jest otwarcie programu Visual Studio, a Wydaj następujące polecenie w **Oknie bezpośredniego**. Aby otworzyć **Okienko bezpośrednie**, otwórz dowolną rozwiązania lub projektu w programie Visual Studio (lub tworzenie nowej aplikacji konsoli puste) i wybierz pozycję **Windows**, **natychmiastowe** z menu **Debugowanie** .

    ?System.NetWebUtility.UrlEncode("password from publisher portal")

Konstruowanie polecenie cyfra przy użyciu hasła zakodowany wraz z lokalizacji repozytorium i nazwa użytkownika.

    git clone https://username:url encoded password@bugbashdev4.scm.azure-api.net/

Po repozytorium jest sklonowany można wyświetlać i pracować z nimi w lokalnym systemie plików. Aby uzyskać więcej informacji zobacz [plików i folderów informacje dotyczące struktury lokalnego repozytorium cyfra](#file-and-folder-structure-reference-of-local-git-repository).

## <a name="to-update-your-local-repository-with-the-most-current-service-instance-configuration"></a>Aby zaktualizować lokalne repozytorium najnowsze konfiguracji wystąpienia usługi

Jeśli wprowadzisz zmiany wystąpienia zarządzania interfejsu API usługi w portalu programu publisher lub przy użyciu interfejsu API usługi REST, należy zapisać te zmiany repozytorium przed zaktualizowaniem lokalnego repozytorium o najnowsze zmiany. Aby to zrobić, kliknij przycisk **Zapisz konfigurację do repozytorium** na karcie **repozytorium konfiguracji** w portalu programu publisher, a następnie Wydaj następujące polecenie w lokalnym repozytorium.

    git pull

Przed uruchomieniem `git pull` upewnij się, czy są w folderze dla lokalnego repozytorium. Jeśli po wykonaniu tylko `git clone` polecenia, a następnie należy zmienić katalogu usługi repo za pomocą polecenia następująco.

    cd bugbashdev4.scm.azure-api.net/

## <a name="to-push-changes-from-your-local-repo-to-the-server-repo"></a>Aby przekazać zmiany z sieci lokalnej repo do repo serwera

Aby przekazać zmiany z lokalnego repozytorium do repozytorium serwera, możesz zatwierdzić wprowadzone zmiany i push ich do repozytorium serwera. Aby zatwierdzić wprowadzone zmiany, Otwórz narzędzie polecenie cyfra, przejdź do katalogu lokalnego repozytorium i korzystać z następujących poleceń.

    git add --all
    git commit -m "Description of your changes"

Aby przekazać wszystkie zatwierdzenia na serwerze, uruchom następujące polecenie.

    git push

## <a name="to-deploy-any-service-configuration-changes-to-the-api-management-service-instance"></a>Aby wdrożyć zmiany konfiguracji usługi wystąpienie usługi zarządzania interfejsu API

Gdy lokalne zmiany są Projekt zatwierdzony i przypisany do repozytorium serwera, może je wdrożyć do wystąpienia usługi zarządzania interfejsu API.

![Wdrażanie][api-management-configuration-deploy]

Aby uzyskać informacji na temat tej operacji za pomocą interfejsu API usługi REST zobacz [zmiany wdrażanie cyfra konfiguracji bazy danych przy użyciu interfejsu API usługi REST](https://msdn.microsoft.com/library/dn781420.aspx#DeployChanges).

## <a name="file-and-folder-structure-reference-of-local-git-repository"></a>Odwołanie struktury plików i folderów lokalnego repozytorium cyfra

Pliki i foldery w repozytorium lokalne cyfra zawierają informacje o wystąpienie usługi konfiguracji.

| Element                       | Opis                                                                                |
|-------------------------   |--------------------------------------------------------------------------------------------|
| folder główny interfejs api zarządzania | Zawiera najwyższego poziomu konfiguracji wystąpienia usługi                                  |
| interfejsy API folderu                | Zawiera konfigurację dla interfejsów API w wystąpieniu usług                            |
| folder grupy              | Zawiera konfigurację dla grup w wystąpieniu usług                          |
| folder zasady            | Zawiera zasady w wystąpieniu usług                                              |
| portalStyles folder        | Zawiera konfigurację dla deweloperów portalu dostosowań w wystąpieniu usług |
| Products folder            | Zawiera konfigurację dla produktów w wystąpieniu usług                        |
| folder szablonów           | Zawiera konfigurację szablony wiadomości e-mail w wystąpieniu usług                 |

Każdy folder może zawierać jeden lub więcej plików, a w niektórych przypadkach jeden lub więcej folderów, na przykład folder dla każdego interfejsu API, produktu lub grupy. Pliki w każdym folderze są specyficzne dla typu obiektu opisanego przez nazwę folderu.

| Typ pliku | Cel                                                                |
|-----------|------------------------------------------------------------------------|
| JSON      | Informacje dotyczące odpowiednich jednostki konfiguracji                  |
| HTML      | Opisy obiektu, często wyświetlane w portalu — Deweloper |
| XML       | Zasady                                                      |
| arkuszy CSS       | Arkusze stylów dostosowywania portalu — Deweloper                        |

Te pliki można tworzyć, usunięte, edytować i zarządzanych w systemie plik lokalny i zmian wdrożony wystąpienia usługi zarządzania interfejsu API.

>[AZURE.NOTE] Następujące jednostki nie są zawarte w repozytorium cyfra i nie można skonfigurować przy użyciu cyfra.
>
>-    Użytkownicy
>-    Subskrypcje
>-    Właściwości
>-    Deweloper portalu podmiotów innych niż style

### <a name="root-api-management-folder"></a>Folder główny interfejs api zarządzania

Pierwiastek `api-management` folder zawiera `configuration.json` plik, który zawiera najwyższego poziomu informacji na temat wystąpienie usługi w następującym formacie.

    {
      "settings": {
        "RegistrationEnabled": "True",
        "UserRegistrationTerms": null,
        "UserRegistrationTermsEnabled": "False",
        "UserRegistrationTermsConsentRequired": "False",
        "DelegationEnabled": "False",
        "DelegationUrl": "",
        "DelegatedSubscriptionEnabled": "False",
        "DelegationValidationKey": ""
      },
      "$ref-policy": "api-management/policies/global.xml"
    }

Pierwsze cztery ustawienia (`RegistrationEnabled`, `UserRegistrationTerms`, `UserRegistrationTermsEnabled`, i `UserRegistrationTermsConsentRequired`) mapa, aby następujące ustawienia na karcie **tożsamości** w sekcji **Zabezpieczenia** .

| Ustawienie tożsamości                     | Mapy                                               |
|--------------------------------------|-------------------------------------------------------|
| RegistrationEnabled                  | Pole wyboru **Przekieruj użytkownikom anonimowym na stronę logowania** |
| UserRegistrationTerms                | Pole tekstowe **warunki użytkowania na tworzenie konta użytkownika**               |
| UserRegistrationTermsEnabled         | Pole wyboru **Pokaż warunki użytkowania na stronie Tworzenie konta**         |
| UserRegistrationTermsConsentRequired | Pole wyboru **Wymagaj zgody**                          |

![Ustawienia tożsamości][api-management-identity-settings]

Poniższe cztery ustawienia (`DelegationEnabled`, `DelegationUrl`, `DelegatedSubscriptionEnabled`, i `DelegationValidationKey`) mapa, aby następujące ustawienia na karcie **Delegowanie** w sekcji **Zabezpieczenia** .

| Ustawienia delegowania           | Mapy                                    |
|------------------------------|--------------------------------------------|
| DelegationEnabled            | Pole wyboru **Pełnomocnik logowania i zapisów**    |
| DelegationUrl                | **Adres URL punktu końcowego delegowanie** pola tekstowego        |
| DelegatedSubscriptionEnabled | Pole wyboru **Pełnomocnik produktu subskrypcji** |
| DelegationValidationKey      | **Pełnomocnik klucz sprawdzania poprawności** pola tekstowego        |

![Ustawienia delegowania][api-management-delegation-settings]

Ustawienie ostateczne `$ref-policy`, mapy do pliku instrukcji globalne zasady wystąpienia usługi.

### <a name="apis-folder"></a>interfejsy API folderu

`apis` Zawiera folder dla każdego interfejsu API w wystąpieniu usług, który zawiera następujące elementy.

-   `apis\<api name>\configuration.json`— to jest skonfigurowany do API i zawiera informacje dotyczące adresu URL usługi wewnętrznej bazy danych i operacji. Jest to te same informacje, które będzie zwracany w przypadku połączeń [uzyskać określone interfejsu API](https://msdn.microsoft.com/library/azure/dn781423.aspx#GetAPI) z `export=true` w `application/json` format.
-   `apis\<api name>\api.description.html`— to jest opis interfejsu API i odpowiada `description` właściwości [obiektu interfejsu API](https://msdn.microsoft.com/library/azure/dn781423.aspx#EntityProperties).
-   `apis\<api name>\operations\`— Ten folder zawiera `<operation name>.description.html` pliki, które mapowanie operacje w interfejsie API. Każdy plik zawiera opis jednej operacji w interfejsie API, której mapy do `description` właściwości [obiektu operacji](https://msdn.microsoft.com/library/azure/dn781423.aspx#OperationProperties) w interfejsie API usługi REST.

### <a name="groups-folder"></a>folder grupy

`groups` Zawiera folder dla każdej grupy zdefiniowane w wystąpieniu usług.

-   `groups\<group name>\configuration.json`— jest to konfiguracja grupy. Jest to te same informacje, które będzie zwracany w przypadku połączeń operację [Uzyskiwanie konkretnej grupy](https://msdn.microsoft.com/library/azure/dn776329.aspx#GetGroup) .
-   `groups\<group name>\description.html`— to jest opis grupy i odpowiada `description` właściwości [obiektu grupy](https://msdn.microsoft.com/library/azure/dn776329.aspx#EntityProperties).

### <a name="policies-folder"></a>folder zasady

`policies` Folder zawiera instrukcje zasad dla wystąpienia usługi.

-   `policies\global.xml`-zawiera zasady zdefiniowane w zakresie globalnym wystąpienia usługi.
-   `policies\apis\<api name>\`— Jeśli masz jakiekolwiek zasady zdefiniowane w zakresie interfejsu API są zawarte w tym folderze.
-   `policies\apis\<api name>\<operation name>\`skoroszyt — Jeśli masz jakiekolwiek zasady zdefiniowane w zakresie operacji są zawarte w tym folderze `<operation name>.xml` pliki, które zamapować na instrukcje zasad dla każdej operacji.
-   `policies\products\`— Jeśli masz jakiekolwiek zasady zdefiniowane w zakresie produktu, są zawarte w tym folderze, który zawiera `<product name>.xml` pliki, które mapowanie instrukcji zasad dla każdego produktu.

### <a name="portalstyles-folder"></a>portalStyles folder

`portalStyles` Folder zawiera arkusze konfiguracji i styl dla deweloperów dostosowań portalu dla wystąpienia usługi.

-   `portalStyles\configuration.json`-zawiera nazwy arkuszy stylów używane przez portal Deweloper
-   `portalStyles\<style name>.css`-każdego `<style name>.css` plik zawiera style dla portalu Deweloper (`Preview.css` i `Production.css` domyślnie).

### <a name="products-folder"></a>Products folder

`products` Zawiera folder dla każdego produktu zdefiniowane w wystąpieniu usług.

-   `products\<product name>\configuration.json`— jest to konfiguracja dla tego produktu. Jest to te same informacje, które będzie zwracany w przypadku połączeń operację [Uzyskiwanie określonego produktu](https://msdn.microsoft.com/library/azure/dn776336.aspx#GetProduct) .
-   `products\<product name>\product.description.html`— to jest opis produktu i odpowiada `description` właściwości [obiektu produktu](https://msdn.microsoft.com/library/azure/dn776336.aspx#Product) w interfejsie API usługi REST.

### <a name="templates"></a>Szablony

`templates` Folder zawiera konfiguracji dla [szablonów wiadomości e-mail](api-management-howto-configure-notifications.md) wystąpienia tej usługi.

-   `<template name>\configuration.json`— jest to konfiguracja szablonu wiadomości e-mail.
-   `<template name>\body.html`— jest to treści szablonu wiadomości e-mail.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać informacji na temat innych sposobów zarządzania wystąpienia usług Zobacz:

-   Zarządzanie wystąpienia usługi przy użyciu następujące polecenia cmdlet programu PowerShell
    -   [Wdrażanie usługi informacje dotyczące poleceń cmdlet programu PowerShell](https://msdn.microsoft.com/library/azure/mt619282.aspx)
    -   [Zarządzanie usługą informacje dotyczące poleceń cmdlet programu PowerShell](https://msdn.microsoft.com/library/azure/mt613507.aspx)
-   Zarządzanie wystąpienia usługi w portalu programu publisher
    -   [Zarządzanie do pierwszego interfejsu API](api-management-get-started.md)
-   Zarządzanie wystąpienia usługi przy użyciu interfejsu API usługi REST
    -   [Interfejs API interfejsu API usługi REST zarządzania odwołania](https://msdn.microsoft.com/library/azure/dn776326.aspx)

## <a name="watch-a-video-overview"></a>Obejrzyj film wideo

> [AZURE.VIDEO configuration-over-git]

[api-management-enable-git]: ./media/api-management-configuration-repository-git/api-management-enable-git.png
[api-management-git-enabled]: ./media/api-management-configuration-repository-git/api-management-git-enabled.png
[api-management-save-configuration]: ./media/api-management-configuration-repository-git/api-management-save-configuration.png
[api-management-save-configuration-confirm]: ./media/api-management-configuration-repository-git/api-management-save-configuration-confirm.png
[api-management-configuration-status]: ./media/api-management-configuration-repository-git/api-management-configuration-status.png
[api-management-configuration-git-clone]: ./media/api-management-configuration-repository-git/api-management-configuration-git-clone.png
[api-management-generate-password]: ./media/api-management-configuration-repository-git/api-management-generate-password.png
[api-management-password]: ./media/api-management-configuration-repository-git/api-management-password.png
[api-management-git-configure]: ./media/api-management-configuration-repository-git/api-management-git-configure.png
[api-management-configuration-deploy]: ./media/api-management-configuration-repository-git/api-management-configuration-deploy.png
[api-management-identity-settings]: ./media/api-management-configuration-repository-git/api-management-identity-settings.png
[api-management-delegation-settings]: ./media/api-management-configuration-repository-git/api-management-delegation-settings.png
[api-management-git-icon-enable]: ./media/api-management-configuration-repository-git/api-management-git-icon-enable.png




