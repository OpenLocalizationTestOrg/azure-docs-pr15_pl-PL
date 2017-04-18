<properties 
   pageTitle="Informacje o wersji 1.2 aktualizacji serii 8000 StorSimple | Microsoft Azure"
   description="W tym artykule opisano nowe funkcje, problemów i rozwiązań dla 1.2 aktualizacji serii 8000 StorSimple."
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
   ms.date="08/18/2016"
   ms.author="alkohli" />

# <a name="storsimple-8000-series-update-12-release-notes"></a>Informacje o wersji 1.2 aktualizacji serii 8000 StorSimple  

## <a name="overview"></a>Omówienie

Następujące informacje o wersji opisano nowe funkcje oraz określenie krytyczne otwarte problemy 1.2 aktualizacji serii 8000 StorSimple. Zawiera listę oprogramowania StorSimple, sterownik i aktualizacji oprogramowania układowego dysku zawartych w tej wersji. 

Aktualizacja 1.2 można stosować do dowolnego urządzenia StorSimple systemem wersji (GA), aktualizacja 0,1, aktualizacja 0,2 lub Aktualizuj 0,3 oprogramowanie. Aktualizacja 1.2 nie jest dostępna, gdy Twoje urządzenie działa aktualizacji 1 lub Aktualizuj 1.1. Jeśli urządzenie działa wersji (GA), zapoznaj się z [skontaktuj się z pomocą techniczną firmy Microsoft](storsimple-contact-microsoft-support.md) , aby pomóc w zainstalowaniu tej aktualizacji.

W poniższej tabeli wymieniono odpowiadającą aktualizacji 1, 1.1 i 1.2 wersje oprogramowania urządzenia.

| Jeśli aktualizacja... | to jest wersja oprogramowania do urządzenia. |
|---------------------|---------------------------------------|
| Aktualizowanie 1.2          | 6.3.9600.17584                        |
| Aktualizowanie 1.1          | 6.3.9600.17521                        |
| Aktualizowanie 1.0          | 6.3.9600.17491                        |

Przejrzyj informacje zawarte w informacjach o wersji przed wdrożeniem rozwiązania StorSimple aktualizacji. Aby uzyskać więcej informacji, zobacz sposób [instalowania aktualizacji 1.2 na urządzeniu StorSimple](storsimple-install-update-1.md). 

>[AZURE.IMPORTANT]
> 
- Trwa około 5-10 godzin, aby zainstalować aktualizację (łącznie z aktualizacji systemu Windows). 
- Aktualizacja 1.2 zawiera oprogramowanie, sterownik LSI i aktualizacji oprogramowania układowego dysku. Aby zainstalować, postępuj zgodnie z instrukcjami [instalacji aktualizacji 1.2 na urządzeniu StorSimple](storsimple-install-update-1.md).
- Dostępność nowych wersji, mogą nie być widoczne aktualizacje natychmiast, ponieważ czynności fazowy wdrożenia aktualizacji. Skanowanie w poszukiwaniu aktualizacji w ciągu kilku dni ponownie one staną się dostępne wkrótce.


## <a name="whats-new-in-update-12"></a>Co nowego w 1.2 aktualizacji

Te funkcje najpierw zostały wydane z aktualizacji 1, która została udostępniona ograniczony zestaw użytkowników. W wersji 1.2 aktualizacji większość użytkowników StorSimple zobaczy następujące nowe funkcje i ulepszenia:

- **Migracja z serii 5000 7000 do 8000 urządzeń serii** — tej wersji wprowadzono nową funkcję migracji umożliwiający StorSimple 5000 7000 serii urządzenia migracji ich danych do urządzenia fizycznie serii StorSimple 8000 lub wirtualnych urządzenia. Funkcja migracji występują dwie wartości klucza oferty:                                                                  

    - **Zapewnianie ciągłości**, umożliwiając migracji istniejących danych na urządzeniach serii 5000 7000 do 8000 serii urządzeń.
    - **Ulepszone funkcji oferowanych przez urządzenia serii 8000**, takich jak scentralizowane zarządzanie wydajną obsługę wielu urządzeń za pośrednictwem usługi Menedżera StorSimple lepiej klasę urządzeń i aktualizacji oprogramowania układowego, virtual urządzenia przenoszenia danych i funkcji w przyszłych przewodnik.

    Znajduje się [Przewodnik po migracji](http://www.microsoft.com/download/details.aspx?id=47322) , aby uzyskać szczegółowe informacje dotyczące metod migrowania serii 5000 7000 StorSimple z urządzeniem serii 8000. 

- **Dostępność w portalu dla instytucji rządowych Azure** — StorSimple jest teraz dostępny w portal Azure dla instytucji rządowych. Zobacz jak [wdrożyć urządzenie StorSimple Portal Azure dla instytucji rządowych](storsimple-deployment-walkthrough-gov.md).

- **Obsługa innych dostawców usług w chmurze** — innych chmury dostawców usług obsługiwane są Amazon S3, S3 Amazon z Rekordów, HP i OpenStack (beta).

- **Aktualizacja do najnowszej API magazynowania** — w tej wersji StorSimple została zaktualizowana do najnowszej usługi Magazyn Azure interfejsów API. Urządzeń serii StorSimple 8000 wersji oprogramowania sprzed aktualizacji 1 (wersji, 0,1, 0,2 i 0,3) wersji Azure przestrzeni dyskowej usługi interfejsów API starszych niż 17 lipca 2009 roku. Podane w zaktualizowanych [powiadomienia o usunięcie wersji usługi przechowywania](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/10/19/microsoft-azure-storage-service-version-removal-update-extension-to-2016.aspx)przez 1 sierpnia 2016 te interfejsy API zostanie zastąpiona. Konieczne jest stosowanie aktualizacji 1 serii 8000 StorSimple przed 1 sierpnia 2016. Jeśli nie możesz to zrobić, urządzeń StorSimple przestanie działać prawidłowo.

- **Pomocy technicznej dla strefy zbędne przestrzeni dyskowej (ZRS)** — uaktualnienie do najnowszej wersji interfejsów API miejsca do magazynowania, serii StorSimple 8000 będzie obsługiwać strefy zbędne przestrzeni dyskowej (ZRS) poza lokalnie zbędne przestrzeni dyskowej (LRS) i zbędne Geo przestrzeni dyskowej (GRS). Zapoznaj się z tego [artykułu na temat opcji nadmiarowości magazyn Azure](../storage/storage-redundancy.md) ZRS szczegóły.

- **Rozszerzony początkowej środowiska rozmieszczania i aktualizacji** — w tej wersji, instalacja i procesy aktualizacji zostały rozszerzone. Aby przekazać opinię do użytkownika, jeśli konfiguracja sieci i ustawienia zapory są niepoprawne zwiększona instalacji za pomocą Kreatora konfiguracji. Dodatkowe polecenia cmdlet diagnostyczne udzielono Ci ułatwiające rozwiązywanie problemów z siecią urządzenia. Zobacz [Rozwiązywanie problemów z artykułu wdrożenia](storsimple-troubleshoot-deployment.md) uzyskać więcej informacji o nowe diagnostyczne polecenia cmdlet używana do rozwiązywania problemów.

## <a name="issues-fixed-in-update-12"></a>Problemy rozwiązywane w 1.2 aktualizacji

Poniższa tabela zawiera podsumowanie problemów, które zostały usunięte w aktualizacji 1.2, 1.1 i 1.    


| Wartość nie. | Funkcja | Problem | Stałe w aktualizacji | Dotyczy urządzenia fizycznego | Dotyczy urządzenia wirtualnego |
|-----|---------|-------|-----------------|---------------------------------|--------------------------------|
| 1 | Windows PowerShell dla StorSimple | Gdy użytkownik zdalny dostęp do urządzenia StorSimple przy użyciu programu Windows PowerShell dla StorSimple, a następnie uruchomić Kreatora konfiguracji, jak najwcześniej IP został wprowadzania danych 0 wystąpił awarii. Ten problem został rozwiązany teraz Update 1. | Aktualizacja 1 | Tak | Tak |
| 2 | Fabryczne | W niektórych przypadkach podczas przeprowadzania zresetowaniu factory StorSimple urządzenie zostały zatrzymane i wyświetlany ten komunikat: **Przywróć factory jest w toku (faza 8)**. To się stało, naciśnięcie klawiszy CTRL + C podczas polecenia cmdlet jest w toku. Ten problem został rozwiązany teraz.| Aktualizacja 1 | Tak | Brak |
| 3 | Fabryczne | Po zresetowaniu factory uszkodzony kontroler podwójne, mieli możliwość kontynuować Rejestracja urządzenia. Wynikiem konfiguracji nieobsługiwany system. Update 1 jest wyświetlany komunikat o błędzie i rejestracji jest zablokowany na urządzeniu, że nie powiodło się factory zresetowała. | Aktualizacja 1 | Tak | Brak |
| 4 | Fabryczne | W niektórych przypadkach podniesiona zostały alerty niezgodności dodatnią wartość FAŁSZ. Niepoprawne niezgodności alertów generowanych jest już na urządzeniach z aktualizacji 1. | Aktualizacja 1 | Tak | Brak |
| 5 | Fabryczne | Jeśli Resetowanie factory zostało przerwane przed ukończeniem, urządzenie wprowadzone trybie odzyskiwania i nie jest możliwe uzyskiwać dostęp do programu Windows PowerShell dla StorSimple. Ten problem został rozwiązany teraz. | Aktualizacja 1 | Tak | Brak |
| 6 | Odzyskiwanie | Błąd odzyskiwania (DR) danych został rozwiązany, którym DR może zakończyć się niepowodzeniem podczas odnajdowania kopii zapasowych na tym urządzeniu docelowej. | Aktualizacja 1 | Tak | Tak |
| 7 | Monitorowanie LED | W niektórych przypadkach monitorowania LED z tyłu urządzenia nie wskazywała prawidłowy stan. Niebieski LED został wyłączony. DANE 0 i LED 1 dane zostały błyskające, nawet, gdy nie zostały skonfigurowane tych interfejsów. Problem został rozwiązany, a LED monitorowanie teraz oznaczania poprawne stanu.  | Aktualizacja 1 | Tak | Brak |
| 8 | Monitorowanie LED | W niektórych przypadkach po zastosowaniu aktualizacji 1, niebieski uproszczonej na kontrolerze active wyłączone umożliwiając trudno Zidentyfikuj kontroler aktywna. Ten problem został rozwiązany w tej wersji poprawki.| Aktualizowanie 1.2 | Tak | Brak |
| 9 | Interfejsy | We wcześniejszych wersjach urządzeniu StorSimple skonfigurowany z bramą obsługi routingu można przejść do trybu offline. W tej wersji ulepszono metrykę routingu dla danych 0 wprowadzone najniższą; w związku z tym nawet jeśli inne interfejsy są włączone w chmurze, cały ruch chmury z urządzenia będą kierowane za pośrednictwem danych 0. | Aktualizacja 1 | Tak | Tak | 
| 10 | Wykonywanie kopii zapasowych | Błąd Update 1, które spowodowało kopie zapasowe kończy się niepowodzeniem po 24 dni został usunięty w wersji 1.1 aktualizacji poprawki. | Aktualizowanie 1.1 | Tak | Tak |
| 11 | Wykonywanie kopii zapasowych | Błąd w poprzednich wersjach spowodowało niskiej wydajności na potrzeby migawek chmury z niskim zmian stawek. Ten problem został rozwiązany w tej wersji poprawki.| Aktualizowanie 1.2 | Tak | Tak |
| 12 | Aktualizacje | Błąd Update 1, zgłoszone nie powiodło się uaktualnienie i uszkodzenie kontrolery, aby przejść do trybu odzyskiwania został usunięty w tej wersji poprawki.| Aktualizowanie 1.2 | Tak | Tak |


## <a name="known-issues-in-update-12"></a>Znane problemy dotyczące programu 1.2 aktualizacji

Poniższa tabela zawiera podsumowanie znane problemy w tej wersji.

| Wartość nie. | Funkcja | Problem | Komentarze i rozwiązania | Dotyczy urządzenia fizycznego | Dotyczy urządzenia wirtualnego |
|-----|---------|-------|----------------------------|----------------------------|---------------------------|
| 1 | Dysk kworum | Sporadycznie w większości dysków w załącznik EBOD 8600 urządzenia są rozłączane, uzyskując ma kworum dysku puli miejsca do magazynowania zostaną trybu offline. Pozostanie on niedostępny nawet wtedy, gdy nawiązywane dysków. | Konieczne będzie Uruchom ponownie urządzenie. Jeśli problem będzie nadal występował, skontaktuj się Support firmy Microsoft dla następnych kroków. | Tak | Brak |
| 2 | Identyfikator kontrolera niepoprawne | Gdy odbywa się wymiana kontrolerze, kontroler 0 może pojawić się jako kontrolerze 1. Podczas zastąpienie kontrolerze podczas ładowania obrazu z węzła równorzędnych identyfikator kontrolera może się pojawić początkowo jako identyfikator kontrolera równorzędnych. Sporadycznie w to zachowanie mogą być widoczne po ponownym uruchomieniu systemu. | Jest wymagane żadne działanie użytkownika. Sytuacja rozwiąże sam po zakończeniu zastąpienie kontrolera. | Tak | Brak |
| 3 | Konta miejsca do magazynowania | Aby usunąć konto miejsca do magazynowania przy użyciu usługi miejsca do magazynowania jest nieobsługiwane scenariusz. Spowoduje to sytuacji, w której nie można pobrać danych użytkownika. | Tak | Tak |
| 4 | Urządzenie pracy awaryjnej | Wiele praca awaryjna kontenera głośność z tym samym urządzeniu źródła do różnych urządzeń nie jest obsługiwane. Awaryjne urządzenia z jednego urządzenia braku na wielu urządzeniach spowoduje, że kontenery głośności w pierwszym nie powiodło się na urządzeniu utraty własności danych. Po przełączeniu tych kontenerów głośność są wyświetlane lub zachowywać się inaczej podczas wyświetlania w portalu klasyczny Azure. | | Tak | Brak |
| 5 | Instalacji | Podczas StorSimple karta dla instalacji programu SharePoint musisz podać IP urządzenia, aby Zainstaluj, aby pomyślnie ukończyć.    | | Tak | Brak |
| 6 | Serwer proxy sieci Web | Jeśli w konfiguracji serwera proxy w sieci web jest protokół HTTPS jako określony protokół, wpłynie na usługi urządzeń komunikacji i urządzenia będą przejść do trybu offline. Obsługa pakietów generowanych jest w procesie przez inne istotne zasobów na urządzeniu. | Upewnij się, że adres URL serwera proxy sieci web ma HTTP jako określony protokół. Aby uzyskać więcej informacji przejdź do [konfiguracji serwera proxy sieci web dla danego urządzenia](storsimple-configure-web-proxy.md). | Tak | Brak |
| 7 | Serwer proxy sieci Web | Jeśli skonfigurować i włączyć serwer proxy sieci web na urządzeniu zarejestrowanych, będzie konieczne ponowne active kontrolera na urządzeniu. | | Tak | Brak |
| 8 | Opóźnienie chmury wysoki i wysoka obciążenie We/Wy | Gdy urządzenia StorSimple wykryje kombinację bardzo wysoki chmury opóźnienia (kolejność sekund) i wysokiego obciążenia We/Wy, wielkość urządzenia przejdź do stanu ograniczone i operacji dotyczących może zakończyć się niepowodzeniem z powodu błędu "urządzenie nie jest gotowe". | Musisz ręcznie ponownie uruchomić kontrolery urządzeń lub wykonywanie awarię urządzenie na odzyskiwanie z takiej sytuacji. | Tak | Brak |
| 9 | Azure programu PowerShell | Kiedy używać polecenia cmdlet Get-AzureStorSimpleStorageAccountCredential **StorSimple & #124; Select-Object - Czekaj najpierw 1 -** aby zaznaczyć pierwszy obiekt tak, aby utworzyć nowy obiekt **VolumeContainer** , polecenia cmdlet zwraca wszystkie obiekty. | Zawijanie polecenia cmdlet w nawiasach w następujący sposób: **(Get-Azure-StorSimpleStorageAccountCredential) & #124; Select-Object - Czekaj najpierw 1 -** | Tak | Tak |
| 10| Migracji | Gdy wielu kontenerów głośność są przekazywane do migracji, EAT dla najnowszej kopii zapasowej są dokładne tylko dla pierwszego kontenera głośność. Ponadto równoległa migracja rozpocznie się po 4 kopii zapasowych w pierwszym kontenerze głośność są migrowane. | Zaleca się migrowanie jeden kontener głośność naraz. | Tak | Brak |
| 11| Migracji | Po przywróceniu ilości nie są dodawane do zasad kopii zapasowej lub grupy dysku wirtualnego. | Należy dodać te ilości do zasady tworzenia kopii zapasowych w celu tworzenia kopii zapasowych. | Tak | Tak |
| 12| Migracji | Po zakończeniu migracji urządzenia serii 5000-7000 nie dostęp do kontenerów migrowane dane. | Zaleca się po migracji zakończeniu i Projekt zatwierdzony — możesz usunąć kontenerów migrowane dane. | Tak | Brak |
| 13| Klonowanie i DR | Urządzenie StorSimple uruchomiony aktualizacji 1 nie klonowanie lub wykonać odzyskiwanie do urządzenia oprogramowanie 1 sprzed aktualizacji. | Należy zaktualizować urządzenia do aktualizacji 1, aby umożliwić te operacje | Tak | Tak |
| 14 | Migracji | Kopia zapasowa konfiguracji migracji może się nie powieść na urządzeniu serii 5000 7000 po głośność grup zawierających nie skojarzone wielkości. | Usuwanie wszystkich grup pustego głośność z nie skojarzone wielkości, a następnie ponów próbę kopia zapasowa konfiguracji.| Tak | Brak |

## <a name="physical-device-updates-in-update-12"></a>Urządzenia fizycznego aktualizacji w 1.2 aktualizacji

Aktualizowanie poprawki 1.2 jest stosowana do urządzenia fizycznego (wersji przed Update 1), wersja oprogramowania zmieni się na 6.3.9600.17584.

## <a name="controller-and-firmware-updates-in-update-12"></a>Kontroler aktualizacje w 1.2 aktualizacji

Ta wersja aktualizacji sterownik i sprzętowego dysku na urządzeniu.
 
- Aby uzyskać więcej informacji na temat aktualizacji sterownika skojarzenia zabezpieczeń zobacz [aktualizacji 1 dla kontrolery LSI skojarzeń zabezpieczeń w programie Microsoft Azure StorSimple urządzenia](https://support.microsoft.com/kb/3043005). 

- Aby uzyskać więcej informacji na temat sprzętowego dysku aktualizacji, zobacz [sprzętowego dysku aktualizacji 1 dla programu Microsoft Azure StorSimple urządzenia](https://support.microsoft.com/kb/3063416).
 
## <a name="virtual-device-updates-in-update-12"></a>Urządzenie wirtualne aktualizacji w 1.2 aktualizacji

Nie można zastosować aktualizację do wirtualnego urządzenia. Nowe urządzenia wirtualne będzie muszą zostać utworzone. 

## <a name="next-steps"></a>Następne kroki

- [Instalowanie aktualizacji 1.2 na urządzeniu](storsimple-install-update-1.md).
 
