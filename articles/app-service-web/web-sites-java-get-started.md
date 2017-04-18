<properties
    pageTitle="Tworzenie aplikacji sieci web dla języka Java w usłudze Azure aplikacji | Microsoft Azure"
    description="Ten samouczek pokazano, jak wdrożyć aplikację sieci web Java Azure aplikacji usługi."
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
    ms.topic="get-started-article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="create-a-java-web-app-in-azure-app-service"></a>Tworzenie aplikacji sieci web dla języka Java w Azure aplikacji usługi

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Ten samouczek pokazano, jak utworzyć Java [aplikacji sieci web w usłudze Azure aplikacji] za pomocą [Azure Portal]. Azure Portal to interfejs sieci web, który służy do zarządzania zasobami Azure.

> [AZURE.NOTE] Aby użyć tego samouczka, potrzebne jest konto Microsoft Azure. Jeśli nie masz konta, możesz [uaktywnić programu Visual Studio subskrybentów korzyści] lub [Utwórz konto w bezpłatnej wersji próbnej].
>
> Jeśli chcesz rozpocząć pracę z Azure aplikacji usługi, aby utworzyć konto Azure, przejdź do [Spróbuj aplikacji usługi]. Od razu można utworzyć aplikację sieci web krótkotrwałe starter w aplikacji usługi; Karta kredytowa nie jest wymagane i nie zobowiązań.

## <a name="java-application-options"></a>Opcje aplikacji Java

Istnieje kilka sposobów, które służą do tworzenia aplikacji języka Java w aplikacji usługi aplikacji sieci web. 

