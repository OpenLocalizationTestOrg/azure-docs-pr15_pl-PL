<properties 
   pageTitle="Przyspieszona sieć w maszyny wirtualnej - portalu | Microsoft Azure"
   description="Dowiedz się, jak skonfigurować przyspieszona Networking Azure maszyn wirtualnych za pomocą portalu Azure."
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/26/2016"
   ms.author="jdial" />

# <a name="accelerated-networking-for-a-virtual-machine"></a>Przyspieszona sieci dla maszyny wirtualnej

> [AZURE.SELECTOR]
- [Azure Portal](virtual-network-accelerated-networking-portal.md)
- [Programu PowerShell](virtual-network-accelerated-networking-powershell.md)

Przyspieszona sieci umożliwia jednym głównym we/wy wirtualizacji (SR IOV) maszyn wirtualnych (Głosowa), znacznie poprawia wydajności sieci. Ta ścieżka wysokiej wydajności pomija hosta z ścieżki danych, zmniejszenie opóźnienia, zakłócenia i użycie Procesora przez najbardziej wymagających obciążeń sieci na obsługiwane typy maszyn wirtualnych. W tym artykule opisano sposób konfigurowania przyspieszona sieć w modelu wdrożenia Menedżera zasobów Azure za pomocą Azure Portal. Można także tworzyć maszyny z przyspieszona sieci przy użyciu programu PowerShell Azure. Aby dowiedzieć się, jak to zrobić, kliknij pole programu PowerShell na początku tego artykułu.

Na poniższej ilustracji przedstawiono komunikacji między dwoma maszyn wirtualnych (maszyn wirtualnych) z i bez przyspieszona sieci:

![Porównanie](./media/virtual-network-accelerated-networking-portal/image1.png)

