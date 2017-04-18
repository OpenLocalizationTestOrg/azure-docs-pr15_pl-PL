<properties
    pageTitle="Łączenie programu SQL Server maszyn wirtualnych (Menedżer zasobów) | Microsoft Azure"
    description="Dowiedz się, jak połączyć z programem SQL Server uruchomione na komputerze wirtualnej platformy Azure. W tym temacie używa modelu Klasyczny wdrożenia. Scenariusze różnią się w zależności od konfiguracji sieci i lokalizacji klienta."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"    
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/21/2016"
    ms.author="jroth" />

# <a name="connect-to-a-sql-server-virtual-machine-on-azure-resource-manager"></a>Nawiązywanie połączenia z maszyny wirtualnej serwera SQL Azure (Menedżer zasobów)

> [AZURE.SELECTOR]
- [Menedżer zasobów](virtual-machines-windows-sql-connect.md)
- [Klasyczny](virtual-machines-windows-classic-sql-connect.md)

## <a name="overview"></a>Omówienie

W tym temacie opisano, jak połączyć się z wystąpieniem programu SQL Server uruchomionym na Azure maszyn wirtualnych. Omówiono kilka [scenariuszy łączności ogólne](#connection-scenarios) , a następnie zawiera [szczegółowe instrukcje dotyczące konfigurowania programu SQL Server łączność w maszyn wirtualnych Azure](#steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]model klasyczny wdrożenia. Aby wyświetlić w klasycznej wersji programu w tym artykule, zobacz [Łączenie do programu SQL Server maszyn wirtualnych w klasycznym Azure](virtual-machines-windows-classic-sql-connect.md).

Jeśli wolisz, aby były pełny opisem inicjowania obsługi administracyjnej i łączności, zobacz [inicjowania obsługi administracyjnej maszyny wirtualnej serwera SQL Azure](virtual-machines-windows-portal-sql-server-provision.md).

## <a name="connection-scenarios"></a>Scenariusze łączenia

Tak, gdy klient łączy się z programem SQL Server uruchomione na komputerze wirtualnych różni się w zależności od lokalizacji klienta i Konfiguracja komputera i sieci. Scenariusze te obejmują:

- [Nawiązywanie połączenia z programem SQL Server w Internecie](#connect-to-sql-server-over-the-internet)
- [Nawiązywanie połączenia z programem SQL Server w tej samej sieci wirtualnej](#connect-to-sql-server-in-the-same-virtual-network)

### <a name="connect-to-sql-server-over-the-internet"></a>Nawiązywanie połączenia z programem SQL Server w Internecie

Jeśli chcesz nawiązać połączenie z aparatu bazy danych programu SQL Server z Internetu, istnieje kilka czynności wymagane, takich jak Konfigurowanie zapory, włączanie uwierzytelniania SQL i Konfigurowanie grupy zabezpieczeń sieci musi być reguły grupy zabezpieczeń sieci w celu zezwalanie na ruch TCP na porcie 1433.

Użycie portalu inicjować obsługę obraz maszyn wirtualnych programu SQL Server przy użyciu Menedżera zasobów, te kroki są wykonywane po wybraniu **publicznej** opcji łączności programu SQL:

![Publiczne opcji łączności SQL podczas inicjowania obsługi administracyjnej](./media/virtual-machines-windows-sql-connect/sql-vm-portal-connectivity.png)

Jeśli nie została podczas inicjowania obsługi administracyjnej, następnie można ręcznie skonfigurować program SQL Server i maszyn wirtualnych, wykonując [kroki w tym artykule, aby ręcznie skonfigurować łączności](#steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm).

>[AZURE.NOTE] Obraz maszyn wirtualnych dla programu SQL Server Express edition nie powoduje automatycznego włączenia protokół TCP/IP. Dla wersji Express należy użyć Menedżer konfiguracji programu SQL Server do [ręcznie włączyć protokół TCP/IP](#configure-sql-server-to-listen-on-the-tcp-protocol) , po utworzeniu maszyn wirtualnych.

Po tych czynności dowolnego klienta z dostępem do Internetu można połączyć się wystąpienie programu SQL Server, określając publiczny adres IP maszyny wirtualnej albo etykieta DNS przypisane do tego adresu IP. Port programu SQL Server jest 1433, nie musisz określić je w parametrach połączenia.

    "Server=sqlvmlabel.eastus.cloudapp.azure.com;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

Mimo że temu łączności dla klientów w Internecie, to oznacza, że każdy może połączyć się z programu SQL Server. Poza klienci będą musieli prawidłowe nazwy użytkownika i hasła. Poziom zabezpieczeń można uniknąć znanego portu 1433. Na przykład jeśli skonfigurowano program SQL Server, aby odsłuchać na porcie 1500 i ustanowioną pisane z wielkiej litery zaporę i zasad grupy zabezpieczeń sieci, można połączyć dołączając numer portu do nazwy serwera, tak jak w poniższym przykładzie:

    "Server=sqlvmlabel.eastus.cloudapp.azure.com,1500;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

>[AZURE.NOTE] Należy pamiętać, że jeśli komunikowanie się za pomocą programu SQL Server podlega normalny [ceny na przesyłanie danych ruchu wychodzącego](https://azure.microsoft.com/pricing/details/data-transfers/)wszystkich wychodzących danych z Azure centrum danych za pomocą tej techniki.

### <a name="connect-to-sql-server-in-the-same-virtual-network"></a>Nawiązywanie połączenia z programem SQL Server w tej samej sieci wirtualnej

[Wirtualna sieć](../virtual-network/virtual-networks-overview.md) udostępnia dodatkowe scenariusze. Maszyny wirtualne można połączyć w tej samej sieci wirtualnej, nawet jeśli te maszyny wirtualne znajdują się w różnych grup zasobów. A używając sieci [VPN witryny do witryny](../vpn-gateway/vpn-gateway-site-to-site-create.md), można utworzyć architekturę hybrydowych łączącego maszyny wirtualne z sieci lokalnej i komputerów.

Wirtualnych sieci umożliwia dołączanie maszyny wirtualne usługi Azure do domeny. To jest jedynym sposobem uwierzytelniania systemu Windows do programu SQL Server. Inne scenariusze łączenia wymaga uwierzytelniania SQL przy użyciu nazwy użytkownika i hasła.

Korzystając z portalu inicjować obsługę obraz maszyn wirtualnych programu SQL Server przy użyciu Menedżera zasobów, reguły zapory pisane z wielkiej litery, komunikacji w wirtualnej sieci są ustawienia po wybraniu **prywatne** opcji łączności programu SQL. Jeśli nie została podczas inicjowania obsługi administracyjnej, następnie można ręcznie skonfigurować program SQL Server i maszyn wirtualnych, wykonując [kroki w tym artykule, aby ręcznie skonfigurować łączności](#steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm). Jednak jeśli zamierzasz skonfigurować środowisko domeny i uwierzytelniania systemu Windows, nie musisz skonfigurować uwierzytelnianie SQL i logowania, wykonaj czynności podane w tym artykule. Nie należy skonfigurować reguły grupy zabezpieczeń sieci dostępu przez internet.

>[AZURE.NOTE] Obraz maszyn wirtualnych dla programu SQL Server Express edition nie powoduje automatycznego włączenia protokół TCP/IP. Dla wersji Express należy użyć Menedżer konfiguracji programu SQL Server do [ręcznie włączyć protokół TCP/IP](#configure-sql-server-to-listen-on-the-tcp-protocol) , po utworzeniu maszyn wirtualnych.

Przy założeniu, że skonfigurowano DNS w sieci wirtualnej, można nawiązać wystąpienie programu SQL Server, określając nazwę komputera maszyn wirtualnych programu SQL Server w parametrach połączenia. W poniższym przykładzie założono także, czy uwierzytelniania systemu Windows również został skonfigurowany i że użytkownik udzielono dostępu do wystąpienie programu SQL Server.

    "Server=mysqlvm;Integrated Security=true"

Należy zauważyć, że w tym scenariuszu można również określić adres IP maszyn wirtualnych.

## <a name="steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm"></a>Instrukcje dotyczące ręcznego konfigurowania łączności programu SQL Server w maszyn wirtualnych Azure

Poniższe kroki przedstawiają sposoby ręczne konfigurowanie łączności z programem wystąpienie programu SQL Server, a następnie opcjonalnie Połącz przez internet przy użyciu programu SQL Server Management Studio (SSMS). Zauważ, że wiele z tych kroków są wykonywane po wybraniu odpowiednich opcji łączności programu SQL Server w portalu.

Przed nawiązaniem połączenia z wystąpieniem programu SQL Server z innego maszyn wirtualnych lub w Internecie, zgodnie z opisem w sekcjach muszą wykonać następujące zadania:

- [Otwarte porty TCP w Zaporze systemu Windows](#open-tcp-ports-in-the-windows-firewall-for-the-default-instance-of-the-database-engine)
- [Konfigurowanie programu SQL Server, aby odsłuchać przy użyciu protokołu TCP](#configure-sql-server-to-listen-on-the-tcp-protocol)
- [Konfigurowanie programu SQL Server dla uwierzytelniania w trybie mieszanym](#configure-sql-server-for-mixed-mode-authentication)
- [Tworzenie logowania uwierzytelnianie programu SQL Server](#create-sql-server-authentication-logins)
- [Konfigurowanie reguły przychodzące grupy zabezpieczeń sieci](#configure-a-network-security-group-inbound-rule-for-the-vm)
- [Konfigurowanie etykiety DNS publiczny adres IP](#configure-a-dns-label-for-the-public-ip-address)
- [Nawiązywanie połączenia z aparatu bazy danych z innego komputera](#connect-to-the-database-engine-from-another-computer)

[AZURE.INCLUDE [Connect to SQL Server in a VM](../../includes/virtual-machines-sql-server-connection-steps.md)]

[AZURE.INCLUDE [Connect to SQL Server in a VM Resource Manager](../../includes/virtual-machines-sql-server-connection-steps-resource-manager-nsg-rule.md)]

[AZURE.INCLUDE [Connect to SQL Server in a VM Resource Manager](../../includes/virtual-machines-sql-server-connection-steps-resource-manager.md)]

## <a name="next-steps"></a>Następne kroki

Aby wyświetlić inicjowania obsługi administracyjnej instrukcje wraz z tych czynności łączności, zobacz [inicjowania obsługi administracyjnej maszyny wirtualnej serwera SQL Azure](virtual-machines-windows-portal-sql-server-provision.md).

[Eksploruj ścieżki szkoleniowe](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) dla programu SQL Server w przypadku Azure maszyn wirtualnych.

Aby uzyskać innych tematów związanych z programem SQL Server w maszyny wirtualne Azure zobacz [Programu SQL Server na maszyn wirtualnych Azure](virtual-machines-windows-sql-server-iaas-overview.md).
