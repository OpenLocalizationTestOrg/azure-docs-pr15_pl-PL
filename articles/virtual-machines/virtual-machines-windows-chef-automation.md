<properties
   pageTitle="Wdrażania Azure maszyn wirtualnych z Kuchmistrz | Microsoft Azure"
   description="Dowiedz się, jak wykonywać za pomocą Kuchmistrz wdrażania automatyczną maszyn wirtualnych i konfiguracji Microsoft Azure"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="diegoviso"
   manager="timlt"
   tags="azure-service-management,azure-resource-manager"
   editor=""/>

<tags ms.service="virtual-machines-windows" ms.workload="infrastructure-services"
ms.tgt_pltfrm="vm-multiple"
ms.devlang="na"
ms.topic="article"
ms.date="05/19/2015"
ms.author="diviso"/>

# <a name="automating-azure-virtual-machine-deployment-with-chef"></a>Automatyzowanie wdrażania Azure maszyn wirtualnych z Kuchmistrz

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Kuchmistrz jest doskonałym narzędziem do przedstawiania automatyzacji i potrzeby stan konfiguracji.

Z naszych najnowszej wersji interfejsu api chmury Kuchmistrz zawiera Integracja z Azure, które zapewnia możliwości obsługi administracyjnej i wdrażanie konfiguracji Stany za pomocą jednego polecenia.

W tym artykule I opisano sposób konfigurowania środowiska Kuchmistrz do obsługi administracyjnej Azure maszyn wirtualnych i przeprowadził Cię przez tworząc zasady lub "Książka kucharska", a następnie wdrażania tego książka kucharska Azure maszyn wirtualnych.

Zaczynamy!

## <a name="chef-basics"></a>Podstawowe informacje o kuchmistrz

Przed rozpoczęciem sugeruje recenzowania podstawowych pojęć dotyczących Kuchmistrz. Istnieje doskonała materiałowych <a href="http://www.chef.io/chef" target="_blank">poniżej</a> i jest zalecane jest szybkie odczytu przed podjęciem próby w tym instruktażu. Będzie można jednak recap podstawy początek.

Poniższy diagram przedstawia architekturę wysokiego poziomu znajduje.

![][2]

Kuchmistrz występują trzy główne składniki architektury: Server Kuchmistrz, Kuchmistrz klienta (węzeł) i roboczej znajduje.

Serwer Kuchmistrz naszych punkt zarządzania i dostępne są dwie opcje serwera Kuchmistrz: hostowanej rozwiązania lub lokalnego rozwiązania. Użyjemy hostowanej rozwiązanie.

Klient Kuchmistrz (węzeł) jest agenta, który znajduje się na serwerach, którym zarządzasz.

Stację Kuchmistrz jest naszych roboczej administrator miejsce, w którym możemy utworzyć nasze zasady i wykonywania naszych zarządzania. Firma Microsoft uruchomienia polecenia **nóż** roboczej znajduje zarządzania infrastrukturą naszych.

Istnieje także pojęcie "Cookbooks" i "Przepisy". Efektywne są zasady możemy definiowanie i zastosować do naszych serwerów.

## <a name="preparing-the-workstation"></a>Przygotowywanie stacji

Najpierw umożliwia przygotowywanie stację. Korzystam z standardowy roboczej systemu Windows. Trzeba utworzyć katalog do przechowywania naszych plików konfiguracji i cookbooks.

Najpierw utwórz folder o nazwie C:\chef.

Następnie utwórz drugą katalogu o nazwie c:\chef\cookbooks.

Teraz należy pobrać plik naszych Azure ustawień, aby Kuchmistrz można komunikować się z naszym Azure subskrypcji.

