<properties 
    pageTitle="Przy użyciu konfiguracji PM2 dla NodeJS w aplikacjach sieci Web w systemie Linux | Microsoft Azure" 
    description="Przy użyciu konfiguracji PM2 dla NodeJS w aplikacjach sieci Web w systemie Linux" 
    keywords="Usługa Azure aplikacji, aplikacji sieci web, nodejs, pm2, linux, oss"
    services="app-service" 
    documentationCenter="" 
    authors="naziml" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016" 
    ms.author="naziml"/>

# <a name="using-pm2-configuration-for-nodejs-in-web-apps-on-linux"></a>Przy użyciu konfiguracji PM2 dla Node.js w aplikacjach sieci Web w systemie Linux

Jeśli ustawisz stos aplikacji Node.js dla aplikacji sieci Web na Linux, zostanie wyświetlony ustawić pliku uruchamiania Node.js, jak pokazano na poniższej ilustracji.

![][1]

Za pomocą tego dowolnego

-   Określanie skrypt uruchamiania aplikacji Node.js (na przykład: /bin/server.js)
-   Określanie PM2 plik konfiguracji dla aplikacji Node.js (na przykład: /foo/process.json)

 >[AZURE.NOTE] Jeśli chcesz procesami węzeł ponownie modyfikacji niektóre pliki, będzie konieczne używanie PM2 konfiguracji. W przeciwnym razie aplikacji nie uruchomi ponownie po odebraniu powiadomienia o zmianie od elementów takich jak ciągły wdrażania po zmianie kodzie aplikacji.

Można sprawdzić Node.js [dokumentacji pliku procesów](http://pm2.keymetrics.io/docs/usage/application-declaration/) wszystkie opcje, ale poniżej znajduje się przykład co chcesz używać jako plik process.json

        {
          "name"        : "worker",
          "script"      : "/bin/server.js",
          "instances"   : 1,
          "merge_logs"  : true,
          "log_date_format" : "YYYY-MM-DD HH:mm Z",
          "watch": ["/bin/server.js", "foo.txt"],
          "watch_options": {
            "followSymlinks": true,
            "usePolling"   : true,
            "interval"    : 5
          }
        }

Są ważnych uwag w tej konfiguracji 

-   Właściwość "skrypt" Określa skrypt start aplikacji.
-   Właściwość "wystąpienia" Określa, ile wystąpień uruchamianie procesu węzeł. Jeśli używasz aplikacji na większych rozmiarów maszyn wirtualnych, zawierających wiele rdzeni, chcesz zmaksymalizować zasobów, ustawiając wartość większą tutaj.
-   Tablica "aktor" Określa wszystkie pliki, których zmiany chcesz ponownie uruchomić procesami węzeł.
-   Dla "watch_options" obecnie należy określić "usePolling" jako true ze względu na sposób zawartości aplikacji jest zainstalowany.


## <a name="next-steps"></a>Następne kroki ##

* [Co to jest usługa aplikacji w systemie Linux?](./app-service-linux-intro.md)

<!--Image references-->
[1]: ./media/app-service-linux-using-nodejs-pm2/nodejs-startup-file.png