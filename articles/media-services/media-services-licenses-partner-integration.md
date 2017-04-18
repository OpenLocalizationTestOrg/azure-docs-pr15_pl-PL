<properties 
    pageTitle="Dostarczanie Widevine licencji usługi multimediów Azure za pomocą partnerów | Microsoft Azure" 
    description="W tym artykule opisano, jak usługi multimediów Azure (AMS) umożliwia przedstawianie strumień dynamicznie są szyfrowane za pomocą AMS PlayReady i Widevine DRMs. Licencja PlayReady pochodzi z serwera licencji PlayReady usługi multimediów i licencji Widevine został dostarczony przez serwer licencji castLabs." 
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
    ms.date="09/26/2016"  
    ms.author="juliako"/>

#<a name="using-partners-to-deliver-widevine-licenses-to-azure-media-services"></a>Dostarczanie Widevine licencji usługi multimediów Azure za pomocą partnerów

##<a name="overview"></a>Omówienie

Usługi multimediów Azure firmy Microsoft umożliwia przedstawianie MPEG-kreska chronione za pomocą DRM Widevine, które są szyfrowane na specyfikacji typowych szyfrowania (CENC).

Począwszy od multimediów usług .NET SDK wersji 3.5.2 usługi multimediów umożliwia konfigurowanie szablonu licencji Widevine i się Widevine licencji. Umożliwia także następujące partnerów AMS ułatwiające przeprowadzania licencji Widevine: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/) [castLabs](http://castlabs.com/company/partners/azure/).

##<a name="castlabs"></a>castLabs

Za pomocą [castLabs](http://castlabs.com/company/partners/azure/) do przeprowadzania Widevine licencji. Aby uzyskać więcej informacji zobacz [Używanie castLabs do przeprowadzania DRM licencje usługi multimediów Azure](media-services-castlabs-integration.md)

##<a name="axinom"></a>Axinom

Za pomocą [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/) do przeprowadzania Widevine licencji. Aby uzyskać więcej informacji zobacz [Używanie Axinom do przeprowadzania DRM licencje usługi multimediów Azure](media-services-axinom-integration.md)


##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Zobacz też

[Przy użyciu PlayReady i/lub Widevine dynamiczne szyfrowania typowych](media-services-protect-with-drm.md)

[Mingfei w blogu](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/)

