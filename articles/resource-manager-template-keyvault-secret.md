<properties
   pageTitle="Szablon Menedżera zasobów dla hasła w klucza magazynu | Microsoft Azure"
   description="Pokazuje schemat Menedżera zasobów do wdrażania hasła klucza magazynu za pomocą szablonu."
   services="azure-resource-manager,key-vault"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/23/2016"
   ms.author="tomfitz"/>

# <a name="key-vault-secret-template-schema"></a>Schematu szablonu tajnego klucza magazynu

Tworzy klucz tajny, który jest przechowywany w klucza magazynu. Ten typ zasobu jest często wdrożona jako zasoby podrzędne [klucza magazynu](resource-manager-template-keyvault.md).

## <a name="schema-format"></a>Format schematu

Aby utworzyć hasło klucza magazynu, należy dodać następującego schematu do szablonu. Hasło można zdefiniować jako zasobu podrzędne z magazynu kluczy lub zasobów najwyższego poziomu. Można zdefiniować go jako zasobu podrzędny po wdrożeniu klucza magazynu w tym samym szablonie. Należy zdefiniować hasło jako zasób najwyższego poziomu, gdy klucza magazynu nie został wdrożony w tym samym szablonie, lub gdy trzeba utworzyć wiele hasła w pętli na typ zasobu. 

    {
        "type": enum,
        "apiVersion": "2015-06-01",
        "name": string,
        "properties": {
            "value": string
        },
        "dependsOn": [ array values ]
    }

## <a name="values"></a>Wartości

W poniższych tabelach opisano wartości, które należy ustawić w schemacie.

