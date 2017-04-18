<properties
    pageTitle="Jak skonfigurować środowisko aplikacji usługi | Microsoft Azure"
    description="Konfiguracja, zarządzanie i monitorowanie środowiska usługi aplikacji"
    services="app-service"
    documentationCenter=""
    authors="ccompy"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="ccompy"/>


# <a name="configuring-an-app-service-environment"></a>Konfigurowanie środowiska aplikacji usługi #

## <a name="overview"></a>Omówienie ##

Na wysokim poziomie środowisku usługi Azure aplikacji składa się z kilku główne składniki:

- Zasoby obliczeń, które są uruchomione w środowisku usługi aplikacji hostowana usługa
- Miejsca do magazynowania
- Bazy danych
- Classic(V1) lub zasób Manager(V2) Azure wirtualnej sieci (VNet) 
- Podsieć z usługą środowisko usługi aplikacji hostowanej uruchomioną w niej

### <a name="compute-resources"></a>Obliczanie zasobów

Możesz korzystać z zasobów obliczeń z puli zasobów cztery.  Każdego środowiska usługi aplikacji (ASE) ma zestaw punktów końcowych przedniej i trzech pul możliwe pracownika. Nie trzeba używać wszystkich trzech pracownik pul — Jeśli chcesz, można użyć jednej lub dwóch.

Hosty w pul zasobów (interfejsy i pracowników) nie są dostępne bezpośrednio z dzierżawami. Nie można łączyć się z nimi, zmienianie ich inicjowania obsługi administracyjnej za pomocą protokołu RDP (Remote Desktop) lub pełnić rolę administratora nad nimi.

Możesz skonfigurować ilość puli zasobów i rozmiar. W ASE dostępne są cztery opcje rozmiaru, które są oznaczone P1 za pośrednictwem P4. Aby uzyskać szczegółowe informacje o tych rozmiarów i ich ceny zobacz [ceny aplikacji usługi](../app-service/app-service-value-prop-what-is.md).
Zmienianie ilości lub rozmiaru nosi nazwę operacji Skala.  Operacja tylko jednej skali może być w toku naraz.

**Kończy się wierzch**: przedniej punkty końcowe są punkty końcowe protokołu HTTP/HTTPS dla Twoje aplikacje, które są przechowywane w swojej ASE. Obciążenia nie działają w przedniej zostanie zakończone.

- ASE zaczyna się od dwóch P2s, która jest wystarczające dla deweloperów/testowanie obciążenia i obciążenia niższego poziomu produkcji. Zdecydowanie zaleca się P3s dla umiarkowane do obciążenia intensywnie produkcji.
- Moderowanie z dużą ilością produkcji obciążenia zaleca się, że masz co najmniej czterech P3s w celu zapewnienia istnieją wystarczające przedniej zostanie zakończone, gdy występuje zaplanowanych. Działania zaplanowanych spowoduje przejście w dół jeden fronton naraz. Zmniejsza ogólnej pojemność zewnętrzną podczas działania związane z obsługą.
- Nie możesz dodać natychmiast nowe wystąpienie zewnętrzną. Mogą przyjmować między godziną 2-3 dostarczaniem.
- Dalsze szczegóły skali, należy monitorować CPU procentowa, procent pamięci i metryk aktywnych żądań dla puli zewnętrzną. Jeśli wartości procentowych Procesora i pamięci przekracza 70%, gdy działa P3s, dodać więcej przedniej zostanie zakończone. Jeśli wartość aktywnych żądań uśrednia do 15 000 do 20 000 żądania na serwer sieci Web, można też dodać więcej przedniej zostanie zakończone. Podstawowym celem jest Procesora i pamięci wartości procentowych poniżej 70% i aktywnych żądań uśrednianie do poniżej 15 000 żądania na wierzch zakończenia, gdy używasz P3s.  

**Pracownicy**: pracowników są, gdzie rzeczywistości uruchamianie aplikacji. Podczas rozbudowy planów usługi aplikacji, która korzysta z pracowników w puli pracownika skojarzonego.

