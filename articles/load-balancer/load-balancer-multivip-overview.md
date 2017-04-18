<properties
   pageTitle="Usługa równoważenia obciążenia wielu służącą do Azure | Microsoft Azure"
   description="Omówienie wielu służącą na równoważenia obciążenia Azure"
   services="load-balancer"
   documentationCenter="na"
   authors="chkuhtz"
   manager="narayan"
   editor=""
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/11/2016"
   ms.author="chkuhtz"
/>

# <a name="multiple-vips-for-azure-load-balancer"></a>Usługa równoważenia obciążenia wielu służącą do Azure

Usługi równoważenia obciążenia Azure umożliwia ładowanie usługi Saldo na wielu porty i/lub wiele adresów IP. Definicje równoważenia obciążenia publicznych i wewnętrznych umożliwia obciążenie przepływów saldo zestawu maszyny wirtualne.

W tym artykule opisano podstawy tej możliwości, ważnych pojęć i ograniczeń. Jeśli zamierzasz użyciem usług na jeden adres IP, możesz znaleźć że uproszczone instrukcje dotyczące [publicznej](load-balancer-get-started-internet-portal.md) lub [wewnętrznego](load-balancer-get-started-ilb-arm-portal.md) ładowanie konfiguracji równoważenia. Dodawanie wielu służącą ma charakter przyrostowy pojedynczy konfiguracji VIP. W tym artykule przy użyciu pojęcia, możesz rozwinąć uproszczoną konfigurację w dowolnym momencie.

Podczas definiowania równoważenia obciążenia Azure serwera sieci Web i konfiguracji wewnętrznej bazy danych są połączone z regułami. Sonda kondycji odwołuje się reguła służy do określania nowych przepływów są wysyłane do węzła w puli wewnętrznej bazy danych. Frontend jest zdefiniowane przez wirtualnych IP (VIP), czyli 3-krotki obejmuje adres IP (publiczna lub wewnętrzny), protokół transportowy (UDP lub TCP) i numer portu. DIP jest adres IP na Azure Sieciowej wirtualnych dołączone do maszyn wirtualnych w puli wewnętrznej bazy danych.

Poniższa tabela zawiera niektóre przykładowe konfiguracje serwera sieci Web:

| VIP | Adres IP | Protocol (protokół) | Port |
|-----|------------|----------|------|
|1|65.52.0.1|PORT TCP|80|
|2|65.52.0.1|PORT TCP|_8080_|
|3|65.52.0.1|_UDP_|80|
|4|_65.52.0.2_|PORT TCP|80|

W tabeli przedstawiono cztery różne frontends. Frontends #1, #2 i #3 się pojedynczego VIP z wieloma regułami. Jest używany ten sam adres IP, ale port lub protokół jest inny dla każdego serwera sieci Web. Frontends nr 1 i #4 są przykładem wielu służącą, w którym ponownie tego samego protokołu serwera sieci Web oraz portu u wielu służącą.

Azure usługi równoważenia obciążenia zapewnia elastyczność podczas definiowania reguły równoważenia obciążenia. Reguły oświadcza, jak adres i port serwera sieci Web są mapowane docelowy adres oraz portu do wewnętrznej bazy danych. Czy ponownie wykorzystywać porty wewnętrznej bazy danych przez reguły zależy od typu reguły. Każdy typ reguły ma określonych wymagań, które mogą mieć wpływ na projekt konfiguracji i sondy hosta. Istnieją dwa typy reguł:

1. Reguły domyślnej z nie ponownego użycia portu wewnętrznej bazy danych
2. Reguła przestawny IP miejsce, w którym są używane ponownie porty wewnętrznej bazy danych

Usługi równoważenia obciążenia Azure umożliwia mieszać oba typy reguł na tej samej konfiguracji usługi równoważenia obciążenia. Usługi równoważenia obciążenia można używać ich jednocześnie dla danego maszyn wirtualnych lub dowolną kombinację, jak przestrzegać ograniczeń reguły. Typ reguły, który można wybrać, zależy od wymagania aplikacji i złożoności obsługi tej konfiguracji. Należy ocenić, jakie reguły są dostosowane do rozwiązania.

Firma Microsoft Eksplorowanie scenariuszom dodatkowo, zaczynając od zachowanie domyślne.

