<properties 
    pageTitle="Konfigurowanie kodera NewTek TriCaster wysłać strumień na żywo pojedynczy szybkość transmisji bitów | Microsoft Azure" 
    description="W tym temacie przedstawiono sposób skonfigurować koder live Tricaster wysyłanie strumienia pojedynczy szybkość transmisji bitów do AMS kanałów, które są włączone dla kodowanie live." 
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
    ms.author="juliako;cenkd;anilmur"/>

#<a name="use-the-newtek-tricaster-encoder-to-send-a-single-bitrate-live-stream"></a>Umożliwia wysłanie strumień na żywo pojedynczy szybkość transmisji bitów encoder NewTek TriCaster

> [AZURE.SELECTOR]
- [Tricaster](media-services-configure-tricaster-live-encoder.md)
- [Pierwiastkowej Live](media-services-configure-elemental-live-encoder.md)
- [Wirecast](media-services-configure-wirecast-live-encoder.md)
- [FMLE](media-services-configure-fmle-live-encoder.md)

W tym temacie przedstawiono sposób skonfigurować koder live [NewTek TriCaster](http://newtek.com/products/tricaster-40.html) wysyłanie strumienia pojedynczy szybkość transmisji bitów do AMS kanałów, które są włączone dla kodowanie live. Aby uzyskać więcej informacji zobacz [Praca z kanałami, które służą do wykonywania Live kodowania z usługi multimediów Azure](media-services-manage-live-encoder-enabled-channels.md).

Ten samouczek pokazano, jak zarządzać usługi multimediów Azure (AMS) za pomocą narzędzia Azure multimediów usług Eksplorator (AMSE). To narzędzie działa tylko na komputerze z systemem Windows. Jeśli korzystasz z Mac lub Linux, należy utworzyć [kanały](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) i [Programy](media-services-portal-creating-live-encoder-enabled-channel.md#create-and-manage-a-program)Azure portal.

>[AZURE.NOTE]Korzystając z Tricaster do wysyłania w udziału kanału informacyjnego AMS kanałów, które są włączone dla kodowanie live, mogą występować zakłócenia audio/wideo na żywo wydarzenia Jeśli określonych funkcji Tricaster, takich jak szybkie przy wycinaniu między źródła lub przenoszenie się z kreda. Zespołu AMS pracuje nad rozwiązywania tych problemów do tego czasu, jej nie jest zalecane do korzystania z tych funkcji.


##<a name="prerequisites"></a>Wymagania wstępne

- [Tworzenie konta usługi multimediów Azure](media-services-portal-create-account.md)
- Upewnij się, jest punkt końcowy Streaming z co najmniej jedną jednostkę przesyłanie strumieniowe przydzielone. Aby uzyskać więcej informacji zobacz [Zarządzanie Streaming punkty końcowe na koncie usługi multimediów](media-services-portal-manage-streaming-endpoints.md)
- Zainstaluj najnowszą wersję z narzędzia [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) .
- Uruchom narzędzie i łączenia się z kontem AMS.

##<a name="tips"></a>Porady

- Jeśli to możliwe, za pomocą połączenia internetowego hardwired.
- Jest regułą podczas określania wymagań w zakresie przepustowości dwukrotnie przesyłanie strumieniowe bitrates. Gdy nie jest obowiązkowe wymagania, go może pomóc w zmniejszeniu wpływ przeciążeń sieci.
- Kiedy, używając oprogramowania oparte kodery, zamknij wszystkie zbędne programy.

## <a name="create-a-channel"></a>Tworzenie kanału

1.  W narzędziu AMSE przejdź do karty **Live** , a następnie kliknij prawym przyciskiem myszy na obszarze kanału. Wybierz pozycję **Utwórz kanał** z menu.

![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster1.png)

2. Określ nazwę kanału opis pole jest opcjonalne. W obszarze Ustawienia kanału wybierz **Standardowy** dla opcji Live kodowanie z protokołem wprowadzania ustaw **RTMP**. Możesz pozostawić wszystkie inne ustawienia, tak jak.


Upewnij się, że zaznaczono opcję **Uruchom teraz nowy kanał** .

3. Kliknij przycisk **Utwórz kanał**.
![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster2.png)

>[AZURE.NOTE] Kanał może potrwać 20 minut, aby rozpocząć.


Podczas uruchamiania kanału można [skonfigurować koder](media-services-configure-tricaster-live-encoder.md#configure_tricaster_rtmp).

>[AZURE.IMPORTANT] Uwaga Po zainicjowaniu kanału przechodzi w stanie gotowości zaczyna się rozliczenia. Aby uzyskać więcej informacji zobacz [Stany kanału](media-services-manage-live-encoder-enabled-channels.md#states).

##<a id=configure_tricaster_rtmp></a>Konfigurowanie kodera NewTek TriCaster

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


###<a name="configuration-steps"></a>Kroki konfiguracji

1. Tworzenie nowego projektu **NewTek TriCaster** w zależności od używanego jakie źródła wejściowego wideo. 
2. Raz w ramach tego projektu, przycisk **strumienia** Znajdź i kliknij ikonę koła zębatego obok go, aby uzyskać dostęp do menu konfiguracji strumienia.

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster3.png)
3. Po otwarciu menu, kliknij przycisk **Nowy** pod nagłówkiem połączenia. Gdy zostanie wyświetlony monit dla tego typu połączenia, wybierz pozycję **Adobe Flash**.

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster4.png)

4. Kliknij **przycisk OK**.

5. Teraz można zaimportować profilu FMLE, klikając strzałkę listy rozwijanej w obszarze **Profil Streaming** i przechodzenie do **przeglądania**.

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster5.png)

6. Przejdź do zapisywania skonfigurowanego profilu FMLE.
7. Zaznacz go i naciśnij przycisk **OK**.

    Po przesłaniu profilu przejdź do następnego kroku.

6. Uzyskaj wejściowy adres URL kanału aby można było go przypisać Tricaster **RTMP punktu końcowego**.
    
    Przejść z powrotem do narzędzia AMSE i sprawdzić stan realizacji kanału. Po stan zmienił się z **początkowy** **uruchomiony**, możesz uzyskać wprowadzania adresu URL.
      
    Gdy kanał jest uruchomiony, kliknij prawym przyciskiem myszy nazwę kanału, przejdź do pozycji umieść wskaźnik myszy na **URL wprowadzania Kopiuj do Schowka** , a następnie wybierz **Podstawowy wprowadzania adresu URL**.  
    
    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster6.png)

