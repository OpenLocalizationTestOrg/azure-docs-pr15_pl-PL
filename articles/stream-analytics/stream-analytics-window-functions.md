<properties
    pageTitle="Wprowadzenie do funkcji okna analizy strumieniu | Microsoft Azure"
    description="Informacje o trzy funkcje okna do analizy strumieniu (tumbling skaczące, przesuwanie)."
    keywords="tumbling oknie przesuwanie okna Skaczące okno"
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


# <a name="introduction-to-stream-analytics-window-functions"></a>Wprowadzenie do funkcji okna analizy strumieniu

W czasie rzeczywistym wiele streaming scenariuszy należy do wykonywania operacji tylko na danych znajdujących się na czasowy systemu windows. Wbudowana obsługa funkcji Obsługa okien jest funkcją klucza analizy strumieniu Azure przenoszącą igłę na wydajność deweloperów w tworzeniu przetwarzanych strumienia złożonych zadań. Analizy strumieniu projektanci czasowy operacji na przesyłanie strumieniowe danych przy użyciu [**Tumbling**](https://msdn.microsoft.com/library/dn835055.aspx), [**Hopping**](https://msdn.microsoft.com/library/dn835041.aspx) i [**ruchomej**](https://msdn.microsoft.com/library/dn835051.aspx) systemu windows. Warto zauważyć, że wszystkie operacje na [oknach](https://msdn.microsoft.com/library/dn835019.aspx) wyjściowy wyniki na **koniec** okna. Dane wyjściowe okna będą pojedyncze zdarzenie, na podstawie funkcji agregującej używane. Wydarzenie ma mieć sygnatura czasowa na koniec okna i wszystkie funkcje okna są zdefiniowane o stałej długości. Na koniec należy pamiętać, że wszystkie funkcje okna powinny być używane w klauzuli [**GROUP BY**](https://msdn.microsoft.com/library/dn835023.aspx) .

![Okno analizy strumieniu funkcje pojęcia](media/stream-analytics-window-functions/stream-analytics-window-functions-conceptual.png)

## <a name="tumbling-window"></a>Okno tumbling

Tumbling okno, które funkcje są używane do segmentu strumienia danych na segmenty odrębnych czasu i funkcji, takich jak w poniższym przykładzie. Kluczowe wyróżniającymi okna Tumbling to, że powtarzają, nie pokrywają się i wydarzenia nie mogą należeć do więcej niż jedno okno tumbling.

![Funkcje okna analizy strumieniu tumbling wprowadzający](media/stream-analytics-window-functions/stream-analytics-window-functions-tumbling-intro.png)

## <a name="hopping-window"></a>Okno rozrzucającym

Skaczące okno funkcji przeskoku do przodu w czasie w określonym czasie. Może być łatwo je sobie wyobrazić jako Tumbling systemu windows, które można nakładają się, więc zdarzeń mogą należeć do więcej niż jeden zestaw wyników okno Hopping. Okno Hopping taki sam, jak Tumbling okna jedną chcesz po prostu ustaw rozmiar przeskoku, musi być taka sama, jak rozmiar okna. 

![Funkcje okna analizy strumieniu skaczące wprowadzający](media/stream-analytics-window-functions/stream-analytics-window-functions-hopping-intro.png)

## <a name="sliding-window"></a>Przesuwanie okna

Przesuwania funkcji okno, w odróżnieniu od okien Tumbling lub Hopping, da wynik **tylko** po wystąpieniu zdarzenia. Wszystkie okna będą mieć co najmniej jedno zdarzenie i okna stale powoduje przejście do przodu przez € (epsilon). Przykład skaczące Windows zdarzeń mogą należeć do więcej niż jedno okno przesuwane.

![Funkcje okna analizy strumieniu przesuwanie wprowadzający](media/stream-analytics-window-functions/stream-analytics-window-functions-sliding-intro.png)

## <a name="getting-help-with-window-functions"></a>Uzyskiwanie pomocy dotyczącej funkcje okna

Aby uzyskać dodatkową pomoc spróbuj nasze [forum analizy strumieniu Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Następne kroki

- [Wprowadzenie do analizy strumieniu Azure](stream-analytics-introduction.md)
- [Wprowadzenie do korzystania z analizy strumieniu Azure](stream-analytics-get-started.md)
- [Skalowanie Azure analizy strumieniu zadania](stream-analytics-scale-jobs.md)
- [Dokumentacja języka kwerend analizy strumieniu Azure](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Strumień Azure analizy zarządzania pozostałych interfejsu API odwołania](https://msdn.microsoft.com/library/azure/dn835031.aspx)
