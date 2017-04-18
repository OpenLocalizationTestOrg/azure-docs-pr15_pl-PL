<properties
    pageTitle="Konfigurowanie ustawień serwera proxy i zapory w dzienniku analizy | Microsoft Azure"
    description="Konfigurowanie ustawień serwera proxy i zapory usługi agentów lub usług usługi OMS przeszukanie do używania określonych portów."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/23/2016"
    ms.author="banders;magoedte"/>

# <a name="configure-proxy-and-firewall-settings-in-log-analytics"></a>Konfigurowanie ustawień serwera proxy i zapory w analizy dziennika

Akcje potrzebne do skonfigurowania serwera proxy i zapory ustawienia dziennika analiz w usługi OMS różnią się podczas korzystania z programu Operations Manager i jego agentów i monitorowanie agentów firmy Microsoft, które bezpośrednio łączyć się z serwerami. Przejrzyj w następujących sekcjach typ agenta, którego używasz.

## <a name="configure-proxy-and-firewall-settings-with-the-microsoft-monitoring-agent"></a>Konfigurowanie ustawień serwera proxy i zapory z programem Microsoft Agent monitorowania

Dla programu Microsoft monitorowania agenta nawiązywanie i zarejestrować usługę on mieć dostęp do numer portu swojej domeny i adresy URL. Jeśli korzystasz z serwera proxy dla komunikacji między agentem i usługę, musisz upewnij się, że odpowiednie zasoby są dostępne. Jeśli używasz zapory, aby ograniczyć dostęp do Internetu, musisz skonfigurować zaporę, aby zezwolić na dostęp do usługi OMS. W poniższej tabeli wymieniono porty, których potrzebuje usługi OMS.

|**Agent zasobów**|**Porty**|**Pomijanie inspekcji HTTPS**|
|--------------|-----|--------------|
|\*. ods.opinsights.azure.com|443|Tak|
|\*. oms.opinsights.azure.com|443|Tak|
|\*. blob.core.windows.net|443|Tak|
|ods.systemcenteradvisor.com|443| |

