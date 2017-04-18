<properties
    pageTitle="Łączenie programu SQL Server maszyn wirtualnych (klasyczny) | Microsoft Azure"
    description="Dowiedz się, jak połączyć z programem SQL Server uruchomione na komputerze wirtualnej platformy Azure. W tym temacie używa modelu Klasyczny wdrożenia. Scenariusze różnią się w zależności od konfiguracji sieci i lokalizacji klienta."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    tags="azure-service-management"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/22/2016"
    ms.author="jroth" />

# <a name="connect-to-a-sql-server-virtual-machine-on-azure-classic-deployment"></a>Nawiązywanie połączenia z maszyny wirtualnej serwera SQL Azure (wdrożenie klasyczny)

> [AZURE.SELECTOR]
- [Menedżer zasobów](virtual-machines-windows-sql-connect.md)
- [Klasyczny](virtual-machines-windows-classic-sql-connect.md)

## <a name="overview"></a>Omówienie

W tym temacie opisano, jak połączyć się z wystąpieniem programu SQL Server uruchomionym na Azure maszyn wirtualnych. Omówiono kilka [scenariuszy łączności ogólne](#connection-scenarios) , a następnie zawiera [szczegółowe instrukcje dotyczące konfigurowania programu SQL Server łączność w maszyn wirtualnych Azure](#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Jeśli korzystasz z maszyny wirtualne Menedżera zasobów, zobacz [Łączenie do programu SQL Server maszyny wirtualnej Azure za pomocą Menedżera zasobów](virtual-machines-windows-sql-connect.md).

## <a name="connection-scenarios"></a>Scenariusze łączenia

Tak, gdy klient łączy się z programem SQL Server uruchomione na komputerze wirtualnych różni się w zależności od lokalizacji klienta i Konfiguracja komputera i sieci. Scenariusze te obejmują:

- [Nawiązywanie połączenia z programem SQL Server w tym samym usługi w chmurze](#connect-to-sql-server-in-the-same-cloud-service)
- [Nawiązywanie połączenia z programem SQL Server w Internecie](#connect-to-sql-server-over-the-internet)
- [Nawiązywanie połączenia z programem SQL Server w tej samej sieci wirtualnej](#connect-to-sql-server-in-the-same-virtual-network)

>[AZURE.NOTE] Przed nawiązaniem połączenia z jednej z następujących metod, należy wykonać [kroki w tym artykule w celu skonfigurowania połączenia](#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).

### <a name="connect-to-sql-server-in-the-same-cloud-service"></a>Nawiązywanie połączenia z programem SQL Server w tym samym usługi w chmurze

Można utworzyć wiele maszyn wirtualnych w tym samym usługi w chmurze. Aby dowiedzieć się, w tym scenariuszu maszyn wirtualnych, zobacz [jak połączyć maszyn wirtualnych z usługą wirtualnej sieci lub w chmurze](virtual-machines-windows-classic-connect-vms.md#connect-vms-in-a-standalone-cloud-service). W tym scenariuszu jest, gdy klient na maszyn wirtualnych próbuje nawiązać połączenia z programem SQL Server uruchomionym na innym komputerze wirtualnych w tym samym usługi w chmurze.

W tym scenariuszu można łączyć przy użyciu maszyn wirtualnych **nazwę** (również wyświetlane jako **Nazwy komputera** lub **Nazwa hosta** w portalu). Jest to nazwa podanych dla maszyn wirtualnych podczas tworzenia. Na przykład jeśli nazwę swojej maszyn wirtualnych SQL **mysqlvm**, klient maszyn wirtualnych w tym samym usługi w chmurze można korzystać następujący ciąg połączenia nawiązania połączenia:

    "Server=mysqlvm;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

### <a name="connect-to-sql-server-over-the-internet"></a>Nawiązywanie połączenia z programem SQL Server w Internecie

Jeśli chcesz nawiązać połączenie z aparatu bazy danych programu SQL Server z Internetu, należy utworzyć punkt końcowy maszyn wirtualnych dla poczty przychodzącej komunikacji TCP. Ten krok konfiguracji Azure kieruje ruch przychodzący port TCP do port TCP, która jest dostępna dla maszyny wirtualnej.

Aby połączyć w Internecie, należy użyć nazwy DNS maszyn wirtualnych i numer portu punktu końcowego maszyn wirtualnych (skonfigurowane w dalszej części tego artykułu). Aby znaleźć nazwę DNS, przejdź do portalu Azure i wybierz **maszyn wirtualnych (klasyczny)**. Zaznacz ten komputer wirtualnych. **Nazwa DNS** jest wyświetlana w sekcji **Omówienie** .

Na przykład można rozważyć klasyczny maszyny wirtualnej o nazwie **mysqlvm** o nazwie DNS **mysqlvm7777.cloudapp.net** oraz punkt końcowy maszyn wirtualnych o **57500**. Przy założeniu poprawnie skonfigurowanego łączności, następujący ciąg połączenia można używać do uzyskiwania dostępu maszyny wirtualnej z dowolnego miejsca w Internecie:

    "Server=mycloudservice.cloudapp.net,57500;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

Mimo że temu łączności dla klientów w Internecie, to oznacza, że każdy może połączyć się z programu SQL Server. Poza klienci będą musieli prawidłowe nazwy użytkownika i hasła. Poziom zabezpieczeń nie używaj znanego portu 1433 dla punktu końcowego publicznej maszyn wirtualnych. I, jeśli to możliwe, Rozważ dodanie ACL na punkt końcowy, aby ograniczyć ruch tylko do klientów zezwolisz. Aby uzyskać instrukcje dotyczące używania ACL z punktów końcowych zobacz [Zarządzanie ACL dla punktu końcowego](virtual-machines-windows-classic-setup-endpoints.md#manage-the-acl-on-an-endpoint).

>[AZURE.NOTE] Należy pamiętać, że jeśli komunikowanie się za pomocą programu SQL Server podlega normalny [ceny na przesyłanie danych ruchu wychodzącego](https://azure.microsoft.com/pricing/details/data-transfers/)wszystkich wychodzących danych z Azure centrum danych za pomocą tej techniki.

### <a name="connect-to-sql-server-in-the-same-virtual-network"></a>Nawiązywanie połączenia z programem SQL Server w tej samej sieci wirtualnej

[Wirtualna sieć](../virtual-network/virtual-networks-overview.md) udostępnia dodatkowe scenariusze. Maszyny wirtualne można połączyć w tej samej sieci wirtualnej, nawet jeśli te maszyny wirtualne zapisane w chmurze różnych usługach. A używając sieci [VPN witryny do witryny](../vpn-gateway/vpn-gateway-site-to-site-create.md), można utworzyć architekturę hybrydowych łączącego maszyny wirtualne z sieci lokalnej i komputerów.

Wirtualnych sieci umożliwia dołączanie maszyny wirtualne usługi Azure do domeny. To jest jedynym sposobem uwierzytelniania systemu Windows do programu SQL Server. Inne scenariusze łączenia wymaga uwierzytelniania SQL przy użyciu nazwy użytkownika i hasła.

Jeśli zamierzasz skonfigurować środowisko domeny i uwierzytelniania systemu Windows, nie musisz skonfigurować publicznej punktu końcowego lub SQL uwierzytelniania i logowania, wykonaj czynności podane w tym artykule. W tym scenariuszu można nawiązać wystąpienie programu SQL Server, określając nazwę maszyn wirtualnych programu SQL Server w parametrach połączenia. W poniższym przykładzie założono, że też skonfigurowano uwierzytelnianie systemu Windows i że użytkownik udzielono dostępu do wystąpienie programu SQL Server.

    "Server=mysqlvm;Integrated Security=true"

## <a name="steps-for-configuring-sql-server-connectivity-in-an-azure-vm"></a>Instrukcje dotyczące konfigurowania łączności programu SQL Server w maszyn wirtualnych Azure

Następująca procedura przedstawia sposób nawiązywania połączenia z wystąpieniem programu SQL Server przez internet przy użyciu programu SQL Server Management Studio (SSMS). Jednak te same kroki dotyczą udostępnianie komputera wirtualnych programu SQL Server dla aplikacji, działa zarówno w lokalnej i w Azure.

Przed nawiązaniem połączenia z wystąpieniem programu SQL Server z innego maszyn wirtualnych lub w Internecie, zgodnie z opisem w sekcjach muszą wykonać następujące zadania:

- [Tworzenie punktu końcowego TCP dla maszyny wirtualnej](#create-a-tcp-endpoint-for-the-virtual-machine)
- [Otwarte porty TCP w Zaporze systemu Windows](#open-tcp-ports-in-the-windows-firewall-for-the-default-instance-of-the-database-engine)
- [Konfigurowanie programu SQL Server, aby odsłuchać przy użyciu protokołu TCP](#configure-sql-server-to-listen-on-the-tcp-protocol)
- [Konfigurowanie programu SQL Server dla uwierzytelniania w trybie mieszanym](#configure-sql-server-for-mixed-mode-authentication)
- [Tworzenie logowania uwierzytelnianie programu SQL Server](#create-sql-server-authentication-logins)
- [Określanie nazwy DNS maszyny wirtualnej](#determine-the-dns-name-of-the-virtual-machine)
- [Nawiązywanie połączenia z aparatu bazy danych z innego komputera](#connect-to-the-database-engine-from-another-computer)

W poniższym diagramie podsumowano ścieżki połączenia:

![Łączenie maszyn wirtualnych programu SQL Server](../../includes/media/virtual-machines-sql-server-connection-steps/SQLServerinVMConnectionMap.png)

[AZURE.INCLUDE [Connect to SQL Server in a VM Classic TCP Endpoint](../../includes/virtual-machines-sql-server-connection-steps-classic-tcp-endpoint.md)]

[AZURE.INCLUDE [Connect to SQL Server in a VM](../../includes/virtual-machines-sql-server-connection-steps.md)]

[AZURE.INCLUDE [Connect to SQL Server in a VM Classic Steps](../../includes/virtual-machines-sql-server-connection-steps-classic.md)]

## <a name="next-steps"></a>Następne kroki

Jeśli planujesz również wysokiej dostępności i odzyskiwanie za pomocą grupy dostępności (AlwaysOn), należy rozważyć implementacji detektor. Bazy danych klienci łączą się odbiornika, a nie bezpośrednio do jednego wystąpienia programu SQL Server. Odbiornik przekierowuje klientów podstawowego replice w grupie dostępności. Aby uzyskać więcej informacji zobacz [Konfigurowanie detektor ILB dla grupy dostępności (AlwaysOn) platformy Azure](virtual-machines-windows-classic-ps-sql-int-listener.md).

Należy przejrzeć wszystkie najważniejsze wskazówki dotyczące zabezpieczeń dla programu SQL Server uruchomionych Azure maszyn wirtualnych. Aby uzyskać więcej informacji zobacz [Zagadnienia dotyczące zabezpieczeń dla programu SQL Server w maszyn wirtualnych Azure](virtual-machines-windows-sql-security.md).

[Eksploruj ścieżki szkoleniowe](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) dla programu SQL Server w przypadku Azure maszyn wirtualnych. 

Aby uzyskać innych tematów związanych z programem SQL Server w maszyny wirtualne Azure zobacz [Programu SQL Server na maszyn wirtualnych Azure](virtual-machines-windows-sql-server-iaas-overview.md).
