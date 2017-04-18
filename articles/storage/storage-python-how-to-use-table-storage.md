<properties
    pageTitle="Jak używać magazyn tabel z Python | Microsoft Azure"
    description="Przechowywanie danych strukturalnych w chmurze przy użyciu magazyn tabel platformy Azure, magazynu danych NoSQL."
    services="storage"
    documentationCenter="python"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="how-to-use-table-storage-from-python"></a>Jak używać magazyn tabel z Python

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Omówienie

Ten przewodnik pokazano, jak wykonywać typowe scenariusze za pomocą usługi Magazyn tabel Azure. Przykłady są zapisane w Python i używanie [Microsoft Azure miejsca do magazynowania SDK dla Python]. Scenariusze objęte obejmują, tworzenie i usuwanie tabeli, oprócz wstawiania i wykonywanie zapytań za podmioty w tabeli.

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-table"></a>Tworzenie tabeli

Obiekt **TableService** umożliwia pracę z usługami tabeli. Poniższy kod tworzy obiekt **TableService** . Dodawanie kodu u góry dowolnego pliku Python, w którym chcesz programowy dostęp do magazynowania Azure:

    from azure.storage.table import TableService, Entity

Poniższy kod tworzy obiekt **TableService** przy użyciu klucz konta i nazwę konta miejsca do magazynowania.  Zamień "Moje konto" i "klucze" Nazwa konta i klucza.

    table_service = TableService(account_name='myaccount', account_key='mykey')

    table_service.create_table('tasktable')

## <a name="add-an-entity-to-a-table"></a>Dodawanie obiektu do tabeli

Aby dodać jednostki, należy najpierw utworzyć słownika lub jednostki, który definiuje wartości i nazw właściwości obiektu. Należy zauważyć, że dla każdego obiektu, musisz określić **PartitionKey** i **RowKey**. Są to unikatowe identyfikatory jednostki. W kwerendach można używać tych wartości dużo szybciej niż można wysyłać kwerendy do innych właściwości. System używa **PartitionKey** , aby automatycznie przekazywać podmioty tabeli przez wiele węzłów magazynu.
Obiektów, które mają samej **PartitionKey** są przechowywane w tym samym węźle. **RowKey** jest unikatowy identyfikator obiektu w obrębie partycją należący do.

Aby dodać obiekt do tabeli, przekazać obiekt słownika **Wstawianie\_jednostki** metody.

    task = {'PartitionKey': 'tasksSeattle', 'RowKey': '1', 'description' : 'Take out the trash', 'priority' : 200}
    table_service.insert_entity('tasktable', task)

Można również przekazać wystąpienie klasy **obiektu** do **Wstawianie\_jednostki** metody.

    task = Entity()
    task.PartitionKey = 'tasksSeattle'
    task.RowKey = '2'
    task.description = 'Wash the car'
    task.priority = 100
    table_service.insert_entity('tasktable', task)

## <a name="update-an-entity"></a>Aktualizowanie obiektu

Kod pokazano, jak zastąpić zaktualizowanej wersji starszej wersji istniejącego obiektu.

    task = {'PartitionKey': 'tasksSeattle', 'RowKey': '1', 'description' : 'Take out the garbage', 'priority' : 250}
    table_service.update_entity('tasktable', 'tasksSeattle', '1', task, content_type='application/atom+xml')

Jeśli jednostki, który jest aktualizowany nie istnieje, nie będą operacji aktualizacji. Jeśli chcesz przechowywać jednostka niezależnie od tego, czy istniała przed, za pomocą **Wstawianie\_lub\_replace_entity**.
W poniższym przykładzie pierwsze wywołanie spowoduje zastąpienie istniejącego obiektu. Drugie wywołanie wstawi nowy obiekt od nie jednostka o określonej **PartitionKey** i **RowKey** istnieje w tabeli.

    task = {'PartitionKey': 'tasksSeattle', 'RowKey': '1', 'description' : 'Take out the garbage again', 'priority' : 250}
    table_service.insert_or_replace_entity('tasktable', 'tasksSeattle', '1', task, content_type='application/atom+xml')

    task = {'PartitionKey': 'tasksSeattle', 'RowKey': '3', 'description' : 'Buy detergent', 'priority' : 300}
    table_service.insert_or_replace_entity('tasktable', 'tasksSeattle', '1', task, content_type='application/atom+xml')

