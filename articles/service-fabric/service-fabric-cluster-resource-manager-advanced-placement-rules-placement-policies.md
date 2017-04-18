<properties
   pageTitle="Menedżer zasobów usługi tkaninie klaster — zasady położenie | Microsoft Azure"
   description="Omówienie zasad dodatkowe położenie oraz reguły pozwalające usługa tkaninie usługi"
   services="service-fabric"
   documentationCenter=".net"
   authors="masnider"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/19/2016"
   ms.author="masnider"/>

# <a name="placement-policies-for-service-fabric-services"></a>Zasady położenie dla usług tkaninie usługi
Istnieje wiele różnych dodatkowych reguł, które mogą trafiać opiekowanie o, jeśli klaster tkaninie usługi jest objęte przez geograficzne odległości, powiedz wielu centrach danych lub Azure regiony lub jeśli środowiska obejmuje wiele obszarów geopolitycznych control (lub kilka liter, miejsce, w którym mają ograniczenia prawne lub zasady istotnych informacji lub odległości związane mieć wpływ na wydajność rzeczywista/czas oczekiwania). Większość z tych można skonfigurować za pośrednictwem właściwości węzła i położenie ograniczenia, ale niektóre są bardziej skomplikowana. Z prośbą prostsze firma Microsoft udostępnia następujące dodatkowe commmands. Tak jak z innych ograniczeń położenie położenie zasad można skonfigurować na podstawie wystąpienia na nazwie usługi.

## <a name="specifying-invalid-domains"></a>Określanie nieprawidłowe domen
Zasady położenie InvalidDomain umożliwia określenie, czy określonej domeny błędów jest nieprawidłowa dla tej pracy. Tych zasad gwarantuje, że określonej usługi nigdy nie zostanie uruchomiona w określonym obszarze, na przykład ze względów geopolitycznych lub firmowych zasad. Wiele domen nieprawidłowe można określić przy użyciu oddzielne zasady.

![Przykład Nieprawidłowa domena][Image1]

Kod:

```csharp
ServicePlacementInvalidDomainPolicyDescription invalidDomain = new ServicePlacementInvalidDomainPolicyDescription();
invalidDomain.DomainName = "fd:/DCEast"; //regulations prohibit this workload here
serviceDescription.PlacementPolicies.Add(invalidDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("InvalidDomain,fd:/DCEast”)
```
## <a name="specifying-required-domains"></a>Określanie wymaganych domen
Zasady położenie wymagane domeny wymaga wszystkich replik stanowe lub wystąpień stateless usługi dla usługi zawarte w określonej domeny. Można określić wiele domen wymagane przez oddzielne zasady.

![Przykład wymagane domeny][Image2]

Kod:

```csharp
ServicePlacementRequiredDomainPolicyDescription requiredDomain = new ServicePlacementRequiredDomainPolicyDescription();
requiredDomain.DomainName = "fd:/DC01/RK03/BL2";
serviceDescription.PlacementPolicies.Add(requiredDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("RequiredDomain,fd:/DC01/RK03/BL2")
```

## <a name="specifying-a-preferred-domain-for-the-primary-replicas"></a>Określanie preferowanej domeny podstawowej replik
Domena podstawowa preferowane jest formantu interesujące, ponieważ umożliwia wybór domeny błędów, w którym należy umieścić podstawowy, jeśli jest to możliwe to zrobić. Gdy wszystko jest prawidłowy podstawowy zakończy w tej domenie. Należy domeny lub błędów replice podstawowego lub zamykanie jakiegoś powodu podstawowych będą migrowane do innej lokalizacji. Jeśli w preferowanym domeny nie ma tej lokalizacji, następnie jeśli to możliwe klaster Menedżera zasobów spowoduje przeniesienie go ponownie preferowanego domenę. Sposób naturalny to ustawienie tylko sens dla pełnowartościową usług. Tych zasad jest najbardziej przydatna w klastrów, które są łączone na Azure regiony lub wielu centrach danych. W następujących sytuacjach używana wszystkich lokalizacji dla nadmiarowości, ale wolisz umieścić podstawowy repliki w określonej lokalizacji w celu udostępniania mniejsze opóźnienia dla operacji, które przejdź do podstawowej (zapisuje, a także domyślnie wszystkie operacje odczytu są obsługiwane przez podstawowy).

![Preferowany domeny podstawowej i pracy awaryjnej][Image3]

```csharp
ServicePlacementPreferPrimaryDomainPolicyDescription primaryDomain = new ServicePlacementPreferPrimaryDomainPolicyDescription();
primaryDomain.DomainName = "fd:/EastUS/";
serviceDescription.PlacementPolicies.Add(invalidDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("PreferredPrimaryDomain,fd:/EastUS")
```

