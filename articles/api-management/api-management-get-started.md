<properties
    pageTitle="Zarządzanie do pierwszego interfejsu API w zarządzaniu interfejsu API Azure | Microsoft Azure"
    description="Dowiedz się, jak tworzyć interfejsy API, dodać operacje i rozpocząć zarządzanie interfejsu API."
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
    ms.topic="hero-article"
    ms.date="10/25/2016"
    ms.author="sdanie"/>

# <a name="manage-your-first-api-in-azure-api-management"></a>Zarządzanie do pierwszego interfejsu API w zarządzaniu interfejsu API platformy Azure

## <a name="overview"> </a>— Omówienie

Ten przewodnik zawiera informacje o szybko rozpocząć pracę przy użyciu usługi zarządzania interfejsu API Azure i pierwszego połączenia interfejsu API.

## <a name="concepts"> </a>Co to jest zarządzanie interfejsu API Azure?

Zarządzanie interfejsu API Azure umożliwia wykonać dowolną wewnętrznej bazy danych i uruchamianie programu interfejsu API pełnoprawny oparte na nim.

Typowe scenariusze obejmują:

* **Zabezpieczanie infrastruktury przenośnych** przez bramkowanie programu access z klawiszami interfejsu API zapobieganie DOS ataki przy użyciu ograniczania lub za pomocą zasad zabezpieczeń zaawansowane, takie jak JWT token sprawdzania poprawności.
* **Włączanie model partnera ekosystemów** , oferowanie partnera szybkie rozpoczęcie korzystania za pośrednictwem portalu Deweloper i tworzenia elewacji interfejsu API rozdzielenie z implementacji wewnętrznych, które nie są dojrzałe zużycia partnera.
* **Uruchomiony program wewnętrznego interfejsu API** oferując scentralizowanej lokalizacji dla organizacji na komunikowanie się informacji o dostępności i najnowszych zmian do interfejsów API usługi bramkowanie dostępu według konta organizacji wszystkie na podstawie bezpiecznego kanału między brama interfejsu API i wewnętrznej bazy danych.


System składa się z następujących składników:

* **Interfejs API bramy** jest punkt końcowy który:
  * Akceptuje połączeń i kieruje je do swojego pomocą interfejsu API.
  * Sprawdza interfejsu API klawiszy, JWT tokeny certyfikaty i inne poświadczenia.
  * Wymusza przydziałami użycia i ocenić ograniczenia.
  * Przy użyciu usługi interfejsu API w czasie rzeczywistym bez modyfikacje kodu.
  * Buforowanie miejsce, w którym Konfigurowanie odpowiedzi wewnętrznej bazy danych.
  * Dzienniki połączeń metadanych na potrzeby analizy.

* **Portal programu publisher** jest interfejsu administracyjnego miejsce, w którym możesz skonfigurować program interfejsu API. Aby użyć:
    * Definiowanie lub zaimportuj schematu interfejsu API.
    * Pakiet interfejsów API do produktów.
    * Konfigurowanie zasad, takich jak przydziały lub przekształcenia interfejsów API.
    * Uzyskaj więcej informacji z analizy.
    * Zarządzanie użytkownikami.

* **Dzięki portalowi deweloperów** pełni funkcję obecności głównym sieci web dla deweloperów, gdzie można:
    * Dokumentacja API odczytu.
    * Wypróbuj interfejs API za pomocą interakcyjnego konsoli.
    * Tworzenie konta i subskrybowanie uzyskiwanie kluczy interfejsu API.
    * Analizy programu Access na ich zastosowania.


## <a name="create-service-instance"> </a>Utworzyć wystąpienia interfejsu API zarządzania

>[AZURE.NOTE] Aby użyć tego samouczka, potrzebne jest konto Azure. Jeśli nie masz konta, możesz utworzyć bezpłatne konto na kilka minut. Aby uzyskać szczegółowe informacje zobacz [Azure bezpłatnej wersji próbnej][].

Pierwszy krok w pracy z interfejsu API zarządzania jest utworzenie wystąpienia usługi. Zaloguj się do [Portalu klasyczny Azure][] , a następnie kliknij przycisk **Nowy**, **Usługi aplikacji**, **Zarządzania interfejsu API**, **Utwórz**.

![Interfejs API zarządzania nowego wystąpienia][api-management-create-instance-menu]

Dla **adresu URL**Określ nazwę unikatowe poddomenę służących do adresu URL usługi.

Wybierz odpowiedni **subskrypcji** i **Region** wystąpienia usługi. Po wprowadzeniu odpowiednie opcje, kliknij przycisk **Dalej** .

![Nowa usługa zarządzania interfejsu API][api-management-create-instance-step1]

