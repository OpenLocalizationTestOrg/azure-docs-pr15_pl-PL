<properties
    pageTitle="Obsługiwane platformy migracji zasobów IaaS od klasycznych do Menedżera zasobów Azure | Microsoft Azure"
    description="W tym artykule opisano za pośrednictwem obsługiwane platformy migracji zasobów, od klasycznych do Menedżera zasobów Azure"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="kasing"/>

# <a name="platform-supported-migration-of-iaas-resources-from-classic-to-azure-resource-manager"></a>Obsługiwane platformy migracji zasobów IaaS od klasycznych do Menedżera zasobów Azure

W tym artykule opisano sposób jest włączenie migracji infrastruktury jako zasoby usługi (IaaS) z klasycznego z modelami wdrażania Menedżera zasobów. Więcej informacji o [Menedżera zasobów Azure funkcje i zalety](../azure-resource-manager/resource-group-overview.md). Firma Microsoft szczegółowo sposób nawiązywania połączenia z modeli dwóch wdrożenia, które występować w ramach subskrypcji za pomocą bramy witryny do witryny wirtualnej sieci zasobów. 

## <a name="goal-for-migration"></a>Cel: migracji

Menedżer zasobów umożliwia wdrażanie złożonych aplikacji za pomocą szablonów, konfiguruje maszyn wirtualnych przy użyciu rozszerzeń maszyn wirtualnych i zawiera zarządzanie dostępem i znakowania. Azure Menedżer zasobów zawiera skalowalna, równolegle wdrażania maszyn wirtualnych w zestawy dostępności. Nowy model wdrożenia także zarządzanie cyklu życia obliczeń, sieci i miejsca do magazynowania niezależnie. Na koniec ma fokusu na temat włączania zabezpieczeń domyślnie z wymuszania maszyn wirtualnych w wirtualnej sieci.

Prawie wszystkie funkcje z modelu Klasyczny wdrożenia są obsługiwane dla obliczeń, sieci i magazynowania w obszarze Menedżer zasobów Azure. Do korzystania z nowych funkcji w Menedżerze zasobów Azure, możesz przeprowadzić migrację istniejącej wdrożeń z modelu wdrożenia klasyczny.

## <a name="changes-to-your-automation-and-tooling-after-migration"></a>Zmiany do automatyzacji i narzędzia po migracji

W ramach migracji zasobów z modelu wdrożenia klasyczny do modelu wdrożenia Menedżera zasobów musisz aktualizacji istniejących automatyzacji lub narzędzia, aby upewnić się, że będzie on nadal działać po migracji.

## <a name="meaning-of-migration-of-iaas-resources-from-classic-to-resource-manager"></a>Znaczenie migracji zasobów IaaS od klasycznych do Menedżera zasobów

Przed możemy przechodzić do szczegółów, Przyjrzyjmy się różnicę między kontrolnego danych i zarządzania kontrolnego operacji na zasoby IaaS.

- *Zarządzanie płaszczyzny* opisano połączeń przychodzących do płaszczyzny zarządzania lub interfejsu API do modyfikowania zasobów. Na przykład operacji, takich jak tworzenie maszyny, ponowne uruchomienie maszyny i aktualizowanie wirtualnej sieci z nowa podsieć zarządzanie zasobami uruchomionego. Nie wpływają one bezpośrednio na nawiązywanie połączeń z wystąpienia.
- *Płaszczyzny danych* (aplikacje) w tym artykule opisano środowisko uruchomieniowe wniosku i polega na interakcję z wystąpienia, które nie przechodzą interfejsu API Azure. Uzyskiwanie dostępu do witryny sieci Web i wysyłane dane z uruchomionego wystąpienia programu SQL Server lub serwera MongoDB zostanie uznane za dane aplikacji lub płaszczyzny interakcji. Kopiowanie obiektów blob z konta miejsca do magazynowania i uzyskiwanie dostępu do publiczny adres IP RDP lub SSH do maszyny wirtualnej są także płaszczyzny danych. Te operacje Zachowaj aplikacji uruchamiany obliczeń, sieci i miejsca do magazynowania.

>[AZURE.NOTE] W niektórych scenariuszach migracji platformie Azure przestaje, zwalnia i ponownym uruchomieniu maszyn wirtualnych. Wiąże się z tym krótkim przestoje kontrolnego danych.

## <a name="supported-scopes-of-migration"></a>Obsługiwane zakresy migracji

Istnieją trzy zakresy migracji, przede wszystkim odwołujące się do uruchamiania, sieci i miejsca do magazynowania. 

### <a name="migration-of-virtual-machines-not-in-a-virtual-network"></a>Migracji maszyn wirtualnych (nie w wirtualnej sieci)

W modelu wdrożenia Menedżera zasobów zabezpieczenia są realizowane dla aplikacji domyślnie. Wszystkie maszyny wirtualne muszą być w wirtualnej sieci w modelu Menedżera zasobów. Ponowne uruchamianie platformy Azure (`Stop`, `Deallocate`, i `Start`) maszyny wirtualne w ramach migracji. Istnieją dwie opcje wirtualnych sieci:

- Możesz zażądać platformy do tworzenia nowych wirtualnej sieci i migrowania maszyny wirtualnej do nowego wirtualnej sieci.
- Maszyny wirtualnej można migrować do istniejącej sieci wirtualnej w Menedżerze zasobów.

>[AZURE.NOTE] W tym zakresie migracji zarówno operacje zarządzania kontrolnego i operacje na danych kontrolnego może ona nie okres czasu podczas migracji.

### <a name="migration-of-virtual-machines-in-a-virtual-network"></a>Migracji maszyn wirtualnych (w wirtualnej sieci)

W przypadku większości konfiguracji maszyn wirtualnych tylko metadane migracji między modelami wdrażania klasyczny i Menedżera zasobów. Źródłowych maszyny wirtualne są uruchomione na tym samym sprzęcie w tej samej sieci i z tego samego magazynu. Operacje zarządzania kontrolnego może ona nie przez pewien okres czasu podczas migracji. Jednak płaszczyzny danych będzie nadal działać. Oznacza to, że aplikacje u góry maszyny wirtualne (klasyczny) nie spowodować przestoje podczas migracji.

Następujące ustawienia nie są obecnie obsługiwane. Jeśli obsługa zostanie dodany w przyszłości, niektóre maszyny wirtualne w tej konfiguracji może spowodować przestoje (Przejdź do tabulatora, cofnąć, a następnie ponownie uruchom operacje maszyn wirtualnych).

-   Masz więcej niż jeden dostępność ustawiona w usłudze w chmurze pojedynczy.
-   Masz jeden lub więcej zestawów dostępność maszyny wirtualne, które nie są w udostępnianiu ustawiona w usłudze w chmurze pojedynczy.

>[AZURE.NOTE] W tym zakresie migracji płaszczyzny zarządzania nie jest dozwolony w okresie czasu podczas migracji. Dla niektórych konfiguracji opisanych wcześniej, występuje przestoje kontrolnego danych.

### <a name="storage-accounts-migration"></a>Migracja kont miejsca do magazynowania

Aby umożliwić Bezproblemowa migracji, należy wdrożyć maszyny wirtualne Menedżera zasobów na koncie klasyczny miejsca do magazynowania. Z tej funkcji zasobów sieci i obliczeń można i powinny być migrowane niezależnie od miejsca do magazynowania konta. Po migracji nad maszyn wirtualnych i wirtualną sieć należy migrację konta przestrzeni dyskowej, aby ukończyć proces migracji. 

>[AZURE.NOTE] Model wdrażania Menedżera zasobów nie ma pojęcia obrazów klasyczny i dyski. Gdy konta miejsca do magazynowania jest migracją, klasyczny obrazy i dysków nie są widoczne w stos Menedżera zasobów, ale kopii wirtualnych dysków twardych pozostaną w oknie konta miejsca do magazynowania. 

