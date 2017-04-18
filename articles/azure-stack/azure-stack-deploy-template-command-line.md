<properties
    pageTitle="Wdrażanie szablonów z poziomu wiersza polecenia w stos Azure | Microsoft Azure"
    description="Dowiedz się, jak wdrażanie szablonów z wewnątrz ClientVM lub po użyciu VPN nawiązać stos Azure za pomocą interfejsu wiersza polecenia i platform (polecenie)."
    services="azure-stack"
    documentationCenter=""
    authors="heathl17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="helaw"/>

# <a name="deploy-templates-in-azure-stack-using-the-command-line"></a>Wdrażanie szablonów w stos Azure za pomocą wiersza polecenia

Wdrażanie szablonów Azure Menedżera zasobów, aby Zapewnić stosem Azure za pomocą wiersza polecenia. Azure szablony Menedżera zasobów wdrażania i obsługi administracyjnej wszystkich zasobów dla aplikacji w jednym, skoordynowanego operacji.

## <a name="download-template"></a>Pobieranie szablonu        
Aby przetestować wdrożenie przy użyciu interfejsu wiersza polecenia, pobrać pliki azuredeploy.json i azuredeploy.parameters.json [Utwórz szablon przykład konta miejsca do magazynowania](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-create-storage-account).

## <a name="deploy-template"></a>Wdrażanie szablonu
Przejdź do folderu, gdzie te pliki były pobierane i uruchom następujące polecenie wdrożenie szablonu:

    azure group create "cliRG" "local" –f azuredeploy.json –d "testDeploy" –e azuredeploy.parameters.json

To polecenie wdraża szablon w grupie zasobów **cliRG** w lokalizacji domyślnej Zapewnić stosem Azure.

## <a name="validate-template-deployment"></a>Sprawdź poprawność rozmieszczania szablonu
Aby wyświetlić tego zasobu konta grupy i miejsca do magazynowania, należy użyć następujących poleceń:

    azure group list

    azure storage account list

## <a name="next-steps"></a>Następne kroki

[Zarządzanie uprawnieniami użytkowników](azure-stack-manage-permissions.md)
