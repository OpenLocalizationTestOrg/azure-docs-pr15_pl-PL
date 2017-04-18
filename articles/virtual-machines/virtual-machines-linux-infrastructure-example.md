<properties
    pageTitle="Przykład infrastruktury Instruktaż | Microsoft Azure"
    description="Informacje na temat ważnych wskazówek projektowanie i wdrażanie wdrażania infrastruktury przykład Azure."
    documentationCenter=""
    services="virtual-machines-linux"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="example-azure-infrastructure-walkthrough"></a>Przykład infrastruktury Azure Instruktaż

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)] 

W tym artykule opisano konstrukcyjnych przykład infrastruktury aplikacji. Firma Microsoft szczegółowo projektowania infrastruktury prosty magazynu online, która łączy wszystkie wskazówki i decyzji wokół konwencje nazewnictwa, zestawy dostępność, wirtualnych sieci i urządzenia do równoważenia obciążenia i faktycznie wdrażania usługi wirtualnych maszyn.


## <a name="example-workload"></a>Przykład obciążenie pracą

Adventure Works cykli chce utworzyć aplikację ze sklepu online w Azure składający się z:

- Dwa serwery nginx klientach zewnętrzną w warstwie sieci web
- Dwa serwery nginx przetwarzania danych i zamówienia w warstwie aplikacji
- Dwie części serwery MongoDB klastrze sharded do przechowywania danych produktów i zamówienia w warstwie bazy danych
- Dwa kontrolery domen usługi Active Directory dla kont odbiorców i dostawców w warstwie uwierzytelniania
- Wszystkie serwery znajdują się w dwóch podsieci:
    - podsieć frontonu dla serwerów sieci web 
    - podsieć wewnętrznej dla serwerów aplikacji, klaster MongoDB i kontrolerów domeny

![Diagram różnych poziomów infrastruktury aplikacji](./media/virtual-machines-common-infrastructure-service-guidelines/example-tiers.png)

Przychodzące bezpiecznego ruchu w sieci web musi być równoważonego między serwerami sieci web jak klienci przeglądanie magazynie online. Kolejność przetwarzania ruchu w postaci HTTP żąda od serwerów musi być równoważonego między serwerami aplikacji sieci web. Ponadto infrastruktury muszą być zaprojektowane wysokiej dostępności.

Wynikowa projekt musi uwzględniać:

- Azure subskrypcji i konta
- Pojedynczej grupy zasobów
- Konta miejsca do magazynowania
- Wirtualnej sieci z dwóch podsieci
- Dostępność ustawia maszyny wirtualne z podobną rolę
- Maszyn wirtualnych

Wszystkie powyższe z tymi konwencjami nazewnictwa:

- Przygodowy cykli Works przy użyciu **[obciążenie pracą IT]-[lokalizacja]-[Azure zasób]** jako prefiksu
    - Na przykład "**azos**" (magazyn online Azure) jest nazwą obciążenie pracą IT i "**Używanie**" (USA wschodniego 2) jest lokalizacja
- Za pomocą kont miejsca do magazynowania adventureazosusesa**[opis]**
    - "adventure" został dodany do prefiks tak, aby zapewnić unikatowość i nazwy kont miejsca do magazynowania nie obsługuje stosowania łączników.
- Wirtualnych sieci za pomocą AZOS — UŻYJ-VN**[liczba]**
- Zestawy dostępności za pomocą azos — za pomocą-jako-**[role]**
- Maszyn wirtualnych w nazwach są używane azos — za pomocą-maszyn wirtualnych -**[vmname]**


## <a name="azure-subscriptions-and-accounts"></a>Subskrypcje Azure i kont

Adventure Works cykli używa swoją subskrypcję przedsiębiorstwa, o nazwie Adventure Works Enterprise subskrypcji, zapewniające rozliczeniami dla obciążenie IT.


## <a name="storage-accounts"></a>Konta miejsca do magazynowania

Adventure Works cykli Określa, że potrzebuje dwa konta miejsca do magazynowania:

- **adventureazosusesawebapp** do standardowego przechowywania serwerów sieci web, serwerów aplikacji i kontrolerów domeny oraz ich dyski danych.
- **adventureazosusesadbclust** do przechowywania Premium serwerów klaster sharded MongoDB i ich dyski danych.


## <a name="virtual-network-and-subnets"></a>Wirtualna sieć i podsieci

Ponieważ wirtualną sieć nie ma potrzeby trwających łączność z siecią lokalnego Adventure pracy cykli, decyzję w wirtualnej sieci tylko do chmury.

Wirtualna sieć tylko do chmury one utworzone za pomocą następujące ustawienia przy użyciu Azure portal:

- Nazwa: AZOS-UŻYJ VN01
- Lokalizacja: Wschodniego USA 2
- Przestrzeń adresów wirtualną sieć: 10.0.0.0/8
- Pierwszą podsieć:
    - Nazwa: serwera sieci Web
    - Przestrzeń adresowa: 10.0.1.0/24
- Drugą podsieć:
    - Nazwa: wewnętrznej bazy danych
    - Przestrzeń adresowa: 10.0.2.0/24


## <a name="availability-sets"></a>Zestawy dostępności

Aby zachować wysoką dostępność wszystkich czterech poziomów ich magazynu online, Adventure Works cykli decyzję na cztery zestawy dostępności.

- **azos Użyj jako web** serwerów sieci web
- **azos Użyj jako aplikacji** dla serwerów aplikacji
- **azos Użyj jako db** dla serwerów w klastrze sharded MongoDB
- **azos Użyj jako dc** kontrolerów domeny


## <a name="virtual-machines"></a>Maszyn wirtualnych

Na poniższych nazw ich maszyny wirtualne Azure przez danego Adventure Works cykli:

- **azos Użyj-maszyn wirtualnych web01** pierwszy serwer sieci web
- **azos Użyj-maszyn wirtualnych web02** drugi serwer sieci web
- **azos Użyj-maszyn wirtualnych app01** dla pierwszego serwera aplikacji
- **azos Użyj-maszyn wirtualnych app02** dla drugiego serwera aplikacji
- **azos Użyj-maszyn wirtualnych db01** dla pierwszego serwera MongoDB w klastrze
- **azos Użyj-maszyn wirtualnych db02** drugi serwer MongoDB w klastrze
- **azos Użyj-maszyn wirtualnych dc01** dla pierwszego kontrolera domeny
- **azos Użyj-maszyn wirtualnych dc02** dla drugiego kontrolera domeny

Oto konfiguracji wynikowe.

![Infrastruktura gotowych aplikacji wdrożony platformy Azure](./media/virtual-machines-common-infrastructure-service-guidelines/example-config.png)

Zamieszczanie tej konfiguracji:

- Tylko do chmury wirtualnej sieci z dwóch podsieci (serwera sieci Web i wewnętrznej bazy danych)
- Dwa konta miejsca do magazynowania
- Cztery zestawy dostępność, jedną dla każdego poziomu magazyn online
- Maszyn wirtualnych do czterech warstw
- Ustawianie zewnętrznych równoważenia obciążenia dla sieci web opartych na HTTPS ruchu z Internetu na serwerach sieci web
- Wewnętrzna ładowania strategie ustaw dla ruchu w sieci web niezaszyfrowanym z serwerów sieci web na serwerach aplikacji
- Pojedynczej grupy zasobów


## <a name="next-steps"></a>Następne kroki

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)] 