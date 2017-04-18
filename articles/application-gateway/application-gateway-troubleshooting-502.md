<properties
   pageTitle="Rozwiązywanie problemów z błędami aplikacji bramy Nieprawidłowa brama (502) | Microsoft Azure"
   description="Dowiedz się, jak rozwiązywanie problemów z błędami 502 bramy aplikacji"
   services="application-gateway"
   documentationCenter="na"
   authors="amitsriva"
   manager="rossort"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/02/2016"
   ms.author="amitsriva" />

# <a name="troubleshooting-bad-gateway-errors-in-application-gateway"></a>Rozwiązywanie problemów z błędami Nieprawidłowa brama w aplikacji bramy

## <a name="overview"></a>Omówienie

Po skonfigurowaniu bramy aplikacji Azure, jeden z błędów, które użytkownicy mogą wystąpić jest "Błąd serwera: 502 — Serwer sieci Web otrzymał nieprawidłową odpowiedź podczas pracy w trybie bramy lub serwera proxy". Może się to zdarzyć z powodu następujących powodów:

- Pula wewnętrznej Azure bramy aplikacji nie jest skonfigurowane lub jest pusta.
- Żadna z maszyny wirtualne lub wystąpień w zestawie skali maszyn wirtualnych jest prawidłowy.
- Maszyny wirtualne lub wystąpienia zestawu skali maszyn wirtualnych wewnętrznej nie odpowiada sondy kondycji domyślne.
- Nieprawidłowe lub nieprawidłowa konfiguracja sondy kondycji niestandardowe.
- Żądanie limitu czasu lub problemy z łącznością z żądania użytkowników.

## <a name="empty-backendaddresspool"></a>Pusty BackendAddressPool

### <a name="cause"></a>Przyczyna

Jeśli bramy aplikacji nie posiada maszyny wirtualne lub maszyn wirtualnych Skala Ustaw skonfigurowane w puli adresów wewnętrznej, nie będą mogli przekierowywać dowolnego żądania klienta i zgłasza błąd Nieprawidłowa brama.

### <a name="solution"></a>Rozwiązanie

Upewnij się, puli adresów wewnętrznej nie jest puste. Można to zrobić albo za pomocą programu PowerShell, polecenie lub portalu.

    Get-AzureRmApplicationGateway -Name "SampleGateway" -ResourceGroupName "ExampleResourceGroup"

Dane wyjściowe z poprzedniego polecenia cmdlet powinien zawierać niepustych wewnętrznej adresów. Oto przykład miejsce, w którym są zwracane dwóch pul którego skonfigurowano FQDN lub adresów IP dla wewnętrznej bazy danych maszyny wirtualne. Stan obsługi administracyjnej BackendAddressPool musi być "powiodło się".
    
    BackendAddressPoolsText: 
            [{
                "BackendAddresses": [{
                    "ipAddress": "10.0.0.10",
                    "ipAddress": "10.0.0.11"
                }],
                "BackendIpConfigurations": [],
                "ProvisioningState": "Succeeded",
                "Name": "Pool01",
                "Etag": "W/\"00000000-0000-0000-0000-000000000000\"",
                "Id": "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name>/backendAddressPools/pool01"
            }, {
                "BackendAddresses": [{
                    "Fqdn": "xyx.cloudapp.net",
                    "Fqdn": "abc.cloudapp.net"
                }],
                "BackendIpConfigurations": [],
                "ProvisioningState": "Succeeded",
                "Name": "Pool02",
                "Etag": "W/\"00000000-0000-0000-0000-000000000000\"",
                "Id": "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name>/backendAddressPools/pool02"
            }]


## <a name="unhealthy-instances-in-backendaddresspool"></a>Nieprawidłowe wystąpień w BackendAddressPool

### <a name="cause"></a>Przyczyna

