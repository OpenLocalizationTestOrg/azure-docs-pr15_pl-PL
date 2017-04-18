<properties
    pageTitle="Azure Menedżera zasobów pomocy technicznej dla Menedżera ruch | Microsoft Azure "
    description="Przy użyciu programu PowerShell dla Menedżera ruch przy użyciu Menedżera zasobów Azure"
    services="traffic-manager"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="azure-resource-manager-support-for-azure-traffic-manager"></a>Azure Menedżera zasobów pomocy technicznej dla Menedżera ruch Azure

Azure Menedżer zasobów jest interfejs preferowanej zarządzania dla usług w Azure. Azure profile Menedżer ruchu można zarządzać, korzystając z oparte na Menedżera zasobów Azure interfejsy API i narzędzi.

## <a name="resource-model"></a>Model zasobów

Azure Menedżer ruchu jest skonfigurowany przy użyciu zbioru ustawień określanych jako Menedżer ruchu profilu. Ten profil zawiera ustawienia, ustawienia routingu ruchu, ustawienia monitorowania punktu końcowego i lista punkty końcowe usługi, do których jest kierowane ruch DNS.

Każdy profil Menedżer ruchu jest reprezentowany przez zasób typu "TrafficManagerProfiles". Na poziomie interfejsu API usługi REST identyfikator URI dla każdego profilu jest w następujący sposób:

    https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Network/trafficManagerProfiles/{profile-name}?api-version={api-version}

## <a name="comparison-with-the-azure-traffic-manager-classic-api"></a>Porównanie z interfejsu API klasyczny Menedżer ruchu Azure

Obsługa Azure Menedżera zasobów dla Menedżera ruch jest używana inna terminologia niż model klasyczny wdrożenia. W poniższej tabeli przedstawiono różnice między Menedżera zasobów i klasycznym terminów:

| Menedżer zasobów terminów | Klasyczny terminów |
|-----------------------|--------------|
| Metoda routingu ruchu | Metoda równoważenia obciążenia |
| Metoda Priority (priorytet) | Metody pracy awaryjnej |
| Metoda ważona | Metoda karuzeli |
| Metoda wydajności | Metoda wydajności |

Na podstawie opinii klientów, możemy zmienić terminologia, aby zwiększyć jasność i skrócić nieporozumień typowych. Nie różni się w funkcji.

## <a name="limitations"></a>Ograniczenia

Przy odwoływaniu się do punktu końcowego typu "AzureEndpoints" dla aplikacji sieci Web, punkty końcowe Menedżer ruchu może odwoływać się tylko domyślne (produkcja) [przedział aplikacji sieci Web](../app-service-web/web-sites-staged-publishing.md). Gniazda niestandardowe nie są obsługiwane. Obejść ten problem można skonfigurować niestandardowe gniazda przy użyciu typu "ExternalEndpoints".

## <a name="setting-up-azure-powershell"></a>Konfigurowanie programu PowerShell Azure

Te instrukcje przy użyciu programu Microsoft Azure. W poniższym artykule wyjaśniono, jak zainstalować i skonfigurować Azure programu PowerShell.

- [Jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md)

W przykładach w tym artykule założono, że masz istniejącej grupy zasobów. Można utworzyć grupę zasobów przy użyciu następującego polecenia:

```powershell
    New-AzureRmResourceGroup -Name MyRG -Location "West US"
```

>[AZURE.NOTE] Azure Menedżera zasobów wymaga, że wszystkie grupy zasobów lokalizacji. Lokalizacja ta jest używana jako domyślnego dla zasobów utworzone w danej grupy zasobów. Jednak ponieważ Menedżer ruchu profilu zasoby są globalnego, nie regionalne, wybór lokalizacji grupa zasobów nie ma wpływu na Menedżer ruchu Azure.

## <a name="create-a-traffic-manager-profile"></a>Tworzenie profilu Menedżer ruchu

Aby utworzyć profil Menedżer ruchu, należy użyć polecenia cmdlet nowy AzureRmTrafficManagerProfile:

```powershell
    $profile = New-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName contoso -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
```

W poniższej tabeli opisano parametry:

