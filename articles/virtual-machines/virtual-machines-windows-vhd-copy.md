<properties
    pageTitle="Tworzenie kopii specjalistyczne maszyn wirtualnych w Azure | Microsoft Azure"
    description="Dowiedz się, jak utworzyć kopię specjalistyczne maszyn wirtualnych systemu Windows działa w Azure, w modelu wdrożenia Menedżera zasobów."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="cynthn"/>
    
    
    
# <a name="create-a-copy-of-a-specialized-windows-vm-running-in-azure"></a>Tworzenie kopii specjalistyczne maszyn wirtualnych systemu Windows z platformy Azure 

W tym artykule pokazano, jak korzystać z narzędzia AZCopy, aby utworzyć kopię wirtualny dysk twardy z specjalistyczne maszyn wirtualnych systemu Windows, uruchomionym w Azure. Kopię wirtualny dysk twardy można następnie umożliwia utworzenie nowego maszyn wirtualnych. 

- Jeśli chcesz skopiować uogólniony maszyn wirtualnych, zobacz, [jak utworzyć obraz maszyn wirtualnych z istniejących uogólniony maszyn wirtualnych Azure](virtual-machines-windows-capture-image.md).

- Jeśli chcesz przekazać wirtualnego dysku twardego z Głosowa lokalnego, podobny do utworzony przy użyciu funkcji Hyper-V, zobacz [przekazywanie wirtualnego dysku twardego systemu Windows z maszyn wirtualnych lokalnego Azure](virtual-machines-windows-upload-image.md).


## <a name="before-you-begin"></a>Przed rozpoczęciem

Upewnij się, że możesz:

- Masz informacji na temat **źródłowa i docelowa kont miejsca do magazynowania**. Źródła maszyn wirtualnych należy nazwy konta i kontener miejsca do magazynowania. Zazwyczaj nazwa kontenera będzie **wirtualnych dysków twardych**. Można również muszą być konto docelowego miejsca do magazynowania. Jeśli nie masz jeszcze jedną, możesz utworzyć jeden za pomocą dowolnej portalu (**Więcej usług** > miejsca do magazynowania konta > Dodaj) lub przy użyciu polecenia cmdlet [New-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) . 

- Ma Azure [PowerShell 1.0](../powershell-install-configure.md) (lub nowszy) zainstalowany.

- Zostały pobrane i zainstalowane [Narzędzia AzCopy](../storage/storage-use-azcopy.md). 


## <a name="deallocate-the-vm"></a>Cofnąć maszyn wirtualnych

Cofnąć Głosowa, która zwolnienie wirtualny dysk twardy do skopiowania. 

- **Portal**: kliknij pozycję **maszyn wirtualnych** > **myVM** > Zatrzymaj
- **PowerShell**: `Stop-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM` zwalnia maszyn wirtualnych, o nazwie **myVM** w grupie zasobów **myResourceGroup**.

**Stan** maszyn wirtualnych w portalu Azure zmienia się z **zatrzymania** **zatrzymania (alokację)**.


## <a name="get-the-storage-account-urls"></a>Uzyskiwanie konta adresy URL miejsca do magazynowania

Potrzebujesz adresy URL kont miejsca do magazynowania źródłowej i docelowej. Jak wygląda adresy URL: `https://<storageaccount>.blob.core.windows.net/<containerName>/`. Jeśli znasz nazwę konta i kontener miejsca do magazynowania, możesz po prostu zastąpić informacji między nawiasami do utworzenia adresu URL. 

Azure portal lub Azure programu Powershell pozwala uzyskać adres URL:

- **Portal**: kliknij pozycję **więcej usług** > **kont miejsca do magazynowania**  >  <storage account> **obiektów blob** i plik źródłowy wirtualnego dysku twardego prawdopodobnie znajduje się w kontenerze **wirtualnych dysków twardych** . Kliknij pozycję **Właściwości** dla kontenera, a następnie skopiuj tekst etykietą **adresu URL**. Konieczne będzie adresy URL kontenerów źródłowa i docelowa. 

- **PowerShell**: `Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"` pobiera dane dla maszyn wirtualnych o nazwie **myVM** w grupie zasobów **myResourceGroup**. W wynikach zajrzyj do sekcji **profilu miejsca do magazynowania** dla **Identyfikatora Uri wirtualnego dysku twardego**. Pierwsza część identyfikator Uri jest adres URL do kontenera i ostatniej części jest nazwą OS wirtualnego dysku twardego maszyn wirtualnych.

