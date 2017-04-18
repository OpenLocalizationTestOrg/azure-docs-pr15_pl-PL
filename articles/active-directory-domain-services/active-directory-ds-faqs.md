<properties
    pageTitle="Często zadawane pytania — Azure Active Directory Domain Services | Microsoft Azure"
    description="Często zadawane pytania dotyczące usług domenowych Active Directory platformy Azure"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="maheshu"/>

# <a name="azure-active-directory-domain-services-frequently-asked-questions-faqs"></a>Azure Active Directory Domain Services: Często zadawane pytania

Ta strona zawiera odpowiedzi na często zadawane pytania dotyczące Azure Active Directory Domain Services. Sprawdzać dostępność aktualizacji.

### <a name="troubleshooting-guide"></a>Rozwiązywanie problemów z przewodnika
Można znaleźć w naszym [Przewodnik Rozwiązywanie problemów](active-directory-ds-troubleshooting.md) rozwiązań typowych problemów napotkanych podczas konfigurowania lub administrowaniem usługami Azure AD domeny.


### <a name="configuration"></a>Konfiguracja

#### <a name="can-i-create-multiple-domains-for-a-single-azure-ad-directory"></a>Czy można utworzyć wiele domen dla pojedynczego katalogu Azure AD?
Wartość nie. Możliwe jest tworzenie tylko jedna domena obsługiwane przez usługi Azure AD domeny dla pojedynczego katalogu Azure AD.  

#### <a name="can-i-enable-azure-ad-domain-services-in-an-azure-resource-manager-virtual-network"></a>Czy można włączyć usług domenowych AD Azure w wirtualnej sieci Menedżer zasobów Azure?
Wartość nie. Azure usług domenowych AD tylko może być włączony w klasycznym Azure wirtualnej sieci. Klasyczny wirtualną sieć można nawiązać Menedżera zasobów wirtualnej sieci przy użyciu wirtualnej sieci zaglądanie do używania domeny zarządzanych w wirtualnej sieci Menedżer zasobów.

#### <a name="can-i-make-azure-ad-domain-services-available-in-multiple-virtual-networks-within-my-subscription"></a>Czy mogę nawiązać usług domenowych AD Azure dostępne w wielu wirtualnych sieci w ramach subskrypcji?
Sama usługa nie obsługuje bezpośrednio w tym scenariuszu. Azure usług domenowych AD jest dostępna w tylko jeden wirtualną sieć naraz. Można jednak skonfigurować łączność między wiele wirtualnych sieci do udostępniania usług domenowych AD Azure do innych wirtualnych sieci. W tym artykule opisano, jak można [połączyć wirtualnych sieci platformy Azure](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).

#### <a name="can-i-enable-azure-ad-domain-services-using-powershell"></a>Czy można włączyć usługi Azure AD domeny przy użyciu programu PowerShell?
Programu PowerShell i automatyczne wdrażanie usług domenowych AD Azure nie jest obecnie dostępna.

