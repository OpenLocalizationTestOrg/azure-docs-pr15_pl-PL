<properties 
   pageTitle="Samouczek kopii zapasowej tablicy Virtual StorSimple | Microsoft Azure"
   description="Opisano, jak utworzyć kopię zapasową udział StorSimple wirtualnych tablicy i ilości."
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
   ms.date="06/07/2016"
   ms.author="alkohli" />

# <a name="back-up-your-storsimple-virtual-array"></a>Wykonywanie kopii zapasowej macierzy wirtualnych StorSimple

## <a name="overview"></a>Omówienie 

Ten samouczek dotyczy wersji ogólnodostępną (GA) 2016 marca uruchomionego Microsoft Azure StorSimple wirtualnych tablicy (nazywane także urządzenia wirtualnego StorSimple lokalnego lub urządzenia wirtualnego StorSimple) lub nowszym.

Tablica Virtual StorSimple jest hybrydowych chmury lokalnej wirtualną urządzeniu magazynującym skonfigurowanego jako serwera plików lub serwerem. Je tworzyć kopie zapasowe, przywracanie z kopii zapasowych i wykonywanie pracy awaryjnej urządzenia, jeśli odzyskiwanie jest potrzebny. Gdy skonfigurowane jako serwer plików, pozwala odzyskiwania na poziomie elementu. Ten samouczek opisano, jak tworzyć kopie zapasowe zaplanowane i ręczne macierzy Virtual StorSimple za pomocą portalu klasyczny Azure lub w sieci web StorSimple interfejsu użytkownika.


## <a name="back-up-shares-and-volumes"></a>Wykonywanie kopii zapasowej udział i ilości

Kopie zapasowe zapewniają ochronę w chwili, poprawić odzyskiwanie danych, a następnie zminimalizować czasy Przywróć akcji i ilości. Może wykonywać kopie zapasowe Udostępnij lub ilością na urządzeniu StorSimple na dwa sposoby: **Zaplanowane** lub **ręcznie**. W poniższych sekcjach omówiono każdej z metod.

> [AZURE.NOTE] W tej wersji zaplanowanych kopii zapasowych są tworzone przez domyślną zasadę, która działa codziennie w określonym czasie i kopię zapasową wszystkich udziałów lub wielkości na urządzeniu. Nie jest możliwe tworzenie zasad niestandardowych dla zaplanowanych kopii zapasowych w tej chwili.

## <a name="change-the-backup-schedule"></a>Zmienianie harmonogramu wykonywania kopii zapasowych

Urządzenia wirtualnego StorSimple zawiera domyślną zasadę kopii zapasowej, rozpoczyna się w określonym czasie dnia (22:30), który tworzy kopię zapasową wszystkich udziałów lub wielkości na tym urządzeniu raz dziennie. Możesz zmienić godzinę, w którym nie można zmienić uruchamianiu kopii zapasowej, ale częstotliwość i przechowywaniem dokumentów, (który określa liczbę kopii zapasowych do zachowania). Podczas te kopie zapasowe całego urządzenia wirtualnego kopii zapasowej; Dlatego zalecamy planowanie te kopie zapasowe dla mniejszego.

W [portalu klasyczny Azure](https://manage.windowsazure.com/) Zmienianie domyślnej godziny rozpoczęcia kopii zapasowej, należy wykonać następujące czynności.

#### <a name="to-change-the-start-time-for-the-default-backup-policy"></a>Aby zmienić czas rozpoczęcia domyślnych zasad kopii zapasowej

1. Przejdź na kartę **Konfiguracja** urządzenia.

2. W sekcji **kopii zapasowej** określ godzinę rozpoczęcia codziennego wykonywania kopii zapasowej.

3. Kliknij przycisk **Zapisz**.

### <a name="take-a-manual-backup"></a>Wykonać ręczną kopię zapasową

Oprócz zaplanowanych kopii zapasowych możesz wykonać ręczne (na żądanie) kopii zapasowej w dowolnym momencie.

#### <a name="to-create-a-manual-on-demand-backup"></a>Aby utworzyć kopię zapasową ręczne (na żądanie)

1. Przejdź na kartę **Akcje** lub **wielkości** .

2. U dołu strony kliknij pozycję **Kopia zapasowa wszystkich**. Wyświetli monit o Sprawdź, czy chcesz teraz wykonać kopię zapasową. Kliknij ikonę wyboru ![zaznacz ikony](./media/storsimple-ova-backup/image3.png) kontynuowanie tworzenia kopii zapasowej.

    ![Potwierdzenie kopii zapasowej](./media/storsimple-ova-backup/image4.png)

    Użytkownik będzie powiadamiany, że zadanie kopii zapasowej zostało uruchomione.

    ![uruchamianie kopii zapasowej](./media/storsimple-ova-backup/image5.png)

    Użytkownik będzie powiadamiany, że zadanie zostało utworzone pomyślnie.

    ![Utworzono zadanie kopii zapasowej](./media/storsimple-ova-backup/image7.png)

3. Aby śledzić postęp zadania, kliknij pozycję **Zadania**.

4. Po zakończeniu zadania kopii zapasowej, przejdź na kartę **Wykaz narzędzia Kopia zapasowa** . Powinien zostać wyświetlony Zakończono tworzenie kopii zapasowej.

    ![Zakończono tworzenie kopii zapasowej](./media/storsimple-ova-backup/image8.png)

5. Ustawianie wybory filtrów do odpowiedniego urządzenia, politykę wykonywania kopii zapasowych i zakres czasu, a następnie kliknij ikonę wyboru ![Zaznacz ikony](./media/storsimple-ova-backup/image3.png).

    Wykonywanie kopii zapasowej powinien być wyświetlany na liście zestawów kopii zapasowych jest wyświetlany w wykazie.

## <a name="view-existing-backups"></a>Wyświetlanie istniejących kopii zapasowych

W portalu klasyczny Azure, aby wyświetlić istniejące kopie zapasowe, należy wykonać następujące czynności.

#### <a name="to-view-existing-backups"></a>Aby wyświetlić istniejące kopie zapasowe

1. Na stronie usługi Menedżera StorSimple kliknij kartę **katalog kopii zapasowej** .

2. Zaznacz kopię zapasową, ustaw w następujący sposób:

    1. Wybierz urządzenie.

    2. Na liście rozwijanej wybierz pozycję Udostępnij lub ilością kopii zapasowej, który chcesz zaznaczyć.

    3. Określ zakres czasu.

    4. Kliknij ikonę wyboru ![](./media/storsimple-ova-backup/image3.png) do wykonania tej kwerendy.

    Kopie zapasowe skojarzony z zaznaczonego udziału lub głośność powinien być wyświetlany na liście zestawów kopii zapasowych.

![video_icon](./media/storsimple-ova-backup/video_icon.png) **wideo dostępne**

Obejrzyj klip wideo, aby zobaczyć, jak można utworzyć akcje, wykonywanie kopii zapasowej akcje i przywracania danych w macierzy Virtual StorSimple.

> [AZURE.VIDEO use-the-storsimple-virtual-array]

## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej o [administrowaniu macierzy Virtual StorSimple](storsimple-ova-web-ui-admin.md).
