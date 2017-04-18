<properties 
    pageTitle="Tworzenie aplikacji sieci Web dla Witaj świecie dla Azure w IntelliJ | Microsoft Azure" 
    description="Ten samouczek pokazano, jak używać narzędzi Azure dla IntelliJ do tworzenia Witaj świecie aplikacji sieci Web dla Azure." 
    services="app-service\web" 
    documentationCenter="java" 
    authors="selvasingh" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="asirveda;robmcm"/>

# <a name="create-a-hello-world-web-app-for-azure-in-intellij"></a>Tworzenie aplikacji sieci Web dla Witaj świecie dla Azure w IntelliJ

Ten samouczek pokazano, jak tworzyć i wdrożyć aplikację podstawowe Witaj świecie Azure jako aplikacji sieci Web przy użyciu [Narzędzi Azure dla IntelliJ]. Prosty przykład JSP są wyświetlane w celu uproszczenia, ale bardzo podobne kroki będzie servlet Java chodzi Azure wdrożenia jest.

Po wykonaniu tego samouczka, aplikacja będzie wyglądać podobnie do następującej ilustracji, podczas wyświetlania w przeglądarce sieci web:

![][01]
 
## <a name="prerequisites"></a>Wymagania wstępne

* Java Developer Kit (JDK), v 1.8 lub nowszym.
* IntelliJ POMYSŁU Ultimate Edition. To można pobrać z <https://www.jetbrains.com/idea/download/index.html>.
* Rozkład serwera aplikacji, takich jak Apache Tomcat lub molo lub serwera sieci web opartych na Java.
* Azure subskrypcji, które mogą pochodzić z <https://azure.microsoft.com/free/> lub <http://azure.microsoft.com/pricing/purchase-options/>.
* Azure zestaw narzędzi IntelliJ. Aby uzyskać więcej informacji zobacz [Instalowanie narzędzi Azure dla IntelliJ].

## <a name="to-create-a-hello-world-application"></a>Aby utworzyć aplikację Witaj świecie

Najpierw Zaczniemy od tworzenia projektu języka Java.

1. Rozpoczynanie IntelliJ i w menu kliknij pozycję **plik**, a następnie **Nowy**, a następnie kliknij **Projekt**.

   ![][02]

1. W oknie dialogowym Nowy projekt **języka Java**, następnie **Aplikacji sieci Web**, wybierz, a następnie kliknij przycisk **Dalej**.

   ![][03a]

   Jeśli zostanie wyświetlony monit, aby kontynuować nie SDK przydzielone, kliknij przycisk **Tak**.

   ![][03b]

1. Na potrzeby tego samouczka nadaj projektu **Java-Web — aplikacja-na-Azure**, a następnie kliknij przycisk **Zakończ**.

   ![][04]

1. W widoku Eksplorator projektu w IntelliJ rozwiń pozycję **Języka Java-sieci Web — aplikacja-na-Azure**, a następnie rozwiń **sieci web**i kliknij dwukrotnie **index.jsp**.

   ![][05]

1. Po otwarciu pliku index.jsp w IntelliJ, dodać tekst, aby dynamiczne wyświetlanie **Witaj świecie!** w istniejącej `<body>` elementu. Usługi zaktualizowanych `<body>` zawartości należy podobne do następującego przykładu:

   `<body><b><% out.println("Hello World!"); %></b></body>` 

1. Zapisz index.jsp.

## <a name="to-deploy-your-application-to-an-azure-web-app-container"></a>Wdrażania aplikacji do kontenera aplikacji Azure Web

Istnieje kilka sposobów, za pomocą których można wdrażać aplikacji sieci web Java Azure. Ten przewodnik opisuje jeden z najłatwiejszym: aplikacja zostanie wdrożony do kontenera aplikacji sieci Web Azure — nie typ projektu ani dodatkowe narzędzia są wymagane. JDK i oprogramowania kontenera sieci web będzie dostępna dla Ciebie w Azure, więc nie trzeba przekazać własne; potrzebny jest aplikacji sieci Web języka Java. W wyniku procesu publikowania aplikacji potrwa sekund, nie minut.

1. W oknie Project Explorer IntelliJ osoby kliknij prawym przyciskiem myszy projektu **Java — sieci Web — aplikacja-na-Azure** . Po wyświetleniu menu kontekstowego wybierz **Azure**, a następnie kliknij **Publikuj jako aplikacji sieci Web Azure...**

   ![][06]

1. Jeśli nie zalogowaniu się do Azure z IntelliJ, zostanie wyświetlony monit o zalogowanie się do konta Azure:

   ![][07]

   Uwaga: Jeśli masz wiele kont Azure, niektóre z monitami podczas procesu logowania w mogą zostać wyświetlone więcej niż jeden raz, nawet jeśli znajdują się tak samo. W takim przypadku nadal po znaku w instrukcjach.

