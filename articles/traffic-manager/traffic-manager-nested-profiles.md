<properties
    pageTitle="Zagnieżdżone profile Menedżera ruch | Microsoft Azure"
    description="W tym artykule opisano funkcję "Zagnieżdżonych profile" Menedżer ruchu Azure"
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

# <a name="nested-traffic-manager-profiles"></a>Zagnieżdżone profile Menedżer ruchu

Menedżer ruchu udostępnia szereg metod routingu ruchu, które umożliwiają kontrolowanie, jak Menedżer ruchu wybiera, który punkt końcowy otrzymujących ruchu z każdego użytkownika końcowego. Aby uzyskać więcej informacji zobacz [Menedżer ruchu routingu ruchu metody](traffic-manager-routing-methods.md).

Każdy profil Menedżer ruchu określa pojedynczą metodę routingu ruchu. Istnieją jednak scenariuszy, które wymagają bardziej zaawansowane routingu ruchu niż routing dostarczony przez jednego profilu Menedżer ruchu. Można zagnieżdżać profile Menedżer ruchu, aby połączyć korzyści wynikających z więcej niż jedną metodę routingu ruchu. Profile zagnieżdżonych umożliwiają zastępują domyślne zachowanie Menedżer ruchu z obsługą większych i bardziej złożonych wdrożeń aplikacji.

W poniższych przykładach przedstawiono sposób używania zagnieżdżonych profile Menedżer ruchu w różnych scenariuszach.

## <a name="example-1-combining-performance-and-weighted-traffic-routing"></a>Przykład 1: Łączenie "Wydajność" i "Ważone" routingu ruchu

Załóżmy, że wdrożyć aplikację w następujących regionach Azure: zachód Stanów Zjednoczonych, zachód Europie i Azji Wschodniej. Metoda routingu ruchu "Wydajności" Menedżer ruchu umożliwia rozpowszechnianie ruch do obszaru najbliżej użytkownika.

![Menedżer ruchu jednym profilu][1]

Załóżmy, że testowanie jest aktualizacja usługi przed powszechnie stopniowych się więcej. Chcesz "ważona" Metoda routingu ruchu umożliwia bezpośrednie niewielki procent ruchu do wdrożenia test. Możesz skonfigurować wdrożenie test obok istniejącego wdrożenia produkcji w Europie Zachód.

Nie można połączyć oba ważone i "wydajności — routingu ruchu w jednym profilu. Aby zrealizować ten scenariusz, możesz utworzyć profilu Menedżer ruchu za pomocą dwóch punktów końcowych Europa Zachodnia i metody routingu ruchu "Ważone". Następnie możesz dodać tego profilu "podrzędny" jako punkt końcowy do profilu "parent". Profil nadrzędnej nadal używana metoda routingu ruchu wydajności i zawiera globalnej wdrożeń jako punkty końcowe.

Na poniższym diagramie przedstawiono w tym przykładzie:

![Zagnieżdżone profile Menedżer ruchu][2]

W tej konfiguracji ruch kierowany za pośrednictwem profilu nadrzędnej rozdziela ruchu w regionach normalnie. W Europie zachód zagnieżdżonych profilu rozdziela ruch do punktów końcowych produkcji i badania według wagi przypisane.

