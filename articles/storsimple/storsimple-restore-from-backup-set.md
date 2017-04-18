<properties 
   pageTitle="Przywracanie z kopii zapasowej woluminu StorSimple | Microsoft Azure"
   description="Wyjaśniono, jak za pomocą strony wykazu kopii zapasowych usługi Menedżera StorSimple przywrócenia woluminu StorSimple z zestawu kopii zapasowych."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="restore-a-storsimple-volume-from-a-backup-set"></a>Przywracanie woluminu StorSimple z zestawu kopii zapasowych

[AZURE.INCLUDE [storsimple-version-selector-restore-from-backup](../../includes/storsimple-version-selector-restore-from-backup.md)]

## <a name="overview"></a>Omówienie

Strona **Wykazu kopii zapasowych** zawiera wszystkie zestawy kopii zapasowych, które zostały utworzone podczas ręcznego lub automatycznego wykonywania kopii zapasowych. Ta strona umożliwia listę wszystkich kopii zapasowych zasady tworzenia kopii zapasowych lub obrót, zaznacz lub usuń kopie zapasowe lub użyć kopii zapasowej do przywrócenia lub klonowanie woluminu.

 ![Aby utworzyć kopię zapasową strony wykazu](./media/storsimple-restore-from-backup-set/HCS_BackupCatalog.png)

Ten samouczek wyjaśniono, jak używać strony **Wykazu kopii zapasowej** do przywrócenia woluminu na urządzeniu z zestawu kopii zapasowych.

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

Strony **Wykazu kopii zapasowych** umożliwia przywracanie głośność StorSimple z kopii zapasowej. 

> [AZURE.WARNING] Przywracanie z kopii zapasowej zastąpią istniejące wielkości z kopii zapasowej. Może to spowodować utratę wszystkie dane, które zostały zapisane po wykonaniu kopii zapasowej.

Przed rozpoczęciem przywracania woluminu upewnij się, że głośność jest w trybie offline. Musisz wykonać wielkość w trybie offline na hoście pierwszego, a następnie urządzenie. Postępuj zgodnie z instrukcjami [pobrać próbkę w trybie offline](storsimple-manage-volumes.md#take-a-volume-offline). Wykonaj poniższe czynności, aby przywrócenia woluminu z zestawu kopii zapasowych.

### <a name="to-restore-from-a-backup-set"></a>Aby przywrócić z zestawu kopii zapasowych

1. Na stronie usługi Menedżera StorSimple kliknij kartę **katalog kopii zapasowej** .

    ![Wykaz kopii zapasowych](./media/storsimple-restore-from-backup-set/HCS_Restore.png)

2. Zaznacz kopię zapasową, ustaw w następujący sposób:
  1. Wybierz odpowiednie urządzenie.
  2. Na liście rozwijanej wybierz zasady głośność lub kopii zapasowej kopii zapasowej, który chcesz zaznaczyć.
  3. Określ zakres czasu.
  4. Kliknij ikonę wyboru ![Zaznacz ikony](./media/storsimple-restore-from-backup-set/HCS_CheckIcon.png) do wykonania tej kwerendy.
 
    Kopie zapasowe skojarzony z wybranego woluminu lub zasad kopii zapasowej powinien być wyświetlany na liście zestawów kopii zapasowych.

3. Rozwiń zestaw do wyświetlania wielkość skojarzone kopii zapasowych. Te ilości należy offline hosta i urządzenie przed można je przywrócić. Postępuj zgodnie z instrukcjami [pobrać próbkę w trybie offline](storsimple-manage-volumes.md#take-a-volume-offline).

    >  [AZURE.IMPORTANT] Upewnij się, że wykonano wielkość w trybie offline na hoście najpierw zająć wielkość w trybie offline na tym urządzeniu. Jeśli nie trwa wielkość w trybie offline na hoście, potencjalnie może spowodować uszkodzenie danych.

4. Wybierz zestaw kopii zapasowych. Kliknij przycisk **Przywróć** w dolnej części strony.

6. Pojawi się monit o potwierdzenie. 

    ![Strona potwierdzenia](./media/storsimple-restore-from-backup-set/HCS_ConfirmRestore.png)

7. Przejrzyj informacje o przywracania i kliknij ikonę wyboru ![zaznacz ikony](./media/storsimple-restore-from-backup-set/HCS_CheckIcon.png). Rozpoczęcie tworzenia zadania przywrócenia, które mogą przeglądać po zalogowaniu się do strony **zadania** . 

8. Po zakończeniu przywracania, można sprawdzić zawartość do wielkości zastępuje wielkości z kopii zapasowej.

![Klip wideo dostępne](./media/storsimple-restore-from-backup-set/Video_icon.png) **wideo dostępne**

Aby obejrzeć klip wideo pokazano, jak używać klonowanie i przywracanie funkcje w StorSimple Aby odzyskać usunięte pliki, kliknij [tutaj](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

## <a name="next-steps"></a>Następne kroki

- Dowiedz się, jak [zarządzać StorSimple ilości](storsimple-manage-volumes.md).

- Dowiedz się, jak [używać usługi Menedżera StorSimple administrowania urządzenia StorSimple](storsimple-manager-service-administration.md).
