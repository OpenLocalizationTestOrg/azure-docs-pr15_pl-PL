<properties 
   pageTitle="Konfigurowanie usługi DNS między dwoma Azure wirtualnych sieci | Microsoft Azure" 
   description="Dowiedz się, jak skonfigurować połączenia VPN i rozpoznawanie nazw domen między dwoma wirtualnych sieci i sposobu konfigurowania replikacji geo HBase." 
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

# <a name="configure-dns-between-two-azure-virtual-networks"></a>Konfigurowanie usługi DNS między dwoma Azure wirtualnych sieci

> [AZURE.SELECTOR]
- [Konfigurowanie połączenia VPN](../hdinsight-hbase-geo-replication-configure-vnets.md)
- [Konfigurowanie systemu DNS](hdinsight-hbase-geo-replication-configure-dns.md)
- [Konfigurowanie replikacji HBase](hdinsight-hbase-geo-replication.md) 


Dowiedz się, jak dodać i skonfigurować serwery DNS, aby Azure wirtualnych sieci powinien obsługiwać rozpoznawanie nazw w ramach i między wirtualnych sieci.

Ten samouczek jest drugą częścią [serii] [ hdinsight-hbase-geo-replication] na temat tworzenia replikacji geo HBase:

- [Konfigurowanie połączenia VPN między dwoma wirtualnych sieci][hdinsight-hbase-geo-replication-vnet]
- Konfigurowanie systemu DNS dla sieci wirtualnych (tego samouczka)
- [Konfigurowanie HBase geo replikacji][hdinsight-hbase-geo-replication]


Na poniższym diagramie przedstawiono dwa wirtualnych sieci utworzone w [artykule Konfigurowanie połączenia VPN między dwoma wirtualnych sieci][hdinsight-hbase-geo-replication-vnet]:

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

- **Dwa Azure wirtualnej sieci przy użyciu połączenia VPN**.  Aby uzyskać instrukcje, zobacz [Konfigurowanie połączenie VPN między dwoma Azure wirtualnych sieci][hdinsight-hbase-geo-replication-vnet].

>[AZURE.NOTE] Usługa Azure nazw i nazw maszyn wirtualnych musi być unikatowa. Nazwa używana w tym samouczku jest Contoso-[nazwa Azure usługi/m]-[Unii Europejskiej-US]. Na przykład Contoso-VNet-Unii Europejskiej jest Azure wirtualną sieć w Europie północnej centrum danych; Contoso DNS USA jest serwera DNS maszyn wirtualnych w centrum danych wschodniego USA. Muszą pochodzić z własnych nazw.
 
 
##<a name="create-azure-virtual-machines-to-be-used-as-dns-servers"></a>Tworzenie Azure maszyn wirtualnych może być używany jako serwery DNS

**Aby utworzyć maszyny wirtualnej w Contoso-VNet-Unii Europejskiej, o nazwie Contoso-DNS-Unii Europejskiej**

1.  Kliknij pozycję **Nowy**, **obliczenia**, **maszyn wirtualnych**, **Z GALERII**.
2.  Wybierz pozycję **Centrum Windows Server 2012 R2 danych**.
3.  Wprowadź:
    - **Nazwa maszyn wirtualnych**: Contoso-DNS-Unii Europejskiej
    - **Nową nazwę użytkownika**: 
    - **Nowe hasło**: 
4.  Wprowadź:
    - **Usługa w CHMURZE**: Tworzenie nowej usługi w chmurze
    - **REGION/grupy KOLIGACJI/wirtualnych sieci**: (Wybierz Contoso-VNet-Unii Europejskiej)
    - **Wirtualna PODSIECI**: podsieci 1
    - **Konto miejsca do magazynowania**: Korzystanie z konta automatycznie wygenerowanego miejsca do magazynowania
    
        Nazwa usługi cloud będzie taka sama jak nazwa maszyn wirtualnych. W tym przypadku jest to Contoso-DNS-Unii Europejskiej. Dla kolejnych maszyn wirtualnych można wybrać używać samej usługi w chmurze.  Maszyn wirtualnych w obszarze samej usługi w chmurze udostępnianie tej samej sieci wirtualnej lub sufiksu domeny.

        Konto magazynu służy do przechowywania maszyn wirtualnych pliku obrazu. 
    - **Punkty końcowe**: (Przewiń w dół i wybierz pozycję **DNS**) 

Po utworzeniu maszyny wirtualnej, Dowiedz się, wewnętrznego, adresów IP i zewnętrznych adresów IP.

1.  Kliknij nazwę maszyn wirtualnych **Contoso-DNS-Unii Europejskiej**.
2.  Kliknij pozycję **pulpit nawigacyjny**.
3.  Zanotuj:
    - PUBLICZNE WIRTUALNY ADRES IP
    - ADRES IP WEWNĘTRZNEGO


**Aby utworzyć maszyny wirtualnej w Contoso-VNet-US o nazwie Contoso DNS USA** 

