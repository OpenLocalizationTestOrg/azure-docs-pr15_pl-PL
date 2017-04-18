<properties 
    pageTitle="Jak przeprowadzić strumieniowego przesyłania live, kodery lokalnego za pomocą portalu Azure | Microsoft Azure" 
    description="Ten samouczek przeprowadzi Cię przez kroki tworzenia kanał, który jest skonfigurowany do dostarczania przekazującej." 
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
    ms.topic="get-started-article"
    ms.date="10/24/2016" 
    ms.author="juliako"/>


#<a name="how-to-perform-live-streaming-with-on-premise-encoders-using-the-azure-portal"></a>Jak przeprowadzić przesyłanie strumieniowe na żywo z kodery lokalnego za pomocą portalu Azure

> [AZURE.SELECTOR]
- [Portal]( media-services-portal-live-passthrough-get-started.md)
- [.NET]( media-services-dotnet-live-encode-with-onpremises-encoders.md)
- [POZOSTAŁE]( https://msdn.microsoft.com/library/azure/dn783458.aspx)

Ten samouczek przeprowadzi Cię przez kroki za pomocą portalu Azure, aby utworzyć **kanał** , który jest skonfigurowany do dostarczania przekazującej. 

##<a name="prerequisites"></a>Wymagania wstępne

Następujące czynności są wymagane do zakończenia tego samouczka:

- Konto Azure. Aby uzyskać szczegółowe informacje zobacz [Azure bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/). 
- Konto usługi multimediów. Aby utworzyć konto usługi multimediów, zobacz [jak utworzyć konto usługi multimediów](media-services-portal-create-account.md).
- Kamera internetowa. Na przykład [encoder Telestream Wirecast](http://www.telestream.net/wirecast/overview.htm).

Zdecydowanie zaleca się Przejrzyj następujące artykuły:

- [Usługi multimediów Azure RTMP pomocy technicznej i kodery Live](https://azure.microsoft.com/blog/2014/09/18/azure-media-services-rtmp-support-and-live-encoders/)
- [Omówienie Live parze przy użyciu usługi multimediów Azure](media-services-manage-channels-overview.md)
- [Przesyłanie strumieniowe z kodery lokalnego, tworzonych przez szybkość transmisji bitów wielu strumieni na żywo](media-services-live-streaming-with-onprem-encoders.md)


##<a id="scenario"></a>Typowy scenariusz przesyłanie strumieniowe live

W poniższej procedurze opisano zadań związanych z tworzenia typowych live przesyłanie strumieniowe aplikacje, które używają kanałów, które są skonfigurowane na dostarczenie przekazującej. Ten samouczek pokazano, jak tworzyć i zarządzać nimi przekazująca kanału i wydarzeń na żywo.

1. Połącz kamery wideo do komputera. Uruchamianie i Konfigurowanie kodera live lokalnego Wyświetla strumienia RTMP lub pofragmentowane MP4 szybkość transmisji bitów wielokrotne. Aby uzyskać więcej informacji zobacz [obsługę RTMP usługi multimediów Azure i kodery Live](http://go.microsoft.com/fwlink/?LinkId=532824).
    
    Ponadto można wykonać ten krok, po utworzeniu kanału.

1. Tworzenie i zacznij przekazująca kanału.
1. Pobierz kanał mogły zjeść tej ostatniej adresu URL. 

    Adres URL ingest jest używana przez koder live wysyłanie strumienia do kanału.
1. Pobieranie adres URL podglądu kanału. 

    Aby sprawdzić, czy kanału poprawnie odbiera strumieniem na żywo za pomocą tego adresu URL.

3. Tworzenie wydarzenia live i programów. 

    Gdy używasz Azure portal, tworzenie zdarzenia na żywo tworzy również środka trwałego. 
      
    >[AZURE.NOTE]Upewnij się mieć co najmniej jedną streaming zastrzeżone jednostki na przesyłanie strumieniowe punktu końcowego, z której chcesz przesyłanie strumieniowe zawartości.
1. Rozpocznij zdarzenie lub programów podczas pracy zacząć strumieniowych i archiwizacji.
2. Opcjonalnie live encoder można sygnalizowane zacząć reklamę. Ogłoszenie zostanie wstawiony w strumienia wyjściowego.
1. Zatrzymywanie zdarzenie lub programów zawsze, gdy chcesz zatrzymać strumieniowych i archiwizacji wydarzenia.
1. Usuwanie wydarzenia lub programów (i opcjonalnie można usunąć elementu).     

>[AZURE.IMPORTANT] Przejrzyj [strumieniowego przesyłania Live, które tworzenie szybkość transmisji bitów wielu strumieni kodery lokalnego](media-services-live-streaming-with-onprem-encoders.md) , aby uzyskać informacje o pojęcia i zagadnienia związane z live streaming z kodery lokalna i przekazujące kanałów.

##<a name="to-view-notifications-and-errors"></a>Aby wyświetlić powiadomienia i błędy

Jeśli chcesz wyświetlić powiadomienia i błędy tworzone przez Azure portal, kliknij ikony powiadomień.

![Powiadomienia](./media/media-services-portal-passthrough-get-started/media-services-notifications.png)

##<a name="configure-streaming-endpoints"></a>Konfigurowanie przesyłania strumieniowego punkty końcowe 

Usługi multimediów zawiera opakowania dynamiczne, umożliwiające dostarczanie usługi MP4s szybkość transmisji bitów wielokrotne w następujących formatach przesyłanie strumieniowe: KRESKI MPEG, HLS, gładkie Streaming lub obr. / min, bez konieczności ponownie utworzyć pakiet do tych streaming format. Dynamiczne opakowań potrzebne do przechowywania i zapłacić za pliki w formacie jednego miejsca do magazynowania i usługi multimediów tworzy i służy właściwej reakcji według żądaniami od klienta.

Aby skorzystać z opakowania dynamiczne, musisz uzyskać co najmniej jedną jednostkę przesyłanie strumieniowe przesyłanie strumieniowe punktu końcowego, z której ma zostać dostarczania zawartości.  

Aby utworzyć i zmień liczbę streaming zastrzeżone jednostki, wykonaj następujące czynności:

1. Zaloguj się w [portalu Azure](https://portal.azure.com/).
1. W oknie **Ustawienia** kliknij pozycję **punkty końcowe strumieniowych**. 

2. Polecenie domyślne streaming punktu końcowego. 

    Zostanie wyświetlone okno **Domyślne STREAMING szczegóły punktu KOŃCOWEGO** .

3. Aby określić liczbę jednostek przesyłanie strumieniowe, przesuń suwak **Streaming jednostki** .

    ![Przesyłanie strumieniowe jednostki](./media/media-services-portal-passthrough-get-started/media-services-streaming-units.png)

4. Kliknij przycisk **Zapisz** , aby zapisać zmiany.

    >[AZURE.NOTE]Przydział wszelkie nowe jednostki może potrwać do 20 minut.
    
##<a name="create-and-start-pass-through-channels-and-events"></a>Tworzenie i uruchomić przekazująca kanałów i zdarzeń

Kanał jest skojarzony z wydarzeń i programy, które umożliwiają kontrolowanie publikowania i przechowywania segmentów strumień na żywo. Kanały Zarządzanie zdarzeń. 
    
Można określić liczbę godzin, aby zachować zarejestrowane zawartości programu ustawienie długości **Okno archiwum** . Tej wartości można ustawić z co najmniej 5 minut maksymalnie 25 godzin. Długość okna archiwum również decyduje o tym że maksymalnej ilości czasu klienci mogą Szukanie wstecz od bieżącej pozycji live. Zdarzenia może zostać uruchomiony przez określoną ilość czasu, ale zawartość, która znajduje się za długość okna wciąż jest pomijany. Tej wartości tej właściwości określa również, jak długo można powiększać manifesty klienta.

Każde zdarzenie jest skojarzony z środka trwałego. Aby opublikować zdarzenia, możesz utworzyć locator na żądanie skojarzony element. O tym locator umożliwia tworzenie przesyłanie strumieniowe adres URL, który można utworzyć dla klientów.

Kanał obsługuje maksymalnie trzy jednocześnie uruchomionego zdarzenia, więc można utworzyć wiele archiwami strumieniu przychodzących. Dzięki temu można opublikować i archiwizowanie różne części zdarzenia, stosownie do potrzeb. Na przykład z wymaganiami firm jest archiwizowania 6 godzin program, ale do emisji tylko ostatnie 10 minut. Aby to zrobić, należy utworzyć dwa jednocześnie uruchomione programy. Jeden program jest ustawiona na archiwizowanie 6 godzin zdarzenia, ale program nie jest opublikowany. Inny program jest ustawiona na archiwizowanie przez 10 minut, a ten program jest opublikowany.

Nie należy ponownie użyć istniejących wydarzeń na żywo. Należy utworzyć i rozpocząć nowego zdarzenia dla każdego zdarzenia.

Rozpocznij zdarzenie, kiedy chcesz uruchomić strumieniowych i archiwizacji. Zatrzymać program, gdy chcesz zatrzymać strumieniowych i archiwizacji wydarzenia. 

Aby usunąć zawartość zarchiwizowane, Zatrzymaj i usunięcie zdarzenia, a następnie usuń skojarzonego elementu. Nie można usunąć środka trwałego, jeśli jest używany przez wydarzenia; Najpierw należy usunąć zdarzenie. 

Nawet po zatrzymaniu i usunięcie zdarzenia użytkowników będzie można przesyłać strumieniowo zarchiwizowane zawartości jako pliku wideo na żądanie, dla jak nie powoduje usunięcia elementu.

Jeśli chcesz zachować zarchiwizowane zawartość, ale nie będzie on dostępny do przesyłania strumieniowego, Usuń przesyłanie strumieniowe locator.

###<a name="to-use-the-portal-to-create-a-channel"></a>Aby utworzyć kanał za pomocą portalu 

W tej sekcji pokazano, jak korzystać z opcji **Szybkiego tworzenia** do utworzenia przekazująca kanału.

Aby uzyskać więcej informacji na temat kanałów przekazująca zobacz [Live streaming z lokalnego kodery przez tworzenie szybkość transmisji bitów wielu strumieni](media-services-live-streaming-with-onprem-encoders.md).

1. W [portalu Azure](https://portal.azure.com/)wybierz swoje konto usługi multimediów Azure.
2. W oknie **Ustawienia** kliknij przycisk **Live streaming**. 

    ![Wprowadzenie](./media/media-services-portal-passthrough-get-started/media-services-getting-started.png)
    
    Zostanie wyświetlone okno **Live streaming** .

3. Kliknij przycisk **Szybkie tworzenie** , aby utworzyć kanał przekazująca z RTMP mogły zjeść tej ostatniej Protocol (protokół).

    Zostanie wyświetlone okno **Tworzenie nowego kanału** .
4. Nadaj nazwę nowego kanału, a następnie kliknij przycisk **Utwórz**. 

    Z RTMP spowoduje to utworzenie przekazująca kanału mogły zjeść tej ostatniej Protocol (protokół).

##<a name="create-events"></a>Tworzenie wydarzenia

1. Wybierz kanał, do którego chcesz dodać wydarzenie.
2. Naciśnij przycisk **Live zdarzenia** .

![Zdarzenia](./media/media-services-portal-passthrough-get-started/media-services-create-events.png)


##<a name="get-ingest-urls"></a>Uzyskiwanie mogły zjeść tej ostatniej adresy URL

Po utworzeniu kanału, możesz uzyskać mogły zjeść tej ostatniej adresy URL, dostarczających na żywo kodera. Koder używa te adresy URL do wprowadzania strumień na żywo.

![Utworzone](./media/media-services-portal-passthrough-get-started/media-services-channel-created.png)

##<a name="watch-the-event"></a>Obejrzyj zdarzenia

Aby obejrzeć wydarzenie, kliknij **czujki** w portalu Azure lub skopiuj adres URL przesyłanie strumieniowe i użyj odtwarzacza wybranych przez użytkownika. 
 
![Utworzone](./media/media-services-portal-passthrough-get-started/media-services-default-event.png)

Zdarzenia na żywo uzyskiwanie automatycznie konwertowane na zawartość na żądanie po zatrzymaniu.

##<a name="clean-up"></a>Oczyść

Aby uzyskać więcej informacji na temat kanałów przekazująca zobacz [Live streaming z lokalnego kodery przez tworzenie szybkość transmisji bitów wielu strumieni](media-services-live-streaming-with-onprem-encoders.md).

- Kanału można zatrzymać tylko wtedy, gdy wszystkie zdarzenia i programy w kanale zostały zatrzymane.  Po zatrzymaniu kanału nie nie spowodować opłat. W razie potrzeby uruchom go ponownie będzie miał taki sam mogły zjeść tej ostatniej adresu URL, nie musisz skonfigurować usługi kodera.
- Kanału można usunąć tylko wtedy, gdy wszystkie wydarzeń na żywo w kanale zostały usunięte.

##<a name="view-archived-content"></a>Wyświetlanie zawartości zarchiwizowane

Nawet po zatrzymaniu i usunięcie zdarzenia użytkowników będzie można przesyłać strumieniowo zarchiwizowane zawartości jako pliku wideo na żądanie, dla jak nie powoduje usunięcia elementu. Nie można usunąć środka trwałego, jeśli jest używany przez wydarzenia; Najpierw należy usunąć zdarzenie. 

Aby zarządzać aktywów, wybierz pozycję **Ustawienia** , a następnie kliknij przycisk **elementy zawartości**.

![Składniki majątku](./media/media-services-portal-passthrough-get-started/media-services-assets.png)

##<a name="next-step"></a>Następny krok

Przejrzyj ścieżki szkoleniowe dla użytkowników usługi multimediów.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
