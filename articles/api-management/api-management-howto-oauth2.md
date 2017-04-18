<properties 
    pageTitle="Jak autoryzować konta Deweloper za pomocą OAuth 2.0 w zarządzaniu interfejsu API platformy Azure" 
    description="Dowiedz się, jak autoryzować użytkowników przy użyciu OAuth 2.0 w zarządzaniu interfejsu API." 
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

# <a name="how-to-authorize-developer-accounts-using-oauth-20-in-azure-api-management"></a>Jak autoryzować konta Deweloper za pomocą OAuth 2.0 w zarządzaniu interfejsu API platformy Azure

Wiele funkcji API obsługuje [OAuth 2.0](http://oauth.net/2/) do zabezpieczania API i upewnij się, że tylko prawidłowe użytkownicy mają dostęp i są dostępne tylko zasobów, do których jest uprawniony. Aby użyć konsoli Deweloper interakcyjnych Azure interfejsu API zarządzania takie interfejsy API, usługi umożliwia skonfigurowanie wystąpienia usług do pracy z Twojej OAuth 2.0 włączony interfejs API.

## <a name="prerequisites"> </a>Wymagania wstępne

Ten przewodnik pokazano, jak skonfigurować wystąpienia usługi zarządzania interfejsu API do używania autoryzacji OAuth 2.0 dla deweloperów konta, ale nie pokazano, jak skonfigurować dostawcę OAuth 2.0. Konfiguracja dla każdego dostawcy OAuth 2.0 różni się, mimo że czynności są podobne, a wymagane elementy dane używane do konfigurowania OAuth 2.0 wystąpienia usługi zarządzania interfejsu API są takie same. W tym temacie przedstawiono przykłady użycia usługi Azure Active Directory jako dostawca OAuth 2.0.

>[AZURE.NOTE] Aby uzyskać więcej informacji na temat konfigurowania 2.0 OAuth przy użyciu usługi Azure Active Directory Zobacz przykładowe [Aplikacji sieci Web — GraphAPI-DotNet][] .

## <a name="step1"> </a>Skonfigurować serwer autoryzacji OAuth 2.0 w zarządzaniu interfejsu API

Aby rozpocząć pracę, kliknij przycisk **Zarządzaj** w portalu klasyczny Azure dotyczące usługi zarządzania interfejsu API. Powoduje przejście do portalu zarządzania interfejsu API programu publisher.

![Portal programu Publisher][api-management-management-console]

>[AZURE.NOTE] Jeśli wystąpienie usługi zarządzania interfejsu API nie został jeszcze utworzony, zobacz [Tworzenie wystąpienie usługi zarządzania interfejsu API][] samouczka [Wprowadzenie do zarządzania interfejsu API Azure][] .

W menu **Zarządzania interfejsu API** po lewej stronie kliknij pozycję **Zabezpieczenia** , kliknij pozycję **OAuth 2.0**, a następnie kliknij **Dodaj autoryzacji serwera**.

![OAuth 2.0][api-management-oauth2]

Po kliknięciu pozycji **Dodaj autoryzacji serwera**, zostanie wyświetlony nowy formularz autoryzacji w serwera.

![Nowy serwer][api-management-oauth2-server-1]

Wprowadź nazwę i opcjonalny opis w polach **Nazwa** i **Opis** . 

>[AZURE.NOTE] Te pola są używane do identyfikowania serwera autoryzacji OAuth 2.0 w bieżącym wystąpieniu usług zarządzania interfejsu API i ich wartości pochodzi z serwerem OAuth 2.0.

Wprowadź **adres URL strony rejestracji klienta**. Ta strona jest miejsce, w którym użytkownicy mogą tworzyć i zarządzać ich kontami i zmienia się w zależności od dostawcy OAuth 2.0 używanego. **Adres URL strony rejestracji klienta** punktów do strony, którą użytkownicy mogą używać do tworzenia i konfigurowania własnych kont dla OAuth 2.0 do grupy dostawców obsługujących zarządzanie kontami użytkowników. Niektóre organizacje Konfigurowanie lub nie korzystać z tej funkcji, nawet jeśli dostawcy OAuth 2.0 obsługuje tę funkcję. Jeśli dostawca OAuth 2.0 nie zarządzania skonfigurowano kont użytkownika, wprowadź symbolu zastępczego adresu URL tutaj takie jak adres URL swojej firmy, lub takich jak `https://placeholder.contoso.com`.

Następnej sekcji formularza zawiera **Kod autoryzacji udzielić typów**, **adres URL punktu końcowego autoryzacji**i **metody żądania autoryzacji** ustawienia.

![Nowy serwer][api-management-oauth2-server-2]

Określ **Kod autoryzacji udzielić typów** , zaznaczając odpowiednie typy. **Kod autoryzacji** określono domyślnie.

Wprowadź **adres URL punktu końcowego autoryzacji**. Dla usługi Azure Active Directory, ten adres URL będzie podobny do następującego adresu URL, gdzie `<client_id>` jest zastępowany identyfikator klienta, który identyfikuje aplikację na serwerze OAuth 2.0.

    https://login.windows.net/<client_id>/oauth2/authorize

**Metoda żądania autoryzacji** Określa, jak żądanie autoryzacji jest wysyłana do serwera OAuth 2.0. **Pobieranie** jest zaznaczone domyślnie.

Następnej sekcji to miejsce, w którym określono **adres URL punktu końcowego tokenu**, **metod uwierzytelniania klienta**, **Wysyłanie metody token dostępu**i **zakresu domyślnego** .

![Nowy serwer][api-management-oauth2-server-3]

Serwer Azure Active Directory OAuth 2.0 **Token adres URL punktu końcowego** będą mieć następujący format miejsce, w którym `<APPID>` ma format `yourapp.onmicrosoft.com`.

    https://login.windows.net/<APPID>/oauth2/token

Ustawienie domyślne **metody uwierzytelniania klient** jest **podstawowy**, a **token dostępu wysyłania metoda** jest **Nagłówek uwierzytelnienia**. Wartości te są skonfigurowane w tej sekcji formularza, wraz z **zakresu domyślnego**.

Sekcja **poświadczenia klienta** zawiera **Identyfikator klienta** i **tajny klienta**, które są uzyskiwane w procesie tworzenia i konfiguracji serwera OAuth 2.0. Gdy określono **Identyfikator klienta** i **klienta hasła** , jest generowany **redirect_uri** **kodu autoryzacji** . Ten identyfikator URI jest używany do konfigurowania adresu URL odpowiedź w konfiguracji serwera OAuth 2.0.

![Nowy serwer][api-management-oauth2-server-4]

Jeśli **Kod autoryzacji udzielić typy** ustawiono **hasło właściciela zasobu**, sekcji **poświadczenia hasła właściciela zasobu** służy do określania tych poświadczeń; w przeciwnym razie użytkownik może pozostaw to pole puste.

![Nowy serwer][api-management-oauth2-server-5]

Po zakończeniu formularza kliknij pozycję **Zapisz** zapisywania konfiguracji serwera autoryzacji interfejsu API zarządzania OAuth 2.0. Po zapisaniu konfiguracji serwera można skonfigurować interfejsy API użyć tej konfiguracji, jak pokazano w następnej sekcji.

## <a name="step2"> </a>Konfigurowanie interfejsu API do uwierzytelnienia użytkownika OAuth 2.0 za pomocą

Kliknij **interfejsy API** z **Interfejsu API zarządzania** menu po lewej stronie, kliknij nazwę odpowiedniej interfejsu API, kliknij pozycję **Zabezpieczenia**, a następnie zaznacz pole wyboru obok **OAuth 2.0**.

![Autoryzacja użytkownika][api-management-user-authorization]

Wybierz żądany **autoryzacji serwera** z listy rozwijanej, a następnie kliknij przycisk **Zapisz**.

![Autoryzacja użytkownika][api-management-user-authorization-save]

## <a name="step3"> </a>Testowanie uwierzytelnienia użytkownika OAuth 2.0 w portalu — Deweloper

Po skonfigurowano serwer autoryzacji OAuth 2.0 i skonfigurowany do interfejsu API tego serwera, można go przetestować, przechodząc do portalu Deweloper i nawiązywania połączeń z interfejsem API.  Kliknij pozycję **dzięki portalowi deweloperów** w prawym górnym menu.

![Dzięki portalowi deweloperów][api-management-developer-portal-menu]

Kliknij pozycję **interfejsy API** w górnym menu i wybierz **Interfejs API Echo**.

![Interfejs API echo][api-management-apis-echo-api]

>[AZURE.NOTE] Jeśli masz tylko jeden interfejs API skonfigurowane lub widoczny dla Twojego konta, klikając pozycję interfejsy API przejście bezpośrednio z operacji dla tego interfejsu API.

Wybierz operację **Uzyskiwanie zasobów** , kliknij polecenie **Otwórz Konsola**, a następnie wybierz **Kod autoryzacji** z listy rozwijanej.

![Otwieranie konsoli][api-management-open-console]

Po zaznaczeniu **Kod autoryzacji** okno podręczne zostanie wyświetlona w formularzu logowania dostawcy OAuth 2.0. W tym przykładzie formularz logowania jest udostępniany przez usługi Azure Active Directory.

>[AZURE.NOTE] Jeśli masz wyskakujące okienka wyłączona, zostanie wyświetlony monit umożliwiający przez przeglądarkę. Po włączeniu ich pojawi się wybierz **Kod autoryzacji** w ponownie i formularz logowania.

![Rejestrowanie][api-management-oauth2-signin]

Po zapisaniu, **nagłówków żądania** są wypełnione `Authorization : Bearer` nagłówek, która zezwala na żądanie.

![Token nagłówek żądania][api-management-request-header-token]

Na tym etapie można skonfigurować żądane wartości parametrów pozostała i prześlij żądanie. 

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji o korzystaniu z OAuth 2.0 i zarządzania interfejsu API Zobacz poniższym klipie wideo i towarzyszących [artykuł](api-management-howto-protect-backend-with-aad.md).

> [AZURE.VIDEO protecting-web-api-backend-with-azure-active-directory-and-api-management]

[api-management-management-console]: ./media/api-management-howto-oauth2/api-management-management-console.png
[api-management-oauth2]: ./media/api-management-howto-oauth2/api-management-oauth2.png
[api-management-user-authorization]: ./media/api-management-howto-oauth2/api-management-user-authorization.png
[api-management-user-authorization-save]: ./media/api-management-howto-oauth2/api-management-user-authorization-save.png
[api-management-oauth2-signin]: ./media/api-management-howto-oauth2/api-management-oauth2-signin.png
[api-management-request-header-token]: ./media/api-management-howto-oauth2/api-management-request-header-token.png
[api-management-developer-portal-menu]: ./media/api-management-howto-oauth2/api-management-developer-portal-menu.png
[api-management-open-console]: ./media/api-management-howto-oauth2/api-management-open-console.png
[api-management-oauth2-server-1]: ./media/api-management-howto-oauth2/api-management-oauth2-server-1.png
[api-management-oauth2-server-2]: ./media/api-management-howto-oauth2/api-management-oauth2-server-2.png
[api-management-oauth2-server-3]: ./media/api-management-howto-oauth2/api-management-oauth2-server-3.png
[api-management-oauth2-server-4]: ./media/api-management-howto-oauth2/api-management-oauth2-server-4.png
[api-management-oauth2-server-5]: ./media/api-management-howto-oauth2/api-management-oauth2-server-5.png
[api-management-apis-echo-api]: ./media/api-management-howto-oauth2/api-management-apis-echo-api.png


[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Wprowadzenie do zarządzania interfejsu API platformy Azure]: api-management-get-started.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Tworzenie wystąpienia usługi zarządzania interfejsu API]: api-management-get-started.md#create-service-instance

[http://oauth.net/2/]: http://oauth.net/2/
[Aplikacja sieci Web — GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

