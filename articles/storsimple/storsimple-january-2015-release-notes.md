<properties 
   pageTitle="Aktualizacja 8000 StorSimple 0,2 wersji | Microsoft Azure"
   description="W tym artykule opisano nowe funkcje i poprawki, otwarte problemy i rozwiązania dostępne dla stycznia 2015 r wersji Microsoft Azure StorSimple (aktualizacja 0,2)."
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
   ms.date="05/16/2016"
   ms.author="v-sharos" />


# <a name="storsimple-8000-series-update-02-release-notes---january-2015"></a>Informacje o wersji 0,2 aktualizacji serii 8000 StorSimple - stycznia 2015 r.

## <a name="overview"></a>Omówienie

Następujące informacje o wersji zidentyfikowanie krytycznych otwarte problemy dla publikacji Microsoft Azure StorSimple stycznia 2015 r. Zawiera listę aktualizacji oprogramowania StorSimple zawartych w tej wersji. To wersji drugiej po wersji StorSimple 8000 serii Zwolnij powszechnie dostępny w lipca 2014 r.
  
Tej aktualizacji nie zmienia wersji oprogramowania urządzenia fizycznego z października aktualizacji. Nadal można wersji 6.3.9600.17312. Obraz używany przez obraz virtual urządzenia zmienił się w tej wersji. W związku z tym wszystkie nowe urządzenia wirtualne utworzonych po 2015-1-20 zostanie wyświetlona w wersji co 6.3.9600.17361.  

Przejrzyj następujące informacje zawarte w wersji aktualizacji stycznia 2015 r.

> [AZURE.IMPORTANT]  
>
>- Tej aktualizacji nie jest dostępne za pośrednictwem usługi Windows Update i nie można zainstalować tak jak inne aktualizacje. Urządzenie nie będą odbierane tę aktualizację, nawet jeśli aktualizacje zostały zastosowane przy użyciu portalu klasyczny Azure. Ta aktualizacja ma zastosowanie tylko do urządzenia wirtualne utworzone po 20 stycznia 2015 r. 
> 
>- Wersji stycznia StorSimple nie zawiera żadnych aktualizacji urządzenia fizycznego StorSimple. Można stosować dostępne aktualizacje dla systemu Windows do wirtualnego urządzenia, w tym ostatnie poprawki zabezpieczeń, ale nie zobaczą zmiany w wersji dla urządzenia fizycznego StorSimple.

## <a name="whats-new-in-the-january-release"></a>Co nowego w wersji stycznia

