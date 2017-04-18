<properties
 pageTitle="Zarządzanie HPC Pack węzłach obliczeń | Microsoft Azure"
 description="Więcej informacji na temat narzędzia skrypt programu PowerShell do dodawania, usuwania, Rozpocznij i Zatrzymaj HPC Pack obliczeń węzłach platformy Azure"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="big-compute"
 ms.date="07/22/2016"
 ms.author="danlep"/>

# <a name="manage-the-number-and-availability-of-compute-nodes-in-an-hpc-pack-cluster-in-azure"></a>Zarządzanie numer i dostępność węzłów do uruchamiania w klastrze HPC Pack platformy Azure

Jeśli utworzono klastrze HPC Pack w maszyny wirtualne Azure, może być sposoby łatwe dodawanie, usuwanie, uruchom (Obsługa administracyjna) lub zatrzymywanie węzła (deprovision) liczba obliczeń maszyny wirtualne w klastrze. Aby wykonać te zadania, należy uruchomić skryptów programu PowerShell Azure, które są zainstalowane na węzła głównego maszyn wirtualnych. Te skrypty można kontrolować liczbę i dostępność zasobów klaster HPC Pack, więc możesz sterować kosztów.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


## <a name="prerequisites"></a>Wymagania wstępne

* **Klaster HPC Pack maszyny wirtualne Azure** - tworzenie klastrze HPC Pack w modelu Klasyczny wdrażania przy użyciu co najmniej HPC Pack 2012 R2 aktualizacji 1. Na przykład wdrażania można zautomatyzować przy użyciu bieżącego obrazu maszyn wirtualnych Pack HPC Azure Marketplace i skrypt programu Azure PowerShell. Dla informacji i wymagania wstępne zobacz [Tworzenie klastrze HPC scenariusz wdrażania HPC Pack IaaS](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md).

    Po wdrożeniu, Znajdź węzeł skrypty zarządzania % CCP\_główne % Kosz z poziomu folderu na węzła głównego. Należy uruchomić wszystkich skryptów jako administrator.

* **Azure publikowanie pliku lub zarządzanie certyfikatu ustawień** - należy wykonać jedną z poniższych czynności na węzła głównego:

    * **Importowanie Azure publikowanie plik ustawień**. Aby to zrobić, uruchom następujące polecenia cmdlet programu PowerShell Azure na węzła głównego:

    ```
    Get-AzurePublishSettingsFile

    Import-AzurePublishSettingsFile –PublishSettingsFile <publish settings file>
    ```

    * **Konfigurowanie certyfikatu zarządzania Azure na węzła głównego**. Jeśli masz plik cer, zaimportuj go w magazynie certyfikatów CurrentUser\My, a następnie uruchom następujące polecenie cmdlet programu PowerShell Azure w środowisku usługi Azure (AzureCloud lub AzureChinaCloud):

    ```
    Set-AzureSubscription -SubscriptionName <Sub Name> -SubscriptionId <Sub ID> -Certificate (Get-Item Cert:\CurrentUser\My\<Cert Thrumbprint>) -Environment <AzureCloud | AzureChinaCloud>
    ```

## <a name="add-compute-node-vms"></a>Dodawanie obliczeń węzeł maszyny wirtualne

Dodaj obliczyć węzły za pomocą skryptu **HpcIaaSNode.ps1 Dodaj** .

### <a name="syntax"></a>W składni
```
Add-HPCIaaSNode.ps1 [-ServiceName] <String> [-ImageName] <String>
 [-Quantity] <Int32> [-InstanceSize] <String> [-DomainUserName] <String> [[-DomainUserPassword] <String>]
 [[-NodeNameSeries] <String>] [<CommonParameters>]

```
### <a name="parameters"></a>Parametry

* **NazwaUsługi** - nazwa chmury usługi tego nowego węzła obliczeń, maszyny wirtualne zostaną dodane do.

* **Nazwa_obrazu** - maszyn wirtualnych Azure nazwa obrazu, który można uzyskać za pośrednictwem portalu klasyczny Azure lub Azure programu PowerShell polecenia cmdlet **Get-AzureVMImage**. Obraz musi spełniać następujące wymagania:

    1. Musi być zainstalowany w systemie operacyjnym Windows.

    2. Musi być zainstalowany pakiet HPC w roli węzła obliczeń.

    3. Obraz musi być prywatne obraz w kategorii użytkownika nie publicznej obraz maszyn wirtualnych Azure.

* **Ilość** — liczby węzłów przetwarzania maszyny wirtualne do dodania.

* **InstanceSize** — rozmiar węzła obliczeń maszyny wirtualne.

* **DomainUserName** - nazwa użytkownika domeny, która będzie używana do Dołącz nowe maszyny wirtualne do domeny.

* **DomainUserPassword** - hasło użytkownika domeny.

* **NodeNameSeries** (opcjonalnie) - nazw deseniu węzły obliczeń. Format musi być &lt; *główny\_nazwa*&gt;&lt;*rozpocząć\_liczbę*&gt;%. Na przykład MyCN % 10% oznacza serię nazwy węzłów obliczeń, rozpoczynając od MyCN11. Jeśli nie zostanie określony, skrypt używa węzeł skonfigurowanych nazw serii w klastrze HPC.

### <a name="example"></a>Przykład

