<properties
    pageTitle="Rozwiązywanie problemów z błędami podczas usuwania konta Azure miejsca do magazynowania, kontenerów lub wirtualnych dysków twardych we wdrożeniu Menedżera zasobów | Microsoft Azure"
    description="Rozwiązywanie problemów z błędami podczas usuwania konta Azure miejsca do magazynowania, kontenerów lub wirtualnych dysków twardych we wdrożeniu Menedżera zasobów"
    services="storage"
    documentationCenter=""
    authors="genlin"
    manager="felixwu"
    editor="na"
    tags="storage"/>

<tags
    ms.service="storage"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="genli"/>

# <a name="troubleshoot-errors-when-you-delete-azure-storage-accounts-containers-or-vhds-in-a-resource-manager-deployment"></a>Rozwiązywanie problemów z błędami podczas usuwania konta Azure miejsca do magazynowania, kontenerów lub wirtualnych dysków twardych we wdrożeniu Menedżera zasobów

[AZURE.INCLUDE [storage-selector-cannot-delete-storage-account-container-vhd](../../includes/storage-selector-cannot-delete-storage-account-container-vhd.md)]

Podczas próby usunięcia konta Azure miejsca do magazynowania, kontener lub wirtualnego dysku twardego (wirtualnego dysku twardego) w [Azure portal](https://portal.azure.com), może wystąpić błędy. Ten artykuł zawiera wskazówki dotyczące rozwiązywania problemów w celu rozwiązania problemu w wdrożenia usługi Azure Menedżera zasobów.

Jeśli ten artykuł nie rozwiązania problemu Azure, odwiedź fora Azure [MSDN i przepełnienie stosu](https://azure.microsoft.com/support/forums/). Możesz opublikować problem na forach te lub do @AzureSupport serwisie Twitter. Ponadto można pliku żądanie obsługi Azure, wybierając pozycję **Uzyskaj pomoc** w witrynie [pomocy technicznej Azure](https://azure.microsoft.com/support/options/) .

## <a name="symptoms"></a>Objawów

### <a name="scenario-1"></a>Scenariusz 1

Podczas próby usunięcia wirtualnego dysku twardego z konta miejsca do magazynowania we wdrożeniu Menedżera zasobów, jest wyświetlany następujący komunikat o błędzie:

**Nie można usunąć obiektów blob "vhds/BlobName.vhd". Błąd: Jest obecnie dzierżawę na to i identyfikator dzierżawy, nie został określony na żądanie.**

Ten problem może wystąpić, ponieważ maszyny wirtualnej (maszyn wirtualnych) ma już dzierżawę na wirtualny dysk twardy, który chcesz usunąć.

### <a name="scenario-2"></a>Scenariusz 2

Podczas próby usunięcia kontenera z konta miejsca do magazynowania we wdrożeniu Menedżera zasobów, jest wyświetlany następujący komunikat o błędzie:

**Nie można usunąć wirtualnych dysków twardych"magazynowania kontener". Błąd: Jest obecnie dzierżawy w kontenerze i identyfikator dzierżawy, nie został określony na żądanie.**

Ten problem może wystąpić, ponieważ kontener wirtualny dysk twardy, który jest zablokowany w stanie dzierżawy.

### <a name="scenario-3"></a>Scenariusz 3

Podczas próby usunięcia konto miejsca do magazynowania we wdrożeniu Menedżera zasobów, jest wyświetlany następujący komunikat o błędzie:

**Nie można usunąć konto miejsca do magazynowania "StorageAccountName". Błąd: Nie można usunąć konto miejsca do magazynowania ze względu na jej artefakty używana.**

Ten problem może wystąpić powodujący konta miejsca do magazynowania wirtualny dysk twardy, który jest w stanie dzierżawy.

## <a name="solution"></a>Rozwiązanie

Aby rozwiązać te problemy, należy określić wirtualny dysk twardy, który powoduje wystąpienie błędu i skojarzone maszyn wirtualnych. Następnie odłączanie wirtualny dysk twardy z maszyn wirtualnych (w przypadku dysków danych) lub usuwanie maszyn wirtualnych, używanym wirtualny dysk twardy (w przypadku dysków systemu operacyjnego). Usuwa dzierżawy wirtualny dysk twardy i umożliwia do usunięcia.

### <a name="step-1-identify-the-problem-vhd-and-the-associated-vm"></a>Krok 1: Identyfikowanie problemu wirtualnego dysku twardego i skojarzone maszyn wirtualnych


1. Zaloguj się do [portalu Azure](https://portal.azure.com).
2. W menu **Centrum** zaznacz **wszystkie zasoby**. Przejdź do konta miejsca do magazynowania, który chcesz usunąć, a następnie wybierz **obiektów blob** > **wirtualnych dysków twardych**.

    ![Zrzut ekranu przedstawiający portalu z konta miejsca do magazynowania i kontener "wirtualnych dysków twardych" wyróżnione](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/opencontainer.png)

3. Sprawdź właściwości każdego wirtualnego dysku twardego w kontenerze. Znajdź wirtualny dysk twardy, który jest w stanie **Leased** . Następnie należy określić, która korzysta z maszyn wirtualnych wirtualnego dysku twardego. Zwykle można określić, która maszyn wirtualnych zawiera wirtualny dysk twardy, zaznaczając pole wyboru Nazwa wirtualnego dysku twardego:

    - Dyski systemu operacyjnego należy wykonać tę konwencję nazewnictwa: VMNameYYYYMMDDHHMMSS.vhd
    - Dyski danych należy wykonać tę konwencję nazewnictwa: VMName-RRRRMMDD-HHMMSS.vhd

    ![Zrzut ekranu przedstawiający informacje kontenera w portalu o nazwie maszyn wirtualnych, stanu dzierżawy "Zablokowane" i "Leased" wyróżniony stan dzierżawy](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/locatevm.png)

### <a name="step-2-remove-the-lease-from-the-vhd"></a>Krok 2: Usuwanie dzierżawy z wirtualnego dysku twardego

Aby usunąć maszyn wirtualnych, używanym wirtualny dysk twardy (w przypadku dysków systemu operacyjnego):

1.  Zaloguj się do [portalu Azure](https://portal.azure.com).
2.  W menu **Centrum** wybierz **maszyn wirtualnych**.
3.  Wybierz pozycję maszyn wirtualnych, który zawiera dzierżawę na wirtualnego dysku twardego.
4.  Upewnij się, że nic się nie korzysta aktywnie maszyny wirtualnej, a nie są już potrzebne maszyny wirtualnej.
5.  W górnej części karta **maszyn wirtualnych szczegóły** wybierz polecenie **Usuń**, a następnie kliknij **Tak,** aby potwierdzić.
6.  Maszyn wirtualnych powinny zostać usunięte, ale należy zachować wirtualnego dysku twardego. Jednak wirtualny dysk twardy powinna już dzierżawę nad nim. Może potrwać kilka minut dzierżawy wydawane. Aby sprawdzić, czy zwolnieniu dzierżawy, przejdź do **wszystkich zasobów** > **Nazwę konta magazynu** > **obiektów blob** > **wirtualnych dysków twardych**. W okienku **Właściwości obiektów Blob** wartość **Stanu dzierżawy** powinna być **Odblokuj**.

Aby odłączyć wirtualny dysk twardy z maszyn wirtualnych, który jest używany przez (w przypadku dysków danych):

1.  Zaloguj się do [portalu Azure](https://portal.azure.com).
2.  W menu **Centrum** wybierz **maszyn wirtualnych**.
3.  Wybierz pozycję maszyn wirtualnych, który zawiera dzierżawę na wirtualnego dysku twardego.
4.  Wybierz **dysków** na karta **Szczegóły maszyn wirtualnych** .
5.  Wybierz dysk danych, który zawiera dzierżawę na wirtualnego dysku twardego. Można określić, która wirtualny dysk twardy jest dołączona na dysku, zaznaczając pole wyboru adresu URL wirtualnego dysku twardego.
6.  Określanie z pewnością, że nic się nie jest korzysta się aktywnie z dysku danych.
7.  Kliknij pozycję **Odłącz** na karta **Szczegóły dysku** .
8.  Dysk powinien być teraz odłączony od Głosowa, a wirtualny dysk twardy ma już dzierżawę nad nim. Może potrwać kilka minut dzierżawy wydawane. Aby sprawdzić, że został wydany dzierżawy, przejdź do **wszystkich zasobów** > **Nazwę konta magazynu** > **obiektów blob** > **wirtualnych dysków twardych**. W okienku **Właściwości obiektów Blob** wartość **Stanu dzierżawy** powinna być **Odblokuj**.

## <a name="what-is-a-lease"></a>Co to jest dzierżawę?

Dzierżawa jest blokada, który może być używany do kontrolowania dostępu do obiektów blob (na przykład wirtualnego dysku twardego). Jeśli obiektów blob jest dzierżawy, tylko właściciele dzierżawy dostęp do obiektów blob. Ważne jest dzierżawę z następujących powodów:

-   Jeśli wielu właścicieli do zapisu w tej samej części to w tym samym czasie zapobiega uszkodzenie danych.
-   To zapobiega przed usunięciem, jeśli zawartość jest aktywnie używany (na przykład maszyny).
-   Konto miejsca do magazynowania zapobiega przed usunięciem, jeśli zawartość jest aktywnie używany (na przykład maszyny).



## <a name="next-steps"></a>Następne kroki

- [Usuwanie konta miejsca do magazynowania](storage-create-storage-account.md#delete-a-storage-account)
- [Sposób podziału zablokowanych dzierżawy magazyn obiektów blob platformy Microsoft Azure (programu PowerShell)](https://gallery.technet.microsoft.com/scriptcenter/How-to-break-the-locked-c2cd6492)
