<properties 
   pageTitle="Zarządzanie urządzeniami z menedżerem migawkę StorSimple | Microsoft Azure"
   description="Informacje dotyczące używania przystawki MMC Menedżer migawkę StorSimple do łączenia i zarządzanie urządzeniami StorSimple."
   services="storsimple"
   documentationCenter=""
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="04/18/2016"
   ms.author="v-sharos" />

# <a name="use-storsimple-snapshot-manager-to-connect-and-manage-storsimple-devices"></a>Aby połączyć i zarządzanie urządzeniami StorSimple za pomocą Menedżera migawkę StorSimple

## <a name="overview"></a>Omówienie

Węzły w okienku Menedżera migawkę StorSimple **zakres** umożliwia weryfikowanie zaimportowanych danych urządzenia StorSimple i odświeżanie urządzeń połączonych miejsca do magazynowania. Ponadto po kliknięciu węzła **urządzenia** , zostanie wyświetlona lista połączonych urządzeń i odpowiednie informacje o stanie w okienku **wyników** .

![Podłączone urządzenia](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_connect_devices.png)

**Rysunek 1: Menedżer migawkę StorSimple urządzeniami** 

W zależności od ustawień **widoku** w okienku **wyników** wyświetlane następujące informacje o każdym urządzeniu. (Aby uzyskać więcej informacji o konfigurowaniu widoku, przejdź do [menu Widok](storsimple-use-snapshot-manager.md#view-menu).


| Kolumny wyników  |Opis          |
|:----------------|:--------------------| 
| Nazwa            | Nazwa urządzenia, zgodnie z konfiguracją w portalu klasyczny Azure|
| Model           | Numer modelu urządzenia|
| Wersja         | Wersja oprogramowania zainstalowanego na tym urządzeniu |
| Stan          | Czy urządzenie jest dostępna |
| Ostatnio zsynchronizowany     | Data i godzina, kiedy ostatnio został zsynchronizowany urządzenia |
| Numer seryjny      | Numer seryjny urządzenia |
 
Kliknięcie prawym przyciskiem myszy węzeł **urządzeń** w okienku **zakres** , można wybrać spośród następujących akcji:

- Dodawanie lub zamienianie urządzenia 
- Podłącz urządzenie i sprawdź, czy importowanie plików 
- Odświeżanie połączonych urządzeń 

Jeśli klikniesz węzła **urządzenia** , a następnie kliknij prawym przyciskiem myszy nazwę urządzenia w okienku **wyników** , można wybrać spośród następujących akcji:

- Uwierzytelnianie urządzenia 
- Widok Szczegóły urządzenia 
- Odśwież urządzenia 
- Usuwanie konfiguracji urządzenia 
- Zmienianie hasła urządzenia

>[AZURE.NOTE] Wszystkie te akcje są również dostępne w okienku **akcji** .
 
Ten samouczek wyjaśniono, jak za pomocą Menedżera migawkę StorSimple nawiązywanie połączenia i zarządzanie urządzeniami i wykonywać następujące zadania:

- Dodawanie lub zamienianie urządzenia 
- Podłącz urządzenie i sprawdź, czy importowanie plików 
- Odświeżanie połączonych urządzeń 
- Uwierzytelnianie urządzenia 
- Widok Szczegóły urządzenia 
- Odświeżanie jedno urządzenie 
- Usuwanie konfiguracji urządzenia 
- Zmienianie hasła wygasły urządzenia
- Zamień urządzenie nie powiodło się

>[AZURE.NOTE] Aby uzyskać ogólne informacje o korzystaniu z interfejsu Menedżera migawkę StorSimple przejdź do [Menedżera migawkę StorSimple interfejsu użytkownika](storsimple-use-snapshot-manager.md).


## <a name="add-or-replace-a-device"></a>Dodawanie lub zamienianie urządzenia

Poniższa procedura umożliwia dodawanie lub zamienianie urządzeniu StorSimple.

#### <a name="to-add-or-replace-a-device"></a>Dodawanie lub zamienianie urządzenia

1. Kliknij ikonę pulpitu, aby uruchomić Menedżera migawkę StorSimple.

2. W okienku **zakres** kliknij prawym przyciskiem myszy węzeł **urządzenia** , a następnie kliknij **Konfiguruj urządzenie**. Zostanie wyświetlone okno dialogowe **Konfigurowanie urządzenia** .

    ![Konfigurowanie urządzenia StorSimple](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_config_device.png) 

3. W polu listy rozwijanej **urządzenie** zaznacz adres IP lub wirtualnego urządzenia. 

4. W polu tekstowym **hasło** wpisz hasło StorSimple migawkę Menedżer utworzonego dla urządzenia w portalu klasyczny Azure. Kliknij **przycisk OK**. Menedżer migawkę StorSimple wyszukiwanie określonymi urządzenia. 

    - Jeśli urządzenie jest dostępna, Menedżer migawkę StorSimple dodaje połączenie. 

    - Jeśli urządzenie jest niedostępna dla dowolnego powodu, Menedżer migawkę StorSimple zwraca komunikat o błędzie. Kliknij **przycisk OK** , aby zamknąć komunikat o błędzie, a następnie kliknij przycisk **Anuluj** , aby zamknąć okno dialogowe **Konfigurowanie urządzenia** .

## <a name="connect-a-device-and-verify-imports"></a>Podłącz urządzenie i sprawdź, czy importowanie plików

Poniższa procedura umożliwia podłączyć urządzenie StorSimple i sprawdź, czy wszystkie istniejących grup głośność, które skojarzono kopie zapasowe zostaną zaimportowane.

#### <a name="to-connect-a-device-and-verify-imports"></a>Aby podłączyć urządzenie i sprawdź, czy importuje

1. Podłącz urządzenie do Menedżera migawkę StorSimple, postępuj zgodnie z instrukcjami w Dodawanie lub zamienianie urządzenia. Podczas łączenia się z urządzeniem, Menedżer migawkę StorSimple działa w następujący sposób:

    - Jeśli urządzenie jest niedostępna dla dowolnego powodu, Menedżer migawkę StorSimple zwraca komunikat o błędzie. 

   - Jeśli urządzenie jest dostępna, Menedżer migawkę StorSimple dodaje połączenie. Gdy wybierz urządzenie, zostanie ono wyświetlone w okienku **wyników** , a wartość pola Stan wskazuje, że urządzenie jest **dostępne**. Menedżer migawkę StorSimple importuje grup głośność skonfigurowane dla urządzenia, pod warunkiem, że jest skojarzony z grupami głośność kopie zapasowe. Zasady kopii zapasowej nie zostaną zaimportowane. Grupy głośność, które nie jest skojarzone kopie zapasowe nie zostaną zaimportowane.

2. Kliknij ikonę pulpitu, aby uruchomić Menedżera migawkę StorSimple.

3. Kliknij prawym przyciskiem myszy węzła najwyższego poziomu w okienku **zakres** , a następnie kliknij **Przełącznik importowanie wyświetlania**.

    ![Wybierz pozycję Przełącz wyświetlanie importowanie plików](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Toggle_Imports_Display.png) 

4. Pojawi się okno dialogowe **Przełącz importowanie wyświetlanie** przedstawiający stan zaimportowanych głośność grupy i kopie zapasowe. Kliknij **przycisk OK**. 

Gdy głośność grupy i kopie zapasowe zostanie pomyślnie zaimportowane, służy Menedżer migawkę StorSimple do zarządzania nimi, tak, jak chcesz zarządzać głośność grupy i kopie zapasowe, które została utworzona i skonfigurowana przy użyciu Menedżera migawkę StorSimple. 

## <a name="refresh-connected-devices"></a>Odświeżanie połączonych urządzeń

Poniższa procedura umożliwia synchronizowanie połączenia urządzeń StorSimple przy użyciu Menedżera migawkę StorSimple.

####<a name="to-refresh-connected-devices"></a>Odświeżanie połączonych urządzeń

1. Kliknij ikonę pulpitu, aby uruchomić Menedżera migawkę StorSimple.

2. W okienku **zakres** **urządzeń**kliknij prawym przyciskiem myszy, a następnie kliknij **Odświeżanie urządzeń**. Synchronizowanie urządzeń podłączonych, przy użyciu Menedżera migawkę StorSimple tak, aby głośność grup i kopie zapasowe, łącznie z dowolnym ostatnio dodane można przeglądać. 

    ![Odświeżanie urządzeń StorSimple](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Refresh_devices.png)
 
Akcja **Odświeżania urządzeń** pobiera wszystkich nowych grup głośności i wszystkie skojarzone kopii zapasowych z urządzeń podłączonych. W odróżnieniu od **Skanuj ilości** akcji dostępnych dla węzła **wielkości** **Odświeżanie urządzeń** nie spowoduje przywrócenia kopii zapasowej rejestru.

## <a name="authenticate-a-device"></a>Uwierzytelnianie urządzenia

Poniższa procedura umożliwia uwierzytelnienia urządzenie StorSimple przy użyciu Menedżera migawkę StorSimple.

#### <a name="to-authenticate-a-device"></a>Do uwierzytelnienia urządzenia

1. Kliknij ikonę pulpitu, aby uruchomić Menedżera migawkę StorSimple.

2. W okienku **zakres** kliknij pozycję **urządzenia**.

3. W okienku **wyników** kliknij prawym przyciskiem myszy nazwę urządzenia, a następnie kliknij **uwierzytelniania**.

4. Zostanie wyświetlone okno dialogowe **uwierzytelniania** . Wpisz hasło urządzenia, a następnie kliknij **przycisk OK**.

    ![Okno dialogowe uwierzytelniania](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Authenticate.png) 
 
## <a name="view-device-details"></a>Widok Szczegóły urządzenia

Poniższa procedura umożliwia Przejrzyj szczegóły urządzenia StorSimple i, jeśli to konieczne, ponownie zsynchronizować urządzenia przy użyciu Menedżera migawkę StorSimple.

#### <a name="to-view-and-resynchronize-device-details"></a>Aby wyświetlić i ponownie zsynchronizować szczegóły urządzenia

1. Kliknij ikonę pulpitu, aby uruchomić Menedżera migawkę StorSimple.

2. W okienku **zakres** kliknij pozycję **urządzenia**.

3. W okienku **wyników** kliknij prawym przyciskiem myszy nazwę urządzenia, a następnie kliknij przycisk **Szczegóły**. 

Zostanie wyświetlone okno dialogowe 4. **Szczegóły urządzenia** . W tym polu wyświetlana nazwa, model, wersji, numer seryjny, stan, iSCSI docelowej kwalifikowana nazwa (IQN) i ostatniej synchronizacji daty i godziny. 

   - Kliknij przycisk **Resync** do synchronizacji urządzenia.

   - Kliknij **przycisk OK** lub **Anuluj** , aby zamknąć okno dialogowe.

    ![Szczegóły urządzenia](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Device_details.png) 
 
## <a name="refresh-an-individual-device"></a>Odświeżanie jedno urządzenie

Poniższa procedura umożliwia ponownie zsynchronizować jedno urządzenie StorSimple przy użyciu Menedżera migawkę StorSimple.

#### <a name="to-refresh-a-device"></a>Aby odświeżyć urządzenia

1. Kliknij ikonę pulpitu, aby uruchomić Menedżera migawkę StorSimple. 

2. W okienku **zakres** kliknij pozycję **urządzenia**. 

3. W okienku **wyników** kliknij prawym przyciskiem myszy nazwę urządzenia, a następnie kliknij **Odświeżanie urządzenia**. Urządzenia do synchronizacji z menedżerem migawkę StorSimple. 

## <a name="delete-a-device-configuration"></a>Usuwanie konfiguracji urządzenia

Poniższa procedura umożliwia usuwanie poszczególnych konfiguracji urządzenia StorSimple za pomocą Menedżera migawkę StorSimple.

#### <a name="to-delete-a-device-configuration"></a>Aby usunąć Konfiguracja urządzenia

1. Kliknij ikonę pulpitu, aby uruchomić Menedżera migawkę StorSimple. 

2. W okienku **zakres** kliknij pozycję **urządzenia**. 

3. W okienku **wyników** kliknij prawym przyciskiem myszy nazwę urządzenia, a następnie kliknij polecenie **Usuń**. 

4. Zostanie wyświetlony następujący komunikat. Kliknij przycisk **Tak,** Aby usunąć konfigurację lub kliknij przycisk **nie** , aby anulować usunięcie.

    ![Usuwanie konfiguracji urządzenia](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_DeleteDevice.png)

## <a name="change-an-expired-device-password"></a>Zmienianie hasła wygasły urządzenia

Wprowadź hasło do uwierzytelnienia urządzenie StorSimple przy użyciu Menedżera migawkę StorSimple. To hasło konfiguruje się podczas konfigurowania urządzenia, korzystając z interfejsu programu Windows PowerShell. Jednak można wygasania hasła. W takim przypadku można zmienić hasło za pomocą portalu klasyczny Azure. Następnie ponieważ urządzenia został skonfigurowany w Menedżerze migawkę StorSimple przed hasła, możesz ponownie uwierzytelnić urządzenia w Menedżerze migawkę StorSimple. 

#### <a name="to-change-the-expired-password"></a>Aby zmienić hasła

1. W portalu klasyczny Azure Uruchom usługę Menedżer StorSimple.

2. Kliknij pozycję **urządzenia** > **Konfigurowanie** urządzenia.

3. Przewiń w dół do sekcji StorSimple migawkę Manager. Wprowadź hasło, które jest 14-15 znaków. Upewnij się, że hasło zawiera zarówno wielkie, małe litery, liczbowe i znaki specjalne.

4. Ponownie wpisz hasło, aby je potwierdzić.

5. Kliknij pozycję **Zapisz** u dołu strony.

#### <a name="to-re-authenticate-the-device"></a>Aby ponownie uwierzytelnić urządzenie

1. Uruchom Menedżera migawkę StorSimple.

2. W okienku **zakres** kliknij pozycję **urządzenia**. W okienku **wyników** zostanie wyświetlona lista skonfigurowanych urządzeń. 

3. Wybierz urządzenie, kliknij prawym przyciskiem myszy, a następnie kliknij **uwierzytelniania**.

4. W oknie **uwierzytelniania** wprowadź nowe hasło. 

5. Wybierz urządzenie, kliknij prawym przyciskiem myszy i wybierz pozycję **Odśwież urządzenia**. Urządzenia do synchronizacji z menedżerem migawkę StorSimple. 

## <a name="replace-a-failed-device"></a>Zamień urządzenie nie powiodło się

Jeśli urządzenie StorSimple kończy się niepowodzeniem i jest zastępowana przez urządzenie wstrzymania (funkcją przełączania awaryjnego), wykonaj następujące czynności, łączenie do nowego urządzenia i wyświetlanie skojarzone kopie zapasowe.

#### <a name="to-connect-to-a-new-device-after-failover"></a>Aby nawiązać nowe urządzenie po przełączeniu

1. Zmień konfigurację połączenia iSCSI do nowego urządzenia. Aby uzyskać instrukcje, przejdź do "kroku 7: instalacji, inicjowania i formatowanie woluminu" w [Rozmieszczanie urządzenia StorSimple lokalnego](storsimple-deployment-walkthrough-u2.md). 

>[AZURE.NOTE] Nowe urządzenie StorSimple ma taki sam adres IP, jak starego, można połączyć starego konfiguracji. 

2. Zatrzymywanie usługi zarządzania StorSimple firmy Microsoft:

    1. Uruchom Menedżera serwera.

    2. Na pulpicie nawigacyjnym Menedżer serwera, w menu **Narzędzia** wybierz **usługi**. 

    3. W oknie **usługi** wybierz **Usługę Microsoft Management StorSimple**. 

    4. W okienku po prawej stronie w obszarze **Usługi Microsoft Management StorSimple**kliknij przycisk **Zatrzymaj usługę**. 

3. Usuń informacje o konfiguracji związanych ze starej urządzeniem: 

    1. W Eksploratorze plików przejdź do C:\ProgramData\Microsoft\StorSimple\BACatalog. 

    2. Usuwanie plików w folderze BACatalog. 

4. Ponowne uruchamianie usługi zarządzania StorSimple firmy Microsoft: 

    1. Na pulpicie nawigacyjnym Menedżer serwera, w menu **Narzędzia** wybierz **usługi**. 

    2. W oknie **usługi** wybierz **Usługę Microsoft Management StorSimple**. 

    3. W okienku po prawej stronie w obszarze **Usługi zarządzania StorSimple firmy Microsoft**, kliknij **ponownie uruchomić usługę**. 

5. Uruchom Menedżera migawkę StorSimple. 

6. Aby skonfigurować nowego urządzenia StorSimple, wykonaj czynności opisane w kroku 2: Podłącz urządzenie StorSimple w [Menedżerze migawkę StorSimple wdrażanie](storsimple-snapshot-manager-deployment.md). 

7. Kliknij prawym przyciskiem myszy węzeł najwyższego poziomu w okienku **zakres** (StorSimple Menedżer migawki w przykładzie), a następnie kliknij **Przełącznik importowanie wyświetlania**. 

8. Komunikat jest wyświetlany, gdy głośność zaimportowanych grupy i kopie zapasowe są widoczne w Menedżerze migawkę StorSimple. Kliknij **przycisk OK**. 

## <a name="next-steps"></a>Następne kroki

- Dowiedz się, jak za [pomocą Menedżera migawkę StorSimple administrowania rozwiązania StorSimple](storsimple-snapshot-manager-admin.md).
- Dowiedz się, jak za [pomocą Menedżera migawkę StorSimple do wyświetlania i zarządzania wielkości](storsimple-snapshot-manager-manage-volumes.md).

