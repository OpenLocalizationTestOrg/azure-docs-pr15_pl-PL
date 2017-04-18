<properties
    pageTitle="Azure AD Xamarin wprowadzenie | Microsoft Azure"
    description="Sposoby tworzenia aplikacji Xamarin, w której można zintegrować z usługą Azure Active Directory do zalogowania się i połączeń Azure AD chronionych za pomocą OAuth interfejsów API."
    services="active-directory"
    documentationCenter="xamarin"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-xamarin"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>


# <a name="integrate-azure-ad-into-a-xamarin-app"></a>Integrowanie Azure AD w aplikacji Xamarin

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Xamarin pozwala na pisanie aplikacji dla urządzeń przenośnych w C#, które można uruchamiać na iOS, Android i Windows (urządzeń przenośnych i komputerów). Jeśli tworzysz aplikację za pomocą Xamarin Azure AD ułatwia niezwykle proste do uwierzytelnienia użytkownika przy użyciu swoich kont usługi Active Directory. Umożliwia także aplikacji do bezpiecznego korzystania z dowolnego interfejsu API chroniony przez Azure AD, takich jak interfejsów API usługi Office 365 lub interfejsu API Azure w sieci web.

W przypadku aplikacji Xamarin, które potrzebują dostępu do chronionych zasobów Azure AD udostępnia Active Directory Authentication Library lub ADAL. Osoby ADAL wyłącznie w celu w życiu jest ułatwiają aplikacji uzyskanie tokeny dostępu. Wykazać się, jak łatwo jest, w tym miejscu będzie budowy aplikację "Katalogu można" który:

-   Działa na iOS, Android komputerze z systemem Windows, Windows Phone i ze Sklepu Windows.
- Używa biblioteki jedną klasę przenośnych (PCL) do uwierzytelniania użytkowników i uzyskać tokenów dla interfejsu API Azure AD wykres
-   Wyszukiwanie w katalogu dla użytkowników z danej głównej nazwy użytkownika.

Aby utworzyć pełną aplikację roboczych, musisz:

2. Konfigurowanie środowiska programowania Xamarin
2. Zarejestruj się w aplikacji z usługą Azure Active Directory.
3. Instalowanie i konfigurowanie ADAL.
5. Umożliwia uzyskanie tokeny Azure AD ADAL.

Aby rozpocząć korzystanie, [Pobierz projektu szkielet](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/skeleton.zip) lub [Pobierz ukończonej przykładowej](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip). Każdy jest rozwiązania programu Visual Studio 2013. Konieczne będzie również dzierżawę Azure AD, w którym można tworzyć użytkowników i rejestrowania aplikacji. Jeśli nie masz jeszcze dzierżawy, [Dowiedz się, jak kupić](active-directory-howto-tenant.md).

## <a name="0-set-up-your-xamarin-development-environment"></a>*0. Konfigurowanie środowiska programowania Xamarin*
Ponieważ ten samouczek zawiera projektów dla systemu iOS, Android i systemu Windows, musisz Visual Studio i Xamarin ze sobą. Aby utworzyć potrzeby środowiska, wykonaj pełne instrukcje dotyczące [konfiguracji i instalowanie programu Visual Studio i Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) w witrynie MSDN. Te instrukcje obejmować materiałów, których możesz przeglądać dowiedzieć się więcej o Xamarin gdy czekasz dla programów instalacyjnych do wykonania. 

Po zakończeniu instalacji konieczne, otwórz rozwiązanie w programie Visual Studio, aby rozpocząć pracę. Znajdziesz sześć projektów: pięć projektów specyficzne dla platformy i jeden Biblioteka klas przenośne, które będą udostępniane na wszystkich platformach`DirectorySearcher.cs`

## <a name="1-register-the-directory-searcher-application"></a>*1. katalog aplikacji można zarejestrować*
Aby włączyć aplikację uzyskać tokenów, najpierw musisz go zarejestrować w dzierżawie usługi Azure AD i udzielać jej uprawnień dostępu do interfejsu API Azure AD wykresu:

