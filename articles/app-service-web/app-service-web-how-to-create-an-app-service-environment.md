<properties 
    pageTitle="Jak utworzyć środowisku aplikacji usługi" 
    description="Opis przepływu tworzenia środowiska usługi aplikacji" 
    services="app-service" 
    documentationCenter="" 
    authors="ccompy" 
    manager="stefsch" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/22/2016" 
    ms.author="ccompy"/>

# <a name="how-to-create-an-app-service-environment"></a>Jak utworzyć środowisku aplikacji usługi #

### <a name="overview"></a>Omówienie ###

Środowiska usługi aplikacji (ASE) są opcja serwisowa Premium usługi aplikacji Azure, która zapewnia możliwości rozszerzona konfiguracja, które nie jest dostępna w sygnatury wielu dzierżawy.  Funkcja ASE zasadniczo wdraża usługa Azure aplikacji do wirtualnej sieci klienta.  Do uzyskania lepszego zrozumienia możliwości oferowane przez środowiska usługi aplikacji przeczytać, [Co to jest środowisku usługi aplikacji] [ WhatisASE] dokumentacji.

### <a name="before-you-create-your-ase"></a>Przed utworzeniem usługi ASE ###

Należy wiedzieć o możliwości, które nie można zmienić.  Aspekty, których nie można zmienić o swojej ASE po jego utworzeniu są:

- Lokalizacja
- Subskrypcji
- Grupa zasobów
- VNet używane
- Podsieci używane 
- Rozmiar podsieci

Wybierając VNet i określanie podsieci, upewnij się, jest wystarczająco duża, aby uwzględnić wszystkie przyszłego zwiększenia ilości.  

### <a name="creating-an-app-service-environment"></a>Tworzenie środowiska aplikacji usługi ###

Istnieją dwa sposoby uzyskiwania dostępu do tworzenia ASE interfejsu użytkownika.  Można znaleźć, wyszukując w Azure Marketplace ***Środowisko usługi aplikacji*** lub za pośrednictwem nowy -> Web + Mobile -> środowisko usługi aplikacji.  Aby utworzyć ASE:

1. Wprowadź nazwę swojej ASE.  Nazwa, którą określono dla ASE będzie używana w przypadku aplikacji utworzonych w ASE.  Jeśli nazwa ASE to appsvcenvdemo Nazwa poddomeny może być. *appsvcenvdemo.p.azurewebsites.net*.  Jeśli utworzono więc aplikacji o nazwie *mytestapp* , a następnie będzie adresach u *mytestapp.appsvcenvdemo.p.azurewebsites.net*.  Nazwa Twojej ASE, nie można używać pustego miejsca.  Jeśli używasz wielkich liter w nazwie, nazwę domeny będzie sumy wersją małe litery o tej nazwie.  Jeśli korzystasz z ILB następnie nazwę ASE nie jest używany w swojej poddomeny, ale zamiast tego jest wymienione podczas tworzenia ASE

    ![][1]

2. Wybierz subskrypcję.  Subskrypcja usługi ASE na potrzeby jest również jeden z wszystkich aplikacji w tym ASE zostanie utworzony przy użyciu.  Nie można umieścić usługi ASE w VNet, który znajduje się w innej subskrypcji

3. Wybierz lub określ nowej grupy zasobów.  Grupa zasobów na potrzeby usługi ASE musi być taka sama, używany na potrzeby usługi VNet.  Zaznaczenie istniejącego VNet wybór grupa zasobów dla swojego ASE zostaną zaktualizowane, aby odzwierciedlała ze swojego VNet.

    ![][2]

