<properties
    pageTitle="Analizowanie multimediów za pomocą portalu Azure | Microsoft Azure"
    description="W tym temacie omówiono sposób przetwarzania multimediów procesorów multimediów analizy multimediów (MPs) za pomocą portalu Azure."
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


# <a name="analyze-your-media-using-the-azure-portal"></a>Analizowanie multimediów za pomocą portalu Azure

> [AZURE.NOTE] Aby użyć tego samouczka, potrzebne jest konto Azure. Aby uzyskać szczegółowe informacje zobacz [Azure bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/). 

## <a name="overview"></a>Omówienie

Analizy usługi multimediów Azure to zbiór składników rozpoznawania mowy i wzroku (w skali przedsiębiorstwa, zgodności, zabezpieczenia i globalne), które ułatwiają organizacje i przedsiębiorstwa do uzyskania sankcji wniosków z ich plików wideo. Aby uzyskać bardziej szczegółowe omówienie analizy usługi multimediów Azure zobacz [w tym](media-services-analytics-overview.md) temacie. 

W tym temacie omówiono sposób przetwarzania multimediów procesorów multimediów analizy multimediów (MPs) za pomocą portalu Azure. Multimedia analizy MPs warzywa pliki MP4 lub JSON. Jeżeli procesor multimediów przedstawiony pliku MP4, stopniowo mogą pobrać plik. Jeżeli procesora multimedia przedstawiony pliku JSON, mogą pobrać plik z magazynu obiektów blob platformy Azure. 

## <a name="choose-an-asset-that-you-want-to-analyze"></a>Wybierz zasób, który chcesz przeanalizować 
 
