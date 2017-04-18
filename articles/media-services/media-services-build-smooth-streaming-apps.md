<properties 
    pageTitle="Wygładzanie Streaming samouczek aplikacji ze Sklepu Windows | Microsoft Azure" 
    description="Dowiedz się, jak utworzyć aplikację C# ze Sklepu Windows z formant XML MediaElement do strumienia płynnego odtwarzania zawartości za pomocą usługi multimediów Azure." 
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
    ms.date="09/26/2016"  
    ms.author="juliako"/>



#<a name="how-to-build-a-smooth-streaming-windows-store-application"></a>Sposoby tworzenia sprawne Streaming aplikacji ze Sklepu Windows

Gładkie Streaming klienta SDK dla systemu Windows 8 umożliwia programistom na tworzenie aplikacji ze Sklepu Windows, które można odtwarzać na żądanie i rzeczywistych wygładzonymi strumieniowych. Oprócz podstawowe odtwarzania wygładzonymi strumieniowego przesyłania zawartości zestawu SDK udostępnia zaawansowanych funkcji, takich jak Microsoft PlayReady ochrony, jakość dźwięku poziom ograniczeń, Live DVR strumienia przełączania, wykrywanie aktualizacji stanu (na przykład zmiany na poziomie jakości) i błędów i tak dalej. Aby uzyskać więcej informacji na obsługiwanych funkcji zobacz [informacje o wersji](http://www.iis.net/learn/media/smooth-streaming/smooth-streaming-client-sdk-for-windows-8-release-notes). Aby uzyskać więcej informacji zobacz [Framework Player dla systemu Windows 8](http://playerframework.codeplex.com/). 

Ten samouczek zawiera cztery lekcji:

1. Tworzenie podstawowego wygładzonymi przesyłanie strumieniowe aplikacji ze sklepu
2. Dodawanie suwaka, aby kontrolować postęp multimediów
3. Wybierz pozycję wygładzonymi Streaming strumieni
4. Wybierz Streaming gładkie ścieżki

##<a name="prerequisites"></a>Wymagania wstępne

- Windows 8 w wersji 32-bitowej lub 64-bitowej. [Windows 8 Enterprise oceny](http://msdn.microsoft.com/evalcenter/jj554510.aspx) można uzyskać w witrynie MSDN.
- Program Visual Studio 2012 lub Visual Studio Express 2012 (lub nowszy). Zostanie wyświetlony wersji próbnej [tutaj](http://www.microsoft.com/visualstudio/11/downloads).
- [Wygładzanie Microsoft Streaming klienta SDK dla systemu Windows 8](http://visualstudiogallery.msdn.microsoft.com/04423d13-3b3e-4741-a01c-1ae29e84fea6?SRC=Homehttp://visualstudiogallery.msdn.microsoft.com/04423d13-3b3e-4741-a01c-1ae29e84fea6?SRC=Home).


Ukończony rozwiązanie dla każdej lekcji można pobrać z przykłady kodu dewelopera MSDN (kod Gallery): 

- [Lekcja 1](http://code.msdn.microsoft.com/Smooth-Streaming-Client-0bb1471f) - Streaming Media Player sprawne prosty systemu Windows 8 
- [Lekcja 2](http://code.msdn.microsoft.com/A-simple-Windows-8-Smooth-ee98f63a) - prosty systemu Windows 8 wygładzonymi Streaming odtwarzacza multimedialnego z suwak kontrolować, 
- [Lekcja 3](http://code.msdn.microsoft.com/A-Windows-8-Smooth-883c3b44) - Streaming odtwarzacza z zaznaczonym strumienia sprawne systemu Windows 8  
- [Lekcji 4](http://code.msdn.microsoft.com/A-Windows-8-Smooth-aa9e4907) - Streaming odtwarzacza z zaznaczonym śledzenia sprawne systemu Windows 8.

##<a name="lesson-1-create-a-basic-smooth-streaming-store-application"></a>Lekcja 1: Tworzenie podstawowego wygładzonymi przesyłanie strumieniowe aplikacji ze sklepu

W tej lekcji użytkownik utworzy aplikacji ze Sklepu Windows z formantem MediaElement odtwarzania wygładzonymi strumienia zawartości.  Wygląda działającej aplikacji:

![Przykład aplikacji ze Sklepu Windows Streaming gładkie][PlayerApplication]
 
Aby uzyskać więcej informacji o projektowaniu aplikacji ze Sklepu Windows zobacz [opracowywanie doskonałe aplikacje dla systemu Windows 8](http://msdn.microsoft.com/windows/apps/br229512.aspx). W tej lekcji zawiera następujące procedury:

1.  Tworzenie projektu ze Sklepu Windows
2.  Projektowanie interfejsu użytkownika (XAML)
3.  Modyfikowanie kodu źródłowego pliku
4.  Można skompilować i przetestować aplikację

**Aby utworzyć projekt ze Sklepu Windows**

1.  Uruchom program Visual Studio 2012 lub nowszym.
2.  W menu **plik** kliknij polecenie **Nowy**, a następnie kliknij **Projekt**.
3.  W oknie dialogowym Nowy projekt wpisz lub wybierz następujące wartości:

Nazwa|Wartość
---|---
Grupa szablonów|Zainstalowane i szablony i Visual C# / magazynu systemu Windows
Szablon|Pustej aplikacji (XAML)
Nazwa|SSPlayer
Lokalizacja|C:\SSTutorials
Nazwa rozwiązania|SSPlayer
Utwórz katalog rozwiązania|(zaznaczone)

4.  Kliknij **przycisk OK**.

**Aby dodać odwołanie do płynnego Streaming SDK klienta**

1.  Korzystając z Eksploratora rozwiązanie kliknij prawym przyciskiem myszy **SSPlayer**, a następnie kliknij **Dodaj odwołanie**.
2.  Wpisz lub wybierz następujące wartości:

Nazwa|Wartość
---|---
Grupa odwołania|Rozszerzenia i systemu Windows
Odwołania|Zaznacz opcję Wygładzanie Microsoft Streaming klienta SDK dla systemu Windows 8 i pakiet Microsoft Visual C++ Runtime
    
3.  Kliknij **przycisk OK**. 

Po dodaniu odwołania, należy wybrać docelowej platformy (x64 lub x86), Dodawanie odwołania, nie działa w konfiguracji platformy Procesora dowolny.  W oknie Eksplorator rozwiązań zostanie wyświetlony żółty znak ostrzeżenie dla tych dodana odwołania.

**Aby zaprojektować odtwarzacza interfejsu użytkownika**

1.  Korzystając z Eksploratora rozwiązanie kliknij dwukrotnie **MainPage.xaml** , aby otworzyć go w widoku projektu.
2.  Znajdź ** &lt;siatki&gt; ** i ** &lt;/Grid&gt; ** znaczniki pliku XAML i wklej następujący kod między dwa tagi:

        <Grid.RowDefinitions>
            <RowDefinition Height="20"/>    <!-- spacer -->
            <RowDefinition Height="50"/>    <!-- media controls -->
            <RowDefinition Height="100*"/>  <!-- media element -->
            <RowDefinition Height="80*"/>   <!-- media stream and track selection -->
            <RowDefinition Height="50"/>    <!-- status bar -->
        </Grid.RowDefinitions>
        
        <StackPanel Name="spMediaControl" Grid.Row="1" Orientation="Horizontal">
            <TextBlock x:Name="tbSource" Text="Source :  " FontSize="16" FontWeight="Bold" VerticalAlignment="Center" />
            <TextBox x:Name="txtMediaSource" Text="http://ecn.channel9.msdn.com/o9/content/smf/smoothcontent/elephantsdream/Elephants_Dream_1024-h264-st-aac.ism/manifest" FontSize="10" Width="700" Margin="0,4,0,10" />
            <Button x:Name="btnSetSource" Content="Set Source" Width="111" Height="43" Click="btnSetSource_Click"/>
            <Button x:Name="btnPlay" Content="Play" Width="111" Height="43" Click="btnPlay_Click"/>
            <Button x:Name="btnPause" Content="Pause"  Width="111" Height="43" Click="btnPause_Click"/>
            <Button x:Name="btnStop" Content="Stop"  Width="111" Height="43" Click="btnStop_Click"/>
            <CheckBox x:Name="chkAutoPlay" Content="Auto Play" Height="55" Width="Auto" IsChecked="{Binding AutoPlay, ElementName=mediaElement, Mode=TwoWay}"/>
            <CheckBox x:Name="chkMute" Content="Mute" Height="55" Width="67" IsChecked="{Binding IsMuted, ElementName=mediaElement, Mode=TwoWay}"/>
        </StackPanel>

        <StackPanel Name="spMediaElement" Grid.Row="2" Height="435" Width="1072"
                    HorizontalAlignment="Center" VerticalAlignment="Center">
            <MediaElement x:Name="mediaElement" Height="356" Width="924" MinHeight="225"
                          HorizontalAlignment="Center" VerticalAlignment="Center" 
                          AudioCategory="BackgroundCapableMedia" />
            <StackPanel Orientation="Horizontal">
                <Slider x:Name="sliderProgress" Width="924" Height="44"
                        HorizontalAlignment="Center" VerticalAlignment="Center"
                        PointerPressed="sliderProgress_PointerPressed"/>
                <Slider x:Name="sliderVolume" 
                        HorizontalAlignment="Right" VerticalAlignment="Center" Orientation="Vertical" 
                        Height="79" Width="148" Minimum="0" Maximum="1" StepFrequency="0.1" 
                        Value="{Binding Volume, ElementName=mediaElement, Mode=TwoWay}" 
                        ToolTipService.ToolTip="{Binding Value, RelativeSource={RelativeSource Mode=Self}}"/>
            </StackPanel>
        </StackPanel>

        <StackPanel Name="spStatus" Grid.Row="4" Orientation="Horizontal">
            <TextBlock x:Name="tbStatus" Text="Status :  " 
               FontSize="16" FontWeight="Bold" VerticalAlignment="Center" HorizontalAlignment="Center" />
            <TextBox x:Name="txtStatus" FontSize="10" Width="700" VerticalAlignment="Center"/>
        </StackPanel>

    Formant MediaElement służy do odtwarzania multimediów. Suwak o nazwie sliderProgress będzie używane w następnej lekcji do sterowania postępu multimediów.

3.  Naciśnij **Klawisze CTRL + S** , aby zapisać plik.

Formant MediaElement nie obsługuje wygładzonymi strumieniowego przesyłania zawartości z gotowych do. Aby włączyć obsługę wygładzonymi Streaming, musisz zarejestrować obsługi strumienia bajtów wygładzonymi strumieniowego przesyłania przez rozszerzenia nazwy pliku i typ MIME.  Aby zarejestrować, używana jest metoda MediaExtensionManager.RegisterByteStremHandler nazw Windows.Media.

W tym pliku XAML niektórych obsługi zdarzeń są skojarzone z kontrolek.  Należy zdefiniować te obsługi zdarzeń.

**Aby zmodyfikować kodu źródłowego pliku**

1.  Korzystając z Eksploratora rozwiązanie kliknij prawym przyciskiem myszy **MainPage.xaml**, a następnie kliknij **Wyświetl kod**.
2.  W górnej części pliku, Dodaj następujący za pomocą instrukcji:

        using Windows.Media;

3.  Na początku klasy **MainPage** , Dodaj następujący element danych:

        private MediaExtensionManager extensions = new MediaExtensionManager();

4.  Na końcu konstruktora **MainPage** Dodaj następujące dwa wiersze:

        extensions.RegisterByteStreamHandler("Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", ".ism", "text/xml");
        extensions.RegisterByteStreamHandler("Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", ".ism", "application/vnd.ms-sstr+xml");
        
5.  Na końcu klasy **MainPage** , poza następujący kod:

        #region UI Button Click Events
        private void btnPlay_Click(object sender, RoutedEventArgs e)
        {
            mediaElement.Play();
            txtStatus.Text = "MediaElement is playing ...";
        }
        
        private void btnPause_Click(object sender, RoutedEventArgs e)
        {
            mediaElement.Pause();
            txtStatus.Text = "MediaElement is paused";
        }
        
        private void btnSetSource_Click(object sender, RoutedEventArgs e)
        {
            sliderProgress.Value = 0;
            mediaElement.Source = new Uri(txtMediaSource.Text);
        
            if (chkAutoPlay.IsChecked == true)
            {
                txtStatus.Text = "MediaElement is playing ...";
            }
            else
            {
                txtStatus.Text = "Click the Play button to play the media source.";
            }
        }
        
        private void btnStop_Click(object sender, RoutedEventArgs e)
        {
            mediaElement.Stop();
            txtStatus.Text = "MediaElement is stopped";
        }
        
        private void sliderProgress_PointerPressed(object sender, PointerRoutedEventArgs e)
        {
            txtStatus.Text = "Seek to position " + sliderProgress.Value;
            mediaElement.Position = new TimeSpan(0, 0, (int)(sliderProgress.Value));
        }
        #endregion

    Program obsługi zdarzeń sliderProgress_PointerPressed zdefiniowany w tym miejscu.  Istnieje więcej działa robić, aby działała, obejmujący w następnej lekcji tego samouczka.
6.  Naciśnij **Klawisze CTRL + S** , aby zapisać plik.

Gotowe kodu źródłowego pliku będzie wyglądać następująco:

![Codeview w aplikacji programu Visual Studio z wygładzonymi Streaming ze Sklepu Windows][CodeViewPic]

**Gromadzenie i przetestować aplikację**

1.  Z menu **Tworzenie** kliknij pozycję **Menedżer konfiguracji**.
2.  Zmienianie **platformy aktywne rozwiązanie** zgodnie z platformy rozwoju.
3.  Naciśnij klawisz **F6** , aby skompilować projektu. 
4.  Naciśnij klawisz **F5** , aby uruchomić aplikację.
5.  W górnej części aplikacji możesz użyć domyślnego wygładzonymi Streaming adres URL lub wprowadzić inny. 
6.  Kliknij przycisk **Ustaw źródło**. Ponieważ **Automatyczne odtwarzanie** jest domyślnie włączona, są automatyczne odtwarzanie multimediów.  Możesz sterować nośnik przy użyciu **odtwarzania**, przyciski **wstrzymywania** i **zatrzymywania** .  Możesz sterować przy użyciu pionowy suwak głośności multimediów.  Jednak suwak poziomy kontroli postępu multimedia nie jest w pełni zaimplementowana jeszcze. 

Ukończono lesson1.  W tej lekcji można użyć Formant MediaElement do odtwarzania wygładzonymi strumieniowych.  W następnej lekcji zostaną dodane suwak, aby określić postępu wygładzonymi strumieniowego przesyłania zawartości.


##<a name="lesson-2-add-a-slider-bar-to-control-the-media-progress"></a>Lekcja 2: Dodawanie suwaka, aby kontrolować postęp multimediów
W lekcji 1 utworzono aplikację ze Sklepu Windows z formantami MediaElement XAML odtwarzanie wygładzonymi multimediów strumieniowych.  Niektóre funkcje podstawowe multimediów, takich jak rozpoczęcie, Zatrzymaj i Wstrzymaj go samodzielnie.  W tej lekcji dodasz pasek suwaka do aplikacji.

W tym samouczku użyjemy czasomierza zaktualizować położenie suwaka na podstawie bieżącego położenia formantu MediaElement.  Suwak rozpoczęcia i zakończenia również czas należy zaktualizować w przypadku zawartości na żywo.  Mogą być lepiej obsłużone w przypadku aktualizacji adaptacyjne źródła.

Źródła multimediów są obiekty, które generują dane multimediów.  Źródło rozpoznawania nazw ma adres URL lub bajt strumienia i tworzy źródło odpowiedniego nośnika dla tej zawartości.  Źródło rozpoznawania nazw jest standardowym sposobem aplikacji do tworzenia źródeł multimediów. 

W tej lekcji zawiera następujące procedury:

1.  Zarejestrować obsługi wygładzonymi strumieniowego przesyłania 
2.  Dodawanie obsługi zdarzeń poziomu Menedżera adaptacyjne źródła
3.  Dodawanie obsługi zdarzeń na poziomie źródła adaptacyjne
4.  Dodawanie obsługi zdarzeń MediaElement
5.  Dodawanie suwak powiązanych kodu kreskowego
6.  Można skompilować i przetestować aplikację

**Zarejestrować obsługi strumienia bajtów Streaming wygładzonymi i przekazywanie propertyset**

1.  Korzystając z Eksploratora rozwiązanie kliknij prawym przyciskiem myszy **MainPage.xaml**, a następnie kliknij **Wyświetl kod**.
2.  Na początku pliku, Dodaj następujący za pomocą instrukcji:

        using Microsoft.Media.AdaptiveStreaming;

3.  Na początku klasy MainPage, Dodaj następujące elementy danych:

        private Windows.Foundation.Collections.PropertySet propertySet = new Windows.Foundation.Collections.PropertySet();             
        private IAdaptiveSourceManager adaptiveSourceManager;
    
4.  W Konstruktorze **MainPage** Dodaj następujący kod po **to. Inicjowanie Components();** wiersz i wierszy kodu rejestracji napisanym w poprzedniej lekcji:
    
        // Gets the default instance of AdaptiveSourceManager which manages Smooth 
        //Streaming media sources.
        adaptiveSourceManager = AdaptiveSourceManager.GetDefault();
        // Sets property key value to AdaptiveSourceManager default instance.
        // {A5CE1DE8-1D00-427B-ACEF-FB9A3C93DE2D}" must be hardcoded.
        propertySet["{A5CE1DE8-1D00-427B-ACEF-FB9A3C93DE2D}"] = adaptiveSourceManager;
    
5.  W Konstruktorze **MainPage** modyfikowanie dwie metody RegisterByteStreamHandler w celu dodania wymienionych parametrów:

        // Registers Smooth Streaming byte-stream handler for ".ism" extension and, 
        // "text/xml" and "application/vnd.ms-ss" mime-types and pass the propertyset. 
        // http://*.ism/manifest URI resources will be resolved by Byte-stream handler.
        extensions.RegisterByteStreamHandler(
            "Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", 
            ".ism", 
            "text/xml", 
            propertySet );
        extensions.RegisterByteStreamHandler(
            "Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", 
            ".ism", 
            "application/vnd.ms-sstr+xml", 
        propertySet);

6.  Naciśnij **Klawisze CTRL + S** , aby zapisać plik.

**Aby dodać źródło adaptacyjne Menedżer poziom obsługi zdarzeń**

1.  Korzystając z Eksploratora rozwiązanie kliknij prawym przyciskiem myszy **MainPage.xaml**, a następnie kliknij **Wyświetl kod**.
2.  Wewnątrz klasy **MainPage** Dodaj następujący element danych:

        private AdaptiveSource adaptiveSource = null;

3.  Na końcu klasy **MainPage** Dodaj następujących zdarzeń:
    
        #region Adaptive Source Manager Level Events
        private void mediaElement_AdaptiveSourceOpened(AdaptiveSource sender, AdaptiveSourceOpenedEventArgs args)
        {
            adaptiveSource = args.AdaptiveSource;
        }
        #endregion Adaptive Source Manager Level Events

4.  Na końcu konstruktora **MainPage** Dodaj następujący wiersz do subskrybowania zdarzenia open adaptacyjne źródła:
    
    adaptiveSourceManager.AdaptiveSourceOpenedEvent += nowe AdaptiveSourceOpenedEventHandler(mediaElement_AdaptiveSourceOpened);

5.  Naciśnij **Klawisze CTRL + S** , aby zapisać plik.

**Aby dodać obsługi zdarzeń na poziomie źródła adaptacyjne**

1.  Korzystając z Eksploratora rozwiązanie kliknij prawym przyciskiem myszy **MainPage.xaml**, a następnie kliknij **Wyświetl kod**.
2.  Wewnątrz klasy **MainPage** Dodaj następujący element danych:
    
        private AdaptiveSourceStatusUpdatedEventArgs adaptiveSourceStatusUpdate; 
        private Manifest manifestObject;
    
3.  Na końcu klasy **MainPage** Dodaj następujące procedury obsługi zdarzeń:

        #region Adaptive Source Level Events
        private void mediaElement_ManifestReady(AdaptiveSource sender, ManifestReadyEventArgs args)
        {
            adaptiveSource = args.AdaptiveSource;
            manifestObject = args.AdaptiveSource.Manifest;
        }
        
        private void mediaElement_AdaptiveSourceStatusUpdated(AdaptiveSource sender, AdaptiveSourceStatusUpdatedEventArgs args)
        {
            adaptiveSourceStatusUpdate = args;
        }
        
        private void mediaElement_AdaptiveSourceFailed(AdaptiveSource sender, AdaptiveSourceFailedEventArgs args)
        {
            txtStatus.Text = "Error: " + args.HttpResponse;
        }
        #endregion Adaptive Source Level Events

4.  Na koniec metoda **mediaElement AdaptiveSourceOpened** Dodaj następujący kod subskrybowanie zdarzeń:
    
        adaptiveSource.ManifestReadyEvent +=
                    mediaElement_ManifestReady;
        adaptiveSource.AdaptiveSourceStatusUpdatedEvent += 
            mediaElement_AdaptiveSourceStatusUpdated;
        adaptiveSource.AdaptiveSourceFailedEvent += 
            mediaElement_AdaptiveSourceFailed;
    
5.  Naciśnij **Klawisze CTRL + S** , aby zapisać plik.

Tego samego zdarzenia są dostępne na adaptacyjne Menedżer poziomie źródła także, które mogą być używane do obsługi funkcji wspólne dla wszystkich elementów multimedialnych w aplikacji. Każdy AdaptiveSource zawiera swoje własne zdarzenia i wszystkie zdarzenia AdaptiveSource będzie kaskadowe w obszarze AdaptiveSourceManager.

**Aby dodać Element multimediów obsługi zdarzeń**

1.  Korzystając z Eksploratora rozwiązanie kliknij prawym przyciskiem myszy **MainPage.xaml**, a następnie kliknij **Wyświetl kod**.
2.  Na końcu klasy **MainPage** Dodaj następujące procedury obsługi zdarzeń:
    
        #region Media Element Event Handlers 
        private void MediaOpened(object sender, RoutedEventArgs e)
        {
            txtStatus.Text = "MediaElement opened";
        }
        
        private void MediaFailed(object sender, ExceptionRoutedEventArgs e)
        {
            txtStatus.Text= "MediaElement failed: " + e.ErrorMessage;
        }
        
        private void MediaEnded(object sender, RoutedEventArgs e)
        {
            txtStatus.Text ="MediaElement ended.";
        }
        #endregion Media Element Event Handlers

3.  Na końcu konstruktora **MainPage** Dodaj następujący kod do dolnego do zdarzenia:
    
        mediaElement.MediaOpened += MediaOpened;
        mediaElement.MediaEnded += MediaEnded;
        mediaElement.MediaFailed += MediaFailed;

4.  Naciśnij **Klawisze CTRL + S** , aby zapisać plik.

**Aby dodać suwaka związane z kodu**

1.  Korzystając z Eksploratora rozwiązanie kliknij prawym przyciskiem myszy **MainPage.xaml**, a następnie kliknij **Wyświetl kod**.
2.  Na początku pliku, Dodaj następujący za pomocą instrukcji:
    
        using Windows.UI.Core;

3.  Wewnątrz klasy **MainPage** Dodaj następujące elementy danych:
    
        public static CoreDispatcher _dispatcher;
        private DispatcherTimer sliderPositionUpdateDispatcher;
    
4.  Na końcu konstruktora **MainPage** Dodaj następujący kod:

        _dispatcher = Window.Current.Dispatcher;
        PointerEventHandler pointerpressedhandler = new PointerEventHandler(sliderProgress_PointerPressed);
        sliderProgress.AddHandler(Control.PointerPressedEvent, pointerpressedhandler, true);    
    
5.  Na końcu klasy **MainPage** Dodaj następujący kod:
    
        #region sliderMediaPlayer
        private double SliderFrequency(TimeSpan timevalue)
        {
            long absvalue = 0;
            double stepfrequency = -1;
        
            if (manifestObject != null)
            {
                absvalue = manifestObject.Duration - (long)manifestObject.StartTime;
            }
            else
            {
                absvalue = mediaElement.NaturalDuration.TimeSpan.Ticks;
            }
        
            TimeSpan totalDVRDuration = new TimeSpan(absvalue);
        
            if (totalDVRDuration.TotalMinutes >= 10 && totalDVRDuration.TotalMinutes < 30)
            {
               stepfrequency = 10;
            }
            else if (totalDVRDuration.TotalMinutes >= 30 
                     && totalDVRDuration.TotalMinutes < 60)
            {
                stepfrequency = 30;
            }
            else if (totalDVRDuration.TotalHours >= 1)
            {
                stepfrequency = 60;
            }
        
            return stepfrequency;
        }
        
        void updateSliderPositionoNTicks(object sender, object e)
        {
            sliderProgress.Value = mediaElement.Position.TotalSeconds;
        }
        
        public void setupTimer()
        {
            sliderPositionUpdateDispatcher = new DispatcherTimer();
            sliderPositionUpdateDispatcher.Interval = new TimeSpan(0, 0, 0, 0, 300);
            startTimer();
        }

        public void startTimer()
        {
            sliderPositionUpdateDispatcher.Tick += updateSliderPositionoNTicks;
            sliderPositionUpdateDispatcher.Start();
        }
        
        // Slider start and end time must be updated in case of live content
        public async void setSliderStartTime(long startTime)
        {
            await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
                TimeSpan timespan = new TimeSpan(adaptiveSourceStatusUpdate.StartTime);
                double absvalue = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero);
                sliderProgress.Minimum = absvalue;
            });
        }
        
        // Slider start and end time must be updated in case of live content
        public async void setSliderEndTime(long startTime)
        {
            await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
                TimeSpan timespan = new TimeSpan(adaptiveSourceStatusUpdate.EndTime);
                double absvalue = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero);
                sliderProgress.Maximum = absvalue;
            });
        }
        #endregion sliderMediaPlayer

    **Uwaga:** CoreDispatcher służy do wprowadzania zmian wątku interfejsu użytkownika z wartością wątku interfejsu użytkownika. W przypadku gardło wątku wysyłającego Deweloper można wybrać wysyłającego, dostarczane przez jego zamierza zaktualizować element interfejsu użytkownika.  Na przykład:
    
        await sliderProgress.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => { TimeSpan 
          timespan = new TimeSpan(adaptiveSourceStatusUpdate.EndTime); 
        double absvalue  = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero); 
          sliderProgress.Maximum = absvalue; }); 
        

6.  Na koniec metoda **mediaElement_AdaptiveSourceStatusUpdated** Dodaj następujący kod:
    
        setSliderStartTime(args.StartTime);
        setSliderEndTime(args.EndTime);

7.  Na koniec metoda **MediaOpened** Dodaj następujący kod:
    
    sliderProgress.StepFrequency = SliderFrequency(mediaElement.NaturalDuration.TimeSpan); sliderProgress.Width = mediaElement.Width; setupTimer();

8.  Naciśnij **Klawisze CTRL + S** , aby zapisać plik.

**Gromadzenie i przetestować aplikację**

1. Naciśnij klawisz **F6** , aby skompilować projektu. 
2.  Naciśnij klawisz **F5** , aby uruchomić aplikację.
3.  W górnej części aplikacji możesz użyć domyślnego wygładzonymi Streaming adres URL lub wprowadzić inny. 
4.  Kliknij przycisk **Ustaw źródło**. 
5.  Testowanie suwaka.

Ukończono lekcji 2.  W tej lekcji suwak jest dodawana do aplikacji. 

##<a name="lesson-3-select-smooth-streaming-streams"></a>Lekcja 3: Wybierz wygładzonymi Streaming strumieni
Gładkie Streaming jest w stanie do strumieniowego przesyłania zawartości z wielu ścieżek audio języka, które można wybrać przez przeglądarki.  W tej lekcji umożliwi odbiorcom wybierz strumieni. W tej lekcji zawiera następujące procedury:

1. Modyfikowanie pliku XAML
2. Modyfikowanie pliku behand kodu
3. Można skompilować i przetestować aplikację


**Aby zmodyfikować plik XAML**

1. Korzystając z Eksploratora rozwiązanie kliknij prawym przyciskiem myszy **MainPage.xaml**, a następnie kliknij **Projektant widoków**.
2. Znajdź &lt;Grid.RowDefinitions&gt;i zmodyfikowanie RowDefinitions, tak aby były wygląda tak:

        <Grid.RowDefinitions>            
            <RowDefinition Height="20"/>
            <RowDefinition Height="50"/>
            <RowDefinition Height="100"/>
            <RowDefinition Height="80"/>
            <RowDefinition Height="50"/>
        </Grid.RowDefinitions>

3. Wewnątrz &lt;siatki&gt;&lt;/Grid&gt; znaczniki, Dodaj następujący kod, aby zdefiniować formant pola listy, aby użytkownicy będą widzieć na liście dostępne strumieni i wybierz pozycję strumienie:

        <Grid Name="gridStreamAndBitrateSelection" Grid.Row="3">
            <Grid.RowDefinitions>
                <RowDefinition Height="300"/>
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="250*"/>
                <ColumnDefinition Width="250*"/>
            </Grid.ColumnDefinitions>
            <StackPanel Name="spStreamSelection" Grid.Row="1" Grid.Column="0">
                <StackPanel Orientation="Horizontal">
                    <TextBlock Name="tbAvailableStreams" Text="Available Streams:" FontSize="16" VerticalAlignment="Center"></TextBlock>
                    <Button Name="btnChangeStreams" Content="Submit" Click="btnChangeStream_Click"/>
                </StackPanel>
                <ListBox x:Name="lbAvailableStreams" Height="200" Width="200" Background="Transparent" HorizontalAlignment="Left" 
                    ScrollViewer.VerticalScrollMode="Enabled" ScrollViewer.VerticalScrollBarVisibility="Visible">
                    <ListBox.ItemTemplate>
                        <DataTemplate>
                            <CheckBox Content="{Binding Name}" IsChecked="{Binding isChecked, Mode=TwoWay}" />
                        </DataTemplate>
                    </ListBox.ItemTemplate>
                </ListBox>
            </StackPanel>
        </Grid>

4. Naciśnij **Klawisze CTRL + S** , aby zapisać zmiany.


**Aby zmodyfikować kodu źródłowego pliku**

1. Korzystając z Eksploratora rozwiązanie kliknij prawym przyciskiem myszy **MainPage.xaml**, a następnie kliknij **Wyświetl kod**.
2. W obszarze nazw SSPlayer, dodać nową klasę: klasy #region strumienia
    
        public class Stream
        {
            private IManifestStream stream;
            public bool isCheckedValue;
            public string name;
    
            public string Name
            {
                get { return name; }
                set { name = value; }
            }
    
            public IManifestStream ManifestStream
            {
                get { return stream; }
                set { stream = value; }
            }
    
            public bool isChecked
            {
                get { return isCheckedValue; }
                set
                {
                    // mMke the video stream always checked.
                    if (stream.Type == MediaStreamType.Video)
                    {
                        isCheckedValue = true;
                    }
                    else
                    {
                        isCheckedValue = value;
                    }
                }
            }
    
            public Stream(IManifestStream streamIn)
            {
                stream = streamIn;
                name = stream.Name;
            }
        }
        #endregion class Stream

3. Na początku klasy MainPage, Dodaj następujące definicje zmiennych:

        private List<Stream> availableStreams;
        private List<Stream> availableAudioStreams;
        private List<Stream> availableTextStreams;
        private List<Stream> availableVideoStreams;

4. Wewnątrz klasy MainPage dodać region następujące czynności:

        #region stream selection
        ///<summary>
        ///Functionality to select streams from IManifestStream available streams
        /// </summary>
        
        // This function is called from the mediaElement_ManifestReady event handler 
        // to retrieve the streams and populate them to the local data members.
        public void getStreams(Manifest manifestObject)
        {
            availableStreams = new List<Stream>();
            availableVideoStreams = new List<Stream>();
            availableAudioStreams = new List<Stream>();
            availableTextStreams = new List<Stream>();
        
            try
            {
                for (int i = 0; i<manifestObject.AvailableStreams.Count; i++)
                {
                    Stream newStream = new Stream(manifestObject.AvailableStreams[i]);
                    newStream.isChecked = false;
        
                    //populate the stream lists based on the types
                    availableStreams.Add(newStream);
        
                    switch (newStream.ManifestStream.Type)
                    {
                        case MediaStreamType.Video:
                            availableVideoStreams.Add(newStream);
                            break;
                        case MediaStreamType.Audio:
                            availableAudioStreams.Add(newStream);
                            break;
                        case MediaStreamType.Text:
                            availableTextStreams.Add(newStream);
                            break;
                    }
        
                    // Select the default selected streams from the manifest.
                    for (int j = 0; j<manifestObject.SelectedStreams.Count; j++)
                    {
                        string selectedStreamName = manifestObject.SelectedStreams[j].Name;
                        if (selectedStreamName.Equals(newStream.Name))
                        {
                            newStream.isChecked = true;
                            break;
                        }
                    }
                }
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }
        }
        
        // This function set the list box ItemSource
        private async void refreshAvailableStreamsListBoxItemSource()
        {
            try
            {
                //update the stream check box list on the UI
                await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, ()
                    => { lbAvailableStreams.ItemsSource = availableStreams; });
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }
        }
        
        // This function creates a selected streams list
        private void createSelectedStreamsList(List<IManifestStream> selectedStreams)
        {
            bool isOneVideoSelected = false;
            bool isOneAudioSelected = false;
        
            // Only one video stream can be selected
            for (int j = 0; j<availableVideoStreams.Count; j++)
            {
                if (availableVideoStreams[j].isChecked && (!isOneVideoSelected))
                {
                    selectedStreams.Add(availableVideoStreams[j].ManifestStream);
                    isOneVideoSelected = true;
                }
            }
        
            // Select the frist video stream from the list if no video stream is selected
            if (!isOneVideoSelected)
            {
                availableVideoStreams[0].isChecked = true;
                selectedStreams.Add(availableVideoStreams[0].ManifestStream);
            }
        
            // Only one audio stream can be selected
            for (int j = 0; j<availableAudioStreams.Count; j++)
            {
                if (availableAudioStreams[j].isChecked && (!isOneAudioSelected))
                {
                    selectedStreams.Add(availableAudioStreams[j].ManifestStream);
                    isOneAudioSelected = true;
                    txtStatus.Text = "The audio stream is changed to " + availableAudioStreams[j].ManifestStream.Name;
                }
            }
        
            // Select the frist audio stream from the list if no audio steam is selected.
            if (!isOneAudioSelected)
            {
                availableAudioStreams[0].isChecked = true;
                selectedStreams.Add(availableAudioStreams[0].ManifestStream);
            }
        
            // Multiple text streams are supported.
            for (int j = 0; j < availableTextStreams.Count; j++)
            {
                if (availableTextStreams[j].isChecked)
                {
                    selectedStreams.Add(availableTextStreams[j].ManifestStream);
                }
            }
        }
        
        // Change streams on a smooth streaming presentation with multiple video streams.
        private async void changeStreams(List<IManifestStream> selectStreams)
        {
            try
            {
                IReadOnlyList<IStreamChangedResult> returnArgs =
                    await manifestObject.SelectStreamsAsync(selectStreams);
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }
        }
        #endregion stream selection

