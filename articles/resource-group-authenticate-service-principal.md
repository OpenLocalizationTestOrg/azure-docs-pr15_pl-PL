<properties
   pageTitle="Tworzenie usługi Azure kapitału przy użyciu programu PowerShell | Microsoft Azure"
   description="W tym artykule opisano, jak za pomocą programu PowerShell Azure tworzenie aplikacji usługi Active Directory i usługi podstawowe i udostępnić go z zasobami za pośrednictwem kontrola dostępu oparta na rolach. Jest wyświetlany sposobów uwierzytelniania aplikacji za pomocą hasła lub certyfikatu."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="09/12/2016"
   ms.author="tomfitz"/>

# <a name="use-azure-powershell-to-create-a-service-principal-to-access-resources"></a>Tworzenie wystawcy usługi dostępu do zasobów przy użyciu programu PowerShell Azure

> [AZURE.SELECTOR]
- [Programu PowerShell](resource-group-authenticate-service-principal.md)
- [Polecenie Azure](resource-group-authenticate-service-principal-cli.md)
- [Portal](resource-group-create-service-principal-portal.md)

Jeśli aplikacja lub skrypt, który wymaga dostępu do zasobów, prawdopodobnie nie chcesz uruchomić ten proces z poświadczeniami użytkownika. Może mieć inne uprawnienia, które mają dla aplikacji, a nie chcesz aplikacji, aby kontynuować, przy użyciu swoich poświadczeń w przypadku zmiany jego obowiązków. Zamiast tego możesz utworzyć tożsamości dla aplikacji, która zawiera poświadczeń uwierzytelniania i przypisania roli. Każdorazowo po uruchomieniu aplikacji uwierzytelniające przy użyciu tych poświadczeń. W tym temacie opisano konfigurowanie wszystko, co jest potrzebne do uruchamiania w obszarze własne poświadczenia i tożsamości aplikacji przy użyciu [Programu PowerShell Azure](powershell-install-configure.md) .

Przy użyciu programu PowerShell masz dwie opcje uwierzytelniania aplikacji AD:

 - hasło
 - certyfikat

