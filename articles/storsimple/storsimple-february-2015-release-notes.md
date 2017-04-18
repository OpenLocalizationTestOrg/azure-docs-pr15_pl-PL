<properties 
   pageTitle="Aktualizacja 8000 StorSimple 0,3 wersji | Microsoft Azure"
   description="W tym artykule opisano nowe funkcje i poprawki, otwarte problemy i rozwiązania dostępne dla luty 2015 wersji Microsoft Azure StorSimple (aktualizacja z 0,3)."
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

# <a name="storsimple-8000-series-update-03-release-notes---february-2015"></a>Informacje o wersji 0,3 aktualizacji serii 8000 StorSimple - luty 2015

## <a name="overview"></a>Omówienie

Następujące informacje o wersji zidentyfikowanie krytycznych otwarte problemy aktualizacji serii StorSimple 8000 0,3 wydane w luty 2015. Zawiera listę aktualizacji oprogramowania StorSimple zawartych w tej wersji. To jest trzecie wydanie po wersji StorSimple 8000 serii Zwolnij powszechnie dostępny w lipiec 2014.
  
Tej aktualizacji nie zmienia wersji oprogramowania urządzenia z aktualizacji stycznia. Nadal można wersji 6.3.9600.17312. Aby potwierdzić, że zainstalowano aktualizację, zaznaczając pole wyboru data **Ostatniej aktualizacji** . Jeśli data jest 2015-2-10 lub nowszy, a następnie po zainstalowaniu aktualizacji pomyślnie.  

Zalecamy skanowanie w poszukiwaniu i zastosować dostępne aktualizacje, bezpośrednio po zainstalowaniu urządzenia StorSimple. Można także włączyć Aktualizacje automatyczne pobieranie i instalowanie aktualizacji o wysokim priorytecie od firmy Microsoft, zaraz po ich wydaniu. Aby uzyskać więcej informacji zobacz [Aktualizowanie urządzenia StorSimple](storsimple-update-device.md).  

Przejrzyj informacje zawarte w informacjach o wersji przed wdrożeniem rozwiązania StorSimple aktualizacji.  

>[AZURE.IMPORTANT]   
>
> - Zainstaluj aktualizację lutego przy użyciu usługi Menedżera StorSimple i nie środowiska Windows PowerShell dla StorSimple.   
> - Trwa około godziny, aby zainstalować tę aktualizację. Jednak jeśli instalujesz aktualizacje zbiorcze może potrwać około 3 godziny do wykonania.  
> - Wersji lutego StorSimple nie zawiera żadnych aktualizacji urządzenia wirtualnego StorSimple. Można stosować dostępne aktualizacje dla systemu Windows do wirtualnego urządzenia, w tym ostatnie poprawki zabezpieczeń, ale nie zobaczą zmiany w wersji dla urządzenia wirtualnego.  

Upewnij się, że przed zaktualizowaniem urządzenia StorSimple są spełnione następujące wymagania wstępne.  

