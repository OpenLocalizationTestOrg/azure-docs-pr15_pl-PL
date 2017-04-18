<properties 
   pageTitle="Wyświetlanie i modyfikowanie nazwy hostów | Microsoft Azure"
   description="Wyświetlanie i zmienianie nazwy hostów dla Azure maszyn wirtualnych, sieci web i ról pracownika do rozpoznawania nazw"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/27/2016"
   ms.author="jdial" />

# <a name="viewing-and-modifying-hostnames"></a>Wyświetlanie i modyfikowanie nazwy hostów

Aby umożliwić odwoływać się nazwa hosta usługi wystąpień roli, należy ustawić wartość w polu Nazwa hosta w pliku konfiguracji usługi dla poszczególnych ról. Można to zrobić, dodając nazwę odpowiedniej hosta do atrybutu **vmName** elementu **roli** . Wartość atrybutu **vmName** jest używany jako podstawy dla nazwy hosta każde wystąpienie roli. Na przykład jeśli istnieją trzy wystąpienia tej roli **vmName** jest *webrole* , nazwy hostów wystąpienia będzie *webrole0*, *webrole1*i *webrole2*. Trzeba określić nazwę hosta maszyn wirtualnych w pliku konfiguracji, ponieważ nazwa hosta dla maszyny wirtualnej jest wypełnione na podstawie nazwy maszyn wirtualnych. Aby uzyskać więcej informacji o konfigurowaniu usługi Microsoft Azure zobacz [Schematu konfiguracji usługi Azure (.cscfg pliku)](https://msdn.microsoft.com/library/azure/ee758710.aspx)

## <a name="viewing-hostnames"></a>Wyświetlanie nazwy hostów

Możesz wyświetlać nazwy hostów maszyn wirtualnych i wystąpienia rola w usłudze w chmurze za pomocą dowolnej z poniższych narzędzi.

### <a name="azure-portal"></a>Azure Portal

[Azure portal](http://portal.azure.com) umożliwia wyświetlanie nazwy hosta dla maszyn wirtualnych na karta Przegląd dla maszyny wirtualnej. Należy pamiętać, że karta zawiera wartość dla **Nazwa** i **Nazwa hosta**. Chociaż początkowo są takie same, zmieniając nazwy hosta nie ulegnie zmianie nazwy maszyn wirtualnych lub wystąpienia roli.

Wystąpienia roli, można też wyświetlić w portalu Azure, ale po ponownym wystąpienia w usłudze w chmurze, nie jest wyświetlana nazwa hosta. Pojawi się nazwa dla każdego wystąpienia, ale tej nazwy nie reprezentuje nazwa hosta.

### <a name="service-configuration-file"></a>Plik konfiguracyjny usługi

Plik konfiguracyjny usługi wdrożonym usługi można pobrać z karta **Konfigurowanie** usługi w portalu Azure. Następnie można wyszukiwać atrybut **vmName** dla elementu **Nazwa roli** wyświetlić nazwa hosta. Należy pamiętać, że nazwa hosta jest używana jako podstawy dla nazwy hosta każde wystąpienie roli. Na przykład jeśli istnieją trzy wystąpienia tej roli **vmName** jest *webrole* , nazwy hostów wystąpienia będzie *webrole0*, *webrole1*i *webrole2*.

### <a name="remote-desktop"></a>Pulpit zdalny

Po włączeniu pulpitu zdalnego (Windows), zdalnych środowiska Windows PowerShell (Windows) lub połączenia SSH (Linux i systemu Windows) do maszyn wirtualnych lub wystąpienia roli, można wyświetlić nazwy hosta z aktywnego połączenia pulpitu zdalnego na różne sposoby:

- Nazwa hosta wpisz w wierszu polecenia lub SSH terminal.

- Wpisz polecenie ipconfig/wszystkie w wierszu polecenia użyj pozycji Monituj (tylko w systemie Windows).

- Wyświetlanie nazwy komputera ustawień systemu (tylko w systemie Windows).

### <a name="azure-service-management-rest-api"></a>Zarządzanie usługą Azure interfejsu API usługi REST

W kliencie pozostałych wykonaj następujące instrukcje:

1. Upewnij się, że masz certyfikat klienta, aby nawiązać połączenie Azure portal. Aby uzyskać certyfikat klienta, wykonaj kroki przedstawione w [jak: pobieranie i Importuj ustawienia publikowania i informacje o subskrypcji](https://msdn.microsoft.com/library/dn385850.aspx). 

1. Ustaw wpis nagłówka o nazwie x ms wersji z wartością 2013-11-01.

1. Wysyłanie zlecenia w następującym formacie: https://management.core.windows.net/\<subscrition identyfikator\>/usługi/hostedservices/\<nazwa usługi\>szczegóły ?embed = true

1. Znajdź element **HostName** dla każdego elementu **RoleInstance** .

>[AZURE.WARNING] Możesz także wyświetlić sufiks domeny wewnętrznej tej usługi cloud z pozostałych odpowiedzi połączenia, zaznaczając pole wyboru elementu **InternalDnsSuffix** lub za pomocą polecenia ipconfig/wszystko z wiersza polecenia w sesji pulpitu zdalnego (Windows) lub przez uruchomionego /etc/resolv.conf kotów z terminal SSH (Linux).

## <a name="modifying-a-hostname"></a>Modyfikowanie nazwa hosta

Nazwa hosta maszyn wirtualnych lub wystąpienia roli można modyfikować, przekazując pliku konfiguracji usługi zmienione lub zmieniając komputer z sesji pulpitu zdalnego.

## <a name="next-steps"></a>Następne kroki

[Rozpoznawanie nazw (DNS)](virtual-networks-name-resolution-for-vms-and-role-instances.md)

[Schemat konfiguracji usługi Azure (.cscfg)](https://msdn.microsoft.com/library/windowsazure/ee758710.aspx)

[Schemat konfiguracji Azure wirtualnej sieci](http://go.microsoft.com/fwlink/?LinkId=248093)

[Korzystanie z plików konfiguracji sieci ustawienia DNS.](virtual-networks-specifying-a-dns-settings-in-a-virtual-network-configuration-file.md)
