<properties
    pageTitle="Wprowadzenie do magazynu klucza Azure stosem | Microsoft Azure"
    description="Wprowadzenie do korzystania z magazynu klucza stos Azure"
    services="azure-stack"
    documentationCenter=""
    authors="rlfmendes"
    manager="natmack"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="ricardom"/>


# <a name="getting-started-with-key-vault"></a>Wprowadzenie do magazynu klucza

W tej sekcji opisano czynności, aby utworzyć magazynu, zarządzanie klucze i hasła, a także zezwolić użytkownikom lub aplikacji do wywołania operacje w magazynu w stos Azure. Poniższe czynności przyjęto założenie, istnieje subskrypcji dzierżawy i usługa KeyVault jest zarejestrowana w tej subskrypcji. Wszystkie polecenia przykładzie są oparte na i poleceń cmdlet KeyVault jako część zestawu SDK programu PowerShell Azure.

## <a name="enabling-the-tenant-subscription-for-vault-operations"></a>Włączanie subskrypcji dzierżawy dla operacji magazynu 

Aby można problemu za wszelkie magazynu, musisz upewnij się, że Twoja subskrypcja została włączona funkcja operacje magazynu. Można to stwierdzenia, wysyłając następującego polecenia programu PowerShell:

    Get-AzureRmResourceProvider -ProviderNamespace Microsoft.KeyVault | ft -AutoSize

Wynik powyższego polecenia Zgłoś "Zarejestrowane" dla każdego wiersza stanu "Rejestracji".

    ProviderNamespace RegistrationState ResourceTypes Locations
    Microsoft.KeyVault Registered {operations} {local}
    Microsoft.KeyVault Registered {vaults} {local}
    Microsoft.KeyVault Registered {vaults/secrets} {local}
    

 Jeśli nie jest to wielkość liter, należy wywołać następujące polecenie, aby zarejestrować usługi KeyVault w ramach subskrypcji:

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.KeyVault

I dane wyjściowe polecenia jest następujących funkcji:
    
    ProviderNamespace : Microsoft.KeyVault
    RegistrationState : Registered
    ResourceTypes : {operations, vaults, vaults/secrets}
    Locations : {local}


>[AZURE.NOTE] Jeśli zostanie wyświetlony komunikat o błędzie: "*Subskrypcja nie jest zarejestrowany Azure klucza magazynu*" podczas wywoływania KeyVault polecenia cmdlet, potwierdź włączono dostawcy zasobów KeyVault zgodnie z instrukcjami powyżej.


## <a name="creating-a-hardened-container-a-vault-in-azure-stack-to-store-and-manage-cryptographic-keys-and-secrets"></a>Tworzenie zaostrzony kontenera (magazynu) w stos Azure do przechowywania i zarządzanie klucze szyfrowania i hasła

Aby można było utworzyć magazynu, dzierżawy należy najpierw utworzyć grupę zasobów. Następujące polecenia programu PowerShell Tworzenie grupy zasobów, a następnie magazynu w danej grupy zasobów. Przykład zawiera również typowe dane wyjściowe tego polecenia cmdlet.

### <a name="creating-a-resource-group"></a>Tworzenie grupy zasobów:

    New-AzureRmResourceGroup -Name vaultrg010 -Location local -Verbose -Force

Wynik:

    VERBOSE: Performing the operation "Replacing resource group ..." on target "".
    VERBOSE: 12:52:51 PM - Created resource group 'vaultrg010' in location 'local'
    ResourceGroupName : vaultrg010
    Location : local
    ProvisioningState : Succeeded
    Tags :
    ResourceId : /subscriptions/fa881715-3802-42cc-a54e-a06adf61584d/resourceGroups/vaultrg010
    

### <a name="creating-a-vault"></a>Tworzenie magazynu:


    New-AzureRmKeyVault -VaultName vault010 -ResourceGroupName vaultrg010 -Location local -Verbose

Wynik:

    VaultUri : https://vault010.vault.azurestack.local
    TenantId : 5454420b-2e38-4b9e-8b56-1712d321cf33
    TenantName : 5454420b-2e38-4b9e-8b56-1712d321cf33
    Sku : Standard
    EnabledForDeployment : False
    EnabledForTemplateDeployment : False
    EnabledForDiskEncryption : False
    AccessPolicies : {5454420b-2e38-4b9e-8b56-1712d321cf33}
    AccessPoliciesText :
    Tenant ID : 5454420b-2e38-4b9e-8b56-1712d321cf33
    Object ID : ca342e90-f6aa-435b-a11c-dfe5ef0bfeeb
    Application ID :
    Display Name : Tenant Admin (tenantadmin1@msazurestack.onmicrosoft.com)
    Permissions to Keys : get, create, delete, list, update, import, backup, restore
    Permissions to Secrets : all
    OriginalVault : Microsoft.Azure.Management.KeyVault.Vault
    ResourceId : /subscriptions/fa881715-3802-42cc-a54e-a06adf61584d/resourceGroups/vaultrg010/providers/Microsoft.KeyVault/vaults/vault010
    VaultName : vault010
    ResourceGroupName : vaultrg010
    Location : local
    Tags : {}
    TagsTable :
    
Wynik to polecenie cmdlet zawiera właściwości magazynu klucza, która została właśnie utworzona. Dwie najważniejsze właściwości są następujące:

-   **Nazwa magazynu**: W tym przykładzie jest **vault010**. Użyje tej nazwy innych poleceń cmdlet klucza magazynu.

-   **Identyfikator URI magazynu**: W tym przykładzie jest https://vault010.vault.azurestack.local. Aplikacje korzystające z magazynu za pośrednictwem jej interfejsu API usługi REST, należy użyć tego identyfikatora URI.

Konto Azure jest teraz autoryzowany do operacji na tym klucza magazynu. Jeszcze nikt inny nie jest.


## <a name="operating-on-keys-and-secrets"></a>Działający na klucze i hasła

Po utworzeniu magazynu, wykonaj kroki w celu utworzenia Zarządzanie klucze i hasła:

### <a name="creating-a-key"></a>Tworzenie klucza

Aby można było utworzyć klucz, za pomocą **AzureKeyVaultKey Dodaj** na poniższym przykładzie. Po pomyślnym utworzeniu klucza polecenia cmdlet będzie wyjściowy nowo utworzonego kluczowe informacje.

    Add-AzureKeyVaultKey -VaultName \$vaultName -Name\$keyVaultKeyName -Verbose -Destination Software

