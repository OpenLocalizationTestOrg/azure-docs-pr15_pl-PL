<properties
    pageTitle="Skalowanie zadania analizy strumieniu z funkcjami Azure maszynowego uczenia | Microsoft Azure"
    description="Dowiedz się, jak prawidłowo skalowanie analizy strumieniu zadań (podziału, ilość SU i inne) podczas korzystania z funkcji Azure maszynowego uczenia."
    keywords=""
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"
/>

# <a name="scale-your-stream-analytics-job-with-azure-machine-learning-functions"></a>Skalowanie zadania analizy strumieniu z funkcjami nauki maszynowego Azure

Często jest prostą konfigurowania zadania analizy strumieniu i uruchamiana kilka przykładowych danych. Co możemy zrobić, gdy trzeba uruchomić to samo zadanie z większą ilość danych? Wymaga nam dowiedzieć się, jak skonfigurować zadania analizy strumieniu tak, aby go będzie skalowanie. W tym dokumencie firma Microsoft poświęcona specjalne aspektów skalowania analizy strumieniu zadania z funkcjami nauki komputera. Informacje na temat ogólnego przeskalować analizy strumieniu zadań, zobacz artykuł [Skalowanie zadania](stream-analytics-scale-jobs.md).

## <a name="what-is-an-azure-machine-learning-function-in-stream-analytics"></a>Co to jest funkcja Azure maszynowego uczenia w analizy strumieniu?

