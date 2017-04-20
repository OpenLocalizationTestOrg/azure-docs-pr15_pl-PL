## <a name="typical-output"></a>Dane wyjściowe zazwyczaj

Poniżej znajduje się przykład danych wyjściowych zapisane do pliku dziennika przez próbkę Hello World. Aby uzyskać dobrą czytelność zostały dodane znaki nowego wiersza i kartę:

```
[{
    "time": "Mon Apr 11 13:48:07 2016",
    "content": "Log started"
}, {
    "time": "Mon Apr 11 13:48:48 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:48:55 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:49:01 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:49:04 2016",
    "content": "Log stopped"
}]
```

## <a name="code-snippets"></a>Wstawki kodu programu

W tej sekcji omówiono niektóre kluczowe części kodu w próbce Hello World.

### <a name="gateway-creation"></a>Tworzenie bramy

Deweloper musi napisać *procesu bramy*. Ten program tworzy wewnętrznej infrastruktury (broker), ładuje moduły i konfiguruje wszystko, aby działać poprawnie. Zestaw SDK zawiera funkcję **Gateway_Create_From_JSON** , aby umożliwiają bootstrap bramy z pliku JSON. Do funkcji **Gateway_Create_From_JSON** musi przekazać je ścieżkę pliku JSON, który określa moduły, aby załadować. 

Można znaleźć kod dla procesu bramy w próbce Hello World w [main.c] [ lnk-main-c] pliku. Aby uzyskać dobrą czytelność fragmentu poniżej pokazuje skróconej wersji kod procesu bramy. Ten program tworzy bramę, a następnie czeka na użytkownika o naciśnięcie klawisza **ENTER** , zanim to łzy w dół bramy. 

```
int main(int argc, char** argv)
{
    GATEWAY_HANDLE gateway;
    if ((gateway = Gateway_Create_From_JSON(argv[1])) == NULL)
    {
        printf("failed to create the gateway from JSON\n");
    }
    else
    {
        printf("gateway successfully created from JSON\n");
        printf("gateway shall run until ENTER is pressed\n");
        (void)getchar();
        Gateway_LL_Destroy(gateway);
    }
    return 0;
}
```

Plik ustawień JSON zawiera listę modułów, aby załadować. Każdy moduł należy określić o:

- **nazwa_modułu**: unikatową nazwę modułu.
- **module_path**: ścieżka do biblioteki zawierającej modułu. Linux jest to plik .so, w systemie Windows jest to plik .dll.
- **argumenty**: moduł musi informacje konfiguracyjne.

Plik JSON zawiera także łącza między modułami, które zostaną przekazane do brokera. Łącze ma dwie właściwości:
- **źródło**: Nazwa modułu z `modules` sekcji, lub "\*".
- **obiekt sink**: Nazwa modułu z `modules` sekcji

Każde łącze definiuje trasy wiadomości i kierunek. Wiadomości z modułu `source` ma być dostarczony do modułu `sink`. `source` Może być ustawiony na "\*", wskazujące, że wiadomości od każdego modułu zostaną odebrane przez `sink`.

Następujący przykład przedstawia plik ustawień JSON, używane do konfigurowania próbki Hello World w systemie Linux. Każda wiadomość produkowane przez moduł `hello_world` będzie wykorzystana przez moduł `logger`. Czy moduł wymaga argumentu zależy od konstrukcji modułu. W tym przykładzie moduł Rejestrator wymaga argumentu, który jest ścieżką do pliku wyjściowego i moduł Hello World nie ma żadnych argumentów:

```
{
    "modules" :
    [ 
        {
            "module name" : "logger",
            "module path" : "./modules/logger/liblogger_hl.so",
            "args" : {"filename":"log.txt"}
        },
        {
            "module name" : "hello_world",
            "module path" : "./modules/hello_world/libhello_world_hl.so",
            "args" : null
        }
    ],
    "links" :
    [
        {
            "source" : "hello_world",
            "sink" : "logger"
        }
    ]
}
```

### <a name="hello-world-module-message-publishing"></a>Publikowanie wiadomości powitania świat modułu

Można znaleźć kod używany przez moduł "hello world" do publikowania komunikatów w ['hello_world.c'] [ lnk-helloworld-c] pliku. Poniższy fragment kodu przedstawia zmieniona wersja z dodatkowe komentarze i niektóre usunięte dla czytelności kodu obsługi błędów:

```
int helloWorldThread(void *param)
{
    // create data structures used in function.
    HELLOWORLD_HANDLE_DATA* handleData = param;
    MESSAGE_CONFIG msgConfig;
    MAP_HANDLE propertiesMap = Map_Create(NULL);
    
    // add a property named "helloWorld" with a value of "from Azure IoT
    // Gateway SDK simple sample!" to a set of message properties that
    // will be appended to the message before publishing it. 
    Map_AddOrUpdate(propertiesMap, "helloWorld", "from Azure IoT Gateway SDK simple sample!")

    // set the content for the message
    msgConfig.size = strlen(HELLOWORLD_MESSAGE);
    msgConfig.source = HELLOWORLD_MESSAGE;

    // set the properties for the message
    msgConfig.sourceProperties = propertiesMap;
    
    // create a message based on the msgConfig structure
    MESSAGE_HANDLE helloWorldMessage = Message_Create(&msgConfig);

    while (1)
    {
        if (handleData->stopThread)
        {
            (void)Unlock(handleData->lockHandle);
            break; /*gets out of the thread*/
        }
        else
        {
            // publish the message to the broker
            (void)Broker_Publish(handleData->brokerHandle, helloWorldMessage);
            (void)Unlock(handleData->lockHandle);
        }

        (void)ThreadAPI_Sleep(5000); /*every 5 seconds*/
    }

    Message_Destroy(helloWorldMessage);

    return 0;
}
```

### <a name="hello-world-module-message-processing"></a>Przetwarzanie wiadomości Hello World modułu

Hello World nigdy nie należy przetwarzać żadnych komunikatów dotyczących opublikowanymi przez inne moduły do brokera. W efekcie wykonania wywołania zwrotnego na wiadomość w module Hello World funkcja zerowa.

```
static void HelloWorld_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{
    /* No action, HelloWorld is not interested in any messages. */
}
```

### <a name="logger-module-message-publishing-and-processing"></a>Komunikat modułu rejestratora publikowania i przetwarzania

Moduł rejestratora odbiera wiadomości z brokera i zapisuje je do pliku. Nigdy nie publikuje żadnych wiadomości. W związku z tym kod modułu rejestratora nigdy nie wywołuje funkcję **Broker_Publish** .

Funkcja **Logger_Recieve** w [logger.c] [ lnk-logger-c] plik jest wywołanie zwrotne brokera wywołuje dostarczania wiadomości do modułu rejestratora. Poniższy fragment kodu przedstawia zmieniona wersja z dodatkowe komentarze i niektóre usunięte dla czytelności kodu obsługi błędów:

```
static void Logger_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{

    time_t temp = time(NULL);
    struct tm* t = localtime(&temp);
    char timetemp[80] = { 0 };

    // Get the message properties from the message
    CONSTMAP_HANDLE originalProperties = Message_GetProperties(messageHandle); 
    MAP_HANDLE propertiesAsMap = ConstMap_CloneWriteable(originalProperties);

    // Convert the collection of properties into a JSON string
    STRING_HANDLE jsonProperties = Map_ToJSON(propertiesAsMap);

    //  base64 encode the message content
    const CONSTBUFFER * content = Message_GetContent(messageHandle);
    STRING_HANDLE contentAsJSON = Base64_Encode_Bytes(content->buffer, content->size);

    // Start the construction of the final string to be logged by adding
    // the timestamp
    STRING_HANDLE jsonToBeAppended = STRING_construct(",{\"time\":\"");
    STRING_concat(jsonToBeAppended, timetemp);

    // Add the message properties
    STRING_concat(jsonToBeAppended, "\",\"properties\":"); 
    STRING_concat_with_STRING(jsonToBeAppended, jsonProperties);

    // Add the content
    STRING_concat(jsonToBeAppended, ",\"content\":\"");
    STRING_concat_with_STRING(jsonToBeAppended, contentAsJSON);
    STRING_concat(jsonToBeAppended, "\"}]");

    // Write the formatted string
    LOGGER_HANDLE_DATA *handleData = (LOGGER_HANDLE_DATA *)moduleHandle;
    addJSONString(handleData->fout, STRING_c_str(jsonToBeAppended);
}
```

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się o sposobach używania zestawu SDK bramy IoT, zobacz następujące tematy:

- [Brama IoT SDK — wysyłanie wiadomości urządzenia do chmury z symulowanym urządzeniu z systemem Linux][lnk-gateway-simulated].
- [Brama IoT Azure SDK] [ lnk-gateway-sdk] na GitHub.

<!-- Links -->
[lnk-main-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/samples/hello_world/src/main.c
[lnk-helloworld-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/modules/hello_world/src/hello_world.c
[lnk-logger-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/modules/logger/src/logger.c
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/
[lnk-gateway-simulated]: ../articles/iot-hub/iot-hub-linux-gateway-sdk-simulated-device.md