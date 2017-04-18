<properties
   pageTitle="Szablon Menedżera zasobów dla klucza magazynu | Microsoft Azure"
   description="Pokazuje schemat Menedżera zasobów do wdrażania magazynami najważniejszych za pomocą szablonu."
   services="azure-resource-manager,key-vault"
   documentationCenter="na"
   authors="tfitzmac"
   manager="wpickett"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/23/2016"
   ms.author="tomfitz"/>

# <a name="key-vault-template-schema"></a>Schematu szablonu klucza magazynu

Tworzy klucza magazynu.

## <a name="schema-format"></a>Format schematu

Aby utworzyć klucza magazynu, Dodaj następujące schematu do sekcji Zasoby szablonu.

    {
        "type": "Microsoft.KeyVault/vaults",
        "apiVersion": "2015-06-01",
        "name": string,
        "location": string,
        "properties": {
            "enabledForDeployment": bool,
            "enabledForTemplateDeployment": bool,
            "enabledForVolumeEncryption": bool,
            "tenantId": string,
            "accessPolicies": [
                {
                    "tenantId": string,
                    "objectId": string,
                    "permissions": {
                        "keys": [ keys permissions ],
                        "secrets": [ secrets permissions ]
                    }
                }
            ],
            "sku": {
                "name": enum,
                "family": "A"
            }
        },
        "resources": [
             child resources
        ]
    }

## <a name="values"></a>Wartości

W poniższych tabelach opisano wartości, które należy ustawić w schemacie.

| Nazwa | Wartość |
| ---- | ---- | 
| Typ | Wyliczenia<br />Wymagane<br />**Microsoft.KeyVault/vaults**<br /><br />Typ zasobu, aby utworzyć. |
| apiVersion | Wyliczenia<br />Wymagane<br />**2015-06-01** lub **2014-12-19-Podgląd**<br /><br />Wersja interfejsu API służących do tworzenia zasobu. | 
| Nazwa | Ciąg<br />Wymagane<br />Nazwa, która jest unikatowy między Azure.<br /><br />Nazwa klucza magazynu, aby utworzyć. Warto rozważyć użycie funkcji [uniqueString](resource-group-template-functions.md#uniquestring) z Konwencja nazewnictwa utworzyć unikatową nazwę, jak pokazano w poniższym przykładzie. |
| Lokalizacja | Ciąg<br />Wymagane<br />Obszar prawidłowych magazynami najważniejszych. Aby określić prawidłową regionów, zobacz [obsługiwane regionów](resource-manager-supported-services.md#supported-regions).<br /><br />Region do obsługi klucza magazynu. |
| właściwości | Obiekt<br />Wymagane<br />[właściwości obiektu](#properties)<br /><br />Obiekt, który określa typ klucza magazynu, aby utworzyć. |
| zasoby | Tablica<br />Opcjonalne<br />Dopuszczalne wartości: [zasoby tajnego klucza magazynu](resource-manager-template-keyvault-secret.md)<br /><br />Zasoby podrzędne dla klucza magazynu. |

<a id="properties" />
### <a name="properties-object"></a>właściwości obiektu

| Nazwa | Wartość |
| ---- | ---- | 
| enabledForDeployment | Wartość logiczna<br />Opcjonalne<br />**wartość PRAWDA** lub **FAŁSZ**<br /><br />Określa, czy magazyn jest włączone do wdrożenia maszyn wirtualnych lub tkaninie usługi. |
| enabledForTemplateDeployment | Wartość logiczna<br />Opcjonalne<br />**wartość PRAWDA** lub **FAŁSZ**<br /><br />Określa, czy magazyn jest włączone do użycia we wdrożeniach szablonu Menedżera zasobów. Aby uzyskać więcej informacji zobacz [przekazania bezpiecznego wartości podczas wdrażania](resource-manager-keyvault-parameter.md) |
| enabledForVolumeEncryption | Wartość logiczna<br />Opcjonalne<br />**wartość PRAWDA** lub **FAŁSZ**<br /><br />Określa, czy magazyn jest włączone szyfrowania głośność. |
| tenantId | Ciąg<br />Wymagane<br />**Globalnie unikatowy identyfikator**<br /><br />Identyfikator dzierżawy dla subskrypcji. Można je podjąć za pomocą polecenia cmdlet [Get-AzureRmSubscription](https://msdn.microsoft.com/library/azure/mt619284.aspx) programu PowerShell lub **Pokaż konto azure** polecenie polecenie Azure. |
| accessPolicies | Tablica<br />Wymagane<br />[Obiekt accessPolicies](#accesspolicies)<br /><br />Tablica do 16 obiekty, które określają uprawnienia dla użytkownika lub wystawcy usługi. |
| Jednostka SKU | Obiekt<br />Wymagane<br />[Jednostka SKU obiektu](#sku)<br /><br />Jednostka SKU dla klucza magazynu. |

<a id="accesspolicies" />
### <a name="propertiesaccesspolicies-object"></a>Obiekt properties.accessPolicies

| Nazwa | Wartość |
| ---- | ---- | 
| tenantId | Ciąg<br />Wymagane<br />**Globalnie unikatowy identyfikator**<br /><br />Identyfikator dzierżawy dzierżawy usługi Azure Active Directory, zawierającej **identyfikator obiektu** tej zasady dostępu |
| Identyfikator obiektu | Ciąg<br />Wymagane<br />**Globalnie unikatowy identyfikator**<br /><br />Identyfikator użytkownika usługi Azure Active Directory lub wystawcy usługi, które będą mieli dostęp do magazynu. Wartość można pobrać z [Get-AzureRmADUser](https://msdn.microsoft.com/library/azure/mt679001.aspx) lub polecenia cmdlet [Get-AzureRmADServicePrincipal](https://msdn.microsoft.com/library/azure/mt678992.aspx) programu PowerShell lub **azure ad użytkownika** lub **sp azure ad** Azure poleceń interfejsu wiersza polecenia. |
| uprawnienia | Obiekt<br />Wymagane<br />[uprawnienia obiektu](#permissions)<br /><br />Uprawnienia w tym magazynu do obiektu usługi Active Directory. |

<a id="permissions" />
### <a name="propertiesaccesspoliciespermissions-object"></a>Obiekt properties.accessPolicies.permissions

| Nazwa | Wartość |
| ---- | ---- | 
| klawisze | Tablica<br />Wymagane<br />**Wszystkie**, **kopii zapasowej**, **Tworzenie**, **odszyfrowywanie**, **Usuwanie**, **Szyfrowanie**, **Uzyskaj**, **Importowanie**, **listy**, **Przywracanie**, **logowania**, **unwrapkey**, **Aktualizowanie**, **Sprawdź**i **wrapkey**<br /><br />Uprawnienia dla kluczy w tym magazynu do tego obiektu usługi Active Directory. Ta wartość jest wymagane w postaci tablicy co najmniej jeden dozwolonych wartości. |
| hasła | Tablica<br />Wymagane<br />**Wszystkie**, **Usuwanie**, **Pobieranie**, **listy**, **Ustawianie**<br /><br />Uprawnienia na hasła tego magazynu do tego obiektu usługi Active Directory. Ta wartość jest wymagane w postaci tablicy co najmniej jeden dozwolonych wartości. |

<a id="sku" />
### <a name="propertiessku-object"></a>Obiekt Properties.SKU

| Nazwa | Wartość |
| ---- | ---- | 
| Nazwa | Wyliczenia<br />Wymagane<br />**Standard**lub **premium** <br /><br />Warstwa usługi KeyVault korzystać.  Standardowe obsługuje oprogramowanie chronione klawiszy i hasła.  Premium dodaje obsługę chroniony HSM klawiszy. |
| Rodzina | Wyliczenia<br />Wymagane<br />**A** <br /><br />Rodzina sku korzystać. |
 
    
## <a name="examples"></a>Przykłady

Poniższy przykład wdraża klucza magazynu i hasło.

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

## <a name="quickstart-templates"></a>Szablony Szybki Start

Następujący szablon Szybki Start wdraża klucza magazynu.

- [Tworzenie magazynu klucza](https://azure.microsoft.com/documentation/templates/101-key-vault-create/)


## <a name="next-steps"></a>Następne kroki

- Aby uzyskać ogólne informacje na temat magazynami najważniejszych zobacz [rozpocząć pracę z magazynu klucza Azure](./key-vault/key-vault-get-started.md).
- Przykład odwoływanie się do hasła klucza magazynu wdrażanie szablonów zobacz [przekazania bezpiecznego wartości podczas wdrażania](resource-manager-keyvault-parameter.md).

