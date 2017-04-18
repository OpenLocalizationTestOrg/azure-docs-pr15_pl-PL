<properties 
    pageTitle="Skonfigurować koder Telestream Wirecast, aby wysłać strumień na żywo pojedynczy szybkość transmisji bitów | Microsoft Azure" 
    description="W tym temacie przedstawiono sposób skonfigurować koder live Wirecast do wysyłania strumienia pojedynczy szybkość transmisji bitów AMS kanałów, które są włączone dla kodowanie live. " 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ne" 
    ms.topic="article" 
    ms.date="10/12/2016"
    ms.author="juliako;cenkdin;anilmur"/>

#<a name="use-the-wirecast-encoder-to-send-a-single-bitrate-live-stream"></a>Umożliwia wysłanie strumień na żywo pojedynczy szybkość transmisji bitów Wirecast encoder

> [AZURE.SELECTOR]
- [Wirecast](media-services-configure-wirecast-live-encoder.md)
- [Pierwiastkowej Live](media-services-configure-elemental-live-encoder.md)
- [Tricaster](media-services-configure-tricaster-live-encoder.md)
- [FMLE](media-services-configure-fmle-live-encoder.md)

W tym temacie przedstawiono sposób skonfigurować koder live [Telestream Wirecast](http://www.telestream.net/wirecast/overview.htm) wysyłanie strumienia pojedynczy szybkość transmisji bitów do AMS kanałów, które są włączone dla kodowanie live.  Aby uzyskać więcej informacji zobacz [Praca z kanałami, które służą do wykonywania Live kodowania z usługi multimediów Azure](media-services-manage-live-encoder-enabled-channels.md).

Ten samouczek pokazano, jak zarządzać usługi multimediów Azure (AMS) za pomocą narzędzia Azure multimediów usług Eksplorator (AMSE). To narzędzie działa tylko na komputerze z systemem Windows. Jeśli korzystasz z Mac lub Linux, należy utworzyć [kanały](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) i [Programy](media-services-portal-creating-live-encoder-enabled-channel.md#create-and-manage-a-program)Azure portal.


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

![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast1.png)

2. Określ nazwę kanału opis pole jest opcjonalne. W obszarze Ustawienia kanału wybierz **Standardowy** dla opcji Live kodowanie z protokołem wprowadzania ustaw **RTMP**. Możesz pozostawić wszystkie inne ustawienia, tak jak.


Upewnij się, że zaznaczono opcję **Uruchom teraz nowy kanał** .

3. Kliknij przycisk **Utwórz kanał**.
![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast2.png)

>[AZURE.NOTE] Kanał może potrwać 20 minut, aby rozpocząć.

Podczas uruchamiania kanału można [skonfigurować koder](media-services-configure-wirecast-live-encoder.md#configure_wirecast_rtmp).

>[AZURE.IMPORTANT] Uwaga Po zainicjowaniu kanału przechodzi w stanie gotowości zaczyna się rozliczenia. Aby uzyskać więcej informacji zobacz [Stany kanału](media-services-manage-live-encoder-enabled-channels.md#states).

##<a id=configure_wirecast_rtmp></a>Konfigurowanie kodera Telestream Wirecast

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

1. Otwórz aplikację Telestream Wirecast z tego komputera używane i konfigurowanie przesyłania strumieniowego RTMP.
2. Konfigurowanie wynik, przechodząc do na karcie **dane wyjściowe** i wybierając pozycję **Ustawienia wyjściowe...**.
    
    Upewnij się, że **Docelowy format wyjściowy** jest ustawiona na **Serwerze RTMP**.
3. Kliknij **przycisk OK**.
4. Na stronie Ustawienia Ustaw pole **docelowe** ma być **Usługi multimediów Azure**.
 
    Profil kodowanie jest wstępnie zaznaczone **H.264 Azure 720 p 16:9 (1280 x 720)**. Aby dostosować te ustawienia, wybierz ikonę koła zębatego po prawej stronie listy w dół, a następnie wybierz **Nowy wstępnie ustawione**.

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast3.png)

5. Konfigurowanie ustawień wstępnych kodera.

    Nazwę ustawienia, a następnie sprawdzić ustawienia zalecane następujące elementy:

    **Klip wideo**
    
    - Koder: MainConcept H.264
    - Ramki na sekundę: 30
    - Średnia transmisji: 5000 kbitów/s (może być określany na podstawie ograniczenia sieciowe)
    - Profil: główne
    - Ramka klucza każdej: 60 ramek

    **Audio**

    - Docelowy transmisji: 192 kbitów/s
    - Natężenia próbki: 44 100 kHz
     
    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast4.png)

6. Naciśnij klawisz **Zapisywanie**.

    Pole kodowanie zawiera teraz nowo utworzonego profilu dostępnych do wyboru. 

    Upewnij się, że zaznaczono nowego profilu.

7. Uzyskaj wejściowy adres URL kanału aby można było go przypisać Wirecast **RTMP punktu końcowego**.
    
    Przejść z powrotem do narzędzia AMSE i sprawdzić stan realizacji kanału. Po stan zmienił się z **początkowy** **uruchomiony**, możesz uzyskać wprowadzania adresu URL.
      
    Gdy kanał jest uruchomiony, kliknij prawym przyciskiem myszy nazwę kanału, przejdź do pozycji umieść wskaźnik myszy na **URL wprowadzania Kopiuj do Schowka** , a następnie wybierz **Podstawowy wprowadzania adresu URL**.  
    
    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast6.png)

8. W oknie Wirecast **Ustawienia wyjściowe** Wklej te informacje w polu **adres** w sekcji Wyjście i przypisać nazwę strumienia. 


    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast5.png)

9. Wybierz **przycisk OK**.

10. Na ekranie głównym **Wirecast** Potwierdź źródeł danych wejściowych audio i wideo są gotowe, a następnie naciśnij **strumienia** w lewym górnym rogu.

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast7.png)

>[AZURE.IMPORTANT]Przed kliknięciem przycisku **strumienia**, **należy** upewnij się, że kanał jest gotowa. 
>Upewnij się również, nie pozostawić kanału w stanie gotowości bez wprowadzania udział kanału informacyjnego przez dłużej niż > 15 minut.

##<a name="test-playback"></a>Odtwarzanie test
  
1. Przejdź do narzędzia AMSE, a następnie kliknij prawym przyciskiem myszy kanał zostanie sprawdzone. Z menu Umieść wskaźnik myszy na **odtwarzanie podglądu** , a następnie kliknij pozycję **Azure Media Player**.  

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast8.png)

Jeśli strumienia znajduje się odtwarzacza, następnie koder zostało poprawnie skonfigurowane nawiązać AMS. 

Jeśli błąd kanału, należy zresetować i dostosować ustawienia kodera. Zobacz temat [Rozwiązywanie problemów z](media-services-troubleshooting-live-streaming.md) orientacji.  

##<a name="create-a-program"></a>Tworzenie programów

1. Po potwierdzeniu kanału odtwarzania, należy utworzyć program. Na karcie **Live** w narzędziu AMSE w obszarze program kliknij prawym przyciskiem myszy i wybierz pozycję **Utwórz nowy Program**.  

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast9.png)

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
