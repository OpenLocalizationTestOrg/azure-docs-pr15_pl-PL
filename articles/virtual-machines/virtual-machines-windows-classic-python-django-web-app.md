<properties
    pageTitle="Python web app z Django | Microsoft Azure"
    description="Ten samouczek przedstawiono sposób udostępnić witrynę sieci Web opartych na Django Azure za pomocą maszyny wirtualnej systemu Windows Server 2012 R2 w centrum danych przy użyciu modelu Klasyczny wdrożenia."
    services="virtual-machines-windows"
    documentationCenter="python"
    authors="huguesv"
    manager="wpickett"
    editor=""
    tags="azure-service-management"/>


<tags 
    ms.service="virtual-machines-windows" 
    ms.workload="web" 
    ms.tgt_pltfrm="vm-windows" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="08/04/2015" 
    ms.author="huvalo"/>


# <a name="django-hello-world-web-application-on-a-windows-server-vm"></a>Django Witaj świecie aplikacji sieci web na maszyn wirtualnych systemu Windows Server

> [AZURE.SELECTOR]
- [Systemu Windows](virtual-machines-windows-classic-python-django-web-app.md)
- [Mac i Linux](virtual-machines-linux-python-django-web-app.md)

<br>

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]
 

Ten samouczek opisano, jak udostępnić witrynę sieci Web opartych na Django na Microsoft Azure za pomocą maszyny wirtualnej systemu Windows Server. Tego samouczka przyjęto założenie, że masz nie wcześniejszego doświadczenia w korzystaniu Azure. Po zakończeniu tego samouczka, konieczne będzie Django aplikacja w górę i uruchamiania w chmurze.

Dowiesz się jak:

* Konfigurowanie Azure maszyny wirtualnej hosta Django. Podczas tego samouczka wyjaśniono, jak to zrobić w systemie Windows Server, to można również wykonać za pomocą Linux maszyn wirtualnych obsługiwany w Azure.
* Tworzenie nowej aplikacji Django z systemu Windows.

Wykonując tego samouczka, zostanie utworzona prostych aplikacji sieci web Witaj świecie. Aplikacja będzie obsługiwany w Azure maszyn wirtualnych.

Zrzut ekranu przedstawiający złożonym aplikacji pojawi się dalej.

![Okno przeglądarki zawierające strony Witaj świecie Azure][1]

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## <a name="creating-and-configuring-an-azure-virtual-machine-to-host-django"></a>Tworzenie i konfigurowanie Azure maszyny wirtualnej hosta Django

1. Postępuj zgodnie z instrukcjami podane [tutaj](virtual-machines-windows-classic-tutorial.md) tworzenie Azure maszyn wirtualnych dystrybucji systemu Windows Server 2012 R2 w centrum danych.

1. Poinstruuj Azure w celu skierowania port 80 ruchu z sieci web do portu 80 maszyny wirtualnej:
 - Przejdź do komputera wirtualnych nowo utworzony w portalu klasyczny Azure, a następnie kliknij kartę **punktów końcowych** .
 - Kliknij przycisk **Dodaj** w dolnej części ekranu.
    ![Dodawanie punktu końcowego](./media/virtual-machines-windows-classic-python-django-web-app/django-helloworld-addendpoint.png)

 - Otwiera protokołu **TCP** przez **publicznej PORT 80** jako **prywatne PORT 80**.
![][port80]
1. Na karcie **pulpitu NAWIGACYJNEGO** kliknij pozycję **POŁĄCZ** z zdalnie zalogować się do nowo utworzonej maszyny wirtualnej Azure za pomocą **Pulpitu zdalnego** .  

**Ważna uwaga:** Wszystkie poniższe instrukcje założono, że użytkownik zalogowany do maszyny wirtualnej poprawnie i zostały wydane dostępne polecenia, a nie na komputerze lokalnym.

## <a id="setup"> </a>Instalowania Python, Django, WFastCGI

**Uwaga:** Aby można było pobrać przy użyciu programu Internet Explorer, być może trzeba skonfigurować ustawienia konfiguracji IE ESC (Start-administracyjnego narzędzia/serwer Menedżera/lokalnego serwera, następnie kliknij pozycję **Konfiguracja zwiększonych zabezpieczeń programu Internet Explorer**, wyłączone).

1. Zainstaluj najnowszą 2.7 Python lub 3.4 z [python.org][].
1. Instalowanie pakietów wfastcgi i django za pomocą pip.

    Dla 2.7 Python Użyj następującego polecenia.

        c:\python27\scripts\pip install wfastcgi
        c:\python27\scripts\pip install django

    Aby uzyskać 3.4 Python Użyj następującego polecenia.

        c:\python34\scripts\pip install wfastcgi
        c:\python34\scripts\pip install django

## <a name="installing-iis-with-fastcgi"></a>Instalowanie programu IIS z FastCGI

1. Instalowanie programu IIS z obsługą FastCGI.  To może potrwać kilka minut do wykonania.

        start /wait %windir%\System32\PkgMgr.exe /iu:IIS-WebServerRole;IIS-WebServer;IIS-CommonHttpFeatures;IIS-StaticContent;IIS-DefaultDocument;IIS-DirectoryBrowsing;IIS-HttpErrors;IIS-HealthAndDiagnostics;IIS-HttpLogging;IIS-LoggingLibraries;IIS-RequestMonitor;IIS-Security;IIS-RequestFiltering;IIS-HttpCompressionStatic;IIS-WebServerManagementTools;IIS-ManagementConsole;WAS-WindowsActivationService;WAS-ProcessModel;WAS-NetFxEnvironment;WAS-ConfigurationAPI;IIS-CGI

