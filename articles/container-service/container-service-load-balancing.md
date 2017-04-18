<properties
   pageTitle="Ładowanie kontenerów saldo w klastrze Usługa Azure kontenera | Microsoft Azure"
   description="Równoważenia obciążenia z wykorzystaniem wielu kontenerów w klastrze Usługa Azure kontener."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Kontenery, Micro usług kontrolera domeny i OS Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/11/2016"
   ms.author="rogardle"/>

# <a name="load-balance-containers-in-an-azure-container-service-cluster"></a>Ładowanie kontenerów saldo w klastrze Usługa Azure kontenera

W tym artykule omówimy sposobu tworzenia równoważenia obciążenia wewnętrznych w kontrolera domeny i OS zarządzane usługi kontenera Azure za pomocą Marathon kg. Pozwoli na poziomie skalowanie aplikacji. Również zostanie włączyć możesz skorzystać z agentem publicznych i prywatnych klastrów przez umieszczenie swojego urządzenia do równoważenia obciążenia w grupie publicznej i kontenerów aplikacji w klastrze prywatne.

## <a name="prerequisites"></a>Wymagania wstępne

[Rozmieszczanie wystąpienie usługi kontenera Azure](container-service-deployment.md) za pomocą orchestrator typ kontrolera domeny-OS i [Upewnij się, że klient może połączyć się klaster](container-service-connect.md). 

## <a name="load-balancing"></a>Równoważenia obciążenia

Istnieją dwie warstwy równoważenia obciążenia w klastrze kontenera usługi, które będzie budowy: 

  1. Azure równoważenia obciążenia zawiera punktów wejścia publicznej (te, których użytkownicy końcowi będą trafień). To jest dostarczany automatycznie przez usługę kontenera Azure i jest domyślnie skonfigurowany do udostępniania port 80 i 443 8080.
  2. Równoważenia obciążenia Marathon (marathon kg) kieruje żądań przychodzących do wystąpień kontenera, które te żądania obsługi. Firma Microsoft skalowania kontenerów świadczenie naszych usług sieci web, marathon kg dynamicznie dostosowuje. Domyślnie w usłudze kontenera nie określono tej usługi równoważenia obciążenia, ale jest bardzo łatwa.

## <a name="marathon-load-balancer"></a>Usługi równoważenia obciążenia Marathon

Usługi równoważenia obciążenia Marathon dynamicznie ponownie konfiguruje oparciu o kontenery, które zostały wdrożone. Należy również mechanizm utratę kontenera lub agenta — jeśli dzieje się tak, Apache Mesos po prostu zacznie kontenera w innym miejscu i dostosowania będą marathon kg.

Aby zainstalować równoważenia obciążenia Marathon, możesz użyć dowolnej kontrolera domeny i system operacyjny web interfejsu użytkownika lub wiersza polecenia.

### <a name="install-marathon-lb-using-dcos-web-ui"></a>Instalowanie kg Marathon przy użyciu kontrolera domeny i OS interfejs sieci Web

  1. Kliknij pozycję "Universe"
  2. Wyszukiwanie "Marathon kg"
  3. Kliknij przycisk "Zainstaluj"

![Instalowanie kg marathon za pośrednictwem interfejsu sieci Web kontrolera domeny/systemu operacyjnego](./media/dcos/marathon-lb-install.png)

### <a name="install-marathon-lb-using-the-dcos-cli"></a>Instalowanie kg Marathon, za pomocą interfejsu wiersza polecenia kontrolera domeny/systemu operacyjnego

Po zainstalowaniu polecenie kontrolera domeny/systemu operacyjnego i zapewnienie można nawiązać klaster, uruchom następujące polecenie z komputera klienckiego:

```bash
dcos package install marathon-lb
```

To polecenie automatycznie instaluje równoważenia obciążenia w klastrze publicznej czynników.

## <a name="deploy-a-load-balanced-web-application"></a>Wdrażanie obciążeniem strategie aplikacji sieci Web

Teraz gdy mamy już pakietu marathon kg, możemy zainstalować aplikację kontenera, który Życzymy do równoważenia obciążenia. W tym przykładzie firma Microsoft będzie wdrożyć serwer sieci web prosty przy użyciu następującej konfiguracji:

