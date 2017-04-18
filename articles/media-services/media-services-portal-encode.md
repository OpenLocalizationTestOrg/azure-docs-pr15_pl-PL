<properties
    pageTitle="Kodowanie środka trwałego za pomocą Media Encoder standardowy Azure portal | Microsoft Azure"
    description="Ten samouczek przeprowadzi Cię przez kroki kodowania środka trwałego za pomocą Media Encoder standardowy Azure portal."
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
    ms.date="10/24/2016"
    ms.author="juliako"/>


# <a name="encode-an-asset-using-media-encoder-standard-with-the-azure-portal"></a>Kodowanie środka trwałego za pomocą Media Encoder standardowy Azure portal

> [AZURE.NOTE] Aby użyć tego samouczka, potrzebne jest konto Azure. Aby uzyskać szczegółowe informacje zobacz [Azure bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/). 

Podczas pracy z jedną z najbardziej typowe scenariusze dostarczane szybkość transmisji bitów adaptacyjne streaming do klientów usługi multimediów Azure. Usługi multimediów obsługuje następujące adaptacyjne szybkość transmisji bitów streaming technologii: HTTP Live Streaming (HLS), gładkie Streaming KRESKOWANIA MPEG i obr. / min (w przypadku tylko licencjobiorców Adobe PrimeTime-dostęp). Aby przygotować pliki wideo do strumieniowego przesyłania adaptacyjne szybkość transmisji bitów, musisz kodowanie wideo źródła do szybkość transmisji bitów wielu plików. Należy użyć **Media Encoder Standard** kodowania do kodowania wideo.  

Usługi multimediów są także opakowań dynamiczne, umożliwiające dostarczanie usługi MP4s szybkość transmisji bitów wielokrotne w następujących formatach przesyłanie strumieniowe: KRESKI MPEG, HLS, gładkie Streaming lub obr. / min, bez konieczności ponownego pakietów do tych streaming format. Z dynamicznego opakowań potrzebne do przechowywania i zapłacić za pliki w formacie jednego miejsca do magazynowania i usługi multimediów będą tworzyć i służyć właściwej reakcji według żądaniami od klienta.

Aby skorzystać z dynamicznego opakowania, musisz wykonaj następujące czynności:

- Kodowanie pliku źródłowego do zestawu szybkość transmisji bitów wielu plików MP4 (kodowania czynności przedstawiono później w tej sekcji).
- Uzyskaj co najmniej jedną jednostkę przesyłanie strumieniowe przesyłanie strumieniowe punktu końcowego, z której ma zostać dostarczania zawartości. Aby uzyskać więcej informacji zobacz [Konfigurowanie przesyłania strumieniowego punktów końcowych](media-services-portal-vod-get-started.md#configure-streaming-endpoints). 

Aby przeskalować przetwarzanie multimediów, zobacz [w tym](media-services-portal-scale-media-processing.md) temacie.

## <a name="encode-with-the-azure-portal"></a>Kodowanie Portal Azure

W tej sekcji opisano czynności, które można wykonać w celu kodowania zawartości z Media Encoder standardowy.

1.  W [portalu Azure](https://portal.azure.com/)wybierz swoje konto usługi multimediów Azure.
2.  W oknie **Ustawienia** wybierz pozycję **elementy zawartości**.  
2.  W oknie **składniki majątku** wybierz pozycję elementu, który ma zostać zakodowany.
3.  Naciśnij przycisk **kodowanie** .
4.  W oknie **kodowanie środka trwałego** wybierz pozycję "Media Encoder standardowy" procesora i ustawienia domyślne. Na przykład jeśli wiesz, wprowadzania wideo ma rozdzielczość 1920 x 1080 pikseli, następnie można "H264 wielu szybkości transmisji bitów 1080p" wstępnie ustawionych. Aby uzyskać więcej informacji na temat ustawień wstępnych, zobacz [ten](https://msdn.microsoft.com/library/azure/mt269960.aspx) artykuł — należy zaznacz najbardziej odpowiednie dla wideo wejściowe. Jeśli masz klipu wideo o niskiej rozdzielczości (640 x 360), a następnie należy nie używać domyślnego "H264 wielu szybkości transmisji bitów 1080p" wstępnie ustawionych.
    
    Ułatwia zarządzanie masz opcję edycji nazwę elementu dane wyjściowe, a nazwa zadania.
        
    ![Kodowanie składników majątku](./media/media-services-portal-vod-get-started/media-services-encode1.png)
5. Naciśnij klawisz **Tworzenie**.


##<a name="next-step"></a>Następny krok

Można monitorować kodowania postęp zadania Portal Azure, zgodnie z opisem w [tym](media-services-portal-check-job-progress.md) artykule.  

##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