- Nie możesz dodać natychmiast pracowników. Ta osoba może potrwać od 2 do 3 godziny do świadczenia, niezależnie od tego, ile są dodawane.
- Skalowanie rozmiaru zasób obliczeń dla każdej puli potrwa 2-3 godziny dla domeny aktualizacji. Istnieje 20 domen aktualizacji w ASE. W przypadku zmiany rozmiaru obliczeń puli pracowników o 10 wystąpieniach skali może zająć między 20 – 30 godzin.
- Zmiana rozmiaru zasobów obliczeń, które są używane w puli pracownik spowoduje zimnej uruchamiania aplikacji, które działają w tej puli pracownika.

Najszybszym sposobem na zmienianie rozmiaru zasobów obliczeń roboczych, który nie jest uruchomiony żadnych aplikacji jest:

- Skali licznik wystąpień 0. Aby cofnąć usługi wystąpienia zajmie około 30 minut.
- Wybierz nowy rozmiar obliczeń i liczbę wystąpień. W tym miejscu spowoduje przejście między godzin 2-3.

Jeśli Twoje aplikacje wymagają rozmiar zasobu obliczeń, możesz nie można korzystać z poprzedniego wskazówki. Zamiast zmieniać rozmiar roboczych, obsługującym te aplikacje, można wypełnić kolejnej puli pracownika z pracowników o odpowiednim rozmiarze i przenieść aplikacji do tej puli.

- Tworzenie dodatkowych wystąpienia rozmiar potrzebne do uruchamiania w innej puli pracownika. Zostanie otwarta od 2 do 3 godziny do wykonania.
- Przydziel ponownie planów usługi aplikacji, które obsługują aplikacje, które muszą rozmiar puli nowo skonfigurowanego pracownika. To jest szybkie operacji, przesyłanego w ciągu mniej niż minuty.  
- Jeśli już nie potrzebujesz tych wystąpień nieużywane skali pierwszego roboczych. Operacja trwa około 30 minut do wykonania.

**Autoscaling**: jest jednym z narzędzi, które mogą ułatwić zarządzanie zużycie zasobów do obliczeń autoscaling. Możesz użyć autoscaling dla zewnętrzną lub pule pracownika. Można wykonać czynności, takich jak Zwiększ usługi wystąpienia dowolnego typu puli rano i zmniejszyć go wieczorem. Lub być może dodać wystąpienia po określonej wartości progowej mniejsza niż liczba pracowników, które są dostępne w puli pracownika.

Jeśli chcesz ustawić reguły autoscale wokół metryki puli zasobów obliczeń, a następnie należy pamiętać podczas inicjowania obsługi administracyjnej wymaga. Aby uzyskać więcej informacji na temat autoscaling środowiska usługi aplikacji, zobacz [jak skonfigurować autoscale w środowisku usługi aplikacji][ASEAutoscale].

### <a name="storage"></a>Miejsca do magazynowania

Każdy ASE skonfigurowano 500 GB miejsca do magazynowania. To miejsce jest używane przez wszystkich aplikacji w ASE. Ten ilości miejsca do magazynowania jest częścią ASE i obecnie nie przełącza się na używanie miejsca na dysku. Jeżeli wprowadzasz zmiany do kierowania wirtualnej sieci lub zabezpieczeń, musisz nadal Zezwalaj na dostęp do magazynowania Azure — lub ASE nie działa.

### <a name="database"></a>Bazy danych

Baza danych zawiera informacje, które definiują środowisko, a także szczegółowe informacje dotyczące aplikacji, które są uruchomione w nim. To jest zbyt stanowi część subskrypcji przechowywanych Azure. Nie jest coś, co ma bezpośredni możliwość manipulowania. Jeżeli wprowadzasz zmiany do kierowania wirtualnej sieci lub zabezpieczeń, musisz nadal Zezwalaj na dostęp do platformy SQL Azure — lub ASE nie działa.

### <a name="network"></a>Sieci

VNet, używanej z Twojej ASE może być jedną, które zostały wprowadzone po utworzeniu ASE lub jedną, które zostały wprowadzone wyprzedzeniem. Po utworzeniu podsieci podczas tworzenia ASE wymusza ASE znajdować się w tej samej grupy zasobów co wirtualnej sieci. Jeśli jest potrzebna grupa zasobów, używane przez usługi ASE na inny niż usługi VNet, następnie należy utworzyć usługi ASE przy użyciu szablonu Menedżera zasobów.

