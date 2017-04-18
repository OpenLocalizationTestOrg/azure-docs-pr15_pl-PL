<properties
   pageTitle="Używanie analizy strumieniu Azure z magazynu danych SQL | Microsoft Azure"
   description="Porady dotyczące korzystania z analizy strumieniu Azure z magazynu danych SQL Azure dla opracowania rozwiązań."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="kevinvngo"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/16/2016"
   ms.author="kevin;barbkess;sonyama"/>

# <a name="use-azure-stream-analytics-with-sql-data-warehouse"></a>Używanie analizy strumieniu Azure z magazynu danych SQL

Azure analizy strumieniu jest w pełni zarządzanych usług, dostarczając małym opóźnieniem wysokiej dostępności, skalowalna złożonych przetwarzania przez strumieniowego przesyłania danych w chmurze. Aby poznać podstawowe zagadnienia, należy do czytania [Wprowadzenie do analizy strumieniu Azure][]. Możesz następnie dowiedzieć się, jak utworzyć rozwiązanie zakończenia do końca analizy strumieniu, wykonując samouczek [rozpocząć korzystanie z platformy Azure analizy strumieniu][] .

W tym artykule dowiesz się, sposób używania magazynu danych SQL Azure bazy danych jako sink wynik dla zadań analizy pary.

## <a name="prerequisites"></a>Wymagania wstępne

Najpierw uruchom przez następujące etapy w samouczku [rozpocząć korzystanie z platformy Azure analizy strumieniu][] .  

1. Tworzenie wydarzenia centrum danych wejściowych
2. Konfigurowanie i rozpoczynanie aplikacji generator zdarzenia
3. Obsługa administracyjna zadanie analizy strumieniu
4. Określ zadania wejście i kwerendy

Następnie utwórz bazę danych magazynu danych SQL Azure

## <a name="specify-job-output-azure-sql-data-warehouse-database"></a>Określ format wyjściowy zadania: baza danych magazynu danych SQL Azure

### <a name="step-1"></a>Krok 1

W swojej pracy analizy strumieniu kliknij przycisk **dane wyjściowe** z górnej części strony, a następnie kliknij **Dodaj dane wyjściowe**.

### <a name="step-2"></a>Krok 2

Wybieranie bazy danych SQL, a następnie kliknij przycisk Dalej.

![][add-output]

### <a name="step-3"></a>Krok 3
Na następnej stronie, wprowadź następujące wartości:

- *Wyjściowy Alias*: wpisz przyjazną nazwę dla tego raportu zadania.
- *Subskrypcji*:
    - Jeśli baza danych SQL Data Warehouse znajduje się w tej samej subskrypcji jako zadanie analizy strumieniu, wybierz pozycję Użyj bazy danych SQL z bieżącej subskrypcji.
    - Jeśli baza danych znajduje się w innej subskrypcji, wybierz pozycję Użyj bazy danych SQL z innej subskrypcji.
- *Bazy danych*: Określ nazwę docelowej bazy danych.
- *Nazwa serwera*: Określ nazwę serwera bazy danych, właśnie został określony. Za pomocą portalu klasyczny Azure znajdowane.

![][server-name]

- *Nazwa użytkownika*: Określ nazwę użytkownika konta, które ma uprawnienia do zapisu bazy danych.
- *Hasło*: hasło dla określonego konta użytkownika.
- *Tabela*: Podaj nazwę tabeli docelowej bazy danych.

![][add-database]

### <a name="step-4"></a>Krok 4

Kliknij przycisk Sprawdź, aby dodać te dane wyjściowe zadania i sprawdź, czy analizy strumieniu można połączyć z bazą danych.

![][test-connection]

Podczas połączenia z bazą danych zakończyło się powodzeniem, pojawi się powiadomienie w dolnej części portalu. U dołu w celu testowania połączenia z bazą danych, możesz kliknąć pozycję Testuj połączenie.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać omówienie integracji zobacz [Omówienie integracji magazynu danych SQL][].

Aby uzyskać więcej porad rozwoju zobacz [Omówienie rozwoju magazynu danych SQL][].

<!--Image references-->

[add-output]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/add-output.png
[server-name]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/dw-server-name.png
[add-database]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/add-database.png
[test-connection]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/test-connection.png

<!--Article references-->

[Wprowadzenie do analizy strumieniu Azure]: ../stream-analytics/stream-analytics-introduction.md
[Wprowadzenie do korzystania z analizy strumieniu Azure]: ../stream-analytics/stream-analytics-get-started.md
[Omówienie tworzenia magazynu danych SQL]:  ./sql-data-warehouse-overview-develop.md
[Omówienie integracji z magazynu danych SQL]:  ./sql-data-warehouse-overview-integrate.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Stream Analytics documentation]: http://azure.microsoft.com/documentation/services/stream-analytics/
