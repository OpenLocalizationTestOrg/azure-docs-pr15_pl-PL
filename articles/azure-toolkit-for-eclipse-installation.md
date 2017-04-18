<properties
    pageTitle="Instalowanie narzędzi Azure Zaćmienie | Microsoft Azure"
    description="Dowiedz się, jak zainstalować pakiet Azure dla programu Eclipse."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690946.aspx -->

# <a name="installing-the-azure-toolkit-for-eclipse"></a>Instalowanie narzędzi Azure dla programu Eclipse

Zestaw narzędzi Azure dla Zaćmienie dostępnych szablonów i funkcje, które umożliwiają łatwe tworzenie, opracowywanie, testowanie i wdrażanie aplikacji Azure za pomocą Zaćmienie środowisko projektowania. Zestaw narzędzi Azure dla Zaćmienie jest projektem Otwórz źródło, którego kod źródłowy jest dostępny w obszarze licencja z witryny projektu na GitHub pod adresem URL:

<https://github.com/Microsoft/Azure-Tools-for-Java>

Poniższe kroki pokazują jak zainstalować pakiet Azure dla programu Eclipse.

[AZURE.INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="to-install-the-azure-toolkit-for-eclipse"></a>Aby zainstalować pakiet Azure dla programu Eclipse

1. Rozpocznij Zaćmienie.

1. Po otwarciu Zaćmienie kliknij menu **Pomoc** , a następnie kliknij pozycję **Zainstaluj nowe oprogramowanie**, jak pokazano na poniższej ilustracji.

    ![Instalowanie narzędzi Azure dla programu Eclipse][01]

1. W oknie dialogowym **Dostępnego oprogramowania** , w polu tekstowym **Praca z** wpisz **http://dl.microsoft.com/eclipse** , a następnie klawisz **Enter** .

1. W okienku **Nazwa** zaznacz **Azure zestaw narzędzi dla Zaćmienie**, a następnie wyczyść pole wyboru **kontaktu Aktualizuj wszystkie witryny podczas instalacji znajdowanie oprogramowanie wymagane**. Ekranu powinna wyglądać podobnie do następującej:

    ![Instalowanie narzędzi Azure dla programu Eclipse][02]

1. Jeśli rozwiniesz **Azure zestaw narzędzi dla Zaćmienie**pojawią się następujące elementy:

    * **Dodatek wniosków aplikacji dla języka Java**: ten składnik umożliwia korzystanie przez telemetrycznego rejestrowania i analizy usługi Azure dla aplikacji i wystąpienia serwera.
    * **Filtr usług kontroli dostępu Azure**: ten składnik zapewnia obsługę uwierzytelniania użytkowników aplikacji przy użyciu Azure ACS, umożliwienie realizacji scenariuszy rejestracji jednokrotnej i umieszczeniu logiczny tożsamości z poziomu aplikacji.
    * **Typowe wtyczki Azure**: ten składnik zapewnia często używane funkcje wymagane przez inne składniki zestaw narzędzi.
    * **Eksplorator Azure dla programu Eclipse**: ten składnik zapewnia często używane funkcje wymagane przez inne składniki zestaw narzędzi.
    * **Azure dodatek dla programu Eclipse przy użyciu języka Java**: ten składnik zapewnia pomocy technicznej dotyczącej opracowywania projektów, które pomagają w tworzeniu, testowanie i wdrażanie aplikacji Java chmury Microsoft Azure w Zaćmienie i wiersza polecenia.
    * **Wtyczka aplikacje Web Azure przy użyciu języka Java**: ten składnik obsługuje wdrażanie aplikacji sieci web Java kontenerów aplikacji sieci Web usługi Microsoft Azure.
    * **4.2 sterownik JDBC firmy Microsoft dla programu SQL Server**: ten składnik zapewnia JDBC interfejsu API dla programu SQL Server i Microsoft Azure SQL Database dla języka Java Platform Enterprise Edition 8.
    * **Pakiet Apache Qpid klienta bibliotek JMS**: ten składnik zapewnia składnika klienta JMS z projektem Apache Qpid, aby włączyć aplikację, aby korzystać z AMQP wiadomości w programie Microsoft Azure.
    * **Pakiet Microsoft Azure bibliotek Java**: ten składnik zapewnia interfejsy API uzyskiwania dostępu do usługi Microsoft Azure, takie jak magazyn, usługa bus, środowisko uruchomieniowe usługi itp.

1. Kliknij przycisk **Dalej**. (Jeśli opóźnień nietypowe podczas instalacji z zestawu narzędzi, upewnij się, że **kontakt Aktualizuj wszystkie witryny podczas instalacji znajdowanie oprogramowanie wymagane** jest zaznaczona pozycja.)

1. W oknie dialogowym **Szczegóły instalacji** kliknij przycisk **Dalej**.

    ![Przejrzyj szczegółowe informacje dotyczące instalacji][03]

1. W oknie dialogowym **Przeglądanie licencji** zapoznaj się z warunkami umowy licencyjnej. Jeśli zaakceptować postanowienia Umowy licencyjnej, kliknij pozycję **akceptuję postanowienia Umowy licencyjnej** , a następnie kliknij przycisk **Zakończ**. (Pozostałe kroki założono, że zaakceptować postanowienia Umowy licencyjnej. Jeśli nie zaakceptujesz warunków umowy licencyjnej, Zakończ proces instalacji.)

    ![Przeglądanie licencji][04]

    Zaćmienie będzie pobrać i zainstalować pakiety wymagane.

    ![Postęp instalacji][05]

1. Jeśli zostanie wyświetlony monit o ponowne uruchomienie Zaćmienie, aby ukończyć instalację, kliknij przycisk **Tak**.

    ![Uruchom ponownie wiersza][06]

## <a name="see-also"></a>Zobacz też

Aby uzyskać więcej informacji na temat narzędzi Azure dla języka Java IDEs zobacz poniższe łącza:

- [Azure zestaw narzędzi dla programu Eclipse]
  - *Instalowanie narzędzi Azure Zaćmienie (ten artykuł)*
  - [Tworzenie aplikacji Web App Witaj świecie dla Azure w Zaćmienie]
  - [Co nowego w Azure zestaw narzędzi dla programu Eclipse]
- [Azure zestaw narzędzi dla IntelliJ]
  - [Instalowanie narzędzi Azure dla IntelliJ]
  - [Tworzenie aplikacji sieci Web dla Witaj świecie dla Azure w IntelliJ]
  - [Co nowego w Azure zestaw narzędzi dla IntelliJ]

Aby uzyskać więcej informacji na temat Azure za pomocą języka Java zobacz [Centrum deweloperów języka Java Azure].

<!-- URL List -->

[Azure zestaw narzędzi dla programu Eclipse]: ./azure-toolkit-for-eclipse.md
[Azure zestaw narzędzi dla IntelliJ]: ./azure-toolkit-for-intellij.md
[Tworzenie aplikacji Web App Witaj świecie dla Azure w Zaćmienie]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Tworzenie aplikacji sieci Web dla Witaj świecie dla Azure w IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Installing the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Instalowanie narzędzi Azure dla IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Co nowego w Azure zestaw narzędzi dla programu Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Co nowego w Azure zestaw narzędzi dla IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Centrum deweloperów języka Java Azure]: https://azure.microsoft.com/develop/java/

<!-- IMG List -->

[01]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-01.png
[02]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-02.png
[03]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-03.png
[04]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-04.png
[05]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-05.png
[06]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-06.png

