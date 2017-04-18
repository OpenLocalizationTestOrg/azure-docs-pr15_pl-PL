<properties
   pageTitle="Zarządzanie z ilości StorSimple | Microsoft Azure"
   description="Wyjaśniono, jak dodawanie, modyfikowanie, monitorowanie i usuwanie wielkości StorSimple i robienia je w trybie offline, jeśli to konieczne."
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
   ms.date="05/11/2016"
   ms.author="v-sharos" />

# <a name="use-the-storsimple-manager-service-to-manage-volumes"></a>Zarządzanie wielkości przy użyciu usługi Menedżera StorSimple

[AZURE.INCLUDE [storsimple-version-selector-manage-volumes](../../includes/storsimple-version-selector-manage-volumes.md)]

## <a name="overview"></a>Omówienie

Ten samouczek wyjaśniono, jak tworzyć i zarządzać nimi ilości na urządzeniu StorSimple i urządzenia wirtualnego StorSimple za pomocą usługi Menedżera StorSimple.

Usługa Menedżera StorSimple to rozszerzenie portalu klasyczny Azure, która umożliwia zarządzanie rozwiązania StorSimple w interfejsie jednej sieci web. Oprócz zarządzania wielkości, usługę Menedżer StorSimple służy do tworzenia i zarządzanie usługami StorSimple, wyświetlanie i zarządzanie urządzeniami, wyświetlanie alertów i wyświetlanie i zarządzanie zasadami kopii zapasowej i wykazu kopii zapasowych.

> [AZURE.NOTE] Azure StorSimple można utworzyć tylko znacznie ustanawianie wielkości. Nie można utworzyć ustanawianie w pełni lub częściowo ustanawianie wielkości w systemie Azure StorSimple.
>
> Cienkie inicjowania obsługi administracyjnej jest technologii wirtualizacji, w której znajduje się dostępnego magazynu przekroczyć fizycznie zasobów. Zamiast rezerwowania wystarczającą ilość miejsca z wyprzedzeniem, Azure StorSimple przydzielić tylko za mało miejsca do wymagań bieżącej używa cienkie inicjowania obsługi administracyjnej. Elastyczne rodzaj magazynu w chmurze ułatwia tej metody, ponieważ Azure StorSimple można zwiększyć lub zmniejszyć magazynu w chmurze do spełnienia wymagań zmiany.

## <a name="the-volumes-page"></a>Na stronie wielkości

Na stronie **wielkości** umożliwia zarządzanie ilości miejsca do magazynowania, których zainicjowano obsługę administracyjną na urządzeniu Microsoft Azure StorSimple do swojego Inicjator (serwery). Zostanie wyświetlona lista wielkości na urządzeniu StorSimple.

 ![Strona wielkości](./media/storsimple-manage-volumes/HCS_VolumesPage.png)

Głośność składa się z serii atrybuty:

- **Nazwa** — opisowa nazwa musi być unikatowa, która pomoże zidentyfikować woluminu. Ta nazwa jest również używana w monitorowania raportów, gdy możesz filtrować według określonego woluminu.

- **Stan** — może być trybu online lub offline. Jeśli woluminu, jeśli jest niedostępny, nie jest widoczny dla Inicjator (serwery), które mają dostęp do korzystać z woluminu.

- **Wydajność** — Określa jak duże jest głośność, traktowany przez inicjator (server). Wydajność określa całkowitą ilość danych, które mogą być przechowywane przez inicjator (server). Znacznie zainicjowano wielkości i danych jest deduplicated. Oznacza to, że urządzenie nie wstępnie przydzielić fizycznie pojemność, wewnętrznie lub w chmurze według pojemność woluminu skonfigurowane. Pojemność woluminu jest przydzielona i zużyte na żądanie.

- **Typ** — typ woluminu może być warstwowych lub archiwizacji (podtyp tiered)

