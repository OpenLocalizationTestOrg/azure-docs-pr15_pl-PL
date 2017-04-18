<properties
   pageTitle="Automatyzowanie wdrażaniem aplikacji za pomocą rozszerzenia maszyn wirtualnych | Microsoft Azure"
   description="Samouczek DotNet Core Azure maszyn wirtualnych"
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="neilpeterson"
   manager="timlt"
   editor="tysonn"
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="09/21/2016"
   ms.author="nepeters"/>

# <a name="application-deployment-with-azure-resource-manager-templates"></a>Wdrażanie aplikacji z szablonami Azure Menedżera zasobów

Gdy wszystkie wymagania infrastrukturalne Azure zostały określone i przekształcić w szablonie rozmieszczania, wdrażanie aplikacji rzeczywisty musi należy zająć się. Wdrażanie aplikacji tutaj odwołuje się do instalowania plików binarnych rzeczywisty aplikacji na zasoby Azure. Dla próbki sklep muzyczny, .net podstawowych, NGINX i Kierownik trzeba zainstalować i skonfigurować na każdym komputerze wirtualnych. Sklep muzyczny plików binarnych musi być zainstalowany na komputerze wirtualnych i bazy danych magazynu muzyki wstępnie utworzone.

Ten dokument, szczegóły, jak rozszerzenia maszyn wirtualnych można zautomatyzować wdrażanie aplikacji i konfiguracji do Azure maszyn wirtualnych. Wszystkie zależności i unikatowe konfiguracje są wyróżnione. Aby uzyskać najlepsze wyniki, wstępnie wdrażać wystąpienie rozwiązanie do subskrypcji Azure i pracy wraz z szablonu Azure Menedżera zasobów. Zakończenie szablonu można znaleźć tutaj — [Wdrożenia sklepu muzyki na Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).

## <a name="configuration-script"></a>Skrypt konfiguracji

Rozszerzenia maszyn wirtualnych to specjalistyczne programy, które wykonać maszyn wirtualnych o podanie automatyzacji konfiguracji. Rozszerzenia są dostępne do wielu określonych celów, takich jak oprogramowania antywirusowego, konfigurację rejestrowania i konfiguracji Docker. Rozszerzenie skryptu niestandardowego może służyć w celu uruchomienia dowolny skrypt maszyny wirtualnej. Przykład sklep muzyczny jest rozszerzenie skryptu niestandardowego na konfigurowanie maszyn wirtualnych Ubuntu i zainstaluj aplikację ze sklepu muzyki.

Przed określające, jak rozszerzenia maszyn wirtualnych są zgłoszone w szablonie Azure Menedżera zasobów, Sprawdź skrypt, który zostanie uruchomiony. Ten skrypt konfiguruje maszyny wirtualnej Ubuntu do obsługi aplikacji sklep muzyczny. Po uruchomieniu skrypt instaluje wszystkie potrzebne oprogramowania, zainstaluj aplikację ze sklepu muzykę z kontrolki źródła i przygotowywanie bazy danych. 

Aby dowiedzieć się więcej na temat hostingu .net podstawowe aplikacji w systemie Linux, zobacz [Publikowanie środowisku produkcyjnym Linux](https://docs.asp.net/en/latest/publishing/linuxproduction.html). 

> W tym przykładzie jest do celów pokaz.

```none
#!/bin/bash

# install dotnet core
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ trusty main" > /etc/apt/sources.list.d/dotnetdev.list'
sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
sudo apt-get update
sudo apt-get install -y dotnet-dev-1.0.0-preview2-003121

# download application
sudo wget https://raw.github.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/music-app/music-store-azure-demo-pub.tar /
sudo mkdir /opt/music
sudo tar -xf music-store-azure-demo-pub.tar -C /opt/music

# install nginx, update config file
sudo apt-get install -y nginx
sudo service nginx start
sudo touch /etc/nginx/sites-available/default
sudo wget https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/music-app/nginx-config/default -O /etc/nginx/sites-available/default
sudo cp /opt/music/nginx-config/default /etc/nginx/sites-available/
sudo nginx -s reload

# update and secure music config file
sed -i "s/<replaceserver>/$1/g" /opt/music/config.json
sed -i "s/<replaceuser>/$2/g" /opt/music/config.json
sed -i "s/<replacepass>/$3/g" /opt/music/config.json
sudo chown $2 /opt/music/config.json
sudo chmod 0400 /opt/music/config.json

# config supervisor
sudo apt-get install -y supervisor
sudo touch /etc/supervisor/conf.d/music.conf
sudo wget https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/music-app/supervisor/music.conf -O /etc/supervisor/conf.d/music.conf
sudo service supervisor stop
sudo service supervisor start

# pre-create music store database
/usr/bin/dotnet /opt/music/MusicStore.dll &
```

## <a name="vm-script-extension"></a>Rozszerzenie skrypt maszyn wirtualnych

Rozszerzenia maszyn wirtualnych mogą być uruchamiane na maszyny wirtualnej w czasie kompilacji, dołączając rozszerzenia zasobów w szablonie Azure Menedżera zasobów. Rozszerzenie można dodawać przy użyciu Kreatora programu Visual Studio Dodawanie zasobów lub przez wstawienie prawidłowych JSON do szablonu. Zasób Skrypt rozszerzenia jest zagnieżdżona zasobu maszyn wirtualnych; to są widoczne w poniższym przykładzie.

Wykonaj to łącze, aby wyświetlić przykładowe JSON w szablonie Menedżera zasobów — [Wewnętrzny skrypt maszyn wirtualnych](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L359). 

Zwróć uwagę na poniżej JSON, że skrypt jest przechowywana w GitHub. Skrypt można również przechowywane w magazynie obiektów Blob platformy Azure. Ponadto szablony Azure Menedżera zasobów umożliwiają wykonywanie skryptów ciąg tworzona w taki sposób, że wartości parametrów szablonu może służyć jako parametrów do wykonywania skryptów. W tym przypadku dane są dostarczane wdrażając szablony, a następnie można użyć tych wartości, podczas wykonywania skryptu.

```none
{
  "apiVersion": "2015-06-15",
  "type": "extensions",
  "name": "config-app",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"
      ]
    },
    "protectedSettings": {
      "commandToExecute": "[concat('sudo sh config-music.sh ',variables('musicStoreSqlName'), ' ', parameters('adminUsername'), ' ', parameters('sqlAdminPassword'))]"
    }
  }
}
```

Aby uzyskać więcej informacji na temat korzystania z rozszerzeniem skryptu niestandardowego zobacz [rozszerzenia skryptów niestandardowe szablony Menedżera zasobów](./virtual-machines-linux-extensions-customscript.md).

## <a name="next-step"></a>Następny krok

<hr>

[Poznaj więcej Azure szablony Menedżera zasobów](https://github.com/Azure/azure-quickstart-templates)
