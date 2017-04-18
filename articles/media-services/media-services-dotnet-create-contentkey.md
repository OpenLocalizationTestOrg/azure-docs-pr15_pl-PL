<properties 
    pageTitle="Tworzenie ContentKeys przy użyciu programu .NET" 
    description="Dowiedz się, jak tworzyć klucze zawartości, które zapewniają bezpieczny dostęp do zasobów." 
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


#<a name="create-contentkeys-with-net"></a>Tworzenie ContentKeys przy użyciu programu .NET

> [AZURE.SELECTOR]
- [POZOSTAŁE](media-services-rest-create-contentkey.md)
- [.NET](media-services-dotnet-create-contentkey.md)

Usługi multimediów umożliwia tworzenie i przedstawianie zaszyfrowanych aktywów. **ContentKey** zapewnia bezpieczny dostęp do Twojej s **zawartości**. 

Podczas tworzenia nowych elementów zawartości (na przykład przed [przekazywać pliki](media-services-dotnet-upload-files.md)), można określić następujące opcje szyfrowania: **StorageEncrypted**, **CommonEncryptionProtected**lub **EnvelopeEncryptionProtected**. 

Podczas wyświetlania elementów dla klientów, można [skonfigurować dla trwałych dynamicznie szyfrowania](media-services-dotnet-configure-asset-delivery-policy.md) z jednym z następujących dwóch skonfigurowała: **DynamicEnvelopeEncryption** lub **DynamicCommonEncryption**.

Zaszyfrowane aktywa muszą być skojarzone z **ContentKey**s. W tym artykule opisano, jak utworzyć klucz zawartości.

>[AZURE.NOTE] Podczas tworzenia nowego środka **StorageEncrypted** przy użyciu zestawu SDK .NET usługi multimediów, **ContentKey** jest automatycznie tworzone i połączone za pomocą elementu.

##<a name="contentkeytype"></a>ContentKeyType

Jedną z wartości, że należy ustawić podczas tworzenia zawartości klucz jest klucza typu zawartości. Wybierz jedną z następujących wartości. 

    public enum ContentKeyType
    {
        /// <summary>
        /// Specifies a content key for common encryption.
        /// </summary>
        /// <remarks>This is the default value.</remarks>
        CommonEncryption = 0,

        /// <summary>
        /// Specifies a content key for storage encryption.
        /// </summary>
        StorageEncryption = 1,

        /// <summary>
        /// Specifies a content key for configuration encryption.
        /// </summary>
        ConfigurationEncryption = 2,

        /// <summary>
        /// Specifies a content key for Envelope encryption.  Only used internally.
        /// </summary>
        EnvelopeEncryption = 4
    }

##<a id="envelope_contentkey"></a>Tworzenie koperty typu ContentKey

Poniższy fragment kodu tworzy klucz zawartości typu szyfrowania koperty. Kojarzy następnie klawisz z określonej zawartości.

    static public IContentKey CreateEnvelopeTypeContentKey(IAsset asset)
    {
        // Create envelope encryption content key
        Guid keyId = Guid.NewGuid();
        byte[] contentKey = GetRandomBuffer(16);

        IContentKey key = _context.ContentKeys.Create(
                                keyId,
                                contentKey,
                                "ContentKey",
                                ContentKeyType.EnvelopeEncryption);

        asset.ContentKeys.Add(key);

        return key;
    }

    static private byte[] GetRandomBuffer(int size)
    {
        byte[] randomBytes = new byte[size];
        using (RNGCryptoServiceProvider rng = new RNGCryptoServiceProvider())
        {
            rng.GetBytes(randomBytes);
        }

        return randomBytes;
    }

Nawiązywanie połączenia

    IContentKey key = CreateEnvelopeTypeContentKey(encryptedsset);



##<a id="common_contentkey"></a>Tworzenie wspólny typ ContentKey    

Poniższy fragment kodu tworzy klucz zawartości na wspólny typ szyfrowania. Kojarzy następnie klawisz z określonej zawartości.

    static public IContentKey CreateCommonTypeContentKey(IAsset asset)
    {
        // Create common encryption content key
        Guid keyId = Guid.NewGuid();
        byte[] contentKey = GetRandomBuffer(16);

        IContentKey key = _context.ContentKeys.Create(
                                keyId,
                                contentKey,
                                "ContentKey",
                                ContentKeyType.CommonEncryption);

        // Associate the key with the asset.
        asset.ContentKeys.Add(key);

        return key;
    }

    static private byte[] GetRandomBuffer(int length)
    {
        var returnValue = new byte[length];

        using (var rng =
            new System.Security.Cryptography.RNGCryptoServiceProvider())
        {
            rng.GetBytes(returnValue);
        }

        return returnValue;
    }
Nawiązywanie połączenia

    IContentKey key = CreateCommonTypeContentKey(encryptedsset); 


##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
