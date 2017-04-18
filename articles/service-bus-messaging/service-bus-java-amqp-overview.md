<properties 
    pageTitle="Omówienie usługi Bus AMQP przy użyciu języka Java | Microsoft Azure" 
    description="Informacje o korzystaniu z zaawansowane wiadomości Kolejkowanie Protocol (AMQP) 1.0 platformy Azure Java." 
    services="service-bus" 
    documentationCenter="java" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="sethm"/>


# <a name="amqp-10-support-in-service-bus"></a>Obsługa AMQP 1.0 w Bus usługi

Usługa w chmurze Bus usługi Azure i lokalna [Bus usługi dla systemu Windows Server (usługa Bus 1.1)](https://msdn.microsoft.com/library/dn282144.aspx) obsługuje zaawansowane wiadomości Kolejkowanie Protocol (AMQP) 1.0. AMQP umożliwia tworzenie i platform, hybrydowych aplikacji przy użyciu protokołu open standardowy. Można utworzyć aplikacji przy użyciu składników wbudowanych przy użyciu różnych języków i struktury, a które działają w różnych systemach operacyjnych. Wszystkie te elementy można połączyć Bus usługi i bezproblemowo wymiany wiadomości biznesowych strukturalnych wydajność i Pełna wierności.

## <a name="introduction-what-is-amqp-10-and-why-is-it-important"></a>Wprowadzenie: Co to jest AMQP 1.0 i dlaczego są istotne?

Zazwyczaj pośredniczącego przesyłania wiadomości korzystają z własnych protokoły dla komunikacji między aplikacjami klienta i brokerów. Oznacza to, że po wybraniu określonego dostawcy brokera wiadomości należy użyć tego dostawcy bibliotek nawiązać tego brokera aplikacjom klienckim. Powoduje stopień zależność od tego dostawcy, ponieważ przenoszenie aplikacji do innego produktu wymaga zmiany kodu we wszystkich aplikacjach połączonego. 

Ponadto łączenie brokerów wiadomości z różnych dostawców jest kłopotliwe. Wymaga to zazwyczaj poziomie aplikacji łączące przenieść wiadomości z jednego systemu i tłumaczenie między formatami ich własnych wiadomości. Jest to wymagane typowych; na przykład po możesz należy podać nowy interfejs ujednolicony do starszych odmienne systemy lub Integracja systemów informatycznych po połączeniu.

Branżowe oprogramowania jest szybko firm; nowe języki programowania i środowisk aplikacji są wprowadzane w czasami bewildering tempie. Podobnie do wymagań systemów informatycznych są obsługiwane w czasie i deweloperów chcesz korzystać z najnowszych funkcji platformy. Jednak czasami wybranego dostawcy wiadomości nie obsługuje tych platform. Ponieważ protokoły są unikatowe, nie jest możliwe przez inne osoby o podanie bibliotek dla tych nowych platform. W związku z tym należy użyć metod, takich jak tworzenie bram lub mosty, które pozwalają na kontynuowanie pracy wiadomości.

Rozwoju z zaawansowane wiadomości Kolejkowanie Protocol (AMQP) 1.0 został decydują tych problemów. Rozpoczęte na Morgan Chase JP podlegającym, takich jak najbardziej finansowe usługowe, użytkownicy programu oprogramowanie pośrednie do przesyłania wiadomości. Celem było proste: tworzenie protokół wiadomości otwartym standardem możliwe tworzenie aplikacji wiadomości przy użyciu składniki utworzone przy użyciu różnych języków, struktury i systemy operacyjne wszystkich korzystanie ze składników najważniejsze obsługi z zakresu dostawców.

## <a name="amqp-10-technical-features"></a>Funkcje techniczne AMQP 1.0

AMQP 1.0 jest wydajność, niezawodne i poziomu przewodowy wiadomości protokół, który można tworzyć niezawodne, między platformami aplikacji do obsługi wiadomości. Protokół ma prosty cel: Aby zdefiniować weryfikowaniu bezpiecznego, niezawodnego i wydajnego przesyłaniem wiadomości między dwiema stronami. Wiadomości, same są kodowane przy użyciu reprezentacją danych, który umożliwia niejednorodnymi nadawców i adresatów, do wymiany wiadomości biznesowych strukturalnych w pełny wierności. Poniżej przedstawiono podsumowanie najważniejszych elementów:

*    **Skuteczność**: AMQP 1.0 jest nawiązaniem połączenia protokołu tego zastosowania kodowanie binarne instrukcje Protocol (protokół) i wiadomości biznesowych przeniesione nad nim. Zawiera schematy zaawansowanych sterowanie przepływem maksymalizować wykorzystanie sieci i połączenia składników. Który wspomniano, protokół ma zrozumiałe wydajność, elastyczność i współdziałania.
*    **Niezawodny**: protokół 1.0 AMQP umożliwia wymianę z zakresu gwarancji niezawodności z fire i zapomnij niezawodne, dokładnie wiadomości — raz potwierdzenie dostarczenia.
*    **Elastyczne**: AMQP 1.0 jest protokołem elastyczne, który może być używany do obsługi różnych topologii. Tego samego protokołu może służyć komunikacji klienta do klienta, klienta do brokera i brokera do brokera.
*    **Model brokera niezależnej**: specyfikacji 1.0 AMQP nie powoduje wymagań na modelu wiadomości, używany przez pośrednika. Oznacza to, że jest możliwe łatwe dodawanie AMQP 1.0 pomocy technicznej do istniejącej wiadomości brokerów.

## <a name="amqp-10-is-a-standard-with-a-capital-s"></a>AMQP 1.0 jest to standardowy (z kapitału ")

AMQP 1.0 znajdował się w rozwoju od 2008 grupy core więcej niż 20 firmy, zarówno technologii dostawców, jak i firm użytkowników końcowych. W tym czasie przedsiębiorstwa użytkownika przyczynić się wymagań ich rzeczywistych i dostawców technologii usprawnionych protokołu w celu spełnienia tych wymagań. W trakcie procesu dostawców brali udział w warsztatach, w których są współpracowały w celu weryfikacji współdziałanie ich implementacji.

W październik 2011 projektach są przenoszone do Komitet techniczny w firmie dla przejścia of strukturalnych informacji standardy (OASIS) oraz standardowe OASIS AMQP 1.0 została wydana w październik 2012. Następujące przedsiębiorstwa udział w Komitet Techniczny podczas opracowywania standard:

*    **Technologia dostawców**: oprogramowanie Axway, Huawei technologii, IIT oprogramowania, INETCO systemów, Kaazing, Microsoft, Mitre Corporation, Primeton technologii, informacje o postępie oprogramowania, czerwony funkcję, SITA, oprogramowanie AG, systemów Solace, VMware, WSO2, Zenika.
*    **Przedsiębiorstwa użytkownika**: banku Ameryki Suisse kredytowej, Boerse niemieckich, Goldman Sachs, JPMorgan Chase.

Oto niektóre z często cytowane zalety otwartych standardów:

*    Mniejsze prawdopodobieństwo blokady w dostawcy
*    Współdziałanie
*    Dostępność szeroki bibliotek i narzędzia
*    Ochrona przed utraty przydatności
*    Dostępność personelu wiedzę
*    Dolny i łatwe w zarządzaniu ryzyka

## <a name="amqp-10-and-service-bus"></a>AMQP Bus 1.0 i usług

Obsługa AMQP 1.0 w Bus usługi Azure oznacza, że można teraz korzystać z usług Bus kolejek i publikowania/subskrypcji brokered funkcje wiadomości z zakresu platform za pomocą protokołu binarnego wydajność. Ponadto można tworzyć aplikacje obejmuje składniki utworzone przy użyciu różnych języków, struktury i systemy operacyjne.

Na poniższym rysunku pokazano przykładowe wdrożenie, w którym Java uruchomionym w systemie Linux, napisane przy użyciu protokół standardowy Java wiadomości usługi (JMS) w interfejsu API i .NET uruchomionym w systemie Windows, wymiany wiadomości za pośrednictwem usługi Bus przy użyciu AMQP 1.0.

![][0]

**Rysunek 1: Przykładu wdrożenia przedstawiający między platformami wiadomości przy użyciu usługi Bus i AMQP 1.0**

W tej chwili następujących bibliotek klienta są znane do pracy z Bus usługi:

| Język | Biblioteka                                                                       |
|----------|-------------------------------------------------------------------------------|
| Java     | Klient Apache Qpid Java wiadomości usługi (JMS)<br/>Klient IIT oprogramowania SwiftMQ Java |
| C        | Qpid Apache protonów C                                                          |
| PHP      | Qpid Apache protonów PHP                                                        |
| Python   | Qpid Apache protonów Python                                                     |


**Rysunek 2: Spis bibliotek klienta AMQP 1.0**

Aby uzyskać więcej informacji na temat do biblioteki za pomocą usługi Bus zobacz [przewodnik dla deweloperów usługi Bus AMQP][]. W sekcji [Następne kroki](service-bus-java-amqp-overview.md#next-steps) łącza do dalszych informacji.

## <a name="summary"></a>Podsumowanie

*    AMQP 1.0 jest otwarte, wiarygodnych wiadomości protokół, który służy do tworzenia i platform, hybrydowych aplikacji. AMQP 1.0 to OASIS standard.
*    Obsługa AMQP 1.0 jest teraz dostępny w Bus usługi Azure, a także Bus usługi dla systemu Windows Server (usługa Bus 1.1). Ceny jest taka sama, jak w przypadku istniejących protokołów.

## <a name="next-steps"></a>Następne kroki

Skorzystaj z poniższych łączy, aby dowiedzieć się więcej o obsłudze AMQP w Bus usług.

*    [Jak używać AMQP 1.0 przy użyciu interfejsu API usługi Bus .NET](service-bus-dotnet-advanced-message-queuing.md)
*    [Jak używać języka Java wiadomości usługi (JMS) interfejsu API z Bus usługi i AMQP 1.0](service-bus-java-how-to-use-jms-api-amqp.md)
*    [Usługi Bus AMQP Przewodnik programisty][]
*    [Specyfikacja interfejsu Advanced wiadomości Kolejkowanie Protocol (AMQP) w wersji 1.0 OASIS](http://docs.oasis-open.org/amqp/core/v1.0/os/amqp-core-complete-v1.0-os.pdf)

[0]: ./media/service-bus-java-amqp-overview/Example1.png
[Usługi Bus AMQP Przewodnik programisty]: service-bus-amqp-dotnet.md

 
