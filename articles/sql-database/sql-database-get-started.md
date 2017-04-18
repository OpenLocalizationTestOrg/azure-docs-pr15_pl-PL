<properties
    pageTitle="Baza danych SQL samouczek: tworzenie bazy danych SQL | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować bazy danych programu SQL server logicznych, reguły zapory serwera bazy danych SQL i przykładowe dane. Ponadto Dowiedz się, jak połączyć się z narzędziami klienta, konfigurowanie użytkowników i konfigurowanie reguły zapory bazy danych."
    keywords="Samouczek bazy danych SQL, tworzenie bazy danych sql"
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/07/2016"
    ms.author="carlrab"/>


# <a name="sql-database-tutorial-create-a-sql-database-in-minutes-by-using-the-azure-portal"></a>Baza danych SQL samouczek: tworzenie bazy danych SQL w minutach za pomocą portalu Azure

> [AZURE.SELECTOR]
- [Azure portal](sql-database-get-started.md)
- [C#](sql-database-get-started-csharp.md)
- [Programu PowerShell](sql-database-get-started-powershell.md)

W tym samouczku możesz dowiedzieć się, jak używać portal Azure, aby:

- Tworzenie bazy danych programu Azure SQL z przykładowymi danymi.
- Tworzenie reguły zapory na poziomie serwera dla pojedynczego adresu IP lub zakres adresów IP.

Te same czynności można wykonywać przy użyciu [C#](sql-database-get-started-csharp.md) albo [programu PowerShell](sql-database-get-started-powershell.md).

[AZURE.INCLUDE [Login](../../includes/azure-getting-started-portal-login.md)]

<a name="create-logical-server-bk"></a>

## <a name="create-your-first-azure-sql-database"></a>Tworzenie pierwszej bazy danych Azure SQL 

1. Jeśli obecnie nie masz połączenie, połącz się [Azure portal](http://portal.azure.com).
2. Kliknij przycisk **Nowy**, kliknij pozycję **dane + miejsca do magazynowania**, a następnie zlokalizuj **Bazy danych SQL**.

    ![Nowej bazy danych sql 1](./media/sql-database-get-started/sql-database-new-database-1.png)

3. Kliknij opcję **Bazy danych SQL** , aby otworzyć karta bazy danych SQL. Zawartość ta karta zmienia się w zależności od liczby subskrypcji i z istniejących obiektów (takich jak istniejące serwery).

    ![Nowej bazy danych sql 2](./media/sql-database-get-started/sql-database-new-database-2.png)

4. W polu tekstowym **Nazwa bazy danych** Podaj nazwę dla pierwszej bazy danych — takie jak "Moje bazy danych". Zielony znacznik wyboru wskazuje, czy podano prawidłową nazwę.

    ![Nowej bazy danych sql 3](./media/sql-database-get-started/sql-database-new-database-3.png)

5. Jeśli masz wiele subskrypcji, wybierz subskrypcję.
6. W obszarze **Grupa zasobów**kliknij przycisk **Utwórz nowy** i podaj nazwę dla pierwszej grupy zasobów — takie jak "Grupa Mój zasobów". Zielony znacznik wyboru wskazuje, czy podano prawidłową nazwę.

    ![Nowej bazy danych sql 4](./media/sql-database-get-started/sql-database-new-database-4.png)

7. W obszarze **Wybierz źródło**kliknij pozycję **Przykładowe** , a następnie w obszarze **Wybierz przykładowy** kliknij **AdventureWorksLT [wersji 12]**.

    ![Nowej bazy danych sql 5](./media/sql-database-get-started/sql-database-new-database-5.png)

8. W obszarze **serwera**kliknij pozycję **Konfiguruj wymagane ustawienia**.

    ![Nowej bazy danych sql 6](./media/sql-database-get-started/sql-database-new-database-6.png)

9. Wybierz polecenie **Utwórz nowy serwer**z karta serwera. Bazy danych programu Azure SQL jest tworzona w obrębie obiektu serwera, który może być serwer nowego lub istniejącego serwera.

    ![Nowej bazy danych sql 7](./media/sql-database-get-started/sql-database-new-database-7.png)

10. Przejrzyj karta **nowego serwera** , aby poznać informacje, które należy podać dla nowego serwera.

    ![Nowej bazy danych sql 8](./media/sql-database-get-started/sql-database-new-database-8.png)

11. W polu tekstowym **Nazwa serwera** Podaj nazwę dla pierwszego serwera — takie jak "Moje nowe server obiektów". Zielony znacznik wyboru wskazuje, czy podano prawidłową nazwę.

    ![Nowej bazy danych sql 9](./media/sql-database-get-started/sql-database-new-database-9.png)
 
12. W obszarze **logowania administrator serwera**wprowadź nazwę użytkownika w celu logowania administratora dla tego serwera — na przykład "konta Moje Administrator". Ten identyfikator logowania nosi nazwę logowania kapitału serwera. Zielony znacznik wyboru wskazuje, czy podano prawidłową nazwę.

    ![Nowej bazy danych sql 10](./media/sql-database-get-started/sql-database-new-database-10.png)

13. W obszarze **hasło** i **Potwierdź hasło**, wprowadź hasło dla serwera konta logowania kapitału — takich jak "p@ssw0rd1". Zielony znacznik wyboru wskazuje, czy podano prawidłowego hasła.

    ![Nowej bazy danych sql 11](./media/sql-database-get-started/sql-database-new-database-11.png)
 
14. W obszarze **Lokalizacja**zaznacz odpowiednie lokalizacji — na przykład "Wschód Australia" centrum danych.

    ![Nowej bazy danych sql 12](./media/sql-database-get-started/sql-database-new-database-12.png)

15. W obszarze ** serwera tworzenie wersji 12 (najnowszą aktualizacją), zwróć uwagę, tylko mieć możliwość utworzenia bieżącej wersji serwera Azure SQL.

    ![Nowej bazy danych sql 13](./media/sql-database-get-started/sql-database-new-database-13.png)

16. Należy zauważyć, że domyślnie pola wyboru **Zezwalaj azure usług dostępu do serwera** jest zaznaczone i nie można zmienić w tym miejscu. Jest to opcja zaawansowane. Chociaż dla większości scenariuszy nie jest to konieczne, można zmienić to ustawienie w ustawienia zapory serwera dla tego obiektu serwera.

    ![Nowej bazy danych sql 14](./media/sql-database-get-started/sql-database-new-database-14.png)

17. Na nowej karta serwera sprawdź wybrane opcje, a następnie kliknij przycisk **Wybierz** , aby wybrać nowego serwera dla nowej bazy danych.

    ![Nowej bazy danych sql 15](./media/sql-database-get-started/sql-database-new-database-15.png)

18. Na karta bazy danych SQL, w obszarze **ceny warstwy**kliknij opcję **S2 standardowy** , a następnie kliknij **podstawowe** wybierz najniższych warstwa cennik dla pierwszej bazy danych. Cennik warstwa zawsze można później zmienić.

    ![Nowej bazy danych sql 16](./media/sql-database-get-started/sql-database-new-database-16.png)

19. Na karta Baza danych SQL Sprawdź wybrane opcje, a następnie kliknij przycisk **Utwórz** , aby utworzyć pierwszy serwer i bazę danych. Wartości, które podany jest sprawdzana poprawność i rozpoczęcie wdrażania.

    ![Nowej bazy danych sql 17](./media/sql-database-get-started/sql-database-new-database-17.png)

20. Na pasku narzędzi portalu kliknij elementy **powiadomienia** , aby sprawdzić stan wdrożenia.

    ![Nowej bazy danych sql 18](./media/sql-database-get-started/sql-database-new-database-18.png)

>[AZURE.IMPORTANT]Po zakończeniu wdrożenia nowego serwera Azure SQL i bazy danych są tworzone w Azure. Nie można połączyć z nowego serwera lub bazy danych przy użyciu narzędzia programu SQL Server, dopóki nie zostanie utworzony regułę zapory serwera, aby otworzyć zaporę bazy danych SQL, aby połączenia z poza Azure.

[AZURE.INCLUDE [Create server firewall rule](../../includes/sql-database-create-new-server-firewall-portal.md)]

## <a name="next-steps"></a>Następne kroki
Teraz, gdy wykonaniu tego samouczka bazy danych SQL i utworzono bazy danych zawierającego dane przykładowe, możesz przystąpić do Poznaj przy użyciu narzędzia do ulubionych.

- Jeśli znasz języku Transact-SQL i SQL Server Management Studio (SSMS), Dowiedz się, jak [Połącz i kwerendy bazy danych SQL za pomocą SSMS](sql-database-connect-query-ssms.md).

- Jeśli wiesz, Excel, Dowiedz się, jak [Nawiązywanie połączenia z bazą danych SQL platformy Azure z programem Excel](sql-database-connect-excel.md).

- Jeśli możesz już zacząć kodowania, wybierz język programowania w [biblioteki połączeń dla bazy danych SQL i programu SQL Server](sql-database-libraries.md).

- Jeśli chcesz przenieść lokalnej bazy danych programu SQL Server Azure, zobacz [Migrowanie bazy danych do bazy danych SQL](sql-database-cloud-migrate.md) , aby dowiedzieć się więcej.

- Jeśli chcesz załadować niektóre dane do nowej tabeli z pliku CSV przy użyciu narzędzia wiersza polecenia BCP, zobacz [Ładowanie danych do bazy danych SQL z pliku CSV przy użyciu BCP](sql-database-load-from-csv-with-bcp.md).

- Jeśli chcesz rozpocząć eksplorowanie zabezpieczeń bazy danych SQL Azure, zobacz [Wprowadzenie do zabezpieczeń](sql-database-get-started-security.md)


## <a name="additional-resources"></a>Dodatkowe zasoby

[Co to jest baza danych SQL?](sql-database-technical-overview.md)
