<properties
    pageTitle="Przywracanie baz danych z obsługą rozciąganie | Microsoft Azure"
    description="Dowiedz się, jak przywrócić rozciąganie\-włączone baz danych."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/01/2016"
    ms.author="douglasl"/>

# <a name="restore-stretch-enabled-databases"></a>Przywracanie z obsługą rozciąganie baz danych

Przywracanie kopii zapasowej bazy danych, w razie potrzeby odzyskać wiele typów błędów, błędów i awarii.

Aby uzyskać więcej informacji o programie Kopia zapasowa Zobacz [włączone rozciąganie kopii zapasowej bazy danych](sql-server-stretch-database-backup.md).

>   [AZURE.NOTE] Kopia zapasowa jest tylko jednej części pełną wysoką dostępność i rozwiązania biznesowego ciągłości. Aby uzyskać więcej informacji o wysokiej dostępności zobacz [Wysoka dostępność rozwiązań](https://msdn.microsoft.com/library/ms190202.aspx).

## <a name="restore-your-sql-server-data"></a>Przywracanie dane programu SQL Server
Aby naprawić błąd sprzętowy lub uszkodzenia, przywracanie rozciąganie\-baza danych programu SQL Server z kopii zapasowej włączona. Możesz nadal korzystać z metod przywracania programu SQL Server, których obecnie jest używana. Aby uzyskać więcej informacji zobacz [Omówienie odzyskiwania i przywracania](https://msdn.microsoft.com/library/ms191253.aspx).

Po przywróceniu bazy danych programu SQL Server, należy uruchomić procedura składowana **sys.sp_rda_reauthorize_db** , aby ponownie nawiązać połączenie między rozciąganie\-włączone bazy danych programu SQL Server i zdalna baza danych Azure. Aby uzyskać więcej informacji zobacz [Przywracanie połączenie bazy danych programu SQL Server i zdalna baza danych Azure](#restore-the-connection-between-the-sql-server-database-and-the-remote-azure-database).

## <a name="restore-your-remote-azure-data"></a>Przywracanie zdalnej danych Azure

### <a name="recover-a-live-azure-database"></a>Odzyskiwanie live Azure bazy danych
Bazy danych programu SQL Server rozciąganie na Azure migawek wszystkich danych dynamicznych co najmniej raz 8 godzin serwisu przy użyciu migawek miejsca do magazynowania Azure. Migawek są obsługiwane przez 7 dni. Dzięki temu będzie można przywrócić jednego z co najmniej 21 punktów danych w czasie w ciągu ostatnich 7 dni do momentu, gdy wykonano ostatnią migawki.

Aby przywrócić live Azure bazy danych do wcześniejszego punktu w czasie przy użyciu Azure portal, wykonaj następujące czynności.

1. Zaloguj się do portalu Azure.
2. Po lewej stronie ekranu wybierz pozycję **PRZEGLĄDAJ** , a następnie wybierz pozycję **Bazy danych SQL**.
3. Przejdź do bazy danych i zaznacz go.
4. W górnej części karta bazy danych kliknij przycisk **Przywróć**.
5. Podaj nową **nazwę bazy danych**, wybierz **Punkt przywracania** , a następnie kliknij przycisk **Utwórz**.
6. Proces przywracania bazy danych rozpocznie i można monitorować za pomocą **POWIADOMIEŃ**.

### <a name="recover-a-deleted-azure-database"></a>Odzyskiwanie usuniętych Azure bazy danych
Usługa bazy danych programu SQL Server rozciąganie Azure przetwarza migawki bazy danych, zanim bazy danych jest usuwana i zachowuje go przez 7 dni. Po zakończeniu tego procesu nie jest już zachowuje migawki z live bazy danych. Umożliwia przywracanie usuniętych bazy danych do punktu, gdy został usunięty.

Aby przywrócić usunięty Azure bazy danych do punktu usunięcie za pomocą portalu Azure, wykonaj następujące czynności.

1. Zaloguj się do portalu Azure.
2. Po lewej stronie ekranu wybierz pozycję **PRZEGLĄDAJ** , a następnie wybierz pozycję **Serwerów SQL**.
3. Przejdź do swojego serwera, a następnie zaznacz go.
4. Przewiń w dół do symulacji karta swojego serwera, kliknij **Usunięte baz danych** .
5. Wybierz usunięte bazę danych, którą chcesz przywrócić.
5. Podaj nową **nazwę bazy danych** , a następnie kliknij przycisk **Utwórz**.
6. Proces przywracania bazy danych rozpocznie i można monitorować za pomocą **POWIADOMIEŃ**.

## <a name="restore-the-connection-between-the-sql-server-database-and-the-remote-azure-database"></a>Przywróć połączenie między bazą danych programu SQL Server i zdalna baza danych Azure

1.  Jeśli zamierzasz nawiązać przywrócenie Azure bazy danych pod inną nazwą lub w innym regionie, uruchom procedura składowana [sys.sp_rda_deauthorize_db](https://msdn.microsoft.com/library/mt703716.aspx) Aby odłączyć od poprzedniego Azure bazy danych.  

2.  Uruchamianie procedura składowana [sys.sp_rda_reauthorize_db](https://msdn.microsoft.com/library/mt131016.aspx) , aby ponownie nawiązać połączenie lokalne rozciąganie\-baza danych do bazy danych Azure włączona.  

    -   Dostarczanie istniejących poświadczeń występujące bazy danych jako sysname lub varchar\(128\) wartość. \(Nie używaj varchar\(max\).\) Możesz wyszukać nazwę poświadczeń w widoku **sys.database\_występujące\_poświadczeń**.  

    -   Określ, czy utworzyć kopię dane zdalne i komunikować się z kopią (zalecane).  

    ```tsql  
    USE <Stretch-enabled database name>;
    GO
    EXEC sp_rda_reauthorize_db
        @credential = N'<existing_database_scoped_credential_name>',
        @with_copy = 1 ;  
    GO
    ```  

## <a name="see-also"></a>Zobacz też

[Zarządzanie i rozwiązywanie problemów z rozciąganie bazy danych](sql-server-stretch-database-manage.md)

[sys.sp_rda_reauthorize_db (w języku Transact-SQL)](https://msdn.microsoft.com/library/mt131016.aspx)

[Wykonywanie kopii zapasowych i przywracanie bazy danych programu SQL Server](https://msdn.microsoft.com/library/ms187048.aspx)
