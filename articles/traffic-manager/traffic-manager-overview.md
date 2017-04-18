<properties
    pageTitle="Co to jest Menedżer ruchu | Microsoft Azure"
    description="Ten artykuł pomoże zrozumieć, co to jest Menedżer ruchu i czy jest wybór routingu prawej ruchu aplikacji"
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

# <a name="overview-of-traffic-manager"></a>Omówienie Menedżera ruchu

Menedżer ruch programu Microsoft Azure pozwala na sterowanie dystrybucji ruchu użytkownika dla punktów końcowych usługi w różnych centrach danych. Punkty końcowe usługi obsługiwane przez Menedżera ruch uwzględnić maszyny wirtualne Azure, aplikacje sieci Web i usług w chmurze. Za pomocą Menedżera ruchu z zewnętrznymi, nie Azure punktów końcowych.

Menedżer ruchu używa systemu nazw domen (DNS) do kierowania żądania klienta do punktu końcowego najbardziej odpowiednie na podstawie [metody routingu ruchu](traffic-manager-routing-methods.md) i kondycji punktów końcowych. Menedżer ruchu udostępnia szereg metod routingu ruchu do własnych potrzeb innej aplikacji, [Monitorowanie](traffic-manager-monitoring.md)kondycji punktu końcowego i automatyczne przejście. Menedżer ruchu jest mechanizm awarii, w tym również awarię całego regionu Azure.

## <a name="traffic-manager-benefits"></a>Zalety Menedżer ruchu

Menedżer ruchu mogą ułatwić:

- **Poprawianie dostępności aplikacji krytycznych**

    Menedżer ruchu zapewnia wysoką dostępność dla aplikacji, monitorowanie punktów końcowych i podając automatyczne przejście, gdy punkt końcowy awarii.

- **Poprawa czas reakcji aplikacji wysokiej wydajności**

    Azure umożliwia uruchomienie usług w chmurze lub witryny sieci Web w centrach danych na całym świecie. Menedżer ruchu poprawia czas reakcji aplikacji, przekierowując ruch do punktu końcowego o najmniejszej opóźnień sieci klienta.

- **Wykonywanie konserwacji usługi bez przestojów**

    Można wykonywać operacje planowanej konserwacji na aplikacji bez przestojów. Menedżer ruchu kieruje ruch do innych punktów końcowych, gdy trwa utrzymania.

- **Łączenie lokalnego i aplikacje oparte na chmurze**

    Menedżer ruchu obsługuje zewnętrzne, punkty końcowe nie Azure włączenie go do użytku z hybrydowego w chmurze i lokalnych wdrożeniach, w tym "serii do chmury," "Migrowanie do chmury," i "pracy awaryjnej w chmurze" scenariusze.

- **Rozpowszechnianie ruchu w przypadku dużych, złożonych wdrożeń**

    Przy użyciu [zagnieżdżonych ruch Menedżer profilów](traffic-manager-nested-profiles.md), metod routingu ruchu można łączyć utworzyć reguły zaawansowane i elastyczne do obsługi potrzeb większych i bardziej złożonych wdrożeń.

[AZURE.INCLUDE [load-balancer-compare-tm-ag-lb-include.md](../../includes/load-balancer-compare-tm-ag-lb-include.md)]

## <a name="next-steps"></a>Następne kroki

- Dowiedz się więcej o [sposobie działania Menedżer ruchu](traffic-manager-how-traffic-manager-works.md).

- Informacje o sposobie tworzenia aplikacji wysokiej dostępności za pomocą [Menedżera ruch punktu końcowego monitorowania](traffic-manager-monitoring.md).

- Dowiedz się więcej na temat [metod routingu ruchu](traffic-manager-routing-methods.md) obsługiwane przez Menedżera ruch.

- [Tworzenie profilu Menedżer ruchu](traffic-manager-manage-profiles.md).

