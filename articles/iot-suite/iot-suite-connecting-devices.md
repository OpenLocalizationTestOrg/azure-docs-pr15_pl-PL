<properties
   pageTitle="Podłącz urządzenie za pomocą C w systemie Windows | Microsoft Azure"
   description="Opisano, jak podłączyć urządzenie pakietu IoT Azure wstępnie zdalnego monitorowania rozwiązanie przy użyciu aplikacji napisana C z systemem Windows."
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


# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-windows"></a>Podłącz urządzenie do zdalnego monitorowania rozwiązanie wstępnie (Windows)

[AZURE.INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="create-a-c-sample-solution-on-windows"></a>Utworzyć rozwiązanie przykładowe C w systemie Windows

Poniższe czynności pokazano, jak używać programu Visual Studio do tworzenia aplikacji klienckiej napisana C, która komunikuje się z zdalne monitorowanie wstępnie rozwiązanie.

Tworzenie projektu starter w Visual Studio 2015 i Dodaj pakiety NuGet Centrum IoT urządzenia klienta:

1. W Visual Studio 2015 r. utworzyć aplikację konsoli C przy użyciu szablonu Visual C++ **Aplikacji konsoli Win32** . Nazwa projektu **RMDevice**.

2. Na stronie **Ustawień aplikacji** w **Kreatorze aplikacji Win32**upewnij się, że **aplikacji konsoli** jest zaznaczone, a następnie wyczyść pole wyboru **nagłówka Precompiled** i **kontroli cyklu opracowywania zabezpieczeń (SDL)**.

3. W **Eksploratorze rozwiązań**Usuń pliki stdafx.h, targetver.h i stdafx.cpp.

4. W **Eksploratorze rozwiązań**Zmień nazwę pliku RMDevice.cpp na RMDevice.c.

5. W **Eksploratorze rozwiązań**kliknij prawym przyciskiem myszy nad projektem **RMDevice** , a następnie kliknij przycisk **Zarządzanie NuGet pakietów**. Kliknij przycisk **Przeglądaj**, a następnie wyszukaj i zainstaluj następujące pakiety NuGet do projektu:

    - Microsoft.Azure.IoTHub.Serializer
    - Microsoft.Azure.IoTHub.IoTHubClient
    - Microsoft.Azure.IoTHub.HttpTransport

6. W **Eksploratorze rozwiązań**kliknij prawym przyciskiem myszy nad projektem **RMDevice** , a następnie kliknij przycisk **Właściwości** , aby otworzyć okno dialogowe **Właściwości strony** projektu. Aby uzyskać szczegółowe informacje, zobacz [Ustawianie właściwości projektu Visual C++][lnk-c-project-properties]. 

7. Kliknij folder **łączące** , a następnie kliknij pozycję Strona właściwości **danych wejściowych** .

8. Dodawanie **crypt32.lib** do właściwości **Dodatkowe zależności** . Kliknij **przycisk OK** , a następnie **przycisk OK** ponownie, aby zapisać wartości właściwości projektu.

## <a name="specify-the-behavior-of-the-iot-hub-device"></a>Określanie zachowania urządzenia Centrum IoT

Za pomocą modelu bibliotek klienta Centrum IoT Określ format wiadomości, które urządzenie wysyła do Centrum IoT i poleceń, otrzymanej z Centrum IoT.

1. W programie Visual Studio Otwórz plik RMDevice.c. Zamienić istniejący `#include` instrukcje z następujący kod:

    ```
    #include "iothubtransporthttp.h"
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
    static const char* hubName = "[IoTHub Name]";
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

## <a name="implement-the-behavior-of-the-device"></a>Implementowanie zachowanie urządzenia

Teraz Dodaj kod, który wykonuje zachowanie zdefiniowane w modelu.

1. Dodaj następujące funkcje, które wykonują, gdy urządzenie odbierze polecenia **SetTemperature** i **SetHumidity** z Centrum IoT:

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
        config.iotHubName = hubName;
        config.iotHubSuffix = hubSuffix;
        config.protocol = HTTP_Protocol;
        config.deviceSasToken = NULL;
        config.protocolGatewayHostName = NULL;
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

5. Zastąp **główna** funkcja następujący kod w celu wywołania funkcji **remote_monitoring_run** :

    ```
    int main()
    {
      remote_monitoring_run();
      return 0;
    }
    ```

6. Kliknij przycisk **Konstruuj** , a następnie **Utworzyć rozwiązanie** do tworzenia aplikacji urządzenia.

7. W **Eksploratorze rozwiązań**kliknij prawym przyciskiem myszy **RMDevice** projektu, kliknij pozycję **Debugowanie**, a następnie kliknij **Uruchom nowe wystąpienie** próbki. Konsola wyświetla wiadomości aplikację wysyła telemetrycznego próbki do koncentratora IoT i odbiera poleceń.

[AZURE.INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]


[lnk-c-project-properties]: https://msdn.microsoft.com/library/669zx6zc.aspx