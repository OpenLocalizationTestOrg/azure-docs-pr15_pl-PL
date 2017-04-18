<properties
    pageTitle="Szyfrowanie po stronie klienta przy użyciu .NET dla magazynu platformy Microsoft Azure | Microsoft Azure"
    description="Biblioteka klienta miejsca do magazynowania Azure dla środowiska .NET obsługuje szyfrowania po stronie klienta oraz integracja z magazynu klucza Azure maksymalne bezpieczeństwo aplikacji magazyn Azure."
    services="storage"
    documentationCenter=".net"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="robinsh"/>


# <a name="client-side-encryption-and-azure-key-vault-for-microsoft-azure-storage"></a>Magazyn klucza szyfrowania po stronie klienta i Azure magazynu platformy Microsoft Azure

[AZURE.INCLUDE [storage-selector-client-side-encryption-include](../../includes/storage-selector-client-side-encryption-include.md)]

## <a name="overview"></a>Omówienie

[Biblioteka klienta Azure miejsca do magazynowania dla pakietu .NET Nuget](https://www.nuget.org/packages/WindowsAzure.Storage) obsługuje szyfrowanie danych w aplikacjach klienckich przed przekazaniem do magazynu Azure i odszyfrowywania danych podczas pobierania do klienta. Biblioteki obsługuje także integracja z [Magazynu klucza Azure](https://azure.microsoft.com/services/key-vault/) dla zarządzania kluczami konta przestrzeni dyskowej.

Samouczek krok po kroku, który prowadzi Cię przez proces szyfrowania przy użyciu szyfrowania po stronie klienta i Azure klucza magazyn obiektów blob zobacz [Szyfrowanie i odszyfrowywanie obiektów blob w magazynie Azure firmy Microsoft przy użyciu magazynu klucza Azure](storage-encrypt-decrypt-blobs-key-vault.md).

Do szyfrowania po stronie klienta przy użyciu języka Java zobacz [Szyfrowanie po stronie klienta przy użyciu języka Java dla magazyn Microsoft Azure](storage-client-side-encryption-java.md).

## <a name="encryption-and-decryption-via-the-envelope-technique"></a>Szyfrowanie i odszyfrowywanie za pomocą techniki koperty

Procesy szyfrowania i odszyfrowywania wykonaj metoda koperty.

### <a name="encryption-via-the-envelope-technique"></a>Szyfrowanie za pomocą techniki koperty

Szyfrowanie za pomocą techniki koperty działa w następujący sposób:

1. Biblioteka klienta Azure magazynowania generuje klucza szyfrowania zawartości (CEK), który jest klawiszem symetrycznej jednego jednorazowej użycia.
2. Dane użytkownika są szyfrowane za pomocą tego CEK.
3. CEK jest następnie zawinięty (szyfrowane) przy użyciu klucza szyfrowania (KEK). KEK jest identyfikowany za pomocą identyfikatora klucza i można asymetrycznym pary kluczy lub symetrycznej klucza i można można zarządzać lokalnie lub przechowywane w magazynami klucza Azure.

    Biblioteka klienta miejsca do magazynowania, samej nigdy nie ma dostęp do KEK. Biblioteka wywołuje Algorytm zawijania klucza dostarczonego przez klucz magazynu. Użytkownicy mogą wybierać dostawców niestandardowych dla klucza zawijanie rozpakowaniu w razie potrzeby.

4. Następnie przekazaniu zaszyfrowane dane z usługą Azure magazyn. Klucz zawiniętego oraz niektóre metadane dodatkowe szyfrowanie jest przechowywana jako metadane (na blob) albo interpolowana z zaszyfrowane dane (wiadomości w kolejce i jednostki tabeli).

### <a name="decryption-via-the-envelope-technique"></a>Odszyfrowywanie za pomocą techniki koperty

Odszyfrowywanie za pomocą techniki koperty sprawdza się w następujący sposób:

1. Biblioteka klienta przyjęto założenie, użytkownik zarządza klucza szyfrowania (KEK) lokalnie lub magazynów klucza Azure. Użytkownik nie musi wiedzieć określonego klucza, który był używany do szyfrowania. Zamiast tego klucza rozpoznawania nazw, który jest rozpoznawany jako różne identyfikatory klucza klawiszy można skonfigurować i używane.
2. Biblioteka klienta do pobrania zaszyfrowane dane oraz materiał szyfrowania, który jest przechowywany w usłudze.
3. Klucz zawiniętego szyfrowania zawartości (CEK) jest następnie nieopakowanych (odszyfrowane) przy użyciu klucza szyfrowania klucza (KEK). W tym miejscu ponownie Biblioteka klienta nie ma dostępu do KEK. Wywołuje po prostu algorytm otwierania magazynu klucz dostawcę lub niestandardowe.
4. Kluczem szyfrowania zawartości (CEK) jest następnie używany do odszyfrowywania danych zaszyfrowanych użytkownika.

## <a name="encryption-mechanism"></a>Mechanizmu szyfrowania

Biblioteka klienta magazynu używa [AES](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) szyfrowanie danych użytkownika. W szczególności [Łączenia blok szyfrowania (CBC)](http://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Cipher-block_chaining_.28CBC.29) trybu AES. Każdy usługi działa nieco inaczej, każdy z nich w tym miejscu będzie omówimy.

### <a name="blobs"></a>BLOB

Biblioteka klienta obecnie obsługuje szyfrowanie tylko całe obiektów blob. W szczególności szyfrowania są obsługiwane w przypadku użytkowników za pomocą **UploadFrom** * metod lub * *OpenWrite** metody. Materiały do pobrania, oba ukończone i pobieranie zakres są obsługiwane.

Podczas szyfrowania Biblioteka klienta wygenerować losową inicjowanie wektorowa IV 16 bajtów razem z kluczem szyfrowania zawartości (CEK) 32 bajtów i szyfrowania koperty obiektów blob danych za pomocą tych informacji. Zawiniętego CEK i metadanych pewne dodatkowe szyfrowanie zostaną następnie zapisane jako blob metadanych wraz z zaszyfrowaną obiektów blob usługi.

> [AZURE.WARNING] Edycji lub przekazywanie własnych metadanych dla obiektów blob, należy się upewnić, że te metadane są zachowywane. Jeśli przekazujesz nowe metadane bez metadanych, zawiniętego CEK IV i inne metadane, zostaną utracone i zawartość obiektów blob nigdy nie będzie można jej pobrać.

Pobieranie zaszyfrowanych blob obejmuje pobierania zawartości całego obiektów blob przy użyciu **DownloadTo***/**BlobReadStream** wygody metod. Zawiniętego CEK jest jako niezapakowany i używana razem z IV (przechowywane jako metadane obiektów blob w tym przypadku) zwraca odszyfrowane dane do użytkowników.

Pobieranie dowolnego zakresu (**DownloadRange*** metody) w zaszyfrowanej obiektów blob wymaga Dopasowywanie zakresu dostarczony przez użytkowników, aby można było uzyskiwać niewielki dodatkowe dane, które może służyć do pomyślnie odszyfrowywanie żądany zakres.

Wszystkie blob typy (blokowanie obiektów blob, strony obiektów blob i dołączanie obiektów blob) można być szyfrowane i odszyfrowywane za pomocą tego programu.

### <a name="queues"></a>Kolejki

Ponieważ wiadomości w kolejce może mieć dowolne formatowanie, Biblioteka klienta określa format niestandardowy, zawierający wektor inicjowania (IV) i klucza szyfrowania zawartości szyfrowanego (CEK) w treści wiadomości.

Podczas szyfrowania Biblioteka klienta generuje losową IV 16 bajtów wraz z przypadkowe CEK 32 bajtów i wykonuje koperty szyfrowanie tekstu wiadomości kolejki za pomocą tych informacji. Zawiniętego CEK i metadanych pewne dodatkowe szyfrowanie następnie zostaną dodane do kolejki zaszyfrowane wiadomości. Ten komunikat zmienione (jak pokazano poniżej) są przechowywane w usłudze.

    <MessageText>{"EncryptedMessageContents":"6kOu8Rq1C3+M1QO4alKLmWthWXSmHV3mEfxBAgP9QGTU++MKn2uPq3t2UjF1DO6w","EncryptionData":{…}}</MessageText>

Podczas odszyfrowywania zawiniętego klucz jest wyodrębnionych w kolejce wiadomości i nie opakowanych. IV jest również wyodrębnionych z poziomu wiadomości z kolejki i używane wraz z klucz nieopakowanych odszyfrować danych kolejki wiadomości. Należy zauważyć, że metadane szyfrowania małych (w obszarze 500 bajtów), aby podczas jej uwzględniony w limicie 64KB wiadomości kolejki, wpływ powinny być zarządzane.

### <a name="tables"></a>Tabele

Biblioteka klienta obsługuje szyfrowanie właściwości obiektu do wstawiania i Zastąp operacji.

>[AZURE.NOTE] Scalanie nie jest obecnie obsługiwane. Ponieważ podzbiór właściwości mogą być zaszyfrowany wcześniej przy użyciu innego klucza, po prostu scalanie nowe właściwości i aktualizowanie metadanych spowoduje utratę danych. Scalanie albo wymaga nawiązywanie połączeń usługi dodatkowe odczytywanie istniejącego obiektu usługi lub przy użyciu nowego klucza na właściwości, które nie są odpowiednie ze względu na wydajność.

Szyfrowanie danych tabeli działa w następujący sposób:  

1. Użytkownicy określić właściwości, które mają być szyfrowane.
2. Biblioteka klienta generuje losową inicjowanie wektorowa IV 16 bajtów wraz z klucza szyfrowania zawartości (CEK) 32 bajtów dla każdej jednostki i wykonuje szyfrowania koperty w poszczególnych właściwości były szyfrowane za wynikających z nowych IV na właściwość. Właściwość zaszyfrowanych jest przechowywana jako dane binarne.
3. Zawiniętego CEK i niektóre metadane dodatkowe szyfrowanie następnie są przechowywane jako dwie dodatkowe właściwości zastrzeżone. Pierwszej właściwości zastrzeżone (_ClientEncryptionMetadata1) jest właściwością ciągu, która zawiera informacje IV, wersję i zawiniętego klucza. Właściwość drugi zastrzeżone (_ClientEncryptionMetadata2) jest właściwością binarnego, która zawiera informacje o właściwościach, które są zaszyfrowane. Informacje znajdujące się w tej właściwości drugiego (_ClientEncryptionMetadata2) jest zaszyfrowany.
4. Ze względu na następujące dodatkowe właściwości zastrzeżone wymagane na potrzeby szyfrowania użytkownicy mogą teraz mieć tylko 250 właściwości niestandardowe zamiast 252. Całkowity rozmiar jednostki musi być mniejszy niż 1 MB.

Należy zauważyć, że tylko ciąg właściwości można szyfrować. W przypadku innych typów właściwości szyfrowania, muszą zostać przekonwertowane na ciągi znaków. Zaszyfrowane ciągi są przechowywane w usłudze jako binarne właściwości i są one konwertowane na ciągi o po odszyfrowywanie.

W przypadku tabel należy oprócz zasady szyfrowania użytkownicy muszą określić właściwości, które mają być szyfrowane. Można to zrobić, określając albo atrybutu [EncryptProperty] (w przypadku jednostki POCO utworzone na podstawie TableEntity) lub szyfrowania rozpoznawania nazw w opcjach wezwanie. Rozwiązywanie problemów z szyfrowania jest pełnomocnika, która pobiera partition klucz, klucz wiersza i nazwa właściwości i zwraca wartość logiczną, która wskazuje, czy właściwości mają zostać zaszyfrowane. Podczas szyfrowania Biblioteka klienta użyje tych informacji zdecydować, czy właściwości mają zostać zaszyfrowane podczas zapisywania do sieci. Pełnomocnik także możliwość logiczny wokół jak właściwości są szyfrowane. (Na przykład jeśli argument X, to szyfrowanie właściwości A; w przeciwnym razie szyfrowanie właściwości, A i B.) Należy zauważyć, że nie jest zapewnienie te informacje podczas czytania i badania jednostki.

### <a name="batch-operations"></a>Operacje partii

W operacji partię tym samym KEK będą używane przez wszystkich wierszy w tej operacji partii ponieważ biblioteka klienta umożliwia tylko jeden obiekt opcje (i w związku z tym jeden zasad KEK) dla operacji partii. Jednak biblioteka klienta wewnętrznie wygeneruje nowy losowych IV i CEK przekształcania przypadkowych pomysłów w wierszu w partii. Użytkownicy mogą wybierać również szyfrowanie różne właściwości dla każdej operacji w partii, definiując to zachowanie w rozpoznawania nazw szyfrowania.

### <a name="queries"></a>Kwerendy

Do wykonywania operacji kwerend, musisz określić klucza rozpoznawania nazw, który może rozpoznawać nazwy klawiszy w zestawie wyników. Jeśli jednostka zawarte w wyniku kwerendy nie powiedzie się z dostawcą, Biblioteka klienta będzie sygnalizować błąd. Dla dowolnej kwerendy, który wykonuje prognozy po stronie serwera Biblioteka klienta spowoduje dodanie właściwości metadanych specjalne szyfrowania (_ClientEncryptionMetadata1 i _ClientEncryptionMetadata2) domyślnie zaznaczone kolumny.

## <a name="azure-key-vault"></a>Azure klucza magazynu

Azure magazynu klucza ułatwia ochronę informacji klucze szyfrowania i hasła używane przez aplikacje w chmurze i usługi. Przy użyciu magazynu klucza Azure, użytkownicy mogą szyfrować klucze i hasła (na przykład kluczy uwierzytelniania, klawiszy konta przestrzeni dyskowej, klucze szyfrowania danych. Pliki PFX i hasła) za pomocą klawiszy, które są chronione moduły sprzętu (HSM). Aby uzyskać więcej informacji, zobacz [Co to jest Azure klucza magazynu?](../key-vault/key-vault-whatis.md).

Biblioteka klienta magazynu używa biblioteki magazynu klucza podstawowego w celu udostępniania wspólne ramy przez Azure zarządzania kluczami. Użytkownicy również uzyskać dodatkowe korzyści z używania biblioteki rozszerzenia magazynu klucza. Biblioteka rozszerzeń znajdują się przydatne funkcje wokół proste i bezproblemowe Symmetric-RSA lokalne i dostawców klucza w chmurze, a także z agregacji i pamięci podręcznej.

### <a name="interface-and-dependencies"></a>Interfejs i zależności

Istnieją trzy pakietów klucza magazynu:

- Microsoft.Azure.KeyVault.Core zawiera IKey i IKeyResolver. To małe opakowania z żadnych zależności. Biblioteka klienta miejsca do magazynowania dla środowiska .NET definiuje go jako zależność.
- Microsoft.Azure.KeyVault zawiera klienta klucza magazynu pozostałych.
- Microsoft.Azure.KeyVault.Extensions zawiera kod wewnętrzny, który zawiera implementacji algorytmów szyfrowania i RSAKey SymmetricKey. W zależności od nazw podstawowych i KeyVault, a funkcje do definiowania agregacji rozpoznawania nazw (gdy użytkownicy chcą korzystać z wielu dostawców klucza) i buforowania kluczy rozpoznawania nazw. Mimo że biblioteka klienta przestrzeni dyskowej nie bezpośrednio zależą od tego pakietu, jeśli użytkownicy chcą korzystać Azure klucza magazynu, do przechowywania klucze albo za pomocą rozszerzenia klucza magazynu używają lokalnej i w chmurze cryptographic dostawców, muszą ten pakiet.

Klucz magazynu jest przeznaczony dla wysokiej wartości kluczy głównych i ograniczania ograniczenia na klucz magazynu zaprojektowano z tym pamiętać. Podczas wykonywania szyfrowania po stronie klienta z magazynu klucza, modelem preferowanym jest klawisze symetrycznej wzorzec przechowywane jako hasła klucza magazynu i przechowywanych w pamięci podręcznej lokalnie. Użytkownicy, należy wykonać następujące czynności:

1. Tworzenie hasła w trybie offline i przekaż go do magazynu klucza.
2. Za pomocą identyfikatora podstawowej tego hasła jako parametr rozwiązać bieżącej wersji tego hasła szyfrowania i pamięci podręcznej te informacje lokalnie. Używanie CachingKeyResolver w pamięci podręcznej; Użytkownicy nie mają zaimplementować własne buforowanie logiczny.
3. Użyj buforowania rozpoznawania nazw jako dane wejściowe podczas tworzenia zasady szyfrowania.

Więcej informacji na temat zastosowania klucza magazynu można znaleźć w [Przykłady kodu szyfrowania](https://github.com/Azure/azure-storage-net/tree/master/Samples/GettingStarted/EncryptionSamples).

## <a name="best-practices"></a>Najważniejsze wskazówki

Obsługa szyfrowania jest dostępna tylko w bibliotece klienta miejsca do magazynowania dla środowiska .NET. Windows Phone i obsługi Windows aktualnie nie obsługuje szyfrowania.

>[AZURE.IMPORTANT] Należy pamiętać o następujących kwestiach podczas korzystania z szyfrowania po stronie klienta:
>
>- Podczas odczytu z lub zapisywania zaszyfrowanych blob, za pomocą całego obiektów blob przekazywania oraz zakres całkowitą obiektów blob pobierania poleceń. Unikanie zapisywania zaszyfrowanych obiektów blob przy użyciu protokołu operacji, takich jak umieścić blok, umieść listy zablokowanych, pisanie stron, wyczyść strony lub dołączanie bloku; w przeciwnym razie możesz uszkodzona zaszyfrowanych obiektów blob i stał się nieczytelny.
>- W przypadku tabel należy istnieje ograniczenie podobne. Uważaj, aby nie aktualizowanie właściwości zaszyfrowane bez aktualizowania metadanych szyfrowania.
>- Po ustawieniu metadanych na zaszyfrowaną obiektów blob może zastąpić metadane związane z szyfrowania wymagane dla odszyfrowywanie, ponieważ ustawienie metadanych nie jest dodatek. Dotyczy to również migawek; Unikaj, określając metadanych podczas tworzenia migawki zaszyfrowanych obiektów blob. Jeśli można ustawić metadanych, pamiętaj wywołać metodę **FetchAttributes** najpierw w celu uzyskania bieżący metadanych szyfrowania, a unikać równoczesne zapisu, gdy jest ustawiany metadanych.
>- Włączanie właściwości **RequireEncryption** w opcji żądanie domyślnych dla użytkowników, których należy działają tylko z zaszyfrowane dane. Aby uzyskać więcej informacji, zobacz poniżej.


## <a name="client-api--interface"></a>Interfejs API klienta / interfejsu

Podczas tworzenia obiektu EncryptionPolicy, użytkownicy mogą podać tylko klawisza (implementacji IKey), tylko program rozpoznawania nazw (implementacji IKeyResolver) lub oba. IKey jest podstawowy typ klucza którą określono za pomocą identyfikatora klucza i udostępniająca logikę dla zawijanie rozpakowaniu. IKeyResolver jest używana do rozwiązywania klawisza podczas procesu odszyfrowywania. Definiuje metodę ResolveKey, która zwraca IKey podane identyfikatora klucza. To umożliwia użytkownikom wybranie wielu kluczy, które są zarządzane w kilku lokalizacjach.

- Do szyfrowania jest on używany zawsze i Brak klucza spowoduje błąd.
- Do odszyfrowywania:
    - Jeśli określono do uzyskania klucza wywoływana jest klucza rozpoznawania nazw. Jeśli program rozpoznawania nazw jest określony, ale nie ma mapowania dla identyfikatora klucza, jest generowany błąd.
    - Jeśli nie określono rozpoznawania nazw, ale określono klucz, klucz jest używany identyfikatora pasuje do wymaganego identyfikatora klucza. Jeśli identyfikator nie odpowiada, jest generowany błąd.

[Przykłady szyfrowania](https://github.com/Azure/azure-storage-net/tree/master/Samples/GettingStarted/EncryptionSamples) pokazano bardziej szczegółowe scenariusz zakończenia do końca dla obiektów blob, kolejki i tabele, wraz z magazynu klucza integracji.

### <a name="requireencryption-mode"></a>Tryb RequireEncryption

Użytkownicy mogą opcjonalnie można włączyć tryb działania, w której musi być zaszyfrowany wszystkie operacje przekazywania i pobierania. W tym trybie próby dane bez zasadę szyfrowania przekazać lub pobrać dane, które nie są szyfrowane w usłudze zakończy się niepowodzeniem, na komputerze klienckim. Właściwość **RequireEncryption** obiektu opcje żądania kontrolki to zachowanie. Jeśli aplikacja są szyfrowane wszystkie obiekty przechowywane w magazynie Azure, można ustawić właściwość **RequireEncryption** na żądanie domyślne dla obiektu klienta usługi. Na przykład ustaw **CloudBlobClient.DefaultRequestOptions.RequireEncryption** **PRAWDA** pozycji Wymagaj szyfrowania dla wszystkich obiektów blob wykonywany za pośrednictwem tego obiektu klienta.

### <a name="blob-service-encryption"></a>Szyfrowanie usługi obiektów blob

Tworzenie obiektu **BlobEncryptionPolicy** i ustaw ją w opcjach żądanie (na interfejsu API lub na poziomie klienta przy użyciu **DefaultRequestOptions**). Wszystkie inne będzie są obsługiwane przez Biblioteka klienta wewnętrznie.

    // Create the IKey used for encryption.
    RsaKey key = new RsaKey("private:key1" /* key identifier */);

    // Create the encryption policy to be used for upload and download.
    BlobEncryptionPolicy policy = new BlobEncryptionPolicy(key, null);

    // Set the encryption policy on the request options.
    BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

    // Upload the encrypted contents to the blob.
    blob.UploadFromStream(stream, size, null, options, null);

    // Download and decrypt the encrypted contents from the blob.
    MemoryStream outputStream = new MemoryStream();
    blob.DownloadToStream(outputStream, null, options, null);

### <a name="queue-service-encryption"></a>Szyfrowanie usługi kolejki

Tworzenie obiektu **QueueEncryptionPolicy** i ustaw ją w opcjach żądanie (na interfejsu API lub na poziomie klienta przy użyciu **DefaultRequestOptions**). Wszystkie inne będzie są obsługiwane przez Biblioteka klienta wewnętrznie.


    // Create the IKey used for encryption.
    RsaKey key = new RsaKey("private:key1" /* key identifier */);

    // Create the encryption policy to be used for upload and download.
    QueueEncryptionPolicy policy = new QueueEncryptionPolicy(key, null);

    // Add message
    QueueRequestOptions options = new QueueRequestOptions() { EncryptionPolicy = policy };
    queue.AddMessage(message, null, null, options, null);

    // Retrieve message
    CloudQueueMessage retrMessage = queue.GetMessage(null, options, null);

### <a name="table-service-encryption"></a>Szyfrowanie usługi tabeli

Oprócz tworzenia zasad szyfrowania i ustawienia opcji żądanie, musisz określić **EncryptionResolver** w **TableRequestOptions**lub atrybut [EncryptProperty] w jednostce.

#### <a name="using-the-resolver"></a>Przy użyciu rozpoznawania nazw


    // Create the IKey used for encryption.
    RsaKey key = new RsaKey("private:key1" /* key identifier */);

    // Create the encryption policy to be used for upload and download.
    TableEncryptionPolicy policy = new TableEncryptionPolicy(key, null);

    TableRequestOptions options = new TableRequestOptions()
    {
        EncryptionResolver = (pk, rk, propName) =>
        {
            if (propName == "foo")
            {
                return true;
            }
            return false;
        },
        EncryptionPolicy = policy
    };

    // Insert Entity
    currentTable.Execute(TableOperation.Insert(ent), options, null);

    // Retrieve Entity
    // No need to specify an encryption resolver for retrieve
    TableRequestOptions retrieveOptions = new TableRequestOptions()
    {
        EncryptionPolicy = policy
    };

    TableOperation operation = TableOperation.Retrieve(ent.PartitionKey, ent.RowKey);
    TableResult result = currentTable.Execute(operation, retrieveOptions, null);

#### <a name="using-attributes"></a>Przy użyciu atrybutów

Jak wcześniej wspomniano, jeśli jednostka wykonuje TableEntity, właściwości można udekorowanego z atrybutem [EncryptProperty] zamiast określanie **EncryptionResolver**.

    [EncryptProperty]
    public string EncryptedProperty1 { get; set; }

## <a name="encryption-and-performance"></a>Wydajność i szyfrowania

Należy zauważyć, że szyfrowania wyniki miejsca do magazynowania danych w dodatkowe obciążenie. Klucz zawartości i IV musi zostać wygenerowany, musi być zaszyfrowany zawartości i dodatkowe metadane musi być sformatowany i przekazane. Ten ogólnych różnią się w zależności od ilości danych są szyfrowane. Zaleca się, że klienci zawsze testują wydajności w czasie projektowania.

## <a name="next-steps"></a>Następne kroki

- [Samouczek: Szyfrowania i odszyfrowywania obiektów blob w magazynie Azure firmy Microsoft przy użyciu magazynu klucza Azure](storage-encrypt-decrypt-blobs-key-vault.md)
- Pobierz [Azure Biblioteka klienta miejsca do magazynowania dla pakietu .NET NuGet](https://www.nuget.org/packages/WindowsAzure.Storage)
- Pobieranie pakietów Azure klucza magazynu NuGet [podstawowych](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Core/), [klientów](http://www.nuget.org/packages/Microsoft.Azure.KeyVault/)i [rozszerzenia](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Extensions/)  
- Odwiedź stronę [Azure klucza magazynu dokumentacji](../key-vault/key-vault-whatis.md)

