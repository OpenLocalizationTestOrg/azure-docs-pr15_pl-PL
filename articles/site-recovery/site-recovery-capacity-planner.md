<properties
    pageTitle="Planowanie zdolności do ochrony maszyn wirtualnych i serwerów fizycznych w Odzyskiwanie witryny Azure | Microsoft Azure"
    description="Odzyskiwanie witryny Azure współrzędne replikacji, pracy awaryjnej i odzyskiwanie maszyn wirtualnych i serwerów fizycznych znajdujących się w lokalnej Azure lub do witryny lokalnej pomocniczą." 
    services="site-recovery" 
    documentationCenter="" 
    authors="rayne-wiselman" 
    manager="jwhit" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery" 
    ms.date="07/12/2016" 
    ms.author="raynew"/>

# <a name="plan-capacity-for-protecting-virtual-machines-and-physical-servers-in-azure-site-recovery"></a>Planowanie zdolności do ochrony maszyn wirtualnych i serwerów fizycznych w Odzyskiwanie witryny Azure

Narzędzie do planowania pojemności odzyskiwania witryny Azure ułatwia ustalanie Twojej wymagania dotyczące wydajności dla ochrony maszyny wirtualne funkcji Hyper-V, maszyny wirtualne VMware i systemu Windows i Linux oraz serwerów fizycznych z platformy Azure Odzyskiwanie witryny.


## <a name="overview"></a>Omówienie

Za pomocą planowania pojemności odzyskiwania witryny można analizować środowiska źródłowego i obciążenia i obliczanie wymagania dotyczące przepustowości, zasobów serwera, które będą potrzebne w lokalizacji źródła i zasoby (maszyn wirtualnych i przestrzeni dyskowej itp), które będą potrzebne w lokalizacji docelowej. 

Narzędzie może zostać uruchomiony na kilka trybów:

- **Planowanie szybkie**: Uruchom narzędzie w tym trybie uzyskiwania sieci i serwera prognozy oparte na Średnia liczba maszyny wirtualne, dysków miejsca do magazynowania i Zmienianie kursu.
- **Planowanie szczegółowy**: Uruchom narzędzie w tym trybie i szczegóły na temat poszczególnych obciążenie pracą na poziomie maszyn wirtualnych. Analizowanie zgodności maszyn wirtualnych i uzyskiwanie prognozy sieci i serwera.

## <a name="before-you-start"></a>Przed rozpoczęciem

Przed uruchomieniem narzędzia:

1. Gromadzenie informacji o środowisku, w tym maszyny wirtualne, dysków na Głosowa, miejsca do magazynowania na dysku.
2. Identyfikowanie szybkość zmian (pochodząca) dzienny zreplikowanej danych. Aby to zrobić:

    - Jeśli możesz już replikacji maszyny wirtualne funkcji Hyper-V, a następnie pobierz [Narzędzie planowania pojemności funkcji Hyper-V](https://www.microsoft.com/download/details.aspx?id=39057) uzyskiwania Zmienianie kursu. [Aby uzyskać więcej informacji](site-recovery-capacity-planning-for-hyper-v-replication.md) na temat tego narzędzia. Zalecamy zapoznanie uruchomić to narzędzie w tygodniu, aby przechwycić średnie.
    - Jeśli masz replikacji maszyn wirtualnych VMware, umożliwia [Planowanie urządzenia pojemności vSphere](https://labs.vmware.com/flings/vsphere-replication-capacity-planning-appliance) ustalanie stopy pochodząca.
    - Jeśli masz replikacja fizycznych serwerów, które będą potrzebne do oszacowania ręcznie.

## <a name="run-the-quick-planner"></a>Terminarz Szybkie uruchamianie
1.  Pobierz i Otwórz narzędzie do [Planowania pojemności odzyskiwania witryny Azure](http://aka.ms/asr-capacity-planner-excel) . Musisz uruchomić makro, dlatego umożliwia Włącz edytowanie i zawartości po wyświetleniu monitu. 
2.  W polu **Wybierz typ terminarz** wybierz **Szybkie terminarz** w polu listy.

    ![Wprowadzenie](./media/site-recovery-capacity-planner/getting-started.png)

3.  W arkuszu **Planowania pojemności** wprowadź wymagane informacje. Wypełnij wszystkie pola w czerwonym poniżej ekranu kółku.

    - W polu **Wybierz rozwiązania** wybierz **Funkcji Hyper-V Azure** lub **VMware/fizycznej Azure**.
    - W **Średnie dzienne zmienić częstotliwość (%)** umieścić informacje gromadzenie, [Narzędzie planowania pojemności funkcji Hyper-V](site-recovery-capacity-planning-for-hyper-v-replication.md) lub [Planowanie urządzenia pojemności vSphere](https://labs.vmware.com/flings/vsphere-replication-capacity-planning-appliance).  
    - **Kompresowanie** dotyczy tylko kompresji oferowana, gdy replikacji maszyny wirtualne VMware lub serwerów fizycznych Azure. Firma Microsoft oszacowania 30% lub więcej, ale możesz zmienić to ustawienie, zależnie od potrzeb. Za pomocą urządzenia innych firm takich jak Riverbed, replikacji maszyny wirtualne funkcji Hyper-V do kompresji Azure. 
    -  W **Danych wejściowych przechowywania** Określ, jak długo należy zachować repliki. Jeśli masz replikacji VMware lub serwerów fizycznych wprowadzania wartości w dniach. Jeśli masz replikacji funkcji Hyper-V Określ czas w godzinach.
    -  **Liczba godzin w których replikacji początkowej partii maszyn wirtualnych należy wykonać** i **Liczba maszyn wirtualnych na partię replikacji początkowej** wprowadzania ustawień, które są używane do obliczenia wymagania replikacji początkowej.  Po wdrożeniu Odzyskiwanie witryny należy przesłać cały zestaw danych początkowych. 

    ![Wartości wejściowych](./media/site-recovery-capacity-planner/inputs.png)

2.  Po umieszczeniu zostały wartości środowiska źródłowego, zawiera wyświetlanych wyników:

    - **Wymagane przepustowości dla różnicy replikacji** (MB/s). Przepustowość sieci replikacji różnicy jest obliczana w średniej dziennego kursu zmiany danych.
    - **Wymagane przepustowości dla replikacji początkowej** (MB/s). Przepustowości dla replikacji początkowej jest obliczana w wartości początkowej replikacji, które należy umieścić w. 
    - **Wymagane miejsca do magazynowania (w GB)** jest całkowite miejsce w magazynie Azure, wymagane.
    - **Suma operacji i/o na SEKUNDĘ standardowego magazynu kont** jest obliczana na podstawie rozmiaru jednostki operacji i/o na SEKUNDĘ 8 K kont całkowitą ilość przestrzeni dyskowej standardowy.  Kurs zmienić szybkie terminarz, którą ta liczba jest obliczana na podstawie dysków maszyny wirtualne źródłowych i dzienny danych. Szczegółowe plan, który ta liczba jest obliczana na podstawie całkowitej liczby maszyny wirtualne, które są mapowane na standardowe maszyny wirtualne Azure i danych umożliwia zmianę stawki na tych maszyny wirtualne. 
    - **Liczba kont standardowego magazynu** zawiera łączną liczbę klientów standardowego magazynu konieczne do ochrony maszyny wirtualne. Zauważ, że konto standardowego magazynu może zawierać najwyżej 20 000 operacji i/o na SEKUNDĘ przez wszystkie maszyny wirtualne standardowego magazynu i maksymalna 500 operacji i/o na SEKUNDĘ obsługiwane na dysku. 
    - **Liczba obiektów blob dysków wymagane** zwraca liczbę dysków utworzonych Azure ilość miejsca do magazynowania.
    - **Liczba kont miejsca do magazynowania premium wymagane** zawiera łączną liczbę klientów miejsca do magazynowania premium, potrzebne do ochrony maszyny wirtualne. Należy zauważyć, że źródła maszyn wirtualnych o wysokiej operacji i/o na SEKUNDĘ (większe niż 20000) wymaga konta miejsca do magazynowania premium. Konto miejsca do magazynowania premium może zawierać maksymalnie 80000 operacji i/o na SEKUNDĘ.
    - **Suma operacji i/o na SEKUNDĘ ilość miejsca do magazynowania premium** jest obliczana na podstawie rozmiaru jednostki operacji i/o na SEKUNDĘ 256 KB kont miejsca do magazynowania premium sumy.  Kurs zmienić szybkie terminarz, którą ta liczba jest obliczana na podstawie dysków maszyny wirtualne źródłowych i dzienny danych. Dla szczegółowe Planner liczba jest obliczana na podstawie całkowitą liczbę maszyny wirtualne, które są mapowane na premium Azure maszyn wirtualnych (seria Zasadami i GS) i dane zmienić częstotliwość na tych maszyny wirtualne. 
    - **Liczba serwerów konfiguracji wymagane** zawiera liczbę serwerów konfiguracji są wymagane do wdrożenia (1)
    - **Wymagane liczby serwerów dodatkowych procesów** pokazuje, czy są wymagane oprócz serwera procesu, który jest skonfigurowany na serwerze konfiguracji domyślnie serwery dodatkowych procesów.
    - **100% dodatkowego miejsca do magazynowania w źródle** pokazuje, czy dodatkowego miejsca do magazynowania jest wymagane w lokalizacji źródłowej.
            
    ![Wynik](./media/site-recovery-capacity-planner/output.png)
 
## <a name="run-the-detailed-planner"></a>Uruchamianie szczegółowe terminarz


1.  Pobierz i Otwórz narzędzie do [Planowania pojemności odzyskiwania witryny Azure](http://aka.ms/asr-capacity-planner-excel) . Musisz uruchomić makro, dlatego umożliwia Włącz edytowanie i zawartości po wyświetleniu monitu. 
2.  W polu **Wybierz typ terminarz** wybierz **Terminarz szczegółowe** w polu listy.

    ![Wprowadzenie](./media/site-recovery-capacity-planner/getting-started-2.png)

3.  W arkuszu **Obciążenie pracą kwalifikacji** wprowadź wymagane informacje. Wypełnij wszystkie zaznaczone pola.

    - W **rdzenie** Określ całkowita liczba rdzeni na serwerze źródła.
    - W **Przydzielanie pamięci w Megabajtach** Określ rozmiar pamięci RAM serwera źródłowego. 
    - **Liczba nic** Określ liczbę kart sieciowych na serwerze źródła. 
    -  W **całkowitą ilość przestrzeni dyskowej (w GB)** określić całkowity rozmiar pamięci maszyn wirtualnych. Na przykład jeśli serwer źródłowy ma 3 dyski 500 GB, całkowity rozmiar jest 1500 GB.
    -  W **liczbę dysków dołączonych** Określ całkowitej liczby dysków serwera źródła.
    -  W **wykorzystanie możliwości dysku** Określ średnie wykorzystanie.
    -  W **codziennie zmienić częstotliwość (%)** Określ, czy dzienny danych zmienić częstotliwość serwera źródła.
    -  W polu **rozmiar mapowanie Azure** albo wprowadź rozmiar pamięci Wirtualnej Azure, którą chcesz zamapować. Jeśli nie chcesz zrobić to ręcznie kliknij**Obliczyć maszyny wirtualne IaaS**. Należy zauważyć, że jeśli ustawienie ręcznego wprowadzania, a następnie kliknij pozycję obliczenia maszyny wirtualne IaaS ustawienia ręcznego mogą być zastępowane ponieważ procesu obliczeń automatycznie identyfikuje najlepiej odpowiada na rozmiar pamięci Wirtualnej Azure.

    ![Obciążenie pracą kwalifikacji](./media/site-recovery-capacity-planner/workload-qualification.png)

4.  Kliknięcie przycisku **Obliczyć maszyny wirtualne IaaS** w tym miejscu jest działanie:

    - Sprawdzanie poprawności obowiązkowe danych wejściowych.
    - Oblicza operacji i/o na SEKUNDĘ i proponuje najlepsze maszyn wirtualnych Azure aize dopasowania dla każdej maszyny wirtualne, który jest uprawniony do replikacji Azure. Jeśli nie można wykryć odpowiedni rozmiar maszyn wirtualnych Azure, że jest wydawany komunikat o błędzie. Na przykład jeśli liczbę dyski dołączone w 65 błąd jest problemów od najwyższej rozmiar maszyn wirtualnych Azure to 64.
    - Sugerowanie konta miejsca do magazynowania, który może być używany dla maszyn wirtualnych Azure.
    - Oblicza całkowitą liczbę standardowego magazynu kont i premium miejsca do magazynowania wymagane dla obciążenie pracą. Przewiń w dół po prawej stronie, aby wyświetlić typ magazynu Azure i konta miejsca do magazynowania, który może być używany dla serwera źródłowego
    - Wykonuje i sortowanie pozostałej części tabeli, na podstawie typu wymagane przestrzeni dyskowej (standardowy lub premium) przypisane do maszyny i liczba dyski podłączone. Dla wszystkich maszyny wirtualne, które spełniają wymagania dotyczące wykonywanie kopii zapasowej Azure, kolumny A (jest maszyn wirtualnych kwalifikowanym?) zawiera wartość Tak. Jeśli do otrzymać maszyny Azure błąd jest wyświetlany.

Kolumny AA NL są dane wyjściowe i wpisz informacje dotyczące poszczególnych maszyn wirtualnych.

![Obciążenie pracą kwalifikacji](./media/site-recovery-capacity-planner/workload-qualification-2.png)


### <a name="example"></a>Przykład
Jako przykład, sześć maszyny wirtualne z wartościami wymienione w tabeli Narzędzie oblicza i przypisuje najlepiej odpowiada Azure maszyn wirtualnych i wymagania w zakresie magazynowania Azure.

![Obciążenie pracą kwalifikacji](./media/site-recovery-capacity-planner/workload-qualification-3.png)

- W wyniku kwerendy, na przykład Pamiętaj o następujących kwestiach:
    
    - Pierwsza kolumna zawiera kolumnę sprawdzania poprawności za pośrednictwem SMS, dysków i pochodząca.
    - Dwa konta standardowego magazynu i jednego miejsca do magazynowania nominalnej są potrzebne do pięciu maszyny wirtualne. 
    -  VM3 nie kwalifikuje się do ochrony, ponieważ jeden lub więcej dysków są więcej niż 1 TB.
    -  VM1 i VM2 mogą używać pierwsze konto standardowego magazynu
    -  VM4 można używać drugiego konta standardowego magazynu.
    -  VM5 i VM6 wymagane jest konto miejsca do magazynowania premium i obie użyć jednego konta.

    >[AZURE.NOTE]  Operacji i/o na SEKUNDĘ standardowych i premium ilość miejsca do magazynowania są obliczane na poziomie maszyn wirtualnych, a nie na poziomie dysku. Standardowy maszyna wirtualna może obsługiwać maksymalnie 500 operacji i/o na SEKUNDĘ na dysku. Jeśli wartość jest większa niż 500 operacji i/o na SEKUNDĘ dysku musisz premium miejsca do magazynowania. Jednak jeśli operacji i/o na SEKUNDĘ dysku więcej niż 500 ale operacji i/o na SEKUNDĘ sumy dysków maszyn wirtualnych są w granicach maszyn wirtualnych Azure standardowej pomocy technicznej (rozmiar pamięci Wirtualnej liczbę dysków, liczbę kart, Procesora, pamięci) następnie terminarz wybiera standard maszyn wirtualnych i nie Zasadami lub GS serii. Musisz ręcznie zaktualizować komórki Azure size mapowania serią odpowiednie Zasadami lub GS maszyn wirtualnych.

5. Po wszystkie szczegóły w miejscu, kliknij przycisk **Prześlij dane do narzędzi planowania** , aby otworzyć **Planowania pojemności** obciążenia są wyróżniony wskazuje, czy są one kwalifikuje się do ochrony lub nie.


### <a name="submit-data-in-the-capacity-planner"></a>Przesyłanie danych w module planowania pojemności

1.  Podczas otwierania arkusza **Planowania pojemności** jest pusta, zgodnie z ustawieniami określonymi przez użytkownika. Wyraz "Obciążenie pracą" pojawia się w komórce **źródła danych wejściowych Infra** wskazujące wprowadzania danych w arkuszu **Obciążenie pracą kwalifikacji** . 
2.  Jeśli chcesz wprowadzić zmiany, które będą potrzebne do modyfikowania arkusza **Kwalifikacji obciążenie pracą** i ponownie kliknij pozycję przesyłanie danych do narzędzia terminarz.  

    ![Narzędzie do planowania pojemności](./media/site-recovery-capacity-planner/capacity-planner.png)


