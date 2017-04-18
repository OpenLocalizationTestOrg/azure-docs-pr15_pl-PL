<properties 
  pageTitle="Wdrażanie własnego rejestru Docker prywatne Azure | Microsoft Azure"
  description="Opis sposobu przy użyciu rejestru Docker do obsługi obrazów kontenera w usłudze Azure magazyn obiektów Blob."
  services="virtual-machines-linux"
  documentationCenter="virtual-machines"
  authors="ahmetalpbalkan"
  editor="squillace"
  manager="timlt"
  tags="azure-service-management,azure-resource-manager" />

<tags
  ms.service="virtual-machines-linux"
  ms.devlang="multiple"
  ms.topic="article"
  ms.tgt_pltfrm="vm-linux"
  ms.workload="infrastructure-services"
  ms.date="09/27/2016" 
  ms.author="ahmetb" />

# <a name="deploying-your-own-private-docker-registry-on-azure"></a>Wdrażanie własnego rejestru Docker prywatne Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]



W tym dokumencie opisano jakie Docker, prywatne rejestru jest i pokazano, jak można wdrożyć obraz kontenera 2.0 rejestru Docker Docker rejestru prywatne na Microsoft Azure za pomocą magazyn obiektów Blob platformy Azure.

Zakłada tego dokumentu:

1. Możesz dowiedzieć się, jak za pomocą Docker i Docker obrazów do przechowywania. (Nie zostanie? [Dowiedz się więcej o Docker](https://www.docker.com))
2. Masz serwera, który został zainstalowany aparat Docker. (Nie zostanie? [To szybko zrobić Azure.](https://azure.microsoft.com/documentation/templates/docker-simple-on-ubuntu/))


## <a name="what-is-a-private-docker-registry"></a>Co to jest prywatne rejestru Docker?

Aby wysłać konteneryzowanego aplikacje w chmurze, utworzyć obraz kontener Docker i zapisać go innym, aby mogą być używane przez siebie i przez inne osoby. 

Podczas tworzenia obrazu kontenera i wysyłania go w chmurze jest łatwe, jest trudne do przechowywania wygenerowanych obrazu w wiarygodny sposób. Z tego powodu Docker udostępnia scentralizowaną usługą o nazwie [Centrum Docker] [ docker-hub] do przechowywania kontenera obrazów w chmurze i pozwala na tworzenie kontenerów w dowolnym momencie przy użyciu tych obrazów.

Mimo że [Centrum Docker] [ docker-hub] jest usługą płatną do przechowywania do kontenera prywatnych aplikacji obrazy potrzeb deweloperów aspektach Docker i udostępnia narzędzia otwórz źródłowy przechowywanie obrazów w własne prywatne rejestru Docker za zaporą lub lokalnego bez naciśnięcie publicznego Internetu.
Ponieważ magazyn obiektów Blob platformy Azure jest łatwe do bezpiecznego, szybko umożliwia jego tworzenie i używanie prywatnych rejestru Docker platformy Azure, który kontrolować samodzielnie.

## <a name="why-should-you-host-a-docker-registry-on-azure"></a>Dlaczego możesz udostępniać rejestru Docker Azure

Hostingu wystąpienia rejestru Docker Microsoft Azure i przechowywanie obrazów na magazyn obiektów Blob platformy Azure, może mieć wiele zalet:

**Zabezpieczeń:** Obrazy Docker nie pozostanie Azure centrach danych —, aby ta osoba nie przechodzą publicznego Internetu, tak jak w przypadku używania Centrum Docker.
  
**Wydajności:** Obrazy Docker są przechowywane w ramach tego samego centrum danych lub regionu jako aplikacji. Oznacza to, że obrazy będą pobierane, szybszą i bardziej niezawodnego w porównaniu z Centrum Docker.

**Niezawodności:** Korzystając z magazynem obiektów Blob platformy Microsoft Azure, może mieć wiele właściwości miejsca do magazynowania, takich jak duża dostępność, nadmiarowości, miejsca do magazynowania premium SSD i tak dalej.

## <a name="configuring-docker-registry-to-use-azure-blob-storage"></a>Konfigurowanie rejestru Docker w celu korzystania z magazynem obiektów Blob platformy Azure

(Zalecamy zapoznać się z [dokumentacją 2.0 rejestru Docker][rejestru dokumentów] przed kontynuowaniem.)

Możesz [skonfigurować] [ registry-config] rejestru Docker na dwa różne sposoby.
Można:

1. Używanie `config.yml` pliku. W tym przypadku należy utworzyć osobne obraz Docker nad `registry` obrazu.
2. Zastępuje domyślny plik konfiguracji za pośrednictwem zmienne środowiska: otrzymuje zadań bez tworzenia i utrzymywania oddzielnych obraz Docker.

W celu uproszczenia w tym temacie wykonuje opcja 2, przy użyciu zmiennych środowiska.

Aby uruchomić rejestru Docker wystąpienia, które:

* używa konta magazynu platformy Azure do przechowywania obrazów
* oczekuje na porcie maszyny wirtualnej 5000
* występują żadne uwierzytelnianie skonfigurowane (niezalecane, zobacz poniższą uwagę)

należy uruchomić następujące polecenie Docker w usługi terminalowe imprezie (Zamień `<storage-account>` i `<storage-key>` przy użyciu poświadczeń):

```sh
$ docker run -d -p 5000:5000 \
     -e REGISTRY_STORAGE=azure \
     -e REGISTRY_STORAGE_AZURE_ACCOUNTNAME="<storage-account>" \
     -e REGISTRY_STORAGE_AZURE_ACCOUNTKEY="<storage-key>" \
     -e REGISTRY_STORAGE_AZURE_CONTAINER="registry" \
     --name=registry \
     registry:2
```

Po zamknięciu polecenia, zobaczysz kontenerze hostingu prywatne wystąpienia Docker rejestru, uruchamiając `docker ps` polecenia na hoście:

```sh
$ docker ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS                    NAMES
3698ddfebc6f        registry:2          "registry cmd/regist   2 seconds ago       Up 1 seconds        0.0.0.0:5000->5000/tcp   registry
```

> [AZURE.IMPORTANT] Konfigurowanie zabezpieczeń rejestru Docker wykracza poza zakres tego dokumentu i rejestru będą dostępne dla wszystkich osób bez uwierzytelniania domyślnie otwarcia portu do portu rejestru punkt końcowy maszyn wirtualnych lub Usługa równoważenia obciążenia, jeśli użyjesz polecenia wdrożenia powyżej.
>
> Przeczytaj [Konfigurowanie rejestru Docker] [ registry-config] dokumentacji, aby dowiedzieć się, jak bezpiecznego wystąpienie rejestru i obrazów.

## <a name="next-steps"></a>Następne kroki

Po umieszczeniu rejestru Konfigurowanie nadszedł czas na Przejdź niektóre więcej go używać. Uruchom docker [rejestru dokumentów]. 

[docker-hub]: https://hub.docker.com/
[registry]: https://github.com/docker/distribution
[dokumenty rejestru]: http://docs.docker.com/registry/
[registry-config]: http://docs.docker.com/registry/configuration/
 
