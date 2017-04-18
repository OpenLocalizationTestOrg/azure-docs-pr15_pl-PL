<properties 
   pageTitle="Konfigurowanie bramy sieci VPN w portalu klasyczny Azure | Microsoft Azure"
   description="W tym artykule opisano podczas konfigurowania wirtualnych sieci VPN bramy i zmieniania bramy typ routingu VPN."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/11/2016"
   ms.author="cherylmc" />

# <a name="configure-a-vpn-gateway-for-the-classic-deployment-model"></a>Brama VPN model klasyczny wdrażania


Jeśli chcesz utworzyć połączenie bezpiecznego lokalnej infrastrukturze między Azure i lokalizacji lokalnego, musisz skonfigurować połączenie VPN bramy. W modelu Klasyczny wdrożenia bramy może być jeden z dwóch typów routingu VPN: statyczna lub dynamiczna. Wybranego typu zależy od planu projektu sieci i urządzenie VPN lokalnego, którego chcesz użyć. 

Na przykład niektóre opcje połączenia, takie jak połączenia punkt do witryny wymagają dynamicznego routingu bramy. Jeśli chcesz brama do obsługi połączeń punktu do witryny (P2S) i połączenia (S2S) witryny do witryny, musisz skonfigurować bramę routingu dynamicznego, mimo że można skonfigurować witryny do witryny albo bramy typ routingu VPN. 

Ponadto należy się upewnić, że urządzenie, którego chcesz użyć dla połączenia obsługuje typ routingu VPN, który chcesz utworzyć. Zobacz [informacje o urządzeniach sieci VPN](vpn-gateway-about-vpn-devices.md).


**Informacje w tym artykule** 

