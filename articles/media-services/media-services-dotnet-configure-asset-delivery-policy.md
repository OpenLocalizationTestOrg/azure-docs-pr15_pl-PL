<properties 
    pageTitle="Konfigurowanie zasad dostarczania zawartości z .NET SDK | Microsoft Azure" 
    description="W tym temacie przedstawiono sposób konfigurowania zasad dostarczania różnych elementów zawartości z SDK .NET usługi multimediów Azure." 
    services="media-services" 
    documentationCenter="" 
    authors="Mingfeiy" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/19/2016"
    ms.author="juliako;mingfeiy"/>

#<a name="configure-asset-delivery-policies-with-net-sdk"></a>Konfigurowanie zasad dostarczania zawartości z zestawu SDK usługi .NET
[AZURE.INCLUDE [media-services-selector-asset-delivery-policy](../../includes/media-services-selector-asset-delivery-policy.md)]

##<a name="overview"></a>Omówienie

Jeśli planujesz aktywów dostarczenia szyfrowane jeden z kroków w przepływie pracy dostarczania zawartości usługi multimediów jest konfigurowanie zasad dostarczania trwałych. Zasady dostarczania zawartości informuje usługi multimediów sposobu dla swojego trwałego dostarczanych: do których streaming protokołu należy do zasobów dynamicznie umieszczane (na przykład, kreska MPEG, HLS, gładkie Streaming lub wszystkie), czy chcesz zaszyfrować dynamicznie do zawartości i w jaki sposób (kopert lub szyfrowania typowych).

W tym temacie omówiono dlaczego i jak utworzyć i skonfigurować zasady dostarczania zawartości.

>[AZURE.NOTE]Aby móc używać dynamiczne opakowania i szyfrowania dynamiczne, należy się upewnić mieć co najmniej jedną jednostkę skali (nazywane także przesyłanie strumieniowe jednostka). Aby uzyskać więcej informacji zobacz [jak skalowanie usługi multimediów](media-services-portal-manage-streaming-endpoints.md).
>
>Ponadto do zawartości musi zawierać zestaw adaptacyjne szybkość transmisji bitów MP4s lub adaptacyjne szybkość transmisji bitów wygładzonymi strumieniowego przesyłania plików.

Na tym samym trwały, można zastosować różne zasady. Na przykład można zastosować PlayReady szyfrowania do szyfrowania wygładzonymi przesyłanie strumieniowe i AES koperty do ŁĄCZNIKA MPEG i HLS. Protokołów, które nie są zdefiniowane w zasadzie dostarczenia (na przykład możesz dodać pojedynczy zasadę tylko określająca HLS jako protokół) będą zablokowanie streaming. Wyjątek to, czy masz określono na wszystkich zasad dostarczania zawartości. Następnie wszystkie protokoły będą mogli w Wyczyść.

Jeśli chcesz przedstawić zasób zaszyfrowanych miejsca do magazynowania, należy skonfigurować zasady dostarczania zawartości. Przed można przesłać strumieniowo do zawartości, serwera strumieniowych usuwa szyfrowania miejsca do magazynowania i umożliwia strumieniowe przesyłanie zawartości za pomocą zasad określonego odbiorcy. Na przykład aby zapewnić usługi trwałego zaszyfrowane kluczem szyfrowania koperty szyfrowania AES (Advanced Standard), Ustaw typ zasad **DynamicEnvelopeEncryption**. Aby usunąć szyfrowanie miejsca do magazynowania i przesyłać strumieniowo zawartości w Wyczyść, Ustaw typ zasad **NoDynamicEncryption**. Przykłady pokazano, jak skonfigurować następujące typy zasad wykonaj.

W zależności od konfiguracji zasad dostarczania zawartości będzie można dynamicznie pakowanie, dynamicznie szyfrowanie i przesyłać strumieniowo następujących protokołów: strumienie wygładzonymi Streaming, HLS KRESKOWANIA MPEG i obr. / min.

Na poniższej liście przedstawiono formaty umożliwia strumienia gładkie, HLS, kreska i obr. / min.

Przesyłanie strumieniowe wygładzonymi:

