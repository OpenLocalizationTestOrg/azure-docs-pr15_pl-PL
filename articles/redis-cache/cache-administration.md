<properties 
    pageTitle="Sposobu administrowania Azure Redis w pamięci podręcznej | Microsoft Azure"
    description="Dowiedz się, jak wykonywanie zadań administracyjnych, takich jak aktualizacje Uruchom ponownie komputer i harmonogram Azure Redis w pamięci podręcznej"
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
    ms.date="09/27/2016"
    ms.author="sdanie" />

# <a name="how-to-administer-azure-redis-cache"></a>Jak administrować Azure Redis w pamięci podręcznej

W tym temacie opisano sposób wykonywania zadań administracyjnych, takich jak ponowne uruchamianie i Planowanie aktualizacji dla wystąpień usługi Azure Redis w pamięci podręcznej.

>[AZURE.IMPORTANT] Ustawienia i funkcjami opisanymi w tym artykule są dostępne tylko dla Premium warstwa w pamięci podręcznej.


## <a name="administration-settings"></a>Ustawienia administracyjne

Ustawienia pamięci podręcznej Azure Redis **Administracja** umożliwiają wykonywanie następujących zadań administracyjnych dotyczących pamięci podręcznej premium. Aby uzyskać dostęp do ustawień administracyjnych, kliknij pozycję **Ustawienia** lub **wszystkie ustawienia** z karta Redis pamięci podręcznej i przewiń do sekcji **Administracja** karta **Ustawienia** .

![Administracja](./media/cache-administration/redis-cache-administration.png)