Wynik polecenia cmdlet *AzureKeyVaultKey Dodaj* jest następująca:

    Attributes : Microsoft.Azure.Commands.KeyVault.Models.KeyAttributes
    Key : {"kid":"https://vault010.vault.azurestack.local/keys/keyVaultKeyName001/86062b02b10342688f3b0b3713e343ff","kty":"RSA","key\_ops":\["encrypt"
    ,"decrypt","sign","verify","wrapKey","unwrapKey"\],"n":"nDkcBQCyyLnXtbwFMnXOWzPDqWKiyjf0G3QTxvuN\_MansEn9X-91q4\_WFmRBCd5zWBqz671iuZO\_D4r0P25
    Fe2lAq\_3T1gATVNGR7LTEU9W5h8AoY10bmt4e0y66Jn2vUV-UTCz4\_vtKSKoiuNXHFR\_tGZ-6YX-frqKIiC8pbE4Qvz1x-c7E-eM\_Cpu87koL95n-Hl3wQRQRPXEPRR6gcHR5E74D1
    gLEFCWKySTo4nXtLoeBMNK5QYEBZIAS61ACbR4czjHn6ty-tZeVTc7hyK\_UO2EbJovQIAhyayfq018uNtCBzjjkqJKnY34kviVCPoTQqOdpHa0FHrloe5FeIw","e":"AQAB"}
    VaultName : vault010
    Name : keyVaultKeyName001
    Version : 86062b02b10342688f3b0b3713e343ff
    Id : https://vault010.vault.azurestack.local:443/keys/keyVaultKeyName001/86062b02b10342688f3b0b3713e343ff
    
Można teraz odwoływać się ten klucz utworzony lub przekazane do magazynu klucza Azure, przy użyciu jej identyfikatora URI. Umożliwia **keyVaultKeyName001-https://vault010.vault.azurestack.local:443-klawiszy** zawsze pobranie bieżącej wersji; i Uzyskaj tej określonej wersji za pomocą **https://vault010.vault.azurestack.local:443-klawiszy keyVaultKeyName001-86062b02b10342688f3b0b3713e343ff** .

### <a name="retrieving-a-key"></a>Pobieranie klucza

**Get-AzureKeyVaultKey** umożliwiają pobieranie klawisza i jego szczegóły na poniższym przykładzie:

    Get-AzureKeyVaultKey -VaultName vault010 -Name keyVaultKeyName001

Oto wynik Get-AzureKeyVaultKey

    Attributes : Microsoft.Azure.Commands.KeyVault.Models.KeyAttributes
    Key : {"kid":"https://vault010.vault.azurestack.local/keys/keyVaultKeyName001/86062b02b10342688f3b0b3713e343ff","kty":"RSA","key\_ops":\["encrypt"
    ,"decrypt","sign","verify","wrapKey","unwrapKey"\],"n":"nDkcBQCyyLnXtbwFMnXOWzPDqWKiyjf0G3QTxvuN\_MansEn9X-91q4\_WFmRBCd5zWBqz671iuZO\_D4r0P25
    Fe2lAq\_3T1gATVNGR7LTEU9W5h8AoY10bmt4e0y66Jn2vUV-UTCz4\_vtKSKoiuNXHFR\_tGZ-6YX-frqKIiC8pbE4Qvz1x-c7E-eM\_Cpu87koL95n-Hl3wQRQRPXEPRR6gcHR5E74D1
    gLEFCWKySTo4nXtLoeBMNK5QYEBZIAS61ACbR4czjHn6ty-tZeVTc7hyK\_UO2EbJovQIAhyayfq018uNtCBzjjkqJKnY34kviVCPoTQqOdpHa0FHrloe5FeIw","e":"AQAB"}
    VaultName : vault010
    Name : keyVaultKeyName001
    Version : 86062b02b10342688f3b0b3713e343ff
    Id : https://vault010.vault.azurestack.local:443/keys/keyVaultKeyName001/86062b02b10342688f3b0b3713e343ff

### <a name="setting-a-secret"></a>Ustawianie hasła

    $secretvalue = ConvertTo-SecureString 'User@123' -AsPlainText -Force
    Set-AzureKeyVaultSecret -Name MySecret-VaultName vault010 -SecretValue $secretvalue
    
Wynik

    Vault Name : vault010
    Name : MySecret
    Version : 65a387f2ed4a416180e852b970846f5b
    Id : https://vault010.vault.azurestack.local:443/secrets/MySecret/65a387f2ed4a416180e852b970846f5b
    Enabled : True
    Expires :
    Not Before :
    Created : 8/17/2016 10:49:52 PM
    Updated : 8/17/2016 10:49:52 PM
    Content Type :
    Tags : 

### <a name="retrieving-a-secret"></a>Pobieranie hasła

    Get-AzureKeyVaultSecret -VaultName vault010

Wynik

    Vault Name : vault010
    Name : MySecret
    Version :
    Id : https://vault010.vault.azurestack.local:443/secrets/MySecret
    Enabled : True
    Expires :
    Not Before :
    Created : 8/17/2016 10:49:52 PM
    Updated : 8/17/2016 10:49:52 PM
    Content Type :
    Tags :

Teraz z magazynu kluczy i klucza lub hasło jest gotowy do aplikacji do użytku.
Należy zezwolić aplikacji do korzystania z nich.

## <a name="authorize-the-application-to-use-the-key-or-secret"></a>Autoryzuj aplikacji użyj klawisza lub hasło 

Aby zezwolić aplikacji, aby uzyskać dostęp do klucza lub hasła w magazyn, należy użyć polecenia cmdlet Set -**AzureRmKeyVaultAccessPolicy** .

Na przykład jeśli Twoja nazwa magazynu jest *ContosoKeyVault* aplikację, którą chcesz zezwolić ma *Identyfikator klienta* *8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed*i chcesz zezwolić aplikacji odszyfrowywanie i zaloguj się za pomocą klawiszy w swojej magazynu, uruchom następujące polecenie:

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToKeys decrypt,sign

Jeśli chcesz zezwolić tej samej aplikacji, aby odczytać hasła do magazynu, uruchom następujące polecenie:

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToSecrets Get


## <a name="next-steps"></a>Następne kroki
[Wdrażanie maszyn wirtualnych za pomocą hasła klucza magazynu](azure-stack-kv-deploy-vm-with-secret.md)

[Wdrażanie maszyn wirtualnych za pomocą certyfikatu klucza magazynu](azure-stack-kv-push-secret-into-vm.md)