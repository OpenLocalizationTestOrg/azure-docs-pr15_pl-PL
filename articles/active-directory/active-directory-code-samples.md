<properties
   pageTitle="Przykłady kodu Azure Active Directory | Microsoft Azure"
   description="Indeks usługi Azure Active Directory przykłady kodu, zorganizowane według scenariusza."
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="priyamohanram"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin"/>

# <a name="azure-active-directory-code-samples"></a>Przykłady kodu Azure Active Directory

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Microsoft Azure Active Directory (Azure AD) umożliwia dodawanie uwierzytelniania i autoryzacji do aplikacji sieci web oraz interfejsy API sieci web. W tej sekcji łączy do przykłady kodu, pokazujące, jak to zrobić i fragmenty kodu, które są dostępne w aplikacjach. Na stronie próbnej kod znajdziesz szczegółowe odczytu-mi tematy, które pomagają w wymagania dotyczące instalacji i konfiguracji. I kod jest ujęta ułatwiające zrozumienie sekcji krytycznych.

Aby poznać podstawowe scenariusz dla każdego typu próbki, zobacz scenariusze uwierzytelniania dla Azure AD.

Współtworzenie naszych próbki GitHub: [Przykłady firmy Microsoft Azure Active Directory i dokumentacji](https://github.com/Azure-Samples?page=3&query=active-directory).

## <a name="web-browser-to-web-application"></a>Przeglądarki sieci Web do aplikacji sieci Web

Tych przykładach pokazano, jak napisać aplikacji sieci web, który kieruje użytkownika przeglądarki, aby się zalogować do Azure AD.



| Język platformy | Przykładowe | Opis
| ----------------- | ------ | -----------
| C#-.NET | [Aplikacja sieci Web — OpenIDConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) | Za pomocą funkcji OpenID nawiązywanie połączenia (OWIN łączenie OpenID ASP.Net pośredniczącym) służą do uwierzytelniania użytkowników z dzierżawy usługi Azure AD.
| C#-.NET | [Aplikacja sieci Web — MultiTenant-OpenIdConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-multitenant-openidconnect) | Wiele dzierżawy .NET MVC aplikacji web korzystającej OpenID nawiązywanie połączenia (OWIN łączenie OpenID ASP.Net pośredniczącym) do uwierzytelniania użytkowników z kilka dzierżaw Azure AD.
| C#-.NET | [Aplikacja sieci Web — WSFederation-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-wsfederation) | Za pomocą federacyjnych (OWIN federacyjnych ASP.Net pośredniczącym) do uwierzytelniania użytkowników z dzierżawy Azure AD.






## <a name="single-page-application-spa"></a>Pojedyncza strona aplikacji (SPA)

W tym przykładzie pokazano, jak napisać aplikację pojedynczej strony zabezpieczone z usługą Azure Active Directory.  

| Język platformy | Przykładowe | Opis
| ----------------- | ------ | -----------
| JavaScript, C#-.NET | [SinglePageApp DotNet](https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp) | Aby zabezpieczyć aplikację oparte na AngularJS pojedynczej strony z punktu końcowego wstecz interfejsu API sieci web programu ASP.NET za pomocą ADAL dla języka JavaScript i Azure AD.


## <a name="native-application-to-web-api"></a>Natywnej aplikacji do interfejsu API sieci Web

Te przykłady kodu przedstawiono sposób tworzenia aplikacji klientami, wywołujących web interfejsów API, które są zabezpieczone przez Azure AD. Korzystają z [Biblioteki uwierzytelniania usługi Azure AD (ADAL)](active-directory-authentication-libraries.md) i [OAuth 2.0 w Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx).

| Język platformy | Przykładowe | Opis
| ----------------- | ------ | -----------
| Języka JavaScript | [NativeClient-MultiTarget-Cordova](https://github.com/Azure-Samples/active-directory-cordova-multitarget) | Tworzenie aplikacji Apache Cordova połączeń interfejsu API sieci web, która używa Azure AD dla uwierzytelniania za pomocą wtyczki ADAL dla Apache Cordova.
| C#-.NET | [NativeClient DotNet](https://github.com/Azure-Samples/active-directory-dotnet-native-desktop) | Aplikacja .NET WPF, która wymaga interfejsu API sieci web, która jest zabezpieczona przy użyciu Azure AD.
| C#-.NET | [NativeClient WindowsStore](https://github.com/Azure-Samples/active-directory-dotnet-windows-store) | Aplikacji ze Sklepu Windows, która wymaga interfejsu API sieci web, która jest zabezpieczona z usługą Azure Active Directory.
| C#-.NET | [NativeClient-WebAPI-MultiTenant-WindowsStore](https://github.com/Azure-Samples/active-directory-dotnet-webapi-multitenant-windows-store) | Aplikacja ze Sklepu Windows, dzwoniąc dzierżawy wielu elementów interfejsu API sieci web zabezpieczone z usługą Azure Active Directory.
| C#-.NET | [WebAPI-OnBehalfOf-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof) | Aplikacja klientami, która wymaga interfejsu API, który pobiera tokenu do działania w imieniu użytkownika oryginalnego, a następnie używa tokenu do połączeń innej interfejsu API sieci web w sieci web.
| C#-.NET | [NativeClient WindowsPhone8.1](https://github.com/Azure-Samples/active-directory-dotnet-windowsphone-8.1) | Aplikację ze Sklepu Windows dla Windows Phone 8.1, wywołującego interfejsu API sieci web jest zabezpieczone Azure AD.
| ObjC | [NativeClient iOS](https://github.com/Azure-Samples/active-directory-ios) | Aplikacja iOS, która wymaga interfejsu API sieci web, która wymaga Azure AD dla uwierzytelniania.
| C#-.NET | [WebAPI-ManuallyValidateJwt-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapi-manual-jwt-validation) | Aplikacja klientami, która zawiera logikę przetwarzania tokenu JWT w sieci web API, zamiast pośredniczącym OWIN.
| C#-Xamarin | [NativeClient Xamarin Android](https://github.com/Azure-Samples/active-directory-xamarin-android) | Powiązanie Xamarin do natywnej Azure AD uwierzytelniania biblioteki (ADAL) w bibliotece Android.
| C#-Xamarin | [NativeClient — Xamarin — iOS](https://github.com/Azure-Samples/active-directory-xamarin-ios) | Powiązanie Xamarin do natywnej Azure AD uwierzytelniania biblioteki (ADAL) dla systemu iOS.
| C#-Xamarin | [NativeClient-MultiTarget-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-native-multitarget) | Projekt Xamarin, który jest przeznaczony dla pięciu platformy i połączeń interfejsu API sieci web, która jest zabezpieczone Azure AD.
| C#-.NET | [NativeClient bezobsługowe DotNet](https://github.com/Azure-Samples/active-directory-dotnet-native-headless) | Natywnej aplikacji, która wykonuje-interactive uwierzytelniania i połączeń interfejsu API sieci web, która jest zabezpieczone Azure AD.



## <a name="web-application-to-web-api"></a>Aplikacja sieci Web do interfejsu API sieci Web

Te przykłady kodu Pokaż jak używać [OAuth 2.0 w Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx) tworzenie aplikacji sieci web połączenia z siecią web interfejsów API, które są zabezpieczone przez Azure AD.

| Język platformy | Przykładowe | Opis
| ----------------- | ------ | -----------
| C#-.NET | [Aplikacja sieci Web — WebAPI-OpenIDConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-openidconnect) | Zadzwoń do witryny sieci web interfejsu API o zalogować uprawnienia użytkownika.
|  C#-.NET | [Aplikacja sieci Web — WebAPI-uwierzytelnianie OAuth2-AppIdentity-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-appidentity) | Zadzwoń do interfejsu API sieci web z uprawnieniami aplikacji.
| C#-.NET | [Aplikacja sieci Web — WebAPI-uwierzytelnianie OAuth2-tożsamości użytkownika — Dotnet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity) | Dodaj autoryzacji [OAuth 2.0 w Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx) do istniejącej aplikacji sieci web w celu nawiązania połączenia interfejsu API sieci web.
| Języka JavaScript | [WebAPI Nodejs](https://github.com/Azure-Samples/active-directory-node-webapi) | Konfigurowanie usługi interfejsu API usługi REST, która jest zintegrowany z usługą Azure Active Directory interfejsu API ochrony. Zawiera serwer Node.js o interfejs API sieci Web.
| C#-.NET | [Aplikacja sieci Web — WebAPI-MultiTenant-OpenIdConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-multitenant-openidconnect) |  Wiele dzierżawy MVC sieci web aplikacja używająca OpenID nawiązywanie połączenia (OWIN łączenie OpenID ASP.Net pośredniczącym) do uwierzytelniania użytkowników z dzierżawy Azure AD. Kod autoryzacji używany do wywołania interfejsu API wykresu.

## <a name="server-or-daemon-application-to-web-api"></a>Serwer lub aplikacją demon interfejsu API sieci Web

Te przykłady kodu przedstawiono sposób tworzenia demon lub serwera aplikacji, która pobiera zasoby z interfejsu API sieci web przy użyciu [Biblioteki uwierzytelniania usługi Azure AD (ADAL)](active-directory-authentication-libraries.md) i [OAuth 2.0 w Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx).

| Język platformy | Przykładowe | Opis
| ----------------- | ------ | -----------
| C#-.NET | [Demon DotNet](https://github.com/Azure-Samples/active-directory-dotnet-daemon) | Aplikacja konsoli wywołuje interfejsu API sieci web. Poświadczenie klienta jest hasłem.
| C#-.NET | [Demon CertificateCredential-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential) | Aplikacja konsoli, która wymaga interfejsu API sieci web. Poświadczenie klienta jest certyfikat.


## <a name="calling-azure-ad-graph-api"></a>Wywoływanie interfejsu API Azure AD wykresu

Poniższe przykładowe kodu pokazano, jak tworzyć aplikacje, które wywołują odczytywanie i zapisywanie danych katalogu interfejsu API Azure AD wykresu.

| Język platformy | Przykładowe | Opis
| ----------------- | ------ | -----------
| Java | [Aplikacja sieci Web GraphAPI-Java](https://github.com/Azure-Samples/active-directory-java-graphapi-web) | Aplikacja sieci web, w korzystającego z interfejsu API wykresu dostępu do danych katalogu Azure AD.
| PHP | [Aplikacja sieci Web — GraphAPI-PHP](https://github.com/Azure-Samples/active-directory-php-graphapi-web) | Aplikacja sieci web, w korzystającego z interfejsu API wykresu dostępu do danych katalogu Azure AD.
| C#-.NET | [Aplikacja sieci Web — GraphAPI-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-web) | Aplikacja sieci web, w korzystającego z interfejsu API wykresu dostępu do danych katalogu Azure AD.
| C#-.NET | [ConsoleApp-GraphAPI-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-console) | Ta aplikacja konsoli zaprezentowano typowe odczytu i zapisu wywołania API wykresu i pokazano, jak wykonać przypisywanie licencji użytkownika i zaktualizować miniaturę fotografii i łączy użytkownika.
| C#-.NET | [ConsoleApp-GraphAPI-DiffQuery-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-diffquery) | Aplikacji konsoli używanym przez kwerendę różnicy w interfejsie API wykresu celu uzyskania okresowych zmian w obiektach użytkowników w dzierżawie usługi Azure AD.
| C#-.NET | [Aplikacja sieci Web — GraphAPI-DirectoryExtensions-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-directoryextensions-web) | Aplikacja MVC używa interfejsu API wykresu kwerend do generowania firmy prosty schemat organizacyjny.
| PHP | [Aplikacja sieci Web — GraphAPI-DirectoryExtensions-PHP](https://github.com/Azure-Samples/active-directory-php-graphapi-directoryextensions-web) | Aplikacja PHP, która wymaga interfejsu API wykresu, aby zarejestrować rozszerzenia, a następnie odczytywanie, aktualizowanie i usuwanie wartości atrybutu rozszerzenia.


## <a name="authorization"></a>Autoryzacja

Te przykłady kodu przedstawiono sposoby używania Azure AD o zezwolenie.

| Język platformy | Przykładowe | Opis
| ----------------- | ------ | -----------
| C#-.NET | [Aplikacja sieci Web — GroupClaims-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-groupclaims) | Wykonywanie kontroli dostępu oparte na roli (RBAC) w aplikacji, która jest zintegrowany z Azure AD przy użyciu oświadczeń grupy usługi Azure Active Directory.
| C#-.NET | [Aplikacja sieci Web — RoleClaims-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-roleclaims) | Wykonywanie kontroli dostępu oparte na roli (RBAC) w aplikacji, która jest zintegrowany z Azure AD przy użyciu ról aplikacji usługi Azure Active Directory.


## <a name="legacy-walkthroughs"></a>Starsze instruktaży

Te instruktaży korzystanie z technologii nieco starsze, ale nadal mogą być przydatne.

| Język platformy | Przykładowe | Opis
| ----------------- | ------ | -----------
| C#-.NET | [Oparta na rolach i oparte na ACL autoryzacji w aplikacji Microsoft Azure AD](http://go.microsoft.com/fwlink/?LinkId=331694) | Wykonywanie autoryzacji oparta na rolach (RBAC) oraz autoryzacji oparty na ACL w aplikacji, która jest zintegrowany z usługą Azure Active Directory.
| C#-.NET |  [Uwierzytelnianie AAL — aplikacji ze Sklepu Windows do usługi REST-](http://go.microsoft.com/fwlink/?LinkId=330605) |  Umożliwia dodanie możliwości uwierzytelnianie użytkownika do aplikacji ze Sklepu Windows [Azure AD uwierzytelniania biblioteki (ADAL)](active-directory-authentication-libraries.md) (dawniej AAL) dla ze Sklepu Windows w wersji Beta.
| C#-.NET | [Uwierzytelnianie ADAL - natywnej aplikacji do usługi REST — za pomocą AAD za pośrednictwem okna dialogowego przeglądarki](http://go.microsoft.com/fwlink/?LinkId=259814) |  Dodawanie funkcji Uwierzytelnianie użytkownika do klienta WPF za pomocą [Biblioteki uwierzytelniania usługi Azure AD (ADAL)](active-directory-authentication-libraries.md) .
| C#-.NET | [Uwierzytelnianie ADAL - natywnej aplikacji do usługi REST — za pomocą ACS za pośrednictwem okna dialogowego przeglądarki](http://code.msdn.microsoft.com/AAL-Native-App-to-REST-de57f2cc) |  Dodawanie funkcji Uwierzytelnianie użytkownika do klienta WPF przy użyciu [Biblioteki uwierzytelniania usługi Azure AD (ADAL)](active-directory-authentication-libraries.md) i [Access 2.0 Control Service (ACS)](http://msdn.microsoft.com/library/azure/hh147631.aspx) .
| C#-.NET | [ADAL - uwierzytelniania do serwera](http://go.microsoft.com/fwlink/?LinkId=259816) | [Azure AD uwierzytelniania biblioteki (ADAL)](active-directory-authentication-libraries.md) umożliwia bezpieczne połączenia usługi od procesu po stronie serwera do usługi REST interfejsu API sieci Web MVC4.
| C#-.NET | [Dodawanie logowania w aplikacji sieci Web przy użyciu Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) | Konfigurowanie aplikacji .NET w celu wykonywania web rejestracji jednokrotnej przed katalogu organizacji Azure AD.
| C#-.NET | [Tworzenie aplikacji sieci Web wielu dzierżawy z Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-webapp-multitenant-openidconnect) | Umożliwia dodanie do logowania jednokrotnego i katalogu możliwości uzyskania dostępu do działania większej liczby organizacji jednej aplikacji .NET Azure AD.
JAVA | [Aplikacja przykładowe Java Azure AD wykresu interfejsu API](http://go.microsoft.com/fwlink/?LinkId=263969) | Za pomocą interfejsu API wykresu dostęp do katalogu danych z usługi Azure Active Directory.
PHP | [Aplikacja przykładowe PHP Azure AD wykresu interfejsu API](http://code.msdn.microsoft.com/PHP-Sample-App-For-Windows-228c6ddb) | Za pomocą interfejsu API wykresu dostęp do katalogu danych z usługi Azure Active Directory.
| C#-.NET | [Przykładowe aplikacji Azure AD wykresu interfejsu API](http://go.microsoft.com/fwlink/?LinkID=262648) | Za pomocą interfejsu API wykresu dostęp do katalogu danych z usługi Azure Active Directory.
| C#-.NET | [Przykładowe aplikacji dla kwerendy różnicy Azure AD wykresu](http://go.microsoft.com/fwlink/?LinkId=275398) | Użyj kwerendy różnicy w interfejsie API wykresu, aby uzyskać okresowych zmian do obiektów użytkowników w dzierżawie usługi Azure AD.
| C#-.NET | [Aplikacja próbki dla integracji chmury wielu dzierżawy stosowania Azure AD](http://go.microsoft.com/fwlink/?LinkId=275397) | Integracja aplikacji wielu dzierżawy w usłudze Azure Active Directory.
| C#-.NET | [Zabezpieczanie aplikacji ze Sklepu Windows i usługi sieci Web REST przy użyciu Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-windows-store) | Tworzenie zasobu prostego interfejsu API sieci web i aplikacji ze Sklepu Windows przy użyciu Azure AD i [Biblioteki uwierzytelniania usługi Azure AD (ADAL)](active-directory-authentication-libraries.md).
| C#-.NET| [Kwerendy Azure AD przy użyciu interfejsu API wykresu](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-web) | Konfigurowanie aplikacji programu Microsoft .NET, aby uzyskać dostęp do danych z katalogu dzierżawy Azure AD za pomocą interfejsu API Azure AD wykresu.

## <a name="see-also"></a>Zobacz też

##### <a name="other-resources"></a>Inne zasoby

[Przewodnik programisty Azure Active Directory](active-directory-developers-guide.md)

[Azure AD wykresu interfejsu API koncepcyjny i odwołań](https://msdn.microsoft.com/library/azure/hh974476.aspx)

[Biblioteka Pomoc interfejsu API Azure AD wykresu](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient)

