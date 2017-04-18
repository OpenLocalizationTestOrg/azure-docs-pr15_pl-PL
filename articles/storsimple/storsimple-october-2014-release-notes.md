<properties 
    pageTitle="Informacje o wersji 0.1 aktualizacji 8000 StorSimple | Microsoft Azure"
    description="W tym artykule opisano nowe funkcje i poprawki, otwarte problemy i rozwiązania dostępne października 2014 r wersji Microsoft Azure StorSimple (aktualizacja z 0,1)."
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
    ms.date="09/21/2016"
    ms.author="alkohli" />

# <a name="storsimple-8000-series-update-01-release-notes--october-2014"></a>Informacje o wersji 0.1 aktualizacji serii 8000 StorSimple — października 2014 r.  

## <a name="overview"></a>Omówienie

Następujące informacje o wersji zidentyfikowanie krytycznych otwarte problemy aktualizacji serii StorSimple 8000 0,1 wydane w października 2014 r. Zawiera listę aktualizacji oprogramowania StorSimple zawartych w tej wersji. To jest pierwsza wersja po wersji StorSimple 8000 serii Zwolnij przypisano powszechnie dostępny w lipiec 2014 i odpowiada wersję oprogramowania 6.3.9600.17312.  

Zalecamy skanowanie w poszukiwaniu i zastosować dostępne aktualizacje, natychmiast po zainstalowaniu urządzenia. Można również włączyć automatyczne aktualizacje, aby pobrać i zainstalować aktualizacje o wysokim priorytecie od firmy Microsoft, zaraz po ich wydaniu. Aby uzyskać więcej informacji, zobacz jak [zaktualizować urządzenia StorSimple](storsimple-update-device.md).  

Przejrzyj informacje zawarte w informacjach o wersji przed wdrożeniem rozwiązania StorSimple aktualizacje.  

>[AZURE.IMPORTANT]
> 
-   Zainstaluj aktualizacje z października przy użyciu usługi Menedżera StorSimple, a nie programu Windows PowerShell dla StorSimple.  
-   Aktualizacje zwykle musisz wykonać około 3 godziny do wykonania.  
-   Wersji października StorSimple nie zawiera żadnych aktualizacji urządzenia wirtualnego StorSimple. Można stosować dostępne aktualizacje systemu Windows, łącznie z ostatnich poprawki zabezpieczeń, ale nie zobaczą zmiany w wersji dla urządzenia wirtualnego.  

Upewnij się, że są spełnione następujące wymagania wstępne, przed zaktualizowaniem urządzenia StorSimple.  