Wprowadź **Firmy Contoso, Ltd.** **Nazwa organizacji**i wprowadź swój adres e-mail w polu **Adres E-Mail administratora** .

>[AZURE.NOTE] Ten adres e-mail jest używany powiadomienia z systemu zarządzania interfejsu API. Aby uzyskać więcej informacji zobacz [jak skonfigurować powiadomienia i szablony wiadomości e-mail w zarządzaniu interfejsu API Azure][].

![Nowa usługa zarządzania interfejsu API][api-management-create-instance-step2]

Wystąpienia usługi zarządzania interfejsu API są dostępne w trzech: Deweloper, Standard i Premium. Domyślnie nowe wystąpienia usługi zarządzania interfejsu API są tworzone w warstwie Deweloper. Zaznacz warstwy Standard lub Premium, zaznacz pole wyboru **Ustawienia zaawansowane** i wybierz żądany poziom na poniższym obrazie.

>[AZURE.NOTE] Warstwa deweloper jest utworzenie, testowanie i programy pilotażowe interfejsu API, gdzie wysoką dostępność nie ma znaczenia. W Standard i Premium poziomów można skalować liczba jednostek zastrzeżone do obsługi ruchu więcej. Poziomy Standard i Premium zapewniają usługi zarządzania interfejsu API z najbardziej moc obliczeniową i wydajność. Ten samouczek można wykonać przy użyciu dowolnego poziomu. Aby uzyskać więcej informacji na temat poziomów zarządzania interfejsu API zobacz [API zarządzania ceny][].

Kliknij pole wyboru, aby utworzyć wystąpienia usługi.

![Nowa usługa zarządzania interfejsu API][api-management-instance-created]

Po utworzeniu wystąpienia usługi następnym krokiem jest tworzenie lub importowanie interfejs API.

## <a name="create-api"> </a>Importowanie interfejs API

Interfejs API składa się z zestawu działań, które mogą być wywoływane z aplikacji klienckiej. Operacje interfejsu API są proxy do istniejących usług sieci web.

Interfejsy API można utworzyć (i operacje można dodawać) ręcznie lub mogą być importowane. W tym samouczku udostępniamy zaimportuje interfejsu API dla próbki kalkulatora usługi sieci web firmy Microsoft i hostowana Azure.

>[AZURE.NOTE] Aby uzyskać wskazówki na temat tworzenia interfejsu API i ręczne dodawanie operacji zobacz [jak utworzyć interfejsy API](api-management-howto-create-apis.md) i [jak dodać operacje, aby interfejs API](api-management-howto-add-operations.md).

Interfejsy API są skonfigurowane w portalu programu publisher jest dostępna za pośrednictwem portalu klasyczny Azure. Aby uzyskać dostęp do portalu programu publisher, kliknij przycisk **Zarządzaj** w portalu klasyczny Azure dotyczące usługi zarządzania interfejsu API.

![Portal programu Publisher][api-management-management-console]

Aby zaimportować kalkulatora interfejsu API, kliknij pozycję **interfejsy API** z **Interfejsu API zarządzania** menu po lewej stronie, a następnie kliknij **Importowanie interfejsu API**.

![Przycisk Importuj interfejsu API][api-management-import-api]

Wykonaj poniższe czynności, aby skonfigurować kalkulatora interfejsu API:

1. Kliknij pozycję **Z adresu URL**, wprowadź **http://calcapi.cloudapp.net/calcapi.json** w polu tekstowym **adres URL dokumentu Specyfikacja** i kliknij przycisk radiowy **Swagger** .
2. Wpisz **obliczenia** w polu tekstowym **sufiks adresu URL interfejsu API sieci Web** .
3. Kliknij w polu **produktów (opcjonalnie)** i wybierz **Starter**.
4. Kliknij przycisk **Zapisz** do zaimportowania API.

![Dodawanie nowego interfejsu API usługi][api-management-import-new-api]

