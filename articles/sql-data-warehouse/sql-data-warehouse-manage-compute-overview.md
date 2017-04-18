<properties
   pageTitle="Zarządzanie power obliczeń w magazynie danych SQL Azure (omówienie) | Microsoft Azure"
   description="Skala wydajności się możliwości w magazynie danych SQL Azure. Możliwość skalowania dostosowując DWUs lub wstrzymywanie i wznawianie zasobów obliczeń kosztów."
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
   ms.date="09/03/2016"
   ms.author="barbkess;sonyama"/>

# <a name="manage-compute-power-in-azure-sql-data-warehouse-overview"></a>Zarządzanie power obliczeń w magazynie danych SQL Azure (omówienie)

> [AZURE.SELECTOR]
- [Omówienie](sql-data-warehouse-manage-compute-overview.md)
- [Portal](sql-data-warehouse-manage-compute-portal.md)
- [Programu PowerShell](sql-data-warehouse-manage-compute-powershell.md)
- [POZOSTAŁE](sql-data-warehouse-manage-compute-rest-api.md)
- [TSQL](sql-data-warehouse-manage-compute-tsql.md)

Architektura magazynu danych SQL rozdzielający miejsca do magazynowania i obliczeń, umożliwiając każdym przeskalować niezależnie. Dzięki temu można skalować się wydajności podczas zapisywania koszty według tylko płacisz za wydajności, gdy ich potrzebujesz. 

To omówienie opisano następujące możliwości poza skalowanie wydajności magazynu danych SQL i udostępnia zalecenia dotyczące i kiedy ich używać. 

- Skala obliczyć power za pomocą dostosowania [danych magazynu jednostki (DWUs)][]
- Wstrzymaj lub Wznów zasobów obliczeń

<a name="scale-performance-bk"></a>

## <a name="scale-performance"></a>Skala wydajności

W magazynie danych SQL można szybko skalowanie wydajności lub ponownie według zwiększenie lub zmniejszenie obliczeń zasobów Procesora, pamięci i przepustowość wejścia/wyjścia. Skalowanie wydajności, wszystko, co należy zrobić to dostosowanie ilości [danych w magazynie jednostki (DWUs)][] który przydziela magazynu danych SQL do bazy danych. Magazyn danych SQL szybko sprawia, że zmiana i obsługuje wszystkie podstawowe zmiany sprzętu i oprogramowania.

Zniknęła są dni, wymagających poszukiwanie rodzaju procesorów, pamięci lub rodzaju miejsca do magazynowania, należy następująco skonfigurować doskonałe wydajności w magazynie danych. Umieszczenie magazynu danych w chmurze, masz już zajęcie się problemy ze sprzętem niższego poziomu. Zamiast tego magazynu danych SQL zapyta o to pytanie: szybkość chcesz przeanalizować dane? 

### <a name="how-do-i-scale-performance"></a>Jak skalowanie wydajności?

Aby elastically zwiększyć lub zmniejszyć usługi power obliczeń, po prostu zmienić [danych magazynu jednostki (DWUs)][] dla bazy danych. Wydajność zwiększa liniowo po dodaniu DWU więcej.  Na wyższych poziomach DWU musisz dodać więcej niż 100 DWUs do Zwróć uwagę do znacznego ulepszenia wydajności. Ułatwiające zaznacz uskoki użyteczny w DWUs, oferujemy poziomów DWU, które będą dają najlepsze wyniki.
 
Aby dopasować DWUs, możesz użyć dowolnej z poniższych metod poszczególnych.

- [Skala obliczyć power Portal Azure][]
- [Skala obliczyć power przy użyciu programu PowerShell][]
- [Skala obliczeń power z pozostałych interfejsy API][]
- [Skala obliczyć power z TSQL][]

### <a name="how-many-dwus-should-i-use"></a>Ile DWUs należy używać?
 
Wydajność w magazynie danych SQL skale liniowo, a zmiana z jednej obliczeń skali do innego (Powiedz od 100 DWUs do 2000 DWUs) się dzieje w sekundach. Uzyskasz możliwość wypróbować różne ustawienia DWU aż do ustalenia najlepsze dopasowanie aktualnego scenariusza.

Aby dowiedzieć się, co to jest idealnym wartość DWU, spróbuj skalowania w górę lub w dół i uruchamianie kwerend kilka po załadowaniu danych. Ponieważ skalowania jest szybkie, możesz spróbować szereg różnych poziomów wydajności, godziny lub mniej. Należy pamiętać, że magazynu danych SQL jest przeznaczony do przetwarzania dużych ilości danych i, aby wyświetlić jego PRAWDA możliwości skalowania, zwłaszcza w większej skali, którego oferujemy, należy użyć duży zestaw danych, które zbliża się lub przekracza 1 TB.

Zalecenia dotyczące znajdowania najlepszych DWU dla swojego obciążenie pracą:

1. Dla magazynu danych w fazie projektowania Rozpocznij od wybrania niewielka liczba DWUs.  Dobry punkt wyjścia jest DW400 lub DW200.
2. Monitorowanie wydajności aplikacji, obserwowania liczbę DWUs zaznaczone w porównaniu z wydajnością, które można obserwować.
3. Określ, ile szybciej lub wolniej wydajności powinny być dla Ciebie do osiągnięcia poziomu optymalną wydajność dla potrzeb przy założeniu skali liniowej.
4. Zwiększ lub Zmniejsz liczbę DWUs proporcjonalnie do jak dużo szybciej lub wolniej ma z pracą do wykonania. Usługa szybkiej odpowiedzi i dopasowywanie zasobów obliczeń w celu spełnienia nowych wymogów DWU.
5. Kontynuuj wprowadzanie korekt, aż do poziomu optymalną wydajność dla potrzeb biznesowych.

### <a name="when-should-i-scale-dwus"></a>Kiedy skalować DWUs?

Jeśli chcesz szybsze uzyskiwanie wyników, Zwiększ usługi DWUs i zapłacić w celu zwiększenia wydajności.  Po potrzebujesz mniej obliczyć power, Zmniejsz usługi DWUs i zapłacić tylko w przypadku tego, czego potrzebujesz. 

Zalecenia dotyczące kiedy przeskalować DWUs:

1. Jeśli aplikacja ma zmian obciążenie pracą, skala DWU poziomy w górę lub w dół, aby zezwalały na wartości i niskie punkty. Na przykład jeśli z pracą zwykle pikach na koniec miesiąca, planowanie Dodaj więcej DWUs tych dni Alokacja maksymalna, a następnie skali po zakończeniu szczytowy okres.
2. Aby wykonać operację ładowania lub przekształcenie dużą ilością danych, rozbudowy DWUs tak, aby szybciej danych jest dostępna.

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Wstrzymaj obliczeń

[AZURE.INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Aby wstrzymać bazy danych, użyj jednej z następujących metod poszczególnych.

- [Wstrzymaj do uruchamiania Portal Azure][]
- [Wstrzymaj do uruchamiania przy użyciu programu PowerShell][]
- [Wstrzymaj do uruchamiania z pozostałych interfejsy API][]

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Życiorys obliczeń

[AZURE.INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

Aby wznowić bazy danych, wykonaj dowolną z następujących metod poszczególnych.

- [Życiorys obliczeń Portal Azure][]
- [Życiorys obliczeń przy użyciu programu PowerShell][]
- [Życiorys obliczeń z pozostałych interfejsy API][]

## <a name="permissions"></a>Uprawnienia

Skalowanie bazy danych wymaga uprawnienia opisane w [ALTER DATABASE][].  Wstrzymywanie i wznawianie wymaga uprawnienia [Współautora bazy danych SQL][] , w szczególności Microsoft.Sql/servers/databases/action.

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Następne kroki
Można znaleźć w następujących artykułach, aby lepiej zrozumieć pojęcia niektóre dodatkowe klucza wydajności:

- [Zarządzanie pracą i współbieżności][]
- [Przegląd projektu tabeli][]
- [Dystrybucja tabeli][]
- [Indeksowanie tabeli][]
- [Podziału tabeli][]
- [Tabeli statystyki][]
- [Najważniejsze wskazówki][]

<!--Image reference-->

<!--Article references-->
[jednostki magazynu danych (DWUs)]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units

[Skala obliczyć power Portal Azure]: ./sql-data-warehouse-manage-compute-portal.md#scale-compute-bk
[Skala obliczyć power przy użyciu programu PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#scale-compute-bk
[Skala obliczeń power z pozostałych interfejsy API]: ./sql-data-warehouse-manage-compute-rest-api.md#scale-compute-bk
[Skala obliczyć power z TSQL]: ./sql-data-warehouse-manage-compute-tsql.md#scale-compute-bk

[capacity limits]: ./sql-data-warehouse-service-capacity-limits.md

[Wstrzymaj do uruchamiania Portal Azure]:  ./sql-data-warehouse-manage-compute-portal.md#pause-compute-bk
[Wstrzymaj do uruchamiania przy użyciu programu PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#pause-compute-bk
[Wstrzymaj do uruchamiania z pozostałych interfejsy API]: ./sql-data-warehouse-manage-compute-rest-api.md#pause-compute-bk

[Życiorys obliczeń Portal Azure]:  ./sql-data-warehouse-manage-compute-portal.md#resume-compute-bk
[Życiorys obliczeń przy użyciu programu PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#resume-compute-bk
[Życiorys obliczeń z pozostałych interfejsy API]: ./sql-data-warehouse-manage-compute-rest-api.md#resume-compute-bk

[Zarządzanie pracą i współbieżności]: ./sql-data-warehouse-develop-concurrency.md
[Przegląd projektu tabeli]: ./sql-data-warehouse-tables-overview.md
[Dystrybucja tabeli]: ./sql-data-warehouse-tables-distribute.md
[Indeksowanie tabeli]: ./sql-data-warehouse-tables-index.md
[Podziału tabeli]: ./sql-data-warehouse-tables-partition.md
[Tabeli statystyki]: ./sql-data-warehouse-tables-statistics.md
[Najważniejsze wskazówki]: ./sql-data-warehouse-best-practices.md 
[development overview]: ./sql-data-warehouse-overview-develop.md

[Współautor bazy danych SQL]: ../active-directory/role-based-access-built-in-roles.md#sql-db-contributor

<!--MSDN references-->
[ZMIANY BAZY DANYCH]: https://msdn.microsoft.com/library/mt204042.aspx

<!--Other Web references-->
[Azure portal]: http://portal.azure.com/
