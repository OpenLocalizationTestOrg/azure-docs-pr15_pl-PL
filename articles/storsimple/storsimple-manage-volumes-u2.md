<properties
   pageTitle="Zarządzanie StorSimple przedstawienia wielkości (U2) | Microsoft Azure"
   description="Wyjaśniono, jak dodawanie, modyfikowanie, monitorowanie i usuwanie wielkości StorSimple i robienia je w trybie offline, jeśli to konieczne."
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
   ms.workload="NA"
   ms.date="10/28/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-manage-volumes-update-2"></a>Usługa Menedżera StorSimple umożliwia zarządzanie wielkości (Aktualizuj 2)

[AZURE.INCLUDE [storsimple-version-selector-manage-volumes](../../includes/storsimple-version-selector-manage-volumes.md)]

## <a name="overview"></a>Omówienie

Ten samouczek wyjaśniono, jak korzystać z usługi Menedżera StorSimple tworzyć i zarządzać nimi ilości na urządzeniu StorSimple i urządzenia wirtualnego StorSimple z aktualizacji 2.

Usługa Menedżera StorSimple jest rozszerzeniem w portalu klasyczny Azure, która umożliwia zarządzanie rozwiązania StorSimple w interfejsie jednej sieci web. Oprócz zarządzania wielkości, usługę Menedżer StorSimple służy do tworzenia i zarządzanie usługami StorSimple, wyświetlanie i zarządzanie urządzeniami, wyświetlanie alertów i wyświetlanie i zarządzanie zasadami kopii zapasowej i wykazu kopii zapasowych.

## <a name="volume-types"></a>Typy głośności

Ilości StorSimple może być:

- **Lokalnie przypięte wielkości**: dane te wielkości pozostają na urządzeniem lokalnym StorSimple przez cały czas.
- **Wielkości Tiered**: dane w tych wielkości można rozsypywać w chmurze.

Archiwizacja głośność jest typem wielkości warstwowych. Większy rozmiar segmentu deduplication używana do przedstawienia wielkości archiwizacji umożliwia urządzenie, aby przenieść większe segmentów danych do chmury. 

