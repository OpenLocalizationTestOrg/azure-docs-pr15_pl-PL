<properties
   pageTitle="Architektura Azure odwołanie — IaaS: Rozszerzanie usługi Active Directory Azure | Microsoft Azure"
   description="Jak wdrażać bezpiecznego hybrydowych architektury sieci przy użyciu autoryzacji usługi Active Directory platformy Azure."
   services="guidance,vpn-gateway,expressroute,load-balancer,virtual-network,active-directory"
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/19/2016"
   ms.author="telmos"/>

# <a name="extending-active-directory-directory-services-adds-to-azure"></a>Rozszerzanie usługi Active Directory (DODAJE) Azure

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

W tym artykule opisano najważniejsze wskazówki dotyczące rozszerzania środowiska usługi Active Directory (AD) Azure świadczenie usług rozproszone uwierzytelnianie za pomocą [Usługi domeny w usłudze Active Directory (AD DS)][active-directory-domain-services]. Ta architektura rozszerza opisana w artykułach [implementacji architektury sieci bezpiecznego hybrydowych platformy Azure] [ implementing-a-secure-hybrid-network-architecture] i [implementowania architektury sieci bezpiecznego hybrydowego z dostępem do Internetu w Azure][implementing-a-secure-hybrid-network-architecture-with-internet-access].

> [AZURE.NOTE] Azure ma dwa różne wdrożenia modele: [Menedżer zasobów] [ resource-manager-overview] i klasyczny. Ta architektura odwołanie używa Menedżera zasobów, które firma Microsoft zaleca się w przypadku wdrożeń nowy.

Korzystasz z usług AD DS do uwierzytelnienia tożsamości. Tych tożsamości mogą należeć do użytkowników, komputerów, aplikacji lub inne zasoby, które stanowią część domeny zabezpieczeń. Można obsługiwać usług AD DS lokalnego, ale w scenariuszu hybrydowe, gdzie znajdują się elementy aplikacji w Azure może być bardziej efektywne, aby odtworzyć tę funkcję i repozytorium AD w chmurze. Tej metody można ograniczyć opóźnienia spowodowane przez wysłanie uwierzytelniania i żądań autoryzacji lokalne z chmury do uruchamiania lokalnego w usługach AD DS. 

Istnieją dwa sposoby obsługi usługi katalogowej w Azure:

1. Możesz użyć [Azure Active Directory (AAD)] [ azure-active-directory] do utworzenia nowej domeny AD w chmurze i połączyć go z lokalnego hasło. Następnie konfiguracji [Azure AD Connect] [ azure-ad-connect] ze zobowiązania replikacji tożsamości przechowywanych w repozytorium lokalnego w chmurze. Należy zauważyć, że katalog w chmurze jest **nie** rozszerzenie lokalny system, zamiast jest kopią zawierającą samej tożsamości. Zmiany wprowadzone do tych tożsamości, które lokalnego zostaną skopiowane do chmury, ale zmiany wprowadzone w chmurze, **nie będzie** można replikować do domeny lokalnej. Na przykład resetowania hasła musi być wykonywana w lokalnym i skopiować zmianę w chmurze za pomocą narzędzie Azure AD Connect. Należy również zauważyć, że w tym samym wystąpieniu programu AAD może być połączony z więcej niż jednego wystąpienia usług AD DS; AAD będzie zawierać tożsamości każdego repozytorium AD, do którego jest połączony.

    AAD jest przydatne w sytuacjach, miejsce, w którym sieci lokalnej i Azure wirtualną sieć hostingu w chmurze zasobów nie są bezpośrednio związane przy użyciu sieci VPN tunelem lub obwód ExpressRoute. Mimo że to rozwiązanie jest proste, go mogą nie być odpowiednie dla systemów miejsce, w którym składniki można przeprowadzić migrację wielu obramowanie na lokalnej chmury to może wymagać zmianę konfiguracji AAD. Ponadto AAD obsługuje tylko uwierzytelniania użytkowników zamiast uwierzytelniania komputera. Niektóre aplikacje i usługi, takie jak programu SQL Server może wymagać uwierzytelnienia komputera w przypadku tego rozwiązania nie jest odpowiedni.

2. Można wdrażać maszyny uruchamianie usług AD DS jako kontrolera domeny platformy Azure, wydłużając istniejącą infrastrukturę AD z lokalnego centrum danych. Ta metoda jest najbardziej typowych scenariuszy, w której są połączone sieci lokalnej i Azure wirtualnych sieci przez połączenie VPN i/lub ExpressRoute. To rozwiązanie umożliwia dwukierunkową replikacji pozwoli Ci wprowadzanie zmian w chmurze i lokalnego, tam, gdzie jest najbardziej odpowiednie. W zależności od wymagań dotyczących zabezpieczeń może być instalacji AD w chmurze:
    - część tej samej domeny jako tego przechowywanych w lokalnej
    - nową domenę w udostępnionej las
    - oddzielne las

<!-- we might want to add info on how to choose from the three options above -->

Ta architektura omówiono rozwiązanie 2, za pomocą tej samej domeny usług AD DS w Azure i lokalnych.

Typowe zastosowanie przypadkach dla tej architektury obejmują:

- Aplikacje hybrydowych miejsce, w którym obciążenia Uruchom częściowo lokalnego i częściowo w Azure.

- Aplikacji i usług, które przeprowadzać uwierzytelnianie za pomocą usługi Active Directory.

## <a name="architecture-diagram"></a>Diagram architektury

Poniższy diagram wyróżnienie ważne składniki, w tym architektury (*kliknij, aby powiększyć*). Aby uzyskać więcej informacji o elementach wyszarzona, przeczytaj [implementacji architektury sieci bezpiecznego hybrydowych platformy Azure] [ implementing-a-secure-hybrid-network-architecture] i [implementowania architektury sieci bezpiecznego hybrydowego z dostępem do Internetu w Azure][implementing-a-secure-hybrid-network-architecture-with-internet-access]:

[! [0]][0]

- **Sieci lokalnej.** Sieci lokalnej zawiera lokalne serwery AD, które mogą wykonywać uwierzytelniania i autoryzacji składniki znajdującego się w lokalnej.

- **Serwery AD.** Są to kontrolery implementacji usługi katalogowej (AD DS) uruchomione maszyny wirtualne w chmurze. Te serwery można udostępnić uwierzytelniania składniki działające w sieci wirtualnych Azure.

- **Podsieć Active Directory.** Serwer usług AD DS są obsługiwane w osobnej podsieci. Reguły NSG Chroń serwer usług AD DS i umożliwiają Zapora przeciw ruchu z nieznanych źródeł.

- **Synchronizacji azure bramy i AD.**. Brama Azure umożliwia połączenie między sieci lokalnej i Azure VNet. Może to być [połączenie VPN] [ azure-vpn-gateway] lub [Azure ExpressRoute][azure-expressroute]. Wszystkie żądania synchronizacji między serwerami AD w chmurze i lokalnego przejść przez bramę. Trasy zdefiniowane przez użytkownika (UDRs) uchwyt routingu ruchu synchronizacji, która przekazuje bezpośrednio na serwerze AD w chmurze i nie przechodzą przez istniejącej sieci wirtualnej urządzenia (NVAs) w tym scenariuszu.

    Aby uzyskać więcej informacji o konfigurowaniu UDRs i NVAs, zobacz [implementacji architektury sieci bezpiecznego hybrydowych platformy Azure][implementing-a-secure-hybrid-network-architecture].

