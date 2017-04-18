<properties
 pageTitle="Jak przeprowadzić aktualizację oprogramowania układowego | Microsoft Azure"
 description="W tym samouczku pokazano sposobów wykonywania aktualizacji oprogramowania"
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

# <a name="tutorial-how-to-do-a-firmware-update-preview"></a>Samouczek: Jak wykonać sprzętowego aktualizacji (wersja preview)

## <a name="introduction"></a>Wprowadzenie
[Wprowadzenie do zarządzania urządzeniami] [ lnk-dm-getstarted] samouczka widać, jak za pomocą [urządzenia podwójnego] [ lnk-devtwin] i [metody (C2D) w chmurze do urządzenia] [ lnk-c2dmethod] pierwotnych zdalne ponowne uruchomienie urządzenia. Ten samouczek korzysta z tym samym pierwotnych Centrum IoT ten artykuł zawiera wskazówki i pokazano, jak przeprowadzić aktualizację sprzętowego symulowany zakończenia do końca.  Ten wzorzec jest używane w implementacji aktualizacji oprogramowania układowego dla próbki urządzenia Edison firmy Intel.

W tym samouczku pokazano sposób do:

- Tworzenie aplikacji konsoli, który wywołuje metodę bezpośredni firmwareUpdate na tym urządzeniu symulowany za pośrednictwem Centrum usługi IoT.
- Tworzenie symulowany urządzenie, które wykonuje firmwareUpdate metoda bezpośrednia, który połączy Cię przez proces wielu etapach oczekuje Pobierz obraz sprzętowego, pobraniu obrazu sprzętowego, a na koniec dotyczy tą sprzętowego obrazu.  W całym wykonywanie każdego etapu zastosowania urządzenia podwójnego zgłoszone właściwości, aby zaktualizować o postępie.

Na końcu tego samouczka masz dwie aplikacji konsoli Node.js:

**dmpatterns_fwupdate_service.js**, który wymaga metoda bezpośrednia na tym urządzeniu symulowany, wyświetla odpowiedzi i okresowo (co 500ms) Wyświetla podwójnego zaktualizowany zgłoszone właściwości.

**dmpatterns_fwupdate_device.js**, który łączy się z Centrum IoT z urządzenia tożsamości utworzony wcześniej, otrzyma firmwareUpdate metoda bezpośrednia, jest uruchomiony proces wielokrotnego stanu w celu zasymulowania tym aktualizacji oprogramowania układowego: Oczekiwanie na pobieranie obrazu, pobierając nowy obraz i na końcu zastosowania obrazu.


Aby użyć tego samouczka, są potrzebne następujące elementy:

Wersja node.js 0.12.x lub nowszym <br/>  [Przygotowywanie środowiska programowania] [ lnk-dev-setup] zawiera opis sposobu instalowania Node.js ten samouczek w systemach Windows i Linux.

Konto Azure active. (Jeśli nie masz konta, możesz utworzyć [bezpłatne konto] [ lnk-free-trial] na kilka minut.)

Wykonaj o [Wprowadzenie do zarządzania urządzeniami](iot-hub-device-management-get-started.md) w artykule Tworzenie Twoim Centrum IoT i uzyskiwanie ciąg połączenia.

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-a-simulated-device-app"></a>Tworzenie aplikacji symulacji

W tej sekcji możesz utworzyć aplikację konsoli Node.js, która odpowiada metoda bezpośrednia o nazwie w chmurze, wyzwalające symulacji aktualizacji oprogramowania i zastosowania podwójnego urządzenia zgłoszone właściwości, aby umożliwić kwerendy podwójnego urządzenia do identyfikowania urządzenia i po ich ostatniego ponownego uruchomienia.

1. Utwórz nowy pusty folder o nazwie **manageddevice**.  W folderze **manageddevice** Utwórz plik package.json przy użyciu następującego polecenia w wierszu polecenia.  Zaakceptuj wszystkie wartości domyślne:

    ```
    npm init
    ```
    