Gdy profil nadrzędnej używa metody routingu ruchu "Wydajności", każdego punktu końcowego musi mieć przypisane lokalizacji. Lokalizacja zostanie przypisany podczas konfigurowania punktu końcowego. Wybierz obszar Azure najbliżej wdrożenia. Azure regiony są obsługiwane przez Internet tabelę opóźnienie wartości lokalizacji. Aby uzyskać więcej informacji, zobacz [Menedżer ruchu "Wydajności" metody routingu ruchu](traffic-manager-routing-methods.md#performance-traffic-routing-method).

## <a name="example-2-endpoint-monitoring-in-nested-profiles"></a>Przykład 2: Punkt końcowy monitorowanie zagnieżdżone profilów

Menedżer ruchu aktywnie monitoruje kondycję każdego punktu końcowego usługi. Jeśli punkt końcowy jest nieprawidłowe, Menedżer ruchu kieruje użytkownikom na alternatywny punkty końcowe, aby zachować dostępność tej usługi. Dotyczy to zachowanie monitorowania i pracy awaryjnej punkt końcowy do wszystkich metod routingu ruchu. Aby uzyskać więcej informacji zobacz [Menedżer ruchu monitorowania punktu końcowego](traffic-manager-monitoring.md). Monitorowanie punktu końcowego działaniu profilów zagnieżdżonych. Profile zagnieżdżonych profilu nadrzędny nie wykonywanie sprawdzanie kondycji na podrzędne bezpośrednio. Zamiast tego kondycji punkty końcowe profilu podrzędne jest używana do obliczania ogólnej kondycji profilu podrzędne. Te informacje dotyczące kondycji jest propagowana górę hierarchii zagnieżdżonych profilu. Profil nadrzędnej, to agregacją kondycja, aby określić, czy ma być skierowanie ruchu do profilu podrzędne. Zobacz sekcję [często zadawane pytania dotyczące](#faq) tego artykułu, aby uzyskać szczegółowe informacje dotyczące kondycji monitorowania zagnieżdżonych profilów.

Powrót do poprzedniego przykładu, załóżmy, że wdrożenie produkcji w Europie zachód kończy się niepowodzeniem. Domyślnie profil "podrzędny" kieruje cały ruch do wdrożenia test. Jeśli wdrożenie test również nie powiedzie się, profil nadrzędnej Określa, że profil podrzędne nie powinny być ruch, ponieważ wszystkie podrzędne punkty końcowe są nieprawidłowe. Następnie profilu nadrzędnej rozdziela ruch w innych regionach.

![Zagnieżdżone profilu pracy awaryjnej (zachowanie domyślne)][3]

Może być zadowalająca w ramach tej Umowy. Lub może być zainteresowanych, że wdrożenia test zamiast ruch ograniczony podzbiór będzie teraz cały ruch Europe Zachód. Bez względu na to kondycji wdrożenia test ma przechodzić do innych regionach w przypadku niepowodzenia wdrażania produkcji w Europie Zachód. Aby możliwe było to awaryjne przeniesienie, zostanie użyty parametr "MinChildEndpoints" podczas konfigurowania profilu dziecka jako punkt końcowy w profilu nadrzędnej. Parametr określa minimalną liczbę dostępne punkty końcowe w profilu podrzędne. Wartość domyślna to "1". W tym scenariuszu wartość MinChildEndpoints do 2. Poniżej tego progu profilu nadrzędne są uwzględnione profil całego podrzędny, który ma być niedostępny i kieruje ruch do innych punktów końcowych.

Na poniższym rysunku przedstawiono tej konfiguracji:

![Zagnieżdżone pracy awaryjnej profilu z "MinChildEndpoints" = 2][4]

>[AZURE.NOTE]
>Metoda routingu ruchu "Priorytet" rozdziela cały ruch do jeden punkt końcowy. Dlatego jest nieco przeznaczenie ustawienie MinChildEndpoints inną niż "1" profilu podrzędne.

## <a name="example-3-prioritized-failover-regions-in-performance-traffic-routing"></a>Przykład 3: Priorytety pracy awaryjnej regionów w routingu ruchu "wydajność.

Domyślne zachowanie metody routingu ruchu "Wydajności" została zaprojektowana tak w celu uniknięcia nadmiernie ładowania dalej najbliższej punktu końcowego i powoduje kaskadowych dużej liczby błędów. W przypadku niepowodzenia punktu końcowego, cały ruch, który będzie kierowany do tego punktu końcowego jest równomiernie rozkładany do innych punktów końcowych wszystkich regionów.

![Routing z pracą awaryjną domyślnego ruchu "wydajność.][5]

Załóżmy jednak wolisz tym przełączeniu ruch Zachodnia Europa Zachodnia Stanów Zjednoczonych, a tylko bezpośrednie ruchu w innych regionach, gdy oba punkty końcowe są niedostępne. Możesz utworzyć tego rozwiązania przy użyciu profilu podrzędny przy użyciu metody routingu ruchu "Priorytet".

![Routing z pracą awaryjną preferencji ruchu "wydajność.][6]

Ponieważ punkt końcowy Zachodnia Europa ma wyższy priorytet niż punkt końcowy zachód Stanów Zjednoczonych, cały ruch są wysyłane do punktu końcowego Europa Zachodnia, gdy oba punkty końcowe są w trybie online. Jeśli Europa Zachodnia kończy się niepowodzeniem, jej ruch jest przekierowywany do USA Zachód. Przy użyciu zagnieżdżonych profilu ruch jest przekierowywany do Azji Wschodniej tylko wtedy, gdy zarówno Europa Zachodnia i zachód USA kończą się niepowodzeniem.

Dla wszystkich regionów, można powtarzać tego wzorca. Zamień wszystkie trzy punkty końcowe w profilu nadrzędnej trzy profile podrzędny, każdy dostarczając sekwencji priorytetów pracy awaryjnej.

## <a name="example-4-controlling-performance-traffic-routing-between-multiple-endpoints-in-the-same-region"></a>Przykład 4: Sterowanie "Wydajności" routingu ruchu między wieloma punkty końcowe w tym samym regionie

Załóżmy, że "Wydajność" routingu ruchu metoda jest używana w profilu, który ma więcej niż jeden punkt końcowy w wybranym obszarze. Domyślnie ruch kierowany do tego regionu jest rozłożone równomiernie na wszystkie dostępne punkty końcowe w danym regionie.

!["Działania" ruchu routingu dystrybucji ruchu w regionie (zachowanie domyślne)][7]

Zamiast dodawać wiele punktów końcowych w Europie Zachód, te punkty końcowe są ujęte w profilu osobnych podrzędne. Profil podrzędne jest dodawany do nadrzędnego jako punkt końcowy tylko w Europie Zachód. Ustawienia w profilu podrzędne można kontrolować rozkład ruch Europa Zachodnia, umożliwiając routingu priorytet lub średniej ważonej ruchu w danym regionie.

![Routing z rozkładem niestandardowego ruchu w regionie ruchu "wydajność.][8]

## <a name="example-5-per-endpoint-monitoring-settings"></a>Przykład 5: Ustawienia monitorowania na końcowego

Załóżmy, że używasz Menedżera ruch aby sprawniej przeprowadzić migrację ruchu z starsza wersja lokalnej witryny sieci web do nowej wersji opartej na chmurze obsługiwany w Azure. Starsze witryny chcesz monitorowanie kondycji witryny za pomocą strony głównej identyfikatora URI. Ale dla nowej wersji opartej na chmurze wdraża niestandardowej strony monitorowania (ścieżka "/ monitor.aspx") zawierającego dodatkowe testy.

![Punkt końcowy Menedżer ruchu monitorowania (zachowanie domyślne)][9]

Ustawienia monitorowania w profilu Menedżer ruchu Zastosuj do wszystkich punktów końcowych w jednym profilu. Zagnieżdżone profilów umożliwia profilu różnych podrzędne na witrynę Definiowanie różne ustawienia monitorowania.

![Punkt końcowy Menedżer ruchu monitorowanie przy użyciu ustawienia jednego punktu końcowego][10]

## <a name="faq"></a>FAQ

### <a name="how-do-i-configure-nested-profiles"></a>Jak skonfigurować profile zagnieżdżonych?

Zagnieżdżone profile Menedżera ruch można skonfigurować przy użyciu zarówno Menedżera zasobów Azure i klasyczny Azure pozostałych interfejsów API, poleceń cmdlet środowiska PowerShell Azure i poleceń polecenie Azure między platformami. Są również obsługiwane przez nowe portal Azure. Nie są obsługiwane w portalu klasyczny.

### <a name="how-many-layers-of-nesting-does-traffic-manger-support"></a>Ile warstwy zagnieżdżania czy Menedżer ruchu obsługuje?

Można zagnieżdżać profile maksymalnie 10 poziomów. "Pętle" nie są dozwolone.

### <a name="can-i-mix-other-endpoint-types-with-nested-child-profiles-in-the-same-traffic-manager-profile"></a>Czy można łączyć innych typów punktu końcowego, z profilami zagnieżdżonych podrzędne, w tym samym profilu Menedżer ruchu?

Wartość Tak. Nie ma żadnych ograniczeń na sposób łączenia punkty końcowe różnych typów w obrębie profilu.

### <a name="how-does-the-billing-model-apply-for-nested-profiles"></a>Jak rozliczeń modelu zastosować profilów zagnieżdżone?

Istnieje nie ujemne ceny wpływ przy użyciu zagnieżdżonych profilów.

Menedżer ruchu rozliczeń ma dwa składniki: sprawdzanie kondycji punktu końcowego i miliony kwerend DNS

- Sprawdzanie kondycji punkt końcowy: są bezpłatne dla profilu podrzędne skonfigurowanego jako punkt końcowy w profilu nadrzędnej. Monitorowanie punkty końcowe w profilu podrzędne są wystawiona w zwykły sposób.
- Kwerendy DNS: każda kwerenda są traktowane jako raz. Zapytanie zwracające punktu końcowego z profilu podrzędne profil nadrzędny jest wliczane w profilu nadrzędnej.

Aby uzyskać szczegółowe informacje zobacz [Menedżer ruchu cennik strony](https://azure.microsoft.com/pricing/details/traffic-manager/).

### <a name="is-there-a-performance-impact-for-nested-profiles"></a>Ma wpływ na wydajność profilów zagnieżdżonych?

Wartość nie. Nie ma żadnego wpływu wydajności poniesionych podczas korzystania z profilami zagnieżdżonych.

Serwery nazw Menedżer ruchu przechodzenia hierarchii profilu wewnętrznie podczas przetwarzania każda kwerenda DNS. Kwerenda DNS do profilu nadrzędnej może otrzymywać odpowiedzi DNS z punktem końcowym od profilu podrzędne. Jeden rekord CNAME jest używana, czy korzystasz z jednego profilu lub zagnieżdżonych profile. Nie jest konieczne do utworzenia rekordu CNAME dla każdego profilu w hierarchii.

### <a name="how-does-traffic-manager-compute-the-health-of-a-nested-endpoint-in-a-parent-profile"></a>Jak Menedżer ruchu obliczyć kondycji zagnieżdżonych punkt końcowy w profilu nadrzędnej?

Profil nadrzędny nie sprawdzania kondycji na podrzędne bezpośrednio. Zamiast tego kondycji profilu podrzędne punkty końcowe są używane do obliczania ogólnej kondycji profilu podrzędne. Te informacje są propagowane w górę hierarchii zagnieżdżonych profilu ustalenie kondycji zagnieżdżonych punktu końcowego. Profil nadrzędnej używa tej zagregowane kondycji do określenia, czy dane mogą być kierowane do podrzędne.

W poniższej tabeli opisano zachowanie z Menedżera ruch kondycji sprawdza zagnieżdżonych punktu końcowego.

|Podrzędne profil monitoruje stan|Nadrzędny punkt końcowy monitoruje stan|Notatki|
|---|---|---|
|Wyłączone. Profil podrzędny został wyłączony.|Zatrzymano|Stan punktu końcowego nadrzędnej zatrzymaniu wyłączony. Stan wyłączona jest zarezerwowana do wskazywania wyłączono punkt końcowy w profilu nadrzędnej.|
|Obniżonej wydajności. Co najmniej jeden element podrzędny profilu końcowym jest w stanie obniżony.| Online: liczba Online punkty końcowe w profilu podrzędne co najmniej jest wartością MinChildEndpoints.<BR>CheckingEndpoint: liczba Online oraz CheckingEndpoint punkty końcowe w profilu podrzędne co najmniej jest wartością MinChildEndpoints.<BR>Ograniczone: inaczej.|Ruch jest przekierowywane do punktu końcowego stanu CheckingEndpoint. Jeśli MinChildEndpoints jest zbyt duża, obniża się zawsze punkt końcowy.|
|W trybie online. Co najmniej jeden element podrzędny profilu końcowym jest do trybu Online. Nie punktu końcowego jest w stanie obniżony.|Zobacz powyżej.||
|CheckingEndpoints. Co najmniej jeden element podrzędny profilu końcowym jest "CheckingEndpoint". Nie punkty końcowe są "Online" lub "Obniżonej wydajności"|Działa tak samo jak powyżej.||
|Nieaktywny. Wszystkie punkty końcowe profilu podrzędne są wyłączone lub zatrzymania lub tego profilu ma żadnych punktów końcowych.|Zatrzymano||


## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej o [sposobie działania Menedżer ruchu](traffic-manager-how-traffic-manager-works.md)

Dowiedz się, jak [utworzyć profil Menedżer ruchu](traffic-manager-manage-profiles.md)

<!--Image references-->
[1]: ./media/traffic-manager-nested-profiles/figure-1.png
[2]: ./media/traffic-manager-nested-profiles/figure-2.png
[3]: ./media/traffic-manager-nested-profiles/figure-3.png
[4]: ./media/traffic-manager-nested-profiles/figure-4.png
[5]: ./media/traffic-manager-nested-profiles/figure-5.png
[6]: ./media/traffic-manager-nested-profiles/figure-6.png
[7]: ./media/traffic-manager-nested-profiles/figure-7.png
[8]: ./media/traffic-manager-nested-profiles/figure-8.png
[9]: ./media/traffic-manager-nested-profiles/figure-9.png
[10]: ./media/traffic-manager-nested-profiles/figure-10.png

