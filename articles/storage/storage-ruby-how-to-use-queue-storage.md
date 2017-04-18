<properties 
    pageTitle="Jak używać magazyn kolejek z dopiskiem | Microsoft Azure" 
    description="Dowiedz się, jak korzystać z usługi Azure kolejki do tworzenia i usuwania kolejki, wstawianie, pobieranie i usuwanie wiadomości. Próbki napisana dopiskiem." 
    services="storage" 
    documentationCenter="ruby" 
    authors="robinsh" 
    manager="carmonm" 
    editor=""/>

<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ruby" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="robinsh"/>


# <a name="how-to-use-queue-storage-from-ruby"></a>Jak używać magazyn kolejek z dopiskiem

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Omówienie

Ten przewodnik zawiera się, jak wykonywać typowe scenariusze przy użyciu usługi Microsoft Azure kolejki miejsca do magazynowania. Przykłady są zapisywane, za pomocą interfejsu API Azure dopiskiem.
Scenariusze, w których zawierać **wstawiania**, **Wgląd**, **Pobieranie**i **Usuwanie** wiadomości w kolejce, a także **Tworzenie i usuwanie kolejek**.

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a>Tworzenie aplikacji dopiskiem fonetycznym

Tworzenie aplikacji dopiskiem fonetycznym. Aby uzyskać instrukcje zobacz [dopiskiem w aplikacji sieci Web szyn na maszyn wirtualnych Azure](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).

## <a name="configure-your-application-to-access-storage"></a>Konfigurowanie aplikacji uzyskać dostęp do magazynu

Aby korzystać z magazynu Azure, należy pobieranie i używanie dopiskiem fonetycznym pakiet azure zawiera zestaw wygody bibliotek, które komunikowanie się za pomocą usługi REST miejsca do magazynowania.

### <a name="use-rubygems-to-obtain-the-package"></a>Umożliwia uzyskanie pakietu RubyGems

1. Za pomocą interfejsu wiersza polecenia **programu PowerShell** (Windows), **Terminal** (Mac) lub **urodzinową** (Unix).

2. Wpisz "gem instalacji azure" w oknie polecenia, aby zainstalować gem i zależności.

### <a name="import-the-package"></a>Importowanie pakietu

Za pomocą edytora tekstu Ulubione, Dodaj następujący tekst na początek pliku dopiskiem fonetycznym miejsce, w którym mają być używane miejsca do magazynowania:

    require "azure"

## <a name="setup-an-azure-storage-connection"></a>Konfigurowanie połączenia Azure miejsca do magazynowania

Moduł azure odczyta zmienne środowiska **AZURE\_miejsca do magazynowania\_konta** i **AZURE\_miejsca do magazynowania\_ACCESS_KEY** dla informacje wymagane do nawiązywania połączenia z kontem Azure miejsca do magazynowania. Jeśli nie ustawiono zmienne środowiska, należy określić informacje o koncie przed **Azure::QueueService** za pomocą następującego kodu:

    Azure.config.storage_account_name = "<your azure storage account>"
    Azure.config.storage_access_key = "<your Azure storage access key>"

 
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

## <a name="how-to-create-a-queue"></a>Jak: Tworzenie kolejki

Poniższy kod tworzy obiektu **Azure::QueueService** , który umożliwia pracę z kolejek.

    azure_queue_service = Azure::QueueService.new

Użyj metody **create_queue()** utworzyć kolejkę o określonej nazwie.

    begin
      azure_queue_service.create_queue("test-queue")
    rescue
      puts $!
    end

## <a name="how-to-insert-a-message-into-a-queue"></a>Jak: Wstawianie wiadomości w kolejce

Aby wstawić wiadomości w kolejce, użyj metody **create_message()** , aby utworzyć nową wiadomość i dodać je do kolejki.

    azure_queue_service.create_message("test-queue", "test message")

## <a name="how-to-peek-at-the-next-message"></a>Jak: Wglądu do następnej wiadomości

Czy wglądu do wiadomości pierwszych stronach kolejki, bez usuwania go z niej, dzwoniąc **wglądu\_messages()** metody. Domyślnie **wglądu\_messages()** wglądu do pojedynczej wiadomości. Można również określić liczbę wiadomości mają być wgląd.

    result = azure_queue_service.peek_messages("test-queue",
      {:number_of_messages => 10})

## <a name="how-to-dequeue-the-next-message"></a>Jak: Usuwania z kolejki następnej wiadomości

