<properties 
   pageTitle="Informacje o wersji StorSimple 8000 serii aktualizacji 2 | Microsoft Azure"
   description="W tym artykule opisano nowe funkcje, problemów i rozwiązań dla StorSimple 8000 serii aktualizacji 2."
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
   ms.date="05/24/2016"
   ms.author="v-sharos" />

# <a name="storsimple-8000-series-update-2-release-notes"></a>Informacje o wersji StorSimple 8000 serii aktualizacji 2  

## <a name="overview"></a>Omówienie

Następujące informacje o wersji opisano nowe funkcje oraz określenie krytyczne otwarte problemy StorSimple 8000 serii aktualizacji 2. Zawiera listę oprogramowania StorSimple, sterowników i aktualizacji oprogramowania układowego dysku zawartych w tej wersji. 

Aktualizacja 2 można stosować do dowolnego urządzenia StorSimple przecinającymi 1.2 aktualizacji wersji (GA) lub Aktualizuj 0,1. Wersja urządzenia skojarzone z aktualizacji 2 jest 6.3.9600.17673.

Przejrzyj informacje zawarte w informacjach o wersji przed wdrożeniem rozwiązania StorSimple aktualizacji.

>[AZURE.IMPORTANT]
> 
- Trwa około 4-7 godzin, aby zainstalować aktualizację (w tym aktualizacji systemu Windows). 
- Aktualizacja 2 zawiera aktualizacje oprogramowania układowego SSD, LSI sterownika i oprogramowania.
- Dostępność nowych wersji, mogą nie być widoczne aktualizacje natychmiast, ponieważ czynności fazowy wdrożenia aktualizacji. Poczekaj kilka dni, a następnie skanowanie w poszukiwaniu aktualizacji ponownie jako te będą dostępne wkrótce.


## <a name="whats-new-in-update-2"></a>Co nowego w pakiecie 2

Aktualizacja 2 wprowadzono następujące nowe funkcje.

- **Lokalnie przypięte wielkości** — w poprzednich wersjach programu serii StorSimple 8000 bloki danych były tiered w chmurze, na podstawie zużycia. Wystąpił żaden sposób, aby mieć pewność, że bloki czy pozostają w lokalnej. W pakiecie 2 podczas tworzenia woluminu, można określić woluminu jako lokalnie przypięte i podstawowe dane z tego woluminu nie będzie można tiered w chmurze. Migawki wielkości lokalnie przypięty nadal zostaną skopiowane do chmury dla kopii zapasowej, tak, aby chmury można używać na potrzeby odzyskiwania danych mobilności i awarii. Ponadto można zmienić typu woluminu (oznacza to, że Konwertuj tiered wielkości do wielkości lokalnie przypięte i tiered wielkości Konwertuj lokalnie przypięta do). 

- **Ulepszenia urządzenia wirtualnego StorSimple** — wcześniej serii StorSimple 8000 umieszczony urządzenia wirtualnego jako rozwiązanie odzyskiwania lub rozwoju/Testowanie awarii. Wystąpił tylko jeden model urządzenia wirtualnego (model 1100). Aktualizacja 2 wprowadza dwóch modeli urządzenia wirtualnego: 

     - 8010 (dawniej nazywanych 1100) — bez zmian; ma pojemność 30 TB i używa Azure standardowego magazynu.
     - 8020 — ma pojemność 64 TB, korzysta z magazynu Azure Premium zwiększyć wydajność.

    Ma jednego wirtualnego dysku twardego dla obu modeli urządzenia wirtualnego (8010-8020). Przy pierwszym uruchomieniu urządzenia wirtualnego, wykryje parametrów platformy i dotyczy wersji prawidłowy model.

- **Ulepszenia sieci** — aktualizacji 2 zawiera następujące ulepszenia sieci:

     - Kart mogą mieć dostęp do chmury, tak aby przejęcie awaryjne może wystąpić, jeśli NIC nie powiedzie się.
     - Usprawnione routingu, stałej metryki chmury włączone bloków.
     - Ponowienie online nie powiodło się zasobów przed trybie awaryjnym.
     - Nowe alertów dla awarie usług.

