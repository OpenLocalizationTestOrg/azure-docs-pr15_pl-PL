<properties 
   pageTitle="Zarządzanie wykaz kopii zapasowych StorSimple | Microsoft Azure"
   description="Wyjaśniono, jak za pomocą strony wykazu kopii zapasowych usługi StorSimple menedżera listy, zaznacz i Usuń zestawy kopii zapasowych dla woluminu."
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
   ms.date="04/28/2016"
   ms.author="v-sharos" />

# <a name="use-the-storsimple-manager-service-to-manage-your-backup-catalog"></a>Zarządzanie wykaz kopii zapasowych przy użyciu usługi Menedżera StorSimple

## <a name="overview"></a>Omówienie

Strona **Wykazu kopii zapasowych** usługi Menedżera StorSimple zawiera wszystkie zestawy kopii zapasowych utworzone podczas ręcznego lub planowanego kopie zapasowe. Ta strona umożliwia listę wszystkich kopii zapasowych zasady tworzenia kopii zapasowych lub obrót, zaznacz lub usuń kopie zapasowe lub użyć kopii zapasowej do przywrócenia lub klonowanie woluminu.

Ten samouczek wyjaśniono, jak na liście, wybierz pozycję i usuwanie zestawu kopii zapasowych. Aby dowiedzieć się, jak przywrócić urządzenia z kopii zapasowej, przejdź do [przywracania urządzenia z zestawu kopii zapasowych](storsimple-restore-from-backup-set.md). Aby dowiedzieć się, jak klonowanie woluminu, przejdź do [klonowanie głośność StorSimple](storsimple-clone-volume.md).

![Wykaz kopii zapasowych](./media/storsimple-manage-backup-catalog/backupcatalog.png) 

Na stronie **Wykaz kopii zapasowych** jest zestawu kwerendę, aby zawęzić kopii zapasowej zaznaczenia. Można filtrować zestawy kopii zapasowych, które są pobierane na podstawie następujących parametrów:

- **Urządzenia** — urządzenia, na którym został utworzony zestaw kopii zapasowych.

- **Zasady kopii zapasowej lub ilością** — zasady kopii zapasowej lub woluminu skojarzonego z tego zestawu kopii zapasowych.

- **Od i do** — zakres dat i godzin, podczas tworzenia zestawu kopii zapasowych.

Filtrowane zestawy kopii zapasowych są następnie oznaczane znakami tabulacji zgodnie z następującymi atrybutami:

- **Nazwa** — Nazwa zasady kopii zapasowej lub woluminu skojarzonego z zestawu kopii zapasowych.

- **Rozmiar** — rzeczywisty rozmiar zestawu kopii zapasowych.

- **Utworzone na** — Data i godzina utworzenia kopii zapasowych. 

- **Typ** — zestawy kopii zapasowej można migawek lokalne lub migawki w chmurze. Lokalne migawki jest kopię zapasową wszystkich danych głośność przechowywanych lokalnie na urządzeniu, migawki chmury odnosi się do tworzenia kopii zapasowej woluminu danych znajdujących się w chmurze. Lokalne migawek udostępnić szybciej, migawki chmurze są wybrać dla ochrony danych.

- **Zainicjowane przez** — kopie zapasowe może zostać zainicjowana automatycznie według harmonogramu lub ręcznie przez użytkownika. Zasady tworzenia kopii zapasowych umożliwia planowanie kopii zapasowych. Za pomocą opcji **Sporządzanie kopii zapasowej** można również wykonać ręczną kopię zapasową.

## <a name="list-backup-sets-for-a-volume"></a>Zestawy kopii zapasowych listy dla woluminu
 
Wykonaj poniższe czynności, aby wyświetlić listę wszystkich kopii zapasowych dla woluminu.

#### <a name="to-list-backup-sets"></a>Aby liście zestawy kopii zapasowych

1. Na stronie usługi Menedżera StorSimple kliknij kartę **katalog kopii zapasowej** .

2. Filtrowanie pozycje w następujący sposób:

    1. Wybierz odpowiednie urządzenie.

    2. Na liście rozwijanej wybierz głośność, aby wyświetlić odpowiednie kopie zapasowe.

    3. Określ zakres czasu.

    4. Kliknij ikonę wyboru ![Ikona modułu sprawdzania](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) do wykonania tej kwerendy.
 
    Kopie zapasowe skojarzone z wybranego woluminu powinien być wyświetlany na liście zestawów kopii zapasowych.

## <a name="select-a-backup-set"></a>Wybierz zestaw kopii zapasowych

Wykonaj następujące czynności, aby zaznaczyć zestawu głośność lub zasad kopii zapasowej kopii zapasowych.

#### <a name="to-select-a-backup-set"></a>Aby zaznaczyć zestaw kopii zapasowych

1. Na stronie usługi Menedżera StorSimple kliknij kartę **katalog kopii zapasowej** .

2. Filtrowanie pozycje w następujący sposób:

    1. Wybierz odpowiednie urządzenie.

    2. Na liście rozwijanej wybierz zasady głośność lub kopii zapasowej kopii zapasowej, który chcesz zaznaczyć.

    3. Określ zakres czasu.

    4. Kliknij ikonę wyboru ![Ikona modułu sprawdzania](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) do wykonania tej kwerendy.

    Kopie zapasowe skojarzony z wybranego woluminu lub zasad kopii zapasowej powinien być wyświetlany na liście zestawów kopii zapasowych.

3. Wybierz i rozwiń zestaw kopii zapasowych. Opcje **Przywracanie** i **Usuwanie** są wyświetlane u dołu strony. Można wykonać jedną z następujących czynności na wybrany zestaw kopii zapasowych.

## <a name="delete-a-backup-set"></a>Usuwanie zestawu kopii zapasowych

Usuń kopię zapasową, gdy już nie chcesz zachować dane skojarzone z nim. Wykonaj poniższe czynności, aby usunąć zestaw kopii zapasowych.

#### <a name="to-delete-a-backup-set"></a>Aby usunąć zestaw kopii zapasowych

1. Na stronie usługi Menedżera StorSimple kliknij **kartę katalog kopii zapasowej**.

2. Filtrowanie pozycje w następujący sposób:

    1. Wybierz odpowiednie urządzenie.

    2. Na liście rozwijanej wybierz zasady głośność lub kopii zapasowej kopii zapasowej, który chcesz zaznaczyć.

    3. Określ zakres czasu.

    4. Kliknij ikonę wyboru ![Ikona modułu sprawdzania](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) do wykonania tej kwerendy.

    Kopie zapasowe skojarzony z wybranego woluminu lub zasad kopii zapasowej powinien być wyświetlany na liście zestawów kopii zapasowych.

3. Wybierz i rozwiń zestaw kopii zapasowych. Opcje **Przywracanie** i **Usuwanie** są wyświetlane u dołu strony. Kliknij przycisk **Usuń**.

4. Gdy usunięcie jest w toku i po pomyślnym zakończeniu, otrzymasz powiadomienie. Po zakończeniu usuwania odświeżenie zapytania na tej stronie. Usunięte zestaw kopii zapasowych pojawi się już na liście zestawów kopii zapasowych.

## <a name="next-steps"></a>Następne kroki

- Dowiedz się, jak za [pomocą wykazu kopii zapasowej do przywrócenia urządzenia z zestawu kopii zapasowych](storsimple-restore-from-backup-set.md).

- Dowiedz się, jak [używać usługi Menedżera StorSimple administrowania urządzenia StorSimple](storsimple-manager-service-administration.md).
