<properties 
   pageTitle="Konfigurowanie połączenia VPN między dwoma wirtualnych sieci | Microsoft Azure" 
   description="Dowiedz się, jak skonfigurować połączenia VPN i rozpoznawanie nazw domen między dwoma Azure wirtualnych sieci i sposobu konfigurowania replikacji geo HBase." 
   services="hdinsight,virtual-network" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="06/28/2016"
   ms.author="jgao"/>

# <a name="configure-a-vpn-connection-between-two-azure-virtual-networks"></a>Konfigurowanie połączenia VPN między dwoma Azure wirtualnych sieci  

> [AZURE.SELECTOR]
- [Konfigurowanie połączenia VPN](hdinsight-hbase-geo-replication-configure-vnets.md)
- [Konfigurowanie systemu DNS](hdinsight-hbase-geo-replication-configure-dns.md)
- [Konfigurowanie replikacji HBase](hdinsight-hbase-geo-replication.md) 

Łączność witryny do witryny Azure wirtualną sieć używa bramą VPN w celu włączenia kanału bezpiecznego przy użyciu Ipsec i IKE. VNets może być w różnych subskrypcjach i różnych regionów. Możesz nawet połączyć VNet do VNet komunikacji z konfiguracji wielu witryn. Istnieje kilka możliwych powodów VNet do łączności VNet:

- Krzyżowe region geo nadmiarowości i geo obecności 
- Regionalnych zastosowań wielu granica silnych izolacji 
- Krzyżowe subskrypcji, komunikacji między organizacji platformy Azure

Aby uzyskać więcej informacji zobacz [Konfigurowanie VNet VNet połączenia](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md). 

Aby wyświetlić go na klip wideo:

> [AZURE.VIDEO configure-the-vpn-connectivity-between-two-azure-virtual-networks]

Ten samouczek jest częścią [serii] [ hdinsight-hbase-replication] na temat tworzenia replikacji geo HBase. 

- Konfigurowanie połączenia VPN między dwiema sieciami wirtualnych (tego samouczka)
- [Konfigurowanie systemu DNS dla sieci wirtualnych][hdinsight-hbase-geo-replication-dns]
- [Konfigurowanie HBase geo replikacji][hdinsight-hbase-geo-replication]

Na poniższym diagramie przedstawiono dwa wirtualnych sieci utworzone w ramach tego samouczka:

![Diagram sieciowy wirtualnej replikacji HDInsight HBase][img-vnet-diagram]
 

##<a name="prerequisites"></a>Wymagania wstępne
Przed rozpoczęciem tego samouczka, musisz mieć następujące czynności:

