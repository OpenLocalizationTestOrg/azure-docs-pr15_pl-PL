<properties 
    pageTitle="Konfigurowanie aplikacji sieci web w usłudze Azure aplikacji" 
    description="Jak skonfigurować aplikację sieci web w usługach aplikacji Azure" 
    services="app-service\web" 
    documentationCenter="" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="configure-web-apps-in-azure-app-service"></a>Konfigurowanie aplikacji sieci web w usłudze Azure aplikacji #

W tym temacie wyjaśniono, jak skonfigurować aplikację sieci web za pomocą [Azure Portal].

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="application-settings"></a>Ustawienia aplikacji

1. Otwórz [Azure Portal]karta dla aplikacji sieci web.
2. Kliknij pozycję **wszystkie ustawienia**.
3. Kliknij pozycję **Ustawienia aplikacji**.

![Ustawienia aplikacji][configure01]

Karta **Ustawienia aplikacji** ma ustawienia zgrupowane w kilka kategorii.

### <a name="general-settings"></a>Ustawienia ogólne

W **ramach wersji**. Jeśli aplikacji używa tych ram, należy ustawić te opcje: 

- **.NET Framework**: Ustawianie programu .NET framework w wersji. 
- **PHP**: Ustawianie wersji PHP lub **Wył** ., aby wyłączyć PHP. 
- **Java**: zaznacz wersji języka Java lub **Wył** ., aby wyłączyć Java. Opcja **Kontenera sieci Web** umożliwia wybieranie między wersjami Tomcat i molo.
- **Python**: Wybierz wersję Python lub **Wył** ., aby wyłączyć Python.

Przyczyn technicznych Włączanie Java dla aplikacji powoduje wyłączenie opcji .NET, PHP i Python.

<a name="platform"></a>
**Platformy**. Umożliwia wybranie, czy aplikacji sieci web działa w środowisku 32-bitowej lub 64-bitowej. 64-bitowego środowiska wymaga trybu Basic lub standardowe. Zwolnij i trybów udostępnione są zawsze uruchamiane w środowisku 32-bitowym.

**Sockets sieci web**. Ustawianie **włączone** , aby włączyć protokół WebSocket; Jeśli na przykład użyto aplikacji sieci web [Programu ASP.NET SignalR] lub [socket.io].

<a name="alwayson"></a>
**Zawsze włączone**. Domyślnie aplikacje sieci web są usuwane, jeśli są one bezczynne przez niektóre czas. Dzięki temu system ochrony zasobów. W trybie Basic lub standardowe możesz włączyć **Zawsze włączone** do zachowania aplikacji załadowane przez cały czas. Jeśli aplikacji uruchamia zadania ciągły sieci web, należy włączyć **Zawsze na**lub zadania sieci web może nie działać prawidłowo.

**Zarządzany planowana wersji**. Ustawia usług IIS [Tryb potoku]. Pozostaw wartość układy (ustawienie domyślne), jeśli nie masz starszych aplikacji, która wymaga ze starszej wersji programu IIS.

**Przełącz automatycznie**. Po włączeniu automatycznego wymiany dla przedział wdrażania aplikacji usługi automatycznie Zamień aplikacji sieci web do produkcji po naciśnięciu aktualizację do tego przedział. Aby uzyskać więcej informacji, zobacz temat [Deploy z tymczasową gniazda dla aplikacji sieci web w usłudze Azure aplikacji] (web witryn — umieszczane publishing.md).

### <a name="debugging"></a>Debugowanie

**Zdalne debugowanie**. Włącza debugowanie zdalnego. Po włączeniu, aby połączyć się bezpośrednio z aplikacji sieci web za pomocą zdalnego debugowania w programie Visual Studio. Zdalne debugowanie pozostaną włączone 48 godzin. 

### <a name="app-settings"></a>Ustawienia aplikacji

Ta sekcja zawiera pary nazwa wartość, które możesz sieci web w górę pobierze po uruchomieniu aplikacji. 

- W przypadku aplikacji .NET te ustawienia zostaną dodane do konfiguracji .NET `AppSettings` w czasie wykonywania zastępowanie istniejących ustawień. 

- Aplikacje PHP, Python, Java i węzeł dostęp do tych ustawień, jak zmienne środowiska w czasie rzeczywistym. Dla każdego ustawienia aplikacji są tworzone dwie zmienne środowiska; jeden o podanej nazwie przy wpisie ustawienia aplikacji, a drugi z prefiksem APPSETTING_. Zawierają taką samą wartość.

### <a name="connection-strings"></a>Parametry połączenia

Parametry połączenia dla połączonej zasobów. 

