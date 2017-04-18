<properties
   pageTitle="Ładowanie danych z programu SQL Server do magazynu danych SQL Azure (SSIS) | Microsoft Azure"
   description="Pokazano, jak utworzyć pakiet SQL Server Integration Services (SSIS) do przenoszenia danych z wielu różnych źródeł danych do magazynu danych SQL."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/08/2016"
   ms.author="lodipalm;sonyama;barbkess"/>

# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-ssis"></a>Ładowanie danych z programu SQL Server do magazynu danych SQL Azure (SSIS)

> [AZURE.SELECTOR]
- [SSIS](sql-data-warehouse-load-from-sql-server-with-integration-services.md)
- [PolyBase](sql-data-warehouse-load-from-sql-server-with-polybase.md)
- [BCP](sql-data-warehouse-load-from-sql-server-with-bcp.md)


Utwórz pakiet SQL Server Integration Services (SSIS), aby załadować dane z programu SQL Server do magazynu danych SQL Azure. Opcjonalnie można restrukturyzacja, przekształcenie i czyszczenia danych jako przechodząca SSIS przepływu danych.

W tym samouczku dowiesz się:

- Utwórz nowy projekt usług Integration Services w programie Visual Studio.
- Połączenia ze źródłami danych, takich jak program SQL Server (jako źródło) i magazynu danych SQL (jako miejsca docelowego).
- Projektowanie pakietu SSIS ładującego dane ze źródła do miejsca docelowego.
- Uruchom pakiet SSIS, aby załadować dane.

Ten samouczek używa programu SQL Server jako źródła danych. Program SQL Server może działać w środowisku lokalnym lub Azure maszyn wirtualnych.

## <a name="basic-concepts"></a>Podstawowe pojęcia

Pakiet jest jednostka pracy w SSIS. Pakiety pokrewne są pogrupowane w projektach. Możesz utworzyć projektów i pakietów projektu w programie Visual Studio z programu SQL Server Data Tools. Proces projektowania jest procesem visual, w którym możesz przeciągnij i upuść składniki z przybornika na powierzchnię projektu, połączyć je i ich właściwości. Po zakończeniu pakietu można opcjonalnie wdrożyć go do programu SQL Server dla pełna zarządzania, monitorowania i zabezpieczeń.

## <a name="options-for-loading-data-with-ssis"></a>Opcje ładowania danych z usług SSIS

Program SQL Server Integration Services (SSIS) jest elastyczne zestaw narzędzi, który udostępnia różnych opcji na potrzeby łączenia się i ładowanie danych do magazynu danych SQL.

1. Nawiązywanie połączenia z magazynem danych SQL za pomocą docelowego netto ADO. Ten samouczek korzysta docelowego netto ADO, ponieważ został najmniejsze opcje konfiguracji.
2. Nawiązywanie połączenia z magazynem danych SQL za pomocą OLE DB miejsca docelowego. Ta opcja może dostarczyć nieco lepszą wydajność niż element docelowy netto ADO.
3. Etap danych w magazynie obiektów Blob platformy Azure za pomocą zadania przekazywanie obiektów Blob platformy Azure. Następnie wykonaj zadania SSIS wykonywanie instrukcji SQL na uruchamianie skryptu Polybase ładującego dane do magazynu danych SQL. Ta opcja umożliwia najlepszą wydajność trzy opcje wymienione poniżej. Aby uzyskać zadania przekazywanie obiektów Blob platformy Azure, Pobierz [Programu Microsoft SQL Server 2016 integracji Services Feature Pack dla Azure][]. Aby dowiedzieć się więcej na temat Polybase, zobacz [Przewodnik po PolyBase][].

## <a name="before-you-start"></a>Przed rozpoczęciem

Aby kroków ten samouczek, należy następująco:

