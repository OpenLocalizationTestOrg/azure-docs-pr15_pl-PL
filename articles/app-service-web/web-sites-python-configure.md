<properties 
    pageTitle="Konfigurowanie Python za pomocą aplikacji sieci Web Azure aplikacji usługi" 
    description="Ten samouczek w tym artykule opisano opcje dotyczące tworzenia i konfigurowania podstawowy serwer sieci Web zgodnego z aplikacji Python interfejs bramy (WSGI) na Azure aplikacji usługi sieci Web." 
    services="app-service" 
    documentationCenter="python" 
    tags="python"
    authors="huguesv" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="02/26/2016" 
    ms.author="huvalo"/>




# <a name="configuring-python-with-azure-app-service-web-apps"></a>Konfigurowanie Python za pomocą aplikacji sieci Web Azure aplikacji usługi

Ten samouczek w tym artykule opisano opcje dotyczące tworzenia i konfigurowania zgłoszenia podstawowego Python zgodny z sieci Web serwera bramy Interface (WSGI) na [Azure aplikacji usługi sieci Web](http://go.microsoft.com/fwlink/?LinkId=529714).

Opisuje dodatkowe funkcje wdrażania cyfra, takie jak środowisko wirtualne i instalacja pakietu przy użyciu requirements.txt.


## <a name="bottle-django-or-flask"></a>Butelki Django lub kolby?

Azure Marketplace zawiera szablony RAM butelki, Django i kolby. Jeśli tworzysz pierwszej aplikacji sieci web w usłudze Azure aplikacji lub nie znasz cyfra, zaleca się wykonaj jedną z następujących samouczki, które zawierają instrukcje krok po kroku dotyczące tworzenia aplikację roboczych z Galerii za pomocą cyfra z systemem Windows lub komputerów Mac:

- [Tworzenie aplikacji sieci web za pomocą butelki](web-sites-python-create-deploy-bottle-app.md)
- [Tworzenie aplikacji sieci web za pomocą Django](web-sites-python-create-deploy-django-app.md)
- [Tworzenie aplikacji sieci web z kolbą](web-sites-python-create-deploy-flask-app.md)


## <a name="web-app-creation-on-azure-portal"></a>Tworzenie aplikacji sieci Web na Azure Portal

Tego samouczka przyjęto założenie istniejącej subskrypcji Azure i dostęp do Azure Portal.

Jeśli nie masz istniejącej aplikacji sieci web, możesz utworzyć jeden z [Azure Portal](https://portal.azure.com).  Kliknij przycisk Nowy w lewym górnym rogu, a następnie kliknij pozycję **Web + Mobile** > **aplikacji sieci Web**.

## <a name="git-publishing"></a>Publikowanie cyfra

Konfigurowanie publikowania cyfra dla aplikacji sieci web nowo utworzonego zgodnie z instrukcjami w [Lokalnym rozmieszczania cyfra Azure aplikacji usługi](app-service-deploy-local-git.md). Samouczku cyfra umożliwia tworzenie i publikowanie aplikacji web app naszych Python Azure aplikacji usługi Zarządzanie.

Po skonfigurowaniu publikowania cyfra repozytorium cyfra zostanie utworzona i skojarzone z aplikacji sieci web. Adres URL z repozytorium będą wyświetlane i odtąd można używać do umieszczenia danych ze środowiska projektowego lokalnych w chmurze. Publikowanie aplikacji za pomocą cyfra, upewnij się, że zainstalowany jest także klient cyfra i skorzystać z instrukcji podanych przesunąć zawartości aplikacji sieci web do usługi aplikacji Azure.


## <a name="application-overview"></a>Omówienie aplikacji

W następnej sekcji są tworzone następujące pliki. Powinny one znajdować się w katalogu głównym repozytorium cyfra.

    app.py
    requirements.txt
    runtime.txt
    web.config
    ptvs_virtualenv_proxy.py


## <a name="wsgi-handler"></a>Obsługa WSGI

WSGI to standard Python opisanego przez [program ten 3333](http://www.python.org/dev/peps/pep-3333/) Definiowanie interfejsu między serwerem sieci web i Python. Umożliwia standardowym interfejsu pomocne przy tworzeniu różnych aplikacji sieci web i za pomocą Python RAM. Popularne RAM web Python dzisiaj za pomocą WSGI. Azure umożliwia aplikacji sieci Web usługi pomocy technicznej dla tych ram; Ponadto zaawansowanych użytkowników można nawet tworzyć własne jak niestandardowe Obsługa następuje wskazówki Specyfikacja WSGI.

Oto przykład `app.py` definiuje niestandardowego programu obsługi:

    def wsgi_app(environ, start_response):
        status = '200 OK'
        response_headers = [('Content-type', 'text/plain')]
        start_response(status, response_headers)
        response_body = 'Hello World'
        yield response_body.encode()

    if __name__ == '__main__':
        from wsgiref.simple_server import make_server

        httpd = make_server('localhost', 5555, wsgi_app)
        httpd.serve_forever()

Można uruchomić tę aplikację lokalnie z `python app.py`, następnie przejdź do `http://localhost:5555` w przeglądarce sieci web.


## <a name="virtual-environment"></a>Środowisko wirtualne

Mimo że aplikacja przykład powyżej nie wymaga wszystkie pakiety zewnętrznych, prawdopodobnie aplikacja będzie wymagać niektóre.

Aby ułatwić zarządzanie zależności zewnętrznych pakietów, wdrażania cyfra Azure obsługuje tworzenie wirtualnych środowisk.

Gdy Azure wykryje requirements.txt w katalogu głównym repozytorium, automatycznie tworzy środowisko wirtualne o nazwie `env`. Takim wdrożenia pierwszego lub podczas każdego wdrożenia od wybranego Python środowisko uruchomieniowe uległa zmianie.

Prawdopodobnie zechcesz utworzyć wirtualny środowisko lokalnie w celu rozwoju, ale nie zawierają w repozytorium cyfra.


## <a name="package-management"></a>Pakiet zarządzania

Pakiety wymienione w requirements.txt zostanie zainstalowany automatycznie w środowisku wirtualnych przy użyciu pip. W takim przypadku każdej wdrożenia, ale pip pominie instalacji, jeśli jest już zainstalowany pakiet.

Przykład `requirements.txt`:

    azure==0.8.4


## <a name="python-version"></a>Wersja Python

[AZURE.INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-runtime.md)]

Przykład `runtime.txt`:

    python-2.7


## <a name="webconfig"></a>Web.config

Musisz utworzyć plik web.config, aby określić sposób obsługi wezwań na serwerze.

Należy zauważyć, że jeśli plik web.x.y.config w repozytorium, gdzie x.y pasuje do wybranego runtime Python, a następnie Azure automatycznie skopiuje odpowiedni plik jako web.config.

W poniższych przykładach web.config zależeć skryptu serwera proxy wirtualnego środowiska, które zostało opisane w następnej sekcji.  Działają w systemie obsługi WSGI w przykładzie użyto `app.py` powyżej.

Przykład `web.config` dla Python 2.7:

    <?xml version="1.0"?>
    <configuration>
      <appSettings>
        <add key="WSGI_ALT_VIRTUALENV_HANDLER" value="app.wsgi_app" />
        <add key="WSGI_ALT_VIRTUALENV_ACTIVATE_THIS"
             value="D:\home\site\wwwroot\env\Scripts\activate_this.py" />
        <add key="WSGI_HANDLER"
             value="ptvs_virtualenv_proxy.get_virtualenv_handler()" />
        <add key="PYTHONPATH" value="D:\home\site\wwwroot" />
      </appSettings>
      <system.web>
        <compilation debug="true" targetFramework="4.0" />
      </system.web>
      <system.webServer>
        <modules runAllManagedModulesForAllRequests="true" />
        <handlers>
          <remove name="Python27_via_FastCGI" />
          <remove name="Python34_via_FastCGI" />
          <add name="Python FastCGI"
               path="handler.fcgi"
               verb="*"
               modules="FastCgiModule"
               scriptProcessor="D:\Python27\python.exe|D:\Python27\Scripts\wfastcgi.py"
               resourceType="Unspecified"
               requireAccess="Script" />
        </handlers>
        <rewrite>
          <rules>
            <rule name="Static Files" stopProcessing="true">
              <conditions>
                <add input="true" pattern="false" />
              </conditions>
            </rule>
            <rule name="Configure Python" stopProcessing="true">
              <match url="(.*)" ignoreCase="false" />
              <conditions>
                <add input="{REQUEST_URI}" pattern="^/static/.*" ignoreCase="true" negate="true" />
              </conditions>
              <action type="Rewrite"
                      url="handler.fcgi/{R:1}"
                      appendQueryString="true" />
            </rule>
          </rules>
        </rewrite>
      </system.webServer>
    </configuration>


Przykład `web.config` dla Python 3.4:

    <?xml version="1.0"?>
    <configuration>
      <appSettings>
        <add key="WSGI_ALT_VIRTUALENV_HANDLER" value="app.wsgi_app" />
        <add key="WSGI_ALT_VIRTUALENV_ACTIVATE_THIS"
             value="D:\home\site\wwwroot\env\Scripts\python.exe" />
        <add key="WSGI_HANDLER"
             value="ptvs_virtualenv_proxy.get_venv_handler()" />
        <add key="PYTHONPATH" value="D:\home\site\wwwroot" />
      </appSettings>
      <system.web>
        <compilation debug="true" targetFramework="4.0" />
      </system.web>
      <system.webServer>
        <modules runAllManagedModulesForAllRequests="true" />
        <handlers>
          <remove name="Python27_via_FastCGI" />
          <remove name="Python34_via_FastCGI" />
          <add name="Python FastCGI"
               path="handler.fcgi"
               verb="*"
               modules="FastCgiModule"
               scriptProcessor="D:\Python34\python.exe|D:\Python34\Scripts\wfastcgi.py"
               resourceType="Unspecified"
               requireAccess="Script" />
        </handlers>
        <rewrite>
          <rules>
            <rule name="Static Files" stopProcessing="true">
              <conditions>
                <add input="true" pattern="false" />
              </conditions>
            </rule>
            <rule name="Configure Python" stopProcessing="true">
              <match url="(.*)" ignoreCase="false" />
              <conditions>
                <add input="{REQUEST_URI}" pattern="^/static/.*" ignoreCase="true" negate="true" />
              </conditions>
              <action type="Rewrite" url="handler.fcgi/{R:1}" appendQueryString="true" />
            </rule>
          </rules>
        </rewrite>
      </system.webServer>
    </configuration>


Pliki statyczne będą obsługiwane przez serwer sieci web bezpośrednio, bez pośrednictwa kod Python, aby poprawić wydajność.

W powyższym przykładzie lokalizacji plików statycznych na dysku powinny być zgodne lokalizacji w adresie URL. Oznacza to, że na żądanie `http://pythonapp.azurewebsites.net/static/site.css` posłużą plik na dysku na `\static\site.css`.

`WSGI_ALT_VIRTUALENV_HANDLER`to, w którym możesz określić obsługi WSGI. W powyższym przykładzie ma `app.wsgi_app` ponieważ obsługi jest funkcja o nazwie `wsgi_app` w `app.py` w folderze głównym.

`PYTHONPATH`można dostosować, ale po zainstalowaniu wszystkich zależności między w środowisku wirtualnych, określając je w requirements.txt, nie należy go zmienić.


## <a name="virtual-environment-proxy"></a>Środowisko wirtualne serwera Proxy

Poniższy skrypt jest używany do pobierania obsługi WSGI, Aktywuj wirtualnego środowiska i dziennika błędów. Jest przeznaczony do ogólnego i używanych bez zmian.

Zawartość `ptvs_virtualenv_proxy.py`:

     # ############################################################################
     #
     # Copyright (c) Microsoft Corporation. 
     #
     # This source code is subject to terms and conditions of the Apache License, Version 2.0. A 
     # copy of the license can be found in the License.html file at the root of this distribution. If 
     # you cannot locate the Apache License, Version 2.0, please send an email to 
     # vspython@microsoft.com. By using this source code in any fashion, you are agreeing to be bound 
     # by the terms of the Apache License, Version 2.0.
     #
     # You must not remove this notice, or any other, from this software.
     #
     # ###########################################################################

    import datetime
    import os
    import sys
    import traceback

    if sys.version_info[0] == 3:
        def to_str(value):
            return value.decode(sys.getfilesystemencoding())

        def execfile(path, global_dict):
            """Execute a file"""
            with open(path, 'r') as f:
                code = f.read()
            code = code.replace('\r\n', '\n') + '\n'
            exec(code, global_dict)
    else:
        def to_str(value):
            return value.encode(sys.getfilesystemencoding())

    def log(txt):
        """Logs fatal errors to a log file if WSGI_LOG env var is defined"""
        log_file = os.environ.get('WSGI_LOG')
        if log_file:
            f = open(log_file, 'a+')
            try:
                f.write('%s: %s' % (datetime.datetime.now(), txt))
            finally:
                f.close()

    ptvsd_secret = os.getenv('WSGI_PTVSD_SECRET')
    if ptvsd_secret:
        log('Enabling ptvsd ...\n')
        try:
            import ptvsd
            try:
                ptvsd.enable_attach(ptvsd_secret)
                log('ptvsd enabled.\n')
            except: 
                log('ptvsd.enable_attach failed\n')
        except ImportError:
            log('error importing ptvsd.\n');

    def get_wsgi_handler(handler_name):
        if not handler_name:
            raise Exception('WSGI_ALT_VIRTUALENV_HANDLER env var must be set')
    
        if not isinstance(handler_name, str):
            handler_name = to_str(handler_name)
    
        module_name, _, callable_name = handler_name.rpartition('.')
        should_call = callable_name.endswith('()')
        callable_name = callable_name[:-2] if should_call else callable_name
        name_list = [(callable_name, should_call)]
        handler = None
        last_tb = ''

        while module_name:
            try:
                handler = __import__(module_name, fromlist=[name_list[0][0]])
                last_tb = ''
                for name, should_call in name_list:
                    handler = getattr(handler, name)
                    if should_call:
                        handler = handler()
                break
            except ImportError:
                module_name, _, callable_name = module_name.rpartition('.')
                should_call = callable_name.endswith('()')
                callable_name = callable_name[:-2] if should_call else callable_name
                name_list.insert(0, (callable_name, should_call))
                handler = None
                last_tb = ': ' + traceback.format_exc()
    
        if handler is None:
            raise ValueError('"%s" could not be imported%s' % (handler_name, last_tb))
    
        return handler

    activate_this = os.getenv('WSGI_ALT_VIRTUALENV_ACTIVATE_THIS')
    if not activate_this:
        raise Exception('WSGI_ALT_VIRTUALENV_ACTIVATE_THIS is not set')

    def get_virtualenv_handler():
        log('Activating virtualenv with %s\n' % activate_this)
        execfile(activate_this, dict(__file__=activate_this))

        log('Getting handler %s\n' % os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        handler = get_wsgi_handler(os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        log('Got handler: %r\n' % handler)
        return handler

    def get_venv_handler():
        log('Activating venv with executable at %s\n' % activate_this)
        import site
        sys.executable = activate_this
        old_sys_path, sys.path = sys.path, []
    
        site.main()
    
        sys.path.insert(0, '')
        for item in old_sys_path:
            if item not in sys.path:
                sys.path.append(item)

        log('Getting handler %s\n' % os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        handler = get_wsgi_handler(os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        log('Got handler: %r\n' % handler)
        return handler


## <a name="customize-git-deployment"></a>Dostosowywanie wdrożenia cyfra

[AZURE.INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-deployment.md)]


## <a name="troubleshooting---package-installation"></a>Rozwiązywanie problemów — instalacji pakietu

[AZURE.INCLUDE [web-sites-python-troubleshooting-package-installation](../../includes/web-sites-python-troubleshooting-package-installation.md)]


## <a name="troubleshooting---virtual-environment"></a>Rozwiązywanie problemów — środowisko wirtualne

[AZURE.INCLUDE [web-sites-python-troubleshooting-virtual-environment](../../includes/web-sites-python-troubleshooting-virtual-environment.md)]

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji zobacz [Centrum deweloperów Python](/develop/python/).

>[AZURE.NOTE] Jeśli chcesz rozpocząć pracę z Azure aplikacji usługi przed utworzeniem konta dla konta Azure, przejdź do [Spróbuj aplikacji usługi](http://go.microsoft.com/fwlink/?LinkId=523751), którym natychmiast można utworzyć aplikację sieci web krótkotrwałe starter w aplikacji usługi. Nie kart kredytowych wymagane; nie zobowiązania.

## <a name="whats-changed"></a>Informacje o zmianach
* Przewodnika do zmiany z witryn sieci Web do usługi aplikacji Zobacz: [Usługa Azure aplikacji i jego wpływ na istniejące usługi Azure](http://go.microsoft.com/fwlink/?LinkId=529714)





 