- **Aktualizowanie ulepszenia** — aktualizacja 1.2 lub starszym serii StorSimple 8000 został zaktualizowany przy użyciu dwóch kanałów: w usłudze Windows Update klaster, iSCSI i tak dalej i Microsoft Update plików binarnych i oprogramowania układowego.
    Aktualizacja 2 używa Microsoft Update dla wszystkich pakietów aktualizacji. Powinna ona prowadzić do mniej czasu poprawki lub wykonywanie pracy awaryjnej. 

- **Aktualizacje oprogramowania układowego** — następujące aktualizacje oprogramowania układowego są uwzględniane:
    - LSI: lsi_sas2.sys wersję produktu 2.00.72.10
    - Tylko SSD (nie aktualizuje dysk twardy): XMGG, XGEG KZ50, F6C2 i VR08

- **Obsługa aktywne** — aktualizacji 2 umożliwia firmy Microsoft uwzględniał dodatkowych informacji diagnostycznych z urządzenia. Gdy nasz zespół operacje identyfikuje urządzenia, które występują problemy, możemy mają większe możliwości zbieranie informacji z urządzenia i sprawdzanie pod kątem błędów. **Akceptowanie aktualizacji 2 umożliwia nam do obsługi aktywne**.    
 

## <a name="issues-fixed-in-update-2"></a>Problemy rozwiązywane w pakiecie 2

Poniższe tabele zawiera podsumowanie problemów, które zostały poprawione w 2 aktualizacje.    

| Wartość nie. | Funkcja | Problem | Dotyczy urządzenia fizycznego | Dotyczy urządzenia wirtualnego |
|-----|---------|-------|--------------------------------|--------------------------------|
| 1 | Interfejsy | Po uaktualnieniu do aktualizacji 1 usługa Menedżera StorSimple zgłoszone, że porty Data2 i Data3 nie powiodło się na jednym kontrolerze. Ten problem został rozwiązany. | Tak | Brak |
| 2 | Aktualizacje | Po uaktualnieniu do aktualizacji 1 alerty alarmu wystąpił w portalu klasyczny Azure na wielu urządzeniach. Ten problem został rozwiązany. | Tak | Brak |
| 3 | Uwierzytelnianie Openstack | Podczas używania Openstack jako dostawca usług w chmurze, może wystąpić błąd, że chmury ciągu uwierzytelnianie zakończyło się zbyt długo. Ten problem usunięto. | Tak | Brak |


## <a name="known-issues-in-update-2"></a>Znane problemy w pakiecie 2

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
| 20 |Lokalnie przypięty wielkości | Jeśli spróbuj dokonać konwersji warstwowych zbiorcza (utworzone i sklonowany z 1.2 aktualizacji lub starszym) lokalnie przypięte i urządzenie jest zaczyna brakować miejsca lub jest awaria chmury, clone(s) może być uszkodzony.| Ten problem występuje tylko w przypadku ilości, które zostały utworzone i sklonowany z sprzed aktualizacji 2 oprogramowania. Należy to rzadko scenariusza.|
| 21 | Konwersja głośności | Nie są aktualizowane ACRs dołączone do woluminu podczas konwersji woluminu jest w toku (tiered do lokalnie przypięte lub odwrotnie). Aktualizowanie ACRs może spowodować uszkodzenie danych. | W razie potrzeby zaktualizuj ACRs przed konwersji woluminu i nie wprowadzaj dalszych aktualizacji awaryjnego w trakcie konwersji. |

## <a name="controller-and-firmware-updates-in-update-2"></a>Kontroler aktualizacje w pakiecie 2

Ta wersja aktualizacji sterownik i sprzętowego dysku na urządzeniu.
 
- Aby uzyskać więcej informacji na temat sprzętowego LSI aktualizacji, zobacz artykuł z bazy wiedzy Microsoft Knowledge base 3121900. 
- Aby uzyskać więcej informacji na temat sprzętowego dysku aktualizacji, zobacz artykuł z bazy wiedzy Microsoft Knowledge base 3121899.
 
## <a name="virtual-device-updates-in-update-2"></a>Aktualizacje urządzenia wirtualnego w pakiecie 2

Nie można zastosować aktualizację do wirtualnego urządzenia. Nowe urządzenia wirtualne będzie muszą zostać utworzone. 

## <a name="next-step"></a>Następny krok

Dowiedz się, jak [zainstalować aktualizacji 2](storsimple-install-update-2.md) na urządzeniu StorSimple.
