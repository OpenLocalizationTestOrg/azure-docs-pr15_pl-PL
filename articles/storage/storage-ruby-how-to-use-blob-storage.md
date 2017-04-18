<properties
    pageTitle="Jak używać magazyn obiektów Blob (miejsca przechowywania obiektu) z dopiskiem | Microsoft Azure"
    description="Przechowywanie danych niestrukturalne w chmurze z magazynem obiektów Blob platformy Azure (miejsca przechowywania obiektu)."
    services="storage"
    documentationCenter="ruby"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="ruby"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="tamram"/>


# <a name="how-to-use-blob-storage-from-ruby"></a>Jak używać magazyn obiektów Blob z dopiskiem

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Omówienie

Magazyn obiektów Blob platformy Azure to usługa, która przechowuje niestrukturalne dane w chmurze jako obiekty i obiektów blob. Magazyn obiektów blob mogą zawierać dowolnego typu tekst lub dane binarne, takich jak dokument, plik lub Instalator aplikacji. Magazyn obiektów blob jest również określane jako miejsca przechowywania obiektu.

Ten przewodnik pojawi się, jak wykonywać typowe scenariusze przy użyciu magazyn obiektów Blob. Przykłady są zapisywane, za pomocą interfejsu API dopiskiem. Scenariusze, objęta obejmują **przekazywanie, wyświetlanie, pobierania danych** i **Usuwanie** obiektów blob.

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a>Tworzenie aplikacji dopiskiem fonetycznym

Tworzenie aplikacji dopiskiem fonetycznym. Aby uzyskać instrukcje zobacz [dopiskiem w aplikacji sieci Web szyn na maszyn wirtualnych Azure](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md)

## <a name="configure-your-application-to-access-storage"></a>Konfigurowanie aplikacji, aby uzyskać dostęp do miejsca do magazynowania

Aby korzystać z magazynu Azure, należy pobieranie i używanie dopiskiem fonetycznym pakiet azure zawiera zestaw wygody bibliotek, które komunikowanie się za pomocą usługi REST miejsca do magazynowania.

### <a name="use-rubygems-to-obtain-the-package"></a>Umożliwia uzyskanie pakietu RubyGems

1. Za pomocą interfejsu wiersza polecenia **programu PowerShell** (Windows), **Terminal** (Mac) lub **urodzinową** (Unix).

2. Wpisz "gem instalacji azure" w oknie polecenia, aby zainstalować gem i zależności.

### <a name="import-the-package"></a>Importowanie pakietu

Za pomocą edytora tekstu Ulubione, Dodaj następujący tekst na początek pliku dopiskiem fonetycznym miejsce, w którym mają być używane miejsca do magazynowania:

    require "azure"

## <a name="setup-an-azure-storage-connection"></a>Konfigurowanie połączenia Azure miejsca do magazynowania

Moduł azure odczyta zmienne środowiska **AZURE\_miejsca do magazynowania\_konta** i **AZURE\_miejsca do magazynowania\_ACCESS_KEY** dla informacje wymagane do nawiązywania połączenia z kontem Azure miejsca do magazynowania. Jeśli nie ustawiono zmienne środowiska, należy określić informacje o koncie przed **Azure::Blob::BlobService** za pomocą następującego kodu:

    Azure.config.storage_account_name = "<your azure storage account>"
    Azure.config.storage_access_key = "<your azure storage access key>"


Aby uzyskać te wartości z konta magazynu Menedżera zasobów w portalu Azure lub klasyczny:

