> [AZURE.SELECTOR]
- [Linux](../articles/iot-hub/iot-hub-linux-gateway-sdk-get-started.md)
- [Systemu Windows](../articles/iot-hub/iot-hub-windows-gateway-sdk-get-started.md)

Ten artykuł zawiera szczegółowe wskazówki [Hello World przykładowy kod] [ lnk-helloworld-sample] do zilustrowania podstawowych składników [Zestawu SDK Azure IoT Gateway] [ lnk-gateway-sdk] architektury. W przykładzie zastosowano zestawu SDK bramy Centrum IoT budować proste bramy, która rejestruje komunikat "hello world" w pliku co pięć sekund.

W tym instruktażu obejmuje:

- **Pojęcia**: Omówienie składników tworzących wszystkie bramy tworzona za pomocą zestawu SDK bramy IoT.  
- **Architektura próbki Hello World**: Opisuje sposób pojęcia stosuje się do próbki Hello World i jak składniki pasują do siebie.
- **Jak zbudować próbki**: kroki wymagane do utworzenia próbki.
- **Jak uruchomić próbki**: kroki wymagane do uruchamiania przykładu. 
- **Dane wyjściowe zazwyczaj**: przykład danych wyjściowych oczekiwać po uruchomieniu próbki.
- **Urywki kodu**: kolekcja wstawek kodu programu, aby pokazać, jak próbki Hello World implementuje bramy kluczowych składników.

## <a name="azure-iot-gateway-sdk-concepts"></a>Pojęcia SDK bramy IoT Azure

Zanim sprawdzić kod przykładowy, lub utworzyć własne bramy pola przy użyciu zestawu SDK bramy IoT, należy zrozumieć podstawowe pojęcia, które stanowią podstawę architektury zestawu SDK.

### <a name="modules"></a>Moduły

Możesz zbudować bramy z zestawu SDK bramy IoT Azure tworząc a montaż *modułów*. Moduły umożliwia wymianę danych między sobą *wiadomości* . Moduł odbiera wiadomość, wykonuje akcję na nim, opcjonalnie przekształca go do nowej wiadomości i następnie publikuje go dla innych modułów do przetworzenia. Niektóre moduły może produkować tylko nowe wiadomości i nigdy nie przetwarzają wiadomości przychodzące. Łańcuch modułów tworzy potok przetwarzania danych z każdego modułu wykonywania przekształcenia na dane w jednym punkcie w tym potoku.

![Łańcuch modułów w bramy zbudowany z zestawu SDK bramy IoT Azure][1]
 
Zestaw SDK zawiera następujące elementy:

- Ułożonych wcześniej modułów, które wykonywanie typowych funkcji bramy.
- Interfejsy dewelopera służy do pisania niestandardowych modułów.
- Infrastrukturę niezbędną do wdrożenia i uruchom zestaw modułów.

Zestaw SDK zawiera warstwę abstrakcji, który umożliwia tworzenie bramy do uruchamiania na wiele systemów operacyjnych i platform.

![Warstwa abstrakcji SDK bramy IoT Azure][2]

### <a name="messages"></a>Wiadomości

Mimo że myślenia o modułach przekazywania wiadomości do siebie nawzajem jest wygodnym sposobem konceptualizacji jak funkcje bramy, to niedokładnie odzwierciedlać co się dzieje. Moduły używać brokera do komunikowania się ze sobą, publikowanie wiadomości broker (bus, pubsub lub innych wzorzec wiadomości) i pozwól broker routowania wiadomości z modułami podłączonymi do niego.

Moduły funkcja **Broker_Publish** publikowanie wiadomości do brokera. Pośrednik dostarcza wiadomości do modułu przez wywołanie funkcji wywołania zwrotnego. Komunikat składa się z zestawu klucz i wartość właściwości i zawartości przekazywana jako bloku pamięci.

![Roli Broker w zestawie SDK bramy IoT Azure][3]

### <a name="message-routing-and-filtering"></a>Routowanie wiadomości i filtrowanie

Istnieją dwa sposoby kierowania wiadomości do poprawnego modułów. Zestaw łączy mogą być przekazywane do brokera, więc brokera wie, źródło i obiekt sink dla każdego modułu lub modułu można filtrować według właściwości wiadomości. Moduł tylko należy stosowac wiadomości, jeśli wiadomość jest przeznaczona dla niego. Łącza i filtrowanie wiadomości jest to, co skutecznie tworzy rurociągu wiadomości.

## <a name="hello-world-sample-architecture"></a>Hello World próbki architektury

W tym przykładzie Hello World przedstawiono Koncepcje opisane w poprzedniej sekcji. Przykład Hello World implementuje bramy, która ma rurociąg składa się z dwóch modułów:

-   *Witaj świecie* moduł tworzy wiadomość co pięć sekund i przekazuje je do modułu rejestratora.
-   Modułu *rejestratora* wiadomości otrzymanej zapisuje w pliku.

![Architektura próbki Hello World zbudowany z zestawu SDK bramy IoT Azure][4]

Zgodnie z opisem w poprzedniej sekcji, moduł Hello World nie przekazuje wiadomości bezpośrednio do modułu rejestratora co pięć sekund. Zamiast tego publikuje komunikat do brokera co pięć sekund.

Moduł rejestratora odbiera komunikat z brokera i akty stronę, zapisywanie zawartości wiadomości do pliku.

Moduł rejestratora zużywa tylko wiadomości z brokera, nigdy nie publikuje nowe wiadomości do brokera.

![Jak broker Routing wiadomości między modułami w zestawie SDK bramy IoT Azure][5]

Rysunek powyżej przedstawiono architekturę próbki Hello World i ścieżki względne do plików źródłowych, które implementują różne części próbki w [repozytorium][lnk-gateway-sdk]. Poznać kod samodzielnie lub użyć wstawki kodu poniżej jako przewodnik.

<!-- Images -->
[1]: media/iot-hub-gateway-sdk-getstarted-selector/modules.png
[2]: media/iot-hub-gateway-sdk-getstarted-selector/modules_2.png
[3]: media/iot-hub-gateway-sdk-getstarted-selector/messages_1.png
[4]: media/iot-hub-gateway-sdk-getstarted-selector/high_level_architecture.png
[5]: media/iot-hub-gateway-sdk-getstarted-selector/detailed_architecture.png

<!-- Links -->
[lnk-helloworld-sample]: https://github.com/Azure/azure-iot-gateway-sdk/tree/master/samples/hello_world
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk