<properties
    pageTitle="Jak skonfigurować klucza magazynu przy użyciu klucza obrotu kompleksowe i inspekcja | Microsoft Azure"
    description="Użyj tej instrukcji ułatwiające rozpoczęcie instalacji przy użyciu klucza obrót i monitorowania dzienników klucza magazynu"
    services="key-vault"
    documentationCenter=""
    authors="swgriffith"
    manager="mbaldwin"
    tags=""/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/05/2016"
    ms.author="jodehavi;stgriffi"/>
#<a name="how-to-setup-key-vault-with-end-to-end-key-rotation-and-auditing"></a>Jak skonfigurować klucza magazynu przy użyciu klucza obrotu kompleksowe i inspekcji

##<a name="introduction"></a>Wprowadzenie

Po utworzeniu magazynu klucza usługi Azure, można rozpocząć używanie tego magazynu do przechowywania kluczy i hasła. Aplikacji nie jest już potrzebny do innej witryny usługi kluczy lub hasła, lecz zażąda ich z magazynu klucza stosownie do potrzeb. Umożliwia aktualizowanie bez wpływania zachowanie aplikacji, która otwiera szerokości możliwości wokół klucza i sposób zarządzania tajne klucze i hasła.

W tym artykule opisano przykładem używanie Azure klucza magazynu do przechowywania hasła, w tym przypadku klucz konta na platformie Azure miejsca do magazynowania, który jest dostępny przez aplikację. Przedstawi się również stosowania zaplanowane obrót klucz konta miejsca do magazynowania. Na koniec przeprowadzi go przez pokaz dzienników inspekcji klucza magazynu oraz podnieść alerty, gdy nieoczekiwane żądań.

> \[AZURE. Uwaga\] tego samouczka nie ma na celu wyjaśniono nimi początkowym zestawem magazynu klucza usługi Azure szczegółowo. Informacje zobacz [Rozpoczynanie pracy z magazynu klucza Azure](key-vault-get-started.md). Lub, aby interfejs wiersza polecenia i Platform, zobacz [tego samouczka równoważne](key-vault-manage-with-cli.md).

##<a name="setting-up-keyvault"></a>Aby skonfigurować KeyVault

Aby włączyć aplikację do pobierania tajne z magazynu klucza Azure, należy najpierw utworzyć hasło i przekaż go do magazynu usługi. Można to osiągnąć łatwo za pomocą programu PowerShell przedstawionych poniżej.

Rozpocznij sesję programu PowerShell Azure i zaloguj się do konta Azure za pomocą następującego polecenia:

```powershell
Login-AzureRmAccount
```

W oknie podręcznym przeglądarki wprowadź Azure nazwa użytkownika i hasło. Azure programu PowerShell otrzymają wszystkie subskrypcje, które są skojarzone z tym kontem i domyślnie używa pierwszego.

Jeśli masz wiele subskrypcji, może być konieczne określanie określonej, który został użyty do utworzenia magazynu klucza usługi Azure. Wpisz poniższe czynności, aby wyświetlić subskrypcje dla Twojego konta:

```powershell
Get-AzureRmSubscription
```

Następnie aby określić subskrypcji, który jest skojarzony z usługi Magazyn klucza, który użytkownik będzie loguje, wpisz:

```powershell
Set-AzureRmContext -SubscriptionId <subscriptionID> 
```

Jak w tym artykule przedstawiono przechowywania klucza konta miejsca do magazynowania jako poufne, będą potrzebne do uzyskania klucz konta miejsca do magazynowania.

```powershell
Get-AzureRmStorageAccountKey -ResourceGroupName <resourceGroupName> -Name <storageAccountName>
```

Po pobraniu poufnych, w tym przypadku swój klucz konta miejsca do magazynowania, należy przekonwertować który bezpiecznego ciąg, a następnie utworzyć hasło z tej wartości na swojej klucza magazynu.

