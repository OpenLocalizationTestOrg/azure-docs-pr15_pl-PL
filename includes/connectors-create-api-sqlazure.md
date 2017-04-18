### <a name="prerequisites"></a>Wymagania wstępne
- Konto Azure; można utworzyć [bezpłatne konto](https://azure.microsoft.com/free)
- [Bazy danych SQL Azure](../articles/sql-database/sql-database-get-started.md) z jego informacje o połączeniach, takich jak nazwa serwera, nazwa bazy danych i nazwy użytkownika i hasła. Te informacje są uwzględniane w parametrach połączenia bazy danych SQL:
  
    Serwer = tcp:*yoursqlservername*. database.windows.net,1433;Initial wykazu =*yourqldbname*; Informacje o zabezpieczeniach utrzymują = FAŁSZ; Identyfikator użytkownika = {your_username}; Hasło = {your_password}; MultipleActiveResultSets = False; Szyfrowanie = PRAWDA; TrustServerCertificate = False; Limit czasu połączenia = 30;

    Dowiedz się więcej na temat [Baz danych SQL Azure](https://azure.microsoft.com/services/sql-database).

> [AZURE.NOTE] Podczas tworzenia bazy danych SQL Azure, można także tworzyć przykładowe bazy danych dostępnych w programie SQL. 



Przed rozpoczęciem korzystania z bazy danych SQL Azure w aplikacji logiczny, połączenia z bazą danych SQL. Możesz to zrobić łatwo w logiczny aplikacji portalu Azure.  

Nawiązywanie połączenia z bazą danych SQL Azure, wykonując następujące czynności:  

1. Tworzenie aplikacji logicznych. W Projektancie aplikacji logika dodać wyzwalacz, a następnie dodaj akcję. Wybierz pozycję **Pokaż Microsoft zarządzane interfejsy API** na liście rozwijanej, a następnie wprowadź "sql" w polu wyszukiwania. Wybierz jedną z czynności:  

    ![Krok tworzenia połączenia platformy SQL Azure](./media/connectors-create-api-sqlazure/sql-actions.png)

2. Jeśli nie utworzono jeszcze wszystkie połączenia z bazą danych SQL, zostanie wyświetlony monit o szczegóły połączenia:  

    ![Krok tworzenia połączenia platformy SQL Azure](./media/connectors-create-api-sqlazure/connection-details.png) 

3. Wprowadź szczegóły bazy danych SQL. Właściwości gwiazdką są wymagane.

    | Właściwość | Szczegóły |
|---|---|
| Połączenia za pomocą bramy | Pozostaw zaznaczone. To jest używana podczas nawiązywania połączenia do lokalnego programu SQL Server. |
| Nazwa połączenia * | Wpisz dowolną nazwę połączenia. | 
| Nazwa serwera SQL * | Wprowadź nazwę serwera; który jest podobny do *servername.database.windows.net*. Nazwa serwera jest wyświetlane we właściwościach bazy danych SQL Azure portalu, a także wyświetlana w parametrach połączenia. | 
| Nazwa bazy danych SQL * | Wprowadź nazwę przekazała bazy danych SQL. To jest wyświetlany w oknie Właściwości bazy danych SQL w parametrach połączenia: początkowego wykazu =*yoursqldbname*. | 
| Nazwa użytkownika * | Wprowadź nazwę użytkownika, którego utworzone podczas tworzenia bazy danych SQL. To jest wyświetlany w oknie Właściwości bazy danych SQL w portalu Azure. | 
| Hasło * | Wprowadź hasło utworzone podczas tworzenia bazy danych SQL. | 

    Te poświadczenia są używane do zezwolić aplikacji logika Aby połączyć i uzyskać dostęp do danych SQL. Po zakończeniu szczegóły swojego połączenia wyglądać podobnie do następującej:  

    ![Krok tworzenia połączenia platformy SQL Azure](./media/connectors-create-api-sqlazure/sample-connection.png) 

4. Wybierz polecenie **Utwórz**. 

5. Zwróć uwagę, że utworzono połączenie. Teraz kontynuować innych czynności w aplikacji logiczny: 

    ![Krok tworzenia połączenia platformy SQL Azure](./media/connectors-create-api-sqlazure/table.png)