## <a name="unsupported-features-and-configurations"></a>Nieobsługiwane funkcje i konfiguracji

Pracujemy obecnie obsługuje niektóre funkcje i konfiguracji. W poniższych sekcjach opisano Nasze zalecenia.

### <a name="unsupported-features"></a>Nieobsługiwane funkcje

Następujące funkcje nie są obecnie obsługiwane. Można opcjonalnie można usunąć te ustawienia, Migrowanie maszyn wirtualnych i ponownie włączyć ustawienia w modelu wdrożenia Menedżera zasobów.

Dostawcy zasobów | Funkcja
---------- | ------------
Obliczenia | Dyski nieskojarzone maszyn wirtualnych.
Obliczenia | Obrazy maszyn wirtualnych.
Sieci | ACL punktu końcowego.
Sieci | Wirtualna sieć bram (witryny, Azure ExpressRoute, bramy aplikacji, wskaż witryny).
Sieci | Za pomocą zaglądanie VNet wirtualnych sieci. (Migrowanie VNet do ARM, a następnie równorzędne) Dowiedz się więcej o [VNet zaglądanie] (.. /Virtual-Network/Virtual-Network-peering-overview.MD).
Sieci | Profile Menedżer ruchu.

### <a name="unsupported-configurations"></a>Nieobsługiwane konfiguracji

Następujące ustawienia nie są obecnie obsługiwane.

Usługa | Konfiguracja | Zalecenia
---------- | ------------ | ------------
Menedżer zasobów | Rola podstawie programu Access Control (RBAC) klasyczny zasobów | Ponieważ identyfikator URI zasobów jest zmodyfikowany po migracji, zaleca się planowania aktualizacji zasad RBAC, które należy wystąpić po migracji.
Obliczenia | Wiele podsieci skojarzone z maszyny | Aktualizowanie konfiguracji podsieci, aby odwołać się tylko podsieci.
Obliczenia | Maszyn wirtualnych, które należą do wirtualnej sieci, ale nie jest jawnie podsieci przypisane | Opcjonalnie można usunąć maszyn wirtualnych.
Obliczenia | Maszyn wirtualnych, które mają alertów, zasady Autoscale | Przez moduł migracji i te ustawienia są usuwane. Zdecydowanie zaleca się dokonać przeglądu środowiska przed wykonaniem migracji. Ponadto można zmienić ustawień alertów, po zakończeniu migracji.
Obliczenia | Rozszerzeń XML maszyn wirtualnych (BGInfo 1.*, Visual Studio debugowania wdrażanie sieci Web i zdalne debugowanie) | Nie jest obsługiwane. Zalecane jest, aby usunąć te rozszerzenia z maszyny wirtualnej, aby kontynuować migracji lub ich zostaną usunięte automatycznie podczas procesu migracji.
Obliczenia | Narzędzia diagnostyczne uruchamiania z nośnikami Premium | Wyłączanie funkcji Diagnostyka uruchamiania dla maszyny wirtualne przed kontynuowaniem migracji. Po zakończeniu migracji, można ponownie włączyć diagnostyki uruchamiania w stosie Menedżera zasobów. Ponadto obiektów blob, używane do zrzut ekranu i szeregowego dzienniki powinny zostać usunięte, więc możesz już nie jest naliczany dla tych obiektów blob.
Obliczenia | Usługami w chmurze, zawierające role web/pracownika | To nie jest obecnie obsługiwane.
Sieci | Sieci wirtualne, które zawierają maszyn wirtualnych i ról w sieci web i pracownika |  To nie jest obecnie obsługiwane.
Azure aplikacji usługi | Wirtualnych sieci, które zawierają środowiskach aplikacji usługi | To nie jest obecnie obsługiwane.
Usługa Azure HDInsight | Wirtualnych sieci, które zawierają usługi HDInsight | To nie jest obecnie obsługiwane.
Program Microsoft Dynamics usług cyklu | Sieci wirtualne, które zawierają maszyn wirtualnych, które są zarządzane przez usługi Dynamics cyklu | To nie jest obecnie obsługiwane.
Obliczenia | Centrum zabezpieczeń Azure rozszerzenia z VNET, zawierający Brama VPN lub bramy encja-Relacja z serwera DNS lokalna | Centrum zabezpieczeń Azure automatycznie instaluje rozszerzenia na maszyn wirtualnych ich zabezpieczeń oraz podnieść alertów. Te rozszerzenia zazwyczaj uzyskiwanie instalowany automatycznie, jeśli Centrum zabezpieczeń Azure jest włączona dla tej subskrypcji. Podczas migracji bramy nie jest obecnie obsługiwane i bramy musi zostać usunięty przed kontynuowaniem zatwierdzeniem migracji, dostęp do Internetu z kontem miejsca do magazynowania maszyn wirtualnych zostaną utracone po usunięciu bramy. Migracja nie będzie działać w takim jak nie można wprowadzić blob stan agenta gościa. Zaleca się wyłączenie zasad Centrum zabezpieczeń Azure subskrypcji 3 godziny przed wykonaniem migracji.

