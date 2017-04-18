<properties
    pageTitle="Przygotowywanie mapowanie sieci funkcji Hyper-V ochrony maszyn wirtualnych przy użyciu oprogramowania VMM w Odzyskiwanie witryny Azure | Microsoft Azure"
    description="Skonfiguruj mapowanie sieci funkcji Hyper-V maszyn wirtualnych replikacji z centrum danych lokalnych Azure lub do witryny pomocniczą."
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
    ms.date="10/05/2016"
    ms.author="raynew"/>


# <a name="prepare-network-mapping-for-hyper-v-virtual-machine-protection-with-vmm-in-azure-site-recovery"></a>Przygotowywanie mapowanie sieci funkcji Hyper-V ochrony maszyn wirtualnych przy użyciu oprogramowania VMM w Odzyskiwanie witryny Azure

Azure Odzyskiwanie witryny składa się na strategii odzyskiwania (BCDR) ciągłości i danych biznesowych przez orchestrating replikacji, pracy awaryjnej i odzyskiwanie maszyn wirtualnych i serwerów fizycznych.

W tym artykule opisano mapowanie sieci, który ułatwia optymalnie Konfigurowanie ustawień sieci podczas korzystania z witryn odzyskiwania replikacji maszyn wirtualnych funkcji Hyper-V znajduje się w chmury VMM między dwoma lokalnych centrach danych, lub centrum danych lokalnych i Azure. Należy zauważyć, że masz replikacji maszyny wirtualne funkcji Hyper-V bez w chmurze VMM, czy replikacji maszyny wirtualne VMware lub serwerów fizycznych, w tym artykule nie jest odpowiednie.

Publikowanie jakieś komentarze lub pytania u dołu tego artykułu lub [Azure odzyskiwania usług Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)w witrynie.


## <a name="overview"></a>Omówienie

Mapowanie sieci jest używana podczas Azure Odzyskiwanie witryny zostanie wdrożony powielić maszyn wirtualnych funkcji Hyper-V, aby Azure lub pomocniczej centrum danych, za pomocą funkcji Hyper-V replice lub SAN replikacji.

- **Replikacja funkcji Hyper-V maszyn wirtualnych w chmury VMM między dwoma lokalnych centrach danych**— sieci mapy mapowania między maszyn wirtualnych sieci na serwerze VMM źródła i sieciami maszyn wirtualnych na serwerze VMM docelowej, wykonaj następujące czynności:

    - **Łączenie maszyn wirtualnych po przełączeniu**— zapewnia podłączony do sieci odpowiednie po przełączeniu maszyn wirtualnych. Maszyny wirtualnej replice będzie połączony z siecią docelową mapowanego Sieć źródłowa.
    - **Miejsce, w replice maszyn wirtualnych na serwerach hosta**— optymalnie umieść maszyn wirtualnych replice na serwerach hosta funkcji Hyper-V. Maszyn wirtualnych replice zostaną umieszczone na hostów, które można uzyskać dostęp do zamapowanych sieci maszyn wirtualnych.
    - **Brak mapowania sieci**— Jeśli nie skonfigurowano mapowanie sieci, zreplikowanej maszyn wirtualnych nie będzie podłączony do sieci maszyn wirtualnych po przełączeniu.

- **Replikacja funkcji Hyper-V maszyn wirtualnych w VMM lokalnego w chmurze Azure**— sieci mapy mapowania między sieciami maszyn wirtualnych na serwerze VMM źródłowym i docelowych Azure sieci, wykonaj następujące czynności:
    - **Łączenie maszyn wirtualnych po przełączeniu**— wszystkich komputerach, które pracy awaryjnej w tej samej sieci można połączyć ze sobą, niezależnie od tego, który plan odzyskiwania znajdują się w.
    - **Brama sieci**— Jeśli brama sieci jest skonfigurowana w celu Azure sieci, maszyn wirtualnych można nawiązać pozostałych maszyn wirtualnych lokalnego.
    - **Brak mapowania sieci**— Jeśli nie skonfigurowano mapowanie sieci, tylko maszyn wirtualnych tej pracy awaryjnej w tego samego planu odzyskiwania będzie mógł połączyć ze sobą po błędów przez Azure.


## <a name="network-mapping-example"></a>Przykład mapowanie sieci

Mapowanie sieci można skonfigurować między sieciami maszyn wirtualnych na dwóch serwerach VMM lub na serwerze VMM pojedynczy Jeśli dwie witryny są zarządzane przez ten sam serwer. Podczas mapowania jest poprawnie skonfigurowany i replikacja jest włączona, maszyny wirtualnej w lokalizacji podstawowy zostanie połączony z siecią, a jej replice w lokalizacji docelowej będzie połączony z jego zamapowanych sieci.

Jeśli sieci zdefiniowano poprawnie w VMM, po wybraniu maszyn wirtualnych sieci docelowej podczas mapowania sieci, VMM chmury źródła, które korzystają z źródła maszyn wirtualnych sieci będą wyświetlane, wraz z sieci maszyn wirtualnych dostępnych w chmury docelowej, które są używane do ochrony.

