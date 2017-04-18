<properties 
   pageTitle="Projektowanie rozwiązań chmury awarii przy użyciu Geo Replikacja bazy danych SQL | Microsoft Azure"
   description="Dowiedz się, jak zaprojektować rozwiązania cloud awarii, wybierając pozycję deseniu prawo pracy awaryjnej."
   services="sql-database"
   documentationCenter="" 
   authors="anosov1960" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA" 
   ms.date="07/16/2016"
   ms.author="sashan"/>

# <a name="disaster-recovery-strategies-for-applications-using-sql-database-elastic-pool"></a>Strategii odzyskiwania danych dla aplikacji za pomocą puli elastyczne bazy danych SQL 

Z upływem zapewne wiesz, że usług w chmurze nie są niezawodne i losowych zdarzeń można i stanie. Baza danych SQL udostępnia kilka możliwości zapewnienie ciągłości aplikacji wystąpieniu tych przypadków. [Elastyczne pul](sql-database-elastic-pool.md) i autonomicznego bazy danych obsługują tego samego rodzaju zastosowanie funkcji. W tym artykule opisano kilka strategii DR dla pul elastyczne korzystać z tych funkcji ciągłości firm bazy danych SQL.

Na potrzeby tego artykułu użyjemy kanonicznych deseniu model władz akredytacji bezpieczeństwa w aplikacji:

<i>Nowoczesna chmurowych aplikacji przepisy jeden SQL baza danych sieci web dla każdego użytkownika końcowego. Niezależny dostawca oprogramowania jest wielu klientów i w związku z tym używa wielu baz danych, nazywany dzierżawy baz danych. Ponieważ baz danych dzierżawy zwykle desenie nieoczekiwane działanie, niezależny dostawca oprogramowania używa puli elastyczne dokonać bazy danych kosztów bardzo przewidywalne na dłuższy czas. Pulę elastyczne również ułatwia zarządzanie wydajności, gdy wzrósł aktywności użytkownika. Oprócz baz danych dzierżawy aplikacja używa kilku baz danych do zarządzania profilami użytkowników, zabezpieczeń, zbieranie upodobania itp. Dostępność poszczególnych dzierżaw nie ma wpływu na dostępność aplikacji jako całość. Jednak dostępności i wydajności Zarządzanie bazami danych jest krytyczne dla funkcji aplikacji i w przypadku zarządzania baz danych trybu offline całej aplikacji jest w trybie offline.</i>  

W pozostałych papieru omówimy strategii DR obejmujących zakres scenariuszy z aplikacji poufnych uruchamiania koszt te wymagania rygorystyczne dostępności.  

## <a name="scenario-1-cost-sensitive-startup"></a>Scenariusz 1. Koszt uruchamiania poufnych

<i>Jestem firm uruchamiania i jest bardzo kosztów poufne.  Chcę, aby uprościć rozmieszczanie i zarządzanie aplikacji i chcę mają ograniczony Umowa dotycząca poziomu usług dla poszczególnych odbiorców. Ale chcę zapewnić stosowanie jako całość nigdy nie jest w trybie offline.</i>

Spełnienie wymogu uproszczenia, należy wdrożyć wszystkie dzierżawy bazy danych w jednej puli elastyczne w regionie Azure wyboru i wdrażanie zarządzania baz danych jako autonomicznego replikowane geo baz danych. Odzyskiwanie dzierżaw korzystać geo przywracanie, który zawiera bez dodatkowych opłat. W celu zapewnienia dostępności Zarządzanie bazami danych, należy je, geo replikować do innego obszaru (krok 1). Koszt trwających konfiguracji odzyskiwanie danych, w tym scenariuszu jest równa całkowity koszt pomocniczej baz danych. Ta konfiguracja została przedstawiona na diagramie dalej.

![Rysunek 1](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-1.png)

W przypadku awarii w regionie podstawowego kroków odzyskiwania w celu dostosowania aplikacji w tryb online są zilustrowane następnego diagramu.

