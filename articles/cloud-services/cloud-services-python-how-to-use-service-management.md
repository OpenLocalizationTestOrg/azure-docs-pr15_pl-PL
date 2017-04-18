<properties
    pageTitle="Jak używać zarządzania usługą interfejsu API (Python) — przewodnik dla funkcji"
    description="Dowiedz się, jak programowo wykonywanie typowych zadań zarządzania usługi z Python."
    services="cloud-services"
    documentationCenter="python"
    authors="lmazuel"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="lmazuel"/>

# <a name="how-to-use-service-management-from-python"></a>Jak za pomocą usługi Zarządzanie z Python

> [AZURE.NOTE] Interfejs API usługi zarządzania jest zastępowany przy użyciu nowego zasobu zarządzania interfejsu API, jest obecnie dostępna w wersji preview.  Zapoznaj się z [dokumentacją zarządzania zasobami Azure](http://azure-sdk-for-python.readthedocs.org/) Aby uzyskać szczegółowe informacje na temat korzystania z nowego interfejsu API zarządzania zasobów z Python.

Ten przewodnik pokazano, jak programowo wykonywanie typowych zadań zarządzania usługi z Python. Klasa **ServiceManagementService** w zestawie [SDK Azure dla Python](https://github.com/Azure/azure-sdk-for-python) obsługuje programowy dostęp do wiele usługi związanych z zarządzaniem funkcji dostępnej w [portal Azure klasyczny] [ management-portal] (na przykład **Tworzenie, aktualizowanie i usuwanie usługami w chmurze, wdrożenia usługi zarządzania danymi i maszyn wirtualnych**). Ta funkcja może być przydatne w tworzenia aplikacji, które wymagają programowy dostęp do zarządzania usługą.

## <a name="WhatIs"> </a>Co to jest usługa Zarządzanie
Interfejs API zarządzania usługi zapewnia programowy dostęp do wiele funkcji zarządzania usługi dostępne za pośrednictwem [portalu klasyczny Azure][management-portal]. Zestaw SDK Azure dla Python umożliwia zarządzanie usługami w chmurze i kont miejsca do magazynowania.

Aby korzystać z interfejsu API zarządzania usługi, musisz [utworzyć konto Azure](https://azure.microsoft.com/pricing/free-trial/).

## <a name="Concepts"> </a>Pojęcia
[Interfejs API zarządzania usługi Azure]był zawijany SDK Azure dla Python[svc-mgmt-rest-api], która jest interfejsu API usługi REST. Wszystkie operacje interfejsu API są wykonywane przez SSL i wzajemnie uwierzytelnianie za pomocą certyfikatów X.509 v3. Usługi zarządzania można uzyskać dostęp z poziomu usługi uruchamiania w Azure lub bezpośrednio w Internecie z dowolnej aplikacji, która żądanie HTTPS wysyłania i odbierania odpowiedzi HTTPS.

## <a name="Installation"> </a>Instalacji

Wszystkie funkcje opisane w tym artykule są dostępne w `azure-servicemanagement-legacy` pakietu, którą można zainstalować za pomocą pip. Aby uzyskać więcej informacji na temat instalacji (na przykład, jeśli jesteś nowym użytkownikiem Python), można znaleźć w tym artykule: [Instalowanie Python i Azure SDK](../python-how-to-install.md)

## <a name="Connect"> </a>Jak: nawiązywanie połączenia z Zarządzanie usługą
Nawiązać połączenie do punktu końcowego usługi zarządzania, potrzebujesz identyfikator subskrypcji Azure i certyfikatu zarządzania prawidłowe. Można uzyskać identyfikator subskrypcji za pomocą [portal Azure klasyczny][management-portal].

> [AZURE.NOTE] Ponieważ Azure SDK dla Python v0.8.0 jest teraz możliwość korzystania z certyfikatów utworzone za pomocą OpenSSL, gdy działa w systemie Windows.  Wymaga Python 2.7.4 lub nowszym. Zalecamy, aby użytkownicy mogli używać OpenSSL zamiast PFX, począwszy od pomocy technicznej dotyczącej PFX certyfikaty prawdopodobnie zostaną usunięte w przyszłości.

### <a name="management-certificates-on-windowsmaclinux-openssl"></a>Certyfikaty zarządzania na Mac-systemu Windows i Linux (OpenSSL)
[OpenSSL](http://www.openssl.org/) umożliwia tworzenie certyfikatu zarządzania.  Faktycznie, należy utworzyć dwa certyfikaty, jedną dla serwera ( `.cer` pliku) i jedną dla klienta ( `.pem` pliku). Aby utworzyć `.pem` plików, należy wykonać:

    openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

Aby utworzyć `.cer` certyfikatu, należy wykonać:

    openssl x509 -inform pem -in mycert.pem -outform der -out mycert.cer

Aby uzyskać więcej informacji na temat certyfikatów Azure zobacz [Omówienie certyfikatów dla usług w chmurze Azure](./cloud-services-certs-create.md). Pełny opis parametrów OpenSSL zobacz dokumentację w [http://www.openssl.org/docs/apps/openssl.html](http://www.openssl.org/docs/apps/openssl.html).

Po utworzeniu tych plików należy przekazać `.cer` pliku Azure przy użyciu akcji "Przekazywania" "ustawienia" [portal Azure klasycznego][management-portal], i zanotuj miejsca zapisywania `.pem` pliku.

Po uzyskaniu Identyfikatora subskrypcji utworzono certyfikat i przekazać `.cer` pliku Azure, można nawiązać punkt końcowy Azure zarządzania przekazując identyfikator subskrypcji i ścieżkę do `.pem` pliku **ServiceManagementService**:

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = '<path_to_.pem_certificate>'

    sms = ServiceManagementService(subscription_id, certificate_path)

W powyższym przykładzie `sms` jest obiektem **ServiceManagementService** . Klasa **ServiceManagementService** jest klasą podstawowego służące do zarządzania usług Azure.

### <a name="management-certificates-on-windows-makecert"></a>Certyfikaty zarządzania w systemie Windows (MakeCert)

Możesz utworzyć certyfikat z podpisem zarządzania przy użyciu swojego komputera `makecert.exe`.  Otwórz **Wiersz polecenia programu Visual Studio** jako **administrator** , a następnie użyj następującego polecenia, zamieniając *AzureCertificate* nazwa certyfikatu, którego chcesz użyć.

    makecert -sky exchange -r -n "CN=AzureCertificate" -pe -a sha1 -len 2048 -ss My "AzureCertificate.cer"

Polecenie tworzy `.cer` plik i instaluje je w magazynie certyfikatów **osobistych** . Aby uzyskać więcej informacji zobacz [Omówienie certyfikatów dla usług w chmurze Azure](./cloud-services-certs-create.md).

Po utworzeniu certyfikatu, należy przekazać `.cer` pliku Azure przy użyciu akcji "Przekazywania" "ustawienia" [portal Azure klasyczny][management-portal].

Po uzyskaniu Identyfikatora subskrypcji utworzono certyfikat i przekazać `.cer` pliku Azure, można nawiązać punkt końcowy Azure zarządzania przekazując identyfikator subskrypcji i lokalizację certyfikatu w magazynie certyfikatów **osobistych** do **ServiceManagementService** (ponownie, Zamień *AzureCertificate* nazwę certyfikatu):

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = 'CURRENT_USER\\my\\AzureCertificate'

    sms = ServiceManagementService(subscription_id, certificate_path)

W powyższym przykładzie `sms` jest obiektem **ServiceManagementService** . Klasa **ServiceManagementService** jest klasą podstawowego umożliwia zarządzanie usługami Azure.

## <a name="ListAvailableLocations"> </a>Jak: Lista dostępnych lokalizacji

Aby wyświetlić listę lokalizacji, które są dostępne dla usługi obsługi, użyj **listy\_lokalizacje** metody:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_locations()
    for location in result:
        print(location.name)

Podczas tworzenia usługi w chmurze lub Usługa magazynu należy podać prawidłową lokalizację. **Lista\_lokalizacje** metoda zawsze zwraca aktualną listę obecnie dostępne lokalizacje. Z tego artykułu są dostępne lokalizacje:

- Europa Zachodnia
- Europa Północna
- Kraje Azji Południowo
- Wschodnioazjatyckie
- Centralny Stany Zjednoczone
- Ameryka Północna centralnej w Stanach Zjednoczonych
- Południowej Niemetryczne centralnej
- Zachód Stany Zjednoczone
- Wschodniej Stany Zjednoczone
- Wschód Japonia
- Japonia zachód
- Brazylia Południowej
- Wschód Australia
- Australia kopiec

## <a name="CreateCloudService"> </a>Jak: Tworzenie usługi w chmurze

Podczas tworzenia aplikacji i uruchamianie jej w Azure kod i konfiguracji razem są nazywane Azure [usługi w chmurze][] (nazywanego *hostowana usługa* w starszych wersjach Azure). **Tworzenie\_hostowanej\_usługi** metoda umożliwia tworzenie nowej usługi hostowanej, dostarczając nazwę usług hostingowych (który musi być unikatowa w Azure), etykiety (automatycznie zakodowany base64), opis i lokalizację.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myhostedservice'
    label = 'myhostedservice'
    desc = 'my hosted service'
    location = 'West US'

    sms.create_hosted_service(name, label, desc, location)

Można wyświetlić listę wszystkich obsługiwanych usług dla subskrypcji z **listy\_hostowanej\_usług** metody:

    result = sms.list_hosted_services()

    for hosted_service in result:
        print('Service name: ' + hosted_service.service_name)
        print('Management URL: ' + hosted_service.url)
        print('Location: ' + hosted_service.hosted_service_properties.location)
        print('')

Jeśli chcesz uzyskać informacje dotyczące określonych usług hostingowych, możesz to zrobić przekazując nazwę usług hostingowych **uzyskać\_hostowanej\_usługi\_właściwości** metody:

    hosted_service = sms.get_hosted_service_properties('myhostedservice')

    print('Service name: ' + hosted_service.service_name)
    print('Management URL: ' + hosted_service.url)
    print('Location: ' + hosted_service.hosted_service_properties.location)

Po utworzeniu usługi w chmurze wdrożeniem kodu z usługą z **Tworzenie\_wdrożenia** metody.

## <a name="DeleteCloudService"> </a>Jak: usuwanie usługi w chmurze

Możesz usunąć usługi w chmurze, przekazując nazwy usługi na **Usuwanie\_hostowanej\_usługi** metody:

    sms.delete_hosted_service('myhostedservice')

Przed usunięciem usługi muszą zostać usunięte wszystkie wdrożenia usługi. (Zobacz [jak: usuwanie wdrożeniu](#DeleteDeployment) Aby uzyskać szczegółowe informacje.)

## <a name="DeleteDeployment"> </a>Jak: usuwanie wdrożeniu

Aby usunąć wdrożeniu, użyj **Usuwanie\_wdrożenia** metody. W poniższym przykładzie pokazano, jak usunąć wdrożeniu o nazwie `v1`.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment('myhostedservice', 'v1')

## <a name="CreateStorageService"> </a>Jak: tworzenie Usługa magazynu

[Usługa magazynu](../storage/storage-create-storage-account.md) umożliwia dostęp do Azure [obiektów blob](../storage/storage-python-how-to-use-blob-storage.md), [tabele](../storage/storage-python-how-to-use-table-storage.md)i [kolejek](../storage/storage-python-how-to-use-queue-storage.md). Aby utworzyć usługa magazynu, należy nazwę usługi (pomiędzy 3 a 24 wielkich liter i unikatowa w Azure), opis, etykiety (maksymalnie 100 znaków automatycznie kodowane do formatu base64) i lokalizacji. W poniższym przykładzie pokazano, jak utworzyć usługa magazynu określając lokalizację.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'mystorageaccount'
    label = 'mystorageaccount'
    location = 'West US'
    desc = 'My storage account description.'

    result = sms.create_storage_account(name, desc, label, location=location)

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

Notatki w poprzednim przykładzie którego stan **Tworzenie\_miejsca do magazynowania\_konta** operacji mogą być pobierane przekazując wynik zwrócony przez **Tworzenie\_miejsca do magazynowania\_konta** do **uzyskać\_operacji\_stanu** metody.  

Można wyświetlić listę kont miejsca do magazynowania i ich właściwości z **listy\_miejsca do magazynowania\_kont** metody:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_storage_accounts()
    for account in result:
        print('Service name: ' + account.service_name)
        print('Location: ' + account.storage_service_properties.location)
        print('')

## <a name="DeleteStorageService"> </a>Jak: usuwanie usługi miejsca do magazynowania

Usługa magazynu można usunąć przekazując nazwy usługi miejsca do magazynowania na **Usuwanie\_miejsca do magazynowania\_konta** metody. Usunięcie usługi Magazyn powoduje usunięcie wszystkich danych przechowywanych w usłudze (BLOB, tabele i kolejek).

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_storage_account('mystorageaccount')

## <a name="ListOperatingSystems"> </a>Jak: Lista dostępnych systemów operacyjnych

Aby wyświetlić listę systemów operacyjnych, które są dostępne dla usługi obsługi, użyj **listy\_operacyjnym\_systemów** metody:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_operating_systems()

    for os in result:
        print('OS: ' + os.label)
        print('Family: ' + os.family_label)
        print('Active: ' + str(os.is_active))

Opcjonalnie możesz użyć **listy\_operacyjnym\_systemu\_rodziny** metodę, która grupy Rodzina systemów operacyjnych:

    result = sms.list_operating_system_families()

    for family in result:
        print('Family: ' + family.label)
        for os in family.operating_systems:
            if os.is_active:
                print('OS: ' + os.label)
                print('Version: ' + os.version)
        print('')

## <a name="CreateVMImage"> </a>Jak: Tworzenie obrazu systemu operacyjnego

Aby dodać obraz systemu operacyjnego do repozytorium obraz, użyj **Dodawanie\_os\_obraz** metody:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'mycentos'
    label = 'mycentos'
    os = 'Linux' # Linux or Windows
    media_link = 'url_to_storage_blob_for_source_image_vhd'

    result = sms.add_os_image(label, media_link, name, os)

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

Aby wyświetlić listę obrazów systemu operacyjnego, które są dostępne, należy użyć **listy\_os\_obrazów** metody. Zawiera wszystkie obrazy platformy i obrazy użytkownika:

    result = sms.list_os_images()

    for image in result:
        print('Name: ' + image.name)
        print('Label: ' + image.label)
        print('OS: ' + image.os)
        print('Category: ' + image.category)
        print('Description: ' + image.description)
        print('Location: ' + image.location)
        print('Media link: ' + image.media_link)
        print('')

## <a name="DeleteVMImage"> </a>Jak: usuwanie obrazu systemu operacyjnego

Aby usunąć obraz użytkownika, użyj **Usuwanie\_os\_obraz** metody:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.delete_os_image('mycentos')

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

## <a name="CreateVM"> </a>Jak: tworzenie maszyny wirtualnej

Aby utworzyć maszyny wirtualnej, należy najpierw utworzyć [Usługa w chmurze](#CreateCloudService).  Następnie utwórz przy użyciu wdrażania maszyn wirtualnych **Tworzenie\_virtual\_komputera\_wdrożenia** metody:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myvm'
    location = 'West US'

    #Set the location
    sms.create_hosted_service(service_name=name,
        label=name,
        location=location)

    # Name of an os image as returned by list_os_images
    image_name = 'OpenLogic__OpenLogic-CentOS-62-20120531-en-us-30GB.vhd'

    # Destination storage account container/blob where the VM disk
    # will be created
    media_link = 'url_to_target_storage_blob_for_vm_hd'

    # Linux VM configuration, you can use WindowsConfigurationSet
    # for a Windows VM instead
    linux_config = LinuxConfigurationSet('myhostname', 'myuser', 'mypassword', True)

    os_hd = OSVirtualHardDisk(image_name, media_link)

    sms.create_virtual_machine_deployment(service_name=name,
        deployment_name=name,
        deployment_slot='production',
        label=name,
        role_name=name,
        system_config=linux_config,
        os_virtual_hard_disk=os_hd,
        role_size='Small')

## <a name="DeleteVM"> </a>Jak: usuwanie maszyny wirtualnej

Aby usunąć maszyny wirtualnej, należy najpierw usunąć za pomocą wdrożenia **Usuwanie\_wdrożenia** metody:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment(service_name='myvm',
        deployment_name='myvm')

Następnie można usunąć usługi w chmurze za pomocą **Usuwanie\_hostowanej\_usługi** metody:

    sms.delete_hosted_service(service_name='myvm')

##<a name="how-to-create-a-virtual-machine-from-a-captured-virtual-machine-image"></a>Jak: Tworzenie maszyny wirtualnej z obrazu przechwycony maszyn wirtualnych

Aby przechwycić obraz maszyn wirtualnych, zadzwonić najpierw **Przechwytywanie\_maszyn wirtualnych\_obraz** metody:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    # replace the below three parameters with actual values
    hosted_service_name = 'hs1'
    deployment_name = 'dep1'
    vm_name = 'vm1'

    image_name = vm_name + 'image'
    image = CaptureRoleAsVMImage    ('Specialized',
        image_name,
        image_name + 'label',
        image_name + 'description',
        'english',
        'mygroup')

    result = sms.capture_vm_image(
            hosted_service_name,
            deployment_name,
            vm_name,
            image
        )

Następnie, aby upewnić się, że pomyślnie przechwycone obrazu, należy użyć **listy\_maszyn wirtualnych\_obrazów** api i upewnij się, że obraz jest wyświetlany w wynikach:

    images = sms.list_vm_images()

Aby utworzyć na końcu maszyny wirtualnej za pomocą przechwycony obraz, użyj **Tworzenie\_wirtualną\_machine\_wdrożenia** metoda przed, ale tym razem przekazać vm_image_name zamiast tego

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myvm'
    location = 'West US'

    #Set the location
    sms.create_hosted_service(service_name=name,
        label=name,
        location=location)

    sms.create_virtual_machine_deployment(service_name=name,
        deployment_name=name,
        deployment_slot='production',
        label=name,
        role_name=name,
        system_config=linux_config,
        os_virtual_hard_disk=None,
        role_size='Small',
        vm_image_name = image_name)

Aby dowiedzieć się więcej na temat Przechwytywanie maszyny wirtualnej Linux, zobacz [Jak przechwytywać maszyny wirtualnej Linux.](../virtual-machines/virtual-machines-linux-classic-capture-image.md)

Aby dowiedzieć się więcej na temat Przechwytywanie maszyny wirtualnej systemu Windows, zobacz [Jak przechwytywać maszyny wirtualnej systemu Windows.](../virtual-machines/virtual-machines-windows-classic-capture-image.md)

## <a name="What's Next"> </a>Następne kroki

Teraz, gdy znasz już podstawy zarządzania usługą, można uzyskać dostęp do [dokumentów referencyjnych kompletny interfejs API dla zestawu SDK Python Azure](http://azure-sdk-for-python.readthedocs.org/) i wykonywanie złożonych zadań łatwe do zarządzania aplikacją python.

Aby uzyskać więcej informacji zobacz [Centrum deweloperów Python](/develop/python/).

[What is Service Management]: #WhatIs
[Concepts]: #Concepts
[How to: Connect to service management]: #Connect
[How to: List available locations]: #ListAvailableLocations
[How to: Create a cloud service]: #CreateCloudService
[How to: Delete a cloud service]: #DeleteCloudService
[How to: Create a deployment]: #CreateDeployment
[How to: Update a deployment]: #UpdateDeployment
[How to: Move deployments between staging and production]: #MoveDeployments
[How to: Delete a deployment]: #DeleteDeployment
[How to: Create a storage service]: #CreateStorageService
[How to: Delete a storage service]: #DeleteStorageService
[How to: List available operating systems]: #ListOperatingSystems
[How to: Create an operating system image]: #CreateVMImage
[How to: Delete an operating system image]: #DeleteVMImage
[How to: Create a virtual machine]: #CreateVM
[How to: Delete a virtual machine]: #DeleteVM
[Next Steps]: #NextSteps
[management-portal]: https://manage.windowsazure.com/
[svc-mgmt-rest-api]: http://msdn.microsoft.com/library/windowsazure/ee460799.aspx


[Usługa w chmurze]:/services/cloud-services/