1. Po zarejestrowaniu pomyślnie do konta usługi Azure, okno dialogowe **Zarządzaj subskrypcjami** spowoduje wyświetlenie subskrypcji, które są skojarzone z poświadczenia. Jeśli istnieje wiele subskrypcji na liście i chcesz pracować z określonym podzbiorem ich, może być opcjonalnie wyczyść pole wyboru tych, których chcesz użyć. Po wybraniu subskrypcji, kliknij przycisk **Zamknij**.

   ![][08]

1. Gdy pojawi się okno dialogowe **Rozmieszczanie do kontenera aplikacji sieci Web Azure** , będzie wyświetlany kontenerów aplikacji sieci Web, wszelkie utworzone wcześniej; Jeśli nie została utworzona wszelkie kontenery, lista będzie pusta.   

   ![][09]

1. Jeśli nie utworzono kontener aplikacji sieci Web Azure przed lub jeśli chcesz opublikować aplikacji do nowego kontenera, wykonaj następujące czynności. W przeciwnym razie wybierz istniejącego kontenera aplikacji sieci Web i przejdź do kroku 6 poniżej.

  1. Kliknij pozycję**+**

        ![][10]

  1. Pojawi się okno dialogowe **Nowy kontener aplikacji sieci Web** , która będzie używana dla kilku czynności.

        ![][11]

  1. Wpisz **Etykieta DNS** dla swojej kontenera aplikacji sieci Web; Spowoduje to formularza Etykieta DNS liści adres URL hosta dla aplikacji sieci web platformy Azure. Uwaga: Nazwa musi być dostępne i spełniają wymagania dotyczące nazewnictwa Azure Web App.

  1. W menu rozwijanym **Web kontenera** wybierz odpowiednie oprogramowanie aplikacji.

        Obecnie możesz wybrać Tomcat 8 Tomcat 7 lub molo 9. Ostatnio używane rozkład wybranego oprogramowania będzie dostępna w Azure i będzie działać na ostatnich rozkład 8 JDK utworzone przez Oracle i udostępniane przez Azure.

  1. W menu rozwijanym **subskrypcji** Wybierz subskrypcję, którą chcesz użyć dla tego rozmieszczenia.

  1. W menu rozwijanego **Grupa zasobów** wybierz grupa zasobów, z którym chcesz skojarzyć aplikacji sieci Web.

        Uwaga: Azure grup zasobów umożliwiają pogrupować zasoby pokrewne, dzięki czemu, na przykład je usunąć ze sobą.

        Wybierz pozycję istniejącej grupy zasobów (Jeśli korzystasz z dowolnego) i Pomiń krok g poniżej lub użyć następującej składni tych kroków, aby utworzyć nową grupę zasobów:

      * Kliknij pozycję **Nowy...**

      * Zostanie wyświetlone okno dialogowe **Nowej grupy zasobów** :

            ![][12]

      * W polu tekstowym **Nazwa** Określ nazwę dla nowej grupy zasobów.

      * W menu rozwijanym **Region** , wybierz odpowiednie dane Azure Centrum lokalizacji dla grupy zasobów.

      * Kliknij **przycisk OK**.

  1. Menu rozwijane **Plan usługi aplikacji** lista plany usługi aplikacji, które są skojarzone z wybrana grupa zasobów.

        Uwaga: Plan usługi aplikacji określa informacje dotyczące takie jak lokalizacja aplikacji sieci Web, warstwie cennik i rozmiar wystąpienie obliczeń. Pojedynczy Planowanie aplikacji usługi może służyć do wielu aplikacji sieci Web, dlatego są obsługiwane niezależnie od określonych wdrażaniem aplikacji sieci Web.

        Wybieranie istniejącej aplikacji usługi Planowanie (Jeśli korzystasz z dowolnego) i Pomiń krok h poniżej lub użyć następującej składni tych kroków, aby utworzyć nowy Plan aplikacji usługi:

      * Kliknij pozycję **Nowy...**

      * Zostanie wyświetlone okno dialogowe **Nowy Plan usługi aplikacji** :

            ![][13]

      * W polu tekstowym **Nazwa** Określ nazwę dla planu usługi aplikacji.

      * W menu rozwijanym **lokalizacji** , wybierz odpowiednie dane Azure Centrum lokalizacji dla planu.

      * W menu rozwijanym **Ceny warstwy** , zaznacz odpowiednie ceny planu. Podczas testowania możesz wybrać **wolny**.

      * W menu rozwijanym **Rozmiaru wystąpienia** wybierz odpowiednie wystąpienie rozmiaru planu. Podczas testowania możesz wybrać **Small**.

  1. Po wykonaniu wszystkich kroków powyżej okna dialogowego nowego kontenera aplikacji sieci Web powinien wyglądać poniższej ilustracji:

        ![][14]

  1. Kliknij **przycisk OK** , aby zakończyć tworzenie nowego kontenera aplikacji sieci Web.

        Poczekaj kilka sekund na liście kontenerów aplikacji Web App odświeżenia, a do kontenera aplikacji web nowo utworzone teraz powinny być zaznaczone na liście.