Istnieją pewne ograniczenia w wirtualnej sieci, która służy do ASE:
 
- Wirtualna sieć musi być regionalne wirtualnej sieci.
- Musi istnieć podsieci z 8 lub więcej adresów miejsce, w którym zostanie wdrożony ASE.
- Po podsieci jest używana do obsługi ASE, nie można zmienić zakres adres podsieci. Z tego powodu zalecamy, że podsieci zawiera co najmniej 64 adresy, aby zezwalały na dowolnym przyszłego zwiększenia ilości ASE.
- Mogą występować nic w podsieci, ale ASE.

W odróżnieniu od hostowana usługa, która zawiera ASE, [wirtualną sieć] [ virtualnetwork] oraz podsieci na podstawie kontrola użytkownika.  Można administrować siecią wirtualną za pośrednictwem wirtualnych sieci interfejsu użytkownika lub programu PowerShell.  ASE mogą być rozmieszczone w klasycznym lub VNet Menedżera zasobów.  Portal i interfejsu API środowiska różnią się nieco między klasyczny i VNets Menedżera zasobów, ale efekt ASE jest taki sam.

VNet, która jest używana do obsługi ASE można użyć albo prywatnych adresów RFC1918 IP lub można użyć publicznych adresów IP.  Jeśli chcesz używać zakresu adresów IP, które nie są objęte RFC1918 (10.0.0.0/8 172.16.0.0/12, 192.168.0.0/16), a następnie należy utworzyć VNet i podsieci, może być używany przez usługi ASE związaną z wcześniejszym tworzenia ASE.

Ponieważ funkcja ta umieszcza usługę aplikacji Azure w wirtualnej sieci, oznacza to, że Twoje aplikacje, które znajdują się w Twojej ASE teraz dostęp do zasobów, które są dostępne za pośrednictwem witryny do witryny wirtualnych sieci prywatnych (VPN) lub ExpressRoute bezpośrednio. Aplikacjami, które są w środowisku usługi aplikacji nie wymagają dodatkowe funkcje sieci, aby uzyskać dostęp do zasobów dostępnych dla wirtualnej sieci, obsługujący środowiska usługi aplikacji. Oznacza to, że nie trzeba używać połączeń hybrydowego lub integracji VNET uzyskiwania dostępu do zasobów w lub połączenie z siecią wirtualną. Nadal można obie te funkcje, jeśli dostęp do zasobów w sieci, które nie są połączone z siecią wirtualną.

Na przykład integracji VNET umożliwia Integracja z wirtualnej sieci, który jest w ramach subskrypcji, ale nie jest połączony z siecią wirtualną, należącym do ASE. Nadal można połączeń hybrydowego dostępu do zasobów, które znajdują się w innych sieciach, podobnie jak zwykle.  

Jeśli masz wirtualnej sieci skonfigurowano VPN ExpressRoute należy pamiętać niektórych potrzeb routingu, które ma ASE. Istnieją pewne konfiguracje rozsyłania zdefiniowane przez użytkownika (UDR), które są niezgodne z ASE. Aby uzyskać szczegółowe informacje o programie ASE wirtualnej sieci z ExpressRoute, zobacz [uruchomiony środowisku usługi aplikacji w wirtualnej sieci z ExpressRoute][ExpressRoute].

#### <a name="securing-inbound-traffic"></a>Zabezpieczanie ruchu przychodzącego

Istnieją dwie podstawowe metody do sterowania przychodzący ruch do Twojej ASE.  Za pomocą sieci zabezpieczeń Groups(NSGs) aby kontrolować, jakie IP adresy uzyskiwać dostęp do Twojej ASE, przedstawione w tym [Jak kontrolować, ruch przychodzący w środowisku usługi aplikacji](app-service-app-service-environment-control-inbound-traffic.md) i można również skonfigurować do ASE z wewnętrznych Balancer(ILB) ładowania.  Tych funkcji można również razem Jeśli chcesz ograniczyć dostęp za pomocą NSGs do swojego ASE ILB.

