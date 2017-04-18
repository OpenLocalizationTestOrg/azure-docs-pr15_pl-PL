<properties
   pageTitle="Azure Zarządzanie kontenera kontenera usługi za pośrednictwem interfejsu API usługi REST | Microsoft Azure"
   description="Wdrażanie kontenerów z klastrem Azure kontenera usługi Mesos za pomocą interfejsu API usługi REST Marathon."
   services="container-service"
   documentationCenter=""
   authors="neilpeterson"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, kontenery, Micro usług, Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/13/2016"
   ms.author="timlt"/>

# <a name="container-management-through-the-rest-api"></a>Zarządzanie kontenera za pośrednictwem interfejsu API usługi REST

Kontrolera domeny i OS zapewnia środowisko umożliwiające wdrażanie i skalowanie grupowany obciążenia podczas dostępnego używanego sprzętu. U góry kontrolera domeny/systemu operacyjnego ma struktury, która zarządza planowania i wykonywania obliczeń obciążenia.

Ramy są dostępne dla wielu popularnych obciążenie pracą, w tym dokumencie opisano, jak tworzyć i skalowanie wdrożeń kontenera przy użyciu Marathon. Przed pracy tych przykładów, należy klaster kontrolera domeny/systemu operacyjnego, który jest skonfigurowany w usłudze Azure w kontenerze. Można również muszą być połączenia zdalnego z tym klastrem. Aby uzyskać więcej informacji o tych elementów zobacz następujące artykuły:

- [Wdrażanie klastrze Usługa Azure kontenera](container-service-deployment.md)
- [Nawiązywanie połączenia z klastrem usługa Azure kontenera](container-service-connect.md)

Po połączeniu z klastrem usługa Azure kontenera, masz dostęp do powiązanych API pozostałych i kontrolera domeny i OS w http://localhost:local port. W przykładach w tym dokumencie przyjęto założenie, że są tunneling na porcie 80. Na przykład punkt końcowy Marathon można się z Tobą skontaktować `http://localhost/marathon/v2/`. Aby uzyskać więcej informacji na różne elementy interfejsu API Zobacz dokumentacji mezosfery [Marathon interfejsu API](https://mesosphere.github.io/marathon/docs/rest-api.html) oraz interfejsu [Chronos API](https://mesos.github.io/chronos/docs/api.html)i [Interfejsu API harmonogramu Mesos](http://mesos.apache.org/documentation/latest/scheduler-http-api/)można znaleźć w dokumentacji Apache.

## <a name="gather-information-from-dcos-and-marathon"></a>Gromadzenie informacji z kontrolera domeny/systemu operacyjnego i Marathon

Przed wdrożeniem kontenerów klaster kontrolera domeny i OS gromadzenie pewne informacje na temat klaster kontrolera domeny/systemu operacyjnego, takie jak nazwy i bieżący stan czynników kontrolera domeny/systemu operacyjnego. W tym celu kwerendy `master/slaves` końcowy interfejsu API usługi REST kontrolera domeny/systemu operacyjnego. Jeśli wszystko odbywa się również, będzie zostanie wyświetlona lista czynników kontrolera domeny i OS oraz niektóre właściwości dla każdej z nich.

```bash
curl http://localhost/mesos/master/slaves
```

Teraz, za pomocą Marathon `/apps` punktu końcowego, aby sprawdzić, czy bieżący wdrażaniem aplikacji do klastrów kontrolera domeny/systemu operacyjnego. Jeśli jest to nowy klaster zostanie wyświetlona pusta tablica dla aplikacji.

```
curl localhost/marathon/v2/apps

{"apps":[]}
```

## <a name="deploy-a-docker-formatted-container"></a>Wdrażanie kontenera sformatowanych przy użyciu Docker

Sformatowany Docker kontenerów za pośrednictwem Marathon należy wdrożyć za pomocą pliku JSON opisującą zamierzonego wdrożenia. Poniższy przykład zostanie wdrożony kontenerze Nginx powiązanie portu 80 agenta kontrolera domeny i OS do portu 80 kontenerze. Należy również zauważyć, że właściwość "acceptedResourceRoles" jest ustawiona na "slave_public". Spowoduje to wdrożyć kontenerze agenta w zestawie skali agenta publicznej.

```json
{
  "id": "nginx",
  "cpus": 0.1,
  "mem": 16.0,
  "instances": 1,
    "acceptedResourceRoles": [
    "slave_public"
  ],
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        { "containerPort": 80, "hostPort": 80, "servicePort": 9000, "protocol": "tcp" }
      ]
    }
  }
}
```

