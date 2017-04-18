<properties 
   pageTitle="StorSimple lokalnie przypięte wielkości — często zadawane pytania | Microsoft Azure"
   description="Zawiera odpowiedzi na często zadawane pytania dotyczące ilości StorSimple lokalnie przypięte."
   services="storsimple"
   documentationCenter="NA"
   authors="manuaery"
   manager="syadav"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/16/2016"
   ms.author="manuaery" />

# <a name="storsimple-locally-pinned-volumes-frequently-asked-questions-faq"></a>StorSimple lokalnie przypięte wielkości: często zadawane pytania

## <a name="overview"></a>Omówienie

Poniżej przedstawiono pytania i odpowiedzi, które mogą zawierać po utworzeniu woluminu StorSimple przypięte lokalnie, konwertowanie woluminu warstwowych lokalnie przypięty objętość (i odwrotnie) lub wykonywanie kopii zapasowej i przywrócenia woluminu lokalnie przypięty.

Pytania i odpowiedzi są ustawione na następujące kategorie

- Tworzenie woluminu lokalnie przypięty
- Wykonywanie kopii zapasowej lokalnie przypięty
- Konwertowanie woluminu warstwowych lokalnie przypięty głośności
- Przywracanie lokalnie przypięty głośności
- Awarii lokalnie przypięty głośności

## <a name="questions-about-creating-a-locally-pinned-volume"></a>Pytania na temat tworzenia lokalnie przypięty woluminu

**PYTANIA.** Co to jest maksymalny rozmiar lokalnie przypięty woluminu, który można utworzyć na urządzeniach serii 8000?

**A** umożliwia obsługę wielkości lokalnie przypięty do 8,5 TB lub warstwowych wielkości maksymalnie 200 TB na tym urządzeniu 8100. Na urządzeniu większe 8600 umożliwia obsługę wielkości lokalnie przypięty do 22,5 TB lub warstwowych wielkości maksymalnie 500 TB.

**PYTANIA.** Aktualizacji 2 niedawno uaktualniony urządzenie 8100 i podczas próby utworzenia woluminu lokalnie przypięty maksymalny rozmiar dostępna jest tylko 6 TB i nie 8,5 TB. Dlaczego nie można utworzyć woluminu 8,5 TB?

**A** umożliwia obsługę lokalnie przypięty wielkości maksymalnie 8,5 lub TB tiered wielkości maksymalnie 200 TB na tym urządzeniu 8100. Jeśli urządzenie już zawiera tiered wielkości, a następnie miejsca do tworzenia woluminu lokalnie przypięty będzie proporcjonalnie niższej niż ten maksymalny limit. Na przykład jeśli 100 TB wielkości warstwowych, że już zainicjowano na urządzeniu 8100 (jest to połowa możliwości warstwowych), następnie maksymalny rozmiar woluminu lokalnego, którą można utworzyć na tym urządzeniu 8100 odpowiednio zmniejszy się do 4 TB (około połowie maksymalnej lokalnie przypięte pojemność woluminu).

Ponieważ niektórych lokalnych miejsce na urządzeniu jest używana do obsługi zestaw roboczy wielkości warstwowych, dostępnego miejsca do tworzenia lokalnie przypięty głośność jest ograniczona, jeśli urządzenie ma tiered wielkości. I odwrotnie Tworzenie woluminu lokalnie przypięty proporcjonalnie zmniejsza dostępnego miejsca do przedstawienia wielkości warstwowych. W poniższej tabeli przedstawiono dostępne możliwości warstwowych na urządzeniach 8100 i 8600 podczas tworzenia lokalnie przypięty wielkości.

|Wydajność ustanawianie lokalnie przypięty wielkości|Pojemność ma zostać zastrzeżona dla warstwowych wielkości - 8100|Pojemność ma zostać zastrzeżona dla warstwowych wielkości - 8600|
|-----|------|------|
|0 | 200 TB | 500 TB |
|1 TB | 176.5 TB | 477.8 TB|
|4 TB | 105.9 TB | 411.1 TB |
|8,5 TB | 0 TB | 311.1 TB|
|10 TB | BRAK | 277.8 TB |
|15 TB | BRAK | 166.7 TB |
|22,5 TB | BRAK | 0 TB |


**PYTANIA.** Dlaczego jest tworzenie woluminu lokalnie przypięty długotrwałych operacji? 