1. **SQL Server Integration Services (SSIS)**. SSIS jest składnikiem programu SQL Server i wymaga wersję próbną lub licencjonowana wersja programu SQL Server. Aby uzyskać wersję próbną programu SQL Server 2016 Preview, zobacz [Ocen serwera SQL][].
2. **Programu visual Studio**. Aby uzyskać bezpłatną Visual Studio 2015 społeczności Edition, zobacz [Społeczności programu Visual Studio][].
3. **Program SQL Server Data Tools for Visual Studio (SSDT)**. Aby uzyskać program SQL Server Data Tools Visual Studio 2015 r, zobacz [Pobieranie programu SQL Server danych narzędzia (SSDT)][].
4. **Przykładowe dane**. Samouczku przykładowych danych przechowywanych w programie SQL Server w przykładowej bazie danych AdventureWorks jako danych źródłowych do załadowania do magazynu danych SQL. Aby uzyskać przykładowe bazy danych AdventureWorks, zobacz [AdventureWorks 2014 przykładowe bazy danych][].
5. **Baza danych SQL Data Warehouse i uprawnienia**. Ten samouczek łączy się w przypadku wystąpienia SQL Data Warehouse i załadowaniu danych do niego. Musisz mieć uprawnienia do tworzenia tabeli i załadować dane.
6. **Reguły zapory**. Musisz utworzyć reguły zapory w magazynie danych SQL o adresie IP komputera lokalnego można było przekazać danych do magazynu danych SQL.

## <a name="step-1-create-a-new-integration-services-project"></a>Krok 1: Utwórz nowy projekt usług Integration Services

1. Uruchom program Visual Studio 2015 r.
2. W menu **plik** wybierz nową **| Projekt**.
3. Przejdź do **zainstalowany | Szablony | Funkcje analizy biznesowej | Usługi integracji** typy projektów.
4. Wybierz **projekt usług Integration**. Podaj wartości dla **nazwy** i **lokalizacji**, a następnie wybierz **przycisk OK**.

Program Visual Studio zostanie otwarty i tworzy nowy projekt Integration Services (SSIS). Następnie Visual Studio zostanie otwarty projektant dla jednego nowego pakietu SSIS (Package.dtsx) w projekcie. Pojawi się ekran następujących obszarów:

- Po lewej stronie **zestaw narzędzi** składników SSIS.
- W środku powierzchni projektowej z wieloma kartami. Zazwyczaj jest używana co najmniej **Przepływ sterowania** i kart **Przepływu danych** .
- Po prawej stronie, w **Eksploratorze rozwiązań** i okienka **Właściwości** .

    ![][01]

## <a name="step-2-create-the-basic-data-flow"></a>Krok 2: Tworzenie przepływu danych podstawowe

1. Przeciągnij zadanie przepływu danych z przybornika do środka powierzchni projektowej (na karcie **Przepływ sterowania** ).

    ![][02]

2. Kliknij dwukrotnie zadanie przepływu danych, aby przełączyć do karty przepływu danych.
3. Z listy innych źródeł w przyborniku przeciągnij źródła ADO.NET na powierzchnię projektu. Z karty źródła nadal zaznaczony należy zmienić jego nazwę na **źródła programu SQL Server** w okienku **Właściwości** .
4. Z listy innych miejsc docelowych w przyborniku przeciągnij docelowego ADO.NET na powierzchnię projektu w obszarze źródło ADO.NET. Z kartą docelowego nadal zaznaczony należy zmienić jego nazwę do **miejsca docelowego SQL DW** w okienku **Właściwości** .

    ![][09]

## <a name="step-3-configure-the-source-adapter"></a>Krok 3: Konfigurowanie karta źródła

1. Kliknij dwukrotnie kartę źródła, aby otworzyć **Edytor źródła ADO.NET**.

    ![][03]

2. Na karcie **Connection Manager** **Edytor źródła ADO.NET**kliknij przycisk **Nowy** obok listy **Menedżera połączeń ADO.NET** , aby otworzyć okno dialogowe **Konfigurowanie ADO.NET Connection Manager** i utworzyć ustawienia połączenia bazy danych programu SQL Server, z którego ten samouczek ładowania danych.

    ![][04]

3. W oknie dialogowym **Konfigurowanie ADO.NET Connection Manager** kliknij przycisk **Nowy** , aby otworzyć okno dialogowe **Menedżer połączeń** i Utwórz nowe połączenie danych.

    ![][05]

4. W oknie dialogowym **Menedżer połączeń** wykonaj następujące czynności.

    1. Dla **dostawcy**wybierz dostawcę danych klient SQL.
    2. W polu **Nazwa serwera**wprowadź nazwę serwera SQL.
    3. W sekcji **zalogować się do serwera** wybierz lub wprowadź informacje uwierzytelniające.
    4. W sekcji **połączenie z bazą danych** wybierz przykładowe bazy danych AdventureWorks.
    5. Kliknij przycisk **Testuj połączenie**.
    
        ![][06]
    
    6. W oknie dialogowym informacjami dotyczącymi testowanie połączenia kliknij **przycisk OK** , aby powrócić do okna dialogowego **Menedżer połączeń** .
    7. W oknie dialogowym **Menedżera połączeń** kliknij **przycisk OK** , aby powrócić do okna dialogowego **Konfigurowanie ADO.NET Connection Manager** .
 
