<properties
   pageTitle="Najważniejsze wskazówki dotyczące przenoszenia Azure przedsiębiorstw | Microsoft Azure"
   description="W tym artykule opisano scaffold, którego przedsiębiorstwa mogą zapewnić bezpieczne i łatwe w zarządzaniu środowisko."
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

# <a name="azure-enterprise-scaffold---prescriptive-subscription-governance"></a>Azure enterprise scaffold - zarządzania subskrypcją porady

Przedsiębiorstw rosnącej złożoności świecie publicznej chmury dla jego elastyczność i elastyczność. Są one korzystanie z zalet chmury, aby wygenerować zysk lub optymalizacji zasobów dla firm. Microsoft Azure udostępnia wiele różnych usług, że przedsiębiorstw można utworzyć takie jak bloki konstrukcyjne w celu rozwiązania szeroką gamę obciążenia i aplikacje. 

Ale znajomość czego zacząć często jest trudne. Po podjęciu decyzji użyć Azure, często powstają na kilka pytań:

- "Jak spełniają naszych wymagania prawne dla suwerenności danych w niektórych krajach?"
- "Jak zapewnić, że ktoś nie przypadkowo zmieniony krytycznego?"
- "Jak sprawdzić, jakie wszystkich zasobów obsługuje tak I może uwzględnić ją i ponownie dokładnie rachunku?"

Perspektywa pustego subskrypcję z nie poręcze jest żmudnym. Ten spację mogą utrudniać przenoszenie Azure.

Ten artykuł zawiera punkt początkowy dla specjalistów potrzebą zarządzania i saldo go z potrzebą elastyczność. Podaj pojęcia scaffold przedsiębiorstwa prowadzącego organizacji wdrażanie i zarządzanie Azure subskrypcji. 

## <a name="need-for-governance"></a>Potrzebą zarządzania

Podczas przenoszenia Azure, musisz zająć się temat zarządzania wcześniej, aby zapewnić skuteczne użycie cloud w przedsiębiorstwie. Niestety godziny i biurokracji tworzenia systemu zarządzania pełna oznacza, że kilka grup firm przejść bezpośrednio do dostawców bez obejmujące enterprise IT. Tej metody można pozostawić przedsiębiorstwa Otwórz do luk Jeśli zasoby nie są prawidłowo zarządzane. Właściwości publicznej chmury elastyczność, elastyczność i oparte na zużycie ceny — są ważne dla grup biznesowych, które trzeba szybko spełnienia wymagań klientów (wewnętrznych i zewnętrznych). Jednak enterprise IT wymaga w celu zapewnienia efektywnego ochrony danych i systemów.

W życiu rusztowania służy do tworzenia podstawa struktury. Scaffold przewodniki ogólne konspektu i zapewnia punkty kontrolne dla stałej systemów ma zostać zainstalowany. Scaffold przedsiębiorstwa jest taki sam: zbiór elastycznych kontrolek i Azure możliwości, jakie zapewniające strukturę dla środowiska i kotwiczenie usługi oparte na chmurze publicznej. Udostępnia producenci (IT i firm grupy) foundation, aby utworzyć i dołączyć nowych usług.

Scaffold jest oparty na rozwiązania, które możemy zdobyte wiele pozyskiwaniu z klientami o różnych rozmiarach. Tych klientów z zakresu od małych organizacjach opracowania rozwiązań w chmurze, Magazyn Fortune 500 przedsiębiorstw i dostawców niezależnych oprogramowania, którzy migracji i tworzenie rozwiązań w chmurze. Scaffold przedsiębiorstwa jest "purpose-built" być elastyczne do obsługi tradycyjnych obciążenia IT i adaptacyjne obciążenia; takie jak deweloperów, którzy tworzą aplikacji oprogramowania jako z usługi (władz akredytacji bezpieczeństwa) oparty na możliwości Azure.

Scaffold przedsiębiorstwa ma być podstawą każdej nowej subskrypcji w ramach Azure. Umożliwia administratorom upewnij się, że obciążenia spełniają wymagania minimalne zarządzania organizacji bez uniemożliwia szybko osiągnięcia własnych celów biznesowych grupy i deweloperów.

