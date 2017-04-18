<properties
   pageTitle="Nawiązywanie połączenia z bezpiecznego klaster prywatne | Microsoft Azure"
   description="W tym artykule opisano, jak do zabezpieczania komunikacji w wersji autonomicznej lub prywatnych klaster także między klientami i klaster."
   services="service-fabric"
   documentationCenter=".net"
   authors="dsk-2015"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/08/2016"
   ms.author="dkshir"/>

# <a name="secure-a-standalone-cluster-on-windows-using-x509-certificates"></a>Zabezpieczanie klastrze autonomicznego w systemie Windows przy użyciu certyfikaty X.509

W tym artykule opisano, jak do zabezpieczania komunikacji między różne węzły autonomicznego klaster systemu Windows, oraz jak do uwierzytelniania klientów nawiązywanie połączenia z tym klastrem przy użyciu X.509 certyfikaty. Dzięki temu tylko autoryzowani użytkownicy mogą uzyskać dostęp do klaster rozmieszczonych aplikacji, a wykonywania zadań zarządzania.  Certyfikat zabezpieczeń powinna być włączona w klastrze, jeśli klaster jest tworzony.  

Aby uzyskać więcej informacji o zabezpieczeniach klaster, takich jak zabezpieczeń węzła do węzła Węzeł Klient zabezpieczeń i kontroli dostępu oparta na rolach, zobacz [scenariusze zabezpieczeń klaster](service-fabric-cluster-security.md).

## <a name="which-certificates-will-you-need"></a>Które certyfikaty są potrzebne?

