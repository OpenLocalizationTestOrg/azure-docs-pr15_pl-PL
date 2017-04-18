<properties
   pageTitle="Tworzenie równoważenia obciążenia wewnętrznych, przy użyciu szablonu w Menedżerze zasobów | Microsoft Azure"
   description="Dowiedz się, jak utworzyć równoważenia obciążenia wewnętrznych, przy użyciu szablonu w Menedżerze zasobów"
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

# <a name="create-an-internal-load-balancer-using-a-template"></a>Tworzenie równoważenia obciążenia wewnętrznych, przy użyciu szablonu

[AZURE.INCLUDE [load-balancer-get-started-ilb-arm-selectors-include.md](../../includes/load-balancer-get-started-ilb-arm-selectors-include.md)]
<BR>
[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][model klasyczny wdrożenia](load-balancer-get-started-ilb-classic-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="deploy-the-template-by-using-click-to-deploy"></a>Wdrażanie szablonu za pomocą kliknięcia do wdrożenia

Poniższy przykładowy szablon dostępny w publicznej repozytorium używa parametru plik zawierający domyślne wartości umożliwia generowanie scenariuszy opisanych powyżej. Aby wdrożyć ten szablon przy użyciu kliknij wdrażania, zrób [to łącze](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-internal-load-balancer), kliknij **Rozmieszczanie Azure**, Zamień wartości domyślne parametrów w razie potrzeby, a następnie postępuj zgodnie z instrukcjami w portalu.

## <a name="deploy-the-template-by-using-powershell"></a>Wdrażanie szablon przy użyciu programu PowerShell

Aby wdrożyć szablon, który został pobrany przy użyciu programu PowerShell, wykonaj poniższe czynności.

1. Jeśli po raz pierwszy używasz Azure programu PowerShell, zobacz, [jak instalowanie i konfigurowanie programu PowerShell Azure](../../articles/powershell-install-configure.md) i postępuj zgodnie z instrukcjami maksymalnie end, aby zalogować się do Azure i wybierz subskrypcję.
2. Pobierz plik parametrów na lokalnym dysku.
3. Edytowanie pliku i zapisz go.
4. Uruchom polecenie cmdlet **AzureRmResourceGroupDeployment nowy** , aby utworzyć grupę zasobów za pomocą tego szablonu.

        New-AzureRmResourceGroupDeployment -Name TestRG -Location westus `
            -TemplateFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json' `
            -TemplateParameterFile 'C:\temp\azuredeploy.parameters.json'

## <a name="deploy-the-template-by-using-the-azure-cli"></a>Wdrażanie szablonu za pomocą interfejsu wiersza polecenia Azure

Aby wdrożyć szablon przy użyciu interfejsu wiersza polecenia Azure, wykonaj poniższe czynności.

1. Jeśli po raz pierwszy używasz polecenie Azure, zobacz [Instalowanie i konfigurowanie polecenie Azure](../../articles/xplat-cli-install.md) i postępuj zgodnie z instrukcjami do miejsca, w którym wybierz swoje konto Azure i subskrypcji.
2. Uruchom polecenie **Konfiguracja azure tryb** , aby przełączyć się do trybu Menedżera zasobów, tak jak pokazano poniżej.

        azure config mode arm

    Oto oczekiwany wynik polecenia powyżej:

        info:    New mode is arm

3. Otwórz plik parametr, zaznacz jego zawartość i zapisanie go w pliku na komputerze. Na przykład możemy zapisano plik parametrów *parameters.json*.

4. Uruchom polecenie **Tworzenie grupy azure wdrożenia** wdrażania nowej usługi równoważenia obciążenia wewnętrznych przy użyciu szablonu i parametr pliki pobrane i modyfikować powyżej. Lista wyświetlana po wynik wyjaśniono parametry używane.

        azure group create --name TestRG --location westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json --parameters-file parameters.json

## <a name="next-steps"></a>Następne kroki

[Konfigurowanie trybu rozkładu równoważenia obciążenia przy użyciu źródła IP koligacji](load-balancer-distribution-mode.md)

[Konfigurowanie ustawienia przekroczenia limitu czasu bezczynności protokołu TCP dla usługi równoważenia obciążenia](load-balancer-tcp-idle-timeout.md)



