<properties
   pageTitle="Podstawowe DotNet Azure maszyn wirtualnych samouczka 1 | Microsoft Azure"
   description="Samouczek DotNet Core Azure maszyn wirtualnych"
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="neilpeterson"
   manager="timlt"
   editor="tysonn"
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="09/21/2016"
   ms.author="nepeters"/>

# <a name="automating-application-deployments-to-azure-virtual-machines"></a>Automatyzowanie wdrażaniem aplikacji do maszyn wirtualnych Azure

Czteroczęściowa serii szczegóły, wdrażania i konfigurowania Azure zasobów i aplikacji przy użyciu Azure zasobów Zarządzanie szablonami. W tej serii przykładowy szablon zostanie wdrożony i zbadać szablonu wdrożenia. Celem tej serii jest zapoznanie na relacje między Azure zasobów i umożliwiają dłonie środowisku wdrażanie szablonów w pełni zintegrowany Azure Menedżera zasobów. Podstawowy poziom wiedzy przy użyciu Menedżera zasobów Azure przyjęto założenie, ten dokument, przed rozpoczęciem tego samouczka poznawanie podstawowe pojęcia Azure Menedżera zasobów.

## <a name="music-store-application"></a>Aplikacja magazynu muzyki

Przykładowe używane w tej serii jest .net podstawową aplikację symulowanie sklep muzyczny, atrakcyjne. Ta aplikacja może zostać rozmieszczony na Linux lub Windows wirtualnych systemu, wdrożeń próbki zostały utworzone dla obu. Aplikacja zawiera aplikacji sieci web i baza danych SQL. Przed czytanie artykuły w tej serii, należy wdrożyć aplikację za pomocą przycisku wdrożenia znajdujące się na tej stronie. Po wdrożeniu w pełni Architektura aplikacji / Azure wygląda na poniższym diagramie. 

Szablon Menedżera zasobów magazynu muzyki można znaleźć tutaj, [Muzyka sklepu Linux szablonu]( https://github.com/neilpeterson/nepeters-azure-templates/tree/master/dotnet-core-music-linux-vm-sql-db)

![Sklep muzyczny aplikacji](./media/virtual-machines-linux-dotnet-core/music-store.png)

Każdy z tych składników, w tym szablonie kojarzenie JSON jest badany w następujących artykułach cztery.

- [**Architektura aplikacji**](./virtual-machines-linux-dotnet-core-2-architecture.md) — składniki aplikacji, takich jak witryny sieci web i baz danych muszą być przechowywana na komputerze Azure zasobów, takich jak maszyn wirtualnych i baz danych programu SQL Azure. Ten dokument opisano potrzeba obliczeń mapowania Azure zasoby i wdrażanie te zasoby z szablonem Azure Menedżera zasobów. 

- [**Program access i zabezpieczenia**](./virtual-machines-linux-dotnet-core-3-access-security.md) — hostingu aplikacji platformy Azure, należy brać pod uwagę jak aplikacja jest dostępny i różnych aplikacji składniki siebie. Ten dokument, szczegóły, dostarczając i zabezpieczanie dostęp do Internetu, aplikacji i dostęp między składnikami aplikacji.

- [**Dostępność i skali**](./virtual-machines-linux-dotnet-core-4-availability-scale.md) — dostępności i skali odnoszą się do aplikacji możliwość pozostają uruchomione podczas przestoje infrastruktury i możliwość skalowania zasobów obliczeń w celu spełnienia wymagań aplikacji. Szczegóły dokumentu składniki potrzebne do wdrożenia równoważenia obciążenia i wysokiej dostępności aplikacji.

- [**Wdrażanie aplikacji**](./virtual-machines-linux-dotnet-core-5-app-deployment.md) — podczas wdrażania aplikacji na maszyn wirtualnych Azure, należy rozważyć metody za pomocą której plików binarnych aplikacji są zainstalowane na komputerze wirtualnych. Ten dokument, szczegóły automatyzacji instalacji aplikacji przy użyciu Azure maszyn wirtualnych niestandardowy skrypt rozszerzenia.

Cel: podczas tworzenia szablonów Azure Menedżera zasobów jest automatyzowania wdrażania infrastruktury Azure i instalowania i konfigurowania aplikacji są obsługiwane na tym Azure infrastruktury. Pracy przez te artykuły znajdują się z przykładem tego środowiska.

## <a name="deploy-the-music-store-application"></a>Wdrażanie aplikacji ze sklepu muzyki

Za pomocą tego przycisku można wdrożyć aplikacji sklep muzyczny.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicrosoft%2Fdotnet-core-sample-templates%2Fmaster%2Fdotnet-core-music-linux%2Fazuredeploy.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>

Szablon Menedżera zasobów Azure wymaga następujące wartości parametrów.

|Nazwa parametru |Opis   |
|---|---|
|SSHKEYDATA   | SSH danych bezpieczny dostęp do maszyny wirtualnej. Aby uzyskać informacje na temat tworzenia parę kluczy SSH zobacz [Tworzenie SSH klawisze służące do maszyny wirtualne Linux platformy Azure](virtual-machines-linux-mac-create-ssh-keys.md).  |
|ADMINUSERNAME   | Nazwa użytkownika administratora jest używana na komputerze wirtualnych i bazy danych SQL Azure.  |
|SQLADMINPASSWORD | Hasło, które jest używane w bazie danych SQL Azure.  |
|NUMBEROFINSTANCES | Liczba maszyn wirtualnych do utworzenia. Każdy z tych maszyn wirtualnych hostem aplikacji sieci web sklep muzyczny, a cały ruch jest równoważenia wokół nich obciążenia. |
|PUBLICIPADDRESSDNSNAME | Globalne unikatowa nazwa DNS skojarzone z publiczny adres IP. |

Po zakończeniu rozmieszczania szablonu, przejdź do publiczny adres IP za pomocą dowolnej przeglądarki internetowej. .Net muzyczny Core witryny zostanie wyświetlona.

## <a name="next-steps"></a>Następne kroki

<hr>

[Krok 1 — architektura Azure szablony Menedżera zasobów](./virtual-machines-linux-dotnet-core-2-architecture.md)

[Krok 2 — program Access i zabezpieczenia w szablonach Azure Menedżera zasobów](./virtual-machines-linux-dotnet-core-3-access-security.md)

[Krok 3 — dostępności i skali w Azure szablony Menedżera zasobów](./virtual-machines-linux-dotnet-core-4-availability-scale.md)

[Krok 4 — wdrażaniem aplikacji za pomocą Azure szablony Menedżera zasobów](./virtual-machines-linux-dotnet-core-5-app-deployment.md)