1. Możesz już przystąpić do ukończenia początkowego wdrażania aplikacji sieci Web Azure; Kliknij **przycisk OK** , aby wdrażanie aplikacji Java wybranym kontenerze aplikacji sieci Web.

    ![][15]

    Uwaga: Domyślnie aplikacja zostanie wdrożony jako podkatalogów serwera aplikacji. Jeśli ma być używany jako głównej aplikacji, zaznacz pole wyboru **Rozmieszczanie do głównego** przed kliknięciem przycisku **OK**.

1. Następnie powinien zostać wyświetlony widok **Dziennika aktywności Azure** , w którym będzie wskazywać stanu wdrażania aplikacji sieci Web.

    ![][16]

    Proces wdrażania aplikacji sieci Web Azure należy wykonać tylko kilka sekund. Gdy z gotowych aplikacji zostanie wyświetlona łącza o nazwie **opublikowane** w kolumnie **Stan** . Po kliknięciu łącza spowoduje przejście do strony głównej usługi wdrożonej aplikacji sieci Web lub można wykonaj czynności podane w poniższej sekcji, aby przejść do aplikacji sieci web.

## <a name="browsing-to-your-web-app-on-azure"></a>Przejdź do aplikacji sieci Web Azure

Aby brows do aplikacji sieci Web Azure możesz użyć widoku **Eksplorator Azure** .

Jeśli widok **Eksploratora Azure** nie jest jeszcze otwarta, możesz go otworzyć, klikając menu **Widok** w IntelliJ, następnie, a następnie kliknij pozycję **Narzędzia Windows**, a następnie kliknij **Eksploratora usługi**. Jeśli możesz już zalogowaniu, monituje możesz to zrobić.

Gdy zostanie wyświetlony widok **Eksploratora Azure** , użyj wykonaj poniższe czynności, aby zatrzymać aplikacji sieci Web: 

1. Rozwiń węzeł **Azure** .

1. Rozwiń węzeł **Aplikacji sieci Web** . 

1. Kliknij prawym przyciskiem myszy żądaną aplikację sieci Web.

1. Gdy pojawi się menu kontekstowego, kliknij przycisk **Otwórz w przeglądarce**.

    ![][17]

## <a name="updating-your-web-app"></a>Aktualizowanie aplikacji sieci Web

Aktualizowanie istniejącego uruchamiania aplikacji sieci Web Azure to proces szybkiego i łatwego i masz dwie opcje aktualizacji:

* Wdrażanie aplikacji sieci Web programu istniejących Java można aktualizować.
* Możesz opublikować dodatkowe aplikacji Java w kontenerze samej aplikacji sieci Web.

W obu przypadkach proces jest identyczny i trwa tylko kilka sekund:

1. W oknie Eksploratora projektu IntelliJ kliknij prawym przyciskiem myszy aplikacji Java, który chcesz zaktualizować lub dodać do istniejącego kontenera aplikacji sieci Web.

1. Po wyświetleniu menu kontekstowego wybierz **Azure** , a następnie **Publikuj jako aplikacji sieci Web Azure...**

1. Ponieważ użytkownik już zalogował się wcześniej, pojawi się lista kontenerów istniejącej aplikacji sieci Web. Wybierz ten, który chcesz opublikować lub ponowne publikowanie aplikacji Java i kliknij **przycisk OK**.

Kilka sekund później, w widoku **Dziennika aktywności Azure** będą wyświetlane rozmieszczenia zaktualizowanej jako **opublikowane** i będzie można sprawdzić zaktualizowanej aplikacji w przeglądarce sieci web.

## <a name="starting-or-stopping-an-existing-web-app"></a>Uruchamianie lub zatrzymywanie istniejącej aplikacji sieci Web

Aby uruchomić lub zatrzymać istniejącego kontenera Azure Web App (w tym wszystkie rozmieszczonych aplikacji Java w nim) służy widok **Eksploratora Azure** .

Jeśli widok **Eksploratora Azure** nie jest jeszcze otwarta, możesz go otworzyć, klikając menu **Widok** w IntelliJ, następnie, a następnie kliknij pozycję **Narzędzia Windows**, a następnie kliknij **Eksploratora usługi**. Jeśli możesz już zalogowaniu, monituje możesz to zrobić.

Gdy zostanie wyświetlony widok **Eksploratora Azure** , użyj wykonaj poniższe czynności, aby uruchomić lub zatrzymać aplikacji sieci Web: 

