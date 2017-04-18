<properties
    pageTitle="Wprowadzenie do przechowywania | Microsoft Azure"
    description="Omówienie miejscem do magazynowania Azure, przechowywanie danych online firmy Microsoft w chmurze. Dowiedz się, jak używać jest najlepszym rozwiązaniem miejsca do magazynowania chmury dostępne w aplikacji."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/25/2016"
    ms.author="tamram"/>

# <a name="introduction-to-microsoft-azure-storage"></a>Wprowadzenie do magazynu platformy Microsoft Azure

## <a name="overview"></a>Omówienie

Azure magazyn jest masowej chmury nowoczesne aplikacje, które korzystają z wytrzymałości, dostępność i skalowalność na potrzeby swoich klientów. Czytając ten artykuł, deweloperzy Informatycy i decyzji biznesowych twórcy informacje o można:

- Co to jest Azure miejsca do magazynowania i jak można wykonać zaletą go w chmurze, urządzeń przenośnych, server i aplikacjach komputerowych
- Jakie typy danych można przechowywać z usługami magazyn Azure: blob danych (obiekt), dane tabeli NoSQL, wiadomości w kolejce i udziały plików.
- Jak odbywa się dostęp do danych w magazynie Azure
- Jak dane magazyn Azure wykonany trwałego za pośrednictwem nadmiarowości i replikacji
- Dokąd przejść dalej do utworzenia pierwszej aplikacji magazyn Azure

Aby rozpocząć pracę z nośnikami Azure szybko, zobacz [Wprowadzenie do przechowywania Azure w pięć minut](storage-getting-started-guide.md).

