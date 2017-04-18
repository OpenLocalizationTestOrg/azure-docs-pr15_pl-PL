<properties
    pageTitle="Jak używać usługi Bus kolejek z dopiskiem | Microsoft Azure"
    description="Dowiedz się, jak używać usługi Bus kolejek w Azure. Przykłady kodu napisanego w dopiskiem."
    services="service-bus"
    documentationCenter="ruby"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="ruby"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-queues"></a>Jak używać usługi Bus kolejki

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Ten przewodnik informacje dotyczące używania usługi Bus kolejek. Przykłady są zapisane w dopiskiem i używanie Azure gem. Scenariusze, w których obejmują **Tworzenie kolejek, wysyłanie i odbieranie wiadomości**i **Usuwanie kolejek**. Aby uzyskać więcej informacji o kolejkach Bus usługi zobacz sekcję [Następne kroki](#next-steps) .

## <a name="what-are-service-bus-queues"></a>Co to są usługi Bus kolejki?

Usługa Bus kolejki obsługują modelu komunikacji *brokered wiadomości* . Dzięki kolejki części aplikacji rozproszonej nie komunikować się bezpośrednio ze sobą; Zamiast tego ich wymiany wiadomości za pomocą kolejki pośredniczącym. Producent wiadomości (nadawcy) ręce wyłączyć wiadomości do kolejki i będzie nadal ich przetwarzania.
Asynchroniczne wiadomości dla klientów indywidualnych (słuchawka) pobiera wiadomości z kolejki i przetwarza je. Producent nie ma czekać na odpowiedzi od konsumenta, aby kontynuować przetwarzanie i wysłać kolejnych wiadomości. Kolejki oferują **pierwszy wchodzi, pierwszy się (FIFO)** dostarczenia wiadomości dla jednego lub więcej konkurencyjnych konsumentów. Oznacza to, że wiadomości są zwykle odbierane i przetwarzane przez odbiorców w kolejności, w jakiej zostały dodane do kolejki, a każda wiadomość jest odbierane i przetwarzane przez konsumenta tylko jedną wiadomość.

![QueueConcepts](./media/service-bus-ruby-how-to-use-queues/sb-queues-08.png)

Usługa Bus kolejki są ogólnego zastosowania technologii, który może być używany dla wielu różnych scenariuszach:

-   Komunikacji między sieci web i pracownik ról w [wielu warstwa Azure aplikacji](service-bus-dotnet-multi-tier-app-using-service-bus-queues.md).
-   Komunikacji między aplikacjami lokalnego i Azure hostowana aplikacje w [rozwiązania hybrydowego](service-bus-dotnet-hybrid-app-using-service-bus-relay.md).
-   Komunikacji między składnikami aplikacji rozproszonej uruchomionego lokalnego w innych organizacji lub działów organizacji.

Korzystanie z kolejek można umożliwiają skalowania lepszej aplikacji i włączyć więcej elastyczności architektury.

## <a name="create-a-namespace"></a>Tworzenie obszaru nazw

Aby rozpocząć korzystanie z usługi Bus kolejek platformy Azure, musisz najpierw utworzyć przestrzeń nazw. Przestrzeń nazw zawiera kontenerze obejmującym adresowania Bus usługi zasobów w aplikacji. Należy utworzyć nazw za pośrednictwem interfejsu wiersza polecenia, ponieważ Azure portal nie tworzenie nazw z połączeniem ACS.

Aby utworzyć przestrzeń nazw:

1. Otwieranie konsoli programu Azure Powershell.

2. Wpisz następujące polecenie, aby utworzyć nazw Bus usługi. Udostępnia własną wartość nazw i określić tego samego regionu jako aplikacja.

    ```
    New-AzureSBNamespace -Name 'yourexamplenamespace' -Location 'West US' -NamespaceType 'Messaging' -CreateACSNamespace $true

    ![Create Namespace](./media/service-bus-ruby-how-to-use-queues/showcmdcreate.png)
    ```

## <a name="obtain-management-credentials-for-the-namespace"></a>Uzyskaj poświadczenia zarządzania obszaru nazw

W celu wykonywania operacji zarządzania, takich jak tworzenie kolejki na nowy obszar nazw, musisz uzyskać poświadczenia zarządzania obszaru nazw.

Polecenia cmdlet programu PowerShell, wykonanego tworzenie nazw bus usługi Azure zostanie wyświetlony klucz, który służy do zarządzania obszarem nazw. Skopiuj wartość **DefaultKey** . W dalszej części tego samouczka użyje tej wartości w kodzie.

![Kopiowanie klucza](./media/service-bus-ruby-how-to-use-queues/defaultkey.png)

> [AZURE.NOTE] Można również znaleźć ten klucz, jeśli zalogować się do [portalu Azure](https://portal.azure.com/) i przejść do informacji o połączeniu obszaru nazw Bus usługi.

## <a name="create-a-ruby-application"></a>Tworzenie aplikacji dopiskiem fonetycznym

Tworzenie aplikacji dopiskiem fonetycznym. Aby uzyskać instrukcje zobacz [Tworzenie aplikacji sieci dopiskiem Azure](/develop/ruby/tutorials/web-app-with-linux-vm/).

## <a name="configure-your-application-to-use-service-bus"></a>Konfigurowanie aplikacji za pomocą usługi Bus

Aby użyć Bus usługi Azure, Pobierz i użycie funkcji Spakuj dopiskiem Azure, zawiera zestaw wygody bibliotek, które komunikowanie się za pomocą usługi REST miejsca do magazynowania.

### <a name="use-rubygems-to-obtain-the-package"></a>Umożliwia uzyskanie pakietu RubyGems

1. Za pomocą interfejsu wiersza polecenia **programu PowerShell** (Windows), **Terminal** (Mac) lub **urodzinową** (Unix).

2. Wpisz "gem instalacji azure" w oknie polecenia, aby zainstalować gem i zależności.

### <a name="import-the-package"></a>Importowanie pakietu

Za pomocą edytora tekstu Ulubione, Dodaj następujący tekst na początek pliku dopiskiem fonetycznym miejsce, w którym mają być używane miejsca do magazynowania:

```
require "azure"
```

## <a name="set-up-an-azure-service-bus-connection"></a>Konfigurowanie połączenia Bus usługi Azure

Moduł Azure odczytuje zmienne środowiska **AZURE\_SERVICEBUS\_nazw** i **AZURE\_SERVICEBUS\_ACCESS_KEY** dla informacje wymagane do połączenia do obszaru nazw Bus usługi. Jeśli nie ustawiono zmienne środowiska, należy określić informacje nazw przed **Azure::ServiceBusService** za pomocą następującego kodu:

```
Azure.config.sb_namespace = "<your azure service bus namespace>"
Azure.config.sb_access_key = "<your azure service bus access key>"
```

Ustaw wartość nazw na wartość, która została utworzona, a nie cały adres URL. Na przykład za pomocą **"yourexamplenamespace"**, nie "yourexamplenamespace.servicebus.windows.net".

## <a name="how-to-create-a-queue"></a>Jak utworzyć kolejki

Obiekt **Azure::ServiceBusService** umożliwia pracę z kolejek. Aby utworzyć kolejkę, użyj metody **create_queue()** . Poniższy przykład tworzy kolejkę lub błędy do drukowania.

```
azure_service_bus_service = Azure::ServiceBusService.new
begin
  queue = azure_service_bus_service.create_queue("test-queue")
rescue
  puts $!
end
```

Można również przekazać obiektu **Azure::ServiceBus::Queue** dodatkowe opcje, który umożliwia zastąpienie wartości kolejki domyślnych, takich jak czas wygaśnięcia wiadomości lub maksymalny rozmiar kolejki. W poniższym przykładzie pokazano, jak ustawić maksymalny rozmiar kolejki 5GB i godzina live na 1 minutę:

```
queue = Azure::ServiceBus::Queue.new("test-queue")
queue.max_size_in_megabytes = 5120
queue.default_message_time_to_live = "PT1M"

queue = azure_service_bus_service.create_queue(queue)
```

## <a name="how-to-send-messages-to-a-queue"></a>Jak wysyłać wiadomości do kolejki

Aby wysłać wiadomość do kolejki Bus usługi połączeń aplikacji **Wysyłanie\_kolejki\_message()** metody obiektu **Azure::ServiceBusService** . Wiadomości wysłane do (i odebrane od) kolejki Bus usługi obiektów **Azure::ServiceBus::BrokeredMessage** i mają pewien zestaw właściwości standardowe (taki jak **Etykieta** i **czas\_do\_live**), słownik, który służy do przechowywania niestandardowej aplikacji określone właściwości i treści danych dowolnego aplikacji. Aplikacji można ustawić w treści wiadomości, przekazując wartość ciągu jako wiadomości i wymagane właściwości standardowe zostanie wypełniona wartości domyślne.

W poniższym przykładzie pokazano, jak wysłać wiadomość testową do kolejki o nazwie "kolejki testowej" przy użyciu **Wysyłanie\_kolejki\_message()**:

```
message = Azure::ServiceBus::BrokeredMessage.new("test queue message")
message.correlation_id = "test-correlation-id"
azure_service_bus_service.send_queue_message("test-queue", message)
```

Usługa Bus kolejki obsługują maksymalny rozmiar wiadomości 256 KB w [standardowej warstwy](service-bus-premium-messaging.md) i 1 MB w [warstwie Premium](service-bus-premium-messaging.md). Nagłówek, który zawiera właściwości aplikacji standardowych i niestandardowych, może mieć maksymalny rozmiar 64 KB. Istnieje limit liczby wiadomości w kolejce, ale jest zakończenie na całkowity rozmiar wiadomości przechowywanych w kolejce. Rozmiar kolejki jest zdefiniowane w czasie tworzenia z maksymalnie 5 GB.

## <a name="how-to-receive-messages-from-a-queue"></a>Jak odebrać wiadomości z kolejki

Wiadomości są odbierane z kolejki przy użyciu **odbieranie\_kolejki\_message()** metody obiektu **Azure::ServiceBusService** . Domyślnie wiadomości są czytanie i zablokowane bez są usuwane z kolejki. Jednak możesz usunąć wiadomości z kolejki jako odczycie przez ustawienie **: peek_lock** opcja **FAŁSZ**.

Domyślne zachowanie służy do odczytu i usuwanie operacji dwuetapowy, która umożliwia także do obsługi aplikacji, nie można tolerować brakujące wiadomości. Gdy usługa Bus otrzymuje żądanie, umożliwia znalezienie wyrazów następnej wiadomości do zużycia, blokuje go, aby uniemożliwić innym konsumentów go odbiera i zwraca go do aplikacji. Po aplikacji zakończeniu przetwarzania wiadomości (lub zapisuje go w wiarygodny sposób przetwarzania przyszłych) wykonuje drugiego procesu Odbierz dzwoniąc **Usuwanie\_kolejki\_message()** metody i ich dostarczania wiadomości zostanie usunięta jako parametru. **Usuwanie\_kolejki\_message()** metody będzie oznaczyć wiadomość jako są używane i usunąć go z niej.

Jeśli **: wglądu\_blokady** parametr jest ustawiony na **wartość FAŁSZ**, czytania i usuwania wiadomości staje się najłatwiejszym modelu, a także sprawdza się najlepiej w przypadku scenariuszy, w których aplikacja przeszkadzają, nie przetwarzania wiadomości w przypadku awarii. Aby zrozumieć, to, warto rozważyć scenariusza, w którym problemy żądania odbioru konsumenta, a następnie ulega awarii przed jego przetworzenia. Ponieważ usługa Bus zostaną zaznaczone wiadomości jako są używane, gdy aplikacja uruchomieniu i rozpoczyna się ponownie przez inne wiadomości, będą zostały odebrane wiadomości, która została wykorzystana przed awarii.

W poniższym przykładzie pokazano sposób odbierania i przetwarzania wiadomości przy użyciu **odbieranie\_kolejki\_message()**. Przykład najpierw odbiera i usuwa wiadomości przy użyciu **: wglądu\_blokady** ustaw wartość **FAŁSZ**, otrzymuje inną wiadomość, a następnie usuwa wiadomości przy użyciu **Usuwanie\_kolejki\_message()**:

```
message = azure_service_bus_service.receive_queue_message("test-queue",
  { :peek_lock => false })
message = azure_service_bus_service.receive_queue_message("test-queue")
azure_service_bus_service.delete_queue_message(message)
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Jak obsługiwać awarie aplikacji i nie można odczytać wiadomości

Usługa Bus zawiera funkcje ułatwiające przywrócenie bezpiecznie z błędami w aplikacji lub trudności przetwarzania wiadomości. Jeśli aplikacja odbiorcy jest nie może przetworzyć wiadomości jakiegoś powodu, a następnie można zadzwonić **odblokować\_kolejki\_message()** metody obiektu **Azure::ServiceBusService** . Ta opcja powoduje Bus usługi odblokować wiadomości w kolejce i udostępnić otrzymać ponownie przez tę samą aplikację dużo lub w innej aplikacji dużo.

Istnieje również skojarzony komunikat zablokowane w kolejce przekroczenie limitu czasu, a jeśli aplikacji nie powiedzie się przetwarzania wiadomości przed limit czasu blokady wygaśnie (na przykład jeśli powoduje awarię aplikacji), następnie Bus usługi automatycznie odblokowanie wiadomości i udostępnia będzie odebrana ponownie.

W przypadku, gdy aplikacja ulega awarii po przetworzeniu wiadomości, ale przed **Usuwanie\_kolejki\_message()** metoda jest wywoływana, a następnie wiadomości będzie się przed przeniesieniem do aplikacji po jego ponownym uruchomieniu. Jest to często nazywane, **Co najmniej raz przetwarzania**; oznacza to, że każda wiadomość jest przetwarzana co najmniej raz, ale w niektórych przypadkach może być przed przeniesieniem tego samego komunikatu. Jeśli tego scenariusza nie można tolerować zduplikowanych przetwarzanie, deweloperów aplikacji należy dodać dodatkowe logiki do ich aplikacji do obsługi dostarczania zduplikowanych wiadomości. Często można to osiągnąć przy użyciu **wiadomości\_identyfikator** właściwość wiadomości, która pozostaje stała wielu prób dostarczenia.

## <a name="next-steps"></a>Następne kroki

Teraz, gdy znasz już podstawy kolejek Bus usługi, skorzystaj z poniższych łączy, aby dowiedzieć się więcej.

-   Omówienie [kolejki, tematy i subskrypcji](service-bus-queues-topics-subscriptions.md).
-   Odwiedź repozytorium [SDK Azure dla dopiskiem](https://github.com/Azure/azure-sdk-for-ruby) GitHub.

Do porównania kolejek Bus usługi Azure omawiane w niniejszym artykule oraz omawiane w artykule [jak za pomocą magazyn kolejek z dopiskiem](../storage/storage-ruby-how-to-use-queue-storage.md) kolejek Azure zobacz [kolejki Azure i kolejek Bus usług Azure — porównanie i Contrasted](service-bus-azure-and-service-bus-queues-compared-contrasted.md)
 
