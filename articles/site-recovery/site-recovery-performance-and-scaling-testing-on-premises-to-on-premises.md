<properties
    pageTitle="Test wydajności i skalę wyniki dla lokalnego replikacji funkcji Hyper-V lokalnego z Odzyskiwanie witryny | Microsoft Azure"
    description="Ten artykuł zawiera informacje o testowania lokalnego do replikacji lokalnego przy użyciu Odzyskiwanie witryny Azure."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor="tysonn"/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="07/06/2016"
    ms.author="raynew"/>

# <a name="performance-test-and-scale-results-for-on-premises-to-on-premises-hyper-v-replication-with-site-recovery"></a>Wydajność testowanie i skalowanie wyników dla lokalnego do lokalnego funkcji Hyper-V replikacji z Odzyskiwanie witryny

Odzyskiwanie witryny Microsoft Azure umożliwia dodać akompaniament i zarządzanie nimi replikacji maszyn wirtualnych i serwerów fizycznych Azure lub pomocnicza centrum danych. Ten artykuł zawiera wyniki testów wydajności, które NAS podczas replikacji maszyn wirtualnych funkcji Hyper-V między dwoma lokalnych centrach danych.



## <a name="overview"></a>Omówienie

Celem testów było sprawdzenie, jak Odzyskiwanie witryny Azure wykonuje się podczas replikacji stanu stałego. Replikacja stanu stałego występuje, gdy maszyn wirtualnych została ukończona replikacji początkowej i jest wykonywana synchronizacja zmian różnicy. Należy do pomiaru wydajności przy użyciu stanu stałej, ponieważ jest stan, w którym większość maszyn wirtualnych pozostać, chyba że wystąpić nieoczekiwane dostawie.


Wdrożenie test składa się z dwóch witryn lokalnego z serwerem VMM w każdej witrynie. Wdrożenie to test jest typowe wdrażania pakietu office głowy/oddział z centrali występującymi jako podstawowej witryny i oddziale w witrynie monitora pomocniczego lub odzyskiwania.

### <a name="what-we-did"></a>Firma Microsoft czy

Oto, co możemy w teście przekazywania:

1. Utworzony przy użyciu szablonów VMM maszyn wirtualnych.

1. Wprowadzenie maszyn wirtualnych i przechwytywania według planu bazowego wskaźniki ponad 12 godzin.

1. Utworzone chmury na serwerach VMM podstawowego i odzyskiwania.

1. Ochrona chmury skonfigurowaną w Odzyskiwanie witryny Azure, łącznie z mapowania chmur źródła i odzyskiwanie.

1. Włączona ochrona dla maszyn wirtualnych i umożliwić ich do wykonania replikacji początkowej.

1. Czas potrzebny na kilka godzin stabilizacji systemu.

1. Przechwytywane wskaźniki ponad 12 godzin, zapewnianie pozostały wszystkich maszyn wirtualnych w stanie oczekiwanych replikacji dla tych 12 godzin.

1. Zmierz różnicy między wskaźniki według planu bazowego i wskaźniki wydajności replikacji.

## <a name="test-deployment-results"></a>Wdrożenie wyniki testu

### <a name="primary-server-performance"></a>Wydajność serwera podstawowego

- Funkcji Hyper-V replice asynchroniczne śledzenie zmian w pliku dziennika o ogólnych minimalne miejsca do magazynowania na serwerze podstawowym.

- Funkcji Hyper-V replice wykorzystuje własny obsługiwanych pamięci podręcznej, aby zminimalizować operacji i/o na SEKUNDĘ obciążenie śledzenia. Przechowuje zapisuje VHDX w pamięci i przenosi je do pliku dziennika przed dziennik są wysyłane do witryny odzyskiwania. Dysk opróżniania również się dzieje, gdy zapisy osiągnął interwałem limit.

- Wykres poniżej pokazano ogólnych operacji i/o na SEKUNDĘ stanu stałego dla replikacji. Widać, że operacji i/o na SEKUNDĘ obciążenie z powodu replikacji jest około 5%, która jest bardzo niska.

