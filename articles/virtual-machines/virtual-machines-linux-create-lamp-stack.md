<properties
    pageTitle="Wdrażanie światło na komputerze wirtualnych Linux | Microsoft Azure"
    description="Dowiedz się, jak zainstalować stos światło na maszyny Linux"
    services="virtual-machines-linux"
    documentationCenter="virtual-machines"
    authors="jluk"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="NA"
    ms.topic="article"
    ms.date="06/07/2016"
    ms.author="juluk"/>

# <a name="deploy-lamp-stack-on-azure"></a>Wdrażanie stos światło Azure
W tym artykule opisano, jak wdrożyć serwer sieci web, MySQL i PHP (stos światło) Azure Apache. Konieczne będzie konto Azure ([Pobierz bezpłatną wersję próbną](https://azure.microsoft.com/pricing/free-trial/)) oraz [Polecenie Azure](../xplat-cli-install.md) , który jest [powiązany z Twoim kontem Azure](../xplat-cli-connect.md).

Istnieją dwie metody instalowania światła omówione w tym artykule:

## <a name="quick-command-summary"></a>Polecenie Szybkie podsumowania

1) Wdrażanie światła w nowych maszyn wirtualnych

```
# One command to create a resource group holding a VM with LAMP already on it
$ azure group create -n uniqueResourceGroup -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json
```

2) Wdrażanie światła w istniejącej maszyn wirtualnych

```
# Two commands: one updates packages, the other installs Apache, MySQL, and PHP
user@ubuntu$ sudo apt-get update
user@ubuntu$ sudo apt-get install apache2 mysql-server php5 php5-mysql
```

## <a name="deploy-lamp-on-new-vm-walkthrough"></a>Wdrażanie światła w nowej Instruktaż maszyn wirtualnych

Możesz rozpocząć od utworzenia nowej [grupy zasobów](../azure-resource-manager/resource-group-overview.md) zawierające maszyn wirtualnych:

    $ azure group create uniqueResourceGroup westus
    info:    Executing command group create
    info:    Getting resource group uniqueResourceGroup
    info:    Creating resource group uniqueResourceGroup
    info:    Created resource group uniqueResourceGroup
    data:    Id:                  /subscriptions/########-####-####-####-############/resourceGroups/uniqueResourceGroup
    data:    Name:                uniqueResourceGroup
    data:    Location:            westus
    data:    Provisioning State:  Succeeded
    data:    Tags: null
    data:
    info:    group create command OK

Aby utworzyć maszyn wirtualnych samej, służy już pisanych znaleziono [tutaj na GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/lamp-app)szablonu Azure Menedżera zasobów.

    $ azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json uniqueResourceGroup uniqueLampName

Powinien zostać wyświetlony odpowiedź monitowanie o niektórych więcej danych wejściowych:

    info:    Executing command group deployment create
    info:    Supply values for the following parameters
    storageAccountNamePrefix: lampprefix
    location: westus
    adminUsername: someUsername
    adminPassword: somePassword
    mySqlPassword: somePassword
    dnsLabelPrefix: azlamptest
    info:    Initializing template configurations and parameters
    info:    Creating a deployment
    info:    Created template deployment "uniqueLampName"
    info:    Waiting for deployment to complete
    data:    DeploymentName     : uniqueLampName
    data:    ResourceGroupName  : uniqueResourceGroup
    data:    ProvisioningState  : Succeeded
    data:    Timestamp          :
    data:    Mode               : Incremental
    data:    CorrelationId      : d51bbf3c-88f1-4cf3-a8b3-942c6925f381
    data:    TemplateLink       : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json
    data:    ContentVersion     : 1.0.0.0
    data:    DeploymentParameters :
    data:    Name                      Type          Value
    data:    ------------------------  ------------  -----------
    data:    storageAccountNamePrefix  String        lampprefix
    data:    location                  String        westus
    data:    adminUsername             String        someUsername
    data:    adminPassword             SecureString  undefined
    data:    mySqlPassword             SecureString  undefined
    data:    dnsLabelPrefix            String        azlamptest
    data:    ubuntuOSVersion           String        14.04.2-LTS
    info:    group deployment create command OK

Teraz utworzono maszyny Linux z światło już zainstalowany na nim. Jeśli chcesz, możesz sprawdzić instalacji przez przejście w dół do pozycji [Sprawdź światło pomyślnie zainstalowane].

## <a name="deploy-lamp-on-existing-vm-walkthrough"></a>Wdrażanie światła w istniejącej Instruktaż maszyn wirtualnych

