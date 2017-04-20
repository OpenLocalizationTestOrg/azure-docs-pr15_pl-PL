## <a name="obtain-an-azure-resource-manager-token"></a>Uzyskaj token Menedżera zasobów

Azure usługi Active Directory musi uwierzytelnić wszystkie zadania, które wykonują na zasoby za pomocą Menedżera zasobów. Tu przykładzie używa uwierzytelnianie hasłem, aby inne podejścia [żądań uwierzytelniania Menedżera zasobów], zobacz[lnk-authenticate-arm].

1. Dodaj następujący kod do metody **Main** w Program.cs do pobrania tokenu z Azure AD przy użyciu aplikacji identyfikator i hasło.

    ```
    var authContext = new AuthenticationContext(string.Format  
      ("https://login.windows.net/{0}", tenantId));
    var credential = new ClientCredential(applicationId, password);
    AuthenticationResult token = authContext.AcquireTokenAsync
      ("https://management.core.windows.net/", credential).Result;
    
    if (token == null)
    {
      Console.WriteLine("Failed to obtain the token");
      return;
    }
    ```

2. Utwórz obiekt **Maxreceivedmessagesizeinbytesresourcemanagementclient** , który używa tokenu dodając następujący kod do końca metody **Main** :

    ```
    var creds = new TokenCredentials(token.AccessToken);
    var client = new ResourceManagementClient(creds);
    client.SubscriptionId = subscriptionId;
    ```

3. Utworzyć lub uzyskać odwołania do grupy zasobów, którego używasz:

    ```
    var rgResponse = client.ResourceGroups.CreateOrUpdate(rgName,
        new ResourceGroup("East US"));
    if (rgResponse.Properties.ProvisioningState != "Succeeded")
    {
      Console.WriteLine("Problem creating resource group");
      return;
    }
    ```

[lnk-authenticate-arm]: https://msdn.microsoft.com/library/azure/dn790557.aspx