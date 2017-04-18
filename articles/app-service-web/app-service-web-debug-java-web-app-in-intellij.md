<properties 
    pageTitle="Debugowanie aplikacji sieci Web Java Azure w IntelliJ | Microsoft Azure" 
    description="Tym samouczku pokazano, jak za pomocą narzędzi Azure dla IntelliJ debugowanie aplikacji sieci Web Java uruchomionych Azure." 
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

# <a name="debug-a-java-web-app-on-azure-in-intellij"></a>Debugowanie aplikacji sieci Web Java Azure w IntelliJ

Tym samouczku pokazano sposób debugowania aplikacji sieci Web Java uruchomionych Azure za pomocą [Narzędzi Azure dla IntelliJ]. W celu uproszczenia prosty przykład strony serwera Java (JSP) będzie używany dla tego samouczka, ale kroki będzie podobna do Java servlet podczas debugowania Azure.

Po wykonaniu tego samouczka, aplikacja będzie wyglądać podobnie do następującej ilustracji, podczas debugowania go w IntelliJ:

![][01]
 
## <a name="prerequisites"></a>Wymagania wstępne

* Java Developer Kit (JDK), v 1.8 lub nowszym.
* IntelliJ POMYSŁU Ultimate Edition. To można pobrać z <https://www.jetbrains.com/idea/download/index.html>.
* Rozkład serwera aplikacji, takich jak Apache Tomcat lub molo lub serwera sieci web opartych na Java.
* Azure subskrypcji, które mogą pochodzić z <https://azure.microsoft.com/en-us/free/> lub <http://azure.microsoft.com/pricing/purchase-options/>.
* Azure zestaw narzędzi IntelliJ. Aby uzyskać więcej informacji zobacz [Instalowanie narzędzi Azure dla IntelliJ].
* Dynamiczne projektu sieci Web utworzona i wdrożony Azure aplikacji usługi; na przykład, zobacz [Tworzenie Witaj świecie aplikacji sieci Web dla Azure w IntelliJ].

## <a name="to-debug-a-java-web-app-on-azure"></a>Debugowanie aplikacji sieci Web Java Azure

Do wykonania tych kroków w tej sekcji, możesz użyć istniejących dynamiczne projektu sieci Web, które zostały już rozmieszczone aplikacji sieci Web Java Azure, pobrać [Przykładowy dynamiczne projektu sieci Web] i postępuj zgodnie instrukcjami [Utwórz Witaj świecie aplikacji sieci Web dla Azure w IntelliJ] , aby wdrożyć go na Azure. 

1. Otwórz projekt języka Java aplikacji sieci Web, której wdrożony Azure w IntelliJ.

1. Kliknij menu **Uruchom** , a następnie kliknij pozycję **Edytuj konfiguracji**.

    ![][02]

1. Gdy zostanie otwarte okno dialogowe **Konfiguracji Uruchom-debugowania** : 

    1. Wybierz pozycję **Azure Web App**.
    1. Polecenie **+** Aby dodać nową konfigurację.
    1. Podaj **nazwę** dla konfiguracji.
    1. Zaakceptuj pozostałe wartości domyślne, które są wyświetlane sugerowane przez Azure zestaw narzędzi, a następnie kliknij **przycisk OK**.

        ![][03]

1. Wybierz konfigurację debugowania Azure Web App, która została właśnie utworzona w prawym górnym rogu menu, a następnie wybierz polecenie **Debugowanie**

    ![][04]

1. Po wyświetleniu monitu o **Włączanie debugowania zdalnego w aplikacji sieci Web zdalnego teraz?**, kliknij **przycisk OK**.

1. Po wyświetleniu monitu tej **aplikacji sieci web jest teraz gotowy do zdalnego debugowania**, kliknij **przycisk OK**.

    ![][05]

1. Wybierz konfigurację debugowania Azure aplikacji sieci Web, która została właśnie utworzona w prawym górnym rogu strony menu, a następnie kliknij na **Debugowanie**.

1. Wiersz polecenia systemu Windows lub powłoki systemu Unix będą otwierane i przygotowywanie połączenia niezbędne do debugowania. Musisz poczekaj, aż połączenie do aplikacji sieci Java Web zdalnego zostanie nawiązane, przed kontynuowaniem. Jeśli korzystasz z systemu Windows, będzie wyglądać jak na poniższej ilustracji:

    ![][06]

1. Wstawianie punktu podziału na stronie JSP, a następnie otwórz adres URL dla aplikacji sieci Web języka Java w przeglądarce:

    1. Otwórz **Eksploratora Azure** w IntelliJ w górę.
    1. Przejdź do **Aplikacji sieci Web** i aplikacji sieci Web Java chcesz debugowanie.
    1. Kliknij prawym przyciskiem myszy w aplikacji sieci Web, a następnie kliknij przycisk **Otwórz w przeglądarce**.
    1. IntelliJ teraz rozpoczną tryb debugowania.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji na temat Azure za pomocą języka Java zobacz [Centrum deweloperów języka Java Azure].

Aby uzyskać dodatkowe informacje o tworzeniu Azure aplikacji sieci Web zobacz [Omówienie aplikacji sieci Web].

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[Azure zestaw narzędzi dla IntelliJ]: ../azure-toolkit-for-intellij.md
[Instalowanie narzędzi Azure dla IntelliJ]: ../azure-toolkit-for-intellij-installation.md
[Tworzenie aplikacji sieci Web dla Witaj świecie dla Azure w IntelliJ]: ./app-service-web-intellij-create-hello-world-web-app.md
[Przykładowy projekt dynamiczne sieci Web]: http://go.microsoft.com/fwlink/?LinkId=817337

[Centrum deweloperów języka Java Azure]: https://azure.microsoft.com/develop/java/
[Omówienie aplikacji sieci Web]: ./app-service-web-overview.md

<!-- IMG List -->

[01]: ./media/app-service-web-debug-java-web-app-in-intellij/01-debug-java-web-app-in-intellij.png
[02]: ./media/app-service-web-debug-java-web-app-in-intellij/02-configure-intellij-remote-debug.png
[03]: ./media/app-service-web-debug-java-web-app-in-intellij/03-debug-configuration.png
[04]: ./media/app-service-web-debug-java-web-app-in-intellij/04-select-debug.png
[05]: ./media/app-service-web-debug-java-web-app-in-intellij/05-ready-for-remote-debugging.png
[06]: ./media/app-service-web-debug-java-web-app-in-intellij/06-windows-command-prompt-connection-successful-to-remote.png
