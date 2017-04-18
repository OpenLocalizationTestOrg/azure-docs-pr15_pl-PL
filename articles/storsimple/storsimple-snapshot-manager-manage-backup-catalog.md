<properties 
   pageTitle="Wykaz kopii zapasowych Menedżer migawkę StorSimple | Microsoft Azure"
   description="Informacje dotyczące używania przystawki MMC Menedżer migawkę StorSimple do wyświetlania i zarządzania wykazu kopii zapasowych."
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
   ms.date="04/26/2016"
   ms.author="v-sharos" />

# <a name="use-storsimple-snapshot-manager-to-manage-the-backup-catalog"></a>Używanie StorSimple migawkę Manager do zarządzania wykaz kopii zapasowych

## <a name="overview"></a>Omówienie

Podstawową funkcją z Menedżera migawkę StorSimple jest umożliwiają tworzenie kopii zapasowych spójną z aplikacją wielkości StorSimple w formie migawek. Następnie migawek znajdują się w pliku XML o nazwie *wykazu kopii zapasowych*. Wykaz kopii zapasowych są rozmieszczone migawek według grupy głośności, a następnie według migawka lokalne lub migawki w chmurze. 

Ten przewodnik opisuje, jak węzeł **Katalogu kopii zapasowej** umożliwia wykonywanie następujących zadań:

- Przywracanie woluminu 
- Klonowanie głośność lub grupy głośności 
- Usuwanie kopii zapasowej 
- Odzyskiwanie pliku
- Przywracanie bazy danych Menedżera migawkę Storsimple

Rozwijanie węzła **Wykaz kopii zapasowych** w okienku **zakresu** , a następnie rozwijanie grupy głośność, możesz wyświetlić wykaz kopii zapasowych.

- Kliknięcie nazwy grupy głośność, w okienku **wyników** zawiera numer lokalny migawek i dostępnych w grupie głośność migawek chmury. 

- Kliknięcie **Migawki lokalne** lub **Migawki w chmurze**, okienku **wyniki** są wyświetlane następujące informacje o każdej kopii zapasowej migawki (w zależności od ustawień **widoku** ): 

    - **Nazwa** — czas utworzenia migawki. 

    - **Typ** — czy to migawkę lokalne lub migawki chmury. 

    - **Właściciel** — właściciel zawartości. 

    - **Dostępne** — czy migawki jest obecnie dostępny. **Wartość true** wskazuje, że migawki jest dostępny i można je przywrócić; **FAŁSZ** oznacza migawki jest już dostępny. 

    - **Zaimportowane** — czy zaimportowano kopii zapasowej. **Wartość true** wskazuje, że kopii zapasowej został zaimportowany przez usługę Menedżer StorSimple w czasie, które urządzenie został skonfigurowany w Menedżerze migawkę StorSimple; **FAŁSZ** wskazuje, że nie został zaimportowany, ale został utworzony za pomocą Menedżera migawkę StorSimple. (Można łatwo zidentyfikować grupę zaimportowanych głośność, ponieważ jest dodawany sufiks identyfikujący urządzenia, z którego został zaimportowany grupie głośności.)

    ![Wykaz kopii zapasowych](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Backup_catalog.png)

- Jeśli rozwiń **Migawkę lokalne** lub **Migawki w chmurze**, a następnie kliknij nazwę migawki poszczególnych, okienko **wyników** zawiera następujące informacje dotyczące wybranego migawki:

    - **Nazwa** — wielkość określone przez literę dysku. 

    - **Nazwa lokalnej** — nazwę lokalną dysk (jeśli jest dostępne). 

    - **Urządzenia** — nazwę urządzenia, na którym znajduje się głośność. 

    - **Dostępne** — czy migawki jest obecnie dostępny. **Wartość true** wskazuje, że migawki jest dostępny i można je przywrócić; **FAŁSZ** oznacza migawki jest już dostępny. 


