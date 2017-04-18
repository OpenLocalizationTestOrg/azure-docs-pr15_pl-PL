<properties
   pageTitle="Praca z systemem roszczeń tożsamości w aplikacjach multitenant | Microsoft Azure"
   description="Sposób użycia roszczeń o poprawności wystawcy i autoryzacji"
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="roshar"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/23/2016"
   ms.author="mwasson"/>

# <a name="working-with-claims-based-identities-in-multitenant-applications"></a>Praca z opartych na oświadczeniach tożsamości w aplikacjach multitenant

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Ten artykuł jest [częścią serii]. Istnieje także kompletnego [przykładowej aplikacji] dostarczonej z tej serii.

## <a name="claims-in-azure-ad"></a>Roszczeń w Azure AD

Gdy użytkownik zaloguje się, Azure AD wysyła token Identyfikatora, który zawiera zestaw oświadczeń o użytkowniku. Wniosek jest po prostu jakieś informacje, wyrażoną w postaci pary klucz wartość. Na przykład `email` = `bob@contoso.com`.  Roszczeń mają wystawcy &mdash; w tym przypadku Azure AD &mdash; czyli jednostki, który uwierzytelnia użytkownika i tworzy roszczeń. Ufasz roszczeń, ponieważ ufasz wystawcy. (I odwrotnie, jeśli nie ufasz wystawcy nie ufaj roszczeń!)

Na wysokim poziomie:

1.  Uwierzytelnianie użytkownika.
2.  Protokołu IDP wysyła zestaw oświadczeń.
3.  Aplikacja normalizuje lub rozszerzają oświadczeń (opcjonalnie).
4.  Aplikacja używa oświadczeń podejmowanie decyzji dotyczących autoryzacji.

W OpenID połączyć zestaw oświadczeń, co jest kontrolowane przez [parametr zakres] żądania uwierzytelniania. Jednak Azure AD problemy ograniczony zestaw oświadczeń za pośrednictwem połączenia OpenID; zobacz [obsługiwane Token i typy oświadczeń]. Jeśli chcesz, aby uzyskać więcej informacji o użytkowniku, musisz korzystać z interfejsu API Azure AD wykresu.

Oto niektóre roszczeń związanych z AAD, który aplikacji zwykle może zwracać uwagę na:

Przejmowanie wpisz identyfikator tokenu |    Opis
-----------------------|--------------
lub | Kto token został wystawiony dla. Są to identyfikator aplikacji klienta. Ogólnie rzecz biorąc nie musisz martwić się o to zastrzeżenie ponieważ automatycznie sprawdza pośredniczącym. Przykład:`"91464657-d17a-4327-91f3-2ed99386406f"`
grupy   | Lista grup AAD, których użytkownik jest członkiem. Przykład:`["93e8f556-8661-4955-87b6-890bc043c30f", "fc781505-18ef-4a31-a7d5-7d931d7b857e"]`
firmy  | [Wystawcy] tokenu OIDC. Przykład:`https://sts.windows.net/b9bd2162-77ac-4fb2-8254-5c36e9c0a9c4/`
Nazwa    | Nazwa wyświetlana użytkownika. Przykład:`"Alice A."`
OID | Identyfikator obiektu dla użytkownika w AAD. Ta wartość jest niezmienny i jednorazowego identyfikator użytkownika. Za pomocą tej wartości, nie wiadomości e-mail jako unikatowy identyfikator dla użytkowników. adresy e-mail można zmienić. Użycie interfejsu API Azure AD wykresu w aplikacji, identyfikator obiektu jest tej wartości używane do informacji w profilu kwerendy. Przykład:`"59f9d2dc-995a-4ddf-915e-b3bb314a7fa4"`
role   | Lista role aplikacji dla użytkownika. Przykład:`["SurveyCreator"]`
TID | Identyfikator dzierżawy. Ta wartość jest unikatowy identyfikator dla dzierżawy w Azure AD. Przykład:`"b9bd2162-77ac-4fb2-8254-5c36e9c0a9c4"`
unique_name | Ludzi czytelne wyświetlaną nazwę użytkownika. Przykład:`"alice@contoso.com"`
głównej nazwy użytkownika | Główna nazwa użytkownika. Przykład:`"alice@contoso.com"`

