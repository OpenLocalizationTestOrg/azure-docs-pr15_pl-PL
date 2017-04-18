<properties 
    pageTitle="Osadzanie adaptacyjne przesyłanie strumieniowe wideo MPEG-kreska w wersji HTML5 aplikacji z DASH.js | Microsoft Azure" 
    description="W tym temacie przedstawiono sposób osadzanie MPEG-kreska adaptacyjne strumieniowego przesyłania wideo w aplikacji HTML5 z DASH.js." 
    authors="Juliako" 
    manager="erikre" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="juliako"/>


#<a name="embedding-a-mpeg-dash-adaptive-streaming-video-in-an-html5-application-with-dashjs"></a>Osadzanie adaptacyjne przesyłanie strumieniowe wideo MPEG-kreska w aplikacji HTML5 z DASH.js

##<a name="overview"></a>Omówienie

MPEG-kreska jest standardem ISO adaptacyjne strumieniowego przesyłania wideo zawartości, która oferuje znaczące korzyści dla osób, które chcesz przedstawić wysokiej jakości, adaptacyjne video streaming format wyjściowy. Z MPEG-kreska strumienia wideo zostanie automatycznie upuść do definicji dolnym, gdy sieć staje się wysyłanym. Zmniejsza to prawdopodobieństwo podglądu wideo "wstrzymania" są wyświetlane podczas odtwarzacza następnego kilka sekund odtwarzania (czyli buforowanie) — pliki do pobrania. Jak ogranicza przeciążeń sieci, odtwarzacza wideo z kolei może zwrócić ze strumieniem wyższą jakość. Możliwość dostosowania przepustowość wymagana powoduje również krótszy czas rozpoczęcia dla połączeń wideo. Oznacza to, że pierwszy kilka sekund mogą być odtwarzane w dolnej części jakości szybkie do pobierania i następnie została buforowana krok do wyższą jakość raz wystarczającej zawartości.

Dash.js jest Otwórz źródło MPEG-kreska odtwarzacza wideo napisana JavaScript. Jej celem jest zapewnienie odtwarzacz niezawodne, i platform, który może nastąpić swobodnie w aplikacji, które wymagają odtwarzania wideo. Umożliwia odtwarzanie plików MPEG-kreska w dowolnej przeglądarce, która obsługuje obecnie W3C multimediów źródła rozszerzenia (MSE) Chrome, Microsoft Edge i IE11 (inne przeglądarki wskazują ich przeznaczenie do obsługi MSE). Aby uzyskać więcej informacji na temat DASH.js js Zobacz repozytorium dash.js GitHub.


##<a name="creating-a-browser-based-streaming-video-player"></a>Tworzenie zgodny z przeglądarką przesyłanie strumieniowe odtwarzacza wideo

Aby utworzyć prosty strona sieci web, w której jest wyświetlana odtwarzacza wideo przewidywanego określa takie Odtwórz, Wstrzymaj, przewiń do tyłu itd., należy:

1. Tworzenie strony HTML
1. Dodawanie znacznika video
1. Dodawanie odtwarzacza dash.js
1. Inicjowanie odtwarzacza
1. Dodawanie stylu CSS
1. Wyświetlanie wyników w przeglądarce, która wykonuje MSE

Inicjowanie odtwarzacza trwa tylko kilku wierszy kodu JavaScript. Przy użyciu dash.js, naprawdę jest to proste osadzanie klipu wideo MPEG-kreska w aplikacjach przeglądarce.

##<a name="creating-the-html-page"></a>Tworzenie strony HTML

Pierwszym krokiem jest utworzenie standardowej strony HTML zawierający **wideo** elementu, Zapisz ten plik jako basicPlayer.html, jak przedstawiono w poniższym przykładzie:

    <!DOCTYPE html>
    <html>
      <head><title>Adaptive Streaming in HTML5</title></head>
      <body>
        <h1>Adaptive Streaming with HTML5</h1>
        <video id="videoplayer" controls></video>
      </body>
    </html>

##<a name="adding-the-dashjs-player"></a>Dodawanie odtwarzacza DASH.js

