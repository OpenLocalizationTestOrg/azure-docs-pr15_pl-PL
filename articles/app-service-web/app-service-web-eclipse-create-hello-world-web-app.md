<properties 
    pageTitle="Tworzenie aplikacji sieci Web dla Witaj świecie dla Azure w Zaćmienie | Microsoft Azure" 
    description="Ten samouczek pokazano, jak używać narzędzi Azure dla programu Eclipse do tworzenia Witaj świecie aplikacji sieci Web dla Azure." 
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
    ms.date="08/26/2016" 
    ms.author="robmcm"/>

# <a name="create-a-hello-world-web-app-for-azure-in-eclipse"></a>Tworzenie aplikacji Web App Witaj świecie dla Azure w Zaćmienie

Ten samouczek pokazano, jak tworzyć i wdrożyć aplikację podstawowe Witaj świecie Azure jako aplikacji sieci Web przy użyciu [Narzędzi Azure dla programu Eclipse]. Prosty przykład JSP są wyświetlane w celu uproszczenia, ale bardzo podobne kroki będzie servlet Java chodzi Azure wdrożenia jest.

Po wykonaniu tego samouczka, aplikacja będzie wyglądać podobnie do następującej ilustracji, podczas wyświetlania w przeglądarce sieci web:

![Podgląd Witaj świecie aplikacji][01]
 
## <a name="prerequisites"></a>Wymagania wstępne

* Java Developer Kit (JDK), v 1.8 lub nowszym.
* Zaćmienie-IDE dla deweloperów Estonia Java Luna lub nowszym. To można pobrać z <http://www.eclipse.org/downloads/>.
* Rozkład serwera aplikacji, takich jak Apache Tomcat lub molo lub serwera sieci web opartych na Java.
* Azure subskrypcji, które mogą pochodzić z <https://azure.microsoft.com/free/> lub <http://azure.microsoft.com/pricing/purchase-options/>.
* Azure zestaw narzędzi dla Zaćmienie. Aby uzyskać więcej informacji zobacz [Instalowanie narzędzi Azure dla programu Eclipse].

## <a name="to-create-a-hello-world-application"></a>Aby utworzyć aplikację Witaj świecie

Najpierw Zaczniemy od tworzenia projektu języka Java.

1. Rozpoczynanie Zaćmienie i w menu kliknij pozycję **plik**, kliknij pozycję **Nowy**, a następnie kliknij **Dynamiczne projektu sieci Web**. (Jeśli nie widzisz **Dynamiczne projektu sieci Web** na liście jako projekt dostępne po kliknięciu pozycji **plik** i **Nowy**, wykonaj następujące czynności: kliknij pozycję **plik**, kliknij przycisk **Nowy**, kliknij pozycję **Projekt...**, rozwiń **sieci Web**, kliknij pozycję **Project Web dynamiczne**i kliknij przycisk **Dalej**.)

1. Na potrzeby tego samouczka nazwę projektu **Moja_aplikacja_sieci_web**. Na ekranie pojawią się podobny do następującego:

    ![Tworzenie nowego projektu sieci Web dynamicznych][02]

1. Kliknij przycisk **Zakończ**.

1. W widoku Eksplorator projektu i Zaćmienie rozwiń **Moja_aplikacja_sieci_web**. Kliknij prawym przyciskiem myszy **sieć Web, zawartość**, kliknij pozycję **Nowy**, a następnie kliknij **Plik JSP**.

1. W oknie dialogowym **Nowy plik JSP** nazwę pliku **index.jsp**zachować folder nadrzędny jako **Moja_aplikacja_sieci_web-sieć Web, zawartość**, a następnie kliknij **Dalej**.

1. W oknie dialogowym **Wybierz szablon JSP** na potrzeby tego samouczka wybierz **Nowy plik JSP (html)**, a następnie kliknij przycisk **Zakończ**.

1. Po otwarciu pliku index.jsp w Zaćmienie, dodawanie tekstu do dynamicznego wyświetlania **Witaj świecie!** w istniejącej `<body>` elementu. Usługi zaktualizowanych `<body>` zawartości należy podobne do następującego przykładu:

    `<body><b><% out.println("Hello World!"); %></b></body>` 