| Nazwa | Wartość |
| ---- | ---- | 
| Typ | Wyliczenia<br />Wymagane<br />**hasła** (jeśli jest używany jako zasoby podrzędne klucza magazynu) lub<br /> **Microsoft.KeyVault/vaults/secrets** (jeśli jest używany jako zasób najwyższego poziomu)<br /><br />Typ zasobu, aby utworzyć. |
| apiVersion | Wyliczenia<br />Wymagane<br />**2015-06-01** lub **2014-12-19-Podgląd**<br /><br />Wersja interfejsu API służących do tworzenia zasobu. | 
| Nazwa | Ciąg<br />Wymagane<br />Jednego wyrazu po wdrożeniu jako zasób podrzędne klucza magazynu lub w formacie **{ruch nazwa klucza magazynu} / {tajny nazwa}** po wdrożeniu jako najwyższego poziomu zasób do dodania do istniejącego magazynu kluczy.<br /><br />Nazwa tego hasła, aby utworzyć. |
| właściwości | Obiekt<br />Wymagane<br />[właściwości obiektu](#properties)<br /><br />Obiekt, który określa wartość hasła w celu utworzenia. |
| dependsOn | Tablica<br />Opcjonalne<br />Rozdzielaną średnikami listę zasobu nazwy lub unikatowe identyfikatory zasobów.<br /><br />Kolekcja zasobów, które zależy od tego łącza. Jeśli klucza magazynu dla tego hasła zostanie wdrożony w tym samym szablonie, obejmować tego elementu, aby upewnić się, że jest używany najpierw nazwę klucza magazynu. |

<a id="properties" />
### <a name="properties-object"></a>właściwości obiektu

| Nazwa | Wartość |
| ---- | ---- | 
| wartość | Ciąg<br />Wymagane<br /><br />Tajny wartość do przechowywania w klucza magazynu. Podczas przekazywania wartości tej właściwości, należy użyć parametru typu **securestring**.  |

    
## <a name="examples"></a>Przykłady

Pierwszy przykład wdraża tajne jako zasoby podrzędne klucza magazynu.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "keyVaultName": {
                "type": "string",
                "metadata": {
                    "description": "Name of the vault"
                }
            },
            "tenantId": {
                "type": "string",
                "metadata": {
                   "description": "Tenant ID for the subscription and use assigned access to the vault. Available from the Get-AzureRmSubscription PowerShell cmdlet"
                }
            },
            "objectId": {
                "type": "string",
                "metadata": {
                    "description": "Object ID of the AAD user or service principal that will have access to the vault. Available from the Get-AzureRmADUser or the Get-AzureRmADServicePrincipal cmdlets"
                }
            },
            "keysPermissions": {
                "type": "array",
                "defaultValue": [ "all" ],
                "metadata": {
                    "description": "Permissions to grant user to keys in the vault. Valid values are: all, create, import, update, get, list, delete, backup, restore, encrypt, decrypt, wrapkey, unwrapkey, sign, and verify."
                }
            },
            "secretsPermissions": {
                "type": "array",
                "defaultValue": [ "all" ],
                "metadata": {
                    "description": "Permissions to grant user to secrets in the vault. Valid values are: all, get, set, list, and delete."
                }
            },
            "vaultSku": {
                "type": "string",
                "defaultValue": "Standard",
                "allowedValues": [
                    "Standard",
                    "Premium"
                ],
                "metadata": {
                    "description": "SKU for the vault"
                }
            },
            "enabledForDeployment": {
                "type": "bool",
                "defaultValue": false,
                "metadata": {
                    "description": "Specifies if the vault is enabled for VM or Service Fabric deployment"
                }
            },
            "enabledForTemplateDeployment": {
                "type": "bool",
                "defaultValue": false,
                "metadata": {
                    "description": "Specifies if the vault is enabled for ARM template deployment"
                }
            },
            "enableVaultForVolumeEncryption": {
                "type": "bool",
                "defaultValue": false,
                "metadata": {
                    "description": "Specifies if the vault is enabled for volume encryption"
                }
            },
            "secretName": {
                "type": "string",
                "metadata": {
                    "description": "Name of the secret to store in the vault"
                }
            },
            "secretValue": {
                "type": "securestring",
                "metadata": {
                    "description": "Value of the secret to store in the vault"
                }
            }
        },
        "resources": [
        {
            "type": "Microsoft.KeyVault/vaults",
            "name": "[parameters('keyVaultName')]",
            "apiVersion": "2015-06-01",
            "location": "[resourceGroup().location]",
            "tags": {
                "displayName": "KeyVault"
            },
            "properties": {
                "enabledForDeployment": "[parameters('enabledForDeployment')]",
                "enabledForTemplateDeployment": "[parameters('enabledForTemplateDeployment')]",
                "enabledForVolumeEncryption": "[parameters('enableVaultForVolumeEncryption')]",
                "tenantId": "[parameters('tenantId')]",
                "accessPolicies": [
                {
                    "tenantId": "[parameters('tenantId')]",
                    "objectId": "[parameters('objectId')]",
                    "permissions": {
                        "keys": "[parameters('keysPermissions')]",
                        "secrets": "[parameters('secretsPermissions')]"
                    }
                }],
                "sku": {
                    "name": "[parameters('vaultSku')]",
                    "family": "A"
                }
            },
            "resources": [
            {
                "type": "secrets",
                "name": "[parameters('secretName')]",
                "apiVersion": "2015-06-01",
                "properties": {
                    "value": "[parameters('secretValue')]"
                },
                "dependsOn": [
                    "[concat('Microsoft.KeyVault/vaults/', parameters('keyVaultName'))]"
                ]
            }]
        }]
    }

Drugi przykład wdraża hasło jako zasób najwyższego poziomu, który jest przechowywany w istniejącej magazynu kluczy.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "keyVaultName": {
                "type": "string",
                "metadata": {
                    "description": "Name of the existing vault"
                }
            },
            "secretName": {
                "type": "string",
                "metadata": {
                    "description": "Name of the secret to store in the vault"
                }
            },
            "secretValue": {
                "type": "securestring",
                "metadata": {
                    "description": "Value of the secret to store in the vault"
                }
            }
        },
        "variables": {},
        "resources": [
            {
                "type": "Microsoft.KeyVault/vaults/secrets",
                "apiVersion": "2015-06-01",
                "name": "[concat(parameters('keyVaultName'), '/', parameters('secretName'))]",
                "properties": {
                    "value": "[parameters('secretValue')]"
                }
            }
        ],
        "outputs": {}
    }


## <a name="next-steps"></a>Następne kroki

- Aby uzyskać ogólne informacje na temat magazynami najważniejszych zobacz [rozpocząć pracę z magazynu klucza Azure](./key-vault/key-vault-get-started.md).
- Przykład odwoływanie się do hasła klucza magazynu wdrażanie szablonów zobacz [przekazania bezpiecznego wartości podczas wdrażania](resource-manager-keyvault-parameter.md).


