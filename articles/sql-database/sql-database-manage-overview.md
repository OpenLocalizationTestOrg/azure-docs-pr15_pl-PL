<properties
    pageTitle="Omówienie: narzędzia do zarządzania dla bazy danych SQL | Microsoft Azure"
    description="Porównuje narzędzia i opcje służące do zarządzania bazy danych SQL Azure"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="sstein"/>

# <a name="overview-management-tools-for-sql-database"></a>Omówienie: narzędzia do zarządzania dla bazy danych SQL

Ten temat opisuje i porównanie narzędzia i opcje służące do zarządzania baz danych programu SQL Azure, więc można wybrać narzędzia właściwego dla zadania, firmy, a. Wybór właściwego narzędzia zależy od liczby baz danych można zarządzać, zadania i jak często wykonywane zadania.

## <a name="azure-portal"></a>Azure portal

[Azure portal](https://portal.azure.com) to aplikacja sieci web miejsce, w którym można tworzyć, aktualizować i usuwanie baz danych i serwerów logicznych i monitorowanie aktywności bazy danych. To narzędzie jest doskonałe, jeśli dopiero zaczynasz z Azure, zarządzanie bazami danych w kilku, lub chcesz zrobić coś szybko.

Aby uzyskać więcej informacji na temat korzystania z portalu zobacz [Zarządzanie bazami danych SQL za pomocą portalu Azure](sql-database-manage-portal.md).

## <a name="sql-server-management-studio-and-sql-server-data-tools-in-visual-studio"></a>SQL Server Management Studio i SQL Server Data Tools w programie Visual Studio

Program SQL Server Management Studio (SSMS) i SQL Server danych narzędzia (SSDT) są narzędziami klienta, które są uruchamiane na komputerze i zarządzania nimi opracowywania bazy danych w chmurze. Jeśli jesteś deweloperem znanych z programu Visual Studio lub innych zintegrowanych środowiskach programistycznych (IDE), [Spróbuj użyć SSDT w programie Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx). Wielu administratorów bazy danych zaznajomieni z SSMS, które mogą być używane z bazy danych programu SQL Azure. [Pobierz najnowszą wersję SSMS](https://msdn.microsoft.com/library/mt238290) i zawsze używać najnowszej wersji podczas pracy z bazą danych SQL Azure. Aby uzyskać więcej informacji o zarządzaniu baz danych SQL Azure z SSMS zobacz [Zarządzanie bazami danych SQL przy użyciu SSMS](sql-database-manage-azure-ssms.md).

> [AZURE.IMPORTANT] Zawsze używaj najnowszą wersję programu [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290) i [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx) do pozostawać aktualizacje Microsoft Azure i baza danych SQL.


## <a name="powershell"></a>Programu PowerShell

Za pomocą programu PowerShell Zarządzanie bazami danych i pule elastyczne bazy danych, a aby zautomatyzować wdrożeń Azure zasobów. Firma Microsoft zaleca to narzędzie do zarządzania dużej liczby baz danych i ich zautomatyzowanie wdrażania i zmiany zasobów w środowisku produkcyjnym.

Aby uzyskać więcej informacji zobacz [Zarządzanie bazą danych SQL przy użyciu programu PowerShell](sql-database-manage-powershell.md)

## <a name="elastic-database-tools"></a>Elastyczne narzędzia bazy danych
Wykonywanie akcji, takich jak przy użyciu narzędzi elastyczne bazy danych 

* Wykonywanie skryptu T-SQL z zestawem bazy danych przy użyciu [elastyczne zadania](sql-database-elastic-jobs-overview.md)
* Przenoszenie bazy danych modelu wielu dzierżawy do modelu pojedyncze dzierżawy za pomocą [narzędzi korespondencji seryjnej podziału](sql-database-elastic-scale-overview-split-and-merge.md)
* Zarządzanie bazami danych w modelu pojedynczego dzierżawy lub modelu dzierżawy wielu elementów przy użyciu [skali elastyczne Biblioteka klienta](sql-database-elastic-database-client-library.md).
 

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Azure Menedżera zasobów](https://azure.microsoft.com/features/resource-manager/)
- [Azure automatyzacji](https://azure.microsoft.com/documentation/services/automation/)
- [Harmonogram Azure](https://azure.microsoft.com/documentation/services/scheduler/)