```powershell
$secretvalue = ConvertTo-SecureString <storageAccountKey> -AsPlainText -Force

Set-AzureKeyVaultSecret -VaultName <vaultName> -Name <secretName> -SecretValue $secretvalue
```
Następnie można uzyskać identyfikator URI hasła, które właśnie utworzony. Będzie można używać w późniejszym etapie zadzwonisz magazynu klucz, aby pobrać poufnych. Uruchom następujące polecenie programu PowerShell i zanotuj wartość "Identyfikator", czyli tajne identyfikatora URI.

```powershell
Get-AzureKeyVaultSecret –VaultName <vaultName>
```

##<a name="setting-up-application"></a>Konfigurowanie aplikacji

Teraz, gdy masz tajne przechowywane można pobrać tego hasła i używać go z kodu. Istnieje kilka czynności wymagane do osiągnięcia to pierwszy i najważniejsze z rejestrowania aplikacji z usługą Azure Active Directory, następnie informacją Azure klucza magazynu informacje o aplikacji, dlatego może ona umożliwiać żądania z aplikacji.

> \[AZURE. Uwaga\] aplikacji należy utworzyć w tej samej dzierżawy usługi Azure Active Directory jako magazynu usługi klucza. 

Najpierw otwórz kartę aplikacji usługi Azure Active Directory

![Otwarte aplikacje w Azure AD](./media/keyvault-keyrotation/AzureAD_Header.png)

Wybierz przycisk Dodaj, aby dodać nową aplikację do usługi Azure AD

![Wybierz pozycję Dodaj aplikację](./media/keyvault-keyrotation/Azure_AD_AddApp.png)

Pozostaw typ aplikacji jako "API sieci WEB i/lub aplikacji sieci Web", a następnie nadaj nazwę aplikacji.

![Nazwa aplikacji](./media/keyvault-keyrotation/AzureAD_NewApp1.png)

Nadaj aplikacji "Logowania jednokrotnego adres URL" i "identyfikator URI aplikacji". Te może być coś, co chcesz wyświetlić ten pokaz i będzie można później zmienić w razie potrzeby.

![Podaj wymagane identyfikatory URI](./media/keyvault-keyrotation/AzureAD_NewApp2.png)

Po dodaniu aplikacji do Azure AD będzie można przełączyć na stronie aplikacji. Od tego momentu kliknij kartę "Konfigurowanie", a następnie znajdź i skopiuj wartość "Identyfikator klienta". Zanotuj identyfikator klienta dla dalszych krokach.

Następnie należy wygenerować klucz dla aplikacji można było korzystać z usługi Azure AD. Można go utworzyć w sekcji "Klawisze" na karcie "Konfiguracja". Zanotuj klucz wygenerowanym aplikacji Azure AD do użycia w późniejszym etapie.

![Azure AD klawisze aplikacji](./media/keyvault-keyrotation/Azure_AD_AppKeys.png)

Przed ustanawianie wezwań z poziomu aplikacji do magazynu klucz będzie konieczne Opisz magazynu klawisz aplikacji i jego "uprawnienia. Następujące polecenie ma nazwę magazynu i identyfikator klienta z Twojej aplikacji Azure AD i udziela "Get" dostępu do swojego klucza magazynu dla aplikacji.

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <clientIDfromAzureAD> -PermissionsToSecrets Get
```

W tym miejscu możesz przystąpić do Rozpoczęcie tworzenia połączeń aplikacji. W aplikacji zostanie najpierw należy zainstalować pakiety NuGet wymagane współdziałanie z magazynu klucza Azure i usługi Azure Active Directory. Z poziomu konsoli Menedżera pakietów Visual Studio wpisz następujące polecenia. Należy zauważyć, że u zapisywanie w tym artykule bieżącą wersję pakietu pozycji 3.10.305231913, więc możesz potwierdzić najnowszej wersji i zaktualizować odpowiednio.

```powershell
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 3.10.305231913

Install-Package Microsoft.Azure.KeyVault
```

W kodzie aplikacji utworzyć klasę do przechowywania metody uwierzytelniania usługi Active Directory. W tym przykładzie, że klasy o nazwie "Witryny". Następnie należy dodać następujące za pomocą.

```csharp
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

Następnie dodaj następujące metody pobrać JWT token z Azure AD. Dla łatwość może chcesz przenieść twardych kodowane wartości ciągów do konfiguracji sieci web lub aplikacji.

