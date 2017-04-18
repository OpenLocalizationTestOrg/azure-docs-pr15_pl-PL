<properties
   pageTitle="Konfigurowanie klaster autonomicznego | Microsoft Azure"
   description="Ten artykuł zawiera opis sposobu konfigurowania usługi autonomicznej lub prywatnych klaster tkaninie usługi."
   services="service-fabric"
   documentationCenter=".net"
   authors="dsk-2015"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/07/2016"
   ms.author="dkshir"/>


# <a name="configuration-settings-for-standalone-windows-cluster"></a>Ustawienia konfiguracji autonomicznej klaster systemu Windows

Ten artykuł zawiera opis sposobu konfigurowania klaster tkaninie usługi autonomicznej za pomocą pliku _**ClusterConfig.JSON**_ . Ten plik umożliwia Określ informacje, takie jak węzły tkaninie usług i ich adresy IP, różne typy węzłów na klaster, konfiguracji zabezpieczeń, a także topologia sieci pod względem domen błędów i uaktualnienia dla klaster autonomicznego.

Gdy [Pobierz tkaninie usługi autonomicznej](service-fabric-cluster-creation-for-windows-server.md#downloadpackage), kilka przykładów pliku ClusterConfig.JSON są pobierane do komputera pracy. Przykłady o *DevCluster* w nazwach pomoże Ci utworzyć klaster o wszystkich trzech węzłów na tym samym komputerze, takich jak węzły logiczne. Wylogowywanie się z tych opcji co najmniej jeden węzeł muszą być oznaczone jako podstawowego węzła. Ten klaster jest przydatna dla środowiska projektowego lub testowego i nie jest obsługiwany jako klaster produkcji. Próbki o *MultiMachine* w ich nazwy pomoże Ci utworzyć klaster jakości produkcji z każdego węzła na osobnym komputerze. Liczby węzłów podstawowego dla tych klaster bazowały na [poziomie niezawodności](#reliability).

1. *ClusterConfig.Unsecure.DevCluster.JSON* i *ClusterConfig.Unsecure.MultiMachine.JSON* pokazano, jak utworzyć niezabezpieczoną klaster test lub produkcji odpowiednio. 
    
2. *ClusterConfig.Windows.DevCluster.JSON* i *ClusterConfig.Windows.MultiMachine.JSON* pokazano, jak utworzyć klaster test lub produkcji chronione za pomocą [zabezpieczeń systemu Windows](service-fabric-windows-cluster-windows-security.md).

3. *ClusterConfig.X509.DevCluster.JSON* i *ClusterConfig.X509.MultiMachine.JSON* pokazująca, jak utworzyć klaster test lub produkcji chronione za pomocą [X509 na podstawie certyfikatu zabezpieczeń](service-fabric-windows-cluster-x509-security.md). 


Teraz możemy będzie sprawdzić, czy różne części pliku _**ClusterConfig.JSON**_ jako poniżej.

## <a name="general-cluster-configurations"></a>Konfiguracje klastrów ogólne
Obejmuje szeroki klaster określonych konfiguracji, jak pokazano poniżej wstawek JSON.

    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "2015-01-01-alpha",

Wszelkie przyjazną nazwę, która może przekazać do klaster tkaninie usługi przypisując do **nazwy** zmiennej. **ClusterConfigurationVersion** jest numer wersji klaster; należy go zwiększyć każdorazowo po uaktualnieniu klaster tkaninie usługi. Należy jednak pozostawić **apiVersion** wartość domyślna.


<a id="clusternodes"></a>
## <a name="nodes-on-the-cluster"></a>Węzły w klastrze
Węzły w klastrze tkaninie usługi można skonfigurować za pomocą sekcji **węzły** , jak to przedstawiono w poniższym wstawek.

    "nodes": [{
        "nodeName": "vm0",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r0",
        "upgradeDomain": "UD0"
    }, {
        "nodeName": "vm1",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType1",
        "faultDomain": "fd:/dc1/r1",
        "upgradeDomain": "UD1"
    }, {
        "nodeName": "vm2",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType2",
        "faultDomain": "fd:/dc1/r2",
        "upgradeDomain": "UD2"
    }],

Klaster tkaninie usługi musi zawierać co najmniej 3 węzły. Możesz dodać więcej węzłów do tej sekcji według konfiguracji. W poniższej tabeli opisano ustawienia konfiguracji dla każdego węzła.

|**Węzeł Konfiguracja**|**Opis**|
|-----------------------|--------------------------|
|NazwaWęzła|Możesz zmienić dowolne przyjazną nazwę do węzła.|
|adres IP|Dowiedz się, adres IP węzła otwarcie okna polecenia i wpisując `ipconfig`. Zanotuj adres IP protokołu IPV4 i przypisać go do zmiennej **adres IP** .|
|nodeTypeRef|Każdy węzeł można przypisywać typu innego węzła. [Typy węzłów](#nodetypes) są definiowane w sekcji poniżej.|
|faultDomain|Błąd domeny umożliwiają administratorom klaster Definiowanie fizycznie węzły, które może zakończyć się niepowodzeniem w tym samym czasie z powodu zależności fizycznie udostępnionych.|
|upgradeDomain|Uaktualnianie domen opisują zestaw węzłów, które są zamknięte uaktualnień usługi tkaninie u o tym samym czasie. Możesz wybrać węzły, które w celu przypisania do uaktualnienia domen, jak nie są ograniczone przez wymagań fizycznym.| 


## <a name="cluster-properties"></a>Klaster Właściwości

Sekcja **Właściwości** w ClusterConfig.JSON jest używana do skonfiguruj klaster w następujący sposób.

<a id="reliability"></a>
### <a name="reliability"></a>Niezawodność 
W sekcji **reliabilityLevel** określa liczbę kopii usług systemowych, które można uruchamiać w węzłach podstawowy klaster. Zwiększa to niezawodność tych usług i w związku z tym klastrze. Można ustawić tej zmiennej *brązowy*, *Srebrny*, *Złoty* lub *Platinum* 3, 5, 7 lub 9 kopii tych usług odpowiednio. Zobacz przykład poniżej.

    "reliabilityLevel": "Bronze",
    
Należy zauważyć, że ponieważ główny węzeł uruchamia pojedynczej kopii usługi systemowe, można będzie co najmniej 3 węzły podstawowego dla *brązowy*5 dla *Silver*, 7 dla *Złoty* i 9 dla poziomów niezawodności *Platinum* .


### <a name="diagnostics"></a>Narzędzia diagnostyczne
Sekcja **diagnosticsStore** umożliwia konfigurowanie parametrów umożliwiające narzędzia diagnostyczne i rozwiązywanie problemów z węzeł lub klaster błędy, jak pokazano w poniższej wstawki kodu. 

    "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "FileShare",
        "IsEncrypted": "false",
        "connectionstring": "c:\\ProgramData\\SF\\DiagnosticsStore"
    }

**Metadane** zamieszczono opis działania narzędzia Diagnostyka sieci klaster i można ustawić według konfiguracji. Zmienne pomoc w zbierania ETW dzienników, zrzuty awaryjne a także liczniki wydajności. Więcej informacji na temat dzienniki śledzenia ETW Odczytaj [Tracelog](https://msdn.microsoft.com/library/windows/hardware/ff552994.aspx) i [ETW Tracing](https://msdn.microsoft.com/library/ms751538.aspx) . Wszystkie dzienniki tym [liczniki wydajności](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) i [awarii zrzuty](https://blogs.technet.microsoft.com/askperf/2008/01/08/understanding-crash-dump-files/) można przekierowanie do folderu **connectionString** na tym komputerze. Za pomocą *AzureStorage* do przechowywania diagnostyki. Poniżej znajduje się fragment próbki.

    "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "AzureStorage",
        "IsEncrypted": "false",
        "connectionstring": "xstore:DefaultEndpointsProtocol=https;AccountName=[AzureAccountName];AccountKey=[AzureAccountKey]"
    }

### <a name="security"></a>Zabezpieczenia 
Sekcja **Zabezpieczenia** jest konieczne klastrze tkaninie usługi bezpiecznego autonomicznego. Poniższy fragment zawiera części tej sekcji.

    "security": {
        "metadata": "This cluster is secured using X509 certificates.",
        "ClusterCredentialType": "X509",
        "ServerCredentialType": "X509",
        . . .
    }

**Metadane** zamieszczono opis bezpiecznego klaster i można ustawić według konfiguracji. **ClusterCredentialType** i **ServerCredentialType** określają typ wykona klaster i węzłów papieru wartościowego. Ustawieniem albo *X509* dla papieru wartościowego na podstawie certyfikatu lub *systemu Windows* dla zabezpieczenia oparte na usługi Azure Active Directory. Pozostałą część sekcji **Zabezpieczenia** będzie można na podstawie typu zabezpieczenia. Przeczytaj [Zabezpieczenia oparte na certyfikatów w klastrze autonomicznej](service-fabric-windows-cluster-x509-security.md) lub [zabezpieczeń systemu Windows w klastrze autonomicznego](service-fabric-windows-cluster-windows-security.md) informacje na temat wypełniania pozostałą część sekcji **Zabezpieczenia** .


<a id="nodetypes"></a>
### <a name="node-types"></a>Typy węzłów
W sekcji **nodeTypes** opisano rodzaj węzły, które zawiera klaster. Należy określić typ co najmniej jeden węzeł dla klastrów, jak pokazano poniżej wstawek. 

    "nodeTypes": [{
        "name": "NodeType0",
        "clientConnectionEndpointPort": "19000",
        "clusterConnectionEndpointPort": "19001",
        "leaseDriverEndpointPort": "19002"
        "serviceConnectionEndpointPort": "19003",
        "httpGatewayEndpointPort": "19080",
        "applicationPorts": {
            "startPort": "20575",
            "endPort": "20605"
        },
        "ephemeralPorts": {
            "startPort": "20606",
            "endPort": "20861"
        },
        "isPrimary": true
    }]

**Nazwa** jest przyjazną nazwę dla tego typu określonego węzła. Aby utworzyć węzeł tego typu węzeł, przypisywanie jego przyjaznej nazwy zmiennej **nodeTypeRef** dla tego węzła jako wymienionych [powyżej](#clusternodes). Dla każdego typu węzła punkty końcowe połączenia, które będą używane. Można wybrać dowolny numer portu dla tych punkty końcowe połączeń, dopóki nie powodują one konfliktu z innymi punkty końcowe w tym klastrze. Klaster z wielu węzłów będzie jeden lub więcej węzłów podstawowego (to znaczy **isPrimary** wartość *PRAWDA*), w zależności od [**reliabilityLevel**](#reliability). W tym artykule [tkaninie usługi Klaster zdolności zagadnienia związane z planowaniem](service-fabric-cluster-capacity.md) informacji o wartości **nodeTypes** i **reliabilityLevel** i wiedzieć, co to są podstawowego i typy węzłów inne niż podstawowe. 

#### <a name="endpoints-used-to-configure-the-node-types"></a>Umożliwia konfigurowanie typy węzłów punkty końcowe

- *clientConnectionEndpointPort* jest używany przez klienta, aby połączyć się z klastrem, podczas korzystania z klienta interfejsów API. 
- *clusterConnectionEndpointPort* jest port, w którym węzły komunikowania się między sobą.
- *leaseDriverEndpointPort* jest używana przez sterownik dzierżawy klaster, aby dowiedzieć się, jeśli węzły są nadal aktywne. 
- *serviceConnectionEndpointPort* jest używane przez aplikacje i usługi wdrożony w węźle na komunikowanie się za pomocą klienta usługi tkaninie na dany węzeł.
- *httpGatewayEndpointPort* jest używane przez Eksploratora tkaninie usługi, aby połączyć się z klastrem.
- *ephemeralPorts* są [dynamiczne porty używane przez system operacyjny](https://support.microsoft.com/kb/929851). Usługa tkaninie użyje części tych aplikacji porty i pozostałe będzie dostępne w systemie operacyjnym. Go będzie również mapować ten zakres do istniejącego zakresu prezentowanie w systemie operacyjnym, tak aby dla wszystkich celów można używać zakresy w przykładowe pliki JSON. Potrzebujesz upewnić się, że różnica między datą początkową a porty końcowe jest co najmniej 255. 
- *applicationPorts* są portów, które będą używane przez aplikacje usług tkaninie. Powinny być podzbiór *ephemeralPorts*, odpowiednio do pokrycia zapotrzebowania punkt końcowy aplikacji. Tkaninie usługi użyje tych zawsze, gdy nowe porty są wymagane, a także zajmować otwierania dla tych portów zapory. 
- *reverseProxyEndpointPort* jest punktu końcowego opcjonalne zwrotny serwer proxy. Aby uzyskać więcej informacji, zobacz [Serwer Proxy usługi tkaninie odwrócić](service-fabric-reverseproxy.md) . 


### <a name="other-settings"></a>Inne ustawienia
Sekcja **fabricSettings** umożliwia ustawianie katalogów głównych tkaninie usługi danych i dzienników. Możesz dostosować te tylko podczas tworzenia klaster początkowej. Poniżej znajduje się fragment próbki w tej sekcji.

    "fabricSettings": [{
        "name": "Setup",
        "parameters": [{
            "name": "FabricDataRoot",
            "value": "C:\\ProgramData\\SF"
        }, {
            "name": "FabricLogRoot",
            "value": "C:\\ProgramData\\SF\\Log"
    }]

Zaleca się przy użyciu dysku-OS jako FabricDataRoot i FabricLogRoot, ponieważ tworzy ona, że więcej niezawodności pod kątem zgodności z systemem operacyjnym ulega awarii. Należy zauważyć, że podczas dostosowywania pierwiastek danych, następnie pierwiastek dziennika będzie znajdować się jeden poziom niżej na poziomie głównym danych.


## <a name="next-steps"></a>Następne kroki

Po umieszczeniu cały plik ClusterConfig.JSON skonfigurowane zgodnie z ustawień klaster autonomicznej, wykonując tego artykułu można wdrażać klaster [utworzyć klaster tkaninie usługi Azure lokalnego lub w chmurze](service-fabric-cluster-creation-for-windows-server.md) , a następnie przejdź do [wizualizacji klaster w Eksploratorze tkaninie usługi](service-fabric-visualizing-your-cluster.md).


