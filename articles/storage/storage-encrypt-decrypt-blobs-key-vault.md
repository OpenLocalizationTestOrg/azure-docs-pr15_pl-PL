<properties
    pageTitle="Samouczek: Szyfrowania i odszyfrowywania obiektów blob w magazynie Azure firmy Microsoft przy użyciu magazynu klucza Azure | Microsoft Azure"
    description="Ten samouczek przeprowadzi Cię przez proces szyfrowania i odszyfrowywania obiektów blob przy użyciu szyfrowania po stronie klienta dla magazyn Microsoft Azure z magazynu klucza Azure."
    services="storage"
    documentationCenter=""
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="required"
    ms.date="10/18/2016"
    ms.author="lakasa;robinsh"/>

# <a name="tutorial-encrypt-and-decrypt-blobs-in-microsoft-azure-storage-using-azure-key-vault"></a>Samouczek: Szyfrowania i odszyfrowywania obiektów blob w magazynie Azure firmy Microsoft przy użyciu magazynu klucza Azure

## <a name="introduction"></a>Wprowadzenie

Ten samouczek opisano, jak utworzyć za pomocą szyfrowania po stronie klienta miejsca do magazynowania z magazynu klucza Azure. Jego przeprowadzi Cię przez proces szyfrowania i odszyfrowywania obiektów blob w aplikacji konsoli przy użyciu tych technologii.

**Szacowany czas wykonania:** 20 minut

Informacje o magazynu klucza Azure, zobacz [Co to jest Azure klucza magazynu?](../key-vault/key-vault-whatis.md).

Informacje o szyfrowania po stronie klienta do przechowywania Azure zobacz [szyfrowania po stronie klienta i Azure klucza magazynu dla magazyn Microsoft Azure](storage-client-side-encryption.md).


## <a name="prerequisites"></a>Wymagania wstępne

Aby użyć tego samouczka, musisz mieć następujące czynności:

- Konta magazynu platformy Azure
- Program Visual Studio 2013 lub nowszym
- Azure programu PowerShell


## <a name="overview-of-client-side-encryption"></a>Omówienie szyfrowania po stronie klienta

Aby uzyskać omówienie szyfrowania po stronie klienta do przechowywania Azure zobacz [szyfrowania po stronie klienta i Azure klucza magazynu dla magazyn Microsoft Azure](storage-client-side-encryption.md)

Oto krótki opis działania szyfrowania po stronie klienta:

1. Klient magazyn Azure SDK generuje klucza szyfrowania zawartości (CEK), który jest klawiszem symetrycznej jednego jednorazowej użycia.
2. Dane klienta są szyfrowane za pomocą tego CEK.
3. CEK jest następnie zawinięty (szyfrowane) przy użyciu klucza szyfrowania (KEK). KEK jest identyfikowany za pomocą identyfikatora klucza i można być asymetrycznym pary kluczy lub symetrycznej klucza i można można zarządzać lokalnie lub przechowywane w Azure klucza magazynu. Klienta miejsca do magazynowania nigdy nie ma dostęp do KEK. Wywołuje ją po prostu Algorytm zawijania klucza dostarczonego przez klucz magazynu. Klienci mogą być klucza zawijanie rozpakowaniu czy za pomocą dostawców niestandardowych.
4. Następnie przekazaniu zaszyfrowane dane z usługą Azure magazyn.


## <a name="set-up-your-azure-key-vault"></a>Konfigurowanie usługi Azure klucza magazynu
Aby kontynuować z tego samouczka, należy wykonać następujące kroki, które zostały opisane w samouczku [wprowadzenie Azure klucza magazynu](../key-vault/key-vault-get-started.md):

- Tworzenie magazynu kluczy.
- Dodaj klucz lub hasło do klucza magazynu.
- Zarejestrowanie aplikacji z usługą Azure Active Directory.
- Autoryzuj aplikacji użyj klawisza lub hasło.

Zanotuj ClientID i ClientSecret, które zostały wygenerowane podczas rejestrowania aplikacji z usługą Azure Active Directory.

Tworzenie oba przyciski w klucza magazynu. Przyjmie pozostałej części samouczka użyto następujących nazw: ContosoKeyVault i TestRSAKey1.


## <a name="create-a-console-application-with-packages-and-appsettings"></a>Tworzenie aplikacji konsoli z pakietów i AppSettings

W programie Visual Studio Tworzenie nowej aplikacji konsoli.

Dodawanie pakietów nuget niezbędne w konsoli Menedżera pakietów.

    Install-Package WindowsAzure.Storage

    // This is the latest stable release for ADAL.
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.16.204221202

    Install-Package Microsoft.Azure.KeyVault
    Install-Package Microsoft.Azure.KeyVault.Extensions


