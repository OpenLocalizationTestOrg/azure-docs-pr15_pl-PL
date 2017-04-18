<properties 
    pageTitle="Jak skonfigurować pamięci podręcznej Azure Redis | Microsoft Azure"
    description="Opis konfiguracji Redis domyślnej Azure Redis w pamięci podręcznej i Dowiedz się, jak skonfigurować wystąpienia usługi Azure Redis w pamięci podręcznej"
    services="redis-cache"
    documentationCenter="na"
    authors="steved0x"
    manager="douge"
    editor="tysonn" />
<tags 
    ms.service="cache"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="08/25/2016"
    ms.author="sdanie" />

# <a name="how-to-configure-azure-redis-cache"></a>Jak skonfigurować Azure Redis w pamięci podręcznej

W tym temacie opisano, jak przeglądać i zaktualizować konfigurację wystąpienia usługi Azure Redis w pamięci podręcznej i obejmuje konfiguracji serwera Redis domyślne dla wystąpień Azure Redis w pamięci podręcznej.

>[AZURE.NOTE] Aby uzyskać więcej informacji na konfigurowanie i używanie funkcji premium pamięci podręcznej zobacz [jak skonfigurować pozostawanie na Premium Azure Redis potrzeby pamięci podręcznej](cache-how-to-premium-persistence.md), [jak skonfigurować klaster na Premium Azure Redis potrzeby pamięci podręcznej](cache-how-to-premium-clustering.md)i [sposobu konfigurowania wirtualną sieć obsługę bufor Redis Azure Premium](cache-how-to-premium-vnet.md).

## <a name="configure-redis-cache-settings"></a>Konfigurowanie ustawień pamięci podręcznej Redis

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-browse.md)]

Azure Redis pamięci podręcznej zawiera następujące ustawienia dotyczące karta **Ustawienia** .

![Redis ustawienia pamięci podręcznej](./media/cache-configure/redis-cache-settings.png)



-   [Pomoc techniczna i rozwiązywanie problemów z ustawieniami](#support-amp-troubleshooting-settings)
-   [Ustawienia ogólne](#general-settings)
    -   [Właściwości](#properties)
    -   [Klawisze dostępu](#access-keys)
    -   [Ustawienia zaawansowane](#advanced-settings)
    -   [Redis Advisor pamięci podręcznej](#redis-cache-advisor)
-   [Ustawienia skali](#scale-settings)
    -   [Cennik warstwy](#pricing-tier)
    -   [Redis klaster rozmiar](#cluster-size)
-   [Ustawienia zarządzania danymi](#data-management-settings)
    -   [Redis utrzymywanie danych](#redis-data-persistence)
    -   [Importuj/Eksportuj](#importexport)
-   [Ustawienia administracyjne](#administration-settings)
    -   [Uruchom ponownie komputer](#reboot)
    -   [Planowanie aktualizacji](#schedule-updates)
-   [Ustawienia diagnostyki](#diagnostics-settings)
-   [Ustawienia sieci](#network-settings)
-   [Ustawienia zarządzania zasobów](#resource-management-settings)

## <a name="support--troubleshooting-settings"></a>Pomoc techniczna i rozwiązywanie problemów z ustawieniami

Ustawienia w sekcji **Obsługa + rozwiązywania problemów** udostępnia opcje rozwiązywania problemów związanych z pamięci podręcznej.

![Obsługa + Rozwiązywanie problemów](./media/cache-configure/redis-cache-support-troubleshooting.png)

Kliknij pozycję **diagnozowanie i rozwiązywanie problemów z** dostarczanych z typowych problemów i strategie dotyczące rozwiązywania tych problemów.

Kliknij pozycję **Dziennik** , aby wyświetlić akcje wykonywane w pamięci podręcznej. Za pomocą filtrowania można również rozwinąć ten widok, aby uwzględnić inne zasoby. Aby uzyskać więcej informacji o pracy z dzienników inspekcji Zobacz [Wyświetlanie zdarzeń i dzienników inspekcji](../monitoring-and-diagnostics/insights-debugging-with-events.md) i [Inspekcja operacji przy użyciu Menedżera zasobów](../resource-group-audit.md). Aby uzyskać więcej informacji dotyczących monitorowania zdarzenia Azure Redis w pamięci podręcznej zobacz [Operacje i alerty](cache-how-to-monitor.md#operations-and-alerts).

**Kondycja zasobów** oczekuje zasobu i wskazuje, czy jest uruchomiony w zgodnie z oczekiwaniami. Aby uzyskać więcej informacji o usłudze kondycji zasobu Azure zobacz [Omówienie kondycji Azure zasobów](../resource-health/resource-health-overview.md).

>[AZURE.NOTE] Kondycja zasobu nie może obecnie informowanie o kondycji wystąpień Azure Redis w pamięci podręcznej obsługiwany w wirtualnej sieci. Aby uzyskać więcej informacji, zobacz [wszystkie funkcje pamięci podręcznej działają podczas hostingu pamięci podręcznej w VNET?](cache-how-to-premium-vnet.md#do-all-cache-features-work-when-hosting-a-cache-in-a-vnet)

Kliknij przycisk **Nowy obsługuje żądania** otwórz prośbę o pomoc techniczną dla pamięci podręcznej.

## <a name="general-settings"></a>Ustawienia ogólne

Ustawienia w sekcji **Ogólne** umożliwiają dostęp do i skonfigurować następujące ustawienia dla pamięci podręcznej.

![Ustawienia ogólne](./media/cache-configure/redis-cache-general-settings.png)

-   [Właściwości](#properties)
-   [Klawisze dostępu](#access-keys)
-   [Ustawienia zaawansowane](#advanced-settings)
-   [Redis Advisor pamięci podręcznej](#redis-cache-advisor)

### <a name="properties"></a>Właściwości

Kliknij pozycję **Właściwości** , aby wyświetlić informacje dotyczące pamięci podręcznej, łącznie z pamięci podręcznej punktu końcowego i porty.

![Redis właściwości pamięci podręcznej](./media/cache-configure/redis-cache-properties.png)

### <a name="access-keys"></a>Klawisze dostępu

Kliknij przycisk **klawiszy dostępu** do wyświetlania lub wygenerować klawisze dostępu dla pamięci podręcznej. Te klawisze są używane razem z nazwą hosta i porty z karta **Właściwości** przez klientów łączących się z pamięci podręcznej.

![Redis klawiszy dostępu pamięci podręcznej](./media/cache-configure/redis-cache-manage-keys.png)






### <a name="advanced-settings"></a>Ustawienia zaawansowane

Karta **Zaawansowane ustawienia** są skonfigurować następujące ustawienia.

-   [Porty dostępu](#access-ports)
-   [Zasady Maxmemory i zastrzeżone maxmemory](#maxmemory-policy-and-maxmemory-reserved)
-   [Powiadomienia Keyspace (ustawienia zaawansowane)](#keyspace-notifications-advanced-settings)


### <a name="access-ports"></a>Porty dostępu

Domyślnie dostęp bez użycia protokołu SSL jest wyłączony dla nowej pamięci podręcznej. Aby włączyć port bez użycia protokołu SSL, kliknij pozycję **nie** **Zezwalaj na dostęp tylko za pośrednictwem protokołu SSL** na **Karta Ustawienia zaawansowane** , a następnie kliknij przycisk **Zapisz**.

![Porty dostępu redis pamięci podręcznej](./media/cache-configure/redis-cache-access-ports.png)

### <a name="maxmemory-policy-and-maxmemory-reserved"></a>Zasady Maxmemory i zastrzeżone maxmemory

Ustawienia **zasad Maxmemory** i **zastrzeżone maxmemory** na karta **Ustawienia zaawansowane** Konfigurowanie zasad pamięci, pamięci podręcznej. Ustawienia **zasad maxmemory** konfiguruje zasady eksmisji zwiększany pamięci podręcznej i **zastrzeżone maxmemory** konfiguruje pamięci zarezerwowane dla procesów innych niż pamięci podręcznej.

![Redis zasady Maxmemory pamięci podręcznej](./media/cache-configure/redis-cache-maxmemory-policy.png)

**Zasady Maxmemory** można wybrać z następujących zasad eksmisji zwiększany.

-   lru lotnych — jest to wartość domyślna.
-   allkeys lru
-   przypadkowe lotnych
-   przypadkowe allkeys
-   ttl lotnych
-   noeviction

Aby uzyskać więcej informacji o zasadach maxmemory zobacz [zasady eksmisji zwiększany](http://redis.io/topics/lru-cache#eviction-policies).

Ustawienie **zastrzeżone maxmemory** konfiguruje ilość pamięci w Megabajtach zarezerwowane dla innych niż pamięci podręcznej operacji, takich jak replikacji podczas pracy awaryjnej. Go można również w przypadku gdy dostępne stosunek fragmentacji wysoki. Ta wartość umożliwia mieć bardziej spójny wygląd serwera Redis zmienia się z ładowania. Ta wartość powinna być ustawiona większe obciążenie pracą, które są pisanie ciężkich. Jeśli pamięć jest zarezerwowana dla tych operacji jest niedostępna dla przechowywania danych w pamięci podręcznej.

>[AZURE.IMPORTANT] Ustawienie **zastrzeżone maxmemory** jest dostępna tylko dla standardowe i umieszcza Premium w pamięci podręcznej.

### <a name="keyspace-notifications-advanced-settings"></a>Powiadomienia Keyspace (ustawienia zaawansowane)

Redis keyspace, powiadomienia są skonfigurowane na karta **Ustawienia zaawansowane** . Powiadomienia o Keyspace umożliwić klientom otrzymywać powiadomienia w przypadku wystąpienia określonych zdarzeń.

![Redis pamięci podręcznej ustawienia zaawansowane](./media/cache-configure/redis-cache-advanced-settings.png)

>[AZURE.IMPORTANT] Powiadomienia Keyspace i ustawienie **Powiadom keyspace zdarzenia** są dostępne tylko dla standardowe i umieszcza Premium w pamięci podręcznej.

Aby uzyskać więcej informacji zobacz [Redis Keyspace powiadomienia](http://redis.io/topics/notifications). Przykładowy kod zobacz plik [KeySpaceNotifications.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/KeySpaceNotifications.cs) w próbce [Witaj świecie](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) .

<a name="recommendations"></a>
## <a name="redis-cache-advisor"></a>Redis Advisor pamięci podręcznej

Karta **zalecenia** wyświetla zalecenia dotyczące pamięci podręcznej. Podczas normalnego działania są wyświetlane żadne zalecenia. 

![Zalecenia](./media/cache-configure/redis-cache-no-recommendations.png)

Jeśli podczas operacji pamięć podręczną, takich jak wysokie użycie pamięci, przepustowość sieci lub obciążenie serwera wystąpią wszystkie warunki, zostanie wyświetlony alert na karta **Redis pamięci podręcznej** .

![Zalecenia](./media/cache-configure/redis-cache-recommendations-alert.png)

Więcej informacji można znaleźć w karta **zalecenia** .

![Zalecenia](./media/cache-configure/redis-cache-recommendations.png)

Można monitorować te metryki na [wykresach monitorowania](cache-how-to-monitor.md#monitoring-charts) i [wykresach zastosowania](cache-how-to-monitor.md#usage-charts) sekcje karta **Redis pamięci podręcznej** .

Każdy cennik warstwie jest różne limity dotyczące połączeń klientów, pamięci i przepustowość. Jeśli pamięć podręczną osiąga maksymalnej pojemności dla tych metryki przez dłuższy czas, zostanie utworzony zalecenia. Aby uzyskać więcej informacji na temat wskaźników i limity przeglądany przez narzędzie **zalecenia** Zobacz poniższej tabeli.

| Redis metryki pamięci podręcznej      | Aby uzyskać więcej informacji, zobacz                                                  |
|-------------------------|---------------------------------------------------------------------------|
| Wykorzystania przepustowości sieci | [Wydajność pamięci podręcznej — dostępna przepustowość](cache-faq.md#cache-performance) |
| Połączonych klientów       | [Domyślne konfiguracji serwera Redis - maxclients](#maxclients)            |
| Obciążenie serwera             | [Wykresy zastosowania - Redis obciążenie serwera](cache-how-to-monitor.md#usage-charts)  |
| Użycie pamięci            | [Wydajność pamięci podręcznej — rozmiar](cache-faq.md#cache-performance)                |

Aby uaktualnić pamięć podręczną, kliknij pozycję **Uaktualnij teraz** zmienianie [ceny warstwy](#pricing-tier) i skalowanie zawartości pamięci podręcznej. Aby uzyskać więcej informacji dotyczących wybierania cennik warstwa, zobacz [jakie oferująca Redis pamięci podręcznej i rozmiar należy używać?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).

## <a name="scale-settings"></a>Ustawienia skali

Ustawienia w sekcji **skali** umożliwiają dostęp do i skonfigurować następujące ustawienia dla pamięci podręcznej.

![Sieci](./media/cache-configure/redis-cache-scale.png)

-   [Cennik warstwy](#pricing-tier)
-   [Redis klaster rozmiar](#cluster-size)

### <a name="pricing-tier"></a>Cennik warstwy

Kliknij przycisk **warstwy ceny** , aby wyświetlić lub zmienić warstwie cennik dla pamięci podręcznej. Aby uzyskać więcej informacji o skalowania zobacz [jak skala Azure Redis w pamięci podręcznej](cache-how-to-scale.md).

![Redis pamięci podręcznej ceny warstwy](./media/cache-configure/pricing-tier.png)

<a name="cluster-size"></a>
### <a name="redis-cluster-size"></a>Redis klaster rozmiar

Kliknij pozycję **Rozmiar klaster Redis (wersja PREVIEW)** , aby zmienić rozmiar klaster dla bieżącej pamięci podręcznej premium klaster włączone.

>[AZURE.NOTE] Zauważ, że podczas warstwie Azure Redis pamięci podręcznej Premium zostanie wydana ogólnodostępną, funkcja Redis rozmiar klaster jest obecnie w podglądzie.

![Redis klaster rozmiar](./media/cache-configure/redis-cache-redis-cluster-size.png)

Aby zmienić rozmiar klaster, za pomocą suwaka lub wpisz liczbę z zakresu od 1 do 10 w polu tekstowym **Liczba Shard** i kliknij **przycisk OK** , aby zapisać.

>[AZURE.IMPORTANT] Klaster redis jest dostępna tylko dla pamięci podręcznej Premium. Aby uzyskać więcej informacji zobacz [jak skonfigurować klaster na Premium Azure Redis potrzeby pamięci podręcznej](cache-how-to-premium-clustering.md).


## <a name="data-management-settings"></a>Ustawienia zarządzania danymi

Ustawienia w sekcji **Zarządzanie danymi** umożliwiają dostęp do i skonfigurować następujące ustawienia dla pamięci podręcznej.

![Zarządzanie danymi](./media/cache-configure/redis-cache-data-management.png)

-   [Redis utrzymywanie danych](#redis-data-persistence)
-   [Importuj/Eksportuj](#importexport)

### <a name="redis-data-persistence"></a>Redis utrzymywanie danych

Kliknij pozycję **Redis utrzymywanie danych** , aby włączyć, wyłączyć lub skonfigurować utrzymywanie danych dla pamięci podręcznej premium.

![Redis utrzymywanie danych](./media/cache-configure/redis-cache-persistence-settings.png)

Aby włączyć utrzymywanie Redis, kliknij **włączone** wykonywanie kopii zapasowych RDB (Redis bazy danych). Aby wyłączyć trwałe Redis, kliknij opcję **wyłączone**.

Aby skonfigurować interwał wykonywania kopii zapasowej, wybierz **Częstotliwość kopii zapasowej** z listy rozwijanej. Można wybierać **15 minut**, **30 minut**, **60 minut**, **6 godzin**, **12 godzin**i **24 godziny**. Ten interwał uruchomiony, licząc w dół po poprzedniej wykonywanie kopii zapasowej powiedzie i po jego upływie została zainicjowana nową kopię zapasową.

Kliknij **Konto miejsca do magazynowania** do przechowywania umożliwia wybór konta, a określanie **klucza podstawowego** lub **klucza pomocniczego** do używania z listy rozwijanej **Klucz magazynowania** . Należy wybrać konto miejsca do magazynowania w tym samym regionie jako pamięci podręcznej, a konto **Magazynowania Premium** jest zalecane, ponieważ magazynowania premium ma wyższej przepustowości. W dowolnym momencie klucz miejsca do magazynowania dla Twojego konta utrzymywanie jest generowane, możesz ponownie wybrać inny klawisz z listy rozwijanej **Klucz magazynowania** .

Kliknij **przycisk OK** , aby zapisać konfigurację trwałe.

>[AZURE.IMPORTANT] Utrzymywanie danych redis jest dostępna tylko dla pamięci podręcznej Premium. Aby uzyskać więcej informacji zobacz [jak skonfigurować pozostawanie na Premium Azure Redis potrzeby pamięci podręcznej](cache-how-to-premium-persistence.md).

### <a name="importexport"></a>Importuj/Eksportuj

Import/Eksport jest operacji zarządzania danych Azure Redis w pamięci podręcznej, dzięki czemu można zaimportować dane do pamięci podręcznej Azure Redis lub eksportowania danych z pamięci podręcznej Azure Redis przez importowania i eksportowania migawki Redis pamięci podręcznej bazy danych (RDB) z pamięci podręcznej premium do obiektów blob strony przy użyciu konta z miejsca do magazynowania Azure. Dzięki temu można przeprowadzić migrację między różnych wystąpieniach Azure Redis w pamięci podręcznej i wypełniać w pamięci podręcznej danych przed użyciem.

Importowanie można przenosić Redis zgodne RDB plików z dowolnego Redis serwerem w chmurze lub środowisko, w tym Redis uruchomionych Linux, Windows lub dowolnego dostawcy chmurze, takich jak Amazon usług sieci Web i innych osób. Importowanie danych jest łatwym sposobem utworzenia pamięci podręcznej z danymi wstępnie wypełnione. Podczas importowania Azure Redis w pamięci podręcznej ładuje pliki RDB z magazynu Azure do pamięci, a następnie wstawia nazwy klawiszy w pamięci podręcznej.

Eksportowanie umożliwia eksportowanie danych przechowywanych w pamięci podręcznej Redis Azure do Redis zgodne pliki RDB. Ta funkcja umożliwia przenoszenie danych z jednego wystąpienia Azure Redis w pamięci podręcznej do drugiego lub na inny serwer Redis. W trakcie eksportu utworzony plik tymczasowy na maszyn wirtualnych obsługującego wystąpienie serwera Azure Redis w pamięci podręcznej, a plik jest przekazywany do konta wyznaczonego miejsca do magazynowania. Po zakończeniu operacji eksportowania ze stanem sukcesu lub niepowodzenia tymczasowy plik zostanie usunięty.

>[AZURE.IMPORTANT] Import/Eksport jest dostępna tylko dla Premium warstwa w pamięci podręcznej. Aby uzyskać więcej informacji i instrukcje zobacz [Importowanie i eksportowanie danych w pamięci podręcznej Azure Redis](cache-how-to-import-export-data.md).


## <a name="administration-settings"></a>Ustawienia administracyjne

Ustawienia w sekcji **Administracja** umożliwiają wykonywanie następujących zadań administracyjnych dotyczących pamięci podręcznej premium. 

![Administracja](./media/cache-configure/redis-cache-administration.png)

-   [Uruchom ponownie komputer](#reboot)
-   [Planowanie aktualizacji](#schedule-updates)

>[AZURE.IMPORTANT] Ustawienia w tej sekcji są dostępne tylko dla Premium warstwa w pamięci podręcznej.

### <a name="reboot"></a>Uruchom ponownie komputer

**Uruchom ponownie** karta umożliwia Uruchom ponownie jeden lub więcej węzłów pamięci podręcznej. Dzięki temu będzie można przetestować aplikację dla elastyczność w przypadku awarii.

![Uruchom ponownie komputer](./media/cache-configure/redis-cache-reboot.png)

Jeśli masz pamięci podręcznej premium z klaster włączone, możesz wybrać które odłamki pamięci podręcznej, aby ponownie uruchomić komputer.

![Uruchom ponownie komputer](./media/cache-configure/redis-cache-reboot-cluster.png)

Aby ponownie uruchomić jednej lub więcej węzłów pamięć podręczną, zaznacz odpowiednie węzły, a następnie kliknij przycisk **Uruchom ponownie**. Jeśli masz pamięci podręcznej premium z klaster włączone, wybierz pozycję shard(s) ponownie uruchomić komputer, a następnie kliknij przycisk **Uruchom ponownie**. Po kilku minutach zaznaczonych węzłów Uruchom ponownie komputer i można je do trybu online później kilka minut.

>[AZURE.IMPORTANT] Uruchom ponownie komputer jest dostępna tylko dla Premium warstwa w pamięci podręcznej. Aby uzyskać więcej informacji i instrukcje zobacz [Administrowanie Azure Redis w pamięci podręcznej - Uruchom ponownie](cache-administration.md#reboot).

### <a name="schedule-updates"></a>Planowanie aktualizacji

Karta **Harmonogram aktualizacji** pozwala określać okno konserwacji Redis serwera aktualizacje dla pamięci podręcznej. 

>[AZURE.IMPORTANT] Zauważ, że okno konserwacji dotyczy tylko Redis aktualizacji serwera i nie chcesz, aby wszystkie Azure aktualizacje lub aktualizacje systemu operacyjnego maszyny wirtualne, które obsługują pamięci podręcznej.

![Planowanie aktualizacji](./media/cache-configure/redis-schedule-updates.png)

Aby określić oknem konserwacji, zaznacz odpowiednie dni i określ godzinę rozpoczęcia okna konserwacji dla każdego dnia, a następnie kliknij przycisk **OK**. Należy zauważyć, że podczas okna konserwacji w czasie UTC. 

>[AZURE.IMPORTANT] Planowanie aktualizacji jest dostępna tylko dla Premium warstwa w pamięci podręcznej. Aby uzyskać więcej informacji i instrukcje zobacz [Administrowanie Azure Redis w pamięci podręcznej - Planowanie aktualizacji](cache-administration.md#schedule-updates).

## <a name="diagnostics-settings"></a>Ustawienia diagnostyki

Sekcja **diagnostyki** umożliwia konfigurowanie diagnostyki dla pamięci podręcznej Redis.

![Narzędzia diagnostyczne](./media/cache-configure/redis-cache-diagnostics.png)

Aby [skonfigurować konto miejsca do magazynowania](cache-how-to-monitor.md#enable-cache-diagnostics) służy do przechowywania diagnostyki pamięci podręcznej, kliknij pozycję **Diagnostyka** .

![Redis diagnostyki pamięci podręcznej](./media/cache-configure/redis-cache-diagnostics-settings.png)

Kliknij **Redis metryki** , aby [wyświetlić metryki](cache-how-to-monitor.md#how-to-view-metrics-and-customize-charts) pamięci podręcznej i **reguł alertów** do [ustawiania reguł alertów](cache-how-to-monitor.md#operations-and-alerts).

Aby uzyskać więcej informacji dotyczących diagnostyki Azure Redis w pamięci podręcznej zobacz [jak monitorowanie Azure Redis w pamięci podręcznej](cache-how-to-monitor.md).


## <a name="network-settings"></a>Ustawienia sieci

Ustawienia w sekcji **sieć** umożliwiają dostęp do i skonfigurować następujące ustawienia dla pamięci podręcznej.

![Sieci](./media/cache-configure/redis-cache-network.png)

>[AZURE.IMPORTANT] Ustawienia wirtualnej sieci są dostępne tylko dla pamięci podręcznej premium, które zostały skonfigurowane z obsługą VNET podczas tworzenia pamięci podręcznej. Informacje na temat tworzenia pamięci podręcznej premium z VNET pomocy technicznej i aktualizacji jego ustawienia, Dowiedz się, [jak skonfigurować wirtualnej sieci pomocy technicznej dotyczącej bufor Redis Azure Premium](cache-how-to-premium-vnet.md).

## <a name="resource-management-settings"></a>Ustawienia zarządzania zasobów

![Zarządzanie zasobami](./media/cache-configure/redis-cache-resource-management.png)

W sekcji **znaczniki** ułatwia organizowanie zasobów. Aby uzyskać więcej informacji zobacz [Używanie znaczników w celu organizowania Azure zasobów](../resource-group-using-tags.md).

Sekcja **blokuje** umożliwia blokowanie subskrypcji, grupa zasobów lub zasób uniemożliwić innym użytkownikom w organizacji przypadkowemu usunięciu lub modyfikowanie krytycznych zasobów. Aby uzyskać więcej informacji zobacz [Blokowanie zasoby przy użyciu Menedżera zasobów Azure](../resource-group-lock-resources.md).

W sekcji **Użytkownicy** zapewnia obsługę kontrola dostępu oparta na rolach (RBAC) w portalu Azure, ułatwiając organizacjom wymagań ich dostępu do zarządzania po prostu i precyzyjne. Aby uzyskać więcej informacji zobacz [Kontrola dostępu oparta na rolach w portalu Azure](../active-directory/role-based-access-control-configure.md).

Kliknij przycisk **Eksportuj szablon** do tworzenia i eksportowanie szablonu wdrożonym zasobów dla przyszłych wdrożeń. Aby uzyskać więcej informacji na temat pracy z szablonami zobacz [zasoby rozmieszczanie dzięki szablonom Azure Menedżera zasobów](../resource-group-template-deploy.md).

## <a name="default-redis-server-configuration"></a>Domyślne konfiguracji serwera Redis

Nowe wystąpienia Azure Redis w pamięci podręcznej skonfigurowano następujące wartości domyślne Redis konfiguracji.

>[AZURE.NOTE] Ustawienia w tej sekcji nie można zmienić przy użyciu `StackExchange.Redis.IServer.ConfigSet` metody. Jeśli ta metoda jest wywoływana z jednym z poleceń w tej sekcji, podobny do następującego wyjątku:  
>
>`StackExchange.Redis.RedisServerException: ERR unknown command 'CONFIG'`
>  
>Wszystkie wartości, które są konfigurowane, takich jak **max pamięci zasady**są konfigurowane za pośrednictwem Azure portalu lub wiersza polecenia narzędzia do zarządzania takich jak polecenie Azure lub programu PowerShell.

|Ustawienie|Wartość domyślna|Opis|
|---|---|---|
|bazy danych|16|Domyślny numer baz danych jest 16, ale można skonfigurować inną liczbę oparte na warstwie cennik. <sup>1</sup> DB 0 jest domyślną bazę danych, możesz wybrać inny na podstawę dla każdego połączenia za pomocą `connection.GetDatabase(dbid)` gdzie dbid jest liczbą z przedziału od `0` i `databases - 1`.|
|maxclients|W zależności od cennik poziom<sup>2</sup>|To jest maksymalna liczba połączonych klientów dozwolona w tym samym czasie. Po osiągnięciu limitu Redis zostanie zamknięte nowych połączeń, wysyłając komunikat o błędzie "Maksymalna liczba klientów z Tobą".|
|zasady maxmemory|lru lotnych|Zasady Maxmemory jest to ustawienie jak Redis wybierze, co należy usunąć po osiągnięciu maxmemory (rozmiar pamięci podręcznej oferujących podczas tworzenia pamięci podręcznej wybrany). Z pamięci podręcznej Azure Redis ustawieniem domyślnym jest lotnych lru, który usuwa klucze wygaśnięcia zestaw przy użyciu algorytmu LRU. To ustawienie można skonfigurować w portalu Azure. Aby uzyskać więcej informacji, zobacz [Maxmemory zasad i zastrzeżone maxmemory](#maxmemory-policy-and-maxmemory-reserved).|
|Przykłady maxmemory|3|LRU i minimalnego algorytmów TTL nie są dokładne algorytmów, ale w przybliżeniu algorytmów (w celu zapisania pamięci), dzięki czemu można zaznaczyć także wielkością próbki do sprawdzenia. Na przykład dla domyślnych Redis Sprawdź trzy klucze i wybierz ten, który został mniej ostatnio używane.|
|LUA terminu|5000|Maksymalny czas wykonywania skryptu Lua (w milisekundach). Po osiągnięciu maksymalnej czas wykonywania Redis można rejestrować należącym do skryptu nadal wykonanie po maksymalny dozwolony czas i rozpocznie się odpowiedzieć kwerendy z powodu błędu.|
|LUA zdarzenia limit|500|To jest maksymalny rozmiar kolejki zdarzeń skrypt.|
|Bufor limit, klienta danych wyjściowych w-buforu limit, normalclient danych wyjściowych w-pubsub|0 0 032mb 8mb 60|Limity buforu wyjściowego klienta może służyć do wymusić odłączenia klientów, którzy nie są odczytywania danych z serwera wystarczająco szybko jakiegoś powodu (typowe przyczyny to, że klient Pub-Sub nie mogą korzystać z wiadomości tak szybko, jak wydawcy można przygotowywania ich). Aby uzyskać więcej informacji zobacz [http://redis.io/topics/clients](http://redis.io/topics/clients).|

<a name="databases"></a>
<sup>1</sup> Limit dla `databases` jest inna dla każdej Azure Redis pamięci podręcznej ceny warstwy i można ustawić na tworzenie pamięci podręcznej. Jeśli nie `databases` ustawienie będzie miało podczas tworzenia pamięci podręcznej, wartość domyślna to 16.

-   Podstawowe oraz standardowy
    -   C0 pamięci podręcznej (250 MB) — maksymalnie 16 baz danych
    -   C1 pamięci podręcznej (1 GB) — maksymalnie 16 baz danych
    -   C2 pamięci podręcznej (2,5 GB) — maksymalnie 16 baz danych
    -   C3 pamięci podręcznej (6 GB) — maksymalnie 16 baz danych
    -   C4 pamięci podręcznej (13 GB) — maksymalnie 32 baz danych
    -   C5 pamięci podręcznej (26 GB) — maksymalnie 48 baz danych
    -   C6 pamięci podręcznej (53 GB) — maksymalnie 64 baz danych
-   Pamięć podręczną Premium
    -   P1 (6 GB – 60 GB) — maksymalnie 16 baz danych
    -   (13 GB - 130 GB) — P2 maksymalnie 32 baz danych
    -   P3 (26 GB - 260 GB) — maksymalnie 48 baz danych
    -   P4 (53 GB - 530 GB) — maksymalnie 64 baz danych
    -   Wszystkie bufory premium z klastrem Redis enabled - Redis klaster obsługuje tylko korzystanie z bazy danych 0 tak `databases` ograniczyć wszelkie pamięci podręcznej premium z klastrem Redis włączony jest faktycznie 1 i [Wybierz](http://redis.io/commands/select) polecenie nie jest dozwolona. Aby uzyskać więcej informacji, zobacz [Czy muszę wprowadzać zmiany w aplikacji pakietu klienta, aby użyć klaster?](#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)


>[AZURE.NOTE] `databases` Ustawienie może być skonfigurowany tylko podczas tworzenia pamięci podręcznej i tylko przy użyciu programu PowerShell, polecenie lub innymi zarządzania klientami. Na przykład konfigurowania `databases` podczas tworzenia pamięci podręcznej przy użyciu programu PowerShell, zobacz [Nowy AzureRmRedisCache](cache-howto-manage-redis-cache-powershell.md#databases).


<a name="maxclients"></a>
<sup>2</sup> `maxclients` różni się w każdej Azure Redis pamięci podręcznej ceny warstwy.

-   Podstawowe oraz standardowy
    -   C0 pamięci podręcznej (250 MB) — maksymalnie 256 połączenia
    -   C1 pamięci podręcznej (1 GB) — do 1000 połączeń
    -   C2 pamięci podręcznej (2,5 GB) — maksymalnie 2000 połączenia
    -   C3 pamięci podręcznej (6 GB) — maksymalnie 5000 połączeń
    -   C4 pamięci podręcznej (13 GB) — maksymalnie 10 000 połączeń
    -   C5 pamięci podręcznej (26 GB) — maksymalnie 15 000 połączeń
    -   C6 pamięci podręcznej (53 GB) — maksymalnie 20 000 połączeń
-   Pamięć podręczną Premium
    -   P1 (6 GB – 60 GB) — maksymalnie 7500 połączenia
    -   P2 (13 GB - 130 GB) — maksymalnie 15 000 połączeń
    -   P3 (26 GB - 260 GB) — do 30 000 połączeń
    -   P4 (53 GB - 530 GB) — maksymalnie 40 000 połączeń

## <a name="redis-commands-not-supported-in-azure-redis-cache"></a>Redis polecenia nie są obsługiwane w pamięci podręcznej Azure Redis

>[AZURE.IMPORTANT] Ponieważ konfiguracja i zarządzania wystąpień Azure Redis w pamięci podręcznej jest zarządzane przez firmę Microsoft następujące polecenia są wyłączone. Jeśli spróbujesz wywołania ich otrzymasz komunikat o błędzie podobny do `"(error) ERR unknown command"`.
>
>-  BGREWRITEAOF
>-  BGSAVE
>-  KONFIGURACJI
>-  DEBUGOWANIE
>-  MIGROWANIE
>-  ZAPISYWANIE
>-  ZAMKNIĘCIE
>-  SLAVEOF
>-  KLASTER - klaster zapisu polecenia są zablokowane, ale tylko do odczytu klaster poleceń jest dozwolone.

Aby uzyskać więcej informacji na temat polecenia Redis zobacz [http://redis.io/commands](http://redis.io/commands).

## <a name="redis-console"></a>Redis konsoli

Bezpieczne można wysyłać polecenia do swojego wystąpień Azure Redis w pamięci podręcznej, za pomocą **Konsoli Redis**, który jest dostępny w standardowym i buforowanie Premium.

>[AZURE.IMPORTANT] Konsola Redis nie działa z VNET klaster i bazy danych inne niż 0. 
>
>-  [VNET](cache-how-to-premium-vnet.md) — Jeśli pamięć podręczną jest częścią VNET, tylko klientów w VNET dostęp do pamięci podręcznej. Ponieważ konsoli Redis korzysta z klienta redis cli.exe hostowana w maszyny wirtualne, które nie są częścią usługi VNET, nie może połączyć się z pamięci podręcznej.
>-  [Klastrowanie](cache-how-to-premium-clustering.md) - Konsola Redis korzysta z klienta redis cli.exe, który nie obsługuje klaster w tej chwili. Narzędzie redis polecenie w [nietrwałe](http://redis.io/download) gałęzi repozytorium Redis u GitHub wykonuje podstawową obsługę podczas pracy z `-c` przełączanie. Aby uzyskać więcej informacji zobacz [gry z klastrem](http://redis.io/topics/cluster-tutorial#playing-with-the-cluster) na [http://redis.io](http://redis.io) w [Redis samouczek klaster](http://redis.io/topics/cluster-tutorial).
>-  Konsola Redis tworzy nowe połączenie bazy danych 0 każdym wysyłaniu polecenia. Nie można używać `SELECT` polecenie, aby wybrać inną bazę danych, ponieważ baza danych jest ustawiany na 0 z każdego polecenia. Aby uzyskać informacje na temat uruchamiania polecenia Redis, włącznie ze zmianą na inną bazę danych, zobacz [sposobu uruchamiania polecenia Redis?](cache-faq.md#how-can-i-run-redis-commands)

Aby uzyskać dostęp do konsoli Redis, kliknij pozycję **konsoli** z karta **Redis pamięci podręcznej** .

![Redis konsoli](./media/cache-configure/redis-console-menu.png)

Do wykonania polecenia przed wystąpienia pamięci podręcznej, wystarczy wpisać do konsoli odpowiedniego polecenia.

![Redis konsoli](./media/cache-configure/redis-console.png)

Aby uzyskać listę poleceń Redis, które są wyłączone Azure Redis w pamięci podręcznej Zobacz poprzedniej sekcji [Redis polecenia nie są obsługiwane w pamięci podręcznej Azure Redis](#redis-commands-not-supported-in-azure-redis-cache) . Aby uzyskać więcej informacji na temat polecenia Redis zobacz [http://redis.io/commands](http://redis.io/commands). 

## <a name="move-your-cache-to-a-new-subscription"></a>Przenoszenie pamięci podręcznej do nowej subskrypcji

Pamięć podręczną można przenieść do nowej subskrypcji, klikając pozycję **Przenieś**.

![Przenoszenie Redis pamięci podręcznej](./media/cache-configure/redis-cache-move.png)

Aby uzyskać informacje na temat przenoszenia zasobów z jednej grupy zasobów do innego, a następnie w jedną subskrypcję do innego zobacz [Przenoszenie zasobów do nowej grupy zasobów lub innej subskrypcji](../resource-group-move-resources.md).

## <a name="next-steps"></a>Następne kroki
-   Aby uzyskać więcej informacji na temat pracy z poleceniami Redis, zobacz [sposobu uruchamiania polecenia Redis?](cache-faq.md#how-can-i-run-redis-commands).
