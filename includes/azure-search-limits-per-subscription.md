Możesz utworzyć wiele usług w ramach subskrypcji, każdą z nich obsługi administracyjnej na określonej warstwie, ograniczone tylko według liczby usług, które mogą na każdej warstwie. Na przykład można utworzyć maksymalnie 12 usługi na warstwie podstawowe i innego 12 na warstwie S1 w obrębie tej samej subskrypcji. Aby uzyskać więcej informacji na temat poziomów zobacz [Wybierz numer Magazynowy lub warstwa Azure wyszukiwania](../articles/search/search-sku-tier.md).

Limity maksymalnej można podnieść na żądanie. Kontakt z pomocą techniczną Azure, jeśli potrzebujesz więcej usług w obrębie tej samej subskrypcji.

Zasób|Bezpłatne|Podstawowe|S1|S2|S3 |HD S3 <sup>1</sup>
---|---|---|---|----|---|----
Maksymalna liczba usług |1 |12 |12  |6 |6 |6 
Maksymalna skala w SU <sup>2</sup>|N/d <sup>3</sup>|3 SU, <sup>4</sup> |36 SU|36 SU|36 SU|SU 12, 3 SU <sup>5</sup>

<sup>1</sup> S3 HD nie obsługuje [indeksatory](../articles/search/search-indexer-overview.md) w tej chwili. 

<sup>2</sup> jednostki wyszukiwania (SU) są fakturowanego jednostki na usługi, przydzielone jako *replice* lub *partition*. Potrzebujesz oba zasoby dla miejsca do magazynowania, indeksowanie i operacji kwerendy. Aby dowiedzieć się więcej na temat ważnych kombinacji pozostających w obszarze maksymalnych, zobacz [poziomy zasobów skali obciążenia kwerendy i indeksu](../articles/search/search-capacity-planning.md). 

<sup>3</sup> wolny jest oparty na udostępnionych zasobów używanych przez wielu subskrybentów. Na tej warstwie istnieje dedykowane zasobów dla poszczególnych subskrybentów. Z tego powodu maksymalna skala jest oznaczony jako nie dotyczy.

<sup>4</sup> basic ma jedną partycją stały. Na tej warstwie SUs dodatkowe są używane do przydzielania więcej replik dla obciążenia lepszą kwerendy.

<sup>5</sup> S3 HD ma strukturę alokacji różnych pod względem dozwolony kombinacje. Replik możesz mieć maksymalnie 12. Partycje maksimum wynosi 3.




