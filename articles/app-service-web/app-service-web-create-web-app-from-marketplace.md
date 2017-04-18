<properties
    pageTitle="Tworzenie aplikacji sieci web z usługi Azure Marketplace | Microsoft Azure"
    description="Dowiedz się, jak utworzyć nową aplikację sieci web WordPress z Azure Marketplace przy użyciu Azure Portal."
    services="app-service\web"
    documentationCenter=""
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/20/2016"
    ms.author="robmcm"/>

<!-- Note: This article replaces web-sites-php-web-site-gallery.md -->

# <a name="create-a-web-app-from-the-azure-marketplace"></a>Tworzenie aplikacji sieci web z usługi Azure Marketplace

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Azure Marketplace udostępnia szeroką gamę aplikacji web popularne opracowane przez firmę Microsoft, innej firmy i inicjatywy oprogramowania Otwórz źródło. Na przykład WordPress, Umbraco CMS, Drupal, itd. Te aplikacje sieci web utworzono na szeroką gamę popularnych struktury, takich jak [PHP] w tym WordPress przykład, [.NET], [Node.js], [Java]i [Python]kilka. Aby utworzyć aplikację sieci web z usługi Azure Marketplace tylko oprogramowania, które są potrzebne jest przeglądarki, która [Azure Portal].

W tym samouczku dowiesz się, jak:

* Znajdź i tworzenie aplikacji sieci web w usłudze Azure aplikacji, oparty na szablonie Azure Marketplace.
* Konfigurowanie ustawień Azure aplikacji usług dla nowej aplikacji sieci web.
* Uruchamianie i zarządzanie nimi aplikacji sieci web.

Do tego samouczka umieszczaniu WordPress blogu z Azure Marketplace. Po wykonaniu kroków opisanych w tym samouczku, konieczne będzie jego własnej witrynie WordPress w górę i uruchamiania w chmurze.

![Przykład WordPress wep aplikacji z pulpitu nawigacyjnego][WordPressDashboard1]

Witryna WordPress, który będzie można wdrożyć w tym samouczku używa MySQL bazy danych. Jeśli chcesz użyć bazy danych SQL dla bazy danych, zobacz [Nami projektu], który jest także dostępny za pośrednictwem usługi Azure Marketplace.

> [AZURE.NOTE]
> Aby użyć tego samouczka, potrzebne jest konto Microsoft Azure. Jeśli nie masz konta, możesz [uaktywnić programu Visual Studio subskrybentów korzyści] [ activate] lub [Utwórz konto w bezpłatnej wersji próbnej][free trial].
>
> Jeśli chcesz rozpocząć pracę z Azure aplikacji usługi, aby utworzyć konto Azure, przejdź do [Spróbuj aplikacji usługi]. Stamtąd możesz od razu utworzyć aplikacji sieci web krótkotrwałe starter w aplikacji usługi — karta kredytowa nie jest wymagane, a istnieje nie zobowiązań.

## <a name="find-and-create-a-web-app-in-azure-app-service"></a>Znajdowanie i tworzenie aplikacji sieci Web w usłudze Azure aplikacji

1. Zaloguj się do [portalu Azure].

1. Kliknij przycisk **Nowy**.
    
    ![Tworzenie nowego zasobu Azure][MarketplaceStart]
    
1. Wyszukiwanie **WordPress**, a następnie kliknij pozycję **WordPress**. (Jeśli chcesz używać bazy danych SQL zamiast MySQL, wyszukaj **Nami projektu**.)

    ![Wyszukiwanie WordPress na rynku][MarketplaceSearch]
    
1. Po przeczytaniu opis aplikacji WordPress, kliknij przycisk **Utwórz**.

    ![Tworzenie aplikacji sieci web WordPress][MarketplaceCreate]

## <a name="configure-azure-app-service-settings-for-your-new-web-app"></a>Konfigurowanie ustawień Azure aplikacji usługi dla nowej aplikacji sieci Web

1. Po utworzeniu nowej aplikacji sieci web, karta Ustawienia WordPress będą wyświetlane, którego użyjesz do należy wykonać następujące czynności:

    ![Konfigurowanie ustawień aplikacji sieci web WordPress][ConfigStart]

1. Wprowadź nazwę dla aplikacji sieci web w polu **aplikacji sieci Web** .

    Ta nazwa musi być unikatowa w domenie azurewebsites.net, ponieważ adres URL aplikacji sieci web będzie *{Nazwa}*. azurewebsites.net. Jeśli wprowadzona nazwa nie jest unikatowy, czerwony wykrzyknik pojawi się w polu tekstowym.

    ![Konfigurowanie WordPress Nazwa aplikacji sieci web][ConfigAppName]

1. Jeśli masz więcej niż jedną subskrypcję, wybierz ten, którego chcesz użyć. 

    ![Konfigurowanie subskrypcji dla aplikacji sieci web][ConfigSubscription]

1. Wybierz **Grupę zasobów** lub Utwórz nowy.

    Aby uzyskać więcej informacji dotyczących grup zasobów, zobacz [Omówienie Menedżera zasobów Azure][ResourceGroups].

    ![Konfigurowanie grupy zasobów dla aplikacji sieci web][ConfigResourceGroup]

1. Wybierz **Plan i lokalizacja usługi aplikacji** lub Utwórz nowy.

    Aby uzyskać więcej informacji o planach usługi aplikacji, zobacz [Omówienie planów Azure aplikacji usługi][AzureAppServicePlans]. 

    ![Konfigurowanie plan usług dla aplikacji sieci web][ConfigServicePlan]

1. Kliknij **bazę danych**, a następnie w **Nowej bazy danych MySQL** karta Podaj wartości wymaganych do konfigurowania bazy danych MySQL.

    . Wprowadź nową nazwę lub pozostaw domyślną nazwę.

    b. Pozostaw **Typ bazy danych** **udostępniony**.

    c. Wybierz tej samej lokalizacji, który wybrano dla aplikacji sieci web.

    d. Wybierz poziom cennik. **Rtęci** - czyli bezpłatnie z minimalnymi połączenia i miejsca na dysku — jest prawidłowy dla tego samouczka.

    e. W karta **Nowej bazy danych MySQL** Zaakceptuj warunki prawne, a następnie kliknij **przycisk OK**. 

    ![Konfigurowanie ustawień bazy danych dla aplikacji sieci web][ConfigDatabase]

1. W karta **WordPress** Zaakceptuj warunki prawne, a następnie kliknij przycisk **Utwórz**. 

    ![Zakończenie ustawień aplikacji sieci web i kliknij przycisk OK][ConfigFinished]

    Azure aplikacji usługi tworzy aplikacji sieci web, zazwyczaj w ciągu mniej niż minuty. Możesz obejrzeć postępu, klikając ikonę dzwonka w górnej części strony portalu.

    ![Wskaźnik postępu][ConfigProgress]

## <a name="launch-and-manage-your-wordpress-web-app"></a>Uruchamianie i zarządzanie nimi aplikacji sieci web WordPress
    
1. Po zakończeniu tworzenia aplikacji sieci web przejdź w Portal Azure do grupy zasobów, w której utworzono aplikacji, a widoczne aplikacji sieci web i bazy danych.

    Dodatkowe zasób z ikoną żarówka jest [Wniosków aplikacji][ApplicationInsights], który zapewnia monitorowania usług dla aplikacji sieci web.

1. W karta **Grupa zasobów** kliknij linię aplikacji sieci web.

    ![Wybieranie aplikacji sieci web WordPress][WordPressSelect]

1. W karta aplikacji sieci Web kliknij przycisk **Przeglądaj**.

    ![Przejdź do aplikacji sieci web WordPress][WordPressBrowse]

1. Jeśli zostanie wyświetlony monit, aby wybrać język dla blogu WordPress, wybierz odpowiedni język, a następnie kliknij przycisk **Kontynuuj**.

    ![Konfigurowanie języka dla aplikacji sieci web WordPress][WordPressLanguage]

