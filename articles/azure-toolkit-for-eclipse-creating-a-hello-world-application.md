<properties
    pageTitle="Tworzenie Witaj świecie usługa w chmurze dla Azure w Zaćmienie"
    description="Dowiedz się, jak utworzyć prostą aplikację Witaj świecie przy użyciu narzędzi Azure dla Zaćmienie."
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

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690944.aspx -->

# <a name="create-a-hello-world-cloud-service-for-azure-in-eclipse"></a>Tworzenie Witaj świecie usługa w chmurze dla Azure w Zaćmienie #

Poniższe kroki pokazano, jak tworzyć i wdrażać podstawowe aplikacji JSP Azure za pomocą narzędzi Azure dla Zaćmienie. Przykład JSP są wyświetlane w celu uproszczenia, ale bardzo podobne kroki będzie servlet Java chodzi Azure wdrożenia jest.

Aplikacja będzie wyglądać podobnie do następującej:

![][ic600360]

## <a name="prerequisites"></a>Wymagania wstępne ##

* Java Developer Kit (JDK), v 1.7 lub nowszej.
* Zaćmienie-IDE dla deweloperów Estonia Java indygo lub nowszym. To można pobrać z <http://www.eclipse.org/downloads/>.
* Rozkład serwera sieci web opartych na Java lub serwera aplikacji, takich jak Apache Tomcat, GlassFish, serwer aplikacji JBoss, molo lub IBM® WebSphere® aplikacji Server wolności Core.
* Azure subskrypcji, które mogą pochodzić z <http://azure.microsoft.com/pricing/purchase-options/>.
* Azure zestaw narzędzi dla Zaćmienie. Aby uzyskać więcej informacji zobacz [Instalowanie narzędzi Azure dla programu Eclipse][].

## <a name="to-create-a-hello-world-application"></a>Aby utworzyć aplikację Witaj świecie ##

Najpierw Zaczniemy od tworzenia projektu języka Java.

*  Rozpoczynanie Zaćmienie i w menu kliknij pozycję **plik**, kliknij pozycję **Nowy**, a następnie kliknij **Dynamiczne projektu sieci Web**. (Jeśli nie widzisz **Dynamiczne projektu sieci Web** na liście jako projekt dostępne po kliknięciu pozycji **plik**, **Nowy**, wykonaj następujące czynności: kliknij pozycję **plik**, kliknij przycisk **Nowy**, kliknij pozycję **Projekt...**, rozwiń **sieci Web**, kliknij pozycję **Project Web dynamiczne**i kliknij przycisk **Dalej**.)
*  Na potrzeby tego samouczka nazwę projektu **MyHelloWorld**. (Upewnij się, możesz użyć tej nazwy, kolejne kroki w tym samouczku dowiesz się spodziewać pliku też nosić nazwę MyHelloWorld). Na ekranie pojawią się podobny do następującego: ![][ic589576]
* Kliknij przycisk **Zakończ**.
* W widoku Eksplorator projektu i Zaćmienie rozwiń **MyHelloWorld**. Kliknij prawym przyciskiem myszy **sieć Web, zawartość**, kliknij pozycję **Nowy**, a następnie kliknij **Plik JSP**.
* W oknie dialogowym **Nowy plik JSP** nazwę pliku **index.jsp**. Zachowaj folder nadrzędny jako **MyHelloWorld-sieć Web, zawartość**, jak pokazano poniżej:  ![][ic659262]
* W oknie dialogowym **Wybierz szablon JSP** na potrzeby tego samouczka wybierz **Nowy plik JSP (html)** , a następnie kliknij przycisk **Zakończ**.
* Po otwarciu pliku index.jsp w Zaćmienie, dodawanie tekstu do dynamicznego wyświetlania **Witaj świecie!** w istniejącej `<body>` elementu. Usługi zaktualizowanych `<body>` zawartości powinien być wyświetlany jako następujące czynności:
```
    <body>
    <b><% out.println("Hello World!"); %></b>
    </body>
```
* Zapisz index.jsp.

## <a name="to-deploy-your-application-to-azure-the-quick-and-simple-way"></a>Aby wdrożyć aplikację, aby Azure, szybki i łatwy sposób ##

Jak masz Java aplikacji sieci web przystąpić do testowania można wypróbowaniem jej bezpośrednio w chmurze Azure za pomocą następujących skrótów.

1. W oknie Project Explorer Zaćmienie osoby kliknij przycisk **MyHelloWorld**.
1. Na pasku narzędzi Zaćmienie kliknij przycisk **Publikuj** , a następnie kliknij przycisk **Publikuj jako Azure usługi w chmurze**
    ![][publishDropdownButton]
