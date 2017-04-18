<properties
   pageTitle="Usługi tkaninie Reverse Proxy | Microsoft Azure"
   description="Użyj serwera proxy odwrotnej tkaninie usługi komunikacji do microservices od wewnątrz i na zewnątrz klaster"
   services="service-fabric"
   documentationCenter=".net"
   authors="BharatNarasimman"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="10/04/2016"
   ms.author="vturecek"/>

# <a name="service-fabric-reverse-proxy"></a>Usługa tkaninie Reverse Proxy

Usługa tkaninie Reverse proxy jest tkaninie wbudowane usługi zwrotny serwer proxy, umożliwiająca adresowania microservices w klastrze tkaninie usług, które uwidaczniają punkty końcowe protokołu HTTP.

## <a name="microservices-communication-model"></a>Microservices komunikacji modelu

Microservices na tkaninie usługi zwykle uruchamiać na podzbiór maszyn wirtualnych w klastrze i można zmienić jeden maszyn wirtualnych w różnych powodów. Aby zmienić dynamicznie punkty końcowe dla microservices. Typowe wzorzec do przekazywania microservice jest poniżej, pętli rozwiązania problemu

1. Rozwiązywanie problemów lokalizacji usługi początkowo za pośrednictwem usługi nazw.
2. Nawiązywanie połączenia z usługą.
3. Ustalanie przyczyny błędów połączenia i ponownie rozwiązać lokalizacji usługi, gdy jest to konieczne.

Proces ten obejmuje zazwyczaj zawijanie bibliotek komunikacji po stronie klienta w pętli ponów próbę korzystający z zasad usługi rozdzielczości i spróbuj ponownie.
Aby uzyskać więcej informacji na ten temat zobacz [komunikowania się z usługami](service-fabric-connect-and-communicate-with-services.md).

### <a name="communicating-via-sf-reverse-proxy"></a>Komunikowanie się za pomocą rz odwrotnej serwera proxy
Zwrotny serwer proxy usług tkaninie działa we wszystkich węzłach w klastrze. Uruchamia proces rozwiązywania całej usługi w imieniu klienta, a następnie przesyła żądanie klienta. Dlatego uruchomionym w klastrze można przy użyciu żadnych bibliotek komunikacji HTTP po stronie klienta skontaktować się z usługą docelową, za pomocą serwera proxy odwrotnej rz uruchomiony lokalnie, w tym samym węźle.

![Komunikacji wewnętrznej][1]

## <a name="reaching-microservices-from-outside-the-cluster"></a>Możliwość Microservices z poza klastrem
Domyślny model komunikacji zewnętrznej dla microservices jest **Wyraź zgodę na** miejsce, w którym każdej usługi, które domyślnie nie są dostępne bezpośrednio z klientów zewnętrznych. [Usługi równoważenia obciążenia Azure](../load-balancer/load-balancer-overview.md) to granica sieci między microservices i zewnętrznych klientów, który wykonuje translatora adresów sieciowych i przekazuje żądania zewnętrznych do wewnętrznego **IP:port** punktów końcowych. Aby udostępnić punkt końcowy microservice bezpośrednio do klientów zewnętrznych, równoważenia obciążenia Azure najpierw musisz skonfigurować do przekazywania ruchu do każdego portu używane przez usługę w klastrze. Ponadto większość microservices (esp. stanowe microservices) nie live we wszystkich węzłach klaster i mogą być przenoszone między węzłów na awaryjnym przeniesieniu, w takich przypadkach równoważenia obciążenia Azure skuteczne nie można określić węzła docelowego repliki znajdują się do przesyłania ruchu do.

### <a name="reaching-microservices-via-the-sf-reverse-proxy-from-outside-the-cluster"></a>Możliwość Microservices za pomocą rz odwrotnej serwera proxy z poza klastrem

Zamiast konfigurowania portów poszczególnych usług w równoważenia obciążenia azure, można skonfigurować tylko port serwera proxy odwrotna kolejność rz usługi równoważenia obciążenia Azure. Dzięki temu klienci poza klastrem do uzyskania dostępu do usług w klastrze za pomocą odwrotnej serwera proxy bez dodatkowych konfiguracji.

![Zewnętrzna komunikacja][0]