- **Access** — umożliwia określenie Inicjator (serwery), które mają dostęp do tego woluminu. Inicjator, które nie są członkami rekordu kontroli dostępu (awaryjnego), który jest skojarzony z wielkość nie zobaczą głośność.

- **Monitorowanie** — Określa, czy jest monitorowane woluminu. Głośność uzyskuje monitorowania domyślnie po jego utworzeniu. Monitorowanie będzie, jednak, zostanie wyłączona na sklonuj głośność. Aby włączyć monitorowanie woluminu, postępuj zgodnie z instrukcjami w monitorze woluminu.

Są najbardziej typowych zadań związanych z woluminu:

- Dodawanie głośności
- Modyfikowanie głośności
- Usuwanie woluminu
- Przełączanie woluminu do trybu offline
- Monitorowanie woluminu

## <a name="add-a-volume"></a>Dodawanie głośności

Podczas wdrażania rozwiązania StorSimple [utworzone woluminu](storsimple-deployment-walkthrough-u1.md#step-6-create-a-volume) . Dodawanie woluminu jest podobnej procedury.

### <a name="to-add-a-volume"></a>Aby dodać woluminu

1. Na stronie **urządzeń** wybierz urządzenie, kliknij go dwukrotnie, a następnie kliknij kartę **Kontenerów głośność** .

2. Zaznacz kontenera głośności, a następnie kliknij strzałkę w powiązanym wierszu, aby uzyskać dostęp do wielkość skojarzone z tym kontenerem.

3. Kliknij przycisk **Dodaj** w dolnej części strony. Dodaj zostanie uruchomiony Kreator głośność.

     ![Dodawanie kreatora głośność podstawowych ustawień](./media/storsimple-manage-volumes/AddVolume1.png)

4. W oknie Dodawanie kreatora głośność, w obszarze **Ustawienia podstawowe**wykonaj następujące czynności:

  1. Wprowadź **nazwę** dla głośność.
  2. Określ **Możliwości obsługi administracyjnej** głośność w GB lub TB. Wydajność musi być między 1 GB i 64 TB dla urządzenia fizycznego. Maksymalna pojemność, który można zainicjowany dla woluminu na urządzenie wirtualne StorSimple jest 30 TB.
  3. Wybierz **Typ użycia** głośność. Korzystania z wielkość warstwowych archiwizowania danych zaznaczając pole wyboru **Użyj tego woluminu dla mniej często używanych danych archiwizacji** zmienia rozmiar fragmentu deduplication dla głośność 512 KB. Jeśli ta opcja nie jest zaznaczone, odpowiednią objętość warstwowych użyje rozmiarze fragmentu 64 KB. Rozmiar fragmentu deduplication umożliwia urządzenie, aby przyspieszyć transfer dużych archiwizowania danych w chmurze. (Warstwowych ilości były wcześniej nazywane wielkości podstawowego).
  5. Kliknij ikonę strzałki ![ikonę strzałki](./media/storsimple-manage-volumes/HCS_ArrowIcon.png)przejdź na stronę **Dodatkowe ustawienia** .

        ![Dodawanie głośności kreatora dodatkowe ustawienia](./media/storsimple-manage-volumes/AddVolume2.png)

5. W obszarze **Dodatkowe ustawienia**Dodaj nowy rekord kontroli dostępu (awaryjnego):

  1. Wybierz rekord kontroli dostępu (awaryjnego) z listy rozwijanej. Alternatywnie możesz dodać nowe awaryjnego. ACRs określają, które hosty ma dostęp do swojej wielkości dopasowując hosta IQN z znajdującego się w rekordzie.
  2. Zaleca się włączenie kopii zapasowej domyślne, zaznaczając pole wyboru **Włącz kopii zapasowej domyślnych dla tego woluminu** .
   3. Kliknij ikonę wyboru ![Ikona modułu sprawdzania](./media/storsimple-manage-volumes/HCS_CheckIcon.png) Aby utworzyć wielkość z określonymi ustawieniami.

Nowe głośność jest teraz gotowy do użycia.

## <a name="modify-a-volume"></a>Modyfikowanie głośności

Modyfikowanie woluminu, gdy trzeba ją rozwinąć lub zmienianie hostów, które dostęp do woluminu.

> [AZURE.IMPORTANT]
>
> - Zmodyfikowanie rozmiar woluminu na tym urządzeniu rozmiar woluminu trzeba zmienić na hoście także.
> - Po stronie hosta kroki opisane w tym miejscu są dla systemu Windows Server 2012 (2012R2). Procedury Linux lub inne systemy operacyjne hosta może się różnić. Zapoznaj się z instrukcje systemu operacyjnego hosta, modyfikując wielkość na hoście innego systemu operacyjnego.

### <a name="to-modify-a-volume"></a>Aby zmodyfikować woluminu

1. Na stronie **urządzeń** wybierz urządzenie, kliknij go dwukrotnie, a następnie kliknij kartę **Kontenera głośność** . Ta strona zawiera listę w formacie tabelarycznym wszystkich kontenerów głośność, które są skojarzone z urządzeniem.

2. Zaznacz kontenera głośności, a następnie kliknij go, aby wyświetlić listę wszystkich wielkości w kontenerze.

3. Na stronie **wielkości** wybierz woluminu, a następnie kliknij polecenie **Modyfikuj**.

4. W Kreatorze woluminu Modyfikuj w obszarze **Podstawowych ustawień**można wykonaj następujące czynności:

  - Edytowanie **Nazwa** i **Typ** , jeśli chcesz zmodyfikować warstwowych głośności do archiwizacji kreski, zaznaczając pole wyboru **Użyj tego woluminu dla mniej często używanych archiwizowania danych** , aby zmienić rozmiar segmentu deduplication dla głośność 512 KB.
  - Zwiększanie **obsługi administracyjnej wydajność**. Można tylko zwiększyć **Wydajność obsługi administracyjnej** . Nie można zmniejszyć objętość, po jego utworzeniu.

    > [AZURE.NOTE] Po przypisaniu go do woluminu, nie można zmienić kontenerze głośność.

5. W obszarze **Dodatkowe ustawienia**możesz wykonać następujące czynności:

  - Modyfikowanie ACRs, pod warunkiem, że głośność jest w trybie offline. Głośność jest w trybie online, należy przejść do trybu offline najpierw. Zapoznaj się z instrukcjami w [pobrać próbkę w trybie offline](#take-a-volume-offline) przed modyfikowaniem awaryjnego.
  - Modyfikowanie listy ACRs po głośność jest w trybie offline.

    > [AZURE.NOTE] Nie można zmienić opcję **Włącz kopii zapasowej domyślnych dla tego woluminu** dla woluminu.

6. Zapisać zmiany, klikając ikonę wyboru ![Ikona wyboru](./media/storsimple-manage-volumes/HCS_CheckIcon.png). Azure portalu klasycznym wyświetli komunikat aktualizowania głośność. Wyświetli komunikat o powodzeniu po wielkość została zaktualizowana.

7. Jeśli są rozwijanie woluminu, wykonaj następujące czynności na komputerze z systemem Windows hosta:

   1. Przejdź do pozycji **Zarządzanie komputerem** ->**zarządzania dysku**.
   2. Kliknij prawym przyciskiem myszy **Zarządzania dysku** i wybierz pozycję **Skanuj dyski**.
   3. Na liście dysków zaznacz głośność zaktualizowany, kliknij prawym przyciskiem myszy, a następnie wybierz pozycję **Rozszerzanie głośność**. Zostanie uruchomiony Kreator rozszerzanie głośność. Kliknij przycisk **Dalej**.
   4. Kończenie pracy kreatora, akceptując wartości domyślne. Po zakończeniu działania kreatora wielkość powinny być wyświetlane zwiększenie rozmiaru.

![Klip wideo dostępne](./media/storsimple-manage-volumes/Video_icon.png) **wideo dostępne**

Aby obejrzeć klip wideo, który przedstawia sposób rozwinąć woluminu, kliknij [tutaj](https://azure.microsoft.com/documentation/videos/expand-a-storsimple-volume/).

## <a name="take-a-volume-offline"></a>Przełączanie woluminu do trybu offline

Konieczne może być w trybie offline pobrać próbkę podczas planowania modyfikować lub go usunąć. Gdy woluminu jest w trybie offline nie jest dostępna do odczytu i zapisu. Należy wykonać wielkość w trybie offline na hoście, a także na tym urządzeniu. Wykonaj poniższe czynności, aby pobrać próbkę w trybie offline.

### <a name="to-take-a-volume-offline"></a>Aby pobrać próbkę w trybie offline

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

### <a name="to-delete-a-volume"></a>Aby usunąć woluminu

1. Na stronie **urządzeń** wybierz urządzenie, kliknij go dwukrotnie, a następnie kliknij kartę **Kontenerów głośność** .

2. Wybierz kontener głośność, zawierającą woluminu, który chcesz usunąć. Kliknij kontener głośność uzyskać dostęp do strony **wielkości** .

3. Wszystkie skojarzone z tym kontenerem wielkości są wyświetlane w formacie tabelarycznym. Sprawdź stan woluminu, który chcesz usunąć. Jeśli głośność, które chcesz usunąć, nie jest w trybie offline, przejść do trybu offline, wykonując kroki opisane w [pobrać próbkę w trybie offline](#take-a-volume-offline).

4. Po głośność jest w trybie offline, kliknij przycisk **Usuń** w dolnej części strony.

5. Po wyświetleniu monitu o potwierdzenie, kliknij przycisk **Tak**. Głośność zostanie usunięte, a następnie na stronie **wielkości** zostanie wyświetlona na liście zaktualizowanych wielkości w kontenerze.

## <a name="monitor-a-volume"></a>Monitorowanie woluminu

Monitorowanie głośności umożliwia zbieranie I/O związane z statystyki dla woluminu. Monitorowanie jest włączone domyślnie wielkość pierwszych 32 tworzonych przez Ciebie. Monitorowanie wielkości dodatkowe jest domyślnie wyłączona. Monitorowanie wielkości sklonowanym jest domyślnie wyłączona.

Wykonaj poniższe czynności, aby włączyć lub wyłączyć monitorowania dla woluminu.

### <a name="to-enable-or-disable-volume-monitoring"></a>Aby włączyć lub wyłączyć monitorowania głośności

1. Na stronie **urządzeń** wybierz urządzenie, kliknij go dwukrotnie, a następnie kliknij kartę **Kontenerów głośność** .

2. Wybierz kontener głośność, w której znajduje się głośności, a następnie kliknij kontener głośność uzyskać dostęp do strony **wielkości** .

3. Wszystkie skojarzone z tym kontenerem wielkości są widoczne w tabelarycznym wyświetlania. Kliknij i wybierz głośność lub klonowanie głośność.

4. U dołu strony kliknij przycisk **Modyfikuj**.

5. W Kreatorze modyfikowanie głośności w obszarze **Ustawienia podstawowe**wybierz z listy rozwijanej **monitorowania** **włączyć** lub **wyłączyć** .

    ![Modyfikowanie głośności podstawowych ustawień](./media/storsimple-manage-volumes/HCS_MonitorVolumeM.png)


## <a name="next-steps"></a>Następne kroki

- Dowiedz się, jak [klonowanie głośność StorSimple](storsimple-clone-volume.md).

- Dowiedz się, jak [używać usługi Menedżera StorSimple administrowania urządzenia StorSimple](storsimple-manager-service-administration.md).
