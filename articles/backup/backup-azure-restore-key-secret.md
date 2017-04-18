<properties
    pageTitle="Przywracanie klucza magazynu klucza i hasło dla szyfrowanego maszyny wirtualne przy użyciu kopii zapasowej Azure | Microsoft Azure"
    description="Dowiedz się, jak przywrócić klucz magazynu klucz i tajny w kopii zapasowej Azure przy użyciu programu PowerShell"
    services="backup"
    documentationCenter=""
    authors="JPallavi"
    manager="vijayts"
    editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="JPallavi" />

# <a name="restore-key-vault-key-and-secret-for-encrypted-vms-using-azure-backup"></a>Przywracanie klucza magazynu klucza i hasło dla zaszyfrowanych maszyny wirtualne przy użyciu kopii zapasowej Azure
Ten artykuł zawiera informacje o przy użyciu kopii zapasowych maszyn wirtualnych Azure do przywracania zaszyfrowanej Azure pośrednictwem SMS, jeśli klucz, a hasło nie istnieje w klucza magazynu. Również można te kroki, jeśli chcesz zachować osobną kopię klawisz (klucza szyfrowania) i hasła (klucza szyfrowania funkcji BitLocker) dla przywrócenie maszyn wirtualnych.

## <a name="pre-requisites"></a>Wymagania wstępne

1. **Wykonywanie kopii zapasowych zaszyfrowanych maszyny wirtualne** - szyfrowane maszyny wirtualne Azure zostały kopii zapasowej przy użyciu kopii zapasowej Azure. Uzyskać szczegółowe informacje dotyczące wykonywania kopii zapasowej zaszyfrowanych Azure maszyny wirtualne można znaleźć w artykule [Zarządzanie kopia zapasowa i przywracanie maszyny wirtualne Azure przy użyciu programu PowerShell](backup-azure-vms-automation.md) .

2. **Konfigurowanie magazynu klucza Azure** — upewnij się, że klawisz magazynu, do którego klucze i hasła trzeba je przywrócić znajduje się już. Szczegółowe informacje na temat zarządzania klucza magazynu można znaleźć artykuł [Wprowadzenie do magazynu klucza Azure](../key-vault/key-vault-get-started.md) .

## <a name="setup-recovery-services-vault"></a>Ustawienia odzyskiwania usług magazynu 
Wykonaj następujące czynności, aby zalogować się do programu PowerShell i ustawić kontekstu magazynu usługi odzyskiwania

### <a name="log-in-to-azure-powershell"></a>Zaloguj się do programu PowerShell Azure 

Zaloguj się do konta Azure za pomocą następującego polecenia cmdlet

```
PS C:\> Login-AzureRmAccount
```

### <a name="set-recovery-services-vault-context"></a>Ustawianie odzyskiwania usług magazynu kontekstu

Po zalogowaniu się, aby wyświetlić listę dostępnych subskrypcji za pomocą następującego polecenia cmdlet

```
PS C:\> Get-AzureRmSubscription
```

Wybierz subskrypcję, w której są dostępne zasoby

```
PS C:\> Set-AzureRmContext -SubscriptionId "<subscription-id>"
```

Ustaw kontekst magazynu przy użyciu magazynu usługi odzyskiwania miejsce, w którym kopia zapasowa zostało włączone dla szyfrowanego maszyny wirtualne

```
PS C:\> Get-AzureRmRecoveryServicesVault -ResourceGroupName "<rg-name>" -Name "<rs-vault-name>" | Set-AzureRmRecoveryServicesVaultContext
```

### <a name="get-recovery-point"></a>Uzyskiwanie punkt odzyskiwania 

Wybierz kontener w magazyn reprezentujący zaszyfrowanych Azure maszyn wirtualnych

```
PS C:\> $namedContainer = Get-AzureRmRecoveryServicesBackupContainer -ContainerType "AzureVM" -Status "Registered" -Name "<vm-name>"
```

Za pomocą tego kontenera, wrócić o element dla odpowiednich maszyny wirtualnej w górę

```
PS C:\> $backupitem = Get-AzureRmRecoveryServicesBackupItem -Container $namedContainer -WorkloadType "AzureVM"
```

Uzyskiwanie tablicy punktów odzyskiwania dla zaznaczonego elementu kopii zapasowej w zmiennej rp

```
PS C:\> $startDate = (Get-Date).AddDays(-7)
PS C:\> $endDate = Get-Date
PS C:\> $rp = Get-AzureRmRecoveryServicesBackupRecoveryPoint -Item $backupitem -StartDate $startdate.ToUniversalTime() -EndDate $enddate.ToUniversalTime()
```