1. Jeśli publikujesz tej aplikacji Azure po raz pierwszy, nie utworzono projektu wdrażania Azure dla tej aplikacji przed projektu wdrażania Azure jest tworzony automatycznie. Powinien zostać wyświetlony następujący monit, zawierającego pakiet JDK i serwer aplikacji, która zostanie automatycznie rozmieszczony na uruchomienie aplikacji.
    ![][ic789598]

    Tej metody skrótów umożliwia szybki i łatwy sposób testowania aplikacji platformy Azure bez konieczności konfigurowania określonego serwera lub JDK, który różni się od ustawienia domyślne. Po zakończeniu wartości domyślne można kliknij **przycisk OK** , aby kontynuować poniższe czynności.
    Jednak jeśli chcesz zmienić JDK lub serwer aplikacji dla aplikacji, później to zrobić, edytując projektu Azure wdrażania, który został utworzony automatycznie dla Ciebie, lub kliknij przycisk **Anuluj** i przeczytaj **temat Azure wdrażania projektów sekcji** tego samouczka.
1. W oknie dialogowym **Publikowanie Azure** :
    1. Jeśli istnieją nie subskrypcji usługi wybierz z listy **subskrypcji** jeszcze, wykonaj poniższe czynności, aby zaimportować informacje o Twojej subskrypcji:
        1. Kliknij pozycję **Importuj z pliku ustawienia publikowania**.
        1. W oknie dialogowym **Informacje o subskrypcji importu** kliknij pozycję **Pobierz plik ustawień publikowania**. Jeśli nie masz jeszcze zalogowany do konta usługi Azure, zostanie wyświetlony monit o zalogowanie się. Następnie zostanie wyświetlony monit Zapisz Azure publikowanie plik ustawień. Zapisz go na komputerze lokalnym.
        1. Nadal w oknie dialogowym **Informacje o subskrypcji importu** kliknij przycisk **Przeglądaj** , wybierz plik z ustawieniami publikowania zapisanego lokalnie w poprzednim kroku, a następnie kliknij przycisk **Otwórz**. Na ekranie powinna wyglądać podobnie do następującej: ![][ic644267]
        1. Kliknij **przycisk OK**.
    1. **Subskrypcji**Wybierz subskrypcję, którego chcesz użyć do wdrożenia.
    1. **Konta miejsca do magazynowania**wybierz konto miejsca do magazynowania, który ma być używany, lub kliknij przycisk **Nowy** , aby utworzyć nowe konto miejsca do magazynowania.
    1. W polu **Nazwa usługi**zaznacz usługi w chmurze, której chcesz użyć, lub kliknij przycisk **Nowy** , aby utworzyć nowy usługi w chmurze.
    1. W **Docelowej OS**wybierz wersję systemu operacyjnego, który ma być używany dla wdrożenia.
    1. W **środowisku docelowym**na potrzeby tego samouczka wybierz **tymczasowego**. (Gdy wszystko będzie już gotowe do wdrożenia do witryny produkcji, zmienisz to **produkcji**.)
    1. Opcjonalnie: Upewnij się, że **zastąpienie poprzedniego wdrożenia** jest zaznaczone, jeśli chcesz, aby nowe wdrożenia można automatycznie zapisywać poprzedniego wdrożenia. Jeśli włączysz tę opcję, będzie nie obsługi "409 Konflikt" problemy podczas publikowania w tej samej lokalizacji.
        Zauważ, że okno dialogowe **Publikowanie Azure** zawiera sekcji dla **Dostępu zdalnego**. Domyślnie dostęp zdalny nie jest włączony i firma Microsoft nie umożliwi go w tym przykładzie. Aby włączyć dostęp zdalny, musisz wprowadzić nazwę użytkownika i hasło używane podczas logowania zdalnie. Aby uzyskać więcej informacji na temat dostępu zdalnego Zobacz [Włączanie dostępu zdalnego wdrożeniach Azure w Zaćmienie][].
        Okno dialogowe usługi **Publikowanie Azure** pojawią się podobny do następującego: ![][ic719488]