Pobieranie programu ustawienia [tutaj](https://manage.windowsazure.com/publishsettings/)publikowania.

Zapisz plik ustawień Publikuj w C:\chef.

##<a name="creating-a-managed-chef-account"></a>Tworzenie konta zarządzane Kuchmistrz

Załóż konto znajduje hostowanej [tutaj](https://manage.chef.io/signup).

Podczas tworzenia konta zostanie wyświetlony monit do utworzenia nowej organizacji.

![][3]

Po utworzeniu organizacji, Pobierz zestaw starter.

![][4]

> [AZURE.NOTE] Jeśli zostanie wyświetlony monit, ostrzeżenie, że kluczy zostaną zresetowane, jest ok, aby kontynuować, jak firma Microsoft nie istniejącej infrastruktury jeszcze skonfigurowane.

Ten plik zip starter kit zawiera pliki konfiguracji organizacji i klawiszy.

##<a name="configuring-the-chef-workstation"></a>Konfigurowanie stacji Kuchmistrz

Wyodrębnianie zawartości starter.zip kuchmistrz, aby C:\chef.

Skopiuj wszystkie pliki w obszarze kuchmistrz starter\chef-repo\.kuchmistrz do katalogu c:\chef.

Katalogu powinna wyglądać podobnie w poniższym przykładzie.

![][5]

Teraz powinny mieć cztery pliki w tym Azure publikowania w katalogu c:\chef.

Pliki PEM zawierają Twojej organizacji i kluczy prywatnych administratora dla komunikacji, gdy plik knife.rb zawiera konfigurację Nóż. Będzie trzeba edytować plik knife.rb.

Otwórz plik w edytorze wybór i modyfikowanie "cookbook_path", usuwając... / ze ścieżki, wydaje się, jak pokazano dalej.

    cookbook_path  ["#{current_dir}/cookbooks"]

Również Dodaj następujący wiersz odzwierciedlające nazwę usługi Azure publikowanie plik ustawień.

    knife[:azure_publish_settings_file] = "yourfilename.publishsettings"

Plik knife.rb powinna wyglądać podobnie do następującego przykładu.

![][6]

Te wiersze zapewni nóż odwołuje się do katalogu cookbooks w obszarze c:\chef\cookbooks, a także używa naszych pliku ustawienia publikowania Azure podczas operacji Azure.

## <a name="installing-the-chef-development-kit"></a>Instalowanie Kuchmistrz Development Kit

Następny, [Pobieranie i instalowanie](http://downloads.getchef.com/chef-dk/windows) ChefDK (Kuchmistrz Development Kit), aby skonfigurować pracy znajduje.

![][7]

Zainstaluj w domyślnej lokalizacji c:\opscode. Tę instalację potrwa około 10 minut.

Upewnij się, że do zmiennej ŚCIEŻKA zawiera wpisy dla C:\opscode\chefdk\bin; C:\opscode\chefdk\embedded\bin;c:\users\yourusername\.chefdk\gem\ruby\2.0.0\bin

Jeśli nie są dostępne, upewnij się, że możesz dodać te ścieżki!

*NALEŻY ZAUWAŻYĆ, ŻE KOLEJNOŚĆ ŚCIEŻKI JEST WAŻNA!* Jeśli do ścieżki opscode nie znajdują się w poprawnej kolejności masz problemy.

Uruchom ponownie miejscu pracy, przed kontynuowaniem.

Następnie możemy zostaną zainstalowane rozszerzenia Azure Nóż. Dzięki temu nóż z "Dodatek Azure".

Uruchom następujące polecenie.

    chef gem install knife-azure ––pre

> [AZURE.NOTE] Argument — sprzed gwarantuje, że otrzymujesz RC najnowszą wersję dodatku Azure nóż, który zapewnia dostęp do najnowszych zestaw interfejsów API.

Istnieje prawdopodobieństwo, że liczba zależności zostanie również zainstalowany w tym samym czasie.

![][8]


Aby upewnić się, że wszystko jest poprawnie skonfigurowany, uruchom następujące polecenie.

    knife azure image list

Jeśli wszystko jest poprawnie skonfigurowany, pojawi się lista dostępnych obrazów Azure przewinąć.

Gratulacje. Konfigurowanie stację!

##<a name="creating-a-cookbook"></a>Tworzenie książka kucharska

Książka kucharska jest używana przez Kuchmistrz do określania zestawu poleceń, które chcesz wykonać na komputerze klienckim zarządzane. Tworzenie książki kucharskiej jest proste i firma Microsoft korzysta z polecenia **kuchmistrz wygenerować książka kucharska** do generowania naszych szablonów książka kucharska. I będą zgłaszać zapytania Mój serwer sieci web książka kucharska jako chcę zasadę, która powoduje automatyczne wdrożenie programu IIS.

W obszarze katalogu C:\Chef uruchom następujące polecenie.

    chef generate cookbook webserver

Spowoduje to wygenerowanie zestawu plików w katalogu C:\Chef\cookbooks\webserver. Teraz należy zdefiniować zestaw poleceń, że chcemy naszemu klientowi Kuchmistrz do wykonania na nasz zarządzanych maszyn wirtualnych.

Polecenia są przechowywane w default.rb pliku. W tym pliku I będzie można Definiowanie zestaw poleceń instaluje IIS, zostanie uruchomiony usług IIS i skopiowanie pliku szablonu w folderze wwwroot.

Modyfikowanie pliku C:\chef\cookbooks\webserver\recipes\default.rb i dodaj następujące wiersze.

    powershell_script 'Install IIS' do
        action :run
        code 'add-windowsfeature Web-Server'
    end

    service 'w3svc' do
        action [ :enable, :start ]
    end

    template 'c:\inetpub\wwwroot\Default.htm' do
        source 'Default.htm.erb'
        rights :read, 'Everyone'
    end

Po wykonaniu tych czynności, należy zapisać plik.

## <a name="creating-a-template"></a>Tworzenie szablonu

Jak wcześniej wspomniano, potrzebne do wygenerowania pliku szablonu, który będzie używany jako strony default.html.

Uruchom następujące polecenie, aby wygenerować szablonu.

    chef generate template webserver Default.htm

Teraz przejdź do pliku C:\chef\cookbooks\webserver\templates\default\Default.htm.erb. Edytowanie pliku przez dodanie kilka prostych kodu "Witaj świecie" HTML, a następnie zapisz ten plik.



## <a name="upload-the-cookbook-to-the-chef-server"></a>Przekazywanie książka kucharska na serwerze Kuchmistrz

W tym kroku możemy są sporządzanie kopii książka kucharska, który został utworzony w naszym komputer lokalny i przekazania go do serwera hostowanej znajduje. Po przesłaniu książka kucharska pojawi się na karcie **zasady** .

    knife cookbook upload webserver

![][9]

## <a name="deploy-a-virtual-machine-with-knife-azure"></a>Wdrażanie maszyny wirtualnej z nóż Azure

Teraz możemy wdrażanie Azure maszyn wirtualnych i stosowanie książka kucharska "Serwer_sieci_web", który zainstaluje naszych usług IIS sieci web usługi i domyślnej strony sieci web.

Aby to zrobić, użyj polecenia **nóż azure server umożliwia tworzenie** .

Czy przykład polecenia pojawi się dalej.

    knife azure server create --azure-dns-name 'diegotest01' --azure-vm-name 'testserver01' --azure-vm-size 'Small' --azure-storage-account 'portalvhdsxxxx' --bootstrap-protocol 'cloud-api' --azure-source-image 'a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-Datacenter-201411.01-en.us-127GB.vhd' --azure-service-location 'Southeast Asia' --winrm-user azureuser --winrm-password 'myPassword123' --tcp-endpoints 80,3389 --r 'recipe[webserver]'

Parametry są oczywiste. Podstaw określonego zmiennych i uruchom.

> [AZURE.NOTE] Za pomocą wiersza polecenia I jest również Automatyzowanie reguł filtru punktu końcowego sieci przy użyciu parametru punkty końcowe — tcp. Otwarty w górę porty 80 i 3389 umożliwia dostęp do mojej strony sieci web i RDP sesji.

Po uruchomieniu polecenia przejdź do portalu Azure i komputera zacząć Obsługa administracyjna zostanie wyświetlona.

![][13]

Następnie zostanie wyświetlona wiersz polecenia.

![][10]

Po zakończeniu rozmieszczania firma Microsoft należy połączyć się z usługą sieci web przez port 80, jak firma Microsoft był otwierany portu, gdy możemy obsługi administracyjnej maszyny wirtualnej z poleceniem Azure Nóż. Tak jak to maszyn wirtualnych tylko maszyny wirtualnej w usłudze w chmurze, można będzie go połączyć z adresu url usługi cloud.

![][11]

Jak widać, przeze mnie uzyskana creative z moim kodu HTML.

Pamiętaj, że firma Microsoft może nawiązywania połączenia za pośrednictwem sesję RDP z portalu klasyczny Azure za pośrednictwem portu 3389.

Mogę Życzymy było to pomocne! Przejdź i rozpocząć infrastruktury jako kod podróży z Azure już dziś!


<!--Image references-->
[2]: ./media/virtual-machines-windows-chef-automation/2.png
[3]: ./media/virtual-machines-windows-chef-automation/3.png
[4]: ./media/virtual-machines-windows-chef-automation/4.png
[5]: ./media/virtual-machines-windows-chef-automation/5.png
[6]: ./media/virtual-machines-windows-chef-automation/6.png
[7]: ./media/virtual-machines-windows-chef-automation/7.png
[8]: ./media/virtual-machines-windows-chef-automation/8.png
[9]: ./media/virtual-machines-windows-chef-automation/9.png
[10]: ./media/virtual-machines-windows-chef-automation/10.png
[11]: ./media/virtual-machines-windows-chef-automation/11.png
[13]: ./media/virtual-machines-windows-chef-automation/13.png


<!--Link references-->
