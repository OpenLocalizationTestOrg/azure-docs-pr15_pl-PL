<properties
   pageTitle="Obciążenia pracą magazynu danych"
   description="Elastyczność magazynu danych SQL umożliwia powiększanie, zmniejsz lub wstrzymać power obliczeń przy użyciu suwakiem jednostek magazynu danych (DWUs). W tym artykule wyjaśniono, metryki magazynu danych i ich relacji z DWUs. "
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
   ms.date="07/25/2016"
   ms.author="barbkess;mausher;jrj;sonyama"/>


# <a name="data-warehouse-workload"></a>Obciążenia pracą magazynu danych
Obciążenie pracą magazynu danych odwołuje się do wszystkich działań, które orzeczona przed magazynu danych. Obciążenia pracą magazynu danych obejmuje cały proces ładowania danych do magazynu, przeprowadzanie analiz i raportowania w magazynie danych, zarządzanie danymi w magazynie danych i eksportowania danych z magazynu danych. Głębokość i szerokość składniki są często dostosowany do poziomu data_spłaty magazynu danych.


## <a name="new-to-data-warehousing"></a>Jesteś nowym użytkownikiem magazynowanie danych?
Magazyn danych jest to zbiór danych, które są ładowane z jednego lub kilku źródeł danych i służy do wykonywania zadań analizy biznesowej, takich jak raportowania i analizy danych.

Magazynów danych są określony przez kwerendy, przejrzyj większej liczby wierszy, duże zakresy danych, które może zwrócić wyniki stosunkowo dużych na potrzeby analizy i raportowania. Magazynów danych są także określony przez obciążenia danych stosunkowo dużych i małych poziomu transakcji zostanie wstawiony lub aktualizacje i usuwa.

- Magazyn danych sprawdza się najlepiej, gdy dane są przechowywane w taki sposób, aby optymalizuje zapytania, które zeskanować dużej liczby wierszy lub duże zakresy danych. Ten rodzaj skanowania sprawdza się najlepiej, gdy dane są przechowywane i przeszukiwana według kolumn, a nie według wierszy.

>[AZURE.NOTE] Indeks columnstore w pamięci, która używa magazynu kolumny, zapewnia maksymalnie 10 x kompresji zysków i 100 x zwiększenie wydajności kwerend na tradycyjne binarne drzewa raportowania i analizy kwerend. Firma Microsoft należy rozważyć, czy indeksy columnstore standardowo do przechowywania i skanowanie dużych danych w magazynie danych.

- Magazyn danych ma odmienne wymagania niż system optymalizuje transakcji online (OLTP) przetwarzania. OLTP system ma wiele Wstawianie, aktualizowanie i usuwanie operacji. Te operacje dążyć do określonych wierszy w tabeli. Tabela ma wykonywać najlepsze wyniki, gdy dane są przechowywane w sposób wiersz po wierszu. Dane można sortować i szybko za dzielenie i Podbój metodę o nazwie binarne wyszukiwania drzewo lub drzewa btree.


## <a name="data-loading"></a>Ładowanie danych
Ładowanie danych jest duży częścią obciążenia pracą magazynu danych. Firmy zazwyczaj dysponują system OLTP zajęty, która śledzi zmiany w ciągu dnia, gdy klienci generowania transakcji. Często wieczorem w oknie konserwacji, transakcje są okresowo przeniesionych lub skopiowanych do magazynu danych. Po zaimportowaniu danych w magazynie danych, analityków można przeprowadzić analizę i podejmowanie decyzji biznesowych na karcie dane.

- Zazwyczaj proces ładowania nosi ETL dla wyodrębniania, przekształcania i ładowania. Zazwyczaj należy dane przekształcania, aby była zgodna z innych danych w magazynie danych. Wcześniej firmy umożliwia wykonywanie przekształceń dedykowane serwery ETL. Teraz z takie szybkie znacznym stopniu równoległe przetwarzanie możesz można najpierw załadować dane do magazynu danych SQL, a następnie wykonaj przekształcenia. Ten proces jest nazywany wyodrębniania, załaduj i przekształcania (ELT) i staje się nowy standard obciążenia pracą magazynu danych.

