<properties
  pageTitle="Ładowanie równoważenia sondy niestandardowych i monitorowanie stanu kondycji | Microsoft Azure"
  description="Dowiedz się, jak za pomocą niestandardowych sondy do równoważenia obciążenia Azure monitorować wystąpienia za usługi równoważenia obciążenia"
  services="load-balancer"
  documentationCenter="na"
  authors="sdwheeler"
  manager="carmonm"
  editor=""
  tags="azure-resource-manager"
/>
<tags
  ms.service="load-balancer"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="infrastructure-services"
  ms.date="10/24/2016"
  ms.author="sewhee" />

# <a name="understand-load-balancer-probes"></a>Opis sondy równoważenia obciążenia

Azure równoważenia obciążenia oferuje możliwości monitorowanie kondycji wystąpienia serwera przy użyciu sondy. Gdy sondy nie odpowiada, równoważenia obciążenia zatrzymuje wysyłanie nowych połączeń z wystąpieniem nieprawidłowe. Nie ma wpływu na istniejące połączenia, a nowe połączenia są wysyłane do prawidłowy wystąpienia.

Role usługi cloud (pracownik ról i sieci web) za pomocą agenta gościa do monitorowania sondy. Musi być skonfigurowany TCP i HTTP sondy niestandardowe, gdy używasz maszyn wirtualnych za usługi równoważenia obciążenia.

## <a name="understand-probe-count-and-timeout"></a>Opis przekroczenia limitu czasu i liczba sondy

Zachowanie sondy zależy od:

- Liczba pomyślnie sondy, umożliwiające wystąpienie mają być oznaczane etykietami w górę.
- Liczba sondy nie powiodło się, które powodują wystąpienie mają być oznaczane etykietami w dół.

Wartość limitu czasu i częstość w SuccessFailCount określają, czy wystąpienie potwierdzić nie jest uruchomiona lub nie działa. W portalu usługi Azure limit czasu jest ustawiona wartość dwa razy częstotliwości.

Konfiguracja sondy wszystkie wystąpienia równoważenia obciążenia dla punktu końcowego (czyli równoważenia obciążenia zestaw) musi być taka sama. Oznacza to, że nie ma konfiguracji różnych sondy dla każdego wystąpienia roli lub maszyn wirtualnych w tej samej usługi obsługiwanych kombinacji określonego punktu końcowego. Na przykład każde wystąpienie musi mieć identyczne portów lokalnych i limitów czasu.

>[AZURE.IMPORTANT] Sonda równoważenia obciążenia korzysta z adresem IP 168.63.129.16. Ten publiczny adres IP ułatwia komunikację do zasobów wewnętrznych platformy Przesuń i właścicielem — IP scenariusz Azure wirtualnej sieci. Wirtualna publiczny adres IP 168.63.129.16 jest używana we wszystkich regionach i nie ulegnie zmianie. Firma Microsoft zaleca umożliwić tego adresu IP w wszelkie zasady zapory lokalnej. Nie należy rozważyć ryzyko związane z zabezpieczeniami, ponieważ tylko wewnętrznych platformy Azure można źródła wiadomości od tego adresu. Jeśli to nie zrobisz, będzie można nieoczekiwane działanie w różnych scenariuszy, takich jak konfigurowanie tego samego zakresu adresów IP 168.63.129.16 i konieczności zduplikowanych adresów IP.

## <a name="learn-about-the-types-of-probes"></a>Informacje na temat typów sondy

### <a name="guest-agent-probe"></a>Sonda agenta gościa

Sonda jest dostępna dla usług w chmurze Azure tylko. Usługi równoważenia obciążenia wykorzystuje agenta gościa wewnątrz maszyny wirtualnej, a następnie wykrywa i odpowiada odpowiedź HTTP 200 OK tylko wtedy, gdy wystąpienie jest w stanie gotowości (oznacza to, że nie w inny stan takich jak zajęty, odtwarzanie lub zatrzymywanie).

Aby uzyskać więcej informacji zobacz [Konfigurowanie pliku definicji usługi (csdef) dla sondy kondycji](https://msdn.microsoft.com/library/azure/ee758710.aspx) lub [rozpocząć tworzenie równoważenia obciążenia z Internetu dla usług w chmurze](load-balancer-get-started-internet-classic-cloud.md#check-load-balancer-health-status-for-cloud-services).

### <a name="what-makes-a-guest-agent-probe-mark-an-instance-as-unhealthy"></a>Co sprawia, że sondy agenta gościa, oznaczanie wystąpienia jako nieprawidłowe?

Jeśli agent gościa nie odpowiada OK 200 HTTP, równoważenia obciążenia oznacza wystąpienie odpowiada i zatrzymuje wysyłanie ruch do tego wystąpienia. Usługi równoważenia obciążenia w dalszym ciągu ping wystąpienie. Jeśli agent Gość odpowiada 200 HTTP, równoważenia obciążenia wysyła ruch do tego wystąpienia ponownie.

Użycie ról w sieci web, kod witryny sieci Web zazwyczaj jest uruchamiany w w3wp.exe, który nie jest monitorowane przez Azure agenta tkaninie lub Gość. Oznacza to, że nie jest informowany błędów w w3wp.exe (na przykład odpowiedzi HTTP 500) do agenta gościa i usługi równoważenia obciążenia nie zajmie to wystąpienie poza obrotu.

### <a name="http-custom-probe"></a>Sonda niestandardowe HTTP

Niestandardowe sondy równoważenia obciążenia HTTP zastępuje domyślne sondy agenta gościa, co oznacza, że można utworzyć własne niestandardowe logikę do określenia kondycji wystąpienia roli. Usługi równoważenia obciążenia sondy punkt końcowy co 15 sekund domyślnie. Wystąpienie jest uważany w obrót równoważenia obciążenia, gdy odpowiada z 200 HTTP w limicie czasu (w sekundach 31 domyślnie).

Może to być przydatne, jeśli chcesz zaimplementować logiki usunąć wystąpienia z obrotu równoważenia obciążenia. Na przykład można zdecydować, Usuń wystąpienie, jeśli jest powyżej 90% Procesora i zwraca stan-200. Jeśli masz role w sieci web, które używają w3wp.exe, oznacza to również zostanie wyświetlony automatyczne monitorowania witryny sieci Web, ponieważ błędy w kodzie witryny sieci Web zwróci stan-200 sondy równoważenia obciążenia.

>[AZURE.NOTE] Sonda niestandardowe HTTP obsługuje ścieżki względne i tylko protokołu HTTP. Protokół HTTPS nie jest obsługiwane.

### <a name="what-makes-an-http-custom-probe-mark-an-instance-as-unhealthy"></a>Co sprawia, że sondy niestandardowe HTTP oznaczanie wystąpienia jako nieprawidłowe?

- Aplikacja HTTP zwraca kod odpowiedzi HTTP niż 200 (na przykład 403, 404 lub 500). To jest dodatnia potwierdzenia, którego wystąpienie aplikacji należy wylogowywanie się z usługi natychmiast.
- Serwer HTTP nie odpowiada limit czasu, po. W zależności od jest ustawiona wartość limitu czasu, może to oznaczać tego wielu sondy żądania Przejdź bez odpowiedzi przed sondy jest oznaczony jako nie działa (oznacza to, że przed SuccessFailCount sondy są wysyłane).
- Serwer zamyka połączenie za pośrednictwem Resetowanie TCP.

### <a name="tcp-custom-probe"></a>Port TCP sondy niestandardowe

Port TCP sondy nawiązywania połączeń, wykonując trójstopniowego z portem zdefiniowane.

### <a name="what-makes-a-tcp-custom-probe-mark-an-instance-as-unhealthy"></a>Co sprawia, że TCP sondy niestandardowe, oznaczanie wystąpienia jako nieprawidłowe?

- Port TCP serwer nie odpowiada na wszystkich limit czasu, po. Gdy sondy jest oznaczony jako nie działa w zależności od liczba żądań sondy nie powiodło się, które zostały skonfigurowane do wysłania nieodebrane przed oznaczeniem sondy jak nie działa.
- Sonda otrzymuje TCP Resetowanie z wystąpienia roli.

Aby uzyskać więcej informacji o konfigurowaniu sondy kondycji HTTP lub sondy TCP zobacz [rozpocząć tworzenie równoważenia obciążenia internetową dostępną w Menedżerze zasobów przy użyciu programu PowerShell](load-balancer-get-started-internet-arm-ps.md#create-lb-rules-nat-rules-a-probe-and-a-load-balancer).

## <a name="add-healthy-instances-back-into-load-balancer-rotation"></a>Dodawanie prawidłowy wystąpienia do obrotu równoważenia obciążenia

Port TCP i HTTP sondy są traktowane jako prawidłowy i oznaczanie wystąpienia roli za prawidłowy, gdy:

- Usługi równoważenia obciążenia otrzymuje dodatnia sondy uruchamiania maszyn wirtualnych po raz pierwszy.
- Liczba SuccessFailCount, (opisane wcześniej) określa wartość pomyślnego sondy, które są wymagane do oznaczania wystąpienie roli jako prawidłowy. Jeśli została usunięta w przypadku wystąpienia roli, liczba sondy pomyślnym, kolejnych musi równa lub przekracza wartości SuccessFailCount, aby oznaczyć wystąpienie roli jako uruchomiony.

>[AZURE.NOTE] Jeśli jest zmienne kondycji wystąpienia roli, równoważenia obciążenia oczekuje już przed wprowadzeniem wystąpienie roli Wstecz w stanie prawidłowy. Można to zrobić za pomocą zasad ochrony użytkowników i infrastruktury.

## <a name="use-log-analytics-for-load-balancer"></a>Używanie analizy dziennika równoważenia obciążenia

Za pomocą [analizy dziennika równoważenia obciążenia](load-balancer-monitor-log.md) Aby sprawdzić stan kondycji sondy i liczba sondy. Rejestrowanie może służyć za pomocą usługi Power BI lub Azure operacyjne wniosków o podanie statystyk dotyczących stanu kondycji usługi równoważenia obciążenia.
