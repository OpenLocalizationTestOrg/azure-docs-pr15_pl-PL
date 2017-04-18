<properties 
    pageTitle="Jak przeprowadzić live streaming tworzenie szybkość transmisji bitów wielu strumieni Portal Azure za pomocą usługi multimediów Azure | Microsoft Azure" 
    description="Ten samouczek przeprowadzi Cię przez kroki tworzenia kanał, który odbiera strumień na żywo szybkość transmisji bitów pojedyncze i kodowane strumieniu szybkość transmisji bitów wielokrotne za pomocą portalu Azure." 
    services="media-services" 
    documentationCenter="" 
    authors="anilmur" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article"
    ms.date="10/24/2016"
    ms.author="juliako"/>


#<a name="how-to-perform-live-streaming-using-azure-media-services-to-create-multi-bitrate-streams-with-the-azure-portal"></a>Jak przeprowadzić live streaming tworzenie szybkość transmisji bitów wielu strumieni Portal Azure za pomocą usługi multimediów Azure

> [AZURE.SELECTOR]
- [Portal](media-services-portal-creating-live-encoder-enabled-channel.md)
- [.NET](media-services-dotnet-creating-live-encoder-enabled-channel.md)
- [INTERFEJSU API USŁUGI REST](https://msdn.microsoft.com/library/azure/dn783458.aspx)

Ten samouczek przeprowadzi Cię przez kroki tworzenia **kanału** , który odbiera strumień na żywo szybkość transmisji bitów pojedyncze i kodowane szybkość transmisji bitów wielokrotne strumieniu.

>[AZURE.NOTE]Uzyskać więcej informacji pojęć związanych z kanałów, do których włączono usługę live kodowanie zobacz [Przesyłanie strumieniowe przy użyciu usługi multimediów Azure, aby utworzyć szybkość transmisji bitów wielu strumieni na żywo](media-services-manage-live-encoder-enabled-channels.md).

##<a name="common-live-streaming-scenario"></a>Typowy scenariusz przesyłanie strumieniowe Live

Poniżej przedstawiono ogólne etapy tworzenia live strumieniowo aplikacje.

>[AZURE.NOTE] Maksymalny czas trwania zalecane zdarzenia na żywo jest obecnie, 8 godzin. Jeśli jest potrzebne do uruchamiania kanału przez dłuższy czas, skontaktuj się z amslived z witryny Microsoft.com.

1. Połącz kamery wideo do komputera. Uruchamianie i Konfigurowanie kodera live lokalnego można wyjściowy strumienia pojedynczy szybkość transmisji bitów w jednym z następujących protokołów: RTMP, gładkie Streaming lub RTP (MPEG-TS). Aby uzyskać więcej informacji zobacz [obsługę RTMP usługi multimediów Azure i kodery Live](http://go.microsoft.com/fwlink/?LinkId=532824).
    
    Ponadto można wykonać ten krok, po utworzeniu kanału.

1. Tworzenie i zacznij kanału. 

1. Pobierz kanał mogły zjeść tej ostatniej adresu URL. 

    Adres URL ingest jest używana przez koder live wysyłanie strumienia do kanału.
1. Pobieranie adres URL podglądu kanału. 

    Aby sprawdzić, czy kanału poprawnie odbiera strumieniem na żywo za pomocą tego adresu URL.

3. Tworzenie wydarzenia i program (także spowoduje utworzenie środka trwałego). 
1. Publikowanie odpowiednie zdarzenie (utworzy locator na żądanie skojarzony element).  

    Upewnij się mieć co najmniej jedną streaming zastrzeżone jednostki na przesyłanie strumieniowe punktu końcowego, z której chcesz przesyłanie strumieniowe zawartości.
1. Rozpocznij zdarzenie, kiedy chcesz uruchomić strumieniowych i archiwizacji.
2. Opcjonalnie live encoder można sygnalizowane zacząć reklamę. Ogłoszenie zostanie wstawiony w strumienia wyjściowego.
1. Zatrzymywanie zdarzenia zawsze, gdy chcesz zatrzymać strumieniowych i archiwizacji wydarzenia.
1. Usunięcie zdarzenia (i opcjonalnie można usunąć elementu).   

##<a name="in-this-tutorial"></a>W tym samouczku

W tym samouczku Azure portal służy do wykonywania następujących zadań: 

2.  Skonfiguruj przesyłanie strumieniowe punktów końcowych.
3.  Tworzenie kanału, który jest skonfigurowany do przeprowadzania kodowanie live.
1.  Uzyskać adres URL mogły zjeść tej ostatniej w celu dostarczania go na żywo kodera. Przy użyciu tego adresu URL live encoder mogły zjeść tej ostatniej strumienia do tego kanału. .
1.  Tworzenie wydarzenia i programów (i środka trwałego)
1.  Publikowanie elementu i uzyskiwanie streaming adresy URL  
1.  Odtwarzanie zawartości 
2.  Czyszczenie

##<a name="prerequisites"></a>Wymagania wstępne

Następujące czynności są wymagane do zakończenia tego samouczka.

- Aby użyć tego samouczka, potrzebne jest konto Azure. Jeśli nie masz konta, możesz utworzyć bezpłatne konto wersji próbnej na kilka minut. Aby uzyskać szczegółowe informacje zobacz [Azure bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/).
- Konto usługi multimediów. Aby utworzyć konto usługi multimediów, zobacz [Tworzenie konta](media-services-portal-create-account.md).
- Kamery internetowej i kodera, które może przesyłać strumień na żywo pojedynczy szybkość transmisji bitów.

##<a name="configure-streaming-endpoints"></a>Konfigurowanie przesyłania strumieniowego punkty końcowe 

Usługi multimediów zawiera dynamiczne opakowań, umożliwiające dostarczanie usługi MP4s szybkość transmisji bitów wielokrotne w następujących formatach przesyłanie strumieniowe: KRESKI MPEG, HLS, gładkie Streaming lub obr. / min, bez konieczności ponownego pakietów do tych streaming format. Z dynamicznego opakowań potrzebne do przechowywania i zapłacić za pliki w formacie jednego miejsca do magazynowania i usługi multimediów będą tworzyć i służyć właściwej reakcji według żądaniami od klienta.

Aby skorzystać z opakowania dynamiczne, musisz uzyskać co najmniej jedną jednostkę przesyłanie strumieniowe przesyłanie strumieniowe punktu końcowego, z której ma zostać dostarczania zawartości.  

Aby utworzyć i zmień liczbę streaming zastrzeżone jednostki, wykonaj następujące czynności:

1. Zaloguj się w [portalu Azure](https://portal.azure.com/) i wybierz konto AMS.
1. W oknie **Ustawienia** kliknij pozycję **punkty końcowe strumieniowych**. 

2. Polecenie domyślne streaming punktu końcowego. 

    Zostanie wyświetlone okno **Domyślne STREAMING szczegóły punktu KOŃCOWEGO** .

3. Aby określić liczbę jednostek przesyłanie strumieniowe, przesuń suwak **Streaming jednostki** .

    ![Przesyłanie strumieniowe jednostki](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-streaming-units.png)

4. Kliknij przycisk **Zapisz** , aby zapisać zmiany.

    >[AZURE.NOTE]Przydział wszelkie nowe jednostki może potrwać do 20 minut.

##<a name="create-a-channel"></a>Tworzenie kanału

1. W [portalu Azure](https://portal.azure.com/)wybierz pozycję usługi multimediów, a następnie kliknij polecenie nazwę swojego konta usługi multimediów.
2. Wybierz pozycję **Przesyłanie strumieniowe na żywo**.
3. Wybierz polecenie **Utwórz niestandardowy**. Ta opcja pozwoli utworzyć kanał, który jest włączona dla kodowanie live.

    ![Tworzenie kanału](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-channel.png)
    
4. Kliknij przycisk **Ustawienia**.
    
    1.  Wybierz typ kanału **Live kodowanie** . Ten typ Określa, czy chcesz utworzyć kanał, który jest włączona dla kodowanie live. Oznacza to, że przychodzące strumienia pojedynczy szybkość transmisji bitów jest wysyłane do kanału i kodowane strumienia szybkość transmisji bitów wielu przy użyciu ustawień określonych encoder live. Aby uzyskać więcej informacji zobacz [Przesyłanie strumieniowe przy użyciu usługi multimediów Azure, aby utworzyć szybkość transmisji bitów wielu strumieni na żywo](media-services-manage-live-encoder-enabled-channels.md). Kliknij przycisk OK.
    2. Określ nazwę kanału.
    3. Kliknij przycisk OK u dołu ekranu.
    
5. Wybierz kartę **Ingest** .

    1. Na tej stronie można wybrać protokół przesyłania strumieniowego. Typ kanału **Live kodowanie** prawidłowego protokołu opcje są następujące:
        
        - Pojedyncza szybkość transmisji bitów Fragmented MP4 (wygładzonymi przesyłanie strumieniowe)
        - Pojedyncza szybkość transmisji bitów RTMP
        - RTP (MPEG-TS): Strumienia transportu MPEG-2 na RTP.
        
        Aby uzyskać szczegółowe informacje o każdym Protocol (protokół) zobacz [streaming za pomocą usługi multimediów Azure, aby utworzyć szybkość transmisji bitów wielu strumieni na żywo](media-services-manage-live-encoder-enabled-channels.md).
    
        Nie można zmienić opcji protokołu podczas kanału lub jego skojarzony zdarzeń i programy są uruchomione. Jeśli jest wymagane różne protokoły, należy utworzyć osobne kanały dla każdego protokołu przesyłanie strumieniowe.  

    2. Ograniczenie IP można stosować na ingest. 
    
        Można zdefiniować adresy IP, które będą mogli mogły zjeść tej ostatniej klipu wideo do tego kanału. Dozwolone adresy IP można określić jako jeden adres IP (np. "10.0.0.1"), zakresu adresów IP przy użyciu adresu IP i maski podsieci CIDR (np. "10.0.0.1/22") lub zakresu adresów IP przy użyciu adresu IP i maski kropkowaną podsieci dziesiętną (np. "10.0.0.1(255.255.252.0)').

        Jeśli określono bez adresów IP, a nie Definicja reguły bez adresu IP będą mogli. Aby zezwolić na dowolny adres IP, Utwórz regułę i ustaw 0.0.0.0/0.

6. Na karcie **Podgląd** stosowanie ograniczeń adresów IP w wersji preview.
7. Na karcie **kodowanie** Określ ustawienie kodowania. 

    Obecnie tylko system jest ustawienie wstępne, możesz wybrać **domyślny 720 p**. Aby określić niestandardowe ustawienie wstępne, otwórz bilet pomocy technicznej firmy Microsoft. Wpisz nazwę ustawienie utworzenia. 

>[AZURE.NOTE] Obecnie start kanał może potrwać do 30 minut. Resetowanie kanał może potrwać do 5 minut.

Po utworzeniu kanału można kliknij kanał i wybierz pozycję **Ustawienia** , gdzie można przeglądać konfiguracji kanałów. 

Aby uzyskać więcej informacji zobacz [Przesyłanie strumieniowe przy użyciu usługi multimediów Azure, aby utworzyć szybkość transmisji bitów wielu strumieni na żywo](media-services-manage-live-encoder-enabled-channels.md).


##<a name="get-ingest-urls"></a>Uzyskiwanie mogły zjeść tej ostatniej adresy URL

Po utworzeniu kanału, możesz uzyskać mogły zjeść tej ostatniej adresy URL, dostarczających na żywo kodera. Koder używa te adresy URL do wprowadzania strumień na żywo.

![ingesturls](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-ingest-urls.png)


##<a name="create-and-manage-events"></a>Tworzenie i zarządzanie nimi wydarzeń

###<a name="overview"></a>Omówienie

Kanał jest skojarzony z wydarzeń i programy, które umożliwiają kontrolowanie publikowania i przechowywania segmentów strumień na żywo. Kanały Zarządzanie zdarzeń i programy. Relacja kanału i Program jest bardzo podobne do tradycyjnych multimediów, gdzie kanał zawiera stałą strumienia zawartości i program jest ograniczone do niektórych czasowy zdarzenia, w tym kanale.

Można określić liczbę godzin, aby zachować zarejestrowane zawartości dla zdarzenia ustawienie długości **Okno archiwum** . Tej wartości można ustawić z co najmniej 5 minut maksymalnie 25 godzin. Długość okna archiwum również decyduje o tym że maksymalnej ilości czasu klienci mogą Szukanie wstecz od bieżącej pozycji live. Zdarzenia może zostać uruchomiony przez określoną ilość czasu, ale zawartość, która znajduje się za długość okna wciąż jest pomijany. Tej wartości tej właściwości określa również, jak długo można powiększać manifesty klienta.

Każde zdarzenie jest skojarzony z środka trwałego. Aby opublikować wydarzenia możesz utworzyć locator na żądanie skojarzony element. O tym locator umożliwi tworzenie przesyłanie strumieniowe adres URL, który można utworzyć dla klientów.

Kanał obsługuje maksymalnie trzy jednocześnie uruchomionego zdarzenia, więc można utworzyć wiele archiwami strumieniu przychodzących. Dzięki temu można opublikować i archiwizowanie różne części zdarzenia, stosownie do potrzeb. Na przykład z wymaganiami firm jest archiwizowania 6 godzin zdarzenia, ale do emisji tylko ostatnie 10 minut. Aby to zrobić, musisz utworzyć dwa jednocześnie uruchomiony zdarzenia. Jedno zdarzenie jest ustawiona na archiwizowanie 6 godzin zdarzenia, ale program nie jest opublikowany. Zdarzenie jest ustawiona na archiwizowanie przez 10 minut, a ten program jest opublikowany.

Nie należy ponownie użyć istniejących programów o nowych wydarzeniach. Zamiast tego tworzenie i uruchamianie nowego programu dla każdego zdarzenia.

Rozpocznij zdarzenie i programów po zacząć strumieniowych i archiwizacji. Zatrzymywanie zdarzenia zawsze, gdy chcesz zatrzymać strumieniowych i archiwizacji wydarzenia. 

Aby usunąć zawartość zarchiwizowane, Zatrzymaj i usunięcie zdarzenia, a następnie usuń skojarzonego elementu. Nie można usunąć środka trwałego, jeśli jest używany przez wydarzenia; Najpierw należy usunąć zdarzenie. 

Nawet po zatrzymaniu i usunięcie zdarzenia użytkowników będzie można przesyłać strumieniowo zarchiwizowane zawartości jako pliku wideo na żądanie, dla jak nie powoduje usunięcia elementu.

Jeśli chcesz zachować zarchiwizowane zawartość, ale nie będzie on dostępny do przesyłania strumieniowego, Usuń przesyłanie strumieniowe locator.

###<a name="createstartstop-events"></a>Tworzenie i Rozpocznij i Zatrzymaj zdarzenia

Po umieszczeniu strumienia ułożony w kanale, tworząc trwałe, programu i przesyłanie strumieniowe Locator można rozpocząć przesyłanie strumieniowe zdarzenia. Zostanie archiwizowanie strumienia i udostępnić użytkownikom za pośrednictwem punktu końcowego Streaming. 

Istnieją dwa sposoby uruchamiania zdarzeń: 

1. Na stronie **kanału** naciśnij klawisz **Live zdarzenia** , aby dodać nowe zdarzenie.

    Określ: Nazwa zdarzenia, nazwy zasobów, okno archiwum i opcję szyfrowania.
    
    ![createprogram](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-program.png)
    
    Jeśli **Publikowanie tego zdarzenia na żywo teraz** zaznaczone, będzie utworzyć zdarzenie publikowania adresy URL.
    
    Możesz nacisnąć, **Rozpocznij**, gdy zechcesz strumienia zdarzenia.

    Po uruchomieniu zdarzenia, możesz nacisnąć **czujki** , aby rozpocząć odtwarzanie zawartości.

2. Możesz również używać skrótu i naciśnij przycisk **Przejdź Live** na stronie **kanału** . Spowoduje to utworzenie domyślnego trwały, Program, a także Streaming.

    Wydarzenie ma nazwę **domyślnego** i okno archiwum jest ustawiona na 8 godzin.

Możesz obejrzeć opublikowanych zdarzenie ze strony **Live zdarzenia** . 

Po kliknięciu **Poza Air**zatrzyma wszystkie zdarzenia live. 


##<a name="watch-the-event"></a>Obejrzyj zdarzenia

Aby obejrzeć wydarzenie, kliknij **czujki** w portalu Azure lub skopiuj adres URL przesyłanie strumieniowe i użyj odtwarzacza wybranych przez użytkownika. 
 
![Utworzone](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-play-event.png)

Zdarzenia na żywo automatycznie konwertuje zdarzeń do zawartości na żądanie po zatrzymaniu.

##<a name="clean-up"></a>Oczyść

Jeśli są wykonywane przesyłanie strumieniowe zdarzeń i chcesz oczyścić zasoby obsługi administracyjnej wcześniej, wykonaj poniższą procedurę.

- Zatrzymywanie publikację strumienia z kodera.
- Zatrzymywanie kanału. Po zatrzymaniu kanału nie wiążą opłat. W razie potrzeby uruchom go ponownie będzie miał taki sam mogły zjeść tej ostatniej adresu URL, nie musisz skonfigurować usługi kodera.
- Możesz wyłączyć punkt końcowy Streaming, chyba że chcesz kontynuować zapewnienie archiwum na żywo wydarzenie jako strumień na żądanie. Jeśli kanał nie jest w stanie zatrzymania, nie będzie powodowało co opłat.
  
##<a name="view-archived-content"></a>Wyświetlanie zawartości zarchiwizowane

Nawet po zatrzymaniu i usunięcie zdarzenia użytkowników będzie można przesyłać strumieniowo zarchiwizowane zawartości jako pliku wideo na żądanie, dla jak nie powoduje usunięcia elementu. Nie można usunąć środka trwałego, jeśli jest używany przez wydarzenia; Najpierw należy usunąć zdarzenie. 

Aby zarządzać aktywów, wybierz pozycję **Ustawienia** , a następnie kliknij przycisk **elementy zawartości**.

![Składniki majątku](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-assets.png)

##<a name="considerations"></a>Zagadnienia dotyczące

- Maksymalny czas trwania zalecane zdarzenia na żywo jest obecnie, 8 godzin. Jeśli jest potrzebne do uruchamiania kanału przez dłuższy czas, skontaktuj się z amslived z witryny Microsoft.com.
- Upewnij się mieć co najmniej jedną streaming zastrzeżone jednostki na przesyłanie strumieniowe punktu końcowego, z której chcesz przesyłanie strumieniowe zawartości.


##<a name="next-step"></a>Następny krok

Przejrzyj ścieżki szkoleniowe dla użytkowników usługi multimediów.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

 