- Upewnij się, że oba kontrolery urządzeń są uruchomione przed aktualizacji. Jeśli którykolwiek kontrolerze nie jest uruchomiony, skanowania nie powiedzie się. Aby sprawdzić, czy kontrolery są w stanie prawidłowy, przejdź do **Stanu sprzętu** w obszarze strony **konserwacji** . W przypadku składników tej **Uwagi potrzebna**, skontaktuj się z Support firmy Microsoft, przed kontynuowaniem.  
- Zapewnić stały adresy IP dla obu kontrolera 0 i 1 kontroler routingu można połączyć się z Internetem, jak są one używane do obsługi aktualizacji do urządzenia. Polecenia [cmdlet połączenie testowe](https://technet.microsoft.com/library/hh849808.aspx) umożliwia ping znanego adresu spoza sieci, takich jak outlook.com, aby sprawdzić, czy kontroler ma połączenie ze sieci zewnętrznej.  
- Upewnij się, że porty wyjściowe wymagane są dostępne na urządzeniu StorSimple dla komunikacji wychodzącej. Aby uzyskać więcej informacji zobacz [wymagania dotyczące urządzenia StorSimple sieci](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).  
- Wersja oprogramowania urządzenie jest starsza niż 6.3.9600.17312 (aktualizacja z października 2014 r.), wyłączyć portów danych 2 i 3 dane, jeśli funkcja jest włączona, przed rozpoczęciem aktualizacji. Jeśli pole Data 2 lub 3 danych porty włączone, stosując aktualizację, może spowodować kontrolerze urządzenia przejść do trybu odzyskiwania. Zauważ, że po wyłączeniu interfejsów sieciowych skojarzone wielkość zostaną przeniesione do offline/Wy zostanie zerwane czas aktualizacji.  

## <a name="whats-new-in-the-october-release"></a>Co nowego w wersji października

Tej aktualizacji zawiera następujące ulepszenia:

- Za pomocą usługi Menedżera StorSimple interfejsu użytkownika można teraz zarządzać kontrolerach urządzenia. Zarządzanie, które obejmują akcje ponownie uruchomić, zamknij lub Włącz kontrolerze. Aby uzyskać więcej informacji przejdź do [zarządzania StorSimple kontrolery urządzeń](storsimple-manage-device-controller.md).  
- Można zaplanować alokacji przepustowości sieci WAN zgodnie z kombinacji dzień tygodnia i godzina dnia. Pozwala na lepsze wykorzystanie przepustowości WAN czasie mniejszego. Szablony różnych przepustowości są dozwolone dla kontenerów różnych głośność. Aby uzyskać więcej informacji przejdź do [zarządzania szablonów przepustowości StorSimple](storsimple-manage-bandwidth-templates.md).  
- Możesz skonfigurować powiadomienia e-mail wyprzedzeniem powiadomić administratorom i innych problemów z istniejących lub niekiedy nadchodzące. Aby uzyskać więcej informacji przejdź do [ustawień alertów Konfiguruj](storsimple-manage-alerts.md#configure-alert-settings).  

## <a name="issues-fixed-in-the-october-release"></a>Problemy rozwiązywane w wersji października


Poniższa tabela zawiera podsumowanie problemów, które zostały poprawione w tej aktualizacji.  

| Wartość nie. | Funkcja | Problem | Dotyczy urządzenia fizycznego | Dotyczy urządzenia wirtualnego |
|-----|---------|-------|---------------------------------|--------------------------------|
| 1 | Interfejsy | W poprzedniej wersji interfejsów dane 2 i 3 dane zostały miejscami w oprogramowaniu. Ten problem został poprawiony w tej aktualizacji. Wyczyść ustawienia i wyłączyć te interfejsy sieciowe przed rozpoczęciem instalacji aktualizacji. Po zainstalowaniu aktualizacji, konieczne będzie skonfigurowanie tych interfejsów. | Tak | Brak |
| 2 | Pakiet pomocy technicznej | W poprzedniej wersji, jeśli po uruchomieniu polecenia cmdlet programu Windows PowerShell **HcsSupportPackage Eksportuj** do pobierania dzienniki Baseboard Współpracuje (IPMI), Niepowodzenie z następującym ostrzeżeniem: "Operacja zakończyła się pomyślnie na tym kontrolerze, ale nie na kontrolerze równorzędnych z powodu następujących błędów. Sprawdź, czy funkcja jest uszkodzony i czy bieżący węzeł można nawiązać równorzędnych." Teraz ten problem zostanie rozwiązany. | Tak | Brak |
| 3 | Urządzenie pracy awaryjnej | W poprzedniej wersji było pomysłem niespójności danych, zadanie **wykrywania kopii zapasowej** nie powiodło się w trybie awaryjnym urządzenia. Teraz ten problem zostanie rozwiązany. | Tak | Brak |
| 4 | Urządzenie pracy awaryjnej | W poprzedniej wersji, po przełączeniu urządzenia kopie zapasowe były widoczne, ale kontenerze skojarzone głośności nie znajdował się na tym urządzeniu docelowej. Teraz ten problem zostanie rozwiązany. | Tak | Brak |
| 5 | Urządzenie pracy awaryjnej | W poprzedniej wersji wystąpił błąd w wyliczenie kopii zapasowych w chmurze podczas operacji przywracania rejestru, który może prowadzić do niespójności danych jeśli wystąpiły problemy z łącznością chmury. | Tak | Brak |
| 6 | Aktualizacji oprogramowania układowego | W poprzedniej wersji — zadanie aktualizacji oprogramowania układowego urządzenia nie powiodło się i wyświetlany komunikat o błędzie, który stwierdzono, że cmdlet nie rozpoznać i że aktualizacji nie powiodła się w wyniku. Kontroler następnie polega w trybie odzyskiwania. Teraz ten problem zostanie rozwiązany. | Tak | Brak |
| 7 | Instalacji | Błędy spowodowane przez urządzenia nie są obrazami poprawnie podczas instalacji teraz zostały naprawione. | Tak | Brak |
| 8 | Fabryczne | Teraz można opcjonalnie pominąć sprawdzanie sprzętowego fabryczne. Jest to zmiana pochodzące z poprzedniej wersji. | Tak | Brak |
| 9 | Fabryczne | W poprzedniej wersji podczas uruchomienia polecenia cmdlet Resetowanie factory, testy wersję oprogramowania układowego zostały wprowadzone tylko w przypadku niektórych składników sprzętu. Testy dodatkowe sprzętowego zostały wprowadzone po ponownym w procesie, co może spowodować zresetowanie kończy się niepowodzeniem. Ta poprawka zapewnia, że wszystkie testy sprzętowego zostały wprowadzone po fabryki Resetowanie polecenia cmdlet uruchomieniu i przed pierwszym systemu uruchom ponownie. | Tak | Brak |
| 10 | Obrót klucza konta miejsca do magazynowania | Polecenia cmdlet **Wywołaj HcsmServiceDataEncryptionKeyChange** umożliwia teraz obrócić klawiszy konta miejsca do magazynowania jest wyświetlany monit o wprowadzenie klucza szyfrowania danych usługi. Jest to zmiana pochodzące z poprzedniej wersji, w której klucza szyfrowania danych usługi przekazano jako parametr w tekście. | Tak | Brak |
| 11 | Awarii w ciągu 24 godzin | Podczas odzyskiwania systemu oczyszczania na tym urządzeniu źródła nie wystąpić awarii bezpośrednio, powodujące niepowodzenie. Ten problem usunięto w tej wersji. | Tak | Brak |

## <a name="known-issues-in-the-october-release"></a>Znane problemy w wydaniu października

Poniższa tabela zawiera podsumowanie znane problemy w tej wersji.

| Wartość nie. | Funkcja | Problem | Komentarze i rozwiązania | Dotyczy urządzenia fizycznego | Dotyczy urządzenia wirtualnego |
|-----|---------|-------|----------------------------|----------------------------|---------------------------|
| 1 | Fabryczne | W niektórych przypadkach podczas wykonywania zresetowaniu factory urządzenia StorSimple mogą zostać zatrzymane i wyświetlić ten komunikat: **Przywróć factory jest w toku (faza 8)**. Dzieje się tak, jeśli w trakcie polecenia cmdlet możesz nacisnąć klawisze CTRL + C. | Po zainicjowaniu Resetowanie factory nie naciśnij klawisze CTRL + C. Jeśli jesteś już w tym stanie, skontaktuj się Support firmy Microsoft dla następnych kroków. | Tak | Brak |
| 2 | Fabryczne | Czy nie Resetuj factory z urządzenia StorSimple, które są aktualizowane na podstawie GA do października 2014 Zwolnij. | Operacja działa tylko, jeśli zainstalowano poprawkę. Skontaktuj się Microsoft Support uzyskać tę poprawkę wymagane. | Tak | Brak | 
| 3 | Dysk kworum | Sporadycznie w większości dysków w załącznik EBOD 8600 urządzenia są rozłączane, uzyskując ma kworum dysku puli miejsca do magazynowania zostaną trybu offline. Pozostanie on niedostępny nawet wtedy, gdy nawiązywane dysków. | Konieczne będzie Uruchom ponownie urządzenie. Jeśli problem będzie nadal występował, skontaktuj się Support firmy Microsoft dla następnych kroków. | Tak | Brak |
| 4 | Błędy migawkę chmury | Sporadycznie w migawkę chmury może zakończyć się niepowodzeniem z powodu błędu **osiągnięto limit kopii zapasowej maksimum**. Dzieje się tak, jeśli przekraczać 255 klony online na tym samym urządzeniu, z tym samym woluminu oryginalnego, który został usunięty. | | Tak | Tak |
| 5 | Identyfikator kontrolera niepoprawne | Gdy odbywa się wymiana kontrolerze, kontroler 0 może pojawić się jako kontrolerze 1. Podczas zastąpienie kontrolerze podczas ładowania obrazu z węzła równorzędnych identyfikator kontrolera może się pojawić początkowo jako identyfikator kontrolera równorzędnych. Sporadycznie w to zachowanie mogą być widoczne po ponownym uruchomieniu systemu. |Jest wymagane żadne działanie użytkownika. Sytuacja rozwiąże sam po zakończeniu zastąpienie kontrolera. | Tak | Brak |
| 6 | Urządzenie monitorowania wykresów | W usłudze Menedżer StorSimple wykresami monitorowania urządzenie nie działa, gdy podstawowe lub uwierzytelnianie NTLM jest włączona w konfiguracji serwera proxy dla urządzenia. | Modyfikowanie konfiguracji serwera proxy sieci web dla urządzenia zarejestrowane z usługą Menedżera StorSimple, więc uwierzytelniania wybrano opcję Brak. W tym celu należy uruchomić programu Windows PowerShell dla polecenia cmdlet StorSimple Set-HcsWebProxy. | Tak | Tak |
| 7 | Konta miejsca do magazynowania | Aby usunąć konto miejsca do magazynowania przy użyciu usługi miejsca do magazynowania jest nieobsługiwane scenariusz. Spowoduje to sytuacji, w której nie można pobrać danych użytkownika. | | Tak | Tak |
| 8 | Urządzenie pracy awaryjnej | Wiele praca awaryjna kontenera głośność z tym samym urządzeniu źródła do różnych urządzeń nie jest obsługiwane. | Awaryjne z jednego urządzenia braku na wielu urządzeniach spowoduje, że kontenery głośności w pierwszym nie powiodło się na urządzeniu utraty własności danych. Po przełączeniu tych kontenerów głośność są wyświetlane lub zachowywać się inaczej podczas wyświetlania w portalu klasyczny Azure. | Tak | Brak |
| 9 | Instalacji | Podczas StorSimple karta dla instalacji programu SharePoint musisz podać IP urządzenia, aby Zainstaluj, aby pomyślnie ukończyć.    | | Tak | Brak |
| 10 | Serwer proxy sieci Web | Jeśli w konfiguracji serwera proxy w sieci web jest protokół HTTPS jako określony protokół, wpłynie na usługi urządzeń komunikacji i urządzenia będą przejść do trybu offline. Obsługa pakietów generowanych jest w procesie przez inne istotne zasobów na urządzeniu. | Upewnij się, że adres URL serwera proxy sieci web ma HTTP jako określony protokół. Więcej informacji na temat [konfiguracji serwera proxy sieci web dla danego urządzenia](storsimple-configure-web-proxy.md). | Tak | Brak |
| 11 | Serwer proxy sieci Web | Jeśli skonfigurować i włączyć serwer proxy sieci web na urządzeniu zarejestrowanych, będzie konieczne ponowne active kontrolera na urządzeniu. | | Tak | Brak |
| 12 | Opóźnienie chmury wysoki i wysoka obciążenie We/Wy | Gdy urządzenia StorSimple wykryje kombinację bardzo wysoki chmury opóźnienia (kolejność sekund) i wysokiego obciążenia We/Wy, wielkość urządzenia przejdź do stanu ograniczone i operacji dotyczących może zakończyć się niepowodzeniem z powodu błędu "urządzenie nie jest gotowe". | Musisz ręcznie ponownie uruchomić kontrolery urządzeń lub wykonywanie awarię urządzenie na odzyskiwanie z takiej sytuacji. | Tak | Brak |

## <a name="physical-device-updates-in-the-october-release"></a>Zwolnij urządzenia fizycznego aktualizacje z października

Gdy te aktualizacje są stosowane do urządzenia fizycznego, wersja oprogramowania zostanie zmieniona na 6.3.9600.17312. Inaczej, te informacje o wersji dotyczą wszystkich modeli urządzenia StorSimple. Aby uzyskać więcej informacji na temat tych aktualizacji zobacz [oprogramowania urządzenia fizycznie październik 2014 aktualizacja dla programu Microsoft Azure StorSimple urządzenia](http://support.microsoft.com/kb/2986997).  

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-the-october-release"></a>Dołączone kolejną kontroler SCSI (SA) i aktualizacji oprogramowania układowego października Zwolnij

Ta wersja aktualizacji sterownika i oprogramowania układowego na kontrolerze skojarzeń zabezpieczeń fizycznie urządzenia. Aby uzyskać więcej informacji na temat kontrolera skojarzeń zabezpieczeń aktualizacji, zobacz [październik 2014 aktualizowanie kontrolerów LSI skojarzeń zabezpieczeń w programie Microsoft Azure StorSimple urządzenia](http://support.microsoft.com/kb/2987020).   

Ta wersja dotyczy również aktualizację skumulowana sprzętowego, którego dotyczy problemy ze zgodnością ze składnikami sprzętu urządzenia. Aby uzyskać więcej informacji na temat aktualizacji oprogramowania sprzętowego zobacz [Aktualizacja dla programu Microsoft Azure StorSimple urządzenia sprzętowego października 2014 r](http://support.microsoft.com/kb/2987015).  

## <a name="virtual-device-updates-in-the-october-release"></a>Zwolnij urządzenia wirtualnego aktualizacje z października

Ta wersja nie zawiera wszystkie aktualizacje dla urządzeń wirtualnych. Zastosowanie tej aktualizacji nie ulegnie zmianie wersję oprogramowania urządzenia wirtualnego.
 
