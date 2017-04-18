<properties
   pageTitle="Wskazówki Autoscaling | Microsoft Azure"
   description="Porady dotyczące autoscale dynamicznie przydzielić zasoby wymagane przez aplikację."
   services=""
   documentationCenter="na"
   authors="dragon119"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="best-practice"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/13/2016"
   ms.author="masashin"/>

# <a name="autoscaling-guidance"></a>Wskazówki dotyczące Autoscaling

[AZURE.INCLUDE [pnp-header](../includes/guidance-pnp-header-include.md)]

## <a name="overview"></a>Omówienie
Autoscaling to proces dynamiczne przydzielanie zasobów wymagane przez aplikację, aby był zgodny wymagania i spełniają umowy poziomu usług (poziomu), przy jednoczesnym minimalizowaniu kosztów obsługi. W miarę zakresu pracy aplikacja może wymagać dodatkowych zasobów, aby umożliwić wykonywanie zadań w odpowiednim czasie. Żądanie slackens, zasobów mogą być cofnięcie przydziału, aby zminimalizować kosztów utrzymania odpowiednią wydajność i spotkania zwiększany.
Autoscaling korzysta z elastyczności hostowana w chmurze środowiskach podczas ułatwienia zarządzanie. Robi to, zmniejszając operatora ciągłe monitorować wydajność systemu i podejmowanie decyzji dotyczących dodawania lub usuwania zasobów.
>[AZURE.NOTE] Autoscaling dotyczy wszystkich zasobów używanych przez aplikację, nie tylko zasoby obliczeń. Na przykład jeśli systemu kolejkach jest używany do wysyłania i odbierania informacji, można utworzyć dodatkowe kolejki trakcie skalowania.

## <a name="types-of-scaling"></a>Typy skalowania
Skalowanie zwykle trwa jednej z następujących dwóch form:

- **Pionowa** (często nazywane _skalowania w górę lub w dół_). Ten formularz wymaga zmodyfikowania sprzęt (rozszerzenia lub zredukowania jego pojemności i wydajności), lub ponownego wdrażania rozwiązania przy użyciu alternatywnych sprzęt, który ma odpowiednie pojemności i wydajności. W środowisku chmury platformy sprzętowej jest zazwyczaj środowisku wirtualizowanym. Chyba że znacznie overprovisioned w oryginalnej sprzętu, z wynikające poświęcić wydatków kapitałowych skalowania pionowo w górę, w tym środowisku obejmuje inicjowania obsługi administracyjnej bardziej zaawansowanych zasobów, a następnie przenosząc systemu na tych nowych zasobów. Skalowanie pionowe często jest procesem kłopotliwe środki, która wymaga wprowadzania w systemie tymczasowo niedostępny w trakcie jest rozmieszczany. Może być możliwe zachować oryginalny systemu podczas nowego sprzętu ponieważ jest ono inicjowane i wyświetlanie w trybie online, ale będzie prawdopodobnie niektóre przerwy podczas przejścia przetwarzanie ze starej środowiska do nowej. Jest nietypowych za pomocą autoscaling wdrożenie strategii skalowania pionowego.
- **Pozioma** (często nazywane _skalowania i_). Ten formularz wymaga wdrożenia rozwiązanie na dodatkowe lub mniej zasobów, które są zwykle zasobów towaru zamiast silnych systemów. Rozwiązanie nadal może działać bez zakłóceń podczas zainicjowano te zasoby. Po zakończeniu procesu obsługi administracyjnej kopii elementów, które składają się rozwiązania można wdrożone na dodatkowych zasobów i udostępnione. Jeśli pomija żądanie, dodatkowe zasoby można odzyskać po elementów za pomocą ich zostać zamknięty czysto. Wiele opartej na chmurze, w tym Microsoft Azure systemy automatyzacji ten formularz skalowania.

## <a name="implement-an-autoscaling-strategy"></a>Wdrożenie strategii autoscaling
Zazwyczaj wdrażaniu strategii autoscaling obejmuje następujące składniki i procesów:

- Oprzyrządowania i monitorowanie systemów na poziomie aplikacji, usług i infrastruktury. Poniższe systemy Przechwytywanie kluczowych miar, takich jak czasy odpowiedzi, długości kolejki procesora i pamięci.
- Logika podejmowania decyzji, można ocenianie współczynniki skalowania monitorowane przed progi system wstępnie zdefiniowany lub harmonogramów i podejmowanie decyzji o tym, czy przeskalować lub nie.
- Składniki, które są odpowiedzialne za przeprowadzenie zadania związane z skalowania systemu, takich jak inicjowania obsługi administracyjnej lub Anuluj inicjowania obsługi administracyjnej zasobów.
- Testowanie, monitorowanie i dostosowywanie strategii autoscaling, aby upewnić się, że działa zgodnie z oczekiwaniami.