W poniższym przykładzie dodawane 20 węzeł obliczeń duży rozmiar maszyny wirtualne w chmurze usługi *hpcservice1*, oparte na obraz maszyn wirtualnych *hpccnimage1*.

```
Add-HPCIaaSNode.ps1 –ServiceName hpcservice1 –ImageName hpccniamge1
–Quantity 20 –InstanceSize Large –DomainUserName <username>
-DomainUserPassword <password>
```


## <a name="remove-compute-node-vms"></a>Usuń węzeł obliczeń maszyny wirtualne

Usuń obliczyć węzły za pomocą skryptu **HpcIaaSNode.ps1 Usuń** .

### <a name="syntax"></a>W składni

```
Remove-HPCIaaSNode.ps1 -Name <String[]> [-DeleteVHD] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]

Remove-HPCIaaSNode.ps1 -Node <Object> [-DeleteVHD] [-Force] [-Confirm] [<CommonParameters>]
```

### <a name="parameters"></a>Parametry

* **Nazwa** - nazwy węzłach do usunięcia. Symbole wieloznaczne są obsługiwane. Nazwa zestawu parametru jest. Nie można określić **nazwę** i **węzeł** parametry.

* **Węzeł** — obiekt HpcNode dla węzłów do usunięcia, który można uzyskać za pomocą polecenia cmdlet [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx)HPC programu PowerShell. Nazwa zestawu parametru jest węzeł. Nie można określić **nazwę** i **węzeł** parametry.

* **DeleteVHD** (opcjonalnie) — ustawienie go usunąć skojarzone dysków maszyny wirtualne, które zostaną usunięte.

* **Wymuszanie** (opcjonalnie) — ustawianie wymusić węzły HPC w trybie offline przed ich usunięcie.

* **Upewnij się** (opcjonalnie) — Monituj o potwierdzenie przed wykonaniem polecenia.

* **Także** — Ustawianie opisano, co się stanie, jeśli polecenie zostanie wykonane bez rzeczywistego wykonania polecenia.

### <a name="example"></a>Przykład

Poniższy przykład wymusza offline węzły z nazwy rozpoczynające się *HPCNode-CN -* i ich usuwa węzły i jego skojarzony dysków.

```
Remove-HPCIaaSNode.ps1 –Name HPCNodeCN-* –DeleteVHD -Force
```

## <a name="start-compute-node-vms"></a>Rozpoczynanie węzeł obliczeń maszyny wirtualne

Rozpocznij obliczyć węzły za pomocą skryptu **Start HpcIaaSNode.ps1** .

### <a name="syntax"></a>W składni

```
Start-HPCIaaSNode.ps1 -Name <String[]> [<CommonParameters>]

Start-HPCIaaSNode.ps1 -Node <Object> [<CommonParameters>]
```
### <a name="parameters"></a>Parametry

* **Nazwa** - nazw przez klaster ma zostać uruchomiony. Obsługiwane są symbole wieloznaczne. Nazwa zestawu parametru jest. Nie można określić **nazwę** i **węzeł** parametry.

* **Węzeł**— obiekt HpcNode dla węzłów ma zostać uruchomiony, który można uzyskać za pomocą polecenia cmdlet [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx)HPC programu PowerShell. Nazwa zestawu parametru jest węzeł. Nie można określić **nazwę** i **węzeł** parametry.

### <a name="example"></a>Przykład

Węzły w poniższym przykładzie zaczyna się od nazwy rozpoczynające się *HPCNode-CN —*.

```
Start-HPCIaaSNode.ps1 –Name HPCNodeCN-*
```

## <a name="stop-compute-node-vms"></a>Zatrzymywanie węzłów przetwarzania maszyny wirtualne

Zatrzymaj obliczyć węzły za pomocą skryptu **HpcIaaSNode.ps1 tabulatora** .

### <a name="syntax"></a>W składni

```
Stop-HPCIaaSNode.ps1 -Name <String[]> [-Force] [<CommonParameters>]

Stop-HPCIaaSNode.ps1 -Node <Object> [-Force] [<CommonParameters>]
```

### <a name="parameters"></a>Parametry


* **Nazwa**- nazw przez klaster, aby je zatrzymać. Symbole wieloznaczne są obsługiwane. Nazwa zestawu parametru jest. Nie można określić **nazwę** i **węzeł** parametry.

* **Węzeł** — obiekt HpcNode dla węzłów ma zostać zatrzymany, który można uzyskać za pomocą polecenia cmdlet [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx)HPC programu PowerShell. Nazwa zestawu parametru jest węzeł. Nie można określić **nazwę** i **węzeł** parametry.

* **Wymuszanie** (opcjonalnie) — ustawianie wymuszanie węzły HPC w trybie offline przed zatrzymaniem je.

### <a name="example"></a>Przykład

Poniższy przykład wymusza offline węzły z nazwami zaczyna się *HPCNode-CN -* , a następnie zatrzymuje węzły.

Zatrzymaj — HPCIaaSNode.ps1 — HPCNodeCN nazwa-*-życie

## <a name="next-steps"></a>Następne kroki

* Jeśli chcesz umożliwia automatyczne Zwiększ lub Zmniejsz węzłach zgodnie z bieżącą pracą zadań i zadań w klastrze, zobacz [automatyczne powiększanie i zmniejszanie zasobów klaster HPC Pack w Azure zgodnie z pracą klaster](virtual-machines-windows-classic-hpcpack-cluster-node-autogrowshrink.md).
