<properties 
   pageTitle="Wdrażanie Menedżera migawkę StorSimple | Microsoft Azure"
   description="Dowiedz się, jak pobrać i zainstalować Menedżer migawkę StorSimple przystawki zarządzania StorSimple funkcje ochrony i wykonywanie kopii zapasowych danych."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="05/24/2016"
   ms.author="v-sharos" />

# <a name="deploy-the-storsimple-snapshot-manager-mmc-snap-in"></a>Wdrażanie przystawki MMC Menedżer migawkę StorSimple

## <a name="overview"></a>Omówienie

Menedżer migawkę StorSimple jest przystawką programu Microsoft Management Console (MMC), która upraszcza zarządzanie kopii zapasowej w środowisku programu Microsoft Azure StorSimple i ochrony danych. StorSimple migawkę Menedżera można zarządzać Microsoft Azure StorSimple lokalnej i w chmurze miejsca do magazynowania tak, jakby była systemu w pełni zintegrowany miejsca do magazynowania, więc upraszczanie procesy tworzenia kopii zapasowych i przywracania i zmniejszenie kosztów. 

Ten samouczek w tym artykule opisano wymagania dotyczące konfiguracji, jak również procedury instalowania, usuwanie i uaktualnianie StorSimple migawkę menedżera.

>[AZURE.NOTE] 
>
>- Nie można używać Menedżera migawkę StorSimple do zarządzania Microsoft Azure StorSimple wirtualnych tablic (nazywane także StorSimple lokalnej wirtualną urządzenia).
>
>- Jeśli planujesz zainstalować na urządzeniu StorSimple StorSimple aktualizacji 2, pamiętaj pobrać najnowszą wersję programu StorSimple migawkę Manager i zainstaluj ją, **przed zainstalowaniem StorSimple aktualizacji 2**. Najnowszą wersję programu StorSimple migawkę Manager jest zgodny z poprzednimi wersjami i działa we wszystkich wersjach wydaną pakietu Microsoft Azure StorSimple. Jeśli korzystasz z poprzedniej wersji StorSimple migawkę menedżera, należy zaktualizować (nie musisz odinstalować poprzednią wersję przed zainstalowaniem nowej wersji).

## <a name="storsimple-snapshot-manager-installation"></a>Menedżer migawkę StorSimple instalacji

Menedżer migawkę StorSimple można zainstalować na komputerach z systemem Windows Server 2008 R2 z dodatkiem SP1, Windows Server 2012 lub system operacyjny Windows Server 2012 R2. Na serwerach z systemem Windows 2008 R2 należy również zainstalować systemu Windows Server 2008 z dodatkiem SP1 i Windows Management Framework 3.0. 

Przed zainstalowaniem lub uaktualnić przystawki Menedżer migawkę StorSimple dla programu Microsoft Management Console (MMC) upewnij się, że serwer Microsoft Azure StorSimple urządzenia i hosta są poprawnie skonfigurowane. 

## <a name="configure-prerequisites"></a>Konfigurowanie wymagania wstępne

Poniższe czynności stanowią omówienie zadania konfiguracji należy wykonać przed zainstalowaniem Menedżera migawkę StorSimple. Ukończono konfigurację Microsoft Azure StorSimple i informacje o instalacji, w tym wymagania systemowe i instrukcje krok po kroku zobacz temat [Deploy urządzenia StorSimple lokalnego](storsimple-deployment-walkthrough.md).

