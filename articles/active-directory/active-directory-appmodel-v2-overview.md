<properties
    pageTitle="Omówienie punktu końcowego 2.0 | Microsoft Azure"
    description="Wprowadzenie do tworzenia aplikacji zarówno Account Microsoft, jak i usługi Azure Active Directory logowania."
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
    ms.date="09/27/2016"
    ms.author="dastrock"/>

# <a name="sign-in-microsoft-account--azure-ad-users-in-a-single-app"></a>Logowania & Account Microsoft Azure AD użytkowników w jednej aplikacji

W przeszłości deweloperem aplikacji do obsługi konta serwera Microsoft i usługi Azure Active Directory została wymagane do integracji z dwóch oddzielnych systemów.  Teraz Przedstawiamy nowa wersja interfejsu API uwierzytelniania umożliwiający Zaloguj się przy użyciu obu typów kont przy użyciu systemu Azure AD.  System uwierzytelniania zbieżnych nazywa się **punkt końcowy 2.0**.  Z punktem końcowym 2.0 — jeden Prosta integracja umożliwia osiągnięcia grupy odbiorców wyświetlanego miliony użytkowników z kontami osobistych i służbowych/szkolnych.

Aplikacje korzystające z punkt końcowy 2.0 — również mogą używać pozostałych interfejsy API z [Programu Microsoft Graph](https://graph.microsoft.io) i [usługi Office 365](https://msdn.microsoft.com/office/office365/howto/authenticate-Office-365-APIs-using-v2) za pomocą dowolnego typu konta.

<!-- For a quick introduction to the v2.0 endpoint, please view the [Getting Started with Microsoft Identities: Enterprise Grade Sign In For Your Apps](https://azure.microsoft.com/documentation/videos/build-2016-getting-started-with-microsoft-identities-enterprise-grade-sign-in-for-your-apps/) video. -->

## <a name="getting-started"></a>Wprowadzenie
[AZURE.VIDEO build-2016-getting-started-with-microsoft-identities-enterprise-grade-sign-in-for-your-apps]

Wybierz pozycję Ulubione platformy z poniższej listy do tworzenia aplikacji przy użyciu naszych bibliotek Otwórz źródło i struktury.  Możesz też umożliwia naszą dokumentację Protocol (protokół) OAuth 2.0 i łączenie OpenID Wyślij / Odbierz wiadomości protokołu bezpośrednio bez konieczności używania biblioteki uwierzytelniania.

<!-- TODO: Finalize this table  -->
[AZURE.INCLUDE [active-directory-v2-quickstart-table](../../includes/active-directory-v2-quickstart-table.md)]

## <a name="whats-new"></a>Co nowego
W tym miejscu informacji koncepcyjnych będą przydatne zrozumieć, co to jest & co nie jest możliwe z punktem końcowym 2.0.

- Informacje na temat [typów aplikacje, które można tworzyć z punktem końcowym 2.0](active-directory-v2-flows.md).
- Opis [ograniczeń, ograniczeń oraz ograniczenia](active-directory-v2-limitations.md) z punktem końcowym 2.0.
- Ostatnio Dodaliśmy obsługę [Administrator ograniczone zakresów](active-directory-v2-scopes.md) i [Uwierzytelnianie OAuth2 klient, który przyznania poświadczeń](active-directory-v2-protocols-oauth-client-creds.md).  Ich wypróbować!

## <a name="reference"></a>Odwołania
Te łącza będą przydatne w przypadku poznawanie platformy szczegóły dotyczące:

- Tworzenie 2016: [Wprowadzenie do programu Microsoft tożsamości: znak klasy przedsiębiorstwa w aplikacji](https://azure.microsoft.com/documentation/videos/build-2016-getting-started-with-microsoft-identities-enterprise-grade-sign-in-for-your-apps/)
- Uzyskiwanie pomocy na temat przepełnienie stosu przy użyciu [azure-usługi active directory](http://stackoverflow.com/questions/tagged/azure-active-directory) lub [adal](http://stackoverflow.com/questions/tagged/adal) znaczniki.
- [Odwołanie do protokołu 2.0](active-directory-v2-protocols.md)
- [Odwołania do tokenu 2.0](active-directory-v2-tokens.md)
- [informacje dotyczące biblioteki 2.0](active-directory-v2-libraries.md)
- [Zakresy i zgody na punkt końcowy 2.0](active-directory-v2-scopes.md)
- [Portal rejestracji aplikacji firmy Microsoft](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)
- [Odwołanie interfejsu API pozostałych usługi Office 365](https://msdn.microsoft.com/office/office365/howto/authenticate-Office-365-APIs-using-v2)
- [Program Microsoft Graph](https://graph.microsoft.io)