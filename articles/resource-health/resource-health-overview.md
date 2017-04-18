<properties
   pageTitle="Przegląd kondycji usługi Azure zasobów | Microsoft Azure"
   description="Omówienie zasobów Azure kondycji"
   services="Resource health"
   documentationCenter="dev-center-name"
   authors="BernardoAMunoz"
   manager=""
   editor=""/>

<tags
   ms.service="resource-health"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="Supportability"
   ms.date="06/01/2016"
   ms.author="BernardoAMunoz"/>

# <a name="azure-resource-health-overview"></a>Przegląd kondycji usługi Azure zasobów

Azure kondycji zasobu to usługa, która udostępnia kondycji poszczególnych zasobów Azure, a także sankcji wskazówki dotyczące rozwiązywania problemów. W środowisku chmury miejsce, w którym nie jest możliwe w celu bezpośredniego dostępu serwerów lub elementów infrastruktury celem kondycji zasobów jest skrócenie czasu, w których klienci wydatki na rozwiązywania problemów, w szczególności zmniejszenia czasu poświęconego Określanie, czy przyczyny problemu określa wewnątrz aplikacji, czy jest powodowany przez zdarzenie wewnątrz platformy Azure.

## <a name="what-is-considered-a-resource-and-how-does-resource-health-decides-if-the-resource-is-healthy-or-not"></a>Co to jest traktowany jako zasobu i skąd kondycji zasobów zdecyduje, czy zasób jest prawidłowy, czy nie? 
Zasób jest wystąpieniem utworzonych przez użytkownika typ zasobu dostarczony przez usługi, na przykład: maszyny wirtualnej, aplikacji sieci Web lub bazy danych SQL. 

Kondycja zasobów zależy od sygnały dostarczanych przez zasób i/lub usługę, aby określić, czy zasób jest prawidłowy. Należy zauważyć, że obecnie zasobów kondycji tylko konta kondycji jeden zasób określonych wpisz a nie należy rozważyć, czy inne elementy, które może przyczynić się do ogólnej kondycji. Na przykład podczas raportowania stanu maszyny wirtualnej, tylko część obliczeń infrastruktury jest traktowany jako, to znaczy problemy w sieci nie jest wyświetlana w stanie zasobów, chyba że istnieje awarię zadeklarowanych usług w takim przypadku go będzie można udostępniać za pośrednictwem transparent u góry karta. Więcej informacji na temat awarii usługi jest oferowane w dalszej części tego artykułu. 

## <a name="how-is-resource-health-different-from-service-health-dashboard"></a>Czym różni się kondycji zasobów z pulpit nawigacyjny kondycji usługi?

Informacji podanych przez kondycji zasobu jest bardziej szczegółowy niż dostarczanych przez pulpit nawigacyjny kondycji usługi. Gdy SHD komunikuje się zdarzenia, które wpływają na dostępność usługi w regionie, kondycji zasobów udostępnia informacje dotyczące określonego zasobu, np. uwidaczniane zdarzenia, które wpływają na dostępność maszyny wirtualnej, aplikacji sieci web lub bazy danych SQL. Na przykład jeśli nieoczekiwanie ponownego uruchomienia węzeł klientów, których maszyn wirtualnych działały w tym węźle będą mogli uzyskać powód, dlaczego ich maszyn wirtualnych jest niedostępny w okresie czasu.   

## <a name="how-to-access-resource-health"></a>Jak uzyskać dostęp do zasobu kondycji
Dostępne za pośrednictwem zasobów kondycja usług istnieje 2 sposoby uzyskiwania dostępu kondycji zasobu.

### <a name="azure-portal"></a>Azure Portal
Karta Kondycja zasobu w Azure Portal szczegółowe informacje dotyczące kondycji zasobu, a także zalecane akcje, które różnią się w zależności od bieżącego kondycję zasobu. Ta karta zapewnia najlepsze wyniki podczas badania kondycji zasobów ułatwia ona dostęp do innych zasobów w portalu. Wymienione przed zestaw zalecane działania w karta Kondycja zasobu zależy bieżąca kondycja:

* Zasoby prawidłowy: ponieważ wykryto nie problemu, który może mieć wpływ na kondycję zasobu, akcje dotyczyła ułatwianie proces rozwiązywania problemów. Na przykład zawiera bezpośredni dostęp do karta Rozwiązywanie problemów, które zawiera wskazówki dotyczące rozwiązania typowych nominalnej klientów problemów.
* Nieprawidłowe zasobu: problemów powodowanych przez Azure, karta są wyświetlane akcje Microsoft trwa (lub miało) aby odzyskać zasobu. W przypadku problemów powodowanych przez użytkownika inicjowane akcje, będzie karta, które można wykonać na liście klientów akcje tak rozwiązania problemu i odzyskiwanie zasobu.  

Gdy logujesz się do portalu Azure, istnieją dwa sposoby uzyskiwania dostępu karta Kondycja zasobu: 

###<a name="open-the-resource-blade"></a>Otwórz karta zasobu
Otwórz karta zasobów dla danego zasobu. Na kartę Ustawienia, która zostanie otwarta obok karta zasobu kliknij kondycja zasobów, aby otworzyć karta Kondycja zasobów. 

![Karta Kondycja zasobów](./media/resource-health-overview/resourceBladeAndResourceHealth.png)

### <a name="help-and-support-blade"></a>Pomoc i karta pomocy technicznej
Otwórz Karta Pomoc i obsługa techniczna, klikając na znak zapytania w prawym górnym rogu, a następnie wybierając pomocy + pomocy technicznej. 

**Na górnym pasku nawigacyjnym**

