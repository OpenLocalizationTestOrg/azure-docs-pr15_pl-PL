<properties
    pageTitle=" Wprowadzenie do dostarczania zawartości na żądanie za pomocą portalu Azure | Microsoft Azure"
    description="Ten samouczek przeprowadzi Cię przez kroki implementacji podstawowej usługi dostarczania zawartości usługi wideo na żądanie (VoD) z aplikacją usługi multimediów Azure (AMS) za pomocą portalu Azure."
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
    ms.topic="get-started-article"
    ms.date="08/30/2016"
    ms.author="juliako"/>


# <a name="get-started-with-delivering-content-on-demand-using-the-azure-portal"></a>Wprowadzenie do dostarczania zawartości na żądanie za pomocą portalu Azure

[AZURE.INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]

Ten samouczek przeprowadzi Cię przez kroki implementacji podstawowej usługi dostarczania zawartości usługi wideo na żądanie (VoD) z aplikacją usługi multimediów Azure (AMS) za pomocą portalu Azure.

> [AZURE.NOTE] Aby użyć tego samouczka, potrzebne jest konto Azure. Aby uzyskać szczegółowe informacje zobacz [Azure bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/). 

Ten samouczek zawiera następujące zadania:

1.  Utwórz konto usługi multimediów Azure.
2.  Skonfiguruj przesyłanie strumieniowe punkt końcowy.
1.  Przekaż plik wideo.
1.  Kodowanie pliku źródłowego do zestawu plików MP4 adaptacyjne szybkość transmisji bitów.
1.  Publikowanie zawartości i uzyskiwanie streaming oraz adresów URL pobierania progresywnego.  
1.  Odtwarzanie zawartości.


## <a name="create-an-azure-media-services-account"></a>Tworzenie konta usługi multimediów Azure

W tej sekcji pokazano, jak utworzyć konto AMS.