Jeśli wszystkie wystąpienia BackendAddressPool są nieprawidłowe, w aplikacji bramy czy nie ma dowolną wewnętrzną rozsyłania żądanie użytkownika. Przyczyną może być również przypadku gdy wystąpienia wewnętrznej jest prawidłowy, ale nie jest wymagana aplikacja wdrożony.

### <a name="solution"></a>Rozwiązanie

Upewnij się, że wystąpienia są prawidłowy i aplikacja jest poprawnie skonfigurowany. Sprawdź, czy wystąpienia wewnętrznej są w stanie odpowiedzieć na polecenie ping w czasie z innego maszyn wirtualnych w tym samym VNet. Jeśli skonfigurowano publiczną punkt końcowy, upewnij się, że żądania przeglądarki dla aplikacji sieci web jest zdatne do użytku.

## <a name="problems-with-default-health-probe"></a>Problemy z sondy kondycji domyślne

### <a name="cause"></a>Przyczyna

502 błędów można także częste wskaźniki sondy zdrowia domyślne nie jest dostęp do wewnętrznej maszyny wirtualne. Gdy wystąpienie aplikacji bramy jest obsługi administracyjnej, automatycznie konfiguruje sondy zdrowia domyślnego do każdego BackendAddressPool za pomocą właściwości BackendHttpSetting. Dane wejściowe użytkownika nie jest wymagane do ustawienia sonda. W szczególności reguły równoważenia obciążenia jest skonfigurowany, skojarzenia się między BackendHttpSetting i BackendAddressPool. Sonda domyślny jest skonfigurowane dla każdego z tych skojarzeń i bramy aplikacji inicjuje połączenie wyboru okresowego każdego wystąpienia w BackendAddressPool na porcie określonego w elemencie BackendHttpSetting. Poniższa tabela zawiera wartości, skojarzone z sondy zdrowia domyślne.


|Właściwość sondy | Wartość | Opis|
|---|---|---|
| Sonda adresu URL| http://127.0.0.1/ | Ścieżka adresu URL |
| Interwał | 30 | Sonda interwał w sekundach |
| Limit czasu  | 30 | Sonda limit czasu w sekundach |
| Nieprawidłowe progowa. | 3 | Sonda liczba powtórzeń. Serwer wewnętrznej jest oznaczony w dół, po licznik awarii sondy następujących po sobie osiągnięciu progu nieprawidłowe. |

### <a name="solution"></a>Rozwiązanie

- Upewnij się, że domyślnej witryny jest skonfigurowana i jest w elemencie 127.0.0.1.
- Jeśli BackendHttpSetting Określa port niż 80, aby odsłuchać tego portu należy skonfigurować domyślnej witryny.
- Połączenie http://127.0.0.1:port powinna zwrócić kod wyniku HTTP 200. To powinna zostać zwrócone w ciągu 30 sekund limitu czasu.
- Upewnij się, że port skonfigurowany jest otwarty i czy nie reguły zapory lub grupy zabezpieczeń sieci Azure, której zablokować ruch wychodzących lub przychodzących na porcie skonfigurowane.
- Jeśli Azure klasyczny, które maszyny wirtualne lub usługa w chmurze jest używana z nazwy FQDN lub publiczny adres IP, upewnij się, że odpowiednie [punkt końcowy](../virtual-machines/virtual-machines-windows-classic-setup-endpoints.md) zostanie otwarty.
- Jeśli maszyn wirtualnych jest skonfigurowany za pomocą Menedżera zasobów Azure i wykracza poza VNet miejsce, w którym zostanie wdrożony bramy aplikacji, [Grupy zabezpieczeń sieci](../virtual-network/virtual-networks-nsg.md) musi być skonfigurowany umożliwia dostęp do wybranego portu.


## <a name="problems-with-custom-health-probe"></a>Problemy z sondy zdrowia niestandardowe

### <a name="cause"></a>Przyczyna

