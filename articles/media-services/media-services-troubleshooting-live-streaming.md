<properties 
    pageTitle="Rozwiązywania problemów — przewodnik dotyczący live streaming | Microsoft Azure" 
    description="W tym temacie przedstawiono sugestie dotyczące rozwiązywania problemów przesyłanie strumieniowe live." 
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
    ms.date="10/12/2016"  
    ms.author="juliako"/>

#<a name="troubleshooting-guide-for-live-streaming"></a>Rozwiązywanie problemów z przewodnika do przesyłania strumieniowego live

W tym temacie przedstawiono sugestie dotyczące rozwiązywania problemów, przesyłanie strumieniowe live.

## <a name="issues-related-to-on-premises-encoders"></a>Problemy związane z lokalnego kodery 

W tej sekcji przedstawiono sugestie dotyczące rozwiązywania problemów związanych z kodery lokalnego, które są skonfigurowane do wysyłania strumienia pojedynczy szybkość transmisji bitów AMS kanałów, których włączono obsługę kodowanie live.

###<a name="problem-would-like-to-see-logs"></a>Problem: Chcesz wyświetlić dzienniki 

- **Potencjalny problem**: nie można znajdowanie encoder dzienniki, które mogą pomóc do rozwiązywania problemów.
    
    - **Telestream Wirecast**: można zazwyczaj znaleźć dzienniki w obszarze C:\Users\{username} \AppData\Roaming\Wirecast\ 
    - **Live stanie wolnym**: można znaleźć zawiera łącza do dzienników w portalu zarządzania. Kliknij **statystykę**, a następnie **dzienników**. Na stronie **Pliki dziennika** zostanie wyświetlona lista dzienników dla wszystkich elementów LiveEvent; Kliknij jeden z pasującymi bieżącej sesji. 
    - **Flash Media Live Encoder**: **... Katalog dziennika** można znaleźć, przechodząc na kartę **Kodowanie dziennika** .
    
###<a name="problem-there-is-no-option-for-outputting-a-progressive-stream"></a>Problem: Nie jest wyświetlana opcja wyprowadzania stopniowego strumienia

- **Potencjalny problem**: encoder używany automatycznie nie bez przeplotu. 

    **Opisane procedury rozwiązywania problemów**: Poszukaj usuwania przeplotu opcji interfejsu kodera. Po włączeniu Anuluj przeplot ponowne sprawdzenie ustawienia wyjściowe progresywnego. 
 
###<a name="problem-tried-several-encoder-output-settings-and-still-unable-to-connect"></a>Problem: Uruchomienie kilku ustawienia wyjściowe encoder i nadal nie możesz połączyć. 

- **Potencjalny problem**: Azure kodowania kanału nie został poprawnie zresetować. 

    **Opisane procedury rozwiązywania problemów**: Upewnij się, że nie jest już naciśnięcie kodera do AMS, należy zatrzymać i resetowanie kanału. Po ponownym uruchomieniu spróbuj nawiązywanie połączenia z kodera z nowymi ustawieniami. Jeśli to nadal nie rozwiąże problemu, spróbuj utworzyć całkowicie nowy kanał, czasami kanałów może zostać uszkodzony po kilku próbach nie powiodło się.  

- **Potencjalny problem**: rozmiar GOP lub ustawienia klucza ramki nie są optymalne. 

    **Opisane procedury rozwiązywania problemów**: interwał rozmiar lub kluczową GOP zalecane jest dwie sekundy. Niektóre kodery Oblicz to ustawienie w polu Liczba ramek, gdy inne osoby za pomocą sekund. Na przykład: dla 30 k/s, rozmiar GOP będzie 60 ramki, która jest równoważna dwie sekundy.  
     
- **Potencjalny problem**: zamknięte porty blokują strumienia. 

    **Opisane procedury rozwiązywania problemów**: podczas przesyłania strumieniowego za pośrednictwem RTMP, sprawdź ustawienia zapory lub serwera proxy, aby potwierdzić, że porty wyjściowe 1935 i 1936 są otwarte. Gdy używasz, RTP streaming, upewnij się, że port wyjściowy 2010 jest otwarty. 


###<a name="problem-when-configuring-the-encoder-to-stream-with-the-rtp-protocol-there-is-no-place-to-enter-a-host-name"></a>Problem: Podczas konfigurowania encoder strumieniu przy użyciu protokołu RTP, istnieje pozwalających wprowadzona nazwa hosta. 

- **Potencjalny problem**: wiele RTP kodery nie zezwalać na nazwy hosta, a adres IP muszą być pobrana.  

    **Opisane procedury rozwiązywania problemów**: Aby znaleźć adres IP, otwórz wiersz polecenia na dowolnym komputerze. Aby to zrobić w systemie Windows, otwórz uruchamianie Uruchom (WIN + R), a następnie wpisz "cmd" otworzyć.  

    Po otwarciu w wierszu polecenia wpisz "Ping [nazwa hosta AMS]". 

    Nazwa hosta mogą być uzyskane przez pominięcie numer portu z Azure mogły zjeść tej ostatniej adresu URL, jako wyróżnione w poniższym przykładzie: 

    RTP://Test2-amstest009.RTP.Channel.mediaservices.Windows.NET:2010- 

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle10.png)

###<a name="problem-unable-to-playback-the-published-stream"></a>Problem: Nie można odtwarzanie opublikowany strumień.
 
- **Potencjalny problem**: jest nie Streaming punktu końcowego uruchomiony lub jest nie przesyłanie strumieniowe jednostkach (skali) przydzielone. 

    **Opisane procedury rozwiązywania problemów**: Przejdź do karty "Streaming punktu końcowego" w narzędziu AMSE i upewnij się, jest punkt końcowy Streaming z jedną jednostkę przesyłanie strumieniowe. 
    


>[AZURE.NOTE] Jeśli po wykonaniu kroków, które nadal nie możesz pomyślnie przesyłania strumieniowego, przedstawia bilet pomocy technicznej, za pomocą portalu Azure.

##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
