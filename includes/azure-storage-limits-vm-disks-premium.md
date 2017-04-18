**Maszyn wirtualnych dysków: na limity konta**

Zasób|Domyślny Limit
---|---
Całkowita pojemność dysku dla każdego konta|35 TB
Wydajność migawki sumy dla każdego konta|10 TB
Maksymalna przepustowość dla każdego konta (ingress + wyjściowego<sup>1</sup>)|< = 50 GB

<sup>1</sup> *Ingress* odwołuje się do wszystkich danych (liczba żądań) są wysyłane na konto miejsca do magazynowania. *Wyjściowego* odwołuje się do wszystkich danych (odpowiedzi) odbierane z konta miejsca do magazynowania.

**Maszyn wirtualnych dysków: na limitów na dysku**

Typ dysku magazynu Premium | P 10 | P20 | P30
---|---|---|---
Rozmiar dysku | 128 nosek | 512 nosek | 1024 nosek (1 TB)
Maksymalna liczba operacji i/o na SEKUNDĘ na dysku | 500 | 2300 | 5000
Maksymalna liczba przepustowość na dysku | 100 MB na sekundę | 150 MB na sekundę | 200 MB na sekundę
Maksymalna liczba dysków dla każdego konta miejsca do magazynowania | 280 | 70 | 35

**Maszyn wirtualnych dysków: na limity maszyn wirtualnych**

Zasób|Domyślny Limit
---|---
Maksymalna liczba operacji i/o na SEKUNDĘ na maszyn wirtualnych|80 000 operacji i/o na SEKUNDĘ z GS5 maszyn wirtualnych<sup>1</sup>
Maksymalna liczba przepustowość na maszyn wirtualnych|2000 MB/s GS5 maszyn wirtualnych<sup>1</sup>

<sup>1</sup> Można znaleźć [Rozmiar pamięci Wirtualnej](../articles/virtual-machines/virtual-machines-linux-sizes.md) ograniczenia dotyczące innych rozmiarów maszyn wirtualnych. 