5. W oknie dialogowym **Konfigurowanie ADO.NET Connection Manager** kliknij **przycisk OK** , aby powrócić do **Edytor źródła ADO.NET**.
6. W oknie **Edytor źródła ADO.NET**na liście **Nazwa tabeli lub widoku** wybierz tabelę **Sales.SalesOrderDetail** .

    ![][07]

7. Kliknij przycisk **Podgląd** , aby wyświetlić pierwszych 200 wierszy danych z tabeli źródłowej w oknie dialogowym **Podgląd wyników kwerendy** .

    ![][08]

8. W oknie dialogowym **Podgląd wyników kwerendy** kliknij przycisk **Zamknij** , aby powrócić do **Edytor źródła ADO.NET**.
9. W oknie **Edytor źródła ADO.NET**kliknij **przycisk OK** , aby zakończyć konfigurowanie źródła danych.

## <a name="step-4-connect-the-source-adapter-to-the-destination-adapter"></a>Krok 4: Łączenie karta źródła do karty miejsce docelowe

1. Wybierz kartę źródło na powierzchni projektowej.
2. Wybierz pozycję niebieską strzałkę, która zostanie rozszerzony na karcie Źródło i przeciągnij go do edytora miejsca docelowego, dopóki nie zostanie przyciągnięta na miejsce.

    ![][10]

    Typowe pakietu SSIS umożliwia kilku innych składników z przybornika SSIS między źródłowa i docelowa restrukturyzacja, przekształcenie i czyszczenia danych jako przechodząca SSIS przepływu danych. Aby zachować możliwie najprostsze w tym przykładzie, możemy łączysz źródła bezpośrednio do miejsca docelowego.

## <a name="step-5-configure-the-destination-adapter"></a>Krok 5: Konfigurowanie karty miejsce docelowe

1. Kliknij dwukrotnie kartę miejsce docelowe, aby otworzyć **Edytor docelowego ADO.NET**.

    ![][11]

2. Na karcie **Connection Manager** **ADO.NET docelowego edytora**kliknij przycisk **Nowy** obok listy **Menedżera połączeń** , aby otworzyć okno dialogowe **Konfigurowanie ADO.NET Connection Manager** i utworzyć ustawienia połączenia bazy danych magazynu danych SQL Azure, w którym ten samouczek ładowania danych.
3. W oknie dialogowym **Konfigurowanie ADO.NET Connection Manager** kliknij przycisk **Nowy** , aby otworzyć okno dialogowe **Menedżer połączeń** i Utwórz nowe połączenie danych.
4. W oknie dialogowym **Menedżer połączeń** wykonaj następujące czynności.
    1. Dla **dostawcy**wybierz dostawcę danych klient SQL.
    2. W polu **Nazwa serwera**wprowadź nazwę magazynu danych SQL.
    3. W sekcji **zalogować się do serwera** wybierz pozycję **uwierzytelniania Użyj programu SQL Server** , a następnie wprowadź informacje uwierzytelniające.
    4. W sekcji **połączenie z bazą danych** wybierz z istniejącą bazą danych SQL magazynu danych.
    5. Kliknij przycisk **Testuj połączenie**.
    6. W oknie dialogowym informacjami dotyczącymi testowanie połączenia kliknij **przycisk OK** , aby powrócić do okna dialogowego **Menedżer połączeń** .
    7. W oknie dialogowym **Menedżera połączeń** kliknij **przycisk OK** , aby powrócić do okna dialogowego **Konfigurowanie ADO.NET Connection Manager** .
5. W oknie dialogowym **Konfigurowanie ADO.NET Connection Manager** kliknij **przycisk OK** , aby powrócić do **Edytora docelowego ADO.NET**.
6. W oknie **Edytor docelowego ADO.NET**kliknij przycisk **Nowy** obok listy **za pomocą tabeli lub widoku** , aby otworzyć okno dialogowe **Tworzenie tabeli** , aby utworzyć nową tabelę docelowego z listą kolumny, która jest zgodna z tabeli źródłowej.

    ![][12a]

