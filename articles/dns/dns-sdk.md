<properties 
   pageTitle="Tworzenie strefy DNS i zestawy rekordów w systemie DNS Azure przy użyciu zestawu SDK .NET | Microsoft Azure" 
   description="Jak utworzyć strefy DNS i zestawy rekordów w systemie DNS Azure za pomocą .NET SDK." 
   services="dns" 
   documentationCenter="na" 
   authors="jtuliani" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="09/19/2016"
   ms.author="jtuliani"/>


# <a name="create-dns-zones-and-record-sets-using-the-net-sdk"></a>Tworzenie strefy DNS i zestawy rekordów przy użyciu zestawu SDK .NET

Można zautomatyzować operacje, aby tworzenie, usuwanie lub aktualizowanie strefy DNS, zestawy rekordów i rekordów przy użyciu zestawu SDK DNS z biblioteką zarządzania systemem DNS .NET. Pełny projekt programu Visual Studio jest dostępna [tutaj.](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True)

## <a name="create-a-service-principal-account"></a>Tworzenie głównej konta usługi

Zazwyczaj programowy dostęp do zasobów Azure otrzymuje się za pomocą konta dedykowanego niż poświadczenia użytkownika. Te konta dedykowane są nazywane "głównej usługi" konta. Aby użyć Azure DNS SDK przykładowy projekt, należy najpierw utwórz konto głównej usługi i przypisz mu odpowiednich uprawnień.

1. Wykonaj [te instrukcje](../resource-group-authenticate-service-principal.md) , aby utworzyć podstawowe konta usługi (Azure DNS SDK przykładowy projekt przy założeniu uwierzytelniania opartego na hasło).

2. Tworzenie grupy zasobów ([poniżej opisano, jak](../azure-portal/resource-group-portal.md)).

3. Uprawnienia konta głównej usługi "Współautora strefy DNS" do grupy zasobów za pomocą Azure RBAC ([poniżej opisano, jak](../active-directory/role-based-access-control-configure.md).)

4. Jeśli używasz Azure DNS SDK przykładowy projekt, edytować plik "plik program.cs" w następujący sposób:
    * Wstawianie poprawne wartości dla tenantId, clientId (nazywane także identyfikator konta) i hasło usługi konta główne subscriptionId w kroku 1.
    * Wprowadź nazwę grupy zasobów wybranego w kroku 2.
    * Wprowadź nazwę strefy DNS wybranych przez użytkownika.

## <a name="nuget-packages-and-namespace-declarations"></a>Pakiety NuGet i deklaracje przestrzeni nazw

Aby użyć Azure DNS .NET SDK, należy zainstalować pakiet NuGet **Biblioteka zarządzania DNS Azure** i inne wymagane pakiety Azure.
 
1. W **Programie Visual Studio**Otwórz projekt lub nowego projektu. 

2. Przejdź do polecenia **Narzędzia** **>** **Menedżera pakietów NuGet** **>** **Zarządzanie pakietów NuGet... rozwiązania**. 

3. Kliknij przycisk **Przeglądaj**, pole wyboru **obejmują wstępną** Włącz, a następnie wpisz **Microsoft.Azure.Management.Dns** w polu wyszukiwania.

4. Wybierz pakiet i kliknij przycisk **Zainstaluj** , aby ją dodać do projektu programu Visual Studio.
 
5. Powtórz proces powyżej, aby również zainstalować następujące pakiety: **Microsoft.Rest.ClientRuntime.Azure.Authentication** i **Microsoft.Azure.Management.ResourceManager**.

## <a name="add-namespace-declarations"></a>Dodawanie deklaracje przestrzeni nazw

Dodaj następujące deklaracje przestrzeń nazw

    using Microsoft.Rest.Azure.Authentication;
    using Microsoft.Azure.Management.Dns;
    using Microsoft.Azure.Management.Dns.Models;

## <a name="initialize-the-dns-management-client"></a>Inicjowanie klienta zarządzania DNS

*DnsManagementClient* zawiera metody i właściwości niezbędnych do zarządzania strefy DNS oraz zestawy danych.  Poniższy kod loguje się do konta głównej usługi i tworzy obiekt DnsManagementClient.

    // Build the service credentials and DNS management client
    var serviceCreds = await ApplicationTokenProvider.LoginSilentAsync(tenantId, clientId, secret);
    var dnsClient = new DnsManagementClient(serviceCreds);
    dnsClient.SubscriptionId = subscriptionId;

