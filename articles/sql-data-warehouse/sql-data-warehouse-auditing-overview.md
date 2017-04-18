<properties
   pageTitle="Inspekcja w magazynie danych Azure SQL | Microsoft Azure"
   description="Rozpoczynanie pracy z inspekcji w magazynie danych SQL Azure"
   services="sql-data-warehouse"
   documentationCenter=""
   authors="ronortloff"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.workload="data-management"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="09/24/2016" 
   ms.author="rortloff;barbkess;sonyama"/>

# <a name="auditing-in-azure-sql-data-warehouse"></a>Inspekcja w magazynie danych Azure SQL

> [AZURE.SELECTOR]
- [Inspekcja](sql-data-warehouse-auditing-overview.md)
- [Wykrywanie zagrożenie](sql-data-warehouse-security-threat-detection.md)

Inspekcja magazynu danych SQL umożliwia rekord, który dziennik zdarzeń w bazie danych do inspekcji na koncie magazyn Azure. Inspekcja może pomóc Obsługa zgodności z przepisami, opis działania bazy danych i lepszy wgląd w niezgodności i różnic w odniesieniu, które mogą wskazywać dotyczy firm lub podejrzane naruszenie zabezpieczeń. Inspekcja magazynu danych SQL również można zintegrować z Microsoft Power BI dla rozwijania raportowania i analizy.

Narzędzia inspekcji Włącz i ułatwienia przestrzeganie standardy zgodności, ale nie daje gwarancji zgodności. Aby uzyskać więcej informacji na temat Azure programy, które obsługują zgodność ze standardami zobacz <a href="http://azure.microsoft.com/support/trust-center/compliance/" target="_blank">Azure Centrum zaufania</a>.

+ [Inspekcja bazy danych — informacje podstawowe]
+ [Konfigurowanie inspekcji dla bazy danych]
+ [Analizowanie dzienników inspekcji i raportów]

##<a id="subheading-1"></a>Podstawy inspekcja bazy danych magazynu danych SQL Azure


Inspekcja bazy danych SQL Data Warehouse umożliwia:

- **Przechowuj** dziennik inspekcji wybranych zdarzeń. Można zdefiniować kategorie Akcje bazy danych pod kątem inspekcji.
- **Raport** działanie bazy danych. Wstępnie skonfigurowane raporty oraz do pulpitu nawigacyjnego umożliwia szybko Rozpocznij pracę z aktywności i raportowaniu zdarzeń.
- **Analizowanie** raportów. Można znaleźć podejrzane zdarzenia, nietypowego działania i trendów.

Możesz skonfigurować inspekcji dla następujących kategorii zdarzeń:

**Zwykły** i języka **SQL sparametryzowane** którego dzienniki inspekcji zbierane są klasyfikowane jako  

- **Dostęp do danych**
- **Zmiany schematu (DDL)**
- **Zmiany danych (DML)**
- **Konta, ról i uprawnień (DCL)**
- **Procedura składowana**, **logowania** i **zarządzania transakcji**.

Dla każdej kategorii zdarzeń inspekcja **Sukces** i **Niepowodzenie** operacji są skonfigurowane oddzielnie.

Aby uzyskać dalsze informacje na temat działań i zdarzeń inspekcji Zobacz <a href="http://go.microsoft.com/fwlink/?LinkId=506733" target="_blank">Odwołanie do formatu dziennika inspekcji (Pobierz plik doc)</a>.

Dzienniki inspekcji są przechowywane na Twoim koncie Azure miejsca do magazynowania. Można zdefiniować okres przechowywania dziennika inspekcji.

Zasady inspekcji można zdefiniować jako domyślną zasadę serwera lub bazy danych. Domyślne zasady inspekcji serwera zostaną zastosowane do wszystkich baz danych na serwerze, które nie mają określonego bazy danych inspekcji zasad zdefiniowane.

Przed ustawieniem up inspekcji inspekcji wyboru, jeśli korzystasz z ["klientów niższych poziomów"](sql-data-warehouse-auditing-downlevel-clients.md).


##<a id="subheading-2"></a>Konfigurowanie inspekcji dla bazy danych

1. Uruchamianie <a href="https://portal.azure.com" target="_blank">Azure Portal</a>.

2. Przejdź do karta konfiguracji bazy danych SQL Data Warehouse / programu SQL Server chcesz poddać inspekcji. Kliknij przycisk **Ustawienia** , na górze, a następnie w karta Ustawienia, a następnie wybierz pozycję **Inspekcja**.

    ![][1]

3. W inspekcji karta konfiguracji najpierw usuń zaznaczenie pola wyboru **Dziedziczą ustawienia inspekcji z serwera** . Pozwala na określanie ustawień dla konkretnej bazy danych.

    ![][2]

4. Następnie należy włączyć inspekcję, klikając przycisk **Włącz** .

    ![][3]

5. W inspekcji karta konfiguracji wybierz **Szczegóły przestrzeni DYSKOWEJ** , aby otworzyć karta miejsca do magazynowania dzienników inspekcji. Wybierz konto Azure magazynowania zapisywania dzienników i okres przechowywania. **Porada:** Aby jak najlepiej wykorzystać szablony wstępnie skonfigurowane raporty za pomocą tego samego konta miejsca do magazynowania dla wszystkich inspekcji baz danych.

    ![][4]