## <a name="restore-encrypted-virtual-machine"></a>Przywracanie zaszyfrowanych maszyn wirtualnych
Wykonaj następujące czynności, aby przywrócić zaszyfrowanych Głosowa, jego klucz i tajny.

### <a name="restore-key"></a>Przywracanie klucza

Tablica $rp powyżej, sortowane w odwrotnej kolejności czasu przy użyciu najnowszych punktu odzyskiwania indeksem 0. Na przykład: $rp [0] zaznacza najnowszą punkt odzyskiwania.

```
PS C:\> $rp1 = Get-AzureRmRecoveryServicesBackupRecoveryPoint -RecoveryPointId $rp[0].RecoveryPointId -Item $backupItem -KeyFileDownloadLocation "C:\Users\downloads"
```

> [AZURE.NOTE]
Po pomyślnym uruchomieniu tego polecenia cmdlet plików obiektów blob otrzymuje generowane w określonym folderze na komputerze, na którym jest uruchomiony. Ten plik obiektów blob reprezentuje klucz szyfrowane w zaszyfrowanej.

Przywracanie klucza wstecz do klucza magazyn przy użyciu następującego polecenia cmdlet. 

```
PS C:\> Restore-AzureKeyVaultKey -VaultName "contosokeyvault" -InputFile "C:\Users\downloads\key.blob"
```

### <a name="restore-secret"></a>Przywracanie hasła

Przywracanie tajne dane z punkt odzyskiwania otrzymanego powyżej

```
PS C:\> $rp1.KeyAndSecretDetails.SecretUrl

https://contosokeyvault.vault.azure.net/secrets/B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/20aaae9eaa99996d89d99a29990d999a
```

> [AZURE.NOTE]
Tekst przed vault.azure.net reprezentuje oryginalną nazwą klucza magazynu. Tekst po tajemnic / odpowiada nazwie tajne. 

Uzyskaj tajne nazwę i wartość z zestawu wyników polecenia cmdlet uruchamianie powyżej, w przypadku którego chcesz użyć tej samej nazwy tajne. W innych przypadkach $secretname poniżej powinny być aktualizowane korzystać z nową nazwą poufne. 

```
PS C:\> $secretname = "B3284AAA-DAAA-4AAA-B393-60CAA848AAAA"
PS C:\> $secretdata = $rp1.KeyAndSecretDetails.SecretData
PS C:\> $Secret = ConvertTo-SecureString -String $secretdata -AsPlainText -Force
```

Ustaw znaczniki dla tego hasła, w przypadku maszyn wirtualnych, należy je przywrócić także. Znacznik DiskEncryptionKeyFileName wartość powinien zawierać nazwę hasła, które ma zostać użyta. 

```
PS C:\> $Tags = @{'DiskEncryptionKeyEncryptionAlgorithm' = 'RSA-OAEP';'DiskEncryptionKeyFileName' = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA.BEK';'DiskEncryptionKeyEncryptionKeyURL' = 'https://contosokeyvault.vault.azure.net:443/keys/KeyName/84daaac999949999030bf99aaa5a9f9';'MachineName' = 'vm-name'}
```

> [AZURE.NOTE]
Wartość DiskEncryptionKeyFileName jest taki sam jak tajne nazwy pobranej powyżej. Wartość dla DiskEncryptionKeyEncryptionKeyURL można uzyskać z magazynu kluczy po ich przywracania ponownie klawisze i przy użyciu polecenia cmdlet [Get-AzureKeyVaultKey](https://msdn.microsoft.com/library/dn868053.aspx)   

Ustawianie tajne powrót do klucza magazynu

```
PS C:\> Set-AzureKeyVaultSecret -VaultName "contosokeyvault" -Name $secretname -SecretValue $secret -Tags $Tags -SecretValue $Secret -ContentType  "Wrapped BEK"
```

### <a name="restore-virtual-machine"></a>Przywracanie maszyn wirtualnych
Powyższe poleceń cmdlet środowiska PowerShell ułatwiają przywrócić klucza i z powrotem tajne klucza magazynu, jeśli wykonano kopię zapasową zaszyfrowanych maszyn wirtualnych przy użyciu kopii zapasowej maszyn wirtualnych Azure. Po przywracaniu, można znaleźć artykuł [Zarządzanie kopia zapasowa i przywracanie maszyny wirtualne Azure przy użyciu programu PowerShell](backup-azure-vms-automation.md) , aby przywrócić zaszyfrowanych maszyny wirtualne.