>[AZURE.WARNING] Konfigurowanie port odwrotnej serwera proxy w module równoważenia obciążenia, sprawia, że wszystkie micro usługi w grupie uwidaczniane punkt końcowy http, należy addressible z poza klastrem.


## <a name="uri-format-for-addressing-services-via-the-reverse-proxy"></a>Identyfikator URI format adresowania usług za pośrednictwem odwrotnej serwera proxy

Reverse proxy używa określonego formatu identyfikatora URI do identyfikowania które partition usługi przychodzące żądanie powinna być przesyłana dalej do:

```
http(s)://<Cluster FQDN | internal IP>:Port/<ServiceInstanceName>/<Suffix path>?PartitionKey=<key>&PartitionKind=<partitionkind>&Timeout=<timeout_in_seconds>
```

 - **http (s):** Zaakceptuj ruch HTTP lub HTTPS można skonfigurować odwrotnej serwera proxy. W przypadku ruchu HTTPS zakończenie SSL występuje w odwrotnej serwera proxy. Żądania, które mają być przekazywane przez odwrotne serwery proxy usług w klastrze są za pomocą protokołu http.
 - **Klaster FQDN | wewnętrznych IP:** Dla klientów zewnętrznych zwrotny serwer proxy można skonfigurować tak, aby jest dostępne za pośrednictwem klaster domeny (na przykład mycluster.eastus.cloudapp.azure.com). Domyślnie zwrotny serwer proxy działa na każdym węźle więc dla ruchu wewnętrznym go można się z Tobą na hosta lokalnego lub dowolny adres IP wewnętrznego węzła (na przykład 10.0.0.1).
 - **Port:** Port określoną odwrotnej serwera proxy. Przykład: 19008.
 - **ServiceInstanceName:** To jest w pełni kwalifikowane wdrożonym wystąpienie nazwę usługi ma zostać uznany za osiągnięcia "tkaninie:-" schematu. Na przykład, aby osiągnąć usługi *tkaninie: / myapp/Moja_usługa/*, można użyć *MojaApl Moja_usługa*.
 - **Ścieżka sufiks:** To rzeczywisty ścieżki adresu URL usługi, którą chcesz nawiązać połączenie. Na przykład *myapi/wartości Dodawanie-3*
 - **PartitionKey:** Podzielone na partycje usługi to jest klucz obliczoną partition partycją, do którego chcesz się skontaktować. Należy zauważyć, że jest to *nie* partycją identyfikator GUID. Ten parametr nie jest wymagane dla usług przy użyciu schematu partition pojedynczych.
 - **PartitionKind:** Schemat partition usługi. Może to być "Int64Range" lub "O nazwie". Ten parametr nie jest wymagane dla usług przy użyciu schematu partition pojedynczych.
 - **Przekroczenia limitu czasu:**  To ustawienie określa limit czasu dla żądania http utworzone przez odwrotnej serwera proxy do usługi w imieniu żądania klienta. Wartość domyślna to jest 60 sekund. Jest to parametr opcjonalny.

### <a name="example-usage"></a>Przykład użycia

Na przykład Przyjrzyjmy się usługi **tkaninie: / MyApp/Moja_usługa** który otwiera odbiornik HTTP na następujący adres URL:

```
http://10.0.05:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/
```

Z następujących zasobów:

 - `/index.html`
 - `/api/users/<userId>`

Jeśli usługa używa schematu podziału pojedynczych, parametry ciągu kwerendy *PartitionKey* i *PartitionKind* nie są wymagane i usługi można się z Tobą za pośrednictwem bramy jako:

 - Zewnętrznie:`http://mycluster.eastus.cloudapp.azure.com:19008/MyApp/MyService`
 - Wewnętrznie:`http://localhost:19008/MyApp/MyService`

Jeśli usługa używa Int64 jednolitego schematu podziału, parametry ciągu kwerendy *PartitionKey* i *PartitionKind* należy do osiągnięcia partycją usługi:

 - Zewnętrznie:`http://mycluster.eastus.cloudapp.azure.com:19008/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`
 - Wewnętrznie:`http://localhost:19008/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`

Aby dotrzeć zasoby udostępniane przez usługę, po prostu umieścić ścieżki zasobu po nazwie usługi w adresie URL:

 - Zewnętrznie:`http://mycluster.eastus.cloudapp.azure.com:19008/MyApp/MyService/index.html?PartitionKey=3&PartitionKind=Int64Range`
 - Wewnętrznie:`http://localhost:19008/MyApp/MyService/api/users/6?PartitionKey=3&PartitionKind=Int64Range`

Bramy następnie przekazuje te żądania do adresu URL usługi:

 - `http://10.0.05:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/index.html`
 - `http://10.0.05:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/api/users/6`

## <a name="special-handling-for-port-sharing-services"></a>Specjalnej obsługi udostępniania portów usług

Bramy aplikacja próbuje ponownie rozwiązać adres usługi i ponawianie żądania, gdy usługa nie można się z Tobą. To jest jedną z głównych zalet bramy, jak kod klienta, nie musisz zaimplementować własną rozdzielczość usługi i rozwiązywanie pętli.

Zazwyczaj gdy usługa nie można się z Tobą oznacza, że wystąpienie usługi lub replice został przeniesiony do innego węzła jako część jej normalnego cyklu życia. W takim przypadku bramy może zostać wyświetlony błąd połączenia sieciowego wskazuje, że jest już otwarty na pierwotnie rozpoznany adres punktu końcowego.

Jednak repliki lub wystąpień usługi można udostępniać proces host i może także udostępnić port obsługiwanych przez serwer sieci web opartych na http.sys, w tym:

 - [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener%28v=vs.110%29.aspx)
 - [WebListener Core programu ASP.NET](https://docs.asp.net/latest/fundamentals/servers.html#weblistener)
 - [Katana](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.OwinSelfHost/)

W tej sytuacji prawdopodobnie serwer sieci web jest dostępna w procesie hosta i odpowiadać na żądania, ale wystąpienie usługi rozwiązany lub replice nie jest już dostępna na hoście. W tym przypadku bramy otrzyma odpowiedź HTTP 404 na serwerze sieci web. W wyniku HTTP 404 ma dwa różne znaczenie:

 1. Adres usługi jest poprawny, ale nie istnieje zasobu wymagane przez użytkownika.
 2. Jest niepoprawny adres usługi, a zasobu wymagane przez użytkownika faktycznie może istnieć na inny węzeł.

W pierwszym przypadku jest to normalny 404 HTTP, uznanych za błąd użytkownika. Jednak w przypadku drugiego użytkownika zażądał zasobu, która istnieje, ale nie bramy je odszukać, ponieważ sama usługa został przeniesiony, w tym przypadku bramy należy ponownie rozpoznać adresu i spróbuj ponownie.

Dlatego bramy wymaga sposób, aby odróżnić między tymi dwoma przypadkami. W celu wykonania tego rozróżniania, wymagany jest wskazówkę z serwera.

 - Domyślnie bramy aplikacji założono litery #2 i próbuje ponownie rozwiązywanie problemów i ponownego wydania wezwanie.
 - Aby wskazać litery #1 do bramy aplikacji, usługa powinna zwrócić następujące nagłówek odpowiedzi HTTP:

`X-ServiceFabric : ResourceNotFound`

Ten nagłówek odpowiedzi HTTP wskazuje normalnej sytuacji HTTP 404, w którym nie istnieje żądany zasób, a bramy nie spróbuje ponownie rozpoznać adres usługi.

## <a name="setup-and-configuration"></a>Instalacja i Konfiguracja
Serwer proxy odwrotnej tkaninie usługi można włączyć dla klaster za pomocą [szablonu Azure Menedżera zasobów](./service-fabric-cluster-creation-via-arm.md).

Po utworzeniu szablonu dla klastrów, którą chcesz wdrożyć (z przykładowe szablony lub tworząc szablon Menedżera zasobów niestandardowych) Reverse proxy można włączyć w szablonie przez następujące kroki.

1. Zdefiniuj port odwrotnej serwera proxy w [sekcji Parametry](../resource-group-authoring-templates.md) szablonu.

    ```json
    "SFReverseProxyPort": {
        "type": "int",
        "defaultValue": 19008,
        "metadata": {
            "description": "Endpoint for Service Fabric Reverse proxy"
        }
    },
    ```
2. Określ port dla każdego z obiektów typ węzła w **klastrze** [sekcji Typ zasobu](../resource-group-authoring-templates.md)

    Dla apiVersion przed "2016-09-01' port jest identyfikowany przez parametr Nazwa ***httpApplicationGatewayEndpointPort***

    ```json
    {
        "apiVersion": "2016-03-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
       "nodeTypes": [
          {
           ...
           "httpApplicationGatewayEndpointPort": "[parameters('SFReverseProxyPort')]",
           ...
          },
        ...
        ],
        ...
    }
    ```

    Dla apiVersion osoby na lub po "2016-09-01' port jest identyfikowany przez parametr Nazwa ***reverseProxyEndpointPort***

    ```json
    {
        "apiVersion": "2016-09-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
       "nodeTypes": [
          {
           ...
           "reverseProxyEndpointPort": "[parameters('SFReverseProxyPort')]",
           ...
          },
        ...
        ],
        ...
    }
    ```

3. Aby adres zwrotny serwer proxy z zewnątrz azure klaster, należy ustawić **Zasady równoważenia obciążenia azure** port określony w kroku 1.

    ```json
    {
        "apiVersion": "[variables('lbApiVersion')]",
        "type": "Microsoft.Network/loadBalancers",
        ...
        ...
        "loadBalancingRules": [
            ...
            {
                "name": "LBSFReverseProxyRule",
                "properties": {
                    "backendAddressPool": {
                        "id": "[variables('lbPoolID0')]"
                    },
                    "backendPort": "[parameters('SFReverseProxyPort')]",
                    "enableFloatingIP": "false",
                    "frontendIPConfiguration": {
                        "id": "[variables('lbIPConfig0')]"
                    },
                    "frontendPort": "[parameters('SFReverseProxyPort')]",
                    "idleTimeoutInMinutes": "5",
                    "probe": {
                        "id": "[concat(variables('lbID0'),'/probes/SFReverseProxyProbe')]"
                    },
                    "protocol": "tcp"
                }
            }
        ],
        "probes": [
            ...
            {
                "name": "SFReverseProxyProbe",
                "properties": {
                    "intervalInSeconds": 5,
                    "numberOfProbes": 2,
                    "port":     "[parameters('SFReverseProxyPort')]",
                    "protocol": "tcp"
                }
            }  
        ]
    }
    ```
4. Aby skonfigurować certyfikatów SSL na porcie dla Reverse proxy, dodać certyfikat do właściwości httpApplicationGatewayCertificate w [sekcji Typ zasobu](../resource-group-authoring-templates.md) **klaster**

    Dla apiVersion przed "2016-09-01' certyfikat jest identyfikowany przez parametr Nazwa ***httpApplicationGatewayCertificate***

    ```json
    {
        "apiVersion": "2016-03-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        "dependsOn": [
            "[concat('Microsoft.Storage/storageAccounts/', parameters('supportLogStorageAccountName'))]"
        ],
        "properties": {
            ...
            "httpApplicationGatewayCertificate": {
                "thumbprint": "[parameters('sfReverseProxyCertificateThumbprint')]",
                "x509StoreName": "[parameters('sfReverseProxyCertificateStoreName')]"
            },
            ...
            "clusterState": "Default",
        }
    }
    ```
    Dla apiVersion osoby na lub po "2016-09-01' certyfikat jest identyfikowany przez parametr Nazwa ***reverseProxyCertificate***
    
    ```json
    {
        "apiVersion": "2016-09-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        "dependsOn": [
            "[concat('Microsoft.Storage/storageAccounts/', parameters('supportLogStorageAccountName'))]"
        ],
        "properties": {
            ...
            "reverseProxyCertificate": {
                "thumbprint": "[parameters('sfReverseProxyCertificateThumbprint')]",
                "x509StoreName": "[parameters('sfReverseProxyCertificateStoreName')]"
            },
            ...
            "clusterState": "Default",
        }
    }
    ```

## <a name="next-steps"></a>Następne kroki
 - Zobacz przykład HTTP komunikacji między usługami w [Przykładowy projekt na GitHUb](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/master/Services/WordCount).

 - [Zdalnego wywołania procedury z zaufanego usługi zdalnej](service-fabric-reliable-services-communication-remoting.md)

 - [Interfejs API, który używa OWIN zaufania usługi sieci Web](service-fabric-reliable-services-communication-webapi.md)

 - [WCF komunikacji za pomocą usług zaufanego](service-fabric-reliable-services-communication-wcf.md)


[0]: ./media/service-fabric-reverseproxy/external-communication.png
[1]: ./media/service-fabric-reverseproxy/internal-communication.png