| Parametr | Opis |
|-----------|-------------|
| Nazwa | Nazwa zasobu dla zasobu profilu Menedżer ruchu. Profile w tej samej grupy zasobów muszą mieć unikatowe nazwy. Ta nazwa różni się od nazwy DNS dla kwerend DNS.|
| ResourceGroupName | Nazwa grupy zasobów zawierającą zasób profilu.|
| TrafficRoutingMethod | Określa metodę routingu ruchu służące do ustalania, który punkt końcowy jest zwracany w odpowiedzi na kwerendę DNS. Możliwe wartości to "Wydajności", "Ważone" lub "Priorytet".|
| RelativeDnsName | Określa hostname część nazwy DNS dostarczony przez tego profilu Menedżer ruchu. Ta wartość jest używana w połączeniu z nazwą domeny DNS używany przez Menedżera ruch Azure do tworzenia w pełni kwalifikowaną nazwę domeny (FQDN) profilu. Na przykład wartość "contoso" staje się "contoso.trafficmanager.net."|
| TTL | Określa DNS Time-to-Live (TTL) w sekundach. Ten TTL informuje rozpoznawania nazw DNS lokalne i jak długo pamięć podręczną DNS odpowiedzi dla tego profilu Menedżer ruchu klientów DNS.|
| MonitorProtocol | Określa protokół za pomocą monitorowanie kondycji punktu końcowego. Możliwe wartości to "HTTP" i "HTTPS".|
| MonitorPort | Określa port TCP umożliwia monitorowanie kondycji punktu końcowego.|
| MonitorPath | Określa ścieżkę względem punktu końcowego nazwę domeny używaną do sondy kondycji punktu końcowego.|

