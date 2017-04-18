<properties
 pageTitle="Wprowadzenie do zarządzania urządzeniami | Microsoft Azure"
 description="Ten samouczek pokazano, jak rozpocząć pracę z zarządzania urządzeniami w Centrum IoT Azure"
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

# <a name="tutorial-get-started-with-device-management-preview"></a>Samouczek: Wprowadzenie do zarządzania urządzeniami (wersja preview)

## <a name="introduction"></a>Wprowadzenie
Aplikacje w chmurze IoT umożliwia pierwotnych w Centrum IoT Azure, to znaczy podwójnego urządzenia i bezpośredni metody, zdalne uruchamianie i monitorowanie urządzenia zarządzania akcje na urządzeniach.  Ten artykuł zawiera wskazówki i kod dla działania aplikacji chmury IoT i urządzenia razem na inicjowanie i uruchom ponownie komputer zdalny urządzenia za pomocą Centrum IoT można monitorować.

Aby zdalnie uruchomić i monitorowania działań zarządzania urządzenie na urządzeniach z aplikacji opartej na chmurze, wewnętrznej, za pomocą Centrum IoT Azure pierwotnych, takich jak [podwójnego urządzenia] [ lnk-devtwin] i [metody (C2D) w chmurze do urządzenia][lnk-c2dmethod]. W tym samouczku pokazano możesz jak aplikacji wewnętrznej i urządzenia można wspólnie pracować umożliwiającym rozpoczęcie tworzenia i monitorować Uruchom ponownie komputer zdalny urządzenia z Centrum IoT.

Metoda C2D umożliwia rozpocząć działania zarządzania urządzenia (na przykład uruchom ponownie komputer, fabryczne i aktualizacji oprogramowania układowego) za pomocą aplikacji wewnętrznej w chmurze. Urządzenie jest odpowiedzialny za:

- Obsługa żądania metoda jest wysyłana z Centrum IoT.
- Inicjowanie żadnej akcji określonych urządzeń na tym urządzeniu.
- Dostarczanie aktualizacji stanu za pośrednictwem podwójnego urządzenia zgłoszone właściwości do koncentratora IoT.

Za pomocą aplikacji wewnętrznej w chmurze do uruchomienia kwerendy podwójnego urządzenia do raportowania postępu działań zarządzania urządzenia.

W tym samouczku pokazano sposób do:

- Azure portal umożliwia tworzenie Centrum IoT i tworzenie urządzenia tożsamości w Twoim Centrum IoT.
- Tworzenie symulowany urządzenie ma bezpośrednich metodę, która umożliwia Uruchom ponownie komputer, który może być wywoływana przez chmury.
- Tworzenie aplikacji konsoli wywołuje na tym urządzeniu symulowany za pośrednictwem Centrum usługi IoT metoda bezpośrednia Uruchom ponownie komputer.

Na końcu tego samouczka masz dwie aplikacji konsoli Node.js:

**dmpatterns_getstarted_device.js**, który łączy się z Centrum IoT z urządzenia tożsamości utworzony wcześniej, otrzyma metoda bezpośrednia Uruchom ponownie komputer, symuluje fizycznie Uruchom ponownie komputer i raporty czasu dla ostatniego Uruchom ponownie komputer.

**dmpatterns_getstarted_service.js**, połączeń metoda bezpośrednia na urządzeniu symulowany, wyświetla odpowiedzi i wyświetla podwójnego zaktualizowany zgłoszone właściwości.

Aby użyć tego samouczka, są potrzebne następujące elementy:

Wersja node.js 0.12.x lub nowszym <br/>  [Przygotowywanie środowiska programowania] [ lnk-dev-setup] zawiera opis sposobu instalowania Node.js ten samouczek w systemach Windows i Linux.

