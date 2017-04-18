<properties
    pageTitle="Zarządzanie kontrola dostępu oparta na rolach przy użyciu interfejsu API usługi REST"
    description="Zarządzanie kontrola dostępu oparta na rolach przy użyciu interfejsu API usługi REST"
    services="active-directory"
    documentationCenter="na"
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="multiple"
    ms.tgt_pltfrm="rest-api"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="managing-role-based-access-control-with-the-rest-api"></a>Zarządzanie kontrola dostępu oparta na rolach przy użyciu interfejsu API usługi REST

> [AZURE.SELECTOR]
- [Programu PowerShell](role-based-access-control-manage-access-powershell.md)
- [Polecenie Azure](role-based-access-control-manage-access-azure-cli.md)
- [INTERFEJSU API USŁUGI REST](role-based-access-control-manage-access-rest.md)

Oparta na rolach programu Access Control (RBAC) w Azure Portal i interfejsu API Menedżera zasobów Azure ułatwia zarządzanie dostępem do subskrypcji i zasobów na poziomie szerokiego. Za pomocą tej funkcji można udzielić dostępu dla użytkowników, grup lub podmiotów usługi Active Directory, przypisując do nich niektóre role w określonym zakresie.

## <a name="list-all-role-assignments"></a>Lista wszystkich przypisania ról

Wyświetla listę wszystkich przypisania ról w określonym zakresie i subscopes.

Aby liście przypisania roli musi mieć dostęp do `Microsoft.Authorization/roleAssignments/read` operacji w tym zakresie. Wbudowane ról uzyskają dostęp do tej operacji. Aby uzyskać więcej informacji na temat przypisania ról i zarządzanie dostęp dla zasobów Azure zobacz [Kontrola dostępu Azure Role-Based](role-based-access-control-configure.md).

### <a name="request"></a>Żądanie

Za pomocą metody **GET** URI następujące czynności:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments?api-version={api-version}&$filter={filter}

W ramach identyfikatora URI wprowadź następujące elementy zastępcze, aby dostosować żądania:

1. Zamień *{zakresu}* zakres, dla którego chcesz wyświetlić przypisania roli. W poniższych przykładach pokazano, jak określić zakres dla różnych poziomach:

  - Subskrypcja: /subscriptions/ {identyfikator subskrypcji}  
  - Grupa zasobów: /subscriptions/ {identyfikator subskrypcji} / resourceGroups/myresourcegroup1  
  - Zasób: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. Zamień *{wersja api}* 2015-07-01.

3. *{Filtra}* należy zastąpić warunku, który ma zostać zastosowana do filtrowania listy przypisania ról:

  - Lista przypisania roli dla tylko określony zakres, z wyłączeniem przypisania ról w subscopes:`atScope()`    
  - Lista przypisania roli dla określonego użytkownika, grupy lub aplikacji:`principalId%20eq%20'{objectId of user, group, or service principal}'`  
  - Lista przypisania roli dla określonego użytkownika, między innymi odziedziczone grupy |`assignedTo('{objectId of user}')`

### <a name="response"></a>Odpowiedź

Kod stanu: 200

```
{
  "value": [
    {
      "properties": {
        "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7",
        "principalId": "2f9d4375-cbf1-48e8-83c9-2a0be4cb33fb",
        "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
        "createdOn": "2015-10-08T07:28:24.3905077Z",
        "updatedOn": "2015-10-08T07:28:24.3905077Z",
        "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
        "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
      },
      "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleAssignments/baa6e199-ad19-4667-b768-623fde31aedd",
      "type": "Microsoft.Authorization/roleAssignments",
      "name": "baa6e199-ad19-4667-b768-623fde31aedd"
    }
  ],
  "nextLink": null
}

```

## <a name="get-information-about-a-role-assignment"></a>Uzyskiwanie informacji na temat przypisania roli

Pobiera informacje o przypisanie roli jednej określonej przez identyfikator przypisania roli.

Aby uzyskać informacje o przypisanie roli, musi mieć dostęp do `Microsoft.Authorization/roleAssignments/read` operacji. Wbudowane ról uzyskają dostęp do tej operacji. Aby uzyskać więcej informacji na temat przypisania ról i zarządzanie dostęp dla zasobów Azure zobacz [Kontrola dostępu Azure Role-Based](role-based-access-control-configure.md).

