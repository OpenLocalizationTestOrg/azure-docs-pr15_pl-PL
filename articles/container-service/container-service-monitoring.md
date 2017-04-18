<properties
   pageTitle="Monitorowanie klaster usługa Azure kontenera z Datadog | Microsoft Azure"
   description="Monitorowanie klaster usługa Azure kontenera z Datadog. Korzystanie z sieci web kontrolera domeny i OS interfejs użytkownika wdrożenia czynników Datadog klaster."
   services="container-service"
   documentationCenter=""
   authors="rbitia"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Kontenery, kontrolera domeny i OS rój Docker Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure"   
   ms.date="07/28/2016"
   ms.author="t-ribhat"/>

# <a name="monitor-an-azure-container-service-cluster-with-datadog"></a>Monitorowanie klaster usługa Azure kontenera z Datadog

W tym artykule firma Microsoft będzie wdrożyć czynników Datadog wszystkie węzły agenta w klastrze Usługa Azure kontener. Konieczne będzie konto z Datadog dla tej konfiguracji. 

## <a name="prerequisites"></a>Wymagania wstępne 

[Rozmieszczanie](container-service-deployment.md) i [Łączenie](container-service-connect.md) klastrze skonfigurowany przez usługę kontenera Azure. Eksplorowanie [Marathon interfejsu użytkownika](container-service-mesos-marathon-ui.md). Przejdź do [http://datadoghq.com](http://datadoghq.com) skonfigurować konto Datadog. 

## <a name="datadog"></a>Datadog 

Datadog jest Usługa monitorowania, które gromadzi dane monitorowania od kontenerów w klastrze Usługa Azure kontener. Datadog ma pulpitu nawigacyjnego Docker integracji możesz wyświetlać określonych miar w obrębie kontenerów. Metryki zdobyte kontenerów są zorganizowane według Procesora i pamięci, sieci We/Wy. Datadog dzieli metryki na kontenerów i obrazów. Przykładowy wygląd interfejsu użytkownika dla użycie Procesora jest poniżej.

![Interfejs użytkownika Datadog](./media/container-service-monitoring/datadog4.png)

## <a name="configure-a-datadog-deployment-with-marathon"></a>Konfigurowanie rozmieszczania Datadog z Marathon

Te kroki procedurach pokazano, jak do konfigurowania i wdrażania aplikacji Datadog klaster z Marathon. 

Uzyskiwanie dostępu za pośrednictwem interfejsu użytkownika kontrolera domeny i OS [http://localhost:80-](http://localhost:80/). Raz w Interfejsie użytkownika kontrolera domeny i OS przejdź do "biznesowej", który znajduje się w lewym dolnym rogu okna i wyszukaj "Datadog" i kliknij przycisk "Zainstaluj".

![Datadog pakietu w biznesowej kontrolera domeny/systemu operacyjnego](./media/container-service-monitoring/datadog1.png)

Teraz, aby zakończyć konfigurację należy konto Datadog lub bezpłatne konto wersji próbnej. Gdy jesteś zalogowany do Datadog wyglądu witryny sieci Web z lewej strony i przejdź do integracji -> następnie interfejsu API. 

![Klucz interfejsu API Datadog](./media/container-service-monitoring/datadog2.png)

Następnie wprowadź klucz interfejsu API do konfiguracji Datadog w biznesowej kontrolera domeny/systemu operacyjnego. 

![Konfiguracja Datadog na całym świecie kontrolera domeny/systemu operacyjnego](./media/container-service-monitoring/datadog3.png) 

W powyższym konfiguracji wystąpienia są ustawione na wartość 10000000, gdy nowy węzeł jest dodawana do klaster Datadog zostanie automatycznie wdrażanie agenta do tego węzła. Jest tymczasowy rozwiązanie. Po zainstalowaniu pakietu, należy przejść z powrotem do witryny sieci Web Datadog i znajdowanie "Pulpity nawigacyjne." Stamtąd zostanie wyświetlony niestandardowy i integracji pulpitów nawigacyjnych. Pulpit nawigacyjny integracji Docker będzie miał wszystkie niezbędne do monitorowania klaster metryki kontener. 
