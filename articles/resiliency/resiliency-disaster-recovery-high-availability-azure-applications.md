<properties
   pageTitle="Odzyskiwanie i wysoką dostępność dla aplikacji Azure | Microsoft Azure"
   description="Omówienia techniczne i szczegółowe informacje dotyczące projektowania wniosków o wysokiej dostępności i awarii odzyskiwania aplikacji utworzonych na Microsoft Azure."
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

#<a name="disaster-recovery-and-high-availability-for-applications-built-on-microsoft-azure"></a>Odzyskiwanie i wysoką dostępność dla aplikacji utworzonych na Microsoft Azure

##<a name="introduction"></a>Wprowadzenie

W tym artykule omówiono wysokiej dostępności aplikacji działających w Azure. Ogólnej strategii wysokiej dostępności zawiera również aspektów awarii. Planowanie błędów i awarii w chmurze wymaga szybko rozpoznać błędy. Następnie implementacji strategii, która jest zgodna z uszkodzenia przestojów aplikacji. Ponadto należy wziąć pod uwagę stopień utracie danych, które aplikacja przeszkadzają nie powodując firm niekorzystne skutki, jak zostanie przywrócony.

Większość firm Załóżmy, że są gotowe do błędy tymczasowy i skalę. Jednak przed można odpowiedzieć na to pytanie dla siebie, czy firmy próba te błędy? Przetestować odzyskiwania baz danych, aby upewnić się, że masz poprawne procesy w miejscu? Prawdopodobnie nie. Jest tak, ponieważ pomyślnego odzyskiwanie zaczyna się od dużej ilości planowania i Projektując do wykonania tych procesów. Podobnie jak wiele innych wymagań współzależności funkcjonalnych, takich jak zabezpieczeń odzyskiwanie rzadko otrzymuje zaplanowania analizy i alokacji czasu, który wymaga. Ponadto większość firm nie masz budżetu rozmieszczone geograficznie regionów o pojemności zbędne. W związku z tym aplikacji krytycznych dla misji nawet często są wykluczone z planowanie odzyskiwania odzyskiwania pisane z wielkiej litery.

Platform, takich jak Azure w chmurze, Przekaż geograficznie regionów świata. Tych platform udostępniają możliwości, które obsługują dostępności i różnych scenariuszy odzyskiwania po awarii. Teraz, może być podawana wszystkich aplikacji krytyczne chmury misji odpowiedniego uwzględniania awarii sprawdzania systemu. Azure ma elastyczność i odzyskiwanie wbudowana do wielu swoich usług. Należy starannie badaniu te funkcje platformy, a dodatkowo z strategii aplikacji.

