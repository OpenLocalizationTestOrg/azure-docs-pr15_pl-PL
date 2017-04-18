<properties
   pageTitle="Odzyskiwanie aplikacji Azure | Microsoft Azure"
   description="Omówienie kwestii technicznych i uzyskać szczegółowe informacje o projektowaniu aplikacji podczas odzyskiwania systemu Microsoft Azure."
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

#<a name="disaster-recovery-for-applications-built-on-microsoft-azure"></a>Odzyskiwanie dla aplikacji utworzonych na Microsoft Azure

Informacje o zarządzaniu tymczasowy błąd jest wysoka dostępność, odzyskiwanie (DR) jest o losowych utracie funkcjonalności aplikacji. Na przykład można rozważyć tego scenariusza, w której regionie awarii. W tym przypadku należy planu uruchomienie aplikacji lub uzyskać dostęp do danych poza obszar Azure. Wykonanie tego planu obejmuje osoby, procesy i obsługi aplikacji, które umożliwiają systemowi funkcji. Właścicieli firm i technologii, którzy Definiowanie systemu operacyjnego tryb awarii także określić poziom funkcjonalności usługi podczas awarii. Poziom funkcjonalności może potrwać kilka formularzy: całkowicie niedostępne, częściowo dostępny (ograniczona funkcjonalność lub opóźnione przetwarzanie) lub w pełni dostępna.

##<a name="azure-disaster-recovery-features"></a>Funkcje odzyskiwania systemu Azure

Podobnie jak w przypadku zagadnienia dostępność Azure zawiera [wskazówki techniczne elastyczność](./resiliency-technical-guidance.md) przeznaczoną do obsługi awarii. Istnieje także relacji między niektóre funkcje dostępności Azure i odtwarzania po awarii. Na przykład zarządzania ról w domenach błędów zwiększa dostępność aplikacji. Bez tego zarządzania awarii sprzętu nieobsługiwanego staje się scenariusz "awarii". Dlatego prawidłowego stosowania dostępności i strategii ważną część sprawdzające awarii aplikacji. Jednak w tym artykule wykracza poza problemy z dostępnością ogólne na zdarzenia awarii bardziej poważne (i rzadkich).

##<a name="multiple-datacenter-regions"></a>Wielu regionów centrum danych

Azure zachowuje centrach danych w wielu regionach na całym świecie. Infrastruktura ta obsługuje kilka scenariuszy odzyskiwania danych, takich jak system geo replikacji Azure przestrzeni dyskowej w regionach pomocniczą. Oznacza również, że można łatwo i niedrogiego wdrożyć usługi w chmurze wielu lokalizacji na całym świecie. Porównaj to z kosztów i trudności podczas uruchamiania własne centrach danych w wielu regionów. Rozmieszczanie danych i usług do wielu regionów chroni aplikacji z głównych dostawie w jednym regionie.

##<a name="azure-traffic-manager"></a>Azure Menedżer ruchu

W przypadku wystąpienia awarii określonego regionu można przekierowywać ruch do usług lub we wdrożeniach w innym regionie. Możesz wykonać ten routingu ręcznie, ale jest bardziej efektywne używanie zautomatyzowany proces. Azure Menedżer ruchu jest przeznaczony dla tego zadania. Można go automatycznie zarządzać awaryjnego przeniesienia ruchu użytkownika do innego obszaru, w przypadku, gdy podstawowy regionu kończy się niepowodzeniem. Ponieważ zarządzanie ruchem jest ważną część ogólnej strategii, należy zrozumieć podstawy pracy z Menedżera ruch.

