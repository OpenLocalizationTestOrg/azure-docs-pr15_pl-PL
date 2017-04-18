<properties
    pageTitle="Archiwizowanie bazy danych programu Azure SQL plik PLECAK przy użyciu Azure Portal"
    description="Archiwizowanie bazy danych programu Azure SQL plik PLECAK przy użyciu Azure Portal"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="08/15/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="archive-an-azure-sql-database-to-a-bacpac-file-using-the-azure-portal"></a>Archiwizowanie bazy danych programu Azure SQL plik PLECAK przy użyciu Azure Portal

> [AZURE.SELECTOR]
- [Azure portal](sql-database-export.md)
- [Programu PowerShell](sql-database-export-powershell.md)

Ten artykuł zawiera wskazówki dotyczące archiwizowania bazy danych Azure SQL w pliku PLECAK (przechowywane w magazynie obiektów blob platformy Azure) za pomocą [Azure portal](https://portal.azure.com).

Jeśli chcesz utworzyć archiwum bazy danych programu Azure SQL, schemat bazy danych i danych można wyeksportować do pliku PLECAK. Plik PLECAK jest po prostu pliku ZIP z rozszerzeniem PLECAK. Plik PLECAK później mogą być przechowywane w magazynie obiektów blob platformy Azure lub magazynu lokalnego w lokalizacji lokalnego i później zaimportowanych wstecz do bazy danych SQL Azure lub SQL Server lokalnej instalacji. 

***Zagadnienia dotyczące***

- Archiwum być transakcyjnie spójne, należy się upewnić, że nie zapisu aktywności występuje podczas eksportowania lub że są eksportowane [transakcyjnie spójne kopii](sql-database-copy.md) bazy danych Azure SQL.
- Maksymalny rozmiar pliku PLECAK archiwizowane magazynem obiektów Blob platformy Azure wynosi 200 GB. Aby archiwizować większy plik PLECAK do magazynu lokalnego, użyj narzędzia wiersza polecenia [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) . To narzędzie jest dostarczany z programu Visual Studio i SQL Server. Możesz też [pobrać](https://msdn.microsoft.com/library/mt204009.aspx) najnowszą wersję programu SQL Server Data Tools uzyskanie to narzędzie.
- Archiwizowanie do magazynu Azure premium za pomocą pliku PLECAK nie jest obsługiwane.
- Jeśli operacji eksportowania przekracza 20 godzin, mogą zostać anulowane. Aby zwiększyć wydajność podczas eksportowania, można wykonywać następujące czynności:
 - Tymczasowe Zwiększ poziom obsługi.
 - Zaprzestanie wszystkie odczytywanie i zapisywanie aktywności podczas eksportowania.
 - [Indeks grupowany](https://msdn.microsoft.com/library/ms190457.aspx) za pomocą wartość różną od null na wszystkich dużych tabel. Bez grupowany indeksy eksportu może nie działać Jeśli trwa dłużej niż 6 – 12 godzin. Jest to spowodowane usługę eksportu musi wykonać skanowanie tabeli, aby spróbować wyeksportować całą tabelę. Dobrym sposobem na określenie, jeśli tabele są zoptymalizowane dla eksportu jest Uruchom **DBCC SHOW_STATISTICS** i upewnij się, że *RANGE_HI_KEY* jest inna niż zero i wartość ma dobrej. Aby uzyskać szczegółowe informacje zobacz [DBCC SHOW_STATISTICS](https://msdn.microsoft.com/library/ms174384.aspx).


> [AZURE.NOTE] BACPACs nie mają być używane do tworzenia kopii zapasowych i przywracanie operacji. Baza danych SQL Azure automatycznie tworzy kopie zapasowe dla każdej bazy danych użytkownika. Aby uzyskać szczegółowe informacje zobacz [Omówienie ciągłości firm](sql-database-business-continuity.md).

Aby zakończyć w tym artykule są potrzebne następujące elementy:

- Subskrypcję usługi Azure.
- Baza danych SQL Azure. 
- [Magazyn standardowy Azure konta](../storage/storage-create-storage-account.md) z kontenera obiektów blob do przechowywania PLECAK w magazynie standardowy.

## <a name="export-your-database"></a>Eksportowanie do bazy danych

Otwórz karta Baza danych SQL dla bazy danych, które chcesz wyeksportować.

> [AZURE.IMPORTANT] Aby zagwarantować pliku transakcyjnie spójne PLECAK należy pierwszy, [Utwórz kopię bazy danych](sql-database-copy.md) , a następnie wyeksportuj kopii bazy danych. 

1.  Przejdź do [portalu Azure](https://portal.azure.com).
2.  Kliknij pozycję **bazy danych SQL**.
3.  Kliknij bazę danych do archiwizacji.
4.  W karta Baza danych SQL kliknij **Eksportowanie** , aby otworzyć karta **Eksport bazy danych** :

    ![przycisk Eksportuj][1]

5.  Kliknięcie pozycji **przestrzeń dyskowa** i wybierz swojego konta i obiektów blob kontenera magazynu przechowywania PLECAK:

    ![Eksportowanie bazy danych][2]

6. Wybierz typ uwierzytelniania. 
7.  Wprowadź poświadczenia uwierzytelniania odpowiednią dla serwera Azure SQL zawierającego bazę danych, które są eksportowane.
8.  Kliknij **przycisk OK** , aby archiwizować bazy danych. Kliknięcie przycisku **OK** tworzy żądanie eksportu bazy danych i przesyła go do usługi. Czas, który zajmie eksportowania zależy od rozmiaru i złożoności bazy danych, a poziom obsługi. Otrzymasz powiadomienie.

    ![Eksportowanie powiadomień][3]

## <a name="monitor-the-progress-of-the-export-operation"></a>Monitorowanie postępu operacji eksportowania

1.  Kliknij pozycję **serwery SQL Server**.
2.  Kliknij pozycję serwer zawierający oryginalnej bazy danych (źródło), który właśnie archiwizowane.
3.  Przewiń w dół do operacji.
4.  W karta SQL server kliknij przycisk **Importuj/Eksportuj historię**:

    ![Importowanie eksportowanie historii][4]

## <a name="verify-the-bacpac-is-in-your-storage-container"></a>Upewnij się, że PLECAK znajduje się w swojej kontenera magazynu

1.  Kliknij pozycję **konta miejsca do magazynowania**.
2.  Kliknij konto miejsca do magazynowania, przechowywania archiwum PLECAK.
3.  Kliknij **kontenerów** i wybierz kontener wyeksportowane bazy danych, do informacji (możesz pobrać i zapisać PLECAK tutaj).

    ![Szczegóły pliku .bacpac][5]  

## <a name="next-steps"></a>Następne kroki

- Aby uzyskać informacje o importowaniu PLECAK z bazą danych SQL Azure, zobacz [Importowanie BACPCAC z bazą danych programu Azure SQL](sql-database-import.md)
- Aby uzyskać informacje o importowaniu PLECAK z bazą danych programu SQL Server, zobacz [Importowanie BACPCAC z bazą danych programu SQL Server](https://msdn.microsoft.com/library/hh710052.aspx)



<!--Image references-->
[1]: ./media/sql-database-export/export.png
[2]: ./media/sql-database-export/export-blade.png
[3]: ./media/sql-database-export/export-notification.png
[4]: ./media/sql-database-export/export-history.png
[5]: ./media/sql-database-export/bacpac-archive.png

