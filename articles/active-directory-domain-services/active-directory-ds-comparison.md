<properties
    pageTitle="Azure usługach domenowych AD: Porównanie Azure AD domeny usługi samodzielnego kontrolery | Microsoft Azure"
    description="Porównanie Azure Active Directory Domain Services do samodzielnego kontrolery"
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
    ms.date="10/01/2016"
    ms.author="maheshu"/>

# <a name="how-to-decide-if-azure-ad-domain-services-is-right-for-your-use-case"></a>Jak zdecydować, jeśli usługi Azure AD domeny jest odpowiedni dla przypadków użycia
Azure usług domenowych AD umożliwia wdrażanie usługi Obciążenie pracą w usługach infrastruktury Azure, bez konieczności martwić się o zachowaniu infrastruktury tożsamości. Ta usługa zarządzanych różni się od typowego wdrożenia usługi Active Directory systemu Windows Server, wdrażanie i administrowanie własne. Usługa jest przeznaczony dla ułatwienia wdrożenia, monitorowanie kondycji automatyczną i rozwiązywanie problemu oraz proste tożsamości infrastruktury chmury. Firma Microsoft są stale rozwijających się usługę, aby dodać obsługę scenariuszy.

Określanie, czy za pomocą usług domenowych AD Azure lub aż i zarządzanie własną infrastruktury usługi Active Directory (Zrób) platformy Azure:

- Zobacz listę urzędów certyfikacji [Funkcje dostępne w usługach domenowych AD Azure](active-directory-ds-features.md).

- Sprawdź typowe [scenariusze wdrażania usługi Azure AD domeny](active-directory-ds-scenarios.md).

