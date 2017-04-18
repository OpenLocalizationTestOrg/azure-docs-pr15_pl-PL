<properties
    pageTitle="Najważniejsze wskazówki i rozwiązywania problemów — przewodnik dotyczący aplikacji węzeł na Azure aplikacji sieci Web"
    description="Dowiedz się, najlepszymi rozwiązaniami i rozwiązywania problemów aplikacji węzeł na Azure aplikacji sieci Web."
    services="app-service\web"
    documentationCenter="nodejs"
    authors="ranjithr"
    manager="wadeh"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="06/06/2016"
    ms.author="ranjithr;wadeh"/>
    
# <a name="best-practices-and-troubleshooting-guide-for-node-applications-on-azure-web-apps"></a>Najważniejsze wskazówki i rozwiązywania problemów — przewodnik dotyczący aplikacji węzeł na Azure aplikacji sieci Web

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

W tym artykule dowiesz się najlepszymi rozwiązaniami i rozwiązywania problemów z [aplikacji węzeł](app-service-web-nodejs-get-started.md) uruchomione na używanie Azure (z [iisnode](https://github.com/azure/iisnode)).

>[AZURE.WARNING] Przy użyciu kroków w witrynie produkcji, należy zachować ostrożność. Zaleca się Rozwiązywanie problemów z aplikacji w ustawieniach nie produkcji na przykład z tymczasowy przedział i jeśli problem został rozwiązany, Zamień usługi tymczasowy przedział z Twojej przedział produkcji.

## <a name="iisnode-configuration"></a>Konfiguracja IISNODE

Ten [plik schematu](https://github.com/Azure/iisnode/blob/master/src/config/iisnode_schema_x64.xml) zawiera wszystkie ustawienia, które można skonfigurować dla iisnode. Niektóre z ustawień, które mogą być przydatne dla aplikacji są:

* nodeProcessCountPerApplication

    To ustawienie określa liczbę procesów węzeł, które są uruchomione na aplikacji usług IIS. Wartością domyślną jest 1. Możesz uruchomić dowolną liczbę node.exe jako liczba core maszyn wirtualnych przez ustawienie to 0. Zalecana wartość wynosi 0 dla większości aplikacji, więc można korzystać ze wszystkich rdzenie na tym komputerze. Node.exe jest jednym z wątkami tak jeden node.exe zajmie maksymalnie 1 podstawowych i uzyskanie Maksymalna wydajność wylogowywanie się z aplikacji węzeł chcesz korzystać wszystkie rdzenie.

* nodeProcessCommandLine

    To ustawienie określa ścieżkę do node.exe. Można ustawić tę wartość, aby wskazywały do wersji node.exe.

* maxConcurrentRequestsPerProcess

    To ustawienie określa maksymalną liczbę żądań wysyłane przez iisnode do każdego node.exe. Na używanie azure wartość domyślna to jest bez zatrzymania. Nie musisz martwić się o to ustawienie. Poza azure używanie wartość domyślna wynosi 1024. Można to skonfigurować w zależności od liczby żądań, otrzymuje aplikacji i szybkości aplikacja przetwarza każde żądanie.

* maxNamedPipeConnectionRetry

    To ustawienie określa maksymalną liczbę godzin iisnode będzie ponawiał próby wprowadzania połączenia na nazwanych potoków, aby wysłać wezwanie na do node.exe. To ustawienie w połączeniu z namedPipeConnectionRetryDelay określa całkowity limit czasu każdego żądania w iisnode. Wartość domyślna to 200 na używanie Azure. Całkowity limit czasu w sekundach = (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000

* namedPipeConnectionRetryDelay

    To ustawienie określa ilość czasu (w ms) iisnode będzie czekać na między kolejnymi próbami, aby wysłać żądanie do node.exe nad nazwanych potoków. Wartość domyślna to 250ms.
    Całkowity limit czasu w sekundach = (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000

    Domyślnie całkowity limit czasu w iisnode na używanie azure wynosi 200 \* 250ms = 50 sekund.

* logDirectory

    To ustawienie określa miejsce, w którym iisnode można rejestrować stdout i stderr katalogu. Wartość domyślna to iisnode, czyli względem katalogu głównego skryptów (miejsce, w którym znajduje się server.js głównego katalogu)

* debuggerExtensionDll

    To ustawienie określa, która wersja programu iisnode węzeł Inspektor ma być używany podczas debugowania aplikacji węzeł. Obecnie iisnode Inspektor 0.7.3.dll i iisnode inspector.dll są tylko 2 prawidłowe wartości dla tego ustawienia. Wartość domyślna to iisnode Inspektor 0.7.3.dll. Wersja iisnode Inspektor 0.7.3.dll używa węzeł Inspektor-0.7.3 i wykorzystuje websockets, więc musisz włączyć websockets na azure aplikacji sieci Web przy użyciu tej wersji. Zobacz <http://www.ranjithr.com/?p=98> , aby uzyskać więcej informacji na temat konfigurowania iisnode, korzystając z nowego inspektora węzeł.

* flushResponse

    Domyślne zachowanie usług IIS to, że jej buforów danych odpowiedzi w górę 4 MB przed opróżniania lub do końca odpowiedzi, zależnie od jest pierwsze. iisnode oferuje ustawienia konfiguracji, aby zastąpić to zachowanie: Aby opróżnić fragment treść jednostki odpowiedzi, jak iisnode otrzymujących list z node.exe, należy ustawić iisnode/@flushResponse atrybut w pliku web.config "true":
    
    ```
    <configuration>    
        <system.webServer>    
            <!-- ... -->    
            <iisnode flushResponse="true" />    
        </system.webServer>    
    </configuration>
    ```

    Włączanie opróżnianie każdego fragmentu treść jednostki odpowiedzi zwiększa wydajność który ogranicza przepustowość sieci ~ 5% (od v0.1.13), więc warto zakresu to ustawienie tylko do punktów końcowych wymagających streaming odpowiedzi (np <location> element w pliku web.config)

    Oprócz tego do przesyłania strumieniowego aplikacji, należy również ustawić responseBufferLimit do obsługi iisnode 0.
    
    ```
    <handlers>    
        <add name="iisnode" path="app.js" verb="\*" modules="iisnode" responseBufferLimit="0"/>    
    </handlers>
    ```

* watchedFiles

    To jest lista rozdzielanej średnikami plików, które będą obserwowane zmiany. Zmiany w pliku powoduje zastosowanie do Kosza. Każdy wpis zawiera nazwy katalogu opcjonalne oraz nazwę wymaganego pliku, które są względem katalog, w którym znajduje się punkt wejścia głównej aplikacji. Symbole wieloznaczne są dozwolone w tylko część nazwy pliku. Wartość domyślna to "\*. js;web.config"

* recycleSignalEnabled

    Wartość domyślna to false. Włączenie aplikacji węzeł można nawiązać nazwanych potoków (zmienna środowiska IISNODE\_sterowania\_POTOKU) i wysłać wiadomość "odtworzenia". Spowoduje to w3wp do Kosza bezpiecznie.

* idlePageOutTimePeriod

    Wartość domyślna to 0, co oznacza, że ta funkcja jest wyłączona. Jeśli skonfigurowano pewną wartość większą niż 0, iisnode będzie strony się swoich procesów podrzędnych co milisekund "idlePageOutTimePeriod". Aby dowiedzieć się, jakie strony się oznacza, można znaleźć w [dokumentacji](https://msdn.microsoft.com/library/windows/desktop/ms682606.aspx). To ustawienie będą przydatne w przypadku aplikacji, które używają dużej ilości pamięci i chcesz, aby pageout pamięci na dysk od czasu do czasu w celu zwolnienia niektórych pamięci RAM.

>[AZURE.WARNING] Włączanie następujące ustawienia konfiguracji aplikacji produkcji, należy zachować ostrożność. Zaleca się nie umożliwia im produkcyjnym aplikacji.

* debugHeaderEnabled

    Wartość domyślna to false. Jeśli ustawiono wartość true, iisnode doda HTTP odpowiedzi nagłówek iisnode debugowania każda odpowiedź HTTP wyśle odbiorcy, że wartość nagłówka iisnode debugowania jest adresu URL. Poszczególne części informacji diagnostycznych mogą zgromadzone sprawdzając fragment adresu URL, ale dużo lepiej wizualizacji to osiągnąć przez otwarcie adresu URL w przeglądarce.

* loggingEnabled

    To ustawienie określa rejestrowanie stdout i stderr przez iisnode. Iisnode Przechwytywanie stdout i stderr z procesów węzeł, który jest uruchamiany, a następnie napisać do katalogu określonego w ustawieniu "logDirectory". Gdy jest włączone, aplikacja będzie można pisania dzienniki w systemie plików i w zależności od ilości rejestrowanie wykonywane przez aplikację, może być wpływu na wydajność.

* devErrorsEnabled

    Wartość domyślna to false. Gdy ustawiono wartość true, iisnode wyświetli kod stanu HTTP i kod błędu Win32 w przeglądarce. Kod win32 będą pomocne w debugowania niektórych rodzajów problemów.
    
* debuggingEnabled (nie włączono w witrynie produkcyjnym)

    To ustawienie określa funkcja debugowania. Iisnode jest zintegrowany z Inspektor węzeł. Po włączeniu tego ustawienia, należy włączyć debugowanie aplikacji węzeł. Po włączeniu tego ustawienia iisnode będzie układu pliki niezbędne Inspektor węzeł w katalogu "debuggerVirtualDir" po pierwszym żądaniu debugowania do aplikacji węzeł. Można załadować inspektora węzeł, wysyłając żądania do http://yoursite/server.js/debug. Możesz sterować część adresu URL debugowania z ustawieniem "debuggerPathSegment". Domyślne debuggerPathSegment = "debugowanie". Można ustawić to na identyfikator GUID, na przykład tak, aby trudniej wykrycie przez inne osoby.

    Zaznacz to [łącze](https://tomasz.janczuk.org/2011/11/debug-nodejs-applications-on-windows.html) , aby uzyskać więcej informacji dotyczących debugowania.

## <a name="scenarios-and-recommendationstroubleshooting"></a>Scenariuszy i rozwiązywania problemów i zalecenia

### <a name="my-node-application-is-making-too-many-outbound-calls"></a>Moja aplikacja węzeł wprowadza zbyt wiele połączeń wychodzących.

Wiele aplikacji czy chcesz nawiązywać połączenia wychodzące jako część ich normalnej pracy. Na przykład gdy nadejdzie żądanie, aplikacji węzeł chcieć skontaktuj się z interfejsu API usługi REST gdzie indziej i uzyskać określone informacje przetwarzania żądania. Czy chcesz używać agenta aktywności Zachowaj podczas nawiązywania połączenia http lub https. Na przykład można moduł agentkeepalive jako swojej aktywności Zachowaj te wywołania ruchu wychodzącego. Dzięki temu, że sockets są używane ponownie na azure Web App maszyn wirtualnych i zmniejszając koszt tworzenia nowego sockets dla każdego żądania ruchu wychodzącego. Ponadto dzięki temu się, że korzystasz z mniejszej liczby sockets zgłaszać wiele żądań ruchu wychodzącego i dlatego nie może przekraczać ustawienie maxSockets, które są przydzielane na maszyn wirtualnych. Zalecenie dotyczące używanie Azure jest wartość agentKeepAlive ustawienie maxSockets sumę 160 sockets na maszyn wirtualnych. Oznacza to, jeśli masz 4 node.exe uruchomionych maszyn wirtualnych chcesz chcesz ustawić ustawienie maxSockets agentKeepAlive 40 na node.exe czyli 160 Suma wg maszyn wirtualnych.

Przykład agentKeepALive konfiguracji:

```
var keepaliveAgent = new Agent({    
    maxSockets: 40,    
    maxFreeSockets: 10,    
    timeout: 60000,    
    keepAliveTimeout: 300000    
});
```

W tym przykładzie założono, że masz 4 node.exe uruchomionych dla swojego maszyn wirtualnych. Jeśli masz inne liczby node.exe uruchomionych maszyn wirtualnych, należy zmodyfikować ustawienie maxSockets, ustawiając odpowiednio.

### <a name="my-node-application-is-consuming-too-much-cpu"></a>Moja aplikacja węzeł jest używające zbyt dużo Procesora.

Zalecenie będą prawdopodobnie przejść z używanie Azure portalu sieci o wysokiej użycie procesora. Możesz również skonfigurować monitorów, aby obejrzeć dla niektórych [metryki](web-sites-monitor.md). Podczas sprawdzania użycie Procesora na [Pulpicie nawigacyjnym Portal Azure](../application-insights/app-insights-web-monitor-performance.md), sprawdź, czy wartość maksymalna liczba procesorów aby nie przeoczyć wartości Szczyt.
W przypadku tam, gdzie oczekujesz aplikacji jest używające zbyt dużo Procesora i nie wyjaśniają przyczyny będzie konieczne profilu aplikacji węzeł.

### 

#### <a name="profiling-your-node-application-on-azure-webapps-with-v8-profiler"></a>Profilowanie aplikacji węzeł na używanie azure z V8 Profiler

Na przykład umożliwia powiedz aplikację świata powitania, który ma być profilu, tak jak pokazano poniżej:

```
var http = require('http');    
function WriteConsoleLog() {    
    for(var i=0;i<99999;++i) {    
        console.log('hello world');    
    }    
}

function HandleRequest() {    
    WriteConsoleLog();    
}

http.createServer(function (req, res) {    
    res.writeHead(200, {'Content-Type': 'text/html'});    
    HandleRequest();    
    res.end('Hello world!');    
}).listen(process.env.PORT);
```

Przejdź do swojej https://yoursite.scm.azurewebsites.net/DebugConsole witryny Menedżer sterowania usługami

Zostanie wyświetlona wiersz polecenia, jak pokazano poniżej. Przejdź do katalogu witryn i wwwroot

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/scm_install_v8.png)

Uruchom polecenie "npm zainstalować v8 profiler"

To należy zainstalować program profilujący v8 węźle\_katalogu moduły i wszystkich jej zależności.
Teraz edytować usługi server.js do profilu aplikacji.

```
var http = require('http');    
var profiler = require('v8-profiler');    
var fs = require('fs');

function WriteConsoleLog() {    
    for(var i=0;i<99999;++i) {    
        console.log('hello world');    
    }    
}

function HandleRequest() {    
    profiler.startProfiling('HandleRequest');    
    WriteConsoleLog();    
    fs.writeFileSync('profile.cpuprofile', JSON.stringify(profiler.stopProfiling('HandleRequest')));    
}

http.createServer(function (req, res) {    
    res.writeHead(200, {'Content-Type': 'text/html'});    
    HandleRequest();    
    res.end('Hello world!');    
}).listen(process.env.PORT);
```

Powyżej zmian zostanie profilu funkcji WriteConsoleLog, a następnie wpisz dane wyjściowe profilu do pliku "profile.cpuprofile" w obszarze wwwroot swojej witryny. Wysłanie wezwania do aplikacji. Zostanie wyświetlona "profile.cpuprofile" pliku utworzonego w obszarze wwwroot swojej witryny.

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/scm_profile.cpuprofile.png)

Pobierz ten plik, a następnie należy otworzyć tego pliku przy użyciu narzędzi F12 Chrome. Naciśnij F12 na chrome, a następnie na karcie"profile". Kliknij przycisk "Obciążenie". Wybierz plik do profile.cpuprofile, który został pobrany. Kliknij profil, który właśnie załadowane.

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/chrome_tools_view.png)

Zobaczysz, że 95% czasu została wykorzystana przez funkcję WriteConsoleLog, tak jak pokazano poniżej. To również pokazano dokładnie numery i pliki źródłowe, powodujące problem.

### <a name="my-node-application-is-consuming-too-much-memory"></a>Moja aplikacja węzeł jest użyciu zbyt dużej ilości pamięci.

Zalecenie będą prawdopodobnie przejść z używanie Azure portalu sieci o zużycie pamięci wysokiej. Możesz również skonfigurować monitorów, aby obejrzeć dla niektórych [metryki](web-sites-monitor.md). Podczas sprawdzania użycie pamięci na [Pulpicie nawigacyjnym Portal Azure](../application-insights/app-insights-web-monitor-performance.md), sprawdź, czy wartości MAX pamięci, aby nie przeoczyć wartości Szczyt.

#### <a name="leak-detection-and-heap-diffing-for-nodejs"></a>Pamięci wykrywanie i Diffing stosu dla node.js 

[Węzeł memwatch](https://github.com/lloyd/node-memwatch) można użyć, aby zidentyfikować przecieki pamięci.
Można zainstalować memwatch tak jak v8 profiler i edytowanie kodu do przechwytywania i różnic stert do identyfikowania pamięć przeciek w aplikacji.

### <a name="my-nodeexes-are-getting-killed-randomly"></a>Moje node.exe zabijanych wprowadzenie losowo 

Istnieje kilka możliwych powodów, dlaczego to może się nie dzieje:

1.  Aplikacja jest generowania nie przechwycony wyjątków — należy wyboru d:\\głównym\\LogFiles\\aplikacji\\pliku errors.txt rejestrowania, aby uzyskać szczegóły na wyjątek. Ten plik ma śledzenia stosu, aby naprawić oparte na tej aplikacji.

2.  Aplikacja jest używające zbyt dużej ilości pamięci, który ma wpływ na inne procesy wprowadzenie. Jeśli pamięć maszyn wirtualnych jest zbliżony 100%, z node.exe można kasować przy Menedżer procesów, aby powiadomić inne procesy umożliwiają wykonywanie niektórych zadań. Aby rozwiązać ten problem, upewnij się, że aplikacja nie ma przecieki pamięci lub jeśli naprawdę aplikacji należy używać dużej ilości pamięci, należy rozmiarów większych maszyn wirtualnych z znacznie więcej pamięci RAM.

### <a name="my-node-application-does-not-start"></a>Nie można uruchomić aplikacji pakietu węzeł

Jeśli aplikacja zwraca 500 błędy podczas uruchamiania, może istnieć kilka przyczyn:

1.  Node.exe nie ma w poprawnej lokalizacji. Sprawdź ustawienia nodeProcessCommandLine.

2.  Plik skryptu głównym nie ma w poprawnej lokalizacji. Sprawdź web.config i upewnij się, że nazwa pliku skryptu głównym w sekcji Obsługa odpowiada plik skryptu głównym.

3.  Konfiguracja Web.config nie jest prawidłowa — Sprawdzanie nazwy i wartości ustawień.

4.  Rozpocznij zimnej — aplikacja trwa zbyt długo do uruchamiania. Jeśli aplikacja trwa dłużej niż (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000 sekund iisnode zwróci błąd 500. Zwiększanie wartości tych ustawień, zgodnie z czasem rozpoczęcia aplikacji aby zapobiec iisnode z Przekroczono limit czasu i zwraca błąd 500.

### <a name="my-node-application-crashed"></a>Moja aplikacja węzeł awarii

Aplikacja jest generowania nie przechwycony wyjątków — należy wyboru d:\\głównym\\LogFiles\\aplikacji\\pliku errors.txt rejestrowania, aby uzyskać szczegóły na wyjątek. Ten plik ma śledzenia stosu, aby naprawić oparte na tej aplikacji.

### <a name="my-node-application-takes-too-much-time-to-startup-cold-start"></a>Moja aplikacja węzeł trwa zbyt wiele czasu na uruchamianie (rozpoczynać zimnej)

Najczęstsze przyczyny to to, że aplikacja ma wiele plików w węźle\_moduły i aplikacja próbuje załadować większość tych plików podczas uruchamiania. Domyślnie ponieważ pliki znajdują się w udziale sieciowym na używanie Azure ładowania tak dużo plików może zająć trochę czasu.
Są to przyspieszyć rozwiązania:

1.  Upewnij się, że masz żadnych zduplikowanych zależności i struktury prostym zależności za pomocą npm3 moduły.

2.  Spróbuj opóźnieniem ładowanie węzeł\_moduły i nie załadować wszystkie moduły podczas uruchamiania. Oznacza to, że połączenie require('module') zostanie przeprowadzone podczas rzeczywiście potrzebujesz funkcji, spróbuj użyć modułu.

3.  Używanie Azure oferuje funkcję o nazwie lokalnej pamięci podręcznej. Ta funkcja kopiuje zawartość z udziału sieciowego na lokalnym dysku maszyn wirtualnych. Ponieważ pliki są lokalne, podczas ładowania węzła\_moduły przebiega szybciej. — [Dokumentacji](../app-service/app-service-local-cache.md) wyjaśniono, jak korzystać z lokalnej pamięci podręcznej bardziej szczegółowo.

## <a name="iisnode-http-status-and-substatus"></a>IISNODE http i podstanów

Ten [plik źródłowy](https://github.com/Azure/iisnode/blob/master/src/iisnode/cnodeconstants.h) zawiera listę wszystkich, zwrócenie przez iisnode kombinację możliwe stan/podstanu w przypadku błędu.

Włączanie FREB dla aplikacji wyświetlić kod błędu win32 (Upewnij się, możesz włączyć FREB tylko w witrynach innych niż produkcji ze względu na wydajność).

| Stan HTTP | Podstan protokołu HTTP | Możliwe przyczyny?                                                                                                                                                                                            
|-------------|----------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------
| 500         | 1000.           | Wystąpił niektórych problem wysyłki żądanie IISNODE — Sprawdź, jeśli została uruchomiona node.exe. Node.exe może ulec awarii podczas uruchamiania. Sprawdź konfigurację web.config pod kątem błędów.                                                                                                                                                                                                                                                                                     |
| 500         | 1001           | -Win32Error 0x2 — nie odpowiada aplikacji do adresu URL. Zaznacz adres URL zmiana reguł lub jeśli express aplikacji zawiera poprawny trasy zdefiniowane. -Win32Error 0x6d — nazwanych potoków jest zajęty — Node.exe nie akceptuje żądania, ponieważ potoku jest zajęty. Sprawdź wysoka użycie procesora. -Inne błędy — Sprawdź, jeśli node.exe awarii.
| 500         | 1002           | Awaria node.exe — sprawdzanie d:\\głównym\\LogFiles\\rejestrowanie errors.txt do śledzenia stosu.                                                                                                                                                                                                                                                                                                                                                                                        |
| 500         | 1003           | Potok konfiguracji problem — nigdy nie powinna być widoczna to, ale jeśli jednak konfiguracja nazwanych potoków jest niepoprawna.                                                                                                                                                                                                                                                                                                                                                          |
| 500         | 1004 1018      | Wystąpił wystąpił błąd podczas wysyłania żądania lub przetwarzania odpowiedzi od node.exe. Sprawdzanie, jeśli node.exe awarii. Sprawdzanie d:\\głównym\\LogFiles\\rejestrowanie errors.txt do śledzenia stosu.                                                                                                                                                                                                                                                                                    |
| 503         | 1000.           | Za mało pamięci, aby przydzielić więcej nazwanych potoków połączenia. Upewnij się, dlaczego aplikacji jest użyciu dużej ilości pamięci. Sprawdź wartość ustawienia maxConcurrentRequestsPerProcess. Jeśli nie bez zatrzymania, a w przypadku dużej żądania, zwiększyć tę wartość, aby zapobiec ten błąd.                                                                                                                                                                                                                                                                                                                  |
| 503         | 1001           | Nie można wysłać żądanie node.exe, ponieważ odtwarzanie aplikacji. Po aplikacji po odtworzeniu, żądania powinien zwykle obsługiwane.                                                                                                                                                                                                                                                                                                               |
| 503         | 1002           | Kod błędu win32 wyboru przyczyny rzeczywisty — żądanie nie można wysłać do node.exe.                                                                                                                                                                                                                                                                                                                                                                               |
| 503         | 1003           | Nazwanych potoków jest zbyt zajęty — Sprawdź, jeśli węzeł przez inne wielu procesorów                                                                                                                                                                                                                                                                                                                                                                                                        
                                                                                                                                                                                                                                                                                                            
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         
Ustawienie poziomu NODE.exe węzeł\_OCZEKUJE\_POTOKU\_wystąpienia. Ta wartość jest domyślnie poza azure używanie 4. Oznacza to, że tego node.exe tylko mogą przyjmować żądania 4 w czasie na nazwanych potoków. Na używanie Azure tę wartość do 5000 i ta wartość powinna być niewystarczający dla większości węzeł aplikacje na używanie azure. Czy nie widzisz 503.1003 na używanie azure, ponieważ mamy wysokiej wartości węzła\_OCZEKUJE\_RURA\_wystąpienia.  |

## <a name="more-resources"></a>Więcej zasobów

Skorzystaj z poniższych łączy, aby dowiedzieć się więcej o aplikacjach node.js Azure aplikacji usługi.

* [Wprowadzenie do aplikacji sieci web Node.js w Azure aplikacji usługi](app-service-web-nodejs-get-started.md)
* [Sposób debugowania Node.js aplikacji web app w usłudze Azure aplikacji](web-sites-nodejs-debug.md)
* [Moduły Node.js za pomocą aplikacji Azure](../nodejs-use-node-modules-azure-apps.md)
* [Usługa Azure aplikacji Web Apps: Node.js](https://blogs.msdn.microsoft.com/silverlining/2012/06/14/windows-azure-websites-node-js/)
* [Centrum deweloperów node.js](../nodejs-use-node-modules-azure-apps.md)
* [Poznawanie Konsola debugowania Super tajne Kudu](https://azure.microsoft.com/documentation/videos/super-secret-kudu-debug-console-for-azure-web-sites/)