- **Azure subskrypcji**. Zobacz [Azure pobrać bezpłatną wersję próbną](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Pracy z programem PowerShell Azure**.

    Przed uruchomieniem skryptów programu PowerShell, upewnij się, że są podłączone do subskrypcji usługi Azure za pomocą następującego polecenia cmdlet:

        Add-AzureAccount

    Jeśli masz wiele subskrypcji Azure, użyj następującego polecenia cmdlet ustawić bieżącej subskrypcji:

        Select-AzureSubscription <AzureSubscriptionName>
        
    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


>[AZURE.NOTE] Usługa Azure nazw i nazw maszyn wirtualnych musi być unikatowa. Nazwa używana w tym samouczku jest Contoso-[nazwa Azure usługi/m]-[Unii Europejskiej-US]. Na przykład Contoso-VNet-Unii Europejskiej jest Azure wirtualną sieć w Europie północnej centrum danych; Contoso DNS USA jest serwera DNS maszyn wirtualnych w USA wschodniego centrum danych. Muszą pochodzić z własnych nazw.
 

##<a name="create-two-azure-vnets"></a>Tworzenie dwóch VNets Azure



**Aby utworzyć wirtualną sieć o nazwie Contoso-VNet-Unii Europejskiej w Europie Ameryka Północna**

1.  Zaloguj się do [portalu klasyczny Azure][azure-portal].
2.  Kliknij pozycję **Nowy**, **usług SIECIOWYCH**, **WIRTUALNEJ sieci**, **Utwórz niestandardowe**.
3.  Wprowadź:

    - **Nazwa**: Contoso-VNet-Unii Europejskiej
    - **Lokalizacja**: Europa Północna

        Samouczku północna Europa i USA wschodniego centrach danych. Możesz wybrać własną centrach danych.
4.  Wprowadź:

    - **Serwera DNS**: (pozostaw to pole puste) 
    
        Konieczne będzie serwera DNS do rozpoznawania nazw w wirtualnych sieci. Aby uzyskać więcej informacji o tym, kiedy rozpoznawanie nazw dostarczonego przez Azure użycia i kiedy należy używać serwera DNS Zobacz [Rozpoznawanie nazw (DNS)](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md). Aby uzyskać instrukcje dotyczące konfigurowania rozpoznawania nazw między VNets, zobacz [Konfigurowanie usługi DNS między dwoma Azure wirtualnych sieci][hdinsight-hbase-dns].
  
    - **Konfigurowanie sieci VPN punktu do witryny**: (niezaznaczone)

        W tym scenariuszu nie dotyczy punktu do witryny.

    - **Konfigurowanie sieci VPN witryny do witryny**: (niezaznaczone)
    
        Skonfigurujesz połączenie VPN witryny do witryny Azure wirtualną sieć w Stanach Zjednoczonych wschodniego centrum danych.
5.  Wprowadź:

    -   **Adres miejsca na uruchamianie IP**: 10.1.0.0
    -   **Adres miejsca CIDR**: / 16
    -   **Uruchamianie podsieci 1 IP**: 10.1.0.0
    -   **CIDR podsieci 1**: / 24

    Przestrzeń adresów nie można zachodzi z wirtualną sieć USA.  

**Aby utworzyć wirtualną sieć o nazwie Contoso-VNet-Unii Europejskiej w Europie zachód**

- Powtórz tę procedurę ostatni z poniższych wartości:

    - **Nazwa**: Contoso VNet USA
    - **Lokalizacja**: wschodniego Stany Zjednoczone
     
    - **Serwera DNS**: (pozostaw to pole puste)
    - **Konfigurowanie sieci VPN punktu do witryny**: (niezaznaczone)
    - **Konfigurowanie sieci VPN witryny do witryny**: (niezaznaczone)
     
    - **Adres miejsca na uruchamianie IP**: 10.2.0.0
    - **Adres miejsca CIDR**: / 16
    - **Uruchamianie podsieci 1 IP**: 10.2.0.0
    - **CIDR podsieci 1**: / 24

















##<a name="configure-a-vpn-connection-between-the-two-vnets"></a>Konfigurowanie połączenia VPN między dwoma VNets

###<a name="create-local-networks"></a>Tworzenie sieci lokalnych

Po utworzeniu VNet VNet konfiguracji, musisz skonfigurować każdy VNet wzajemnego identyfikowania witryn sieci lokalnej. W tej sekcji będzie skonfigurować każdego VNet jako sieci lokalnej. Sieci lokalnych udostępniać odpowiednich VNet samej obszarach adresów IP.

![Konfigurację sieci VPN Azure witryny do witryny — azure sieci lokalnych][img-vnet-lnet-diagram]


**Aby utworzyć sieci lokalnej o nazwie Contoso-LNet-Unii Europejskiej pasujących przestrzeni adresów sieciowych Contoso-VNet-Unii Europejskiej**

1. Z portalu klasyczny Azure kliknij pozycję **Nowy**, **Usług SIECIOWYCH**, **WIRTUALNĄ sieć**, **Dodawanie sieci lokalnej**.
3. Wprowadź:

    - **Nazwa**: Contoso-LNet-Unii Europejskiej
    - **Adres IP urządzenia VPN**: 192.168.0.1 (ten adres zostaną zaktualizowane później)

        Zazwyczaj należy użyć rzeczywisty adres IP zewnętrznego dla urządzenia sieci VPN. Dla VNet VNet konfiguracji zostanie użyty adres IP bramy sieci VPN. Zakładając, że nie utworzono bram VPN dla dwóch VNets jeszcze, wprowadź adres IP arbitary i wrócić do poprawka.
4.  Wprowadź:

    - **IP uruchamianie miejsca na adres:** 10.1.0.0
    - **CIDR miejsca na adres:** /16
    
    To musi odpowiadać dokładnie zakres, który określonej wcześniej dla firmy Contoso-VNet-Unii Europejskiej.

**Aby utworzyć sieci lokalnej o nazwie Contoso-LNet-US pasujących przestrzeni adresów sieciowych Contoso VNet USA**

- Powtórz tę procedurę ostatni z poniższych parametrów:

    - **Nazwa**: Contoso LNet USA
    - **Adres IP urządzenia VPN**: 192.168.0.1 (ten adres zostaną zaktualizowane nowszy)
     
    - **Adres miejsca na uruchamianie IP**: 10.2.0.0
    - **Adres miejsca CIDR**: / 16


###<a name="create-vpn-gateways"></a>Tworzenie bramy sieci VPN

Istnieją dwie części w tej konfiguracji. Skonfiguruj VNet połączenia sieci lokalnej witryny do witryny, a następnie utworzyć dynamiczne routingu sieci VPN. VNet do VNet wymaga bram Azure VPN z dynamicznego routingu sieci VPN. Azure statycznego routingu sieci VPN nie są obsługiwane.

**Aby skonfigurować połączenie witryny do witryny firmy Contoso-VNet-Unii Europejskiej Contoso LNet USA**

1.  Z portalu klasyczny Azure kliknij przycisk **sieci** w okienku po lewej stronie
2.  Kliknij pozycję **Contoso-VNet-Unii Europejskiej**.
3.  Kliknij kartę **CONFIGUE** .
4.  Sprawdź **Nawiązywanie połączenia z sieci lokalnej**.
5.  W **Sieci lokalnej**wybierz pozycję **Contoso LNet USA**.
6.  Kliknij przycisk **Dodaj bramy podsieci** w sekcji wirtualną sieć adres spacje.
7.  Kliknij przycisk **ZAPISZ**.
8.  Kliknij **przycisk OK** , aby potwierdzić.


**Aby utworzyć bramę VPN dla firmy Contoso-VNet-Unii Europejskiej**

1.  W klasycznym Portal Azure kliknij kartę **pulpitu NAWIGACYJNEGO** .
4.  Kliknij przycisk **Utwórz bramy** w dolnej części strony, a następnie kliknij **Routing dynamiczny**.
5.  Kliknij przycisk **Tak,** aby potwierdzić. Zwróć uwagę, grafika bramy na stronie zmieni się na żółty i opisem Tworzenie bramy. Zazwyczaj trwa około 15 minut bramy utworzyć.

    Gdy stan bramy zmieni się łączenie, adres IP dla każdej bramy będą widoczne na pulpicie nawigacyjnym. Zanotuj adres IP, który odpowiada do każdego VNet, aby go nie mieszać je w górę. Są używane podczas edytowania adresów IP symbol zastępczy dla urządzenia VPN w sieci lokalnych adresów IP.

6.  Tworzenie kopii **Adres IP bramy**. Użyje go do konfigurowania adresu IP bramy sieci VPN firmy Contoso VNet Unii w następnej sekcji.

**Aby utworzyć bramę sieci VPN dla firmy Contoso-VNet-Unii Europejskiej**

- Powtórz tę procedurę dwie ostatnie, aby skonfigurować łączności witryny do witryny firmy Contoso VNet USA Contoso-LNet-Unii Europejskiej i Tworzenie bramy sieci VPN do firmy Contoso Vnet USA. Po wykonaniu tych czynności, konieczne będzie adres IP Brama VPN dla firmy Contoso VNet USA.


### <a name="set-the-vpn-device-ip-addresses-for-local-networks"></a>Ustawianie urządzenia VPN adresów IP dla sieci lokalnych
W ostatniej sekcji możesz utworzyć bramę VPN dla każdego z VNets. Masz adresów IP bram sieci VPN. Teraz możesz przejść wstecz skonfigurować adresy IP sieci VPN urządzenia.

**Aby skonfigurować adres IP urządzenia VPN firmy Contoso-LNet-Unii Europejskiej** 

1.  Z portalu klasyczny Azure kliknij **sieci** w okienku po lewej stronie.
2.  Kliknij pozycję **Sieci lokalnych** z góry.
3.  Kliknij **Contoso-LNet-Unii Europejskiej**, a następnie kliknij przycisk **Edytuj** na dole.
4.  Zaktualizuj **adres IP urządzenia VPN**.  Jest to adres, który zostanie wyświetlony na karcie Pulpit NAWIGACYJNY Contoso-VNET-Unii Europejskiej.
5.  Kliknij przycisk prawy.
6.  Kliknij przycisk Sprawdź.

**Aby skonfigurować adres IP urządzenia VPN firmy Contoso LNet USA** 

- Powtarzanie ostatniej czynności, aby skonfigurować adres IP urządzenia VPN firmy Contoso LNet USA.

###<a name="set-vnet-gateway-keys"></a>Ustawianie kluczy bramy VNet

Przy użyciu udostępnionego klucza bramy Vnet uwierzytelniania połączeń między wirtualnych sieci. Nie można skonfigurować klucz z portalu klasyczny Azure. Należy użyć programu PowerShell lub .NET SDK.

**Aby ustawić klucze**

1. W miejscu pracy Otwórz **Windows PowerShell ISE** lub konsoli programu Windows PowerShell.
2. Aktualizowanie parametrów w ten skrypt obserwuj, a następnie uruchom go:

        Add-AuzreAccount
        Select-AzureSubscription -[AzureSubscriptionName]
        Set-AzureVNetGatewayKey -VNetName ContosoVNet-EU -LocalNetworkSiteName Contoso-LNet-US -SharedKey A1b2C3D4
        Set-AzureVNetGatewayKey -VNetName ContosoVNet-US -LocalNetworkSiteName Contoso-LNet-EU -SharedKey A1b2C3D4 


##<a name="check-the-vpn-connection"></a>Sprawdź połączenie VPN 

Bez wszelkie maszyny wirtualne wdrożony VNets umożliwia wizualne widoku diagram sieciowy wirtualnej strony pulpitu nawigacyjnego VNet w portalu klasyczny Azure Sprawdź stan połączenia:

![Usługa HDInsight HBase replikacji wirtualną sieć stan połączenia VPN][img-vpn-status]
  



##<a name="next-steps"></a>Następne kroki

W tym samouczku zapoznaniu skonfigurować połączenie VPN między dwoma Azure wirtualnych sieci. Obejmuje dwa artykuły w tej serii:

- [Konfigurowanie usługi DNS między dwoma Azure wirtualnych sieci][hdinsight-hbase-geo-replication-dns]
- [Konfigurowanie HBase geo replikacji][hdinsight-hbase-geo-replication]



[hdinsight-hbase-geo-replication-dns]: hdinsight-hbase-geo-replication-configure-dns.md
[hdinsight-hbase-geo-replication]: hdinsight-hbase-geo-replication.md


[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-portal]: https://portal.azure.com

[powershell-install]: ../install-configure-powershell.md



[hdinsight-hbase-replication]: hdinsight-hbase-geo-replication.md
[hdinsight-hbase-dns]: hdinsight-hbase-geo-replication-configure-dns.md


[img-vnet-diagram]: ./media/hdinsight-hbase-geo-replication-configure-vnets/hdinsight-hbase-vpn-diagram.png
[img-vnet-lnet-diagram]: ./media/hdinsight-hbase-geo-replication-configure-vnets/hdinsight-hbase-vpn-lnet-diagram.png
[img-vpn-status]: ./media/hdinsight-hbase-geo-replication-configure-vnets/hdinsight-hbase-vpn-status.png 