4. Wybierz odpowiednie opcje wirtualnej sieci i lokalizacji.  Możesz utworzyć nowy VNet lub zaznacz istniejącego VNet.  Po wybraniu nowego VNet, a następnie można określić nazwę i lokalizację. Nowy VNet będzie mieć 192.268.250.0/23 zakres adresu i podsieci o nazwie **domyślny** jest określony jako 192.168.250.0/24.  Możesz też po prostu wybrać istniejącego Classic lub VNet Menedżera zasobów.  Wybieranie typu VIP określa usługi ASE może uzyskiwać bezpośrednio z Internetu (zewnętrzny) lub korzysta z wewnętrznych równoważenia obciążenia (ILB).  Aby dowiedzieć się więcej na temat ich odczytywać [przy użyciu wewnętrzny usługi równoważenia obciążenia ze środowiskiem usługi aplikacji][ILBASE].  Wybierz typ VIP zewnętrzne można wybrać liczbę zewnętrznych adresów IP systemu jest tworzone na potrzeby IPSSL.  W przypadku wybrania wewnętrznego należy określić poddomena będzie korzystać z ASE.  ASEs mogą być rozmieszczone w wirtualnych sieci korzystające z *obu* zakresów publiczny adres, *lub* RFC1918 adres spacji (to znaczy adresy prywatne).  Aby użyć wirtualnej sieci zakres adresów publicznych, należy utworzyć VNet wyprzedzeniem.  Po zaznaczeniu istniejącego VNet potrzebne do utworzenia nowej podsieci podczas tworzenia ASE.  **Nie można użyć wstępnie zaprojektowanych podsieci w portalu.  Jeśli tworzysz usługi ASE przy użyciu szablonu Menedżera zasobów, możesz utworzyć ASE z istniejącym podsieci.**  Do utworzenia ASE z Użyj szablonu tych informacji w tym miejscu [tworzenia w środowisku usługi aplikacji z szablonu] [ ILBAseTemplate] i tutaj [tworzenia środowisku ILB aplikacji usługi z szablonu][ASEfromTemplate].

### <a name="details"></a>Szczegóły ###

ASE zostanie utworzona i 2 frontonu i 2 pracowników.  Frontonu pełnić rolę punkty końcowe protokołu HTTP/HTTPS i wysyłać ruch do pracowników, które są role, mające obsługi aplikacji.   Możesz dostosować ilość po utworzeniu ASE i można nawet Ustawianie reguł autoscale na tych pul zasobów.  Szczegółowe wokół ręcznego skalowania, zarządzanie i monitorowanie środowisku usługi aplikacji przejdź tutaj: [jak skonfigurować środowisko usługi aplikacji][ASEConfig] 

W podsieci używane przez ASE może istnieć tylko jeden ASE.  Nie można użyć podsieci cokolwiek innego niż ASE

### <a name="after-app-service-environment-creation"></a>Po utworzeniu środowisko usługi aplikacji ###

Po utworzeniu ASE można dostosować.

- Liczba frontonu (minimalna: 2)
- Liczba pracowników (minimalna: 2)
- Liczba adresów IP dostępnych dla adresów IP protokołu SSL
- Obliczanie rozmiarów zasobów używanych przez frontonu lub pracowników (minimalny rozmiar fronton jest P2)


Istnieje szczegółowe wokół ręczne skalowania, zarządzanie i monitorowanie aplikacji usługi środowiskach tutaj: [jak skonfigurować środowisko usługi aplikacji][ASEConfig] 

Uzyskać informacji na temat autoscaling jest przewodnik tutaj: [jak skonfigurować autoscale w środowisku usługi aplikacji][ASEAutoscale]

Istnieją dodatkowe zależności, które nie są dostępne dla dostosowania, takich jak bazy danych i miejsca do magazynowania.  Te są obsługiwane przez Azure i pochodzić z systemem.  Magazyn systemu obsługuje maksymalnie 500 GB dla całego środowiska usługi aplikacji i bazy danych zostanie dopasowana, Azure odpowiednio skali systemu.


## <a name="getting-started"></a>Wprowadzenie
Wszystkie artykuły i w jaki sposób — aby rekordami dla środowiska usługi aplikacji są dostępne w [pliku README dla środowiska usługi aplikacji](../app-service/app-service-app-service-environments-readme.md).

Aby rozpocząć pracę ze środowiskami usługi aplikacji, zobacz [Wprowadzenie do środowiska usługi aplikacji][WhatisASE]

Aby uzyskać więcej informacji na temat platformy Azure aplikacji usługi zobacz [Azure aplikacji usługi][AzureAppService].

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]
 

<!--Image references-->
[1]: ./media/app-service-web-how-to-create-an-app-service-environment/asecreate-basecreateblade.png
[2]: ./media/app-service-web-how-to-create-an-app-service-environment/asecreate-vnetcreation.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[ASEConfig]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/ 
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[ILBASE]: http://azure.microsoft.com/documentation/articles/app-service-environment-with-internal-load-balancer/
[ILBAseTemplate]: http://azure.microsoft.com/documentation/templates/201-web-app-ase-create/
[ASEfromTemplate]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-create-ilb-ase-resourcemanager/
