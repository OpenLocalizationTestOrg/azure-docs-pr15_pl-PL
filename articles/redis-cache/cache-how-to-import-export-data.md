<properties 
    pageTitle="Importowanie i eksportowanie danych w pamięci podręcznej Azure Redis | Microsoft Azure" 
    description="Dowiedz się, jak importowanie i eksportowanie danych do i z magazynem obiektów blob z Twojej wystąpienia Azure Redis w pamięci podręcznej premium" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/15/2016" 
    ms.author="sdanie"/>

# <a name="import-and-export-data-in-azure-redis-cache"></a>Importowanie i eksportowanie danych w pamięci podręcznej Azure Redis

Import/Eksport jest Azure Redis w pamięci podręcznej operacji zarządzania danymi, która umożliwia importowanie danych do pamięci podręcznej Azure Redis lub eksportowania danych z pamięci podręcznej Azure Redis za importowania i eksportowania migawki Redis pamięci podręcznej bazy danych (RDB) z pamięci podręcznej premium do obiektów blob strony przy użyciu konta z miejsca do magazynowania Azure. Import/Eksport umożliwia migrowanie między różnych wystąpieniach Azure Redis w pamięci podręcznej lub wypełnienia w pamięci podręcznej danych przed użyciem.

Ten artykuł zawiera przewodnik do importowania i eksportowania danych z pamięci podręcznej Azure Redis i zawiera odpowiedzi na często zadawane pytania.

>[AZURE.IMPORTANT] Importuj/Eksportuj w wersji preview i jest dostępna tylko dla [poziomu premium](cache-premium-tier-intro.md) pamięci podręcznej.

## <a name="import"></a>Importowanie

Importowanie można przenosić Redis zgodne RDB plików z dowolnego Redis serwerem w chmurze lub środowisko, w tym Redis uruchomionych Linux, Windows lub dowolnego dostawcy chmurze, takich jak Amazon usług sieci Web i innych osób. Importowanie danych jest łatwym sposobem utworzenia pamięci podręcznej z danymi wstępnie wypełnione. Podczas procesu importowania Azure Redis w pamięci podręcznej ładuje pliki RDB z magazynu Azure do pamięci, a następnie wstawia nazwy klawiszy w pamięci podręcznej.

