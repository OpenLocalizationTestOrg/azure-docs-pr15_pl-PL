<properties
    pageTitle="Usługi federacyjne Active Directory platformy Azure | Microsoft Azure"
    description="W tym dokumencie dowiesz się, jak wdrożyć serwer usług AD FS platformy Azure dla wysokiej dostępności."
    keywords="Wdrażanie usług AD FS platformy azure, wdrażanie azure adfs, azure adfs, azure ad fs, wdrażanie usług adfs i wdrażanie usług ad fs adfs w azure, wdrażanie usług adfs platformy azure, wdrażanie usług AD FS azure, adfs azure i wprowadzenie do usług AD FS, Azure AD FS w Azure, iaas, ADFS, Przenieś adfs Azure"
    services="active-directory"
    documentationCenter=""
    authors="anandyadavmsft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/03/2016"
    ms.author="anandy;billmath"/>

# <a name="ad-fs-deployment-in-azure"></a>AD FS wdrażania platformy Azure 

Program AD FS udostępnia federacji tożsamości uproszczone, bezpiecznej i możliwości logowania jednokrotnego (SSO), Web. Federacja z usługą Azure Active Directory lub usługi Office 365 umożliwia użytkownikom uwierzytelnienia przy użyciu poświadczeń lokalnego i dostęp do wszystkich zasobów w chmurze. W wyniku staje się istotne wysokiej dostępności infrastruktury usług AD FS w celu zapewnienia dostępu do zasobów zarówno w lokalnej i w chmurze. Wdrażanie usług AD FS platformy Azure mogą pomóc osiągnąć wysoką dostępność wymagane z minimalnymi działań.
Istnieje kilka korzyści wynikających z wdrożenia usług AD FS platformy Azure, niektóre z nich są wymienione poniżej:

* **Wysoce** - z możliwości zestawy dostępność Azure, można zapewnić wysoce dostępnej infrastruktury.
* **Łatwy skali** — potrzebujesz więcej wydajności? Łatwe migrowanie na komputerach bardziej zaawansowanych za pomocą kilku kliknięć platformy Azure
* **Nadmiarowości Geo krzyżowe** — z Azure Geo nadmiarowości można mieć pewność, że infrastruktury jest wysoce dostępna na całym świecie
* **Łatwy Zarządzanie** — przy użyciu opcji wysoce uproszczone zarządzanie w Azure portal zarządzania infrastrukturą jest bardzo proste i łatwy 

## <a name="design-principles"></a>Zasady projektowania

![Projekt wdrożenia](./media/active-directory-aadconnect-azure-adfs/deployment.png)

Na powyższym diagramie przedstawiono zalecany Topologia podstawowa zacząć wdrażanie infrastruktury programu AD FS platformy Azure. Poniżej wymieniono zasady za różne składniki topologii:

* **Kontrolera domeny i serwery ADFS**: Jeśli masz mniej niż 1000 użytkowników możesz po prostu zainstalować roli usług AD FS na kontrolerach domeny. Jeśli nie ma żadnego wpływu wydajności na kontrolerach domeny lub jeśli masz ponad 1000 użytkowników, wdrażanie usług AD FS na różnych serwerach.
* **Serwer WAP** — jest to konieczne do wdrożenia serwerów Proxy aplikacji sieci Web tak, aby użytkownicy mogą korzystać z AD FS, gdy znajdują się one w sieci firmowej również.
* **Strefy Zdemilitaryzowanej**: serwery Proxy aplikacji sieci Web zostaną umieszczone w strefy Zdemilitaryzowanej i między strefy Zdemilitaryzowanej i podsieci wewnętrznej jest dozwolony dostęp tylko port TCP/443.
* **Urządzenia do równoważenia obciążenia**: w celu zapewnienia wysokiej dostępności serwer usług AD FS i serwer Proxy aplikacji sieci Web, zaleca się używanie równoważenia obciążenia wewnętrznych dla serwerów usług AD FS i usługi równoważenia obciążenia Azure dla serwerów Proxy aplikacji sieci Web.
* **Zestawy dostępność**: zapewnienie nadmiarowości w wdrożenia usług AD FS, zaleca się grupowanie dwóch lub więcej maszyn wirtualnych w dostępność ustawieniem podobne obciążenia. Ta konfiguracja gwarantuje, że podczas albo zdarzenie niezaplanowane lub planowanej konserwacji, co najmniej jedna maszyna wirtualna będzie dostępny
* **Konta miejsca do magazynowania**: zaleca się dwa konta miejsca do magazynowania. Masz konto jednego miejsca do magazynowania może prowadzić do utworzenia pojedynczego punktu awarii i mogą spowodować rozmieszczania stają się niedostępne w scenariuszu prawdopodobne miejsce, w którym konta miejsca do magazynowania awarii. Dwa miejsca do magazynowania konta może pomóc w skojarzyć jednego konta miejsca do magazynowania dla każdego wiersza błędów.
* **Sieć podziału**: serwery Proxy aplikacji sieci Web powinny zostać wdrożone w osobnej sieci strefy Zdemilitaryzowanej. Można podzielić jeden wirtualną sieć na dwóch podsieci, a następnie wdrożyć serwery Proxy aplikacji usług sieci Web w odizolowanych podsieci. Po prostu można konfigurować ustawienia grupy zabezpieczeń sieci dla każdej podsieci i Zezwalaj tylko wymagane komunikacji między dwoma podsieci. Na scenariusz wdrażania poniżej podano więcej szczegółów