1. W [portalu Azure](https://portal.azure.com/)wybierz swoje konto usługi multimediów Azure.
2. W oknie **Ustawienia** wybierz pozycję **elementy zawartości**.  
.
    ![Analizowanie klipów wideo](./media/media-services-portal-analyze/media-services-portal-analyze001.png)

2. Wybierz pozycję elementu, który chcesz przeanalizować, a następnie naciśnij przycisk **Analiza** .
        
    ![Analizowanie klipów wideo](./media/media-services-portal-analyze/media-services-portal-analyze002.png)

3. W oknie **proces multimedialnej z analizy multimediów** wybierz pozycję procesora. 

    Pozostałej części tego artykułu wyjaśniono, dlaczego i jak używać każdego procesora. 
   
4. Naciśnij klawisz **Utwórz** do początku zadania.

## <a name="azure-media-indexer"></a>Indeksowanie multimediów Azure

Procesor multimediów **Indeksatora multimediów Azure** umożliwia udostępnianie plików multimedialnych i treści można wyszukiwać, a także do generowania ścieżki podpisów kodowanych. Tej sekcji zawiera niektóre informacje dotyczące opcji, które można określić dla tego CR.

![Analizowanie klipów wideo](./media/media-services-portal-analyze/media-services-portal-analyze003.png)

### <a name="language"></a>Język

Język naturalny uznawane plik multimedialny. Na przykład angielski i hiszpański. 

### <a name="captions"></a>Podpisy

Możesz wybrać format etykiety, wygenerowania z zawartości. Indeksowania zadania można wygenerować podpisów kodowanych pliki w następujących formatach:  

- **Sami**
- **TTML**
- **WebVTT**

Zamknięty podpis (DW) pliki w następujących formatach można udostępnić plików audio i wideo dla osoby niepełnosprawne słuchu.

### <a name="aib-file"></a>Plik AIB

Wybierz tę opcję, jeśli chcesz wygenerować plik Audio Blob indeksu dla niestandardowej IFilter serwera SQL. Aby uzyskać więcej informacji zobacz [tym](https://azure.microsoft.com/blog/using-aib-files-with-azure-media-indexer-and-sql-server/) blogu.

### <a name="keywords"></a>Słowa kluczowe

Wybierz tę opcję, jeśli chcesz wygenerować plik XML słów kluczowych. Ten plik zawiera słowa kluczowe wyodrębnionych z zawartością mowy o częstotliwości i informacje dotyczące przesunięcia.

### <a name="job-name"></a>Nazwa zadania

Przyjazna nazwa umożliwia identyfikację zadania. [W tym](media-services-portal-check-job-progress.md) artykule opisano, jak można monitorować postęp zadania. 

### <a name="output-file"></a>Plik docelowy

Przyjazna nazwa pozwala identyfikować zawartość dane wyjściowe. 

## <a name="azure-media-hyperlapse"></a>Hyperlapse multimediów Azure

Azure Hyperlapse multimediów jest CR tworzące wygładzonymi czas, jaki upłynął klipów wideo z pierwszej osoby lub aparatu fotograficznego akcji zawartość.  Aby uzyskać więcej informacji zobacz [w tym](media-services-hyperlapse-content.md) temacie. Tej sekcji zawiera niektóre informacje dotyczące opcji, które można określić dla tego CR.

![Analizowanie klipów wideo](./media/media-services-portal-analyze/media-services-portal-analyze004.png)

### <a name="speed"></a>Szybkość 

Określ doskonale przyspieszanie wideo wprowadzania danych. Wynik to ustalonych i czas, jaki upłynął odwzorowanie danych wejściowych wideo.

### <a name="job-name"></a>Nazwa zadania

Przyjazna nazwa umożliwia identyfikację zadania. [W tym](media-services-portal-check-job-progress.md) artykule opisano, jak można monitorować postęp zadania. 

### <a name="output-file"></a>Plik docelowy

Przyjazna nazwa pozwala identyfikować zawartość dane wyjściowe. 

## <a name="azure-media-face-detector"></a>Wykrywanie nominalnej multimediów Azure

Procesor multimediów **Azure multimediów nominalnej wykrywanie** (CR) umożliwia zliczanie, śledzenie ruchów, a nawet ocenić uczestnictwo odbiorców i reakcji za pośrednictwem min. Ta usługa zawiera dwie funkcje: 

- **Wykrywanie buźka**

    Wykrywanie nominalnej umożliwia znalezienie wyrazów i śledzenie ludzi powierzchni w pliku wideo. Wielu powierzchni można wykryć, a następnie śledzenia ich poruszania się po, czas i lokalizację metadanymi zwrócone w pliku JSON. Podczas śledzenia spróbuje udzielić jednolity identyfikator do samej nominalnej, gdy osoby jest poruszanie się po aplikacji na ekranie, nawet jeśli są zablokowane lub krótko pozostaw ramki.

    >[AZURE.NOTE]Tej usługi nie jest sprawdzana rozpoznawania min. Osoba, która opuszcza ramki lub staje się blokować dla zbyt długo będzie mieć nowy identyfikator gdy zostaną zwrócone.

- **Wykrywanie emocje**
    
    Wykrywanie emocje jest opcjonalny składnik procesora multimediów wykrywania nominalnej, która zwraca analizy na wiele atrybutów emocjonalne z powierzchni wykryte, takich jak szczęście, sadness, dostępność i gniew. 

![Analizowanie klipów wideo](./media/media-services-portal-analyze/media-services-portal-analyze005.png)

### <a name="detection-mode"></a>Tryb wykrywania

Jedną z następujących trybów mogą być używane przez procesor:

- wykrywanie buźka
- na wykrywania emocje buźka
- wykrywanie emocje agregacji

### <a name="job-name"></a>Nazwa zadania

Przyjazna nazwa umożliwia identyfikację zadania. [W tym](media-services-portal-check-job-progress.md) artykule opisano, jak można monitorować postęp zadania. 

### <a name="output-file"></a>Plik docelowy

Przyjazna nazwa pozwala identyfikować zawartość dane wyjściowe. 

## <a name="azure-media-motion-detector"></a>Wykrywanie ruchu multimediów Azure

Procesor multimediów **Azure multimediów ruchu wykrywanie** (CR) umożliwia efektywne identyfikują sekcje miejsc, w przeciwnym razie długie i procesu klip wideo. Wykrywanie ruchu może służyć na materiału statyczne kamery do identyfikowania sekcje wideo wykrycie ruchu. Generuje plik JSON zawierający metadane z sygnatury czasowe i dopasowania region, w którym wystąpiło zdarzenie.

Przeznaczona do zabezpieczenia źródła wideo, ta technologia będzie mógł przyporządkować ruchu do odpowiednich wydarzeń i wyniki fałszywie dodatnie, takie jak cienie i zmian oświetlenia. Umożliwia generowanie alertów zabezpieczeń na podstawie źródeł kamery bez otrzymywania wiadomości-śmieci nieograniczone zdarzenia nie ma znaczenia, podczas możliwość wyodrębniania chwil zainteresowania bardzo długo nadzoru klipy wideo.

![Analizowanie klipów wideo](./media/media-services-portal-analyze/media-services-portal-analyze006.png)

## <a name="azure-media-video-thumbnails"></a>Miniatury wideo multimediów Azure

Tego procesora może ułatwić tworzyć podsumowania długi klipów wideo, wybierając automatycznie interesujące wstawki z wideo źródła. Jest to przydatne, gdy chcesz podać krótkie omówienie czego można oczekiwać w długich wideo. Aby uzyskać szczegółowe informacje i przykłady zobacz [Używanie Azure multimediów wideo miniatury tworzenie podsumowania klip wideo](media-services-video-summarization.md)

![Analizowanie klipów wideo](./media/media-services-portal-analyze/media-services-portal-analyze008.png)

### <a name="job-name"></a>Nazwa zadania

Przyjazna nazwa umożliwia identyfikację zadania. [W tym](media-services-portal-check-job-progress.md) artykule opisano, jak można monitorować postęp zadania. 

### <a name="output-file"></a>Plik docelowy

Przyjazna nazwa pozwala identyfikować zawartość dane wyjściowe. 


##<a name="next-steps"></a>Następne kroki

Wyświetlanie ścieżki szkoleniowe dla użytkowników usług multimediów.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