- Na koniec [Porównanie usług domenowych AD Azure Zrób opcji AD](active-directory-ds-comparison.md#compare-azure-ad-domain-services-to-diy-ad-domain-in-azure).


## <a name="compare-azure-ad-domain-services-to-diy-ad-domain-in-azure"></a>Porównanie usług domenowych AD Azure AD samodzielnego domenie Azure
Poniższa tabela ułatwia ustalenie między za pomocą usług Azure AD domen i zarządzanie własną infrastruktury usługi Active Directory platformy Azure.

|**Funkcja**|**Usługi domenowe Azure AD**|**"Zrób" AD w Azure maszyny wirtualne**|
|---|:---:|:---:|
|[**Usługa zarządzanych**](active-directory-ds-comparison.md#managed-service)|**& #x 2713;**|**& #x 2715;**|
|[**Bezpieczne instalacje**](active-directory-ds-comparison.md#secure-deployments)|**& #x 2713;**|Administrator musi bezpiecznego wdrożenia.|
|[**Serwer DNS**](active-directory-ds-comparison.md#dns-server)|**& #x 2713;** (usługa zarządzanych)|**& #x 2713;**|
|[**Uprawnienia administratora domeny lub przedsiębiorstwa**](active-directory-ds-comparison.md#domain-or-enterprise-administrator-privileges)|**& #x 2715;**|**& #x 2713;**|
|[**Dołączanie do domeny**](active-directory-ds-comparison.md#domain-join)|**& #x 2713;**|**& #x 2713;**|
|[**Uwierzytelnianie domeny przy użyciu NTLM i Kerberos**](active-directory-ds-comparison.md#domain-authentication-using-ntlm-and-kerberos)|**& #x 2713;**|**& #x 2713;**|
|[**Niestandardowe struktura Organizacyjna**](active-directory-ds-comparison.md#custom-ou-structure)|**& #x 2713;**|**& #x 2713;**|
|[**Rozszerzenia schematu**](active-directory-ds-comparison.md#schema-extensions)|**& #x 2715;**|**& #x 2713;**|
|[**Relacje zaufania AD domenami**](active-directory-ds-comparison.md#ad-domain-or-forest-trusts)|**& #x 2715;**|**& #x 2713;**|
|[**Przeczytaj LDAP**](active-directory-ds-comparison.md#ldap-read)|**& #x 2713;**|**& #x 2713;**|
|[**Bezpieczny protokół LDAP (LDAPS)**](active-directory-ds-comparison.md#secure-ldap)|**& #x 2713;**|**& #x 2713;**|
|[**Wpisywanie LDAP**](active-directory-ds-comparison.md#ldap-write)|**& #x 2715;**|**& #x 2713;**|
|[**Zasady grupy**](active-directory-ds-comparison.md#group-policy)|Prosta|Pełne|
|[**Geo distributed wdrożenia**](active-directory-ds-comparison.md#geo-dispersed-deployments)|**& #x 2715;**|**& #x 2713;**|


#### <a name="managed-service"></a>Usługa zarządzanych
Azure usług domenowych AD domeny są zarządzane przez firmę Microsoft. Nie musisz martwić, poprawki, aktualizacje, monitorowania, kopii zapasowych i zapewniania dostępności domeny. Te zadania związane z zarządzaniem dostępnych jako usługa w ramach Microsoft Azure dla swoich domen zarządzane.

#### <a name="secure-deployments"></a>Bezpieczne instalacje
Bezpieczne domeny zarządzanych został zablokowany zgodnie z firmy Microsoft najważniejsze wskazówki dotyczące zabezpieczeń w przypadku wdrożeń AD. Poniższe najważniejsze wskazówki wynikiem dekad zespołu produktu AD możliwości rysunek techniczny i pomocniczych wdrożeń AD. W przypadku wdrożeń Zrób, które należy wykonać kroki wdrażania, aby zablokować, w dół i Zabezpieczanie wdrożenia.

#### <a name="dns-server"></a>Serwer DNS
Azure AD domenie usług domenowych zarządzanych zawiera zarządzane usługi DNS. Członkowie grupy "Administratorzy AAD kontrolera domeny" mogą zarządzać DNS w domenie zarządzane. Członkowie tej grupy podano pełnymi uprawnieniami administrowania systemem DNS dla domeny zarządzane. Zarządzanie systemem DNS mogą być wykonywane za pomocą "konsoli administracyjnej DNS" zawarte w pakiecie narzędzia administracji zdalnej serwera (RSAT).

#### <a name="domain-or-enterprise-administrator-privileges"></a>Uprawnienia do domeny lub Administrator przedsiębiorstwa
Te pełnymi uprawnieniami nie są dostępne w domenie zarządzanych AAD DS. Nie można uruchomić aplikacje, które wymagają tych podwyższonym poziomem uprawnień zostać zainstalowana Uruchom przed zarządzanych domen. Mniejszy podzbiór uprawnienia administracyjne są dostępne dla członków grupy administracji delegowanej o nazwie "Administratorzy AAD kontrolera domeny". Tych uprawnień obejmuje uprawnienia do konfigurowania systemu DNS, konfigurowanie zasad grupy, uzyskać uprawnień administratora na komputerach domeny itp.

#### <a name="domain-join"></a>Dołączanie do domeny
Maszyn wirtualnych można dołączyć do domeny zarządzane, podobnie jak dołączyć komputery do domeny AD.

#### <a name="domain-authentication-using-ntlm-and-kerberos"></a>Uwierzytelnianie domeny przy użyciu NTLM i Kerberos
Z usługami Azure AD domeny poświadczenia firmowe służy do uwierzytelniania zarządzanych domeny. Poświadczenia są synchronizowane z dzierżawy usługi Azure AD. Zsynchronizowany dzierżaw Azure AD Connect gwarantuje, że zmiany wprowadzone poświadczeń lokalnych są synchronizowane z Azure AD. W przypadku konfiguracji domeny samodzielnego może być konieczne konfigurowanie relacji zaufania domeny z lokalnego konta las użytkownikom uwierzytelnianie przy użyciu poświadczeń firmy. Ewentualnie konieczne może być Konfigurowanie replikacji AD, aby upewnić się, że hasło użytkownika zsynchronizować maszyn wirtualnych kontrolera domeny Azure.

#### <a name="custom-ou-structure"></a>Niestandardowe struktura Organizacyjna
Członkowie grupy "Administratorzy AAD kontrolera domeny" mogą tworzyć niestandardowe organizacyjnych w domenie zarządzanych.

#### <a name="schema-extensions"></a>Rozszerzenia schematu
Nie można rozszerzyć schematu podstawowego domeny zarządzanych usług domenowych AD Azure. Dlatego aplikacje korzystające z rozszerzeń schematu AD (na przykład nowych atrybutów w obiekcie użytkownika) nie zniesiony i przesunięte do usługi Katalogowej AAD domen.

#### <a name="ad-domain-or-forest-trusts"></a>Domeny AD lub zaufania las
Nie można skonfigurować zarządzanych domen do skonfigurowania relacji zaufania (ruch przychodzący i wychodzący) z innymi domenami. Dlatego scenariuszy, takich jak wdrożeń las zasobów lub przypadków, której nie chcesz się synchronizować hasła do Azure AD nie można używać usług domenowych AD Azure.

#### <a name="ldap-read"></a>Odczyt LDAP
Zarządzane domeny obsługuje więcej obciążenia LDAP. W związku z tym można wdrażać aplikacji, które wykonują LDAP operacji odczytu w domenie zarządzane.

#### <a name="secure-ldap"></a>Bezpieczny protokół LDAP
Można skonfigurować usług domenowych AD Azure bezpieczny dostęp LDAP z domeną zarządzane, łącznie z Internetem.

#### <a name="ldap-write"></a>Wpisywanie LDAP
Zarządzane domena jest tylko do odczytu dla obiektów użytkowników. W związku z tym aplikacji, które wykonują operacji zapisu LDAP atrybutów obiektu użytkownika nie działają w domenie zarządzanych. Ponadto hasła użytkowników nie można zmienić z w domenie zarządzane. Inny przykład będzie zmiana członkostwa w grupach lub atrybuty grupy w domenie zarządzane, który nie jest dozwolony. Jednak wszelkie zmiany atrybutów użytkownika i hasła w Azure AD (za pośrednictwem portalu programu PowerShell i Azure) lub AD są synchronizowane z domeną zarządzanych Zasadami AAD lokalnego.

#### <a name="group-policy"></a>Zasady grupy
Grupa zaawansowanych zasad konstrukcji nie są obsługiwane w domenie zarządzanych Zasadami AAD. Nie można na przykład, Utwórz i wdrażanie osobne obiekty zasad grupy dla każdej niestandardowej jednostce Organizacyjnej w domenie albo użyj WMI filtrowanie kierowanie zasad grupy. Ma wbudowane obiektu zasad grupy każdego dla kontenerów "Komputery AADDC" i "AADDC użytkowników", które można dostosować tak, aby skonfigurować zasady grupy.

#### <a name="geo-dispersed-deployments"></a>Rozproszone Geo wdrożenia
Azure domen zarządzanych w usługach domenowych AD są dostępne w jednej sieci wirtualnej platformy Azure. W przypadku scenariuszy, które wymagają kontrolery ma być dostępny w wielu regionów Azure w dowolnym miejscu na świecie konfigurowania kontrolerów domeny w maszyny wirtualne IaaS Azure może być lepszym.


## <a name="do-it-yourself-diy-ad-deployment-options"></a>Opcje wdrażania (DIY) AD "Zrób"
Przypadków użycia wdrożenia może być konieczne niektóre funkcje oferowane przez instalacji systemu Windows Server AD. W tych przypadkach należy rozważyć jedną z następujących opcji Zrób (DIY):

- **Domeny chmury autonomicznego:** Możesz skonfigurować autonomicznego "chmury domena" przy użyciu Azure maszyn wirtualnych, które zostały skonfigurowane jako kontrolery domeny. Infrastruktura ta nie obsługują do lokalnego środowiska AD. Ta opcja wymaga innego zestawu "chmury poświadczeń" logowania i administrowania maszyny wirtualne w chmurze.

- **Wdrażania las zasobu:** Możesz skonfigurować domenę w topologii las zasobów, przy użyciu Azure maszyn wirtualnych skonfigurowanych jako kontrolery domen. Następnie można skonfigurować relacji zaufania AD do lokalnego środowiska AD. Możliwe komputerach Dołączanie domeny (maszyny wirtualne Azure) ten las zasobu w chmurze. Uwierzytelnianie użytkownika odbywa się nad jednym połączenie VPN-ExpressRoute z katalogu lokalnego.

- **Rozszerzenie domeny lokalnej Azure:** Można nawiązać Azure wirtualną sieć sieci lokalnej przy użyciu połączenia VPN-ExpressRoute tak, aby do swojej lokalnej można łączyć maszyny wirtualne Azure AD. Alternatywnego jest promowanie replice kontrolerów domeny lokalnego w Azure jako maszyny. Możesz następnie skonfigurować ją do replikacji przez połączenie VPN-ExpressRoute do katalogu lokalnego. W tym trybie wdrożenia skuteczne rozszerza domeny lokalnej Azure.

> [AZURE.NOTE] Należy określić, lepiej dopasowanego opcję samodzielnego dla przypadków użycia do wdrożenia. Należy rozważyć, czy [Udostępnianie opinii](active-directory-ds-contact-us.md) nam pomóc zrozumieć, co może pomóc w funkcji wybierzesz usług domenowych AD Azure w przyszłości. Ten opinie pomagają nam są obsługiwane w usłudze lepiej własnych potrzeb wdrożenia i przypadków użycia.

Firma Microsoft zostały opublikowane ułatwić samodzielnego instalacji [wskazówek dotyczących wdrażania systemu Windows Server usługi Active Directory na maszyn wirtualnych Azure](https://msdn.microsoft.com/library/azure/jj156090.aspx) .


## <a name="related-content"></a>Zawartość pokrewna
- [Funkcje — usługi Azure AD domeny](active-directory-ds-features.md)

- [Wdrożeń - usług domenowych AD Azure](active-directory-ds-scenarios.md)

- [Wskazówki dotyczące wdrażania systemu Windows Server usługi Active Directory w przypadku Azure maszyn wirtualnych](https://msdn.microsoft.com/library/azure/jj156090.aspx)