>[AZURE.IMPORTANT]Przed rozpoczęciem należy przejrzeć [Lista kontrolna konfiguracji wdrażania](storsimple-deployment-walkthrough.md#deployment-configuration-checklist) i i [Warunki wstępne wdrażania](storsimple-deployment-walkthrough.md#deployment-prerequisites) w [Rozmieszczanie urządzenia StorSimple lokalnego](storsimple-deployment-walkthrough.md).
> <br>
 
### <a name="before-you-install-storsimple-snapshot-manager"></a>Przed rozpoczęciem instalacji Menedżera migawkę StorSimple

1. Rozpakowywanie, zainstalować i podłączyć urządzenie Microsoft Azure StorSimple, zgodnie z opisem w [zainstalować urządzenia StorSimple 8100](storsimple-8100-hardware-installation.md) lub [urządzeniu StorSimple 8600](storsimple-8600-hardware-installation.md).

2. Upewnij się, że komputera-hosta działa jeden z następujących systemów operacyjnych:

    - Windows Server 2008 R2 (na serwerach Windows 2008 R2, należy również zainstalować Windows Management Framework 3.0 i Windows Server 2008 z dodatkiem SP1)
    - System Windows Server 2012
    - Windows Server 2012 R2
 
    Urządzenie wirtualne StorSimple hosta musi być maszyny wirtualnej Microsoft Azure. 

3. Upewnij się, że spełniają wszystkie wymagania konfiguracji Microsoft Azure StorSimple. Aby uzyskać szczegółowe informacje przejdź do [wdrożenia wymagania wstępne](storsimple-deployment-walkthrough.md#deployment-prerequisites).

4. Podłącz urządzenie do hosta i wykonaj początkowej konfiguracji. Aby uzyskać szczegółowe informacje przejdź do [kroków wdrażania](storsimple-deployment-walkthrough.md#deployment-steps).

5. Tworzenie wielkości na tym urządzeniu, przydzielanie ich do hosta i sprawdź, czy host można zainstalować i używać ich. Menedżer migawkę StorSimple obsługuje następujące wielkości: 

    - Podstawowe
    - Prosta wielkości
    - Dynamiczne wielkości
    - Lustrzane odbicie wielkości dynamiczne (RAID 1)
    - Udostępnione klaster wielkości
 
    Dla informacji o tworzeniu wielkości na urządzeniu StorSimple lub urządzenia wirtualnego StorSimple, przejdź do [kroku 6: Tworzenie woluminu](storsimple-deployment-walkthrough.md#step-6-create-a-volume), w [Rozmieszczanie urządzenia StorSimple lokalnego](storsimple-deployment-walkthrough.md).

## <a name="install-a-new-storsimple-snapshot-manager"></a>Instalowanie nowego Menedżera migawkę StorSimple

Przed zainstalowaniem Menedżera migawkę StorSimple, upewnij się, wielkość utworzony na urządzeniu StorSimple lub StorSimple zainstalowany, zainicjowany i sformatowane zgodnie z opisem w [wymagania wstępne konfigurowanie](#configure-prerequisites)urządzenia wirtualnego.

>[AZURE.IMPORTANT]
>
>- Urządzenie wirtualne StorSimple hosta musi być maszyny wirtualnej Microsoft Azure. 
>
>- Host musi być zainstalowany system Windows 2008 R2, Windows Server 2012 lub Windows Server 2012 R2. Jeśli serwer działa system Windows Server 2008 R2, należy również zainstalować systemu Windows Server 2008 z dodatkiem SP1 i Windows Management Framework 3.0.
>
>- Przed nawiązaniem połączenia urządzenia do Menedżera migawkę StorSimple, musisz skonfigurować połączenia iSCSI na hoście na urządzeniu StorSimple.

Wykonaj poniższe czynności, aby ukończyć instalację świeży z StorSimple migawkę menedżera. Jeśli instalujesz uaktualnienie, przejdź do [uaktualnienie lub ponowne zainstalowanie Menedżera migawkę StorSimple](#upgrade-or-reinstall-storsimple-snapshot-manager).

- Krok 1: Instalowanie Menedżera StorSimple migawki 
- Krok 2: Nawiązywanie połączenia StorSimple migawkę Menedżer urządzeń 
- Krok 3: Sprawdź połączenie na urządzeniu 

###<a name="step-1-install-storsimple-snapshot-manager"></a>Krok 1: Instalowanie Menedżera StorSimple migawki

Wykonaj następujące czynności w celu zainstalowania Menedżera migawkę StorSimple.

#### <a name="to-install-storsimple-snapshot-manager"></a>Aby zainstalować Menedżera migawkę StorSimple

1. Pobieranie oprogramowania Menedżer migawkę StorSimple (Przejdź do [Menedżera migawkę StorSimple](https://www.microsoft.com/download/details.aspx?id=44220) w programie Microsoft Download Center) i zapisz go lokalnie na hoście.

2. W Eksploratorze plików kliknij prawym przyciskiem myszy folder skompresowany, a następnie kliknij **wyodrębnić wszystkie**.

3. W oknie **Foldery wyodrębnić skompresowane (Zipped)** w polu **Wybierz miejsce docelowe i wyodrębnianie plików** wpisz lub wyszukaj ścieżkę miejsce, w którym chcesz plików do wyodrębnienia. 

       >[AZURE.IMPORTANT] Należy zainstalować Menedżer migawkę StorSimple na dysk C:.
 
4. Zaznacz pole wyboru **Pokaż wyodrębnione pliki po zakończeniu** , a następnie kliknij pozycję **Wyodrębnij**.

    ![Wyodrębnianie plików, okno dialogowe](./media/storsimple-snapshot-manager-deployment/HCS_SSM_extract_files.png) 

4. Po zakończeniu wyodrębniania zostanie otwarty folder docelowy. Kliknij dwukrotnie ikonę ustawienia aplikacji, który pojawia się w folderze docelowym.

5. Po wyświetleniu komunikatu **Pomyślnie konfiguracji** , kliknij przycisk **Zamknij**. Powinien zostać wyświetlony ikonę StorSimple migawkę Manager na komputerze stacjonarnym.

    ![ikony na pulpicie](./media/storsimple-snapshot-manager-deployment/HCS_SSM_desktop_icon.png) 

### <a name="step-2-connect-storsimple-snapshot-manager-to-a-device"></a>Krok 2: Nawiązywanie połączenia StorSimple migawkę Menedżer urządzeń

Wykonaj następujące czynności, nawiązania połączenia StorSimple migawkę Menedżer urządzeń StorSimple.

#### <a name="to-connect-storsimple-snapshot-manager-to-a-device"></a>Aby nawiązać StorSimple migawkę Menedżera urządzenia

1. Kliknij ikonę Menedżer migawkę StorSimple na komputerze stacjonarnym. Zostanie wyświetlone okno Menedżer migawkę StorSimple. Okno zawiera okienko **zakresu** , okienko **wyników** i okienko **Akcje** . 

    ![Interfejs użytkownika Menedżera migawkę StorSimple](./media/storsimple-snapshot-manager-deployment/HCS_SSM_gui_panes.png) 

    - Okienko **zakresu** (lewe okienko) zawiera listę węzłów zorganizowane w strukturze drzewa. Możesz rozwinąć niektóre węzły, aby wybrać widok lub określone dane dotyczące tego węzła. Kliknij ikonę strzałki, aby rozwinąć lub zwinąć węzeł. Kliknij prawym przyciskiem myszy odpowiedni element w okienku **zakres** , aby wyświetlić listę dostępnych akcji dla tego elementu. 

    - Okienko **wyników** (środkowe okienko) zawiera informacje szczegółowe informacje o węzeł, widoku lub dane, które zaznaczone w okienku **zakresu** .

    - Okienko **Akcje** zawiera listę operacji, które można wykonywać na węzeł, widoku lub dane, które zaznaczone w okienku **zakresu** .

    Pełny opis interfejsu użytkownika Menedżera migawkę StorSimple zobacz [Menedżer migawkę StorSimple interfejsu użytkownika](storsimple-use-snapshot-manager.md).

2. W okienku **zakres** kliknij prawym przyciskiem myszy węzeł **urządzenia** , a następnie kliknij **Konfiguruj urządzenie**. Zostanie wyświetlone okno dialogowe **Konfigurowanie urządzenia** .

    ![Konfigurowanie urządzenia](./media/storsimple-snapshot-manager-deployment/HCS_SSM_config_device.png) 

3. W polu listy **urządzeń** wybierz adres IP urządzenia Microsoft Azure StorSimple lub wirtualnych. W polu tekstowym **hasło** wpisz hasło StorSimple migawkę Menedżer utworzonego dla urządzenia w portalu klasyczny Azure. Kliknij **przycisk OK**.

4. Menedżer migawkę StorSimple wyszukiwanie określonymi urządzenia. Jeśli urządzenie jest dostępna, Menedżer migawkę StorSimple dodaje połączenie. Możesz to zrobić [Sprawdź połączenie na urządzeniu](#to-verify-the-connection) , aby potwierdzić, że połączenia został dodany pomyślnie.

    Jeśli urządzenie jest niedostępna dla dowolnego powodu, Menedżer migawkę StorSimple zwraca komunikat o błędzie. Kliknij **przycisk OK** , aby zamknąć komunikat o błędzie, a następnie kliknij przycisk **Anuluj** , aby zamknąć okno dialogowe **Konfigurowanie urządzenia** .

5. Podczas łączenia się z urządzeniem, Menedżer migawkę StorSimple importuje każdej grupy głośność skonfigurowane dla danego urządzenia, pod warunkiem, że grupie głośność jest skojarzony kopie zapasowe. Grupy głośność, które nie jest skojarzone kopie zapasowe nie zostaną zaimportowane. Ponadto kopii zapasowej zasady, które zostały utworzone dla grupy głośności nie zostaną zaimportowane. Aby wyświetlić zaimportowane grupy, kliknij prawym przyciskiem myszy węzeł **Grupy głośność** wierzchu w okienku **zakres** , a następnie kliknij przycisk **Przełącz zaimportowane grupy**.

### <a name="step-3-verify-the-connection-to-the-device"></a>Krok 3: Sprawdź połączenie na urządzeniu

Wykonaj następujące czynności, aby sprawdzić, czy Menedżer migawkę StorSimple jest połączony z urządzenia StorSimple.

#### <a name="to-verify-the-connection"></a>Aby sprawdzić połączenie

1. W okienku **zakres** kliknij pozycję węzła **urządzenia** .

    ![Stan urządzenia StorSimple Menedżera migawki](./media/storsimple-snapshot-manager-deployment/HCS_SSM_Device_status.png) 

2. Sprawdź w okienku **wyników** : 

   - Jeśli na ikonie urządzenia pojawi się zielony wskaźnik i **dostępna** jest wyświetlana w kolumnie **Stan** , urządzenie jest podłączone. 

   - Jeśli na ikonie urządzenia pojawi się czerwony wskaźnik, a w kolumnie **Stan** jest wyświetlany jest niedostępny, urządzenie nie jest podłączone. 

   - Jeśli **odświeżanie** znajduje się w kolumnie **Stan** , Menedżer migawkę StorSimple jest pobieranie głośność grupy i skojarzone kopii zapasowych podłączonego urządzenia.

## <a name="upgrade-or-reinstall-storsimple-snapshot-manager"></a>Uaktualnianie lub ponownie zainstalować Menedżera migawkę StorSimple

Należy całkowicie odinstalować Menedżera migawkę StorSimple przed uaktualnić lub ponownie zainstalować to oprogramowanie. 

Przed ponownym zainstalowaniem Menedżera migawkę StorSimple, kopię zapasową istniejącej bazy danych Menedżera migawkę StorSimple na hoście. To zapisuje informacje kopii zapasowej zasad i konfiguracji, dzięki czemu można łatwo przywrócić dane z kopii zapasowej.

Podczas uaktualniania lub ponowne zainstalowanie Menedżera migawkę StorSimple, wykonaj następujące czynności:

- Krok 1: Odinstalowywanie Menedżer migawkę StorSimple 
- Krok 2: Wykonywanie kopii zapasowej bazy danych Menedżera migawkę StorSimple 
- Krok 3: Zainstaluj ponownie Menedżera migawkę StorSimple i przywracanie bazy danych 

### <a name="step-1-uninstall-storsimple-snapshot-manager"></a>Krok 1: Odinstalowywanie Menedżer migawkę StorSimple

Wykonaj następujące czynności, aby odinstalować Menedżera migawkę StorSimple.

#### <a name="to-uninstall-storsimple-snapshot-manager"></a>Aby odinstalować Menedżera migawkę StorSimple

1. Na hoście otwórz **Panel sterowania**, kliknij pozycję **Programy**, a następnie kliknij pozycję **Programy i funkcje**.

2. W okienku po lewej stronie kliknij przycisk **Odinstaluj lub zmień program**.

3. Kliknij prawym przyciskiem myszy **StorSimple migawkę Menedżera**, a następnie kliknij polecenie **Odinstaluj**.

4. Zostanie uruchomiony program instalacyjny Menedżera migawkę StorSimple. Kliknij przycisk **Modyfikuj ustawienia**, a następnie kliknij polecenie **Odinstaluj**.

    >[AZURE.NOTE] W przypadku wszystkich procesów konsoli MMC uruchomione w tle, takich jak Menedżer migawkę StorSimple lub dysku zarządzania odinstalowywania nie powiedzie się i zostanie wyświetlony komunikat, aby zamknąć wszystkie wystąpienia programu MMC przed podjęciem próby Odinstaluj program. Wybierz pozycję **Automatycznie zamknij aplikacje i spróbuj uruchomić je po zakończeniu instalacji ponownie**, a następnie kliknij **przycisk OK**.
 
5. Po zakończeniu procesu dezinstalacji pojawi się komunikat **Pomyślnie konfiguracji** . Kliknij przycisk **Zamknij**.

### <a name="step-2-back-up-the-storsimple-snapshot-manager-database"></a>Krok 2: Wykonywanie kopii zapasowej bazy danych Menedżera migawkę StorSimple

Wykonaj następujące czynności, aby utworzyć i zapisać kopię bazy danych Menedżera migawkę StorSimple.

#### <a name="to-back-up-the-database"></a>Aby utworzyć kopię zapasową bazy danych

1. Zatrzymywanie usługi zarządzania StorSimple firmy Microsoft:

   1. Uruchom Menedżera serwera.

   2. Na pulpicie nawigacyjnym Menedżer serwera, w menu **Narzędzia** wybierz **usługi**.

   3. Na stronie **usług** wybierz pozycję **Usługa zarządzania StorSimple firmy Microsoft**.

   4. W okienku po prawej stronie w obszarze **Usługi Microsoft Management StorSimple**kliknij przycisk **Zatrzymaj usługę**.

        ![Zatrzymaj usługę Menedżera StorSimple](./media/storsimple-snapshot-manager-deployment/HCS_SSM_stop_service.png)

2. Przejdź do C:\ProgramData\Microsoft\StorSimple\BACatalog. 

    >[AZURE.NOTE] ProgramData jest to folder ukryty.

3. Znajdź plik XML wykazu, skopiuj plik i przechowywać kopię w bezpiecznym miejscu lub w chmurze.

    ![Plik kopii zapasowej wykazu StorSimple](./media/storsimple-snapshot-manager-deployment/HCS_SSM_bacatalog.png)

4. Ponowne uruchamianie usługi zarządzania StorSimple firmy Microsoft: 

    1. Na pulpicie nawigacyjnym Menedżer serwera, w menu **Narzędzia** wybierz **usługi**.

    2. Na stronie **usług** wybierz pozycję **Microsoft StorSimple zarządzania Servic**e.

    3. W okienku po prawej stronie w obszarze **Usługi zarządzania StorSimple firmy Microsoft**, kliknij **ponownie uruchomić usługę**. 

### <a name="step-3-reinstall-storsimple-snapshot-manager-and-restore-the-database"></a>Krok 3: Zainstaluj ponownie Menedżera migawkę StorSimple i przywracanie bazy danych

Aby ponownie zainstalować Menedżera migawkę StorSimple, wykonaj czynności w [Instalowanie nowego Menedżera migawkę StorSimple](#install-a-new-storsimple-snapshot-manager). Przywrócenie bazy danych Menedżera migawkę StorSimple następnie, wykonaj poniższą procedurę.

#### <a name="to-restore-the-database"></a>Aby przywrócić bazę danych

1. Zatrzymywanie usługi zarządzania StorSimple firmy Microsoft:

    1. Uruchom Menedżera serwera.

    2. Na pulpicie nawigacyjnym Menedżer serwera, w menu **Narzędzia** wybierz **usługi**.

    3. Na stronie **usług** wybierz pozycję **Usługa zarządzania StorSimple firmy Microsoft**.

    4. W okienku po prawej stronie w obszarze **Usługi Microsoft Management StorSimple**kliknij przycisk **Zatrzymaj usługę**.

2. Przejdź do C:\ProgramData\Microsoft\StorSimple\BACatalog. 

     >[AZURE.NOTE] ProgramData jest to folder ukryty.

3. Usuń plik XML wykazu i zamień go na wersję, który został wcześniej zapisany.

4. Ponowne uruchamianie usługi zarządzania StorSimple firmy Microsoft: 

    1. Na pulpicie nawigacyjnym Menedżer serwera, w menu **Narzędzia** wybierz **usługi**.

    2. Na stronie **usług** wybierz pozycję **Usługa zarządzania StorSimple firmy Microsoft**.

    3. W okienku po prawej stronie w obszarze **Usługi zarządzania StorSimple firmy Microsoft**, kliknij **ponownie uruchomić usługę**.

## <a name="next-steps"></a>Następne kroki

- Aby dowiedzieć się więcej na temat Menedżera migawkę StorSimple, przejdź do [Co to jest Menedżer migawkę StorSimple?](storsimple-what-is-snapshot-manager.md).

- Aby dowiedzieć się, jak interfejs użytkownika Menedżera migawkę StorSimple, przejdź do [interfejsu użytkownika Menedżera migawkę StorSimple](storsimple-use-snapshot-manager.md).

- Aby dowiedzieć się więcej o korzystaniu z Menedżera migawkę StorSimple, przejdź do [Menedżera migawkę StorSimple Użyj administrowania rozwiązania StorSimple](storsimple-snapshot-manager-admin.md).
