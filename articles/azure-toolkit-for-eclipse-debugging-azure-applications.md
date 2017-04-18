<properties
    pageTitle="Debugowanie aplikacji Azure w Zaćmienie"
    description="Informacje na temat aplikacji Azure debugowanie przy użyciu narzędzi Azure dla programu Eclipse."
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

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690949.aspx -->

# <a name="debugging-azure-applications-in-eclipse"></a>Debugowanie aplikacji Azure w Zaćmienie #

Korzystanie z zestawu narzędzi Azure dla programu Eclipse, można debugowania aplikacji czy działają w Azure lub emulatora obliczeń Jeśli korzystasz z systemu Windows w systemie operacyjnym. Poniższa ilustracja przedstawia okno dialogowe umożliwia zdalne debugowanie właściwości **Debugowanie** :

![][ic719504]

Tego samouczka przyjęto założenie, że masz już aplikacja, z której został utworzony pomyślnie, a są znanych emulatorze obliczeń i wdrażanie Azure.

Użyjemy aplikacji z [używania biblioteki środowisko uruchomieniowe usługi Azure w JSP][] samouczek jako punkt początkowy dla tego tematu. Przed kontynuowaniem utwórz tej aplikacji, jeśli nie masz już gotowe.

## <a name="to-debug-your-application-while-running-in-azure"></a>Aby debugowanie aplikacji podczas korzystania z platformy Azure ##

>[AZURE.WARNING] Zestaw narzędzi bieżącej obsługę języka Java zdalne debugowanie jest przeznaczona przede wszystkim w przypadku wdrożeń uruchomione w emulatorze Azure obliczeń. Ponieważ debugowania połączenia nie jest bezpieczny, nie należy włączać zdalne debugowanie we wdrożeniach produkcji. Jeśli potrzebujesz debugowania aplikacji działa w chmurze Azure, tymczasowy wdrażania za pomocą, ale okazuje możliwe, gdy ktoś zna adresu IP rozmieszczenia chmury nieautoryzowanego dostępu do sesji programu debugowania nawet w przypadku tymczasowej wdrożenia.

1. Tworzenie projektu badań w emulatorze: Eksplorator projektu w Zaćmienie osoby, kliknij prawym przyciskiem myszy **MyAzureProject**, kliknij pozycję **Właściwości**, kliknij pozycję **Azure**i ustaw **Tworzenie dla** **wdrożenia do chmury**.
1. Odbudowywanie projektu: Z menu Zaćmienie kliknij pozycję **Projekt**, a następnie kliknij przycisk **Konstruuj wszystko**.
1. Wdrażanie aplikacji z *tymczasowej* platformy Azure.
    >[AZURE.IMPORTANT] Jak wcześniej wspomniano, zdecydowanie zaleca się debugowania w emulatorze obliczeń w większości przypadków następnie debugowania w środowisku tymczasowy tylko wtedy, gdy dodatkowe debugowania jest potrzebna. Zalecane jest nie debugowania w środowisku produkcyjnym.
1. Gdy wdrożenia będzie gotowa platformy Azure, uzyskanie nazw DNS do wdrożenia w [Portalu zarządzania Azure][]. Tymczasowy wdrożenia ma nazwę DNS w formie http://*&lt;guid&gt;*. cloudapp.net, gdzie * &lt;guid&gt; * jest wartością GUID przypisane przez Azure.
1. W oknie Project Explorer Zaćmienie osoby kliknij prawym przyciskiem myszy **WorkerRole1**, kliknij pozycję **Azure**, a następnie kliknij **Debugowanie**.
1. W oknie dialogowym **Właściwości dla debugowania WorkerRole1** :
    1. Sprawdzanie **Włączanie debugowania zdalnego dla tej roli.**
    1. Dla **punktu końcowego wprowadzania używać**za pomocą **Debugowanie (publicznego: 8090, prywatne: 8090)**.
    1. Upewnij się, że **Rozpoczęcie maszyny wirtualnej Java w trybie wstrzymania, trwa oczekiwanie na połączenie debugowania** jest zaznaczona.
        >[AZURE.IMPORTANT] Opcja **Rozpocznij maszyny wirtualnej Java w trybie wstrzymania, trwa oczekiwanie na połączenie debugowania** jest przeznaczona zaawansowanych debugowania scenariuszy w emulatorze obliczeń tylko (nie w przypadku wdrożeń chmury). Jeśli opcja **Uruchom maszyny wirtualnej Java w trybie wstrzymania, trwa oczekiwanie na połączenie debugowania** jest używana, będzie zawiesić proces uruchamiania serwera, aż debugowania Zaćmienie jest podłączone do jego maszyny wirtualnej Java. Podczas sesji debugowania za pomocą emulatora obliczeń można użyć tej opcji, nie jest używana do sesji debugowania we wdrożeniu chmury. Inicjowanie serwera odbywa się w zadaniu Azure uruchamiania i chmura Azure nie udostępnia publicznych punkty końcowe dopóki nie zakończy się zadanie uruchomienia. W związku z tym proces uruchamiania nie zostanie ukończona pomyślnie Jeśli ta opcja jest włączona w chmurze wdrożenia, ponieważ nie będzie mógł nawiązuje połączenie z zewnętrznym klienta Zaćmienie.
