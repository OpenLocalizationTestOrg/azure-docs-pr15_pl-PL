<properties
    pageTitle="Jak używać magazyn kolejek z Python | Microsoft Azure"
    description="Dowiedz się, jak korzystać z usługi Azure kolejki Python do tworzenia i usuwania kolejki, wstawianie, pobieranie i usuwanie wiadomości."
    services="storage"
    documentationCenter="python"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="robinsh"/>

# <a name="how-to-use-queue-storage-from-python"></a>Jak używać magazyn kolejek z Python

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Omówienie

Ten przewodnik zawiera się, jak wykonywać typowe scenariusze przy użyciu usługi Azure kolejki miejsca do magazynowania. Przykłady są zapisane w Python i używanie [Microsoft Azure miejsca do magazynowania SDK dla Python]. Scenariusze, w których zawierać **wstawiania**, **Wgląd**, **Pobieranie**i **Usuwanie** wiadomości w kolejce, a także **Tworzenie i usuwanie kolejek**. Aby uzyskać więcej informacji o kolejkach można znaleźć w sekcji [następne kroki].

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="how-to-create-a-queue"></a>Jak: Tworzenie kolejki

Obiekt **QueueService** umożliwia pracę z kolejek. Poniższy kod tworzy obiekt **QueueService** . Dodaj następujący w górnej części dowolnego pliku Python, w którym chcesz programowy dostęp do magazynowania Azure:

    from azure.storage.queue import QueueService

Poniższy kod tworzy obiekt **QueueService** przy użyciu klucza konta i nazwę konta magazynu. Zamień "Moje konto" i "klucze" Nazwa konta i klucza.

    queue_service = QueueService(account_name='myaccount', account_key='mykey')

    queue_service.create_queue('taskqueue')


## <a name="how-to-insert-a-message-into-a-queue"></a>Jak: Wstawianie wiadomości w kolejce

Aby wstawić wiadomości w kolejce, użyj **umieszczenie\_wiadomości** metody, aby utworzyć nową wiadomość i dodać je do kolejki.

    queue_service.put_message('taskqueue', u'Hello World')


## <a name="how-to-peek-at-the-next-message"></a>Jak: Wglądu do następnej wiadomości

Czy wglądu do wiadomości pierwszych stronach kolejki, bez usuwania go z niej, dzwoniąc **wglądu\_wiadomości** metody. Domyślnie **wglądu\_wiadomości** wglądu do pojedynczej wiadomości.

    messages = queue_service.peek_messages('taskqueue')
    for message in messages:
        print(message.content)


## <a name="how-to-dequeue-messages"></a>Jak: Usuwania z kolejki wiadomości

Kod usuwa wiadomość z kolejki w dwóch krokach. Gdy połączenie telefoniczne **uzyskać\_wiadomości**, zostanie wyświetlony komunikat dalej w kolejce domyślnie. Zwrócone wiadomości **uzyskać\_wiadomości** staje się niewidoczne dla innych kodu do czytania wiadomości z tej kolejki. Domyślnie ta wiadomość pozostanie niewidoczne przez 30 sekund. Aby zakończyć usuwanie wiadomości z kolejki, należy również wywołać **Usuwanie\_wiadomości**. Opisanego niżej procesu usuwania wiadomości gwarantuje, że w przypadku niepowodzenia kodu przetwarzania wiadomości ze względu na błąd sprzętu i oprogramowania, inne wystąpienie kodu można wyświetlenie tego samego komunikatu i spróbuj ponownie. Połączenia kod **Usuwanie\_wiadomości** w prawo po przetworzeniu wiadomości.

    messages = queue_service.get_messages('taskqueue')
    for message in messages:
        print(message.content)
        queue_service.delete_message('taskqueue', message.id, message.pop_receipt)

Istnieją dwa sposoby można dostosować pobieranie wiadomości z kolejki.
Najpierw zostanie wyświetlony partię wiadomości (32). Drugi można ustawić invisibility wydłużyć lub skrócić przekroczenie limitu czasu, umożliwiając kodzie więcej lub mniej czasu na w pełni procesu każdej wiadomości. Poniższy kod przykładzie użyto **uzyskać\_wiadomości** metodę 16 wiadomości są pobierane za jednym połączenia. Następnie przetwarza każda wiadomość e-mail przy użyciu pętli for. Ustawia również invisibility przekroczenia limitu czasu, aby pięć minut dla każdej wiadomości.

    messages = queue_service.get_messages('taskqueue', num_messages=16, visibility_timeout=5*60)
    for message in messages:
        print(message.content)
        queue_service.delete_message('taskqueue', message.id, message.pop_receipt)      


## <a name="how-to-change-the-contents-of-a-queued-message"></a>Jak: Zmienianie zawartości wiadomość w kolejce

Możesz zmienić zawartość wiadomości w miejscu w kolejce. Jeśli wiadomość reprezentuje zadania, można tę funkcję aktualizacji stanu zadania. Poniższy zastosowania kod **Aktualizowanie\_wiadomości** metodę aktualizowania wiadomości. Widoczność przekroczenia limitu czasu jest ustawiona na 0, co oznacza natychmiast pojawi się komunikat i zawartość zostanie zaktualizowana.

    messages = queue_service.get_messages('taskqueue')
    for message in messages:
        queue_service.update_message('taskqueue', message.id, message.pop_receipt, 0, u'Hello World Again')

## <a name="how-to-get-the-queue-length"></a>Jak: Uzyskiwanie długość kolejki

Możesz uzyskać szacowana liczba wiadomości w kolejce. **Uzyskać\_kolejki\_metadanych** metoda zwróci się do usługi kolejki do zwracania metadanych dotyczących kolejki i **approximate_message_count**. Wynikiem jest tylko przybliżonego, ponieważ wiadomości można dodać lub usunąć po usługa kolejki odpowiada na wezwanie.

    metadata = queue_service.get_queue_metadata('taskqueue')
    count = metadata.approximate_message_count

## <a name="how-to-delete-a-queue"></a>Jak: Usuwanie kolejki

Aby usunąć kolejkę i wszystkie zawarte w niej wiadomości, zadzwoń do **Usuwanie\_kolejki** metody.

    queue_service.delete_queue('taskqueue')

## <a name="next-steps"></a>Następne kroki

Teraz, gdy znasz już podstawy miejsca w kolejce, skorzystaj z poniższych łączy, aby dowiedzieć się więcej.

- [Centrum deweloperów Python](/develop/python/)
- [Usługi Azure magazyn interfejsu API usługi REST](http://msdn.microsoft.com/library/azure/dd179355)
- [Blog zespołu Azure miejsca do magazynowania]
- [Magazyn Microsoft Azure SDK dla Python]

[Blog zespołu Azure miejsca do magazynowania]: http://blogs.msdn.com/b/windowsazurestorage/
[Magazyn Microsoft Azure SDK dla Python]: https://github.com/Azure/azure-storage-python
