<properties
    pageTitle="Rozpoczynanie pracy z biblioteką CDN Azure dla środowiska .NET | Microsoft Azure"
    description="Dowiedz się, jak napisać aplikacjom Zarządzanie CDN Azure za pomocą programu Visual Studio .NET."
    services="cdn"
    documentationCenter=".net"
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/15/2016"
    ms.author="casoper"/>

# <a name="get-started-with-azure-cdn-development"></a>Wprowadzenie do opracowywania Azure CDN

> [AZURE.SELECTOR]
- [Node.js](cdn-app-dev-node.md)
- [.NET](cdn-app-dev-net.md)

[Biblioteka CDN Azure dla środowiska .NET](https://msdn.microsoft.com/library/mt657769.aspx) umożliwia zautomatyzować tworzenie i zarządzanie profilami CDN i punktów końcowych.  Ten samouczek opisano tworzenie prostych aplikacji konsoli .NET demonstrujący kilka z dostępnych operacji.  Ten samouczek nie ma na celu opisania wszystkimi aspektami biblioteki CDN Azure dla środowiska .NET szczegółowo.

Potrzebujesz programu Visual Studio 2015 do użycia tego samouczka.  [Visual Studio społeczności 2015](https://www.visualstudio.com/products/visual-studio-community-vs.aspx) jest dostępne bezpłatnie do pobrania.

> [AZURE.TIP] [Ukończony projektu z tego samouczka](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c) jest dostępna do pobrania w witrynie MSDN.

[AZURE.INCLUDE [cdn-app-dev-prep](../../includes/cdn-app-dev-prep.md)]

## <a name="create-your-project-and-add-nuget-packages"></a>Tworzenie projektu i dodawanie pakietów Nuget

Firma Microsoft została utworzona grupa zasobów dla naszych profile CDN i uprawnienie naszych Azure AD aplikacji do zarządzania profilami CDN i punkty końcowe w tej grupie, możemy będą mieli możliwość tworzenia naszych aplikacji.

Z poziomu programu Visual Studio 2015 r., kliknij **plik**, **Nowy** **projektu...** , aby otworzyć okno dialogowe nowego projektu.  Rozwiń **Visual C#**, a następnie w okienku po lewej stronie wybierz **systemu Windows** .  Kliknij **Aplikację konsoli** w środkowym okienku.  Nadaj nazwę projektu, a następnie kliknij przycisk **OK**.  

![Nowy projekt](./media/cdn-app-dev-net/cdn-new-project.png)

Nasze projekt ma za pomocą niektórych bibliotek Azure zawarte w pakietach Nuget.  Dodawanie osoby do projektu.

1. Kliknij menu **Narzędzia** , **Menedżer pakietów Nuget**, a następnie **Konsoli Menedżera pakietów**.

    ![Zarządzanie pakietów Nuget](./media/cdn-app-dev-net/cdn-manage-nuget.png)

2. W konsoli Menedżera pakietów wykonaj następujące polecenie, aby zainstalować **Active Directory uwierzytelniania biblioteki (ADAL)**:

    `Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory`

3. Wykonaj poniższe czynności, aby zainstalować **Biblioteki zarządzania CDN Azure**:

    `Install-Package Microsoft.Azure.Management.Cdn`

## <a name="directives-constants-main-method-and-helper-methods"></a>Dyrektywy, stałych główna metoda i metod pomocy

Załóż podstawowej struktury nasz program napisany.

1. Ponownie na karcie Plik Program.cs Zamień `using` dyrektywy u góry z następujących czynności:

    ```csharp
    using System;
    using System.Collections.Generic;
    using Microsoft.Azure.Management.Cdn;
    using Microsoft.Azure.Management.Cdn.Models;
    using Microsoft.Azure.Management.Resources;
    using Microsoft.Azure.Management.Resources.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    ```

2. Należy zdefiniować kilka stałych używanych naszych metod.  W `Program` klasy, lecz przed `Main` metody, Dodaj następujący.  Pamiętaj zamienić symbole zastępcze, łącznie z ** &lt;nawiasy&gt;**, własne wartościami stosownie do potrzeb.

    ```csharp
    //Tenant app constants
    private const string clientID = "<YOUR CLIENT ID>";
    private const string clientSecret = "<YOUR CLIENT AUTHENTICATION KEY>"; //Only for service principals
    private const string authority = "https://login.microsoftonline.com/<YOUR TENANT ID>/<YOUR TENANT DOMAIN NAME>";

    //Application constants
    private const string subscriptionId = "<YOUR SUBSCRIPTION ID>";
    private const string profileName = "CdnConsoleApp";
    private const string endpointName = "<A UNIQUE NAME FOR YOUR CDN ENDPOINT>";
    private const string resourceGroupName = "CdnConsoleTutorial";
    private const string resourceLocation = "<YOUR PREFERRED AZURE LOCATION, SUCH AS Central US>";
    ```

3. Również na poziomie klasy, należy zdefiniować tych dwóch zmiennych.  Użyjemy tych później ustalenie, jeśli naszego punktu końcowego i profilu już istnieje.

    ```csharp
    static bool profileAlreadyExists = false;
    static bool endpointAlreadyExists = false;
    ```

4.  Zamienianie `Main` metody w następujący sposób:

    ```csharp
    static void Main(string[] args)
    {
        //Get a token
        AuthenticationResult authResult = GetAccessToken();

        // Create CDN client
        CdnManagementClient cdn = new CdnManagementClient(new TokenCredentials(authResult.AccessToken))
            { SubscriptionId = subscriptionId };

        ListProfilesAndEndpoints(cdn);

        // Create CDN Profile
        CreateCdnProfile(cdn);

        // Create CDN Endpoint
        CreateCdnEndpoint(cdn);
        
        Console.WriteLine();

        // Purge CDN Endpoint
        PromptPurgeCdnEndpoint(cdn);

        // Delete CDN Endpoint
        PromptDeleteCdnEndpoint(cdn);

        // Delete CDN Profile
        PromptDeleteCdnProfile(cdn);

        Console.WriteLine("Press Enter to end program.");
        Console.ReadLine();
    }
    ```

5. Niektóre z naszych innych metod zamiar Monituj o pytania "Tak/nie".  Dodaj poniższej metody, aby ułatwić który nieco:

    ```csharp
    private static bool PromptUser(string Question)
    {
        Console.Write(Question + " (Y/N): ");
        var response = Console.ReadKey();
        Console.WriteLine();
        if (response.Key == ConsoleKey.Y)
        {
            return true;
        }
        else if (response.Key == ConsoleKey.N)
        {
            return false;
        }
        else
        {
            // They pressed something other than Y or N.  Let's ask them again.
            return PromptUser(Question);
        }
    }
    ```

Teraz, gdy podstawowa struktura nasz program jest zapisywany, należy tworzymy metody o nazwie `Main` metody.

## <a name="authentication"></a>Uwierzytelnianie

Przed firma Microsoft korzysta z biblioteką zarządzania CDN Azure, potrzebne do uwierzytelniania naszych wystawcy usługi i uzyskać tokenu uwierzytelniania.  Ta metoda używa ADAL do pobierania tokenu.

```csharp
private static AuthenticationResult GetAccessToken()
{
    AuthenticationContext authContext = new AuthenticationContext(authority); 
    ClientCredential credential = new ClientCredential(clientID, clientSecret);
    AuthenticationResult authResult = 
        authContext.AcquireTokenAsync("https://management.core.windows.net/", credential).Result;

    return authResult;
}
```

Jeśli korzystasz z uwierzytelniania użytkowników indywidualnych, `GetAccessToken` metody wygląda nieco inaczej.

>[AZURE.IMPORTANT] Tylko za pomocą tego kodu przykładowego wybór uwierzytelnianie użytkownika zamiast głównej usługi ma być.

```csharp
private static AuthenticationResult GetAccessToken()
{
    AuthenticationContext authContext = new AuthenticationContext(authority);
    AuthenticationResult authResult = authContext.AcquireTokenAsync("https://management.core.windows.net/",
        clientID, new Uri("http://<redirect URI>"), new PlatformParameters(PromptBehavior.RefreshSession)).Result;

    return authResult;
}
```

Pamiętaj zamienić `<redirect URI>` z Przekieruj wprowadzone podczas rejestrowania aplikacji w Azure AD identyfikatora URI.

## <a name="list-cdn-profiles-and-endpoints"></a>Profile CDN listy i punkty końcowe

Teraz możemy przystąpić do wykonywania operacji CDN.  Najpierw naszej metoda wykonuje listy profilów i punkty końcowe w naszym grupa zasobów i w przypadku znalezienia zgodnego punktu końcowego i profilu nazwy określone w naszym stałych sprawia, że notatki tego na później, firma Microsoft nie próby utworzenia duplikatów.

```csharp
private static void ListProfilesAndEndpoints(CdnManagementClient cdn)
{
    // List all the CDN profiles in this resource group
    var profileList = cdn.Profiles.ListByResourceGroup(resourceGroupName);
    foreach (Profile p in profileList)
    {
        Console.WriteLine("CDN profile {0}", p.Name);
        if (p.Name.Equals(profileName, StringComparison.OrdinalIgnoreCase))
        {
            // Hey, that's the name of the CDN profile we want to create!
            profileAlreadyExists = true;
        }

        //List all the CDN endpoints on this CDN profile
        Console.WriteLine("Endpoints:");
        var endpointList = cdn.Endpoints.ListByProfile(p.Name, resourceGroupName);
        foreach (Endpoint e in endpointList)
        {
            Console.WriteLine("-{0} ({1})", e.Name, e.HostName);
            if (e.Name.Equals(endpointName, StringComparison.OrdinalIgnoreCase))
            {
                // The unique endpoint name already exists.
                endpointAlreadyExists = true;
            }
        }
        Console.WriteLine();
    }
}
```

## <a name="create-cdn-profiles-and-endpoints"></a>Tworzenie profilów CDN i punkty końcowe

Następnie utworzymy profilu.

```csharp
private static void CreateCdnProfile(CdnManagementClient cdn)
{
    if (profileAlreadyExists)
    {
        Console.WriteLine("Profile {0} already exists.", profileName);
    }
    else
    {
        Console.WriteLine("Creating profile {0}.", profileName);
        ProfileCreateParameters profileParms =
            new ProfileCreateParameters() { Location = resourceLocation, Sku = new Sku(SkuName.StandardVerizon) };
        cdn.Profiles.Create(profileName, profileParms, resourceGroupName);
    }
}
```

Po utworzeniu profilu utworzymy punktu końcowego.

```csharp
private static void CreateCdnEndpoint(CdnManagementClient cdn)
{
    if (endpointAlreadyExists)
    {
        Console.WriteLine("Profile {0} already exists.", profileName);
    }
    else
    {
        Console.WriteLine("Creating endpoint {0} on profile {1}.", endpointName, profileName);
        EndpointCreateParameters endpointParms =
            new EndpointCreateParameters()
            {
                Origins = new List<DeepCreatedOrigin>() { new DeepCreatedOrigin("Contoso", "www.contoso.com") },
                IsHttpAllowed = true,
                IsHttpsAllowed = true,
                Location = resourceLocation
            };
        cdn.Endpoints.Create(endpointName, endpointParms, profileName, resourceGroupName);
    }
}
```

>[AZURE.NOTE] W powyższym przykładzie przypisuje punkt końcowy źródła o nazwie *Contoso* z hostname `www.contoso.com`.  Należy zmienić tak, aby wskazywały hostname własne pochodzenia.

## <a name="purge-an-endpoint"></a>Trwałe usuwanie punktu końcowego

Przy założeniu, że został utworzony punkt końcowy, jedno zadanie typowe firma Microsoft może być wykonanie w naszym programie jest usuwanie zawartości w naszym punktu końcowego.

```csharp
private static void PromptPurgeCdnEndpoint(CdnManagementClient cdn)
{
    if (PromptUser(String.Format("Purge CDN endpoint {0}?", endpointName)))
    {
        Console.WriteLine("Purging endpoint. Please wait...");
        cdn.Endpoints.PurgeContent(endpointName, profileName, resourceGroupName, new List<string>() { "/*" });
        Console.WriteLine("Done.");
        Console.WriteLine();
    }
}
```

>[AZURE.NOTE] W przykładzie powyżej ciągu `/*` oznacza chcę wyczyścić wszystkie elementy w folderze głównym ścieżkę punktu końcowego.  Jest to równoważne sprawdzanie **Wyczyść wszystko** w oknie dialogowym Azure portal "czyszczenie". W `CreateCdnProfile` metodę, po utworzeniu naszą profil jako profil **CDN Azure z Verizon** przy użyciu kodu `Sku = new Sku(SkuName.StandardVerizon)`, dlatego ta zakończy się pomyślnie.  Jednak profile **CDN Azure z Akamai** nie obsługują **Wyczyść wszystko**, więc jeśli profilu Akamai została z tego samouczka, czy trzeba uwzględnić konkretnych ścieżek, aby wyczyścić.

## <a name="delete-cdn-profiles-and-endpoints"></a>Usuwanie profilów CDN i punkty końcowe

Metody ostatniej spowoduje usunięcie naszego punktu końcowego i profilu.

```csharp
private static void PromptDeleteCdnEndpoint(CdnManagementClient cdn)
{
    if(PromptUser(String.Format("Delete CDN endpoint {0} on profile {1}?", endpointName, profileName)))
    {
        Console.WriteLine("Deleting endpoint. Please wait...");
        cdn.Endpoints.DeleteIfExists(endpointName, profileName, resourceGroupName);
        Console.WriteLine("Done.");
        Console.WriteLine();
    }
}

private static void PromptDeleteCdnProfile(CdnManagementClient cdn)
{
    if(PromptUser(String.Format("Delete CDN profile {0}?", profileName)))
    {
        Console.WriteLine("Deleting profile. Please wait...");
        cdn.Profiles.DeleteIfExists(profileName, resourceGroupName);
        Console.WriteLine("Done.");
        Console.WriteLine();
    }
}
```

## <a name="running-the-program"></a>Uruchomienie programu

Pracujemy obecnie skompilować i uruchomić program, klikając przycisk **Start** , w programie Visual Studio.

![Program działający](./media/cdn-app-dev-net/cdn-program-running-1.png)

Gdy program osiągnie powyżej wiersza, można powrócić do swojej grupy zasobów w portalu Azure i zobacz, utworzony profil.

![Powodzenia!](./media/cdn-app-dev-net/cdn-success.png)

Potwierdzamy można następnie z instrukcjami, aby wykonać pozostałą część programu.

![Kończenie pracy programu](./media/cdn-app-dev-net/cdn-program-running-2.png)

## <a name="next-steps"></a>Następne kroki

Aby zobaczyć ukończonego projektu z tego instruktażu, [Pobierz próbki](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c).

Aby znaleźć dodatkowe dokumentacji w bibliotece zarządzania CDN Azure .NET, wyświetlanie [odwołania w witrynie MSDN](https://msdn.microsoft.com/library/mt657769.aspx).

Zarządzanie zasobami CDN przy użyciu [programu PowerShell](./cdn-manage-powershell.md).


