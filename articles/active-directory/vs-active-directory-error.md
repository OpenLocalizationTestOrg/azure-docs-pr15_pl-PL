<properties 
    pageTitle="Błąd podczas wykrywania uwierzytelniania" 
    description="Kreator połączenia usługi active directory wykrył typ uwierzytelniania niezgodny" 
    services="active-directory" 
    documentationCenter="" 
    authors="TomArcher" 
    manager="douge" 
    editor=""/>
  
<tags 
    ms.service="active-directory" 
    ms.workload="web" 
    ms.tgt_pltfrm="vs-getting-started" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="tarcher"/>

# <a name="error-during-authentication-detection"></a>Błąd podczas wykrywania uwierzytelniania

Podczas wykrywania poprzedniego kod uwierzytelniania, Kreator wykrył typ uwierzytelniania niezgodny.   

##<a name="what-is-being-checked"></a>Co to jest zaznaczone pole wyboru?

**Uwaga:** Aby można było poprawnie wykryć poprzedniego kod uwierzytelniania w projekcie, można utworzyć projektu.  Jeśli nie masz poprzedniego kod uwierzytelniania w projekcie wystąpił ten błąd, odbudowanie i spróbuj ponownie.

###<a name="project-types"></a>Tworzenie niestandardowego typu projektu

Kreator sprawdza, jakiego typu projektu projektujesz, aby go dodanie logiki uwierzytelniania prawo do projektu.  Jeśli występuje dowolny kontroler, który pochodzi od `ApiController` projektu w programie project, jest uważany w projekcie WebAPI.  Jeśli istnieje tylko kontrolery utworzone na podstawie `MVC.Controller` projektu w programie project, jest uważany w projekcie MVC.  Pozostałej zawartości nie jest obsługiwane przez kreatora.  Projekty formularzy sieci Web nie są obecnie obsługiwane.

###<a name="compatible-authentication-code"></a>Kod zgodnego uwierzytelniania

Kreator sprawdza również ustawienia uwierzytelniania, które zostały wcześniej skonfigurowane przy użyciu kreatora lub są zgodne z kreatora.  Jeśli wszystkie ustawienia są obecne, jest traktowany jako wielobieżnej sprawy, a Kreator otworzyć i wyświetlić ustawienia.  Jeśli tylko niektóre ustawienia są obecne, jest traktowany jako sprawy błędu.

W programie project MVC Kreator sprawdza dowolną z następujących ustawień wynikające z poprzedniego użycia kreatora:

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:AADInstance" value="" />
    <add key="ida:PostLogoutRedirectUri" value="" />

Ponadto Kreator sprawdza dowolne z następujących ustawień w projekcie interfejs API sieci Web, wynikające z poprzedniego użycia kreatora:

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:Audience" value="" />

###<a name="incompatible-authentication-code"></a>Kod niezgodny uwierzytelniania

Na koniec Kreator próbuje wykryć wersji kodu uwierzytelniania skonfigurowane we wcześniejszych wersjach programu Visual Studio. Jeśli otrzymasz ten błąd, oznacza to, że projektu zawiera typ uwierzytelniania niezgodny. Kreator wykryje następujących typów uwierzytelniania z wcześniejszych wersji programu Visual Studio:

* Uwierzytelnianie systemu Windows 
* Poszczególnych kont użytkowników 
* Konta organizacji 
 

Wykrywanie uwierzytelniania systemu Windows w projekcie MVC, kreator wyszuka `authentication` elementów z pliku **web.config** .

<pre>
    &lt;Konfiguracja&gt;
        &lt;system.web&gt;
            <span style="background-color: yellow">&lt;tryb uwierzytelniania = "Windows"-&gt;</span>
        &lt;/system.web&gt;
    &lt;/Configuration&gt;
</pre>

Wykrywanie uwierzytelniania systemu Windows w projekcie interfejs API sieci Web, kreator wyszuka `IISExpressWindowsAuthentication` element z pliku **.csproj** projektu:

<pre>
    &lt;Projekt&gt;
&lt;PropertyGroup&gt;
            <span style="background-color: yellow">&lt;IISExpressWindowsAuthentication&gt;włączone&lt;/IISExpressWindowsAuthentication&gt;</span>
        &lt;/PropertyGroup > &lt;/projektu        &gt;
</pre>

Wykrywanie uwierzytelniania poszczególnych kont użytkowników, Kreator wyszukuje elementu pakietu z pliku **Packages.config** .

<pre>
    &lt;pakiety&gt;
        <span style="background-color: yellow">&lt;pakietu id="Microsoft.AspNet.Identity.EntityFramework" wersji = "2.1.0" targetFramework = "net45"-&gt;</span>
    &lt;/pakietów&gt;
</pre>

Wykrywanie stare formy uwierzytelniania konta organizacji, Kreator wyszukuje następujący element z **web.config**:

<pre>
    &lt;Konfiguracja&gt;
        &lt;appSettings&gt;
            <span style="background-color: yellow">&lt;Dodaj klucz = wartość "ida: obszaru" = "***"-&gt;</span>
        &lt;/appSettings&gt;
    &lt;/Configuration&gt;
</pre>

Aby zmienić typ uwierzytelniania, Usuń typ uwierzytelniania niezgodny i ponownie uruchom kreatora.

Aby uzyskać więcej informacji zobacz [Uwierzytelnianie scenariusze Azure AD](active-directory-authentication-scenarios.md).