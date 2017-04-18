<properties 
    pageTitle="Jak utworzyć interfejsy API w zarządzaniu interfejsu API platformy Azure" 
    description="Dowiedz się, jak utworzyć i skonfigurować interfejsy API w zarządzaniu interfejsu API Azure." 
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

# <a name="how-to-create-apis-in-azure-api-management"></a>Jak utworzyć interfejsy API w zarządzaniu interfejsu API platformy Azure

Interfejs API w zarządzaniu interfejsu API reprezentuje zestaw operacji, które mogą być wywoływane w aplikacjach klienckich. Nowe składniki interfejsu API są tworzone w portalu programu publisher, a następnie dodać odpowiednie operacje. Po dodaniu operacje API zostanie dodana do produktu i można opublikować. Po opublikowaniu interfejs API można subskrybuje i używane przez deweloperów.

Ten przewodnik zawiera pierwszy krok w procesie: jak utworzyć i skonfigurować nowego interfejsu API usługi zarządzania interfejsu API w. Uzyskać więcej informacji o operacji dodawania i publikowania produktu zobacz [Dodawanie operacje, aby interfejs API][] oraz [jak można tworzyć i publikować produktu][].

## <a name="create-new-api"> </a>Tworzenie nowego interfejsu API usługi

Interfejsy API są utworzona i skonfigurowana w portalu programu publisher. Aby uzyskać dostęp do portalu programu publisher, kliknij przycisk **Zarządzaj** w portalu klasyczny Azure dotyczące usługi zarządzania interfejsu API.

![Portal programu Publisher][api-management-management-console]

>Jeśli wystąpienie usługi zarządzania interfejsu API nie został jeszcze utworzony, zobacz [Tworzenie wystąpienie usługi zarządzania interfejsu API][] samouczka [Wprowadzenie do zarządzania interfejsu API Azure][] .

Kliknij pozycję **interfejsy API** z **Interfejsu API zarządzania** menu po lewej stronie, a następnie kliknij **Dodaj interfejsu API**.

![Tworzenie interfejsu API][api-management-create-api]

Okno **Dodawanie nowego interfejsu API usługi** umożliwia konfigurowanie nowego interfejsu API.

![Dodawanie nowego interfejsu API usługi][api-management-add-new-api]

Następujące pola służą do konfigurowania nowego interfejsu API.

-   **Interfejs API sieci Web nazwa** zawiera unikatową i opisową nazwę dla interfejsu API. Jest on wyświetlany w portali Deweloper i publisher.
-   **Adres URL usługi sieci Web** odwołuje się do wykonania API usługi HTTP. Interfejs API zarządzania umożliwia przesłanie dalej wezwania na ten adres.
-   Podstawowy adres URL usługi zarządzania interfejsu API widnieje **sufiks adresu URL interfejsu API sieci Web** . Podstawy adres URL jest wspólne dla wszystkich interfejsów API obsługiwanych przez wystąpienie usługi zarządzania interfejsu API. Zarządzanie interfejsu API odróżnia interfejsy API przez ich sufiks i w związku z tym sufiks musi być unikatowa dla każdego interfejsu API dla danego programu publisher.
-   **Schemat adresu URL interfejsu API sieci Web** określa protokołów można uzyskać dostęp do API. Protokół HTTPs określono domyślnie.
-   Opcjonalnie można dodać ten nowy interfejs API do produkt, kliknij **produktów (opcjonalnie)** listy rozwijanej i wybierz produkt. Ten krok można powtarzać wielokrotnie Dodawanie API do wiele produktów.

Skonfigurowane żądane wartości, kliknij przycisk **Zapisz**. Po utworzeniu nowego interfejsu API stronę podsumowania API jest wyświetlany w portalu programu publisher.

![Podsumowanie interfejsu API][api-management-api-summary]

## <a name="configure-api-settings"> </a>Interfejsu API Konfigurowanie ustawień

Karta **Ustawienia** zweryfikować i edytowanie konfiguracji interfejsu API. **Interfejs API sieci Web nazwę**, **adres URL usługi sieci Web**i **sufiks adresu URL interfejsu API sieci Web** są ustawiany po interfejsie API zostanie utworzona i którą można modyfikować w tym miejscu. **Opis** zawiera opcjonalny opis, a **adres URL sieci Web interfejs API systemu** Określa, protokołów można uzyskać dostęp do API.

![Ustawienia interfejsu API][api-management-api-settings]

Aby skonfigurować uwierzytelnianie bramy implementacji interfejsu API usługi wewnętrznej bazy danych, wybierz kartę **Zabezpieczenia** . **Przy użyciu poświadczeń** listy rozwijanej można skonfigurować uwierzytelnianie **podstawowe HTTP** lub **Certyfikaty klienta** . Aby użyć uwierzytelniania podstawowego HTTP, wystarczy wprowadzić wymaganych poświadczeń. Aby uzyskać informacje na temat korzystania z uwierzytelnianie klienta na podstawie certyfikatu zobacz [jak zabezpieczanie usług wewnętrznej przy użyciu funkcji uwierzytelniania certyfikat klienta w zarządzaniu interfejsu API Azure][].

Aby skonfigurować **uwierzytelnienia użytkownika** przy użyciu OAuth 2.0 również można kartę **Zabezpieczenia** . Aby uzyskać więcej informacji zobacz [jak Autoryzuj Deweloper konta za pomocą OAuth 2.0 w zarządzaniu interfejsu API Azure][].

![Ustawienia uwierzytelniania podstawowego][api-management-api-settings-credentials]

Kliknij przycisk **Zapisz** , aby zapisać zmiany wprowadzone w ustawieniach interfejsu API.

## <a name="next-steps"> </a>Następne kroki

Po utworzeniu interfejsu API i skonfigurować, następne kroki polegają na dodawanie operacji API, Dodaj API z produktem i opublikować go, tak aby była dostępna dla deweloperów. Aby uzyskać więcej informacji zobacz następujące artykuły.

-   [Jak dodać operacje do interfejsu API][]
-   [Jak tworzyć i publikować produktu][]





[api-management-create-api]: ./media/api-management-howto-create-apis/api-management-create-api.png
[api-management-management-console]: ./media/api-management-howto-create-apis/api-management-management-console.png
[api-management-add-new-api]: ./media/api-management-howto-create-apis/api-management-add-new-api.png
[api-management-api-settings]: ./media/api-management-howto-create-apis/api-management-api-settings.png
[api-management-api-settings-credentials]: ./media/api-management-howto-create-apis/api-management-api-settings-credentials.png
[api-management-api-summary]: ./media/api-management-howto-create-apis/api-management-api-summary.png
[api-management-echo-operations]: ./media/api-management-howto-create-apis/api-management-echo-operations.png

[What is an API?]: #what-is-api
[Create a new API]: #create-new-api
[Configure API settings]: #configure-api-settings
[Configure API operations]: #configure-api-operations
[Next steps]: #next-steps

[Jak dodać operacje do interfejsu API]: api-management-howto-add-operations.md
[Jak tworzyć i publikować produktu]: api-management-howto-add-products.md

[Wprowadzenie do zarządzania interfejsu API platformy Azure]: api-management-get-started.md
[Tworzenie wystąpienia usługi zarządzania interfejsu API]: api-management-get-started.md#create-service-instance
[Zabezpieczania usług wewnętrznej przy użyciu klienta certyfikatu uwierzytelniania w zarządzaniu interfejsu API platformy Azure]: api-management-howto-mutual-certificates.md
[Jak autoryzować konta Deweloper za pomocą OAuth 2.0 w zarządzaniu interfejsu API platformy Azure]: api-management-howto-oauth2.md