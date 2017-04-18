<properties
    pageTitle="Bazy danych SQL spróbuj: Używanie C# do utworzenia bazy danych SQL | Microsoft Azure"
    description="Spróbuj bazy danych SQL dotyczącej opracowywania aplikacji SQL i C# i tworzenie bazy danych SQL Azure z używania biblioteki bazy danych SQL dla środowiska .NET C#."
    keywords="Spróbuj sql, sql c#"   
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="csharp"
   ms.workload="data-management"
   ms.date="10/04/2016"
   ms.author="sstein"/>

# <a name="try-sql-database-use-c-to-create-a-sql-database-with-the-sql-database-library-for-net"></a>Spróbuj bazy danych SQL: Używanie C# do utworzenia bazy danych SQL z biblioteką bazy danych SQL dla środowiska .NET


> [AZURE.SELECTOR]
- [Azure portal](sql-database-get-started.md)
- [C#](sql-database-get-started-csharp.md)
- [Programu PowerShell](sql-database-get-started-powershell.md)

Dowiedz się, jak używać C# do tworzenia bazy danych programu Azure SQL z [Biblioteki Microsoft Azure SQL zarządzania dla środowiska .NET](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql). Ten artykuł zawiera opis sposobu tworzenia jednej bazie danych SQL i C#. Aby utworzyć pul elastyczne bazy danych, zobacz [Tworzenie puli elastyczne bazy danych](sql-database-elastic-pool-create-csharp.md).

Biblioteka zarządzania bazy danych SQL Azure, dla środowiska .NET zawiera [Menedżera zasobów Azure](../azure-resource-manager/resource-group-overview.md)-podstawie interfejs API, który był zawijany [Oparte na Menedżera zasobów z interfejsu API usługi REST SQL bazy danych](https://msdn.microsoft.com/library/azure/mt163571.aspx).

>[AZURE.NOTE] Wiele nowych funkcji bazy danych SQL są obsługiwane tylko podczas używania [model wdrożenia Azure Menedżera zasobów](../azure-resource-manager/resource-group-overview.md), aby zawsze należy używać r **Biblioteka zarządzania bazy danych SQL Azure dla środowiska .NET ([dokumenty](https://msdn.microsoft.com/library/azure/mt349017.aspx) | [Pakietu NuGet](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql))**. Starsze [biblioteki oparte na modelu Klasyczny wdrożenia](https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Sql) są obsługiwane tylko zgodności z poprzednimi wersjami, zalecamy zapoznanie Użyj nowszego bibliotek Menedżera zasobów, na podstawie.

Aby wykonać czynności opisane w tym artykule, są potrzebne następujące elementy:

- Subskrypcję usługi Azure. W razie potrzeby subskrypcji usługi Azure po prostu kliknij **Bezpłatne konto** u góry strony, a następnie wróć do końca w tym artykule.
- Programu Visual Studio. Aby uzyskać bezpłatną kopię programu Visual Studio zobacz stronę [Visual Studio — pliki do pobrania](https://www.visualstudio.com/downloads/download-visual-studio-vs) .

>[AZURE.NOTE] W tym artykule tworzy nową, pustą bazę danych SQL. Modyfikowanie metody *CreateOrUpdateDatabase(...)* w następującym przykładzie kopiowanie bazy danych, skali baz danych, utworzyć bazę danych w puli itd. Aby uzyskać więcej informacji Zobacz klasy [DatabaseCreateMode](https://msdn.microsoft.com/library/microsoft.azure.management.sql.models.databasecreatemode.aspx) i [DatabaseProperties](https://msdn.microsoft.com/library/microsoft.azure.management.sql.models.databaseproperties.aspx) .



## <a name="create-a-console-app-and-install-the-required-libraries"></a>Tworzenie aplikacji konsoli i zainstaluj wymagane bibliotek

1. Uruchom program Visual Studio.
2. Kliknij pozycję **plik** > **Nowy** > **projektu**.
3. Tworzenie C# **Aplikacji konsoli** i nadaj mu nazwę: *SqlDbConsoleApp*


Aby utworzyć bazy danych SQL z C#, załadować bibliotek wymagane zarządzania (za pomocą [konsoli Menedżer pakietu](http://docs.nuget.org/Consume/Package-Manager-Console)):

1. Kliknij pozycję **Narzędzia** > **Menedżera pakietów NuGet** > **Konsoli Menedżera pakietów**.
2. Typ `Install-Package Microsoft.Azure.Management.Sql –Pre` zainstalować najnowszą [Biblioteki zarządzania Microsoft Azure SQL](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql).
3. Typ `Install-Package Microsoft.Azure.Management.ResourceManager –Pre` zainstalować [Microsoft Azure Menedżera zasobów biblioteki](https://www.nuget.org/packages/Microsoft.Azure.Management.ResourceManager).
4. Typ `Install-Package Microsoft.Azure.Common.Authentication –Pre` do zainstalowania [Biblioteki Microsoft Azure typowych uwierzytelniania](https://www.nuget.org/packages/Microsoft.Azure.Common.Authentication). 



> [AZURE.NOTE] W przykładach w tym artykule formularz obraz każdego żądania interfejsu API i zablokować do momentu zakończenia pozostałych połączeń w usłudze podstawowej. Dostępne są metody asynchroniczne.


## <a name="create-a-sql-database-server-firewall-rule-and-sql-database---c-example"></a>Tworzenie bazy danych programu SQL server, reguły zapory i baza danych SQL - C# przykład

Poniższy przykład tworzy grupę zasobów, serwer reguły zapory i baza danych SQL. Zobacz [Tworzenie głównej dostęp do zasobów usługi](#create-a-service-principal-to-access-resources) , aby uzyskać `_subscriptionId, _tenantId, _applicationId, and _applicationSecret` zmiennych.

Zamień zawartość **Plik Program.cs** wpisem i aktualizowanie `{variables}` własnymi wartościami aplikacji (nie powinno obejmować `{}`).


    using Microsoft.Azure;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.Azure.Management.Sql;
    using Microsoft.Azure.Management.Sql.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System;
    
    namespace SqlDbConsoleApp
    {
    class Program
       {
        // For details about these four (4) values, see
        // https://azure.microsoft.com/documentation/articles/resource-group-authenticate-service-principal/
        static string _subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}";
        static string _tenantId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}";
        static string _applicationId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}";
        static string _applicationSecret = "{your-password}";

        // Create management clients for the Azure resources your app needs to work with.
        // This app works with Resource Groups, and Azure SQL Database.
        static ResourceManagementClient _resourceMgmtClient;
        static SqlManagementClient _sqlMgmtClient;

        // Authentication token
        static AuthenticationResult _token;

        // Azure resource variables
        static string _resourceGroupName = "{resource-group-name}";
        static string _resourceGrouplocation = "{Azure-region}";

        static string _serverlocation = _resourceGrouplocation;
        static string _serverName = "{server-name}";
        static string _serverAdmin = "{server-admin-login}";
        static string _serverAdminPassword = "{server-admin-password}";

        static string _firewallRuleName = "{firewall-rule-name}";
        static string _startIpAddress = "{0.0.0.0}";
        static string _endIpAddress = "{255.255.255.255}";

        static string _databaseName = "{dbfromcsarticle}";
        static string _databaseEdition = DatabaseEditions.Basic;
        static string _databasePerfLevel = ""; // "S0", "S1", and so on here for other tiers


        static void Main(string[] args)
        {
            // Authenticate:
            _token = GetToken(_tenantId, _applicationId, _applicationSecret);
            Console.WriteLine("Token acquired. Expires on:" + _token.ExpiresOn);

            // Instantiate management clients:
            _resourceMgmtClient = new ResourceManagementClient(new Microsoft.Rest.TokenCredentials(_token.AccessToken));
            _sqlMgmtClient = new SqlManagementClient(new TokenCloudCredentials(_subscriptionId, _token.AccessToken));


            Console.WriteLine("Resource group...");
            ResourceGroup rg = CreateOrUpdateResourceGroup(_resourceMgmtClient, _subscriptionId, _resourceGroupName, _resourceGrouplocation);
            Console.WriteLine("Resource group: " + rg.Id);


            Console.WriteLine("Server...");
            ServerGetResponse sgr = CreateOrUpdateServer(_sqlMgmtClient, _resourceGroupName, _serverlocation, _serverName, _serverAdmin, _serverAdminPassword);
            Console.WriteLine("Server: " + sgr.Server.Id);

            Console.WriteLine("Server firewall...");
            FirewallRuleGetResponse fwr = CreateOrUpdateFirewallRule(_sqlMgmtClient, _resourceGroupName, _serverName, _firewallRuleName, _startIpAddress, _endIpAddress);
            Console.WriteLine("Server firewall: " + fwr.FirewallRule.Id);

            Console.WriteLine("Database...");
            DatabaseCreateOrUpdateResponse dbr = CreateOrUpdateDatabase(_sqlMgmtClient, _resourceGroupName, _serverName, _databaseName, _databaseEdition, _databasePerfLevel);
            Console.WriteLine("Database: " + dbr.Database.Id);


            Console.WriteLine("Press any key to continue...");
            Console.ReadKey();
        }

        static ResourceGroup CreateOrUpdateResourceGroup(ResourceManagementClient resourceMgmtClient, string subscriptionId, string resourceGroupName, string resourceGroupLocation)
        {
            ResourceGroup resourceGroupParameters = new ResourceGroup()
            {
                Location = resourceGroupLocation,
            };
            resourceMgmtClient.SubscriptionId = subscriptionId;
            ResourceGroup resourceGroupResult = resourceMgmtClient.ResourceGroups.CreateOrUpdate(resourceGroupName, resourceGroupParameters);
            return resourceGroupResult;
        }

        static ServerGetResponse CreateOrUpdateServer(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverLocation, string serverName, string serverAdmin, string serverAdminPassword)
        {
            ServerCreateOrUpdateParameters serverParameters = new ServerCreateOrUpdateParameters()
            {
                Location = serverLocation,
                Properties = new ServerCreateOrUpdateProperties()
                {
                    AdministratorLogin = serverAdmin,
                    AdministratorLoginPassword = serverAdminPassword,
                    Version = "12.0"
                }
            };
            ServerGetResponse serverResult = sqlMgmtClient.Servers.CreateOrUpdate(resourceGroupName, serverName, serverParameters);
            return serverResult;
        }


        static FirewallRuleGetResponse CreateOrUpdateFirewallRule(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverName, string firewallRuleName, string startIpAddress, string endIpAddress)
        {
            FirewallRuleCreateOrUpdateParameters firewallParameters = new FirewallRuleCreateOrUpdateParameters()
            {
                Properties = new FirewallRuleCreateOrUpdateProperties()
                {
                    StartIpAddress = startIpAddress,
                    EndIpAddress = endIpAddress
                }
            };
            FirewallRuleGetResponse firewallResult = sqlMgmtClient.FirewallRules.CreateOrUpdate(resourceGroupName, serverName, firewallRuleName, firewallParameters);
            return firewallResult;
        }



        static DatabaseCreateOrUpdateResponse CreateOrUpdateDatabase(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverName, string databaseName, string databaseEdition, string databasePerfLevel)
        {
            // Retrieve the server that will host this database
            Server currentServer = sqlMgmtClient.Servers.Get(resourceGroupName, serverName).Server;

            // Create a database: configure create or update parameters and properties explicitly
            DatabaseCreateOrUpdateParameters newDatabaseParameters = new DatabaseCreateOrUpdateParameters()
            {
                Location = currentServer.Location,
                Properties = new DatabaseCreateOrUpdateProperties()
                {
                    CreateMode = DatabaseCreateMode.Default,
                    Edition = databaseEdition,
                    RequestedServiceObjectiveName = databasePerfLevel
                }
            };
            DatabaseCreateOrUpdateResponse dbResponse = sqlMgmtClient.Databases.CreateOrUpdate(resourceGroupName, serverName, databaseName, newDatabaseParameters);
            return dbResponse;
        }



        private static AuthenticationResult GetToken(string tenantId, string applicationId, string applicationSecret)
        {
            AuthenticationContext authContext = new AuthenticationContext("https://login.windows.net/" + tenantId);
            _token = authContext.AcquireToken("https://management.core.windows.net/", new ClientCredential(applicationId, applicationSecret));
            return _token;
        }
      }
    }





## <a name="create-a-service-principal-to-access-resources"></a>Tworzenie usługi kapitału uzyskiwać dostęp do zasobów

Poniższy skrypt programu PowerShell tworzy aplikację Active Directory (AD) i wystawcy usługi, jaka jest potrzebna do uwierzytelnienia naszych aplikacji C#. Skrypt wyświetla wartości, które administrator powinien dla powyższego przykładu C#. Aby uzyskać szczegółowe informacje Zobacz za [pomocą Azure utworzyć usługi kapitału dostępu do zasobów](../resource-group-authenticate-service-principal.md).

   
    # Sign in to Azure.
    Add-AzureRmAccount
    
    # If you have multiple subscriptions, uncomment and set to the subscription you want to work with.
    #$subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}"
    #Set-AzureRmContext -SubscriptionId $subscriptionId
    
    # Provide these values for your new AAD app.
    # $appName is the display name for your app, must be unique in your directory.
    # $uri does not need to be a real uri.
    # $secret is a password you create.
    
    $appName = "{app-name}"
    $uri = "http://{app-name}"
    $secret = "{app-password}"
    
    # Create a AAD app
    $azureAdApplication = New-AzureRmADApplication -DisplayName $appName -HomePage $Uri -IdentifierUris $Uri -Password $secret
    
    # Create a Service Principal for the app
    $svcprincipal = New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId
    
    # To avoid a PrincipalNotFound error, I pause here for 15 seconds.
    Start-Sleep -s 15
    
    # If you still get a PrincipalNotFound error, then rerun the following until successful. 
    $roleassignment = New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $azureAdApplication.ApplicationId.Guid
    
    
    # Output the values we need for our C# application to successfully authenticate
    
    Write-Output "Copy these values into the C# sample app"
    
    Write-Output "_subscriptionId:" (Get-AzureRmContext).Subscription.SubscriptionId
    Write-Output "_tenantId:" (Get-AzureRmContext).Tenant.TenantId
    Write-Output "_applicationId:" $azureAdApplication.ApplicationId.Guid
    Write-Output "_applicationSecret:" $secret



## <a name="next-steps"></a>Następne kroki
Teraz, gdy mimo wypróbowania bazy danych SQL i skonfigurować bazy danych za pomocą C#, możesz uzyskać następujące artykuły:

- [Nawiązywanie połączenia z bazą danych SQL z programu SQL Server Management Studio i wykonać przykładowa kwerenda T-SQL](sql-database-connect-query-ssms.md)

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Baza danych SQL](https://azure.microsoft.com/documentation/services/sql-database/)
- [Klasy bazy danych](https://msdn.microsoft.com/library/azure/microsoft.azure.management.sql.models.database.aspx)




<!--Image references-->
[1]: ./media/sql-database-get-started-csharp/aad.png
[2]: ./media/sql-database-get-started-csharp/permissions.png
[3]: ./media/sql-database-get-started-csharp/getdomain.png
[4]: ./media/sql-database-get-started-csharp/aad2.png
[5]: ./media/sql-database-get-started-csharp/aad-applications.png
[6]: ./media/sql-database-get-started-csharp/add.png
[7]: ./media/sql-database-get-started-csharp/add-application.png
[8]: ./media/sql-database-get-started-csharp/add-application2.png
[9]: ./media/sql-database-get-started-csharp/clientid.png
