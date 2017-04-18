<properties
    pageTitle="Program SQL Server w przypadku Azure maszyn wirtualnych — często zadawane pytania | Microsoft Azure"
    description="Ten artykuł zawiera odpowiedzi na często zadawane pytania dotyczące uruchomiony program SQL Server na maszyny wirtualne Azure."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="v-shysun"
    manager="felixwu"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/13/2016"
    ms.author="v-shysun"/>

# <a name="sql-server-on-azure-virtual-machines-faq"></a>Program SQL Server w przypadku Azure maszyn wirtualnych — często zadawane pytania

Ten temat zawiera odpowiedzi na niektóre często zadawane pytania dotyczące uruchomiony [Program SQL Server na maszyn wirtualnych Azure](https://azure.microsoft.com/services/virtual-machines/sql-server/).

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="frequently-asked-questions"></a>Często zadawane pytania

1. **Jak utworzyć Azure maszyn wirtualnych z programem SQL Server?**

    Istnieją dwa sposoby wykonaj następujące czynności. Najprostszym rozwiązaniem jest utworzenie maszyny wirtualnej, która zawiera programu SQL Server. Samouczek dotyczący tworzący konto w usłudze Azure i tworzenie maszyny SQL z portalu zobacz [Obsługa administracyjna maszyny wirtualnej programu SQL Server w Azure Portal](virtual-machines-windows-portal-sql-server-provision.md). Masz również możliwość ręcznego instalowania programu SQL Server na maszyny i ponowne używanie licencję lokalnego z [Mobilności licencji poprzez Software Assurance Azure](https://azure.microsoft.com/pricing/license-mobility/).

1. **Jaka jest różnica między maszyny wirtualne SQL a usługą bazy danych SQL?**

    Koncepcji uruchomionych dla programu SQL Server Azure maszyn wirtualnych że różni się od uruchomiony program SQL Server w zdalnego centrum danych. [Baza danych SQL](../sql-database/sql-database-technical-overview.md) oferuje natomiast bazy danych jako usługi. Baza danych SQL nie ma dostępu do komputerów, na których obsługi baz danych. W celu porównania pełny, zobacz [Wybierz chmury opcja programu SQL Server: bazy danych SQL Azure (PaaS) lub SQL Server na Azure maszyny wirtualne (IaaS)](../sql-database/sql-database-paas-vs-sql-server-iaas.md).

1. **Jak migrację lokalnej bazy danych programu SQL Server w chmurze?**

    Najpierw należy utworzyć Azure maszyn wirtualnych z wystąpieniem programu SQL Server. Następnie należy przeprowadzić migrację baz danych lokalnych do tego wystąpienia. Aby uzyskać strategii migracji danych zobacz [Migrowanie bazy danych programu SQL Server do programu SQL Server w maszyn wirtualnych Azure](virtual-machines-windows-migrate-sql.md).

2. **Czy można zmieniać zainstalowane funkcje i zainstalować drugie wystąpienie programu SQL Server na tym samym maszyn wirtualnych?**

    Wartość Tak. Nośnika instalacyjnego programu SQL Server znajduje się w folderze na dysku **C** . Uruchom **Setup.exe** w tej lokalizacji, aby dodać nowe wystąpienia programu SQL Server lub zmienić inne zainstalowane funkcje programu SQL Server na komputerze.

3. **Jak uaktualnić do wersji i wydania programu SQL Server w maszyn wirtualnych Azure?**

    Obecnie nie ma możliwości uaktualnienia w miejscu dla programu SQL Server działa w maszyn wirtualnych Azure. Tworzenie nowego Azure maszyny wirtualnej w odpowiedniej wersji programu SQL Server i wersji, a następnie przeprowadzić migrację baz danych na nowy serwer przy użyciu standardowych [technik migracji danych](virtual-machines-windows-migrate-sql.md).

4. **Jak zainstalować Moja licencjonowana kopia programu SQL Server na maszyn wirtualnych Azure**

    Kopiowanie nośnika instalacyjnego programu SQL Server do maszyn wirtualnych systemu Windows Server, a następnie zainstalować programu SQL Server na maszyn wirtualnych. Licencjonowania powodów, musi mieć [Licencję mobilności poprzez Software Assurance Azure](https://azure.microsoft.com/pricing/license-mobility/).

5. **Czy masz zapłacić kosztów SQL maszyny, jeśli jest on używany tylko do wstrzymania i przełączania awaryjnego?**

    Jeśli tworzysz maszyn wirtualnych SQL za pośrednictwem galerii musi mieć oddzielnej licencji dla wstrzymania maszyn wirtualnych SQL i ceny jest taka sama. Jeśli ręcznie zainstalować programu SQL Server na komputerze wirtualnych z [Licencji mobilności](https://azure.microsoft.com/pricing/license-mobility/)masz opcję, aby mieć jedno bezpłatne pasywne wystąpienie programu SQL, do przełączania awaryjnego. Przejrzyj ograniczenia i wymagania.

6. **Jak są aktualizacje i dodatki service pack stosowane maszyn wirtualnych programu SQL Server?**

    Przedstawianie maszyn wirtualnych kontrolę nad na komputerze obsługującym, w tym, kiedy i jak zastosować aktualizacje. W systemie operacyjnym możesz ręcznie zastosować aktualizacji systemu windows lub można włączyć usługę planowania, o nazwie [Automatycznego poprawiania](virtual-machines-windows-classic-sql-automated-patching.md). Automatyczne instalacje Patching wszystkie aktualizacje, które są oznaczone jako ważne, w tym aktualizacje programu SQL Server w tej kategorii. Inne opcjonalne aktualizacje do programu SQL Server musi być zainstalowany ręcznie.

7. **Jest to możliwe do ustawiania konfiguracji nie jest wyświetlana w galerii maszyn wirtualnych (na przykład Windows 2008 R2 + programu SQL Server 2012)?**

    Wartość nie. Dla maszyn wirtualnych Galeria obrazów, które obejmują programu SQL Server możesz wybrać jeden z podanych obrazów.

9. **Jak zainstalować narzędzia danych SQL na moim maszyn wirtualnych Azure?**

    Pobierz i zainstaluj narzędzia danych SQL z [Programu Microsoft SQL Server Data Tools – Business Intelligence for Visual Studio 2013 ](https://www.microsoft.com/en-us/download/details.aspx?id=42313).

## <a name="resources"></a>Zasoby

Aby uzyskać omówienie programu SQL Server na maszyn wirtualnych Azure Obejrzyj wideo [maszyn wirtualnych Azure jest najlepszą platformą w przypadku programu SQL Server 2016](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/Azure-VM-is-the-best-platform-for-SQL-Server-2016). Możesz również wyświetlić dobre wprowadzenie w temacie [Programu SQL Server w środowisku maszyn wirtualnych systemu Azure Przegląd](virtual-machines-windows-sql-server-iaas-overview.md).

Inne zasoby obejmują:

- [Inicjowanie obsługi programu SQL Server maszyny wirtualnej Azure Portal](virtual-machines-windows-portal-sql-server-provision.md)
- [Migrowanie bazy danych do programu SQL Server na Azure maszyn wirtualnych](virtual-machines-windows-migrate-sql.md)
- [Wysoką dostępność i odzyskiwanie w przypadku programu SQL Server w środowisku maszyn wirtualnych Azure](virtual-machines-windows-sql-high-availability-dr.md)
- [Wydajność najważniejsze wskazówki dotyczące programu SQL Server w maszyn wirtualnych Azure](virtual-machines-windows-sql-performance.md)
- [Desenie aplikacji i strategii rozwoju dla programu SQL Server w środowisku maszyn wirtualnych Azure](virtual-machines-windows-sql-server-app-patterns-dev-strategies.md)
