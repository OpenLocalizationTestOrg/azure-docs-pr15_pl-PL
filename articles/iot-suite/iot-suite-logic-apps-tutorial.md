<properties
  pageTitle="Pakiet Azure IoT i aplikacje logiczny | Microsoft Azure"
  description="Samouczek dotyczący nawiązać połączenie logiczny aplikacje pakietu IoT Azure dla procesu biznesowego."
  services=""
  suite="iot-suite"
  documentationCenter=""
  authors="aguilaaj"
  manager="timlt"
  editor=""/>

<tags
  ms.service="iot-suite"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="na"
  ms.date="08/16/2016"
  ms.author="araguila"/>
  
# <a name="tutorial-connect-logic-app-to-your-azure-iot-suite-remote-monitoring-preconfigured-solution"></a>Samouczek: Łączenie aplikacji logiki do rozwiązania Azure IoT pakietu zdalnego monitorowania wstępnie

[Microsoft Azure IoT Suite] [ lnk-internetofthings] zdalne monitorowanie wstępnie rozwiązanie jest świetnym sposobem szybko Rozpocznij pracę z exemplifies rozwiązanie IoT zestaw funkcji zakończenia do końca. Ten samouczek przeprowadzi Cię przez dodawanie logiki aplikacji do firmy Microsoft Azure IoT pakietu zdalne monitorowanie wstępnie rozwiązanie. Te kroki pokazano, jak można wykonać rozwiązania IoT jeszcze bardziej, przez nawiązanie procesów biznesowych.

_Jeśli szukasz instrukcje dotyczące świadczenia zdalne monitorowanie wstępnie rozwiązanie, zobacz [Samouczek: wprowadzenie do rozwiązania IoT wstępnie][lnk-getstarted]._

Przed rozpoczęciem tego samouczka, należy:

- Obsługa administracyjna zdalne monitorowanie wstępnie rozwiązanie w ramach subskrypcji Azure.