7. W oknie dialogowym **Tworzenie tabeli** wykonaj następujące czynności.

    1. Zmienianie nazwy tabeli docelowej do **Szczegóły zamówienia sprzedaży**.
    2. Usuń kolumnę **może występować** . Typ danych **uniqueidentifier** nie jest obsługiwany w magazynie danych SQL.
    3. Zmień typ danych w kolumnie **LineTotal** **pieniądze**. **Dziesiętna** typ danych nie jest obsługiwany w magazynie danych SQL. Aby uzyskać informacje na temat typów danych obsługiwanych zobacz [Tworzenie tabeli (magazynu danych SQL Azure, Parallel Data Warehouse)][].
    
        ![][12b]
    
    4. Kliknij **przycisk OK** , aby utworzyć tabelę i powrócić do **Edytora docelowego ADO.NET**.

8. W oknie **Edytor docelowego ADO.NET**wybierz kartę **mapowania** , aby zobaczyć, jak kolumn w źródle są mapowane na kolumny w miejscu docelowym.

    ![][13]

9. Kliknij **przycisk OK** , aby zakończyć konfigurowanie źródła danych.

## <a name="step-6-run-the-package-to-load-the-data"></a>Krok 6: Uruchomić pakiet, aby załadować dane

Uruchom pakiet, klikając przycisk **Start** na pasku narzędzi lub wybierając jedną z opcji **Uruchom** w menu **Debugowanie** .

Jak pakiet rozpoczyna się, zostanie wyświetlony żółty koła wirująca, aby wskazać aktywności, jak również liczby wierszy pory przetwarzane.

![][14]

Po zakończeniu wykonywania pakietu Zobacz zielone symbole zaznaczenia w celu wskazania sukcesu, a także całkowitą liczbę wierszy danych załadowane ze źródła do miejsca docelowego.

![][15]

Gratulacje! Aby załadować dane do magazynu danych SQL Azure pomyślnie wykorzystano SQL Server Integration Services.

## <a name="next-steps"></a>Następne kroki

- Dowiedz się więcej na temat SSIS przepływu danych. Rozpocznij tutaj: [Przepływ danych][].
- Dowiedz się, jak debugowania i rozwiązywanie problemów z swoich pakietów bezpośrednio w środowisko projektowe. Rozpocznij tutaj: [Rozwiązywanie problemów z narzędzia do projektowania pakietu][].
- Dowiedz się, jak wdrożyć SSIS projektów i pakietów Integration Services Server lub w innej lokalizacji przechowywania. Rozpocznij tutaj: [wdrożenie projektów i pakietów][].

<!-- Image references -->
[01]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ssis-designer-01.png
[02]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ssis-data-flow-task-02.png
[03]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-source-03.png
[04]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-connection-manager-04.png
[05]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-connection-05.png
[06]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/test-connection-06.png
[07]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-source-07.png
[08]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/preview-data-08.png
[09]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/source-destination-09.png
[10]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/connect-source-destination-10.png
[11]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-destination-11.png
[12a]: ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/destination-query-before-12a.png
[12b]: ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/destination-query-after-12b.png
[13]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/column-mapping-13.png
[14]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/package-running-14.png
[15]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/package-success-15.png

<!-- Article references -->

<!-- MSDN references -->
[Przewodnik PolyBase]: https://msdn.microsoft.com/library/mt143171.aspx
[Pobierz program SQL Server Data Tools (SSDT)]: https://msdn.microsoft.com/library/mt204009.aspx
[Tworzenie tabeli (magazynu danych Azure SQL, magazynu danych równoległe)]: https://msdn.microsoft.com/library/mt203953.aspx
[Przepływ danych]: https://msdn.microsoft.com/library/ms140080.aspx
[Narzędzia do rozwiązywania problemów dla rozwoju pakietu]: https://msdn.microsoft.com/library/ms137625.aspx
[Wdrożenie projektów i pakietów]: https://msdn.microsoft.com/library/hh213290.aspx

<!--Other Web references-->
[Usług Microsoft SQL Server 2016 Integration Services Feature Pack dla Azure]: http://go.microsoft.com/fwlink/?LinkID=626967
[Program SQL Server ocen]: https://www.microsoft.com/en-us/evalcenter/evaluate-sql-server-2016
[Społeczność programu Visual Studio]: https://www.visualstudio.com/en-us/products/visual-studio-community-vs.aspx
[AdventureWorks 2014 przykładowe bazy danych]: https://msftdbprodsamples.codeplex.com/releases/view/125550
