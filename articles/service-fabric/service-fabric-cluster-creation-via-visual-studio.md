<properties
   pageTitle="Aby skonfigurować klaster tkaninie usługi za pomocą programu Visual Studio | Microsoft Azure"
   description="Opisano, jak skonfigurować klaster tkaninie usługi przy użyciu Menedżera zasobów Azure szablonów utworzonych przez program project Azure grupa zasobów w programie Visual Studio"
   services="service-fabric"
   documentationCenter=".net"
   authors="karolz-ms"
   manager="adegeo"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/06/2016"
   ms.author="karolz@microsoft.com"/>

# <a name="set-up-a-service-fabric-cluster-by-using-visual-studio"></a>Konfigurowanie klastrze tkaninie usługi przy użyciu programu Visual Studio
W tym artykule opisano, jak skonfigurować klaster tkaninie usługi Azure za pomocą programu Visual Studio i szablonu Azure Menedżera zasobów. Aby utworzyć szablon użyjemy projektu programu Visual Studio Azure grupy zasobów. Po utworzeniu szablonu można wdrożyć bezpośrednio do Azure z programu Visual Studio. Można również używać, ze skryptu lub jako część funkcji integracji ciągły (CI).

## <a name="create-a-service-fabric-cluster-template-by-using-an-azure-resource-group-project"></a>Tworzenie szablonu klaster tkaninie usługi przy użyciu projekt grupa zasobów Azure
Aby rozpocząć, otwórz Visual Studio i utworzyć projekt grupy Azure zasobów (jest dostępny w folderze **chmury** ):

![Okno dialogowe Nowy projekt z wybranego projektu Azure grupa zasobów][1]

Można utworzyć nowe rozwiązanie programu Visual Studio dla tego projektu lub ją dodać do istniejącego rozwiązania.

