<properties
   pageTitle="Biblioteki uwierzytelniania usługi Azure Active Directory | Microsoft Azure"
   description="Biblioteki uwierzytelniania usługi Azure AD (ADAL) umożliwia klienta deweloperów aplikacji łatwo uwierzytelniania użytkowników w usłudze w chmurze lub lokalnej usługi Active Directory (AD) i uzyskać tokeny dostępu do zabezpieczania interfejsu API."
   services="active-directory"
   documentationCenter=""
   authors="bryanla"
   manager="mbaldwin"
   editor="mbaldwin" />
<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/11/2016"
   ms.author="mbaldwin" />

# <a name="azure-active-directory-authentication-libraries"></a>Biblioteki uwierzytelniania usługi Azure Active Directory

Uwierzytelnianie Azure AD biblioteki (ADAL) projektanci aplikacji klienckich łatwo uwierzytelniania użytkowników w usłudze w chmurze lub lokalnej usługi Active Directory (AD), a następnie uzyskać tokeny dostępu do zabezpieczania interfejsu API. ADAL oferuje wiele funkcji uwierzytelniania tworzącej łatwiejsze dla deweloperów, takie jak asynchroniczne pomocy technicznej, można konfigurować token pamięci podręcznej, które są przechowywane tokeny dostępu i Odśwież tokenów, automatyczne odświeżanie token po wygaśnięciu token dostępu i tokenu odświeżania jest dostępna i nie tylko. Obsługa większość złożoność ADAL można zawęzić Deweloper reguł biznesowych w ich stosowania i łatwo zabezpieczanie zasobów bez ekspertem w zakresie zabezpieczeń.

ADAL jest dostępna na różnych platformach.