##<a name="steps-to-deploy-ad-fs-in-azure"></a>Kroki wdrażania usług AD FS platformy Azure

Kroki wymienione w tej sekcji konspektu Podręcznik wdrożenia poniżej przedstawiono infrastruktury usług AD FS Azure.

### <a name="1-deploying-the-network"></a>1. wdrażanie sieci

Zgodnie z powyższym możesz można albo tworzenie dwóch podsieci w jednej wirtualnej sieci albo utwórz dwa całkowicie różne wirtualnych sieci (VNet). Ten artykuł będzie skoncentrowanie się na wdrażanie pojedynczy wirtualną sieć i podzielić je na dwa podsieci. Obecnie jest łatwiejsze podejście zgodnie z dwóch oddzielnych VNets wymaga VNet do bramy VNet komunikacji.

**1.1 Tworzenie wirtualnych sieci**

![Tworzenie wirtualnych sieci](./media/active-directory-aadconnect-azure-adfs/deploynetwork1.png)
    
W portalu usługi Azure wybierz wirtualnej sieci i można wdrażać wirtualnej sieci i jednej podsieci natychmiastowe jednym kliknięciem. INT podsieci, jest również zdefiniowany i jest teraz gotowy, do maszyny wirtualne do dodania.
Następnym krokiem jest dodanie innej podsieci do sieci, to znaczy podsieci strefy Zdemilitaryzowanej. Aby utworzyć podsieci strefy Zdemilitaryzowanej, po prostu

* Zaznacz nowo utworzony sieci
* W oknie właściwości wybierz podsieci
* W tej podsieci panelu kliknij przycisk Dodaj
* Podaj podsieci nazwisko i adres miejsca informacji w celu utworzenia podsieci

![Podsieci](./media/active-directory-aadconnect-azure-adfs/deploynetwork2.png)


![Podsieci strefy Zdemilitaryzowanej](./media/active-directory-aadconnect-azure-adfs/deploynetwork3.png)

**1.2. utworzenie sieci grupy zabezpieczeń**

Grupy zabezpieczeń sieci (NSG) zawiera listę reguł listy kontroli dostępu (ACL), które umożliwiają lub odmówić ruch sieciowy usługi wystąpieniach maszyn wirtualnych w wirtualnej sieci. NSGs może być skojarzone z podsieci lub poszczególne wystąpienia maszyn wirtualnych w tej podsieci. Gdy NSG jest skojarzony z podsieci, reguły ACL dotyczą wszystkich wystąpień maszyn wirtualnych w tej podsieci.
Do tych wskazówek, zostanie utworzony dwóch NSGs: jeden dla sieci wewnętrznej i strefy Zdemilitaryzowanej. Ta osoba będzie oznaczone NSG_INT i NSG_DMZ odpowiednio.

![Tworzenie NSG](./media/active-directory-aadconnect-azure-adfs/creatensg1.png)

Po utworzeniu NSG będzie 0 przychodzących i 0 reguły ruchu wychodzącego. Po zainstalowaniu i funkcjonalności ról na poszczególnych serwerach, reguły przychodzące i wychodzące, mogą zostać wprowadzone zgodnie z żądany poziom zabezpieczeń.

![Inicjowanie NSG](./media/active-directory-aadconnect-azure-adfs/nsgint1.png)

Po utworzeniu NSGs skojarzyć NSG_INT podsieci INT i NSG_DMZ podsieci strefy Zdemilitaryzowanej. Zrzut ekranu jest podany poniżej:

![Konfigurowanie NSG](./media/active-directory-aadconnect-azure-adfs/nsgconfigure1.png)

* Kliknij pozycję podsieci, aby otworzyć panel dla podsieci
* Wybierz pozycję podsieci, które chcesz skojarzyć NSG 

Po konfiguracji panel dla podsieci powinna wyglądać podobnie do poniżej:

![Podsieci po NSG](./media/active-directory-aadconnect-azure-adfs/nsgconfigure2.png)

**1.3. Tworzenie połączenia do lokalnego**

Aby wdrożyć domeny kontrolera platformy azure są wymagane połączenia do lokalnego. Azure oferuje różne opcje łączności nawiązać infrastruktury w lokalnej infrastrukturze Azure.

* Punkt do witryny
* Wirtualna sieć witryny do witryny
* ExpressRoute

Zaleca się używanie ExpressRoute. ExpressRoute pozwala na tworzenie prywatnych połączeń między Azure centrach danych oraz infrastrukturę w swojej siedzibie lub w środowisku Współtworzenie lokalizacji. ExpressRoute połączenia nie przechodzą publicznie w Internecie. Oferują więcej niezawodności, szybkości szybciej, dolnym opóźnienia i zabezpieczeń wyższymi niż typowy połączenia przez Internet.
Podczas zaleca się używanie ExpressRoute, można wybrać dowolny metody połączenia, najlepiej dopasowane do Twojej organizacji. Aby dowiedzieć się więcej na temat ExpressRoute i różnych opcji łączności przy użyciu ExpressRoute, przeczytaj [Omówienie kwestii technicznych ExpressRoute](https://aka.ms/Azure/ExpressRoute).

### <a name="2-create-storage-accounts"></a>2. Utwórz konta miejsca do magazynowania

Aby zachować wysoką dostępność i uniknąć zależność na koncie jednego miejsca do magazynowania, można utworzyć dwa konta miejsca do magazynowania. Komputery w każdym zestawie dostępność należy podzielić na dwie grupy, a następnie przypisać poszczególnych grup konta oddzielnie.

![Tworzenie kont miejsca do magazynowania](./media/active-directory-aadconnect-azure-adfs/storageaccount1.png)

### <a name="3-create-availability-sets"></a>3. Tworzenie zestawów dostępności

Dla każdej roli (kontrolera domeny i AD FS i WAP) tworzenie zestawów dostępność, zawierające 2 maszyn co najmniej. Dzięki temu osiągnąć większą dostępność dla poszczególnych ról. Podczas tworzenia dostępność zestawy, konieczne jest decyduje o następujących czynności:
* **Błąd domen**: maszyn wirtualnych w tej samej domeny błędów udostępnianie przełącznik sieci fizycznej i tym samym zasilania. Zaleca się co najmniej 2 domen błędów. Wartość domyślna to 3 i możesz pozostawić go jako odbywa się w celu wdrażania
* **Aktualizowanie domen**: komputerów należących do tej samej domeny aktualizacji ponownego uruchomienia razem podczas aktualizacji. Chcesz mieć co najmniej 2 domen aktualizacji. Wartość domyślna to 5 i możesz pozostawić go jako odbywa się w celu wdrażania

![Zestawy dostępności](./media/active-directory-aadconnect-azure-adfs/availabilityset1.png)

Tworzenie następujących zestawów dostępności

| Konfigurowanie dostępności | Rola | Błąd domen | Aktualizowanie domen |
|:----------------:|:----:|:-----------:|:-----------|
| contosodcset | KONTROLERA DOMENY I ADFS | 3 | 5 |
| contosowapset | WAP | 3 | 5 |

### <a name="4--deploy-virtual-machines"></a>4. wdrażanie maszyn wirtualnych
Następnym krokiem jest wdrożenie maszyn wirtualnych, obsługujące różne role w infrastrukturze. Co najmniej dwa komputery zaleca się w każdym zestawie dostępności. Tworzenie sześć maszyn wirtualnych wdrożenia podstawowe.

| Komputera | Rola | Podsieci | Konfigurowanie dostępności | Konto miejsca do magazynowania | Adres IP |
|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|
|contosodc1|KONTROLERA DOMENY I ADFS|INT|contosodcset|contososac1|Statyczne|
|contosodc2|KONTROLERA DOMENY I ADFS|INT|contosodcset|contososac2|Statyczne|
|contosowap1|WAP|STREFY ZDEMILITARYZOWANEJ|contosowapset|contososac1|Statyczne|
|contosowap2|WAP|STREFY ZDEMILITARYZOWANEJ|contosowapset|contososac2|Statyczne|

Jak można zauważyć, nie NSG określono. Jest to spowodowane azure umożliwia NSG na poziomie podsieci. Następnie możesz sterować ruch sieciowy komputera przy użyciu poszczególnych NSG, skojarzone z albo podsieci, czyli obiektu NIC. Przeczytaj więcej o [Co to jest grupa zabezpieczeń sieci (NSG)](https://aka.ms/Azure/NSG).
Jeśli zarządzasz DNS, zaleca się statyczny adres IP. Można użyć Azure DNS i zamiast tego w rekordach DNS dla swojej domeny, odwołują się do nowego komputerach przez ich nazwy FQDN Azure.
Swoje okienko maszyn wirtualnych powinna wyglądać podobnie do poniżej po zakończeniu rozmieszczania:

![Maszyn wirtualnych wdrożony](./media/active-directory-aadconnect-azure-adfs/virtualmachinesdeployed_noadfs.png)

### <a name="5-configuring-the-domain-controller--ad-fs-servers"></a>5. Konfigurowanie kontrolera domeny i serwer usług AD FS
 W celu uwierzytelnienia wszystkie przychodzące żądanie, usług AD FS należy skontaktować się kontrolera domeny. Aby zapisać kosztów podróży z platformy Azure kontrolera domeny lokalnego uwierzytelniania, zaleca się wdrażanie replice kontrolera domeny w Azure. W celu osiągnięcia wysokiej dostępności, zaleca się utworzenie zestawu dostępności kontrolerów domeny 2 co najmniej.

|Kontrolera domeny|Rola|Konto miejsca do magazynowania|
|:-----:|:-----:|:-----:|
|contosodc1|Replice|contososac1|
|contosodc2|Replice|contososac2|

* Promowanie dwa serwery jako replice kontrolery domeny z usługą DNS
* Skonfiguruj serwer usług AD FS instalując roli usług AD FS za pomocą Menedżera serwera.

###<a name="6---deploying-internal-load-balancer-ilb"></a>6. wdrażanie usługi równoważenia obciążenia wewnętrzny (ILB)

**6.1. ILB tworzenie**

Aby wdrożyć ILB, wybierz pozycję urządzenia do równoważenia obciążenia w Azure portal i kliknij przycisk Dodaj (+).
>[AZURE.NOTE] Jeśli nie ma **Urządzenia do równoważenia obciążenia** w menu, kliknij przycisk **Przeglądaj** w lewym dolnym rogu portalu i przewiń do momentu wyświetlenia **Urządzenia do równoważenia obciążenia**.  Kliknij pozycję żółta gwiazdka, aby dodać go do menu. Teraz wybierz nową ikonę równoważenia obciążenia, aby otworzyć panel, aby rozpocząć konfigurowanie usługi równoważenia obciążenia.

![Przeglądanie równoważenia obciążenia](./media/active-directory-aadconnect-azure-adfs/browseloadbalancer.png)

* **Nazwa**: odpowiednią nazwę do równoważenia obciążenia
* **Program**: ponieważ ten równoważenia obciążenia zostaną umieszczone przed serwer usług AD FS i jest przeznaczona dla połączenia sieci wewnętrznej, zaznacz "Wewnętrznego"
* **Wirtualna sieć**: wybierz pozycję wirtualnej sieci, w której są wdrażanie usługi usług AD FS
* **Podsieci**: Wybierz tutaj podsieci wewnętrznej
* **Przypisywanie adresów IP**: dynamiczne

![Usługi równoważenia obciążenia wewnętrznych](./media/active-directory-aadconnect-azure-adfs/ilbdeployment1.png)
 
Po kliknięciu przycisku Utwórz i wdrożeniu ILB, znajdą na liście urządzenia do równoważenia obciążenia:

![Urządzenia do równoważenia obciążenia po ILB](./media/active-directory-aadconnect-azure-adfs/ilbdeployment2.png)
 
Następnym krokiem jest skonfigurowanie puli wewnętrznej bazy danych i sondy wewnętrznej bazy danych.

**6.2. Konfigurowanie ILB puli wewnętrznej bazy danych**

Zaznacz ILB nowo utworzonego w panelu urządzenia do równoważenia obciążenia. Zostanie otwarty panel ustawień. 
1.  Wybierz pozycję Pule wewnętrznej bazy danych przy użyciu panelu Ustawienia
2.  W panelu puli Dodaj wewnętrznej bazy danych kliknij polecenie Dodaj maszyn wirtualnych
3.  Zostanie wyświetlona panelem miejsce, w którym można wybrać zestaw dostępności
4.  Wybierz zestaw dostępność usług AD FS

![Konfigurowanie ILB puli wewnętrznej bazy danych](./media/active-directory-aadconnect-azure-adfs/ilbdeployment3.png)
 
**6.3. sonda Konfigurowanie**

W panelu Ustawienia ILB wybierz sondy.
1.  Kliknij przycisk Dodaj
2.  Podać szczegóły dotyczące sondy. **Nazwa**: sondy b nazwy. **Protocol (protokół)**: TCP c. **Port**: d 443 (HTTPS). **Interwał**: 5 (wartość domyślna) — jest to interwału, jaką ILB będzie sondy maszyny w puli e wewnętrznej bazy danych. **Limit liczby wyświetlanych elementów nieprawidłowe**: 2 (domyślnie val ue) — jest to progu sonda następujących po sobie błędów, po upływie którego ILB będzie deklarować komputera w puli wewnętrznej bazy danych nie odpowiada, a Zatrzymaj wysyłanie ruch do niego.

![Konfigurowanie sondy ILB](./media/active-directory-aadconnect-azure-adfs/ilbdeployment4.png)
 
**6.4. Tworzenie reguł równoważenia obciążenia**

Aby efektywnie saldo ruchu, ILB należy skonfigurować przy użyciu reguł równoważenia obciążenia. Aby utworzyć reguły, równoważenia obciążenia 
1.  Wybierz pozycję reguły w panelu Ustawienia ILB równoważenia obciążenia
2.  Wybierz polecenie Dodaj w panelu reguły równoważenia obciążenia
3.  W panelu reguły równoważenia obciążenia Dodaj. **Nazwa**: Podaj nazwę dla reguły b. **Protocol (protokół)**: wybierz pozycję TCP c. **Port**: 443 d. **Port wewnętrznej bazy danych**: 443 e. **Pula wewnętrznej bazy danych**: Zaznacz pulę dla usług AD FS klaster wcześniejszych f utworzono. **Sonda**: wybierz pozycję sonda utworzony wcześniej dla serwerów usług AD FS

![Konfigurowanie ILB równoważenia reguł](./media/active-directory-aadconnect-azure-adfs/ilbdeployment5.png)

**6.5. Aktualizuj DNS z ILB**

Przejdź do swojego serwera DNS i tworzenie CNAME dla ILB. CNAME powinny być usługi federacyjnej z adresem IP wskazujący na adres IP ILB. Na przykład jeśli adres ILB DIP to 10.3.0.8 i usługa federacyjna zainstalowany jest fs.contoso.com, następnie utwórz CNAME dla fs.contoso.com wskazująca 10.3.0.8.
Daje to pewność, że wszystkie komunikacji dotyczące zakończenia fs.contoso.com w górę w ILB i odpowiednio rozsyłania.

###<a name="7---configuring-the-web-application-proxy-server"></a>7. Konfigurowanie serwera Proxy aplikacji sieci Web

**7.1. Konfigurowanie serwerów Proxy aplikacji sieci Web do osiągnięcia serwer usług AD FS**

Aby upewnić się, że serwerów Proxy aplikacji sieci Web będą mogli uzyskać dostęp do serwerów usług AD FS za ILB, Utwórz rekord w %systemroot%\system32\drivers\etc\hosts dla ILB. Należy zauważyć, że nazwa wyróżniająca (nazwy domeny) powinny być nazwą usługi federacyjne, na przykład fs.contoso.com. I wpis IP powinno być ILB adres IP (10.3.0.8, tak jak w przykładzie).

**7.2. Instalowanie roli serwera Proxy aplikacji sieci Web**

Po upewnieniu się, że serwerów Proxy aplikacji sieci Web będą mogli uzyskać dostęp do serwerów usług AD FS za ILB, możesz zainstalować dalej serwerów Proxy aplikacji sieci Web. Serwery Proxy aplikacji sieci Web nie można dołączyć do domeny. Instalowanie ról serwera Proxy aplikacji sieci Web na dwa serwery Proxy aplikacji sieci Web, wybierając pozycję dostęp zdalny do roli. Menedżer serwera przeprowadzi Cię do ukończenia instalacji WAP.
Aby uzyskać więcej informacji na temat instalacji WAP przeczytaj [Instalowanie i konfigurowanie serwera Proxy aplikacji sieci Web](https://technet.microsoft.com/library/dn383662.aspx).

###<a name="8---deploying-the-internet-facing-public-load-balancer"></a>8. wdrażanie, Internet przeciwległych równoważenia obciążenia (publiczne)

**8.1. Tworzenie dostępnych równoważenia obciążenia (publiczne) przez Internet**
 
W portalu Azure wybierz pozycję urządzenia do równoważenia obciążenia, a następnie kliknij polecenie Dodaj. W panelu Utwórz równoważenia obciążenia wprowadź następujące informacje
1. **Nazwa**: nazwę równoważenia obciążenia
2. **Schemat**: publicznej — ta opcja informuje Azure, że ten równoważenia obciążenia muszą publiczny adres.
3. **Adres IP**: Utwórz nowy adres IP (dynamiczny)

![Internet przeciwległych równoważenia obciążenia](./media/active-directory-aadconnect-azure-adfs/elbdeployment1.png)

Po wdrożeniu równoważenia obciążenia pojawi się na liście urządzenia do równoważenia obciążenia.

![Lista równoważenia obciążenia](./media/active-directory-aadconnect-azure-adfs/elbdeployment2.png)
 
**8.2. przypisać etykietę DNS publiczny adres IP**

Kliknij wpis równoważenia obciążenia nowo utworzonego w panelu urządzenia do równoważenia obciążenia, aby wyświetlić panel konfiguracji. Wykonaj kroki w celu skonfigurowania Etykieta DNS dla publiczny adres IP:
1.  Kliknij pozycję na publiczny adres IP. Spowoduje to otwarcie panelu publiczny adres IP i ustawienia
2.  Kliknij pozycję od konfiguracji
3.  Podaj etykietę DNS. Będzie publicznej Etykieta DNS, którego mają dostęp z dowolnego miejsca, na przykład contosofs.westus.cloudapp.azure.com. Możesz dodać wpis w zewnętrznym systemie DNS usługi federacyjne (na przykład fs.contoso.com), który jest rozpoznawana jako etykieta DNS równoważenia obciążenia zewnętrznych (contosofs.westus.cloudapp.azure.com).

![Konfigurowanie usługi równoważenia obciążenia dostępnych przez internet](./media/active-directory-aadconnect-azure-adfs/elbdeployment3.png) 

![Konfigurowanie, internet przeciwległych równoważenia obciążenia (DNS)](./media/active-directory-aadconnect-azure-adfs/elbdeployment4.png)

**8.3. Konfigurowanie puli wewnętrznej bazy danych dla równoważenia obciążenia Internet przeciwległych (publiczna)** 

Wykonaj te same kroki, tak jak w tworzeniu równoważenia obciążenia wewnętrznych, aby skonfigurować puli wewnętrznej bazy danych dla przeciwległych Internet (publiczne) usługi równoważenia obciążenia jako dostępność, ustaw dla serwerów WAP. Na przykład contosowapset.

![Konfigurowanie puli równoważenia obciążenia przeciwległych Internet wewnętrznej bazy danych](./media/active-directory-aadconnect-azure-adfs/elbdeployment5.png)
 
**8,4. sonda Konfigurowanie**

Wykonaj te same kroki, tak jak konfigurowanie usługi równoważenia obciążenia wewnętrznych skonfigurowanie sondy serwerów WAP puli wewnętrznej bazy danych.

![Konfigurowanie sondy równoważenia obciążenia przeciwległych Internet](./media/active-directory-aadconnect-azure-adfs/elbdeployment6.png)
 
**8,5. Tworzenie reguł równoważenia obciążenia**

Wykonaj te same kroki, tak jak ILB skonfigurowanie równoważenia obciążenia regułę dla port TCP 443.

![Konfigurowanie równoważenia reguł równoważenia obciążenia przeciwległych Internet](./media/active-directory-aadconnect-azure-adfs/elbdeployment7.png)
 
###<a name="9---securing-the-network"></a>9. Zabezpieczanie sieci

**9.1. zabezpieczanie podsieci wewnętrznej**

Ogólnie mówiąc potrzebujesz następujących reguł efektywne zapewnienie wewnętrznej podsieci (w kolejności, w sposób opisany poniżej)

|Reguły|Opis|Przepływ|
|:----|:----|:------:|
|AllowHTTPSFromDMZ| Zezwalaj na komunikację HTTPS z strefy Zdemilitaryzowanej | Ruch przychodzący |
|DenyInternetOutbound| Brak dostępu do Internetu | Wychodzące |

![Reguły dostępu INT (ruch przychodzący)](./media/active-directory-aadconnect-azure-adfs/nsg_int.png)

[komentarz]: <> (![reguły dostępu INT (ruch przychodzący)](./media/active-directory-aadconnect-azure-adfs/nsgintinbound.png)) [komentarz]: <> (![reguły dostępu INT (wychodzące)](./media/active-directory-aadconnect-azure-adfs/nsgintoutbound.png))
 
**9.2. podsieć strefy Zdemilitaryzowanej Zabezpieczanie**

|Reguły|Opis|Przepływ|
|:----|:----|:------:|
|AllowHTTPSFromInternet| Zezwól HTTPS z Internetu na strefy Zdemilitaryzowanej | Ruch przychodzący|
|DenyInternetOutbound|  Więcej niż HTTPS Internetem jest zablokowane | Wychodzące |

![Rozszerzenie reguły dostępu (ruch przychodzący)](./media/active-directory-aadconnect-azure-adfs/nsg_dmz.png)

[komentarz]: <> (![reguły dostępu rozszerzenie (ruch przychodzący)](./media/active-directory-aadconnect-azure-adfs/nsgdmzinbound.png)) [komentarz]: <> (![reguły dostępu rozszerzenie (wychodzące)](./media/active-directory-aadconnect-azure-adfs/nsgdmzoutbound.png))

>[AZURE.NOTE] Jeśli użytkownik klienta certyfikatu uwierzytelniania (uwierzytelnianie z klienckim protokołem TLS przy użyciu X509 certyfikaty użytkowników) jest wymagane, a następnie TCP wymaga usług AD FS portu 49443 można włączyć dla przychodzących połączeń dostępu.

###<a name="10--test-the-ad-fs-sign-in"></a>10. logowania test usług AD FS

Najprostszym sposobem jest, aby przetestować usług AD FS jest za pomocą strony IdpInitiatedSignon.aspx. Aby móc wykonać, że jest niezbędne w celu umożliwienia IdpInitiatedSignOn we właściwościach usług AD FS. Postępuj zgodnie z instrukcjami poniżej, aby sprawdzić ustawienia usług AD FS
1.  Uruchamianie poniżej polecenia cmdlet na serwerze usług AD FS, włącz je przy użyciu programu PowerShell.
    Ustawianie AdfsProperties - EnableIdPInitiatedSignonPage $true 
2.  Z dowolnego https://adfs.thecloudadvocate.com/adfs/ls/IdpInitiatedSignon.aspx dostępu zewnętrznego komputera  
3.  Powinien zostać wyświetlony na stronie usług AD FS, takich jak poniżej:

![Strona logowania test](./media/active-directory-aadconnect-azure-adfs/test1.png)

Na pomyślne logowania jego umożliwi komunikat o powodzeniu tak jak pokazano poniżej:

![Testowanie sukcesu](./media/active-directory-aadconnect-azure-adfs/test2.png)

## <a name="template-for-deploying-ad-fs-in-azure"></a>Szablon do wdrażania usług AD FS platformy Azure

Szablon wdraża Konfiguracja komputera 6, 2 kontrolerów domeny, usług AD FS i WAP.

[Usług AD FS w szablonie Azure wdrażania](https://github.com/paulomarquesc/adfs-6vms-regular-template-based)

Można używać istniejących wirtualną sieć lub utworzyć nowe VNET podczas wdrażania tego szablonu. Poniżej przedstawiono różnych parametrów dostępnych do dostosowywania wdrażanie opis użycia parametru w procesie wdrażania. 

| Parametr | Opis |
|:--------|:-----|
|Lokalizacja| Region wdrożenia zasobów na przykład wschód US. |
|StorageAccountType| Typ konta miejsca do magazynowania|
|VirtualNetworkUsage| Wskazuje, jeśli nowy wirtualna sieć zostanie utworzona lub użycie istniejącej|
|VirtualNetworkName| Nazwa wirtualną sieć do utworzenia obowiązkowe na zarówno istniejące czy nowe zastosowania wirtualnej sieci|
|VirtualNetworkResourceGroupName| Nazwa grupy zasobów miejsce, w którym znajduje się istniejących wirtualnej sieci. Podczas korzystania z istniejącą sieć wirtualną, staje się on obowiązkowy tak rozmieszczenia można znaleźć identyfikator istniejącego wirtualną sieć|
|VirtualNetworkAddressRange| Zakres adres nowego VNET, obowiązkowe tworzenia nowego wirtualną sieć|
|InternalSubnetName| Nazwa podsieci wewnętrznej obowiązkowe na obu opcji zastosowania wirtualną sieć (nowego lub istniejącego)|
|InternalSubnetAddressRange| Zakres adres podsieci wewnętrznych zawiera kontrolery domeny i ADFS serwerów, obowiązkowe, tworzenia nowego wirtualną sieć.|
|DMZSubnetAddressRange| Zakres adres podsieci strefy zdemilitaryzowanej zawiera Windows aplikacji serwery proxy, obowiązkowe, tworzenia nowego wirtualnej sieci.|
|DMZSubnetName| Nazwa podsieci wewnętrznej obowiązkowe na obu opcji zastosowania wirtualną sieć (nowego lub istniejącego). |
|ADDC01NICIPAddress| Wewnętrzny adres IP pierwszego kontrolera domeny, ten adres IP statycznie są przypisywane do kontrolera domeny, musi być prawidłowego adresu ip w wewnętrznych podsieci|
|ADDC02NICIPAddress| Wewnętrzny adres IP kontrolera domeny, ten adres IP statycznie są przypisywane do kontrolera domeny, musi być prawidłowego adresu ip w wewnętrznych podsieci|
|ADFS01NICIPAddress| Wewnętrzny adres IP pierwszego serwera ADFS ten adres IP powoduje przypisanie statycznie na serwerze usług ADFS, muszą być prawidłowego adresu ip w wewnętrznych podsieci|
|ADFS02NICIPAddress| Wewnętrzny adres IP drugi serwer usług ADFS, ten adres IP powoduje przypisanie statycznie na serwerze usług ADFS, muszą być prawidłowego adresu ip w wewnętrznych podsieci|
|WAP01NICIPAddress| Wewnętrzny adres IP pierwszego serwera WAP powoduje przypisanie statycznie na serwerze WAP ten adres IP, musi być prawidłowego adresu ip w podsieci strefy Zdemilitaryzowanej|
|WAP02NICIPAddress| Wewnętrzny adres IP drugi serwer WAP, zostanie przypisany statycznie na serwerze WAP ten adres IP, musi być prawidłowego adresu ip w podsieci strefy Zdemilitaryzowanej|
|ADFSLoadBalancerPrivateIPAddress| Wewnętrzny adres IP usługi równoważenia obciążenia ADFS, ten adres IP statycznie są przypisywane do równoważenia obciążenia, muszą być prawidłowego adresu ip w wewnętrznych podsieci|
|ADDCVMNamePrefix| Prefiks nazwy maszyn wirtualnych kontrolerów domeny|
|ADFSVMNamePrefix| Prefiks nazwy maszyn wirtualnych dla serwerów usług ADFS organizacji|
|WAPVMNamePrefix| Prefiks nazwy maszyn wirtualnych dla serwerów WAP|
|ADDCVMSize| Rozmiar pamięci wirtualnej kontroler domeny|
|ADFSVMSize| Rozmiar pamięci wirtualnej serwerze usług ADFS organizacji|
|WAPVMSize| Rozmiar pamięci wirtualnej serwerów WAP|
|AdminUserName| Nazwę administratora lokalnego maszyn wirtualnych|
|AdminPassword| Hasło lokalnego konta administratora maszyn wirtualnych|

## <a name="additional-resources"></a>Dodatkowe zasoby
* [Zestawy dostępności](https://aka.ms/Azure/Availability ) 
* [Usługi równoważenia obciążenia Azure](https://aka.ms/Azure/ILB)
* [Usługi równoważenia obciążenia wewnętrznych](https://aka.ms/Azure/ILB/Internal)
* [Usługi równoważenia obciążenia witrynie internetowej](https://aka.ms/Azure/ILB/Internet)
* [Konta miejsca do magazynowania](https://aka.ms/Azure/Storage )
* [Azure wirtualnych sieci](https://aka.ms/Azure/VNet)
* [Usług AD FS i łącza serwera Proxy aplikacji sieci Web](http://aka.ms/ADFSLinks) 

## <a name="next-steps"></a>Następne kroki

* [Integrowanie tożsamości lokalnych z usługi Azure Active Directory](active-directory-aadconnect.md)
* [Konfigurowanie i zarządzanie nimi z usług AD FS, za pomocą narzędzie Azure AD Connect](active-directory-aadconnectfed-whatis.md)
* [Wysoką dostępność geograficzne krzyżowe AD FS wdrożenia w Azure przy użyciu Menedżera ruch Azure](active-directory-adfs-in-azure-with-azure-traffic-manager.md)