1. Zapisz index.jsp.

## <a name="to-deploy-your-application-to-an-azure-web-app-container"></a>Wdrażania aplikacji do kontenera aplikacji Azure Web

Istnieje kilka sposobów, za pomocą których można wdrażać aplikacji sieci web Java Azure. Ten przewodnik opisuje jeden z najłatwiejszym: aplikacja zostanie wdrożony do kontenera aplikacji sieci Web Azure — nie typ projektu ani dodatkowe narzędzia są wymagane. JDK i oprogramowania kontenera sieci web będzie dostępna dla Ciebie w Azure, więc nie trzeba przekazać własne; potrzebny jest aplikacji sieci Web języka Java. W wyniku procesu publikowania aplikacji potrwa sekund, nie minut.

1. W oknie Project Explorer Zaćmienie osoby kliknij prawym przyciskiem myszy **Moja_aplikacja_sieci_web**.

1. W menu kontekstowym wybierz **Azure**, a następnie kliknij polecenie **Publikuj jako aplikacji sieci Web Azure...**

    ![Publikowanie jako Azure aplikacji sieci Web][03]
   
    Alternatywnie podczas projektu aplikacji sieci web jest zaznaczona w oknie Eksploratora projektu, możesz kliknij przycisk **Publikuj** listy rozwijanej na pasku narzędzi i wybierz **Publikuj jako Azure Web App** z niego:
   
    ![Publikowanie jako Azure aplikacji sieci Web][14]
   
1. Jeśli nie zalogowaniu się do Azure z Zaćmienie, zostanie wyświetlony monit o zalogowanie się do konta Azure:

    ![Azure dialogowe Logowanie][04]
   
    Jeśli masz wiele kont Azure, niektóre z monitami podczas procesu logowania w mogą zostać wyświetlone więcej niż jeden raz, nawet jeśli znajdują się tak samo. W takim przypadku nadal po znaku w instrukcjach.

1. Po zarejestrowaniu pomyślnie do konta usługi Azure, okno dialogowe **Zarządzaj subskrypcjami** spowoduje wyświetlenie subskrypcji, które są skojarzone z poświadczenia. Jeśli istnieje wiele subskrypcji na liście i chcesz pracować z określonym podzbiorem ich, może być opcjonalnie wyczyść pole wyboru tych, których chcesz użyć. Po wybraniu subskrypcji, kliknij przycisk **Zamknij**.

    ![Zarządzanie subskrypcjami, okno dialogowe][05]
   
1. Gdy pojawi się okno dialogowe **Rozmieszczanie do kontenera aplikacji sieci Web Azure** , będzie wyświetlany kontenerów aplikacji sieci Web, wszelkie utworzone wcześniej; Jeśli nie została utworzona wszelkie kontenery, lista będzie pusta.

    ![Wdrażanie kontenera aplikacji sieci Web Azure, okno dialogowe][06]
   
