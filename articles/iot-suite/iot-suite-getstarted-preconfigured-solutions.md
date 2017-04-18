<properties
    pageTitle="Wprowadzenie do rozwiązania wstępnie | Microsoft Azure"
    description="Skorzystać z tego samouczka, aby dowiedzieć się, jak wdrożyć rozwiązanie pakietu IoT Azure wstępnie."
    services=""
    suite="iot-suite"
    documentationCenter=""
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-suite"
     ms.devlang="na"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/16/2016"
     ms.author="dobett"/>

# <a name="tutorial-get-started-with-the-preconfigured-solutions"></a>Samouczek: Wprowadzenie do rozwiązania wstępnie

## <a name="introduction"></a>Wprowadzenie

Azure IoT pakietu [rozwiązania wstępnie] [ lnk-preconfigured-solutions] łączenie wielu usług Azure IoT do końca do końca rozwiązań, implementujących typowe scenariusze biznesowe IoT. Rozwiązanie *zdalne monitorowanie* wstępnie łączy i monitoruje urządzenia. Za pomocą rozwiązanie do analizy strumieniu dane z urządzenia oraz na potrzeby ulepszania wyników biznesowych dokonując procesów automatyczne odpowiadanie na tym strumienia danych.

Ten samouczek pokazano, jak inicjować obsługę zdalnego monitorowania rozwiązanie wstępnie skonfigurowane. On również przeprowadzi Cię przez podstawowych funkcji zdalnego monitorowania rozwiązanie. Dostęp wiele z tych funkcji można uzyskać, za pomocą pulpitu nawigacyjnego rozwiązanie, który wdraża wraz z wstępnie rozwiązanie:

![Zdalne monitorowanie wstępnie rozwiązanie pulpitu nawigacyjnego][img-dashboard]

Aby użyć tego samouczka, należy aktywną subskrypcję Azure.

> [AZURE.NOTE]  Jeśli nie masz konta, możesz utworzyć bezpłatne konto wersji próbnej na kilka minut. Aby uzyskać szczegółowe informacje, zobacz [Bezpłatną wersję próbną Azure][lnk_free_trial].