-   [Uruchom ponownie komputer](#reboot)
-   [Planowanie aktualizacji](#schedule-updates)

## <a name="reboot"></a>Uruchom ponownie komputer

**Uruchom ponownie** karta umożliwia Uruchom ponownie jeden lub więcej węzłów pamięci podręcznej. Dzięki temu będzie można przetestować aplikację dla elastyczność w przypadku awarii.

![Uruchom ponownie komputer](./media/cache-administration/redis-cache-reboot.png)

Jeśli masz pamięci podręcznej premium z klaster włączone, możesz wybrać które odłamki pamięci podręcznej, aby ponownie uruchomić komputer.

![Uruchom ponownie komputer](./media/cache-administration/redis-cache-reboot-cluster.png)

Aby ponownie uruchomić jeden lub więcej węzłów pamięć podręczną, zaznacz odpowiednie węzły, a następnie kliknij przycisk **Uruchom ponownie**. Jeśli masz pamięci podręcznej premium z klaster włączone, wybierz pozycję shard(s) ponownie uruchomić komputer, a następnie kliknij przycisk **Uruchom ponownie**. Po kilku minutach zaznaczonych węzłów Uruchom ponownie komputer i można je do trybu online później kilka minut.

Wpływ na aplikacje klienckie zależnie od węzłów, który ponownym.

-   **Wzorzec** — po uruchomieniu węzeł wzorca, Azure Redis w pamięci podręcznej przełącza do węzła replice i umożliwia podnoszenie poziomu do wzorca. W tym awaryjnym przeniesieniu może być krótki odstęp, w którym połączenia może zakończyć się niepowodzeniem w pamięci podręcznej.
-   **Podrzędna** - po ponownym uruchomieniu węzeł podrzędny jest zazwyczaj wpływu klientom pamięci podręcznej.
-   **Oba wzorca i podrzędna** - oba węzły pamięci podręcznej są ponownie uruchomić, wszystkie dane zostaną utracone w pamięci podręcznej i połączenia z pamięci podręcznej błędów aż główny węzeł zawiera powrocie do trybu online. Jeśli skonfigurowano [utrzymywanie danych](cache-how-to-premium-persistence.md), zostaną przywrócone najnowszą kopię zapasową, gdy pamięci podręcznej przechodzi do trybu online. Należy zauważyć, że wszelkie zapisy pamięci podręcznej, które wystąpiły po najnowszą kopię zapasową zostają utracone.
-   **Węzłów premium pamięci podręcznej klaster włączone** - po ponownym uruchomieniu węzłów pamięci podręcznej premium z klaster włączone, zachowanie jest taka sama, jak po ponownym uruchomieniu węzłów pamięci podręcznej grupowany.


>[AZURE.IMPORTANT] Uruchom ponownie komputer jest dostępna tylko dla Premium warstwa w pamięci podręcznej.

## <a name="reboot-faq"></a>Uruchom ponownie — często zadawane pytania

-   [Który węzeł powinien I Uruchom ponownie do testowania mojej aplikacji?](#which-node-should-i-reboot-to-test-my-application)
-   [Czy mogę Uruchom ponownie pamięci podręcznej, aby wyczyścić połączeń klientów?](#can-i-reboot-the-cache-to-clear-client-connections)
-   [Spowoduje utratę danych z moim pamięci podręcznej Jeśli zrobić, uruchom ponownie komputer?](#will-i-lose-data-from-my-cache-if-i-do-a-reboot)
-   [Można uruchamiać I ponownie moje pamięci podręcznej przy użyciu programu PowerShell, polecenie lub inne narzędzia do zarządzania?](#can-i-reboot-my-cache-using-powershell-cli-or-other-management-tools)
-   [Jakie ceny poziomów można użyć funkcji Uruchom ponownie komputer?](#what-pricing-tiers-can-use-the-reboot-functionality)


### <a name="which-node-should-i-reboot-to-test-my-application"></a>Który węzeł powinien I Uruchom ponownie do testowania mojej aplikacji?

Aby przetestować elastyczność aplikacji przed awariami podstawowego węzła pamięć podręczną, uruchom ponownie węzeł **wzorca** . Aby przetestować elastyczność aplikacji przed awariami pomocniczej węzła, uruchom ponownie węzeł **podrzędny** . Aby przetestować elastyczność aplikacji przed awariami sumy pamięci podręcznej, uruchom ponownie **oba** węzły.

### <a name="can-i-reboot-the-cache-to-clear-client-connections"></a>Czy mogę Uruchom ponownie pamięci podręcznej, aby wyczyścić połączeń klientów?

Tak, jeśli ponownym pamięci podręcznej są usuwane wszystkie połączenia klienta. Można to przydatne w przypadku, gdy wszystkie połączenia klienta są używane, na przykład ze względu na błąd logiczny lub błąd w kliencie. Każdej warstwie cennik jest innego [klienta limity połączeń](cache-configure.md#default-redis-server-configuration) dla różnych rozmiarach, a po osiągnięciu tych limitów nie większej liczby połączeń klienta są akceptowane. Ponowne uruchamianie pamięci podręcznej umożliwia czyszczenie wszystkich połączeń klientów.

>[AZURE.IMPORTANT] Połączenia klienta są używane w górę ze względu na błąd logiczny lub błąd w kodzie klienta, Zauważ, że StackExchange.Redis zostanie automatycznie ponownie nawiązać połączenie po powrocie do trybu online węzeł Redis. Jeśli źródłowych problem nie został rozwiązany, połączeń klienta będzie można używać w górę.

### <a name="will-i-lose-data-from-my-cache-if-i-do-a-reboot"></a>Spowoduje utratę danych z moim pamięci podręcznej Jeśli zrobić, uruchom ponownie komputer?

Jeśli ponownym węzły **głównego** i **podrzędna** wszystkie dane w pamięci podręcznej (lub w tym shard, jeśli korzystasz z pamięci podręcznej premium z klaster włączone) zostaną utracone. Jeśli masz skonfigurowane [utrzymywanie danych](cache-how-to-premium-persistence.md), gdy pamięci podręcznej przechodzi do trybu online zostaną przywrócone najnowszą kopię zapasową. Należy zauważyć, że wszelkie zapisy pamięci podręcznej, które wystąpiły po utworzenia kopii zapasowej zostają utracone.

Jeśli ponownym tylko jeden z węzłów dane nie są tracone zwykle, ale nadal może być. Na przykład jeśli uruchomieniu węzeł wzorca i zapisu pamięci podręcznej jest w toku, dane z pamięci podręcznej zapisu zostaną utracone. Inny scenariusz utratą danych będzie, jeśli ponownym jeden węzeł i innych węzeł stanie się przerywane ze względu na błąd w tym samym czasie. Aby uzyskać więcej informacji na temat możliwych przyczyn utraty danych, zobacz [co się stało z moimi danymi w Redis?](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md).

### <a name="can-i-reboot-my-cache-using-powershell-cli-or-other-management-tools"></a>Można uruchamiać I ponownie moje pamięci podręcznej przy użyciu programu PowerShell, polecenie lub inne narzędzia do zarządzania?

Tak, dla środowiska PowerShell instrukcje zobacz [ponowne uruchomienie Redis pamięci podręcznej](cache-howto-manage-redis-cache-powershell.md#to-reboot-a-redis-cache).

### <a name="what-pricing-tiers-can-use-the-reboot-functionality"></a>Jakie ceny poziomów można użyć funkcji Uruchom ponownie komputer?

Uruchom ponownie komputer jest dostępny tylko w premium ceny warstwy.

## <a name="schedule-updates"></a>Planowanie aktualizacji

Karta **Harmonogram aktualizacji** pozwala określać okno konserwacji dla pamięci podręcznej. Po określeniu okna konserwacji aktualizacje serwera Redis zostały wprowadzone w tym oknie. Zauważ, że okno konserwacji dotyczy tylko Redis aktualizacji serwera i nie chcesz, aby wszystkie Azure aktualizacje lub aktualizacje systemu operacyjnego maszyny wirtualne, które obsługują pamięci podręcznej.

![Planowanie aktualizacji](./media/cache-administration/redis-schedule-updates.png)

Aby określić oknem konserwacji, zaznacz odpowiednie dni i określ godzinę rozpoczęcia okna konserwacji dla każdego dnia, a następnie kliknij przycisk **OK**. Należy zauważyć, że podczas okna konserwacji w czasie UTC. 

>[AZURE.NOTE] Okno domyślny konserwacji aktualizacji jest 5 godzin. Ta wartość jest można skonfigurować z poziomu portalu Azure, ale można ją skonfigurować przy użyciu programu PowerShell `MaintenanceWindow` parametru polecenia cmdlet [New-AzureRmRedisCacheScheduleEntry](https://msdn.microsoft.com/library/azure/mt763833.aspx) . Aby uzyskać więcej informacji, zobacz [zaplanowanych aktualizacji przy użyciu programu PowerShell, polecenie lub inne narzędzia do zarządzania można zarządzać?](#can-i-managed-scheduled-updates-using-powershell-cli-or-other-management-tools)

## <a name="schedule-updates-faq"></a>Planowanie aktualizacje: często zadawane pytania

-   [Gdy aktualizacje występować, jeśli nie jest używany z funkcji Aktualizacje harmonogram?](#when-do-updates-occur-if-i-dont-use-the-schedule-updates-feature)
-   [Jakiego rodzaju aktualizacje zostały wprowadzone w oknie zaplanowanych?](#what-type-of-updates-are-made-during-the-scheduled-maintenance-window)
-   [Czy mogę zarządzanych zaplanowanych aktualizacji przy użyciu programu PowerShell, polecenie lub inne narzędzia do zarządzania](#can-i-managed-scheduled-updates-using-powershell-cli-or-other-management-tools)
-   [Jakie ceny poziomów można użyć funkcji Aktualizacje harmonogramu?](#what-pricing-tiers-can-use-the-schedule-updates-functionality)

### <a name="when-do-updates-occur-if-i-dont-use-the-schedule-updates-feature"></a>Gdy aktualizacje występować, jeśli nie jest używany z funkcji Aktualizacje harmonogram?

Jeśli nie określisz oknem konserwacji, aktualizacje mogą zostać wprowadzone w dowolnym momencie.

### <a name="what-type-of-updates-are-made-during-the-scheduled-maintenance-window"></a>Jakiego rodzaju aktualizacje zostały wprowadzone w oknie zaplanowanych?

Tylko Redis server, które aktualizacje zostały wprowadzone w oknie zaplanowanych. Okno konserwacji nie ma zastosowania do Azure aktualizacje lub aktualizacje do maszyn wirtualnych systemu operacyjnego.

### <a name="can-i-managed-scheduled-updates-using-powershell-cli-or-other-management-tools"></a>Czy mogę zarządzanych zaplanowanych aktualizacji przy użyciu programu PowerShell, polecenie lub inne narzędzia do zarządzania

Tak, można zarządzać aktualizacje według harmonogramu przy użyciu następujące polecenia cmdlet programu PowerShell.

-   [Get-AzureRmRedisCachePatchSchedule](https://msdn.microsoft.com/library/azure/mt763835.aspx)
-   [Nowy AzureRmRedisCachePatchSchedule](https://msdn.microsoft.com/library/azure/mt763834.aspx)
-   [Nowy AzureRmRedisCacheScheduleEntry](https://msdn.microsoft.com/library/azure/mt763833.aspx)
-   [Usuń AzureRmRedisCachePatchSchedule](https://msdn.microsoft.com/library/azure/mt763837.aspx)

### <a name="what-pricing-tiers-can-use-the-schedule-updates-functionality"></a>Jakie ceny poziomów można użyć funkcji Aktualizacje harmonogramu?

Planowanie aktualizacji jest dostępna tylko w premium ceny warstwy.

## <a name="next-steps"></a>Następne kroki

-   Zapoznaj się z funkcjami więcej [Warstwa premium Azure Redis w pamięci podręcznej](cache-premium-tier-intro.md) .