Aby wdrożyć sformatowane Docker kontenera, Utwórz pliku JSON, albo użyj próbki w [usłudze Azure kontenera pokaz](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/marathon/marathon.json). Zapisz go w dostępnym miejscu. Następny Aby wdrożyć kontenera, uruchom następujące polecenie. Określ nazwę pliku JSON.

```
curl -X POST http://localhost/marathon/v2/apps -d @marathon.json -H "Content-type: application/json"
```

Wynik będzie podobny do następującego:

```json
{"version":"2015-11-20T18:59:00.494Z","deploymentId":"b12f8a73-f56a-4eb1-9375-4ac026d6cdec"}
```

Teraz Jeśli kwerenda Marathon dla aplikacji, tej nowej aplikacji będzie wyświetlana w wynikach.

```
curl localhost/marathon/v2/apps
```

## <a name="scale-your-containers"></a>Skalowanie kontenerów

Umożliwia także interfejsu API Marathon skalowania lub przeskalować we wdrożeniach aplikacji. W poprzednim przykładzie wdrożono jedno wystąpienie aplikacji. Załóżmy skalowanie ten się trzy wystąpienia aplikacji. W tym celu należy utworzyć plik JSON przy użyciu następujący tekst JSON i zapisać go w dostępnym miejscu.

```json
{ "instances": 3 }
```

Uruchom następujące polecenie, aby skalowania aplikacji.

>[AZURE.NOTE] Identyfikator URI będzie http://localhost/marathon/v2/apps/, a następnie identyfikator aplikacji przeskalować. Jeśli korzystasz z próbki Nginx, który znajduje się w tym miejscu, identyfikator URI będzie http://localhost/marathon/v2/apps/nginx.

```json
curl http://localhost/marathon/v2/apps/nginx -H "Content-type: application/json" -X PUT -d @scale.json
```

Na koniec kwerendy punkt końcowy Marathon dla aplikacji. Zobaczysz, że teraz są trzy kontenery Nginx.

```
curl localhost/marathon/v2/apps
```

## <a name="use-powershell-for-this-exercise-marathon-rest-api-interaction-with-powershell"></a>W tym celu za pomocą programu PowerShell: interfejsu API usługi REST Marathon interakcji z programem PowerShell

Te same czynności można wykonywać przy użyciu polecenia programu PowerShell na komputerze z systemem Windows.

Uzyskanie informacji o klastrze kontrolera domeny/systemu operacyjnego, takie jak nazwy agenta i stan agenta, uruchom następujące polecenie.

```powershell
Invoke-WebRequest -Uri http://localhost/mesos/master/slaves
```

Sformatowany Docker kontenerów za pośrednictwem Marathon należy wdrożyć za pomocą pliku JSON opisującą zamierzonego wdrożenia. Poniższy przykład zostanie wdrożony kontenerze Nginx powiązanie portu 80 agenta kontrolera domeny i OS do portu 80 kontenerze.

```json
{
  "id": "nginx",
  "cpus": 0.1,
  "mem": 16.0,
  "instances": 1,
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        { "containerPort": 80, "hostPort": 80, "servicePort": 9000, "protocol": "tcp" }
      ]
    }
  }
}
```

Utwórz plik JSON, albo użyj próbki w [usłudze Azure kontenera pokaz](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/marathon/marathon.json). Zapisz go w dostępnym miejscu. Następny Aby wdrożyć kontenera, uruchom następujące polecenie. Określ nazwę pliku JSON.

```powershell
Invoke-WebRequest -Method Post -Uri http://localhost/marathon/v2/apps -ContentType application/json -InFile 'c:\marathon.json'
```

Umożliwia także interfejsu API Marathon skalowania lub przeskalować we wdrożeniach aplikacji. W poprzednim przykładzie wdrożono jedno wystąpienie aplikacji. Załóżmy skalowanie ten się trzy wystąpienia aplikacji. W tym celu należy utworzyć plik JSON przy użyciu następujący tekst JSON i zapisać go w dostępnym miejscu.

```json
{ "instances": 3 }
```

Uruchom następujące polecenie, aby skalowania aplikacji.

> [AZURE.NOTE] Identyfikator URI będzie http://localhost/marathon/v2/apps/, a następnie identyfikator aplikacji przeskalować. Jeśli korzystasz z próbki Nginx opisane poniżej, identyfikator URI będzie http://localhost/marathon/v2/apps/nginx.

```powershell
Invoke-WebRequest -Method Put -Uri http://localhost/marathon/v2/apps/nginx -ContentType application/json -InFile 'c:\scale.json'
```

## <a name="next-steps"></a>Następne kroki

- [Dowiedz się więcej o Mesos HTTP punktów końcowych]( http://mesos.apache.org/documentation/latest/endpoints/).
- [Dowiedz się więcej o interfejsu API usługi REST Marathon]( https://mesosphere.github.io/marathon/docs/rest-api.html).
