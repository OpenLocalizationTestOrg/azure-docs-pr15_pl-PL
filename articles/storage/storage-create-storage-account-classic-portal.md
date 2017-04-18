<properties
    pageTitle="Sposoby tworzenia, zarządzania i usunąć konto miejsca do magazynowania w portalu klasyczny Azure | Microsoft Azure"
    description="Utwórz nowe konto miejsca do magazynowania, zarządzanie klawiszy dostępu do konta lub usuwanie konta miejsca do magazynowania w Azure Portal. Informacje na temat kont standardowych i premium przestrzeni dyskowej."
    services="storage"
    documentationCenter=""
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/26/2016"
    ms.author="robinsh"/>


# <a name="about-azure-storage-accounts"></a>Informacje o kontach Azure miejsca do magazynowania

[AZURE.INCLUDE [storage-selector-portal-create-storage-account](../../includes/storage-selector-portal-create-storage-account.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools](../../includes/storage-try-azure-tools.md)]

## <a name="overview"></a>Omówienie

Konto Azure magazynowania umożliwia dostęp do usług obiektów Blob platformy Azure, kolejki tabeli i plik w magazynie Azure. Konta miejsca do magazynowania przewiduje unikatowych nazw obiektów Azure miejsca do magazynowania danych. Domyślnie dane na koncie jest dostępna tylko dla Ciebie właściciela konta.

Istnieją dwa typy kont miejsca do magazynowania:

- Konto standardowego magazynu obejmuje magazyn obiektów Blob, tabeli kolejki i plik.
- Konto miejsca do magazynowania premium obecnie obsługuje tylko dysków Azure maszyn wirtualnych. Zobacz [miejsca do magazynowania Premium: wysokiej wydajności miejsca do magazynowania dla obciążenia maszyn wirtualnych Azure](storage-premium-storage.md) szczegółowo omówiono Premium przestrzeni dyskowej.

## <a name="storage-account-billing"></a>Karta konta miejsca do magazynowania

Konta dla Azure masową według konta miejsca do magazynowania. Miejsca do magazynowania koszty są oparte na cztery czynniki: pojemność, schemat replikacji transakcje miejsca do magazynowania i dane wyjściowe.

- Pojemność odwołuje się do ilości miejsca do magazynowania przydziale konto jest używany do przechowywania danych. Koszt, po prostu magazynowaniem danych jest określona przez ilości danych są przechowywane i jak są replikowane.
- Replikacja Określa, ile kopii danych są obsługiwane jednocześnie, w jakich lokalizacjach.
- Transakcje dotyczą wszystkie operacje odczytu i zapisu do magazynu Azure.
- Dane wyjściowe odwołuje się do danych przesyłanych poza Azure region. Po danych na swoim koncie miejsca do magazynowania jest dostępu do aplikacji, która nie jest uruchomiony w tym samym regionie, czy ta aplikacja jest usługi w chmurze lub innego typu aplikacji, są naliczane dla dane wyjściowe. (Dla usługi Azure, możesz podjąć kroki do grupowania danych i usług w tym samym centrach danych w celu zmniejszenia lub wyeliminowania opłat za przesyłanie danych wyjściowe.)  

