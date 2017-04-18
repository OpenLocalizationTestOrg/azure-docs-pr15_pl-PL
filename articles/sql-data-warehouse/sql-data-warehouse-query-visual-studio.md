<properties
   pageTitle="Kwerenda Azure SQL magazynu danych (Visual Studio) | Microsoft Azure"
   description="Magazyn danych SQL kwerendy przy użyciu programu Visual Studio."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/16/2016"
   ms.author="sonyama;barbkess"/>

# <a name="query-azure-sql-data-warehouse-visual-studio"></a>Kwerenda Azure SQL Data Warehouse (Visual Studio)

> [AZURE.SELECTOR]
- [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
- [Nauka Azure komputera](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
- [Programu Visual Studio](sql-data-warehouse-query-visual-studio.md)
- [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 

Używając programu Visual Studio do kwerendy magazynu danych SQL Azure w ciągu kilku minut. Ta metoda korzysta z rozszerzeniem SQL Server danych narzędzia (SSDT) w programie Visual Studio. 

## <a name="prerequisites"></a>Wymagania wstępne

Aby skorzystać z tego samouczka, należy następująco:

+ Istniejący magazyn danych SQL. Aby utworzyć ankietę, zobacz [Tworzenie magazynu danych SQL][].
+ SSDT programu Visual Studio. Jeśli masz program Visual Studio, prawdopodobnie masz już to. Aby uzyskać instrukcje dotyczące instalacji i opcji zobacz [Instalowanie programu Visual Studio i SSDT][].
+ W pełni kwalifikowana nazwa serwera SQL. Aby znaleźć to, zobacz [Łączenie z magazynu danych SQL][].

## <a name="1-connect-to-your-sql-data-warehouse"></a>1. nawiązywanie połączenia z magazynu danych SQL

1. Otwórz program Visual Studio 2013 lub 2015 r.
2. Otwórz Eksploratora obiektów programu SQL Server. Aby to zrobić, wybierz pozycję **Wyświetl** > **Eksplorator obiektów programu SQL Server**.

    ![Eksplorator obiektów programu SQL Server][1]

3. Kliknij ikonę **Dodaj programu SQL Server** .

    ![Dodawanie programu SQL Server][2]

4. Wypełnij pola w oknie dialogowym Łączenie do okna serwera.

    ![Nawiązywanie połączenia z serwerem][3]

    - **Nazwa serwera**. Wprowadź **nazwę serwera** uprzednio identyfikowane.
    - **Uwierzytelnianie**. Wybierz **Uwierzytelnianie programu SQL Server** lub **zintegrowanego uwierzytelniania usługi Active Directory**.
    - **Nazwa użytkownika** i **hasło**. Wprowadź nazwę użytkownika i hasło, jeśli powyżej wybrano uwierzytelnianie programu SQL Server.
    - Kliknij przycisk **Połącz**.

5. Eksplorowanie, rozwiń węzeł serwera Azure SQL. Możesz wyświetlić skojarzone z serwerem bazy danych. Rozwijanie AdventureWorksDW, aby wyświetlić tabele w bazie danych przykładowych.

    ![Przegląd AdventureWorksDW][4]

## <a name="2-run-a-sample-query"></a>2. przykładowa kwerenda uruchamianie

Teraz, gdy nawiązaniu połączenia z bazą danych, Przejdźmy napisać kwerendę.

1. Kliknij prawym przyciskiem myszy bazy danych programu SQL Server Explorer obiektu.

2. Wybierz **Nowe zapytanie**. Zostanie otwarte nowe okno kwerendy.

    ![Nowe zapytanie][5]

3. Skopiuj tę kwerendę TSQL do okna kwerendy:

    ```sql
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```

4. Uruchom zapytanie. Aby to zrobić, kliknij zieloną strzałkę lub za pomocą następujących skrótów: `CTRL` + `SHIFT` + `E`.

    ![Uruchamianie kwerendy][6]

5. Przeglądanie wyników kwerendy. W tym przykładzie tabela FactInternetSales zawiera wiersze 60398.

    ![Wyniki kwerendy][7]

## <a name="next-steps"></a>Następne kroki

Teraz, gdy można łączyć i kwerendy, spróbuj [wizualizacji danych za pomocą PowerBI][].

Aby skonfigurować środowisko uwierzytelniania usługi Azure Active Directory, zobacz [uwierzytelniania do magazynu danych SQL][].

<!--Arcticles-->
[Nawiązywanie połączenia z magazynem danych SQL]: sql-data-warehouse-connect-overview.md
[Tworzenie magazynu danych SQL]: sql-data-warehouse-get-started-provision.md
[Instalowanie programu Visual Studio i SSDT]: sql-data-warehouse-install-visual-studio.md
[Uwierzytelnianie do magazynu danych SQL]: sql-data-warehouse-authentication.md
[Wizualizowanie danych za pomocą PowerBI]: sql-data-warehouse-get-started-visualize-with-power-bi.md  

<!--Other-->
[Azure portal]: https://portal.azure.com

<!--Image references-->

[1]: media/sql-data-warehouse-query-visual-studio/open-ssdt.png
[2]: media/sql-data-warehouse-query-visual-studio/add-server.png
[3]: media/sql-data-warehouse-query-visual-studio/connection-dialog.png
[4]: media/sql-data-warehouse-query-visual-studio/explore-sample.png
[5]: media/sql-data-warehouse-query-visual-studio/new-query2.png
[6]: media/sql-data-warehouse-query-visual-studio/run-query.png
[7]: media/sql-data-warehouse-query-visual-studio/query-results.png
