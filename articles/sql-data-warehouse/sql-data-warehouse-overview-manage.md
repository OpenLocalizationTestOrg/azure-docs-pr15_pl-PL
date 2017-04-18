<properties
   pageTitle="Zarządzanie bazami danych w magazynie danych SQL Azure | Microsoft Azure"
   description="Omówienie Zarządzanie bazami danych SQL magazynu danych. Zawiera narzędzia do zarządzania, DWUs i wydajność skala w nowym oknie, rozwiązywanie problemów z wydajności kwerend, ustanawianie zasad zabezpieczeń dobre i przywracanie bazy danych z uszkodzenie danych lub awaria regionalne."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/16/2016"
   ms.author="barbkess;sonyama;"/>

# <a name="manage-databases-in-azure-sql-data-warehouse"></a>Zarządzanie bazami danych w magazynie danych SQL Azure

Program SQL Data Warehouse zautomatyzowanie wielu aspektów Zarządzanie bazami danych. Na przykład skalowanie wydajności wystarczy dostosować zapłacić za odpowiedni poziom zasobów obliczeń, a następnie umożliwić SQL Data Warehouse wykonać tę pracę Skalowanie zewnętrzne i skalowanie ponownie. 

Niewątpliwie można monitorować z pracą do identyfikowania potrzeb wydajności, a także rozwiązywanie problemów z kwerendami długim. Konieczne będzie także wykonywać zadania zabezpieczeń kilka Zarządzanie uprawnieniami użytkowników i logowania.

To Omówienie obejmuje tych aspektów zarządzania magazynu danych SQL.

- Narzędzia do zarządzania
- Skala obliczeń
- Wstrzymywanie i wznawianie
- Najważniejsze wskazówki dotyczące wydajności
- Monitorowanie kwerendy
- Zabezpieczenia
- Kopia zapasowa i przywracanie

## <a name="management-tools"></a>Narzędzia do zarządzania

Różne narzędzia umożliwia zarządzanie bazami danych w magazynie danych SQL. Zarządzanie bazami danych, będzie można opracowywać preferencji narzędzia dla każdego typu zadania, które trzeba wykonać.

### <a name="azure-portal"></a>Azure portal
[Azure portal][] jest oparte na sieci web portalu, którym można tworzyć, aktualizować i usuń bazy danych i monitorowanie zasoby bazy danych. To narzędzie doskonale nadaje się to jeśli dopiero zaczynasz z Azure, zarządzanie niewielką liczbą bazy danych magazynu danych, lub chcesz szybko zrobić coś.

Aby rozpocząć pracę z portalem Azure, zobacz [Tworzenie magazynu danych SQL (Azure portal)][].

### <a name="sql-server-data-tools-in-visual-studio"></a>Program SQL Server Data Tools w programie Visual Studio
[Program SQL Server Data Tools][] (SSDT) w programie Visual Studio umożliwia nawiązanie połączenia, zarządzanie i opracowanie baz danych. Jeśli jesteś deweloperem znanych z programu Visual Studio lub innych zintegrowanych środowiskach programistycznych (IDE), spróbuj użyć SSDT w programie Visual Studio.

SSDT zawiera Eksplorator obiektów serwera SQL, który umożliwia wizualizowanie, łączenie i wykonywanie skryptów w bazach danych magazynu danych SQL. Aby szybko połączyć magazynu danych SQL, możesz po prostu kliknij przycisk **Otwórz w programie Visual Studio** na pasku poleceń podczas przeglądania bazy danych w portalu klasyczny Azure.  

Aby rozpocząć pracę z SSDT w programie Visual Studio, zobacz [Magazynu danych SQL Azure kwerendy przy użyciu programu Visual Studio][].

### <a name="command-line-tools"></a>Narzędzia wiersza polecenia
Narzędzia wiersza polecenia są idealne rozwiązanie w przypadku automatyzowanie z obciążeń pracą.  Programu PowerShell i sqlcmd dwa sposoby doskonałe automatyzacji procesów.  Zalecamy, aby te narzędzia do zarządzania dużą liczbą serwerów logicznych i wdrażanie zmiany zasobów w środowisku produkcyjnym, jak niezbędne zadania mogą być tworzone, a następnie automatycznego.

### <a name="dynamic-management-views"></a>Dynamiczne zarządzanie widokami 

DMVs są masła i chleb zarządzania magazynu danych SQL. Prawie wszystkie informacje, które powierzchni w portalu zależy od DMVs. Aby wyświetlić listę DMVs magazynu danych SQL, zobacz [Widoki magazynu danych SQL systemu][].

Aby rozpocząć pracę, zobacz [Łączenie i kwerenda z sqlcmd][]i [Utwórz bazę danych (programu PowerShell)][].

## <a name="scale-compute"></a>Skala obliczeń

W magazynie danych SQL można szybko skalowanie wydajności lub ponownie według zwiększenie lub zmniejszenie obliczeń zasobów Procesora, pamięci i przepustowość wejścia/wyjścia. Skalowanie wydajności, wszystko, co należy zrobić to dostosowywać liczbę jednostek magazynu danych (DWUs), które program SQL Data Warehouse przydziela do bazy danych. Magazyn danych SQL szybko sprawia, że zmiana i obsługuje wszystkie podstawowe zmiany sprzętu i oprogramowania.