>[AZURE.NOTE] Przed rozpoczęciem operacji importowania, upewnij się, że pliku Redis bazy danych (RDB) lub plików są przekazywane do strony BLOB w magazynie Azure, w tym samym regionie i subskrypcji jako wystąpienia Azure Redis w pamięci podręcznej. Aby uzyskać więcej informacji zobacz [Rozpoczynanie pracy z magazynem obiektów Blob platformy Azure](../storage/storage-dotnet-how-to-use-blobs.md). Jeśli wyeksportowany plik RDB przy użyciu funkcji [Azure Redis eksportowanie pamięci podręcznej](#export) , plik RDB znajduje się już w blob strony i jest gotowy do importowania.

1. Aby zaimportować jeden lub więcej wyeksportowane pamięci podręcznej obiektów blob, [Przejdź do pamięci podręcznej](cache-configure.md#configure-redis-cache-settings) w portalu Azure i kliknij pozycję **Importowanie danych** z karta **Ustawienia** wystąpienia z pamięci podręcznej.

    ![Importowanie danych][cache-import-data]

2. Kliknij pozycję **Wybierz pozycję Blob(s)** i wybierz konto miejsca do magazynowania, zawierający dane, których chcesz zaimportować.

    ![Wybierz konto miejsca do magazynowania][cache-import-choose-storage-account]

3. Kliknij kontener, która zawiera dane do zaimportowania.

    ![Wybierz kontener][cache-import-choose-container]

4. Zaznacz jeden lub więcej obiektów blob do zaimportowania, klikając pozycję obszar po lewej stronie nazwy obiektów blob, a następnie kliknij przycisk **Wybierz**.

    ![Wybierz pozycję obiektów blob][cache-import-choose-blobs]

5. Kliknij pozycję **Importowanie** , aby rozpocząć proces importowania.

    >[AZURE.IMPORTANT] Pamięci podręcznej nie jest dostępny przez klientów pamięci podręcznej podczas procesu importowania i wszelkie istniejące dane w pamięci podręcznej zostanie usunięty.

    ![Importowanie][cache-import-blobs]

    Można monitorować postęp operacji importu, wykonując powiadomienia z portalu Azure lub wyświetlania zdarzeń w [dziennika inspekcji](cache-configure.md#support-amp-troubleshooting-settings).

    ![Postęp importowania][cache-import-data-import-complete] 


## <a name="export"></a>Eksportowanie

Eksportowanie umożliwia eksportowanie danych przechowywanych w pamięci podręcznej Redis Azure do Redis zgodne pliki RDB. Ta funkcja umożliwia przenoszenie danych z jednego wystąpienia Azure Redis w pamięci podręcznej do drugiego lub na inny serwer Redis. Podczas procesu eksportowania jest tworzony plik tymczasowy na maszyn wirtualnych obsługującego wystąpienie serwera Azure Redis w pamięci podręcznej, a plik jest przekazywany do konta wyznaczonego miejsca do magazynowania. Po zakończeniu operacji eksportowania ze stanem sukcesu lub niepowodzenia tymczasowy plik zostanie usunięty.

1. Aby wyeksportować bieżącej zawartości pamięci podręcznej, aby miejsca do magazynowania, [Przejdź do pamięci podręcznej](cache-configure.md#configure-redis-cache-settings) w portalu Azure, a następnie kliknij pozycję **Eksportuj dane** z karta **Ustawienia** wystąpienia z pamięci podręcznej.

    ![Wybierz kontener miejsca do magazynowania][cache-export-data-choose-storage-container]

2. Kliknij pozycję **Wybierz kontenera magazynu** i wybierz konto odpowiednie miejsca do magazynowania. Konto miejsca do magazynowania musi być w tej samej subskrypcji i region, pamięci podręcznej.

    >[AZURE.IMPORTANT] Współpraca z obiektami blob strony, które są obsługiwane przez zarówno klasyczny i ARM miejsca do magazynowania konta, ale nie są obsługiwane przez [Blob miejsca do magazynowania kont](../storage/storage-blob-storage-tiers.md#blob-storage-accounts) w tej chwili Importuj/Eksportuj.

    ![Konto miejsca do magazynowania][cache-export-data-choose-account]

3. Wybierz odpowiednie obiektów blob kontener, a następnie kliknij przycisk **Wybierz**. Aby użyć nowego kontenera, kliknij **Dodaj kontenera** , aby go najpierw dodać, a następnie wybierz ją z listy.

    ![Wybierz kontener miejsca do magazynowania][cache-export-data-container]

4. Wpisz **Prefiks nazwy obiektów Blob** , a następnie kliknij polecenie **Eksportuj** , aby rozpocząć proces eksportowania. Prefiks nazwy obiektów blob jest używany do prefiksu nazwy plików generowane przez tej operacji eksportowania.

    ![Eksportowanie][cache-export-data]

    Można monitorować postęp operacji eksportowania, wykonując powiadomienia z portalu Azure lub wyświetlając zdarzenia w [dziennika inspekcji](cache-configure.md#support-amp-troubleshooting-settings).

    ![][cache-export-data-export-complete]

    Pamięć podręczną pozostają dostępne do użycia w procesie eksportowania.


## <a name="importexport-faq"></a>Import/Eksport — często zadawane pytania

Ta sekcja zawiera odpowiedzi na często zadawane pytania dotyczące funkcji Importuj/Eksportuj.

-   [Jakie ceny poziomów służy Importuj/Eksportuj?](#what-pricing-tiers-can-use-importexport)
-   [Czy można importować dane z dowolnego serwera Redis?](#can-i-import-data-from-any-redis-server)
-   [Moje pamięci podręcznej będą dostępne podczas operacji importu i eksportu?](#will-my-cache-be-available-during-an-importexport-operation)
-   [Czy mogę korzystać z klastrem Redis Importuj/Eksportuj?](#can-i-use-importexport-with-redis-cluster)
-   [Jak działa Import/Eksport z niestandardowe ustawienie bazy danych](#how-does-importexport-work-with-a-custom-databases-setting)
-   [Czym różni się importu i eksportu z utrzymywanie Redis?](#how-is-importexport-different-from-redis-persistence)
-   [Czy mogę zautomatyzować przy użyciu programu PowerShell, polecenie lub innymi klientami zarządzania Importuj/Eksportuj](#can-i-automate-importexport-using-powershell-cli-or-other-management-clients)
-   [Otrzymuję błąd przekroczenia limitu czasu podczas Moje operacji importu i eksportu. Co to znaczy?](#i-received-a-timeout-error-during-my-importexport-operation.-what-does-it-mean)
-   [Podczas eksportowania danych z magazynem obiektów Blob platformy Azure, masz się komunikat o błędzie. Co się stało?](#i-got-an-error-when-exporting-my-data-to-azure-blob-storage.-what-happened)


### <a name="what-pricing-tiers-can-use-importexport"></a>Jakie ceny poziomów służy Importuj/Eksportuj?

Import/Eksport jest dostępna tylko w premium ceny warstwy.

### <a name="can-i-import-data-from-any-redis-server"></a>Czy można importować dane z dowolnego serwera Redis?

Tak, oprócz zaimportować dane wyeksportowane z wystąpień Azure Redis w pamięci podręcznej, można importować pliki RDB przez serwer Redis systemem w chmurze lub środowiska, takich jak Linux, Windows, lub dostawców, takich jak usługi sieci Web Amazon w chmurze. W tym celu należy przekazać plik RDB z odpowiednim serwera Redis do obiektów blob strony przy użyciu konta z miejsca do magazynowania Azure, a następnie zaimportować je do wystąpienia Azure Redis w pamięci podręcznej premium. Na przykład można eksportować dane z pamięci podręcznej produkcji, a następnie zaimportować je do pamięci podręcznej użyte jako część środowiska wzorcowego do testowania lub migracji. 

### <a name="will-my-cache-be-available-during-an-importexport-operation"></a>Moje pamięci podręcznej będą dostępne podczas operacji importu i eksportu?

-   **Eksportowanie** — pamięci podręcznej są dostępne i można kontynuować do użycia pamięci podręcznej podczas operacji eksportowania.
-   **Importowanie** - pamięci podręcznej stają się niedostępne podczas uruchamiania operacji importowania i stają się dostępne po ukończeniu operacji importowania.


### <a name="can-i-use-importexport-with-redis-cluster"></a>Czy mogę korzystać z klastrem Redis Importuj/Eksportuj?

Tak, a użytkownik może Importuj/Eksportuj między grupowany pamięci podręcznej i pamięci podręcznej grupowany. Od Redis klaster [obsługuje tylko bazy danych 0](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering), nie będzie można zaimportować jakichkolwiek danych w bazach danych inną niż 0. Podczas importowania danych z pamięci podręcznej grupowany klucze są rozdzielane odłamki klastrze. 

### <a name="how-does-importexport-work-with-a-custom-databases-setting"></a>Jak działa Import/Eksport z niestandardowe ustawienie bazy danych

Kilka poziomów cennik ma różne [ograniczenia baz danych](cache-configure.md#databases), więc istnieje kilka zagadnień podczas importu, jeśli skonfigurowano Niestandardowa wartość dla `databases` ustawienie podczas tworzenia pamięci podręcznej.

-   Jeśli celem importowania cennik warstwa z dolną `databases` limit niż warstwie, z którego wyeksportowano:
    -   Jeśli korzystasz z domyślnej liczby `databases` czyli 16 dla wszystkich poziomów cennik są tracone żadne dane.
    -   Jeśli korzystasz z niestandardowej liczbę `databases` tego wchodzi w limity dla poziomu, do której importujesz, są tracone żadne dane.
    -   Jeśli dane w bazie danych, którego rozmiar przekracza limity nowy poziom, wyeksportowane dane nie są importowane dane z tych wyższą baz danych.

### <a name="how-is-importexport-different-from-redis-persistence"></a>Czym różni się importu i eksportu z utrzymywanie Redis?

Azure utrzymywanie Redis pamięci podręcznej umożliwia utrzymują dane przechowywane w Redis do magazynu Azure. Po skonfigurowaniu trwałe pamięci podręcznej Azure Redis będzie nadal występował migawkę Redis pamięci podręcznej w formacie binarnym Redis na dysku na podstawie na można konfigurować częstotliwości kopii zapasowej. Jeśli wystąpi losowych zdarzenie, które wyłączają podstawowy i replice pamięć podręczną, danych z pamięci podręcznej zostanie przywrócony automatycznie przy użyciu najnowszych migawkę. Aby uzyskać więcej informacji zobacz [jak skonfigurować utrzymywanie danych na Premium Azure Redis potrzeby pamięci podręcznej](cache-how-to-premium-persistence.md).

Importuj / Eksportuj umożliwia przenoszenie danych do lub eksportowanie z Azure Redis w pamięci podręcznej. Nie Konfigurowanie kopii zapasowych i przywracanie przy użyciu utrzymywanie Redis.


### <a name="can-i-automate-importexport-using-powershell-cli-or-other-management-clients"></a>Czy mogę zautomatyzować przy użyciu programu PowerShell, polecenie lub innymi klientami zarządzania Importuj/Eksportuj

Tak, dla środowiska PowerShell instrukcje zobacz [do zaimportowania Redis pamięci podręcznej](cache-howto-manage-redis-cache-powershell.md#to-import-a-redis-cache) i [do wyeksportowania Redis pamięci podręcznej](cache-howto-manage-redis-cache-powershell.md#to-export-a-redis-cache).



### <a name="i-received-a-timeout-error-during-my-importexport-operation-what-does-it-mean"></a>Otrzymuję błąd przekroczenia limitu czasu podczas Moje operacji importu i eksportu. Co to znaczy?

Jeśli użytkownik pozostanie na karta **danych importowania** lub **eksportowania danych** przez okres dłuższy niż 15 minut przed rozpoczęciem operacji, otrzymujesz komunikat o błędzie podobny do następującego.

    The request to import data into cache 'contoso55' failed with status 'error' and error 'One of the SAS URIs provided could not be used for the following reason: The SAS token end time (se) must be at least 1 hour from now and the start time (st), if given, must be at least 15 minutes in the past.

Aby rozwiązać ten problem, zainicjować importowanie lub eksportowanie przed 15 minut przed upływem.

### <a name="i-got-an-error-when-exporting-my-data-to-azure-blob-storage-what-happened"></a>Podczas eksportowania danych z magazynem obiektów Blob platformy Azure, masz się komunikat o błędzie. Co się stało?

Importuj/Eksportuj działa tylko z plikami RDB przechowywane jako blob strony. Inne typy obiektów blob nie są obsługiwane w tej chwili, w tym konta magazyn obiektów blob z ciepłej i atrakcyjne warstwy.


## <a name="next-steps"></a>Następne kroki
Dowiedz się, jak korzystać z większej liczby funkcji pamięci podręcznej premium.

-   [Wprowadzenie do poziomu Azure Redis pamięci podręcznej Premium](cache-premium-tier-intro.md)    

  
<!-- IMAGES -->
[cache-settings-import-export-menu]: ./media/cache-how-to-import-export-data/cache-settings-import-export-menu.png
[cache-export-data-choose-account]: ./media/cache-how-to-import-export-data/cache-export-data-choose-account.png
[cache-export-data-choose-storage-container]: ./media/cache-how-to-import-export-data/cache-export-data-choose-storage-container.png
[cache-export-data-container]: ./media/cache-how-to-import-export-data/cache-export-data-container.png
[cache-export-data-export-complete]: ./media/cache-how-to-import-export-data/cache-export-data-export-complete.png
[cache-export-data]: ./media/cache-how-to-import-export-data/cache-export-data.png
[cache-import-data]: ./media/cache-how-to-import-export-data/cache-import-data.png
[cache-import-choose-storage-account]: ./media/cache-how-to-import-export-data/cache-import-choose-storage-account.png
[cache-import-choose-container]: ./media/cache-how-to-import-export-data/cache-import-choose-container.png
[cache-import-choose-blobs]: ./media/cache-how-to-import-export-data/cache-import-choose-blobs.png
[cache-import-blobs]: ./media/cache-how-to-import-export-data/cache-import-blobs.png
[cache-import-data-import-complete]: ./media/cache-how-to-import-export-data/cache-import-data-import-complete.png








