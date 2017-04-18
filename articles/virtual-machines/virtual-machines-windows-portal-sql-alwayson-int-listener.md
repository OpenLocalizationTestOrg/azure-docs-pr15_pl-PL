<properties
   pageTitle="Tworzenie odbiornika dla grupy availabilty (AlwaysOn) dla programu SQL Server w maszyn wirtualnych Azure"
   description="Instrukcje krok po kroku dotyczące tworzenia detektor dla grupy availabilty (AlwaysOn) dla programu SQL Server w maszyn wirtualnych Azure"
   services="virtual-machines"
   documentationCenter="na"
   authors="MikeRayMSFT"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="virtual-machines"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows-sql-server"
   ms.workload="infrastructure-services"
   ms.date="07/12/2016"
   ms.author="MikeRayMSFT"/>

# <a name="configure-an-internal-load-balancer-for-an-alwayson-availability-group-in-azure"></a>Konfigurowanie usługi równoważenia obciążenia wewnętrznych dla grupy dostępności (AlwaysOn) platformy Azure

W tym temacie wyjaśniono, jak utworzyć równoważenia obciążenia wewnętrznych dla grupy dostępności (AlwaysOn) programu SQL Server w Azure maszyn wirtualnych z systemem w modelu Menedżera zasobów. Grupy dostępności (AlwaysOn) wymaga usługi równoważenia obciążenia w przypadku wystąpienia programu SQL Server w przypadku Azure maszyn wirtualnych. Usługi równoważenia obciążenia przechowuje adres IP odbiornika grupy dostępności. Jeżeli grupy dostępności obejmuje powiązanie regionów, każdego regionu musi równoważenia obciążenia.

Aby wykonać to zadanie, należy mieć wdrożony w przypadku Azure maszyn wirtualnych w modelu Menedżer zasobów grupy dostępności (AlwaysOn) programu SQL Server. Oba maszyn wirtualnych programu SQL Server musi należeć do tego samego zestawu dostępności. [Szablon programu Microsoft](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) umożliwia automatyczne tworzenie grupy dostępności (AlwaysOn) w Menedżerze Azure zasobów. Ten szablon automatycznie utworzy równoważenia obciążenia wewnętrzny. 

