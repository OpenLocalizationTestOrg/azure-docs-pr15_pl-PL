<properties
   pageTitle="Aby zarejestrować nazwy hostów za pomocą dynamicznego DNS"
   description="Ta strona zawiera szczegóły na temat konfigurowania dynamicznego DNS, aby zarejestrować nazwy hostów w serwerach DNS."
   services="dns"
   documentationCenter="na"
   authors="GarethBradshawMSFT"
   manager="carmonm"
   editor="" />
<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="sewhee" />

# <a name="using-dynamic-dns-to-register-hostnames-in-your-own-dns-server"></a>Aby zarejestrować nazwy hostów w serwera DNS za pomocą dynamicznego DNS

[Azure udostępnia rozpoznawanie nazw](virtual-networks-name-resolution-for-vms-and-role-instances.md) dla wirtualnych maszyn i wystąpień roli. Jednak podczas rozdzielczość nazwa musi wykracza poza oferowane przez Azure, należy podać serwerów DNS. Uzyskasz możliwość Dostosuj rozwiązania DNS do własnych potrzeb określonych. Na przykład konieczne może uzyskać dostęp do zasobów lokalnych za pośrednictwem kontrolera domeny usługi Active Directory.

Gdy niestandardowych serwerów DNS są obsługiwane jako maszyny wirtualne Azure możesz przesyłać kwerendy hostname o tym samym vnet Azure, aby rozpoznawać nazwy hostów. Jeśli nie chcesz używać tą drogą, można zarejestrować swojej nazwy hostów maszyn wirtualnych w swojego serwera DNS za pomocą dynamicznego DNS.  Azure nie ma możliwości (na przykład poświadczeń) do utworzenia rekordów bezpośrednio na serwerach DNS, wymagane są często rozmieszczenia alternatywny. Poniżej przedstawiono kilka typowych scenariuszy z rozwiązania alternatywne.

## <a name="windows-clients"></a>Klienci systemu Windows

Klientów systemu Windows — sprzężone domeny podjąć próbę niezabezpieczoną aktualizacje dynamiczne DNS (DDNS) podczas ich uruchamiania lub zmiany jego adresu IP. Nazwa DNS jest nazwa hosta oraz podstawowy sufiks DNS. Azure pozostawia puste podstawowy sufiks DNS, ale można tak ustawić w Głosowa, za pośrednictwem [interfejsu użytkownika](https://technet.microsoft.com/library/cc794784.aspx) lub [przy użyciu automatyzacji](https://social.technet.microsoft.com/forums/windowsserver/3720415a-6a9a-4bca-aa2a-6df58a1a47d7/change-primary-dns-suffix).

Domeny klientów z systemem Windows rejestrować adresy IP kontrolera domeny przy użyciu bezpiecznego DNS dynamiczne. Proces Dołączanie domeny ustawia podstawowy sufiks DNS na kliencie i tworzy i obsługuje relacji zaufania.

## <a name="linux-clients"></a>Klienci Linux

Klienci Linux ogólnie nie rejestrować z serwera DNS po ponownym uruchomieniu komputera, założono, że oznacza serwerze. Serwery DHCP Azure osoby nie mają możliwości lub poświadczenia, aby zarejestrować rekordy w swojego serwera DNS.  Za pomocą narzędzia o nazwie *nsupdate*, który znajduje się w pakiecie powiązania, aby wysłać aktualizacje dynamiczne rekordów DNS. Ponieważ protokół dynamiczne DNS jest ustandaryzowane, można użyć *nsupdate* nawet wtedy, gdy nie używasz powiązania z serwera DNS.

Możesz użyć haki, wydawanych przez klienta DHCP do tworzenia i obsługi wpisie nazwa hosta serwera DNS. Cyklu DHCP klient wykonuje skrypty w */etc/dhcp/dhclient-exit-hooks.d/*. To może służyć do rejestrowania nowy adres IP przy użyciu *nsupdate*. Na przykład:

        #!/bin/sh
        requireddomain=mydomain.local

        # only execute on the primary nic
        if [ "$interface" != "eth0" ]
        then
            return
        fi

        # when we have a new IP, perform nsupdate
        if [ "$reason" = BOUND ] || [ "$reason" = RENEW ] ||
           [ "$reason" = REBIND ] || [ "$reason" = REBOOT ]
        then
            host=`hostname`
            nsupdatecmds=/var/tmp/nsupdatecmds
              echo "update delete $host.$requireddomain a" > $nsupdatecmds
              echo "update add $host.$requireddomain 3600 a $new_ip_address" >> $nsupdatecmds
              echo "send" >> $nsupdatecmds

              nsupdate $nsupdatecmds
        fi

        #done
        exit 0;

Za pomocą polecenia *nsupdate* do przeprowadzania aktualizacji zabezpieczeń dynamicznego DNS. Na przykład podczas korzystania z serwera DNS powiązać, parę kluczy publicznych i prywatnych jest [generowany](http://linux.yyz.us/nsupdate/).  Serwer DNS jest [skonfigurowana](http://linux.yyz.us/dns/ddns-server.html) publiczna część klucza, aby mogli zweryfikować podpis na żądanie. Zapewnienie, że para kluczy *nsupdate* w kolejności dynamiczne DNS zaktualizować żądanie do podpisania, należy użyć opcji *-k* .

Podczas korzystania z serwera DNS systemu Windows, umożliwia uwierzytelnianie Kerberos przy użyciu parametru *-g* w *nsupdate* (niedostępne w wersji systemu Windows *nsupdate*). W tym celu należy użyć *kinit* załadować poświadczeń (np. z [pliku keytab](http://www.itadmintools.com/2011/07/creating-kerberos-keytab-files.html)). Następnie *nsupdate g* użyje poświadczenia z pamięci podręcznej.

W razie potrzeby możesz dodać do swojej maszyny wirtualne sufiks wyszukiwania DNS. Sufiks DNS jest określony w pliku */etc/resolv.conf* . Większość distros Linux automatycznie zarządzać zawartość tego pliku, więc zwykle nie można edytować go. Jednak można zastąpić sufiks przy użyciu polecenia *zastępują* klienta DHCP. W tym celu w */etc/dhcp/dhclient.conf*należy dodać:

        supersede domain-name <required-dns-suffix>;

