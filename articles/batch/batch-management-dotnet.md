<properties
    pageTitle="Konto zarządzania zasobami za pomocą .NET zarządzania wsadowe | Microsoft Azure"
    description="Tworzenie, usuwanie i modyfikowanie Azure partii koncie zasobów z biblioteką .NET zarządzania partii."
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="big-compute"
    ms.date="10/19/2016"
    ms.author="marsma"/>

# <a name="manage-azure-batch-accounts-and-quotas-with-batch-management-net"></a>Zarządzanie kontami partii Azure i przydziałów z partii zarządzania .NET

> [AZURE.SELECTOR]
- [Azure portal](batch-account-create-portal.md)
- [Zarządzanie partii .NET](batch-management-dotnet.md)

Można zmniejszyć konserwacji obciążenie w aplikacjach partii Azure za pomocą [Partii zarządzania .NET] [ api_mgmt_net] biblioteki, aby zautomatyzować tworzenie konta partię, usuwania, zarządzania kluczami i odnajdowanie przydziału.

- **Tworzenie i usuwanie kont partii** w dowolnym obszarze. Jeśli jako dostawca oprogramowania niezależnych (Model) na przykład podasz obsługi dla klientów, w których każda przypisano konto odrębne partii na potrzeby rozliczeń, możesz dodać konta możliwości tworzenia i usuwania z portalem klienta.
- **Pobieranie i klucze wyniku konta** programowy dla każdego z kont partii. To może ułatwić przestrzega zasad zabezpieczeń, wymuszające najazd okresowych lub wygaśnięcie klawiszy konta. Jeśli masz kilka kont partii w różnych regionach Azure automatyzacji tego procesu przerzucania zwiększa wydajność rozwiązania.
- **Sprawdzanie konta przydziałów** i sporządzanie czynności prób i błędów poza określanie konta partii, które mają jakie ograniczenia. Zaznaczając przydziałami konta przed rozpoczęciem zadania, tworzenie pul lub dodawanie węzłów obliczeń, gdzie można dostosować wyprzedzeniem lub podczas obliczania te zasoby są tworzone. Można określić, które konta wymagają przydziału zwiększa przed rozdzieleniem dodatkowe zasoby w tych kont.
- **Łączenie elementów innych usług Azure** dla środowiska wszystkimi funkcjami zarządzania — przy użyciu [Usługi Azure Active Directory]partii zarządzania .NET[aad_about]i [Menedżera zasobów Azure] [ resman_overview] razem w tej samej aplikacji. Za pomocą tych funkcji oraz ich interfejsy API, można zapewnić obsługi uwierzytelniania frictionless, możliwość tworzenia i usuwania grup zasobów i możliwości, które są opisane powyżej do rozwiązania do zarządzania zakończenia do końca.

> [AZURE.NOTE] Gdy w tym artykule omówiono programowych zarządzania kontami partię, klucze i przydziałów, można wykonać wiele z tych czynności za pomocą [Azure portal][azure_portal]. Aby uzyskać więcej informacji zobacz [Tworzenie konta partii Azure za pomocą portalu Azure](batch-account-create-portal.md) i [przydziałami i limity dotyczące usługi Azure partię](batch-quota-limit.md).

## <a name="create-and-delete-batch-accounts"></a>Tworzenie i usuwanie partii kont

Jak wspomniano, jednej z podstawowych funkcji interfejsu API zarządzania partii jest do tworzenia i usuwania kont partii w regionie Azure. W tym celu należy użyć [BatchManagementClient.Account.CreateAsync] [ net_create] i [DeleteAsync][net_delete], lub ich odpowiedników synchroniczne.

Poniższy fragment kodu tworzy konta, uzyskuje nowo utworzonych kont w usłudze partii i usuwa go. W tym wstawkę i innych osób, w tym artykule `batchManagementClient` jest w pełni zainicjowany wystąpienie [BatchManagementClient][net_mgmt_client].

```csharp
// Create a new Batch account
await batchManagementClient.Account.CreateAsync("MyResourceGroup",
    "mynewaccount",
    new BatchAccountCreateParameters() { Location = "West US" });

// Get the new account from the Batch service
AccountResource account = await batchManagementClient.Account.GetAsync(
    "MyResourceGroup",
    "mynewaccount");

// Delete the account
await batchManagementClient.Account.DeleteAsync("MyResourceGroup", account.Name);
```

