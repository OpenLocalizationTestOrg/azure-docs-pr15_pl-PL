<properties
 pageTitle="Rozszerzenia maszyn wirtualnych i funkcje | Microsoft Azure"
 description="Dowiedz się, jakie rozszerzenia są dostępne dla Azure maszyn wirtualnych, pogrupowane według co oni podać albo jak poprawić."
 services="virtual-machines-linux"
 documentationCenter=""
 authors="neilpeterson"
 manager="timlt"
 editor=""
 tags="azure-service-management,azure-resource-manager"/>

<tags
 ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="infrastructure-services"
 ms.date="09/22/2016"
 ms.author="nepeters"/>

# <a name="about-virtual-machine-extensions-and-features"></a>Informacje o rozszerzenia maszyn wirtualnych i funkcje

## <a name="azure-vm-extensions"></a>Rozszerzenia Azure maszyn wirtualnych

Azure rozszerzenia maszyn wirtualnych są małych aplikacji, które mają zadania konfiguracji i automatyzacji wdrożenia wpis na maszyn wirtualnych Azure. Na przykład jeśli maszyny wirtualnej wymaga oprogramowania ochrony zainstalowane i oprogramowania antywirusowego lub Docker konfiguracji, rozszerzeniem maszyn wirtualnych może służyć do zakończenia tych zadań. Azure rozszerzenia maszyn wirtualnych mogą być uruchamiane przy użyciu interfejsu wiersza Azure pod polecenia programu PowerShell, zarządzanie zasobów szablony i Azure portal. Rozszerzenia mogą być dołączany do nowego wdrażania maszyn wirtualnych lub uruchamiana dowolnego istniejącego systemu.

Niniejszy dokument zawiera wymagania wstępne dotyczące rozszerzenia maszyn wirtualnych Azure i wskazówki na temat wykrywanie dostępne rozszerzenia maszyn wirtualnych. 

## <a name="azure-vm-agent"></a>Agent Azure maszyn wirtualnych

Agent maszyn wirtualnych Azure zarządza interakcji między maszyn wirtualnych Azure i kontroler tkaninie Azure. Agent maszyn wirtualnych jest odpowiedzialny za wiele funkcjonalności aspektów wdrażania i zarządzania maszyn wirtualnych Azure, łącznie z rozszerzeniami maszyn wirtualnych. Agent maszyn wirtualnych Azure preinstalowane w systemie zdjęcia z galerii Azure i można zainstalować na obsługiwane systemy operacyjne. 

Informacji na temat obsługiwane systemy operacyjne i instrukcje dotyczące instalacji zobacz [Podręcznik użytkownika agenta Linux Azure](./virtual-machines-linux-agent-user-guide.md).

## <a name="discover-vm-extensions"></a>Poznawanie rozszerzenia maszyn wirtualnych

Wiele różnych rozszerzeń maszyn wirtualnych są dostępne do użytku z maszyn wirtualnych Azure. Aby wyświetlić pełną listę, uruchom następujące polecenie z polecenie Azure, zamieniając lokalizacji lokalizację.

```none
azure vm extension-image list westus
```

<br />

## <a name="common-vm-extensions"></a>Typowe rozszerzenia maszyn wirtualnych

|Nazwa rozszerzenia   |Opis   |Więcej informacji   |
|---|---|---|
|Rozszerzenie skryptu niestandardowego Linux  | Uruchamianie skryptów przed maszyn wirtualnych Azure  |[Rozszerzenie skryptu niestandardowego Linux](./virtual-machines-linux-extensions-customscript.md)   |
|Rozszerzenie docker |Instaluje demona Docker do obsługi poleceń Docker zdalnych.  | [Rozszerzenie maszyn wirtualnych docker](./virtual-machines-linux-dockerextension.md)  |
|Rozszerzenie programu Access maszyn wirtualnych | Odzyskać dostęp do maszyn wirtualnych Azure  |[Rozszerzenie programu Access maszyn wirtualnych](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) |
|Rozszerzenie diagnostyki Azure |Zarządzanie diagnostyki Azure |[Rozszerzenie diagnostyki Azure](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |

