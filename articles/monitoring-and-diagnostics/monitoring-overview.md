<properties
    pageTitle="Omówienie monitorowanie platformy Microsoft Azure | Microsoft Azure"
    description="Najwyższego poziomu omówienie monitorowania i diagnostyki platformy Microsoft Azure tym alertów, webhooks, autoscale i inne."
    authors="rboucher"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"l
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/11/2016"
    ms.author="robb"/>

# <a name="overview-of-monitoring-in-microsoft-azure"></a>Omówienie monitorowanie platformy Microsoft Azure

Ten artykuł zawiera omówienie monitorowania Azure zasobów. Udostępnia łącza do informacji w określonych typów zasobów.  Dla wysokiego poziomu informacji na temat aplikacji z wartością Azure punktu widzenia monitorowania zobacz [wskazówki na temat monitorowania i diagnostyczne](../best-practices-monitoring.md).

Aplikacje w chmurze są złożone z dużej liczbie ruchomych elementów. Monitorowanie zawiera dane, aby upewnić się, że aplikacja pozostaje w górę i uruchamianie prawidłowy stan. Pomaga także stave potencjalne problemy i rozwiązywanie problemów z wcześniejszych etapach. Ponadto możesz użyć monitorowania danych do uzyskania głębokości wniosków o aplikacji. Wiedzy, można pomóc w celu zwiększenia wydajności aplikacji lub łatwość lub zautomatyzować akcje, które w przeciwnym razie wymagają ręczne.

Na poniższym diagramie przedstawiono poglądowy widok Azure monitorowania, łącznie z typem dzienniki, które może zbierać i co można zrobić z tymi danymi.   

![Modelu logicznego monitorowania i informacje diagnostyczne dla zasobów bez obliczeń](./media/monitoring-overview/MonitoringAzureResources-non-compute_v3.png)

Rysunek 1: Model koncepcyjny monitorowania i informacje diagnostyczne dla zasobów bez obliczeń

<br/>

![Modelu logicznego monitorowania i informacje diagnostyczne dla zasobów obliczeń](./media/monitoring-overview/MonitoringAzureResources-compute_v3.png)

Rysunek 2: Model koncepcyjny monitorowania i informacje diagnostyczne dla zasobów obliczeń


## <a name="monitoring-sources"></a>Monitorowanie źródeł
### <a name="activity-logs"></a>Dzienniki aktywności
Dziennik (wcześniej nazywane dzienników inspekcji lub działa) informacji można znaleźć o zasób jak było widać Azure infrastruktury. Dziennik zawiera informacje, takie jak godziny, kiedy zasoby są tworzone lub zniszczone.  

### <a name="host-vm"></a>Host maszyn wirtualnych
**Obliczanie tylko**


Niektóre obliczenia zasoby takie jak usług w chmurze, maszyn wirtualnych i tkaninie usługi mają dedykowane maszyn wirtualnych hosta współdziałają z. Maszyn wirtualnych hosta odpowiada maszyn wirtualnych głównego w modelu monitor maszyny wirtualnej funkcji Hyper-V. W tym przypadku może zbierać metryki w tylko maszyn wirtualnych hosta oprócz system operacyjny gościa.  

Dla innych usług Azure nie jest zawsze 1:1 mapowania między zasobu i określonego maszyny hosta dzięki czemu metryki maszyn wirtualnych hosta nie są dostępne.


### <a name="resource---metrics-and-diagnostics-logs"></a>Zasób - metryki i dzienniki diagnostyczne
Możliwe do zgromadzenia metryki różnią się w zależności od typu zasobu. Na przykład maszyn wirtualnych zawiera statystyki na dysku Jo i Procesora procent. Ale te statystykę nie istnieje dla kolejki Bus usługi, zamiast tego zawiera miar, takich jak kolejki przepustowość rozmiar i wiadomości.

Dla zasobów obliczeń można uzyskać metryki dla modułów system operacyjny gościa i diagnostyce, takich jak diagnostyki Azure. Diagnostyka Azure ułatwia gromadzenie i kierowanie Zbierz dane diagnostyczne w innych lokalizacjach, w tym Azure miejsca do magazynowania.

Lista obecnie możliwe do zgromadzenia metryki jest dostępna na [obsługiwane metryki](monitoring-supported-metrics.md).

### <a name="application---diagnostics-logs-application-logs-and-metrics"></a>Aplikacja — dzienniki diagnostyczne, dzienniki aplikacji i miar
**Obliczanie tylko**

Aplikacje można uruchamiać na bieżąco system operacyjny gościa w modelu obliczeń. Wysyłają własnych zestawów dzienniki i wskaźniki.

Typy miar

- Liczniki wydajności
- Dzienniki aplikacji
- Dzienniki zdarzeń systemu Windows
- Źródło zdarzenia .NET
- Dzienniki programu IIS
- Pojawiają ETW zależności
- Zrzuty awaryjne
- Dzienniki błędów klienta


## <a name="uses-for-monitoring-data"></a>Zastosowanie do monitorowania danych

### <a name="visualize"></a>Wizualizowanie
Wizualizowanie danych monitorowania w wykresów i grafik ułatwia znajdowanie trendów szybciej niż przeszukiwać dane.  

Kilka metod wizualizacji obejmują:

- Za pomocą portalu Azure
- Przesyłanie danych do wniosków aplikacji Azure
- Przesyłanie danych do programu Microsoft PowerBI
- Rozsyłanie danych do narzędzia innych firm wizualizacji za pomocą live streaming lub przez narzędzie odczyt archiwum w magazynie Azure

