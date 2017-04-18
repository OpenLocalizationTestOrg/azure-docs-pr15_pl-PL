<properties
   pageTitle="Co to jest sieć listy kontroli dostępu (ACL)?"
   description="Więcej informacji na temat list ACL"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="what-is-an-endpoint-access-control-list-acls"></a>Co to jest punkt końcowy listy kontroli dostępu (ACL)?

Punkt końcowy listy kontroli dostępu (ACL) jest dostępne dla wdrożenia Azure zawiera rozszerzenie zabezpieczeń. List ACL umożliwia selektywne zezwolić lub odmówić ruch dla punktu końcowego maszyn wirtualnych. Ta funkcja filtrowania pakietów zapewnia dodatkową warstwę zabezpieczeń. Można określić sieć list ACL tylko punkty końcowe. Nie można określić lista ACL wirtualnej sieci lub określonej podsieci zawarte w wirtualnej sieci.

> [AZURE.IMPORTANT] Zaleca się używanie grup zabezpieczeń sieci (NSGs) zamiast ACL, o ile to możliwe. Aby dowiedzieć się więcej na temat NSGs, zobacz [Co to jest grupa zabezpieczeń sieci?](virtual-networks-nsg.md).

ACL można skonfigurować przy użyciu programu PowerShell lub portalu zarządzania. Aby skonfigurować sieć list ACL przy użyciu programu PowerShell, zobacz [Zarządzanie listy kontroli dostępu (ACL) dla punktów końcowych przy użyciu programu PowerShell](virtual-networks-acl-powershell.md). Aby skonfigurować sieć ACL za pomocą portalu zarządzania, zobacz [jak się punkty końcowe maszyn wirtualnych](../virtual-machines/virtual-machines-windows-classic-setup-endpoints.md).

Za pomocą list ACL sieci, możesz wykonać następujące czynności:

- Selektywne zezwolić lub odmówić ruch przychodzący według podsieci zdalnej zakres adresów IP protokołu IPv4 do punktu końcowego wprowadzania maszyn wirtualnych.

- Adresy IP czarnej listy

- Tworzenie wielu reguł na punkt końcowy maszyn wirtualnych

- Określanie 50 reguły ACL na punkt końcowy maszyn wirtualnych

- Używanie reguł kolejnością zapewnienie prawidłowego zestawu reguł są stosowane do punktu końcowego danego maszyn wirtualnych (od najniższej do najwyższej)

- Umożliwia określenie listy ACL dla określonej podsieci zdalny adres IP protokołu IPv4.

## <a name="how-acls-work"></a>Jak działają ACL

ACL jest obiekt, który zawiera listę reguł. Po utworzeniu listy ACL i zastosować go do punktu końcowego maszyn wirtualnych, filtrowanie pakietów odbywa się w węźle hosta usługi maszyn wirtualnych. Oznacza to, że dane z zdalne adresy IP są filtrowane według węzeł hosta ACL reguł dopasowania zamiast na swojej maszyn wirtualnych. Dzięki temu usługi maszyn wirtualnych wydatków cennego cyklów Procesora na filtrowanie pakietów.

Po utworzeniu maszyny wirtualnej domyślne ACL zostaną umieszczone w miejscu, aby zablokować cały ruch przychodzący. Jednak jeśli punkt końcowy jest tworzony dla (port 3389), następnie domyślne ACL jest zmodyfikowane w celu umożliwienia cały ruch przychodzący dla tego punktu końcowego. Ruch przychodzący z dowolnym podsieci zdalnej następnie mają dostęp do tego punktu końcowego i nie obsługi administracyjnej zapory jest wymagany. Inne porty są blokowane dla ruchu przychodzącego, chyba że punkty końcowe są tworzone dla tych portów. Domyślnie jest dozwolone ruchu wychodzącego.

**Przykładowa tabela ACL domyślne**

| **Reguła #** | **Podsieci zdalnej** | **Punkt końcowy** | **Zezwolenie i odmówić** |
|--------|---------------|----------|-------------|
| 100    | 0.0.0.0/0     | 3389     | O zezwolenie      |

## <a name="permit-and-deny"></a>Zezwalać lub odmówić

Selektywne można zezwolić lub odmówić ruch sieciowy punkt końcowy wprowadzania maszyn wirtualnych, tworząc reguły określające "pozwolenie" lub "odmówić". Należy pamiętać, że domyślnie po utworzeniu punktu końcowego cały ruch jest dozwolony do punktu końcowego. Z tego powodu jest ważne dowiedzieć się, jak tworzyć reguły o zezwolenie i odmówić i umieść je kolejności pierwszeństwa, jeśli chcesz, aby szczegółowe sterowanie ruch sieciowy, który chcesz zezwolić dotrzeć do punktu końcowego maszyn wirtualnych.

Kwestie do rozważenia:

1. **Nie list ACL —** Domyślnie po utworzeniu punktu końcowego możemy zezwolić wszystkim dla punktu końcowego.

1. **Zezwolić-** Można dodać jedną lub więcej "pozwolenie" zakresów, domyślnie są odmowy odbierania pozostałe zakresy. Tylko pakiety z zakresu dozwolonych adresów IP będzie można komunikować się z punktem końcowym maszyn wirtualnych.

1. **Odmówić-** Można dodać jedną lub więcej "odmówić" zakresów, można wprowadzać wszystkie zakresy ruchu domyślnie.

1. **Kombinacja zezwalać lub odmówić-** Można użyć kombinacji "pozwolenie" i "odmówić", gdy użytkownik chce dotycząca szczególnych limit określony zakres IP dozwolone lub odmówić.

## <a name="rules-and-rule-precedence"></a>Reguły i pierwszeństwa reguł

ACL sieci można skonfigurować na określonym maszyn wirtualnych punktów końcowych. Na przykład można określić sieć ACL punktu końcowego RDP utworzone na komputerze wirtualnych które blokady w dół dostępu dla określonych adresów IP. W poniższej tabeli przedstawiono sposób udzielanie dostępu do publicznych wirtualne adresy IP (służącą) określonego zakresu pozwalające na dostęp do RDP. Wszystkie inne zdalnego adresy IP są odrzucane. Firma Microsoft wykonaj *najniższą pierwszeństwo* kolejność reguł.

### <a name="multiple-rules"></a>Wiele reguł

W poniższym przykładzie Jeśli chcesz umożliwić dostęp do punktu końcowego RDP tylko z dwóch publicznej IPv4 zakresy adresów (65.0.0.0/8 i 159.0.0.0/8), można uzyskać to, określając dwie reguły *zezwolić* . W tym przypadku od RDP jest tworzona domyślnie dla maszyny wirtualnej, można zablokować dostęp do portu RDP, w oparciu o podsieci zdalnej. W poniższym przykładzie pokazano sposób, aby udzielić dostępu do publicznej wirtualne adresy IP (służącą) określonego zakresu pozwalające na dostęp do RDP. Wszystkie inne zdalnego adresy IP są odrzucane. Dzieje się tak, ponieważ sieć ACL można skonfigurować dla punktu końcowego określonego maszyn wirtualnych i domyślnie jest odmowa dostępu.

**Przykład — wiele reguł**

| **Reguła #** | **Podsieci zdalnej** | **Punkt końcowy** | **Zezwolenie i odmówić** |
|--------|---------------|----------|-------------|
| 100    | 65.0.0.0/8    | 3389     | O zezwolenie      |
| 200    | 159.0.0.0/8   | 3389     | O zezwolenie      |

### <a name="rule-order"></a>Kolejność reguł

Ponieważ wiele reguł można określić dla punktu końcowego, musi być sposobem organizowania reguły w celu określenia, które reguła ma pierwszeństwo. Kolejność reguł określa priorytet. ACL sieci wykonaj *najniższą pierwszeństwo* kolejność reguł. W poniższym przykładzie punkt końcowy na porcie 80 selektywne jest udzielony dostęp tylko określonych zakresów adresów IP. Aby skonfigurować tak, mamy reguły Odmów (reguły \# 100) dla adresów w polu 175.1.0.1/24. Następnie określono drugą regułę o pierwszeństwie 200, która umożliwia dostęp do wszystkich adresów w obszarze 175.0.0.0/8.

**Przykład — pierwszeństwem reguł**

| **Reguła #** | **Podsieci zdalnej** | **Punkt końcowy** | **Zezwolenie i odmówić** |
|--------|---------------|----------|-------------|
| 100    | 175.1.0.1/24  | 80       | Odmawianie        |
| 200    | 175.0.0.0/8   | 80       | O zezwolenie      |

## <a name="network-acls-and-load-balanced-sets"></a>Sieci ACL i ładowanie zestawów strategii

Sieć ACL można określić dla punktu końcowego zestawu (kg ustawiono) równoważenia obciążenia. Jeśli określono ACL dla zestawu kg, ACL sieci jest stosowana do wszystkich maszyn wirtualnych w tym zestawie kg. Na przykład jeśli zestaw kg jest tworzone "Port 80" i ustawianie kg zawiera 3 maszyny wirtualne, ACL sieci utworzony na punkt końcowy "Port 80" jednej maszyn wirtualnych zostaną automatycznie zastosowane do innych maszyny wirtualne.

![Sieci ACL i ładowanie zestawów strategii](./media/virtual-networks-acl/IC674733.png)

## <a name="next-steps"></a>Następne kroki

[Jak zarządzać listy kontroli dostępu (ACL) dla punktów końcowych przy użyciu programu PowerShell](virtual-networks-acl-powershell.md)
