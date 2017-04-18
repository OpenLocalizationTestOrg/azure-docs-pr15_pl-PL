<properties 
   pageTitle="Wersji 2.2 aktualizacji serii 8000 StorSimple | Microsoft Azure"
   description="W tym artykule opisano nowe funkcje, problemów i rozwiązań dla StorSimple 8000 serii aktualizacji 2.2."
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
   ms.date="07/18/2016"
   ms.author="alkohli" />

# <a name="storsimple-8000-series-update-22-release-notes"></a>Informacje o wersji 2.2 aktualizacji serii 8000 StorSimple  

## <a name="overview"></a>Omówienie

Następujące informacje o wersji opisano nowe funkcje oraz określenie krytyczne otwarte problemy StorSimple 8000 serii aktualizacji 2.2. Zawiera listę aktualizacji oprogramowania StorSimple zawartych w tej wersji. 

Aktualizacja 2.2 można stosować do dowolnego urządzenia StorSimple przecinającymi 2.1 aktualizacji wersji (GA) lub Aktualizuj 0,1. Wersja urządzenia skojarzone z 2.2 aktualizacji jest 6.3.9600.17708.

Przejrzyj informacje zawarte w informacjach o wersji przed wdrożeniem rozwiązania StorSimple aktualizacji.

>[AZURE.IMPORTANT]
> 
> - Aktualizacja 2.2 zawiera tylko aktualizacji oprogramowania. Trwa około 1,5-2 godzin, aby zainstalować tę aktualizację. 

> - Jeśli używasz 2.1 aktualizacji, zalecamy stosowanie aktualizacji 2.2 tak szybko, jak to możliwe.

> - Dostępność nowych wersji, mogą nie być widoczne aktualizacje natychmiast, ponieważ czynności fazowy wdrożenia aktualizacji. Poczekaj kilka dni, a następnie skanowanie w poszukiwaniu aktualizacji ponownie jako te będą dostępne wkrótce.


## <a name="whats-new-in-update-22"></a>Co nowego w 2.2 aktualizacji

Następujące kluczowe ulepszenia wprowadzono w 2.2 aktualizacji.

 
- **Optymalizacja odzyskiwanie miejsca automatycznego** — po usunięciu znacznie ustanawianie ilości danych, bloków nieużywanego miejsca do magazynowania konieczne można odzyskać. Ta wersja uległa proces odzyskiwanie miejsca z chmury, uzyskując nieużywana staje się dostępna szybciej w porównaniu z poprzedniej wersji.


- **Ulepszenia wydajności migawkę** — Aktualizacja 2.2 uległa czasu przetwarzania w chmurze migawki w niektórych scenariuszach w przypadku dużej ilości są używane i wymagane są minimalne do pochodząca nie danych. Scenariusz, który chcesz korzystać z tego ulepszania będzie wielkość archiwum.


- **Zbieranie pakiet wzmacnianie Obsługa** — zostały ulepszenia w ten sposób gromadzenia i przekazane w tej wersji pakietu pomocy technicznej. 


- **Aktualizowanie ulepszeń niezawodności** — ta wersja zawiera poprawki, które powodują większa niezawodność aktualizacji.

  
 

## <a name="issues-fixed-in-update-22"></a>Problemy rozwiązywane w 2.2 aktualizacji

Podsumowanie problemów, które zostały usunięte w 2.2 aktualizacje i 2.1 znajdują się w poniższych tabelach.    