{streaming punktu końcowego usługi multimediów nazwę konta name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

HLS:

{streaming punktu końcowego usługi multimediów nazwę konta name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

MPEG ŁĄCZNIKA

{streaming punktu końcowego usługi multimediów nazwę konta name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

OBR. / MIN

{streaming punktu końcowego usługi multimediów nazwę konta name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=f4m-f4f)

Aby uzyskać instrukcje, jak publikować środka trwałego i tworzyć przesyłanie strumieniowe adresu URL zobacz [Tworzenie przesyłanie strumieniowe adresu URL](media-services-deliver-streaming-content.md).

##<a name="considerations"></a>Zagadnienia dotyczące

- Nie można usunąć AssetDeliveryPolicy skojarzone z środka trwałego podczas locator (przesyłanie strumieniowe) na żądanie nie istnieje dla tego zasobu. Zaleca się usunąć zasady z elementu przed usunięciem zasad.
- Nie można utworzyć przesyłanie strumieniowe locator trwałego zaszyfrowanych miejsca do magazynowania, jeśli nie zasady dostarczania zawartości są ustawione.  Jeśli elementu nie jest zaszyfrowany miejsca do magazynowania, system pozwoli locator tworzenie i przesyłanie strumieniowe zawartości w wyczyść bez zasad dostarczania zawartości.
- Można mieć wiele zasad dostarczania zawartości skojarzonych z pojedynczego zasobu, ale można określić tylko jeden sposób, aby obsługiwać danej AssetDeliveryProtocol.  Co oznacza, spróbuj połączyć dwie zasady dostarczenia, które określają protokół AssetDeliveryProtocol.SmoothStreaming, który będzie wynikiem jest błąd, ponieważ system nie wie, której jeden ma zostać zastosowany, gdy klient wysyła żądanie Streaming gładkie.
- Jeśli masz środka trwałego z istniejących locator przesyłanie strumieniowe elementu nie można połączyć nowych zasad (można odłączyć istniejące zasady z elementu, lub aktualizowanie zasady dostarczania skojarzonych z trwałym).  Najpierw musisz usunąć przesyłanie strumieniowe locator, dostosowywanie zasad i ponownie utworzyć przesyłanie strumieniowe locator.  Za pomocą samego locatorId gdy odtworzenie przesyłanie strumieniowe locator, ale należy upewnić się, że nie który powodować problemy dla klientów, ponieważ zawartości mogą być buforowane pochodzenie lub CDN za.


##<a name="clear-asset-delivery-policy"></a>Zasady dostarczania wyczyść składników majątku

Z poniższej metody **ConfigureClearAssetDeliveryPolicy** określa się nie stosuj dynamiczne szyfrowania i dostarczyć strumienia w dowolnej z następujących protokołów: ŁĄCZNIKA MPEG, HLS i wygładzonymi Streaming protokołów. Warto te zasady zastosowane do aktywów miejsca do magazynowania szyfrowane.

Aby uzyskać informacje dotyczące wartości, które można określić podczas tworzenia AssetDeliveryPolicy zobacz sekcję [typy używane podczas definiowania AssetDeliveryPolicy](#types) .

statyczne ConfigureClearAssetDeliveryPolicy void publicznej (IAsset zawartości) {zasad IAssetDeliveryPolicy = _context. AssetDeliveryPolicies.Create ("Zasad Wyczyść" AssetDeliveryPolicyType.NoDynamicEncryption, AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.Dash, null);

trwały. DeliveryPolicies.Add(policy); }

##<a name="dynamiccommonencryption-asset-delivery-policy"></a>Zasady dostarczania zawartości DynamicCommonEncryption


Z poniższej metody **CreateAssetDeliveryPolicy** tworzy **AssetDeliveryPolicy** jest skonfigurowany do stosowanie dynamiczne szyfrowania typowych (**DynamicCommonEncryption**) wygładzonymi protokołu przesyłanie strumieniowe (inne protokoły zostaną zablokowane z streaming). Metoda przyjmuje dwa parametry: **trwały** (zawartości, do którego chcesz zastosować zasady dostarczania) i **IContentKey** (klucz zawartości typu **CommonEncryption** , aby uzyskać więcej informacji, zobacz: [Tworzenie zawartości klucza](media-services-dotnet-create-contentkey.md#common_contentkey)).

Aby uzyskać informacje dotyczące wartości, które można określić podczas tworzenia AssetDeliveryPolicy zobacz sekcję [typy używane podczas definiowania AssetDeliveryPolicy](#types) .


statyczne publicznej CreateAssetDeliveryPolicy void (IAsset zawartości, klawisz IContentKey) {identyfikator Uri acquisitionUrl = klucz. GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);

AssetDeliveryPolicyConfiguration słownika < AssetDeliveryPolicyConfigurationKey, ciąg > Nowy słownik < AssetDeliveryPolicyConfigurationKey, ciąg > = {{AssetDeliveryPolicyConfigurationKey.PlayReadyLicenseAcquisitionUrl, acquisitionUrl.ToString()}};

        var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                "AssetDeliveryPolicy",
            AssetDeliveryPolicyType.DynamicCommonEncryption,
            AssetDeliveryProtocol.SmoothStreaming,
            assetDeliveryPolicyConfiguration);

        // Add AssetDelivery Policy to the asset
        asset.DeliveryPolicies.Add(assetDeliveryPolicy);

        Console.WriteLine();
        Console.WriteLine("Adding Asset Delivery Policy: " +
            assetDeliveryPolicy.AssetDeliveryPolicyType);
    }

Usługi multimediów Azure umożliwia dodawanie szyfrowania Widevine. Poniższy przykład pokazuje zarówno PlayReady i Widevine dodana do zasad dostarczania zawartości.


    static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
    {
        // Get the PlayReady license service URL.
        Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);
        

        // GetKeyDeliveryUrl for Widevine attaches the KID to the URL.
        // For example: https://amsaccount1.keydelivery.mediaservices.windows.net/Widevine/?KID=268a6dcb-18c8-4648-8c95-f46429e4927c.  
        // The WidevineBaseLicenseAcquisitionUrl (used below) also tells Dynamaic Encryption 
        // to append /? KID =< keyId > to the end of the url when creating the manifest.
        // As a result Widevine license acquisition URL will have KID appended twice, 
        // so we need to remove the KID that in the URL when we call GetKeyDeliveryUrl.

        Uri widevineUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.Widevine);
        UriBuilder uriBuilder = new UriBuilder(widevineUrl);
        uriBuilder.Query = String.Empty;
        widevineUrl = uriBuilder.Uri;

        Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
            new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
        {
            {AssetDeliveryPolicyConfigurationKey.PlayReadyLicenseAcquisitionUrl, acquisitionUrl.ToString()},
            {AssetDeliveryPolicyConfigurationKey.WidevineLicenseAcquisitionUrl, widevineUrl.ToString()}

        };

        var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                "AssetDeliveryPolicy",
            AssetDeliveryPolicyType.DynamicCommonEncryption,
            AssetDeliveryProtocol.Dash,
            assetDeliveryPolicyConfiguration);


        // Add AssetDelivery Policy to the asset
        asset.DeliveryPolicies.Add(assetDeliveryPolicy);

    }

>[AZURE.NOTE]W przypadku szyfrowania z Widevine, czy tylko można to zrobić przy użyciu ŁĄCZNIKA. Upewnij się określić ŁĄCZNIKA w Protokół dostarczania zawartości.


##<a name="dynamicenvelopeencryption-asset-delivery-policy"></a>Zasady dostarczania zawartości DynamicEnvelopeEncryption 

Z poniższej metody **CreateAssetDeliveryPolicy** tworzy **AssetDeliveryPolicy** skonfigurowanym dotyczyć Koperta dynamiczne szyfrowania (**DynamicEnvelopeEncryption**) protokołów wygładzonymi Streaming, HLS i ŁĄCZNIKA (Jeśli zechcesz określa niektóre protokoły one zostaną zablokowane z streaming). Metoda przyjmuje dwa parametry: **trwały** (zawartości, do którego chcesz zastosować zasady dostarczania) i **IContentKey** (klucz zawartości typu **EnvelopeEncryption** , aby uzyskać więcej informacji, zobacz: [Tworzenie zawartości klucza](media-services-dotnet-create-contentkey.md#envelope_contentkey)).


Aby uzyskać informacje dotyczące wartości, które można określić podczas tworzenia AssetDeliveryPolicy zobacz sekcję [typy używane podczas definiowania AssetDeliveryPolicy](#types) .   

    private static void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
    {
        
        //  Get the Key Delivery Base Url by removing the Query parameter.  The Dynamic Encryption service will
        //  automatically add the correct key identifier to the url when it generates the Envelope encrypted content
        //  manifest.  Omitting the IV will also cause the Dynamice Encryption service to generate a deterministic
        //  IV for the content automatically.  By using the EnvelopeBaseKeyAcquisitionUrl and omitting the IV, this
        //  allows the AssetDelivery policy to be reused by more than one asset.
        //
        Uri keyAcquisitionUri = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.BaselineHttp);
        UriBuilder uriBuilder = new UriBuilder(keyAcquisitionUri);
        uriBuilder.Query = String.Empty;
        keyAcquisitionUri = uriBuilder.Uri;

        // The following policy configuration specifies: 
        //   key url that will have KID=<Guid> appended to the envelope and
        //   the Initialization Vector (IV) to use for the envelope encryption.
        Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
            new Dictionary<AssetDeliveryPolicyConfigurationKey, string> 
        {
            {AssetDeliveryPolicyConfigurationKey.EnvelopeBaseKeyAcquisitionUrl, keyAcquisitionUri.ToString()},
        };

        IAssetDeliveryPolicy assetDeliveryPolicy =
            _context.AssetDeliveryPolicies.Create(
                        "AssetDeliveryPolicy",
                        AssetDeliveryPolicyType.DynamicEnvelopeEncryption,
                        AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.Dash,
                        assetDeliveryPolicyConfiguration);

        // Add AssetDelivery Policy to the asset
        asset.DeliveryPolicies.Add(assetDeliveryPolicy);

        Console.WriteLine();
        Console.WriteLine("Adding Asset Delivery Policy: " + assetDeliveryPolicy.AssetDeliveryPolicyType);
    }


##<a id="types"></a>Typy używane podczas definiowania AssetDeliveryPolicy

###<a id="AssetDeliveryProtocol"></a>AssetDeliveryProtocol 

    /// <summary>
    /// Delivery protocol for an asset delivery policy.
    /// </summary>
    [Flags]
    public enum AssetDeliveryProtocol
    {
        /// <summary>
        /// No protocols.
        /// </summary>
        None = 0x0,

        /// <summary>
        /// Smooth streaming protocol.
        /// </summary>
        SmoothStreaming = 0x1,

        /// <summary>
        /// MPEG Dynamic Adaptive Streaming over HTTP (DASH)
        /// </summary>
        Dash = 0x2,

        /// <summary>
        /// Apple HTTP Live Streaming protocol.
        /// </summary>
        HLS = 0x4,

        /// <summary>
        /// Adobe HTTP Dynamic Streaming (HDS)
        /// </summary>
        Hds = 0x8,

        /// <summary>
        /// Include all protocols.
        /// </summary>
        All = 0xFFFF
    }

###<a id="AssetDeliveryPolicyType"></a>AssetDeliveryPolicyType

    /// <summary>
    /// Policy type for dynamic encryption of assets.
    /// </summary>
    public enum AssetDeliveryPolicyType
    {
        /// <summary>
        /// Delivery Policy Type not set.  An invalid value.
        /// </summary>
        None,

        /// <summary>
        /// The Asset should not be delivered via this AssetDeliveryProtocol. 
        /// </summary>
        Blocked, 

        /// <summary>
        /// Do not apply dynamic encryption to the asset.
        /// </summary>
        /// 
        NoDynamicEncryption,  

        /// <summary>
        /// Apply Dynamic Envelope encryption.
        /// </summary>
        DynamicEnvelopeEncryption,

        /// <summary>
        /// Apply Dynamic Common encryption.
        /// </summary>
        DynamicCommonEncryption
    }

###<a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType

    /// <summary>
    /// Delivery method of the content key to the client.
    /// </summary>
    public enum ContentKeyDeliveryType
    {
        /// <summary>
        /// None.
        /// </summary>
        None = 0,

        /// <summary>
        /// Use PlayReady License acquistion protocol
        /// </summary>
        PlayReadyLicense = 1,

        /// <summary>
        /// Use MPEG Baseline HTTP key protocol.
        /// </summary>
        BaselineHttp = 2,

        /// <summary>
        /// Use Widevine License acquistion protocol
        ///
        </summary>
        Widevine = 3

    }

###<a id="AssetDeliveryPolicyConfigurationKey"></a>AssetDeliveryPolicyConfigurationKey

    /// <summary>
    /// Keys used to get specific configuration for an asset delivery policy.
    /// </summary>
    public enum AssetDeliveryPolicyConfigurationKey
    {
        /// <summary>
        /// No policies.
        /// </summary>
        None,

        /// <summary>
        /// Exact Envelope key URL.
        /// </summary>
        EnvelopeKeyAcquisitionUrl,

        /// <summary>
        /// Base key url that will have KID=<Guid> appended for Envelope.
        /// </summary>
        EnvelopeBaseKeyAcquisitionUrl,
        
        /// <summary>
        /// The initialization vector to use for envelope encryption in Base64 format.
        /// </summary>
        EnvelopeEncryptionIVAsBase64,

        /// <summary>
        /// The PlayReady License Acquisition Url to use for common encryption.
        /// </summary>
        PlayReadyLicenseAcquisitionUrl,

        /// <summary>
        /// The PlayReady Custom Attributes to add to the PlayReady Content Header
        /// </summary>
        PlayReadyCustomAttributes,

        /// <summary>
        /// The initialization vector to use for envelope encryption.
        /// </summary>
        EnvelopeEncryptionIV,

        /// <summary>
        /// Widevine DRM acquisition url
        /// </summary>
        WidevineLicenseAcquisitionUrl
    }

##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


