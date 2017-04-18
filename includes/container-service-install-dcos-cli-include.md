<properties
   pageTitle="Instalowanie kontrolera domeny i OS polecenie | Microsoft Azure"
   description="Instalowanie kontrolera domeny i OS polecenie."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Kontenery, Micro usług kontrolera domeny/systemu operacyjnego, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/10/2016"
   ms.author="rogardle"/>

>[AZURE.NOTE] Jest to praca z klastrów systemu kontrolera domeny i OS ACS. Istnieje bez konieczności to zrobić dla klastrów opartych na rój ACS.

Przede wszystkim, [Nawiązywanie połączenia z klaster systemu kontrolera domeny i OS ACS](../articles/container-service/container-service-connect.md). Po wykonaniu tej polecenie kontrolera domeny i OS można zainstalować na komputerze klienta z poniższych poleceń:

```bash
sudo pip install virtualenv
mkdir dcos && cd dcos
wget https://raw.githubusercontent.com/mesosphere/dcos-cli/master/bin/install/install-optout-dcos-cli.sh
chmod +x install-optout-dcos-cli.sh
./install-optout-dcos-cli.sh . http://localhost --add-path yes
```

Jeśli używasz starszej wersji programu Python można zauważyć, niektóre "InsecurePlatformWarnings". Można zignorować te.

Aby rozpocząć bez ponownego uruchomienia programu powłoki, uruchom polecenie:

```bash
source ~/.bashrc
```

Ten krok nie będzie konieczne po rozpoczęciu nowego powłoki.

Teraz można sprawdzić, czy jest zainstalowany, polecenie:

```bash
dcos --help
```