Aby rozpocząć, [Pobierz pakiet klaster autonomicznego](service-fabric-cluster-creation-for-windows-server.md#downloadpackage) do jednego z węzłów w klastrze. W pakiecie pobrany spowoduje znalezienie pliku **ClusterConfig.X509.MultiMachine.json** . Otwórz plik i zapoznaj się z sekcją **zabezpieczeń** w sekcji **Właściwości** :

    "security": {
        "metadata": "The Credential type X509 indicates this is cluster is secured using X509 Certificates. The thumbprint format is - d5 ec 42 3b 79 cb e5 07 fd 83 59 3c 56 b9 d5 31 24 25 42 64.",
        "ClusterCredentialType": "X509",
        "ServerCredentialType": "X509",
        "CertificateInformation": {
            "ClusterCertificate": {
                "Thumbprint": "[Thumbprint]",
                "ThumbprintSecondary": "[Thumbprint]",
                "X509StoreName": "My"
            },
            "ServerCertificate": {
                "Thumbprint": "[Thumbprint]",
                "ThumbprintSecondary": "[Thumbprint]",
                "X509StoreName": "My"
            },
            "ClientCertificateThumbprints": [{
                "CertificateThumbprint": "[Thumbprint]",
                "IsAdmin": false
            }, {
                "CertificateThumbprint": "[Thumbprint]",
                "IsAdmin": true
            }],
            "ClientCertificateCommonNames": [{
                "CertificateCommonName": "[CertificateCommonName]",
                "CertificateIssuerThumbprint" : "[Thumbprint]",
                "IsAdmin": true
            }]
            "HttpApplicationGatewayCertificate":{
                "Thumbprint": "[Thumbprint]",
                "X509StoreName": "My"
            }
        }
    }

W tej sekcji opisano certyfikatów, które należy do zabezpieczania autonomicznego klaster systemu Windows. Aby włączyć na podstawie certyfikatu zabezpieczeń ustawić wartości **ClusterCredentialType** i **ServerCredentialType** *X509*.

>[AZURE.NOTE] [Odcisku palca](https://en.wikipedia.org/wiki/Public_key_fingerprint) jest podstawowej tożsamości certyfikatu. W tym artykule [jak pobrać odcisku palca certyfikatu](https://msdn.microsoft.com/library/ms734695.aspx) odcisku palca certyfikatów, które można utworzyć.

W poniższej tabeli wymieniono certyfikatów, które należy w ustawieniach klaster:

|**Ustawienie CertificateInformation**|**Opis**|
|-----------------------|--------------------------|
|ClusterCertificate|Ten certyfikat jest wymagane do zabezpieczania komunikacji między węzły w klastrze. Za pomocą dwóch różnych certyfikatów, głównego i pomocniczego uaktualnienia. W sekcji **odcisku palca** oraz że pomocniczej w zmiennych **ThumbprintSecondary** ustawić odcisku palca certyfikatu podstawowego.|
|ServerCertificate|Dyplom dla klienta podczas próby nawiązania połączenia z tym klastrem. Dla wygody możesz używać tego samego certyfikatu dla *ClusterCertificate* i *ServerCertificate*. Za pomocą dwóch certyfikatów inny serwer, głównego i pomocniczego uaktualnienia. W sekcji **odcisku palca** oraz że pomocniczej w zmiennych **ThumbprintSecondary** ustawić odcisku palca certyfikatu podstawowego. |
|ClientCertificateThumbprints|To jest zbiór certyfikatów, które chcesz zainstalować na uwierzytelnionych klientów. Możesz mieć wiele zainstalowane na komputerach, które chcesz udostępnić klaster certyfikaty innego klienta. Ustawianie odcisku palca każdego certyfikatu w zmiennej **CertificateThumbprint** . Jeśli **IsAdmin** jest ustawiony na *wartość PRAWDA*, następnie klienta przy użyciu tego certyfikatu na nim zainstalowany wykonać administrator działań związanych z zarządzaniem w klastrze. Jeśli **IsAdmin** ma *wartość FAŁSZ*, klienta przy użyciu tego certyfikatu można wykonać tylko akcje dozwolone dotyczący praw dostępu do użytkownika, zazwyczaj tylko do odczytu. Aby uzyskać więcej informacji dotyczących ról więcej [kontroli dostępu (RBAC) oparta na rolach](service-fabric-cluster-security.md/#role-based-access-control-rbac)  |
|ClientCertificateCommonNames|Ustawianie typowych nazwę pierwszej certyfikat klienta dla **CertificateCommonName**. **CertificateIssuerThumbprint** jest odcisku palca wystawcy tego certyfikatu. W tym artykule [korzysta z certyfikatów](https://msdn.microsoft.com/library/ms731899.aspx) Aby dowiedzieć się więcej o typowych nazw i wystawcy.|
|HttpApplicationGatewayCertificate|Jest to opcjonalne certyfikat, który może być określony, jeśli chcesz zabezpieczyć Centrum aplikacji Http. Upewnij się, że reverseProxyEndpointPort znajduje się w nodeTypes, jeśli korzystasz z tego certyfikatu.|

Oto przykład klaster konfiguracji której udzielono certyfikatów klaster, klientów i serwerów.

 ```
 {
    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "2016-09-26",
    "nodes": [{
        "nodeName": "vm0",
        "metadata": "Replace the localhost below with valid IP address or FQDN",
        "iPAddress": "10.7.0.5",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r0",
        "upgradeDomain": "UD0"
    }, {
      "nodeName": "vm1",
            "metadata": "Replace the localhost with valid IP address or FQDN",
        "iPAddress": "10.7.0.4",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r1",
        "upgradeDomain": "UD1"
    }, {
        "nodeName": "vm2",
      "iPAddress": "10.7.0.6",
            "metadata": "Replace the localhost with valid IP address or FQDN",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r2",
        "upgradeDomain": "UD2"
    }],
    "properties": {
        "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "FileShare",
        "IsEncrypted": "false",
        "connectionstring": "c:\\ProgramData\\SF\\DiagnosticsStore"
        }
        "security": {
            "metadata": "The Credential type X509 indicates this is cluster is secured using X509 Certificates. The thumbprint format is - d5 ec 42 3b 79 cb e5 07 fd 83 59 3c 56 b9 d5 31 24 25 42 64.",
            "ClusterCredentialType": "X509",
            "ServerCredentialType": "X509",
            "CertificateInformation": {
                "ClusterCertificate": {
                    "Thumbprint": "a8 13 67 58 f4 ab 89 62 af 2b f3 f2 79 21 be 1d f6 7f 43 26",
                    "X509StoreName": "My"
                },
                "ServerCertificate": {
                    "Thumbprint": "a8 13 67 58 f4 ab 89 62 af 2b f3 f2 79 21 be 1d f6 7f 43 26",
                    "X509StoreName": "My"
                },
                "ClientCertificateThumbprints": [{
                    "CertificateThumbprint": "c4 c18 8e aa a8 58 77 98 65 f8 61 4a 0d da 4c 13 c5 a1 37 6e",
                    "IsAdmin": false
                }, {
                    "CertificateThumbprint": "71 de 04 46 7c 9e d0 54 4d 02 10 98 bc d4 4c 71 e1 83 41 4e",
                    "IsAdmin": true
                }]
            }
        },
        "reliabilityLevel": "Bronze",
        "nodeTypes": [{
            "name": "NodeType0",
            "clientConnectionEndpointPort": "19000",
            "clusterConnectionEndpoint": "19001",
            "httpGatewayEndpointPort": "19080",
            "applicationPorts": {
                "startPort": "20001",
                "endPort": "20031"
            },
            "ephemeralPorts": {
                "startPort": "20032",
                "endPort": "20062"
            },
            "isPrimary": true
        }
         ],
        "fabricSettings": [{
            "name": "Setup",
            "parameters": [{
                "name": "FabricDataRoot",
                "value": "C:\\ProgramData\\SF"
            }, {
                "name": "FabricLogRoot",
                "value": "C:\\ProgramData\\SF\\Log"
            }]
        }]
    }
}
 ```

## <a name="aquire-the-x509-certificates"></a>Pobrać X.509 certyfikatów
Do zabezpieczania komunikacji w klastrze, będzie należy najpierw uzyskać certyfikat X.509 dla swojego węzłach. Ponadto aby ograniczyć połączenie z tym klastrem dla autoryzowanych komputerach i użytkowników, będzie konieczne uzyskanie i zainstalowanie certyfikatów dla komputerów klienckich.

Dla klastrów uruchomione obciążenia produkcji, należy używać [Urząd certyfikacji](https://en.wikipedia.org/wiki/Certificate_authority) X.509 certyfikat z podpisem bezpiecznego klaster. Aby uzyskać szczegółowe informacje umożliwiające uzyskanie te certyfikaty, przejdź do [jak: uzyskiwanie certyfikatu](http://msdn.microsoft.com/library/aa702761.aspx).

W przypadku klastrów, których używasz do celów testowych możesz za pomocą certyfikatu z podpisem własnym.

## <a name="optional-create-a-self-signed-certificate"></a>Opcjonalnie: Tworzenie certyfikatu z podpisem własnym
Jednym ze sposobów tworzenia podpisem certyfikatu, które mogą być chronione poprawnie jest za pomocą skryptu *CertSetup.ps1* w folderze usługi tkaninie SDK w katalogu *C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup\Secure*. Edytowanie tego pliku i umożliwia tworzenie certyfikatu z odpowiednią nazwę.

Teraz wyeksportuj certyfikat do pliku PFX chroniony hasłem. Najpierw musisz uzyskać odcisku palca certyfikatu. Uruchom aplikację certmgr.exe. Przejdź do folderu **Lokalny\Osobiste** i Znajdź certyfikat, który został utworzony. Kliknij dwukrotnie certyfikat, aby go otworzyć, wybierz kartę *Szczegóły* i przewiń pole *odcisku palca* . Skopiuj wartość do polecenia programu PowerShell poniżej, usuwając spacje.  Zmień wartość *$pswd* przydatności bezpieczne hasło ochrony i uruchamianie programu PowerShell:

```   
$pswd = ConvertTo-SecureString -String "1234" -Force –AsPlainText
Get-ChildItem -Path cert:\localMachine\my\<Thumbprint> | Export-PfxCertificate -FilePath C:\mypfx.pfx -Password $pswd
```

Aby wyświetlić szczegóły certyfikat, który został zainstalowany na komputerze można uruchamiać następujące polecenia programu PowerShell:

```
$cert = Get-Item Cert:\LocalMachine\My\<Thumbprint>
Write-Host $cert.ToString($true)
```

Ewentualnie Jeśli masz subskrypcję usługi Azure wykonaj sekcji [certyfikatów Dodaj klucz magazynu](service-fabric-cluster-creation-via-arm.md#add-certificate-to-key-vault).

## <a name="install-the-certificates"></a>Instalowanie certyfikatów
Po umieszczeniu certyfikaty, możesz zainstalować je na przez klaster. Węzły muszą mieć dostęp do najnowszych programu Windows PowerShell 3.x zainstalowany na ich. Konieczne będzie Powtórz te czynności w każdym węźle dla zarówno klaster, jak i serwera oraz certyfikatów dowolnego pomocniczą.

1. Kopiowanie plików PFX do węzła.
2. Otwórz okno programu PowerShell jako administrator, a następnie wprowadź następujące polecenia. Zamień *$pswd* hasło, które zostało użyte do utworzenia tego certyfikatu. Zamień *$PfxFilePath* pełną ścieżkę PFX kopiowane do tego węzła.

    ```
    $pswd = "1234"
    $PfcFilePath ="C:\mypfx.pfx"
    Import-PfxCertificate -Exportable -CertStoreLocation Cert:\LocalMachine\My -FilePath $PfxFilePath -Password (ConvertTo-SecureString -String $pswd -AsPlainText -Force)
    ```

3. Następnie należy ustawić kontrola dostępu na ten certyfikat, aby proces tkaninie usługi, który zostanie uruchomiona w ramach konta usługi sieciowej, można go używać, uruchamiając skrypt następujące. Podaj odcisku palca certyfikatu i "Usługa sieciowa" dla konta usługi. Aby sprawdzić, czy ACL certyfikatu są poprawne przy użyciu narzędzia certmgr.exe i spojrzenie na zarządzanie kluczy prywatnych na certyfikat.

    ```
    param
    (
        [Parameter(Position=1, Mandatory=$true)]
        [ValidateNotNullOrEmpty()]
        [string]$pfxThumbPrint,

        [Parameter(Position=2, Mandatory=$true)]
        [ValidateNotNullOrEmpty()]
        [string]$serviceAccount
        )

        $cert = Get-ChildItem -Path cert:\LocalMachine\My | Where-Object -FilterScript { $PSItem.ThumbPrint -eq $pfxThumbPrint; };

        # Specify the user, the permissions and the permission type
        $permission = "$($serviceAccount)","FullControl","Allow"
        $accessRule = New-Object -TypeName System.Security.AccessControl.FileSystemAccessRule -ArgumentList $permission;

        # Location of the machine related keys
        $keyPath = $env:ProgramData + "\Microsoft\Crypto\RSA\MachineKeys\";
        $keyName = $cert.PrivateKey.CspKeyContainerInfo.UniqueKeyContainerName;
        $keyFullPath = $keyPath + $keyName;

        # Get the current acl of the private key
        $acl = (Get-Item $keyFullPath).GetAccessControl('Access')

        # Add the new ace to the acl of the private key
        $acl.SetAccessRule($accessRule);

        # Write back the new acl
        Set-Acl -Path $keyFullPath -AclObject $acl -ErrorAction Stop

        #Observe the access rights currently assigned to this certificate.
        get-acl $keyFullPath| fl
        ```

4. Repeat the steps above for each server certificate. You can also use these steps to install the client certificates on the machines that you want to allow access to the cluster.

## Create the secure cluster
After configuring the **security** section of the **ClusterConfig.X509.MultiMachine.json** file, you can proceed to [Create your cluster](service-fabric-cluster-creation-for-windows-server.md#createcluster) section to configure the nodes and create the standalone cluster. Remember to use the **ClusterConfig.X509.MultiMachine.json** file while creating the cluster. For example, your command might look like the following:

```
.\CreateServiceFabricCluster.ps1 - ClusterConfigFilePath.\ClusterConfig.X509.MultiMachine.json - MicrosoftServiceFabricCabFilePath.\MicrosoftAzureServiceFabric.cab - AcceptEULA $true
```

Once you have the secure standalone Windows cluster successfully running, and have setup the authenticated clients to connect to it, follow the section [Connect to a secure cluster using PowerShell](service-fabric-connect-to-secure-cluster.md#connectsecurecluster) to connect to it. For example:

```
Łączenie ServiceFabricCluster - ConnectionEndpoint 10.7.0.4:19000 - KeepAliveIntervalInSec 10 - X509Credential - ServerCertThumbprint 057b9544a6f2733e0c8d3a60013a58948213f551 - FindType FindByThumbprint - FindValue 057b9544a6f2733e0c8d3a60013a58948213f551 - Pole StoreLocation CurrentUser — pole Nazwasklepu Moje
```

If you are logged on to one of the machines in the cluster, since this already has the certificate installed locally you can simply run the Powershell command to connect to the cluster and show a list of nodes:

```
Łączenie ServiceFabricCluster Get-ServiceFabricNode
```
To remove the cluster call the following command:

```
.\RemoveServiceFabricCluster.ps1 - ClusterConfigFilePath.\ClusterConfig.X509.MultiMachine.json - MicrosoftServiceFabricCabFilePath.\MicrosoftAzureServiceFabric.cab
```
