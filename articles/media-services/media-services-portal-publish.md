<properties
    pageTitle="  Publikowanie zawartości za pomocą portalu Azure | Microsoft Azure"
    description="Ten samouczek przeprowadzi Cię przez kroki publikacji zawartości Portal Azure."
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

# <a name="publish-content-with-the-azure-portal"></a>Publikowanie zawartości za pomocą portalu Azure

> [AZURE.SELECTOR]
- [Portal](media-services-portal-publish.md)
- [.NET](media-services-deliver-streaming-content.md)
- [POZOSTAŁE](media-services-rest-deliver-streaming-content.md)

## <a name="overview"></a>Omówienie

> [AZURE.NOTE] Aby użyć tego samouczka, potrzebne jest konto Azure. Aby uzyskać szczegółowe informacje zobacz [Azure bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/). 

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

Aby uzyskać więcej informacji zobacz [Omówienie zawartości dostarczanie](media-services-deliver-content-overview.md).

>[AZURE.NOTE] Jeśli portal umożliwia tworzenie Locator przed marca 2015 r, Locator z daty wygaśnięcia dwóch lat zostały utworzone.  

Aby zaktualizować datę wygaśnięcia na uchwycie, użyj [pozostałych](http://msdn.microsoft.com/library/azure/hh974308.aspx#update_a_locator ) lub interfejsów API [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) . Należy zauważyć, że po zaktualizowaniu datę wygaśnięcia locator skojarzeń zabezpieczeń adres URL zmienia się.

### <a name="to-use-the-portal-to-publish-an-asset"></a>Aby opublikować środka trwałego za pomocą portalu

Aby opublikować środka trwałego za pomocą portalu, wykonaj następujące czynności:

1. W [portalu Azure](https://portal.azure.com/)wybierz swoje konto usługi multimediów Azure.
1. Wybierz pozycję **Ustawienia** > **aktywów**.
1. Wybierz pozycję elementu, który chcesz opublikować.
1. Kliknij przycisk **Publikuj** .
1. Wybierz typ locator.
2. Naciśnij klawisz, **Dodaj**.

    ![Publikowanie](./media/media-services-portal-vod-get-started/media-services-publish1.png)

Adres URL zostanie dodana do listy adresów URL **Opublikowane**.

## <a name="play-content-from-the-portal"></a>Odtwarzanie zawartości z portalu

Azure portal udostępnia odtwarzaczu zawartości, którego można używać do testowania klipu wideo.

Kliknij odpowiedni plik wideo, a następnie kliknij przycisk **Odtwórz** .

![Publikowanie](./media/media-services-portal-vod-get-started/media-services-play.png)

Niektóre kwestie:

- Upewnij się, że został opublikowany wideo.
- To **odtwarzacza** jest odtwarzany w domyślnej streaming punktu końcowego. Jeśli chcesz odtworzyć z punkt końcowy strumieniowych inne niż domyślne, kliknij, aby skopiuj adres URL i użyć innego odtwarzacza. Na przykład [Usługi Azure odtwarzacza](http://amsplayer.azurewebsites.net/azuremediaplayer.html).
- Przesyłanie strumieniowe punktu końcowego, z której streaming musi być zainstalowany.  
- Aby przesyłać strumieniowo ze strumieniowych punktu końcowego, należy dodać co najmniej jedną jednostkę przesyłanie strumieniowe. Aby uzyskać więcej informacji zobacz [w tym](media-services-portal-scale-streaming-endpoints.md) temacie.   

##<a name="next-steps"></a>Następne kroki

Przejrzyj ścieżki szkoleniowe dla użytkowników usługi multimediów.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


