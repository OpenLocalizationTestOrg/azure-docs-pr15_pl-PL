<properties
   pageTitle="Scenariusze i przykłady dotyczące zarządzania subskrypcją | Microsoft Azure"
   description="Przykłady sposobu Implementowanie zarządzania Azure subskrypcji dla typowych scenariuszy."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="rdendtler"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/05/2016"
   ms.author="rodend;karlku;tomfitz"/>

# <a name="examples-of-implementing-azure-enterprise-scaffold"></a>Przykłady stosowania scaffold Azure przedsiębiorstwa

Ten temat zawiera przykłady jak zaimplementować jednostka zalecenia dotyczące [scaffold Azure przedsiębiorstwa](resource-manager-subscription-governance.md). Aby przedstawić najważniejsze wskazówki dotyczące typowych scenariuszy używa fikcyjnej firmy o nazwie Contoso. 

## <a name="background"></a>Tła

Contoso jest na całym świecie firma, która zapewnia rozwiązań łańcuch dostaw dla klientów w dowolnych danych z modelu "Jako usługa oprogramowania" do modelu detaliczny wdrożony w lokalnej.  Na całym świecie z centrów znaczną rozwoju w Indie, Stany Zjednoczone i Kanada ich opracowania oprogramowania. 

Część model firmy jest podzielony na kilka jednostek biznesowych niezależne, które zarządzają produktów w znaczącej business. Każdy jednostka biznesowa ma własny deweloperów, menedżerów produktów i architektów. 

Jednostka biznesowa usług technologii przedsiębiorstwa (ETS) zapewnia scentralizowane możliwości IT i zarządza kilka centrach danych, w których jednostek biznesowych, host swoje aplikacje. Wraz z zarządzania centrami danych, organizacji ETS zapewnia i zarządza scentralizowane współpracy (na przykład pocztę e-mail i witrynami sieci Web) i usług sieci i telefonii. Są również zarządzać obciążenia klienta skierowaną mniejszych jednostek biznesowych, którzy nie mają operacyjnych dla personelu. 

W tym temacie są używane następujące personas:

- Dave jest administratorem ETS Azure.
- Alicja jest firmy Contoso Dyrektor rozwoju w w jednostce biznesowej łańcuch dostaw. 

Contoso musi tworzenia aplikacji programu LOB i aplikacji skierowaną klienta. Przyjęła do uruchamiania aplikacji Azure. Dave odczytuje temat [zarządzania subskrypcją porady](resource-manager-subscription-governance.md) i jest teraz gotowy do wykonania zalecenia. 

## <a name="scenario-1-line-of-business-application"></a>Scenariusz 1: Linia biznesowych aplikacji

Contoso tworzy systemu zarządzania kodu źródłowego (BitBucket) może być używany przez deweloperów w dowolnym miejscu na świecie.  Aplikacja używa infrastruktury jako usługa (IaaS) do obsługi i składa się z serwerami sieci web i serwera bazy danych. Deweloperzy uzyskują dostęp do serwerów w ich środowiskach programistycznych, ale nie potrzebują dostępu do serwerów w Azure. Contoso ETS chce umożliwia zarządzanie aplikacji właściciela aplikacji i zespołu. Aplikacja jest dostępna tylko podczas w sieci firmowej firmy Contoso. Dave należy skonfigurować subskrypcji dla tej aplikacji. Subskrypcja będzie także obsługiwać innego oprogramowania związane z Deweloper w przyszłości.  

### <a name="naming-standards--resource-groups"></a>Standardy nazewnictwa i grupy zasobów

Dave tworzy subskrypcji do obsługi narzędzi Deweloper, które są wspólne dla całej wszystkich jednostek biznesowych. Musi nadane nazwy opisowe dla grup zasobów i subskrypcji (w przypadku aplikacji i sieci). Tworzy on następujące grupy zasobów i subskrypcji:

| Element | Nazwa | Opis |
| ---- | ---- | ----------- |
| Subskrypcji | Contoso ETS DeveloperTools produkcji | Obsługa typowych narzędzi dla deweloperów |
| Grupa zasobów | rgBitBucket | Zawiera serwer aplikacji sieci web i serwer bazy danych |
| Grupa zasobów | rgCoreNetworks | Zawiera wirtualnych sieci i połączenie bramy witryny do witryny |


### <a name="role-based-access-control"></a>Kontrola dostępu oparta na rolach

Po utworzeniu tej subskrypcji, Dave chce upewnić się, że odpowiednie zespołów i właściciele aplikacji ma dostęp do ich zasobów. Dave rozpoznaje, że każdy zespół ma odmienne wymagania. Osoba wykorzystuje grupy, które zostały zsynchronizowane z firmy Contoso lokalnej usługi Active Directory (AD) usługi Azure Active Directory i zawiera odpowiedni poziom dostępu do zespołów. 

Dave przypisuje następujące role subskrypcji: 

| Rola | Przypisane do | Opis |
| ---- | ----------- | ----------- |
| [Właściciel](./active-directory/role-based-access-built-in-roles.md#owner) | Zarządzany identyfikator z firmy Contoso AD | Ta nazwa jest określana za pomocą zaledwie w programie access czas (JIT) przy użyciu narzędzia Zarządzanie tożsamościami firmy Contoso i gwarantuje, że uprawnienia dostępu właściciela subskrypcji jest w pełni inspekcji. |
| [Menedżer zabezpieczeń](./active-directory/role-based-access-built-in-roles.md#security-manager) | Zabezpieczenia i ryzyka działu zarządzania | Ta rola umożliwia użytkownikom przeglądanie Centrum zabezpieczeń Azure i stan zasobów. |
| [Sieć trybu współautora](./active-directory/role-based-access-built-in-roles.md##network-contributor) | Zespołu obsługującego sieć | Ta rola umożliwia firmy Contoso zespołu obsługującego sieć VPN witryny i wirtualnych sieci do zarządzania. |
| *Niestandardowe roli* | Właściciel aplikacji | Dave tworzy roli możliwość modyfikowania zasoby w grupie zasobów. Aby uzyskać więcej informacji zobacz [Role niestandardowe w Azure RBAC](./active-directory/role-based-access-control-custom-roles.md) |

### <a name="policies"></a>Zasady

Dave ma następujące wymagania dotyczące zarządzania zasobami subskrypcji:

- Ponieważ narzędzia programistyczne obsługują deweloperów całym świecie, nie chciałby uniemożliwić użytkownikom tworzenie zasobów w dowolnym regionie. Jednak musi on wiedzieć, gdzie są tworzone zasoby. 
- Jest on związanych z kosztami. Dlatego chce uniemożliwić tworzenie zbyt drogich maszyn wirtualnych właściciele aplikacji.  
- Ponieważ ta aplikacja obsługuje deweloperów w wiele jednostek biznesowych, chce znakowanie każdemu zasobowi z właścicielem jednostki i aplikacji biznesowych. Za pomocą tych znaczników, ETS można BOM odpowiednie członkom zespołu.

Tworzy on następujących [zasad Menedżera zasobów](resource-manager-policy.md): 

| Pole | Efekt | Opis |
| ----- | ------ | ----------- |
| Lokalizacja | inspekcji | Tworzenie zasobów w dowolnym regionie inspekcji |
| Typ | odmawianie | Odmówić tworzenia maszyn wirtualnych serii G |
| znaczniki | odmawianie | Wymagania aplikacji właściciela znacznika |
| znaczniki | odmawianie | Wymaganie znacznika Centrum kosztów |
| znaczniki | Dołączanie | Dołącz nazwę znacznika **jednostką biznesową** i etykieta wartość **ETS** wszystkie zasoby |


### <a name="resource-tags"></a>Znaczniki zasobów

Dave zakłada, że musi mieć określone informacje na rachunku do identyfikowania Centrum kosztów do wykonania BitBucket. Ponadto Dave chce wiedzieć wszystkie zasoby należące do ETS. 

Następujące [tagi](resource-group-using-tags.md) on dodaje do grupy zasobów i zasoby. 
 
| Nazwa znacznika | Etykieta Wartość |
| -------- | --------- |
| ApplicationOwner | Nazwa osoby zarządzającej tej aplikacji. |
| CostCenter | Centrum kosztów grupę, do której jest płacisz za zużycie Azure. |
| Jednostką biznesową | **ETS** (jednostki biznesowej skojarzone z subskrypcją) |

### <a name="core-network"></a>Podstawowe sieci

Contoso ETS informacji zabezpieczeń i ryzyka zespół zarządzający przegląda Krzysztofa oraz proponowany plan, aby przenieść aplikacji Azure. Chcą upewnić się, że aplikacja nie jest udostępniana w Internecie.  Dave też ma dewelopera aplikacji, które w przyszłości zostaną przeniesione do Azure. Te aplikacje wymagają interfejsów publicznych.  Do tych wymagań on udostępnia wirtualnych sieci wewnętrzne i zewnętrzne i grupy zabezpieczeń sieci ograniczenia dostępu.

Tworzy on następujące zasoby:

| Typ zasobu | Nazwa | Opis |
| ------------- | ---- | ----------- |
| Wirtualna sieć | vnInternal | Używana razem z aplikacją BitBucket i jest połączony za pośrednictwem ExpressRoute firmy Contoso sieci firmowej.  Podsieć (sbBitBucket) udostępnia aplikację z określonym obszarem adresów IP. |
| Wirtualna sieć | vnExternal | Dostępne dla przyszłych aplikacji, które wymagają publicznej punktów końcowych. |
| Grupa zabezpieczeń sieci | nsgBitBucket | Gwarantuje, że powierzchni atakiem obciążenie jest zminimalizowane, umożliwiając połączeń tylko na porcie 443 podsieci miejsce, w którym znajduje się aplikacja (sbBitBucket). |

### <a name="resource-locks"></a>Blokowania zasobów 

Dave rozpoznaje, że nawiązywanie połączenia z siecią firmową firmy Contoso wirtualnej sieci wewnętrznej muszą być chronione przed wayward skryptu ani przypadkowym usunięciom. 

Tworzy on następujące [Blokada zasobu](resource-group-lock-resources.md):

| Typ blokady | Zasób | Opis |
| --------- | -------- | ----------- |
| **CanNotDelete** | vnInternal | Uniemożliwia użytkownikom usuwanie wirtualnej sieci lub podsieci, ale nie uniemożliwia dodanie nowych podsieci. |

### <a name="azure-automation"></a>Azure automatyzacji 

Dave ma nic, aby zautomatyzować dla tej aplikacji. Mimo że utworzył konto Azure automatyzacji on nie pierwotnie go używać. 

### <a name="azure-security-center"></a>Centrum zabezpieczeń Azure 

Zarządzanie usługami contoso IT musi szybkie identyfikowanie wiadomości i obsługiwać zagrożeń. Chcą także zrozumieć, jakie problemach.  

W celu spełnienia tych wymagań, Dave umożliwia [Azure Centrum zabezpieczeń](./security-center/security-center-intro.md)oraz zapewnia dostęp do roli Menedżera zabezpieczeń. 

## <a name="scenario-2-customer-facing-app"></a>Scenariusz 2: skierowaną klienta aplikacji

Zarządzanie biznesowych w jednostce biznesowej łańcuch dostaw zidentyfikował różne możliwości, aby zwiększyć zaangażowanie klientom firmy Contoso za pomocą karty lojalności. Zespół Alicji musi utworzyć tej aplikacji i zdecyduje, że Azure zwiększa możliwość spełniają potrzeb biznesowych. Alicja współdziała z Dave z ETS skonfigurowanie dwóch Subskrypcje dotyczące tworzenia i używania tej aplikacji.

### <a name="azure-subscriptions"></a>Subskrypcje Azure 

Dave loguje się do portalu Enterprise Azure i widzi działu łańcuch dostaw już istnieje.  Jednak tego projektu jest pierwszym projektem dla zespołu łańcuch dostaw platformy Azure, Dave rozpoznaje potrzebę nowego konta dla zespołu opracowującego Alicji.  On tworzy konto "Badania i rozwój" dla swojego zespołu i przypisuje dostęp do Alicji. Alicja loguje się za pośrednictwem portalu konta i tworzy dwa subskrypcje: jedna przechowuje serwery projektowe, a drugi do przechowywania serwerów produkcji.  Anna obejmuje wcześniej wskazanych standardami nazewnictwa podczas tworzenia następujących subskrypcji: 

| Użyj subskrypcji | Nazwa |
| ---------------- | ---- |
| Opracowywania | Rozwoju LoyaltyCard SupplyChain ResearchDevelopment |
| Produkcji | SupplyChain operacji LoyaltyCard produkcji |

### <a name="policies"></a>Zasady

Dave Alicja omówić aplikacji i określić, że ta aplikacja służy tylko klientów w regionie Ameryki Północnej.  Alicja i swojego zespołu zamierzasz utworzyć aplikację przy użyciu środowiska usługi aplikacji i Azure SQL Azure firmy. Ta osoba może być konieczne tworzenie maszyn wirtualnych w czasie projektowania.  Alicja chce, aby zapewnić jego deweloperów zasoby, które są potrzebne do Eksplorowanie i badanie problemów bez pobierania danych w ETS. 

Dla **subskrypcji rozwoju**tworzą następujących zasad:

| Pole | Efekt | Opis |
| ----- | ------ | ----------- |
| Lokalizacja | inspekcji | Przeprowadzanie inspekcji tworzenia zasobów w dowolnym regionie. |

Nie ograniczaj typu sku utworzone przez użytkownika w fazie projektowania, a nie wymagają znaczniki dla grup zasobów lub zasobów.

W przypadku **subskrypcji produkcji**tworzą następujących zasad:

| Pole | Efekt | Opis |
| ----- | ------ | ----------- |
| Lokalizacja | odmawianie | Odmowa tworzenia zasobów poza USA centrach danych. |
| znaczniki | odmawianie | Wymagania aplikacji właściciela znacznika |
| znaczniki | odmawianie | Wymaganie znacznika działu. | 
| znaczniki | Dołączanie | Dołączanie znacznik danych do każdej grupy zasobów, która wskazuje środowisku produkcyjnym. |

Nie ograniczaj typu sku utworzone przez użytkownika w produkcji.

### <a name="resource-tags"></a>Znaczniki zasobów 

Dave zakłada, że musi być określonych informacji do identyfikowania grup poprawne firm rozliczeń i własności. Określa on tagi zasobów dla grupy zasobów i zasoby. 
 
Nazwa znacznika | Etykieta Wartość |
| -------- | --------- |
| ApplicationOwner | Nazwa osoby zarządzającej tej aplikacji. |
| Dział | Centrum kosztów grupę, do której jest płacisz za zużycie Azure. |
| EnvironmentType | **Produkcji** (Nawet jeśli subskrypcja obejmuje **produkcji** w nazwie, łącznie z tego tagu umożliwia łatwą identyfikację podczas przeglądania zasobów w portalu lub na rachunku.) |

### <a name="core-networks"></a>Podstawowe sieci

Contoso ETS informacji zabezpieczeń i ryzyka zespół zarządzający przegląda Krzysztofa oraz proponowany plan, aby przenieść aplikacji Azure. Chcą upewnić się, że samodzielnie i chronione w sieci strefy Zdemilitaryzowanej aplikacji lojalności karty.  Aby zrealizować to wymaganie, Krzysztofa i Alicja Tworzenie wirtualnych sieci zewnętrznej i grupy zabezpieczeń sieci w celu wyodrębnienia aplikacji lojalności karty sieci firmy Contoso.  

Dla **subskrypcji rozwoju**tworzą:

| Typ zasobu | Nazwa | Opis |
| ------------- | ---- | ----------- |
| Wirtualna sieć | vnInternal | Służy karta lojalności Contoso środowisko projektowania i jest połączony za pośrednictwem ExpressRoute firmy Contoso sieci firmowej. |

W przypadku **subskrypcji produkcji**tworzą:

| Typ zasobu | Nazwa | Opis |
| ------------- | ---- | ----------- |
| Wirtualna sieć | vnExternal | Obsługuje aplikację lojalności karty, a nie jest podłączone bezpośrednio do firmy Contoso ExpressRoute. Kod zostanie przypisany przez system ich kodu źródłowego bezpośrednio do usługi PaaS. |
| Grupa zabezpieczeń sieci | nsgBitBucket | Gwarantuje, że powierzchni atakiem obciążenie jest zminimalizowane, tylko umożliwiając komunikacji przychodzącego na port TCP 443.  Contoso bada, za pomocą zapory aplikacji sieci Web dla dodatkowej ochrony. |  

### <a name="resource-locks"></a>Blokowania zasobów 

Dave Alicja przyznaje i zdecydować dodać blokowania zasobów na niektóre z najważniejszych zasobów w środowisku zapobiec przypadkowym usunięciom podczas wypychanych wadliwe kodu. 

Tworzą blokady następujące czynności:

| Typ blokady | Zasób | Opis |
| --------- | -------- | ----------- |
| **CanNotDelete** | vnExternal | Aby uniemożliwić użytkownikom usunięcie wirtualnej sieci lub podsieci. Blokady nie uniemożliwia dodanie nowych podsieci. |

### <a name="azure-automation"></a>Azure automatyzacji 

Alicja i jej do zespołu opracowującego mają rozległa runbooks Zarządzanie środowiska dla tej aplikacji. Runbooks umożliwia dodawanie/usuwanie węzłów stosowania oraz inne zadania DevOps. 

Aby użyć tych runbooks, umożliwiają [automatyzację](./automation/automation-intro.md).

### <a name="azure-security-center"></a>Centrum zabezpieczeń Azure 

Zarządzanie usługami contoso IT musi szybkie identyfikowanie wiadomości i obsługiwać zagrożeń. Chcą także zrozumieć, jakie problemach.  

W celu spełnienia tych wymagań, Dave umożliwia Centrum zabezpieczeń Azure. Zapewnia on Azure Centrum zabezpieczeń monitoruje zasoby i zapewnia dostęp do zespołów DevOps i zabezpieczenia. 

## <a name="next-steps"></a>Następne kroki

- Aby uzyskać informacje dotyczące tworzenia szablonów Menedżera zasobów, zobacz [Najważniejsze wskazówki dotyczące tworzenia szablonów Azure Menedżera zasobów](resource-manager-template-best-practices.md).

*[Karl Kuhnhausen](https://github.com/karlkuhnhausen) przyczynić się do tego tematu.*