Konto Azure active. (Jeśli nie masz konta, możesz utworzyć [bezpłatne konto] [ lnk-free-trial] na kilka minut.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-a-simulated-device-app"></a>Tworzenie aplikacji symulacji

W tej sekcji możesz utworzyć aplikację konsoli Node.js, która odpowiada metoda bezpośrednia o nazwie w chmurze, wyzwalające symulacji, uruchom ponownie komputer i zastosowania podwójnego urządzenia zgłoszone właściwości, aby umożliwić kwerendy podwójnego urządzenia do identyfikowania urządzenia i po ich ostatniego ponownego uruchomienia.

1. Utwórz nowy pusty folder o nazwie **manageddevice**.  W folderze **manageddevice** Utwórz plik package.json przy użyciu następującego polecenia w wierszu polecenia.  Zaakceptuj wszystkie wartości domyślne:

    ```
    npm init
    ```
    
2. W swojej wiersza polecenia w folderze **manageddevice** , uruchom następujące polecenie, aby zainstalować **azure-iot-device@dtpreview** pakiet SDK urządzenia i **azure-iot-device-mqtt@dtpreview** pakietu:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. Przy użyciu edytora tekstu, Utwórz nowy plik **dmpatterns_getstarted_device.js** w folderze **manageddevice** .

4. Dodaj następujące "Wymagaj' instrukcje na początku pliku **dmpatterns_getstarted_device.js** :

    ```
    'use strict';

    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```

5. Dodawanie zmiennej **connectionString** i za jej pomocą tworzyć klientem urządzenia.  Zastąp ciąg połączenia urządzenia parametry połączenia.  

    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myDeviceId;SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

6. Dodaj następującą funkcję do implementacji metody bezpośrednio na tym urządzeniu

    ```
    var onReboot = function(request, response) {
        
        // Respond the cloud app for the direct method
        response.send(200, 'Reboot started', function(err) {
            if (!err) {
                console.error('An error occured when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.');
            }
        });
        
        // Report the reboot before the physical restart
        var date = new Date();
        var patch = {
            iothubDM : {
                reboot : {
                    lastReboot : date.toISOString(),
                }
            }
        };

        // Get device Twin
        client.getTwin(function(err, twin) {
            if (err) {
                console.error('could not get twin');
            } else {
                console.log('twin acquired');
                twin.properties.reported.update(patch, function(err) {
                    if (err) throw err;
                    console.log('Device reboot twin state reported')
                });  
            }
        });
        
        // Add your device's reboot API for physical restart.
        console.log('Rebooting!');
    };
    ```

7. Otwórz połączenie z Twoim Centrum IoT i uruchomić odbiornik metoda bezpośrednia:

    ```
    client.open(function(err) {
        if (err) {
            console.error('Could not open IotHub client');
        }  else {
            console.log('Client opened.  Waiting for reboot method.');
            client.onDeviceMethod('reboot', onReboot);
        }
    });
    ```
    
8. Zapisz i zamknij plik **dmpatterns_getstarted_device.js** .

 [AZURE.NOTE] Aby zachować proste czynności, ten samouczek nie powoduje żadnych zasad ponów próbę. W kodzie produkcyjnym wykonała zasady ponów próbę (na przykład wykładniczego wycofywania), sugerowanej w artykule w witrynie MSDN [Przejściowych obsługi błędów][lnk-transient-faults].

## <a name="trigger-a-remote-reboot-on-the-device-using-a-direct-method"></a>Uruchom ponownie komputer zdalny na tym urządzeniu metodą bezpośredniego wyzwalacza 

W tej sekcji możesz utworzyć aplikację konsoli Node.js, która inicjuje Uruchom ponownie komputer zdalny na urządzeniu metodą bezpośredni i kwerend podwójnego urządzenia są używane do wyszukiwania podczas ostatniego ponownego uruchomienia dla danego urządzenia.

1. Utwórz nowy pusty folder o nazwie **triggerrebootondevice**.  W folderze **triggerrebootondevice** Utwórz plik package.json przy użyciu następującego polecenia w wierszu polecenia.  Zaakceptuj wszystkie wartości domyślne:

    ```
    npm init
    ```
    
2. W swojej wiersza polecenia w folderze **triggerrebootondevice** , uruchom następujące polecenie, aby zainstalować **azure-iothub@dtpreview** pakiet SDK urządzenia i **azure-iot-device-mqtt@dtpreview** pakietu:

    ```
    npm install azure-iothub@dtpreview --save
    ```
    
3. Przy użyciu edytora tekstu, Utwórz nowy plik **dmpatterns_getstarted_service.js** w folderze **triggerrebootondevice** .

4. Dodaj następujące "Wymagaj' instrukcje na początku pliku **dmpatterns_getstarted_service.js** :

    ```
    'use strict';

    var Registry = require('azure-iothub').Registry;
    var Client = require('azure-iothub').Client;
    ```

5. Dodaj następujące deklaracji zmiennych i Zamień wartości symbolu zastępczego.

    ```
    var connectionString = '{iothubconnectionstring}';
    var registry = Registry.fromConnectionString(connectionString);
    var client = Client.fromConnectionString(connectionString);
    var deviceToReboot = 'myDeviceId';
    ```
    
6. Dodaj następującą funkcję do wywołania metody urządzenia do uruchomienia urządzenia:

    ```
    var startRebootDevice = function(twin) {

        var methodName = "reboot";
        
        var methodParams = {
            methodName: methodName,
            payload: null,
            timeoutInSeconds: 30
        };
        
        client.invokeDeviceMethod(deviceToReboot, methodParams, function(err, result) {
            if (err) { 
                console.error("Direct method error: "+err.message);
            } else {
                console.log("Successfully invoked the device to reboot.");  
            }
        });
    };
    ```

7. Dodaj następującą funkcję wyszukiwania dla urządzenia i uzyskiwanie ostatniego Uruchom ponownie komputer:

    ```
    var queryTwinLastReboot = function() {

        registry.getTwin(deviceToReboot, function(err, twin){

            if (twin.properties.reported.iothubDM != null)
            {
                if (err) {
                    console.error('Could not query twins: ' + err.constructor.name + ': ' + err.message);
                } else {
                    var lastRebootTime = twin.properties.reported.iothubDM.reboot.lastReboot;
                    console.log('Last reboot time: ' + JSON.stringify(lastRebootTime, null, 2));
                }
            } else 
                console.log('Waiting for device to report last reboot time.');
        });
    };
    ```
    
8. Dodaj następujący kod, aby wywołują funkcje, które będą wyzwalać metoda bezpośrednia Uruchom ponownie komputer i kwerendy dla ostatniego Uruchom ponownie komputer:

    ```
    startRebootDevice();
    setInterval(queryTwinLastReboot, 2000);
    ```
    
9. Zapisz i zamknij plik **dmpatterns_getstarted_service.js** .

## <a name="run-the-applications"></a>Uruchamianie aplikacji

Teraz możesz przystąpić do uruchamiania aplikacji.

1. W wierszu polecenia — w folderze **manageddevice** , uruchom następujące polecenie, aby rozpocząć wykrywanie metoda bezpośrednia Uruchom ponownie komputer.

    ```
    node dmpatterns_getstarted_device.js
    ```

2. W dolnym wierszu polecenia w folderze **triggerrebootondevice** uruchom następujące polecenie, aby Uruchom ponownie komputer zdalny i kwerenda podwójnego urządzenia znaleźć ostatnio Uruchom ponownie czasu.

    ```
    node dmpatterns_getstarted_service.js
    ```

3. Reagowanie na metoda bezpośrednia będą widzieli pod drukowanie wiadomości

## <a name="customize-and-extend-the-device-management-actions"></a>Dostosowywanie i rozszerzanie urządzenie zarządzania akcje

Rozwiązanie IoT można rozwinąć zdefiniowany zestaw urządzenia zarządzania desenie lub włączyć Desenie niestandardowe przy użyciu podwójnego urządzenia i pierwotnych metody C2D. Inne akcje zarządzania urządzenia przykładami fabryczne, aktualizacji oprogramowania układowego, aktualizacji oprogramowania, zarządzania power, sieci i łączności zarządzania i szyfrowania danych.

## <a name="device-maintenance-windows"></a>Urządzenie konserwacji w systemie windows

Zazwyczaj możesz skonfigurować urządzenia do działania w czasie, która minimalizuje przerwy w pracy i przestoje.  Windows konserwacji urządzenia są często używane deseń do czasu, gdy urządzenie należy zaktualizować konfigurację. Rozwiązanie wewnętrznej umożliwia odpowiednie właściwości podwójnego urządzenia Zdefiniuj i włącz zasady na urządzeniu, umożliwiającą oknem konserwacji. Gdy urządzenie odbierze zasad okna konserwacji, służy zgłoszoną właściwość podwójnego urządzenia aby raportować stan zasady. Aplikacja wewnętrznej można użyć kwerendy podwójnego urządzenia do poświadczenia zgodności urządzeń i każdej zasady.

## <a name="next-steps"></a>Następne kroki

W tym samouczku używana metoda bezpośrednia wyzwalać Uruchom ponownie komputer zdalny na urządzeniu, używanym podwójnego urządzenia zgłoszone właściwości raportowania ostatniego Uruchom ponownie komputer z urządzenia i kwerenda podwójnego urządzenia odkryć, uruchom ponownie komputer ostatniego urządzenia z chmury.

Aby kontynuować, wprowadzenie do Centrum IoT i wzorców zarządzania urządzenia, takich jak zdalnego przez aktualizację oprogramowania układowego air, zobacz:

[Samouczek: Jak wykonać aktualizacji oprogramowania][lnk-fwupdate]

Aby dowiedzieć się, jak zwiększyć usługi IoT metody rozwiązania i planowanie połączeń na wielu urządzeniach, zobacz [Planowanie i emisji zadania] [ lnk-tutorial-jobs] samouczka.

Aby kontynuować, wprowadzenie do Centrum IoT, zobacz [Wprowadzenie do SDK bramy IoT][lnk-gateway-SDK].


<!-- images and links -->
[img-output]: media/iot-hub-get-started-with-dm/image6.png
[img-dm-ui]: media/iot-hub-get-started-with-dm/dmui.png

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md

[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-fwupdate]: iot-hub-firmware-update.md
[Azure portal]: https://portal.azure.com/
[Using resource groups to manage your Azure resources]: ../azure-portal/resource-group-portal.md
[lnk-dm-github]: https://github.com/Azure/azure-iot-device-management
[lnk-tutorial-jobs]: iot-hub-schedule-jobs.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx