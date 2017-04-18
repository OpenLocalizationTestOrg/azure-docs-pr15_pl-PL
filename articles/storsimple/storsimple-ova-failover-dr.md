<properties
   pageTitle="Po awarii odzyskiwania i urządzenia awaryjnego macierzy wirtualnych StorSimple"
   description="Dowiedz się więcej o pracy awaryjnej macierzy Virtual StorSimple."
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

# <a name="disaster-recovery-and-device-failover-for-your-storsimple-virtual-array"></a>Po awarii odzyskiwania i urządzenia awaryjnego macierzy wirtualnych StorSimple


## <a name="overview"></a>Omówienie

Ten artykuł zawiera opis awarii usługi Microsoft Azure StorSimple wirtualnych tablicy (nazywane także StorSimple lokalnej wirtualną urządzenie) w tym szczegółowe kroki wymagane przechodzić do innego urządzenia wirtualnego wypadku awarii. Trybie awaryjnym umożliwia migrację danych z urządzenia *źródła* w obrębie centrum danych na *inne urządzenie znajduje się w tej samej lub innej lokalizacji geograficznej* . Tym przełączeniu urządzenie jest przeznaczony dla całego urządzenia. Podczas awaryjnym przeniesieniu własności zmiany danych chmury urządzenia źródła do tego urządzenia.

Urządzenia awaryjnego jest orchestrated za pomocą funkcji odzyskiwania (DR) danych i została zainicjowana na stronie **urządzenia** . Ta strona rejestruje wszystkich urządzeń StorSimple połączony z usługą Menedżera StorSimple. Dla każdego urządzenia są wyświetlane przyjazną nazwę, stan, ustanawianie i maksymalnej pojemności, typu i modelu.

![](./media/storsimple-ova-failover-dr/image15.png)

Ten artykuł dotyczy tablic wirtualnych StorSimple tylko. Aby nie na urządzeniu z systemem 8000 serii, przejdź do [awaryjnego i odzyskiwanie urządzenia StorSimple](storsimple-device-failover-disaster-recovery.md).


## <a name="what-is-disaster-recovery"></a>Co to jest Odzyskiwanie?

W scenariuszu odzyskiwania (DR) awarii urządzenie podstawowe przestaje działać. W tej sytuacji można przenieść dane chmury skojarzone z urządzeniem nie powiodło się do innego urządzenia przy użyciu urządzenia podstawowego jako *źródła* i określając inne urządzenie jako *docelowej*. Ten proces jest nazywany tym *pracy awaryjnej*. Podczas awaryjnym przeniesieniu wszystkich wielkość lub akcje z urządzenia źródła zmienić własność i są przenoszone do urządzenia. Bez filtrowania danych jest dozwolone.

DR modelu jako przywracania pełny urządzenia za pomocą ciepła opartych na mapie obsługi i śledzenie. Na konturowy jest definiowana przypisując wartość ciepła do danych oparty na odczytywanie i zapisywanie wzorców. Ten ciepła zamapuj następnie poziomów najniższe fragmentów danych ciepła w chmurze najpierw przy zachowaniu fragmentów danych wysoka ciepła (najczęściej używane) w warstwie lokalny. Podczas DR konturowy służy do przywrócenia i rehydrate danych z chmury. Urządzenie pobiera wszystkie ilości/akcje w ostatnim ostatniej kopii zapasowej (określone wewnętrznie) i wykonuje przywracania z tej kopii zapasowej. Cały proces DR jest orchestrated przez urządzenie.


## <a name="prerequisites-for-device-failover"></a>Wymagania wstępne do przełączania awaryjnego urządzenia


### <a name="prerequisites"></a>Wymagania wstępne

Do dowolnego urządzenia przełączania awaryjnego być spełnione następujące wymagania:

- Urządzenia musi być w stanie **zdezaktywowane** .

- Urządzenie musi się pojawić jako **aktywna** w portalu klasyczny Azure. Należy zapewnić urządzenia wirtualnego pojemności samego lub wyższego. Następnie należy używać lokalnego interfejs użytkownika sieci web do konfigurowania i pomyślnie zarejestrować urządzenie wirtualne.

    > [AZURE.IMPORTANT] Nie spróbować skonfigurować zarejestrowanych urządzenia wirtualnego za pośrednictwem usługi, klikając pozycję **Instalator wykonane urządzenia**. Konfiguracja urządzenia nie powinny być wykonywane za pośrednictwem usługi.

- Urządzenie źródłowa i docelowa muszą być tego samego typu. Tylko może zakończyć niepowodzeniem na urządzenie wirtualne skonfigurowany jako serwer plik na inny serwer plików. Dotyczy to także serwerem.