## <a name="recommendations"></a>Zalecenia

W tej sekcji przedstawiono zalecenia dotyczące implementowania usług AD DS w Azure, obejmujące:

- Zalecenia dotyczące maszyn wirtualnych.

- Zalecenia dotyczące sieci.

- Zalecenia dotyczące zabezpieczeń. 

- Aktywne zalecenia katalogu witryn.

- Aktywne zalecenia położenie FSMO katalogu.

>[AZURE.NOTE] Dokument [wskazówek dotyczących wdrażania systemu Windows Server usługi Active Directory na maszyn wirtualnych Azure] [ ad-azure-guidelines] zawiera bardziej szczegółowe informacje o wiele punktów.

### <a name="vm-recommendations"></a>Zalecenia dotyczące maszyn wirtualnych

Utwórz maszyny wirtualne z odpowiednimi zasobami obsługę przewidywaną wielkość ruchu. Użyj rozmiaru maszyn hostingu usług AD DS w środowisku lokalnym jako punktu początkowego. Monitorowanie wykorzystania zasobów; można zmieniać rozmiar maszyny wirtualne i skalowanie w dół, jeśli znajdują się zbyt duży. Aby uzyskać więcej informacji na temat zmiany rozmiaru kontrolerach domeny usług AD DS, zobacz [Planowanie wydajności dla usługi Active Directory][capacity-planning-for-adds].

Tworzenie osobnych danych wirtualnego dysku do przechowywania bazy danych, dzienników i SYSVOL programu AD. Nie należy przechowywać te elementy na tym samym dysku co system operacyjny. Należy zauważyć, że domyślnie dyski danych, które zostały dołączone do maszyny używane przy zastosowaniu. Jednak ta forma buforowania mogą powodować konflikt z wymaganiami usług AD DS. Z tego powodu ustawieniu *Preferencji pamięci podręcznej hosta* na dysku danych na *Brak*. Aby uzyskać więcej informacji, zobacz [rozmieszczenie Windows Server AD DS w bazie danych i SYSVOL][adds-data-disks].

