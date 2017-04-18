<properties
    pageTitle="Skalowanie omówienie przetwarzania multimedia | Microsoft Azure"
    description="W tym temacie przedstawiono skalowania przetwarzania multimediów z usługi multimediów Azure."
    services="media-services"
    documentationCenter=""
    authors="juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="juliako"/>


# <a name="scaling-media-processing-overview"></a>Omówienie przetwarzania multimediów skalowania

Ta strona zawiera z omówieniem i dlaczego przeskalować przetwarzanie multimediów. 

## <a name="overview"></a>Omówienie

Konto usługi multimediów jest skojarzony z zastrzeżone typ jednostki, która określa szybkości przetwarzania multimediów przetwarzania zadania. Można pobrać z następujących typów jednostek zastrzeżone: **S1**, **S2**i **S3**. Na przykład to samo zadanie kodowania przyspieszenia użycie Porównaj typ jednostek **S2** zastrzeżone typu **S1** . Aby uzyskać więcej informacji zobacz [Zastrzeżone typy jednostek](https://azure.microsoft.com/blog/high-speed-encoding-with-azure-media-services/).

Oprócz określającą typ zastrzeżone jednostki, można określić ustanawianie konta zastrzeżone jednostki. Liczba jednostek zastrzeżone ustanawianie określa liczbę zadań multimedialne, które mogą być przetwarzane jednocześnie z danego konta. Na przykład jeśli Twoje konto ma pięciu jednostek zastrzeżone, multimediów pięć zadania ma działać jednocześnie tak długo jak istnieją zadania mają być przetwarzane. Zadania pozostałe będzie oczekiwać w kolejce i ma uzyskać pobierany przetwarzania kolejno po zakończeniu uruchomione zadanie. Jeśli konto nie ma dowolną zastrzeżone jednostki obsługi administracyjnej, następnie zadania będą pobierane w górę kolejno. W tym przypadku czas oczekiwania między jedno zadanie zakończy i następny początkowej zależy od dostępności zasobów w systemie.

## <a name="choosing-between-different-reserved-unit-types"></a>Wybieranie między typami innej jednostki zastrzeżone

Poniższa tabela ułatwia podjęcie decyzji, wybierając między różne szybkości kodowania. Również udostępnia kilka przykładów wzorca i zawiera adresy URL skojarzeń zabezpieczeń, którego można używać do pobierania klipów wideo, w których można wykonywać swoje własne testy:

Scenariusze|**S1**|**S2**|**S3**|
----------|------------|----------|------------
Przeznaczenie wielkość liter| Pojedyncze kodowanie szybkość transmisji bitów. <br/>Pliki SD lub poniżej rozwiązania, czasu nie poufne, niski koszt.|Pojedynczy szybkość transmisji bitów i kodowanie wielu szybkości transmisji bitów.<br/>Normalny zastosowania zarówno SD i HD kodowania. |Pojedynczy szybkość transmisji bitów i kodowanie wielu szybkości transmisji bitów.<br/>Pełna HD i 4K rozdzielczości wideo. Czas kodowania przetwarzania poufnych, szybciej. 
Wzorzec|[Pliku wejściowego: ramki 640x360p u 29,97 długie 5 minut na sekundę](https://wamspartners.blob.core.windows.net/for-long-term-share/Whistler_5min_360p30.mp4?sr=c&si=AzureDotComReadOnly&sig=OY0TZ%2BP2jLK7vmcQsCTAWl33GIVCu67I02pgarkCTNw%3D).<br/><br/>Kodowanie pojedynczy szybkość transmisji bitów pliku MP4, o takiej samej rozdzielczości trwa około 11 minut.|[Plik wejściowy: ramki 1280x720p u 29,97 długie 5 minut na sekundę](https://wamspartners.blob.core.windows.net/for-long-term-share/Whistler_5min_720p30.mp4?sr=c&si=AzureDotComReadOnly&sig=OY0TZ%2BP2jLK7vmcQsCTAWl33GIVCu67I02pgarkCTNw%3D)<br/><br/>Kodowanie z "H264 pojedynczy szybkość transmisji bitów 720p" wstępnie trwa około 5 minut.<br/><br/>Kodowanie z "H264 wielu szybkości transmisji bitów 720p" ustawienie wstępne trwa około 11,5 minut.|[Pliku wejściowego: ramki 1920x1080p u 29,97 długie 5 minut na sekundę](https://wamspartners.blob.core.windows.net/for-long-term-share/Whistler_5min_1080p30.mp4?sr=c&si=AzureDotComReadOnly&sig=OY0TZ%2BP2jLK7vmcQsCTAWl33GIVCu67I02pgarkCTNw%3D). <br/><br/>Kodowanie z "H264 pojedynczy szybkość transmisji bitów 1080p" wstępnie trwa około minuty 2.7.<br/><br/>Kodowanie z "H264 wielu szybkości transmisji bitów 1080p" ustawienie wstępne trwa około 5.7 minut.

##<a name="considerations"></a>Zagadnienia dotyczące

>[AZURE.IMPORTANT] Przejrzyj zagadnienia opisane w tej sekcji.  

- Zastrzeżone jednostki pracą parallelizing przetwarzania multimediów, łącznie z indeksowania zadań przy użyciu indeksowania multimediów Azure.  Jednak w przeciwieństwie do kodowania indeksowania nie uzyskać przetwarzane szybciej z szybciej zastrzeżone jednostki.

- Jeśli używasz udostępnionej puli, oznacza to, że bez dowolnego zastrzeżone jednostek zadań kodowanie wybrać samej wydajności podobnie jak w przypadku S1 RUs. Istnieje jednak działać nie granica górna na czas zadań poświęcić w kolejce stanie i w dowolnym momencie, co najwyżej jednego zadania.

- Następujące centrach danych nie oferują typ jednostki **S2** zastrzeżone: Południowej Brazylia, zachód Indie Indie centralnej i Indie południe.

- Następujące centrach danych nie oferują typ jednostki **S3** zastrzeżone: Brazylia południe, zachód Indie Indie centralnej.

- Najwyższą liczbę jednostek określoną w okresie 24-godzinnym służy do obliczania kosztów.


##<a name="quotas-and-limitations"></a>Przydziały i ograniczenia

Aby uzyskać informacji na temat przydziałów i ograniczenia oraz sposobu otwierania bilet pomocy technicznej zobacz [przydziałów](media-services-quotas-and-limitations.md).

##<a name="next-step"></a>Następny krok

Osiągnięcia skalowania zadanie przetwarzania multimediów z jednym z tych technologii: 

> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-encoding-units.md)
- [Portal](media-services-portal-scale-media-processing.md)
- [POZOSTAŁE](https://msdn.microsoft.com/library/azure/dn859236.aspx)
- [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
- [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)

##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
