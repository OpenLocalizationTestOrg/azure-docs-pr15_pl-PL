<properties
    pageTitle="Monitorowanie aplikacji w usłudze Azure aplikacji"
    description="Dowiedz się, jak monitorowanie aplikacji w usłudze Azure aplikacji przy użyciu Azure Portal."
    services="app-service"
    documentationCenter=""
    authors="btardif"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/07/2016"
    ms.author="byvinyal"/>

# <a name="how-to-monitor-apps-in-azure-app-service"></a>Jak: monitorowanie aplikacji w usłudze Azure aplikacji

[Usługa aplikacji](http://go.microsoft.com/fwlink/?LinkId=529714) zapewnia wbudowanych funkcji monitorowania w [Azure Portal](https://portal.azure.com).
Dotyczy to także możliwość przeglądanie **przydziałów** i **metryki** dla aplikacji, a także plan usług aplikacji, aby skonfigurować **alerty** , a nawet **skalowania** automatycznie na podstawie tych metryk.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="understanding-quotas-and-metrics"></a>Opis bezproblemowa współpraca i

### <a name="quotas"></a>Przydziałów

Aplikacje obsługiwane w aplikacji usługi będą podlegać pewne *ograniczenia* zasobów, które mogą również używać. Limity są definiowane przez **plan usług aplikacji** skojarzonych z tej aplikacji.

Jeśli aplikacja jest obsługiwana w planie **wolny** lub **udostępnione** , ograniczenia dotyczące zasobów, których można używać aplikacji są definiowane przez **przydziałów**.

Jeśli aplikacja jest obsługiwana w **podstawowej**, plan **Standardowy** lub **Premium** , a następnie limity na zasoby, które mogą również używać są ustawiane przez **rozmiar** (małe, średnie, duże) i **wystąpienia liczba** (1, 2, 3,...) **plan usług aplikacji**.

**Przydziałów** dla aplikacji **wolny** lub **udostępnione** są:

* **CPU(Short)**
   * Liczba procesorów niedozwolone dla tej aplikacji w 3 minut. Przydział ponownie ustawia co 3 minuty.
* **CPU(Day)**
   * Suma Procesora dozwolone dla tej aplikacji w ciągu dnia. Przydział ponownie ustawia północy UTC raz na 24 godziny.
* **Pamięci**
   * Całkowita liczba pamięci niedozwolone dla tej aplikacji.
* **Przepustowości**
   * Suma wychodzących przepustowość dozwolona dla tej aplikacji w ciągu dnia.
   Przydział ponownie ustawia co 24 godziny północy UTC.
* **System plików**
   * Całkowita ilość miejsca do magazynowania dozwolone.

Tylko przydziałów, które dotyczą aplikacji hostowanej **podstawowe**, **standardowych** i plany **Premium** jest **system plików**.

Więcej informacji na temat określonej kwoty, limity i funkcje dostępne dla różnych wersji produktu usługi aplikacji można znaleźć tutaj: [Limity usługi Azure subskrypcji](../azure-subscription-service-limits.md#app-service-limits)

#### <a name="quota-enforcement"></a>Wymuszanie przydziałów

Jeśli aplikację w jego użyciem przekracza **Procesora (skrót)**, **Procesora (dzień)**lub przydział **przepustowości** następnie aplikacji zostaną zatrzymane, dopóki nie określi ponownie przydziału. W tym czasie spowoduje wszystkie przychodzące żądania **HTTP 403**.
![][http403]

Jeśli przydziału **pamięci** aplikacji zostanie przekroczony, aplikacja zostaną ponownie uruchomiony.

Jeśli zostanie przekroczony przydział **plików** , następnie dowolną Napisz operacja zakończy się niepowodzeniem, ta opcja uwzględnia zapisywanie dzienników.

Przydziałów można zwiększyć lub usuwane z Twojej aplikacji przez uaktualnienie planu obsługi aplikacji.

### <a name="metrics"></a>Metryki

**Metryki** zawierają informacje o aplikacji lub zachowanie plan usług aplikacji.

Dla **aplikacji**są dostępne wskaźniki:

* **Średni czas reakcji**
   * Średni czas aplikacji do obsługi żądania w ms.
* **Średnia pamięć zestaw roboczy**
   * Średnia liczba pamięci bazy MIB używane przez aplikację.
* **Czas Procesora**
   * Liczba procesorów w sekundach zużywanej przez aplikację. Aby uzyskać więcej informacji na ten temat, metryki zobacz: [Procent Procesora CPU czasu w porównaniu z](#cpu-time-vs-cpu-percentage)
* **Dane w**
   * Liczba przychodzących przepustowości za pomocą aplikacji w bazy MIB.
* **Danych**
   * Liczba wychodzących przepustowości za pomocą aplikacji w bazy MIB.
* **Http 2xx**
   * Liczba żądań uzyskując kodu stanu http > = 200, ale < 300.
* **Http 3xx**
   * Liczba żądań uzyskując kodu stanu http > = 300, ale < 400.
* **HTTP 401**
   * Liczba żądań uzyskując kodu stanu HTTP 401.
* **HTTP 403**
   * Liczba żądań uzyskując kodu stanu HTTP 403.
* **HTTP 404**
   * Liczba żądań uzyskując kodu stanu HTTP 404.
* **HTTP 406**
   * Liczba żądań uzyskując kodu stanu HTTP 406.
* **Http 4xx**
   * Liczba żądań uzyskując kodu stanu http > = 400, ale < 500.
* **Błędy serwera HTTP**
   * Liczba żądań uzyskując kodu stanu http > = 500, ale < 600.
* **Zestaw roboczy pamięci**
   * Bieżąca ilość pamięci używanej przez aplikację w bazy MIB.
* **Żądania**
   * Całkowita liczba żądań niezależnie od ich utworzony kod stanu HTTP.

**Plan usług aplikacji**są dostępne wskaźniki:

>[AZURE.NOTE] Metryki plan usługi aplikacji są dostępne tylko dla planów w **podstawowych**, **Standard** i **Premium** SKU.

* **Procesor procent**
   * Średnia Procesora używany we wszystkich wystąpieniach planu.
* **Procent ilości pamięci**
   * Pamięć średnia, używana we wszystkich wystąpieniach planu.
* **Dane w**
   * Średnia przepustowość przychodząca używany we wszystkich wystąpieniach planu.
* **Danych**
   * Średnia wychodzącej przepustowość we wszystkich wystąpieniach planu.
* **Długość kolejki dysku**
   * Średnia liczba wystąpień zarówno żądania odczytu i zapisu znajdujące się w kolejce ilość miejsca do magazynowania. Długość kolejki dysku jest wskazanie aplikacji, która może być zmniejszają ze względu na dysku nadmiarowe.
* **Długość kolejki http**
   * Średnia liczba żądań HTTP, które miały usiąść w kolejce przed wypełniane. Wysoki lub rosnącymi długość kolejki HTTP jest objawem planu obciążony.

### <a name="cpu-time-vs-cpu-percentage"></a>Procent w porównaniu z Procesora CPU czasu
<!-- To do: Fix Anchor (#CPU-time-vs.-CPU-percentage) -->

Istnieją 2 metryki, które odzwierciedlają użycie Procesora. **Czas Procesora** i **Procent Procesora**

**Czas Procesora** jest przydatne w przypadku aplikacji obsługiwany w planów **wolny** lub **udostępnione** , ponieważ ich kwot jest zdefiniowana w minutach procesorów używanych przez aplikację.

**Wartość procentowa Procesora** z drugiej strony jest przydatne w przypadku aplikacji obsługiwany w plany **podstawowe**, **standard** i **premium** , ponieważ może być skalowany się i tej metryki jest dobrym wskaźnikiem ogólnego zastosowania we wszystkich wystąpieniach.

##<a name="metrics-granularity-and-retention-policy"></a>Metryki szczegółowości i zasady przechowywania

Metryki dla aplikacji i plan usług aplikacji są rejestrowane i agregacją przez usługę z następujących stopnie szczegółowości i zasady przechowywania:

 * **Minuta** szczegółowości metryki są zachowywane **48** godzin
 * **Godzina** szczegółowości metryki są zachowywane przez **30 dni**
 * Metryki szczegółowości **dnia** są zachowywane przez **90 dni**

## <a name="monitoring-quotas-and-metrics-in-the-azure-portal"></a>Monitorowanie przydziałów i wskaźniki w portalu Azure.

Możesz przejrzeć stan różnych **przydziałów** i **metryki** wpływu aplikacji w [Azure Portal](https://portal.azure.com).

![][quotas]
**Przydziałów** można znaleźć w obszarze Ustawienia >**przydziałów**. Interfejsu użytkownika umożliwia przeglądanie: (1) nazwę przydziałów, (2) jej interwał resetowania, (3) jej limit bieżącego i (4) bieżącej wartości.

![][metrics]
**Metryki** może mieć dostęp bezpośrednio z karta zasobu. Można także dostosować wykres z: (1) **kliknij pozycję** , a także (2) wybierz pozycję **Edytuj wykresu**.
W tym miejscu możesz zmienić **zakres czasu**(3), (4) **Typ wykresu**i (5) **metryki** do wyświetlenia.  

Dowiedz się więcej o metryki tutaj: [Monitorowanie usługi metryki](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

## <a name="alerts-and-autoscale"></a>Alerty i Autoscale
Metryki dla plan aplikacji lub usługi aplikacji może być złapać do alerty, aby dowiedzieć się więcej na ten temat, zobacz [odbieranie alertów](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

Obsługiwany w podstawowego, standardowy lub specjalnego aplikacji usług plany aplikacji aplikacji usługi pomocy technicznej **autoscale**. Gdy aplikacja jest nadmierne świadczenia dzięki temu można skonfigurować reguły, które można monitorować metryki plan aplikacji usługi i można zwiększyć lub zmniejszyć licznik wystąpień, dostarczając dodatkowych zasobów, stosownie do potrzeb lub zapisywanie pieniądze. Dowiedz się więcej o automatyczne skalowanie tutaj: [jak skala](../monitoring-and-diagnostics/insights-how-to-scale.md) i Oto [Najważniejsze wskazówki dotyczące autoscaling Azure Monitor](../monitoring-and-diagnostics/insights-autoscale-best-practices.md)

>[AZURE.NOTE] Jeśli chcesz rozpocząć pracę z Azure aplikacji usługi przed utworzeniem konta dla konta Azure, przejdź do [Spróbuj aplikacji usługi](http://go.microsoft.com/fwlink/?LinkId=523751), którym natychmiast można utworzyć aplikację sieci web krótkotrwałe starter w aplikacji usługi. Nie kart kredytowych wymagane; nie zobowiązania.

## <a name="whats-changed"></a>Informacje o zmianach
* Przewodnika do zmiany z witryn sieci Web do usługi aplikacji Zobacz: [Usługa Azure aplikacji i jego wpływ na istniejące usługi Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

[fzilla]:http://go.microsoft.com/fwlink/?LinkId=247914
[vmsizes]:http://go.microsoft.com/fwlink/?LinkID=309169



<!-- Images. -->
[http403]: ./media/web-sites-monitor/http403.png
[quotas]: ./media/web-sites-monitor/quotas.png
[metrics]: ./media/web-sites-monitor/metrics.png