>[AZURE.NOTE] **Zarządzanie interfejsu API** obsługuje obecnie zarówno 1.2 i 2.0 wersji dokumentu Swagger do zaimportowania. Upewnij się, że, mimo że [specyfikacji Swagger 2.0](http://swagger.io/specification) oświadcza, że `host`, `basePath`, i `schemes` właściwości są opcjonalne, dokumentu Swagger 2.0 **musi** zawierać te właściwości; w przeciwnym razie go nie można uzyskać zaimportowane. 

Po zaimportowaniu API stronę podsumowania API jest wyświetlany w portalu programu publisher.

![Interfejs API podsumowania][api-management-imported-api-summary]

Sekcja interfejsu API zawiera kilka kart. Na karcie **Podsumowanie** Wyświetla podstawowe metryki i informacje o interfejsie API. Karta [Ustawienia](api-management-howto-create-apis.md#configure-api-settings) służy do wyświetlania i edytowania konfiguracji interfejsu API. Karta [operacje](api-management-howto-add-operations.md) jest używana do zarządzania operacje interfejsu API. Kartę **Zabezpieczenia** można skonfigurować bramę uwierzytelniania dla serwera wewnętrznej bazy danych przy użyciu uwierzytelniania podstawowego lub [wzajemnego certyfikatu uwierzytelniania](api-management-howto-mutual-certificates.md)i skonfigurowanie [uwierzytelnienia użytkownika przy użyciu OAuth 2.0](api-management-howto-oauth2.md).  Karta **problemy** służy do wyświetlania problemy zgłaszane przez deweloperów, którzy korzystają z interfejsów API. Karta **produkty** służy do konfigurowania produktów, które zawierają ten interfejs API.

Domyślnie każde wystąpienie interfejsu API zarządzania udostępnia dwa przykładowe produkty:

-   **Starter**
-   **Nieograniczoną**

W tym samouczku podstawowe API kalkulatora został dodany do produktu Starter importowane API.

W celu nawiązywania połączeń z interfejsem API, deweloperzy muszą musisz zasubskrybować produktu, co umożliwia im dostępu do niego. Programiści mogą subskrybować produktów w portalu deweloper lub Administratorzy subskrybować deweloperów produktów w portalu programu publisher. Jesteś administratorem, od momentu utworzenia wystąpienia interfejsu API zarządzania w poprzednich krokach samouczka, aby domyślnie już subskrybujesz każdego produktu.

## <a name="call-operation"> </a>Połączeń operacji z portalu — Deweloper

Operacje można wywołać bezpośrednio z poziomu portalu Deweloper, który zapewnia wygodny sposób, aby wyświetlić i przetestować operacje interfejsu API. W tym kroku samouczek nawiąże połączenie podstawowe API kalkulatora operacja **Dodaj dwóch liczb całkowitych** . Kliknij pozycję **dzięki portalowi deweloperów** z menu u góry prawej strony portalu programu publisher.

![Dzięki portalowi deweloperów][api-management-developer-portal-menu]

Kliknij pozycję **interfejsy API** w górnym menu, a następnie kliknij **Prostego kalkulatora** , aby wyświetlić dostępne operacje.

![Dzięki portalowi deweloperów][api-management-developer-portal-calc-api]

Uwaga Przykładowe opisy i parametry, które zostały zaimportowane razem z interfejsu API i operacji, dostarczając dokumentację dla deweloperów, korzystające z tej operacji. Opisy te można także dodawać ręcznie dodanie operacji.

Aby zadzwonić do operacji **Dodaj dwie liczby całkowite** , kliknij pozycję **Wypróbuj**.

![Wypróbuj][api-management-developer-portal-calc-api-console]

Można wprowadzić kilka wartości dla parametrów lub Zachowaj wartości domyślne i kliknij przycisk **Wyślij**.

![HTTP Get][api-management-invoke-get]

Po wywołaniu operacji, dzięki portalowi deweloperów Wyświetla **Stan odpowiedzi**, **nagłówki odpowiedzi**i każda **zawartość odpowiedzi**.

![Odpowiedź][api-management-invoke-get-response]

## <a name="view-analytics"> </a>Wyświetlanie analiz

Aby wyświetlić analizy dla prostego kalkulatora, przejdź do portalu programu publisher, wybierając pozycję **Zarządzaj** z menu u góry prawej strony portalu Deweloper.

![Zarządzanie][api-management-manage-menu]

Widok domyślny portalu programu publisher to **pulpitu nawigacyjnego**, której omówiono wystąpienia interfejsu API zarządzania.

![Pulpit nawigacyjny][api-management-dashboard]

Umieść wskaźnik myszy na wykres dla **Prostego kalkulatora** wyświetlić określone metryki użycia interfejsu API dla danego okresu.

>[AZURE.NOTE] Jeśli nie widzisz żadnych wierszy na wykresie, powróć do portalu Deweloper i dzwonić w interfejsie API, poczekaj chwilę i następnie wróć do pulpitu nawigacyjnego.

Kliknij pozycję **Wyświetl szczegóły** , aby wyświetlić stronę podsumowania dla interfejsu API, łącznie z większą wersję wyświetlonej metryki.

![Analizy][api-management-mouse-over]

![Podsumowanie][api-management-api-summary-metrics]

Szczegółowe metryki i raportów kliknij pozycję **analizy** z menu **Zarządzania interfejsu API** po lewej stronie.

![Omówienie][api-management-analytics-overview]

W sekcji **analizy** zawiera następujące cztery karty:

-   **Rzut oka** zapewnia ogólnego metryki użycia i kondycji, a także deweloperów górny, Najpopularniejsze produkty, górny interfejsy API i górnym operacji.
-   **Użycie** podano szczegółowe interfejsu API i przepustowość, łącznie z reprezentacji geograficznej.
-   Zespoły **kondycji** na kody stanu pamięci podręcznej sukcesu stawki, czasy odpowiedzi i interfejsu API i świadczenie czasy odpowiedzi.
-   **Działania** zawiera raportów, które Przechodzenie do szczegółów na określone działania przez dewelopera, produktów i operacja interfejsu API.

## <a name="next-steps"> </a>Następne kroki

- Dowiedz się, jak [Ochrona z interfejsu API z limity szybkości](api-management-howto-product-with-rules.md).

[Azure bezpłatnej wersji próbnej]: http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=api_management_hero_a

[Create an API Management instance]: #create-service-instance
[Create an API]: #create-api
[Add an operation]: #add-operation
[Add the new API to a product]: #add-api-to-product
[Subscribe to the product that contains the API]: #subscribe
[Call an operation from the Developer Portal]: #call-operation
[View analytics]: #view-analytics
[Next steps]: #next-steps


[How to manage developer accounts in Azure API Management]: api-management-howto-create-or-invite-developers.md
[Configure API settings]: api-management-howto-create-apis.md#configure-api-settings
[Jak skonfigurować powiadomienia i szablony wiadomości e-mail w zarządzania interfejsu API platformy Azure]: api-management-howto-configure-notifications.md
[Responses]: api-management-howto-add-operations.md#responses
[How create and publish a product]: api-management-howto-add-products.md
[Interfejs API zarządzania ceny]: http://azure.microsoft.com/pricing/details/api-management/

[Portal Azure klasyczny]: https://manage.windowsazure.com/

[api-management-management-console]: ./media/api-management-get-started/api-management-management-console.png
[api-management-create-instance-menu]: ./media/api-management-get-started/api-management-create-instance-menu.png
[api-management-create-instance-step1]: ./media/api-management-get-started/api-management-create-instance-step1.png
[api-management-create-instance-step2]: ./media/api-management-get-started/api-management-create-instance-step2.png
[api-management-instance-created]: ./media/api-management-get-started/api-management-instance-created.png
[api-management-import-api]: ./media/api-management-get-started/api-management-import-api.png
[api-management-import-new-api]: ./media/api-management-get-started/api-management-import-new-api.png
[api-management-imported-api-summary]: ./media/api-management-get-started/api-management-imported-api-summary.png
[api-management-calc-operations]: ./media/api-management-get-started/api-management-calc-operations.png
[api-management-list-products]: ./media/api-management-get-started/api-management-list-products.png
[api-management-add-api-to-product]: ./media/api-management-get-started/api-management-add-api-to-product.png
[api-management-add-myechoapi-to-product]: ./media/api-management-get-started/api-management-add-myechoapi-to-product.png
[api-management-api-added-to-product]: ./media/api-management-get-started/api-management-api-added-to-product.png
[api-management-developers]: ./media/api-management-get-started/api-management-developers.png
[api-management-add-subscription]: ./media/api-management-get-started/api-management-add-subscription.png
[api-management-add-subscription-window]: ./media/api-management-get-started/api-management-add-subscription-window.png
[api-management-subscription-added]: ./media/api-management-get-started/api-management-subscription-added.png
[api-management-developer-portal-menu]: ./media/api-management-get-started/api-management-developer-portal-menu.png
[api-management-developer-portal-calc-api]: ./media/api-management-get-started/api-management-developer-portal-calc-api.png
[api-management-developer-portal-calc-api-console]: ./media/api-management-get-started/api-management-developer-portal-calc-api-console.png
[api-management-invoke-get]: ./media/api-management-get-started/api-management-invoke-get.png
[api-management-invoke-get-response]: ./media/api-management-get-started/api-management-invoke-get-response.png
[api-management-manage-menu]: ./media/api-management-get-started/api-management-manage-menu.png
[api-management-dashboard]: ./media/api-management-get-started/api-management-dashboard.png

[api-management-add-response]: ./media/api-management-get-started/api-management-add-response.png
[api-management-add-response-window]: ./media/api-management-get-started/api-management-add-response-window.png
[api-management-developer-key]: ./media/api-management-get-started/api-management-developer-key.png
[api-management-mouse-over]: ./media/api-management-get-started/api-management-mouse-over.png
[api-management-api-summary-metrics]: ./media/api-management-get-started/api-management-api-summary-metrics.png
[api-management-analytics-overview]: ./media/api-management-get-started/api-management-analytics-overview.png
[api-management-analytics-usage]: ./media/api-management-get-started/api-management-analytics-usage.png
[api-management-]: ./media/api-management-get-started/api-management-.png
[api-management-]: ./media/api-management-get-started/api-management-.png
