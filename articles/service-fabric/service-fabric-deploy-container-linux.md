<properties
   pageTitle="Usługi tkaninie i wdrażanie kontenerów w Linux | Microsoft Azure"
   description="Usługi i wykorzystanie kontenerów Docker rozmieszczanie aplikacji microservice. W tym artykule opisano możliwości, jakie zapewnia tkaninie usługi dla kontenerów i jak wdrożyć obraz kontener Docker w klaster"
   services="service-fabric"
   documentationCenter=".net"
   authors="msfussell"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/24/2016"
   ms.author="msfussell"/>

# <a name="deploy-a-docker-container-to-service-fabric"></a>Wdrażanie kontenera Docker tkaninie usługi

> [AZURE.SELECTOR]
- [Wdrażanie kontenerem systemu Windows](service-fabric-deploy-container.md)
- [Wdrażanie Docker kontenera](service-fabric-deploy-container-linux.md)

W tym artykule opisano tworzenie konteneryzowanego usług w kontenerach Docker w systemie Linux.

Usługa tkaninie występuje kilka możliwości kontenera, ułatwiające tworzenia aplikacji, które składają się z microservices, które są konteneryzowanego. Te są nazywane konteneryzowanego usług.

Możliwości obejmują;

- Kontener wdrażania obrazu i aktywacja
- Zarządzanie zasobów
- Uwierzytelnianie repozytorium
- Kontener port mapowanie portów hosta
- Odnajdowanie kontenerami i komunikacji
- Możliwość skonfigurowania i zmienne środowiska


## <a name="packaging-a-docker-container-with-yeoman"></a>Pakowanie kontenera docker z yeoman
Pakowanie kontenera w systemie Linux, można wybrać albo za pomocą szablonu yeoman lub [ręcznie utworzyć pakiet aplikacji](service-fabric-deploy-container.md#manually-packaging-and-deploying-a-container).

Aplikacja usługi tkaninie może zawierać co najmniej jeden kontenery, każda z konkretnej roli w przedstawiania funkcje aplikacji. Usługa SDK tkaninie Linux zawiera generator [Yeoman](http://yeoman.io/) , który ułatwia tworzenie aplikacji i Dodaj obraz kontener. Aby utworzyć nową aplikację z jednego kontenera Docker o nazwie *SimpleContainerApp*, można użyć Yeoman. Możesz dodać więcej usług później, edytując wygenerowane pojawiają plików.

## <a name="create-the-application"></a>Tworzenie aplikacji

1. W polu terminal, wpisz **yo azuresfguest**.

2. W ramach wybierz **kontener** .

3. Nazwa aplikacji SimpleContainerApp np.

4. Podaj adres URL obrazu kontenera z DockerHub repo. Spowoduje to przejście formularza [repo] / [nazwa obrazu]

![Generator Yeoman tkaninie usługi dla kontenerów][sf-yeoman]

## <a name="deploy-the-application"></a>Wdrażanie aplikacji

Po skonstruowaniu aplikacji należy wdrożyć go z klastrem lokalnej przy użyciu interfejsu wiersza polecenia Azure.

1. Połącz się z lokalnym klastrem tkaninie usługi.

    ```bash
    azure servicefabric cluster connect
    ```

2. Skorzystaj z tego skryptu instalacji opisane w szablonie, skopiuj pakiet aplikacji do sklepu obraz klaster, zarejestrować typ aplikacji oraz tworzenia wystąpienie aplikacji.

    ```bash
    ./install.sh
    ```

3. Otwórz przeglądarkę sieci i przejdź do usługi tkaninie Eksploratora u http://localhost:19080-Eksploratora (Zamień host lokalny z prywatne IP maszyn wirtualnych, korzystając z Vagrant w systemie Mac OS X).

4. Rozwiń węzeł aplikacji i zwróć uwagę, że jest teraz wpis typu aplikacji, a innej pierwsze wystąpienie tego typu.

5. Odinstaluj skrypt podany w szablonie umożliwia usuwanie aplikację i wyrejestrowywanie typ aplikacji.

    ```bash
    ./uninstall.sh
    ```

## <a name="next-steps"></a>Następne kroki

- [Omówienie usług tkaninie i kontenerów](service-fabric-containers-overview.md)
- [Interakcja z klastrów tkaninie usługi przy użyciu interfejsu wiersza polecenia Azure](service-fabric-azure-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-deploy-container-linux/sf-container-yeoman.png

