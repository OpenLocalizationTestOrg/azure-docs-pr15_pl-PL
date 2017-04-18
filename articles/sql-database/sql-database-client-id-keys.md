<properties
   pageTitle="Uzyskiwanie żądane wartości uwierzytelniania aplikacji, aby uzyskać dostęp do bazy danych SQL z kodu | Microsoft Azure"
   description="Tworzenie wystawcy usługi z kodu dostępu do bazy danych SQL."
   services="sql-database"
   documentationCenter=""
   authors="stevestein"
   manager="jhubbard"
   editor=""
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/30/2016"
   ms.author="sstein"/>

# <a name="get-the-required-values-for-authenticating-an-application-to-access-sql-database-from-code"></a>Uzyskiwanie żądane wartości uwierzytelniania aplikacji, aby uzyskać dostęp do bazy danych SQL z kodu

Umożliwia tworzenie i zarządzanie bazą danych SQL z kodu aplikacji musisz zarejestrować w domenie Azure Active Directory (AAD) w subskrypcji, w której utworzono Azure zasobów.

## <a name="create-a-service-principal-to-access-resources-from-an-application"></a>Tworzenie usługi kapitału uzyskiwać dostęp do zasobów z aplikacji

Musisz dysponować najnowszą [Azure programu PowerShell](https://msdn.microsoft.com/library/mt619274.aspx) zainstalować i uruchomić. Aby uzyskać szczegółowe informacje zobacz [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md).

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




## <a name="see-also"></a>Zobacz też

- [Tworzenie bazy danych SQL z C#](sql-database-get-started-csharp.md)
- [Nawiązywanie połączenia z bazą danych SQL za pomocą uwierzytelniania usługi Azure Active Directory](sql-database-aad-authentication.md)