1. Jeśli nie utworzono kontener aplikacji sieci Web Azure przed lub jeśli chcesz opublikować aplikacji do nowego kontenera, wykonaj następujące czynności. W przeciwnym razie wybierz istniejącego kontenera aplikacji sieci Web i przejdź do kroku 7 poniżej.

    1. Kliknij pozycję **Nowy...**

        ![Wdrażanie kontenera aplikacji sieci Web Azure, okno dialogowe][15]

    1. Zostanie wyświetlone okno dialogowe **Nowy kontener aplikacji sieci Web** :

        ![Okno dialogowe Nowa kontenera aplikacji sieci Web][07a]

    1. Wpisz **Etykieta DNS** dla swojej kontenera aplikacji sieci Web; Spowoduje to formularza Etykieta DNS liści adres URL hosta dla aplikacji sieci web platformy Azure. (Należy zauważyć, czy nazwa musi być dostępny i spełniają wymagania dotyczące nazewnictwa Azure Web App).

    1. W menu rozwijanym **Web kontenera** wybierz odpowiednie oprogramowanie aplikacji.

        Obecnie możesz wybrać Tomcat 8 Tomcat 7 lub molo 9. Ostatnio używane rozkład wybranego oprogramowania będzie dostępna w Azure i będzie działać na ostatnich rozkład 8 JDK utworzone przez Oracle i udostępniane przez Azure.

    1. W menu rozwijanym **subskrypcji** Wybierz subskrypcję, którą chcesz użyć dla tego rozmieszczenia.

    1. W menu rozwijanego **Grupa zasobów** wybierz grupa zasobów, z którym chcesz skojarzyć aplikacji sieci Web. (Azure grup zasobów umożliwiają pogrupować zasoby pokrewne, dzięki czemu, na przykład je usunąć ze sobą.)

        Wybierz pozycję istniejącej grupy zasobów (Jeśli korzystasz z dowolnego) i Pomiń krok g poniżej lub użyć następującej składni tych kroków, aby utworzyć nową grupę zasobów:

        * Kliknij pozycję **Nowy...**

        * Zostanie wyświetlone okno dialogowe **Nowej grupy zasobów** :

            ![Okno dialogowe nowej grupy zasobów][08]

        * W polu tekstowym **Nazwa** Określ nazwę dla nowej grupy zasobów.

        * W menu rozwijanym **Region** , wybierz odpowiednie dane Azure Centrum lokalizacji dla grupy zasobów.

        * OPCJONALNIE: Domyślnie ostatnie rozkład Java 8 zostanie wdrożony przez Azure automatycznie do kontenera aplikacji sieci web jako usługi maszyny wirtualnej Java. Można jednak określić innej wersji i rozkład maszyny wirtualnej Java, jeśli wymaga aplikacji sieci Web. Aby określić JDK dla aplikacji sieci Web, kliknij kartę **JDK** i wybierz jedną z następujących opcji:

            * **Rozmieszczanie domyślne JDK oferowanych przez usługę Azure aplikacji sieci Web**: Ta opcja zostanie wdrożony ostatnie rozkład Java 8.

            * **Rozmieszczanie 3rd strony JDK dostępne na Azure**: Ta opcja pozwala wybrać z listy JDKs, które są dostarczane przez Microsoft Azure.

            * **Rozmieszczanie własnej JDK z tej lokalizacji plik do pobrania**: Ta opcja umożliwia określenie własnego JDK rozkładem musi być dostarczana w pliku ZIP i przekazać do lokalizacji pobierania publicznie lub konto Azure miejsca do magazynowania, dla których masz dostęp.

            ![Okno dialogowe Nowa kontenera aplikacji sieci Web][07b]

    * Kliknij **przycisk OK**.

    1. Menu rozwijane **Plan usługi aplikacji** lista plany usługi aplikacji, które są skojarzone z wybrana grupa zasobów. (Plany aplikacji usługi Określ informacje, takie jak lokalizacja aplikacji sieci Web, warstwie cennik i rozmiar wystąpienie obliczeń. Pojedynczy Planowanie aplikacji usługi może służyć do wielu aplikacji sieci Web, dlatego są obsługiwane niezależnie od określonych wdrażaniem aplikacji sieci Web.)

        Wybieranie istniejącej aplikacji usługi Planowanie (Jeśli korzystasz z dowolnego) i Pomiń krok h poniżej lub użyć następującej składni tych kroków, aby utworzyć nowy Plan aplikacji usługi:

        * Kliknij pozycję **Nowy...**

        * Zostanie wyświetlone okno dialogowe **Nowy Plan usługi aplikacji** :

            ![Okno dialogowe Nowy Plan usługi aplikacji][09]

        * W polu tekstowym **Nazwa** Określ nazwę dla planu usługi aplikacji.

        * W menu rozwijanym **lokalizacji** , wybierz odpowiednie dane Azure Centrum lokalizacji dla planu.

        * W menu rozwijanym **Ceny warstwy** , zaznacz odpowiednie ceny planu. Podczas testowania możesz wybrać **wolny**.

        * W menu rozwijanym **Rozmiaru wystąpienia** wybierz odpowiednie wystąpienie rozmiaru planu. Podczas testowania możesz wybrać **Small**.

    1. Po wykonaniu wszystkich kroków powyżej okna dialogowego nowego kontenera aplikacji sieci Web powinien wyglądać poniższej ilustracji:

        ![Okno dialogowe Nowa kontenera aplikacji sieci Web][10]

    1. Kliknij **przycisk OK** , aby zakończyć tworzenie nowego kontenera aplikacji sieci Web.

        Poczekaj kilka sekund na liście kontenerów aplikacji Web App odświeżenia, a do kontenera aplikacji web nowo utworzone teraz powinny być zaznaczone na liście.