2. W swojej wiersza polecenia w folderze **manageddevice** , uruchom następujące polecenie, aby zainstalować **azure-iot-device@dtpreview** pakiet SDK urządzenia i **azure-iot-device-mqtt@dtpreview** pakietu:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. Przy użyciu edytora tekstu, Utwórz nowy plik **dmpatterns_fwupdate_device.js** w folderze **manageddevice** .

4. Dodaj następujące "Wymagaj' instrukcje na początku pliku **dmpatterns_fwupdate_device.js** :

    ```
    'use strict';

    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```

5. Dodawanie zmiennej **connectionString** i za jej pomocą tworzyć klienta urządzenia.  

    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myDeviceId;SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

6. Dodaj następujący funkcji, która będzie używana do aktualizacji podwójnego zgłoszone właściwości

    ```
    var reportFWUpdateThroughTwin = function(twin, firmwareUpdateValue) {
      var patch = {
          iothubDM : {
            firmwareUpdate : firmwareUpdateValue
          }
      };

      twin.properties.reported.update(patch, function(err) {
        if (err) throw err;
        console.log('twin state reported')
      });
    };
    ```
    
7. Dodaj następujące funkcje, które będą zasymulowania pobieranie i stosowanie obrazu oprogramowania układowego.

    ```
    var simulateDownloadImage = function(imageUrl, callback) {
      var error = null;
      var image = "[fake image data]";
      
      console.log("Downloading image from " + imageUrl);
      
      callback(error, image);
    }

    var simulateApplyImage = function(imageData, callback) {
      var error = null;
      
      if (!imageData) {
        error = {message: 'Apply image failed because of missing image data.'};
      }
      
      callback(error);
    }
    ```

8. Dodaj następujący funkcji zaktualizuje stan aktualizacji oprogramowania układowego za pośrednictwem podwójnego zgłoszone właściwości Trwa oczekiwanie na pobieranie.  Zazwyczaj urządzeń uzyskają aktualizacji niedostępny, a przyczyny zasad zdefiniowane przez administratora urządzenia, aby rozpocząć pobieranie i stosowanie aktualizacji.  Jest to miejsce, w którym chcesz uruchomić logiczny, aby włączyć tę zasadę.  Dla uproszczenia możemy uczestniczysz opóźnienia na cztery sekundy i kontynuowanie Pobierz obraz oprogramowania układowego. 

    ```
    var waitToDownload = function(twin, fwPackageUriVal, callback) {
      var now = new Date();
      
      reportFWUpdateThroughTwin(twin, {
        fwPackageUri: fwPackageUriVal,
        status: 'waiting',
        error : null,
        startedWaitingTime : now.toISOString()
      });
      setTimeout(callback, 4000);
    };
    ```
    
9. Dodaj następujący funkcji zaktualizuje stan aktualizacji oprogramowania układowego za pośrednictwem podwójnego przekazane właściwości pobierania obrazu oprogramowania układowego.  Występuje w górę przez symulowanie pobranie oprogramowania układowego, a na koniec aktualizuje stan aktualizacji oprogramowania układowego informowanie pobierania sukcesu lub niepowodzenia.

    ```
    var downloadImage = function(twin, fwPackageUriVal, callback) {
      var now = new Date();   
      
      reportFWUpdateThroughTwin(twin, {
        status: 'downloading',
      });
      
      setTimeout(function() {
        // Simulate download
        simulateDownloadImage(fwPackageUriVal, function(err, image) {
          
          if (err)
          {
            reportFWUpdateThroughTwin(twin, {
              status: 'downloadfailed',
              error: {
                code: error_code,
                message: error_message,
              }
            });
          }
          else {        
            reportFWUpdateThroughTwin(twin, {
              status: 'downloadComplete',
              downloadCompleteTime: now.toISOString(),
            });
            
            setTimeout(function() { callback(image); }, 4000);   
          }
        });
        
      }, 4000);
    }
    ```
    
10. Dodaj następujący funkcji zaktualizuje stan aktualizacji oprogramowania układowego za pośrednictwem podwójnego zgłoszone właściwości do zastosowania obrazu oprogramowania układowego.  Występuje w górę, symulacji, stosując obraz sprzętowego, a na koniec aktualizuje stan aktualizacji oprogramowania układowego informowanie Zastosuj sukcesu lub niepowodzenia.

    ```
    var applyImage = function(twin, imageData, callback) {
      var now = new Date();   
      
      reportFWUpdateThroughTwin(twin, {
        status: 'applying',
        startedApplyingImage : now.toISOString()
      });
      
      setTimeout(function() {
        
        // Simulate apply firmware image
        simulateApplyImage(imageData, function(err) {
          if (err) {
            reportFWUpdateThroughTwin(twin, {
              status: 'applyFailed',
              error: {
                code: err.error_code,
                message: err.error_message,
              }
            });
          } else { 
            reportFWUpdateThroughTwin(twin, {
              status: 'applyComplete',
              lastFirmwareUpdate: now.toISOString()
            });    
            
          }
        });
        
        setTimeout(callback, 4000);
        
      }, 4000);
    }
    ```

11. Dodaj następujące functoin, obsługi metody firmwareUpdate i zainicjować proces aktualizacji oprogramowania układowego wielu etapach.

    ```
    var onFirmwareUpdate = function(request, response) {
            
      // Respond the cloud app for the direct method
      response.send(200, 'FirmwareUpdate started', function(err) {
        if (!err) {
          console.error('An error occured when sending a method response:\n' + err.toString());
        } else {
          console.log('Response to method \'' + request.methodName + '\' sent successfully.');
        }
      });

      // Get the parameter from the body of the method request
      var fwPackageUri = JSON.parse(request.payload).fwPackageUri;

      // Obtain the device twin
      client.getTwin(function(err, twin) {
        if (err) {
          console.error('Could not get device twin.');
        } else {
          console.log('Device twin acquired.');
          
          // Start the multi-stage firmware update
          waitToDownload(twin, fwPackageUri, function() {
            downloadImage(twin, fwPackageUri, function(imageData) {
              applyImage(twin, imageData, function() {});    
            });  
          });

        }
      });
    }
    ```
    
12. Na koniec Dodaj następujący kod, który łączy do koncentratora IoT jako urządzenie, 

    ```
    client.open(function(err) {
      if (err) {
        console.error('Could not connect to IotHub client');
      }  else {
        console.log('Client connected to IoT Hub.  Waiting for firmwareUpdate direct method.');
      }
      
      client.onDeviceMethod('firmwareUpdate', onFirmwareUpdate(request, response));
    });
    ```

> [AZURE.NOTE] Aby zachować proste czynności, ten samouczek nie powoduje żadnych zasad ponów próbę. W kodzie produkcyjnym wykonała zasady ponów próbę (na przykład wykładniczego wycofywania), sugerowanej w artykule w witrynie MSDN [Przejściowych obsługi błędów][lnk-transient-faults].

## <a name="trigger-a-remote-firmware-update-on-the-device-using-a-direct-method"></a>Wyzwalanie aktualizacji oprogramowania zdalnego na tym urządzeniu, przy użyciu metody bezpośrednie 

W tej sekcji możesz utworzyć aplikację konsoli Node.js, która inicjuje aktualizacji oprogramowania zdalnego na urządzeniu metodą bezpośredni i używa kwerend podwójnego urządzenia okresowo uzyskanie stanu aktualizacji oprogramowania układowego aktywne na tym urządzeniu.


1. Utwórz nowy pusty folder o nazwie **triggerfwupdateondevice**.  W folderze **triggerfwupdateondevice** Utwórz plik package.json przy użyciu następującego polecenia w wierszu polecenia.  Zaakceptuj wszystkie wartości domyślne:

    ```
    npm init
    ```
    
2. W swojej wiersza polecenia w folderze **triggerfwupdateondevice** uruchom następujące polecenie, aby zainstalować pakiet SDK urządzenia **azure iothub** i pakiet **azure-iot urządzenia — mqtt** :

    ```
    npm install azure-iot-hub@dtpreview --save
    ```
    
