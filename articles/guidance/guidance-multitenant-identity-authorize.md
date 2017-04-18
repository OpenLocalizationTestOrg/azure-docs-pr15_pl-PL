<properties
   pageTitle="Autoryzacja w aplikacjach multitenant | Microsoft Azure"
   description="Jak przeprowadzić autoryzacji w aplikacji multitenant"
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
   ms.date="06/02/2016"
   ms.author="mwasson"/>

# <a name="role-based-and-resource-based-authorization-in-multitenant-applications"></a>Oparta na rolach i opartych na zasobach autoryzacji w aplikacjach multitenant

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Ten artykuł jest [częścią serii]. Istnieje także kompletnego [przykładowej aplikacji] dostarczonej z tej serii.

Nasze [implementacji] to aplikacja ASP.NET Core 1.0. W tym artykule przyjrzymy ogólne dwa sposoby autoryzacji, przy użyciu autoryzacji interfejsy API opisane w ASP.NET Core 1.0.

-   **Oparta na rolach autoryzacji**. Autoryzowanie czynności w oparciu o role przypisane do użytkownika. Na przykład niektóre akcje wymagają roli administratora.
-   **Autoryzacji oparty na zasób**. Autoryzowanie akcji według określonego zasobu. Na przykład każdy zasób ma właściciela. Właściciel może usuwać zasobu; Nie można innym użytkownikom.

Typowa aplikacja będzie stosować mieszanych. Na przykład aby usunąć zasób, użytkownik musi być zasobów właściciel _lub_ administrator.


## <a name="role-based-authorization"></a>Autoryzacja oparta na rolach

[Firma ankiet] [ Tailspin] definiuje aplikacja następujących ról:

- Administrator. Można wykonywać wszystkie operacje OBSŁUGIWAŁ na dowolny ankiety, należący do tej dzierżawy.
- Kreator. Można tworzyć nowe ankiety
- Czytnik. Można przeczytać wszelkie ankiety, które należą do tej dzierżawy

Role dotyczą _użytkowników_ aplikacji. W aplikacji ankiet użytkownik jest administratorem, Kreator albo czytnika.

Aby uzyskać omówienie jak zdefiniować i zarządzanie rolami zobacz [ról aplikacji].

Niezależnie od tego, jak można zarządzać rolami będzie wyglądać podobnie do kodu autoryzacji. Podstawowe ASP.NET 1.0 wprowadza analizę o nazwie [Zasady autoryzacji][policies]. Za pomocą tej funkcji Definiowanie zasady autoryzacji w kodzie, a następnie Zastosuj tych zasad do Kontroler akcji. Zasady jest odłączona od administratora.

### <a name="create-policies"></a>Tworzenie zasad

Aby zdefiniować zasady, najpierw należy utworzyć klasę, która wykonuje `IAuthorizationRequirement`. Najłatwiej jest je dziedziczyć `AuthorizationHandler`. W `Handle` metoda sprawdzenie odpowiednich claim(s).

Oto przykład z poziomu aplikacji ankiet firma:

```csharp
public class SurveyCreatorRequirement : AuthorizationHandler<SurveyCreatorRequirement>, IAuthorizationRequirement
{
    protected override void Handle(AuthorizationContext context, SurveyCreatorRequirement requirement)
    {
        if (context.User.HasClaim(ClaimTypes.Role, Roles.SurveyAdmin) ||
            context.User.HasClaim(ClaimTypes.Role, Roles.SurveyCreator))
        {
            context.Succeed(requirement);
        }
    }
}
```

> [AZURE.NOTE] Zobacz [SurveyCreatorRequirement.cs]

Ta klasa definiuje wymagania użytkownika, aby utworzyć nową ankietę. Użytkownik musi być w roli SurveyAdmin lub SurveyCreator.

W klasie uruchamiania Definiowanie nazwanych zasady, które zawiera co najmniej jeden wymagania. Jeśli istnieje wiele wymagań, użytkownik musi spełniać wymagania, _co_ ma być autoryzowany. Poniższy kod definiuje dwie zasady:

```csharp
services.AddAuthorization(options =>
{
    options.AddPolicy(PolicyNames.RequireSurveyCreator,
        policy =>
        {
            policy.AddRequirements(new SurveyCreatorRequirement());
            policy.AddAuthenticationSchemes(CookieAuthenticationDefaults.AuthenticationScheme);
        });

    options.AddPolicy(PolicyNames.RequireSurveyAdmin,
        policy =>
        {
            policy.AddRequirements(new SurveyAdminRequirement());
            policy.AddAuthenticationSchemes(CookieAuthenticationDefaults.AuthenticationScheme);
        });
});
```

> [AZURE.NOTE] Zobacz [Startup.cs]

Kod ustawia również schemat uwierzytelniania, który wskazuje ASP.NET, które pośredniczącym uwierzytelniania powinna działać, jeśli Autoryzacja nie powiedzie się. W tym przypadku określona pośredniczącym uwierzytelniania plików cookie, ponieważ pośredniczącym uwierzytelniania plików cookie można przekierowywać użytkownika do strony "Dostęp zabroniony". Lokalizacji strony dostęp zabroniony jest ustawiona w opcji AccessDeniedPath dla pośredniczącym plików cookie; zobacz [Konfigurowanie pośredniczącym uwierzytelniania].

### <a name="authorize-controller-actions"></a>Autoryzuj kontroler akcje

Na koniec, aby autoryzować akcji w kontrolerze MVC, ustawić zasady w `Authorize` atrybut:

```csharp
[Authorize(Policy = "SurveyCreatorRequirement")]
public IActionResult Create()
{
    // ...
}
```

We wcześniejszych wersjach programu ASP.NET na atrybucie chcesz ustawić właściwość **ról** :

```csharp
// old way
[Authorize(Roles = "SurveyCreator")]

```

Nadal jest to obsługiwane w ASP.NET 1.0 podstawowych, ale został kilka wad w porównaniu z zasady autoryzacji:

-   Zakłada roszczeń określonego typu. Dla każdego typu roszczeń sprawdzić zasady. Role są tylko typu oświadczeń.
-   Nazwa roli jest wpisane bezpośrednio do atrybutu. Za pomocą zasad logika autoryzacji jest w jednym miejscu łatwiejsza do aktualizowania lub nawet obciążenia w obszarze Ustawienia konfiguracji.
-   Zasady włączyć bardziej złożone decyzji dotyczących autoryzacji (np. wiek > = 21) nie można wyrazić przez członkostwo w roli prosty.

## <a name="resource-based-authorization"></a>Zasób w oparciu autoryzacji

_Zasób podstawie autoryzacji_ występuje, gdy zezwolenia zależy od określonego zasobu, która będzie miało wpływ na operacji. W aplikacji ankiet firma każdej ankiety ma właściciela i współautorzy zero do wielu.

-   Właściciel można czytać, aktualizowanie, usuwanie, publikowanie i cofanie publikowania ankiety.
-   Właściciel może przypisywać współautorzy na ankietę.
-   Współautorzy można czytać i aktualizowanie ankiety.

Zauważ, że "właściciela" i "współautora" nie są ról aplikacji; są one przechowywane na ankietę w bazie danych aplikacji. Aby sprawdzić, czy użytkownik może usunąć ankiety, na przykład aplikacji sprawdza, czy użytkownik jest właścicielem tej ankiety.

W ASP.NET Core 1.0 zaimplementować autoryzacji oparty na zasób wynikających z **AuthorizationHandler** i **obsługiwać** metody.

```csharp
public class SurveyAuthorizationHandler : AuthorizationHandler<OperationAuthorizationRequirement, Survey>
{
     protected override void Handle(AuthorizationContext context, OperationAuthorizationRequirement operation, Survey resource)
    {
    }
}
```

Należy zauważyć, że ta klasa ma jednoznacznie określony typ dla obiektów ankiety.  Zarejestruj się klasie DI podczas uruchamiania:

```csharp
services.AddSingleton<IAuthorizationHandler>(factory =>
{
    return new SurveyAuthorizationHandler();
});
```

Do sprawdzania autoryzacji za pomocą interfejsu **IAuthorizationService** , które można wprowadzić do kontrolerach. Poniższy kod sprawdza, czy użytkownik może czytać ankiety:

```csharp
if (await _authorizationService.AuthorizeAsync(User, survey, Operations.Read) == false)
{
    return new HttpStatusCodeResult(403);
}
```

Ponieważ możemy przekazać `Survey` obiektu, to połączenie wywoła `SurveyAuthorizationHandler`.

W kodzie autoryzacji dobrym rozwiązaniem jest agregacji wszystkie uprawnienia użytkownika oparta na rolach i opartych na zasobach, a następnie sprawdź Ustawianie agregacji przed żądanej operacji.
Oto przykład z aplikacji ankiety. Aplikacja określa różne typy uprawnień:

- Administrator
- Trybu współautora
- Kreator
- Właściciel
- Czytnik

Aplikacja definiuje również zestaw możliwych działań na ankiety:

- Tworzenie
- Odczyt
- Aktualizacja
- Usuwanie
- Publikowanie
- Unpublsh

Poniższy kod tworzy wykaz uprawnień dla określonego użytkownika i ankiety. Zwróć uwagę, że kod analizuje użytkownika aplikacji role i pola właściciela/współautora ankiety.

```csharp
protected override void Handle(AuthorizationContext context, OperationAuthorizationRequirement operation, Survey resource)
{
    var permissions = new List<UserPermissionType>();
    string userTenantId = context.User.GetTenantIdValue();
    int userId = ClaimsPrincipalExtensions.GetUserKey(context.User);
    string user = context.User.GetUserName();

    if (resource.TenantId == userTenantId)
    {
        // Admin can do anything, as long as the resource belongs to the admin's tenant.
        if (context.User.HasClaim(ClaimTypes.Role, Roles.SurveyAdmin))
        {
            context.Succeed(operation);
            return;
        }

        if (context.User.HasClaim(ClaimTypes.Role, Roles.SurveyCreator))
        {
            permissions.Add(UserPermissionType.Creator);
        }
        else
        {
            permissions.Add(UserPermissionType.Reader);
        }

        if (resource.OwnerId == userId)
        {
            permissions.Add(UserPermissionType.Owner);
        }
    }
    if (resource.Contributors != null && resource.Contributors.Any(x => x.UserId == userId))
    {
        permissions.Add(UserPermissionType.Contributor);
    }
    if (ValidateUserPermissions[operation](permissions))
    {
        context.Succeed(operation);
    }
}
```

> [AZURE.NOTE] Zobacz [SurveyAuthorizationHandler.cs].

W aplikacji wielu dzierżawy należy się upewnić, że uprawnienia nie "pamięci" do innej dzierżawy danych. W aplikacji ankiet uprawnienia współautora jest dozwolona między dzierżawami &mdash; Przydzielanie kogoś z fuzji jako contriubutor. Inne typy uprawnień są ograniczone do zasobów, które należą do dzierżawy tego użytkownika. Aby wymusić to wymaganie, kod sprawdza identyfikator dzierżawy przed przyznaniem uprawnień. ( `TenantId` Pól przypisany po utworzeniu ankiety.)

Następnym krokiem jest sprawdzenie operacji (odczytu, aktualizacji, Usuń itp.) przed uprawnienia. Aplikacja ankiet wykonuje tej czynności za pomocą tabeli odnośników funkcji:

```csharp
static readonly Dictionary<OperationAuthorizationRequirement, Func<List<UserPermissionType>, bool>> ValidateUserPermissions
    = new Dictionary<OperationAuthorizationRequirement, Func<List<UserPermissionType>, bool>>

    {
        { Operations.Create, x => x.Contains(UserPermissionType.Creator) },

        { Operations.Read, x => x.Contains(UserPermissionType.Creator) ||
                                x.Contains(UserPermissionType.Reader) ||
                                x.Contains(UserPermissionType.Contributor) ||
                                x.Contains(UserPermissionType.Owner) },

        { Operations.Update, x => x.Contains(UserPermissionType.Contributor) ||
                                x.Contains(UserPermissionType.Owner) },

        { Operations.Delete, x => x.Contains(UserPermissionType.Owner) },

        { Operations.Publish, x => x.Contains(UserPermissionType.Owner) },

        { Operations.UnPublish, x => x.Contains(UserPermissionType.Owner) }
    };
```


## <a name="next-steps"></a>Następne kroki

- Przeczytaj artykuł dalej w tej serii: [Zabezpieczanie wewnętrznej bazie danych sieci web interfejsu API w aplikacji multitenant][web-api]
- Aby dowiedzieć się więcej o autoryzacji zasobów oparte na podstawowych 1.0 programu ASP.NET, zobacz [Autoryzacji oparty na zasób][rbac].

<!-- Links -->
[Tailspin]: guidance-multitenant-identity-tailspin.md
[częścią serii]: guidance-multitenant-identity.md
[Role aplikacji]: guidance-multitenant-identity-app-roles.md
[policies]: https://docs.asp.net/en/latest/security/authorization/policies.html
[rbac]: https://docs.asp.net/en/latest/security/authorization/resourcebased.html
[implementacji]: guidance-multitenant-identity-tailspin.md
[SurveyCreatorRequirement.cs]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/src/Tailspin.Surveys.Security/Policy/SurveyCreatorRequirement.cs
[Startup.CS]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/src/Tailspin.Surveys.Web/Startup.cs
[Konfigurowanie uwierzytelniania pośredniczącym]: guidance-multitenant-identity-authenticate.md#configuring-the-authentication-middleware
[SurveyAuthorizationHandler.cs]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/src/Tailspin.Surveys.Security/Policy/SurveyAuthorizationHandler.cs
[Przykładowa aplikacja]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps
[web-api]: guidance-multitenant-identity-web-api.md
