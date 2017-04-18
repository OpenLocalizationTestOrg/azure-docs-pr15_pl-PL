<properties
    pageTitle="Omówienie protokołu IPv6 dla równoważenia obciążenia Azure | Microsoft Azure"
    description="Opis obsługę protokołu IPv6 usługi równoważenia obciążenia Azure i równoważenia obciążenia maszyny wirtualne."
    services="load-balancer"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
    keywords="Protokół IPv6, równoważenia obciążenia azure, podwójne stos, publiczny adres ip, natywny protokół ipv6, mobile, iot"
/>
<tags
    ms.service="load-balancer"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/14/2016"
    ms.author="sewhee"
/>

# <a name="overview-of-ipv6-for-azure-load-balancer"></a>Omówienie protokołu IPv6 dla równoważenia obciążenia Azure

Urządzenia do równoważenia obciążenia z Internetu mogą być rozmieszczone z adresem IP protokołu IPv6. Oprócz połączenia IPv4 to udostępnia następujące możliwości:

* Natywnych zakończenia do końca łączności IPv6 między publicznej klientów internetowych i Azure wirtualnych maszyn za pośrednictwem usługi równoważenia obciążenia.
* Natywne zakończenia do końca ruchu wychodzącego łączności IPv6 między maszyny wirtualne i publicznej klientów korzystających z protokołu Internet IPv6.

Poniższej ilustracji przedstawiono funkcje protokołu IPv6 dla równoważenia obciążenia Azure.

![Równoważenia obciążenia Azure, z protokołu IPv6](./media/load-balancer-ipv6-overview/load-balancer-ipv6.png)

Po wdrożeniu klienta IP protokołu IPv4 lub Internet włączony protokół IPv6 można komunikować się z publicznych adresów IP protokołu IPv4 lub IPv6 (lub nazwy hostów) z równoważenia obciążenia Azure z Internetu. Równoważenia obciążenia kieruje pakiety IPv6 prywatne adresy IP protokołu IPv6 maszyny wirtualne, używania translatora adresów sieciowych (NAT). Klient Internetu IPv6 nie możesz komunikować się bezpośrednio z adresem IP protokołu IPv6 maszyny wirtualne.

## <a name="features"></a>Funkcje

Wbudowana obsługa protokołu IPv6 maszyny wirtualne wdrożony za pomocą Menedżera zasobów Azure udostępnia:

1. Równoważenia obciążenia usług protokołu IPv6 dla klientów protokołu IPv6 w Internecie
2. Punkty końcowe natywny protokół IPv6 i IPv4 maszyny wirtualne ("dwa skumulowany")
3. Ruch przychodzący i wychodzący zainicjowane natywnych połączenia protokołu IPv6
4. Włączanie protokołów takich jak HTTP (S), port TCP i UDP szeroką gamę architekturach obsługi komputera

## <a name="benefits"></a>Zalety

Ta funkcja umożliwia następujące kluczowe korzyści:

* Spełniają wymaganie, że nowe aplikacje są dostępne dla klientów tylko IPv6 przepisów dla instytucji rządowych
* Włącz mobile i Internet deweloperów czynności (IOT) za pomocą adresu rosnących mobile i rynkach IOT skumulowany narożnej maszyn wirtualnych Azure (IPv4 + IPv6)

## <a name="details-and-limitations"></a>Szczegóły i ograniczenia

Szczegóły