[AZURE.INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

## <a name="view-the-solution-dashboard"></a>Wyświetlanie pulpitu nawigacyjnego rozwiązanie

Pulpit nawigacyjny rozwiązanie umożliwia zarządzanie wdrożonym rozwiązanie. Można na przykład wyświetlić telemetrycznego, dodać urządzeń i skonfigurować reguły.

1.  Po ukończeniu inicjowania obsługi administracyjnej i kafelku wstępnie rozwiązania wskazuje **Gotowe**, kliknij przycisk **Uruchamianie** , aby otworzyć portalem zdalnego rozwiązanie monitorowania na nowej karcie.

    ![Uruchamianie wstępnie rozwiązanie][img-launch-solution]

2.  Domyślnie portalu rozwiązanie zawiera *rozwiązanie pulpitu nawigacyjnego*. Możesz wybrać inne widoki przy użyciu menu po lewej stronie.

    ![Zdalne monitorowanie wstępnie rozwiązanie pulpitu nawigacyjnego][img-dashboard]

Pulpit nawigacyjny zawiera następujące informacje:

- Mapa Wyświetla lokalizację każdego urządzenia podłączonego do rozwiązania. Przy pierwszym uruchomieniu rozwiązanie, istnieją cztery symulowany urządzenia. Symulowany urządzenia są wykonywane jako Azure WebJobs i rozwiązanie używa interfejsu API mapy Bing kreślenia informacji na mapie.
- Panel **Historia telemetrycznego** kreśli telemetrycznego wilgotności i temperatury z wybranego urządzenia w pobliżu czasie rzeczywistym i wyświetla agregowanie danych, takich jak wilgotność maksymalnej, minimalnej i średnia.
- Panel **Historia alarmu** pokazuje ostatnie zdarzenia alarmu, gdy wartość telemetrycznego Przekroczono próg. Można zdefiniować własne alarmy oprócz przykładów utworzone przez wstępnie rozwiązanie.

## <a name="view-the-device-list"></a>Wyświetlanie listy urządzenia

Na liście urządzenia wyświetlane wszystkich urządzeń zarejestrowanych w rozwiązaniu. Możesz wyświetlać i edytować metadane urządzeń, Dodaj lub usuń urządzenia, a wysyłanie poleceń do urządzenia.

1.  Kliknij pozycję **urządzenia** w menu po lewej stronie, aby wyświetlić *listę urządzeń* dla tego rozwiązania.

    ![Lista urządzeń na pulpicie nawigacyjnym][img-devicelist]

2.  Lista urządzeń informuje, że czterech urządzeń symulowany utworzone przez proces inicjowania obsługi administracyjnej.

3.  Kliknij urządzenie na liście urządzeń, aby wyświetlić szczegóły urządzenia.

    ![Szczegóły urządzenia na pulpicie nawigacyjnym][img-devicedetails]

Panel **Szczegóły urządzenia** zawiera trzy sekcje:

- W sekcji **Akcje** listy Akcje, które można wykonywać na urządzeniu. Jeśli wyłączysz urządzenie, już nie jest dozwolona telemetrycznego wysyłanie i odbieranie poleceń. Jeśli urządzenie jest wyłączone, możesz następnie włączyć go ponownie. Możesz dodać reguły skojarzone z urządzeniem wyzwalające alarmu, gdy wartość telemetrycznego przekracza progu. Możesz też wysyłać polecenia do urządzenia. Po pierwszym podłączeniu urządzenia, informuje rozwiązanie polecenia, które można odpowiedzieć.
- W sekcji **Właściwości urządzenia** listy metadanych urządzenia. Niektóre metadane pochodzą z urządzenia (na przykład producenta), a inne są generowane przez rozwiązanie (na przykład godzina utworzenia). Możesz edytować metadane urządzeń, w tym miejscu.
- W sekcji **Kluczy uwierzytelniania** listy klawiszy, które urządzenie może używać do uwierzytelniania rozwiązanie.

## <a name="send-a-command-to-a-device"></a>Polecenie Wyślij do urządzenia

W okienku szczegółów urządzenia pokazuje wszystkie polecenia, które obsługuje konkretnego urządzenia i umożliwia chcesz wysłać polecenia do urządzenia. Podczas pierwszego uruchomienia urządzenia, wysyła informacji na temat obsługiwanych poleceń do rozwiązania.

1.  Kliknij przycisk **polecenia** w okienku Szczegóły urządzenia dla wybranego urządzenia.

    ![Polecenia urządzenia na pulpicie nawigacyjnym][img-devicecommands]

2.  Z listy polecenia Wybierz **PingDevice** .

3.  Kliknij **polecenie Wyślij**.

4.  Można wyświetlić stan polecenia w historii poleceń.

    ![Polecenie stan na pulpicie nawigacyjnym][img-pingcommand]

Rozwiązania do śledzenia stanu każdego polecenia, które wysyła. Wynikiem funkcji jest początkowo **Oczekiwanie**. Urządzenie raportów, że uruchomione polecenie, wynik jest równa **sukcesu**.

## <a name="add-a-new-simulated-device"></a>Dodawanie nowego urządzenia symulowany

Po wdrożeniu wstępnie rozwiązanie obsługi administracyjnej automatycznie cztery urządzenia przykładowych wyświetlanych na liście urządzeń. Te urządzenia to *symulowany urządzeń* działa w WebJob Azure. Symulowany urządzeń ułatwiają eksperymentować wstępnie rozwiązanie bez konieczności wdrażania rzeczywistą fizycznie urządzenia. Jeśli chcesz podłączyć urządzenie rzeczywistą rozwiązanie, zobacz [Łączenie urządzenia do komputera zdalnego, monitorowanie wstępnie rozwiązanie] [ lnk-connect-rm] samouczka.

Poniższe kroki pokazują jak dodać symulowany urządzenia do rozwiązania:

1.  Przejść z powrotem do listy urządzenia.

2.  Kliknij przycisk **+ Dodaj urządzenie** w lewym dolnym rogu, aby dodać urządzenie.

    ![Dodawanie urządzenia wstępnie rozwiązanie][img-adddevice]

3.  Kliknij przycisk **Dodaj nowy** na kafelku **Symulowany urządzenia** .

    ![Ustawianie nowego szczegóły urządzenia na pulpicie nawigacyjnym][img-addnew]
    
    Oprócz tworzenia nowego urządzenia symulowany, można również dodać urządzenia fizycznego, jeśli chcesz utworzyć **Niestandardowe urządzenie**. Aby dowiedzieć się więcej o łączeniu fizycznymi rozwiązanie, zobacz [Łączenie urządzenia w pakiecie IoT zdalne monitorowanie wstępnie rozwiązanie][lnk-connect-rm].

4.  Wybierz opcję **Pozwól mi Definiowanie identyfikator urządzenia**, a następnie wprowadź nazwę identyfikator urządzenia unikatowe, takie jak **mydevice_01**.

5.  Kliknij przycisk **Utwórz**.

    ![Zapisywanie nowego urządzenia][img-definedevice]

6. W kroku 3 **Dodaj symulowany urządzenie**kliknij przycisk **Gotowe** , aby powrócić do listy urządzenia.

7. Urządzenie z **systemem** można wyświetlać na liście urządzeń.

    ![Widok nowe urządzenie na liście urządzenia][img-runningnew]

8. Możesz także wyświetlić symulowany telemetrycznego z nowym urządzeniu na pulpicie nawigacyjnym:

    ![Widok telemetrycznego z nowego urządzenia][img-runningnew-2]

## <a name="edit-the-device-metadata"></a>Edytowanie metadanych urządzenia

Po podłączeniu najpierw urządzenia rozwiązanie, wysyła metadanych do rozwiązania. Podczas edytowania metadanych urządzenia za pomocą pulpitu nawigacyjnego rozwiązanie wysyła nowe wartości metadanych do urządzenia i zapisuje nowe wartości w bazie danych DocumentDB rozwiązanie. Aby uzyskać więcej informacji, zobacz [urządzenia tożsamości rejestru i DocumentDB][lnk-devicemetadata].

1.  Przejść z powrotem do listy urządzenia.

2.  Wybierz nowe urządzenie z **Listy urządzeń**, a następnie kliknij przycisk **Edytuj** , aby edytować **Właściwości urządzenia**:

    ![Edytowanie metadanych urządzenia][img-editdevice]

3. Przewiń w dół i zmiany vales szerokości i długości geograficznej. Następnie kliknij przycisk **Zapisz zmiany w rejestrze urządzenia**.

    ![Edytowanie metadanych urządzenia][img-editdevice2]

4. Przejść z powrotem do pulpitu nawigacyjnego, Lokalizacja urządzenia została zmieniona na mapie:

    ![Edytowanie metadanych urządzenia][img-editdevice3]

## <a name="add-a-rule-for-the-new-device"></a>Dodawanie reguły dla nowego urządzenia

Istnieją żadne reguły dla nowego urządzenia, która właśnie została dodana. W tej sekcji możesz dodać regułę, która uaktywnia alarmu 47 stopnie przekracza temperatury zgłaszane przez nowego urządzenia. Przed rozpoczęciem Zwróć uwagę, że Historia telemetrycznego dla nowego urządzenia na pulpicie nawigacyjnym pokazuje, temperatury urządzenie nigdy nie przekracza 45 stopni.

1.  Przejść z powrotem do listy urządzenia.

2.  Wybierz nowe urządzenie z **Listy urządzeń**, a następnie kliknij **Dodaj regułę** , aby dodać regułę dla urządzenia.

3. Utwórz regułę używa **temperatury** jako pole danych, który używa **AlarmTemp** jako wynik przekroczenia temperatury 47 stopni:

    ![Dodawanie reguły urządzenia][img-adddevicerule]

4. Kliknij przycisk **Zapisz i wyświetlanie reguł** Aby zapisać zmiany.

5.  Kliknij przycisk **polecenia** w okienku Szczegóły urządzenia do nowego urządzenia.

    ![Dodawanie reguły urządzenia][img-adddevicerule2]

6.  Z listy polecenia Wybierz **ChangeSetPointTemp** i ustaw **SetPointTemp** do 45. Następnie kliknij **Polecenie Wyślij**:

    ![Dodawanie reguły urządzenia][img-adddevicerule3]

7.  Przejść z powrotem do pulpitu nawigacyjnego rozwiązanie. Po pewnym czasie zostanie wyświetlony nowy wpis w okienku **Historii alarmu** przekroczenia temperatury zgłaszane przez nowym urządzeniu progu 47 stopni:

    ![Dodawanie reguły urządzenia][img-adddevicerule4]

8. Można przeglądać i edytować wszystkie reguły na stronie **reguły** pulpitu nawigacyjnego:

    ![Lista reguły urządzenia][img-rules]

9. Można przeglądać i edytować wszystkie akcje, które można podjąć w odpowiedzi na reguły na stronie **Akcje** pulpitu nawigacyjnego:

    ![Akcje listy urządzenia][img-actions]

> [AZURE.NOTE] Można zdefiniować akcje, które można wysyłać wiadomości e-mail lub wiadomości SMS w odpowiedzi na reguły lub Integracja z systemem linii biznesowych za pomocą [Aplikacji logika][lnk-logic-apps]. Aby uzyskać więcej informacji, zobacz [Łączenie aplikacji logiki do usługi Azure IoT pakietu zdalnego monitorowania wstępnie rozwiązanie][lnk-logicapptutorial].

## <a name="other-features"></a>Inne funkcje

Za pomocą portalu rozwiązanie, które można przeszukiwać dla urządzeń o określonych cech, takich jak numer modelu:

![Wyszukiwanie urządzenia][img-search]

Urządzenie zostanie wyłączone, a po jego jest wyłączona, można je usunąć:

![Wyłączanie i usuwanie urządzenia][img-disable]

## <a name="behind-the-scenes"></a>W tle

Podczas wdrażania rozwiązania wstępnie procesu wdrażania tworzy wielu zasobów w subskrypcji Azure, wybrane. Można wyświetlić te zasoby [portal]Azure[lnk-portal]. Proces wdrażania tworzy **Grupa zasobów** przy użyciu nazwy na podstawie nazwy wybranej dla wstępnie rozwiązania:

![Wstępnie skonfigurowane rozwiązanie w portalu Azure][img-portal]

Ustawienia poszczególnych zasobów można wyświetlić, wybierając go na liście zasoby w grupie zasobów.

Możesz także wyświetlić kod źródłowy wstępnie rozwiązanie. Zdalny monitorowania kodu źródłowego wstępnie rozwiązanie znajduje się w [azure iot-remote monitorowania] [ lnk-rmgithub] repozytorium GitHub:

- **DeviceAdministration** folder zawiera kod źródłowy pulpitu nawigacyjnego.
- **Simulator** folder zawiera kod źródłowy symulowany urządzenia.
- **EventProcessor** folder zawiera kod źródłowy dla procesu wewnętrznej, który obsługuje telemetrycznego przychodzących.

Po wykonaniu tych czynności, możesz usunąć wstępnie rozwiązanie z subskrypcji usługi Azure na [azureiotsuite.com] [ lnk-azureiotsuite] witryny. Ta witryna pozwala łatwo usunąć wszystkie zasoby, które Obsługa administracyjna została zainicjowana przy tworzeniu wstępnie rozwiązanie.

> [AZURE.NOTE] W celu zapewnienia, Usuń wszystkie związane z wstępnie rozwiązanie, usuń go na [azureiotsuite.com] [ lnk-azureiotsuite] witryny i po prostu nie usuwaj grupa zasobów w portalu.

## <a name="next-steps"></a>Następne kroki

Teraz, gdy zostały rozmieszczone rozwiązanie pracy wstępnie, możesz nadal wprowadzenie do pakietu IoT, czytając w następujących artykułach:

- [Zdalne monitorowanie wstępnie Instruktaż rozwiązanie][lnk-rm-walkthrough]
- [Podłącz urządzenie zdalnego rozwiązanie, wstępnie skonfigurowanego monitorowania][lnk-connect-rm]
- [Uprawnienia w witrynie azureiotsuite.com][lnk-permissions]

[img-launch-solution]: media/iot-suite-getstarted-preconfigured-solutions/launch.png
[img-dashboard]: media/iot-suite-getstarted-preconfigured-solutions/dashboard.png
[img-devicelist]: media/iot-suite-getstarted-preconfigured-solutions/devicelist.png
[img-devicedetails]: media/iot-suite-getstarted-preconfigured-solutions/devicedetails.png
[img-devicecommands]: media/iot-suite-getstarted-preconfigured-solutions/devicecommands.png
[img-pingcommand]: media/iot-suite-getstarted-preconfigured-solutions/pingcommand.png
[img-adddevice]: media/iot-suite-getstarted-preconfigured-solutions/adddevice.png
[img-addnew]: media/iot-suite-getstarted-preconfigured-solutions/addnew.png
[img-definedevice]: media/iot-suite-getstarted-preconfigured-solutions/definedevice.png
[img-runningnew]: media/iot-suite-getstarted-preconfigured-solutions/runningnew.png
[img-runningnew-2]: media/iot-suite-getstarted-preconfigured-solutions/runningnew2.png
[img-rules]: media/iot-suite-getstarted-preconfigured-solutions/rules.png
[img-editdevice]: media/iot-suite-getstarted-preconfigured-solutions/editdevice.png
[img-editdevice2]: media/iot-suite-getstarted-preconfigured-solutions/editdevice2.png
[img-editdevice3]: media/iot-suite-getstarted-preconfigured-solutions/editdevice3.png
[img-adddevicerule]: media/iot-suite-getstarted-preconfigured-solutions/addrule.png
[img-adddevicerule2]: media/iot-suite-getstarted-preconfigured-solutions/addrule2.png
[img-adddevicerule3]: media/iot-suite-getstarted-preconfigured-solutions/addrule3.png
[img-adddevicerule4]: media/iot-suite-getstarted-preconfigured-solutions/addrule4.png
[img-actions]: media/iot-suite-getstarted-preconfigured-solutions/actions.png
[img-portal]: media/iot-suite-getstarted-preconfigured-solutions/portal.png
[img-search]: media/iot-suite-getstarted-preconfigured-solutions/solutionportal_07.png
[img-disable]: media/iot-suite-getstarted-preconfigured-solutions/solutionportal_08.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com
[lnk-logic-apps]: https://azure.microsoft.com/documentation/services/app-service/logic/
[lnk-portal]: http://portal.azure.com/
[lnk-rmgithub]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-devicemetadata]: iot-suite-what-are-preconfigured-solutions.md#device-identity-registry-and-documentdb
[lnk-logicapptutorial]: iot-suite-logic-apps-tutorial.md
[lnk-rm-walkthrough]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-connect-rm]: iot-suite-connecting-devices.md
[lnk-permissions]: iot-suite-permissions.md
