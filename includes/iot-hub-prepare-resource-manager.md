## <a name="prepare-to-authenticate-azure-resource-manager-requests"></a>Przygotowanie do uwierzytelniania żądań Menedżera zasobów

Wszystkie operacje wykonywane na zasoby za pomocą [Menedżera zasobów] musi uwierzytelniać[ lnk-authenticate-arm] z Azure Active Directory (AD). Najłatwiejszą metodą skonfigurowania to jest użyć środowiska PowerShell lub Powershell.

Należy zainstalować [Azure PowerShell 1.0] [ lnk-powershell-install] lub nowszy, aby kontynuować.

Poniższe kroki pokazują jak skonfigurować uwierzytelnianie hasłem dla aplikacji AD przy użyciu programu PowerShell. Te polecenia można uruchamiać w sesji środowiska PowerShell standardowe.

1. Zaloguj się do otrzymywania za pomocą następującego polecenia:

    ```
    Login-AzureRmAccount
    ```

2. Zanotuj swój **TenantId** i **SubscriptionId**. Trzeba będzie je później.

3. Tworzenie nowej aplikacji usługi Active Directory Azure przy użyciu następujące polecenie, zastępując posiadaczy miejsce:

    - **{Nazwa wyświetlana}:** nazwę wyświetlaną dla aplikacji, takich jak **MySampleApp**
    - **{Adres URL strony głównej}:** adres URL strony głównej aplikacji, takich jak **http://mysampleapp/home**. Ten adres URL musi wskazywać prawdziwej aplikacji.
    - **{Identyfikator aplikacji}:** Identyfikator unikatowy takich jak **http://mysampleapp**. Ten adres URL musi wskazywać prawdziwej aplikacji.
    - **{Hasło}:** Hasło, które zostaną użyte do uwierzytelnienia z aplikacji.

    ```
    New-AzureRmADApplication -DisplayName {Display name} -HomePage {Home page URL} -IdentifierUris {Application identifier} -Password {Password}
    ```
    
4. Zanotuj **Identyfikator aplikacji** aplikacji został utworzony. Trzeba będzie to później.

5. Tworzenie nowej usługi głównej, za pomocą następujące polecenie, zastępując **{MyApplicationId}** **ApplicationId** w poprzednim kroku:

    ```
    New-AzureRmADServicePrincipal -ApplicationId {MyApplicationId}
    ```
    
6. Instalator przypisanie roli za pomocą następujące polecenie, zastępując **{MyApplicationId}** swój **Identyfikator aplikacji**.

    ```
    New-AzureRmRoleAssignment -RoleDefinitionName Owner -ServicePrincipalName {MyApplicationId}
    ```
    
Tworzenie aplikacji usługi Azure AD, która umożliwi użytkownikowi uwierzytelniać z niestandardowych aplikacji C# zostało zakończone. W dalszej części tego samouczka potrzebne są następujące wartości:

- TenantId
- SubscriptionId
- Identyfikator aplikacji
- Hasło

[lnk-authenticate-arm]: https://msdn.microsoft.com/library/azure/dn790557.aspx
[lnk-powershell-install]: ../articles/powershell-install-configure.md
