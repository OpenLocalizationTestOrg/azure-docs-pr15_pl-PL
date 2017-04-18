<properties
   pageTitle="Podłącz urządzenie za pomocą C w systemie Linux | Microsoft Azure"
   description="Opisano, jak podłączyć urządzenie pakietu IoT Azure wstępnie zdalnego monitorowania rozwiązanie przy użyciu aplikacji napisanym w C uruchomionych Linux."
   services=""
   suite="iot-suite"
   documentationCenter="na"
   authors="dominicbetts"
   manager="timlt"
   editor=""/>

<tags
   ms.service="iot-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/05/2016"
   ms.author="dobett"/>


# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-linux"></a>Podłącz urządzenie do zdalnego monitorowania rozwiązanie wstępnie (Linux).

[AZURE.INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="build-and-run-a-sample-c-client-linux"></a>Tworzenie i uruchamianie klienta próbkę C Linux

W poniższych procedurach opisano sposób tworzenia aplikacji klienckiej, napisana C i wbudowane i przeprowadzić Ubuntu Linux, który komunikuje się z zdalnego monitorowania rozwiązanie wstępnie skonfigurowane. Aby wykonać te kroki, należy urządzenie działa Ubuntu wersji 15.04 lub 15.10. Przed kontynuowaniem, należy zainstalować pakiety wstępne na urządzeniu Ubuntu przy użyciu następującego polecenia:

```
sudo apt-get install cmake gcc g++
```

## <a name="install-the-client-libraries-on-your-device"></a>Instalowanie bibliotek klienta na urządzeniu

Centrum IoT Azure bibliotek klienta są dostępne w pakiecie, które można zainstalować na urządzeniu Ubuntu za pomocą polecenia **get stanie** . Wykonaj poniższe czynności, aby zainstalować pakiet zawierający Centrum IoT klienta biblioteki i nagłówek pliki na komputerze Ubuntu:

1. Dodawanie repozytorium AzureIoT do komputera:

    ```
    sudo add-apt-repository ppa:aziotsdklinux/ppa-azureiot
    sudo apt-get update
    ```

2. Zainstaluj pakiet azure-iot-sdk-c deweloperów

    ```
    sudo apt-get install -y azure-iot-sdk-c-dev
    ```

## <a name="add-code-to-specify-the-behavior-of-the-device"></a>Dodawanie kodu, aby określić zachowanie urządzenia

Utwórz folder o nazwie na komputerze Ubuntu **zdalnego\_monitorowania**. W **zdalnego\_monitorowania** folder utworzyć cztery pliki **main.c** **zdalnego\_monitoring.c**, **zdalnego\_monitoring.h**i **CMakeLists.txt**.

Za pomocą modelu bibliotek klienta serializatora Centrum IoT Określ format wiadomości, które urządzenie wysyła do Centrum IoT i poleceń, otrzymanej z Centrum IoT.

1. W edytorze tekstów, otwórz **zdalnego\_monitoring.c** pliku. Dodaj następujący `#include` instrukcje:

    ```
    #include "iothubtransportamqp.h"
    #include "schemalib.h"
    #include "iothub_client.h"
    #include "serializer.h"
    #include "schemaserializer.h"
    #include "azure_c_shared_utility/threadapi.h"
    #include "azure_c_shared_utility/platform.h"
    ```

2. Dodaj następujące deklaracji zmiennych po `#include` instrukcji. Zamienianie wartości symbol zastępczy [identyfikator urządzenia] i [klucza urządzenia] z wartościami dla swojego urządzenia z zdalnego monitorowania pulpitu nawigacyjnego rozwiązanie. Aby zamienić [nazwa IoTHub] za pomocą Hostname Centrum IoT z pulpitu nawigacyjnego. Na przykład jeśli nazwa hosta usługi IoT Centrum jest **contoso.azure devices.net**, Zastąp [nazwa IoTHub] **contoso**:

    ```
    static const char* deviceId = "[Device Id]";
    static const char* deviceKey = "[Device Key]";
    static const char* hubName = "IoTHub Name]";
    static const char* hubSuffix = "azure-devices.net";
    ```

3. Dodaj następujący kod w celu zdefiniowania modelu, który umożliwia komunikowanie się za pomocą Centrum IoT na tym urządzeniu. Ten model Określa, że urządzenie wysyła jako telemetrycznego temperatury, zewnętrznych temperatury, wilgotności i identyfikator urządzenia. Urządzenie wysyła również metadanych dotyczących urządzenia do koncentratora IoT, łącznie z listy dostępnych poleceń urządzenie obsługuje. To urządzenie odpowiada poleceń **SetTemperature** i **SetHumidity**:

    ```
    // Define the Model
    BEGIN_NAMESPACE(Contoso);

    DECLARE_STRUCT(SystemProperties,
      ascii_char_ptr, DeviceID,
      _Bool, Enabled
    );

    DECLARE_STRUCT(DeviceProperties,
      ascii_char_ptr, DeviceID,
      _Bool, HubEnabledState
    );

    DECLARE_MODEL(Thermostat,

      /* Event data (temperature, external temperature and humidity) */
      WITH_DATA(int, Temperature),
      WITH_DATA(int, ExternalTemperature),
      WITH_DATA(int, Humidity),
      WITH_DATA(ascii_char_ptr, DeviceId),

      /* Device Info - This is command metadata + some extra fields */
      WITH_DATA(ascii_char_ptr, ObjectType),
      WITH_DATA(_Bool, IsSimulatedDevice),
      WITH_DATA(ascii_char_ptr, Version),
      WITH_DATA(DeviceProperties, DeviceProperties),
      WITH_DATA(ascii_char_ptr_no_quotes, Commands),

      /* Commands implemented by the device */
      WITH_ACTION(SetTemperature, int, temperature),
      WITH_ACTION(SetHumidity, int, humidity)
    );

    END_NAMESPACE(Contoso);
    ```

### <a name="add-code-to-implement-the-behavior-of-the-device"></a>Dodawanie kodu do wykonania zachowanie urządzenia

Dodawanie funkcji do wykonania, gdy urządzenie odbierze polecenia z poziomu Centrum i kod, aby wysłać symulowany telemetrycznego do koncentratora.

1. Dodaj następujące funkcje, które wykonują, gdy urządzenie odbierze polecenia **SetTemperature** i **SetHumidity** zdefiniowane w modelu:

    ```
    EXECUTE_COMMAND_RESULT SetTemperature(Thermostat* thermostat, int temperature)
    {
      (void)printf("Received temperature %d\r\n", temperature);
      thermostat->Temperature = temperature;
      return EXECUTE_COMMAND_SUCCESS;
    }

    EXECUTE_COMMAND_RESULT SetHumidity(Thermostat* thermostat, int humidity)
    {
      (void)printf("Received humidity %d\r\n", humidity);
      thermostat->Humidity = humidity;
      return EXECUTE_COMMAND_SUCCESS;
    }
    ```

2. Dodaj następującą funkcję wysyłania wiadomości do Centrum IoT:

    ```
    static void sendMessage(IOTHUB_CLIENT_HANDLE iotHubClientHandle, const unsigned char* buffer, size_t size)
    {
      IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromByteArray(buffer, size);
      if (messageHandle == NULL)
      {
        printf("unable to create a new IoTHubMessage\r\n");
      }
      else
      {
        if (IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, NULL, NULL) != IOTHUB_CLIENT_OK)
        {
          printf("failed to hand over the message to IoTHubClient");
        }
        else
        {
          printf("IoTHubClient accepted the message for delivery\r\n");
        }

        IoTHubMessage_Destroy(messageHandle);
      }
    free((void*)buffer);
    }
    ```

3. Dodaj następującą funkcję, który przechwytuje w górę biblioteki szeregowania w zestawie SDK:

    ```
    static IOTHUBMESSAGE_DISPOSITION_RESULT IoTHubMessage(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
    {
      IOTHUBMESSAGE_DISPOSITION_RESULT result;
      const unsigned char* buffer;
      size_t size;
      if (IoTHubMessage_GetByteArray(message, &buffer, &size) != IOTHUB_MESSAGE_OK)
      {
        printf("unable to IoTHubMessage_GetByteArray\r\n");
        result = EXECUTE_COMMAND_ERROR;
      }
      else
      {
        /*buffer is not zero terminated*/
        char* temp = malloc(size + 1);
        if (temp == NULL)
        {
          printf("failed to malloc\r\n");
          result = EXECUTE_COMMAND_ERROR;
        }
        else
        {
          memcpy(temp, buffer, size);
          temp[size] = '\0';
          EXECUTE_COMMAND_RESULT executeCommandResult = EXECUTE_COMMAND(userContextCallback, temp);
          result =
            (executeCommandResult == EXECUTE_COMMAND_ERROR) ? IOTHUBMESSAGE_ABANDONED :
            (executeCommandResult == EXECUTE_COMMAND_SUCCESS) ? IOTHUBMESSAGE_ACCEPTED :
            IOTHUBMESSAGE_REJECTED;
          free(temp);
        }
      }
      return result;
    }
    ```

4. Dodaj następującą funkcję nawiązywanie połączenia z Centrum IoT, wysyłanie i odbieranie wiadomości i odłączyć od niego. Zwróć uwagę, jak urządzenie wysyła metadanych o sobie, łącznie z poleceniami, który obsługuje, do koncentratora IoT podczas łączenia. Metadane umożliwia rozwiązanie zaktualizować stan urządzenia z **systemem** na pulpicie nawigacyjnym:

    ```
    void remote_monitoring_run(void)
    {
      if (platform_init() != 0)
      {
        printf("Failed to initialize the platform.\r\n");
      }
      else
      {
        if (serializer_init(NULL) != SERIALIZER_OK)
        {
          printf("Failed on serializer_init\r\n");
        }
        else
        {
          IOTHUB_CLIENT_CONFIG config;
          IOTHUB_CLIENT_HANDLE iotHubClientHandle;

          config.deviceId = deviceId;
          config.deviceKey = deviceKey;
          config.deviceSasToken = NULL;
          config.iotHubName = hubName;
          config.iotHubSuffix = hubSuffix;
          config.protocol = AMQP_Protocol;
          iotHubClientHandle = IoTHubClient_Create(&config);
          if (iotHubClientHandle == NULL)
          {
            (void)printf("Failed on IoTHubClient_CreateFromConnectionString\r\n");
          }
          else
          {
            Thermostat* thermostat = CREATE_MODEL_INSTANCE(Contoso, Thermostat);
            if (thermostat == NULL)
            {
              (void)printf("Failed on CREATE_MODEL_INSTANCE\r\n");
            }
            else
            {
              STRING_HANDLE commandsMetadata;

              if (IoTHubClient_SetMessageCallback(iotHubClientHandle, IoTHubMessage, thermostat) != IOTHUB_CLIENT_OK)
              {
                printf("unable to IoTHubClient_SetMessageCallback\r\n");
              }
              else
              {

                /* send the device info upon startup so that the cloud app knows
                   what commands are available and the fact that the device is up */
                thermostat->ObjectType = "DeviceInfo";
                thermostat->IsSimulatedDevice = false;
                thermostat->Version = "1.0";
                thermostat->DeviceProperties.HubEnabledState = true;
                thermostat->DeviceProperties.DeviceID = (char*)deviceId;

                commandsMetadata = STRING_new();
                if (commandsMetadata == NULL)
                {
                  (void)printf("Failed on creating string for commands metadata\r\n");
                }
                else
                {
                  /* Serialize the commands metadata as a JSON string before sending */
                  if (SchemaSerializer_SerializeCommandMetadata(GET_MODEL_HANDLE(Contoso, Thermostat), commandsMetadata) != SCHEMA_SERIALIZER_OK)
                  {
                    (void)printf("Failed serializing commands metadata\r\n");
                  }
                  else
                  {
                    unsigned char* buffer;
                    size_t bufferSize;
                    thermostat->Commands = (char*)STRING_c_str(commandsMetadata);

                    /* Here is the actual send of the Device Info */
                    if (SERIALIZE(&buffer, &bufferSize, thermostat->ObjectType, thermostat->Version, thermostat->IsSimulatedDevice, thermostat->DeviceProperties, thermostat->Commands) != IOT_AGENT_OK)
                    {
                      (void)printf("Failed serializing\r\n");
                    }
                    else
                    {
                      sendMessage(iotHubClientHandle, buffer, bufferSize);
                    }

                  }

                  STRING_delete(commandsMetadata);
                }

                thermostat->Temperature = 50;
                thermostat->ExternalTemperature = 55;
                thermostat->Humidity = 50;
                thermostat->DeviceId = (char*)deviceId;

                while (1)
                {
                  unsigned char*buffer;
                  size_t bufferSize;

                  (void)printf("Sending sensor value Temperature = %d, Humidity = %d\r\n", thermostat->Temperature, thermostat->Humidity);

                  if (SERIALIZE(&buffer, &bufferSize, thermostat->DeviceId, thermostat->Temperature, thermostat->Humidity, thermostat->ExternalTemperature) != IOT_AGENT_OK)
                  {
                    (void)printf("Failed sending sensor value\r\n");
                  }
                  else
                  {
                    sendMessage(iotHubClientHandle, buffer, bufferSize);
                  }

                  ThreadAPI_Sleep(1000);
                }
              }

              DESTROY_MODEL_INSTANCE(thermostat);
            }
            IoTHubClient_Destroy(iotHubClientHandle);
          }
          serializer_deinit();
        }
        platform_deinit();
      }
    }
    ```
    
    Odwołanie Oto przykładowa wiadomość **DeviceInfo** wysyłane do Centrum IoT podczas uruchamiania:

    ```
    {
      "ObjectType":"DeviceInfo",
      "Version":"1.0",
      "IsSimulatedDevice":false,
      "DeviceProperties":
      {
        "DeviceID":"mydevice01", "HubEnabledState":true
      }, 
      "Commands":
      [
        {"Name":"SetHumidity", "Parameters":[{"Name":"humidity","Type":"double"}]},
        { "Name":"SetTemperature", "Parameters":[{"Name":"temperature","Type":"double"}]}
      ]
    }
    ```
    
    Odwołanie Oto przykładowa wiadomość **telemetrycznego** wysyłane do Centrum IoT:

    ```
    {"DeviceId":"mydevice01", "Temperature":50, "Humidity":50, "ExternalTemperature":55}
    ```
    
    Odwołanie Oto przykładowa otrzymania z Centrum IoT **polecenia** :
    
    ```
    {
      "Name":"SetHumidity",
      "MessageId":"2f3d3c75-3b77-4832-80ed-a5bb3e233391",
      "CreatedTime":"2016-03-11T15:09:44.2231295Z",
      "Parameters":{"humidity":23}
    }
    ```

### <a name="add-code-to-invoke-the-remotemonitoringrun-function"></a>Dodawanie kodu do wywołania funkcji remote_monitoring_run

W edytorze tekstów otwórz plik **remote_monitoring.h** . Dodaj następujący kod:

```
void remote_monitoring_run(void);
```

W edytorze tekstów otwórz plik **main.c** . Dodaj następujący kod:

```
#include "remote_monitoring.h"

int main(void)
{
    remote_monitoring_run();

    return 0;
}
```

## <a name="use-cmake-to-build-the-client-application"></a>Tworzenie aplikacji klienckiej przy użyciu CMake

W poniższej procedurze opisano sposób użycia *CMake* do tworzenia aplikacji klienta.

1. W edytorze tekstów otwórz plik **CMakeLists.txt** w folderze **remote_monitoring** .

2. Dodaj poniższe instrukcje, aby określić sposób tworzenia aplikacji klienckiej:

    ```
    cmake_minimum_required(VERSION 2.8.11)

    set(AZUREIOT_INC_FOLDER ".." "/usr/include/azureiot")

    include_directories(${AZUREIOT_INC_FOLDER})

    set(sample_application_c_files
        ./remote_monitoring.c
        ./main.c
    )

    set(sample_application_h_files
        ./remote_monitoring.h
    )

    add_executable(sample_app ${sample_application_c_files} ${sample_application_h_files})

    target_link_libraries(sample_app
        serializer
        iothub_client
        iothub_client_amqp_transport
        aziotsharedutil
        uamqp
        pthread
        curl
        ssl
        crypto
    )
    ```

3. W folderze **remote_monitoring** utworzenia folderu służącego do przechowywania plików *Upewnij* , które generuje CMake, a następnie uruchom polecenia **cmake** i **Wprowadź** w następujący sposób:

    ```
    mkdir cmake
    cd cmake
    cmake ../
    make
    ```

4. Uruchom aplikację klienta i wysyłanie telemetrycznego do koncentratora IoT:

    ```
    ./sample_app
    ```

[AZURE.INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

