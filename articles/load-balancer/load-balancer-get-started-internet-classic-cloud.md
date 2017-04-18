<properties
   pageTitle="Wprowadzenie do tworzenia internetowej przeciwległych równoważenia obciążenia w klasycznym wdrożenia modelu przy użyciu dla usług w chmurze | Microsoft Azure"
   description="Dowiedz się, jak utworzyć internetową przeciwległych równoważenia obciążenia w modelu Klasyczny wdrożenia dla usług w chmurze"
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
   ms.date="03/17/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internet-facing-load-balancer-for-cloud-services"></a>Wprowadzenie do tworzenia dostępnych równoważenia obciążenia dla usług w chmurze przez Internet

[AZURE.INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]W tym artykule opisano, jak model klasyczny wdrożenia. Można także [dowiedzieć się, jak utworzyć internetową przeciwległych równoważenia obciążenia za pomocą Menedżera zasobów Azure](load-balancer-get-started-internet-arm-cli.md).

Usług w chmurze są konfigurowane automatycznie przy użyciu usługi równoważenia obciążenia i można je dostosować przez model usługi.

## <a name="create-a-load-balancer-using-the-service-definition-file"></a>Tworzenie usługi równoważenia obciążenia przy użyciu pliku definicji usługi

Można wykorzystać SDK Azure dla .NET 2,5 aktualizacji usługi w chmurze. Ustawienia punktu końcowego dla usług w chmurze są wprowadzane w pliku .csdef [definicji usługi](https://msdn.microsoft.com/library/azure/gg557553.aspx).

W poniższym przykładzie pokazano konfiguracji pliku servicedefinition.csdef wdrożenia chmurze:

Sprawdzanie snipet pliku .csdef wygenerowane przez wdrożenia chmury, można zobaczyć zewnętrzny punkt końcowy skonfigurowany do używania porty protokołu HTTP na porcie 10000, 10001 i 10002.


    <ServiceDefinition name=“Tenant“>
    <WorkerRole name=“FERole” vmsize=“Small“>
    <Endpoints>
        <InputEndpoint name=“FE_External_Http” protocol=“http” port=“10000“ />
        <InputEndpoint name=“FE_External_Tcp“  protocol=“tcp“  port=“10001“ />
        <InputEndpoint name=“FE_External_Udp“  protocol=“udp“  port=“10002“ />

        <InputEndpointname=“HTTP_Probe” protocol=“http” port=“80” loadBalancerProbe=“MyProbe“ />

        <InstanceInputEndpoint name=“InstanceEP” protocol=“tcp” localPort=“80“>
           <AllocatePublicPortFrom>
              <FixedPortRange min=“10110” max=“10120“  />
           </AllocatePublicPortFrom>
        </InstanceInputEndpoint>
        <InternalEndpoint name=“FE_InternalEP_Tcp” protocol=“tcp“ />
    </Endpoints>
    </WorkerRole>
    </ServiceDefinition>




## <a name="check-load-balancer-health-status-for-cloud-services"></a>Sprawdzanie stanu kondycji usługi równoważenia obciążenia dla usług w chmurze


Oto przykład sondy kondycji:

        <LoadBalancerProbes>
        <LoadBalancerProbe name=“MyProbe” protocol=“http” path=“Probe.aspx” intervalInSeconds=“5” timeoutInSeconds=“100“ />
        </LoadBalancerProbes>

Usługi równoważenia obciążenia łączy informacje punktu końcowego i sondy do utworzenia adresu URL w postaci http://{DIP VM}:80/Probe.aspx, który może być używany do kwerendy kondycji usługi.

Usługa wykrywa okresowych sondy z tego samego adresu IP. Jest to żądanie sondy kondycji pochodzące z hosta węzła miejsce, w którym jest uruchomiony maszyny wirtualnej.
Usługa ma reagować z kodem stanu HTTP 200 do równoważenia obciążenia do przyjęto założenie, że usługa jest prawidłowy. Wszelkie inne kod stanu HTTP (na przykład 503) bezpośrednio trwa maszyny wirtualnej poza obrotu.

Definicja sondy steruje także częstotliwość sondy. W naszym przypadku powyżej równoważenia obciążenia jest badanie punkt końcowy co 5 sekundach. Odebranie odpowiedzi dodatnią dla 10 s (dwa sondy interwałów) sondy przyjęto, że w dół, a maszyny wirtualnej, jest przyjmowana poza obrotu. Podobnie jeśli usługa znajduje się poza obrót i odebraniu odpowiedzi dodatnia, usługę jest przywracane do obrotu od razu. Jeśli usługa jest zmienne między prawidłowy i nieprawidłowe, równoważenia obciążenia można wybrać opóźnić ponowne wprowadzenie usługi powrót do obrotu, dopóki zostaną prawidłowy dla liczby sondy.

Sprawdzanie schematu definicji usługi dla [sondy kondycji](https://msdn.microsoft.com/library/azure/jj151530.aspx) , aby uzyskać więcej informacji.

## <a name="next-steps"></a>Następne kroki

[Przed rozpoczęciem konfigurowania równoważenia obciążenia wewnętrznych](load-balancer-get-started-ilb-arm-ps.md)

[Konfigurowanie trybu rozkładu równoważenia obciążenia](load-balancer-distribution-mode.md)

[Konfigurowanie ustawienia przekroczenia limitu czasu bezczynności protokołu TCP dla usługi równoważenia obciążenia](load-balancer-tcp-idle-timeout.md)

