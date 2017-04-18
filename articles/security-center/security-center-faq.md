<properties
   pageTitle="Często zadawane pytania Centrum zabezpieczeń Azure | Microsoft Azure"
   description="Niniejszy artykuł zawiera odpowiedzi na pytania dotyczące Centrum zabezpieczeń Azure."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/27/2016"
   ms.author="terrylan"/>

# <a name="azure-security-center-frequently-asked-questions-faq"></a>Centrum zabezpieczeń Azure — często zadawane pytania

Niniejszy artykuł zawiera odpowiedzi na pytania na temat Centrum zabezpieczeń Azure, usługa, która ułatwia zapobieganie i odpowiadanie na zagrożenia za pomocą lepszą wgląd i kontroli nad zabezpieczeniami zasobów Microsoft Azure.

## <a name="general-questions"></a>Ogólne pytania

### <a name="what-is-azure-security-center"></a>Co to jest Centrum zabezpieczeń Azure?
Centrum zabezpieczeń Azure ułatwia zapobieganie, wykrywanie i odpowiadanie na zagrożenia za pomocą lepszą wgląd i kontroli nad zabezpieczeniami Azure zasobów. Zapewnia zabezpieczenia zintegrowane monitorowania i zasad zarządzania w subskrypcjach usługi, pomaga wykrywać zagrożenia, które w przeciwnym razie przejdź niezauważalnie i współpraca z Szeroki ekosystem rozwiązania zabezpieczeń.