Po utworzeniu ASE utworzy VIP w swojej VNet.  Istnieją dwa typy VIP wewnętrznych i zewnętrznych.  Po utworzeniu ASE z zewnętrznych VIP Twoje aplikacje w swojej ASE zostaną dostępny za pośrednictwem adres internetowy routingu IP. Po wybraniu wewnętrzny usługi ASE skonfigurowano ILB i nie są bezpośrednio internet dostępne.  ASE ILB nadal wymaga zewnętrznych VIP, ale jest używany tylko do Azure zarządzanie i obsługę programu access.  

Podczas tworzenia ILB ASE udostępnia poddomeny używane przez ILB ASE i będzie trzeba zarządzać DNS poddomeny zadanej.  Ponieważ wartość nazwy poddomeny również potrzebne do zarządzania certyfikatu użytego dostępu HTTPS.  Po utworzeniu ASE zostanie wyświetlony monit o podanie certyfikatu.  Aby dowiedzieć się więcej na temat tworzenia i używania ASE ILB odczytywać [przy użyciu wewnętrzny usługi równoważenia obciążenia ze środowiskiem usługi aplikacji][ILBASE]. 


## <a name="portal"></a>Portal

Można zarządzać i monitorować środowiska usługi aplikacji przy użyciu interfejsu użytkownika w portalu Azure. Jeśli masz ASE, to może powodować wyświetlanie symbol aplikacji usługi na swojej bocznym. Ten symbol jest używany do przedstawiania środowiska usługi aplikacji w portalu Azure:

![Symbol środowiska usługi aplikacji][1]

Aby otworzyć interfejsu użytkownika, który zawiera listę wszystkich do środowiska usługi aplikacji, można użyć ikony lub wybierz Pagon skierowany w (">" symbol) w dolnej części paska bocznego zaznacz środowiska usługi aplikacji. Po zaznaczeniu jednego z ASEs na liście, możesz otworzyć interfejsu użytkownika, który jest używany do monitorowania i zarządzania nim.

![Interfejs użytkownika do monitorowania i zarządzania nimi środowiska usługi aplikacji][2]

To pierwszy karta zawiera niektóre właściwości usługi ASE wraz z metryczne wykres na puli zasobów. Niektóre właściwości, które są wyświetlane w bloku **Essentials** są również hiperłączy, które zostanie wyświetlone kartę, która jest z nim skojarzonego. Na przykład można wybrać nazwę **Wirtualną sieć** otwarcia interfejsu użytkownika skojarzonego z wirtualną sieć, uruchomione w Twojej ASE. **Plany aplikacji usługi** , jak i **aplikacje** otwarcia karty, których te elementy, które znajdują się w Twojej ASE.  

### <a name="monitoring"></a>Monitorowanie

Wykresy umożliwiają Zobacz różne wskaźniki w każdej puli zasobów. Dla puli zewnętrzną można monitorować średniej Procesora i pamięci. Pracownik puli można monitorować ilość, która jest używana i ilość, która jest dostępna.

Wiele aplikacji usługi, które można wprowadzić plany za pomocą pracowników w puli pracownika. Obciążenie pracą nie jest rozpowszechniane w taki sam sposób, na serwerach frontonu, więc użycie Procesora i pamięci nie udostępnia wiele kategoriach przydatne informacje. Bardziej ważne jest do śledzenia liczbę pracowników, które były używane i dostępne — zwłaszcza jeśli zarządzasz tego systemu dla innych użytkowników.  

Umożliwia także wszystkich miar, które mogą być śledzone na wykresach Aby skonfigurować alerty. Konfigurowanie alertów w tym miejscu działa tak samo jak w innym miejscu w aplikacji usługi. Można ustawić alert z dowolnej części interfejsu użytkownika **alerty** lub przechodzenie do dowolnego metryki interfejsu użytkownika szczegółów i wybierając pozycję **Dodaj Alert**.

![Metryki interfejsu użytkownika][3]

Wskaźniki, które właśnie zostały omówione są metryki środowisko usługi aplikacji. Dostępne są także miar, które są dostępne na poziomie plan usługi aplikacji. Jest to miejsce, w którym monitorowania Procesora i pamięci sprawia, że wiele właściwe rozwiązanie.

W ASE wszystkie plany aplikacji usługi są dedykowane plany aplikacji usługi. Oznacza to, że tylko aplikacje, które są uruchomione na hosts przydzielono plan usług aplikacji to aplikacje w programie aplikacji usługi. Aby wyświetlić szczegóły w planie aplikacji usługi, wywoływanie planu usług aplikacji z dowolnego z tych list w Interfejsie użytkownika ASE lub **Przeglądanie aplikacji usługi planów** (której znajdują się wszystkie ich).   