1. Teraz możesz przystąpić do ukończenia początkowego wdrażania aplikacji sieci Web Azure:

    ![Wdrażanie kontenera aplikacji sieci Web Azure, okno dialogowe][11]

    Kliknij **przycisk OK** , aby wdrażanie aplikacji Java wybranym kontenerze aplikacji sieci Web.

    Domyślnie aplikacja zostanie wdrożony jako podkatalogów serwera aplikacji. Jeśli ma być używany jako głównej aplikacji, zaznacz pole wyboru **Rozmieszczanie do głównego** przed kliknięciem przycisku **OK**.

1. Następnie powinien zostać wyświetlony widok **Dziennika aktywności Azure** , w którym będzie wskazywać stanu wdrażania aplikacji sieci Web.

    ![Dziennik Azure][12]

    Proces wdrażania aplikacji sieci Web Azure należy wykonać tylko kilka sekund. Gdy z gotowych aplikacji zostanie wyświetlona łącza o nazwie **opublikowane** w kolumnie **Stan** . Po kliknięciu łącza go spowoduje przejście do strony głównej usługi wdrożonej aplikacji sieci Web.

## <a name="updating-your-web-app"></a>Aktualizowanie aplikacji sieci Web

Aktualizowanie istniejącego uruchamiania aplikacji sieci Web Azure to proces szybkiego i łatwego i masz dwie opcje aktualizacji:

* Wdrażanie aplikacji sieci Web programu istniejących Java można aktualizować.
* Możesz opublikować dodatkowe aplikacji Java w kontenerze samej aplikacji sieci Web.

W obu przypadkach proces jest identyczny i trwa tylko kilka sekund:

1. W oknie Eksploratora projektu Zaćmienie kliknij prawym przyciskiem myszy aplikacji Java, który chcesz zaktualizować lub dodać do istniejącego kontenera aplikacji sieci Web.

1. Po wyświetleniu menu kontekstowego wybierz **Azure** , a następnie **Publikuj jako aplikacji sieci Web Azure...**

1. Ponieważ użytkownik już zalogował się wcześniej, pojawi się lista kontenerów istniejącej aplikacji sieci Web. Wybierz ten, który chcesz opublikować lub ponowne publikowanie aplikacji Java i kliknij **przycisk OK**.

Kilka sekund później, w widoku **Dziennika aktywności Azure** będą wyświetlane rozmieszczenia zaktualizowanej jako **opublikowane** i będzie można sprawdzić zaktualizowanej aplikacji w przeglądarce sieci web.

## <a name="stopping-an-existing-web-app"></a>Zatrzymywanie istniejącej aplikacji sieci Web

Aby zatrzymać istniejącej aplikacji sieci Web Azure kontener, (w tym wszystkie rozmieszczonych aplikacji Java w nim) służy widok **Eksploratora Azure** .

Jeśli widok **Eksploratora Azure** nie jest jeszcze otwarta, możesz go otworzyć, klikając menu **okno** w Zaćmienie, następnie, a następnie kliknij pozycję **Pokaż widok**, a następnie **inne...**, a następnie **Azure**, a następnie kliknij **Eksploratora Azure**. Jeśli możesz już zalogowaniu, monituje możesz to zrobić.

Gdy zostanie wyświetlony widok **Eksploratora Azure** , użyj wykonaj poniższe czynności, aby zatrzymać aplikacji sieci Web: 

1. Rozwiń węzeł **Azure** .

1. Rozwiń węzeł **Aplikacji sieci Web** . 

1. Kliknij prawym przyciskiem myszy żądaną aplikację sieci Web.

1. Gdy pojawi się menu kontekstowego, kliknij przycisk **Zatrzymaj**.

    ![Zatrzymywanie istniejącej aplikacji sieci Web][13]

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji na temat narzędzi Azure dla języka Java IDEs zobacz poniższe łącza:

- [Azure zestaw narzędzi dla programu Eclipse]
  - [Instalowanie narzędzi Azure dla programu Eclipse]
  - *Tworzenie aplikacji sieci Web dla Witaj świecie dla Azure w Zaćmienie (ten artykuł)*
  - [Co nowego w Azure zestaw narzędzi dla programu Eclipse]
- [Azure zestaw narzędzi dla IntelliJ]
  - [Instalowanie narzędzi Azure dla IntelliJ]
  - [Tworzenie aplikacji sieci Web dla Witaj świecie dla Azure w IntelliJ]
  - [Co nowego w Azure zestaw narzędzi dla IntelliJ]

Aby uzyskać więcej informacji na temat Azure za pomocą języka Java zobacz [Centrum deweloperów języka Java Azure].

Aby uzyskać dodatkowe informacje o tworzeniu Azure aplikacji sieci Web zobacz [Omówienie aplikacji sieci Web].

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure zestaw narzędzi dla programu Eclipse]: ../azure-toolkit-for-eclipse.md
[Azure zestaw narzędzi dla IntelliJ]: ../azure-toolkit-for-intellij.md
[Create a Hello World Web App for Azure in Eclipse]: ./app-service-web-eclipse-create-hello-world-web-app.md
[Tworzenie aplikacji sieci Web dla Witaj świecie dla Azure w IntelliJ]: ./app-service-web-intellij-create-hello-world-web-app.md
[Instalowanie narzędzi Azure dla programu Eclipse]: ../azure-toolkit-for-eclipse-installation.md
[Instalowanie narzędzi Azure dla IntelliJ]: ../azure-toolkit-for-intellij-installation.md
[Co nowego w Azure zestaw narzędzi dla programu Eclipse]: ../azure-toolkit-for-eclipse-whats-new.md
[Co nowego w Azure zestaw narzędzi dla IntelliJ]: ../azure-toolkit-for-intellij-whats-new.md

[Centrum deweloperów języka Java Azure]: https://azure.microsoft.com/develop/java/
[Omówienie aplikacji sieci Web]: ./app-service-web-overview.md

<!-- IMG List -->

[01]: ./media/app-service-web-eclipse-create-hello-world-web-app/01-Web-Page.png
[02]: ./media/app-service-web-eclipse-create-hello-world-web-app/02-Dynamic-Web-Project.png
[03]: ./media/app-service-web-eclipse-create-hello-world-web-app/03-Context-Menu.png
[04]: ./media/app-service-web-eclipse-create-hello-world-web-app/04-Log-In-Dialog.png
[05]: ./media/app-service-web-eclipse-create-hello-world-web-app/05-Manage-Subscriptions-Dialog.png
[06]: ./media/app-service-web-eclipse-create-hello-world-web-app/06-Deploy-To-Azure-Web-Container.png
[07a]: ./media/app-service-web-eclipse-create-hello-world-web-app/07a-New-Web-App-Container-Dialog.png
[07b]: ./media/app-service-web-eclipse-create-hello-world-web-app/07b-New-Web-App-Container-Dialog.png
[08]: ./media/app-service-web-eclipse-create-hello-world-web-app/08-New-Resource-Group-Dialog.png
[09]: ./media/app-service-web-eclipse-create-hello-world-web-app/09-New-Service-Plan-Dialog.png
[10]: ./media/app-service-web-eclipse-create-hello-world-web-app/10-Completed-Web-App-Container-Dialog.png
[11]: ./media/app-service-web-eclipse-create-hello-world-web-app/11-Completed-Deploy-Dialog.png
[12]: ./media/app-service-web-eclipse-create-hello-world-web-app/12-Activity-Log-View.png
[13]: ./media/app-service-web-eclipse-create-hello-world-web-app/13-Azure-Explorer-Web-App.png
[14]: ./media/app-service-web-eclipse-create-hello-world-web-app/14-publishDropdownButton.png
[15]: ./media/app-service-web-eclipse-create-hello-world-web-app/15-New-Azure-Web-Container.png