## <a name="create-or-update-a-dns-zone"></a>Tworzenie lub aktualizowanie strefy DNS

Aby utworzyć strefy DNS, najpierw "Strefy" zostanie utworzony obiekt ma zawierać parametry strefy DNS. Ponieważ strefy DNS nie są połączone z określonego regionu, lokalizacji jest równa "global". W tym przykładzie [Menedżer zasobów Azure "znacznika"](https://azure.microsoft.com/updates/organize-your-azure-resources-with-tags/) jest również dodawana do strefy.

Do faktycznie Tworzenie lub aktualizowanie strefy Azure DNS, obiekty strefy, które zawierają parametrów strefy są przekazywane do metody *DnsManagementClient.Zones.CreateOrUpdateAsyc* .

>[AZURE.NOTE] DnsManagementClient obsługuje trzy tryby działania: obraz ("CreateOrUpdate"), asynchroniczne ("CreateOrUpdateAsync"), lub asynchroniczna z dostępem do odpowiedzi HTTP (CreateOrUpdateWithHttpMessagesAsync).  Możesz wybrać jedną z następujących trybów w zależności od potrzeb aplikacji.

Azure DNS obsługuje współbieżności optymistycznych, o nazwie [Etags](dns-getstarted-create-dnszone.md). W tym przykładzie określając "*" "Jeżeli — brak-dopasowania" Nagłówek informuje Azure DNS, aby utworzyć strefy DNS, jeśli jeszcze nie istnieje.  Połączenie zakończy się niepowodzeniem, jeśli strefa o podanej nazwie już istnieje w określonej grupy zasobów.

    // Create zone parameters
    var dnsZoneParams = new Zone("global"); // All DNS zones must have location = "global"
    
    // Create a Azure Resource Manager 'tag'.  This is optional.  You can add multiple tags
    dnsZoneParams.Tags = new Dictionary<string, string>();
    dnsZoneParams.Tags.Add("dept", "finance");
    
    // Create the actual zone.
    // Note: Uses 'If-None-Match *' ETAG check, so will fail if the zone exists already.
    // Note: For non-async usage, call dnsClient.Zones.CreateOrUpdate(resourceGroupName, zoneName, dnsZoneParams, null, "*")
    // Note: For getting the http response, call dnsClient.Zones.CreateOrUpdateWithHttpMessagesAsync(resourceGroupName, zoneName, dnsZoneParams, null, "*")
    var dnsZone = await dnsClient.Zones.CreateOrUpdateAsync(resourceGroupName, zoneName, dnsZoneParams, null, "*");

## <a name="create-dns-record-sets-and-records"></a>Tworzenie zestawów rekordów DNS i rekordy

Rekordy DNS są obsługiwane jako zestaw rekordów. Zestaw rekordów jest zestaw rekordów zawierających tę samą nazwę i typ rekordu w strefie.  Nazwa zestawu rekordów jest względem nazwę strefy, a nie w pełni kwalifikowana nazwa DNS.

Aby utworzyć lub zaktualizować rekordu Ustaw "rekordów", parametry obiekt jest tworzony i przekazywane do *DnsManagementClient.RecordSets.CreateOrUpdateAsync*. Zgodnie z strefy DNS są dostępne trzy tryby pracy: obraz ("CreateOrUpdate"), asynchroniczne ("CreateOrUpdateAsync"), lub asynchroniczna z dostępem do odpowiedzi HTTP (CreateOrUpdateWithHttpMessagesAsync).

Podobnie jak w przypadku strefy DNS operacji na zestawach rekordów obejmują obsługę optymistyczny współbieżności.  W tym przykładzie ponieważ podano "Jeżeli dopasowania" ani "Jeżeli żaden-dopasowania", zestawu rekordów jest zawsze tworzone.  To połączenie zastępuje dowolny istniejący rekord zestaw z taką samą nazwę i typ rekordu w tej strefie DNS.

    // Create record set parameters
    var recordSetParams = new RecordSet();
    recordSetParams.TTL = 3600;

    // Add records to the record set parameter object.  In this case, we'll add a record of type 'A'
    recordSetParams.ARecords = new List<ARecord>();
    recordSetParams.ARecords.Add(new ARecord("1.2.3.4"));

    // Add metadata to the record set.  Similar to Azure Resource Manager tags, this is optional and you can add multiple metadata name/value pairs
    recordSetParams.Metadata = new Dictionary<string, string>();
    recordSetParams.Metadata.Add("user", "Mary");

    // Create the actual record set in Azure DNS
    // Note: no ETAG checks specified, will overwrite existing record set if one exists
    var recordSet = await dnsClient.RecordSets.CreateOrUpdateAsync(resourceGroupName, zoneName, recordSetName, RecordType.A, recordSetParams);

## <a name="get-zones-and-record-sets"></a>Strefy i zestawy rekordów

Metody *DnsManagementClient.Zones.Get* i *DnsManagementClient.RecordSets.Get* pobierać odpowiednio poszczególne strefy i zestawy rekordów. Zestawy rekordów są oznaczane ich typ, nazwa i grupy zasobów i strefy, którą istnieją w. Strefy są oznaczane jej nazwę i grupa zasobów, które istnieją w.

    var recordSet = dnsClient.RecordSets.Get(resourceGroupName, zoneName, recordSetName, RecordType.A);
    
## <a name="update-an-existing-record-set"></a>Aktualizowanie istniejącego zestawu rekordów

Aktualizowanie istniejącego zestawu rekordów DNS, najpierw pobrać zestawu rekordów, a następnie zaktualizuj zawartość zestawu rekordów, a następnie przesłać zmiany.  W tym przykładzie jest określona "Etag" z zestawu pobieranego rekordu w parametrze "Jeżeli dopasowania". Połączenie zakończy się niepowodzeniem, jeśli operacji równoczesne zmodyfikował ustawić w tym samym czasie.

    var recordSet = dnsClient.RecordSets.Get(resourceGroupName, zoneName, recordSetName, RecordType.A);

    // Add a new record to the local object.  Note that records in a record set must be unique/distinct
    recordSet.ARecords.Add(new ARecord("5.6.7.8"));

    // Update the record set in Azure DNS
    // Note: ETAG check specified, update will be rejected if the record set has changed in the meantime
    recordSet = await dnsClient.RecordSets.CreateOrUpdateAsync(resourceGroupName, zoneName, recordSetName, RecordType.A, recordSet, recordSet.Etag);

## <a name="list-zones-and-record-sets"></a>Lista stref i zestawy rekordów

Aby wyświetlić strefy, użyj metody *DnsManagementClient.Zones.List...* , które obsługują listę wszystkich stref w danej grupy zasobów albo wszystkich stref w danej subskrypcji Azure (za pośrednictwem grup zasobów.) Aby wyświetlić zestawy rekordów, użyj metody *DnsManagementClient.RecordSets.List...* , które obsługują albo wyświetlanie listy wszystkich zestawach rekordów w określonej strefie lub tylko te zestawy rekordów określonego typu.

Zwróć uwagę, gdy lista stref i zestawy rekordów, które wyniki mogą zostać podzielony na strony.  W poniższym przykładzie pokazano, jak przejść do strony wyników. (Rozmiar sztucznie strony "2" jest używany do wymuszania stronicowania; w praktyce można pominąć ten parametr i użyty domyślny rozmiar strony)

    // Note: in this demo, we'll use a very small page size (2 record sets) to demonstrate paging
    // In practice, to improve performance you would use a large page size or just use the system default
    int recordSets = 0;
    var page = await dnsClient.RecordSets.ListAllInResourceGroupAsync(resourceGroupName, zoneName, "2");
    recordSets += page.Count();

    while (page.NextPageLink != null)
    {
        page = await dnsClient.RecordSets.ListAllInResourceGroupNextAsync(page.NextPageLink);
        recordSets += page.Count();
    }

## <a name="next-steps"></a>Następne kroki

Pobierz [Azure DNS .NET SDK przykładowy projekt](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True), który zawiera przykłady dalszego używania SDK .NET Azure DNS, włącznie z przykładami dla innych typów rekordów DNS.