### <a name="settings"></a>Ustawienia

W karta ASE jest sekcję **Ustawienia** , która zawiera kilka ważnych możliwości:

**Ustawienia** > **Właściwości**: karta **Ustawienia** otwierany automatycznie podczas wywoływanie usługi karta ASE. U góry jest **Właściwości**. Istnieje wiele elementów w tym miejscu zbędne, aby wyświetlić w **Essentials**, ale co to jest bardzo przydatne jest **Wirtualny adres IP**, a także **Ruchu wychodzącego adresów IP**.

![Karta Ustawienia i właściwości][4]

**Ustawienia** > **Adresów IP**: podczas tworzenia aplikacji IP Secure Sockets Layer (SSL) w swojej ASE, potrzebujesz adresu IP protokołu SSL. Aby można było uzyskać, usługi ASE musi adresy IP protokołu SSL, które jest właścicielem, które można przypisać. Po utworzeniu ASE ma adres IP protokołu SSL do tego celu, ale można dodać więcej. Istnieje opłatę dla dodatkowych adresów IP protokołu SSL adresy, jak pokazano w [Aplikacji usługi ceny] [ AppServicePricing] (w sekcji w połączeniach SSL). Dodatkowe cena to cena SSL adresów IP.

**Ustawienia** > **Puli końcu przedniego** / **Pul pracownik**: każdy z tych karty puli zasobów oferuje możliwość wyświetlania informacji tylko na tej puli zasobów, oprócz dostarczając kontrolek w pełni przeskalować tej puli zasobów.  

Karta podstawowego dla każdej puli zasobów zawiera wykres z metryki dla tej puli zasobów. Tak jak w przypadku wykresów z karta ASE możesz przejść do wykresu i skonfiguruj alerty w razie potrzeby. Ustawianie alertu z karta ASE puli zasobów dla określonych działa tak samo, jak to zrobić z puli zasobów. Karta **Ustawienia** puli pracownik masz dostęp do wszystkich aplikacji lub plany aplikacji usługi są uruchomione w tej puli pracownika.

![Ustawienia puli roboczego interfejsu użytkownika][5]

### <a name="portal-scale-capabilities"></a>Funkcje skali portalu  

Istnieją trzy operacje skali:

- Zmienianie liczby adresów IP w ASE, które są dostępne dla zastosowania SSL adresów IP.
- Zmienianie rozmiaru zasobów obliczeń, który jest używany w puli zasobów.
- Zmienianie liczby zasobów obliczeń, które są używane w puli zasobów, ręcznie lub za pośrednictwem autoscaling.

Istnieją trzy sposoby kontrolowania liczby serwerów, które masz w swojej pul zasobów, w portalu:

- Operacja skali z głównego karta ASE u góry. Umożliwia skali wielu zmian w konfiguracji pul frontonu i pracownika. Są wszystkie stosowane jako pojedyncza.
- Ręczne skali operacji karta **Skala** puli poszczególnych zasobów, które znajduje się w obszarze **Ustawienia**.
- Autoscaling, skonfigurowana z karta **Skala** puli poszczególnych zasobów.

Aby użyć operacji skali na karta ASE, przeciągnij suwak do ilości i Zapisz stronę. Ten interfejs obsługuje także zmiany rozmiaru.  

![Skala interfejsu użytkownika][6]

Aby użyć funkcji instrukcji lub autoscale w puli zasobów określonych, przejdź do strony **Ustawienia** > **Puli końcu przedniego** / **Pul pracownik** , zależnie od potrzeb. Następnie otwarcia puli, które chcesz zmienić. Przejdź do pozycji **Ustawienia** > **skali się** lub **Ustawienia** > **rozbudowy**. Karta **Skala się** umożliwia sterowanie ilość wystąpienie. **Skala w górę** umożliwia określanie rozmiaru zasobów.  

![Ustawienia skali interfejsu użytkownika][7]

## <a name="fault-tolerance-considerations"></a>Zagadnienia dotyczące odporności na uszkodzenia