1. Zaloguj się do [portalu Azure](https://portal.azure.com).
2. Przejdź do konta miejsca do magazynowania, dla którego chcesz użyć.
3. W karta Ustawienia po prawej stronie kliknij przycisk **Klawiszy dostępu**.
4. Na kartę klawisze dostępu, która jest wyświetlana zostaną wyświetlone klawisz dostępu 1 i klawisz dostępu 2. Można użyć jednego z tych. 
5. Kliknij ikonę Kopiuj, aby skopiować klucz do Schowka. 

Aby uzyskać te wartości z konta klasyczny miejsca do magazynowania w klasycznym portal Azure:

1. Zaloguj się do [Klasyczny portal Azure](https://manage.windowsazure.com).
2. Przejdź do konta miejsca do magazynowania, dla którego chcesz użyć.
3. Kliknij przycisk **ZARZĄDZAJ klawiszy dostępu** w dolnej części okienka nawigacji.
4. W wyświetlonym okno dialogowe zobaczysz nazwę konta magazynu, klucz podstawowy dostęp i klucz awaryjny dostęp. Klucz dostępu można użyć główną i pomocniczą jeden. 
5. Kliknij ikonę Kopiuj, aby skopiować klucz do Schowka.

## <a name="create-a-container"></a>Tworzenie kontenera

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

Obiekt **Azure::Blob::BlobService** umożliwia pracę z kontenerów i obiektów blob. Aby utworzyć kontener, użyj **Tworzenie\_container()** metody.

W poniższym przykładzie tworzy kontener lub wydrukować komunikat o błędzie, jeśli występuje dowolny.

    azure_blob_service = Azure::Blob::BlobService.new
    begin
      container = azure_blob_service.create_container("test-container")
    rescue
      puts $!
    end

Jeśli chcesz pliki w kontenerze publicznie, można ustawić uprawnienia kontenera.

Można modyfikować <strong>Tworzenie\_container()</strong> połączenia w celu przekazania **: publicznej\_dostępu\_poziom** opcję:

    container = azure_blob_service.create_container("test-container",
      :public_access_level => "<public access level>")


Prawidłowe wartości parametru **: publicznej\_dostępu\_poziom** są opcji:

* **obiektów blob:** Określa publicznej dostęp do odczytu dla kontenerów i obiektów blob danych. Klientów można wyliczyć obiektów blob w kontenerze przy użyciu anonimowego żądania, ale nie można wyliczyć kontenerów w ramach konta miejsca do magazynowania.

* **kontenera:** Określa publiczny dostęp do odczytu dla obiektów blob. Obiektów blob danych w tym kontenerze można odczytywać przy użyciu anonimowego żądania, ale kontenera dane nie są dostępne. Klienci nie można wyliczyć obiektów blob w kontenerze przy użyciu anonimowego żądania.

Można też kliknąć, można zmodyfikować poziom dostępu publicznego kontenera za pomocą **ustawić\_kontenera\_acl()** metodę, aby określić poziom dostępu publicznego.

W poniższym przykładzie zmienia poziom dostępu publicznego do **kontenera**:

    azure_blob_service.set_container_acl('test-container', "container")

## <a name="upload-a-blob-into-a-container"></a>Przekazywanie obiektów blob do kontenera

Aby przekazać zawartość do obiektów blob, użyj **Tworzenie\_blok\_blob()** metodę w celu utworzenia obiektów blob, za pomocą pliku lub ciągu jako zawartość obiektów blob.

Poniższy kod spowoduje przekazanie pliku **test.png** jako nowy obiektów blob o nazwie "obraz-obiektów blob" w kontenerze.

    content = File.open("test.png", "rb") { |file| file.read }
    blob = azure_blob_service.create_block_blob(container.name,
      "image-blob", content)
    puts blob.name

## <a name="list-the-blobs-in-a-container"></a>Lista obiektów blob w kontenerze

Aby wyświetlić listę kontenerów, użyj metody **list_containers()** .
Aby wyświetlić listę obiektów blob znajdujące się w kontenerze, użyj **listy\_blobs()** metody.

Wyświetla to adresy URL wszystkich obiektów blob w kontenerach dla konta.

    containers = azure_blob_service.list_containers()
    containers.each do |container|
      blobs = azure_blob_service.list_blobs(container.name)
      blobs.each do |blob|
        puts blob.name
      end
    end

## <a name="download-blobs"></a>Pobieranie obiektów blob

Aby pobrać obiektów blob, użyj **uzyskać\_blob()** metoda pobierania zawartości.

W poniższym przykładzie pokazano przy użyciu **uzyskać\_blob()** pobrać zawartość "obiektów blob obrazu" i zapisz je w pliku lokalnego.

    blob, content = azure_blob_service.get_blob(container.name,"image-blob")
    File.open("download.png","wb") {|f| f.write(content)}

## <a name="delete-a-blob"></a>Usuwanie obiektów Blob
Na koniec, aby usunąć obiektów blob, należy użyć **Usuwanie\_blob()** metody. W poniższym przykładzie pokazano, jak usunąć obiektów blob.

    azure_blob_service.delete_blob(container.name, "image-blob")

## <a name="next-steps"></a>Następne kroki

Aby poznać bardziej złożone zadania miejsca do magazynowania, skorzystaj z poniższych łączy:

- [Blog zespołu Azure miejsca do magazynowania](http://blogs.msdn.com/b/windowsazurestorage/)
- Repozytorium [SDK Azure dla dopiskiem](https://github.com/WindowsAzure/azure-sdk-for-ruby) w GitHub
- [Przesyłanie danych za pomocą narzędzia wiersza polecenia AzCopy](storage-use-azcopy.md)