1. Kliknij przycisk **Publikuj** publikowanie środowiska przemieszczania.
    Gdy zostanie wyświetlony monit, aby wykonać pełne kompilację, kliknij przycisk **Tak**. To może potrwać kilka minut dla pierwszej kompilacji.
    **Dziennik Azure** będzie wyświetlany w sekcji Widoki Zaćmienie na kartach.
    ![][ic719489]
   Za pomocą tego dziennika, a także widok **konsoli** , aby wyświetlić postęp wdrożenia. Alternatywny jest zalogować się do [Portalu zarządzania Azure][]i monitorowanie stanu za pomocą sekcji **Usług w chmurze** .
1. Po pomyślnym wdrożeniu wdrożenia **Azure dziennik** pokazuje stan **opublikowany**. Kliknij pozycję **opublikowany**, jak pokazano na poniższej ilustracji i przeglądarki zostanie otwarte wystąpienie rozmieszczenia.
    ![][ic719490]

Ponieważ znajdowała się wdrożenia w środowisku wzorcowym, nazwa DNS będzie w formie http://&lt;*guid*&gt;. cloudapp.net i będzie adres URL zawiera nazwę DNS plus sufiksu dla aplikacji. Na przykład http://447564652c20426f6220526f636b7321.cloudapp.net/MyHelloWorld. (Część **MyHelloWorld** jest uwzględniana wielkość liter). Można również zawiera nazwę DNS, jeśli użytkownik kliknie nazwę wdrożenia w portalu zarządzania platformą Azure (w obszarze usług w chmurze część portalu zarządzania).

Mimo że ten samouczek był wdrożenia do środowiska wzorcowego, wdrożenia do produkcji obejmuje te same kroki, z wyjątkiem w oknie dialogowym **Publikowanie Azure** , zaznacz **produkcji** zamiast **tymczasowej** dla **środowiska docelowego**. Wdrożenie produkcji wyników w adresie URL na podstawie nazwy DNS wybranych przez użytkownika, zamiast GUID używanej do tymczasowego.

>[AZURE.WARNING] W tym momencie wdrożono Azure aplikacji w chmurze. Jednak przed kontynuowaniem sobie sprawę, że wdrożonej aplikacji, nawet jeśli nie jest uruchomiony, będą nadal Naliczanie czasu fakturowanego dla subskrypcji. W związku z tym, niezwykle ważne jest usuwanie niechcianych wdrożeń z subskrypcji Azure.

## <a name="about-azure-deployment-projects"></a>Projekty Azure wdrożenia — informacje ##

Aby wdrożyć jedną lub więcej aplikacji Java Azure, potrzebna jest Azure wdrożenia projektu. Był odtwarzany roli "pakiet" potrzebnej aplikacji, które zostaną zawinięte w celu publikowane Azure.

Oprócz informacji o aplikacjach, Azure wdrożenia programu project zawiera także informacje o innych kluczowe składniki wdrażania, najważniejsze: kontener serwera aplikacji do uruchamiania aplikacji sieci web w oraz środowisko uruchomieniowe Java, aby go uruchomić. Azure obsługuje dużej liczby programów Java i serwerach aplikacji Java, których można wybierać spośród.

Mimo że przykładowe używane w tym miejscu jest znacznie uproszczone w celach szkoleniowych, projektu wdrażania Azure może również zawierać inne ważne informacje o konfiguracji umożliwia tworzenie usług w chmurze niemal dowolnie złożone, skalowalna, wysokiej dostępności, wielu z aplikacjami. Możesz włączyć **koligacji sesji ("lepkich sesje")**, **Szybki pamięci podręcznej**, **zdalne debugowanie**, **odciążanie protokołu SSL**, **Zapora/port routingu**, **dostępu zdalnego**i wiele innych zaawansowanych funkcji.

Jeśli zostały ukończone poprzedniej sekcji tego samouczka ("do wdrażania aplikacji Azure, szybki i łatwy sposób"), zostanie wyświetlona nowego projektu wdrożenia Azure w oknie Project Explorer generowane automatycznie dla Ciebie i o nazwie "**MyHelloWorld_onAzure**".

Można również uruchomieniu tego samouczka, najpierw tworzenia pustej Azure wdrożenia projektu samodzielnie, a następnie dodając do aplikacji do niego. Jest dłużej proces, ale które zapewnia większą kontrolę nad początkowej konfiguracji od początku.

Aby utworzyć nowy projekt wdrożenia Azure od podstaw, kliknij przycisk **Nowy Projekt wdrożenia Azure** ![][ic710876].

Niezależnie od tego, czy są Praca z już istniejącego projektu wdrożenia Azure lub tworzenie od podstaw, można zmienić jego ustawienia konfiguracji i elementów, takich jak JDK lub serwera aplikacji jednakowo jest łatwe w dowolnym momencie.