Ta aktualizacja zawiera poprawki związane z wielkość przejść do trybu offline na tym urządzeniu wirtualną. (Zobacz [problemy rozwiązywane w tym wydaniu](#issues-fixed-in-the-january-release)).  

Aktualizacja nie zawiera nowe funkcje lub funkcji.  

## <a name="issues-fixed-in-the-january-release"></a>Problemy rozwiązywane w wersji stycznia

W poniższej tabeli opisano problem został rozwiązany w tej aktualizacji.

|Wartość nie.|Funkcja|Problem|Dotyczy urządzenia fizycznego|Dotyczy urządzenia wirtualnego 
|---|-------|-----|--------------------------|-------------------------
|1|Wielkości przejść do trybu offline|Gdy opóźnienia wysoka chmury utrzymują kilka minut, wielkość urządzenia wirtualnego StorSimple przejść do trybu offline na hostów. Ta poprawka zwiększa próg chmury opóźnienia w związku z tym zminimalizowaniu sytuacje powodujące wielkość przejść do trybu offline na hosts.|Brak|Tak  

## <a name="known-issues-in-the-january-release"></a>Znane problemy w wydaniu stycznia

Poniższa tabela zawiera podsumowanie znane problemy w tej wersji.
 
|Wartość nie.|Funkcja|Problem|Komentarze i rozwiązania|Dotyczy urządzenia fizycznego|Dotyczy urządzenia wirtualnego 
|---|-------|-----|-------------------|--------------------------|-------------------------
|1| Fabryczne|  W niektórych przypadkach podczas wykonywania zresetowaniu factory urządzenia StorSimple mogą zostać zatrzymane i wyświetlić ten komunikat: **Przywróć factory jest w toku (faza 8).** Dzieje się tak, jeśli w trakcie polecenia cmdlet możesz nacisnąć klawisze CTRL + C.| Po zainicjowaniu Resetowanie factory nie naciśnij klawisze CTRL + C. Jeśli jesteś już w tym stanie, skontaktuj się Support firmy Microsoft dla następnych kroków.|Tak|Brak|
|2|Dysk kworum| Sporadycznie w większości dysków w załącznik EBOD 8600 urządzenia są rozłączane, uzyskując ma kworum dysku puli miejsca do magazynowania zostaną trybu offline. Pozostanie on niedostępny nawet wtedy, gdy nawiązywane dysków.|Konieczne będzie Uruchom ponownie urządzenie. Jeśli problem będzie nadal występował, skontaktuj się Support firmy Microsoft dla następnych kroków.|Tak |Brak
|3|Błędy migawkę chmury|Sporadycznie w migawkę chmury może zakończyć się niepowodzeniem z powodu błędu **osiągnięto limit kopii zapasowej maksimum**. Dzieje się tak, jeśli przekraczać 255 klony online na tym samym urządzeniu, z tym samym woluminu oryginalnego, który został usunięty.||Tak|Tak
|4|Identyfikator kontrolera niepoprawne|Gdy odbywa się wymiana kontrolerze, kontroler 0 może pojawić się jako kontrolerze 1. Podczas zastąpienie kontrolerze podczas ładowania obrazu z węzła równorzędnych identyfikator kontrolera może się pojawić początkowo jako identyfikator kontrolera równorzędnych.  Sporadycznie w to zachowanie mogą być widoczne po ponownym uruchomieniu systemu.|Jest wymagane żadne działanie użytkownika. Sytuacja rozwiąże sam po zakończeniu zastąpienie kontrolera.|Tak|Brak 
|5| Urządzenie monitorowania wykresów|W usłudze Menedżer StorSimple wykresami monitorowania urządzenie nie działa, gdy podstawowe lub uwierzytelnianie NTLM jest włączona w konfiguracji serwera proxy dla urządzenia.|Modyfikowanie konfiguracji serwera proxy sieci web dla urządzenia zarejestrowane z usługą Menedżera StorSimple, więc uwierzytelniania wybrano opcję Brak. W tym celu należy uruchomić programu Windows PowerShell dla polecenia cmdlet StorSimple Set-HcsWebProxy.|Tak|Tak
|6| Konta miejsca do magazynowania|Aby usunąć konto miejsca do magazynowania przy użyciu usługi miejsca do magazynowania jest nieobsługiwane scenariusz. Spowoduje to sytuacji, w której nie można pobrać danych użytkownika.|| Tak |  Tak
|7|Urządzenie pracy awaryjnej| Wiele praca awaryjna kontenera głośność z tym samym urządzeniu źródła do różnych urządzeń nie jest obsługiwane.| Awaryjne z jednego urządzenia braku na wielu urządzeniach spowoduje, że kontenery głośności w pierwszym nie powiodło się na urządzeniu utraty własności danych. Po przełączeniu tych kontenerów głośność są wyświetlane lub zachowywać się inaczej podczas wyświetlania w portalu klasyczny Azure.|Tak|Brak
|8| Instalacji|Podczas StorSimple karta dla instalacji programu SharePoint musisz podać IP urządzenia, aby Zainstaluj, aby pomyślnie ukończyć.||Tak|Brak
|9| Serwer proxy sieci Web|Jeśli w konfiguracji serwera proxy w sieci web jest protokół HTTPS jako określony protokół, wpłynie na usługi urządzeń komunikacji i urządzenia będą przejść do trybu offline. Obsługa pakietów generowanych jest w procesie przez inne istotne zasobów na urządzeniu.|Upewnij się, że adres URL serwera proxy sieci web ma HTTP jako określony protokół. Uzyskaj więcej informacji na temat [konfiguracji serwera proxy sieci web dla danego urządzenia](storsimple-configure-web-proxy.md).|Tak |Brak
|10|Serwer proxy sieci Web|  Jeśli skonfigurować i włączyć serwer proxy sieci web na urządzeniu zarejestrowanych, będzie konieczne ponowne active kontrolera na urządzeniu.|| Tak |Brak
|11|Opóźnienie chmury wysoki i wysoka obciążenie We/Wy|Gdy urządzenia StorSimple wykryje kombinację bardzo wysoki chmury opóźnienia (kolejność sekund) i wysokiego obciążenia We/Wy, wielkość urządzenia przejdź do stanu ograniczone i operacji dotyczących może zakończyć się niepowodzeniem z powodu błędu "urządzenie nie jest gotowe".|Musisz ręcznie ponownie uruchomić kontrolery urządzeń lub wykonywanie awarię urządzenie na odzyskiwanie z takiej sytuacji.|Tak|Brak

## <a name="physical-device-updates-in-the-january-release"></a>Zwolnij aktualizacji urządzenia fizycznego w styczniu

Tej aktualizacji nie zawiera inne zmiany na urządzeniu StorSimple.

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-the-january-release"></a>Dołączone kolejną kontroler SCSI (SA) i aktualizacji oprogramowania układowego w styczniu Zwolnij

Ta wersja nie zawiera żadnych aktualizacji kontrolera SCSI (SA) dołączone kolejną lub oprogramowania układowego. Aktualizacja sterownika jest października 2014 wersji. 

## <a name="virtual-device-updates-in-the-january-release"></a>Zwolnij aktualizacji urządzenia wirtualnego w styczniu

Ta wersja zawiera zaktualizowaną wirtualnego urządzenia. Urządzenia wirtualne utworzone po 20 stycznia 2015 r zostanie wyświetlona wersja oprogramowania jako 6.3.9600.17361.

 
