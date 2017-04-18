<properties 
   pageTitle="Co to jest StorSimple? | Microsoft Azure" 
   description="W tym artykule opisano StorSimple obsługi urządzenia, urządzenia wirtualnego, usług i zarządzanie magazynem i wprowadza kluczowe terminy używane w StorSimple." 
   services="storsimple" 
   documentationCenter="NA" 
   authors="SharS" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD" 
   ms.date="10/05/2016"
   ms.author="v-sharos@microsoft.com"/>

# <a name="storsimple-8000-series-a-hybrid-cloud-storage-solution"></a>Seria StorSimple 8000: masowej hybrydowych chmury

## <a name="overview"></a>Omówienie

Witamy w programie Microsoft Azure StorSimple, zintegrowane masowej, zarządzającą zadań magazynu między urządzeniami lokalnego i magazynu w chmurze Microsoft Azure. StorSimple jest wydajność, optymalne koszty i łatwe w zarządzaniu obszar sieci SAN masowej eliminujący wiele problemów i koszty skojarzone z przedsiębiorstwa miejsca do magazynowania i ochrony danych. Go używa własnych urządzenia serii StorSimple 8000, można zintegrować z usługami w chmurze i zapewnia zestaw narzędzi do zarządzania dla Bezproblemowa widoku wszystkie przedsiębiorstwa miejscem do magazynowania, łącznie z magazynu w chmurze. (StorSimple informacje na temat wdrażania opublikowanych w witrynie sieci Web Microsoft Azure dotyczy tylko urządzenia serii StorSimple 8000. Jeśli korzystasz z urządzenia serii StorSimple 5000-7000, przejdź do [Pomocy StorSimple](http://onlinehelp.storsimple.com/).)

StorSimple używa [magazynu obsługi](#automatic-storage-tiering) Zarządzanie dane przechowywane w różnych nośników. Bieżący zestaw roboczy są magazynowane lokalnie w stanie stałym dyski SSD, rzadziej używane dane są przechowywane na dyskach twardych (dysków twardych) i archiwizacji danych zostanie przypisany do chmury. Ponadto StorSimple używa deduplication i kompresji w celu zmniejszenia ilości przestrzeni dyskowej, która korzysta z danych. Aby uzyskać więcej informacji przejdź do [Deduplication i kompresji](#deduplication-and-compression). Definicje innych najważniejszych terminów i pojęć używanych w dokumentacji serii StorSimple 8000 przejdź do [terminologii StorSimple](#storsimple-terminology) na końcu tego artykułu.

Z StorSimple aktualizacji 2 odpowiednie ilości można określić jako *lokalnie przypięte* zapewnienie pozostaje lokalne do urządzenia podstawowego i nie warstwa w chmurze. Pozwala na uruchamianie obciążeń poufne do opóźnienie chmurze, takich jak SQL i maszyn wirtualnych obciążeń pracą lokalnie przypięte ilości kontynuując używanie w chmurze do tworzenia kopii zapasowych. Aby uzyskać więcej informacji na temat lokalnie przypięty wielkości zobacz [Używanie usługi Menedżera StorSimple Zarządzanie wielkości](storsimple-manage-volumes-u2.md). 

Aktualizacja 2 umożliwia tworzenie StorSimple urządzeń wirtualnych korzystać z opóźnienia niskiej i wysokiej wydajności dostarczony przez Azure premium miejsca do magazynowania. Aby uzyskać więcej informacji na temat urządzeń wirtualnych premium StorSimple, zobacz [rozmieszczanie i zarządzanie nimi urządzenie wirtualne StorSimple platformy Azure](storsimple-virtual-device-u2.md). Aby uzyskać więcej informacji na temat przechowywania Azure premium, przejdź do [miejsca do magazynowania Premium: wysokiej wydajności miejsca do magazynowania dla Azure maszyn wirtualnych obciążenia](../storage/storage-premium-storage.md).

Oprócz Zarządzanie magazynem funkcje ochrony danych StorSimple umożliwiają tworzenie na żądanie zaplanowanych kopii zapasowych, a następnie zapisane lokalnie lub w chmurze. Kopie zapasowe są pobierane w formularzu przyrostowe migawki, co oznacza, że mogą być tworzone i szybko przywrócić. Migawki chmury może być bardzo ważne w scenariuszy odzyskiwania systemu, ponieważ Zamień systemów magazyn pomocniczy (na przykład kopia zapasowa taśmą) i umożliwia przywracanie danych do centrum danych lub alternatywny witryn, w razie potrzeby.

![ikona klipu wideo](./media/storsimple-overview/video_icon.png) Obejrzyj klip wideo zawiera krótkie wprowadzenie do programu Microsoft Azure StorSimple.

> [AZURE.VIDEO storsimple-hybrid-cloud-storage-solution]

## <a name="why-use-storsimple"></a>Dlaczego warto używać StorSimple?

W poniższej tabeli opisano niektóre z najważniejszych zalet dostępnych w programie Microsoft Azure StorSimple.

| Funkcja | Korzyści |
|---------|---------|
|Integracja przezroczystości | Microsoft Azure StorSimple Protokół iSCSI sposób niewidoczny łącze magazynów danych. To zapewnia dane przechowywane w chmurze, w centrum danych, lub na serwerach zdalnych może znajdować się w jednym miejscu.|
|Koszty ograniczona miejsca do magazynowania|Microsoft Azure StorSimple przydziela wystarczających lokalnie lub magazynu w chmurze do spełnienia wymagań bieżącej i rozszerza magazynu w chmurze tylko wtedy, gdy jest to konieczne. Go dodatkowo zmniejsza wymagania dotyczące przechowywania i wydatków za eliminowania zbędnych wersji tych samych danych (deduplication) i za pomocą kompresji.|
|Zarządzanie magazynem uproszczone|Microsoft Azure StorSimple zapewnia system narzędzia administracyjne, które służą do konfigurowania i zarządzania dane przechowywane w lokalnym na serwerze zdalnym i w chmurze. Ponadto można zarządzać kopii zapasowych i przywracanie funkcje z przystawką programu Microsoft Management Console (MMC). StorSimple zawiera osobne, opcjonalnych interfejs, który umożliwia rozszerzanie StorSimple zarządzania i ochrony usługi danych do zawartości przechowywanej na serwerach programu SharePoint. |
|Ulepszone odzyskiwanie i zgodność|Nie wymaga czasu przedłużone odzyskiwanie usługi Microsoft Azure StorSimple. Zamiast tego przywraca go danych jest odpowiednio. Oznacza to, że normalnego działania można kontynuować pracę z minimalnymi zakłócenia w pracy. Ponadto można skonfigurować zasady do określania harmonogramów kopii zapasowej i przechowywanie danych.
|Mobilność danych|Dane przekazane do usługi Microsoft Azure cloud services są dostępne z innych witryn na potrzeby odzyskiwania i migracji. Ponadto StorSimple umożliwia konfigurowanie urządzeń wirtualnych StorSimple na wirtualnych maszyn działa w programie Microsoft Azure. Maszyny wirtualne używać urządzeń wirtualnych dostępu do przechowywanych danych do celów testowych lub odzyskiwania.|
|Obsługa innych dostawców usług w chmurze |Seria StorSimple 8000 z oprogramowaniem aktualizacja 1 lub nowszej obsługuje Amazon S3 z Rekordów, HP i OpenStack chmury usługi, a także Microsoft Azure. (Nadal będzie potrzebne konto Microsoft Azure miejsca do magazynowania na potrzeby zarządzania urządzenia.) Aby uzyskać więcej informacji przejdź do [Nowości w 1.2 aktualizacji](storsimple-update1-release-notes.md#whats-new-in-update-12).|
|Zapewnianie ciągłości | Aktualizacja 1 lub nowszy zawiera nową funkcję migracji, która umożliwia StorSimple 5000 7000 serię użytkownikom przeprowadzić migrację danych do urządzenia serii StorSimple 8000.|
|Dostępność w Portal Azure dla instytucji rządowych | Aktualizacja StorSimple 1 lub nowszym jest dostępna w portalu dla instytucji rządowych Azure. Aby uzyskać więcej informacji zobacz temat [Deploy urządzeniu StorSimple lokalnego w portalu dla instytucji rządowych](storsimple-deployment-walkthrough-gov.md).|
|Ochrona danych i dostępność | Seria StorSimple 8000 z aktualizacji 1 lub nowszym obsługuje strefy zbędne przestrzeni dyskowej (ZRS), oprócz lokalnie zbędne przestrzeni dyskowej (LRS) i zbędne Geo przestrzeni dyskowej (GRS). Można znaleźć [w tym artykule na temat opcji nadmiarowości magazyn Azure](https://azure.microsoft.com/documentation/articles/storage-redundancy/) szczegółowe ZRS.|
|Obsługa aplikacji krytycznych | Z StorSimple aktualizacji 2 można określić odpowiednie ilości lokalnie przypięte. Ta funkcja zapewnia, że dane wymagane przez kluczowych aplikacji nie jest warstwowa w chmurze. Lokalnie przypięty wielkości nie podlegają opóźnienia w chmurze lub problemy z łącznością. Aby uzyskać więcej informacji na temat wielkości lokalnie przypięty zobacz [Używanie usługi Menedżera StorSimple Zarządzanie wielkości](storsimple-manage-volumes-u2.md).|
|Krótki czas oczekiwania i Wysoka wydajność | StorSimple aktualizacji 2 umożliwia tworzenie wirtualnych urządzenia, które skorzystać z wysokiej wydajności funkcji krótki czas oczekiwania Azure premium przestrzeni dyskowej. Aby uzyskać więcej informacji na temat urządzeń wirtualnych premium StorSimple, zobacz [rozmieszczanie i zarządzanie nimi urządzenie wirtualne StorSimple platformy Azure](storsimple-virtual-device-u2.md).|

![ikona klipu wideo](./media/storsimple-overview/video_icon.png) Obejrzyj [ten klip wideo](https://www.youtube.com/watch?v=4MhJT5xrvQw&feature=youtu.be) zawiera omówienie StorSimple 8000 serii funkcje i zalety.

## <a name="storsimple-components"></a>Składniki StorSimple

Rozwiązanie Microsoft Azure StorSimple zawiera następujące składniki:

- **Urządzenie Microsoft Azure StorSimple** — tablicę miejsca do magazynowania hybrydowych lokalnego, która zawiera twarde SSD i dysków twardych, razem z nadmiarowe kontrolery i możliwości automatyczne przejście. Kontrolery Zarządzanie magazynem obsługi, umieszczania obecnie używane (lub ciepłej) danych w magazynie lokalnym (na urządzeniu lub lokalnych serwerów), podczas przenoszenia mniej danych często używanych w chmurze.
- **Urządzenia wirtualnego StorSimple** — nazywane także urządzenia wirtualnego StorSimple, to wersja oprogramowania urządzenia StorSimple replikuje architektura i większość funkcji na urządzeniu magazynującym fizycznie hybrydowych. Urządzenia wirtualnego StorSimple działa na jeden węzeł Azure maszyn wirtualnych. Wirtualna urządzeń Premium, które skorzystać z magazynu Azure premium, są dostępne w pakiecie 2 lub nowszy.
- **Usługa Menedżera StorSimple** — rozszerzenie portalu klasyczny Azure, która umożliwia zarządzanie StorSimple lub urządzenia wirtualnego StorSimple w interfejsie jednej sieci web. Usługa Menedżera StorSimple umożliwia tworzenie i zarządzanie usługami, wyświetlanie i zarządzanie urządzeniami, wyświetlanie alertów, należy i wyświetlanie i zarządzanie Zarządzanie zasadami kopii zapasowej i wykazu kopii zapasowych.
- **Windows PowerShell dla StorSimple** — interfejs wiersza polecenia służące do zarządzania urządzeniem StorSimple. Windows PowerShell dla StorSimple zawiera funkcje, które umożliwiają rejestrowanie urządzenia StorSimple, skonfigurować interfejs sieciowy na urządzeniu, zainstalować niektórych rodzajów aktualizacje, rozwiązywanie problemów z urządzenia po zalogowaniu się do sesji pomocy technicznej i Zmień stan urządzenia. Można korzystać z programu Windows PowerShell dla StorSimple przez nawiązanie połączenia z konsoli szeregowego lub za pomocą programu Windows PowerShell zdalnych.
- **Polecenia cmdlet StorSimple programu PowerShell azure** — zbiór poleceń cmdlet programu Windows PowerShell, które umożliwiają automatyzację zadań poziomu usług i migracji z poziomu wiersza polecenia. Aby uzyskać więcej informacji na temat polecenia cmdlet programu PowerShell Azure dla StorSimple przejdź do [informacje dotyczące poleceń cmdlet](https://msdn.microsoft.com/library/dn920427.aspx).
- **Menedżer migawkę StorSimple** — przystawki używanego głośność grupy i usługi kopiowania cień głośność systemu Windows do generowania spójną z aplikacją kopie zapasowe. Ponadto można użyć StorSimple migawkę Menedżera do tworzenia kopii zapasowej harmonogramów i klonowanie lub przywracanie wielkości. 
- **Karta StorSimple dla programu SharePoint** — narzędzie, które przezroczysty rozszerza Microsoft Azure StorSimple miejsca do magazynowania i ochrony danych do farmy serwerów programu SharePoint, podczas tworzenia magazynowania StorSimple widoczny i zarządzaniu z portalu administracji centralnej programu SharePoint.

Na poniższym diagramie zawiera szczegółowego widoku architektura Microsoft Azure StorSimple i składników.

![Architektura StorSimple](./media/storsimple-overview/overview-big-picture.png)

W poniższych sekcjach opisano każdą z tych składników bardziej szczegółowo i wyjaśniono, jak rozwiązanie rozmieszcza danych, przydziela miejsca do magazynowania i ułatwia zarządzanie magazynem i ochrony danych. Ostatni sekcja zawiera definicje niektóre ważne terminy i pojęć związanych z StorSimple składniki oraz zarządzania nimi.

## <a name="storsimple-device"></a>Urządzenie StorSimple

Urządzenie Microsoft Azure StorSimple jest tablicą miejsca do magazynowania hybrydowych lokalnego, zawierającego podstawowy i iSCSI dostęp do danych znajdujących się na niej. Zarządza komunikacji z magazynu w chmurze i pomaga w celu zapewnienia bezpieczeństwa i poufności wszystkich danych, która znajduje się na rozwiązanie Microsoft Azure StorSimple.

Urządzenie StorSimple zawiera twarde SSD i dysków twardych dysków twardych, a także pomocy technicznej do klastrów i automatyczne przełączania awaryjnego. Zawiera udostępnionego procesor, udostępnionej przestrzeni dyskowej oraz dwa kontrolery lustrzane. Każdy kontroler oferuje następujące funkcje:

- Połączenie z komputera-hosta
- Maksymalnie sześć portów sieciowych, aby nawiązać połączenie sieci lokalnej (LAN)
- Monitorowania sprzętu
- Pamięci RAM nie lotnych (NVRAM), który zachowuje informacje, nawet jeśli zostanie przerwany power
- Obsługą klastrów aktualizowanie Zarządzanie aktualizacje oprogramowania na serwerach w klastrze pracy awaryjnej, aby uniknąć aktualizacje minimalnego lub nie mają wpływu na dostępność usługi
- Klaster usługi, które funkcje, takie jak klaster wewnętrznej, wysokiej dostępności i zminimalizowania niepożądanych, które mogą wystąpić, jeśli dysk twardy SSD kończy się niepowodzeniem lub jest do trybu offline

Tylko jeden kontroler jest aktywny w dowolnym momencie w czasie. Jeśli aktywne kontrolerze nie powiedzie się, drugi kontroler automatycznie staje się aktywne. 

Aby uzyskać więcej informacji przejdź do [StorSimple sprzętowej i stan](storsimple-monitor-hardware-status.md).

## <a name="storsimple-virtual-device"></a>Urządzenia wirtualnego StorSimple

StorSimple umożliwia tworzenie wirtualnych urządzenie replikuje architektura i możliwości na urządzeniu magazynującym fizycznie hybrydowych. Urządzenia wirtualnego StorSimple (nazywane także urządzenia wirtualnego StorSimple) jest uruchamiany w jeden węzeł Azure maszyn wirtualnych. (Urządzenie wirtualne mogą być tworzone tylko na Azure maszyn wirtualnych. Nie można utworzyć na urządzeniu StorSimple lub lokalnego serwera.) 

Urządzenie wirtualne oferuje następujące funkcje:

- Zachowuje się jak fizycznie urządzenia, a mogą oferować interfejsu iSCSI do maszyn wirtualnych w chmurze. 
- Można utworzyć nieograniczoną liczbę urządzeń wirtualnych w chmurze i włączanie ich włączanie i wyłączanie funkcji stosownie do potrzeb. 
- Pomaga symulować środowiskami lokalnymi awarii, rozwoju i scenariuszy testowania i może pomóc w poziomie elementu pobierania z kopii zapasowych. 

Z aktualizacji 2 lub nowszym urządzenia wirtualnego StorSimple jest dostępna w dwóch modeli: urządzenie 8010 (wcześniej nazywanego 1100 model) i 8020. Urządzenie 8010 ma maksymalnej pojemności 30 TB. 8020 urządzenia, które wykorzystuje przestrzeni dyskowej Azure premium, ma maksymalnej pojemności 64 TB. (W lokalnym poziomów, Magazyn Azure premium przechowuje dane na twarde SSD należy standardowego magazynu są przechowywane dane na dysków twardych.) Należy zauważyć, że trzeba mieć konto miejsca do magazynowania Azure premium korzystania z magazynu premium. Aby uzyskać więcej informacji na temat przechowywania premium, przejdź do [miejsca do magazynowania Premium: wysokiej wydajności miejsca do magazynowania dla Azure maszyn wirtualnych obciążenia](../storage/storage-premium-storage.md).

Aby uzyskać więcej informacji na temat urządzenia wirtualnego StorSimple, przejdź do [rozmieszczanie i zarządzanie nimi urządzenie wirtualne StorSimple platformy Azure](storsimple-virtual-device-u2.md).

## <a name="storsimple-manager-service"></a>Usługa Menedżera StorSimple

Microsoft Azure StorSimple udostępnia interfejs użytkownika oparte na sieci web (usługa Menedżer StorSimple), który umożliwia centralne zarządzanie Centrum danych i magazynu w chmurze. Usługa Menedżera StorSimple umożliwia wykonywanie następujących zadań:

- Konfigurowanie ustawień systemu dla urządzeń StorSimple.
- Konfigurowanie i zarządzanie ustawień zabezpieczeń dla urządzeń StorSimple.
- Konfigurowanie poświadczeń w chmurze i właściwości.
- Konfigurowanie i zarządzanie wielkości na serwerze.
- Konfigurowanie grupy głośność.
- Tworzenie kopii zapasowych i przywracania danych.
- Monitorowanie wydajności.
- Przejrzyj ustawienia systemowe i identyfikowanie ewentualnym problemom.

Usługa Menedżera StorSimple umożliwia wykonywanie wszystkich zadań administracyjnych, z wyjątkiem tych, które wymagają systemu czas, taki jak początkowej konfiguracji i instalowanie aktualizacji.

Aby uzyskać więcej informacji przejdź do tematu [Używanie usługę Menedżer StorSimple administrowania urządzenia StorSimple](storsimple-manager-service-administration.md).

## <a name="windows-powershell-for-storsimple"></a>Windows PowerShell dla StorSimple

Windows PowerShell dla StorSimple udostępnia interfejs wiersza polecenia, który służy do tworzenia i zarządzania usługą Microsoft Azure StorSimple i konfigurowanie i monitorowanie urządzeń StorSimple. Jest oparte na programie Windows PowerShell interfejs wiersza polecenia, zawierającego dedykowane polecenia cmdlet zarządzania urządzenia StorSimple. Windows PowerShell dla StorSimple zawiera funkcje, które umożliwiają:

- Zarejestruj się w urządzeniu.
- Konfigurowanie interfejsu sieciowego na urządzeniu.
- Zainstaluj niektórych rodzajów aktualizacji.
- Rozwiązywanie problemów z urządzenia po zalogowaniu się do sesji pomocy technicznej.
- Zmień stan urządzenia.

Dostępne środowiska Windows PowerShell dla StorSimple z poziomu konsoli szeregowego (na klawiaturze komputera-hosta podłączone bezpośrednio do urządzenia) lub zdalnie przy użyciu zdalnej programu Windows PowerShell. Zauważ, że niektóre środowiska Windows PowerShell dla StorSimple zadań, takich jak rejestracja urządzenia początkowej, jest możliwe tylko na konsoli szeregowego. 

Aby uzyskać więcej informacji przejdź do [Użycia programu Windows PowerShell dla StorSimple administrowania urządzenia](storsimple-windows-powershell-administration.md).

## <a name="azure-powershell-storsimple-cmdlets"></a>Azure polecenia cmdlet programu PowerShell StorSimple

Polecenia cmdlet StorSimple programu PowerShell Azure to zbiór poleceń cmdlet programu Windows PowerShell, które umożliwiają automatyzację zadań poziomu usług i migracji z poziomu wiersza polecenia. Aby uzyskać więcej informacji na temat polecenia cmdlet programu PowerShell Azure dla StorSimple przejdź do [informacje dotyczące poleceń cmdlet](https://msdn.microsoft.com/library/dn920427.aspx).

## <a name="storsimple-snapshot-manager"></a>Menedżer migawkę StorSimple

Menedżer migawkę StorSimple jest przystawką programu Microsoft Management Console (MMC), który służy do utworzenia spójnych, w chwili kopii lokalnej i danych w chmurze. Uruchamia przystawki na hoście z systemem Windows Server. Za pomocą Menedżera migawkę StorSimple w celu:

- Konfigurowanie, wykonywanie kopii zapasowej i usuwanie wielkości.
- Konfigurowanie grup głośność, aby upewnić się, którego kopię zapasową danych jest spójna aplikacji.
- Zarządzanie zasadami kopii zapasowej, tak aby danych kopii zapasowej interwałem zgodnie z harmonogramem i przechowywany w lokalizacji wyznaczonych (lokalnie lub w chmurze).
- Przywracanie wielkości i pojedyncze pliki.

Kopie zapasowe są przechwytywane jako migawki, które rejestrować tylko zmiany od czasu ostatniego migawkę wykonano i wymagają znacznie mniej miejsca do magazynowania niż pełne kopie zapasowe. Można tworzyć kopii zapasowej harmonogramów lub wykonać natychmiastowej kopie zapasowe, stosownie do potrzeb. Ponadto można StorSimple migawkę Menedżer ustanowić zasady przechowywania tego formantu, ile migawek zostaną zapisane. Jeśli chcesz później przywrócić dane z kopii zapasowej, umożliwia StorSimple migawkę menedżera, możesz wybrać z katalogu lokalnego lub migawki w chmurze. 

Danych lub jeśli chcesz przywrócić dane z innego powodu, Menedżer migawkę StorSimple przywraca go stopniowo jest odpowiednio. Przywracanie danych nie wymaga zamykania całego systemu podczas przywrócenia pliku, Zastąp sprzętu lub przenoszenie operacji do innej witryny.

Aby uzyskać więcej informacji, przejdź do [Co to jest Menedżer migawkę StorSimple?](storsimple-what-is-snapshot-manager.md)

## <a name="storsimple-adapter-for-sharepoint"></a>Karta StorSimple dla programu SharePoint

Microsoft Azure StorSimple zawiera kartę StorSimple dla programu SharePoint, składnik opcjonalny, który rozszerza przezroczysty StorSimple funkcje ochrony danych i magazynowania do farmy serwerów programu SharePoint. Karta współpracuje z dostawcą zdalny magazyn obiektów Blob (SPZ) i funkcję SQL Server SPZ umożliwia przenoszenie obiektów blob na serwerze kopii zapasowej przez system Microsoft Azure StorSimple. Microsoft Azure StorSimple następnie są przechowywane dane obiektów BLOB lokalnie lub w chmurze, na podstawie zużycia.

Karta StorSimple dla programu SharePoint jest zarządzany z poziomu portalu administracji centralnej programu SharePoint. W związku z tym zarządzania programu SharePoint pozostaje scentralizowane, a wszystkie miejsca do magazynowania wydaje się w farmie programu SharePoint.

Aby uzyskać więcej informacji przejdź do [Karty StorSimple dla programu SharePoint](storsimple-adapter-for-sharepoint.md). 

## <a name="storage-management-technologies"></a>Technologie zarządzania miejsca do magazynowania
 
Oprócz dedykowane urządzenia StorSimple, urządzenia wirtualnego i inne składniki Microsoft Azure StorSimple jest używane następujące technologie oprogramowania zapewnia szybki dostęp do danych i w celu zmniejszenia zużycia miejsca do magazynowania:

- [Automatyczne przechowywanie obsługi](#automatic-storage-tiering) 
- [Cienkie inicjowania obsługi administracyjnej](#thin-provisioning) 
- [Deduplication i kompresji](#deduplication-and-compression) 

### <a name="automatic-storage-tiering"></a>Automatyczne przechowywanie obsługi

Microsoft Azure StorSimple automatycznie rozmieszcza dane w logiczne poziomów na podstawie bieżącego użycia, wiek i relacji do innych danych. Dane, które są najbardziej aktywne jest przechowywany lokalnie, gdy automatycznie migracji mniej aktywny i nieaktywny danych w chmurze. Na poniższym diagramie przedstawiono tej metody miejsca do magazynowania.
 
![StorSimple miejsca do magazynowania warstwy](./media/storsimple-overview/hcs-data-services-storsimple-components-tiers.png)

Aby włączyć szybki dostęp, StorSimple przechowuje bardzo aktywna danych (ciepłej danych) na twarde SSD na urządzeniu StorSimple. Przechowywanych w niej danych, który jest używany czasami (ogrzewać danych) na dysków twardych w pliku lub na serwerach w centrum danych. Przenosi nieaktywny, dane kopii zapasowej, a dane przechowywane w Archiwizacja i zgodność w chmurze. 

>[AZURE.NOTE] W pakiecie 2 lub nowszy można określić woluminu przypięte lokalnie, w tym przypadku dane pozostają na urządzeniem lokalnym i nie jest tiered w chmurze. 

StorSimple skoryguje i rozmieszcza danych oraz zmienianie przydziałów magazynowania jako upodobania. Na przykład pewne informacje mogą być mniej aktywnego czasu. Jak zmieni się stopniowo mniej aktywnego, dysków twardych, a następnie w chmurze są migrowane z twarde SSD. Jeśli te same dane z aktywnym ponownie, jest on migracji z powrotem do na urządzeniu magazynującym.

Proces tiering miejsca do magazynowania przebiega w następujący sposób:

1. Administrator systemu konfiguruje konto Microsoft Azure chmury miejsca do magazynowania.
2. Administrator używa konsoli szeregowego i usługa Menedżer StorSimple (działa w portalu klasyczny Azure) do skonfigurowania serwera urządzenie i plik, tworzenie zasad ochrony wielkości i danych. Maszyny lokalnego (na przykład serwery plików) umożliwia uzyskać dostęp do urządzenia StorSimple Internet Small komputera System Interface (iSCSI).
3. Początkowo StorSimple są przechowywane dane w warstwie SSD szybkie urządzenia.
4. Jak warstwie SSD zbliża się wydajność, StorSimple deduplicates i kompresuje najstarszych bloki danych i przenosi je do poziomu dysk twardy.
5. Jako możliwości dysk twardy warstwa metod StorSimple są szyfrowane najstarszych bloki danych i bezpiecznie wysyła je do konta Microsoft Azure miejsca do magazynowania przy użyciu protokołu HTTPS.
6. Microsoft Azure tworzy wielu replikach dane w jego centrum danych i zdalny centrum danych, zapewnienie, że dane można odzyskać, jeśli występuje po awarii. 
7. Gdy serwer plików żąda danych przechowywanych w chmurze, StorSimple zwraca go bezproblemowo i przechowywana kopia w warstwie SSD urządzenia StorSimple.

### <a name="thin-provisioning"></a>Cienkie inicjowania obsługi administracyjnej

Cienkie inicjowania obsługi administracyjnej jest technologii wirtualizacji, w której znajduje się dostępnego magazynu przekroczyć fizycznie zasobów. Zamiast rezerwowania wystarczającą ilość miejsca z wyprzedzeniem, StorSimple przydzielić tylko za mało miejsca do wymagań bieżącej używa cienkie inicjowania obsługi administracyjnej. Elastyczne rodzaj magazynu w chmurze ułatwia tej metody, ponieważ StorSimple można zwiększyć lub zmniejszyć magazynu w chmurze do spełnienia wymagań zmiany. 

>[AZURE.NOTE] Nie zainicjowano znacznie lokalnie przypięty wielkości. Po utworzeniu wielkość miejsca do magazynowania przydzielone do woluminu tylko lokalne jest obsługi administracyjnej w całości.

### <a name="deduplication-and-compression"></a>Deduplication i kompresji

Microsoft Azure StorSimple używa kompresji deduplication i dane, aby jeszcze bardziej zmniejszyć wymagania dotyczące miejsca do magazynowania.

Deduplication zmniejsza ogólnej ilości danych przechowywanych przez eliminowania nadmiarowości w zestawie danych przechowywanych. Zmian informacji StorSimple ignoruje danych bez zmian i rejestruje tylko zmiany. Ponadto StorSimple powoduje zmniejszenie ilości danych przechowywanych identyfikujący typ przepływu pracy i usuwając niepotrzebne informacje. 

>[AZURE.NOTE] Dane ilości lokalnie przypięty nie jest deduplicated lub skompresowany. Jednak kopie zapasowe wielkości lokalnie przypięty są deduplicated i skompresowany.

## <a name="storsimple-workload-summary"></a>Podsumowanie obciążenie pracą StorSimple

Podsumowanie obsługiwane obciążenia StorSimple są oznaczane znakami tabulacji poniżej.

| Scenariusz                  | Obciążenie pracą                | Obsługiwane |  Ograniczenia                                  | Wersja              |
|---------------------------|-------------------------|-----------|------------------------------------------------|----------------------|
| Współpraca za pomocą             | Udostępnianie plików            | Tak       |                                                | Wszystkie wersje         |
| Współpraca za pomocą             | Udostępnianie plików rozłożone| Tak       |                                                | Wszystkie wersje         |
| Współpraca za pomocą             | Programu SharePoint              | Tak *      |Obsługiwane tylko w przypadku lokalnie przypięty wielkości      | Aktualizacja 2 lub nowszy   |
| Archiwizacja                  | Plik protokołu Simple archiwizowania   | Tak       |                                                | Wszystkie wersje         |
| Wirtualizacji            | Maszyn wirtualnych        | Tak *      |Obsługiwane tylko w przypadku lokalnie przypięty wielkości      | Aktualizacja 2 lub nowszy   |
| Bazy danych                  | SQL                     | Tak *      |Obsługiwane tylko w przypadku lokalnie przypięty wielkości      | Aktualizacja 2 lub nowszy   |
| Monitorowania wideo        | Monitorowania wideo      | Tak *       |Obsługiwane, gdy urządzenie StorSimple jest przeznaczony tylko do tej pracy| Aktualizacja 2 lub nowszy   |
| Wykonywanie kopii zapasowych                    | Kopia zapasowa podstawowego docelowej | Tak *      |Obsługiwane, gdy urządzenie StorSimple jest przeznaczony tylko do tej pracy| Aktualizacja 3 lub nowszy |
| Wykonywanie kopii zapasowych                    | Kopia zapasowa pomocniczej docelowej | Tak *      |Obsługiwane, gdy urządzenie StorSimple jest przeznaczony tylko do tej pracy| Aktualizacja 3 lub nowszy |

*Tak & #42; — Rozwiązanie wskazówki i ograniczenia powinny być stosowane.*

Następujące obciążenia nie są obsługiwane przez urządzenia serii StorSimple 8000. Jeśli używany na StorSimple, obciążenie spowoduje nieobsługiwanej konfiguracji.

-  Tworzenie medycznych obrazów
-  Exchange
-  VDI
-  Oracle
-  SAP
-  Duży danych
-  Dystrybucja zawartości
-  Uruchamianie z SCSI

Poniżej przedstawiono listę elementów infrastruktury StorSimple obsługiwane. 

| Scenariusz | Obciążenie pracą      | Obsługiwane |  Ograniczenia                                 | Wersja      |
|----------|---------------|-----------|-----------------------------------------------|--------------|
| Ogólne  | Trasa Express | Tak       |                                               |Wszystkie wersje |
| Ogólne  | DataCore FC   | Tak *       |Obsługiwane z DataCore SANsymphony            | Wszystkie wersje |
| Ogólne  | DFSR          | Tak *      |Obsługiwane tylko w przypadku lokalnie przypięty wielkości     | Wszystkie wersje |
| Ogólne  | Indeksowanie      | Tak *       |Do przedstawienia wielkości warstwowych, jest obsługiwana tylko metadanych indeksowania (Brak danych).<br>Dla lokalnie przypięty wielkości pełną indeksowanie jest obsługiwana.| Wszystkie wersje |
| Ogólne  | Oprogramowania antywirusowego    | Tak *       |Do przedstawienia wielkości warstwowych jest obsługiwana tylko skanowanie przy otwieraniu i zamknij.<br> Dla lokalnie przypięty wielkości pełne skanowanie jest obsługiwana.| Wszystkie wersje |

*Tak & #42; — Rozwiązanie wskazówki i ograniczenia powinny być stosowane.*

## <a name="storsimple-terminology"></a>Terminologia StorSimple 

Przed wdrożeniem tego rozwiązania Microsoft Azure StorSimple, zalecamy zapoznanie poniższe terminy i definicje.

### <a name="key-terms-and-definitions"></a>Kluczowe terminy i definicje

| Termin (skrót lub skrót) | Opis |
| ------------------------------ | ---------------- |
| rekord kontroli dostępu (awaryjnego)    | Rekord skojarzony z woluminu na urządzeniu Microsoft Azure StorSimple określające, które hosty można się z nim połączyć. Oznaczanie jest oparty na iSCSI kwalifikowana nazwa (IQN) hostów (zawarte w awaryjnego) łączących się z urządzeniem StorSimple.|
| AES-256                        | 256-bitowy algorytm szyfrowania AES (Advanced Standard) do szyfrowania danych podczas przenoszenia go do i z chmury. |
| rozmiar jednostki alokacji (Australia)     | Systemy plików najmniejsza ilość miejsca na dysku, która może być przeznaczona do przechowywania pliku w systemie Windows. Jeśli rozmiar pliku nie jest wielokrotnością rozmiaru klaster, należy dodatkowe miejsce do przechowywania pliku (do następnego wielokrotnością rozmiaru klaster) uzyskując utracone miejsca i system dysku twardego. <br>Australia polecane do przedstawienia wielkości Azure StorSimple jest 64 KB, ponieważ działa on również z algorytmów deduplication.|
| obsługi automatycznego przechowywania      | Automatyczne przenoszenie mniej aktywnego danych z twarde SSD dysków twardych, a następnie warstwa w chmurze, a następnie włączać zarządzania wszystkie miejsca do magazynowania w interfejsie użytkownika centralnej.|
| wykaz kopii zapasowych | Kolekcja kopie zapasowe, zwykle powiązane przez typ aplikacji, która została użyta. Ten zbiór zostanie wyświetlony na stronie katalog kopii zapasowej usługi Menedżera StorSimple interfejsu użytkownika.|
| Plik kopii zapasowej wykazu             | Plik zawierający listę dostępnych migawek przechowywanych w kopii zapasowej bazy danych programu StorSimple migawkę Manager. |
| zasady kopii zapasowej                   | Wybór wielkości, typ kopii zapasowej i harmonogram, która umożliwia tworzenie kopii zapasowych na wstępnie zdefiniowanego harmonogramu.|
| duże obiekty binarne (BLOB)    | Kolekcja dane binarne przechowywane jako całość w systemie zarządzania bazy danych. BLOB są zwykle obrazy, audio lub inne obiekty multimedialne, jednak czasami binarne kod wykonywalny jest przechowywana jako obiektów BLOB.|
| Wyzwania uzgadniania uwierzytelniania protokołu (CHAP) | Protokół używany do uwierzytelniania równorzędnych połączenie, oparte na partnerze udostępniania hasła lub hasło. CHAP mogą być jednokierunkowe lub wzajemnego. Z jednokierunkowe CHAP docelowej uwierzytelnia inicjator. Wzajemnego CHAP wymaga, że obiekt docelowy uwierzytelnić Inicjator i że inicjator uwierzytelnia docelowej. | 
| klonowanie                          | Kopię woluminu. |
|Chmura jako warstwa (CaaT)          | Chmury miejsca do magazynowania scałkowanej w przedziale jako warstwa w architekturze miejsca do magazynowania, tak, aby wszystkie miejsca do magazynowania jest wyświetlany jako część jedną sieć miejsca do magazynowania.|
| chmury usługi dostawcę   | Dostawca środowiska usługi w chmurze.|
| Migawka chmury                 | W chwili kopia głośność dane, które są przechowywane w chmurze. Migawki chmury odpowiada migawkę replikować na komputerze z inną, poza przestrzeni dyskowej. Migawki chmurze są szczególnie przydatne w sytuacjach odzyskiwania po awarii.|
| klucza szyfrowania w chmurze miejsca do magazynowania   | Hasła lub klucza używane przez urządzenia StorSimple uzyskiwać dostęp do danych zaszyfrowanych wysyłane przez urządzenia w chmurze.|
| Aktualizowanie obsługą klastrów         | Zarządzanie aktualizacje oprogramowania na serwerach w klastrze pracy awaryjnej, aby uniknąć aktualizacje minimalnego lub nie mają wpływu na dostępność usługi.|
| ścieżki danych                       | Kolekcja jednostkami organizacyjnymi wykonywania operacji wzajemnie połączonych przetwarzania danych.|
| Dezaktywowanie                     | Trwały akcję, która spowoduje przerwanie połączenia między urządzeniem StorSimple i usług w chmurze skojarzone. Migawki chmury urządzenia po ten proces i może być klonowany lub używana awarii.|
| odzwierciedlające dysku                 | Replikacji wielkości dysku logicznego w osobnych słabo dysków w czasie rzeczywistym, aby zapewnić stałą dostępność.|
| dysk dynamiczny odzwierciedlające         | Replikacja wielkości dysku logicznego na dyskach dynamicznych.|
| dyski dynamiczne                  | Format głośności dysku, który używa dysku Menedżer Logicznych do przechowywania i zarządzanie danymi na wielu dyskach fizycznych. Dyski dynamiczne mogą powiększone o podanie więcej miejsca.|
| Rozszerzone załącznik wiele dysków (EBOD) | Pomocnicza załącznik urządzenia Microsoft Azure StorSimple zawierający dodatkowego dysku twardego dysków w poszukiwaniu dodatkowego miejsca do magazynowania.|
| FAT inicjowania obsługi administracyjnej               | Konwencjonalny magazynowania inicjowania obsługi administracyjnej w magazynie, które przydzielono miejsca na podstawie przewidywanych potrzeb (i zazwyczaj jest poza bieżącą konieczności). Zobacz też: *cienkie inicjowania obsługi administracyjnej*.|
| na dysku twardym (dysk twardy)          | Dysk, na którym używa obracania płyt do przechowywania danych.|
| hybrydowe magazynu w chmurze           | Architektura miejsca do magazynowania, która korzysta z zasobów lokalnych i poza nim, w tym magazynu w chmurze.|
| Internet Small komputera System Interface (iSCSI) | Magazynu opartego na protokole IP standard sieciowy łączenia urządzenia do przechowywania danych lub urządzenia.|
| inicjatorach                 | Składnik oprogramowania, który umożliwia komputera z systemem Windows, aby nawiązać połączenie z siecią zewnętrznych magazynu opartego na iSCSI.|
| iSCSI kwalifikowana nazwa (IQN)      | Unikatowa nazwa identyfikująca docelowego iSCSI lub inicjator.|
| obiekt docelowy iSCSI                    | Składnik oprogramowania, który udostępnia scentralizowaną iSCSI podsystemów dysków w sieci magazynowania.|
| Live archiwizacji                  | Podejście miejsca do magazynowania, w którym archiwizacji dane są dostępne cały czas (go nie znajduje się poza terenem zakładu na taśmą, na przykład). Microsoft Azure StorSimple korzysta z programu live archiwizacji.|
|lokalnie przypięty głośności | głośność, który znajduje się na tym urządzeniu, a nigdy nie jest warstwowa w chmurze. |
| lokalne migawki                  | W chwili kopia danych głośność, który jest przechowywany na tym urządzeniu Microsoft Azure StorSimple.|
| StorSimple Microsoft Azure      | Zaawansowane rozwiązanie składający się z urządzenia magazynowania centrum danych i oprogramowania, która umożliwia organizacjom wykorzystać magazynu w chmurze, tak jakby była ona magazynowania centrum danych. StorSimple ułatwia ochronę danych i zarządzanie danymi redukując koszty. Rozwiązanie powoduje konsolidowanie podstawową miejsca do magazynowania, archiwum, wykonywanie kopii zapasowych i awarii odzyskiwania (DR) za pośrednictwem Integracja z chmury. Łącząc zarządzania SAN chmury i magazynowania danych na platformie klasy korporacyjnej, urządzenia StorSimple umożliwić szybkość, uproszczenia i niezawodność dla wszystkich potrzeb związanych z miejsca do magazynowania.|
| Zasilania i chłodzenia moduł (PCM)  | Składniki sprzętu urządzenia StorSimple składający się ze źródła zasilania oraz wentylatorów, w związku z tym nazwa Power i chłodzenia modułu. Podstawowy załącznik urządzenie ma dwa PCMs 764W załącznik EBOD ma dwa PCMs 580W.|
| podstawowy załącznik               | Załącznik głównym urządzenia StorSimple, która zawiera kontrolery platformy aplikacji.|
| cel czasu odzyskiwania (RTO)   | W pełni po przywróceniu maksymalną ilość czasu, który powinien być wydatkowaną przed proces biznesowy lub systemu po awarii.| 
|kolejną dołączone SCSI (SA)       | Typ na dysku twardym (dysk twardy).|
| klucza szyfrowania danych usługi     | Klucz udostępnione do każdego nowego urządzenia StorSimple rejestruje usłudze Menedżer StorSimple. Dane konfiguracji usługa Menedżera StorSimple i urządzenie jest zaszyfrowany przy użyciu klucza publicznego, a następnie można odszyfrować tylko na tym urządzeniu, przy użyciu klucza prywatnego. Klucza szyfrowania danych usługi umożliwia usługę, aby uzyskać klucz prywatny do odszyfrowywania.|
| klucz rejestru usługi        | Klucz, który ułatwia zarejestrować urządzenia StorSimple usłudze Menedżer StorSimple, tak aby była wyświetlana w portalu klasyczny Azure dla przyszłych zarządzania działań.|
| Małe komputera System Interface (SCSI) | Zestaw standardów fizycznie łączenia komputerów i przekazywania danych między nimi.|
| dysk stanie stałym (SSD)         | Dysk zawierający ani ruchomych elementów; na przykład dysk flash.|
| Konto miejsca do magazynowania                 | Zestaw poświadczenia dostępu do połączonych z kontem miejsca do magazynowania dla danej chmury dostawcy usług.| 
| Karta StorSimple dla programu SharePoint| Składnik Microsoft Azure StorSimple przezroczysty rozszerza StorSimple miejsca do magazynowania i ochrony danych do farmy serwerów programu SharePoint.|
| Usługa Menedżera StorSimple      | Rozszerzenie portalu klasyczny Azure umożliwia zarządzanie Azure StorSimple lokalnego i urządzeń wirtualnych.|
| Menedżer migawkę StorSimple     | Microsoft Management Console (MMC) przystawki zarządzania kopia zapasowa i przywracanie operacje w programie Microsoft Azure StorSimple.|
| sporządzanie kopii zapasowej                     | Funkcja, która umożliwia użytkownikowi przejęcie interakcyjnych wykonywania kopii zapasowej woluminu. To alternatywny sposób podjęcia ręczną kopię zapasową woluminu zamiast korzystających z automatycznej kopii zapasowej za pomocą zdefiniowanych zasad.|
| cienkie inicjowania obsługi administracyjnej               | Metoda optymalizowania wydajności, z którym dostępnego miejsca jest używana w systemach miejsca do magazynowania. W zubożonym inicjowania obsługi administracyjnej, przechowywania przydzielonego przez wielu użytkowników według minimalny odstęp wymagany przez każdego użytkownika w dowolnym momencie. Zobacz też *fat inicjowania obsługi administracyjnej*.|
| obsługi | Rozmieszczanie danych w logiczne grupowanie na podstawie bieżącego użycia, wiek i relacji do innych danych. StorSimple automatycznie rozmieszcza dane w warstwy.   |
| Głośność                          | Obszary logiczną magazynowania przedstawione w postaci dyski. Ilości StorSimple odpowiadają wielkość zainstalowanych przez hosta, łącznie z tymi wykryte iSCSI i urządzenia StorSimple.|
 | kontener głośności                | Grupa wielkości i ustawień, które dotyczą. Wszystkie ilości w urządzeniu StorSimple są pogrupowane w kontenerów głośność. Ustawienia kontenera głośność obejmują konta miejsca do magazynowania, ustawienia szyfrowania dane wysyłane do chmury za pomocą klawiszy skojarzony szyfrowania i przepustowości operacji dotyczących w chmurze.|
| Grupa głośności                    | W Menedżerze migawkę StorSimple grupy głośność jest zbiór wielkości skonfigurowane tak, aby ułatwić przetwarzanie kopii zapasowej.|
| Usługi kopiowania woluminu w tle (VSS)| Usługa systemu operacyjnego Windows Server, która zapewnia spójność aplikacji przez komunikowania się z aplikacji obsługującej VSS koordynowanie tworzenie migawek przyrostowe. VSS gwarantuje, że aplikacje są tymczasowo nieaktywne, gdy są wykonywane migawki.|
| Windows PowerShell dla StorSimple | Oparte na programie Windows PowerShell interfejs wiersza polecenia używane do działania i zarządzanie Twoim urządzeniem StorSimple. Przy zachowaniu niektóre podstawowe funkcje programu Windows PowerShell, to interfejs ma dodatkowe dedykowane polecenia cmdlet, które są przeznaczone dla zarządzanie urządzeniem StorSimple.|


## <a name="next-steps"></a>Następne kroki

Informacje dotyczące [zabezpieczeń StorSimple](storsimple-security.md).




 

 