Dodawanie AppSettings do App.Config.

    <appSettings>
        <add key="accountName" value="myaccount"/>
        <add key="accountKey" value="theaccountkey"/>
        <add key="clientId" value="theclientid"/>
        <add key="clientSecret" value="theclientsecret"/>
        <add key="container" value="stuff"/>
    </appSettings>

Dodaj następujący `using` instrukcji i upewnij się dodać odwołanie do System.Configuration do projektu.

    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System.Configuration;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    using Microsoft.Azure.KeyVault;
    using System.Threading;     
    using System.IO;


## <a name="add-a-method-to-get-a-token-to-your-console-application"></a>Dodawanie metody uzyskiwania token do aplikacji konsoli

Poniższa metoda jest używana przez klasy magazynu klucza, wymagających uwierzytelnienia dostępu do swojego klucza magazynu.

    private async static Task<string> GetToken(string authority, string resource, string scope)
    {
        var authContext = new AuthenticationContext(authority);
        ClientCredential clientCred = new ClientCredential(
            ConfigurationManager.AppSettings["clientId"],
            ConfigurationManager.AppSettings["clientSecret"]);
        AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

        if (result == null)
            throw new InvalidOperationException("Failed to obtain the JWT token");

        return result.AccessToken;
    }

## <a name="access-storage-and-key-vault-in-your-program"></a>Dostęp do przechowywania i klucza magazynu w programie

W tej funkcji głównego Dodaj następujący kod.

    // This is standard code to interact with Blob storage.
    StorageCredentials creds = new StorageCredentials(
        ConfigurationManager.AppSettings["accountName"],
        ConfigurationManager.AppSettings["accountKey"]);
    CloudStorageAccount account = new CloudStorageAccount(creds, useHttps: true);
    CloudBlobClient client = account.CreateCloudBlobClient();
    CloudBlobContainer contain = client.GetContainerReference(ConfigurationManager.AppSettings["container"]);
    contain.CreateIfNotExists();

    // The Resolver object is used to interact with Key Vault for Azure Storage.
    // This is where the GetToken method from above is used.
    KeyVaultKeyResolver cloudResolver = new KeyVaultKeyResolver(GetToken);


> [AZURE.NOTE] Magazyn kluczy obiektowych modeli
>
>Ważne jest, aby dowiedzieć się, że są faktyczną dwa klucza magazynu obiektowych modeli obowiązujących: jeden jest oparty na interfejsu API usługi REST (KeyVault nazw), a druga rozszerzenie szyfrowania po stronie klienta.

> Klient magazynu klucza interakcji z interfejsu API usługi REST i rozumie JSON klawisze sieci Web i hasła dla dwóch rodzajów czynności, które są zawarte w klucza magazynu.

> Rozszerzenia magazynu klucza są klas, które wydawać się utworzony specjalnie dla szyfrowania po stronie klienta w magazynie Azure. Zawierają interfejs kluczy (IKey) i klas oparty na koncepcji rozpoznawania klucza. Istnieją dwa implementacji IKey, które należy wiedzieć: RSAKey i SymmetricKey. Teraz stanie się z czynności, które są zawarte w magazynu klucza, ale w tym momencie są niezależnych klas (aby klucz i tajny pobrane przez klienta magazynu klucza nie zaimplementować IKey).


## <a name="encrypt-blob-and-upload"></a>Szyfrowanie obiektów blob i przekaż
Dodaj następujący kod do szyfrowania obiektów blob i przekaż go do swojego konta Azure miejsca do magazynowania. Metoda **ResolveKeyAsync** , która jest używana zwraca IKey.


    // Retrieve the key that you created previously.
    // The IKey that is returned here is an RsaKey.
    // Remember that we used the names contosokeyvault and testrsakey1.
    var rsa = cloudResolver.ResolveKeyAsync("https://contosokeyvault.vault.azure.net/keys/TestRSAKey1", CancellationToken.None).GetAwaiter().GetResult();


    // Now you simply use the RSA key to encrypt by setting it in the BlobEncryptionPolicy.
    BlobEncryptionPolicy policy = new BlobEncryptionPolicy(rsa, null);
    BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

    // Reference a block blob.
    CloudBlockBlob blob = contain.GetBlockBlobReference("MyFile.txt");

    // Upload using the UploadFromStream method.
    using (var stream = System.IO.File.OpenRead(@"C:\data\MyFile.txt"))
        blob.UploadFromStream(stream, stream.Length, null, options, null);


