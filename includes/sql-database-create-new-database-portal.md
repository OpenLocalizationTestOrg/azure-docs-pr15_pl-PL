
<!--
includes/sql-database-create-new-database-portal.md

Latest Freshness check:  2016-04-11 , carlrab.

As of circa 2016-04-11, the following topics might include this include:
articles/sql-database/sql-database-get-started-tutorial.md

-->
## <a name="create-a-new-azure-sql-database"></a>Tworzenie nowej bazy danych Azure SQL

Aby utworzyć nową bazę danych Azure SQL na serwerze logiczne bazy danych SQL Azure nowym lub istniejącym, wykonaj następujące czynności w portalu Azure.

1. Jeśli obecnie nie masz połączenie, połącz się [Azure portal](http://portal.azure.com).
2. Kliknij przycisk **Nowy**, wpisz **Bazy danych SQL**, a następnie kliknij **Bazy danych SQL (nowej bazy danych)**.

     ![Nowej bazy danych](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-1.png)

3. Kliknij pozycję **Baza danych SQL (nowej bazy danych)**.

     ![Nowej bazy danych](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-2.png)

4. Kliknij przycisk **Utwórz** , aby utworzyć nową bazę danych w usłudze bazy danych SQL.

     ![Nowej bazy danych](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-3.png)

5. Wprowadź wartości dla następujących właściwości serwera:

 - Nazwa bazy danych
 - Subskrypcja: Dotyczy tylko wtedy, gdy masz wiele subskrypcji.
 - Grupa zasobów: Jeśli możesz dopiero zaczynasz, użyj grupy zasobów serwera logiczne.
 - Wybierz źródło: Możesz wybrać pusta baza danych przykładowych danych i kopii zapasowej Azure bazy danych. Aby przeprowadzić migrację bazy danych programu SQL Server lokalnego lub ładowania danych przy użyciu narzędzia wiersza polecenia BCP, skorzystaj z łączy na końcu tego artykułu.
 - Serwer: Nowym lub istniejącym logiczne serwer.
 - Logowanie do administratora serwera
 - Hasło
 - Cennik warstwa: Jeśli po prostu zaczynasz, użyj wartości domyślnej S0.
 - Sortowanie: Dotyczy tylko wtedy, gdy pusta baza danych została wybrana.

        ![New database](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-4.png)

6.  Kliknij przycisk **Utwórz**. W obszarze powiadomień widać rozpoczętej wdrożenia.

     ![Nowej bazy danych](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-5.png)

7. Poczekaj do wdrożenia na zakończenie przed przejściem do następnego kroku.

     ![Nowej bazy danych](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-6.png)
