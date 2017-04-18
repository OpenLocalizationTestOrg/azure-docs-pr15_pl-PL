<properties
   pageTitle="Skalowanie klaster ACS z polecenie Azure | Microsoft Azure"
   description="Jak skalowanie klaster usługa kontenera Azure za pomocą interfejsu wiersza polecenia Azure."
   services="container-service"
   documentationCenter=""
   authors="Thraka"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, kontenery, Micro usług, Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/03/2016"
   ms.author="timlt"/>

# <a name="scale-an-azure-container-service"></a>Skala usługi Azure kontenera

Można skalować limit liczby węzłów stosowanych usługi Azure kontenera ACS (Service) za pomocą narzędzia polecenie Azure. Gdy używasz polecenie Azure przeskalować zwraca nowy plik konfiguracji reprezentującą zastosowane zmiany na kontenerze.

## <a name="about-the-command"></a>Polecenie — informacje

Polecenie Azure musi być w trybie Menedżera zasobów Azure umożliwia interakcję z kontenerami Azure. Można przełączyć do trybu Menedżera zasobów, dzwoniąc `azure config mode arm`. `acs` Polecenia jest podrzędnego — polecenie o nazwie `scale` wykonuje wszystkie operacje skali usługi kontener. Można uzyskać pomoc dotyczącą różnych parametry używane w polecenia Skaluj, uruchamiając `azure acs scale --help`, który wyświetla podobną do następującej:

```azurecli
azure acs scale --help

help:    The operation to scale a container service.
help:
help:    Usage: acs scale [options] <resource-group> <name> <new-agent-count>
help:
help:    Options:
help:      -h, --help                               output usage information
help:      -v, --verbose                            use verbose output
help:      -vv                                      more verbose with debug output
help:      --json                                   use json output
help:      -g, --resource-group <resource-group>    resource-group
help:      -n, --name <name>                        name
help:      -o, --new-agent-count <new-agent-count>  New agent count
help:      -s, --subscription <subscription>        The subscription identifier
help:
help:    Current Mode: arm (Azure Resource Management)
```

## <a name="use-the-command-to-scale"></a>Skalowanie przy użyciu polecenia

Przeskalować usługi kontenera, należy najpierw ustalić **Grupa zasobów** i **Nazwa Azure kontenera ACS (Service)**, a także określić liczba nowych czynników. Korzystając z kwotą mniejszych lub nowszy, można skalować w dół lub odpowiednio w górę.

Warto wiedzieć jakie bieżąca liczba czynników kontenerze usługi przed skalowania. Używanie `azure acs show <resource group> <ACS name>` polecenie, aby zwrócić konfiguracji ACS. Uwaga wynik <mark>Statystyka</mark> .

#### <a name="see-current-count"></a>Zobacz bieżąca liczba

```azurecli
azure acs show containers-test containerservice-containers-test

info:    Executing command acs show
data:
data:     Id                 : /subscriptions/<guid>/resourceGroups/containers-test/providers/Microsoft.ContainerService/containerServices/containerservice-containers-test
data:     Name               : containerservice-containers-test
data:     Type               : Microsoft.ContainerService/ContainerServices
data:     Location           : westus
data:     ProvisioningState  : Succeeded
data:     OrchestratorProfile
data:       OrchestratorType : DCOS
data:     MasterProfile
data:       Count            : 1
data:       DnsPrefix        : myprefixmgmt
data:       Fqdn             : myprefixmgmt.westus.cloudapp.azure.com
data:     AgentPoolProfiles
data:       #0
data:         Name           : agentpools
data:         <mark>Count          : 1</mark>
data:         VmSize         : Standard_D2
data:         DnsPrefix      : myprefixagents
data:         Fqdn           : myprefixagents.westus.cloudapp.azure.com
data:     LinuxProfile
data:       AdminUsername    : azureuser
data:       Ssh
data:         PublicKeys
data:           #0
data:             KeyData    : ssh-rsa <ENCODED VALUE>
data:     DiagnosticsProfile
data:       VmDiagnostics
data:         Enabled        : true
data:         StorageUri     : https://<storageid>.blob.core.windows.net/
```  

#### <a name="scale-to-new-count"></a>Skalowanie do nowego liczby

Prawdopodobnie jest oczywiste, można skalować usługi kontenera, dzwoniąc `azure acs scale` i dostarczenie **Grupa zasobów**, **Nazwa ACS**i **Liczba agenta grupy**. Skalowanie usługi kontenera polecenie Azure zwraca ciąg JSON reprezentującą nowej konfiguracji usługi kontenera, w tym nowego licznika agenta.

```azurecli
azure acs scale containers-test containerservice-containers-test 10

info:    Executing command acs scale
data:    {
data:        id: '/subscriptions/<guid>/resourceGroups/containers-test/providers/Microsoft.ContainerService/containerServices/containerservice-containers-test',
data:        name: 'containerservice-containers-test',
data:        type: 'Microsoft.ContainerService/ContainerServices',
data:        location: 'westus',
data:        provisioningState: 'Succeeded',
data:        orchestratorProfile: { orchestratorType: 'DCOS' },
data:        masterProfile: {
data:            count: 1,
data:            dnsPrefix: 'myprefixmgmt',
data:            fqdn: 'myprefixmgmt.westus.cloudapp.azure.com'
data:        },
data:        agentPoolProfiles: [
data:            {
data:                name: 'agentpools',
data:                <mark>count: 10</mark>,
data:                vmSize: 'Standard_D2',
data:                dnsPrefix: 'myprefixagents',
data:                fqdn: 'myprefixagents.westus.cloudapp.azure.com'
data:            }
data:        ],
data:        linuxProfile: {
data:            adminUsername: 'azureuser',
data:            ssh: {
data:                publicKeys: [
data:                    { keyData: 'ssh-rsa <ENCODED VALUE>' }
data:                ]
data:            }
data:        },
data:        diagnosticsProfile: {
data:            vmDiagnostics: { enabled: true, storageUri: 'https://<storageid>.blob.core.windows.net/' }
data:        }
data:    }
info:    acs scale command OK
``` 

## <a name="next-steps"></a>Następne kroki

- [Wdrażanie klastrze](container-service-deployment.md)