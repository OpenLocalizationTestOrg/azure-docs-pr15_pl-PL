<properties
    pageTitle="Rozwiązywanie problemów z usuwania konta Azure miejsca do magazynowania, kontenerów i wirtualnych dysków twardych w klasycznym wdrożeniu | Microsoft Azure"
    description="Rozwiązywanie problemów z usuwania konta Azure miejsca do magazynowania, kontenerów i wirtualnych dysków twardych w klasycznym wdrożeniu"
    services="storage"
    documentationCenter=""
    authors="genlin"
    manager="felixwu"
    editor="tysonn"
    tags="storage"/>

<tags
    ms.service="storage"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="genli"/>

# <a name="troubleshoot-deleting-azure-storage-accounts-containers-or-vhds-in-a-classic-deployment"></a>Rozwiązywanie problemów z usuwania konta Azure miejsca do magazynowania, kontenerów i wirtualnych dysków twardych w klasycznym wdrożeniu

[AZURE.INCLUDE [storage-selector-cannot-delete-storage-account-container-vhd](../../includes/storage-selector-cannot-delete-storage-account-container-vhd.md)]

Podczas próby usunięcia konta Azure miejsca do magazynowania, kontener lub wirtualnego dysku twardego w [Azure portal](https://portal.azure.com/) lub [portal Azure klasyczny](https://manage.windowsazure.com/), może wystąpić błędy. Problemy może być spowodowany w następujących sytuacjach:

-   Po usunięciu maszyny dysku i wirtualnego dysku twardego nie są automatycznie usuwane. Może to być przyczynach niepowodzenia na usunięcie konta miejsca do magazynowania. Nie możemy Usuń dysku, tak aby dysku można użyć do zainstalowania innej maszyn wirtualnych.

-   Nadal jest dzierżawę na dysku lub obiektów blob, który jest skojarzony z dysku.

Jeśli Azure problemu nie jest opisane w tym artykule, odwiedź fora Azure [MSDN i przepełnienie stosu](https://azure.microsoft.com/support/forums/). Możesz opublikować problem na forach te lub do @AzureSupport serwisie Twitter. Ponadto można pliku żądanie obsługi Azure, wybierając pozycję **Uzyskaj pomoc** w witrynie [pomocy technicznej Azure](https://azure.microsoft.com/support/options/) .

## <a name="symptoms"></a>Objawów

Następujące sekcja zawiera typowe błędy, które mogą zostać wyświetlone podczas próby usunięcia konta Azure miejsca do magazynowania, kontenerów lub wirtualnych dysków twardych.

### <a name="scenario-1-unable-to-delete-a-storage-account"></a>Scenariusz 1: Nie można usunąć konto miejsca do magazynowania

Po przejściu do konta miejsca do magazynowania w [Azure portal](https://portal.azure.com/) lub [portal Azure klasyczny](https://manage.windowsazure.com/) i wybierz pozycję **Usuń**, może zostać wyświetlony następujący komunikat o błędzie:

*Konto miejsca do magazynowania StorageAccountName zawiera obrazów maszyn wirtualnych. Upewnij się, że te obrazów maszyn wirtualnych zostaną usunięte przed usunięciem tego konta miejsca do magazynowania.*

Może zostać również wyświetlony następujący błąd:

**Portal na Azure**:

*Nie można usunąć konto miejsca do magazynowania < maszyn wirtualnych--nazwę konta magazynu->. Nie można usunąć konta miejsca do magazynowania < maszyn wirtualnych--nazwę konta magazynu->: "konto miejsca do magazynowania < maszyn wirtualnych--nazwę konta magazynu-> ma niektóre aktywne zdjęcia i/lub dyskach. Upewnij się, zdjęcia i/lub dyskach zostaną usunięte przed usunięciem tego konta przestrzeni dyskowej. ".*

**Portal klasyczny na Azure**:

*Konto miejsca do magazynowania < maszyn wirtualnych--nazwę konta magazynu-> ma niektóre aktywne zdjęcia i/lub dyskach, np. xxxxxxxxx-xxxxxxxxx-O-209490240936090599. Upewnij się, te obrazy i/lub dyskach zostaną usunięte przed usunięciem tego konta miejsca do magazynowania.*

Lub

**Portal na Azure**:

Konto miejsca do magazynowania *< maszyn wirtualnych--nazwę konta magazynu-> ma 1 (w kontenerach) mające aktywnego obrazu i/lub artefaktów dysku. Upewnij się, tych artefaktów zostaną usunięte z repozytorium obrazów przed usunięciem tego konta magazynu*.

**Portal klasyczny na Azure**:

Konto przesyłanie nie powiodło się miejsca do magazynowania *< maszyn wirtualnych--nazwę konta magazynu-> ma 1 (w kontenerach) mające aktywnego obrazu i/lub artefaktów dysku. Upewnij się, że te artefakty zostaną usunięte z repozytorium obrazu przed usunięciem tego konta miejsca do magazynowania. Podczas próby usunięcia konta miejsca do magazynowania i jest nadal aktywne dysków związanych z nim, zostanie wyświetlony komunikat informujący o tym, istnieją aktywne dyski, które trzeba usunąć*.

### <a name="scenario-2-unable-to-delete-a-container"></a>Scenariusz 2: Nie można usunąć kontenera

Podczas próby usunięcia kontenera magazynu, może zostać wyświetlony następujący komunikat o błędzie:

Nie można usunąć kontenera magazynu * <container name>. Błąd: "jest obecnie dzierżawy w kontenerze i identyfikator dzierżawy, nie został określony na żądanie*.

### <a name="scenario-3-unable-to-delete-a-vhd"></a>Scenariusz 3: Nie można usunąć wirtualnego dysku twardego

Po usunięciu maszyny i spróbuj go usunąć obiektów blob skojarzone wirtualnych dysków twardych, może zostać wyświetlony następujący komunikat:

*Nie można usunąć obiektów blob "Ścieżka/XXXXXX-XXXXXX-os-1447379084699.vhd'. Błąd: "jest obecnie dzierżawę na to i identyfikator dzierżawy, nie został określony na żądanie.*

## <a name="solution"></a>Rozwiązanie
Rozwiązania typowych problemów, wypróbuj następujące metody:

### <a name="step-1-delete-any-os-disks-that-are-preventing-deletion-of-the-storage-account-container-or-vhd"></a>Krok 1: Usuwanie wszystkich dysków systemu operacyjnego, które uniemożliwiają usunięcie konta miejsca do magazynowania, kontener lub wirtualnego dysku twardego

1. Przełącz się do [portalu klasyczny Azure](https://manage.windowsazure.com/).
2. Wybierz pozycję **maszyn wirtualnych** > **dysków**.

    ![Obraz dysków w środowisku maszyn wirtualnych systemu Azure portalu klasyczny.](./media/storage-cannot-delete-storage-account-container-vhd/VMUI.png)

3. Znajdź dysków, które są skojarzone z kontem miejsca do magazynowania, kontener lub wirtualny dysk twardy, który chcesz usunąć. Po zaznaczeniu lokalizację dysku, możesz znaleźć konta skojarzonego miejsca do magazynowania, kontener lub wirtualnego dysku twardego.

    ![Obraz przedstawiający informacje o lokalizacji dla dysków portalu klasyczny Azure](./media/storage-cannot-delete-storage-account-container-vhd/DiskLocation.png)

4. Upewnij się, czy nie maszyn wirtualnych znajduje się na polu **dołączone do** dysków, a następnie usuń dysków.

    > [AZURE.NOTE] Jeśli dysk jest dołączony do maszyny, nie można go usunąć. Dyski należą już usuniętego maszyn wirtualnych asynchroniczne. Może upłynąć kilka minut po usunięciu maszyn wirtualnych dla tego pola wyczyścić w górę.

### <a name="step-2-delete-any-vm-images-that-are-preventing-deletion-of-the-storage-account-or-container"></a>Krok 2: Usuń wszystkie obrazy maszyn wirtualnych, które uniemożliwiają usunięcie konta miejsca do magazynowania lub kontener

1. Przełącz się do [portalu klasyczny Azure](https://manage.windowsazure.com/).
2. Wybierz pozycję **maszyn wirtualnych** > **obrazy**, a następnie usuń obrazy, które są skojarzone z kontem miejsca do magazynowania, kontener lub wirtualnego dysku twardego.

    Następnie ponownie spróbuj usunąć konta miejsca do magazynowania, kontener lub wirtualnego dysku twardego.

> [AZURE.WARNING] Należy wykonać kopię zapasową wszystkiego, który chcesz zapisać przed usunięciem konta. Nie jest możliwe przywrócić Konto usunięte miejsca do magazynowania lub pobrać dowolną zawartość, która zawiera przed usunięciem. To również prawdziwe dla wszystkich zasobów w oknie konta: po usunięciu wirtualny dysk twardy, obiektów blob, tabeli, kolejki lub plik zostaje trwale usunięty. Upewnij się, że zasób nie jest używany.

## <a name="about-the-stopped-deallocated-status"></a>Informacje o statusie zatrzymania (alokację)

Maszyny wirtualne, które zostały utworzone w modelu Klasyczny wdrożenia i które zatrzymane będzie miał stan **Zatrzymano (alokację)** na [Azure portal](https://portal.azure.com/) lub [portal Azure klasyczny](https://manage.windowsazure.com/).

**Portal azure klasyczny**:

![Zatrzymanie maszyny wirtualne stan (Deallocated) w portalu Azure.](./media/storage-cannot-delete-storage-account-container-vhd/moreinfo2.png)


**Azure portal**:

![Zatrzymano (alokację) stan maszyny wirtualne portalu klasyczny Azure.](./media/storage-cannot-delete-storage-account-container-vhd/moreinfo1.png)

Stan "Zatrzymano (alokację)" udostępnia zasoby komputera, takie jak Procesor, pamięć i sieci. Dyski, jednak są nadal zachowywane więc można szybko ponownie utworzyć maszyn wirtualnych w razie potrzeby. Te dyski są tworzone na bieżąco wirtualnych dysków twardych, które są kopii Azure magazyn. Konto miejsca do magazynowania ma następujące wirtualnych dysków twardych, a na tych wirtualnych dysków twardych dysków ma dzierżawy.

## <a name="next-steps"></a>Następne kroki

- [Usuwanie konta miejsca do magazynowania](storage-create-storage-account.md#delete-a-storage-account)
- [Sposób podziału zablokowanych dzierżawy magazyn obiektów blob platformy Microsoft Azure (programu PowerShell)](https://gallery.technet.microsoft.com/scriptcenter/How-to-break-the-locked-c2cd6492)