- Powtórz tę samą procedurę, aby utworzyć maszyny wirtualnej z następujących wartości:
    - Nazwa maszyn wirtualnych: Contoso-DNS-pl
    - REGION/grupy KOLIGACJI/WIRTUALNEJ sieci: wybierz pozycję Contoso VNET USA
    - WIRTUALNA PODSIECI: Podsieci-1
    - Konto miejsca do magazynowania: Korzystanie z konta automatycznie wygenerowanego miejsca do magazynowania
    - Punkty końcowe: (Wybierz DNS)

##<a name="set-static-ip-addresses-for-the-two-virtual-machines"></a>Konfigurowanie adresów IP dla dwóch maszyn wirtualnych

Serwery DNS wymaga adresów IP.  Nie można wykonać ten krok z portalu klasyczny Azure. Należy użyć programu PowerShell Azure.

**Aby skonfigurować statyczny adres IP dla dwóch maszyn wirtualnych**

1. Otwórz program Windows PowerShell ISE.
2. Uruchom następujące polecenia cmdlet.  

        Add-AzureAccount
        Select-AzureSubscription [YourAzureSubscriptionName]
        
        Get-AzureVM -ServiceName Contoso-DNS-EU -Name Contoso-DNS-EU | Set-AzureStaticVNetIP -IPAddress 10.1.0.4 | Update-AzureVM
        Get-AzureVM -ServiceName Contoso-DNS-US -Name Contoso-DNS-US | Set-AzureStaticVNetIP -IPAddress 10.2.0.4 | Update-AzureVM 

    Nazwausługi jest nazwą usługi cloud. Ponieważ serwer DNS jest to pierwsza maszyna wirtualna Usługa w chmurze, nazwa usługi cloud jest taka sama jak nazwa maszyn wirtualnych.

    Użytkownik może być konieczne zaktualizowanie NazwaUsługi i nazwę pasują do nazw, które masz.


##<a name="add-the-dns-server-role-the-two-virtual-machines"></a>Dodawanie maszyn wirtualnych roli dwóch serwera DNS

**Aby dodać rola serwera DNS na potrzeby firmy Contoso-DNS-Unii Europejskiej**

1.  Z portalu klasyczny Azure kliknij **maszyn wirtualnych** po lewej stronie. 
2.  Kliknij pozycję **Contoso-DNS-Unii Europejskiej**.
3.  Kliknij pozycję **Pulpit NAWIGACYJNY** od góry.
4.  Kliknij pozycję **POŁĄCZ** z dołu i postępuj zgodnie z instrukcjami, aby połączyć się z komputerem wirtualnych za pośrednictwem RDP.
2.  W ramach tej sesji RDP kliknij przycisk systemu Windows w lewym dolnym rogu, aby otworzyć ekran startowy.
3.  Kliknij Kafelek **Menedżera serwera** .
4.  Kliknij przycisk **Dodaj role i funkcje**.
5.  Kliknij przycisk **Dalej**
6.  Wybrana **Instalacja oparta na rolach lub funkcji**, a następnie kliknij przycisk **Dalej**.
7.  Zaznacz ten komputer wirtualnych DNS (go są wyróżnione już), a następnie kliknij przycisk **Dalej**.
8.  Sprawdzanie **serwera DNS**.
9.  Kliknij pozycję **Dodaj funkcje**, a następnie kliknij przycisk **Kontynuuj**.
10. Kliknij trzy razy przycisk **Dalej** , a następnie kliknij przycisk **Zainstaluj**. 

**Aby dodać rola serwera DNS na potrzeby firmy Contoso DNS USA**

- Powtórz kroki, aby dodać rolę DNS **Contoso-DNS -**US.

##<a name="assign-dns-servers-to-the-virtual-networks"></a>Przypisywanie serwerów DNS do wirtualnych sieci

**Aby zarejestrować dwa serwery DNS**

1.  Z portalu klasyczny Azure kliknij pozycję **Nowy**, **Usług SIECIOWYCH**, **WIRTUALNĄ sieć**, **ZAREJESTROWAĆ serwera DNS**.
2.  Wprowadź:
    - **Nazwa**: Contoso-DNS-Unii Europejskiej
    - **Adres IP serwera DNS**: 10.1.0.4 — adres IP musi pasujących maszyn wirtualnych adresu IP serwera DNS.
     
3.  Powtórz proces, aby zarejestrować Contoso DNS USA następujące ustawienia:
    - **Nazwa**: Contoso DNS USA
    - **Adres IP serwera DNS**: 10.2.0.4

**Aby przypisać dwa serwery DNS do dwóch wirtualnych sieci**

1.  Kliknij przycisk **sieci** w okienku po lewej stronie w portalu klasyczny.
2.  Kliknij pozycję **Contoso-VNet-Unii Europejskiej**.
3.  Kliknij przycisk **Konfiguruj**.
4.  Zaznacz **Contoso-DNS-Unii Europejskiej** w sekcji **serwerów dns** .
5.  Kliknij przycisk **ZAPISZ** u dołu strony, a następnie kliknij przycisk **Tak,** aby potwierdzić.
6.  Powtórz proces, aby przypisać serwera DNS **Contoso DNS USA** wirtualną sieć **Contoso VNet USA** .