- Utwórz konto SendGrid, aby umożliwić wysyłanie wiadomości e-mail, która ma wyzwalać procesów biznesowych. Możesz Utwórz konto bezpłatne konto wersji próbnej u [SendGrid](https://sendgrid.com/) , klikając pozycję **Wypróbuj bezpłatnie**. Po zarejestrowaniu bezpłatne konto wersji próbnej, należy utworzyć [klucz interfejsu API](https://sendgrid.com/docs/User_Guide/Settings/api_keys.html) w SendGrid, zapewniającej uprawnienia do wysyłania poczty e-mail. Potrzebujesz ten klucz interfejsu API w dalszej części samouczka.

Przy założeniu zostały już obsługi administracyjnej zdalne monitorowanie wstępnie rozwiązanie, przejdź do grupy zasobów dotyczących tego rozwiązania w [Azure portal][lnk-azureportal]. Grupa zasobów ma taką samą nazwę jak nazwa rozwiązania wybierzesz podczas obsługi administracyjnej zdalnego monitorowania rozwiązania. W grupie zasobów można wyświetlić ustanawianie Azure zasobów dla swojego rozwiązania z wyjątkiem aplikacji usługi Azure Active Directory, która znajduje się w portalu klasyczny Azure. Następujące zrzut ekranu przedstawia przykład karta **Grupa zasobów** dla zdalnego monitorowania wstępnie rozwiązania:

![](media/iot-suite-logic-apps-tutorial/resourcegroup.png)

Aby rozpocząć, konfigurowanie aplikacji logiki do użytku z wstępnie rozwiązanie.

## <a name="set-up-the-logic-app"></a>Konfigurowanie aplikacji warunków logicznych

1. Kliknij przycisk __Dodaj__ w górnej części Twojej karta Grupa zasobów w portalu Azure.

2. Wyszukiwanie __Aplikacji logika__, zaznacz go i kliknij przycisk **Utwórz**.

3. Wpisz __nazwę__ i używanie tej samej **subskrypcji** i **Grupa zasobów** , w której został użyty podczas zainicjowano obsługę administracyjną zdalnego rozwiązania monitorowania. Kliknij przycisk __Utwórz__.

    ![](media/iot-suite-logic-apps-tutorial/createlogicapp.png)

4. Po zakończeniu wdrożenia, możesz sprawdzić, czy aplikacja logiczny znajduje się jako zasoby w grupie zasobów.

5. Kliknij aplikację logiczny, przejdź do karta logiki aplikacji, wybierz szablon **Pustej aplikacji logiczny** , aby otworzyć **Projektanta aplikacji logicznych**.

    ![](media/iot-suite-logic-apps-tutorial/logicappsdesigner.png)

6. Wybierz pozycję __żądanie__. Ta akcja umożliwia określenie, czy przychodzące żądanie HTTP o określonym JSON sformatowany czynności ładunku jako wyzwalacz.

7. Wklej następujący ciąg do schematu żądanie JSON treści:

    ```
    {
      "$schema": "http://json-schema.org/draft-04/schema#",
      "id": "/",
      "properties": {
        "DeviceId": {
          "id": "DeviceId",
          "type": "string"
        },
        "measuredValue": {
          "id": "measuredValue",
          "type": "integer"
        },
        "measurementName": {
          "id": "measurementName",
          "type": "string"
        }
      },
      "required": [
        "DeviceId",
        "measurementName",
        "measuredValue"
      ],
      "type": "object"
    }
    ```
    
    > [AZURE.NOTE] Możesz skopiować adres URL wpisu HTTP po zapisanie aplikacji logiczny, ale najpierw należy dodać akcję.

8. Kliknij pozycję __+ Nowy krok__ w obszarze ręczny wyzwalacz. Następnie kliknij przycisk **Dodaj akcję**.

    ![](media/iot-suite-logic-apps-tutorial/logicappcode.png)

9. Wyszukiwanie **SendGrid - wysyłanie wiadomości e-mail** , a następnie kliknij go.

    ![](media/iot-suite-logic-apps-tutorial/logicappaction.png)

10. Wprowadź nazwę połączenia, takie jak **SendGridConnection**, wprowadź **Klucz interfejsu API SendGrid** został utworzony, gdy konfiguracja konta SendGrid, a następnie kliknij przycisk **Utwórz**.

    ![](media/iot-suite-logic-apps-tutorial/sendgridconnection.png)

11. Dodaj adresy e-mail, którego jesteś właścicielem w polach **od** i **do** . Dodawanie **zdalne monitorowanie alert [identyfikator urządzenia]** do pola **temat** . W polu **Treść wiadomości E-mail** Dodaj **urządzenia [identyfikator urządzenia] ma zgłoszone [measurementName] z wartością [measuredValue].** Możesz dodać **[identyfikator urządzenia]**, **[measurementName]**i **[measuredValue]** , klikając w sekcji **można wstawić dane z poprzednich kroków** .

    ![](media/iot-suite-logic-apps-tutorial/sendgridaction.png)

12. W górnym menu, kliknij przycisk __Zapisz__ .

13. Kliknij pozycję wyzwalacz **żądanie** i skopiuj wartość __Post protokołu Http do tego adresu URL__ . Ten adres URL jest potrzebny w dalszej części tego samouczka.

> [AZURE.NOTE] Logika aplikacje umożliwiają uruchamianie [wiele różnych rodzajów akcji] [ lnk-logic-apps-actions] łącznie z działaniami w usłudze Office 365. 

## <a name="set-up-the-eventprocessor-web-job"></a>Ustawianie zadania EventProcessor sieci Web

W tej sekcji umożliwia nawiązanie połączenia wstępnie rozwiązanie do tworzenia aplikacji logiczny. Aby wykonać to zadanie, należy dodać adres URL, aby wyzwolić aplikacji logiki do akcję, która jest uruchamiana, gdy wartość czujnika urządzenia przekracza progu.

1. Klonowanie najnowszą wersję pakietu za pomocą klienta cyfra [azure iot-remote monitorowania repozytorium github][lnk-rmgithub]. Na przykład:

    ```
    git clone https://github.com/Azure/azure-iot-remote-monitoring.git
    ```

2. W programie Visual Studio Otwórz __RemoteMonitoring.sln__ z lokalną kopię repozytorium.

3. Otwórz plik __ActionRepository.cs__ w **infrastruktury\\repozytorium** folder.

4. Aktualizacja słownika **actionIds** z __Http Post do tego adresu URL__ zauważyć z Twojej aplikacji logicznych w następujący sposób:

    ```
    private Dictionary<string,string> actionIds = new Dictionary<string, string>()
    {
        { "Send Message", "<Http Post to this UR>" },
        { "Raise Alarm", "<Http Post to this UR> }
    };
    ```

5. Zapisanie zmian w rozwiązanie, a następnie zamknij program Visual Studio.

## <a name="deploy-from-the-command-line"></a>Wdrażanie z poziomu wiersza polecenia

W tej sekcji należy wdrożyć usługi zaktualizowanej wersji zdalnego rozwiązania monitorowania, aby zamienić wersji obecnie uruchomione w Azure.

1. Po [konfiguracji deweloperów] [ lnk-devsetup] instrukcje dotyczące konfigurowania środowiska dla wdrożenia.

2.  Aby wdrożyć lokalnie, wykonaj [lokalnego wdrożenia] [ lnk-localdeploy] instrukcje.

3.  Aby wdrożyć w chmurze i aktualizowanie istniejącego wdrożenia chmury, wykonaj [wdrożenia chmury] [ lnk-clouddeploy] instrukcje. Użyj nazwy rozmieszczenia oryginalną nazwą wdrożenia. Na przykład jeśli oryginalny wdrażania był nazywany **demologicapp**, użyj następującego polecenia:

    ``
    build.cmd cloud release demologicapp
    ``
    
    Po uruchomieniu skryptu budowania należy używać tego samego konta Azure, subskrypcji, region i wystąpieniem usługi Active Directory, z którym został użyty podczas zainicjowano obsługę administracyjną rozwiązanie.

## <a name="see-your-logic-app-in-action"></a>Zobacz aplikacji logicznych w działaniu

Zdalny monitorowania rozwiązanie wstępnie występują dwie reguły konfigurowanie domyślnie podczas obsługę rozwiązania. Obie reguły są na tym urządzeniu **SampleDevice001** :

* Temperatura > 38.00
* Wilgotność > 48.00

Reguły dotyczące temperatury Uaktywnia zadanie **Podnieść alarmu** i reguły wilgotność Uaktywnia zadanie **SendMessage** . Zakładając, że ten sam adres URL obie akcje używane klasy **ActionRepository** , aplikacji logika uaktywnia dla każdej reguły. Obie reguły za pomocą SendGrid wysłać wiadomość e-mail **na** adres z szczegóły alertu.

> [AZURE.NOTE] Aplikacja logiczny w dalszym ciągu wyzwalanie każdorazowo progu zostały spełnione. Aby uniknąć niepotrzebnych wiadomości e-mail, można wyłączyć reguły w portalu usługi rozwiązanie lub wyłączanie aplikacji logicznych w [Azure portal][lnk-azureportal].

Oprócz odbierania wiadomości e-mail, można również dostępna po uruchomieniu aplikacji logicznych w portalu:

![](media/iot-suite-logic-apps-tutorial/logicapprun.png)

## <a name="next-steps"></a>Następne kroki

Teraz aplikacji logika wykorzystano nawiązywania połączenia z procesem biznesowym wstępnie rozwiązanie, możesz dowiedzieć się więcej informacji na temat opcji dostosowywania wstępnie rozwiązań:

- [Dynamiczne telemetrycznego za pomocą zdalnego rozwiązanie wstępnie monitorowania][lnk-dynamic]
- [Metadane informacji urządzeń w zdalne monitorowanie wstępnie rozwiązanie][lnk-devinfo]

[lnk-dynamic]: iot-suite-dynamic-telemetry.md
[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[lnk-internetofthings]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-getstarted]: iot-suite-getstarted-preconfigured-solutions.md
[lnk-azureportal]: https://portal.azure.com
[lnk-logic-apps-actions]: ../connectors/apis-list.md
[lnk-rmgithub]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-devsetup]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/dev-setup.md
[lnk-localdeploy]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/local-deployment.md
[lnk-clouddeploy]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/cloud-deployment.md
