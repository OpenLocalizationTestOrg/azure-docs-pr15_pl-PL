<properties
    pageTitle="Nawiązywanie połączenia z bazą danych SQL za pomocą kwerendy C# | Microsoft Azure"
    description="Napisać program w języku C# do kwerendy, a następnie nawiązywanie połączenia z bazą danych SQL. Informacje o adresy IP, parametry połączenia, bezpiecznego logowania i bezpłatnego programu Visual Studio."
    services="sql-database"
    keywords="c# zapytania bazy danych, c# kwerendy, nawiązywanie połączenia z bazą danych, SQL C#"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="08/17/2016"
    ms.author="stevestein"/>



# <a name="connect-to-a-sql-database-with-visual-studio"></a>Nawiązywanie połączenia z bazą danych SQL przy użyciu programu Visual Studio

> [AZURE.SELECTOR]
- [Programu Visual Studio](sql-database-connect-query.md)
- [SSMS](sql-database-connect-query-ssms.md)
- [Program Excel](sql-database-connect-excel.md)

Dowiedz się, jak utworzyć połączenie z bazą danych programu Azure SQL w programie Visual Studio. 

## <a name="prerequisites"></a>Wymagania wstępne


Aby połączyć się z bazą danych SQL przy użyciu programu Visual Studio, są potrzebne następujące elementy: 


- Baza danych SQL Aby nawiązać połączenie. W tym artykule użyto przykładowej bazy danych **AdventureWorks** . Aby uzyskać przykładowe bazy danych AdventureWorks, zobacz [Tworzenie pokaz bazy danych](sql-database-get-started.md).


- Visual Studio aktualizacja z 2013 4 (lub nowszy). Firma Microsoft udostępnia teraz społeczności programu Visual Studio *bezpłatnie*.
 - [Pobieranie programu Visual Studio społeczności](http://www.visualstudio.com/products/visual-studio-community-vs)
 - [Więcej opcji wolny Visual Studio](http://www.visualstudio.com/products/free-developer-offers-vs.aspx)




## <a name="open-visual-studio-from-the-azure-portal"></a>Otwieranie programu Visual Studio z portalem Azure


1. Zaloguj się do [portalu Azure](https://portal.azure.com/).

2. Kliknij pozycję **Więcej usług** > **bazy danych SQL**
3. Otwórz karta bazy danych **AdventureWorks** zlokalizowania i kliknięcia bazy danych *AdventureWorks* .

6. Kliknij przycisk **Narzędzia** w górnej części karta bazy danych:

    ![Nowe zapytanie. Nawiązywanie połączenia z serwerem bazy danych SQL: SQL Server Management Studio](./media/sql-database-connect-query/tools.png)

7. Kliknij przycisk **Otwórz w programie Visual Studio** (jeśli jest potrzebna Visual Studio, kliknij łącze pobierania):

    ![Nowe zapytanie. Nawiązywanie połączenia z serwerem bazy danych SQL: SQL Server Management Studio](./media/sql-database-connect-query/open-in-vs.png)


8. Program Visual Studio zostanie wyświetlona w oknie **połączenie z serwerem** już ustawiona w celu połączenia z serwerem i wybranej w portalu bazy danych.  (Kliknij **Opcje** , aby Sprawdź, czy połączenie jest ustawiona do właściwej bazy danych). Wpisz swoje hasło administratora serwera, a następnie kliknij przycisk **Połącz**.


    ![Nowe zapytanie. Nawiązywanie połączenia z serwerem bazy danych SQL: SQL Server Management Studio](./media/sql-database-connect-query/connect.png)


8. Jeśli nie masz zestawu reguł zapory dla adresu IP komputera, jest wyświetlany komunikat *nie można połączyć* w tym miejscu. Aby utworzyć reguły zapory, zobacz [Konfigurowanie reguły zapory na poziomie serwera bazy danych SQL Azure](sql-database-configure-firewall-settings.md).


9. Po pomyślnym nawiązaniu połączenia za pomocą połączenia z bazą danych zostanie otwarte okno **Eksploratora obiektów programu SQL Server** .

    ![Nowe zapytanie. Nawiązywanie połączenia z serwerem bazy danych SQL: SQL Server Management Studio](./media/sql-database-connect-query/sql-server-object-explorer.png)


## <a name="run-a-sample-query"></a>Uruchamianie kwerendy próbki

Teraz, gdy masz możemy połączenie z bazą danych, poniższe kroki pokazują jak uruchomić prostej kwerendy:

2. Kliknij prawym przyciskiem myszy bazę danych, a następnie wybierz pozycję **Nowe zapytanie**.

    ![Nowe zapytanie. Nawiązywanie połączenia z serwerem bazy danych SQL: SQL Server Management Studio](./media/sql-database-connect-query/new-query.png)

3. W oknie dialogowym Kwerenda skopiuj i wklej następujący kod.

        SELECT
        CustomerId
        ,Title
        ,FirstName
        ,LastName
        ,CompanyName
        FROM SalesLT.Customer;

4. Kliknij przycisk **Wykonaj** , aby uruchomić kwerendę:

    ![Zakończyło się pomyślnie. Nawiązywanie połączenia z serwerem bazy danych SQL: SVisual Studio](./media/sql-database-connect-query/run-query.png)

## <a name="next-steps"></a>Następne kroki

- Otwieranie bazy danych programu SQL w programie Visual Studio korzysta z programu SQL Server Data Tools. Aby uzyskać więcej informacji zobacz [Program SQL Server Data Tools](https://msdn.microsoft.com/library/hh272686.aspx).
- Aby nawiązać połączenie z bazą danych SQL przy użyciu kodu, zobacz [Nawiązywanie połączenia z bazą danych SQL przy użyciu .NET (C#)](sql-database-develop-dotnet-simple.md).



