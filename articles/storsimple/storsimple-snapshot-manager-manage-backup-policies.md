<properties 
   pageTitle="Zasady kopii zapasowej Menedżera migawkę StorSimple | Microsoft Azure"
   description="Informacje dotyczące używania przystawki MMC Menedżer migawkę StorSimple do tworzenia i zarządzanie zasadami kopii zapasowej, które sterują zaplanowanych kopii zapasowych."
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
   ms.date="05/12/2016"
   ms.author="v-sharos" />

# <a name="use-storsimple-snapshot-manager-to-create-and-manage-backup-policies"></a>Tworzenie i zarządzanie zasadami kopii zapasowej za pomocą Menedżera migawkę StorSimple

## <a name="overview"></a>Omówienie

Zasady tworzenia kopii zapasowych tworzy harmonogram wykonywania kopii zapasowych danych głośność lokalnie lub w chmurze. Podczas tworzenia kopii zapasowej zasady, można określić także zasady przechowywania. (Zachować maksymalnie 64 migawek). Aby uzyskać więcej informacji o zasadach kopii zapasowej, zobacz [Typy kopii zapasowych](storsimple-what-is-snapshot-manager.md#backup-type) w [serii StorSimple 8000: rozwiązanie chmury hybrydowych](storsimple-overview.md).

Ten samouczek w tym miejscu wyjaśniono, jak:

- Tworzenie kopii zapasowej zasad 
- Edytuj zasady tworzenia kopii zapasowych 
- Usuwanie zasady tworzenia kopii zapasowych 

## <a name="create-a-backup-policy"></a>Tworzenie kopii zapasowej zasad

Poniższa procedura umożliwia utworzenie nowej zasady tworzenia kopii zapasowych.

#### <a name="to-create-a-backup-policy"></a>Aby utworzyć zasady tworzenia kopii zapasowych

1. Kliknij ikonę pulpitu, aby uruchomić Menedżera migawkę StorSimple.

2. W okienku **zakres** kliknij prawym przyciskiem myszy **Zasady kopii zapasowej**, a następnie kliknij **Utwórz zasady kopii zapasowej**.

    ![Tworzenie kopii zapasowej zasad](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_BU_policy.png)

    Zostanie wyświetlone okno dialogowe **Tworzenie zasady** . 

    ![Tworzenie zasad — ogólne](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_general.png)

3. Na karcie **Ogólne** wypełnij następujące informacje:

   1. W polu tekstowym **Nazwa** wpisz nazwę zasady.

   2. W polu tekstowym **Grupa zbiorczymi** wpisz nazwę grupy głośność skojarzone z zasad.

   3. Wybierz **migawkę lokalne** lub **migawki w chmurze**.

   4. Wybierz liczbę migawek przechowywania. Po wybraniu **wszystkich**, 64 migawki będzie zachowane (wartość maksymalna). 

4. Kliknij kartę **Harmonogram** .

    ![Tworzenie zasad — karta harmonogram](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_schedule.png)

5. Na karcie **Harmonogram** Podaj następujące informacje: 

   1. Kliknij pole wyboru **Włącz** , aby zaplanować następnej kopii zapasowej.

   2. W obszarze **Ustawienia**zaznacz **jeden raz**, **Dzienny**, **Tygodniowy**lub **Miesięczny**. 

   3. W polu **Rozpoczęcie** kliknij ikonę kalendarza, a następnie wybierz datę rozpoczęcia.

   4. W obszarze **Ustawienia zaawansowane**można ustawić opcjonalne powtarzania harmonogramów i datę końcową.

   5. Kliknij **przycisk OK**.

Po utworzeniu zasady tworzenia kopii zapasowych w okienku **wyników** zostanie wyświetlone następujące informacje:

- **Nazwa** — Nazwa zasady kopii zapasowej.

- **Typ** — migawki lokalne lub migawki chmury.

- **Grupa zbiorczymi** — grupa zbiorczymi skojarzone z zasad.

- **Przechowywanie** — liczby migawek zachowane; wartość maksymalna to 64.

- **Utworzono** — Data utworzenia tych zasad.

- **Włączone** — czy zasady są obecnie efekt: **PRAWDA** wskazuje, że jest włączone; **FAŁSZ** wskazuje, że nie jest w obiekcie. 

## <a name="edit-a-backup-policy"></a>Edytuj zasady tworzenia kopii zapasowych

Poniższa procedura umożliwia edytowanie istniejących zasad kopii zapasowej.

#### <a name="to-edit-a-backup-policy"></a>Aby edytować zasady tworzenia kopii zapasowych

1. Kliknij ikonę pulpitu, aby uruchomić Menedżera migawkę StorSimple. 

2. W okienku **zakres** kliknij węzeł **Zasady kopii zapasowej** . Kopii zapasowej zasad zostaną wyświetlone w okienku **wyników** . 

3. Kliknij prawym przyciskiem myszy zasady, które chcesz edytować, a następnie kliknij przycisk **Edytuj**. 

    ![Edytuj zasady tworzenia kopii zapasowych](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Edit_BU_policy.png) 

4. Gdy pojawi się okno **Utwórz zasady** , wprowadź zmiany, a następnie kliknij **przycisk OK**. 

## <a name="delete-a-backup-policy"></a>Usuwanie zasady tworzenia kopii zapasowych

Poniższa procedura umożliwia usunięcie zasady tworzenia kopii zapasowych.

#### <a name="to-delete-a-backup-policy"></a>Aby usunąć zasady tworzenia kopii zapasowych

1. Kliknij ikonę pulpitu, aby uruchomić Menedżera migawkę StorSimple. 

2. W okienku **zakres** kliknij węzeł **Zasady kopii zapasowej** . Kopii zapasowej zasad zostaną wyświetlone w okienku **wyników** . 

3. Kliknij prawym przyciskiem myszy kopii zapasowej zasady, które chcesz usunąć, a następnie kliknij polecenie **Usuń**.

4. Po wyświetleniu komunikatu potwierdzenia kliknij przycisk **Tak**.

    ![Usuwanie potwierdzenia zasad kopii zapasowej](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Delete_BU_policy.png)

## <a name="next-steps"></a>Następne kroki

- Dowiedz się, jak za [pomocą Menedżera migawkę StorSimple administrowania rozwiązania StorSimple](storsimple-snapshot-manager-admin.md).
- Dowiedz się, jak za [pomocą Menedżera migawkę StorSimple do wyświetlania i zarządzanie zadaniami kopii zapasowej](storsimple-snapshot-manager-manage-backup-jobs.md).
