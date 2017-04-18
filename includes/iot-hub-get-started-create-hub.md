## <a name="create-an-iot-hub"></a>Tworzenie Centrum IoT

Tworzenie Centrum IoT dla swojego urządzenia symulowany nawiązać połączenie. Poniższe kroki pokazano, jak wykonać to zadanie za pomocą portalu Azure.

1. Zaloguj się do [portalu Azure][lnk-portal].

2. W Jumpbar, kliknij przycisk **Nowy** > **Internet rzeczy** > **Centrum IoT Azure**.

    ![Azure portal Jumpbar][1]

3. W karta **Centrum IoT** wybierz konfigurację Twoim Centrum IoT.

    ![Karta Centrum IoT][2]

    * W polu **Nazwa** wprowadź nazwę dla swojego Centrum IoT. Jeśli **Nazwa** jest ważnych i dostępnych, zielony znacznik wyboru pojawi się w polu **Nazwa** .
    * Wybierz [poziom ceny i skali][lnk-pricing]. Ten samouczek nie wymaga określonego poziomu. Ten samouczek za pomocą bezpłatnej warstwa F1.
    * **Grupa zasobów**Tworzenie nowej grupy zasobów, lub wybierz istniejący. Aby uzyskać więcej informacji, zobacz [Używanie grup zasobów do zarządzania zasobami Azure][lnk-resource-groups].
    * W polu **Lokalizacja**wybierz lokalizację do obsługi z Centrum IoT. Ten samouczek wybierz odpowiednią lokalizację najbliższej.

4. Po wybraniu Twoim Centrum IoT opcje konfiguracji, kliknij przycisk **Utwórz**.  Może potrwać kilka minut na Azure, aby utworzyć Twoim Centrum IoT. Aby sprawdzić stan, można monitorować postęp w Startboard lub panel powiadomienia.

    ![Nowy status Centrum IoT][3]

5. Gdy Centrum IoT został utworzony pomyślnie, kliknij przycisk Nowy kafelku Twoim Centrum IoT w portal Azure, aby otworzyć karta dla nowego koncentratora IoT. Zanotuj **Hostname**, a następnie kliknij **zasady dostępu udostępnione**.

    ![Karta Centrum IoT nowy][4]

6. W karta **zasady dostępu udostępnione** kliknij zasadę **iothubowner** , a następnie skopiuj i zanotuj parametry połączenia karta **iothubowner** . Aby uzyskać więcej informacji, zobacz [Kontrola dostępu] [ lnk-access-control] w "Azure Centrum IoT przewodnik dewelopera".

    ![Karta zasady udostępniania][5]


<!-- Images. -->
[1]: ./media/iot-hub-get-started-create-hub/create-iot-hub1.png
[2]: ./media/iot-hub-get-started-create-hub/create-iot-hub2.png
[3]: ./media/iot-hub-get-started-create-hub/create-iot-hub3.png
[4]: ./media/iot-hub-get-started-create-hub/create-iot-hub4.png
[5]: ./media/iot-hub-get-started-create-hub/create-iot-hub5.png

<!-- Links -->
[lnk-resource-groups]: ../articles/azure-portal/resource-group-portal.md
[lnk-portal]: https://portal.azure.com/
[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-access-control]: ../articles/iot-hub/iot-hub-devguide-security.md