>[AZURE.NOTE] Jeśli nie widzisz projektu grupy z zasobami Azure węźle chmury, nie masz SDK Azure zainstalowany. Uruchom Instalatora platformy sieci Web ([Zainstaluj ją teraz](http://www.microsoft.com/web/downloads/platform.aspx) Jeśli jeszcze nie masz) i wyszukaj "Azure SDK.NET" i zainstalować wersję, która jest zgodna z używaną wersją programu Visual Studio.

Po naciśnięciu przycisku OK Visual Studio wyświetli monit o wybierz szablon Menedżera zasobów, które chcesz utworzyć:

![Wybierz szablon Azure okno dialogowe z szablonu klaster tkaninie usługę wybranego][2]

Wybierz szablon klaster tkaninie usługi, a następnie ponownie kliknij przycisk OK. Teraz utworzono szablon Menedżera zasobów i projektu.

## <a name="prepare-the-template-for-deployment"></a>Przygotowywanie szablonu do wdrożenia
Przed wdrożeniem tego szablonu do utworzenia klaster, należy podać wartości parametrów wymagane szablonu. Te wartości parametrów są odczytywane z `ServiceFabricCluster.parameters.json` pliku, który znajduje się w `Templates` folderu projektu grupy z zasobami. Otwórz plik i podaj następujące wartości:

|Nazwa parametru           |Opis|
|-----------------------  |--------------------------|
|adminUserName            |Nazwa konta administratora dla komputerów tkaninie usługi (węzłach).|
|certificateThumbprint    |Odcisku palca certyfikatu, które będą zabezpieczać klaster.|
|sourceVaultResourceId    |*Identyfikator zasobu* klucza magazyn miejsce, w którym znajduje się certyfikat, który zabezpiecza klaster.|
|certificateUrlValue      |Adres URL certyfikat zabezpieczeń klaster.|

Szablon Menedżera zasobów tkaninie Visual Studio usługi tworzy bezpieczny klaster, który jest chroniony certyfikatu. Ten certyfikat jest identyfikowany przez parametry trzy ostatnie szablonu (`certificateThumbprint`, `sourceVaultValue`, i `certificateUrlValue`), i musi istnieć w **Azure klucza magazynu**. Aby uzyskać więcej informacji na temat tworzenia klaster certyfikat zabezpieczeń, zobacz artykuł [tkaninie usługi Klaster scenariusze zabezpieczeń](service-fabric-cluster-security.md#x509-certificates-and-service-fabric) .

## <a name="optional-change-the-cluster-name"></a>Opcjonalnie: Zmień nazwę klaster
Każdy klaster tkaninie usługi ma nazwę. Po utworzeniu klastrze tkaninie platformy Azure klaster nazwa określa (razem z obszaru Azure) systemu nazw domen (DNS) nazwę grupie. Na przykład jeśli nazwa klaster `myBigCluster`i (Azure region) grupa zasobów, w którym będzie przechowywana nowy klaster znajduje się wschodniego Stanów Zjednoczonych, nazwa DNS klaster będzie `myBigCluster.eastus.cloudapp.azure.com`.

Domyślnie nazwa klaster jest generowane automatycznie a unikatowe dołączając losowe sufiks do prefiks "klaster". Dzięki temu można łatwo korzystać z szablonu jako część systemu **ciągły integracji** (CI). Jeśli chcesz użyć nazwy określone dla klaster jest opisową, wartość `clusterName` zmiennych w pliku szablonu Menedżera zasobów (`ServiceFabricCluster.json`) do nazwy wybranego. To pierwsza zmienna zdefiniowane w tym pliku.

## <a name="optional-add-public-application-ports"></a>Opcjonalnie: Dodaj porty publicznej aplikacji
Ponadto można zmienić porty aplikacji publicznej grupie przed wdrożeniem tego. Domyślnie szablon zostanie otwarty w górę zaledwie dwóch publicznej porty TCP (80 i 8081). Jeśli potrzebujesz więcej aplikacji modyfikowania definicji równoważenia obciążenia Azure w szablonie. Definicja znajduje się w pliku szablonu głównym (`ServiceFabricCluster.json`). Otwórz plik i wyszukaj `loadBalancedAppPort`. Każdy port jest skojarzony z trzech artefakty:

1. Zmienna szablonu, który definiuje wartość port TCP portu:

    ```json
    "loadBalancedAppPort1": "80"
    ```

2. *Sonda* definiujące, jak często i ile czasu ładowania Azure równoważenia próbuje użyć określonego węzła tkaninie usługi przed awarii na inny. Sondy są częścią usługi równoważenia obciążenia zasobów. Poniżej przedstawiono definicję sondy pierwszego domyślny port aplikacji:

    ```json
    {
        "name": "AppPortProbe1",
        "properties": {
            "intervalInSeconds": 5,
            "numberOfProbes": 2,
            "port": "[variables('loadBalancedAppPort1')]",
            "protocol": "Tcp"
        }
    }
    ```

3. *Reguła równoważenia obciążenia* powiązanych ze sobą portu i sondy, umożliwiający równoważenia zestawu węzłach tkaninie usługi:

    ```json
    {
        "name": "AppPortLBRule1",
        "properties": {
            "backendAddressPool": {
                "id": "[variables('lbPoolID0')]"
            },
            "backendPort": "[variables('loadBalancedAppPort1')]",
            "enableFloatingIP": false,
            "frontendIPConfiguration": {
                "id": "[variables('lbIPConfig0')]"
            },
            "frontendPort": "[variables('loadBalancedAppPort1')]",
            "idleTimeoutInMinutes": 5,
            "probe": {
                "id": "[concat(variables('lbID0'),'/probes/AppPortProbe1')]"
            },
            "protocol": "Tcp"
        }
    }
    ```
Jeśli aplikacje, które ma zostać umieszczony z klastrem potrzebujesz więcej portów, możesz dodać je przez tworzenie dodatkowych sondy i równoważenia obciążenia definicjami reguł. Aby uzyskać więcej informacji na temat pracy z usługi równoważenia obciążenia Azure za pomocą Menedżera zasobów szablonów zobacz [Wprowadzenie do tworzenia równoważenia obciążenia wewnętrznych, przy użyciu szablonu](../load-balancer/load-balancer-get-started-ilb-arm-template.md).

## <a name="deploy-the-template-by-using-visual-studio"></a>Wdrażanie szablon przy użyciu programu Visual Studio
Po zapisaniu wszystkich wartości wymaganego parametru w`ServiceFabricCluster.param.dev.json` pliku, możesz przystąpić do wdrożenia szablon i Utwórz klaster tkaninie usługi. Kliknij prawym przyciskiem myszy projekt grupy zasobów w oknie Eksplorator rozwiązań Visual Studio i wybierz pozycję **Rozmieszczanie | Nowe wdrożenia...**. W razie potrzeby Visual Studio spowoduje wyświetlenie okna dialogowego **Rozmieszczanie do grupy zasobów** prośbą do uwierzytelniania Azure:

![Wdrażanie umożliwia otwarcie okna dialogowego Grupa zasobów][3]

Okno dialogowe umożliwia wybranie istniejącej grupy zasobów Menedżer zasobów dla klaster i udostępnia opcję, aby utworzyć nowy. Zwykle warto użyć oddzielnej grupie zasobów dla klastrów tkaninie usługi.

Po naciśnięciu przycisku rozmieszczanie programu Visual Studio wyświetli monit o potwierdzenie wartości parametrów szablonu. Kliknij przycisk **Zapisz** . Jeden parametr nie ma wartości trwałe: hasło konta administracyjnego klaster. Należy podać wartość hasła, gdy Visual Studio monituje o jeden.

>[AZURE.NOTE] Począwszy od Azure SDK 2.9 Visual Studio obsługuje haseł odczytu z **Magazynu klucza Azure** podczas wdrażania. W oknie dialogowym Parametry szablonu należy zauważyć, że `adminPassword` parametru pole tekstowe zawiera niewielką ikonę "klucz" po prawej stronie. Ta ikona umożliwia wybranie istniejącego hasła klucza magazynu jako hasło administratora dla klaster. Po prostu upewnij się, w celu umożliwienia pierwszego dostępu Menedżer zasobów Azure wdrożenia szablonu w zasadach zaawansowane dostępu do klucza magazynu. 

Można monitorować postęp procesu wdrażania w oknie dane wyjściowe programu Visual Studio. Po zakończeniu rozmieszczania szablonu nowy klaster jest gotowy do użycia!

>[AZURE.NOTE] Jeśli nigdy nie użyto programu PowerShell do zarządzania Azure z komputera, którego używasz, teraz, musisz wykonać nieco utrzymanie.
>1. Włączanie skryptów programu PowerShell, uruchamiając [`Set-ExecutionPolicy`](https://technet.microsoft.com/library/hh849812.aspx) polecenia. W przypadku komputerów rozwoju "nieograniczony" zasady są zazwyczaj zaakceptować.
>2. Zdecydować, czy zezwolić zbioru danych diagnostycznych z polecenia programu PowerShell Azure, a następnie uruchom [`Enable-AzureRmDataCollection`](https://msdn.microsoft.com/library/mt619303.aspx) lub [`Disable-AzureRmDataCollection`](https://msdn.microsoft.com/library/mt619236.aspx) stosownie do potrzeb. To pozwoli uniknąć niepotrzebne monitów podczas wdrażania szablonu.

Jeśli istnieją jakiekolwiek błędy, przejdź do [portalu Azure](https://portal.azure.com/) i otwórz grupa zasobów, które używany do. Kliknij pozycję **wszystkie ustawienia**, a następnie kliknij **wdrożeń** karta Ustawienia. Niepowodzenie wdrożenia grupa zasobów, które pozostawia szczegółowe informacje diagnostyczne.

>[AZURE.NOTE] Usługa tkaninie klastrów wymagają niektórych liczby węzłów należy się zachować dostępność i zachować state - określane jako "zachowaniu kworum". Dlatego nie jest bezpiecznie wyłączyć wszystkie komputery w klastrze chyba że najpierw wykonaniu [pełnej kopii zapasowej według stanu](service-fabric-reliable-services-backup-restore.md).

## <a name="next-steps"></a>Następne kroki
- [Więcej informacji na temat konfigurowania usługi tkaninie klaster za pomocą portalu Azure](service-fabric-cluster-creation-via-portal.md)
- [Dowiedz się, jak zarządzać i wdrażanie aplikacji usługi tkaninie przy użyciu programu Visual Studio](service-fabric-manage-application-in-visual-studio.md)

<!--Image references-->
[1]: ./media/service-fabric-cluster-creation-via-visual-studio/azure-resource-group-project-creation.png
[2]: ./media/service-fabric-cluster-creation-via-visual-studio/selecting-azure-template.png
[3]: ./media/service-fabric-cluster-creation-via-visual-studio/deploy-to-azure.png
