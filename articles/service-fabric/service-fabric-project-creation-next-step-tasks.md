<properties
   pageTitle="Następne kroki tworzenia projektu tkaninie usługi | Microsoft Azure"
   description="Ten artykuł zawiera łącza do zestawu podstawowych zadaniach rozwoju tkaninie usługi"
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/08/2016"
   ms.author="seanmck"/>

# <a name="your-service-fabric-application-and-next-steps"></a>Następne kroki i tkaninie usługi aplikacji usługi
Aplikacja tkaninie usługi Azure została utworzona. W tym artykule opisano w skład projektu i niektórych potencjalne następne kroki.

## <a name="your-application"></a>Aplikacja
Każdej nowej aplikacji zawiera projekt aplikacji. Czasami może występować jednej lub dwóch dodatkowe projekty, w zależności od typu wybranej usługi.

### <a name="the-application-project"></a>Projekt aplikacji
Projekt aplikacji składa się z:

- Zestaw odwołania do usług, które składają się na aplikację.

- Publikowanie dwa profile (lokalny i chmurze), które służą do obsługi preferencje dotyczące pracy z różnych środowiskach — na przykład preferencji punkt końcowy klaster i czy przeprowadzić uaktualnienie wdrożeń domyślnie.

- Dwa parametru plików aplikacji (lokalny i chmury) używane do obsługi konfiguracji specyficzne dla środowiska aplikacji, takie jak liczba partycje, aby utworzyć usługi.

- Skrypt wdrożenia, który służy do wdrażania aplikacji z poziomu wiersza polecenia lub jako część automatyczną ciągłego procesu integracji i wdrażania.

- Oczywiste aplikacji, który opisuje aplikacji. Manifest znajdują się w folderze ApplicationPackageRoot.

### <a name="stateless-service"></a>Usługa bezstanowa
Po dodaniu nowej usługi stateless Visual Studio dodaje projektu usługi do usługi rozwiązanie, które zawiera typ podrzędne `StatelessService`. Usługa zwiększa zmiennej lokalnej licznika.

### <a name="stateful-service"></a>Usługa stanowe
Po dodaniu nowej usługi stanowe Visual Studio powoduje dodanie projektu usługi do rozwiązania zawierającego typu podrzędne `StatefulService`. Usługa zwiększa licznika w jego `RunAsync` metody i zapisuje wynik w `ReliableDictionary`.

### <a name="actor-service"></a>Usługa Aktor
Po dodaniu nowych Aktor zaufanego programu Visual Studio dodaje do rozwiązania dwa projekty: projekt aktor i project interfejsu.

Projekt Aktor zawiera metody ustawiania i wprowadzenie wartości licznik, która jest prawidłowo zachowywane w Państwie aktora. Projekt interfejsu interfejs, który umożliwia wywołania Aktor innych usług.

### <a name="stateless-web-api"></a>Bezstanowa interfejsu API sieci Web
Bezstanowa projektu interfejs API sieci Web udostępnia usługę podstawowe sieci web, która umożliwia otwieranie aplikacji do klientów zewnętrznych. Aby uzyskać więcej informacji na temat projektu strukturalne, zobacz [usługi interfejs API usługi tkaninie sieci Web przy użyciu OWIN samodzielnej obsługi](service-fabric-reliable-services-communication-webapi.md).

### <a name="aspnet-core"></a>Podstawowe programu ASP.NET

SDK tkaninie usługi zawiera ten sam zestaw ASP.NET Core szablony, które są dostępne dla autonomicznego ASP.NET Core projektów: pusty [Interfejs API sieci Web][aspnet-webapi]i [Aplikacji sieci Web][aspnet-webapp].

## <a name="next-steps"></a>Następne kroki
### <a name="create-an-azure-cluster"></a>Tworzenie klastrze Azure
Usługa SDK tkaninie zawiera klaster lokalny dotyczące projektowania i testowania. Aby utworzyć klaster w Azure, zobacz [Konfigurowanie klastrze tkaninie usługi z portalu Azure][create-cluster-in-portal].

### <a name="try-deploying-to-azure-for-free-with-party-clusters"></a>Spróbuj wdrożenie Azure bezpłatnie z klastrów firmy

Jeśli chcesz wypróbować aplikacji wdrażanie i zarządzanie nimi w Azure bez konfigurowania własnych klastrów, możesz użyć bezpłatna [Usługa firmy klaster](http://aka.ms/tryservicefabric).

### <a name="publish-your-application-to-azure"></a>Publikowanie aplikacji Azure
Możesz opublikować aplikacji bezpośrednio z programu Visual Studio do klastrów Azure. Aby dowiedzieć się, jak to zrobić, zobacz [Publikowanie aplikacji Azure][publish-app-to-azure].

### <a name="use-service-fabric-explorer-to-visualize-your-cluster"></a>Wizualizowanie klaster za pomocą Eksploratora tkaninie usługi
Eksplorator tkaninie usługa oferuje łatwy sposób wizualizacji klaster, łącznie z wdrożonych aplikacji i fizyczny układ. Aby dowiedzieć się więcej, zobacz [wizualizacji klaster przy użyciu Eksploratora tkaninie usługi][visualize-with-sfx].

### <a name="version-and-upgrade-your-services"></a>Wersja i uaktualniania usługi
Usługa tkaninie umożliwia niezależne przechowywanie wersji i uaktualnianie niezależnych usług w aplikacji. Aby uzyskać więcej informacji, zobacz [Przechowywanie wersji i uaktualniania usługi][app-upgrade-tutorial].

### <a name="configure-continuous-integration-with-visual-studio-team-services"></a>Konfigurowanie ciągły integracji z programu Visual Studio Team Services
Aby dowiedzieć się, jak możesz skonfigurować proces ciągły integracji, tkaninie usługi aplikacji, zobacz [Konfigurowanie ciągły Integracja z programu Visual Studio Team Services][ci-with-vso].


<!-- Links -->
[add-web-frontend]: service-fabric-add-a-web-frontend.md
[create-cluster-in-portal]: service-fabric-cluster-creation-via-portal.md
[publish-app-to-azure]: service-fabric-publish-app-remote-cluster.md
[visualize-with-sfx]: service-fabric-visualizing-your-cluster.md
[ci-with-vso]: service-fabric-set-up-continuous-integration.md
[reliable-services-webapi]: service-fabric-reliable-services-communication-webapi.md
[app-upgrade-tutorial]: service-fabric-application-upgrade-tutorial.md
[aspnet-webapi]: https://docs.asp.net/en/latest/tutorials/first-web-api.html
[aspnet-webapp]: https://docs.asp.net/en/latest/tutorials/first-mvc-app/index.html
