<properties
    pageTitle="Co się stało z projektu MVC (Visual Studio usługi Azure Active Directory połączenie usługi) | Microsoft Azure "
    description="W tym artykule opisano, co się stanie z projektem MVC podczas łączenia się Azure AD przy użyciu usług programu Visual Studio połączenia"
    services="active-directory"
    documentationCenter="na"
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

# <a name="what-happened-to-my-mvc-project-visual-studio-azure-active-directory-connected-service"></a>Co się stało z projektu MVC (Visual Studio usługi Azure Active Directory połączenie usługi)?

> [AZURE.SELECTOR]
> - [Wprowadzenie](vs-active-directory-dotnet-getting-started.md)
> - [Co się stało](vs-active-directory-dotnet-what-happened.md)



## <a name="references-have-been-added"></a>Dodano odwołania

### <a name="nuget-package-references"></a>Odwołania do pakietu NuGet

- **Microsoft.IdentityModel.Protocol.Extensions**
- **Microsoft.Owin**
- **Microsoft.Owin.Host.SystemWeb**
- **Microsoft.Owin.Security**
- **Microsoft.Owin.Security.Cookies**
- **Microsoft.Owin.Security.OpenIdConnect**
- **Owin**
- **System.IdentityModel.Tokens.Jwt**

### <a name="net-references"></a>Odwołania .NET

- **Microsoft.IdentityModel.Protocol.Extensions**
- **Microsoft.Owin**
- **Microsoft.Owin.Host.SystemWeb**
- **Microsoft.Owin.Security**
- **Microsoft.Owin.Security.Cookies**
- **Microsoft.Owin.Security.OpenIdConnect**
- **Owin**
- **System.IdentityModel**
- **System.IdentityModel.Tokens.Jwt**
- **System.Runtime.Serialization**

## <a name="code-has-been-added"></a>Kod został dodany

### <a name="code-files-were-added-to-your-project"></a>Kod pliki zostały dodane do projektu

Klasę startową uwierzytelniania **App_Start/Startup.Auth.cs** zostało dodane do projektu zawierającego Logika uruchamiania uwierzytelniania Azure AD. Ponadto klasą kontroler Controllers/AccountController.cs został dodany, zawierający metod **SignIn()** i **SignOut()** . Na koniec częściowy widok **Views/Shared/_LoginPartial.cshtml** został dodany, zawierające łącza akcji na rejestrowanie i wyloguj się.

### <a name="startup-code-was-added-to-your-project"></a>Kod uruchamiania został dodany do projektu

Jeśli masz już klasy uruchamiania w projekcie, Metoda **konfiguracji** została zaktualizowana do uwzględnienia połączenia do **ConfigureAuth(app)**. W przeciwnym razie klasę startową został dodany do projektu.

### <a name="your-appconfig-or-webconfig-has-new-configuration-values"></a>Twój app.config lub web.config ma nowe wartości konfiguracji

Dodano następujące pozycje konfiguracji.


    <appSettings>
        <add key="ida:ClientId" value="ClientId from the new Azure AD App" />
        <add key="ida:AADInstance" value="https://login.microsoftonline.com/" />
        <add key="ida:Domain" value="The selected Azure AD Domain" />
        <add key="ida:TenantId" value="The Id of your selected Azure AD Tenant" />
        <add key="ida:PostLogoutRedirectUri" value="Your project start page" />
    </appSettings>

### <a name="an-azure-active-directory-ad-app-was-created"></a>Utworzono aplikację Azure Active Directory (AD)
Aplikacja Azure AD został utworzony w katalogu, w którym wybrane w kreatorze.

##<a name="if-i-checked-disable-individual-user-accounts-authentication-what-additional-changes-were-made-to-my-project"></a>Zaznaczenie tego pola można *wyłączyć uwierzytelnianie poszczególnych kont użytkowników*, jakie dodatkowe zmiany zostały wprowadzone w projekcie?
Odwołania do pakietu NuGet zostały usunięte, a pliki zostały usunięte i kopii zapasowej. W zależności od stanu projektu może być konieczne ręczne usunięcie dodatkowe informacje lub pliki lub zmodyfikuj kod zależnie od potrzeb.

### <a name="nuget-package-references-removed-for-those-present"></a>Odwołania do pakietu NuGet usunięte (odpowiadające Prezentuj)

- **Microsoft.AspNet.Identity.Core**
- **Microsoft.AspNet.Identity.EntityFramework**
- **Microsoft.AspNet.Identity.Owin**

### <a name="code-files-backed-up-and-removed-for-those-present"></a>Pliki kodu kopie zapasowe i usunęli (odpowiadające Prezentuj)

Każdy z następujących plików kopii zapasowej i usunięty z projektu. Pliki kopii zapasowej znajdują się w folderze "Kopia zapasowa" w katalogu głównego katalogu projektu.

- **App_Start\IdentityConfig.CS**
- **Controllers\ManageController.CS**
- **Models\IdentityModels.CS**
- **Models\ManageViewModels.CS**

### <a name="code-files-backed-up-for-those-present"></a>Pliki kodu kopii zapasowej (odpowiadające Prezentuj)

Wykonano następujące plików kopii zapasowej przed zamiany. Pliki kopii zapasowej znajdują się w folderze "Kopia zapasowa" w katalogu głównego katalogu projektu.

- **Startup.CS**
- **App_Start\Startup.auth.CS**
- **Controllers\AccountController.CS**
- **Views\Shared\_LoginPartial.cshtml**

## <a name="if-i-checked-read-directory-data-what-additional-changes-were-made-to-my-project"></a>Zaznaczenie *Odczyt katalogu danych*, jakie dodatkowe zmiany zostały wprowadzone w projekcie?

Dodatkowe informacje zostały dodane.

###<a name="additional-nuget-package-references"></a>Dodatkowe informacje pakietu NuGet

- **EntityFramework**
- **Microsoft.Azure.ActiveDirectory.GraphClient**
- **Microsoft.Data.Edm**
- **Microsoft.Data.OData**
- **Microsoft.Data.Services.Client**
- **Microsoft.IdentityModel.Clients.ActiveDirectory**
- **System.Spatial**

###<a name="additional-net-references"></a>Dodatkowe informacje .NET

- **EntityFramework**
- **EntityFramework.SqlServer**
- **Microsoft.Azure.ActiveDirectory.GraphClient**
- **Microsoft.Data.Edm**
- **Microsoft.Data.OData**
- **Microsoft.Data.Services.Client**
- **Microsoft.IdentityModel.Clients.ActiveDirectory**
- **Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms**
- **System.Spatial**

###<a name="additional-code-files-were-added-to-your-project"></a>Dodatkowe pliki kodu zostały dodane do projektu

Dwa pliki zostały dodane do obsługi pamięci podręcznej tokenu: **Models\ADALTokenCache.cs** i **Models\ApplicationDbContext.cs**.  Dodatkowy kontrolerze i wyświetlanie zostały dodane do ilustrowanie uzyskiwania dostępu do informacji dotyczących profilów użytkowników przy użyciu wykresu Azure interfejsów API.  Te pliki są **Controllers\UserProfileController.cs** i **Views\UserProfile\Index.cshtml**.

###<a name="additional-startup-code-was-added-to-your-project"></a>Dodatkowy kod uruchamiania został dodany do projektu

W pliku **startup.auth.cs** nowy obiekt **OpenIdConnectAuthenticationNotifications** został dodany do **powiadomienia** członkiem **OpenIdConnectAuthenticationOptions**.  To jest włączone odbieranie kod OAuth i exchange go dla token dostępu.

###<a name="additional-changes-were-made-to-your-appconfig-or-webconfig"></a>Dodatkowe zmiany zostały wprowadzone do app.config lub web.config

Dodano następujące pozycje dodatkowa konfiguracja.

    <appSettings>
        <add key="ida:ClientSecret" value="Your Azure AD App's new client secret" />
    </appSettings>

Dodano następujących sekcjach konfiguracji i parametry połączenia.

    <configSections>
        <!-- For more information on Entity Framework configuration, visit http://go.microsoft.com/fwlink/?LinkID=237468 -->
        <section name="entityFramework" type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=6.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" requirePermission="false" />
    </configSections>
    <connectionStrings>
        <add name="DefaultConnection" connectionString="Data Source=(localdb)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\aspnet-[AppName + Generated Id].mdf;Initial Catalog=aspnet-[AppName + Generated Id];Integrated Security=True" providerName="System.Data.SqlClient" />
    </connectionStrings>
    <entityFramework>
        <defaultConnectionFactory type="System.Data.Entity.Infrastructure.LocalDbConnectionFactory, EntityFramework">
          <parameters>
            <parameter value="mssqllocaldb" />
          </parameters>
        </defaultConnectionFactory>
        <providers>
          <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
        </providers>
    </entityFramework>


###<a name="your-azure-active-directory-app-was-updated"></a>Zaktualizowano aplikacji Azure Active Directory
Azure Active Directory aplikacji została zaktualizowana do uwzględnienia uprawnień *Odczyt katalogu danych* i dodatkowe klucz został utworzony, następnie użytego jako *ida: ClientSecret* w pliku **web.config** .

[Dowiedz się więcej na temat usługi Azure Active Directory](https://azure.microsoft.com/services/active-directory/)
