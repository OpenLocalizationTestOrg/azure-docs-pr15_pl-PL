## <a name="load-balancer-differences"></a>Różnice równoważenia obciążenia

Istnieją różne opcje do rozpowszechniania ruchu sieciowego przy użyciu programu Microsoft Azure. Te opcje działają inaczej od siebie, o różnych funkcji Ustawianie i obsługa techniczna różnych scenariuszach. Każdy mogą być używane oddzielnie, lub połączenie tych elementów.

- **Usługi równoważenia obciążenia Azure** działa w warstwie transportu (4 warstwy w stos odwołanie OSI sieci). Umożliwia poziom sieci dystrybucji ruchu w jego wystąpieniach aplikacji działa w tym samym centrum danych Azure.

- **Brama aplikacji** działa w warstwie aplikacji (7 warstwy w stos odwołanie OSI sieci). Działa jako usługi reverse proxy, zakończenie połączenia klienta i przesyłanie dalej wezwania do wewnętrznej punktów końcowych.

- **Menedżer ruchu** działa na poziomie DNS.  Aby skierować ruch użytkowników końcowych do punktów końcowych globalnie rozłożone używa odpowiedzi DNS. Następnie łączą się klienci do tych punktów końcowych bezpośrednio.

W poniższej tabeli wymieniono funkcje oferowane przez każdej usługi:

| Usługa | Usługi równoważenia obciążenia Azure | Brama aplikacji | Menedżer ruchu |
|---|---|---|---|
|Technologia| Poziom transportu (warstwy 4) | Poziom aplikacji (warstwy 7) | Poziom DNS |
| Obsługiwane protokoły aplikacji | Dowolny | HTTP i HTTPS |  Dowolny (punktu końcowego HTTP jest wymagany do monitorowania punktu końcowego) |
| Punkty końcowe | Azure wystąpienia roli maszyny wirtualne i usług w chmurze | Azure wewnętrznego adresu IP ani publicznego Internetu adres IP | Azure maszyny wirtualne, usług w chmurze, aplikacje sieci Web Azure i zewnętrznych punktów końcowych |
| Obsługa Vnet | Może być używany do obu aplikacji (Vnet) internetowych, jak wewnętrznych i przeciwległych | Może być używany do obu aplikacji (Vnet) Internet, jak wewnętrznych i przeciwległych |    Obsługuje tylko aplikacje z Internetu |
Monitorowanie punktu końcowego | Obsługiwane przez sondy | Obsługiwane przez sondy | Obsługiwane za pośrednictwem protokołu HTTP/HTTPS GET | 

Azure równoważenia obciążenia i bramy aplikacji trasy sieciowe ruchu do punktów końcowych, ale mają one zastosowania różnych scenariuszy, jaki ruch do obsługi. Poniższa tabela ułatwia rozróżnianie między urządzenia do równoważenia obciążenia dwóch:

| Typ | Usługi równoważenia obciążenia Azure | Brama aplikacji |
|---|---|---|
| Protokoły | UDP/TCP | HTTP / HTTPS |
| Zastrzeżenie IP | Obsługiwane | Brak obsługi | 
| Tryb równoważenia obciążenia | 5 tuple(source IP, source port, destination IP, destination port, protocol type) | Okrężnego<br>Routing na podstawie adresu URL | 
| Tryb równoważenia obciążenia (źródłowy adres IP / lepkich sesji) |  2-krotki (adres IP źródła i docelowy adres IP), 3-krotki (adres IP źródła, docelowy adres IP i port). Można skalować w górę lub w dół oparte na liczbę maszyn wirtualnych | Koligacja systemem plików cookie<br>Routing na podstawie adresu URL |
| Sondy kondycji | Wartość domyślna: sondy interwał - 15 sekundach. Sporządzone poza obrót: 2 ciągły błędy. Obsługa sondy zdefiniowane przez użytkownika | Interwał bezczynne sondy 30 sekund. Pobierać po 5 ruch live następujących po sobie błędów lub awaria jednej sondy w trybie bezczynne. Obsługa sondy zdefiniowane przez użytkownika | 
| Odciążanie protokołu SSL | Brak obsługi | Obsługiwane | 
  