- Na serwerze plików DR zalecamy Połącz urządzenie do tej samej domeny co źródła, tak aby uprawnienia udostępniania są automatycznie. Tylko przełączanie awaryjne do urządzenia w tej samej domeny jest obsługiwana w tej wersji.

### <a name="other-considerations"></a>Inne zagadnienia

- Firma Microsoft zaleca sporządzanie wielkości lub akcji na urządzenia w trybie offline.

- Jeśli jest planowane trybie awaryjnym, zalecamy wykonaj kopię zapasową urządzenia, a następnie kontynuować tym przełączeniu zminimalizować utratę danych. Jeśli jest to nieplanowanego przełączania awaryjnego, najnowszą kopię zapasową będą używane do przywrócić urządzenia.

- Urządzenia dostępnych dla DR to urządzenia, które ustanowić samego lub większy w porównaniu z urządzenia. Urządzenia, które są połączone z usługą, ale nie spełnia kryteria wystarczającą ilością miejsca na nie będą dostępne jako urządzenia.

### <a name="dr-prechecks"></a>DR prechecks

Przed rozpoczęciem DR prechecks są wykonywane na tym urządzeniu. Kontrole pomocy, upewnij się, że nie wystąpią błędy podczas DR rozpoczyna się. Prechecks obejmują:

- Sprawdzanie poprawności konta miejsca do magazynowania

- Sprawdzanie łączności chmura azure

- Sprawdzanie wolnego miejsca na urządzenie

- Sprawdzania, czy urządzenia źródłowego serwera iSCSI ma prawidłowych nazw awaryjnego, IQN (nie przekracza 220 znaków) i hasło CHAP (12 do 16 znaków) skojarzone z wielkość

Jeśli dowolny z powyższych prechecks nie, nie można kontynuować DR. Trzeba rozwiązują te problemy, a następnie ponów próbę DR.

Po pomyślnym zakończeniu DR własność dane chmur na tym urządzeniu źródła są przenoszone do urządzenia. Urządzenia następnie nie jest już dostępna w portalu. Dostęp do wszystkich ilości akcji na tym urządzeniu źródła jest zablokowany, a aktywnym urządzenia.

> [AZURE.IMPORTANT]
> 
> Jeśli urządzenie nie jest już dostępna, maszyny wirtualnej, który zainicjowany w systemie hosta jest nadal przez inne zasoby. Po pomyślnym zakończeniu DR, możesz usunąć ten maszyn wirtualnych systemu hosta.

## <a name="fail-over-to-a-virtual-array"></a>Nie na tablicę wirtualny

Zaleca się, że masz innej tablicy Virtual StorSimple obsługi administracyjnej, konfigurować za pośrednictwem lokalnych sieci web interfejsu użytkownika i zarejestrowana w usłudze Menedżer StorSimple przed wykonaniem tej procedury.


> [AZURE.IMPORTANT]
> 
> - Nie są dozwolone spowodować awarię z urządzenia serii StorSimple 8000 urządzenie wirtualne 1200.
> - Po wymuszeniu przejęcia z urządzenia wirtualnego przetwarzania standardowy FIPS (Federal Information) włączone wdrożony w portalu dla instytucji rządowych urządzenie wirtualne w portalu klasyczny Azure. To także odwrotnej.

Wykonaj poniższe czynności, aby przywrócić urządzenia urządzenie wirtualne StorSimple docelowej.

1. Sporządzanie wielkości/akcje w trybie offline na hoście. Zapoznaj się z instrukcjami specyficzne dla systemu operacyjnego na hoście wielkości akcje przełączyć do trybu offline. Jeśli nie już offline, należy wykonać wszystkich wielkości akcji w trybie offline na tym urządzeniu, przechodząc do **urządzeń > Akcje** (lub **urządzenia > wielkości**). Wybierz pozycję Udostępnij/głośności, a następnie kliknij przycisk **tryb offline** w dolnej części strony. Po wyświetleniu monitu o potwierdzenie, kliknij przycisk **Tak**. Powtórz tę procedurę dla wszystkich akcji i wielkość na urządzeniu.

2. Na stronie **urządzeń** wybierz urządzenie źródła do przełączania awaryjnego, a następnie kliknij przycisk **Dezaktywuj**. 
    ![](./media/storsimple-ova-failover-dr/image16.png)

3. Pojawi się monit o potwierdzenie. Dezaktywacja urządzenie jest trwały proces, którego nie można cofnąć. Możesz również przypomnienie pojawi się podjęcie do akcji i ilości w trybie offline na hoście.

    ![](./media/storsimple-ova-failover-dr/image18.png)