### <a name="archive"></a>Archiwum
Monitorowanie danych jest zwykle zapisywane do magazynu Azure i składowane tam, dopóki nie zostaną usunięte.

Kilka sposobów używać tych danych:

- Po zapisaniu możesz mieć inne narzędzia w obrębie lub poza Azure czytać i przetworzyć go.
- Pobieranie danych lokalnie w celu archiwum lokalnego lub zmienianie zasad przechowywania w chmurze do przechowywania danych przez dłuższy czas.  
- Możesz pozostawić dane w magazynie Azure, ale trzeba zapłacić do przechowywania Azure zależą od ilości danych, które przechowujesz.
-

### <a name="query"></a>Kwerendy
Azure Monitor interfejsu API usługi REST, krzyżyk platformy wiersza polecenia interfejs poleceń, polecenia cmdlet programu PowerShell lub .NET SDK umożliwia dostęp do danych w systemie lub Azure miejsca do magazynowania

Przykłady:

-  Pobieranie danych dla aplikacji niestandardowej monitorowania, które zostały zapisane
-  Tworzenie niestandardowych kwerend i wysyłanie danych do aplikacji innych firm.

### <a name="route"></a>Trasa
Możesz przesyłać strumieniowo monitorowania danych w innych lokalizacjach w czasie rzeczywistym.

Przykłady:

- Wyślij do wniosków aplikacji, aby móc używać narzędzi do wizualizacji brak.
- Wyślij do koncentratorów zdarzenia, więc możesz rozesłać do narzędzia innej firmy do przeprowadzenia analizy w czasie rzeczywistym.

### <a name="automate"></a>Automatyzowanie
Możesz użyć monitorowania danych do zdarzenia wyzwalacza lub nawet cały proces należą:

- Używanie danych do autoscale do uruchamiania wystąpień w górę lub w dół według ładowania aplikacji.
- Wysyłać wiadomości e-mail, gdy przecina metryki interwałem progu.
- Adres URL sieci web (webhook) do wykonania akcji w systemie poza Azure połączeń
- Rozpoczynanie działań aranżacji w Azure automatyzacji, aby wykonać dowolną różnych zadań

## <a name="methods-of-use"></a>Metody pracy
Zazwyczaj można modyfikować dane śledzenia, routingu i pobieranie przy użyciu jednej z następujących metod. Nie wszystkie metody są dostępne dla wszystkich akcji lub typów danych.

- [Azure portal](https://portal.azure.com)
- [Programu PowerShell](insights-powershell-samples.md)  
- [Interfejs wiersza polecenia i platform (polecenie)](insights-cli-samples.md)
- [INTERFEJSU API USŁUGI REST](https://msdn.microsoft.com/library/dn931943.aspx)
- [ZESTAW SDK PROGRAMU .NET](https://msdn.microsoft.com/library/dn802153.aspx)

## <a name="azures-monitoring-offerings"></a>Azure i monitorowanie ofert
Azure ma dostępnych dla monitorowania usług od zera infrastruktury do telemetrycznego aplikacji. Najlepsza Strategia monitorowania łączy wykorzystywanie wszystkich trzech do uzyskania pełnego, szczegółowe wgląd kondycji usługi.

- [Monitorowanie azure](http://aka.ms/azmondocs) — ofert wizualizacji, kwerendy, routingu, alertu, autoscale i automatyzacji danych zarówno z Azure infrastruktury (dziennik) i każdego pojedynczego zasobu Azure (dzienniki diagnostyczne). Ten artykuł jest częścią dokumentację Monitora Azure. Nazwa monitora Azure został wydany wrzesień 25 w Ignite 2016.  Nazwa poprzedniego jest "Azure wnioski."  
- [Wnioski aplikacji](https://azure.microsoft.com/documentation/services/application-insights/) — zapewnia sformatowanego wykrywanie i diagnostyki problemów w warstwie aplikacji usługi, dobrze zintegrowany u góry danych z monitorowania Azure. Jest platformy diagnostyki domyślnego dla aplikacji sieci Web usługi.  Mogą przesyłać dane z innych usług.  
- [Analizy dziennika](https://azure.microsoft.com/documentation/services/log-analytics/) część [Pakietu zarządzania operacje](https://www.microsoft.com/cloud-platform/operations-management-suite) — rozwiązaniem jest kompleksowy IT zarządzania dla lokalnego i innych firm opartej na chmurze infrastruktury (na przykład AWS) oprócz Azure zasobów.  Dane z Azure Monitor można skierowana bezpośrednio do analizy dziennika, dzięki której będziesz widzieć metryki i dzienniki dla całego środowiska w jednym miejscu.     


## <a name="next-steps"></a>Następne kroki
Dowiedz się więcej o

- [Azure Monitor w klipu wideo z Ignite 2016](https://myignite.microsoft.com/videos/4977)
- [Wprowadzenie do programu Azure Monitor](monitoring-get-started.md)
- [Diagnostyka azure](../azure-diagnostics.md) , jeśli próbujesz diagnozowanie problemów w aplikacji usługi w chmurze, maszyn wirtualnych lub tkaninie usługi.
- [Wnioski aplikacji](https://azure.microsoft.com/documentation/services/application-insights/) , jeśli próbujesz problemów diagnostyczne w aplikacji sieci Web usługi aplikacji.
- [Rozwiązywanie problemów z magazynu Azure](../storage/storage-e2e-troubleshooting.md) podczas korzystania z obiektów blob miejsca do magazynowania, tabel lub kolejki
- [Dziennik analizy](https://azure.microsoft.com/documentation/services/log-analytics/) i [pakiecie zarządzania operacji](https://www.microsoft.com/cloud-platform/operations-management-suite)