Niestandardowe zdrowia sondy Zezwalaj dodatkową elastyczność badanie zachowanie domyślne. Korzystając z niestandardowego sondy, użytkownicy mogą skonfigurować interwał sondy, adres URL i ścieżkę, aby przetestować i ile negatywnych odpowiedzi zaakceptować przed oznaczeniem wystąpienia wewnętrznej puli jako nieprawidłowe. Następujące dodatkowe właściwości są dodawane.

|Właściwość sondy| Opis|
|---|---|
| Nazwa | Nazwa sondy. Ta nazwa jest używana dotyczyć sondy w ustawieniach HTTP wewnętrznej. |
| Protocol (protokół) | Protokół używany do wysyłania sondy. Sonda będzie korzystać z protokołu zdefiniowane w obszarze Ustawienia protokołu HTTP wewnętrznej |
| Hosta |  Nazwa hosta, aby wysłać sondy. Dotyczy tylko wtedy, gdy skonfigurowano wiele witryn na bramie aplikacji. To różni się od nazwy hosta maszyn wirtualnych.  |
| Ścieżka | Ścieżka względna sondy. Nieprawidłowa ścieżka zaczyna się od "/". Sonda jest wysyłana do \<protokół\>://\<hosta\>:\<port\>\<ścieżki\> |
| Interwał | Sonda interwał w sekundach. To jest odstęp czasu między dwoma sondy następujących po sobie.|
| Limit czasu | Sonda limitu czasu w sekundach. Sonda jest oznaczone jako zakończone niepowodzeniem, jeśli nie nadchodzi prawidłowa odpowiedź w tym okresie limitu czasu. |
| Nieprawidłowe progowa. | Sonda liczba powtórzeń. Serwer wewnętrznej jest oznaczony w dół, po licznik awarii sondy następujących po sobie osiągnięciu progu nieprawidłowe. |


### <a name="solution"></a>Rozwiązanie

Sprawdź, czy sondy kondycji niestandardowe jest skonfigurowana jako powyższej tabeli. Oprócz powyższych kroków rozwiązywania problemów również sprawdzić, czy:

- Upewnij się, że sondy jest poprawnie określony zgodnie z [przewodnika](application-gateway-create-probe-ps.md).
- Jeśli skonfigurowano bramy aplikacji dla jednej witryny, domyślnie hoście należy określić nazwę jako '127.0.0.1', chyba że skonfigurowane w inny sposób w sondy niestandardowe.
- Upewnij się, że połączenia http://\<hosta\>:\<port\>\<ścieżki\> zwraca kod wyniku HTTP 200.
- Upewnij się, że interwał limitu czasu i UnhealtyThreshold są dopuszczalne zakresy.

## <a name="request-time-out"></a>Limit czasu żądania

### <a name="cause"></a>Przyczyna

Po otrzymaniu żądania użytkownika aplikacji brama stosuje reguły skonfigurowane do wezwania i przekierowuje go do wystąpienia wewnętrznej puli. Oczekuje na można konfigurować interwał czasu na odpowiedź z wystąpienia wewnętrznej. Domyślnie ten interwał wynosi **30 sekund**. Jeśli aplikacja bramy nie otrzymujesz odpowiedź od wewnętrznej aplikacji w tym okresie, żądanie użytkownika zobaczy 502 błąd.

### <a name="solution"></a>Rozwiązanie

Brama aplikacji pozwala użytkownikom na konfigurowanie ustawienie BackendHttpSetting, które mogą być następnie zastosowane do różnych zestawów. Różne pule wewnętrznej może mieć różne BackendHttpSetting i limitu czasu żądania w związku z tym różne skonfigurowane.

    New-AzureRmApplicationGatewayBackendHttpSettings -Name 'Setting01' -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 60

## <a name="next-steps"></a>Następne kroki

Jeśli te czynności nie rozwiąże problemu, otwórz [obsługuje biletów](https://azure.microsoft.com/support/options/).
