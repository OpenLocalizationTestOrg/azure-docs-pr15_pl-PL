<properties
    pageTitle="Pobierz Azure SDK dla PHP"
    description="Dowiedz się, jak pobrać i zainstalować Azure SDK dla PHP."
    documentationCenter="php"
    services="app-service\web"
    authors="allclark"
    manager="douge"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="06/01/2016"
    ms.author="allclark;yaqiyang"/>

#<a name="download-the-azure-sdk-for-php"></a>Pobierz Azure SDK dla PHP

## <a name="overview"></a>Omówienie

Zestaw SDK Azure dla PHP obejmuje składniki, które umożliwiają opracowywania, wdrażania i zarządzania aplikacjami PHP dla Azure. W szczególności SDK Azure dla PHP jest następująca:

* **Biblioteki klienta PHP Azure**. Te biblioteki klasy udostępniają interfejs umożliwiający dostęp do funkcji Azure, takich jak usługi zarządzania danymi i usług w chmurze.  
* **Azure interfejs wiersza polecenia dla komputerów Mac, Linux i systemu Windows (polecenie Azure)**. To jest zestaw poleceń dotyczących wdrażania i zarządzania Azure usługi, takie jak Azure witryn sieci Web i maszyn wirtualnych Azure. Praca polecenie Azure na dowolnej platformie, w tym Mac, Linux i systemu Windows.
* **Azure programu PowerShell (tylko w systemie Windows)**. To jest zestaw poleceń cmdlet programu PowerShell dotyczących wdrażania i zarządzania usługi Azure, takie jak usług w chmurze i maszyn wirtualnych.
* **Azure emulatory (tylko w systemie Windows)**. Emulatory obliczeń i miejsca do magazynowania są lokalne emulatory usługi zarządzania danych, które umożliwiają sprawdzanie lokalnie aplikacji i usług w chmurze. Emulatory Azure uruchamiane tylko w systemie Windows.

Poniższych sekcjach opisano, jak pobrać i zainstalować składniki opisany powyżej.

Z instrukcjami podanymi w tym temacie założono, że masz [PHP] [ install-php] zainstalowany.

> [AZURE.NOTE] Musi być PHP 5,5 lub wyższej przy użyciu bibliotek klienta PHP dla Azure.

##<a name="php-client-libraries-for-azure"></a>Biblioteki klienta PHP Azure

PHP bibliotek klienta dla Azure udostępnia interfejs do uzyskiwania dostępu do funkcji Azure, takich jak usługi zarządzania danymi i usług z dowolnego systemu operacyjnego w chmurze. Te biblioteki można zainstalować za pośrednictwem kompozytor.

Aby uzyskać informacje o używaniu bibliotek klienta PHP dla Azure, zobacz, [jak korzystać z usługi obiektów Blob][blob-service], [jak korzystać z usługi tabeli] [ table-service] i [jak używać usługi kolejki][queue-service].

###<a name="install-via-composer"></a>Instalowanie przy użyciu Kompozytor

1. [Instalowanie cyfra][install-git].


    > [AZURE.NOTE] W systemie Windows będzie również musisz dodać cyfra wykonywalny zmiennej środowiska usługi ścieżki.

2. Tworzenie pliku o nazwie **composer.json** w katalogu głównym projektu i Dodaj następujący kod do niego:

        {
            "require": {
                "microsoft/windowsazure": "^0.4"
            }
        }

3. Pobierz ** [composer.phar] [ composer-phar] ** w główną projektu.

4. Otwórz okno wiersza polecenia i wykonać to w główną projektu

        php composer.phar install

##<a name="azure-powershell-and-azure-emulators"></a>Azure programu PowerShell i emulatory Azure

Azure PowerShell to zestaw poleceń cmdlet środowiska PowerShell wdrażanie usług oraz zarządzania nimi Azure (na przykład usług w chmurze i maszyn wirtualnych). Emulatory Azure są emulatory usługi zarządzania danych, które umożliwiają sprawdzanie lokalnie aplikacji i usług w chmurze. Te elementy są obsługiwane tylko w systemie Windows.

Zalecany sposób zainstalować Azure programu PowerShell i emulatory Azure ma za pomocą [Instalatora platformy sieci Web firmy Microsoft][download-wpi]. Zauważ, że możesz również wybrać zainstalować inne składniki rozwoju, takich jak PHP, SQL Server, Drivers firmy Microsoft dla programu SQL Server dla PHP i WebMatrix.

Aby dowiedzieć się, jak za pomocą programu PowerShell Azure, zobacz [jak za pomocą programu PowerShell Azure][powershell-tools].

##<a name="azure-cli"></a>Polecenie Azure

Polecenie Azure to zestaw poleceń dotyczących wdrażania i zarządzania Azure usługi, takie jak Azure witryn sieci Web i maszyn wirtualnych Azure. Aby uzyskać informacje o instalowaniu polecenie Azure zobacz [Instalowanie polecenie Azure](xplat-cli-install.md).

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji zobacz [Centrum deweloperów PHP](/develop/php/).


[install-php]: http://www.php.net/manual/en/install.php
[composer-github]: https://github.com/composer/composer
[composer-phar]: http://getcomposer.org/composer.phar
[nodejs-org]: http://nodejs.org/
[install-node-linux]: https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager
[download-wpi]: http://go.microsoft.com/fwlink/?LinkId=253447
[mac-installer]: http://go.microsoft.com/fwlink/?LinkId=252249
[blob-service]: http://go.microsoft.com/fwlink/?LinkId=252714
[table-service]: http://go.microsoft.com/fwlink/?LinkId=252715
[queue-service]: http://go.microsoft.com/fwlink/?LinkId=252716
[azure cli]: http://go.microsoft.com/fwlink/?LinkId=252717
[powershell-tools]: http://go.microsoft.com/fwlink/?LinkId=252718
[php-sdk-github]: http://go.microsoft.com/fwlink/?LinkId=252719
[install-git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