3. Po potwierdzeniu Rozpocznie się dezaktywowania. Po pomyślnym zakończeniu dezaktywowania, otrzymasz powiadomienie.

    ![](./media/storsimple-ova-failover-dr/image19.png)

4. Na stronie **urządzeń** stan urządzenia zmieni się teraz na **zdezaktywowane**.

    ![](./media/storsimple-ova-failover-dr/image20.png)

5. Wybierz urządzenie, został dezaktywowany i u dołu strony kliknij **pracy awaryjnej**.

6. W Kreatorze pracy awaryjnej Potwierdź otwarty wykonaj następujące czynności:

    1. Z listy rozwijanej dostępnych urządzeń, wybierz pozycję **urządzeniem.** Tylko masz wystarczającej urządzenia są wyświetlane na liście rozwijanej.

    2. Przejrzyj szczegóły skojarzone z urządzeniem źródła, takie jak nazwa urządzenia, pojemność i nazwy akcji, które nie powiedzie się nad.

        ![](./media/storsimple-ova-failover-dr/image21.png)

7. Sprawdź **I zgadza się, że pracy awaryjnej jest operacji trwały i po tym przełączeniu zostanie ukończony pomyślnie, urządzenia zostaną usunięte**.

8. Kliknij ikonę wyboru ![](./media/storsimple-ova-failover-dr/image1.png).


9. Zadanie pracy awaryjnej będą inicjowane i otrzymasz powiadomienie. Kliknij przycisk **zadania** monitorowanie tym przełączeniu.

    ![](./media/storsimple-ova-failover-dr/image22.png)

10. Na stronie **zadania** zostanie wyświetlony zadanie pracy awaryjnej utworzoną dla urządzenia. To zadanie wykonuje DR prechecks.

    ![](./media/storsimple-ova-failover-dr/image23.png)

    Po pomyślnym DR prechecks stanowiska pracy awaryjnej będzie zduplikować zadań przywracania dla każdego Udostępnij/woluminu, znajdującego się na urządzeniu źródła.

    ![](./media/storsimple-ova-failover-dr/image24.png)

11. Po zakończeniu tym przełączeniu przejdź na stronę **urządzenia** .

    . Wybierz urządzenia wirtualnego StorSimple użytego jako urządzenia dla awaryjnego procesu.

    b. Przejdź do strony **akcji** (lub **wielkości** Jeśli serwerem). Teraz powinny być wymienione wszystkich akcji (ilości) ze starej urządzenia.
    
    ![](./media/storsimple-ova-failover-dr/image25.png)

![](./media/storsimple-ova-failover-dr/video_icon.png)**Klip wideo jest niedostępna**

W tym klipie wideo pokazano, jak użytkownik może się nie powieść na urządzenie wirtualne StorSimple lokalnego do innego urządzenia wirtualnego.

> [AZURE.VIDEO storsimple-virtual-array-disaster-recovery]

## <a name="business-continuity-disaster-recovery-bcdr"></a>Business ciągłości awarii (BCDR)

Scenariusz odzyskiwania (BCDR) po awarii ciągłości firm występuje w momencie awarii całego Azure centrum danych. Może to wpłynąć na usługi Menedżera StorSimple i urządzeń towarzyszących StorSimple.

W przypadku urządzeń StorSimple, które zostały zarejestrowane po prostu, przed wystąpieniem awarii tych urządzeniach StorSimple może być konieczne do usunięcia. Po awarii można ponownie utworzyć i skonfigurować te urządzenia.

## <a name="errors-during-dr"></a>Błędy podczas DR

**Chmura łączności awarii podczas DR**

Jeśli łączność chmurze jest zerwane po rozpoczęciu DR przed zakończeniem Przywróć urządzenia, DR zakończy się niepowodzeniem i otrzymasz powiadomienie. Urządzenia, której użyto dla oprogramowania DR następnie jest oznaczony jako *nie będzie można używać.* Tym samym urządzeniu docelowej nie można następnie dla przyszłych DRs.

**Nie zgodnych urządzeń**

Jeśli dostępne urządzenia nie ma wystarczająco dużo miejsca, pojawi się komunikat o błędzie w celu stwierdzenia, czy nie zgodnych urządzeń.

**Sprawdź błędy**

Jeśli jeden z prechecks nie jest spełniony, zostanie wyświetlona precheck błędy.

## <a name="next-steps"></a>Następne kroki

Informacje dotyczące sposobu [administrowania macierzy Virtual StorSimple korzystanie z lokalnego interfejsu użytkownika w sieci web](storsimple-ova-web-ui-admin.md).
