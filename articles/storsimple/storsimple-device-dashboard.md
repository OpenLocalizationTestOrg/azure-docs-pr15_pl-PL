<properties
   pageTitle="Za pomocą pulpitu nawigacyjnego urządzenia Menedżera StorSimple | Microsoft Azure"
   description="W tym artykule opisano pulpit nawigacyjny urządzenia usługi Menedżera StorSimple i jak go używać do wyświetlania metryki magazynowania i inicjator połączonego i Znajdź numer seryjny i IQN."
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
   ms.date="09/21/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-device-dashboard"></a>Za pomocą pulpitu nawigacyjnego urządzenia Menedżera StorSimple

## <a name="overview"></a>Omówienie

Pulpit nawigacyjny urządzenia Menedżera StorSimple zawiera przegląd informacji dla konkretnego urządzenia StorSimple, w odróżnieniu od pulpitu nawigacyjnego usługa, która zapewnia informacje o wszystkich urządzeń zawarte w rozwiązaniu Microsoft Azure StorSimple.

![Strony pulpitu nawigacyjnego urządzenia](./media/storsimple-device-dashboard/StorSimple_DeviceDashbaord1M.png)

Pulpit nawigacyjny zawiera następujące informacje:

- **Obszar wykresu** — będziesz widzieć metryki odpowiednie miejsca do magazynowania w obszarze wykresu u góry pulpitu nawigacyjnego. Na tym wykresie możesz wyświetlić metryki dla magazynu w chmurze sumy zużywanej przez urządzenia w okresie i sumy magazynowania podstawowego (zakres danych, napisane przez hosty do urządzenia).

     W tym kontekście *podstawowy* odwołuje się do łączną liczbę zapisanych przez hosta danych i mogą być podzielone według typu głośność: *podstawową pamięć warstwowych* zawiera zarówno danych przechowywanych lokalnie i tiered danych w chmurze; *podstawowy lokalnie przypięte miejsca do magazynowania* zawiera tylko dane zgromadzone. *Magazynu w chmurze*, z drugiej strony, to wartość łączną liczbę dane przechowywane w chmurze. Ta opcja uwzględnia warstwowych danych i wykonywania kopii zapasowych. Zauważ, że dane przechowywane w chmurze jest deduplicated i skompresowane, podstawowy wskazuje ilość miejsca w magazynie używany przed dane są deduplicated i skompresowany. (Porównanie tych dwóch liczb, aby uzyskać ogólny obraz tego kursu kompresji). Dla obu podstawowego i magazynu w chmurze, kwoty wyświetlane będą oparte na częstotliwość śledzenia można skonfigurować. Na przykład jeśli wybierzesz częstotliwości tydzień, następnie wykresu będą wyświetlane dane dla każdego dnia w poprzednim tygodniu.

     Wykres można skonfigurować następująco:

     - Aby wyświetlić ilość zużyta w czasie magazynu w chmurze, wybierz opcję **Używane miejsca do magazynowania w CHMURZE** . Aby wyświetlić całkowitą ilość przestrzeni dyskowej, który został zapisany przez hosta, wybierz odpowiednie opcje **Podstawowego TIERED miejsca do magazynowania używane** , a **podstawowego LOKALNIE PRZYPIĘTE miejsca do magazynowania** . Na ilustracji zostaną zaznaczone obie opcje; Dlatego wykres pokazuje ilości miejsca do magazynowania zarówno w chmurze, jak i podstawowy. Należy zauważyć, że wszelkie magazynu głównego używany przed zainstalowaniem aktualizacji 2 jest reprezentowany przez wiersz **Podstawowego TIERED miejsca do magazynowania użycia** .
     - Aby określić przedział czasu 1 tydzień, 1 miesiąc, 3 miesięcy lub lat 1 za pomocą menu rozwijanego w prawym górnym rogu wykresu. Zauważ, że najwyższego poziomu wykresu są odświeżane tylko jeden raz dziennie i w związku z tym zostaną zastosowane sumy poprzedniego dnia.

     Aby uzyskać więcej informacji zobacz [Używanie usługi Menedżera StorSimple monitorowanie urządzenia StorSimple](storsimple-monitor-device.md).

- **Omówienie zastosowania** — w obszarze **Przegląd zastosowania** widać ilość podstawowego zajęte, ilość miejsca do magazynowania ustanawianie i maksymalna pojemność dla danego urządzenia. Porównując te liczby zastosowania maksymalnej ilości miejsca do magazynowania dostępnej widać na pierwszy rzut Jeśli musisz uzyskać dodatkowe miejsce do magazynowania. Warto zauważyć, że to omówienie jest aktualizowana co 15 minut, ze względu na różnicę w częstotliwość aktualizowania mogą znajdować się różną niż wyświetlana na powyższych obszar wykresu, który jest aktualizowany codziennie. Aby uzyskać więcej informacji zobacz [Używanie usługi Menedżera StorSimple monitorowanie urządzenia StorSimple](storsimple-monitor-device.md).