- Upewnij się, że oba kontrolery urządzeń są uruchomione przed aktualizacji. Jeśli którykolwiek kontrolerze nie jest uruchomiony, skanowania nie powiedzie się. Aby sprawdzić, czy kontrolery są w stanie prawidłowy, przejdź do **Stanu sprzętu** w obszarze strony **konserwacji** . W przypadku składników tej **Uwagi potrzebna**, skontaktuj się z Support firmy Microsoft, przed kontynuowaniem.
- Zapewnić stały adresy IP zarówno kontroler 0 i 1 kontroler routingu można połączyć się z Internetem, jak są one używane do obsługi aktualizacji do urządzenia. Polecenia [cmdlet połączenie testowe](https://technet.microsoft.com/library/hh849808.aspx) umożliwia ping znanego adresu spoza sieci, takich jak outlook.com, aby sprawdzić, czy kontroler ma połączenie ze sieci zewnętrznej.
- Upewnij się, że porty 80 i 443 są dostępne na urządzeniu StorSimple dla komunikacji wychodzącej. Aby uzyskać więcej informacji zobacz [wymagania dotyczące urządzenia StorSimple sieci](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).
- Wersja oprogramowania urządzenie jest starsza niż 6.3.9600.17312 (aktualizacja z października 2014 r.), wyłączyć portów danych 2 i 3 dane, jeśli funkcja jest włączona, przed rozpoczęciem aktualizacji. Pozostawiając danych 2 lub 3 danych porty włączona po zainstalowaniu aktualizacji może spowodować kontrolerze urządzenia przejść do trybu odzyskiwania. Zauważ, że po wyłączeniu interfejsów sieciowych skojarzone wielkość zostaną przeniesione do offline operacji dotyczących zostanie zerwane czas aktualizacji.  
  
## <a name="whats-new-in-the-february-release"></a>Co nowego w wersji lutego

Ta aktualizacja zawiera poprawki fabryki resetowania problem, który wystąpił na dla urządzeń, które miały została uaktualniona z GA Zwolnij do wersji października 2014 r. Aby uzyskać więcej informacji zobacz [problemy rozwiązywane w tym wydaniu](#issues-fixed-in-the-february-release).   

Tej aktualizacji nie zawiera nowe funkcje lub funkcji.  

## <a name="issues-fixed-in-the-february-release"></a>Problemy rozwiązywane w wersji lutego

W poniższej tabeli opisano problem został rozwiązany w tej aktualizacji.  
 
| Wartość nie. | Funkcja | Problem | Dotyczy urządzenia fizycznego | Dotyczy urządzenia wirtualnego |
|-----|---------|-------|---------------------------------|-------------------------------|
| 1 | Fabryczne | Spróbuj wykonać fabryki Resetowanie na urządzeniu, z którą pierwotnie miał zainstalowanej wersji GA (wersja 6.3.9600.17215), ale została zaktualizowana do października wersji (wersja 6.3.9600.17312). Resetowanie fabryki kończy się niepowodzeniem i urządzenie staje się nietrwałe. | Tak | Brak |


## <a name="known-issues-in-the-february-release"></a>Znane problemy w wydaniu lutego

Poniższa tabela zawiera podsumowanie znane problemy w tej wersji.
 
| Wartość nie. | Funkcja | Problem | Komentarze i rozwiązania | Dotyczy urządzenia fizycznego  | Dotyczy urządzenia wirtualnego |
|-----|---------|-------|----------------------------|-----------------------------|--------------------------|
| 1 | Fabryczne | W niektórych przypadkach podczas wykonywania zresetowaniu factory urządzenia StorSimple mogą zostać zatrzymane i wyświetlić ten komunikat: **Przywróć factory jest w toku (faza 8)**. Dzieje się tak, jeśli w trakcie polecenia cmdlet możesz nacisnąć klawisze CTRL + C. | Po zainicjowaniu Resetowanie factory nie naciśnij klawisze CTRL + C. Jeśli jesteś już w tym stanie, skontaktuj się Support firmy Microsoft dla następnych kroków. | Tak | Brak |
| 2 | Dysk kworum | Sporadycznie w większości dysków w zabezpieczenie EBOD 8600device jest odłączony, uzyskując ma kworum dysku puli miejsca do magazynowania zostaną trybu offline. Pozostanie on niedostępny nawet wtedy, gdy nawiązywane dysków. | Konieczne będzie Uruchom ponownie urządzenie. Jeśli problem będzie nadal występował, skontaktuj się Support firmy Microsoft dla następnych kroków. | Tak | Brak |
| 3 | Błędy migawkę chmury | Sporadycznie w migawkę chmury może zakończyć się niepowodzeniem z powodu błędu **osiągnięto limit kopii zapasowej maksimum**. Dzieje się tak, jeśli przekraczać 255 klony online na tym samym urządzeniu, z tym samym woluminu oryginalnego, który został usunięty. |  | Tak | Tak |
| 4 | Identyfikator kontrolera niepoprawne | Gdy odbywa się wymiana kontrolerze, kontroler 0 może pojawić się jako kontrolerze 1. Podczas zastąpienie kontrolerze podczas ładowania obrazu z węzła równorzędnych identyfikator kontrolera może się pojawić początkowo jako identyfikator kontrolera równorzędnych. Sporadycznie w to zachowanie mogą być widoczne po ponownym uruchomieniu systemu. | Jest wymagane żadne działanie użytkownika. Sytuacja rozwiąże sam po zakończeniu zastąpienie kontrolera. | Tak | Brak |
| 5 | Urządzenie monitorowania wykresów | W usłudze Menedżer StorSimple wykresami monitorowania urządzenie nie działa, gdy podstawowe lub uwierzytelnianie NTLM jest włączona w konfiguracji serwera proxy dla urządzenia. | Modyfikowanie konfiguracji serwera proxy sieci web dla urządzenia zarejestrowane z usługą Menedżera StorSimple, więc uwierzytelniania wybrano opcję Brak. W tym celu należy uruchomić programu Windows PowerShell dla polecenia cmdlet StorSimple Set-HcsWebProxy. | Tak | Tak |
| 6 | Konta miejsca do magazynowania | Aby usunąć konto miejsca do magazynowania przy użyciu usługi miejsca do magazynowania jest nieobsługiwane scenariusz. Spowoduje to sytuacji, w której nie można pobrać danych użytkownika. |  | Tak | Tak |
| 7 | Urządzenie pracy awaryjnej | Wiele praca awaryjna kontenera głośność z tym samym urządzeniu źródła do różnych urządzeń nie jest obsługiwane.  Awaryjne z jednego urządzenia braku na wielu urządzeniach spowoduje, że kontenery głośności w pierwszym nie powiodło się na urządzeniu utraty własności danych. Po przełączeniu tych kontenerów głośność są wyświetlane lub zachowywać się inaczej podczas wyświetlania w portalu klasyczny Azure. |   | Tak | Brak |
| 8 | Instalacji | Podczas StorSimple karta dla instalacji programu SharePoint musisz podać IP urządzenia, aby Zainstaluj, aby pomyślnie ukończyć. |  | Tak | Brak |
| 9 | Serwer proxy sieci Web | Jeśli w konfiguracji serwera proxy w sieci web jest protokół HTTPS jako określony protokół, wpłynie na usługi urządzeń komunikacji i urządzenia będą przejść do trybu offline. Obsługa pakietów generowanych jest w procesie przez inne istotne zasobów na urządzeniu. | Upewnij się, że adres URL serwera proxy sieci web ma HTTP jako określony protokół. Więcej informacji na temat [konfiguracji serwera proxy sieci web dla danego urządzenia](storsimple-configure-web-proxy.md). | Tak | Brak |
| 10 | Serwer proxy sieci Web | Jeśli skonfigurować i włączyć serwer proxy sieci web na urządzeniu zarejestrowanych, będzie konieczne ponowne active kontrolera na urządzeniu. |  | Tak | Brak |
| 11 | Opóźnienie chmury wysoki i wysoka obciążenie We/Wy | Gdy urządzenia StorSimple wykryje kombinację bardzo wysoki chmury opóźnienia (kolejność sekund) i wysokiego obciążenia We/Wy, wielkość urządzenia przejdź do stanu ograniczone i operacji dotyczących może zakończyć się niepowodzeniem z powodu błędu "urządzenie nie jest gotowe". | Musisz ręcznie ponownie uruchomić kontrolery urządzeń lub wykonywanie awarię urządzenie na odzyskiwanie z takiej sytuacji. | Tak | Brak |

## <a name="physical-device-updates-in-the-february-release"></a>Zwolnij urządzenia fizycznego aktualizacje z lutego

Ta aktualizacja rozwiązuje problem resetowania factory wystąpienia na urządzenia, które miały została uaktualniona z GA Zwolnij do wersji października 2014 r. Nie zawiera żadnych innych aktualizacji na urządzeniu StorSimple.  

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-the-february-release"></a>Dołączone kolejną kontroler SCSI (SA) i aktualizacji oprogramowania układowego w lutym Zwolnij

Ta wersja nie zawiera żadnych aktualizacji kontrolera SCSI (SA) dołączone kolejną lub oprogramowania układowego. Aktualizacja sterownika jest października 2014 wersji.  

## <a name="virtual-device-updates-in-the-february-release"></a>Zwolnij urządzenia wirtualnego aktualizacje z lutego

Ta wersja nie zawiera wszystkie aktualizacje dla urządzeń wirtualnych. Zastosowanie tej aktualizacji nie ulegnie zmianie wersję oprogramowania urządzenia wirtualnego.
 