#### <a name="is-azure-ad-domain-services-available-in-the-new-azure-portal"></a>Usług domenowych AD Azure jest dostępna w nowy portal Azure?
Wartość nie. Azure usług domenowych AD można skonfigurować tylko [portal Azure klasyczny](https://manage.windowsazure.com). Oczekujemy rozszerzyć obsługę [Azure portal](https://portal.azure.com) w przyszłości.

#### <a name="can-i-add-domain-controllers-to-an-azure-ad-domain-services-managed-domain"></a>Czy można dodać kontrolery domeny zarządzanych usług domenowych AD Azure?
Wartość nie. Domena, dostępne w usługach domenowych AD Azure jest domeną zarządzane. Nie musisz dodawać, konfigurowanie i inny sposób zarządzania kontrolerami domeny dla tej domeny — tych działań związanych z zarządzaniem służą jako usługi przez firmę Microsoft. W związku z tym nie możesz dodać dodatkowe kontrolery domeny (odczytu i zapisu lub tylko do odczytu) dla domeny zarządzane.

### <a name="administration-and-operations"></a>Administracja i operacji

#### <a name="can-i-connect-to-the-domain-controller-for-my-managed-domain-using-remote-desktop"></a>Czy mogę nawiązać kontrolera domeny dla danej domeny zarządzane za pomocą pulpitu zdalnego?
Wartość nie. Nie masz uprawnień do łączenia się kontrolerów domeny dla domeny zarządzane za pomocą pulpitu zdalnego. Członkowie grupy "Administratorzy AAD kontrolera domeny" mogą administrować zarządzanych domeny przy użyciu narzędzi administracyjnych AD, takie jak Active Directory administracji Centrum (ADAC) lub AD programu PowerShell. Te narzędzia są zainstalowane za pomocą funkcji "Narzędzia administracji zdalnej serwera" w systemie Windows server dołączony do zarządzanych domeny.

#### <a name="ive-enabled-azure-ad-domain-services-what-user-account-do-i-use-to-domain-join-machines-to-this-domain"></a>Włączeniu usług domenowych AD Azure. Jakie konto użytkownika należy używać na komputerach sprzężenia domeny do tej domeny?
Dodany do grupy administracyjnej (na przykład "AAD kontrolera domeny Administratorzy") użytkownikom komputerów Dołączanie domeny. Ponadto użytkownicy w tej grupie uzyskują dostęp zdalny do pulpitu na komputerach, które zostały dołączone do domeny.

#### <a name="can-i-wield-domain-administrator-privileges-for-the-domain-provided-by-azure-ad-domain-services"></a>Czy można trzymać uprawnienia administratora domeny dla domeny, dostępne w usługach domenowych AD Azure?
Wartość nie. Nie udzielono uprawnień administracyjnych w domenie zarządzane. Uprawnienia zarówno domeny administratora i Administrator przedsiębiorstwa nie są dostępne do użycia w tej domenie. Istniejące administrator domeny lub grupy administrator przedsiębiorstwa w katalogu Azure AD również nie udzielono uprawnień administratora domeny i przedsiębiorstwa w domenie.

#### <a name="can-i-modify-group-memberships-using-ldap-or-other-ad-administrative-tools-on-domains-provided-by-azure-ad-domain-services"></a>Czy można modyfikować członkostwa w grupach za pomocą LDAP lub inne narzędzia administracyjne AD w domenach dostępne w usługach domenowych AD Azure?
Wartość nie. Nie można modyfikować członkostwa w grupach w domenach obsługiwanych przez usługi Azure AD domeny. To samo dotyczy atrybuty użytkownika. Możesz jednak zmienić atrybuty użytkownika lub członkostwa w grupach w Azure AD lub w domenie lokalnej. Wprowadzone zmiany są synchronizowane automatycznie do usług domenowych AD Azure.

#### <a name="can-i-extend-the-schema-of-the-domain-provided-by-azure-ad-domain-services"></a>Czy można rozszerzyć schemat domeny, dostępne w usługach domenowych AD Azure?
Wartość nie. Schemat jest podawana przez firmę Microsoft dla zarządzanych domeny. Rozszerzenia schematu nie są obsługiwane w usługach domenowych AD Azure.

#### <a name="can-i-modify-or-add-dns-records-in-my-managed-domain"></a>Czy modyfikowanie lub dodawanie rekordów DNS w mojej zarządzanych domeny?
Wartość Tak. Użytkownicy należący do grupy "Administratorzy AAD kontrolera domeny" udzielono uprawnień administratora DNS, aby zmodyfikować rekordów DNS w domenie zarządzane. Ci użytkownicy umożliwia konsoli Menedżer DNS na komputerze z systemem Windows Server dołączony do domeny zarządzane, zarządzać systemem DNS. Aby użyć konsoli Menedżer DNS, zainstaluj "Narzędzia serwera DNS", który jest częścią opcjonalną funkcją "Narzędzia administracji zdalnej serwera" na serwerze. Więcej informacji na temat [Narzędzia do zarządzania, monitorowania i rozwiązywania problemów DNS](https://technet.microsoft.com/library/cc753579.aspx) jest dostępna w witrynie TechNet.


### <a name="billing-and-availability"></a>Dostępność i rozliczeń

#### <a name="is-azure-ad-domain-services-a-paid-service"></a>Czy usługi Azure AD domenowe usługą płatną?
Wartość Tak. Aby uzyskać więcej informacji zobacz [ceny strony](https://azure.microsoft.com/pricing/details/active-directory-ds/).

#### <a name="is-there-a-free-trial-for-the-service"></a>Czy istnieje bezpłatnej wersji próbnej usługi?
Ta usługa znajduje się w bezpłatnej wersji próbnej dla Azure. Możesz zalogować uzyskać [bezpłatną wersję próbną pakietu Azure jednego miesiąca](https://azure.microsoft.com/pricing/free-trial/).

#### <a name="can-i-get-azure-ad-domain-services-as-part-of-enterprise-mobility-suite-ems"></a>W ramach programu Enterprise Suite mobilności (EMS) można uzyskać usług domenowych AD Azure?
#### <a name="do-i-need-azure-ad-premium-to-use-azure-ad-domain-services"></a>Czy muszę Azure AD Premium, aby używać usług domenowych AD Azure?
Wartość nie. Azure usług domenowych AD jest repartycyjny usługi Azure i nie jest częścią EMS. Azure AD Domain Services są dostępne dla wszystkich wersji Azure AD (bezpłatne, podstawowe i, Premium) i są wystawiona co godzinę, w zależności od zastosowania.

#### <a name="what-azure-regions-is-the-service-available-in"></a>Jakie regiony Azure jest dostępna w usłudze?
Zapoznaj się z [Usługi Azure według regionów](https://azure.microsoft.com/regions/#services/) stronie, aby wyświetlić listę Azure regionów, w której znajduje się usług domenowych AD Azure.
