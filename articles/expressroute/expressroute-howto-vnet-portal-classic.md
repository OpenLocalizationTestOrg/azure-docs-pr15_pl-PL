<properties
   pageTitle="Konfigurowanie wirtualnej sieci i bramy dla ExpressRoute w portalu klasyczny | Microsoft Azure"
   description="W tym artykule przeprowadzi Cię przez proces konfigurowania sieci wirtualnej dla ExpressRoute przy użyciu modelu Klasyczny wdrożenia i portalu klasyczny."
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags 
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="09/20/2016"
   ms.author="cherylmc"/>

# <a name="create-a-virtual-network-for-expressroute-in-the-classic-portal"></a>Tworzenie sieci wirtualnej dla ExpressRoute w portalu klasyczny

Kroki opisane w tym artykule przeprowadzi Cię przez skonfigurowanie wirtualnej sieci i bramy wirtualną sieć do użytku z ExpressRoute przy użyciu modelu Klasyczny wdrożenia i portalu klasyczny.

Jeśli szukasz instrukcji dotyczących model wdrożenia Menedżera zasobów, można użyć następujących artykułów: [Tworzenie wirtualnej sieci przy użyciu programu PowerShell](../virtual-network/virtual-networks-create-vnet-arm-ps.md) i [Dodaj bramy VPN, aby VNet Menedżera zasobów dla ExpressRoute](expressroute-howto-add-gateway-resource-manager.md).

**Informacje dotyczące modeli Azure wdrażania**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="create-a-classic-vnet-and-gateway"></a>Tworzenie klasyczny VNet i bramy