1. Tworzenie aplikacji programu, a następnie skonfiguruj **Ustawienia aplikacji**.

    Usługi aplikacji udostępnia kilka wersji Tomcat i molo konfiguracji domyślnej. Aplikacji, która będzie obsługiwać będzie pracować z jednym z wbudowanych wersji, ta metoda konfigurowania kontenera web najłatwiej jest i jest doskonałe, gdy wszystko, co chcesz zrobić to przekazywanie pliku też kontenera sieci web. Dla tej metody utworzyć aplikację w Azure Portal, a następnie przejdź do sekcji karta **Ustawienia aplikacji** dla aplikacji wybierz swoją wersję Java wraz z żądanego kontenera Java w sieci web. Korzystając z tej metody można użyć zarówno Java i kontener usługi sieci web są uruchamiane z plików programów. Inne metody należy umieścić kontenerze sieci web i potencjalnie maszyny wirtualnej Java w dysku twardego. Korzystając z tego modelu, nie mają dostępu do edytowania plików w tej części systemu plików. Oznacza to, którego nie można elementów takich jak skonfigurować plik *server.xml* lub umieścić w folderze */lib* pliki biblioteki. Aby uzyskać więcej informacji, zobacz [Tworzenie i konfigurowanie aplikacji sieci web Java](#appsettings) sekcji w dalszej części tego samouczka.
    
2. Używanie szablonu z Azure Marketplace.

    Azure Marketplace zawiera szablony, które automatyczne tworzenie i konfigurowanie aplikacji sieci web Java z Tomcat lub molo kontenerów sieci web. Kontenery sieci web, utworzone szablony są konfigurowane. Aby uzyskać więcej informacji zobacz sekcję [Użyj szablonu Java z Azure Marketplace](#marketplace) tego samouczka.
  
3. Tworzenie aplikacji programu i ręczne skopiowanie i edytować pliki konfiguracji 

    Można udostępniać niestandardowej aplikacji Java, która nie wdrożony w każdym z kontenerów web dostępne w aplikacji usługi. Na przykład:
    
    * Aplikacja języka Java wymaga wersji Tomcat lub molo, które bezpośrednio nie jest obsługiwana przez usługę aplikacji lub dostępne w galerii.
    * Aplikacja języka Java zajmie żądania HTTP, a nie wdrażanie jako też do istniejącego kontenera sieci web.
    * Chcesz samodzielnie skonfigurować kontenerze web od podstaw. 
    * Chcesz używać wersji języka Java, który nie jest obsługiwane w aplikacji usługi i chcesz przekazać samodzielnie.

    Dla takich przypadkach możesz utworzyć aplikację za pomocą portalu Azure, a następnie ręcznie podaj odpowiednie środowisko uruchomieniowe plików. W tym przypadku przydziałami miejsca miejsca do magazynowania dla Twojego planu usługi aplikacji wliczane pliki. Aby uzyskać więcej informacji zobacz [przekazać niestandardową aplikację sieci web Java Azure].

## <a name="portal"></a>Tworzenie i konfigurowanie aplikacji sieci web dla języka Java

W tej sekcji przedstawiono sposób tworzysz aplikację sieci web i skonfiguruj go dla języka Java przy użyciu karta **Ustawienia aplikacji** portalu.

1. Zaloguj się do [portalu Azure].

2. Kliknij pozycję **Nowy > Web + Mobile > sieci Web App**.

    ![Nowa aplikacja sieci Web][newwebapp]

4. Wprowadź nazwę dla aplikacji sieci web w polu **aplikacji sieci Web** .

    Ta nazwa musi być unikatowa w domenie azurewebsites.net, ponieważ adres URL aplikacji sieci web będzie {nazwa}. azurewebsites.net. Jeśli wprowadzona nazwa nie jest unikatowy, czerwony wykrzyknik pojawi się w polu tekstowym.

5. Wybierz **Grupę zasobów** lub Utwórz nowy.

    Aby uzyskać więcej informacji dotyczących grup zasobów zobacz [Korzystanie Portal Azure do zarządzania zasobami Azure].

6. Wybierz **Plan i lokalizacja usługi aplikacji** lub Utwórz nowy.

    Aby uzyskać więcej informacji o planach usługi aplikacji zobacz [Omówienie planów Azure aplikacji usługi].

7. Kliknij przycisk **Utwórz**.

    ![Tworzenie aplikacji sieci Web][newwebapp2]
 
8. Po utworzeniu aplikacji sieci web kliknij pozycję **aplikacje sieci Web > {aplikacji sieci web}**.
 
    ![Wybierz aplikację sieci Web][selectwebapp]

9. W karta **aplikacji sieci Web** kliknij pozycję **Ustawienia**.

10. Kliknij pozycję **Ustawienia aplikacji**.

11. Wybieranie odpowiedniej **wersji języka Java**. 

12. Wybieranie odpowiedniej **Java wersją pomocniczą**. Jeśli wybierzesz **najnowszych**aplikacji użyje najnowszą wersją pomocniczą jest dostępna w aplikacji usługi dla tej wersji głównych Java. Element **najnowszych** jest unikatowy dla aplikacji Java utworzonych w obszarze **Ustawienia aplikacji**. Jeśli tworzysz aplikację Java z galerii, użytkownik ma własny kontenera i zmiany maszyny wirtualnej Java do zarządzania. 

12. Wybierz żądany **kontener sieci Web**. Po wybraniu nazwy kontenera, który zaczyna się **od najnowszych**aplikacji zostaną zachowane w najnowszej wersji tej sieci web kontenera głównych wersji, które są dostępne w aplikacji usługi. 

    ![Wersje kontenera sieci Web][versions]

13. Kliknij przycisk **Zapisz**.

    W chwili aplikacji sieci web będzie Java i skonfigurowana do korzystania z wybranym kontenerze sieci web.

14. Kliknij pozycję **sieci Web aplikacje > {nowej aplikacji sieci web}**.

15. Kliknij pozycję adres **URL** , przejdź do nowej witryny.

    Strony sieci web potwierdza utworzono aplikacji sieci web opartych na Java.

## <a name="marketplace"></a>Korzystanie z szablonu Java z Azure Marketplace

W tej sekcji przedstawiono sposób utworzenia Java aplikacji sieci web przy użyciu usługi Azure Marketplace. Samego przepływu ogólne można również utworzyć opartego na języku Java mobile lub aplikacji interfejsu API. 

1. Zaloguj się do [portalu Azure]

2. Kliknij pozycję **Nowy > Marketplace**.

    ![Nowe witryny Marketplace][newmarketplace]

3. Kliknij pozycję **Web + Mobile**.

    Może być konieczne przewinięcie w lewo do zobacz Karta **Marketplace** , w którym można wybrać **Web + Mobile**.

4. W polu tekstowym Wyszukaj wprowadź nazwę serwera aplikacji Java, takich jak **Apache Tomcat** lub **molo**, a następnie naciśnij klawisz Enter.

5. W wynikach wyszukiwania kliknij pozycję Serwer aplikacji Java.

    ![Molo urządzeń przenośnych w sieci Web][webmobilejetty]

6. W pierwszym karta **Apache Tomcat** lub **molo** kliknij przycisk **Utwórz**.

    ![Karta nadbrzeży portalu][jettyblade]

7. W następnej karta **Apache Tomcat** lub **molo** wprowadź nazwę dla aplikacji sieci web w polu **aplikacji sieci Web** .

    Ta nazwa musi być unikatowa w domenie azurewebsites.net, ponieważ adres URL aplikacji sieci web będzie {nazwa}. azurewebsites.net. Jeśli wprowadzona nazwa nie jest unikatowy, czerwony wykrzyknik pojawi się w polu tekstowym.

8. Wybierz **Grupę zasobów** lub Utwórz nowy.

    Aby uzyskać więcej informacji dotyczących grup zasobów zobacz [Korzystanie Portal Azure do zarządzania zasobami Azure].

9. Wybierz **Plan i lokalizacja usługi aplikacji** lub Utwórz nowy.

    Aby uzyskać więcej informacji o planach usługi aplikacji zobacz [Omówienie planów Azure aplikacji usługi].

10. Kliknij przycisk **Utwórz**.

    ![Tworzenie portalu nadbrzeży][jettyportalcreate2]

    Na chwilę, zwykle mniej niż minuta Azure kończy się tworzenie nowej aplikacji sieci web.

11. Kliknij pozycję **sieci Web aplikacje > {nowej aplikacji sieci web}**.

12. Kliknij pozycję adres **URL** , przejdź do nowej witryny.

    ![Adres URL nadbrzeży][jettyurl]

    Wyposażenie Tomcat domyślny zestaw stron. Jeśli wybierzesz Tomcat widoczna będzie podobny do następującego przykładu strony.

    ![Za pomocą Apache Tomcat aplikacji sieci Web][tomcat]

    Jeśli wybierzesz molo widoczna będzie podobny do następującego przykładu strony. Molo nie ma domyślny zestaw strony, dlatego samej JSP, używanym do pusta witryna Java wynikowej tutaj.

    ![Za pomocą molo aplikacji sieci Web][jetty]

Teraz, gdy aplikacji sieci web zostały utworzone za pomocą aplikacji kontenera, zobacz do sekcji [Następne kroki](#next-steps) , aby dowiedzieć się, jak przekazać aplikację, aby aplikacji sieci web.

## <a name="next-steps"></a>Następne kroki

W tym momencie masz serwer aplikacji Java działa w aplikacji sieci web w usłudze Azure aplikacji. Aby wdrożyć własnego kodu aplikacji sieci web, zobacz [Dodawanie aplikacji lub strony sieci Web do aplikacji sieci web języka Java].

Aby uzyskać więcej informacji na temat tworzenia aplikacji Java platformy Azure zobacz [Centrum deweloperów języka Java].

<!-- URL List -->

[Dodawanie aplikacji lub strony sieci Web do aplikacji sieci web języka Java]: ./web-sites-java-add-app.md
[Omówienie plany Azure aplikacji usługi]: ../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md
[Azure Portal]: https://portal.azure.com/
[aktywowanie korzyści subskrybentów programu Visual Studio]: http://go.microsoft.com/fwlink/?LinkId=623901
[Utwórz konto w bezpłatnej wersji próbnej]: http://go.microsoft.com/fwlink/?LinkId=623901
[Spróbuj aplikacji usługi]: http://go.microsoft.com/fwlink/?LinkId=523751
[w usłudze Azure aplikacji przy użyciu aplikacji sieci Web]: http://go.microsoft.com/fwlink/?LinkId=529714
[Centrum deweloperów języka Java]: /develop/java/
[Zarządzanie zasobami Azure za pomocą Azure Portal]: ../azure-portal/resource-group-portal.md
[Przekazać niestandardową aplikację sieci web Java Azure]: ./web-sites-java-custom-upload.md

<!-- IMG List -->

[newwebapp]: ./media/web-sites-java-get-started/newwebapp.png
[newwebapp2]: ./media/web-sites-java-get-started/newwebapp2.png
[selectwebapp]: ./media/web-sites-java-get-started/selectwebapp.png
[versions]: ./media/web-sites-java-get-started/versions.png
[newmarketplace]: ./media/web-sites-java-get-started/newmarketplace.png
[webmobilejetty]: ./media/web-sites-java-get-started/webmobilejetty.png
[jettyblade]: ./media/web-sites-java-get-started/jettyblade.png
[jettyportalcreate2]: ./media/web-sites-java-get-started/jettyportalcreate2.png
[jettyurl]: ./media/web-sites-java-get-started/jettyurl.png
[tomcat]: ./media/web-sites-java-get-started/tomcat.png
[jetty]: ./media/web-sites-java-get-started/jetty.png
