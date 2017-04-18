<properties
    pageTitle="Tematy pomocy portalu rejestracji aplikacji | Microsoft Azure"
    description="Opis różne funkcje w portalu rejestracji aplikacji firmy Microsoft."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>

# <a name="app-registration-reference"></a>Odwołanie rejestracji aplikacji
Niniejszy dokument zawiera kontekst i opisy poszczególnych funkcji dostępnych w portalu rejestracji aplikacji Microsoft [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).

## <a name="my-applications"></a>Aplikacje
Ta lista zawiera wszystkie aplikacje zarejestrowany do użytku z punktem końcowym 2.0 Azure AD.  Te aplikacje mogą się zalogować użytkownicy z obu osobistych kont z konta Microsoft i konta służbowego/szkolnego z usługi Azure Active Directory.  Aby dowiedzieć się więcej na temat punktu końcowego 2.0 Azure AD, zobacz nasze [2.0 — omówienie](active-directory-appmodel-v2-overview.md).  Te aplikacje również można zintegrować z końcowym uwierzytelniania konta Microsoft, `https://login.live.com`.

## <a name="live-sdk-applications"></a>Live SDK aplikacji
Ta lista zawiera wszystkie aplikacje zarejestrowany do użytku wyłącznie za pomocą konta Microsoft.  Nie są włączone do użytku z usługi Azure Active Directory jakiejkolwiek.  Jest to miejsce, w którym znajdują się wszystkie aplikacje, które wcześniej został zarejestrowany z portalu Deweloper MSA pod adresem `https://account.live.com/developers/applications`.  Wszystkie funkcje, które były wykonywane na `https://account.live.com/developers/applications` można teraz wykonać w tym nowego portalu `https://apps.dev.microsoft.com`.  Jeśli masz dalsze pytania dotyczące aplikacji konta Microsoft, skontaktuj się z nami.

## <a name="application-secrets"></a>Stosowanie hasła
Hasła aplikacji są poświadczeń aplikacji do wykonania niezawodne [uwierzytelnianie klientów](http://tools.ietf.org/html/rfc6749#section-2.3) z usługą Azure Active Directory.  W OAuth i łączenie OpenID hasła aplikacji często nosi nazwę `client_secret`.  W polu Protokół 2.0 dowolnej aplikacji, która odbiera tokenu zabezpieczającego w lokalizacji adresach sieci web (za pomocą `https` schemat), należy użyć hasła aplikacji do identyfikacji Azure AD przy realizacji tej tokenu zabezpieczającego.  Ponadto, wszelkie klientami tego recieves tokeny na urządzeniu będzie zabronione z przeprowadzać uwierzytelnianie klienta przy użyciu hasła aplikacji w celu zapobieżenia go na przechowywanie hasła w środowiskach niebezpiecznej.

Każdą z nich może zawierać dwa hasła prawidłową aplikacją na dowolnym etapie w czasie.  Zachowywanie dwa hasła, masz ablilty do przeprowadzania okresowych przerzucania klucza poprzez całego środowiska aplikacji.  Po przeprowadzeniu migracji całości aplikacji do nowego hasła, możesz usunąć stare hasło i obsługi administracyjnej nową.

W tym momencie tylko dwa typy hasła aplikacji są dozwolone w portalu rejestracji aplikacji.  Wybieranie **Generowanie nowego hasła** Generowanie i przechowywanie tajnego w magazynie odpowiednich danych, które są pomocne w aplikacji.  Wybieranie **Wygenerować nową parę klucza** spowoduje utworzenie nowego pary kluczy publicznych i prywatnych, które mogą być pobierane i używany do uwierzytelniania klienta Azure AD.

## <a name="profile"></a>Profil
W sekcji profilu portal rejestracji aplikacji umożliwia dostosowywanie stronie logowania do aplikacji.  W tej chwili można zmienić znak logo aplikacji strony, warunki adres URL usługi i zasady zachowania poufności informacji.  Logo nie może być przezroczysty obraz pikseli 48 x 48 lub 50 x 50 w pliku GIF, PNG lub JPEG 15 KB lub mniejszym.  Spróbuj zmiana wartości i wyświetlania wyniku logowania na stronie!

## <a name="live-sdk-support"></a>Obsługa Live SDK
Po włączeniu "Live SDK Obsługa" wszelkie hasła aplikacji, możesz utworzyć będzie obsługi administracyjnej do obu Azure AD i magazynów Account Microsoft.  Dzięki temu będzie aplikacjom łączyć bezpośrednio z usługi Microsoft Account (login.live.com).  Jeśli chcesz utworzyć aplikację za pomocą Account Microsoft bezpośrednio (a nie za pomocą punkt końcowy 2.0 Azure AD), należy upewnić się, że jest włączona obsługa Live SDK.

Wyłączenie obsługi Live SDK będziesz mieć pewność, że hasło aplikacji tylko są zapisywane w danych Azure AD przechowywania.  Dane Azure AD magazynu zawiera przepisów klasy korporacyjnej, umożliwiające spełnić pewne kryteria, takie jak zgodności dotyczących FISMA.  Możesz włączyć obsługę Live SDK, aplikacja mogą nie osiągnąć zgodność z niektóre z tych standardów.

Jeśli planujesz tylko ponowne używanie punkt końcowy 2.0 Azure AD, możesz wyłączyć bezpieczne Live SDK pomocy technicznej.

