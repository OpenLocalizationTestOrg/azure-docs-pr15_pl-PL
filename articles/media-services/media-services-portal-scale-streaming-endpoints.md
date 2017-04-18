<properties
    pageTitle=" Skala streaming punkty końcowe Portal Azure | Microsoft Azure"
    description="Ten samouczek przeprowadzi Cię przez kroki skalowania przesyłanie strumieniowe punkty końcowe Portal Azure."
    services="media-services"
    documentationCenter=""
    authors="Juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="juliako"/>


# <a name="scale-streaming-endpoints-with-the-azure-portal"></a>Skala streaming punkty końcowe Portal Azure

##<a name="overview"></a>Omówienie

> [AZURE.NOTE] Aby użyć tego samouczka, potrzebne jest konto Azure. Aby uzyskać szczegółowe informacje zobacz [Azure bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/). 

Podczas pracy z jedną z najbardziej typowe scenariusze jest przedstawiania wideo za pośrednictwem szybkość transmisji bitów adaptacyjne streaming dla klientów usługi multimediów Azure. Usługi multimediów obsługuje następujące adaptacyjne szybkość transmisji bitów streaming technologii: HTTP Live Streaming (HLS), gładkie Streaming KRESKOWANIA MPEG i obr. / min (w przypadku tylko licencjobiorców Adobe PrimeTime-dostęp).

Usługi multimediów zawiera dynamiczne opakowań, umożliwiające dostarczanie usługi adaptacyjne szybkość transmisji bitów zawartości MP4 zakodowany w streaming formaty obsługiwane przez usługi multimediów (ŁĄCZNIKA MPEG, HLS, gładkie Streaming, obr. / min) tylko na czas, bez konieczności przechowywanie wersji wstępnie detaliczny każdej z tych streaming format.

Aby skorzystać z dynamicznego opakowania, musisz wykonaj następujące czynności:

- Kodowanie pliku kodery (źródło) do zestawu plików adaptacyjne szybkość transmisji bitów MP4 (kodowania czynności przedstawiono w dalszej części tego samouczka).  
- Tworzenie co najmniej jeden jednostki przesyłanie strumieniowe *Przesyłanie strumieniowe punktu końcowego* z której zamierzasz dostarczania zawartości. Poniżej pokazano, sposób zmieniania liczby jednostek przesyłanie strumieniowe.

Z dynamicznego opakowań potrzebne do przechowywania i zapłacić za pliki w formacie jednego miejsca do magazynowania i usługi multimediów będą tworzyć i służyć właściwej reakcji według żądaniami od klienta.

Ponadto można kontrolować możliwości usługa strumieniowego przesyłania punktu końcowego do obsługi rosnącej wymagania dotyczące przepustowości za pomocą dostosowania przesyłanie strumieniowe jednostki. Zalecane jest przydzielić skali jeden lub więcej aplikacji w środowisku produkcyjnym. Przesyłanie strumieniowe jednostki zapewniają zarówno pojemności dedykowane wyjściowym, którego można kupić w przyrostach 200 MB/s i dodatkowe funkcje które funkcji, która obejmuje: [dynamiczne opakowań](media-services-dynamic-packaging-overview.md), integracja CDN i Konfiguracja zaawansowana. Aby uzyskać więcej informacji zobacz [Zarządzanie przesyłanie strumieniowe punkty końcowe Portal Azure](media-services-portal-manage-streaming-endpoints.md).

## <a name="scale-streaming-endpoints"></a>Skala streaming punkty końcowe

Aby utworzyć i zmień liczbę streaming zastrzeżone jednostki, wykonaj następujące czynności:

1. W [portalu Azure](https://portal.azure.com/)wybierz swoje konto usługi multimediów Azure.
2. W oknie **Ustawienia** wybierz pozycję **Streaming punktów końcowych**.
3. Kliknij punkt końcowy przesyłanie strumieniowe, który chcesz skalować. 
4. Przesuń suwak, aby określić liczbę jednostek przesyłanie strumieniowe
 
![Przesyłanie strumieniowe punktu końcowego](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints3.png)

Następujące kwestie:

- Przydział wszelkie nowe jednostki przesyłanie strumieniowe może potrwać około 20 minut do wykonania. 
- Obecnie przejdziesz z wartości dodatnie strumieniowych jednostki powrót do Brak, można wyłączyć na żądanie streaming dla maksymalnie godzinę.
- Najwyższą liczbę jednostek określoną w okresie 24-godzinnym służy do obliczania kosztów. Aby uzyskać informacje o cenach szczegóły zobacz [Szczegóły cennik usługi multimediów](http://go.microsoft.com/fwlink/?LinkId=275107).

##<a name="next-steps"></a>Następne kroki

Przejrzyj ścieżki szkoleniowe dla użytkowników usługi multimediów.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