| Brak | Funkcja                                    | Problem                                                                                                                                                                                                                                                                                        | Dotyczy urządzenia fizycznego | Dotyczy urządzenia wirtualnego |
|----|--------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------|---------------------------|
| 1  | Wydajność hosta                      | W starszej wersji występują problemy z wydajnością po stronie hosta zostały obserwowane podczas tworzenia wielkości lokalnie przypięte i podczas konwersji wielkości warstwowych na lokalnie przypięty głośność. Te problemy zostały rozwiązane w tej wersji powodując poprawa wydajności hosta podczas procedury tworzenia i konwersji głośność.                                                                        | Tak                        | Brak                        |
| 2  | Lokalnie przypięty wielkości                     | Sporadycznie w system może ulec awarii podczas tworzenia lokalnie przypięty głośność. Ten problem został rozwiązany w tej wersji.                                                                                                                                                               | Tak                        | Brak                        |
| 3  | Obsługi                                    | Wystąpiły awarie sporadycznie po metadanych dla urządzeń chmury StorSimple (8010 i 8020) tiered w chmurze. Ten problem został rozwiązany w tej wersji.                                                                                                                              | Brak                         | Tak                       |
| 4  | Tworzenia migawki                          | Wystąpiły problemy związane z tworzenie przyrostowe migawek w scenariuszach z dużą ilością i minimalna liczba do pochodząca nie danych. Te problemy zostały rozwiązane w tej wersji.                                                                                                                 | Tak                        | Tak                       |
| 5  | Uwierzytelnianie Openstack                   | Używając Openstack jako dostawca usług w chmurze, użytkownik może napotkać rzadko błędów związanych z uwierzytelnianie, miejsce, w którym parsera JSON spowodowało awarię. Ten problem został rozwiązany w tej wersji.                                                                                                                              | Tak                        | Brak                        |
| 6  | Kopiowanie strony hosta                             | We wcześniejszych wersjach programów rzadko błędów związanych z czasem ODX było widoczne podczas kopiowania danych z jednego woluminu do innego woluminu. Spowoduje to w trybie awaryjnym kontroler i system potencjalnie można przejść do trybu odzyskiwania. Ten problem został rozwiązany w tej wersji. | Tak                        | Brak       |
| 7  | Oprzyrządowania zarządzania Windows (WMI) | We wcześniejszych wersjach programów, zostały kilka wystąpień błąd serwera proxy sieci web, z wyjątkiem "<ManagementException> błąd ładowania dostawcy". Ten błąd został przypisany przeciek pamięci WMI i ustala się teraz.                                                               | Tak                        | Brak                        |
| 8  | Aktualizacja                                     | W niektórych przypadkach rzadkich, we wcześniejszych wersjach programów użytkownik otrzymano "CisPowershellHcsscripterror" podczas próby skanowania lub instalowanie aktualizacji. Ten problem został rozwiązany w tej wersji.                                                                                        | Tak                        | Tak                       |
| 9  | Pakiet pomocy technicznej                            | W tej wersji zostały ulepszenia sposobu zebrane i przekazać pakiet pomocy technicznej.                                                                                                                                                                                                      | Tak                        | Tak                                    |


## <a name="known-issues-in-update-22"></a>Znane problemy w 2.2 aktualizacji

Poniższa tabela zawiera podsumowanie znane problemy w tej wersji.