> [AZURE.IMPORTANT] Zarządzanie ma kluczowe znaczenie dla efektywnego Azure. Ten artykuł jest przeznaczony dla implementacji technicznej scaffold przedsiębiorstwa, ale tylko dotykającego na szerszym proces i relacje między składnikami. Zasady zarządzania przepływał z góry na dół i zależy od firmy chce uzyskanie. Oczywiście tworzenia modelu zarządzania dla Azure zawiera przedstawicielami z IT, ale co ważne powinien mieć znaczący reprezentacja z firm grupy osób prowadzących i zabezpieczeń i zarządzanie ryzykiem. W końcu scaffold przedsiębiorstwa jest o łagodzenia ryzyka firm ułatwiające misji i celów organizacji.

Poniższa ilustracja zawiera opis składników scaffold. Podstawą zależy od pełnego plan działy, kont i subskrypcji. Kolumn składa się z zasady Menedżera zasobów i silnych standardami nazewnictwa. Pozostałe scaffold pochodzi z podstawowych funkcji Azure i funkcji tego Włącz bezpieczne i łatwe w zarządzaniu środowisko.

![składniki scaffold](./media/resource-manager-subscription-governance/components.png)

> [AZURE.NOTE] Azure rozwinęła się szybko od momentu wprowadzenia w 2008. Wzrostu wymagane przez firmę Microsoft Inżynieria członkom zespołu przemyślenia ich podejście do zarządzania i wdrażanie usług. Model Menedżera zasobów Azure została wprowadzona w 2014 i zastępuje model klasyczny wdrożenia. Menedżer zasobów umożliwia organizacjom łatwiej wdrażać, organizowanie i kontrolowania Azure zasobów. Menedżer zasobów zawiera parallelization podczas tworzenia zasobów w celu szybszego wdrażania złożonych, wzajemnie rozwiązań. Zawiera również kontrola dostępu szczegółowego i możliwości zasobów znacznika metadanymi. Firma Microsoft zaleca tworzenia wszystkich zasobów za pomocą modelu Menedżera zasobów. Scaffold przedsiębiorstwa jawnie jest przeznaczony dla modelu Menedżera zasobów.

## <a name="define-your-hierarchy"></a>Definiowanie hierarchii

Podstawą scaffold jest Azure rejestrowania przedsiębiorstwa (i Enterprise Portal). Rejestracja przedsiębiorstwa określa kształt za pomocą usług Azure w firmie i jest podstawową strukturę zarządzania. Umowa enterprise klienci będą mogli dodatkowo podzielić środowisku działy, kont i na koniec subskrypcji. Subskrypcję usługi Azure to podstawowa jednostka miejsce, w którym znajdują się wszystkie zasoby. Definiuje kilka ograniczenia Azure, takie jak liczba rdzeni zasobów, itp.

![Hierarchia](./media/resource-manager-subscription-governance/agreement.png)

Różni się co przedsiębiorstwa i hierarchii w poprzednim obrazu umożliwia znaczną elastyczność organizowania Azure w firmie. Przed wdrożeniem wskazówki zawarte w tym dokumencie, należy do hierarchii i zrozumienia wzajemnego wpływu na rozliczeń, dostęp do zasobów i złożoności.

Są trzy typowe wzorce dla rejestracji Azure:

- Wzór **funkcjonalności**

    ![funkcjonalności](./media/resource-manager-subscription-governance/functional.png)

- Wzór **jednostki biznesowej** 

    ![Business](./media/resource-manager-subscription-governance/business.png)

- Wzór **geograficznych**

    ![geograficzne](./media/resource-manager-subscription-governance/geographic.png)

Stosowanie scaffold na poziomie subskrypcji, aby rozszerzyć Zarządzanie wymagania przedsiębiorstwa do subskrypcji.

## <a name="naming-standards"></a>Standardy nazewnictwa