### <a name="request"></a>Żądanie

Za pomocą metody **GET** URI następujące czynności:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

W ramach identyfikatora URI wprowadź następujące elementy zastępcze, aby dostosować żądania:

1. Zamień *{zakresu}* zakres, dla którego chcesz wyświetlić przypisania roli. W poniższych przykładach pokazano, jak określić zakres dla różnych poziomach:

  - Subskrypcja: /subscriptions/ {identyfikator subskrypcji}  
  - Grupa zasobów: /subscriptions/ {identyfikator subskrypcji} / resourceGroups/myresourcegroup1  
  - Zasób: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. Zamień *{ruch identyfikator roli przydziału}* identyfikator GUID przypisania roli.

3. Zamień *{wersja api}* 2015-07-01.

### <a name="response"></a>Odpowiedź

Kod stanu: 200

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c",
    "principalId": "672f1afa-526a-4ef6-819c-975c7cd79022",
    "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "createdOn": "2015-10-05T08:36:26.4014813Z",
    "updatedOn": "2015-10-05T08:36:26.4014813Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleAssignments/196965ae-6088-4121-a92a-f1e33fdcc73e",
  "type": "Microsoft.Authorization/roleAssignments",
  "name": "196965ae-6088-4121-a92a-f1e33fdcc73e"
}

```

## <a name="create-a-role-assignment"></a>Tworzenie przypisania roli

Tworzenie przypisania roli na określony zakres dla określonej kapitału udzielanie wybranej roli.

Aby utworzyć przypisanie roli, musi mieć dostęp do `Microsoft.Authorization/roleAssignments/write` operacji. Wbudowane ról tylko *właściciel* i *Administrator dostępu użytkowników* uzyskają dostęp do tej operacji. Aby uzyskać więcej informacji na temat przypisania ról i zarządzanie dostęp dla zasobów Azure zobacz [Kontrola dostępu Azure Role-Based](role-based-access-control-configure.md).

### <a name="request"></a>Żądanie

Użyj metody **umieszczenie** z URI następujące:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

W ramach identyfikatora URI wprowadź następujące elementy zastępcze, aby dostosować żądania:

1. Zamień *{zakresu}* zakresu, w którym chcesz utworzyć przypisania roli. Podczas tworzenia przypisania roli w zakresie nadrzędnej, wszystkie zakresy podrzędne dziedziczą same przypisania roli. W poniższych przykładach pokazano, jak określić zakres dla różnych poziomach:

  - Subskrypcja: /subscriptions/ {identyfikator subskrypcji}  
  - Grupa zasobów: /subscriptions/ {identyfikator subskrypcji} / resourceGroups/myresourcegroup1   
  - Zasób: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. Zamień *{ruch identyfikator roli przydziału}* nowy identyfikator GUID, która staje się identyfikator GUID nowe przypisanie roli.

3. Zamień *{wersja api}* 2015-07-01.

Dla zawartości żądania wprowadź wartości w następującym formacie:

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b"
  }
}

```

| Nazwa elementu     | Wymagane | Typ   | Opis |
|------------------|----------|--------|-------------|
| roleDefinitionId | Tak      | Ciąg | Identyfikator roli. Format identyfikatora jest:`{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id-guid}` |
| principalId      | Tak      | Ciąg | Identyfikator obiektu kapitału Azure AD (użytkownika, grupy lub wystawcy usługi), której przypisano roli. |

### <a name="response"></a>Odpowiedź

Kod stanu: 201

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b",
    "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND",
    "createdOn": "2015-12-16T00:27:19.6447515Z",
    "updatedOn": "2015-12-16T00:27:19.6447515Z",
    "createdBy": null,
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleAssignments/2e9e86c8-0e91-4958-b21f-20f51f27bab2",
  "type": "Microsoft.Authorization/roleAssignments",
  "name": "2e9e86c8-0e91-4958-b21f-20f51f27bab2"
}

