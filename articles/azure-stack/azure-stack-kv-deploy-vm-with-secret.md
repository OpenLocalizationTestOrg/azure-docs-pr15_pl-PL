<properties
    pageTitle="Wdrażanie maszyny przy użyciu hasła przechowywane w Azure stos klucza magazynu | Microsoft Azure"
    description="Dowiedz się, jak wdrożyć maszyny przy użyciu hasła przechowywane w Azure stos klucza magazynu"
    services="azure-stack"
    documentationCenter=""
    authors="rlfmendes"
    manager="natmack"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/26/2016"
    ms.author="ricardom"/>

# <a name="deploy-a-vm-by-retrieving-the-password-stored-in-key-vault"></a>Wdrażanie maszyny pobierając hasło przechowywane w klucza magazynu

Gdy trzeba przekazać wartość bezpieczne (na przykład hasła) jako parametr podczas wdrażania można Przechowaj tę wartość jako poufne w stos Azure magazynu kluczy i odwoływać się wartości w innych szablonów Azure Menedżera zasobów. Możesz umieścić tylko odwołanie do tego hasła do szablonu, aby hasło nigdy nie są wyświetlane. Nie trzeba ręcznie wprowadź wartość dla tego hasła za każdym razem, wdrażanie zasobów. Można określić, którzy użytkownicy lub podmioty usługi można uzyskać dostęp do tego hasła.

## <a name="reference-a-secret-with-static-id"></a>Dokumentacja dotycząca hasło identyfikatora statyczne

Odwołanie tajny z w pliku parametrów przekazuje wartości do szablonu. Przekazując identyfikator zasobu klucza magazynu i nazwę tego hasła odwoływać się hasło. W tym przykładzie tajny klucza magazynu musi już istnieć. Używanie wartość statyczną dla jej identyfikator zasobu.

    "parameters": {
    "adminPassword": {
    "reference": {
    "keyVault": {
    "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
    },
    "secretName": "sqlAdminPassword"


>[AZURE.NOTE]Parametr, który akceptuje hasło należy *securestring*.

## <a name="next-steps"></a>Następne kroki
[Wdrażanie aplikacji przykładowych z magazynu klucza](azure-stack-kv-sample-app.md)

[Wdrażanie maszyn wirtualnych za pomocą certyfikatu klucza magazynu](azure-stack-kv-push-secret-into-vm.md)

