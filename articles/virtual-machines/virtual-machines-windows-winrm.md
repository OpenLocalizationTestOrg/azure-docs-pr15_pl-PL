<properties
    pageTitle="Konfigurowanie dostępu WinRM dla maszyn wirtualnych w Menedżerze zasobów Azure | Microsoft Azure"
    description="Jak skonfigurować program WinRM dostęp do użytku z Menedżera zasobów Azure maszyn wirtualnych"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/16/2016"
    ms.author="singhkay"/>

# <a name="setting-up-winrm-access-for-virtual-machines-in-azure-resource-manager"></a>Konfigurowanie dostępu WinRM dla maszyn wirtualnych w Menedżerze zasobów Azure

## <a name="winrm-in-azure-service-management-vs-azure-resource-manager"></a>WinRM w porównaniu Zarządzanie usługą Azure Azure Menedżera zasobów

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]model klasyczny wdrażania

* Aby uzyskać omówienie Menedżera zasobów Azure zobacz ten [artykuł](../azure-resource-manager/resource-group-overview.md)
* Różnice między Zarządzanie usługą Azure i Menedżera zasobów Azure zobacz ten [artykuł](../resource-manager-deployment-model.md)

Kluczowe różnica Ustawianie konfiguracji WinRM między dwoma stosy jest, jak certyfikat jest zainstalowany na maszyn wirtualnych. W stosie Menedżera zasobów Azure certyfikaty są zaprojektowany jako zasoby zarządzane przez dostawcę klucza magazynu zasobów. Dlatego użytkownik musi podać własne certyfikatu i przekaż go do magazynu klucza przed użyciem go w maszyny.

Poniżej przedstawiono kroki, które należy wykonać, aby skonfigurować maszyny z łącznością WinRM

1. Tworzenie klucza magazynu
2. Tworzenie certyfikatu z podpisem własnym
3. Przekazywanie certyfikatu z podpisem własnym do magazynu klucza
4. Uzyskać adres URL usługi certyfikatu z podpisem własnym w magazynu klucza
5. Dokumentacja dotycząca adresu URL certyfikatów z podpisem własnym podczas tworzenia maszyn wirtualnych

## <a name="step-1-create-a-key-vault"></a>Krok 1: Tworzenie klucza magazynu

Możesz użyć poniżej polecenie, aby utworzyć magazynu klucza

```
New-AzureRmKeyVault -VaultName "<vault-name>" -ResourceGroupName "<rg-name>" -Location "<vault-location>" -EnabledForDeployment -EnabledForTemplateDeployment
```

## <a name="step-2-create-a-self-signed-certificate"></a>Krok 2: Tworzenie certyfikatu z podpisem własnym
Możesz utworzyć certyfikat z podpisem własnym za pomocą tego skryptu programu PowerShell

```
$certificateName = "somename"

$thumbprint = (New-SelfSignedCertificate -DnsName $certificateName -CertStoreLocation Cert:\CurrentUser\My -KeySpec KeyExchange).Thumbprint

$cert = (Get-ChildItem -Path cert:\CurrentUser\My\$thumbprint)

$password = Read-Host -Prompt "Please enter the certificate password." -AsSecureString

Export-PfxCertificate -Cert $cert -FilePath ".\$certificateName.pfx" -Password $password
```

## <a name="step-3-upload-your-self-signed-certificate-to-the-key-vault"></a>Krok 3: Przekazywanie certyfikatu z podpisem własnym do magazynu klucza

Przed przekazaniem certyfikatu do magazynu klucz utworzony w kroku 1, należy go przekonwertowane na format zrozumiałą Microsoft.Compute dostawcy zasobów. Poniżej programu PowerShell skrypt, aby umożliwić można to zrobić

```
$fileName = "<Path to the .pfx file>"
$fileContentBytes = Get-Content $fileName -Encoding Byte
$fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)

$jsonObject = @"
{
  "data": "$filecontentencoded",
  "dataType" :"pfx",
  "password": "<password>"
}
"@

$jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
$jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)

$secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText –Force
Set-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>" -SecretValue $secret
```

## <a name="step-4-get-the-url-for-your-self-signed-certificate-in-the-key-vault"></a>Krok 4: Uzyskać adres URL usługi certyfikatu z podpisem własnym w magazynu klucza

