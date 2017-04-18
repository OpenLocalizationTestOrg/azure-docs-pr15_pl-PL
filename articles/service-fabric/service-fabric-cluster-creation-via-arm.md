
<properties
   pageTitle="Tworzenie klaster tkaninie usługi bezpiecznego za pomocą Menedżera zasobów Azure | Microsoft Azure"
   description="W tym artykule opisano, jak skonfigurować klaster tkaninie usługi bezpiecznego platformy Azure przy użyciu Menedżera zasobów Azure, Azure klucza magazynu i Azure Active Directory (AAD) uwierzytelniania klienta."
   services="service-fabric"
   documentationCenter=".net"
   authors="chackdan"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/25/2016"
   ms.author="vturecek"/>

# <a name="create-a-service-fabric-cluster-in-azure-using-azure-resource-manager"></a>Utworzyć klaster tkaninie usługi platformy Azure za pomocą Menedżera zasobów Azure

> [AZURE.SELECTOR]
- [Azure Menedżera zasobów](service-fabric-cluster-creation-via-arm.md)
- [Azure portal](service-fabric-cluster-creation-via-portal.md)

Jest to przewodnik krok po kroku, który przeprowadzi Cię przez kroki konfigurowania klastrze tkaninie usługi Azure bezpiecznej platformy Azure za pomocą Menedżera zasobów Azure. Ten przewodnik przeprowadzi Cię przez następujące czynności:

 - Konfigurowanie magazynu klucz do zarządzania klawisze służące do zabezpieczeń klaster i aplikacji.
 - Tworzenie zabezpieczonego klaster w Azure przy użyciu Menedżera zasobów Azure.
 - Uwierzytelniania użytkowników z Azure Active Directory (AAD) do zarządzania klastrem.

Klaster bezpiecznego jest zapobiega nieupoważnionemu dostępowi do operacji zarządzania tym wdrażanie, uaktualnianie i usuwanie aplikacji, usług oraz dane, które zawierają klastrze. Klaster niezabezpieczoną jest klaster, że każda osoba można łączyć się w dowolnym momencie i wykonywać operacje zarządzania. Umożliwia utworzenie niezabezpieczoną klaster, ale jest **zdecydowanie zalecane, aby utworzyć klaster bezpiecznego**. Klaster niezabezpieczoną **nie może zostać wysłana później** — można utworzyć nowy klaster.

