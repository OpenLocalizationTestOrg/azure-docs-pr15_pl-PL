<properties
   pageTitle="Wizualizowanie danych magazynu danych SQL platformy Microsoft Azure Power BI"
   description="Wizualizowanie danych SQL magazynu danych dzięki usłudze Power BI"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor="" />

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/16/2016"
   ms.author="lodipalm;barbkess;sonyama" />

# <a name="visualize-data-with-power-bi"></a>Wizualizowanie danych za pomocą usługi Power BI

> [AZURE.SELECTOR]
- [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
- [Nauka Azure komputera](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
- [Programu Visual Studio](sql-data-warehouse-query-visual-studio.md)
- [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 

Tym samouczku pokazano, jak za pomocą usługi Power BI połączenie z magazynu danych SQL i utworzyć kilka podstawowych wizualizacji.

> [AZURE.VIDEO azure-sql-data-warehouse-sample-data-and-powerbi]

## <a name="prerequisites"></a>Wymagania wstępne

Aby kroków ten samouczek, należy następująco:

- Program SQL Data Warehouse wstępnie załadowane z bazą danych AdventureWorksDW. Do zapewniania obsługi to, zobacz [Tworzenie magazynu danych SQL][] i wybierz pozycję, aby załadować dane przykładowe. Jeśli już masz magazynu danych, ale nie masz przykładowe dane, możesz [ręcznie ładowanie przykładowych danych][].


## <a name="1-connect-to-your-database"></a>1. połączenia z bazą danych

Aby otworzyć usługę Power BI i połączenia z bazą danych AdventureWorksDW:

1. Zaloguj się do [portalu Azure][].
2. Kliknij pozycję **bazy danych programu SQL** i Wybieranie bazy danych AdventureWorks SQL Data Warehouse.

    ![Znajdź swoją bazę danych][1]

3. Kliknij przycisk "Otwórz w usłudze Power BI".

    ![Przycisk Power BI][2]

4. Powinien zostać wyświetlony stronie połączenie z magazynu danych SQL, wyświetlając adres strony sieci web bazy danych. Kliknij przycisk Dalej.

    ![Power BI połączenia][3]

6. Wprowadź nazwę użytkownika serwera Azure SQL i hasło, a będzie w pełni połączony z bazą danych SQL magazynu danych.

    ![Power BI logowanie][4]

7. Po zapisaniu usługi Power BI, kliknij zestaw danych AdventureWorksDW na karta po lewej stronie. Spowoduje to otwarcie bazy danych.

    ![Power BI Otwórz AdventureWorksDW][5]



## <a name="2-create-a-report"></a>2. Tworzenie raportu

Teraz możesz przystąpić do analizowania danych przykładowych AdventureWorksDW za pomocą usługi Power BI. Aby przeprowadzić analizę, AdventureWorksDW ma widok o nazwie AggregateSales. Ten widok zawiera kilka podstawowych wskaźników do analizy sprzedaży firmy.

1. Aby utworzyć mapę kwoty sprzedaży według kodów pocztowych, w okienku po prawej stronie pola, kliknij widok AggregateSales, aby ją rozwinąć. Kliknij pozycję kolumny kod_pocztowy i KwotaSprzedaży, aby je zaznaczyć.

    ![Power BI wybierz AggregateSales][6]

    Power BI automatycznie rozpoznaje jest dane geograficzne i umieścić go na mapie za Ciebie.

    ![Power BI mapy][7]

2. Spowoduje to utworzenie wykresu słupkowego, przedstawiający wielkość sprzedaży według dochód klienta. Aby utworzyć to przejdź do widoku rozszerzonego AggregateSales. Kliknij pole KwotaSprzedaży. Przeciągnij pole dochód klienta z lewej strony i upuść go na osi.

    ![Power BI wybierz osi][8]

    Na wykresie słupkowym możemy przeniesiony po lewej stronie.

    ![Power BI paskiem][9]

3. Spowoduje to utworzenie wykresu liniowego, przedstawiający kwoty sprzedaży według daty zamówienia. Aby utworzyć to przejdź do widoku rozszerzonego AggregateSales. Kliknij pozycję KwotaSprzedaży i OrderDate. W wizualizacji kolumny kliknij ikonę wykresu liniowego; to jest pierwszą ikonę w drugim wierszu w obszarze wizualizacji.

    ![Zaznacz wiersz wykres w programie Power BI][10]

    Teraz masz raportu, który zawiera trzy różnych wizualizacji danych.

    ![Power BI wiersza][11]

Postęp można zapisać w dowolnej chwili, klikając pozycję **plik** i wybierając, **Zapisz**.

## <a name="next-steps"></a>Następne kroki
Teraz, gdy mamy przedstawiania trochę czasu, ogrzać z danymi przykładowymi, zobacz jak [opracowywać][], [Ładowanie][]i [migracji][]. Lub zapoznaj się z w [witrynie sieci Web usługi Power BI][].

<!--Image references-->
[1]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-find-database.png
[2]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-button.png
[3]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-connect-to-azure.png
[4]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-sign-in.png
[5]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-open-adventureworks.png
[6]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-aggregatesales.png
[7]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-map.png
[8]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-chooseaxis.png
[9]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-bar.png
[10]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-prepare-line.png
[11]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-line.png
[12]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-save.png

<!--Article references-->
[Migrowanie]: sql-data-warehouse-overview-migrate.md
[można opracowywać]: sql-data-warehouse-overview-develop.md
[Ładowanie]: sql-data-warehouse-overview-load.md
[Ręczne ładowanie przykładowych danych]: sql-data-warehouse-load-sample-databases.md
[connecting to SQL Data Warehouse]: sql-data-warehouse-integrate-power-bi.md
[Tworzenie magazynu danych SQL]: sql-data-warehouse-get-started-provision.md

<!--Other-->
[Azure portal]: https://portal.azure.com/
[Power BI witryny sieci Web]: http://www.powerbi.com/