1. Kliknij przycisk **Utwórz debugowania konfiguracji**.
1. W oknie dialogowym **Konfiguracja debugowanie Azure** :
    1. Dla **projektu Java debugowania**wybierz projekt, **MyHelloWorld** .
    1. **Konfigurowanie debugowania**Sprawdź **chmura Azure (przemieszczenia)**.
    1. Upewnij się, że **Azure obliczyć emulatora** jest zaznaczona.
    1. Dla **hosta**wpisz nazwę serwera DNS etapowej wdrożenia, ale bez poprzedniego **http://**. Na przykład (Użyj swojego GUID zamiast GUID pokazano poniżej): **4e616d65-6f6e-6 d 65-6973-526f62657274.cloudapp.net**
1. Kliknij **przycisk OK** , aby zamknąć okno dialogowe **Konfiguracja debugowanie Azure** .
1. Kliknij **przycisk OK** , aby zamknąć okno dialogowe **Właściwości dla debugowania WorkerRole1** .
1. Jeśli nie masz już ustawiony w index.jsp punkt przerwania, należy ją skonfigurować:
    1. W jego Zaćmienie Eksplorator projektu rozwiń pozycję **MyHelloWorld**, rozwiń pozycję **sieć Web, zawartość**, a następnie kliknij dwukrotnie **index.jsp**.
    1. W index.jsp kliknij prawym przyciskiem myszy na niebieskim pasku z lewej strony kodu języka Java i kliknij **Przełącznik punktów kontrolnych**, jak pokazano poniżej: ![][ic551537]
1. W menu Zaćmienie osoby kliknij polecenie **Uruchom** , a następnie kliknij pozycję **Debugowanie konfiguracji**.
1. W oknie dialogowym **Debugowanie konfiguracji** rozwiń **Zdalnego aplikacji Java** w okienku po lewej stronie, wybierz **Chmura Azure (WorkerRole1)**, a kliknij **Debugowanie**.
1. W przeglądarce, uruchamianie aplikacji etapowej **http://***&lt;guid&gt;***.cloudapp.net/MyHelloWorld**, zastępując GUID z Twojej nazwy DNS dla * &lt;guid&gt;*. Jeśli zostanie wyświetlony monit, okno dialogowe **Potwierdzenia Przełącz perspektywy** , kliknij przycisk **Tak**. Sesję debugowania powinno być teraz wykonane do wiersza kodu, w którym została ustawiona przerwania.

>[AZURE.NOTE] Jeśli próbujesz uruchomić zdalnego debugowania połączenia w celu wdrożenia, zawierającą wielu wystąpień roli działa, nie można obecnie kontrolować którym debugowania będzie początkowo połączony z, jak równoważenia obciążenia Azure będą losowo wybierz wystąpienie. Po nawiązaniu połączenia z to wystąpienie, jednak będzie debugowania tego samego wystąpienia. Uwaga również po pewnym okresie nieaktywności więcej niż 4 minuty (na przykład, gdy jest zatrzymane w punkcie przerwania przez dłuższy czas) Azure może zamknąć połączenie.

## <a name="debugging-a-specific-role-instance-in-a-multi-instance-deployment"></a>Debugowanie w przypadku wystąpienia konkretnej roli we wdrożeniu wielu wystąpień ##

Jeśli wdrożenia działa w chmurze, będzie najprawdopodobniej uruchamiana go w wielu obliczeń, lub roli wystąpienia. Dzięki temu będzie można wykorzystać gwarancji 99,95 Azure % dostępności, a także możliwość skalowania aplikacji.

W takich przypadkach może być konieczne zdalne debugowanie aplikacji Java w przypadku konkretnej roli. Jednak jeśli włączysz tylko zwykły końcowy wprowadzania danych do debugowania, równoważenia obciążenia Azure spowoduje to praktycznie niemożliwe nawiązania debugowania wystąpieniu konkretnej roli. Zamiast tego go połączysz wystąpieniu roli, który go odbiera losowo.