W poniższej tabeli wymieniono typy oświadczeń znajdujące się w token Identyfikatora. W ASP.NET Core 1.0 pośredniczącym łączenie OpenID Konwertuje niektóre typy oświadczeń po jego wypełnia kolekcję oświadczeniach dla głównej użytkownika:

-   OID >`http://schemas.microsoft.com/identity/claims/objectidentifier`
-   TID >`http://schemas.microsoft.com/identity/claims/tenantid`
-   unique_name >`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`
-   UPN >`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn`

## <a name="claims-transformations"></a>Roszczeń przekształcenia

Podczas przepływ uwierzytelniania można zmodyfikować oświadczeń, które można uzyskać od protokołu IDP. W 1.0 Core ASP.NET można wykonywać transformacja oświadczeniach umieszczona w zdarzeniu **AuthenticationValidated** z pośredniczącym OpenID połączenia. (Zobacz [zdarzenia uwierzytelniania]).

Które Dodawanie podczas **AuthenticationValidated** roszczeń są przechowywane w pliku cookie uwierzytelniania sesji. Nie uzyskiwanie one przypisany powrót do Azure AD.

Oto kilka przykładów transformacji oświadczeń:

-   **Roszczeń normalizacji**lub wprowadzać oświadczeniach spójne dla użytkowników. Dotyczy to szczególnie wystąpienia roszczeń związanych z wieloma IDPs, których może być roszczeń różne typy podobne informacje.
Na przykład Azure AD wysyła roszczeń "upn", zawierający e-mail użytkownika. Inne IDPs może wysłać roszczeń "e-mail". Poniższy kod konwertuje zastrzeżenie "upn" zastrzeżenie "e-mail":

    ```csharp
    var email = principal.FindFirst(ClaimTypes.Upn)?.Value;
    if (!string.IsNullOrWhiteSpace(email))
    {
      identity.AddClaim(new Claim(ClaimTypes.Email, email));
    }
    ```

- Dodawanie **domyślnego rościć sobie wartości** roszczeń, które nie są wyświetlane &mdash; na przykład przypisanie użytkownika do domyślnej roli. W niektórych przypadkach może to uprościć logiczny autoryzacji.
- Dodaj **niestandardowe typy oświadczeń** z poszczególnych aplikacji informacje o użytkowniku. Na przykład może przechowywać niektóre informacje o użytkowniku w bazie danych. Można dodać niestandardowe roszczeń z tych informacji do biletów uwierzytelniania. Roszczenia są przechowywane w pliku cookie, więc należy ją uzyskać z bazy danych na początku sesji logowania. Z drugiej strony należy unikać tworzenia zbyt duże pliki cookie, dlatego należy wziąć pod uwagę zależność między rozmiar plików cookie i odnośniki bazy danych.   

Po zakończeniu przepływ uwierzytelniania oświadczeń są dostępne w `HttpContext.User`. W tym momencie można traktować je jako tylko do odczytu Kolekcja &mdash; np używać ich do podejmowania decyzji dotyczących autoryzacji.

## <a name="issuer-validation"></a>Sprawdzanie poprawności wystawcy
W OpenID połączyć roszczeń wystawcy ("firmy") służy do identyfikowania protokołu IDP, który wystawiony token Identyfikatora. Część przepływ uwierzytelniania OIDC jest sprawdzenie, czy roszczeń wystawcy odpowiada rzeczywistej wystawcy. Pośredniczącym OIDC obsługuje to za Ciebie.

W przypadku Azure AD unikatowe na dzierżawcę AD ma wartość wystawcy (`https://sts.windows.net/<tenantID>`). W związku z tym aplikacja powinna wykonać dodatkową kontrolę, aby upewnić się, że wystawcy reprezentuje dzierżawy, który jest dozwolony do zalogowania się do aplikacji.

Stosowania jednego dzierżawy możesz po prostu Sprawdź, czy wystawcy jest własne dzierżawy. W rzeczywistości pośredniczącym OIDC powinien się tym zająć automatycznie domyślnie. W aplikacji dla wielu dzierżawy należy zezwolić dla wielu emitentów, odpowiadających różnych dzierżaw. Poniżej przedstawiono ogólne podejście umożliwia:

-   W opcjach pośredniczącym OIDC ustaw **ValidateIssuer** ma wartość FAŁSZ. Ta opcja powoduje wyłączenie funkcji automatycznego sprawdzania.
-   Po zarejestrowaniu dzierżawy, przechowywania dzierżawy i wystawcy w użytkownika bazy danych.
-   Gdy użytkownik zaloguje się, wyszukiwanie wystawcy w bazie danych. Jeśli nie została odnaleziona wystawcy, oznacza to, dzierżawy nie utworzył konto. Można przekierowywać je na znak o stronę w górę.
-  Można również blacklist niektórych dzierżaw; na przykład dla klientów, którzy nie zapłacić swoją subskrypcję.

Aby uzyskać bardziej szczegółowe informacje, zobacz [zapisów i dzierżawa ułatwiającej rozpoczęcie korzystania w aplikacji multitenant][signup].

## <a name="using-claims-for-authorization"></a>Za pomocą wniosków o zezwolenia

Roszczeń tożsamości użytkownika jest już wbudowanymi jednostki. Na przykład użytkownik może być adres e-mail, numer telefonu, urodziny, płeć itd. Być może protokołu IDP użytkownika są przechowywane wszystkie te informacje. Ale gdy uwierzytelnienia użytkownika, zostanie wyświetlony zwykle podzbiór tych roszczeń. W tym modelu tożsamość użytkownika jest po prostu pakietu roszczeń. Po wprowadzeniu autoryzacji decyzji dotyczących użytkownika będzie wyglądać dla określonego zestawów roszczeń. Innymi słowy, pytanie ostatecznie "Użytkownika X wykonać akcję Y" staje się "Czy użytkownik ma X rościć sobie Z".

Poniżej przedstawiono kilka podstawowych desenie sprawdzania roszczeń.

-  Aby sprawdzić, czy użytkownik ma określony roszczeń z określonej wartości:

    ```csharp
    if (User.HasClaim(ClaimTypes.Role, "Admin")) { ... }
    ```
    Kod sprawdza, czy użytkownik ma roszczenia roli z wartością "Administrator". Obsługuje ona prawidłowo wielkości liter, w których użytkownik nie stwierdzenia, roli lub wielu roszczeń roli.

    Klasa **ClaimTypes** definiuje stałe dla roszczeń często używanych typów. Jednak może zawierać dowolną wartość ciągu typu roszczeń.

-   Aby uzyskać pojedynczą wartość dla typu roszczeń, jeśli potrzebujesz istniała co najwyżej jedną wartość:
    ```csharp
     string email = User.FindFirst(ClaimTypes.Email)?.Value;
    ```
-   Aby wyświetlić wszystkie wartości dla danego typu roszczeń:

    ```csharp
     IEnumerable<Claim> groups = User.FindAll("groups");
    ```

Aby uzyskać więcej informacji, zobacz [Autoryzacja oparta na rolach i opartych na zasobach w aplikacjach multitenant][authorization].

## <a name="next-steps"></a>Następne kroki

- Przeczytaj artykuł dalej w tej serii: [zapisów i dzierżawa ułatwiającej rozpoczęcie korzystania w aplikacji multitenant][signup]
- Dowiedz się więcej o [autoryzacji opartych na oświadczeniach] w dokumentacji 1.0 Core programu ASP.NET

<!-- Links -->
[częścią serii]: guidance-multitenant-identity.md
[Parametr zakresu]: http://nat.sakimura.org/2012/01/26/scopes-and-claims-in-openid-connect/
[Obsługiwane Token i typy oświadczeń]: ../active-directory/active-directory-token-and-claims.md
[wystawcy]: http://openid.net/specs/openid-connect-core-1_0.html#IDToken
[Zdarzenia uwierzytelniania]: guidance-multitenant-identity-authenticate.md#authentication-events
[signup]: guidance-multitenant-identity-signup.md
[Autoryzacja opartych na oświadczeniach]: https://docs.asp.net/en/latest/security/authorization/claims.html
[Przykładowa aplikacja]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps
[authorization]: guidance-multitenant-identity-authorize.md