- **Alerty** — obszaru **alerty** zawiera zestawienie alertów dla swojego urządzenia. Liczba są udostępniane liczby alertów na wszystkich poziomach ważności i alerty są pogrupowane według ważności. Kliknięcie przycisku ważności alertu otwarcia widoku zakresu kartę alerty, aby pokazać tylko alerty ten poziom ważności, w przypadku tego urządzenia.

- **Zadania** — obszar **zadania** zawiera wyników ostatniej aktywności zadania. To może Ci upewnić się, że system działa zgodnie z oczekiwaniami, lub go można informujący o konieczności podejmowania działań naprawczych. Aby wyświetlić więcej informacji na temat ostatnio ukończonych zadań, kliknij pozycję **zadania zakończyła się pomyślnie w ciągu ostatnich 24 godzin**.

- Obszar **Szybkie rzut** po prawej stronie pulpit nawigacyjny zawiera przydatne informacje, takie jak modelu urządzenia, numer seryjny, stan, opis i numer wielkości.

Można również skonfigurować pracy awaryjnej i wyświetlać inicjator połączonego z pulpitu nawigacyjnego urządzenia.

Typowe zadania, które mogą być wykonywane na tej stronie są:

- Wyświetlanie połączonego inicjator

- Znajdź numer seryjny urządzenia

- Znajdź element docelowy urządzenia IQN

## <a name="view-connected-initiators"></a>Wyświetlanie połączonego inicjator

Możesz wyświetlić inicjatorach, połączone z urządzeniem, klikając pozycję **Widok połączony inicjator** łącza podanego w obszarze **Szybkie rzut** pulpitu nawigacyjnego urządzenia. Ta strona zawiera Tabelaryczny spis inicjator pomyślnym podłączeniu urządzenia. Dla każdego inicjator można wyświetlić:

- ISCSI kwalifikowana nazwa (IQN): Inicjator połączonego.

- Nazwa rekordu kontroli dostępu (awaryjnego), który umożliwia ten połączonego inicjator.

- Adres IP inicjator połączonego.

- Interfejsy sieciowe połączone ze inicjator na urządzeniu magazynowania. Te zakresu danych 0-5 danych.

- Wszystkie wielkości, które jest połączone inicjator mają dostępu według bieżącej konfiguracji awaryjnego.

Jeśli widzisz nieoczekiwane inicjator na tej liście, lub nie widzisz oczekiwaniami, przejrzyj konfiguracji awaryjnego. Maksymalnie 512 inicjator można połączyć się z urządzeniem.

## <a name="find-the-device-serial-number"></a>Znajdź numer seryjny urządzenia

Po skonfigurowaniu / wielościeżkowego (MPIO) firmy Microsoft na tym urządzeniu, może być konieczne numer seryjny urządzenia. Wykonaj poniższe czynności, aby znaleźć numer seryjny urządzenia.

#### <a name="to-find-the-device-serial-number"></a>Aby znaleźć numer seryjny urządzenia

1. Przejdź do pozycji **urządzenia** > **pulpitu nawigacyjnego**.

2. W okienku po prawej stronie pulpitu nawigacyjnego Znajdź obszar **Szybkie rzut** .

3. Przewiń listę i Znajdź numer seryjny.

## <a name="find-the-device-target-iqn"></a>Znajdź element docelowy urządzenia IQN

Po skonfigurowaniu uwierzytelniania Protocol (CHAP) na urządzeniu StorSimple, może być konieczne docelowej urządzenia IQN. Wykonaj poniższe czynności, aby znaleźć docelowej urządzenia IQN.

### <a name="to-find-the-device-target-iqn"></a>Aby znaleźć docelowej urządzenia IQN

1. Przejdź do pozycji **urządzenia** > **pulpitu nawigacyjnego**.

1. W okienku po prawej stronie pulpitu nawigacyjnego Znajdź obszar **Szybkie rzut** .

1. Przewiń listę i Znajdź docelowej IQN.

## <a name="next-steps"></a>Następne kroki

- Dowiedz się więcej na temat [Menedżera StorSimple pulpit nawigacyjny usługi](storsimple-service-dashboard.md).
- Dowiedz się więcej o [korzystaniu z usługi Menedżera StorSimple administrowania urządzenia StorSimple](storsimple-manager-service-administration.md).