## <a name="the-migration-experience"></a>Środowisko migracji

Przed rozpoczęciem obsługi migracji jest zalecane następujące czynności:

- Upewnij się, że zasoby, które chcesz przeprowadzić migrację nie używaj nieobsługiwane funkcje ani konfiguracji. Zazwyczaj platformie wykrywa tych problemów i generuje komunikat o błędzie.
- Jeśli masz maszyny wirtualne, które nie znajdują się w sieci wirtualnej, będzie można został zatrzymany i alokację w ramach operacji Przygotuj. Jeśli nie chcesz utracić publiczny adres IP, sprawdź do rezerwowania adresu IP przed powodujące Operacja przygotowywania. Jednak w przypadku maszyny wirtualne w wirtualnej sieci, są nie zatrzymana i alokację.
- Planowanie migracji do innych niż godzinami przyjmować nieoczekiwane błędy, które mogą wystąpić podczas migracji.
- Pobierz bieżącej konfiguracji usługi maszyny wirtualne przy użyciu programu PowerShell, poleceń interfejsu wiersza polecenia (polecenie) lub interfejsów API pozostałych aby ułatwić sprawdzania poprawności po ukończeniu kroku Przygotuj.
- Aktualizowanie skryptów automatyzacji operationalization obsługę modelu wdrożenia Menedżera zasobów, przed rozpoczęciem migracji. Opcjonalnie możesz wykonać operacji GET zasoby są w stanie gotowe.
- Szacowanie zasady RBAC, które są skonfigurowane na klasyczny zasobów IaaS i zaplanowanie po zakończeniu migracji.

Przepływ pracy migracji jest w następujący sposób

![Zrzut ekranu przedstawiający migracji przepływu pracy](./media/virtual-machines-windows-migration-classic-resource-manager/migration-workflow.png)

>[AZURE.NOTE] Wszystkie czynności opisane w poniższych sekcjach przedstawiono idempotent. Jeśli masz problem z inną niż funkcji nieobsługiwanych lub błąd konfiguracji zalecane jest ponów próbę Przygotuj, przerwania lub przekazania operacji. Platformy Azure spróbuje ją ponownie.

### <a name="validate"></a>Sprawdzanie poprawności

Operacja sprawdzania poprawności jest pierwszym krokiem procesu migracji. Celem tego kroku jest analizowanie danych w tle dla zasobów w ramach migracji i zwrócić powodzenia i niepowodzenia, jeśli zasoby są w stanie migracji.

Wybraniu wirtualnej sieci lub hostowanej usługi (Jeśli nie jest wirtualną sieć) chcesz sprawdzić poprawność migracji.

* Jeśli zasób nie jest w stanie migracji, platformy Azure zawiera listę wszystkich powody Dlaczego nie jest obsługiwane dla migracji.

### <a name="prepare"></a>Przygotowywanie

