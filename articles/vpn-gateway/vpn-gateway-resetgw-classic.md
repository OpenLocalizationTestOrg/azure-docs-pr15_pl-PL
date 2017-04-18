<properties
   pageTitle="Resetowanie brama Azure VPN | Microsoft Azure"
   description="W tym artykule opisano Resetowanie bramy sieci VPN Azure. Artykuł dotyczy VPN bram w klasycznym i modeli wdrażania Menedżera zasobów."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager,azure-service-management"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="cherylmc"/>

# <a name="reset-an-azure-vpn-gateway-using-powershell"></a>Resetowanie Brama VPN Azure przy użyciu programu PowerShell


W tym artykule opisano Resetowanie Azure bramy sieci VPN, przy użyciu poleceń cmdlet programu PowerShell. Te instrukcje obejmują zarówno model klasyczny wdrażania, jak i model wdrożenia Menedżera zasobów.

Resetowanie brama Azure VPN jest przydatne w przypadku utraty połączenia VPN między lokalnej na jeden lub więcej tuneli S2S VPN. W tej sytuacji urządzenia VPN lokalnego wszystkie działają poprawnie, ale nie są w stanie ustanawiać tunele IPsec z bram Azure VPN. 

Każdy brama Azure VPN składa się z dwóch wystąpień maszyn wirtualnych w konfiguracji wstrzymania aktywne. Użycie polecenia cmdlet programu PowerShell Resetowanie bramy, ponownie bramy, a następnie przywrócenie konfiguracji lokalnej krzyżowe do niego. Brama przechowywana publiczny adres IP, który jest już. Oznacza to, że nie musisz zaktualizować konfigurację routera VPN z nowego publiczny adres IP brama Azure VPN.  

Po wydaniu polecenia bieżącego wystąpienia aktywne brama Azure VPN jest uruchomiony ponownie natychmiast. Będzie przerwy krótki podczas awaryjne z aktywnego wystąpienia (przy ponownym uruchomieniu) do wstrzymania wystąpienia. Odstępu powinna być mniejsza niż minutę.

Jeśli połączenie nie zostanie przywrócona po ponownym, problemów z tego samego polecenia ponownie, aby ponownie uruchomić drugiego wystąpienia maszyn wirtualnych (Nowa brama aktywna). Jeśli tuż wymagane są dwa ponownego uruchamiania, będzie nieco dłuższym okresie miejsce, w którym są uruchomienia oba wystąpienia maszyn wirtualnych (aktywna i wstrzymania). Spowoduje to dłuższa przerwa na połączenia VPN, aż do 2 do 4 minut maszyny wirtualne do wykonania jest uruchamiany ponownie.

Po dwóch uruchamiany ponownie Jeśli nadal występują problemy z łącznością między lokalnej, otwórz prośbę o pomoc techniczną z portalu Azure.

## <a name="before-you-begin"></a>Przed rozpoczęciem

Przed zresetowaniem Centrum Sprawdź kluczowe elementy znajdujące się poniżej dla każdego tunelem VPN IPsec--witryny (S2S). Wszelkie niezgodności elementów spowoduje Rozłącz tuneli S2S VPN. Sprawdzanie i poprawianie konfiguracji sieci lokalnej i bram Azure VPN pozwala niepotrzebne ponownego uruchamiania i zakłóceń dla innych połączeń pracy na bramy.

Przed zresetowaniem Centrum, sprawdź następujące elementy:

- Adresy Internet IP (służącą) dla bramy Azure VPN i Brama VPN lokalnego są skonfigurowane poprawnie zarówno Azure i lokalnych zasad sieci VPN.
- Klucz wstępny musi być taka sama na zarówno Azure i lokalnych bram sieci VPN.
- W przypadku zastosowania określonej konfiguracji IPsec/IKE, takich jak szyfrowania, mieszania algorytmów i PFS (doskonałe bezpieczeństwo przesyłania), upewnij się, że Azure i lokalnych bram VPN jest tym samym konfiguracji.

## <a name="reset-a-vpn-gateway-using-the-resource-management-deployment-model"></a>Resetowanie bramy sieci VPN przy użyciu modelu wdrożenia zarządzanie zasobami

Polecenia cmdlet programu PowerShell Menedżera zasobów resetowania bramy jest `Reset-AzureRmVirtualNetworkGateway`. Poniższy przykład resetuje brama Azure VPN, "VNet1GW", w grupie zasobów "TestRG1".

    $gw = Get-AzureRmVirtualNetworkGateway -Name VNet1GW -ResourceGroup TestRG1
    Reset-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw

## <a name="reset-a-vpn-gateway-using-the-classic-deployment-model"></a>Resetowanie bramy sieci VPN przy użyciu modelu Klasyczny wdrażania

Polecenia cmdlet programu PowerShell na potrzeby resetowania brama Azure VPN jest `Reset-AzureVNetGateway`. Poniższy przykład resetuje bramy Azure VPN wirtualną sieć o nazwie "ContosoVNet".
 
    Reset-AzureVNetGateway –VnetName “ContosoVNet” 

Wynik:

    Error          :
    HttpStatusCode : OK
    Id             : f1600632-c819-4b2f-ac0e-f4126bec1ff8
    Status         : Successful
    RequestId      : 9ca273de2c4d01e986480ce1ffa4d6d9
    StatusCode     : OK


## <a name="next-steps"></a>Następne kroki
    
Zobacz [informacje dotyczące poleceń cmdlet programu PowerShell usługi zarządzania](https://msdn.microsoft.com/library/azure/mt617104.aspx) i [informacje dotyczące poleceń cmdlet programu PowerShell Menedżera zasobów](http://go.microsoft.com/fwlink/?LinkId=828732) , aby uzyskać więcej informacji.






