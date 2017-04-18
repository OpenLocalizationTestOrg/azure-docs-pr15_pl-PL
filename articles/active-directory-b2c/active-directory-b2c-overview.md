<properties
    pageTitle="B2C Azure Active Directory: Omówienie | Microsoft Azure"
    description="Tworzenie aplikacji dla klientów indywidualnych skierowaną z Azure Active Directory B2C"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-sign-up-and-sign-in-consumers-in-your-applications"></a>Azure Active Directory B2C: Utwórz konto, a następnie zaloguj się konsumentów w aplikacjach

Azure Active Directory B2C jest Zarządzanie tożsamościami pełna chmury witryny sieci web dla klientów indywidualnych skierowaną i aplikacji dla urządzeń przenośnych. Jest usługą globalny wysokiej dostępności, która może setki milionów tożsamości dla klientów indywidualnych. Oparte na bezpiecznej platformy klasy korporacyjnej, Azure Active Directory B2C przechowywana aplikacji, Twoja firma i osób korzystających ze chroniony.

W przeszłości deweloperów, którzy przetransponowane mogą rejestrować się i zalogować się konsumentów w aplikacjach czy zostały zapisane własne kodu. I czy jest używane w lokalnej bazy danych lub systemy do przechowywania nazwy użytkowników i hasła. Azure Active Directory B2C oferuje deweloperów lepszym sposobem integracji Zarządzanie tożsamościami dla klientów indywidualnych w ich aplikacjach za pomocą platformy bezpieczne, zgodne ze standardami i bogatego zestawu extensible zasady. Jeśli używasz Azure Active Directory B2C, osób korzystających ze mogą tworzyć konta dla aplikacji przy użyciu ich istniejącego konta społecznościowych (Facebook, Google, Amazon, LinkedIn) lub tworząc nowe poświadczenia (adres e-mail i hasło, lub nazwy użytkownika i hasła); nazywamy ostatnie "rachunki lokalne."

## <a name="get-started"></a>Rozpoczynanie pracy

Aby utworzyć aplikację, która akceptuje indywidualnych potwierdzenia i zaloguj się, możesz będzie pierwszy musisz rejestrować aplikacji z B2C Azure Active Directory dzierżawa. Uzyskaj własne dzierżawy, wykonując czynności opisane w sekcji [Tworzenie dzierżawy usługi Azure AD B2C](active-directory-b2c-get-started.md).

Aplikacja usługi Azure Active Directory B2C można zapisać, wybierając pozycję wysyłania wiadomości protokołu bezpośrednio przy użyciu [OAuth 2.0](active-directory-b2c-reference-protocols.md#oauth2-authorization-code-flow) lub [Otwórz identyfikator połączenia](active-directory-b2c-reference-protocols.md#openid-connect-sign-in-flow), lub korzystania z naszych bibliotek w celu wykonywania pracy. Wybierz pozycję Ulubione platformy w poniższej tabeli i rozpocząć pracę.

[AZURE.INCLUDE [active-directory-b2c-quickstart-table](../../includes/active-directory-b2c-quickstart-table.md)]

## <a name="whats-new"></a>Co nowego

Znaleźć w tym miejscu razy, aby uzyskać informacje o przyszłe zmiany B2C Azure Active Directory. Firma Microsoft będzie również tweet o aktualizacji przy użyciu @AzureAD.

- Informacje dotyczące naszych [Struktura zasad dotyczących extensible](active-directory-b2c-reference-policies.md) i typy zasad, które można tworzyć i używać w aplikacjach.
- Zakładka naszym [blogu usługi](https://blogs.msdn.microsoft.com/azureadb2c/) powiadomień na problemy z usługą pomocnicze, aktualizacji, stanu i czynniki. Nadal monitorować [Stan Azure pulpitu nawigacyjnego](https://azure.microsoft.com/status/) także.
- Bieżący [ograniczenia dotyczące usług, ograniczeń oraz ograniczenia](active-directory-b2c-limitations.md).
- Na koniec [Przykładowy kod](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore-b2c) przy użyciu Azure AD B2C i ASP.NET Core.

## <a name="how-to-articles"></a>Artykuły

Dowiedz się, jak korzystać z określonych funkcji Azure Active Directory B2C:

- Konfigurowanie konta [serwisu Facebook](active-directory-b2c-setup-fb-app.md), [Google +](active-directory-b2c-setup-goog-app.md) [konto Microsoft](active-directory-b2c-setup-msa-app.md), [Amazon](active-directory-b2c-setup-amzn-app.md)i [LinkedIn](active-directory-b2c-setup-li-app.md) do użytku w aplikacjach dla klientów indywidualnych skierowaną.
- [Użyj atrybuty niestandardowe na potrzeby zbierania informacji na temat osób korzystających ze](active-directory-b2c-reference-custom-attr.md).
- [Włącz uwierzytelnianie wieloskładnikowe Azure w aplikacjach dla klientów indywidualnych skierowaną](active-directory-b2c-reference-mfa.md).
- [Konfigurowanie samoobsługowego resetowania hasła dla osób korzystających ze](active-directory-b2c-reference-sspr.md).
- [Dostosowywanie wyglądu i działania konta logowania i innych stron widocznych dla klientów indywidualnych](active-directory-b2c-reference-ui-customization.md) , które są obsługiwane przez Azure Active Directory B2C.
- [Korzystanie z Azure Active Directory wykresu interfejsu API do programowy tworzenie, odczytywanie, aktualizowanie i Usuń konsumentów](active-directory-b2c-devquickstarts-graph-dotnet.md) w dzierżawie usługi Azure Active Directory B2C.

## <a name="next-steps"></a>Następne kroki

Te łącza będą przydatne w przypadku poznawanie usługi szczegóły dotyczące:

- Zobacz [Azure Active Directory B2C cennik informacji](https://azure.microsoft.com/pricing/details/active-directory-b2c/).
- Uzyskaj pomoc na przepełnienie stosu przy użyciu [azure-usługi active directory](http://stackoverflow.com/questions/tagged/azure-active-directory) lub [adal](http://stackoverflow.com/questions/tagged/adal) znaczniki.
- Przekaż nam informacji za pomocą [Głosu użytkownika](https://feedback.azure.com/forums/169401-azure-active-directory/)— chcemy poznać Twoją ich! Użyj frazy "AzureADB2C:" w tytule swój wpis, tak aby było je znaleźć.
- Przejrzyj [Azure AD B2C protokół odwołania](active-directory-b2c-reference-protocols.md).
- Przejrzyj [Azure AD B2C odwołania do tokenu](active-directory-b2c-reference-tokens.md).
- W tym artykule [Azure Active Directory B2C często zadawane pytania](active-directory-b2c-faqs.md).
- [Plik pomocy technicznej żądań Azure Active Directory B2C](active-directory-b2c-support.md).

## <a name="get-security-updates-for-our-products"></a>Uzyskiwanie aktualizacji zabezpieczeń systemu oferowanych produktów

Zachęcamy do otrzymywać powiadomienia o przy odwiedzania [tej strony](https://technet.microsoft.com/security/dd252948) i subskrybowania doradcze alertów zabezpieczeń, gdy występują zdarzeń.
