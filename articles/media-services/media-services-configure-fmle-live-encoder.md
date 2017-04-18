<properties 
    pageTitle="Konfigurowanie kodera FMLE, aby wysłać strumień na żywo pojedynczy szybkość transmisji bitów | Microsoft Azure" 
    description="W tym temacie przedstawiono sposób skonfigurować koder interfejsu Flash Media Live Encoder (FMLE), wysyłanie strumienia pojedynczy szybkość transmisji bitów do AMS kanałów, które są włączone dla kodowanie live." 
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

#<a name="use-the-fmle-encoder-to-send-a-single-bitrate-live-stream"></a>Umożliwia wysłanie strumień na żywo pojedynczy szybkość transmisji bitów FMLE encoder

> [AZURE.SELECTOR]
- [FMLE](media-services-configure-fmle-live-encoder.md)
- [Pierwiastkowej Live](media-services-configure-elemental-live-encoder.md)
- [Tricaster](media-services-configure-tricaster-live-encoder.md)
- [Wirecast](media-services-configure-wirecast-live-encoder.md)

W tym temacie przedstawiono sposób skonfigurować koder [Flash Media Live Encoder](http://www.adobe.com/products/flash-media-encoder.html) (FMLE), wysyłanie strumienia pojedynczy szybkość transmisji bitów do AMS kanałów, które są włączone dla kodowanie live. Aby uzyskać więcej informacji zobacz [Praca z kanałami, które służą do wykonywania Live kodowania z usługi multimediów Azure](media-services-manage-live-encoder-enabled-channels.md).

Ten samouczek pokazano, jak zarządzać usługi multimediów Azure (AMS) za pomocą narzędzia Azure multimediów usług Eksplorator (AMSE). To narzędzie działa tylko na komputerze z systemem Windows. Jeśli korzystasz z Mac lub Linux, należy utworzyć [kanały](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) i [Programy](media-services-portal-creating-live-encoder-enabled-channel.md#create-and-manage-a-program)Azure portal.

Należy zauważyć, że ten samouczek w tym artykule opisano używanie AAC. Jednak FMLE nie obsługuje AAC domyślnie. Należy zakupić dodatek AAC kodowania np MainConcept: [dodatek AAC](http://www.mainconcept.com/products/plug-ins/plug-ins-for-adobe/aac-encoder-fmle.html)

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

![FMLE](./media/media-services-fmle-live-encoder/media-services-fmle1.png)

2. Określ nazwę kanału opis pole jest opcjonalne. W obszarze Ustawienia kanału wybierz **Standardowy** dla opcji Live kodowanie z protokołem wprowadzania ustaw **RTMP**. Możesz pozostawić wszystkie inne ustawienia, tak jak.


Upewnij się, że zaznaczono opcję **Uruchom teraz nowy kanał** .

3. Kliknij przycisk **Utwórz kanał**.
![FMLE](./media/media-services-fmle-live-encoder/media-services-fmle2.png)

>[AZURE.NOTE] Kanał może potrwać 20 minut, aby rozpocząć.


Podczas uruchamiania kanału można [skonfigurować koder](media-services-configure-fmle-live-encoder.md#configure_fmle_rtmp).

>[AZURE.IMPORTANT] Uwaga Po zainicjowaniu kanału przechodzi w stanie gotowości zaczyna się rozliczenia. Aby uzyskać więcej informacji zobacz [Stany kanału](media-services-manage-live-encoder-enabled-channels.md#states).

##<a id=configure_fmle_rtmp></a>Konfigurowanie kodera FMLE

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

1. Przejdź do interfejsu Flash Media Live kodera (FMLE) na komputerze używany.

    Interfejs jest jednej strony głównej ustawień. Pamiętaj o zalecanych ustawienia, aby rozpocząć pracę z strumieniowego przesyłania przy użyciu FMLE.
    
    - Format: H.264 szybkość: 30,00 
    - Rozmiar wejściowy: 1280 x 720 
    - Transmisji: 5000 KB/s (może być określany na podstawie ograniczenia sieciowe)  

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle3.png)

    Podczas korzystania z przeplotem źródeł, zapoznaj się z opcją "Opcji Usuń przeplot" znacznik wyboru

2. Wybierz ikonę klucza francuskiego obok pola Format, te dodatkowe ustawienia powinny być:

    - Profil: główne
    - Poziom: 4.0
    - Częstotliwość kluczową: dwie sekundy 
    
    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle4.png)

3. Ustawienie następujące ważne audio:
    
    - Format: AAC 
    - Przykładowe kurs: Hz 44100 b
    - Szybkość transmisji bitów: 192 kb/s
    
    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle5.png)

6. Uzyskaj wejściowy adres URL kanału aby można było go przypisać FMLE **RTMP punktu końcowego**.
    
    Przejść z powrotem do narzędzia AMSE i sprawdzić stan realizacji kanału. Po stan zmienił się z **początkowy** **uruchomiony**, możesz uzyskać wprowadzania adresu URL.
      
    Gdy kanał jest uruchomiony, kliknij prawym przyciskiem myszy nazwę kanału, przejdź do pozycji umieść wskaźnik myszy na **URL wprowadzania Kopiuj do Schowka** , a następnie wybierz **Podstawowy wprowadzania adresu URL**.  
    
    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle6.png)

7. Wklej te informacje w polu **Adres URL FMS** sekcji dane wyjściowe, a następnie przypisać nazwę strumienia. 

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle7.png)

    W celu zapewnienia dodatkowych nadmiarowości Powtórz te kroki z adresem URL pomocniczej danych wejściowych.
8. Zaznacz pole wyboru **Połącz**.

>[AZURE.IMPORTANT]Przed kliknięciem przycisku **Połącz**, **musisz** upewnij się, że kanał jest gotowy. 
>Upewnij się również, nie pozostawić kanału w stanie gotowości bez wprowadzania udział kanału informacyjnego przez dłużej niż > 15 minut.

##<a name="test-playback"></a>Odtwarzanie test
  
1. Przejdź do narzędzia AMSE, a następnie kliknij prawym przyciskiem myszy kanał zostanie sprawdzone. Z menu Umieść wskaźnik myszy na **odtwarzanie podglądu** , a następnie kliknij pozycję **Azure Media Player**.  

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle8.png)

Jeśli strumienia znajduje się odtwarzacza, następnie koder zostało poprawnie skonfigurowane nawiązać AMS. 

Jeśli błąd kanału, należy zresetować i dostosować ustawienia kodera. Zobacz temat [Rozwiązywanie problemów z](media-services-troubleshooting-live-streaming.md) orientacji.  

##<a name="create-a-program"></a>Tworzenie programów

1. Po potwierdzeniu kanału odtwarzania, należy utworzyć program. Na karcie **Live** w narzędziu AMSE w obszarze program kliknij prawym przyciskiem myszy i wybierz pozycję **Utwórz nowy Program**.  

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle9.png)

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
