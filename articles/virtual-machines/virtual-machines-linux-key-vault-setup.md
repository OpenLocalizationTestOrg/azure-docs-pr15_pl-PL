<properties
    pageTitle="Konfigurowanie magazynu klucz dla maszyn wirtualnych w Menedżerze zasobów Azure | Microsoft Azure"
    description="Jak skonfigurować klucz magazynu do użytku z maszyn wirtualnych Azure Menedżera zasobów."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/31/2016"
    ms.author="singhkay"/>

# <a name="set-up-key-vault-for-virtual-machines-in-azure-resource-manager"></a>Konfigurowanie magazynu klucz dla maszyn wirtualnych w Menedżerze zasobów Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]model klasyczny wdrażania

W stosie Menedżera zasobów Azure hasła i certyfikaty są zaprojektowany jako zasoby, które są dostarczane przez dostawcę zasobu klucza magazynu. Aby uzyskać więcej informacji na temat magazynu klucza Azure, zobacz [Co to jest Azure klucza magazynu?](../key-vault/key-vault-whatis.md)

Aby dla klucza magazynu do użycia w środowisku maszyn wirtualnych systemu Azure Menedżera zasobów, należy ustawić właściwość *EnabledForDeployment* magazynu klucza na PRAWDA. Możesz to zrobić w klientach różnych."

## <a name="use-cli-to-set-up-key-vault"></a>Konfigurowanie klucza magazynu przy użyciu interfejsu wiersza polecenia
Aby utworzyć klucza magazynu przy użyciu interfejsu wiersza polecenia (polecenie), zobacz [Zarządzanie klucza magazynu przy użyciu interfejsu wiersza polecenia](../key-vault/key-vault-manage-with-cli.md#create-a-key-vault).

Dla interfejsu wiersza polecenia należy utworzyć klucza magazynu przed przypisać zasady wdrożenia. Możesz to zrobić przy użyciu następującego polecenia:

    azure keyvault set-policy ContosoKeyVault –enabled-for-deployment true

## <a name="use-templates-to-set-up-key-vault"></a>Konfigurowanie klucza magazynu przy użyciu szablonów
Korzystając z szablonu, należy ustawić `enabledForDeployment` właściwości `true` dla zasobu klucza magazynu.

    {
      "type": "Microsoft.KeyVault/vaults",
      "name": "ContosoKeyVault",
      "apiVersion": "2015-06-01",
      "location": "<location-of-key-vault>",
      "properties": {
        "enabledForDeployment": "true",
        ....
        ....
      }
    }

Aby uzyskać inne opcje, które można skonfigurować podczas tworzenia klucza magazynu przy użyciu szablonów zobacz [Tworzenie klucza magazynu](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).
