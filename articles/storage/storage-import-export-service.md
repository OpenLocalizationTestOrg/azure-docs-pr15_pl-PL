<properties
    pageTitle="Przy użyciu importu i eksportu do przesyłania danych z magazynem obiektów Blob | Microsoft Azure"
    description="Dowiedz się, jak tworzyć importowanie i eksportowanie zadań w portalu Azure klasyczny przesyłanie danych do blob miejsca do magazynowania."
    authors="renashahmsft"
    manager="aungoo"
    editor="tysonn"
    services="storage"
    documentationCenter=""/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="renash"/>


# <a name="use-the-microsoft-azure-importexport-service-to-transfer-data-to-blob-storage"></a>Przesyłanie danych do magazyn obiektów Blob za pomocą usługi Microsoft Azure Importuj/Eksportuj

## <a name="overview"></a>Omówienie

Usługa Azure Importuj/Eksportuj umożliwia bezpieczne przesyłanie dużych ilości danych z magazynem obiektów blob platformy Azure poprzez wysyłanie dysków twardych do centrum danych Azure. Za pomocą tej usługi do przenoszenia danych z magazynem obiektów blob Azure do stacji dysków twardych i wysłać do witryny lokalnej. Ta usługa jest odpowiednia w sytuacjach, gdzie chcesz przenieść kilka TBs danych do lub z Azure, ale przekazywania lub pobierania przez sieć nie jest możliwe z powodu ograniczonej przepustowości lub koszty sieci wysoki.

Usługa wymaga dysków twardych należy locker bitowej zaszyfrowane dla bezpieczeństwa danych. Usługa obsługuje konta klasyczny magazynowania Prezentuj we wszystkich regionach Azure publicznej. Musisz wysłać dysków twardych do jednego z obsługiwanych lokalizacji określonej w dalszej części tego artykułu.

W tym artykule dowiesz się więcej o usłudze Azure Importuj/Eksportuj i jak wysłać dyski do kopiowania danych z magazynem obiektów Blob platformy Azure.

> [AZURE.IMPORTANT] Można tworzyć i zarządzać importowanie i eksportowanie zadania klasyczny przechowywanie przy użyciu portalu klasyczny lub [Importuj/Eksportuj usługi REST interfejsów API](http://go.microsoft.com/fwlink/?LinkID=329099). Menedżer zasobów magazynowania konta nie są obsługiwane w tej chwili.

## <a name="when-should-i-use-the-azure-importexport-service"></a>Kiedy należy używać usługi Azure Importuj/Eksportuj

Należy rozważyć przy użyciu usługi Azure Importuj/Eksportuj podczas przekazywania lub pobierania danych przez sieć jest zbyt wolno lub wprowadzenie dodatkowych przepustowości koszt zakazującym.

Można użyć tej usługi np w scenariuszach:

- Migrowanie danych w chmurze: Przenieś dużych ilości danych do Azure szybko i efektywnie kosztów.
- Zawartość rozkładu: szybkie wysyłanie danych do witryny klienta.
- Kopia zapasowa: Sporządzanie kopii zapasowych danych lokalnych do przechowywania w magazynie obiektów blob platformy Azure.
- Odzyskiwanie danych: odzyskiwanie dużej ilości danych przechowywanych w magazynie obiektów blob i go do swojej lokalizacji lokalnego.

## <a name="pre-requisites"></a>Wymagania wstępne

W tej sekcji możemy zostały wymienione wymagania wstępne wymagane do używania tej usługi. Przejrzyj je uważnie przed wysłaniem dysków.

### <a name="storage-account"></a>Konto magazynu

Musi być istniejącej subskrypcji usługi Azure i co najmniej jeden **Klasyczny** magazynu kont do korzystania z usługi Importuj/Eksportuj. Każdego zadania mogą służyć do przesyłania danych do lub z tylko jednego konta klasyczny miejsca do magazynowania. Innymi słowy zadanie pojedynczy Importuj/Eksportuj nie może obejmować między wieloma kontami miejsca do magazynowania. Aby uzyskać informacje na temat tworzenia nowego konta miejsca do magazynowania zobacz [jak utworzyć konto miejsca do magazynowania](storage-create-storage-account.md#create-a-storage-account).

### <a name="blob-types"></a>Typy obiektów blob

Usługa Azure Importuj/Eksportuj umożliwia kopiowanie danych do obiektów blob **Blok** lub blob **strony** . I odwrotnie można eksportować tylko blob **Blok** , blob **strony** lub **Dołączanie** obiektów blob z magazynu Azure przy użyciu tej usługi.

### <a name="job"></a>Zadanie

Aby rozpocząć proces importowania i eksportowania z magazynem obiektów Blob, należy najpierw utworzyć zadanie. Zadanie może być zadania importu lub eksportu:

- Tworzenie zadania importu, gdy chcesz przenieść dane mają lokalnego do obiektów blob na koncie Azure przestrzeni dyskowej.
- Tworzenie zadania eksportu, gdy chcesz przenieść dane obecnie przechowywane jako obiektów blob na koncie miejsca do magazynowania na dyskach twardych, które są wysyłane do Ciebie.

Po utworzeniu zadania możesz powiadomić usługę Importuj/Eksportuj że użytkownik będzie wysyłki jeden lub więcej dyskach twardych do centrum danych Azure.

- Dla zadania importu będzie wysyłki dyski z danymi.
- Zadania eksportu będzie wysyłki pustych dyskach twardych.
- Można wysłać do 10 dysków twardych na zadanie.

Można utworzyć importu lub eksportu zadania za pomocą [portalu klasyczny](https://manage.windowsazure.com/) lub [Interfejsu API usługi REST Importuj/Eksportuj miejsca do magazynowania Azure](http://go.microsoft.com/fwlink/?LinkID=329099).

### <a name="client-tool"></a>Narzędzie do klienta

Pierwszym krokiem przy tworzeniu **Importowanie** zadania jest przygotowywanie dysku, które zostanie wysłane do zaimportowania. Aby przygotować dysku, możesz nawiązać lokalnego serwera i uruchomić narzędzie Azure Importuj/Eksportuj klienta na serwerze lokalnym. To narzędzie klienta ułatwia kopiowanie danych na dysku, szyfrowanie danych na dysku przy użyciu funkcji BitLocker i generowanie plików dziennika dysk.

Pliki dziennika zawierają podstawowe informacje o zadanie i dysk, takie jak numer seryjny dysk i nazwę konta magazynu. Ten plik dziennika nie znajduje się na dysku. Jest on używany podczas tworzenia zadania importu. Krok po kroku szczegółowe informacje o tworzeniu zadania znajdują się w dalszej części tego artykułu.

Narzędzie do klienta tylko jest zgodny z 64-bitowym systemie operacyjnym Windows. Zobacz sekcję [System operacyjny](#operating-system) dla określonej wersji systemu operacyjnego obsługiwane.

Pobierz najnowszą wersję pakietu [Narzędzie klienta Azure Importuj/Eksportuj](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409). Aby uzyskać więcej informacji na temat korzystania z narzędzia Importuj/Eksportuj Azure zobacz [Azure Importuj/Eksportuj narzędzia odwołań](http://go.microsoft.com/fwlink/?LinkId=329032).

### <a name="hard-disk-drives"></a>Dysków twardych

Tylko 3,5 cala wewnętrznych dyskach twardych SATA II/III są obsługiwane przez usługę Importuj/Eksportuj. Możesz użyć twardych do 10TB.
Importowanie zadań tylko pierwszy głośność danych na dysku będą przetwarzane. Wielkość danych musi być sformatowany NTFS.
Podczas kopiowania danych na dysku twardym, możesz dołączyć go bezpośrednio przy użyciu łącznika SATA lub można dołączyć go zewnętrznie za pomocą zewnętrznego SATA II i III USB. Zaleca się przy użyciu jednego z następujących zewnętrznych karty SATA II i III USB:

- Anker 68UPSATAA - 02BU
- Anker 68UPSHHDS-BU
- Startech SATADOCK22UE
- Sharkoon QuickPort XT HC

Jeśli masz konwerter, który nie znajduje się powyżej, możesz spróbować uruchomienia narzędzia Importuj/Eksportuj Azure za pomocą usługi konwertera przygotowywanie dysku i sprawdzić, czy działa przed zakupem konwertera obsługiwane.

> [AZURE.IMPORTANT] Zewnętrznych dysków twardych dostarczane z wbudowanych karta USB nie są obsługiwane przez tę usługę. Ponadto nie można używać dysku wewnątrz obudowy zewnętrzny dysk twardy; Nie wysyłaj zewnętrznych dysków twardych.

### <a name="encryption"></a>Szyfrowanie

Dane na dysku musi być zaszyfrowany funkcją BitLocker. Chroni dane, gdy będzie ona w drodze.

Importowanie zadań istnieją dwa sposoby przeprowadzić szyfrowania. Pierwszym sposobem jest użycie / szyfrowanie parametrów podczas uruchamiania narzędzia klienta podczas przygotowywania dysk. Druga metoda jest włączyć funkcją BitLocker ręcznie na dysku i określić klucza szyfrowania w wierszu polecenia narzędzia klienta podczas przygotowywania dysków.

Dla zadań eksportu po skopiowaniu danych na dyskach, usługa są szyfrowane dysku za pomocą funkcji BitLocker przed wysłaniem go do Ciebie. Kluczem szyfrowania będą dostarczane użytkownikowi za pośrednictwem portalu klasyczny.  

### <a name="operating-system"></a>System operacyjny

Jedną z następujących 64-bitowych systemów operacyjnych umożliwia przygotowanie dysku twardego przy użyciu narzędzia Import/Eksport Azure przed wysłaniem dysk Azure:

System Windows 7 przedsiębiorstwa, Windows 7 Ultimate systemu Windows 8 Pro, przedsiębiorstwa systemu Windows 8, Windows 8.1 Pro, system Windows 8.1 Enterprise systemu Windows 10<sup>1</sup>, Windows Server 2008 R2, Windows Server 2012, systemu Windows Server 2012 R2. Wszystkie te systemy operacyjne obsługują dysków funkcją BitLocker.

> [AZURE.IMPORTANT] <sup>1</sup> Jeśli korzystasz z komputera systemu Windows 10 aby przygotować się na dysku twardym, Pobierz najnowszą wersję pakietu narzędzie Azure Importuj/Eksportuj.

### <a name="locations"></a>Lokalizacje

Kopiowanie danych do i z wszystkich kont magazynowania publicznej Azure obsługuje usługa Azure Importuj/Eksportuj. Można wysłać dysków twardych do jednej z następujących lokalizacji. W przypadku konta miejsca do magazynowania w miejscu publicznym Azure, która nie jest określana poniżej, lokalizacji alternatywnych wysyłki będzie dostępna podczas tworzenia zadania za pomocą portalu klasyczny lub interfejsu API usługi REST Importuj/Eksportuj.

Obsługiwane lokalizacje wysyłkowe:

- Wschodniej Stany Zjednoczone

- Zachód Stany Zjednoczone

- Wschodniej USA 2

- Centralny Stany Zjednoczone

- Ameryka Północna centralnej w Stanach Zjednoczonych

- Południowej centralnej Stany Zjednoczone

- Europa Północna

- Europa Zachodnia

- Wschodnioazjatyckie

- Kraje Azji Południowo

- Wschód Australia

- Australia kopiec

- Zachód Japonii

- Wschód Japonii

- Indie centralnej


### <a name="shipping"></a>Wysyłki

**Dyski wysyłki centrum danych:**

Podczas tworzenia zadania importu lub eksportu, należy zapewnić adres wysyłkowy jednego z obsługiwanych lokalizacje, aby wysłać dysków. Adres wysyłkowy podany zależy od lokalizacji konta miejsca do magazynowania, ale nie może być taka sama jak lokalizacja konta z miejsca do magazynowania.

Za pomocą przewoźników, takie jak FedEx, przez firmę DHL, UPS lub US pocztę do wysłania dysków do adres wysyłkowy.

**Dyski wysyłki z centrum danych:**

Podczas tworzenia Importuj lub Eksportuj zadanie, należy podać adresu zwrotnego dla dostawy dysków po zakończeniu pracy przez firmę Microsoft. Upewnij się, że podasz prawidłowy adres zwrotny w celu uniknięcia opóźnień przetwarzania.

Trzeba także podać prawidłową carrier FedEx lub przez firmę DHL numer używanego przez firmę Microsoft dla wysyłki dyski ponownie konta. Numer konta FedEx jest wymagane dla wysyłki dyski ponownie z lokalizacji Europy i USA. Numer konta przez firmę DHL jest wymagane dla wysyłki dysków ponownie z lokalizacji w Azji i Australii. Jeśli nie masz możesz utworzyć [FedEx](http://www.fedex.com/us/oadr/) (dla Stanów Zjednoczonych i Europie) lub konta przewoźnika [przez firmę DHL](http://www.dhl.com/) (Azja i Australia). Jeśli masz już numer konta przewoźnika, sprawdź, czy są poprawne.

W dostawy pakiety, można śledzić terminów na [Warunki usługi Microsoft Azure](https://azure.microsoft.com/support/legal/services-terms/).

> [AZURE.IMPORTANT] Uwaga nośnik fizyczny, który jest dostarczany może być konieczne krzyżowe międzynarodowe obramowania. Użytkownik jest odpowiedzialny za zapewnienie, że Twoje nośnik fizyczny i dane są zaimportowane i/lub wyeksportowane zgodnie z obowiązującym prawem. Przed wysłaniem nośnik fizyczny, skontaktuj się z usługi doradców, aby zweryfikować, że Twoje multimediów i dane można prawnie wysłane do Centrum określonych danych. Dzięki temu zapewnienie osiągnie firmy Microsoft, w odpowiednim czasie. Na przykład dowolny pakiet, który będzie krzyżowy międzynarodowe obramowania musi faktura handlowa do towarzyszy z pakietem (z wyjątkiem przypadku przekroczenia obramowań w Unii Europejskiej). Można wydrukować kopię wypełnionego faktura handlowa z witryny sieci Web przewoźników. Przykład komercyjnych faktury są [faktura handlowa przez firmę DHL] (http://invoice-template.com/wp-content/uploads/dhl-commercial-invoice-template.pdf) lub [Faktura handlowa FedEx](http://images.fedex.com/downloads/shared/shipdocuments/blankforms/commercialinvoice.pdf). Upewnij się, że Microsoft nie zostało wskazane jako eksporter.

## <a name="how-does-the-azure-importexport-service-work"></a>Jak działa usługa Azure Importuj/Eksportuj

Dane można przesyłać między swojej witryny lokalnej i magazyn obiektów blob platformy Azure przez tworzenie zadań i wysyłki dysków twardych do centrum danych Azure za pomocą usługi Azure Importuj/Eksportuj. Każdego dysku twardego, który jest skojarzony z pojedynczego zadania. Każde zadanie jest skojarzone z kontem jednego miejsca do magazynowania. Zapoznaj się z [sekcji wymagania wstępne](#pre-requisites) dokładnie, aby uzyskać informacje o szczegółowe informacje na temat tej usługi, takie jak obsługiwane typy obiektów blob, typów dysku, lokalizacji i wysyłki.

W tej sekcji opisano będzie na wysokim poziomie etapy importowanie i eksportowanie zadania. W [sekcji Szybkie uruchamianie](#quick-start)w dalszej części udostępniamy instrukcje krok po kroku, aby utworzyć importu i eksportu zadania.

### <a name="inside-an-import-job"></a>Wewnątrz zadania importu

Na wysokim poziomie zadania importu obejmuje następujące kroki:

- Określanie dane do zaimportowania i liczba dyski, które mają być.
- Identyfikowanie obiektów blob miejsca docelowego dla danych w magazynie obiektów Blob.
- Narzędzie Azure Importuj/Eksportuj do skopiuj dane do jednego lub więcej dysków twardych i ich szyfrowania przy użyciu funkcji BitLocker.  
- Tworzenie zadania importu na koncie klasyczny magazynowania docelowej przy użyciu portalu klasyczny lub interfejsu API usługi REST Importuj/Eksportuj. Jeśli za pomocą portalu klasyczny, przekazywać pliki dziennika dysk.
- Podaj adres zwrotny i numer konta przewoźnika ma być używany dla wysyłki dyski powrót do.
- Są dostarczane dysków twardych do adres wysyłkowy podany podczas tworzenia zadania.
- Zaktualizuj dostarczenia numer w sekcji szczegółów zadania importu śledzenia i przesyłanie zadania importu.
- Dyski są odbierane i przetwarzane w centrum przetwarzania danych Azure.
- Dyski są dostarczane za pomocą konta przewoźnika w polu adres zwrotny w zadania importu.

    ![Rysunek 1:Import przepływu pracy](./media/storage-import-export-service/importjob.png)


### <a name="inside-an-export-job"></a>Wewnątrz zadanie eksportu

Na wysokim poziomie zadanie eksportu obejmuje następujące kroki:

- Określanie danych do wyeksportowania i liczba dyski, które mają być.
- Określ blob źródła lub ścieżek kontenera danych w magazynie obiektów Blob.
- Tworzenie zadania eksportu na koncie miejsca do magazynowania źródła przy użyciu portalu klasyczny lub interfejsu API usługi REST Importuj/Eksportuj.
- Określ blob źródła lub ścieżek kontenera danych w zadaniu eksportu.
- Podaj adres zwrotny i numer konta przewoźnika dla ma być używany dla wysyłki dyski powrót do.
- Są dostarczane dysków twardych do adres wysyłkowy podany podczas tworzenia zadania.
- Aktualizowanie dostarczenia numer w sekcji szczegółów zadania eksportu śledzenia i Prześlij zadanie eksportu.
- Dyski są odbierane i przetwarzane w centrum przetwarzania danych Azure.
- Dyski są szyfrowane przy użyciu funkcji BitLocker; klucze są dostępne za pośrednictwem portalu klasyczny.  
- Dyski są dostarczane za pomocą konta przewoźnika w polu adres zwrotny w zadania importu.

    ![Rysunek 2:Export przepływu pracy](./media/storage-import-export-service/exportjob.png)

### <a name="viewing-your-job-status"></a>Wyświetlanie informacji o stanie zadania

Śledzenie stanu importowanie lub eksportowanie może dotyczyć zadania z portalu klasyczny. Przejdź do swojego konta miejsca do magazynowania w portalu klasyczny, a następnie kliknij kartę **Importuj/Eksportuj** . Na stronie pojawi się lista zadań. Można filtrować listy stan zadania, nazwa zadania, typ zadania lub numer identyfikacyjny.

Zostanie wyświetlony jeden z następujących statusów zadania w zależności od tego, gdzie jest dysku w procesie.

| Stan zadania   | Opis                                                                                                                                                                                                                                                                                                                                                                        |
|:-------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Tworzenie     | Zadania został utworzony, ale nie zostały jeszcze wprowadzone informacje dotyczące wysyłki.                                                                                                                                                                                                                                                                                                |
| Wysyłki     | Zadania został utworzony, i podano informacje dotyczące wysyłki. **Uwaga**: dysk dostarczeniu centrum danych Azure stan wciąż mogą pokazywać "Wysyłki" przez dłuższy czas. Po uruchomienia usługi Kopiowanie danych, stan zostanie zmieniona na "Transferowanie". Jeśli chcesz zobaczyć bardziej szczegółowe stan dysku, można użyć interfejsu API usługi REST Importuj/Eksportuj. |
| Przenoszenie | Dane są przesyłane z dysku twardego (w przypadku zadania importu) lub na dysku twardym (w przypadku zadanie eksportu).                                                                                                                                                                                                                                                                 |
| Pakowanie    | Transferu danych, a do wysyłki, aby odczytywał przygotowuje się na dysku twardym.                                                                                                                                                                                                                                                                             |
| Kończenie     | Dysk twardy został wysłany do Ciebie.                                                                                                                                                                                                                                                                                                                                      |

### <a name="time-to-process-job"></a>Czas na proces zadania

Czas trwania proces zadania Importuj/Eksportuj zmienia się w zależności od różnych czynników, takich jak czas dostawy zadań typu, typu i rozmiar kopiowanych danych i rozmiar dysków opisane. Usługa Importuj/Eksportuj nie ma Umowa dotycząca poziomu usług. Za pomocą interfejsu API usługi REST w celu śledzenia postępu zadania lepiej. W operacji listy zadań, która wskazuje postęp Kopiuj jest parametrem procentu wykonania. Bliższy kontakt z nami w razie potrzeby szacowana, aby ukończyć zadanie krytyczne Importuj/Eksportuj czasu.

### <a name="pricing"></a>Cennik

**Dysk opłata obsługę**

Istnieje opłatę dysk obsługi dla każdego dysku przetwarzane jako część importowania lub eksportowania zadania. Zobacz szczegóły na [Azure ceny Importuj/Eksportuj](https://azure.microsoft.com/pricing/details/storage-import-export/).

**Koszty wysyłki**

Gdy wysyłasz dyski Azure płacisz koszt wysyłki przewoźnika wysyłki. Gdy Microsoft zwraca dyski do Ciebie, koszt wysyłki jest naliczany do konta przewoźnika, które opisane w czasie tworzenia zadania.

**Koszty transakcji**

Istnieje bez kosztów transakcja podczas importowania danych do magazyn obiektów blob. Opłaty standardowy wyjściowym są stosowane w przypadku, gdy dane są eksportowane z magazynem obiektów blob. Aby uzyskać więcej szczegółów dotyczących kosztów transakcja, zobacz [przesyłania danych cennik.](https://azure.microsoft.com/pricing/details/data-transfers/)

## <a name="quick-start"></a>Szybki Start

W tej sekcji udostępniamy instrukcje krok po kroku dotyczące tworzenia zadanie eksportu i importu. Upewnij się, że spełniają wszystkie [wymagania wstępne](#pre-requisites) przed przejściem do przodu.

## <a name="how-to-create-an-import-job"></a>Jak utworzyć zadanie importu?

Tworzenie zadania importu, aby skopiować dane do swojego konta magazynu platformy Azure na dyskach twardych przez wysyłki jeden lub więcej dysków zawierającej dane do Centrum określonych danych. Zadania importu obejmuje szczegółowe informacje o dysków twardych, danych do skopiowania, kierować konta przestrzeni dyskowej oraz informacji dotyczących wysyłki z usługą Azure Importuj/Eksportuj. Tworzenie zadania importu jest trzy kroki. Najpierw należy przygotować dysków przy użyciu narzędzia klienta Azure Importuj/Eksportuj. Drugi Prześlij zadanie importu za pomocą portalu klasyczny. Trzecia wysłać dyski Aby adres wysyłkowy podany podczas tworzenia zadania i zaktualizuj informacje o wysyłki w szczegóły swojego zadania.   

> [AZURE.IMPORTANT] Można przesłać tylko jedno zadanie na konto miejsca do magazynowania. Każdy dysk, który można zaimportować do jednego miejsca do magazynowania konta. Załóżmy na przykład, że chcesz zaimportować dane do dwóch kont przestrzeni dyskowej. Należy użyć osobnych dysków twardych dla każdego konta miejsca do magazynowania i utworzyć osobne zadania dla każdego konta miejsca do magazynowania.

### <a name="prepare-your-drives"></a>Przygotowywanie dysków

Pierwszy krok w przypadku importowania danych przy użyciu usługi Azure Importuj/Eksportuj do przygotowania dysków przy użyciu narzędzia klienta Azure Importuj/Eksportuj. Wykonaj poniższe czynności, aby przygotować dysków.

1.  Identyfikowanie danych do zaimportowania. Może to być katalogów i autonomicznego plików na serwerze lokalnym lub w udziale sieciowym.  

2.  Określenie liczby dyski, które mają być w zależności od całkowity rozmiar danych. Zaopatrzenie wymaganą liczbę dysków twardych SATA II/III 3,5 cala.

3.  Zidentyfikuj konta docelowego miejsca do magazynowania, kontenera, katalogów wirtualnych i obiektów blob.

4.  Określanie katalogów i/lub autonomicznego pliki, które zostaną skopiowane do każdego dysku twardego.

5.  Skopiuj dane do jednego lub więcej dysków za pomocą [Narzędzia Azure Importuj/Eksportuj](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409) .

    - Narzędzie Azure Importuj/Eksportuj tworzy sesje Kopiuj, aby skopiować dane ze źródła do stacji dysków twardych. W sesji jedną kopią narzędzie można kopiować jednego katalogu wraz z jego podkatalogów lub pojedynczy plik.

    - Wiele kopii może być konieczne sesje Jeśli źródło danych zawiera wiele katalogów.

    - Przygotowanie każdego dysku twardego wymaga co najmniej jednej kopii sesji.

6.  Możesz określić / szyfrowanie parametr, aby włączyć funkcją BitLocker na dysku twardym. Można też kliknąć można także włączyć funkcją BitLocker ręcznie na dysku twardym i określ klucz uruchomienia narzędzia.

7.  Narzędzie Importuj/Eksportuj Azure generuje plik dziennika dysk dla każdego dysku, jak jest gotowa. Plik dziennika dysk jest przechowywany na komputerze lokalnym, nie ma na dysku. Będzie Przekaż plik dziennika, podczas tworzenia zadania importu. Plik dziennika dysk zawiera identyfikator dysku i klucza funkcji BitLocker, a także inne informacje o stacji dysków.
**Ważne**: każdego dysku twardego przygotowywania spowoduje w pliku dziennika. Podczas tworzenia zadania importu za pomocą portalu klasycznym należy przekazać wszystkie pliki dziennika dysków, które są częścią tego zadania importu. Dysków bez arkusza, które pliki nie będą przetwarzane.

8.  Po zakończeniu przygotowanie dysku nie zmodyfikować dane na dysków twardych lub pliku dziennika.

Poniżej znajdują się polecenia i przykłady dotyczące przygotowywania dysk twardy za pomocą narzędzia klienta Azure Importuj/Eksportuj.

Azure Importuj/Eksportuj klienta PrepImport polecenie Narzędzie podczas pierwszej sesji Kopiuj skopiować katalogu:

    WAImportExport PrepImport /sk:<StorageAccountKey> /csas:<ContainerSas> /t: <TargetDriveLetter> [/format] [/silentmode] [/encrypt] [/bk:<BitLockerKey>] [/logdir:<LogDirectory>] /j:<JournalFile> /id:<SessionId> /srcdir:<SourceDirectory> /dstdir:<DestinationBlobVirtualDirectory> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]

**Przykład:**

W poniższym przykładzie pracujemy kopiowania wszystkich plików i katalogów z H:\Video na dysku twardym umieszczony na X: podrzędne. Dane zostaną zaimportowane na koncie miejsca do magazynowania docelowego określonym przez klucz konta miejsca do magazynowania i do kontenera magazynu o nazwie wideo. Jeśli nie ma kontenera magazynu, zostanie on utworzony. To polecenie także sformatować i szyfrowanie dysk twardy docelowej.

    WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Video1 /logdir:c:\logs /sk:storageaccountkey /t:x /format /encrypt /srcdir:H:\Video1 /dstdir:video/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt

Azure Importuj/Eksportuj klienta PrepImport polecenie narzędzie dla kolejnych kopii sesji skopiować katalogu:

    WAImportExport PrepImport /j:<JournalFile> /id:<SessionId> /srcdir:<SourceDirectory> /dstdir:<DestinationBlobVirtualDirectory> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]

Kopiowanie kolejnych sesji do tego samego dysku twardym Określ tę samą nazwę pliku i podaj nowy identyfikator sesji; istnieje bez konieczności ponownie, zapewniające dysk klucz i docelowej konta miejsca do magazynowania ani sformatować lub szyfrowania dysku. W tym przykładzie Trwa kopiowanie folderu H:\Photo i jego katalogi sub na ten sam dysk docelowej w kontenerze miejsca do magazynowania o nazwie zdjęcia.

    WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Photo /srcdir:H:\Photo /dstdir:photo/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt

Narzędzie klienta Azure Importuj/Eksportuj polecenie PrepImport dla pierwszej sesji kopii kopiowaniu pliku:

    WAImportExport PrepImport /sk:<StorageAccountKey> /csas:<ContainerSas> /t: <TargetDriveLetter> [/format] [/silentmode] [/encrypt] [/bk:<BitLockerKey>] [/logdir:<LogDirectory>] /j:<JournalFile> /id:<SessionId> /srcfile:<SourceFile> /dstblob:<DestinationBlobPath> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]

Narzędzie klienta Azure Importuj/Eksportuj PrepImport polecenia Kopiuj kolejnych sesji kopiowaniu pliku:

    WAImportExport PrepImport /j:<JournalFile> /id:<SessionId> /srcfile:<SourceFile> /dstblob:<DestinationBlobPath> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]

**Pamiętaj**: Domyślnie zostaną zaimportowane dane w postaci bloków BLOB. Parametr /BlobType umożliwia importowanie danych jako blob strony. Na przykład Jeśli importujesz pliki wirtualny dysk twardy, które zostanie zainstalowany jako dyski na maszyn wirtualnych Azure, należy je zaimportować jako blob strony. Jeśli masz pewności, jakie obiektów blob typu użyć, można określić /blobType:auto a pomożemy określenia odpowiedniego typu. W tym przypadku wszystkie pliki wirtualnego dysku twardego i vhdx zostaną zaimportowane w postaci obiektów blob strony, a pozostałe zostaną zaimportowane w postaci bloków BLOB.

Dowiedz się więcej o korzystaniu z narzędzia klienta Azure Importuj/Eksportuj w [Przygotowywanie dysków twardych do zaimportowania](https://msdn.microsoft.com/library/dn529089.aspx).

Ponadto zapoznaj się z [Przykładowego przepływu pracy do przygotowania dysków twardych o zadania importu](https://msdn.microsoft.com/library/dn529097.aspx) bardziej szczegółowe instrukcje krok po kroku.  

### <a name="create-the-import-job"></a>Tworzenie zadania importu

1.  Gdy zostały przygotowane na dysku, przejdź do swojego konta przestrzeni dyskowej w [klasycznym portal](https://manage.windowsazure.com) i wyświetlić pulpit nawigacyjny. W obszarze **Szybkie skrócie**kliknij przycisk **Utwórz zadanie importu**. Przejrzyj czynności i zaznacz pole wyboru, aby wskazać przygotowano dysku i że masz dostępne pliku dziennika dysk.

2.  W kroku 1 Podaj informacje kontaktowe dla osoby odpowiedzialnej za to zadanie importu i prawidłowy adres zwrotny. Jeśli chcesz zapisać dane pełny dziennik dla zadania importu, zaznacz opcję, aby **zapisać pełny dziennik w kontenerze obiektów blob Moje "waimportexport"**.

3.  W kroku 2 Przekaż pliki dziennika dysk otrzymanych podczas kroku przygotowanie dysk. Należy przekazać jeden plik dla każdego dysku, które zostały przygotowane.

    ![Tworzenie zadania importu — krok 3](./media/storage-import-export-service/import-job-03.png)

4.  W kroku 3 wprowadź opisową nazwę zadania importu. Należy zauważyć, że wprowadzona nazwa może zawierać tylko małe litery, cyfry, łączniki i podkreślenia, musi rozpoczynać się od litery i nie może zawierać spacji. Zostanie użyta nazwa wybrane do śledzenia zadań, gdy są one w toku i po zakończeniu.

    Następnie wybierz obszar centrum danych z listy. Obszar centrum danych będzie wskazywać centrum danych i adres, do którego możesz wysłać pakietu. Zobacz często zadawane pytania poniżej Aby uzyskać więcej informacji.

5.  W kroku 4 wybierz operatora zwrotu z listy, a następnie wprowadź numer swojego konta przewoźnika. Przy użyciu tego konta Microsoft wysłać dyski powrotem po zakończeniu zadania importu.

    Jeśli masz numer identyfikacyjny, wybierz operatora dostarczenia na liście, a następnie wprowadź numer identyfikacyjny.

    Jeśli nie został numer identyfikacyjny jeszcze, wybierz pozycję **dam informację wysyłki dla tego zadania importu po I dostarczono Mój pakiet**, następnie dokończ proces importu.

6. Aby wprowadzić numer identyfikacyjny po dostarczono pakietu, wróć do strony **Importuj/Eksportuj** dla Twojego konta miejsca do magazynowania w portalu klasyczny wybierz zadanie z listy, a następnie wybierz polecenie **Informacje o wysyłki**. Nawigowanie za pomocą kreatora, a następnie wprowadź numer identyfikacyjny w kroku 2.

    Jeśli numer nie zostanie zaktualizowana w ciągu 2 tygodni Tworzenie zadania, zadanie wygaśnie.

    Jeśli zadanie jest w stanie tworzenie, wysyłki lub transferowanie, możesz zaktualizować carrier numer swojego konta w obszarze krok 2 kreatora. Gdy zadanie jest w stanie opakowań, nie można zaktualizować numeru konta przewoźnika dla tego zadania.

7. Możesz śledzić postęp zadania na pulpicie nawigacyjnym portalu. Zobacz, ich zadanie w poprzedniej sekcji znaczenie, [wyświetlając informacji o stanie zadania](#viewing-your-job-status).

## <a name="how-to-create-an-export-job"></a>Jak utworzyć zadania eksportu?

Tworzenie zadania eksportu, aby powiadomić usługi Importuj/Eksportuj, że użytkownik będzie można wysyłki jeden lub więcej dysków pusty centrum danych tak, aby danych może być wyeksportowane z Twojego konta miejsca do magazynowania dysków i dysków, a następnie wysłany do Ciebie.

### <a name="prepare-your-drives"></a>Przygotowywanie dysków

Zaleca się następujące testy wstępne dotyczące przygotowywania dysków zadanie eksportu:

1. Sprawdź liczbę dysków wymagane za pomocą polecenia PreviewExport narzędzie Azure Importuj/Eksportuj. Aby uzyskać więcej informacji zobacz [Podgląd sterują zastosowania dla zadania eksportu](https://msdn.microsoft.com/library/azure/dn722414.aspx). Pomaga Podgląd zastosowania dysk dla obiektów blob wybranej na podstawie rozmiaru dysków, który zamierzasz użyć.

2. Sprawdź, czy użytkownik może odczytu/zapisu na dysku twardym, dostarczonej zadania eksportu.

### <a name="create-the-export-job"></a>Tworzenie zadania eksportu

1.  Aby utworzyć zadanie eksportu, przejdź do swojego konta miejsca do magazynowania w [klasycznym portalu](https://manage.windowsazure.com)i wyświetlić pulpit nawigacyjny. W obszarze **Szybkie skrócie**kliknij przycisk **Utwórz zadania eksportu** i postępuj zgodnie z instrukcjami kreatora.

2.  W kroku 2 informacji kontaktowych osoby odpowiedzialnej dla tego zadania eksportu. Jeśli chcesz zapisać dane pełny dziennik zadania eksportu, zaznacz opcję, aby **zapisać pełny dziennik w kontenerze obiektów blob Moje "waimportexport"**.

3.  W kroku 3 określić, które chcesz wyeksportować z Twojego konta miejsca do magazynowania na pusty dysk lub dyski dane obiektów blob. Istnieje możliwość Eksportuj wszystkie dane obiektów blob na koncie miejsca do magazynowania lub można określić, które obiektów blob lub zestawy obiektów blob do wyeksportowania.

    Aby określić obiektów blob do wyeksportowania, selektor **Równe** i określ ścieżkę względną do obiektów blob, począwszy od nazwy kontenera. Umożliwia określenie kontenera root *$root* .

    Aby określić wszystkie obiekty BLOB, zaczynając od prefiksu, selektor **Rozpoczyna się od** i określ prefiks, począwszy od ukośnik "/". Prefiks może być prefiksu nazwa kontenera, nazwa kontenera wykonane lub nazwę kontenera ukończone, a następnie prefiks nazwy obiektów blob.

    W poniższej tabeli pokazano przykłady prawidłowych obiektów blob ścieżek:

    Selektor|Ścieżka obiektów blob|Opis
    ---|---|---
    Zaczyna się od|/|Eksportuje wszystkie obiekty BLOB na koncie miejsca do magazynowania
    Zaczyna się od|/$root-|Eksportuje wszystkie obiekty BLOB w kontenerze katalogu głównego
    Zaczyna się od|/Book|Eksportuje wszystkie obiekty BLOB w dowolnym kontenerze, który zaczyna się od prefiksu **książki**
    Zaczyna się od|/Music/|Eksportuje wszystkie obiekty BLOB w kontenerze **muzyki**
    Zaczyna się od|/ muzyki/miłości|Eksportuje wszystkie obiekty BLOB w kontenerze **muzyki** , które zaczynają się od prefiksu **lubisz**
    Równa się|$root/logo.bmp|Eksportowanie obiektu blob **logo.bmp** w kontenerze katalogu głównego
    Równa się|videos/Story.mp4|Eksportowanie obiektu blob **story.mp4** w kontenerze **klipów wideo**

    Należy podać ścieżek obiektów blob w prawidłowe formaty, aby uniknąć błędów podczas przetwarzania, jak pokazano na tym zrzucie ekranu.

    ![Tworzenie zadania eksportu — krok 3](./media/storage-import-export-service/export-job-03.png)


4.  W kroku 4 wprowadź opisową nazwę zadania eksportu. Wprowadzona nazwa może zawierać tylko małe litery, cyfry, łączniki i podkreślenia, musi rozpoczynać się od litery i nie może zawierać spacji.

    Obszar centrum danych będzie wskazywać centrum danych, do której wysyłasz musi opakowaniu. Zobacz często zadawane pytania poniżej Aby uzyskać więcej informacji.

5.  W kroku 5 wybierz operatora zwrotu z listy, a następnie wprowadź numer swojego konta przewoźnika. Przy użyciu tego konta Microsoft wysłać dysków powrotem po zakończeniu pracy eksportu.

    Jeśli masz numer identyfikacyjny, wybierz operatora dostarczenia na liście, a następnie wprowadź numer identyfikacyjny.

    Jeśli nie został numer identyfikacyjny jeszcze, wybierz pozycję **dam informację wysyłki dla tego zadania eksportu po I dostarczono Mój pakiet**, następnie dokończ proces eksportowania.

6. Aby wprowadzić numer identyfikacyjny po dostarczono pakietu, wróć do strony **Importuj/Eksportuj** dla Twojego konta miejsca do magazynowania w portalu klasyczny wybierz zadanie z listy, a następnie wybierz polecenie **Informacje o wysyłki**. Nawigowanie za pomocą kreatora, a następnie wprowadź numer identyfikacyjny w kroku 2.

    Jeśli numer nie zostanie zaktualizowana w ciągu 2 tygodni Tworzenie zadania, zadanie wygaśnie.

    Jeśli zadanie jest w stanie tworzenie, wysyłki lub transferowanie, możesz zaktualizować carrier numer swojego konta w obszarze krok 2 kreatora. Gdy zadanie jest w stanie opakowań, nie można zaktualizować numeru konta przewoźnika dla tego zadania.

    > [AZURE.NOTE] Jeśli obiektów blob do wyeksportowania jest używany podczas kopiowania na dysku twardym komputera, usługa Azure Importuj/Eksportuj zrób migawkę to i kopiowanie migawki.

7.  Możesz śledzić postęp zadania na pulpicie nawigacyjnym w portalu klasyczny. Zobacz, co oznacza stan każdego zadania w poprzedniej sekcji na "Wyświetlanie informacji o stanie zadania".

8.  Po otrzymaniu dyski z wyeksportowane dane można wyświetlać i skopiuj kluczy funkcji BitLocker wygenerowane przez usługę stacji dysków. Przejdź do swojego konta miejsca do magazynowania w portalu klasyczny, a następnie kliknij kartę Importuj/Eksportuj. Z listy wybierz zadanie eksportu, a następnie kliknij przycisk wyświetlanie kluczy. Kluczy funkcji BitLocker są wyświetlane, tak jak pokazano poniżej:

    ![Wyświetlić kluczy funkcji BitLocker dla zadania eksportu](.\media\storage-import-export-service\export-job-bitlocker-keys.png)

Przejdź do poniższej sekcji często zadawane pytania dotyczące jako zawiera często zadawane pytania, które klienci wystąpić podczas korzystania z tej usługi.

## <a name="frequently-asked-questions"></a>Często zadawane pytania ##


**Jak długo trwa kopiowanie danych po dyskach osiągnie centrum danych?**

Czas na kopiowanie zmienia się w zależności od różnych czynników, takich jak typ zadania, typu i rozmiaru danych kopiowanych, rozmiar dysków podano i istniejące obciążenie pracą. Go się różnić od kilku dni do kilku tygodni, w zależności od następujących czynników. Dlatego jest trudne zapewnić ogólne szacowanie. Usługa próbuje zoptymalizować zadania, kopiując wiele dysków równolegle, jeśli to możliwe. Jeśli masz zadanie krytyczne Importuj/Eksportuj czasu, skontaktuj się z nami oceny.

**Kiedy używać usługi Importuj/Eksportuj Azure?**
Jeden należy rozważyć przy użyciu Azure Importuj Eksportuj, jeśli przekazywania lub pobierania sieci trwa około oszacowania więcej niż 7 dni. Aby obliczyć, jak długo potrwa przy użyciu dowolnego kalkulatora online lub, [pobierając](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/archive/master.zip) plik umieszczony w Azure importowanie eksportowanie pozostałych interfejsu API próbie w repozytorium Azure próbki w [Kalkulatora szybkość transferu danych](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html). Nie jest dokładne obliczenie, ale tylko oznaczeniem drukowania.

**Czy można używać usługi Azure Importuj/Eksportuj za pomocą Menedżera zasobów magazynowania konta?**

Nie, nie można kopiować danych do lub z konta magazynu Menedżera zasobów bezpośrednio przy użyciu usługi Azure Importuj/Eksportuj. Usługa obsługuje tylko konta klasyczny miejsca do magazynowania. Pomoc dotycząca konta magazynu Menedżera zasobów będzie już wkrótce. Alternatywnie importowanie danych do konta klasyczny miejsca do magazynowania i przeprowadzić migrację z kontem miejsca do magazynowania Menedżera zasobów przy użyciu [AzCopy](storage-use-azcopy.md) lub CopyBlob.

**Czy można skopiować pliki Azure za pomocą usługi Azure Importuj/Eksportuj?**

Nie, usługa Azure Importuj/Eksportuj obsługuje tylko blob blok i obiektów blob strony. Wszystkie inne typy miejsca do magazynowania w tym pliki Azure, tabele i kolejek nie są obsługiwane.

**Usługa Azure Importuj/Eksportuj jest dostępna dla dostawcy subskrypcji?**

Nie, usługa Azure Importuj/Eksportuj nie obsługuje subskrypcje dostawcy. Obsługa zostaną dodane w przyszłości.

**Krok przygotowania dysków dla zadania importu można pominąć lub można przygotować dysk bez kopiowania?**

Należy przygotować dowolnego dysku, który chcesz wysłać do importowania danych przy użyciu narzędzia klienta Azure Importuj/Eksportuj. Aby skopiować dane na dysku, należy użyć narzędzia klienta.

**Należy wykonać dowolną przygotowanie dysku, podczas tworzenia zadania eksportu?**

Zaleca się bez, ale niektóre wstępnej kontroli. Sprawdź liczbę dysków wymagane za pomocą polecenia PreviewExport narzędzie Azure Importuj/Eksportuj. Aby uzyskać więcej informacji zobacz [Podgląd sterują zastosowania dla zadania eksportu](https://msdn.microsoft.com/library/azure/dn722414.aspx). Pomaga Podgląd zastosowania dysk dla obiektów blob wybranej na podstawie rozmiaru dysków, który zamierzasz użyć. Ponadto sprawdź, czy można odczytywać i zapisywać na dysku twardym, dostarczonej zadania eksportu.

**Co się stanie, jeśli obsługiwane wymagania przypadkowo wysyłać dysk twardy, która nie jest zgodna?**

Centrum danych Azure zwróci dysk, która nie jest zgodna z wymaganiami obsługiwane dla Ciebie. Jeśli tylko niektóre dyski w pakiecie wymagań pomocy technicznej tych dysków będą przetwarzane i dyski, które nie spełnia wymagań zostaną zwrócone do Ciebie.

**Czy można anulować Moje zadania?**

Możesz anulować zadanie podczas jego stan to tworzenia lub wysyłki.

**Jak długo trwa wyświetlanie stanu zadań wykonanych w klasycznym portalu**

Możesz wyświetlać stan ukończonych zadań w ciągu 90 dni. Ukończonych zadań, zostaną usunięte po 90 dni.

**Gdy chcę importowanie lub eksportowanie więcej niż 10 dysków, co należy zrobić?**

Jedną operację importowania lub eksportowania zadania można odwoływać się tylko 10 dysków w jednym zadaniu usługi Importuj/Eksportuj. Jeśli użytkownik chce wysłać więcej niż 10 dysków, możesz utworzyć wiele zadań. Dyski, które są skojarzone z to samo zadanie musi być wysyłane razem w pakiecie.

**Do czego służy karta USB SATA, który nie znajduje się na liście zalecanych?**

Jeśli masz konwerter, który nie znajduje się powyżej, możesz spróbować uruchomienia narzędzia Importuj/Eksportuj Azure za pomocą usługi konwertera przygotowywanie dysku i sprawdzić, czy działa przed zakupem konwertera obsługiwane.

**Czy usługa formatowania dysków przed ich zwracania?**

Wartość nie. Wszystkie dyski są szyfrowane przy użyciu funkcji BitLocker.

**Od firmy Microsoft może zakupić dyski Importuj/Eksportuj zadań?**

Wartość nie. Musisz wysłać dysków dla obu importowanie i eksportowanie zadań.

**Po zakończeniu zadania importu będzie Moje dane jak wygląda w oknie konta miejsca do magazynowania? Zostaną zachowane Moje hierarchii katalogów?**

Podczas przygotowywania dysku twardego zadania importu, miejsce docelowe jest określony przez /dstdir: parametru. Jest to kontener docelowy na koncie miejsca do magazynowania, do których są kopiowane dane z dysku twardego. W tym kontenerze docelowy katalogów wirtualnych są tworzone foldery z dysku twardego i obiektów blob są tworzone pliki.

**Jeśli dysk zawiera pliki znajdują się już na moim koncie miejsca do magazynowania, usługę zastąpią istniejące blob na moim koncie miejsca do magazynowania?**

Podczas przygotowywania dysku, możesz określić pliki docelowe powinny być zastąpione czy ignorowane przy użyciu parametru o nazwie /Disposition: < zmienić | zastąpić nie | zastąpić >. Domyślnie usługa będzie zmienić nazwę nowych plików zamiast zastąpienie istniejącego obiektów blob.

**Narzędzie Azure Importuj/Eksportuj do klienta jest zgodny z 32-bitowych systemów operacyjnych?**
Wartość nie. Narzędzie do klienta tylko jest zgodny z 64-bitowych systemów operacyjnych Windows. Zapoznaj się sekcji systemów operacyjnych [wymagania wstępne](#pre-requisites) , aby uzyskać pełną listę obsługiwanych wersji systemu operacyjnego.

**Należy umieścić cokolwiek innego niż dysk twardy Mój pakiet?**

Wyślij tylko dysku twardego. Nie zawiera elementów takich jak power dostaw kable lub USB.

**Czy muszę wysłać Moje dysków przy użyciu FedEx lub przez firmę DHL?**

Można wysłać dyski centrum danych za pomocą każdego znane przewoźnika, takie jak FedEx, przez firmę DHL, UPS lub US pocztę. Jednak dla wysyłki dyski powrót do centrum danych, należy podać liczbę konto FedEx w Stanach Zjednoczonych i Unii Europejskiej lub numer konta przez firmę DHL w regionach Azji i Australii.

**Czy istnieją jakiekolwiek ograniczenia z wysyłki międzynarodowych dysk?**

Uwaga nośnik fizyczny, który jest dostarczany może być konieczne krzyżowe międzynarodowe obramowania. Użytkownik jest odpowiedzialny za zapewnienie, że Twoje nośnik fizyczny i dane są zaimportowane i/lub wyeksportowane zgodnie z obowiązującym prawem. Przed wysłaniem nośnik fizyczny, skontaktuj się z usługi doradców, aby zweryfikować, że Twoje multimediów i dane można prawnie wysłane do Centrum określonych danych. Dzięki temu zapewnienie osiągnie firmy Microsoft, w odpowiednim czasie.

**Podczas tworzenia zadania, adres wysyłkowy znajduje się inna niż lokalizacja konta Moje miejsca do magazynowania. Co należy zrobić?**

Niektóre konta lokalizację są mapowane do wysyłki alternatywnej lokalizacji. Już dostępne lokalizacje wysyłkowe mogą być tymczasowo przekształcane do alternatywnej lokalizacji. Zawsze sprawdzić adres wysyłkowy podany podczas tworzenia zadania przed wysłaniem dysków.

**Dlaczego mój status zadania w klasycznym portalu powiedzieć "wysyłki" po wyświetleniu w witrynie sieci Web Carrier Mój pakiet został dostarczony?**

Zmiany stanu w portalu klasyczny z wysyłki do przeniesienia, kiedy dysk przetwarzania rozpoczyna się. Jeśli dysk osiągnięciu funkcji, ale nie rozpoczęło przetwarzania, stan zadania będzie wyświetlana jako wysyłki.

**Gdy dostawy dysk, przewoźnika monituje o podanie nazwy tego kontaktu centrum danych i numer telefonu. Co powinien zawierać?**

Numer telefonu są dostępne podczas tworzenia zadania. Jeśli potrzebujesz nazwę kontaktu, skontaktuj się z nami u waimportexport@microsoft.com i firma Microsoft mają dostęp do tych informacji.

**Do kopiowania pliku PST skrzynek pocztowych i danych programu SharePoint do usługi Office 365 można użyć usługi Azure Importuj/Eksportuj?**

Zajrzyj do [plików PST importu lub danych programu SharePoint do usługi Office 365](https://technet.microsoft.com/library/ms.o365.cc.ingestionhelp.aspx).

**Do kopiowanie mojej kopii zapasowych w trybie offline z usługą Azure kopii zapasowej można użyć usługi Azure Importuj/Eksportuj?**

Zajrzyj do [przepływu pracy w trybie Offline kopii zapasowych w kopii zapasowej Azure](../backup/backup-azure-backup-import-export.md).

## <a name="see-also"></a>Zobacz też:

- [Aby skonfigurować narzędzie klienta Azure Importuj/Eksportuj](https://msdn.microsoft.com/library/dn529112.aspx)

- [Przesyłanie danych za pomocą narzędzia wiersza polecenia AzCopy](storage-use-azcopy.md)

- [Importowanie Azure eksportu pozostałych interfejsu API próbki](https://azure.microsoft.com/documentation/samples/storage-dotnet-import-export-job-management/)