6. Kliknij przycisk **OK** , aby zapisać konfigurację szczegóły miejsca do magazynowania.


7. W obszarze **Rejestrowanie przez zdarzenie**kliknij **sukcesów** i **PORAŻEK** Rejestruj wszystkie zdarzenia, lub wybierz pozycję kategorie poszczególne zdarzenia.


8. Jeśli konfigurujesz Inspekcja dla bazy danych, może być konieczne zmiany parametrów połączenia z klienta, aby upewnić się, że inspekcja danych jest poprawnie rejestrowany. Zaznacz temat [Modyfikowania FDQN serwera w parametrach połączenia](sql-data-warehouse-auditing-downlevel-clients.md) dla połączeń klienta niższego poziomu.

9. Kliknij **przycisk OK**.


##<a id="subheading-3">Analizowanie dzienników inspekcji i raportów</a>

Dzienniki inspekcji są agregowane w kolekcji tabel ze sklepu z prefiksem **SQLDBAuditLogs** na koncie Azure miejsca do magazynowania, wybrana podczas instalacji. Można wyświetlić za pomocą narzędzia, takie jak <a href="http://azurestorageexplorer.codeplex.com/" target="_blank">Eksplorator magazynu Azure</a>pliki dziennika.

Szablon raportu wstępnie pulpitu nawigacyjnego jest dostępny jako <a href="http://go.microsoft.com/fwlink/?LinkId=403540" target="_blank">Arkusz kalkulacyjny programu Excel do pobrania</a> ułatwiają szybkie analizowanie danych dziennika. Aby użyć szablonu na dzienników inspekcji, potrzebujesz programu Excel 2013 lub nowszego i dodatku Power Query, który można pobrać <a href="http://www.microsoft.com/download/details.aspx?id=39379">tutaj</a>.

Szablon zawiera dane przykładowe fikcyjnej w nim i możesz skonfigurować dodatku Power Query do zaimportowania dziennika inspekcji bezpośrednio z konta usługi Azure miejsca do magazynowania.

Aby uzyskać bardziej szczegółowe instrukcje dotyczące pracy z szablonu raportu przeczytaj <a href="http://go.microsoft.com/fwlink/?LinkId=506731">How (pobieranie dokumentu)</a>.

![][5]


##<a id="subheading-4">Wskazówki dotyczące zastosowania produkcji</a>
Opis w tej sekcji dotyczą zrzutów ekranu powyżej. <a href="https://portal.azure.com" target="_blank">Azure Portal</a> lub <a href= "https://manage.windowsazure.com/" target="_bank">Klasyczny klasyczny Portal Azure</a> mogą być stosowane.


##<a id="subheading-5"></a>Ponowne generowanie klucza miejsca do magazynowania

Produkcji najprawdopodobniej będzie okresowo odświeżanie kluczy miejsca do magazynowania. Gdy odświeżanie kluczy należy ponownie zapisać zasady. Ten proces jest następująca:.


1. W inspekcji karta konfiguracji (opisane w temacie Konfigurowanie inspekcji sekcji powyżej) Przełącz **Klawisz dostępu miejsca do magazynowania** z *podstawowego* *pomocniczej* i **Zapisywanie**.
![][4]
2. Przejdź do miejsca do magazynowania konfiguracji karta i **ponownie wygenerować** *Klucza podstawowego programu Access*.

3. Przejdź do inspekcji karta konfiguracji, przełączanie **Klawisz dostępu miejsca do magazynowania** z *pomocniczym* *podstawowy* i naciśnij klawisz **Zapisywanie**.

4. Wróć do przechowywania interfejsu użytkownika i **ponownie wygenerować** *Pomocniczej klawisz dostępu* (jako przygotowanie do następnego cyklu odświeżania klawiszy.

##<a id="subheading-6"></a>Automatyzacji
Istnieje kilka poleceń cmdlet programu PowerShell, które umożliwiają konfigurowanie inspekcji w bazie danych SQL Azure. Aby uzyskać dostęp do poleceń cmdlet inspekcji musi być zainstalowany programu PowerShell w trybie Azure Menedżera zasobów.

> [AZURE.NOTE] Moduł [Azure Menedżera zasobów](https://msdn.microsoft.com/library/dn654592.aspx) jest obecnie w podglądzie. Może nie zapewnić takie same możliwości zarządzania jako Azure modułu.

Gdy jesteś w trybie Menedżera zasobów Azure, uruchom `Get-Command *AzureSql*` do wyświetlania listy dostępnych poleceń cmdlet.


<!--Anchors-->
[Inspekcja bazy danych — informacje podstawowe]: #subheading-1
[Konfigurowanie inspekcji dla bazy danych]: #subheading-2
[Analizowanie dzienników inspekcji i raportów]: #subheading-3


<!--Image references-->
[1]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing.png
[2]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-inherit.png
[3]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-enable.png
[4]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-storage-account.png
[5]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-dashboard.png


<!--Link references-->