Większości środowisk opartej na chmurze, takiej jak Azure, zawiera wbudowane autoscaling mechanizmy typowe scenariusze tego adresu. Jeśli środowisko lub usługi, których nie udostępnia odpowiednie automatyczne skalowanie funkcji lub jeśli masz wymagania skrajnych autoscaling poza możliwości niestandardowej implementacji może być konieczne. Za pomocą tej implementacji niestandardowych zbieranie operacyjne i systemu miar, analizowania ich do identyfikowania odpowiednie dane i następnie odpowiednio skalowanie zasobów.


## <a name="configure-autoscaling-for-an-azure-solution"></a>Konfigurowanie autoscaling Azure rozwiązania
Istnieje kilka opcji do konfigurowania autoscaling dla usługi Azure rozwiązań:

- **Azure Autoscale** obsługuje najbardziej typowe scenariusze skalowania zgodnie z harmonogramem i opcjonalnie wyzwalane skalowania operacje na podstawie metryk runtime (na przykład wykorzystanie procesora, długość kolejki lub liczniki wbudowane i niestandardowe). Można skonfigurować zasady autoscaling prostego rozwiązania szybko i łatwo za pomocą portalu Azure. Aby uzyskać bardziej szczegółowe kontrolki, można wprowadzić za pomocą [Interfejsu API usługi REST zarządzania usługi Azure](https://msdn.microsoft.com/library/azure/ee460799.aspx) lub [Azure interfejsu API usługi REST Menedżera zasobów](https://msdn.microsoft.com//library/azure/dn790568.aspx). [Biblioteka zarządzania usług Azure monitorowania](http://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Monitoring) i [Microsoft wniosków biblioteki](https://www.nuget.org/packages/Microsoft.Azure.Insights/) (w wersji preview) są SDK, które zezwalają zbieranie metryki z różnych zasobów, a następnie wykonywać autoscaling, wykorzystując interfejsów API pozostałych. Dla zasobów w przypadku, gdy obsługa Menedżera zasobów Azure nie jest dostępna, czy korzystasz z usług w chmurze Azure interfejsu API usługi REST zarządzania usługi może być używany do autoscaling. We wszystkich innych przypadkach za pomocą Menedżera zasobów Azure.
- **Niestandardowe rozwiązania**, oparte na sieci oprzyrządowania na pasku aplikacji, a funkcje zarządzania Azure, może być przydatne. Na przykład można diagnostyki Azure lub inne metody oprzyrządowania w aplikacji, wraz z kodu niestandardowego w celu ciągłe monitorowanie i eksportowanie metryki aplikacji. Może mieć niestandardowe reguły, które działają na tych metryki i korzystać z Zarządzanie usługą lub interfejsu API usługi REST Menedżera zasobów jest wyzwalać autoscaling. Metryki dla powodujące operacji skalowania może być dowolny liczników wbudowane lub niestandardowe lub innych oprzyrządowania, które wdrożenie aplikacji. Jednak niestandardowym nie jest łatwe do zaimplementowania i powinny być traktowane tylko jeśli żaden z poprzednich metod spełnienia wymagań. [Blokowanie aplikacji Autoscaling](http://msdn.microsoft.com/library/hh680892%28v=pandp.50%29.aspx) korzysta z tej metody.
- **Usługi innych firm**, takie jak [Paraleap AzureWatch](http://www.paraleap.com/AzureWatch)umożliwiają skalowanie rozwiązaniem opartym na harmonogramów, wskaźniki wydajności usługi systemu i ładowanie niestandardowych reguł i kombinacje różnych rodzajów reguł.

Przy wyborze rozwiązania, które autoscaling do przyjęcia, rozważ następujące wskazówki:

- Korzystanie z funkcji wbudowanych autoscaling platformy, jeśli można dostosować do własnych potrzeb. Jeśli nie, należy rozważyć, czy naprawdę potrzebujesz bardziej złożonych funkcji skalowania. Przykłady dodatkowe wymagania może zawierać więcej szczegółowości kontroli, różne sposoby wykrywania zdarzenia wyzwalacza skalowania, skalowania w subskrypcjach i skalowanie innych typów zasobów.
- Należy rozważyć, jeśli istnieje przewidywania obciążenie aplikacji z wystarczającą dokładnością tylko zależeć autoscaling według harmonogramu (Dodawanie i usuwanie wystąpienia spotkania przewidywane wartości na żądanie). Miejsce, w którym nie jest to możliwe, użyj reaktywne autoscaling na podstawie metryk zbierane w czasie rzeczywistym, aby umożliwić aplikacji do obsługi nieprzewidywalny zmian w żądanie. Zazwyczaj można połączyć z tych metod. Na przykład utworzyć strategii, która powoduje dodanie zasobów, takich jak kolejki, zgodnie z harmonogramem godzin, gdy wiadomo, że aplikacja jest najbardziej zajęty obliczeń i magazynowania. Dzięki temu, aby upewnić się, że wydajność jest dostępna, gdy wymagane bez opóźnień podczas uruchamiania nowych wystąpień. Ponadto dla każdej reguły według harmonogramu, należy zdefiniować metryki umożliwiająca reaktywne autoscaling w tym okresie, aby upewnić się, że aplikacja może obsługiwać wartości stałej, ale nieprzewidywalny na żądanie.
- Często jest trudne do zrozumienia relacji między metryki i pojemnością wymagań, zwłaszcza w przypadku, gdy początkowego wdrażania aplikacji. Wolisz obsługi administracyjnej nieco dodatkowej pojemności na początku, a następnie monitorowania i dostosowywania reguł autoscaling, aby przenieść możliwości bliżej rzeczywiste obciążenie.

### <a name="use-azure-autoscale"></a>Używanie Azure Autoscale
Autoscale umożliwia konfigurowanie skali się i skalowanie w opcjach rozwiązania. Autoscale można automatycznie dodawać i usuwać wystąpień usług w chmurze Azure sieci web i ról pracownik, usługi Azure Mobile i funkcji aplikacji sieci Web w usłudze Azure w aplikacji. Można także włączyć, automatyczne skalowanie przez współczynnik równy uruchamianie i zatrzymywanie wystąpienia maszyn wirtualnych Azure. Strategii Azure autoscaling zawiera dwa zestawy czynników:

- Oparte na harmonogram autoscaling, który zapewnia dodatkowe wystąpienia są dostępne się z oczekiwanych Szczyt użycie i można skalować w po upływie czasu Szczyt. Umożliwia upewnij się, że masz wystarczających wystąpienia już uruchomiony, nie czekając na reagowanie na obciążenia systemu.
- Autoscaling podstawie metryki, który w czynników, takich jak średnia użycie Procesora przez godzinę ostatniej ani zaległości wiadomości, które rozwiązanie jest przetwarzania Azure miejsca do magazynowania lub w kolejce Bus usługi Azure przypadku. Dzięki aplikacjom reagować niezależnie od zasad autoscaling według harmonogramu, aby zezwalały na niezaplanowane lub nieprzewidzianych zmian w żądanie.

Podczas korzystania z Autoscale, rozważ następujące wskazówki:

- Strategii autoscaling łączy według harmonogramu a podstawie metryki skalowania. Możesz określić obu rodzajów reguł usługi.
- Należy skonfigurować reguły autoscaling, a następnie można monitorować wydajność aplikacji w czasie. Dostosowywanie sposobu skale systemu w razie potrzeby przy użyciu wyników monitorowania. Jednak kontynuować Pamiętaj, że autoscaling nie jest chwilową proces. Czas reakcji na metryczne, takich jak średnia Procesora wykorzystania przekraczają (lub poniżej) określony próg.
- Reguły Autoscaling, które korzysta z mechanizmu wykrywania na podstawie atrybutu mierzone wyzwalacza (na przykład zastosowania lub kolejki długość Procesora) umożliwia zagregowaną wartość przez godzinę, zamiast wartości chwilową wyzwalanie akcji autoscaling. Domyślnie agregacji stanowi średnią wartości. Dzięki temu system z reakcji zbyt szybko lub powodujące szybkich oscylacji. Umożliwia także czasu dla nowego wystąpienia, które są automatycznie uruchomiony do rozliczenia do uruchamiania tryb zapobiegania autoscaling dodatkowe akcje z występujące podczas uruchamiania nowych wystąpień. Dla usług w chmurze Azure i maszyn wirtualnych Azure, domyślny okres dla agregacji jest 45 minut, dlatego może potrwać do tego okresu czasu metryki wyzwalać autoscaling w odpowiedzi na tych najwyższych wartościach na żądanie. Można zmienić okres rozliczeniowy przy użyciu zestawu SDK, ale należy pamiętać, że może powodować nieoczekiwane wyniki, okresów mniej niż 25 minut (Aby uzyskać więcej informacji, zobacz [Automatyczne skalowanie usług w chmurze z wartością procentową Procesora z biblioteką zarządzania usługami monitorowania w Azure](http://rickrainey.com/2013/12/15/auto-scaling-cloud-services-on-cpu-percentage-with-the-windows-azure-monitoring-services-management-library/)). W przypadku aplikacji sieci Web okresu uśredniania jest dużo krótsza, umożliwiając nowych wystąpień mają być dostępne w około pięć minut po zmianie do środka wyzwalacza średnia.
- Jeśli skonfigurujesz autoscaling przy użyciu zestawu SDK, a nie w portalu sieci web, możesz określić bardziej szczegółowe harmonogram, w którym są aktywne reguł. Można również utworzyć własne metryki i używać ich z tekstem lub bez tych istniejące w reguł autoscaling. Na przykład możesz korzystać z innych liczników, takie jak liczba żądań na sekundę lub dostępności pamięci średnia, lub Użyj liczników niestandardowych mierzące określonym procesom biznesowym.
- Gdy autoscaling maszyn wirtualnych Azure, należy wdrożyć liczbę wystąpień maszyny wirtualnej, która jest równa maksymalnej liczby umożliwia autoscaling rozpocząć. Te wystąpienia musi być częścią ten sam zestaw dostępności. Mechanizm autoscaling maszyn wirtualnych nie Tworzenie lub usuwanie wystąpienia maszyn wirtualnych; Zamiast tego reguły autoscaling, które możesz skonfigurować będzie uruchamiać i zatrzymywać odpowiednią liczbę te wystąpienia. Aby uzyskać więcej informacji zobacz [Automatyczne skalowanie aplikacji uruchomionej role w sieci Web, role pracownik lub maszyn wirtualnych](./cloud-services/cloud-services-how-to-scale.md).
- Jeśli nie można uruchomić nowych wystąpień, prawdopodobnie została osiągnięta maksymalna dla subskrypcji lub wystąpi błąd podczas uruchamiania portalu mogą pokazywać że operacji autoscaling zakończyła się pomyślnie. Jednak kolejne zdarzenia **ChangeDeploymentConfiguration** wyświetlane w portalu będzie widoczne tylko, że zażądano uruchomienia usługi i będzie żadne zdarzenie wskazuje, że została pomyślnie ukończona.
- Interfejs użytkownika portalu sieci web umożliwia połączyć zasobów, takich jak wystąpienia bazy danych SQL i kolejki wystąpienie usługi obliczeń. Dzięki temu będzie można łatwiej uzyskać dostęp do oddzielnego ręczne i automatyczne skalowanie opcje konfiguracji dla każdego z połączonych zasobów. Aby uzyskać więcej informacji, zobacz [jak: łączenie zasobu do usługi w chmurze](cloud-services-how-to-manage.md#linkresources) i [sposobu skalowania aplikacji](./cloud-services/cloud-services-how-to-scale.md).
- Po skonfigurowaniu wielu zasad i reguł, może powodować konflikt ze sobą. Autoscale używane następujące reguły rozwiązywania konfliktów, aby upewnić się, że jest zawsze odpowiednią liczbę wystąpień:
  - Skala operacje zawsze pierwszeństwo skali w operacji.
  - Podczas skalowania operacje konflikt, pierwszeństwo ma reguła, który inicjuje wzrostu największa liczba wystąpień.
  - Kiedy skalować w konflikcie operacji, reguły, która inicjuje zmniejszenie najmniejszą liczbę wystąpień ma pierwszeństwo.

<a name="the-azure-monitoring-services-management-library"></a>

## <a name="application-design-considerations-for-implementing-autoscaling"></a>Uwagi dotyczące projektowania aplikacji stosowania autoscaling
Autoscaling nie jest błyskawiczne rozwiązanie. Po prostu Dodawanie zasobów do systemu lub uruchomione wystąpienia więcej procesu nie daje gwarancji zwiększy wydajność systemu. Podczas projektowania strategii autoscaling, należy uwzględnić następujące punkty:

- System muszą być zaprojektowane jako poziomie skalowalność. Unikaj nawiązywania założenia dotyczące Koligacja wystąpienia; nie Projektowanie rozwiązania, które wymagają, że kod zawsze działa konkretnego wystąpienia procesu. Gdy skalowania w poziomie usługi w chmurze lub witryny sieci web, nie przyjęto założenie, że seria żądań z tego samego źródła, zawsze będą kierowane do tego samego wystąpienia. Z tego samego powodu Projektowanie usług jako bezstanowy pomijane jest wymaganie seria żądań z aplikacji, aby zawsze będą kierowane do tego samego wystąpienia usługi. Projektując usługa, która przeczyta wiadomości z kolejki i przetwarza je, nie wprowadzaj założeń, o których wystąpienie uchwyty usługi określonej wiadomości. Autoscaling można uruchomić dodatkowe wystąpienia usługi w miarę długość kolejki. [Wzór konsumentów uczestniczących w zawodach](http://msdn.microsoft.com/library/dn568101.aspx) opisano sposoby radzenia w tym scenariuszu.
- Jeśli rozwiązanie wykonuje zadania długotrwałe, projektowanie to zadanie do obsługi Skalowanie zewnętrzne i skalowanie w. Bez ukończenia care, takie zadania może uniemożliwić zamykana czysto po systemu skale w wystąpienie procesu lub może spowodować utratę danych, jeśli wymusić zakończenia procesu. Najlepiej, jeśli refactor zadania długim i podzielić przetwarzania wykonującego do mniejszą fragmentów. [Potoki i deseniu filtry](http://msdn.microsoft.com/library/dn568100.aspx) znajdują się z przykładem jak można to osiągnąć.
- Można także zaimplementować mechanizmu punkt kontrolny podać informacje o zadaniu w regularnych odstępach rekordów i Zapisz ten stan w trwały magazyn, które mogą uzyskiwać dostęp do dowolne wystąpienie procesu uruchomionego zadania. W ten sposób Jeśli ten proces został zamknięty, pracy, do której został on wykonywanie można wznowić z ostatniego punktu kontrolnego za pomocą innego wystąpienia.
- Uruchomienie zadania w tle na wystąpienia osobnych obliczeń, takich jak w pracownik ról usług w chmurze hostowanej aplikacji może być konieczne skalowanie różne części aplikacji przy użyciu różnych zasad skalowania. Na przykład może być konieczne wdrażanie obliczeń interfejsu użytkownika dodatkowe wystąpienia bez zwiększenie liczby tła obliczyć wystąpienia lub przeciwieństwem to. Jeśli różnych poziomów usług (na przykład pakiety usług podstawowego i specjalnego), może być konieczne skalowania zasobów obliczeń dla pakietów usług premium bardziej odważnie niż pakietów podstawowej usługi w celu spełnienia zwiększany.
- Warto rozważyć użycie długość kolejki, w którym interfejsu użytkownika i tła wyliczenia wystąpień komunikowanie się jako kryterium dla strategii autoscaling. Jest to zalecane wskaźnik równowagi lub różnica między bieżące obciążenie i możliwości przetwarzania zadania w tle.
- Jeśli strategii autoscaling na liczniki mierzące procesów biznesowych, takie jak liczba zamówień na godzinę lub Średni czas wykonywania złożonych transakcji, upewnij się, lepiej zrozumieć powiązanie wyniki te typy liczników i wymagania dotyczące wydajności obliczeń rzeczywiste. Może być konieczne przeskalować więcej niż jeden składnik lub obliczyć jednostkę w odpowiedzi na zmiany w liczników procesów biznesowych.  
- Aby zapobiec dołącza do skalowania nadmiernie oraz w celu uniknięcia koszty skojarzone z uruchomionymi tysiące wystąpienia systemu, warto rozważyć ograniczenie maksymalną liczbę wystąpień, które mogą być automatycznie dodawane. Większość mechanizmy autoscaling umożliwiają określenie minimalną i maksymalną liczbę wystąpień reguły. Ponadto należy rozważyć bezpiecznie obniżeniu funkcji system zapewnia maksymalną liczbę wystąpień zostały wdrożone czy nadal nadmiernie systemu.
- Zachowaj pamiętać tej autoscaling może nie być bardziej odpowiednia mechanizmu obsługi szybkiego serii w obciążenie pracą. Czas obsługi administracyjnej Rozpoczynanie nowych wystąpień usług i dodawanie zasobów do systemu, a żądanie Szczyt może przekazano do czasu, który dodatkowych zasobów udostępniono. W tym scenariuszu może być lepsze ograniczania usługę. Aby uzyskać więcej informacji zobacz [Ograniczanie deseniem](http://msdn.microsoft.com/library/dn589798.aspx).
- I odwrotnie jeśli jest potrzebna możliwości przetwarzania wszystkie żądania, gdy wielkość zmienia się szybko i koszt nie główne czynniki uczestniczące, warto rozważyć użycie strategii autoscaling rygorystyczne uruchamiające dodatkowe wystąpienia szybciej. Umożliwia także zasady według harmonogramu, rozpoczynająca się wystarczające liczbę wystąpień spotkania maksymalne obciążenie przed ciężar ten jest planowane.
- Mechanizm autoscaling należy monitorować proces autoscaling i Zarejestruj szczegóły każde zdarzenie autoscaling (co wyzwalane go, dodane lub usunięte, zasobów i czasu). Jeśli tworzysz mechanizm autoscaling niestandardowe, upewnij się, zawiera tej funkcji. Analizowanie informacji ułatwiających miary skuteczność strategii dotyczącej autoscaling oraz go dostosować, jeśli to konieczne. Można dostosować zarówno w krótkim, jak upodobania stają się wyraźniejsze i powyżej długim rozwija firmy lub wymagań aplikacji są obsługiwane. Jeśli aplikacja osiągnie górną granicę zdefiniowanych dla autoscaling, mechanizmu może również alert operator, kto może ręcznie uruchomić na dodatkowych zasobów, jeśli to konieczne. Należy pamiętać, że w tych okolicznościach, operator może być również odpowiedzialne za ręcznie usunąć te zasoby po ułatwia obciążenie pracą.

## <a name="related-patterns-and-guidance"></a>Wskazówki i desenie pokrewne
Następujące wskazówki i desenie mogą również odnosić się do rozwiązania podczas wykonywania autoscaling:

- [Ograniczanie deseniem](http://msdn.microsoft.com/library/dn589798.aspx). Ten wzorzec opisano, jak aplikacja może kontynuować działać i spełniają zwiększany, gdy wzrost żądanie umieści skrajnych obciążenia zasobów. Ograniczanie umożliwia z autoscaling systemu przed zbyt wyeliminowania systemu skale się.
- [Uczestniczących w zawodach deseniu konsumentów](http://msdn.microsoft.com/library/dn568101.aspx). Ten wzorzec opisano, jak zaimplementować puli wystąpień usługi, obsługujące wiadomości w każdym wystąpieniu aplikacji. Autoscaling może służyć do rozpoczynania i kończenia wystąpień usługi zgodnie z pracą przewidywanych. Ta metoda umożliwia systemowi w celu przetwarzania wielu wiadomości jednocześnie do optymalizacji wydajności, zwiększyć dostępność i skalowalność i równoważenia obciążenia.
- [Wskazówki dotyczące telemetrycznego i oprzyrządowania](http://msdn.microsoft.com/library/dn589775.aspx). Oprzyrządowania i telemetrycznego są istotne do zbierania informacji, które można dysku procesu autoscaling.

## <a name="more-information"></a>Więcej informacji
- [Jak skala aplikacji](./cloud-services/cloud-services-how-to-scale.md)
- [Automatyczne skalowanie aplikacji uruchomionej role w sieci Web, role pracownik lub maszyn wirtualnych](cloud-services-how-to-manage.md#linkresources)
- [Jak: łączenie zasobu do usługi w chmurze](cloud-services-how-to-manage.md#linkresources)
- [Skalowanie połączonych zasobów](./cloud-services/cloud-services-how-to-scale.md#scale-link-resources)
- [Monitorowanie Biblioteka zarządzania usług Azure](http://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Monitoring)
- [Zarządzanie usługą Azure interfejsu API usługi REST](http://msdn.microsoft.com/library/azure/ee460799.aspx)
- [Azure interfejsu API usługi REST Menedżera zasobów](https://msdn.microsoft.com/library/azure/dn790568.aspx)
- [Biblioteka Insights firmy Microsoft](https://www.nuget.org/packages/Microsoft.Azure.Insights/)
- [Operacje na Autoscaling](http://msdn.microsoft.com/library/azure/dn510374.aspx)
- [Namespace Microsoft.WindowsAzure.Management.Monitoring.Autoscale](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.management.monitoring.autoscale.aspx)