<properties
 pageTitle="Jak planować zadania | Microsoft Azure"
 description="W tym samouczku pokazano sposobu planowania zadań"
 services="iot-hub"
 documentationCenter=".net"
 authors="juanjperez"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="juanpere"/>

# <a name="tutorial-schedule-and-broadcast-jobs-preview"></a>Samouczek: Planowanie i emitowanie zadań (wersja preview)

## <a name="introduction"></a>Wprowadzenie
Azure Centrum IoT jest w pełni zarządzane usługa, która pozwala wewnętrznej aplikacji do tworzenia i śledzenie zadań, które planowanie i aktualizowanie miliony urządzenia.  Zadania może być używany do następujących czynności:

- Aktualizowanie właściwości podwójnego potrzeby urządzenia
- Aktualizowanie urządzenia podwójnego znaczników
- Wywoływanie metody chmury do urządzenia

Koncepcji zadania jest zawijany jedną z następujących czynności i śledzenie postępu realizacji z zestawem urządzeń, która jest zdefiniowana przez kwerendę podwójnego.  Na przykład przy użyciu zadania wewnętrznej aplikacji może wywołać metody Uruchom ponownie komputer, na urządzeniach 10 000, określone przez kwerendę podwójnego i zaplanowane w przyszłości.  Ta aplikacja następnie możesz śledzić postęp każde z tych urządzeń odbieranie i wykonać metody Uruchom ponownie komputer.

Dowiedz się więcej o każdej z tych funkcji w następujących artykułach:

- Podwójnego urządzenia i właściwości: [Wprowadzenie do podwójnego] [ lnk-get-started-twin] i [Samouczek: jak za pomocą właściwości podwójnego][lnk-twin-props]
- Metody chmury do urządzenia: [Przewodnik dewelopera — bezpośrednie metod] [ lnk-dev-methods] i [Samouczek: metody C2D][lnk-c2d-methods]

W tym samouczku pokazano sposób do:

- Tworzenie symulowany urządzenie ma bezpośrednich metodę, która umożliwia lockDoor, który może zostać wywołany przez aplikację ponownie zakończenia.
- Tworzenie aplikacji konsoli wywołuje metodę bezpośredni lockDoor na tym urządzeniu symulowane przy użyciu zadania i aktualizuje właściwości podwójnego potrzeby przy użyciu zadania urządzenia.

Na końcu tego samouczka masz dwie aplikacji konsoli Node.js:

**simDevice.js**, który łączy się z koncentratora IoT za pomocą urządzenia tożsamości i odbiera metoda bezpośrednia lockDoor.

**scheduleJobService.js**, które połączeń metoda bezpośrednia na urządzeniu symulowany i aktualizowania właściwości disired podwójnego przy użyciu zadania.

Aby użyć tego samouczka, są potrzebne następujące elementy:

Wersja node.js 0.12.x lub nowszym <br/>  [Przygotowywanie środowiska programowania] [ lnk-dev-setup] zawiera opis sposobu instalowania Node.js ten samouczek w systemach Windows i Linux.