- Od razu pracy awaryjnej zarządzania bazy danych (2) do obszaru DR. 
- Zmienianie parametrów połączenia aplikacji, aby wskazywały DR region. Wszystkich nowych kont i baz danych dzierżawy będą tworzone w regionie DR. Obecnych klientów widzą dane o jego tymczasowo niedostępny.
- Tworzenie puli elastyczne w takiej samej konfiguracji jako oryginalny Pula (3). 
- Przywracanie geo umożliwia utworzenie kopii dzierżawy baz danych (4). Można rozważyć powodujące poszczególnych przywraca przez połączenia przez użytkownika końcowego lub użyć niektórych inny schemat priorytet określonej aplikacji.

W tym momencie aplikacja jest powrocie do trybu online w regionie DR, ale niektórzy klienci będzie powodować opóźnienia dostępu do danych.

![Rysunek 2](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-2.png)

Jeśli awaria tymczasowy, jest możliwe, że podstawowy obszar zostaną przywrócone przy Azure przed wszystkie przywraca są wykonane w regionie DR. W takim przypadku możesz należy dodać akompaniament przenoszenie wniosek z powrotem do obszaru podstawowego. Proces zostanie czynności przedstawione na diagramie dalej.
 
- Anuluj wszystkie pozostałe Przywróć geo żądania.   
- Awaryjnego zarządzania baz danych do obszaru podstawowego (5). Uwaga: Po odzyskiwania regionu stare kolory podstawowe mają automatycznie stają się pomocnicze. Teraz ich przełączy role ponownie. 
- Zmienianie parametrów połączenia aplikacji, aby wskazywały podstawowego region. Teraz wszystkich nowych kont i baz danych dzierżawy będą tworzone w regionie podstawowego. Niektóre obecnych klientów widzą dane o jego tymczasowo niedostępny.   
- Ustawianie wszystkich baz danych w puli DR tylko do odczytu, aby upewnić się, że nie można ich modyfikować w regionie DR (6). 
- Dla każdej bazy danych w puli DR, która została zmieniona od czasu odzyskiwania Zmień lub usuń właściwych baz danych w puli podstawowego (7). 
- Kopiowanie zaktualizowanych baz danych z puli DR do podstawowej puli (8). 
- Usuwanie puli DR (9)

W tym momencie aplikacja będzie online w obszarze podstawowy z wszystkich dzierżawy baz danych dostępnych w puli podstawowego.

![Rysunek 3](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-3.png)

Kluczowe **korzyści** tej strategii jest niska trwających koszt warstwa odporność. Kopie zapasowe są pobierane automatycznie przez usługę bazy danych SQL nie zmiana aplikacji i bez dodatkowych opłat.  Koszt powstaje tylko wtedy, gdy zostaną przywrócone elastyczne baz danych. **Zależność** to, że wykonano odzyskiwania wszystkich dzierżawy baz danych potrwa znaczące. Zależy całkowita liczba przywraca rozpocznie w regionie DR i ogólny rozmiar bazy danych dzierżawy. Nawet wtedy, gdy względem innych Wyznaczanie priorytetów przywraca kilka dzierżaw, będzie można konkurencji z wszystkich przywraca, które są inicjowane w tym samym regionie, jak usługa rozstrzygają i ograniczenia w celu zminimalizowania ogólny wpływ na obecnych klientów baz danych. Ponadto odzyskiwania baz danych dzierżawy, nie można uruchomić dopiero po utworzeniu nowej puli elastyczne w regionie DR.

## <a name="scenario-2-mature-application-with-tiered-service"></a>Scenariusz 2. Dojrzałe aplikacji z usługą warstwowych 

<i>Jestem dojrzałych aplikacji władz akredytacji bezpieczeństwa z oferowanych usług warstwowych i różnych zwiększany dla wersji próbnej klientów i opłaty klientów. W przypadku wersji próbnej klientów mam zajmowała możliwie. Wersji próbnej klienci mogą korzystać przestoje, ale chcę zmniejszyć jej prawdopodobieństwa. W przypadku rzeczywistych klientów dowolnego przestoje jest ryzyka lotów. Tak, chcę upewnić się, że opłaty klientów zawsze będą mogli uzyskiwać dostępu do danych.</i> 