### <a name="how-do-i-get-azure-security-center"></a>Jak uzyskać Centrum zabezpieczeń Azure?
Centrum zabezpieczeń Azure jest włączone za subskrypcję usługi Microsoft Azure i uzyskać do nich dostęp z [portalu Azure](https://azure.microsoft.com/features/azure-portal/). ([Zaloguj się do portalu](https://portal.azure.com), wybierz pozycję **Przeglądaj**i przewiń do **Centrum zabezpieczeń**).  

## <a name="billing"></a>Rozliczenia

### <a name="how-does-billing-work-for-azure-security-center"></a>Jak działa rozliczeń Centrum zabezpieczeń Azure?
Centrum zabezpieczeń jest oferowane w dwa poziomy: bezpłatnych i standardowy.

Bezpłatne warstwa umożliwia ustawianie zasad zabezpieczeń i odbieranie alertów zabezpieczeń, zdarzeń i zalecenia, które przeprowadzają użytkownika przez proces konfigurowania potrzebnych formantów. Przy użyciu bezpłatnego warstwa można również monitorować stan zabezpieczeń usługi Azure zasobów i rozwiązań partnerów zintegrowany z subskrypcji usługi Azure.

Standardowy warstwie znajdują się wykryć zaawansowane funkcje plus bezpłatne warstwa: zagrożenie analizy, funkcjonalne analizy awarii analizy i wykrywania anomalii. Bezpłatne 90-dniowa wersja próbna warstwie standardowy jest dostępna. Aby uaktualnić, zaznacz cennik warstwa w [Zasady zabezpieczeń](security-center-policies.md#setting-security-policies-for-subscriptions). Aby dowiedzieć się więcej, zobacz [Centrum zabezpieczeń ceny](security-center-pricing.md) .

## <a name="data-collection"></a>Zbieranie danych

Centrum zabezpieczeń zbiera dane z maszyn wirtualnych w celu oceny stanu zabezpieczeń, podaj zalecenia dotyczące zabezpieczeń i powiadomienia na zagrożenia. Po raz pierwszy uzyskujesz dostęp do Centrum zabezpieczeń, zbierania danych jest włączona w przypadku wszystkich maszyn wirtualnych w ramach subskrypcji. Zbieranie danych jest zalecane, ale użytkownik może rezygnacji przez [Wyłączenie zbieranie danych](#how-do-i-disable-data-collection) w Centrum zabezpieczeń zasad.

### <a name="how-do-i-disable-data-collection"></a>Jak wyłączyć zbierania danych?

**Zbieranie danych** można wyłączyć dla subskrypcji w zasadach zabezpieczeń w dowolnym momencie. ([Zaloguj się do portalu Azure](https://portal.azure.com), wybierz pozycję **Przeglądaj**, wybierz pozycję **Centrum zabezpieczeń**i wybierz **zasadę**.)  Po wybraniu subskrypcji nowej kart zostanie otwarty i oferuje opcję, aby wyłączyć **zbioru danych** . Wybierz opcję **Usuwanie agentów** na Wstążce górny usunąć czynników z istniejących maszyn wirtualnych.

> [AZURE.NOTE] Można ustawić zasady zabezpieczeń na poziomie grupy zasobów i poziom subskrypcji Azure, ale musisz wybrać subskrypcji, aby wyłączyć zbioru danych.

### <a name="how-do-i-enable-data-collection"></a>Jak włączyć zbierania danych?
Zbieranie danych można włączyć dla subskrypcjach Azure w zasad zabezpieczeń. Aby włączyć zbierania danych, [Zaloguj się do portalu Azure](https://portal.azure.com), wybierz pozycję **Przeglądaj**, wybierz **Centrum zabezpieczeń**, a następnie wybierz **zasad**. **Zbieranie danych** **w** i konfigurowanie konta miejsca do magazynowania, w którym chcesz dane mają być pobierane do (zobacz pytanie "[miejsce, w którym dane są przechowywane?](#where-is-my-data-stored)"). **Zbieranie danych** jest włączona, automatycznie zbiera informacje o konfiguracji i zdarzeń zabezpieczeń ze wszystkich obsługiwanych maszyn wirtualnych w subskrypcji.

> [AZURE.NOTE] Można ustawić zasady zabezpieczeń na poziomie grupy zasobów i poziom subskrypcji Azure, ale konfiguracji zbierania danych występuje tylko na poziomie subskrypcji.

### <a name="what-happens-when-data-collection-is-enabled"></a>Co się stanie, jeśli jest włączona funkcja zbierania danych?
Zbieranie danych jest włączone za pomocą agenta monitorowania Azure i rozszerzenia monitorowanie zabezpieczeń Azure. Rozszerzenie monitorowanie zabezpieczeń Azure skanuje dla różnych konfiguracji odpowiednie zabezpieczeń i przesyła go do śledzenia [Śledzenia zdarzeń dla systemu Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW). Ponadto system operacyjny tworzy wpisy w dzienniku zdarzeń.  Agent monitorowania Azure odczytuje wpisy dziennika zdarzeń i ETW służy do śledzenia i kopiuje je do swojego konta miejsca do magazynowania na potrzeby analizy.  Jest to konto miejsca do magazynowania, który skonfigurowano w zasadach zabezpieczeń. Aby uzyskać więcej informacji o koncie miejsca do magazynowania, zobacz pytanie "[miejsce, w którym dane są przechowywane?](#where-is-my-data-stored)"

### <a name="does-the-monitoring-agent-or-security-monitoring-extension-impact-the-performance-of-my-servers"></a>Rozszerzenie agenta monitorowania lub monitorowanie zabezpieczeń wpłynie wydajności Moje serwery?
Agent i rozszerzenia powoduje zużycie nominalna ilość zasobów systemowych i powinien mieć nieco wpływ na wydajność.

### <a name="where-is-my-data-stored"></a>Gdzie są przechowywane dane?
Dla każdego regionu, w którym masz maszyn wirtualnych z systemem możesz wybrać konto miejsca do magazynowania, w której są przechowywane dane zebrane z tych maszyn wirtualnych. Ułatwia umożliwiające zachowanie danych w tym samym obszarze geograficznym do celów suwerenności danych i prywatności. Możesz wybrać konto miejsca do magazynowania dla subskrypcji w zasad zabezpieczeń. ([Zaloguj się do portalu Azure](https://portal.azure.com), wybierz pozycję **Przeglądaj**, wybierz pozycję **Centrum zabezpieczeń**i wybierz **zasadę**.) Po kliknięciu subskrypcji zostanie wyświetlona karta nowy. Wybierz pozycję **Wybierz miejsca do magazynowania konta** zaznacz obszar.

> [AZURE.NOTE] Można ustawić zasady zabezpieczeń na poziomie grupy zasobów i poziom subskrypcji Azure, ale zaznaczenie obszaru dla Twojego konta miejsca do magazynowania występuje tylko na poziomie subskrypcji.

Aby dowiedzieć się więcej o przechowywaniu Azure i kont miejsca do magazynowania, zobacz [Dokumentację miejsca do magazynowania](https://azure.microsoft.com/documentation/services/storage/) i [o Azure miejsca do magazynowania konta](../storage/storage-create-storage-account.md).

## <a name="using-azure-security-center"></a>Korzystanie z Centrum zabezpieczeń Azure

### <a name="what-is-a-security-policy"></a>Co to są zasady zabezpieczeń?
Zasady zabezpieczeń określa zestawu kontrolek, które są zalecane dla zasobów w określonej subskrypcji lub grupa zasobów. W Centrum zabezpieczeń Azure definiowania zasad dla usługi Azure subskrypcje i grup zasobów według wymagań dotyczących zabezpieczeń firmy i typ aplikacji lub poufność danych w każdej subskrypcji.

Na przykład zasobów używanych do rozwoju lub test może być wymagania dotyczące zabezpieczeń innym niż używane do produkcji aplikacji. Ponadto aplikacje z regulowanym danych, takich jak osoby (informacje umożliwiające identyfikację) mogą wymagać wyższy poziom zabezpieczeń. Zasady zabezpieczeń w Centrum zabezpieczeń Azure będzie sterują zalecenia dotyczące zabezpieczeń i monitorowania. Aby dowiedzieć się więcej na temat zasad zabezpieczeń, zobacz [Monitorowanie Centrum zabezpieczeń Azure prawidłowość zabezpieczeń](security-center-monitoring.md).

> [AZURE.NOTE] W przypadku konfliktu między zasadami na poziomie subskrypcji i poziomu zasad grupy zasobów zasady poziomu grupy zasobów pierwszeństwo.

### <a name="who-can-modify-a-security-policy"></a>Kto może modyfikować zasady zabezpieczeń?
Zasady zabezpieczeń są skonfigurowane dla każdej subskrypcji lub grupa zasobów. Aby zmodyfikować zasady zabezpieczeń na poziomie subskrypcji lub grupa zasobów, musi być właścicielem lub współautora tej subskrypcji.

Aby dowiedzieć się, jak skonfigurować zasady zabezpieczeń, zobacz [Ustawienia zasad zabezpieczeń w Centrum zabezpieczeń Azure](security-center-policies.md).

### <a name="what-is-a-security-recommendation"></a>Co to jest rekomendacji zabezpieczeń?
Centrum zabezpieczeń Azure analizuje stan zabezpieczeń Azure zasobów. Po zidentyfikowaniu słabych zabezpieczeń są tworzone zalecenia. Zalecenia pomagają Konfigurowanie potrzebnego formantu. Przykłady:

- Inicjowania obsługi administracyjnej ochrony przed złośliwym oprogramowaniem do identyfikowania i usuwania złośliwego oprogramowania
- Konfigurowanie reguł kontrola ruchu do maszyn wirtualnych oraz [Grup zabezpieczeń sieci](../virtual-network/virtual-networks-nsg.md)
- Obsługa administracyjna zapory aplikacji sieci web, aby pomóc Ci chronić atakami kierowanie aplikacji sieci web
- Wdrażanie brakujących aktualizacji systemu
- Adresowanie konfiguracji systemu operacyjnego, które nie są zgodne zalecane plany bazowe

Tylko zalecenia, które są włączone w oknie Zasady zabezpieczeń przedstawiono poniżej.

### <a name="how-can-i-see-the-current-security-state-of-my-azure-resources"></a>Jak wyświetlić bieżący stan zabezpieczeń zasobów Azure?
Kafelka **kondycji zasobów** na karta **Centrum zabezpieczeń** zawiera ogólny postawie zabezpieczeń środowiska podziałem w środowisku maszyn wirtualnych systemu, aplikacji sieci web i innych zasobów. Każdemu zasobowi ma pokazywanie wskaźnika, jeśli określono żadnych potencjalnych luk. Kliknięcie kafelka kondycji zasobów wyświetla zasobów i służy do identyfikowania miejsce, w którym jest wymagana Uwaga lub mogą występować problemy.

### <a name="what-triggers-a-security-alert"></a>Wywołujących alert zabezpieczeń?
Centrum zabezpieczeń Azure automatycznie gromadzi, analizuje i fuses dziennika danych z usługi Azure zasobów, sieci i rozwiązań partnerów, takich jak ochrony przed złośliwym oprogramowaniem i zapory. Gdy zostaną wykryte zagrożenia, zostanie utworzona alert zabezpieczeń. Przykłady wykrywania:

- Złamany maszyn wirtualnych komunikowania się z znanego złośliwego adresów IP
- Zaawansowane złośliwego oprogramowania przy użyciu raportowania błędów systemu Windows
- Atakami przed maszyn wirtualnych
- Alerty zabezpieczeń z rozwiązań zabezpieczenia zintegrowane partnerów, takie jak ochrona przed złośliwym lub zapory aplikacji sieci Web

### <a name="whats-the-difference-between-threats-detected-and-alerted-on-by-microsoft-security-response-center-versus-azure-security-center"></a>Jaka jest różnica między zagrożeń wykryte i alerty na Microsoft Security Response Center i Centrum zabezpieczeń Azure?
Centrum odpowiedzi zabezpieczeń firmy Microsoft (gdy) wykonuje monitorowanie zabezpieczeń wybierz Azure sieci i infrastruktury i odbiera zagrożenie analizy i abuse skargi od osób trzecich. Gdy MSRC staje się pamiętać, że dane klientów uzyskano dostęp przez stronę prawem lub nieautoryzowanych lub że odbiorcy stosowania Azure nie spełnia warunków dopuszczalnego używania Menedżera zdarzenia zabezpieczeń powiadamia klienta. Powiadomienie o zazwyczaj występuje przez wysłanie wiadomości e-mail do kontaktów zabezpieczeń określone w Centrum zabezpieczeń Azure lub właścicielem subskrypcji Azure, jeśli nie został określony kontakt zabezpieczeń.

Centrum zabezpieczeń to usługa Azure nieustannie monitoruje Azure środowisko klienta i dotyczy analizy automatyczne wykrycie szeroką gamę działań potencjalnie niebezpiecznych. Te wykryć są udostępnione jako alerty zabezpieczeń w Centrum zabezpieczeń pulpitu nawigacyjnego.

### <a name="how-are-permissions-handled-in-azure-security-center"></a>Jak uprawnienia są obsługiwane w Centrum zabezpieczeń Azure?
Centrum zabezpieczeń Azure obsługuje oparta na rolach programu access. Aby dowiedzieć się więcej na temat kontrola dostępu oparta na rolach (RBAC) platformy Azure, zobacz [Kontrola dostępu oparta na rolach katalogowej Active Azure](../active-directory/role-based-access-control-configure.md).

Gdy użytkownik otwiera Centrum zabezpieczeń, tylko zalecenia i alertów, które dotyczą zasobów, do których użytkownik ma dostęp do będą widoczne. Oznacza to, że użytkownicy zobaczą tylko elementów związanych z zasobami miejsce, w którym użytkownik jest przypisana rola właściciela, współautorów lub czytnika subskrypcji lub grupa zasobów, do której należy zasób.

Jeśli chcesz:

- **Edytowanie zasad zabezpieczeń**, musi być właścicielem lub współautora subskrypcji.
- **Zastosuj zalecenia**, musi być właścicielem lub współautora subskrypcji.
- **Mają wgląd stanu zabezpieczeń we wszystkich subskrypcji**, musi być właścicielem, trybu współautora lub czytnika (administrator IT, zabezpieczenia zespołu) w każdej subskrypcji.
- **Masz dostęp do stanu zabezpieczeń zasobów**, musi być grupa zasobów właściciel współautora lub czytnika (DevOps).

### <a name="which-azure-resources-are-monitored-by-azure-security-center"></a>Zasoby, które Azure są monitorowane przez Centrum zabezpieczeń Azure?
Centrum zabezpieczeń Azure monitoruje Azure następujące zasoby:

- Wirtualna maszyn (łącznie z [Usługami w chmurze](../cloud-services/cloud-services-choose-me.md))
- Azure wirtualnych sieci
- Usługa SQL Azure
- Zintegrowany z subskrypcji usługi Azure, takich jak Zapora aplikacji sieci web na maszyny wirtualne i na [Środowisko usługi aplikacji](../app-service/app-service-app-service-environments-readme.md) rozwiązań partnerów

## <a name="virtual-machines"></a>Maszyn wirtualnych

### <a name="what-types-of-virtual-machines-will-be-supported"></a>Typy maszyn wirtualnych, jaki będzie obsługiwany?
Monitorowanie kondycji zabezpieczeń i zalecenia są dostępne dla wirtualnych maszyn utworzone za pomocą obu [Klasyczny i modeli wdrażania Menedżera zasobów](../azure-classic-rm.md).

Obsługiwane maszyny wirtualne systemu Windows:

- Windows Server 2008 R2
- System Windows Server 2012
- Windows Server 2012 R2

Obsługiwane maszyny wirtualne Linux:

- Ubuntu wersji 12.04, 14.04, 16.04
- Debian wersji 7, 8
- CentOS wersji 6. \*, 7.*
- Czerwony funkcję Enterprise Linux (RHEL) w wersji 6. \*, 7.*
- SUSE Linux Enterprise Server (SLES) wersji 11. \*, 12.*
- Wersje programu Oracle Linux 6. \*, 7.*

Uruchamianie w usłudze w chmurze maszyny wirtualne są również obsługiwane. Tylko chmury usługi ról w sieci web i Pracownik działa w produkcji, które są monitorowane gniazda. Aby dowiedzieć się więcej na temat usługi w chmurze, zobacz [Omówienie usług w chmurze](../cloud-services/cloud-services-choose-me.md).

### <a name="why-doesnt-azure-security-center-recognize-the-antimalware-solution-running-on-my-azure-vm"></a>Dlaczego Centrum zabezpieczeń Azure nie rozpoznaje ochrony przed złośliwym oprogramowaniem działającej na Moje maszyn wirtualnych Azure?

Centrum zabezpieczeń Azure ma tylko wgląd zainstalowany za pomocą Azure rozszerzenia ochrony przed złośliwym oprogramowaniem. Na przykład Centrum zabezpieczeń nie będzie mógł wykryć ochrony przed złośliwym oprogramowaniem, który był zainstalowany na podanej obraz lub jeśli zainstalowano ochrony przed złośliwym oprogramowaniem maszyn wirtualnych za pomocą procesów (na przykład konfiguracji systemów zarządzania).

### <a name="why-do-i-get-the-message-missing-scan-data-for-my-vm"></a>Dlaczego pojawia się komunikat "Brak przeglądania danych" dla mojej maszyn wirtualnych?

Może upłynąć trochę czasu (zazwyczaj mniej niż godziny) dla danych skanowania można wypełnić po włączeniu zbieranie danych w Centrum zabezpieczeń Azure. Skanowanie nie będą wypełniać dla maszyny wirtualne w stanie zatrzymania.

### <a name="why-do-i-get-the-message-vm-agent-is-missing"></a>Dlaczego otrzymuję komunikat "Agent maszyn wirtualnych jest Brak?"

Agent maszyn wirtualnych musi być zainstalowany na maszyny wirtualne w celu umożliwienia zbierania danych. Agent maszyn wirtualnych jest instalowana domyślnie dla maszyny wirtualne wdrożonych z Azure Marketplace. Aby uzyskać informacje o tym, jak zainstalować agenta maszyn wirtualnych na inne maszyny wirtualne, zobacz blogu [agenta maszyn wirtualnych i rozszerzenia](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/).