W tym temacie przedstawiono sposób za pomocą obu opcji w programie PowerShell. Jeśli zamierzasz logowania Azure z platformy programowej (takie Python, dopiskiem lub Node.js) uwierzytelniania hasła może być usługi najlepszym rozwiązaniem. Przed podjęciem decyzji o za pomocą hasła lub certyfikatu, zobacz sekcję [Przykładowe aplikacje](#sample-applications) przykłady uwierzytelniania w różnych ram.

## <a name="active-directory-concepts"></a>Pojęcia Active Directory

W tym artykule możesz utworzyć dwa obiekty — aplikacja Active Directory (AD) i głównej usługi. Aplikacja AD jest reprezentacją globalnej aplikacji. Zawiera on poświadczeń (identyfikator aplikacji i hasło albo certyfikat). Usługa kapitału jest lokalnej reprezentacji aplikacji w usłudze Active Directory. Zawiera przypisanie roli. W tym temacie omówiono aplikację dzierżawy jedno miejsce, w którym aplikacja jest przeznaczony do uruchamiania w obrębie organizacji tylko jeden. Zazwyczaj używać aplikacji pojedyncze dzierżawy dla aplikacji z LOB, uruchamianych w organizacji. W aplikacji pojedyncze dzierżawy masz jedną aplikację AD kapitału jedną usługę.

Być może zastanawiasz się — Dlaczego należy oba obiekty? Tej metody nabiera przy wyborze dzierżawy wielu aplikacji. Zazwyczaj jest używana dzierżawy wielu aplikacji dla aplikacji (władz akredytacji bezpieczeństwa) oprogramowania jako usług, gdy aplikacja działa w wielu różnych subskrypcjach. W przypadku wielu dzierżawy aplikacji masz jedną aplikację AD wielu podmiotów usługi (jeden w każdej usługi Active Directory dają dostęp do aplikacji). Aby skonfigurować aplikację wielu dzierżawy, zobacz [Przewodnik programisty do autoryzacji przy użyciu interfejsu API Menedżera zasobów Azure](resource-manager-api-authentication.md).

## <a name="required-permissions"></a>Wymagane uprawnienia

Aby wykonać w tym temacie, musisz mieć uprawnienia wystarczające zarówno usługi Azure Active Directory i subskrypcji usługi Azure. Należy w szczególności, można utworzyć aplikację w usłudze Active Directory i przypisać wartość kapitału usług do roli. 

Z usługi Active Directory konta, musisz być administratorem (na przykład **Administrator globalny** lub **Administrator użytkowników**). Jeśli Twoje konto jest przypisany do roli **użytkownika** , musisz poproś administratora o podniesienia poziomu uprawnień użytkownika.

W ramach subskrypcji, musisz mieć konto `Microsoft.Authorization/*/Write` programu access, której udzielono za pośrednictwem rola [właściciel](./active-directory/role-based-access-built-in-roles.md#owner) lub [Administrator dostępu użytkowników](./active-directory/role-based-access-built-in-roles.md#user-access-administrator) . Jeśli Twoje konto jest przypisane do roli **współautora** , otrzymujesz komunikat o błędzie, podczas próby przypisać wartość kapitału usług do roli. Ponownie administrator subskrypcji musi udzielić użytkownikowi odpowiednie prawa dostępu.

Teraz przejdź do sekcji uwierzytelniania [hasła](#create-service-principal-with-password) lub [certyfikatu](#create-service-principal-with-certificate) .

## <a name="create-service-principal-with-password"></a>Tworzenie usługi kapitału przy użyciu hasła

W tej sekcji należy wykonać procedurę:

- Tworzenie aplikacji AD przy użyciu hasła
- Tworzenie wystawcy usługi
- Przypisanie roli czytnika wystawcy usługi

Aby szybko wykonać poniższe czynności, zobacz następujące trzy polecenia cmdlet. 

     $app = New-AzureRmADApplication -DisplayName "{app-name}" -HomePage "https://{your-domain}/{app-name}" -IdentifierUris "https://{your-domain}/{app-name}" -Password "{your-password}"
     New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId
     New-AzureRmRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $app.ApplicationId.Guid

Przyjrzyjmy te kroki uważniej aby upewnić się, że wiesz, proces.

1. Zaloguj się do swojego konta.

        Add-AzureRmAccount

1. Tworzenie nowej aplikacji usługi Active Directory, dostarczając nazwę wyświetlaną identyfikatora URI, który opisuje aplikacji, identyfikatory URI identyfikować aplikacji, a następnie hasło dla tożsamości użytkownika aplikacji.

        $app = New-AzureRmADApplication -DisplayName "exampleapp" -HomePage "https://www.contoso.org/exampleapp" -IdentifierUris "https://www.contoso.org/exampleapp" -Password "<Your_Password>"

     Identyfikatory URI dla aplikacji pojedyncze dzierżawy, nie zostały zweryfikowane.
     
     Jeśli Twoje konto nie ma [wymagane uprawnienia](#required-permissions) w usłudze Active Directory, zobacz komunikat o błędzie wskazujący "Authentication_Unauthorized" lub "w kontekście odnaleziono nie subskrypcji".

1. Sprawdzić, czy nowy obiekt aplikacji. 

        $app
        
     Uwaga w szczególności właściwości **Identyfikator aplikacji** , co jest potrzebne do tworzenia usług podmioty, przypisania ról i uzyskiwanie token dostępu.

        DisplayName             : exampleapp
        ObjectId                : c95e67a3-403c-40ac-9377-115fa48f8f39
        IdentifierUris          : {https://www.contoso.org/example}
        HomePage                : https://www.contoso.org
        Type                    : Application
        ApplicationId           : 8bc80782-a916-47c8-a47e-4d76ed755275
        AvailableToOtherTenants : False
        AppPermissions          : 
        ReplyUrls               : {}

2. Tworzenie wystawcy usługi aplikacji przez przekazanie identyfikator aplikacji aplikacji usługi Active Directory.

        New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId

3. Udzielanie uprawnień głównej usługi od Twojej subskrypcji. W tym przykładzie możesz dodać do roli **czytnika** , co daje uprawnienia do odczytu wszystkich zasobów w subskrypcji wystawcy usługi. Dla pozostałych ról [RBAC: wbudowane role](./active-directory/role-based-access-built-in-roles.md). W przypadku parametru **ServicePrincipalName** podaj **Identyfikator aplikacji** , który został użyty podczas tworzenia aplikacji. 

        New-AzureRmRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $app.ApplicationId.Guid

    Jeśli Twoje konto nie ma uprawnień wystarczających do przypisywanie roli, pojawi się komunikat o błędzie. Komunikat informujący, usługi konto **nie ma autoryzacji do wykonania akcji "Microsoft.Authorization/roleAssignments/write" w zakresie "/ subskrypcje / {guid}"**. 

To wszystko! Do aplikacji AD i kapitału usług są skonfigurowane. Następnej sekcji pokazano, jak zalogować się przy użyciu poświadczeń przy użyciu programu PowerShell. Jeśli chcesz używać poświadczeń w aplikacji kod, można przejść bezpośrednio do [aplikacji przykładowych](#sample-applications). 

### <a name="provide-credentials-through-powershell"></a>Poświadczenia przy użyciu programu PowerShell

Teraz musisz zalogować się jako aplikację do wykonywania operacji.

1. Tworzenie obiektu **parametr PSCredential** zawierający poświadczenia, uruchamiając polecenie **Get-poświadczeń** . Potrzebujesz **Identyfikator aplikacji** przed uruchomieniem tego polecenia dlatego upewnij się, że masz to dostępny do wklejenia.

        $creds = Get-Credential

2. Zostanie wyświetlony monit o wprowadzenie poświadczeń. Nazwa użytkownika wybierz **Identyfikator aplikacji** , który został użyty podczas tworzenia aplikacji. Hasła użyj podanej podczas tworzenia konta.

     ![Wprowadź poświadczenia](./media/resource-group-authenticate-service-principal/arm-get-credential.png)

2. Gdy się zalogujesz się jako podmiot zabezpieczeń usługi, należy podać identyfikator dzierżawy katalogu dla aplikacji AD. Dzierżawy jest wystąpieniem usługi Active Directory. Jeśli masz tylko jedną subskrypcję, możesz użyć:

        $tenant = (Get-AzureRmSubscription).TenantId
    
     Jeśli masz więcej niż jedną subskrypcję, określ subskrypcji, w którym znajduje się usługi Active Directory. Aby uzyskać więcej informacji zobacz [jak Azure subskrypcje są skojarzone z usługi Azure Active Directory](./active-directory/active-directory-how-subscriptions-associated-directory.md).

        $tenant = (Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId

4. Zaloguj się jako wartość kapitału usługi, określając, że to konto jest wystawcy usługi i dostarcz obiektu poświadczeń. 

        Add-AzureRmAccount -Credential $creds -ServicePrincipal -TenantId $tenant
        
     Użytkownik jest teraz uwierzytelniony jako głównej usługi dla aplikacji usługi Active Directory, która została utworzona.

### <a name="save-access-token-to-simplify-log-in"></a>Zapisywanie token dostępu w celu uproszczenia Zaloguj

Aby uniknąć, dostarczając kapitału poświadczenia każdorazowo niezbędne do logowania, możesz zapisać token dostępu.

1. Aby użyć bieżącego tokenu dostępu w sesji później, Zapisz profilu.

        Save-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json
        
     Otwieranie profilu i sprawdź jego zawartość. Zwróć uwagę, że zawiera ona token dostępu. 
        
2. Zamiast ręcznie zalogować się ponownie później, po prostu załadować profilu.

        Select-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json
        
> [AZURE.NOTE] Token dostępu wygasa, więc za pomocą zapisanego profilu działa tylko dla, jak długo token jest prawidłowy.
        
## <a name="create-service-principal-with-certificate"></a>Tworzenie usługi kapitału z certyfikatem

W tej sekcji należy wykonać procedurę:

- Tworzenie certyfikatu z podpisem własnym
- Tworzenie aplikacji AD przy użyciu certyfikatu
- Tworzenie wystawcy usługi
- Przypisanie roli czytnika wystawcy usługi

Aby szybko wykonać poniższe czynności z Azure PowerShell 2.0 w systemie Windows 10 lub Windows Server 2016 Technical Preview, zobacz następujące polecenia cmdlet. 

    $cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=exampleapp" -KeySpec KeyExchange
    $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())
    $app = New-AzureRmADApplication -DisplayName "exampleapp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.contoso.org/example" -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore
    New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId
    New-AzureRmRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $app.ApplicationId.Guid

Przyjrzyjmy te kroki uważniej aby upewnić się, że wiesz, proces. W tym artykule przedstawiono również sposób wykonywać zadania w przypadku korzystania z wcześniejszych wersji programu Azure PowerShell lub systemy operacyjne.

### <a name="create-the-self-signed-certificate"></a>Tworzenie certyfikatu z podpisem własnym

Wersja programu PowerShell dostępne w przypadku systemu Windows 10 i Windows Server 2016 Technical Preview oferuje zaktualizowane polecenia cmdlet **Nowy SelfSignedCertificate** do wygenerowania certyfikat z podpisem własnym. Polecenia cmdlet New-SelfSignedCertificate masz starsze systemy operacyjne, ale nie oferuje parametry, potrzebne do tego tematu. Zamiast tego należy zaimportować moduł, który chcesz wygenerować certyfikatu. W tym temacie opisano obie metody do wygenerowania certyfikatu opartego na system operacyjny, czy zostały spełnione. 

- Jeśli używasz **systemu Windows 10 lub Windows Server 2016 Technical Preview**, uruchom następujące polecenie, aby utworzyć certyfikat z podpisem własnym: 

        $cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=exampleapp" -KeySpec KeyExchange
       
- Jeśli użytkownik **systemu Windows 10 lub Windows Server 2016 Technical Preview nie jest zainstalowany**, należy pobrać [podpisany generator certyfikat](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) z Microsoft Script Center. Wyodrębnianie jego zawartość i zaimportuj polecenia cmdlet, które są potrzebne.
     
        # Only run if you could not use New-SelfSignedCertificate
        Import-Module -Name c:\ExtractedModule\New-SelfSignedCertificateEx.ps1
    
     Następnie wygenerować certyfikatu.
    
        $cert = New-SelfSignedCertificateEx -Subject "CN=exampleapp" -KeySpec "Exchange" -FriendlyName "exampleapp"

Masz certyfikatu i można przystąpić do tworzenia aplikacji AD.

### <a name="create-the-active-directory-app-and-service-principal"></a>Tworzenie aplikacji usługi Active Directory i usługa kapitału

1. Pobieranie wartości klucza z certyfikatu.

        $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

2. Zaloguj się do swojego konta Azure.

        Add-AzureRmAccount

3. Tworzenie nowej aplikacji usługi Active Directory, dostarczając nazwę wyświetlaną identyfikatora URI, który opisuje aplikacji, identyfikatory URI identyfikować aplikacji, a następnie hasło dla tożsamości użytkownika aplikacji.

     Jeśli masz Azure PowerShell 2.0 (sierpień 2016 lub nowszy), użyj następującego polecenia cmdlet:

        $app = New-AzureRmADApplication -DisplayName "exampleapp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.contoso.org/example" -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore      
    
    Jeśli masz Azure PowerShell 1.0, użyj następującego polecenia cmdlet:
    
        $app = New-AzureRmADApplication -DisplayName "exampleapp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.contoso.org/example" -KeyValue $keyValue -KeyType AsymmetricX509Cert  -EndDate $cert.NotAfter -StartDate $cert.NotBefore      
    
    Identyfikatory URI dla aplikacji pojedyncze dzierżawy, nie zostały zweryfikowane.
    
    Jeśli Twoje konto nie ma [wymagane uprawnienia](#required-permissions) w usłudze Active Directory, zobacz komunikat o błędzie wskazujący "Authentication_Unauthorized" lub "w kontekście odnaleziono nie subskrypcji".
        
    Sprawdzić, czy nowy obiekt aplikacji. 

        $app

    Zwróć uwagę, właściwości **Identyfikator aplikacji** , co jest potrzebne do tworzenia podmioty usługi, przypisania ról i uzyskiwanie tokeny dostępu.

        DisplayName             : exampleapp
        ObjectId                : c95e67a3-403c-40ac-9377-115fa48f8f39
        IdentifierUris          : {https://www.contoso.org/example}
        HomePage                : https://www.contoso.org
        Type                    : Application
        ApplicationId           : 8bc80782-a916-47c8-a47e-4d76ed755275
        AvailableToOtherTenants : False
        AppPermissions          : 
        ReplyUrls               : {}


5. Tworzenie wystawcy usługi aplikacji przez przekazanie identyfikator aplikacji aplikacji usługi Active Directory.

        New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId

6. Udzielanie uprawnień głównej usługi od Twojej subskrypcji. W tym przykładzie możesz dodać do roli **czytnika** , co daje uprawnienia do odczytu wszystkich zasobów w subskrypcji wystawcy usługi. Dla pozostałych ról [RBAC: wbudowane role](./active-directory/role-based-access-built-in-roles.md). W przypadku parametru **ServicePrincipalName** podaj **Identyfikator aplikacji** , który został użyty podczas tworzenia aplikacji.

        New-AzureRmRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $app.ApplicationId.Guid

    Jeśli Twoje konto nie ma uprawnień wystarczających do przypisywanie roli, pojawi się komunikat o błędzie. Komunikat informujący, usługi konto **nie ma autoryzacji do wykonania akcji "Microsoft.Authorization/roleAssignments/write" w zakresie "/ subskrypcje / {guid}"**.

To wszystko! Do aplikacji AD i kapitału usług są skonfigurowane. Następnej sekcji pokazano, jak zalogować się przy użyciu certyfikatu przy użyciu programu PowerShell.

### <a name="provide-certificate-through-automated-powershell-script"></a>Podaj certyfikat w trybie automatycznego skrypt programu PowerShell

Gdy się zalogujesz się jako podmiot zabezpieczeń usługi, należy podać identyfikator dzierżawy katalogu dla aplikacji AD. Dzierżawy jest wystąpieniem usługi Active Directory. Jeśli masz tylko jedną subskrypcję, możesz użyć:

    $tenant = (Get-AzureRmSubscription).TenantId
    
Jeśli masz więcej niż jedną subskrypcję, określ subskrypcji, w którym znajduje się usługi Active Directory. Aby uzyskać więcej informacji zobacz [Administrowanie katalogu Azure AD](./active-directory/active-directory-administer.md).

    $tenant = (Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId

Uwierzytelnianie za pomocą skryptu, określ konto jest głównej usługi i zapewnić odcisku palca certyfikatu, identyfikator aplikacji i identyfikator dzierżawy. Aby zautomatyzować skrypt, można przechowywać te wartości jako zmienne środowiska i pobieranie ich podczas wykonywania lub można je uwzględnić w skrypt.

    Add-AzureRmAccount -ServicePrincipal -CertificateThumbprint $cert.Thumbprint -ApplicationId $app.ApplicationId -TenantId $tenant

Użytkownik jest teraz uwierzytelniony jako głównej usługi dla aplikacji usługi Active Directory, która została utworzona.

## <a name="sample-applications"></a>Przykładowe aplikacje

Następujące aplikacje przykładowych pokazano, jak logować się jako wystawcy usługi.

**.NET**

- [Wdrażanie SSH włączone maszyn wirtualnych za pomocą szablonu z .NET](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-template-deployment/)
- [Zarządzanie Azure zasobów i grup zasobów program .NET](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-resources-and-groups/)

**Java**

- [Wprowadzenie do zasobów — wdrażanie przy użyciu szablonu Azure Menedżera zasobów — w Java](https://azure.microsoft.com/documentation/samples/resources-java-deploy-using-arm-template/)
- [Wprowadzenie do zasobów — Zarządzanie grupa zasobów — w Java](https://azure.microsoft.com/documentation/samples/resources-java-manage-resource-group//)

**Python**

- [Wdrażanie SSH włączone maszyn wirtualnych za pomocą szablonu w Python](https://azure.microsoft.com/documentation/samples/resource-manager-python-template-deployment/)
- [Zarządzanie Azure zasobów i grup zasobów z Python](https://azure.microsoft.com/documentation/samples/resource-manager-python-resources-and-groups/)

**Node.js**

- [Wdrażanie SSH włączone maszyn wirtualnych za pomocą szablonu w Node.js](https://azure.microsoft.com/documentation/samples/resource-manager-node-template-deployment/)
- [Zarządzanie Azure zasobów i grup zasobów z Node.js](https://azure.microsoft.com/documentation/samples/resource-manager-node-resources-and-groups/)

**Dopiskiem**

- [Wdrażanie SSH włączone maszyn wirtualnych za pomocą szablonu w dopiskiem](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-template-deployment/)
- [Zarządzanie Azure zasobów i grup zasobów z dopiskiem](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)

## <a name="next-steps"></a>Następne kroki
  
- Aby uzyskać szczegółowe instrukcje na integrowanie aplikacji Azure zarządzania zasobami zobacz [Przewodnik po dewelopera do autoryzacji przy użyciu interfejsu API Menedżera zasobów Azure](resource-manager-api-authentication.md).
- Aby uzyskać bardziej szczegółowy opis głównych usług i aplikacji zobacz [obiekty aplikacji i usług kapitału](./active-directory/active-directory-application-objects.md). 
- Aby uzyskać więcej informacji na temat uwierzytelniania usługi Active Directory zobacz [Scenariusze uwierzytelniania Azure AD](./active-directory/active-directory-authentication-scenarios.md).



