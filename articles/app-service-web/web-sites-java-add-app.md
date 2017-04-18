<properties 
    pageTitle="Dodawanie aplikacji Java do Azure aplikacji usługi sieci Web" 
    description="Tym samouczku pokazano, jak dodać strony lub aplikacją wystąpienia Azure aplikacji usługi sieci Web aplikacji, która jest już skonfigurowany do używania języka Java." 
    services="app-service\web" 
    documentationCenter="java" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="add-a-java-application-to-azure-app-service-web-apps"></a>Dodawanie aplikacji Java do Azure aplikacji usługi sieci Web

Po Java aplikacji sieci web w [Usłudze Azure aplikacji][] już zainicjować opisane na [Tworzenie aplikacji sieci web języka Java w usłudze Azure aplikacji](web-sites-java-get-started.md), możesz przekazać aplikacji przez umieszczenie swojego też w folderze **Używanie** .

Ścieżka nawigacji do folderu **Używanie** różni się w zależności od sposobu skonfigurowania wystąpienia aplikacji sieci Web.

- Po skonfigurowaniu aplikacji sieci web przy użyciu usługi Azure Marketplace ścieżka do folderu **Używanie** ma postać **d:\home\site\wwwroot\bin\application\_server\webapps**, gdzie **aplikacji\_serwera** to nazwa serwera aplikacji w celu wystąpienia aplikacji sieci Web. 
- Po skonfigurowaniu aplikacji sieci web przy użyciu interfejsu użytkownika konfiguracji Azure ścieżka do folderu **Używanie** znajduje się w formularzu **d:\home\site\wwwroot\webapps**. 

Pamiętaj, aby przekazać aplikacji lub strony sieci web, w tym [scenariuszy integracji ciągły](app-service-continuous-deployment.md)umożliwiają kontrolki źródła. FTP jest również opcję przekazywanie aplikacji lub strony sieci web.

Uwaga w przypadku aplikacji sieci web Tomcat: po przekazaniu pliku też do folderu, **Używanie** , serwer aplikacji Tomcat wykryje dodano go, a następnie automatycznie pobierze go. Należy zauważyć, że po skopiowaniu plików (innych niż pliki też) do katalogu głównego serwera aplikacji zostanie należy ponownie uruchomić te pliki są używane. Funkcje autoload dla aplikacji sieci web Tomcat Java uruchomionych Azure jest oparty na pliku też dodawane, lub nowych plików lub katalogów dodawane do folderu **Używanie** . 

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji zobacz [Centrum deweloperów języka Java](/develop/java/).

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- External Links -->
[Azure aplikacji usługi]: http://go.microsoft.com/fwlink/?LinkId=529714
