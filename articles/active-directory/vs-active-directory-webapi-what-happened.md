<properties
    pageTitle="Co się stało z projektu WebApi (Visual Studio usługi Azure Active Directory połączenie usługi) | Microsoft Azure "
    description="W tym artykule opisano, co się dzieje z projektem MVC WebApi nawiązać Azure AD przy użyciu programu Visual Studio"
  services="active-directory"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="web"
    ms.tgt_pltfrm="vs-what-happened"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="tarcher"/>

# <a name="what-happened-to-my-webapi-project-visual-studio-azure-active-directory-connected-service"></a>Co się stało z projektu WebApi (Visual Studio usługi Azure Active Directory połączenie usługi)

> [AZURE.SELECTOR]
> - [Wprowadzenie](vs-active-directory-webapi-getting-started.md)
> - [Co się stało](vs-active-directory-webapi-what-happened.md)

##<a name="references-have-been-added"></a>Dodano odwołania

###<a name="nuget-package-references"></a>Odwołania do pakietu NuGet

- `Microsoft.Owin`
- `Microsoft.Owin.Host.SystemWeb`
- `Microsoft.Owin.Security`
- `Microsoft.Owin.Security.ActiveDirectory`
- `Microsoft.Owin.Security.Jwt`
- `Microsoft.Owin.Security.OAuth`
- `Owin`
- `System.IdentityModel.Tokens.Jwt`

###<a name="net-references"></a>Odwołania .NET

- `Microsoft.Owin`
- `Microsoft.Owin.Host.SystemWeb`
- `Microsoft.Owin.Security`
- `Microsoft.Owin.Security.ActiveDirectory`
- `Microsoft.Owin.Security.Jwt`
- `Microsoft.Owin.Security.OAuth`
- `Owin`
- `System.IdentityModel.Tokens.Jwt`

##<a name="code-changes"></a>Zmiany kodu

###<a name="code-files-were-added-to-your-project"></a>Kod pliki zostały dodane do projektu

Klasę startową uwierzytelniania **App_Start/Startup.Auth.cs** zostało dodane do projektu zawierającego Logika uruchamiania uwierzytelniania Azure AD.

###<a name="startup-code-was-added-to-your-project"></a>Kod uruchamiania został dodany do projektu

Jeśli masz już klasę startową w projekcie, Metoda **konfiguracji** została zaktualizowana do uwzględnienia połączenia do `ConfigureAuth(app)`. W przeciwnym razie klasę startową został dodany do projektu.


###<a name="your-appconfig-or-webconfig-file-has-new-configuration-values"></a>Plik app.config lub web.config ma nowe wartości konfiguracji.

Dodano następujące pozycje konfiguracji.
```
    `<appSettings>
            <add key="ida:ClientId" value="ClientId from the new Azure AD App" />
            <add key="ida:Tenant" value="Your selected Azure AD Tenant" />
            <add key="ida:Audience" value="The App ID Uri from the wizard" />
    </appSettings>`
```

###<a name="an-azure-ad-app-was-created"></a>Azure AD aplikacja została utworzona

Aplikacja Azure AD został utworzony w katalogu, w którym wybrane w kreatorze.

[Dowiedz się więcej na temat usługi Azure Active Directory](https://azure.microsoft.com/services/active-directory/)

##<a name="if-i-checked-disable-individual-user-accounts-authentication-what-additional-changes-were-made-to-my-project"></a>Zaznaczenie tego pola można *wyłączyć uwierzytelnianie poszczególnych kont użytkowników*, jakie dodatkowe zmiany zostały wprowadzone w projekcie?
Odwołania do pakietu NuGet zostały usunięte, a pliki zostały usunięte i kopii zapasowej. W zależności od stanu projektu może być konieczne ręczne usunięcie dodatkowe informacje lub pliki lub zmodyfikuj kod zależnie od potrzeb.

###<a name="nuget-package-references-removed-for-those-present"></a>Odwołania do pakietu NuGet usunięte (odpowiadające Prezentuj)

- `Microsoft.AspNet.Identity.Core`
- `Microsoft.AspNet.Identity.EntityFramework`
- `Microsoft.AspNet.Identity.Owin`

###<a name="code-files-backed-up-and-removed-for-those-present"></a>Pliki kodu kopie zapasowe i usunęli (odpowiadające Prezentuj)

Każdy z następujących plików kopii zapasowej i usunięty z projektu. Pliki kopii zapasowej znajdują się w folderze "Kopia zapasowa" w katalogu głównego katalogu projektu.

- `App_Start\IdentityConfig.cs`
- `Controllers\AccountController.cs`
- `Controllers\ManageController.cs`
- `Models\IdentityModels.cs`
- `Providers\ApplicationOAuthProvider.cs`

###<a name="code-files-backed-up-for-those-present"></a>Pliki kodu kopii zapasowej (odpowiadające Prezentuj)

Wykonano następujące plików kopii zapasowej przed zamiany. Pliki kopii zapasowej znajdują się w folderze "Kopia zapasowa" w katalogu głównego katalogu projektu.

- `Startup.cs`
- `App_Start\Startup.Auth.cs`

##<a name="if-i-checked-read-directory-data-what-additional-changes-were-made-to-my-project"></a>Zaznaczenie *Odczyt katalogu danych*, jakie dodatkowe zmiany zostały wprowadzone w projekcie?

###<a name="additional-changes-were-made-to-your-appconfig-or-webconfig"></a>Dodatkowe zmiany zostały wprowadzone do app.config lub web.config

Dodano następujące pozycje dodatkowa konfiguracja.

```
    `<appSettings>
        <add key="ida:Password" value="Your Azure AD App's new password" />
    </appSettings>`
```

###<a name="your-azure-active-directory-app-was-updated"></a>Zaktualizowano aplikacji Azure Active Directory
Azure Active Directory aplikacji została zaktualizowana do uwzględnienia uprawnień *Odczyt katalogu danych* i dodatkowe klucz został utworzony, następnie użytego jako *Ida: hasło* w `web.config` pliku.

[Dowiedz się więcej na temat usługi Azure Active Directory](https://azure.microsoft.com/services/active-directory/)