Operacja przygotowywania jest drugim krokiem procesu migracji. Celem tego kroku jest zasymulowania transformacja zasobów IaaS od klasycznych do Menedżera zasobów zasobów i prezentowanie to obok siebie w celu wizualizacji.

Wybraniu wirtualnej sieci lub hostowanej usługi (Jeśli nie jest wirtualną sieć) chcesz przygotowywanie do migracji.

* Zasób nie jest w stanie migracji, platformy Azure przestaje procesu migracji list lub przyczyny niepowodzenia Operacja przygotowywania.
* Jeśli zasób jest w stanie migracji, blokady pierwszej platformy Azure w dół operacji kontrolnego zarządzania dla zasobów w ramach migracji. Na przykład jesteś nie można dodać dysku danych do maszyn wirtualnych w obszarze migracji.

Platformy Azure następnie zacznie migracji metadanych od klasycznych do Menedżera zasobów dla migracji zasobów.

Po zakończeniu operacji Przygotuj dostępna opcja wizualizacji zasobów w obu klasyczny i Menedżera zasobów. Dla każdej usługi cloud w modelu Klasyczny wdrażania platformy Azure tworzy nazwę grupy zasobów, zawierającą wzorzec `cloud-service-name>-migrated`.

>[AZURE.NOTE] Maszyn wirtualnych, które nie znajdują się w klasycznym wirtualnej sieci zostały zatrzymane deallocated na tym etapie migracji.

### <a name="check-manual-or-scripted"></a>Sprawdzanie (ręcznie lub przy użyciu skryptu)

W kroku wyboru Opcjonalnie umożliwia konfigurację pobranego wcześniej do sprawdzania poprawności poprawne migracji. Alternatywnie możesz zalogować się do portalu i kontroli właściwości i zasoby, aby sprawdzić, czy migracja metadanych odpowiada Twoim potrzebom.

Jeśli są migrowane wirtualnej sieci, większość konfiguracji maszyn wirtualnych nie jest ponownie. W przypadku aplikacji na te maszyny wirtualne można sprawdzić, czy aplikacja jest nadal i rozpocząć pracę.

Istnieje możliwość przetestowania do monitorowania automatyzacji i operacyjne skryptom czy maszyny wirtualne działają zgodnie z oczekiwaniami i czy zaktualizowanych skryptów działa prawidłowo. Tylko operacje GET są obsługiwane, gdy zasoby są w stanie przygotowany.

Istnieje nie przedział czasu Ustaw, przed którym chcesz przekazać migracji. Może zająć ilość czasu, w tym stanie. Jednak płaszczyzny zarządzania jest zablokowany dla tych zasobów do momentu albo przerwać lub zatwierdzenie.

Jeśli zauważysz jakiekolwiek problemy, możesz zawsze przerwać migracji i wróć do modelu Klasyczny wdrożenia. Po możesz przejść z powrotem, platformy Azure zostanie otwarty operacje zarządzania kontrolnego na zasoby, dzięki czemu możesz wznowić normalne operacje na tych maszyny wirtualne w modelu Klasyczny wdrożenia.

### <a name="abort"></a>Przerwać

Przerywanie jest krok opcjonalny, używanego do odwrócić zmiany do modelu Klasyczny wdrożenia i zatrzymywania migracji.

>[AZURE.NOTE] Nie można wykonać tej operacji, po mają wyzwalane operacja zatwierdzenia.  

### <a name="commit"></a>Zatwierdź

Po zakończeniu sprawdzania poprawności możesz przekazać migracji. Zasoby nie były już wyświetlane w klasycznym i są dostępne tylko w modelu wdrożenia Menedżera zasobów. Zasoby migracją można zarządzać tylko w przypadku nowego portalu.

>[AZURE.NOTE] To jest operacji idempotent. Jeśli nie powiedzie się, zaleca się spróbuj jeszcze raz. Jeśli nadal się nie powieść, utworzyć bilet pomocy technicznej lub wpis na forum ze znacznikiem ClassicIaaSMigration naszych [maszyn wirtualnych forum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WAVirtualMachinesforWindows)w witrynie.

