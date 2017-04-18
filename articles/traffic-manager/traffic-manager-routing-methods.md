<properties
    pageTitle="Menedżer ruch — ruch metody routingu | Microsoft Azure"
    description="W tym artykule pomoże zrozumieć ruch różnych metod routingu używany przez Menedżera ruchu"
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="traffic-manager-traffic-routing-methods"></a>Metody routingu ruchu Menedżera ruchu

Azure Menedżer ruchu obsługuje trzy metody routingu ruchu, aby dowiedzieć się, jak skierować ruch sieciowy do różnych punktach usługi. Menedżer ruchu dotyczy metody routingu ruchu każdego kwerendę DNS. Metoda routingu ruchu określa, który punkt końcowy zwrócone w odpowiedzi DNS.

Obsługa Azure Menedżera zasobów dla Menedżera ruch jest używana inna terminologia niż model klasyczny wdrożenia. W poniższej tabeli przedstawiono różnice między Menedżera zasobów i klasycznym terminów:

| Menedżer zasobów terminów | Klasyczny terminów |
|-----------------------|--------------|
| Metoda routingu ruchu | Metoda równoważenia obciążenia |
| Metoda Priority (priorytet) | Metody pracy awaryjnej |
| Metoda ważona | Metoda karuzeli |
| Metoda wydajności | Metoda wydajności |

Na podstawie opinii klientów, możemy zmienić terminologia, aby zwiększyć jasność i skrócić nieporozumień typowych. Nie różni się w funkcji.

Dostępne są trzy metody routingu ruchu w Menedżerze ruchu:

- **Priority (priorytet):** Wybierz pozycję "Priorytet", jeśli chcesz używać punkt końcowy podstawowej usługi dla całego ruchu i podaj wykonywania kopii zapasowych w przypadku podstawowego lub kopii zapasowej punkty końcowe są niedostępne.
- **Ważona:** Wybierz pozycję "ważona" umożliwia rozpowszechnianie ruchu w zestawie punkty końcowe albo równomiernie lub według wagi, który możesz zdefiniować.
- **Wydajności:** Wybierz pozycję "Wydajności", gdy punkty końcowe w różnych miejscach i chcesz użytkownikom końcowym za pomocą punkt końcowy "najbliższy" w odniesieniu do najmniejszej czasu oczekiwania w sieci.

Wszystkie profile Menedżera ruch obejmować monitorowanie kondycji punktu końcowego i pracy awaryjnej automatyczne punktu końcowego. Aby uzyskać więcej informacji zobacz [Menedżer ruchu monitorowania punktu końcowego](traffic-manager-monitoring.md). Jednego profilu Menedżer ruchu można użyć tylko jedną metodę routingu ruchu. Można wybrać metodę routingu inny ruch dla swojego profilu, w dowolnym momencie. Zmiany zostaną zastosowane w ciągu minuty, a powstaje brak przestojów. Metody routingu ruchu można łączyć przy użyciu zagnieżdżonych profilów Menedżer ruchu. Zagnieżdżanie umożliwia zaawansowane i elastyczne routingu ruchu konfiguracji, które potrzeb większych, złożonych aplikacji. Aby uzyskać więcej informacji zobacz [zagnieżdżonych profile Menedżer ruchu](traffic-manager-nested-profiles.md).

## <a name="priority-traffic-routing-method"></a>Metoda routingu ruchu Priority (priorytet)

Często organizacji chce udostępnić niezawodności jego usług wdrażając jedną lub więcej kopii zapasowej usług, w przypadku awarii usługi podstawowego. Metoda routingu ruchu "Priorytet" umożliwia Azure klientom łatwo wdrożyć tego wzorca pracy awaryjnej.

![Azure Menedżer ruchu "Priorytet" metody routingu ruchu][1]

Profil Menedżer ruchu zawiera listy priorytetów punkty końcowe usługi. Domyślnie Menedżer ruchu wysyła cały ruch do podstawowego punktu końcowego (najwyższy priorytet). Jeśli punkt końcowy podstawowego nie jest dostępna, Menedżer ruchu kieruje ruch do drugiego punktu końcowego. Jeśli głównego i pomocniczego końcowych nie są dostępne, ruch jest wysyłana do innych i tak dalej. Dostępność punktu końcowego jest oparty na skonfigurowanego stanu (włączony lub wyłączony) oraz monitorowanie trwających punktu końcowego.

### <a name="configuring-endpoints"></a>Konfigurowanie punktów końcowych

