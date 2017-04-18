<properties
   pageTitle="Zarządzanie Azure zabezpieczeń i monitorowanie omówienie | Microsoft Azure"
   description=" Azure udostępnia mechanizmy zabezpieczeń, aby ułatwić zarządzanie i monitorowanie usług w chmurze Azure i maszyn wirtualnych.  W tym artykule omówiono te podstawowe funkcje zabezpieczeń i usługi. "
   services="security"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="StevenPo"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="terrylan"/>

# <a name="azure-security-management-and-monitoring-overview"></a>Zarządzanie Azure zabezpieczeń i monitorowania — omówienie

Azure udostępnia mechanizmy zabezpieczeń, aby ułatwić zarządzanie i monitorowanie usług w chmurze Azure i maszyn wirtualnych. W tym artykule omówiono te podstawowe funkcje zabezpieczeń i usługi. Znajdują się łącza do artykułów zawierających informacje na temat każdej Aby dowiedzieć się więcej.

Zabezpieczenia usługi Microsoft cloud jest powiązania i wspólnej odpowiedzialności między użytkownikiem a firmą Microsoft. Wspólnej odpowiedzialności oznacza, że firma Microsoft jest odpowiedzialny za Microsoft Azure i fizycznego zabezpieczenia wraz z danymi Wyśrodkowanie (przy użyciu zabezpieczeń zabezpieczenia, takich jak drzwi wpis znaczek zablokowane, ogrodzenia i zabezpieczeń). Ponadto Azure zapewnia silne poziomy zabezpieczeń cloud w warstwie oprogramowania, która spełnia wymagania zabezpieczenia, prywatność i zgodność klientom wymagających.

Jesteś właścicielem danych i tożsamości, odpowiedzialność za ochrony ich, zabezpieczenia zasobów lokalnych i zabezpieczeń składników chmury, nad którymi zarządzasz. Firma Microsoft świadczy z kontroli bezpieczeństwa i funkcje ułatwiające ochronę danych oraz aplikacji. Usługi odpowiedzialność za bezpieczeństwo zależy od rodzaju usługi w chmurze.

Poniżej przedstawiono saldo odpowiedzialność za klient i firmy Microsoft.

![Wspólnej odpowiedzialności][1]

Aby uzyskać szczegółowego dive do zarządzania zabezpieczeń zobacz [Zarządzanie zabezpieczeń platformy Azure](azure-security-management.md).

Poniżej przedstawiono podstawowe funkcje powinny być zawarte w tym artykule:

- Kontrola dostępu oparta na rolach
- Ochrony przed złośliwym oprogramowaniem
- Uwierzytelnianie wieloskładnikowe
- ExpressRoute
- Wirtualna sieć bram
- Zarządzanie tożsamościami uprzywilejowanych
- Ochrona tożsamości
- Centrum zabezpieczeń

## <a name="role-based-access-control"></a>Kontrola dostępu oparta na rolach

Oparta na rolach kontrola dostępu (RBAC) umożliwia zarządzanie szerokiego dostępu dla zasobów Azure. Przy użyciu RBAC, możesz przyznać osób tylko ilość dostępu, które są potrzebne do wykonywania zadań.  RBAC może również pomóc upewnij się, że po osób pozostawia organizacji utracą dostęp do zasobów w chmurze.

Dowiedz się więcej:

- [Active Directory blogu zespołu na RBAC](http://i1.blogs.technet.com/b/ad/archive/2015/10/12/azure-rbac-is-ga.aspx)
- [Kontrola dostępu oparta na rolach Azure](../active-directory/role-based-access-control-configure.md)

## <a name="antimalware"></a>Ochrony przed złośliwym oprogramowaniem

Za pomocą oprogramowania ochrony przed złośliwym oprogramowaniem dostawców głównych zabezpieczeń, takich jak Microsoft, Symantec Trend Micro, McAfee i Kaspersky chronić maszyn wirtualnych z programami do wyświetlania reklam, złośliwy plików i innymi zagrożeniami, z Azure.

Microsoft Antimalware oferuje możliwość instalowania agenta ochrony przed złośliwym oprogramowaniem zarówno PaaS ról i maszyn wirtualnych. W zależności od systemu ochrona punktu końcowego Centrum, ta funkcja łączy sprawdzone lokalnego technologię zabezpieczeń w chmurze.

Oferujemy także ścisła integracja trendu [Głębokości zabezpieczeń](http://www.trendmicro.com/us/enterprise/cloud-solutions/deep-security/)™ i [SecureCloud](http://www.trendmicro.com/us/enterprise/cloud-solutions/secure-cloud/)™ produktów platformy Azure. DeepSecurity jest rozwiązanie antywirusowe i SecureCloud to rozwiązanie szyfrowania. DeepSecurity zostanie wdrożony wewnątrz maszyny wirtualne przy użyciu modelu rozszerzenia. Korzystając z portalu interfejsu użytkownika i programu PowerShell, można użyć DeepSecurity w obrębie nowego maszyny wirtualne, które są jest surowa w górę lub istniejącej maszyny wirtualne, które są już zainstalowane.

Ochrona punktu końcowego Symantec (wrz) obsługiwane jest też Azure. Za pośrednictwem portalu integracji klienci mają możliwość określenia, które zamierzają wrz w maszyn wirtualnych za pomocą. WRZ, można zainstalować na nowym maszyn wirtualnych przez Azure Portal lub można zainstalować na istniejące maszyn wirtualnych przy użyciu programu PowerShell.

Dowiedz się więcej:

- [Wdrażanie rozwiązań ochrony przed złośliwym oprogramowaniem Azure maszyn wirtualnych](https://azure.microsoft.com/blog/deploying-antimalware-solutions-on-azure-virtual-machines/)
- [Microsoft ochrony przed złośliwym oprogramowaniem, usługami w chmurze Azure i maszyn wirtualnych](../security/azure-security-antimalware.md)
- [Jak zainstalować i skonfigurować Trend Micro głębokości Security jako usługa na maszyn wirtualnych systemu Windows](../virtual-machines/virtual-machines-windows-classic-install-trend.md)
- [Jak zainstalować i skonfigurować ochrona punktu końcowego Symantec na maszyn wirtualnych systemu Windows](../virtual-machines/virtual-machines-windows-classic-install-symantec.md)
- [Nowe opcje ochrony przed złośliwym oprogramowaniem do ochrony Azure maszyn wirtualnych — ochrona punktu końcowego McAfee](https://azure.microsoft.com/blog/new-antimalware-options-for-protecting-azure-virtual-machines/)

## <a name="multi-factor-authentication"></a>Uwierzytelnianie wieloskładnikowe

Uwierzytelnianie wieloskładnikowe Azure (MFA) jest to metoda uwierzytelniania, który wymaga korzystania z więcej niż jedną metodę weryfikacji i dodanie krytyczne drugą warstwę zabezpieczeń dodatki logowania użytkownika i transakcje. MFA ułatwia ochronę informacji dostęp do danych i aplikacji podczas spotkania żądanie użytkownika prosty proces logowania. Zapewnia silne uwierzytelnianie za pomocą różnych opcji Weryfikacja — rozmowy telefonicznej, SMS lub aplikacji dla urządzeń przenośnych powiadomienia lub weryfikacji kod i 3rd firmy PRZYSIĘGĄ tokeny.

Dowiedz się więcej:

- [Uwierzytelnianie wieloskładnikowe](https://azure.microsoft.com/documentation/services/multi-factor-authentication/)
- [Co to jest Azure uwierzytelnianie wieloskładnikowe?](../multi-factor-authentication/multi-factor-authentication.md)
- [Jak działa uwierzytelnianie wieloskładnikowe Azure](../multi-factor-authentication/multi-factor-authentication-how-it-works.md)

## <a name="expressroute"></a>ExpressRoute

Microsoft Azure ExpressRoute pozwala na rozszerzenie sieci lokalnej w chmurze firmy Microsoft przez połączenie prywatne dedykowane uproszczony przez dostawcę łączności. Przy użyciu ExpressRoute można nawiązać połączenia z usługami w chmurze firmy Microsoft, takich jak Microsoft Azure, Office 365 i CRM Online. Łączność może być dowolna z każdym sieci (IP VPN), sieci Ethernet typu punkt-punkt lub wirtualnego połączenia między przez dostawcę łączności w funkcji współtworzenia lokalizacji. ExpressRoute połączenia nie przechodzą publicznie w Internecie. Dzięki temu ExpressRoute połączenia do oferowania więcej niezawodności, szybsze szybkości opóźnienia dolnym i lepsze zabezpieczenia niż typowy połączenia przez Internet.

Dowiedz się więcej:

- [Omówienie kwestii technicznych ExpressRoute](../expressroute/expressroute-introduction.md)

## <a name="virtual-network-gateways"></a>Wirtualna sieć bram

Bramy sieci VPN, nazywany Azure wirtualna sieć bram są używane do wysyłania ruchu sieciowego między wirtualnych sieci i lokalizacji lokalnego. Służą również wysyłać ruch między wieloma wirtualnej sieci w obrębie Azure (VNet-VNet).  Bramy sieci VPN zapewniają bezpieczny lokalnej infrastrukturze łączność między Azure i infrastruktury.

Dowiedz się więcej:

- [Informacje o bram sieci VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md)
- [Omówienie zabezpieczeń sieci Azure](security-network-overview.md)

## <a name="privileged-identity-management"></a>Zarządzanie tożsamościami uprzywilejowanych

Czasami użytkownicy muszą do wykonywania operacji uprzywilejowanych Azure zasobów lub w innych aplikacjach władz akredytacji bezpieczeństwa. Często oznacza to, że organizacji ma nadać im stały dostęp uprzywilejowanych usługi Azure Active Directory (Azure AD). Jest to zagrożenie bezpieczeństwa wzrostu dla zasobów hostowana w chmurze, ponieważ organizacje wystarczająco nie można monitorować, co robią tych użytkowników z ich uprzywilejowanych dostęp.
Ponadto jeśli konto użytkownika z dostępem uprzywilejowanych jest bezpieczne, tego jednego naruszenia mogą mieć wpływ ogólny zabezpieczeń chmury. Azure AD uprawnieniach Zarządzanie tożsamościami pozwala rozwiązać ten czynnik ryzyka przez zmniejszenie czasu Uwidacznianie uprawnień i Zwiększanie widoczności do użycia.  

Zarządzanie tożsamościami uprzywilejowanych pojęcia tymczasowe administrator roli lub "tylko w czasie" access administratora, który jest użytkownik, który należy ukończyć proces aktywacji w celu przypisania roli. Proces aktywacji zmienia przydziału użytkownika do roli w Azure AD z nieaktywny na aktywny przez określony czas okresu, takich jak 8 godzin.

Dowiedz się więcej:

- [Zarządzanie tożsamościami uprawnieniach Azure AD](../active-directory/active-directory-privileged-identity-management-configure.md)
- [Rozpoczynanie pracy z Azure AD uprzywilejowanych Zarządzanie tożsamościami](../active-directory/active-directory-privileged-identity-management-getting-started.md)

## <a name="identity-protection"></a>Ochrona tożsamości

Ochronę tożsamości usługi Azure Active Directory (AD) zawiera skonsolidowany widok podejrzanych działań logowania i słabych chronić Twojej firmy. Ochrona tożsamości wykrywa podejrzanych działań dla użytkowników i tożsamości uprawnieniach (Administrator), oparte na sygnały, takich jak atakami siłowymi przecieku poświadczenia i rejestrowaniu z nieznanych lokalizacji i zainfekowany urządzeń.

Udostępniając powiadomienia i zalecane naprawy, ochronę tożsamości pomaga w zmniejszeniu ryzyka w czasie rzeczywistym. Istotność ryzyka użytkownika jest obliczana, a można skonfigurować zasady automatycznie ułatwiające ochronę informacji aplikacji dostęp z przyszłych zagrożeń ryzyka.

Dowiedz się więcej:

- [Ochrona tożsamości Azure Active Directory](../active-directory/active-directory-identityprotection.md)
- [Kanał 9: Azure AD i Pokaż tożsamości: Podgląd ochrony tożsamości](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

## <a name="security-center"></a>Centrum zabezpieczeń

Centrum zabezpieczeń Azure ułatwia zapobieganie, wykrywanie i odpowiadanie na zagrożenia i zapewnia zwiększone wgląd i kontrolować, zabezpieczenia Azure zasobów. Zapewnia zabezpieczenia zintegrowane monitorowania i zasad zarządzania w subskrypcjach usługi Azure, pomaga wykrywać zagrożenia, które w przeciwnym razie przejdź niezauważalnie i współpraca z Szeroki ekosystem rozwiązania zabezpieczeń.

Centrum zabezpieczeń pomaga zoptymalizować i monitorowanie zabezpieczeń Azure zasobów według:

- Pozwala na zdefiniowanie zasad dla zasobów Azure subskrypcji zgodnie z potrzebami zabezpieczeń firmy i typ aplikacji lub poufność danych w każdej subskrypcji.
- Monitorowanie stanu Azure maszyn wirtualnych, sieci i aplikacji.
- Zapewnia listy alertów zabezpieczeń priorytetów, w tym alerty z rozwiązań partnerów zintegrowane, oraz informacje potrzebne do szybkiego badania i zalecenia dotyczące korygowania atakiem.

Dowiedz się więcej:

- [Wprowadzenie do Centrum zabezpieczeń Azure](../security-center/security-center-intro.md)

<!--Image references-->
[1]: ./media/security-management-and-monitoring-overview/shared-responsibility.png