## <a name="frequently-asked-questions"></a>Często zadawane pytania

**Ten plan migracji wpływa na dowolny z istniejącymi usługami lub aplikacje, które działają w przypadku maszyn wirtualnych Azure?**

Wartość nie. Maszyny wirtualne (klasyczny) są w pełni obsługiwane usługi w ogólnodostępną. Nadal można rozwinąć za usługi Microsoft Azure za pomocą tych zasobów.

**Co się stanie z moim maszyny wirtualne, jeśli I nie jest planowane na temat przeprowadzania migracji w najbliższej przyszłości?**

Są firma Microsoft nie zaniechanie korzystania z istniejących API klasyczny i zasobów modelu. Chcemy ułatwić migracji, uwzględniając zaawansowane funkcje, które są dostępne w modelu wdrożenia Menedżera zasobów. Zdecydowanie zalecamy zapoznanie się [niektóre funkcje,](virtual-machines-windows-compare-deployment-models.md) które są częścią IaaS w obszarze Menedżer zasobów.

**Co oznacza ten plan migracji dla mojej istniejącej narzędzia?**

Aktualizowanie narzędzia do modelu wdrożenia Menedżera zasobów jest jednym z najważniejszych zmian, które należy uwzględnić w planach usługi migracji.

**Jak długo pakiet będzie przestoje kontrolnego zarządzania?**

To zależy od liczby zasobów, które są migrowane. W przypadku mniejszych wdrożeń (kilka dziesiątki maszyny wirtualne) całego migracji trwa mniej niż godzinę. W przypadku wdrożeń na dużą skalę (setki maszyny wirtualne) migracji może potrwać kilka godzin.

**Po migracji zasobów zostały wprowadzone w Menedżerze zasobów I Przywróć?**

Jak zasoby są w stanie przygotowany, może przerwać migracji. Wycofywanie nie jest obsługiwana, po zasobów zostały pomyślnie zmigrowane za pośrednictwem operacja zatwierdzenia.

**Można przywrócić Moje migracji w przypadku niepowodzenia operacji zatwierdzania?**

Nie można przerwać migracji, jeśli operacja zatwierdzenia nie powiedzie się. Wszystkie operacje migracji, włącznie z działaniem Zatwierdź są idempotent. Dlatego zalecamy spróbuj jeszcze raz po pewnym czasie. Jeśli możesz nadal buźkę komunikat o błędzie, utworzyć bilet pomocy technicznej lub wpis na forum znacznikiem ClassicIaaSMigration naszych [maszyn wirtualnych forum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WAVirtualMachinesforWindows)w witrynie.

**Czy muszę kupić innego elektrycznego express rozsyłania, mając za pomocą IaaS w obszarze Menedżer zasobów?**

Wartość nie. Ostatnio włączony jest [Przenoszenie obwodów ExpressRoute od klasycznych do modelu wdrożenia Menedżera zasobów](../expressroute/expressroute-move.md). Nie musisz kupić nowy obwód ExpressRoute, jeśli już istnieje.

**Co zrobić, jeśli mam zostały skonfigurowane zasady kontrola dostępu oparta na rolach dla moich klasyczny zasobów IaaS?**

Podczas migracji zasobów przekształcać od klasycznych do Menedżera zasobów. Dlatego zalecamy planowania aktualizacji zasad RBAC, które należy wystąpić po migracji.

**Co zrobić, jeśli jest używany Odzyskiwanie witryny Azure lub kopii zapasowej Azure dzisiaj?**