## <a name="creating-a-new-django-application"></a>Tworzenie nowej aplikacji Django

1.  Z *C:\inetpub\wwwroot*wpisz następujące polecenie, aby utworzyć nowy projekt Django:

    Dla 2.7 Python Użyj następującego polecenia.

        C:\Python27\Scripts\django-admin.exe startproject helloworld

    Aby uzyskać 3.4 Python Użyj następującego polecenia.

        C:\Python34\Scripts\django-admin.exe startproject helloworld

    ![Wynik polecenia Nowy AzureService](./media/virtual-machines-windows-classic-python-django-web-app/django-helloworld-cmd-new-azure-service.png)

1.  Polecenie **Administrator django** generuje podstawowej struktury witryn sieci Web opartych na Django:

  -   **helloworld\manage.PY** ułatwia rozpoczęcie hostingu i zatrzymywanie hostingu witryny sieci Web opartych na Django
  -   **helloworld\helloworld\settings.PY** zawiera Django ustawienia aplikacji.
  -   **helloworld\helloworld\urls.PY** zawiera kod mapowania między każdego adresu url i swoją opinię.

1.  Tworzenie nowego pliku o nazwie **views.py** w katalogu *C:\inetpub\wwwroot\helloworld\helloworld* . To będzie zawierać widoku są renderowane za pomocą strony "Witaj świecie". Uruchom Edytor, a następnie wprowadź następujące informacje:

        from django.http import HttpResponse
        def home(request):
            html = "<html><body>Hello World!</body></html>"
            return HttpResponse(html)

1.  Zamień zawartość pliku urls.py następujące czynności.

        from django.conf.urls import patterns, url
        urlpatterns = patterns('',
            url(r'^$', 'helloworld.views.home', name='home'),
        )

## <a name="configuring-iis"></a>Konfigurowanie usług IIS

1. Odblokowywanie sekcji Obsługa w globalnej applicationhost.config.  Spowoduje to włączenie stosowania obsługi python w swojej web.config.

        %windir%\system32\inetsrv\appcmd unlock config -section:system.webServer/handlers

1. Włącz WFastCGI.  Spowoduje to dodanie aplikacji do globalnej applicationhost.config, odwołująca się do interpretera Python wykonywalny i skrypt wfastcgi.py.

    Python 2.7:

        c:\python27\scripts\wfastcgi-enable

    Python 3.4:

        c:\python34\scripts\wfastcgi-enable

1. Tworzenie pliku web.config w *C:\inetpub\wwwroot\helloworld*.  Wartość `scriptProcessor` atrybut powinny być takie same dane wyjściowe w poprzednim kroku.  Zobacz stronę dla [wfastcgi][] na pypi, aby uzyskać więcej o ustawieniach wfastcgi.

    Python 2.7:

        <configuration>
          <appSettings>
            <add key="WSGI_HANDLER" value="django.core.handlers.wsgi.WSGIHandler()" />
            <add key="PYTHONPATH" value="C:\inetpub\wwwroot\helloworld" />
            <add key="DJANGO_SETTINGS_MODULE" value="helloworld.settings" />
          </appSettings>
          <system.webServer>
            <handlers>
                <add name="Python FastCGI" path="*" verb="*" modules="FastCgiModule" scriptProcessor="C:\Python27\python.exe|C:\Python27\Lib\site-packages\wfastcgi.pyc" resourceType="Unspecified" />
            </handlers>
          </system.webServer>
        </configuration>

    Python 3.4:

        <configuration>
          <appSettings>
            <add key="WSGI_HANDLER" value="django.core.handlers.wsgi.WSGIHandler()" />
            <add key="PYTHONPATH" value="C:\inetpub\wwwroot\helloworld" />
            <add key="DJANGO_SETTINGS_MODULE" value="helloworld.settings" />
          </appSettings>
          <system.webServer>
            <handlers>
                <add name="Python FastCGI" path="*" verb="*" modules="FastCgiModule" scriptProcessor="C:\Python34\python.exe|C:\Python34\Lib\site-packages\wfastcgi.py" resourceType="Unspecified" />
            </handlers>
          </system.webServer>
        </configuration>

1. Aktualizowanie lokalizacji programu IIS domyślna witryna sieci Web wskaż folder projektu django.

        %windir%\system32\inetsrv\appcmd set vdir "Default Web Site/" -physicalPath:"C:\inetpub\wwwroot\helloworld"

1. Na koniec ładowanie strony sieci web w przeglądarce.

![Okno przeglądarki zawierające strony Witaj świecie Azure][1]


## <a name="shutting-down-your-azure-virtual-machine"></a>Zamykanie usługi Azure maszyny wirtualnej

Po zakończeniu z tego samouczka, zamknij i/lub usuwanie maszyny wirtualnej nowo utworzonego Azure, aby zwolnić zasoby dla innych samouczków dotyczących i nie przyjmować Azure kosztów.

[1]: ./media/virtual-machines-windows-classic-python-django-web-app/django-helloworld-browser-azure.png

[port80]: ./media/virtual-machines-windows-classic-python-django-web-app/django-helloworld-port80.png

[Web Platform Installer]: http://www.microsoft.com/web/downloads/platform.aspx
[Python.org]: https://www.python.org/downloads/
[wfastcgi]: https://pypi.python.org/pypi/wfastcgi
