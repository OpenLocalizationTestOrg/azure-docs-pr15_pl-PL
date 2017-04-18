<properties 
    pageTitle="Debugowanie aplikacji sieci Web Java Azure w Zaćmienie | Microsoft Azure" 
    description="Tym samouczku pokazano, jak za pomocą narzędzi Azure dla programu Eclipse debugowanie aplikacji sieci Web Java uruchomionych Azure." 
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
    ms.date="09/20/2016" 
    ms.author="asirveda;robmcm"/>

# <a name="debug-a-java-web-app-on-azure-in-eclipse"></a>Debugowanie aplikacji sieci Web Java Azure w Zaćmienie

Tym samouczku pokazano sposób debugowania aplikacji sieci Web Java uruchomionych Azure za pomocą [Narzędzi Azure dla programu Eclipse]. W celu uproszczenia prosty przykład strony serwera Java (JSP) będzie używany dla tego samouczka, ale kroki będzie podobna do Java servlet podczas debugowania Azure.

Po wykonaniu tego samouczka, aplikacja będzie wyglądać podobnie do następującej ilustracji, podczas debugowania go w Zaćmienie:

![][01]
 
## <a name="prerequisites"></a>Wymagania wstępne

* Java Developer Kit (JDK), v 1.8 lub nowszym.
* Zaćmienie-IDE dla deweloperów Estonia Java indygo lub nowszym. To można pobrać z <http://www.eclipse.org/downloads/>.
* Rozkład serwera aplikacji, takich jak Apache Tomcat lub molo lub serwera sieci web opartych na Java.
* Azure subskrypcji, które mogą pochodzić z <https://azure.microsoft.com/en-us/free/> lub <http://azure.microsoft.com/pricing/purchase-options/>.
* Azure zestaw narzędzi dla Zaćmienie. Aby uzyskać więcej informacji zobacz [Instalowanie narzędzi Azure dla programu Eclipse].
* Dynamiczne projektu sieci Web utworzona i wdrożony Azure aplikacji usługi; na przykład, zobacz [Tworzenie Witaj świecie aplikacji sieci Web dla Azure w Zaćmienie].

## <a name="to-debug-a-java-web-app-on-azure"></a>Debugowanie aplikacji sieci Web Java Azure

Aby wykonać te kroki w tej sekcji, możesz użyć istniejących dynamiczne projektu sieci Web, które zostały już rozmieszczone aplikacji sieci Web Java Azure, pobieranie [Przykładowych dynamiczne projektu sieci Web] i postępuj zgodnie instrukcjami w sekcji [Tworzenie Witaj świecie aplikacji sieci Web dla Azure w Zaćmienie] wdrożyć Azure. 

1. Otwórz Zaćmienie.

1. Skonfiguruj limity czasu dla zdalnego debugowania:

    1. Kliknij menu **systemu Windows** w Zaćmienie, a następnie kliknij polecenie **Preferencje**.
    1. Rozwiń węzeł **Java** , a następnie wybierz pozycję **Debugowanie**.
    1. Konfigurowanie ustawień zarówno **debugowania limit czasu (ms)** i **Uruchamianie limit czasu (ms)** do `120000`.

        ![][02]

    1. Kliknij **przycisk OK** , aby zamknąć okno dialogowe **Preferencje** .

1. W widoku Eksplorator projektu i Zaćmienie prawym przyciskiem myszy kliknij dynamiczne projektu sieci Web, który został wdrożony Azure. Po wyświetleniu menu kontekstowego wybierz pozycję **Debugowanie jako**, a następnie kliknij **Azure Web App**.

    ![][03]

1. Jeśli po raz pierwszy debugowania dynamiczne projektu sieci Web, zostanie otwarte okno dialogowe **Konfiguracji debugowanie** ; Możesz zaakceptować domyślne wartości, które są określone przez zestaw narzędzi na karcie **Połącz** . Na karcie **źródło** kliknij przycisk **Dodaj**, a następnie **Java projektu**, zaznacz **Dynamiczne projektu sieci Web**, a następnie kliknij **przycisk OK**. Po wykonaniu tych czynności kliknij przycisk **Debugowanie**.

    ![][04]

1. Po wyświetleniu monitu o **Włączanie debugowania zdalnego w aplikacji sieci Web zdalnego teraz?**, kliknij **przycisk OK**.

1. Po wyświetleniu monitu tej **aplikacji sieci web jest teraz gotowy do zdalnego debugowania**, kliknij **przycisk OK**.

    ![][05]

1. Gdy pojawi się okno dialogowe **Debugowanie konfiguracji** , kliknij przycisk **Debugowanie**.

1. Wiersz polecenia systemu Windows lub powłoki systemu Unix będą otwierane i przygotowywanie połączenia niezbędne do debugowania. Musisz poczekaj, aż połączenie do aplikacji sieci Java Web zdalnego zostanie nawiązane, przed kontynuowaniem. Jeśli korzystasz z systemu Windows, będzie wyglądać jak na poniższej ilustracji.

    ![][06]

1. Wstawianie punktu podziału na stronie JSP, a następnie otwórz adres URL dla aplikacji sieci Web języka Java w przeglądarce:

    1. Otwórz **Eksploratora Azure** w Zaćmienie w górę.
    1. Przejdź do **Aplikacji sieci Web** i aplikacji sieci Web Java chcesz debugowanie.
    1. Kliknij prawym przyciskiem myszy w aplikacji sieci Web, a następnie kliknij przycisk **Otwórz w przeglądarce**.
    1. Zaćmienie będzie teraz wprowadzić w tryb debugowania.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji na temat Azure za pomocą języka Java zobacz [Centrum deweloperów języka Java Azure].

Aby uzyskać dodatkowe informacje o tworzeniu Azure aplikacji sieci Web zobacz [Omówienie aplikacji sieci Web].

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[Azure zestaw narzędzi dla programu Eclipse]: ../azure-toolkit-for-eclipse.md
[Instalowanie narzędzi Azure dla programu Eclipse]: ../azure-toolkit-for-eclipse-installation.md
[Tworzenie aplikacji Web App Witaj świecie dla Azure w Zaćmienie]: ./app-service-web-eclipse-create-hello-world-web-app.md
[Przykładowy projekt dynamiczne sieci Web]: http://go.microsoft.com/fwlink/?LinkId=817337

[Centrum deweloperów języka Java Azure]: https://azure.microsoft.com/develop/java/
[Omówienie aplikacji sieci Web]: ./app-service-web-overview.md

<!-- IMG List -->

[01]: ./media/app-service-web-debug-java-web-app-in-eclipse/01-debug-java-web-app-in-eclipse.png
[02]: ./media/app-service-web-debug-java-web-app-in-eclipse/02-configure-eclipse-remote-debug.png
[03]: ./media/app-service-web-debug-java-web-app-in-eclipse/03-debug-as.png
[04]: ./media/app-service-web-debug-java-web-app-in-eclipse/04-debug-configurations.png
[05]: ./media/app-service-web-debug-java-web-app-in-eclipse/05-ready-for-remote-debugging.png
[06]: ./media/app-service-web-debug-java-web-app-in-eclipse/06-windows-command-prompt-connection-successful-to-remote.png
