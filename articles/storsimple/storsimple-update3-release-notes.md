<properties 
   pageTitle="Informacje o wersji StorSimple 8000 serii aktualizacji 3 | Microsoft Azure"
   description="W tym artykule opisano nowe funkcje, problemów i rozwiązań dla StorSimple 8000 serii aktualizacji 3."
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
   ms.date="10/14/2016"
   ms.author="alkohli" />

# <a name="storsimple-8000-series-update-3-release-notes"></a>Informacje o wersji StorSimple 8000 serii aktualizacji 3  

## <a name="overview"></a>Omówienie

Następujące informacje o wersji opisano nowe funkcje oraz określenie krytyczne otwarte problemy StorSimple 8000 serii aktualizacji 3. Zawiera listę aktualizacji oprogramowania StorSimple zawartych w tej wersji. 

Aktualizacja 3 można stosować do dowolnego urządzenia StorSimple przecinającymi 2.2 aktualizacji wersji (GA) lub Aktualizuj 0,1. Wersja urządzenia skojarzone z aktualizacji 3 jest 6.3.9600.17759.

Przejrzyj informacje zawarte w informacjach o wersji przed wdrożeniem rozwiązania StorSimple aktualizacji.

>[AZURE.IMPORTANT]
> 
> - Aktualizacja 3 jest oprogramowanie urządzenia, sterownik LSI sprzętowego i Storport i aktualizuje Spaceport. Trwa około 1,5-2 godzin, aby zainstalować tę aktualizację. 

> - Dostępność nowych wersji, mogą nie być widoczne aktualizacje natychmiast, ponieważ czynności fazowy wdrożenia aktualizacji. Poczekaj kilka dni, a następnie skanowanie w poszukiwaniu aktualizacji ponownie jako te będą dostępne wkrótce.


## <a name="whats-new-in-update-3"></a>Co nowego w aktualizacji 3

W aktualizacji 3 wprowadzono następujące udoskonalenia klucza i poprawki.

 
- Uruchom **automatycznego obszaru odzyskiwanie ulegnie zmianie** — Uruchamianie aktualizacji 3, algorytmów odzyskiwanie miejsca na kontrolerze wstrzymania systemu uzyskując szybsze wykonywanie. Aby uzyskać więcej informacji dotyczących portów, które są wymagane do pracy z odzyskania miejsca zapoznaj się z [StorSimple sieci wymagania](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).

- **Ulepszenia wydajności** — aktualizacji 3 ma zwiększona wydajność odczytu i zapisu w chmurze.