Standardami jest nazewnictwa pierwszego słupka: scaffold. Dobrze zaprojektowany standardami nazewnictwa umożliwiają identyfikowanie zasobów w portalu na rachunku i w ramach skryptów. Prawdopodobnie masz już standardami nazewnictwa infrastruktury lokalnego. Dodając do środowiska usługi Azure, powinno obejmować standardami nazewnictwa Azure zasobów. Nazewnictwa ułatwienia bardziej efektywne zarządzanie środowiskiem na wszystkich poziomach.

> [AZURE.TIP]
>
> - Przeglądanie i przyjmuje gdzie możliwe [wskazówki desenie i wskazówki](./guidance/guidance-naming-conventions.md). Te wskazówki ułatwia ustalenie w zrozumiałej nazewnictwa.
> - Używanie camelCasing dla nazwy zasobów (na przykład myResourceGroup i vnetNetworkName). Uwaga: Istnieje niektóre zasoby, takie jak kont miejsca do magazynowania, dostępna tylko w przypadku używania małe litery (i innych znaków specjalnych).
> - Warto rozważyć użycie Menedżera zasobów Azure zasady (opisane w następnej sekcji) aby wymusić standardami nazewnictwa.
 
## <a name="policies-and-auditing"></a>Zasady i inspekcji

Druga słupka: scaffold polega na tworzenie [zasad Menedżera zasobów Azure](resource-manager-policy.md) i [Dziennik inspekcji](resource-group-audit.md). Zasady Menedżera zasobów umożliwiają zarządzanie ryzykiem w Azure. Można zdefiniować zasady, które zapewnić suwerenności danych przez ograniczanie, wymuszanie lub inspekcja niektórych akcji. 

- Zasady jest systemem **Zezwalaj na** domyślne. Sterowanie akcje poprzez określenie i przypisywanie zasad do zasobów, które odmówić lub inspekcji akcje zasobów.
- Zasady opisano przez definicje zasad w języku definicji zasad (jeżeli to warunki).
- Możesz utworzyć zasady z JSON (Javascript Object Notation) pliki sformatowane. Po zdefiniowaniu zasady, można przypisać do określonego zakresu: subskrypcji, grupa zasobów. czy do zasobów.

Zasady mają kilka akcji, umożliwiające szerokiego podejście do swojego scenariuszy. Dostępne są następujące akcje:

-   **Odmów**: blokuje żądanie zasobu
-   **Inspekcji**: umożliwia wezwanie, ale dodanie wiersza do dziennik (który może służyć do świadczenia alerty lub wyzwalanie runbooks)
-   **Dołączanie**: Dodaje określone informacje do tego zasobu. Na przykład jeśli na zasób nie jest znacznika "CostCenter", Dodaj ten znacznik z wartością domyślną.

### <a name="common-uses-of-resource-manager-policies"></a>Typowe zastosowania zasady Menedżera zasobów

Azure zasady Menedżera zasobów są zaawansowane narzędzia w Azure zestaw narzędzi. Umożliwiają uniknąć nieoczekiwane kosztów, w celu identyfikowania Centrum kosztów dla zasobów za pomocą znakowania i upewnij się, że compliancy wymagania są spełnione. Gdy zasady są połączone z wbudowanych funkcji inspekcji, można fashion złożone i elastyczne rozwiązania. Zasady pozwalają firm, które zapewniają kontrolki obciążenia "Tradycyjny IT" i "Agile" obciążenia; takie jak opracowywania aplikacji klienta. Najczęściej używane desenie, które widzimy zasad są następujące:

- **Dane Geo zgodności suwerenności** - Azure zawiera regiony całym świecie. Często przedsiębiorstw chcesz kontrolować, gdzie są tworzone zasoby (zarówno w celu zapewnienia suwerenności danych albo upewnij się, że zasoby są tworzone zbliżony konsumentów końcowych zasobów).
- **Koszt zarządzania** — subskrypcji Azure może zawierać zasoby wielu typów i Skala. Firmy chcesz często upewnij się, że standardowa subskrypcje unikać zbyt duży zasoby, które można kosztów setki złotych miesiąc lub więcej.
- **Domyślne zarządzania za pośrednictwem wymagane znaczniki** - wymaganie znaczników jest jednym z najczęściej używanych i wysoce odpowiedniej funkcji. Korzystając z zasad Menedżera zasobów Azure przedsiębiorstw będą mogli upewnij się, że zasób jest odpowiednio oznakowane. Tagi najczęściej używane są: typ działu, właściciel zasobu i środowiska (na przykład - produkcji, testowanie, rozwój)

