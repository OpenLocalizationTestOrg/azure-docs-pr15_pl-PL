<!--author=SharS last changed: 01/15/2016-->

#### <a name="to-install-update-12-from-the-azure-classic-portal"></a>Aby zainstalować 1.2 aktualizacji z portalu klasyczny Azure

1. Na stronie usługi StorSimple wybierz swoje urządzenie. Przejdź do pozycji **urządzenia** > **Konserwacja**.

2. U dołu strony kliknij pozycję **Skanowanie aktualizacje**. Zadanie zostanie utworzony wykonać skanowanie w poszukiwaniu dostępnych aktualizacji. Gdy zadanie zakończyło się pomyślnie, otrzymasz powiadomienie.

3. W sekcji **Aktualizacji oprogramowania** na tej samej stronie zobaczysz, że są dostępne nowe aktualizacje oprogramowania. Zalecamy, przejrzyj informacje o wersji, przed zainstalowaniem 1.2 aktualizacji na urządzeniu.

    ![Instalowanie aktualizacji oprogramowania](./media/storsimple-install-update-via-portal/InstallUpdate12_11M.png)

4. U dołu strony kliknij pozycję **Zainstaluj aktualizacje**.

5. Pojawi się monit o potwierdzenie. Kliknij **przycisk OK**.

6. Okno dialogowe **Zainstaluj aktualizacje** zostanie wyświetlona. Urządzenia powinny spełniać kontroli wymienionych w tym oknie dialogowym. Poniższe czynności, zostały ukończone przed aktualizacji. Wybierz pozycję **Opis przedstawionej powyżej i chcę zaktualizować urządzenie**. Kliknij ikonę wyboru.

    ![Komunikat z potwierdzeniem](./media/storsimple-install-update-via-portal/InstallUpdate12_2M.png)

7. Zestaw automatyczne sprawdzanie wstępna rozpocznie się teraz. W tym:

    - **Sprawdza kondycji kontrolerze** , aby zweryfikować kontrolerów urządzeń prawidłowy i w trybie online.
    
    - **Sprawdza sprzęt Składnik kondycji** Aby sprawdzić, czy wszystkie składniki sprzętowe na urządzeniu StorSimple prawidłowy.
    
    - **Sprawdza danych 0** , aby sprawdzić, czy dane 0 jest włączona na urządzeniu. Jeśli ten interfejs nie jest włączona, należy włączyć ją, a następnie ponów próbę.
    
    - **Sprawdza danych 2 i 3 danych** do weryfikacji interfejsy danych 2 i 3 dane nie są włączone. Jeśli włączono tych interfejsów, będzie konieczne je wyłączyć, a następnie spróbuj zaktualizować urządzenie. Ten test odbywa się tylko wtedy, gdy chcesz zaktualizować z oprogramowaniem GA urządzenia. Urządzenia z wersjami 0,1, 0,2 lub 0,3 nie będzie ten test.
    
    - **Sprawdzanie bramy** na dowolnym urządzeniu wersją przed aktualizacji 1. Ten test odbywa się na tym urządzeniu oprogramowaniem 1 sprzed aktualizacji, ale nie powiodła się na urządzeń, które zostały skonfigurowane dla karty sieciowej niż danych 0 bramy.
 
    Aktualizacja 1.2 zostanie zastosowany jedynie w przypadku, jeśli wszystkie powyższe testy sprzed aktualizacji zostały wykonane pomyślnie. Użytkownik będzie powiadamiany, że trwa testy sprzed aktualizacji.
  
    ![Wstępnie sprawdzanie powiadomień](./media/storsimple-install-update-via-portal/InstallUpdate12_3M.png)

    Poniżej przedstawiono przykład, w której nie można sprawdzić przed uaktualnieniem. Należy sprawdzić, czy kontrolerów urządzeń są prawidłowy i w trybie online. Konieczne będzie również sprawdzić kondycję składniki sprzętowe. W tym przykładzie kontroler 0 i 1 kontroler składników wymagają uwagi. Konieczne może się kontaktować Support firmy Microsoft, jeśli nie można ich rozwiązania przez siebie.

     ![Sprawdzanie przed nie powiodło się](./media/storsimple-install-update-via-portal/HCS_PreUpgradeChecksFailed-include.png)

    > [AZURE.NOTE] Po zastosowaniu 1.2 aktualizacji na urządzeniu StorSimple kontroli danych 2 i 3 dane i sprawdź bramy nie będzie już niezbędne dla przyszłych aktualizacji. Inne kontrole przed nadal będą wymagane.


8. Po przed uaktualnieniem testy zostały wykonane pomyślnie, zostanie utworzony zadanie aktualizacji. Gdy zadanie aktualizacji została pomyślnie utworzona, otrzymasz powiadomienie.
 
    ![Tworzenie zadania aktualizacji](./media/storsimple-install-update-via-portal/InstallUpdate12_44M.png)

    Aktualizacja zostanie następnie zastosowany na urządzeniu.
 
9. Monitorowanie postępu zadania aktualizacji, kliknij pozycję **Zadania**. Na stronie **zadania** można wyświetlić postęp aktualizacji. 

    ![Aktualizowanie informacji o postępie zadania](./media/storsimple-install-update-via-portal/InstallUpdate12_5M.png)

10. Aktualizacja będzie trwać kilka godzin. W dowolnej chwili możesz wyświetlić szczegółowe informacje o zadaniu.

    ![Aktualizowanie szczegółów zadania](./media/storsimple-install-update-via-portal/InstallUpdate12_6M.png)

11. Po ukończeniu zadania, przejdź do strony **konserwacji** i przewiń w dół do **Aktualizacji oprogramowania**.

12. Upewnij się, że urządzenie działa **1.2 aktualizacji serii 8000 StorSimple (6.3.9600.17584)**. **Ostatnia aktualizacja daty** należy również zmienić.

    ![Strony konserwacji składników](./media/storsimple-install-update-via-portal/InstallUpdate12_10M.png)

13. Pojawi się teraz, że są dostępne aktualizacje tryb konserwacji. Te aktualizacje są kłopotliwe środki aktualizacje, które powodują przestoje urządzenia i mogą być stosowane tylko za pośrednictwem interfejsu programu Windows PowerShell urządzenia. Postępuj zgodnie z instrukcjami [instalowania aktualizacji tryb konserwacji](storsimple-update-device.md#install-maintenance-mode-updates-via-windows-powershell-for-storsimple) zainstalować następujące aktualizacje za pomocą programu Windows PowerShell dla StorSimple.

> [AZURE.NOTE] W niektórych przypadkach może zostać wyświetlony komunikat wskazujący konserwacja tryb aktualizacje są dostępne w górę do 24 godzin po aktualizacji tryb konserwacji pomyślnie są stosowane na tym urządzeniu.  