-   Zaloguj się do [portalu zarządzania Azure](https://manage.windowsazure.com)
-   W nawigacji po lewej kliknij w **Usłudze Active Directory**
-   Wybierz pozycję dzierżawy, w której chcesz zarejestrować tę aplikację.
-   Kliknij kartę **aplikacje** , a następnie kliknij przycisk **Dodaj** w szufladzie dołu.
-   Postępuj zgodnie z monitami i Utwórz nową **Natywnej aplikacji**.
    -   **Nazwa** aplikacji opisując aplikacji dla użytkowników końcowych
    -   **Przekierowywanie Uri** to połączenie schemat i ciąg używające Azure AD zwraca token odpowiedzi. Wprowadź wartość, np. `http://DirectorySearcher`.
-   Po zakończeniu rejestracji AAD przypisze aplikacji unikatowego identyfikatora klienckiego. Musisz tę wartość w następnej sekcji, aby skopiować go na karcie **Konfigurowanie** .
- Ponadto na karcie **Konfigurowanie** Odszukaj sekcję "Uprawnienia do innych aplikacji". Na podstawie "Usługi Azure Active Directory" Dodaj uprawnień **dostępu i katalogu organizacji** w obszarze **Delegowane uprawnienia**. Spowoduje to włączenie aplikacji kwerendy API wykres dla użytkowników.

## <a name="2-install--configure-adal"></a>*2. Zainstaluj i skonfiguruj ADAL*
Teraz, gdy masz aplikację w Azure AD, można zainstalować ADAL i wpisz swój kod dotyczące tożsamości. Aby ADAL mieć możliwość komunikowania się z usługą Azure Active Directory musisz dostarczyć pewne informacje na temat aplikacji rejestracji.
-   Rozpocznij, dodając ADAL do wszystkich projektów w nim przy użyciu konsoli Menedżera pakietów.

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirectorySearcherLib
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Android
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Desktop
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-iOS
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Universal
`

- Należy zauważyć, że dwóch odwołań biblioteki są dodawane do każdego projektu — PCL części ADAL i części specyficzny dla platformy.

-   W programie project DirectorySearcherLib, otwórz `DirectorySearcher.cs`. Zmienianie wartości elementów członkowskich zajęć, aby odzwierciedlała wartości wprowadzane w Azure Portal. Kod będzie odwoływać się do tych wartości przy każdym użyciu ADAL.
    -   `tenant` Jest domeną Twojej dzierżawy Azure AD contoso.onmicrosoft.com np.
    -   `clientId` Jest clientId aplikacji skopiowany z portalu.
    - `returnUri` Jest redirectUri wprowadzone w portalu, np. `http://DirectorySearcher`.

## <a name="3--use-adal-to-get-tokens-from-aad"></a>*3. Używanie ADAL, aby uzyskać tokenów z AAD*
*Almost* wszystkie logiki uwierzytelniania aplikacji znajduje się w `DirectorySearcher.SearchByAlias(...)`. Jest niezbędne w projektach specyficzne dla platformy przekazanie kontekstowe parametr `DirectorySearcher` PCL.

- Najpierw otwórz `DirectorySearcher.cs` i dodać nowy parametr `SearchByAlias(...)` metody. `IPlatformParameters`jest to kontekstowe parametr, która obejmuje obiekty specyficzne dla platformy, które ADAL wymaga uwierzytelniania.

```C#
public static async Task<List<User>> SearchByAlias(string alias, IPlatformParameters parent)
{
```

-   Następnie zainicjować `AuthenticationContext` -ADAL jest podstawowy zajęć. Jest to miejsce, w którym należy przekazać ADAL współrzędne niezbędne do komunikowania się z usługą Azure Active Directory. Następnie zadzwoń `AcquireTokenAsync(...)`, która przyjmuje `IPlatformParameters` obiektu i wywoła przepływ uwierzytelniania należy zwrócić token do aplikacji.

```C#
...
    AuthenticationResult authResult = null;
    try
    {
        AuthenticationContext authContext = new AuthenticationContext(authority);
        authResult = await authContext.AcquireTokenAsync(graphResourceUri, clientId, returnUri, parent);
    }
    catch (Exception ee)
    {
        results.Add(new User { error = ee.Message });
        return results;
    }
...
```
- `AcquireTokenAsync(...)`najpierw spróbuje zwracana tokenu do żądanego zasobu (API wykres w tym przypadku) bez monitowania użytkownika o podanie poświadczeń (za pośrednictwem pamięci podręcznej lub odświeżanie stare tokeny). Tylko wtedy, gdy jest to konieczne, będzie widoczny użytkownika logowania Azure AD na stronie przed podczas uzyskiwania wymagane token.


- Następnie można dołączyć token dostępu na żądanie API wykres w nagłówku autoryzacji:

```C#
...
    request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", authResult.AccessToken);
...
```

To wszystko dla `DirectorySearcher` PCL i aplikacji jest związany z tożsamości kodu.  Czy nadal jest połączenie `SearchByAlias(...)` metody w każdej z tych platform widoki, i gdzie konieczne, dodać kod błędnej obsługi cyklu życia interfejsu użytkownika.

####<a name="android"></a>Android:
- W `MainActivity.cs`, numer telefonu, aby dodać `SearchByAlias(...)` na przycisku kliknij obsługi:

```C#
List<User> results = await DirectorySearcher.SearchByAlias(searchTermText.Text, new PlatformParameters(this));
```
- Należy zastąpić `OnActivityResult` cyklu życia metoda przekazywania uwierzytelniania przekierowuje powrót do odpowiedniej metody.  ADAL zapewnia metodę Pomocnik to w systemie Android:

```C#
...
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
    base.OnActivityResult(requestCode, resultCode, data);
    AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);
}
...
```

####<a name="windows-desktop"></a>Pulpit systemu Windows:
- W `MainWindow.xaml.cs`, po prostu wywoływania `SearchByAlias(...)` przechodzące `WindowInteropHelper` na pulpicie `PlatformParameters` obiektu:

```C#
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

####<a name="ios"></a>iOS:
- W `DirSearchClient_iOSViewController.cs`, iOS `PlatformParameters` obiektu po prostu pobiera odwołanie do Kontroler widoku:

```C#
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

####<a name="windows-universal"></a>Uniwersalny systemu Windows:
- Uniwersalny Windows otwórz `MainPage.xaml.cs` i implementowanie `Search` metodę, która używa metody Pomocnik udostępnionego projektu do zaktualizuj interfejs użytkownika w razie potrzeby.

```C#
...
    List<User> results = await DirectorySearcherLib.DirectorySearcher.SearchByAlias(SearchTermText.Text, new PlatformParameters(PromptBehavior.Auto, false));
...
```

Gratulacje! Teraz masz pracy Xamarin aplikację, która ma możliwość uwierzytelniania użytkowników i bezpieczne połączenie API sieci Web przy użyciu OAuth 2.0 pięciu różnych platformach. Jeśli jeszcze tego nie zrobiono, nadszedł czas, aby wypełnić dzierżawy usługi z niektórzy użytkownicy. Uruchom aplikację DirectorySearcher i zaloguj się przy użyciu jednego z tych użytkowników. Wyszukiwanie innych użytkowników według ich głównej nazwy użytkownika.

ADAL ułatwia włączenie typowych funkcji tożsamości do aplikacji. Trwa istotnych wszystkich prac dirty dla Ciebie — zarządzania pamięcią podręczną, obsługa protokołu OAuth, prezentowanie użytkownika z identyfikatora interfejsu użytkownika, odświeżanie wygasłe tokeny i innych elementów. Naprawdę wiedzieć wystarczy połączenie jednego interfejsu API `authContext.AcquireToken*(…)`.

W przypadku odwołania, ukończony przykładowy (bez wartości konfiguracji) jest dostępna [w tym miejscu](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip). Możesz teraz można przejść do scenariusze dodatkowe tożsamości. Warto wypróbować:

[Zabezpieczenia programu .NET interfejsu API sieci Web z usługą Azure Active Directory >>](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