**Przykłady**

"Tradycyjny IT" subskrypcji dla wiersza firmie

-   Wymuszanie znaczniki dział a właściciel wszystkich zasobów
-   Ograniczanie tworzenie zasobów do regionu Ameryki Północnej
-   Ograniczanie możliwości tworzenia maszyny wirtualne serii G i klastrów HDInsight

"Adaptacyjne" środowiska dla jednostki biznesowej, tworzenie aplikacji do chmury

- Aby spełnić wymagania suwerenności danych, Zezwalaj na tworzenie zasobów tylko w określonym regionie.
- Wymuszanie znacznika środowiska wszystkich zasobów. Jeśli zasób jest tworzona bez znacznika, dołącz **środowiska: nieznany** znacznik do tego zasobu.
- Inspekcja zasoby są tworzone rejonów spoza Ameryki Północnej, ale nie uniemożliwia.
- Inspekcja podczas tworzenia zasobów kosztowych wysokiego.

> [AZURE.TIP]
>
> Najczęściej używane zasady Menedżera zasobów w organizacji jest kontrola zasobów *miejsce, w którym* można tworzyć i *typach zasobów* można tworzyć. Oprócz oferuje formanty na *miejsce* i *jakie*, wiele jednostek upewnij się, że zasoby mają odpowiednich metadanych do tyłu zużycia przy użyciu zasad. Zaleca się stosowanie zasad na poziomie subskrypcji do:
>
> - Władzą dane Geo zgodności
> - Koszt zarządzania
> - Wymagane znaczniki (powodowane potrzeba biznesowa, takie jak adres rachunku, właściciel aplikacji)
>
> Możesz zastosować dodatkowe zasady na niższym poziomie zakresu.
 
### <a name="audit---what-happened"></a>Inspekcji — co się stało?

Aby zobaczyć, jak działa środowiska, należy inspekcji aktywności użytkownika. Większość typów zasobów w Azure tworzenie dzienników diagnostycznych, które można analizować przy użyciu narzędzia dziennika lub w pakiecie zarządzania operacje Azure. Czy zbieranie dzienników aktywności w wielu subskrypcje o podanie działów lub widoku przedsiębiorstwa. Rekordy inspekcji są zarówno ważne narzędzie diagnostyczne i ważnych mechanizmu wyzwalacza zdarzeń w środowisku Azure.

Umieść dzienników aktywności za pomocą Menedżera zasobów wdrożeń umożliwiają określenie **operacji** zajęła i kto je wykonać. Dzienniki aktywności mogą pochodzić i agregacją za pomocą narzędzi, takich jak analizy dziennika.

## <a name="resource-tags"></a>Znaczniki zasobów

Jak użytkownicy w Twojej organizacji Dodawanie zasobów do subskrypcji, staje się bardziej należy skojarzyć zasobów odpowiednich działów, klientów i środowiska. Metadane można dołączać do zasobów za pomocą [znaczników](resource-group-using-tags.md). Wprowadź informacje dotyczące zasobu lub właściciela za pomocą znaczników. Znaczniki umożliwiają nie tylko agregowanie i grupowanie zasobów na różne sposoby, ale używać tych danych do celów obciążenia zwrotnego. Można oznaczać zasoby o maksymalnie 15 pary klucz: wartość. 

Znaczniki zasobów są elastyczne i powinno zostać dołączone do większości zasobów. Przykłady typowych znaczników zasobu:

- Adres rachunku
- Dział (lub jednostki biznesowej)
- Środowisko (produkcji, etapie rozwoju)
- Warstwy (warstwa, warstwy aplikacji)
- Właściciel aplikacji
- NazwaProjektu

![znaczniki](./media/resource-manager-subscription-governance/resource-group-tagging.png)