Dostawcy zasobów Microsoft.Compute musi adresu URL tajny wewnątrz magazynu klawisz podczas inicjowania obsługi administracyjnej maszyn wirtualnych. Dzięki temu dostawcy zasobów Microsoft.Compute pobrać hasło i tworzenie równoważne certyfikatu na maszyn wirtualnych.

>[AZURE.NOTE]Adres URL to hasło musi zawierać także wersji. Przykładowy adres URL wygląda poniżej https://contosovault.vault.azure.net:443-hasła contososecret-01h9db0df2cd4300a20ence585a6s7ve


#### <a name="templates"></a>Szablony

Możesz uzyskać łącze do adresu URL przy użyciu szablonu poniżej kodu

    "certificateUrl": "[reference(resourceId(resourceGroup().name, 'Microsoft.KeyVault/vaults/secrets', '<vault-name>', '<secret-name>'), '2015-06-01').secretUriWithVersion]"

#### <a name="powershell"></a>Programu PowerShell

Można uzyskać przy użyciu tego adresu URL poniżej polecenia programu PowerShell

    $secretURL = (Get-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>").Id

## <a name="step-5-reference-your-self-signed-certificates-url-while-creating-a-vm"></a>Krok 5: Odwołanie adresu URL certyfikatów z podpisem własnym podczas tworzenia maszyn wirtualnych

#### <a name="azure-resource-manager-templates"></a>Azure szablony Menedżera zasobów

Podczas tworzenia maszyn wirtualnych za pomocą szablonów, certyfikat otrzymuje wymienianych w sekcji haseł i sekcję winRM, jak pokazano poniżej:

    "osProfile": {
          ...
          "secrets": [
            {
              "sourceVault": {
                "id": "<resource id of the Key Vault containing the secret>"
              },
              "vaultCertificates": [
                {
                  "certificateUrl": "<URL for the certificate you got in Step 4>",
                  "certificateStore": "<Name of the certificate store on the VM>"
                }
              ]
            }
          ],
          "windowsConfiguration": {
            ...
            "winRM": {
              "listeners": [
                {
                  "protocol": "http"
                },
                {
                  "protocol": "https",
                  "certificateUrl": "<URL for the certificate you got in Step 4>"
                }
              ]
            },
            ...
          }
        },

Przykładowy szablon dla powyższego można znaleźć tutaj w [201 maszyn wirtualnych — program winrm-keyvault-systemu windows](https://azure.microsoft.com/documentation/templates/201-vm-winrm-keyvault-windows)

Kod źródłowy dla tego szablonu można znaleźć w [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-winrm-keyvault-windows)

#### <a name="powershell"></a>Programu PowerShell

    $vm = New-AzureRmVMConfig -VMName "<VM name>" -VMSize "<VM Size>"
    $credential = Get-Credential
    $secretURL = (Get-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>").Id
    $vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName "<Computer Name>" -Credential $credential -WinRMHttp -WinRMHttps -WinRMCertificateUrl $secretURL
    $sourceVaultId = (Get-AzureRmKeyVault -ResourceGroupName "<Resource Group name>" -VaultName "<Vault Name>").ResourceId
    $CertificateStore = "My"
    $vm = Add-AzureRmVMSecret -VM $vm -SourceVaultId $sourceVaultId -CertificateStore $CertificateStore -CertificateUrl $secretURL

## <a name="step-6-connecting-to-the-vm"></a>Krok 6: Nawiązywanie połączenia z maszyn wirtualnych
Aby możliwe było nawiązanie do maszyn wirtualnych, najpierw upewnij się, komputer jest skonfigurowany do zarządzania zdalnego WinRM. Uruchom program PowerShell jako administrator, a następnie wykonaj poniżej polecenie, aby upewnić się, skonfigurowaniu.

    Enable-PSRemoting -Force

>[AZURE.NOTE] Może być konieczne upewnij się, że Usługa WinRM jest uruchomiony, jeśli powyższe nie działa. Możesz robić w tej aplikacji`Get-Service WinRM`

Po zakończeniu instalacji można nawiązać połączenie przy użyciu maszyn wirtualnych poniżej polecenia

    Enter-PSSession -ConnectionUri https://<public-ip-dns-of-the-vm>:5986 -Credential $cred -SessionOption (New-PSSessionOption -SkipCACheck -SkipCNCheck -SkipRevocationCheck) -Authentication Negotiate