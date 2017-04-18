<properties
   pageTitle="Lista kontrolna elastyczność | Microsoft Azure"
   description="Lista kontrolna zawiera wskazówki dotyczące rozwiązania problemów elastyczność podczas projektowania."
   services=""
   documentationCenter="na"
   authors="petertaylor9999"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="best-practice"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="petertay"/>

# <a name="azure-resiliency-guidance-resiliency-checklist"></a>Wskazówki Azure elastyczność: Lista kontrolna elastyczności

Projektowanie aplikacji dla elastyczność wymaga planowania i łagodzenia różne tryby awaryjne, które mogą wystąpić. Przejrzyj elementy w następującej listy kontrolnej przed projekt aplikacji tak, aby był bardziej elastyczne.

## <a name="requirements"></a>Wymagania dotyczące

- **Definiowanie wymagań dotyczących dostępności klienta.** Klient uzyskuje dostępność wymagania dotyczące elementów w aplikacji i wpłynie to na projekt aplikacji. Pobieranie umowy z klienta do celów dostępność każdego fragmentu aplikacji, w przeciwnym razie projektu może nie spełnia oczekiwań klienta. Aby uzyskać więcej informacji zobacz sekcję [Definiowanie wymagań elastyczność](guidance-resiliency-overview.md#defining-your-resiliency-requirements) [projektowania mechanizm wniosków o Azure](guidance-resiliency-overview.md) dokumentu.

## <a name="failure-mode-analysis"></a>Analiza tryb błędu

- **Wykonywanie analizy tryb błąd (FMA) dla aplikacji.** FMA jest procesem tworzenia elastyczność do aplikacji w fazie projektowania. Cele FMA obejmują:  

    - Określ, jakiego rodzaju błędów aplikacji mogą wystąpić. 
    - Przechwytywanie potencjalny wpływ każdego typu Błąd na pasku aplikacji i efekty.
    - Określenie strategii odzyskiwania. 

    Aby uzyskać więcej informacji, zobacz [Projektowanie mechanizm aplikacji dla Azure: analiza tryb błędu][fma].  

## <a name="application"></a>Aplikacji

- **Wdrażanie wielu wystąpień usług.** Usługi uniknięcia zakończy się niepowodzeniem, a jeśli aplikacja zależy od jedno wystąpienie usługi go uniknięcia nie powiedzie się również. Do zapewniania obsługi wielu wystąpień usługi [aplikacji Azure](../app-service/app-service-value-prop-what-is.md), wybierz [Plan usługi aplikacji](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) udostępniającej wielu wystąpień. Dla usług w chmurze Azure skonfiguruj każdego z poszczególnych ról za pomocą [wielu wystąpień](../cloud-services/cloud-services-choose-me.md#scaling-and-management). [Azure wirtualnych maszyn](../virtual-machines/virtual-machines-windows-about.md), upewnij się, że architektury maszyn wirtualnych zawiera więcej niż jeden maszyn wirtualnych i każdego maszyn wirtualnych będzie obejmować [Ustawianie dostępności][availability-sets].   

- **Umożliwia rozpowszechnianie żądania usługi równoważenia obciążenia.** Równoważenia obciążenia rozdziela żądania aplikacji wystąpieniach usług prawidłowy, usuwając nieprawidłowe wystąpienia z obrotu. Jeśli usługa korzysta z usługi aplikacji Azure lub usług w chmurze Azure, jest równoważenia dla Ciebie obciążenia. Jednak aplikacja używa maszyny wirtualne Azure, potrzebujesz obsługi administracyjnej usługi równoważenia obciążenia. Zobacz Omówienie [Równoważenia obciążenia Azure](../load-balancer/load-balancer-overview.md) , aby uzyskać więcej informacji. 

- **Konfigurowanie bram aplikacji Azure, aby użyć wielu wystąpień.** W zależności od potrzeb aplikacji [Azure aplikacji bramy](../application-gateway/application-gateway-introduction.md) może być lepiej dostosowane do rozpowszechniania żądania usługi aplikacji. Jednak jednego wystąpienia usługi bramy aplikacji są nie gwarancji, Umowa dotycząca poziomu usług, jest możliwe że aplikacja może nie działać, jeśli wystąpienie aplikacji bramy nie powiedzie się. Inicjowanie obsługi więcej niż jedno wystąpienie aplikacji bramy średniej lub większe zagwarantować dostępność usługi na warunkach [Umowa dotycząca poziomu usług](https://azure.microsoft.com/support/legal/sla/application-gateway/v1_0/).

- **Używanie zestawów dostępność dla każdego poziomu aplikacji**. Umieszczanie swojego wystąpienia w [Ustawianie dostępności] [ availability-sets] gwarancje łączności z co najmniej jedno wystąpienie maszyn wirtualnych w ramach [Umowa dotycząca poziomu usług](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_2/). Jeśli pośrednictwem usługi SMS nie znajdują się w zestaw dostępność, którego nie ma gwarantuje ich ochronie i istnieje możliwość może nie powiedzie się lub jednocześnie zaktualizowane. 

- **Należy rozważyć, czy wdrażania aplikacji u wielu regionów.** Jeśli aplikacja zostanie wdrożony jednego regionu, w rzadkich całego regionu staje się niedostępna, aplikacji, także będą niedostępne. Może to być do przyjęcia na warunkach SLA aplikacji. Jeśli tak, należy rozważyć, czy wdrażania aplikacji i usług w wielu regionów. Wdrożenie wielu regionów można użyć deseniu aktywny aktywny (dystrybucję żądań wielu wystąpień aktywnego) lub deseniu pasywne aktywne (pamiętając wystąpieniem "ciepłej" minimalną, w przypadku wystąpienia podstawowego kończy się niepowodzeniem). Zaleca się wdrożyć wielu wystąpień aplikacji usług w pary regionalne. Aby uzyskać więcej informacji, zobacz [firm ciągłości i awarii odzyskiwania (BCDR): regiony sparowane Azure](../best-practices-availability-paired-regions.md).

- **Wdrożenie elastyczność deseni dla operacji zdalnych odpowiednim.** Jeśli aplikacja zależy od komunikacji między usługami zdalnego, ścieżka komunikacji uniknięcia zakończy się niepowodzeniem. W przypadku wystąpienia wielu awarii pozostałe wystąpienia prawidłowy aplikacji usług może przerasta żądaniami. Są przydatne w przypadku sposób postępowania z typowych błędów w tym wzorca przekroczenia limitu czasu, [Spróbuj ponownie wzorca]kilku wzorców[retry-pattern], [rozmieszczenie] [ circuit-breaker] deseniu, a inne osoby. Aby uzyskać więcej informacji zobacz [Projektowanie mechanizm aplikacji dla Azure](guidance-resiliency-overview.md#implementing-resiliency-strategies). 

- **Odpowiadanie na zwiększenie obciążenia za pomocą autoscaling.** Jeśli aplikacja nie jest skonfigurowany do skalowania automatycznie wraz ze wzrostem obciążenia, jest to możliwe, że aplikacji usług zakończy się niepowodzeniem, jeśli ich nasycenia żądaniami użytkownika. Aby uzyskać więcej informacji zobacz następujące artykuły:

    - Ogólne: [Lista kontrolna skalowalność](../best-practices-scalability-checklist.md) 
    - Usługa Azure aplikacji: [Skalowanie licznik wystąpień ręcznie lub automatycznie][app-service-autoscale]
    - Usług w chmurze: [Jak automatyczne skalowanie usługi w chmurze][cloud-service-autoscale]
    - Maszyn wirtualnych: [Ustawia automatyczne skalowanie i maszyn wirtualnych skali][vmss-autoscale]


- **Wykonuje operacje asynchroniczne, o ile to możliwe.** Obraz operacji można całej zasobów i blokowanie innych operacji podczas wywołujący czeka aby ukończyć proces. Projektowanie każdego część aplikacji, aby umożliwić dla operacji asynchronicznych, o ile to możliwe. Aby uzyskać więcej informacji na temat sposobu realizacji asynchroniczne programowania w języku C#, zobacz [asynchroniczne programowania przy użyciu asynchroniczne i poczekać na][asynchronous-c-sharp].

- **Aby skierować ruch aplikacji do różnych regionów za pomocą Menedżera ruch Azure.**  [Azure Menedżer ruchu] [ traffic-manager] wykonuje równoważenia na poziomie DNS i może skierować ruch do różnych regionów oparte na [routingu ruchu] [ traffic-manager-routing] metody użytkownika i kondycji punkty końcowe aplikacji. 

- **Konfigurowanie i testowanie sondy kondycji dla swojego urządzenia do równoważenia obciążenia i ruch kierownictwa.** Zapewnić logiki kondycji sprawdza krytycznych części systemu i odpowiednio odpowiada sondy kondycji.

    - Kondycji sondy Menedżer [ruch Azure] [ traffic-manager] i [Równoważenia obciążenia Azure] [ load-balancer] służyć określoną funkcję. Dla Menedżera ruch sondy kondycja Określa, czy przechodzić do innego regionu. Do równoważenia obciążenia go określa, czy usunąć maszyny z obrotu.      

    - Dla sondy Menedżer ruchu punkt końcowy kondycji należy sprawdzić krytyczne zależności wdrażania w ramach tego samego regionu, a których awaria powinno powodować awarię na innego regionu.  

    - Do równoważenia obciążenia punkt końcowy kondycji Zgłoś kondycji maszyn wirtualnych. Nie uwzględniaj inne poziomy lub usług zewnętrznych. W przeciwnym razie błąd, który ma miejsce poza maszyn wirtualnych spowoduje równoważenia obciążenia usunąć maszyn wirtualnych z obrotu.

    - Wskazówki dotyczące wykonywania monitorowanie kondycji w aplikacji zobacz [Kondycji punktu końcowego monitorowania deseniu](https://msdn.microsoft.com/library/dn589789.aspx).

- **Monitorowanie usługi innych firm.** Jeśli aplikacja ma zależności dotyczące usług innych firm, określenie miejsca i jak tych usług innych firm może się nie powieść i co skuteczne te błędy będą mieć w swojej aplikacji. Usługi innych firm mogą nie zawierają monitorowania i diagnostyki, dlatego ważne do logowania wywołania do nich i ich powiązania z kondycją aplikacji i rejestrowanie przy użyciu Unikatowy identyfikator diagnostyczne. Aby uzyskać więcej informacji na temat najlepszych rozwiązań monitorowania i diagnostyki, zobacz [wskazówki monitorowania i diagnostyki] [ monitoring-and-diagnostics-guidance] dokumentu.

- **Upewnij się, że wszelkie usługi innych firm, których można używać zapewnia umowa dotycząca poziomu usług.** Jeśli aplikacja zależy od usługi innych firm, ale trzeciej zawiera gwarantowana dostępność w formularzu Umowa dotycząca poziomu usług, dostępności aplikacji nie należy również zagwarantować. Usługi Umowa dotycząca poziomu usług jest tylko najmniej dostępne część aplikacji.


## <a name="data-management"></a>Zarządzanie danymi

- **Opis metod replikacji dla źródeł danych aplikacji.** Dane aplikacji są przechowywane w różnych źródeł danych i mają różne dostępność wymagania. Szacowanie metod replikacji dla każdego typu przechowywania danych w Azure, tym [Azure replikacji przestrzeni dyskowej](../storage/storage-redundancy.md) oraz [SQL Replikacja bazy danych Active Geo -](../sql-database/sql-database-geo-replication-overview.md) upewnij się, że wymagania dane aplikacji są spełnione.

- **Upewnij się, że nie pojedyncze konto użytkownika ma dostęp do danych zarówno produkcji, jak i kopia zapasowa.** Kopie zapasowe danych są bezpieczne, jeśli jeden pojedyncze konto użytkownika ma uprawnienia do zapisu zarówno produkcji, jak i kopii zapasowej źródeł. Złośliwy użytkownik celowo można usunąć wszystkie dane, gdy zwykły użytkownik przypadkowo można usunąć. Projektowanie aplikacji, aby ograniczyć uprawnienia dla każdego konta użytkownika, tak aby tylko wymagających zapisu użytkownicy mają dostęp do zapisu i jest tylko do produkcji lub kopii zapasowej, ale nie obu.

- **Dokument źródła danych nie powieść nad i się nie powieść, proces i należy je przetestować.** W przypadku gdy źródła danych nie catastrophically ludzi operatora będą musiały Wykonaj zestaw udokumentowanymi instrukcjami przechodzić do nowego źródła danych. Mając błędy, kroki udokumentowane operatora nie będzie mógł pomyślnie wykonaj je i się nie powieść przez zasób. Regularnie przetestować czynności instrukcji, aby sprawdzić, czy operatora po ich jest możliwość pomyślnie niepowodzenie nad i powrócić do źródła danych.

- **Sprawdź poprawność kopie zapasowe danych.** Regularnie Zweryfikuj danych kopii zapasowych oczekiwań, uruchamiając skrypt do sprawdzania poprawności integralności danych, schematu i kwerend. Istnieje żaden punkt o kopii zapasowej, jeśli nie jest przydatne przywrócić źródła danych. Zaloguj się i raport niespójnościach, aby usługa Kopia zapasowa jest niemożliwa.

- **Warto rozważyć użycie miejsca do magazynowania typ konta, który jest zbędne geo.** Dane przechowywane w konto Azure miejsca do magazynowania jest zawsze replikować lokalnie. Istnieją jednak wielu strategie replikacji do wyboru zainicjowano obsługę administracyjną konta miejsca do magazynowania. Zaznacz [Zbędne magazyn Geo odczytu Azure (Pomoc Zdalna GRS)](../storage/storage-redundancy.md#read-access-geo-redundant-storage) do ochrony danych aplikacji przed rzadkich przypadkach, gdy całego regionu stanie się niedostępny.

    > [AZURE.NOTE] Dla maszyny wirtualne nie są oparte na GRS pomoc Zdalna replikacji przywrócenie dysków maszyn wirtualnych (wirtualny dysk twardy plików). Użyj zamiast tego [Kopia zapasowa Azure][azure-backup].   

## <a name="operations"></a>Operacje

- **Implementowanie monitorowania i alerty najlepsze rozwiązania w aplikacji.** Bez właściwego monitorowania, narzędzia diagnostyczne i alertu istnieje żaden sposób wykrywanie błędów w aplikacji i powiadomienia operatora rozwiązywania tych problemów. Aby uzyskać więcej informacji na temat najlepszych rozwiązań, zobacz [wskazówki monitorowania i diagnostyki] [ monitoring-and-diagnostics-guidance] dokumentu.

- **Zmierz zdalnego o połączeniach i udostępnić informacje do zespołu aplikacji.**  Jeśli nie śledzenia i raportów statystyki połączenia zdalnego w czasie rzeczywistym z łatwością Przejrzyj poniższe informacje, zespołu operacji nie będzie miał chwilową widok do kondycji aplikacji. A jeśli tylko Godzina średnia połączenia zdalnego miary, użytkownik nie ma wystarczających informacji, aby wyświetlić problemy w usługach. Podsumowywanie metryki połączenia zdalnego, takich jak czas oczekiwania, przepustowość i błędów w percentylu 99 i 95. Wykonywanie analiz statystycznych na metryki w celu wykrycia błędów występujących w każdej wartości percentylu.

- **Śledzenie liczby wyjątków przejściowych i ponowne próby w odpowiednich czasu.** Jeśli nie śledzenie i monitorowanie przejściowych wyjątki i ponowne próby w czasie, jest możliwe, że problemu lub błąd można go ukryć, Logika ponawiania aplikacji. Oznacza to, że jeśli do monitorowania i rejestrowania tylko wyświetlana sukces lub Niepowodzenie operacji, fakt, że operację miał ponowienia wielokrotnie z powodu wyjątki zostaną ukryte. Trendu wzrostu wyjątki w czasie wskazuje, czy usługa napotkał problem i może zakończyć się niepowodzeniem. Aby uzyskać więcej informacji, zobacz [Ponów próbę wskazówki określonych usług][retry-service-guidance].

- **Wdrożenie systemu wczesnego ostrzegania ostrzeżeniem operatora.** Identyfikowanie klucza wydajności wskaźników zdrowia aplikacji, takich jak przejściowych wyjątki i zdalnych połączeń opóźnienie i ustawianie wartości progowe odpowiednie dla każdego z nich. Wysyłanie alertu z operacjami po osiągnięciu progu. Ustaw te progi na poziomie, które wskazują problemy przed stać się krytyczne i wymagają odpowiedzi odzyskiwania.

- **Dokument procesu publikacji aplikacji.** Bez dokumentacji proces szczegółowy wersji operatora może wdrażania aktualizacji uszkodzone lub nieprawidłowo Konfigurowanie ustawień dla aplikacji. Wyraźnie Definiowanie procesu udostępniania dokumentów i upewnij się, że jest dostępna dla całego zespołu operacji. Najważniejsze wskazówki dotyczące mechanizm wdrażania aplikacji wyszczególniono w ramach [wdrożenia mechanizm] [ guidance-resilient-deployment] sekcji elastyczności wytycznych.

- **Upewnij się, że więcej niż jedna osoba w zespole przygotowaniu monitorowanie aplikacji i wykonywać wszystkie czynności ręcznie.** Jeśli masz tylko jednego operatora na zespołu, do którego można monitorować aplikację i rozpocząć proces odzyskiwania, osoba staje się pojedynczego punktu awarii. Szkolenie wielu osobom na wykrywanie i odzyskiwanie i upewnij się, że zawsze będzie co najmniej jeden aktywna w dowolnym momencie.

- **Automatyzowanie procesu wdrażania aplikacji.** Jeśli wymagane ręcznie wdrażania aplikacji jest pracowników, błędu człowieka mogą powodować wdrożenia kończy się niepowodzeniem. Uzyskać więcej informacji na temat najlepszych rozwiązań do automatyzowania wdrażania aplikacji, zobacz [wdrożenia mechanizm] [ guidance-resilient-deployment] sekcji elastyczności wytycznych.

- **Projektowanie procesu wersji, aby zmaksymalizować dostępność aplikacji.** Jeśli proces wersji wymaga, aby przejść do trybu offline podczas wdrażania usługi, aplikacja będą niedostępne do momentu ich powrocie do trybu online. Za pomocą techniki wdrożenia [niebiesko zieloną](http://martinfowler.com/bliki/BlueGreenDeployment.html) lub [Zwalnianie Kanaryjskich](http://martinfowler.com/bliki/CanaryRelease.html) wdrażania aplikacji z produkcją. Obejmować jedną z następujących metod, wdrażanie kodu wersji obok kodzie produkcyjnym, użytkownicy kodu wersji można nastąpi przekierowanie do kodu produkcji w przypadku awarii. Aby uzyskać więcej informacji, zobacz [wdrożenia mechanizm] [ guidance-resilient-deployment] sekcji elastyczności wytycznych.

- **Zaloguj się i inspekcji wdrożeń aplikacji.** Jeśli korzystasz z umieszczane technik wdrażania takich jak wersji niebiesko zieloną lub Mozga, będzie więcej niż jedną wersję aplikacji działa w produkcji. Jeśli wystąpi problem, niezwykle ważne jest, aby określić, jaka wersja programu aplikacji powoduje problem. Wdrożenie strategii niezawodne funkcje rejestrowania Przechwytywanie maksymalnej informacje specyficzne dla wersji, jak to możliwe. 

- **Upewnij się, że aplikacja nie działa sprzętu [ogranicza Azure subskrypcji](../azure-subscription-service-limits.md).** Subskrypcje Azure mieć ograniczenia określonych typów zasobów, takie jak liczba grup zasobów, liczby rdzeni i liczba kont miejsca do magazynowania.  Wymagań dotyczących aplikacji przekroczenia limitu Azure subskrypcji, utwórz inny Azure subskrypcji i dostarczanie wystarczających zasobów.

- **Upewnij się, że aplikacja nie działa sprzętu [ogranicza na usługę](../azure-subscription-service-limits.md).** Poszczególne usługi Azure mają ograniczenia zużycie &mdash; na przykład limitów miejsca do magazynowania, przepustowość, liczba połączeń, żądania na sekundę i inne wskaźniki. Aplikacja zakończy się niepowodzeniem, jeśli próbuje użyć tylko limity te zasoby. Spowoduje to ograniczania i możliwe przestojów dla odpowiednich użytkowników. 

    W zależności od określonej usługi i wymagań dotyczących aplikacji często uniknąć można te limity skalowania w górę (na przykład wybierając inny poziom cennik) lub Skalowanie zewnętrzne (dodawania nowych wystąpień).  

- **Projektowanie aplikacji wymagania dotyczące magazynu objęte elementów docelowych wydajność i skalowalność Azure miejsca do magazynowania.** Azure miejsca do magazynowania ma działać w wstępnie zdefiniowanych elementów docelowych wydajność i skalowalność, dlatego projektowanie aplikacji są używane miejsca do magazynowania w ramach tych elementów docelowych. W przypadku przekroczenia tych elementów docelowych aplikacji będą wrażenia ograniczania miejsca do magazynowania. Aby rozwiązać ten problem, dodawać kolejne konta miejsca do magazynowania. Po uruchomieniu sprzętu limitu magazynowania konta, obsługi administracyjnej dodatkowe subskrypcje Azure i obsługi administracyjnej kolejne konta miejsca do magazynowania. Aby uzyskać więcej informacji zobacz [skalowalność miejsca do magazynowania Azure i cele](../storage/storage-scalability-targets.md).

- **Wybierz odpowiedni rozmiar maszyn wirtualnych aplikacji.** Zmierz Procesora rzeczywistych pamięci, dysku i we/wy z pośrednictwem usługi SMS produkcji i sprawdź, czy rozmiar pamięci Wirtualnej, który został zaznaczony, jest wystarczające. Jeśli nie, aplikacja mogą występować problemy zdolności jako maszyny wirtualne dojechać do swoich limitów. Rozmiary maszyn wirtualnych opisane szczegółowo w dokumencie [rozmiarów maszyn wirtualnych w Azure](../virtual-machines/virtual-machines-windows-sizes.md) .

- **Określają, czy obciążenie pracą aplikacji jest stabilny czy zmian w czasie.** Jeśli z pracą zmienia się w czasie, użyj Azure maszyn wirtualnych skali ustawia automatycznie skali liczbę wystąpień maszyn wirtualnych. W przeciwnym razie należy ręcznie zwiększyć lub zmniejszyć liczbę maszyny wirtualne. Aby uzyskać więcej informacji zobacz [Omówienie zestawy skali maszyn wirtualnych](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md).

- **Wybierz pozycję warstwa prawo usług dla bazy danych SQL Azure.** Aplikacja używa bazy danych SQL Azure, upewnij się, że została wybrana warstwa odpowiednią usługę. Jeśli wybierzesz warstwy, która nie jest w stanie obsługiwać wymagania dotyczące jednostek (DTU) transakcji bazy danych aplikacji, z użyciem danych będzie ograniczenie. Aby uzyskać więcej informacji na temat wybierania plan usług poprawne, zobacz [opcji bazy danych SQL i wydajności: opis, jakie opcje są dostępne w każdej warstwie usługi](../sql-database/sql-database-service-tiers.md) dokumentu. 

- **Masz planu powrotu do wdrożenia.** Istnieje możliwość, że wdrożenia aplikacji może zakończyć się niepowodzeniem i powodować aplikacji stają się niedostępne. Projektowanie procesu wycofywania wróć do ostatnią znaną dobrą wersję i zminimalizować przestoje. Zobacz [mechanizm wdrażania] [ guidance-resilient-deployment] sekcji elastyczności wytycznych, aby uzyskać więcej informacji. 

- **Tworzenie procesu interakcji z Azure pomocy technicznej.** Jeśli proces skontaktowanie się z [obsługuje Azure](https://azure.microsoft.com/support/plans/) nie jest ustawiona, zanim zajdzie taka potrzeba się z pomocą techniczną, przestoje zostanie przedłużony zgodnie z pomocy technicznej jest nawigować po raz pierwszy. Uwzględnij proces kontaktu z pomocą techniczną i zamocowaniem problemów jako część aplikacji elastyczność od początku.

- **Upewnij się, że aplikacja nie używa więcej niż maksymalna liczba kont miejsca do magazynowania dla subskrypcji.** Azure pozwala maksymalnie 200 kont miejsca do magazynowania dla subskrypcji. Jeśli aplikacja wymaga więcej miejsca do magazynowania kont nie są jeszcze dostępne w ramach subskrypcji, musisz utworzyć nową subskrypcję, a następnie utwórz konta dodatkowego miejsca do magazynowania. Aby uzyskać więcej informacji zobacz [Azure subskrypcji i limity dotyczące usługi, przydziały oraz ograniczenia](../azure-subscription-service-limits.md#storage-limits).

- **Upewnij się, że aplikacja nie przekracza celów skalowalność maszyn wirtualnych dysków.** Maszyn wirtualnych Azure IaaS obsługuje dołączanie liczba dyski danych w zależności od kilku czynników, takich jak rozmiar pamięci Wirtualnej i typ konta miejsca do magazynowania. Jeśli aplikacja jest większa niż celów skalowalność dysków maszyn wirtualnych, obsługi administracyjnej konta dodatkowego miejsca do magazynowania i Utwórz na dyskach maszyn wirtualnych. Aby uzyskać więcej informacji, zobacz [skalowalność miejsca do magazynowania Azure i cele] (.. / storage/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks)

## <a name="test"></a>Test

- **Wykonywanie awaryjnego i powrotu testowania aplikacji.** Jeśli jeszcze nie w pełni przetestowane awaryjnego i powrotu, nie może być określone, że w aplikacji usługi zależne przywracane w sposób zsynchronizowane podczas awarii. Upewnij się, że zależne od aplikacji usług awaryjnego i błędów w odpowiedniej kolejności.

- **Wykonywanie uruchomienie błędów, testowanie aplikacji.** Aplikacja może się nie powieść wiele różnych powodów, takich jak certyfikat wygaśnie nasycenia zasoby systemowe maszyny lub błędy miejsca do magazynowania. Testowanie aplikacji w środowisku możliwie najbliżej produkcji, symulowanie lub powodujące rzeczywistą błędy. Na przykład usunąć certyfikaty, sztucznie zajmują zasoby systemowe lub usunąć źródła miejsca do magazynowania. Sprawdź aplikacji możliwość odzyskiwanie z wszystkich typów błędów, tylko oraz w połączeniu. Sprawdź, czy błędy nie są propagowanie lub kaskadowych za pomocą systemu.

- **Uruchom testy produkcji przy użyciu zarówno dane użytkownika syntetycznych i rzeczywistych.** Badań i produkcji są rzadko identyczne, dlatego należy używać niebiesko zieloną lub Mozga wdrożenia i przetestować aplikację produkcji. Dzięki temu będzie można przetestować aplikację produkcji obciążeniu rzeczywistą i upewnij się, że działają zgodnie z oczekiwaniami po wdrożeniu w pełni.

## <a name="security"></a>Zabezpieczenia

- **Wdrożenie aplikacji poziom ochrony przed odmowie atakami usług (DDoS).** Usług Azure są chronione przed atakami DDos w warstwie sieci. Jednak Azure nie chroni atakami warstwy aplikacji, ponieważ trudno odróżniać żądania PRAWDA użytkownika z żądania złośliwego użytkownika. Aby uzyskać więcej informacji na temat ochrony przed atakami DDoS warstwy aplikacji zobacz sekcję "Ochrona przed DDoS" [Microsoft Azure Network Security](http://download.microsoft.com/download/C/A/3/CA3FC5C0-ECE0-4F87-BF4B-D74064A00846/AzureNetworkSecurity_v3_Feb2015.pdf) (plik PDF do pobrania).

- **Wdrożenie zasada przyznawania jak najmniejszych uprawnień dostępu do aplikacji zasobów.** Domyślne dla dostępu do zasobów aplikacji powinna być jak restrykcyjnie, jak to możliwe. Udzielanie uprawnień na poziomie wyższą na podstawie zatwierdzenia. Udzielanie nadmiernie ograniczeń dostępu do zasobów aplikacji domyślnie może spowodować ktoś celowo lub przypadkowo usuwania zasobów. Azure zapewnia [Kontrola dostępu oparta na rolach](../active-directory/role-based-access-built-in-roles.md) do zarządzania uprawnieniami, ale ważne jest, aby sprawdzić, co najmniej uprawnienia uprawnień dla innych zasobów, które mają własne systemów uprawnienia, takich jak program SQL Server. 

## <a name="telemetry"></a>Telemetrycznego

- **Rejestrowanie danych telemetrycznych, gdy aplikacja jest uruchomiona w środowisku produkcyjnym.** Przechwytywanie informacji telemetrycznego niezawodne, gdy aplikacja działa w środowisku produkcyjnym, lub nie ma wystarczających informacji do diagnozowania przyczyny problemów podczas aktywnie służy użytkowników. Więcej informacji znajduje się na liście rejestrowanie najważniejsze wskazówki jest dostępna w [orientacji monitorowania i diagnostyczne] [ monitoring-and-diagnostics-guidance] dokumentu.

- **Wdrożenie rejestrowanie przy użyciu asynchroniczne deseniem.** Jeśli rejestrowanie operacji są synchroniczne, może zablokować możliwość kodzie aplikacji. Upewnij się, wykonanie z operacjami rejestrowania jako operacji asynchronicznych. 

- **Dostosować danych dziennika granicami usługi.** W typowych aplikacji n warstwowych żądanie użytkownika może być przechodzenie przez kilka ograniczenia usługi. Na przykład żądanie użytkownika na ogół pochodzi w warstwie sieci web jest przekazywane do poziomu firm i na końcu są zachowywane w warstwie danych. W bardziej złożonych scenariuszach żądanie użytkownika może być rozdzielana wiele różnych sklepów usług i danych. Upewnij się, że system rejestrowania metkami połączeń granicami usługi, co pozwala na śledzenie żądania w aplikacji.

##  <a name="azure-resources"></a>Zasoby Azure 

- **Korzystanie z szablonów Azure Menedżera zasobów do świadczenia zasobów.** Menedżer zasobów szablonów ułatwiają zautomatyzować wdrożeń przy użyciu programu PowerShell lub polecenie Azure, które prowadzą do niezawodne procesu wdrażania. Aby uzyskać więcej informacji, zobacz [Omówienie Menedżera zasobów Azure][resource-manager].

- **Nadaj zasobów zrozumiałej nazwy.** Określeniu znaczące nazwy zasobów ułatwia Znajdź określonego zasobu i opis roli. Aby uzyskać więcej informacji zobacz [zalecane nazewnictwa konwencje Azure zasobów](guidance-naming-conventions.md) 

- **Korzystanie z kontroli dostępu oparta na rolach (RBAC)**. Kontrolowanie dostępu do zasobów Azure wdrażane za pomocą RBAC. RBAC można przypisać role autoryzacji do członków zespołu DevOps, aby zapobiec przypadkowemu usunięciu lub zmiany wdrożonym zasobów. Aby uzyskać więcej informacji zobacz [Wprowadzenie do zarządzania dostępu w portalu Azure](../active-directory/role-based-access-control-what-is.md) 

- **Użyj blokowania zasobów dla krytycznych zasoby, takie jak maszyny wirtualne.** Blokowania zasobów uniemożliwiają operatora przypadkowego usunięcia zasobu. Aby uzyskać więcej informacji zobacz [Blokowanie zasoby przy użyciu Menedżera zasobów Azure](../resource-group-lock-resources.md) 

- **Pary regionalne.** Wdrażając dwóch regionów, wybierz regionów z tej samej pary regionalne. W przypadku awarii ogólne odzyskiwania jeden region priorytety są przypisywane z każdej pary. Niektórych usług, takich jak zbędne Geo magazynowania zapewniają automatyczne replikacji iloczynów region. Aby uzyskać więcej informacji, zobacz [firm ciągłości i awarii odzyskiwania (BCDR): regiony sparowane Azure](../best-practices-availability-paired-regions.md) 

- **Organizowanie grup zasobów, funkcja i cyklu życia**.  Ogólnie grupa zasobów powinien zawierać zasobów, które należą samego cyklu życia. Ułatwia wdrożeń, Usuń wdrożeń test i przypisać uprawnienia dostępu, redukując prawdopodobieństwo wdrożenia produkcyjnej jest przypadkowego usunięcia lub zmodyfikowane. Tworzenie oddzielnych grup zasobów dla produkcji, rozwój i przetestuj środowiskach. We wdrożeniu w przypadku wprowadzenia zasobów dla każdego regionu oddzielnych grup zasobów. Ułatwia ponownie rozmieścić jeden region bez wpływu na inne regionu. 

## <a name="azure-services"></a>Usług Azure 

Zastosuj następujące elementy listy kontrolnej przeznaczonej do określonych usług platformy Azure.

###  <a name="app-service"></a>Aplikacji usługi 

- **Za pomocą warstwa Standard lub Premium.** Te poziomy pomocy technicznej tymczasowy gniazda i automatycznego wykonywania kopii zapasowych. Aby uzyskać więcej informacji zobacz [Omówienie szczegółowo planów Azure aplikacji usługi](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) 

- **Aby uniknąć skalowania w górę lub w dół.** Zamiast tego wybrać warstwy i wystąpienie rozmiar spełniających wymagań dotyczących wydajności w obszarze typowym obciążeniu i [skali się](../app-service-web/web-sites-scale.md) wystąpienia do obsługi zmian w natężenia ruchu. Skalowanie i rozwijaniu może pojawić się ponowne uruchomienie aplikacji.  

- **Konfiguracja magazynu jako ustawienia aplikacji.** Ustawienia aplikacji służy do przechowywania ustawienia konfiguracji jako ustawienia aplikacji. Definiowanie ustawień w szablonach Menedżera zasobów lub przy użyciu programu PowerShell, tak aby można je jako część automatycznego wdrażania zastosować / aktualizacja procesu, który jest bardziej niezawodne. Aby uzyskać więcej informacji zobacz [Konfigurowanie aplikacji sieci web w usłudze Azure w aplikacji](../app-service-web/web-sites-configure.md). 

- **Tworzenie osobnych planów aplikacji usługi do produkcji i testowania.** Nie używaj gniazda wdrożenia produkcji testowania.  Wszystkie aplikacje w ramach tego samego planu aplikacji usługi Udostępnianie samego wystąpienia maszyn wirtualnych. Jeśli umieszczasz produkcji i wdrożeń test tego samego planu, jego negatywnie wpłynąć na wdrożenie produkcji. Na przykład testów obciążenia pogarsza witryny produkcyjnym. Umieszczając wdrożeń test do oddzielnych planu, można je odizolować z wersji produkcji.  

- **Aplikacje web osobnych z sieci web interfejsów API**. Jeśli rozwiązanie ma frontonu sieci web i interfejsu API sieci web, warto rozważyć decomposing je do oddzielnych aplikacji usługi aplikacji. Ten projekt ułatwia do rozkładania rozwiązanie przez obciążenie pracą. Aplikacji sieci web oraz interfejsu API można uruchamiać w osobnym planach usługi aplikacji, aby niezależne skalowanie. Jeśli nie potrzebujesz skalowalność na tym samym poziomie najpierw możesz wdrażanie aplikacji do tego samego planu i przenieść je do oddzielnych planów później, jeśli to konieczne.

- **Unikaj, wykonywanie kopii zapasowej bazy danych programu SQL Azure za pomocą funkcji wykonywania kopii zapasowych aplikacji usługi.** Użyj [bazy danych SQL automatycznego wykonywania kopii zapasowych][sql-backup]. Wykonywanie kopii zapasowych aplikacji usługi eksportuje bazy danych do pliku .bacpac SQL koszty DTUs.  

- **Wdrażanie tymczasowy przedział.** Tworzenie przedział wdrożenia dla tymczasowego. Wdrażanie aplikacji aktualizacje tymczasowy przedział i sprawdź wdrożenie przed wstawiany produkcji. Zmniejsza ryzyko nieprawidłowe aktualizacji produkcji. Zapewnia również, rozgrzane wszystkie wystąpienia przed są zamienione na produkcji. Wiele aplikacji ma istotne warmup i czas rozpoczęcia zimnej. Aby uzyskać więcej informacji zobacz [Konfigurowanie tymczasowej środowiska dla aplikacji sieci web w usłudze Azure aplikacji](../app-service-web/web-sites-staged-publishing.md). 

-  **Tworzenie przedział wdrażania, aby wstrzymać ostatniej znanej dobrej wdrożenia (ostatnia).** Po wdrożeniu aktualizacji produkcji przenoszenie poprzedniego wdrożenia produkcji przedział ostatnia. Ułatwia do przywrócenia nieprawidłowe wdrożenia. Jeśli później wykryjesz problem, można powrócić do wersji ostatnia. Aby uzyskać więcej informacji, zobacz [Architektura Azure odwołania: aplikacji sieci web podstawowe](guidance-web-apps-basic.md). 

- **Włącz rejestrowanie diagnostyczne**, takich jak rejestrowania aplikacji i rejestrowania serwera sieci web. Rejestrowanie jest ważna w przypadku monitorowania i diagnostyki. Zobacz [Włączanie diagnostyki rejestrowania aplikacji sieci web w usłudze Azure aplikacji](../app-service-web/web-sites-enable-diagnostic-log.md)

- **Dziennik blob miejsca do magazynowania**. Ułatwia do gromadzenia i analizy danych. 

- **Utwórz konto przechowywania dzienników.** Nie należy używać tego samego konta miejsca do magazynowania dla dzienników i dane aplikacji. Ułatwia to zapobieganie rejestrowania z zmniejszenie wydajności aplikacji. 

- **Monitorowanie wydajności.** Za pomocą usługi, takiej jak [Nowy Relic](http://newrelic.com/) lub [Wniosków aplikacji](../application-insights/app-insights-overview.md) monitorowanie wydajności aplikacji i zachowanie obciążeniu do monitorowania wydajności.  Monitorowanie wydajności umożliwia wgląd w czasie rzeczywistym w aplikacji. Umożliwia sprawdzanie pod kątem błędów i wykonaj analizę przyczyn błędów. 


###  <a name="application-gateway"></a>Brama aplikacji 

- **Inicjowanie obsługi co najmniej dwa wystąpienia.** Wdrażanie aplikacji bramy z co najmniej dwa wystąpienia. Jedno wystąpienie jest pojedynczy punkt awarii. Dwa lub więcej obiektów za pomocą nadmiarowości i skalowalność. Aby można było uzyskać [Umowa dotycząca poziomu usług](https://azure.microsoft.com/support/legal/sla/application-gateway/v1_0/), obsługi administracyjnej dwa lub więcej obiektów średnie lub większej. 

### <a name="azure-search"></a>Azure wyszukiwania

- **Inicjowanie obsługi więcej niż jednej replice.** Użyj co najmniej dwie repliki dla odczytu wysokiej dostępności lub trzy do odczytu i zapisu dostępności.

- **Konfigurowanie indeksatory w przypadku wdrożeń wielu region.** Jeśli masz wdrażania wielu regionu, należy rozważyć opcje obsługi ciągłości indeksowania.

    - Jeśli źródło danych jest replikowane geo, wskaż jej replice źródła danych lokalnych każdego indeksatora każdej regionalne usługi Azure wyszukiwania.  

    - Jeśli źródło danych nie jest replikowane geo, wskazywać wielu indeksatory na tym samym źródłem danych, tak aby usługi Azure wyszukiwanie w wielu regionów przez cały czas i niezależne indeksu ze źródła danych. Aby uzyskać więcej informacji, zobacz [Uwagi dotyczące wydajności i optymalizacji wyszukiwania Azure][search-optimization].

###  <a name="azure-storage"></a>Azure miejsca do magazynowania 

- **Dane aplikacji wybierz odczytu zbędne geo miejsca do magazynowania (Pomoc Zdalna GRS).** Magazyn GRS pomoc Zdalna replikuje dane na pomocniczym region i zapewnia dostęp tylko do odczytu z obszaru pomocniczą. Jeśli istnieje awaria miejsca do magazynowania w obszarze podstawowy, aplikacji może odczytać danych z pomocniczą regionu. Aby uzyskać więcej informacji zobacz [replikacji magazyn Azure](../storage/storage-redundancy.md).

- **Za pomocą dysków dla maszyn wirtualnych Premium miejsca do magazynowania** Aby uzyskać więcej informacji, zobacz [miejsca do magazynowania Premium: wysokiej wydajności miejsca do magazynowania dla Azure maszyn wirtualnych obciążenia](../storage/storage-premium-storage.md).

- **Do przechowywania kolejki utworzyć kolejkę kopii zapasowej w innym regionie.** Magazyn kolejek replice tylko do odczytu jest ograniczona użycia, ponieważ nie można w kolejce lub usuwania z kolejki elementów. Należy utworzyć kolejkę kopii zapasowej na koncie miejsca do magazynowania w innym regionie. W przypadku awarii miejsca do magazynowania, aplikacja można używać kolejki kopii zapasowej, aż podstawowy obszar ponownie stanie się dostępna. W ten sposób aplikacji może nadal przetwarzać nowego żądania.  

###  <a name="documentdb"></a>DocumentDB 

- **Replikować bazy danych między różnymi regionami.** Za pomocą konta w przypadku DocumentDB bazy danych ma jednego zapisu region i wielu regionów odczytu. Jeśli występuje błąd w regionie zapisu, można znaleźć w replice innym. SDK klienta obsługuje to automatycznie. Możesz również nie działać na regionu zapisu do innego regionu. Aby uzyskać więcej informacji zobacz [dane Rozłóż globalnie DocumentDB](../documentdb/documentdb-distribute-data-globally.md).


###  <a name="sql-database"></a>Baza danych SQL 

- **Za pomocą warstwa Standard lub Premium.** Te poziomy Podaj okres już w chwili przywracania (35 dni). Aby uzyskać więcej informacji zobacz [Opcje bazy danych SQL i wydajności](../sql-database/sql-database-service-tiers.md).

- **Włącz inspekcję bazy danych SQL.** Inspekcja może służyć do diagnozowania atakami lub wystąpienia błędu człowieka. Aby uzyskać więcej informacji zobacz [Wprowadzenie do inspekcji bazy danych SQL](../sql-database/sql-database-auditing-get-started.md). 

- **Replikacja Geo Active użycia** Replikacja Geo Active umożliwia utworzenie czytelne pomocniczym w innym regionie.  Jeśli podstawową bazą danych nie powiedzie się, lub po prostu musi być do trybu offline, wykonaj ręczne awarię na pomocniczej bazy danych.  Aż po wymuszeniu przejęcia pomocniczej bazy danych jest tylko do odczytu.  Aby uzyskać więcej informacji zobacz [SQL Replikacja bazy danych Active Geo](../sql-database/sql-database-geo-replication-overview.md). 

- **Używanie sharding**. Warto rozważyć użycie sharding na poziomie dzielenia bazy danych. Sharding zapewnia izolacji błędów. Aby uzyskać więcej informacji zobacz [Skalowanie się z bazą danych SQL Azure](../sql-database/sql-database-elastic-scale-introduction.md). 

- **Przywracanie w chwili umożliwia odzyskiwanie z błędu człowieka.**  Przywracanie w chwili zwraca bazy danych do wcześniejszego punktu w czasie. Aby uzyskać więcej informacji, zobacz [odzyskiwanie bazy danych programu Azure SQL za pomocą automatyczne kopie][sql-restore].

- **Przywracanie geo umożliwia odzyskiwanie z awarię usług.** Przywracanie Geo przywraca bazę danych z kopii zapasowej geo zbędne.  Aby uzyskać więcej informacji, zobacz [odzyskiwanie bazy danych programu Azure SQL za pomocą automatyczne kopie][sql-restore].


###  <a name="sql-server-running-in-a-vm"></a>Program SQL Server (działa w maszyny)

- **Replikacji bazy danych.** Za pomocą programu SQL Server zawsze na grupy dostępności replikacji bazy danych. Zapewnia wysoką dostępność, jeśli jedno wystąpienie programu SQL Server nie powiedzie się. Aby uzyskać więcej informacji zobacz [więcej informacji...](guidance-compute-n-tier-vm.md) 

- **Wykonywanie kopii zapasowej bazy danych**. Jeśli już używasz [Azure kopii zapasowej](https://azure.microsoft.com/documentation/services/backup/) do tworzenia kopii zapasowych pośrednictwem usługi SMS, warto rozważyć użycie [Kopia zapasowa Azure za pomocą DPM obciążenia programu SQL Server](../backup/backup-azure-backup-sql.md). Ta metoda ma jedna rola administratora kopii zapasowej dla organizacji i procedurę odzyskiwania ujednolicony maszyny wirtualne i SQL Server. W przeciwnym razie za pomocą [Programu SQL Server zarządzane kopia zapasowa Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx). 


###  <a name="traffic-manager"></a>Menedżer ruchu 

- **Uruchamianie ręczne awarii.** Po przełączeniu Menedżer ruchu wykonaj awarii ręcznego, zamiast automatycznie awarii ponownie. Przed niepowodzeniem ponownie, sprawdź, czy wszystkie podsystemów aplikacji są prawidłowy.  W przeciwnym razie możesz utworzyć sytuacji, gdy aplikacja przerzucony i z powrotem między centrami danych. Aby uzyskać więcej informacji zobacz [Uruchamianie maszyny wirtualne w wielu regionów](guidance-compute-multiple-datacenters.md).

- **Tworzenie punktu końcowego sondy kondycji**. Tworzenie niestandardowego punktu końcowego, który raporty dotyczące ogólnej kondycji aplikacji. Dzięki temu Menedżer ruchu niepowodzenie nad, jeśli wszystkie ścieżki krytycznej kończy się niepowodzeniem, nie tylko frontonu. Punkt końcowy powinna zwrócić kod błędu HTTP, jeśli dowolne zależność krytyczne jest nieprawidłowe lub jest nieosiągalny. Nie raportować błędy niekrytyczne usługach, jednak. W przeciwnym razie sondy kondycji może wyzwolenia pracy awaryjnej, gdy nie jest potrzebna, tworzenie wyniki fałszywie dodatnie. Aby uzyskać więcej informacji zobacz [Menedżer ruchu punktu końcowego monitorowania i pracy awaryjnej](../traffic-manager/traffic-manager-monitoring.md).


###  <a name="virtual-machines"></a>Maszyn wirtualnych 

- **Unikaj uruchamiania obciążenie pracą produkcji na jednym maszyn wirtualnych.** Pojedynczy wdrażania maszyn wirtualnych jest mechanizm do niezaplanowane lub planowanej konserwacji. Należy umieścić wiele maszyny wirtualne w Ustaw dostępność lub ustaw skali maszyn wirtualnych, przy użyciu usługi równoważenia obciążenia na wierzchu. Aby można było uzyskać Umowa dotycząca poziomu usług, należy wdrożyć co najmniej dwa maszyn wirtualnych w ten sam zestaw dostępności. Aby uzyskać więcej informacji zobacz [Omówienie zestawy skali maszyn wirtualnych](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md). 

- **Określanie dostępności, ustawianie podczas obsługę maszyn wirtualnych.** Obecnie jest sposobem Dodawanie maszyny Menedżera zasobów do dostępność po zainicjowano obsługę administracyjną maszyn wirtualnych. Po dodaniu nowych maszyn wirtualnych do istniejącego dostępność Ustaw, upewnij się, że tworzenie NIC dla maszyn wirtualnych i dodać NIC do puli adresów wewnętrznej w module równoważenia obciążenia. W przeciwnym razie równoważenia obciążenia nie skierować ruch sieciowy do tego maszyn wirtualnych. 

- **Umieść każdej warstwy aplikacji do oddzielnych zestawu dostępności.** W aplikacji N warstwowych nie należy umieścić maszyny wirtualne z różnych poziomów w ten sam zestaw dostępności. Maszyny wirtualne w zestawie dostępności są umieszczane w domenach błędów (FDs) i zaktualizuj domen (UD). Jednak uzyskanie korzyścią nadmiarowości FDs i UDs każdej maszyn wirtualnych w zestawie dostępność muszą mieć możliwość obsługi takiego samego żądania klienta. 

- **Wybierz odpowiedni rozmiar maszyn wirtualnych na podstawie wymagań dotyczących wydajności.** Podczas przenoszenia istniejącego pracą Azure, rozpoczyna się rozmiar pamięci Wirtualnej, który najlepiej pasuje do lokalnych serwerów. Następnie pomiaru wydajności usługi rzeczywiste obciążenie Procesora, pamięci i dysku operacji i/o na SEKUNDĘ i dopasowywanie rozmiaru, w razie potrzeby. Pomaga to upewnić się, że aplikacja działa zgodnie z oczekiwaniami w środowisku chmury. Ponadto jeśli potrzebujesz kart, należy pamiętać o limit NIC dla każdego rozmiaru. 

- **Używanie premium miejsca do magazynowania dla wirtualnych dysków twardych.** Azure magazynowania Premium zapewnia obsługę dysku wysokiej wydajności i małych opóźnień. Aby uzyskać więcej informacji, zobacz [miejsca do magazynowania Premium: wysokiej wydajności miejsca do magazynowania dla obciążenia maszyn wirtualnych Azure](../storage/storage-premium-storage.md) wybierz rozmiar pamięci Wirtualnej, obsługującego premium miejsca do magazynowania. 

- **Utwórz konto osobne miejsca do magazynowania dla każdego maszyn wirtualnych.** Umieść wirtualnych dysków twardych dla jednego maszyn wirtualnych pod uwagę oddzielnie. Pomaga to uniknąć naciśnięcie limity operacji i/o na SEKUNDĘ dla kont miejsca do magazynowania. Aby uzyskać więcej informacji zobacz [skalowalność miejsca do magazynowania Azure i cele](../storage/storage-scalability-targets.md). Jednak w przypadku rozmieszczania dużej liczby maszyny wirtualne, należy pamiętać o limit na subskrypcji dla kont miejsca do magazynowania. Zobacz [limitów miejsca do magazynowania](../azure-subscription-service-limits.md#storage-limits).

- **Tworzenie konta osobne miejsca do magazynowania dla dzienniki diagnostyczne**. Dzienniki diagnostyczne zapisu do tego samego konta miejsca do magazynowania jako wirtualnych dysków twardych, aby uniknąć rejestrowania diagnostycznego nie wpływają na operacji i/o na SEKUNDĘ dysków maszyn wirtualnych. Konto standardowe lokalnie zbędne przestrzeni dyskowej (LRS) jest umożliwiający dzienniki diagnostyczne. 

- **Instalowanie aplikacji na dysku danych nie dysku systemu operacyjnego.** W przeciwnym razie może osiągnąć limit rozmiaru dysku. 

- **Kopia zapasowa Azure umożliwia wykonywanie kopii zapasowej maszyny wirtualne.** Wykonywanie kopii zapasowych chronić się przed utratą danych przypadkowe. Aby uzyskać więcej informacji zobacz [Chronienie maszyny wirtualne Azure z magazynu usługi odzyskiwania](../backup/backup-azure-vms-first-look-arm.md).

- **Włączanie dzienników diagnostycznych**, w tym metryki kondycji podstawowe, dzienniki infrastruktury i [uruchamiania narzędzia diagnostyczne][boot-diagnostics]. Diagnostyka uruchamiania może ułatwić diagnozowanie błąd uruchamiania, jeśli do maszyn wirtualnych otrzymuje stan startowy. Aby uzyskać więcej informacji, zobacz [Omówienie pakietu Azure dzienniki diagnostyczne][diagnostics-logs].

- **Korzystanie z rozszerzeniem AzureLogCollector**. (Tylko VMs systemu Windows). To rozszerzenie agreguje dzienniki platformy Azure i przekazywania ich do magazynu Azure, bez operatora zdalne logowanie do maszyn wirtualnych. Aby uzyskać więcej informacji zobacz [Rozszerzenie AzureLogCollector](../virtual-machines/virtual-machines-windows-log-collector-extension.md).


###  <a name="virtual-network"></a>Wirtualna sieć 

- **Aby listy sprawdzonej lub bloku publicznych adresów IP, Dodaj NSG do podsieci.** Blokowanie dostępu użytkownikom złośliwy lub Zezwalaj na dostęp tylko z użytkowników, którzy mają uprawnienia dostępu do aplikacji.  

- **Tworzenie sondy kondycji niestandardowe.** Sondy kondycji usługi równoważenia obciążenia przetestować HTTP lub TCP. Jeżeli maszyny używa serwera HTTP, sondy HTTP jest lepsza wskaźnik stanu kondycji niż sondy TCP. Sonda HTTP wybierz niestandardowe punktu końcowego, który zgłasza ogólny stan aplikacji, w tym wszystkie krytyczne zależności. Aby uzyskać więcej informacji zobacz [Omówienie równoważenia obciążenia Azure](../load-balancer/load-balancer-overview.md).

- **Nie blokuj sondy kondycji.** Sonda kondycji usługi równoważenia obciążenia jest wysyłana z znanego adresu IP 168.63.129.16. Nie zablokować ruch do lub z tego adresu IP w dowolnej zasady zapory lub zasad grupy (NSG) zabezpieczeń sieci. Blokowanie sondy kondycji spowoduje równoważenia obciążenia usunąć maszyn wirtualnych z obrotu. 

- **Włącz rejestrowanie usługi równoważenia obciążenia.** Dzienniki Pokaż ile maszyny wirtualne na wewnętrznej nie odbierają ruch sieciowy z powodu odpowiedzi sondy nie powiodło się. Aby uzyskać więcej informacji zobacz [Dziennik analizy równoważenia obciążenia Azure](../load-balancer/load-balancer-monitor-log.md).


<!-- links -->
[app-service-autoscale]: ../monitoring-and-diagnostics/insights-how-to-scale.md
[asynchronous-c-sharp]:https://msdn.microsoft.com/library/mt674882.aspx
[availability-sets]:../virtual-machines/virtual-machines-windows-manage-availability.md
[azure-backup]: https://azure.microsoft.com/documentation/services/backup/
[boot-diagnostics]: https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/
[circuit-breaker]: https://msdn.microsoft.com/library/dn589784.aspx
[cloud-service-autoscale]: ../cloud-services/cloud-services-how-to-scale.md
[diagnostics-logs]: ../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md
[fma]: guidance-resiliency-failure-mode-analysis.md
[guidance-resilient-deployment]: guidance-resiliency-overview.md#resilient-deployment
[load-balancer]: load-balancer/load-balancer-overview.md
[monitoring-and-diagnostics-guidance]: ../best-practices-monitoring.md
[resource-manager]: ../azure-resource-manager/resource-group-overview.md
[retry-pattern]: https://msdn.microsoft.com/library/dn589788.aspx
[retry-service-guidance]: ../best-practices-retry-service-specific.md
[search-optimization]: ../search/search-performance-optimization.md
[sql-backup]: ../sql-database/sql-database-automated-backups.md
[sql-restore]: ../sql-database/sql-database-recovery-using-backups.md
[traffic-manager]: ../traffic-manager/traffic-manager-overview.md
[traffic-manager-routing]: ../traffic-manager/traffic-manager-routing-methods.md
[vmss-autoscale]: ../virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview.md