Na poniższym diagramie użytkownicy łączyć do adresu URL, który został określony menedżer ruchu (__http://myATMURL.trafficmanager.net__) i że zestawienia adresy URL witryn rzeczywista (__http://app1URL.cloudapp.net__ i __http://app2URL.cloudapp.net__). W zależności od konfiguracji kryteriów po użytkownikom rozsyłania, użytkownicy będą wysyłane do odpowiedniej witryny rzeczywista po decyduje o zasady. Ustawienia zasad są karuzeli, wydajności ani przełączenie awaryjne. Dla tego artykułu będziemy danych z wybraną opcją pracy awaryjnej.

![Rozsyłanie za pośrednictwem Azure Menedżer ruchu](./media/resiliency-disaster-recovery-azure-applications/routing-using-azure-traffic-manager.png)

Podczas konfiguracji Menedżer ruchu, podaj nowy prefiks Menedżer ruchu DNS. To prefiks adresu URL, który będzie zapewniają użytkownikom dostęp do tej usługi. Menedżer ruchu abstracts teraz o jeden poziom w górę, a nie na poziomie region równoważenia obciążenia. Menedżer ruchu DNS mapowany CNAME dla wszystkich wdrożeniach zarządzanych.

W Menedżerze ruch Określ priorytet wdrożenia, które użytkownicy będą kierowane do, gdy wystąpi błąd. Menedżer ruchu monitoruje punkty końcowe wdrożeń i notatki, w przypadku niepowodzenia podstawowego wdrożenia. W błąd Menedżer ruchu analizuje liście priorytetem wdrożeniach i kieruje użytkowników do następnego na liście.

Mimo że Menedżer ruchu zdecyduje, gdzie można znaleźć w trybie awaryjnym, możesz określić, czy Twoja domena pracy awaryjnej jest nieaktywni lub aktywny, gdy nie jesteś w trybie pracy awaryjnej. Funkcja ma nic robić przy użyciu Menedżera ruch Azure. Menedżer ruchu wykrywa błąd w podstawowej witryny i przesuwany nad do witryny pracy awaryjnej. Menedżer ruchu przesuwany nad niezależnie od tego, czy danej witryny jest aktualnie obsługujący użytkowników, czy nie.

Aby uzyskać więcej informacji o tym, jak działa Menedżer ruchu Azure odwołują się do:

 * [Omówienie Menedżera ruchu](../traffic-manager/traffic-manager-overview.md)
 * [Konfigurowanie routingu metody pracy awaryjnej](../traffic-manager/traffic-manager-configure-failover-routing-method.md)

##<a name="azure-disaster-scenarios"></a>Scenariusze awarii Azure

Poniższych sekcjach omówiono kilka różnych typów danych scenariuszy. Zakłócenia w obsłudze całego regionu nie są tylko przyczyny błędów całej aplikacji. Niska błędy projektu lub Administracja również może prowadzić do awarii. Należy rozważyć możliwe przyczyny awarii podczas projektowania i testowania fazy planu odzyskiwania. Dobry plan korzysta z funkcji Azure i rozszerzają je z strategii specyficzne dla aplikacji. Wybrane odpowiedzi jest spowodowana znaczenie aplikacji, celem punktu odzyskiwania (RPO) i cel czasu odzyskiwania (RTO).

###<a name="application-failure"></a>Błąd aplikacji

Azure Menedżer ruchu automatycznie obsługuje błędy powstałe w wyniku źródłowych oprogramowania sprzętu lub systemu operacyjnego hosta maszyny wirtualnej. Azure tworzy nowe wystąpienie roli na serwerze działa i dodaje ją do równoważenia obciążenia obrotu. Jeśli liczba wystąpień roli jest większa niż jeden, Azure przenoszony przetwarzanie innych uruchomione wystąpienia roli podczas zamieniania uszkodzonego węzła.

Występują błędy aplikacji poważne, wykonywanych niezależnie od wszystkie błędy sprzętu lub systemu operacyjnego. Aplikacja może zakończyć się niepowodzeniem z powodu losowych wyjątki spowodowane przez nieprawidłowe logiczny lub problemów integralności danych. Tak, aby system monitorowania można wykrywać warunków uszkodzenia i powiadomienia o tym, administrator aplikacji, musi zawierać wystarczającą ilość telemetrycznego do kodu. Administrator, który ma pełną wiedzę procesów odzyskiwania danych może mieć decyzji wywołania awaryjnego procesu. Możesz też administrator po prostu zaakceptować awarii dostępność, aby usunąć błędy krytyczne.

###<a name="data-corruption"></a>Uszkodzenia danych

Azure automatycznie zapisuje dane magazynu Azure i bazy danych SQL Azure trzy razy nadmiarowo w domenach różnych błędów w tym samym regionie. Jeśli używasz geo replikacji, dane są przechowywane dodatkowe trzykrotnie w innym regionie. Jednak użytkownicy lub aplikacja uszkodzi dane w podstawowej kopii, dane szybko replikuje dane pozostałe kopie. Niestety efektem trzy kopie uszkodzone dane.

Aby zarządzać potencjalne uszkodzenia danych, masz dwie opcje. Najpierw można zarządzać niestandardowych strategii wykonywania kopii zapasowych. Kopie zapasowe można przechowywać w Azure lub lokalnych, zależnie od potrzeb biznesowych i przepisów zarządzania. Innym rozwiązaniem jest korzystać z nowych opcji przywracania w chwili odzyskiwania bazy danych SQL. Aby uzyskać więcej informacji zobacz sekcję strategii [danych awarii](#data-strategies-for-disaster-recovery).

###<a name="network-outage"></a>Awaria sieci

Gdy części Azure sieci są niedostępne, nie można uzyskać dostęp do aplikacji lub danych. Jeśli przynajmniej jedno wystąpienie roli są niedostępne z powodu problemów z siecią, Azure używa pozostałe wystąpienia dostępnych aplikacji. Jeśli aplikacji nie ma dostępu wraz z danymi ze względu na awaria sieci Azure, potencjalnie uruchomieniem w trybie ograniczone lokalnie przy użyciu zapisujące dane. Należy zaprojektować strategii odzyskiwania danych do uruchamiania w trybie ograniczone w aplikacji. W niektórych aplikacjach to może nie być praktycznych.

Innym rozwiązaniem jest do przechowywania danych w innej lokalizacji, dopóki nie zostanie przywrócony łączności. W przypadku trybu awaryjnego nie opcję, pozostałe opcje są przestoje aplikacji lub przełączanie awaryjne do naprzemiennych region. Projekt aplikacji działa w trybie ograniczone jest tyle decyzji biznesowych jako technicznych. Jest to omówione w sekcji na [obniżonej wydajności funkcji aplikacji](#degraded-application-functionality).

###<a name="failure-of-a-dependent-service"></a>Błąd usługi zależne

Azure udostępnia wiele usług, które mogą wystąpić okresowe przestoje. Jako przykład, warto rozważyć [Azure Redis w pamięci podręcznej](https://azure.microsoft.com/services/cache/) . Ta usługa wielu dzierżawy zapewnia buforowania do aplikacji. Należy rozważyć, co się dzieje w aplikacji, jeśli usługa zależne jest niedostępna. Na różne sposoby w tym scenariuszu jest podobny do tego scenariusza awarii sieci. Jednak ustalając każdej usługi niezależne powoduje poprawa potencjalne do planu ogólnego.

Azure Redis pamięci podręcznej znajdują się pamięci podręcznej do aplikacji w programie rozmieszczenia usługi cloud zalety odzyskiwania po awarii. Najpierw teraz działa usługa na rolach lokalnych wdrożenia. W związku z tym możesz lepiej monitorowania i zarządzanie statusem w pamięci podręcznej w ramach procesów ogólnego zarządzania dla usług w chmurze. Ten rodzaj pamięci podręcznej udostępnia również nowe funkcje. Jeden z nowych funkcji jest szybkiej pamięci podręcznej danych. Pomaga to zachowanie zapisujące dane, jeśli jeden węzeł kończy się niepowodzeniem przy zachowaniu kopie na innych węzłach.

Zauważ, że szybkiej zmniejszy przepustowość i zwiększa opóźnienie ze względu na aktualizowanie pomocniczej kopii na zapis. Zwiększa dwukrotnie także ilości pamięci, która jest używana dla każdego elementu, dlatego planowanie w tym. Określone w przykładzie pokazano, że każda zależne usługa może być możliwości, które usprawnią działanie swojej dostępności i odporności na błędy losowych.

Z każdego zależne usługi należy zrozumieć zależności zakłócenia usługi. W tym przykładzie buforowania może być możliwe uzyskiwać dostęp do danych bezpośrednio z bazy danych, dopóki Przywracanie pamięci podręcznej. Będzie trybu awaryjnego pod względem wydajności, ale chcesz uzyskać pełną funkcjonalność w zakresie danych.

###<a name="region-wide-service-disruption"></a>Obszar usługi zakłócenia w pracy

Błędy poprzedniego przede wszystkim zostały błędy, które mogą być zarządzane w ramach tego samego regionu Azure. Jednak należy również przygotować dla jest zakłócenia usługi całego regionu. W przypadku wystąpienia awarii usługi region na poziomie lokalnie zbędne kopie danych nie są dostępne. Po włączeniu replikacji geo istnieją trzy dodatkowe kopie obiektów blob i tabel w innym regionie. Jeśli Microsoft deklaruje region utracone, ponownie mapuje Azure wszystkie wpisy DNS do regionu replikowane geo.

>[AZURE.NOTE] Należy pamiętać, że nie masz żadnej kontrolki przez ten proces i wystąpi się tylko w przypadku awarii usługi region na poziomie. Z tego powodu w przypadku korzystania z innych aplikacji strategii tworzenia kopii zapasowych uzyskanie najwyższego poziomu dostępności. Aby uzyskać więcej informacji zobacz sekcję strategii [danych awarii](#data-strategies-for-disaster-recovery).

###<a name="azure-wide-service-disruption"></a>Usługi Azure zakłócenia w pracy

W planowanie odzyskiwania, należy rozważyć cały zakres możliwych awarii. Jedną z najbardziej poważne zakłócenia w obsłudze związane z wszystkich regionów Azure jednocześnie. Podobnie jak w przypadku innych zakłócenia w obsłudze, możesz określić, że ryzyko wystąpienia tymczasowych problemów będzie mieć w takim przypadku. Zakłócenia w obsłudze szerokie, obejmujące regionów powinny być znacznie rzadkich niż zakłócenia w obsłudze odizolowanych, obejmujące usługi zależne lub pojedynczy regionów.

Jednak w niektórych aplikacjach kluczowych można zdecydować, że musi być planu wykonywania kopii zapasowych w tym scenariuszu także. Planowanie wydarzenia mogą zawierać awarii usługi w [chmurze alternatywny](#alternative-cloud) lub [lokalnego hybrydowych i rozwiązanie w chmurze](#hybrid-on-premises-and-cloud-solution).

###<a name="degraded-application-functionality"></a>Funkcje aplikacji ograniczone

Dobrze zaprojektowany aplikacji zwykle używa zbioru modułów, które komunikowania się między sobą mimo wykonania desenie luźno powiązanych wymiany informacji. Aplikacja DR przyjazne wymaga podział zadań na poziomie modułu. To, aby uniemożliwić zakłócenia w pracy usługi zależne wyłączania całej aplikacji. Na przykład można rozważyć aplikacji sieci web handlowego firmy y. Następujących modułów mogą stanowić aplikacji:

 * __Katalog produktów__ umożliwia użytkownikom przeglądanie produktów.
 * __Koszyka__ umożliwia dodawanie i usuwanie produktów w ich koszyka.
 * __Stan zamówienia__ jest wyświetlany stan wysyłki zamówień użytkownika.
 * __Ponowne przesłanie zamówienia__ Kończenie znajdujących się w sesji zakupów przez złożenie zamówienia z płatnością.
 * __Kolejność przetwarzania__ sprawdza kolejność integralności danych i sprawdza dostępności ilość.

Gdy awarii zależne od modułu w tej aplikacji, jak moduł działać, dopóki odzyskuje tej części? Dobrze wysokiej systemu wprowadza ograniczenia izolacji za pośrednictwem podział zadań zarówno w czasie projektowania, jak i w czasie rzeczywistym. Umożliwia klasyfikowanie każdy błąd jako odzyskania i odzyskania. Błędy odzyskania nie zajmie dół moduł, ale można ograniczyć odwracalny błąd za pośrednictwem rozwiązania alternatywne. Opisane w sekcji wysokiej dostępności, możesz ukryć niektóre problemy z użytkowników, obsługi błędów i wykonywanie akcji alternatywny. Podczas bardziej działaniu usługi aplikacja może być całkowicie niedostępne. Trzecia opcja jest jednak kontynuować obsługę użytkowników w trybie ograniczone.

Na przykład bazy danych do obsługi zleceń odbywa się w dół, module przetwarzania kolejności utraci możliwość przetwarzania transakcji sprzedaży. W zależności od architektury może to być twardym lub niemożliwe ponowne przesłanie zamówienia i zamówienia przetwarzania części aplikacji, aby kontynuować. Jeśli aplikacja nie jest przeznaczony do obsługi tego scenariusza, całej aplikacji może przejść do trybu offline.

W tym samym scenariuszu jest jednak możliwe, że dane produktu jest przechowywany w innej lokalizacji. W takim przypadku moduł wykazu nadal można używać do wyświetlania produktów. W trybie ograniczone aplikacja będzie nadal są dostępne dla użytkowników dostępnych funkcji, takich jak wyświetlanie katalogu produktów. Inne części aplikacji, jednak jest niedostępny, takich jak kolejnością lub zapasów kwerend.

Inna odmiana centra trybu awaryjnego wydajności zamiast funkcji. Na przykład można rozważyć scenariusza, w którym katalogu produktów jest buforowana za pośrednictwem Azure Redis w pamięci podręcznej. Jeśli buforowanie jest niedostępny, aplikacja może przejść bezpośrednio do magazynu serwera pobieranie informacji o wykazie produktu. Ale dostęp może być mniejsza niż wersja buforowana. Z tego powodu wydajność aplikacji obniża się do momentu pełni przywróceniu usługi buforowania.

Określanie, ile aplikacji będzie nadal jest funkcja w trybu awaryjnego decyzji biznesowych i technicznych decyzji. Aplikacja muszą również zdecydować, jak informowanie użytkowników o tymczasowych problemów. W tym przykładzie aplikacja może zezwala na wyświetlanie produktów i nawet dodając je do koszyka. Jednak gdy użytkownik będzie próbował dokonać zakupu, aplikacja powiadamia użytkownika, że modułu Sprzedaż jest tymczasowo niedostępny. Nie jest idealnym rozwiązaniem klienta, ale uniemożliwia awarii usługi całej aplikacji.

##<a name="data-strategies-for-disaster-recovery"></a>Strategie dane dotyczące awarii

Poprawnie obsługi danych jest obszarem najtrudniejsze uzyskiwania prawo w dowolnym planu odzyskiwania danych. Przywracanie danych jest również częścią procesu odzyskiwania, które zwykle trwa najwięcej czasu. Różne opcje w trybach obniżenie wydajności powodują problemach trudne do odzyskania danych z powodu niepomyślnego i spójność po awarii.

Jednym z czynników jest potrzebna do przywracania lub zachować kopię danych aplikacji. Te dane zostaną wykorzystane do celów transakcji w witrynie pomocniczej i odwołania. Ustawienie w lokalnym wymaga planowania proces drogich i długiej w celu wdrożenia strategii odzyskiwania danych regionów. Wygodne większość dostawców chmury, w tym Azure, łatwo Zezwalaj rozmieszczanie aplikacji do wielu obszarów. Te regiony są geograficznie rozsiane w taki sposób, że awarii usługi regionów powinny być bardzo rzadko. Strategia obsługi danych między różnymi regionami jest jednym z czynników uczestniczące powodzenia dowolnego planu odzyskiwania danych.

W poniższych sekcjach omówiono technik odzyskiwania danych związanych z kopie zapasowe danych, danych źródłowych i danych transakcji.

###<a name="backup-and-restore"></a>Kopia zapasowa i przywracanie

Regularnego wykonywania kopii zapasowych danych aplikacji może obsługiwać kilka scenariuszy odzyskiwania po awarii. Zasoby magazynu innego wymagają różnych technik.

Dla podstawowej, standardowe i baza danych SQL Premium poziomów można korzystać z w chwili Przywróć, aby odzyskać bazę danych. Aby uzyskać więcej informacji, zobacz [Omówienie: firm ciągłości i bazy danych odzyskiwanie z bazą danych SQL w chmurze](../sql-database/sql-database-business-continuity.md). Innym rozwiązaniem jest użycie aktywnej replikacji Geo bazy danych SQL. Zmiany w bazie danych zostanie automatycznie replikuje na pomocniczej bazy danych, w tym samym regionie Azure lub nawet w innym regionie Azure. Dzięki temu potencjalne sposobem niektóre więcej ręcznego techniki synchronizacji danych przedstawionej w tym artykule. Aby uzyskać więcej informacji, zobacz [Omówienie: SQL Replikacja bazy danych Active Geo-](../sql-database/sql-database-geo-replication-overview.md).

Można również używać bardziej ręcznego podejście do tworzenia kopii zapasowych i przywracania. Użycie polecenia Kopiuj bazy danych, aby utworzyć kopię bazy danych. Aby uzyskać kopię zapasową z transakcji spójności, należy użyć tego polecenia. Umożliwia także usługę Importuj/Eksportuj bazy danych SQL Azure. Obsługuje eksportowania baz danych PLECAK pliki, które są przechowywane w magazynie obiektów Blob platformy Azure.

Wbudowane nadmiarowości przestrzeni dyskowej Azure tworzy dwie repliki pliku kopii zapasowej w tym samym regionie. Jednak częstotliwość uruchamiania wykonywania kopii zapasowej pozwala określić usługi RPO ilości danych, które mogą zostać utracone w scenariuszach awarii. Załóżmy na przykład, czy wykonywanie kopii zapasowej w górnej części godzinę, a po awarii występuje dwie minuty przed początek godzinę. Tracisz 58 minut danych, których doszło od ostatniej kopii zapasowej. Ponadto do ochrony przed zakłócenia usługi regionu, należy skopiować pliki PLECAK do alternatywny region. Możesz wybrać opcję przywracania kopie zapasowe w regionie alternatywny. Aby uzyskać więcej informacji, zobacz [Omówienie: firm ciągłości i bazy danych odzyskiwanie z bazą danych SQL w chmurze](../sql-database/sql-database-business-continuity.md).

Do przechowywania Azure można opracowywać własne niestandardowe proces tworzenia kopii zapasowej lub użyć jednego z wielu narzędzia do tworzenia kopii zapasowych innych firm. Należy zauważyć, że większość projektów aplikacji złożoności dodatkowe miejsce, w którym zasobami magazynowania zależą od siebie. Na przykład można rozważyć z bazą danych SQL zawiera kolumnę z łączem do obiektów blob w magazynie Azure. Jeśli kopie zapasowe nie nawiązały jednocześnie, bazy danych może być wskaźnika blob, w którym nie wykonano kopii zapasowej przed awarii. Aplikacja lub planu odzyskiwania danych zaimplementować procesów obsługiwać ten niespójności po odzyskiwania.

###<a name="reference-data-pattern-for-disaster-recovery"></a>Wzorzec danych odwołania awarii

Dane źródłowe jest tylko do odczytu danych, który obsługuje funkcje aplikacji. Zwykle nie zmienia często. Mimo że kopia zapasowa i przywracanie jest jednym ze sposobów obsługę zakłócenia w obsłudze całego regionu, RTO jest stosunkowo długich. Podczas wdrażania aplikacji do obszaru pomocniczej kilka strategii poprawić RTO dla danych źródłowych.

Ponieważ odwołanie zmian w danych rzadko, możesz poprawić RTO przy zachowaniu trwałą kopię danych źródłowych w regionie pomocniczą. Dzięki temu czas wymagany do przywracania kopii zapasowych w wypadku awarii. Aby spełnić wymagania odzyskiwania danych regionów, należy wdrożyć aplikację i danych źródłowych razem w wielu regionów. Jak wspomniano w [wzorzec danych odwołania wysokiej dostępności](./resiliency-high-availability-azure-applications.md#reference-data-pattern-for-high-availability), należy wdrożyć dane źródłowe do roli, zewnętrzne miejsca do magazynowania lub kombinację obu.

Model wdrażania danych odwołania węzłów obliczeń niejawnie spełnia wymagania odzyskiwania po awarii. Odwołanie rozmieszczania danych do bazy danych SQL wymaga wdrażanie kopię danych źródłowych dla każdego regionu. Tym samym strategii dotyczy magazyn Azure. Należy wdrożyć kopię wszelkie dane źródłowe, które są przechowywane w magazynie Azure w regionach głównego i pomocniczego.

![Publikacji danych odwołania do regionów podstawowych i pomocniczych](./media/resiliency-disaster-recovery-azure-applications/reference-data-publication-to-both-primary-and-secondary-regions.png)

Należy zaimplementować własnych procedur wykonywania kopii zapasowych specyficzne dla aplikacji dla wszystkich danych, w tym danych źródłowych. Replikowane Geo kopii między różnymi regionami są używane tylko w działaniu usługi region na poziomie. Aby zapobiec rozszerzonego przestoje, wdrożyć krytyczne części danych aplikacji pomocniczej region. Na przykład Ta topologia Zobacz [modelu pasywne aktywne](#active-passive).

###<a name="transactional-data-pattern-for-disaster-recovery"></a>Wzorzec danych transakcji awarii

Wdrożenie strategii tryb w pełni funkcjonalny danych wymaga asynchroniczne replikacji transakcji danych w regionie pomocniczą. Praktyczne uruchomienia systemu windows, w ramach których mogą występować replikacji określi cech RPO aplikacji. Może być nadal odzyskać dane, które zostały utracone z podstawowego regionu podczas okna replikacji. Można również łączyć się z obszaru pomocniczej później.

W poniższych przykładach architektura zapewniają kilka sugestii na różne sposoby obsługi danych transakcji w scenariuszu pracy awaryjnej. Należy Zauważ, że w tych przykładach nie są pełne. Na przykład pośrednie lokalizację, takich jak kolejki mogą zostać zamienione na bazy danych SQL Azure. Same kolejki może być magazyn Azure lub Bus usługi Azure kolejkach (zobacz [kolejki Azure i kolejki Bus usługi — porównanie i porównanie oznaczonymi](../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md)). Miejsc magazynowania serwera również może być zróżnicowany, takie jak tabele Azure zamiast bazy danych SQL. Ponadto role pracownik może zostać wstawiony jako pośrednictwa różne kroki. Najważniejsze jest nie w celu emulacji pól tych architektury dokładnie, ale brać pod uwagę różne rozwiązania alternatywne w odzyskiwania transakcji danych i moduły pokrewnych.

####<a name="replication-of-transactional-data-in-preparation-for-disaster-recovery"></a>Replikacja danych transakcji w przygotowanie do awarii

Należy rozważyć, czy aplikacji, która używa magazynu Azure kolejek do przechowywania danych transakcji. Dzięki temu role pracownika do przetwarzania transakcji danych do bazy danych serwera w architekturze rozłączone. W tym celu transakcji formularz tymczasowe buforowania, jeśli zewnętrzną ról wymaga natychmiastowej kwerendy danych. W zależności od poziomu uszkodzenia utratą danych można replikować kolejek, bazy danych lub wszystkie zasoby miejsca do magazynowania. Z tylko Replikacja bazy danych jeśli awarii podstawowego region, możesz nadal odzyskać dane w kolejce po podstawowy obszar wróci.

Na poniższym diagramie przedstawiono architekturę, w którym bazy danych serwera jest synchronizowane między różnymi regionami.

![Replikacja danych transakcji w przygotowanie do awarii](./media/resiliency-disaster-recovery-azure-applications/replicate-transactional-data-in-preparation-for-disaster-recovery.png)

Największym wyzwaniem do wykonania tej architektury jest strategii replikacji między regionami. Usługa synchronizacji danych SQL Azure umożliwia replikacji na tego typu. Jednak usługa jest nadal w podglądzie i nie jest zalecane dla środowisku produkcyjnym. Aby uzyskać więcej informacji, zobacz [Omówienie: firm ciągłości i bazy danych odzyskiwanie z bazą danych SQL w chmurze](../sql-database/sql-database-business-continuity.md). W przypadku aplikacji produkcji możesz inwestycji w innej firmy, lub utworzyć logiki replikacji w kodzie. W zależności od architektury replikacji może być dwukierunkowa, który jest także bardziej złożone.

Język, który może powodować wyświetlanie implementacji potencjalne za pomocą kolejki pośrednie w poprzednim przykładzie. Role pracownika, która przetwarza dane do miejsca docelowego miejsca do magazynowania ostateczne może zmienić zarówno w obszaru podstawowy i pomocniczy region. Są one uproszczony zadania i wykonane wskazówki dotyczące kodu replikacji wykracza poza zakres tego artykułu. Istotne jest wiele czasu i testowania należy skoncentrować się na sposób replikacji danych do obszaru pomocniczą. Dodatkowe przetwarzania i testowania ułatwiają, upewnij się, że procesy awaryjnego i odzyskiwania poprawnie obsługiwać niespójności danych ani transakcje zduplikowane.

>[AZURE.NOTE] Większość tego artykułu omówiono na platformie usług (PaaS). Dodatkowe opcje replikacji i dostępność dla aplikacji hybrydowych posługiwać się maszyn wirtualnych Azure. Te aplikacje hybrydowych infrastruktury za pomocą usług (IaaS) do obsługi programu SQL Server w środowisku maszyn wirtualnych systemu platformy Azure. Dzięki temu metod tradycyjnych dostępności w programie SQL Server, takich jak grupy dostępności (AlwaysOn) lub wysyłki dziennika. Technik, takich jak (AlwaysOn), działają tylko między wystąpieniami programu SQL Server w środowisku lokalnym i Azure maszyn wirtualnych. Aby uzyskać więcej informacji zobacz [wysokiej dostępności i odzyskiwanie w przypadku programu SQL Server w maszyn wirtualnych Azure](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).

####<a name="degraded-application-mode-for-transaction-capture"></a>Tryb aplikacji ograniczone do przechwytywania transakcji

Należy rozważyć drugiego architekturę, która działa w trybie ograniczone. Aplikacja pomocniczej region dezaktywuje wszystkich funkcji, takich jak raportowanie analizy biznesowej (BI) lub Opróżnianie kolejki. Przyjmuje najważniejsze rodzaje transakcji przepływy pracy określonych wymagań. System rejestruje transakcje i zapisuje je do kolejek. System może opóźnić przetwarzanie danych podczas etapu początkowego awarii usługi. Jeśli w oknie oczekiwany czas ponownie uaktywnić systemie podstawowy obszar ról pracowników w regionie podstawowego można opróżnić kolejek. Dzięki temu przeznaczonego do scalenia bazy danych. Jeśli ulegnie awarii usługi podstawowy obszar poza okno dopuszczalna, aplikacja może rozpocząć przetwarzania kolejek.

W tym scenariuszu bazy danych na pomocniczej zawiera przyrostowe transakcji dane, które muszą być scalane po uaktywnieniu podstawowy. Na poniższym diagramie przedstawiono strategię tymczasowo przechowywania danych transakcji, dopóki nie zostanie przywrócony podstawowy region.

![Tryb aplikacji ograniczone do przechwytywania transakcji](./media/resiliency-disaster-recovery-azure-applications/degraded-application-mode-for-transaction-capture.png)

Aby poznać techniki zarządzania danymi mechanizm Azure aplikacji, zobacz [odporność: wskazówki dotyczące mechanizm architektury chmury](https://channel9.msdn.com/Series/FailSafe).

##<a name="deployment-topologies-for-disaster-recovery"></a>Wdrożenie topologii awarii

Należy przygotować kluczowych aplikacji możliwość zakłócenia usługi region. W tym celu włączenia strategii wdrażania regionów do planowania operacyjnego.

Wdrożenia regionów może obejmować procesów IT pro publikowanie danych aplikacji i odwołań do obszaru pomocniczej po awarii. Jeśli aplikacja wymaga błyskawiczne pracy awaryjnej, procesu wdrażania może obejmować Instalatora aktywne/pasywne lub aktywny/aktywny. Ten typ wdrażania ma istniejących wystąpień aplikacji działa w regionie alternatywny. Narzędzia routingu takich jak Menedżer ruchu Azure udostępnia równoważenia obciążenia usługi na poziomie DNS. Jego wykrywanie zakłócenia w obsłudze i kierowanie użytkowników do różnych regionów w razie potrzeby.

Część pomyślnego odzyskiwanie Azure jest Projektując tego odzyskiwania do rozwiązania od początku. Chmura znajdują się dodatkowe opcje odzyskiwania z błędy podczas danych, które nie są dostępne w tradycyjnych dostawcy hostingu. W szczególności użytkownik może dynamicznie i szybko przydzielić zasoby do innego obszaru. W związku z tym nie płatność wiele zasobów bezczynne gdy czekasz, występuje błąd.

Poniższych sekcjach omówiono topologii rozmieszczania awarii. Zazwyczaj jest zależnościami lepszą koszt lub złożoność dostępności dodatkowe.

###<a name="single-region-deployment"></a>Wdrożenie pojedynczego regionu

Wdrożenie pojedynczego regionu nie jest naprawdę topologii odzyskiwanie danych, ale jest przeznaczona do kontrast za pomocą innych architekturach. Pojedynczy region wdrożeń są wspólne dla aplikacji platformy Azure. Jednak tego typu wdrożenia nie jest poważne zawodnik dla planu odzyskiwania danych.

Poniższy diagram przedstawia aplikacja działająca w jednym regionie Azure. Azure Menedżer ruchu i korzystanie z domen błędów i uaktualnianie zwiększyć dostępność aplikacji w regionie.

![Wdrożenie pojedynczego regionu](./media/resiliency-disaster-recovery-azure-applications/single-region-deployment.png)

W tym miejscu jest widoczna, że baza danych znajduje się pojedynczego punktu awarii. Mimo że Azure replikuje dane domen różnych błędów do wewnętrznego replik, wszystkie występuje w tym samym regionie. Aplikacja nie wytrzymać awarii losowych. Jeśli obszaru ulegnie uszkodzeniu, wszystkich domen błędów Przejdź w dół — w tym wszystkie wystąpienia usługi i zasoby miejsca do magazynowania.

Wszystkich najmniej ważnych aplikacji należy opracować plan wdrażanie aplikacji u wielu regionów. Należy rozważyć RTO i kosztów, ograniczeń, uwzględniając, które wdrażanie topologii korzystać.

Spójrzmy teraz w określonych wzorców do obsługi pracy awaryjnej wielu różnych regionów. W tych przykładach wszystkich Opisz procesu za pomocą dwóch regionów.

###<a name="redeployment-to-a-secondary-azure-region"></a>Ponownego rozmieszczania pomocniczej regionu Azure

W strukturze ponownego rozmieszczania do obszaru pomocniczej tylko podstawowy obszar zawiera aplikacje i bazy danych działają. Pomocnicza region nie jest skonfigurowana do automatycznego przełączania awaryjnego. Dlatego w przypadku wystąpienia awarii musi pokrętła w górę wszystkie części usługi w nowym regionie. Ta opcja uwzględnia przekazywania usługi w chmurze Azure wdrażanie usługi w chmurze, przywracanie danych i zmienianie DNS, aby przekierowywać ruch.

Mimo że to najczęściej przystępnej opcji regionów, ma najgorszego cechy RTO. W tym modelu są przechowywane kopie zapasowe pakiet i bazy danych usług obsługiwanych lokalnie lub w przypadku magazyn obiektów Blob platformy Azure obszaru pomocniczej. Należy jednak wdrażanie nową usługę i przywrócenie danych usuniętych przed zostało ono przywrócone operacji. Nawet wtedy, gdy w pełni zautomatyzować transfer danych z magazynu kopii zapasowej, Obracająca się nowe środowisko bazy danych zajmuje dużo czasu. Przenoszenie danych z kopii zapasowej miejsca z pustą bazą danych na pomocniczej region jest najbardziej drogich częścią proces przywracania. Musisz to zrobić, jednak aby wyświetlić nowej bazy danych do stanu działania, ponieważ nie jest on replikowane.

Najlepszym rozwiązaniem jest przechowywanie pakietów usług w magazynie obiektów Blob w regionie pomocniczą. Dzięki temu przekazać pakiet do Azure, czyli, co się dzieje po wdrożeniu z komputera rozwoju lokalnego. Można szybko wdrożyć pakietów usług do nowej usługi w chmurze z magazynem obiektów Blob za pomocą skryptów programu PowerShell.

Ta opcja jest praktyczne tylko w przypadku niekrytyczne aplikacjom przeszkadzają RTO wysoki. Na przykład może to działać dla aplikacji, która może być niedostępna przez kilka godzin, ale powinna być uruchomiona ponownie w ciągu 24 godzin.

![Ponownego rozmieszczania pomocniczej regionu Azure](./media/resiliency-disaster-recovery-azure-applications/redeploy-to-a-secondary-azure-region.png)

###<a name="active-passive"></a>Aktywne pasywne

Wzorzec pasywne aktywne jest odpowiadającą preferować w pełni wiele firm. Ten wzorzec przekazuje ulepszenia RTO ze stosunkowo niewielką wzrost kosztów na wzorcu ponownego rozmieszczania.
W tym scenariuszu jest ponownie podstawowy i pomocniczy region Azure. Cały ruch prowadzi do aktywnego wdrożenia podstawowego regionu. Region pomocniczej lepiej przygotować się na odzyskiwanie ponieważ baza danych jest uruchomiona w obu regionów. Ponadto mechanizmu synchronizacji znajduje się w miejscu między nimi. Tej metody wstrzymania może obejmować dwie odmiany: podejście tylko do bazy danych lub pełne rozmieszczenie w regionie pomocniczą.

####<a name="database-only"></a>Tylko bazy danych

Pierwsza deseniu pasywne aktywne tylko podstawowy obszar ma chmury wdrożonej aplikacji usługi. Jednak w odróżnieniu od wzorca ponownego rozmieszczania dla obu regionów są synchronizowane z zawartości bazy danych. (Aby uzyskać więcej informacji, zobacz sekcję na [wzorzec danych transakcji awarii](#transactional-data-pattern-for-disaster-recovery)). W przypadku wystąpienia awarii ma mniej wymagań aktywacji. Uruchom aplikację w regionie pomocniczej, zmienić parametry połączenia z bazą danych i zmienić wpisy DNS, tak aby przekierowywać ruch.

Przykład deseniu ponownego rozmieszczania należy zostały już zapisane pakietów usług w magazynie obiektów Blob platformy Azure w regionie pomocniczej celu szybszego wdrażania. W przeciwieństwie do deseniu ponownego rozmieszczania nie spowodować większość ogólnych, która wymaga operacja przywracania bazy danych. Baza danych jest gotowa i działa. Zapisuje dużo czasu, to przystępnej deseniu DR. Należy również najpopularniejszych deseniu DR.

![Aktywne — pasywne tylko bazy danych](./media/resiliency-disaster-recovery-azure-applications/active-passive-database-only.png)

####<a name="full-replica"></a>Pełna replice

Druga deseniu pasywne aktywne zarówno podstawowy obszar i pomocniczej region, musi pełne wdrożenie. Wdrożenie to obejmuje usług w chmurze i zsynchronizowane bazy danych. Jednak tylko podstawowy obszar aktywnie obsługuje żądania sieciowe do użytkowników. Region pomocniczej staje się aktywny tylko wtedy, gdy podstawowy obszar ulegnie awarii usługi. W takim przypadku wszystkie nowe żądania sieci trasy do obszaru pomocniczą. Azure Menedżer ruchu można zarządzać tej pracy awaryjnej automatycznie.

Awaria wystąpiła szybciej niż odmian tylko do bazy danych, ponieważ są już zainstalowane usługi. Ten wzorzec zawiera bardzo niskie RTO. Region pomocniczej pracy awaryjnej musi być gotowa do wysłania natychmiast po awarii obszaru podstawowego.

Wraz z szybsze czas reakcji tego wzorca przewaga wstępnie przydzielanie i wdrażanie usługi tworzenia kopii zapasowych. Nie musisz martwić region nie ma miejsce, aby przydzielić nowych wystąpień w danych. Jest to ważne, jeśli obszar Azure pomocniczej zbliża wydajność. Nie jest gwarantowana (Umowa dotycząca poziomu usług) aby od razu będą mogli wdrażanie liczbę nowych usług w chmurze w dowolnym regionie.

Najlepszy czas odpowiedzi w tym modelu mają podobne skali (liczba wystąpień roli) w regionach głównego i pomocniczego. Niezależnie od zalet płacisz za wystąpienia nieużywane obliczeń jest kosztów i nie może to być najbardziej rozsądne wybór finansowych. Z tego powodu jest najczęściej przy użyciu wersji nieco skalowany szczegółów z usługami w chmurze na pomocniczym region. Następnie można szybko się nie powieść, nad i skalowania pomocniczej wdrożenia, w razie potrzeby. Należy zautomatyzować awaryjnego procesu, dzięki czemu po podstawowy obszar jest niedostępny, uaktywnić kolejne wystąpienia, w zależności od Załaduj. To może obejmować wykorzystanie mechanizm autoscaling, takie jak [Skala maszyn wirtualnych zestawów](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md).

Na poniższym diagramie przedstawiono modelu, gdzie regionów głównego i pomocniczego zawierają usługi w chmurze w pełni wdrożony w deseniu pasywne aktywne.

![Pasywne aktywne, pełny replice](./media/resiliency-disaster-recovery-azure-applications/active-passive-full-replica.png)

###<a name="active-active"></a>Aktywny aktywny

Chwili możesz jest prawdopodobnie ważniejszymi rozwoju wzorców: zmniejszenie RTO zwiększa kosztów i złożoności. Rozwiązanie aktywny aktywny podziały rzeczywiście tym tendencji w odniesieniu do kosztów.

Wzór aktywny aktywny usługami w chmurze i bazy danych są w pełni wdrożona w obu regionów. W przeciwieństwie do modelu pasywne aktywne dla obu regionów odbierać danych użytkownika. Ta opcja powoduje najszybszą czas odzyskiwania. Do obsługi części obciążenia w poszczególnych regionach już skalowania usług. DNS jest już włączone używać obszaru pomocniczą. Istnieje dodatkowe złożoność w wyborze sposobu kierowania użytkowników do odpowiedni region. Planowanie karuzeli może być możliwe. Istnieje więcej prawdopodobieństwo, że niektórzy użytkownicy będą używać określonego regionu, w którym znajduje się podstawowy kopię swoich danych.

W przypadku awaryjnym przeniesieniu po prostu wyłącz DNS do podstawowego region. Cały ruch to kieruje do obszaru pomocniczej.

Nawet w modelu istnieją pewne różnice. Na przykład na poniższym diagramie przedstawiono modelu miejsce, w którym podstawowy obszar właścicielem wzorca kopii bazy danych. Usług w chmurze w obu regionów zapisać podstawowego bazy danych. Wdrożenie pomocniczą można przeczytać z podstawowego lub zreplikowanej bazy danych. Replikacja w tym przykładzie się dzieje w jednym ze sposobów.

![Aktywny aktywny](./media/resiliency-disaster-recovery-azure-applications/active-active.png)

Istnieje Wadą podwyższonego do architektury aktywny aktywny na powyższym diagramie. Drugiego regionu musi dostęp do bazy danych w regionie pierwszy, ponieważ znajduje się główna kopia. Wydajność znacznie pomija wyłączanie przypadku uzyskiwania dostępu do danych z poza obszarem. W przypadku połączeń między regionu bazy danych należy rozważyć różnego rodzaju tworzeniu partii strategii w celu zwiększenia wydajności telefonów. Aby uzyskać więcej informacji zobacz [jak za pomocą tworzeniu partii, aby zwiększyć wydajność aplikacji bazy danych SQL](../sql-database/sql-database-use-batching-to-improve-performance.md).

Alternatywny architektura może obejmować każdego regionu dostęp bezpośrednio do własnej bazy danych. W tym modelu różnego rodzaju dwukierunkowy replikacji jest wymagane do zsynchronizowania baz danych w każdym regionie.

W strukturze aktywny aktywny nie może być konieczne dowolną liczbę wystąpień na podstawowy obszar, tak jak w strukturze pasywne aktywne. Jeśli masz 10 wystąpieniach na region podstawowego w architekturze pasywne aktywne, może być konieczne tylko 5 w każdym z regionów w architekturze aktywny aktywny. Dla obu regionów teraz udostępniać Załaduj. Może to być oszczędności na wzór pasywne aktywne, jeśli zachowasz ciepłych wstrzymania w stronie biernej region o 10 wystąpieniach oczekiwanie do przełączania awaryjnego.

Uznasz, że dopóki nie zostanie przywrócony podstawowy region, pomocniczej region może pojawić się szybkiego wzrost nowych użytkowników. Jeśli istnieją 10 000 użytkowników na poszczególnych serwerach, gdy podstawowy obszar ulegnie awarii usługi, pomocniczej region nieoczekiwanie ma do obsługi 20 000 użytkowników. Reguły na pomocniczym regionu monitorowania musi wykryć tego wzrostu i podwójny wystąpienia w regionie pomocniczą. Aby uzyskać więcej informacji zobacz sekcję na [Wykrywanie awarii](#failure-detection).

##<a name="hybrid-on-premises-and-cloud-solution"></a>Hybrydowe lokalnego i rozwiązanie w chmurze

Jeden dodatkowych strategii awarii ma zaprojektować aplikacji hybrydowe, która jest uruchamiany w lokalnej i w chmurze. W zależności od aplikacji podstawowy obszar może być dowolnej lokalizacji. Należy rozważyć, czy poprzedniego architektury i wyobrazić region podstawowym lub pomocniczym jako lokalizacji lokalnego.

W tych architektury hybrydowych istnieją niektóre problemy. Najpierw większość tego artykułu ma skierowana desenie architektura PaaS. Typowe aplikacji PaaS platformy Azure nie korzysta z określonych Azure konstrukcji, takich jak role, usług w chmurze i Menedżer ruchu. Aby utworzyć lokalnego rozwiązania dla tego typu aplikacji PaaS wymaga architekturę znacznie różni się. To może nie być możliwe z zarządzania lub widzenia kosztów.

Jednak rozwiązania hybrydowego awarii ma mniej wyzwania dla architektury tradycyjny, które po prostu przeniesione do chmury. Dotyczy to architektury, które używają IaaS. Aplikacje IaaS używać maszyn wirtualnych w chmurze, która może zawierać odpowiedników bezpośredni lokalnego. Za pomocą wirtualnych sieci nawiązywania połączenia z zasobami sieci lokalnej maszyny w chmurze. Spowoduje to otwarcie kilka możliwości, które nie są dostępne z aplikacji PaaS. Program SQL Server można na przykład korzystać z rozwiązań odzyskiwania danych, takich jak grupy dostępności (AlwaysOn) i odzwierciedlające bazy danych. Aby uzyskać szczegółowe informacje zobacz [wysokiej dostępności i odzyskiwanie w przypadku programu SQL Server w środowisku maszyn wirtualnych Azure](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).

Rozwiązania IaaS zapewniają również łatwiejsze ścieżki dla aplikacji lokalnej Azure za pomocą opcji pracy awaryjnej. W pełni funkcjonalny aplikacji może być w regionie istniejącego lokalnego. Jednak co zrobić, jeśli nie masz zasoby, aby zachować geograficznie regionu do przełączania awaryjnego? Można zdecydować, za pomocą maszyn wirtualnych i wirtualnych sieci uzyskać działanie aplikacji platformy Azure. W takim przypadku Definiowanie procesów, które synchronizowanie danych w chmurze. Następnie Azure wdrożenia staje się pomocniczej region, aby użyć do przełączania awaryjnego. Podstawowy obszar pozostaje aplikacji lokalnej. Aby uzyskać więcej informacji na temat architektury IaaS i możliwości, zapoznaj się z [dokumentacją maszyn wirtualnych](https://azure.microsoft.com/documentation/services/virtual-machines/).

##<a name="alternative-cloud"></a>Alternatywny chmury

Istnieją sytuacje, w której nawet niezawodności Microsoft Cloud nie spełnia reguł wewnętrznych zgodności lub zasady, które wymagają Twojej organizacji. Nawet najlepsze przygotowanie i projektu do wykonania kopii zapasowej systemów podczas awarii wypada słabo, jeśli istnieje zakłócenia globalne usługi dostawcy usługi cloud.

Warto przeprowadzić porównanie wymagań dotyczących dostępności z kosztów i złożoności zwiększona dostępność. Wykonywanie analizy ryzyka i zdefiniuj RTO i RPO dotyczących rozwiązania. Jeśli aplikacji nie można tolerować dowolnego przestoje, zrozumiałe może należy rozważyć użycie innego rozwiązania cloud. Chyba że cały Internet awarii, inne rozwiązanie chmury nadal może być dostępna w przypadku Azure staje się globalnie niedostępne.

Podobnie jak w przypadku scenariuszy hybrydowego wdrożenia pracy awaryjnej w poprzedniej architektury odzyskiwania danych mogą istnieć w innym rozwiązaniem chmury. Alternatywny chmury DR witryn stosuje się tylko w przypadku rozwiązań, których RTO umożliwia bardzo mały, jeśli istnieją, przestoje. Należy zauważyć, że rozwiązania korzystającego z witryną DR poza Azure wymaga więcej pracy w celu konfigurowania, opracowywania, wdrażania i obsługi. Jest również trudniejsze do wdrożenia najlepsze rozwiązania w architekturze między chmury. Mimo że platformy chmury podobne pojęcia wysokiego poziomu, interfejsy API i architektury różnią się.

Jeśli zdecydujesz się na dzielenie programu DR między różnych platform, czy zrozumiałe Aby zaprojektować warstwy abstrakcji w projektowaniu rozwiązań. Jeśli to zrobisz, nie będzie wymagało opracowanie i obsługa dwie wersje tej samej aplikacji dla chmury różnych platform w przypadku awarii. Jako z tego scenariusza hybrydowych stosowania maszyn wirtualnych Azure lub usługa Azure kontener może się w takich przypadkach niż w przypadku projektów PaaS specyficzne dla chmury.

##<a name="automation"></a>Automatyzacji

Niektóre wzorców, których wspomniano tylko wymagają szybkie aktywacji wdrożeń trybu offline, a także przywrócenie określonych części systemu. Automatyzacji lub skryptów, obsługuje możliwość aktywować zasoby na żądanie i szybko wdrażanie rozwiązań. W tym artykule związane z DR automatyzacji jest postaci przy użyciu [Programu PowerShell Azure](https://msdn.microsoft.com/library/azure/jj156055.aspx), ale [Interfejsu API usługi REST zarządzania usługi](https://msdn.microsoft.com/library/azure/ee460799.aspx) jest również odpowiednią opcję.

Opracowywanie skryptów ułatwia zarządzanie części DR, która nie obsługuje przezroczysty Azure. Ma to zaletą produkcji spójne wyniki każdym, która minimalizuje ryzyko błędu człowieka. Również o wstępnie zdefiniowanych skryptów DR skraca czas odbudowanie system i elementy składowe pośrodku awarii. Nie chcesz spróbować ręcznie sprawdzić, jak przywrócić witryny, gdy będzie ona pieniędzy w dół i przegrywająca co minutę.

Po utworzeniu skrypty należy je przetestować wielokrotnie od początku do końca. Po zweryfikowaniu ich podstawowych funkcji, upewnij się, przetestuj je w [symulacji danych](#disaster-simulation). Dzięki temu odkryć wady skrypty lub procesy.

Za najbardziej skuteczne rozwiązanie przy użyciu automatyzacji jest utworzenie repozytorium programu PowerShell lub interfejsu wiersza polecenia (polecenie) skrypty Azure awarii. Wyraźnie oznaczyć i kategoryzowanie ich łatwe wyszukiwanie. Wyznacza się jedna osoba repozytorium i przechowywanie wersji skrypty do zarządzania. Dokument je również z objaśnienia parametry oraz przykłady użycia skryptu. Upewnij się również zachowanie Niniejsza dokumentacja synchronizacji z Azure wdrożeń. To podkreśla przeznaczenie o jednej osoby odpowiedzialne za wszystkie części repozytorium.

##<a name="failure-detection"></a>Wykrywanie awarii

Poprawnie obsługuje problemów z dostępności i odzyskiwanie, muszą mieć możliwość wykrywanie i diagnozowanie błędów. Wykonaj czynności zaawansowanych serwerów i wdrażania monitorowania, aby szybko określić, kiedy systemu lub jej części działają nieoczekiwanie. Narzędzia, które przeglądać ogólnej kondycji usługi w chmurze i jego zależności monitorowania mogą wykonywać część tej pracy. Jeden narzędzie Microsoft jest [System Centrum 2016](https://www.microsoft.com/en-us/server-cloud/products/system-center-2016/). Narzędzia innych producentów oferuje również możliwości monitorowania. Większość rozwiązań monitorowania Śledzenie liczników klucza wydajności i dostępności usługi.

Te narzędzia są kluczową, nie zastępuje potrzeby planowanie wykrywanie błędów i raportowania w usłudze w chmurze. Musi zaplanować do prawidłowego używania diagnostyki Azure. Liczniki wydajności niestandardowej lub wpisy dziennika zdarzeń można także część ogólnej strategii. Dzięki temu dodatkowych danych podczas awarii szybko rozpoznać problem i przywracać pełne możliwości. Umożliwia także dodatkowe metryk za pomocą narzędzi do monitorowania można określić kondycji aplikacji. Aby uzyskać więcej informacji zobacz [Włączanie diagnostyki Azure w Azure usług w chmurze](../cloud-services/cloud-services-dotnet-diagnostics.md). Aby uzyskać informacje dotyczące sposobu planowania ogólny "modelu" zobacz [odporność: wskazówki dotyczące mechanizm architektury chmury](https://channel9.msdn.com/Series/FailSafe).

##<a name="disaster-simulation"></a>Symulacja danych

Testowanie symulacji polega na tworzenie małych sytuacjach rzeczywistych na hali pracy obserwować, jak reagować członków zespołu. Symulacji również pokazywać, jak efektywne są rozwiązania w planie odzyskiwania. Przeprowadzić symulacji w taki sposób, utworzone scenariusze nie przerwać rzeczywisty firm podczas nadal Cierpisz rzeczywistą sytuacje.

Należy rozważyć, czy Projektując typu "Panel przełączania" w aplikacji, aby ręcznie zasymulowania problemy z dostępnością. Na przykład za pomocą przełącznika wygładzone wyzwalanie bazy danych programu access wyjątki modułu sortowania, co powoduje nieprawidłowe działanie. Można wykonać podobne najprostsze podejście dla innych modułów, na poziomie interfejsu sieci.

Symulacja wyróżnienie nieodpowiedniego usuwające problemy. Scenariusze symulowany musi być całkowicie kontrolowane. Oznacza to, że, nawet w przypadku planu odzyskiwania wydaje się kończy się niepowodzeniem, można przywrócić sytuację znormalizować bez uszkodzenia znaczące. Ważne jest również informuje wyższego poziomu kierownictwa kiedy i jak ćwiczeń symulacji zostanie wykonana. Ten plan powinny zawierać informacje o czasie lub zasoby, które mogą zostać nieproduktywne, gdy test symulacji jest uruchomiony. Podczas badania już poddania swojego planu odzyskiwania danych, jest również zdefiniowanie, jak będzie mierzone sukcesu.

Istnieje kilka technik, których można przetestować planów odzyskiwania po awarii. Jednak większość z nich jest po prostu zmodyfikowanej wersji tych podstawowych technik. Główny napędowej za te testy jest ocena jak to możliwe i jak poprawnie jest plan odzyskiwania. Odzyskiwanie testowania zespoły na stronie Szczegóły, aby odkryć dziury w planie podstawowe odzyskiwania.

##<a name="next-steps"></a>Następne kroki

Ten artykuł jest częścią serii artykułów dotyczyła [Odzyskiwanie i wysoką dostępność dla aplikacji utworzonych na platformy Microsoft Azure](./resiliency-disaster-recovery-high-availability-azure-applications.md). Artykuł poprzedniego w tej serii jest [wysokiej dostępności aplikacji utworzonych na platformy Microsoft Azure](./resiliency-high-availability-azure-applications.md).