Ten artykuł został napisany dla modelu Klasyczny wdrażania za pomocą [portalu klasyczny](https://manage.windowsazure.com) (nie portal Azure). 

**Informacje dotyczące modeli Azure wdrażania**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="configuration-overview"></a>Omówienie konfiguracji

Poniższe kroki szczegółową Konfigurowanie bramy sieci VPN, w portalu klasyczny Azure. Te kroki dotyczą usługi bram dla sieci wirtualne, które zostały utworzone przy użyciu modelu Klasyczny wdrożenia. Obecnie nie wszystkie ustawienia konfiguracji bramy są dostępne w portalu Azure. Gdy przekraczają one zostanie utworzony nowy zestaw instrukcji, które dotyczą Azure portal.


1. [Tworzenie bramy sieci VPN dla swojego VNet](#create-a-vpn-gateway)

1. [Gromadzenie informacji o konfiguracji urządzenia VPN](#gather-information-for-your-vpn-device-configuration)

1. [Konfigurowanie urządzenia VPN](#configure-your-vpn-device)

1. [Sprawdź zakresy sieci lokalnej i adres IP bramy sieci VPN](#verify-your-local-network-ranges-and-vpn-gateway-ip-address)

### <a name="before-you-begin"></a>Przed rozpoczęciem

Przed skonfigurowaniem usługi bramy, najpierw należy utworzyć wirtualnej sieci. Aby uzyskać procedury tworzenia wirtualnej sieci lokalnej krzyżowe łączności, zobacz [Konfigurowanie sieci wirtualnej się przez połączenie VPN witryny do witryny](vpn-gateway-site-to-site-create.md)lub [Konfiguruj wirtualną sieć się przez połączenie VPN punktu do witryny](vpn-gateway-point-to-site-create.md). Następnie wykonaj następujące czynności, aby skonfigurować bramę VPN i zbieranie informacji potrzebnych do skonfigurowania urządzenia VPN. 

Jeśli masz już bramą VPN i chcesz zmienić typ routingu VPN, zobacz [jak zmienić typ routingu VPN z bramy](#how-to-change-the-vpn-routing-type-for-your-gateway).

## <a name="create-a-vpn-gateway"></a>Tworzenie bramy sieci VPN

1. W [portalu klasyczny Azure](https://manage.windowsazure.com)na stronie **sieci** Zweryfikuj w kolumnie Stan dla wirtualnych sieci **utworzone**.

1. W kolumnie **Nazwa** kliknij nazwę wirtualnej sieci.

1. Na stronie **pulpitu nawigacyjnego** Zwróć uwagę, że ten VNet nie ma jeszcze skonfigurowane bramy. Zobaczysz ten status, podczas wykonywania kroków czynności, aby skonfigurować funkcję bramy.

![Brama nie utworzono](./media/vpn-gateway-configure-vpn-gateway-mp/IC717025.png)


Następnie u dołu strony kliknij **Tworzenie bramy**. Możesz wybrać pozycję *Routing statyczny* lub *Routing dynamiczny*. Wybranego typu routingu VPN zależy od kilku czynników. Na przykład urządzenie VPN obsługuje i czy jest potrzebny do obsługi połączeń punktu do witryny. Sprawdź [O urządzeniach VPN łączności sieciowej wirtualnego](vpn-gateway-about-vpn-devices.md) Sprawdź typ routingu VPN, które są potrzebne. Po utworzeniu bramy nie można zmienić między typami routingu VPN brama bez usuwanie i ponowne tworzenie bramy. System zostanie wyświetlony monit o potwierdzenie zamiaru bramy utworzony, kliknij przycisk **Tak**.

![Typ routingu VPN bramy](./media/vpn-gateway-configure-vpn-gateway-mp/IC717026.png)

Tworząc Centrum, zwróć uwagę, grafika bramy na stronie zmieni się na żółty i opisem *Tworzenie bramy*. Może potrwać do 45 minut bramy utworzyć. Poczekaj, aż bramy zakończeniu przed przejściem do przodu z innymi ustawieniami konfiguracji.

![Tworzenie bramy](./media/vpn-gateway-configure-vpn-gateway-mp/IC717027.png)

Gdy bramy zmieni się *Łączenie*, można zbierać informacje niezbędne dla swojego urządzenia VPN.

![Łączenie bramy](./media/vpn-gateway-configure-vpn-gateway-mp/IC717028.png)

## <a name="gather-information-for-your-vpn-device-configuration"></a>Gromadzenie informacji o konfiguracji urządzenia VPN

Po utworzeniu bramy gromadzenie informacji o konfiguracji urządzenia VPN. Te informacje znajduje się na stronie **pulpitu nawigacyjnego** w sieci wirtualnej:

1. **Adres IP bramy-** Adres IP można znaleźć na stronie **pulpitu nawigacyjnego** . Nie będzie widoczne poczekaj, aż po zakończeniu Centrum tworzenia.

1. **Klucz udostępnione —** Kliknij pozycję **Zarządzaj klawisz** u dołu ekranu. Kliknij ikonę obok klawisz, aby skopiować go do Schowka, a następnie wklej i zapisać klucz. Ten przycisk działa tylko po jednym tunelem S2S VPN. Jeśli masz wiele tuneli S2S VPN, użyj *Uzyskiwanie wirtualna sieć udostępniane klucza bramy* interfejsu API lub polecenia cmdlet programu PowerShell.

![Zarządzanie klucza](./media/vpn-gateway-configure-vpn-gateway-mp/IC717029.png)


## <a name="configure-your-vpn-device"></a>Konfigurowanie urządzenia VPN

Po wykonaniu powyższych czynności, możesz lub administrator sieci należy skonfigurować urządzenia VPN, aby utworzyć połączenie. Zobacz [Temat urządzeń VPN łączności sieciowej wirtualnych](vpn-gateway-about-vpn-devices.md) uzyskać więcej informacji o urządzeniach sieci VPN.

Po skonfigurowaniu urządzenia VPN dla swojego VNet można wyświetlać informacje o połączeniu zaktualizowane na stronie pulpitu nawigacyjnego.

Możesz również uruchomić jedną z następujących poleceń, aby sprawdzić połączenie:

|                      | Cisco ASA             | ISR Cisco automatycznego odzyskiwania systemu         | Juniper SSG-ISG | Juniper SRX-J                            |
|----------------------|-----------------------|-----------------------|-----------------|------------------------------------------|
| **Sprawdzanie skojarzenia zabezpieczeń w trybie głównym**  | Pokazywanie szyfrowania isakmp sa | Pokazywanie szyfrowania isakmp sa | Pobierz plik cookie ike  | Pokazywanie zabezpieczeń skojarzenia zabezpieczeń ike   |
| **Sprawdzanie skojarzeń zabezpieczeń trybu szybkiego** | Pokazywanie szyfrowania ipsec sa  | Pokazywanie szyfrowania ipsec sa  | Uzyskiwanie sa          | Pokazywanie skojarzenia zabezpieczeń zabezpieczeń ipsec |


## <a name="verify-your-local-network-ranges-and-vpn-gateway-ip-address"></a>Sprawdź zakresy sieci lokalnej i adres IP bramy sieci VPN

### <a name="verify-your-vpn-gateway-ip-address"></a>Weryfikowanie adresu IP bramy sieci VPN

Dla bramy połączenia adres IP dla danego urządzenia VPN musi być poprawnie skonfigurowany dla sieci lokalnej określone dla konfiguracji między lokalnej. Zazwyczaj jest to skonfigurować podczas procesu konfigurowania witryny do witryny. Jednak jeśli wcześniej używano tej sieci lokalnej z innego urządzenia lub adres IP został zmieniony tej sieci lokalnej, Edytuj ustawienia, aby określić prawidłowy adres IP bramy.

1. Aby sprawdzić adres IP bramy, kliknij polecenie **sieci** w okienku po lewej stronie portalu, a następnie wybierz pozycję **Sieci lokalnych** w górnej części strony. Zostanie wyświetlony adres Brama VPN dla każdej sieci lokalnej, które zostały utworzone. Aby edytować adres IP, zaznacz VNet, a następnie kliknij przycisk **Edytuj** w dolnej części strony.

1. Na stronie **Określ szczegóły swojej sieci lokalnej** Edytuj adres IP, a następnie kliknij następny strzałkę u dołu strony.

1. Na stronie **Określ przestrzeni adresów** kliknij znacznik wyboru w prawym dolnym, aby zapisać ustawienia.

### <a name="verify-the-address-ranges-for-your-local-networks"></a>Sprawdź zakresy adresów sieci lokalnej

Poprawne przepływ przez bramę do swojej lokalizacji lokalnego ruchu należy zweryfikować, że poszczególne zakresy adresów IP jest określony. Poszczególne zakresy musi być wymieniona w konfiguracji Azure **Sieci lokalnej** . W zależności od konfiguracji sieci lokalnej lokalizacji może to być nieco dużych zadania. Ruch powiązanej dla znajdującej się w obrębie listy zakresów adresów IP będą wysyłane za pośrednictwem wirtualnych sieci VPN bramy. Zakresy, które możesz listy nie muszą być prywatne zakresów, mimo że można sprawdzić, czy konfiguracji lokalnego może odbierać ruch przychodzący.

Aby dodać lub edytować zakresy dla sieci lokalnej, wykonaj następujące czynności.

1. Aby edytować zakresy adresów IP dla sieci lokalnej, kliknij polecenie **sieci** w okienku po lewej stronie portalu, a następnie wybierz pozycję **Sieci lokalnych** w górnej części strony. W portalu Najprostszym sposobem na wyświetlenie zakresy, które zostały umieszczone jest na stronie **Edytowanie** . Aby wyświetlić zakresy, zaznacz VNet, a następnie kliknij przycisk **Edytuj** w dolnej części strony.

1. Na stronie **Określ szczegóły swojej sieci lokalnej** nie wprowadzono żadnych zmian. Kliknij przycisk Następny strzałkę u dołu strony.

1. Na stronie **Określ przestrzeni adresów** wprowadź zmiany odstęp adresu sieci. Następnie kliknij znacznik wyboru, aby zapisać konfigurację.

## <a name="how-to-view-gateway-traffic"></a>Jak wyświetlić ruch bramy

Na stronie wirtualną sieć **pulpitu nawigacyjnego** można wyświetlanie bramy i ruch bramy.

Na stronie **pulpitu nawigacyjnego** można wyświetlić następujące czynności:

- Ilości danych, który jest ułożony przez bramę, zarówno danych, jak i danych.

- Nazwy serwerów DNS, które są określane w sieci wirtualnej.

- Połączenie między Centrum i urządzenia VPN.

- Klucz udostępniony, który służy do konfigurowania połączenie bramy urządzenia VPN.


## <a name="how-to-change-the-vpn-routing-type-for-your-gateway"></a>Jak zmienić typ routingu VPN z bramy

Ponieważ niektóre konfiguracje łączności są dostępne tylko dla określonych typów routingu bramy, może się okazać, musisz zmienić typ routingu VPN istniejącej bramy sieci VPN bramy. Na przykład można dodać łączności punktu do witryny do już istniejącego połączenia witryny do witryny zawierającej statyczne bramy. Połączenia punkt do witryny wymagają dynamiczne bramy. Oznacza to skonfigurować połączenie P2S, należy zmienić bramy sieci VPN typu routingu z statyczne na dynamiczne.

Jeśli chcesz zmienić typ routingu VPN bramy będzie usunąć istniejące bramy, a następnie utwórz Nowa brama z nowym typem routingu. Nie musisz usunąć całą wirtualną sieć Aby zmienić typ routingu bramy.

Przed zmianą Centrum typ routingu VPN, upewnij się sprawdzić, czy urządzenia VPN obsługuje routingu typ, który ma być używany. Aby pobrać nowe routingu przykłady konfiguracji i sprawdź wymagania urządzenia VPN, zobacz [Informacje o urządzeń VPN wirtualnych łączności sieciowej](vpn-gateway-about-vpn-devices.md).

>[AZURE.IMPORTANT] Po usunięciu Brama wirtualnej sieci VPN jest publikowany VIP przypisane do bramy. Gdy odtworzyć bramy, nowe VIP przydzielono go.

1. **Usuń istniejące Brama VPN.**

    Na stronie **pulpitu nawigacyjnego** wirtualnej sieci przejdź do dolnej części strony, a następnie kliknij przycisk **Usuń bramy**. Poczekaj na powiadomienie, że brama została usunięta. Po otrzymaniu powiadomienia na ekranie usunięto Centrum, możesz utworzyć Nowa brama.

1. **Tworzenie nowych bram sieci VPN.**

    Wykonaj procedurę w górnej części strony, aby utworzyć nowych bram: [Tworzenie bramy sieci VPN](#create-a-vpn-gateway).


## <a name="next-steps"></a>Następne kroki

Możesz dodać maszyn wirtualnych z siecią wirtualną. Dowiedz się, [jak utworzyć niestandardowe maszyny wirtualnej](../virtual-machines/virtual-machines-windows-classic-createportal.md).

Jeśli chcesz skonfigurować połączenie VPN punktu do witryny, zobacz [Konfigurowanie połączenie VPN punktu do witryny](vpn-gateway-point-to-site-create.md).

 
