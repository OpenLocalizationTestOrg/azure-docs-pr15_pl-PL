## <a name="create-a-device-management-enabled-iot-hub"></a>Tworzenie urządzenia zarządzanie włączone Centrum IoT

Ponieważ IoT Centrum zarządzania urządzeniami w podglądzie, należy utworzyć koncentratora IoT Zarządzanie włączone urządzenie. Gdy zarządzania urządzeniami Centrum IoT osiągnie ogólnodostępną, ten samouczek zostaną zaktualizowane. Poniższe kroki pokazują jak wykonać to zadanie, za pomocą portalu Azure.

1.  Zaloguj się do [portalu Azure].
2.  W Jumpbar kliknij przycisk **Nowy**, a następnie kliknij pozycję **Internet czynności**, a następnie kliknij **Centrum IoT Azure**.

    ![][img-new-hub]

3.  W karta **Centrum IoT** wybierz konfigurację Twoim Centrum IoT.

    ![][img-configure-hub]

  -   W polu **Nazwa** wprowadź nazwę dla swojego Centrum IoT. Jeśli **Nazwa** jest ważnych i dostępnych, zielony znacznik wyboru pojawi się w polu **Nazwa** .
  -   Wybierz **poziom ceny i skalę**. Ten samouczek nie wymaga określonego poziomu.
  -   **Grupa zasobów**Tworzenie nowej grupy zasobów, lub wybierz istniejący. Aby uzyskać więcej informacji zobacz [Używanie grup zasobów do zarządzania zasobami Azure].
  -   Zaznacz to pole, aby **Włączyć zarządzania urządzeniami - Podgląd**.
  -   W polu **Lokalizacja**wybierz lokalizację do obsługi z Centrum IoT. Zarządzanie urządzeniami Centrum IoT jest dostępna tylko w Stanach Zjednoczonych wschodniego, Europy Północnej i Azji Wschodniej podczas publicznej podglądu.

    > [AZURE.NOTE]Jeśli nie zostanie zaznaczone pole **Włącz zarządzania urządzeniami**, próbki nie działają.<br/>Zaznaczając pole wyboru **Włącz MDM**, możesz utworzyć podgląd Centrum IoT obsługiwane tylko w Stanach Zjednoczonych wschodniego, Europy Północnej i Azji Wschodniej i nie są przeznaczone do produkcji scenariuszy. Urządzenia nie można migrować do i wylogowywanie się z koncentratorów Zarządzanie włączone urządzenie.

4.  Po wybraniu Twoim Centrum IoT opcje konfiguracji, kliknij przycisk **Utwórz**. Może potrwać kilka minut na Azure, aby utworzyć Twoim Centrum IoT. Aby sprawdzić stan, można monitorować postęp w **Startboard** lub panel **powiadomienia** .

    ![][img-monitor]

5.  Gdy Centrum IoT został utworzony pomyślnie, karta do koncentratora będą automatycznie otwierane. Zanotuj **Hostname**, a następnie kliknij **zasady dostępu udostępnione**.

    ![][img-keys]

6.  Kliknij opcję zasady **iothubowner** , a następnie skopiuj i zanotuj parametry połączenia karta **iothubowner** . Skopiuj go do lokalizacji, w której masz dostęp do późniejszego ponieważ potrzebny do użycia tego samouczka.

    > [AZURE.NOTE] W produkcji scenariuszy upewnij się wstrzymać się od wykorzystania poświadczeń **iothubowner** .

    ![][img-connection]

Teraz utworzono urządzeniu Zarządzanie włączone IoT Centrum. Potrzebujesz parametry połączenia do użycia tego samouczka.

## <a name="create-a-device-identity"></a>Tworzenie urządzenia tożsamości

W tej sekcji, użyj narzędzia węzeł o nazwie [Eksploratora Centrum IoT] [ iot-hub-explorer] tworzenia urządzenia tożsamości dla tego samouczka.

1. W środowisku wiersza polecenia, uruchom następujące czynności:

    npm instalacji -giothub-explorer@latest

2. Następnie uruchom następujące polecenie, aby zalogować się do Centrum dokonywaniu podstaw `{service connection string}` przy użyciu parametrów połączenia Centrum IoT skopiowane:

    Eksplorator iothub logowania "{Usługa parametry połączenia}"

3. Na koniec utworzenie nowej tożsamości urządzenia o nazwie `myDeviceId` przy użyciu polecenia:

    Eksplorator iothub tworzenie myDeviceId — parametry połączenia

Zanotuj parametry połączenia urządzenia z wyników. Ten ciąg połączenia jest używane przez aplikację urządzenia nawiązywania połączenia z Twoim Centrum IoT jako urządzenie.

![][img-identity]

Zapoznaj się z [Wprowadzenie do Centrum IoT] [ lnk-getstarted] dla umożliwia utworzenie programowy tożsamości urządzenia.

<!-- images and links -->
[img-new-hub]: media/iot-hub-get-started-create-hub-pp/image1.png
[img-configure-hub]: media/iot-hub-get-started-create-hub-pp/image2.png
[img-monitor]: media/iot-hub-get-started-create-hub-pp/image3.png
[img-keys]: media/iot-hub-get-started-create-hub-pp/image4.png
[img-connection]: media/iot-hub-get-started-create-hub-pp/image5.png
[img-identity]: media/iot-hub-get-started-create-hub-pp/devidentity.png

[Azure portal]: https://portal.azure.com/
[iot-hub-explorer]: https://github.com/Azure/azure-iot-sdks/tree/master/tools/iothub-explorer

[lnk-getstarted]: ../articles/iot-hub/iot-hub-csharp-csharp-getstarted.md
[Korzystanie z grup zasobów do zarządzania zasobami Azure]: ../articles/azure-portal/resource-group-portal.md