> [AZURE.NOTE] Z programu SQL Server 2016, możesz teraz przeprowadzanie analiz w czasie rzeczywistym z tabelą OLTP. Nie zastępuje potrzebę magazynu danych do przechowywania i analizowania danych, ale zapewnia sposób, aby przeprowadzić analizę w czasie rzeczywistym.

### <a name="reporting-and-analysis-queries"></a>Kwerendy raportowania i analizy.
Kwerendy raportowania i analizy często są podzielone na małe, średnie i duże na podstawie wielu kryteriów, ale zwykle według czasu. W większości magazynów danych jest mieszane obciążenie szybkie pracy i kwerend. W każdym przypadku ważne jest, aby określić taka kombinacja i aby określić częstotliwość (godzina, codziennie, koniec miesiąca, koniec kwartału i tak dalej). Należy pamiętać, że obciążenie pracą kwerendy mieszanych związane z współbieżności, prowadzić do planowania dla magazynu danych pojemności pisane z wielkiej litery.

- Planowanie wydajności może być złożone zadanie dla zapytania mieszanych obciążenie pracą, zwłaszcza gdy trzeba czas wyprzedzenia długi, aby zwiększyć wydajność do magazynu danych. Magazynu danych SQL usuwa pilnych planowania pojemności można powiększanie i zmniejszanie wydajności, w dowolnym momencie, a od miejsca do magazynowania i wydajności niezależne są skonfigurowane.

### <a name="data-management"></a>Zarządzanie danymi
Zarządzanie danymi jest ważne, zwłaszcza w przypadku, gdy wiadomo, że użytkownik może brakować miejsca na dysku w najbliższej przyszłości. Magazynów danych zazwyczaj podzielić dane na znaczenie zakresy, które są przechowywane jako partycje w tabeli. Wszystkie produkty oparte na program SQL Server umożliwiają przenoszenie partycje i tabeli. Ten partition przełączania umożliwia przenoszenie starszych danych do magazynowania mniej drogich i zachować najnowszych danych dostępnych w magazyn online.

- Indeksy Columnstore obsługuje tabele podzielone na partycje. W przypadku indeksów columnstore tabele podzielone na partycje są używane do zarządzania danymi i archiwizacji. Dla tabel wiersz po wierszu partycje mają większe rolę w wydajności kwerend.  

- PolyBase jest odtwarzany ważną rolę zarządzania danymi. Przy użyciu PolyBase, masz opcję archiwizowanie starszych danych z magazynem obiektów blob Hadoop lub Azure.  Udostępnia wiele opcji, ponieważ dane są nadal w trybie online.  Może trwać dłużej do pobierania danych z Hadoop, ale zależnościami czas pobierania może być większe niż koszt miejsca do magazynowania.

### <a name="exporting-data"></a>Eksportowanie danych
Jednym ze sposobów udostępniania danych w raportach i analizy jest wysyłanie danych z magazynu danych do serwerów dedykowane do uruchamiania raportów i analizy. Serwery te są nazywane marts danych. Można na przykład wstępnie przetwarzanie danych z raportu i przekonwertowanie go z magazynu danych na wiele serwerów na świecie, aby go udostępnić ogólnie dla klientów i analityków.

- Do generowania raportów, dziennie może wypełnieniu serwerów raportowania tylko do odczytu z migawki codzienny danych. Dzięki temu większej przepustowości dla klientów, zapewniając obniżenie zapotrzebowania na zasoby obliczeń w magazynie danych. Z proporcji zabezpieczeń marts danych umożliwia zmniejszenie liczby użytkowników, którzy mają dostęp do magazynu danych.
- Analiz można skonstruować moduł analizy w magazynie danych i uruchamiana analizy magazynu danych lub wstępne przetwarzanie danych i go wyeksportować do serwera analizy w celu dalszej analizy.

## <a name="next-steps"></a>Następne kroki
Dowiedz się, po zapoznaniu się nieco magazynu danych SQL, jak szybko [utworzyć magazynu danych SQL][] i [ładowanie przykładowych danych][].

<!--Image references-->

<!--Article references-->
[ładowanie przykładowych danych]: ./sql-data-warehouse-load-sample-databases.md
[Tworzenie magazynu danych SQL]: ./sql-data-warehouse-get-started-provision.md

<!--MSDN references-->

<!--Other web references-->