- **Ulepszenia związane z migracją** — w tej wersji, kilka poprawki i ulepszenia zostały przygotowane dla funkcji migracji z urządzeń serii 5000-7000 8000 urządzeń serii. Aby uzyskać więcej informacji na temat korzystania z funkcji migracji przejdź do [migracji z urządzenia serii 5000-7000 do urządzenia serii 8000](https://www.microsoft.com/en-us/download/details.aspx?id=47322). 

- **Monitorowania poprawek pokrewne** — w tej wersji błędy związane z monitorowania wykresów, pulpit nawigacyjny usługi i pulpit nawigacyjny urządzenia zostały ustalone.


## <a name="issues-fixed-in-update-3"></a>Problemy rozwiązywane w aktualizacji 3

Podsumowanie problemów, które zostały usunięte w aktualizacji 3 znajdują się w poniższych tabelach.    

| Brak | Funkcja                                    | Problem                                                                                                                                                                                                                                                                                        | Dotyczy urządzenia fizycznego | Dotyczy urządzenia wirtualnego |
|----|--------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------|---------------------------|
| 1  | Migracji danych po stronie hosta                      | W starszej wersji urządzenia chmury StorSimple została przejść do trybu offline podczas migracji danych po stronie hosta. Ten problem został rozwiązany w tej wersji.                                                | Brak                        | Tak                        |
| 2  | Lokalnie przypięty wielkości                     | W poprzedniej wersji wystąpiły problemy związane z błędy We/Wy, błędów konwersji głośności i błędy ścieżki danych do przedstawienia wielkości lokalnie przypięty. Tych problemów powodowanych głównego i stałe w tej wersji.                | Tak                        | Brak                       |
| 3  | Monitorowanie                                    | Wystąpiły wiele problemów związanych z jednostki raportowania i monitorowania oraz miejsce, w którym została wyświetlane niepoprawne informacje dotyczące wielkości lokalnie przypięty wykresy pulpitu nawigacyjnego urządzenia. Te problemy zostały rozwiązane w tej wersji. | Tak                         | Brak                       |
| 4  | Duże zapisy we/wy                          | Używając StorSimple dla obciążenia obejmujących dużą ilością zapisu, użytkownik będzie napotkasz rzadko błąd, miejsce, w którym zestaw roboczy został jest tiered w chmurze. Ten problem został rozwiązany w tej wersji. | Tak                        | Tak                       |
| 5  | Wykonywanie kopii zapasowych                                     | W niektórych przypadkach rzadkich, we wcześniejszych wersjach programów po użytkownik miał kopii zapasowej zdalnego klonowanie, czy wystąpiły błędy chmury i operacji czy błąd się. W tej wersji problem został rozwiązany, a operacja zakończy się pomyślnie.             | Tak                        | Tak                       |
| 6  | Zasady kopii zapasowej                                     | W niektórych przypadkach rzadkich w starszych wersjach oprogramowania, wystąpił błąd dotyczące usuwania zasad kopii zapasowej. Ten problem został rozwiązany w tej wersji.       | Tak                        | Tak                       |



## <a name="known-issues-in-update-3"></a>Znane problemy dotyczące aktualizacji 3

Poniższa tabela zawiera podsumowanie znane problemy w tej wersji.

| Wartość nie. | Funkcja | Problem | Komentarze i rozwiązania | Dotyczy urządzenia fizycznego | Dotyczy urządzenia wirtualnego |
|-----|---------|-------|----------------------------|----------------------------|---------------------------|
| 1 | Dysk kworum | Sporadycznie w jeśli większość dysków w załącznik EBOD 8600 urządzenia są rozłączane, uzyskując kworum dysku, następnie puli miejsca do magazynowania przejdzie trybu offline. Pozostanie on niedostępny nawet wtedy, gdy nawiązywane dysków. | Konieczne będzie Uruchom ponownie urządzenie. Jeśli problem będzie nadal występował, skontaktuj się Support firmy Microsoft dla następnych kroków. | Tak | Brak |
| 2 | Identyfikator kontrolera niepoprawne | Gdy odbywa się wymiana kontrolerze, kontroler 0 może pojawić się jako kontrolerze 1. Podczas zastąpienie kontrolerze podczas ładowania obrazu z węzła równorzędnych identyfikator kontrolera może się pojawić początkowo jako identyfikator kontrolera równorzędnych. Sporadycznie w to zachowanie mogą być widoczne po ponownym uruchomieniu systemu. | Jest wymagane żadne działanie użytkownika. Sytuacja rozwiąże sam po zakończeniu zastąpienie kontrolera. | Tak | Brak |
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
| 22 | Aktualizacje | Stosując aktualizacji 3, strony **konserwacji** w portalu klasyczny Azure spowoduje wyświetlenie następującego komunikatu związane z aktualizacji 2 — "seria 8000 StorSimple Aktualizacja 2 zawiera możliwość firmie Microsoft na pobieranie informacji dziennika aktywnie z urządzenia po wykrywamy potencjalne problemy". To jest mylące jak wskazuje, że urządzenie jest aktualizowany w celu aktualizacji 2. Gdy urządzenie jest succeesfully została zaktualizowana do wersji aktualizacji 3, ten komunikat zniknie. | Ten problem zostanie rozwiązany w przyszłej wersji. | Tak | Brak |


## <a name="controller-and-firmware-updates-in-update-3"></a>Kontroler aktualizacje w aktualizacji 3

Ta wersja obejmuje LSI aktualizacje sterownika i oprogramowania układowego. Aby uzyskać więcej informacji na temat instalacji sterownik LSI i aktualizacji oprogramowania układowego Zobacz [Instalowanie aktualizacji 3](storsimple-install-update-3.md) na urządzeniu StorSimple.

 
## <a name="virtual-device-updates-in-update-3"></a>Urządzenie wirtualne aktualizacji w aktualizacji 3

Nie można zastosować aktualizację na urządzeniu chmury StorSimple (nazywane także urządzenia wirtualnego). Nowe urządzenia wirtualne będzie muszą zostać utworzone. 


## <a name="next-step"></a>Następny krok

Dowiedz się, jak [zainstalować aktualizacji 3](storsimple-install-update-3.md) na urządzeniu StorSimple.
