<properties
   pageTitle="Tworzenie bramy aplikacji przy użyciu interfejsu wiersza polecenia Azure w Menedżerze zasobów | Microsoft Azure"
   description="Dowiedz się, jak utworzyć bramy aplikacji przy użyciu interfejsu wiersza polecenia Azure w Menedżerze zasobów"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace" />

# <a name="create-an-application-gateway-by-using-the-azure-cli"></a>Tworzenie bramy aplikacji przy użyciu interfejsu wiersza polecenia Azure

> [AZURE.SELECTOR]
- [Azure portal](application-gateway-create-gateway-portal.md)
- [Azure PowerShell Menedżera zasobów](application-gateway-create-gateway-arm.md)
- [Azure klasycznego programu PowerShell](application-gateway-create-gateway.md)
- [Azure szablonu Menedżera zasobów](application-gateway-create-gateway-arm-template.md)
- [Polecenie Azure](application-gateway-create-gateway-cli.md)

Azure brama aplikacji jest równoważenia obciążenia warstwy-7. Zapewnia on awaryjnym przeniesieniu routingu wydajności żądania HTTP między różnych serwerów, czy są one na chmurze lub lokalnego. Brama aplikacji oferuje następujące funkcje dostarczenia aplikacji: HTTP ładowanie równoważenia, koligacji sesji plików cookie i odciążanie Secure Sockets Layer (SSL) sondy niestandardowe zdrowia i obsługę wielu witrynach.

## <a name="prerequisite-install-the-azure-cli"></a>Wymagania wstępne: Instalowanie polecenie Azure

Aby wykonać czynności opisane w tym artykule, należy [zainstalować Azure interfejs wiersza polecenia dla komputerów Mac, Linux i systemu Windows (polecenie Azure)](../xplat-cli-install.md) i musisz [zalogować się do Azure](../xplat-cli-connect.md). 

> [AZURE.NOTE] Jeśli nie masz konta usługi Azure, należy jedną. Zwiększenie logowania [bezpłatną wersję próbną tutaj](../active-directory/sign-up-organization.md).

## <a name="scenario"></a>Scenariusz

W tym scenariuszu możesz dowiedzieć się, jak utworzyć bramy aplikacji za pomocą portalu Azure.

W tym scenariuszu będzie:

- Tworzenie bramy średnie aplikacji z dwoma wystąpieniami.
- Tworzenie wirtualnych sieci o nazwie AdatumAppGatewayVNET z zastrzeżone blok CIDR 10.0.0.0/16.
- Tworzenie podsieci o nazwie Appgatewaysubnet, której używa 10.0.0.0/28 jako jego blok CIDR.
- Konfigurowanie certyfikatu SSL odciążania.

![Przykład scenariusza][scenario]

>[AZURE.NOTE] Sondy dodatkowej konfiguracji bramy aplikacji, w tym niestandardowe zdrowia, adresów puli wewnętrznej bazy danych i dodatkowe reguły są skonfigurowane po skonfigurowaniu bramy aplikacji, a nie podczas początkowego wdrażania.

## <a name="before-you-begin"></a>Przed rozpoczęciem

Azure bramy aplikacji wymaga osobnej podsieci. Podczas tworzenia wirtualnej sieci, upewnij się, pozostawienie miejsca adres do dostępnych jest kilka podsieci. Po rozmieszczeniu bramy aplikacji do podsieci bram tylko dodatkowe aplikacji są w stanie mają zostać dodane do danej podsieci.

## <a name="log-in-to-azure"></a>Zaloguj się do Azure

Otwórz okno **wiersza polecenia programu Microsoft Azure**i zaloguj się. 

    azure login

Po wpisaniu powyższym przykładzie znajduje się kod. Przejdź do https://aka.ms/devicelogin w przeglądarce, aby kontynuować proces logowania.

![Cmd pokazywanie urządzenia logowania][1]

W przeglądarce wprowadź kod otrzymany. Nastąpi przekierowanie do strony logowania.

![przeglądarki, aby wprowadzić kod][2]

Po kodzie zostały wprowadzone po zalogowaniu się, zamknij przeglądarkę, aby kontynuować tego scenariusza.