W przypadku aplikacji .NET te parametry połączenia zostaną dodane do konfiguracji .NET `connectionStrings` ustawienia w czasie wykonywania zastępowanie istniejących wpisów, gdzie klucz równa się nazwie połączonej bazy danych. 

W przypadku aplikacji PHP, Python, Java i węzeł te ustawienia będą dostępne jako zmienne środowiska w czasie wykonywania prefiksem typu połączenia. Prefiksy zmiennych środowiska są następujące: 

- Program SQL Server:`SQLCONNSTR_`
- MySQL:`MYSQLCONNSTR_`
- Funkcje bazy danych SQL`SQLAZURECONNSTR_`
- Niestandardowy:`CUSTOMCONNSTR_`

Na przykład jeśli nazwany parametry połączenia MySql `connectionstring1`, czy są dostępne za pośrednictwem zmiennej środowiska `MYSQLCONNSTR_connectionString1`.

### <a name="default-documents"></a>Dokumenty domyślne

Dokument domyślny jest strona sieci web jest wyświetlany pod adresem URL głównej witryny sieci Web.  Pierwszy odpowiedniego pliku na liście jest używana. 

Aplikacje sieci Web może używać modułów, że trasy na podstawie adresu URL, a nie obsługujących zawartość statyczną, w takim przypadku nie jest jako takie nie dokument domyślny.    

### <a name="handler-mappings"></a>Mapowania obsługi

W tym obszarze umożliwia dodawanie procesorów skryptu niestandardowego do obsługi żądań dla określonego rozszerzenia. 

- **Rozszerzenie**. Rozszerzenie pliku do obsługi, takich jak *.php lub handler.fcgi. 
- **Ścieżka procesor skryptu**. Ścieżka bezwzględna procesor skryptów. Żądania dostępu do plików, które są zgodne z rozszerzeniem zostanie przetworzony przez procesor skryptów. Za pomocą ścieżki `D:\home\site\wwwroot` Aby odwołać się do Twojej aplikacji katalogu.
- **Dodatkowe argumenty**. Opcjonalnie argumenty wiersza polecenia dla procesora skryptu 


### <a name="virtual-applications-and-directories"></a>Aplikacje wirtualne i katalogów 
 
Aby skonfigurować aplikacje wirtualne i katalogi, określ każdego katalogu wirtualnego i jego odpowiedniej ścieżki fizycznej względem na poziomie głównym witryny sieci Web. Opcjonalnie można zaznacz pole wyboru **aplikacji** , aby oznaczyć katalogu wirtualnego jako aplikacji.


## <a name="enabling-diagnostic-logs"></a>Włączanie dzienników diagnostycznych

Aby włączyć dzienników diagnostycznych:

1. W karta dla aplikacji sieci web kliknij pozycję **wszystkie ustawienia**.
2. Kliknij pozycję **Dzienniki diagnostyczne**. 

Opcje zapisywania dzienniki diagnostyczne z aplikacji sieci web, która obsługuje rejestrowania: 

- **Rejestrowanie aplikacji**. Zapisuje Dzienniki aplikacji w systemie plików. Rejestrowanie okresu w okresie 12 godzin. 

**Poziom**. Po włączeniu rejestrowania aplikacji, ta opcja określa, że ilość informacji, które będą rejestrowane (błędu, ostrzeżenia, informacje lub pełne).

**Rejestrowanie serwera sieci web**. Dzienniki są zapisywane w formacie pliku dziennika rozszerzonego W3C. 

**Szczegółowe komunikaty o błędach**. Pliki htm komunikatami o błędach szczegółowe zapisuje. Pliki są zapisywane w obszarze /LogFiles/DetailedErrors. 

**Śledzenie żądań nie powiodło się**. Dzienniki nie powiodło się żądania dostępu do plików XML. Pliki są zapisywane w obszarze /LogFiles/W3SVC*xxx*, gdzie xxx to unikatowy identyfikator. Ten folder zawiera pliku XSL oraz jeden lub więcej plików XML. Upewnij się, o pobranie pliku XSL, ponieważ zapewnia funkcje formatowania i filtrowania zawartości plików XML.

Aby wyświetlić pliki dziennika, należy najpierw utworzyć FTP poświadczeń, w następujący sposób:

1. W karta dla aplikacji sieci web kliknij pozycję **wszystkie ustawienia**.
2. Kliknij pozycję **wdrożenie poświadczeń**.
3. Wprowadź nazwę użytkownika i hasło.
4. Kliknij przycisk **Zapisz**.

![Ustaw poświadczenia wdrażania][configure03]

