<properties
   pageTitle="Omówienie usług tkaninie | Microsoft Azure"
   description="Omówienie tkaninie usługi, której aplikacji składają się z wielu microservices o podanie skali i odporność. Usługa jest platformy systemów dystrybucji używane do tworzenia skalowalna, niezawodne i łatwe zarządzanych aplikacjach dla chmury"
   services="service-fabric"
   documentationCenter=".net"
   authors="msfussell"
   manager="timlt"
   editor="masnider"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/22/2016"
   ms.author="mfussell"/>

# <a name="overview-of-service-fabric"></a>Omówienie usług tkaninie
Jest to platforma systemów dystrybucji, która ułatwia pakowanie, wdrażanie i zarządzanie microservices skalowalna i niezawodne usługi. Usługa tkaninie dotyczy również znaczną wyzwania w opracowywania aplikacji i zarządzania nimi chmury. Deweloperzy i Administratorzy można uniknąć rozwiązywania problemów złożonych infrastruktury i fokusu zamiast tego na implementacji krytyczne, wymagających obciążenia wiedzy, że są one skalowalna, niezawodne i łatwe w zarządzaniu. Usługa tkaninie reprezentuje platformy pośredniczącym generacji tworzenia oraz zarządzania nimi te klasy korporacyjnej, aplikacje cloud skalą poziomu 1.