Jeśli to konieczne, możesz zmienić wielkość typ z lokalnym tiered lub z tiered do lokalnego. Aby uzyskać więcej informacji przejdź do sekcji [Zmienianie typu woluminu](#change-the-volume-type).

### <a name="locally-pinned-volumes"></a>Lokalnie przypięty wielkości

Lokalnie przypięty są w pełni ustanawianie wielkości, które wykonywać nie danych w chmurze, zapewniając lokalne gwarancje dotyczące danych podstawowych, niezależnie od łączności chmury. Dane dotyczące ilości lokalnie przypięty jest deduplicated i nie skompresowany; jednak migawek wielkości lokalnie przypięty są deduplicated. 

W pełni zainicjowano lokalnie przypięty wielkości; w związku z tym musi mieć wystarczającą ilością miejsca na urządzeniu, gdy są tworzone. Umożliwia obsługę lokalnie przypięty wielkości maksymalnie maksymalny rozmiar 8 TB na tym urządzeniu StorSimple 8100 i 20 TB na tym urządzeniu 8600. StorSimple zastrzega sobie pozostałe miejsce lokalne na tym urządzeniu migawek, metadanych i przetwarzania danych. Można zwiększyć rozmiar wielkości lokalnie przypięty do maksymalna miejsca, ale nie można zmniejszyć rozmiar woluminu raz utworzone.

Po utworzeniu woluminu lokalnie przypięty dostępnego miejsca do utworzenia wielkości warstwowych jest ograniczona. Odwrotna obowiązuje również: mając istniejących warstwowych ilości miejsca do tworzenia lokalnie przypięte wielkości będzie niższa od maksymalną podanych powyżej. Aby uzyskać więcej informacji na lokalnym ilości zapoznaj się z [często zadawanych pytań dotyczących lokalnie przypięty wielkości](storsimple-local-volume-faq.md).   

### <a name="tiered-volumes"></a>Wielkości warstwowych

Wielkości warstwowych są znacznie ustanawianie wielkości, w których często używanych danych pozostaje lokalne na tym urządzeniu, a mniej często używanych danych jest automatycznie warstwowa w chmurze. Cienkie inicjowania obsługi administracyjnej jest technologii wirtualizacji, w której znajduje się dostępnego magazynu przekroczyć fizycznie zasobów. Zamiast rezerwowania wystarczającą ilość miejsca z wyprzedzeniem, StorSimple przydzielić tylko za mało miejsca do wymagań bieżącej używa cienkie inicjowania obsługi administracyjnej. Elastyczne rodzaj magazynu w chmurze ułatwia tej metody, ponieważ StorSimple można zwiększyć lub zmniejszyć magazynu w chmurze do spełnienia wymagań zmiany.

Korzystania z wielkość warstwowych archiwizowania danych zaznaczając pole wyboru **Użyj tego woluminu dla mniej często używanych danych archiwizacji** zmienia rozmiar fragmentu deduplication dla głośność 512 KB. Jeśli ta opcja nie jest zaznaczone, odpowiednią objętość warstwowych użyje rozmiarze fragmentu 64 KB. Rozmiar fragmentu deduplication umożliwia urządzenie, aby przyspieszyć transfer dużych archiwizowania danych w chmurze.

>[AZURE.NOTE] Archiwizacja wielkości utworzone za pomocą w wersji 2 sprzed aktualizacji StorSimple zostanie zaimportowany jako warstwowych z zaznaczonym polem wyboru archiwizacji.

### <a name="provisioned-capacity"></a>Ustanawianie wydajność

Zapoznaj się z poniższej tabeli ustanawianie pojemności dla każdego typu urządzenia i obrót. (Należy zauważyć, że lokalnie przypięty ilości nie są dostępne na urządzenie wirtualne).

|             | Maksymalny warstwowych rozmiar woluminu | Przypięte lokalnie maksymalny rozmiar woluminu |
|-------------|----------------------------|------------------------------------|
| **Fizycznymi** |       |       |
| 8100                 | 64 TB | 8 TB |
| 8600                 | 64 TB | 20 TB |
| **Urządzenia wirtualne**  |       |       |
| 8010                | 30 TB | N/D!   |
| 8020               | 64 TB | N/D!   |

## <a name="the-volumes-page"></a>Na stronie wielkości

Na stronie **wielkości** umożliwia zarządzanie ilości miejsca do magazynowania, których zainicjowano obsługę administracyjną na urządzeniu Microsoft Azure StorSimple do swojego Inicjator (serwery). Zostanie wyświetlona lista wielkości na urządzeniu StorSimple.

 ![Strona wielkości](./media/storsimple-manage-volumes-u2/VolumePage.png)

Głośność składa się z serii atrybuty:

- **Nazwa woluminu** — opisową nazwę, która musi być unikatowa i ułatwia identyfikowanie głośność. Ta nazwa jest również używana w monitorowania raportów, gdy możesz filtrować według określonego woluminu.

- **Stan** — może być trybu online lub offline. Jeśli woluminu jest w trybie offline nie jest widoczny dla Inicjator (serwery), które mają dostęp do korzystać z woluminu.

- **Wydajność** — określa całkowitą ilość danych, które mogą być przechowywane przez inicjator (server). Przypięte lokalnie wielkości pełni zainicjowano obsługę administracyjną i znajdują się na tym urządzeniu StorSimple. Znacznie zainicjowano warstwowych wielkości i danych jest deduplicated. Z znacznie ustanawianie wielkości urządzenia nie wstępnie przydzielić fizycznie pojemność wewnętrznie lub w chmurze według pojemność woluminu skonfigurowane. Pojemność woluminu jest przydzielona i zużyte na żądanie.

- **Typ** — wskazuje, czy głośność jest **Tiered** (ustawienie domyślne) lub **lokalnie przypięte**.

- **Kopii zapasowej** — wskazuje, czy domyślną zasadę kopii zapasowej istnieje woluminu.

- **Access** — umożliwia określenie Inicjator (serwery), które mają dostęp do tego woluminu. Inicjator, które nie są członkami rekordu kontroli dostępu (awaryjnego), który jest skojarzony z wielkość nie zobaczą głośność.

- **Monitorowanie** — Określa, czy jest monitorowane woluminu. Głośność uzyskuje monitorowania domyślnie po jego utworzeniu. Monitorowanie będzie, jednak, zostanie wyłączona na sklonuj głośność. Aby włączyć monitorowanie woluminu, postępuj zgodnie z instrukcjami w [monitorze woluminu](#monitor-a-volume). 

Skorzystaj z instrukcji w tym samouczku umożliwia wykonywanie następujących zadań:

- Dodawanie głośności 
- Modyfikowanie głośności 
- Zmienianie typu głośności
- Usuwanie woluminu 
- Przełączanie woluminu do trybu offline 
- Monitorowanie woluminu 

## <a name="add-a-volume"></a>Dodawanie głośności

Podczas wdrażania rozwiązania StorSimple [utworzone woluminu](storsimple-deployment-walkthrough-u2.md#step-6-create-a-volume) . Dodawanie woluminu jest podobnej procedury.

#### <a name="to-add-a-volume"></a>Aby dodać woluminu

1. Na stronie **urządzeń** wybierz urządzenie, kliknij go dwukrotnie, a następnie kliknij kartę **Kontenerów głośność** .

2. Z listy wybierz kontenera głośności i kliknij dwukrotnie, aby uzyskać dostęp do wielkość skojarzone z tym kontenerem.

3. Kliknij przycisk **Dodaj** w dolnej części strony. Dodaj zostanie uruchomiony Kreator głośność.

     ![Dodawanie kreatora głośność podstawowych ustawień](./media/storsimple-manage-volumes-u2/TieredVolEx.png)

4. W oknie Dodawanie kreatora głośność, w obszarze **Ustawienia podstawowe**wykonaj następujące czynności:

  1. Wprowadź **nazwę** dla głośność.
  2. Wybierz **Typ użycia** z listy rozwijanej. Dla obciążenia, które wymagają dane mają być dostępne lokalnie na urządzeniu przez cały czas, wybierz pozycję **Przypięte lokalnie**. Dla wszystkich innych typów danych wybierz pozycję **Tiered**. (**Tiered** to ustawienie domyślne).
  3. Jeśli wybrano **Tiered** w kroku 2, można zaznacz pole wyboru **korzystać z tego woluminu dla mniej często używanych archiwizowania danych** , skonfigurowanie archiwizacji głośność.
  4. Wprowadź **Możliwości obsługi administracyjnej** głośność GB lub TB. Zobacz [Wydajność Provisioned](#provisioned-capacity) dla maksymalne rozmiary dla każdego typu urządzenia i obrót. Przyjrzyj się **Pojemność** do określenia, ile miejsca do magazynowania faktycznie znajduje się na urządzeniu.

5. Kliknij ikonę strzałki![Ikona strzałki](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png). Jeśli konfigurujesz lokalnie przypięty głośność, zostanie wyświetlony poniższy komunikat.

    ![Zmienianie głośności typ wiadomości](./media/storsimple-manage-volumes-u2/LocalVolEx.png)
   
5. Kliknij ikonę strzałki ![ikonę strzałki](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png)ponownie, aby przejść do strony **Dodatkowe ustawienia** .

    ![Dodawanie głośności kreatora dodatkowe ustawienia](./media/storsimple-manage-volumes-u2/AddVolume2.png)<br>

6. W obszarze **Dodatkowe ustawienia**Dodaj nowy rekord kontroli dostępu (awaryjnego):
  
  1. Wybierz rekord kontroli dostępu (awaryjnego) z listy rozwijanej. Alternatywnie możesz dodać nowe awaryjnego. ACRs określają, które hosty ma dostęp do swojej wielkości dopasowując hosta IQN z znajdującego się w rekordzie. Jeśli nie określisz awaryjnego, zostanie wyświetlony poniższy komunikat.

        ![Określanie awaryjnego](./media/storsimple-manage-volumes-u2/SpecifyACR.png)

  2. Zaleca się, że zaznaczono pole wyboru **Włącz kopii zapasowej domyślnych dla tego woluminu** .
  3. Kliknij ikonę wyboru ![Ikona modułu sprawdzania](./media/storsimple-manage-volumes-u2/HCS_CheckIcon.png) Aby utworzyć wielkość z określonymi ustawieniami.

Nowe głośność jest teraz gotowy do użycia.

>[AZURE.NOTE] Tworzenie lokalnie przypięty głośności, a następnie utworzenia innej lokalnie przypięta głośność natychmiast po Tworzenie woluminu, które zadania wykonywane po kolei. Pierwsze zadanie tworzenia głośność musi zakończyć się przed rozpoczęciem następnego zadania tworzenia głośność.

## <a name="modify-a-volume"></a>Modyfikowanie głośności

Modyfikowanie woluminu, gdy trzeba ją rozwinąć lub zmienianie hostów, które dostęp do woluminu.

> [AZURE.IMPORTANT] 
>
> - Zmodyfikowanie rozmiar woluminu na tym urządzeniu rozmiar woluminu trzeba zmienić na hoście także. 
> - Po stronie hosta kroki opisane w tym miejscu są dla systemu Windows Server 2012 (2012R2). Procedury Linux lub inne systemy operacyjne hosta może się różnić. Zapoznaj się z instrukcje systemu operacyjnego hosta, modyfikując wielkość na hoście innego systemu operacyjnego. 

#### <a name="to-modify-a-volume"></a>Aby zmodyfikować woluminu

1. Na stronie **urządzeń** wybierz urządzenie, kliknij go dwukrotnie, a następnie kliknij kartę **Kontenerów głośność** .

2. Z listy wybierz kontenera głośności i kliknij dwukrotnie, aby wyświetlić wielkość skojarzone z tym kontenerem.

3. Zaznacz woluminu, a u dołu strony kliknij przycisk **Modyfikuj**. Zostanie uruchomiony Kreator głośność Modyfikuj.

4. W Kreatorze woluminu Modyfikuj w obszarze **Podstawowych ustawień**można wykonaj następujące czynności:

  - Zmień **nazwę**.
  - Konwertowanie **Typu zastosowania** z lokalnie przypięta do tiered lub z tiered do lokalnie przypięte (zobacz [Zmienianie typu woluminu](#change-the-volume-type) , aby uzyskać więcej informacji).
  - Zwiększanie **obsługi administracyjnej wydajność**. Można tylko zwiększyć **Wydajność obsługi administracyjnej** . Nie można zmniejszyć objętość, po jego utworzeniu.

5. W obszarze **Dodatkowe ustawienia**możesz zmienić awaryjnego, pod warunkiem, że głośność jest w trybie offline. Głośność jest w trybie online, należy przejść do trybu offline najpierw. Zapoznaj się z instrukcjami w [pobrać próbkę w trybie offline](#take-a-volume-offline) przed modyfikowaniem awaryjnego.

    > [AZURE.NOTE] Nie można zmienić opcję **Włącz kopii zapasowej domyślnych** dla woluminu.

6. Zapisać zmiany, klikając ikonę wyboru ![Ikona wyboru](./media/storsimple-manage-volumes-u2/HCS_CheckIcon.png). Azure portalu klasycznym wyświetli komunikat aktualizowania głośność. Wyświetli komunikat o powodzeniu po wielkość została zaktualizowana.

7. Jeśli są rozwijanie woluminu, wykonaj następujące czynności na komputerze z systemem Windows hosta:

   1. Przejdź do pozycji **Zarządzanie komputerem** ->**zarządzania dysku**.
   2. Kliknij prawym przyciskiem myszy **Zarządzania dysku** i wybierz pozycję **Skanuj dyski**.
   3. Na liście dysków zaznacz głośność zaktualizowany, kliknij prawym przyciskiem myszy, a następnie wybierz pozycję **Rozszerzanie głośność**. Zostanie uruchomiony Kreator rozszerzanie głośność. Kliknij przycisk **Dalej**.
   4. Kończenie pracy kreatora, akceptując wartości domyślne. Po zakończeniu działania kreatora wielkość powinny być wyświetlane zwiększenie rozmiaru.

    >[AZURE.NOTE] Jeśli rozwiń lokalnie przypięty głośności, a następnie rozwiń przypięta innego lokalnie głośność natychmiast po zadania rozszerzeń głośność uruchamiane kolejno. Pierwsze zadanie rozszerzeń głośność musi zakończyć się przed rozpoczęciem następnego zadania rozszerzeń głośność.

![Klip wideo dostępne](./media/storsimple-manage-volumes-u2/Video_icon.png) **wideo dostępne**

Aby obejrzeć klip wideo, który przedstawia sposób rozwinąć woluminu, kliknij [tutaj](https://azure.microsoft.com/documentation/videos/expand-a-storsimple-volume/).

## <a name="change-the-volume-type"></a>Zmienianie typu głośności

Można zmienić głośność z tiered do lokalnie przypięte lub z lokalnie przypięta do warstwowych. Konwersja nie należy jednak częste wystąpienie. Powody do konwersji woluminu z tiered do lokalnie przypięte są:

- Lokalne gwarancje wydajność i dostępność danych
- Odrzucenie opóźnienia w chmurze i problemy z łącznością chmury.

Zazwyczaj są to małe istniejącego woluminu, które mają być często. Lokalnie przypięty głośność jest w pełni obsługi administracyjnej po jego utworzeniu. Jeśli głośność warstwowych są konwertowane na lokalnie przypięty głośność, StorSimple sprawdza, czy masz wystarczającą ilością miejsca na urządzeniu z systemem przed rozpoczęciem konwersji. Jeśli masz za mało miejsca, zostanie wyświetlony komunikat o błędzie, a operacja zostanie anulowana. 

> [AZURE.NOTE] Przed rozpoczęciem konwersja tiered do lokalnie przypięte, upewnij się, należy rozważyć wymagania dotyczące miejsca na swojej obciążeń pracą. 

Można zmienić głośność lokalnie przypięty do woluminu warstwowych, jeśli potrzebujesz więcej miejsca do zapewniania obsługi innych wielkości. Podczas konwertowania lokalnie przypięty woluminu tiered pojemność zwiększenia urządzenia rozmiarem możliwości wydaną. Jeśli problemy z łącznością zapobiec konwersji woluminu z lokalnym typu warstwowych typu, wielkość lokalne działać właściwości woluminu warstwowych, dopóki nie zakończy się konwersji. Jest to ponieważ niektóre dane mogą mieć rozrzucone w chmurze. Rozlanej danych będzie zajmować lokalnej przestrzeni na tym urządzeniu, który nie jest możliwe do czasu tej operacji jest ponowne uruchomienie i wykonane.

>[AZURE.NOTE] Konwertowanie woluminu może zająć trochę czasu, a nie można anulować konwersji po jego uruchomieniu. Głośność pozostaje w trybie online podczas konwersji i, które można wykonywać kopie zapasowe, ale nie można rozwinąć lub przywrócić głośność, podczas konwersji odbywa się.  

Konwersja warstwowych na lokalnie przypięty głośność negatywnie wpływają na wydajność urządzenia. Ponadto następujące czynniki może wydłużyć czas potrzebny do wykonania konwersji:

- Jest niewystarczająca przepustowość.

- Nie ma żadnych bieżącej kopii zapasowej.

Aby zminimalizować efektów, które mogą mieć następujących czynników:

- Przejrzyj zasady z przepustowości i upewnij się, że dedykowane 40 przepustowości MB/s jest dostępna.
- Planowanie konwersji mniejszego.
- Zrób migawkę chmurze przed rozpoczęciem konwersji.

Jest konwertowany przypisaną (obsługi różnych obciążenia), następnie możesz należy priorytet konwersji woluminu tak, aby większej ilości priorytet są konwertowane najpierw. Na przykład należy przekonwertować wielkości, które obsługują wirtualnych maszyn lub wielkości z obciążeń pracą SQL przed przekonwertowaniem wielkości z obciążeń pracą udostępnianie plików.

#### <a name="to-change-the-volume-type"></a>Aby zmienić typ głośności

1. Na stronie **urządzeń** wybierz urządzenie, kliknij go dwukrotnie, a następnie kliknij kartę **Kontenerów głośność** .

2. Z listy wybierz kontenera głośności i kliknij dwukrotnie, aby wyświetlić wielkość skojarzone z tym kontenerem.

3. Zaznacz woluminu, a u dołu strony kliknij przycisk **Modyfikuj**. Zostanie uruchomiony Kreator głośność Modyfikuj.

4. Na stronie **Ustawienia podstawowe** zmienić typ użycia, wybierając nowy typ z listy rozwijanej **Typ użycia** .

    - W przypadku zmiany typu do **lokalnie przypięte**, StorSimple sprawdza, czy jest wystarczającej.
    - Jeśli chcesz zmienić typ do **Tiered** i tego woluminu będą używane do archiwizacji danych, zaznacz pole wyboru **korzystać z tego woluminu dla mniej często używanych archiwizowania danych** .

        ![Pole wyboru archiwum](./media/storsimple-manage-volumes-u2/ModifyTieredVolEx.png)

5. Kliknij ikonę strzałki ![ikonę strzałki](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png) przejdź na stronę **Dodatkowe ustawienia** . Jeśli konfigurujesz lokalnie przypięty głośność, zostanie wyświetlony następujący komunikat.

    ![Zmienianie głośności typ wiadomości](./media/storsimple-manage-volumes-u2/ModifyLocalVolEx.png)

6. Kliknij ikonę strzałki ![Ikona strzałki](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png) ponownie, aby kontynuować.

7. Kliknij ikonę wyboru ![Ikona modułu sprawdzania](./media/storsimple-manage-volumes-u2/HCS_CheckIcon.png) Aby rozpocząć proces konwersji. Azure portal wyświetli komunikat Zbiorcza aktualizacja. Wyświetli komunikat o powodzeniu po wielkość została zaktualizowana.

## <a name="take-a-volume-offline"></a>Przełączanie woluminu do trybu offline

Konieczne może być w trybie offline pobrać próbkę podczas planowania modyfikować lub go usunąć. Gdy woluminu jest w trybie offline nie jest dostępna do odczytu i zapisu. Należy wykonać wielkość w trybie offline na hoście, a także na tym urządzeniu. 

#### <a name="to-take-a-volume-offline"></a>Aby pobrać próbkę w trybie offline

1. Upewnij się, że wielkość w danym nie jest używany przed podjęciem go w trybie offline.

2. Sporządzanie pierwszego wielkość w trybie offline na hoście. Dzięki temu ryzyka uszkodzenia danych na wielkość. Informacje dotyczące określonych kroków zapoznaj się z instrukcjami dla używanego systemu operacyjnego hosta.

3. Po hoście jest w trybie offline, należy wykonać wielkość na urządzeniu w trybie offline, wykonując następujące czynności:

  1. Na stronie **urządzeń** wybierz urządzenie, kliknij go dwukrotnie, a następnie kliknij kartę **Kontenerów głośność** . Na karcie **Kontenerów głośność** wymienione w formacie tabelarycznym wszystkich kontenerów głośność, które są skojarzone z urządzeniem.
  2. Zaznacz kontenera głośności, a następnie kliknij go, aby wyświetlić listę wszystkich wielkości w kontenerze.
  3. Wybierz pozycję woluminu, a następnie kliknij przycisk **tryb offline**.
  4. Po wyświetleniu monitu o potwierdzenie, kliknij przycisk **Tak**. Głośność powinien być teraz w trybie offline.

    Po woluminu jest w trybie offline, **Przejdź do trybu Online** opcja staje się dostępna.

> [AZURE.NOTE] Polecenie **Przełącz do trybu Offline** przesyła żądanie na urządzeniu, aby przełączyć do trybu offline głośność. Jeśli hosts jest nadal używana głośność, powoduje zerwane połączenia, ale trwa głośności w trybie offline nie powiedzie się. 

## <a name="delete-a-volume"></a>Usuwanie woluminu

> [AZURE.IMPORTANT] Głośność możesz usunąć tylko wtedy, gdy jest w trybie offline.

Wykonaj poniższe czynności, aby usunąć woluminu.

#### <a name="to-delete-a-volume"></a>Aby usunąć woluminu

1. Na stronie **urządzeń** wybierz urządzenie, kliknij go dwukrotnie, a następnie kliknij kartę **Kontenerów głośność** .

2. Wybierz kontener głośność, zawierającą woluminu, który chcesz usunąć. Kliknij kontener głośność uzyskać dostęp do strony **wielkości** .

3. Wszystkie skojarzone z tym kontenerem wielkości są wyświetlane w formacie tabelarycznym. Sprawdź stan woluminu, który chcesz usunąć. Jeśli głośność, które chcesz usunąć, nie jest w trybie offline, przejść do trybu offline, wykonując kroki opisane w [pobrać próbkę w trybie offline](#take-a-volume-offline).

4. Po głośność jest w trybie offline, kliknij przycisk **Usuń** w dolnej części strony.

5. Po wyświetleniu monitu o potwierdzenie, kliknij przycisk **Tak**. Głośność zostanie usunięte, a następnie na stronie **wielkości** zostanie wyświetlona na liście zaktualizowanych wielkości w kontenerze.

    >[AZURE.NOTE]Jeśli usuniesz lokalnie przypięty głośność, miejsca dla nowych wielkości mogą nie zostać zaktualizowane natychmiast. Usługa Menedżera StorSimple okresowo aktualizuje lokalne miejsca. Sugerujemy zapoznawanie Poczekaj kilka minut, zanim spróbujesz utworzyć nowy.<br> Ponadto usunięcia lokalnie przypięty głośności, a następnie usuń innego lokalnie przypięta głośność natychmiast po usunięcia głośność, które zadania wykonywane po kolei. Pierwsze zadanie usuwania głośność musi zakończyć się przed rozpoczęciem następnego zadania usuwania głośność.
 
## <a name="monitor-a-volume"></a>Monitorowanie woluminu

Monitorowanie głośności umożliwia zbieranie I/O związane z statystyki dla woluminu. Monitorowanie jest włączone domyślnie wielkość pierwszych 32 tworzonych przez Ciebie. Monitorowanie wielkości dodatkowe jest domyślnie wyłączona. Monitorowanie wielkości sklonowanym jest domyślnie wyłączona.

Wykonaj poniższe czynności, aby włączyć lub wyłączyć monitorowania dla woluminu.

#### <a name="to-enable-or-disable-volume-monitoring"></a>Aby włączyć lub wyłączyć monitorowania głośności

1. Na stronie **urządzeń** wybierz urządzenie, kliknij go dwukrotnie, a następnie kliknij kartę **Kontenerów głośność** .

2. Wybierz kontener głośność, w której znajduje się głośności, a następnie kliknij kontener głośność uzyskać dostęp do strony **wielkości** .

3. Wszystkie skojarzone z tym kontenerem wielkości są widoczne w tabelarycznym wyświetlania. Kliknij i wybierz głośność lub klonowanie głośność.

4. U dołu strony kliknij przycisk **Modyfikuj**.

5. W Kreatorze modyfikowanie głośności w obszarze **Ustawienia podstawowe**wybierz z listy rozwijanej **monitorowania** **włączyć** lub **wyłączyć** .

## <a name="next-steps"></a>Następne kroki

- Dowiedz się, jak [klonowanie głośność StorSimple](storsimple-clone-volume.md).

- Dowiedz się, jak [używać usługi Menedżera StorSimple administrowania urządzenia StorSimple](storsimple-manager-service-administration.md).

 
