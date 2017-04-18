<properties
    pageTitle="Azure aplikacji usługi aplikacji web app zaawansowane konfiguracji i rozszerzenia"
    description="Aby przekształcić pliku ApplicationHost.config w aplikacji sieci web Azure aplikacji usługi i dodawanie prywatne rozszerzenia umożliwiające akcje niestandardowe administracji za pomocą deklaracji Transformation(XDT) dokumentu XML."
    authors="cephalin"
    writer="cephalin"
    editor="mollybos"
    manager="wpickett"
    services="app-service"
    documentationCenter=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/25/2016"
    ms.author="cephalin"/>

# <a name="azure-app-service-web-app-advanced-config-and-extensions"></a>Azure aplikacji usługi aplikacji web app zaawansowane konfiguracji i rozszerzenia

Za pomocą deklaracji [Przekształcenie dokumentu XML](http://msdn.microsoft.com/library/dd465326.aspx) (XDT), można przekształcić pliku [ApplicationHost.config](http://www.iis.net/learn/get-started/planning-your-iis-architecture/introduction-to-applicationhostconfig) w aplikacji sieci web w usłudze Azure aplikacji. Za pomocą deklaracji XDT Dodawanie prywatne rozszerzenia umożliwiające akcje administracji aplikacji sieci web niestandardowe. Ten artykuł zawiera rozszerzenia aplikacji sieci web Menedżera PHP próbki, które umożliwia zarządzanie ustawieniami PHP za pośrednictwem interfejsu sieci web.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

##<a id="transform"></a>Konfiguracja zaawansowana za pośrednictwem ApplicationHost.config
Platformy aplikacji usługi zapewnia elastyczność i kontrolę konfiguracji aplikacji sieci web. Mimo że standardowy plik konfiguracji ApplicationHost.config programu IIS nie jest dostępny do edycji bezpośrednio w aplikacji usługi, platformy obsługuje deklaracyjnych model przekształcenie ApplicationHost.config oparte na przemian dokumentu XML (XDT).

Aby korzystać z tej funkcji przekształcenie, możesz utworzyć plik ApplicationHost.xdt z zawartością XDT i miejsce, w obszarze katalogu głównego (d:\home\site) w [Konsoli Kudu](https://github.com/projectkudu/kudu/wiki/Kudu-console). Może być konieczne ponowne uruchomienie aplikacji sieci Web Aby zmiany zostały wprowadzone.

W poniższym przykładzie applicationHost.xdt pokazano, jak dodać nową zmienną środowiska niestandardowych do aplikacji sieci web, która używa PHP 5.4.

    <?xml version="1.0"?>
    <configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
        <system.webServer>
                <fastCgi>
                    <application>
                        <environmentVariables>
                                <environmentVariable name="CONFIGTEST" value="TEST" xdt:Transform="Insert" xdt:Locator="XPath(/configuration/system.webServer/fastCgi/application[contains(@fullPath,'5.4')]/environmentVariables)" />
                        </environmentVariables>
                    </application>
                </fastCgi>
        </system.webServer>
    </configuration>


Plik dziennika ze stanem przekształcenie i szczegóły jest dostępna z głównego FTP w obszarze LogFiles\Transform.

Aby uzyskać dodatkowe próbki zobacz [https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples](https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples).

**Uwaga**<br />
Elementy na liście modułów w obszarze `system.webServer` nie można usunąć lub zamówić ponownie, ale możliwe są dodanych do listy.


##<a id="extend"></a>Rozszerzanie aplikacji sieci web

###<a id="overview"></a>Omówienie rozszerzenia aplikacji sieci web prywatnych

Usługa aplikacji obsługuje rozszerzenia aplikacji sieci web jako punkt rozszerzeń dla działań administracyjnych. Niektóre funkcje platformy aplikacji usługi są w rzeczywistości wykonywane jako zainstalowany rozszerzenia. Gdy nie można zmodyfikować rozszerzenia platformy zainstalowany, można utworzyć i skonfigurować rozszerzenia prywatnych dla aplikacji sieci web. Ta funkcja również jest zależna od XDT deklaracji. Podstawowe etapy tworzenia rozszerzenia aplikacji sieci web prywatne są następujące:

1. Rozszerzenie aplikacji **zawartości**sieci Web: tworzenie dowolnej aplikacji sieci web obsługiwanych przez aplikacji usługi
2. Rozszerzenie aplikacji **deklaracji**w sieci Web: Tworzenie pliku ApplicationHost.xdt
3. Rozszerzenie aplikacji **wdrożenia**w sieci Web: umieszczanie zawartości w folderze SiteExtensions`root`

Łącza wewnętrzne dla aplikacji sieci web powinny wskazywać ścieżkę względem ścieżka aplikacji określone w pliku ApplicationHost.xdt. Zmiany w pliku ApplicationHost.xdt wymaga odtworzenia aplikacji sieci web.

**Uwaga**: dodatkowe informacje dotyczące tych najważniejszych elementów jest dostępna w [https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions](https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions).

Aby przedstawić instrukcje dotyczące tworzenia i włączanie rozszerzenia aplikacji sieci web prywatnych znajduje się szczegółowy przykład. Kod źródłowy, na przykład Menedżer PHP, znajdujący się można pobrać z [https://github.com/projectkudu/PHPManager](https://github.com/projectkudu/PHPManager).

###<a id="SiteSample"></a>Przykład rozszerzenia aplikacji sieci Web: Menedżer PHP

Menedżer PHP jest rozszerzenia aplikacji sieci web, które umożliwia web app administratorom łatwe wyświetlanie i konfigurowanie ustawień PHP, bez konieczności modyfikowania plików .ini PHP bezpośrednio przy użyciu interfejsu sieci web. Typowe pliki konfiguracji PHP Dołączanie pliku php.ini znajdującego się w obszarze pliki programów i. user.ini pliku znajdującego się w folderze głównym aplikacji sieci web. Ponieważ plik php.ini nie jest edytowalny bezpośrednio na platformie aplikacji usługi, rozszerzenie Menedżera PHP używa. plik user.ini, aby zastosować zmiany ustawień.

####<a id="PHPwebapp"></a>Menedżer PHP aplikacji sieci web

Strona główna wdrożenie Menedżera PHP jest następująca:

![TransformSitePHPUI][TransformSitePHPUI]

Jak widać, rozszerzenia aplikacji sieci web jest tak jak aplikacji sieci web zwykła, ale z plikiem ApplicationHost.xdt dodatkowe umieszczony w folderze głównym aplikacji sieci web (szczegółowe informacje o pliku ApplicationHost.xdt są dostępne w następnej sekcji tego artykułu).

Rozszerzenie Menedżera PHP został utworzony przy użyciu szablonu Visual Studio ASP.NET MVC 4 przechowywania. Poniższa ilustracja z Eksplorator rozwiązań przedstawia strukturę rozszerzenie Menedżera PHP.

![TransformSiteSolEx][TransformSiteSolEx]

Tylko specjalną logikę potrzebną do pliku we/wy należy wskazać miejsce, w którym znajduje się w katalogu wwwroot aplikacji sieci web. Jak w poniższym przykładzie pokazano, zmiennych środowiska "główne" oznacza ścieżkę katalogu głównego aplikacji sieci web, a ścieżka wwwroot może być skonstruowane za pomocą dołączania "site\wwwroot":

    /// <summary>
    /// Gives the location of the .user.ini file, even if one doesn't exist yet
    /// </summary>
    private static string GetUserSettingsFilePath()
    {
            var rootPath = Environment.GetEnvironmentVariable("HOME"); // For use on Azure Websites
            if (rootPath == null)
            {
                rootPath = System.IO.Path.GetTempPath(); // For testing purposes
            };
            var userSettingsFile = Path.Combine(rootPath, @"site\wwwroot\.user.ini");
            return userSettingsFile;
    }


Po umieszczeniu ścieżkę katalogu, w operacji We/Wy zwykły plik umożliwia odczytywanie i zapisywanie plików.

Jeden punkt ostrzeżenia z rozszerzenia aplikacji sieci web odniesieniu do obsługi łącza wewnętrzne.  Jeśli masz łączach w plikach HTML, które powodują ścieżki bezwzględne łącza wewnętrzne w aplikacji sieci web, musisz zapewnić, że te łącza są poprzedzony przez nazwę rozszerzenia jako główną. Jest to potrzebne, ponieważ głównym numer wewnętrzny jest teraz "-`[your-extension-name]`-" nie tylko "/", więc każdym wewnętrznych łącza muszą być aktualizowane odpowiednio. Załóżmy na przykład, że kod zawiera łącza do następujących czynności:

`"<a href="/Home/Settings">PHP Settings</a>"`

Jeśli łącze stanowi część rozszerzenia aplikacji sieci web, łącze musi być w następującym formacie:

`"<a href="/[your-site-name]/Home/Settings">Settings</a>"`

To wymaganie można obejść przez jeden tylko ścieżki względne w aplikacji sieci web lub w przypadku aplikacji ASP.NET przy użyciu `@Html.ActionLink` metody, który tworzy odpowiednie łącza dla Ciebie.

####<a id="XDT"></a>Plik applicationHost.xdt

Kod rozszerzenia aplikacji sieci web przechodzi w obszarze %HOME%\SiteExtensions\[Twoja nazwa rozszerzenia]. Będzie nazywamy to katalog główny rozszerzenia.  

Aby zarejestrować rozszerzenia aplikacji sieci web z plikiem applicationHost.config, należy umieścić w pliku o nazwie ApplicationHost.xdt w katalogu głównym rozszerzenia. Zawartość pliku ApplicationHost.xdt powinna wyglądać następująco:

    <?xml version="1.0"?>
    <configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
        <system.applicationHost>
                <sites>
                    <site name="%XDT_SCMSITENAME%" xdt:Locator="Match(name)">
                        <!-- NOTE: Add your extension name in the application paths below -->
                        <application path="/[your-extension-name]" xdt:Locator="Match(path)" xdt:Transform="Remove" />
                        <application path="/[your-extension-name]" applicationPool="%XDT_APPPOOLNAME%" xdt:Transform="Insert">
                            <virtualDirectory path="/" physicalPath="%XDT_EXTENSIONPATH%" />
                        </application>
                    </site>
                </sites>
        </system.applicationHost>
    </configuration>

Nazwa wybranego jako nazwę rozszerzenia powinien mieć taką samą nazwę jak folderem rozszerzenia.

Efektem dodawania nowych ścieżka aplikacji do `system.applicationHost` listy witryn w witrynie Menedżer sterowania usługami. Witryna Menedżer sterowania usługami jest punkt końcowy Administracja witryny przy użyciu poświadczeń określonego programu access. Ma adres URL `https://[your-site-name].scm.azurewebsites.net`.  

    <system.applicationHost>
        ...
        <site name="~1[your-website]" id="1716402716">
                <bindings>
                    <binding protocol="http" bindingInformation="*:80: [your-website].scm.azurewebsites.net" />
                    <binding protocol="https" bindingInformation="*:443: [your-website].scm.azurewebsites.net" />
                </bindings>
                <traceFailedRequestsLogging enabled="false" directory="C:\DWASFiles\Sites\[your-website]\VirtualDirectory0\LogFiles" />
                <detailedErrorLogging enabled="false" directory="C:\DWASFiles\Sites\[your-website]\VirtualDirectory0\LogFiles\DetailedErrors" />
                <logFile logSiteId="false" />
                <application path="/" applicationPool="[your-website]">
                    <virtualDirectory path="/" physicalPath="D:\Program Files (x86)\SiteExtensions\Kudu\1.24.20926.5" />
                </application>
                <!-- Note the custom changes that go here -->
                <application path="/[your-extension-name]" applicationPool="[your-website]">
                    <virtualDirectory path="/" physicalPath="C:\DWASFiles\Sites\[your-website]\VirtualDirectory0\SiteExtensions\[your-extension-name]" />
                </application>
            </site>
    </sites>
      ...
    </system.applicationHost>

###<a id="deploy"></a>Wdrażanie rozszerzenia aplikacji sieci Web

Aby zainstalować rozszerzenia aplikacji sieci web, należy użyć FTP do skopiowania wszystkich plików aplikacji sieci web, aby `\SiteExtensions\[your-extension-name]` folderu aplikacji sieci web, na którym chcesz zainstalować rozszerzenia.  Pamiętaj skopiować plik ApplicationHost.xdt do tej lokalizacji, a także. Ponowne uruchamianie aplikacji sieci web, aby włączyć rozszerzenie.

Powinny być widoczne rozszerzenia aplikacji sieci web na:

`https://[your-site-name].scm.azurewebsites.net/[your-extension-name]`

Zauważ, że adres URL wygląda tak samo jak adres URL dla aplikacji sieci web, z wyjątkiem, że korzysta HTTPS i zawiera ".scm".

Istnieje możliwość wyłączyć wszystkie prywatne rozszerzenia (nie zainstalowany) dla aplikacji sieci web podczas badania i rozwój, dodając ustawień aplikacji przy użyciu klucza `WEBSITE_PRIVATE_EXTENSIONS` i wartości `0`.

>[AZURE.NOTE] Jeśli chcesz rozpocząć pracę z Azure aplikacji usługi przed utworzeniem konta dla konta Azure, przejdź do [Spróbuj aplikacji usługi](http://go.microsoft.com/fwlink/?LinkId=523751), którym natychmiast można utworzyć aplikację sieci web krótkotrwałe starter w aplikacji usługi. Nie kart kredytowych wymagane; nie zobowiązania.

## <a name="whats-changed"></a>Informacje o zmianach
* Przewodnika do zmiany z witryn sieci Web do usługi aplikacji Zobacz: [Usługa Azure aplikacji i jego wpływ na istniejące usługi Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- IMAGES -->
[TransformSitePHPUI]: ./media/web-sites-transform-extend/TransformSitePHPUI.png
[TransformSiteSolEx]: ./media/web-sites-transform-extend/TransformSiteSolEx.png
 