Można skonfigurować w środowisku usługi aplikacji do 55 zasoby obliczeń sumy. Tych 55 zasobów obliczeń tylko 50 może służyć do obciążenia hosta. Przyczyną jest podwójny. Istnieje co najmniej 2 zasobów zewnętrzną obliczeń.  Który pozostawia do 53 do obsługi alokacji puli pracownika. Aby zapewnić odporność na uszkodzenia, trzeba mieć dodatkowe obliczeń zasobu przydzielonego zgodnie z następującymi regułami:

- Każdy roboczych wymaga co najmniej 1 zasobów dodatkowych obliczeń, które nie są dostępne dla przydzielania obciążenie pracą.
- Gdy ilość zasobu obliczeń w puli pracownik przechodzi powyżej określonej wartości, innego zasobu obliczeń jest wymagana odporność na uszkodzenia. Nie jest wielkość liter w puli zewnętrzną.

W dowolnej pojedynczej roboczych wymagania dotyczące odporności na uszkodzenia obejmują dla danej wartości X zasobów przydzielonych do puli pracownika:

- Jeśli argument X ma od 2 do 20, ilości zasobów można używać obliczeń, które są dostępne dla obciążenia jest X-1.
- Jeśli X jest zawarte między 21 i 40, ilości zasobów można używać obliczeń, które są dostępne dla obciążenia jest X-2.
- Jeśli X jest zawarte między 41 i 53, ilości zasobów można używać obliczeń, które są dostępne dla obciążenia jest X-3.

Minimalne za ma 2 serwerach frontonu i 2 pracowników.  Z podanych instrukcji następnie, Oto kilka przykładów wyjaśnienie:  

- Jeśli masz 30 pracowników w jednej puli 28 ich może służyć obciążenia hosta.
- Jeśli masz 2 pracowników w jednej puli 1 może służyć obciążenia hosta.
- Jeśli masz 20 pracowników w jednej puli 19 może służyć obciążenia hosta.  
- Jeśli masz 21 pracowników w jednej puli, następnie nadal tylko 19 może służyć do obciążenia hosta.  

Ważne jest proporcji odporność na uszkodzenia, ale musisz należy pamiętać podczas skalowania powyżej określone progi. Jeśli chcesz dodać więcej możliwości z 20 wystąpienia, należy przejść do 22 lub nowszej ponieważ 21 nie są dodawane wszelkie możliwości. Dotyczy to także przechodząc ponad 40, gdzie kolejny numer, który zapewnia możliwości jest 42.  

## <a name="deleting-an-app-service-environment"></a>Usuwanie środowisku aplikacji usługi ##

Jeśli chcesz usunąć środowisku usługi aplikacji, następnie po prostu użyć **akcji** w górnej części karta Środowisko usługi aplikacji. Gdy to zrobisz, zostanie wyświetlony monit o wprowadzenie nazwy środowiska usługi aplikacji, aby potwierdzić, że na pewno chcesz to zrobić. Należy zauważyć, że po usunięciu środowisku usługi aplikacji możesz usunąć całą zawartość w nim również.  

![Usuwanie środowisku usługi aplikacji interfejsu użytkownika][9]  

## <a name="getting-started"></a>Wprowadzenie

Aby rozpocząć pracę ze środowiskami usługi aplikacji, zobacz [jak utworzyć środowisku usługi aplikacji](app-service-web-how-to-create-an-app-service-environment.md).

Aby uzyskać więcej informacji na temat platformy Azure aplikacji usługi zobacz [Azure aplikacji usługi](../app-service/app-service-value-prop-what-is.md).

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!--Image references-->
[1]: ./media/app-service-web-configure-an-app-service-environment/ase-icon.png
[2]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-aseblade.png
[3]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-poolchart.png
[4]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-properties.png
[5]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-poolblade.png
[6]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-scalecommand.png
[7]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-poolscale.png
[8]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-pricingtiers.png
[9]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-deletease.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[Appserviceplans]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[HowtoScale]: http://azure.microsoft.com/documentation/articles/app-service-web-scale-a-web-app-in-an-app-service-environment/
[ControlInbound]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[ExpressRoute]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/
[ILBASE]: http://azure.microsoft.com/documentation/articles/app-service-environment-with-internal-load-balancer/
