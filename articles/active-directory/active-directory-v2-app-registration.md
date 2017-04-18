<properties
    pageTitle="Rejestracja aplikacji 2.0 | Microsoft Azure"
    description="Jak zarejestrować aplikację z firmą Microsoft włączania logowania i uzyskiwanie dostępu do usług firmy Microsoft przy użyciu punkt końcowy 2.0"
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

# <a name="how-to-register-an-app-with-the-v20-endpoint"></a>Jak zarejestrować aplikację z punktem końcowym 2.0

Aby utworzyć aplikację, która akceptuje zarówno MSA & Azure AD logowania, musisz najpierw zarejestrować aplikację z firmą Microsoft.  W tej chwili nie będzie mógł korzystać z żadnych istniejących aplikacji może być z usługą Azure Active Directory lub MSA - musisz całkiem nowy komentarz.

> [AZURE.NOTE]
    Nie wszystkie scenariusze usługi Azure Active Directory i funkcje są obsługiwane przez punkt końcowy 2.0.  Aby określić, należy użyć punktu końcowego 2.0, przeczytaj o [ograniczenia 2.0](active-directory-v2-limitations.md).

## <a name="visit-the-microsoft-app-registration-portal"></a>Odwiedź portal rejestracji aplikacji firmy Microsoft
Najpierw najpierw — przejdź do [https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).  Jest to nowy portal rejestracji aplikacji miejsce, w którym można zarządzać swoimi aplikacjami firmy Microsoft.

Zaloguj się przy użyciu, albo osobiste lub służbowe konto Microsoft.  Jeśli nie masz albo utwórz nowe konto osobiste. Zrealizuj nie trwa długo —, firma Microsoft będzie czekać tutaj.

Zrobić? Należy teraz można patrzy listę aplikacji firmy Microsoft, która jest prawdopodobnie puste.  Załóżmy zmienić.

Kliknij pozycję **Dodaj aplikację**, a następnie nadaj plikowi nazwę.  Portalu przypisze aplikacji globalnie unikatowy identyfikator aplikacji, który ma być używany w dalszej części kodu.  Jeśli aplikacji zawiera składnik po stronie serwera wymaga tokeny dostępu dla połączeń interfejsy API (Pomyśl: Office, Azure lub własnego interfejsu API sieci web), warto również utworzyć **Hasło aplikacji** tutaj.
<!-- TODO: Link for app secrets -->

Następnie dodaj platform, który będzie używany w aplikacji.

- W przypadku aplikacji sieci web podaj **Przekierowywać URI** miejsce, w którym można wysyłać wiadomości do logowania.
- Dla aplikacji dla urządzeń przenośnych skopiuj uri Przekieruj domyślnych automatycznego utworzenia w dół.

Opcjonalnie można dostosować wygląd strony logowania w sekcji profilu.  Upewnij się przed przejściem kliknij przycisk **Zapisz** .

> [AZURE.NOTE] Podczas tworzenia aplikacji przy użyciu [https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)aplikacji zostanie zarejestrowany w dzierżawie głównym konta, którego używasz, aby zalogować się do portalu.  Oznacza to, że nie można zarejestrować aplikacji w dzierżawie usługi Azure AD przy użyciu osobistego konta Microsoft.  Jeśli chcesz jawnie rejestrowania aplikacji w szczególności dzierżawy, zaloguj się przy użyciu konta utworzony dzierżawy.

## <a name="build-a-quick-start-app"></a>Tworzenie aplikacji szybki start
Teraz, gdy masz aplikacji firmy Microsoft, można wykonać materiały szkoleniowe 2.0 — szybki start.  Poniżej przedstawiono kilka zaleceń:

[AZURE.INCLUDE [active-directory-v2-quickstart-table](../../includes/active-directory-v2-quickstart-table.md)]
