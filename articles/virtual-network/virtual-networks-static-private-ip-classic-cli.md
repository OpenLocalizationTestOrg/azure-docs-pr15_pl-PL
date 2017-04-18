<properties 
   pageTitle="Jak skonfigurować statyczny adres IP prywatne w trybie klasycznym ausing polecenie | Microsoft Azure"
   description="Opis prywatnych statyczne adresy IP (spadku) oraz jak nimi zarządzać w trybie klasycznym za pomocą interfejsu wiersza polecenia"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-service-management"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="how-to-set-a-static-private-ip-address-classic-in-azure-cli"></a>Jak ustawić prywatne statycznego adresu IP (klasyczny) w polecenie Azure

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]W tym artykule opisano, jak model klasyczny wdrożenia. Można również [zarządzać statycznego adresu IP prywatne w modelu wdrożenia Menedżera zasobów](virtual-networks-static-private-ip-arm-cli.md).

Poniższe przykładowe polecenie Azure polecenia przewiduje środowisku prosty już utworzone. Jeśli chcesz uruchomić polecenia wyświetlaną w tym dokumencie, należy najpierw utworzyć środowisku testowym opisanego w [Tworzenie vnet](virtual-networks-create-vnet-classic-cli.md).

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>Jak określić statyczny adres IP prywatne podczas tworzenia maszyn wirtualnych
Aby utworzyć nowe maszyny o nazwie *DNS01* w nowej usługi w chmurze o nazwie *TestService* według tego scenariusza powyżej, wykonaj następujące kroki:

1. Jeśli po raz pierwszy używasz polecenie Azure, zobacz [Instalowanie i konfigurowanie polecenie Azure](../xplat-cli-install.md) i postępuj zgodnie z instrukcjami do miejsca, w którym wybierz swoje konto Azure i subskrypcji.
1. Uruchom polecenie **tworzenia usługi azure** , aby utworzyć usługa w chmurze.

        azure service create TestService --location uscentral

    Oczekiwany wynik:

        info:    Executing command service create
        info:    Creating cloud service
        data:    Cloud service name TestService
        info:    service create command OK
    
2. Uruchom polecenie **azure Tworzenie maszyn wirtualnych** , aby utworzyć maszyn wirtualnych. Zwróć uwagę, wartość dla statycznego adresu IP prywatne. Lista wyświetlana po wynik wyjaśniono parametry używane.

        azure vm create -l centralus -n DNS01 -w TestVNet -S "192.168.1.101" TestService bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2 adminuser AdminP@ssw0rd

    Oczekiwany wynik:

        info:    Executing command vm create
        warn:    --vm-size has not been specified. Defaulting to "Small".
        info:    Looking up image bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2
        info:    Looking up virtual network
        info:    Looking up cloud service
        warn:    --location option will be ignored
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Retrieving storage accounts
        info:    Creating VM
        info:    OK
        info:    vm create command OK

    - **-l (lub — lokalizacja)**. Azure region, w której zostanie utworzony maszyn wirtualnych. W naszym scenariuszu *centralus*.
    - **-n (lub nazwa — maszyn wirtualnych)**. Nazwa maszyn wirtualnych do utworzenia.
    - **-w (lub — nazwy wirtualnej sieci)**. Nazwa VNet, w której zostanie utworzony maszyn wirtualnych. 
    - **-S (lub — statyczne ip)**. Prywatne adresem IP dla maszyn wirtualnych.
    - **TestService**. Nazwa miejsce, w którym będą tworzone maszyn wirtualnych usługi w chmurze.
    - **bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2**. Obraz używany do tworzenia maszyn wirtualnych.
    - **adminuser**. Lokalne administrator maszyn wirtualnych systemu Windows.
    - **AdminP@ssw0rd**. Hasło administratora lokalnego maszyn wirtualnych systemu Windows.

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Jak pobrać statyczne prywatne informacje o adresie IP dla maszyn wirtualnych
Aby wyświetlić statyczną prywatne informacje o adresie IP dla maszyn wirtualnych, utworzone za pomocą skryptu powyżej, uruchom następujące polecenie, polecenie Azure i obserwować wartość dla *Sieci StaticIP*:

    azure vm static-ip show DNS01

Oczekiwany wynik:

    info:    Executing command vm static-ip show
    info:    Getting virtual machines
    data:    Network StaticIP "192.168.1.101"
    info:    vm static-ip show command OK

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Jak usunąć statycznego adresu IP prywatne z maszyny
Aby usunąć statyczny adres IP prywatne dodane do maszyn wirtualnych w skrypt powyżej, uruchom następujące polecenie, polecenie Azure:
    
    azure vm static-ip remove DNS01

Oczekiwany wynik:

    info:    Executing command vm static-ip remove
    info:    Getting virtual machines
    info:    Reading network configuration
    info:    Updating network configuration
    info:    vm static-ip remove command OK

## <a name="how-to-add-a-static-private-ip-to-an-existing-vm"></a>Jak dodać statyczny adres IP prywatne do istniejącego maszyn wirtualnych
Aby dodać statycznego prywatny adres IP maszyn wirtualnych, utworzone za pomocą skryptu powyżej runt on następującego polecenia:

    azure vm static-ip set DNS01 192.168.1.101

Oczekiwany wynik:

    info:    Executing command vm static-ip set
    info:    Getting virtual machines
    info:    Looking up virtual network
    info:    Reading network configuration
    info:    Updating network configuration
    info:    vm static-ip set command OK

## <a name="next-steps"></a>Następne kroki

- Informacje na temat zastrzeżone publiczne adresy [IP](virtual-networks-reserved-public-ip.md) .
- Informacje na temat adresów [wystąpienie poziomu publicznych adresów IP (ILPIP)](virtual-networks-instance-level-public-ip.md) .
- Zwróć się [interfejsy API pozostałych zastrzeżony adres IP](https://msdn.microsoft.com/library/azure/dn722420.aspx).