1. Zaloguj się w [portalu Azure](https://portal.azure.com/).
2. Kliknij pozycję **+ Nowy** > **Web + Mobile** > **usługi multimediów**.

    ![Tworzenie usługi multimediów](./media/media-services-portal-vod-get-started/media-services-new1.png)

3. **Tworzenie** konta usługi multimediów wprowadź żądane wartości.

    ![Tworzenie usługi multimediów](./media/media-services-portal-vod-get-started/media-services-new3.png)
    
    1. W polu **Nazwa konta**wpisz nazwę nowego konta AMS. Nazwa konta usługi multimediów są wszystkie liczby litera lub litery bez spacji, a jest 3-24 znaków.
    2. W subskrypcji wybierz jeden z różnych subskrypcjach Azure, które mają dostęp do.
    
    2. **Grupa zasobów**wybierz zasób nowym lub istniejącym.  Grupa zasobów to zbiór zasobów, które współużytkują cyklu życia, uprawnień i zasady. Dowiedz się, [w tym miejscu](azure-resource-manager/resource-group-overview.md#resource-groups).
    3. W **lokalizacji**wybierz pozycję regionu geograficznego służy do przechowywania rekordów multimedia i metadanych dla Twojego konta usługi multimediów. Ten obszar jest używany do przetwarzania i przesyłanie strumieniowe multimediów. Tylko dostępne regiony usługi multimediów są wyświetlane w polu listy rozwijanej. 
    
    3. **Miejsca do magazynowania konta**wybierz konto miejsca do magazynowania o podanie magazyn obiektów blob zawartości multimedialnej z konta usługi multimediów. Można wybrać istniejące konto miejsca do magazynowania w tym samym regionu geograficznego jako konta usługi multimediów, lub można utworzyć konto miejsca do magazynowania. W tym samym regionie tworzone jest nowe konto miejsca do magazynowania. Zasady przechowywania nazwy kont są takie same jak w przypadku kont usługi multimediów.

        Dowiedz się więcej o miejsca do magazynowania [w tym miejscu](storage-introduction.md).

    4. Wybierz pozycję **Przypnij do pulpitu nawigacyjnego** , aby wyświetlić postęp wdrożenia konta.
    
7. Kliknij przycisk **Utwórz** w dolnej części formularza.

    Po pomyślnym utworzeniu konta zostanie zmieniony na **Uruchamianie**. 

    ![Ustawienia usługi multimediów](./media/media-services-portal-vod-get-started/media-services-settings.png)

    Aby zarządzać swoim kontem AMS (na przykład przekazywanie klipów wideo, kodowanie składników majątku, monitorować postęp zadania) za pomocą okna **Ustawienia** .

## <a name="manage-keys"></a>Zarządzanie klawiszy

Potrzebujesz nazwę konta i informacje o kluczu podstawowym na programowy dostęp do konta usługi multimediów.

1. W portalu usługi Azure wybierz swoje konto. 

    Po prawej stronie zostanie wyświetlone okno **Ustawienia** . 

2. W oknie **Ustawienia** wybierz pozycję **klawiszy**. 

    **Zarządzaj kluczami** systemu windows zawiera nazwę konta, a zostanie wyświetlona kluczy podstawowych i pomocniczych. 
3. Naciśnij przycisk Kopiuj, aby skopiować wartości.
    
    ![Klucze usługi multimediów](./media/media-services-portal-vod-get-started/media-services-keys.png)

## <a name="configure-streaming-endpoints"></a>Konfigurowanie przesyłania strumieniowego punkty końcowe

Podczas pracy z jedną z najbardziej typowe scenariusze jest przedstawiania wideo za pośrednictwem szybkość transmisji bitów adaptacyjne streaming dla klientów usługi multimediów Azure. Usługi multimediów obsługuje następujące adaptacyjne szybkość transmisji bitów streaming technologii: HTTP Live Streaming (HLS), gładkie Streaming KRESKOWANIA MPEG i obr. / min (w przypadku tylko licencjobiorców Adobe PrimeTime-dostęp).

Usługi multimediów zawiera opakowań dynamiczne pozwala na przedstawianie usługi adaptacyjne szybkość transmisji bitów zawartości MP4 zakodowany w streaming formaty obsługiwane przez usługi multimediów (ŁĄCZNIKA MPEG, HLS, gładkie Streaming, obr. / min) tylko na czas, bez konieczności przechowywanie wersji wstępnie detaliczny każdej z tych streaming format.

Aby skorzystać z dynamicznego opakowania, musisz wykonaj następujące czynności:

- Kodowanie pliku kodery (źródło) do zestawu plików adaptacyjne szybkość transmisji bitów MP4 (kodowania czynności przedstawiono w dalszej części tego samouczka).  
- Tworzenie co najmniej jeden jednostki przesyłanie strumieniowe *Przesyłanie strumieniowe punktu końcowego* z której zamierzasz dostarczania zawartości. Poniżej pokazano, sposób zmieniania liczby jednostek przesyłanie strumieniowe.

Dynamiczne pakowania, potrzebne do przechowywania i zapłacić za pliki w formacie jednego miejsca do magazynowania i usługi multimediów tworzy i służy właściwej reakcji według żądaniami od klienta.

Aby utworzyć i zmień liczbę streaming zastrzeżone jednostki, wykonaj następujące czynności:


1. W oknie **Ustawienia** kliknij pozycję **punkty końcowe strumieniowych**. 

2. Kliknij przycisk domyślne streaming punktu końcowego. 

    Zostanie wyświetlone okno **Domyślne STREAMING szczegóły punktu KOŃCOWEGO** .

3. Aby określić liczbę jednostek przesyłanie strumieniowe, przesuń suwak **Streaming jednostki** .

    ![Przesyłanie strumieniowe jednostki](./media/media-services-portal-vod-get-started/media-services-streaming-units.png)

4. Kliknij przycisk **Zapisz** , aby zapisać zmiany.

    >[AZURE.NOTE]Przydział wszelkie nowe jednostki może potrwać do 20 minut.

## <a name="upload-files"></a>Przekazywanie plików

Aby strumienia wideo przy użyciu usługi multimediów Azure, musisz przekazywanie klipów wideo źródła, kodowanie je do wielu bitrates i publikowanie wynik. Pierwszym krokiem jest objęta w tej sekcji. 

1. W oknie **Ustawienia** kliknij pozycję **zasoby**.

    ![Przekazywanie plików](./media/media-services-portal-vod-get-started/media-services-upload.png)

3. Kliknij przycisk **Przekaż** .

    Zostanie wyświetlone okno **przekazywanie zawartości wideo** .

    >[AZURE.NOTE] Nie ma żadnych limit rozmiaru pliku.
    
4. Przejdź do odpowiedniej wideo na komputerze, zaznacz go i naciśnij przycisk OK.  

    Pozwala uruchomić przekazywanie i można wyświetlić postęp w obszarze Nazwa pliku.  

Po zakończeniu przekazywania pojawi się nowy trwały wyświetlane w oknie **zasoby** . 

## <a name="encode-assets"></a>Kodowanie składników majątku

Podczas pracy z jedną z najbardziej typowe scenariusze dostarczane szybkość transmisji bitów adaptacyjne streaming do klientów usługi multimediów Azure. Usługi multimediów obsługuje następujące adaptacyjne szybkość transmisji bitów streaming technologii: HTTP Live Streaming (HLS), gładkie Streaming KRESKOWANIA MPEG i obr. / min (w przypadku tylko licencjobiorców Adobe PrimeTime-dostęp). Aby przygotować pliki wideo do strumieniowego przesyłania adaptacyjne szybkość transmisji bitów, musisz kodowanie wideo źródła do szybkość transmisji bitów wielu plików. Należy użyć **Media Encoder Standard** kodowania do kodowania wideo.  

Usługi multimediów są także opakowań dynamiczne pozwala na przedstawianie usługi MP4s szybkość transmisji bitów wielokrotne w następujących formatach przesyłanie strumieniowe: KRESKI MPEG, HLS, gładkie Streaming lub obr. / min, bez konieczności ponownie utworzyć pakiet do tych streaming format. Dynamiczne pakowania, potrzebne do przechowywania i zapłacić za pliki w formacie jednego miejsca do magazynowania i usługi multimediów tworzy i służy właściwej reakcji według żądaniami od klienta.

Aby skorzystać z dynamicznego opakowania, musisz wykonaj następujące czynności:

- Kodowanie pliku źródłowego do zestawu szybkość transmisji bitów wielu plików MP4 (kodowania czynności przedstawiono później w tej sekcji).
- Uzyskaj co najmniej jedną jednostkę przesyłanie strumieniowe przesyłanie strumieniowe punktu końcowego, z której ma zostać dostarczania zawartości. Aby uzyskać więcej informacji zobacz [Konfigurowanie przesyłania strumieniowego punktów końcowych](media-services-portal-vod-get-started.md#configure-streaming-endpoints). 

### <a name="to-use-the-portal-to-encode"></a>Aby korzystać z portalu do kodowania

W tej sekcji opisano czynności, które można wykonać w celu kodowania zawartości z Media Encoder standardowy.

1.  W oknie **Ustawienia** wybierz pozycję **elementy zawartości**.  
2.  W oknie **składniki majątku** wybierz pozycję elementu, który ma zostać zakodowany.
3.  Naciśnij przycisk **kodowanie** .
4.  W oknie **kodowanie środka trwałego** wybierz pozycję "Media Encoder standardowy" procesora i ustawienia domyślne. Na przykład jeśli wiesz, wprowadzania wideo ma rozdzielczość 1920 x 1080 pikseli, następnie można "H264 wielu szybkości transmisji bitów 1080p" wstępnie ustawionych. Aby uzyskać więcej informacji na temat ustawień wstępnych, zobacz [ten](https://msdn.microsoft.com/library/azure/mt269960.aspx) artykuł — należy zaznacz najbardziej odpowiednie dla wideo wejściowe. Jeśli masz klipu wideo o niskiej rozdzielczości (640 x 360), a następnie należy nie używać domyślnego "H264 wielu szybkości transmisji bitów 1080p" wstępnie ustawionych.
    
    Ułatwia zarządzanie masz opcję edycji nazwę elementu dane wyjściowe, a nazwa zadania.
        
    ![Kodowanie składników majątku](./media/media-services-portal-vod-get-started/media-services-encode1.png)
5. Naciśnij klawisz **Tworzenie**.

### <a name="monitor-encoding-job-progress"></a>Monitorowanie kodowania postępu zadania

Monitorowanie postępu kodowania zadania, kliknij pozycję **Ustawienia** (w górnej części strony), a następnie wybierz pozycję **zadania**.

![Zadania](./media/media-services-portal-vod-get-started/media-services-jobs.png)

## <a name="publish-content"></a>Publikowanie zawartości

Aby zapewnić użytkownika adres URL, który może służyć do strumienia lub pobrać zawartość, należy najpierw "Opublikuj" do zawartości, tworząc locator. Locator zapewniają dostęp do plików przechowywanych w elementu. Usługi multimediów obsługuje dwa typy Locator: 

- Przesyłanie strumieniowe Locator (OnDemandOrigin), służący do strumieniowego przesyłania adaptacyjne (na przykład do strumienia ŁĄCZNIKA MPEG, HLS lub gładkie Streaming). Tworzenie locator przesyłanie strumieniowe usługi zawartości musi zawierać plik .ism. 
- Stopniowe Locator (SA), używane do dostarczania wideo za pośrednictwem pobierania progresywnego.


Przesyłanie strumieniowe adres URL ma następujący format i służy do odtwarzania wygładzonymi Streaming składników majątku.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Aby utworzyć HLS streaming adresu URL, należy dołączyć (format = m3u8 aapl) do adresu URL.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

Aby utworzyć ŁĄCZNIKA MPEG streaming adresu URL, należy dołączyć (format = mpd czasu csf) do adresu URL.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)


Adres URL skojarzeń zabezpieczeń ma następujący format.

    {blob container name}/{asset name}/{file name}/{SAS signature}

>[AZURE.NOTE] Jeśli portal umożliwia tworzenie Locator przed marca 2015 r, Locator z daty wygaśnięcia dwa lata zostały utworzone.  

Aby zaktualizować datę wygaśnięcia na uchwycie, użyj [pozostałych](http://msdn.microsoft.com/library/azure/hh974308.aspx#update_a_locator ) lub interfejsów API [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) . Po zaktualizowaniu datę wygaśnięcia locator skojarzeń zabezpieczeń zmianę adresu URL.

### <a name="to-use-the-portal-to-publish-an-asset"></a>Aby opublikować środka trwałego za pomocą portalu

Aby opublikować środka trwałego za pomocą portalu, wykonaj następujące czynności:

1. Wybierz pozycję **Ustawienia** > **aktywów**.
1. Wybierz pozycję elementu, który chcesz opublikować.
1. Kliknij przycisk **Publikuj** .
1. Wybierz typ locator.
2. Naciśnij klawisz, **Dodaj**.

    ![Publikowanie](./media/media-services-portal-vod-get-started/media-services-publish1.png)

Adres URL zostanie dodany do listy adresów URL **Opublikowane**.

## <a name="play-content-from-the-portal"></a>Odtwarzanie zawartości z portalu

Azure portal udostępnia odtwarzaczu zawartości, którego można używać do testowania klipu wideo.

Kliknij odpowiedni plik wideo, a następnie kliknij przycisk **Odtwórz** .

![Publikowanie](./media/media-services-portal-vod-get-started/media-services-play.png)

Niektóre kwestie:

- Upewnij się, że został opublikowany wideo.
- To **odtwarzacza** jest odtwarzany w domyślnej streaming punktu końcowego. Jeśli chcesz odtworzyć z punkt końcowy strumieniowych inne niż domyślne, kliknij, aby skopiuj adres URL i użyć innego odtwarzacza. Na przykład [Usługi Azure odtwarzacza](http://amsplayer.azurewebsites.net/azuremediaplayer.html).

##<a name="next-steps"></a>Następne kroki

Przejrzyj ścieżki szkoleniowe dla użytkowników usługi multimediów.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