Na stronie [Ceny miejsca do magazynowania Azure](https://azure.microsoft.com/pricing/details/storage) zawiera szczegółowe informacje cennik pojemność, replikacji i transakcje. Na stronie [Szczegółów ceny przeniesienia danych](https://azure.microsoft.com/pricing/details/data-transfers/) zawiera szczegółowe informacje cennik dla dane wyjściowe.

Aby uzyskać szczegółowe informacje dotyczące miejsca do magazynowania konta pojemności i wydajności elementów docelowych zobacz [skalowalność miejsca do magazynowania Azure i cele](storage-scalability-targets.md).

> [AZURE.NOTE] Po utworzeniu Azure maszyn wirtualnych konto miejsca do magazynowania jest tworzony automatycznie w lokalizacji wdrożenia Jeśli nie masz już konto miejsca do magazynowania w tym miejscu. Dlatego nie jest konieczne wykonaj poniższe czynności, aby utworzyć konto miejsca do magazynowania dla dysków maszyn wirtualnych. Nazwę konta magazynu będzie oparty na tym nazwisku maszyn wirtualnych. Zapoznaj się z [dokumentacją maszyn wirtualnych Azure](https://azure.microsoft.com/documentation/services/virtual-machines/) uzyskać więcej szczegółowych informacji.

## <a name="create-a-storage-account"></a>Tworzenie konta miejsca do magazynowania

1. Zaloguj się do [portalu klasyczny Azure](https://manage.windowsazure.com).

2. Kliknij przycisk **Nowy** na pasku zadań u dołu strony. Wybieranie **Usług danych** | **miejsca do magazynowania**, a następnie kliknij przycisk **Szybkie tworzenie**.

    ![NewStorageAccount](./media/storage-create-storage-account-classic-portal/storage_NewStorageAccount.png)

3. W **adres URL**wprowadź nazwę konta magazynu.

    > [AZURE.NOTE] Nazwy kont miejsca do magazynowania musi być pomiędzy 3 a 24 znaków i może zawierać liczby i wyłącznie małymi literami.
    >  
    > Nazwę konta magazynu musi być unikatowa w Azure. Portal klasyczny Azure wskazują, jeśli już pochodzi nazwę konta magazynu, które można wybrać.

    Aby uzyskać szczegółowe informacje o używaniu nazwę konta magazynu adresem obiekty w magazynie Azure, zobacz [punkty końcowe konta miejsca do magazynowania](#storage-account-endpoints) poniżej.

4. W **Grupie lokalizacji/koligacji**wybierz lokalizację dla Twojego konta miejsca do magazynowania, Zamknij, możesz lub klientom. Jeśli będzie można uzyskać dostępu do danych na swoim koncie miejsca do magazynowania z innej usługi Azure, takich jak maszyn wirtualnych Azure lub usługi w chmurze, warto wybrać grupę koligacji z listy do grupy konta miejsca do magazynowania w tym samym centrum danych z innych usług Azure, które są używane w celu zwiększenia wydajności i niższe koszty.

    Należy zauważyć, że musisz wybrać grupę koligacji po utworzeniu konta miejsca do magazynowania. Nie możesz przenieść istniejące konto do grupy koligacji. Uzyskać więcej informacji o grupach koligacji zobacz [Usługa lokalizacji współpracujących z grupą koligacji](#service-co-location-with-an-affinity-group) poniżej.

    >[AZURE.IMPORTANT] Aby określić, które lokalizacje są dostępne dla subskrypcji, możesz zadzwonić operację [listy wszystkich dostawców zasobów](https://msdn.microsoft.com/library/azure/dn790524.aspx) . Aby wyświetlić listę dostawców z programu PowerShell, zadzwoń do [Get-AzureLocation](https://msdn.microsoft.com/library/azure/dn757693.aspx). Z .NET użyj metody [listy](https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.provideroperationsextensions.list.aspx) klasy ProviderOperationsExtensions.
    >
    >Ponadto zobacz [Regionów Azure](https://azure.microsoft.com/regions/#services) , aby uzyskać więcej informacji na temat dostępnych usług w którym regionie.


5. Jeśli masz więcej niż jedną subskrypcję Azure, pole **subskrypcji** jest wyświetlane. W **subskrypcji**wprowadź Azure subskrypcję, do której chcesz użyć do konta miejsca do magazynowania z.

6. W przypadku **replikacji**wybierz żądany poziom replikacji dla Twojego konta miejsca do magazynowania. Opcja replikacji zalecane jest replikacji zbędne geo zawiera maksymalny wytrzymałości dla określonego typu danych. Aby uzyskać więcej informacji na temat opcji replikacji magazyn Azure zobacz [Replikacja magazyn Azure](storage-redundancy.md).

6. Kliknij przycisk **Utwórz konto miejsca do magazynowania**.

    Może potrwać kilka minut, aby utworzyć konto miejsca do magazynowania. Aby sprawdzić stan, można monitorować powiadomień w dolnej części Portal klasyczny Azure. Po utworzeniu konta miejsca do magazynowania z nowym kontem miejsca do magazynowania ma stanu **Online** i jest gotowa do użycia.

![StoragePage](./media/storage-create-storage-account-classic-portal/Storage_StoragePage.png)


### <a name="storage-account-endpoints"></a>Punkty końcowe konta miejsca do magazynowania

Każdy obiekt, który jest przechowywany w magazynie Azure ma unikatowy adres URL. Nazwę konta magazynu stanowi poddomeny tego adresu. Kombinacja nazwy poddomeny i domeny, która jest specyficzne dla każdej usługi, stanowi punkt *końcowy* dla Twojego konta miejsca do magazynowania.

Na przykład jeśli Twoje konto miejsca do magazynowania nosi nazwę *mystorageaccount*, punkty końcowe domyślne dla Twojego konta miejsca do magazynowania to:

- Blob usługi: http://*mystorageaccount*. blob.core.windows.net

- Tabela usługi: http://*mystorageaccount*. table.core.windows.net

- W kolejce usługi: http://*mystorageaccount*. queue.core.windows.net

- Usługa plików: http://*mystorageaccount*. file.core.windows.net

Po utworzeniu konta, można wyświetlić punkty końcowe dla konta miejsca do magazynowania na pulpicie nawigacyjnym miejsca do magazynowania w [Klasycznym Portal Azure](https://manage.windowsazure.com) .

Adres URL dostępu do obiektu na koncie miejsca do magazynowania jest tworzona przez dołączenie lokalizacji obiektu na koncie miejsca do magazynowania do punktu końcowego. Na przykład adres obiektów blob może być w tym formacie: http://*mystorageaccount*.blob.core.windows.net/*mycontainer*/*myblob*.

Można również skonfigurować niestandardową nazwę domeny do użytku z Twojego konta miejsca do magazynowania. Aby uzyskać szczegółowe informacje, zobacz temat [Konfigurowanie niestandardowej nazwy domeny dla usługi punktu końcowego usługi Magazyn obiektów blob](storage-custom-domain-name.md) .

### <a name="service-co-location-with-an-affinity-group"></a>Usługa lokalizacji współpracujących z grupą koligacji

*Grupa koligacja* jest geograficzne grupowania usług Azure i maszyny wirtualne za pomocą konta usługi Azure miejsca do magazynowania. Grupa koligacji zwiększyć wydajność usługi przez lokalizowanie obciążenia komputera, w tym samym centrum danych lub obok docelowych odbiorców użytkownika. Ponadto nie rozliczeń opłat są poniesione w związku z wyjściowego przy dostępu do danych na koncie miejsca do magazynowania z innej usługi, który jest częścią tej samej grupy koligacji.

> [AZURE.NOTE]  Aby utworzyć grupę koligacji, Otwórz obszar <b>Ustawienia</b> [Klasyczny Portal Azure](https://manage.windowsazure.com), kliknij pozycję <b>grupy koligacji</b>, a następnie kliknij <b>Dodaj grupę koligacji</b> lub przycisk <b>Dodaj</b> . Można również utworzyć i zarządzanie grupami koligacji za pomocą interfejsu API zarządzania usługi Azure. Aby uzyskać więcej informacji, zobacz <a href="http://msdn.microsoft.com/library/azure/ee460798.aspx">operacje na koligacji grup</a> .

## <a name="view-copy-and-regenerate-storage-access-keys"></a>Wyświetlanie, kopiowanie i ponownie wygenerować klawiszy dostępu miejsca do magazynowania

Podczas tworzenia konta magazynu platformy Azure generuje dwa miejsca do magazynowania 512-bitowej klawiszy dostępu, które służą do uwierzytelniania podczas uzyskiwania dostępu do konta miejsca do magazynowania. Udostępniając dwóch klawiszy dostępu miejsca do magazynowania, Azure umożliwia ponownie wygenerować klucze nie przerwy w usłudze miejsca do magazynowania lub dostęp do tej usługi.

> [AZURE.NOTE] Firma Microsoft zaleca unikanie udostępniania kluczy dostępu miejsca do magazynowania żadnym innym osobom. Aby zezwolenia na dostęp do zasobów magazynowania bez się kluczy dostępu, możesz użyć *podpisu udostępniania*. Podpis udostępniania zapewnia dostęp do zasobów na Twoim koncie, dla interwału zdefiniowanego i uprawnień, które można określić. Aby uzyskać więcej informacji, zobacz [Za pomocą udostępnionego dostępu podpisów (SA)](storage-dotnet-shared-access-signature-part-1.md) .

W [Klasycznym Portal Azure](https://manage.windowsazure.com)za pomocą **Narzędzia do zarządzania kluczami** na pulpicie nawigacyjnym lub stronie **Magazyn** do wyświetlania, kopiowanie i ponownie wygenerować klawiszy dostępu miejsca do magazynowania, używanych na dostęp do usług obiektów Blob, tabel i kolejki.

### <a name="copy-a-storage-access-key"></a>Kopiowanie klawisz dostępu miejsca do magazynowania  

**Narzędzia do zarządzania kluczami** służy do kopiowania miejsca do magazynowania klucz dostępu do użycia w parametry połączenia. Parametry połączenia wymaga nazwę konta magazynu i klucz do użycia w uwierzytelniania. Aby uzyskać informacje o konfigurowaniu parametry połączenia, aby uzyskać dostęp do usług Azure magazynu zobacz [Konfigurowanie parametry połączenia miejsca do magazynowania Azure](storage-configure-connection-string.md).

1. W [Klasycznym Portal Azure](https://manage.windowsazure.com)kliknij pozycję **Magazyn**, a następnie kliknij nazwę konta przestrzeni dyskowej, aby otworzyć pulpitu nawigacyjnego.

2. Kliknij przycisk **Zarządzaj klawiszy**.

    Zostanie otwarte okno **Zarządzania klawiszy dostępu** .

    ![Managekeys](./media/storage-create-storage-account-classic-portal/Storage_ManageKeys.png)


3. Aby skopiować klawisz dostępu miejsca do magazynowania, zaznacz tekst klucza. A następnie kliknij prawym przyciskiem myszy, a następnie kliknij polecenie **Kopiuj**.

### <a name="regenerate-storage-access-keys"></a>Ponownie wygenerować klawiszy dostępu miejsca do magazynowania
Zalecamy zmieniać klawisze dostępu do konta miejsca do magazynowania okresowo, aby zwiększyć bezpieczeństwo połączeń miejsca do magazynowania. Dwóch klawiszy dostępu są przypisana, dzięki czemu można obsługiwać połączenia z kontem miejsca do magazynowania przy użyciu jednego klucza dostępu podczas Generuj kod dostępu.

> [AZURE.WARNING] Ponowne generowanie kluczy dostępu może wpłynąć na usługi Azure, a także własne aplikacje, które są zależne od konta miejsca do magazynowania. Przy użyciu nowego klucza można zaktualizować wszystkich klientów, którzy przy użyciu klucza dostępu do uzyskania dostępu do konta miejsca do magazynowania.

**Usługi multimediów** — Jeśli masz usługi multimediów, które są zależne od konta miejsca do magazynowania, musisz ponownie synchronizowane klawiszy dostępu za pomocą usługi multimediów po generowania kluczy.

**Aplikacje** — Jeśli używasz aplikacji sieci web lub usługami w chmurze, które korzystają z konta miejsca do magazynowania, spowoduje utratę połączenia Generuj klawiszy, chyba że użycia kluczy. 

**Eksploratorów miejsca do magazynowania** — Jeśli korzystasz z dowolnego [miejsca do magazynowania Eksploratora aplikacji](storage-explorers.md), prawdopodobnie należy zaktualizować klucz magazynowania używane przez te aplikacje.

Oto proces obracanie klawiszy dostępu do miejsca do magazynowania:

1. Aktualizowanie ciągów połączeń w kodzie aplikacji, aby odwołać klawisz dostępu pomocniczej konta miejsca do magazynowania.

2. Odtworzyć klucz podstawowy dostępu do konta miejsca do magazynowania. W [Klasycznym Portal Azure](https://manage.windowsazure.com)z pulpitu nawigacyjnego lub na stronie **Konfigurowanie** kliknij pozycję **Narzędzia do zarządzania kluczami**. Kliknij pozycję **Generuj** w obszarze klucz podstawowy dostęp, a następnie kliknij przycisk **Tak,** aby potwierdzić, że chcesz wygenerować nowy klucz.

3. Aktualizowanie ciągów połączeń w kodzie, aby odwołać się nowy klucz podstawowy dostęp.

4. Generuj kod dostępu pomocniczą.

## <a name="delete-a-storage-account"></a>Usuwanie konta miejsca do magazynowania

Aby usunąć konto miejsca do magazynowania, które nie są już używasz, użyj **Usuwanie** pulpitu nawigacyjnego lub na stronie **Konfigurowanie** . **Usuwanie** Usuwa konto całego miejsca do magazynowania, wraz ze wszystkimi obiektami blob, tabele i kolejek w oknie konta.

> [AZURE.WARNING] Nie jest możliwe przywrócić Konto usunięte miejsca do magazynowania lub pobrać dowolną zawartość, która zawiera przed usunięciem. Należy wykonać kopię zapasową wszystkiego, który chcesz zapisać przed usunięciem konta. To również prawdziwe dla wszystkich zasobów w oknie konta — po usunięciu obiektów blob, tabeli, kolejki lub plik zostaje trwale usunięty.
>
> Jeśli Twoje konto miejsca do magazynowania plikami wirtualnego dysku twardego Azure maszyn wirtualnych, następnie należy usunąć odpowiednie obrazy i dysków, które używają tych plików wirtualnego dysku twardego przed usunięciem konta miejsca do magazynowania. Najpierw wyłączyć maszyny wirtualnej, gdy jest uruchomiony, a następnie usuń go. Aby usunąć dysków, przejdź do karty **dysków** i Usuń wszystkie dostępne dyski. Aby usunąć obrazy, przejdź do karty **obrazy** i Usuń wszystkie obrazy, które są przechowywane na koncie.

1. W [Klasycznym Portal Azure](https://manage.windowsazure.com)kliknij **miejsca do magazynowania**.

2. Kliknij w dowolnym miejscu zapis konta miejsca do magazynowania, z wyjątkiem nazwę, a następnie kliknij polecenie **Usuń**.

     - Lub -

    Kliknij nazwę konta przestrzeni dyskowej, aby otworzyć na pulpicie nawigacyjnym, a następnie kliknij polecenie **Usuń**.

3. Kliknij przycisk **Tak,** aby potwierdzić, że chcesz usunąć konto miejsca do magazynowania.

## <a name="next-steps"></a>Następne kroki

- Aby dowiedzieć się więcej na temat magazyn Azure, zapoznaj się z [dokumentacją magazyn Azure](https://azure.microsoft.com/documentation/services/storage/).
- Odwiedź [Blog zespołu Azure miejsca do magazynowania](http://blogs.msdn.com/b/windowsazurestorage/).
- [Przesyłanie danych za pomocą narzędzia wiersza polecenia AzCopy](storage-use-azcopy.md)
