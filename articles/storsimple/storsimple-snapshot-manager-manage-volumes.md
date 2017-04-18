<properties 
   pageTitle="Menedżer migawkę StorSimple i ilości | Microsoft Azure"
   description="Informacje dotyczące używania przystawki MMC Menedżer migawkę StorSimple do wyświetlania i zarządzania wielkości i skonfigurować kopie zapasowe."
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
   ms.date="04/18/2016"
   ms.author="v-sharos" />

# <a name="use-storsimple-snapshot-manager-to-view-and-manage-volumes"></a>Wyświetlanie i zarządzanie nimi wielkości za pomocą Menedżera migawkę StorSimple

## <a name="overview"></a>Omówienie

Węzeł Menedżer migawkę StorSimple **wielkości** (w okienku **zakresu** ) umożliwia wybierz wielkości i wyświetlanie informacji o nich. Wielkość są przedstawiane jako dyski, które odpowiadają do wielkości zainstalowanych przez hosta. Węzeł **wielkości** zawiera lokalne wielkości i typy głośność, które są obsługiwane przez StorSimple, włączając wielkość wykryte iSCSI i urządzenia. 

Aby uzyskać więcej informacji na temat obsługiwanych wielkości przejdź do [obsługi wielu typów głośność](storsimple-what-is-snapshot-manager.md#support-for-multiple-volume-types).

![Głośność listy w okienku wyników](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Volume_node.png)

Węzeł **wielkości** pozwala także Skanuj lub usuwanie wielkości, gdy Menedżer migawkę StorSimple wykryje je. 

Ten samouczek wyjaśniono, jak można zainstalować, zainicjować i formatować i Menedżer migawkę StorSimple do:

- Wyświetlanie informacji o wielkości 
- Usuwanie wielkości
- Skanuj wielkości 
- Konfigurowanie woluminu podstawowego i jego kopię zapasową
- Konfigurowanie dynamicznego woluminu lustrzane i jego kopię zapasową

>[AZURE.NOTE] Wszystkie akcje węzeł **Głośność** są również dostępne w okienku **akcji** .
 
## <a name="mount-volumes"></a>Wielkości instalacji

Poniższa procedura umożliwia instalacji, zainicjować i formatować StorSimple. Zarządzanie dysku, narzędzie systemu zarządzania dyskach twardych i odpowiadające im wielkości lub partycje użyto w tej procedurze. Aby uzyskać więcej informacji na temat zarządzania dysku przejdź do [Zarządzania dysku](https://technet.microsoft.com/library/cc770943.aspx) w witrynie Microsoft TechNet w sieci Web.

#### <a name="to-mount-volumes"></a>Aby zainstalować wielkości

1. Na komputerze hosta Uruchom inicjatorach firmy Microsoft.

2. Go adresy IP interfejsu portalu docelowej lub adres IP odnajdowania i połączyć się z urządzeniem. Po podłączeniu urządzenia wielkość będzie dostępne dla systemu Windows. Aby uzyskać więcej informacji o używaniu inicjatorach firmy Microsoft, przejdź do sekcji "Nawiązywanie połączenia z urządzeniem docelowym iSCSI" w [Instalowanie i konfigurowanie programu Microsoft inicjator iSCSI][1].

3. Aby uruchomić Zarządzanie dysku, użyj jednej z następujących opcji:

    - W polu **Uruchom** , wpisz Diskmgmt.msc.

    - Uruchamianie Menedżera serwera, rozwiń węzeł **miejsca do magazynowania** , a następnie wybierz **Zarządzania dysku**.

    - Rozpoczynanie **Narzędzia administracyjne**, rozwiń węzeł **Zarządzanie komputerem** , a następnie wybierz **Zarządzania dysku**. 

    >[AZURE.NOTE] Aby uruchomić Zarządzanie dysku, należy użyć uprawnień administratora.
 
4. Sporządzanie woluminy w trybie online:

   1. W obszarze Zarządzanie dysku kliknij prawym przyciskiem myszy żadnego woluminu oznaczone **Offline**.

   2. Kliknij **ponownie uaktywnić dysk**. Dysk jest zaznaczony **Online** po ponownym uaktywnieniu.

5. Inicjowanie woluminy:

   1. Kliknij prawym przyciskiem myszy wielkość odkryte.

   2. W menu wybierz **Zainicjować dysk**.

   3. W oknie dialogowym **Inicjowanie dysku** Wybierz dyski, które chcesz zainicjować, a następnie kliknij **przycisk OK**.

6. Formatowanie wielkości proste:

   1. Kliknij prawym przyciskiem myszy woluminu, który chcesz sformatować.

   2. W menu wybierz **Nowy prosty**.

   3. Kreator prostych nowy do formatowania woluminu:

      - Określ wielkość głośność.
      - Wprowadź literę.
      - Wybierz pozycję system plików NTFS.
      - Określ wielkość jednostek przydziału 64 KB.
      - Wykonaj szybkie formatowanie.

7. Formatowanie wielu partition wielkości. Aby uzyskać instrukcje przejdź do sekcji "Partycje i ilości" w [Implementacji zarządzania dysku](https://msdn.microsoft.com/library/dd163556.aspx).

## <a name="view-information-about-your-volumes"></a>Wyświetlanie informacji o swojej wielkości

Poniższa procedura umożliwia wyświetlanie informacji o lokalnym i ilości Azure StorSimple.

#### <a name="to-view-volume-information"></a>Aby wyświetlić informacje o głośności

1. Kliknij ikonę pulpitu, aby uruchomić Menedżera migawkę StorSimple. 

2. W okienku **zakres** kliknij węzeł **wielkości** . W okienku **wyników** zostanie wyświetlona lista wielkości lokalnych i zainstalowany, włączając wszystkie wielkość Azure StorSimple. Kolumny w okienku **wyników** są konfigurowane. (Kliknij prawym przyciskiem myszy węzeł **wielkości** , wybierz pozycję **Widok**, a następnie wybierz **Dodaj/Usuń kolumny**.)

    ![Konfigurowanie kolumn](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_View_volumes.png)

    Kolumny wyników | Opis 
    :--------------|:-------------
    Nazwa           | W kolumnie **Nazwa** zawiera literę przypisane do każdego woluminu odkryte.
    Urządzenie         | W kolumnie **urządzenie** zawiera adres IP urządzenia podłączonego do komputera hosta.
    Nazwa głośność urządzenia | W kolumnie **Nazwa głośność urządzenia** zawiera nazwę głośność urządzenia, do którego należy wybranego woluminu. Jest to nazwa głośność zdefiniowane w portalu klasyczny Azure dla tego woluminu określonego.
    Ścieżek dostępu   | W kolumnie **Ścieżek dostępu** wyświetlany ścieżkę dostępu do woluminu. Jest to dysk litery lub instalacji punkt przecięcia głośność jest dostępny na hoście.
 
## <a name="delete-a-volume"></a>Usuwanie woluminu

Poniższa procedura umożliwia usuwanie woluminu za pomocą Menedżera migawkę StorSimple.

>[AZURE.NOTE] Nie można usunąć woluminu, jeśli jest częścią dowolnej grupie głośność. (Opcja Usuń nie jest dostępna dla ilości, które należą do grupy głośność). Należy usunąć grupę całego woluminu, aby usunąć głośność.


#### <a name="to-delete-a-volume"></a>Aby usunąć woluminu

1. Kliknij ikonę pulpitu, aby uruchomić Menedżera migawkę StorSimple.

2. W okienku **zakres** kliknij węzeł **wielkości** . 

3. W okienku **wyników** kliknij prawym przyciskiem myszy woluminu, który chcesz usunąć.

4. W menu kliknij polecenie **Usuń**. 

    ![Usuwanie woluminu](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Delete_volume.png) 

5. Zostanie wyświetlone okno dialogowe **Usuwanie głośność** . W polu tekstowym wpisz **Potwierdź** , a następnie kliknij **przycisk OK**.

6. Domyślnie Menedżer migawkę StorSimple kopię zapasową woluminu zanim zostanie usunięta. W ten sposób można ochronę przed utratą danych jeśli niezamierzony został usunięty. Menedżer migawkę StorSimple wyświetli komunikat postępu **Migawki automatyczne** , podczas jego kopię zapasową głośność. 

    ![Automatyczne migawkę wiadomości](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Automatic_snap.png) 

## <a name="rescan-volumes"></a>Skanuj wielkości

Poniższa procedura umożliwia Skanuj wielkość podłączony do Menedżera migawkę StorSimple.

#### <a name="to-rescan-the-volumes"></a>Aby Skanuj wielkość

1. Kliknij ikonę pulpitu, aby uruchomić Menedżera migawkę StorSimple.

2. W okienku **zakres** **wielkości**kliknij prawym przyciskiem myszy, a następnie kliknij **Skanuj wielkości**.

    ![Skanuj wielkości](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Rescan_volumes.png)
 
    Ta procedura synchronizuje listę głośność przy użyciu Menedżera migawkę StorSimple. Wszelkie zmiany, takie jak nowe woluminy lub usunięte, będą odzwierciedlane w wynikach.

## <a name="configure-and-back-up-a-basic-volume"></a>Konfigurowanie i wykonywanie kopii zapasowej woluminu podstawowego

Poniższa procedura umożliwia konfigurowanie kopii zapasowej woluminu podstawowego, a następnie od razu rozpocząć tworzenie kopii zapasowej lub tworzenie zasad zaplanowanych kopii zapasowych.

### <a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem:

- Upewnij się, komputer urządzenia i hosta StorSimple są poprawnie skonfigurowane. Aby uzyskać więcej informacji przejdź do [Rozmieszczanie urządzenia StorSimple lokalnego](storsimple-deployment-walkthrough-u2.md).

- Zainstaluj i skonfiguruj StorSimple migawkę menedżera. Aby uzyskać więcej informacji przejdź do [Wdrożenia Menedżera migawkę StorSimple](storsimple-snapshot-manager-deployment.md).

#### <a name="to-configure-backup-of-a-basic-volume"></a>Aby skonfigurować kopii zapasowej woluminu podstawowego

1. Tworzenie podstawowego woluminu na tym urządzeniu StorSimple.

2. Zainstaluj, zainicjować i formatowanie głośność, zgodnie z opisem w [instalacji wielkości](#mount-volumes). 

3. Kliknij ikonę Menedżer migawkę StorSimple na komputerze stacjonarnym. Zostanie wyświetlone okno Menedżer migawkę StorSimple. 

4. W okienku **zakres** kliknij prawym przyciskiem myszy węzeł **wielkości** , a następnie wybierz **Skanuj wielkości**. Po zakończeniu skanowania listy wielkości powinien być wyświetlany w okienku **wyników** . 

5. W okienku **wyników** kliknij prawym przyciskiem myszy głośności, a następnie wybierz **Utwórz grupę głośność**. 

    ![Tworzenie grupy głośności](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Create_volume_group.png) 

6. W oknie dialogowym **Utwórz grupę głośność** wpisz nazwę grupy głośność, przypisywanie do niego wielkości, a następnie kliknij **przycisk OK**.

7. W okienku **zakres** rozwiń węzeł **Grupy głośność** . Nowa grupa głośność powinien zostać wyświetlony węźle **Grupy głośność** . 

8. Kliknij prawym przyciskiem myszy nazwę grupy głośność.

    - Aby uruchomić interakcyjnych zadanie kopii zapasowej (na żądanie) kliknij polecenie **Wykonaj kopię zapasową**. 

    - Aby zaplanować automatyczne wykonywanie kopii zapasowej, kliknij pozycję **Utwórz zasady kopii zapasowej**. Na stronie **głównej** wybierz grupę głośność z listy. Na stronie **Harmonogram** wprowadź szczegóły harmonogramu. Gdy skończysz, kliknij **przycisk OK**. 

9. Aby potwierdzić rozpoczęcia zadania wykonywania kopii zapasowej, rozwiń węzeł **zadania** w okienku **zakres** , a następnie kliknij węzeł **uruchomiony** . Na liście obecnie uruchomione zadania zostanie wyświetlony w okienku **wyników** . 

## <a name="configure-and-back-up-a-dynamic-mirrored-volume"></a>Konfigurowanie i wykonywanie kopii zapasowej woluminu dynamicznego lustrzane

Wykonaj poniższe czynności, aby skonfigurować kopii zapasowej dynamiczne woluminu lustrzane:

- Krok 1: Użyj Zarządzanie dysku do utworzenia woluminu dynamicznego lustrzane. 

- Krok 2: Za pomocą StorSimple migawkę Menedżera Konfigurowanie kopii zapasowej.

### <a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem:

- Upewnij się, komputer urządzenia i hosta StorSimple są poprawnie skonfigurowane. Aby uzyskać więcej informacji przejdź do [Rozmieszczanie urządzenia StorSimple lokalnego](storsimple-deployment-walkthrough-u2.md).

- Zainstaluj i skonfiguruj StorSimple migawkę menedżera. Aby uzyskać więcej informacji przejdź do [Wdrożenia Menedżera migawkę StorSimple](storsimple-snapshot-manager-deployment.md).

- Konfigurowanie dwóch objętości na tym urządzeniu StorSimple. (W przykładzie wielkość dostępne są **dysk 1** i **2 dysku**). 

### <a name="step-1-use-disk-management-to-create-a-dynamic-mirrored-volume"></a>Krok 1: Zarządzanie dysku do utworzenia woluminu lustrzane dynamiczne

Zarządzanie dysku jest narzędziem systemu zarządzania dyskach twardych i wielkości lub partycje, które są w nich zawarte. Aby uzyskać więcej informacji na temat zarządzania dysku przejdź do [Zarządzania dysku](https://technet.microsoft.com/library/cc770943.aspx) w witrynie Microsoft TechNet w sieci Web.

#### <a name="to-create-a-dynamic-mirrored-volume"></a>Aby utworzyć lustrzane woluminu dynamicznego

1. Aby uruchomić Zarządzanie dysku, użyj jednej z następujących opcji: 

   - Otwarcie okna **Uruchamianie** wpisz **Diskmgmt.msc**, a następnie naciśnij klawisz Enter.

   - Uruchamianie Menedżera serwera, rozwiń węzeł **miejsca do magazynowania** , a następnie wybierz **Zarządzania dysku**. 

   - Rozpoczynanie **Narzędzia administracyjne**, rozwiń węzeł **Zarządzanie komputerem** , a następnie wybierz **Zarządzania dysku**. 

2. Upewnij się, czy masz dwie ilości dostępne na tym urządzeniu StorSimple. (W przykładzie wielkość dostępne są **dysk 1** i **2 dysku**). 

3. W oknie Zarządzanie dysku, w prawej kolumnie dolnym okienku kliknij prawym przyciskiem myszy **dysk 1** i wybierz **Nowy zdublowany**. 

    ![Nowy lustrzane](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_New_mirrored_volume.png) 

4. Na stronie **Nowy zdublowany** kreatora kliknij przycisk **Dalej**.

5. Na stronie **Wybierz dyski** wybierz **dysk 2** w okienku **wybrane** , kliknij przycisk **Dodaj**, a następnie kliknij **Dalej**. 

6. Na stronie **Przypisywanie litery dysku lub ścieżki** Zaakceptuj ustawienia domyślne, a następnie kliknij przycisk **Dalej**. 

7. Na stronie **Głośność Format** w polu **Rozmiar jednostki alokacji** wybierz **64 KB**. Zaznacz pole wyboru **Wykonaj szybkie formatowanie** , a następnie kliknij przycisk **Dalej**. 

8. Na stronie **Kończenie pracy nowy zdublowany** Przejrzyj ustawienia, a następnie kliknij przycisk **Zakończ**. 

9. Aby wskazać, że dysk podstawowy jest konwertowana na dysk dynamiczny zostanie wyświetlony komunikat. Kliknij przycisk **Tak**.

    ![Dysk dynamiczny konwersji wiadomości](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Disk_management_msg.png) 

10. W zarządzaniu dysku upewnij się, że dysk 1 i 2 dysku są wyświetlane jako dynamiczne lustrzane. (**Dynamiczne** powinien być wyświetlany w kolumnie Stan, a kolor paska zdolności powinien zmienić się na czerwony, wskazująca woluminu lustrzane odbicie). 

    ![Dyski dynamiczne zarządzanie zdublowany dysku](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Verify_dynamic_disks_2.png) 
 
### <a name="step-2-use-storsimple-snapshot-manager-to-configure-backup"></a>Krok 2: Używanie StorSimple migawkę Manager do skonfigurowania kopii zapasowej

Poniższa procedura umożliwia konfigurowanie woluminu dynamicznego lustrzane odbicie, a następnie od razu rozpocząć tworzenie kopii zapasowej lub tworzenie zasad zaplanowanych kopii zapasowych.

#### <a name="to-configure-backup-of-a-dynamic-mirrored-volume"></a>Aby skonfigurować kopii zapasowej woluminu dynamicznego lustrzane

1. Kliknij ikonę Menedżer migawkę StorSimple na komputerze stacjonarnym. Zostanie wyświetlone okno Menedżer migawkę StorSimple. 

2. W okienku **zakres** kliknij prawym przyciskiem myszy węzeł **wielkości** , a następnie wybierz pozycję **Skanuj wielkości**. Po zakończeniu skanowania listy wielkości powinien być wyświetlany w okienku **wyników** . Wielkość lustrzane dynamiczne jest wyświetlany jako pojedynczy głośność. 

3. W okienku **wyników** kliknij prawym przyciskiem myszy dynamiczne głośność lustrzane odbicie, a następnie kliknij **Utwórz grupę głośność**. 

4. W oknie dialogowym **Utwórz grupę głośność** wpisz nazwę grupy głośność, przypisywanie wielkość lustrzane dynamicznej do tej grupy, a następnie kliknij **przycisk OK**. 

5. W okienku **zakres** rozwiń węzeł **Grupy głośność** . Nowa grupa głośność powinien zostać wyświetlony węźle **Grupy głośność** . 

6. Kliknij prawym przyciskiem myszy nazwę grupy głośność. 

    - Aby uruchomić interakcyjnych zadanie kopii zapasowej (na żądanie) kliknij polecenie **Wykonaj kopię zapasową**. 

    - Aby zaplanować automatyczne wykonywanie kopii zapasowej, kliknij pozycję **Utwórz zasady kopii zapasowej**. Na stronie **głównej** wybierz grupę głośność z listy. Na stronie **Harmonogram** wprowadź szczegóły harmonogramu. Gdy skończysz, kliknij **przycisk OK**. 

7. Można monitorować zadania wykonywania kopii zapasowej, podczas jego działania. W okienku **zakres** rozwiń węzeł **zadań** , a następnie kliknij **uruchomiony**, szczegóły zadania są wyświetlane w okienku **wyników** . Po zakończeniu zadania kopii zapasowej, dane są przenoszone do listy zadań **ostatniej 24** godzin. 

## <a name="next-steps"></a>Następne kroki

- Dowiedz się, jak za [pomocą Menedżera migawkę StorSimple administrowania rozwiązania StorSimple](storsimple-snapshot-manager-admin.md).
- Dowiedz się, jak za [pomocą Menedżera migawkę StorSimple tworzenie grup i zarządzanie nimi głośność](storsimple-snapshot-manager-manage-volume-groups.md).

<!--Reference links-->
[1]: https://msdn.microsoft.com/library/ee338480(v=ws.10).aspx