Jeśli potrzebujesz pomocy w tworzeniu maszyny Linux można głowy [tutaj, aby dowiedzieć się, jak utworzyć maszyny Linux] (. / virtual-machines-linux-quick-create-cli.md). Następnie należy SSH do maszyn wirtualnych Linux. Jeśli potrzebujesz pomocy dotyczącej tworzenia klucza SSH można głowy [tutaj, aby dowiedzieć się, jak utworzyć klucz SSH na Linux-Mac] (. / virtual-machines-linux-mac-create-ssh-keys.md).
Jeśli masz już klawisza SSH, przejdź dalej i SSH do swojego maszyn wirtualnych Linux z `ssh username@uniqueDNS`.

Teraz, gdy pracujesz w maszyn wirtualnych usługi Linux, możemy przeprowadzi przez instalowania stosu światło na podstawie Debian dystrybucji. Dokładne polecenia mogą się różnić dla innych distros Linux.

#### <a name="installing-on-debianubuntu"></a>Instalowanie na Debian-Ubuntu

Konieczne będzie następujące zainstalowanego pakietu: `apache2`, `mysql-server`, `php5`, i `php5-mysql`. Możesz zainstalować te elementy, bezpośrednio dane te pakiety lub przy użyciu Tasksel. Poniżej przedstawiono instrukcje dotyczące obu opcji.
Przed rozpoczęciem instalacji należy pobrać i zainstalować aktualizację pakietu list.

    user@ubuntu$ sudo apt-get update
    
##### <a name="individual-packages"></a>Poszczególne pakiety
Za pomocą stanie get:

    user@ubuntu$ sudo apt-get install apache2 mysql-server php5 php5-mysql

##### <a name="using-tasksel"></a>Przy użyciu Tasksel
Alternatywnie możesz pobrać Tasksel, narzędzie Debian-Ubuntu, które instalacje wielu powiązanych pakietów jako skoordynowanego "zadania" do systemu.

    user@ubuntu$ sudo apt-get install tasksel
    user@ubuntu$ sudo tasksel install lamp-server

Po uruchomieniu jedną z powyższych opcji zostanie wyświetlony monit o zainstalowanie tych pakietów i wiele innych zależności. Naciśnij klawisz "y", a następnie "Enter, aby kontynuować i wykonaj inne instrukcjami, aby ustawić hasło administratora dla MySQL. Minimalne wymagane rozszerzenia PHP, aby używać PHP z MySQL zostanie zainstalowany. 

![][1]

Uruchom następujące polecenie, aby wyświetlić inne rozszerzenia PHP, które są dostępne jako pakietów:

    user@ubuntu$ apt-cache search php5


#### <a name="create-infophp-document"></a>Tworzenie dokumentu info.php

Teraz powinno być możliwe sprawdzić, która wersja programu Apache, MySQL i PHP za pośrednictwem wiersza polecenia, wpisując `apache2 -v`, `mysql -v`, lub `php -v`.

Jeśli chcesz przetestować dodatkowo, możesz utworzyć szybkie stronie informacje o PHP, aby wyświetlić w przeglądarce. Tworzenie nowego pliku z Nano edytora tekstu przy użyciu tego polecenia:

    user@ubuntu$ sudo nano /var/www/html/info.php

W edytorze tekstów GNU Nano Dodaj następujące wiersze:

    <?php
    phpinfo();
    ?>

Następnie zapisz i zamknij Edytor tekstów.

Uruchom ponownie Apache przy użyciu tego polecenia, aby wszystkie nowe instalacje zostanie zastosowane.

    user@ubuntu$ sudo service apache2 restart

## <a name="verify-lamp-successfully-installed"></a>Sprawdź światło zainstalowana pomyślnie

Teraz możesz sprawdzić strona z informacjami o PHP właśnie utworzonego w przeglądarce, przechodząc do http://youruniqueDNS/info.php, powinna wyglądać podobnie do następującej.

![][2]

Instalacji Apache można sprawdzić, wyświetlając Apache2 Ubuntu domyślną stronę, przechodząc do Ciebie http://youruniqueDNS/. Powinien zostać wyświetlony podobny do tego.

![][3]

Gratulacje, masz tylko ustawienia stosem światło na maszyn wirtualnych usługi Azure!

## <a name="next-steps"></a>Następne kroki

Zapoznaj się z dokumentacją Ubuntu w stos światła:

- [https://help.ubuntu.com/Community/ApacheMySQLPHP](https://help.ubuntu.com/community/ApacheMySQLPHP)

[1]: ./media/virtual-machines-linux-deploy-lamp-stack/configmysqlpassword-small.png
[2]: ./media/virtual-machines-linux-deploy-lamp-stack/phpsuccesspage.png
[3]: ./media/virtual-machines-linux-deploy-lamp-stack/apachesuccesspage.png
