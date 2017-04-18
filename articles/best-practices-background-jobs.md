<properties
   pageTitle="Wskazówki dotyczące zadania w tle | Microsoft Azure"
   description="Wytyczne dotyczące tła zadań tego uruchomieniami niezależne interfejsu użytkownika."
   services=""
   documentationCenter="na"
   authors="dragon119"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="best-practice"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/21/2016"
   ms.author="masashin"/>

# <a name="background-jobs-guidance"></a>Wskazówki dotyczące zadań tła

[AZURE.INCLUDE [pnp-header](../includes/guidance-pnp-header-include.md)]

## <a name="overview"></a>Omówienie

Wiele typów aplikacji wymagają zadania w tle, które są uruchamiane niezależnie od interfejsu użytkownika (UI). Jako przykład można wymienić zadań, intensywnie przetwarzanie zadań i długotrwałych procesów, takich jak przepływy pracy. Zadań wykonywanych w tle mogą być wykonywane bez konieczności interakcji użytkownika — aplikacji można uruchomić zadanie, a następnie kontynuować przetwarzania interakcyjnych żądania od użytkowników. Pomaga to zminimalizować obciążenie interfejsu użytkownika, którego można zwiększyć dostępność i zmniejszyć czas reakcji interakcyjnych aplikacji.

Na przykład jeśli aplikacja jest wymagane do generowania miniatur obrazów, które zostaną przekazane przez użytkowników, go w tym jako zadania w tle i zapisywanie miniaturę do magazynu po jego zakończeniu — bez użytkownika konieczności poczekaj, aż proces do wykonania. W taki sam sposób umieszczania rzędu użytkownika można inicjować przepływ pracy tła przetwarzającym kolejności, gdy interfejs użytkownika umożliwia użytkownikowi kontynuować przeglądanie aplikacji sieci web. Po ukończeniu zadania w tle ją zaktualizować dane przechowywane zamówienia i wysyłanie wiadomości e-mail do użytkownika, którego potwierdza kolejności.

Kiedy można rozważyć, czy do wykonania zadania jako zadania w tle, główne kryteria jest, czy zadanie może zostać uruchomiony bez interakcji użytkownika i bez interfejsu użytkownika wymagających czekać na zadanie do wykonania. Zadania, które wymagają użytkownika lub interfejs czekać, gdy są one wypełnione mogą nie być właściwe jako zadań wykonywanych w tle.

## <a name="types-of-background-jobs"></a>Typy zadań wykonywanych w tle

Zadań wykonywanych w tle obejmują zwykle co najmniej jedną z następujących zadań:

