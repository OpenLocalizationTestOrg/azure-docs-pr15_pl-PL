<properties
    pageTitle="Konfigurowanie ustawień integracji Azure klucza magazynu dla programu SQL Server na Azure maszyny wirtualne (klasyczny)"
    description="Dowiedz się, jak do automatyzowania konfiguracji programu SQL Server szyfrowania do użytku z magazynu klucza Azure. W tym temacie wyjaśniono, jak korzystać z Azure klucza magazynu integracji z programem SQL Server w środowisku maszyn wirtualnych systemu tworzyć w modelu Klasyczny wdrożenia."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/26/2016"
    ms.author="jroth"/>

# <a name="configure-azure-key-vault-integration-for-sql-server-on-azure-vms-classic"></a>Konfigurowanie ustawień integracji Azure klucza magazynu dla programu SQL Server na Azure maszyny wirtualne (klasyczny)

> [AZURE.SELECTOR]
- [Menedżer zasobów](virtual-machines-windows-ps-sql-keyvault.md)
- [Klasyczny](virtual-machines-windows-classic-ps-sql-keyvault.md)

## <a name="overview"></a>Omówienie
Istnieje wiele funkcji szyfrowania programu SQL Server, takich jak [szyfrowanie danych przezroczysty (TDE)](https://msdn.microsoft.com/library/bb934049.aspx), [szyfrowanie na poziomie kolumny (CLE)](https://msdn.microsoft.com/library/ms173744.aspx)i [szyfrowania kopii zapasowej](https://msdn.microsoft.com/library/dn449489.aspx). Te formy szyfrowania trzeba przechowywać klucze szyfrowania, których używasz na potrzeby szyfrowania i zarządzanie nimi. Usługa Azure klucza magazynu (AKV) ma na celu poprawę zabezpieczeń i zarządzania klucze w lokalizacji bezpieczne i wysokiej dostępności. [Program SQL Server Connector](http://www.microsoft.com/download/details.aspx?id=45344) umożliwia SQL Server, aby te klawisze z magazynu klucza Azure.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Jeśli korzystasz z programu SQL Server lokalnego maszyn, istnieje [czynności można wykonać, aby uzyskać dostęp do magazynu klucza Azure z komputera lokalnego programu SQL Server](https://msdn.microsoft.com/library/dn198405.aspx). Ale dla programu SQL Server w maszyny wirtualne Azure, można zaoszczędzić czas przy użyciu funkcji *Integracji magazynu klucza Azure* . Z kilku poleceń cmdlet środowiska PowerShell Azure Aby włączyć tę funkcję można zautomatyzować niezbędne do maszyn wirtualnych SQL uzyskać dostęp do swojego klucza magazynu konfiguracji.

Jeśli ta funkcja jest włączona, jej automatycznie instaluje program SQL Server Connector, konfiguruje dostawcy EKM, aby uzyskać dostęp do magazynu klucza Azure i tworzy poświadczeń, aby umożliwić użytkownikowi dostęp do magazynu. Jeśli elementu czynności opisane w dokumentacji lokalnych wcześniej wspomniano, widać, że ta funkcja pozwala zautomatyzować kroki 2 i 3. Tylko element, który jeszcze trzeba zrobić ręcznie jest utworzenie klucza magazynu oraz klawiszy. W tym miejscu jest zautomatyzowany wszystkie ustawienia maszyn wirtualnych usługi SQL. Po zakończeniu tej konfiguracji tej funkcji można wykonać instrukcji T SQL, aby rozpocząć szyfrowanie bazy danych lub z kopii zapasowych w normalny sposób.

[AZURE.INCLUDE [AKV Integration Prepare](../../includes/virtual-machines-sql-server-akv-prepare.md)]

## <a name="configure-akv-integration"></a>Konfigurowanie ustawień integracji AKV
Konfigurowanie integracji magazynu klucza Azure przy użyciu programu PowerShell. W poniższych sekcjach przedstawiono przegląd wymagane parametry, a następnie przykładowy skrypt programu PowerShell.

### <a name="install-the-sql-server-iaas-extension"></a>Zainstaluj rozszerzenie IaaS serwera SQL

Najpierw [Zainstaluj rozszerzenie IaaS serwera SQL](virtual-machines-windows-classic-sql-server-agent-extension.md).

### <a name="understand-the-input-parameters"></a>Opis parametrów wejściowych
W poniższej tabeli wymieniono parametrów wymaganych do uruchomienia skryptu programu PowerShell w następnej sekcji.

|Parametr|Opis|Przykład|
|---|---|---|
|**$akvURL**|**Adres URL klucza magazynu**|"https://contosokeyvault.vault.azure.net/"|
|**$spName**|**Nazwa wystawcy usługi**|"fde2b411 - 33d 5-4e11-af04eb07b669ccf2"|
|**$spSecret**|**Tajny wystawcy usługi**|"9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM ="|
|**$credName**|**Nazwa poświadczeń**: integracja z programem AKV tworzy poświadczeń w ramach programu SQL Server, co pozwala maszyn wirtualnych mieć dostęp do klucza magazynu. Wybierz inną nazwę dla tego poświadczenia.|"mycred1"|
|**$vmName**|**Nazwa maszyn wirtualnych**: nazwę utworzonego wcześniej maszyny SQL.|"myvmname"|
|**$serviceName**|**Nazwa usługi**: usługi w chmurze nazwę, którą jest skojarzony z maszyn wirtualnych SQL.|"mycloudservicename"|

### <a name="enable-akv-integration-with-powershell"></a>Włącz AKV integrację z programem PowerShell
Polecenia cmdlet **New-AzureVMSqlServerKeyVaultCredentialConfig** tworzy obiekt konfiguracji dla funkcji integracji magazynu klucza Azure. **Ustawianie AzureVMSqlServerExtension** konfiguruje Integracja z parametrem **KeyVaultCredentialSettings** . Poniższe kroki pokazują sposób używania tych poleceń.

1. W programie PowerShell Azure najpierw skonfigurować parametrów wejściowych wartościami określonych zgodnie z opisem w poprzednich sekcjach tego tematu. Poniższy skrypt jest na przykład.

        $akvURL = "https://contosokeyvault.vault.azure.net/"
        $spName = "fde2b411-33d5-4e11-af04eb07b669ccf2"
        $spSecret = "9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM="
        $credName = "mycred1"
        $vmName = "myvmname"
        $serviceName = "mycloudservicename"
2.  Aby skonfigurować i włączyć integrację z programem AKV następnie skorzystaj z tego skryptu.

        $secureakv =  $spSecret | ConvertTo-SecureString -AsPlainText -Force
        $akvs = New-AzureVMSqlServerKeyVaultCredentialConfig -Enable -CredentialName $credname -AzureKeyVaultUrl $akvURL -ServicePrincipalName $spName -ServicePrincipalSecret $secureakv
        Get-AzureVM -ServiceName $serviceName -Name $vmName | Set-AzureVMSqlServerExtension -KeyVaultCredentialSettings $akvs | Update-AzureVM

Rozszerzenie agenta IaaS SQL zaktualizuje maszyn wirtualnych SQL w tej nowej konfiguracji.

[AZURE.INCLUDE [AKV Integration Next Steps](../../includes/virtual-machines-sql-server-akv-next-steps.md)]
