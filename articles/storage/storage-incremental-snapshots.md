<properties
    pageTitle="Używane przyrostowe migawki kopii zapasowych i odzyskiwania maszyn wirtualnych Azure | Microsoft Azure"
    description="Tworzenie niestandardowego rozwiązania kopii zapasowych i odzyskiwania dysków Azure maszyn wirtualnych przy użyciu migawek przyrostowe."
    services="storage"
    documentationCenter="na"
    authors="aungoo-msft"
    manager="tadb"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="aungoo"/>


# <a name="back-up-azure-virtual-machine-disks-with-incremental-snapshots"></a>Wykonywanie kopii zapasowej dysków Azure maszyn wirtualnych przy użyciu przyrostowe migawki

## <a name="overview"></a>Omówienie

Magazyn Azure oferuje możliwość migawek obiektów blob. Migawki przechwycić stan obiektów blob w danym momencie. W tym artykule firma Microsoft opisując scenariusz jak zachowasz kopie zapasowe dysków maszyn wirtualnych za pomocą migawek. Tej metody można użyć, gdy nie chcesz używać kopii zapasowej Azure i Usługa odzyskiwania, a następnie utworzyć niestandardowe strategii wykonywania kopii zapasowych dla dysków maszyn wirtualnych.

Azure maszyn wirtualnych dyski są przechowywane jako blob strony w magazynie Azure. Ponieważ firma Microsoft omówieniu strategii wykonywania kopii zapasowych maszyn wirtualnych dysków w tym artykule, firma Microsoft będzie odwołujące się do migawki w kontekście blob strony. Aby dowiedzieć się więcej na temat migawek, zajrzyj do [tworzenia migawki obiektów Blob](https://msdn.microsoft.com/library/azure/hh488361.aspx).

## <a name="what-is-a-snapshot"></a>Co to jest migawkę?

Migawka obiektów blob jest tylko do odczytu wersji blob, w którym jest rejestrowany w punkcie w czasie. Po utworzeniu migawki go można przeczytać, skopiowane, lub usunięte, ale nie można ich modyfikować. Migawki umożliwiają wykonywanie kopii zapasowej obiektów blob wyświetlaną w momencie w czasie. Do pozostałych wersji 2015-04-05 trzeba było możliwość kopiowania pełny migawek. Z pozostałych wersji 2015-07-08 i powyżej, możesz również skopiować przyrostowe migawek.

## <a name="full-snapshot-copy"></a>Kopiowanie pełny migawki

Migawki można skopiować do innego konta miejsca do magazynowania jako blob, aby zachować kopie zapasowe podstawowej obiektów blob. Możesz również skopiować migawkę przez blob podstawowej, która przypomina przywracanie obiektów blob do wcześniejszej wersji. Po skopiowaniu migawki z jednego miejsca do magazynowania konta do innego zajmie tego samego obszaru jako obiektów blob strony podstawowej. W związku z tym Kopiowanie migawki całego z jednego miejsca do magazynowania konta do innego będzie wolne, a także zajmie dużo miejsca na koncie docelowego miejsca do magazynowania.

>[AZURE.NOTE] Po skopiowaniu podstawowej obiektów blob do innego miejsca docelowego migawek to nie są kopiowane razem. Podobnie jeśli podstawowy obiektów blob zastąpić kopię, migawki skojarzone z podstawowych obiektów blob nie występuje i pozostać niezmieniona w obszarze Nazwa podstawowej obiektów blob.

### <a name="back-up-disks-using-snapshots"></a>Wykonywanie kopii zapasowej dyski przy użyciu migawek

Jako strategię wykonywania kopii zapasowych dla dysków maszyn wirtualnych migawek obiektów blob dysku lub na stronie i skopiuj je do innego konta miejsca do magazynowania za pomocą narzędzi, takich jak operacja [Kopiowania obiektów Blob](https://msdn.microsoft.com/library/azure/dd894037.aspx) lub [AzCopy](storage-use-azcopy.md). Migawkę można kopiować do miejsca docelowego blob strony pod inną nazwą. Otrzymane blob strony miejsce docelowe jest blob zapisywalny strony i nie migawkę. W dalszej części tego artykułu będą opisano czynności do wykonania kopii zapasowych maszyn wirtualnych dyskach przy użyciu migawek.

### <a name="restore-disks-using-snapshots"></a>Przywracanie dyski przy użyciu migawek

Gdy nadchodzi czas, aby przywrócić poprzednią wersję stabilny przechwytywane w jednym z kopii zapasowej migawki dysku, możesz skopiować migawkę przez blob strony podstawowej. Po migawki jest podwyższany do strony podstawowej blob pozostaje migawki, ale źródła jest zastępowany kopię, która może być zarówno odczytywana i zapisywana. W dalszej części tego artykułu będą opisano czynności, aby przywrócić poprzednią wersję dysku z jego migawki.

### <a name="implementing-full-snapshot-copy"></a>Wykonania kopii pełny migawki

Można zaimplementować kopię pełnego migawki, wykonując poniższe czynności,

-   Najpierw zrób migawkę podstawowej obiektów blob operacji [Obiektów Blob migawkę](https://msdn.microsoft.com/library/azure/ee691971.aspx) .
-   Następnie skopiuj migawki kontu miejsca do magazynowania docelowej przy użyciu [Obiektów Blob Kopiuj](https://msdn.microsoft.com/library/azure/dd894037.aspx).
-   Powtórz tę procedurę, aby zachować kopie zapasowe usługi podstawowej obiektów blob.

## <a name="incremental-snapshot-copy"></a>Kopiowanie przyrostowe migawki

Nowa funkcja w [GetPageRanges](https://msdn.microsoft.com/library/azure/ee691973.aspx) interfejsu API zawiera dużo lepszym sposobem wykonywanie kopii zapasowej migawki blob strony lub dysków. Interfejs API zwraca listę zmian między podstawowej obiektów blob i migawki. Zmniejsza ilość używanego miejsca przechowywania kopii zapasowej konta. Interfejs API obsługuje blob strony na Premium miejsca do magazynowania, a także standardowego magazynu. Za pomocą tego interfejsu API, można teraz tworzyć szybsze i efektywne rozwiązania tworzenia kopii zapasowych dla maszyny wirtualne Azure. Będą dostępne w wersji pozostałych 2015-07-08 lub nowszy.

Przyrostowe kopię migawkę umożliwia kopiowanie z jednego miejsca do magazynowania konta do innego różnicy między,

-   Podstawa obiektów blob i jego migawki lub
-   Dowolne dwa zrzuty podstawowej obiektów blob

Pod warunkiem, że są spełnione następujące warunki,

- To został utworzony w sty – 1 – 2016 lub nowszym.
- To nie została zastąpiona [PutPage](https://msdn.microsoft.com/library/azure/ee691975.aspx) lub [Kopiowanie obiektów Blob](https://msdn.microsoft.com/library/azure/dd894037.aspx) między dwoma migawek.


**Uwaga**: Ta funkcja jest dostępna dla Premium i standardowy blob strony Azure.

Jeśli używasz strategię wykonywania kopii zapasowych niestandardowych używa migawki, kopiowanie migawki z jednego miejsca do magazynowania konta do innego mogą być bardzo wolne i zajmuje dużo miejsca. Zamiast wklejać migawki całego do konta magazynu kopii zapasowej, można napisać różnicę między kolejnymi migawki blob kopii zapasowej strony. Dzięki temu podczas kopiowania i miejsce do przechowywania kopii zapasowych znacznie zmniejszony.

### <a name="implementing-incremental-snapshot-copy"></a>Wykonania kopii przyrostowe migawki

Kopiowanie migawki przyrostowe można zaimplementować, wykonując poniższe czynności,

-   Zrób migawkę podstawowej obiektów blob przy użyciu [Obiektów Blob migawkę](https://msdn.microsoft.com/library/azure/ee691971.aspx).
-   Kopiowanie migawki do konta magazynu kopii zapasowej docelowej za pomocą [Kopiowania obiektów Blob](https://msdn.microsoft.com/library/azure/dd894037.aspx). Są to obiektów blob kopii zapasowej strony. Zrób migawkę tego obiektów blob kopii zapasowej strony i przechowywać w kopii zapasowej konta.
-   Skorzystaj z innej migawki podstawowej obiektów blob przy użyciu obiektów Blob migawkę.
-   Uzyskaj różnicę między pierwszym i drugim migawek podstawowej obiektów blob przy użyciu [GetPageRanges](https://msdn.microsoft.com/library/azure/ee691973.aspx). Umożliwia określenie wartości daty/godziny migawki w celu uzyskania różnicy z nowego parametru **prevsnapshot** . Jeśli ten parametr jest, odpowiedź pozostałych będzie zawierać tylko stron, które zostały zmienione między migawki docelowej i poprzedniej migawki także wyczyść strony.
-   Aby zastosować te zmiany do obiektów blob kopii zapasowej strony za pomocą [PutPage](https://msdn.microsoft.com/library/azure/ee691975.aspx) .
-   Na koniec zrób migawkę obiektów blob kopii zapasowej strony i zapisać go w oknie konta magazynu kopii zapasowej.

W następnej sekcji będzie opisano bardziej szczegółowo jak zachowasz kopie zapasowe dysków za pomocą kopiowania przyrostowe migawki

## <a name="scenario"></a>Scenariusz

W tej sekcji będą opisano scenariusza, który wymaga niestandardowych strategii wykonywania kopii zapasowych w przypadku używania migawek dysków maszyn wirtualnych.

Należy rozważyć maszyny Azure serii DS z premium miejsca do magazynowania P30 dysku. Dysk P30 o nazwie *mypremiumdisk* jest przechowywana w konto miejsca do magazynowania premium o nazwie *mypremiumaccount*. Konto standardowego magazynu o nazwie *mybackupstdaccount* będzie używana do przechowywania kopii zapasowej *mypremiumdisk*. Chcemy zachować migawki *mypremiumdisk* co 12 godzin.

Aby dowiedzieć się o tworzeniu konta miejsca do magazynowania i dyski, odwołują się do [konta usługi o Azure miejsca do magazynowania](storage-create-storage-account.md).

Aby dowiedzieć się, wykonywanie kopii zapasowej maszyny wirtualne Azure, skorzystaj z [Planowanie maszyn wirtualnych Azure wykonywania kopii zapasowych](../backup/backup-azure-vms-introduction.md).

## <a name="steps-to-maintain-backups-of-a-disk-using-incremental-snapshots"></a>Czynności, aby zachować kopie zapasowe dysk przy użyciu przyrostowe migawki

Czynności opisane poniżej będzie migawek *mypremiumdisk* i przechowywanie kopii zapasowych w *mybackupstdaccount*. Kopia zapasowa będzie blob standardowej strony o nazwie *mybackupstdpageblob*. Blob kopii zapasowej strony zawsze zostaną zastosowane niezmienionym jako migawkę ostatniego *mypremiumdisk*.

1.  Najpierw należy utworzyć obiektów blob kopii zapasowej strony dla dysku premium miejsca do magazynowania. Aby to zrobić, zrób migawkę *mypremiumdisk* o nazwie *mypremiumdisk_ss1*.
2.  Kopiowanie tej migawki do mybackupstdaccount jako obiektu blob strony o nazwie *mybackupstdpageblob*.
3.  Zrób migawkę *mybackupstdpageblob* o nazwie *mybackupstdpageblob_ss1*, przy użyciu [Obiektów Blob migawki](https://msdn.microsoft.com/library/azure/ee691971.aspx) i zapisać go w *mybackupstdaccount*.
4.  Podczas tworzenia kopii zapasowych tworzenie innej migawki *mypremiumdisk*, powiedz *mypremiumdisk_ss2*, a następnie zapisz go w *mypremiumaccount*.
5.  Uzyskaj zmiany przyrostowe między dwoma migawki, *mypremiumdisk_ss2* i *mypremiumdisk_ss1*, za pomocą [GetPageRanges](https://msdn.microsoft.com/library/azure/ee691973.aspx) na *mypremiumdisk_ss2* z parametrem **prevsnapshot** ustawionym sygnatura czasowa *mypremiumdisk_ss1*. Zapisać te zmiany przyrostowe obiektów blob kopii zapasowej strony *mybackupstdpageblob* w *mybackupstdaccount*. Jeśli istnieją usunięte zakresy w zmiany przyrostowe, musi być usuwany z obiektów blob kopii zapasowej strony. Umożliwia zapisanie zmian przyrostowe obiektów blob kopii zapasowej strony [PutPage](https://msdn.microsoft.com/library/azure/ee691975.aspx) .
6.  Zrób migawkę kopii zapasowej strony obiektów blob *mybackupstdpageblob*, o nazwie *mybackupstdpageblob_ss2*. Usuwanie poprzedniego migawkę *mypremiumdisk_ss1* z nominalnej miejsca do magazynowania.
7.  Powtórz kroki od 4 do 6 okien kopii zapasowej. W ten sposób można zachować kopie zapasowe *mypremiumdisk* z konta standardowego magazynu.

![Wykonywanie kopii zapasowej dysku przy użyciu przyrostowe migawki](./media/storage-incremental-snapshots/storage-incremental-snapshots-1.png)

## <a name="steps-to-restore-a-disk-from-snapshots"></a>Czynności, aby przywrócić dysk z migawki

Czynności opisane poniżej spowoduje przywrócenie dysku premium, *mypremiumdisk* wcześniejszych migawek z magazynu kopii zapasowej konta *mybackupstdaccount*.

1.  Identyfikowanie punktu w czasie, który chcesz przywrócić dysku premium. Załóżmy, że jest to migawkę *mybackupstdpageblob_ss2*, który jest przechowywany w konta magazynu kopii zapasowej *mybackupstdaccount*.
2.  W mybackupstdaccount promowanie migawkę *mybackupstdpageblob_ss2* jako nowy obiektów blob kopii zapasowej strony podstawowej *mybackupstdpageblobrestored*.
3.  Zrób migawkę tego obiektów blob przywrócenie kopii zapasowej strony, o nazwie *mybackupstdpageblobrestored_ss1*.
4.  Kopiowanie obiektów blob strony przywracane *mybackupstdpageblobrestored* z *mybackupstdaccount* do *mypremiumaccount* jako nowego dysku premium *mypremiumdiskrestored*.
5.  Zrób migawkę *mypremiumdiskrestored*, o nazwie *mypremiumdiskrestored_ss1* dokonywania przyszłych przyrostowe kopie zapasowe.
6.  Punkt usługi Katalogowej serie maszyn wirtualnych na przywróconym dysku *mypremiumdiskrestored* i odłączanie stare *mypremiumdisk* z maszyn wirtualnych.
7.  Rozpocznij proces kopii zapasowej opisane w poprzedniej sekcji podano przywrócenie dysku *mypremiumdiskrestored*, używając *mybackupstdpageblobrestored* jako obiektów blob kopii zapasowej strony.

![Przywracanie dysku z migawki](./media/storage-incremental-snapshots/storage-incremental-snapshots-2.png)

## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej informacji na temat tworzenia migawek obiektów blob i Planowanie infrastruktury kopii zapasowej maszyn wirtualnych przy użyciu poniższych łączy.

- [Tworzenie migawki obiektów Blob](https://msdn.microsoft.com/library/azure/hh488361.aspx)
- [Planowanie infrastruktury kopii zapasowych maszyn wirtualnych](../backup/backup-azure-vms-introduction.md)