W tym artykule wymieniono kroki niezbędne architektura, należy wykonać, aby dowód awarii wdrożenia usługi Azure. Następnie można zaimplementować większego procesu biznesowego w ciągłości. Biznesplan ciągłości jest kontynuowana warunkach niekorzystne planu. Może to być błąd technologii, takich jak usługi działających lub naturalne danych, takich jak awaria Burza lub power. Elastyczność aplikacji dla awarii jest tylko niektóre większego procesu odzyskiwania po awarii, zgodnie z opisem w tym dokumencie NIST: [Przewodnik po planowaniu pod awaryjny systemów informatycznych technologii](https://www.fismacenter.com/sp800-34.pdf).

Poniższe sekcje definiowanie różnych poziomów błędów, technik na zajęcie się je i architektury, które obsługują następujące techniki. Te informacje znajdują się dane wejściowe do procesów odzyskiwania danych i procedur, aby upewnić się, że usługi strategii odzyskiwania danych działa prawidłowo i efektywne.

##<a name="characteristics-of-resilient-cloud-applications"></a>Właściwości aplikacje w chmurze mechanizm

Dobrze wysokiej aplikacji może wytrzymać błędy możliwości na poziomie taktyczne, a także przeszkadzają strategic błędy systemowe na poziomie region. Poniższe sekcje Definiowanie terminologia wymienianych w całym dokumencie powinno zawierać opis różnych aspektów usług w chmurze mechanizm.

###<a name="high-availability"></a>Wysoka dostępność

Aplikację wysokiej dostępności chmury wykonuje strategie pochłania awarii zależności, takich jak usługi zarządzane oferowanych przez platformę chmury. Pomimo możliwych błędów możliwości platformy chmury tej metody pozwala aplikacji w dalszym ciągu powodować wystąpienie oczekiwanych właściwości systemowych funkcjonalne i współzależności funkcjonalnych. To jest objęta szczegółowo w seriach wideo kanału 9 [odporność: wskazówki dotyczące mechanizm architektury chmury](https://channel9.msdn.com/Series/FailSafe).

Podczas implementowania aplikacji należy rozważyć prawdopodobieństwo awaria możliwości. Ponadto należy rozważyć wpływ posiadanych awarii na pasku aplikacji z perspektywy firm przed do nurkowania głębokich do strategii realizacji. Bez ukończenia uwzględnienie firm wpływu i prawdopodobieństwa naciśnięcie warunek ryzyka, wykonania może być drogich i potencjalnie niepotrzebne.

Należy rozważyć motoryzacyjnych odpowiednio wysoką dostępność. Nawet części jakości i inżynieria najwyższej nie uniemożliwia rzadkie błędy. Na przykład kiedy samochód uzyskuje prostym opony, samochodu jest nadal uruchomiony, ale działa z funkcjami ograniczone. Jeśli zaplanowano dla tego wystąpienia potencjalnych można jedną z tych opony zapasowe cienkie zaokrąglone do momentu wyświetlenia sklep napraw. Mimo że zapasowe opony nie zezwala na szybkości, nadal może działać pojazdu, aż Zamień opony. Podobnie usługa w chmurze plany dla potencjalną utratą możliwości zapobiec stosunkowo niewielki problem wyłączania całej aplikacji. Dotyczy to nawet wtedy, gdy usługa w chmurze należy uruchomić z funkcjami ograniczone.

Istnieje kilka kluczowych cech charakterystycznych usług w chmurze wysokiej dostępności: dostępność, skalowalność i odporność na uszkodzenia. Mimo że te właściwości są powiązane, należy opis poszczególnych i jak przyczynić się do ogólnej dostępności rozwiązania.

###<a name="availability"></a>Dostępność

Aplikacja dostępne są uwzględnione dostępność podstawowej infrastruktury i usług zależnych. Aplikacje dostępne usuwanie pojedynczych punktów awarii za pośrednictwem nadmiarowości i mechanizm projektu. Możesz rozszerzyć zakres brać pod uwagę dostępność w Azure, należy zrozumieć pojęcia skutecznych dostępność platformy. Dostępność skutecznych uważa umowy poziomu usług (SLA) każdego zależne usług i ich skumulowana wpływu na dostępność całego układu.

Dostępność systemu jest miarą procent przedział czasu, którą system będzie działać. Na przykład dostępność SLA co najmniej dwa wystąpienia roli sieci web lub pracownika w Azure jest procent 99,95 (poza 100 procent). Nie zmierzyć, funkcjonalność usług uruchomionych na tych ról i wydajność. Jednak skutecznych dostępność usługi w chmurze, również dotyczy różnych zwiększany innych usług zależne. Więcej ruchomych elementów w systemie, dłuższych przygotowań, które należy wykonać, aby zapewnić stosowanie resiliently może spełniać wymagania dostępność jej użytkowników końcowych.

Należy rozważyć następujące zwiększany usługi Azure, która korzysta z usług Azure: obliczeń, bazy danych SQL Azure i przechowywanie Azure.

|Usługa Azure|UMOWA DOTYCZĄCA POZIOMU USŁUG   |Potencjalne minut przestoje i miesiąc (30 dni)|
|:------------|:-----|:----------------------------------------:|
|Obliczenia      |99,95%|21,6 minut                              |
|Baza danych SQL |99,99%|4.3 minut                              |
|Miejsca do magazynowania      |99.90%|43,2 minut                              |

Należy zaplanować dla wszystkich usług potencjalnie Przejdź w dół o różnych porach. W tym przykładzie uproszczone łączna liczba minut na miesiąc, które aplikacja może nie działać jest 108 minut. 30-dniowych miesięcy zawiera liczbę minut 43,200. 108 minut jest procent.25 łączna liczba minut w ciągu miesiąca 30-dniową (43,200 w minutach). Umożliwia skutecznych dostępność 99.75 procent dla usług w chmurze.

Jednak za pomocą techniki dostępność opisane w tym dokumencie można poprawić ten. Na przykład podczas projektowania aplikację, aby kontynuować, gdy bazy danych SQL jest niedostępna, możesz usunąć która na podstawie równania. Może to oznaczać, że aplikacja zostanie uruchomiona z funkcjami ograniczona, więc są również wymagań brać pod uwagę. Aby uzyskać pełną listę zwiększany Azure zobacz [Umów dotyczących poziomu usług](https://azure.microsoft.com/support/legal/sla/).

###<a name="scalability"></a>Skalowalność

Skalowalność wpływa bezpośrednio na dostępność. Aplikacja, z której kończy się niepowodzeniem obciążeniu lepszą nie jest już dostępny. Skalowalna aplikacje będą mogli lepszą zapotrzebowania z spójne wyniki w systemie windows dopuszczalnego czasu. Kiedy system jest skalowalna, skale poziomo lub pionowo do zarządzania wzrost obciążenia przy zachowaniu spójne wydajności. W prostych słowach skalowanie w poziomie dodaje więcej maszyn o tym samym rozmiarze (procesora, pamięci i przepustowość) podczas pionowa podwyżki skalowania rozmiar istniejących komputerach. Azure masz pionowa opcje skalowania wybieranie różnych rozmiarach maszynowego do obliczeń. Jednak zmiany rozmiaru komputer wymaga ponownego wdrażania. Dlatego najbardziej elastyczne rozwiązania są przeznaczone dla skalowanie w poziomie. Jest szczególnie dla obliczeń, ponieważ łatwo można zwiększyć liczbę uruchomione wystąpienia dowolnego roli sieci web lub pracownika. Tych dodatkowych wystąpień uchwyt lepszą ruch przez portal Azure w sieci Web, skryptów programu PowerShell lub kodu. Bazowy na tę decyzję wzrost określonej metryki monitorowane. W tym scenariuszu wydajność użytkownika lub metryki nie mogą być zauważalne upuszczania obciążeniu. Zazwyczaj ról w sieci web i pracownik zawierają każde Państwo zewnętrznie. Dzięki temu równoważenia obciążenia elastyczne i bezpieczne obsługi o wszelkich zmianach wystąpienie liczby. Skalowanie w poziomie sprawdza się dobrze również usługi, takie jak magazyn Azure, które nie zawierają warstwowych opcji skalowania pionowego.

Chmura wdrożeń powinny być widoczne jako zbiór jednostki skali. Dzięki aplikacji elastyczną w obsługi przepustowość potrzeb użytkowników końcowych. Jednostki skali są łatwiejsze do wizualizacji na poziomie serwera sieci web i aplikacji. Jest to spowodowane Azure zawiera już węzły stateless obliczeń przy użyciu ról w sieci web i pracownika. Dodanie większych obliczeniowych skali do instalacji nie spowoduje skutki aplikacji stan zarządzania stronie ponieważ stateless skali obliczeń. Osoby odpowiedzialne za zarządzanie partycją danych jest jednostkę skali miejsca do magazynowania (strukturalne i niestrukturalne). Przykładami skali przestrzeni dyskowej partition tabeli Azure, kontener obiektów Blob platformy Azure i bazy danych SQL Azure. Nawet użycie wielu kont magazyn Azure ma bezpośredni wpływ na skalowalność aplikacji. Należy zaprojektować usługi w chmurze wysoce skalowalna przydadzą się wiele miejsca do magazynowania skali jednostek. Na przykład jeśli aplikacja używa danych relacyjnych, należy podzielić dane w kilku baz danych programu SQL. Pozwoli to magazynowania pracę modelu jednostki skali elastyczne obliczeń. Podobnie magazyn Azure umożliwia danych podziału schematów, które wymagają zamierzonego projektów stosownie do potrzeb przepustowość warstwy obliczeń. Aby uzyskać listę najważniejsze wskazówki dotyczące projektowania usług w chmurze skalowalna zobacz [Najważniejsze wskazówki dotyczące projektowania pełnej usług na Azure usług w chmurze](https://azure.microsoft.com/blog/best-practices-for-designing-large-scale-services-on-windows-azure/).

###<a name="fault-tolerance"></a>Odporność na uszkodzenia

Aplikacji powinna przyjęto założenie, że co funkcja chmury zależne mogą i będzie Przejdź w dół w pewnym momencie w czasie. Aplikację odporność na uszkodzenia wykrywa i ruchy wokół elementów nie powiodło się, aby kontynuować i zwracać poprawne wyniki w określonym przedziale czasu. Warunki błędu przejściowych odporność na uszkodzenia systemu będzie stosować zasady ponów próbę. Bardziej błędów aplikacja może wykrywania problemów, a nie na alternatywny sprzętu lub plany aż awarii. Niezawodne aplikacji można poprawnie Zarządzanie awarii jednej lub kilku części i kontynuowanie pracy poprawnie. Odporność na uszkodzenia aplikacji można użyć jednego lub więcej strategii projektu, takich jak nadmiarowości, replikacji lub ograniczone funkcje.

##<a name="disaster-recovery"></a>Odzyskiwanie

Wdrożenie chmury może przestać działać z powodu awarii systemowych usługi zależne lub podstawowej infrastruktury. Warunkach ciągłości Biznesplan uruchamia proces odzyskiwania po awarii. Proces ten obejmuje zazwyczaj zarówno personel, jak i procedury automatycznego Aby ponownie aktywować aplikacji w obszarze dostępne. W tym celu przełączania użytkowników aplikacji, danych i usług do nowej sekcji. Obejmuje także stosowania nośnika kopii zapasowej lub trwających replikacji.

Należy rozważyć odpowiednio poprzedniego, który wysoką dostępność to możliwość odzyskiwanie z prostym opony przy użyciu zapasowe w porównaniu. Natomiast odzyskiwanie obejmuje etapy po awarii samochodami, gdzie samochodu działa już. W takim przypadku jest najlepszym rozwiązaniem jest znalezienie wydajny sposób, aby zmienić samochodów, dzwoniąc usługi podróży lub znajomego. W tym scenariuszu prawdopodobnie ma być dłuższe w powrót w podróży. Istnieje więcej złożoności naprawienie i powrót do oryginalnego pojazdu. W ten sam sposób odzyskiwanie do innego obszaru jest złożone zadanie, które zwykle obejmuje niektóre przestoje i potencjalną utratą danych. Aby lepiej zrozumieć i oceny strategii odzyskiwania danych, należy zdefiniować dwóch warunków: cel czasu odzyskiwania (RTO) i odzyskiwania wskazywać cel (RPO).

###<a name="recovery-time-objective"></a>Cel czasu odzyskiwania

RTO jest maksymalna ilość czasu przydzielonego Przywracanie pełnej funkcjonalności aplikacji. Według potrzeb biznesowych, a dotyczy ważność aplikacji. Aplikacje biznesowe krytyczne wymagają RTO niski.

###<a name="recovery-point-objective"></a>Odzyskiwanie punktów cel

RPO jest oknem dopuszczalnego czasu utracone dane z powodu procesu odzyskiwania. Na przykład jeśli RPO jest godzinę, musisz całkowicie wykonywanie kopii zapasowej lub odtworzyć dane co najmniej co godzinę. Po wywoływanie aplikacji w regionie alternatywny do godziny danych może brakować danych kopii zapasowej. Przykład RTO kluczowych aplikacji docelowej dużo mniejszy RPO.

##<a name="checklist"></a>Lista kontrolna

Podsumowanie najważniejszych punktów, które zostały omówione w niniejszym artykule (i jego pokrewne artykuły o [wysokiej dostępności](./resiliency-high-availability-azure-applications.md) i [Odzyskiwanie](./resiliency-disaster-recovery-azure-applications.md) zastosowaniach Azure). Podsumowanie to będzie działać jako lista kontrolna elementów, które należy rozważyć własne dostępności i planowanie odtwarzania po awarii. Poniżej przedstawiono najważniejsze wskazówki, które są przydatne w przypadku klientów potrzebujących pomocy przy poważnie o Implementowanie pomyślnego rozwiązania.

1. Przeprowadzanie ocena ryzyka dla każdej aplikacji, ponieważ każdy mają różne wymagania. Niektóre aplikacje są bardziej krytyczne od innych osób i uzasadnia dodatkowe koszty do zaprojektować ich awarii.
1. Za pomocą tych informacji do definiowania RTO i RPO dla każdej aplikacji.
1. Projektowanie zapewnia błąd, zaczynając od architektury aplikacji.
1. Najważniejsze wskazówki dotyczące wysokiej dostępności zaimplementować podczas równoważenia koszt, złożoności i ryzyka.
1. Implementowanie plany odzyskiwania danych i procesów.
  * Należy rozważyć, czy błędy, obejmujące poziomu modułu maksymalnie awaria pełną chmury.
  * Ustanowić strategii tworzenia kopii zapasowych dla wszystkich odwołań i transakcji danych.
  * Wybierz pozycję architektura odzyskiwanie danych z wielu witryn.
1. Definiowanie konkretnych właścicieli procesów odzyskiwanie danych, automatyzacji i badań. Właściciel należy zarządzanie i właścicielem całego procesu.
1. Procesy dokumentu, dzięki czemu można je łatwo powtarzalnych. Mimo że jest jednego właściciela, wiele osób powinno być możliwe opis i wykonaj procesy w awaryjnego.
1. Przeszkolenie pracowników do realizacji procesu.
1. Użyj symulacji awarii zwykłą dla sprawdzania poprawności procesu i szkolenia.

##<a name="summary"></a>Podsumowanie

Gdy sprzętu lub aplikacji nie w Azure, techniki i strategie dotyczące zarządzania nimi różnią się od gdy wystąpi błąd w systemach lokalnego. Główny powód to jest rozwiązania cloud zwykle więcej zależności infrastruktury rozkładany Azure region i zarządzać jako oddzielne usługi. Musi dotyczyć błędy częściowe przy użyciu techniki wysokiej dostępności. Aby zarządzać więcej poważne błędy, prawdopodobnie z powodu zdarzenia awarii, użyj strategii odzyskiwania danych.

Azure wykryje i obsługuje wiele błędów, ale istnieje wiele typów błędów, które wymagają strategii specyficzne dla aplikacji. Aktywnie należy przygotować i zarządzanie nimi awarie aplikacji, usług i danych.

Podczas tworzenia aplikacji dostępności i planu odzyskiwania danych, rozważ konsekwencji firm awarii aplikacji. Definiowanie procesów, zasad i procedur, aby przywrócić systemów krytycznych po jakiejś awarii czas, planowanie i zatwierdzenia. I po utworzeniu planów, nie można zatrzymać. Należy regularnie analizować, testowanie i stale ulepszać planów na podstawie portfela aplikacji, potrzeb biznesowych i dostępnej technologii. Azure zawiera nowe funkcje i zgłasza wyzwania tworzenia niezawodne aplikacje, które wytrzymać błędy.

##<a name="additional-resources"></a>Dodatkowe zasoby

[Wysoka dostępność dla aplikacji utworzonych na Microsoft Azure](resiliency-high-availability-azure-applications.md)

[Odzyskiwanie dla aplikacji utworzonych na Microsoft Azure](resiliency-disaster-recovery-azure-applications.md)

[Wytyczne techniczne elastyczność Azure](resiliency-technical-guidance.md)

[Omówienie: W chmurze firmy ciągłości i bazy danych odzyskiwanie z bazy danych SQL](../sql-database/sql-database-business-continuity.md)

[Wysoka dostępność i awarii odzyskiwania dla programu SQL Server w maszyn wirtualnych Azure](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md)

[Odporność: Wskazówki dotyczące architektury mechanizm chmury](https://channel9.msdn.com/Series/FailSafe)

[Najważniejsze wskazówki dotyczące projektowania na dużą skalę usług na usług w chmurze Azure](https://azure.microsoft.com/blog/best-practices-for-designing-large-scale-services-on-windows-azure/)

##<a name="next-steps"></a>Następne kroki

Ten artykuł jest częścią serii artykułów dotyczyła odzyskiwanie i wysoką dostępność dla aplikacji Azure. Następny artykuł z tej serii jest [Wysoka dostępność dla aplikacji utworzonych na platformy Microsoft Azure](resiliency-high-availability-azure-applications.md).
