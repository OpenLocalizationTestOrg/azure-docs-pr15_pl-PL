<properties
   pageTitle="Interfejsy API pozostałych Menedżera zasobów | Microsoft Azure"
   description="Omówienie przykłady uwierzytelniania i zastosowania interfejsy API pozostałych Menedżera zasobów"
   services="azure-resource-manager"
   documentationCenter="na"
   authors="navalev"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/23/2016"
   ms.author="navale;tomfitz;"/>
   
# <a name="resource-manager-rest-apis"></a>Interfejsy API pozostałych Menedżera zasobów

> [AZURE.SELECTOR]
- [Azure programu PowerShell](powershell-azure-resource-manager.md)
- [Polecenie Azure](xplat-cli-azure-resource-manager.md)
- [Portal](./azure-portal/resource-group-portal.md) 
- [INTERFEJSU API USŁUGI REST](resource-manager-rest-api.md)

Za każdego połączenia do Menedżera zasobów Azure za każdy szablon wdrożonym za każdego konta skonfigurowanego miejsca do magazynowania jest jednego lub kilku połączenia do Menedżera zasobów Azure RESTful interfejsu API. W tym temacie jest poświęcona tych interfejsów API i jak je wywołać bez przy użyciu dowolnego zestawu SDK w ogóle. Może to być bardzo przydatne, jeśli chcesz, aby Pełna kontrola wszystkich wniosków o Azure lub jeśli SDK dla preferowany język nie jest dostępna lub nie obsługuje operacji, którą chcesz wykonać.

