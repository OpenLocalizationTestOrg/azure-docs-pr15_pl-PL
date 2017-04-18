<properties
    pageTitle="Zmienianie identyfikator dzierżawy klucza magazynu przeniesienia subskrypcji | Microsoft Azure"
    description="Dowiedz się, jak przełączyć identyfikator dzierżawy klucza magazynu po subskrypcji jest przenoszony do innej dzierżawy"
    services="key-vault"
    documentationCenter=""
    authors="amitbapat"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/13/2016"
    ms.author="ambapat"/>

# <a name="change-a-key-vault-tenant-id-after-a-subscription-move"></a>Zmienianie Identyfikatora dzierżawy klucza magazynu przeniesienia subskrypcji
### <a name="q-my-subscription-was-moved-from-tenant-a-to-tenant-b-how-do-i-change-the-tenant-id-for-my-existing-key-vault-and-set-correct-acls-for-principals-in-tenant-b"></a>P: subskrypcji został przeniesiony z dzierżawy odpowiedzi do dzierżawy B. Jak zmienić identyfikator dzierżawy do mojej istniejącej magazynu kluczy i ustaw poprawne ACL głównych w dzierżawie B?

Po utworzeniu nowego magazynu klucza w subskrypcji automatycznie jest związany domyślny identyfikator dzierżawy usługi Azure Active Directory dla tej subskrypcji. Wszystkie wpisy zasady dostępu są również powiązane z tym identyfikatorem dzierżawy. Po przeniesieniu subskrypcji Azure z dzierżawy A do B dzierżawy usługi istniejących magazynami najważniejszych są niedostępne przez podmioty (użytkowników i aplikacji) w dzierżawie B. Aby rozwiązać ten problem, należy:

- Zmień identyfikator dzierżawy skojarzone z wszystkich istniejących kluczy magazynami w tej subskrypcji do dzierżawy B.
- Usuń wszystkie istniejące wpisy zasady dostępu.
- Dodawanie nowych wpisów zasady dostępu skojarzonych z dzierżawy B.

Na przykład jeśli masz klucza magazynu "myvault" w subskrypcji, która została przeniesiona z dzierżawy odpowiedzi do dzierżawcy B, Oto jak zmienić identyfikator dzierżawy dla tego magazynu kluczy i usuń starą zasady dostępu.

<pre>
$vaultResourceId = (get AzureRmKeyVault - VaultName myvault). ResourceId $vault = Get-AzureRmResource — ResourceId $vaultResourceId - ExpandProperties $vault. Properties.TenantId = (Get-AzureRmContext). Tenant.TenantId $vault. Properties.AccessPolicies = @() Set AzureRmResource - ResourceId $vaultResourceId-właściwości $vault. Właściwości
</pre>

Ponieważ tego magazynu w dzierżawie A przed jej przeniesieniem oryginalną wartość **$vault. Properties.TenantId** to: A dzierżawy, podczas **(Get-AzureRmContext). Tenant.TenantId** jest dzierżawa B.

Teraz, gdy z magazynu jest skojarzony z Identyfikatorem poprawne dzierżawy i stare wpisy zasady dostępu są usuwane, należy ustawić nowy dostęp wpisy zasad z [Zestawu AzureRmKeyVaultAccessPolicy](https://msdn.microsoft.com/library/mt603625.aspx).

## <a name="next-steps"></a>Następne kroki

Jeśli masz pytania dotyczące magazynu klucza Azure, odwiedź [Fora magazynu klucza Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault).
