<properties
   pageTitle="Tworzenie usługi kapitału z polecenie Azure | Microsoft Azure"
   description="Informacje dotyczące używania Azure interfejsu wiersza polecenia do tworzenia aplikacji usługi Active Directory i usługi podstawowe i udostępnić go z zasobami za pośrednictwem kontrola dostępu oparta na rolach. Jest wyświetlany sposobów uwierzytelniania aplikacji za pomocą hasła lub certyfikatu."
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
   ms.date="09/30/2016"
   ms.author="tomfitz"/>

# <a name="use-azure-cli-to-create-a-service-principal-to-access-resources"></a>Tworzenie wystawcy usługi dostępu do zasobów za pomocą interfejsu wiersza polecenia Azure

> [AZURE.SELECTOR]
- [Programu PowerShell](resource-group-authenticate-service-principal.md)
- [Polecenie Azure](resource-group-authenticate-service-principal-cli.md)
- [Portal](resource-group-create-service-principal-portal.md)


Jeśli aplikacja lub skrypt, który wymaga dostępu do zasobów, prawdopodobnie nie chcesz uruchomić ten proces z poświadczeniami użytkownika. Może mieć inne uprawnienia, które mają dla aplikacji, a nie chcesz aplikacji, aby kontynuować, przy użyciu swoich poświadczeń w przypadku zmiany jego obowiązków. Zamiast tego możesz utworzyć tożsamości dla aplikacji, która zawiera poświadczeń uwierzytelniania i przypisania roli. Każdorazowo po uruchomieniu aplikacji uwierzytelniające przy użyciu tych poświadczeń. W tym temacie pokazano, jak skonfigurować aplikację do uruchamiania w obszarze własne poświadczenia i tożsamości za pomocą [Azure interfejsu wiersza polecenia dla komputerów Mac, Linux i systemu Windows](xplat-cli-install.md) .

Przy użyciu interfejsu wiersza polecenia Azure masz dwie opcje uwierzytelniania aplikacji AD:

 - hasło
 - certyfikat

