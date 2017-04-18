<properties 
   pageTitle="Przywracanie z kopii zapasowej woluminu StorSimple | Microsoft Azure"
   description="Wyjaśniono, jak za pomocą strony wykazu kopii zapasowych usługi Menedżera StorSimple przywrócenia woluminu StorSimple z zestawu kopii zapasowych."
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

# <a name="restore-a-storsimple-volume-from-a-backup-set-update-2"></a>Przywracanie woluminu StorSimple z zestawu kopii zapasowych (Aktualizuj 2)

[AZURE.INCLUDE [storsimple-version-selector-restore-from-backup](../../includes/storsimple-version-selector-restore-from-backup.md)]

## <a name="overview"></a>Omówienie

Strona **Wykazu kopii zapasowych** zawiera wszystkie zestawy kopii zapasowych, które zostały utworzone podczas ręcznego lub automatycznego wykonywania kopii zapasowych. Ta strona umożliwia listę wszystkich kopii zapasowych zasady tworzenia kopii zapasowych lub obrót, zaznacz lub usuń kopie zapasowe lub użyć kopii zapasowej do przywrócenia lub klonowanie woluminu.

 ![Aby utworzyć kopię zapasową strony wykazu](./media/storsimple-restore-from-backup-set-u2/restore.png)

Ten samouczek wyjaśniono, jak przywrócić urządzenia z zestawu kopii zapasowych za pomocą strony **Wykazu kopii zapasowych** .

Można przywrócić woluminu z dysku lokalnego lub wykonywanie kopii zapasowych w chmurze. W obu przypadkach Przywracanie funkcje informowania o wielkość w trybie online bezpośrednio w przypadku, gdy dane są pobierane w tle. 

Przed rozpoczęciem operacji przywracania należy pamiętać o następujących czynności:

