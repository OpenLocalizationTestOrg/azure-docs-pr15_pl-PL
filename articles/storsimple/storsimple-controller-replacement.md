<properties 
   pageTitle="Zamienianie kontroler urządzenia StorSimple | Microsoft Azure"
   description="W tym miejscu wyjaśniono, jak usunąć i zastąpić jednej lub obu moduły kontroler na urządzeniu StorSimple."
   services="storsimple"
   documentationCenter=""
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

# <a name="replace-a-controller-module-on-your-storsimple-device"></a>Zamień moduł kontrolera na urządzeniu StorSimple

## <a name="overview"></a>Omówienie

Ten samouczek wyjaśniono, jak usunąć i zamienić jednej lub obu modułów kontroler w urządzeniu StorSimple. Zawiera omówienie również źródłowych logika dotycząca kontroler jednej lub dwóch sytuacji wymiany.

>[AZURE.NOTE] Przed wykonaniem zamienny kontrolerze, zalecamy zawsze aktualizacji oprogramowania sprzętowego kontrolera do najnowszej wersji.
>
>Aby zapobiec uszkodzeniu urządzeniem StorSimple, nie wysunąć kontroler aż LED są wyświetlane jako jedną z następujących czynności:
>
>- Wszystkie światła są wyłączone.
>- LED 3 ![ikona zielony znacznik wyboru](./media/storsimple-controller-replacement/HCS_GreenCheckIcon.png), i ![Ikona czerwonego krzyżyka](./media/storsimple-controller-replacement/HCS_RedCrossIcon.png) są migające i LED 0 i LED 7 są **włączone**.

W poniższej tabeli przedstawiono scenariusze zamienny kontroler obsługiwane.

