<properties
 pageTitle="Rozmiary usług w chmurze | Microsoft Azure"
 description="Wyświetla rozmiary różnych maszyn wirtualnych (i identyfikatory) dla Azure chmury usługi sieci web i pracownik ról."
 services="cloud-services"
 documentationCenter=""
 authors="Thraka"
 manager="timlt"
 editor=""/>
<tags
 ms.service="cloud-services"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="tbd"
 ms.date="10/27/2016"
 ms.author="adegeo"/>

# <a name="sizes-for-cloud-services"></a>Rozmiary usług w chmurze

W tym temacie opisano dostępne rozmiary i opcje usługi w chmurze roli wystąpienia (ról w sieci web i pracownika). Umożliwia także zagadnienia dotyczące rozmieszczania, o których warto pamiętać podczas planowania korzystać z tych zasobów. Rozmiar każdego ma identyfikator umieszczasz w [pliku definicji usługi](cloud-services-model-and-package.md#csdef).

Usług w chmurze jest jednym z wielu typów zasobów do uruchamiania oferowanych przez Azure. Kliknij [tutaj](cloud-services-choose-me.md) Aby uzyskać więcej informacji na temat usług w chmurze.

> [AZURE.NOTE]Aby wyświetlić pokrewne limity Azure, zobacz [subskrypcji Azure i limity dotyczące usługi, przydziały oraz ograniczenia](../azure-subscription-service-limits.md)

## <a name="sizes-for-web-and-worker-role-instances"></a>Rozmiary dla sieci web i pracownik wystąpień roli

Istnieje wiele standardowych rozmiarów Azure. Zagadnienia dotyczące niektórych rozmiary obejmują:

* Maszyny wirtualne serii są przeznaczone do uruchamiania aplikacji, które wymagają dysku tymczasowym wydajność i nowszy power obliczeń. Maszyny wirtualne serii procesorów szybciej, wyższy stosunek pamięci do podstawowych i dyskiem (SSD) przewidzieć dysku tymczasowym. Aby uzyskać szczegółowe informacje Zobacz ogłoszeniu w blogu Azure, [Nowe rozmiary maszyn wirtualnych serii](https://azure.microsoft.com/blog/2014/09/22/new-d-series-virtual-machine-sizes/).

* Serii Dv2, kontynuacja do oryginalnej serii D, funkcji Procesora bardziej zaawansowanych. Procesor Dv2 serii jest około 35% szybciej niż Procesora serii. Jest oparty na najnowszej generacji 2,4 GHz Intel Xeon® E5-2673 v3 procesor (Haswell) i 2.0 technologii zwiększenie wydajności Turbo firmy Intel, można przejść do 3.1 GHz. Seria Dv2 ma samej konfiguracji pamięci i na dysku jako serii D.

*   Maszyny wirtualne serii G oferują najwięcej pamięci i uruchamiać na hostów, które mają rodziny procesorów firmy Intel Xeon E5 V3.

*   Seria A maszyny wirtualne mogą być rozmieszczone na różnych typów sprzętu i procesorów. To ograniczenie rozmiaru na podstawie sprzętu do oferowania wydajności procesora zgodne uruchomionego wystąpienia, niezależnie od tego sprzętu, który zostanie wdrożony w. Aby określić sprzęt fizyczny, na którym jest używany rozmiar, kwerendę sprzęt wirtualnych z poziomu maszyny wirtualnej.

*   Rozmiar A0 to nadmiernie subskrybowanych na sprzętu. Dla tego określonego rozmiaru tylko innych wdrożeniach klienta może mieć wpływ na wydajność usługi uruchomionego obciążenie pracą. Względne wyniki są przedstawione poniżej jako oczekiwany według planu bazowego, objęte przybliżonego zmienności 15 procent.


Rozmiar maszyny wirtualnej ma wpływ na ceny. Rozmiar wpływa także możliwości przetwarzania, pamięci i przechowywania maszyny wirtualnej. Koszty magazynowania są obliczane oddzielnie na podstawie używanych stron w oknie konta miejsca do magazynowania. Aby uzyskać szczegółowe informacje zobacz [Szczegóły ceny maszyn wirtualnych](https://azure.microsoft.com/pricing/details/virtual-machines/) i [Ceny miejsca do magazynowania Azure](https://azure.microsoft.com/pricing/details/storage/). 


Następujące kwestie mogą ułatwić decyduje o rozmiarze:


* Rozmiary A8 A11 i H serii są nazywane także *wystąpienia obliczeniowych*. Zaprojektowane i zoptymalizowane pod kątem obliczeniowych sprzętu uruchamianej rozmiary i aplikacje obciążenie sieci, w tym obliczeniowej wysokiej wydajności (HPC) klaster aplikacji, modelowania i symulacji. Seria A8 A11 używa Intel Xeon E5 — 2670 @ 2,6 GHZ i serii H używa v3 Intel Xeon E5-2667 @ 3,2 GHz. Aby uzyskać szczegółowe informacje i zagadnienia dotyczące korzystania z tych rozmiarów zobacz [informacje o serii H i obliczeniowych maszyny wirtualne A serii](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md). 

* Dv2 serii, serii D, G serii doskonale nadają się do aplikacji, które wymagają szybszych procesorów, lepiej lokalnym dysku wydajności lub mają wyższą wymagania pamięci.  Oferują zaawansowane kombinacji dla wielu aplikacji klasy korporacyjnej.

*   Niektóre fizycznej hostów w centrach danych Azure mogą nie obsługuje większych rozmiarów maszyn wirtualnych, takich jak A5 — A11. W wyniku zobaczysz komunikat o błędzie **nie można skonfigurować maszyn wirtualnych {nazwa komputera}** lub **nie można utworzyć maszyn wirtualnych {nazwa komputera}** podczas zmiany rozmiaru istniejące maszyny wirtualnej do nowego rozmiaru; Tworzenie nowej maszyny wirtualnej w wirtualnej sieci utworzone przed 16 kwietnia 2013; czy dodanie nowa maszyna wirtualna do istniejącej usługi w chmurze. Zobacz [o błędzie: "Nie można skonfigurować maszyna wirtualna"](https://social.msdn.microsoft.com/Forums/9693f56c-fcd3-4d42-850e-5e3b56c7d6be/error-failed-to-configure-virtual-machine-with-a5-a6-or-a7-vm-size?forum=WAVirtualMachinesforWindows) na forum pomocy technicznej dla obejścia dla każdego scenariusza wdrożenia.  

* Twoja subskrypcja również może ograniczyć liczby rdzeni, który można wdrożyć w niektórych rodzin rozmiar. Aby zwiększyć limit, skontaktuj się z obsługą Azure.


## <a name="performance-considerations"></a>Zagadnienia dotyczące wydajności

Utworzono koncepcja jednostki obliczyć Azure (ACU) aby zapewnić sposób porównywania wydajności komputera (CPU) w wersji produktu Azure. Dzięki temu identyfikację, czyli SKU najczęściej spełniają Twoje potrzeby wydajności.  ACU jest obecnie ustandaryzowane na małych (Standard_A1) maszyn wirtualnych jest następnie 100 i wszystkie inne wersje reprezentują około ile szybciej że SKU można uruchamiać standardowego wzorca. 

>[AZURE.IMPORTANT] ACU jest tylko wskazówki.  Wyniki z pracą mogą się różnić. 

<br>

|Jednostka SKU rodziny |ACU-Core |
|---|---|
|[Standard_A0](#a-series)   |50 |
|[Standard_A1-4](#a-series) |100 |
|[Standard_A5-7](#a-series) |100 |
|[A8 A11](#a-series)    |225 *|
|[D1-14](#d-series) |160 |
|[D1 15v2](#dv2-series) |210 - 250 *|
|[G1-5](#g-series)  |180 – 240 *|
|[H](#h-series) |290 - 300 *|

ACUs oznaczone symbolem * za pomocą technologii Intel® Turbo zwiększyć częstotliwość Procesora i podaj podniesienie wydajności.  Ilość zwiększenie wydajności mogą się różnić w zależności od rozmiar pamięci Wirtualnej, obciążenie pracą i inne obciążenia uruchomionych dla tego samego hosta.

## <a name="size-tables"></a>Zmień rozmiar tabeli

W poniższej tabeli przedstawiono rozmiary i możliwości, które zapewniają.

* Pojemność są wyświetlane w jednostkach nosek lub 1024 ^ 3 bajtów. Gdy porównywanie dysków mierzony w GB (1000 ^ 3 bajtów) na dyskach mierzony w nosek (1024 ^ 3) należy pamiętać, że liczby zdolności podane w nosek może pojawić się mniejszy. Na przykład 1023 nosek = 1098.4 GB

* Niewystarczająca przepustowość jest mierzony w operacji wejścia i wyjścia na sekundę (operacji i/o na SEKUNDĘ) i MB/s gdzie MB/s = 10 ^ 6 bajtów na sekundę.

* Dyski danych mogą działać w trybie buforowanej lub bez buforowania. Operacji dysku zapisujące dane w tryb pamięci podręcznej host jest ustawiony na **tylko do odczytu** lub **odczytu i zapisu**.  Dane bez buforowania dysku operacji tryb pamięci podręcznej hosta jest ustawiony na **Brak**.

* Maksymalna przepustowość sieci jest maksymalna przepustowość zagregowane przydzielona i przypisane według typu maszyn wirtualnych. Maksymalna przepustowość zawiera wskazówki dotyczące wybierania odpowiedniego typu maszyn wirtualnych zapewnienie pojemności sieci odpowiednie jest dostępna. Podczas przenoszenia między niski, umiarkowane, wysoki i bardzo wysoki, przepustowość zwiększa się odpowiednio. Wydajność sieci rzeczywista zależy wiele czynników, takich jak sieci i obciążenia aplikacji oraz ustawienia sieci aplikacji.


## <a name="a-series"></a>A serii

| Rozmiar        | Rdzenie Procesora | Pamięć: nosek | Lokalny dysk twardy: nosek | Maksymalna liczba dyski danych | Niewystarczająca przepustowość max: operacji i/o na SEKUNDĘ | Maksymalna liczba nic / przepustowość sieci |
|-------------|-----------|--------------|-----------------------|----------------|--------------------|-----------------------|
| Standard_A0 | 1         | 0.768        | 20                    | 1              | 1 x 500              | 1 i niska                   |
| Standard_A1 | 1         | 1,75         | 70                    | 2              | 2 x 500              | 1 / Średni              |
| Standard_A2 | 2         | 3,5 GB       | 135                   | 4              | 4 x 500              | 1 / umiarkowane              |
| Standard_A3 | 4         | 7            | 285                   | 8              | 8 x 500              | 2 / wysoki                  |
| Standard_A4 | 8         | 14           | 605                   | 16             | 16 x 500             | 4 / wysoki                  |
| Standard_A5 | 2         | 14           | 135                   | 4              | 4 X 500              | 1 / umiarkowane              |
| Standard_A6 | 4         | 28           | 285                   | 8              | 8 x 500              | 2 / wysoki                  |
| Standard_A7 | 8         | 56           | 605                   | 16             | 16 x 500             | 4 / wysoki                  |

## <a name="a-series---compute-intensive-instances"></a>A-serii — wystąpienia obliczeniowych

Aby uzyskać informacje i zagadnienia dotyczące korzystania z rozmiary, zobacz [informacje o serii H i obliczeniowych maszyny wirtualne A serii](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md).


| Rozmiar         | Rdzenie Procesora | Pamięć: nosek | Lokalny dysk twardy: nosek | Maksymalna liczba dyski danych | Niewystarczająca przepustowość max: operacji i/o na SEKUNDĘ | Maksymalna liczba nic / przepustowość sieci |
|--------------|-----------|--------------|-----------------------|----------------|--------------------|-----------------------|
| Standard_A8 * | 8         | 56           | 382                   | 16             | 16 x 500             | 2 / wysoki                  |
| Standard_A9 * | 16        | 112          | 382                   | 16             | 16 x 500             | 4 / bardzo wysoki             |
| Standard_A10 | 8         | 56           | 382                   | 16             | 16 x 500             | 2 / wysoki                  |
| Standard_A11 | 16        | 112          | 382                   | 16             | 16 x 500             | 4 / bardzo wysoki             |

* RDMA stanie

## <a name="d-series"></a>D.


| Rozmiar         | Rdzenie Procesora | Pamięć: nosek | Lokalne SSD: nosek | Maksymalna liczba dyski danych | Niewystarczająca przepustowość max: operacji i/o na SEKUNDĘ | Maksymalna liczba nic / przepustowość sieci |
|--------------|-----------|--------------|----------------------|----------------|--------------------|-----------------------|
| Standard_D1  | 1         | 3.5          | 50                   | 2              | 2 x 500              | 1 / Średni              |
| Standard_D2  | 2         | 7            | 100                  | 4              | 4 x 500              | 2 / wysoki                  |
| Standard_D3  | 4         | 14           | 200                  | 8              | 8 x 500              | 4 / wysoki                  |
| Standard_D4  | 8         | 28           | 400                  | 16             | 16 x 500             | 8 / wysoki                  |
| Standard_D11 | 2         | 14           | 100                  | 4              | 4 x 500              | 2 / wysoki                  |
| Standard_D12 | 4         | 28           | 200                  | 8              | 8 x 500              | 4 / wysoki                  |
| Standard_D13 | 8         | 56           | 400                  | 16             | 16 x 500             | 8 / wysoki                  |
| Standard_D14 | 16        | 112          | 800                  | 32             | 32 x 500             | 8 / bardzo wysoki             |

## <a name="dv2-series"></a>Seria Dv2

| Rozmiar            | Rdzenie Procesora | Pamięć: nosek | Lokalne SSD: nosek | Maksymalna liczba dyski danych | Niewystarczająca przepustowość max: operacji i/o na SEKUNDĘ | Maksymalna liczba nic / przepustowość sieci |
|-----------------|-----------|--------------|----------------------|----------------|--------------------|-----------------------|
| Standard_D1_v2  | 1         | 3.5          | 50                   | 2              | 2 x 500              | 1 / Średni              |
| Standard_D2_v2  | 2         | 7            | 100                  | 4              | 4 x 500              | 2 / wysoki                  |
| Standard_D3_v2  | 4         | 14           | 200                  | 8              | 8 x 500              | 4 / wysoki                  |
| Standard_D4_v2  | 8         | 28           | 400                  | 16             | 16 x 500             | 8 / wysoki                  |
| Standard_D5_v2  | 16        | 56           | 800                  | 32             | 32 x 500             | 8 / bardzo wysoki        |
| Standard_D11_v2 | 2         | 14           | 100                  | 4              | 4 x 500              | 2 / wysoki                  |
| Standard_D12_v2 | 4         | 28           | 200                  | 8              | 8 x 500              | 4 / wysoki                  |
| Standard_D13_v2 | 8         | 56           | 400                  | 16             | 16 x 500             | 8 / wysoki                  |
| Standard_D14_v2 | 16        | 112          | 800                  | 32             | 32 x 500             | 8 / bardzo wysoki        |
| Standard_D15_v2 | 20        | 140          | 1000                | 40             | 40 x 500             | 8 / bardzo wysoki        |

## <a name="g-series"></a>G serii

| Rozmiar        | Rdzenie Procesora | Pamięć: nosek  | Lokalne SSD: nosek  | Maksymalna liczba dyski danych | Maksymalna liczba jest niewystarczająca przepustowość: operacji i/o na SEKUNDĘ | Maksymalna liczba nic / przepustowość sieci |
|-------------|-----------|--------------|----------------------|----------------|--------------------|-----------------------|
| Standard_G1 | 2         | 28           | 384                  | 4              | 4 x 500            | 1 / wysoki                  |
| Standard_G2 | 4         | 56           | 768                  | 8              | 8 x 500            | 2 / wysoki                  |
| Standard_G3 | 8         | 112          | 1,536                | 16             | 16 x 500           | 4 / bardzo wysoki             |
| Standard_G4 | 16        | 224          | 3072                | 32             | 32 x 500           | 8 / bardzo wysoki        |
| Standard_G5 | 32        | 448          | 6,144                | 64             | 64 x 500           | 8 / bardzo wysoki        |


## <a name="h-series"></a>Seria H

Azure maszyn wirtualnych H serii są następnej generacji wydajność obliczeniowych, że maszyny wirtualne celem wyższej klasy potrzeb obliczeniowych, takich jak modelowanie molekularnej i obliczeniowe dynamics objętości. Te 8 i 16 core maszyny wirtualne są wbudowane na technologii procesor firmy Intel 2667 Haswell E5 V3 DDR4 pamięci i lokalne przechowywanie SSD podstawie. 

Oprócz znacznych power Procesora serii H oferuje różnych opcji krótki czas oczekiwania RDMA sieci przy użyciu FDR InfiniBand i kilka konfiguracji pamięci do obsługi intensywnie obliczeniowa wymagania dotyczące pamięci.


| Rozmiar           | Rdzenie Procesora | Pamięć: nosek | Lokalne SSD: nosek | Maksymalna liczba dyski danych | Maksymalna liczba jest niewystarczająca przepustowość: operacji i/o na SEKUNDĘ | Maksymalna liczba nic / przepustowość sieci |
|----------------|-----------|-------------|--------------------------|----------------|---------------------------|------------------------------|
| Standard_H8    | 8         | 56          | 1000.                     | 16             | 16 x 500                    | 8 / wysoki                      |
| Standard_H16   | 16        | 112         | 2000                     | 32             | 32 x 500                    | 8 / bardzo wysoki                  |
| Standard_H8m   | 8         | 112         | 1000.                     | 16             | 16 x 500                    | 8 / wysoki                      |
| Standard_H16m  | 16        | 224         | 2000                     | 32             | 32 x 500                    | 8 / bardzo wysoki                 |
| Standard_H16r * | 16        | 112         | 2000                     | 32             | 32 x 500                    | 8 / bardzo wysoki                  |
| Standard_H16mr * | 16        | 224         | 2000                     | 32             | 32 x 500                    | 8 / bardzo wysoki                  |


* RDMA stanie

## <a name="notes-standard-a0---a4-using-cli-and-powershell"></a>Uwagi: Standardowy A0 - A4 przy użyciu interfejsu wiersza polecenia i programu PowerShell 

W modelu Klasyczny wdrożenia nazwy rozmiar niektórych maszyn wirtualnych nieco różnią się polecenie i programu PowerShell:

* Standard_A0 jest ExtraSmall 
* Standard_A1 jest mały
* Standard_A2 jest średni
* Standard_A3 jest duży
* Standard_A4 jest ExtraLarge

## <a name="configure-sizes-for-cloud-services"></a>Konfigurowanie rozmiarów dla usług w chmurze

Możesz określić rozmiar maszyn wirtualnych w przypadku wystąpienia ról w ramach modelu usług opisane w [pliku definicji usługi](cloud-services-model-and-package.md#csdef). Rozmiar roli określa liczbę rdzenie Procesora, pojemności pamięci i rozmiaru system lokalny pliku przydzielona do uruchomionego wystąpienia. Wybierz rozmiar roli według zapotrzebowanie aplikacji.

Oto przykład określania rozmiar rola ma być [Standard_D2](#general-purpose-d) w przypadku wystąpienia ról w sieci Web:

```xml
<WorkerRole name="Worker1" vmsize="<mark>Standard_D2</mark>">
...
</WorkerRole>
```

## <a name="next-steps"></a>Następne kroki

- Informacje o [subskrypcji azure i limity dotyczące usługi, przydziały oraz ograniczenia](../azure-subscription-service-limits.md).
- Dowiedz się więcej [o serii H i obliczeniowych maszyny wirtualne serii A](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md) dla obciążenia takich jak maks wydajności (HPC).