```

## <a name="delete-a-role-assignment"></a>Usuwanie przypisania roli

Usuń przypisanie roli w określonym zakresie.

Aby usunąć przypisanie roli, musi mieć dostęp do `Microsoft.Authorization/roleAssignments/delete` operacji. Wbudowane ról tylko *właściciel* i *Administrator dostępu użytkowników* uzyskają dostęp do tej operacji. Aby uzyskać więcej informacji na temat przypisania ról i zarządzanie dostęp dla zasobów Azure zobacz [Kontrola dostępu Azure Role-Based](role-based-access-control-configure.md).

### <a name="request"></a>Żądanie

**Usuń** metodę za pomocą następujących identyfikatora URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

W ramach identyfikatora URI wprowadź następujące elementy zastępcze, aby dostosować żądania:

1. Zamień *{zakresu}* zakresu, w którym chcesz utworzyć przypisania roli. W poniższych przykładach pokazano, jak określić zakres dla różnych poziomach:

  - Subskrypcja: /subscriptions/ {identyfikator subskrypcji}  
  - Grupa zasobów: /subscriptions/ {identyfikator subskrypcji} / resourceGroups/myresourcegroup1  
  - Zasób: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. Zamień *{ruch identyfikator roli przydziału}* identyfikator GUID przypisania roli.

3. Zamień *{wersja api}* 2015-07-01.

### <a name="response"></a>Odpowiedź

Kod stanu: 200

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b",
    "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND",
    "createdOn": "2015-12-17T23:21:40.8921564Z",
    "updatedOn": "2015-12-17T23:21:40.8921564Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleAssignments/5eec22ee-ea5c-431e-8f41-82c560706fd2",
  "type": "Microsoft.Authorization/roleAssignments",
  "name": "5eec22ee-ea5c-431e-8f41-82c560706fd2"
}

```

## <a name="list-all-roles"></a>Lista wszystkich ról

Lista wszystkich ról, które są dostępne dla przydziału w określonym zakresie.

Do listy ról musi mieć dostęp do `Microsoft.Authorization/roleDefinitions/read` operacji w tym zakresie. Wbudowane ról uzyskają dostęp do tej operacji. Aby uzyskać więcej informacji na temat przypisania ról i zarządzanie dostęp dla zasobów Azure zobacz [Kontrola dostępu Azure Role-Based](role-based-access-control-configure.md).

### <a name="request"></a>Żądanie

Za pomocą metody **GET** URI następujące czynności:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions?api-version={api-version}&$filter={filter}

W ramach identyfikatora URI wprowadź następujące elementy zastępcze, aby dostosować żądania:

1. Zamień *{zakresu}* zakres, dla którego chcesz wyświetlić role. W poniższych przykładach pokazano, jak określić zakres dla różnych poziomach:

  - Subskrypcja: /subscriptions/ {identyfikator subskrypcji}  
  - Grupa zasobów: /subscriptions/ {identyfikator subskrypcji} / resourceGroups/myresourcegroup1  
  - /Subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1 zasobów  

2. Zamień *{wersja api}* 2015-07-01.

3. *{Filtra}* należy zastąpić warunku, który chcesz zastosować do filtrowania listy ról:

  - Role listy dostępne dla przydziałów w określonym zakresie oraz wszelkie z jego zakresów podrzędne:`atScopeAndBelow()`
  - Wyszukiwanie przy użyciu nazwy wyświetlanej konkretnej roli: `roleName%20eq%20'{role-display-name}'`. Za pomocą formularza zakodowany w adresie URL, nazwy wyświetlanej konkretnej roli. Na przykład`$filter=roleName%20eq%20'Virtual%20Machine%20Contributor'` |

### <a name="response"></a>Odpowiedź

Kod stanu: 200

