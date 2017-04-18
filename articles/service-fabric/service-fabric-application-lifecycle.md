<properties
   pageTitle="Cykl życia aplikacji na tkaninie usługi | Microsoft Azure"
   description="W tym artykule opisano opracowywania, wdrażania, testowanie, uaktualniania, utrzymania i usuwanie aplikacji usług tkaninie."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>


<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/25/2016"
   ms.author="ryanwi"/>


# <a name="service-fabric-application-lifecycle"></a>Cykl życia aplikacji tkaninie usługi
Jak na innych platformach, aplikacji na tkaninie usługi Azure zazwyczaj przechodzi przez następujące etapy: projektowanie, rozwój, testowania, wdrażania, uaktualniania, konserwacji i usuwania. Usługa tkaninie zapewnia obsługę najlepszych cyklu życia pełne zastosowanie chmury aplikacjom, od projektowania do wdrożenia, zarządzania infrastrukturą i konserwacja ewentualne likwidować. Model usługi udostępnia kilka różne role użytkownika niezależnie uczestniczyć w cyklu życia aplikacji. W tym artykule omówiono za pośrednictwem interfejsów API i jak są używane przez różne role w fazy świadczenia usługi tkaninie aplikacji.

## <a name="service-model-roles"></a>Role modelu usługi
Role modelu usługi są:

- **Usługa Deweloper**: rozwija moduły i ogólne usługi, które można ponownie celu i używane w wielu aplikacjach tego samego typu lub różnych typów. Na przykład usługa kolejki może służyć do tworzenia śledzenia biletów (zgłoszeń) lub elektronicznego aplikacja (koszyk).

- **Deweloper aplikacji**: tworzy aplikacje integracja zbiór usług spełnia określonych wymagań lub scenariuszy. Na przykład elektronicznego witryny sieci Web może zintegrować "JSON Stateless frontonu usługi", "Aukcji Stateful Usługa" oraz "kolejki Stateful" auctioning rozwiązania.

- **Administrator aplikacji**: sprawia, że decyzje dotyczące konfiguracji aplikacji (wypełnianie parametry szablonu konfiguracji), wdrażania (mapowanie dostępnych zasobów) i jakość usługi. Na przykład administrator aplikacji zdecyduje język ustawienia regionalne (w języku angielskim dla Stanów Zjednoczonych) lub japońskim w Japonii, na przykład aplikacji. Różne wdrożonej aplikacji mają różne ustawienia.

- **Operator**: wdrożenie aplikacji na podstawie konfiguracji aplikacji oraz wymagania określone przez administratora aplikacji. Na przykład operator przepisy i wdrożenie aplikacji i gwarantuje, że jest on uruchomiony w Azure. Operatory monitorować informacje o kondycji i wydajności aplikacji i obsługa fizycznej infrastruktury, stosownie do potrzeb.


## <a name="develop"></a>Można opracowywać
1. *Usługa Deweloper* rozwija różnych typów usług przy użyciu modelu programowania [Aktorów zaufanego](service-fabric-reliable-actors-introduction.md) lub [Zaufanego usług](service-fabric-reliable-services-introduction.md) .
2. *Deweloper usługi* opisano sposób deklaratywny typy opracowanym przez usługę w pliku manifestu usługi składający się z jednego lub kilku pakietów kodu, konfiguracji i danych.
3. *Deweloper aplikacji* utworzy aplikacji przy użyciu różnych typów.
4. *Deweloper aplikacji* deklaratywnie opisuje typ aplikacji w manifest aplikacji przez odwołanie manifesty usługi składowe usług i odpowiednio zastępowanie i parametryzacja różne ustawienia konfiguracji i wdrażanie usług składowych.

Zobacz [Rozpoczynanie pracy z zaufanego uczestników](service-fabric-reliable-actors-get-started.md) i [rozpocząć pracę z usługami zaufanego](service-fabric-reliable-services-quick-start.md) przykłady.

