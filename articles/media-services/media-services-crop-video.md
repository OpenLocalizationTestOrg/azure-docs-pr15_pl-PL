<properties
    pageTitle="Jak przyciąć wideo | Microsoft Azure"
    description="W tym artykule przedstawiono sposób przycinanie klipów wideo z Media Encoder standardowy."
    services="media-services"
    documentationCenter=""
    authors="anilmur"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/26/2016"  
    ms.author="anilmur;juliako;"/>

# <a name="crop-videos-with-media-encoder-standard"></a>Przycinanie klipów wideo z Media Encoder standardowy

Media Encoder standardowy (MES) służy do przycinania wprowadzone wideo. Kadrowanie jest to proces wyboru prostokątnym okna w ramce wideo i kodowanie tylko pikseli to okno. Poniższy diagram pomaga przedstawić procesu.

![Przycinanie klipu wideo](./media/media-services-crop-video/media-services-crop-video01.png)

Załóżmy, że masz jako dane wejściowe wideo, który został rozdzielczość 1920 x 1080 pikseli (współczynnik proporcji 16:9), ale ma czarne paski (pola słupka) w lewo i w prawo, tak aby tylko okno 4:3 lub 1440 x 1080 pikseli zawiera Uaktywnij wideo. Edytowanie się czarne paski za pomocą MES i kodowanie region 1440 x 1080.

Przycinanie w MES jest etapie przetwarzania wstępnego, więc przycinania parametrów w kodowania ustawienie dotyczą oryginalnego wideo wprowadzania. Kodowanie jest dalszym etapie i ustawienia szerokości i wysokości zastosować *wstępnie przetwarzane* wideo, a nie oryginalnego wideo. Podczas projektowania ustawienia należy wykonać następujące czynności: () wybierz parametrów przycinania, oparty na oryginalnego wideo wprowadzania, a (b) wybierz swojego kodowanie ustawienia oparte na przycięte wideo. Jeśli nie są zgodne z kodowanie ustawienia do przycięcia wideo, nie będzie dane wyjściowe, zgodnie z oczekiwaniami.

W [poniższych](media-services-advanced-encoding-with-mes.md#encoding_with_dotnet) sekcjach przedstawiono sposób tworzenia kodowania zadania z MES i określania niestandardowy wstępnie ustawione dla kodowania zadania. 

## <a name="creating-a-custom-preset"></a>Tworzenie animacji niestandardowej

W przykładzie przedstawionym na diagramie:

1. Oryginalne dane wejściowe są 1920 x 1080
1. Musi być przycięty do wyników 1440 x 1080, który jest wyśrodkowany w ramce wprowadzania danych
1. Oznacza to, że Przesunięcie X (1920 — 1440) / 2 = 240 i Y przesunięcie zero
1. Szerokość i wysokość prostokąta przycinania jest 1440 i 1080, w odpowiednio
1. Na scenie kodowanie Zadaj ma warzywa trzy warstwy, są odpowiednio rozdzielczości 1440 x 1080 960 x 720 i 480 x 360

###<a name="json-preset"></a>Ustawienie wstępne JSON


    {
      "Version": 1.0,
      "Sources": [
        {
          "Streams": [],
          "Filters": {
            "Crop": {
                "X": 240,
                "Y": 0,
                "Width": 1440,
                "Height": 1080
            }
          },
          "Pad": true
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 3400,
              "MaxBitrate": 3400,
              "BufferWindow": "00:00:05",
              "Width": 1440,
              "Height": 1080,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 2250,
              "MaxBitrate": 2250,
              "BufferWindow": "00:00:05",
              "Width": 960,
              "Height": 720,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1250,
              "MaxBitrate": 1250,
              "BufferWindow": "00:00:05",
              "Width": 480,
              "Height": 360,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
        },
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }


##<a name="restrictions-on-cropping"></a>Ograniczenia dotyczące przycinania

Funkcja przycinania ma być ręczny. Trzeba załadować wprowadzania wideo do odpowiedniego narzędzia edycji, które pozwala zaznaczyć ramki zainteresowania, umieść kursor w celu określenia przesunięcia prostokąta przycinania, w celu określenia kodowania ustawienie wstępne, który jest dostosowanych do tego określonego wideo itp. Ta funkcja jest nie ma na celu elementów takich jak: automatyczne wykrywanie i usuwanie obramowania czarny letterbox-pillarbox w danych wejściowych wideo.

Następujące ograniczenia dotyczą funkcji przycinania. Jeśli te nie są spełnione, kodowanie zadania mogą się nie powieść, lub wygenerować nieoczekiwany wynik.

1. Współrzędne i rozmiaru prostokąta przycinania mają do wprowadzania klipu wideo
1. Jak wcześniej wspomniano, szerokość i wysokość w ustawieniach kodowanie musi odpowiadać przycięte klipu wideo
1. Przycinanie dotyczy wideo przechwycone w orientacji poziomej (to znaczy nie dotyczy wideo zarejestrowane wraz z smartphone przechowywanych w pionie lub w trybie pionowa)
1. Sprawdza się najlepiej z obiektem wideo przechwycone za pomocą pikseli kwadratowych

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="next-step"></a>Następny krok
 
Zobacz usługi multimediów Azure ścieżki szkoleniowe dla użytkowników Aby uzyskać więcej informacji o doskonałych funkcji oferowanych przez AMS.  

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]
