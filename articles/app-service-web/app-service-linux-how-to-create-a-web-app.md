<properties 
    pageTitle="Jak utworzyć aplikację sieci Web przy użyciu aplikacji usługi w systemie Linux | Microsoft Azure" 
    description="Sieci Web app tworzenia przepływu pracy aplikacji usługi w systemie Linux." 
    keywords="Usługa Azure aplikacji, aplikacji sieci web, linux, oss"
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

# <a name="create-a-web-app-with-app-service-on-linux"></a>Tworzenie aplikacji sieci Web przy użyciu aplikacji usługi w systemie Linux

## <a name="using-the-management-portal-to-create-your-web-app"></a>Tworzenie aplikacji sieci web za pomocą portalu zarządzania
Możesz rozpocząć tworzenie aplikacji sieci Web w systemie Linux w [portalu zarządzania](https://portal.azure.com) , jak pokazano na poniższej ilustracji.

![][1]

Po wybraniu opcji poniżej, zostaną wyświetlone karta tworzenie jak pokazano na poniższej ilustracji. 

![][2]

-   Nadaj nazwę aplikacji sieci web.
-   Wybieranie istniejącej grupy zasobów lub Utwórz nowy. (Zobacz regionów dostępnych w [sekcji ograniczeń](./app-service-linux-intro.md)).
-   Wybierz istniejący plan usługi aplikacji lub Utwórz nową jedną (zobacz aplikacji usługi plan notatki w [sekcji ograniczeń](./app-service-linux-intro.md)). 
-   Wybierz pozycję stos aplikacji, które mają być używane. Zostanie wyświetlony Wybieranie między różnymi wersjami programu Node.js i PHP. 

Gdy aplikacja utworzona, można zmienić stos aplikacji z ustawienia aplikacji, jak pokazano na poniższej ilustracji.

![][3]

## <a name="deploying-your-web-app"></a>Wdrażanie aplikacji sieci web

Wybieranie "Opcje wdrażania" za pomocą portalu zarządzania udostępnia opcję za pomocą repozytorium cyfra lub repozytorium GitHub lokalnego wdrażania aplikacji. Instrukcje później są podobnie do aplikacji sieci web nie Linux i można postępuj zgodnie z instrukcjami w naszym [lokalnego wdrożenia cyfra](./app-service-deploy-local-git.md) lub naszą artykuł [ciągły wdrożenia](./app-service-continuous-deployment.md) GitHub.

Za pomocą FTP przekazywanie aplikacji do witryny. Punkt końcowy FTP dla aplikacji sieci web można uzyskać z sekcji dzienniki diagnostyczne, jak pokazano na poniższej ilustracji.

![][4]


## <a name="next-steps"></a>Następne kroki ##

* [Co to jest usługa aplikacji w systemie Linux?](./app-service-linux-intro.md)
* [Przy użyciu konfiguracji PM2 dla Node.js w aplikacjach sieci Web w systemie Linux](./app-service-linux-using-nodejs-pm2.md)

<!--Image references-->
[1]: ./media/app-service-linux-how-to-create-a-web-app/top-level-create.png
[2]: ./media/app-service-linux-how-to-create-a-web-app/create-blade.png
[3]: ./media/app-service-linux-how-to-create-a-web-app/application-settings-change-stack.png
[4]: ./media/app-service-linux-how-to-create-a-web-app/diagnostic-logs-ftp.png