```csharp
public async static Task<string> GetToken(string authority, string resource, string scope)
{
    var authContext = new AuthenticationContext(authority);

    ClientCredential clientCred = new ClientCredential("<AzureADApplicationClientID>","<AzureADApplicationClientKey>");

    AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

    if (result == null)

    throw new InvalidOperationException("Failed to obtain the JWT token");

    return result.AccessToken;
}
```

Na koniec możesz dodać wymagany kod do połączeń klucza magazynu i pobierania tajne wartość. Najpierw należy dodać następujące za pomocą instrukcji.

```csharp
using Microsoft.Azure.KeyVault;
```

Następną czynnością będzie dodanie połączeń metody wywołania klucza magazynu i pobierania poufnych. W tej metody można użyć zapewni tajny identyfikator URI, który został zapisany w poprzednim kroku. Należy zauważyć, użyj metody GetToken z klasy witryny utworzony w poprzednim przykładzie.
    
```csharp
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));

var sec = kv.GetSecretAsync(<SecretID>).Result.Value;
```

Po uruchomieniu aplikacji teraz powinno być uwierzytelniania usługi Azure Active Directory, a następnie pobierając tajne wartość z magazynu klucza usługi Azure.

##<a name="key-rotation-using-azure-automation"></a>Obrót klucza przy użyciu automatyzacji Azure

Dostępne są różne opcje stosowania strategii obrotu wartości, które zawierają jako Azure klucza magazynu hasła. Hasła można obracać w ramach procesu wykonywanego ręcznie, ich może być obrócony filmowych korzystając z interfejsu API lub może być obrócony drodze skrypt automatyzacji. Na potrzeby tego artykułu będziemy się używanie Azure programu PowerShell połączone z automatyzacji Azure, aby zmienić klucz dostępu do konta na platformie Azure, a następnie hasło klucza magazynu zostanie odpowiednio zaktualizowana z tego nowego klucza. 

W celu umożliwienia automatyzacji Azure do ustawiania wartości tajne w swojej klucza magazynu należy uzyskać identyfikator klienta dla połączenia o nazwie AzureRunAsConnection, które zostały utworzone podczas ustanowioną wystąpienia automatyzacji Azure. Możesz wyświetlić ten identyfikator, wybierając "Aktywa" z wystąpienia automatyzacji Azure. Stamtąd wybierz "połączenia, a następnie wybierz zasady usług"AzureRunAsConnection". Należy wziąć pod uwagę identyfikator aplikacji. 

![Identyfikator klienta automatyzacji Azure](./media/keyvault-keyrotation/Azure_Automation_ClientID.png)

Podczas pracy w oknie składniki majątku będzie również chcesz wybrać "Moduły". Z modułów będzie wybierz "Galerię", a następnie wyszukaj i 'Import' zaktualizowane wersje każdego z następujących modułów.

    Azure
    Azure.Storage   
    AzureRM.Profile
    AzureRM.KeyVault
    AzureRM.Automation
    AzureRM.Storage
    
> \[AZURE. Uwaga\] u zapisywanie w tym artykule tylko powyżej zauważyć moduły nie trzeba aktualizować skryptu udostępnione poniżej. Jeśli okaże się, że zadanie automatyzacji kończy się niepowodzeniem, należy się upewnić, czy masz wszystkie niezbędne moduły i współzależnościami zaimportowane.