Polecenie cmdlet tworzy profilu Menedżer ruchu w Azure i zwraca odpowiedni obiekt profilu programu PowerShell. W tym momencie profil nie zawiera wszystkie punkty końcowe. Aby uzyskać więcej informacji o dodawaniu punktów końcowych do profilu Menedżer ruchu zobacz [Dodawanie punkty końcowe Menedżer ruchu](#adding-traffic-manager-endpoints).

## <a name="get-a-traffic-manager-profile"></a>Pobierz profil Menedżer ruchu

Aby pobrać istniejący obiekt profilu Menedżer ruchu, należy użyć polecenia cmdlet Get-AzureRmTrafficManagerProfle:

```powershell
    $profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
```

To polecenie cmdlet zwraca obiekt profilu Menedżer ruchu.

## <a name="update-a-traffic-manager-profile"></a>Aktualizowanie profilu Menedżer ruchu

Modyfikowanie profilów Menedżer ruchu obejmuje proces krok 3:

1. Pobierz profil przy użyciu Get-AzureRmTrafficManagerProfile lub za pomocą zwracane przez AzureRmTrafficManagerProfile nowy profil.
2. Modyfikowanie profilu. Możesz dodać i usunąć punkty końcowe lub zmienić parametry punktu końcowego lub profilu. Te zmiany są operacje w trybie offline. Powoduje tylko zmianę lokalny obiekt w pamięci, która reprezentuje profilu.
3. Zatwierdź zmiany przy użyciu polecenia cmdlet Set-AzureRmTrafficManagerProfile.

Z wyjątkiem RelativeDnsName profilu można zmienić wszystkie właściwości profilu. Aby zmienić RelativeDnsName, możesz usunąć profil i nowego profilu pod nową nazwą.

Poniższy przykład przedstawia sposób zmieniania TTL profilu:

```powershell
    $profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
    $profile.Ttl = 300
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

Istnieją trzy typy punktów końcowych Menedżer ruchu:

1. **Azure punkty końcowe** są usługi obsługiwane platformy Azure
2. **Zewnętrzne punkty końcowe** są usługi hostowanej poza Azure
3. **Zagnieżdżone punkty końcowe** są używane do skonstruowania zagnieżdżonych hierarchiach ruch Menedżer profilów. Zagnieżdżone punkty końcowe włączyć zaawansowane routingu ruchu konfiguracji złożonych aplikacji.

We wszystkich przypadkach trzy punkty końcowe mogą być dodawane na dwa sposoby:

1. Przy użyciu procesu krok 3 opisane powyżej. Zaletą tej metody to, że kilka zmian punktu końcowego mogą być wprowadzane w pojedynczej aktualizacji.
2. Przy użyciu polecenia cmdlet New-AzureRmTrafficManagerEndpoint. To polecenie cmdlet dodaje punktu końcowego do istniejącego profilu Menedżer ruchu w jednej operacji.

## <a name="adding-azure-endpoints"></a>Dodawanie punktów końcowych Azure

Azure punkty końcowe odwoływać się do usługi obsługiwane platformy Azure. Obsługiwane są trzy typy Azure punkty końcowe:

1. Aplikacje Azure Web
2. "Klasycznego" chmury usługi (które mogą zawierać usługi PaaS lub maszyn wirtualnych IaaS)
3. Azure zasoby PublicIpAddress (które może zostać dołączony do równoważenia obciążenia lub maszyny wirtualnej NIC). PublicIpAddress musi mieć przypisane do użytku w Menedżerze ruch nazwę DNS.

W każdym przypadku:

- Usługa jest określona przy użyciu parametru "targetResourceId" Dodaj AzureRmTrafficManagerEndpointConfig lub AzureRmTrafficManagerEndpoint nowy.
- "Docelowy" i "EndpointLocation" wynikających TargetResourceId.
- Określanie "waga" jest opcjonalna. Grubości są używane tylko, jeśli profil jest skonfigurowany do używania metody routingu ruchu "Ważone". W przeciwnym razie są ignorowane. Jeśli określony, wartość musi być liczbą z przedziału od 1 do 1000. Wartość domyślna to "1".
- Określanie "priorytet" jest opcjonalna. Priorytety są używane tylko, jeśli profil jest skonfigurowany do używania metody routingu ruchu "Priorytet". W przeciwnym razie są ignorowane. Prawidłowe wartości są od 1 do 1000 niższe wartości wskazująca wyższy priorytet. Jeśli określony dla punktów końcowych, musi być określona dla wszystkich punktów końcowych. Jeśli jest pominięta, wartości domyślne, rozpoczynając od "1" są stosowane w kolejności, że punkty końcowe są wyświetlane.

### <a name="example-1-adding-web-app-endpoints-using-add-azurermtrafficmanagerendpointconfig"></a>Przykład 1: Dodawanie punktów końcowych aplikacji sieci Web przy użyciu AzureRmTrafficManagerEndpointConfig Dodaj

W tym przykładzie możemy utworzyć profil Menedżer ruchu i dodaj dwa aplikacji Web App punkty końcowe przy użyciu polecenia cmdlet AzureRmTrafficManagerEndpointConfig Dodaj.

```powershell
    $profile = New-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName myapp -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
    $webapp1 = Get-AzureRMWebApp -Name webapp1
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName webapp1ep -TrafficManagerProfile $profile -Type AzureEndpoints -TargetResourceId $webapp1.Id -EndpointStatus Enabled
    $webapp2 = Get-AzureRMWebApp -Name webapp2
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName webapp2ep -TrafficManagerProfile $profile -Type AzureEndpoints -TargetResourceId $webapp2.Id -EndpointStatus Enabled
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

### <a name="example-2-adding-a-classic-cloud-service-endpoint-using-new-azurermtrafficmanagerendpoint"></a>Przykład 2: Dodawanie punktu końcowego "klasycznego" chmury usługi przy użyciu AzureRmTrafficManagerEndpoint nowy

W tym przykładzie "klasycznego" końcowego usługi w chmurze jest dodawany do profilu Menedżer ruchu. W tym przykładzie możemy określonego profilu przy użyciu nazwy grup zasobów i profilu, zamiast przechodzące obiektu profilu. Obie metody są obsługiwane.

    $cloudService = Get-AzureRmResource -ResourceName MyCloudService -ResourceType "Microsoft.ClassicCompute/domainNames" -ResourceGroupName MyCloudService
    New-AzureRmTrafficManagerEndpoint -Name MyCloudServiceEndpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type AzureEndpoints -TargetResourceId $cloudService.Id -EndpointStatus Enabled

### <a name="example-3-adding-a-publicipaddress-endpoint-using-new-azurermtrafficmanagerendpoint"></a>Przykład 3: Dodawanie punkt końcowy publicIpAddress przy użyciu AzureRmTrafficManagerEndpoint nowy

W tym przykładzie publicznej zasobu adresu IP jest dodawany do profilu Menedżer ruchu. Publiczny adres IP musi mieć nazwę DNS skonfigurowane i mogą być powiązane NIC maszyny lub równoważenia obciążenia.

    $ip = Get-AzureRmPublicIpAddress -Name MyPublicIP -ResourceGroupName MyRG
    New-AzureRmTrafficManagerEndpoint -Name MyIpEndpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type AzureEndpoints -TargetResourceId $ip.Id -EndpointStatus Enabled

## <a name="adding-external-endpoints"></a>Dodawanie zewnętrznych punktów końcowych

Menedżer ruchu używa zewnętrznych punktów końcowych, aby skierować ruch do usług hostowanej poza Azure. Podobnie jak w przypadku Azure punkty końcowe, zewnętrznych punktów końcowych można dodawać za pomocą Dodaj-AzureRmTrafficManagerEndpointConfig następuje zestawu AzureRmTrafficManagerProfile lub AzureRMTrafficManagerEndpoint nowy.

Podczas określania zewnętrznych punktów końcowych:

- Nazwa domeny punktu końcowego należy określić przy użyciu parametru "Docelowy"
- Jeśli używana jest metoda routingu ruchu "Wydajności", "EndpointLocation" jest wymagane. W przeciwnym razie jest opcjonalna. Wartość musi być [Nazwa prawidłowych regionu Azure](https://azure.microsoft.com/regions/).
- "Masa" i "Priorytet" są opcjonalne.

### <a name="example-1-adding-external-endpoints-using-add-azurermtrafficmanagerendpointconfig-and-set-azurermtrafficmanagerprofile"></a>Przykład 1: Dodawanie zewnętrznych punktów końcowych przy użyciu AzureRmTrafficManagerEndpointConfig Dodaj i ustaw AzureRmTrafficManagerProfile

W tym przykładzie możemy utworzyć profil Menedżer ruchu, dodać dwa zewnętrznych punktów końcowych i zatwierdź zmiany.

    $profile = New-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName myapp -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName eu-endpoint -TrafficManagerProfile $profile -Type ExternalEndpoints -Target app-eu.contoso.com -EndpointStatus Enabled
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName us-endpoint -TrafficManagerProfile $profile -Type ExternalEndpoints -Target app-us.contoso.com -EndpointStatus Enabled
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile

### <a name="example-2-adding-external-endpoints-using-new-azurermtrafficmanagerendpoint"></a>Przykład 2: Dodawanie zewnętrznych punktów końcowych przy użyciu AzureRmTrafficManagerEndpoint nowy

W tym przykładzie dodajemy zewnętrzny punkt końcowy do istniejącego profilu. Profil jest określona za pomocą nazwy grupy profilu i zasobów.

    New-AzureRmTrafficManagerEndpoint -Name eu-endpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type ExternalEndpoints -Target app-eu.contoso.com -EndpointStatus Enabled

## <a name="adding-nested-endpoints"></a>Dodawanie punktów końcowych "Zagnieżdżone"

Każdy profil Menedżer ruchu określa pojedynczą metodę routingu ruchu. Istnieją jednak scenariuszy, które wymagają bardziej zaawansowane routingu ruchu niż routing dostarczony przez jednego profilu Menedżer ruchu. Można zagnieżdżać profile Menedżer ruchu, aby połączyć korzyści wynikających z więcej niż jedną metodę routingu ruchu. Profile zagnieżdżonych umożliwiają zastępują domyślne zachowanie Menedżer ruchu z obsługą większych i bardziej złożonych wdrożeń aplikacji. Aby uzyskać bardziej szczegółowe przykłady zobacz [Profile zagnieżdżone Menedżer ruchu](traffic-manager-nested-profiles.md).

Zagnieżdżone punkty końcowe są skonfigurowane w profilu nadrzędnej, przy użyciu typu określonego punktu końcowego, "NestedEndpoints". Podczas określania zagnieżdżonych punkty końcowe:

- Punkt końcowy musi być określony przy użyciu parametru "targetResourceId"
- Jeśli używana jest metoda routingu ruchu "Wydajności", "EndpointLocation" jest wymagane. W przeciwnym razie jest opcjonalna. Wartość musi być [Nazwa prawidłowych regionu Azure](http://azure.microsoft.com/regions/).
- "Priorytet" i "Waga" są opcjonalne, jak w przypadku Azure punktów końcowych.
- Parametr "MinChildEndpoints" jest opcjonalny. Wartość domyślna to "1". Jeśli liczba dostępne punkty końcowe przypada poniżej wartości progowej, profilu nadrzędnej uważa profilu podrzędny "obniżonej wydajności" i przekierowywanie ruchu do punktów końcowych w profilu nadrzędnej.

### <a name="example-1-adding-nested-endpoints-using-add-azurermtrafficmanagerendpointconfig-and-set-azurermtrafficmanagerprofile"></a>Przykład 1: Dodawanie zagnieżdżonych punkty końcowe przy użyciu AzureRmTrafficManagerEndpointConfig Dodaj i ustaw AzureRmTrafficManagerProfile

W tym przykładzie możemy utworzyć nowe podrzędny menedżer ruchu i nadrzędnej profile, dodać podrzędne jako punkt końcowy zagnieżdżonych do nadrzędnej i zatwierdź zmiany.

    $child = New-AzureRmTrafficManagerProfile -Name child -ResourceGroupName MyRG -TrafficRoutingMethod Priority -RelativeDnsName child -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
    $parent = New-AzureRmTrafficManagerProfile -Name parent -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName parent -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName child-endpoint -TrafficManagerProfile $parent -Type NestedEndpoints -TargetResourceId $child.Id -EndpointStatus Enabled -EndpointLocation "North Europe" -MinChildEndpoints 2
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile

Ich omówień w tym przykładzie firma Microsoft nie zostały dodane inne punkty końcowe do profilów podrzędny lub nadrzędny.

### <a name="example-2-adding-nested-endpoints-using-new-azurermtrafficmanagerendpoint"></a>Przykład 2: Dodawanie zagnieżdżonych punkty końcowe przy użyciu AzureRmTrafficManagerEndpoint nowy

W tym przykładzie możemy dodać podrzędne profil jako punkt końcowy zagnieżdżonych do istniejącego profilu nadrzędnej. Profil jest określona za pomocą nazwy grupy profilu i zasobów.

    $child = Get-AzureRmTrafficManagerEndpoint -Name child -ResourceGroupName MyRG
    New-AzureRmTrafficManagerEndpoint -Name child-endpoint -ProfileName parent -ResourceGroupName MyRG -Type NestedEndpoints -TargetResourceId $child.Id -EndpointStatus Enabled -EndpointLocation "North Europe" -MinChildEndpoints 2

## <a name="update-a-traffic-manager-endpoint"></a>Aktualizowanie punkt końcowy Menedżer ruchu

Istnieją dwie metody aktualizowania istniejący punkt końcowy Menedżer ruchu:

1. Pobierz profil Menedżer ruchu za pomocą Get-AzureRmTrafficManagerProfile, aktualizowanie właściwości punktu końcowego w profilu i zatwierdzanie zmian przy użyciu zestawu AzureRmTrafficManagerProfile. Ta metoda zaletą jest możliwość aktualizacji więcej niż jeden punkt końcowy w jednej operacji.
2. Uzyskaj punkt końcowy Menedżer ruchu za pomocą Get-AzureRmTrafficManagerEndpoint, aktualizowanie właściwości punktu końcowego i zatwierdzanie zmian przy użyciu zestawu AzureRmTrafficManagerEndpoint. Ta metoda jest prostsze, ponieważ nie wymaga indeksowania do tablicy punkty końcowe w profilu.

### <a name="example-1-updating-endpoints-using-get-azurermtrafficmanagerprofile-and-set-azurermtrafficmanagerprofile"></a>Przykład 1: Aktualizowanie punkty końcowe przy użyciu Get-AzureRmTrafficManagerProfile i AzureRmTrafficManagerProfile zestawu

W tym przykładzie możemy zmodyfikowania priorytetu na dwa punkty końcowe w ramach istniejącego profilu.

    $profile = Get-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG
    $profile.Endpoints[0].Priority = 2
    $profile.Endpoints[1].Priority = 1
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile

### <a name="example-2-updating-an-endpoint-using-get-azurermtrafficmanagerendpoint-and-set-azurermtrafficmanagerendpoint"></a>Przykład 2: Aktualizowanie punktu końcowego przy użyciu Get-AzureRmTrafficManagerEndpoint i AzureRmTrafficManagerEndpoint zestawu

W tym przykładzie możemy zmienić grubość jeden punkt końcowy w istniejącego profilu.

    $endpoint = Get-AzureRmTrafficManagerEndpoint -Name myendpoint -ProfileName myprofile -ResourceGroupName MyRG -Type ExternalEndpoints
    $endpoint.Weight = 20
    Set-AzureRmTrafficManagerEndpoint -TrafficManagerEndpoint $endpoint

## <a name="enabling-and-disabling-endpoints-and-profiles"></a>Włączanie i wyłączanie punkty końcowe i profilów

Menedżer ruchu umożliwia poszczególnych punktów końcowych do włączone i wyłączone, oraz umożliwiające włączanie i wyłączanie całego profilów.
Te zmiany mogą zostać wprowadzone przez wprowadzenie/aktualizowania i ustawienie punktu końcowego lub profilu zasobów. Usprawnienie tych typowych operacji, są one również obsługiwane przez dedykowane poleceń cmdlet.

### <a name="example-1-enabling-and-disabling-a-traffic-manager-profile"></a>Przykład 1: Włączanie i wyłączanie profilu Menedżer ruchu

Aby włączyć profil Menedżer ruchu, za pomocą AzureRmTrafficManagerProfile Włącz. Profil można określić za pomocą obiektu profilu. Obiekt profilu mogą być przekazywane przez proces lub za pomocą "-TrafficManagerProfile" parametru. W tym przykładzie możemy Określ nazwę grupy zasobów i profilu profilu.

```powershell
    Enable-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyResourceGroup
```

Aby wyłączyć profilu Menedżer ruchu:

```powershell
    Disable-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyResourceGroup
```

Polecenie cmdlet Disable AzureRmTrafficManagerProfile jest wyświetlany monit o potwierdzenie. Tego monitu można pominąć przy użyciu "-życie" parametru.

### <a name="example-2-enabling-and-disabling-a-traffic-manager-endpoint"></a>Przykład 2: Włączanie i wyłączanie punkt końcowy Menedżer ruchu

Aby włączyć punkt końcowy Menedżer ruchu, użyj AzureRmTrafficManagerEndpoint Włącz. Istnieją dwa sposoby, aby określić punkt końcowy

1. Przy użyciu obiektu TrafficManagerEndpoint przekazywane przez proces lub "-TrafficManagerEndpoint" parametru
2. Za pomocą Nazwa punktu końcowego, typ punktu końcowego, nazwa profilu i nazwa grupy zasobów:

```powershell
    Enable-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG
```

Podobnie Aby wyłączyć punkt końcowy Menedżer ruchu:

```powershell
     Disable-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG -Force
```

Podobnie jak w przypadku AzureRmTrafficManagerProfile Wyłącz, polecenia cmdlet AzureRmTrafficManagerEndpoint Wyłącz zostanie wyświetlony monit o potwierdzenie. Tego monitu można pominąć przy użyciu "-życie" parametru.

## <a name="delete-a-traffic-manager-endpoint"></a>Usuwanie punktu końcowego Menedżer ruchu

Aby usunąć poszczególnych punktów końcowych, należy użyć polecenia cmdlet Usuń AzureRmTrafficManagerEndpoint:

```powershell
    Remove-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG
```

To polecenie cmdlet jest wyświetlany monit o potwierdzenie. Tego monitu można pominąć przy użyciu "-życie" parametru.

## <a name="delete-a-traffic-manager-profile"></a>Usuwanie profilu Menedżer ruchu

Aby usunąć profil Menedżer ruchu, należy użyć polecenia cmdlet AzureRmTrafficManagerProfile Usuń Określanie nazwy grup zasobów i profilu:

```powershell
    Remove-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG [-Force]
```

To polecenie cmdlet jest wyświetlany monit o potwierdzenie. Tego monitu można pominąć przy użyciu "-życie" parametru.

Profil, który ma zostać usunięty można również określić za pomocą obiektu profilu:

```powershell
    $profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
    Remove-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile [-Force]
```

Ta sekwencja także mogą być przekazywane w potoku:

```powershell
    Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG | Remove-AzureRmTrafficManagerProfile [-Force]
```

## <a name="next-steps"></a>Następne kroki

[Monitorowanie Menedżer ruchu](traffic-manager-monitoring.md)

[Zagadnienia dotyczące wydajności Menedżer ruchu](traffic-manager-performance-considerations.md)