Ten [krótki klip wideo](https://aka.ms/servicefabricvideo) zawiera wprowadzenie do usługi tkaninie i microservices.


## <a name="applications-composed-of-microservices"></a>Aplikacje składający się z microservices
Usługa tkaninie umożliwia tworzenie i zarządzanie nimi skalowalna i niezawodne aplikacje składający się z microservices uruchomionych bardzo wysoki gęstości udostępnionej puli maszyn (określone jako klaster). Udostępnia zaawansowane środowisko uruchomieniowe do tworzenia rozłożone, skalowalna microservices bezstanowe i stanowe. Umożliwia także pełna aplikacji zarządzania możliwości obsługi administracyjnej, wdrażanie, monitorowanie, uaktualniania poprawki, a usuwanie wdrożony aplikacji.

Dlaczego jest podejście microservices istotne? Są dwa główne powody:

1. Umożliwiają skalowanie różne części aplikacji, w zależności od jego potrzeb.

2. Członkom zespołu deweloperów mogą być bardziej adaptacyjne w procesie rozmieszczania zmiany, a więc zapewniają funkcje klientom szybszą i bardziej często.

Usługa tkaninie uprawnień wielu usługi firmy Microsoft dzisiaj, łącznie z bazy danych SQL Azure, Azure DocumentDB, Cortana Power BI, Intune firmy Microsoft, koncentratory zdarzenia Azure, Azure IoT, Skype dla firm i wiele podstawowe usług Azure.

Tkaninie usługi są dostosowane do tworzenia chmury natywnych usług, które można uruchomić małe, stosownie do potrzeb, a rosnąć do ogromną skalę z setki lub tysiące komputerów.

Usług internetowych skali dzisiejszą utworzono z microservices. Microservices przykładami protokół bram, profilów użytkowników, zakupy wózki zapasów przetwarzania, kolejki i umieszcza w pamięci podręcznej. Usługa jest platformy microservices, która zapewnia każdej microservice unikatową nazwę, która może być bezstanowym lub stanowe.

Tkaninie usługi zapewnia pełna możliwości obsługi i cyklu zarządzania aplikacjom składający się z tych microservices. Obsługuje microservices wewnątrz kontenerów wdrożony i aktywować w klastrze tkaninie usługi. Przenoszenie z maszyny wirtualne kontenerów powoduje, że możliwości wzrost kolejności wielkość w gęstości. Podobnie innego rzędu w gęstości staje się możliwe przez przeniesienie z kontenerów do microservices. Na przykład jeden klaster bazy danych SQL Azure obejmuje setki komputerów z systemem dziesiątki tysięcy kontenerów hostingu sumę tysiące baz danych. Każda baza danych jest microservice stanowe tkaninie usługi. To samo dotyczy innych usług wcześniej wspomniano, tworzonym terminów "informatycznych" jest używany do opisu usługi tkaninie możliwości. Jeśli kontenerów dają dużej gęstości, następnie microservices umożliwiają informatycznych.

Aby uzyskać więcej informacji w ujęciu microservices, przeczytaj [Dlaczego podejście microservices do tworzenia aplikacji?](service-fabric-overview-microservices.md)

## <a name="container-deployment-and-orchestration"></a>Kontener wdrożenia i aranżacji
Usługa jest [orchestrator](service-fabric-cluster-resource-manager-introduction.md) microservices klastrze maszyn. Microservices może być realizowana na różne sposoby możliwość korzystania z [modelach programowania tkaninie usługi](service-fabric-choose-framework.md) wdrożeniem [pliki wykonywalne gościa](service-fabric-deploy-existing-app.md). Usługa tkaninie można wdrażać usług w kontenerze obrazy i ważne można łączyć zarówno usług w procesów, jak i usługi w kontenerach razem w tej samej aplikacji. Jeśli chcesz tylko [Wdrażanie i zarządzanie nią obrazy kontenera](service-fabric-containers-overview.md) klastrze maszyn, usługa jest doskonałym rozwiązaniem dla tego.


## <a name="create-service-fabric-clusters-anywhere"></a>Tworzenie klastrów tkaninie usługi w dowolnym miejscu
Możesz utworzyć klastrów tkaninie usługi w wielu środowiskach, łącznie z Azure lub lokalnie, w systemie Windows Server lub w systemie Linux. Ponadto środowisko projektowania w zestawie SDK jest identyczny z środowisku produkcyjnym z nie emulatory związane. Innymi słowy gdy jest ono uruchomione w klastrze rozwoju lokalnego go wdraża w tym samym klastrze w innych środowiskach.

Aby uzyskać więcej informacji na temat tworzenia klastrów lokalnego, znaleźć w artykule [Tworzenie klaster na serwerze systemu Windows i Linux oraz](service-fabric-deploy-anywhere.md) lub Azure tworzenie klaster [przez Azure portal](service-fabric-cluster-creation-via-portal.md).

![Platformy tkaninie usługi][Image1]

## <a name="stateless-and-stateful-service-fabric-microservices"></a>Bezstanowe i stanowe microservices tkaninie usługi

Usługa tkaninie umożliwia tworzenie aplikacji składający się z microservices. Bezstanowa microservices (protokół bram, web proxy itp.) nie obsługują tych stan poza dowolnego danego żądania i odpowiedzi w usłudze. Azure role pracownika usług w chmurze są przykładem stateless usługi. Stanowe microservices (konta użytkowników, baz danych, urządzenia, zakupów wózki kolejek, itp.) Obsługa tych autorytatywnych stanie poza żądania i odpowiedzi. Aplikacje Internet skali dzisiejszą składają się z kombinacji bezstanowe i stanowe microservices.

Dlaczego warto mieć stanowe microservices wraz z nich stateless? Są dwa główne powody:

1. Możliwość tworzenia wysokiej przepustowości, małym opóźnieniem, na uszkodzenia błąd przetwarzania transakcji online (OLTP) usług, zachowując kod i zamknij danych na tym samym komputerze. Kilka przykładów są interakcyjne sklepy, wyszukiwania, systemów Internet czynności (IoT), systemów obrotu, systemy wykrywania przetwarzania i oszustwa karty kredytowej i zarządzanie typami rekordów osobistych.

2. Uproszczenia projektu aplikacji. Microservices stanowe konieczności dodatkowe kolejki i umieszcza w pamięci podręcznej, zazwyczaj konieczne do usunięcia dostępności i opóźnienie wymagania aplikacja czysto bezstanowa. Stanowe usług są sposób naturalny wysokiej dostępności i małym opóźnieniem, co ogranicza liczbę ruchomych elementów do zarządzania w aplikacji jako całość.

Więcej informacji na temat wzorców aplikacji z tkaninie usługi, przeczytaj [scenariuszy aplikacji](service-fabric-application-scenarios.md) i [wybierając pozycję ramy modelu programowania](service-fabric-choose-framework.md) tej usługi

## <a name="application-lifecycle-management"></a>Zarządzanie aplikacjami cyklu
Usługa tkaninie zapewnia obsługę najlepszych zarządzania cyklu życia pełne zastosowanie (cyklem tworzenia) aplikacje w chmurze — od projektowania do wdrożenia, zarządzania infrastrukturą i konserwacja do ewentualnego likwidować.

Administratorzy aplikacji możliwościom cyklem tworzenia tkaninie usługi / operatorów IT Umożliwia proste min. dotyku przepływy pracy do świadczenia, wdrażania poprawki i monitorowanie aplikacji. Te wbudowane przepływy pracy znacznie zmniejszenia obciążenia operatorów IT, aby zachować aplikacje przez cały czas dostępne.

Większość aplikacji składają się z kombinacji bezstanowe i stanowe microservices i innych plików wykonywalnych i obsługi wdrożone razem. Konfigurując Silne typy w aplikacji i microservices detaliczny tkaninie usługi umożliwia wdrażanie wielu wystąpień aplikacji. Każde wystąpienie jest zarządzane i uaktualnić niezależnie. Należy pamiętać, że usługa jest możliwość wdrożenia *dowolne* pliki wykonywalne lub środowisko uruchomieniowe i ich niezawodne. Na przykład usługa tkaninie wdraża ASP.NET Core 1, Node.js, maszyny wirtualne języka Java, skryptów lub cokolwiek innego, tworzących aplikacji.

Aby uzyskać więcej informacji o zarządzaniu cyklu życia aplikacji czytanie [cyklu życia aplikacji](service-fabric-application-lifecycle.md) i na wdrażanie kodu, zobacz temat [Deploy Gość wykonywalny](service-fabric-deploy-existing-app.md)

## <a name="key-capabilities"></a>Kluczowe funkcje
Za pomocą usługi tkaninie, możesz wykonać następujące czynności:

- Tworzenie aplikacji znacznym stopniu skalowalna, które są własny ran.

- Można opracowywać aplikacje składający się z microservices przy użyciu modelu programowania tkaninie usługi. Lub po prostu obsługi plików wykonywalnych gościa i innych środowisk aplikacji wybranych przez użytkownika, takich jak ASP.NET Core 1 lub Node.js.

- Rozwijanie wysoce niezawodne microservices bezstanowe i stanowe.

- Wdrażanie i dodać akompaniament kontenerów należą Windows kontenerów i kontenerów Docker klastrze. Te kontenery, można pliki wykonywalne gościa kontenera lub zaufanego microservices bezstanowe i stanowe.  W obu przypadkach zostanie wyświetlony kontener port mapowanie portów hosta, Wyszukiwanie kontenera i automatyzacji awaryjnego przełączania.

- Uprościć projektowanie aplikacji przy użyciu stanowe microservices zamiast pamięci podręcznej i kolejki.

- Wdrażanie Azure lub chmur lokalnym systemem Windows Server i Linux oraz z zero zmiany kodu. Jednokrotnego zapisu, a następnie wdrażać dowolne miejsce do dowolnego klastrów tkaninie usługi.

- Rozwinąć za pomocą metody "centrum danych na tym komputerze". Środowisko lokalne projektowania jest ten sam kod, którego jest uruchamiany w Azure centrach danych.

- Wdrażanie aplikacji w sekundach.

- Wdrażanie aplikacji gęstości wyższymi niż maszyn wirtualnych, wdrażanie setki lub tysiące aplikacji na komputerze.

- Wdrażanie różnych wersji tej samej aplikacji obok siebie, każdy niezależne uaktualnić.

- Zarządzanie cyklem życia stanowe aplikacji bez dowolnego przestoje, łącznie z uaktualnienia przerywanie i nierozdzielający.

- Zarządzanie aplikacjami za pomocą interfejsów API .NET, języka Java (Linux), programu PowerShell, polecenie Azure (Linux) lub pozostałych interfejsy.

- Uaktualnianie i niezależna poprawka microservices w aplikacjach.

- Monitorowanie i diagnozowanie kondycji aplikacji i ustawić zasady dotyczące wykonywania automatycznych naprawy.

- Możliwość poza skalowanie liczby węzłów na klaster, jak również Skala up w skali lub Skala szczegółów rozmiar każdego węzła, że aplikacje automatyczne skalowanie i są rozpowszechnione zgodnie dostępnych zasobów.

- Obejrzyj własny ran równoważenia zasobów dodać akompaniament ponownej dystrybucji aplikacji w klastrze. Usługa odzyskuje z błędów i optymalizuje dystrybucji obciążenia według dostępnych zasobów.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Następne kroki

* Aby uzyskać więcej informacji:
    * [Dlaczego microservices dojechać do tworzenia aplikacji?](service-fabric-overview-microservices.md)
    * [Omówienie terminologia](service-fabric-technical-overview.md)
* Konfigurowanie usługi tkaninie [środowisko projektowania](service-fabric-get-started.md)  
* [Wybieranie ramy modelu programowania](service-fabric-choose-framework.md) tej usługi

[Image1]: media/service-fabric-overview/Service-Fabric-Overview.png
