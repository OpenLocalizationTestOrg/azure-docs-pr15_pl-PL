<properties 
    pageTitle="Odtwarzanie zawartości | Microsoft Azure" 
    description="Ten temat zawiera listę uczestników istniejących że służy do odtwarzania zawartości." 
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
    ms.topic="article" 
    ms.date="10/12/2016" 
    ms.author="juliako"/>


#<a name="playing-your-content-with-existing-players"></a>Odtwarzanie zawartości z istniejącej graczami

Usługi multimediów Azure obsługuje wiele popularnych przesyłanie strumieniowe formatach, na przykład Streaming gładkie, Live przesyłania strumieniowego HTTP i MPEG-kreska. W tym temacie wskazano istniejących graczami, które służą do testowania strumienie.

>[AZURE.NOTE]Do odtwarzania dynamicznie spakowany lub dynamicznie zaszyfrowanej zawartości, upewnij się, że uzyskać co najmniej jedną jednostkę przesyłanie strumieniowe przesyłanie strumieniowe punktu końcowego, z której ma zostać dostarczania zawartości. Aby dowiedzieć się, jak skalowania przesyłanie strumieniowe jednostki, zobacz: [sposobu skalowania przesyłanie strumieniowe jednostki](media-services-portal-manage-streaming-endpoints.md).

### <a name="the-azure-portal-media-services-content-player"></a>Azure portalu usługi multimediów zawartości Windows Media player

**Azure** portal udostępnia odtwarzaczu zawartości, którego można używać do testowania klipu wideo.

Kliknij odpowiedni klip wideo (Upewnij się, został on [opublikowany](media-services-portal-publish.md)) i kliknij przycisk **Odtwórz** w dolnej części portalu.

Niektóre kwestie:

- **MEDIA PLAYER zawartości usług** odtwarzany w domyślnej streaming punktu końcowego. Jeśli ma być odtwarzany z punkt końcowy strumieniowych inne niż domyślne, użyj innego odtwarzacza. Na przykład [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).


![AMSPlayer][AMSPlayer]

###<a name="azure-media-player"></a>Odtwarzacza multimediów Azure

Za pomocą [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html) do odtwarzania zawartości (Wyczyść lub chroniony) w dowolnej z następujących formatów:

- Gładkie strumieniowego przesyłania
- MPEG ŁĄCZNIKA
- HLS
- Stopniowe MP4


###<a name="flash-player"></a>Program Flash Player

####<a name="aes-encrypted-with-token"></a>Szyfrowane AES z tokenu

[http://aestoken.azurewebsites.NET](http://aestoken.azurewebsites.net)

###<a name="silverlight-players"></a>Odtwarzacze Silverlight

####<a name="monitoring"></a>Monitorowanie

[http://SMF.cloudapp.NET/healthmonitor](http://smf.cloudapp.net/healthmonitor)

####<a name="playready-with-token"></a>PlayReady z tokenu

[http://sltoken.azurewebsites.NET](http://sltoken.azurewebsites.net)

### <a name="dash-players"></a>Odtwarzacze ŁĄCZNIKA

[http://dashplayer.azurewebsites.NET](http://dashplayer.azurewebsites.net)

[http://dashif.org](http://dashif.org)

###<a name="other"></a>Inne

Aby przetestować adresy URL HLS umożliwia także:

- **Safari** na urządzeniu z systemem iOS lub
- **Odtwarzacz HLS 3ivx** w systemie Windows.

##<a name="developing-video-players"></a>Opracowywanie odtwarzacze wideo

Uzyskać informacji o opracowywaniu własne graczami zobacz [rozwijających się odtwarzacze wideo](media-services-develop-video-players.md)




##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


[AMSPlayer]: ./media/media-services-playback-content-with-existing-players/media-services-portal-player.png
