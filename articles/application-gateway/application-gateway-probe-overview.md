

<properties
   pageTitle="Kondycja monitorowania — omówienie Azure aplikacji bramy | Microsoft Azure"
   description="Więcej informacji na temat możliwości monitorowania w polu Brama aplikacji Azure"
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

# <a name="application-gateway-health-monitoring-overview"></a>Omówienie monitorowania kondycji bramy aplikacji

Azure brama aplikacji domyślnie monitoruje kondycję wszystkich zasobów w jej puli wewnętrznej i są automatycznie usuwane wszelkie traktowane jako nieprawidłowe z puli zasobów. Brama aplikacja będzie nadal monitorować nieprawidłowe wystąpienia i dodaje je ponownie do puli wewnętrznej prawidłowy, gdy staną się dostępne i odpowiadanie na sondy zdrowia. Brama aplikacji wysyła kondycji sondy z tego samego portu, zdefiniowanego w ustawieniach HTTP wewnętrznej. Dzięki temu, że sondy testuje tego samego portu, że klienci będą stosowane nawiązywania połączenia z wewnętrznej bazy danych.

![przykład sondy bramy aplikacji][1]

Oprócz korzystania monitorowania sondy stanu domyślnego, można także dostosować sondy kondycji odpowiednio wymagania aplikacji. W tym artykule opisano zarówno domyślne i niestandardowe kondycji sondy.

## <a name="default-health-probe"></a>Sonda zdrowia domyślne

Konfigurując nie żadnej konfiguracji niestandardowych sondy bramy aplikacji automatycznie konfiguruje sondy zdrowia domyślne. Zachowanie monitorowania polega na wprowadzanie żądania HTTP adresy IP skonfigurowane dla puli wewnętrznej. Na sondy domyślne Jeśli ustawienia protokołu http wewnętrznej bazy danych są skonfigurowane dla HTTPS, sondy użyje https także aby przetestować zdrowia wewnętrznych bazach danych.

Na przykład: Konfigurowanie Centrum aplikacji do korzystania z serwerów wewnętrznej A, B i C do odbierania HTTP ruch sieciowy na porcie 80. Monitorowanie kondycji domyślne testów trzech serwerów co 30 sekund prawidłowy odpowiedź HTTP. Prawidłowy odpowiedź HTTP ma [kodu stanu](https://msdn.microsoft.com/library/aa287675.aspx) od 200 do 399.

Jeśli serwer A domyślnym sprawdzaniem sondy nie powiedzie się, bramy aplikacji powoduje usunięcie go z puli wewnętrznej, a ruch sieciowy przestaje ułożony do tego serwera. Sonda domyślną będzie nadal Sprawdź serwera co 30 sekund. Po serwer A odpowiada pomyślnie jedno żądanie z sondy zdrowia domyślne, dodaniu ponownie jako prawidłowy puli wewnętrznej i ruch zaczyna się ponownie ułożony na serwerze.

### <a name="default-health-probe-settings"></a>Domyślne ustawienia sondy kondycji

|Właściwość sondy | Wartość | Opis|
|---|---|---|
| Sonda adresu URL| http://127.0.0.1:\<port\>/ | Ścieżka adresu URL |
| Interwał | 30 | Sonda interwał w sekundach |
| Limit czasu  | 30 | Sonda limit czasu w sekundach |
| Nieprawidłowe progowa. | 3 | Sonda liczba powtórzeń. Serwer wewnętrznej jest oznaczony w dół, po licznik awarii sondy następujących po sobie osiągnięciu progu nieprawidłowe. |

> [AZURE.NOTE] Port będzie zawsze tego samego portu ustawienia HTTP wewnętrznej.

Sonda domyślne analizuje tylko w http://127.0.0.1:\<port\> do określenia stanu zdrowia. Jeśli musisz skonfigurować sondy zdrowia, aby przejść do niestandardowego adresu URL lub zmodyfikuj inne ustawienia, należy użyć niestandardowej sondy, zgodnie z opisem w poniższej procedurze.

## <a name="custom-health-probe"></a>Sonda kondycji niestandardowe

Niestandardowe sondy umożliwiają mieć większą kontrolę nad monitorowanie kondycji. Korzystając z niestandardowego sondy, można skonfigurować interwał sondy, adresu URL i ścieżkę, aby przetestować i ile negatywnych odpowiedzi zaakceptować przed oznaczeniem wystąpienia wewnętrznej puli jako nieprawidłowe.

### <a name="custom-health-probe-settings"></a>Ustawienia sondy kondycji niestandardowe

Poniższa tabela zawiera definicje właściwości sondy zdrowia niestandardowe.

|Właściwość sondy| Opis|
|---|---|
| Nazwa | Nazwa sondy. Ta nazwa jest używana dotyczyć sondy w ustawieniach HTTP wewnętrznej. |
| Protocol (protokół) | Protokół używany do wysyłania sondy. Sonda będzie korzystać z protokołu zdefiniowane w obszarze Ustawienia protokołu HTTP wewnętrznej |
| Hosta |  Nazwa hosta, aby wysłać sondy. Dotyczy tylko wtedy, gdy wiele witryn jest skonfigurowany dla bramy aplikacji, w przeciwnym razie użyj "127.0.0.1". To różni się od nazwy hosta maszyn wirtualnych. |
| Ścieżka | Ścieżka względna sondy. Nieprawidłowa ścieżka zaczyna się od "/". |
| Interwał | Sonda interwał w sekundach. To jest odstęp czasu między dwoma sondy następujących po sobie.|
| Limit czasu | Sonda limitu czasu w sekundach. Sonda jest oznaczone jako zakończone niepowodzeniem, jeśli nie nadchodzi prawidłowa odpowiedź w tym okresie limitu czasu. |
| Nieprawidłowe progowa. | Sonda liczba powtórzeń. Serwer wewnętrznej jest oznaczony w dół, po licznik awarii sondy następujących po sobie osiągnięciu progu nieprawidłowe. |

> [AZURE.IMPORTANT] Jeśli skonfigurowano bramy aplikacji dla jednej witryny, domyślnie hoście należy określić nazwę jako '127.0.0.1', chyba że skonfigurowane w inny sposób w sondy niestandardowe.
Odwołanie sondy niestandardowe są wysyłane do \<protokół\>://\<hosta\>:\<port\>\<ścieżki\>. Port używany będzie tego samego portu, zgodnie z definicją w obszarze Ustawienia protokołu HTTP wewnętrznej.

## <a name="next-steps"></a>Następne kroki

Po szkoleniowe na temat monitorowanie kondycji bramy aplikacji można skonfigurować [niestandardowe kondycji sondy](application-gateway-create-probe-portal.md) Azure portal lub [sondy kondycji niestandardowych](application-gateway-create-probe-ps.md) przy użyciu programu PowerShell i model wdrożenia Menedżera zasobów Azure.

[1]: ./media/application-gateway-probe-overview/appgatewayprobe.png