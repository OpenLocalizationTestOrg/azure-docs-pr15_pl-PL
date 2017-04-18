<properties
   pageTitle="Klonowanie głośność StorSimple | Microsoft Azure"
   description="W tym artykule opisano klonowanie różnych typów i kiedy ich używać i wyjaśniono, jak korzystać z zestawu na klonowanie poszczególnych głośność kopii zapasowych."
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
   ms.date="07/27/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-clone-a-volume-update-2"></a>Klonowanie zbiorcza (Aktualizuj 2) za pomocą usługi Menedżera StorSimple

[AZURE.INCLUDE [storsimple-version-selector-clone-volume](../../includes/storsimple-version-selector-clone-volume.md)]

## <a name="overview"></a>Omówienie

Na stronie **Katalog kopii zapasowej** usługi Menedżera StorSimple są wyświetlane wszystkie zestawy kopii zapasowych, które zostały utworzone podczas ręcznego lub automatycznego wykonywania kopii zapasowych. Ta strona umożliwia listę wszystkich kopii zapasowych zasady tworzenia kopii zapasowych lub obrót, zaznacz lub usuń kopie zapasowe lub użyć kopii zapasowej do przywrócenia lub klonowanie woluminu.

![Strony wykazu kopii zapasowych](./media/storsimple-clone-volume-u2/backupCatalog.png)  

Ten samouczek zawiera opis sposobu używania zestawu na klonowanie poszczególnych głośność kopii zapasowych. Objaśniono także różnica między klony *przejściowych* i *stałe* .