Aby zmienić JDK, lub serwera aplikacji lub na liście aplikacji w istniejącym projekcie Azure wdrażania:

1. Rozwiń węzeł projektu (np. **MyHelloWorld_onAzure**) w oknie Eksploratora projektu
2. Kliknij prawym przyciskiem myszy **WorkerRole1**
3. Rozwinięcie menu **Azure** w menu kontekstowym
4. Kliknij pozycję **konfiguracji serwera**

Niezależnie od tego, czy rozpoczęto poniższe kroki konfiguracji serwera, edytując istniejącego projektu Azure wdrażania, jak pokazano powyżej lub tworzenia nowej witryny od podstaw, pojawi się ten sam rodzaj okien dialogowych umożliwia konfigurowanie usługi JDK, składniki serwera i aplikacji. Aby dowiedzieć się więcej na temat zmiany ustawień w tych oknach dialogowych, na przykład zmienić JDK, serwer aplikacji i dodawanie lub usuwanie aplikacji we wdrożeniu, zobacz artykuł [Właściwości konfiguracji serwera][] .

## <a name="windows-only-to-deploy-your-application-to-the-compute-emulator"></a>Tylko w systemie Windows: wdrażania aplikacji do emulatora obliczeń ##

>[AZURE.NOTE] Azure emulatora jest dostępna tylko w systemie Windows. Jeśli korzystasz z systemu operacyjnego innego niż Windows, należy pominąć tę sekcję.

Po utworzeniu nowego projektu wdrożenia Azure, wykonując kroki opisane wcześniej, to znaczy niejawnie przez opublikowanie aplikacji Azure, JDK i stosowanie serwery zostały skonfigurowane do chmury, ale nie do lokalnych emulacji. Aby przygotować projektu badań w emulatorze lokalnych, wykonaj następujące czynności:

1. W oknie Project Explorer Zaćmienie osoby kliknij przycisk **MyHelloWorld_onAzure**.
1. Kliknij prawym przyciskiem myszy **WorkerRole1**.
1. Rozwinięcie menu **Azure** w menu kontekstowym.
1. Kliknij pozycję **konfiguracji serwera**.
1. Na karcie **JDK** zaznacz, jeśli pakiet zawiera wstępnie skonfigurowane jest domyślne lokalne JDK dla Ciebie. Jeśli nie lub jeśli chcesz zmienić założonej wartości domyślnych, upewnij się, że jest zaznaczone pole wyboru **Użyj JDK z ta ścieżka pliku do testowania lokalnie** i określono lokalizację JDK, którego chcesz użyć. Jeśli chcesz go zmienić, kliknij przycisk **Przeglądaj** i za pomocą przycisk Przeglądaj, wybierz lokalizację katalogu JDK korzystać.
1. Kliknij kartę **serwer** .
1. W polu tekstowym **ścieżka lokalnego serwera** w dolnej części okna dialogowego wpisz ścieżkę serwera zainstalowana lokalnie, odpowiadający typ i numer wersji głównych serwer wybrany w górnej części okna dialogowego w obszarze pole wyboru **Wdrażanie serwera tego typu** . Jeśli chcesz użyć innego typu lub wersja główna serwera aplikacji, należy najpierw zmienić wyboru w obszarze tego pola wyboru.
1. Kliknij **przycisk OK**.
1. Na pasku narzędzi Zaćmienie kliknij przycisk **Uruchom w emulatora Azure** ![][ic710879]. Jeśli przycisku **Uruchom w emulatora Azure** nie jest włączona, upewnij się, że **MyHelloWorld_onAzure** jest zaznaczone w oknie Project Explorer Zaćmienie osoby i upewnij się, że Eksplorator projektu i Zaćmienie ma fokus w bieżącym oknie. Najpierw zostanie rozpoczęcie pełnej kompilacji projektu i następnie uruchamianie aplikacji sieci web języka Java w emulatorze obliczeń. (Uwaga w zależności od cech wydajności komputera, tworzenie pierwszej może potrwać między kilka sekund kilka minut, że kolejne tworzy otrzymają szybciej.) Po zakończeniu kompilacji najpierw możesz zostanie wyświetlony monit, systemu Windows (Kontrola konta) umożliwia to polecenie, aby wprowadzić zmiany na komputerze. Kliknij przycisk **Tak**.

>[AZURE.IMPORTANT] Jeśli nie widać na monit funkcji Kontrola konta użytkownika, sprawdź na pasku zadań systemu Windows dla ikony funkcji Kontrola konta użytkownika i kliknij je najpierw. Czasami funkcji Kontrola konta użytkownika monit nie jest wyświetlany jako oknie, ale jest widoczna tylko jako ikona na pasku zadań.