Oto przykład ilustrujący ten mechanizm. Przyjrzyjmy się organizacji z dwóch miejscach w Nowy Jork i Chicago.

**Lokalizacja** | **Serwer VMM** | **Maszyn wirtualnych sieci** | **Zamapowane**
---|---|---|---
Nowy Jork | NowyJork VMM| NowyJork VMNetwork1 | Zamapowane VMNetwork1 Chicago
 |  | NowyJork VMNetwork2 | Niezamapowane
Chicago | VMM Chicago| VMNetwork1 Chicago | Zamapowane NowyJork VMNetwork1
 | | VMNetwork2 Chicago | Niezamapowane

Z tym przykładzie:

- Po utworzeniu maszyny wirtualnej replice dla maszyn wirtualnych podłączonego do NowyJork VMNetwork1 będzie można połączyć VMNetwork1 Chicago.
- Po utworzeniu maszyny wirtualnej replice NowyJork VMNetwork2 lub VMNetwork2 Chicago nie będzie połączony z siecią dowolnej.

Poniżej przedstawiono, jak chmury VMM są skonfigurowane w naszym przykładzie organizacji i sieci logiczne skojarzone z chmury.

### <a name="cloud-protection-settings"></a>Ustawienia ochrony chmury

**Chroniony chmury** | **Ochrona chmury** | **Sieci logicznej (Nowy Jork)**  
---|---|---
GoldCloud1 | GoldCloud2 |
SilverCloud1| SilverCloud2 |
GoldCloud2 | <p>BRAK</p><p></p> | <p>NowyJork LogicalNetwork1</p><p>LogicalNetwork1 Chicago</p>
SilverCloud2 | <p>BRAK</p><p></p> | <p>NowyJork LogicalNetwork1</p><p>LogicalNetwork1 Chicago</p>

### <a name="logical-and-vm-network-settings"></a>Ustawienia sieci logicznych, jak i maszyn wirtualnych

**Lokalizacja** | **Sieci logicznej** | **Skojarzony maszyn wirtualnych sieci**
---|---|---
Nowy Jork | NowyJork LogicalNetwork1 | NowyJork VMNetwork1
Chicago | LogicalNetwork1 Chicago | VMNetwork1 Chicago
 | LogicalNetwork2Chicago | VMNetwork2 Chicago

### <a name="target-networks"></a>Sieci docelowej

Na podstawie tych ustawień, po wybraniu maszyn wirtualnych sieci docelowej, w poniższej tabeli przedstawiono opcje, które będą dostępne.

**Wybierz pozycję** | **Chroniony chmury** | **Ochrona chmury** | **Dostępne sieci docelowej**
---|---|---|---
VMNetwork1 Chicago | SilverCloud1 | SilverCloud2 | Dostępne
 | GoldCloud1 | GoldCloud2 | Dostępne
VMNetwork2 Chicago | SilverCloud1 | SilverCloud2 | Nie jest dostępna
 | GoldCloud1 | GoldCloud2 | Dostępne



## <a name="multiple-subnets"></a>Wiele podsieci

Jeśli jeden z tych podsieci ma taką samą nazwę jak podsieci, na którym znajduje się maszyny wirtualnej źródła sieci docelowej zawiera wiele podsieci, następnie maszyny wirtualnej replice będzie połączony z tej podsieci docelowej po przełączeniu. W przypadku nie podsieć docelowej z odpowiednia nazwa maszyny wirtualnej zostanie połączony z pierwszą podsieć w sieci.


### <a name="failback"></a>Awarii

Aby sprawdzić, co się dzieje w przypadku awarii (odwrotnej replikacji), załóżmy się, czy NowyJork VMNetwork1 jest mapowane na VMNetwork1-Chicago z poniższych ustawień.


**Maszyn wirtualnych** | **Połączony z siecią maszyn wirtualnych**
---|---
VM1 | Sieć VMNetwork1
VM2 (replice VM1) | VMNetwork1 Chicago

Przy użyciu tych ustawień Przeanalizujmy się, co się dzieje w kilka możliwych scenariuszy.

**Scenariusz** | **Wynik**
---|---
Bez zmian we właściwościach sieci 2 maszyn wirtualnych po przełączeniu. | Maszyn wirtualnych-1 jest połączony z siecią źródła.
Właściwości sieci maszyn wirtualnych-2, są zmieniane po przełączeniu i jest odłączony. | Maszyn wirtualnych-1 jest odłączony.
Właściwości sieci maszyn wirtualnych-2 są zmieniane po przełączeniu i jest połączony z VMNetwork2 Chicago. | Jeśli nie jest zamapowany VMNetwork2 Chicago, zostanie rozłączone maszyn wirtualnych-1.
Mapowanie sieci VMNetwork1 Chicago został zmieniony. | Maszyn wirtualnych-1 będzie połączony z siecią teraz zamapowane VMNetwork1 Chicago.


## <a name="next-steps"></a>Następne kroki

Teraz, gdy masz lepszego zrozumienia mapowanie sieci, [Wprowadzenie do wdrożenia Odzyskiwanie witryny](site-recovery-best-practices.md).