Poniższe kroki tworzenie klasyczny VNet i Brama wirtualnej sieci. Jeśli masz już klasyczny VNet, zobacz sekcję [Konfiguruj istniejących VNet klasyczny](#config) w tym artykule.

1. Zaloguj się do [portalu klasyczny Azure](http://manage.windowsazure.com).

2. W lewym dolnym rogu ekranu kliknij przycisk **Nowy**. W okienku nawigacji kliknij pozycję **Usługi sieciowe**, a następnie kliknij **Wirtualnej sieci**. Kliknij przycisk **Utwórz niestandardowy** , aby uruchomić Kreatora konfiguracji.

3. Na stronie **Szczegóły wirtualna sieć** wprowadź następujące informacje:

    - **Nazwa** — nazwa wirtualnej sieci. Po wdrożeniu usługi wystąpienia maszyny wirtualne i PaaS, więc nie możesz nawiązać nazwę zbyt skomplikowane, będziesz używać tej nazwy wirtualną sieć.
    - **Lokalizacja** — lokalizacji dotyczy bezpośrednio do lokalizacji fizycznej (region), w którym chcesz zasobów pośrednictwem (SMS) do znajdują się. Na przykład jeśli chcesz maszyny wirtualne, które wdrażanie z tą siecią wirtualną się znajdować się w USA wschodniego, zaznacz tej lokalizacji. Nie można zmienić regionu skojarzone z siecią wirtualną po jego utworzeniu.

4. Na stronie **serwery DNS i połączenia VPN** wprowadź następujące informacje, a następnie kliknij strzałkę następnego w prawej dolnej. 

    - **Serwery DNS** - wprowadź nazwę serwera DNS i adres IP lub wybierz serwer DNS wcześniej zarejestrowanych w menu skrótów. To ustawienie nie tworzy serwera DNS. Umożliwia określenie serwerów DNS, które ma być używany do rozpoznawania nazw dla tej wirtualnej sieci.
    - **Łączność witryny do witryny** — zaznacz pole wyboru dla **Konfigurowanie sieci VPN witryny do witryny**.
    - **ExpressRoute** — zaznacz pole wyboru **Użyj ExpressRoute**. Ta opcja jest wyświetlana tylko, jeśli zaznaczona pozycja **Konfiguruj VPN witryny do witryny**.
    - **Sieci lokalnej** — musisz mieć witrynę sieci lokalnej dla ExpressRoute. Jednak w przypadku połączenia ExpressRoute prefiksów adresów określone dla witryny sieci lokalnej będą ignorowane. Zamiast tego prefiksów adresów ogłaszane do firmy Microsoft przez obwód ExpressRoute zostanie użyty do celów routingu.<BR>Jeśli masz już sieci lokalnej utworzone dla połączenia ExpressRoute, można go wybrać z listy rozwijanej. Jeśli nie, zaznacz opcję **Podaj sieci lokalnej**.

5. Na stronie **witryny do witryny łączności** pojawi się w przypadku wybrania Określ sieci lokalnej w poprzednim kroku. Aby skonfigurować sieci lokalnej, wprowadź następujące informacje, a następnie kliknij strzałkę następnego. 

    - **Nazwa** - nazwa, którym chcesz nawiązać połączenie lokalne (lokalnego) sieci witryny.
    - **Przestrzeń adresów** - tym uruchamianie IP i CIDR (liczba adresów). Można określić dowolny zakres adres, dopóki go nie nakładały się z zakresu adresów wirtualnych sieci. Zazwyczaj to określić zakresy adresów sieci lokalnej, ale w przypadku ExpressRoute, te ustawienia nie są używane. Jednak to ustawienie jest wymagane w celu utworzenia sieci lokalnej, korzystając z portalu klasyczny.
    - **Dodaj adres miejsca** — to ustawienie nie dotyczy ExpressRoute.


6. Na stronie **Wirtualna sieć adres spacje** wprowadź następujące informacje, a następnie kliknij znacznik wyboru w prawym dolnym, aby skonfigurować sieć. 

    - **Przestrzeń adresów** - tym uruchamianie liczba IP i adres. Upewnij się, że wybrane obszary adresów nie zachodzi obszary adresów znajdujących się w sieci lokalnej.
    - **Dodaj podsieci** - tym uruchamianie IP i liczba adresów. Dodatkowe podsieci nie są wymagane.
    - **Dodaj bramy podsieci** — kliknij, aby dodać podsieci bramy. Podsieć bramy jest używana tylko do bramy wirtualną sieć i jest wymagany dla tej konfiguracji.<BR>Podsieć bramy CIDR (liczba adresów) dla ExpressRoute musi być /28 lub większa (/ 27, / 26 itp.). Dzięki temu za mało adresów IP w tej podsieci umożliwia konfigurację, którą chcesz pracować. W portalu klasyczny po zaznaczeniu pola wyboru, aby użyć ExpressRoute, portalu określa podsieć bramy z /28.  Nie można dostosowywać CIDR liczba adresów w portalu klasyczny. Podsieci bramy będzie wyświetlany jako **bramy** w portalu klasyczny, mimo że rzeczywistą nazwę podsieci bramy, która jest tworzona jest faktycznie **GatewaySubnet**. Przy użyciu programu PowerShell lub w portalu Azure, można wyświetlać tę nazwę.

7. Kliknij znacznik wyboru u dołu strony i rozpocznie Tworzenie wirtualnych sieci. Po zakończeniu instalacji zostanie wyświetlona **utworzono** wyświetlana w obszarze **stanu** na stronie **sieci** w portalu klasyczny.

## <a name="gw"></a>Tworzenie bramy

1. Na stronie **sieci** kliknij wirtualną sieć, która została właśnie utworzona, a następnie kliknij pozycję **pulpit nawigacyjny** w górnej części strony.

2. U dołu strony **pulpitu nawigacyjnego** kliknij **Tworzenie bramy** i wybierz pozycję **Routing dynamiczny**. Kliknij przycisk **Tak,** aby potwierdzić, że chcesz utworzyć bramę.

3. Po uruchomieniu bramy tworzenia pojawi się zezwolenia wiadomości, które znasz, rozpoczętej bramy. Może potrwać do 45 minut bramy utworzyć.

4. Łącze sieci do obwodu. Postępuj zgodnie z instrukcjami w artykule [jak połączyć VNets do obwodów ExpressRoute](expressroute-howto-linkvnet-classic.md).

## <a name="config"></a>Konfigurowanie istniejących VNet klasyczny dla ExpressRoute

Jeśli masz już klasyczny VNet, można skonfigurować go, aby nawiązać połączenie ExpressRoute w portalu klasyczny. Ustawienia różnią się od sekcji powyżej, dlatego zapoznaj się z tych sekcji zapoznać się z wymagane ustawienia. Jeśli chcesz utworzyć połączenie coexisting ExpressRoute--— witryny, zobacz kroki opisane [w tym artykule](expressroute-howto-coexist-classic.md) . Są one inne niż opisanych w tym artykule.
 
1. Należy utworzyć sieci lokalnej przed zaktualizowaniem pozostałe ustawienia VNet. Aby utworzyć nowy sieci lokalnej, która jest wymagane podczas konfigurowania ExpressRoute za pośrednictwem portalu klasyczny, kliknij pozycję **Nowy** **>** **Usług sieciowych** **>** **Wirtualną sieć** **>** **Dodaj sieci lokalnej**. Postępuj zgodnie z instrukcjami kreatora, aby utworzyć sieci lokalnej.

2. Użyj strony **Konfiguruj** , aby zaktualizować pozostałe ustawienia usługi VNet i kojarzenie VNet do sieci lokalnej.

3. Po skonfigurowaniu ustawień, przejdź do sekcji [Tworzenie bramy](#gw) tego artykułu, aby utworzyć bramę.


## <a name="next-steps"></a>Następne kroki

- Jeśli chcesz dodać maszyn wirtualnych do sieci wirtualnej, zobacz [ścieżki szkoleniowe dla użytkowników maszyn wirtualnych](https://azure.microsoft.com/documentation/learning-paths/virtual-machines/).
- Jeśli chcesz dowiedzieć się więcej na temat ExpressRoute, zobacz [Omówienie ExpressRoute](expressroute-introduction.md).


 
