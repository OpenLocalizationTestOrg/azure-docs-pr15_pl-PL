<properties
    pageTitle="Jak używać magazyn tabel platformy Azure z dopiskiem | Microsoft Azure"
    description="Przechowywanie danych strukturalnych w chmurze przy użyciu magazyn tabel platformy Azure, magazynu danych NoSQL."
    services="storage"
    documentationCenter="ruby"
    authors="tamram"
    manager="carmonm"
    editor=""/>
<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="ruby"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="how-to-use-azure-table-storage-from-ruby"></a>Jak używać magazyn tabel platformy Azure z dopiskiem

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Omówienie

Ten przewodnik zawiera się, jak wykonywać typowe scenariusze korzystania z usługi Azure tabeli. Przykłady są zapisywane, za pomocą interfejsu API dopiskiem. Scenariusze, objęta obejmują, **Tworzenie i usuwanie tabeli, wstawianie i wykonywanie zapytań za podmioty w tabeli**.

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a>Tworzenie aplikacji dopiskiem fonetycznym

Aby uzyskać instrukcje dotyczące tworzenia dopiskiem fonetycznym aplikacji, zobacz [dopiskiem w aplikacji sieci Web szyn na maszyn wirtualnych Azure](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).


## <a name="configure-your-application-to-access-storage"></a>Konfigurowanie aplikacji, aby uzyskać dostęp do miejsca do magazynowania

Aby korzystać z magazynu Azure, musisz pobrać i skorzystać z dopiskiem fonetycznym pakietu azure, który oferuje zestaw wygody bibliotek, które komunikowanie się za pomocą usługi REST miejsca do magazynowania.

### <a name="use-rubygems-to-obtain-the-package"></a>Umożliwia uzyskanie pakietu RubyGems

1. Za pomocą interfejsu wiersza polecenia **programu PowerShell** (Windows), **Terminal** (Mac) lub **urodzinową** (Unix).

2. W oknie polecenia do zainstalowania gem i zależności, wpisz **gem zainstalować azure** .

### <a name="import-the-package"></a>Importowanie pakietu

Za pomocą edytora tekstu Ulubione, Dodaj następujący tekst na początek pliku dopiskiem fonetycznym miejsce, w którym mają być używane miejsca do magazynowania:

    require "azure"

## <a name="set-up-an-azure-storage-connection"></a>Konfigurowanie połączenia magazyn Azure

Moduł azure odczyta zmienne środowiska **AZURE\_miejsca do magazynowania\_konta** i **AZURE\_miejsca do magazynowania\_dostępu\_klucza** dla informacje wymagane do nawiązywania połączenia z kontem magazyn Azure. Jeśli nie ustawiono zmienne środowiska, należy określić informacje o koncie przed **Azure::TableService** za pomocą następującego kodu:

    Azure.config.storage_account_name = "<your azure storage account>"
    Azure.config.storage_access_key = "<your azure storage access key>"

Aby uzyskać te wartości z konta magazynu Menedżera zasobów w Azure Portal lub klasyczny:

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

## <a name="create-a-table"></a>Tworzenie tabeli

Obiekt **Azure::TableService** pozwala pracować z tabelami i jednostki. Aby utworzyć tabelę, należy użyć **Tworzenie\_table()** metody. Poniższy przykład tworzy tabelę lub wydrukować komunikat o błędzie, jeśli występuje dowolny.

    azure_table_service = Azure::TableService.new
    begin
      azure_table_service.create_table("testtable")
    rescue
      puts $!
    end

## <a name="add-an-entity-to-a-table"></a>Dodawanie obiektu do tabeli

Aby dodać jednostki, należy najpierw utworzyć obiektu skrótu, który definiuje właściwości obiektu. Należy zauważyć, że dla każdej jednostki, musisz określić **PartitionKey** i **RowKey**. Te są unikatowe identyfikatory jednostki i wartości, które można wyszukiwać znacznie szybciej niż inne właściwości. Magazyn Azure używa **PartitionKey** , aby automatycznie przekazywać podmioty tabeli przez wiele węzłów magazynu. Podmioty o tym samym **PartitionKey** są przechowywane w tym samym węźle. **RowKey** jest unikatowy identyfikator obiektu w obrębie partition, do której należy.

    entity = { "content" => "test entity",
      :PartitionKey => "test-partition-key", :RowKey => "1" }
    azure_table_service.insert_entity("testtable", entity)

## <a name="update-an-entity"></a>Aktualizowanie obiektu

Istnieje wiele metod do aktualizacji istniejącego obiektu:

