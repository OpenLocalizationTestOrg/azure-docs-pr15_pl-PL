<properties 
    pageTitle="Skonfigurować koder stanie wolnym na żywo, aby wysłać strumień na żywo pojedynczy szybkość transmisji bitów | Microsoft Azure" 
    description="W tym temacie przedstawiono sposób skonfigurować koder stanie wolnym Live wysyłanie strumienia pojedynczy szybkość transmisji bitów do AMS kanałów, które są włączone dla kodowanie live." 
    services="media-services" 
    documentationCenter="" 
    authors="cenkdin" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ne" 
    ms.topic="article" 
    ms.date="10/12/2016"
    ms.author="cenkdin;anilmur;juliako"/>

#<a name="use-the-elemental-live-encoder-to-send-a-single-bitrate-live-stream"></a>Umożliwia wysłanie strumień na żywo pojedynczy szybkość transmisji bitów encoder stanie wolnym Live

> [AZURE.SELECTOR]
- [Pierwiastkowej Live](media-services-configure-elemental-live-encoder.md)
- [Tricaster](media-services-configure-tricaster-live-encoder.md)
- [Wirecast](media-services-configure-wirecast-live-encoder.md)
- [FMLE](media-services-configure-fmle-live-encoder.md)

W tym temacie przedstawiono sposób skonfigurować koder [Stanie wolnym Live](http://www.elementaltechnologies.com/products/elemental-live) do wysyłania strumienia pojedynczy szybkość transmisji bitów AMS kanałów, które są włączone dla kodowanie live.  Aby uzyskać więcej informacji zobacz [Praca z kanałami, które służą do wykonywania Live kodowania z usługi multimediów Azure](media-services-manage-live-encoder-enabled-channels.md).

Ten samouczek pokazano, jak zarządzać usługi multimediów Azure (AMS) za pomocą narzędzia Azure multimediów usług Eksploratora (AMSE). To narzędzie działa tylko na komputerze z systemem Windows. Jeśli korzystasz z Mac lub Linux, należy utworzyć [kanałów](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) i [Programy](media-services-portal-creating-live-encoder-enabled-channel.md#create-and-manage-a-program)Azure portal.

##<a name="prerequisites"></a>Wymagania wstępne

- Musi być znają tworzenie wydarzeń na żywo za pomocą stanie wolnym Live interfejsu sieci web.
- [Tworzenie konta usługi multimediów Azure](media-services-portal-create-account.md)
- Upewnij się, jest punkt końcowy Streaming z co najmniej jedną jednostkę przesyłanie strumieniowe przydzielone. Aby uzyskać więcej informacji zobacz [Zarządzanie Streaming punkty końcowe na koncie usługi multimediów](media-services-portal-manage-streaming-endpoints.md).
- Zainstaluj najnowszą wersję z narzędzia [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) .
- Uruchom narzędzie i łączenia się z kontem AMS.

##<a name="tips"></a>Porady

- Jeśli to możliwe, za pomocą połączenia internetowego hardwired.
- Jest regułą podczas określania wymagań w zakresie przepustowości dwukrotnie przesyłanie strumieniowe bitrates. Gdy nie jest obowiązkowe wymagania, go może pomóc w zmniejszeniu wpływ przeciążeń sieci.
- Kiedy, używając oprogramowania oparte kodery, zamknij wszystkie zbędne programy.

## <a name="elemental-live-with-rtp-ingest"></a>Mogły zjeść tej ostatniej stanie wolnym Live z RTP

W tej sekcji przedstawiono sposób skonfigurować koder stanie wolnym Live wysyłające strumień na żywo pojedynczy szybkość transmisji bitów nad RTP.  Aby uzyskać więcej informacji zobacz [strumienia MPEG TS nad RTP](media-services-manage-live-encoder-enabled-channels.md#channel).

### <a name="create-a-channel"></a>Tworzenie kanału

1.  W narzędziu AMSE przejdź do karty **Live** , a następnie kliknij prawym przyciskiem myszy na obszarze kanału. Wybierz pozycję **Utwórz kanał** z menu.

![Stanie wolnym](./media/media-services-elemental-live-encoder/media-services-elemental1.png)

2. Określ nazwę kanału opis pole jest opcjonalne. W obszarze Ustawienia kanału wybierz pozycję **Standardowy** dla opcji Live kodowanie protokołu wprowadzania ustaw **RTP (MPEG-TS)**. Możesz pozostawić wszystkie inne ustawienia, tak jak.


Upewnij się, że zaznaczono opcję **Uruchom teraz nowy kanał** .

3. Kliknij przycisk **Utwórz kanał**.
![Stanie wolnym](./media/media-services-elemental-live-encoder/media-services-elemental12.png)

>[AZURE.NOTE] Kanał może potrwać 20 minut, aby rozpocząć.

Podczas uruchamiania kanału można [skonfigurować koder](media-services-configure-elemental-live-encoder.md#configure_elemental_rtp).

>[AZURE.IMPORTANT] Uwaga Po zainicjowaniu kanału przechodzi w stanie gotowości zaczyna się rozliczenia. Aby uzyskać więcej informacji zobacz [Stany kanału](media-services-manage-live-encoder-enabled-channels.md#states).

###<a id=configure_elemental_rtp></a>Konfigurowanie kodera stanie wolnym Live 

W tym samouczku używane są następujące ustawienia wyjściowe. Pozostałe w tej sekcji opisano kroki konfiguracji bardziej szczegółowo. 

**Klip wideo**:
 
- Koder-dekoder: H.264 
- Profil: Wysoki (poziom 4.0) 
- Szybkość transmisji bitów: 5000 KB/s 
- Klatka: dwie sekundy (60 sekund) 
- Ramka kurs: 30
 
**Audio**:

- Koder-dekoder: AAC (LC) 
- Szybkość transmisji bitów: 192 kb/s 
- Przykładowe kurs: 44,1 kHz


####<a name="configuration-steps"></a>Kroki konfiguracji

1. Przejdź do interfejsu sieci web **Stanie wolnym Live** i Konfigurowanie kodera przesyłania strumieniowego **UDP/TS** . 

2. Po utworzeniu nowego zdarzenia, przewiń w dół do grupy dane wyjściowe i Dodaj grupę dane wyjściowe **UDP/TS** . 

3. Utwórz nowe dane wyjściowe, wybierając **nowego strumienia** i klikając pozycję **Dodaj dane wyjściowe**.  
    
    ![Stanie wolnym](./media/media-services-elemental-live-encoder/media-services-elemental13.png)
    
    >[AZURE.NOTE] Zaleca się, że stanie wolnym zdarzenie ma kod czasowy, ustawić "Zegar systemowy", aby ułatwić encoder ponownie w razie awarii strumienia.

4. Teraz, gdy wynik został utworzony, kliknij przycisk **Dodaj strumienia**. Teraz można skonfigurować ustawienia wyjściowe. 
5. Przewiń w dół do "Strumienia 1" która właśnie została utworzona i kliknij kartę **wideo** po lewej stronie rozwiń sekcję ustawienia **Zaawansowane** . 

    ![Stanie wolnym](./media/media-services-elemental-live-encoder/media-services-elemental4.png)

    Live stanie wolnym ma szeroką gamę dostosowywania dostępne, następujące ustawienia są zalecane dla wprowadzenie do przesyłania strumieniowego do AMS. 
    
    - Rozdzielczość: 1280 x 720 
    - FrameRate: 30 
    - GOP rozmiar: ramy 60 
    - Tryb przeplotu: stopniowe 
    - Szybkość transmisji bitów: 5000000 bit/s (to może być określany na podstawie ograniczenia sieciowe) 
    

    ![Stanie wolnym](./media/media-services-elemental-live-encoder/media-services-elemental5.png)

6. Uzyskaj wprowadzania adresu URL tego kanału.
    
    Przejść z powrotem do narzędzia AMSE i sprawdzić stan realizacji kanału. Po stan zmienił się z **początkowy** **uruchomiony**, możesz uzyskać wprowadzania adresu URL.
      
    Gdy kanał jest uruchomiony, kliknij prawym przyciskiem myszy nazwę kanału, przejdź do pozycji umieść wskaźnik myszy na **URL wprowadzania Kopiuj do Schowka** , a następnie wybierz **Podstawowy wprowadzania adresu URL**.  
    
    ![Stanie wolnym](./media/media-services-elemental-live-encoder/media-services-elemental6.png)
    
1. Wklej te informacje w polu **Docelowym podstawowy** pierwiastka. Wszystkie pozostałe ustawienia może pozostać domyślny.
    
    ![Stanie wolnym](./media/media-services-elemental-live-encoder/media-services-elemental14.png)

    Dla dodatkowych nadmiarowości Powtórz te kroki, z adresem URL pomocniczej danych wejściowych, tworząc na innej karcie "Dane wyjściowe" przesyłania strumieniowego UDP/TS.
    
7. Kliknij przycisk **Utwórz** (Jeśli utworzono nowe zdarzenie) lub **Aktualizuj** (jeśli edytowanie istniejących wcześniej zdarzeń) i Kontynuuj, aby rozpocząć kodera. 

>[AZURE.IMPORTANT]Przed kliknięciem przycisku **Start** na interfejsu sieci web stanie wolnym Live, **musisz** upewnij się, czy kanał jest gotowa. 
>Upewnij się również, nie pozostawić kanału w stanie gotowości bez zdarzenia dłużej niż > 15 minut.

Po działaniu strumienia przez 30 sekund przejść z powrotem do odtwarzania narzędzie i badania AMSE.  

###<a name="test-playback"></a>Odtwarzanie test
  
1. Przejdź do narzędzia AMSE, a następnie kliknij prawym przyciskiem myszy kanał zostanie sprawdzone. Z menu Umieść wskaźnik myszy na **odtwarzanie podglądu** , a następnie kliknij pozycję **Azure Media Player**.  

    ![Stanie wolnym](./media/media-services-elemental-live-encoder/media-services-elemental8.png)

Jeśli strumienia znajduje się odtwarzacza, następnie koder zostało poprawnie skonfigurowane nawiązać AMS. 

Jeśli błąd kanału, należy zresetować i dostosować ustawienia kodera. Zobacz temat [Rozwiązywanie problemów z](media-services-troubleshooting-live-streaming.md) orientacji.   

###<a name="create-a-program"></a>Tworzenie programów

1. Po potwierdzeniu kanału odtwarzania, należy utworzyć program. Na karcie **Live** w narzędziu AMSE w obszarze program kliknij prawym przyciskiem myszy i wybierz pozycję **Utwórz nowy Program**.  

    ![Stanie wolnym](./media/media-services-elemental-live-encoder/media-services-elemental9.png)

2. Określ nazwę programu i w razie potrzeby dostosuj **Długość okna archiwum** (który domyślnie 4 godziny). Można także określić miejsce przechowywania lub pozostawić jako domyślny.  
3. Zaznacz pole wyboru **Uruchom Program teraz** .
4. Kliknij przycisk **Utwórz Program**.  
  
    Uwaga: Tworzenie Program ma mniej czasu niż tworzenie kanału.    
 
5. Po uruchomieniu programu Potwierdź odtwarzania, klikając prawym przyciskiem myszy pozycję program i przechodzenie do **odtwarzania programy** , a następnie wybierając **z Azure Media Player**.  
6. Po potwierdzeniu, kliknij prawym przyciskiem myszy program ponownie wybierz pozycję **Skopiuj adres URL wyjścia do Schowka** (lub pobrać tych informacji z **Ustawienia i informacje o programie** opcji z menu). 

Strumień jest teraz gotowy do osadzony w odtwarzaczu lub distributed odbiorcom do wyświetlania live.  

## <a name="troubleshooting"></a>Rozwiązywanie problemów

Zobacz temat [Rozwiązywanie problemów z](media-services-troubleshooting-live-streaming.md) orientacji. 


##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