## <a name="rule-type-1-no-backend-port-reuse"></a>Reguły typu #1: nie ponownego użycia portu wewnętrznej bazy danych

![Ilustracja MultiVIP](./media/load-balancer-multivip-overview/load-balancer-multivip.png)

W tym scenariuszu służącą do serwera sieci Web są konfigurowane w następujący sposób:

| VIP | Adres IP | Protocol (protokół) | Port |
|-----|------------|----------|------|
|![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1|65.52.0.1|PORT TCP|80|
|![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2|*65.52.0.2*|PORT TCP|80|

DIP jest miejscem docelowym ruchu przychodzącego przepływu. W puli wewnętrznej bazy danych każdego maszyn wirtualnych udostępnia żądanej usługi na porcie unikatowe na DIP. Ta usługa jest skojarzony z serwera sieci Web za pośrednictwem Definicja reguły.

Zdefiniowano dwie reguły:

| Reguły | Mapowanie serwera sieci Web | Do puli wewnętrznej bazy danych |
|------|--------------|-----------------|
| 1 | ![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) VIP1:80 | ![wewnętrznej bazy danych](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) DIP1:80, ![wewnętrznej bazy danych](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) DIP2:80 |
| 2 | ![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) VIP2:80 | ![wewnętrznej bazy danych](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) DIP1:81, ![wewnętrznej bazy danych](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) DIP2:81 |

Mapowanie wykonane w równoważenia obciążenia Azure jest teraz w następujący sposób:

| Reguły | Adres VIP IP | Protocol (protokół) | Port | Miejsce docelowe | Port |
|------|----------------|----------|------|-----|------|
|![reguły](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1|65.52.0.1|PORT TCP|80|Adres IP DIP|80|
|![reguły](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2|65.52.0.2|PORT TCP|80|Adres IP DIP|81|

Każda reguła musi mieć przepływu z unikatową kombinacją docelowy adres IP i port docelowy. Przez zmianę z numerem portu docelowego przepływu, wiele reguł zapewnia przepływów samej DIP na różne porty.

Kondycja sondy zawsze są kierowane do DIP maszyny. Należy się upewnić, możesz że do sondy odzwierciedla kondycji maszyn wirtualnych.

## <a name="rule-type-2-backend-port-reuse-by-using-floating-ip"></a>Reguły typu #2: ponowne użycie portu wewnętrznej bazy danych przy użyciu przestawny IP

Azure równoważenia obciążenia zapewnia elastyczność ponowne port serwera sieci Web u wielu służącą niezależnie od użytej reguły. Ponadto kilka scenariuszy aplikacji wolisz lub wymagają tego samego portu, który będzie używany przez wiele wystąpień aplikacji na jednym maszyn wirtualnych w puli wewnętrznej bazy danych. Typowych przykładów ponownego użycia portu zawiera klaster wysokiej dostępności, sieci wirtualnej urządzenia i Uwidacznianie wielu TLS punkty końcowe bez ponownego szyfrowania.

Jeśli chcesz ponownie użyć portu wewnętrznej bazy danych przez wiele reguł, należy włączyć przestawny IP w definicji reguły.

Przestawny IP jest częścią określane jako bezpośredni serwera zwracana (DSR). DSR składa się z dwóch części: topologii przepływu i adresów IP schemat mapowania. Na poziomie platformy Azure równoważenia obciążenia zawsze działa w topologii przepływu DSR niezależnie od tego, czy jest włączony przestawny IP, czy nie. Oznacza to, że część ruchu wychodzącego przepływu jest zawsze poprawnie ulegną przepływ bezpośrednio powrót do początku.

Z domyślny typ reguły Azure udostępnia równoważenia schemat mapowania adresów IP w celu ułatwienia używania tradycyjnych obciążenia. Włączanie przestawny IP zmienia schemat mapowania adresów IP, aby możliwa była zmiana dodatkowe zgodnie z poniższym opisem.

Na poniższym diagramie przedstawiono tej konfiguracji:

![Ilustracja MultiVIP](./media/load-balancer-multivip-overview/load-balancer-multivip-dsr.png)

W tym scenariuszu dla każdej maszyn wirtualnych w puli wewnętrznej bazy danych występują trzy interfejsy:

* DIP: wirtualna NIC skojarzone z maszyn wirtualnych (zasób NIC Azure firmy)
* VIP1: interfejsu sprzężenia zwrotnego w gościa, za pomocą systemu operacyjnego, który skonfigurowano z adresem IP VIP1
* VIP2: interfejsu sprzężenia zwrotnego w gościa, za pomocą systemu operacyjnego, który skonfigurowano z adresem IP VIP2

>[AZURE.IMPORTANT] Konfiguracja interfejsy logiczne odbywa się w obrębie gościa OS. Ta konfiguracja jest wykonywane lub nie zarządza Azure. Bez tej konfiguracji reguł nie będzie działać. Definicje sondy kondycji za pomocą DIP maszyn wirtualnych zamiast VIP logiczne. Dlatego usługi należy podać sondy odpowiedzi na porcie DIP odzwierciedlająca stan usług oferowanych na VIP logiczne.

Załóżmy tej samej konfiguracji serwera sieci Web, jak w poprzednim scenariuszu:

| VIP | Adres IP | Protocol (protokół) | Port |
|-----|------------|----------|------|
|![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1|65.52.0.1|PORT TCP|80|
|![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2|*65.52.0.2*|PORT TCP|80|

Zdefiniowano dwie reguły:

| Reguły | Mapowanie serwera sieci Web | Do puli wewnętrznej bazy danych |
|------|--------------|-----------------|
| 1 | ![reguły](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) VIP1:80 | ![wewnętrznej bazy danych](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) VIP1:80 (w VM1 i VM2) |
| 2 | ![reguły](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) VIP2:80 | ![wewnętrznej bazy danych](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) VIP2:80 (w VM1 i VM2) |

W poniższej tabeli przedstawiono pełną mapowania w równoważenia obciążenia:

| Reguły | Adres VIP IP | Protocol (protokół) | Port | Miejsce docelowe | Port |
|------|----------------|----------|------|-------------|------|
|![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1|65.52.0.1|PORT TCP|80|identyczne VIP (65.52.0.1)|identyczne VIP (80)|
|![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2|65.52.0.2|PORT TCP|80|identyczne VIP (65.52.0.2)|identyczne VIP (80)|

Miejsce docelowe ruchu przychodzącego przepływu jest adres VIP interfejsu sprzężenia zwrotnego w maszyn wirtualnych. Każda reguła musi mieć przepływu z unikatową kombinacją docelowy adres IP i port docelowy. Przez zmianę adresu IP przepływu, ponownego użycia portu jest możliwe w tym samym maszyn wirtualnych. Usługi są wyświetlane do równoważenia obciążenia przez powiązanie jej adres IP i port interfejsu sprzężenia zwrotnego odpowiednich VIP.

Zwróć uwagę, że w tym przykładzie nie zmienia się z numerem portu docelowego. Mimo że to scenariusz przestawny IP, równoważenia obciążenia Azure obsługuje także definiowania reguły ponownego wpisywania z numerem portu docelowego wewnętrznej bazy danych, a także inne niż port docelowy serwera sieci Web.

Typ reguły przestawny IP jest podstawą kilku wzorców Konfiguracja usługi równoważenia obciążenia. Przykładem, która jest obecnie dostępna jest Konfiguracja [SQL (AlwaysOn) z wielu detektorów](../virtual-machines/virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md) . W czasie firma Microsoft będzie dokumentu większej liczby poniższych scenariuszach.

## <a name="limitations"></a>Ograniczenia

* Wiele konfiguracji VIP są obsługiwane tylko z IaaS maszyny wirtualne.
* Z regułą przestawny IP aplikacji należy użyć DIP dla ruchu wychodzącego przepływów. Jeśli aplikacja powiązana z adresem VIP skonfigurowane na interfejsie sprzężenia zwrotnego w gościa systemu operacyjnego, następnie SNAT nie jest dostępny do edycji ruchu wychodzącego przepływu i przepływ kończy się niepowodzeniem.
* Publiczne adresy IP mają wpływ na rozliczenia. Aby uzyskać więcej informacji zobacz [ceny adres IP](https://azure.microsoft.com/pricing/details/ip-addresses/)
* Zastosowanie limity subskrypcji. Aby uzyskać więcej informacji zobacz [limity usługi](../azure-subscription-service-limits.md#networking-limits) , aby uzyskać szczegółowe informacje.