Po pobraniu identyfikator aplikacji dla połączenia automatyzacji Azure, należy sprawdzić magazynu klucza usługi Azure, czy ta aplikacja ma dostęp do aktualizacji hasła z magazynu. Można to osiągnąć przy użyciu następującego polecenia programu PowerShell.

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <applicationIDfromAzureAutomation> -PermissionsToSecrets Set
```

Następnie możesz wybrać zasób "Runbooks" w obszarze wystąpienia automatyzacji Azure i wybierz pozycję "Dodaj działań aranżacji". Wybierz pozycję szybkie tworzenie. Nazwa Twojej działań aranżacji i wybierz pozycję "Programu PowerShell" jako typ działań aranżacji. Opcjonalnie można dodać opis. Na koniec kliknij przycisk "Utwórz".

![Tworzenie działań aranżacji](./media/keyvault-keyrotation/Create_Runbook.png)

W okienku edytor dla Twojej nowej działań aranżacji spowoduje wklejenie tego skryptu programu PowerShell.

```powershell
$connectionName = "AzureRunAsConnection"
try
{
    # Get the connection "AzureRunAsConnection "
    $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         

    "Logging in to Azure..."
    Add-AzureRmAccount `
        -ServicePrincipal `
        -TenantId $servicePrincipalConnection.TenantId `
        -ApplicationId $servicePrincipalConnection.ApplicationId `
        -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint 
    "Login complete."
}
catch {
    if (!$servicePrincipalConnection)
    {
        $ErrorMessage = "Connection $connectionName not found."
        throw $ErrorMessage
    } else{
        Write-Error -Message $_.Exception
        throw $_.Exception
    }
}

#Optionally you may set the following as parameters
$StorageAccountName = <storageAccountName>
$RGName = <storageAccountResourceGroupName>
$VaultName = <keyVaultName>
$SecretName = <keyVaultSecretName>

#Key name. For example key1 or key2 for the storage account
New-AzureRmStorageAccountKey -ResourceGroupName $RGName -StorageAccountName $StorageAccountName -KeyName "key2" -Verbose
$SAKeys = Get-AzureRmStorageAccountKey -ResourceGroupName $RGName -Name $StorageAccountName

$secretvalue = ConvertTo-SecureString $SAKeys[1].Value -AsPlainText -Force

$secret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $SecretName -SecretValue $secretvalue
```

W okienku Edytor można wybrać "Test okienko" Aby przetestować skrypt. Po uruchomieniu skrypt bezbłędnie można wybrać opcję "Opublikuj", a następnie można Zastosuj harmonogram działań aranżacji przywrócić w okienku Konfiguracja działań aranżacji.

##<a name="key-vault-auditing-pipeline"></a>Inspekcja magazynu klucza planowana

Po skonfigurowaniu magazynu klucza Azure można włączyć inspekcji zbieranie dzienników na żądania dostępu do magazynu klucza. Dzienniki te są przechowywane w wyznaczonych konta magazynu platformy Azure i następnie można pobierane, monitorowanie i analizy. Poniżej opisano scenariusz z wykorzystuje funkcje Azure aplikacje logiczny Azure i klucza magazynu inspekcji dzienniki, aby utworzyć potok, aby wysłać wiadomość e-mail, gdy hasła z magazynu są pobierane przez aplikację, która jest zgodna z identyfikatorem aplikacji w aplikacji sieci web.

Najpierw należy włączyć rejestrowanie dla magazynu usługi klucza. Można to zrobić za pomocą następujących poleceń programu PowerShell (szczegółowe informacje są widoczne [poniżej](key-vault-logging.md)):

```powershell
$sa = New-AzureRmStorageAccount -ResourceGroupName <resourceGroupName> -Name <storageAccountName> -Type Standard\_LRS -Location 'East US'
$kv = Get-AzureRmKeyVault -VaultName '<vaultName>' 
Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent
```

Gdy ta opcja jest włączona, następnie dzienników inspekcji rozpocznie zbieranie pod uwagę wyznaczonego miejsca do magazynowania. Te dzienniki będzie zawierać zdarzeń o tym, jak i podczas uzyskiwania dostępu do Twojej magazynami klucza i przez kogo. 

> \[AZURE. Uwaga\] można dostęp do informacji rejestrowania, co najwyżej, 10 minut po klucz magazynu operacji. W większości przypadków będą szybciej niż to.

Następnym krokiem jest [utworzenie kolejkę Bus usługi Azure](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md). Są to miejsce, w którym dzienniki inspekcji magazynu klucza są usuwane. W kolejce, aplikacja logiki będzie po ich skompletowanie i działać w ich. Aby utworzyć Bus usługi jest stosunkowo bezpośrednie i poniżej wysokiego poziomu czynności:

1. Tworzenie nazw Bus usługi (Jeśli masz już jeden, którego chcesz użyć dla tego, a następnie przejdź do kroku 2).
2. Przejdź do Bus usługi w portalu i zaznacz obszar nazw, aby utworzyć kolejki w.
3. Wybierz pozycję Nowy, a następnie wybierz pozycję Usługa Bus -> kolejki i wprowadź wymagane informacje.
4. Chwyć Bus usługi informacje o połączeniu, wybierając obszar nazw i klikając pozycję _Informacje o połączeniu_. Informacje te będą potrzebne do następnej strony.

Konieczne będzie dalej, [Tworzenie funkcji Azure](../azure-functions/functions-create-first-azure-function.md) ankieta dzienniki klucza magazynu w ramach konta miejsca do magazynowania, a następnie wybierz nowy zdarzeń. Jest to funkcja, która zostanie wywołana zgodnie z harmonogramem.

Tworzenie funkcji Azure (Wybierz nowy -> funkcji aplikacji w portalu). Podczas tworzenia można użyć planu istniejącego lub Utwórz nowy. Można też wybrać opcję do obsługi dynamicznego. Więcej informacji na temat funkcji hostingu opcji można znaleźć [w tym miejscu](../azure-functions/functions-scale.md).

Po utworzeniu funkcja Azure, przejdź do niej i wybierz pozycję czasomierza, funkcja i C\# na ekranie startowym kliknij przycisk **Utwórz** .

![Karta Start funkcje Azure](./media/keyvault-keyrotation/Azure_Functions_Start.png)

Na karcie _opracowanie_ zastąpić run.csx następujące czynności:

```csharp
#r "Newtonsoft.Json"

using System;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage.Blob;
using Microsoft.ServiceBus.Messaging; 
using System.Text;

public static void Run(TimerInfo myTimer, TextReader inputBlob, TextWriter outputBlob, TraceWriter log) 
{ 
    log.Info("Starting");

    CloudStorageAccount sourceStorageAccount = new CloudStorageAccount(new StorageCredentials("<STORAGE_ACCOUNT_NAME>", "<STORAGE_ACCOUNT_KEY>"), true);

    CloudBlobClient sourceCloudBlobClient = sourceStorageAccount.CreateCloudBlobClient();

    var connectionString = "<SERVICE_BUS_CONNECTION_STRING>";
    var queueName = "<SERVICE_BUS_QUEUE_NAME>";

    var sbClient = QueueClient.CreateFromConnectionString(connectionString, queueName);

    DateTime dtPrev = DateTime.UtcNow;
    if(inputBlob != null)
    {
        var txt = inputBlob.ReadToEnd();

        if(!string.IsNullOrEmpty(txt))
        {
            dtPrev = DateTime.Parse(txt);
            log.Verbose($"SyncPoint: {dtPrev.ToString("O")}");
        }
        else
        {
            dtPrev = DateTime.UtcNow;
            log.Verbose($"Sync point file didnt have a date. Setting to now.");
        }
    }

    var now = DateTime.UtcNow;

    string blobPrefix = "insights-logs-auditevent/resourceId=/SUBSCRIPTIONS/<SUBSCRIPTION_ID>/RESOURCEGROUPS/<RESOURCE_GROUP_NAME>/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/<KEY_VAULT_NAME>/y=" + now.Year +"/m="+now.Month.ToString("D2")+"/d="+ (now.Day).ToString("D2")+"/h="+(now.Hour).ToString("D2")+"/m=00/";

    log.Info($"Scanning:  {blobPrefix}");

    IEnumerable<IListBlobItem> blobs = sourceCloudBlobClient.ListBlobs(blobPrefix, true);

    log.Info($"found {blobs.Count()} blobs");

    foreach(var item in blobs)
    {
        if (item is CloudBlockBlob)
        {
            CloudBlockBlob blockBlob = (CloudBlockBlob)item;

            log.Info($"Syncing: {item.Uri}");

            string sharedAccessUri = GetContainerSasUri(blockBlob);

            CloudBlockBlob sourceBlob = new CloudBlockBlob(new Uri(sharedAccessUri));

            string text;
            using (var memoryStream = new MemoryStream())
            {
                sourceBlob.DownloadToStream(memoryStream);
                text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
            }

            dynamic dynJson = JsonConvert.DeserializeObject(text);

            //required to order by time as they may not be in the file
            var results = ((IEnumerable<dynamic>) dynJson.records).OrderBy(p => p.time);

            foreach (var jsonItem in results)
            {
                DateTime dt = Convert.ToDateTime(jsonItem.time);

                if(dt>dtPrev){
                    log.Info($"{jsonItem.ToString()}");

                    var payloadStream = new MemoryStream(Encoding.UTF8.GetBytes(jsonItem.ToString()));
                    //When sending to ServiceBus, use the payloadStream and set keeporiginal to true
                    var message = new BrokeredMessage(payloadStream, true);
                    sbClient.Send(message);
                    dtPrev = dt;
                }
            }
        }
    }
    outputBlob.Write(dtPrev.ToString("o"));
}

