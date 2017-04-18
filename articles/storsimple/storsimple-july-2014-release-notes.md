<properties 
   pageTitle="Zwolnij 8000 StorSimple wersji wersji | Microsoft Azure"
   description="W tym artykule opisano nowe funkcje, otwarte problemy i rozwiązania dostępna dla lipiec 2014 wersji Microsoft Azure StorSimple."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
 <tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="04/18/2016"
   ms.author="v-sharos" />

# <a name="storsimple-8000-series-release-version-release-notes---july-2014"></a>Informacje o wersji StorSimple 8000 serii Zwolnij wersji – lipiec 2014 

## <a name="overview"></a>Omówienie

Informacje o wersji Obserwuj identyfikowanie krytyczne otwarte problemy dla serii 8000 StorSimple lipiec 2014 ogólnodostępną (GA) wersji pakietu Microsoft Azure StorSimple. Ta wersja odpowiada wersję oprogramowania 6.3.9600.17215.  

Inaczej, te informacje o wersji dotyczą wszystkich modeli urządzenia StorSimple. Informacje o wersji są stale aktualizowane; jak zostaną wykryte problemy krytyczne wymagające obejść ten problem, są one dodawane. Przed wdrożeniem tego rozwiązania Microsoft Azure StorSimple, należy rozważyć następujące informacje.  

## <a name="known-issues-in-this-release"></a>Znane problemy w tej wersji
Poniższa tabela zawiera podsumowanie znane problemy w tej wersji.  
 
| Wartość nie. | Funkcja | Problem | Komentarze i rozwiązania | Dotyczy urządzenia fizycznego | Dotyczy urządzenia wirtualnego |
|-----|---------|-------|----------------------------|----------------------------|---------------------------|
| 1 | Fabryczne | W niektórych przypadkach podczas wykonywania zresetowaniu factory urządzenia StorSimple mogą zostać zatrzymane i wyświetlić ten komunikat: **Przywróć factory jest w toku (faza 8)**. Dzieje się tak, jeśli w trakcie polecenia cmdlet możesz nacisnąć klawisze CTRL + C. | Po zainicjowaniu Resetowanie factory nie naciśnij klawisze CTRL + C. Jeśli jesteś już w tym stanie, skontaktuj się Support firmy Microsoft dla następnych kroków. | Tak | Brak |
| 2 | Dysk kworum | Sporadycznie w większości dysków w załącznik EBOD 8600 urządzenia są rozłączane, uzyskując ma kworum dysku puli miejsca do magazynowania zostaną trybu offline. Pozostanie on niedostępny nawet wtedy, gdy nawiązywane dysków. | Konieczne będzie Uruchom ponownie urządzenie. Jeśli problem będzie nadal występował, skontaktuj się Support firmy Microsoft dla następnych kroków. | Tak | Brak |
| 3 | Błędy migawkę chmury | Sporadycznie w migawkę chmury może zakończyć się niepowodzeniem z powodu błędu **osiągnięto limit kopii zapasowej maksimum**. Dzieje się tak, jeśli przekraczać 255 klony online na tym samym urządzeniu, z tym samym woluminu oryginalnego, który został usunięty. | | Tak | Tak |
| 4 | Identyfikator kontrolera niepoprawne | Gdy odbywa się wymiana kontrolerze, kontroler 0 może pojawić się jako kontrolerze 1. Podczas wymiany kontrolera podczas ładowania obrazu z węzła równorzędnych identyfikator kontrolera może się pojawić początkowo jako identyfikator kontrolera równorzędnych. Sporadycznie w to zachowanie mogą być widoczne po ponownym uruchomieniu systemu. | Jest wymagane żadne działanie użytkownika. Sytuacja rozwiąże się po zakończeniu wymiany kontrolera. | Tak | Brak |
| 5 | Urządzenie monitorowania wykresów | W usłudze Menedżer StorSimple wykresami monitorowania urządzenie nie działa, gdy podstawowe lub uwierzytelnianie NTLM jest włączona w konfiguracji serwera proxy dla urządzenia. | Modyfikowanie konfiguracji serwera proxy sieci web dla urządzenia zarejestrowane z usługą Menedżera StorSimple, więc uwierzytelniania wybrano opcję Brak. W tym celu należy uruchomić programu Windows PowerShell dla polecenia cmdlet StorSimple Set-HcsWebProxy. | Tak | Tak |
| 6 | Konta miejsca do magazynowania | Aby usunąć konto miejsca do magazynowania przy użyciu usługi miejsca do magazynowania jest nieobsługiwane scenariusz. Spowoduje to sytuacji, w której nie można pobrać danych użytkownika. | | Tak | Tak |
| 7 | Awarii | Awarii w ciągu 24 godzin awarii (DR) nie jest obsługiwane. | | Tak | Brak |
| 8 | Urządzenie pracy awaryjnej | Wiele praca awaryjna kontenera głośność z tym samym urządzeniu źródła do różnych urządzeń nie jest obsługiwane. Awaryjne z jednego urządzenia braku na wielu urządzeniach spowoduje, że kontenery głośności w pierwszym nie powiodło się na urządzeniu utraty własności danych. Po przełączeniu tych kontenerów głośność są wyświetlane lub zachowywać się inaczej podczas wyświetlania w portalu klasyczny Azure. | | Tak | Brak |
| 9 | Instalacji | Podczas StorSimple karta dla instalacji programu SharePoint należy podać IP urządzenia, aby pomyślnie ukończyć. | | Tak | Brak |
| 10 | Interfejsy | Interfejsy danych 2 i 3 dane zostały miejscami w oprogramowaniu. | Skontaktuj się z Support firmy Microsoft, jeśli musisz skonfigurować tych interfejsów. | Tak | Brak |


 