Aby dodać dash.js implementacji odwołania do aplikacji, musisz chwyć pliku dash.all.js z wersji 1.0 dash.js projektu. To powinny być zapisywane w folderze JavaScript aplikacji. Ten plik jest plikiem wygody, który pobiera ze sobą cały kod dash.js niezbędne do jednego pliku. Jeśli masz Przejrzyj wokół repozytorium dash.js będzie znaleźć pojedyncze pliki, testowanie kod i wiele innych, ale jeśli wszystkie chcesz zrobić jest używanie dash.js, a następnie plik dash.all.js to, co jest potrzebne.

Aby dodać odtwarzacza dash.js do aplikacji, Dodaj tag skrypt do sekcji głowy basicPlayer.html:

    <!-- DASH-AVC/265 reference implementation -->
    < script src="js/dash.all.js"></script>


Następnie należy utworzyć funkcję, aby zainicjować odtwarzacza podczas ładowania strony. Dodaj następujący skrypt po wierszu załadować dash.all.js:

    <script>
    // setup the video element and attach it to the Dash player
    function setupVideo() {
      var url = "http://wams.edgesuite.net/media/MPTExpressionData02/BigBuckBunny_1080p24_IYUV_2ch.ism/manifest(format=mpd-time-csf)";
      var context = new Dash.di.DashContext();
      var player = new MediaPlayer(context);
                      player.startup();
                      player.attachView(document.querySelector("#videoplayer"));
                      player.attachSource(url);
    }
    </script>

Ta funkcja najpierw tworzy DashContext. Służy do konfigurowania aplikacji w środowisku określone środowisko uruchomieniowe. Z technicznego punktu widzenia definiuje klasy, które framework uruchomienie zależności należy używać podczas tworzenia aplikacji. W większości przypadków program Dash.di.DashContext.

Następnie wystąpienia klasie podstawowej strukturze dash.js MediaPlayer. Ta klasa zawiera podstawowe metody potrzebne, takie jak odtwarzanie i zatrzymaj wskaźnik myszy, zarządza relacji z elementem wideo i zarządza także interpretacji pliku opis prezentacji multimediów (MPD), który zawiera opis klipu wideo ma być odtwarzany.

Funkcja startup() klasy MediaPlayer jest wywoływana aby upewnić się, że odtwarzacza jest gotowy do odtwarzania wideo. Między innymi ta funkcja zapewnia, że wszystkie potrzebne klasy (zdefiniowana w kontekście) zostały wprowadzone. Po oznaczającej odtwarzacza wideo element można dołączać do go przy użyciu funkcji attachView(). Dzięki temu MediaPlayer wprowadzić strumienia wideo do elementu, a także sterować odtwarzaniem stosownie do potrzeb.

Przekazać adres URL pliku MPD MediaPlayer tak, aby zna o klipie wideo, że jest planowane do odtwarzania. Funkcja setupVideo() właśnie utworzony muszą być wykonywane po pełni załadowaniu strony. To zrobić przy użyciu zdarzenia onload elementu treści. Zmienianie swojego <body> element, aby:

    <body onload="setupVideo()">

Na koniec Ustaw rozmiar wideo elementu przy użyciu arkuszy CSS. W środowisku przesyłanie strumieniowe adaptacyjne szczególnie ważne jest, ponieważ podczas odtwarzania przystosowuje do zmiany warunków sieciowych może ulec zmianie rozmiaru wideo odtwarzanych. Ten prosty pokaz po prostu obowiązujących wideo elementu, który ma być 80% okna przeglądarki dostępne, dodając następujące arkuszy CSS w sekcji głowy strony:
    
    <style>
    video {
      width: 80%;
      height: 80%;
    }
    </style>

##<a name="playing-a-video"></a>Odtwarzanie klipu wideo

Odtwarzanie klipu wideo, przeglądarce plik basicPlayback.html i kliknij przycisk Odtwórz na odtwarzacza wideo wyświetlane.


##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Zobacz też

[Można opracowywać aplikacje odtwarzacza wideo](media-services-develop-video-players.md)

[Repozytorium dash.js GitHub](https://github.com/Dash-Industry-Forum/dash.js) 