Przy użyciu Menedżera zasobów Azure skonfigurować priorytet punktu końcowego jawnie przy użyciu właściwości "priorytet" dla każdego punktu końcowego. Ta właściwość jest wartością z zakresu od 1 do 1000. Niższe wartości reprezentują wyższy priorytet. Punkty końcowe nie mogą współużytkować wartości priorytetów. Ustawienie właściwości jest opcjonalna. Po pominięty, jest używany domyślny priorytet na podstawie kolejności punktu końcowego.

Interfejs klasyczny priorytet punktu końcowego skonfigurowano niejawnie. Priorytet zależy od kolejności, w której punkty końcowe są wyświetlane w definicji profilu.

## <a name="weighted-traffic-routing-method"></a>Metodę ważoną routingu ruchu

"Ważona" metody routingu ruchu umożliwia równomierne rozłożenie ruch lub aby użyć wstępnie zdefiniowanych wagi.

![Azure Menedżer ruchu "" routingu ruchu metodę ważoną][2]

W przypadku metody ważonej routingu ruchu waga przypisać każdy punkt końcowy w konfiguracji profilu Menedżer ruchu. Waga jest liczbą całkowitą od 1 do 1000. Ten parametr jest opcjonalna. Jeśli pominięty, menedżerów ruch używa domyślna grubość "1".

Dla każdej odebranej kwerendy DNS Menedżer ruchu losowo wybiera punktu końcowego dostępne. Prawdopodobieństwo Wybieranie punktu końcowego zależy od wagi przypisane do wszystkich dostępnych punktów końcowych. Przy użyciu tej samej wagi wszystkie wyniki punkty końcowe w dystrybucji nawet ruchu. Za pomocą grubości wyżej lub niżej na określonych punktów końcowych powoduje, że te punkty końcowe mają zostać zwrócone więcej lub mniej często w odpowiedzi DNS.

Metodę ważoną udostępnia kilka scenariuszy przydatne:

- Uaktualnianie aplikacji stopniowe: przydzielanie procent ruchu do kierowania do nowego punktu końcowego i stopniowo zwiększać ruchu z upływem czasu na 100%.
- Migracja aplikacji Azure: Tworzenie profilu przy użyciu zarówno Azure i zewnętrznych punktów końcowych. Dostosowywanie grubości punktów końcowych do wolisz nowe punkty końcowe.
- Którym następuje chmury dodatkowych: szybko rozwiń wdrożenia lokalnego w chmurze, umieszczając go za profilu Menedżer ruchu. Gdy potrzebujesz dodatkowych możliwości w chmurze, możesz dodać lub włączyć więcej punkty końcowe i określić, jaka część ruch przechodzi do każdego punktu końcowego.

Nowy portal Azure obsługuje konfigurację routingu ruchu ważoną. Nie można skonfigurować grubości w portalu klasyczny. Można również skonfigurować przy użyciu Menedżera zasobów i klasycznej wersji programu Azure PowerShell, polecenie oraz interfejsy API pozostałych grubości.

Jest ważne dowiedzieć się, że odpowiedzi DNS w pamięci podręcznej przez klientów i przez serwery DNS cykliczne, których klienci rozpoznawania nazw DNS. Ten pamięci podręcznej mogą mieć wpływ na rozkład ważonej ruch. W przypadku dużej liczby klientów i serwery DNS cykliczne dystrybucji ruchu działa zgodnie z oczekiwaniami. Jednak w przypadku małej liczby klientów lub serwerów DNS cykliczne pamięci podręcznej znacznie pochylić dystrybucji ruchu.

Typowe przypadki użycia obejmują:

- Środowisk projektowych i testowych
- Komunikacja aplikacji do aplikacji
- Aplikacje celem wąskie użytkownika — base, którego udostępnianie infrastrukturze DNS cykliczne typowych (na przykład pracowników firmy nawiązywania połączenia przez serwer proxy)

Te efekty buforowania DNS są wspólne dla wszystkich ruchem DNS systemów routingu, nie tylko Menedżer ruchu Azure. W niektórych przypadkach jawnie wyczyścić pamięć podręczną DNS mogą dostarczyć obejść ten problem. W innych przypadkach alternatywna metoda routingu ruchu może być bardziej odpowiednie.

## <a name="performance-traffic-routing-method"></a>Metoda routingu ruchu wydajności

Wdrażanie punkty końcowe w dwóch lub kilku lokalizacjach na całym świecie poprawić czas reakcji aplikacji wiele przez przekazywanie ruchu do wybranej lokalizacji najbliżej, do Ciebie. Metoda routingu ruchu "Wydajności" obsługuje tę funkcję.

![Azure Menedżer ruchu "Wydajności" metody routingu ruchu][3]

