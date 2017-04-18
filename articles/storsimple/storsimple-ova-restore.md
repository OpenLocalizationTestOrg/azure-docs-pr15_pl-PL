<properties
   pageTitle="Przywracanie z kopii zapasowej macierzy wirtualnych StorSimple"
   description="Dowiedz się więcej na temat przywracania kopii zapasowej macierzy Virtual StorSimple."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="06/07/2016"
   ms.author="alkohli"/>

# <a name="restore-from-a-backup-of-your-storsimple-virtual-array"></a>Przywracanie z kopii zapasowej macierzy wirtualnych StorSimple

## <a name="overview"></a>Omówienie 

Ten artykuł dotyczy do wersji ogólnodostępną (GA) 2016 marca uruchomionego programu Microsoft Azure StorSimple wirtualnych tablicy (nazywane także urządzenia wirtualnego StorSimple lokalnego lub urządzenia wirtualnego StorSimple) lub nowszym. W tym artykule opisano krok po kroku Przywracanie z kopii zapasowej zestawu akcji lub wielkości w macierzy Virtual StorSimple. Artykuł również szczegóły działaniem odzyskiwania poziomie elementu w macierzy Virtual StorSimple skonfigurowanego jako serwer plików.


## <a name="restore-shares-from-a-backup-set"></a>Przywracanie akcji z zestawu kopii zapasowych


**Zanim spróbujesz przywrócić akcje, upewnij się, że masz wystarczającą ilością miejsca na tym urządzeniu, aby ukończyć tej operacji.** Aby Przywracanie z kopii zapasowej w [portal Azure klasyczny](https://manage.windowsazure.com/), wykonaj następujące czynności.

#### <a name="to-restore-a-share"></a>Aby przywrócić udziału

1.  Przejdź do **katalogu kopii zapasowych**. Filtrowanie według odpowiedniego urządzenia i zakres czasu, aby wyszukać kopie zapasowe. Kliknij ikonę wyboru ![](./media/storsimple-ova-restore/image1.png) Aby wykonać kwerendę.


1.  Na liście zestawów kopii zapasowych wyświetlane kliknij i wybierz kopii zapasowej. Rozwijanie kopii zapasowej, aby wyświetlić różne akcje pod nim. Kliknij pozycję, a następnie wybierz pozycję Udostępnij, który chcesz przywrócić.

2.  U dołu strony kliknij przycisk **Przywróć jako nowy**.

3.  Rozpoczęcie tworzenia kreatora **przywracania jako nowy udział** . Na stronie **Określ nazwę i lokalizację** :


    1.  Sprawdź nazwę urządzenia źródła. Należy to urządzenie, które zawiera udostępnianie, który chcesz przywrócić. Wybór urządzenia jest wyszarzona. Aby wybrać urządzenie innego źródła, należy zakończyć działanie kreatora i ponownie zaznacz kopii zapasowej ponownie.

    2.  Podaj nazwę Udostępnij. Udostępnij nazwa musi zawierać znaki 3 do 127.

    3.  Przejrzyj rozmiar, typ i uprawnienia związane z udziału, do którego chcesz przywrócić. Będzie można modyfikować właściwości udziału za pomocą Eksploratora Windows, po zakończeniu przywracania.

    4.  Kliknij ikonę wyboru ![](./media/storsimple-ova-restore/image1.png).

        ![](./media/storsimple-ova-restore/image9.png)

1.  Po zakończeniu zadania przywracania przywracania zostanie uruchomiony i zostanie wyświetlone powiadomienie innym. Aby monitorować postęp przywracania, kliknij pozycję **zadania**. Spowoduje to przejście do strony **zadania** .

2.  Możesz śledzić postęp zadania przywracania. Gdy przywracania jest wykonane w 100%, przejść z powrotem do strony **akcji** na urządzeniu.

3.  Nowy udział przywrócenie można teraz wyświetlać na liście akcji na urządzeniu. Uwaga tej Przywróć odbywa się do tego samego typu Udostępnij. Udostępnianie warstwowych zostanie przywrócony jako tiered i lokalnie przypięty udostępnianie jako udział lokalnie przypięty.

Ukończono konfigurację urządzenia i masz materiału jak Kopia zapasowa i przywracanie udziału. 


## <a name="restore-volumes-from-a-backup-set"></a>Przywracanie wielkości z zestawu kopii zapasowych


Aby Przywracanie z kopii zapasowej w portalu klasyczny Azure, wykonaj następujące czynności. Przywracanie kopii zapasowej przywraca nowy na tym samym urządzeniu wirtualnych; Nie można przywrócić do innego urządzenia.

#### <a name="to-restore-a-volume"></a>Aby przywrócić woluminu

1.  Przejdź do **katalogu kopii zapasowych**. Filtrowanie według odpowiedniego urządzenia i zakres czasu, aby wyszukać kopie zapasowe. Kliknij ikonę wyboru ![](./media/storsimple-ova-restore/image1.png) Aby wykonać kwerendę.

2.  Na liście zestawów kopii zapasowych wyświetlane kliknij i wybierz kopii zapasowej. Rozwijanie kopii zapasowej, aby wyświetlić różne wielkości pod nim. Wybierz pozycję woluminu, który chcesz przywrócić. 

5.  U dołu strony kliknij przycisk **Przywróć jako nowy**. Zostanie uruchomiony Kreator **Przywracanie jako nowy** .

1.  Na stronie **Określ nazwę i lokalizację** :


    1.  Sprawdź nazwę urządzenia źródła. Należy to urządzenie, które zawiera woluminu, który chcesz przywrócić. Wybór urządzenia jest niedostępna. Aby wybrać urządzenie innego źródła, należy zakończyć działanie kreatora i ponownie zaznacz kopii zapasowej ponownie.

    2.  Podaj nazwę głośność wielkość Trwa przywracanie jako nowy. Nazwa woluminu musi zawierać znaki 3 do 127.

    3.  Kliknij ikonę strzałki.

        ![](./media/storsimple-ova-restore/image12.png)

1.  Na stronie **Określanie hosts, które mogą używać tego woluminu** wybierz odpowiednie ACRs z listy rozwijanej.

    ![](./media/storsimple-ova-restore/image13.png)

1.  Kliknij ikonę wyboru ![](./media/storsimple-ova-restore/image1.png). Rozpocznie to zadanie przywracania i zostanie wyświetlone następujące powiadomienie, że zadanie jest w toku.

2.  Po zakończeniu zadania przywracania przywracania zostanie uruchomiony i zostanie wyświetlone powiadomienie innym. Aby monitorować postęp przywracania, kliknij pozycję **zadania**. Spowoduje to przejście do strony **zadania** .

3.  Możesz śledzić postęp zadania przywracania. Przejdź do strony **wielkości** na urządzeniu.

4.  Nowy przywrócenie można teraz wyświetlać na liście wielkości na urządzeniu. Uwaga tej Przywróć odbywa się do tego samego typu, wielkości. Warstwowych głośność zostanie przywrócony jako tiered i lokalnie przypięty głośność zostanie przywrócony jako lokalnie przypięty głośność.

5.  Gdy wielkość pojawi się na liście wielkości online, głośność jest dostępna do użycia.  Na hoście inicjator iSCSI odświeżyć listę elementów docelowych w oknie właściwości inicjator iSCSI.  Nowy obiekt docelowy, zawierające nazwę przywróceniu woluminu powinien być wyświetlany jako "nieaktywny" w kolumnie Stan.

6.  Wybierz element docelowy, a następnie kliknij przycisk **Połącz**.   Po inicjator jest podłączone do elementu docelowego, zmienić stan na **połączony**. 

7.  W oknie **Zarządzanie dysku** wielkość zainstalowanego pojawi, jak pokazano na poniższej ilustracji. Kliknij prawym przyciskiem myszy wielkość odkryte (kliknij nazwę dysku), a następnie kliknij pozycję **Online**.

> [AZURE.IMPORTANT] Podczas próby Przywracanie z kopii zapasowej woluminu lub w udziale ustawić, jeśli zadanie przywracania kończy się niepowodzeniem, głośność docelowej lub nadal można utworzyć udziału w portalu. Należy pamiętać, że usunąć tego woluminu docelowej lub udostępnianie w portalu w celu zminimalizowania problemów w przyszłości wynikających z tego elementu.

## <a name="item-level-recovery-ilr"></a>Odzyskiwanie poziomie elementu (ILR)

Tej wersji wprowadzono odzyskiwania poziomie elementu (ILR) na urządzeniu virtual StorSimple skonfigurowanego jako serwer plików. Ta funkcja umożliwia szczegółowego odzyskiwanie plików i folderów z kopii zapasowej chmury wszystkich akcji na tym urządzeniu StorSimple. Użytkownicy mogą pobrać usunięte pliki z ostatnie kopie zapasowe przy użyciu samoobsługowego modelu.

Każdy udział ma folder *.backups* , który zawiera najbardziej aktualne kopie zapasowe. Użytkownik może przejść do odpowiedniej kopii zapasowej, skopiuj odpowiednie pliki i foldery z kopii zapasowej i ich przywracania. Dzięki temu połączeń administratorom przywracania pliki z kopii zapasowych.

1.  Podczas wykonywania ILR, możesz wyświetlić kopie zapasowe za pomocą Eksploratora Windows. Kliknij przycisk Udostępnij określonych, który chcesz przejrzeć kopii zapasowej dla. Zostanie wyświetlony folder *.backups* utworzone w obszarze Udostępnianie, w którym są przechowywane wszystkie kopie zapasowe. Rozwiń folder *.backups* , aby wyświetlić kopie zapasowe. Folder spowoduje wyświetlenie widok całą hierarchię kopii zapasowej. Ten widok jest tworzony na żądanie i zwykle trwa tylko kilka sekund, aby utworzyć.

    5 ostatnich kopie zapasowe są wyświetlane w ten sposób i może służyć do odzyskiwania poziomie elementu. 5 ostatnie kopie zapasowe obejmują zarówno domyślne według harmonogramu, jak i ręcznego tworzenia kopii zapasowych.

    
    -   **Zaplanowane kopie zapasowe** określona jako &lt;nazwę urządzenia&gt;DailySchedule-RRRRMMDD-HHMMSS-UTC.

    -   Określona jako Ad-hoc — RRRRMMDD-HHMMSS-UTC **ręcznego tworzenia kopii zapasowych** .
    
        ![](./media/storsimple-ova-restore/image14.png)

1.  Identyfikowanie zawierające najbardziej aktualną wersję usunięty plik kopii zapasowej. Jeśli nazwa folderu zawiera sygnatury czasowej UTC w każdym z powyższych przypadków, czas, w którym został utworzony folder po raz obecnego urządzenia uruchomienia kopii zapasowej. Aby zlokalizować i zidentyfikować kopii zapasowych za pomocą folderu sygnatura czasowa.

2.  Zlokalizuj folder lub plik, który chcesz przywrócić w kopii zapasowej zidentyfikowanego w poprzednim kroku. Należy zauważyć, że można przeglądać tylko pliki lub foldery, których masz uprawnienia. Jeśli nie jesteś mieli dostęp do niektórych plików lub folderów, należy skontaktować się z administratorem udostępnianie, który można używać Eksploratora Windows, aby edytować uprawnienia udostępniania i umożliwiają dostęp do określonego pliku lub folderu. Jest zalecane najlepiej administrator Udostępnij konieczności grupy użytkowników, a nie jednego użytkownika.

3.  Kopiowanie pliku lub folderu do odpowiedniego udziału na serwerze plików StorSimple.

![video_icon](./media/storsimple-ova-restore/video_icon.png) **wideo dostępne**

Obejrzyj klip wideo, aby zobaczyć, jak można utworzyć akcje, wykonywanie kopii zapasowej akcje i przywracania danych w macierzy Virtual StorSimple.

> [AZURE.VIDEO use-the-storsimple-virtual-array]

## <a name="next-steps"></a>Następne kroki

Informacje dotyczące sposobu [administrowania macierzy Virtual StorSimple korzystanie z lokalnego interfejsu użytkownika w sieci web](storsimple-ova-web-ui-admin.md).