## <a name="restore-a-volume"></a>Przywracanie woluminu

Poniższa procedura umożliwia przywrócenia woluminu z kopii zapasowej.

#### <a name="prerequisites"></a>Wymagania wstępne

Jeśli jeszcze tego nie zrobiono, Utwórz głośność i grupa zbiorczymi, a następnie usuń głośność. Domyślnie Menedżer migawkę StorSimple kopię zapasową woluminu przed zezwalające zostanie usunięta. W ten sposób można zapobiec utracie danych, przez przypadek usunięcie głośność lub jeśli dane mają można odzyskać dowolnego powodu. 

Menedżer migawkę StorSimple zostanie wyświetlony komunikat, podczas tworzenia kopii zapasowej ostrożności.

![Automatyczne migawkę wiadomości](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Automatic_snap.png) 

>[AZURE.IMPORTANT]Nie można usunąć woluminu, który jest częścią grupy głośność. Opcja Usuń jest niedostępna. <br>

#### <a name="to-restore-a-volume"></a>Aby przywrócić woluminu

1. Kliknij ikonę pulpitu, aby uruchomić Menedżera migawkę StorSimple. 

2. W okienku **zakres** rozwiń węzeł **Wykazu kopii zapasowej** , rozwiń grupy głośności, a następnie kliknij **Lokalne migawki** lub **Migawek chmury**. W okienku **wyników** zostanie wyświetlona lista kopii zapasowych. 

3. Znajdowanie kopii zapasowej, który chcesz przywrócić, kliknij prawym przyciskiem myszy, a następnie kliknij przycisk **Przywróć**. 

    ![Przywracanie wykaz kopii zapasowych](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Restore_BU_catalog.png) 

4. Na stronie Potwierdzenie zapoznaj się ze szczegółami, wpisz **Potwierdź**, a następnie kliknij **przycisk OK**. Menedżer migawkę StorSimple używa kopii zapasowej do przywrócenia głośność. 

    ![Przywracanie komunikatu potwierdzenia](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Restore_volume_msg.png) 

5. Akcja Przywróć można monitorować podczas jego działania. W okienku **zakres** rozwiń węzeł **zadań** , a następnie kliknij **uruchomiony**. Szczegóły zadania są wyświetlane w okienku **wyników** . Po zakończeniu zadania przywrócenia szczegóły zadania są przenoszone do listy **ostatniej 24 godzin** .

## <a name="clone-a-volume-or-volume-group"></a>Klonowanie głośność lub grupy głośności

Poniższa procedura umożliwia utworzenie duplikat (klonowanie) głośność lub grupy głośność.

#### <a name="to-clone-a-volume-or-volume-group"></a>Aby klonowanie głośność lub grupy głośności

1. Kliknij ikonę pulpitu, aby uruchomić Menedżera migawkę StorSimple.

2. W okienku **zakres** rozwiń węzeł **Wykazu kopii zapasowej** , rozwiń grupy głośności, a następnie kliknij **Migawek chmury**. W okienku **wyników** zostanie wyświetlona lista kopie zapasowe.

3. Znajdowanie głośności lub grupę głośność, która ma klonowanie, kliknij prawym przyciskiem myszy głośność lub głośność nazwę grupy, a następnie kliknij przycisk **klonowanie**. Zostanie wyświetlone okno dialogowe **Klonowanie migawkę chmury** .

    ![Klonowanie migawkę chmury](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Clone.png) 

4. Wypełnij okno dialogowe **Klonowanie chmury migawki** w następujący sposób: 

    1. W polu tekstowym **Nazwa** wpisz nazwę sklonowanym głośność. Ta nazwa będzie wyświetlana w węźle **wielkości** . 

    2. (Opcjonalnie) wybierz **dysk**, a następnie wybierz literę z listy rozwijanej. 

    3. (Opcjonalnie) wybierz pozycję **Folder (NTFS)**i wpisz ścieżkę folderu lub kliknij przycisk Przeglądaj i wybierz lokalizację folderu. 

    4. Kliknij przycisk **Utwórz**.