Możesz usunąć wiadomość z kolejki w dwóch krokach.

1. Gdy połączenie telefoniczne **listy\_messages()**, zostanie wyświetlony komunikat dalej w kolejce domyślnie. Można również określić liczbę wiadomości, które chcesz otrzymywać. Komunikaty zwracane z **listy\_messages()** staje się niewidoczne dla innych kodu do czytania wiadomości z tej kolejki. Należy przekazać w polu Limit czasu widoczność w sekundach jako parametru.

2. Aby zakończyć, usuwając wiadomość z kolejki, możesz również wywołać **delete_message()**.

Opisanego niżej procesu usuwania wiadomości gwarantuje, że w przypadku niepowodzenia kodu przetwarzania wiadomości ze względu na błąd sprzętu i oprogramowania, inne wystąpienie kodu można wyświetlenie tego samego komunikatu i spróbuj ponownie. Połączenia kod **Usuwanie\_message()** w prawo po przetworzeniu wiadomości.

    messages = azure_queue_service.list_messages("test-queue", 30)
    azure_queue_service.delete_message("test-queue", 
      messages[0].id, messages[0].pop_receipt)

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Jak: Zmienianie zawartości wiadomość w kolejce

Możesz zmienić zawartość wiadomości w miejscu w kolejce. Kod poniżej aktualizuje za pomocą metody **update_message()** do wiadomości. Metoda zwróci krotki zawierający pop otrzymania wiadomości kolejki i UTC wartość daty i godziny przedstawiającą gdy wiadomość będzie widoczny w kolejce.

    message = azure_queue_service.list_messages("test-queue", 30)
    pop_receipt, time_next_visible = azure_queue_service.update_message(
      "test-queue", message.id, message.pop_receipt, "updated test message", 
      30)

## <a name="how-to-additional-options-for-dequeuing-messages"></a>Jak: Dodatkowe opcje usuwania wiadomości

Istnieją dwa sposoby można dostosować pobieranie wiadomości z kolejki.

1. Możesz uzyskać partię wiadomości.

2. Można ustawić invisibility wydłużyć lub skrócić przekroczenie limitu czasu, umożliwiając kodzie więcej lub mniej czasu na w pełni procesu każdej wiadomości.

Poniższy kod przykładzie użyto **listy\_messages()** sposobem 15 wiadomości są pobierane za jednym połączenia. Następnie umożliwia drukowanie i usuwa każdej wiadomości. Ustawia również invisibility przekroczenia limitu czasu, aby pięć minut dla każdej wiadomości.

    azure_queue_service.list_messages("test-queue", 300
      {:number_of_messages => 15}).each do |m|
      puts m.message_text
      azure_queue_service.delete_message("test-queue", m.id, m.pop_receipt)
    end

## <a name="how-to-get-the-queue-length"></a>Jak: Uzyskiwanie długość kolejki

Możesz wyświetlić oszacowanie liczba wiadomości w kolejce. **Uzyskać\_kolejki\_metadata()** metoda zwróci się do usługi kolejki zwraca liczbę przybliżonego wiadomości i metadanych o kolejce.

    message_count, metadata = azure_queue_service.get_queue_metadata(
      "test-queue")

## <a name="how-to-delete-a-queue"></a>Jak: Usuwanie kolejki

Aby usunąć kolejkę i wszystkie zawarte w niej wiadomości, zadzwoń do **Usuwanie\_queue()** metoda obiektu kolejki.

    azure_queue_service.delete_queue("test-queue")

## <a name="next-steps"></a>Następne kroki

Teraz, gdy znasz już podstawy miejsca w kolejce, skorzystaj z poniższych łączy, aby uzyskać informacje o bardziej złożone zadania miejsca do magazynowania.

- Odwiedź [Blog zespołu Azure miejsca do magazynowania](http://blogs.msdn.com/b/windowsazurestorage/)
- Odwiedź repozytorium [SDK Azure dla dopiskiem](https://github.com/WindowsAzure/azure-sdk-for-ruby) GitHub

Do porównania usługa kolejki Azure omawiane w niniejszym artykule oraz kolejek Bus usług Azure omawiane w artykule [jak za pomocą kolejek Bus usług](/develop/ruby/how-to-guides/service-bus-queues/) zobacz [kolejki Azure i kolejek Bus usług — porównanie i Contrasted](../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md)
 