- **Musi mieć wielkość w trybie offline** — sporządzanie wielkość w trybie offline na hoście i urządzenie przed Rozpocznij operację przywracania. Mimo że przywracanie automatycznie określa wielkość online na urządzeniu, należy ręcznie przełączyć urządzenia w trybie online na hoście. Możesz zabrać ze sobą wielkość online na hoście jak głośność jest w trybie online na urządzeniu. (Nie trzeba czekać dopiero po zakończeniu operacji przywracania.) Aby uzyskać procedury przejdź do [pobrać próbkę w trybie offline](storsimple-manage-volumes-u2.md#take-a-volume-offline).

- **Typ woluminu po przywróceniu** — wielkości usunięte zostaną przywrócone zależy od typu migawki; oznacza to, że ilości, które zostały lokalnie przypięte zostaną przywrócone jako lokalnie przypięte i ilości, które zostały tiered zostaną przywrócone jako warstwowych.

    Dla istniejących wielkości bieżącego typu zastosowania wielkości zastępuje typ, który jest przechowywany w migawki. Na przykład jeśli przywrócisz woluminu z migawki, która została wykonana po typu woluminu została tiered i typ woluminu lokalnie jest teraz przypięty (z powodu operacja konwersji, która została wykonana), następnie wielkość zostanie przywrócony jako lokalnie przypięty głośność. Podobnie jeśli istniejącego woluminu lokalnie przypięty została rozwinięta, a następnie przywrócić z migawki starsze wykonana po wielkość była mniejsza, wielkość przywrócenie zachować bieżący rozmiar rozwinięty.

    Nie można przekonwertować woluminu z woluminu warstwowych do woluminu lokalnie przypięty lub lokalnie przypięty głośność woluminu warstwowych, podczas przywróceniu głośność. Poczekaj, aż zakończeniu operacji przywracania, a następnie można przekonwertować wielkość innego typu. Aby dowiedzieć się, jak przekonwertowaniu woluminu przejdź do sekcji [Zmienianie typu woluminu](storsimple-manage-volumes-u2.md#change-the-volume-type). 

- **Rozmiar woluminu zostaną odzwierciedlone w przywróconym głośność** — jest to ważne, jeśli jest przywracane lokalnie przypięty głośność, który został usunięty (ponieważ jest w pełni zainicjowano wielkości lokalnie przypięty). Upewnij się, że masz wystarczającą ilością miejsca przed podjęciem próby przywrócenia woluminu lokalnie przypiętych, który wcześniej został usunięty. 

- **Nie można rozwinąć głośność podczas go przywrócić** — Czekaj dopiero po zakończeniu operacji przywracania przed podjęciem próby rozwiń głośność. Aby dowiedzieć się, jak rozwijanie woluminu przejdź do [Modyfikuj woluminu](storsimple-manage-volumes-u2.md#modify-a-volume).

- **Można wykonać kopię zapasową, gdy jest przywracane woluminu lokalnego** — procedury przejdź do tematu [Używanie usługę Menedżer StorSimple do zarządzania zasadami kopii zapasowej](storsimple-manage-backup-policies.md).

- **Możesz anulować operacji przywracania** — w przypadku anulowania zadania przywrócenia, a następnie wielkość zostanie wycofana do stanu sprzed zainicjowane przywracanie. Aby uzyskać procedury przejdź do [anulowanie zadania](storsimple-manage-jobs-u2.md#cancel-a-job).

## <a name="how-to-use-the-backup-catalog"></a>Jak za pomocą wykazu kopii zapasowych

Strony **Wykazu kopii zapasowych** zawiera kwerendę, która ułatwia zawęzić kopii zapasowej Ustawianie zaznaczenia. Można filtrować zestawy kopii zapasowych pobieranych na podstawie następujących parametrów:

- **Urządzenia** — urządzenia, na którym został utworzony zestaw kopii zapasowych.
- **Wykonywanie kopii zapasowych zasad** lub **Głośność** — zasady kopii zapasowej lub woluminu skojarzonego z tego zestawu kopii zapasowych.
- **Od** i **do** — zakres dat i godzin, podczas tworzenia zestawu kopii zapasowych.

Filtrowane zestawy kopii zapasowych są następnie oznaczane znakami tabulacji zgodnie z następującymi atrybutami:

- **Nazwa** — Nazwa zasady kopii zapasowej lub woluminu skojarzonego z zestawu kopii zapasowych.
- **Rozmiar** — rzeczywisty rozmiar zestawu kopii zapasowych.
- **Utworzono w** — Data i godzina utworzenia kopii zapasowych. 
- **Typ** — zestawy kopii zapasowej można migawek lokalne lub migawki w chmurze. Lokalne migawki jest kopię zapasową wszystkich danych głośność przechowywanych lokalnie na urządzeniu, migawki chmury odnosi się do tworzenia kopii zapasowej woluminu danych znajdujących się w chmurze. Lokalne migawek udostępnić szybciej, migawki chmurze są wybrać dla ochrony danych.
- **Zainicjowane przez** — kopie zapasowe może zostać zainicjowana automatycznie, zgodnie z harmonogramem, lub ręcznie przez użytkownika. (Umożliwia zasady tworzenia kopii zapasowych zaplanować wykonywanie kopii zapasowych. Możesz też służy opcja **Sporządzanie kopii zapasowej** podjęcie interakcyjnych kopia zapasowa.)

## <a name="how-to-restore-your-storsimple-volume-from-a-backup"></a>Jak przywrócić głośność StorSimple z kopii zapasowej

Strony **Wykazu kopii zapasowych** umożliwia przywracanie głośność StorSimple z kopii zapasowej. Należy pamiętać, jednak, Przywracanie woluminu zostanie przywrócony wielkość do stanu sprzed po wykonaniu kopii zapasowej. Wszystkie dane, które zostały dodane po zakończeniu operacji wykonywania kopii zapasowej zostaną utracone.

> [AZURE.WARNING] Przywracanie z kopii zapasowej zastąpią istniejące wielkości z kopii zapasowej. Może to spowodować utratę wszystkie dane, które zostały zapisane po wykonaniu kopii zapasowej.

### <a name="to-restore-your-volume"></a>Aby przywrócić głośność

1. Na stronie usługi Menedżera StorSimple kliknij kartę **katalog kopii zapasowej** .

    ![Wykaz kopii zapasowych](./media/storsimple-restore-from-backup-set-u2/restore.png)

2. Zaznacz kopię zapasową, ustaw w następujący sposób:
  1. Wybierz odpowiednie urządzenie.
  2. Na liście rozwijanej wybierz zasady głośność lub kopii zapasowej kopii zapasowej, który chcesz zaznaczyć.
  3. Określ zakres czasu.
  4. Kliknij ikonę wyboru ![Zaznacz ikony](./media/storsimple-restore-from-backup-set-u2/HCS_CheckIcon.png) do wykonania tej kwerendy.
 
    Kopie zapasowe skojarzony z wybranego woluminu lub zasad kopii zapasowej powinien być wyświetlany na liście zestawów kopii zapasowych.

3. Rozwiń zestaw do wyświetlania wielkość skojarzone kopii zapasowych. Te ilości należy offline hosta i urządzenie przed można je przywrócić. Dostęp do wielkość na stronie **Kontenerów głośności** , a następnie postępuj zgodnie z instrukcjami [pobrać próbkę w trybie offline](storsimple-manage-volumes-u2.md#take-a-volume-offline) , aby je przełączyć do trybu offline.

    > [AZURE.IMPORTANT] Upewnij się, że wykonano wielkość w trybie offline na hoście najpierw zająć wielkość w trybie offline na tym urządzeniu. Jeśli nie trwa wielkość w trybie offline na hoście, potencjalnie może spowodować uszkodzenie danych.

4. Przejść z powrotem do karty **Wykazu kopii zapasowej** , a następnie wybierz zestaw kopii zapasowych.

5. Kliknij przycisk **Przywróć** w dolnej części strony.

6. Pojawi się monit o potwierdzenie. Przejrzyj informacje o przywracania, a następnie zaznacz pole wyboru potwierdzenia.

    ![Strona potwierdzenia](./media/storsimple-restore-from-backup-set-u2/ConfirmRestore.png)

7. Kliknij ikonę wyboru ![zaznacz ikony](./media/storsimple-restore-from-backup-set-u2/HCS_CheckIcon.png). Rozpoczęcie tworzenia zadania przywrócenia, które mogą przeglądać po zalogowaniu się do strony **zadania** . 

8. Po zakończeniu przywracania, można sprawdzić zawartość do wielkości zastępuje wielkości z kopii zapasowej.

![Klip wideo dostępne](./media/storsimple-restore-from-backup-set-u2/Video_icon.png) **wideo dostępne**

Aby obejrzeć klip wideo pokazano, jak używać klonowanie i przywracanie funkcje w StorSimple Aby odzyskać usunięte pliki, kliknij [tutaj](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

## <a name="if-the-restore-fails"></a>Jeśli przywracanie nie powiedzie się.

Jeśli przywracanie nie powiedzie się z dowolnego powodu, otrzymają alert. W takim przypadku Odśwież listę kopii zapasowej, aby sprawdzić, czy wykonywanie kopii zapasowej jest nadal ważna. Jeśli kopia zapasowa jest prawidłowy i przywracane z chmury, następnie problemy z łącznością może powodować problem. 

Aby wykonać operację przywracania, sporządzanie wielkość w trybie offline na hoście i spróbuj ponownie operacji przywracania. Należy zauważyć, że wszelkie zmiany w danych głośność, które były wykonywane podczas procesu przywracania zostaną utracone.

## <a name="next-steps"></a>Następne kroki

- Dowiedz się, jak [zarządzać StorSimple ilości](storsimple-manage-volumes-u2.md).

- Dowiedz się, jak [używać usługi Menedżera StorSimple administrowania urządzenia StorSimple](storsimple-manager-service-administration.md).
