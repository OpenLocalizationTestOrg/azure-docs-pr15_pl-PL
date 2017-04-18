<properties
   pageTitle="Omówienie tablicy Virtual StorSimple | Microsoft Azure"
   description="W tym artykule opisano StorSimple tablicy wirtualnych zintegrowane masowej, zarządzającą zadań magazynu między lokalnej wirtualną urządzenia i magazynu w chmurze Microsoft Azure."
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
   ms.date="10/06/2016"
   ms.author="alkohli" />

# <a name="introduction-to-the-storsimple-virtual-array"></a>Wprowadzenie do tablicy wirtualnych StorSimple

## <a name="overview"></a>Omówienie

Zapraszamy do programu Microsoft Azure StorSimple wirtualnych tablicy, zintegrowane masowej, zarządzającą zadań magazynu między lokalnego wirtualne urządzenie działa monitor maszyny wirtualnej i Microsoft Azure magazynu w chmurze. Wirtualna tablicy (nazywane także StorSimple lokalnej wirtualną urządzenia) to serwera plików wydajność, optymalne koszty i łatwe w zarządzaniu lub iSCSI rozwiązanie eliminuje wiele problemów i koszty skojarzone z przedsiębiorstwa miejsca do magazynowania i ochrony danych. Wirtualna tablica jest szczególnie nadają się do zdalnego/oddział scenariusze pakietu office (ROBO).

To omówienie omówiono wirtualnych tablicy. 

- Aby uzyskać omówienie serii StorSimple 8000, przejdź do [serii StorSimple 8000: rozwiązanie chmury hybrydowych](storsimple-overview.md). 