Oto zrzut ekranu z [Klasyczny Portal Azure](https://manage.windowsazure.com) dla obiektów blob, który został zaszyfrowany przy użyciu klawisza przechowywanych w magazynu klucza szyfrowania po stronie klienta. Właściwość **Identyfikator klucza** jest identyfikator URI klucza w magazynu klucza, działająca jako KEK. Właściwość **EncryptedKey** zawiera zaszyfrowana wersja CEK.

![Zrzut ekranu przedstawiający obiektów Blob metadanych, które zawiera metadane szyfrowania][1]

> [AZURE.NOTE] Podczas przeglądania w Konstruktorze BlobEncryptionPolicy pojawi się, że można zaakceptować klucz i/lub rozpoznawania nazw. Należy pamiętać, bezpośrednio po rozpoznawania nazw nie można używać do szyfrowania, ponieważ nie są obecnie działa obsługujące domyślny klawisz.



## <a name="decrypt-blob-and-download"></a>Odszyfrowywanie obiektów blob i Pobierz
Odszyfrowywanie jest wyjątkowo, gdy przy użyciu klas rozpoznawania nazw miały znaczenie. Identyfikator klucza używanego do szyfrowania jest skojarzony z obiektów blob w jego metadanych, dlatego nie można pobrać klucza, pamiętając, skojarzenie między klucz i obiektów blob powód. Masz upewnić się, że klawisz ten pozostaje w klucza magazynu.   

Klucz prywatny kluczem RSA pozostanie w magazynu klucz, więc do odszyfrowywania ma być wykonywana, klucz szyfrowane z metadanych obiektów blob, który zawiera CEK są wysyłane do magazynu klucz do odszyfrowywania.

Dodaj poniższe czynności, aby odszyfrowywanie blob, w którym zostały przekazane.

    // In this case, we will not pass a key and only pass the resolver because
    // this policy will only be used for downloading / decrypting.
    BlobEncryptionPolicy policy = new BlobEncryptionPolicy(null, cloudResolver);
    BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

    using (var np = File.Open(@"C:\data\MyFileDecrypted.txt", FileMode.Create))
        blob.DownloadToStream(np, null, options, null);


> [AZURE.NOTE] Istnieje kilka innych rodzajów rozpoznawania nazw w celu ułatwienia zarządzania kluczami, w tym: AggregateKeyResolver i CachingKeyResolver.


## <a name="use-key-vault-secrets"></a>Za pomocą hasła klucza magazynu
Sposobem tajne za pomocą szyfrowania po stronie klienta jest za pośrednictwem klasy SymmetricKey, ponieważ hasło jest zasadniczo symetrycznej klawisza. Ale jak wspomniano powyżej, hasła w klucza magazynu nie mapowanie dokładnie SymmetricKey. Istnieje kilka sposobów opis:


- Klucz w SymmetricKey musi być stałą długość: 128, 192, 256, 384 lub 512 bitów.
- Klucz w SymmetricKey powinny być zakodowany Base64.
- Klucz tajny magazynu klucz, który będzie używany jako SymmetricKey musi mieć typ zawartości "aplikacji oktet-strumienia" w klucza magazynu.

Oto przykład w programie PowerShell tworzenia hasła w magazynu klucz, który może być używany jako SymmetricKey.
Uwaga: Wartość ustalony $key, są tylko w celu pokaz. We własnym kodzie zapewne zechcesz do wygenerowania tego klucza.

    // Here we are making a 128-bit key so we have 16 characters.
    //  The characters are in the ASCII range of UTF8 so they are
    //  each 1 byte. 16 x 8 = 128.
    $key = "qwertyuiopasdfgh"
    $b = [System.Text.Encoding]::UTF8.GetBytes($key)
    $enc = [System.Convert]::ToBase64String($b)
    $secretvalue = ConvertTo-SecureString $enc -AsPlainText -Force

    // Substitute the VaultName and Name in this command.
    $secret = Set-AzureKeyVaultSecret -VaultName 'ContoseKeyVault' -Name 'TestSecret2' -SecretValue $secretvalue -ContentType "application/octet-stream"

W aplikacji konsoli samego połączenia jako przed służy do pobierania tego hasła jako SymmetricKey.

    SymmetricKey sec = (SymmetricKey) cloudResolver.ResolveKeyAsync(
        "https://contosokeyvault.vault.azure.net/secrets/TestSecret2/",
        CancellationToken.None).GetAwaiter().GetResult();

To wszystko. Ciesz się!

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji o korzystaniu z magazynem tabel platformy Microsoft Azure z C# zobacz [Biblioteka klienta programu Microsoft Azure miejsca do magazynowania dla środowiska .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).

Aby uzyskać więcej informacji na temat interfejsu API usługi REST obiektów Blob zobacz [Interfejsu API usługi REST usługi obiektów Blob](https://msdn.microsoft.com/library/azure/dd135733.aspx).

Aby uzyskać najnowsze informacje dotyczące magazyn Microsoft Azure przejdź do [Blogu zespołu Microsoft Azure miejsca do magazynowania](http://blogs.msdn.com/b/windowsazurestorage/).


<!--Image references-->
[1]: ./media/storage-encrypt-decrypt-blobs-key-vault/blobmetadata.png