Jest to typ scenariusz miejsce, w którym skorzystać wprowadzania punkty końcowe wystąpienie ułatwi umożliwiają debugowanie w przypadku wystąpienia konkretnej roli.

Załóżmy, że planujesz przeprowadzenie maksymalnie 5 wystąpienia roli wdrożenia. Na stronie właściwości **punkty końcowe** w oknie dialogowym Właściwości roli, tworzenie punktu końcowego wprowadzania wystąpienia i przypisać go w zakresie portów publiczne, a nie numer portu pojedynczy. Na przykład w polu wprowadzania **publicznej port** określ **81 85**.

Po wdrożeniu aplikacji przy użyciu tego punktu końcowego wystąpienie Azure przypisać unikatowy numer portu z tego zakresu do wszystkich wystąpień roli. Następnie, aby dowiedzieć się, którym masz przypisany numer portu, którego, można użyć zmiennej środowiska**_PUBLICPORT** *InstanceEndpointName*(w przypadku *InstanceEndpointName* nazwę przypisaną po utworzeniu punkt końcowy wystąpienie) automatycznie skonfigurowany przez zestaw narzędzi we wdrożeniu programu (na przykład, przywracając jej wartość w stopce strony sieci Web, więc można ją przeczytać, po przejściu do niego).

Gdy wiesz, jakiego numeru portu publicznej przypisano to wystąpienie, odwołanie w konfiguracji debugowania w Zaćmienie, przez umieszczenie go do nazwy hosta tej usługi. Spowoduje to włączenie debugowania Zaćmienie nawiązywania połączenia z tego konkretnego wystąpienia, a nie jakiekolwiek innych wystąpień.

## <a name="windows-only-to-debug-your-application-while-running-in-the-compute-emulator"></a>Tylko w systemie Windows: jest debugowanie aplikacji podczas pracy w emulatorze obliczeń ##

>[AZURE.NOTE] Azure emulatora jest dostępna tylko w systemie Windows. Jeśli korzystasz z systemu operacyjnego innego niż Windows, należy pominąć tę sekcję. 

1. Tworzenie projektu badań w emulatorze: Eksplorator projektu w Zaćmienie osoby, kliknij prawym przyciskiem myszy **MyAzureProject**, kliknij polecenie **Właściwości**, kliknij **Azure**i ustaw **Tworzenie dla** **Testowanie w emulatora**.
1. Odbudowywanie projektu: Z menu Zaćmienie kliknij pozycję **Projekt**, a następnie kliknij przycisk **Konstruuj wszystko**.
1. W oknie Project Explorer Zaćmienie osoby kliknij prawym przyciskiem myszy **WorkerRole1**, kliknij pozycję **Azure**, a następnie kliknij **Debugowanie**.
1. W oknie dialogowym **Właściwości dla debugowania WorkerRole1** :
    1. Sprawdzanie **Włączanie debugowania zdalnego dla tej roli.**
    1. Dla **punktu końcowego wprowadzania używać**za pomocą domyślnego punktu końcowego generowane automatycznie przez zestaw narzędzi na liście **Debugowanie (publicznego: 8090, prywatne: 8090)**.
    1. Upewnij się, że jest zaznaczona opcja **Uruchom maszyny wirtualnej Java w trybie wstrzymania, trwa oczekiwanie na połączenie debugowania** .
        >[AZURE.IMPORTANT] Opcja **Rozpocznij maszyny wirtualnej Java w trybie wstrzymania, trwa oczekiwanie na połączenie debugowania** jest przeznaczona zaawansowanych debugowania scenariuszy w emulatorze obliczeń tylko (nie w przypadku wdrożeń chmury). Jeśli opcja **Uruchom maszyny wirtualnej Java w trybie wstrzymania, trwa oczekiwanie na połączenie debugowania** jest używana, będzie zawiesić proces uruchamiania serwera, aż debugowania Zaćmienie jest podłączone do jego maszyny wirtualnej Java. Podczas sesji debugowania za pomocą emulatora obliczeń można użyć tej opcji, nie jest używana do sesji debugowania we wdrożeniu chmury. Inicjowanie serwera odbywa się w zadaniu Azure uruchamiania i chmura Azure nie udostępnia publicznych punkty końcowe dopóki nie zakończy się zadanie uruchomienia. W związku z tym proces uruchamiania nie zostanie ukończona pomyślnie Jeśli ta opcja jest włączona w chmurze wdrożenia, ponieważ nie będzie mógł nawiązuje połączenie z zewnętrznym klienta Zaćmienie.