Ten artykuł nie zostaną przekierowane do każdej interfejs API, który będzie widoczna w Azure, ale wolisz użyć niektórych jako przykład jak wtyczce i łączyć się z nimi. Jeśli Poznaj podstawy można teraz i przeczytaj [Azure Menedżera zasobów pozostałych API Reference](https://msdn.microsoft.com/library/azure/dn790568.aspx) Aby znaleźć szczegółowe informacje dotyczące sposobu używania reszty za pośrednictwem interfejsów API.

## <a name="authentication"></a>Uwierzytelnianie
Uwierzytelnianie dla ARM jest obsługiwany przez Azure Active Directory (AD). Aby połączyć się wszystkich API, należy najpierw uwierzytelnienia z usługą Azure Active Directory, aby otrzymywać token uwierzytelniania, które można przekazać każdego żądania. Jak możemy zawierająca opis wyłącznie połączenia bezpośrednio do interfejsów API usługi REST również przyjmijmy że nie chcesz przeprowadzać uwierzytelnianie za pomocą hasła normalny nazwa_użytkownika miejsce, w którym ekranem-może być pytanie o nazwę użytkownika i hasło i może nawet inne mechanizmy uwierzytelniania używane w dwa scenariusze uwierzytelniania współczynnik. W związku z tym, zostanie utworzony co to jest nazywany aplikację Azure AD i kapitału usługa, która będzie używana do logowania przy użyciu. Jednak pamiętać, że Azure AD obsługuje kilku procedur uwierzytelniania i każdy z nich może być używane do pobierania token uwierzytelniania potrzebnej dla kolejne żądania interfejsu API.
Aby uzyskać instrukcje krok po kroku, wykonaj [Tworzenie Azure AD aplikacji i zasady usług](./resource-group-create-service-principal-portal.md) .

### <a name="generating-an-access-token"></a>Generowanie Token dostępu 
Uwierzytelniania Azure AD jest wykonywane przez nawiązywanie połączeń ze Azure AD znajdują się w login.microsoftonline.com. Aby można było uwierzytelnić muszą mieć następujące informacje:

* Azure AD identyfikator dzierżawy (nazwa tego Azure AD, którego używasz do logowania, często jako firmy, ale nie jest konieczne)
* Identyfikator aplikacji (wykonywane w kroku tworzenia aplikacji Azure AD)
* Hasło (wybranej podczas tworzenia aplikacji Azure AD)

W poniżej żądania HTTP upewnij się zamienić "Azure AD dzierżawy identyfikator", "Identyfikator aplikacji" i "Hasło" prawidłowe wartości.

**Rodzajowy żądania HTTP:**

```HTTP
POST /<Azure AD Tenant ID>/oauth2/token?api-version=1.0 HTTP/1.1 HTTP/1.1
Host: login.microsoftonline.com
Cache-Control: no-cache
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&resource=https%3A%2F%2Fmanagement.core.windows.net%2F&client_id=<Application ID>&client_secret=<Password>
```

... zostanie (Jeśli uwierzytelnianie zakończyło się powodzeniem) powodują podobne odpowiedź na to:

```json
{
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1448199959",
  "not_before": "1448196059",
  "resource": "https://management.core.windows.net/",
  "access_token": "eyJ0eXAiOiJKV1QiLCJhb...86U3JI_0InPUk_lZqWvKiEWsayA"
}
```
(Access_token w podanych odpowiedzi zostały skrócone celu zwiększenia czytelności)

**Generowanie token dostępu za pomocą imprezie:**

```console
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "grant_type=client_credentials&resource=https://management.core.windows.net&client_id=<application id>&client_secret=<password you selected for authentication>" https://login.microsoftonline.com/<Azure AD Tenant ID>/oauth2/token?api-version=1.0
```

**Generowanie token dostępu przy użyciu programu PowerShell:**

```powershell
Invoke-RestMethod -Uri https://login.microsoftonline.com/<Azure AD Tenant ID>/oauth2/token?api-version=1.0 -Method Post
 -Body @{"grant_type" = "client_credentials"; "resource" = "https://management.core.windows.net/"; "client_id" = "<application id>"; "client_secret" = "<password you selected for authentication>" }
```

Odpowiedź zawiera Token dostępu, informacje o tym, jak długo token jest prawidłowe i dowiedzieć się, jakie zasobów umożliwiają token dla.
Token dostępu, otrzymany w poprzednich połączeń HTTP muszą być przekazywane w dla wszystkich żądania API ARM jako nagłówek o nazwie "Autoryzacji" wartością "Okaziciela YOUR_ACCESS_TOKEN". Zwróć uwagę, odstęp między "Okaziciela" i Token dostępu.

Jak widać z powyższych czynności nie powoduje HTTP, tokenu jest prawidłowy dla danego okresu w którym należy pamięci podręcznej i ponownego użycia tego samego tokenu. Nawet jeśli jest możliwe do uwierzytelniania Azure AD dla każdego połączenia interfejsu API, może być bardzo mało wydajne.

## <a name="calling-arm-rest-apis"></a>Interfejsy API pozostałych ARM telefonicznej

[Poniżej opisano interfejsy API pozostałych Menedżera zasobów Azure](https://msdn.microsoft.com/library/azure/dn790568.aspx) , a także przez poza zakresem dla tego samouczka, aby zastosowania każdego i każdy dokument. Niniejsza dokumentacja będzie tylko przy użyciu kilku interfejsów API opisano podstawowe zastosowania interfejsów API, a następnie możemy możesz zapoznaj się z dokumentacją oficjalnym.

### <a name="list-all-subscriptions"></a>Lista wszystkich subskrypcji

Jednym najłatwiejszym operacji, które można wykonywać jest lista dostępnych subskrypcji, których będziesz mieć dostęp. W poniżej żądanie widać, jak Token dostępu jest przekazywany jako nagłówka.

(Zamiast YOUR_ACCESS_TOKEN usługi rzeczywistą dostęp Token).

```HTTP
GET /subscriptions?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json
```

... w wyniku spowoduje wyświetlenie listy subskrypcji, które może uzyskać dostęp do tego podmiotu usługi

(Identyfikatorów subskrypcji poniżej zostały skrócone czytelność tekstu)

```json
{
  "value": [
    {
      "id": "/subscriptions/3a8555...555995",
      "subscriptionId": "3a8555...555995",
      "displayName": "Azure Subscription",
      "state": "Enabled",
      "subscriptionPolicies": {
        "locationPlacementId": "Internal_2014-09-01",
        "quotaId": "Internal_2014-09-01"
      }
    }
  ]
}
```

### <a name="list-all-resource-groups-in-a-specific-subscription"></a>Lista wszystkich grup zasobów w określonej subskrypcji

Wszystkie zasoby dostępne za pośrednictwem interfejsów API ARM są zagnieżdżone wewnątrz grupy zasobów. Chwilę do kwerendy ARM dla istniejących grup zasobów naszych korzystania z subskrypcji poniżej uzyskiwanie żądanie HTTP. Zwróć uwagę, jak identyfikator subskrypcji jest przekazywany jako część adresu URL tym razem.

(Zamiast YOUR_ACCESS_TOKEN i IDENTYFIKATOR_SUBSKRYPCJI rzeczywisty Token dostępu i identyfikator subskrypcji)

```HTTP
GET /subscriptions/SUBSCRIPTION_ID/resourcegroups?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json
```

Odpowiedź zostanie wyświetlony zależy, czy masz żadnych grup zasobów zdefiniowane, a jeśli tak, ile.

(Identyfikatorów subskrypcji poniżej zostały skrócone czytelność tekstu)

```json
{
    "value": [
        {
            "id": "/subscriptions/3a8555...555995/resourceGroups/myfirstresourcegroup",
            "name": "myfirstresourcegroup",
            "location": "eastus",
            "properties": {
                "provisioningState": "Succeeded"
            }
        },
        {
            "id": "/subscriptions/3a8555...555995/resourceGroups/mysecondresourcegroup",
            "name": "mysecondresourcegroup",
            "location": "northeurope",
            "tags": {
                "tagname1": "My first tag"
            },
            "properties": {
                "provisioningState": "Succeeded"
            }
        }
    ]
}
```

### <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Pory firma Microsoft została tylko zostały wykonywania kwerend API ARM, aby uzyskać informacje, czas było zamiast tego utworzyć zasoby i Zacznijmy od najprostszych ze wszystkich, grupa zasobów. Następujące żądanie HTTP umożliwia utworzenie nowej grupy zasobów w regionie i lokalizacji wybranych przez użytkownika i dodaje do niej jeden lub więcej znaczników (przykładzie pod faktycznie tylko dodaje jeden tag).

(Zamiast YOUR_ACCESS_TOKEN, IDENTYFIKATOR_SUBSKRYPCJI, RESOURCE_GROUP_NAME z rzeczywisty Token dostępu, identyfikator subskrypcji i nazwę grupy zasobów, których chcesz utworzyć)

```HTTP
PUT /subscriptions/SUBSCRIPTION_ID/resourcegroups/RESOURCE_GROUP_NAME?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json

{
  "location": "northeurope",
  "tags": {
    "tagname1": "test-tag"
  }
}
```

Jeśli kończy się pomyślnie, zostanie wyświetlony podobne odpowiedź na to

```json
{
  "id": "/subscriptions/3a8555...555995/resourceGroups/RESOURCE_GROUP_NAME",
  "name": "RESOURCE_GROUP_NAME",
  "location": "northeurope",
  "tags": {
    "tagname1": "test-tag"
  },
  "properties": {
    "provisioningState": "Succeeded"
  }
}
```

Grupa zasobów została pomyślnie utworzona w Azure. Gratulacje!

### <a name="deploy-resources-to-a-resource-group-using-an-arm-template"></a>Wdrażanie zasobów do grupy zasobów przy użyciu szablonu ARM

Z ARM należy wdrożyć przy użyciu szablonów ARM zasobów. Szablon ARM definiuje kilka zasoby i ich zależności. W tej sekcji tylko Przyjmijmy zaznajomieni z szablonów ARM i będzie tylko pokażemy, jak możesz nawiązać połączenie interfejsu API, aby rozpocząć wdrożenia jednego. Szczegółową dokumentację ARM szablonów można znaleźć tutaj.

Wdrożenie szablonu ARM nie różnią się znacznie jak połączeń innych interfejsów API. Jednym z ważnych aspektów jest wdrożenie szablonu może potrwać sporo czasu, w zależności od tego, co to jest umieszczona w szablonie i tylko zwróci wywołanie interfejsu API oraz jest możesz jako deweloper można wyszukiwać stan wdrożenia Aby dowiedzieć się, po zakończeniu wdrożenia.

W tym przykładzie użyjemy publicznie dostępne ARM szablon dostępny na [GitHub](https://github.com/Azure/azure-quickstart-templates). Szablon, którego firma Microsoft będzie używany będzie wdrożyć maszyny Linux regionie Zachód USA. Mimo że tego szablonu będzie mieć szablon dostępny w repozytorium publicznej, takich jak GitHub, można też zaznaczyć przekazać pełny szablonu jako część żądania. Należy zauważyć, że firma Microsoft udostępnia jako część żądanie, które będą używane w szablonie używanych wartości parametru.

(Zamiast IDENTYFIKATOR_SUBSKRYPCJI, RESOURCE_GROUP_NAME, DEPLOYMENT_NAME, YOUR_ACCESS_TOKEN, GLOBALY_UNIQUE_STORAGE_ACCOUNT_NAME, ADMIN_USER_NAME, ADMIN_PASSWORD i DNS_NAME_FOR_PUBLIC_IP wartości odpowiednie dla Twojego żądania)

```HTTP
PUT /subscriptions/SUBSCRIPTION_ID/resourcegroups/RESOURCE_GROUP_NAME/providers/microsoft.resources/deployments/DEPLOYMENT_NAME?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json

{
  "properties": {
    "templateLink": {
      "uri": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-simple-linux-vm/azuredeploy.json",
      "contentVersion": "1.0.0.0",
    },
    "mode": "Incremental",
    "parameters": {
        "newStorageAccountName": {
          "value": "GLOBALY_UNIQUE_STORAGE_ACCOUNT_NAME"
        },
        "adminUsername": {
          "value": "ADMIN_USER_NAME"
        },
        "adminPassword": {
          "value": "ADMIN_PASSWORD"
        },
        "dnsNameForPublicIP": {
          "value": "DNS_NAME_FOR_PUBLIC_IP"
        },
        "ubuntuOSVersion": {
          "value": "15.04"
        }
    }
  }
}
```

Aby poprawić czytelność Niniejsza dokumentacja zostały pominięte bardzo długo odpowiedzi JSON dla tego żądania. Odpowiedź będzie zawierać informacji dotyczących wdrażania szablonu, która została właśnie utworzona.

