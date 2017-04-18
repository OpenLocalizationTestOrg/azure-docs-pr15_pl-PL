<properties
   pageTitle="Utworzyć klaster autonomicznego o maszyny wirtualne Azure z systemem Windows | Microsoft Azure"
   description="Dowiedz się, jak tworzyć i zarządzać nimi klaster tkaninie usługi Azure w przypadku Azure maszyn wirtualnych z systemem Windows Server."
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
   ms.date="08/05/2016"
   ms.author="dkshir;chackdan"/>



# <a name="create-a-three-node-standalone-service-fabric-cluster-with-azure-virtual-machines-running-windows-server"></a>Utworzyć klaster trzy węzeł autonomiczny tkaninie usługi Azure maszyn wirtualnych z systemem Windows Server

W tym artykule opisano, jak utworzyć klaster na systemu Windows Azure wirtualnych maszyn, za pomocą Instalatora usługi tkaninie autonomicznego dla systemu Windows Server. Jest to szczególny przypadek [Tworzenie i zarządzanie nimi klastrze z systemem Windows Server](service-fabric-cluster-creation-for-windows-server.md) maszyny wirtualne umiejscowienia [Maszyny wirtualne Azure z systemem Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md), ale nie tworzysz [Azure klaster tkaninie usługi opartej na chmurze](service-fabric-cluster-creation-via-portal.md). Różnica polega na klaster tkaninie usługi autonomicznej utworzone przez następujące czynności całkowicie odbywa się przez Ciebie, podczas Azure klastrów tkaninie usługi opartej na chmurze są zarządzane i uaktualnić przez dostawcę usługi tkaninie zasobów.


## <a name="steps-to-create-the-standalone-cluster"></a>Procedura tworzenia klaster autonomicznego

1. Zaloguj się do portalu Azure i Utwórz nowe systemu Windows Server 2012 R2 centrum danych maszyn wirtualnych w grupie zasobów. Przeczytaj artykuł [Tworzenie maszyn wirtualnych systemu Windows w portalu Azure](../virtual-machines/virtual-machines-windows-hero-tutorial.md) , aby uzyskać więcej informacji.
2. Dodawanie kilku więcej systemu Windows Server 2012 R2 centrum danych maszyny wirtualne do tej samej grupy zasobów. Upewnij się, że każda maszyny wirtualne zawiera tę samą nazwę użytkownika administratora i hasło podczas tworzenia. Raz utworzony powinny być widoczne wszystkie trzy maszyny wirtualne w tej samej sieci wirtualnej.
3. Nawiązywanie połączenia z każdego z pośrednictwem SMS i wyłączanie Zapory systemu Windows za pomocą [Menedżera serwera, lokalnego serwera pulpitu nawigacyjnego](https://technet.microsoft.com/library/jj134147.aspx). Dzięki temu, że ruch sieciowy mogą się komunikować między komputerami. Podczas połączenia z każdym komputerze, uzyskać adres IP, otwierając wiersz polecenia i wpisując `ipconfig`. Można też kliknąć widać adres IP każdego komputera, wybierając pozycję zasobu wirtualną sieć dla grupy zasobów w portalu Azure.
4. Nawiązywanie połączenia z jedną z pośrednictwem SMS i sprawdzić, czy możesz pomyślnie ping innych dwóch maszyny wirtualne.
5. Nawiązywanie połączenia z jedną z pośrednictwem SMS i [Pobierz pakiet autonomiczny tkaninie usługi dla systemu Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) do nowego folderu na komputerze i Wyodrębnij pakiet.
6. Otwórz plik *ClusterConfig.Unsecure.MultiMachine.json* w Notatniku i edytować każdy węzeł trzy adresy IP maszyn. Zmienianie nazwy klaster u góry i Zapisz plik.  Poniżej przedstawiono częściowego przykład manifestu klaster.

    ```
    {
        "name": "TestCluster",
        "clusterManifestVersion": "1.0.0",
        "apiVersion": "2015-01-01-alpha",
        "nodes": [
        {
            "nodeName": "vm0",
            "metadata": "Replace the localhost with valid IP address or FQDN below",
            "iPAddress": "10.7.0.5",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc1/r0",
            "upgradeDomain": "UD0"
        },
        {
            "nodeName": "vm1",
            "metadata": "Replace the localhost with valid IP address or FQDN below",
            "iPAddress": "10.7.0.4",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc2/r0",
            "upgradeDomain": "UD1"
        },
        {
            "nodeName": "vm2",
            "metadata": "Replace the localhost with valid IP address or FQDN below",
            "iPAddress": "10.7.0.6",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc3/r0",
            "upgradeDomain": "UD2"
        }
    ],
    ```

7. Otwieranie [okna środowiska PowerShell ISE](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise). Przejdź do folderu, w którym wyodrębnionych autonomicznego pobrany pakiet Instalatora i zapisano plik manifestu klaster. Uruchom następujące polecenia programu PowerShell.

    ```
    .\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json -MicrosoftServiceFabricCabFilePath .\MicrosoftAzureServiceFabric.cab
    ```

8. Powinien zostać wyświetlony programu PowerShell, uruchamianie, połączyć się z każdym komputerem i utworzyć klaster. Po około minuty, możesz sprawdzić, jeśli klaster działa przez nawiązanie połączenia z Eksploratora tkaninie usługi na jednym z adresem IP komputera np. za pomocą `http://10.7.0.5:19080/Explorer/index.html`. Ponieważ jest to klaster autonomicznej za pomocą maszyny wirtualne Azure, aby zabezpieczyć będzie trzeba [wdrażać certyfikatów do maszyny wirtualne Azure](service-fabric-windows-cluster-x509-security.md) lub skonfigurować jedną maszyn jako [administrator systemu Windows Server Active Directory (AD) dla uwierzytelniania systemu Windows](service-fabric-windows-cluster-windows-security.md), tak jak robisz w swojej siedzibie.


## <a name="next-steps"></a>Następne kroki
- [Tworzenie autonomicznego tkaninie usługi klastrów na serwerze systemu Windows i Linux oraz](service-fabric-deploy-anywhere.md)
- [Dodawanie lub usuwanie węzły do klastrów tkaninie usługi autonomicznego](service-fabric-cluster-windows-server-add-remove-nodes.md)
- [Ustawienia konfiguracji autonomicznej klaster systemu Windows](service-fabric-cluster-manifest.md)
- [Zabezpieczanie klastrze autonomicznego w systemie Windows przy użyciu zabezpieczeń systemu Windows](service-fabric-windows-cluster-windows-security.md)
- [Zabezpieczanie klastrze autonomicznego w systemie Windows przy użyciu X509 certyfikatów](service-fabric-windows-cluster-x509-security.md)