**ODPOWIEDZI.** Lokalnie przypięty wielkości grubo zainicjowano obsługę administracyjną. Aby utworzyć miejsca na lokalnym poziomów urządzenie, część danych z istniejącej wielkości warstwowych może zostać przeniesiony w chmurze podczas inicjowania obsługi administracyjnej. Ponieważ to zależy od rozmiaru woluminu obsługi administracyjnej, istniejące dane na urządzeniu i przepustowość w chmurze, czas potrzebny do tworzenia woluminu lokalnego może być kilka godzin.

**PYTANIA.** Jak długo trwa tworzenie woluminu lokalnie przypięty?

**ODPOWIEDZI.** Dlatego grubo zainicjowano lokalnie przypięty wielkości, niektóre dane z warstwowych wielkości może zostać przeniesiony w chmurze podczas inicjowania obsługi administracyjnej. W związku z tym czas potrzebny do utworzenia woluminu lokalnie przypięty zależy od wielu czynników, takich jak rozmiar woluminu, dane na urządzeniu i przepustowości. Na urządzeniu świeżo zainstalowanych występują nie ilości czas potrzebny do utworzenia lokalnie przypięty głośność jest około dziesięć minut na terabajtów danych. Jednak tworzenie wielkości lokalnym może potrwać kilka godzin, na podstawie czynników wyjaśniono na urządzeniu, który jest używany.

**PYTANIA.** Chcę utworzyć lokalnie przypięty głośność. Czy istnieją najważniejsze wskazówki, które należy wiedzieć o?

**ODPOWIEDZI.** Lokalnie przypięty wielkości są odpowiednie dla obciążenia, które wymagają lokalne gwarancje danych przez cały czas i rozróżniają opóźnienia w chmurze. Podczas ustalając zastosowania wielkości lokalne dla każdego z obciążeń pracą, należy pamiętać, że z następujących czynności:

- Grubo zainicjowano lokalnie przypięty wielkości i tworzenie lokalnych wielkości wpływa na dostępnego miejsca do przedstawienia wielkości warstwowych. W związku z tym zaleca się zaczynać się mniejszym wielkości i rozbudowy wraz ze wzrostem wymagane do miejsca do magazynowania.

- Inicjowanie obsługi administracyjnej wielkości lokalne jest długotrwałych operacji, która może obejmować przekazywanie istniejących danych z warstwowych wielkości w chmurze. W wyniku mogą wystąpić obniżonej wydajności tych ilości.

- Inicjowanie obsługi administracyjnej wielkości lokalne jest czasochłonne. Rzeczywisty czas związane zależy od wielu czynników: rozmiar danych głośność jest obsługi administracyjnej, na urządzeniu i przepustowość. Jeśli użytkownik nie wykonano kopii zapasowej do istniejącego woluminu w chmurze, Tworzenie woluminu jest mniejsza. Sugerujemy zapoznawanie tworzenia migawek chmury wielkości istniejących przed obsługę woluminu lokalnego.
 
- Możesz przekonwertować istniejący wielkości warstwowych lokalnie przypięty wielkości, a konwersja obejmuje inicjowania obsługi administracyjnej miejsca na urządzeniu do wyniku głośność lokalnie przypięty (oprócz wyłączania warstwowych danych [ewentualne] z chmury). Ponownie jest długotrwałych operacji, która zależy od czynników, które zostały omówione powyżej. Zaleca się wykonanie kopii zapasowej do istniejącego woluminu przed konwersji podczas procesu będzie jeszcze niższa, jeśli istniejącego woluminu nie są kopii zapasowej. Urządzenia mogą również wydajności ograniczona w trakcie tego procesu.
    