Więcej przykładów znaczników zobacz [zalecane nazewnictwa konwencje Azure zasobów](./guidance/guidance-naming-conventions.md).

> [AZURE.TIP]
>
> Tworzenie znakowania strategii, która określa w subskrypcjach usługi, jakie metadane jest wymagany dla firm, Finanse, zabezpieczenia, zarządzanie ryzykiem i ogólnego zarządzania środowiska. Należy rozważyć, co zasadę, która wymaga znakowania:
>
> - Grupy zasobów
> - Miejsca do magazynowania
> - Maszyn wirtualnych
> - Serwery środowiska i sieci web usługi aplikacji

## <a name="resource-group"></a>Grupa zasobów 

Menedżer zasobów umożliwia umieszczenia zasobów w zrozumiałej grupach koligacji rozliczeń lub naturalne zarządzanie. Jak wspomniano wcześniej, Azure występują dwa modele wdrożenia. We wcześniejszych modelu Klasyczny podstawowa jednostka zarządzania był subskrypcji. Było trudne do podziału zasobów w ramach subskrypcji, które doprowadziły do utworzenia dużej liczby subskrypcji. Z modelem Menedżera zasobów możemy pokazano wprowadzenie grup zasobów. Grupy zasobów są kontenerami zasobów, które mają wspólne cyklu życia lub udostępnić atrybut na przykład "wszystkie serwery mają SQL" lub "Aplikacji A".

Grupy zasobów nie może być zawarte w sobie i zasoby tylko mogą należeć do grupy zasobów. Można użyć niektórych akcji wszystkie zasoby w grupie zasobów. Na przykład grupa zasobów usunięcie wszystkie zasoby w grupie zasobów. Zazwyczaj należy zaznaczyć całą aplikację lub powiązanych system tej samej grupy zasobów. Na przykład aplikacji trzy warstwa o nazwie Contoso aplikacji sieci Web zawiera serwer sieci web, serwera aplikacji i program SQL server w tej samej grupy zasobów.

> [AZURE.TIP]
>
> Jak organizować grupy zasobów mogą różnić się z obciążeń pracą "Tradycyjny IT" obciążenia "Adaptacyjne IT"
>
> - "Tradycyjny IT" obciążenia najczęściej są pogrupowane według elementów w tym samym cyklu życia, takich jak aplikacji. Grupowanie na podstawie aplikacji umożliwia zarządzanie poszczególnych aplikacji.
> - "Adaptacyjne IT" obciążenia zwykle skoncentrować się na aplikacje w chmurze skierowaną klientów zewnętrznych. Grupy zasobów odzwierciedlała warstwy wdrożenia (na przykład warstwa, warstwy aplikacji) i zarządzanie.

## <a name="role-based-access-control"></a>Kontrola dostępu oparta na rolach

Prawdopodobnie zadajesz samodzielnie "kto powinien mieć dostęp do zasobów"? i "jak kontrolować dostęp?" Zależy ma zezwolenie lub brak zezwolenia dostępu do portalu Azure i kontrolowania dostępu do zasobów w portalu. 

Po początkowym opublikowano Azure, kontroli dostępu do subskrypcji zostały podstawowe: administratorem lub Współtworzenie. Dostęp do subskrypcji w klasycznym modelu wprost dostępu do wszystkich zasobów w portalu. Ten brak szerokiego sterowania doprowadziły do mnożenia subskrypcje zapewnienie poziomu kontroli dostępu rozsądne do rejestrowania Azure.

Ten mnożenia subskrypcji nie jest już potrzebne. Za pomocą pola Kontrola dostępu oparta na rolach można przypisywać użytkowników do ról standardowy (na przykład "czytnik" i "writer" typowych ról). Można także zdefiniować role niestandardowe.