5. Po zakończeniu procesu klonowania należy zainicjować sklonowanym głośność. Uruchamianie Menedżera serwera, a następnie zacznij zarządzania dysku. Aby uzyskać szczegółowe instrukcje zobacz [Instalowanie wielkości](storsimple-snapshot-manager-manage-volumes.md#mount-volumes). Po zainicjowaniu, głośność zostaną wyświetlone w okienku **zakres** węźle **wielkości** . Jeśli nie widzisz wielkość na liście, Odśwież listę wielkości (kliknij prawym przyciskiem myszy węzeł **wielkości** , a następnie kliknij przycisk **Odśwież**).

## <a name="delete-a-backup"></a>Usuwanie kopii zapasowej

Poniższa procedura umożliwia usuwanie migawki z wykazu kopii zapasowych. 

>[AZURE.NOTE]Usunięcie migawkę powoduje usunięcie danych w kopii zapasowej skojarzone z migawki. Jednak proces oczyszczania danych z chmury może trochę potrwać.<br>
 
#### <a name="to-delete-a-backup"></a>Aby usunąć kopii zapasowej

1. Kliknij ikonę pulpitu, aby uruchomić Menedżera migawkę StorSimple.

2. W okienku **zakres** rozwiń węzeł **Wykazu kopii zapasowej** , rozwiń grupy głośności, a następnie kliknij **Lokalne migawki** lub **Migawek chmury**. W okienku **wyników** zostanie wyświetlona lista migawek. 

3. Kliknij prawym przyciskiem myszy migawkę, którą chcesz usunąć, a następnie kliknij polecenie **Usuń**.

4. Po wyświetleniu komunikatu potwierdzenia kliknij **przycisk OK**. 

## <a name="recover-a-file"></a>Odzyskiwanie pliku

Jeśli plik przypadkowo zostanie usunięty z woluminu, można odzyskać plik pobierania migawkę wstępnie daty usunięcie, tworzenie duplikat głośność za pomocą migawkę, a następnie skopiuj plik z woluminu sklonowanym do oryginalnego woluminu.

#### <a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem upewnij się, że masz bieżącej kopii zapasowej grupy głośność. Następnie usuń pliku zapisanego na jednym wielkości w tej grupie głośność. Na koniec wykonaj następujące czynności, aby przywrócić usunięty plik z kopii zapasowej. 

#### <a name="to-recover-a-deleted-file"></a>Aby odzyskać usuniętego pliku

1. Kliknij ikonę Menedżer migawkę StorSimple na komputerze stacjonarnym. Zostanie wyświetlone okno konsoli Menedżer migawkę StorSimple. 

2. W okienku **zakres** rozwiń węzeł **Wykazu kopii zapasowej** , a następnie przejdź do migawki zawierającej usunięty plik. Zazwyczaj należy wybrać migawki został utworzony tuż przed usunięciem. 

3. Znajdowanie głośności, który chcesz klonowanie, kliknij prawym przyciskiem myszy i kliknij pozycję **klonowanie**. Zostanie wyświetlone okno dialogowe **Klonowanie migawkę chmury** .

    ![Klonowanie migawkę chmury](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Clone.png) 

4. Wypełnij okno dialogowe **Klonowanie chmury migawki** w następujący sposób: 

   1. W polu tekstowym **Nazwa** wpisz nazwę sklonowanym głośność. Ta nazwa będzie wyświetlana w węźle **wielkości** . 

   2. (Opcjonalnie) Wybierz **dysk**, a następnie wybierz literę z listy rozwijanej. 

   3. (Opcjonalnie) Wybierz **Folder (NTFS)**i wpisz ścieżkę folderu lub kliknij przycisk **Przeglądaj** i wybierz lokalizację folderu. 

   4. Kliknij przycisk **Utwórz**. 

5. Po zakończeniu procesu klonowania należy zainicjować sklonowanym głośność. Uruchamianie Menedżera serwera, a następnie zacznij zarządzania dysku. Aby uzyskać szczegółowe instrukcje zobacz [Instalowanie wielkości](storsimple-snapshot-manager-manage-volumes.md#mount-volumes). Po zainicjowaniu, głośność zostaną wyświetlone w okienku **zakres** węźle **wielkości** . 

    Jeśli nie widzisz wielkość na liście, Odśwież listę wielkości (kliknij prawym przyciskiem myszy węzeł **wielkości** , a następnie kliknij przycisk **Odśwież**).

6. Otwórz folder NTFS, który zawiera sklonowanym głośność, rozwiń węzeł **wielkości** , a następnie otwórz sklonowanym głośność. Znajdź plik, który chcesz odzyskać, a następnie skopiować go do woluminu podstawowego.

7. Po przywróceniu pliku, możesz usunąć folder NTFS, który zawiera sklonowanym głośność.

## <a name="restore-the-storsimple-snapshot-manager-database"></a>Przywracanie bazy danych Menedżera migawkę StorSimple

Regularnie czy wykonywanie kopii zapasowej bazy StorSimple migawkę menedżera na hoście. Jeśli występuje po awarii lub komputer nie powiedzie się z dowolnego powodu, można przywrócić go z kopii zapasowej. Tworzenie kopii zapasowej bazy danych jest procesem ręcznego.

#### <a name="to-back-up-and-restore-the-database"></a>Tworzenie kopii zapasowych i przywracanie bazy danych

1. Zatrzymywanie usługi zarządzania StorSimple firmy Microsoft:

    1. Uruchom Menedżera serwera.

    2. Na pulpicie nawigacyjnym Menedżer serwera, w menu **Narzędzia** wybierz **usługi**.

    3. W oknie **usługi** wybierz **Usługę Microsoft Management StorSimple**.

    4. W okienku po prawej stronie w obszarze **Usługi Microsoft Management StorSimple**kliknij przycisk **Zatrzymaj usługę**.

2. Na hoście przejdź do C:\ProgramData\Microsoft\StorSimple\BACatalog. 

    >[AZURE.NOTE] ProgramData jest to folder ukryty.
 
3. Znajdź plik XML wykazu, skopiuj plik i przechowywać kopię w bezpiecznym miejscu lub w chmurze. Jeśli host nie powiedzie się, można odzyskać kopii zapasowej zasad utworzonych w Menedżerze migawkę StorSimple za pomocą tego pliku kopii zapasowej.

    ![Plik kopii zapasowej wykazu StorSimple Azure](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_bacatalog.png)

4. Ponowne uruchamianie usługi zarządzania StorSimple firmy Microsoft: 

    1. Na pulpicie nawigacyjnym Menedżer serwera, w menu **Narzędzia** wybierz **usługi**.
    
    2. W oknie **usługi** wybierz **Usługę Microsoft Management StorSimple**.

    3. W okienku po prawej stronie w obszarze **Usługi zarządzania StorSimple firmy Microsoft**, kliknij **ponownie uruchomić usługę**.

5. Na hoście przejdź do C:\ProgramData\Microsoft\StorSimple\BACatalog. 

6. Usuń plik XML wykazu i zamień go na wersję kopii zapasowej, utworzone przez Ciebie. 

7. Kliknij ikonę pulpitu StorSimple migawkę Manager do uruchomienia Menedżera migawkę StorSimple. 

## <a name="next-steps"></a>Następne kroki

- Dowiedz się więcej o [korzystaniu z Menedżera migawkę StorSimple administrowania rozwiązania StorSimple](storsimple-snapshot-manager-admin.md).
- Dowiedz się więcej o [StorSimple migawkę Menedżera zadań i przepływy pracy](storsimple-snapshot-manager-admin.md#storsimple-snapshot-manager-tasks-and-workflows).