|Wielkość liter|Wymiana scenariusz|Procedura dotyczy|
|:---|:-------------------|:-------------------|
|1|Jeden kontroler jest w stanie awarii, drugi kontroler jest prawidłowy i aktywną.|[Wymiana pojedynczy kontroler](#replace-a-single-controller), opisujący [logiki zastąpienie pojedynczy kontroler](#single-controller-replacement-logic), a także [zastąpienie czynności](#single-controller-replacement-steps).|
|2|Oba kontrolery nie powiodła się i wymagać wymiany. Obudowie dysków and.disk załącznik jest prawidłowy.|[Wymiana podwójne kontrolerze](#replace-both-controllers), opisujący [logiki zamienny podwójne kontrolerze](#dual-controller-replacement-logic), a także [czynności zamienny](#dual-controller-replacement-steps). |
|3|Są miejscami kontrolerów z tym samym urządzeniu lub z innego urządzenia. Obudowa, dyski i załączniki dysku jest prawidłowy.|Zostanie wyświetlony komunikat alertu niezgodności przedział.|
|4|Jeden kontroler nie jest widoczny, a drugi kontroler kończy się niepowodzeniem.|[Wymiana podwójne kontrolerze](#replace-both-controllers), opisujący [logiki zamienny podwójne kontrolerze](#dual-controller-replacement-logic), a także [czynności zamienny](#dual-controller-replacement-steps).|
|5|Jeden lub oba nie powiodło się. Nie można uzyskać dostęp do urządzenia, za pomocą konsoli szeregowego lub zdalnych programu Windows PowerShell.|[Kontakt z pomocą techniczną firmy Microsoft](storsimple-contact-microsoft-support.md) procedurę zamienny kontroler ręcznego.|
|6|Kontrolery są w wersji kompilacji różnych, która może być:<ul><li>Kontrolery zainstalowana wersja oprogramowania.</li><li>Kontrolery zainstalowana wersja innego oprogramowania układowego.</li></ul>|W przypadku różnych wersji oprogramowania kontroler logiczny zastąpienie wykrywa, że i aktualizuje wersję oprogramowania na kontrolerze zamienny.<br><br>Jeśli wersje oprogramowania sprzętowego kontrolera są różne i jest starszą wersję oprogramowania układowego **nie** automatycznie rozszerzenia komunikat alertu pojawią się w portalu klasyczny Azure. Należy Skanuj w poszukiwaniu aktualizacji i instalowanie aktualizacji oprogramowania układowego.</br></br>Jeśli wersje oprogramowania sprzętowego kontrolera są różne i starszą wersję oprogramowania układowego jest automatycznie rozszerzenia, logiczny zamienny kontroler wykryje to, a po uruchomieniu kontroler oprogramowania układowego zostaną automatycznie zaktualizowane.|

Musisz usunąć moduł kontrolerze, jeśli nie powiodła się. Jeden lub oba modułów kontrolera może się nie powieść, co może spowodować w pojedynczy kontroler wymiana lub wymiany dwóch kontrolera. Wymiana procedury i logiki ich zobacz następujące artykuły:

- [Zamienianie pojedynczy kontroler](#replace-a-single-controller)
- [Zamienianie obu kontrolerów](#replace-both-controllers)
- [Usuwanie kontrolera](#remove-a-controller)
- [Wstawianie kontrolera](#insert-a-controller)
- [Zidentyfikuj kontroler aktywne na urządzeniu](#identify-the-active-controller-on-your-device)

>[AZURE.IMPORTANT] Przed usuwania i zamieniania kontrolerze, przejrzyj informacje bezpieczeństwa w [Wymiana składników sprzętu StorSimple](storsimple-hardware-component-replacement.md).

## <a name="replace-a-single-controller"></a>Zamienianie pojedynczy kontroler

Jeden dwa kontroler na tym urządzeniu Microsoft Azure StorSimple nie powiodło się, działa poprawnie lub brakuje należy zastąpić pojedynczy kontroler. 

### <a name="single-controller-replacement-logic"></a>Logika zamienny pojedynczy kontroler

W zastąpienie pojedynczy kontroler należy najpierw usunąć administratora danych nie powiodło się. (Pozostałe kontroler w urządzeniu jest aktywne kontroler). Po wstawieniu kontrolera zamienny, wykonywane są następujące akcje:

1. Wymiana kontrolera natychmiast uruchamia komunikowania się z urządzeniem StorSimple.

2. Migawki wirtualnego dysku twardego (wirtualnego dysku twardego) dla aktywnego kontrolera jest kopiowana na kontrolerze zamienny.

3. Migawka jest zmodyfikowany tak, aby po uruchomieniu kontroler zastąpienie z tym wirtualnego dysku twardego zostanie rozpoznany jako kontroler wstrzymania.

4. Po zakończeniu modyfikacji kontroler zastąpienie rozpocznie się jako administrator wstrzymania.

5. Po uruchomieniu zarówno kontrolery klastrze przechodzi do trybu online.

### <a name="single-controller-replacement-steps"></a>Pojedynczy kontroler zamienny czynności

Jeśli jeden kontroler w urządzeniu Microsoft Azure StorSimple kończy się niepowodzeniem, należy wykonać następujące czynności. (Drugi kontroler musi być aktywna i uruchomiony. Jeśli oba kontrolery się nie powieść lub niepoprawnie, przejdź do [dwóch kontroler zamienny kroków](#dual-controller-replacement-steps).)

>[AZURE.NOTE] Może potrwać 30 — 45 minut kontrolera ponownie uruchomić i całkowicie odzyskiwanie z procedury zamienny pojedynczy kontroler. Całkowity czas całej procedury, w tym dołączanie kable, jest około 2 godziny.

#### <a name="to-remove-a-single-failed-controller-module"></a>Aby usunąć moduł pojedynczy kontroler nie powiodło się

1. W portalu klasyczny Azure przejdź do usługi Menedżera StorSimple, kliknij kartę **urządzenia** , a następnie kliknij nazwę urządzenia, które chcesz monitorować.

2. Przejdź do pozycji **Konserwacja > Stan sprzętu**. Stan kontroler 0 lub 1 kontroler powinien być czerwony, oznaczający błąd.

    >[AZURE.NOTE] To nie powiodło się kontroler w zamiennych pojedynczy kontroler jest zawsze kontrolera wstrzymania.

3. Aby zlokalizować moduł kontrolera nie powiodło się za pomocą rysunek 1 i poniższej tabeli.  

    ![Tablica połączeń urządzenia podstawowego załącznik modułów](./media/storsimple-controller-replacement/IC740994.png)

    **Rysunek 1** Tylnej strony StorSimple urządzenia

  	|Etykieta|Opis|
  	|:----|:----------|
  	|1|PCM 0|
  	|2|PCM 1|
  	|3|Kontroler 0|
  	|4|Kontroler 1|

4. Na kontrolerze nie powiodło się należy usunąć wszystkie kable sieciowe połączonego z portów danych. Jeśli jest używany model 8600, także usunąć kable skojarzeń zabezpieczeń łączące kontroler na kontrolerze EBOD.

5. Postępuj zgodnie z instrukcjami [Usuwanie kontrolera](#remove-a-controller) , aby usunąć kontroler nie powiodło się. 

6. Zainstaluj zastąpienie factory w tym samym przedział, z którego usunięto uszkodzony kontroler. Uaktywnia to logiczny zamienny pojedynczy kontroler. Aby uzyskać więcej informacji zobacz [logiki zastąpienie pojedynczy kontroler](#single-controller-replacement-logic).

7. Gdy logiczny zastąpienie pojedynczy kontroler postępów w tle, ponownie podłącz kable. Zwrócić uwagę, aby połączyć wszystkie kable ten sam sposób, aby były one połączone przed zastąpić.

8. Po uruchomieniu kontroler Sprawdź **Stan kontrolera** i **klaster stanu** w portalu klasyczny Azure, aby sprawdzić, czy kontroler jest prawidłowy stan i jest w stanie wstrzymania.

>[AZURE.NOTE] Jeśli są monitorowania urządzenia za pomocą konsoli szeregowego, można zobaczyć wielokrotnego ponownego uruchamiania podczas kontroler jest odzyskiwanie z procedury zamienny. Po menu Konsola szeregowa są prezentowane, następnie Pamiętaj o ukończeniu zastąpić. Menu nie są wyświetlane w ciągu dwóch godzin: rozpoczynanie wymiany kontrolerze, zapoznaj się z [skontaktuj się z pomocą techniczną firmy Microsoft](storsimple-contact-microsoft-support.md).

## <a name="replace-both-controllers"></a>Zamienianie obu kontrolerów

Po obu kontrolerów na tym urządzeniu Microsoft Azure StorSimple nie powiodło się, są nieprawidłowo lub brakuje, należy zastąpić obu kontrolerów. 

### <a name="dual-controller-replacement-logic"></a>Logika zamienny podwójny kontroler

W zastąpienie dwóch kontrolerze najpierw usuń obu kontrolerów nie powiodło się, a następnie wstawić zamiany. Podczas wstawiania kontrolery zastąpienie dwóch wykonywane są następujące akcje:

1. Kontroler wymiana w przedział 0 sprawdza następujące czynności:
 
   1. Jest go za pomocą bieżących wersji oprogramowania układowego i oprogramowania?

   2. Jest to część klaster?

   3. Jest uruchomiony kontroler równorzędnych i jest to wykres kolumnowy grupowany?
                            
    Jeśli żaden z tych warunków nie są spełnione, kontroler wyszukuje najnowszej dzienny kopii zapasowej (znajdujące się w **nonDOMstorage** na dysku S). Kontroler kopiuje najnowszą migawkę wirtualnego dysku twardego z kopii zapasowej.

2. Kontroler w przedział 0 używa migawki samego obrazu.

3. W międzyczasie administratora w przedział 1 czeka na kontrolerze 0, wypełnić wprowadzenie i uruchomić.

4. Po uruchomieniu kontroler 0 kontrolerze 1 wykrywa klaster utworzone przez administratora 0, wyzwalające logiczny zamienny pojedynczy kontroler. Aby uzyskać więcej informacji zobacz [logiki zastąpienie pojedynczy kontroler](#single-controller-replacement-logic).

5. Po obu kontrolerach zostanie uruchomiony i klaster będzie trybu online.

>[AZURE.IMPORTANT] Po zastąpienie dwóch kontrolerze po skonfigurowaniu urządzenia StorSimple, konieczne jest wykonanie ręczne tworzenie kopii zapasowych urządzenia. Dzienny kopie zapasowe konfiguracji urządzenia nie muszą być uruchamiane poczekaj, aż po upływie 24 godzin. Praca z [Pomocy technicznej firmy Microsoft](storsimple-contact-microsoft-support.md) Utwórz kopię zapasową urządzenia ręcznie.

### <a name="dual-controller-replacement-steps"></a>Podwójny kontroler zastąpienie czynności

Ten przepływ pracy jest wymagany w przypadku, gdy oba kontroler w urządzeniu Microsoft Azure StorSimple nie powiodła się. Może się to zdarzyć w centrum danych, w której układ chłodzenia przestanie działać, a w wyniku obu kontrolery kończą się niepowodzeniem w krótkim czasu. W zależności od tego, czy urządzenia StorSimple jest wyłączona, i tego, czy korzystasz z 8600 lub 8100 model, innego zestawu czynności są wymagane.

>[AZURE.IMPORTANT] 1 godzina kontrolera ponownie uruchomić i całkowicie odzyskać z procedury zamienny podwójny kontroler może potrwać 45 minut. Całkowity czas całej procedury, w tym dołączanie kable, jest około 2,5 godziny.

#### <a name="to-replace-both-controller-modules"></a>Aby zamienić obu moduły kontroler

1. Jeśli urządzenie jest wyłączone, Pomiń ten krok i przejść do następnego kroku. Jeśli urządzenie jest włączone, wyłączyć urządzenie.
                                        
    1. Jeśli jest używany model 8600, wyłączyć pierwszego podstawowego załącznik, a następnie wyłącz opcję Załącznik EBOD.

    2. Poczekaj, aż urządzenie została całkowicie zamknięta. Wszystkie LED z tyłu urządzenia będą wyłączone.

2. Usuń wszystkie kable sieciowe, które są podłączone do portów danych. Jeśli jest używany model 8600, także usunąć kable skojarzeń zabezpieczeń łączących się załącznik EBOD z podstawowego załącznik.

3. Usuwanie obu kontrolerów z urządzenia StorSimple. Aby uzyskać więcej informacji zobacz [Usuwanie kontrolera](#remove-a-controller).

4. Najpierw Wstaw przyklejanych factory kontroler 0, a następnie Wstaw kontroler 1. Aby uzyskać więcej informacji zobacz [Wstawianie kontrolerze](#insert-a-controller). Uaktywnia to logiczny zastąpienie podwójny kontroler. Aby uzyskać więcej informacji zobacz [logiki zamienny podwójny kontroler](#dual-controller-replacement-logic).

5. Gdy logiczny zastąpienie kontroler postępów w tle, ponownie podłącz kable. Zwrócić uwagę, aby połączyć wszystkie kable ten sam sposób, aby były one połączone przed zastąpić. Zobacz szczegółowe instrukcje modelu kabel sekcji urządzenia [zainstalować urządzenia StorSimple 8100](storsimple-8100-hardware-installation.md) lub [urządzeniu StorSimple 8600](storsimple-8600-hardware-installation.md).

6. Włącz urządzenie StorSimple. Jeśli korzystasz z modelu 8600:

    1. Upewnij się, że załącznik EBOD jest włączona w pierwszym.

    2. Poczekaj, aż załącznik EBOD jest uruchomiony.

    3. Włącz funkcję podstawowego załącznik.

    4. Po pierwszym kontrolerze ponownym uruchomieniu i jest w stanie prawidłowy, system zostanie uruchomiony.

    >[AZURE.NOTE] Jeśli są monitorowania urządzenia za pomocą konsoli szeregowego, można zobaczyć wielokrotnego ponownego uruchamiania podczas kontroler jest odzyskiwanie z procedury zamienny. Po wyświetleniu menu konsoli szeregowego następnie Pamiętaj, że zastąpić jest pełny. Menu nie są wyświetlane w ciągu 2,5 godzin: rozpoczynanie wymiany kontrolerze, zapoznaj [się z pomocą techniczną firmy Microsoft](storsimple-contact-microsoft-support.md).

## <a name="remove-a-controller"></a>Usuwanie kontrolera

Wykonaj poniższą procedurę, aby usunąć moduł uszkodzony kontroler StorSimple na urządzeniu z systemem.

>[AZURE.NOTE] Następujące ilustracje są kontrolera 0. Kontrolera 1 to będzie można cofnąć.

#### <a name="to-remove-a-controller-module"></a>Aby usunąć moduł kontrolera

1. Ujmij zatrzaśnięć modułu między kciuka i wskazującym.

2. Lekko zmieścić kciuka i wskazującym ze sobą, aby zwolnić zatrzaśnięć kontrolera usługi.

    ![Zwalnianie zatrzaśnięć kontroler](./media/storsimple-controller-replacement/IC741047.png)

    **Rysunek 2** Zwalnianie zatrzaśnięć kontroler

2. Umożliwia zatrzask jako uchwyt slajdów kontroler poza obudowy.

    ![Przesuwanie kontroler poza obudowy](./media/storsimple-controller-replacement/IC741048.png)

    **Rysunek 3** Przesuwanie kontrolera poza obudowy

## <a name="insert-a-controller"></a>Wstawianie kontrolera

Poniższa procedura umożliwia zainstalować moduł dostarczonym factory kontrolera po usunięciu uszkodzony moduł StorSimple na urządzeniu z systemem.

#### <a name="to-install-a-controller-module"></a>Aby zainstalować moduł kontrolera

1. Sprawdź, czy jest szkody do łączników interfejsu. Nie można zainstalować moduł Jeśli dowolne PIN łącznika są uszkodzone lub też odchyleniu.

2. Ustaw moduł kontrolera do podwozia podczas pełni zwolnieniu zatrzask. 

    ![Przesuwanie kontroler do obudowy](./media/storsimple-controller-replacement/IC741053.png)

    **Rysunek 4** Przesuwanie kontroler do obudowy

3. Z modułem kontrolera wstawiony Rozpocznij zamknięcia zatrzask pozostawiając push moduł kontrolera do obudowy. Zatrzask będzie prowadzić do przeprowadzenia kontroler na właściwe miejsce.

    ![Zamykanie zatrzaśnięć kontroler](./media/storsimple-controller-replacement/IC741054.png)

    **Rysunek 5** Zamykanie zatrzaśnięć kontroler

4. Gotowe po zatrzask przyciągnięta na miejsce. **OK** LED będą znajdować się na.  

    >[AZURE.NOTE] Może potrwać do 5 minut kontroler i LED do aktywowania.

5. Aby sprawdzić, czy zastąpić zakończy się powodzeniem, w portalu klasyczny Azure, przejdź do pozycji **urządzenia** > **Konserwacja** > **Stan sprzętu**i upewnij się, że zarówno kontroler 0, jak i kontroler 1 są prawidłowy (stan to zielony).

## <a name="identify-the-active-controller-on-your-device"></a>Zidentyfikuj kontroler aktywne na urządzeniu

Istnieje wiele sytuacji, takich jak zastąpienie rejestracji lub kontroler urządzenia po raz pierwszy, które wymagają zlokalizować kontroler aktywne na urządzeniu StorSimple. Aktywne kontroler przetwarza wszystkie dysku sprzętowego i sieci operacje. Do identyfikowania active kontrolera można służą dowolną z następujących metod:

- [Zidentyfikuj kontroler aktywnej za pomocą portalu klasyczny Azure](#use-the-azure-classic-portal-to-identify-the-active-controller)

- [Używanie programu Windows PowerShell dla StorSimple do identyfikowania active kontrolera](#use-windows-powershell-for-storsimple-to-identify-the-active-controller)

- [Sprawdzanie urządzenia fizycznego do identyfikowania active kontrolera](#check-the-physical-device-to-identify-the-active-controller)

Każde z tych procedur opisano dalej.

### <a name="use-the-azure-classic-portal-to-identify-the-active-controller"></a>Zidentyfikuj kontroler aktywnej za pomocą portalu klasyczny Azure

W portalu klasyczny Azure, przejdź do **urządzenia** > **Konserwacja**i przewiń do sekcji **kontrolerów** . W tym miejscu można sprawdzić, którego kontrolera jest aktywna.

![Zidentyfikuj kontroler aktywna w portalu klasyczny Azure](./media/storsimple-controller-replacement/IC752072.png)

**Rysunek 6** Azure portalu klasyczny przedstawiający active kontrolera

### <a name="use-windows-powershell-for-storsimple-to-identify-the-active-controller"></a>Używanie programu Windows PowerShell dla StorSimple do identyfikowania active kontrolera

Podczas uzyskiwania dostępu do urządzenia za pomocą konsoli szeregowego są prezentowane wiadomość transparentu. Transparent wiadomość zawiera urządzenie podstawowe informacje, takie jak model, nazwy, zainstalowana wersja oprogramowania i stan kontroler, który chcesz uzyskać dostęp. Poniższy obraz przedstawia przykład transparent wiadomości:

![Liczba kolejna transparentem](./media/storsimple-controller-replacement/IC741098.png)

**Rysunek 7** Wstęga wiadomości przedstawiający kontroler 0 jako aktywny

Komunikat w postaci transparentu umożliwia określanie, czy kontroler, który są połączeni z jest aktywne lub pasywne.

### <a name="check-the-physical-device-to-identify-the-active-controller"></a>Sprawdzanie urządzenia fizycznego do identyfikowania active kontrolera

Aby zidentyfikować active kontrolera na urządzeniu, odszukaj niebieski LED powyżej portu danych 5 z tyłu podstawowego załącznik.

Jeśli ten LED jest miganie, kontroler jest aktywna, a drugi kontroler jest w stanie wstrzymania. Skorzystaj z następujących diagramu i tabeli jako pomoc.

![Tablica urządzenia podstawowego załącznik połączeń z porty danych](./media/storsimple-controller-replacement/IC741055.png)

**Rysunek 8** Tylnej strony podstawowej załącznik z danych porty i protokoły LED monitorowanie

|Etykieta|Opis|
|:----|:----------|
|1-6|DANE 0 – 5 portów sieci|
|7|Niebieski LED|


## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej na temat [Wymiana składników sprzętu StorSimple](storsimple-hardware-component-replacement.md).