```json
{
  "id": "web",
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "yeasy/simple-web",
      "network": "BRIDGE",
      "portMappings": [
        { "hostPort": 0, "containerPort": 80, "servicePort": 10000 }
      ],
      "forcePullImage":true
    }
  },
  "instances": 3,
  "cpus": 0.1,
  "mem": 65,
  "healthChecks": [{
      "protocol": "HTTP",
      "path": "/",
      "portIndex": 0,
      "timeoutSeconds": 10,
      "gracePeriodSeconds": 10,
      "intervalSeconds": 2,
      "maxConsecutiveFailures": 10
  }],
  "labels":{
    "HAPROXY_GROUP":"external",
    "HAPROXY_0_VHOST":"YOUR FQDN",
    "HAPROXY_0_MODE":"http"
  }
}

```

  * Ustaw wartość `HAProxy_0_VHOST` pełni kwalifikowaną nazwę domeny usługi równoważenia obciążenia usługi agentów. Jest to w formularzu `<acsName>agents.<region>.cloudapp.azure.com`. Na przykład utworzyć klaster usługi kontenera o nazwie `myacs` w regionie `West US`, będzie Kwalifikowaną `myacsagents.westus.cloudapp.azure.com`. Można to również znaleźć, szukając równoważenia obciążenia, z "agent" w nazwie podczas wyszukiwania przy użyciu zasobów w grupie zasobów utworzonym usługi kontenera w [Azure portal](https://portal.azure.com).
  * Ustawianie portu servicePort > = 10 000. Identyfikuje usługa, która jest uruchamiany w tym kontenerze — marathon kg używa to w celu zidentyfikowania usług, które należy go saldo przez.
  * Ustawianie `HAPROXY_GROUP` etykiety do "zewnętrzne".
  * Ustawianie `hostPort` 0. Oznacza to, że Marathon dowolnie będą przydzielane dostępny port.
  * Ustawianie `instances` liczby wystąpień chcesz utworzyć. Można zawsze skalować te i rozwijaniu później.

Warto noing domyślnie Marathon zostanie wdrożony do klastrów prywatne, oznacza to, że powyżej wdrożenia będzie dostępny tylko za pośrednictwem usługi równoważenia obciążenia, który jest zwykle zachowanie, które możemy najszybciej.

### <a name="deploy-using-the-dcos-web-ui"></a>Wdrażanie przy użyciu kontrolera domeny i OS interfejs sieci Web

  1. Odwiedź stronę Marathon w http://localhost/marathon (po konfigurowania [tunelem SSH](container-service-connect.md) i kliknij pozycję`Create Appliction`
  2. W `New Application` kliknij okno dialogowe `JSON Mode` w prawym górnym rogu
  3. Wklej powyżej JSON do edytora
  4. Kliknij pozycję`Create Appliction`

### <a name="deploy-using-the-dcos-cli"></a>Wdrażanie przy użyciu interfejsu wiersza polecenia kontrolera domeny/systemu operacyjnego

Wdrożenie tej aplikacji przy użyciu interfejsu wiersza polecenia kontrolera domeny i OS skopiować JSON powyżej w pliku o nazwie `hello-web.json`i uruchom:

```bash
dcos marathon app add hello-web.json
```

## <a name="azure-load-balancer"></a>Usługi równoważenia obciążenia Azure

Domyślnie równoważenia obciążenia Azure udostępnia porty 80, 8080 i 443. Jeśli korzystasz z jednej z tych trzech porty (tak jak możemy w powyższym przykładzie), a następnie nie ma żadnych elementów, które należy wykonać. Powinno być możliwe trafienie FQDN równoważenia obciążenia agenta — i zawsze, gdy odświeżanie, użytkownik będzie naciśnij jeden z trzech web serwerów w sposób okrężny. Jeśli używasz innego portu, potrzebny dodać regułę okrężny i sondy na równoważenia obciążenia portu, który został użyty. Można to zrobić za pomocą [Interfejsu wiersza polecenia Azure](../xplat-cli-azure-resource-manager.md), z poleceniami `azure network lb rule create` i `azure network lb probe create`. Możesz też to zrobić przy użyciu Azure Portal.


## <a name="additional-scenarios"></a>Dodatkowe scenariusze

Może mieć scenariusza, której używasz dla różnych domenach udostępniania różnych usługach. Na przykład:

mydomain1.com -> Azure LB:80 -> marathon-lb:10001 -> mycontainer1:33292  
mydomain2.com -> Azure LB:80 -> marathon-lb:10002 -> mycontainer2:22321

Aby osiągnąć ten cel, zapoznaj się z [wirtualną hosts](https://mesosphere.com/blog/2015/12/04/dcos-marathon-lb/), które umożliwiają kojarzenie domen do określonych kg marathon ścieżki.

Także możesz udostępnić innym porty i ponownie zamapować je do usługi poprawnego za marathon kg. Na przykład:

Azure lb:80 -> marathon-lb:10001 -> mycontainer:233423  
Azure lb:8080 -> marathon-lb:1002 -> mycontainer2:33432


## <a name="next-steps"></a>Następne kroki

Zobacz więcej na [kg marathon](https://dcos.io/docs/1.7/usage/service-discovery/marathon-lb/)można znaleźć w dokumentacji kontrolera domeny i OS.