Pełna nazwa użytkownika FTP jest "app\username", której *aplikacji* jest nazwą aplikacji sieci web. Nazwa użytkownika jest wyświetlana w karta aplikacji sieci web, w obszarze **Essentials**.  

![Poświadczenia wdrożenia FTP][configure02]

## <a name="other-configuration-tasks"></a>Inne zadania konfiguracyjne

### <a name="ssl"></a>SSL 

W trybie Basic lub standardowe możesz przekazać certyfikatów SSL dla domeny niestandardowej. Aby uzyskać więcej informacji zobacz [Włącz protokół HTTPS dla aplikacji sieci web]. 

Aby wyświetlić przekazane certyfikatów kliknij pozycję **Wszystkie ustawienia** > **domen niestandardowych i SSL**.

### <a name="domain-names"></a>Nazwy domen

Dodaj nazwy domeny niestandardowej dla aplikacji sieci web. Aby uzyskać więcej informacji zobacz [Konfigurowanie niestandardowej nazwy domeny dla aplikacji sieci web w usłudze Azure aplikacji].

Aby wyświetlić nazwy domeny, kliknij polecenie **Wszystkie ustawienia** > **domen niestandardowych i SSL**.

### <a name="deployments"></a>Wdrożenia

- Konfigurowanie rozmieszczania ciągły. Zobacz [Przy użyciu cyfra do wdrażania aplikacji sieci Web w usłudze Azure aplikacji](./web-sites-deploy.md).
- Gniazda wdrożenia. Zobacz [wdrożyć do przemieszczania środowisk dla aplikacji sieci Web w usłudze Azure aplikacji].


Aby wyświetlić gniazda swojego wdrożenia, kliknij polecenie **Wszystkie ustawienia** > **gniazda wdrożenia**.

### <a name="monitoring"></a>Monitorowanie

W trybie Basic lub standardowy można sprawdzić dostępność HTTP lub HTTPS punkty końcowe z maksymalnie trzy geo distributed lokalizacji. Test monitorowania zakończy się niepowodzeniem, jeśli kod odpowiedzi protokołu HTTP jest błąd (4xx lub 5xx) lub odpowiedź trwa dłużej niż 30 sekund. Punkt końcowy jest traktowany jako dostępne, jeśli monitorowania testów działa z określonych lokalizacji. 

Aby uzyskać więcej informacji, zobacz [jak: monitorowanie stan punktu końcowego web].

>[AZURE.NOTE] Jeśli chcesz rozpocząć pracę z Azure aplikacji usługi przed utworzeniem konta dla konta Azure, przejdź do [Spróbuj aplikacji usługi], którym natychmiast można utworzyć aplikację sieci web krótkotrwałe starter w aplikacji usługi. Nie kart kredytowych wymagane; nie zobowiązania.

## <a name="next-steps"></a>Następne kroki

- [Konfigurowanie niestandardowej nazwy domeny w usłudze Azure aplikacji]
- [Włącz protokół HTTPS w aplikacji w usłudze Azure aplikacji]
- [Zmienianie aplikacji sieci web w usłudze Azure aplikacji]
- [Monitorowanie podstawy dla aplikacji sieci Web w usłudze Azure aplikacji]

<!-- URL List -->

[SignalR programu ASP.NET]: http://www.asp.net/signalr
[Azure Portal]: https://portal.azure.com/
[Konfigurowanie niestandardowej nazwy domeny w usłudze Azure aplikacji]: ./web-sites-custom-domain-name.md
[Wdrożyć do przemieszczania środowisk dla aplikacji sieci Web w usłudze Azure aplikacji]: ./web-sites-staged-publishing.md
[Włącz protokół HTTPS w aplikacji w usłudze Azure aplikacji]: ./web-sites-configure-ssl-certificate.md
[Jak: monitorowanie stan punktu końcowego sieci web]: http://go.microsoft.com/fwLink/?LinkID=279906
[Monitorowanie podstawy dla aplikacji sieci Web w usłudze Azure aplikacji]: ./web-sites-monitor.md
[Tryb potoku]: http://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture#Application
[Zmienianie aplikacji sieci web w usłudze Azure aplikacji]: ./web-sites-scale.md
[Socket.IO]: ./web-sites-nodejs-chat-app-socketio.md
[Spróbuj aplikacji usługi]: http://go.microsoft.com/fwlink/?LinkId=523751

<!-- IMG List -->

[configure01]: ./media/web-sites-configure/configure01.png
[configure02]: ./media/web-sites-configure/configure02.png
[configure03]: ./media/web-sites-configure/configure03.png
