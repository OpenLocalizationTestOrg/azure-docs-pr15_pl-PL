<properties
    pageTitle="Konfigurowanie DHCPv6 maszyny wirtualne Linux | Microsoft Azure"
    description="Jak skonfigurować DHCPv6 dla maszyny wirtualne Linux."
    services="load-balancer"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
    keywords="Protokół IPv6, równoważenia obciążenia azure, podwójne stos, publiczny adres ip, natywny protokół ipv6, mobile, iot"
/>
<tags
    ms.service="load-balancer"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/14/2016"
    ms.author="sewhee"
/>

# <a name="configuring-dhcpv6-for-linux-vms"></a>Konfigurowanie DHCPv6 Linux maszyny wirtualne

Niektóre obrazy maszyn wirtualnych Linux Azure Marketplace nie mają domyślnie skonfigurowane DHCPv6. Do obsługi protokołu IPv6, należy skonfigurować DHCPv6 w w rozkład Linux OS używanego. Różne dystrybucji Linux mają różne sposoby konfigurowania DHCPv6, ponieważ używają różnych pakietów.

>[AZURE.NOTE] Ostatnie obrazy SUSE Linux i CoreOS w Azure Marketplace zostały wstępnie skonfigurowane z DHCPv6. Podczas korzystania z tych obrazów są wymagane nie dodatkowe zmiany.

Ten dokument opisano, jak włączyć DHCPv6, dzięki czemu komputer wirtualnych Linux uzyskuje adres IPv6.

>[AZURE.WARNING] Nieprawidłowo edytowania plików konfiguracji sieci mogą powodować utracisz dostęp do sieci do usługi maszyn wirtualnych. Zaleca się sprawdzenie zmiany w konfiguracji w systemach innych niż produkcyjnych. Z instrukcjami podanymi w tym artykule przetestowano na najnowsze wersje obrazy Linux Azure Marketplace. Zapoznaj się z dokumentacją dla wersję Linux bardziej szczegółowe instrukcje.

## <a name="ubuntu"></a>Ubuntu

1. Edytowanie pliku `/etc/dhcp/dhclient6.conf` i Dodaj poniższy wiersz:

        timeout 10;

2. Edytowanie konfiguracji sieci interfejsu eth0 o następującej konfiguracji:

    * Na **Ubuntu 12.04 i 14.04**edytować plik`/etc/network/interfaces.d/eth0.cfg`
    * Na **Ubuntu 16.04**edytować plik`/etc/network/interfaces.d/50-cloud-init.cfg`

    ```bash
    iface eth0 inet6 auto
        up sleep 5
        up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true
    ```

3. Odnowić adres IPv6:

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="debian"></a>Debian

1. Edytowanie pliku `/etc/dhcp/dhclient6.conf` i Dodaj poniższy wiersz:

        timeout 10;

2. Edytowanie pliku `/etc/network/interfaces` i dodaj następujące:

        iface eth0 inet6 auto
            up sleep 5
            up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true

3. Odnowić adres IPv6:

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="rhel--centos--oracle-linux"></a>RHEL-CentOS i Oracle Linux

1. Edytowanie pliku `/etc/sysconfig/network` i Dodaj następujący parametr:

        NETWORKING_IPV6=yes

2. Edytowanie pliku `/etc/sysconfig/network-scripts/ifcfg-eth0` i dodaj następujące dwa parametry:

        IPV6INIT=yes
        DHCPV6C=yes

3. Odnowić adres IPv6:

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-11--opensuse-13"></a>SLES 11 & openSUSE 13

Ostatnie obrazy SLES i openSUSE platformy Azure zostały wstępnie skonfigurowane z DHCPv6. Podczas korzystania z tych obrazów są wymagane nie dodatkowe zmiany. Jeśli masz maszyny na podstawie obrazu SUSE starszych lub niestandardowy, wykonaj następujące czynności:

1. Instalowanie `dhcp-client` pakiet, w razie potrzeby:

    ```bash
    # sudo zypper install dhcp-client
    ```

2. Edytowanie pliku `/etc/sysconfig/network/ifcfg-eth0` i Dodaj następujący parametr:

        DHCLIENT6_MODE='managed'

3. Odnowić adres IPv6:

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-12-and-opensuse-leap"></a>SLES 12 i openSUSE przestępnych

Ostatnie obrazy SLES i openSUSE platformy Azure zostały wstępnie skonfigurowane z DHCPv6. Podczas korzystania z tych obrazów są wymagane nie dodatkowe zmiany. Jeśli masz maszyny na podstawie obrazu SUSE starszych lub niestandardowy, wykonaj następujące czynności:

1. Edytowanie pliku `/etc/sysconfig/network/ifcfg-eth0` i Zamień ten parametr

        #BOOTPROTO='dhcp4'

    z następującą wartość:

        BOOTPROTO='dhcp'

2. Dodaj następujący parametr do `/etc/sysconfig/network/ifcfg-eth0`:

        DHCLIENT6_MODE='managed'

3. Odnowić adres IPv6:

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="coreos"></a>CoreOS

Ostatnie obrazy CoreOS Azure zostały wstępnie skonfigurowane z DHCPv6. Podczas korzystania z tych obrazów są wymagane nie dodatkowe zmiany. Jeśli masz maszyny na podstawie obrazu CoreOS starszych lub niestandardowy, wykonaj następujące czynności:

1. Edytowanie pliku`/etc/systemd/network/10_dhcp.network`

        [Match]
        eth0

        [Network]
        DHCP=ipv6

2. Odnowić adres IPv6:

    ```bash
    # sudo systemctl restart systemd-networkd
    ```