1. Na stronie WordPress **powitalnej** wprowadź informacje o konfiguracji wymaganych przez WordPress, a następnie kliknij **WordPress instalacji**.

    ![Konfigurowanie ustawień aplikacji sieci web WordPress][WordPressConfigure]

1. Zaloguj się przy użyciu poświadczeń utworzonego na stronie **powitalnej** .  

1. Strony pulpitu nawigacyjnego witryny będą otworzyć i wyświetlić podane informacje.    

    ![Wyświetlanie pulpitu nawigacyjnego WordPress][WordPressDashboard2]

## <a name="next-steps"></a>Następne kroki

W tym samouczku użytkownik zobaczył, jak tworzyć i wdrażanie aplikacji sieci web programu przykład z usługi Azure Marketplace.

Aby uzyskać więcej informacji na temat pracy z aplikacji sieci Web usługi, zobacz łącza po lewej stronie strony (w przypadku wielu przeglądarki w systemie windows) lub u góry strony (dla systemu windows wąskie przeglądarek).

Aby uzyskać więcej informacji na temat opracowywania aplikacji sieci web WordPress Azure, zobacz [Opracowywania WordPress Azure aplikacji usługi][WordPressOnAzure]. 

<!-- URL List -->

[PHP]: https://azure.microsoft.com/develop/php/
[.NET]: https://azure.microsoft.com/develop/net/
[Node.js]: https://azure.microsoft.com/develop/nodejs/
[Java]: https://azure.microsoft.com/develop/java/
[Python]: https://azure.microsoft.com/develop/python/
[activate]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[free trial]: https://azure.microsoft.com/pricing/free-trial/
[Spróbuj aplikacji usługi]: http://go.microsoft.com/fwlink/?LinkId=523751
[ResourceGroups]: ../resource-group-overview.md
[AzureAppServicePlans]: ../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md
[ApplicationInsights]: https://azure.microsoft.com/services/application-insights/
[Azure Portal]: https://portal.azure.com/
[Nami projektu]: http://projectnami.org/
[WordPressOnAzure]: ./develop-wordpress-on-app-service-web-apps.md

<!-- IMG List -->

[MarketplaceStart]: ./media/app-service-web-create-web-app-from-marketplace/marketplacestart.png
[MarketplaceSearch]: ./media/app-service-web-create-web-app-from-marketplace/marketplacesearch.png
[MarketplaceCreate]: ./media/app-service-web-create-web-app-from-marketplace/marketplacecreate.png
[ConfigStart]: ./media/app-service-web-create-web-app-from-marketplace/configstart.png
[ConfigAppName]: ./media/app-service-web-create-web-app-from-marketplace/configappname.png
[ConfigSubscription]: ./media/app-service-web-create-web-app-from-marketplace/configsubscription.png
[ConfigResourceGroup]: ./media/app-service-web-create-web-app-from-marketplace/configresourcegroup.png
[ConfigServicePlan]: ./media/app-service-web-create-web-app-from-marketplace/configserviceplan.png
[ConfigDatabase]: ./media/app-service-web-create-web-app-from-marketplace/configdatabase.png
[ConfigFinished]: ./media/app-service-web-create-web-app-from-marketplace/configfinished.png
[ConfigProgress]: ./media/app-service-web-create-web-app-from-marketplace/configprogress.png
[WordPressSelect]: ./media/app-service-web-create-web-app-from-marketplace/wpselect.png
[WordPressBrowse]: ./media/app-service-web-create-web-app-from-marketplace/wpbrowse.png
[WordPressLanguage]: ./media/app-service-web-create-web-app-from-marketplace/wplanguage.png
[WordPressDashboard1]: ./media/app-service-web-create-web-app-from-marketplace/wpdashboard1.png
[WordPressDashboard2]: ./media/app-service-web-create-web-app-from-marketplace/wpdashboard2.png
[WordPressConfigure]: ./media/app-service-web-create-web-app-from-marketplace/wpconfigure.png
