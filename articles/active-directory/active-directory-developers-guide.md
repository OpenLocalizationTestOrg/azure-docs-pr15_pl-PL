<properties
   pageTitle="Przewodnik programisty usługi Azure Active Directory | Microsoft Azure"
   description="Ten artykuł zawiera kompleksowy przewodnik Deweloper zorientowane na zasoby dla usługi Azure Active Directory."
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/24/2016"
   ms.author="mbaldwin"/>


# <a name="azure-active-directory-developers-guide"></a>Przewodnik programisty usługi Azure Active Directory

## <a name="overview"></a>Omówienie
Jako sposób zarządzania tożsamością jako platformy usług (IDMaaS) Azure Active Directory (AD) zawiera programista skuteczny sposób integracji Zarządzanie tożsamościami w swoich aplikacjach. Następujące artykuły umożliwiają omówienia wdrażania i kluczowe funkcje Azure AD. Sugerujemy czytania w kolejności lub przejść do [wprowadzenie](#getting-started) , jeśli możesz już przystąpić do Drąż.


1. [Zalety integracji z usługą Azure AD](./develop/active-directory-how-to-integrate.md): Poznawanie, dlaczego Integracja z usługą Azure Active Directory oferuje najlepsze rozwiązania do bezpiecznego logowania i autoryzacji.

1. [Scenariusze dotyczące uwierzytelniania Azure AD](active-directory-authentication-scenarios.md): korzystać z uproszczonej uwierzytelniania w Azure AD o podanie logowania jednokrotnego w aplikacji.

1. [Integracja aplikacji z usługą Azure Active Directory](active-directory-integrating-applications.md): Dowiedz się, jak dodawanie, aktualizowanie i usuwanie aplikacji z usługi Azure Active Directory i informacji o znakowaniu wytyczne dotyczące zintegrowane aplikacje.

1. [Interfejs API Azure AD wykresu](active-directory-graph-api.md): programowy dostęp Azure AD za pośrednictwem interfejsu API usługi REST punkty końcowe za pomocą interfejsu API Azure AD wykresu. Interfejs API Azure AD wykres jest również dostępne za pośrednictwem [Programu Microsoft Graph](https://graph.microsoft.io/). Program Microsoft Graph zapewnia ujednolicony interfejs API, który umożliwia dostęp do wielu Microsoft usługi w chmurze interfejsów API, za pośrednictwem jeden punkt końcowy interfejsu API usługi REST i przy użyciu tokenu pojedynczy dostęp.

1. [Azure AD uwierzytelniania bibliotek](active-directory-authentication-libraries.md): łatwe uwierzytelniania użytkowników w celu uzyskania tokeny dostępu za pomocą bibliotek uwierzytelniania Azure AD dla środowiska .NET, JavaScript, celem-C, Android i innych elementów.


## <a name="getting-started"></a>Wprowadzenie

Samouczki są dostosowane do wielu platform i pomoże Ci szybko rozpocząć tworzenie oprogramowania z usługi Azure Active Directory. Jako wstępny musisz [uzyskać dzierżawę usługi Azure Active Directory](active-directory-howto-tenant.md).

### <a name="mobile-and-pc-application-quick-start-guides"></a>Przewodniki Szybki start aplikacji komórkowego i komputera

|[![iOS](./media/active-directory-developers-guide/ios.png)](active-directory-devquickstarts-ios.md)|[![Android](./media/active-directory-developers-guide/android.png)](active-directory-devquickstarts-android.md)|[![.NET](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-dotnet.md)|[![Uniwersalny systemu Windows](./media/active-directory-developers-guide/windows.png)](active-directory-devquickstarts-windowsstore.md)|[![Xamarin](./media/active-directory-developers-guide/xamarin.png)](active-directory-devquickstarts-xamarin.md)|[![Cordova](./media/active-directory-developers-guide/cordova.png)](active-directory-devquickstarts-cordova.md)|[![OAuth 2.0](./media/active-directory-developers-guide/oauth-2.png)](active-directory-protocols-oauth-code.md)
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
|[iOS](active-directory-devquickstarts-ios.md)|[Android](active-directory-devquickstarts-android.md)|[.NET](active-directory-devquickstarts-dotnet.md)|[Uniwersalny systemu Windows](active-directory-devquickstarts-windowsstore.md)|[Xamarin](active-directory-devquickstarts-xamarin.md)|[Cordova](active-directory-devquickstarts-cordova.md)|[Integracja bezpośrednio z OAuth 2.0](active-directory-protocols-oauth-code.md)|

### <a name="web-application-quick-start-guides"></a>Przewodniki Szybki start aplikacji sieci Web

|[![.NET](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-webapp-dotnet.md)|[![Java](./media/active-directory-developers-guide/java.png)](active-directory-devquickstarts-webapp-java.md)|[![AngularJS](./media/active-directory-developers-guide/angularjs.png)](active-directory-devquickstarts-angular.md)|[![Języka JavaScript](./media/active-directory-developers-guide/javascript.png)](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi)|[![Node.js](./media/active-directory-developers-guide/nodejs.png)](active-directory-devquickstarts-openidconnect-nodejs.md) | [![Łączenie OpenID](./media/active-directory-developers-guide/openid-connect.png)](active-directory-protocols-openid-connect-code.md)
|:--:|:--:|:--:|:--:|:--:|:--:|
|[.NET](active-directory-devquickstarts-webapp-dotnet.md)|[Java](active-directory-devquickstarts-webapp-java.md)|[AngularJS](active-directory-devquickstarts-angular.md)|[Języka JavaScript](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi)|[Node.js](active-directory-devquickstarts-openidconnect-nodejs.md)|[Integracja bezpośrednio z OpenID nawiązywanie połączenia](active-directory-protocols-openid-connect-code.md)|

### <a name="web-api-quick-start-guides"></a>Przewodniki Szybki start interfejsu API sieci Web

|[![.NET](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-webapi-dotnet.md)|[![Node.js](./media/active-directory-developers-guide/nodejs.png)](active-directory-devquickstarts-webapi-nodejs.md)
|:--:|:--:|
|[.NET](active-directory-devquickstarts-webapi-dotnet.md)|[Node.js](active-directory-devquickstarts-webapi-nodejs.md)

### <a name="querying-the-directory-quickstart-guide"></a>Kwerenda katalogu Przewodnik Szybki Start

| [![.NET](./media/active-directory-developers-guide/graph.png)](active-directory-graph-api-quickstart.md)|
|:--:|
|[Interfejs API wykresu](active-directory-graph-api-quickstart.md)|

## <a name="how-tos"></a>Kwestie

Te artykuły opisano sposoby wykonywania określonych zadań za pomocą usługi Azure Active Directory:

- [Uzyskiwanie dzierżawy usługi Azure AD](active-directory-howto-tenant.md)
- [Zaloguj się każdy użytkownik Azure AD przy użyciu deseń wielu dzierżawy aplikacji](active-directory-devhowto-multi-tenant-overview.md)
- Włączanie rejestracji Jednokrotnej krzyżowe aplikacji przy użyciu ADAL, [Android](active-directory-sso-android.md) i urządzeń z [systemem iOS](active-directory-sso-ios.md)
- [Ustawianie jako aplikacji certyfikowane AppSource do Azure AD](active-directory-devhowto-appsource-certified.md)
- [Lista aplikacji w galerii aplikacji Azure AD](active-directory-app-gallery-listing.md)
- [Przesyłanie aplikacji sieci web dla usługi Office 365 do pulpitu nawigacyjnego ze sprzedawcą](https://msdn.microsoft.com/office/office365/howto/submit-web-apps-seller-dashboard)
- [Opis manifest aplikacji usługi Azure Active Directory](active-directory-application-manifest.md)
- [Opis znakowania wytyczne dotyczące przyciski nabycia logowania i aplikacji w aplikacji klienckiej](active-directory-branding-guidelines.md)
- [W wersji Preview: Sposób tworzenia aplikacje, które Zaloguj się przy użyciu konta zarówno osobiste i służbowe](active-directory-appmodel-v2-overview.md)
- [W wersji Preview: Sposób tworzenia aplikacji, które tworzą konta i zaloguj się konsumentów](../active-directory-b2c/active-directory-b2c-overview.md)
- [Podgląd: Konfigurowanie token istnienia w Azure AD](active-directory-configurable-token-lifetimes.md) przy użyciu programu PowerShell. Zobacz [operacje zasad](https://msdn.microsoft.com/library/azure/ad/graph/api/policy-operations) i [obiektu zasad](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#policy-entity) , aby uzyskać szczegółowe informacje na temat konfigurowania przy użyciu interfejsu API Azure AD wykresu.

## <a name="reference"></a>Odwołania

Te artykuły Podaj odwołanie foundation dla pozostałych biblioteki uwierzytelniania interfejsy API, protokoły, błędy, przykłady kodu i punkty końcowe.  

###  <a name="support"></a>Pomoc techniczna
- [Oznakowane pytania](http://stackoverflow.com/questions/tagged/azure-active-directory): znajdowanie usługi Azure Active Directory rozwiązań przepełnienie stosu, wyszukując znaczniki [azure-usługi active directory](http://stackoverflow.com/questions/tagged/azure-active-directory) i [adal](http://stackoverflow.com/questions/tagged/adal).
- Zobacz [Słownik Deweloper Azure AD](active-directory-dev-glossary.md) definicje często używanych terminów związanych z opracowywania aplikacji i integracji.

### <a name="code"></a>Kod

- [Usługi Azure Active Directory Otwórz źródło bibliotek](http://github.com/AzureAD): Najłatwiejszym sposobem znalezienia źródła biblioteki jest przy użyciu naszej [listy biblioteki](active-directory-authentication-libraries.md).

- [Przykłady usługi Azure Active Directory](https://github.com/azure-samples?query=active-directory): Najprostszym sposobem nawigowania w liście próbki jest za pomocą [indeksu przykłady kodu](active-directory-code-samples.md).

- [Active Directory uwierzytelniania biblioteki (ADAL) dla środowiska .NET](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet) — dokumentacji jest dostępna zarówno w [najnowszej wersji głównych](https://docs.microsoft.com/active-directory/adal/microsoft.identitymodel.clients.activedirectory) , jak i w [poprzedniej wersji głównych](https://docs.microsoft.com/active-directory/adal/v2/microsoft.identitymodel.clients.activedirectory).

### <a name="graph-api"></a>Interfejs API wykresu

- [Odwołanie interfejsu API wykresu](https://msdn.microsoft.com/library/azure/hh974476.aspx): pozostałe odwołania w przypadku Azure Active Directory wykresu interfejsu API. [Wyświetlanie interakcyjne środowiska odwołanie interfejsu API wykresu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

- [Interfejs API wykresu zakresów uprawnień](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes): OAuth 2.0 zakresów uprawnień, które są używane do kontrolowania dostępu aplikacji do danych katalogów w dzierżawie.

### <a name="authentication-and-authorization-protocols"></a>Protokoły uwierzytelniania i autoryzacji

- [Podpisywanie przerzucania klucza w Azure AD](active-directory-signing-key-rollover.md): Dowiedz się więcej o Azure AD podpisywania cadence przerzucania klucza i aktualizowania klucz dla najbardziej typowych scenariuszy aplikacji.

- [OAuth 2.0 Protocol (protokół): za pomocą udzielanie kod autoryzacji](active-directory-protocols-oauth-code.md): za pomocą protokołu OAuth 2.0 autoryzacji kod Udziel, aby autoryzować dostęp do aplikacji sieci Web oraz interfejsy API sieci Web w usłudze Active Directory platformy Azure dzierżawa.

- [OAuth 2.0 Protocol (protokół): opis niejawne Udziel](active-directory-dev-understanding-oauth2-implicit-grant.md): Dowiedz się więcej o udzielanie niejawne autoryzacji i czy jest odpowiedni dla aplikacji.

- [OAuth 2.0 Protocol (protokół): Usługa połączenia przy użyciu klienta poświadczenia](active-directory-protocols-oauth-service-to-service.md): udzielanie poświadczeń klienta 2.0 OAuth pozwala usługi sieci web (poufne klient) za pomocą własnej poświadczeń uwierzytelniania podczas nawiązywania połączeń z innej usługi sieci web, zamiast personifikacji użytkownika. W tym scenariuszu klient jest zazwyczaj usługi sieci web warstwy środkowej, demon usługi lub witryny sieci Web.

- [Protokół 1.0 łączenie OpenID: logowania i uwierzytelniania](active-directory-protocols-openid-connect-code.md): protokół 1.0 łączenie OpenID rozszerza OAuth 2.0 do użytku protokół uwierzytelniania. Aplikacji klienckiej, można otrzymywać id_token Zarządzanie procesem logowania lub uzupełnić przepływ kod autoryzacji do odbierania zarówno id_token, jak i autoryzacji kodu.

- [Odwołanie do protokołu SAML 2.0](active-directory-saml-protocol-reference.md): protokołu SAML 2.0 umożliwia aplikacjom zapewnić pojedynczy obsługi logowania jednokrotnego użytkownikom.

- [Protokół 1.2 federacyjnych](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html): usługi Azure Active Directory obsługuje 1.2 federacyjnych zgodnie z sieci Web usług federacyjnych wersji 1.2 specyfikacja. Aby uzyskać więcej informacji o dokumencie metadanych Federacji zobacz [Metadanych Federacji](active-directory-federation-metadata.md).

- [Obsługiwane typy token i oświadczeń](active-directory-token-and-claims.md): umożliwia tego przewodnika i ocenić oświadczeń w tokeny SAML 2.0 i JSON tokeny sieci Web (JWT).

## <a name="videos"></a>Klipy wideo

### <a name="build"></a>Tworzenie

Te prezentacje na opracowywania aplikacji przy użyciu usługi Azure Active Directory funkcji głośniki, którzy pracują bezpośrednio w zespół inżynierów. Prezentacje obejmuje podstawowe tematy, w tym IDMaaS, uwierzytelniania federacji tożsamości i logowania jednokrotnego.

- [Tożsamość firmy Microsoft: Stan Unii i przyszłych kierunków](https://azure.microsoft.com/documentation/videos/build-2016-microsoft-identity-state-of-the-union-and-future-direction/)
- [Azure Active Directory: Zarządzanie tożsamościami jako usługa nowoczesny aplikacji](https://azure.microsoft.com/documentation/videos/build-2015-azure-active-directory-identity-management-as-a-service-for-modern-applications/)
- [Można opracowywać aplikacje sieci web nowoczesny z usługą Azure Active Directory](https://azure.microsoft.com/documentation/videos/build-2015-develop-modern-web-applications-with-azure-active-directory/)
- [Można opracowywać Nowoczesna macierzyste aplikacje z usługą Azure Active Directory](https://azure.microsoft.com/documentation/videos/build-2015-develop-modern-native-applications-with-azure-active-directory/)

### <a name="azure-friday"></a>Azure piątek
[Azure piątek](https://azure.microsoft.com/documentation/videos/azure-friday/) jest cyklicznych piątek serii wideo 1:1, która jest przeznaczona do wprowadzenia możesz krótki (10-15 minut) wywiady z ekspertów na różnych tematów Azure.  Funkcja filtru usług na stronie, aby wyświetlić wszystkie usługi Azure Active Directory klipy wideo.

- [Azure tożsamości 101](https://azure.microsoft.com/documentation/videos/azure-identity-basics/)
- [Azure tożsamości 102](https://azure.microsoft.com/documentation/videos/azure-identity-creating-active-directory/)
- [Azure tożsamości 103](https://azure.microsoft.com/documentation/videos/azure-identity-application-to-authenticate/)

## <a name="social"></a>Społecznościowych

- [Zespół usługi Active Directory blogu](http://blogs.technet.com/b/ad/): najnowszych osiągnięć w świecie usługi Azure Active Directory.

- [Zespół wykresu Azure Active Directory blogu](http://blogs.msdn.com/b/aadgraphteam): usługi Azure Active Directory informacje, które są specyficzne dla interfejsu API wykresu.

- [Tożsamość chmury](http://www.cloudidentity.net): pomysły na zarządzanie tożsamościami jako usługa, z kapitału Azure Active Directory PM.  

- [Usługi Azure Active Directory w serwisie Twitter](https://twitter.com/azuread): Anonsy usługi Azure Active Directory w 140 znaków.

## <a name="windows-server-on-premises-development"></a>Rozwoju lokalnego serwera systemu Windows
Aby uzyskać wskazówki na temat korzystania z rozwoju Windows Server i Active Directory Federation Services (ADFS) zobacz:

- [AD FS scenariusze dla deweloperów](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/ad-fs-scenarios-for-developers): omówienie składniki usług AD FS i sposobu jej działania, zawierająca szczegóły dotyczące scenariuszy obsługiwanego uwierzytelniania i autoryzacji.
- [Instruktaży usług AD FS](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/ad-fs-development): lista artykułów samouczek, które zapewniają instrukcje krok po kroku dotyczące implementowania przepływów powiązanych uwierzytelniania i autoryzacji.