> [AZURE.TIP]
>  
> - Podłącz sklepu firmy tożsamości (najczęściej Active Directory) do usługi Azure Active Directory za pomocą narzędzia AD Connect.
> - Formant administratora i Co — administrator subskrypcji przy użyciu tożsamości zarządzanych. **Nie** przypiszesz administratora i Co — administrator do nowego właściciela subskrypcji. Aby zapewnić prawa **właściciela** do grupy lub poszczególnych w zamian użyj RBAC role.
> - Dodawanie Azure użytkowników do grupy (na przykład aplikacji X właściciele) w usłudze Active Directory. Grupa zsynchronizowanej umożliwia uzyskanie członków grupy odpowiednie prawa do zarządzania grupa zasobów, zawierającą aplikację.
> - Wykonaj zasada przyznawania **jak najmniejszych uprawnień** wymaganych do oczekiwanych pracę. Na przykład:
>     - Grupa wdrażania: Grupę, której tylko niemożliwe zasobów.
>     - Zarządzanie maszyn wirtualnych: Grupy, do której ma możliwość ponownego uruchomienia maszyny wirtualne (w przypadku operacji)

## <a name="azure-resource-locks"></a>Blokowania Azure zasobów

Jak Twoja organizacja dodaje program core services z subskrypcją, staje się bardziej ważne upewnić się, że te usługi są dostępne w celu uniknięcia zakłócenia firm. [Blokowania zasobów](resource-group-lock-resources.md) umożliwiają Ogranicz operacje na miejsce, w którym modyfikowanie lub usuwanie ich woli znaczący wpływ na aplikacje lub infrastruktury chmury zasobów wysokiej wartości. Blokady można zastosować do subskrypcji, grupa zasobów lub zasobu. Zazwyczaj możesz zastosować blokady do foundational zasobów, takich jak wirtualnych sieci, bramy i konta miejsca do magazynowania. 

Blokowania zasobów jest obecnie obsługiwany dwie wartości: CanNotDelete i tylko do odczytu. CanNotDelete oznacza, że użytkownicy (z odpowiednich uprawnień) można nadal czytanie lub modyfikowanie zasobu, ale nie można go usunąć. Tylko do odczytu oznacza, że autoryzowani użytkownicy nie można usunąć ani zmodyfikować zasobu.

Aby utworzyć lub usunąć blokady zarządzania, musi mieć dostęp do `Microsoft.Authorization/*` lub `Microsoft.Authorization/locks/*` akcje.
Wbudowane ról tylko właściciel i Administrator dostępu użytkowników udzielono działaniami.

> [AZURE.TIP] Opcje sieci Core powinny być chronione z blokady. Przypadkowym usunięciom bramy VPN witryny do witryny może być katastrofalny do subskrypcji usługi Azure. Azure nie umożliwia usuwanie wirtualnej sieci, który jest używany, ale stosowanie więcej ograniczeń jest przydatne. Zasady są również ważnych utrzymania odpowiednie kontrolki. Firma Microsoft zaleca stosowanie blokady **CanNotDelete** do zasady, które są używane.
>
> - Wirtualna sieć: CanNotDelete
> - Grupa zabezpieczeń sieci: CanNotDelete
> - Zasady: CanNotDelete

## <a name="core-networking-resources"></a>Podstawowe zasoby sieciowe

Dostęp do zasobów może być wewnętrznych (w sieci) lub zewnętrznych (za pośrednictwem Internetu). Jest proste użytkownikom w organizacji przypadkowo umieścić zasoby w miejscu problem i potencjalnie złośliwy dostępu je otworzyć. Podobnie jak w przypadku urządzeń lokalna przedsiębiorstw należy dodać kontrole w celu zapewnienia, że użytkownicy Azure podejmowanie decyzji prawym. Bramą subskrypcji wskazano zasobów podstawowych, które zapewniają podstawowe kontroli dostępu. Podstawowe zasoby składają się z:

- Obiekty kontenera dla podsieci znajdują się w **wirtualnej sieci** . Chociaż nie niezbędne, często jest używany podczas łączenia aplikacjom wewnętrznych zasobów firmy.
- **Grupy zabezpieczeń sieci** są podobne do zaporę i podać reguły jak zasobu można "porozmawiać" sieci. Zapewniają kontrolę nad jak / Jeśli podsieci (lub maszyn wirtualnych) można połączyć się z Internetem lub innych podsieci w tej samej sieci wirtualnej.