Funkcję maszynowego uczenia analizy strumieniu może służyć takich jak wywołaniu funkcji zwykła w języku kwerend analizy strumieniu. Za sceny wywołania funkcji są faktyczną żądania usługi sieci Web uczenia Azure. Obsługa usług sieci web uczenia Machine "tworzeniu partii" wiele wierszy, nazywany minimalnej partii, w tym samym sieci web usługi interfejsu API połączenia, w celu zwiększenia ogólnej wydajności. Można znaleźć w następujących artykułach szczegółowe; [Funkcje Azure maszynowego uczenia do analizy strumieniu](https://blogs.technet.microsoft.com/machinelearning/2015/12/10/azure-ml-now-available-as-a-function-in-azure-stream-analytics/) i [Usług sieci Web uczenia maszynowego Azure](machine-learning/machine-learning-consume-web-services.md#request-response-service-rrs).

## <a name="configure-a-stream-analytics-job-with-machine-learning-functions"></a>Konfigurowanie zadanie analizy strumieniu z funkcjami nauki komputera

Podczas konfigurowania funkcji maszynowego uczenia analizy strumieniu zadania, istnieją dwa parametry brać pod uwagę rozmiar partii połączenia funkcji nauki komputera i przesyłanie strumieniowe jednostki (SUs) obsługi administracyjnej dla zadania analizy strumieniu. Aby określić odpowiednie wartości dla tych, najpierw należy podjąć decyzję między opóźnienie i przepustowość, oznacza to, że opóźnienie zadania analizy strumieniu i przepustowość każdego SU. SUs może być zawsze dodany do zadania, aby zwiększyć przepustowość również podzielone na partycje kwerendy analizy strumieniu, jednak dodatkowe SUs zwiększa koszt wykonywania zadania.

Dlatego jest ważne określić opóźnienie w wykonywania zadania analizy strumieniu *uszkodzenia* . Sposób naturalny zwiększa dodatkowe opóźnienie uruchamiania Azure maszynowego uczenia żądania obsługi z rozmiaru partii, który będzie złożony opóźnienie zadania analizy strumieniu. Z drugiej strony, zwiększając rozmiar partii umożliwia zadanie analizy strumieniu przetwarzania *kolejnych zdarzeń z *sam numer * maszynowego uczenia żądań usługi sieci web. Często wzrost opóźnienie usługi sieci web maszynowego uczenia jest częściowej liniowej zwiększenie rozmiaru partii, dlatego należy brać pod uwagę najbardziej wydajne wielkość partii usługi sieci web uczenia komputera w dowolnej sytuacji. Domyślny rozmiar partii żądań usług sieci web wynosi 1000 i mogą być zmieniane za pomocą [Interfejsu API usługi REST analizy strumieniu](https://msdn.microsoft.com/library/mt653706.aspx "Interfejsu API usługi REST analizy strumieniu") lub [klienta programu PowerShell dla analizy strumieniu](stream-analytics-monitor-and-manage-jobs-use-powershell.md "klienta programu PowerShell dla analizy strumieniu").

Po określeniu wielkości liczbę jednostek Streaming (SUs) jest możliwe, na podstawie liczby zdarzeń, które funkcja musi procesu na sekundę. Więcej informacji na temat jednostki Streaming zwróć się artykuł [skali analizy strumieniu zadania](stream-analytics-scale-jobs.md#configuring-streaming-units).

Na ogół ma 20 równoczesne połączeń maszynowego uczenia usługi sieci web dla każdej 6 SUs z tym wyjątkiem, że 1 zadania SU i 3 zadania SU otrzymają 20 połączeń równoczesne również.  Na przykład jeśli szybkość wprowadzania danych jest 200 000 wydarzeń na sekundę oraz rozmiar partii zostało domyślne 1000 wyniku opóźnienie usługi sieci web z partii Minimalna zdarzenia 1000 jest 200 ms. Oznacza to, że każdego połączenia, można wprowadzać 5 żądania usługi sieci web uczenia komputera w drugiej. 20 połączeń z zadania analizy strumieniu może przetworzyć 20 000 zdarzeń w 200 MS i w związku z tym 100 000 zdarzeń w drugiej. Dlatego przetwarzania 200 000 wydarzeń na sekundę, analizy strumieniu zadanie wymaga 40 równoczesne połączenia, które pojawia do 12 SUs. Poniższy diagram przedstawia żądania zadania analizy strumieniu do punktu końcowego usługi sieci web uczenia komputera — co SUs 6 z 20 połączeń jednocześnie do usługi sieci web maszynowego uczenia się u max.

![Analizy strumieniu skali przykładu zadania Machine nauki funkcji 2] (./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-00.png "Analizy strumieniu skali przykładu zadania Machine nauki funkcji 2")

Ogólnie ***B*** rozmiaru partii, ***L*** opóźnienie usługi sieci web w rozmiarze partii B (w milisekundach), przepustowość zadanie analizy strumieniu z ***N*** SUs jest:

![Skalowanie analizy strumieniu z maszynowego uczenia formuła funkcji] (./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-02.png "Skalowanie analizy strumieniu z maszynowego uczenia formuła funkcji")

Dodatkowe uwarunkowania może być "max połączenia równoczesne" na stronie usługi sieci web uczenia komputera, zalecamy można ustawić wartość maksymalna (obecnie 200).

Aby uzyskać więcej informacji na ten Przejrzyj ustawienia [artykuł skalowanie dla usług sieci Web uczenia komputera](../machine-learning/machine-learning-scaling-webservice.md).

## <a name="example--sentiment-analysis"></a>Przykład — analizy upodobania

Poniższy przykład zawiera zadanie analizy strumieniu z analizą upodobania funkcji maszynowego uczenia, zgodnie z opisem w [Samouczek integracji nauki maszynowego analizy strumieniu](stream-analytics-machine-learning-integration-tutorial.md).

Kwerenda jest prostej kwerendy w pełni podzielone na partycje obserwowane przez funkcję **upodobania** , tak jak pokazano poniżej:

    WITH subquery AS (
        SELECT text, sentiment(text) as result from input
    )

    Select text, result.[Score]
    Into output
    From subquery

Scenariusz; o przepustowości 10 000 tweety na sekundę należy utworzyć zadanie analizy strumieniu do wykonywania analiz upodobania tweety (zdarzeniami). Przy użyciu 1 SU, to zadanie analizy strumieniu będzie mogła obsługiwać dane? Korzystając z domyślnego rozmiaru partii 1000 zadania powinno być możliwe pracę danych wejściowych. Dalsze dodano funkcję maszynowego uczenia należy generować nie więcej niż drugi opóźnienie, czyli ogólne opóźnienie domyślnego analizy upodobania maszynowego uczenia usługi sieci web (z domyślnego rozmiaru partii 1000). Opóźnienie zadania analizy strumieniu **ogólną** lub zakończenia do końca zwykle są kilka sekund. Zapoznaj się z bardziej szczegółowe do tego zadania analizy strumieniu *zwłaszcza* maszynowego uczenia wywołania funkcji. O wielkości partii jako 1000, przepustowości 10 000 zdarzeń potrwa około 10 żądania usługi sieci web. Nawet w przypadku 1 SU istnieje za mało równoczesne połączenia, aby zezwalały na ruch wprowadzania danych.

Ale co zrobić, jeśli stopa zdarzenie wprowadzania zwiększa 100 x i teraz zadanie analizy strumieniu wymaga przetwarzania 1 000 000 tweety na sekundę? Dostępne są dwie opcje:

1.  Zwiększanie rozmiaru partii lub
2.  Strumień wejściowy przetwarzania zdarzeń równolegle partition

Za pomocą opcji pierwszy zwiększa zadania **opóźnienie** .

Za pomocą drugiej opcji więcej SUs musi być przygotowana i w związku z tym generowanie bardziej równoczesne żądania usługi sieci web nauki komputera. Oznacza to, że zwiększa **Koszt** .


Załóżmy, że opóźnienie analizy upodobania maszynowego uczenia usługi sieci web jest 200 MS partii zdarzenia 1000 lub poniżej, 250ms partii 5000 zdarzenia, 300ms partii zdarzenia 10 000 lub 500ms partii 25 000 zdarzenia.

1. Za pomocą opcji pierwszy, (**nie** więcej SUs inicjowania obsługi administracyjnej), rozmiar partii może wzrosnąć do **25 000**. To z kolei umożliwia zadania przetwarzania zdarzeń 1 000 000 zł przy 20 równoczesne połączenia z usługą sieci web uczenia komputera (z opóźnienie 500ms na połączenia). Dlatego dodatkowe opóźnienie zadania analizy strumieniu z powodu żądania funkcji upodobania przed żądań usług sieci web uczenia komputera zostanie zwiększona od **200 MS** do **500ms**. Należy zauważyć, że partii rozmiar **nie** być jednak lepszą nieskończenie, jak maszynowego uczenia usług sieci web wymaga rozmiar ładunku żądania 4 MB lub mniejsze usługi sieci web żądania przekroczenia limitu czasu 100 sekund operacji.
2. Za pomocą drugą opcję, rozmiar partii pozostanie w 1000, z czasem oczekiwania usługi sieci web 200 MS, co 20 równoczesne połączenia z usługą sieci web można przetwarzać 1000 zdarzenia *20* 5 = 100 000 na sekundę. Sposób przetwarzania 1 000 000 wydarzeń na sekundę, zadania może być konieczne 60 SUs. W porównaniu do pierwszą opcję, zadanie analizy strumieniu spowodowałby więcej żądań usług sieci web partii, z kolei generowania lepszą koszt.

Poniżej znajduje się tabela przepustowości zadania analizy strumieniu dla różnych SUs i rozmiarów partii (w liczbie wydarzeń na sekundę).

| rozmiar partii (opóźnienie ML)  | 500 (200 ms) | 1000 (200 ms) | 5000 (250ms) | 10 000 (300ms) | 25 000 (500ms) |
|--------|-------------------------|---------------|---------------|----------------|----------------|
| **1 SU** | 2500 | 5000 | 20 000 | 30 000 | 50 000 |
| **3 SUs** | 2500 | 5000 | 20 000 | 30 000 | 50 000 |
| **6 SUs** | 2500 | 5000 | 20 000 | 30 000 | 50 000 |
| **12 SUs** | 5000 | 10 000 | 40 000 | 60 000 | 100 000 |
| **18 SUs** | 7 500 | 15 000 | 60 000 | 90,000 | 150,000 |
| **24 SUs** | 10 000 | 20 000 | 80,000 | 120 000 | 200 000 |
| **…** | … | … | … | … | … |
| **60 SUs** | 25 000 | 50 000 | 200 000 | 300 000 | 500 000 |

Chwili powinna już dobrze zapoznać się z działania funkcji maszynowego uczenia analizy strumieniu. Może też rozumiesz analizy strumieniu zadania "pobrania" danych ze źródła danych i każdego "wypychania" zwracanych serię zdarzeń dla zadania analizy strumieniu przetwarzania. Jak wpływ modelu ten pobieraj nauki maszynowego web zleceń serwisowych?

Zazwyczaj rozmiar partii ustawione dla funkcji maszynowego uczenia dokładnie będzie podzielić przez liczbę zdarzeń zwracane przez każdego zadania analizy strumieniu "pobieraj". Gdy to występuje maszynowego uczenia usługi sieci web będzie wywoływana za pomocą partie "częściowe". Można to zrobić do nie ponoszenia ogólnych opóźnienie zadania dodatkowe tych wydarzeń pobieraj pobieraj.

## <a name="new-function-related-monitoring-metrics"></a>Nowe związane z funkcji monitorowania metryki

W obszarze Monitor zadanie analizy strumieniu zostały dodane trzy dodatkowe metryki związane z funkcji. Są ŻĄDANIA funkcji, funkcja zdarzeń i ŻĄDANIA funkcji nie powiodło się, jak pokazano na poniższym rysunku.

![Skalowanie analizy strumieniu z maszynowego uczenia metryki funkcji] (./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-01.png "Skalowanie analizy strumieniu z maszynowego uczenia metryki funkcji")

Są zdefiniowane w następujący sposób:

**Funkcja ŻĄDANIA**: liczba żądań funkcji.

**Funkcja zdarzeń**: liczba zdarzeń w żądania funkcji.

**Niepowodzenie ŻĄDANIA funkcji**: liczba żądań funkcji nie powiodło się.

## <a name="key-takeaways"></a>Takeaways kluczowe  

Aby podsumować główne punkty, aby przeskalować zadanie analizy strumieniu z funkcjami maszynowego uczenia, być traktowane jako następujące elementy:

1.  Stopa zdarzenie wprowadzania
2.  Opóźnienie tolerowaną uruchomione zadanie analizy strumieniu (i tym samym rozmiar partii żądań usług sieci web uczenia komputera)
3.  Ustanawianie SUs analizy strumieniu i liczba żądań usług sieci web uczenia komputera (dodatkowe związane z funkcji koszty)

Jako przykład został użyty w pełni podzielone na partycje zapytania analizy strumieniu. Jeśli potrzebna jest bardziej złożonych kwerend na [forum analizy strumieniu Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) to doskonałe źródło uzyskać dodatkową pomoc od zespołu analizy strumieniu.

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej na temat analizy strumieniu, zobacz:

- [Wprowadzenie do korzystania z analizy strumieniu Azure](stream-analytics-get-started.md)
- [Skalowanie Azure analizy strumieniu zadania](stream-analytics-scale-jobs.md)
- [Dokumentacja języka kwerend analizy strumieniu Azure](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Strumień Azure analizy zarządzania pozostałych interfejsu API odwołania](https://msdn.microsoft.com/library/azure/dn835031.aspx)