## <a name="get-the-storage-access-keys"></a>Uzyskiwanie klawiszy dostępu miejsca do magazynowania

Klawisze dostępu dla znaleźć źródłowa i docelowa kont miejsca do magazynowania. Aby uzyskać więcej informacji na temat klawisze dostępu Zobacz [temat Azure miejsca do magazynowania konta](../storage/storage-create-storage-account.md).

- **Portal**: kliknij pozycję **więcej usług** > **kont miejsca do magazynowania**  >  <storage account> **Wszystkie ustawienia** > **klawiszy dostępu**. Kopiowanie klucza oznaczony jako **klucz1**.
- **PowerShell**: `Get-AzureRmStorageAccountKey -Name mystorageaccount -ResourceGroupName myResourceGroup` w grupie zasobów **myResourceGroup**otrzymuje klucz miejsca do magazynowania dla konta magazynu **mystorageaccount** . Kopiowanie klucza oznaczony jako **klucz1**.


## <a name="copy-the-vhd"></a>Skopiuj wirtualny dysk twardy 

Możesz skopiować pliki między kontami miejsca do magazynowania przy użyciu AzCopy. Kontenera docelowego jeśli określonym kontenerze nie istnieje, go będą tworzone dla Ciebie. 

Aby użyć AzCopy, otwórz wiersz polecenia na komputerze lokalnym, a następnie przejdź do folderu, w którym zainstalowano AzCopy. Będzie podobny do *\Microsoft SDKs\Azure\AzCopy C:\Program Files (x86)*. 

Aby skopiować wszystkie pliki znajdujące się w kontenerze, należy użyć przełącznika **/S** . To może służyć do kopiowania OS wirtualnego dysku twardego i wszystkie dane, jeśli znajdują się w tym samym kontenerze. W tym przykładzie pokazano sposób skopiuj wszystkie pliki w kontenerze **mysourcecontainer** w przestrzeni dyskowej konta **mysourcestorageaccount** do kontenera **mydestinationcontainer** na koncie **mydestinationstorageaccount** miejsca do magazynowania. Zamień nazwy konta miejsca do magazynowania i kontenerów na własny. Zamienianie `<sourceStorageAccountKey1>` i `<destinationStorageAccountKey1>` z własnych klawiszy.

```
    AzCopy /Source:https://mysourcestorageaccount.blob.core.windows.net/mysourcecontainer `
        /Dest:https://mydestinationatorageaccount.blob.core.windows.net/mydestinationcontainer `
        /SourceKey:<sourceStorageAccountKey1> /DestKey:<destinationStorageAccountKey1> /S
```

Jeśli chcesz skopiować określonego wirtualnego dysku twardego w kontenerze z wielu plików, możesz określić nazwę pliku za pomocą przełącznika /Pattern. W tym przykładzie zostaną skopiowane tylko plik o nazwie **myFileName.vhd** .

```
    AzCopy /Source:https://mysourcestorageaccount.blob.core.windows.net/mysourcecontainer `
        /Dest:https://mydestinationatorageaccount.blob.core.windows.net/mydestinationcontainer `
        /SourceKey:<sourceStorageAccountKey1> /DestKey:<destinationStorageAccountKey1> `
        /Pattern:myFileName.vhd
```


Po zakończeniu, zostanie wyświetlony komunikat, który może wyglądać:

```
  Finished 2 of total 2 file(s).
  [2016/10/07 17:37:41] Transfer summary:
  -----------------
  Total files transferred: 2
  Transfer successfully:   2
  Transfer skipped:        0
  Transfer failed:         0
  Elapsed time:            00.00:13:07
```

## <a name="troubleshooting"></a>Rozwiązywanie problemów

- Kiedy używać AZCopy, jeśli zostanie wyświetlony błąd "nie można uwierzytelnić żądania serwer. Upewnij się, że wartość nagłówka autoryzacji został utworzony poprawnie w tym podpis. " i korzystasz z 2 klucza lub klucza magazynu pomocniczego, spróbuj użyć klucza podstawowego lub 1st magazynowania danych.


## <a name="next-steps"></a>Następne kroki

- Możesz utworzyć nowy maszyn wirtualnych dołączając [kopię wirtualny dysk twardy do maszyny jako dysk systemu operacyjnego](virtual-machines-windows-create-vm-specialized.md).












