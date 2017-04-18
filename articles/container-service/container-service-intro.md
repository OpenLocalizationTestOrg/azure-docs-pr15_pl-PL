<properties
   pageTitle="Wprowadzenie usługi Azure kontenera | Microsoft Azure"
   description="Usługa Azure kontenera umożliwia uprościć tworzenie, konfiguracji i zarządzanie klastrze maszyn wirtualnych, wstępnie skonfigurowanych do uruchamiania aplikacji konteneryzowanego."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, kontenery, Micro usług, Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/13/2016"
   ms.author="rogardle"/>

# <a name="azure-container-service-introduction"></a>Wprowadzenie kontenera usługi Azure

Usługa Azure kontenera powoduje, że prostsze umożliwiają tworzenie i konfigurowanie oraz zarządzanie klastrze maszyn wirtualnych, wstępnie skonfigurowanych do uruchamiania aplikacji konteneryzowanego. Używa zoptymalizowane konfiguracji popularne narzędzia planowania i aranżacji Otwórz źródło. Pozwala na korzystanie z posiadanych umiejętności i rysowanie na duże i rosnącej treść specjalizacji społeczności, wdrażania i zarządzać nimi, kontener aplikacjach Microsoft Azure.


![Usługa Azure kontenera umożliwia zarządzanie aplikacjami konteneryzowanego na wiele hostów Azure.](./media/acs-intro/acs-cluster.png)


Usługa Azure kontenera wykorzystuje format kontenera Docker, aby upewnić się, że kontenerów aplikacji są w pełni przenośne. Obsługuje także wybór Marathon i kontrolera domeny i OS lub Docker Swarm, dlatego można skalować te aplikacje do tysięcy kontenerów lub nawet dziesiątki tysięcy.

Za pomocą usługi kontenera Azure, możesz korzystać z funkcji klasy korporacyjnej Azure, zachowując przenoszenia aplikacji — w tym przenoszenia na warstwach aranżacji.

<a name="using-azure-container-service"></a>Za pomocą usługi Azure kontenera
-----------------------------

Naszej sieci z usługą kontenera Azure jest zapewnienie kontenera środowisko macierzyste przy użyciu narzędzia Otwórz źródło i technologii, które są popularne wśród klientów dzisiaj. W tym celu możemy uwidaczniają standardowe punkty końcowe interfejsu API usługi wybranym orchestrator (kontrolera domeny i OS lub Docker Swarm). Za pomocą tych punkty końcowe, mogą korzystać z oprogramowania, który jest w stanie rozmawiać, aby te punkty końcowe. Na przykład w przypadku Docker Swarm końcowy, możesz wybrać za pomocą interfejsu wiersza polecenia Docker (polecenie). Kontrolera domeny/systemu operacyjnego można użyć polecenie DCOS.

<a name="creating-a-docker-cluster-by-using-azure-container-service"></a>Tworzenie klastrze Docker za pomocą usługi kontenera Azure
-------------------------------------------------------

Aby rozpocząć używanie usługa Azure kontenera, należy wdrożyć klastrze Usługa kontenera Azure za pośrednictwem portalu (wyszukiwanie "Usługa Azure kontenera"), za pomocą szablonu Menedżera zasobów Azure ([Docker Swarm](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm) lub [Kontrolera domeny i OS](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos)) lub przy użyciu [interfejsu wiersza polecenia](/documentation/articles/xplat-cli-install/). Szablony dostarczonych Szybki Start można zmodyfikować w taki sposób, aby uwzględnić dodatkowego lub zaawansowanego Azure konfiguracji. Aby uzyskać więcej informacji o wdrażaniu klastrze Usługa Azure kontenera zobacz [Rozmieszczanie klastrze Usługa Azure kontener](container-service-deployment.md).

<a name="deploying-an-application"></a>Wdrażanie aplikacji
------------------------

Usługa Azure kontenera umożliwia wybranie Docker Swarm lub kontrolera domeny i OS dla aranżacji. Jak wdrożyć aplikację w zależności od wybranej funkcji orchestrator.

### <a name="using-dcos"></a>Za pomocą kontrolera domeny/systemu operacyjnego

