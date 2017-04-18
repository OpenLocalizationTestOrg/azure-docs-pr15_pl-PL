<properties
    pageTitle="Szyfrowania po stronie klienta z Python magazynu platformy Microsoft Azure | Microsoft Azure"
    description="Biblioteka Azure miejsca do magazynowania klienta do Python obsługuje szyfrowanie po stronie klienta, maksymalne bezpieczeństwo aplikacji magazyn Azure."
    services="storage"
    documentationCenter="python"
    authors="dineshmurthy"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="dineshm"/>


# <a name="client-side-encryption-with-python-for-microsoft-azure-storage"></a>Szyfrowania po stronie klienta z Python magazynu platformy Microsoft Azure

[AZURE.INCLUDE [storage-selector-client-side-encryption-include](../../includes/storage-selector-client-side-encryption-include.md)]

## <a name="overview"></a>Omówienie

[Biblioteka klienta Azure miejsca do magazynowania dla Python](https://pypi.python.org/pypi/azure-storage) obsługuje szyfrowanie danych w aplikacjach klienckich przed przekazaniem do magazynu Azure i odszyfrowywania danych podczas pobierania do klienta.

>[AZURE.NOTE] Biblioteka Azure Python miejsca do magazynowania jest w podglądzie.

## <a name="encryption-and-decryption-via-the-envelope-technique"></a>Szyfrowanie i odszyfrowywanie za pomocą techniki koperty
Procesy szyfrowania i odszyfrowywania wykonaj metoda koperty.

### <a name="encryption-via-the-envelope-technique"></a>Szyfrowanie za pomocą techniki koperty
Szyfrowanie za pomocą techniki koperty działa w następujący sposób:

1.  Biblioteka klienta Azure magazynowania generuje klucza szyfrowania zawartości (CEK), który jest klawiszem symetrycznej jednego jednorazowej użycia.

2.  Dane użytkownika są szyfrowane za pomocą tego CEK.

3.  CEK jest następnie zawinięty (szyfrowane) przy użyciu klucza szyfrowania (KEK). KEK jest identyfikowany za pomocą identyfikatora klucza i może być asymetrycznym pary kluczy lub klucz symetrycznej odbywa się lokalnie.
Biblioteka klienta miejsca do magazynowania, samej nigdy nie ma dostęp do KEK. Biblioteka wywołuje Algorytm zawijania klucza dostarczonego przez KEK. Użytkownicy mogą wybierać dostawców niestandardowych dla klucza zawijanie rozpakowaniu w razie potrzeby.

4.  Następnie przekazaniu zaszyfrowane dane z usługą Azure magazyn. Klucz zawiniętego oraz niektóre metadane dodatkowe szyfrowanie jest przechowywana jako metadane (na blob) albo interpolowana z zaszyfrowane dane (wiadomości w kolejce i jednostki tabeli).

### <a name="decryption-via-the-envelope-technique"></a>Odszyfrowywanie za pomocą techniki koperty
Odszyfrowywanie za pomocą techniki koperty sprawdza się w następujący sposób:

1.  Biblioteka klienta przyjęto założenie, że użytkownik jest lokalnie Zarządzanie kluczem szyfrowania klucza (KEK). Użytkownik nie musi wiedzieć określonego klucza, który był używany do szyfrowania. Zamiast tego klucza rozpoznawania nazw, który jest rozpoznawany jako różne identyfikatory klucza klawiszy, można skonfigurować i używane.

2.  Biblioteka klienta do pobrania zaszyfrowane dane oraz materiał szyfrowania, który jest przechowywany w usłudze.

3.  Klucz zawiniętego szyfrowania zawartości (CEK) jest następnie nieopakowanych (odszyfrowane) przy użyciu klucza szyfrowania klucza (KEK). W tym miejscu ponownie Biblioteka klienta nie ma dostępu do KEK. Wywołuje ją po prostu algorytm otwierania dostawcy niestandardowego.

4.  Kluczem szyfrowania zawartości (CEK) jest następnie używany do odszyfrowywania danych zaszyfrowanych użytkownika.

## <a name="encryption-mechanism"></a>Mechanizmu szyfrowania  
Biblioteka klienta magazynu używa [AES](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) szyfrowanie danych użytkownika. W szczególności [Łączenia blok szyfrowania (CBC)](http://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Cipher-block_chaining_.28CBC.29) trybu AES. Każdy usługi działa nieco inaczej, więc możemy przedstawimy każdego z nich w tym miejscu.

### <a name="blobs"></a>BLOB
Biblioteka klienta obecnie obsługuje szyfrowanie tylko całe obiektów blob. W szczególności szyfrowania są obsługiwane w przypadku użytkowników za pomocą **Tworzenie*** metod. Materiały do pobrania, oba ukończone i pobieranie zakres są obsługiwane i parallelization przekazywania i pobierania jest dostępny.

Podczas szyfrowania Biblioteka klienta wygenerować losową inicjowanie wektorowa IV 16 bajtów razem z kluczem szyfrowania zawartości (CEK) 32 bajtów i szyfrowania koperty obiektów blob danych za pomocą tych informacji. Zawiniętego CEK i metadanych pewne dodatkowe szyfrowanie zostaną następnie zapisane jako blob metadanych wraz z zaszyfrowaną obiektów blob usługi.

>[AZURE.WARNING] Edycji lub przekazywanie własnych metadanych dla obiektów blob, należy się upewnić, że te metadane są zachowywane. Po wysłaniu nowe metadane bez metadanych zawiniętego CEK, IV i inne metadane, zostaną utracone i zawartość obiektów blob nigdy nie będzie można jej pobrać.

Pobieranie zaszyfrowanych blob obejmuje pobierania zawartości całego obiektów blob za pomocą **uzyskać*** wygody metod. Zawiniętego CEK jest jako niezapakowany i używana razem z IV (przechowywane jako metadane obiektów blob w tym przypadku) zwraca odszyfrowane dane do użytkowników.

Pobieranie dowolnego zakresu (**Uzyskiwanie*** przekazany metod z parametrami zakresu) w zaszyfrowanej obiektów blob wymaga Dopasowywanie zakresu dostarczony przez użytkowników, aby można było uzyskiwać niewielki dodatkowe dane, które może służyć do pomyślnie odszyfrowywanie żądany zakres.

Blokowanie obiektów blob i obiektów blob strony tylko mogą być szyfrowane i odszyfrowywane za pomocą tego programu. Nie jest obecnie nie jest obsługiwane dla szyfrowania dołączanie obiektów blob.

### <a name="queues"></a>Kolejki
Ponieważ wiadomości w kolejce może mieć dowolne formatowanie, Biblioteka klienta określa format niestandardowy, zawierający wektor inicjowania (IV) i klucza szyfrowania zawartości szyfrowanego (CEK) w treści wiadomości.

Podczas szyfrowania Biblioteka klienta generuje losową IV 16 bajtów wraz z przypadkowe CEK 32 bajtów i wykonuje koperty szyfrowanie tekstu wiadomości kolejki za pomocą tych informacji. Zawiniętego CEK i metadanych pewne dodatkowe szyfrowanie następnie zostaną dodane do kolejki zaszyfrowane wiadomości. Ten komunikat zmienione (jak pokazano poniżej) są przechowywane w usłudze.

    <MessageText>{"EncryptedMessageContents":"6kOu8Rq1C3+M1QO4alKLmWthWXSmHV3mEfxBAgP9QGTU++MKn2uPq3t2UjF1DO6w","EncryptionData":{…}}</MessageText>

Podczas odszyfrowywania zawiniętego klucz jest wyodrębnionych w kolejce wiadomości i nie opakowanych. IV jest również wyodrębnionych z poziomu wiadomości z kolejki i używane wraz z klucz nieopakowanych odszyfrować danych kolejki wiadomości. Należy zauważyć, że metadane szyfrowania małych (w obszarze 500 bajtów), aby podczas jej uwzględniony w limicie 64KB wiadomości kolejki, wpływ powinny być zarządzane.

### <a name="tables"></a>Tabele
Biblioteka klienta obsługuje szyfrowanie właściwości obiektu do wstawiania i Zastąp operacji.

>[AZURE.NOTE] Scalanie nie jest obecnie obsługiwane. Ponieważ podzbiór właściwości mogą być zaszyfrowany wcześniej przy użyciu innego klucza, po prostu scalanie nowe właściwości i aktualizowanie metadanych spowoduje utratę danych. Scalanie albo wymaga nawiązywanie połączeń usługi dodatkowe odczytywanie istniejącego obiektu usługi lub przy użyciu nowego klucza na właściwości, które nie są odpowiednie ze względu na wydajność.

Szyfrowanie danych tabeli działa w następujący sposób:

1.  Użytkownicy określić właściwości, które mają być szyfrowane.

2.  Biblioteka klienta generuje losową inicjowanie wektorowa IV 16 bajtów wraz z klucza szyfrowania zawartości (CEK) 32 bajtów dla każdej jednostki i wykonuje szyfrowania koperty w poszczególnych właściwości były szyfrowane za wynikających z nowych IV na właściwość. Właściwość zaszyfrowanych jest przechowywana jako dane binarne.

3.  Zawiniętego CEK i niektóre metadane dodatkowe szyfrowanie następnie są przechowywane jako dwie dodatkowe właściwości zastrzeżone. Pierwszy zastrzeżone właściwości (\_ClientEncryptionMetadata1) jest właściwością ciągu, która zawiera informacje IV, wersję i zawiniętego klucza. Druga zastrzeżone właściwości (\_ClientEncryptionMetadata2) jest właściwością dwójkową, która zawiera informacje o właściwościach, które są zaszyfrowane. Informacje znajdujące się w tej właściwości drugiego (\_ClientEncryptionMetadata2) jest zaszyfrowany.

4.  Ze względu na następujące dodatkowe właściwości zastrzeżone wymagane na potrzeby szyfrowania użytkownicy mogą teraz mieć tylko 250 właściwości niestandardowe zamiast 252. Całkowity rozmiar jednostki musi być mniejszy niż 1MB.

    Należy zauważyć, że tylko ciąg właściwości można szyfrować. W przypadku innych typów właściwości szyfrowania, muszą zostać przekonwertowane na ciągi znaków. Zaszyfrowane ciągi są przechowywane w usłudze jako binarne właściwości i są one konwersji z powrotem na ciągi (ciągi nieprzetworzonych, nie EntityProperties typu EdmType.STRING) po odszyfrowywanie.

    W przypadku tabel należy oprócz zasady szyfrowania użytkownicy muszą określić właściwości, które mają być szyfrowane. To jest możliwe dzięki przechowywaniu tych właściwości w TableEntity obiektów z wartością typu EdmType.STRING i szyfrowania ma wartość true lub ustawienie encryption_resolver_function w obiekcie tableservice. Rozwiązywanie problemów z szyfrowania jest funkcję, która pobiera partition klucz, klucz wiersza i nazwa właściwości i zwraca wartość logiczną, która wskazuje, czy mają zostać zaszyfrowane tej właściwości. Podczas szyfrowania Biblioteka klienta użyje tych informacji zdecydować, czy właściwości mają zostać zaszyfrowane podczas zapisywania do sieci. Pełnomocnik także możliwość logiczny wokół jak właściwości są szyfrowane. (Na przykład jeśli argument X, to szyfrowanie właściwości A; w przeciwnym razie szyfrowanie właściwości, A i B.) Należy zauważyć, że nie jest zapewnienie te informacje podczas czytania i badania jednostki.

### <a name="batch-operations"></a>Operacje partii
Jedna zasada szyfrowania dotyczy wszystkich wierszy w partii. Biblioteka klienta wewnętrznie wygeneruje nowy losowych IV i CEK przekształcania przypadkowych pomysłów w wierszu w partii. Użytkownicy mogą wybierać również szyfrowanie różne właściwości dla każdej operacji w partii, definiując to zachowanie w rozpoznawania nazw szyfrowania.
Jeśli partia jest tworzona jako Menedżer kontekstu metodą batch() tableservice, zasady szyfrowania tableservice automatycznie zostanie zastosowany do partii. Jeśli partia tworzona jest jawnie przez wywoływanie konstruktora, zasady szyfrowania należy jako parametr przekazano i lewy za w niezmienionej postaci dla ważności partii.
Należy zauważyć, że jednostki są szyfrowane, jak są importowane partii za pomocą zasad szyfrowania partii (jednostki nie są szyfrowane w momencie zatwierdzeniem partii przy użyciu zasady szyfrowania tableservice).

### <a name="queries"></a>Kwerendy
Do wykonywania operacji kwerend, musisz określić klucza rozpoznawania nazw, który może rozpoznawać nazwy klawiszy w zestawie wyników. Jeśli jednostka zawarte w wyniku kwerendy nie powiedzie się z dostawcą, Biblioteka klienta będzie sygnalizować błąd. Dla dowolnej kwerendy, który wykonuje prognozy po stronie serwera, Biblioteka klienta spowoduje dodanie właściwości metadanych specjalne szyfrowania (\_ClientEncryptionMetadata1 i \_ClientEncryptionMetadata2) domyślnie zaznaczone kolumny.

>[AZURE.IMPORTANT] Należy pamiętać o następujących kwestiach podczas korzystania z szyfrowania po stronie klienta:
>
>- Podczas odczytu z lub zapisywania zaszyfrowanych blob, za pomocą całego obiektów blob przekazywania oraz zakres całkowitą obiektów blob pobierania poleceń. Unikanie zapisywania zaszyfrowanych obiektów blob przy użyciu protokołu operacji, takich jak umieścić blok, umieść listy zablokowanych, pisanie stron lub wyczyść strony; w przeciwnym razie możesz uszkodzona zaszyfrowanych obiektów blob i stał się nieczytelny.
>
>- W przypadku tabel należy istnieje ograniczenie podobne. Uważaj, aby nie aktualizowanie właściwości zaszyfrowane bez aktualizowania metadanych szyfrowania.
>
>- Po ustawieniu metadanych na zaszyfrowaną obiektów blob może zastąpić metadane związane z szyfrowania wymagane dla odszyfrowywanie, ponieważ ustawienie metadanych nie jest dodatek. Dotyczy to również migawek; Unikaj, określając metadanych podczas tworzenia migawki zaszyfrowanych obiektów blob. Jeśli można ustawić metadanych, pamiętaj wywołać metodę **get_blob_metadata** najpierw w celu uzyskania bieżący metadanych szyfrowania, a unikać równoczesne zapisu, gdy jest ustawiany metadanych.
>
>- Włącz flaga **require_encryption** obiektu usługi dla użytkowników, których należy działają tylko z zaszyfrowane dane. Aby uzyskać więcej informacji, zobacz poniżej.

Biblioteka klienta miejsca do magazynowania oczekuje dostarczonych KEK i klucza rozpoznawania nazw implementowania interfejsu. [Azure klucza magazynu](https://azure.microsoft.com/services/key-vault/) obsługę zarządzania Python KEK oczekuje i zintegrowany tej biblioteki po zakończeniu.


## <a name="client-api--interface"></a>Interfejs API klienta / interfejsu
Po utworzeniu obiektu usługi miejsca do magazynowania (to znaczy blockblobservice), użytkownik może przypisać wartości pola, które składają się zasady szyfrowania: key_encryption_key, key_resolver_function i require_encryption. Użytkownicy mogą podać tylko KEK tylko rozpoznawania nazw lub obie. key_encryption_key jest podstawowy typ klucza którą określono za pomocą identyfikatora klucza i udostępniająca logikę dla zawijanie rozpakowaniu. key_resolver_function jest używana do rozwiązywania klawisza podczas procesu odszyfrowywania. Zwraca prawidłową KEK podane identyfikatora klucza. To umożliwia użytkownikom wybranie wielu kluczy, które są zarządzane w kilku lokalizacjach.

KEK należy zaimplementować poniższych metod, aby pomyślnie szyfrowania danych:
- wrap_key(cek): był zawijany określonej CEK (bajtów), korzystając z algorytmu wybrany przez użytkownika. Zwraca klucz zawinięty.
- get_key_wrap_algorithm(): zwraca algorytm używany do zawijanie klawiszy.
- get_kid(): zwraca identyfikator klucza ciąg dla tego KEK.
KEK należy zaimplementować poniższych metod, aby pomyślnie odszyfrowywanie danych:
- unwrap_key (cek, algorytm): zwraca formularzu nieopakowanych CEK określony, przy użyciu algorytmu określony ciąg.
- get_kid(): zwraca ciąg identyfikatora klucza dla tego KEK.

Kluczowe rozpoznawania nazw co najmniej musi zaimplementować metodę, która podane identyfikator klucza zwraca odpowiednich KEK, implementacji interfejsu powyżej. Tylko ta metoda jest przydzielane do key_resolver_function właściwości obiektu usługi.

- Do szyfrowania jest on używany zawsze i Brak klucza spowoduje błąd.
- Do odszyfrowywania:
    - Kluczowe rozpoznawania nazw jest wywoływana, jeśli określony w celu uzyskania klucza. Jeśli program rozpoznawania nazw jest określony, ale nie ma mapowania dla identyfikatora klucza, jest generowany błąd.
    - Jeśli nie określono rozpoznawania nazw, ale określono klucz, klucz jest używany identyfikatora pasuje do wymaganego identyfikatora klucza. Jeśli identyfikator nie odpowiada, jest generowany błąd.

      Przykłady szyfrowania w azure.storage.samples <fix URL>wykazać scenariusza końcu do końca bardziej szczegółowe dla obiektów blob, kolejkach i tabel.
        Przykładowe implementacji KEK i klucza rozpoznawania nazw znajdują się w przykładowe pliki jako KeyWrapper i KeyResolver odpowiednio.

### <a name="requireencryption-mode"></a>Tryb RequireEncryption
Użytkownicy mogą opcjonalnie można włączyć tryb działania, w której musi być zaszyfrowany wszystkie operacje przekazywania i pobierania. W tym trybie próby dane bez zasadę szyfrowania przekazać lub pobrać dane, które nie są szyfrowane w usłudze zakończy się niepowodzeniem, na komputerze klienckim. Flaga **require_encryption** obiektu usługi kontrolki to zachowanie.

### <a name="blob-service-encryption"></a>Szyfrowanie usługi obiektów blob
Ustawić szyfrowania pola zasad blockblobservice obiektu. Wszystkie inne będzie są obsługiwane przez Biblioteka klienta wewnętrznie.

    # Create the KEK used for encryption.
    # KeyWrapper is the provided sample implementation, but the user may use their own object as long as it implements the interface above.
    kek = KeyWrapper('local:key1') # Key identifier

    # Create the key resolver used for decryption.
    # KeyResolver is the provided sample implementation, but the user may use whatever implementation they choose so long as the function set on the service object behaves appropriately.
    key_resolver = KeyResolver()
    key_resolver.put_key(kek)

    # Set the KEK and key resolver on the service object.
    my_block_blob_service.key_encryption_key = kek
    my_block_blob_service.key_resolver_funcion = key_resolver.resolve_key

    # Upload the encrypted contents to the blob.
    my_block_blob_service.create_blob_from_stream(container_name, blob_name, stream)

    # Download and decrypt the encrypted contents from the blob.
    blob = my_block_blob_service.get_blob_to_bytes(container_name, blob_name)

### <a name="queue-service-encryption"></a>Szyfrowanie usługi kolejki
Ustawić szyfrowania pola zasad queueservice obiektu. Wszystkie inne będzie są obsługiwane przez Biblioteka klienta wewnętrznie.

    # Create the KEK used for encryption.
    # KeyWrapper is the provided sample implementation, but the user may use their own object as long as it implements the interface above.
    kek = KeyWrapper('local:key1') # Key identifier

    # Create the key resolver used for decryption.
    # KeyResolver is the provided sample implementation, but the user may use whatever implementation they choose so long as the function set on the service object behaves appropriately.
    key_resolver = KeyResolver()
    key_resolver.put_key(kek)

    # Set the KEK and key resolver on the service object.
    my_queue_service.key_encryption_key = kek
    my_queue_service.key_resolver_funcion = key_resolver.resolve_key

    # Add message
    my_queue_service.put_message(queue_name, content)

    # Retrieve message
    retrieved_message_list = my_queue_service.get_messages(queue_name)

### <a name="table-service-encryption"></a>Szyfrowanie usługi tabeli
Oprócz tworzenia zasad szyfrowania i ustawienia opcji żądanie, musisz określić **encryption_resolver_function** na **tableservice**lub atrybut szyfrowanie na EntityProperty.

### <a name="using-the-resolver"></a>Przy użyciu rozpoznawania nazw

    # Create the KEK used for encryption.
    # KeyWrapper is the provided sample implementation, but the user may use their own object as long as it implements the interface above.
    kek = KeyWrapper('local:key1') # Key identifier

    # Create the key resolver used for decryption.
    # KeyResolver is the provided sample implementation, but the user may use whatever implementation they choose so long as the function set on the service object behaves appropriately.
    key_resolver = KeyResolver()
    key_resolver.put_key(kek)

    # Define the encryption resolver_function.
    def my_encryption_resolver(pk, rk, property_name):
            if property_name == 'foo':
                    return True
            return False

    # Set the KEK and key resolver on the service object.
    my_table_service.key_encryption_key = kek
    my_table_service.key_resolver_funcion = key_resolver.resolve_key
    my_table_service.encryption_resolver_function = my_encryption_resolver

    # Insert Entity
    my_table_service.insert_entity(table_name, entity)

    # Retrieve Entity
    # Note: No need to specify an encryption resolver for retrieve, but it is harmless to leave the property set.
    my_table_service.get_entity(table_name, entity['PartitionKey'], entity['RowKey'])

### <a name="using-attributes"></a>Przy użyciu atrybutów
Jak wcześniej wspomniano, właściwość może być oznaczony szyfrowania przechowywaniu go w obiekcie EntityProperty i ustawiając pole szyfrowanie.

    encrypted_property_1 = EntityProperty(EdmType.STRING, value, encrypt=True)

## <a name="encryption-and-performance"></a>Wydajność i szyfrowania
Należy zauważyć, że szyfrowania wyniki miejsca do magazynowania danych w dodatkowe obciążenie. Klucz zawartości i IV musi zostać wygenerowany, musi być zaszyfrowany zawartości i dodatkowe metadane musi być sformatowany i przekazane. Ten ogólnych różnią się w zależności od ilości danych są szyfrowane. Zaleca się, że klienci zawsze testują wydajności w czasie projektowania.

## <a name="next-steps"></a>Następne kroki

- Pobierz [Azure Biblioteka klienta miejsca do magazynowania dla pakietu PyPi języka Java](https://pypi.python.org/pypi/azure-storage)
- Pobierz [Biblioteka klienta Azure miejsca do magazynowania dla Python źródła z GitHub kodu](https://github.com/Azure/azure-storage-python)