Do obsługi tego scenariusza, należy oddzielić wersji próbnej dzierżaw od płatnej dzierżaw przez umieszczenie ich w osobnych pule elastyczne. Klienci wersji próbnej woli dolnym eDTU dla dzierżawy i dolnym Umowa dotycząca poziomu usług z dłuższy czas odzyskiwania. Płatności użytkownicy będą się w puli z wyższymi eDTU dla dzierżawy i nowszy Umowa dotycząca poziomu usług. Aby zagwarantować najniższych czas przywrócenia, baz danych dzierżawy rzeczywistych klientów powinny być replikowane geo. Ta konfiguracja została przedstawiona na diagramie dalej. 

![Rysunek 4](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-4.png)

Tak jak w pierwszym scenariuszu zarządzania baz danych jest bardzo aktywny Aby używać autonomicznego geo zreplikowanej bazy danych dla niego (1). Dzięki przewidywalne wydajności dla nowej subskrypcji klienta, aktualizacje profilu i innych operacji zarządzania. Region, w którym znajdują się kolory podstawowe części baz danych zarządzania będzie podstawowego region i region, w którym znajdują się pomocnicze z baz danych zarządzania będzie DR region.

Baz danych dzierżawy rzeczywistych klientów będą mieć aktywne baz danych w zestawie "płatnej" obsługi administracyjnej w regionie podstawowego. Należy zapewnić puli pomocniczą o takiej samej nazwie w regionie DR. Każdy dzierżawy może być replikowane geo do puli pomocniczą (2). Spowoduje to włączenie szybkiego odzyskiwania wszystkie dzierżawy bazy danych przy użyciu pracy awaryjnej. 

W przypadku awarii w obszarze podstawowy, kroków odzyskiwania w celu dostosowania aplikacji w trybie online są przedstawione na diagramie następnego:

![Rysunek 5](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-5.png)

- Natychmiast przestać działać na zarządzanie baz danych do regionu DR (3).
- Zmienianie parametrów połączenia aplikacji, aby wskazywały DR region. Teraz wszystkich nowych kont i baz danych dzierżawy będą tworzone w regionie DR. Obecnych klientów wersji próbnej zostanie wyświetlony dane o jego tymczasowo niedostępny.
- Pracy awaryjnej baz danych dzierżawy płatnej puli w regionie DR natychmiast przywracania ich dostępności (4). W związku z tym przełączeniu jest szybkie metadanych zmiany poziomu można rozważyć optymalizację miejsce, w którym poszczególne praca awaryjna muszą być uruchamiane na żądanie przez połączenia użytkownika końcowego. 
- Jeśli rozmiar eDTU pomocniczej puli został niższej niż podstawowych, ponieważ pomocniczej baz danych wymagane tylko możliwości przetwarzania dzienników zmian podczas były pomocnicze, należy natychmiast zwiększyć wydajność puli teraz, aby zezwalały na pełny obciążenie wszystkich dzierżaw (5). 
- Tworzenie nowego zestawu elastyczną z tej samej nazwie i tej samej konfiguracji w regionie DR dla klientów wersji próbnej baz danych (6). 
- Po utworzeniu puli klientów wersji próbnej umożliwia przywracanie geo Przywracanie bazy danych pojedyncze dzierżawy próbnej do nowej puli (7). Można rozważyć powodujące poszczególnych przywraca przez połączenia przez użytkownika końcowego lub użyć niektórych inny schemat priorytet określonej aplikacji.

W tym momencie aplikacja jest powrocie do trybu online w regionie DR. Wszystkie rzeczywistych klientów mają dostęp do swoich danych, podczas próbnej klientów będzie powodować opóźnienia dostępu do danych.

Podstawowy obszar jest odzyskać przez Azure *po* przywróceniu aplikacji w regionie DR można kontynuować uruchamianie aplikacji w danym regionie lub możesz zdecydować, czy nie podstawowego region. W przypadku podstawowego region odzyskanego *przed* awaryjnego procesu zostanie zakończone, należy rozważyć w przypadku braku wstecz od razu. Awarii podejmie kroki przedstawione w następnej diagramu: 
 
![Rysunek 6](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-6.png)

- Anuluj wszystkie pozostałe Przywróć geo żądania.   
- Awaryjnego zarządzania baz danych (8). Po odzyskaniu regionu starego podstawowego były automatycznie stają się pomocniczej. Teraz staje się podstawowych ponownie.  
- Pracy awaryjnej płatnej dzierżawy bazy danych (9). Podobnie po odzyskiwania regionu stare kolory podstawowe automatycznie stają się pomocnicze. Teraz staną się kolory podstawowe ponownie. 
- Ustawianie przywróceniu wersji próbnej baz danych, które zostały zastąpione w regionie DR tylko do odczytu (10).
- Dla każdej bazy danych z puli klientów wersji próbnej DR zmienione w stosunku do odzyskiwania Zmień lub Usuń odpowiednią bazę danych w puli podstawowego wersji próbnej klientów (11). 
- Kopiowanie zaktualizowanych baz danych z puli DR do podstawowej puli (12). 
- Usuwanie puli DR (13) 

> [AZURE.NOTE] Operacja pracy awaryjnej jest asynchroniczna. Aby zminimalizować czas odtwarzania należy wykonać polecenia pracy awaryjnej bazy dzierżawy partiami co najmniej 20 baz danych. 

Kluczowe **korzyści** tej strategii jest najwyższe Umowa dotycząca poziomu usług dla klientów płatności. Gwarantuje również, że nowe wersje próbne są odblokowane zaraz po utworzeniu puli DR wersji próbnej. **Zależność** to, że to ta konfiguracja wzrośnie całkowity koszt baz danych dzierżawy przez koszty pomocniczej puli DR dla klientów płatnej. Ponadto jeśli pomocniczej Pula ma inny rozmiar, rzeczywistych klientów będzie wydajności dolnym po przełączeniu dopóki nie zakończy się uaktualnienie puli w regionie DR. 

## <a name="scenario-3-geographically-distributed-application-with-tiered-service"></a>Scenariusz 3. Odpowiednio rozmieszczone geograficznie aplikacji z usługą warstwowych

<i>Mam dojrzałych aplikacji władz akredytacji bezpieczeństwa z oferowanych usług warstwowych. Chcę oferowanie bardzo rygorystyczne SLA płatnej klientów i zmniejszyć ryzyko wpływu wystąpieniu dostawie, ponieważ nawet krótkie przerwy mogą powodować niezadowolenie klienta. Niezwykle ważne, że rzeczywistych klientów można zawsze dostęp do danych jest. Wersje próbne są bezpłatne i Umowa dotycząca poziomu usług nie ma na liście podczas okresu próbnego.</i> 

Aby zrealizować ten scenariusz, należy użyć trzech oddzielnych pul elastyczne. Dwa pul równa rozmiar z wysokiej eDTUs na bazę danych został zastrzeżony w dwóch różnych regionów do przechowywania baz danych dzierżawy klientów płatnej. Puli trzecia zawierające wersji próbnej dzierżaw woli dolnym eDTUs na bazę danych i być przygotowana w jednym z dwóch regionów.

Zagwarantowanie najniższe odzyskiwania czasie dostawie dzierżawy rzeczywistych klientów baz danych powinny być replikowane geo z 50% podstawowego baz danych w każdym z dwóch regionów. Podobnie każdego regionu woli 50% pomocniczej baz danych. W ten sposób Jeśli obszar jest w trybie offline tylko 50% baz danych klientów płatnych może mieć negatywny wpływ na i wymaga pracy awaryjnej. Inne bazy danych będzie pozostaną niezmienione. Ta konfiguracja została przedstawiona na poniższym rysunku:

![Rysunek 4](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-7.png)