Bez przyspieszona sieci wszystkich ruchu sieciowego i maszyn wirtualnych muszą przejść hosta i przełącznika wirtualną. Wirtualny przełącznik zawiera wszystkie wymuszania zasad, takich jak grupy zabezpieczeń sieci, listy kontroli dostępu, izolacji i innych usług sieci z obsługą ruchu sieciowego. Aby dowiedzieć się więcej, przeczytaj artykuł [wirtualizacji sieci funkcji Hyper-V i wirtualny przełącznik](https://technet.microsoft.com/library/jj945275.aspx) .

Przyspieszona sieciowych ruch sieciowy nadejścia karty sieciowej (NIC) i następnie przekazywana maszyn wirtualnych. Wszystkie zasady sieci stosowanych wirtualny przełącznik bez przyspieszona sieć są przekazywać i sprzętu. Stosowanie zasad sprzętu umożliwia NIC do przodu ruchu sieciowego bezpośrednio do Głosowa, pomijanie hosta i wirtualny przełącznik przy zachowaniu zasady, które on zastosowany w polu host.

Zalety przyspieszona sieć są stosowane tylko do maszyn wirtualnych, który jest włączone. Aby uzyskać najlepsze wyniki jest idealnym rozwiązaniem włączyć tę funkcję, na co najmniej dwa maszyny wirtualne połączony z tym samym VNet. Podczas komunikacji za pośrednictwem VNets lub łączących lokalnego, ta funkcja ma minimalny wpływ na ogólną opóźnienie.

[AZURE.INCLUDE [virtual-network-preview](../../includes/virtual-network-preview.md)]

##<a name="benefits"></a>Zalety

- **Mniejsze opóźnienia / wyższą pakietów na sekundę (pps):** Odebranie wirtualny przełącznik ścieżki danych usuwa razem, gdy poświęcają pakietów hosta przetwarzanie zasad i zwiększanie liczby pakietów, które mogą być przetwarzane wewnątrz maszyn wirtualnych.
- **Zmniejszenia zakłócenia:** Przetwarzanie wirtualny przełącznik zależy od ilości zasady, które należy zastosować i obciążenie Procesora robi przetwarzania. Odciążanie wymuszania zasad sprzętu usuwa tego zmienności dostarczania pakietów bezpośrednio do Głosowa, usuwając hosta maszyn wirtualnych komunikacji wszystkich przerwania oprogramowania i przełączniki kontekstu.
- **Zmniejszyła się procesora:** Pomijanie wirtualny przełącznik na hoście prowadzi do mniej procesora przetwarzania ruchu sieciowego.

## <a name="limitations"></a>Ograniczenia

Następujące ograniczenia istnieją podczas korzystania z tej funkcji:
 
- **Sieci tworzenia interfejsu:** Przyspieszona sieci można włączyć tylko dla nowego interfejsu sieciowego.  Nie można włączyć dla istniejącego interfejsu sieci.
- **Tworzenie maszyn wirtualnych:** Karty sieciowej z włączoną obsługą przyspieszona sieci tylko może zostać dołączony do maszyny po utworzeniu maszyn wirtualnych. Interfejs sieciowy nie zostaną dołączone do istniejącego maszyn wirtualnych.
- **Regionów:** Oferowane w tylko regionów zachód centralnej nam oraz Azure Europe Zachód. Ustawianie regionów zostaną rozszerzone w przyszłości.
- **System operacyjny obsługiwane:** Microsoft Windows Server 2012 R2 i Windows Server 2016 Technical Preview 5. Wkrótce zostanie dodany Linux i systemu Windows Server 2012 pomocy technicznej.
- **Rozmiar pamięci Wirtualnej:** Standard_D15_v2 i Standard_DS15_v2 są obsługiwane tylko rozmiarów wystąpienie maszyn wirtualnych. Aby uzyskać więcej informacji zobacz artykuł [rozmiarów maszyn wirtualnych systemu Windows](../virtual-machines/virtual-machines-windows-sizes.md) . Zestaw obsługiwane rozmiary wystąpienie maszyn wirtualnych zostaną rozszerzone w przyszłości.

Zmiany te ograniczenia zostanie ogłoszone za pośrednictwem strony [Azure wirtualna sieć — aktualizacje](https://azure.microsoft.com/updates/accelerated-networking-in-preview) .

## <a name="create-a-windows-vm-with-accelerated-networking"></a>Tworzenie maszyn wirtualnych systemu Windows z sieci przyspieszona

1. Zarejestruj się w usłudze podglądu, wysyłając wiadomość e-mail do [Przyspieszona subskrypcje sieci](mailto:axnpreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e) przy użyciu swojego Identyfikatora subskrypcji i przeznaczenie. Nie wykonaj pozostałe kroki do momentu, gdy otrzymasz wiadomość e-mail z powiadomieniem, zostały już zaakceptowane do podglądu.
2. Zaloguj się do portalu Azure pod adresem http://portal.azure.com.
3. Utwórz maszyny, wykonując kroki opisane w artykule [Tworzenie maszyn wirtualnych systemu Windows](../virtual-machines/virtual-machines-windows-hero-tutorial.md) , wybranie opcji:
    - Wybierz pozycję system operacyjny wymienionych w sekcji ograniczenia tego artykułu.
    - Wybierz lokalizację (region) wymienionych w sekcji ograniczenia tego artykułu.
    - Wybierz rozmiar maszyn wirtualnych wymienionych w sekcji ograniczenia tego artykułu. Jeśli jeden z obsługiwanych rozmiarów nie ma na liście, kliknij pozycję **Wyświetl wszystkie** w karta **Wybierz rozmiar** , aby wybrać odpowiedni rozmiar z rozszerzonej listy.
    - W karta **Ustawienia** kliknij pozycję *włączone* **akcelerowanego sieci**, jak pokazano na poniższej ilustracji:

        ![Ustawienia](./media/virtual-network-accelerated-networking-portal/image3.png)

    >[AZURE.NOTE] Opcja przyspieszona sieci będą widoczne tylko jeśli Wykonano następujące czynności:
    >
    >- Przyjęciu do podglądu
    >- Zaznaczone obsługiwanego systemu operacyjnego, lokalizacji i rozmiarów maszyn wirtualnych wymienionych w sekcji ograniczenia tego artykułu.

5. Po utworzeniu maszyn wirtualnych pobrać [sterownik przyspieszona sieć](https://gallery.technet.microsoft.com/Azure-Accelerated-471b5d84), łączenie i logowanie do maszyn wirtualnych i uruchom Instalatora pakietu sterowników wewnątrz maszyn wirtualnych.
6. Kliknij prawym przyciskiem myszy przycisk systemu Windows, a następnie kliknij pozycję **Menedżer urządzeń**. Sprawdź, czy **Karta Ethernet funkcja wirtualna Mellanox ConnectX-3** jest wyświetlana w obszarze opcji **sieci** po rozwinięciu, jak pokazano na poniższej ilustracji:

    ![Menedżer zadań](./media/virtual-network-accelerated-networking-portal/image2.png)