W tym temacie przedstawiono sposób w polecenie Azure za pomocą obu opcji. Jeśli zamierzasz logowania Azure z platformy programowej (takie Python, dopiskiem lub Node.js) uwierzytelniania hasła może być usługi najlepszym rozwiązaniem. Przed podjęciem decyzji o za pomocą hasła lub certyfikatu, zobacz sekcję [Przykładowe aplikacje](#sample-applications) przykłady uwierzytelniania w różnych ram.

## <a name="active-directory-concepts"></a>Pojęcia Active Directory

W tym artykule możesz utworzyć dwa obiekty — aplikacja Active Directory (AD) i głównej usługi. Aplikacja AD jest reprezentacją globalnej aplikacji. Zawiera on poświadczeń (identyfikator aplikacji i hasło albo certyfikat). Usługa kapitału jest lokalnej reprezentacji aplikacji w usłudze Active Directory. Zawiera przypisanie roli. W tym temacie omówiono aplikację dzierżawy jedno miejsce, w którym aplikacja jest przeznaczony do uruchamiania w obrębie organizacji tylko jeden. Zazwyczaj używać aplikacji pojedyncze dzierżawy dla aplikacji z LOB, uruchamianych w organizacji. W aplikacji pojedyncze dzierżawy masz jedną aplikację AD kapitału jedną usługę.

Być może zastanawiasz się — Dlaczego należy oba obiekty? Tej metody nabiera przy wyborze dzierżawy wielu aplikacji. Zazwyczaj jest używana dzierżawy wielu aplikacji dla aplikacji (władz akredytacji bezpieczeństwa) oprogramowania jako usług, gdy aplikacja działa w wielu różnych subskrypcjach. W przypadku wielu dzierżawy aplikacji masz jedną aplikację AD wielu podmiotów usługi (jeden w każdej usługi Active Directory dają dostęp do aplikacji). Aby skonfigurować aplikację wielu dzierżawy, zobacz [Przewodnik programisty do autoryzacji przy użyciu interfejsu API Menedżera zasobów Azure](resource-manager-api-authentication.md).

## <a name="required-permissions"></a>Wymagane uprawnienia

Aby wykonać w tym temacie, musisz mieć uprawnienia wystarczające zarówno usługi Azure Active Directory i subskrypcji usługi Azure. Należy w szczególności, można utworzyć aplikację w usłudze Active Directory i przypisać wartość kapitału usług do roli. 

Z usługi Active Directory konta, musisz być administratorem (na przykład **Administrator globalny** lub **Administrator użytkowników**). Jeśli Twoje konto jest przypisany do roli **użytkownika** , musisz poproś administratora o podniesienia poziomu uprawnień użytkownika.

W ramach subskrypcji, musisz mieć konto `Microsoft.Authorization/*/Write` programu access, której udzielono za pośrednictwem rola [właściciel](./active-directory/role-based-access-built-in-roles.md#owner) lub [Administrator dostępu użytkowników](./active-directory/role-based-access-built-in-roles.md#user-access-administrator) . Jeśli Twoje konto jest przypisane do roli **współautora** , otrzymujesz komunikat o błędzie, podczas próby przypisać wartość kapitału usług do roli. Ponownie administrator subskrypcji musi udzielić użytkownikowi odpowiednie prawa dostępu.

Teraz przejdź do sekcji uwierzytelniania [hasła](#create-service-principal-with-password) lub [certyfikatu](#create-service-principal-with-certificate) .

## <a name="create-service-principal-with-password"></a>Utwórz usługę kapitału przy użyciu hasła

W tej sekcji wykonaj czynności, aby utworzyć aplikację AD przy użyciu hasła, a przypisanie roli czytnika głównej usługi.

Przyjrzyjmy się te kroki.

1. Zaloguj się do swojego konta.

        azure login

1. Istnieją dwie opcje dotyczące tworzenia aplikacji AD. Możesz utworzyć aplikację AD i usługi kapitału w jednym kroku lub są tworzone oddzielnie. Utwórz je w jednym kroku, jeśli nie ma potrzeby określ strony głównej i identyfikator URI aplikacji. Tworzenie ich oddzielnie Jeśli chcesz ustawić te wartości dla aplikacji sieci web. Obie opcje są wyświetlane w tym kroku.

     - Aby utworzyć kapitału w jednym kroku AD aplikacji i usług, należy podać nazwę aplikacji i hasło, jak pokazano na następujące polecenie:
     
            azure ad sp create -n exampleapp -p {your-password}     
     
     - Aby utworzyć aplikację AD oddzielnie, zapewniają nazwę aplikacji, strona główna identyfikatora URI, identyfikator URI i hasło, jak pokazano w następujące polecenie:
     
            azure ad app create -n exampleapp --home-page http://www.contoso.org --identifier-uris https://www.contoso.org/example -p <Your_Password>

         Poprzednie polecenie zwraca wartość Identyfikator aplikacji. Aby utworzyć wystawcy usługi, wprowadź tę wartość jako parametr w następujące polecenie:
     
            azure ad sp create -a <AppId>
     
     Jeśli Twoje konto nie ma [wymagane uprawnienia](#required-permissions) w usłudze Active Directory, zobacz komunikat o błędzie wskazujący "Authentication_Unauthorized" lub "w kontekście odnaleziono nie subskrypcji".
    
     Nowy podmiot usługi jest zwracany dla obu opcji. **Identyfikator obiektu** jest potrzebna podczas udzielanie uprawnień. Identyfikator guid wymieniony **Główne nazwy usług** jest potrzebna podczas logowania. Ten identyfikator guid jest tę samą wartość co identyfikator aplikacji. W aplikacjach przykładowych ta wartość jest określane jako **Identyfikator klienta**. 
    
        info:    Executing command ad sp create
        + Creating application exampleapp
        / Creating service principal for application 7132aca4-1bdb-4238-ad81-996ff91d8db+
        data:    Object Id:               ff863613-e5e2-4a6b-af07-fff6f2de3f4e
        data:    Display Name:            exampleapp
        data:    Service Principal Names:
        data:                             7132aca4-1bdb-4238-ad81-996ff91d8db4
        data:                             https://www.contoso.org/example
        info:    ad sp create command OK

2. Udzielanie uprawnień głównej usługi od Twojej subskrypcji. W tym przykładzie możesz dodać do roli **czytnika** , co daje uprawnienia do odczytu wszystkich zasobów w subskrypcji wystawcy usługi. Dla pozostałych ról [RBAC: wbudowane role](./active-directory/role-based-access-built-in-roles.md). W przypadku parametru **ServicePrincipalName** podaj **identyfikator obiektu** , który został użyty podczas tworzenia aplikacji. 

        azure role assignment create --objectId ff863613-e5e2-4a6b-af07-fff6f2de3f4e -o Reader -c /subscriptions/{subscriptionId}/

     Jeśli Twoje konto nie ma uprawnień wystarczających do przypisywanie roli, pojawi się komunikat o błędzie. Komunikat informujący, usługi konto **nie ma autoryzacji do wykonania akcji "Microsoft.Authorization/roleAssignments/write" w zakresie "/ subskrypcje / {guid}"**. 

To wszystko! Do aplikacji AD i wystawcy usługi są skonfigurowane. Następnej sekcji pokazano, jak zalogować się przy użyciu poświadczeń za pośrednictwem interfejsu wiersza polecenia Azure. Jeśli chcesz używać poświadczeń w aplikacji kod, nie musisz kontynuować w tym temacie. Można przejść bezpośrednio do [aplikacji przykładowych](#sample-applications) przykłady logowaniem się przy użyciu swojego identyfikatora aplikacji i hasła. 

### <a name="provide-credentials-through-azure-cli"></a>Poświadczenia za pośrednictwem interfejsu wiersza polecenia Azure

Teraz musisz zalogować się jako aplikację do wykonywania operacji.

1. Gdy się zalogujesz się jako podmiot zabezpieczeń usługi, należy podać identyfikator dzierżawy katalogu dla aplikacji AD. Dzierżawy jest wystąpieniem usługi Active Directory. Aby pobrać identyfikator dzierżawy dla subskrypcji uwierzytelnionym, należy użyć:

        azure account show

     Która zwraca:

        info:    Executing command account show
        data:    Name                        : Windows Azure MSDN - Visual Studio Ultimate
        data:    ID                          : {guid}
        data:    State                       : Enabled
        data:    Tenant ID                   : {guid}
        data:    Is Default                  : true
        ...

     Jeśli jest potrzebne do uzyskania identyfikator dzierżawy innej subskrypcji, użyj następującego polecenia:

        azure account show -s {subscription-id}

2. Jeśli chcesz pobrać identyfikator klienta, aby używać do logowania, użyj:

        azure ad sp show -c exampleapp --json

     Wartość używana do logowania jest identyfikator guid wymieniony w polu nazwy głównej usługi.

        [
          {
            "objectId": "ff863613-e5e2-4a6b-af07-fff6f2de3f4e",
            "objectType": "ServicePrincipal",
            "displayName": "exampleapp",
            "appId": "7132aca4-1bdb-4238-ad81-996ff91d8db4",
            "servicePrincipalNames": [
              "https://www.contoso.org/example",
              "7132aca4-1bdb-4238-ad81-996ff91d8db4"
            ]
          }
        ]

3. Zaloguj się jako podstawowe usługi.

        azure login -u 7132aca4-1bdb-4238-ad81-996ff91d8db4 --service-principal --tenant {tenant-id}

    Zostanie wyświetlony monit o podanie hasła. Podaj hasło, które zostało określone podczas tworzenia aplikacji AD.

        info:    Executing command login
        Password: ********

Użytkownik jest teraz uwierzytelniony jako podstawowe usługi dla podmiotu usługi, który został utworzony.

## <a name="create-service-principal-with-certificate"></a>Tworzenie usługi kapitału z certyfikatem

W tej sekcji należy wykonać procedurę:

- Tworzenie certyfikatu z podpisem własnym
- Tworzenie aplikacji AD przy użyciu certyfikatu i wystawcy usługi
- Przypisanie roli czytnika wystawcy usługi

Aby wykonać te kroki, musisz mieć [OpenSSL](http://www.openssl.org/) zainstalowany.

1. Tworzenie certyfikatu z podpisem własnym.

        openssl req -x509 -days 3650 -newkey rsa:2048 -out cert.pem -nodes -subj '/CN=exampleapp'

2. Łączenie klucze publiczne i prywatne.

        cat privkey.pem cert.pem > examplecert.pem

3. Otwórz plik **examplecert.pem** i odszukaj długiej sekwencji znaków między **---rozpoczęcia certyfikat---** i **---zakończyć certyfikat---**. Skopiuj dane certyfikatu. Należy przekazać te dane jako parametr, podczas tworzenia głównej usługi.

1. Zaloguj się do swojego konta.

        azure login

1. Istnieją dwie opcje dotyczące tworzenia aplikacji AD. Możesz utworzyć aplikację AD i usługi kapitału w jednym kroku lub są tworzone oddzielnie. Utwórz je w jednym kroku, jeśli nie ma potrzeby określ strony głównej i identyfikator URI aplikacji. Tworzenie ich oddzielnie Jeśli chcesz ustawić te wartości dla aplikacji sieci web. Obie opcje są wyświetlane w tym kroku.

     - Aby utworzyć kapitału w jednym kroku AD aplikacji i usług, zapewniają nazwy aplikacji i dane certyfikatu, jak pokazano w następujące polecenie:
     
            azure ad sp create -n exampleapp --cert-value <certificate data>
     
     - Aby utworzyć aplikację AD oddzielnie, zapewniają nazwę aplikacji, strona główna identyfikatora URI, identyfikator URI i dane certyfikatu, jak pokazano w następujące polecenie:
     
            azure ad app create -n exampleapp --home-page http://www.contoso.org --identifier-uris https://www.contoso.org/example --cert-value <certificate data>

         Poprzednie polecenie zwraca wartość Identyfikator aplikacji. Aby utworzyć wystawcy usługi, wprowadź tę wartość jako parametr w następujące polecenie:
     
            azure ad sp create -a <AppId>
  
     Jeśli Twoje konto nie ma [wymagane uprawnienia](#required-permissions) w usłudze Active Directory, zobacz komunikat o błędzie wskazujący "Authentication_Unauthorized" lub "w kontekście odnaleziono nie subskrypcji".
    
     Nowy podmiot usługi jest zwracany dla obu opcji. Identyfikator obiektu jest potrzebna podczas udzielanie uprawnień. Identyfikator guid wymieniony **Główne nazwy usług** jest potrzebna podczas logowania. Ten identyfikator guid jest tę samą wartość co identyfikator aplikacji. W aplikacjach przykładowych ta wartość jest określane jako **Identyfikator klienta**. 
    
        info:    Executing command ad sp create
        - Creating service principal for application 4fd39843-c338-417d-b549-a545f584a74+
        data:    Object Id:        7dbc8265-51ed-4038-8e13-31948c7f4ce7
        data:    Display Name:     exampleapp
        data:    Service Principal Names:
        data:                      4fd39843-c338-417d-b549-a545f584a745
        data:                      https://www.contoso.org/example
        info:    ad sp create command OK
        
2. Udzielanie uprawnień głównej usługi od Twojej subskrypcji. W tym przykładzie możesz dodać do roli **czytnika** , co daje uprawnienia do odczytu wszystkich zasobów w subskrypcji wystawcy usługi. Dla pozostałych ról [RBAC: wbudowane role](./active-directory/role-based-access-built-in-roles.md). W przypadku parametru **ServicePrincipalName** podaj **identyfikator obiektu** , który został użyty podczas tworzenia aplikacji. 

        azure role assignment create --objectId 7dbc8265-51ed-4038-8e13-31948c7f4ce7 -o Reader -c /subscriptions/{subscriptionId}/

     Jeśli Twoje konto nie ma uprawnień wystarczających do przypisywanie roli, pojawi się komunikat o błędzie. Komunikat informujący, usługi konto **nie ma autoryzacji do wykonania akcji "Microsoft.Authorization/roleAssignments/write" w zakresie "/ subskrypcje / {guid}"**. 

### <a name="provide-certificate-through-automated-azure-cli-script"></a>Podać certyfikatu za pomocą zautomatyzowanego skryptu polecenie Azure

Teraz musisz zalogować się jako aplikację do wykonywania operacji.

1. Gdy się zalogujesz się jako podmiot zabezpieczeń usługi, należy podać identyfikator dzierżawy katalogu dla aplikacji AD. Dzierżawy jest wystąpieniem usługi Active Directory. Aby pobrać identyfikator dzierżawy dla subskrypcji uwierzytelnionym, należy użyć:

        azure account show

     Która zwraca:

        info:    Executing command account show
        data:    Name                        : Windows Azure MSDN - Visual Studio Ultimate
        data:    ID                          : {guid}
        data:    State                       : Enabled
        data:    Tenant ID                   : {guid}
        data:    Is Default                  : true
        ...

     Jeśli jest potrzebne do uzyskania identyfikator dzierżawy innej subskrypcji, użyj następującego polecenia:

        azure account show -s {subscription-id}

1. Aby pobrać odcisku palca certyfikatu i usuń zbędne znaki, należy użyć:

        openssl x509 -in "C:\certificates\examplecert.pem" -fingerprint -noout | sed 's/SHA1 Fingerprint=//g'  | sed 's/://g'
    
     Która zwraca wartość odcisku palca są podobne do:

        30996D9CE48A0B6E0CD49DBB9A48059BF9355851

2. Jeśli chcesz pobrać identyfikator klienta, aby używać do logowania, użyj:

        azure ad sp show -c exampleapp

     Wartość używana do logowania jest identyfikator guid wymieniony w polu nazwy głównej usługi.

        [
          {
            "objectId": "7dbc8265-51ed-4038-8e13-31948c7f4ce7",
            "objectType": "ServicePrincipal",
            "displayName": "exampleapp",
            "appId": "4fd39843-c338-417d-b549-a545f584a745",
            "servicePrincipalNames": [
              "https://www.contoso.org/example",
              "4fd39843-c338-417d-b549-a545f584a745"
            ]
          }
        ]

1. Zaloguj się jako podstawowe usługi.

        azure login --service-principal --tenant {tenant-id} -u 4fd39843-c338-417d-b549-a545f584a745 --certificate-file C:\certificates\examplecert.pem --thumbprint {thumbprint}

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
- Aby uzyskać więcej informacji na temat korzystania z certyfikatów i polecenie Azure, zobacz [uwierzytelniania opartego na certyfikat z głównych usługi Azure z wiersza polecenia Linux](http://blogs.msdn.com/b/arsen/archive/2015/09/18/certificate-based-auth-with-azure-service-principals-from-linux-command-line.aspx). 