Aby zaktualizować konfiguracji serwera DNS, należy ponownie uruchomić wszystkie maszyn wirtualnych wdrożonych wirtualnych sieci.

**Aby ponownie uruchomić maszyn wirtualnych**

1. Z portalu klasyczny Azure kliknij **maszyn wirtualnych** po lewej stronie.
2. Kliknij pozycję **Contoso-DNS-Unii Europejskiej**.
3. Kliknij pozycję **pulpit nawigacyjny** od góry.
4. Kliknij przycisk **Uruchom** u dołu.
5. Powtórz te same kroki, aby ponownie uruchomić **Contoso DNS USA**.


##<a name="configure-dns-conditional-forwarders"></a>Konfigurowanie usługi DNS warunkowego przesyłania dalej

Serwer DNS w każdej wirtualnej sieci można tylko rozpoznawania nazw DNS w tym wirtualnej sieci. Musisz skonfigurować usługi warunkowego przesyłania dalej, aby wskazywały serwera DNS równorzędnych nazwę rozwiązania w wirtualnej sieci równorzędnej.

Aby skonfigurować usługę warunkowego przesyłania dalej, należy wiedzieć sufiksów domeny dwa serwery DNS. Sufiksy DNS mogą się różnić w zależności od konfiguracji usług w chmurze, użytą podczas tworzenia maszyn wirtualnych. Dla każdego sufiksu DNS używane w wirtualnej sieci możesz dodać usługi warunkowego przesyłania dalej. 

**Aby znaleźć sufiksów domeny dwa serwery DNS**

1. RDP do **Firmy Contoso-DNS-Unii Europejskiej**.
2. Otwieranie konsoli programu Windows PowerShell lub wiersz polecenia.
3. Uruchom **ipconfig**i zanotuj **sufiks DNS specyficzne dla połączenia**.
4. Nie zamykaj sesja RDP, nadal będzie potrzebne. 
5. Powtórz te same kroki, aby dowiedzieć się, **sufiks DNS konkretnego połączenia** **Contoso DNS USA**.


**Aby skonfigurować przesyłanie dalej DNS**
 
1.  Sesja RDP **Contoso-DNS-Unii Europejskiej**kliknij klawisz systemu Windows w lewym dolnym.
2.  Kliknij pozycję **Narzędzia administracyjne**.
3.  Kliknij pozycję **DNS**.
4.  W okienku po lewej stronie rozwiń **powiadomienia o stanie dostarczenia**, **Contoso-DNS-Unii Europejskiej**.
5.  Wprowadź następujące informacje:
    - **Domeny DNS**: wprowadź sufiks DNS USA DNS Contoso. Na przykład: Contoso-DNS-US.b5.internal.cloudapp.net.
    - **Adresy IP serwerów głównych**: wprowadź 10.2.0.4, czyli adres IP Contoso-DNS-USA firmy.
6.  Naciśnij klawisz **ENTER**, a następnie kliknij **przycisk OK**.  Teraz można rozpoznać adresu IP Contoso-DNS-USA osoby z firmy Contoso-DNS-Unii Europejskiej.
7.  Powtórz kroki, aby dodać przesyłania z usługą DNS na komputerze wirtualnych Contoso DNS USA z następujących wartości:
    - **Domeny DNS**: wprowadź sufiks DNS Unii DNS Contoso. 
    - **Adresy IP serwerów głównych**: wprowadź 10.2.0.4, czyli Contoso-DNS-Unii Europejskiej na adres IP.

##<a name="test-the-name-resolution-across-the-virtual-networks"></a>Testowanie rozpoznawania nazw w sieciach wirtualnych

Teraz można przetestować rozpoznawanie nazwy hosta sieci wirtualnej. Polecenie ping jest domyślnie zablokowana przez zaporę.  Aby rozwiązać maszyn wirtualnych serwera DNS (należy użyć nazwy FQDN) w sieciach równorzędnych przy użyciu narzędzia nslookup.  


##<a name="next-steps"></a>Następne kroki

W tym samouczku zapoznaniu skonfigurować rozpoznawanie nazw w sieci wirtualnych połączeń VPN. Obejmuje dwa artykuły w tej serii:

- [Konfigurowanie połączenia VPN między dwoma Azure wirtualnych sieci][hdinsight-hbase-geo-replication-vnet]
- [Konfigurowanie HBase geo replikacji][hdinsight-hbase-geo-replication]



[hdinsight-hbase-geo-replication]: hdinsight-hbase-geo-replication.md
[hdinsight-hbase-geo-replication-vnet]: hdinsight-hbase-geo-replication-configure-vnets.md
[powershell-install]: powershell-install-configure.md

[img-vnet-diagram]: ./media/hdinsight-hbase-geo-replication-configure-DNS/HDInsight.HBase.VPN.diagram.png 