>[AZURE.NOTE] 
>
>Głośność lokalnie przypięty będzie klonowany jako warstwowych głośność. W razie potrzeby sklonowanym woluminu, który ma być przypięta lokalnie, możesz przekonwertować klonowanie lokalnie przypięty głośność po pomyślnym zakończeniu operacji klonowanie. Aby dowiedzieć się, jak konwertowania woluminu warstwowych lokalnie przypięty głośność przejdź do sekcji [Zmienianie typu woluminu](storsimple-manage-volumes-u2.md#change-the-volume-type).
>
>Przy próbie konwertowanie sklonowanym woluminu z tiered do lokalnie przypięte zaraz po klonowanie (gdy jest nadal przejściowych klonowanie), konwersja zakończy się niepowodzeniem z komunikatem o błędzie:
>
>`Unable to modify the usage type for volume {0}. This can happen if the volume being modified is a transient clone and hasn’t been made permanent. Take a cloud snapshot of this volume and then retry the modify operation.` 
>
>To jest błąd tylko wtedy, gdy są klonowanie do innego urządzenia. Możesz pomyślnie przekonwertować woluminu przypięte lokalnie, jeśli najpierw zostanie przekonwertowany na sklonuj przejściowych trwały klonowanie. Aby przekonwertować przejściowych klonowanie trwały klonowanie, zrób migawkę chmury go.

## <a name="create-a-clone-of-a-volume"></a>Tworzenie klonowanie woluminu

Na tym samym urządzeniu, inne urządzenie lub nawet maszyny wirtualnej można utworzyć klonowanie, przy użyciu lokalnie lub migawki w chmurze.

#### <a name="to-clone-a-volume"></a>Aby klonowanie woluminu

1. Na stronie usługi Menedżera StorSimple kliknij kartę **wykazu kopii zapasowej** , a następnie wybierz zestaw kopii zapasowych.

2. Rozwiń zestaw do wyświetlania wielkość skojarzone kopii zapasowych. Kliknij i wybierz woluminu z zestawu kopii zapasowych.

     ![Klonowanie woluminu](./media/storsimple-clone-volume-u2/CloneVol.png) 

3. Kliknij pozycję **klonowanie** , aby rozpocząć, klonowanie wybranego woluminu.

4. W Kreatorze klonowanie głośności w obszarze **Określ nazwę i lokalizację**:

  1. Zidentyfikuj urządzenia. To jest lokalizacja, w której zostanie utworzony duplikat. Można wybrać tego samego urządzenia lub określić inne urządzenie. Jeśli wybierzesz woluminu skojarzonego z innych dostawców usług w chmurze (nie Azure) listy rozwijanej dla urządzenia będą wyświetlane tylko fizycznymi. Nie można klonowanie woluminu skojarzonego z innych dostawców usług chmury na urządzeniu wirtualną.

        >[AZURE.NOTE] Upewnij się, że wydajność wymaganą dla klonowanie jest niższy niż możliwości urządzenia.

  2. Określ nazwę głośność unikatowe dla swojego klonowanie. Nazwa musi zawierać od 3 do 127 znaków. 
    
        >[AZURE.NOTE] Pole **Klonowanie głośność jako** będzie **Tiered** , nawet wtedy, gdy są klonowanie lokalnie przypięty głośność. Nie można zmienić to ustawienie; Jednak w razie potrzeby sklonowanym woluminu lokalnie można także przypięte, można przekonwertować klonowanie woluminu lokalnie przypięty po utworzeniu klonowanie. Aby dowiedzieć się, jak konwertowania woluminu warstwowych lokalnie przypięty głośność przejdź do sekcji [Zmienianie typu woluminu](storsimple-manage-volumes-u2.md#change-the-volume-type).

        ![Kreator klonowanie 1](./media/storsimple-clone-volume-u2/clone1.png) 

  3. Kliknij ikonę strzałki ![Ikona strzałki](./media/storsimple-clone-volume-u2/HCS_ArrowIcon.png) Aby przejść do następnej strony.

5. W obszarze **hosts Określ, które mogą używać tego woluminu**:

  1. Określ rekord kontroli dostępu (awaryjnego) na sklonuj. Można dodawać nowe awaryjnego lub wybierz z istniejącej listy.

        ![Kreator klonowanie 2](./media/storsimple-clone-volume-u2/clone2.png) 

  2. Kliknij ikonę wyboru ![Ikona wyboru](./media/storsimple-clone-volume-u2/HCS_CheckIcon.png)Aby zakończyć operację.

6. Zadanie klonowanie będą inicjowane i powiadomienia, gdy zostanie pomyślnie utworzony. Kliknij przycisk **Zadania** monitorowanie klonowanie zadania na stronie **zadania** . Po zakończeniu wykonywania zadania klonowanie, zobaczysz następujący komunikat:

    ![Klonowanie wiadomości](./media/storsimple-clone-volume-u2/CloneMsg.png) 

7. Po zakończeniu zadania klonowanie:

  1. Przejdź na stronę **urządzenia** , a następnie wybierz kartę **Kontenerów głośność** . 
  2. Wybierz kontener głośność, który jest skojarzony z wielkość źródła, które możesz sklonowany. Na liście wielkości powinna być widoczna klonowanie, która właśnie została utworzona.

>[AZURE.NOTE] Monitorowanie i domyślne kopii zapasowej zostały wyłączone automatycznie na sklonowanym głośność.

Klonowanie, utworzony w ten sposób jest przejściowych klonowanie. Aby uzyskać więcej informacji na temat typów klonowanie zobacz [przejściowych a trwały klonów](#transient-vs.-permanent-clones).

Ten klonowanie jest teraz zwykła głośności, a dowolnej operacji, która jest możliwe w woluminu będzie dostępny na sklonuj. Należy skonfigurować ten głośność żadnej kopii zapasowej.

## <a name="transient-vs-permanent-clones"></a>Zmiennych a trwały klonów

Klony przejściowych są tworzone tylko wtedy, gdy są klonowanie do innego urządzenia. Czy klonowanie woluminu określonego z zestawu do innego urządzenia zarządzane przez Menedżera StorSimple kopii zapasowych. Klonowanie przejściowych będzie zawierać odwołania do danych wielkości oryginalny a użyje tych danych do odczytu i zapisu lokalnie na urządzeniu docelowej. 

Po zrób migawkę chmury przejściowych klonowanie, klonowanie wynikowy będzie *trwały* klonowanie. W trakcie tego procesu zostanie utworzona kopia danych w chmurze i czas, aby skopiować te dane zależy od rozmiaru danych i opóźnienia Azure, (jest to kopia Azure do Azure). Ten proces może potrwać dni, tygodni. Klonowanie przejściowych staje się trwały klonowanie w ten sposób i nie zawiera żadnych odwołań do oryginalne dane głośność, który został sklonowany z. 

## <a name="scenarios-for-transient-and-permanent-clones"></a>Scenariusze klony przejściowych i stały

W poniższych sekcjach opisano przykład sytuacje, w których można używać klony przejściowych i stałe.

### <a name="item-level-recovery-with-a-transient-clone"></a>Odzyskiwanie poziomie elementu z przejściowych klonowanie

Należy odzyskać plik prezentacji programu Microsoft PowerPoint jeden rok tygodniowych. IT administrator identyfikuje kopii zapasowej z tego przedziału czasu, a następnie filtruje głośność. Następnie administrator klonów głośność, wyszukuje plik, którego szukasz i jej miejsce. W tym scenariuszu przejściowych klonowanie jest używana. 
 
![Klip wideo dostępne](./media/storsimple-clone-volume-u2/Video_icon.png) **wideo dostępne**

Aby obejrzeć klip wideo pokazano, jak używać klonowanie i przywracanie funkcje w StorSimple Aby odzyskać usunięte pliki, kliknij [tutaj](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

### <a name="testing-in-the-production-environment-with-a-permanent-clone"></a>Testowanie w środowisku produkcyjnym z trwały klonowanie

Należy sprawdzić testowania błędów w środowisku produkcyjnym. Tworzenie klonowanie wielkości w środowisku produkcyjnym, a następnie zrób migawkę chmury ten klonowanie utworzonego niezależnych sklonowanym. W tym scenariuszu trwały klonowanie jest używana.  

## <a name="next-steps"></a>Następne kroki
- Dowiedz się, jak [przywrócić woluminu StorSimple z zestawu kopii zapasowych](storsimple-restore-from-backup-set-u2.md).

- Dowiedz się, jak [używać usługi Menedżera StorSimple administrowania urządzenia StorSimple](storsimple-manager-service-administration.md).

 
