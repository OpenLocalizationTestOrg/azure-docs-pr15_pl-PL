<properties
    pageTitle="Azure Słownik — słownik Azure | Microsoft Azure"
    description="Opis terminologii chmury na platformie Azure za pomocą Azure słownika. Ten krótki słownik Azure definicje typowych warunków cloud Azure."
    keywords="Azure słownik, terminologia w chmurze, Azure słownik, definicje terminologii, warunków cloud"
    services="na"
    documentationCenter="na"
    authors="MonicaRush"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="monicar"/>


# <a name="microsoft-azure-glossary-a-dictionary-of-cloud-terminology-on-the-azure-platform"></a>Słownik Microsoft Azure: słownika terminologia chmury na platformie Azure

Słownik Microsoft Azure jest krótki słownika terminologia chmury na platformie Azure.

## <a name="find-service-definitions-and-other-cloud-terms"></a>Wyszukiwanie definicji usługi i innych terminów związanych z chmury

* Definicje usług Azure i ich AWS odpowiedników zobacz [platformy Microsoft Azure i Amazon usług sieci Web](https://azure.microsoft.com/campaigns/azure-vs-aws/mapping/).

* Ogólne branżowe warunków cloud uzyskać [warunków środowiska chmury](https://azure.microsoft.com/overview/cloud-computing-dictionary/).

Azure słownik z powyższych dwóch odwołań zapewnia taksonomii zakończenia do końca Azure i industry chmury.  

## <a name="azure-glossary-list"></a>Lista Azure słownik


### <a name="account"></a>konta  
Pracy lub szkole lub osobistego konta Microsoft, używanym do dostępu i zarządzanie Azure subskrypcji.  
Zobacz też, [jak Azure subskrypcje są skojarzone z usługi Azure Active Directory](./active-directory/active-directory-how-subscriptions-associated-directory.md)


### <a name="availability-set"></a>Konfigurowanie dostępności  
Kolekcja maszyn wirtualnych, które są razem zarządzane o podanie nadmiarowości aplikacji i niezawodności. Korzystanie z zestawu dostępność sprawia, że w podczas zdarzeń konserwacji planowanego lub niezaplanowane co najmniej maszyn wirtualnych jest dostępna.  
Zobacz też [Zarządzanie dostępność maszyn wirtualnych systemu Windows](./virtual-machines/virtual-machines-windows-manage-availability.md) lub [Zarządzanie dostępność maszyn wirtualnych Linux](./virtual-machines/virtual-machines-linux-manage-availability.md)


### <a name="classic-model"></a>Model Azure klasyczny wdrażania  
Jeden z dwóch [modeli wdrażania](resource-manager-deployment-model.md) umożliwia wdrażanie zasobów w Azure (nowy model jest Menedżer zasobów Azure). Niektóre zasoby Azure mogą być rozmieszczone w jednym modelu lub inne, podczas gdy inne osoby mogą być rozmieszczone w obu modeli. Wskazówki dotyczące szczegółów poszczególnych Azure zasobów, które model(s) zasób może zostać wdrożony z.


### <a name="cli"></a>Azure interfejs wiersza polecenia (polecenie)  
[Interfejs wiersza polecenia](xplat-cli-install.md) służącej do zarządzania usług Azure z systemu Windows, OSX i Linux oraz komputerach.


### <a name="powershell"></a>Azure programu PowerShell  
[Interfejs wiersza polecenia](powershell-install-configure.md) do zarządzania usług Azure za pomocą wiersza polecenia na komputerach z systemem Windows. Niektóre usługi lub funkcje usługi można zarządzać tylko przy użyciu programu PowerShell lub interfejsu wiersza polecenia. Wskazówki dotyczące poszczególnych szczegóły poszczególnych Azure zasobów, które model(s) zasób może zostać wdrożony z.   
Zobacz też, [jak zainstalować i skonfigurować Azure programu PowerShell](powershell-install-configure.md)


### <a name="arm-model"></a>Model wdrażania usługi Azure Menedżera zasobów  
Jeden z dwóch [modeli wdrażania](resource-manager-deployment-model.md) umożliwia wdrażanie zasobów w Microsoft Azure (drugi jest model wdrożenia klasyczny). Niektóre zasoby Azure mogą być rozmieszczone w jednym modelu lub inne, podczas gdy inne osoby mogą być rozmieszczone w obu modeli. Wskazówki dotyczące szczegółów poszczególnych Azure zasobów, które model(s) zasób może zostać wdrożony z.


### <a name="fault-domain"></a>błąd domeny  
Kolekcja maszyn wirtualnych w zestawie dostępność niekiedy może się nie powieść w tym samym czasie. Przykładem jest grupy komputerów w stojaków, które współużytkują typowe przełącznika źródła i sieci power. Platformy Azure maszyn wirtualnych w zestawie dostępność automatycznie są oddzielone wielu domenach błędów.  
Zobacz też [Zarządzanie dostępność maszyn wirtualnych systemu Windows](./virtual-machines/virtual-machines-windows-manage-availability.md) lub [Zarządzanie dostępność maszyn wirtualnych Linux](./virtual-machines/virtual-machines-linux-manage-availability.md)  


### <a name="geo"></a>Geo  
Zdefiniowane ograniczenie siedziby danych, zwykle zawierający dwa lub więcej regionów. Granice mogą być w obrębie lub poza granice państwowe i są zależne od Reguła podatkowa. Co geo zawiera co najmniej jeden region. Przykłady geos części Azji i Japonii. Skrót *geograficznych*.  
Zobacz też [regionów Azure](best-practices-availability-paired-regions.md)


### <a name="geo-replication"></a>Geo replikacji  
Proces automatycznie replikacji zawartości, takiej jak obiektów blob, tabel i kolejek w obrębie pary regionalne.  
Zobacz też [Active Geo Replikacja bazy danych Azure SQL](./sql-database/sql-database-geo-replication-overview.md)


### <a name="image"></a>Obraz  
Plik, który zawiera system operacyjny i Konfiguracja aplikacji, który może być używany do tworzenia dowolnej liczby maszyn wirtualnych. W Azure istnieją dwa typy obrazów: maszyn wirtualnych obraz i obraz systemu operacyjnego. Obraz maszyn wirtualnych zawiera system operacyjny i wszystkie dyski dołączone maszyn wirtualnych po utworzeniu obrazu. Obraz systemu operacyjnego zawiera tylko uogólniony system operacyjny z nie ma żadnych danych konfiguracji dysku.  
Zobacz też [Nawigacja i wybierz pozycję obrazy maszyn wirtualnych systemu Windows Azure za pomocą programu PowerShell lub interfejsu wiersza polecenia](./virtual-machines/virtual-machines-windows-cli-ps-findimage.md)


### <a name="limits"></a>ograniczenia  
Liczba zasobów, które można utworzyć lub testu wydajności, który można osiągnąć. Limity są zwykle skojarzone z subskrypcji, usług i oferty.  
Zobacz też [Azure subskrypcji i limity dotyczące usługi, przydziały oraz ograniczenia](azure-subscription-service-limits.md)


### <a name="load-balancer"></a>Usługa równoważenia obciążenia  
Zasób rozdziela przychodzący ruch między komputerami w sieci. Platformy Azure równoważenia obciążenia rozdziela ruch do maszyn wirtualnych zdefiniowane w zestawie równoważenia obciążenia. [Usługa równoważenia obciążenia](./load-balancer/load-balancer-overview.md) może być z Internetu lub może być wewnętrzny.  


### <a name="offer"></a>oferty  
Cennik, środków i powiązanych warunków stosowanych do subskrypcji usługi Azure.  
Zobacz [Azure strony Szczegóły oferty](https://azure.microsoft.com/support/legal/offer-details/)


### <a name="portal"></a>Portal  
Bezpieczny portalu sieci Web umożliwia wdrażanie i zarządzanie nią usług Azure.  Istnieją dwa portali: [Azure portal](http://portal.azure.com/) i [Klasyczny portalu](http://manage.windowsazure.com/). Niektóre usługi są dostępne w obu portali, podczas gdy inne osoby są dostępne tylko w jednym lub w drugim. [Diagram dostępności portal Azure](https://azure.microsoft.com/features/azure-portal/availability/) Lista usług, które są dostępne w których portalu.  


### <a name="region"></a>region  
Obszar w obrębie geo, czy nie pośrednio krajowych obramowania, który zawiera co najmniej jeden centrach danych. Ceny i usługi regionalne i typów oferty są udostępniane na poziomie region. Obszar zazwyczaj wraz z innego regionu, który może zawierać do kilku mila kilkuset z dala od komputera, do formularza parę regionalne. Pary regionalne może służyć jako mechanizm odzyskiwanie i scenariusze wysokiej dostępności. Także nazywane ogólnie *lokalizacji*.  
Zobacz też [regionów Azure](best-practices-availability-paired-regions.md)


### <a name="resource"></a>zasób  
Element, który jest częścią usługi Azure rozwiązanie. Każda usługa Azure umożliwia wdrażanie różnych typów zasobów, takich jak bazy danych lub maszyn wirtualnych.   
Zobacz też [Omówienie Menedżera zasobów Azure](azure-resource-manager/resource-group-overview.md)


### <a name="resource-group"></a>Grupa zasobów  
Kontener w Menedżerze zasobów zawierający zasoby pokrewne dla aplikacji. Grupa zasobów może zawierać wszystkie zasoby dla aplikacji lub tylko tych zasobów, które są logicznie pogrupowane. Możesz zdecydować, jak chcesz przydzielić zasoby do grup zasobów według co sprawia, że najbardziej odpowiednie dla Twojej organizacji.  
Zobacz też [Omówienie Menedżera zasobów Azure](azure-resource-manager/resource-group-overview.md)


### <a name="arm-template"></a>Szablon Menedżera zasobów  
Plik JSON deklaratywnie definiuje jeden lub więcej zasobów Azure i definiuje zależności między wdrożonym zasobów. Ten szablon umożliwia wdrażanie zasobów, spójne i wielokrotnie.  
Zobacz też [szablonów do tworzenia Azure Menedżera zasobów](resource-group-authoring-templates.md)


### <a name="resource-provider"></a>dostawcy zasobów  
Usługa, która dostarcza zasobów można wdrożyć i zarządzać nimi za pomocą Menedżera zasobów. Każdy dostawca zasobów udostępnia operacje dotyczące pracy z zasobów, które są wdrożone. Zasób dostawców są dostępne za pośrednictwem portalu Azure, Azure programu PowerShell i kilka SDK programowania.  
Zobacz też [Omówienie Menedżera zasobów Azure](azure-resource-manager/resource-group-overview.md)


### <a name="role"></a>Rola  
Sposób kontrolowania dostępu, które mogą być przypisane do użytkowników, grupy i usług. Role będą mogli wykonać czynności, takie jak tworzenie, zarządzanie i czytanie Azure zasobów.  
Zobacz też [RBAC: wbudowane role](./active-directory/role-based-access-built-in-roles.md)


### <a name="sla"></a>Umowa dotycząca poziomu usług (SLA)  
Umowa opisującą zobowiązań firmy Microsoft łączności i czas pracy. Każda usługa Azure ma określone Umowa dotycząca poziomu usług.  
Zobacz też [umów dotyczących poziomu usług](https://azure.microsoft.com/support/legal/sla/)


### <a name="storage-account"></a>Konto miejsca do magazynowania  
Konto miejsca do magazynowania, które zapewnia dostęp do usług obiektów Blob platformy Azure, kolejki tabeli i plik w magazynie Azure. Konta miejsca do magazynowania przewiduje unikatowych nazw obiektów Azure miejsca do magazynowania danych.  
Zobacz też [o Azure miejsca do magazynowania konta](./storage/storage-create-storage-account.md)


### <a name="subscription"></a>subskrypcji  
Umowa klienta z firmą Microsoft, który pozwala na uzyskanie usług Azure. Subskrypcja ceny i warunki powiązanych podlegają oferty wybrany dla subskrypcji. Zobacz [Umowy subskrypcji Online firmy Microsoft](https://azure.microsoft.com/support/legal/subscription-agreement/).  
Zobacz też, [jak Azure subskrypcje są skojarzone z usługi Azure Active Directory](./active-directory/active-directory-how-subscriptions-associated-directory.md)


### <a name="tag"></a>znacznik  
Indeksowania termin, który umożliwia kategoryzowanie zasobów zgodnie z własnymi potrzebami za zarządzanie lub rozliczenia. Za pomocą znaczników, gdy zbiór złożonych grupy zasobów i zasoby, i musisz wizualizowanie aktywa w taki sposób, który najlepiej tworzonemu. Na przykład może oznaczać zasoby, które służą podobne ról w organizacji lub należą do tego samego działu.  
Zobacz też [Używanie znaczników w celu organizowania Azure zasobów](resource-group-using-tags.md)


### <a name="update-domain"></a>Aktualizowanie domeny  
Kolekcja maszyn wirtualnych w zestawie dostępności, które zostały zaktualizowane w tym samym czasie. Maszyn wirtualnych w tej samej domeny aktualizacji ponownego uruchomienia razem podczas planowanej konserwacji. Zablokowanie Azure ponownego uruchomienia więcej niż jedną domenę aktualizacji naraz. Także nazywane uaktualnienia domeny.  
Zobacz też [Zarządzanie dostępność maszyn wirtualnych systemu Windows](./virtual-machines/virtual-machines-windows-manage-availability.md) lub [Zarządzanie dostępność maszyn wirtualnych Linux](./virtual-machines/virtual-machines-linux-manage-availability.md)  


### <a name="vm"></a>maszyn wirtualnych  
Implementacja oprogramowania fizycznie komputerze, na którym jest uruchomiony system operacyjny. Wiele maszyn wirtualnych może być równocześnie na tym samym sprzęcie. Platformy Azure maszyn wirtualnych są dostępne w różnych rozmiarach.  
Zobacz też [dokumentacji maszyn wirtualnych](https://azure.microsoft.com/documentation/services/virtual-machines/)


### <a name="vm-extension"></a>rozszerzenie maszyn wirtualnych  
Zasób, który zawiera zachowania lub funkcje, które zapewniają albo innych programów, pracy lub umożliwiają korzystanie z komputera. Na przykład można rozszerzenia dostępu do maszyn wirtualnych Aby zresetować lub zmodyfikuj wartości dostępu zdalnego na Azure maszyn wirtualnych.  
Zobacz też [temat rozszerzenia maszyn wirtualnych i funkcje (Windows)](./virtual-machines/virtual-machines-windows-extensions-features.md) lub [rozszerzenia maszyn wirtualnych i funkcje (Linux).](./virtual-machines/virtual-machines-linux-extensions-features.md)


### <a name="vnet"></a>wirtualna sieć  
Sieć zapewnia łączność między usługi Azure zasoby, które się odizolowane od innych dzierżaw Azure. Mogą być połączone, innych Azure wirtualnych sieci przez [Brama VPN Azure](./vpn-gateway/vpn-gateway-about-vpngateways.md) i sieci lokalnej przy użyciu [wiele opcji](./vpn-gateway/vpn-gateway-cross-premises-options.md). W pełni można kontrolować bloki adresów IP, ustawień DNS, zasady zabezpieczeń i trasy do tabel w obrębie tej sieci.  
Zobacz też [Omówienie wirtualnej sieci](./virtual-network/virtual-networks-overview.md)  

###<a name="see-also"></a>**Zobacz też**  
- [Rozpoczynanie pracy z platformy Azure](https://azure.microsoft.com/get-started/)
- [Centrum zasobów w chmurze](https://azure.microsoft.com/resources/)  
- [Azure aplikacji biznesowych](https://azure.microsoft.com/overview/business-apps-on-azure/)
- [Azure w centrum danych](https://azure.microsoft.com/overview/business-apps-on-azure/) 