static string GetContainerSasUri(CloudBlockBlob blob) 
{
    SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();

    sasConstraints.SharedAccessStartTime = DateTime.UtcNow.AddMinutes(-5);
    sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
    sasConstraints.Permissions = SharedAccessBlobPermissions.Read;

    //Generate the shared access signature on the container, setting the constraints directly on the signature.
    string sasBlobToken = blob.GetSharedAccessSignature(sasConstraints);

    //Return the URI string for the container, including the SAS token.
    return blob.Uri + sasBlobToken;
}
```
> \[AZURE. Uwaga\] upewnij się, że zastępowania zmiennych w kodzie powyżej wskaż miejsce, w którym są zapisywane dzienniki magazynu klucz konta miejsca do magazynowania, Bus usługi został utworzony wcześniej i określonej ścieżki do dzienników miejsca do magazynowania klucza magazynu.

Funkcja przejmuje najnowszą pliku dziennika z konta miejsca do magazynowania miejsce, w którym są zapisywane dzienniki magazynu klucza, grabs najnowsze wydarzenia z tego pliku i umieszcza je do kolejki Bus usługi. Ponieważ pojedynczy plik może mieć wiele zdarzeń, np. przez godzinę pełny, możemy utworzy plik _sync.txt_ , funkcja również wyglądający na ustalenie sygnatura czasowa ostatniego zdarzenia, pobranego w górę. Temu będziesz mieć pewność, że firma Microsoft nie push tego samego zdarzenia wiele razy. Ten plik _sync.txt_ zawiera po prostu sygnatura czasowa dla ostatniego wykrytych zdarzenia. Dzienniki, podczas ładowania, musisz posortowane według sygnatury czasowej, aby upewnić się, że są one układane poprawnie.

W przypadku tej funkcji możemy odwoływać się kilka dodatkowych bibliotek, które nie są dostępne okno w funkcjach Azure. Uwzględnienie tych potrzebujemy funkcje Azure, aby uwzględniał je przy użyciu nuget. Wybierz opcję _Wyświetlanie plików_ 

![Wyświetlanie plików](./media/keyvault-keyrotation/Azure_Functions_ViewFiles.png)

i dodać nowy plik o nazwie _project.json_ o następującej treści:

```json
    {
      "frameworks": {
        "net46":{
          "dependencies": {
                "WindowsAzure.Storage": "7.0.0",
                "WindowsAzure.ServiceBus":"3.2.2"
          }
        }
       }
    }