- Obciążenie Procesora zadania, takie jak obliczeń matematycznych lub analizy strukturalnej modelu.
- Zadania I-O-intensywnie, takich jak wykonywanie serii transakcji miejsca do magazynowania lub Indeksowanie plików.
- Zadań, takich jak aktualizacje są danych lub przetwarzanie według harmonogramu.
- Długotrwałe przepływy pracy, takich jak realizacja zamówienia lub inicjowania obsługi administracyjnej usługi i systemy.
- Przetwarzanie ważne dane miejsce, w którym zadanie jest przekazane do lokalizacji zabezpieczyć przetwarzania. Na przykład nie może być procesu poufnych danych w aplikacji sieci web. Zamiast tego można użyć deseniu, takich jak [strażnika](http://msdn.microsoft.com/library/dn589793.aspx) przesyłanie danych są one odizolowane tle, do uzyskiwania dostępu do chronionych miejsca do magazynowania.

## <a name="triggers"></a>Wyzwalaczy

Zadań wykonywanych w tle może zostać zainicjowana na kilka różnych sposobów. Podlegają do jednej z następujących kategorii:

- [**Wyzwalacze sterowane zdarzeniami**](#event-driven-triggers). Zadanie jest uruchomiona w odpowiedzi na zdarzenie, zwykle podjętych przez użytkownika lub krok w przepływie pracy.
- [**Wyzwalacze sterowanych harmonogramu**](#schedule-driven-triggers). Zadanie jest wywoływana zgodnie z harmonogramem opartym na czasomierza. Może to być harmonogramu cyklicznego lub jednorazowe wywołanie, które podano na później.

### <a name="event-driven-triggers"></a>Sterowane zdarzeniami wyzwalaczy

Sterowane zdarzeniami wywołania użyto wyzwalacza, aby rozpocząć zadania w tle. Przykłady używania sterowane zdarzeniami wyzwalaczy obejmują:

- Interfejs użytkownika lub innego zadania umieszcza wiadomości w kolejce. Wiadomość zawiera dane o akcję, która miało miejsce, takie jak użytkownika, umieszczając zamówienia. Zadania w tle wykrywa w kolejce i wykrywa nadejście nowej wiadomości. Odczytuje wiadomości i korzysta z danych w nim jako dane wejściowe do zadania w tle.
- Interfejs użytkownika lub innego zadania zapisuje lub aktualizuje wartość w magazynie. Zadania w tle monitoruje przechowywania i wykrywa zmiany. Odczytuje dane i używa go jako dane wejściowe zadanie w tle.
- Interfejs użytkownika lub inne zadanie wysyła żądanie do punktu końcowego, takie jak identyfikator URI HTTPS lub interfejs API uwidaczniany jako usługa sieci web. Przekazuje dane, które jest wymagane do ukończenia zadania w tle jako część żądania. Punkt końcowy, usługi sieci web wywołuje zadania tła, który używa danych jako własnych danych wejściowych.

Typowe zadania, które są dostosowane do wywołania sterowane zdarzeniami przykładami przetwarzania przepływy pracy wysyłania informacji do usługi zdalnej, wysyłanie wiadomości e-mail i inicjowania obsługi administracyjnej nowych użytkowników w aplikacjach multitenant obrazu.

### <a name="schedule-driven-triggers"></a>Wyzwalacze sterowanych harmonogramu

Wywołania sterowanych harmonogram używa czasomierza, aby rozpocząć zadania w tle. Przy użyciu sterowanych harmonogram wyzwalaczy należą:

- Timer, która działa lokalnie w aplikacji lub jako część aplikacji systemu operacyjnego wywołuje zadania w tle na bieżąco.
- Czasomierza działa w innej aplikacji lub usługi czasomierza, takich jak harmonogram Azure przesyła żądanie do interfejsu API usługi sieci web na bieżąco. Interfejs API, usługi sieci web wywołuje zadania w tle.
- Zostanie uruchomiony czasomierza powoduje zadania tła można wywołać jeden raz z opóźnieniem określony czas lub w określonym czasie, oddzielnego procesu lub aplikacji.

Typowe zadania, które są dostosowane do wywołania sterowanych harmonogram przykładami procedur przetwarzanie wsadowe (na przykład aktualizowanie listy powiązanych produktów dla użytkowników na podstawie ich ostatnie zachowań), zadań rutynową przetwarzania danych (na przykład aktualizacji indeksów lub generowania wyników skumulowane), analiza danych raportów dzienny, oczyszczanie przechowywania danych i sprawdzania spójności danych.

Jeśli używasz sterowanych harmonogramu zadania, która musi uruchomić jako jedno wystąpienie, należy pamiętać o następujących czynności:

- Jeśli jest skalowany wystąpienia obliczeń, które jest uruchomiony harmonogram (na przykład maszyny wirtualnej za pomocą zaplanowanych zadań systemu Windows), konieczne będzie wielu wystąpień harmonogram uruchomiony. Te można uruchomić wielu wystąpień zadania.
- Jeśli zadania rozpocznie się przez dłużej niż okres między zdarzeniami harmonogram, harmonogram może rozpocząć kolejnego wystąpienia zadania, gdy poprzedniego jest nadal uruchomiony.

## <a name="returning-results"></a>Zwraca wyników

Zadań wykonywanych w tle asynchroniczne wykonywanie w oddzielnym procesie, a nawet w innej lokalizacji, z interfejsu użytkownika lub procesu, który wywołany zadania w tle. Najlepiej, jeśli zadania w tle są operacje "uruchomienie i zapomnij", a ich postępu realizacji nie ma wpływu na interfejs użytkownika lub proces wywołujący. Oznacza to, że proces wywołujący nie czekać na ukończenie zadania. Dlatego jej nie może automatycznie wykryć po zakończeniu zadania.

Jeśli jest wymagane zadania w tle można komunikować się z połączeń zadania, aby wskazać postępu lub zakończenia, należy zaimplementować mechanizmu to. Oto kilka przykładów są:

- Wpisz wartość wskaźnik stanu do magazynu dostępnej dla zadania interfejsu użytkownika lub rozmówcy, które można monitorować lub zaznacz tę wartość, jeśli jest wymagane. Inne dane, które zadania w tle należy powrócić do rozmówcy mogą być umieszczane w tym samym przestrzeni dyskowej.
- Ustanowić interfejsu użytkownika lub rozmówcy wykrywa na kolejka odpowiedzi. Zadania w tle można wysyłać wiadomości do kolejki wskazać ukończenia i stan. Dane, które zadania w tle należy powrócić do rozmówcy mogą być umieszczane w wiadomości. Jeśli używasz Bus usługi Azure umożliwia właściwości **parametr From** i **CorrelationId** wdrożenie tej funkcji. Aby uzyskać więcej informacji zobacz [korelacji w usługi Bus Brokered wiadomości](http://www.cloudcasts.net/devguide/Default.aspx?id=13029).
- Udostępnianie interfejsu API lub punkt końcowy z zadania w tle, dostępnej dla interfejsu użytkownika lub rozmówcy uzyskanie informacji o stanie. Dane, które zadania w tle musi zwracać do wywołującego może być dołączone do odpowiedzi.
- Masz oddzwonić do interfejsu użytkownika lub rozmówcy przy użyciu interfejsu API do oznaczania stanu w punktach wstępnie zdefiniowany lub po zakończeniu zadania w tle. Może to być za pośrednictwem zdarzenia generowane lokalnie lub mechanizmu publikowania i subskrybowania. Dane, które zadania w tle musi zwracać do wywołującego może zostać objęta ładunku żądania lub wydarzenia.

## <a name="hosting-environment"></a>Środowisko macierzyste

Zadania w tle mogą obsługiwać za pomocą zakresu usług różnych platformy Azure:

- [**Aplikacje azure Web i WebJobs**](#azure-web-apps-and-webjobs). WebJobs umożliwia wykonywanie zadania niestandardowe na podstawie zakresu różnych rodzajów skryptów lub programów wykonywalny w kontekście aplikacji sieci web.
- [**Role witryny sieci web i pracownik azure usług w chmurze**](#azure-cloud-services-web-and-worker-roles). Można wpisać kod w roli, który wykonuje jako zadania w tle.
- [**Azure maszyn wirtualnych**](#azure-virtual-machines). Jeśli używasz usługi systemu Windows lub użyć harmonogramu zadań systemu Windows, jest częściej zadań tła w dedykowanej maszyny wirtualnej.
- [**Azure partię**](./batch/batch-technical-overview.md). Jest usługą platformę, która planuje obliczeniowych pracy do uruchomienia w zbiorze zarządzanych maszyn wirtualnych i automatycznie skali obliczyć zasobów do potrzeb zadań.

W poniższej sekcji każdej z tych opcji szczegółowo opisano oraz określenie ułatwiające wybierz odpowiednią opcję.

## <a name="azure-web-apps-and-webjobs"></a>Aplikacje Azure Web i WebJobs

Azure WebJobs umożliwia wykonywanie zadań niestandardowych jako zadania w tle w aplikacji sieci Web programu Azure. WebJobs są uruchamiane w kontekście aplikacji sieci web jako ciągły proces. WebJobs także uruchamiać w odpowiedzi na zdarzenie wyzwalające harmonogramu Azure lub czynniki zewnętrzne, takie jak zmiany magazyn obiektów blob i kolejek wiadomości. Zadania można uruchomić i zatrzymana na żądanie i łagodnego zamknięcia. Jeśli stale uruchomionego WebJob kończy się niepowodzeniem, jest automatycznie ponownie. Akcje ponawiania i błędu są konfigurowane.

Po skonfigurowaniu WebJob:

- Jeśli chcesz, aby zadanie odpowiedzieć wyzwalacza sterowane zdarzeniami, należy go skonfigurować jako **uruchamianie ciągłe**. Skrypt lub program znajduje się w folderze o nazwie witryny/wwwroot-app_data/zadań i ciągły.
- Jeśli chcesz odpowiedzieć wyzwalacza sterowanych harmonogramu zadania, należy go skonfigurować jako **Uruchom zgodnie z harmonogramem**. Skrypt lub program znajduje się w folderze o nazwie witryny/wwwroot-app_data/zadań i wyzwalane.
- Jeśli wybierzesz opcję **Uruchom na żądanie** podczas konfigurowania zadania, będzie wykonywać tego samego kodu jako opcja **Uruchom zgodnie z harmonogramem** , po uruchomieniu.

Azure WebJobs Uruchom w trybie piaskownicy aplikacji sieci web. Oznacza to, że mogą mieć dostęp do zmiennych środowiska i udostępniać informacje, takie jak parametry połączenia aplikacji sieci web. Zadanie ma dostęp do Unikatowy identyfikator urządzenia, na którym działa zadanie. Parametry połączenia o nazwie **AzureWebJobsStorage** zapewnia dostęp do kolejki Azure miejsca do magazynowania, obiektów blob i tabele danych aplikacji i dostęp do usług Bus wiadomości i komunikacji. Parametry połączenia o nazwie **AzureWebJobsDashboard** zapewnia dostęp do plików dziennika Akcja zadania.

Azure WebJobs mają następujące cechy:

- **Zabezpieczenia**: WebJobs są chronione za pomocą poświadczeń wdrażania aplikacji sieci web.
- **Obsługiwane typy plików**: możesz zdefiniować WebJobs za pomocą skryptów poleceń (cmd), pliki wsadowe (bat), skryptów programu PowerShell (.ps1), urodzinową skryptów powłoki (.sh), PHP skryptów (.php) Python skryptów (.py), kod JavaScript (js) i wykonywalny programów (.exe, .jar i inne).
- **Wdrożenie**: wdrożeniem skrypty i pliki wykonywalne Azure portal, przy użyciu dodatek [WebJobsVs](https://visualstudiogallery.msdn.microsoft.com/f4824551-2660-4afa-aba1-1fcc1673c3d0) dla programu Visual Studio lub [Visual Studio 2013 aktualizacji 4](http://www.visualstudio.com/news/vs2013-update4-rc-vs) (można tworzyć i wdrażać tej opcji), przy użyciu [Zestawu SDK WebJobs Azure](./app-service-web/websites-dotnet-webjobs-sdk-get-started.md)lub kopiując je bezpośrednio do następujących lokalizacji:
  - Dla wyzwolony wykonanie: witryny/wwwroot-app_data/zadań i wyzwalane / {zadanie nazwę}
  - Wykonywanie ciągły: witryny/wwwroot-app_data/zadań i ciągły / {zadanie nazwę}
- **Rejestrowanie**: Console.Out jest traktowany (oznaczonego) jako informacje. Console.Error jest traktowana jako błąd. Masz dostęp do monitorowania i diagnostyki informacji za pomocą portalu Azure. Pliki dziennika można pobrać bezpośrednio z witryny. Są zapisywane w następujących miejscach:
  - Dla wyzwolony wykonanie: Vfs danych i zadania i wyzwalane jobName
  - Wykonywanie ciągły: Vfs danych i zadań i ciągły jobName
- **Konfiguracja**: WebJobs można skonfigurować za pomocą portalu, interfejsu API usługi REST i programu PowerShell. Za pomocą pliku konfiguracji o nazwie settings.job w tym samym katalogu głównego jako skrypt zadania o podanie informacji o konfiguracji dla zadania. Na przykład:
  - {"stopping_wait_time": 60}
  - {"is_singleton": PRAWDA}

### <a name="considerations"></a>Zagadnienia dotyczące

- Domyślnie skala WebJobs przy użyciu aplikacji sieci web. Można jednak skonfigurować zadań do wykonania w jedno wystąpienie, ustawiając właściwość konfiguracji **is_singleton** **wartość PRAWDA**. Jedno wystąpienie WebJobs są przydatne w przypadku zadań, których nie chcesz skalować lub Uruchom jako wielokrotności jednoczesnego wystąpienia, takich jak indeksowanie analiza danych i podobne zadania.
- Aby zminimalizować zadania na wydajność aplikacji sieci web, należy rozważyć utworzenie pustego wystąpienie Azure aplikacji sieci Web w nowym planie usługi aplikacji do hosta WebJobs, które mogą być długo uruchomiony lub zasób intensywnej.

### <a name="more-information"></a>Więcej informacji

- [Azure WebJobs zaleca zasobów](./app-service-web/websites-webjobs-resources.md) zawiera listę wiele przydatnych zasobów, pobierania i próbki dla WebJobs.

## <a name="azure-cloud-services-web-and-worker-roles"></a>Azure ról w sieci web i pracownik usług w chmurze

Można wykonywać zadania w tle w roli sieci web lub w roli osobnych pracownika. Przy podejmowaniu decyzji za pomocą roli Pracownik, warto rozważyć skalowalność i elastyczność wymagania, istnienia zadania, zwolnij cadence, zabezpieczeń, odporność na uszkodzenia, konfliktu, złożoności i architektury logiczne. Aby uzyskać więcej informacji zobacz [Obliczyć deseniu konsolidacji zasobów](http://msdn.microsoft.com/library/dn589778.aspx).

Istnieje kilka sposobów wykonania zadania w tle w roli usług w chmurze:

- Tworzenie Implementacja klasy **RoleEntryPoint** w roli i jego metod umożliwia wykonywanie zadania w tle. Uruchamianie zadania w kontekście WaIISHost.exe. Ich użyj metody **GetSetting** klasy **CloudConfigurationManager** , aby załadować ustawienia konfiguracji. Aby uzyskać więcej informacji zobacz [cyklu życia (Cloud Services)](#lifecycle-cloud-services).
- Za pomocą uruchamiania zadania do wykonania zadania w tle podczas uruchamiania aplikacji. Aby wymusić tych zadań, aby kontynuować działanie w tle, należy ustawić właściwość **taskType** do **tła** (Jeśli to nie zrobisz, procesu uruchamiania aplikacji będzie zatrzymanie i poczekaj na zakończenie zadania). Aby uzyskać więcej informacji zobacz [Uruchamianie zadania uruchamiania w Azure](./cloud-services/cloud-services-startup-tasks.md).
- Za pomocą WebJobs SDK do wykonania zadania w tle takich jak WebJobs inicjowanych jako zadanie uruchomienia. Aby uzyskać więcej informacji zobacz [Rozpoczynanie pracy z zestawu SDK WebJobs Azure](./app-service-web/websites-dotnet-webjobs-sdk-get-started.md).
- Za pomocą zadania uruchamiania instalowania usługi systemu Windows, który wykonuje zadania w tle jeden lub więcej. Należy ustawić właściwość **taskType** do **tła** , dlatego usługa wykonuje w tle. Aby uzyskać więcej informacji zobacz [Uruchamianie zadania uruchamiania w Azure](./cloud-services/cloud-services-startup-tasks.md).

### <a name="running-background-tasks-in-the-web-role"></a>Uruchamianie zadania w tle w roli sieci web

Główną zaletą uruchamianie zadania w tle w roli sieci web jest zapisywanie w hostingu kosztów, ponieważ nie jest wymagane do wdrożenia dodatkowe role.

### <a name="running-background-tasks-in-a-worker-role"></a>Uruchamianie zadania w tle w roli pracownika

Uruchamianie zadania w tle w roli Pracownik ma wiele zalet:

- Umożliwia zarządzanie skalowania oddzielnie dla każdego typu, roli. Na przykład może być konieczne kolejne wystąpienia ról w sieci web do obsługi bieżące obciążenie, ale mniejsza liczba wystąpień roli Pracownik, który wykonuje zadania w tle. Przez skalowania tła zadania do uruchamiania wystąpień niezależnie od ról interfejsu użytkownika, można zmniejszyć koszty obsługi przy zachowaniu wydajności.
- Jej offloads obciążenie dla zadania w tle z roli sieci web. Role w sieci web, która udostępnia interfejs użytkownika może pozostać odpowiada i może to oznaczać, że mniejszej liczby wystąpień są wymagane do obsługi danej objętości żądania od użytkowników.
- Umożliwia wdrożenie odstęp dotyczy. Każdego typu roli zaimplementować określonego zestawu zadań jasno zdefiniowane i pokrewnych. Dzięki temu projektowania i konserwowanie kod łatwiejsze, ponieważ istnieje mniej współzależność kod i funkcje między poszczególnymi rolami.
- Może pomóc w celu wyodrębnienia poufnych procesów i danych. Na przykład nie role w sieci web, implementujących interfejs muszą mieć dostęp do danych zarządzanych i kontrolowane przez rolę pracownika. Może to być przydatne w Wzmacnianie zabezpieczeń, zwłaszcza gdy użyć deseniu takich jak [Strażnika deseniem](http://msdn.microsoft.com/library/dn589793.aspx).  

### <a name="considerations"></a>Zagadnienia dotyczące

Rozważ następujące wskazówki, wybierając jak i gdzie wdrożenia zadania w tle podczas korzystania z usług w chmurze sieci web i pracownik ról:

- Hostingu zadania w tle w istniejącej roli sieci web można zapisać koszty prowadzenia roli Pracownik osobnych tylko dla tych zadań. Jest jednak może mieć wpływ na wydajność i dostępność aplikacji przypadku konfliktu przetwarzania i innych zasobów. Za pomocą roli Pracownik osobnych chroni ról w sieci web z wpływu zadania w tle długim lub zasobów.
- Jeśli zadania w tle przy użyciu klasy **RoleEntryPoint** , można łatwo przenosić do innej roli. Na przykład jeśli utworzyć klasę w roli sieci web ale później okazało się, należy uruchomić zadania w roli Pracownik, możesz przenieść Implementacja klasy **RoleEntryPoint** do roli pracownika.
- Uruchamianie zadania są przeznaczone do wykonania programu lub skryptu. Wdrażanie wykonywanie zadania w tle jako program wykonywalny może być trudne, zwłaszcza jeśli wymaga również wdrożenia zestawy zależne. Może to być ułatwia wdrażanie i definiowanie wykonywanie zadania w tle, gdy używasz uruchamiania zadań za pomocą skryptu.
- Wyjątki powodujące niepowodzenie zadanie w tle jest wpływ różnych, w zależności od sposobu ich są obsługiwane:
  - Użycie metody klasy **RoleEntryPoint** roli go ponownie, aby automatycznie uruchamia zadania spowoduje, że zadania nie powiodło się. Może to wpłynąć na dostępność aplikacji. Aby tego uniknąć, upewnij się, że możesz umieścić obsługi w klasie **RoleEntryPoint** i wszystkie zadania w tle niezawodne wyjątków. Aby ponownie uruchomić zadania, które kończą się niepowodzeniem, gdy jest to właściwe przy użyciu kodu, a zostać zgłoszony wyjątek ponowne roli tylko wtedy, gdy bezpiecznie nie można odzyskać z powodu niepomyślnego w kodzie.
  - Jeśli używasz uruchamiania zadania, to odpowiedzialne za zarządzanie wykonywanie zadań i sprawdzania, czy nie jest on.
- Zarządzanie i monitorowanie uruchamiania zadania jest trudniejsze niż metoda klasy **RoleEntryPoint** . Jednak Azure WebJobs SDK zawiera pulpitu nawigacyjnego ułatwia zarządzanie WebJobs zainicjowania zadaniom uruchamiania.

### <a name="more-information"></a>Więcej informacji

- [Obliczanie deseniu konsolidacji zasobów](http://msdn.microsoft.com/library/dn589778.aspx)
- [Rozpoczynanie pracy z zestawu SDK WebJobs Azure](./app-service-web/websites-dotnet-webjobs-sdk-get-started.md)

## <a name="azure-virtual-machines"></a>Azure maszyn wirtualnych

Zadania w tle mogą być wykonywane w sposób, który uniemożliwia wdrażania aplikacji sieci Web Azure lub usług w chmurze, lub tych opcji może nie być wygodne. Typowe przykłady są i programy wykonywalny usług systemu Windows i narzędzia innych firm. Inny przykład może być programów napisanych dla środowisko inna niż hostingu aplikacji. Na przykład być może Unix lub Linux program, który chcesz wykonać z aplikacji systemu Windows lub .NET. Można skorzystać z różnych systemów operacyjnych dla Azure maszyn wirtualnych i uruchomić usługę lub plik wykonywalny na tym komputerze wirtualną.

Aby pomóc wybierz, kiedy należy używać maszyn wirtualnych, zobacz [usługi Azure aplikacji i usług w chmurze porównanie maszyn wirtualnych](./app-service-web/choose-web-site-cloud-service-vm.md). Informacje dotyczące opcji dla maszyn wirtualnych zobacz [maszyn wirtualnych i usługa w chmurze rozmiar Azure](http://msdn.microsoft.com/library/azure/dn197896.aspx). Aby uzyskać więcej informacji na temat systemów operacyjnych i gotowych obrazów, które są dostępne dla maszyn wirtualnych zobacz [Azure Marketplace maszyn wirtualnych](https://azure.microsoft.com/gallery/virtual-machines/).

Aby rozpocząć zadania w tle w osobnym maszyny wirtualnej, masz szereg opcji:

- Zadania można wykonywać w żądanie bezpośrednio z aplikacji, wysyłając żądania do punktu końcowego, który udostępnia zadania. To przekazuje w jakiekolwiek dane, która wymaga zadania. Ten punkt końcowy wywołuje zadania.
- Można skonfigurować uruchamianie według harmonogramu przy użyciu harmonogramu lub czasomierza jest dostępna w systemie operacyjnym wybranego zadania. Na przykład w systemie Windows umożliwia harmonogramu zadań systemu Windows wykonywanie skryptów i zadania. Lub, jeśli masz zainstalowany na komputerze wirtualnych programu SQL Server, można użyć programu SQL Server Agent wykonywanie skryptów i zadania.
- Harmonogram Azure umożliwia inicjować zadania przez dodanie wiadomości do kolejki, która wykrywa zadania na lub przez wysłanie żądania interfejs API, który udostępnia zadania.

Zobacz sekcję wcześniejszych [wyzwalaczy](#triggers) Aby uzyskać więcej informacji na temat sposobu rozpoczęciu zadania w tle.  

### <a name="considerations"></a>Zagadnienia dotyczące

Przy podejmowaniu decyzji wdrożenia zadania w tle w Azure maszyn wirtualnych, należy uwzględnić następujące punkty:

- Hostingu zadania w tle w osobnym Azure maszyny wirtualnej zapewnia elastyczność i pozwala precyzyjnie kontrolować inicjowania, wykonanie planowania i alokacji zasobów. Jednak zwiększa koszt środowisko uruchomieniowe maszyny wirtualnej musi być wdrożenie tylko wykonywać zadania w tle.
- Istnieje możliwość monitorowania zadania w Azure portal i nie możliwości automatycznego ponownego uruchomienia niepowodzeniem —, mimo że możesz monitorować podstawowe stan maszyny wirtualnej i zarządzać nim przy użyciu [Polecenia cmdlet zarządzania usługi Azure](http://msdn.microsoft.com/library/azure/dn495240.aspx). Istnieją jednak nie urządzenia do sterowania procesów i wątków w węzłach obliczeń. Zazwyczaj przy użyciu maszyny wirtualnej będzie wymagało dodatkowego nakładu pracy do wykonania mechanizmu zbiera dane z oprzyrządowania w zadaniu, z systemu operacyjnego maszyny wirtualnej. Rozwiązania, które mogą być właściwe jest używany [Pakiet administracyjny Centrum systemu dla Azure](http://technet.microsoft.com/library/gg276383.aspx).
- Rozważ utworzenie sondy monitorowania, które są dostępne za pośrednictwem protokołu HTTP punktów końcowych. Kod tych sondy może sprawdzania kondycji, zbieranie informacji operacyjnych oraz statystyk — zbiera informacje o błędzie i przywrócić aplikacji do zarządzania. Aby uzyskać więcej informacji zobacz [Zdrowia punktu końcowego monitorowania deseniem](http://msdn.microsoft.com/library/dn589789.aspx).

### <a name="more-information"></a>Więcej informacji

- W [środowisku maszyn wirtualnych systemu](https://azure.microsoft.com/services/virtual-machines/) Azure
- [Azure maszyn wirtualnych — często zadawane pytania](virtual-machines/virtual-machines-linux-classic-faq.md)

## <a name="design-considerations"></a>Uwagi dotyczące projektowania

Istnieje kilka podstawowych kwestie do rozważenia podczas projektowania zadania w tle. W poniższych sekcjach omówiono podziału, konflikty i koordynowania.

## <a name="partitioning"></a>Podziału

Jeśli zdecydujesz uwzględnić zadania w tle w ramach istniejącego wystąpienia obliczeń (na przykład aplikacji sieci web, ról w sieci web, istniejącej roli Pracownik lub maszyn wirtualnych), należy rozważyć, jak wpłynie to atrybuty jakości wystąpienie obliczeń i zadania w tle, się. Czynniki te pomogą zdecydować, czy przekazać zadań za pomocą istniejącego wystąpienia obliczeń lub rozdzielić do wystąpienia osobnych obliczeń:

- **Dostępność**: zadania w tle nie może być konieczne ma taki sam poziom dostępności innych części aplikacji, w szczególności interfejsu użytkownika i inne części, które wiążą się bezpośrednio z użytkownikiem. Zadania w tle może być bardziej elastyczne z opóźnienie, błędy próbowano połączenia i inne czynniki tego dostępność wpływ, ponieważ operacje można umieścić w kolejce. Jednak musi być wystarczające pojemności, aby uniknąć kopii zapasowej żądań, które można zablokować kolejek i wpływa na stosowanie jako całość.
- **Skalowalność**: zadania w tle mogą być wymagane skalowalność innego niż interfejsu użytkownika i interakcyjne części aplikacji. Skalowanie interfejsu użytkownika może być konieczne uzyskanie wartości na żądanie, gdy zadania zaległe tła może być wykonane podczas porach przez mniejszą liczbę wystąpień obliczeń.
- **Elastyczność**: błąd obliczeń wystąpienie, po prostu zadania w tle może nie śmiertelnie wpływa na stosowanie jako całość Jeśli żądania dla tych zadań, które mogą być w kolejce lub odroczone do czasu udostępnienia zadanie ponownie. W razie wystąpienia obliczeń i/lub zadania może zostać uruchomiony ponownie w odpowiednich odstępach, użytkowników aplikacji może być niezmieniona.
- **Zabezpieczenia**: zadania w tle może być wymagania dotyczące zabezpieczeń różnych ani ograniczeń niż interfejsu użytkownika lub inne części aplikacji. Za pomocą wystąpienia osobnych obliczeń, można określić środowisku zabezpieczeń dla zadań. Za pomocą wzorców, takich jak strażnika do wyodrębnienia wystąpień obliczeń tła z interfejsu użytkownika w celu maksymalnego bezpieczeństwa i odstęp.
- **Wydajność**: Możesz wybrać typ wystąpienia obliczeń dla tła zadań specjalnie dopasowanie wymagania zadania. Może to oznaczać, za pomocą mniej drogich opcji uruchamiania, jeśli zadania nie wymaga takie same możliwości przetwarzania jako interfejsu użytkownika lub większe wystąpienie, jeśli wymagają dodatkowych możliwości i zasobów.
- **Zarządzanie**: zadania w tle może być inny rytm projektowania i wdrażania z kodem głównej aplikacji lub interfejsu użytkownika. Wdrażanie ich wystąpieniu osobnych obliczeń można uprościć aktualizacje i przechowywanie wersji.
- **Koszt**: Dodawanie obliczyć wystąpienia wykonać podwyżki zadania tła hostingu kosztów. Zależność między dodatkowe możliwości i tych dodatkowych kosztów należy rozważyć.

Aby uzyskać więcej informacji zobacz [Znak wiodący wybory wzoru](http://msdn.microsoft.com/library/dn568104.aspx) i [uczestniczących w zawodach konsumentów](http://msdn.microsoft.com/library/dn568101.aspx).

## <a name="conflicts"></a>Konflikty

Jeśli masz wiele wystąpień zadania w tle, jest możliwe, że będzie współzawodniczenia uzyskać dostęp do zasobów i usług, takich jak bazy danych i miejsca do magazynowania. Równoczesne dostęp może spowodować konfliktu zasobów, które mogą powodować konflikty w dostępność usług i integralności danych w magazynie. Pesymistyczny metody blokowania można rozwiązać konfliktu zasobów. Dzięki temu uczestniczących w zawodach wystąpienia zadania z jednocześnie uzyskiwania dostępu do usługi lub uszkodzenia danych.

Innym sposobem rozwiązywania konfliktów jest określenie zadania w tle jako pojedyncza, aby była ona tylko kiedykolwiek jedno wystąpienie jest uruchomione. Jednak dzięki temu korzyści niezawodności i wydajności, które umożliwiają konfiguracji wystąpienia wielokrotności. To jest szczególnie w przypadku interfejsu użytkownika może dostarczyć wystarczające pracy do zachowania zajęty więcej niż jednego zadania w tle.

Ważne jest zapewnienie, że zadania w tle automatycznie można uruchomić ponownie i czy jest wystarczające zdolności do radzenia sobie z wartości na żądanie. Można to osiągnąć przydzielając wystąpieniem obliczeń środki wystarczające wdrażając mechanizm kolejkowanie, który można przechowywać żądania później, gdy zmniejszy żądanie lub przy użyciu kombinacji tych technik.

## <a name="coordination"></a>Można je zsynchronizować

Zadania w tle może być złożone i może wymagać wielu pojedynczych zadań do wykonania do uzyskania wyniku lub spełniają wszystkie wymagania. Jest wspólny dla tych scenariuszy, aby podzielić zadanie na mniejszych niejawnego czynności lub podzadania, które mogą być wykonywane przez wielu odbiorców. Wieloetapowym zadania może być bardziej efektywne i bardziej elastyczne, ponieważ poszczególne czynności mogą się do ponownego użycia w wielu zadań. Również jest łatwe dodawanie, usuwanie lub modyfikowanie kolejność kroków.

Koordynowanie wielu zadań i kroków może być trudne, ale istnieją trzy typowe wzorce, których można Przewodnik po implementacji rozwiązanie:

- **Decomposing zadania do wielu kroków do ponownego użycia**. Aplikacja może być konieczne wykonywanie wielu zadań o różnych szerokościach złożoności na przetwarza informacje. Proste, ale sztywne podejście do wykonania tej aplikacji może być przetwarzania jako moduł wbudowanymi. Jednak to podejście jest prawdopodobnie w celu zmniejszenia możliwości Refaktoryzacja kod, jego optymalizację lub ponowne używanie go, jeśli części samej przetwarzania są wymagane w innym miejscu w aplikacji. Aby uzyskać więcej informacji zobacz [potoki i filtry deseniem](http://msdn.microsoft.com/library/dn568100.aspx).
- **Zarządzanie wykonanie działania dla zadania**. Aplikacji może wykonywać zadania, które składają się kilka kroków (niektóre z nich może wywołać usługi zdalnej lub uzyskać dostęp do zasobów zdalnych). Poszczególne kroki mogą być niezależne od siebie, ale są one orchestrated przez logiki aplikacji, która wykonuje zadanie. Aby uzyskać więcej informacji zobacz [Harmonogram agenta inspektora deseniem](http://msdn.microsoft.com/library/dn589780.aspx).
- **Zarządzanie odzyskiwania czynności zadania, które kończą się niepowodzeniem**. Aplikacji może być konieczne cofnąć pracy, do której jest wykonywane przez pewne czynności (które definiują ostatecznie spójne działanie) Jeśli jeden lub więcej czynności kończą się niepowodzeniem. Aby uzyskać więcej informacji zobacz [Wyrównywania deseń transakcji](http://msdn.microsoft.com/library/dn589804.aspx).

## <a name="lifecycle-cloud-services"></a>Cykl życia (usługami w chmurze)

 Jeśli zdecydujesz się na wykonania zadań wykonywanych w tle dla aplikacji usług w chmurze, korzystających z sieci web i pracownik ról przy użyciu klasy **RoleEntryPoint** , jest pamiętać cyklu życia tej klasy, aby móc jej używać poprawnie.

Role w sieci Web i pracownik przejść przez zbiór różne fazy podczas uruchamiania, uruchamianie i zatrzymywanie. Klasa **RoleEntryPoint** udostępnia serię zdarzeń wskazujące, gdy występują te etapy. Skorzystaj z poniższych zainicjować, uruchomić i zatrzymywanie zadań niestandardowe tło. Pełny cykl jest:

- Azure ładowania zestawu ról i wyszukuje zajęć, który pochodzi od **RoleEntryPoint**.
- Jeśli znajdzie tej klasy połączeń **RoleEntryPoint.OnStart()**. Ta metoda zainicjować zadania w tle można zastąpić.
- Po zakończeniu metody **OnStart** Azure telefoniczne, które **Application_Start()** w pliku globalnego aplikacji, jeśli jest Prezentuj (na przykład Global.asax w roli sieci web programu ASP.NET uruchomiony).
- Azure połączeń **RoleEntryPoint.Run()** na nowego wątku pierwszego planu, który wykonuje równolegle z **OnStart()**. Zastąpienie tej metody, aby rozpocząć zadania w tle.
- Po zakończeniu metody Run Azure najpierw połączeń **Application_End()** w pliku globalnej aplikacji, jeśli to występuje, a następnie wywołuje **RoleEntryPoint.OnStop()**. Można zastąpić metodę **OnStop** zatrzymać zadania w tle, Oczyść zasobów pozbycie się obiekty i zamknąć połączenia, które może być używane zadania.
- Proces roboczy Azure roli hosta jest zatrzymana. W tym momencie roli zostanie odtworzony i uruchomi się ponownie.

Więcej informacji i przykład zastosowania metody klasy **RoleEntryPoint** zobacz [Obliczyć deseniu konsolidacji zasobów](http://msdn.microsoft.com/library/dn589778.aspx).

## <a name="considerations"></a>Zagadnienia dotyczące

Jeśli planujesz, jak będzie wykonywać zadania w tle w sieci web lub pracownika roli, należy rozważyć następujące punkty:

- Domyślne **Uruchamianie** implementacji metody w klasie **RoleEntryPoint** zawiera wywołanie **Thread.Sleep(Timeout.Infinite)** czas nieokreślony zachowuje aktywności roli. Jeśli zastąpić metody **Run** (jest to zazwyczaj niezbędne do wykonywania zadania w tle), nie należy zezwolić na kodzie wyjść z metody, chyba że chcesz Kosza wystąpienie roli.
- Typowe stosowania metody **Run** zawiera kod, aby rozpocząć każdego zadania w tle i konstrukcji pętli okresowo sprawdza, czy stan wszystkich zadań w tle. Ją ponownie, który się nie powieść lub monitorowanie tokeny anulowania, wskazujące ukończenie zadania.
- Jeśli zadanie w tle zgłasza powodu nieobsługiwanego wyjątku, to zadanie powinny być odtwarzane umożliwienie inne zadania w tle w roli, aby nadal działać. Jednak jeśli wyjątek przyczyną jest uszkodzenie obiektów poza zadania, na przykład udostępnionej przestrzeni dyskowej, wyjątek powinny być obsługiwane przez swojej klasy **RoleEntryPoint** , wszystkie zadania należy anulowana, a metody **Run** powinny mieć możliwość zakończenia. Azure ponownym roli.
- Użyj metody **OnStop** w celu wstrzymywania lub skasować zadania w tle i Oczyść zasobów. To może obejmować zatrzymywanie długim lub wieloetapowym zadań. Koniecznie należy rozważyć, jak można to zrobić, aby uniknąć niespójności danych. Jeśli w przypadku wystąpienia roli przestaje dowolnego powodu niż zamknięcia zainicjowany przez użytkownika, uruchomiony w metodzie **OnStop** kod musi zostać wypełnione w ciągu 5 minut, zanim zostanie zakończone wymusić. Zapewnić, że kod można zrealizować w danym czasie przeszkadzają nie działa do zakończenia.  
- Zostanie uruchomiony równoważenia obciążenia Azure, przekierowując ruch do wystąpienia roli, gdy metoda **RoleEntryPoint.OnStart** zwraca wartość **PRAWDA**. Dlatego warto rozważyć wprowadzenie kodu inicjowanie metody **OnStart** tak, aby wystąpień roli, które nie pomyślnego zainicjowania nie będziesz odbierać ruch.
- Możesz użyć zadania uruchamiania oprócz metody klasy **RoleEntryPoint** . Uruchamianie zadania należy używać zainicjować ustawienia, które należy zmienić w równoważenia obciążenia Azure, ponieważ te zadania będą przed roli otrzymuje żądań. Aby uzyskać więcej informacji zobacz [Uruchamianie zadania uruchamiania w Azure](./cloud-services/cloud-services-startup-tasks.md).
- W zadaniu uruchamiania występuje błąd, może wymusić roli, aby stale Uruchom ponownie. To może uniemożliwić wykonywanie wirtualnej zamiany adres IP (VIP) Powrót do poprzednio etapowej wersji, ponieważ wymaga wymiany wyłączny dostęp do roli. Nie można to uzyskać podczas ponownego uruchamiania roli. Aby rozwiązać ten problem:
    -  Dodaj następujący kod do początku **OnStart** i **Uruchamianie** metod w roli użytkownika:

    ```C#
    var freeze = CloudConfigurationManager.GetSetting("Freeze");
    if (freeze != null)
    {
        if (Boolean.Parse(freeze))
        {
            Thread.Sleep(System.Threading.Timeout.Infinite);
        }
    }
    ```

   - Dodawanie definicji ustawienie **Zablokuj** jako wartość logiczna ServiceDefinition.csdef i ServiceConfiguration. *pliki .cscfg roli i ustaw ją na* *FAŁSZ* *. Jeśli roli przechodzi do trybu powtarzających się ponowne uruchomienie komputera, możesz zmienić to ustawienie w celu * *PRAWDA** blokowanie wykonanie roli i umożliwić jego zamianę przy użyciu poprzedniej wersji.

## <a name="resiliency-considerations"></a>Zagadnienia dotyczące elastyczności

Zadania w tle musi być mechanizm w celu zapewnienia zaufania usługi aplikacji. Podczas planowania i projektowania zadania w tle, rozważ następujące wskazówki:

- Zadania w tle muszą mieć możliwość obsługiwać uruchomieniu roli lub usługi bez uszkodzenia danych lub wprowadzenie niespójności w aplikacji. Długotrwałe lub wieloetapowym zadań należy rozważyć _punktów kontrolnych_ , zapisując stan zadania w wygenerowanie lub jako wiadomości w kolejce, jeśli jest to właściwe. Można na przykład utrzymują informacje o stanie w wiadomości w kolejce i stopniowo zaktualizować informacje o stanie z postępu zadania, tak aby zadania mogą być przetwarzane z ostatniego znanej dobrej punktu kontrolnego — zamiast ponownego uruchomienia od początku. Korzystanie z kolejek Bus usługi Azure, możesz korzystać sesji wiadomości umożliwiający tego samego scenariusza. Sesje umożliwia zapisywanie i pobieranie stan przetwarzania aplikacji przy użyciu metody [SetState](http://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagesession.setstate.aspx) i [GetState](http://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagesession.getstate.aspx) . Aby uzyskać więcej informacji o projektowaniu niezawodne wieloetapowym procesów i przepływy pracy zobacz [Harmonogram agenta opiekuna deseniu](http://msdn.microsoft.com/library/dn589780.aspx).
- Korzystając z sieci web lub pracownika ról do obsługi wielu zadania w tle, projektowanie usługi Zastępowanie metody **Run** , aby monitorować nie powiodło się lub zablokowane zadań i uruchomić je ponownie. W przypadku, gdy nie jest praktyczne i korzystasz z roli Pracownik, wymusić roli Pracownik go ponownie podczas zamykania z metody **Run** .
- Kiedy używać kolejek można komunikować się z zadania w tle, kolejek może działać jako buforu do przechowywania żądania, które są wysyłane do zadań podczas aplikacji znajduje się w obszarze wyższymi niż normalny ładowania. Dzięki temu zadań w celu zapoznaj się z interfejsu użytkownika w okresach. Oznacza to również, że odtwarzanie roli nie blokuje interfejsu użytkownika. Aby uzyskać więcej informacji zobacz [opartych na kolejkach deseniu bilansowania obciążenia](http://msdn.microsoft.com/library/dn589783.aspx). Jeśli niektóre zadania są ważniejszych niż inne, warto rozważyć implementacji [Priorytet kolejki deseniu](http://msdn.microsoft.com/library/dn589794.aspx) aby upewnić się, że te zadania uruchamiane przed mniej ważne z nich.
- Zadania w tle zainicjowane przez wiadomości lub wiadomości proces musi być przeznaczony do obsługi niespójności, takich jak wiadomości przychodzące kolejności, wiadomości, które wielokrotnie powodują wystąpienie błędu (często nazywane _wiadomości_) i wiadomości, które są dostarczane w więcej niż jeden raz. Należy rozważyć następujące kwestie:
  - Wiadomości, które muszą być przetwarzane w określonej kolejności, takie jak kontakty, które zmieniają się danych na podstawie istniejących wartości danych (na przykład dodawanie wartości do istniejącej wartości), może nie dotrzeć w kolejności, w jakiej zostały wysłane. Alternatywnie może być obsługiwane przez innego wystąpienia zadania w tle w innej kolejności z powodu zróżnicowanych obciążenie każdego wystąpienia. Wiadomości, które muszą zostać przetworzone w określonej kolejności obejmuje numerem, klucz lub drugi wskaźnik zadania w tle można użyć, aby upewnić się, że są przetwarzane w odpowiedniej kolejności. Jeśli używasz Bus usługi Azure umożliwia sesji wiadomości gwarantuje kolejności dostarczenia. Jednak jest zazwyczaj bardziej efektywne, jeśli to możliwe zaprojektować proces tak, aby kolejność wiadomości nie ma znaczenia.
  - Zazwyczaj zadania w tle będą wglądu wiadomości w kolejce, aby tymczasowo ukryć je z innych odbiorców wiadomości. Następnie usuwa wiadomości po zakończeniu przetwarzania. Jeśli zadanie w tle kończy się niepowodzeniem, gdy przetwarzania wiadomości, wiadomości, to pojawi się ponownie w kolejce po wygaśnięciu limitu czasu wglądu. Jest ono przetwarzane przez inne wystąpienie zadania lub tylko podczas następnego cyklu przetwarzania tego wystąpienia. Jeśli wiadomość zawsze powoduje błąd w konsumenta, jego blokuje zadania, kolejki i ewentualnie samej aplikacji gdy zapełni kolejki. Dlatego jest niezbędne do wykrywanie i usuwanie wiadomości z kolejki. Jeśli używasz Bus usługi Azure wiadomości, które powodują wystąpienie błędu mogą być przenoszone automatycznie lub ręcznie do kolejki skojarzone utraconych wiadomości.
  - Kolejki jest gwarantowana w _najmniej raz_ mechanizmy, ale ich tego samego komunikatu mogą dostarczyć więcej niż jeden raz. Ponadto jeśli zadanie w tle nie powiedzie się, po przetworzeniu wiadomości, ale zanim zostanie usunięta z kolejki, wiadomość zmieni się do przetwarzania ponownie. Zadania w tle powinny być idempotent, co oznacza, że więcej niż jeden raz przetwarzania tego samego komunikatu nie powoduje błąd lub niespójności w danych aplikacji. Niektóre operacje naturally są idempotent, takich jak wartość przechowywanych na określonym nową wartość. Jednak operacji, takich jak dodawanie wartości do istniejącej wartości przechowywanej bez sprawdzania, czy wartość przechowywana nadal jest taka sama jak kiedy wiadomość została wysłana spowoduje niespójności. Aby automatycznie usunąć zduplikowane wiadomości można skonfigurować kolejki Bus usługi Azure.
  - Niektóre wiadomości, takie jak kolejki Azure miejsca do magazynowania i kolejki Bus usługi Azure, systemy de-queue właściwości liczba, która wskazuje, ile razy odczytywania wiadomości z kolejki. Może to być przydatne w obsługi powtarzających się i poison wiadomości. Aby uzyskać więcej informacji zobacz [Asynchroniczne Elementarz wiadomości](http://msdn.microsoft.com/library/dn589781.aspx) i [Desenie Idempotency](http://blog.jonathanoliver.com/2010/04/idempotency-patterns/).

## <a name="scaling-and-performance-considerations"></a>Uwagi dotyczące skalowania i wydajności

Zadania w tle musisz oferować wydajność wystarczającą do zapewnienia nie zablokować tę aplikację, lub powodować niespójności z powodu operacji opóźnione, gdy system jest obciążeniu. Zazwyczaj zwiększona wydajność przez skalowania wystąpienia obliczeń, które obsługują zadania w tle. Podczas planowania i projektowania zadania w tle, należy wziąć pod uwagę następujące punkty wokół wydajność i skalowalność:

- Autoscaling Azure obsługuje (Skalowanie zewnętrzne i skalowanie się ponownie) na podstawie bieżącej żądanie i ładowanie — lub wstępnie zdefiniowaną zgodnie z harmonogramem, aplikacjach sieci Web web usług w chmurze i ról pracownika i maszyn wirtualnych umieszczony wdrożenia. Aby upewnić się, że aplikacji jako całość ma wystarczające możliwości wydajności minimalizując kosztów obsługi za pomocą tej funkcji.
- W przypadku, gdy zadania w tle mają możliwości różnych wydajności z innych części aplikacji usług w chmurze (na przykład interfejsu użytkownika lub składniki, takie jak warstwa dostępu do danych), hostingu zadania w tle w roli Pracownik oddzielnym umożliwia interfejsu użytkownika i tła role zadania niezależne skalowanie do zarządzania Załaduj. Jeśli wiele zadania w tle jest możliwości wydajności znacznie różni się od siebie, rozważ dzieląc w osobnym pracownik role i skalowanie każdego typu roli niezależnie. Jednak pamiętać, że to może zwiększyć kosztów obsługi w porównaniu z łączenie wszystkich zadań w mniejszą liczbę ról.
- Po prostu skalowanie ról może nie być wystarczające, aby zapobiec utracie wydajności obciążeniu. Również może być konieczne skalowanie kolejki miejsca do magazynowania i inne zasoby, aby wyeliminować pojedynczy punkt całkowitej przetwarzania łańcuch staje się gardło. Rozważ też inne ograniczenia, takich jak maksymalnej przepustowości miejsca do magazynowania i innych usług, korzystających z aplikacji i zadania w tle.
- Zadania w tle muszą być zaprojektowane skalowania. Na przykład muszą być w stanie dynamicznie wykrywanie liczby kolejek miejsca do magazynowania w użyciu, aby odsłuchać w lub wysyłanie wiadomości do odpowiedniej kolejki.
- Domyślnie WebJobs skali ich skojarzone wystąpienie Azure aplikacji sieci Web. Jednak jeśli chcesz WebJob, aby uruchomić jako jedno wystąpienie, można utworzyć plik Settings.job, który zawiera dane JSON **{"is_singleton": PRAWDA}**. Wymusza Azure, aby uruchomić tylko jedno wystąpienie WebJob, nawet w przypadku wielu wystąpień aplikacji sieci web skojarzone. Może to być przydatne technika zaplanowanych zadań, które należy uruchomić jako jedno wystąpienie.

## <a name="related-patterns"></a>Desenie pokrewne

- [Asynchroniczne Elementarz wiadomości](http://msdn.microsoft.com/library/dn589781.aspx)
- [Wskazówki dotyczące Autoscaling](http://msdn.microsoft.com/library/dn589774.aspx)
- [Wyrównywania transakcji deseniem](http://msdn.microsoft.com/library/dn589804.aspx)
- [Konkurujących deseniu odbiorcy](http://msdn.microsoft.com/library/dn568101.aspx)
- [Obliczanie podziału wskazówki](http://msdn.microsoft.com/library/dn589773.aspx)
- [Obliczanie deseniu konsolidacji zasobów](http://msdn.microsoft.com/library/dn589778.aspx)
- [Wzór strażnika](http://msdn.microsoft.com/library/dn589793.aspx)
- [Znak wiodący wybory deseniu](http://msdn.microsoft.com/library/dn568104.aspx)
- [Potoki i deseniu filtrów](http://msdn.microsoft.com/library/dn568100.aspx)
- [Priorytet kolejki deseniu](http://msdn.microsoft.com/library/dn589794.aspx)
- [Ładowanie opartych na kolejkach bilansowanie deseniem](http://msdn.microsoft.com/library/dn589783.aspx)
- [Harmonogram agenta opiekuna deseniem](http://msdn.microsoft.com/library/dn589780.aspx)

## <a name="more-information"></a>Więcej informacji

- [Skalowanie Azure aplikacji z rolami pracownika](http://msdn.microsoft.com/library/hh534484.aspx#sec8)
- [Wykonywanie zadania w tle](http://msdn.microsoft.com/library/ff803365.aspx)
- [Cykl życia uruchamiania roli Azure](http://blog.syntaxc4.net/post/2011/04/13/windows-azure-role-startup-life-cycle.aspx) (wpis w blogu)
- [Cykl życia roli usług Azure chmury](http://channel9.msdn.com/Series/Windows-Azure-Cloud-Services-Tutorials/Windows-Azure-Cloud-Services-Role-Lifecycle) (wideo)
- [Rozpoczynanie pracy z Azure WebJobs SDK](./app-service-web/websites-dotnet-webjobs-sdk-get-started.md)
- [Azure kolejki i usługa Bus kolejki — porównanie i porównanie oznaczonymi](./service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md)
- [Jak włączyć diagnostyki w usłudze w chmurze](./cloud-services/cloud-services-dotnet-diagnostics.md)