5. Znajdź odpowiednią metodę mediaElement_ManifestReady, Dołącz poniższy kod na końcu funkcja:
    
        getStreams(manifestObject);
        refreshAvailableStreamsListBoxItemSource();

    Aby MediaElement manifest będzie gotowa, kod otrzymuje listę dostępnych strumieni i wypełnia pole listy interfejsu użytkownika z listy.

6. Wewnątrz klasy MainPage Znajdź interfejs przyciski kliknij pozycję region, zdarzeń, a następnie dodaj następujące definicji funkcji:

        private void btnChangeStream_Click(object sender, RoutedEventArgs e)
        {
            List<IManifestStream> selectedStreams = new List<IManifestStream>();

            // Create a list of the selected streams
            createSelectedStreamsList(selectedStreams);

            // Change streams on the presentation
            changeStreams(selectedStreams);
        }

**Gromadzenie i przetestować aplikację**

1. Naciśnij klawisz **F6** , aby skompilować projektu. 
2.  Naciśnij klawisz **F5** , aby uruchomić aplikację.
3.  W górnej części aplikacji możesz użyć domyślnego wygładzonymi Streaming adres URL lub wprowadzić inny. 
4.  Kliknij przycisk **Ustaw źródło**. 
5.  Domyślny język jest audio_eng. Spróbuj przełączać się między audio_eng i audio_es. Każdym, możesz wybrać nowego strumienia, należy kliknąć przycisk Prześlij.