|Platformy|Nazwa pakietu|Klient/serwer|Plik do pobrania|Kod źródłowy|Dokumentacja i przykłady|
|---|---|---|---|---|---|
|.NET klienta ze Sklepu Windows, UWP Xamarin iOS i Android|Biblioteki uwierzytelniania usługi Active Directory (ADAL) .NET v3 |Klienta|[Microsoft.IdentityModel.Clients.ActiveDirectory (NuGet)](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory)|[ADAL dla środowiska .NET (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet)|[Dokumentacja](https://docs.microsoft.com/active-directory/adal/microsoft.identitymodel.clients.activedirectory)|
|.NET klienta, ze Sklepu Windows, Windows Phone 8.1 |Biblioteki uwierzytelniania usługi Active Directory (ADAL) dla .NET w wersji 2 |Klienta|[Microsoft.IdentityModel.Clients.ActiveDirectory (NuGet)](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/2.28.2)|[ADAL dla środowiska .NET (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/releases/tag/v2.28.2)|[Dokumentacja](https://docs.microsoft.com/active-directory/adal/v2/microsoft.identitymodel.clients.activedirectory)|
|Języka JavaScript|Biblioteki uwierzytelniania usługi Active Directory (ADAL) dla języka JavaScript|Klienta|[ADAL dla języka JavaScript (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-js)|[ADAL dla języka JavaScript (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-js)|Przykład: [SinglePageApp-DotNet (Github)](https://github.com/AzureADSamples/SinglePageApp-DotNet)|
|IOS OS X|Biblioteki uwierzytelniania usługi Active Directory (ADAL) celem C|Klienta|[ADAL celem-c (CocoaPods)](http://cocoadocs.org/docsets/ADAL/)|[ADAL celem-c (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-objc)|Przykład: [NativeClient systemu iOS (Github)](https://github.com/AzureADSamples/NativeClient-iOS)|
|Android|Biblioteki uwierzytelniania usługi Active Directory (ADAL) dla systemu Android|Klienta|[ADAL dla systemu Android (centralnym repozytorium)](http://search.maven.org/remotecontent?filepath=com/microsoft/aad/adal/)|[ADAL dla systemu Android (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-android)|Przykład: [NativeClient Android (Github)](https://github.com/AzureADSamples/NativeClient-Android)|
|Node.js|Biblioteki uwierzytelniania usługi Active Directory (ADAL) Node.js|Klienta|[ADAL dla Node.js (npm)](https://www.npmjs.com/package/adal-node)|[ADAL dla Node.js (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-nodejs)|Przykład: [WebAPI-Nodejs (Github)](https://github.com/AzureADSamples/WebAPI-Nodejs)|
|Node.js|Usługi Microsoft Azure Active Directory Passport pośredniczącym uwierzytelniania dla węzła|Klienta|[Azure Active paszportu katalogu dla Node.js (npm)](https://www.npmjs.com/package/passport-azure-ad)|[Azure Active Directory dla Node.js (Github)](https://github.com/AzureAD/passport-azure-ad)||
|Java|Biblioteki uwierzytelniania usługi Active Directory (ADAL) dla języka Java|Klienta|[ADAL dla języka Java (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-java)|[ADAL dla języka Java (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-java)||
|.NET|Rozszerzenia protokołu tożsamości dla programu Microsoft .NET Framework 4,5|Serwer|[Microsoft.IdentityModel.Protocol.Extensions (NuGet)](https://www.nuget.org/packages/Microsoft.IdentityModel.Protocol.Extensions)|[Azure AD tożsamości model rozszerzenia .NET (Github)](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet)||
|.NET|Obsługa Token Web JSON dla programu Microsoft .net Framework 4,5|Serwer|[System.IdentityModel.Tokens.Jwt (NuGet)](https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt)|[Azure AD tożsamości modelu rozszerzenia .NET (Github)](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet)||
|.NET|Oprogramowanie pośrednie OWIN, który umożliwia korzystanie z technologii firmy Microsoft dla uwierzytelniania aplikacji.|Serwer|[Microsoft.Owin.Security.ActiveDirectory (NuGet)](https://www.nuget.org/packages/Microsoft.Owin.Security.ActiveDirectory/)|[OWIN (CodePlex)](http://katanaproject.codeplex.com)||
|.NET|Oprogramowanie pośrednie OWIN, które umożliwia aplikacji OpenIDConnect za pomocą uwierzytelniania.|Serwer|[Microsoft.Owin.Security.OpenIdConnect (NuGet)](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect)|[OWIN (CodePlex)](http://katanaproject.codeplex.com)|Przykład: [Aplikacja sieci Web OpenIDConnecty DotNet (Github)](https://github.com/AzureADSamples/WebApp-OpenIDConnect-DotNet)|
|.NET|Oprogramowanie pośrednie OWIN, które umożliwia aplikacji federacyjnych za pomocą uwierzytelniania.|Serwer|[Microsoft.Owin.Security.WsFederation (NuGet)](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation)|[OWIN (CodePlex)](http://katanaproject.codeplex.com)|Przykład: [Aplikacja sieci Web WSFederation DotNet (Github)](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet)|

## <a name="scenarios"></a>Scenariusze

Oto trzy typowe scenariusze, w których ADAL może być używany do uwierzytelniania.  

### <a name="authenticating-users-of-a-client-application-to-a-remote-resource"></a>Uwierzytelnianie użytkowników aplikacji klienckiej do zdalnego zasobu

W tym scenariuszu Deweloper ma klienta, takich jak aplikacji WPF, który ma dostęp do zdalnego zasobu zabezpieczonego przez Azure AD, takie jak interfejsu API sieci web. On ma Azure subskrypcję, wie, jak wywołania interfejs API niższego sieci web i zna dzierżawy Azure AD, która korzysta z interfejsu API sieci web. W wyniku on umożliwia ADAL ułatwienia uwierzytelnianie za pomocą Azure AD, przez pełni Delegowanie obsługi uwierzytelniania do ADAL lub jawnie obsługi poświadczeń użytkownika. ADAL umożliwia łatwe do uwierzytelnienia użytkownika, uzyskaj od Azure AD token dostępu i token Odśwież, a następnie użyj token dostępu nawiązać żądania do interfejsu API sieci web.

Przykładowy kod demonstrujący w tym scenariuszu przy użyciu funkcji uwierzytelniania do Azure AD zobacz [Natywnej aplikacji WPF klienta interfejsu API sieci Web](https://github.com/azureadsamples/nativeclient-dotnet).

### <a name="authenticating-a-server-application-to-a-remote-resource"></a>Uwierzytelniania aplikacji serwera do zdalnego zasobu

W tym scenariuszu Deweloper ma aplikacji na serwerze, który ma dostęp do zdalnego zasobu zabezpieczonego przez Azure AD, takie jak interfejsu API sieci web. On ma Azure subskrypcję, wie, jak należy wywoływanie usługi podrzędne i wie, że korzysta z dzierżawy Azure AD interfejs API sieci web. W wyniku on Użyj ADAL w celu ułatwienia uwierzytelnianie za pomocą Azure AD przez jawnie obsługi aplikacji poświadczeń. ADAL umożliwia łatwe do pobrania tokenu z Azure AD przy użyciu poświadczeń klienta aplikacji, a następnie wprowadź za pomocą token żądania do interfejsu API sieci web. ADAL obsługuje również zarządzanie ważności token dostępu, buforowanie go i odnawianie je w razie potrzeby. Przykładowy kod demonstrujący w tym scenariuszu zobacz [Aplikacji konsoli API sieci Web](https://github.com/AzureADSamples/Daemon-DotNet).

### <a name="authenticating-a-server-application-on-behalf-of-a-user-to-access-a-remote-resource"></a>Uwierzytelniania aplikacji serwera w imieniu użytkownika dostępu zdalnego zasobu

W tym scenariuszu Deweloper ma aplikacji na serwerze, który ma dostęp do zdalnego zasobu zabezpieczonego przez Azure AD, takie jak interfejsu API sieci web. Żądanie musi być wykonane w imieniu użytkownika w Azure AD. On ma Azure subskrypcję, wie, jak wywołać podrzędne interfejsu API sieci web i zna Azure AD dzierżawa używane przez usługę. Po uwierzytelnieniu użytkownika do aplikacji sieci web aplikacja może uzyskać kod autoryzacji dla użytkownika z usługi Azure Active Directory. Aby uzyskać token dostępu i odświeżyć token w imieniu użytkownika przy użyciu poświadczeń klienta i Kod autoryzacji skojarzone z aplikacją z Azure AD aplikacji sieci web można użyć ADAL. Gdy aplikacja sieci web jest posiada token dostępu, można zadzwonić interfejsu API sieci web, do momentu wygaśnięcia tokenu. Po wygaśnięciu tokenu aplikacji sieci web służy ADAL, aby uzyskać nowy token dostępu za pomocą odświeżanie token otrzymanymi poprzednio.


## <a name="see-also"></a>Zobacz też

[Przewodnik projektanta usługi Azure Active Directory](active-directory-developers-guide.md)

[Scenariusze dotyczące uwierzytelniania dla usługi Azure Active directory](active-directory-authentication-scenarios.md)

[Przykłady kodu usługi Azure Active Directory](active-directory-code-samples.md)