* **Aktualizowanie\_entity():** Aktualizowanie istniejącego obiektu, zastępując go.
* **korespondencji seryjnej\_entity():** Aktualizuje istniejącego obiektu przez scalanie nowe wartości właściwości istniejącego obiektu.
* **Wstaw\_lub\_korespondencji seryjnej\_entity():** Aktualizowanie istniejącego obiektu, zamieniając go. Jeśli jednostka nie istnieje, zostanie wstawiony nowy:
* **Wstaw\_lub\_zamienić\_entity():** Aktualizuje istniejącego obiektu przez scalanie nowe wartości właściwości istniejącego obiektu. Jeśli jednostka nie istnieje, zostanie wstawiony nowy.

W poniższym przykładzie pokazano aktualizowanie za pomocą jednostki **Aktualizowanie\_entity()**:

    entity = { "content" => "test entity with updated content",
      :PartitionKey => "test-partition-key", :RowKey => "1" }
    azure_table_service.update_entity("testtable", entity)

Z **Aktualizowanie\_entity()** i **korespondencji seryjnej\_entity()**, jeśli jednostki, który chcesz zaktualizować nie istnieje, a następnie operacji aktualizacji zakończy się niepowodzeniem. W związku z tym chcesz przechowywać jednostka niezależnie od tego, czy już istnieje, należy w zamian użyć **Wstawianie\_lub\_zamienić\_entity()** lub **Wstawianie\_lub\_korespondencji seryjnej\_entity()**.

## <a name="work-with-groups-of-entities"></a>Praca z grupami jednostek

Czasem warto przesyłanie wielu operacji razem w partii zapewnienie Atomowej przetwarzania przez serwer. Celu, który należy najpierw utworzyć obiekt **partii** a następnie użyj **wykonywanie\_batch()** metoda **TableService**. Poniższy przykład ilustruje przesyłanie dwa podmioty RowKey 2 i 3 w serii. Należy zauważyć, że działa tylko dla obiektów z tym samym PartitionKey.

    azure_table_service = Azure::TableService.new
    batch = Azure::Storage::Table::Batch.new("testtable",
      "test-partition-key") do
      insert "2", { "content" => "new content 2" }
      insert "3", { "content" => "new content 3" }
    end
    results = azure_table_service.execute_batch(batch)

## <a name="query-for-an-entity"></a>Kwerenda obiektu

Aby wykonać kwerendę jednostki w tabeli, użyj **uzyskać\_entity()** metody, przekazując nazwę tabeli, **PartitionKey** i **RowKey**.

    result = azure_table_service.get_entity("testtable", "test-partition-key",
      "1")

## <a name="query-a-set-of-entities"></a>Kwerendy zestaw jednostek

Aby wykonać kwerendę zestawu obiektów w tabeli, kwerendy obiektu skrótu panelami **kwerendy\_entities()** metody. W poniższym przykładzie pokazano wprowadzenie wszystkich jednostek z tym samym **PartitionKey**:

    query = { :filter => "PartitionKey eq 'test-partition-key'" }
    result, token = azure_table_service.query_entities("testtable", query)

> [AZURE.NOTE] Jeśli zestaw wyników jest zbyt duża dla pojedynczej kwerendy zwraca token kontynuacji zostaną zwrócone służącego do pobierania kolejnych stron.

## <a name="query-a-subset-of-entity-properties"></a>Kwerendy podzbiór właściwości obiektu

Kwerendy do tabeli można pobrać kilka właściwości z obiektu. Ta metoda, o nazwie "rzut" zmniejsza przepustowość i zwiększyć wydajność kwerendy, szczególnie w przypadku dużych obiektów. Za pomocą klauzuli select i przebiegu nazwy właściwości, które chcesz przenieść klienta.

    query = { :filter => "PartitionKey eq 'test-partition-key'",
      :select => ["content"] }
    result, token = azure_table_service.query_entities("testtable", query)

## <a name="delete-an-entity"></a>Usuwanie obiektu

Aby usunąć jednostkę, należy użyć **Usuwanie\_entity()** metody. Należy przekazać Nazwa tabeli, która zawiera jednostkę PartitionKey i RowKey jednostki.

        azure_table_service.delete_entity("testtable", "test-partition-key", "1")

## <a name="delete-a-table"></a>Usuwanie tabeli

Aby usunąć tabelę, należy użyć **Usuwanie\_table()** metody i przekazania Nazwa tabeli, który chcesz usunąć.

        azure_table_service.delete_table("testtable")

## <a name="next-steps"></a>Następne kroki

Aby poznać bardziej złożone zadania miejsca do magazynowania, skorzystaj z poniższych łączy:

- [Blog zespołu Azure miejsca do magazynowania](http://blogs.msdn.com/b/windowsazurestorage/)
- Repozytorium [SDK Azure dla dopiskiem](http://github.com/WindowsAzure/azure-sdk-for-ruby) w GitHub
