<properties
   pageTitle="Tworzenie równoważenia obciążenia wewnętrznych dla usług w chmurze w modelu Klasyczny wdrożenia | Microsoft Azure"
   description="Dowiedz się, jak utworzyć równoważenia obciążenia wewnętrznych, przy użyciu programu PowerShell w modelu Klasyczny wdrażania"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/09/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internal-load-balancer-classic-for-cloud-services"></a>Wprowadzenie do tworzenia równoważenia obciążenia wewnętrznych (klasyczny) dla usług w chmurze

[AZURE.INCLUDE [load-balancer-get-started-ilb-classic-selectors-include.md](../../includes/load-balancer-get-started-ilb-classic-selectors-include.md)]

<BR>

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Dowiedz się, jak [wykonać te kroki przy użyciu modelu Menedżera zasobów](load-balancer-get-started-ilb-arm-ps.md).


## <a name="configure-internal-load-balancer-for-cloud-services"></a>Konfigurowanie usługi równoważenia obciążenia wewnętrznych dla usług w chmurze

Usługi równoważenia obciążenia wewnętrzny jest obsługiwane zarówno w środowisku maszyn wirtualnych systemu, jak i usług w chmurze. Punktu końcowego usługi równoważenia obciążenia wewnętrznych utworzone w usłudze w chmurze, znajdującej się poza regionalne wirtualnej sieci będą dostępne tylko w ramach usługi w chmurze.

Konfiguracja usługi równoważenia obciążenia wewnętrznych ma można ustawić podczas tworzenia pierwszego wdrożenia w usłudze w chmurze, jak pokazano w przykładzie poniżej.

>[AZURE.IMPORTANT] Wymagania wstępne do uruchomienia poniższe kroki ma wirtualnej sieci już utworzone w celu wdrożenia chmury. Konieczne będzie, wirtualną sieć nazwę i nazwę podsieci, aby utworzyć wewnętrznych równoważenia obciążenia.

### <a name="step-1"></a>Krok 1

Otwórz plik konfiguracyjny usługi (.cscfg) z wdrożeniem chmury w programie Visual Studio i dodaj następującą sekcję Tworzenie wewnętrznego równoważenia obciążenia w obszarze ostatnio "`</Role>`" elementu konfiguracji sieci.




    <NetworkConfiguration>
      <LoadBalancers>
        <LoadBalancer name="name of the load balancer">
          <FrontendIPConfiguration type="private" subnet="subnet-name" staticVirtualNetworkIPAddress="static-IP-address"/>
        </LoadBalancer>
      </LoadBalancers>
    </NetworkConfiguration>


Dodawanie wartości dla pliku konfiguracji sieci pokazać, jak będzie wyglądać. W tym przykładzie założono, że została utworzona podsieć 10.0.0.0/24 podsieci o nazwie test_subnet i statyczny adres IP 10.0.0.4 o nazwie "test_vnet". Usługi równoważenia obciążenia będzie nazwę testLB.

    <NetworkConfiguration>
      <LoadBalancers>
        <LoadBalancer name="testLB">
          <FrontendIPConfiguration type="private" subnet="test_subnet" staticVirtualNetworkIPAddress="10.0.0.4"/>
        </LoadBalancer>
      </LoadBalancers>
    </NetworkConfiguration>

Aby uzyskać więcej informacji o schemacie równoważenia obciążenia zobacz [Dodawanie Usługa równoważenia obciążenia](https://msdn.microsoft.com/library/azure/dn722411.aspx).

### <a name="step-2"></a>Krok 2


Zmienianie plik definicji (.csdef) usługi, aby dodać punkty końcowe do wewnętrznego równoważenia obciążenia. Moment wystąpienia roli jest tworzony, plik definicji usługi spowoduje dodanie wystąpień roli do wewnętrznego równoważenia obciążenia.


    <WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
      <Endpoints>
        <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" loadBalancer="load-balancer-name" />
      </Endpoints>
    </WorkerRole>

Po tych samych wartości z powyższego przykładu możesz dodać wartości w pliku definicji usługi.

    <WorkerRole name=WorkerRole1" vmsize="A7" enableNativeCodeExecution="[true|false]">
      <Endpoints>
        <InputEndpoint name="endpoint1" protocol="http" localPort="80" port="80" loadBalancer="testLB" />
      </Endpoints>
    </WorkerRole>

Ruch sieciowy będzie równoważenia przy użyciu usługi równoważenia obciążenia testLB przy użyciu portu 80 dla przychodzących żądań wysyłania do wystąpienia roli Pracownik również na porcie 80 obciążenia.


## <a name="next-steps"></a>Następne kroki

[Konfigurowanie trybu rozkładu równoważenia obciążenia przy użyciu źródła IP koligacji](load-balancer-distribution-mode.md)

[Konfigurowanie ustawienia przekroczenia limitu czasu bezczynności protokołu TCP dla usługi równoważenia obciążenia](load-balancer-tcp-idle-timeout.md)