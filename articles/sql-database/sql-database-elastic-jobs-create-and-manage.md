<properties
    pageTitle="Tworzenie i zarządzanie nimi skalowanego się bazy danych programu SQL Azure za pomocą elastyczne zadania | Azure analizy"
    description="Przeprowadź przez tworzenie i Zarządzanie zadaniem elastyczne bazy danych."
    services="sql-database"
    documentationCenter=""
    manager="jhubbard"
    authors="ddove"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/27/2016"
    ms.author="ddove"/>

# <a name="create-and-manage-scaled-out-azure-sql-databases-using-elastic-jobs-preview"></a>Tworzenie i zarządzanie nimi skalować się bazy danych programu SQL Azure za pomocą elastyczne zadania (wersja preview)

> [AZURE.SELECTOR]
- [Azure portal](sql-database-elastic-jobs-create-and-manage.md)
- [Programu PowerShell](sql-database-elastic-jobs-powershell.md)


**Elastyczne bazy danych zadania** uprościć zarządzanie grupami baz danych, wykonywania operacji administracyjnych, takie jak zmiany schematu, zarządzania poświadczeń, aktualizacje danych odwołania, zbieranie danych dotyczących wydajności lub zbioru telemetrycznego dzierżawy (klienta). Elastyczne zadań baz danych jest obecnie dostępna za pośrednictwem Azure portal i poleceń cmdlet programu PowerShell. Jednak Azure pobierający portalu ograniczona funkcjonalność ograniczona do wykonania przez wszystkie bazy danych w [puli elastyczne bazy danych (wersja preview)](sql-database-elastic-pool.md). Aby uzyskać dostęp do dodatkowych funkcji i wykonywanie skryptów całej grupy baz danych w tym zbiorze niestandardowy lub zestawu shard (utworzone przy użyciu [Biblioteka klienta elastyczne bazy danych](sql-database-elastic-scale-introduction.md)), zobacz [Tworzenie zadań i zarządzania nimi przy użyciu programu PowerShell](sql-database-elastic-jobs-powershell.md). Aby uzyskać więcej informacji o zadaniach zobacz [Omówienie zadań elastyczne bazy danych](sql-database-elastic-jobs-overview.md). 

## <a name="prerequisites"></a>Wymagania wstępne

* Subskrypcję usługi Azure. W bezpłatnej wersji próbnej zobacz [bezpłatną wersję próbną jednego miesiąca](https://azure.microsoft.com/pricing/free-trial/).
* Pula elastyczne bazy danych. Zobacz [temat elastyczne pul bazy danych](sql-database-elastic-pool.md)
* Instalacja składników usługi zadania elastyczne bazy danych. Zobacz [Instalowanie usługi zadania elastyczne bazy danych](sql-database-elastic-jobs-service-installation.md).

## <a name="creating-jobs"></a>Tworzenie zadań

1. Za pomocą [Azure portal](https://portal.azure.com), z istniejącej puli zadania elastyczne bazy danych, kliknij przycisk **Utwórz zadanie**.
2. Wpisz nazwy użytkownika i hasła administratora bazy danych (utworzony podczas instalacji zadań) baza danych sterowania zadania (Magazyn metadanych dla zadań).

    ![Nazwa zadania, wpisz lub Wklej kod i kliknij przycisk Uruchom][1]
2. W karta **Tworzenie zadania** wpisz nazwę zadania.
3. Wpisz nazwę użytkownika i hasło, aby połączyć docelowych baz danych z odpowiednimi uprawnieniami wykonywanie skryptu powiodła się.
4. Wklej lub wpisz skrypt T-SQL.
5. Kliknij przycisk **Zapisz** , a następnie kliknij polecenie **Uruchom**.

    ![Tworzenie zadań i uruchamianie][5]

## <a name="run-idempotent-jobs"></a>Uruchamianie idempotent zadań

Po uruchomieniu skryptu z zestawem baz danych, należy upewnić się, że skrypt jest idempotent. Oznacza to, że skrypt muszą mieć możliwość uruchamiać wiele razy, nawet jeśli go nie powiodło się przed niekompletna. Na przykład, gdy skrypt kończy się niepowodzeniem, zadanie zostanie automatycznie wykonana ponownie do skutku (w granicach, jako ponów próbę logicznych ostatecznie przestaną ponawianie próby). Sposób, w tym celu jest użycie klauzuli "Jeżeli ISTNIEJE" i Usuń wszystkie wystąpienia znalezionych przed utworzeniem nowego obiektu. Przykład przedstawiono poniżej:

    IF EXISTS (SELECT name FROM sys.indexes
            WHERE name = N'IX_ProductVendor_VendorID')
    DROP INDEX IX_ProductVendor_VendorID ON Purchasing.ProductVendor;
    GO
    CREATE INDEX IX_ProductVendor_VendorID
    ON Purchasing.ProductVendor (VendorID);

Możesz też użyć klauzuli "Jeśli nie ISTNIEJE" przed utworzeniem nowego wystąpienia:

    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
     CREATE TABLE TestTable(
      TestTableId INT PRIMARY KEY IDENTITY,
      InsertionTime DATETIME2
     );
    END
    GO

    INSERT INTO TestTable(InsertionTime) VALUES (sysutcdatetime());
    GO

Ten skrypt następnie aktualizuje tabelę utworzony wcześniej.

    IF NOT EXISTS (SELECT columns.name FROM sys.columns INNER JOIN sys.tables on columns.object_id = tables.object_id WHERE tables.name = 'TestTable' AND columns.name = 'AdditionalInformation')
    BEGIN

    ALTER TABLE TestTable

    ADD AdditionalInformation NVARCHAR(400);
    END
    GO

    INSERT INTO TestTable(InsertionTime, AdditionalInformation) VALUES (sysutcdatetime(), 'test');
    GO


## <a name="checking-job-status"></a>Sprawdzanie stanu zadania

Po rozpoczęciu zadania możesz sprawdzić o postępach tego procesu.

1. Na stronie puli elastyczne baza danych kliknij pozycję **Zarządzanie zadaniami**.

    ![Kliknij pozycję "Zarządzanie zadaniami"][2]

2. Kliknij nazwę () zadania. **Stan** może zostać "Ukończone" lub "Nie powiodło się". Szczegóły zadania pojawią się (b) z jego data i godzina tworzenia i uruchamiania. Listy (c), pod który przedstawia postęp skryptu dla każdej bazy danych w puli, podając jego szczegóły daty i godziny.

    ![Sprawdzanie na koniec zadania][3]


## <a name="checking-failed-jobs"></a>Sprawdzanie nie powiodło się zadań

Jeśli zadanie nie powiedzie się, można znaleźć w dzienniku jego wykonanie. Kliknij nazwę zadania nie powiodło się, aby wyświetlić jego szczegóły.

![Zaznacz zadanie nie powiodło się][4]


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-create-and-manage/screen-1.png
[2]: ./media/sql-database-elastic-jobs-create-and-manage/click-manage-jobs.png
[3]: ./media/sql-database-elastic-jobs-create-and-manage/running-jobs.png
[4]: ./media/sql-database-elastic-jobs-create-and-manage/failed.png
[5]: ./media/sql-database-elastic-jobs-create-and-manage/screen-2.png

 