Aby uzyskać szczegółowe informacje o narzędziach biblioteki i inne zasoby dotyczące pracy z miejscem do magazynowania Azure, zobacz poniższe [Czynności](#next-steps) .

## <a name="what-is-azure-storage"></a>Co to jest Azure miejsca do magazynowania?

Obliczanie umożliwia nowych scenariuszy do zastosowań wymagających skalowalna, trwałe i wysokiej dostępności danych — czyli dokładnie, dlaczego Microsoft opracowanych magazyn Azure w chmurze. Oprócz pozwalający programistom na tworzenie dużych aplikacji do obsługi nowych scenariuszy magazyn Azure udostępnia foundation miejsca do magazynowania dla Azure maszyn wirtualnych, dalsze dowód jego niezawodności.

Azure miejsca do magazynowania jest znacznym stopniu skalowalna, dzięki czemu można przechowywać i proces setki terabajtów danych w celu obsługi scenariuszy duży danych wymaganych przez analizy naukowych, finansowych i aplikacji. Lub mogą zawierać niewielkich ilości danych wymaganych do witryny sieci Web małej firmy. Miejsce, w którym przypada potrzeb, możesz zapłacić tylko dane, które przechowujesz. Azure magazynowania obecnie przechowuje dziesiątki trillions obiektów unikatowe klienta i średnio obsługuje miliony żądania na sekundę.

Azure miejsca do magazynowania jest elastyczne, dzięki czemu projektowanie aplikacji dla dużej liczby odbiorców globalnej i skalowanie tych aplikacji w razie potrzeby — zarówno pod względem ilości danych przechowywanych i liczba wnioski przed nim. Płatność tylko w przypadku możesz użyć, i tylko wtedy, gdy go używać.

Magazyn Azure używa podziału automatyczne system automatycznie ładowania sald danych oparty na ruch. Oznacza to, że jako wymagania dotyczące zwiększania Twojej aplikacji, Magazyn Azure automatycznie przydziela odpowiednie zasoby, aby osiągnięte.

Azure miejsca do magazynowania jest dostępne w miejscu na świecie, z dowolnego typu aplikacji, czy jest uruchomiony w chmurze, na komputerze, lokalnego serwera lub telefonu komórkowego lub tabletu urządzenia. Za pomocą magazyn Azure w przenośnych scenariuszach miejsce, w którym są przechowywane podzestawu danych na tym urządzeniu aplikacji i synchronizuje je przy użyciu pełnego zestawu danych przechowywanych w chmurze.

Magazyn Azure obsługuje klientów korzystających z rozwoju wygodny zestaw systemów operacyjnych (w tym Windows i Linux) i różnych języków programowania (w tym .NET, języka Java, Node.js, Python, dopiskiem, PHP i C++ i urządzeń przenośnych języków programowania). Magazyn Azure udostępnia również zasobów danych za pośrednictwem prostych pozostałych interfejsów API, które są dostępne dla każdego klienta możliwość wysyłania i odbierania danych za pośrednictwem protokołu HTTP/HTTPS.

Azure magazynowania Premium udostępnia obsługę dysku wysokiej wydajności i małych opóźnień intensywnie obciążenia We/Wy uruchomione na maszyn wirtualnych Azure. Z miejscem do magazynowania Premium Azure można dołączyć wiele dysków danych trwałych maszyn wirtualnych i skonfigurować je zgodnie z potrzebami wydajności. Każdy dysk danych jest przechowywana w dysk SSD w magazynie Premium Azure Maksymalna wydajność wejścia/wyjścia. Zobacz [miejsca do magazynowania Premium: wysokiej wydajności miejsca do magazynowania dla obciążenia maszyn wirtualnych Azure](storage-premium-storage.md) uzyskać więcej szczegółowych informacji.

## <a name="introducing-the-azure-storage-services"></a>Wprowadzenie do usług Azure magazynu

Magazyn Azure oferuje następujące cztery usługi: Blob miejsca do magazynowania, Magazyn tabel, magazyn kolejek i przechowywania plików.

- Magazyn obiektów blob są przechowywane dane niestrukturalne obiektu. Obiektów blob może być dowolnego typu tekst lub dane binarne, takich jak dokument, plik lub Instalator aplikacji. Magazyn obiektów blob jest również określane jako miejsca przechowywania obiektu.
- Magazyn tabel przechowuje zestawy danych strukturalnych. Magazyn tabel jest przechowywanie danych atrybutu klucza NoSQL, co umożliwia szybki rozwój i szybki dostęp do dużych ilości danych.
- Magazyn kolejek znajdują się wiarygodnych wiadomości, przetwarzanie przepływu pracy i komunikacji między składnikami usług w chmurze.
- Magazyn plików udostępnia udostępnionych miejsca do magazynowania dla starszych aplikacji za pomocą protokołu SMB standardowy. Azure maszyn wirtualnych i usług w chmurze udostępnić plik danych przez składniki aplikacji za pomocą zainstalowanych udziałów i aplikacje lokalnego można uzyskiwać dostęp do danych pliku w udziale za pośrednictwem usługi plik interfejsu API usługi REST.

Konto Azure miejsca do magazynowania jest bezpieczne konto, który umożliwia dostęp do usług w magazynie Azure. Konta magazynu zawiera unikatowych nazw dla zasobów miejsca do magazynowania. Na poniższej ilustracji pokazano relacje między zasoby Azure miejsca do magazynowania na koncie miejsca do magazynowania:

![Zasoby Azure miejsca do magazynowania](./media/storage-introduction/storage-concepts.png)

[AZURE.INCLUDE [storage-account-types-include](../../includes/storage-account-types-include.md)]

[AZURE.INCLUDE [storage-versions-include](../../includes/storage-versions-include.md)]

## <a name="blob-storage"></a>Magazyn obiektów blob

Dla użytkowników z dużych ilości danych niestrukturalne obiektu do przechowywania w chmurze magazyn obiektów Blob oferuje efektywne pod względem kosztów i skalowalność rozwiązanie. Magazyn obiektów Blob umożliwia przechowywanie zawartości, takich jak:

- Dokumenty
- Danych społecznościowych, takich jak zdjęcia, klipy wideo, muzyka i blogów
- Kopie zapasowe plików, komputerów, baz danych i urządzeń
- Obrazy i tekst dla aplikacji sieci web
- Dane konfiguracyjne aplikacje w chmurze
- Duży danych, takich jak dzienniki i innych dużych zestawów danych

Co obiektów blob są zorganizowane w kontenerze. Kontenery umożliwiają także przydatne, aby przypisać zasady zabezpieczeń do grupy obiektów. Konto miejsca do magazynowania może zawierać dowolną liczbę kontenerów i kontenera może zawierać dowolną liczbę obiektów blob maksymalnie 500 TB limitem konta miejsca do magazynowania.  

Blob miejsca do magazynowania oferuje trzy typy obiektów blob, blokowanie obiektów blob, dołączyć obiektów blob i blob strony (dyski).

- Blokowanie obiektów blob są zoptymalizowane dla strumieniowego przesyłania i przechowywania obiektów w chmurze, a są dobrym rozwiązaniem do przechowywania dokumentów, pliki multimedialne, kopie zapasowe itp.
- Dołączanie obiektów blob są podobne do obiektów blob blok, ale są zoptymalizowane pod kątem dołączyć operacje. Dołączanie obiektów blob można aktualizować tylko przez dodanie nowego bloku na końcu. Dołączanie obiektów blob są dobrym rozwiązaniem dla scenariuszy, takich jak rejestrowanie, miejsce, w którym trzeba napisać tylko do końca to nowe dane.
- Blob strony są zoptymalizowane pod kątem reprezentującą dysków IaaS i pomocniczych losowe zapisuje, a może być w rozmiarze do 1 TB. Sieć Azure maszyn wirtualnych dołączonych IaaS dysk jest przechowywana jako blob strony wirtualnego dysku twardego.

Dla bardzo dużych zestawów danych, gdzie ograniczenia sieci wprowadzić przekazywania lub pobierania danych z magazynem obiektów Blob przez sieć nierealne można wysłać dysk twardy do firmy Microsoft do importowania lub eksportowania danych bezpośrednio z poziomu Centrum danych. Zobacz [Używanie usługi Microsoft Azure Importuj/Eksportuj do przesyłania danych do Blob miejsca do magazynowania](storage-import-export-service.md).

## <a name="table-storage"></a>Magazyn tabel

Nowoczesne aplikacje często zażądać magazynów większa skalowalność i elastyczność niż ich poprzednie wersje oprogramowania jest wymagane. Magazyn tabel oferuje wysokiej dostępności, znacznym stopniu skalowalna przestrzeni dyskowej, aby aplikacja automatycznie można skalować zapotrzebowania użytkownika. Magazyn tabel jest sklepu atrybutu klucza NoSQL firmy Microsoft — został schemaless projekt, dzięki czemu inne niż tradycyjny relacyjnych baz danych. Z magazynu schemaless danych jest łatwe do dostosowania danych jako na potrzeby usługi evolve aplikacji. Magazyn tabel jest łatwe w użyciu, więc deweloperów można szybko tworzyć aplikacje. Dostęp do danych jest szybkie i efektywne pod względem kosztów dla wszystkich typów aplikacji.  Magazyn tabel jest zazwyczaj znacząco niższe w polu Koszt tradycyjna SQL podobne ilości danych.

Magazyn tabel jest magazynem atrybutu klucza, co oznacza przechowywanie każdej wartości w tabeli o nazwie wpisany właściwości. Nazwa właściwości może służyć do filtrowania i określanie kryteriów wyboru. Kolekcja właściwości i ich wartości obejmują jednostki. Ponieważ Magazyn tabel jest schemaless, dwa podmioty w tej samej tabeli może zawierać różne kolekcje właściwości, a te właściwości mogą być różnych typów.

Magazyn tabel służy do przechowywania elastyczne zestawy danych, takich jak dane użytkownika dla aplikacji sieci web, książki adresowe, informacje o urządzeniach i inny rodzaj metadanych, która wymaga usługi.  Mogą zawierać dowolną liczbę obiektów w tabeli, a konto miejsca do magazynowania może zawierać dowolną liczbę tabel, aż do limitem konta miejsca do magazynowania.

Podobnie jak obiektów blob i kolejek, deweloperzy mogą zarządzać, a tabela programu access przechowywanie przy użyciu standardowych pozostałych protokoły, jednak magazyn tabel również obsługuje protokołu OData upraszczanie zaawansowane możliwości kwerend i włączając zarówno JSON i AtomPub (oparte na języku XML) formatów.

W przypadku aplikacji internetowych dzisiejszą NoSQL baz danych, takich jak magazyn tabel oferują popularne sposobem tradycyjnych relacyjnych baz danych.

## <a name="queue-storage"></a>Magazyn kolejek

W projektowaniu aplikacji skali, składniki aplikacji często są odłączona, tak aby można skalować niezależnie. Magazyn kolejek rozwiązaniem jest wiarygodnych wiadomości dla asynchroniczne komunikacji między składnikami aplikacji, czy korzystają w chmurze, na komputerze, na serwerze lokalnym lub na urządzeniu przenośnym. Magazyn kolejek umożliwia zarządzanie zadaniami asynchroniczne i tworzenie przepływów pracy procesu.

Konto miejsca do magazynowania może zawierać dowolną liczbę kolejek. Kolejka może zawierać dowolną liczbę wiadomości, limitem konta miejsca do magazynowania. Pojedyncze wiadomości mogą być maksymalnie 64 KB.

## <a name="file-storage"></a>Magazyn plików

Azure magazyn plików oferuje opartej na chmurze udziały plików SMB tak, aby można migrować starsze aplikacje korzystające z udziały plików Azure szybko i bez ponownego kosztów. Magazyn plików Azure aplikacje Azure maszyn wirtualnych lub usług w chmurze można zainstalować udziału plików w chmurze, tak jak aplikacji klasycznej instaluje typowe udziałem SMB. Dowolna liczba składniki aplikacji można, a następnie zainstalować i jednocześnie dostępu do udziału miejsca do magazynowania plików.

Ponieważ udziału miejsca do magazynowania plików jest udziału plików SMB standardowy, aplikacje platformy Azure dostęp do danych w sekcji udostępnianie przez system plików interfejsy API we/wy. Można w związku z tym używać ich istniejący kod i umiejętności potrzebne do migrowania istniejących aplikacji. Informatycy można użyć poleceń cmdlet środowiska PowerShell do tworzenia, zainstalować i zarządzać nimi udziały plików miejsca do magazynowania w ramach zarządzania aplikacjami Azure.

Przykład innych usług Azure miejsca do magazynowania magazyn plików udostępnia interfejsu API usługi REST uzyskiwania dostępu do danych w udziale. Lokalna aplikacji może wywołać magazyn plików interfejsu API usługi REST, aby uzyskać dostęp do danych w udziale plików. W ten sposób przedsiębiorstwa można przeprowadzić migrację niektórych starszych aplikacji Azure i kontynuować działanie innym użytkownikom w obrębie własnej organizacji. Należy zauważyć, że instalowanie udziału plików jest dostępna tylko dla aplikacji działających w Azure; Aplikacja lokalnego może dostęp tylko do udziału pliku za pośrednictwem interfejsu API usługi REST.

Rozłożone aplikacje mogą również używać magazyn plików do przechowywania i udostępniania danych przydatny aplikacji i rozwoju i narzędzia do testowania. Na przykład aplikacja może przechowywać pliki konfiguracji i diagnostyczne danych, takich jak dzienniki, metryki i awarię dokonuje zrzutu w pliku miejsca do magazynowania udostępnić tak, aby były dostępne dla wielu maszyn wirtualnych i role. Deweloperzy i Administratorzy mogą zawierać narzędzia, które są potrzebne do tworzenia lub zarządzać aplikacją w udziale miejsca do magazynowania plików, które są dostępne dla wszystkich składników, zamiast instalowania ich na każdej maszyn wirtualnych lub wystąpienia roli.

## <a name="access-to-blob-table-queue-and-file-resources"></a>Dostęp do obiektów Blob, tabeli kolejki i zasobów plików

Domyślnie tylko właściciel konta miejsca do magazynowania ma dostęp do konta magazynu zasobów. Zabezpieczania danych muszą zostać uwierzytelnione każdego żądania na zasoby na Twoim koncie. Uwierzytelnianie oparte na modelu klucz udostępniony. Obiektów blob można również skonfigurować do obsługi uwierzytelniania anonimowego.

Konta miejsca do magazynowania przypisano dwa klucze prywatny dostęp przy tworzeniu, które służą do uwierzytelniania. Dwa klucze gwarantuje, aplikacja nadal dostępne, gdy regularnie Generuj klucze jako wspólnych ćwiczenia zarządzania kluczami zabezpieczeń.

Jeśli potrzebujesz umożliwić użytkownikom kontroli dostępu do zasobów magazynowania, możesz utworzyć podpis udostępniania. Podpis udostępniania (SA) jest tokenu, który można dołączać do adresu URL, który umożliwia delegowanej dostępu do zasobu miejsca do magazynowania. Każdego, kto ma token dostęp do zasobu, wskazywany przez nią przy użyciu uprawnień, który umożliwia określenie, na czas, że jest prawidłowy. Począwszy od wersji 2015-04-05, Magazyn Azure obsługuje dwa rodzaje udostępnienia podpisów: obsługi skojarzeń zabezpieczeń i skojarzeń zabezpieczeń konta.

Usługa skojarzeń zabezpieczeń deleguje dostępu do zasobu w co najmniej jeden z usług przechowywania: Usługa obiektów Blob, kolejki, tabeli lub pliku.

Konto skojarzeń zabezpieczeń deleguje dostęp do zasobów w jednej lub większej liczby usług magazynu. Możesz delegować dostęp do operacji poziomu usług, które nie są dostępne za pomocą usługi skojarzeń zabezpieczeń. Można również delegować dostęp do odczytu, pisanie i usuwanie operacji na kontenery obiektów blob, tabele, kolejkach i udziałach plików, które nie są dozwolone w usłudze skojarzenia zabezpieczeń.

Ponadto można określić, że kontenera i jego obiektów blob lub określonych obiektów blob, są dostępne dla publicznego dostępu. Jeśli zdecydujesz się kontenera lub obiektów blob jest publiczna, każdy może odczytać go anonimowo; Uwierzytelnianie nie jest wymagane.  Publiczne kontenerów i obiektów blob są przydatne zasoby takie jak multimedia i dokumenty, które są obsługiwane w witrynach publikowania.  Aby zmniejszyć opóźnień sieci dla odbiorców globalnej, pamięci podręcznej obiektów blob dane używane przez witryny sieci Web z sieci CDN Azure.

Aby uzyskać więcej informacji na temat podpisów udostępniania, zobacz [Przy użyciu podpisów dostępu do udostępnionej (SA)](storage-dotnet-shared-access-signature-part-1.md) . Więcej informacji na temat bezpiecznego dostępu do konta miejsca do magazynowania, zobacz [Zarządzanie anonimowe dostęp do odczytu kontenerów i obiektów blob](storage-manage-access-to-resources.md) i [uwierzytelniania dla usługi Azure miejsca do magazynowania](https://msdn.microsoft.com/library/azure/dd179428.aspx) .

## <a name="replication-for-durability-and-high-availability"></a>Replikacja wytrzymałości i wysokiej dostępności

Aby zapewnić wytrzymałości i wysoka dostępność zawsze replikacji danych na koncie Microsoft Azure miejsca do magazynowania. Replikacja kopiuje danych, w tym samym centrum danych lub do drugiego centrum danych, w zależności od wybranej opcji replikacji możesz wybrać. Replikacja chroni dane i zachowuje Twojej aplikacji wydłużyć czas wypadku awarii przejściowych sprzętu. Jeśli dane są replikowane do drugiego centrum danych, która również chroni dane przed losowych awariami w lokalizacji podstawowej.

Replikacja zapewnia, że Twoje konto miejsca do magazynowania spełnia [Umowa dotycząca poziomu usług (SLA) do przechowywania](https://azure.microsoft.com/support/legal/sla/storage/) nawet obliczu błędy. Zobacz gwarancje Umowa dotycząca poziomu usług, aby uzyskać informacje dotyczące magazynu Azure wytrzymałości i dostępności. 

Podczas tworzenia konta miejsca do magazynowania, można wybrać jedną z następujących opcji replikacji:  

- **Lokalnie zbędne miejsce do magazynowania (LRS).** Magazyn lokalnie zbędne obsługuje trzy kopie danych. LRS są replikowane trzy razy w jedno centrum danych w jednym regionie. LRS chroni dane z awarie sprzętu normalny, ale nie z uszkodzonego jedno centrum danych.  

    LRS jest oferowane w rabat. Maksymalna wytrzymałości zaleca używanie zbędne geo przestrzeni dyskowej, opisane poniżej.


- **Strefa zbędne miejsce do magazynowania (ZRS).** Zbędne strefy magazynowania obsługuje trzy kopie danych. ZRS są replikowane trzy razy w dwóch lub trzech urządzeń, w jednym regionie lub w dwóch regionów, dostarczając wytrzymałości wyższymi niż LRS. ZRS gwarantuje, że dane są trwałe w jednym regionie.  

    ZRS zapewnia wyższy poziom ważności niż LRS; dla maksymalna wytrzymałości zaleca użycie zbędne geo przestrzeni dyskowej, opisane poniżej.  

    > [AZURE.NOTE] ZRS jest obecnie dostępny tylko dla obiektów blob bloku, a jest tylko obsługiwana dla wersji 2014-02-14 lub nowszej.
    >
    > Po utworzeniu konta miejsca do magazynowania i zaznaczone ZRS, nie można przekonwertować go do używania innego typu replikacji lub odwrotnie.

- **Zbędne Geo przestrzeni dyskowej (GRS)**. GRS przechowuje sześć kopie danych. GRS dane są replikowane trzy razy w obszarze podstawowy i są replikowane trzy razy w regionie pomocniczej setki mila poza podstawowym regionu, zapewnia najwyższy poziom ważności. W przypadku awarii w regionie podstawowy magazyn Azure będzie przełączanie awaryjne do obszaru pomocniczą. GRS sprawia, że dane jest trwałe w dwóch oddzielnych regionów.

    Aby uzyskać informacji na temat głównego i pomocniczego par według regionów zobacz [Regionów Azure](https://azure.microsoft.com/regions/).

- **Dostęp do odczytu zbędne geo miejsca do magazynowania (Pomoc Zdalna GRS)**. Replikuje dane na pomocniczym lokalizacji geograficznej i również zapewnia dostęp do danych w lokalizacji pomocniczej odczytu magazynowania zbędne geo dostęp do odczytu. Miejsca do magazynowania zbędne geo dostęp do odczytu pozwala korzystać z danych podstawowych i pomocniczych lokalizacji, w przypadku, gdy jednej lokalizacji staje się niedostępna. Miejsca do magazynowania zbędne geo dostęp do odczytu jest domyślna opcja dla Twojego konta miejsca do magazynowania, domyślnie podczas jej tworzenia. 

    > [AZURE.IMPORTANT] Możesz zmienić sposób danych są replikowane po utworzeniu konta miejsca do magazynowania, o ile nie określono ZRS podczas tworzenia konta. Jednak pamiętać, że mogą ponoszenia przeniesieniu dodatkowe dane jednorazowego kosztów, jeśli zmienisz z LRS GRS lub GRS pomoc Zdalna.

Aby uzyskać dodatkowe informacje na temat opcji replikacji miejsca do magazynowania, zobacz [replikacji magazyn Azure](storage-redundancy.md) .

Informacje o replikacji konta miejsca do magazynowania cenach, uzyskać [Azure ceny miejsca do magazynowania](https://azure.microsoft.com/pricing/details/storage/). Aby uzyskać więcej informacji na temat dostępnych usług w każdym regionie, zobacz [Obszary Azure](https://azure.microsoft.com/regions/#services) .

Architektura szczegółowe informacje na temat wytrzymałości z nośnikami Azure, zobacz [SOSP papieru — magazyn Azure: wysoce dostępne przestrzeni dyskowej usługi w chmurze z spójności silne](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx).


## <a name="transferring-data-to-and-from-azure-storage"></a>Przesyłanie danych do i z miejsca do magazynowania Azure

Za pomocą narzędzia wiersza polecenia AzCopy kopiowania obiektów blob, plików i danych tabeli konta miejsca do magazynowania lub wielu kont miejsca do magazynowania. Aby uzyskać więcej informacji, zobacz [Przenoszenie danych za pomocą narzędzia wiersza polecenia AzCopy](storage-use-azcopy.md) .

AzCopy jest tworzona na bieżąco [Azure Biblioteka przepływu danych](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement/), która jest obecnie dostępna w podglądzie.

Usługa Azure Importuj/Eksportuj umożliwia obiektów blob danych do importowania lub eksportowania danych obiektów blob z Twojego konta miejsca do magazynowania za pośrednictwem dyskiem dysku twardego wysłany do centrum danych Azure. Aby uzyskać więcej informacji o usłudze Importuj/Eksportuj zobacz [Używanie usługi Microsoft Azure Importuj/Eksportuj Transfer danych z magazynem obiektów Blob](storage-import-export-service.md).

## <a name="pricing"></a>Cennik

[AZURE.INCLUDE [storage-account-billing-include](../../includes/storage-account-billing-include.md)]

## <a name="storage-apis-libraries-and-tools"></a>Interfejsy API miejsca do magazynowania, bibliotek i narzędzia

Azure zasobów magazynowania jest możliwy przez dowolny język, który można utworzyć żądania HTTP/HTTPS. Ponadto magazyn Azure oferuje programowania biblioteki do kilku popularnych językach. Te biblioteki uprościć wielu aspektów pracy z magazyn Azure obsługi szczegóły, takie jak wywołania synchroniczne i asynchroniczne, tworzeniu partii operacji, Zarządzanie wyjątkami, automatyczne ponowne próby zachowania podczas działania itd. Biblioteki są obecnie dostępne dla następujących języków i platformach z innymi osobami w potoku:

### <a name="azure-storage-data-services"></a>Usługi danych Azure miejsca do magazynowania

- [Usługi Magazyn interfejsu API usługi REST](http://msdn.microsoft.com/library/azure/dd179355.aspx)
- [Biblioteka klienta miejsca do magazynowania dla .NET, Windows Phone i obsługi systemu Windows](https://www.nuget.org/packages/WindowsAzure.Storage/)
- [Biblioteka klienta miejsca do magazynowania dla C++](https://github.com/Azure/azure-storage-cpp)
- [Biblioteka klienta miejsca do magazynowania dla języka Java i Android](/develop/java/)
- [Biblioteka klienta miejsca do magazynowania dla Node.js](http://dl.windowsazure.com/nodestoragedocs/index.html)
- [Biblioteka klienta miejsca do magazynowania dla PHP](/develop/php/)
- [Biblioteka klienta miejsca do magazynowania dla dopiskiem](/develop/ruby/)
- [Biblioteka klienta miejsca do magazynowania dla Python](/develop/python/)
- [Polecenia cmdlet miejsca do magazynowania dla środowiska PowerShell 1.0](https://msdn.microsoft.com/library/azure/mt269418.aspx)

### <a name="azure-storage-management-services"></a>Usługi zarządzania Azure miejsca do magazynowania

- [Wykaz interfejsu API pozostałych dostawcy zasobów miejsca do magazynowania](https://msdn.microsoft.com/library/azure/mt163683.aspx)
- [Biblioteka klienta dostawcy zasobów miejsca do magazynowania dla środowiska .NET](https://msdn.microsoft.com/library/azure/mt131037.aspx)
- [Polecenia cmdlet dostawcy zasobów miejsca do magazynowania dla programu PowerShell 1.0](https://msdn.microsoft.com/library/azure/mt607151.aspx)
- [Przestrzeni dyskowej usługi zarządzania interfejsu API usługi REST (klasyczny)](https://msdn.microsoft.com/library/azure/ee460790.aspx)

### <a name="azure-storage-data-movement-services"></a>Azure przestrzeni dyskowej usługi przepływu danych

- [Interfejsu API usługi REST usługi Importuj/Eksportuj miejsca do magazynowania](https://msdn.microsoft.com/library/azure/dn529096.aspx)
- [Biblioteka klienta przepływu danych miejsca do magazynowania dla środowiska .NET](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement/)

### <a name="tools-and-utilities"></a>Narzędzia

- [Eksplorator magazynu platformy Azure](http://go.microsoft.com/fwlink/?LinkID=822673&clcid=0x409)
- [Narzędzia klienckie Azure miejsca do magazynowania](storage-explorers.md)
- [Narzędzia i SDK Azure](https://azure.microsoft.com/tools/)
- [Emulator Azure miejsca do magazynowania](http://www.microsoft.com/download/details.aspx?id=43709)
- [Azure programu PowerShell](../powershell-install-configure.md)
- [Narzędzie wiersza polecenia AzCopy](http://aka.ms/downloadazcopy)

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej na temat magazyn Azure, zapoznaj się z następujących zasobów:

### <a name="documentation"></a>Dokumentacja

- [Dokumentacja Azure miejsca do magazynowania](https://azure.microsoft.com/documentation/services/storage/)

### <a name="for-administrators"></a>Dla administratorów

- [Przy użyciu programu PowerShell Azure z nośnikami Azure](storage-powershell-guide-full.md)
- [Polecenie Azure za pomocą Azure miejsca do magazynowania](storage-azure-cli.md)

### <a name="for-net-developers"></a>Dla deweloperów .NET

- [Rozpoczynanie pracy z magazynem obiektów Blob platformy Azure za pomocą .NET](storage-dotnet-how-to-use-blobs.md)
- [Rozpoczynanie pracy z magazynem tabel platformy Azure za pomocą .NET](storage-dotnet-how-to-use-tables.md)
- [Wprowadzenie do magazynowania kolejki Azure za pomocą .NET](storage-dotnet-how-to-use-queues.md)
- [Wprowadzenie do przechowywania plików Azure w systemie Windows](storage-dotnet-how-to-use-files.md)

### <a name="for-javaandroid-developers"></a>Dla deweloperów Java i Android

- [Jak używać magazyn obiektów Blob z języka Java](storage-java-how-to-use-blob-storage.md)
- [Jak używać magazyn tabel z języka Java](storage-java-how-to-use-table-storage.md)
- [Jak używać magazyn kolejek z języka Java](storage-java-how-to-use-queue-storage.md)
- [Jak używać magazyn plików z języka Java](storage-java-how-to-use-file-storage.md)

### <a name="for-nodejs-developers"></a>Dla deweloperów Node.js

- [Jak używać magazyn obiektów Blob z Node.js](storage-nodejs-how-to-use-blob-storage.md)
- [Jak używać magazyn tabel z Node.js](storage-nodejs-how-to-use-table-storage.md)
- [Jak używać magazyn kolejek z Node.js](storage-nodejs-how-to-use-queues.md)

### <a name="for-php-developers"></a>Dla programistów PHP

- [Jak używać magazyn obiektów Blob z PHP](storage-php-how-to-use-blobs.md)
- [Jak używać magazyn tabel z PHP](storage-php-how-to-use-table-storage.md)
- [Jak używać magazyn kolejek z PHP](storage-php-how-to-use-queues.md)

### <a name="for-ruby-developers"></a>Dla deweloperów dopiskiem fonetycznym

- [Jak używać magazyn obiektów Blob z dopiskiem](storage-ruby-how-to-use-blob-storage.md)
- [Jak używać magazyn tabel z dopiskiem](storage-ruby-how-to-use-table-storage.md)
- [Jak używać magazyn kolejek z dopiskiem](storage-ruby-how-to-use-queue-storage.md)

### <a name="for-python-developers"></a>Dla deweloperów Python

- [Jak używać magazyn obiektów Blob z Python](storage-python-how-to-use-blob-storage.md)
- [Jak używać magazyn tabel z Python](storage-python-how-to-use-table-storage.md)
- [Jak używać magazyn kolejek z Python](storage-python-how-to-use-queue-storage.md)
- [Jak używać magazyn plików z Python](storage-python-how-to-use-file-storage.md)