- Informacje StorSimple 5000-7000 urządzenia serii przejdź do [StorSimple pomocy Online](http://onlinehelp.storsimple.com/).

Wirtualna tablicy obsługuje iSCSI lub protokół bloku komunikatów serwera (SMB). Działa na istniejącą infrastrukturę monitor maszyny wirtualnej, a zawiera obsługi chmury, wykonywanie kopii zapasowych w chmurze, szybkie przywracanie, odzyskiwania poziomie elementu i funkcji odzyskiwania po awarii.

W poniższej tabeli przedstawiono ważne funkcje wirtualnych tablicy.

| Funkcja | Wirtualna tablicy |
| ------- | ------------- |
|Wymagania dotyczące instalacji | Używa infrastruktury wirtualizacji (Hyper-V lub VMware)|
| Dostępność | Pojedynczy węzeł |
| Pojemność (łącznie z chmury) |Maksymalnie 64 użyteczne TB na urządzenie wirtualne |
| Wydajność lokalną | 390 GB do 6,4 TB użyteczne na urządzenie wirtualne (wymagana do obsługi administracyjnej 500 GB do 8 TB miejsca na dysku)|
| Protokoły natywnych | iSCSI lub SMB |
| Cel czasu odzyskiwania (RTO) | iSCSI: mniej niż 2 minuty bez względu na rozmiar |
| Odzyskiwanie punktów cel (RPO) | Dzienny i kopii zapasowych na żądanie |
| Obsługi miejsca do magazynowania | Przy użyciu ciepła mapowanie w celu określenia, jakie dane powinny tiered, lub pomniejszyć |
| Pomoc techniczna | Infrastruktura wirtualizacji obsługiwane przez dostawcę |
| Wydajność | Zależnie od infrastruktury podstawowej |
| Mobilność danych | Na tym samym urządzeniu można przywrócić element poziomie lub odzyskiwania (plik server) |
| Poziomy miejsca do magazynowania | Monitor maszyny wirtualnej lokalnego przechowywania i chmury |
| Udostępnianie rozmiar |Tiered: maksymalnie 20 TB; lokalnie przypięte: do 2 TB |
| Rozmiar woluminu |Tiered: maksymalnie 5 TB; lokalnie przypięte: maksymalnie 500 GB |
| Migawki | Spójne ze stanem awarii |
| Odzyskiwanie na poziomie elementu | Tak; użytkownicy mogli przywrócić z akcji |

## <a name="why-use-storsimple"></a>Dlaczego warto używać StorSimple?

StorSimple łączy użytkowników i serwerów Azure miejsca do magazynowania w minutach, nie modyfikacji aplikacji.

W poniższej tabeli opisano niektóre z najważniejszych korzyści, które zawiera rozwiązanie wirtualnych tablicy.

| Funkcja | Korzyści |
|---------|---------|
| Integracja przezroczystości | Wirtualna tablicy obsługuje iSCSI lub protokół SMB. Przenoszenie danych między lokalnym warstwa i warstwa chmurze jest bezproblemowa i niewidoczne dla użytkownika.|
| Koszty ograniczona miejsca do magazynowania | Z StorSimple obsługi administracyjnej lokalnego magazynu wystarczających do spełnienia wymagań bieżącej ciepłej najczęściej używanych danych. Miejsca do magazynowania potrzeb powiększanie, StorSimple poziomów zimnej dane efektywne pod względem kosztów w chmurze miejsca do magazynowania. Dane są deduplicated i skompresowane przed wysłaniem w chmurze, aby jeszcze bardziej zmniejszyć wymagania dotyczące przechowywania i wydatków.|
| Zarządzanie magazynem uproszczone | StorSimple udostępnia centralne zarządzanie w chmurze za pomocą Menedżera StorSimple Zarządzanie wielu urządzeń.| 
| Ulepszone odzyskiwanie i zgodność | StorSimple ułatwia szybsze odzyskiwanie natychmiast przywracanie metadanych i przywracanie danych zgodnie z potrzebami. Oznacza to, że normalnego działania można kontynuować pracę z minimalnymi zakłócenia w pracy.|
| Mobilność danych | Dane tiered w chmurze są dostępne z innych witryn na potrzeby odzyskiwania i migracji. Należy zauważyć, że danych można przywrócić tylko do tablicy wirtualnych oryginalnej. Jednak użyć funkcji odzyskiwania po awarii Aby przywrócić całą macierz wirtualnych innego wirtualną tablicy.|

## <a name="storsimple-workload-summary"></a>Podsumowanie obciążenie pracą StorSimple

Podsumowanie obsługiwane obciążenia StorSimple są oznaczane znakami tabulacji poniżej.

| Scenariusz                | Obciążenie pracą              | Obsługiwane |  Ograniczenia                                  | Wersja              |
|-------------------------|-----------------------|-----------|------------------------------------------------|----------------------|
| Współpraca za pomocą ROBO      | Udostępnianie plików          | Tak       | Zobacz [maksymalną serwera plików](storsimple-ova-limits.md). <br>Zobacz [wymagania systemowe dla obsługiwanych wersji SMB](storsimple-ova-system-requirements.md).   | Wszystkie wersje      |


## <a name="workflows"></a>Przepływy pracy

Tablica wirtualnych StorSimple jest szczególnie odpowiednie dla następujących przepływy pracy:

- [Zarządzanie w chmurze](#cloud-based-storage-management)
- [Niezależny od lokalizacji kopii zapasowej](#location-independent-backup)
- [Odzyskiwanie danych ochrony i danych](#data-protection-and-disaster-recovery)

### <a name="cloud-based-storage-management"></a>Zarządzanie w chmurze

Usługę Menedżer StorSimple działa w portalu klasyczny Azure umożliwia zarządzanie danymi przechowywanymi na wielu urządzeniach i w wielu lokalizacjach. To jest szczególnie przydatne w sytuacjach gałąź rozłożone. Należy zauważyć, że należy utworzyć osobne wystąpienia usługę Menedżer StorSimple Zarządzanie fizycznymi StorSimple i tablic wirtualnych. 

![Zarządzanie w chmurze](./media/storsimple-ova-overview/cloud-based-storage-management.png)

### <a name="location-independent-backup"></a>Niezależny od lokalizacji kopii zapasowej

Z tabeli wirtualnej migawek chmury zapewniają niezależny od lokalizacji, w chwili kopia woluminu lub udostępnianie. Chmura migawki są domyślnie włączone i nie można wyłączyć. Wszystkie wielkości i akcje są kopii zapasowej w górę w tym samym czasie za pomocą jednego dzienny zasad kopii zapasowej, a można wykonać dodatkowe ad hoc kopie zapasowe, gdy jest to konieczne.

### <a name="data-protection-and-disaster-recovery"></a>Odzyskiwanie danych ochrony i danych

Wirtualna tablicy obsługuje następujących scenariuszy odzyskiwania danych ochrony i danych:

- **Przywracanie woluminu lub udostępnianie** — Użyj przywracania jako nowego przepływu pracy aby odzyskać głośność lub udostępnianie. Aby odzyskać całego woluminu lub udostępnianie za pomocą tej metody.
- **Odzyskiwanie poziomie elementu** — akcje dostęp do uproszczone ostatnie kopie zapasowe. Można łatwo odzyskać osobne pliki z folderu .backup specjalne dostępne w chmurze. Ta funkcja przywracania jest sterowanych przez użytkownika i nie interwencji administratora jest wymagany.
- **Odzyskiwanie** — Użyj możliwość pracy awaryjnej odzyskiwania wszystkich wielkości lub akcji do nowej tablicy wirtualną. Możesz Tworzenie nowej tablicy wirtualnych zarejestrować go w usłudze Menedżer StorSimple, a następnie kończą się niepowodzeniem w tablicy wirtualnych oryginalnej. Nowej tablicy wirtualnych przyjmie następnie ustanawianie zasobów. 

## <a name="virtual-array-components"></a>Wirtualna elementów tablicy.

Wirtualna tablica zawiera następujące składniki:

- [Wirtualna tablicy](#virtual-array) — na urządzeniu magazynującym chmury hybrydowych według maszyny wirtualnej obsługi administracyjnej w środowisku wirtualizowanym lub monitor maszyny wirtualnej.  
- [Usługa Menedżera StorSimple](#storsimple-manager-service) — rozszerzenie portalu klasyczny Azure umożliwia zarządzanie urządzeniami StorSimple co najmniej jeden z interfejsu jednej sieci web, którego można korzystać z różnych lokalizacji geograficznych. Usługa Menedżera StorSimple umożliwia tworzenie i zarządzanie usługami, wyświetlanie i zarządzanie urządzeniami i alertów i zarządzanie wielkości, udziały i istniejące migawek.
- [Interfejs użytkownika lokalnego sieci web](#local-web-user-interface) — oparte na sieci web interfejsu użytkownika, który jest używany do konfigurowania urządzenia, aby go połączenie sieci lokalnej, a następnie zarejestrować urządzenia z usługą Menedżera StorSimple. 
- [Interfejs wiersza polecenia](#command-line-interface) — interfejs programu Windows PowerShell, który umożliwia uruchamianie sesji pomocy technicznej dla wirtualnych tablicy.
W poniższych sekcjach opisano każdą z tych składników bardziej szczegółowo i wyjaśniono, jak rozwiązanie rozmieszcza danych, przydziela miejsca do magazynowania i ułatwia zarządzanie magazynem i ochrony danych.

### <a name="virtual-array"></a>Wirtualna tablicy

Wirtualna tablica jest rozwiązanie węzeł jednego miejsca do magazynowania, które umożliwia przechowywanie podstawowego, zarządza komunikacji z magazynu w chmurze i pomaga w celu zapewnienia bezpieczeństwa i poufności wszystkich danych, który jest przechowywany na tym urządzeniu.

Wirtualna tablica jest dostępne w jednym modelu, który jest dostępny do pobrania. Macierz zawiera maksymalna pojemność 6,4 TB na urządzeniu (z źródłowych wymaganie miejsca do magazynowania 8 TB) i tym 64 TB miejsca do magazynowania w chmurze. 

Wirtualna tablica zawiera następujące funkcje:

- Działa kosztów. Go używa istniejącą infrastrukturę wirtualizacji oraz mogą być rozmieszczone na do istniejących funkcji Hyper-V lub VMware monitor maszyny wirtualnej.
- Znajduje się w obrębie centrum danych, a można skonfigurować jako serwerem lub na serwerze plików. 
- Jest zintegrowany z chmury.
- Kopie zapasowe są przechowywane w chmurze, która może ułatwić odzyskiwanie i uprościć odzyskiwania poziomie elementu (ILR). 
- Tak, jak chcesz je zastosować do urządzenia fizycznego, możesz zastosować aktualizacje do wirtualnego tablicy.

>[AZURE.NOTE] Nie można rozwinąć wirtualnych tablicy. W związku z tym należy do zapewniania obsługi dostępna odpowiednia ilość miejsca do magazynowania, podczas tworzenia urządzenia wirtualnego. 

### <a name="storsimple-manager-service"></a>Usługa Menedżera StorSimple

Microsoft Azure StorSimple udostępnia interfejs użytkownika oparte na sieci web (usługa Menedżer StorSimple), który umożliwia centralne zarządzanie Centrum danych i magazynu w chmurze. Usługa Menedżera StorSimple umożliwia wykonywanie następujących zadań:

- Można zarządzać wieloma tablic wirtualnych StorSimple z jednej usługi. 
- Konfigurowanie i zarządzanie ustawień zabezpieczeń dla urządzeń StorSimple. (Szyfrowania w chmurze jest uzależniony od firmy Microsoft Azure API).
- Konfigurowanie magazynu poświadczeń konta i właściwości.
- Konfigurowanie i zarządzanie wielkości lub akcji.
- Tworzenie kopii zapasowych i przywracania danych na wielkości lub akcji.
- Monitorowanie wydajności.
- Przejrzyj ustawienia systemowe i identyfikowanie ewentualnym problemom.

Usługę Menedżer StorSimple umożliwia wykonywanie codzienne zadania administracyjne usługi wirtualnych tablicy.

Aby uzyskać więcej informacji przejdź do tematu [Używanie usługę Menedżer StorSimple administrowania urządzenia StorSimple](storsimple-manager-service-administration.md).

### <a name="local-web-user-interface"></a>Interfejs użytkownika lokalnego sieci web

Wirtualna tablica zawiera oparte na sieci web interfejsu użytkownika, który jest używany do jednorazowej konfiguracji i rejestracji urządzenia z usługą Menedżera StorSimple. Można ją zamknij i uruchom ponownie wirtualnych tablicy, uruchom testy diagnostyczne, aktualizacji, Zmień hasło administratora urządzenia, wyświetlanie dzienniki systemu i skontaktuj się z programu Microsoft Support na zgłoszenie żądania usługi. 

Aby dowiedzieć się, jak za pomocą interfejsu użytkownika oparte na sieci web przejdź do tematu [Używanie oparte na sieci web interfejs użytkownika do zarządzania macierzy Virtual StorSimple](storsimple-ova-web-ui-admin.md).

### <a name="command-line-interface"></a>Interfejs wiersza polecenia

Uwzględniane interfejsu programu Windows PowerShell umożliwia zainicjować sesji pomocy technicznej z Microsoft Support tak, aby ich może ułatwić rozwiązywanie problemów z i rozwiązywania problemów, które mogą wystąpić na urządzeniu wirtualną.

## <a name="storage-management-technologies"></a>Technologie zarządzania miejsca do magazynowania

Oprócz wirtualnych tablicy i innych składników do zapewnienia szybkiego dostępu do ważnych danych, zmniejszenia zużycia miejsca do magazynowania i ochrona danych przechowywanych w macierzy wirtualnych rozwiązanie StorSimple używane następujące technologie oprogramowania:

- [Automatyczne przechowywanie obsługi](#automatic-storage-tiering) 
- [Lokalnie przypięty akcje i ilości](#locally-pinned-shares-and-volumes)
- [Deduplication i kompresja danych tiered lub kopii zapasowej w chmurze](#deduplication-and-compression-for-data-tiered/backed-up-to-the-cloud) 
- [Zaplanowane i na żądanie kopii zapasowych](#scheduled-and-on-demand-backups)

### <a name="automatic-storage-tiering"></a>Automatyczne przechowywanie obsługi

Wirtualna tablicy do zarządzania przechowywane dane w wirtualnej macierzy i chmury używane nowego mechanizmu tiering. Dostępne są tylko dwa poziomy: lokalnej wirtualną tablicy i Azure magazynu w chmurze. Tablica Virtual StorSimple automatycznie rozmieszcza danych poziomów oparte na konturowy, która śledzi bieżące użycie, wiek i relacje z innymi danymi. Dane, które są najbardziej aktywne (najnowszych) są przechowywane lokalnie, gdy automatycznie migracji mniej aktywny i nieaktywny danych w chmurze. (Wszystkie kopie zapasowe są przechowywane w chmurze). StorSimple skoryguje i rozmieszcza danych oraz zmienianie przydziałów magazynowania jako upodobania. Na przykład pewne informacje mogą być mniej aktywnego czasu. Jak zmieni się stopniowo mniej aktywnego, jest warstwowa się w chmurze. Jeśli te same dane z aktywnym ponownie, jest warstwowa w tablicy miejsca do magazynowania.

Dane dla akcji warstwowych lub ilością jest gwarantowana własną przestrzeń lokalne warstwy. (około 10% ustanawianie miejsca dla tej Udostępnij lub głośność). Gdy zmniejsza dostępnego magazynu na tym urządzeniu wirtualnych dla tej Udostępnij lub głośność, gwarantuje, że obsługi dla jednej Udostępnij lub głośność nie wpłynie zgodnie z potrzebami tiering innych akcji lub wielkości. Dlatego bardzo zajęty obciążenie pracą na jednym Udostępnij lub głośność nie można wymusić inne obciążenia w chmurze. 

![automatyczne przechowywanie obsługi](./media/storsimple-ova-overview/automatic-storage-tiering.png)

>[AZURE.NOTE] Umożliwia określenie woluminu przypięte lokalnie, w tym przypadku dane pozostają na tablicy wirtualnych i nigdy nie jest tiered w chmurze. Aby uzyskać więcej informacji przejdź do [lokalnie przypięte udział i ilości](#locally-pinned-shares-and-volumes).

### <a name="locally-pinned-shares-and-volumes"></a>Lokalnie przypięty akcje i ilości

Możesz utworzyć odpowiednie akcje i ilości lokalnie przypięte. Ta funkcja zapewnia, że dane wymagane przez aplikacje krytyczne pozostaje w wirtualnej tablicy i nigdy nie jest warstwowa w chmurze. Akcje lokalnie przypięte i ilości są następujące funkcje: 

- Nie są one podlegają opóźnienia w chmurze lub problemy z łącznością.
- Są nadal korzystać ze StorSimple chmury kopii zapasowej i awarii funkcji odzyskiwania.

Można przywrócić lokalnie przypięty Udostępnij lub głośność jako tiered lub udziale warstwowych lub przypięte lokalnie głośność. 

Aby uzyskać więcej informacji na temat wielkości lokalnie przypięty przejdź do tematu [Używanie usługę Menedżer StorSimple Zarządzanie wielkości](storsimple-manage-volumes-u2.md).

### <a name="deduplication-and-compression-for-data-tiered-or-backed-up-to-the-cloud"></a>Deduplication i kompresja danych tiered lub kopii zapasowej w chmurze

StorSimple używa kompresji deduplication i dane, aby jeszcze bardziej zmniejszyć wymagania dotyczące magazynu w chmurze. Deduplication zmniejsza ogólnej ilości danych przechowywanych przez eliminowania nadmiarowości w zestawie danych przechowywanych. Zmian informacji StorSimple ignoruje danych bez zmian i rejestruje tylko zmiany. Ponadto StorSimple powoduje zmniejszenie ilości danych przechowywanych identyfikujący typ przepływu pracy i usuwając zduplikowane informacje. 

>[AZURE.NOTE] Dane przechowywane w wirtualnej macierzy nie jest deduplicated lub skompresowany. Wszystkie deduplication i kompresji występuje po prostu, przed wysłaniem danych do chmury.

### <a name="scheduled-and-on-demand-backups"></a>Zaplanowane i na żądanie kopii zapasowych

Funkcje ochrony danych StorSimple umożliwiają tworzenie kopii zapasowych na żądanie. Ponadto harmonogram wykonywania kopii zapasowych domyślnej gwarantuje, że dane kopii zapasowej codziennie. Kopie zapasowe są pobierane w formularzu migawki przyrostowe, które są przechowywane w chmurze. Migawki, które zarejestrować tylko zmiany od czasu ostatniej kopii zapasowej, można tworzyć i szybko przywrócić. Migawek może być bardzo ważne w scenariuszy odzyskiwania danych, ponieważ Zamień systemów magazyn pomocniczy (na przykład kopia zapasowa taśmą) i umożliwia przywracanie danych do centrum danych lub alternatywny witryn, w razie potrzeby.

## <a name="next-steps"></a>Następne kroki

Dowiedz się, jak [przygotować portal wirtualne tablicy](storsimple-ova-deploy1-portal-prep.md).


