<properties
   pageTitle="Monitorowanie klaster usługa Azure kontenera z Sysdig | Microsoft Azure"
   description="Monitorowanie klaster usługa Azure kontenera z Sysdig."
   services="container-service"
   documentationCenter=""
   authors="rbitia"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Kontenery, kontrolera domeny i OS Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/08/2016"
   ms.author="t-ribhat"/>

# <a name="monitor-an-azure-container-service-cluster-with-sysdig"></a>Monitorowanie klaster usługa Azure kontenera z Sysdig

W tym artykule firma Microsoft będzie wdrożyć czynników Sysdig wszystkie węzły agenta w klastrze Usługa Azure kontener. Potrzebne jest konto u Sysdig dla tej konfiguracji. 

## <a name="prerequisites"></a>Wymagania wstępne 

[Rozmieszczanie](container-service-deployment.md) i [Łączenie](container-service-connect.md) klastrze skonfigurowany przez usługę kontenera Azure. Eksplorowanie [Marathon interfejsu użytkownika](container-service-mesos-marathon-ui.md). Przejdź do [http://app.sysdigcloud.com](http://app.sysdigcloud.com) skonfigurować konto chmury Sysdig. 

## <a name="sysdig"></a>Sysdig

Sysdig to monitorowania usługa, która umożliwia monitorowanie kontenerów w klastrze. Wiadomo, że Sysdig pomoc w rozwiązywaniu problemów, ale również ma usługi podstawowe metryki monitorowania dla Procesora, sieci, pamięci i we/wy. Sysdig można łatwo zobaczyć kontenery, które pracują hardest lub zasadniczo przy użyciu najbardziej pamięci i Procesora. Ten widok znajduje się w sekcji "Przegląd", który jest obecnie w wersji beta. 

![Interfejs użytkownika Sysdig](./media/container-service-monitoring-sysdig/sysdig6.png) 

## <a name="configure-a-sysdig-deployment-with-marathon"></a>Konfigurowanie rozmieszczania Sysdig z Marathon

Te kroki procedurach pokazano, jak do konfigurowania i wdrażania aplikacji Sysdig klaster z Marathon. 

Uzyskiwanie dostępu za pośrednictwem interfejsu użytkownika kontrolera domeny i OS [http://localhost:80-](http://localhost:80/) raz w Interfejsie użytkownika kontrolera domeny i OS przejdź do "Biznesowej", który znajduje się w lewym dolnym rogu okna, a następnie wyszukaj "Sysdig."

![Sysdig w biznesowej kontrolera domeny/systemu operacyjnego](./media/container-service-monitoring-sysdig/sysdig1.png)

Teraz konieczne do wykonania w konfiguracji konta chmury Sysdig lub bezpłatne konto wersji próbnej. Po zalogowaniu chmurze Sysdig witryny sieci Web, kliknij swoją nazwę użytkownika, a na stronie powinna być widoczna "Dostęp klucza." 

![Klucz interfejsu API Sysdig](./media/container-service-monitoring-sysdig/sysdig2.png) 

Następnie wprowadź klucz dostępu do konfiguracji Sysdig w biznesowej kontrolera domeny i OS. 

![Konfiguracja Sysdig na całym świecie kontrolera domeny/systemu operacyjnego](./media/container-service-monitoring-sysdig/sysdig3.png)

Zestaw teraz wystąpienia do 10000000 dlatego zawsze, gdy nowy węzeł zostanie dodany do klastrów Sysdig zostanie automatycznie wdrożyć agenta do tego nowego węzła. Jest tymczasowy rozwiązanie, aby upewnić się, że Sysdig zostanie wdrożony do wszystkich nowych czynników w klastrze. 

![Konfiguracja Sysdig w kontrolera domeny i OS biznesowej — wystąpienia](./media/container-service-monitoring-sysdig/sysdig4.png)

Po zainstalowaniu pakietu przejść z powrotem do Sysdig interfejsu użytkownika i będzie można eksplorować metryki zastosowania różnych dla kontenerów w klastrze. 