* Usługa Azure DNS zawiera zarówno A IP protokołu IPv4, jak i protokół IPv6 AAAA rekordów i odpowiada oba rekordy dla usługi równoważenia obciążenia. Klient wybiera adresów (IPv4 lub IPv6) do komunikowania się z.
* W przypadku maszyn wirtualnych inicjuje połączenie z publiczną urządzenia podłączonego do Internetu IPv6, maszyn źródłowy adres IPv6 jest sieć adresem przekształcić (NAT) publiczny adres IP protokołu IPv6 usługi równoważenia obciążenia.
* Maszyny wirtualne systemem Linux musi być skonfigurowany do uzyskiwania adresu IP protokołu IPv6 za pośrednictwem DHCP. Wiele obrazów Linux w galerii Azure są już skonfigurowane do obsługi protokołu IPv6 bez zmian. Aby uzyskać więcej informacji zobacz [Konfigurowanie DHCPv6 dla maszyny wirtualne Linux](load-balancer-ipv6-for-linux.md)
* Jeśli wybierzesz sondy kondycji za pomocą usługi równoważenia obciążenia, tworzenie sondy IPv4 i używać go z punkty końcowe IPv4 i IPv6. Jeśli usługa na swojej maszyn wirtualnych ulegnie uszkodzeniu, IPv4 i IPv6 punkty końcowe są pobierane poza obrotu.

Ograniczenia

* Nie można dodać reguł równoważenia obciążenia protokołu IPv6 w portalu Azure. Reguły można tworzyć tylko za pomocą szablonu, polecenie, programu PowerShell.
* Nie może uaktualnić istniejących maszyny wirtualne umożliwia adresy IP protokołu IPv6. Należy wdrożyć nowe maszyny wirtualne.
* Jeden adres IPv6 można przypisywać do karty sieciowej jednej w każdym maszyn wirtualnych.
* Adresy IP protokołu IPv6 publiczne nie można przypisać do maszyny. Mogą być tylko przypisywane do równoważenia obciążenia.
* Nie można skonfigurować odwrotne wyszukiwanie DNS dla swojej publicznej adresy IP protokołu IPv6.
* Maszyny wirtualne adresy IP protokołu IPv6 nie mogą być członkami usługi w chmurze Azure. Ta osoba może być połączony z wirtualną sieć Azure (VNet) i komunikować się ze sobą przez ich adresy IP protokołu IPv4.
* Adresy IP protokołu IPv6 prywatne mogą być rozmieszczone na poszczególnych maszyny wirtualne w grupie zasobów, ale nie można go wdrożyć w grupie zasobów przez zestawami Skala.
* Azure maszyny wirtualne nie można połączyć przez IPv6 do innych maszyny wirtualne, innych usług Azure lub urządzenia lokalnego. Ich można komunikować się tylko z usługi równoważenia obciążenia Azure przez IPv6. Jednak ich można komunikować się z tych innych zasobów za pomocą protokołu IPv4.
* Ochrony grupy zabezpieczeń sieci (NSG) dla protokołu IPv4 jest obsługiwany we wdrożeniach stos podwójnego (IPv4 + IPv6). NSGs nie mają zastosowania do punktów końcowych protokołu IPv6.
* Punkt końcowy protokołu IPv6 na maszyn wirtualnych nie są wyświetlane bezpośrednio do Internetu. Jest za usługi równoważenia obciążenia. Tylko porty określone w regułach równoważenia obciążenia są dostępne przez IPv6.
* Zmienianie parametrów IdleTimeout dla protokołu IPv6 jest **nie są obecnie obsługiwane**. Wartość domyślna to czterech minut.

## <a name="next-steps"></a>Następne kroki

Dowiedz się, jak wdrożyć usługę równoważenia obciążenia z protokołu IPv6.

* [Dostępność protokołu IPv6 według regionów](https://go.microsoft.com/fwlink/?linkid=828357)
* [Wdrażanie usługi równoważenia obciążenia przy użyciu protokołu IPv6 przy użyciu szablonu](load-balancer-ipv6-internet-template.md)
* [Wdrażanie usługi równoważenia obciążenia przy użyciu protokołu IPv6 przy użyciu programu PowerShell Azure](load-balancer-ipv6-internet-ps.md)
* [Wdrażanie usługi równoważenia obciążenia przy użyciu protokołu IPv6 przy użyciu interfejsu wiersza polecenia Azure](load-balancer-ipv6-internet-cli.md)
