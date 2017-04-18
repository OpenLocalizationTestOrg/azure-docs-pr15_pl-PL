<properties
    pageTitle="Rozmieszczanie maszyn wirtualnych za pomocą certyfikatu przy użyciu magazynu klucza stos Azure | Microsoft Azure"
    description="Dowiedz się, jak wdrożyć maszyn wirtualnych i wprowadzić certyfikat z magazynu klucza stos Azure"
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
    ms.date="09/26/2016"
    ms.author="ricardom"/>

# <a name="create-vms-and-include-certificates-retrieved-from-key-vault"></a>Utwórz maszyny wirtualne i Dołącz certyfikaty pobrane z magazynu klucza

W stos Azure maszyny wirtualne są rozmieszczane za pomocą Menedżera zasobów Azure i certyfikatów teraz można przechowywać w Azure stos klucza magazynu. Następnie stos Azure (Microsoft.Compute dostawcy zasobów na konkretnym) umieszcza je do swojego maszyny wirtualne podczas rozmieszczania maszyny wirtualne. Certyfikaty mogą być używane w wielu scenariuszach, w tym SSL, szyfrowania i uwierzytelniania certyfikat oparty.

Przy użyciu tej metody, możesz zachować certyfikat bezpieczne. Obecnie nie w obrazie maszyn wirtualnych lub pliki konfiguracji aplikacji lub niebezpiecznej lokalizacji. Ustawiając odpowiedniej zasady dostępu dla klucza magazynu, można również kontrolować, kto uzyskuje dostęp do certyfikatu. Kolejną zaletą jest zarządzać własnych certyfikatów w jednym miejscu Azure stos klucza magazynu.

Oto krótki przegląd procesu:

-   Potrzebujesz certyfikatu w formacie pfx.

-   Tworzenie magazynu kluczy (za pomocą szablonu lub następujący przykładowy skrypt).

-   Upewnij się, że masz włączony przełącznik EnabledForDeployment.

-   Przekazywanie certyfikatu jako poufne.

## <a name="deploying-vms"></a>Wdrażanie maszyny wirtualne

Przykładowy skrypt tworzy klucza magazynu, a następnie zapisuje certyfikat przechowywane w pliku pfx w katalogu lokalnym, aby klucza magazynu jako poufne.

    $vaultName = "contosovault"
    $resourceGroup = "contosovaultrg"
    $location = "local"
    $secretName = "servicecert"
    $fileName = "keyvault.pfx"
    $certPassword = "abcd1234"

    # =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=

    $fileContentBytes = get-content \$fileName -Encoding Byte

    $fileContentEncoded =
    [System.Convert\]::ToBase64String(\$fileContentBytes)
    $jsonObject = @"
    {
    "data": "\$filecontentencoded",
    "dataType" :"pfx",
    "password": "\$certPassword"
    }
    @$jsonObjectBytes = [System.Text.Encoding\]::UTF8.GetBytes(\$jsonObject)
    $jsonEncoded = \[System.Convert\]::ToBase64String(\$jsonObjectBytes)
    Switch-AzureMode -Name AzureResourceManager
    New-AzureResourceGroup -Name \$resourceGroup -Location \$location
    New-AzureKeyVault -VaultName \$vaultName -ResourceGroupName
    $resourceGroup -Location \$location -sku standard -EnabledForDeployment
    $secret = ConvertTo-SecureString -String \$jsonEncoded -AsPlainText -Force
    Set-AzureKeyVaultSecret -VaultName \$vaultName -Name \$secretName -SecretValue \$secret

Pierwsza część skrypt odczytuje plik PFX, a następnie zapisuje go jako JSON obiekt z base64 zawartości pliku zakodowany. Następnie obiekt JSON jest również base64 zakodowany.

Następnie umożliwia utworzenie nowej grupy zasobów i następnie utworzy klucza magazynu. Uwaga parametr ostatniego polecenia Nowy AzureKeyVault "-EnabledForDeployment", którego udziela dostępu Azure (do dostawcy zasobów Microsoft.Compute) do odczytu hasła z klucza magazynu w przypadku wdrożeń.

Ostatnie polecenie zapisuje obiekt JSON base64 zakodowany w klucza magazynu jako poufne.