Tak jak w poprzednich scenariuszach zarządzania baz danych jest bardzo aktywny, należy je skonfigurować jako baz autonomicznego replikowane geo danych (1). Dzięki wydajności przewidywalne nowe subskrypcje klienta, aktualizacje profilu i innych operacji zarządzania. Region A będzie podstawowego regionu zarządzania baz danych i region B będzie używany do odzyskiwania zarządzania baz danych.

Baz danych dzierżawy rzeczywistych klientów będzie również replikowane geo, ale kolory podstawowe i pomocnicze dzielone region A i region B (2). Podstawowy baz danych dzierżawy wpływ awarii w ten sposób można przełączanie awaryjne do innego regionu i stanie się dostępna. Nie będzie można wcale wpływu na drugą baz danych dzierżawy. 

Na następnym diagramie przedstawiono kroki odzyskiwania, aby wykonać w przypadku awarii występuje w regionie odpowiedzi.

![Rysunek 5](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-8.png)

- Natychmiast przestać działać na zarządzanie bazy danych do regionu B (3).
- Zmienianie parametrów połączenia aplikacji, aby wskazywały zarządzania baz danych w regionie B. modyfikowanie zarządzania baz danych, aby upewnić się, w regionie B zostaną utworzone nowe konta i baz danych dzierżawy i istniejące bazy danych dzierżawy znajdują się tam także. Obecnych klientów wersji próbnej zostanie wyświetlony dane o jego tymczasowo niedostępny.
- Pracy awaryjnej płatnej dzierżawy baz danych do puli 2 w regionie B, aby od razu przywrócić ich dostępności (4). W związku z tym przełączeniu jest szybkie metadanych zmiany poziomu można rozważyć optymalizację miejsce, w którym poszczególne praca awaryjna muszą być uruchamiane na żądanie przez połączenia użytkownika końcowego. 
- Od teraz pulę 2 zawiera tylko podstawowy baz danych, których całkowita obciążenie pracą w puli wzrośnie, należy natychmiast zwiększyć rozmiar eDTU (5). 
- Tworzenie nowego zestawu elastyczną z tej samej nazwie i tej samej konfiguracji w regionie B dla klientów wersji próbnej baz danych (6). 
- Po utworzeniu puli umożliwia przywracanie geo Przywracanie bazy danych pojedyncze dzierżawy próbnej do puli (7). Można rozważyć powodujące poszczególnych przywraca przez połączenia przez użytkownika końcowego lub użyć niektórych inny schemat priorytet określonej aplikacji.


> [AZURE.NOTE] Operacja pracy awaryjnej jest asynchroniczna. Aby zminimalizować czas odtwarzania należy wykonać polecenia pracy awaryjnej bazy dzierżawy partiami co najmniej 20 baz danych. 

W tym momencie aplikacja jest powrocie do trybu online w regionie B. Wszystkich odbiorców płatności mają dostęp do swoich danych, podczas próbnej klientów będzie powodować opóźnienia dostępu do danych.

Po region A jest odzyskać należy zdecydować, czy chcesz używać region B dla klientów wersji próbnej lub aby za pomocą puli wersji próbnej klientów w regionie odpowiedzi. Jedno kryterium może być % baz danych dzierżawy próbnej zmodyfikowane odzyskiwania. Bez względu na tę decyzję będzie konieczne ponowne saldo dzierżaw płatnego między dwoma zestawami. Następny diagram przedstawia proces, gdy baz danych dzierżawy próbnej awarii region odpowiedzi.  
 
![Rysunek 6](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-9.png)