Ukończono Lekcja 3.  W tej lekcji możesz dodać funkcję, aby wybrać strumieni.

##<a name="lesson-4-select-smooth-streaming-tracks"></a>Lekcja 4: Wybierz Streaming gładkie ścieżki
Gładkie strumieniowego przesyłania prezentacji może zawierać wiele plików wideo zakodowany z różne poziomy jakości (szybkości bitowej) i rozwiązania. W tej lekcji umożliwi użytkownikom na Wybieranie ścieżki. W tej lekcji zawiera następujące procedury:

1. Modyfikowanie pliku XAML
2. Modyfikowanie pliku behand kodu
3. Można skompilować i przetestować aplikację

**Aby zmodyfikować plik XAML**

1. Korzystając z Eksploratora rozwiązanie kliknij prawym przyciskiem myszy **MainPage.xaml**, a następnie kliknij **Projektant widoków**.
2. Znajdź &lt;siatki&gt; znakowanie z nazw **gridStreamAndBitrateSelection**, dołączająca poniższy kod na końcu tagu:

        <StackPanel Name="spBitRateSelection" Grid.Row="1" Grid.Column="1">
         <StackPanel Orientation="Horizontal">
             <TextBlock Name="tbBitRate" Text="Available Bitrates:" FontSize="16" VerticalAlignment="Center"/>
             <Button Name="btnChangeTracks" Content="Submit" Click="btnChangeTrack_Click" />
         </StackPanel>
         <ListBox x:Name="lbAvailableVideoTracks" Height="200" Width="200" Background="Transparent" HorizontalAlignment="Left" 
                  ScrollViewer.VerticalScrollMode="Enabled" ScrollViewer.VerticalScrollBarVisibility="Visible">
             <ListBox.ItemTemplate>
                 <DataTemplate>
                     <CheckBox Content="{Binding Bitrate}" IsChecked="{Binding isChecked, Mode=TwoWay}"/>
                 </DataTemplate>
             </ListBox.ItemTemplate>
         </ListBox>
        </StackPanel>