![Podstawowe wyniki](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744913.png)

Funkcji Hyper-V replice wykorzystuje pamięci na serwerze podstawowym, aby zoptymalizować dysku. Jak pokazano na poniższym diagramem, pamięć obciążenie na wszystkich serwerach w klastrze podstawowy jest margines. Pamięć obciążenie wyświetlana jest wartością procentową pamięci używanej przez replikacji w porównaniu z pamięć zainstalowanych na serwerze funkcji Hyper-V.

![Podstawowe wyniki](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744914.png)

Funkcji Hyper-V replice ma minimalne obciążenie Procesora. Jak pokazano na wykresie, ogólnych replikacji znajduje się w zakresie 2-3%.

![Podstawowe wyniki](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744915.png)

### <a name="secondary-recovery-server-performance"></a>Wydajność serwera pomocniczego (odzyskiwania)

Replice funkcji Hyper-V używa małą ilością pamięci na serwerze odzyskiwania zoptymalizować liczbę operacji miejsca do magazynowania. Wykres podsumowuje użycie pamięci na serwerze odzyskiwania. Pamięć obciążenie wyświetlane jest wartością procentową pamięci używanej przez replikacji w porównaniu z pamięć zainstalowanych na serwerze funkcji Hyper-V.

![Wyniki pomocniczej](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744916.png)

Kwota operacji We/Wy w witrynie odzyskiwania jest funkcją liczbę operacji zapisu w witrynie podstawowego. Załóżmy Spójrz na operacje We/Wy razem w porównaniu z całkowitą operacji We/Wy w witrynie odzyskiwania i operacje na stronie podstawowego zapisu. Wykresy wskazują, że sumę operacji i/o na SEKUNDĘ w witrynie odzyskiwania jest

- Około 1,5 zapisu operacji i/o na SEKUNDĘ na podstawowy.

- Około 37% sumy operacji i/o na SEKUNDĘ w witrynie podstawowego.

![Wyniki pomocniczej](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744917.png)

![Wyniki pomocniczej](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744918.png)

### <a name="effect-of-replication-on-network-utilization"></a>Efekt replikacji dla wykorzystania sieci

Średnia 275 MB na sekundę przepustowości sieci użyto węzłów podstawowego i odzyskiwania (z włączoną kompresją) przed istniejącej przepustowości 5 GB na sekundę.

![Wykorzystanie sieci wyników](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744919.png)

### <a name="effect-of-replication-on-virtual-machine-performance"></a>Efekt replikacji na wydajność maszyn wirtualnych

Ważne jest wpływ replikacji na obciążenia produkcji uruchomionych maszyn wirtualnych. Jeśli podstawowej witryny jest odpowiednio zainicjowany dla replikacji, nie powinny być wpływu na obciążenie pracą. Śledzenie mechanizmu lightweight replice funkcji Hyper-V zapewnia nie dotyczy obciążenia uruchomiony w środowisku maszyn wirtualnych podczas replikacji stałej liczbie. Jest to zilustrowane w następujących wykresów.

Wykres operacji i/o na SEKUNDĘ wykonywane przez maszyn wirtualnych z systemem innym obciążenia przed i po replikacji został włączony. Aby obserwować, że istnieje różnica między nimi.

![Wyniki efekt replice](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744920.png)

Przepustowość maszyn wirtualnych z systemem innym obciążenia przed i po replikacji została włączona wykres. Można zaobserwować, że replikacji ma znaczącego wpływu.

![Efekty replice wyników](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744921.png)

### <a name="conclusion"></a>Wnioski