1. Rozwiń węzeł **Azure** .

1. Rozwiń węzeł **Aplikacji sieci Web** . 

1. Kliknij prawym przyciskiem myszy żądaną aplikację sieci Web.

1. Po wyświetleniu menu kontekstowego, kliknij polecenie **Uruchom** lub **Zatrzymaj**. Należy zauważyć, że opcji menu kontekstowego obsługujących, aby można było tylko zatrzymanie uruchomionego aplikacji sieci web i uruchomić aplikację sieci web, która nie jest uruchomiony.

    ![][18]

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji na temat narzędzi Azure dla języka Java IDEs zobacz poniższe łącza:

- [Azure zestaw narzędzi dla programu Eclipse]
  - [Instalowanie narzędzi Azure dla programu Eclipse]
  - [Tworzenie aplikacji Web App Witaj świecie dla Azure w Zaćmienie]
  - [Co nowego w Azure zestaw narzędzi dla programu Eclipse]
- [Azure zestaw narzędzi dla IntelliJ]
  - [Instalowanie narzędzi Azure dla IntelliJ]
  - *Tworzenie aplikacji sieci Web dla Witaj świecie dla Azure w IntelliJ (ten artykuł)*
  - [Co nowego w Azure zestaw narzędzi dla IntelliJ]

Aby uzyskać więcej informacji na temat Azure za pomocą języka Java zobacz [Centrum deweloperów języka Java Azure].

Aby uzyskać dodatkowe informacje o tworzeniu Azure aplikacji sieci Web zobacz [Omówienie aplikacji sieci Web].

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure zestaw narzędzi dla programu Eclipse]: ../azure-toolkit-for-eclipse.md
[Azure zestaw narzędzi dla IntelliJ]: ../azure-toolkit-for-intellij.md
[Tworzenie aplikacji Web App Witaj świecie dla Azure w Zaćmienie]: ./app-service-web-eclipse-create-hello-world-web-app.md
[Create a Hello World Web App for Azure in IntelliJ]: ./app-service-web-intellij-create-hello-world-web-app.md
[Instalowanie narzędzi Azure dla programu Eclipse]: ../azure-toolkit-for-eclipse-installation.md
[Instalowanie narzędzi Azure dla IntelliJ]: ../azure-toolkit-for-intellij-installation.md
[Co nowego w Azure zestaw narzędzi dla programu Eclipse]: ../azure-toolkit-for-eclipse-whats-new.md
[Co nowego w Azure zestaw narzędzi dla IntelliJ]: ../azure-toolkit-for-intellij-whats-new.md

[Centrum deweloperów języka Java Azure]: https://azure.microsoft.com/develop/java/
[Omówienie aplikacji sieci Web]: ./app-service-web-overview.md

<!-- IMG List -->

[01]: ./media/app-service-web-intellij-create-hello-world-web-app/01-Web-Page.png
[02]: ./media/app-service-web-intellij-create-hello-world-web-app/02-File-New-Project.png
[03a]: ./media/app-service-web-intellij-create-hello-world-web-app/03-New-Project-Dialog.png
[03b]: ./media/app-service-web-intellij-create-hello-world-web-app/03-No-SDK-Specified.png
[04]: ./media/app-service-web-intellij-create-hello-world-web-app/04-New-Project-Dialog.png
[05]: ./media/app-service-web-intellij-create-hello-world-web-app/05-Open-Index-Page.png
[06]: ./media/app-service-web-intellij-create-hello-world-web-app/06-Azure-Publish-Context-Menu.png
[07]: ./media/app-service-web-intellij-create-hello-world-web-app/07-Azure-Log-In-Dialog.png
[08]: ./media/app-service-web-intellij-create-hello-world-web-app/08-Manage-Subscriptions.png
[09]: ./media/app-service-web-intellij-create-hello-world-web-app/09-App-Containers.png
[10]: ./media/app-service-web-intellij-create-hello-world-web-app/10-Add-App-Container.png
[11]: ./media/app-service-web-intellij-create-hello-world-web-app/11-New-App-Container.png
[12]: ./media/app-service-web-intellij-create-hello-world-web-app/12-New-Resource-Group.png
[13]: ./media/app-service-web-intellij-create-hello-world-web-app/13-New-App-Service-Plan.png
[14]: ./media/app-service-web-intellij-create-hello-world-web-app/14-New-App-Container.png
[15]: ./media/app-service-web-intellij-create-hello-world-web-app/15-Deploy-To-Azure.png
[16]: ./media/app-service-web-intellij-create-hello-world-web-app/16-Progress-Indicator.png
[17]: ./media/app-service-web-intellij-create-hello-world-web-app/17-Browse-Web-App.png
[18]: ./media/app-service-web-intellij-create-hello-world-web-app/18-Stop-Web-App.png