| Wartość nie. | Funkcja | Problem | Komentarze i rozwiązania | Dotyczy urządzenia fizycznego | Dotyczy urządzenia wirtualnego |
|-----|---------|-------|----------------------------|----------------------------|---------------------------|
| 1 | Dysk kworum | Sporadycznie w jeśli większość dysków w załącznik EBOD 8600 urządzenia są rozłączane, uzyskując kworum dysku, następnie puli miejsca do magazynowania przejdzie trybu offline. Pozostanie on niedostępny nawet wtedy, gdy nawiązywane dysków. | Konieczne będzie Uruchom ponownie urządzenie. Jeśli problem będzie nadal występował, skontaktuj się Support firmy Microsoft dla następnych kroków. | Tak | Brak |
| 2 | Identyfikator kontrolera niepoprawne | Gdy odbywa się wymiana kontrolerze, kontroler 0 może pojawić się jako kontrolerze 1. Podczas wymiany kontrolera podczas ładowania obrazu z węzła równorzędnych identyfikator kontrolera może się pojawić początkowo jako identyfikator kontrolera równorzędnych. Sporadycznie w to zachowanie mogą być widoczne po ponownym uruchomieniu systemu. | Jest wymagane żadne działanie użytkownika. Sytuacja rozwiąże sam po zakończeniu zastąpienie kontrolera. | Tak | Brak |
| 3 | Konta miejsca do magazynowania | Aby usunąć konto miejsca do magazynowania przy użyciu usługi miejsca do magazynowania jest nieobsługiwane scenariusz. Spowoduje to sytuacji, w której nie można pobrać danych użytkownika.|  | Tak | Tak |
| 4 | Urządzenie pracy awaryjnej | Wiele praca awaryjna kontenera głośność z tym samym urządzeniu źródła do różnych urządzeń nie jest obsługiwane. Awaryjne z jednego urządzenia braku na wielu urządzeniach spowoduje, że kontenery głośności w pierwszym nie powiodło się na urządzeniu utraty własności danych. Po przełączeniu tych kontenerów głośność są wyświetlane lub zachowywać się inaczej podczas wyświetlania w portalu klasyczny Azure. | | Tak | Brak |
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
| 15 | Azure poleceń cmdlet programu PowerShell i lokalnie przypięty wielkości | Nie można utworzyć lokalnie przypięty głośność za pomocą poleceń cmdlet środowiska PowerShell Azure. (Głośność, wszelkie utworzone za pomocą programu PowerShell Azure będzie tiered.) |Zawsze używaj usługę Menedżera StorSimple skonfigurować lokalnie przypięty wielkości.| Tak | Brak |
| 16 |Miejsca dla lokalnie przypięty wielkości | Jeśli usuniesz lokalnie przypięty głośność, miejsca dla nowych wielkości mogą nie zostać zaktualizowane natychmiast. Usługa Menedżer StorSimple aktualizuje lokalne miejsca około co godzinę.| Czekaj na godzinę przed próby utworzenia nowego woluminu. | Tak | Brak |
| 17 | Lokalnie przypięty wielkości | Zadanie przywracania udostępnia tymczasowe migawkę kopii zapasowej w katalogu kopii zapasowej, ale tylko na czas trwania zadania przywracania. Ponadto udostępnia grupy dysku wirtualnego z prefiksem **tmpCollection** na stronie **Zasad kopii zapasowej** , ale tylko na czas trwania zadania przywracania. | Ten problem może wystąpić, jeśli zadania przywracania zawiera tylko lokalnie przypięte wielkości lub kombinację wielkości lokalnie przypiętych i warstwowych. Jeśli zadanie przywracania zawiera tylko warstwowych wielkości, ten problem nie będzie obsługiwane. Wymagane jest nie udziału użytkownika. | Tak | Brak |
| 18 | Lokalnie przypięty wielkości | W przypadku anulowania zadania przywrócenia i trybie awaryjnym kontroler występuje natychmiast po zadaniu przywracania zostanie wyświetlona **Niepowodzenie** zamiast **Anulowane**. Jeśli zadanie przywracania kończy się niepowodzeniem i awaria kontroler wystąpiła natychmiast po zadanie przywracania zostanie wyświetlona **anulowany** zamiast **nie powiodło się**. | Ten problem może wystąpić, jeśli zadania przywracania zawiera tylko lokalnie przypięte wielkości lub kombinację wielkości lokalnie przypiętych i warstwowych. Jeśli zadanie przywracania zawiera tylko warstwowych wielkości, ten problem nie będzie obsługiwane. Wymagane jest nie udziału użytkownika. | Tak | Brak |
| 19 |Lokalnie przypięty wielkości | Jeśli anulowanie zadania przywracania lub przywracania kończy się niepowodzeniem, a następnie awaria kontroler wystąpiła, zadanie dodatkowe przywracania pojawi się na stronie **zadania** . | Ten problem może wystąpić, jeśli zadania przywracania zawiera tylko lokalnie przypięte wielkości lub kombinację wielkości lokalnie przypiętych i warstwowych. Jeśli zadanie przywracania zawiera tylko warstwowych wielkości, ten problem nie będzie obsługiwane. Wymagane jest nie udziału użytkownika. | Tak | Brak |
| 20 |Lokalnie przypięty wielkości | Jeśli spróbuj dokonać konwersji warstwowych zbiorcza (utworzone i sklonowany z 1.2 aktualizacji lub starszym) lokalnie przypięte i urządzenie jest zaczyna brakować miejsca lub jest awaria chmury, clone(s) może być uszkodzony.| Ten problem występuje tylko w przypadku ilości, które zostały utworzone i sklonowany z sprzed aktualizacji 2.1 oprogramowania. Należy to rzadko scenariusza.|
| 21 | Konwersja głośności | Nie są aktualizowane ACRs dołączone do woluminu podczas konwersji woluminu jest w toku (tiered do lokalnie przypięte lub odwrotnie). Aktualizowanie ACRs może spowodować uszkodzenie danych. | W razie potrzeby zaktualizuj ACRs przed konwersji woluminu i nie wprowadzaj dalszych aktualizacji awaryjnego w trakcie konwersji. |

## <a name="controller-and-firmware-updates-in-update-22"></a>Kontroler aktualizacje w 2.2 aktualizacji

Ta wersja aktualizacjami tylko do oprogramowania. Jednak aktualizowania z wersji przed aktualizacji nr 2, należy zainstalować sterownik Storport, Spaceport i (w niektórych przypadkach) dysku aktualizacje oprogramowania układowego na urządzeniu.
 
Aby uzyskać więcej informacji na temat instalacji sterownika, Storport, Spaceport i aktualizacji oprogramowania układowego dysku Zobacz [Instalowanie aktualizacji 2.2](storsimple-install-update-21.md) na urządzeniu StorSimple.

 
## <a name="virtual-device-updates-in-update-22"></a>Urządzenie wirtualne aktualizacji w 2.2 aktualizacji

Nie można zastosować aktualizację do wirtualnego urządzenia. Nowe urządzenia wirtualne będzie muszą zostać utworzone. 

## <a name="next-step"></a>Następny krok

Dowiedz się, jak [zainstalować aktualizację 2.2](storsimple-install-update-21.md) na urządzeniu StorSimple.