Jeśli wolisz, możesz [ręcznie skonfigurować grupy dostępności (AlwaysOn)](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

W tym temacie wymaga grup dostępności są już skonfigurowane.  

Tematy pokrewne obejmują:

 - [Konfigurowanie grupy dostępności (AlwaysOn) w Azure maszyn wirtualnych (Graficznym)](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)   
 
 - [Konfigurowanie połączenia VNet do VNet za pomocą Menedżera zasobów Azure i programu PowerShell](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

## <a name="steps"></a>Czynności

Przez Instruktaż ten dokument będzie tworzenie i konfigurowanie usługi równoważenia obciążenia w portalu Azure. Po zakończeniu którego, skonfiguruj klaster, aby użyć adresu IP z usługi równoważenia obciążenia odbiornika grupy dostępności (AlwaysOn).

## <a name="create-and-configure-the-load-balancer-in-the-azure-portal"></a>Tworzenie i konfigurowanie usługi równoważenia obciążenia w portalu Azure

W tej części zadania będą wykonaj następujące czynności w portalu Azure:

1. Tworzenie usługi równoważenia obciążenia i skonfigurować adres IP

1. Konfigurowanie puli wewnętrznej bazy danych

1. Tworzenie sondy 

1. Ustawianie zasad równoważenia obciążenia

>[AZURE.NOTE] W przypadku serwerów SQL w różnych grup zasobów i regionów, wykonasz wszystkie kroki dwa razy, raz w każdej grupie zasobów.

## <a name="1-create-the-load-balancer-and-configure-the-ip-address"></a>1. Tworzenie równoważenia obciążenia i skonfigurować adres IP

Pierwszym krokiem jest utworzenie równoważenia obciążenia. Otwórz Azure portal, grupa zasobów, która zawiera maszyn wirtualnych programu SQL Server. W grupie zasobów kliknij przycisk **Dodaj**.

- Wyszukaj **Usługa równoważenia obciążenia**. W wynikach wyszukiwania zaznacz **Równoważenia obciążenia**, która jest opublikowany przez **firmę Microsoft**.

- Na karta **Równoważenia obciążenia** kliknij przycisk **Utwórz**.

- Na **Tworzenie Usługa równoważenia obciążenia**, konfigurowanie usługi równoważenia obciążenia w następujący sposób:

| Ustawienie | Wartość |
| ----- | ----- |
| **Nazwa** | Nazwa tekst reprezentujący równoważenia obciążenia. Na przykład **sqlLB**. |
| **Schematu** | **Wewnętrzna** |
| **Wirtualna sieć** | Wybierz pozycję serwerów SQL znajdujących się w sieci wirtualnej.   |
| **Podsieć**  | Wybierz pozycję podsieci, która w są serwery SQL. |
| **Subskrypcji** | Jeśli masz wiele subskrypcji, może być wyświetlany w tym polu. Wybierz subskrypcję, do której mają być skojarzone z tego zasobu. Zazwyczaj jest tym samym subskrypcji jako wszystkie zasoby w grupie dostępności.  |
| **Grupa zasobów** | Wybierz pozycję serwerów SQL znajdujących się w grupie zasobów. | 
| **Lokalizacja** | Wybierz pozycję Azure serwerów SQL znajdują się w lokalizacji. |

- Kliknij przycisk **Utwórz**. 

Azure tworzy równoważenia obciążenia, który skonfigurowano powyżej. Usługi równoważenia obciążenia należy do określonej sieci, podsieci, grupa zasobów i lokalizacji. Po zakończeniu Azure Sprawdź ustawienia równoważenia obciążenia platformy Azure. 

Teraz skonfiguruj adres IP usługi równoważenia obciążenia.  

- Wybierz polecenie karta **Ustawienia** równoważenia obciążenia, **adres IP**. Karta **adres IP** wykazuje, że jest równoważenia obciążenia prywatne w tej samej sieci wirtualnej jako serwery SQL. 

- Ustaw następujące ustawienia: 

| Ustawienie | Wartość |
| ----- | ----- |
| **Podsieć** | Wybierz pozycję podsieci, która w są serwery SQL. |
| **Przydział** | **Statyczne** |
| **Adres IP** | Wpisz adres IP wirtualnego nieużywane z podsieci.  |

- Zapisz ustawienia.

Teraz równoważenia obciążenia ma adres IP. Rekord ten adres IP. Ten adres IP ma być używany podczas tworzenia detektora w klastrze. Za pomocą skryptu programu PowerShell w dalszej części tego artykułu, użyj tego adresu dla `$ILBIP` zmiennej.



## <a name="2-configure-the-backend-pool"></a>2. Skonfiguruj puli wewnętrznej bazy danych

Następnym krokiem jest utworzenie puli adresów wewnętrznej bazy danych. Azure połączeń do wewnętrznej bazy danych puli adresów *puli wewnętrznej bazy danych*. W tym przypadku pula wewnętrznej bazy danych jest adresy dwa serwery SQL w grupie dostępności. 

- W grupie zasobów kliknij równoważenia obciążenia, który został utworzony. 

- Wybierz polecenie **Ustawienia** **pul wewnętrznej bazy danych**.

- Na **Pule adresów wewnętrznej bazy danych**kliknij przycisk **Dodaj** , aby utworzyć pulę adresów wewnętrznej bazy danych. 

- Na **Dodawanie puli wewnętrznej bazy danych** w polu **Nazwa**wpisz nazwę puli wewnętrznej bazy danych.

- W obszarze **maszyn wirtualnych** kliknij przycisk **+ Dodaj maszyny wirtualnej**. 

- W obszarze **maszyn wirtualnych wybierz** kliknij pozycję **Wybierz zestaw dostępność** i określ zestawu dostępności, że należy maszyn wirtualnych programu SQL Server.

- Po wybraniu opcji zestaw dostępności, kliknij przycisk **Wybierz maszyn wirtualnych**. Kliknij dwa maszyn wirtualnych obsługujące wystąpienia programu SQL Server w grupie dostępności. Kliknij przycisk **Wybierz**. 

- Kliknij **przycisk OK** , aby zamknąć karty dla **maszyn wirtualnych wybierz**i **Dodaj puli wewnętrznej bazy danych**. 

Azure aktualizacji ustawień puli adresów wewnętrznej bazy danych. Ustawianie swojej dostępności zawiera teraz puli dwa serwery SQL.

## <a name="3-create-a-probe"></a>3. sonda tworzenie

Następnym krokiem jest utworzenie sondy. Sonda Określa, jak Azure sprawdzisz, które serwerów SQL jest aktualnie właścicielem odbiornika grupy dostępności. Azure będzie Sonda usługi na podstawie adresu IP na porcie podany podczas tworzenia sondy.

- Karta **Ustawienia** równoważenia obciążenia wybierz polecenie **sondy**. 

- Na **sondy** karta kliknij przycisk **Dodaj**.

- Skonfiguruj sondy na karta **sondy Dodaj** . Konfigurowanie sondy przy użyciu następujących wartości:

| Ustawienie | Wartość |
| ----- | ----- |
| **Nazwa** | Nazwa tekstu reprezentujący sondy. Na przykład **SQLAlwaysOnEndPointProbe**. |
| **Protocol (protokół)** | **PORT TCP** |
| **Port** | Możesz użyć dowolnego dostępny port. Na przykład *59999*.    |
| **Interwał**  | *5* | 
| **Nieprawidłowe progowa.**  | *2* | 

- Kliknij **przycisk OK**. 

>[AZURE.NOTE] Upewnij się, że port zadanej jest otwarty na zaporze zarówno serwera SQL. Oba serwery wymagają regułę ruchu przychodzącego port TCP, którego używasz. Aby uzyskać więcej informacji, zobacz [Dodawanie lub edytowanie reguły zapory](http://technet.microsoft.com/library/cc753558.aspx) . 

Azure tworzy sondy. Azure przy użyciu sondy testowanie które programu SQL Server ma detektor dla grupy dostępności.

## <a name="4-set-the-load-balancing-rules"></a>4. Ustaw zasady równoważenia obciążenia

Ustawianie zasad równoważenia obciążenia. Zasady równoważenia obciążenia skonfigurować sposób równoważenia obciążenia kieruje ruch do serwerów SQL. Dla tej usługi równoważenia obciążenia będzie włączyć bezpośredni zwrotnego serwera, ponieważ tylko jeden z dwóch serwerów SQL kiedykolwiek właścicielem dostępność zasobu odbiornika grupy w danej chwili.

- Karta **Ustawienia** równoważenia obciążenia wybierz polecenie **reguły równoważenia obciążenia**. 

- Na karta **równoważenia obciążenia reguł** kliknij przycisk **Dodaj**.

- Konfigurowanie reguły równoważenia obciążenia przy użyciu karta **Dodaj załadować równoważenia reguł** . Użyj następujących ustawień: 

| Ustawienie | Wartość |
| ----- | ----- |
| **Nazwa** | Nazwa tekst reprezentujący reguły równoważenia obciążenia. Na przykład **SQLAlwaysOnEndPointListener**. |
| **Protocol (protokół)** | **PORT TCP** |
| **Port** | *1433*   |
| **Port wewnętrznej bazy danych** | *1433*. należy zauważyć, że to będzie wyłączone, ponieważ ta reguła używa **Przestawny IP (zwrotu bezpośredni server)**.   |
| **Sonda** | Użyj nazwy sondy utworzone dla tej usługi równoważenia obciążenia. |
| **Utrzymywanie sesji**  | **Brak** | 
| **Limit czasu bezczynności (w minutach)**  | *4* | 
| **Przestawny IP (zwrotu bezpośredni server)**  | **Włączone** | 

 >[AZURE.NOTE] Może być konieczne przewinięcie strony w dół na karta, aby wyświetlić wszystkie ustawienia.

- Kliknij **przycisk OK**. 

- Azure konfiguruje reguły równoważenia obciążenia. Teraz równoważenia obciążenia jest skonfigurowany do rozsyłania ruchu do programu SQL Server, obsługującego detektor dla grupy dostępności. 

W tym momencie grupa zasobów zawiera równoważenia obciążenia, nawiązywanie połączenia z obu komputerach programu SQL Server. Usługi równoważenia obciążenia zawiera również adres IP odbiornika grupy dostępności (AlwaysOn) programu SQL Server, aby maszynowo może odpowiadać na żądania dla grup dostępności.

>[AZURE.NOTE] Jeśli serwery SQL znajdują się w dwóch oddzielnych regionów, powtórz kroki w innym regionie. Każdego regionu wymaga usługi równoważenia obciążenia. 

## <a name="configure-the-cluster-to-use-the-load-balancer-ip-address"></a>Skonfiguruj klaster na używanie adresu IP usługi równoważenia obciążenia 

Następnym krokiem jest Konfigurowanie odbiornika w klastrze, a następnie przełącz odbiornika w trybie online. Aby to zrobić, wykonaj następujące czynności: 

1. Tworzenie odbiornika grupy dostępności w klastrze pracy awaryjnej 

1. Przesuń odbiornika w trybie online

## <a name="1-create-the-availablity-group-listener-on-the-failover-cluster"></a>1. Tworzenie odbiornika grupy o dostępności w klastrze pracy awaryjnej

W tym kroku możesz ręcznie utworzyć odbiornika grupy dostępność w Menedżerze klaster pracy awaryjnej i SQL Server Management Studio (SSMS).

- Nawiązywanie połączenia z Azure maszyny wirtualnej, która obsługuje podstawowego replice za pomocą RDP. 

- Otwórz Menedżera klaster pracy awaryjnej.

- Wybierz węzeł **sieci** i zanotuj nazwę sieci klaster. Ta nazwa będzie używana w `$ClusterNetworkName` zmiennych w skrypt programu PowerShell.

- Rozwinięcie nazwy klaster, a następnie kliknij przycisk **role**.

- W okienku **role** kliknij prawym przyciskiem myszy nazwę grupy dostępności, a następnie wybierz pozycję **Dodaj zasobów** > **Punkt dostępu klienta**.

- W polu **Nazwa** Tworzenie nazwy dla tej nowej odbiornika, a następnie kliknij przycisk **Dalej** dwa razy, a następnie kliknij **Zakończ**. Nie powodują odbiornika lub zasób do trybu online na tym etapie.

 >[AZURE.NOTE] Nazwa dla nowego odbiornika jest nazwę sieciową, której aplikacji będzie korzystać z bazami danych w grupie dostępność programu SQL Server.

- Kliknij kartę **zasoby** , a następnie rozwiń węzeł właśnie utworzony punkt dostępu klienta. Kliknij prawym przyciskiem myszy zasób IP, a następnie kliknij polecenie Właściwości. Zanotuj nazwę z adresem IP. Będzie używać tej nazwy w `$IPResourceName` zmiennych w skrypt programu PowerShell.

- W polu **Adres IP** kliknij **Statyczny adres IP** i ustaw statycznego adresu IP do tego samego adresu, który został użyty podczas skonfigurować adres IP usługi równoważenia obciążenia na Azure portal. Włącz NetBIOS dla tego adresu, a następnie kliknij **przycisk OK**. Powtórz ten krok dla każdego zasobu adresów IP, jeśli rozwiązanie obejmuje wiele VNets Azure. 

- Na węzła obsługującego podstawowego replice Otwórz podwyższonym poziomem środowiska PowerShell ISE i wklej następujące polecenia do nowego skryptu.

        $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP Address resource name
        $ILBIP = “<X.X.X.X>” # the IP Address of the Internal Load Balancer (ILB). This is the static IP address for the load balancer you configured in the Azure portal.
    
        Import-Module FailoverClusters
    
        Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}

- Zaktualizuj zmienne i uruchomić skrypt programu PowerShell, aby skonfigurować adres IP i port dla nowych odbiornika.

 >[AZURE.NOTE] Jeśli serwery SQL znajdują się w oddzielnym regionów, należy uruchomić skrypt programu PowerShell dwa razy. Po raz pierwszy za pomocą nazwy sieci klaster, nazwa zasobu Klaster IP i ładowanie adres IP równoważenia z pierwszej grupy zasobów. Po raz drugi Użyj nazwy sieci klaster, nazwa zasobu Klaster IP i załadować adresu IP równoważenia z drugim grupa zasobów.

Klaster zawiera teraz zasób odbiornika Dostępność grupy.

## <a name="2-bring-the-listener-online"></a>2. Przesuń odbiornika w trybie online

Z dostępność zasobu odbiornika grupy skonfigurowane możesz zabrać ze sobą odbiornika online, aby aplikacje można nawiązać połączenie bazy danych w grupie dostępności z odbiornika.

- Przejdź wstecz do Menedżer klastrów pracy awaryjnej. Rozwiń węzeł **role** , a następnie wyróżnij grupy dostępności. Na karcie **zasoby** kliknij prawym przyciskiem myszy nazwę odbiornika, a następnie kliknij polecenie **Właściwości**.

- Kliknij kartę **zależności** . W przypadku wielu zasobów na liście, sprawdź, czy adresy IP są lub nie oraz, zależności. Kliknij **przycisk OK**.

- Kliknij prawym przyciskiem myszy nazwę odbiornika, a następnie kliknij przycisk **Przejdź do trybu Online**.


- Gdy odbiornika online, na karcie **zasoby** , kliknij prawym przyciskiem myszy grupy dostępności, a następnie kliknij polecenie **Właściwości**.

- Tworzenie współzależności dla zasobu Nazwa odbiornika (nie IP adres zasoby Nazwa). Kliknij **przycisk OK**.


- Uruchamianie programu SQL Server Management Studio i łączenie replice podstawowego.


- Przejdź do **dostępności (AlwaysOn) wysoki** | **grupy dostępności** | **detektory grupy dostępności**. 


- Powinien zostać wyświetlony nazwę odbiornika utworzoną w Menedżerze klaster pracy awaryjnej. Kliknij prawym przyciskiem myszy nazwę odbiornika, a następnie kliknij polecenie **Właściwości**.


- W polu **Port** Określ numer portu odbiornika grupy dostępności za pomocą $EndpointPort użyto wcześniej (1433 był domyślnym), kliknij **przycisk OK**.

Teraz masz grupy dostępności (AlwaysOn) programu SQL Server Azure maszyn wirtualnych działa w trybie Menedżera zasobów. 

## <a name="test-the-connection-to-the-listener"></a>Testowanie połączenia odbiornika

Aby przetestować połączenie:

1. RDP z programem SQL Server, która znajduje się w tej samej sieci wirtualnej, ale nie ma replice. Może to być inne programu SQL Server w klastrze.

1. Aby przetestować połączenie za pomocą narzędzia **sqlcmd** . Na przykład poniższy skrypt nawiąże połączenie **sqlcmd** podstawowego replice za pośrednictwem detektora uwierzytelniania systemu Windows:

        sqlcmd -S <listenerName> -E

Połączenie SQLCMD nawiązać automatycznie podstawowego replice znajduje się niezależnie od tego wystąpienia programu SQL Server. 

## <a name="guidelines-and-limitations"></a>Wskazówki i ograniczenia

Należy zauważyć, że usługa równoważenia obciążenia następujące wskazówki dotyczące dostępności grupy odbiornika platformy Azure za pomocą wewnętrznych:

- Tylko jeden detektor grupy dostępności wewnętrzny jest obsługiwana na usługi w chmurze, ponieważ odbiornik jest skonfigurowany do równoważenia obciążenia, a istnieje tylko jedna równoważenia obciążenia wewnętrzny. Jednak jest możliwość utworzenia multipe detektory zewnętrznych. 

- Przy użyciu usługi równoważenia obciążenia wewnętrznych tylko dostęp odbiornika z w tej samej sieci wirtualnej.
 