- Anulowanie wszystkich żądań oczekujących Przywróć geo do skorzystania z wersji próbnej puli DR.   
- Awaryjnego zarządzania bazy danych (8). Po ich odzyskaniu regionu stare podstawowego były automatycznie stał się pomocniczej. Teraz staje się podstawowych ponownie.  
- Wybierz pozycję które płatnej dzierżawy bazy danych nie powiedzie się powrót do puli 1 i Inicjowanie przełączanie awaryjne do ich pomocnicze (9). Po odzyskaniu regionu wszystkie bazy danych w puli 1 została automatycznie pomocnicze. Teraz 50% ich staną się kolory podstawowe ponownie. 
- Zmniejszanie rozmiaru puli 2 do oryginalnej eDTU (10).
- Ustawianie wszystkich przywrócić wersji próbnej baz danych w regionie B tylko do odczytu (11).
- Dla każdej bazy danych w wersji próbnej puli DR, która została zmieniona od czasu odzyskiwania Zmień lub Usuń odpowiednią bazę danych w wersji próbnej podstawowej puli (12). 
- Kopiowanie zaktualizowanych baz danych z puli DR do podstawowej puli (13). 
- Usuwanie puli DR (14) 

Kluczowe **korzyści** tej strategii są:

- Obsługuje on najbardziej rygorystyczne Umowa dotycząca poziomu usług dla klientów płatności ponieważ zapewnia, że awarii nie mogą mieć wpływ na więcej niż 50% baz danych dzierżawy. 
- Gwarantuje nowe wersje próbne są odblokowane, jak dziennik puli DR zostanie utworzony w trakcie odzyskiwania. 
- Umożliwia bardziej efektywne wykorzystanie możliwości puli jako 50% pomocniczej baz danych w puli 1 i 2 puli zapewniona jest mniej aktywny, a następnie podstawowego bazy danych.

Są głównym **korzystnych rozwiązań** :

- Operacje OBSŁUGIWAŁ przed baz danych zarządzania mają mniejsze opóźnienia dla użytkowników końcowych połączony z obszaru A niż użytkownicy końcowi połączony z regionu B, jak zostanie wykonana przed podstawowych baz danych zarządzania.
- Wymaga bardziej złożone projektu bazy danych zarządzania. Na przykład każdy rekord dzierżawy musi mieć znacznik lokalizacji, który należy zmienić podczas awaryjnego i powrotu.  
- Niższa wydajność niż zazwyczaj może być rzeczywistych klientów, dopóki nie zakończy się uaktualnienie puli w regionie B. 

## <a name="summary"></a>Podsumowanie

W tym artykule omówiono strategii odzyskiwania danych dla poziomu bazy danych używana przez aplikację wielu dzierżawy model władz akredytacji bezpieczeństwa. Strategii, która zostanie wybrana opcja powinny być oparte na potrzeby aplikacji, takiej jak modelu biznesowego, Umowa dotycząca poziomu usług chcesz oferowanie klientom, budżet ograniczenia itp. Każdy opisane strategii określone korzyści i zależność może podejmowanie decyzji o informacje. Ponadto konkretnej aplikacji prawdopodobnie będzie zawierać inne składniki Azure. Dlatego należy zapoznać się wskazówkami zawartymi ich firm ciągłości i dodać akompaniament odzyskiwania warstwy bazy danych z nimi. Aby dowiedzieć się więcej o zarządzaniu odzyskiwania aplikacji baz danych platformy Azure, skorzystaj z [Projektowanie rozwiązania cloud awarii](./sql-database-designing-cloud-solutions-for-disaster-recovery.md) .  


## <a name="next-steps"></a>Następne kroki

- Aby uzyskać informacje o bazy danych SQL Azure automatycznego wykonywania kopii zapasowych, zobacz [Baza danych SQL automatycznego wykonywania kopii zapasowych](sql-database-automated-backups.md)
- Przegląd ciągłości działalności i scenariuszy zobacz [Omówienie ciągłości firm](sql-database-business-continuity.md)
- Aby dowiedzieć się o używaniu kopie zapasowe automatycznego odzyskiwania, zobacz [Przywracanie bazy danych z kopii zapasowych zainicjowane usługi](sql-database-recovery-using-backups.md)
- Aby poznać opcje odzyskiwania szybciej, zobacz [Aktywne Geo replikacji](sql-database-geo-replication-overview.md)  
- Aby dowiedzieć się o używaniu kopie zapasowe automatycznego archiwizowania, zobacz [Kopiowanie bazy danych](sql-database-copy.md)