Aby dowiedzieć się więcej na temat skalowania DWUs, zobacz [Wydajność Skala][].

##  <a name="pause-and-resume"></a>Wstrzymywanie i wznawianie

Aby zapisać kosztów, można wstrzymywać i wznawiać obliczeń zasobów na żądanie. Na przykład jeśli nie będzie używać bazy danych w nocy i w weekendy, można Zatrzymaj wskaźnik myszy w takich przypadkach i wznowienie go w ciągu dnia. Nie możesz obciążona dla DWUs podczas baza danych została wstrzymana.

Aby uzyskać więcej informacji zobacz [Wstrzymaj obliczenia][]i [obliczyć życiorysu][].

## <a name="performance-best-practices"></a>Najważniejsze wskazówki dotyczące wydajności

Gdy wprowadzenie do nowych technologii, odkrywanie porady i wskazówki, które działają najlepiej prawo od początku pozwalają zaoszczędzić wiele czasu.  Najlepsze rozwiązania w całej wiele tematów dotyczących naszych spowoduje znalezienie.

Aby wyświetlić wiele podsumowanie najważniejszych zagadnień, przy opracowywaniu usługi Obciążenie pracą, zobacz [Najważniejsze wskazówki magazynu danych SQL][].

## <a name="query-monitoring"></a>Monitorowanie kwerendy

Czasami kwerenda jest uruchomiona zbyt długo, ale nie masz pewności który z nich jest dziedziczonej z istotnymi elementami. Program SQL Data Warehouse ma widoków dynamiczne zarządzanie (DMVs), które umożliwiają wiadomo, która kwerenda trwa zbyt długo. 

Aby znaleźć kwerend, zobacz [Monitorowanie z pracą przy użyciu DMVs][].

## <a name="security"></a>Zabezpieczenia

Aby zachować bezpieczny system, musi znajdować się w obrębie alertu i zabezpieczyć się przed nieautoryzowanym dostępem dowolnego typu. System zabezpieczeń należy upewnij się, że reguły zapory znajdują się w miejscu, tak aby tylko autoryzowanych łączyć adresów IP. Musi odpowiedniego uwierzytelnienia poświadczeń użytkownika. Gdy użytkownik jest połączony z bazą danych, użytkownik tylko powinien mieć uprawnienia do wykonania z minimalnej liczby akcji. Aby zabezpieczyć dane, możesz użyć szyfrowania. Istotne jest również inspekcji i śledzenia, więc można odtwarzanie zdarzenia, jeśli istnieje jakiekolwiek podejrzane działania.

Aby dowiedzieć się o zarządzaniu zabezpieczeń, głowy nad do [Omówienie zabezpieczeń][].

## <a name="backup-and-restore"></a>Kopia zapasowa i przywracanie

Posiadanie zaufanego backps danych jest podstawowe części dowolnej produkcyjnej bazy danych. Program SQL Data Warehouse chroni dane przez automatyczne wykonywanie kopii zapasowych aktywne baz danych w regularnych odstępach czasu. Te kopie zapasowe umożliwiają odzyskiwanie z scenariusze miejsce, w którym został uszkodzony danych lub przypadkowo usunięte danych lub bazę danych.  Harmonogramu kopii zapasowych danych zasady przechowywania i przywracanie bazy danych, zobacz [Przywracanie z migawki][].

## <a name="next-steps"></a>Następne kroki
Używanie projektu bazy danych warto, że zasady ułatwi zarządzanie baz danych w magazynie danych SQL. Aby dowiedzieć się więcej, głowy nad do [Omówienie tworzenia][].

<!--Image references-->

<!--Article references-->
[Tworzenie magazynu danych SQL (Azure Portal)]: sql-data-warehouse-get-started-provision.md
[Tworzenie bazy danych (programu PowerShell)]: sql-data-warehouse-get-started-provision-powershell
[connection]: sql-data-warehouse-develop-connections.md
[Kwerenda Azure SQL magazynu danych przy użyciu programu Visual Studio]: sql-data-warehouse-query-visual-studio.md
[Nawiązywanie połączenia i kwerendy z sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md
[Omówienie tworzenia]: sql-data-warehouse-overview-develop.md
[Monitorowanie z pracą przy użyciu DMVs]: sql-data-warehouse-manage-monitor.md
[Wstrzymaj obliczeń]: sql-data-warehouse-manage-compute-overview.md#pause-compute-bk
[Przywracanie z migawki]: sql-data-warehouse-restore-database-overview.md
[Życiorys obliczeń]: sql-data-warehouse-manage-compute-overview.md#resume-compute-performance-bk
[Skala wydajności]: sql-data-warehouse-manage-compute-overview.md#scale-performance-bk
[Omówienie zabezpieczeń]: sql-data-warehouse-overview-manage-security.md
[Najważniejsze wskazówki magazynu danych SQL]: sql-data-warehouse-best-practices.md
[Widoki systemu magazynu danych SQL]: sql-data-warehouse-reference-tsql-system-views.md

<!--MSDN references-->
[Program SQL Server Data Tools]: https://msdn.microsoft.com/library/mt204009.aspx

<!--Other web references-->
[Azure portal]: http://portal.azure.com/