![Pomoc + pomocy technicznej](./media/resource-health-overview/HelpAndSupport.png)

Klikając Kafelek zostanie wyświetlona karta subskrypcji kondycji zasobu, która zawiera listę wszystkich zasobów w ramach subskrypcji. Obok każdego zasobu jest ikona oznaczająca jego kondycji. Kliknięcie każdego zasobu spowoduje otwarcie karta Kondycja zasobów.

**Zasób kondycji kafelków**

![Zasób kondycji kafelków](./media/resource-health-overview/resourceHealthTile.png)

## <a name="what-does-my-resource-health-status-mean"></a>Co oznacza stan kondycji Moje zasobów
Istnieje 4 statusy różnych kondycji wyświetlane dla zasobu.

### <a name="available"></a>Dostępne
Usługa nie wykrył problemów w platformę, na której może mieć wpływ na dostępność zasobu. To jest wskazywana przez ikonę zielony znacznik wyboru. 

![Zasób jest dostępny](./media/resource-health-overview/Available.png)

### <a name="unavailable"></a>Niedostępne

W tym przypadku usługa wykrył bieżąco problem w platformę, na której jest wpływające na ochronę dostępności tego zasobu, na przykład węzeł miejsce, w którym został uruchomiony maszyn wirtualnych nieoczekiwanie ponownie uruchomić. To jest oznaczany czerwoną ikonę ostrzeżenie. Dodatkowe informacje o tym problemie znajduje się w środkowej części karta, w tym: 

1.  Akcje Microsoft trwa odzyskać zasobu 
2.  Szczegółowe osi czasu problemu, włącznie z czasem oczekiwanych rozdzielczości
3.  Lista zalecane akcje dla użytkowników 

![Zasób jest niedostępny](./media/resource-health-overview/Unavailable.png)

### <a name="unavailable--customer-initiated"></a>Niedostępny — inicjowanych przez klienta
Zasób jest niedostępne z powodu żądanie klienta, takich jak zatrzymanie zasób lub żąda ponowne uruchomienie komputera. To jest oznaczany niebieska ikona informacyjna. 

![Zasób jest niedostępny z powodu użytkownika inicjowane akcji](./media/resource-health-overview/userInitiated.png)

### <a name="unknown"></a>Nieznany
Usługa nie otrzymała informacji na temat tego zasobu przez ponad 5 minut. To jest wskazywana przez ikonę szarym znaku zapytania. 

Należy zauważyć, że nie jest ostateczne wskazania, że występuje problem z zasobem, więc klientów należy tych zaleceń:

* Jeśli jest uruchomiony zasób, zgodnie z oczekiwaniami, ale jego kondycji jest ustawiona na nieznany w stanie zasobów, występują problemy i można się spodziewać stan zasobu, aby zaktualizować prawidłowy po upływie kilku minut.
* W przypadku problemów z dostępem do zasobu i jego kondycji jest ustawiona na nieznany w stanie zasobów, może to wskazywać najwcześniejsze może istnieć problem i dodatkowych badań należy przeprowadzić aż kondycji jest aktualizowana prawidłowy lub nieprawidłowe

![Kondycja zasobów jest nieznany](./media/resource-health-overview/unknown.png)

## <a name="service-impacting-events"></a>Zdarzenia wpływające na ochronę usługi
Jeśli zasób może mieć negatywny wpływ na przez trwających wydarzenia wpływające na ochronę usługi, transparent będzie wyświetlany w górnej części karta Kondycja zasobów. Kliknięcie na banerze spowoduje otwarcie karta zdarzeń inspekcji, co powoduje wyświetlenie dodatkowych informacji na temat awarii.

![SIE może mieć negatywny wpływ kondycji zasobu](./media/resource-health-overview/serviceImpactingEvent.png)

## <a name="what-else-do-i-need-to-know-about-resource-health"></a>Co jeszcze należy wiedzieć o kondycji zasobu?

### <a name="signal-latency"></a>Opóźnienia sygnału
Sygnału kanału informacyjnego kondycji zasobów, może być maksymalnie 15 min opóźnione, które mogą powodować rozbieżności między bieżący stan kondycji zasobu i jego rzeczywistej dostępności. Należy pamiętać tak jak pomoże wyeliminować niepotrzebne czas prowadzące badanie możliwych problemów. 

### <a name="special-case-for-sql"></a>Specjalne litery SQL 
Kondycja zasobów raportów stanu bazy danych SQL nie programu SQL server. Podczas przejścia tą drogą udostępnia bliższy obrazu kondycji, wymaga to, że wielu składników i usług należy uwzględnić ustalenie kondycji bazy danych. Bieżący sygnału zależy od logowania do bazy danych, co oznacza, że dla baz danych, które odbieranie regularnych logowania, (obejmujące między innymi otrzymaniu żądania wykonania kwerendy) kondycji stan regularnie pojawi się. Jeśli baza danych nie była używana w okresie 10 minut lub więcej, zostanie przeniesiony do nieznany stan. Oznacza to, że baza danych jest niedostępne, wystarczy, że nie ma sygnału zostały emisji, ponieważ zostały wykonane nie logowania. Nawiązywanie połączenia z bazą danych i uruchamianie kwerendy będzie wysyłać sygnały, aby określić i aktualizowanie stanu kondycji bazy danych.

## <a name="feedback"></a>Opinie
Zawsze jesteśmy otwarte opinii i sugestii! Prześlij nam [Sugestie](https://feedback.azure.com/forums/266794-support-feedback). Możesz także współpracować z nami za pośrednictwem [serwisu Twitter](https://twitter.com/azuresupport) lub na [forum w witrynie MSDN](https://social.msdn.microsoft.com/Forums/azure).
