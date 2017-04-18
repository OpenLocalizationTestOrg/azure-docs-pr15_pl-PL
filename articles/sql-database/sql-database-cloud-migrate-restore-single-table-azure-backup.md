<properties
    pageTitle="Przywracanie z kopii zapasowej bazy danych SQL Azure jednej tabeli | Microsoft Azure"
    description="Dowiedz się, jak przywrócić jednej tabeli z kopii zapasowej bazy danych SQL Azure."
    services="sql-database"
    documentationCenter=""
    authors="dalechen"
    manager="felixwu"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/31/2016"
    ms.author="daleche"/>


# <a name="how-to-restore-a-single-table-from-an-azure-sql-database-backup"></a>Jak przywrócić jednej tabeli z kopii zapasowej bazy danych SQL Azure

Mogą wystąpić sytuacja, w której przypadkowo modyfikacji danych w bazie danych SQL, a teraz chcesz odzyskać jedną tabelą dotyczy. W tym artykule opisano, jak przywrócić jednej tabeli w bazie danych z jednej z bazą danych SQL [automatycznych kopii zapasowych](sql-database-automated-backups.md).

## <a name="preparation-steps-rename-the-table-and-restore-a-copy-of-the-database"></a>Czynności przygotowawczych: Zmienianie nazwy tabeli wykonywania i przywracania kopii bazy danych
1. Zidentyfikuj tabelę w bazie danych programu Azure SQL, który chcesz zamienić przywrócona kopia. Zmienianie nazwy tabeli za pomocą programu Microsoft SQL Management Studio. Na przykład zmień nazwę tabeli &lt;Nazwa tabeli&gt;_old.

    **Uwaga** Aby uniknąć blokowaniu, upewnij się, że nic się nie dzieje uruchomionych dla tabeli, która jest zmieniana. Jeśli występują problemy, upewnij się, że wykonać tę procedurę w oknie konserwacji.

2. Przywracanie kopii zapasowej bazy danych do punktu w czasie, którą chcesz odzyskać za pomocą czynności [Przywracanie In_Time punktu](sql-database-recovery-using-backups.md#point-in-time-restore) .

    **Notatki**:
    - Nazwa przywrócenie bazy danych będzie w formacie DBName + sygnatura czasowa. na przykład **Adventureworks2012_2016-01-01T22-12Z**. Ten krok nie zastępować istniejących Nazwa bazy danych na serwerze. Jest to miara bezpieczeństwa i jest ona przeznaczona pozwala sprawdzić przywrócenie bazy danych przed upuść ich bieżącej bazy danych i Zmień nazwę przywrócenie bazy danych do produkcji.
    - Wszystkie poziomy wydajności od najprostszych do premii kopie zapasowe są automatycznie przez usługę, z różnymi metryki przechowywania kopii zapasowej, w zależności od poziomu:

| Przywracanie bazy danych | Podstawowe warstwy | Standardowy warstwy | Poziomy Premium |
| :-- | :-- | :-- | :-- |
|  Przywracanie punktu w czasie |  Dowolny punkt, w ciągu 7 dni|Dowolny punkt ciągu 35 dni| Dowolny punkt ciągu 35 dni|

## <a name="copying-the-table-from-the-restored-database-by-using-the-sql-database-migration-tool"></a>Kopiowanie tabeli z przywrócenie bazy danych przy użyciu narzędzia do migracji bazy danych SQL
1. Pobierz i zainstaluj [Kreator migracji bazy danych SQL](https://sqlazuremw.codeplex.com).

2. Otworzyć Kreatora migracji bazy danych SQL, na stronie **Zaznacz proces** , wybierz **bazę danych w obszarze analiza i migracji**, a następnie kliknij **Dalej**.
![Kreator migracji bazy danych SQL — wybierz proces](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/1.png)
3. W oknie dialogowym **połączenie z serwerem** należy zastosować następujące ustawienia:
 - **Nazwa serwera**: wystąpienie i platformy SQL Azure
 - **Uwierzytelnianie**: **Uwierzytelnianie programu SQL Server**. Wprowadź poświadczenia logowania.
 - **Bazy danych**: **wzorzec DB (listę wszystkich baz danych)**.
 - **Uwaga** Domyślnie kreator zapisuje informacje logowania. Jeśli nie chcesz, aby, wybierz pozycję **Zapomnisz informacje logowania**.
![Migracja bazy danych SQL Kreator — wybierz źródło — krok 1](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/2.png)
4. W oknie dialogowym **Wybieranie źródła** wybierz nazwę przywrócenie bazy danych z sekcji **czynności przygotowawczych** jako źródła, a następnie kliknij przycisk **Dalej**.

    ![Migracja bazy danych SQL Kreator — wybierz źródło — krok 2](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/3.png)

5. W oknie dialogowym **Wybieranie obiektów** wybierz opcję **Wybierz określonych obiektów bazy danych** , a następnie wybierz table(or tables) której chcesz przeprowadzić migrację do serwera docelowego.
![Kreator migracji bazy danych SQL — Wybieranie obiektów](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/4.png)

6. Na stronie **Podsumowanie Kreatora skryptu** kliknij przycisk **Tak** , gdy zostanie wyświetlony monit o tego, czy możesz przystąpić do wygenerowania skrypt języka SQL. Masz również możliwość zapisania skrypt TSQL do późniejszego użycia.
![Kreator migracji bazy danych SQL — Podsumowanie Kreatora skryptu](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/5.png)

7. Na stronie **Podsumowanie wyników** kliknij przycisk **Dalej**.
![Kreator migracji bazy danych SQL — podsumowanie wyników](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/6.png)

8. Na stronie **Ustawienia połączenia z serwerem docelowej** kliknij pozycję **Połącz z serwera**, a następnie wprowadź dane w następujący sposób:
    - **Nazwa serwera**: wystąpienie serwera docelowej
    - **Uwierzytelnianie**: **Uwierzytelnianie programu SQL Server**. Wprowadź poświadczenia logowania.
    - **Bazy danych**: **wzorzec DB (listę wszystkich baz danych)**. Opcja zawiera listę wszystkich baz danych na serwerze docelowym.

    ![Migracja bazy danych SQL wizard — ustawienia połączenia z serwerem docelowej](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/7.png)

9. Kliknij pozycję **Połącz**, wybierz docelową bazę danych, którą chcesz przenieść tabeli i kliknij przycisk **Dalej**. To zakończenia, uruchamianie skryptu wcześniej wygenerowane i powinien zostać wyświetlony tabeli nowo przenoszone, kopiowane do docelowej bazy danych.

## <a name="verification-step"></a>Proces weryfikacji
1. Kwerendy, a następnie przetestuj nowo skopiowanego tabeli, aby upewnić się, że dane jest prawidłowa. Po potwierdzeniu można usunąć formularza o zmienionej nazwie tabeli sekcji **czynności przygotowawczych** . (na przykład &lt;Nazwa tabeli&gt;_old).

## <a name="next-steps"></a>Następne kroki

[Baza danych SQL automatycznych kopii zapasowych](sql-database-automated-backups.md)
