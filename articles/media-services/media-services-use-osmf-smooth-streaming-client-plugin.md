<properties 
    pageTitle="Przesyłanie strumieniowe wtyczki wygładzonymi Framework multimediów Otwórz źródło" 
    description="Dowiedz się, jak za pomocą wtyczki Azure usług wygładzonymi strumieniowego przesyłania dla programu Adobe Otwórz źródło multimediów Framework." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"
    ms.author="juliako"/>


# <a name="how-to-use-the-microsoft-smooth-streaming-plugin-for-the-adobe-open-source-media-framework"></a>Jak używać gładkie Microsoft Streaming dodatek dla programu Adobe Otwórz źródło multimediów Framework

##<a name="overview"></a>Omówienie


Dodatek Microsoft wygładzonymi Streaming Otwórz źródło multimediów Framework 2.0 (RR dla OSMF) rozszerza możliwości domyślne OSMF i dodaje Microsoft wygładzonymi Streaming odtwarzanie zawartości do nowych i istniejących odtwarzacze OSMF. Dodatek dodaje również wygładzonymi Streaming funkcje odtwarzania do odtwarzania multimediów światła (SMP).

SS dla OSMF zawiera dwie wersje wtyczki:

- Statyczne wygładzonymi Streaming wtyczki dla OSMF (SWC)

- Dynamiczne wygładzonymi Streaming wtyczki dla OSMF (SWF)