```
{
  "value": [
    {
      "properties": {
        "roleName": "Virtual Machine Contributor",
        "type": "BuiltInRole",
        "description": "Lets you manage virtual machines, but not access to them, and not the virtual network or storage account they\u2019re connected to.",
        "assignableScopes": [
          "/"
        ],
        "permissions": [
          {
            "actions": [
              "Microsoft.Authorization/*/read",
              "Microsoft.Compute/availabilitySets/*",
              "Microsoft.Compute/locations/*",
              "Microsoft.Compute/virtualMachines/*",
              "Microsoft.Compute/virtualMachineScaleSets/*",
              "Microsoft.Insights/alertRules/*",
              "Microsoft.Network/applicationGateways/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatRules/join/action",
              "Microsoft.Network/loadBalancers/read",
              "Microsoft.Network/locations/*",
              "Microsoft.Network/networkInterfaces/*",
              "Microsoft.Network/networkSecurityGroups/join/action",
              "Microsoft.Network/networkSecurityGroups/read",
              "Microsoft.Network/publicIPAddresses/join/action",
              "Microsoft.Network/publicIPAddresses/read",
              "Microsoft.Network/virtualNetworks/read",
              "Microsoft.Network/virtualNetworks/subnets/join/action",
              "Microsoft.Resources/deployments/*",
              "Microsoft.Resources/subscriptions/resourceGroups/read",
              "Microsoft.Storage/storageAccounts/listKeys/action",
              "Microsoft.Storage/storageAccounts/read",
              "Microsoft.Support/*"
            ],
            "notActions": []
          }
        ],
        "createdOn": "2015-06-02T00:18:27.3542698Z",
        "updatedOn": "2015-12-08T03:16:55.6170255Z",
        "createdBy": null,
        "updatedBy": null
      },
      "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
      "type": "Microsoft.Authorization/roleDefinitions",
      "name": "9980e02c-c2be-4d73-94e8-173b1dc7cf3c"
    }
  ],
  "nextLink": null
}

```

## <a name="get-information-about-a-role"></a>Uzyskiwanie informacji na temat roli

Pobiera informacje o pojedynczej roli określonej przez identyfikator definicji roli. Aby uzyskać informacje o pojedynczej roli za pomocą nazwy wyświetlanej, zobacz [listę wszystkich ról](role-based-access-control-manage-access-rest.md#list-all-roles).

Aby uzyskać informacje o roli, musi mieć dostęp do `Microsoft.Authorization/roleDefinitions/read` operacji. Wbudowane ról uzyskają dostęp do tej operacji. Aby uzyskać więcej informacji na temat przypisania ról i zarządzanie dostęp dla zasobów Azure zobacz [Kontrola dostępu Azure Role-Based](role-based-access-control-configure.md).

### <a name="request"></a>Żądanie

Za pomocą metody **GET** URI następujące czynności:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

W ramach identyfikatora URI wprowadź następujące elementy zastępcze, aby dostosować żądania:

1. Zamień *{zakresu}* zakres, dla którego chcesz wyświetlić przypisania roli. W poniższych przykładach pokazano, jak określić zakres dla różnych poziomach:

  - Subskrypcja: /subscriptions/ {identyfikator subskrypcji}  
  - Grupa zasobów: /subscriptions/ {identyfikator subskrypcji} / resourceGroups/myresourcegroup1  
  - Zasób: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. Zamień *{ruch identyfikator roli definicji}* identyfikator GUID definicji roli.

3. Zamień *{wersja api}* 2015-07-01.

### <a name="response"></a>Odpowiedź

Kod stanu: 200

```
{
  "value": [
    {
      "properties": {
        "roleName": "Virtual Machine Contributor",
        "type": "BuiltInRole",
        "description": "Lets you manage virtual machines, but not access to them, and not the virtual network or storage account they\u2019re connected to.",
        "assignableScopes": [
          "/"
        ],
        "permissions": [
          {
            "actions": [
              "Microsoft.Authorization/*/read",
              "Microsoft.Compute/availabilitySets/*",
              "Microsoft.Compute/locations/*",
              "Microsoft.Compute/virtualMachines/*",
              "Microsoft.Compute/virtualMachineScaleSets/*",
              "Microsoft.Insights/alertRules/*",
              "Microsoft.Network/applicationGateways/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatRules/join/action",
              "Microsoft.Network/loadBalancers/read",
              "Microsoft.Network/locations/*",
              "Microsoft.Network/networkInterfaces/*",
              "Microsoft.Network/networkSecurityGroups/join/action",
              "Microsoft.Network/networkSecurityGroups/read",
              "Microsoft.Network/publicIPAddresses/join/action",
              "Microsoft.Network/publicIPAddresses/read",
              "Microsoft.Network/virtualNetworks/read",
              "Microsoft.Network/virtualNetworks/subnets/join/action",
              "Microsoft.Resources/deployments/*",
              "Microsoft.Resources/subscriptions/resourceGroups/read",
              "Microsoft.Storage/storageAccounts/listKeys/action",
              "Microsoft.Storage/storageAccounts/read",
              "Microsoft.Support/*"
            ],
            "notActions": []
          }
        ],
        "createdOn": "2015-06-02T00:18:27.3542698Z",
        "updatedOn": "2015-12-08T03:16:55.6170255Z",
        "createdBy": null,
        "updatedBy": null
      },
      "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
      "type": "Microsoft.Authorization/roleDefinitions",
      "name": "9980e02c-c2be-4d73-94e8-173b1dc7cf3c"
    }
  ],
  "nextLink": null
}

```

## <a name="create-a-custom-role"></a>Tworzenie roli niestandardowych
Tworzenie niestandardowego roli.

Aby utworzyć niestandardową rolę, musi mieć dostęp do `Microsoft.Authorization/roleDefinitions/write` operacji na wszystkich `AssignableScopes`. Wbudowane ról tylko *właściciel* i *Administrator dostępu użytkowników* uzyskają dostęp do tej operacji. Aby uzyskać więcej informacji na temat przypisania ról i zarządzanie dostęp dla zasobów Azure zobacz [Kontrola dostępu Azure Role-Based](role-based-access-control-configure.md).

### <a name="request"></a>Żądanie

Użyj metody **umieszczenie** z URI następujące:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

W ramach identyfikatora URI wprowadź następujące elementy zastępcze, aby dostosować żądania:

1. Zamień *{zakresu}* pierwszego *AssignableScope* roli niestandardowych. W poniższych przykładach pokazano, jak określić zakres dla różnych poziomów.

  - Subskrypcja: /subscriptions/ {identyfikator subskrypcji}  
  - Grupa zasobów: /subscriptions/ {identyfikator subskrypcji} / resourceGroups/myresourcegroup1  
  - Zasób: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. Zamień *{ruch identyfikator roli definicji}* nowy identyfikator GUID, która staje się identyfikator GUID nowej roli niestandardowych.

3. Zamień *{wersja api}* 2015-07-01.

Dla zawartości żądania wprowadź wartości w następującym formacie:

```
{
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "properties": {
    "roleName": "Virtual Machine Operator",
    "description": "Lets you monitor virtual machines and restart them.",
    "type": "CustomRole",
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ]
  }
}

```

| Nazwa elementu | Wymagane | Typ | Opis |
|--------------|----------|------|-------------|
| Nazwa         | Tak | Ciąg   | Identyfikator GUID niestandardowego roli.    |
| properties.roleName               | Tak | Ciąg   | Nazwa wyświetlana roli niestandardowych. Maksymalny rozmiar 128 znaków.                        |
| Properties.Description            | Brak  | Ciąg   | Opis roli niestandardowych. Maksymalny rozmiar 1024 znaków.                                               |
| Properties.Type                   | Tak | Ciąg   | Ustaw "CustomRole."                                         |
| Properties.permissions.Actions    | Tak | Ciąg] | Tablica ciągów akcję określającą operacje udzielane przez rolę niestandardowe.             |
| properties.permissions.notActions | Brak  | Ciąg] | Tablica ciągów akcję określającą operacje, aby wykluczyć z operacji udzielane przez rolę niestandardowe. |
| properties.assignableScopes       | Tak | Ciąg] | Tablica zakresów, w których można używać niestandardowych roli.   |

