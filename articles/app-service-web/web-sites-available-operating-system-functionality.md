<properties 
    pageTitle="Funkcje systemu operacyjnego na Azure aplikacji usługi" 
    description="Więcej informacji na temat funkcji systemu operacyjnego dostępnych dla aplikacji sieci web, pomocą aplikacji dla urządzeń przenośnych i aplikacji interfejsu API usługi aplikacji Azure" 
    services="app-service" 
    documentationCenter="" 
    authors="cephalin" 
    manager="wpickett" 
    editor="mollybos"/>

<tags 
    ms.service="app-service" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/01/2016" 
    ms.author="cephalin"/>

# <a name="operating-system-functionality-on-azure-app-service"></a>Funkcje systemu operacyjnego na Azure aplikacji usługi #

W tym artykule opisano typowe funkcje systemu operacyjnego według planu bazowego, które są dostępne dla wszystkich aplikacji uruchomionych [Azure aplikacji usługi](http://go.microsoft.com/fwlink/?LinkId=529714). Ta funkcja obejmuje plików, sieci i dostępu do rejestru i dzienniki diagnostyczne i zdarzeń. 

<a id="tiers"></a>
## <a name="app-service-plan-tiers"></a>Warstwy plan usług aplikacji

Usługa aplikacji działa klienta aplikacji w środowisku hostingu wielu dzierżawy. Aplikacje wdrożony w poziomów **bezpłatnego** i **udostępnione** uruchomiony w procesy udostępnionego maszyn wirtualnych, podczas uruchamiania aplikacji wdrożony w **Standard** i **Premium** poziomów na maszyny wirtualne dedykowane specjalnie dla aplikacji skojarzonych z jednego odbiorcy.

Ponieważ usługa aplikacja obsługuje płynną obsługę skalowania między różnych poziomów, konfiguracja zabezpieczeń wymuszane dla aplikacji usługi aplikacji nie zmienia się. Dzięki temu, że aplikacje nie nieoczekiwanie zachowywać się inaczej, niepowodzeniem w nieoczekiwany sposób, jeśli plan usług aplikacji powoduje przejście z jednej warstwy do drugiego.

<a id="developmentframeworks"></a>
## <a name="development-frameworks"></a>Ramy opracowywania

Warstwy cennik usługi aplikacji kontrolowanie natężenia obliczeń zasobów (Procesora, ilość miejsca do magazynowania, pamięci i wyjściowego sieci) dostępnych dla aplikacji. Jednak szerokość framework funkcji dostępnych dla aplikacji zmienia się niezależnie od skalowania warstwy.

Aplikacja usługi obsługuje wiele RAM rozwoju, tym ASP.NET, ASP klasyczny, node.js PHP i Python - wszystkich jest uruchomiony jako rozszerzenia w programie IIS. W celu uproszczenia i znormalizować konfiguracji zabezpieczeń, aplikacji usługi aplikacji zwykle uruchomić różne struktury rozwoju z ustawień domyślnych. Dostosowywanie powierzchni interfejsu API i funkcje dla każdego framework poszczególnych rozwoju mogło być jeden ze sposobów konfigurowania aplikacji. Zamiast tego aplikacji usługi przejście bardziej ogólne podejście, włączając wspólną linię bazową funkcjonalności systemu operacyjnego, niezależnie od tego, w ramach opracowywania aplikacji.

Poniższe sekcje podsumowywanie ogólne rodzaje system operacyjny funkcji dostępnych dla aplikacji usługi aplikacji.

<a id="FileAccess"></a>
##<a name="file-access"></a>Dostęp do plików

Istnieją różne dyski w aplikacji usługi, w tym dysków sieciowych i lokalnych dyskach twardych.

<a id="LocalDrives"></a>
### <a name="local-drives"></a>Dyski lokalne

Zasadniczo aplikacji usługa jest uruchomiony na bieżąco infrastruktury Azure PaaS (platform jako usługa). W wyniku dysków lokalnych, które są "dołączony" maszyn wirtualnych są takie same typy dysk dostępnych dla dowolnego roli Pracownik działa Azure. Ta opcja uwzględnia dysk systemu operacyjnego (dysk D:\), dysk aplikacji, który zawiera pliki cspkg pakietu Azure używane wyłącznie przez usługę aplikacji (i niedostępne dla klientów) i dysk "użytkownika" (dysk C:\) o rozmiarze są różne w zależności od rozmiaru maszyn wirtualnych.

<a id="NetworkDrives"></a>
### <a name="network-drives-aka-unc-shares"></a>Dyskach sieciowych (czyli UNC udostępnia)

Jedną z unikatowe aspekty usługi aplikacji, która ułatwia wdrażaniem aplikacji i konserwacji proste jest, że cała zawartość użytkownika są przechowywane na zbiór udziałach UNC. Ten model bardzo właściwego mapy typowe struktury przechowywania zawartości używane przez lokalnego hostingu środowiska, w których wiele serwerów usługi równoważenia obciążenia. 

W aplikacji usługi istnieje liczba udziałów UNC utworzonych w każdym centrum danych. Wartość procentową zawartości użytkownika dla wszystkich klientów w każdym centrum danych jest przydzielona do każdego udziału UNC. Ponadto udostępnianie wszystkich subskrypcji jednego odbiorcy jest zawsze umieszczane na tym samym UNC zawartość pliku. 

Należy zauważyć, że ze względu na sposób cloud services pracy, określonych maszyny wirtualnej odpowiedzialny za hostingu udziale UNC zmieni się w czasie. Jest zagwarantować, że udziałach UNC zostanie zainstalowany przez różne maszyn wirtualnych zgodnie z ich wprowadzenia w górę lub w dół w normalnym toku operacji chmury. Z tego powodu aplikacje nigdy nie należy ustawić stałe założenia, że informacje o komputera ścieżki UNC pliku jest trwały na czasie. Zamiast tego, czy używać wygodny *symulowane* ścieżki **D:\home\site** , która zapewnia aplikacji usługi. Ta ścieżka bezwzględne symulowane zapewnia przenośnego, aplikacji i użytkownika — niezależne od metodę odwołujące się do własnej aplikacji. Za pomocą **D:\home\site**, jedną mogą przesyłać plików udostępnionych aplikacji aplikacji bez konieczności konfigurowania nowej ścieżki bezwzględne dla każdego termicznego.

<a id="TypesOfFileAccess"></a>
### <a name="types-of-file-access-granted-to-an-app"></a>Typy dostępu do pliku przyznane aplikacji

Każdego z klientów Subskrypcja obejmuje strukturę katalogów zastrzeżone w udziale UNC określone w centrum danych. Klient może mieć wiele aplikacji utworzonych w centrum danych, więc wszystkich katalogów należących do subskrypcji jednego odbiorcy są tworzone na tym samym UNC udostępnianie. Udostępnij może zawierać katalogów, takie jak kontakty dla zawartości, błędów i dzienniki diagnostyczne i starsze niż aplikacja utworzona przez kontrolki źródła. Zgodnie z oczekiwaniami, katalogów aplikacji klienta są dostępne do odczytu i zapisu w czasie rzeczywistym przy użyciu kodu aplikacji tej aplikacji.

Na dyskach lokalnych dołączone do maszyny wirtualnej uruchamianej aplikacji aplikacji usługi zastrzega sobie fragmentu miejsca na dysku C:\ do tymczasowego przechowywania lokalne specyficzne dla aplikacji. Mimo że aplikacja ma pełny odczytu/zapisu do własnej tymczasowe magazynu lokalnego, magazynu naprawdę nie jest przeznaczone do używania bezpośrednio przy użyciu kodu aplikacji. Zamiast zamiarem jest zapewnienie magazynowanie plików tymczasowych dla sieci web i usług IIS środowisk aplikacji. Aplikacji usługi ogranicza również tymczasowe dostępne dla każdej aplikacji, aby uniemożliwić poszczególne aplikacje używające nadmiarowe ilości miejsca do magazynowania pliku lokalnego magazynu lokalnego.

Dwa przykłady używaniu tymczasowych magazynu lokalnego w aplikacji usługi są katalogu plików tymczasowych ASP.NET i katalogu programu IIS skompresowane pliki. System kompilacji programu ASP.NET korzysta z katalogu "Plików tymczasowych ASP.NET" jako lokalizacja pamięci podręcznej tymczasowych kompilacji. Usług IIS korzysta z katalogu "Usług IIS skompresowany pliki tymczasowe" do przechowywania odpowiedzi skompresowane dane wyjściowe. Oba typy plików zastosowania (jak również inne osoby) są mapowane ponownie w aplikacji usługi do magazynu lokalnego tymczasowego-app. Ten ponownego mapowania gwarantuje, że funkcje będzie nadal występował, zgodnie z oczekiwaniami.

Każdy aplikacji w aplikacji usługi działa jako tożsamość procesu losowe unikatowe roboczego uprawnieniach min. o nazwie "tożsamości puli aplikacji", dodatkowo opisanych tutaj: [http://www.iis.net/learn/manage/configuring-security/application-pool-identities](http://www.iis.net/learn/manage/configuring-security/application-pool-identities). Kod aplikacji używa tej tożsamości podstawowe dostęp tylko do odczytu, aby dysk systemu operacyjnego (dysk D:\). Oznacza to, kod aplikacji można wyświetlić listę typowych struktur katalogów i przeczytaj typowych plików na dysk systemu operacyjnego. Mimo że to może sprawiać nieco ogólne poziom dostępu, tym samym katalogi i pliki są dostępne, gdy możesz dodawać do roli pracownika w Azure hostowana usługa i odczytywania zawartości dysku. 

<a name="multipleinstances"></a>
### <a name="file-access-across-multiple-instances"></a>Dostęp do plików w wielu wystąpień

Katalogu macierzystego zawartością aplikacji, a w nim zapisywać kod aplikacji. Jeśli aplikacja działa na wielu wystąpień, katalogu macierzystego współużytkowany wszystkie wystąpienia tak, aby wyświetlić wszystkie wystąpienia tego samego katalogu. Tak na przykład jeśli aplikacja zapisuje przekazane pliki do katalogu macierzystego, te pliki są natychmiast dostępna dla wszystkich wystąpień. 

<a id="NetworkAccess"></a>
## <a name="network-access"></a>Dostęp do sieci
Kod aplikacji można używać TCP/IP i protokołów UDP podstawie nawiązać wychodzące sieci połączenia z Internetem dostępne punkty końcowe, z użyciem usług zewnętrznych. Aplikacje umożliwia łączenie się z usługami w Azure & #151, na przykład przez ustanowienie połączeń HTTPS do bazy danych SQL te same protokoły.

Istnieje także ograniczone możliwości dla aplikacji do nawiązania połączenia z jednym lokalnym sprzężenia zwrotnego i aplikację nasłuchują na tym socket lokalne sprzężenia zwrotnego. Ta funkcja istnieje przede wszystkim, aby włączyć aplikacje, które nasłuchują na lokalnym sprzężenia zwrotnego sockets jako część ich funkcji. Należy zauważyć, że poszczególnych aplikacjach widzi połączenie "prywatne" sprzężenia zwrotnego. Aplikacja "A" nie można słuchać socket lokalne sprzężenia zwrotnego, ustalone przez aplikację "B".

Nazwanych potoków są również obsługiwane jako mechanizm komunikacji między procesami (IPC) między różne procesy, które wspólnie Uruchom aplikację. Na przykład moduł FastCGI programu IIS zależy od nazwanych potoków koordynowanie indywidualnych procesów, uruchamianych stron PHP.


<a id="Code"></a>
## <a name="code-execution-processes-and-memory"></a>Wykonywanie kodu, procesów i pamięci
Jak wspomniano wcześniej, aplikacje uruchamiane wewnątrz uprawnieniach min. procesy przy użyciu tożsamości puli aplikacji losowe. Kod aplikacji ma dostęp do obszaru pamięci skojarzone z procesu roboczego, a także wszystkie podrzędne procesy, które mogą być zduplikowany procesy CGI lub innych aplikacji. Jednak jednej aplikacji nie może uzyskać dostępu pamięci lub danych innej aplikacji nawet w przypadku na tym samym komputerze wirtualnych.

Aplikacje można uruchomić skrypty lub stron zapisanych przy użyciu struktury rozwoju obsługiwane w sieci web. Aplikacji usługi nie skonfigurować ustawienia struktury sieci web w tryb bardziej ograniczony. Na przykład aplikacje ASP.NET uruchomionych dla aplikacji usługi uruchamiane w "pełnego" zaufania, a nie w trybie bardziej ograniczonym zaufania. Ramy sieci Web, w tym ASP klasyczny i ASP.NET, można zadzwonić składniki COM w trakcie (ale nie w nowym składników COM proces), takich jak ADO (ActiveX Data Objects) są rejestrowane domyślnie w systemie operacyjnym Windows.

Aplikacje można zduplikować i uruchomić dowolnego kodu. Jest dozwolony dla aplikacji do wykonywania operacji, takich jak duplikowanie powłoka poleceń lub uruchamianie skryptu programu PowerShell. Jednak mimo że dowolnego kodu i procesów może zduplikowanego z aplikacji, wykonywalny programów i skryptów są nadal ograniczone do uprawnienia przydzielone puli aplikacji nadrzędnej. Na przykład aplikacji można zduplikować plik wykonywalny, który sprawia, że połączenia wychodzące HTTP, ale aby samo wykonywalny nie próby usunięcia powiązania adres IP maszyny wirtualnej z jego karty NIC. Nawiązywanie połączenia wychodzącego może być niska uprawnieniach kodu, ale próby skonfigurowanie ustawień sieci na komputerze wirtualną wymaga uprawnień administracyjnych.


<a id="Diagnostics"></a>
## <a name="diagnostics-logs-and-events"></a>Dzienniki diagnostyczne i zdarzeń
Informacje dziennika jest inny zestaw danych, który niektóre aplikacje będą próbować uzyskać dostęp. Typy informacji dziennika dostępna dla kodu działa w aplikacji usługi zawiera diagnostyczne i rejestrowanie informacji generowanych przez aplikację, która jest również łatwy do aplikacji. 

Na przykład W3C HTTP dzienniki generowane przez aplikację aktywne są dostępne, albo w katalogu dziennika w lokalizację udziału sieciowego utworzonych dla aplikacji usługi lub dostępne w magazynie obiektów blob, jeśli odbiorca ma skonfigurowane rejestrowanie W3C do magazynu. Opcja ostatni umożliwia dużych ilości dzienniki zbierane bez ryzyka przekraczają limity miejsca do magazynowania plików skojarzone z udziału sieciowego.

W podobny szyjnej informacje diagnostyczne w czasie rzeczywistym w aplikacjach .NET mogą również być rejestrowane przy użyciu śledzenie .NET i infrastruktury diagnostyki z opcjami umożliwiającymi zapisywać informacji o śledzeniu w udziale sieciowym w aplikacji, albo do lokalizacji przechowywania obiektów blob.

Obszary diagnostyki rejestrowania i śledzenia, które nie są dostępne dla aplikacji są zdarzenia ETW systemu Windows i typowych dzienniki zdarzeń systemu Windows (np. systemu, aplikacji i zabezpieczeń dzienniki zdarzeń). Ponieważ informacje o śledzeniu ETW potencjalnie może być widoczny dla komputera (z prawą stronę ACL), są blokowane odczytu i zapisu do zdarzenia ETW. Deweloperów zauważyć, że interfejs API wymaga do odczytu i zapisu ETW wydarzeń i typowych dzienniki zdarzeń systemu Windows są wyświetlane do pracy, ale jest tak, ponieważ aplikacji usługi jest "faking" połączenia, aby były wyświetlane powiodła się. W rzeczywistości kod aplikacji nie ma dostępu do tych danych zdarzenia.

<a id="RegistryAccess"></a>
## <a name="registry-access"></a>Dostęp do rejestru
Aplikacje mają dostęp tylko do odczytu, aby większość (ale nie wszystkie) rejestru maszyny wirtualnej są uruchamiane na. W praktyce oznacza to, że klucze rejestru, które umożliwiają dostęp tylko do odczytu do lokalnej grupy użytkowników są dostępne dla aplikacji. Jednego obszaru rejestru, który nie jest obecnie obsługiwane dla odczytu lub zapisu jest HKEY\_bieżącego\_gałęzi użytkownika.

Zapisu w rejestrze jest zablokowany, łącznie z dostępem do kluczy rejestru dla poszczególnych użytkowników. Z perspektywy aplikacji do zapisu w rejestrze należy nigdy nie opierać w środowisku Azure ponieważ aplikacje mogą (i wykonaj) Uzyskaj migracji na różnych komputerach wirtualnych. Tylko trwałych zapisywalny przestrzeni dyskowej, które mogą być zależne od przez aplikację jest strukturą katalog zawartości dla aplikacji przechowywanych w udziałach UNC usługi aplikacji. 

>[AZURE.NOTE] Jeśli chcesz rozpocząć pracę z Azure aplikacji usługi przed utworzeniem konta dla konta Azure, przejdź do [Spróbuj aplikacji usługi](http://go.microsoft.com/fwlink/?LinkId=523751), którym natychmiast można utworzyć aplikację sieci web krótkotrwałe starter w aplikacji usługi. Nie kart kredytowych wymagane; nie zobowiązania.

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]
 
 