1. Kliknij przycisk **Utwórz debugowania konfiguracji**.
1. W oknie dialogowym **Konfiguracja debugowanie Azure** :
    1. Dla **projektu Java debugowania**wybierz projekt, **MyHelloWorld** .
    1. **Konfigurowanie debugowania**Sprawdź **Azure obliczyć emulatora**.
1. Kliknij **przycisk OK** , aby zamknąć okno dialogowe **Konfiguracja debugowanie Azure** .
1. Kliknij **przycisk OK** , aby zamknąć okno dialogowe **Właściwości dla debugowania WorkerRole1** .
1. Ustaw punkt przerwania w index.jsp:
    1. W jego Zaćmienie Eksplorator projektu rozwiń pozycję **MyHelloWorld**, rozwiń pozycję **sieć Web, zawartość**, a następnie kliknij dwukrotnie **index.jsp**.
    1. W index.jsp kliknij prawym przyciskiem myszy na niebieskim pasku po lewej stronie kodzie Java i kliknij **Przełącznik punktów kontrolnych**, jak pokazano poniżej: ![][ic551537]

       Punkt przerwania ustawiono, jeśli znajduje się ikona przerwania na niebieskim pasku na lewo od kodu języka Java.
1. Uruchom aplikację w emulatorze obliczeń przez kliknięcie przycisku **Uruchom w emulatora Azure** na pasku narzędzi Azure.
1. W menu Zaćmienie osoby kliknij polecenie **Uruchom** , a następnie kliknij pozycję **Debugowanie konfiguracji**.
1. W oknie dialogowym **Debugowanie konfiguracji** rozwiń **Zdalna aplikacja języka Java** w okienku po lewej stronie, wybierz pozycję **Azure emulatora (WorkerRole1)**, a kliknij **Debugowanie**.
1. Po emulatora obliczeń wskazuje, że aplikacja jest uruchomiona w przeglądarce, uruchom **8080/MyHelloWorld**.
    Jeśli zostanie wyświetlony monit, okno dialogowe **Potwierdzenia Przełącz perspektywy** , kliknij przycisk **Tak**.
    Sesję debugowania powinno być teraz wykonane do wiersza kodu, w którym została ustawiona przerwania.

Pokazano to sposób debugowania w emulatorze obliczeń; następnej sekcji pokazano, sposób debugowania aplikacji wdrożone Azure.

## <a name="debugging-notes"></a>Debugowanie notatek ##

* Po, możesz przełączać perspektywy z **Debugowanie** **języka Java** przez kliknięcie menu Zaćmienie firmy, klikając **okno** **Otwarty**i wybierając perspektywy, który ma być używany.
* Aby włączyć debugowanie zdalnego w GlassFish, nie należy używać funkcji konfiguracji debugowania zdalnego Azure zestawu narzędzi dla programu Eclipse. Zamiast ręcznie skonfigurować GlassFish. Ze względu na sposób GlassFish traktuje opcje języka Java wstępnie zdefiniowane zmienne środowiska funkcja konfiguracji debugowania zdalnego narzędzi nie działa poprawnie z GlassFish. Po włączeniu funkcji Konfiguracja debugowania zdalnego narzędzi go może uniemożliwić GlassFish uruchomienie.

## <a name="see-also"></a>Zobacz też ##

[Azure zestaw narzędzi dla programu Eclipse][]

[Tworzenie aplikacji Witaj świecie dla Azure w Zaćmienie][]

[Instalowanie narzędzi Azure dla programu Eclipse][] 

Aby uzyskać więcej informacji na temat Azure za pomocą języka Java zobacz [Centrum deweloperów języka Java Azure][].

<!-- URL List -->

[Centrum deweloperów języka Java Azure]: http://go.microsoft.com/fwlink/?LinkID=699547
[Portal Azure zarządzania]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure zestaw narzędzi dla programu Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Tworzenie aplikacji Witaj świecie dla Azure w Zaćmienie]: http://go.microsoft.com/fwlink/?LinkID=699533
[Instalowanie narzędzi Azure dla programu Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Korzystanie z biblioteki środowisko uruchomieniowe usługi Azure w JSP]: http://go.microsoft.com/fwlink/?LinkID=699551

<!-- IMG List -->

[ic719504]: ./media/azure-toolkit-for-eclipse-debugging-azure-applications/ic719504.png
[ic551537]: ./media/azure-toolkit-for-eclipse-debugging-azure-applications/ic551537.png
