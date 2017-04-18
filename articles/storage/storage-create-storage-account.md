<properties
    pageTitle="Sposoby tworzenia, zarządzania i usuwania konta miejsca do magazynowania w Azure Portal | Microsoft Azure"
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

Konto Azure magazynowania zawiera unikatowych nazw do przechowywania i udostępniania obiekty danych magazyn Azure. Wszystkie obiekty na koncie przestrzeni dyskowej są ze sobą zafakturowane jako grupy. Domyślnie dane na koncie jest dostępna tylko dla Ciebie właściciela konta.

[AZURE.INCLUDE [storage-account-types-include](../../includes/storage-account-types-include.md)]

## <a name="storage-account-billing"></a>Karta konta miejsca do magazynowania

[AZURE.INCLUDE [storage-account-billing-include](../../includes/storage-account-billing-include.md)]

> [AZURE.NOTE] Po utworzeniu Azure maszyn wirtualnych konto miejsca do magazynowania jest tworzony automatycznie w lokalizacji wdrożenia Jeśli nie masz już konto miejsca do magazynowania w tym miejscu. Dlatego nie jest konieczne wykonaj poniższe czynności, aby utworzyć konto miejsca do magazynowania dla dysków maszyn wirtualnych. Nazwę konta magazynu będzie oparty na tym nazwisku maszyn wirtualnych. Zapoznaj się z [dokumentacją maszyn wirtualnych Azure](https://azure.microsoft.com/documentation/services/virtual-machines/) uzyskać więcej szczegółowych informacji.

## <a name="storage-account-endpoints"></a>Punkty końcowe konta miejsca do magazynowania

Każdy obiekt, który jest przechowywany w magazynie Azure ma unikatowy adres URL. Nazwę konta magazynu stanowi poddomeny tego adresu. Kombinacja nazwy poddomeny i domeny, która jest specyficzne dla każdej usługi, stanowi punkt *końcowy* dla Twojego konta miejsca do magazynowania.

Na przykład jeśli Twoje konto miejsca do magazynowania nosi nazwę *mystorageaccount*, punkty końcowe domyślne dla Twojego konta miejsca do magazynowania to:

- Blob usługi: http://*mystorageaccount*. blob.core.windows.net

- Tabela usługi: http://*mystorageaccount*. table.core.windows.net

- W kolejce usługi: http://*mystorageaccount*. queue.core.windows.net

- Usługa plików: http://*mystorageaccount*. file.core.windows.net

> [AZURE.NOTE] Konto magazyn obiektów Blob udostępnia tylko punktu końcowego usługi obiektów Blob.

Adres URL dostępu do obiektu na koncie miejsca do magazynowania jest tworzona przez dołączenie lokalizacji obiektu na koncie miejsca do magazynowania do punktu końcowego. Na przykład adres obiektów blob może być w tym formacie: http://*mystorageaccount*.blob.core.windows.net/*mycontainer*/*myblob*.

Można również skonfigurować niestandardową nazwę domeny do użytku z Twojego konta miejsca do magazynowania. W przypadku kont klasyczny miejsca do magazynowania zobacz [Konfigurowanie domenę niestandardową nazwę punkt końcowy magazyn obiektów Blob](storage-custom-domain-name.md) , aby uzyskać szczegółowe informacje. W przypadku kont miejsca do magazynowania Menedżera zasobów ta funkcja nie został dodany do [Azure portal](https://portal.azure.com) jeszcze, ale można ją skonfigurować przy użyciu programu PowerShell. Aby uzyskać więcej informacji zobacz polecenia cmdlet [Set-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607146.aspx) .  

## <a name="create-a-storage-account"></a>Tworzenie konta miejsca do magazynowania

1. Zaloguj się do [portalu Azure](https://portal.azure.com).

2. W menu Centrum wybierz **Nowy** -> **danych + miejsca do magazynowania** -> **konta miejsca do magazynowania**.

3. Wprowadź nazwę konta magazynu. Aby uzyskać szczegółowe informacje o używaniu nazwę konta magazynu adresem obiekty w magazynie Azure, zobacz [punkty końcowe konta miejsca do magazynowania](#storage-account-endpoints) .

    > [AZURE.NOTE] Nazwy kont miejsca do magazynowania musi być pomiędzy 3 a 24 znaków i może zawierać liczby i wyłącznie małymi literami.
    >  
    > Nazwę konta magazynu musi być unikatowa w Azure. Azure portal wskazują, jeśli nazwę konta magazynu wybrane jest już używana.

4. Określ model wdrożenia, który ma być używany: **Menedżer zasobów** lub **klasycznego**. **Menedżer zasobów** jest model wdrożenia zalecane. Aby uzyskać więcej informacji zobacz [wdrożenia opis Menedżera zasobów i wdrażania klasyczny](../resource-manager-deployment-model.md).

    > [AZURE.NOTE] Konta magazyn obiektów blob można utworzyć tylko przy użyciu modelu wdrożenia Menedżera zasobów.

5. Wybierz typ konta miejsca do magazynowania: **uniwersalny** lub **Magazyn obiektów Blob**. **Uniwersalny** to ustawienie domyślne.

    Jeśli wybrano **uniwersalny** , określić poziom wydajności: **Standard** lub **Premium**. Wartość domyślna to **Standardowy**. Uzyskać więcej informacji dotyczących konta standardowe i specjalnego miejsca do magazynowania, zobacz [Wprowadzenie do magazyn Microsoft Azure](storage-introduction.md) i [miejsca do magazynowania Premium: wysokiej wydajności miejsca do magazynowania dla Azure maszyn wirtualnych obciążenia](storage-premium-storage.md).

    Jeśli wybrano **Magazyn obiektów Blob** , określić poziom dostępu: **aktywny** lub **atrakcyjne**. Wartość domyślna to **aktywny**. Zobacz [Magazyn obiektów Blob platformy Azure: schłodzić i Hot poziomów](storage-blob-storage-tiers.md) uzyskać więcej szczegółowych informacji.

6. Wybierz opcję replikacji dla konta miejsca do magazynowania: **LRS**, **GRS**, **Pomoc Zdalna GRS**lub **ZRS**. Wartość domyślna to **GRS pomoc Zdalna**. Aby uzyskać więcej informacji na temat opcji replikacji magazyn Azure zobacz [Replikacja magazyn Azure](storage-redundancy.md).

7. Wybierz subskrypcję, w którym chcesz utworzyć nowe konto miejsca do magazynowania.

8. Podaj nową grupę zasobów lub zaznacz istniejącej grupy zasobów. Aby uzyskać więcej informacji dotyczących grup zasobów zobacz [Omówienie Menedżera zasobów Azure](../azure-resource-manager/resource-group-overview.md).

9. Wybierz lokalizację geograficzną konta miejsca do magazynowania. Aby uzyskać więcej informacji na temat dostępnych usług w którym regionie, zobacz [Obszary Azure](https://azure.microsoft.com/regions/#services) .

10. Kliknij przycisk **Utwórz** , aby utworzyć konto miejsca do magazynowania.

## <a name="manage-your-storage-account"></a>Zarządzać swoim kontem miejsca do magazynowania

### <a name="change-your-account-configuration"></a>Zmiana konfiguracji konta

Po utworzeniu konta miejsca do magazynowania, możesz zmienić jego konfiguracji, takie jak zmiana opcji replikacji używane dla konta lub zmieniania poziomu dostępu do konta magazyn obiektów Blob. W [Azure portal](https://portal.azure.com)przejdź do swojego konta miejsca do magazynowania, kliknij polecenie **wszystkie ustawienia** , a następnie kliknij **konfiguracji** , aby wyświetlić lub zmienić konfigurację konta.

> [AZURE.NOTE] W zależności od wybranej podczas tworzenia konta miejsca do magazynowania warstwie wydajności niektóre opcje replikacji może być niedostępne.

Zmiana opcji replikacji spowoduje zmianę do ceny. Aby uzyskać więcej informacji, zobacz stronę [ceny Azure miejsca do magazynowania](https://azure.microsoft.com/pricing/details/storage/) .

W przypadku kont magazyn obiektów Blob zmieniania poziomu dostępu może spowodować opłaty na zmianę oprócz zmiany do ceny. [Konta magazyn obiektów Blob — ceny i rozliczeń](storage-blob-storage-tiers.md#pricing-and-billing) , aby uzyskać więcej informacji, zobacz.

### <a name="manage-your-storage-access-keys"></a>Zarządzanie klawiszy dostępu do miejsca do magazynowania

Po utworzeniu konta magazynu platformy Azure generuje dwóch klawiszy dostępu 512-bitowej miejsca do magazynowania, które służą do uwierzytelniania podczas uzyskiwania dostępu do konta miejsca do magazynowania. Udostępniając dwóch klawiszy dostępu miejsca do magazynowania, Azure umożliwia ponownie wygenerować klucze nie przerwy w usłudze miejsca do magazynowania lub dostęp do tej usługi.

> [AZURE.NOTE] Firma Microsoft zaleca unikanie udostępniania kluczy dostępu miejsca do magazynowania żadnym innym osobom. Aby zezwolenia na dostęp do zasobów magazynowania bez się kluczy dostępu, możesz użyć *podpisu udostępniania*. Podpis udostępniania zapewnia dostęp do zasobów na Twoim koncie, dla interwału zdefiniowanego i uprawnień, które można określić. Aby uzyskać więcej informacji, zobacz [Za pomocą udostępnionego dostępu podpisów (SA)](storage-dotnet-shared-access-signature-part-1.md) .

#### <a name="view-and-copy-storage-access-keys"></a>Wyświetlanie i kopiowanie klawiszy dostępu miejsca do magazynowania

W [portalu Azure](https://portal.azure.com)przejdź do swojego konta miejsca do magazynowania, kliknij polecenie **wszystkie ustawienia** , a następnie kliknij **klawisze dostępu** , aby wyświetlić, kopiowanie i ponownie wygenerować klawiszy dostępu do konta. Karta **Klawiszy dostępu** zawiera również ciągów wstępnie skonfigurowane połączenie przy użyciu kluczy podstawowych i pomocniczych, skopiowane można używać w aplikacjach.

#### <a name="regenerate-storage-access-keys"></a>Ponownie wygenerować klawiszy dostępu miejsca do magazynowania

Zalecamy zmieniać klawisze dostępu do konta miejsca do magazynowania okresowo, aby zwiększyć bezpieczeństwo połączeń miejsca do magazynowania. Dwóch klawiszy dostępu jest przypisana, dzięki czemu można obsługiwać połączenia z kontem miejsca do magazynowania przy użyciu jednego klucza dostępu podczas Generuj kod dostępu.

> [AZURE.WARNING] Ponowne generowanie kluczy dostępu może wpłynąć na usługi Azure, a także własne aplikacje, które są zależne od konta miejsca do magazynowania. Przy użyciu nowego klucza można zaktualizować wszystkich klientów, którzy przy użyciu klucza dostępu do uzyskania dostępu do konta miejsca do magazynowania.

**Usługi multimediów** — Jeśli masz usługi multimediów, które są zależne od konta miejsca do magazynowania, musisz ponownie synchronizowane klawiszy dostępu za pomocą usługi multimediów po generowania kluczy.

**Aplikacje** — Jeśli używasz aplikacji sieci web lub usługami w chmurze, które korzystają z konta miejsca do magazynowania, spowoduje utratę połączeń Generuj klawiszy, chyba że użycia kluczy.

**Eksploratorów miejsca do magazynowania** — Jeśli korzystasz z dowolnego [miejsca do magazynowania Eksploratora aplikacji](storage-explorers.md), prawdopodobnie należy zaktualizować klucz magazynowania używane przez te aplikacje.

Oto proces obracanie klawiszy dostępu do miejsca do magazynowania:

1. Aktualizowanie ciągów połączeń w kodzie aplikacji, aby odwołać klawisz dostępu pomocniczej konta miejsca do magazynowania.

2. Odtworzyć klucz podstawowy dostępu do konta miejsca do magazynowania. Na karta **Klawiszy dostępu** kliknij **Ponownie wygenerować klucz1**, a następnie kliknij przycisk **Tak,** aby potwierdzić, że chcesz wygenerować nowy klucz.

3. Aktualizowanie ciągów połączeń w kodzie, aby odwołać się nowy klucz podstawowy dostęp.

4. Klawisz dostępu pomocniczej odtworzyć w taki sam sposób.

## <a name="delete-a-storage-account"></a>Usuwanie konta miejsca do magazynowania

Aby usunąć konto miejsca do magazynowania, które nie są już używasz, przejdź do konta miejsca do magazynowania w [Azure portal](https://portal.azure.com)i kliknij przycisk **Usuń**. Usunięcie konta miejsca do magazynowania powoduje usunięcie całej rachunku, w tym wszystkie dane w oknie konta.

> [AZURE.WARNING] Nie jest możliwe przywrócić Konto usunięte miejsca do magazynowania lub pobrać dowolną zawartość, która zawiera przed usunięciem. Należy wykonać kopię zapasową wszystkiego, który chcesz zapisać przed usunięciem konta. To również prawdziwe dla wszystkich zasobów w oknie konta — po usunięciu obiektów blob, tabeli, kolejki lub plik zostaje trwale usunięty.

Aby usunąć konto miejsca do magazynowania, który jest skojarzony z Azure maszyn wirtualnych, najpierw należy usunięto jakiekolwiek dyski maszyn wirtualnych. Usunięcie nie najpierw dysków maszyn wirtualnych, następnie podczas próby usunięcia konta miejsca do magazynowania, zobaczysz komunikat o błędzie podobny do:

    Failed to delete storage account <vm-storage-account-name>. Unable to delete storage account <vm-storage-account-name>: 'Storage account <vm-storage-account-name> has some active image(s) and/or disk(s). Ensure these image(s) and/or disk(s) are removed before deleting this storage account.'.

Jeśli konto magazynu używa modelu wdrożenia klasyczny, można usunąć dysk maszyn wirtualnych, wykonując następujące kroki w [Azure portal](https://manage.windowsazure.com):

1. Przejdź do [klasycznej portal Azure](https://manage.windowsazure.com).
2. Przejdź do karty maszyn wirtualnych.
3. Kliknij kartę dysków.
4. Wybierz dysk danych, a następnie kliknij polecenie Usuń dysk.
5. Aby usunąć obrazów dysku, przejdź do karty obrazy i Usuń wszystkie obrazy, które są przechowywane na koncie.

Aby uzyskać więcej informacji zapoznaj się z [dokumentacją maszyn wirtualnych Azure](http://azure.microsoft.com/documentation/services/virtual-machines/).

## <a name="next-steps"></a>Następne kroki

- [Magazynem obiektów Blob platformy Azure: Poziomy schłodzić i aktywny](storage-blob-storage-tiers.md)
- [Azure replikacji miejsca do magazynowania](storage-redundancy.md)
- [Konfigurowanie parametry połączenia Azure miejsca do magazynowania](storage-configure-connection-string.md)
- [Przesyłanie danych za pomocą narzędzia wiersza polecenia AzCopy](storage-use-azcopy.md)
- Odwiedź [Blog zespołu Azure miejsca do magazynowania](http://blogs.msdn.com/b/windowsazurestorage/).