Wdrażanie co najmniej dwa maszyny wirtualne działającego usług AD DS jako kontrolery Azure wirtualną siecią [dostępność](#Availability-considerations) powodów.

### <a name="networking-recommendations"></a>Zalecenia dotyczące sieci

Konfigurowanie interfejsu sieciowego dla każdej maszyny wirtualne, hostingu usług AD DS ze prywatnych adresów IP. Ta konfiguracja lepiej obsługuje DNS na wszystkich maszyny wirtualne AD. Aby uzyskać więcej informacji, zobacz [jak ustawić statyczny adres IP prywatne w portalu Azure][set-a-static-ip-address].

> [AZURE.NOTE] Nie ma maszyny AD DS wirtualne publicznych adresów IP. [Zagadnienia dotyczące zabezpieczeń] zobacz[ security-considerations] uzyskać więcej szczegółowych informacji.

Należy się upewnić, że ruch może swobodnego przechodzić między serwerami AD w chmurze i lokalnego:

- Dodawanie reguł NSG do podsieci AD, która zezwala na ruch przychodzący przy lokalnego. Aby uzyskać szczegółowe informacje dotyczące portów, które są używane w usługach AD DS, zobacz [usługi Active Directory i Active Directory Domain usług wymagań dotyczących portów][ad-ds-ports].

- Upewnij się, że tabele UDR nie skierować ruch usług AD DS za pośrednictwem NVAs używane w tym scenariuszu. W przypadku samodzielnego wdrożeń w zależności od tego, jakie NVAs używasz można przekierowywać ruch.

### <a name="security-recommendations"></a>Zalecenia dotyczące zabezpieczeń

AD DS serwery obsługi uwierzytelniania i dlatego są bardzo ważnych elementów. Zapobieganie bezpośrednie narażenie serwer usług AD DS z Internetem. Umieść serwer usług AD DS w osobnej podsieci, przy użyciu własnej zapory. Upewnij się, że porty należy używać usług AD DS do uwierzytelniania i autoryzacji i serwerów synchronzing pozostają otwarte, ale Zamknij wszystkie inne porty. Aby uzyskać więcej informacji, zobacz [Usługa Active Directory i Active Directory Domain usług wymagań dotyczących portów][ad-ds-ports]. [Składniki rozwiązania] [ solution-components] sekcji w dalszej części tego dokumentu zawiera NSG zasady używanego rozwiązania próbki do otwierania portów.

Reguły NSG umożliwia utworzenie prostej zapory. Jeśli potrzebujesz bardziej zaawansowane ochrony można zaimplementować obwód zwiększenia bezpieczeństwa wokół serwery przy użyciu parę podsieci i NVAs, zgodnie z opisem w dokumencie [implementacji architektury sieci bezpiecznego hybrydowego z dostępem do Internetu w Azure][implementing-a-secure-hybrid-network-architecture-with-internet-access].

Szyfrowanie dysku obsługującym bazę danych usług AD DS za pomocą funkcji BitLocker lub Azure szyfrowania dysku.

### <a name="active-directory-site-recommendations"></a>Aktywne zalecenia katalogu witryn

Możesz użyć witryn w usługach AD DS do grupy razem kontrolery połączone za pomocą szybkiego łącza. Kontrolery domeny w tej samej witrynie usług AD DS automatycznie replikowane ich zmiany katalogu, a niezbędne do obsługi replikacji jest nieco konfiguracji.

Aby kontrolować ruch replikacji między Azure i lokalnych centrum danych, zaleca się dodawanie oddzielnej witryny usług AD DS reprezentować adres zajmowanego przez Azure. Możesz skonfigurować łącze witryn między swoje lokalnie usług AD DS witryny i bardziej efektywną komunikację sterowania replikacją między witrynami.

Aby zastosować różne obiekty zasad grupy (GPO) połączonych komputerach w Azure i korzystać z aplikacji, które są "Witryna wiedzieć", takich jak Menedżera konfiguracji centrum System umożliwia także oddzielenia witryny.

### <a name="active-directory-fsmo-placement-recommendations"></a>Active Directory FSMO położenie zalecenia

Elastyczne serwery jednego wzorca operacji (FSMO) są specjalistyczne kontrolery, reposnsible spójności danych przez różne ustawienia w usługach AD DS, wymienione poniżej.

- **Wzorzec schematu**. To jest rola las na poziomie, który zachowuje strukturę schematu używane przez usług AD DS.

- **Wzorzec nazw domen**. Jest to rolę las na poziomie, która zarządza informacjami o nazwach domen w ramach las.

- **Podstawowego kontrolera domeny (podstawowego kontrolera domeny)**. To jest rola na poziomie domeny. Podstawowemu obsługuje aktualizacje haseł i blokady konta. Jest konsultacji przez innych DCs żądania obsługi uwierzytelniania zawierających niezgodne hasła. Podstawowemu również obsługuje aktualizacje zasad grupy i docelowego kontrolera domeny dla starszych aplikacji i niektóre narzędzia administracyjne niektórych operacji zapisu.

- **Wzorzec względnego ID (RID)**. RID są używane do identyfikowania jednoznacznie obiektów w katalogu. Ten serwer jest odpowiedzialny za wygenerowanie na poziomie domeny puli RID do użycia przez wszystkie serwery AD w tej domenie.

- **Rolę infrastruktury**. Obiekt w jedną domenę można odwoływać się obiektu w innej domeny. Ta rola na poziomie domeny jest odpowiedzialny za aktualizowanie SID i nazwa wyróżniająca w odwołaniu do innej domeny obiektu obiektu. Zauważ, że serwer implementowania tej roli nie może również działać jako serwer wykazu globalnego (GC).

Aby uzyskać więcej informacji, zobacz temat [ról FSMO usługi Active Directory w systemie Windows][AD-FSMO-roles-in-windows].

W tym scenariuszu zaleca się, że można uniknąć wdrażania ról FSMO kontrolerów z platformy Azure. 

## <a name="availability-considerations"></a>Zagadnienia dotyczące dostępności

Tworzenie dostępności, ustaw dla serwerów AD. Upewnij się, że są co najmniej dwa serwery w zestawie. Serwery AD w chmurze należy kontrolerów domeny w tej samej domeny. Spowoduje to włączenie automatycznego replikacji między serwerami.

Rozważ też wyznaczające serwery jako [Wzorce operacji] [ standby-operations-masters] w przypadku łączności z serwerem uatrakcyjniając rolę FSMO kończy się niepowodzeniem.

## <a name="security-considerations"></a>Zagadnienia dotyczące zabezpieczeń

Jeśli wszystkie zadania administratora domeny prawdopodobnie może być wykonywane przy użyciu DCs lokalnego, warto rozważyć oznaczanie DCs w chmurze, tylko do odczytu. Kontrolera domeny tylko do odczytu tylko przechowuje podzbiór poświadczeń użytkowników, (wystarczająco do uwierzytelniania lokalnie) i można skonfigurować do informacji w pamięci podręcznej tylko dla określonych użytkowników. W związku z tym jeśli kontrolera domeny tylko do odczytu jest bezpieczne, tylko informacje dla użytkowników pamięci podręcznej zostaną zmienione, zamiast szczegóły każdego konta w tej domenie. Aby uzyskać więcej informacji, zobacz [Kontrolery domeny tylko do odczytu][read-only-dc].

Aby zminimalizować luka w zabezpieczeniach poszczególnych kont użytkowników i zapobiec próby włamania, wykonaj najważniejsze wskazówki dotyczące konfigurowania i konserwowanie haseł użytkowników w AD. Aby uzyskać więcej informacji, zobacz [Najważniejsze wskazówki dotyczące wymuszanie zasady haseł][best_practices_ad_password_policy]. Ponadto należy zachować ostrożność grupy, które można przypisać użytkowników. Na przykład należy wprowadzać zwykli użytkownicy członkowie grupy Administratorzy przedsiębiorstwa, grupy Administratorzy schematu oraz grupy Administratorzy domeny.

## <a name="scalability-considerations"></a>Zagadnienia dotyczące skalowalność

AD jest automatycznie skalowalna w przypadku sterowników domeny, które są częścią tej samej domeny. Żądania są rozmieszczane wszystkich kontrolerach w domenie. Możesz dodać innego kontrolera domeny, a zostaną automatycznie zsynchronizowane z domeną. Nieskonfigurowane równoważenia obciążenia osobnych, aby skierować ruch do kontrolerów w tej domenie. Zapewnienie, że wszystkie kontrolery mają wystarczającej ilości pamięci i zasobami magazynowania do obsługi bazy danych domeny. najlepiej ustawić wszystkie kontrolera domeny maszyny wirtualne ten sam rozmiar.

## <a name="management-considerations"></a>Uwagi dotyczące zarządzania

Nie należy kopiować plików wirtualnego dysku twardego kontrolerów domeny zamiast regularnego wykonywania kopii zapasowych przywracaniu może powodować niespójności w stanie między kontrolerami domeny.

Zamknij i uruchom ponownie maszyny uruchamiany roli kontrolera domeny w Azure w systemie operacyjnym gościa zamiast przy użyciu opcji Zamknij Azure Portal. Zamknij maszyn wirtualnych za pomocą Azure Portal powoduje maszyn wirtualnych przydzielenia. Ta akcja pozwala zresetować GenerationID maszyn wirtualnych, która jest niepożądane dla kontrolera domeny. Gdy jest ustawiany GenerationID maszyn wirtualnych, również zresetowaniem invocationID repozytorium AD, puli RID zostanie odrzucony i SYSVOL jest oznaczony jako nieautorytatywnych.

## <a name="monitoring-considerations"></a>Zagadnienia dotyczące monitorowania

Nie można monitorować i obsługa sieci serwerów AD może powodować problemy z takich jak:

- **Błąd logowania**. Błąd logowania może wystąpić w całej domeny lub las rozdzielczości relacji lub nazwę zaufania kończy się niepowodzeniem, lub jeśli serwer wykazu globalnego nie może ustalić uniwersalny członkostwo w grupach.

- **Blokada konta**. Konta użytkowników i usługi może stać się zablokowane podstawowego kontrolera domeny jest niedostępny, czy replikacja nie powiedzie się między kilkoma kontrolerami domeny.

- **Błąd kontrolera domeny**. Jeśli dysk z plikiem Ntds.dit działa miejsca na dysku, kontrolera domeny może przestać działać.

- **Błąd aplikacji**. Aplikacje, które są krytyczne dla firmy, takich jak program Microsoft Exchange lub innej aplikacji poczty e-mail, może zakończyć się niepowodzeniem, jeśli nie kwerendy książki adresowej do katalogu.

- **Niespójne katalogu danych**. Jeśli replikacja nie powiedzie się przez dłuższy czas, zduplikowane obiekty można tworzyć w katalogu i może wymagać rozległa diagnozowania i czasu, aby wyeliminować.

- **Błąd podczas tworzenia konta**. Kontrolera domeny nie może utworzyć konta użytkowników i komputerów, jeśli powoduje pełne wykorzystanie jego dostaw identyfikatorów względnych i wzorca RID jest niedostępny.

- **Błąd zasad zabezpieczeń**. Jeśli folder udostępniony SYSVOL nie replikuje prawidłowo, obiekty zasad grupy i zasad zabezpieczeń nie są poprawnie stosowane do klientów.

Monitorowanie Serwery AD dokładnie i Przygotuj się do szybkiego podejmowania działań naprawczych. Tworzenie listy kontrolnej monitorowania zadania wykonywane w odpowiednich odstępach. Na przykład można zaplanować następujące zadania krytyczne codziennie:

- Sprawdzenie i rozwiązywanie alerty zgłoszone przez administratorów domeny 

- Sprawdź, czy wszystkie kontrolery pozwala na komunikowanie się i działa replikacji.

- Upewnij się, że SYSVOL pozostanie udostępniony.

- Rozwiązać problemy z synchronizacją czasu.

- Sprawdź, czy miejsca na dyskach fizycznych używane przez AD.

Często może wykonać inne powtarzających się zadań mniej. Można na przykład Przejrzyj relacje zaufania i sprawdź zaufania przestarzałe lub niepoprawnych co tydzień, a Sprawdź, czy wszystkie kontrolery są uruchomione przy użyciu te same dodatki service pack i poprawki poprawki co miesiąc.

Aby uzyskać więcej informacji, zobacz [Monitorowanie usługi Active Directory][monitoring_ad]. Możesz zainstalować narzędzi, takich jak [Systemy Centrum] [ microsoft_systems_center] na serwerze monitorowania (zobacz [diagram architektury][architecture]) ułatwiające wykonywanie następujących zadań. 

## <a name="solution-components"></a>Składniki rozwiązania

Rozwiązanie przewidziane Ta architektura tworzy sieci bezpiecznego hybrydowe, zgodnie z opisem dokumenty [implementacji architektury sieci bezpiecznego hybrydowych platformy Azure] [ implementing-a-secure-hybrid-network-architecture] i [implementowania architektury sieci bezpiecznego hybrydowego z dostępem do Internetu w Azure][implementing-a-secure-hybrid-network-architecture-with-internet-access], ale z dodatkiem następujące elementy:

- Grupa zasobów Azure o nazwie dns *basename*-rg, gdzie *basename* jest prefiks możesz określić, kiedy wdrożeniem rozwiązania programu.

- Dwa Azure maszyny wirtualne o nazwie *basename*-ad1-maszyn wirtualnych i *basename*-ad2-głosowa, utworzone w grupie *basename*dns rg zasobów. Te maszyny wirtualne są skonfigurowane jako serwery AD z usługami katalogowymi i systemie DNS, zainstalowaniu i skonfigurowaniu.

- NSG o nazwie *basename*-ad-nsg w grupie *basename*- ntwk rg Azure zasobów. Ta grupa zasobów jest częścią infrastruktury, która stanowi sieci bezpiecznego hybrydowego, ale nowe NSG jest dodatek, który definiuje reguły przychodzące zabezpieczeń dla serwerów AD, jak pokazano w poniższej tabeli:


    Priority (priorytet)|Nazwa|Źródła|Miejsce docelowe|Usługa|Akcja|
    --------|----|------|-----------|-------|------|
    170|vnet do port53|10.0.0.0/16|Dowolny|Custom(ANY/53)|Zezwalaj na|
    180|vnet do port88|10.0.0.0/16|Dowolny|Custom(ANY/88)|Zezwalaj na|
    190|vnet do port135|10.0.0.0/16|Dowolny|Custom(ANY/135)|Zezwalaj na|
    200|vnet do port137 9|10.0.0.0/16|Dowolny|Custom(ANY/137-139)|Zezwalaj na|
    210|vnet do port389|10.0.0.0/16|Dowolny|Custom(ANY/389)|Zezwalaj na|
    220|vnet do port464|10.0.0.0/16|Dowolny|Custom(ANY/464)|Zezwalaj na|
    230|vnet do rpc dynamiczne|10.0.0.0/16|Dowolny|Custom(ANY/49152-65535)|Zezwalaj na|
    240|onprem ad do port53|192.168.0.0/24|Dowolny|Custom(ANY/53)|Zezwalaj na|
    250|onprem ad do port88|192.168.0.0/24|Dowolny|Custom(ANY/88)|Zezwalaj na|
    260|onprem ad do port135|192.168.0.0/24|Dowolny|Custom(ANY/135)|Zezwalaj na|
    270 stopni|onprem ad do port389|192.168.0.0/24|Dowolny|Custom(ANY/389)|Zezwalaj na|
    280|onprem ad do port464|192.168.0.0/24|Dowolny|Custom(ANY/464)|Zezwalaj na|
    290|Zezwalanie rdp zarządzania|10.0.0.128/25|Dowolny|Custom(ANY/3389)|Zezwalaj na|
    300|Zezwalaj na bramy|10.0.255.224/27|Dowolny|Custom(ANY/any)|Zezwalaj na|
    310|Zezwalaj na własny|10.0.255.192/27|Dowolny|Custom(ANY/any)|Zezwalaj na|
    320|vnet — odmówić|Dowolny|Dowolny|Custom(ANY/any)|Zezwalaj na|

    Usług AD DS używa portów 53, 89 135, 389 i 464, aby zaakceptować przychodzące replikacji i ruch uwierzytelniania. W tej tabeli kontrolera domeny lokalnej znajduje się w 192.168.0.0/24 miejsca adres (obszaru adresu mogą się różnić — te informacje możesz określić jako parametr do szablonów używany przez to rozwiązanie.

    NSG definiuje również z następującymi regułami Zabezpieczenia wychodzących, które umożliwiają synchronizacji i autoryzacji przepływ ruchu powrót do sieci lokalnej:

    Priority (priorytet)|Nazwa|Źródła|Miejsce docelowe|Usługa|Akcja|
    --------|----|------|-----------|-------|------|
    100|port53 w nowym oknie|Dowolny|192.168.0.0/24|Custom(ANY/53)|Zezwalaj na|
    110|port88 w nowym oknie|Dowolny|192.168.0.0/24|Custom(ANY/88)|Zezwalaj na|
    120|port135 w nowym oknie|Dowolny|192.168.0.0/24|Custom(ANY/135)|Zezwalaj na|
    130|port389 w nowym oknie|Dowolny|192.168.0.0/24|Custom(ANY/389)|Zezwalaj na|
    140|port445 w nowym oknie|Dowolny|192.168.0.0/24|Custom(ANY/445)|Zezwalaj na|
    150|port464 w nowym oknie|Dowolny|192.168.0.0/24|Custom(ANY/464)|Zezwalaj na|
    160|w nowym oknie rpc dynamicznych|Dowolny|192.168.0.0/24|Custom(ANY/49152-65535)|Zezwalaj na|

Skrypt do dyspozycji rozwiązanie również wykonać następujące zadania:

- Dodaje *basename*-ad1-maszyn wirtualnych i *basename*-ad2-maszyn wirtualnych serwerów jako kontrolery domeny do domeny. Można wyświetlić te serwery w konsoli *Użytkownicy usługi Active Directory i komputery* w lokalnej kontrolera domeny:

![[1]][1]

- Nowa podsieć (10.0.0.0/16) tworzy AD witryny o nazwie Azure VNet-Ad-witryn do domeny. Ta witryna zawiera *basename*-ad1-maszyn wirtualnych i *basename*-ad2-maszyn wirtualnych serwerów. 

- Dodaje ustawień witryny między transportu IP, które Konfigurowanie interwału replikacji między lokalnej witryny i kontrolerami domeny w chmurze. Można wyświetlić ustawienia podsieci, witryn i ustawienia transportu w konsoli *Lokacje i Active Directory serwery* lokalnego kontrolera domeny:

![[2]][2]

## <a name="deployment"></a>Wdrożenie

Rozwiązanie przykładowe występują następujące prerequsites:

- Domeny lokalnego została już skonfigurowana i że masz skonfigurowany w taki sposób, a zainstalowane usługi Routing i dostęp zdalny do obsługi sieci VPN połączyć się z bramą Azure VPN.


- Zainstalowano najnowszą wersję programu polecenie Azure. [Wykonaj te instrukcje, aby uzyskać szczegółowe informacje][cli-install].

- Jeśli jest instalowany rozwiązanie z systemu Windows, należy zainstalować narzędzie udostępniające powłoki imprezie, na przykład [Pulpit GitHub][github-desktop].

>[AZURE.NOTE] Jeśli nie masz dostępu do istniejącej domeny lokalnego, możesz utworzyć środowisku testowym przy użyciu [onpremdeploy.sh] [ onpremdeploy] urodzinową skrypt. Ten skrypt tworzy sieci i maszyn wirtualnych w chmurze, które symuluje konfiguracji bardzo podstawowe lokalnego. Edytuj ten skrypt przed uruchomieniem i ustaw następujące zmienne zdefiniowane na początku pliku:
>
> - **BASE_NAME**. Prefiks zdefiniowane przez użytkownika dla grupy zasobów i maszyn wirtualnych utworzone przez skrypt. Ten element powinien być **dłuższa niż 5 znaków** w inny sposób skrypt spróbuje Generowanie maszyn wirtualnych z nieprawidłową nazwą i kończą się niepowodzeniem.
> 
> - **SUBSKRYPCJI**. Identyfikatora usługi Azure subskrypcji. Grupa zasobów zostanie utworzony w tym suscription.
> 
> - **Lokalizacja**. Lokalizacja Azure, w której chcesz utworzyć grupy zasobów, takich jak *eastus* lub *westus*.
> 
> - **ADMIN_USER_NAME**. Nazwa służących do konta administratora w maszyn wirtualnych.
> 
> - **ADMIN_PASSWORD**. Hasło do konta administratora.

Wykonaj poniższe czynności, aby utworzyć rozwiązanie przykładowe:

1. Pobieranie i edytować [azuredeploy.sh] [ azuredeploy] skryptów i ustawić następujące parametry na początku pliku:

    - **BASE_NAME**. Prefiks zdefiniowane przez użytkownika dla grup zasobów i maszyny wirtualne utworzone przez skrypt. Jak poprzednio ten element powinny być **dłuższa niż 5 znaków**.

    - **SUBSKRYPCJI**. Identyfikatora usługi Azure subskrypcji.

    - **Typ_systemu_operacyjnego**. System operacyjny (*Windows* i *Linux oraz*) służących do poziomu dostępu do sieci web, firm i danych maszyny wirtualne. Zauważ, że wszystkie serwery AD utworzone przez skrypt uruchomić systemu Windows Server 2012, bez względu na to ustawienie.

    - **Nazwa_domeny**. Nazwa domeny lokalnej. Należy zauważyć, że użycie środowiska utworzone przez skrypt onpremdeploy.sh, musi to być *contoso.com*.

    - **Lokalizacja**. Lokalizacja Azure, w której chcesz utworzyć grupy zasobów.

    - **ADMIN_USER_NAME**. Nazwa używana dla kont administratorów w różnych maszyny wirtualne.

    - **ADMIN_PASSWORD**. Hasło do konta administratora.

    - **ON_PREMISES_PUBLIC_IP**. Publiczny adres IP komputera lokalnego VPN.

    - **ON_PREMISES_ADDRESS_SPACE**. Miejsce wewnętrzny adres sieci lokalnej. Jeśli używasz środowiska utworzone przez skrypt onpremdeploy.sh musi to być 192.168.0.0/16.

    - **VPN_IPSEC_SHARED_KEY**. Klucz udostępniony IPSec używany do nawiązywania połączenia VPN między sieci lokalnej i bramą Azure VPN.

    - **ON_PREMISES_DNS_SERVER_ADDRESS**. Adres IP lokalnego serwera DNS. Jeśli używasz środowiska utworzone przez skrypt onpremdeploy.sh musi to być 192.168.0.4

    - **ON_PREMISES_DNS_SUBNET_PREFIX** Prefiks adresu podsieci lokalnego. Jeśli używasz środowiska utworzone przez skrypt onpremdeploy.sh musi to być 192.168.0.0/24.

    >[AZURE.NOTE] Aby zapisać zasobów i czasu, skrypt nie tworzy poziomów dostępu firm lub danych. Jeśli potrzebujesz tych elementów, można Usuń komentarze z następujących sekcji skryptu azuredeploy.sh:
    >
    >
    > ```
    > #### # create biz tier
    > #### TEMPLATE_URI=${URI_BASE}/ARMBuildingBlocks/Templates/bb-ilb-backend-http-https.json
    > #### SUBNET_NAME_PREFIX=${DEPLOYED_BIZ_SUBNET_NAME_PREFIX}
    > #### ILB_IP_ADDRESS=${BIZ_ILB_IP_ADDRESS}
    > #### NUMBER_VMS=${BIZ_NUMBER_VMS}
    > #### 
    > #### RESOURCE_GROUP=${BASE_NAME}-${SUBNET_NAME_PREFIX}-tier-rg
    > #### VM_NAME_PREFIX=${SUBNET_NAME_PREFIX}
    > #### VM_COMPUTER_NAME_PREFIX=${SUBNET_NAME_PREFIX}
    > #### VNET_RESOURCE_GROUP=${NTWK_RESOURCE_GROUP}
    > #### VNET_NAME=${DEPLOYED_VNET_NAME}
    > #### SUBNET_NAME=${DEPLOYED_BIZ_SUBNET_NAME}
    > #### PARAMETERS="{\"baseName\":{\"value\":\"${BASE_NAME}\"},\"vnetResourceGroup\":{\"value\":\"${VNET_RESOURCE_GROUP}\"},\"vnetName\":{\"value\":\"${VNET_NAME}\"},\"subnetName\":{\"value\":\"${SUBNET_NAME}\"},\"adminUsername\":{\"value\":\"${ADMIN_USER_NAME}\"},\"adminPassword\":{\"value\":\"${ADMIN_PASSWORD}\"},\"subnetNamePrefix\":{\"value\":\"${SUBNET_NAME_PREFIX}\"},\"ilbIpAddress\":{\"value\":\"${ILB_IP_ADDRESS}\"},\"osType\":{\"value\":\"${OS_TYPE}\"},\"numberVMs\":{\"value\":${NUMBER_VMS}},\"vmNamePrefix\":{\"value\":\"${VM_NAME_PREFIX}\"},\"vmComputerNamePrefix\":{\"value\":\"${VM_COMPUTER_NAME_PREFIX}\"}}"
    > #### 
    > #### echo
    > #### echo
    > #### echo azure group create --name ${RESOURCE_GROUP} --location ${LOCATION} --subscription ${SUBSCRIPTION}
    > ####      azure group create --name ${RESOURCE_GROUP} --location ${LOCATION} --subscription ${SUBSCRIPTION}
    > #### echo
    > #### echo
    > #### echo azure group deployment create --template-uri ${TEMPLATE_URI} -g ${RESOURCE_GROUP} -p ${PARAMETERS} --subscription ${SUBSCRIPTION}
    > ####      azure group deployment create --template-uri ${TEMPLATE_URI} -g ${RESOURCE_GROUP} -p ${PARAMETERS} --subscription ${SUBSCRIPTION}
    > #### 
    > #### # create db tier
    > #### TEMPLATE_URI=${URI_BASE}/ARMBuildingBlocks/Templates/bb-ilb-backend-http-https.json
    > #### SUBNET_NAME_PREFIX=${DEPLOYED_DB_SUBNET_NAME_PREFIX}
    > #### ILB_IP_ADDRESS=${DB_ILB_IP_ADDRESS}
    > #### NUMBER_VMS=${DB_NUMBER_VMS}
    > #### 
    > #### RESOURCE_GROUP=${BASE_NAME}-${SUBNET_NAME_PREFIX}-tier-rg
    > #### VM_NAME_PREFIX=${SUBNET_NAME_PREFIX}
    > #### VM_COMPUTER_NAME_PREFIX=${SUBNET_NAME_PREFIX}
    > #### VNET_RESOURCE_GROUP=${NTWK_RESOURCE_GROUP}
    > #### VNET_NAME=${DEPLOYED_VNET_NAME}
    > #### SUBNET_NAME=${DEPLOYED_DB_SUBNET_NAME}
    > #### PARAMETERS="{\"baseName\":{\"value\":\"${BASE_NAME}\"},\"vnetResourceGroup\":{\"value\":\"${VNET_RESOURCE_GROUP}\"},\"vnetName\":{\"value\":\"${VNET_NAME}\"},\"subnetName\":{\"value\":\"${SUBNET_NAME}\"},\"adminUsername\":{\"value\":\"${ADMIN_USER_NAME}\"},\"adminPassword\":{\"value\":\"${ADMIN_PASSWORD}\"},\"subnetNamePrefix\":{\"value\":\"${SUBNET_NAME_PREFIX}\"},\"ilbIpAddress\":{\"value\":\"${ILB_IP_ADDRESS}\"},\"osType\":{\"value\":\"${OS_TYPE}\"},\"numberVMs\":{\"value\":${NUMBER_VMS}},\"vmNamePrefix\":{\"value\":\"${VM_NAME_PREFIX}\"},\"vmComputerNamePrefix\":{\"value\":\"${VM_COMPUTER_NAME_PREFIX}\"}}"
    > #### 
    > #### echo
    > #### echo
    > #### echo azure group create --name ${RESOURCE_GROUP} --location ${LOCATION} --subscription ${SUBSCRIPTION}
    > ####      azure group create --name ${RESOURCE_GROUP} --location ${LOCATION} --subscription ${SUBSCRIPTION}
    > #### echo
    > #### echo
    > #### echo azure group deployment create --template-uri ${TEMPLATE_URI} -g ${RESOURCE_GROUP} -p ${PARAMETERS} --subscription ${SUBSCRIPTION}
    > ####      azure group deployment create --template-uri ${TEMPLATE_URI} -g ${RESOURCE_GROUP} -p ${PARAMETERS} --subscription ${SUBSCRIPTION}
    > ```

2. Otwórz wiersz powłoki imprezie i przejdź do folderu zawierającego skrypt azuredeploy.sh.

3. Zaloguj się do konta Azure. W powłoce imprezie wpisz następujące polecenie:

    ```
    azure login
    ```

    Postępuj zgodnie z instrukcjami, aby nawiązać połączenie Azure.

4. Uruchom polecenie `./azuredeploy.sh`, a następnie zaczekaj, aż skrypt tworzy infrastruktury sieciowej.

5. W wierszu polecenia *Sprawdź, czy VNet został utworzony*za pomocą Azure portal Sprawdzanie utworzenia grupy zasobów o nazwie *basename*- ntwk-rg i że zawiera ona elementy podobne do tych pokazano na poniższej ilustracji:

    ![[3]][3]

    >[AZURE.NOTE] W przykładach pokazano *basename* została ustawiona na *chmurze* uruchomienie skryptu.

    Kliknij *basename*- vnet VNet, kliknij pozycję *podsieci*i sprawdź, czy zostały utworzone podsieci, jak pokazano poniżej:

    ![[4]][4]

6. W wierszu w oknie powłoki imprezie naciśnięcie klawisza i zaczekaj, aż warstwa i równoważenia obciążenia są tworzone.

7. Po wyświetleniu monitu, *Sprawdź, czy warstwa został utworzony prawidłowo*za pomocą Azure portal sprawdzanie utworzono grupę zasobów o nazwie *basename*sieci web — warstwa rg i że zawiera ona elementy podobne do tych, jak pokazano poniżej:

    ![[5]][5]

8. W wierszu w oknie powłoki imprezie naciśnięcie klawisza i zaczekaj, aż NVAs są tworzone.

9. W wierszu *Sprawdź, czy NVA został utworzony prawidłowo*użyto Azure portal, aby sprawdzić, czy grupę zasobów o nazwie *basename*— zarządzania — rg został utworzony przy użyciu następujące zagadnienia:

    ![[6]][6]

10. W wierszu w oknie powłoki imprezie naciśnięcie klawisza i zaczekaj, aż zostanie utworzona jumpbox.

11. Po wyświetleniu monitu, *Sprawdź, czy jumpbox został utworzony prawidłowo*Sprawdź następujące elementy zostały dodane do grupy zasobów - zarządzania rg *basename*za pomocą Azure portal:

    - Konfigurowanie dostępności o nazwie *basename*- jb-jako.

    - Maszyn wirtualnych o nazwie *basename*— jb — maszyn wirtualnych.

    - Karty sieciowej o nazwie *basename*- jb-nic.

12. W wierszu w oknie powłoki imprezie naciśnięcie klawisza i zaczekaj, aż brama Azure VPN i połączenia są tworzone. Należy zauważyć, że ten krok może potrwać do 30 minut.

13. Po wyświetleniu monitu, *Sprawdź, czy Brama VPN został utworzony prawidłowo*Sprawdź następujące elementy zostały dodane do grupy zasobów rg — ntwk *basename*za pomocą Azure portal:

    - Brama sieci lokalnej o nazwie na lokalnej lgw.
    
    - Brama wirtualną sieć o nazwie *basename*- vpngw.

    - Połączenie bramy o nazwie *basename*- vnet-vpnconn. Należy zauważyć, że stan tego połączenia może być *braku połączenia* , jeśli nie zostały jeszcze skonfigurowane lokalnego na koniec połączenie. to będzie dotyczyć później.

14. W wierszu w oknie powłoki imprezie naciśnięcie klawisza i zaczekaj, aż tworzenia maszyny wirtualne i inne zasoby dla strefy Zdemilitaryzowanej.

15. W wierszu *Sprawdź, czy strefy Zdemilitaryzowanej został utworzony prawidłowo*użyto Azure portal, aby sprawdzić, czy grupę zasobów o nazwie *basename*- strefy zdemilitaryzowanej-rg został utworzony przy użyciu następujące zagadnienia:

    ![[7]][7]

16. W wierszu w oknie powłoki imprezie naciśnij dowolny klawisz. Następujących monitów powinny być wyświetlane:

    ```text
    Manual Step...

    Please configure your on-premises network to connect to the Azure VNet

    Make sure that you can connect to the on-premises AD server from the Azure VMs
    ```

    Zaloguj się do komputera lokalnego łączącym się Azure bramy i odpowiednio skonfigurować połączenie. Dodawanie trasy statyczne na urządzeniu bramy lokalnej, który kieruje requestsfor zakres adresów 10.0.0.0/16 przez bramę do VNet. Procedura w ten sposób będą zależą od tego, jak w przypadku łączenia. Zobacz [implementacji hybrydowych architektury sieci przy użyciu Azure i lokalnych VPN] [ implementing-a-hybrid-network-architecture-with-vpn] uzyskać więcej informacji.

    Zwróć uwagę, że możesz znaleźć publiczny adres IP brama Azure VPN za pomocą portalu Azure przyjrzenie się bramy — vpngw *basename*w grupie zasobów rg — ntwk *basename*:

    ![[8]][8]

    Można określić, czy nawiązaniu połączenia poprawnie sprawdzając stan połączenia - vnet vpnconn *basename*. Powinny mieć ustawionej *połączony*.

    Aby przetestować połączenie, otwórz Podłączanie pulpitu zdalnego do jumpbox (10.0.0.254) na komputerze znajduje się w sieci lokalnej.

17. W wierszu w oknie powłoki imprezie naciśnij dowolny klawisz. Przy następnym wierszu, *Naciśnij dowolny klawisz, aby zaktualizować ustawienia VNet VNet wskazywały DNS lokalnego*, naciśnij klawisz i zaczekaj, aż ustawień DNS VNet zostaną zaktualizowane do odpowiedniej wartości określony w parametrze **ON_PREMISES_DNS_SERVER_ADDRESS** w obszarze skrypt azuredeploy.sh.

18. W wierszu, *Upewnij się, że ustawienia serwera DNS na VNet zostały zaktualizowane*, umożliwia sprawdzenie ustawienia *serwerów DNS* *basename*- vnet VNet w grupie zasobów rg — ntwk *basename*Azure portal. Należy skonfigurować *Niestandardowe DNS*i *podstawowy serwer DNS* powinien być adres lokalnego serwera DNS:

    ![[9]][9]

19. W wierszu *Naciśnij dowolny klawisz, aby utworzyć grupę zasobów dla serwerów AD* w oknie powłoki imprezie naciśnij klawisz i zaczekaj, aż zostanie utworzona grupa zasobów dla zablokowanych Serwery AD w chmurze.

20. W wierszu *Naciśnij dowolny klawisz, aby utworzyć maszyny wirtualne dla serwerów AD* w oknie powłoki imprezie naciśnięcie klawisza i poczekaj, aż maszyny wirtualne utworzone i dodane do grupy zasobów.

21. Gdy zostanie wyświetlony, *Naciśnij dowolny klawisz, aby dołączyć do maszyny wirtualne do domeny lokalnej* , przejdź do portalu Azure i sprawdź utworzono grupę o nazwie *basename*dns-rg i czy zawiera dwa maszyny Wirtualne (*basename*-ad1-maszyn wirtualnych i *basename*-ad2-maszyn wirtualnych):

    ![[10]][10]

22. W grupie zasobów rg — ntwk *basename*wyboru utworzony NSG o nazwie *basename*-ad-nsg. Sprawdź reguły przychodzące i wychodzące zabezpieczeń dla tego NSG. Powinny być one wymienione w tabelach w [składniki rozwiązania] zgodne[ solution-components] sekcji.

23. W wierszu w oknie powłoki imprezie naciśnięcie klawisza i zaczekaj, aż maszyny wirtualne są dodawane do domeny lokalnej.

24. U *Przejdź do lokalnego serwera AD, aby upewnić się, że komputery zostały dodane do domen* Monituj, łączenie się z komputerem lokalnym i sprawdź, czy oba maszyny wirtualne zostały dodane do domeny za pomocą konsoli *Użytkownicy usługi Active Directory i komputery* :

    ![[11]][11]

25. W wierszu w oknie powłoki imprezie naciśnięcie klawisza i zaczekaj, aż zostanie utworzona witryna replikacji AD, w domenie.

26. U *Przejdź do lokalnego serwera AD, aby upewnić się, że została utworzona witryna replikacji* Monituj, za pomocą konsoli *Lokacje i Active Directory usług* do Sprawdź, czy replikacji witryny o nazwie *Azure Vnet-Ad-witryny* został utworzony pomyślnie, razem z łącze między witryny transportu IP o nazwie *AzureToOnpremLink*i podsieci, która odwołuje się do VNet:

    ![[12]][12]

27. W wierszu w oknie powłoki imprezie naciśnięcie klawisza i zaczekaj, aż skrypt instaluje usługami katalogowymi i systemie DNS na wszystkich maszyny wirtualne AD.

28. Gdy zostanie wyświetlony monit *logowania należy do każdego serwera Azure AD w celu zweryfikowania, że pomyślnie skonfigurowano usługami katalogowymi* , Otwieranie połączenia pulpitu zdalnego z komputera lokalnego jumpbox (*basename*- jb-maszyn wirtualnych), a następnie otwórz inną Podłączanie pulpitu zdalnego z jumpbox do pierwszego serwera AD (*basename*-ad1-maszyn wirtualnych). Zaloguj się przy użyciu **nazwa_domeny**, **ADMIN_USER_NAME**i **ADMIN_PASSWORD** określony w skrypt azuredeploy.sh. Za pomocą Menedżera serwera, upewnij się, że ról w usługach AD DS i systemie DNS obie zostały dodane. Powtórz tę procedurę dla drugi serwer AD (*basename*-ad2-maszyn wirtualnych).

29. W wierszu w oknie powłoki imprezie naciśnij dowolny klawisz. Gdy zostanie wyświetlony monit, *Naciśnij dowolny klawisz, aby skonfigurować ustawienia Azure VNet DNS, aby wskazywały DNS platformy Azure* , naciśnij klawisz i zezwolić skryptom aktualizowanie ustawień DNS dla VNet.

30. Po wierszu *Upewnij się, że DNS VNet ustawienie został zaktualizowany odwołanie DNS maszyn wirtualnych Azure serwery* pojawia się przy użyciu Azure portalu Sprawdź ustawienie *Serwery DNS* *basename*- vnet VNet w grupie *basename*- ntwk rg zasobów. Serwery DNS głównego i pomocniczego powinna odwoływać się dwie maszyny wirtualne AD:

    ![[13]][13]

31. Uruchom ponownie wszystkich maszyny wirtualne AD przed kontynuowaniem. Ten krok jest niezbędne w celu zapewnienia, że każdy skompletowanie poprawne ustawienia DNS z platformy Azure. Poczekaj, aż obu maszyny wirtualne są uruchomione przed kontynuowaniem.

32. W wierszu w oknie powłoki imprezie naciśnij dowolny klawisz. W następnym wierszu, *Naciśnij dowolny klawisz, aby zastosować bramy UDR do podsieci bramy (go mogła zostać usunięta)*, naciśnij klawisz i zezwolić skryptom odświeżanie bramy UDR.

33. Upewnij się, że skrypt zakończy się pomyślnie.

## <a name="next-steps"></a>Następne kroki

- Dowiedz się, najważniejsze wskazówki dotyczące [tworzenia las zasobów DODAJE] [ adds-resource-forest] platformy Azure.

- Dowiedz się, najważniejsze wskazówki dotyczące [tworzenia infrastrukturę ADFS] [ adfs] platformy Azure.

<!-- links -->

[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[adfs]: ./guidance-identity-adfs.md
[guidance-vpn-gateway]: ./guidance-hybrid-network-vpn.md
[adds-resource-forest]: ./guidance-identity-adds-resource-forest.md
[script]: #sample-solution-script
[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-3-tier-vm.md
[implementing-a-secure-hybrid-network-architecture-with-internet-access]: ./guidance-iaas-ra-secure-vnet-dmz.md
[implementing-a-secure-hybrid-network-architecture]: ./guidance-iaas-ra-secure-vnet-hybrid.md
[implementing-a-hybrid-network-architecture-with-vpn]: ./guidance-hybrid-network-vpn.md
[active-directory-domain-services]: https://technet.microsoft.com/library/dd448614.aspx
[active-directory-federation-services]: https://technet.microsoft.com/windowsserver/dd448613.aspx
[azure-active-directory]: ../active-directory-domain-services/active-directory-ds-overview.md
[azure-ad-connect]: ../active-directory/active-directory-aadconnect.md
[architecture]: #architecture_diagram
[security-considerations]: #security-considerations
[recommendations]: #recommendations
[azure-vpn-gateway]: https://azure.microsoft.com/documentation/articles/vpn-gateway-about-vpngateways/
[azure-expressroute]: https://azure.microsoft.com/documentation/articles/expressroute-introduction/
[claims-aware applications]: https://msdn.microsoft.com/en-us/library/windows/desktop/bb736227(v=vs.85).aspx
[active-directory-federation-services-overview]: https://technet.microsoft.com/en-us/library/hh831502(v=ws.11).aspx
[capacity-planning-for-adds]: http://social.technet.microsoft.com/wiki/contents/articles/14355.capacity-planning-for-active-directory-domain-services.aspx
[ad-ds-ports]: https://technet.microsoft.com/library/dd772723(v=ws.11).aspx
[where-to-place-an-fs-proxy]: https://technet.microsoft.com/library/dd807048(v=ws.11).aspx
[powershell-ad]: https://technet.microsoft.com/en-us/library/ee617195.aspx
[ad_network_recommendations]: #network_configuration_recommendations_for_AD_DS_VMs
[domain_and_forests]: https://technet.microsoft.com/library/cc759073(v=ws.10).aspx
[best_practices_ad_password_policy]: https://technet.microsoft.com/magazine/ff741764.aspx
[monitoring_ad]: https://msdn.microsoft.com/library/bb727046.aspx
[microsoft_systems_center]: https://www.microsoft.com/server-cloud/products/system-center-2016/
[cli-install]: https://azure.microsoft.com/documentation/articles/xplat-cli-install
[github-desktop]: https://desktop.github.com/
[sssd-and-active-directory]: https://help.ubuntu.com/lts/serverguide/sssd-ad.html
[set-a-static-ip-address]: https://azure.microsoft.com/documentation/articles/virtual-networks-static-private-ip-arm-pportal/
[ad-azure-guidelines]: https://msdn.microsoft.com/library/azure/jj156090.aspx
[adds-data-disks]: https://msdn.microsoft.com/library/azure/jj156090.aspx#BKMK_PlaceDB
[AD-FSMO-recommendations]: #active_directory_FSMO_placement_recommendations
[AD-FSMO-roles-in-windows]: https://support.microsoft.com/en-gb/kb/197132
[standby-operations-masters]: https://technet.microsoft.com/library/cc794737(v=ws.10).aspx
[transfer-FSMO-roles]: https://technet.microsoft.com/library/cc816946(v=ws.10).aspx
[view-fsmo-roles]: https://technet.microsoft.com/library/cc816893(v=ws.10).aspx
[read-only-dc]: https://technet.microsoft.com/library/cc732801(v=ws.10).aspx
[AD-sites-and-subnets]: https://blogs.technet.microsoft.com/canitpro/2015/03/03/step-by-step-setting-up-active-directory-sites-subnets-site-links/
[sites-overview]: https://technet.microsoft.com/library/cc782048(v=ws.10).aspx
[implementing-adfs]: ./guidance-iaas-ra-secure-vnet-adfs.md
[onpremdeploy]: https://github.com/mspnp/blueprints/blob/master/ARMBuildingBlocks/guidance-iaas-ra-ad-extension/onpremdeploy.sh
[azuredeploy]: https://github.com/mspnp/blueprints/blob/master/ARMBuildingBlocks/guidance-iaas-ra-ad-extension/azuredeploy.sh
[solution-components]: #solution_components

[0]: ./media/guidance-iaas-ra-secure-vnet-ad/figure1.png "Zabezpieczanie architektury sieci hybrydowym z usługą Active Directory"
[1]: ./media/guidance-iaas-ra-secure-vnet-ad/figure2.png "Wyświetlanie dwóch maszyny wirtualne Azure jako serwery konsolę Użytkownicy usługi Active Directory i komputerami"
[2]: ./media/guidance-iaas-ra-secure-vnet-ad/figure3.png "Konsola Active Directory Lokacje i usługi z ustawieniami replikacji dla witryny w chmurze"
[3]: ./media/guidance-iaas-ra-secure-vnet-ad/figure4.png "Zawartość grupy basename — ntwk — rg zasobów"
[4]: ./media/guidance-iaas-ra-secure-vnet-ad/figure5.png "Podsieci VNet basename vnet"
[5]: ./media/guidance-iaas-ra-secure-vnet-ad/figure6.png "Elementy w warstwie sieci web"
[6]: ./media/guidance-iaas-ra-secure-vnet-ad/figure7.png "NVAs w grupie basename zarządzania rg zasobów"
[7]: ./media/guidance-iaas-ra-secure-vnet-ad/figure8.png "Zasoby w grupie zasobów basename-strefy zdemilitaryzowanej rg"
[8]: ./media/guidance-iaas-ra-secure-vnet-ad/figure9.png "Znajdowanie publiczny adres IP brama Azure VPN"
[9]: ./media/guidance-iaas-ra-secure-vnet-ad/figure10.png "Ustawienia serwera DNS dla * basename *-vnet VNet"
[10]: ./media/guidance-iaas-ra-secure-vnet-ad/figure11.png "* Basename *-dns — grupa zasobów rg zawierającą serwer AD maszyny wirtualne"
[11]: ./media/guidance-iaas-ra-secure-vnet-ad/figure12.png "Użytkownicy usługi Active Directory i komputerach konsoli pozycje serwer AD maszyny wirtualne jako członków domeny"
[12]: ./media/guidance-iaas-ra-secure-vnet-ad/figure13.png "Konsolę Lokacje i Active Directory Services z witryną replikacji dla serwerów Azure AD"
[13]: ./media/guidance-iaas-ra-secure-vnet-ad/figure14.png "Ustawienia serwera DNS dla VNet basename vnet odwoływanie się do serwerów AD w chmurze"