<properties 
   pageTitle="Monitorowanie urządzenia StorSimple | Microsoft Azure"
   description="Informacje dotyczące używania usługi Menedżera StorSimple monitorowanie wydajności, wykorzystania możliwości, przepustowość sieci i wydajności urządzenia We/Wy."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/16/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-monitor-your-storsimple-device"></a>Monitorowanie urządzenia StorSimple za pomocą usługi Menedżera StorSimple 

## <a name="overview"></a>Omówienie

Za pomocą usługi Menedżera StorSimple monitorowanie określonych urządzeń w rozwiązaniu StorSimple. Możesz utworzyć niestandardowe wykresy oparte na we/wy wydajności, wykorzystania możliwości przepustowość sieci i wskaźniki urządzenia. 

Aby wyświetlić monitorowania informacje dotyczące konkretnego urządzenia, w portalu klasyczny Azure, wybierz usługę Menedżer StorSimple. Kliknij kartę **Monitor** , a następnie wybierz z listy urządzeń. Strona **Monitor** zawiera następujące informacje.

## <a name="io-performance"></a>Wydajność wejścia/wyjścia 

**Wydajność wejścia/wyjścia** ścieżki metryki powiązanych z liczbą odczytu i pisanie operacje między albo interfejsów inicjator iSCSI na serwerze hosta i urządzenie lub urządzenie i chmury. Ten wydajności można zmierzyć woluminu określonego, kontenera głośność określonych lub wszystkich kontenerów głośność.

Wykres poniżej przedstawia we/wy dla inicjator urządzenia w celu wielkości urządzenia produkcji. Metryki przedstawić czytane i pisanie bajtów na sekundę, czytanie i pisanie Jo operacji na sekundę, czytać i zapisywać opóźnienia.

![Wydajność Jo z inicjator do urządzenia](./media/storsimple-monitor-device/StorSimple_IO_Performance_For_InitiatorTODevice_For_AllVolumesM.png)

Dla tego samego urządzenia operacji We/Wy są wykreślane dane z urządzenia w chmurze dla wszystkich kontenerów głośność. Na tym urządzeniu dane są tylko w warstwie liniowy i nic się nie ma rozrzucone w chmurze. Nie istnieją żadne operacje odczytu i zapisu występujące z urządzenia w chmurze. Dlatego wartości na wykresie są co 5 minut odpowiadający częstotliwość, w którym weryfikowania jest zaznaczone pole wyboru między urządzeniem i usługi. 

![Wydajność Jo z urządzenia do chmury](./media/storsimple-monitor-device/StorSimple_IO_Performance_For_DeviceTOCloud_For_AllVolumeContainersM.png)


Dla tego samego urządzenia migawkę chmury wykonano dla danych objętości, zaczynając od 2:00 pm. Wynikiem danych wynikających z urządzenia w chmurze. Odczyt zapis były obsługiwane w chmurze w tym czasie trwania. Wykres Jo pokazuje Szczyt różnych metryki odpowiadające do momentu, gdy wykonano migawki. 

![Wydajność Jo urządzenia do chmury po migawkę chmury](./media/storsimple-monitor-device/StorSimple_IO_Performance_For_DeviceTOCloud_For_AllVolumeContainers2M.png)


## <a name="capacity-utilization"></a>Wykorzystanie wydajności 

**Wykorzystanie możliwości** śledzi metryki ilości miejsca na przechowywanie danych jest używana przez wielkości, kontenerów głośność lub urządzenia. Możesz utworzyć raportów opartych na wykorzystania możliwości podstawowy magazyn, magazynu w chmurze lub magazynie urządzenia. Wykorzystanie pojemności można zmierzyć woluminu określonego, kontenera określonego woluminu lub wszystkich kontenerów głośność.


Podstawowe chmury i pojemność urządzenia można opisać w następujący sposób:

###<a name="primary-storage-capacity-utilization"></a>Wykorzystanie możliwości podstawowego magazynu
 
Te wykresy wyświetlanie ilości danych zapisywane wielkości StorSimple przed dane są deduplicated i skompresowany. Przez wszystkich wielkości lub dla pojedynczego woluminu, można wyświetlać wykorzystanie magazynu podstawowego.

Podczas przeglądania wykresy podstawowy głośność możliwości wykorzystania dla wszystkich wielkości i każdej wielkości poszczególnych i sumować dane podstawowe w obu przypadkach, może być niezgodność między dwoma liczbami. Suma dane podstawowe ilości wszystkich nie można dodać do całkowita suma danych pierwotnych wielkości poszczególnych. Może to być spowodowane jedną z następujących czynności:

- **Dostępne dla wszystkich ilości danych migawki**: to zachowanie jest widoczny tylko wtedy, gdy używasz wersji wcześniejszej niż aktualizacji 3. Podstawowe dane wyświetlane dla wszystkich wielkość jest sumą podstawowego dane dla każdego woluminu i migawkę. Podstawowe dane wyświetlane dla danego woluminu odpowiada tylko ilości danych na wielkość (i nie zawiera odpowiednie dane migawki woluminu).

    To można także wyjaśnić następującego równania:

    *Dane podstawowe (wszystkie objętości) = Suma (podstawowy danych (objętość i) + rozmiar migawkę danych (objętość i))*
    
    *Jeżeli dane podstawowe (objętość i) = rozmiar danych pierwotnych przydzielone do woluminu i*
 
    Usunięcie migawki za pośrednictwem usługi usunięcie odbywa się asynchroniczne w tle. Może upłynąć trochę czasu, rozmiar woluminu dane mają być aktualizowane po usunięcia migawki. 

    Jeśli z aktualizacji 3 lub nowszej, następnie dane migawki jest niedostępna w danych głośność. I wykorzystania podstawowego jest obliczana w następujący sposób:

    * Podstawowego danych (wszystkie objętości) = Suma (podstawowy danych (objętość i)
    
    *Jeżeli dane podstawowe (objętość i) = rozmiar danych pierwotnych przydzielone do woluminu i*
 
- **Wielkości monitorowanie wyłączone uwzględnione wszystkie wielkości**: Jeśli masz wielkości na urządzeniu, dla którego monitorowania jest wyłączona, monitorowania dane dla tych wielkości poszczególnych nie będą dostępne na wykresach. Jednak dane dla wszystkich wielkości na wykresie zawiera wielkość, dla których monitorowanie jest wyłączona. 
 
- **Usunięte wielkości z live skojarzony kopie zapasowe dostępny dla wszystkich wielkości**: Jeśli ilości danymi migawki są usuwane, ale skojarzone migawki nadal istnieje, a następnie może zostać wyświetlony niezgodność.

- **Usunięte ilości dostępne dla wszystkich wielkości**: W niektórych przypadkach stare wielkości może istnieć, mimo że te zostały usunięte. Efekt usunięcia nie jest widoczny i urządzenie może wyświetlić dolnym pojemność. Należy skontaktować się Microsoft Support usunąć tych wielkości.

Następujące wykresy Pokaż wykorzystania możliwości podstawowy urządzenia StorSimple przed i po migawkę chmury. Jak to tylko obrót dane migawki chmury nie należy zmieniać podstawową pamięć. Jak widać, wykres pokazuje różnicy w wykorzystania możliwości podstawowego wyniku migawki chmury. Migawka chmury uruchomić około 2:00 pm na tym urządzeniu.

![Wykorzystanie możliwości podstawowego przed migawkę chmury](./media/storsimple-monitor-device/StorSimple_PrimaryCapacityUtil_For_AllVolumes2M.png)

![Wykorzystanie możliwości podstawowego po migawkę chmury](./media/storsimple-monitor-device/StorSimple_PrimaryCapacityUtil_For_AllVolumes1M.png)

Jeśli używasz Aktualizacja 2 lub nowszym, możesz można podział wykorzystania możliwości podstawowy według poszczególnych głośność, wszystkie ilości wszystkich wielkości warstwowych i wszystkich wielkości lokalnie przypięty tak jak pokazano poniżej. Podziału przez wszystkich wielkości lokalnie przypięty pozwoli szybko ustalić, ile lokalnych warstwa jest używana w górę.

![Wykorzystanie możliwości podstawowy dla wszystkich wielkości lokalnie przypięty](./media/storsimple-monitor-device/localvolumes.png)


###<a name="cloud-storage-capacity-utilization"></a>Wykorzystanie możliwości magazynu chmury

Te wykresy pokazują używane magazynu w chmurze. Te dane są deduplicated i skompresowany. Kwota ta zawiera migawek chmury, które mogą zawierać dane, które nie jest uwzględniana w dowolnym woluminu podstawowego i są przechowywane na potrzeby przechowywania starszych lub wymagane. Można porównać podstawowych i chmury dane zużycie magazynu, aby zorientować szybkości redukcji danych, mimo że numer nie będą dokładne. Wykorzystanie możliwości magazynu chmury urządzenia StorSimple przed i po migawkę chmury wykresach. Migawki chmury uruchomić około 2:00 pm na tym urządzeniu i można zobaczyć wykorzystania możliwości chmury zrzut w tym samym czasie, zwiększając od 5.73 MB 4.04 GB.

![Wykorzystania możliwości chmurze przed migawkę chmury](./media/storsimple-monitor-device/StorSimple_CloudCapacityUtil_For_AllVolumeContainers2M.png)

![Chmury wykorzystania możliwości po migawkę chmury](./media/storsimple-monitor-device/StorSimple_CloudCapacityUtil_For_AllVolumeContainers1M.png)


###<a name="device-storage-capacity-utilization"></a>Wykorzystanie możliwości magazynu urządzenia

Te wykresy pokazują całkowitego wykorzystania urządzenia, które będą użycia więcej niż podstawowy magazynowania, ponieważ zawiera on warstwie liniowej SSD. Ten poziom zawiera ilości danych, która również występuje na urządzeniu jest innych warstw. Wydajność w warstwie liniowej SSD ponownym włączeniu tak, aby po nadejściu nowych danych starych danych jest przenoszony do poziomu dysk twardy (co jest deduplicated i skompresowany), a następnie w chmurze.

Czasem wykorzystania możliwości podstawowego i wykorzystania możliwości urządzenia prawdopodobnie zwiększy razem do momentu rozpoczęcia dane do być tiered w chmurze. W tym momencie wykorzystania możliwości urządzenia prawdopodobnie rozpocznie płaska, ale wykorzystania możliwości podstawowego zwiększa podczas zapisywania większej ilości danych.

Następujące wykresy Pokaż wykorzystania możliwości podstawowy urządzenia StorSimple przed i po migawkę chmury. Migawka chmury zaczęła na 2:00 pm, a wykorzystania możliwości urządzenia zmniejszenie w tym czasie. Wykorzystanie możliwości magazynu urządzenia zakończył działanie od 11.58 GB do 7.48 GB. To wskazuje, że prawdopodobnie nieskompresowane dane w warstwie SSD liniowej została deduplicated, skompresowany i przeniesione do poziomu dysk twardy. Należy zauważyć, że jeśli urządzenie już dużych ilości danych w poziomów SSD i dysk twardy, mogą nie być widoczne spadek. W tym przykładzie urządzenie ma niewielkiej ilości danych.

![Wykorzystanie możliwości urządzenia przed migawkę chmury](./media/storsimple-monitor-device/StorSimple_DeviceCapacityUtil2M.png)

![Wykorzystanie możliwości urządzenia po migawkę chmury](./media/storsimple-monitor-device/StorSimple_DeviceCapacityUtil1M.png)


## <a name="network-throughput"></a>Przepustowość sieci

**Przepustowość sieci** śledzi metryki dotyczące ilości danych, przeniesione z interfejsów sieciowych inicjator iSCSI na serwerze hosta i urządzenia i między urządzeniem a chmury. Można monitorować tej metryki dla każdego z interfejsów sieciowych iSCSI na urządzeniu.

Następujące wykresy pokazują przepustowość sieci danych 0 i 4 danych obu 1 GbE interfejsów na urządzeniu. W takim przypadku danych 0 cloud włączono 4 danych został włączony iSCSI. Można wyświetlić przychodzącego i ruchu wychodzącego dla urządzenia StorSimple. Z powodu prostym linii na wykresie, rozpoczynając od 3:24 pm jest fakt, że firma Microsoft zbierania danych tylko na 5 minut i należy je zignorować. 

![Przepustowość sieci dla Data4](./media/storsimple-monitor-device/StorSimple_NetworkThroughput_Data0M.png)

![Przepustowość sieci dla Data4](./media/storsimple-monitor-device/StorSimple_NetworkThroughput_Data4M.png)


## <a name="device-performance"></a>Wydajność urządzenia 

**Wydajność urządzenia** śledzi metryki związanych z wydajnością urządzenia. Poniżej przedstawiono statystykę wykorzystania Procesora dla urządzenia w produkcji.

![Użycie Procesora na urządzeniu](./media/storsimple-monitor-device/StorSimple_DeviceMonitor_DevicePerformance1M.png)

## <a name="next-steps"></a>Następne kroki

- Dowiedz się, jak [używać pulpit nawigacyjny urządzenia usługi Menedżera StorSimple](storsimple-device-dashboard.md).

- Dowiedz się, jak [używać usługi Menedżera StorSimple administrowania urządzenia StorSimple](storsimple-manager-service-administration.md).
