<properties 
    pageTitle="Przenoszenie danych do bazy danych SQL Azure szkoleniowe Azure na komputerze | Azure" 
    description="Tworzenie tabeli SQL i ładowania danych do tabeli SQL" 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/14/2016"
    ms.author="bradsev" /> 

# <a name="move-data-to-an-azure-sql-database-for-azure-machine-learning"></a>Przenoszenie danych do bazy danych SQL Azure szkoleniowe maszynowego Azure

W tym temacie opisano opcje przenoszenie danych z plików prostych (format CSV lub TSV) lub z danych przechowywanych w programie SQL Server w wersji lokalnej bazy danych programu Azure SQL. W ramach procesu nauki danych zespołu są te zadania przenoszenia danych do chmury.

Temat wokół opcji przenoszenia danych do programu SQL Server w wersji lokalnej szkoleniowe na komputerze zobacz [Przenoszenie danych programu SQL Server na Azure maszyn wirtualnych](machine-learning-data-science-move-sql-server-virtual-machine.md).

Poniższe łącza **menu** tematy z opisami sposobów mogły zjeść tej ostatniej danych w środowiskach docelowej miejsce, w którym dane można przechowywane i przetwarzane podczas procesu nauki danych zespołu (TDSP).

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

W poniższej tabeli przedstawiono opcje przenoszenia danych do bazy danych SQL Azure.

<b>ŹRÓDŁA</b> |<b>Miejsce docelowe: Baza danych SQL Azure</b> |
-------------- |--------------------------------|
<b>Prosty plik (CSV lub TSV sformatowany)</b> |<a href="#bulk-insert-sql-query">Kwerenda SQL Wstawianie zbiorcze |
<b>Środowisko lokalne programu SQL Server</b> | 1. <a href="#export-flat-file">eksportowanie do pliku prostego<br> 2. <a href="#insert-tables-bcp">Kreator migracji bazy danych SQL<br> 3. <a href="#db-migration">bazy danych kopii zapasowych i przywracanie<br> 4. <a href="#adf">Factory azure danych |


## <a name="prereqs"></a>Wymagania wstępne
Procedury opisane w tym miejscu są wymagane:

* **Azure subskrypcji**. Jeśli nie masz subskrypcji, można Załóż [bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/).
* **Konto Azure miejsca do magazynowania**. Korzystasz z konta Azure miejsca do magazynowania do przechowywania danych w tym samouczku. Jeśli nie masz konta usługi Azure miejsca do magazynowania, zobacz artykuł [Tworzenie konta miejsca do magazynowania](storage-create-storage-account.md#create-a-storage-account) . Po utworzeniu konta miejsca do magazynowania, musisz uzyskać klucz konta używanego do uzyskiwania dostępu do przechowywania. Zobacz [Wyświetlanie, Kopiuj i klawisze dostępu wyniku miejsca do magazynowania](storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).
* Dostęp do **bazy danych Azure SQL**. Jeśli musisz skonfigurować bazy danych SQL Azure, [Wprowadzenie do programu Microsoft Azure SQL Database](../sql-database/sql-database-get-started.md) zawiera informacje na temat obsługi administracyjnej nowego wystąpienia bazy danych SQL Azure.
* Zainstalowane i skonfigurowane **Azure programu PowerShell** lokalnie. Aby uzyskać instrukcje zobacz [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md).

**Dane**: procesu migracji przedstawiono przy użyciu [taksówki Warszawa zestawu danych](http://chriswhong.com/open-data/foil_nyc_taxi/). Warszawa taksówki zestawu danych zawiera informacje o danych podróży i targów i jest dostępna, jak wspomniano tego wpisu w magazynie obiektów blob platformy Azure: [Warszawa taksówki danych](http://www.andresmh.com/nyctaxitrips/). Próbki i opis te pliki znajdują się w [Warszawa taksówki podróży zestawu danych w polu Opis](machine-learning-data-science-process-sql-walkthrough.md#dataset).
 
Możesz dostosować procedur opisanych tutaj, aby zbiór danych użytkownika lub zgodnie z opisem przy użyciu zestawu danych taksówki Warszawa, postępuj zgodnie z instrukcjami. Aby przekazać taksówki Warszawa zestawu danych do bazy danych programu SQL Server w wersji lokalnej, wykonaj procedurę opisaną w [Zbiorcze importowanie danych do bazy danych programu SQL Server](machine-learning-data-science-process-sql-walkthrough.md#dbload). Poniższe instrukcje dotyczą programu SQL Server na maszyn wirtualnych Azure, ale jest taka sama procedury dotyczące przekazywania do lokalnego programu SQL Server.


## <a name="file-to-azure-sql-database"></a>Przenoszenie danych ze źródła prosty plik z bazą danych programu Azure SQL

Dane w plikach stała (CSV lub TSV sformatowany) mogą być przenoszone do bazy danych programu Azure SQL za pomocą zapytania SQL wstawić zbiorczo.

### <a name="bulk-insert-sql-query"></a>Kwerenda SQL Wstawianie zbiorcze

Kroki procedury za pomocą kwerendy SQL Wstawianie zbiorcze są podobne do tych opisane w sekcjach przenoszenia danych ze źródła prosty plik do programu SQL Server na maszyn wirtualnych Azure. Aby uzyskać szczegółowe informacje zobacz [Zbiorcze Wstawianie zapytania SQL](machine-learning-data-science-move-sql-server-virtual-machine.md#insert-tables-bulkquery).


##<a name="sql-on-prem-to-sazure-sql-database"></a>Przenoszenie danych z lokalnego programu SQL Server z bazą danych programu Azure SQL

Jeśli źródło danych jest przechowywany w programie SQL Server w wersji lokalnej, istnieją różne możliwości przenoszenia danych do bazy danych programu Azure SQL:

1. [Eksportowanie do pliku prostego](#export-flat-file) 
2. [Kreator migracji bazy danych SQL](#insert-tables-bcp)
3. [Bazy danych kopii zapasowych i przywracanie](#db-migration)
4. [Factory Azure danych](#adf)

Instrukcje dotyczące pierwsze trzy są bardzo podobne do tych sekcji [przenoszenia](machine-learning-data-science-move-sql-server-virtual-machine.md) danych programu SQL Server na Azure maszyn wirtualnych, które obejmują takie same procedury. Łącza do odpowiedniej sekcji w tym temacie przedstawiono w poniższych instrukcjach.

###<a name="export-flat-file"></a>Eksportowanie do pliku prostego

Instrukcje dotyczące tego eksportowania z plikiem prostym są podobne do tych objęte [Eksportowanie do pliku prostego](machine-learning-data-science-move-sql-server-virtual-machine.md#export-flat-file).

###<a name="insert-tables-bcp"></a>Kreator migracji bazy danych SQL

Procedura korzystania z Kreatora migracji bazy danych SQL są podobne do tych objęte [Kreator migracji bazy danych SQL](machine-learning-data-science-move-sql-server-virtual-machine.md#sql-migration).

###<a name="db-migration"></a>Bazy danych kopii zapasowych i przywracanie

Instrukcje dotyczące używania bazy danych kopii zapasowych i przywracanie są podobne do wymienione w [bazie danych i przywracania kopii zapasowych](machine-learning-data-science-move-sql-server-virtual-machine.md#sql-backup).

###<a name="adf"></a>Factory Azure danych

Procedura przenoszenia danych do bazy danych programu Azure SQL z Azure Factory danych (ADF) znajduje się w temacie [Przenoszenie danych z lokalnego programu SQL server do platformy SQL Azure z Azure danych Factory](machine-learning-data-science-move-sql-azure-adf.md). W tym temacie przedstawiono sposób przenieść dane z bazy danych programu SQL Server w wersji lokalnej bazy danych programu Azure SQL za pośrednictwem magazyn obiektów Blob platformy Azure za pomocą ADF. 

Warto rozważyć użycie ADF danych trzeba zmigrować ciągłe w scenariuszu hybrydowe, który uzyskuje dostęp do zasobów zarówno w lokalnego, jak i w chmurze, a dane jest wykonany lub musi można modyfikować lub zostały dodane do niego po migrowane reguł biznesowych. ADF umożliwia planowanie i monitorowanie zadań przy użyciu prostej skryptów JSON, które zarządzają przepływu danych w regularnych odstępach czasu. ADF zawiera również inne funkcje, takie jak obsługa złożonych operacji.




