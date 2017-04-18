<properties 
   pageTitle="Menedżer migawkę StorSimple głośność grupy | Microsoft Azure"
   description="Informacje dotyczące używania przystawki MMC Menedżer migawkę StorSimple do tworzenia i zarządzanie grupami głośność."
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

# <a name="use-storsimple-snapshot-manager-to-create-and-manage-volume-groups"></a>Tworzenie i zarządzanie grupami głośność za pomocą Menedżera migawkę StorSimple

## <a name="overview"></a>Omówienie

Węzeł **Grupy obrót** w okienku **zakresu** umożliwia przypisywanie wielkości grupom głośność, wyświetlać informacje o grupie głośność, zaplanować wykonywanie kopii zapasowych i edytowanie grupy głośność. 

Głośność grupy są pul wielkości powiązanych pozwalają zagwarantować, że kopie zapasowe są spójne aplikacji. Uzyskać więcej informacji zobacz [wielkości i głośność grup](storsimple-what-is-snapshot-manager.md#volumes-and-volume-groups) i [integrację z usługą kopiowania w tle woluminu systemu Windows](storsimple-what-is-snapshot-manager.md#integration-with-windows-volume-shadow-copy-service).

>[AZURE.IMPORTANT] 
>
> * Wszystkie ilości w grupie głośność musi pochodzić z chmury jednego usługodawcę.
> 
> * Po skonfigurowaniu grupy głośności nie mieszać udostępnione klaster wielkości (CSVs) i nie CSVs w tej samej grupie głośność. Menedżer migawkę StorSimple nie obsługuje CSVs i innych niż CSVs w tym samym migawkę.
 
![Węzeł grupy głośności](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Volume_groups.png)

**Rysunek 1: Węzeł StorSimple migawkę Menedżer głośności grupy** 

Ten samouczek wyjaśniono, jak korzystając z Menedżera migawkę StorSimple w celu:

- Wyświetlanie informacji o grup głośności 
- Tworzenie grupy głośności
- Wykonywanie kopii zapasowej grupy głośności
- Edytowanie grupy głośności
- Usuwanie grupy głośności

Wszystkie te akcje są również dostępne w okienku **akcji** .
 
## <a name="view-volume-groups"></a>Wyświetlanie grup głośności

Po kliknięciu węzeł **Grupy głośności** w okienku **wyników** zawiera następujące informacje o każdej grupy głośność, w zależności od dokonanych wyborów kolumny. (Kolumny w okienku **wyników** są konfigurowane. Kliknij prawym przyciskiem myszy węzeł **wielkości** , wybierz **Widok**i następnie wybierz pozycję **Dodaj/Usuń kolumny**.)

Kolumny wyników | Opis 
:--------------|:------------ 
Nazwa           | W kolumnie **Nazwa** zawiera nazwę grupy głośność.
Aplikacji    | Kolumna **aplikacje** pokazuje liczbę VSS autorzy obecnie zainstalowane i uruchomione na hoście systemu Windows.
Zaznaczone       | Kolumny **wybrane** zawiera numer wielkości, które są zawarte w grupie głośność. Wartość zero (0) wskazuje, że aplikacji nie jest skojarzone z wielkość w grupie głośność.
Zaimportowane       | W kolumnie **zaimportowanych** pokazany zaimportowanych objętości. Gdy jest ustawiony na **wartość PRAWDA**, ta kolumna wskazuje, że grupy głośności zostały zaimportowane z portalem klasyczny Azure i nie został utworzony w Menedżerze migawkę StorSimple.
 
>[AZURE.NOTE] Menedżer migawkę StorSimple głośność grupy są również wyświetlane na karcie **Zasady kopii zapasowych** w portalu klasyczny Azure.
 
## <a name="create-a-volume-group"></a>Tworzenie grupy głośności

Poniższa procedura umożliwia tworzenie grupy głośność.

#### <a name="to-create-a-volume-group"></a>Aby utworzyć grupę głośności

1. Kliknij ikonę pulpitu, aby uruchomić Menedżera migawkę StorSimple. 

2. W okienku **zakres** **Głośność grupy**kliknij prawym przyciskiem myszy, a następnie kliknij **Utwórz grupę głośność**. 

    ![Tworzenie grupy głośności](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Create_volume_group.png)
 
    Zostanie wyświetlone okno dialogowe **Utwórz grupę głośność** . 

    ![Okno dialogowe grupa głośność tworzenia](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_CreateVolumeGroup_dialog.png) 

3.  Wprowadź następujące informacje: 

    1. W polu **Nazwa** wpisz unikatową nazwę dla nowej grupy głośność. 

    2. W oknie dialogowym **aplikacji** wybierz pozycję Programy skojarzone z wielkość, które zostanie dodany do grupy głośność. 

        Tylko te aplikacje, które używają wielkości StorSimple, a autorzy VSS włączone dla nich list okno **aplikacji** . Edytor VSS jest włączone tylko wtedy, gdy wszystkie ilości, które writer zna ilości StorSimple. Jeśli pole aplikacje jest pusta, żadnych aplikacji używające wielkości Azure StorSimple i masz obsługiwane autorzy VSS są instalowane. (Obecnie Azure StorSimple obsługuje programu Microsoft Exchange i SQL Server). Aby uzyskać więcej informacji na temat autorzy VSS zobacz [Integracja z usługą kopiowania w tle woluminu systemu Windows](storsimple-what-is-snapshot-manager.md#integration-with-windows-volume-shadow-copy-service).

        Jeśli wybierzesz aplikacji, ilości wszystkich skojarzonych z nim również automatycznie zaznaczona. I odwrotnie jeśli wybierzesz wielkości skojarzone z określonej aplikacji, aplikacja jest automatycznie wybrana w oknie **aplikacji** . 

    3. W oknie dialogowym **wielkości** wybierz ilości StorSimple, aby dodać do grupy głośność. 

      - Możesz umieścić ilości z jednego lub wielu partycje. (Przypisaną partition może być dyski dynamiczne lub dysków podstawowych z wielu partycje.) Woluminu, który zawiera wiele partycje jest traktowany jako całość. W związku z tym po dodaniu do grupy głośność tylko jeden partycje wszystkie inne partycje są automatycznie dodawane do tej grupy głośności w tym samym czasie. Po dodaniu wielu głośność partition do grupy głośność wielu głośność partition w dalszym ciągu jest traktowana jako całość.

      - Możesz utworzyć grupy pustego głośność przypisując nie każdy do nich. 

      - Nie mieszać udostępnione klaster wielkości (CSVs) i nie CSVs w tej samej grupie głośność. Menedżer migawkę StorSimple nie obsługuje wielkości CSV i ilości-CSV w tym samym migawkę. 

4. Kliknij **przycisk OK** , aby zapisać grupie głośność.

## <a name="back-up-a-volume-group"></a>Wykonywanie kopii zapasowej grupy głośności

Poniższa procedura umożliwia wykonywanie kopii zapasowej grupy głośność.

#### <a name="to-back-up-a-volume-group"></a>Aby utworzyć kopię zapasową grupy głośności

1. Kliknij ikonę pulpitu, aby uruchomić Menedżera migawkę StorSimple.

2. W okienku **zakres** rozwiń węzeł **Głośność grup** , kliknij prawym przyciskiem myszy nazwę grupy głośności, a następnie kliknij **Sporządzanie kopii zapasowej**. 

    ![Wykonywanie kopii zapasowej grupa zbiorczymi natychmiast](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Take_backup.png)

3. W oknie dialogowym **Kopia zapasowa wykonać** wybierz **Migawkę lokalne** lub **Migawki w chmurze**, a następnie kliknij **Utwórz**. 

    ![Sporządzanie okna dialogowego kopii zapasowej](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_TakeBackup_dialog.png) 

4. Aby potwierdzić, że kopia zapasowa jest uruchomiony, rozwiń węzeł **zadań** , a następnie kliknij **uruchomiony**. Wykonywanie kopii zapasowej powinny znajdować się na liście.

5. Aby wyświetlić migawkę złożonym, rozwiń węzeł **Wykaz narzędzia Kopia zapasowa** , rozwiń głośność nazwę grupy, a następnie kliknij **Migawkę lokalne** lub **Migawki chmury**. Wykonywanie kopii zapasowej zostaną wyświetlone, jeśli zostało zakończone pomyślnie. 

## <a name="edit-a-volume-group"></a>Edytowanie grupy głośności

Poniższa procedura umożliwia edytowanie grupy głośność.

#### <a name="to-edit-a-volume-group"></a>Aby edytować grupę głośności

1. Kliknij ikonę pulpitu, aby uruchomić Menedżera migawkę StorSimple.

2. W okienku **zakres** rozwiń węzeł **Głośność grup** , kliknij prawym przyciskiem myszy nazwę grupy głośności, a następnie kliknij **Edytuj**. 

3. Zostanie wyświetlone okno dialogowe **Utwórz grupę głośność **. Można zmienić **nazwy**, **aplikacji**i **ilości** pozycje. 

4. Kliknij **przycisk OK** , aby zapisać zmiany.

## <a name="delete-a-volume-group"></a>Usuwanie grupy głośności

Poniższa procedura umożliwia usuwanie grupy głośność. 

>[AZURE.WARNING] To powoduje także usunięcie wszystkich skojarzonych z grupą głośność kopii zapasowych.

#### <a name="to-delete-a-volume-group"></a>Aby usunąć grupę głośności

1. Kliknij ikonę pulpitu, aby uruchomić Menedżera migawkę StorSimple. 

2. W okienku **zakres** rozwiń węzeł **Głośność grup** , kliknij prawym przyciskiem myszy nazwę grupy głośności, a następnie kliknij **Usuń**. 

3. Zostanie wyświetlone okno dialogowe **Usuwanie grupy głośność** . W polu tekstowym wpisz **Potwierdź** , a następnie kliknij **przycisk OK**. 

    Grupa usuniętego woluminu znika z listy w okienku **wyników** i wszystkie kopie zapasowe, które są skojarzone z tej grupy głośność są usuwane z katalogu kopii zapasowej.

## <a name="next-steps"></a>Następne kroki

- Dowiedz się, jak za [pomocą Menedżera migawkę StorSimple administrowania rozwiązania StorSimple](storsimple-snapshot-manager-admin.md).
- Dowiedz się, jak za [pomocą Menedżera migawkę StorSimple tworzenie i zarządzanie zasadami kopii zapasowej](storsimple-snapshot-manager-manage-backup-policies.md).