1. Przejrzyj wyniki obliczeń emulatora interfejsu użytkownika, aby określić, czy istnieją jakiekolwiek problemy z projektem. W zależności od zawartości rozmieszczenia może potrwać kilka minut na aplikację, aby w pełni można uruchomić w emulatorze obliczeń.
1. Uruchom przeglądarkę i użyj adresu URL `http://localhost:8080/MyHelloWorld` jako adres ( `MyHelloWorld` część adresu URL jest uwzględniana wielkość liter). Powinien zostać wyświetlony aplikacji MyHelloWorld (wyjście index.jsp), podobne na poniższej ilustracji: ![][ic589579]

Gdy chcesz zatrzymać aplikacji w programie emulatorze obliczeń na pasku narzędzi Zaćmienie kliknij przycisk **Resetuj emulatora Azure** ![][ic710880].

## <a name="to-delete-your-deployment"></a>Aby usunąć wdrożenia ##

Aby usunąć z wdrożeniem w ramach zestawu narzędzi Azure dla programu Eclipse, upewnij się, że **MyHelloWorld_onAzure** jest zaznaczone w oknie Project Explorer Zaćmienie osoby, upewnij się, Eksplorator projektu Zaćmienie występują bieżącego okna skoncentrowanie, a następnie kliknij przycisk **Cofnij publikowanie** ![][ic710883], na pasku narzędzi Zaćmienie. (Prawym przyciskiem myszy **MyHelloWorld_onAzure** w jego Zaćmienie Eksplorator projektu, klikając pozycję **Azure** , a następnie klikając polecenie **Undeploy z chmury Azure**może wykonać tę samą operację). Zostanie wyświetlone okno dialogowe **Cofanie publikowania projektu Azure** .

![][ic719491]

Wybierz usługę subskrypcji i w chmurze, zawierającą wdrożenia, wybierz pozycję wdrożenia, który chcesz usunąć, a następnie kliknij **Cofnij publikowanie**.

(Zamiast korzystanie z zestawu narzędzi, aby usunąć wdrożenia jest użycie sekcji **Usług w chmurze** do portalu zarządzania Azure: Przejdź do wdrożenia, zaznacz go, a następnie kliknij przycisk **Usuń** . To będzie zatrzymać, a następnie usuń, wdrażania. Jeśli tylko chcesz zatrzymać wdrożenia, nie usuwając go, kliknij przycisk **Zatrzymaj** zamiast przycisku **Usuń** , ale jak wcześniej wspomniano, jeśli nie powoduje usunięcia wdrożenia, opłaty rozliczaniu będzie w dalszym ciągu Naliczanie wdrożenia nawet wtedy, gdy jest zatrzymana).

## <a name="see-also"></a>Zobacz też ##

[Azure zestaw narzędzi dla programu Eclipse][]

[Instalowanie narzędzi Azure dla programu Eclipse][] 

[Co nowego w Azure zestaw narzędzi dla programu Eclipse][]

Aby uzyskać więcej informacji na temat Azure za pomocą języka Java zobacz [Centrum deweloperów języka Java Azure][].

<!-- URL List -->

[Centrum deweloperów języka Java Azure]: http://go.microsoft.com/fwlink/?LinkID=699547
[Portal Azure zarządzania]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Role Properties]: http://go.microsoft.com/fwlink/?LinkID=699525
[Azure zestaw narzędzi dla programu Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Włączanie dostępu zdalnego dla Azure wdrażania Zaćmienie]: http://go.microsoft.com/fwlink/?LinkID=699538
[Instalowanie narzędzi Azure dla programu Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Właściwości konfiguracji serwera]: http://go.microsoft.com/fwlink/?LinkID=699525#server_configuration_properties
[Co nowego w Azure zestaw narzędzi dla programu Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699552

<!-- IMG List -->

[ic589576]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic589576.png
[ic589579]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic589579.png
[ic600360]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic600360.png
[ic644267]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic644267.png
[ic659262]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic659262.png
[ic710876]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710876.png
[ic710879]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710879.png
[ic710880]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710880.png
[ic710882]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710882.png
[ic710883]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710883.png
[ic719488]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719488.png
[ic719489]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719489.png
[ic719490]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719490.png
[ic719491]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719491.png
[ic789598]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic789598.png
[publishDropdownButton]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/publishDropdownButton.png