![Podstawowe operacje sieciowe](./media/resource-manager-subscription-governance/core-network.png)

> [AZURE.TIP]
>  
> - Tworzenie wirtualnych sieci przeznaczonym do obciążenia skierowaną zewnętrzne i wewnętrzne skierowaną obciążenia. Tej metody zmniejsza ryzyko przypadkowego wprowadzania maszyn wirtualnych, które są przeznaczone do wewnętrznego obciążenia w obszarze przeciwległych zewnętrznych.
> - Grupy zabezpieczeń sieci są krytyczne dla tej konfiguracji. Co najmniej zablokować dostęp do Internetu z wirtualnych sieci wewnętrzne i zablokować dostęp do sieci firmowej z wirtualnych sieci zewnętrznych.

### <a name="automation"></a>Automatyzacji

Zarządzanie zasobami pojedynczo jest zarówno czasochłonne i potencjalnie błędów podatne dla niektórych działań. Azure udostępnia różne funkcje automatyzacji, w tym automatyzacji Azure, logiczny aplikacji i funkcji Azure. [Automatyzacja Azure](./automation/automation-intro.md) umożliwia administratorom tworzenie i definiowanie runbooks do obsługi typowych zadań zarządzania zasobami. Możesz utworzyć runbooks za pomocą edytora kodu programu PowerShell lub graficznego edytora. Służy do tworzenia złożonych wielu etapach przepływy pracy. Azure automatyzacji często używany do obsługi typowych zadań, takich jak zamykanie nieużywanych zasobów lub tworzenie zasobów w odpowiedzi na określonego wyzwalacza bez konieczności interwencja.

> [AZURE.TIP]
>
> - Utwórz konto Azure automatyzacji i przejrzyj dostępne runbooks (linia zarówno graficznych i polecenia) dostępne w [Galerii działań aranżacji](./automation/automation-runbook-gallery.md).
> - Importowanie i dostosowywanie klucza runbooks do własnych potrzeb.
>
> Typowy scenariusz jest możliwość maszyn wirtualnych Start-zamknięcia zgodnie z harmonogramem. Istnieje runbooks przykład, że są dostępne w galerii zarówno obsłudze tego scenariusza i nauki sposób ją rozwinąć.


## <a name="azure-security-center"></a>Centrum zabezpieczeń Azure

Być może jedną z najważniejszych blokują do chmury wdrażania został problemy zabezpieczeniami. Menedżerów ryzyka i działy zabezpieczeń konieczne upewnij się, że są bezpieczne zasobów w Azure. 

[Centrum zabezpieczeń Azure](./security-center/security-center-intro.md) udostępnia centralne widok stanu zabezpieczeń zasobów w subskrypcje i zawiera wskazówki, które pomagają chronić złamany zasobów. Można go włączyć bardziej szczegółowe zasady (na przykład zastosowania zasady do określonych grup zasobów umożliwiające enterprise, aby dostosować ich postawie ryzyko, które są one adresowania). Na koniec Centrum zabezpieczeń Azure to platforma otwarta, który umożliwia tworzenie oprogramowania podłączana do Centrum zabezpieczeń Azure rozszerza możliwości partnerów firmy Microsoft i niezależnych dostawców oprogramowania. 

> [AZURE.TIP]
>
> Centrum zabezpieczeń Azure jest domyślnie włączona w każdej subskrypcji. Jednak należy włączyć zbieranie danych w środowisku maszyn wirtualnych systemu, aby umożliwić Centrum zabezpieczeń Azure zainstalować jego agenta i rozpocząć zbieranie danych.
>
> ![zbieranie danych](./media/resource-manager-subscription-governance/data-collection.png)

## <a name="next-steps"></a>Następne kroki

- Teraz, gdy znasz dotyczące zarządzania subskrypcją, nadszedł czas na Zobacz tych zaleceń w praktyce. Zobacz [Przykłady stosowania zarządzania Azure subskrypcji](resource-manager-subscription-examples.md).

*[Karl Kuhnhausen](https://github.com/karlkuhnhausen) przyczynić się do tego tematu.*
