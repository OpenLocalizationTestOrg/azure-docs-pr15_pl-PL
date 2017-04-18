<properties
   pageTitle="Dodawanie lub usuwanie węzły do klastrów tkaninie usługi autonomicznej | Microsoft Azure"
   description="Dowiedz się, jak dodać lub usunąć węzły z klastrem tkaninie usługi Azure fizycznej lub maszyn wirtualnych z systemem Windows Server, które mogą być lokalnego lub w chmurze dowolny."
   services="service-fabric"
   documentationCenter=".net"
   authors="dsk-2015"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/20/2016"
   ms.author="dkshir;chackdan"/>


# <a name="add-or-remove-nodes-to-a-standalone-service-fabric-cluster-running-on-windows-server"></a>Dodawanie lub usuwanie węzły do klastrów tkaninie usługi autonomicznej działa w systemie Windows Server

Po utworzeniu [klaster tkaninie usługi autonomicznej na komputerach z systemem Windows Server](service-fabric-cluster-creation-for-windows-server.md)potrzeb firmy mogą się zmienić tak, aby dodać lub usunąć wiele węzłów na klaster może być konieczne. W tym artykule opisano szczegółowe kroki, aby osiągnąć ten cel.


## <a name="add-nodes-to-your-cluster"></a>Dodawanie węzłów na klaster

1. Przygotuj maszyn wirtualnych komputera ma zostać dodany do klaster przez wykonanie kroków wymienionych w sekcji [Przygotowywanie komputerach, aby spełnić wymagania wstępne dotyczące wdrażania klastrów](service-fabric-cluster-creation-for-windows-server.md#preparemachines) .
2. Planowanie, które domeny błędów i uaktualniania domeny zamierzasz dodać maszyn wirtualnych komputera, tak aby.
3. Pulpit zdalny (RDP) do maszyn wirtualnych komputera, do którego chcesz dodać do klaster.
4. Kopiowanie lub [Pobierz autonomicznego tkaninie usługi dla systemu Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) do tego komputera i maszyn wirtualnych i rozpakuj plik pakietu.
5. Uruchamianie programu Powershell jako administrator, a następnie przejdź do lokalizacji rozpakowanym pakietem.
6. Uruchamianie programu Powershell *AddNode.ps1* z parametrami opisujący węzeł nowy, aby dodać. W poniższym przykładzie dodaje nowy węzeł o nazwie VM5, typ NodeType0, adres IP 182.17.34.52 do UD1 i FD1. *ExistingClusterConnectionEndPoint* jest punkt końcowy połączenia dla węzła już w istniejącym klastrem. Dla tego punktu końcowego można określić adres IP *dowolnego* węzła w klastrze.

```
.\AddNode.ps1 -NodeName VM5 -NodeType NodeType0 -NodeIPAddressorFQDN 182.17.34.52 -ExistingClientConnectionEndpoint 182.17.34.50:19000 -UpgradeDomain UD1 -FaultDomain FD1 -AcceptEULA

```

## <a name="remove-nodes-from-your-cluster"></a>Usuwanie węzłów klaster

1. W zależności od poziomu Reliablity wybrany dla klaster nie można usunąć pierwszej n (3-5-7-9) węzły typu główny węzeł
2. Uruchamianie polecenia RemoveNode w klastrze deweloperów nie jest obsługiwane.
2. Pulpit zdalny (RDP) do maszyn wirtualnych komputera, który chcesz usunąć z klastrem.
2. Kopiowanie lub [pobrać pakiet autonomiczny tkaninie usługi w systemie Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) i rozpakuj plik pakietu do tego maszyn wirtualnych i komputera.
3. Uruchamianie programu Powershell jako administrator, a następnie przejdź do lokalizacji rozpakowanym pakietem.
4. Uruchom *RemoveNode.ps1* w programie PowerShell. W poniższym przykładzie powoduje usunięcie bieżącego węzła z klastrem. *ExistingClientConnectionEndpoint* jest punkt końcowy połączenia klienta dla każdego węzła pozostanie w klastrze. Wybierz adres IP i port punktu końcowego *dowolnego* **innego węzła** w klastrze. Ten **drugi węzeł** z kolei zaktualizuje konfiguracji klaster węzeł usunięte. 

```
.\RemoveNode.ps1 -ExistingClientConnectionEndpoint 182.17.34.50:19000
```

Pamiętaj, że nawet po usunięciu węzeł, może być widoczna jako w dół w kwerendach i SFX. To jest znane wady i zostanie ustawiony w nowej wersji. 


## <a name="next-steps"></a>Następne kroki
- [Ustawienia konfiguracji autonomicznej klaster systemu Windows](service-fabric-cluster-manifest.md)
- [Zabezpieczanie klastrze autonomicznego w systemie Windows przy użyciu zabezpieczeń systemu Windows](service-fabric-windows-cluster-windows-security.md)
- [Zabezpieczanie klastrze autonomicznego w systemie Windows przy użyciu X509 certyfikatów](service-fabric-windows-cluster-x509-security.md)
- [Tworzenie klastrze tkaninie usługi autonomicznej z maszyny wirtualne Azure z systemem Windows](service-fabric-cluster-creation-with-windows-azure-vms.md)