![Zalogowano pomyślnie][3]

## <a name="switch-to-resource-manager-mode"></a>Przełącz do trybu Menedżera zasobów

    azure config mode arm

## <a name="create-the-resource-group"></a>Tworzenie grupy zasobów

Przed utworzeniem bramy aplikacji, tworzona jest grupa zasobów zawierają bramy aplikacji. Poniżej przedstawiono polecenia.

    azure group create -n AdatumAppGatewayRG -l eastus

## <a name="create-a-virtual-network"></a>Tworzenie wirtualnych sieci

Po utworzeniu grupy zasobów, wirtualną sieć jest tworzona bramy aplikacji.  W poniższym przykładzie przestrzeni adresów była jako 10.0.0.0/16 zgodnie z definicją w poprzednim notatek scenariusza.

    azure network vnet create -n AdatumAppGatewayVNET -a 10.0.0.0/16 -g AdatumAppGatewayRG -l eastus

## <a name="create-a-subnet"></a>Tworzenie podsieci

Po utworzeniu wirtualną sieć podsieci zostanie dodany bramy aplikacji.  Jeśli planujesz bramy aplikacji za pomocą aplikacji sieci web hostowanej w tej samej sieci wirtualnej jako brama aplikacji, należy pozostawić miejsca na innej podsieci.

    azure network vnet subnet create -g AdatumAppGatewayRG -n Appgatewaysubnet -v AdatumAppGatewayVNET -a 10.0.0.0/28 

## <a name="create-the-application-gateway"></a>Tworzenie bramy aplikacji

Po utworzeniu wirtualnej sieci i podsieci wymagania wstępne bramy aplikacji zostały wykonane. Ponadto certyfikat wcześniej eksportowanego pliku PFX i hasło dla certyfikatu jest wymagane dla następny krok: adresy IP używane do wewnętrznej bazy danych są adresy IP serwera wewnętrznej bazy danych. Te wartości mogą być prywatne adresy IP w wirtualnej sieci, publiczne adresy IP lub w pełni kwalifikowanych nazw domen dla serwerów wewnętrznej bazy danych.

    azure network application-gateway create -n AdatumAppGateway -l eastus -g AdatumAppGatewayRG -e AdatumAppGatewayVNET -m Appgatewaysubnet -r 134.170.185.46,134.170.188.221,134.170.185.50 -y c:\AdatumAppGateway\adatumcert.pfx -x P@ssw0rd -z 2 -a Standard_Medium -w Basic -j 443 -f Enabled -o 80 -i http -b https -u Standard

> [AZURE.NOTE] Aby uzyskać listę parametrów, które może być udostępniana podczas tworzenia, uruchom następujące polecenie: **Tworzenie bramy aplikacji azure sieci — pomoc**.

W tym przykładzie powoduje bramy podstawowe aplikacji z domyślnych ustawień odbiornika, puli wewnętrznej bazy danych, ustawienia http wewnętrznej bazy danych i reguły. Konfiguruje go również odciążania SSL. Można modyfikować poniższe ustawienia do potrzeb wdrożenia, po pomyślnym inicjowania obsługi administracyjnej.
Jeśli masz już aplikacji sieci web zdefiniowane za pomocą puli wewnętrznej bazy danych w poprzednich krokach po utworzeniu równoważenia obciążenia zaczyna się.

## <a name="next-steps"></a>Następne kroki

Dowiedz się, jak tworzenie niestandardowych kondycji sondy, odwiedź witrynę [Tworzenie sondy kondycji niestandardowe](application-gateway-create-probe-portal.md)

Dowiedz się, jak skonfigurować odciążanie protokołu SSL i wykonać kosztów odszyfrowywania SSL wyłączanie serwerów sieci web, odwiedź witrynę [Skonfigurować odciążanie protokołu SSL](application-gateway-ssl-arm.md)

<!--Image references-->

[scenario]: ./media/application-gateway-create-gateway-cli/scenario.png
[1]: ./media/application-gateway-create-gateway-cli/figure1.png
[2]: ./media/application-gateway-create-gateway-cli/figure2.png
[3]: ./media/application-gateway-create-gateway-cli/figure3.png