Pojęcia są takie same związane z tworzeniem bezpiecznego klastrów, czy klastrów są klastrów Linux lub grupy systemu Windows. Aby uzyskać więcej informacji i Pomocnik skrypty do tworzenia bezpiecznego klastrów Linux zobacz [Tworzenie bezpiecznego klastrów w systemie Linux](#secure-linux-clusters)

## <a name="log-in-to-azure"></a>Zaloguj się do Azure
Ten przewodnik używa [Programu PowerShell Azure][azure-powershell]. Po rozpoczęciu nowej sesji programu PowerShell, zaloguj się do konta Azure i wybierz subskrypcję przed wykonaniem polecenia Azure.

Zaloguj się do konta azure:

```powershell
Login-AzureRmAccount
```

Wybierz subskrypcję:

```powershell
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

## <a name="set-up-key-vault"></a>Konfigurowanie magazynu klucza

W tej sekcji opisano tworzenie magazynu klucz dla klastrów tkaninie usługi platformy Azure i dla aplikacji usługi tkaninie. Aby uzyskać pełne informacje na magazynu klucza odwołują się do [magazynu klucza przewodnik Wprowadzenie do][key-vault-get-started].

Usługa tkaninie używa certyfikaty X.509 do bezpiecznego klastrze i zapewniają funkcje zabezpieczeń aplikacji. Azure klucza magazynu jest używana do zarządzania certyfikaty dla klastrów tkaninie usługi platformy Azure. Po wdrożeniu klastrze platformy Azure odpowiedzialny za tworzenie klastrów tkaninie usługi dostawcy Azure zasobów pobiera certyfikaty z magazynu klucza i instaluje je w klastrze maszyny wirtualne.

Poniższy diagram przedstawia relacje między magazynu klucza, klaster tkaninie usługi i dostawcę Azure zasobów, który używa certyfikatów przechowywanych w magazynu klawisz podczas tworzenia klastrze:

![Instalacja certyfikatu][cluster-security-cert-installation]

### <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Pierwszym krokiem jest utworzenie grupa zasobów dla magazynu klucza. Zaleca się wprowadzanie klucza magazynu do własnej grupy zasobów. Pozwala usunąć grup zasobów obliczeń i miejsca do magazynowania, łącznie z Grupa zasobów, zawierającą klaster tkaninie usługi bez utraty klucze i hasła usługi. Grupa zasobów, zawierającą z magazynu klucza musi być w tym samym regionie jako klaster, który jest używany.

```powershell

    New-AzureRmResourceGroup -Name mycluster-keyvault -Location 'West US'
    WARNING: The output object type of this cmdlet is going to be modified in a future release.
    
    ResourceGroupName : mycluster-keyvault
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/<guid>/resourceGroups/mycluster-keyvault

```

### <a name="create-key-vault"></a>Tworzenie magazynu klucza 

Tworzenie magazynu klucza w nowej grupy zasobów. Klucz magazynu **musi być włączona na potrzeby wdrożenia** do obsługi dostawcy zasobu usługi tkaninie uzyskiwania certyfikatów z go i zainstaluj na węzłach:

```powershell

    New-AzureRmKeyVault -VaultName 'myvault' -ResourceGroupName 'mycluster-keyvault' -Location 'West US' -EnabledForDeployment
    
    
    Vault Name                       : myvault
    Resource Group Name              : mycluster-keyvault
    Location                         : West US
    Resource ID                      : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault
    Vault URI                        : https://myvault.vault.azure.net
    Tenant ID                        : <guid>
    SKU                              : Standard
    Enabled For Deployment?          : False
    Enabled For Template Deployment? : False
    Enabled For Disk Encryption?     : False
    Access Policies                  :
                                       Tenant ID                :    <guid>
                                       Object ID                :    <guid>
                                       Application ID           :
                                       Display Name             :    
                                       Permissions to Keys      :    get, create, delete, list, update, import, backup, restore
                                       Permissions to Secrets   :    all
    
    
    Tags                             :
```

Jeśli masz istniejące magazynu klucza, możesz ją włączyć wdrożenie przy użyciu interfejsu wiersza polecenia Azure:

```cli
> azure login
> azure account set "your account"
> azure config mode arm 
> azure keyvault list
> azure keyvault set-policy --vault-name "your vault name" --enabled-for-deployment true
```

<a id="add-certificate-to-key-vault"></a>
## <a name="add-certificates-to-key-vault"></a>Dodawanie certyfikatów do magazynu klucza

Certyfikaty są używane w tkaninie usługi zapewniające uwierzytelniania i szyfrowania do zabezpieczenia różnych aspektów klastrze i jego aplikacji. Aby uzyskać więcej informacji dotyczących używania certyfikatów w tkaninie usługi, zobacz [Usługa tkaninie klaster zabezpieczeń scenariusze][service-fabric-cluster-security].

### <a name="cluster-and-server-certificate-required"></a>Certyfikat serwera i klaster (wymagany) 

Ten certyfikat jest wymagane do zabezpieczania klastrze i zapobieganie nieupoważnionemu dostępowi do niego. Zapewnia bezpieczeństwo klaster na kilka sposobów:
 
 - **Klaster uwierzytelniania:** Uwierzytelnianie komunikacji węzeł węzeł do Federacji klaster. Tylko węzły, które mogą okazać się jego tożsamości z ten certyfikat można dołączyć klaster.
 - **Uwierzytelniania serwera:** Uwierzytelnia punkty końcowe zarządzania klaster na kliencie zarządzania, tak aby klient zarządzania wie, że aktualnie mówi do klastrów rzeczywistą. Ten certyfikat zawiera również SSL do zarządzania HTTPS interfejsu API i usługa tkaninie Eksploratora, za pomocą protokołu HTTPS.

Aby służyć do tych celów, certyfikat musi spełniać następujące wymagania:

 - Certyfikat musi zawierać klucz prywatny.
 - Certyfikat muszą zostać utworzone dla wymiany kluczy, można wyeksportować do pliku wymiana informacji osobistych (pfx).
 - Nazwa podmiotu certyfikatu musi odpowiadać domeny używanego do uzyskiwania dostępu klaster tkaninie usługi. Ten matchng jest wymagana do zapewnienia protokołu SSL dla punktów końcowych zarządzania HTTPS i Explorer tkaninie usługi klaster. Nie można uzyskać certyfikat SSL od urzędu certyfikacji (CA) dla `.cloudapp.azure.com` domeny. Należy uzyskać niestandardowej nazwy domeny dla klaster. Gdy żądania certyfikatu od urzędu certyfikacji, nazwa podmiotu certyfikatu musi odpowiadać niestandardowej nazwy domeny na potrzeby klaster.

### <a name="application-certificates-optional"></a>Certyfikaty aplikacji (opcjonalnie)

Dowolna liczba dodatkowych certyfikatów można zainstalować w klastrze ze względów bezpieczeństwa aplikacji. Przed utworzeniem klaster, należy rozważyć scenariusze zabezpieczeń aplikacji, które wymagają certyfikatu musi być zainstalowany na węzłach, takich jak:

 - Szyfrowanie i odszyfrowywanie wartości konfiguracji aplikacji
 - Szyfrowanie danych w węzłach podczas replikacji 

### <a name="formatting-certificates-for-azure-resource-provider-use"></a>Certyfikaty dla zasobu Azure dostawcy Użyj formatowania

Pliki kluczy prywatnych (pfx) mogą być dodawane lub używane bezpośrednio za pomocą klucza magazynu. Jednak dostawcy Azure zasobów wymaga kluczy mają być przechowywane w formacie JSON specjalny, który zawiera PFX jako base 64 zakodowany ciągu oraz hasło klucza prywatnego. Aby uwzględnić następujące wymagania, klawiszy należy umieszczony w ciągu JSON i następnie przechowywane jako *hasła* klucza magazynu.

Aby ułatwić ten proces, modułu programu PowerShell jest [dostępny na GitHub][service-fabric-rp-helpers]. Wykonaj poniższe czynności, aby użyć modułu:

  1. Pobierz całą zawartość repo do katalogu lokalnego. 
  2. Zaimportuj moduł w oknie programu PowerShell:

  ```powershell
  PS C:\Users\vturecek> Import-Module "C:\users\vturecek\Documents\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1"
  ```
     
`Invoke-AddCertToKeyVault` Polecenia w tym modułu programu PowerShell automatycznie formatuje klucz prywatny certyfikatu do ciągu JSON i przekazuje go do magazynu klucza. Dodawanie certyfikatu klaster i wszelkie dodatkowe aplikacji certyfikaty do magazynu klucza za pomocą go. Powtórz ten krok dla dodatkowych certyfikatów, które chcesz zainstalować w klastrze.

```powershell
 Invoke-AddCertToKeyVault -SubscriptionId <guid> -ResourceGroupName mycluster-keyvault -Location "West US" -VaultName myvault -CertificateName mycert -Password "<password>" -UseExistingCertificate -ExistingPfxFilePath "C:\path\to\mycertkey.pfx"
    
    Switching context to SubscriptionId <guid>
    Ensuring ResourceGroup mycluster-keyvault in West US
    WARNING: The output object type of this cmdlet is going to be modified in a future release.
    Using existing valut myvault in West US
    Reading pfx file from C:\path\to\key.pfx
    Writing secret to myvault in vault myvault
    
    
Name  : CertificateThumbprint
Value : <value>

Name  : SourceVault
Value : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault

Name  : CertificateURL
Value : https://myvault.vault.azure.net:443/secrets/mycert/4d087088df974e869f1c0978cb100e47

```

Poprzedni ciągi są wszystkie klucza magazynu wymaganiach wstępnych konfigurowania szablonu Menedżera zasobów klaster tkaninie usługa, która jest instalowana certyfikatów do uwierzytelniania węzeł, zarządzania punkt końcowy zabezpieczeń i uwierzytelniania i dowolnej aplikacji dodatkowe funkcje zabezpieczeń, używające certyfikatów X.509. W tym momencie powinny być następujące ustawienia platformy Azure:

 - Grupa zasobów magazynu klucza
   - Kluczowe magazynu
     - Klaster certyfikatu uwierzytelniania serwera
     - Certyfikaty aplikacji

## <a name="set-up-azure-active-directory-for-client-authentication"></a>Konfigurowanie usługi Azure Active Directory uwierzytelniania klienta

AAD umożliwia zarządzanie dostępem użytkowników do aplikacji, które są podzielone na aplikacji za pomocą logowania oparte na sieci web interfejs użytkownika i aplikacji klasy klientami organizacji (nazywanego dzierżaw). W tym dokumencie przyjęto założenie, że zostały już utworzone dzierżawy. Jeśli nie, Rozpocznij od przeczytania [sposób uzyskiwania dzierżawy usługi Azure Active Directory][active-directory-howto-tenant].

Klaster tkaninie usługa oferuje kilka punktów wejścia do jego funkcji zarządzania, między innymi oparte na sieci web [Usługi tkaninie Eksploratora] [ service-fabric-visualizing-your-cluster] i [Visual Studio][service-fabric-manage-application-in-visual-studio]. W wyniku możesz utworzyć dwie aplikacje AAD kontrolowanie dostępu do klaster, jedna aplikacja sieci web i jeden natywnej aplikacji.

Aby uprościć niektóre procedura konfigurowania AAD z klastrem tkaninie usługi, został utworzony zestaw skrypty środowiska Windows PowerShell.

>[AZURE.NOTE] Należy wykonać te kroki *przed* utworzeniem klaster, w przypadku której skrypty oczekiwać nazwy klaster i punkty końcowe, powinny być planowane wartości, nie obramowania, które zostały już utworzone.

1. [Pobrać skrypty] [ sf-aad-ps-script-download] z komputerem.

2. Kliknij prawym przyciskiem myszy plik zip, wybierz polecenie **Właściwości**, a następnie zaznacz pole wyboru **Odblokuj** i Zastosuj.

3. Wyodrębnianie pliku zip.

4. Uruchamianie `SetupApplications.ps1`, dostarczając TenantId, NazwaKlastra i WebApplicationReplyUrl jako parametrów. Na przykład:

    ```powershell
    .\SetupApplications.ps1 -TenantId '690ec069-8200-4068-9d01-5aaf188e557a' -ClusterName 'mycluster' -WebApplicationReplyUrl 'https://mycluster.westus.cloudapp.azure.com:19080/Explorer/index.html'
    ```

    Możesz znaleźć swojego **TenantId** przez wykonanie polecenia programu PowerShell "Get-AzureSubscription" ". Spowoduje to wyświetlenie **TenantId** dla każdej subskrypcji.

    **NazwaKlastra** służy do prefiks aplikacji AAD utworzone przez skrypt. Nie musi być zgodna z nazwą rzeczywisty klaster dokładnie tak, jak jest przeznaczona tylko do ułatwić mapowanie artefakty AAD klaster tkaninie usługi, który jest używany z.

    **WebApplicationReplyUrl** jest domyślny punkt końcowy zwracające AAD użytkowników po ukończeniu procesu logowania. To do punktu końcowego usługi tkaninie Explorer należy ustawić dla klaster, która domyślnie jest:

    https://&lt;cluster_domain&gt;: 19080-Eksploratora

    Zostanie wyświetlony monit o zalogowanie się do konta z uprawnieniami administratora dzierżawy AAD. Po wykonaniu, skrypt przechodzi do tworzenia sieci web i natywnej aplikacji reprezentować klaster tkaninie usługi. W aplikacjach dzierżawy w [portal Azure klasyczny][azure-classic-portal], powinna być widoczna dwa nowe wpisy:

    - *NazwaKlastra*\_klaster
    - *NazwaKlastra*\_klienta

    Drukowanie skryptu, Json, wymagane w szablonie Menedżera zasobów Azure podczas tworzenia klaster w następnej sekcji, więc należy PowerShell otwarte okno.

```json
"azureActiveDirectory": {
  "tenantId":"<guid>",
  "clusterApplication":"<guid>",
  "clientApplication":"<guid>"
},
```

## <a name="create-a-service-fabric-cluster-resource-manager-template"></a>Tworzenie szablonu na tkaninie usługi Klaster Menedżera zasobów

W tej sekcji dane wyjściowe poprzedniego polecenia programu PowerShell są używane w szablonie tkaninie usługi Klaster Menedżera zasobów.

Menedżer zasobów przykładowe szablony są dostępne w [galerii szablonów szybki start platformy Azure na GitHub][azure-quickstart-templates]. Te szablony może służyć jako punkt początkowy szablonu klaster. 

### <a name="create-the-resource-manager-template"></a>Tworzenie szablonu Menedżera zasobów

Ten przewodnik używa [klaster bezpiecznego 5 węzłów] [ service-fabric-secure-cluster-5-node-1-nodetype-wad] przykład szablonu i parametrów szablonu. Pobierz `azuredeploy.json` i `azuredeploy.parameters.json` do komputera i Otwórz oba pliki w edytorze tekstu Ulubione.

### <a name="add-certificates"></a>Dodawanie certyfikatów

Certyfikaty są dodawane do szablonu Menedżera zasobów klaster odwołując magazyn klucz zawierający klucze certyfikatów. Zaleca się, że te wartości klucza magazynu są umieszczane w pliku parametrów szablonu Menedżera zasobów, aby zachować Menedżera zasobów plik szablonu do ponownego użycia i wolne od wartości specyficzne dla wdrożenia.

#### <a name="add-all-certificates-to-the-vmss-osprofile"></a>Dodawanie wszystkich certyfikatów do VMSS osProfile

Każdy certyfikat, który musi być zainstalowany w klastrze musi być skonfigurowany w sekcji osProfile zasobów VMSS (Microsoft.Compute/virtualMachineScaleSets). Powoduje to, że dostawcy zasobów, aby zainstalować certyfikat na maszyny wirtualne. Ta opcja uwzględnia certyfikat klaster, a także wszystkie certyfikaty zabezpieczeń aplikacji, który ma być używany dla aplikacji:

```json
{
  "apiVersion": "2016-03-30",
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  ...
  "properties": {
    ...
    "osProfile": {
      ...
      "secrets": [
        {
          "sourceVault": {
            "id": "[parameters('sourceVaultValue')]"
          },
          "vaultCertificates": [
            {
              "certificateStore": "[parameters('clusterCertificateStorevalue')]",
              "certificateUrl": "[parameters('clusterCertificateUrlValue')]"
            },
            {
              "certificateStore": "[parameters('applicationCertificateStorevalue')",
              "certificateUrl": "[parameters('applicationCertificateUrlValue')]"
            },
            ...
          ]
        }
      ]
    }
  }
}
```

#### <a name="configure-service-fabric-cluster-certificate"></a>Konfigurowanie usługi tkaninie klaster certyfikatu

Klaster certyfikatu uwierzytelniania również należy skonfigurować w zasobie klaster tkaninie usługi (Microsoft.ServiceFabric/clusters) i rozszerzenia tkaninie usługi dla VMSS w zasobie VMSS. Dzięki temu tkaninie usługi dostawcy zasobów jest skonfigurowana do używania przez klaster i uwierzytelniania serwera dla punktów końcowych zarządzania.

##### <a name="vmss-resource"></a>Zasób VMSS:

```json
{
  "apiVersion": "2016-03-30",
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  ...
  "properties": {
    ...
    "virtualMachineProfile": {
      "extensionProfile": {
        "extensions": [
          {
            "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
            "properties": {
              ...
              "settings": {
                ...
                "certificate": {
                  "thumbprint": "[parameters('clusterCertificateThumbprint')]",
                  "x509StoreName": "[parameters('clusterCertificateStoreValue')]"
                },
                ...
              }
            }
          }
        ]
      }
    }
  }
}
```

##### <a name="service-fabric-resource"></a>Zasób tkaninie usługi:

```json
{
  "apiVersion": "2016-03-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  "location": "[parameters('clusterLocation')]",
  "dependsOn": [
    "[concat('Microsoft.Storage/storageAccounts/', variables('supportLogStorageAccountName'))]"
  ],
  "properties": {
    "certificate": {
      "thumbprint": "[parameters('clusterCertificateThumbprint')]",
      "x509StoreName": "[parameters('clusterCertificateStoreValue')]"
    },
    ...
  }
}
```

### <a name="insert-aad-config"></a>Wstawianie AAD konfiguracji

Konfiguracja AAD utworzony wcześniej można wstawiać bezpośrednio do szablonu Menedżera zasobów, jednak zaleca się wyodrębnić wartości do parametrów najpierw do pliku parametrów do zachowania szablonu Menedżera zasobów do ponownego użycia i wolne od wartości określonych do wdrożenia.

```json
{
  "apiVersion": "2016-03-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  ...
  "properties": {
    "certificate": {
      "thumbprint": "[parameters('clusterCertificateThumbprint')]",
      "x509StoreName": "[parameters('clusterCertificateStorevalue')]"
    },
    ...
    "azureActiveDirectory": {
      "tenantId": "[parameters('aadTenantId')]",
      "clusterApplication": "[parameters('aadClusterApplicationId')]",
      "clientApplication": "[parameters('aadClientApplicationId')]"
    },
    ...
  }
}
```

### < "Konfigurowanie arm" ></a>parametry szablonu skonfigurować Menedżera zasobów

Ponadto przy użyciu wartości wyjściowych z poleceń klucza magazynu i AAD PowerShell można wypełnić pliku parametrów:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": { 
        ...
        "clusterCertificateStoreValue": {
            "value": "My"
        },
        "clusterCertificateThumbprint": {
            "value": "<thumbprint>"
        },
        "clusterCertificateUrlValue": {
            "value": "https://myvault.vault.azure.net:443/secrets/myclustercert/4d087088df974e869f1c0978cb100e47"
        },
        "applicationCertificateStorevalue": {
            "value": "My"
        },
        "applicationCertificateUrlValue": {
            "value": "https://myvault.vault.azure.net:443/secrets/myapplicationcert/2e035058ae274f869c4d0348ca100f08"
        },
        "sourceVaultvalue": {
            "value": "/subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault"
        },
        "aadTenantId": {
            "value": "<guid>"
        },
        "aadClusterApplicationId": {
            "value": "<guid>"
        },
        "aadClientApplicationId": {
            "value": "<guid>"
        },
        ...
    }
}
```
W tym momencie powinny być następujące:

 - Grupa zasobów magazynu klucza
    - Kluczowe magazynu
    - Klaster certyfikatu uwierzytelniania serwera
    - Certyfikat Szyfrowanie danych
 - Azure dzierżawy usługi Active Directory 
    - AAD aplikacji do zarządzania opartego na sieci web i usługi tkaninie Eksploratora
    - AAD aplikacji na potrzeby zarządzania klientami
    - Użytkownicy z przypisanych ról 
 - Usługa tkaninie klaster Menedżera zasobów szablonu
    - Certyfikaty skonfigurować za pomocą klucza magazynu
    - Azure Active Directory skonfigurowane 

Na poniższym diagramie przedstawiono miejsce, w którym konfiguracja magazynu klucza i AAD dopasowania do szablonu Menedżera zasobów.

![Mapa współzależności Menedżera zasobów][cluster-security-arm-dependency-map]

## <a name="create-the-cluster"></a>Utworzyć klaster

Teraz można przystąpić do tworzenia klaster [ARM]za pomocą[resource-group-template-deploy].

#### <a name="test-it"></a>Należy je przetestować

Użyj następującego polecenia programu PowerShell, aby przetestować szablonu Menedżera zasobów z plikiem parametrów:

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

#### <a name="deploy-it"></a>Wdrażanie go

Jeśli Menedżera zasobów szablonu jest prawdziwe, użyj następującego polecenia programu PowerShell wdrożenia szablonu Menedżera zasobów z plikiem parametrów:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

<a name="assign-roles"></a>
## <a name="assign-users-to-roles"></a>Przypisywanie użytkowników do ról

Po utworzeniu aplikacji reprezentować klaster należy przypisać użytkowników do ról obsługiwanych przez usługę tkaninie: tylko do odczytu i administrator. Można to zrobić za pomocą [portal Azure klasyczny][azure-classic-portal].

1. Przejdź do swojej dzierżawy i wybierz pozycję aplikacje.
2. Wybieranie aplikacji sieci web, która ma nazwę, podobnie jak `myTestCluster_Cluster`.
3. Kliknij kartę użytkownicy.
4. Wybierz użytkownika do przypisywania i kliknij przycisk **Przypisz** u dołu ekranu.

    ![Przypisywanie użytkowników do roli przycisku][assign-users-to-roles-button]

5. Wybierz rolę, aby przypisać użytkownikowi.

    ![Przypisywanie użytkowników do ról][assign-users-to-roles-dialog]

>[AZURE.NOTE] Aby uzyskać więcej informacji na temat ról w tkaninie usługi zobacz [Kontrola dostępu oparta na rolach dla klientów usługi tkaninie](service-fabric-cluster-security-roles.md).

 <a name="secure-linux-cluster"></a> 
##  <a name="create-secure-clusters-on-linux"></a>Tworzenie bezpiecznego klastrów w systemie Linux

Aby ułatwić ten proces, dostarczono skrypt pomocy [tutaj](http://github.com/ChackDan/Service-Fabric/tree/master/Scripts/CertUpload4Linux). Do użycia tego skryptu Pomocnik, zakłada się, że masz już zainstalowany polecenie Azure i znajduje się w ścieżce. Upewnij się, że skrypt ma uprawnienia do wykonania, uruchamiając `chmod +x cert_helper.py` po pobraniu go. Pierwszym krokiem jest do logowania się do konta usługi Azure za pomocą interfejsu wiersza polecenia z `azure login` polecenia. Po zalogowaniu się do konta usługi Azure, użyj pomocniczy wraz z podpisanego certyfikatem urzędu certyfikacji, jak to przedstawiono następujące polecenie:

```sh
./cert_helper.py [-h] CERT_TYPE [-ifile INPUT_CERT_FILE] [-sub SUBSCRIPTION_ID] [-rgname RESOURCE_GROUP_NAME] [-kv KEY_VAULT_NAME] [-sname CERTIFICATE_NAME] [-l LOCATION] [-p PASSWORD]

The -ifile parameter can take a .pfx or a .pem file as input, with the certificate type (pfx or pem, or ss if it is a self-signed cert).
The parameter -h prints out the help text.
```

To polecenie zwraca trzy następujące ciągi jako wynik: 

1. SourceVaultID, czyli identyfikator nowego grupa zasobów KeyVault jej utworzenia. 

2. CertificateUrl dostępu do certyfikatu.

3. CertificateThumbprint, który jest używany do uwierzytelniania.


W poniższym przykładzie pokazano, jak korzystać z polecenia:

```sh
./cert_helper.py pfx -sub "fffffff-ffff-ffff-ffff-ffffffffffff"  -rgname "mykvrg" -kv "mykevname" -ifile "/home/test/cert.pfx" -sname "mycert" -l "East US" -p "pfxtest"
```
Wykonywanie poprzedniego polecenia oferuje trzy ciągi w następujący sposób:

```sh
SourceVault: /subscriptions/fffffff-ffff-ffff-ffff-ffffffffffff/resourceGroups/mykvrg/providers/Microsoft.KeyVault/vaults/mykvname
CertificateUrl: https://myvault.vault.azure.net/secrets/mycert/00000000000000000000000000000000
CertificateThumbprint: 0xfffffffffffffffffffffffffffffffffffffffff
```

 Nazwa podmiotu certyfikatu musi odpowiadać domeny używanego do uzyskiwania dostępu klaster tkaninie usługi. Jest to wymagane zapewnia SSL klaster punkty końcowe zarządzania HTTPS Eksploratora tkaninie usługi. Nie można uzyskać certyfikat SSL od urzędu certyfikacji (CA) dla `.cloudapp.azure.com` domeny. Należy uzyskać niestandardowej nazwy domeny dla klaster. Gdy żądania certyfikatu od urzędu certyfikacji nazwa podmiotu certyfikatu musi odpowiadać niestandardowej nazwy domeny na potrzeby klaster.

Są to pozycje potrzebne przy tworzeniu klastrze tkaninie Usługa bezpiecznego (bez AAD), zgodnie z opisem w [Parametry szablonu skonfigurować Menedżera zasobów](#configure-arm). Można nawiązać bezpiecznego klaster za pośrednictwem instrukcjami w [uwierzytelniania dostępu klienta z klastrem](service-fabric-connect-to-secure-cluster.md). Linux podglądu klastrów nie obsługują uwierzytelniania AAD. Możesz zacząć przypisywać role administratora i klienta, zgodnie z opisem w sekcji [Przypisywanie ról do użytkowników](#assign-roles). Określając ról administratora i klienta dla klastrów Podgląd Linux, musisz podać odciski palców certyfikatu uwierzytelniania (w przeciwieństwie do nazwy podmiotu, ponieważ nie sprawdzania poprawności łańcucha lub odwołania jest wykonywana w tej wersji preview).


Jeśli chcesz użyć certyfikatu z podpisem własnym do celów testowych, można ten sam skrypt do generowania certyfikat z podpisem własnym i przekaż go do KeyVault, podając flagę `ss` zamiast dostarczając certyfikat ścieżkę i nazwę certyfikatu. Na przykład zobacz następujące polecenie służące do tworzenia i przekazywanie certyfikatu z podpisem własnym:

```sh
./cert_helper.py ss -rgname "mykvrg" -sub "fffffff-ffff-ffff-ffff-ffffffffffff" -kv "mykevname"   -sname "mycert" -l "East US" -p "selftest" -subj "mytest.eastus.cloudapp.net" 
```

To polecenie zwraca samej trzy ciągi, SourceVault, CertificateUrl i CertificateThumbprint, który jest używany do tworzenia bezpiecznego klaster Linux, wraz z lokalizacji, w której został umieszczony certyfikatu z podpisem własnym. Aby połączyć się z klastrem certyfikatu z podpisem własnym jest potrzebna.  Można nawiązać bezpiecznego klaster za pośrednictwem instrukcjami w [uwierzytelniania dostępu klienta z klastrem](service-fabric-connect-to-secure-cluster.md). Nazwa podmiotu certyfikatu musi odpowiadać domeny używanego do uzyskiwania dostępu klaster tkaninie usługi. Jest to wymagane zapewnia SSL klaster punkty końcowe zarządzania HTTPS Eksploratora tkaninie usługi. Nie można uzyskać certyfikat SSL od urzędu certyfikacji (CA) dla `.cloudapp.azure.com` domeny. Należy uzyskać niestandardowej nazwy domeny dla klaster. Gdy żądania certyfikatu od urzędu certyfikacji nazwa podmiotu certyfikatu musi odpowiadać niestandardowej nazwy domeny na potrzeby klaster.

Parametry podane przez skrypt Pomocnik może być wypełnione portalu, jak opisano w sekcji [Tworzenie klaster w portalu Azure](service-fabric-cluster-creation-via-portal.md#create-cluster-portal).

## <a name="next-steps"></a>Następne kroki

W tym momencie masz bezpiecznego klaster z usługi Azure Active Directory zapewniający zarządzania uwierzytelniania. [Nawiązywanie połączenia z klaster](service-fabric-connect-to-secure-cluster.md) dalej i Dowiedz się, jak [zarządzać hasła aplikacji](service-fabric-application-secret-management.md).

## <a name="troubleshoot-setting-up-azure-active-directory-for-client-authentication"></a>Rozwiązywanie problemów z przygotowywaniem usługi Azure Active Directory uwierzytelnianie klienta

Jeśli napotkasz problem podczas konfigurowania usługi Azure Active Directory uwierzytelnianie klienta na podstawie Przejrzyj następujące suggestings potencjalne rozwiązania.

### <a name="service-fabric-explorer-prompts-for-selecting-certificate"></a>Usługa tkaninie Explorer monituje o podanie Wybieranie certyfikatu

#### <a name="problem"></a>Problem

Po logowania się pomyślnie AAD strony logowania w Eksploratorze tkaninie usługi przeglądarce powraca do strony głównej, ale wyświetli okno dialogowe Wybieranie certyfikatu.

![Okno dialogowe Wybieranie certyfikatu SFX][sfx-select-certificate-dialog]

#### <a name="reason"></a>Przyczyna

Użytkownik nie został przypisany roli w AAD klaster aplikacji. Dlatego AAD uwierzytelnianie kończy się niepowodzeniem w klastrze tkaninie usługi. Usługa tkaninie Explorer przechodzi do certyfikatu uwierzytelniania.

#### <a name="solution"></a>Rozwiązanie

Postępuj zgodnie z instrukcjami konfigurowania AAD i przypisywać role użytkowników. Ponadto "Użytkownika przydziału wymagane do aplikacji programu ACCESS" zaleca się włączenie jako `SetupApplications.ps1` czy.

### <a name="connect-with-powershell-fails-with-error-the-specified-credentials-are-invalid"></a>Nawiązywanie połączenia przy użyciu programu PowerShell kończy się niepowodzeniem z powodu błędu: określone poświadczenia są nieprawidłowe

#### <a name="problem"></a>Problem

Kiedy używać programu PowerShell do nawiązania połączenia z klastrem przy użyciu trybu zabezpieczeń "AzureActiveDirectory", po logowania się pomyślnie AAD strona logowania, połączenie kończy się niepowodzeniem z powodu błędu: określone poświadczenia są nieprawidłowe wyświetlane.

#### <a name="solution"></a>Rozwiązanie

Działa tak samo jak powyżej.

### <a name="service-fabric-explorer-signing-in-return-failure-aadsts50011"></a>Eksplorator tkaninie usługi logowania w zamian błąd: AADSTS50011

#### <a name="problem"></a>Problem

Po logowania AAD stronę logowania w Eksploratorze tkaninie usługi, Strona zwraca znak niepowodzenie - AADSTS50011: adres zwrotny &lt;adres url&gt; nie ma odpowiednika adresy odpowiedź skonfigurowane dla aplikacji: &lt;guid&gt;. 

![Adres zwrotny SFX niezgodne][sfx-reply-address-not-match]

#### <a name="reason"></a>Przyczyna

Aplikacji cluster(web) reprezentującą próby Eksploratora tkaninie Usługa uwierzytelniania AAD, jako część żądania zapewnia zwrotnym adresem URL przekierowania. Ale nie znajduje się na liście AAD aplikacja "Adres URL odpowiedzi".

#### <a name="solution"></a>Rozwiązanie

Dodawanie adresu url usługi tkaninie Eksploratora "Adres URL odpowiedzi" na karcie Konfigurowanie aplikacji cluster(web) lub zamienianie jeden z elementów na liście. Następnie zapisz.

![Adres url odpowiedzi aplikacji sieci Web][web-application-reply-url]

### <a name="can-i-reuse-the-same-aad-tenant-for-multiple-clusters"></a>Czy można ponownie użyć tej samej dzierżawy AAD dla wielu klientów?

#### <a name="answer"></a>Odpowiedzi

Wartość Tak. Ale pamiętaj dodać adres URL Eksploratora tkaninie usług do aplikacji cluster(web), które nie będą działać inaczej Eksploratora tkaninie usługi.

### <a name="why-do-i-still-need-server-certificate-while-aad-enabled"></a>Dlaczego czy nadal potrzebuję certyfikat podczas włączania AAD?

#### <a name="answer"></a>Odpowiedzi

FabricClient i FabricGateway, określając wzajemnego uwierzytelniania. W przypadku uwierzytelniania AAD integracja AAD zapewnia, że tożsamości klienta do serwera i certyfikat jest używany do weryfikowania tożsamości serwera. Aby uzyskać więcej informacji na temat współdziałania certyfikatu na tkaninie usługi sprawdzanie [certyfikaty X.509 i tkaninie usługi][x509-certificates-and-service-fabric]

<!-- Links -->
[azure-powershell]:https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[key-vault-get-started]:../key-vault/key-vault-get-started.md
[aad-graph-api-docs]:https://msdn.microsoft.com/library/azure/ad/graph/api/api-catalog
[azure-classic-portal]: https://manage.windowsazure.com
[service-fabric-rp-helpers]: https://github.com/ChackDan/Service-Fabric/tree/master/Scripts/ServiceFabricRPHelpers
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[active-directory-howto-tenant]: ../active-directory/active-directory-howto-tenant.md
[service-fabric-visualizing-your-cluster]: service-fabric-visualizing-your-cluster.md
[service-fabric-manage-application-in-visual-studio]: service-fabric-manage-application-in-visual-studio.md
[sf-aad-ps-script-download]:http://servicefabricsdkstorage.blob.core.windows.net/publicrelease/MicrosoftAzureServiceFabric-AADHelpers.zip
[azure-quickstart-templates]: https://github.com/Azure/azure-quickstart-templates
[service-fabric-secure-cluster-5-node-1-nodetype-wad]: https://github.com/Azure/azure-quickstart-templates/blob/master/service-fabric-secure-cluster-5-node-1-nodetype-wad/
[resource-group-template-deploy]: https://azure.microsoft.com/documentation/articles/resource-group-template-deploy/
[x509-certificates-and-service-fabric]: service-fabric-cluster-security.md#x509-certificates-and-service-fabric

<!-- Images -->
[cluster-security-arm-dependency-map]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-arm-dependency-map.png
[cluster-security-cert-installation]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-cert-installation.png
[assign-users-to-roles-button]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles-button.png
[assign-users-to-roles-dialog]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles.png
[sfx-select-certificate-dialog]: ./media/service-fabric-cluster-creation-via-arm/sfx-select-certificate-dialog.png
[sfx-reply-address-not-match]: ./media/service-fabric-cluster-creation-via-arm/sfx-reply-address-not-match.png
[web-application-reply-url]: ./media/service-fabric-cluster-creation-via-arm/web-application-reply-url.png