## <a name="change-a-group-of-entities"></a>Zmienianie grupy jednostek

Czasem warto przesyłanie wielu operacji razem w partii zapewnienie Atomowej przetwarzania przez serwer. Aby wykonać, aby użyć klasy **TableBatch** . Jeśli chcesz przesłać partii, połączeń **Zatwierdź\_partii**. Zauważ, że wszystkie obiekty muszą znajdować się w partycją tym samym celu można zmienić jako partię. W poniższym przykładzie dodaje dwie jednostki razem w serii.

    from azure.storage.table import TableBatch
    batch = TableBatch()
    task10 = {'PartitionKey': 'tasksSeattle', 'RowKey': '10', 'description' : 'Go grocery shopping', 'priority' : 400}
    task11 = {'PartitionKey': 'tasksSeattle', 'RowKey': '11', 'description' : 'Clean the bathroom', 'priority' : 100}
    batch.insert_entity(task10)
    batch.insert_entity(task11)
    table_service.commit_batch('tasktable', batch)

Można także partie o składni Menedżera contex:

    task12 = {'PartitionKey': 'tasksSeattle', 'RowKey': '12', 'description' : 'Go grocery shopping', 'priority' : 400}
    task13 = {'PartitionKey': 'tasksSeattle', 'RowKey': '13', 'description' : 'Clean the bathroom', 'priority' : 100}

    with table_service.batch('tasktable') as batch:
        batch.insert_entity(task12)
        batch.insert_entity(task13)


## <a name="query-for-an-entity"></a>Kwerenda obiektu

Aby wykonać kwerendę jednostki w tabeli, użyj **uzyskać\_jednostki** metody przekazując **PartitionKey** i **RowKey**.

    task = table_service.get_entity('tasktable', 'tasksSeattle', '1')
    print(task.description)
    print(task.priority)

## <a name="query-a-set-of-entities"></a>Kwerendy zestaw jednostek

W tym przykładzie umożliwia znalezienie wszystkich zadań w Seattle w zależności od **PartitionKey**.

    tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'")
    for task in tasks:
        print(task.description)
        print(task.priority)

## <a name="query-a-subset-of-entity-properties"></a>Kwerendy podzbiór właściwości obiektu

Kwerendy do tabeli można pobrać kilka właściwości z obiektu.
Ta metoda, o nazwie *rzut*zmniejsza przepustowość i zwiększyć wydajność kwerendy, szczególnie w przypadku dużych obiektów. Użyty parametr **Wybierz** i przebiegu nazwy właściwości, które chcesz przełączyć do klienta.

Kwerenda poniższy kod zwraca tylko opisy jednostek w tabeli.

[AZURE.NOTE] Poniższy fragment działa tylko w odniesieniu do usługi przechowywania w chmurze. To nie jest obsługiwana przez emulator miejsca do magazynowania.

    tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'", select='description')
    for task in tasks:
        print(task.description)

## <a name="delete-an-entity"></a>Usuwanie obiektu

Jednostka można usunąć za pomocą kluczem partycją i wiersza.

    table_service.delete_entity('tasktable', 'tasksSeattle', '1')

## <a name="delete-a-table"></a>Usuwanie tabeli

Poniższy kod powoduje usunięcie tabeli z konta miejsca do magazynowania.

    table_service.delete_table('tasktable')

## <a name="next-steps"></a>Następne kroki

Teraz, gdy znasz już podstawy miejsca w tabeli, skorzystaj z poniższych łączy, aby dowiedzieć się więcej.

- [Centrum deweloperów Python](/develop/python/)
- [Usługi Azure magazyn interfejsu API usługi REST](http://msdn.microsoft.com/library/azure/dd179355)
- [Blog zespołu Azure miejsca do magazynowania]
- [Magazyn Microsoft Azure SDK dla Python]

[Azure blogu zespołu miejsca do magazynowania]: http://blogs.msdn.com/b/windowsazurestorage/
[Magazyn Microsoft Azure SDK dla Python]: https://github.com/Azure/azure-storage-python
