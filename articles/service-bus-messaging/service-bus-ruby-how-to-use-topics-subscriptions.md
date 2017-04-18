<properties
    pageTitle="Jak używać usługi Bus tematy (dopiskiem) | Microsoft Azure"
    description="Dowiedz się, jak używać tematy Bus usługi i subskrypcje w Azure. Przykłady kodu są zapisywane dla dopiskiem fonetycznym aplikacji."
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

# <a name="how-to-use-service-bus-topicssubscriptions"></a>Jak korzystać z subskrypcji tematy Bus usług

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Ten artykuł zawiera opis sposobu używania tematy Bus usługi i subskrypcje z dopiskiem fonetycznym aplikacji. Scenariusze, w których zawierać **tworzenia tematy i subskrypcje, tworzenia filtrów subskrypcji, wysyłanie wiadomości** do tematu, **odbierania wiadomości z subskrypcji**i **Usuwanie tematów i subskrypcji**. Aby uzyskać więcej informacji na tematy i subskrypcje zobacz sekcję [Następne kroki](#next-steps) .

## <a name="service-bus-topics-and-subscriptions"></a>Tematy Bus usługi i subskrypcje

Subskrypcje i tematy Bus usługi pomocy technicznej *publikowania subskrypcji* wiadomości komunikacji modelu. Podczas korzystania z subskrypcji i tematów, części aplikacji rozproszonej komunikuje się bezpośrednio ze sobą; Zamiast tego wymienianych wiadomości za pomocą temacie pośredniczącym.

![TopicConcepts](./media/service-bus-ruby-how-to-use-topics-subscriptions/sb-topics-01.png)

W odróżnieniu od kolejki Bus usługi, w którym każda wiadomość jest przetwarzana przez jednego dla klientów indywidualnych, tematy i subskrypcje zapewniają formularzem **jeden do wielu** komunikacji, używając deseń publikowania/subskrypcji. Istnieje możliwość rejestrowania wiele subskrypcji na temat. Gdy wiadomość jest wysyłana do tematu, go jest następnie udostępniane do każdej subskrypcji przetwarzania niezależnie.

Subskrypcja tematów przypomina wirtualnych kolejka otrzymuje kopie wiadomości, które zostały wysłane do tematu. Opcjonalnie można zarejestrować zasady filtru tematu w oparciu o na subskrypcje, który umożliwia filtrowanie i ograniczanie na temat wiadomości, które są odbierane przez które subskrypcje tematu.

Tematy Bus usługi i subskrypcje umożliwiają skalowanie przetwarzania dużej liczby wiadomości w dużej liczby użytkowników i aplikacje.

## <a name="create-a-namespace"></a>Tworzenie obszaru nazw

Aby rozpocząć korzystanie z usługi Bus kolejek platformy Azure, musisz najpierw utworzyć przestrzeń nazw. Przestrzeń nazw zawiera kontenerze obejmującym adresowania Bus usługi zasobów w aplikacji. Należy utworzyć nazw za pośrednictwem interfejsu wiersza polecenia, ponieważ [Azure portal][] nie tworzenie nazw z połączeniem ACS.

Aby utworzyć przestrzeń nazw:

1. Otwórz okno konsoli Azure programu Powershell.

2. Wpisz następujące polecenie, aby utworzyć obszar nazw. Udostępnia własną wartość nazw i określić tego samego regionu jako aplikacja.

    ```
    New-AzureSBNamespace -Name 'yourexamplenamespace' -Location 'West US' -NamespaceType 'Messaging' -CreateACSNamespace $true
    ```

    ![Tworzenie Namespace](./media/service-bus-ruby-how-to-use-topics-subscriptions/showcmdcreate.png)

## <a name="obtain-default-management-credentials-for-the-namespace"></a>Uzyskaj domyślne poświadczenia zarządzania obszaru nazw

W celu wykonywania operacji zarządzania, takich jak tworzenie kolejki na nowy obszar nazw, musisz uzyskać poświadczenia zarządzania obszaru nazw.

Polecenia cmdlet programu PowerShell, wykonanego tworzenie nazw Bus usługi zostanie wyświetlony klucz, który służy do zarządzania obszarem nazw. Skopiuj wartość **DefaultKey** . W dalszej części tego samouczka użyje tej wartości w kodzie.

![Kopiowanie klucza](./media/service-bus-ruby-how-to-use-topics-subscriptions/defaultkey.png)

> [AZURE.NOTE]
> Można również znaleźć ten klucz, jeśli zalogować się do [portalu Azure][] i przejść do informacji o połączeniu obszaru nazw.

## <a name="create-a-ruby-application"></a>Tworzenie aplikacji dopiskiem fonetycznym

Aby uzyskać instrukcje zobacz [Tworzenie aplikacji sieci dopiskiem Azure](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).

## <a name="configure-your-application-to-use-service-bus"></a>Konfigurowanie aplikacji użyj usługi Bus

Aby użyć usługi Bus, Pobierz i użycie funkcji Spakuj dopiskiem Azure, zawiera zestaw wygody bibliotek, które komunikowanie się za pomocą usługi REST miejsca do magazynowania.

### <a name="use-rubygems-to-obtain-the-package"></a>Umożliwia uzyskanie pakietu RubyGems

1. Za pomocą interfejsu wiersza polecenia **programu PowerShell** (Windows), **Terminal** (Mac) lub **urodzinową** (Unix).

2. Wpisz "gem instalacji azure" w oknie polecenia, aby zainstalować gem i zależności.

### <a name="import-the-package"></a>Importowanie pakietu

Za pomocą edytora tekstu Ulubione, Dodaj następujący tekst na początek dopiskiem fonetycznym pliku, w którym mają być używane miejsca do magazynowania:

```
require "azure"
```

## <a name="set-up-a-service-bus-connection"></a>Konfigurowanie połączenia usługi Bus

Moduł Azure odczytuje zmienne środowiska **AZURE\_SERVICEBUS\_nazw** i **AZURE\_SERVICEBUS\_dostępu\_klucza** dla informacje wymagane do połączenia do obszaru nazw. Jeśli nie ustawiono zmienne środowiska, należy określić informacje nazw przed **Azure::ServiceBusService** za pomocą następującego kodu:

```
Azure.config.sb_namespace = "<your azure service bus namespace>"
Azure.config.sb_access_key = "<your azure service bus access key>"
```

Ustaw wartość nazw na wartość, która została utworzona zamiast pełnego adresu URL. Na przykład za pomocą **"yourexamplenamespace"**, nie "yourexamplenamespace.servicebus.windows.net".

## <a name="create-a-topic"></a>Tworzenie tematu

Obiekt **Azure::ServiceBusService** pozwala pracować z tematami. Poniższy kod tworzy obiekt **Azure::ServiceBusService** . Aby utworzyć tematu, użyj **Tworzenie\_topic()** metody. Poniższy przykład tworzy tematu lub drukowania błędy Jeśli istnieją.

```
azure_service_bus_service = Azure::ServiceBusService.new
begin
  topic = azure_service_bus_service.create_queue("test-topic")
rescue
  puts $!
end
```

Można również przekazać obiektu **Azure::ServiceBus::Topic** dodatkowe opcje, które umożliwiają Zastępowanie domyślnych ustawień tematu takie jak godzina wiadomości na żywo lub maksymalny rozmiar kolejki. W poniższym przykładzie pokazano ustawianie maksymalny rozmiar kolejki 5GB i czasu wygaśnięcia na 1 minutę:

```
topic = Azure::ServiceBus::Topic.new("test-topic")
topic.max_size_in_megabytes = 5120
topic.default_message_time_to_live = "PT1M"

topic = azure_service_bus_service.create_topic(topic)
```

## <a name="create-subscriptions"></a>Tworzenie subskrypcji

Subskrypcje tematu są również tworzone z obiektem **Azure::ServiceBusService** . Subskrypcje są nazywane i nie może mieć opcjonalne filtr, który ogranicza zestaw dostarczane do subskrypcji wirtualnych kolejki wiadomości.

Subskrypcje są trwałe i będzie w dalszym ciągu istnieją dopiero albo lub temat są skojarzone, są usuwane. Jeśli aplikacja zawiera logiki do tworzenia subskrypcji, najpierw skontaktować się czy subskrypcja już istnieje przy użyciu metody getSubscription.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Tworzenie subskrypcji z filtr domyślny (MatchAll)

Filtr **MatchAll** jest filtr domyślny, który jest używany, jeśli nie filtr został określony podczas tworzenia nowej subskrypcji. Użycie filtru **MatchAll** wszystkie wiadomości opublikowany w tym temacie są umieszczane w kolejce wirtualnych subskrypcji. Poniższy przykład tworzy subskrypcji o nazwie "wszystkie wiadomości" i używa filtrowanie **MatchAll** domyślne.

```
subscription = azure_service_bus_service.create_subscription("test-topic", "all-messages")
```

### <a name="create-subscriptions-with-filters"></a>Tworzenie subskrypcji przy użyciu filtrów

Można również zdefiniować filtry, które pozwalają określić, które należy są wyświetlane wiadomości wysłane do tematu w określonej subskrypcji.

Najbardziej elastyczna typ filtru obsługiwane w przypadku subskrypcji jest **Azure::ServiceBus::SqlFilter**, który wykonuje podzbiór standardzie SQL92. Filtry SQL działają na właściwości wiadomości, które są publikowane w tym temacie. Aby uzyskać więcej informacji na temat wyrażeń, których można używać z filtrem SQL Przejrzyj składni [SqlFilter.SqlExpression](http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx) .

Filtry można dodać do subskrypcji, przy użyciu **Tworzenie\_rule()** metody obiektu **Azure::ServiceBusService** . Ta metoda pozwala na dodawanie nowych filtrów do istniejącej subskrypcji.

Ponieważ filtr domyślny jest stosowany automatycznie do wszystkich nowych subskrypcji, należy najpierw usunąć filtr domyślny lub **MatchAll** zastępują inne filtry, które można określić. Możesz usunąć reguły domyślnej przy użyciu **Usuwanie\_rule()** metody obiektu **Azure::ServiceBusService** .

Poniższy przykład tworzy subskrypcji o nazwie "Maks wiadomości" z wybiera tylko wiadomości, które mają niestandardowy **Azure::ServiceBus::SqlFilter** **wiadomości\_numer** właściwość większa niż 3:

```
subscription = azure_service_bus_service.create_subscription("test-topic", "high-messages")
azure_service_bus_service.delete_rule("test-topic", "high-messages", "$Default")

rule = Azure::ServiceBus::Rule.new("high-messages-rule")
rule.topic = "test-topic"
rule.subscription = "high-messages"
rule.filter = Azure::ServiceBus::SqlFilter.new({
  :sql_expression => "message_number > 3" })
rule = azure_service_bus_service.create_rule(rule)
```

Podobnie poniższym przykładzie jest tworzona subskrypcji o nazwie "min. wiadomości" z **Azure::ServiceBus::SqlFilter** wybierające tylko wiadomości, które mają **message_number** właściwość mniejsze niż lub równe 3:

```
subscription = azure_service_bus_service.create_subscription("test-topic", "low-messages")
azure_service_bus_service.delete_rule("test-topic", "low-messages", "$Default")

rule = Azure::ServiceBus::Rule.new("low-messages-rule")
rule.topic = "test-topic"
rule.subscription = "low-messages"
rule.filter = Azure::ServiceBus::SqlFilter.new({
  :sql_expression => "message_number <= 3" })
rule = azure_service_bus_service.create_rule(rule)
```

Gdy wiadomość jest wysyłana teraz do "temat testowy", zawsze będą dostarczane do odbiorców subskrybuje subskrypcji tematu "wszystkie wiadomości", a selektywne dostarczane do odbiorców subskrybuje "Maks wiadomości" i "niski wiadomości" subskrypcje tematu (w zależności treść wiadomości).

## <a name="send-messages-to-a-topic"></a>Wysyłanie wiadomości na temat

Aby wysłać wiadomość do tematu Bus usługi, należy użyć aplikacji **Wysyłanie\_tematu\_message()** metody obiektu **Azure::ServiceBusService** . Wiadomości wysyłane do usługi Bus tematy są wystąpień obiektów **Azure::ServiceBus::BrokeredMessage** . Obiekty **Azure::ServiceBus::BrokeredMessage** mają pewien zestaw właściwości standardowe (taki jak **Etykieta** i **czas\_do\_live**), słownik, który służy do przechowywania niestandardowej aplikacji określone właściwości i treści ciąg danych. Aplikacja można ustawić treści wiadomości, przekazując wartości ciągu na **Wysyłanie\_tematu\_message()** metody i wszystkie wymagane właściwości standardowe zostaną wypełnione według wartości domyślnej.

Poniższy przykład przedstawia sposób do wysyłania wiadomości do "temat testowy" pięć testowych. Należy zauważyć, że wartości właściwości niestandardowej **message_number** każdej wiadomości zmienia się na iteracji pętli (określa, z jakiej subskrypcji otrzymujących list):

```
5.times do |i|
  message = Azure::ServiceBus::BrokeredMessage.new("test message " + i,
    { :message_number => i })
  azure_service_bus_service.send_topic_message("test-topic", message)
end
```

Usługa Bus tematy pomocy technicznej maksymalny rozmiar wiadomości 256 KB w [standardowej warstwy](service-bus-premium-messaging.md) i 1 MB w [warstwie Premium](service-bus-premium-messaging.md). Nagłówek, który zawiera właściwości aplikacji standardowych i niestandardowych, może mieć maksymalny rozmiar 64 KB. Istnieje limit liczby znajdujące się na temat wiadomości, ale jest zakończenie na całkowity rozmiar wiadomości według tematu. Rozmiar tego tematu jest zdefiniowany w czasie tworzenia z maksymalnie 5 GB.

## <a name="receive-messages-from-a-subscription"></a>Odbieranie wiadomości z subskrypcji

Wiadomości są odbierane z subskrypcji przy użyciu **odbieranie\_subskrypcji\_message()** metody obiektu **Azure::ServiceBusService** . Domyślnie wiadomości są read(peak) i zablokowane bez usuwania go z subskrypcji. Można czytać i usunąć wiadomość z subskrypcji, ustawiając **wglądu\_blokady** opcja **FAŁSZ**.

Domyślne zachowanie służy do odczytu i usuwanie operacji dwuetapowy, która umożliwia także do obsługi aplikacji, nie można tolerować brakujące wiadomości. Gdy usługa Bus otrzymuje żądanie, umożliwia znalezienie wyrazów następnej wiadomości do zużycia, blokuje go, aby uniemożliwić innym konsumentów go odbiera i zwraca go do aplikacji. Po aplikacji zakończeniu przetwarzania wiadomości (lub zapisuje go w wiarygodny sposób przetwarzania przyszłych) wykonuje drugiego procesu Odbierz dzwoniąc **Usuwanie\_subskrypcji\_message()** metody i ich dostarczania wiadomości zostanie usunięta jako parametru. **Usuwanie\_subskrypcji\_message()** metody zostaną oznaczyć wiadomość jako są używane i usunąć go z subskrypcji.

Jeśli **: wglądu\_blokady** parametr jest ustawiony na **wartość FAŁSZ**, czytania i usuwania wiadomości staje się najłatwiejszym modelu, a także sprawdza się najlepiej w przypadku scenariuszy, w których aplikacja przeszkadzają, nie przetwarzania wiadomości w przypadku awarii. Aby zrozumieć, to, warto rozważyć scenariusza, w którym problemy żądania odbioru konsumenta, a następnie ulega awarii przed jego przetworzenia. Ponieważ usługa Bus zostanie oznaczona wiadomość jako są używane, a następnie, gdy aplikacja uruchomieniu i rozpoczyna się ponownie przez inne wiadomości, będą zostały odebrane wiadomości, która została wykorzystana przed awarii.

W poniższym przykładzie pokazano, jak mogą być odbierane wiadomości i przetworzone przy użyciu **otrzymywać\_subskrypcji\_message()**. Przykład najpierw odbiera i usuwa wiadomość subskrypcję "niski wiadomości" przy użyciu **: wglądu\_blokady** ustaw wartość **FAŁSZ**, otrzyma następnej wiadomości z "wiadomości wysoki", a następnie usuwa wiadomości przy użyciu **Usuwanie\_subskrypcji\_message()**:

```
message = azure_service_bus_service.receive_subscription_message(
  "test-topic", "low-messages", { :peek_lock => false })
message = azure_service_bus_service.receive_subscription_message(
  "test-topic", "high-messages")
azure_service_bus_service.delete_subscription_message(message)
```

## <a name="handle-application-crashes-and-unreadable-messages"></a>Uchwyt awarii aplikacji i nie można odczytać wiadomości

Usługa Bus zawiera funkcje ułatwiające przywrócenie bezpiecznie z błędami w aplikacji lub trudności przetwarzania wiadomości. Jeśli aplikacja odbiorcy jest nie może przetworzyć wiadomości jakiegoś powodu, a następnie można zadzwonić **odblokować\_subskrypcji\_message()** metody obiektu **Azure::ServiceBusService** . Ta opcja powoduje Bus usługi odblokować wiadomości w ramach subskrypcji i udostępnić otrzymać ponownie przez tę samą aplikację dużo lub w innej aplikacji dużo.

Jest również przekroczenie limitu czasu skojarzone z komunikatem zablokowane w ramach subskrypcji, a jeśli aplikacji nie powiedzie się przetwarzania wiadomości przed limit czasu blokady wygaśnie (na przykład jeśli powoduje awarię aplikacji), usługi Bus będą automatycznie odblokowywanie wiadomości i udostępnić będzie odebrana ponownie.

W przypadku, gdy aplikacja ulega awarii po przetworzeniu wiadomości, ale przed **Usuwanie\_subskrypcji\_message()** metoda jest wywoływana, a następnie po jego ponownym uruchomieniu wiadomość jest przed przeniesieniem do aplikacji. Jest to często nazywane, **Co najmniej raz przetwarzania**; oznacza to, że każda wiadomość zostanie przetworzony co najmniej raz, ale w niektórych przypadkach może być przed przeniesieniem tego samego komunikatu. Jeśli tego scenariusza nie można tolerować zduplikowane przetwarzanie, deweloperzy aplikacji należy dodać dodatkowe logiki do ich aplikacji do obsługi dostarczania zduplikowane wiadomości. Tę logikę często uzyskuje się za pomocą **wiadomości\_identyfikator** właściwość wiadomości, która pozostaje stała wielu prób dostarczenia.

## <a name="delete-topics-and-subscriptions"></a>Usuwanie tematów i subskrypcji

Tematy i subskrypcje są trwałe i musi być jawnie usunięta przez [Azure portal][] lub programowo. W poniższym przykładzie przedstawiono sposób usuwanie tematu o nazwie "temat testowy".

```
azure_service_bus_service.delete_topic("test-topic")
```

Usuwanie tematu powoduje także usunięcie żadnej subskrypcji, które są zarejestrowane wraz z tematem. Subskrypcje mogą być również usuwane niezależnie. Poniższy kod przedstawia sposób usunięcia subskrypcji o nazwie "Wysoki wiadomości" w temacie "temat testowy":

```
azure_service_bus_service.delete_subscription("test-topic", "high-messages")
```

## <a name="next-steps"></a>Następne kroki

Teraz, gdy znasz już podstawy tematów dotyczących usługi Bus, skorzystaj z poniższych łączy, aby dowiedzieć się więcej.

- Zobacz [kolejki, tematy i subskrypcji](service-bus-queues-topics-subscriptions.md).
- Opis interfejsu API dla [SqlFilter](http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx).
- Odwiedź repozytorium [SDK Azure dla dopiskiem](https://github.com/Azure/azure-sdk-for-ruby) GitHub.
 
[Azure portal]: https://portal.azure.com