## <a name="requiring-replicas-to-be-distributed-among-all-domains-and-disallowing-packing"></a>Wymaganie replik ma być rozdzielona między wszystkich domen i niezezwalające pakowania
Inne zasady, które można określić jest wymaganie repliki, aby zawsze rozdzielana między domenami błędów dostępne. Dzieje się tak domyślnie w większości przypadków, w którym klaster jest uszkodzony, jednak istnieją zdegenerowanego przypadki, gdzie replik dla danego partition może być tymczasowo trafiać spakowany do pojedynczej awarii lub uaktualnienia domeny. Na przykład załóżmy, że który mimo że klaster ma 9 węzły w domenach błędów 3 (0, 1 i 2) i usługi ma repliki 3, polega węzły, które są stosowane do tych replik błędów domen 1 i 2 w dół i z powodu problemów z pojemnością były ważne żaden z innych węzłów z tych domen. W przypadku usługi tkaninie tworzenie zastępują tych replik, Menedżer zasobów klaster woli umieść je w domenie błąd 0, ale który tworzy sytuacji, gdy naruszone ograniczenie domeny błędów. Zwiększa prawdopodobieństwo, że zestawie całego mogą zostać utracone (w przypadku FD 0 jest permananently utracone). (Aby uzyskać więcej informacji na temat ograniczeń i ograniczenia priorytetów ogólnie rzecz biorąc, zapoznaj się [w tym temacie](service-fabric-cluster-resource-manager-management-integration.md#constraint-priorities) )

Jeśli kiedykolwiek zobaczył ostrzeżenie kondycji, takie jak "równoważenia obciążenia wykrył naruszenie ograniczenia replice: tkaninie:-<some service name> drugiej <some partition ID> narusza ograniczenie: FaultDomain" po naciśnięciu ten warunek lub podobną go. Zazwyczaj są to przejściowych (węzły nie loguj się w dół długo lub jeśli wykonują i trzeba utworzyć poprawnej jest węzłach w domenach poprawne błędów, które są ważne), ale istnieją pewne obciążeń, czy wolisz handlu dostępność ryzyka utraty ich repliki. Firma Microsoft można to zrobić, określając zasadę "RequireDomainDistribution", która będzie zagwarantować nie dwie repliki z tym samym partition kiedykolwiek możliwość w tej samej domeny błędów lub uaktualnianie.

Niektóre obciążenia ogół woli docelowej liczba replik (kopii stan) w każdym czasie (ze przed awariami sumy domeny i wiedzy, że są zwykle odzyskać stan lokalnego), inne osoby czy wolisz podjąć przestoje starsze niż ryzyka dotyczy poprawności i dataloss. Ponieważ większość obciążenia produkcji uruchomić z więcej niż 3 replikami, wartością domyślną jest nie wymagają rozkładu domeny i umożliwić równoważenia i pracy awaryjnej zwykle obsługiwać przypadki, nawet jeśli oznacza to, że tymczasowo domena ma wiele replik spakowany do niego.

Kod:

```csharp
ServicePlacementRequireDomainDistributionPolicyDescription distributeDomain = new ServicePlacementRequireDomainDistributionPolicyDescription();
serviceDescription.PlacementPolicies.Add(distributeDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("RequiredDomainDistribution")
```

Teraz może być możliwe używać tych konfiguracji dla usług w klastrze, która nie została geograficznie łączone? Czy użytkownik może! Ale nie jest doskonałe powodu zbyt — zwłaszcza konfiguracji domeny wymagane, nieprawidłowe i preferowanej należy unikać chyba że faktycznie pracujesz w klastrze geograficznie łączone — nie zrozumiałe wszelkie próby wymusić danego obciążenie pracą, aby uruchomić w stojaków pojedynczej lub Preferuj niektórych części lokalne klaster przed innego, chyba że jest różnego rodzaju segmentacji sprzętu lub Obciążenie pracą, przechodząc , i tych przypadkach można rozwiązać za pomocą ograniczeń położenie normalny.

## <a name="next-steps"></a>Następne kroki
- Więcej informacji na temat innych opcji dostępnych do skonfigurowania usług wyewidencjonowanie temat z konfiguracji Menedżera zasobów klaster dostępne [informacje na temat konfigurowania usług](service-fabric-cluster-resource-manager-configure-services.md)

[Image1]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-invalid-placement-domain.png
[Image2]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-required-placement-domain.png
[Image3]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-preferred-primary-domain.png