Ten dokument przyjęto założenie, że czytnik zawiera ogólne znają OSMF i OSMF wtyczki. Aby uzyskać więcej informacji na temat OSMF można znaleźć w dokumentacji [oficjalna witryna OSMF](http://osmf.org/).

###<a name="smooth-streaming-plugin-for-osmf-20"></a>Gładkie wtyczki Streaming OSMF 2.0

Dodatek obsługuje ładowania i odtwarzania na żądanie wygładzonymi strumieniowych następujące funkcje:

- Odtwarzanie z wygładzonymi Streaming na żądanie (Odtwórz, Wstrzymaj, wyszukiwania, Zatrzymaj)
- Odtwarzanie Live wygładzonymi Streaming (Odtwórz)
- Live funkcje DVR (Wstrzymaj, wyszukiwania, DVR odtwarzanie Go Live)
- Obsługa kodery-dekodery wideo — H.264
- Obsługa kodery-dekodery Audio - AAC
- Przełączania z wbudowanych interfejsy API OSMF wielu języków audio
- Maksymalna liczba odtwarzanie jakości zaznaczenia z wbudowanych interfejsy API OSMF
- Podpisy z OSMF dodatek podpisy kodowane boczną
- Adobe&reg; Flash&reg; Player 11.4 lub nowszy.
- Ta wersja obsługuje tylko OSMF 2.0.

## <a name="supported-features-and-known-issues"></a>Obsługiwane funkcje i znane problemy

Aby uzyskać pełną listę obsługiwanych funkcji nieobsługiwane funkcje i znane problemy dotyczą [tego dokumentu](http://download.microsoft.com/download/3/1/B/31B63D97-574E-4A8D-BF8D-170744181724/Smooth_Streaming_Plugin_for_OSMF.pdf).


## <a name="loading-the-plugin"></a>Ładowanie dodatku
Wtyczki OSMF można załadować statycznie (w czasie kompilacji) lub dynamicznie (w czasie wykonywania). Dodatek wygładzonymi Streaming do pobrania OSMF zawiera zarówno dynamicznych i statycznych wersje.

- Ładowanie statyczny: Aby załadować statycznie, plik biblioteki statyczne (SWC) jest wymagane. Wtyczki statyczne są dodawane jako odwołanie do projektów i scalanie wewnątrz pliku wyjściowego w czasie kompilacji.

- Dynamiczne ładowanie: Aby załadować dynamicznie, wstępnie skompilowana plik (SWF) jest wymagane. Dynamiczne wtyczki są ładowane w czasie wykonywania i nieuwzględniona w wyniku projektu. (Wyjście skompilowane) Dynamiczne wtyczki można załadować za pomocą protokołów HTTP i plik.

Aby uzyskać więcej informacji na statycznego i dynamicznego ładowania zawiera oficjalnym [OSMF dodatek strony](http://osmf.org/dev/osmf/OtherPDFs/osmf_plugin_dev_guide.pdf).

###<a name="ss-for-osmf-static-loading"></a>SS dla OSMF ładowania statyczne
Wstawkę kodu poniżej pokazano, jak załadować dodatek SS OSMF statycznie i odtwarzanie wideo podstawowe za pomocą klasy OSMF MediaFactory. Przed tym SS OSMF kodu, upewnij się, że odwołanie projektu zawiera dodatek statyczne "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swc".

```
package 
{
    
    import com.microsoft.azure.media.AdaptiveStreamingPluginInfo;
    
    import flash.display.*;
    import org.osmf.media.*;
    import org.osmf.containers.MediaContainer;
    import org.osmf.events.MediaErrorEvent;
    import org.osmf.events.MediaFactoryEvent;
    import org.osmf.events.MediaPlayerStateChangeEvent;
    import org.osmf.layout.*;
    
    
    
    [SWF(width="1024", height="768", backgroundColor='#405050', frameRate="25")]
    public class TestPlayer extends Sprite
    {        
        public var _container:MediaContainer;
        public var _mediaFactory:DefaultMediaFactory;
        private var _mediaPlayerSprite:MediaPlayerSprite;
        

        public function TestPlayer( )
        {
            stage.quality = StageQuality.HIGH;

            initMediaPlayer();

        }
    
        private function initMediaPlayer():void
        {
        
            // Create the container (sprite) for managing display and layout
            _mediaPlayerSprite = new MediaPlayerSprite();    
            _mediaPlayerSprite.addEventListener(MediaErrorEvent.MEDIA_ERROR, onPlayerFailed);
            _mediaPlayerSprite.addEventListener(MediaPlayerStateChangeEvent.MEDIA_PLAYER_STATE_CHANGE, onPlayerStateChange);
            _mediaPlayerSprite.scaleMode = ScaleMode.NONE;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;
            //Adds the container to the stage
            addChild(_mediaPlayerSprite);
            
            // Create a mediafactory instance
            _mediaFactory = new DefaultMediaFactory();
            
            // Add the listeners for PLUGIN_LOADING
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD,onPluginLoaded);
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD_ERROR, onPluginLoadFailed );
            
            // Load the plugin class 
            loadAdaptiveStreamingPlugin( );  
            
        }
        
        private function loadAdaptiveStreamingPlugin( ):void
        {
            var pluginResource:MediaResourceBase;
            
            pluginResource = new PluginInfoResource(new AdaptiveStreamingPluginInfo( )); 
            _mediaFactory.loadPlugin( pluginResource ); 
        }
        
        private function onPluginLoaded( event:MediaFactoryEvent ):void
        {
            // The plugin is loaded successfully.
            // Your web server needs to host a valid crossdomain.xml file to allow plugin to download Smooth Streaming files.
        loadMediaSource("http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest")
        
        }
        
        private function onPluginLoadFailed( event:MediaFactoryEvent ):void
        {
            // The plugin is failed to load ...
        }
        
        
        private function onPlayerStateChange(event:MediaPlayerStateChangeEvent) : void
        {
            var state:String;
            
            state =  event.state;
            
            switch (state)
            {
                case MediaPlayerState.LOADING: 
                    
                    // A new source is started to load.
                    
                    break;
                
                case  MediaPlayerState.READY :   
                    // Add code to deal with Player Ready when it is hit the first load after a source is loaded. 
                    
                    break;
                
                case MediaPlayerState.BUFFERING :
                    
                    break;
                
                case  MediaPlayerState.PAUSED :
                    break;      
                // other states ...          
            }
        }
        
        private function onPlayerFailed(event:MediaErrorEvent) : void
        {
            // Media Player is failed .           
        }
        
        private function loadMediaSource(sourceURL : String):void 
        {
            // Take an URL of SmoothStreamingSource's manifest and add it to the page.
            
            var resource:URLResource= new URLResource( sourceURL );
            
            var element:MediaElement = _mediaFactory.createMediaElement( resource );
            _mediaPlayerSprite.scaleMode = ScaleMode.LETTERBOX;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;
            
            // Add the media element
            _mediaPlayerSprite.media = element;
        }     
        
    }
}
```


###<a name="ss-for-osmf-dynamic-loading"></a>SS dla OSMF dynamiczne ładowanie

Wstawkę kodu poniżej pokazano, jak załadować dodatek SS OSMF dynamicznie i odtwarzanie basic wideo, za pomocą klasy OSMF MediaFactory. Przed tym SS OSMF kodu, skopiuj wtyczki dynamiczne "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf" do folderu projektu, jeśli chcesz załadować przy użyciu protokołu pliku, lub kopiowanie w obszarze Serwer ładowania HTTP sieci web. Istnieje nie trzeba uwzględnić "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swc" w odwołaniach do projektu.

 
{pakietu
    
    import flash.display.*;
    import org.osmf.media.*;
    import org.osmf.containers.MediaContainer;
    import org.osmf.events.MediaErrorEvent;
    import org.osmf.events.MediaFactoryEvent;
    import org.osmf.events.MediaPlayerStateChangeEvent;
    import org.osmf.layout.*;
    import flash.events.Event;
    import flash.system.Capabilities;

    
    //Sets the size of the SWF
    
    [SWF(width="1024", height="768", backgroundColor='#405050', frameRate="25")]
    public class TestPlayer extends Sprite
    {        
        public var _container:MediaContainer;
        public var _mediaFactory:DefaultMediaFactory;
        private var _mediaPlayerSprite:MediaPlayerSprite;
        
        
        public function TestPlayer( )
        {
            stage.quality = StageQuality.HIGH;
            initMediaPlayer();
        }
        
        private function initMediaPlayer():void
        {
            
            // Create the container (sprite) for managing display and layout
            _mediaPlayerSprite = new MediaPlayerSprite();    
            _mediaPlayerSprite.addEventListener(MediaErrorEvent.MEDIA_ERROR, onPlayerFailed);
            _mediaPlayerSprite.addEventListener(MediaPlayerStateChangeEvent.MEDIA_PLAYER_STATE_CHANGE, onPlayerStateChange);

            //Adds the container to the stage
            addChild(_mediaPlayerSprite);
            
            // Create a mediafactory instance
            _mediaFactory = new DefaultMediaFactory();
            
            // Add the listeners for PLUGIN_LOADING
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD,onPluginLoaded);
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD_ERROR, onPluginLoadFailed );
            
            // Load the plugin class 
            loadAdaptiveStreamingPlugin( );  
            
        }
        
        private function loadAdaptiveStreamingPlugin( ):void
        {
            var pluginResource:MediaResourceBase;
            var adaptiveStreamingPluginUrl:String;

            // Your dynamic plugin web server needs to host a valid crossdomain.xml file to allow loading plugins.

            adaptiveStreamingPluginUrl = "http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf";
            pluginResource = new URLResource(adaptiveStreamingPluginUrl);
            _mediaFactory.loadPlugin( pluginResource ); 

        }
        
        private function onPluginLoaded( event:MediaFactoryEvent ):void
        {
            // The plugin is loaded successfully.

            // Your web server needs to host a valid crossdomain.xml file to allow plugin to download Smooth Streaming files.

    loadMediaSource("http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest")
        }
        
        private function onPluginLoadFailed( event:MediaFactoryEvent ):void
        {
            // The plugin is failed to load ...
        }
        
        
        private function onPlayerStateChange(event:MediaPlayerStateChangeEvent) : void
        {
            var state:String;
            
            state =  event.state;
            
            switch (state)
            {
                case MediaPlayerState.LOADING: 
                    
                    // A new source is started to load.
                    
                    break;
                
                case  MediaPlayerState.READY :   
                    // Add code to deal with Player Ready when it is hit the first load after a source is loaded. 
                    
                    break;
                
                case MediaPlayerState.BUFFERING :
                    
                    break;
                
                case  MediaPlayerState.PAUSED :
                    break;      
                // other states ...          
            }
        }
        
        private function onPlayerFailed(event:MediaErrorEvent) : void
        {
            // Media Player is failed .           
        }
        
        private function loadMediaSource(sourceURL : String):void 
        {
            // Take an URL of SmoothStreamingSource's manifest and add it to the page.
            
            var resource:URLResource= new URLResource( sourceURL );
            
            var element:MediaElement = _mediaFactory.createMediaElement( resource );
            _mediaPlayerSprite.scaleMode = ScaleMode.LETTERBOX;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;
            // Add the media element
            _mediaPlayerSprite.media = element;
        }     
        
    }
}

##<a name="strobe-media--playback-with-the-ss-odmf-dynamic-plugin"></a>Światła odtwarzania multimediów z wtyczki dynamiczne SS ODMF

Gładkie Streaming dla wtyczki dynamiczne OSMF jest zgodny z [Światła odtwarzania multimediów (SMP)](http://osmf.org/strobe_mediaplayback.html). SS dla wtyczki OSMF umożliwia dodawanie wygładzonymi Streaming odtwarzanie zawartości do SMP. Aby to zrobić, skopiuj "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf" w obszarze serwer sieci web ładowania HTTP, wykonaj następujące czynności:

1.  Przeglądanie [strony Konfiguracja światła odtwarzania multimediów](http://osmf.org/dev/2.0gm/setup.html). 
2.  Ustaw src wygładzonymi przesyłanie strumieniowe źródła (np. http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest) 
3.  Wprowadź żądane zmiany w konfiguracji, a następnie kliknij przycisk Wyświetl podgląd i aktualizacji.
 
    **Uwaga** Serwer sieci web zawartości musi prawidłowych crossdomain.xml. 
4.  Skopiuj i Wklej kod prostą stronę HTML przy użyciu edytora tekstu Ulubione, takich jak w poniższym przykładzie:



        <html>
        <body>
        <object width="920" height="640"> 
        <param name="movie" value="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf"></param>
        <param name="flashvars" value="src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest &autoPlay=true"></param>
        <param name="allowFullScreen" value="true"></param>
        <param name="allowscriptaccess" value="always"></param>
        <param name="wmode" value="direct"></param>
        <embed src="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf" 
            type="application/x-shockwave-flash" 
            allowscriptaccess="always" 
            allowfullscreen="true" 
            wmode="direct" 
            width="920" 
            height="640" 
            flashvars=" src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest&autoPlay=true">
        </embed>
        </object>
        </body>
        </html>



5. Dodawanie wtyczki wygładzonymi OSMF Streaming do kodu osadzania i Zapisz.

        <html>
        <object width="920" height="640"> 
        <param name="movie" value="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf"></param>
        <param name="flashvars" value="src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest&autoPlay=true&plugin_AdaptiveStreamingPlugin=http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf&AdaptiveStreamingPlugin_retryLive=true&AdaptiveStreamingPlugin_retryInterval=10"></param>
        <param name="allowFullScreen" value="true"></param>
        <param name="allowscriptaccess" value="always"></param>
        <param name="wmode" value="direct"></param>
        <embed src="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf" 
            type="application/x-shockwave-flash" 
            allowscriptaccess="always" 
            allowfullscreen="true" 
            wmode="direct" 
            width="920" 
            height="640" 
            flashvars="src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest&autoPlay=true&plugin_AdaptiveStreamingPlugin=http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf&AdaptiveStreamingPlugin_retryLive=true&AdaptiveStreamingPlugin_retryInterval=10">
        </embed>
        </object>
        </html>


6.  Zapisywanie strony HTML i publikowanie na serwerze sieci web. Przejdź do strony sieci web opublikowanych za pomocą programu Flash ulubione&reg; odtwarzacza włączone przeglądarki internetowej (Internet Explorer, Chrome, Firefox, tak dalej).
7.  Korzystać z wygładzonymi strumieniowych wewnątrz Adobe&reg; Flash&reg; odtwarzacza.

Aby uzyskać więcej informacji na ogólnego rozwoju OSMF Zobacz oficjalnym [OSMF rozwoju strony](http://osmf.org/resources.html).

##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



##<a name="see-also"></a>Zobacz też

[Adaptacyjne Streaming wtyczki aktualizacji OSMF firmy Microsoft](https://azure.microsoft.com/blog/2014/10/27/microsoft-adaptive-streaming-plugin-for-osmf-update/) 
