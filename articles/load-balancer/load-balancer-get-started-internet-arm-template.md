<properties
   pageTitle="Tworzenie internetowej przeciwległych równoważenia obciążenia w Menedżerze zasobów przy użyciu szablonu | Microsoft Azure"
   description="Dowiedz się, jak utworzyć internetową przeciwległych równoważenia obciążenia w Menedżerze zasobów przy użyciu szablonu"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="creating-an-internet-facing-load-balancer-using-a-template"></a>Tworzenie internetowej przeciwległych równoważenia obciążenia przy użyciu szablonu

[AZURE.INCLUDE [load-balancer-get-started-internet-arm-selectors-include.md](../../includes/load-balancer-get-started-internet-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]W tym artykule opisano, jak model wdrożenia Menedżera zasobów. Możesz także [dowiedzieć się, jak utworzyć internetową przeciwległych równoważenia obciążenia przy użyciu modelu Klasyczny wdrażania](load-balancer-get-started-internet-classic-portal.md)


[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploy-the-template-by-using-click-to-deploy"></a>Wdrażanie szablonu za pomocą kliknięcia do wdrożenia

Poniższy przykładowy szablon dostępny w publicznej repozytorium używa parametru plik zawierający domyślne wartości umożliwia generowanie scenariuszy opisanych powyżej. Aby wdrożyć ten szablon przy użyciu kliknij wdrażania, zrób [to łącze](http://go.microsoft.com/fwlink/?LinkId=544801), kliknij **Rozmieszczanie Azure**, Zamień wartości domyślne parametrów w razie potrzeby, a następnie postępuj zgodnie z instrukcjami w portalu.

## <a name="deploy-the-template-by-using-powershell"></a>Wdrażanie szablon przy użyciu programu PowerShell

Aby wdrożyć szablon, który został pobrany przy użyciu programu PowerShell, wykonaj poniższe czynności.

1. Jeśli po raz pierwszy używasz Azure programu PowerShell, zobacz, [jak instalowanie i konfigurowanie programu PowerShell Azure](../../articles/powershell-install-configure.md) i postępuj zgodnie z instrukcjami maksymalnie end, aby zalogować się do Azure i wybierz subskrypcję.

2. Uruchom polecenie cmdlet **AzureRmResourceGroupDeployment nowy** , aby utworzyć grupę zasobów za pomocą tego szablonu.

        New-AzureRmResourceGroupDeployment -Name TestRG -Location uswest `
            -TemplateFile 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json' `
            -TemplateParameterFile 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.parameters.json'

## <a name="deploy-the-template-by-using-the-azure-cli"></a>Wdrażanie szablonu za pomocą interfejsu wiersza polecenia Azure

Aby wdrożyć szablon przy użyciu interfejsu wiersza polecenia Azure, wykonaj poniższe czynności.

1. Jeśli po raz pierwszy używasz polecenie Azure, zobacz [Instalowanie i konfigurowanie polecenie Azure](../../articles/xplat-cli-install.md) i postępuj zgodnie z instrukcjami do miejsca, w którym wybierz swoje konto Azure i subskrypcji.
2. Uruchom polecenie **Konfiguracja azure tryb** , aby przełączyć się do trybu Menedżera zasobów, tak jak pokazano poniżej.

        azure config mode arm

    Oto oczekiwany wynik polecenia powyżej:

        info:    New mode is arm

3. Z poziomu przeglądarki przejdź do [Odpowiedniego szablonu Szybki Start](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-loadbalancer-lbrules), skopiuj zawartość pliku json i Wklej do nowego pliku na komputerze. W tym scenariuszu jest czy kopiowanie wartości mniejszych niż w pliku o nazwie **c:\lb\azuredeploy.parameters.json**.
4. Uruchom polecenie cmdlet **Tworzenie grupy azure wdrożenia** wdrażania nowej usługi równoważenia obciążenia przy użyciu szablonu i parametr pliki pobrane i modyfikować powyżej. Lista wyświetlana po wynik wyjaśniono parametry używane.

        azure group create -n TestRG -l westus -f 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json' -e 'c:\lb\azuredeploy.parameters.json'

## <a name="next-steps"></a>Następne kroki

[Przed rozpoczęciem konfigurowania równoważenia obciążenia wewnętrznych](load-balancer-get-started-ilb-arm-ps.md)

[Konfigurowanie trybu rozkładu równoważenia obciążenia](load-balancer-distribution-mode.md)

[Konfigurowanie ustawienia przekroczenia limitu czasu bezczynności protokołu TCP dla usługi równoważenia obciążenia](load-balancer-tcp-idle-timeout.md)