3. Naciśnij **Klawisze CTRL + S** , aby zapisać zmiany he


**Aby zmodyfikować kodu źródłowego pliku**

1. Korzystając z Eksploratora rozwiązanie kliknij prawym przyciskiem myszy **MainPage.xaml**, a następnie kliknij **Wyświetl kod**.
2. W obszarze nazw SSPlayer Dodaj nową klasę:
    
        #region class Track
        public class Track
        {
            private IManifestTrack trackInfo;
            public string _bitrate;
            public bool isCheckedValue;
    
            public IManifestTrack TrackInfo
            {
                get { return trackInfo; }
                set { trackInfo = value; }
            }
    
            public string Bitrate
            {
                get { return _bitrate; }
                set { _bitrate = value; }
            }
    
            public bool isChecked
            {
                get { return isCheckedValue; }
                set
                {
                    isCheckedValue = value;
                }
            }
    
            public Track(IManifestTrack trackInfoIn)
            {
                trackInfo = trackInfoIn;
                _bitrate = trackInfoIn.Bitrate.ToString();
            }
            //public Track() { }
        }
        #endregion class Track

3. Na początku klasy MainPage, Dodaj następujące definicje zmiennych:
    
        private List<Track> availableTracks;

4. Wewnątrz klasy MainPage dodać region następujące czynności:
    
        #region track selection
        /// <summary>
        /// Functionality to select video streams
        /// </summary>

        /// This Function gets the tracks for the selected video stream
        public void getTracks(Manifest manifestObject)
        {
            availableTracks = new List<Track>();

            IManifestStream videoStream = getVideoStream();
            IReadOnlyList<IManifestTrack> availableTracksLocal = videoStream.AvailableTracks;
            IReadOnlyList<IManifestTrack> selectedTracksLocal = videoStream.SelectedTracks;

            try
            {
                for (int i = 0; i < availableTracksLocal.Count; i++)
                {
                    Track thisTrack = new Track(availableTracksLocal[i]);
                    thisTrack.isChecked = true;

                    for (int j = 0; j < selectedTracksLocal.Count; j++)
                    {
                        string selectedTrackName = selectedTracksLocal[j].Bitrate.ToString();
                        if (selectedTrackName.Equals(thisTrack.Bitrate))
                        {
                            thisTrack.isChecked = true;
                            break;
                        }
                    }
                    availableTracks.Add(thisTrack);
                }
            }
            catch (Exception e)
            {
                txtStatus.Text = e.Message;
            }
        }

        // This function gets the video stream that is playing
        private IManifestStream getVideoStream()
        {
            IManifestStream videoStream = null;
            for (int i = 0; i < manifestObject.SelectedStreams.Count; i++)
            {
                if (manifestObject.SelectedStreams[i].Type == MediaStreamType.Video)
                {
                    videoStream = manifestObject.SelectedStreams[i];
                    break;
                }
            }
            return videoStream;
        }

        // This function set the UI list box control ItemSource
        private async void refreshAvailableTracksListBoxItemSource()
        {
            try
            {
                // Update the track check box list on the UI 
                await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, ()
                    => { lbAvailableVideoTracks.ItemsSource = availableTracks; });
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }        
        }

        // This function creates a list of the selected tracks.
        private void createSelectedTracksList(List<IManifestTrack> selectedTracks)
        {
            // Create a list of selected tracks
            for (int j = 0; j < availableTracks.Count; j++)
            {
                if (availableTracks[j].isCheckedValue == true)
                {
                    selectedTracks.Add(availableTracks[j].TrackInfo);
                }
            }
        }

        // This function selects the tracks based on user selection 
        private void changeTracks(List<IManifestTrack> selectedTracks)
        {
            IManifestStream videoStream = getVideoStream();
            try
            {
                videoStream.SelectTracks(selectedTracks);
            }
            catch (Exception ex)
            {
                txtStatus.Text = ex.Message;
            }
        }
        #endregion track selection