Kontrolera domeny/systemu operacyjnego jest rozproszony system operacyjny, oparty na jądrze systemów dystrybucji Apache Mesos. Apache Mesos są przechowywane w Apache Software Foundation i przedstawiono [największych nazw w IT](http://mesos.apache.org/documentation/latest/powered-by-mesos/) jako użytkowników i współautorów.

![Usługa Azure kontenera skonfigurowane dla rój czynników i wzorce.](media/acs-intro/dcos.png)

Kontrolera domeny/systemu operacyjnego i Apache Mesos to zestaw funkcji atrakcyjnych:

-   Sprawdzona skalowalność

-   Odporność na uszkodzenia replikowane wzorca i slaves przy użyciu Apache ZooKeeper

-   Obsługa sformatowane Docker kontenerów

-   Natywne izolacji między zadaniami z kontenerami Linux

-   Multiresource planowania (pamięci, Procesora, dysku i porty)

-   Java, Python oraz interfejsy API C++ do tworzenia nowej aplikacji równoległego

-   Interfejs użytkownika sieci web do wyświetlania stanu klaster

Domyślnie kontrolera domeny i OS uruchomionych usługa Azure kontener zawiera platformy aranżacji Marathon planowania obciążenia. Jest dostępny w pakiecie wdrażania kontrolera domeny i OS ACS jednak całość mezosfery usług, które można dodać do tej usługi należą Spark, Hadoop, Cassandra i wiele innych.

![Kontrolera domeny i OS biznesowej w usłudze Azure kontenera](media/dcos/universe.png)

#### <a name="using-marathon"></a>Przy użyciu Marathon

Marathon jest inicjowania całym i sterujących dla usług w cgroups — lub w przypadku usługi kontenera Azure, sformatowane Docker kontenerów. Marathon udostępnia interfejs sieci web, z której można wdrażać aplikacji. Możesz dostępna pod adresem URL, który może wyglądać `http://DNS_PREFIX.REGION.cloudapp.azure.com` miejsce, w którym DNS\_PREFIKS i regionu są definiowane w czasie rozmieszczania. Oczywiście możesz także podać nazwę DNS. Aby uzyskać więcej informacji na temat uruchamiania kontenera korzystanie z sieci web Marathon interfejsu użytkownika zobacz [Zarządzanie kontenera za pośrednictwem interfejsu użytkownika w sieci web](container-service-mesos-marathon-ui.md).

![Listy aplikacji Marathon](media/dcos/marathon-applications-list.png)

Za pomocą interfejsów API pozostałych do komunikowania się z Marathon. Istnieje wiele bibliotek klienta, które są dostępne dla poszczególnych narzędzi. Obejmują różnych językach — i oczywiście, można użyć protokołu HTTP w innym języku. Ponadto wielu popularnych narzędzi DevOps zapewniają obsługę Marathon. Zapewnia to maksymalną elastyczność dla zespołu operacje podczas pracy z klastrem usługa Azure kontener. Aby uzyskać więcej informacji na temat uruchamiania kontenera przy użyciu interfejsu API usługi REST Marathon zobacz [Zarządzanie kontenera przy użyciu interfejsu API usługi REST](container-service-mesos-marathon-rest.md).

### <a name="using-docker-swarm"></a>Za pomocą rój Docker

Rój docker udostępnia natywnego klaster dla Docker. Ponieważ Docker Swarm obsługuje standard interfejsu API Docker, wszelkie narzędzia, które już komunikuje demon Docker za pomocą rój przezroczysty skalowanie do wielu hostów w usłudze Azure kontener.

![Usługa Azure kontener jest skonfigurowany do używania kontrolera domeny i OS — jumpbox, czynników i wzorców.](media/acs-intro/acs-swarm2.png)

Obsługiwane narzędzia do zarządzania kontenerów w klastrze rój zawiera, ale nie są ograniczone do następujących czynności:

-   Dokku

-   Zredaguj docker polecenie i Docker

-   Krane

-   Jenkins

<a name="videos"></a>Klipy wideo
------

Wprowadzenie do usługi kontenera Azure (101):  

> [AZURE.VIDEO azure-container-service-101]

Budowanie aplikacji przy użyciu usługi Azure kontenera (kompilacja 2016)

> [AZURE.VIDEO build-2016-building-applications-using-the-azure-container-service]
