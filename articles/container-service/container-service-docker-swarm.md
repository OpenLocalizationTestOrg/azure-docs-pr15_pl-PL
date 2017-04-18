<properties
   pageTitle="Azure kontenera usługi zarządzania kontenera z Docker Swarm | Microsoft Azure"
   description="Wdrażanie kontenerów Docker Swarm w usłudze Azure kontenera"
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

# <a name="container-management-with-docker-swarm"></a>Zarządzanie kontenera przy użyciu Docker Swarm

Rój docker udostępnia środowisko wdrażaniu konteneryzowanego obciążenia za pośrednictwem puli zestawu hostów Docker. Rój docker używa natywnych interfejsu API Docker. Przepływ pracy do zarządzania kontenerów na Swarm Docker jest niemal identyczne co będzie na hoście jeden kontener. Niniejszy dokument zawiera proste przykłady wdrażanie konteneryzowanego obciążenia w wystąpieniu usług kontenera Azure Docker Swarm. Więcej szczegółowo dokumentację dotyczącą Docker Swarm zobacz [Docker Swarm na Docker.com](https://docs.docker.com/swarm/).

Wymagania wstępne dotyczące ćwiczeń w tym dokumencie:

[Tworzenie klastrze rój w usłudze Azure kontenera](container-service-deployment.md)

[Nawiązywanie połączenia z klastrem rój w usłudze Azure kontenera](container-service-connect.md)

## <a name="deploy-a-new-container"></a>Wdrażanie nowego kontenera

Aby utworzyć nowy kontener w Docker Swarm, za pomocą `docker run` polecenie (dzięki czemu, które zostały otwarte tunelem SSH do wzorców zgodnie z powyższym wstępnych). W tym przykładzie tworzy kontenera z `yeasy/simple-web` obrazu:


```bash
user@ubuntu:~$ docker run -d -p 80:80 yeasy/simple-web

4298d397b9ab6f37e2d1978ef3c8c1537c938e98a8bf096ff00def2eab04bf72
```

Po utworzeniu kontenerze za pomocą `docker ps` do zwracania informacji o kontenerze. W tym miejscu znajdują się wymieniony agentem rój obsługujący kontenerze:


```bash
user@ubuntu:~$ docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                 NAMES
4298d397b9ab        yeasy/simple-web    "/bin/sh -c 'python i"   31 seconds ago      Up 9 seconds        10.0.0.5:80->80/tcp   swarm-agent-34A73819-1/happy_allen
```  

Teraz masz dostęp do aplikacji, która działa w tym kontenerze za pośrednictwem publiczna nazwa DNS równoważenia obciążenia agenta rój. Te informacje można znaleźć w portalu Azure:  


![Odwiedź rzeczywistą wyników](media/real-visit.jpg)  

Domyślnie równoważenia obciążenia ma porty 80, otwórz 8080 i 443. Jeśli chcesz połączyć na inny port, należy otworzyć ten port na równoważenia obciążenia Azure puli agenta.

## <a name="deploy-multiple-containers"></a>Wdrażanie wielu kontenerów

Jak wielu kontenerów są uruchamiane przez wykonywanie "Uruchom docker" wielokrotnie, można użyć `docker ps` polecenie, aby zobaczyć, która obsługuje kontenerów są uruchomione na. W poniższym przykładzie trzy kontenery są równomiernie przez trzy agentów rój:  


```bash
user@ubuntu:~$ docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                 NAMES
11be062ff602        yeasy/simple-web    "/bin/sh -c 'python i"   11 seconds ago      Up 10 seconds       10.0.0.6:83->80/tcp   swarm-agent-34A73819-2/clever_banach
1ff421554c50        yeasy/simple-web    "/bin/sh -c 'python i"   49 seconds ago      Up 48 seconds       10.0.0.4:82->80/tcp   swarm-agent-34A73819-0/stupefied_ride
4298d397b9ab        yeasy/simple-web    "/bin/sh -c 'python i"   2 minutes ago       Up 2 minutes        10.0.0.5:80->80/tcp   swarm-agent-34A73819-1/happy_allen
```  

## <a name="deploy-containers-by-using-docker-compose"></a>Wdrażanie kontenerów przy użyciu Docker redagowanie

Zredaguj Docker służy do automatycznego wdrożenia i konfiguracji wielu kontenerów. Aby to zrobić, upewnij się, utworzono tunelem Secure Shell (SSH) i że zmienna DOCKER_HOST została ustawiona (zobacz wymagania wstępne powyżej).

Tworzenie pliku docker compose.yml na komputerze lokalnym. W tym celu należy użyć [przykładowych](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/docker-compose.yml).

```bash
web:
  image: adtd/web:0.1
  ports:
    - "80:80"
  links:
    - rest:rest-demo-azure.marathon.mesos
rest:
  image: adtd/rest:0.1
  ports:
    - "8080:8080"

```

Uruchamianie `docker-compose up -d` zacząć wdrożeń kontenera:


```bash
user@ubuntu:~/compose$ docker-compose up -d
Pulling rest (adtd/rest:0.1)...
swarm-agent-3B7093B8-0: Pulling adtd/rest:0.1... : downloaded
swarm-agent-3B7093B8-2: Pulling adtd/rest:0.1... : downloaded
swarm-agent-3B7093B8-3: Pulling adtd/rest:0.1... : downloaded
Creating compose_rest_1
Pulling web (adtd/web:0.1)...
swarm-agent-3B7093B8-3: Pulling adtd/web:0.1... : downloaded
swarm-agent-3B7093B8-0: Pulling adtd/web:0.1... : downloaded
swarm-agent-3B7093B8-2: Pulling adtd/web:0.1... : downloaded
Creating compose_web_1
```

Na koniec listy uruchomienia kontenerów zostaną zwrócone. Na wyświetlonej liście wymienione kontenery, wdrożone przy użyciu Docker Redagowanie:


```bash
user@ubuntu:~/compose$ docker ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS                     NAMES
caf185d221b7        adtd/web:0.1        "apache2-foreground"   2 minutes ago       Up About a minute   10.0.0.4:80->80/tcp       swarm-agent-3B7093B8-0/compose_web_1
040efc0ea937        adtd/rest:0.1       "catalina.sh run"      3 minutes ago       Up 2 minutes        10.0.0.4:8080->8080/tcp   swarm-agent-3B7093B8-0/compose_rest_1
```

Oczywiście można użyć `docker-compose ps` przyjrzenie się kontenerów zdefiniowane w swojej `compose.yml` pliku.

## <a name="next-steps"></a>Następne kroki

[Dowiedz się więcej o Docker Swarm](https://docs.docker.com/swarm/)