Punkt końcowy "najbliższy" nie jest niekoniecznie najbliższym mierzonych według odległość geograficznych. Zamiast tego metodę routingu ruchu "Wydajności" Określa punkt końcowy najbliższym za pomocą pomiaru czasu oczekiwania w sieci. Menedżer ruch zapisuje tabeli opóźnienie programu Internet do śledzenia przebiegu czasu między zakresy adresów IP i każdy Azure centrum danych.

Menedżer ruchu wyszukuje źródłowy adres IP przychodzące żądanie DNS w tabeli opóźnienie Internet. Menedżer ruchu wybiera punktu końcowego dostępne w Azure centrum danych, ma najniższe opóźnienie tego zakresu adres IP, a następnie zwraca tego punktu końcowego w odpowiedzi DNS.

Jak wyjaśniono, w [Jaki sposób działa Menedżer ruchu](traffic-manager-how-traffic-manager-works.md), Menedżer ruchu nie otrzymuje kwerend DNS bezpośrednio z klientami. Zamiast kwerendy DNS pochodzą z usługi DNS cykliczne że klienci są skonfigurowane do za pomocą. W związku z tym, adres IP służące do ustalania "najbliższy" punkt końcowy nie jest adres IP klienta, ale jest adres IP usługi DNS cykliczne. W praktyce ten adres IP jest dobrym serwera proxy dla klienta.

Menedżer ruchu regularnie aktualizuje tabeli opóźnienie Internet, aby uwzględnić zmiany w globalnej internetowe i nowe obszary Azure. Jednak wydajność aplikacji zależy w czasie rzeczywistym warianty obciążenia w Internecie. Routingu ruchu wydajności nie monitorować obciążenie punkt końcowy danej usługi. Jednak jeśli punkt końcowy jest niedostępny, Menedżer ruchu oznacza nie w odpowiedzi na kwerendy DNS.

Informacje, które należy pamiętać:

- Jeśli Twój profil zawiera wiele punktów końcowych w tym samym regionie Azure, następnie Menedżer ruchu rozdziela ruch równomiernie w dostępne punkty końcowe w danym regionie. Jeśli wolisz rozkładu inny ruch w regionie, można użyć [zagnieżdżonych profile Menedżer ruchu](traffic-manager-nested-profiles.md).

- Jeśli wszystkie włączone punkty końcowe w danym regionie Azure są ograniczone, Menedżer ruchu rozdziela ruch przez wszystkie inne dostępne punkty końcowe zamiast najbliższy punktu końcowego. Tę logikę zapobiega awarii kaskadowych za nie przeciążenie najbliższy punktu końcowego. Do definiowania sekwencji preferowanej pracy awaryjnej, należy użyć [zagnieżdżonych profile Menedżer ruchu](traffic-manager-nested-profiles.md).

- Podczas korzystania z metody routingu ruchu wydajności z zewnętrznych punktów końcowych lub zagnieżdżonych punktów końcowych, należy określić lokalizację tych punktów końcowych. Wybierz obszar Azure najbliżej wdrożenia. Te lokalizacje są obsługiwane przez Internet tabelę opóźnienie wartości.

- Algorytm wybiera punkt końcowy jest deterministyczna. Kwerendy DNS powtarzających się od tego samego klienta są kierowane do samego punktu końcowego. Zazwyczaj klientów za pomocą różnych cykliczne serwerów DNS podczas podróży. Klient może być przesłane do innego punktu końcowego. Routing może również dotyczyć aktualizacje do tabeli opóźnienie Internet. W związku z tym metody routingu ruchu wydajności nie gwarantuje, że klient jest zawsze kierowane do samego punktu końcowego.

- Po zmianie tabeli opóźnienie Internet można zauważyć, że niektórzy klienci są kierowane do innego punktu końcowego. Ta zmiana routingu jest dokładniejsze na podstawie bieżących danych opóźnienie. Te aktualizacje są niezbędne do obsługi dokładność routingu ruchu wydajności, jak Internet stale rozwoju.

## <a name="next-steps"></a>Następne kroki

Dowiedz się, jak opracowywania aplikacji wysokiej dostępności za pomocą [Menedżera ruch punktu końcowego monitorowania](traffic-manager-monitoring.md)

Dowiedz się, jak [utworzyć profil Menedżer ruchu](traffic-manager-manage-profiles.md)

<!--Image references-->
[1]: ./media/traffic-manager-routing-methods/priority.png
[2]: ./media/traffic-manager-routing-methods/weighted.png
[3]: ./media/traffic-manager-routing-methods/performance.png