### <a name="response"></a>Odpowiedź

Kod stanu: 201

```
{
  "properties": {
    "roleName": "Virtual Machine Operator",
    "type": "CustomRole",
    "description": "Lets you monitor virtual machines and restart them.",
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ],
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "createdOn": "2015-12-18T00:10:51.4662695Z",
    "updatedOn": "2015-12-18T00:10:51.4662695Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "type": "Microsoft.Authorization/roleDefinitions",
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7"
}

```

## <a name="update-a-custom-role"></a>Aktualizowanie rolę niestandardową

Modyfikowanie rolę niestandardową.

Aby zmodyfikować rolę niestandardową, musi mieć dostęp do `Microsoft.Authorization/roleDefinitions/write` operacji na wszystkich `AssignableScopes`. Wbudowane ról tylko *właściciel* i *Administrator dostępu użytkowników* uzyskają dostęp do tej operacji. Aby uzyskać więcej informacji na temat przypisania ról i zarządzanie dostęp dla zasobów Azure zobacz [Kontrola dostępu Azure Role-Based](role-based-access-control-configure.md).

### <a name="request"></a>Żądanie

Użyj metody **umieszczenie** z następujących identyfikatora URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

W ramach identyfikatora URI wprowadź następujące elementy zastępcze, aby dostosować żądania:

1. Zamień *{zakresu}* pierwszego *AssignableScope* roli niestandardowych. W poniższych przykładach pokazano, jak określić zakres dla różnych poziomach:

  - Subskrypcja: /subscriptions/ {identyfikator subskrypcji}  
  - Grupa zasobów: /subscriptions/ {identyfikator subskrypcji} / resourceGroups/myresourcegroup1  
  - Zasób: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. Zamień *{ruch identyfikator roli definicji}* identyfikator GUID niestandardowego roli.

3. Zamień *{wersja api}* 2015-07-01.

Dla zawartości żądania wprowadź wartości w następującym formacie:

```
{
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "properties": {
    "roleName": "Virtual Machine Operator",
    "description": "Lets you monitor virtual machines and restart them.",
    "type": "CustomRole",
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ]
  }
}

```

| Nazwa elementu | Wymagane | Typ | Opis |
|--------------|----------|------|-------------|
| Nazwa         | Tak      | Ciąg | Identyfikator GUID niestandardowego roli. |
| properties.roleName | Tak | Ciąg | Nazwa wyświetlana zaktualizowanych roli niestandardowych. |
| Properties.Description | Brak | Ciąg | Opis roli niestandardowych zaktualizowane. |
| Properties.Type | Tak | Ciąg | Ustaw "CustomRole." |
| Properties.permissions.Actions | Tak | Ciąg] | Tablica ciągów akcję określającą operacje, do których zaktualizowane rola niestandardowe zapewnia dostęp. |
| properties.permissions.notActions | Brak | Ciąg] | Tablica ciągów akcję określającą operacje, aby wykluczyć z działań, które udziela zaktualizowane rolę niestandardową. |
| properties.assignableScopes | Tak | Ciąg] | Tablica zakresów, w których można używać zaktualizowanych rolę niestandardową. |

### <a name="response"></a>Odpowiedź

Kod stanu: 201

```
{
  "properties": {
    "roleName": "Virtual Machine Operator",
    "type": "CustomRole",
    "description": "Lets you monitor virtual machines and restart them.",
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ],
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "createdOn": "2015-12-18T00:10:51.4662695Z",
    "updatedOn": "2015-12-18T00:10:51.4662695Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "type": "Microsoft.Authorization/roleDefinitions",
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7"
}

```

## <a name="delete-a-custom-role"></a>Usuwanie niestandardowego roli

Usuwanie niestandardowego roli.

Aby usunąć rolę niestandardową, musi mieć dostęp do `Microsoft.Authorization/roleDefinitions/delete` operacji na wszystkich `AssignableScopes`. Wbudowane ról tylko *właściciel* i *Administrator dostępu użytkowników* uzyskają dostęp do tej operacji. Aby uzyskać więcej informacji na temat przypisania ról i zarządzanie dostęp dla zasobów Azure zobacz [Kontrola dostępu Azure Role-Based](role-based-access-control-configure.md).

### <a name="request"></a>Żądanie

**Usuń** metodę za pomocą URI następujące czynności:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

W ramach identyfikatora URI wprowadź następujące elementy zastępcze, aby dostosować żądania:

1. Zamień *{zakresu}* zakres, dla której chcesz usunąć definicji roli. W poniższych przykładach pokazano, jak określić zakres dla różnych poziomach:

  - Subskrypcja: /subscriptions/ {identyfikator subskrypcji}  
  - Grupa zasobów: /subscriptions/ {identyfikator subskrypcji} / resourceGroups/myresourcegroup1  
  - Zasób: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. Zamień *{ruch identyfikator roli definicji}* identyfikator GUID roli definicji roli niestandardowych.

3. Zamień *{wersja api}* 2015-07-01.

### <a name="response"></a>Odpowiedź

Kod stanu: 200

```
{
  "properties": {
    "roleName": "Virtual Machine Operator",
    "type": "CustomRole",
    "description": "Lets you monitor virtual machines and restart them.",
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ],
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "createdOn": "2015-12-16T00:07:02.9236555Z",
    "updatedOn": "2015-12-16T00:07:02.9236555Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/0bd62a70-e1b8-4e0b-a7c2-75cab365c95b",
  "type": "Microsoft.Authorization/roleDefinitions",
  "name": "0bd62a70-e1b8-4e0b-a7c2-75cab365c95b"
}

```


[AZURE.INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]
