<properties
   pageTitle="Azure Menedżera zasobów pomocy technicznej dla usługi równoważenia obciążenia | Microsoft Azure "
   description="Przy użyciu programu powershell dla usługi równoważenia obciążenia przy użyciu Menedżera zasobów Azure. Korzystanie z szablonów dla równoważenia obciążenia"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />


# <a name="using-azure-resource-manager-support-with-azure-load-balancer"></a>Azure Menedżera zasobów pomocy technicznej za pomocą usługi równoważenia obciążenia Azure

Azure Menedżer zasobów jest struktury preferowanej zarządzania dla usług w Azure. Azure równoważenia obciążenia można zarządzać, korzystając z oparte na Menedżera zasobów Azure interfejsy API i narzędzi.

## <a name="concepts"></a>Pojęcia

Przy użyciu Menedżera zasobów równoważenia obciążenia Azure zawiera następujące zasoby podrzędne:

- Konfiguracja IP zewnętrzną — równoważenia obciążenia może zawierać jeden lub więcej adresów IP fronton, nazywanego wirtualne adresy IP (służącą). Pełnienie roli ingress ruchu osoby te adresy IP.

- Pula adresów wewnętrznej — są adresy IP skojarzone z maszyn wirtualnych sieci karta sieciowa (NIC) do którego będą podzielone ładowania.

- Zasady równoważenia obciążenia — właściwości reguła mapy danego fronton IP i kombinację port do zestawu adresów IP wewnętrznej i kombinację port. Równoważenia obciążenia pojedynczy może zawierać wiele równoważenia reguły obciążenia. Każda reguła to kombinacja zewnętrzną IP i portów i wewnętrznej adresu IP i portu skojarzone z maszyny wirtualne.

- Sondy — sondy umożliwiają umożliwia śledzenie kondycji wystąpień maszyn wirtualnych. Jeśli sonda kondycji nie powiedzie się, w wystąpieniu maszyn wirtualnych zostaną automatycznie przeniesione do poza obrotu.

- Reguły przychodzące translatora adresów Sieciowych — translatora adresów Sieciowych zasad określających ruchu przychodzącego ułożony do przodu zakończenie IP i rozdzielana IP wewnętrznej.

![](./media/load-balancer-arm/load-balancer-arm.png)

## <a name="quickstart-templates"></a>Szablony Szybki Start

Azure Menedżera zasobów umożliwia obsługi administracyjnej aplikacji przy użyciu szablonu deklaracyjnych. W jednym szablonie należy wdrożyć wielu usług wraz z ich zależności. Wielokrotnie wdrażanie aplikacji każdej etapie cyklu życia aplikacji za pomocą tego samego szablonu.

Szablony mogą zawierać definicje dla maszyn wirtualnych, wirtualnych sieci, zestawy dostępność, interfejsów sieciowych (NIC), konta miejsca do magazynowania, urządzenia do równoważenia obciążenia, grupy zabezpieczeń sieci i publiczne adresy IP. Dzięki szablonom można utworzyć wszystko, co jest potrzebne do złożonej aplikacji. Plik szablonu można sprawdzić w systemie zarządzania zawartością dla wersji kontroli i współpracy.

[Dowiedz się więcej na temat szablonów](http://go.microsoft.com/fwlink/?LinkId=544798)

[Dowiedz się więcej o zasobów](../virtual-network/resource-groups-networking.md)

Szablony Szybki Start przy użyciu usługi równoważenia obciążenia Azure znajdują się w [repozytorium GitHub](https://github.com/Azure/azure-quickstart-templates) hostingu zestaw szablonów społeczności wygenerowane.

Przykłady szablonów:

- [2 maszyny wirtualne usługi równoważenia obciążenia i zasady równoważenia obciążenia](http://go.microsoft.com/fwlink/?LinkId=544799)
- [2 maszyny wirtualne w VNET z regułami wewnętrznych równoważenia obciążenia i równoważenia obciążenia](http://go.microsoft.com/fwlink/?LinkId=544800)
- [2 maszyny wirtualne w równoważenia obciążenia i skonfigurować reguły translatora adresów Sieciowych na kg](http://go.microsoft.com/fwlink/?LinkId=544801)


## <a name="setting-up-azure-load-balancer-with-a-powershell-or-cli"></a>Konfigurowanie usługi równoważenia obciążenia Azure za pomocą programu PowerShell lub interfejsu wiersza polecenia

Wprowadzenie do poleceń cmdlet, narzędzi wiersza polecenia i interfejsów API pozostałych Azure Menedżera zasobów

- [Polecenia cmdlet Azure sieci](https://msdn.microsoft.com/library/azure/mt163510.aspx) może służyć do tworzenia równoważenia obciążenia.
- [Jak utworzyć równoważenia obciążenia, za pomocą Menedżera zasobów Azure](load-balancer-get-started-ilb-arm-ps.md)
- [Polecenie Azure za pomocą zarządzania zasobami Azure](../xplat-cli-azure-resource-manager.md)
- [Interfejsy API pozostałych Usługa równoważenia obciążenia](https://msdn.microsoft.com/library/azure/mt163651.aspx)


## <a name="next-steps"></a>Następne kroki

Możesz także [rozpocząć tworzenie internetowej przeciwległych równoważenia obciążenia](load-balancer-get-started-internet-arm-ps.md) i konfigurowanie jakiego rodzaju [Tryb rozkładu](load-balancer-distribution-mode.md) zachowanie ruch sieci usługi równoważenia obciążenia określonych.

Dowiedz się, jak zarządzać [Ustawienia limitu czasu bezczynności TCP dla usługi równoważenia obciążenia](load-balancer-tcp-idle-timeout.md). Jest to ważne, gdy aplikacja należy zachować aktywności dla serwerów za równoważenia obciążenia połączeń.