Więcej informacji na temat [tworzenia lokalnie przypięty głośności](storsimple-manage-volumes-u2.md#add-a-volume)

**PYTANIA.** W tym samym czasie można utworzyć przypisaną lokalnie przypięty?

**ODPOWIEDZI.** Tak, ale wszystkie zadania tworzenia i rozszerzenia lokalnie przypięty głośność są przetwarzane w kolejności.

Grubo zainicjowano lokalnie przypięty wielkości i wymaga to tworzenie lokalnych miejsca na tym urządzeniu (co może doprowadzić do istniejących danych z warstwowych wielkości, aby zostać przeniesiony do chmury podczas inicjowania obsługi administracyjnej). W związku z tym jeśli zadanie inicjowania obsługi administracyjnej jest w toku, inne zadania tworzenia woluminu lokalnego zostaną umieszczone w kolejce do momentu zakończenia tego zadania.

Podobnie jeśli jest rozwinięciu istniejącego woluminu lokalnego lub warstwowych głośność jest konwertowana na lokalnie przypięty głośność, tworzenia nowego woluminu lokalnie przypięty jest kolejce aż do zakończenia poprzedniego zadania. Zwiększenie rozmiaru woluminu lokalnie przypięty obejmuje rozszerzenia istniejącego lokalnego miejsca dla tego woluminu. Konwersja z warstwowych lokalnie przypięte głośność obejmuje również tworzenie lokalnej przestrzeni dla lokalnie wynikowym przypięte głośność. W zarówno tych operacji, tworzenie lub rozszerzanie lokalnej przestrzeni typu Liczba długa działa zadania.

Te zadania można wyświetlać na stronie **zadania** usługi Azure StorSimple menedżera. Zadanie, które aktywnie jest przetwarzana jest stale aktualizowany w celu odzwierciedlenia postępu miejsca inicjowania obsługi administracyjnej. Pozostałe zadania lokalnie przypięty głośność jest oznaczony jako działania, ale został zablokowany postępy i pobrane w kolejności, w jakiej zostały one w kolejce.

**PYTANIA.** Po usunięciu lokalnie przypięty głośność. Dlaczego nie widzisz obszaru osuszonego odzwierciedlane w dostępnego miejsca na dysku, gdy próbuję utworzyć nowy? 

**ODPOWIEDZI.** Jeśli usuniesz lokalnie przypięty głośność, miejsca dla nowych wielkości mogą nie zostać zaktualizowane natychmiast. Usługa Menedżer StorSimple aktualizuje lokalne miejsca około co godzinę. Sugerujemy zapoznawanie Czekaj na godzinę przed próby utworzenia nowego woluminu.

**PYTANIA.** Lokalnie przypięty wielkości są obsługiwane na urządzeniu chmurze?

**ODPOWIEDZI.** Lokalnie przypięty ilości nie są obsługiwane na urządzeniu chmury (8010 i 8020 urządzenia wcześniej określone jako urządzenia wirtualnego StorSimple).

**PYTANIA.** Tworzyć i zarządzać nimi lokalnie przypięty wielkości można używać polecenia cmdlet programu PowerShell Azure? 

**ODPOWIEDZI.** Nie, nie można utworzyć lokalnie przypięty wielkości przy użyciu poleceń cmdlet programu PowerShell Azure (tiered głośność, wszelkie utworzone za pomocą programu PowerShell Azure). Również Sugerujemy nie umożliwiająca polecenia cmdlet programu PowerShell Azure modyfikowanie wszystkie właściwości lokalnie przypięty wielkości, jak będzie miał niepożądany efekt zmodyfikowanie typu woluminu do warstwowych.

## <a name="questions-about-backing-up-a-locally-pinned-volume"></a>Pytania na temat kopii zapasowych lokalnie przypięty głośności

**PYTANIA.** Są lokalne migawek wielkości lokalnie przypięty obsługiwane?

**ODPOWIEDZI.** Tak, można wykonać lokalne migawki wielkości lokalnie przypięty. Jednak zdecydowanie zaleca się regularne tworzenie kopii zapasowej usługi lokalnie przypięty wielkości z chmury migawki, aby upewnić się, że dane są chronione w możliwości awarii.

**PYTANIA.** Czy są jakieś wskazówki do zarządzania lokalnego migawek dla lokalnie przypięty ilości?

**ODPOWIEDZI.** Częste migawek lokalne razem z pakietem dużą liczbę pochodząca danych wielkości lokalnie przypięty może spowodować lokalnej przestrzeni na tym urządzeniu, można szybko zużyte i powodują danych z warstwowych wielkości zostanie przeniesiony do chmury. Zaleca się w związku z tym, że zminimalizować liczbę migawek lokalny.

**PYTANIA.** Otrzymano alert informujący, że mogą być unieważniane mojej lokalnej migawek wielkości lokalnie przypięty. Jeśli to możliwe?

**ODPOWIEDZI.** Częste migawek lokalne razem z pakietem dużą liczbę pochodząca danych wielkości lokalnie przypięty może spowodować lokalnej przestrzeni na tym urządzeniu szybko zużycie. Jeśli lokalne poziomów urządzenie jest intensywnie używany, awaria rozszerzonego chmury może spowodować urządzenia staje się pełny i zapisuje przychodzących do woluminu może spowodować unieważnieniu migawki (jak bez spacji istnieje zaktualizować migawki dotyczyć starsze bloki danych, który został zastąpiony). W takiej sytuacji będzie zgłaszane zapisywania głośność, ale lokalne migawek może być nieprawidłowy. Nie ma żadnego wpływu na do istniejącej migawki chmury.

Alert ostrzeżenie jest informujący, że takiej sytuacji można mogą pojawić się i upewnij się, że taki sam adres w odpowiednim czasie przeglądania planów migawek lokalne do rzadziej migawek lokalne lub usuwanie starszych migawek lokalne, które nie są wymagane.

Jeśli lokalne migawki są unieważniane, otrzymasz alert informacji powiadomienie, że lokalne migawek dla określonej zasady kopii zapasowej mają zostały unieważniane obok listy sygnatury czasowe migawek lokalnych, które zostały unieważniane. Migawek zostaną usunięte automatycznie i nie będą mogli wyświetlać je na stronie **Wykazy kopii zapasowych** w portalu klasyczny Azure.

## <a name="questions-about-converting-a-tiered-volume-to-a-locally-pinned-volume"></a>Pytania dotyczące konwertowania woluminu warstwowych lokalnie przypięty głośności

**PYTANIA.** Jestem I obserwowania niektórych powolność na tym urządzeniu podczas konwertowania woluminu warstwowych na lokalnie przypięty głośność. Dlaczego tak się dzieje? 

**ODPOWIEDZI.** Proces konwersji obejmuje dwa kroki: 

  1. Obsługa administracyjna miejsca na tym urządzeniu, aby szybko do--przekonwertowane lokalnie przypięta głośność.
  2. Pobieranie danych warstwowych z w chmurze, aby upewnić się, lokalne gwarantuje.

Oba te kroki są długie długotrwałych operacji, które są zależne od rozmiaru woluminu konwertowany danych na urządzeniu i przepustowość. Jak dane z istniejącego woluminu warstwowych może rozsypywać w chmurze w ramach procesu obsługi administracyjnej, urządzenia mogą wystąpić obniżonej wydajności, w tym czasie. Ponadto, proces konwersji może być mniejsza jeśli:

- Nie wykonano kopii istniejącego woluminu w górę w chmurze; Zaleca się wykonanie kopii zapasowej do przedstawienia wielkości przed podjęciem konwersji.

- Zasady ograniczania przepustowości zostały zastosowane, które mogą ograniczyć przepustowość w chmurze; Zaleca się w związku z tym, że masz dedykowane MB/s 40 lub więcej połączenia w chmurze.

- Konwersja może potrwać kilka godzin ze względu na czynniki wielokrotności wyjaśniono powyżej; w związku z tym zaleca się wykonanie tej operacji w godzinach innych niż wartości lub weekend unikać wpływ na końcu konsumentów.

Więcej informacji na temat [konwersji woluminu warstwowych do woluminu lokalnie przypięty](storsimple-manage-volumes-u2.md#change-the-volume-type)

**PYTANIA.** Czy można anulować operacji konwersji woluminu?

**ODPOWIEDZI.** Nie, nie możesz Anuluj Operacja konwersji raz zainicjowana. Zgodnie z opisem w poprzedniego pytania, należy pamiętać o potencjalnych problemach wydajności, że mogą wystąpić podczas procesu i zgodne z najważniejszymi wskazówkami wymienionych powyżej, podczas planowania do konwersji.

**PYTANIA.** Co się stanie z moim głośności w przypadku niepowodzenia operacji konwersji?

**ODPOWIEDZI.** Konwersji woluminu może się niepowodzeniem z powodu problemów z łącznością z chmury. Urządzenia po pewnym czasie może przestać proces konwersji po serii zakończone niepowodzeniem próby może spowodować przerwanie warstwowych danych z chmury. W takim przypadku nadal będzie typ źródła głośność przed konwersji typu woluminu i:

- Alert krytyczny zostaną podniesione informujący o błąd konwersji głośność. Więcej informacji na temat [alerty dotyczące ilości lokalnie przypięty](storsimple-manage-alerts.md#locally-pinned-volume-alerts)

- Jeśli są konwertowane warstwowych na lokalnie przypięty głośność, wielkość będzie powodować wystąpienie właściwości woluminu warstwowych, zgodnie z danymi nadal mogą znajdować się w chmurze. Sugerujemy rozwiązać problemy z łącznością, a następnie ponów próbę operacji konwersji.
 
- Podobnie podczas konwersji lokalnie przypięty do woluminu warstwowych nie powiedzie się, mimo że wielkość będzie oznaczona jako lokalnie przypięty głośność, go będzie działać jako warstwowych objętość (ponieważ danych może mieć rozrzucone w chmurze). Jednak nadal zajmują miejsce na lokalnym poziomów urządzenia. Ten obszar nie będą dostępne dla innych lokalnie przypięty wielkości. Sugerujemy ponownie wykonać tę operację, aby zapewnić zakończeniu konwersji głośności i lokalnej przestrzeni na tym urządzeniu można odzyskać.

## <a name="questions-about-restoring-a-locally-pinned-volume"></a>Pytania na temat przywracania lokalnie przypięty głośności

**PYTANIA.** Są lokalnie przypięty wielkości przywrócona od razu?

**ODPOWIEDZI.** Tak, lokalnie przypięty ilości zostaną przywrócone natychmiast. Jak informacje o wielkość metadanych są pobierane z chmury w ramach operacji przywracania, głośność trybu online i mogą uzyskiwać dostęp do hosta. Jednak lokalne gwarancje dotyczące wielkość, że dane nie będą Prezentuj, aż wszystkie dane został pobrany z chmury, a może wystąpić ograniczona wydajność tych ilości czasu trwania przywracania.

**PYTANIA.** Jak długo trwa przywracanie woluminu lokalnie przypięty?

**ODPOWIEDZI.** Lokalnie przypięty wielkości są od razu przywrócić i przełączyć do trybu online po zainicjowaniu metadane głośność jest pobierana z chmury, gdy dane woluminu w dalszym ciągu można pobrać w tle. Ten ostatni część Przywracanie — powrót lokalne gwarancje dla danych objętości — długotrwałych operacji i może potrwać kilka godzin dla wszystkich danych należy lokalne ponownie. Czas potrzebny do wykonania takie same zależy od wielu czynników, takich jak rozmiar woluminu Trwa przywracanie i przepustowości. Usunięcie woluminu oryginalnego, że trwa przywracanie kolejny raz nastąpi przekierowanie do tworzenia lokalnych miejsca na urządzeniu w ramach operacji przywracania.

**PYTANIA.** Chcę przywrócić Moje istniejącego woluminu lokalnie przypięty do starszych migawki (wykonana po wielkość została tiered). Głośność będzie można przywrócić jako tiered w tym przypadku?

**ODPOWIEDZI.** Nie, zostaną przywrócone wielkość jako lokalnie przypięty głośność. Mimo że daty migawkę do momentu, gdy został tiered głośność, podczas przywracania istniejącego woluminu, StorSimple zawsze używa typu woluminu na dysku istnieje obecnie.

**PYTANIA.** Moje lokalnie przypięty głośności I rozszerzony ostatnio, ale teraz trzeba przywrócić dane do czasu, gdy wielkość była mniejszym rozmiarze. Spowoduje zmianę rozmiaru Przywróć głośności i będzie potrzebny zwiększyć rozmiar woluminu po zakończeniu przywracania?

**ODPOWIEDZI.** Tak, przywracanie spowoduje zmianę rozmiaru głośności i będzie konieczne rozszerzanie rozmiaru woluminu po zakończeniu przywracania.

**PYTANIA.** Czy mogę zmienić typ woluminu podczas przywracania?

**A.** Nie, nie można zmienić typu woluminu podczas przywracania.

- Ilości, które zostały usunięte zostaną przywrócone jako typ przechowywanych w migawki.

- Istniejące ilości zostaną przywrócone na podstawie ich bieżącego typu, niezależnie od typu przechowywanych w migawki (zobacz poprzedniego dwa pytania).
 
**PYTANIA.** Chcę przywrócić Moje lokalnie przypięty głośność, ale można pobrać punkt niepoprawne tworzenia migawek. Czy można anulować bieżącej operacji przywracania?

**ODPOWIEDZI.** Tak, możesz anulować przywracanie w toku. Stan głośność zostanie wycofana do stanu na początku przywracania. Jednak zapisu, wszelkie wprowadzone podczas przywracania jest w toku wielkość zostaną utracone.

**PYTANIA.** Można uruchomić operację przywracania na jednym z moich lokalnie przypięty wielkości, a teraz zobaczyć migawki w moim wykazie zaległości, które nie recollect tworzenia. Do czego to służy?

**ODPOWIEDZI.** Jest tymczasowy migawki, zostanie utworzona przed przywracanie, który jest używany na potrzeby wycofywania w przypadku przywracania została anulowana lub nie powiedzie się. Nie usuwaj tej migawki; jej zostaną automatycznie usunięte po zakończeniu przywracania. Ten problem może wystąpić, jeśli zadania przywracania zawiera tylko lokalnie przypięte wielkości lub kombinację wielkości lokalnie przypiętych i warstwowych. Jeśli zadanie przywracania zawiera tylko warstwowych wielkości, ten problem nie będzie obsługiwane.

**PYTANIA.** Czy można klonowanie lokalnie przypięty głośności?

**ODPOWIEDZI.** Tak, możesz to zrobić. Jednak wielkość lokalnie przypięty będzie domyślnie klonowany jako warstwowych głośność. Więcej informacji na temat [klonowanie lokalnie przypięty głośności](storsimple-clone-volume-u2.md)

## <a name="questions-about-failing-over-a-locally-pinned-volume"></a>Pytania dotyczące awarii lokalnie przypięty głośności

**PYTANIA.** Do czego jest potrzebny niepowodzenie na Moje urządzenie do innego urządzenia fizycznym. Moje lokalnie przypięty wielkości nie powiedzie się powyżej lub lokalnie przypięciu tiered?

**ODPOWIEDZI.** W zależności od wersji oprogramowania urządzenia docelowego lokalnie przypięty wielkości nie powiedzie się nad jako:

- Przypięte lokalnie, jeśli urządzenie działa StorSimple 8000 serii Aktualizacja 2
- Tiered Jeśli urządzenie działa StorSimple serii 8000 aktualizowanie 1.x
- Tiered Jeśli urządzenie jest urządzenia chmury (aktualizacji oprogramowania w wersji 2 lub aktualizowanie 1.x)

Więcej informacji na temat [awaryjnego i DR dla lokalnie przypięte wielkości w wersjach](storsimple-device-failover-disaster-recovery.md#device-failover-across-software-versions)

**PYTANIA.** Są lokalnie przypięty wielkości przywracane natychmiast podczas awarii (DR)?

**ODPOWIEDZI.** Tak, lokalnie przypięty ilości zostaną przywrócone natychmiast podczas pracy awaryjnej. Jak informacje o wielkość metadanych są pobierane z chmury w ramach operacji pracy awaryjnej, głośność tryb online na urządzeniu docelowej i mogą uzyskiwać dostęp do hosta. W międzyczasie dane woluminu będzie w dalszym ciągu pobieranie w tle i mogą wystąpić obniżonej wydajności tych ilości na czas trwania tym przełączeniu.

**PYTANIA.** Wyświetlać pracy awaryjnej zadanie jest wykonywane, jak można śledzić postęp wielkości lokalnie przypięty Trwa przywracanie na urządzenie?

**ODPOWIEDZI.** Podczas operacji pracy awaryjnej pracy awaryjnej zadania jest oznaczony jako raz wszystkie wielkość w zestawie pracy awaryjnej zostały natychmiast przywrócić i przełączyć do trybu online na urządzeniu docelowej. Ta opcja uwzględnia lokalnie przypięty przedstawienia wielkości, które mogą nie Jednak lokalne gwarancje danych będzie dostępna jedynie po pobraniu wszystkich danych dla głośność. Możesz śledzić ten proces dla każdego lokalnie przypięty woluminu, który nie powiodło się od podstaw, monitorowanie odpowiadające im zadania przywracania są tworzone jako część tym przełączeniu. Te indywidualnych zadań przywracania zostanie utworzone tylko dla lokalnie przypięty wielkości.

**PYTANIA.** Czy mogę zmienić typ woluminu podczas pracy awaryjnej?

**ODPOWIEDZI.** Nie, nie można zmienić typu woluminu w trybie awaryjnym. Jeśli kończy się niepowodzeniem nad do innego fizycznie urządzenia z systemem StorSimple 8000 serii Aktualizacja 2, wielkość nie powiedzie się nad zależy od typu głośność przechowywane migawki. Przy awarii jakąkolwiek inną wersję urządzenia, można znaleźć na pytanie powyżej na typu woluminu po przełączeniu.

**PYTANIA.** Można nie w kontenerze głośność z ilości przypięty lokalnie na urządzeniu chmury?

**ODPOWIEDZI.** Tak, możesz to zrobić. Wielkość lokalnie przypięty nie powiedzie się nad jako warstwowych. Więcej informacji na temat [awaryjnego i DR dla lokalnie przypięte wielkości w wersjach](storsimple-device-failover-disaster-recovery.md#considerations-for-device-failover)