## <a name="deploy"></a>Wdrażanie
1. *Administrator aplikacji* dostosowanie typu aplikacji do określonej aplikacji do wdrożenia do klastrów tkaninie usługi, określając odpowiednie parametry elementu **ApplicationType** w manifeście aplikacji.

2. *Operator* wysyła pakiet aplikacji do sklepu obraz klaster przy użyciu [metody **CopyApplicationPackage** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage.aspx) lub polecenia [cmdlet **ServiceFabricApplicationPackage kopii** ](https://msdn.microsoft.com/library/azure/mt125905.aspx). Pakiet aplikacji zawiera manifest aplikacji i zbiór pakietów usług. Usługa tkaninie wdraża aplikacji przechowywane w magazynie obraz, który może być magazyn obiektów blob platformy Azure lub usługa systemowa tkaninie usługi pakiet aplikacji.

3. Następnie *operator* Inicjuje obsługę administracyjną typ aplikacji w klastrze docelowej z pakiet aplikacji przekazywanych przy użyciu [metody **ProvisionApplicationAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync.aspx), polecenia [cmdlet **ServiceFabricApplicationType rejestru** ](https://msdn.microsoft.com/library/azure/mt125958.aspx)lub [operacji pozostałych **Obsługa administracyjna aplikacji** ](https://msdn.microsoft.com/library/azure/dn707672.aspx).

4. Po zainicjowaniu obsługi administracyjnej aplikacji, *operator* uruchamia aplikację z parametrami określone przez *administratora aplikacji* przy użyciu [metody **CreateApplicationAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.createapplicationasync.aspx), polecenia [cmdlet **New-ServiceFabricApplication** ](https://msdn.microsoft.com/library/azure/mt125913.aspx)lub [ **Tworzenie aplikacji** REST operacji](https://msdn.microsoft.com/library/azure/dn707676.aspx).

5. Po wdrożeniu aplikacji, *operator* używa [metody **CreateServiceAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.servicemanagementclient.createserviceasync.aspx), polecenia [cmdlet **New-ServiceFabricService** ](https://msdn.microsoft.com/library/azure/mt125874.aspx)lub [ **Utwórz usługę** pozostałych operacji](https://msdn.microsoft.com/library/azure/dn707657.aspx) do utworzenia nowego wystąpienia usługi dla aplikacji na podstawie usług dostępnych typów.

6. Aplikacja działa teraz w klastrze tkaninie usługi.

Przykłady, zobacz temat [Deploy aplikacji](service-fabric-deploy-remove-applications.md) .

## <a name="test"></a>Test
1. Po wdrożeniu klaster rozwoju lokalnego lub klastrze test, *Deweloper usługi* uruchamiania Scenariusz testów wbudowanych pracy awaryjnej za pomocą klasy [**FailoverTestScenarioParameters**](https://msdn.microsoft.com/library/azure/system.fabric.testability.scenario.failovertestscenarioparameters.aspx) i [**FailoverTestScenario**](https://msdn.microsoft.com/library/azure/system.fabric.testability.scenario.failovertestscenario.aspx) lub polecenia [cmdlet **Wywołaj ServiceFabricFailoverTestScenario** ](https://msdn.microsoft.com/library/azure/mt644783.aspx). Scenariusz testów pracy awaryjnej uruchamia określonej usługi za pośrednictwem ważne przejścia i praca awaryjna, aby upewnić się, że jest nadal dostępne i działa.

2. *Usługa Deweloper* następnie uruchamia Scenariusz testów wbudowanych chaos za pomocą klasy [**ChaosTestScenarioParameters**](https://msdn.microsoft.com/library/azure/system.fabric.testability.scenario.chaostestscenarioparameters.aspx) i [**ChaosTestScenario**](https://msdn.microsoft.com/library/azure/system.fabric.testability.scenario.chaostestscenario.aspx) lub polecenia [cmdlet **Wywołaj ServiceFabricChaosTestScenario** ](https://msdn.microsoft.com/library/azure/mt644774.aspx). Scenariusz testów chaos losowo wywołuje wielu węzeł, kod pakietu i błędów replice w grupie.

3. *Deweloper usługi* [testów do usługi komunikacji](service-fabric-testability-scenarios-service-communication.md) scenariuszy testowania, które przenoszą podstawowego repliki wokół klaster w programie.

Aby uzyskać więcej informacji, zobacz [Wprowadzenie do usługi Analysis błędów](service-fabric-testability-overview.md) .

## <a name="upgrade"></a>Uaktualnianie
1. *Deweloper usługi* aktualizuje składowe usługi występującego aplikacji i/lub rozwiązania usterek i udostępnia nową wersję manifestu usługi.

2. *Deweloper aplikacji* zastępuje parameterizes ustawienia konfiguracji i wdrażanie usług spójne i zawiera nową wersję manifest aplikacji. Deweloper aplikacji, a następnie zamieszczanie nowe wersje manifesty usługi w aplikacji i zawiera nową wersję tego typu aplikacji pakietu aplikacji zaktualizowane.

3. *Administrator aplikacji* zamieszczanie nowej wersji typ aplikacji w aplikacji docelowej aktualizując odpowiednie parametry.

5. *Operator* wysyła pakiet aplikacji zaktualizowany w magazynie obraz klaster przy użyciu [metody **CopyApplicationPackage** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage.aspx) lub polecenia [cmdlet **ServiceFabricApplicationPackage kopii** ](https://msdn.microsoft.com/library/azure/mt125905.aspx). Pakiet aplikacji zawiera manifest aplikacji i zbiór pakietów usług.

6. *Operator* Inicjuje obsługę administracyjną nowej wersji aplikacji w klastrze docelowej przy użyciu [metody **ProvisionApplicationAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync.aspx), polecenia [cmdlet **ServiceFabricApplicationType rejestru** ](https://msdn.microsoft.com/library/azure/mt125958.aspx)lub [operacji pozostałych **Obsługa administracyjna aplikacji** ](https://msdn.microsoft.com/library/azure/dn707672.aspx).

7. *Operator* uaktualnienie aplikacji docelowej do nowej wersji przy użyciu [metody **UpgradeApplicationAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.upgradeapplicationasync.aspx), polecenia [cmdlet **Start ServiceFabricApplicationUpgrade** ](https://msdn.microsoft.com/library/azure/mt125975.aspx)lub [ **uaktualnić aplikację** pozostałych operacji](https://msdn.microsoft.com/library/azure/dn707633.aspx).

8. *Operator* sprawdza postęp uaktualnienia przy użyciu [metody **GetApplicationUpgradeProgressAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.getapplicationupgradeprogressasync.aspx), polecenia [cmdlet **Get-ServiceFabricApplicationUpgrade** ](https://msdn.microsoft.com/library/azure/mt125988.aspx)lub [operacji pozostałych **Uzyskiwanie postęp uaktualnienia aplikacji** ](https://msdn.microsoft.com/library/azure/dn707631.aspx).

9. Jeśli to konieczne, *operator* modyfikuje i przywrócenie parametry bieżące uaktualnienie aplikacji przy użyciu [metody **UpdateApplicationUpgradeAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.updateapplicationupgradeasync.aspx), polecenia [cmdlet **ServiceFabricApplicationUpgrade aktualizacji** ](https://msdn.microsoft.com/library/azure/mt126030.aspx)lub [ **Aktualizacja aplikacji uaktualnienie** pozostałych operacji](https://msdn.microsoft.com/library/azure/mt628489.aspx).

10. Jeśli to konieczne, *operator* z rozwiniętymi informacjami ponownie bieżące uaktualnienie aplikacji przy użyciu [metody **RollbackApplicationUpgradeAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.rollbackapplicationupgradeasync.aspx), polecenia [cmdlet **Start ServiceFabricApplicationRollback** ](https://msdn.microsoft.com/library/azure/mt125833.aspx)lub [ **Uaktualnienie aplikacji wycofywania** pozostałych operacji](https://msdn.microsoft.com/library/azure/mt628494.aspx).

11. Usługa tkaninie uaktualnienie aplikacji docelowej uruchamiania w klastrze bez utraty dostępności jego składowe usługi.

Zobacz [aplikacji uaktualnianie samouczek](service-fabric-application-upgrade-tutorial.md) przykłady.

## <a name="maintain"></a>Obsługa
1. System operacyjny zostanie uaktualniony i poprawki tkaninie usługi interfejsów z Azure infrastruktury, aby zagwarantować dostępność wszystkich aplikacji uruchamiania w klastrze.

2. Aktualizacje i poprawki do platformy tkaninie usługi tkaninie usługi uaktualnienie samej bez utraty dostępności aplikacji uruchamiania w klastrze.

3. *Administrator aplikacji* zatwierdza dodawania lub usuwania węzłów w klastrze analiza danych wykorzystania możliwości historyczne i przewidywanych przyszłych żądanie.

4. *Operator* dodaje i usuwa węzły określonej przez *administratora aplikacji*.

5. Gdy nowe węzły zostaną dodane do lub istniejące węzły zostaną usunięte z klastrem, tkaninie usługi automatycznie ładowania salda uruchomione aplikacje we wszystkich węzłach w klastrze uzyskanie optymalnej wydajności.

## <a name="remove"></a>Usuwanie
1. *Operator* można usunąć konkretnego wystąpienia uruchomioną usługę w klastrze bez usuwania całej aplikacji przy użyciu [metody **DeleteServiceAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.servicemanagementclient.deleteserviceasync.aspx), polecenia [cmdlet **ServiceFabricService Usuń** ](https://msdn.microsoft.com/library/azure/mt126033.aspx)lub [ **Usunąć usługi** REST operacji](https://msdn.microsoft.com/library/azure/dn707687.aspx).  

2. *Operator* można również usunąć wystąpienie aplikacji i wszystkie jego usługi przy użyciu [metody **DeleteApplicationAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.deleteapplicationasync.aspx), polecenia [cmdlet **ServiceFabricApplication Usuń** ](https://msdn.microsoft.com/library/azure/mt125914.aspx)lub [ **Usuwanie aplikacji** REST operacji](https://msdn.microsoft.com/library/azure/dn707651.aspx).

3. Po zatrzymano aplikacji i usług, *operator* można wstrzymać obsługi administracyjnej typ aplikacji przy użyciu [metody **UnprovisionApplicationAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.unprovisionapplicationasync.aspx), polecenia [cmdlet **Unregister ServiceFabricApplicationType** ](https://msdn.microsoft.com/library/azure/mt125885.aspx)lub [ **Wstrzymywanie obsługi administracyjnej aplikacji** REST operacji](https://msdn.microsoft.com/library/azure/dn707671.aspx). Wstrzymanie obsługi administracyjnej typ aplikacji nie powoduje usunięcia pakiet aplikacji z ImageStore. Należy ręcznie usunąć pakiet aplikacji.

4. *Operator* usuwa ImageStore przy użyciu [metody **RemoveApplicationPackage** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.removeapplicationpackage.aspx) lub polecenia [cmdlet **ServiceFabricApplicationPackage Usuń** ](https://msdn.microsoft.com/library/azure/mt163532.aspx)pakiet aplikacji.

Przykłady, zobacz temat [Deploy aplikacji](service-fabric-deploy-remove-applications.md) .

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji o tworzeniu testowanie i zarządzanie tkaninie usługi aplikacji i usług, zobacz:

- [Niezawodne uczestników](service-fabric-reliable-actors-introduction.md)
- [Niezawodne usług](service-fabric-reliable-services-introduction.md)
- [Wdrażanie aplikacji](service-fabric-deploy-remove-applications.md)
- [Uaktualnianie aplikacji](service-fabric-application-upgrade.md)
- [Omówienie pola](service-fabric-testability-overview.md)
- [Przykładowy cykl życia aplikacja REST](service-fabric-rest-based-application-lifecycle-sample.md)