Aby przeprowadzić migrację komputera wirtualnych, które są włączone dla kopii zapasowej, zobacz [I wykonano kopię zapasową Moje klasyczny maszyny wirtualne w kopii zapasowej magazynu. Teraz chcę Migrowanie Moje maszyn wirtualnych w trybie klasycznym do trybu Menedżera zasobów. Jak można utworzyć kopię zapasową je w magazynu usługi odzyskiwania?](../backup/backup-azure-backup-ibiza-faq.md#i-have-backed-up-my-classic-vms-in-backup-vault-now-i-want-to-migrate-my-vms-from-classic-mode-to-resource-manager-mode-how-can-i-backup-them-in-recovery-services-vault)

**Można sprawdzić poprawność mojej subskrypcji lub zasobów, aby sprawdzić, czy jest do migracji?**

Wartość Tak. W przypadku opcji obsługiwane platformy migracji pierwszy krok w ramach przygotowań do migracji jest Sprawdź, czy zasoby są w stanie migracji. W przypadku operacji sprawdzania poprawności nie powiedzie się, wyświetlany jest komunikat z powodów, których nie można ukończyć migrację.

**Co się stanie, jeśli podczas przygotowywania zasoby IaaS do migracji w błąd przydziału uruchamianie?**

Zalecamy, aby przerwać migracji, a następnie zaloguj żądanie pomocy technicznej, aby zwiększyć przydziałów w regionie, gdzie są migrowane maszyny wirtualne. Po zatwierdzeniu żądania przydziału można uruchomić ponownie wykonywania kroków migracji.

**Jak zgłosić problem?**

Publikowanie wpisów swoje problemy i pytania dotyczące migracji w naszym [forum maszyn wirtualnych](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WAVirtualMachinesforWindows), ze słowem kluczowym ClassicIaaSMigration. Zaleca się opublikowanie swoje pytania na tym forum. Jeśli masz umowy pomocy technicznej, jesteś Zapraszamy logowania także bilet pomocy technicznej.

**Co zrobić, jeśli nie podobają nazw zasobów, które platformy wybraną podczas migracji?**

Wszystkie zasoby, które wyraźnie nadać nazwy w modelu Klasyczny wdrożenia są zachowywane podczas migracji. W niektórych przypadkach są tworzone nowe zasoby. Na przykład: dla każdej maszyn wirtualnych jest tworzona karty sieciowej. Firma Microsoft obecnie nie obsługuje możliwość sterowania nazwy tych nowych zasobów utworzone podczas migracji. Zaloguj się do głosów na tę funkcję na [forum opinii Azure](http://feedback.azure.com).

* *Komunikatu o błędzie *"maszyn wirtualnych jest raportowanie ogólny stan agent nie jest gotowa. W związku z tym nie można migrować maszyn wirtualnych. Upewnij się, że Agent maszyn wirtualnych jest raportowanie ogólny stan agenta jako gotowy"* lub *"maszyn wirtualnych zawiera rozszerzenie statusem nie jest zgłaszane z maszyn wirtualnych. W związku z tym ten maszyn wirtualnych nie można migrować."***

Ten komunikat jest Odebrano maszyn wirtualnych nie ma ruchu wychodzącego łączność z Internetem. Agent maszyn wirtualnych używa połączenia wychodzące do osiągnięcia konto Azure miejsca do magazynowania dla aktualizacja stanu agenta co pięć minut.


## <a name="next-steps"></a>Następne kroki
Teraz, gdy opis migracji klasyczny IaaS zasobów do Menedżera zasobów, możesz rozpocząć migrację zasobów.

- [Techniczne głębokości dive obsługiwane platformy migracji od klasycznych do Menedżera zasobów Azure](virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
- [Migrowanie IaaS zasoby z klasycznym do Menedżera zasobów Azure za pomocą programu PowerShell](virtual-machines-windows-ps-migration-classic-resource-manager.md)
- [Migrowanie IaaS zasoby z klasycznym do Menedżera zasobów Azure za pomocą interfejsu wiersza polecenia](virtual-machines-linux-cli-migration-classic-resource-manager.md)
- [Klonowanie klasyczny maszyny wirtualnej do Menedżera zasobów Azure za pomocą skryptów programu PowerShell społeczności](virtual-machines-windows-migration-scripts.md)
