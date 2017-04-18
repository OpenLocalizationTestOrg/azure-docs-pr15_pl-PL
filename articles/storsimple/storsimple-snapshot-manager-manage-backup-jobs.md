<properties 
   pageTitle="StorSimple migawkę Menedżera zadań kopii zapasowej | Microsoft Azure"
   description="Informacje dotyczące używania przystawki MMC Menedżer migawkę StorSimple do wyświetlania i zarządzanie zadaniami kopii zapasowej według harmonogramu, obecnie uruchomione i zakończone."
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


# <a name="use-storsimple-snapshot-manager-to-view-and-manage-backup-jobs"></a>Używanie Menedżera migawkę StorSimple do wyświetlania i zarządzanie zadaniami kopii zapasowej

## <a name="overview"></a>Omówienie

Węzeł **zadania** w okienku **zakres** Wyświetla **Zaplanowane**, **ostatniego 24 godzin**i **z** kopii zapasowej zadania, które zainicjowane interakcyjnie lub przez zasady skonfigurowane. 

Ten samouczek wyjaśniono korzystaniem węzeł **zadań** do wyświetlania informacji na temat według harmonogramu, ostatnich i obecnie uruchomione zadania kopii zapasowej. (Na liście zadań i informacji pojawia się w okienku **wyników** ). Ponadto można kliknąć prawym przyciskiem myszy zadanie wymienionych, a menu kontekstowego, które zawiera listę dostępnych akcji.

## <a name="view-scheduled-jobs"></a>Wyświetlanie zaplanowanych zadań

Poniższa procedura umożliwia wyświetlanie zaplanowanych zadań kopii zapasowych.

#### <a name="to-view-scheduled-jobs"></a>Aby wyświetlić zaplanowanych zadań

1. Kliknij ikonę pulpitu, aby uruchomić Menedżera migawkę StorSimple. 

2. W okienku **zakres** rozwiń węzeł **zadań** , a następnie kliknij pozycję **Harmonogram**. W okienku **wyników** zostaną wyświetlone następujące informacje:

    - **Nazwa** — nazwa zaplanowanej migawki

    - **Uruchamianie następnej** — Data i godzina następnego zaplanowanej migawki

    - **Ostatnie uruchomienie** — Data i godzina ostatnich zaplanowanej migawki

    >[AZURE.NOTE] Dla jednorazowego tylko migawki **Uruchom dalej** i **Uruchomić ostatniego** będą takie same. 
 
    ![Zaplanowanych zadań kopii zapasowej](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_scheduled.png) 
 
3. Aby wykonać dodatkowe akcje na określonego zadania, kliknij prawym przyciskiem myszy nazwę zadania w okienku **wyników** , a następnie wybierz jedną z opcji menu.

## <a name="view-recent-jobs"></a>Wyświetlanie ostatnich zadań

Poniższa procedura umożliwia wyświetlanie kopii zapasowych i przywracanie zadań, które zostały wykonane w ciągu ostatnich 24 godzin.

#### <a name="to-view-recent-jobs"></a>Aby wyświetlić ostatnie zadania

1. Kliknij ikonę pulpitu, aby uruchomić Menedżera migawkę StorSimple.

2. W okienku **zakres** rozwiń węzeł **zadań** , a następnie kliknij **ostatni 24 godzin**. Okienko **wyników** zawiera zadania kopii zapasowej ostatnie 24 godziny (maksymalnie 64 zadań). W okienku **wyników** , w zależności od opcji **widoku** zadanej zostanie wyświetlony następujące informacje:

    - **Nazwa** — nazwę migawki według harmonogramu.
 
    - **Wprowadzenie** — Data i godzina rozpoczęcia migawki.

    - **Zatrzymano** — Data i godzina podczas migawkę na koniec lub został zakończony.

    - Czas **jaki upłynął między nimi** — ilość czasu między **wprowadzenie** i **zatrzymania** .

    - **Stan** — stan ostatnio ukończonego zadania. **Sukcesu** wskazuje kopii zapasowej został pomyślnie utworzony. **Niepowodzenie** wskazuje, że zadanie nie zostało wykonane pomyślnie.

    - **Informacje o** — przyczyny błędu.

    - **Bajtów przetwarzania (MB)** — zakres danych w grupie głośność, która została przetworzona (w MB). 

    ![Zadania, które uruchomiono w ciągu ostatnich 24 godzin](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_Last_24_hours.png) 

3. Aby wykonać dodatkowe akcje na określonego zadania, kliknij prawym przyciskiem myszy nazwę zadania w okienku **wyników** , a następnie wybierz jedną z opcji menu.

    ![Usuwanie zadania](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Delete_backup.png) 
     
## <a name="view-currently-running-jobs"></a>Wyświetla aktualnie uruchomione zadania

Przy użyciu poniższej procedury do widoku zadań, które są obecnie uruchomione.

#### <a name="to-view-currently-running-jobs"></a>Aby wyświetlić uruchomione zadania

1. Kliknij ikonę pulpitu, aby uruchomić Menedżera migawkę StorSimple.

2. W okienku **zakres** rozwiń węzeł **zadań** , a następnie kliknij przycisk **uruchamiania**. W zależności od zadanej opcje **widoku** w okienku **wyników** pojawia się następujące informacje: 

    - **Nazwa** — nazwę migawki według harmonogramu.

    - **Wprowadzenie** — Data i godzina rozpoczęcia migawki.

    - **Punkt kontrolny** — bieżącej akcji kopii zapasowej.

    - **Stan** — procent ukończenia.
    
    - **Jaki upłynął między nimi** — ilość czasu, jaki upłynął od początku kopii zapasowej. 

    - **Średnia produktywność (MB)** — stopień całkowitą liczbę bajtów danych przetwarzanych niż całkowity czas wykonywania przetwarzania (MB).

    - **Bajtów przetwarzania (MB)** — całkowitą liczbę bajtów danych przetwarzanych (w MB).

    - **Bajtów napisane (MB)** — całkowitą liczbę bajtów danych (w MB). Zawiera dane, a także metadane, a więc jest zazwyczaj większe niż przetwarzane bajtów.

    ![Obecnie uruchomionych zadań](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_running.png)

3. Aby wykonać dodatkowe akcje na określonego zadania, kliknij prawym przyciskiem myszy nazwę zadania w okienku **wyników** , a następnie wybierz jedną z opcji menu.

## <a name="next-steps"></a>Następne kroki

- Dowiedz się, jak za [pomocą Menedżera migawkę StorSimple administrowania rozwiązania StorSimple](storsimple-snapshot-manager-admin.md).
- Dowiedz się, jak za [pomocą Menedżera migawkę StorSimple zarządzać wykazu kopii zapasowych](storsimple-snapshot-manager-manage-backup-catalog.md).















            


 