3. Przy użyciu edytora tekstu, Utwórz nowy plik **dmpatterns_getstarted_service.js** w folderze **triggerfwupdateondevice** .

4. Dodaj następujące "Wymagaj' instrukcje na początku pliku **dmpatterns_getstarted_service.js** :

    ```
    'use strict';

    var Registry = require('azure-iothub').Registry;
    var Client = require('azure-iothub').Client;
    ```

5. Dodaj następujące deklaracji zmiennych i Zamień wartości symbolu zastępczego.

    ```
    var connectionString = '{device_connectionstring}';
    var registry = Registry.fromConnectionString(connectionString);
    var client = Client.fromConnectionString(connectionString);
    var deviceToUpdate = 'myDeviceId';
    ```
    
6. Dodaj następującą funkcję, aby znaleźć i wyświetlania wartości firmwareUpdate zgłoszone właściwości.

    ```
    var queryTwinFWUpdateReported = function() {
        registry.getTwin(deviceToUpdate, function(err, twin){
            if (err) {
              console.error('Could not query twins: ' + err.constructor.name + ': ' + err.message);
            } else {
              console.log((JSON.stringify(twin.properties.reported.iothubDM.firmwareUpdate)) + "\n");
            }
        });
    };
    ```

7. Dodaj następującą funkcję do wywołania metody firmwareUpdate ponowne uruchomienie urządzenia:

    ```
    var startFirmwareUpdateDevice = function() {
      var params = {
          fwPackageUri: 'https://secureurl'
      };
      
      var methodName = "firmwareUpdate";
      var payloadData =  JSON.stringify(params);
      
      var methodParams = {
        methodName: methodName,
        payload: payloadData,
        timeoutInSeconds: 30
      };
      
      client.invokeDeviceMethod(deviceToUpdate, methodParams, function(err, result) {
        if (err) {
          console.error('Could not start the firmware update on the device: ' + err.message)
        } 
      });
    };
    ```

8. Na koniec Dodaj następującą funkcję do kodu, aby rozpocząć oprogramowania układowego aktualizację sekwencji i rozpoczęcia okresowo wyświetlania podwójnego zgłoszone właściwości:

    ```
    startFirmwareUpdateDevice();
    setInterval(queryTwinFWUpdateReported, 500);
    ```
    
9. Zapisz i zamknij plik **dmpatterns_fwupdate_service.js** .

## <a name="run-the-applications"></a>Uruchamianie aplikacji

Teraz możesz przystąpić do uruchamiania aplikacji.

1. W wierszu polecenia — w folderze **manageddevice** , uruchom następujące polecenie, aby rozpocząć wykrywanie metoda bezpośrednia Uruchom ponownie komputer.

    ```
    node dmpatterns_fwupdate_device.js
    ```

2. W dolnym wierszu polecenia w folderze **triggerfwupdateondevice** uruchom następujące polecenie, aby Uruchom ponownie komputer zdalny i kwerenda podwójnego urządzenia znaleźć ostatnio Uruchom ponownie czasu.

    ```
    node dmpatterns_fwupdate_service.js
    ```

3. Reagowanie na metoda bezpośrednia będą widzieli pod drukowanie wiadomości

## <a name="next-steps"></a>Następne kroki

W tym samouczku używana metoda bezpośrednia wyzwalać aktualizacji oprogramowania zdalnego na urządzeniu i okresowo używane zgłoszone podwójnego proces aktualizacji właściwości, aby poznać informacje o postępie oprogramowania układowego.  

Aby dowiedzieć się, jak zwiększyć usługi IoT metody rozwiązania i planowanie połączeń na wielu urządzeniach, zobacz [Planowanie i emisji zadania] [ lnk-tutorial-jobs] samouczka.

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-dm-getstarted]: iot-hub-device-management-get-started.md
[lnk-tutorial-jobs]: iot-hub-schedule-jobs.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