> [AZURE.NOTE] Aplikacje, które używają biblioteki .NET zarządzania partii i jego klasy BatchManagementClient wymaga dostępu **administratora usługi** lub **coadministrator** do subskrypcję, do której właścicielem klienta partii do zarządzania. Aby uzyskać więcej informacji, zobacz sekcję [Usługi Azure Active Directory](#azure-active-directory) i [AccountManagement] [ acct_mgmt_sample] przykładowy kod.

## <a name="retrieve-and-regenerate-account-keys"></a>Pobieranie i ponownie wygenerować klucze konta

Uzyskiwanie kluczy podstawowych i pomocniczych konta z dowolnym kontem partii w ramach subskrypcji przy użyciu [ListKeysAsync][net_list_keys]. Te klucze można odtworzyć przy użyciu [RegenerateKeyAsync][net_regenerate_keys].

```csharp
// Get and print the primary and secondary keys
BatchAccountListKeyResult accountKeys =
    await batchManagementClient.Account.ListKeysAsync(
        "MyResourceGroup",
        "mybatchaccount");
Console.WriteLine("Primary key:   {0}", accountKeys.Primary);
Console.WriteLine("Secondary key: {0}", accountKeys.Secondary);

// Regenerate the primary key
BatchAccountRegenerateKeyResponse newKeys =
    await batchManagementClient.Account.RegenerateKeyAsync(
        "MyResourceGroup",
        "mybatchaccount",
        new BatchAccountRegenerateKeyParameters() {
            KeyName = AccountKeyType.Primary
            });
```

> [AZURE.TIP] Możesz utworzyć przepływ pracy usprawniony połączenia dla aplikacji zarządzania. Najpierw uzyskać klucz konta dla konta partii chcesz zarządzać usługą za pomocą [ListKeysAsync][net_list_keys]. Następnie należy użyć tego klucza podczas inicjowania biblioteki .NET wsadowe [BatchSharedKeyCredentials] [ net_sharedkeycred] klasy, która jest używana podczas inicjowania [BatchClient][net_batch_client].

## <a name="check-azure-subscription-and-batch-account-quotas"></a>Sprawdzanie subskrypcji Azure i przydziałów konta partii

Subskrypcje Azure i poszczególnych usług Azure, takich jak partii wszystkie mają przydziały domyślne, które ograniczyć liczbę niektórych obiektów w nich. Dla przydziałów domyślne dla subskrypcji Azure zobacz [Azure subskrypcji i limity dotyczące usługi, przydziały oraz ograniczenia](./../azure-subscription-service-limits.md). Domyślne przydziały usługi partii dla [przydziałów i limity dotyczące usługi Azure partię](batch-quota-limit.md). Za pomocą biblioteki .NET zarządzania partii, możesz sprawdzić tych przydziałów w aplikacjach. Umożliwia podejmowanie decyzji alokacji przed dodać konta lub obliczyć zasobów, takich jak pule i obliczyć węzły.

### <a name="check-an-azure-subscription-for-batch-account-quotas"></a>Sprawdzanie Azure subskrypcji dla przydziałów konta partii

Przed utworzeniem konta partii w regionie, możesz sprawdzić subskrypcji Azure, aby sprawdzić, czy użytkownik jest możliwość dodawania konta w danym regionie.

W poniższych wstawkę kodu najpierw używamy [BatchManagementClient.Account.ListAsync] [ net_mgmt_listaccounts] uzyskanie zbiór wszystkich kont partii, które są w ramach subskrypcji. Po możemy po uzyskaniu tej kolekcji określamy, ile konta są w regionie docelowej. Następnie użyjemy [BatchManagementClient.Subscriptions] [ net_mgmt_subscriptions] uzyskać przydziału konta partii i określanie, ile konta (jeśli istnieje) można utworzyć w danym regionie.

```csharp
// Get a collection of all Batch accounts within the subscription
BatchAccountListResponse listResponse =
        await batchManagementClient.Account.ListAsync(new AccountListParameters());
IList<AccountResource> accounts = listResponse.Accounts;
Console.WriteLine("Total number of Batch accounts under subscription id {0}:  {1}",
    creds.SubscriptionId,
    accounts.Count);

// Get a count of all accounts within the target region
string region = "westus";
int accountsInRegion = accounts.Count(o => o.Location == region);

// Get the account quota for the specified region
SubscriptionQuotasGetResponse quotaResponse = await batchManagementClient.Subscriptions.GetSubscriptionQuotasAsync(region);
Console.WriteLine("Account quota for {0} region: {1}", region, quotaResponse.AccountQuota);

// Determine how many accounts can be created in the target region
Console.WriteLine("Accounts in {0}: {1}", region, accountsInRegion);
Console.WriteLine("You can create {0} accounts in the {1} region.", quotaResponse.AccountQuota - accountsInRegion, region);
```

W powyższych wstawek `creds` jest wystąpieniem [TokenCloudCredentials][azure_tokencreds]. Aby zapoznać się z przykładem tworzenia tego obiektu, zobacz [AccountManagement] [ acct_mgmt_sample] kod przykładowy na GitHub.

### <a name="check-a-batch-account-for-compute-resource-quotas"></a>Zaznacz konto partii dla przydziałów zasobów obliczeń

Przed zwiększeniem zasobów obliczeń w rozwiązaniu partię, możesz sprawdzić, zapewniające zasoby, które mają być przydzielane nie może przekraczać przydziałów dla konta. W poniższych wstawkę kodu możemy drukowanie informacje o przydziałach dla konta partii o nazwie `mybatchaccount`. W aplikacji można użyć tych informacji do określenia, czy konto może obsługiwać dodatkowe zasoby do utworzenia.

```csharp
// First obtain the Batch account
BatchAccountGetResponse getResponse =
    await batchManagementClient.Account.GetAsync("MyResourceGroup", "mybatchaccount");
AccountResource account = getResponse.Resource;

// Now print the compute resource quotas for the account
Console.WriteLine("Core quota: {0}", account.Properties.CoreQuota);
Console.WriteLine("Pool quota: {0}", account.Properties.PoolQuota);
Console.WriteLine("Active job and job schedule quota: {0}", account.Properties.ActiveJobAndJobScheduleQuota);
```

> [AZURE.IMPORTANT] Jeśli istnieją domyślne przydziały dla subskrypcji Azure i usług, wiele z tych ograniczeń można podnieść wysyłając żądanie w [Azure portal][azure_portal]. Aby uzyskać instrukcje dotyczące zwiększania przydziałami konta partii na przykład zobacz [przydziałami i limity dotyczące usługi Azure partię](batch-quota-limit.md) .

## <a name="batch-management-net-azure-ad-and-resource-manager"></a>Zarządzanie .NET Azure AD partii i Menedżera zasobów

Podczas pracy z biblioteką .NET zarządzania partii zwykle również korzystasz z [Usługi Azure Active Directory] [ aad_about] (Azure AD) i [Menedżera zasobów Azure][resman_overview]. Przykładowy projekt omówiony poniżej jest używane zarówno usługi Azure Active Directory, jak i Menedżer zasobów podczas jego zaprezentowano API .NET zarządzania partii.

### <a name="azure-active-directory"></a>Azure Active Directory

Azure samej używa Azure AD dla uwierzytelniania swoich klientów, Administratorzy usługi i użytkowników w organizacji. W kontekście .NET zarządzania partii Azure AD służy do uwierzytelnienia administratorem subskrypcji lub Współtworzenie. Dzięki temu biblioteki zarządzania z kwerendy usługi partię, a następnie wykonaj czynności opisane w tym artykule.

W programie project przykładowe omówiony poniżej Azure [Active Directory Authentication Library] [ aad_adal] (ADAL) służy do monit o podanie poświadczeń firmy Microsoft. Gdy poświadczenia administratora lub coadministrator są dostarczane, aplikacji można wyszukiwać Azure listę subskrypcji — i można utworzyć i usunąć zarówno grup zasobów, jak i kont partii.

### <a name="resource-manager"></a>Menedżer zasobów

Po utworzeniu konta partii z biblioteką .NET zarządzania partii zostanie zwykle utworzony je w obrębie [grupy zasobów][resman_overview]. Grupa zasobów można utworzyć programowo przy użyciu [ResourceManagementClient] [ resman_client] klasy w [.NET Menedżera zasobów] [ resman_api] biblioteki. Lub możesz dodać konto do istniejącej grupy zasobów utworzonego wcześniej za pomocą [Azure portal][azure_portal].

## <a name="sample-project-on-github"></a>Przykładowy projekt na GitHub

Aby zobaczyć partii zarządzania .NET w działaniu, zapoznaj się z [AccountManagment] [ acct_mgmt_sample] przykładowy projekt na GitHub. Ta aplikacja konsoli Pokazuje tworzenia i zastosowania [BatchManagementClient] [ net_mgmt_client] i [ResourceManagementClient][resman_client]. Ilustruje też zastosowania Azure [Active Directory Authentication Library] [ aad_adal] (ADAL), wymagane przez obu klientów.

Aby pomyślnie uruchomić przykładowej aplikacji, należy najpierw zarejestrować go z usługą Azure Active Directory za pomocą portalu Azure. Postępuj zgodnie z instrukcjami w sekcji [Dodawanie aplikacji](../active-directory/active-directory-integrating-applications.md#adding-an-application) w [aplikacjach Integracja z usługą Azure Active Directory] [ aad_integrate] zarejestrować przykładowej aplikacji w ramach konta użytkownika w katalogu domyślne. Pamiętaj wybrać **Natywnej aplikacji klienckiej** dla typu aplikacji i można określić dowolny prawidłowy identyfikator URI (takie jak `http://myaccountmanagementsample`) dla **Przekierowania URI**— nie musi być rzeczywistą punktu końcowego.

Po dodaniu aplikacji, delegować uprawnienie **Zarządzanie usługą Azure programu Access jako organizacji** do aplikacji *Interfejs API systemu Windows Azure usługi zarządzania* w obszarze Ustawienia aplikacji w portalu:

![Uprawnienia aplikacji w Azure portal][2]

> [AZURE.TIP] **Interfejs API systemu Windows Azure usługi zarządzania** nie są wyświetlane w obszarze *uprawnienia do innych aplikacji*, kliknij pozycję **Dodaj aplikację**, wybierz **Interfejs API systemu Windows Azure usługi zarządzania**, a następnie kliknij przycisk znacznika wyboru. Następnie delegować uprawnienia określone powyżej.

Po dodaniu aplikacji zgodnie z powyższym opisem aktualizowanie `Program.cs` w [AccountManagment] [ acct_mgmt_sample] przykładowy projekt z aplikacji przekierowywać URI i identyfikator klienta. Na karcie **Konfigurowanie** aplikacji znajdują się te wartości:

![Konfiguracja aplikacji w Azure portal][3]

[AccountManagment] [ acct_mgmt_sample] aplikacja przykładowa prezentuje następujące operacje:

1. Uzyskiwanie tokenu zabezpieczającego z Azure AD przy użyciu [ADAL][aad_adal]. Jeśli użytkownik nie jest już zalogowany, są monitowani o podanie poświadczeń Azure.
2. Przy użyciu tokenu zabezpieczającego uzyskanego z Azure AD, utworzyć [SubscriptionClient] [ resman_subclient] do kwerendy Azure dla listy subskrypcji skojarzone z kontem. Dzięki temu zaznaczenia subskrypcji, jeśli wiele znajdują się.
3. Tworzenie obiektu poświadczeń skojarzonych z wybraną subskrypcję.
4. Tworzenie [ResourceManagementClient] [ resman_client] przy użyciu poświadczeń.
5. Za pomocą [ResourceManagementClient] [ resman_client] do utworzenia grupy zasobów.
6. Za pomocą [BatchManagementClient] [ net_mgmt_client] do wykonywania operacji konta na kilka partii:
  - Utwórz konto partii w nowej grupy zasobów.
  - Uzyskiwanie nowo utworzonego konta w usłudze partii.
  - Drukowanie klawiszy konta dla nowego konta.
  - Odtworzyć nowego klucza podstawowego dla konta.
  - Drukowanie informacje o przydziałach dla konta.
  - Drukowanie informacji o przydziałach dla subskrypcji.
  - Drukowanie wszystkich kont w obrębie subskrypcji.
  - Usuń nowo utworzone konto.
7. Usuwanie grupy zasobów.

Przed usunięciem tej nowo utworzone konta i zasobów grupy, można sprawdzić zarówno w [Azure portal][azure_portal]:

![Portal Azure wyświetlanie grupa zasobów i konta partii][1]
<br />
*Portal Azure wyświetlanie nowej grupy zasobów i konta partii*

[aad_about]: ../active-directory/active-directory-whatis.md "Co to jest Azure Active Directory?"
[aad_adal]: ../active-directory/active-directory-authentication-libraries.md
[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Scenariusze uwierzytelniania Azure AD"
[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Integracja aplikacji z usługą Azure Active Directory"
[acct_mgmt_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/AccountManagement
[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_mgmt_net]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[azure_portal]: http://portal.azure.com
[azure_storage]: https://azure.microsoft.com/services/storage/
[azure_tokencreds]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.tokencloudcredentials.aspx
[batch_explorer_project]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[net_batch_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_list_keys]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.listkeysasync.aspx
[net_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.createasync.aspx
[net_delete]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.deleteasync.aspx
[net_regenerate_keys]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.regeneratekeyasync.aspx
[net_sharedkeycred]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.auth.batchsharedkeycredentials.aspx
[net_mgmt_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.batchmanagementclient.aspx
[net_mgmt_subscriptions]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.batchmanagementclient.subscriptions.aspx
[net_mgmt_listaccounts]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.iaccountoperations.listasync.aspx
[resman_api]: https://msdn.microsoft.com/library/azure/mt418626.aspx
[resman_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.resourcemanagementclient.aspxs
[resman_subclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.subscriptions.subscriptionclient.aspx
[resman_overview]: ../azure-resource-manager/resource-group-overview.md

[1]: ./media/batch-management-dotnet/portal-01.png
[2]: ./media/batch-management-dotnet/portal-02.png
[3]: ./media/batch-management-dotnet/portal-03.png