7. Wklej te informacje w polu **Lokalizacja** w obszarze **Serwer Flash** w projekcie Tricaster. Również nadać nazwę strumienia w polu **Identyfikator strumienia** . 

    Jeśli informacje strumienia został dodany do profilu FMLE, go również można zaimportować do tej sekcji, klikając **Ustawień importowania**, przejdź do zapisanego profilu FMLE i kliknij przycisk **OK**. Na podstawie informacji z FMLE należy wypełnić odpowiednie pola Flash Server.

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster7.png)

9. Po zakończeniu kliknij **przycisk OK** u dołu ekranu. Po zakończeniu danych wejściowych audio i wideo do Tricaster Rozpocznij przesyłanie strumieniowe do AMS, klikając przycisk **strumienia** .

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster11.png)

>[AZURE.IMPORTANT]Przed kliknięciem przycisku **strumienia**, **należy** upewnij się, że kanał jest gotowa. 
>Upewnij się również, nie pozostawić kanału w stanie gotowości bez wprowadzania udział kanału informacyjnego przez dłużej niż > 15 minut. 

##<a name="test-playback"></a>Odtwarzanie test
  
1. Przejdź do narzędzia AMSE, a następnie kliknij prawym przyciskiem myszy kanał zostanie sprawdzone. Z menu Umieść wskaźnik myszy na **odtwarzanie podglądu** , a następnie kliknij pozycję **Azure Media Player**.  

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster8.png)

Jeśli strumienia znajduje się odtwarzacza, następnie koder zostało poprawnie skonfigurowane nawiązać AMS. 

Jeśli błąd kanału, należy zresetować i dostosować ustawienia kodera. Zobacz temat [Rozwiązywanie problemów z](media-services-troubleshooting-live-streaming.md) orientacji.  

##<a name="create-a-program"></a>Tworzenie programów

1. Po potwierdzeniu kanału odtwarzania, należy utworzyć program. Na karcie **Live** w narzędziu AMSE w obszarze program kliknij prawym przyciskiem myszy i wybierz pozycję **Utwórz nowy Program**.  

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster9.png)

2. Określ nazwę programu i w razie potrzeby dostosuj **Długość okna archiwum** (który domyślnie 4 godziny). Można także określić miejsce przechowywania lub pozostawić jako domyślny.  
3. Zaznacz pole wyboru **Uruchom Program teraz** .
4. Kliknij przycisk **Utwórz Program**.  
  
    Uwaga: Tworzenie Program ma mniej czasu niż tworzenie kanału.    
 
5. Po uruchomieniu programu Potwierdź odtwarzania, klikając prawym przyciskiem myszy pozycję program i przechodzenie do **odtwarzania programy** , a następnie wybierając **z Azure Media Player**.  
6. Po potwierdzeniu, kliknij prawym przyciskiem myszy program ponownie wybierz pozycję **Skopiuj adres URL wyjścia do Schowka** (lub pobrać tych informacji z **Ustawienia i informacje o programie** opcji z menu). 

Strumień jest teraz gotowy do osadzony w odtwarzaczu lub distributed odbiorcom do wyświetlania live.  


## <a name="troubleshooting"></a>Rozwiązywanie problemów

Zobacz temat [Rozwiązywanie problemów z](media-services-troubleshooting-live-streaming.md) orientacji. 


##<a name="next-step"></a>Następny krok

Przejrzyj ścieżki szkoleniowe dla użytkowników usługi multimediów.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
