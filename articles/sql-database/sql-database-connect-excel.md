<properties
    pageTitle="Łączenie programu Excel do bazy danych SQL | Microsoft Azure"
    description="Dowiedz się, jak nawiązywania połączenia z bazą danych Azure SQL w chmurze programu Microsoft Excel. Importowanie danych do programu Excel, raportowanie i danych badań."
    services="sql-database"
    keywords="Łączenie programu excel do sql, importowanie danych do programu excel"
    documentationCenter=""
    authors="joseidz"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/05/2016"
    ms.author="joseidz"/>


# <a name="sql-database-tutorial-connect-excel-to-an-azure-sql-database-and-create-a-report"></a>Baza danych SQL samouczek: łączenie programu Excel z bazy danych programu Azure SQL i tworzenie raportów

> [AZURE.SELECTOR]
- [Programu Visual Studio](sql-database-connect-query.md)
- [SSMS](sql-database-connect-query-ssms.md)
- [Program Excel](sql-database-connect-excel.md)

Dowiedz się, jak połączyć program Excel z bazą danych SQL w chmurze, aby można było importować dane i tworzyć tabele i wykresy na podstawie wartości w bazie danych. W tym samouczku, który skonfiguruje połączenie między programem Excel a tabela bazy danych Zapisz plik zawiera dane i informacje o połączeniu dla programu Excel, a następnie tworzenie wykresu przestawnego na podstawie wartości bazy danych.

Przed rozpoczęciem musisz z bazą danych SQL platformy Azure. Jeśli nie masz, zobacz [Tworzenie pierwszej bazy danych SQL](sql-database-get-started.md) , aby uzyskać bazy danych za pomocą przykładowych danych w górę i uruchamiania w ciągu kilku minut. W tym artykule będzie zaimportować przykładowych danych do programu Excel z tego artykułu, ale można wykonać podobne kroki na swoich danych.

Konieczne będzie również kopię programu Excel. W tym artykule korzysta z [programu Microsoft Excel 2016](https://products.office.com/en-US/).

## <a name="connect-excel-to-a-sql-database-and-create-an-odc-file"></a>Nawiązywanie połączenia z bazą danych SQL programu Excel i Utwórz plik odc

1.  Aby połączyć program Excel do bazy danych SQL, Otwórz program Excel Utwórz nowy skoroszyt i otwórz istniejący skoroszyt programu Excel.

2.  Na pasku menu w górnej części strony kliknij pozycję **dane**, kliknij przycisk **Z innych źródeł**, a następnie kliknij **Z programu SQL Server**.

    ![Wybierz źródło danych: łączenie programu Excel do bazy danych SQL.](./media/sql-database-connect-excel/excel_data_source.png)

    Zostanie otwarty Kreator połączenia danych.

3.  W oknie dialogowym **połączenie z serwerem bazy danych** wpisz bazy danych SQL **o nazwie serwera** chcesz połączyć w formularzu <*nazwa_serwera*>**. database.windows.net**. Na przykład **adworkserver.database.windows.net**.

4.  W obszarze **poświadczenia logowania**kliknij opcję **Użyj następujących nazwę użytkownika i hasło**, wpisz **Nazwę użytkownika** i **hasło** Konfigurowanie serwera bazy danych SQL podczas jego tworzenia, a następnie kliknij przycisk **Dalej**.

    ![Wpisz poświadczenia logowania i nazwa serwera](./media/sql-database-connect-excel/connect-to-server.png)

    > [AZURE.TIP] W zależności od środowiska sieci nie można się połączyć, lub możesz utracić połączenie, jeśli serwer bazy danych SQL nie zezwala ruchu z adresu IP klienta. Przejdź do [portalu Azure](https://portal.azure.com/), kliknij pozycję serwery SQL, kliknij serwera, w obszarze Ustawienia kliknij pozycję Zapora i Dodaj swojego adresu IP klienta. Aby uzyskać szczegółowe informacje, zobacz [jak skonfigurować ustawienia zapory](sql-database-configure-firewall-settings.md) .

5. W oknie dialogowym **Wybieranie bazy danych i tabeli** wybierz bazę danych mają być używane z listy, a następnie kliknij tabele lub widoki, które chcesz pracować z (Wybraliśmy **vGetAllCategories**), a następnie kliknij przycisk **Dalej**.

    ![Wybierz bazę danych i tabeli.](./media/sql-database-connect-excel/select-database-and-table.png)

    Zostanie wyświetlone okno dialogowe **Zapisywanie pliku połączenia danych i kończenie** miejsce, w którym zawierają informacje o plik połączenia (*.odc) bazy danych pakietu Office, która korzysta z programu Excel. Możesz pozostawić ustawienia domyślne lub dostosować odpowiednie opcje.

6. Można pozostawić ustawienia domyślne, ale w szczególności Zanotuj **Nazwę pliku** . **Opis**, **Przyjazną nazwę**i **Słów kluczowych** ułatwiają i innych użytkowników, należy pamiętać, co chcesz się połączyć i Znajdź połączenie. Kliknij pozycję **zawsze próbował używać tego pliku, aby odświeżyć dane** , jeśli chcesz, aby informacje o połączeniu przechowywane w pliku odc, więc można aktualizować, kiedy się z nim połączyć, a następnie kliknij przycisk **Zakończ**.

    ![Zapisywanie pliku odc](./media/sql-database-connect-excel/save-odc-file.png)

    Zostanie wyświetlone okno dialogowe **Importowanie danych** .

## <a name="import-the-data-into-excel-and-create-a-pivot-chart"></a>Importowanie danych do programu Excel i Utwórz wykres przestawny
Teraz, gdy zostaną utworzone połączenie i utworzony plik z informacjami o danych i połączenia, czytasz do importowania danych.

1. W oknie dialogowym **Importowanie danych** kliknij odpowiednią opcję, którego chcesz prezentowanie danych w arkuszu, a następnie kliknij **przycisk OK**. Wybraliśmy **wykresu przestawnego**. Możesz również wybrać utworzyć **Nowy arkusz** lub **Dodaj te dane do modelu danych**. Aby uzyskać więcej informacji na modelach danych zobacz [Tworzenie modelu danych w programie Excel](https://support.office.com/article/Create-a-Data-Model-in-Excel-87E7A54C-87DC-488E-9410-5C75DBCB0F7B). Kliknij pozycję **Właściwości** , aby poznać informacje o pliku odc, że utworzony w poprzednim kroku i wybierz opcje odświeżania danych.

    ![Wybieranie formatu danych w programie Excel](./media/sql-database-connect-excel/import-data.png)

    Arkusz zawiera teraz Tabela przestawna puste i wykres.

8. W obszarze **Pola tabeli przestawnej**zaznacz wszystkie pola wyboru dla pól, które chcesz wyświetlić.

    ![Konfigurowanie raportu bazy danych.](./media/sql-database-connect-excel/power-pivot-results.png)

> [AZURE.TIP]Jeśli chcesz się połączyć innych skoroszytów programu Excel i arkusze do bazy danych, kliknij pozycję **dane**, kliknij przycisk **połączenia**, kliknij przycisk **Dodaj**, wybierz połączenie, którego utworzono z listy, a następnie kliknij **Otwórz**.
> ![Otwieranie połączenia z innym skoroszytem](./media/sql-database-connect-excel/open-from-another-workbook.png)

## <a name="next-steps"></a>Następne kroki

- Dowiedz się, jak [Nawiązywanie połączenia z bazą danych SQL z programu SQL Server Management Studio](sql-database-connect-query-ssms.md) wykonywania zaawansowanych kwerend i analizy.
- Więcej informacji na temat korzyści wynikających z [pul elastyczne](sql-database-elastic-pool.md).
- Dowiedz się, jak [utworzyć aplikacji sieci web, która łączy do bazy danych SQL na wewnętrznej](../app-service-web/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md).