5. Znajdź odpowiednią metodę mediaElement_ManifestReady, Dołącz poniższy kod na końcu funkcja:

        getTracks(manifestObject);
        refreshAvailableTracksListBoxItemSource();

6. Wewnątrz klasy MainPage Znajdź interfejs przyciski kliknij pozycję region, zdarzeń, a następnie dodaj następujące definicji funkcji:

        private void btnChangeStream_Click(object sender, RoutedEventArgs e)
        {
            List<IManifestStream> selectedStreams = new List<IManifestStream>();

            // Create a list of the selected streams
            createSelectedStreamsList(selectedStreams);

            // Change streams on the presentation
            changeStreams(selectedStreams);
        }

**Gromadzenie i przetestować aplikację**

1. Naciśnij klawisz **F6** , aby skompilować projektu. 
2.  Naciśnij klawisz **F5** , aby uruchomić aplikację.
3.  W górnej części aplikacji możesz użyć domyślnego wygładzonymi Streaming adres URL lub wprowadzić inny. 
4.  Kliknij przycisk **Ustaw źródło**. 
5.  Domyślnie wszystkie ścieżki strumienia wideo są zaznaczone. Aby poeksperymentować zmian stawek bitowej, można wybierz najniższe transmisji dostępne, a następnie wybierz najwyższe transmisji dostępne. Po każdej zmianie, należy kliknij przycisk Prześlij.  Zostaną wyświetlone zmiany jakość wideo.

Ukończono lekcji 4.  W tej lekcji możesz dodać funkcję, aby wybrać ścieżki.


##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="other-resources"></a>Inne zasoby:
- [Jak utworzyć aplikację wygładzonymi JavaScript 8 Windows Streaming z zaawansowanych funkcji](http://blogs.iis.net/cenkd/archive/2012/08/10/how-to-build-a-smooth-streaming-windows-8-javascript-application-with-advanced-features.aspx)
- [Gładkie przesyłanie strumieniowe omówienie kwestii technicznych](http://www.iis.net/learn/media/on-demand-smooth-streaming/smooth-streaming-technical-overview)

[PlayerApplication]: ./media/media-services-build-smooth-streaming-apps/SSClientWin8-1.png
[CodeViewPic]: ./media/media-services-build-smooth-streaming-apps/SSClientWin8-2.png
 