Oto przykładowe dane wyjściowe z poprzedniego skrypt:

    VERBOSE:  – Created resource group 'contosovaultrg' in
    location
    'eastus'
    ResourceGroupName : contosovaultrg
    Location          : eastus
    ProvisioningState : Succeeded
    Tags              :
    Permissions       :
                        Actions NotActions
                        ======= ==========
                        \*
    ResourceId        :
    /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-dd149b4aeb56/re
    sourceGroups/contosovaultrg
    VaultUri             : https://contosovault.vault.azure.net
    TenantId             : xxxxxxxx-xxxx-xxxx-xxxx-2d7cd011db47
    TenantName           : xxxxxxxx
    Sku                  : standard
    EnabledForDeployment : True
    AccessPolicies       : {xxxxxxxx-xxxx-xxxx-xxxx-2d7cd011db47}
    AccessPoliciesText   :
                           Tenant ID              :
                           xxxxxxxx-xxxx-xxxx-xxxx-2d7cd011db47
                           Object ID              :
                           xxxxxxxx-xxxx-xxxx-xxxx-b092cebf0c80
                           Application ID         :
                           Display Name           : Derick Developer  (derick@contoso.com)
                           Permissions to Keys    : get, create, delete,
                           list, update, import, backup, restore
                           Permissions to Secrets : all
    OriginalVault        : Microsoft.Azure.Management.KeyVault.Vault
    ResourceId           :
    /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-dd149b4aeb56                 
    /resourceGroups/contosovaultrg/providers/Microsoft.KeyV
                           ault/vaults/contosovault
    VaultName            : contosovault
    ResourceGroupName    : contosovaultrg
    Location             : eastus
    Tags                 : {}
    TagsTable            :
    SecretValue     : System.Security.SecureString
    SecretValueText :
    ew0KImRhdGEiOiAiTUlJSkN3SUJBekNDQ01jR0NTcUdTSWIzRFFFSEFh
    Q0NDTGdFZ2dpME1JSUlzRENDQmdnR0NTcUdTSWIzRFFFSEFhQ0NCZmtF           
    Z2dYMU1JSUY4VENDQmUwR0N5cUdTSWIzRFFFTUNnRUNvSUlFL2pDQ0JQ
    &lt;&lt;&lt; Output truncated… &gt;&gt;&gt;
    Attributes      :
    Microsoft.Azure.Commands.KeyVault.Models.SecretAttributes
    VaultName       : contosovault
    Name            : servicecert
    Version         : e3391a126b65414f93f6f9806743a1f7
    Id              :
    https://contosovault.vault.azure.net:443/secrets/servicecert
                      /e3391a126b65414f93f6f9806743a1f7

Teraz możemy przystąpić do wdrożenia szablonu maszyn wirtualnych. Zanotuj identyfikator URI tajny z wyników (jako wyróżnione w poprzednim danych wyjściowych w zielony).

Konieczne będzie szablonu znajdującego się w tym miejscu. Parametry interesujące (oprócz zwykłych parametry maszyn wirtualnych) są nazwa magazynu, grupa zasobów magazynu i tajne identyfikatora URI. Oczywiście można również pobrać GitHub i zmodyfikuj stosownie do potrzeb.

Po wdrożeniu ten maszyn wirtualnych Azure wstawia go certyfikat do maszyn wirtualnych.
W systemie Windows certyfikaty plik PFX są dodawane z kluczem prywatnym, którego nie można wyeksportować. Certyfikat jest dodawana do lokalizacji certyfikatu LocalMachine, przy użyciu magazynie certyfikatów dostarczonego przez użytkownika. W systemie Linux, plik certyfikatu jest umieszczony w katalogu /var/lib/waagent z nazwą pliku &lt;UppercaseThumbprint&gt;.crt dla X509 plik certyfikatu i &lt;UppercaseThumbprint&gt;.prv dla klucz prywatny.
Obydwa typy plików są .pem sformatowany.

Aplikacja zazwyczaj znajduje certyfikat za pomocą odcisku palca i nie wymaga zmiany.

## <a name="retiring-certificates"></a>Certyfikaty na emeryturę


W poprzedniej sekcji możemy pokazano możesz sposób przekazać nowego certyfikatu do swojego istniejącego maszyny wirtualne. Ale stare certyfikatu jest nadal maszyn wirtualnych i nie można usunąć. Aby zwiększyć bezpieczeństwo możesz zmienić atrybut stare hasło na "Wyłączone" tak, aby nawet w przypadku starego szablonu próbuje Tworzenie maszyn wirtualnych przy użyciu tej starszej wersji certyfikatu, będzie. Poniżej opisano, jak ustawić określoną wersję tajne można wyłączyć:

    Set-AzureKeyVaultSecretAttribute -VaultName contosovault -Name servicecert -Version e3391a126b65414f93f6f9806743a1f7 -Enable 0

## <a name="conclusion"></a>Wnioski


Ta metoda nowego certyfikatu można rozdzielić z obrazu maszyn wirtualnych lub ładunku aplikacji. Aby usunięto jeden punkt zagrożeń.

Certyfikat można także odnowić i przekazane do magazynu klucza bez konieczności ponownego tworzenia obrazu maszyn wirtualnych lub pakiet wdrażania aplikacji. Aplikacja nadal musi jednak dostarczane z nowego identyfikatora URI dla tej nowej wersji certyfikatu.

Dzięki oddzieleniu certyfikat z maszyn wirtualnych lub ładunku aplikacji, możemy teraz zredukowany liczba pracowników, mających bezpośredni dostęp do certyfikatu.

Dodatkową korzyścią teraz masz jeden wygodnie w magazynu klucz do zarządzania własnych certyfikatów, w tym wszystkie wersje wdrożone w czasie.

## <a name="next-steps"></a>Następne kroki

[Wdrażanie maszyn wirtualnych za pomocą hasła klucza magazynu](azure-stack-kv-deploy-vm-with-secret.md)

[Zezwalanie aplikacji uzyskać dostęp do klucza magazynu](azure-stack-kv-sample-app.md)