Konto Azure active. (Jeśli nie masz konta, możesz utworzyć [bezpłatne konto] [ lnk-free-trial] na kilka minut.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-a-simulated-device-app"></a>Tworzenie aplikacji symulacji

W tej sekcji możesz utworzyć aplikację konsoli Node.js, która odpowiada metoda bezpośrednia o nazwie w chmurze, wyzwalające symulacji, uruchom ponownie komputer i zastosowania podwójnego urządzenia zgłoszone właściwości, aby umożliwić kwerendy podwójnego urządzenia do identyfikowania urządzenia i po ich ostatniego ponownego uruchomienia.

1. Utwórz nowy pusty folder o nazwie **simDevice**.  W folderze **simDevice** Utwórz plik package.json przy użyciu następującego polecenia w wierszu polecenia.  Zaakceptuj wszystkie wartości domyślne:

    ```
    npm init
    ```
    
2. W swojej wiersza polecenia w folderze **simDevice** uruchom następujące polecenie, aby zainstalować pakiet SDK urządzenia **azure iot urządzeń** i pakiet **azure-iot urządzenia — mqtt** :

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. Przy użyciu edytora tekstu, Utwórz nowy plik **simDevice.js** w folderze **simDevice** .

4. Dodaj następujące 'Wymagaj' instrukcje na początku pliku **simDevice.js** :

    ```
    'use strict';

    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```

5. Dodawanie zmiennej **connectionString** i za jej pomocą tworzyć klienta urządzenia.  

    ```
    var connectionString = 'HostName={youriothostname};DeviceId={yourdeviceid};SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

6. Dodaj następującą funkcję obsługi metody lockDoor.

    ```
    var onLockDoor = function(request, response) {
        
        // Respond the cloud app for the direct method
        response.send(200, function(err) {
            if (!err) {
                console.error('An error occured when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.');
            }
        });
        
        console.log('Locking Door!');
    };
    ```

7. Dodaj następujący kod, aby zarejestrować obsługi dla metody lockDoor.

    ```
    client.open(function(err) {
        if (err) {
            console.error('Could not connect to IotHub client.');
        }  else {
            console.log('Client connected to IoT Hub.  Waiting for reboot direct method.');
            client.onDeviceMethod('lockDoor', onLockDoor);
        }
    });
    ```
    
8. Zapisz i zamknij plik **simDevice.js** .

> [AZURE.NOTE] Aby zachować proste czynności, ten samouczek nie powoduje żadnych zasad ponów próbę. W kodzie produkcyjnym wykonała zasady ponów próbę (na przykład wykładniczego wycofywania), sugerowanej w artykule w witrynie MSDN [Przejściowych obsługi błędów][lnk-transient-faults].

## <a name="schedule-jobs-for-calling-a-direct-method-and-updating-a-twins-properties"></a>Planowanie zadania bezpośrednie metody i właściwości podwójnego aktualizacji

W tej sekcji możesz utworzyć aplikację konsoli Node.js, który inicjuje zdalnego lockDoor na urządzeniu metodą bezpośredni i aktualizowania właściwości podwójnego.

1. Utwórz nowy pusty folder o nazwie **scheduleJobService**.  W folderze **scheduleJobService** Utwórz plik package.json przy użyciu następującego polecenia w wierszu polecenia.  Zaakceptuj wszystkie wartości domyślne:

    ```
    npm init
    ```
    
2. W swojej wiersza polecenia w folderze **scheduleJobService** uruchom następujące polecenie, aby zainstalować pakiet SDK urządzenia **azure iothub** i pakiet **azure-iot urządzenia — mqtt** :

    ```
    npm install azure-iothub@dtpreview uuid --save
    ```
    
3. Przy użyciu edytora tekstu, Utwórz nowy plik **scheduleJobService.js** w folderze **scheduleJobService** .

4. Dodaj następujące "Wymagaj' instrukcje na początku pliku **dmpatterns_gscheduleJobServiceetstarted_service.js** :

    ```
    'use strict';

    var uuid = require('uuid');
    var JobClient = require('azure-iothub').JobClient;
    ```

5. Dodaj następujące deklaracji zmiennych i Zamień wartości symbolu zastępczego.

    ```
    var connectionString = '{iothubconnectionstring}';
    var deviceArray = ['myDeviceId'];
    var startTime = new Date();
    var maxExecutionTimeInSeconds =  3600;
    var jobClient = JobClient.fromConnectionString(connectionString);
    ```
    
6. Dodaj następującą funkcję używany do monitorowania wykonania zadania:

    ```
    function monitorJob (jobId, callback) {
        var jobMonitorInterval = setInterval(function() {
            jobClient.getJob(jobId, function(err, result) {
            if (err) {
                console.error('Could not get job status: ' + err.message);
            } else {
                console.log('Job: ' + jobId + ' - status: ' + result.status);
                if (result.status === 'completed' || result.status === 'failed' || result.status === 'cancelled') {
                clearInterval(jobMonitorInterval);
                callback(null, result);
                }
            }
            });
        }, 5000);
    }
    ```

7. Dodaj następujący kod, aby zaplanować zadanie, które wywołuje metodę urządzenia:

    ```
    var methodParams = {
        methodName: 'lockDoor',
        payload: null,
        timeoutInSeconds: 45
    };

    var methodJobId = uuid.v4();
    console.log('scheduling Device Method job with id: ' + methodJobId);
    jobClient.scheduleDeviceMethod(methodJobId,
                                deviceArray,
                                methodParams,
                                startTime,
                                maxExecutionTimeInSeconds,
                                function(err) {
        if (err) {
            console.error('Could not schedule device method job: ' + err.message);
        } else {
            monitorJob(methodJobId, function(err, result) {
                if (err) {
                    console.error('Could not monitor device method job: ' + err.message);
                } else {
                    console.log(JSON.stringify(result, null, 2));
                }
            });
        }
    });
    ```
        
8. Dodaj następujący kod, aby zaplanować zadanie, aby zaktualizować podwójnego:

    ```
    var twinPatch = {
        etag: '*',
        desired: {
            building: '43',
            floor: 3
        }
    };

    var twinJobId = uuid.v4();

    console.log('scheduling Twin Update job with id: ' + twinJobId);
    jobClient.scheduleTwinUpdate(twinJobId,
                                deviceArray,
                                twinPatch,
                                startTime,
                                maxExecutionTimeInSeconds,
                                function(err) {
        if (err) {
            console.error('Could not schedule twin update job: ' + err.message);
        } else {
            monitorJob(twinJobId, function(err, result) {
                if (err) {
                    console.error('Could not monitor twin update job: ' + err.message);
                } else {
                    console.log(JSON.stringify(result, null, 2));
                }
            });
        }
    });
    ```
    
9. Zapisz i zamknij plik **scheduleJobService.js** .

## <a name="run-the-applications"></a>Uruchamianie aplikacji

Teraz możesz przystąpić do uruchamiania aplikacji.

1. W wierszu polecenia — w folderze **simDevice** , uruchom następujące polecenie, aby rozpocząć wykrywanie metoda bezpośrednia Uruchom ponownie komputer.

    ```
    node simDevice.js
    ```

2. W dolnym wierszu polecenia w folderze **scheduleJobService** uruchom następujące polecenie, aby Uruchom ponownie komputer zdalny i kwerenda podwójnego urządzenia znaleźć ostatnio Uruchom ponownie czasu.

    ```
    node scheduleJobService.js
    ```

3. Zostaną wyświetlone informacje z zarówno urządzenia i wewnętrznej aplikacji.


## <a name="next-steps"></a>Następne kroki

W tym samouczku zadanie jest używana podczas planowania metodę bezpośrednio do urządzenia i Aktualizuj właściwości podwójnego urządzenia.

Aby kontynuować, wprowadzenie do Centrum IoT i wzorców zarządzania urządzenia, takich jak zdalnego przez aktualizację oprogramowania układowego air, zobacz:

[Samouczek: Jak wykonać aktualizacji oprogramowania][lnk-fwupdate]

Aby kontynuować, wprowadzenie do Centrum IoT, zobacz [Wprowadzenie do SDK bramy IoT][lnk-gateway-SDK].

[lnk-get-started-twin]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-props]: iot-hub-node-node-twin-how-to-configure.md
[lnk-c2d-methods]: iot-hub-c2d-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-fwupdate]: iot-hub-firmware-update.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx