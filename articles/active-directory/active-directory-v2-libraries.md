<properties
   pageTitle="Azure Active Directory 2.0 uwierzytelniania bibliotek | Microsoft Azure"
   description="Bibliotek zgodne klienta i serwera pośredniczącym biblioteki i powiązanych biblioteki, źródła i łącza próbki dla punktu końcowego usługi Azure Active Directory 2.0."
   services="active-directory"
   documentationCenter=""
   authors="skwan"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/30/2016"
   ms.author="skwan;bryanla"/>


# <a name="azure-active-directory-v20-authentication-libraries"></a>Azure Active Directory 2.0 uwierzytelniania bibliotek
Punkt końcowy 2.0 usługi Azure Active Directory (Azure AD) obsługuje protokoły OAuth 2.0 i OpenID łączenie 1.0 standardowym. Za pomocą różnych bibliotek firmy Microsoft i innych organizacjach z punktem końcowym 2.0.

Podczas tworzenia aplikacji, która używa punkt końcowy 2.0, zaleca się użycie bibliotek, które są zapisywane przez ekspertów domeny protokół obserwujących metodologii cyklu opracowywania zabezpieczeń (SDL), na przykład [język, a następnie przez firmę Microsoft][Microsoft-SDL]. Jeśli zdecydujesz się na kod strony pomocy technicznej dla protokołów, zalecamy wykonaj metodologii SDL i zwrócić szczególną uwagę do zagadnienia dotyczące zabezpieczeń w specyfikacji standardów dla każdego protokołu.

## <a name="types-of-libraries"></a>Typy bibliotek

Azure AD 2.0 współpracuje dwa typy bibliotek:

- **Bibliotek klienta**. Natywne klienci i serwery Użyj bibliotek klienta, aby uzyskać tokeny dostępu do nawiązywania połączeń z zasobu, takiego jak Microsoft Graph.
- **Bibliotek pośredniczącym serwera**. Aplikacje sieci Web za pomocą bibliotek pośredniczącym serwera dla logowania użytkownika. Interfejsy API sieci Web za pomocą bibliotek pośredniczącym serwera do sprawdzania poprawności tokenów, które są wysyłane przez klientów natywnych lub inny serwer.

## <a name="library-support"></a>Biblioteka techniczna
Ponieważ dowolnej ze standardami biblioteki można wybrać, gdy używasz punkt końcowy 2.0, należy wiedzieć, gdzie szukać pomocy technicznej. Problemy i żądania funkcji w kodzie biblioteki skontaktuj się z właścicielem biblioteki. Problemy i żądań funkcji w implementacji protokołu usługi stronie kontakt z firmą Microsoft.

Biblioteki są dostępne w dwóch kategorii pomocy technicznej:

- **Obsługiwane przez firmę Microsoft**. Firma Microsoft udostępnia rozwiązania dla tych bibliotek i przeprowadzonych SDL ukończenia starannością na tych bibliotek.
- **Zgodne**. Firma Microsoft testowanych te biblioteki w podstawowe scenariusze i potwierdzić, że działają z punktem końcowym 2.0. Microsoft nie umożliwia rozwiązania tych bibliotek i nie wystąpiło Przegląd tych bibliotek. Problemy i funkcji powinny być kierowane do biblioteki open source projektu.

Aby uzyskać listę bibliotek, które współpracują z punktem końcowym 2.0 Zobacz następnej sekcji w tym artykule.

## <a name="microsoft-supported-client-libraries"></a>Bibliotek obsługiwane przez firmę Microsoft klienta
| Platformy| Nazwa biblioteki| Plik do pobrania | Kod źródłowy | Przykładowe |
| :-: | :-: | :-: | :-: | :-: |
| Xamarin .NET Sklepu Windows | Uwierzytelnianie za pomocą Microsoft biblioteki (MSAL) dla środowiska .NET | [Microsoft.Identity.Client (NuGet)][ClientLib-NET-Lib] | [MSAL dla środowiska .NET (GitHub)][ClientLib-NET-Repo] | [Przykładowe klientami pulpitu systemu Windows][ClientLib-NET-Sample] |
| Node.js | Wtyczka Passport.js usługi Active Directory platformy Microsoft Azure | [Passport Azure AD (npm)][ClientLib-Node-Lib] | [Passport Azure AD (GitHub)][ClientLib-Node-Repo] | Wkrótce |

<!--- COMMENTING OUT UNTIL THEY ARE READY
| iOS, Mac | Microsoft Authentication Library (MSAL) for ObjC | In development | In development | In development |
| Android | Microsoft Authentication Library (MSAL) for Android | In development | In development | In development |
| JavaScript | Microsoft Authentication Library (MSAL) for JavaScript | In development | In development | In development |
 -->

## <a name="microsoft-supported-server-middleware-libraries"></a>Obsługiwane przez firmę Microsoft server pośredniczącym bibliotek
| Platformy| Nazwa biblioteki| Plik do pobrania | Kod źródłowy | Przykładowe |
| :-: | :-: | :-: | :-: | :-: |
| .NET 4.x | OWIN OpenID łączenie pośredniczącym programu ASP.NET | [Microsoft.Owin.Security.OpenIdConnect (NuGet)][ServerLib-Net4-Owin-Oidc-Lib] | [Katana projektu (CodePlex)][ServerLib-Net4-Owin-Oidc-Repo] | [Przykładowe aplikacji sieci Web][ServerLib-Net4-Owin-Oidc-Sample] |
| .NET 4.x | OWIN pośredniczącym okaziciela OAuth dla programu ASP.NET | [Microsoft.Owin.Security.OAuth (NuGet)][ServerLib-Net4-Owin-Oauth-Lib] | [Katana projektu (CodePlex)][ServerLib-Net4-Owin-Oauth-Repo] | [Przykładowy interfejsu API sieci Web][ServerLib-Net4-Owin-Oauth-Sample] |
| Podstawowe .NET | OWIN OpenID połączyć pośredniczącym .NET Core | [Microsoft.AspNetCore.Authentication.OpenIdConnect (NuGet)][ServerLib-NetCore-Owin-Oidc-Lib] | [Zabezpieczenia programu ASP.NET (GitHub)][ServerLib-NetCore-Owin-Oidc-Repo] | [Przykładowe aplikacji sieci Web][ServerLib-NetCore-Owin-Oidc-Sample] |
| Podstawowe .NET | OWIN pośredniczącym okaziciela OAuth dla .NET Core | [Microsoft.AspNetCore.Authentication.OAuth (NuGet)][ServerLib-NetCore-Owin-Oauth-Lib] | [Zabezpieczenia programu ASP.NET (GitHub)][ServerLib-NetCore-Owin-Oauth-Repo] | Wkrótce |
| Node.js | Microsoft Azure Active Directory Passport.js wtyczki | [Passport Azure AD (npm)][ServerLib-Node-Lib] | [Passport Azure AD (GitHub)][ServerLib-Node-Repo] | [Przykładowe aplikacji sieci Web][ServerLib-Node-Sample] |
<!--- COMMENTING UNTIL SAMPLE IS AVAILABLE
| .NET 4.x, .NET Core | JSON Web Token Handler for .NET | [System.IdentityModel.Tokens.Jwt (NuGet)][ServerLib-Net-Jwt-Lib] | [Azure AD identity model extensions for .NET (GitHub)][ServerLib-Net-Jwt-Repo] | Coming soon |
--->
## <a name="compatible-client-libraries"></a>Bibliotek zgodne klienta
| Platformy| Nazwa biblioteki | Sprawdzania wersji | Kod źródłowy | Przykładowe |
| :-: | :-: | :-: | :-: | :-: |
| Android | [OIDCAndroidLib](https://github.com/kalemontes/OIDCAndroidLib/wiki) | 0.2.1 | [OIDCAndroidLib](https://github.com/kalemontes/OIDCAndroidLib) | [Przykładowe natywnej aplikacji](active-directory-v2-devquickstarts-android.md) |
| iOS | [NXOAuth2Client](https://github.com/nxtbgthng/OAuth2Client) | 1.2.8 | [NXOAuth2Client](https://github.com/nxtbgthng/OAuth2Client) | [Przykładowe natywnej aplikacji](active-directory-v2-devquickstarts-ios.md)|
| Języka JavaScript | [Hello.js](https://adodson.com/hello.js/) | pytanie 1.13.5 | [Hello.js](https://github.com/MrSwitch/hello.js) | Wkrótce |
| Kolby Python | [Kolby OAuthlib](https://github.com/lepture/flask-oauthlib) | 0.9.3 | [Kolby OAuthlib](https://github.com/lepture/flask-oauthlib) | Wkrótce |
| Dopiskiem | [OmniAuth](https://github.com/omniauth/omniauth/wiki) | omniauth:1.3.1</br>omniauth-oauth2:1.4.0 | [OmniAuth](https://github.com/omniauth/omniauth)</br>[Uwierzytelnianie OmniAuth OAuth2](https://github.com/intridea/omniauth-oauth2) | Wkrótce |
<!--- REMOVING BRANDON'S FOR NOW
|  |  |  |  |  |
| Android | [OAuth2 Client](https://github.com/wuman/android-oauth-client) |   | [OAuth2 Client](https://github.com/wuman/android-oauth-client)  | Coming soon  |
| Java | [WSO2 Identity Server](https://docs.wso2.com/display/IS500/Introducing+the+Identity+Server) | [Version 5.2.0](http://wso2.com/products/identity-server/) | [Source](https://docs.wso2.com/display/IS500/Building+from+Source) | [Samples index](https://docs.wso2.com/display/IS500/Samples)  |
| Java | [Java Gluu Server](https://gluu.org/docs/) |   | [oxAuth](https://github.com/GluuFederation/oxAuth)  | Coming soon |
| Node.js | [NPM passport-openidconnect](https://www.npmjs.com/package/passport-openidconnect) | 0.0.1  | [Passport-OpenID Connect](https://github.com/jaredhanson/passport-openidconnect) | Coming soon  |
| PHP | [OpenID Connect Basic Client](https://github.com/jumbojett/OpenID-Connect-PHP) |   | [OpenID Connect Basic Client](https://github.com/jumbojett/OpenID-Connect-PHP)  | Coming soon  |
-->

## <a name="compatible-server-middleware-libraries"></a>Bibliotek pośredniczącym zgodne serwera
Wkrótce

## <a name="related-content"></a>Zawartość pokrewna
Aby uzyskać więcej informacji na temat punktu końcowego 2.0 Azure AD, zobacz [Omówienie 2.0 modelu aplikacji Azure AD][AAD-App-Model-V2-Overview].

Aby Pomóż nam uściślić i kształtowanie naszych zawartości, użyj Disqus funkcja komentarzy na końcu tego artykułu na wyrażanie opinii.

<!--Image references-->

<!--Reference style links -->
[AAD-App-Model-V2-Overview]: active-directory-appmodel-v2-overview.md
[ClientLib-NET-Lib]: http://www.nuget.org/packages/Microsoft.Identity.Client
[ClientLib-NET-Repo]: https://github.com/AzureAD/microsoft-authentication-library-for-dotnet
[ClientLib-NET-Sample]: active-directory-v2-devquickstarts-wpf.md
[ClientLib-Node-Lib]: https://www.npmjs.com/package/passport-azure-ad
[ClientLib-Node-Repo]: https://github.com/AzureAD/passport-azure-ad
[ClientLib-Node-Sample]:
[ClientLib-Iosmac-Lib]:
[ClientLib-Iosmac-Repo]:
[ClientLib-Iosmac-Sample]:
[ClientLib-Android-Lib]:
[ClientLib-Android-Repo]:
[ClientLib-Android-Sample]:
[ClientLib-Js-Lib]:
[ClientLib-Js-Repo]:
[ClientLib-Js-Sample]:
[Microsoft-SDL]: http://www.microsoft.com/sdl/default.aspx
[ServerLib-Net4-Owin-Oidc-Lib]: https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/
[ServerLib-Net4-Owin-Oidc-Repo]: http://katanaproject.codeplex.com/
[ServerLib-Net4-Owin-Oidc-Sample]: active-directory-v2-devquickstarts-dotnet-web.md
[ServerLib-Net4-Owin-Oauth-Lib]: https://www.nuget.org/packages/Microsoft.Owin.Security.OAuth/
[ServerLib-Net4-Owin-Oauth-Repo]: http://katanaproject.codeplex.com/
[ServerLib-Net4-Owin-Oauth-Sample]: https://azure.microsoft.com/en-us/documentation/articles/active-directory-v2-devquickstarts-dotnet-api/
[ServerLib-Net-Jwt-Lib]: https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt
[ServerLib-Net-Jwt-Repo]: https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet
[ServerLib-Net-Jwt-Sample]:/
[ServerLib-NetCore-Owin-Oidc-Lib]: https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.OpenIdConnect/
[ServerLib-NetCore-Owin-Oidc-Repo]: https://github.com/aspnet/Security
[ServerLib-NetCore-Owin-Oidc-Sample]: https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore-v2
[ServerLib-NetCore-Owin-Oauth-Lib]: https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.OAuth/
[ServerLib-NetCore-Owin-Oauth-Repo]: https://github.com/aspnet/Security
[ServerLib-NetCore-Owin-Oauth-Sample]:/
[ServerLib-Node-Lib]: https://www.npmjs.com/package/passport-azure-ad
[ServerLib-Node-Repo]: https://github.com/AzureAD/passport-azure-ad/
[ServerLib-Node-Sample]: https://azure.microsoft.com/en-us/documentation/articles/active-directory-v2-devquickstarts-node-web/