```
Podczas _zapisywania_ spowoduje funkcji Azure, aby pobrać wymaganych plików binarnych. 

Przejdź na kartę **Integracja** i nadaj parametr czasomierza zrozumiałą nazwę, aby użyć w funkcji. W kodzie powyżej oczekuje czasomierza zwany _myTimer_. Określ [CRON wyrażenia](../app-service-web/web-sites-create-web-jobs.md#CreateScheduledCRON) w następujący sposób: 0 \* \* \* \* \* dla czasomierz, który spowoduje, że funkcja uruchamianie raz na minutę. 

W tej samej karcie **Integrowanie** Dodaj wprowadzania, który będzie typu _Magazyn obiektów Blob platformy Azure_. Spowoduje to wskaż plik _sync.txt_ , który zawiera sygnatury czasowej ostatniego zdarzenia elementu przy użyciu funkcji. To będą dostępne w funkcji według nazwy parametru. W kodzie powyżej wprowadzania magazyn obiektów Blob platformy Azure oczekuje Nazwa parametru być _inputBlob_. Wybierz konto miejsca do magazynowania, gdzie będzie się znajdować plik _sync.txt_ (może być tym samym lub z konta innego miejsca do magazynowania) i w polu Ścieżka podaj ścieżkę, w którym plik przebywa w formacie {container-name}/path/to/sync.txt.

Dodaj dane wyjściowe, które będą typu Wyjście _Magazyn obiektów Blob platformy Azure_ . Spowoduje to również wskaż plik _sync.txt_ zdefiniowanych w danych wejściowych. Będzie można używać za pomocą funkcji napisać sygnatura czasowa zdarzenia ostatniego elementu. Powyższy kod oczekuje ten parametr na wywoływanie _Blob_wyj_.

W tym momencie ta funkcja jest gotowy. Upewnij się wrócić do karty **opracowanie** i _Zapisywanie_ kodu. Sprawdź błędy kompilacji okno dane wyjściowe i usuń te odpowiednio. Jeśli skompilowany, następnie kod teraz powinna być uruchomiona i co minutę sprawdzi dzienniki klucza magazynu i push nowego zdarzenia na zdefiniowane kolejki Bus usługi. Informacje o logowaniu zamieścić na każdym okna dziennika, który zostanie wywołana funkcja powinna być widoczna.

###<a name="azure-logic-app"></a>Logika Azure aplikacji

Następny potrzebujemy utworzyć aplikację logiczny Azure, wybierz zdarzenia, które funkcja jest naciśnięcie do kolejki Bus usługi, przeanalizować zawartość i wysyłanie wiadomości e-mail na podstawie warunku filtrowanego.

[Tworzenie aplikacji logiczny](../app-service-logic/app-service-logic-create-a-logic-app.md) , wybierając pozycję Nowy -> logiczny aplikacji. 

Po utworzeniu aplikacji logika przejdź do niej i wybierz polecenie _Edytuj_. W edytorze logiki aplikacji wybierz _Usługę Bus kolejki_ zarządzany interfejs api i wprowadź poświadczenia Bus usługi, aby połączyć go z kolejki.

![Logika Azure Bus aplikacji usługi](./media/keyvault-keyrotation/Azure_LogicApp_ServiceBus.png)

Następnie wybierz pozycję _Dodaj warunek_. W warunku, przełącz się do _Edytor zaawansowany_ i wprowadź następujące informacje, zamieniając APP_ID rzeczywisty APP_ID aplikacji sieci web:

```
@equals('<APP_ID>', json(decodeBase64(triggerBody()['ContentData']))['identity']['claim']['appid'])
```

To wyrażenie zasadniczo zwróci **FAŁSZ,** Jeśli właściwość **Identyfikator aplikacji** od przychodzących zdarzenia (czyli w treści wiadomości usługi Bus) nie jest **Identyfikator aplikacji** aplikacji. 

Teraz Utwórz akcji w obszarze opcji _Jeśli nie, nie rób nic,..._ .

![Azure logiki aplikacji wybierz akcję](./media/keyvault-keyrotation/Azure_LogicApp_Condition.png)

W przypadku akcji wybierz pozycję _Office 365 — wysyłanie wiadomości e-mail_. Wypełnij pola, aby utworzyć wiadomość e-mail do wysyłania, gdy warunek zdefiniowane zwraca wartość false. Jeśli nie masz usługi Office 365 można przeglądać rozwiązania alternatywne wobec osiągnięcia tak samo.

W tym momencie masz planowana kompleksowego, który raz na minutę będzie wyglądać na nowy klucz magazynu dzienników inspekcji. Wszelkie nowe dzienniki znalezionych go przesunie je do kolejki Bus usługi. Aplikacja logiczny zostanie wyzwolony zaraz po nowej wiadomości znajdzie się w kolejce i jeśli identyfikator w zdarzeniu nie pasuje do identyfikatora aplikacji aplikacji wywołującej, a następnie wyślij wiadomość e-mail. 