Poniższa procedura umożliwia konfigurowanie ustawień serwera proxy dla agenta monitorowania firmy Microsoft przy użyciu Panelu sterowania. Musisz użyć procedury dla każdego serwera. Jeśli masz wiele serwerów, które należy skonfigurować, mogą okazać łatwiejszy w użyciu skrypt, aby zautomatyzować ten proces. Jeśli tak, zobacz następną procedurę, [Aby skonfigurować ustawienia serwera proxy dla za pomocą skryptu monitorowania agenta firmy Microsoft](#to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-a-script).

### <a name="to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-control-panel"></a>Aby skonfigurować ustawienia serwera proxy dla za pomocą Panelu sterowania monitorowania agenta firmy Microsoft

1. Otwórz **Panel sterowania**.

2. Otwórz **monitorowania agenta firmy Microsoft**.

3. Kliknij kartę **Ustawienia serwera Proxy** .<br>  
  ![Karta Ustawienia serwera proxy](./media/log-analytics-proxy-firewall/proxy-direct-agent-proxy.png)

4. Zaznacz pole wyboru **Użyj serwera proxy** i wpisz adres URL i numer portu, jeśli jest wymagana, podobnie jak w przedstawionym przykładzie. Jeśli serwer proxy wymaga uwierzytelniania, wpisz nazwę użytkownika i hasło, aby uzyskać dostęp do serwera proxy.

Poniższa procedura umożliwia utworzenie skrypt programu PowerShell, który może zostać uruchomiony ustawień serwera proxy dla każdego agenta, który bezpośrednio łączy się z serwerami.

### <a name="to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-a-script"></a>Aby skonfigurować ustawienia serwera proxy dla za pomocą skryptu monitorowania agenta firmy Microsoft

Skopiuj poniższy przykładowy, je zaktualizować informacje specyficzne dla środowiska, zapisz go z rozszerzeniem nazwy pliku PS1, a następnie uruchom skrypt na każdym komputerze łączącym się bezpośrednio do usługę.

        
    param($ProxyDomainName="http://proxy.contoso.com:80", $cred=(Get-Credential))

    # First we get the Health Service configuration object.  We need to determine if we
    #have the right update rollup with the API we need.  If not, no need to run the rest of the script.
    $healthServiceSettings = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'

    $proxyMethod = $healthServiceSettings | Get-Member -Name 'SetProxyInfo'

    if (!$proxyMethod)
    {
         Write-Output 'Health Service proxy API not present, will not update settings.'
         return
    }

    Write-Output "Clearing proxy settings."
    $healthServiceSettings.SetProxyInfo('', '', '')

    $ProxyUserName = $cred.username

    Write-Output "Setting proxy to $ProxyDomainName with proxy username $ProxyUserName."
    $healthServiceSettings.SetProxyInfo($ProxyDomainName, $ProxyUserName, $cred.GetNetworkCredential().password)
        

## <a name="configure-proxy-and-firewall-settings-with-operations-manager"></a>Konfigurowanie ustawień serwera proxy i zapory z programu Operations Manager

Dla grupy zarządzania programu Operations Manager do łączenia się i zarejestrować usługę on mieć dostęp do numery portów domeny i adresy URL. Jeśli korzystasz z serwera proxy dla komunikacji między serwerami zarządzania programu Operations Manager i usługę, musisz upewnij się, że odpowiednie zasoby są dostępne. Jeśli używasz zapory, aby ograniczyć dostęp do Internetu, musisz skonfigurować zaporę, aby zezwolić na dostęp do usługi OMS. Nawet jeśli serwer zarządzania programu Operations Manager nie jest za serwerem proxy, może być jego agentów. W tym przypadku serwer proxy należy skonfigurować w taki sam sposób, jak agenci są Aby włączyć i umożliwić zabezpieczeń i zarządzanie dziennikiem rozwiązanie dane są wysyłane do usługi OMS usługi sieci web.

Aby agentów programu Operations Manager do komunikowania się z usługę infrastruktury programu Operations Manager (w tym czynników) powinny mieć ustawienia serwera proxy poprawne i wersji. W konsoli programu Operations Manager określono ustawienie agentów serwera proxy. Twoja wersja powinny być jedną z następujących czynności:

- Operacje Menedżera 2012 z dodatkiem SP1 pakietu zbiorczego aktualizacji 7 lub nowszy
- Operacje Menedżera 2012 R2 zbiorczego aktualizacji 3 lub nowszej


W poniższej tabeli wymieniono porty związane z tych zadań.

>[AZURE.NOTE] Niektóre z poniższych zasobów wspomnieć Advisor i operacyjne wniosków, oba były poprzednich wersji usługi OMS. Jednak zasobów na liście zmieni się w przyszłości.

Poniżej przedstawiono listę zasobów agenta i porty:<br>

|**Agent zasobów**|**Porty**|
|--------------|-----|
|\*. ods.opinsights.azure.com|443|
|\*. oms.opinsights.azure.com|443|
|\*.blob.Core.Windows.NET/\*|443|
|ods.systemcenteradvisor.com|443|
<br>
Poniżej przedstawiono listę zasobów serwera zarządzania i portów:<br>

|**Zasób serwera zarządzania**|**Porty**|**Pomijanie inspekcji HTTPS**|
|--------------|-----|--------------|
|Service.systemcenteradvisor.com|443| |
|\*. service.opinsights.azure.com|443| |
|\*. blob.core.windows.net|443|Tak| 
|Data.systemcenteradvisor.com|443| | 
|ods.systemcenteradvisor.com|443| | 
|\*. ods.opinsights.azure.com|443|Tak| 
<br>
Poniżej przedstawiono listę zasobów konsoli usługi OMS i programu Operations Manager i portów.<br>

|**Zasób konsoli usługi OMS i Operations Manager**|**Porty**|
|----|----|
|Service.systemcenteradvisor.com|443|
|\*. service.opinsights.azure.com|443|
|\*. live.com|Port 80 i 443|
|\*. microsoft.com|Port 80 i 443|
|\*. microsoftonline.com|Port 80 i 443|
|\*. mms.microsoft.com|Port 80 i 443|
|Login.Windows.NET|Port 80 i 443|
<br>

Skorzystaj z poniższych instrukcji, aby zarejestrować grupy zarządzania programu Operations Manager z usługę. Jeśli masz problemy komunikacji między grupy zarządzania i usługę, skorzystaj z instrukcji sprawdzania poprawności rozwiązywać przesyłania danych do usługę.

### <a name="to-request-exceptions-for-the-oms-service-endpoints"></a>Aby zażądać wyjątki dla punktów końcowych usługi usługi OMS

1. Skorzystaj z informacji z pierwszej tabeli przedstawiony wcześniej, aby upewnić się, że zasoby niezbędne dla programu Operations Manager management server są dostępne za pośrednictwem zapory dowolny, że może być.
2. Skorzystaj z informacji z drugiej tabeli przedstawiony wcześniej, aby upewnić się, że zasoby niezbędne dla operacji konsoli programu Operations Manager i usługi OMS są dostępne za pośrednictwem zapory dowolny, że może być.
3. Jeśli korzystasz z serwera proxy w programie Internet Explorer, upewnij się, jest skonfigurowany i działa poprawnie. Aby sprawdzić, możesz otworzyć web bezpiecznego połączenia (HTTPS), na przykład [https://bing.com](https://bing.com). Jeśli połączenie bezpiecznego sieci web nie działa w przeglądarce, nie prawdopodobnie będzie działać w konsoli zarządzania programu Operations Manager przy użyciu usługi sieci web w chmurze.

### <a name="to-configure-the-proxy-server-in-the-operations-manager-console"></a>Konfigurowanie serwera proxy w konsoli programu Operations Manager

1. Otwórz konsolę programu Operations Manager i zaznacz obszar roboczy **administracji** .

2. Rozwijanie **Operacyjne wniosków**, a następnie wybierz **Operacyjne połączenia wnioski**.<br>  
    ![Połączenie usługi OMS menedżera operacji](./media/log-analytics-proxy-firewall/proxy-om01.png)
3. W widoku usługi OMS połączenie kliknij pozycję **Konfigurowanie serwera Proxy**.<br>  
    ![Połączenie usługi Menedżera OMS operacje Konfigurowanie serwera Proxy](./media/log-analytics-proxy-firewall/proxy-om02.png)
4. W operacyjne wniosków ustawienia Kreator: Serwera Proxy, zaznacz pole wyboru **Użyj serwera proxy, aby uzyskać dostęp do działania usługi sieci Web wniosków**, a następnie wpisz adres URL z portu numer, na przykład **http://myproxy:80**.<br>  
    ![Adres serwera proxy usługi OMS menedżera operacji](./media/log-analytics-proxy-firewall/proxy-om03.png)


### <a name="to-specify-credentials-if-the-proxy-server-requires-authentication"></a>Aby określić poświadczenia, jeśli serwer proxy wymaga uwierzytelniania
 Poświadczenia serwera proxy i ustawienia konieczne propagowanie do zarządzanych komputerów, które zgłasza się do usługi OMS. Te serwery powinny być w *Grupie monitorowania serwera Microsoft System Centrum Advisor*. Poświadczenia są szyfrowane w rejestrze każdego serwera w grupie.

1. Otwórz konsolę programu Operations Manager i zaznacz obszar roboczy **administracji** .
2. W obszarze **Konfiguracji RunAs**wybierz **profilów**.
3. Otwieranie profilu **System Centrum Advisor uruchamianie jako serwer Proxy profilu** .  
    ![Obraz profilu System Center Advisor uruchamianie jako serwera Proxy](./media/log-analytics-proxy-firewall/proxy-proxyacct1.png)
4. W oknie Uruchamianie jako profil kreatora kliknij przycisk **Dodaj** za pomocą konta Uruchom jako. Możesz utworzyć nowe konto Uruchom jako lub użyj istniejącego konta. To konto musi mieć uprawnień wystarczających do przechodzą przez serwer proxy.  
    ![obraz Kreatora uruchamiania jako profilu](./media/log-analytics-proxy-firewall/proxy-proxyacct2.png)
5. Aby skonfigurować konto, aby zarządzać, aby otworzyć okno Wyszukiwanie obiektu wybierz pozycję **wybranej klasy, grupie lub obiektu** .  
    ![obraz Kreatora uruchamiania jako profilu](./media/log-analytics-proxy-firewall/proxy-proxyacct2-1.png)
6. Wyszukaj, a następnie wybierz **Grupę monitorowania serwerów Microsoft System Centrum Advisor**.  
    ![Obraz pola wyszukiwanie obiektu](./media/log-analytics-proxy-firewall/proxy-proxyacct3.png)
7. Kliknij **przycisk OK** , aby zamknąć Dodaj pole Konto Uruchom jako.  
    ![obraz Kreatora uruchamiania jako profilu](./media/log-analytics-proxy-firewall/proxy-proxyacct4.png)
8. Kończenie pracy kreatora i zapisać zmiany.  
    ![obraz Kreatora uruchamiania jako profilu](./media/log-analytics-proxy-firewall/proxy-proxyacct5.png)


### <a name="to-validate-that-oms-management-packs-are-downloaded"></a>Aby sprawdzić poprawność Zarządzanie usługi OMS pakiety są pobierane

Po dodaniu rozwiązania do usługi OMS, można wyświetlić w konsoli programu Operations Manager, jako pakietów zarządzania w obszarze **Administracja**. Wyszukaj *Advisor Centrum systemu* je szybko znaleźć.  
    ![pakiety zarządzania pobierane](./media/log-analytics-proxy-firewall/proxy-mpdownloaded.png) lub możesz również sprawdzić pakietów zarządzania usługi OMS przy użyciu następującego polecenia środowiska Windows PowerShell w programu Operations Manager management server:

    ```
    Get-ScomManagementPack | where {$_.DisplayName -match 'Advisor'} | select Name,DisplayName,Version,KeyToken
    ```

### <a name="to-validate-that-operations-manager-is-sending-data-to-the-oms-service"></a>Aby sprawdzić poprawność tego programu Operations Manager wysyłanie danych do usługę

1. Na serwerze zarządzania programu Operations Manager otwórz Monitor wydajności (perfmon.exe) i wybierz **Monitor wydajności**.
2. Kliknij przycisk **Dodaj**, a następnie wybierz pozycję **Kondycja usługi zarządzania grupy**.
3. Dodawanie liczników, które zaczynają się od **ciągu HTTP**.  
    ![Dodawanie liczników](./media/log-analytics-proxy-firewall/proxy-sendingdata1.png)
4. Jeśli konfiguracji programu Operations Manager jest dobrym, zobaczysz aktywności liczników kondycji usługi zarządzania dla zdarzeń i innych elementów danych, oparty na pakiety zarządzania dodanych w usługi OMS i zasady zbierania skonfigurowanego dziennika.  
    ![Działania przedstawiający monitorowanie wydajności](./media/log-analytics-proxy-firewall/proxy-sendingdata2.png)


## <a name="next-steps"></a>Następne kroki

- [Dodawanie analizy dziennika rozwiązań z galerii rozwiązań](log-analytics-add-solutions.md) Dodawanie funkcji do zbierania danych.
- Zapoznaj się z [wyszukiwania dziennika](log-analytics-log-searches.md) wyświetlić szczegółowe informacje zgromadzone przez rozwiązań.
