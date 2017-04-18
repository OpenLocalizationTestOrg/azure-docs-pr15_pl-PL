<properties
    pageTitle="Włączanie koligacji sesji za pomocą narzędzi Azure dla programu Eclipse"
    description="Dowiedz się, jak włączyć przy użyciu narzędzi Azure dla Zaćmienie koligacji sesji."
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

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690950.aspx -->

# <a name="enable-session-affinity"></a>Włączanie koligacji sesji #

W zestawie narzędzi Azure Zaćmienie możesz włączyć koligacji sesji protokołu HTTP lub "lepkich sesji", dla poszczególnych ról. Poniższa ilustracja przedstawia okno dialogowe używane do włączania funkcji koligacji sesji właściwości **Równoważenia obciążenia** :

![][ic719492]

## <a name="to-enable-session-affinity-for-your-role"></a>Aby włączyć koligacji sesji dla roli użytkownika ##

1. Kliknij prawym przyciskiem myszy roli w jego Zaćmienie Eksplorator projektu, kliknij pozycję **Azure**, a następnie kliknij **Równoważenia obciążenia**.
1. W oknie dialogowym **właściwości do równoważenia obciążenia WorkerRole1** :
    1. Sprawdzanie **koligacji sesji Włączanie HTTP (lepkich sesje) dla tej roli.**
    1. **Punkt końcowy wprowadzania umożliwia**wybierz punkt końcowy wprowadzania danych, aby użyć, na przykład **http (publicznej: 80, prywatne: 8080)**. Aplikacja należy używać tego punktu końcowego jako punktu końcowego HTTP. Możesz włączyć wiele punktów końcowych dla danej roli, ale możesz wybrać tylko jeden z nich do obsługi lepkich sesji.
    1. Odbudowywanie aplikacji.

Po włączeniu, jeśli masz więcej niż jedno wystąpienie roli, żądania HTTP pochodzące z określonego klienta będzie nadal są obsługiwane przez tego samego wystąpienia roli.

Zestaw narzędzi Zaćmienie umożliwia to instalując specjalne moduł usług IIS o nazwie aplikacji żądanie routingu (ARR) do każdego wystąpienia do roli. ARR zmienia trasę żądania HTTP wystąpieniu odpowiednią rolę. Zestaw narzędzi automatycznie skonfiguruje wybranego punktu końcowego, tak aby ruch przychodzący HTTP najpierw jest kierowane do oprogramowania ARR. Zestaw narzędzi również tworzy nowy punkt końcowy wewnętrznego który Java serwer jest skonfigurowany do słuchania. To jest punkt końcowy używane przez ARR, aby przekierowywać ruch HTTP wystąpieniu odpowiednią rolę. Dzięki temu każde wystąpienie roli z nazwami wielu wystąpień służy jako odwrotnej serwera proxy dla wszystkich innych przypadkach, włączenie lepkich sesji.

## <a name="notes-about-session-affinity"></a>Uwagi dotyczące koligacji sesji ##

* Koligacja sesji nie działa w emulatorze obliczeń. Ustawienia mogą być stosowane w emulatorze obliczeń bez zaburzania procesu kompilacji lub obliczyć wykonanie emulatora, ale sama ta funkcja nie działa w emulatorze obliczeń.
* Włączanie koligacji sesji spowoduje wzrost ilości miejsca zajmowanego przez wdrożenia w Azure, dodatkowego oprogramowania będą pobierane i instalowane w swojej roli wystąpienia podczas uruchamiania usługi w chmurze Azure.
* Podczas inicjowania każdej roli będzie trwać dłużej.
* Punkt końcowy wewnętrznych, funkcji rerouter ruch, wymienione powyżej, będzie added.ss

Na przykład sposobem utrzymywania dane sesji po włączeniu koligacji sesji zobacz [jak obsługa sesji danych za pomocą koligacji sesji][].

## <a name="see-also"></a>Zobacz też ##

[Azure zestaw narzędzi dla programu Eclipse][]

[Tworzenie aplikacji Witaj świecie dla Azure w Zaćmienie][]

[Instalowanie narzędzi Azure dla programu Eclipse][] 

[Jak korzystać z danych sesji z koligacją sesji][]

Aby uzyskać więcej informacji na temat Azure za pomocą języka Java zobacz [Centrum deweloperów języka Java Azure][].

<!-- URL List -->

[Centrum deweloperów języka Java Azure]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure zestaw narzędzi dla programu Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Tworzenie aplikacji Witaj świecie dla Azure w Zaćmienie]: http://go.microsoft.com/fwlink/?LinkID=699533
[Jak korzystać z danych sesji z koligacją sesji]: http://go.microsoft.com/fwlink/?LinkID=699539
[Instalowanie narzędzi Azure dla programu Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic719492]: ./media/azure-toolkit-for-eclipse-enable-session-affinity/ic719492.png