Wyniki wyraźnie wskazują, że odzyskiwanie witryny Azure, w połączeniu z funkcji Hyper-V replice, skale z minimum obciążenie dla dużych klaster.  Odzyskiwanie witryny Azure udostępnia proste wdrażanie, replikacji, zarządzanie i monitorowanie. Replice funkcji Hyper-V udostępnia niezbędnej infrastruktury skalowania pomyślnego replikacji. Planowania optymalnego wdrażania Sugerujemy zapoznawanie się pobieranie [Planowania pojemności replice funkcji Hyper-V](https://www.microsoft.com/download/details.aspx?id=39057).

## <a name="test-environment-details"></a>Szczegółowe informacje dotyczące testu środowiska

### <a name="primary-site"></a>Podstawowej witryny

- Witryna podstawowego ma klastrze zawierającym pięć funkcji Hyper-V serwery mają zainstalowany 470 maszyn wirtualnych.

- Różne obciążenia uruchamianie maszyn wirtualnych i mieć włączoną ochroną Odzyskiwanie witryny Azure.

- Miejsca do magazynowania dla węzła jest udostępniany przez iSCSI SAN. Model — Hitachi HUS130.

- Każdy serwer klaster ma cztery sieci kart z jednym GB.

- Dwa kart sieciowych podłączonych do sieci prywatnej iSCSI i dwóch połączonych z sieci zewnętrznej przedsiębiorstwa. Jedną z sieciami zewnętrznymi jest zarezerwowana do tylko komunikacja klaster.

![Wymagania sprzętowe podstawowego](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744922.png)

|Serwer|PAMIĘCI RAM|Model|Procesor|Liczba procesorów|NIC|Oprogramowanie|
|---|---|---|---|---|---|---|
|Serwery funkcji Hyper-V w klastrze: <br />ESTLAB HOST11<br />ESTLAB HOST12<br />ESTLAB HOST13<br />ESTLAB HOST14<br />ESTLAB HOST25|128ESTLAB HOST25 zawiera 256|R820 Dell™ PowerEdge™|Karta Intel(R) Xeon(R) E5 Procesora-4620 0 @ 2,20 GHz|4|Mogę GB x 4|Windows Server centrum danych 2012 R2 (x64) + roli Hyper-V|
|Serwer VMM|2|||2|1 GB|Windows Server bazy danych 2012 R2 (x 64) + VMM 2012 R2|

### <a name="secondary-recovery-site"></a>Witryna pomocniczego (odzyskiwania)

- Pomocnicza witryna zawiera klaster pracy awaryjnej sześć.

- Miejsca do magazynowania dla węzła jest udostępniany przez iSCSI SAN. Model — Hitachi HUS130.

![Specyfikacja podstawowego sprzętu](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744923.png)

|Serwer|PAMIĘCI RAM|Model|Procesor|Liczba procesorów|NIC|Oprogramowanie|
|---|---|---|---|---|---|---|
|Serwery funkcji Hyper-V w klastrze: <br />ESTLAB HOST07<br />ESTLAB HOST08<br />ESTLAB HOST09<br />ESTLAB HOST10|96|R720 Dell™ PowerEdge™|Karta Intel(R) Xeon(R) E5 Procesora-2630 0 @ 2.30GHz|2|Mogę GB x 4|Windows Server centrum danych 2012 R2 (x64) + roli Hyper-V|
|ESTLAB HOST17|128|R820 Dell™ PowerEdge™|Karta Intel(R) Xeon(R) E5 Procesora-4620 0 @ 2,20 GHz|4||Windows Server centrum danych 2012 R2 (x64) + roli Hyper-V|
|ESTLAB HOST24|256|R820 Dell™ PowerEdge™|Karta Intel(R) Xeon(R) E5 Procesora-4620 0 @ 2,20 GHz|2||Windows Server centrum danych 2012 R2 (x64) + roli Hyper-V|
|Serwer VMM|2|||2|1 GB|Windows Server bazy danych 2012 R2 (x 64) + VMM 2012 R2|

### <a name="server-workloads"></a>Obciążenia serwera

- Do celów testowych możemy pobrane obciążenia rzadko używana w przedsiębiorstwie scenariuszy.

- Firma Microsoft za pomocą [IOMeter](http://www.iometer.org) cechy obciążenie pracą podsumowane w tabeli symulacji.

- Wszystkie profile IOMeter są ustawione na pisanie losowe bajtów w celu zasymulowania najgorszych zapisu deseni dla obciążenia.

|Obciążenie pracą|Rozmiar we/wy (KB)|% Programu access|% Odczytu|Wy zaległych|Wzór wejścia/wyjścia|
|---|---|---|---|---|---|
|Serwer plików|48163264|60 20 %5 %5% 10%|80 80% 80% 80% 80%|88888|Wszystkie 100% losowe|
|Program SQL Server (objętość 1) programu SQL Server (objętość 2)|864|100% 100%|70 %0%|88|100% random100% sekwencyjne|
|Exchange|32|100%|67%|8|100% losowe|
|Stacja VDI|464|34 66%|95 70%|11|Przypadkowe obu 100%|
|Serwer plików w sieci Web|4864|33 34% 33%|95 95% 95%|888|Wszystkie 75% losowe|

### <a name="virtual-machine-configuration"></a>Konfiguracja maszyn wirtualnych

- 470 maszyn wirtualnych w klastrze podstawowego.

- Wszystkie maszyn wirtualnych z dyskiem VHDX.

- Maszyn wirtualnych z systemem obciążenia podsumowane w tabeli. Wszystkie zostały utworzone za pomocą Szablony VMM.

|Obciążenie pracą|# Maszyny wirtualne|Minimalna ilość pamięci RAM (GB)|Maksymalna liczba pamięci RAM (GB)|Rozmiar dysku logicznego (GB) na maszyn wirtualnych|Maksymalna liczba operacji i/o na SEKUNDĘ|
|---|---|---|---|---|---|
|Program SQL Server|51|1|4|167|10|
|Exchange Server|71|1|4|552|10|
|Serwer plików|50|1|2|552|22|
|VDI|149|.5|1|80|6|
|Serwer sieci Web|149|.5|1|80|6|
|SUMA|470|||96.83 TB|4108|

### <a name="azure-site-recovery-settings"></a>Azure ustawienia Odzyskiwanie witryny

- Odzyskiwanie witryny Azure został skonfigurowany pod kątem lokalnego do lokalnego ochrony

- Serwer VMM ma cztery chmury skonfigurowane, zawierające serwery klastrów funkcji Hyper-V i ich maszyn wirtualnych.

|Podstawowy VMM w chmurze|Chroniony maszyn wirtualnych w chmurze|Częstotliwość replikacji|Odzyskiwanie dodatkowe punkty|
|---|---|---|---|
|PrimaryCloudRpo15m|142|w ciągu 15 minut|Brak|
|PrimaryCloudRpo30s|47|30 sekund|Brak|
|PrimaryCloudRpo30sArp1|47|30 sekund|1|
|PrimaryCloudRpo5m|235|5 min|Brak|

### <a name="performance-metrics"></a>Wskaźniki

W tabeli przedstawiono wskaźników wydajności i liczniki, które zostały mierzony w ramach wdrożenia.

|Metryka|Licznik|
|---|---|
|PROCESOR|\Processor(_Total)\% czas procesora|
|Dostępną pamięć|\Memory\Available MB|
|OPERACJI I/O NA SEKUNDĘ|Transfer \Disk \PhysicalDisk (_Total) na sekundę|
|Maszyn wirtualnych (operacji i/o na SEKUNDĘ) operacje odczytu/s|Urządzenie magazynu wirtualnego \Hyper-V (<VHD>) \Read operacje na sekundę|
|Operacje zapisu (operacji i/o na SEKUNDĘ) maszyn wirtualnych na sekundę|Urządzenie magazynu wirtualnego \Hyper-V (<VHD>) \Write operacji/S|
|Przeczytaj przepustowość maszyn wirtualnych|Urządzenie magazynu wirtualnego \Hyper-V (<VHD>) \Read bajtów/s|
|Przepustowość zapisu maszyn wirtualnych|Urządzenie magazynu wirtualnego \Hyper-V (<VHD>) \Write bajtów/s|


## <a name="next-steps"></a>Następne kroki

- [Konfigurowanie ochrony między dwiema witrynami VMM lokalnego](site-recovery-vmm-to-vmm.md)
