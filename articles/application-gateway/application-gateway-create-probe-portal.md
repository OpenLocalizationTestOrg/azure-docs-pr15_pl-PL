<properties
   pageTitle="Tworzenie niestandardowego sondy dla bramy aplikacji za pomocą portalu | Microsoft Azure"
   description="Dowiedz się, jak utworzyć niestandardowe sondy bramy aplikacji za pomocą portalu"
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

# <a name="create-a-custom-probe-for-application-gateway-by-using-the-portal"></a>Tworzenie niestandardowego sondy bramy aplikacji za pomocą portalu

> [AZURE.SELECTOR]
- [Azure portal](application-gateway-create-probe-portal.md)
- [Azure PowerShell Menedżera zasobów](application-gateway-create-probe-ps.md)
- [Azure klasycznego programu PowerShell](application-gateway-create-probe-classic-ps.md)

[AZURE.INCLUDE [azure-probe-intro-include](../../includes/application-gateway-create-probe-intro-include.md)]

## <a name="scenario"></a>Scenariusz

Następującym scenariuszu przechodzi przez tworzenie sondy niestandardowych kondycji w istniejącej bramy aplikacji.
Scenariusz zakłada już wykonane kroki opisane tworzenie [Bramy aplikacji](application-gateway-create-gateway-portal.md).

## <a name="createprobe"></a>Tworzenie sondy

Sondy są konfigurowane w dwóch krokach za pośrednictwem portalu. Pierwszym krokiem jest utworzenie sondy, następnie dodać sondy w ustawieniach http wewnętrznej bazy danych aplikacji bramy.

### <a name="step-1"></a>Krok 1

Przejdź do http://portal.azure.com i wybierz istniejący bramy aplikacji.

![Omówienie aplikacji bramy][1]

### <a name="step-2"></a>Krok 2

Kliknij **sondy** i kliknij przycisk **Dodaj** , aby dodać nowy sondy.

![Dodawanie karta sondy z wypełnionymi informacjami][2]

### <a name="step-3"></a>Krok 3

Wypełnij wymagane informacje dotyczące sondy, a po zakończeniu kliknij **przycisk OK**.

- **Nazwa** — jest to przyjazną nazwę, aby sonda, która jest dostępna w portalu.
- **Host** — jest to nazwa hosta, która służy do sondy. Dotyczy tylko wtedy, gdy wiele witryn jest skonfigurowany dla bramy aplikacji, w przeciwnym razie za pomocą '127.0.0.1'. To różni się od nazwy hosta maszyn wirtualnych.
- **Ścieżka** - pozostałą część pełnego adresu url niestandardowej sondy. Nieprawidłowa ścieżka zaczyna się od "/"
- **Interwał (w sekundach)** - częstotliwość sondy uruchomieniu sprawdzania kondycji. Nie zaleca się Ustawianie niższej niż 30 sekund.
- **Limit czasu (w sekundach)** — czas oczekiwania sondy limit czasu. Limit czasu musi być wystarczająco wysoka, że połączenia http można nawiązać upewnij się, że jest dostępna na stronie kondycja wewnętrznej bazy danych.
- **Próg nieprawidłowe** — liczba niepomyślne mają być traktowane jako nieprawidłowe. Próg 0 oznacza, że jeśli nie udaje się sprawdzanie kondycji wewnętrznej zostaną określone nieprawidłowe natychmiast.

> [AZURE.IMPORTANT] Nazwa hosta jest nazwa serwera. Jest to nazwa hosta wirtualnego uruchomiony na serwerze aplikacji. Sonda jest wysyłana do http://(host name):(port from httpsetting)-urlPath

![ustawienia konfiguracji sondy][3]

## <a name="add-probe-to-the-gateway"></a>Dodawanie sondy do bramy

Teraz, gdy został utworzony sondy jest godzina, aby dodać go do bramy. Sonda ustawienia na stronie Ustawienia protokołu http wewnętrznej bazy danych aplikacji bramy.

### <a name="step-1"></a>Krok 1

Kliknij pozycję **Ustawienia protokołu HTTP** bramy aplikacji, a następnie kliknij bieżące wewnętrznej bazy danych http ustawienia w oknie wywoływanie karta konfiguracji.

![Protokół HTTPS w oknie Ustawienia][4]

### <a name="step-2"></a>Krok 2

Na **appGatewayBackEndHttp** karta Ustawienia kliknij pozycję **Użyj niestandardowych sondy** i wybierz sondy utworzone w sekcji [Tworzenie sondy](#createprobe) .
Po zakończeniu kliknij **przycisk OK** , a ustawienia zostaną zastosowane.

![Karta Ustawienia appgatewaybackend][5]

Sonda domyślne sprawdza domyślnego dostępu do aplikacji sieci web. Teraz, gdy został utworzony niestandardowy sondy bramy aplikacji używa ścieżkę niestandardową zdefiniowane monitorowanie kondycji dla wewnętrznej bazy danych zaznaczone. W oparciu o kryteria, które zostało zdefiniowane, bramy aplikacji sprawdza pliku określonego w sondy. Jeśli połączenie host: Port / ścieżki nie zwraca odpowiedzi Stan Http 200 OK, serwer jest przyjmowana poza obrót po osiągnięciu progu nieprawidłowe. Badanie będzie nadal Nieprawidłowe wystąpienie, aby określić, kiedy prawidłowy się ponownie. Po dodaniu wystąpienia ponownie puli prawidłowy server ruch rozpoczynają się, ułożony do niego ponownie, a badanie do wystąpienia ich w określonych odstępach użytkownika w zwykły sposób.


## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się, jak skonfigurować odciążanie protokołu SSL Azure aplikacji bramy zobacz [Konfigurowanie Offload SSL](application-gateway-ssl-portal.md)

[1]: ./media/application-gateway-create-probe-portal/figure1.png
[2]: ./media/application-gateway-create-probe-portal/figure2.png
[3]: ./media/application-gateway-create-probe-portal/figure3.png
[4]: ./media/application-gateway-create-probe-portal/figure4.png
[5]: ./media/application-gateway-create-probe-portal/figure5.png