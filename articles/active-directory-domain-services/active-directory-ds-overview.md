<properties
    pageTitle="Omówienie Azure Active Directory Domain Services | Microsoft Azure"
    description="Omówienie Azure Active Directory Domain Services"
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
    ms.date="10/07/2016"
    ms.author="maheshu"/>

# <a name="azure-ad-domain-services"></a>Usługi domenowe Azure AD

## <a name="overview"></a>Omówienie
Usługi infrastruktury Azure umożliwiają wdrażanie szeroką gamę obliczeniowych rozwiązań w sposób adaptacyjne. Z maszyn wirtualnych Azure można wdrażać niemal natychmiast i płacisz tylko przez minutę. Przy użyciu pomocy technicznej dla systemu Windows, Linux, SQL Server, Oracle, IBM, SAP i BizTalk, należy wdrożyć wszelkie obciążenie pracą, dowolny język na niemal dowolnym system operacyjny. Następujące korzyści umożliwiają Migrowanie starszych aplikacji wdrożony w lokalnym Azure, aby zapisać na wydatki operacyjne.

Kluczowy element migrowanie aplikacji lokalnej Azure obsługuje potrzeb tożsamości te aplikacje. Aplikacje z obsługą katalogu może Polegaj na LDAP w celu odczytu lub zapisu do katalogu firmy lub za pomocą zintegrowanego uwierzytelniania systemu Windows (uwierzytelnianie Kerberos lub NTLM) do uwierzytelniania użytkowników końcowych. Linia biznesowych (LOB) działa w systemie Windows Server zwykle wdrożeniu aplikacji na komputerach domeny sprzężone, mogą być zarządzane, przy użyciu zasad grupy. Zależności w infrastrukturze firmy tożsamości do aplikacji "dźwigu i zmiana" lokalnego w chmurze, należy rozwiązać.

Administratorzy często włączyć do jednego z następujących rozwiązań do spełnienia wymagań tożsamości aplikacji wdrożony w Azure:

- Wdrażanie witryny do witryny połączenie VPN między obciążenia działa usługi infrastruktury Azure i firmowej katalogu lokalnego.
- Rozszerzanie firmowej infrastruktury i domenami usługi Active Directory, konfigurując replice kontrolerów domeny przy użyciu Azure maszyn wirtualnych.
- Wdrażanie autonomiczny domeny w Azure za pomocą kontrolery rozmieszczony jako Azure maszyn wirtualnych.

Tych metod mogą być z wysoki koszt i administrację. Administratorzy są wymagane do wdrożenia kontrolerów domeny przy użyciu maszyn wirtualnych w Azure. Ponadto są potrzebne do zarządzania, bezpiecznego, poprawki, monitorowanie, aby utworzyć kopię zapasową i rozwiązywanie problemów z tych maszyn wirtualnych. Zależność od połączenia VPN z katalogu lokalnego powoduje obciążenia wdrożony w Azure dotyczyć zakłócenia sieci lub awarii. Te awarii sieci z kolei wpływa na dolnym przestojów i zmniejszonej niezawodności dla tych aplikacji.

Firma Microsoft zaprojektowane w usługach domenowych AD Azure jako alternatywa łatwiejsze.


## <a name="introducing-azure-ad-domain-services"></a>Wprowadzenie do usług domenowych AD Azure
Azure usług domenowych AD udostępnia domeny zarządzanych usług, takich jak dołączyć do domeny, uwierzytelnianie Kerberos/NTLM zasad, LDAP, grupy, które są w pełni zgodne z usługą Active Directory systemu Windows Server. Mogą używać tych usług domeny bez konieczności wdrażania, zarządzanie i poprawka kontrolery w chmurze. Azure usług domenowych AD można zintegrować z istniejących Azure AD dzierżawy usługi, umożliwiając użytkownikom Zaloguj się przy użyciu poświadczeń firmy. Ponadto można istniejących grup i kont użytkowników bezpieczny dostęp do zasobów, zapewnić w ten sposób jest bardziej płynna "dźwigu i shift" zasobów lokalnych usługi infrastruktury Azure.

Azure funkcji usług domenowych AD współpracuje niezależnie od tego, czy dzierżawy usługi Azure AD jest tylko do chmury lub zsynchronizowany z usługą Active Directory w lokalnej.

### <a name="azure-ad-domain-services-for-cloud-only-organizations"></a>Azure usługach domenowych AD dla organizacji tylko do chmury
Tylko do chmury Azure AD dzierżawy (często nazywane "zarządzanych dzierżaw") nie ma dowolnego miejsca tożsamości lokalnego. Innymi słowy konta użytkowników, hasła i członkostwa w grupach są wszystkie natywnych w chmurze — oznacza to, że tworzenia i zarządzania nimi Azure AD. Należy wziąć pod uwagę chwilę że Contoso jest tylko do chmury Azure AD dzierżawy. Jak pokazano na poniższej ilustracji, administrator firmy Contoso skonfigurował wirtualnej sieci w usługach infrastruktury Azure. Aplikacje i obciążeń serwerów wdrożonych w tym wirtualną sieć w środowisku maszyn wirtualnych Azure. Contoso jest tylko do chmury dzierżawy, wszystkich tożsamości użytkownika, swoje poświadczenia i członkostwa w grupach są tworzone i zarządzane w Azure AD.

![Omówienie usług Azure AD domeny](./media/active-directory-domain-services-overview/aadds-overview.png)

Firmy Contoso informatykiem można włączyć usługach domenowych AD Azure ich dzierżawcy Azure AD i wybierz pozycję, aby udostępnić usług domenowych w tym wirtualnej sieci. Usług domenowych w usłudze Azure AD przepisy zarządzanych domeny i udostępnia w wirtualnej sieci. Wszystkie konta użytkowników, członkostwa w grupach i dostępne w dzierżawie Azure AD firmy Contoso poświadczeń użytkownika są również dostępne w tej domenie nowo utworzony. Ta funkcja umożliwia użytkownikom w organizacji do zalogowania się do domeny przy użyciu poświadczeń firmy — na przykład przy nawiązywaniu połączenia zdalnego na komputerach domeny za pomocą pulpitu zdalnego. Administratorzy mogą obsługi administracyjnej dostęp do zasobów w domenie za pomocą istniejącego członkostwa w grupach. Aplikacje wdrożony w środowisku maszyn wirtualnych w wirtualnej sieci mogą używać funkcji, takich jak dołączyć do domeny, LDAP odczytu LDAP powiązania, uwierzytelnianie NTLM i Kerberos i zasad grupy.

Pod kilkoma względami istotne zarządzanych domeny, który jest zainicjowany przez usługi Azure AD domeny są następujące:

- Firmy Contoso informatykiem nie trzeba zarządzać, poprawka lub monitorowanie tej domeny lub dowolnego kontrolera domeny dla tej domeny zarządzane.
- Nie jest konieczne do zarządzania replikacją AD dla tej domeny. Konta użytkowników, członkostwa w grupach i poświadczenia z dzierżawy Azure AD firmy Contoso są automatycznie dostępne w tej domenie zarządzane.
- Ponieważ domena jest zarządzane przez Azure AD usług domenowych, Contoso w informatykiem nie ma uprawnień administratora domeny lub Administrator przedsiębiorstwa w tej domenie.


### <a name="azure-ad-domain-services-for-hybrid-organizations"></a>Azure usługach domenowych AD dla organizacji hybrydowego
Organizacje z hybrydowym infrastruktury informatycznej korzystać z zasobów w chmurze i zasobów lokalnych. Takie organizacje synchronizacja informacji z ich katalogu lokalnego do ich dzierżawy Azure AD. Jak organizacje hybrydowych Szukaj, aby przeprowadzić migrację więcej aplikacji w lokalnej w chmurze, zwłaszcza starsze katalogu aplikacje z obsługą, usług domenowych AD Azure może być przydatne do nich.

Przykładowa Corporation zainstalowała [Narzędzie Azure AD Connect](../active-directory/active-directory-aadconnect.md), aby zsynchronizować tożsamości informacje z ich katalogu lokalnego ich dzierżawy Azure AD. Informacje tożsamości, która jest synchronizowana zawiera konta użytkowników, ich mieszania poświadczeń uwierzytelniania (synchronizacji haseł) i członkostwa w grupach.

> [AZURE.NOTE] **Synchronizacja haseł jest obowiązkowe organizacjom hybrydowych za pomocą usług Azure AD domeny**. To wymaganie dotyczy, ponieważ poświadczenia użytkowników są wymagane w domenie zarządzanych udostępnianych przez usługi Azure AD domeny do uwierzytelnienia tych użytkowników za pomocą metody uwierzytelniania NTLM lub Kerberos.

![Azure AD domeny usługi dla przykładowej Corporation](./media/active-directory-domain-services-overview/aadds-overview-synced-tenant.png)

Na powyższej ilustracji pokazano, jak organizacje z hybrydowym infrastruktury informatycznej, takich jak przykładowa Corporation, mogą używać usług domenowych AD Azure. W przykładowej aplikacji i obciążenia serwera, które wymagają usług domenowych wdrożonych w wirtualnej sieci w usługach infrastruktury Azure. W przykładowej informatykiem można włączyć usługi Azure AD domeny w ich dzierżawy Azure AD i wybierz pozycję, aby udostępnić zarządzanych domeny w tym wirtualnej sieci. Ponieważ przykładowa jest organizacja o hybrydowych infrastruktury informatycznej, konta użytkowników, grupy i poświadczenia są synchronizowane ich dzierżawy Azure AD z ich katalogu lokalnego. Ta funkcja umożliwia użytkownikom zalogować się do domeny przy użyciu poświadczeń firmy — na przykład podczas nawiązywania połączenia zdalnego na komputerach dołączania do domeny za pomocą pulpitu zdalnego. Administratorzy mogą obsługi administracyjnej dostęp do zasobów w domenie za pomocą istniejącego członkostwa w grupach. Aplikacje wdrożony w środowisku maszyn wirtualnych w wirtualnej sieci mogą używać funkcji, takich jak dołączyć do domeny, LDAP odczytu LDAP powiązania, uwierzytelnianie NTLM i Kerberos i zasad grupy.

Pod kilkoma względami istotne zarządzanych domeny, który jest zainicjowany przez usługi Azure AD domeny są następujące:

- Zarządzane domena jest domeną autonomicznego. Nie ma rozszerzenie domeny lokalnej przykładowej firmy.
- W przykładowej informatykiem nie trzeba zarządzać, poprawki, lub monitora kontrolery domeny dla tej domeny zarządzane.
- Nie jest konieczne do zarządzania replikacją AD do tej domeny. Konta użytkowników, członkostwa w grupach i poświadczenia z katalogu lokalnego przykładowej firmy są synchronizowane Azure AD przy użyciu Azure AD Connect. Te konta użytkowników, członkostwa w grupach i poświadczenia są automatycznie dostępne w domenie zarządzane.
- Ponieważ domena jest zarządzane przez Azure AD usług domenowych, przykładowej firmy informatykiem nie ma uprawnień administratora domeny lub Administrator przedsiębiorstwa w tej domenie.


## <a name="benefits"></a>Zalety
Z usługami Azure AD domeny można korzystać ze następujące korzyści:

-   **Prosty** — spełnia wymagań tożsamości maszyn wirtualnych wdrożony usługi infrastruktury Azure za pomocą kilku kliknięć prosty. Nie ma potrzeby wdrażanie i zarządzanie infrastrukturą tożsamości w usłudze łączności Azure lub konfiguracji Wstecz w lokalnej infrastrukturze tożsamości.

-   **Układy** — usług domenowych AD Azure jest mocno zintegrowany z dzierżawy usługi Azure AD. Teraz możesz użyć Azure AD zintegrowane opartej na chmurze katalogu przeznaczoną na potrzeby nowoczesne aplikacje i tradycyjne aplikacje z obsługą z katalogu.

-   **Zgodne** — usług domenowych AD Azure jest oparty na infrastruktury ocen sprawdzone przedsiębiorstwa usługi Active Directory systemu Windows Server. Dlatego aplikacji polega na lepszej zgodności z funkcjami usługi Active Directory systemu Windows Server. Nie wszystkie funkcje dostępne w systemie Windows Server AD są obecnie dostępne w usługach domenowych AD Azure. Jednak dostępne funkcje są zgodne z odpowiednich funkcji Windows Server AD, które zależne w infrastrukturze lokalnego. Możliwości sprzężenia LDAP, Kerberos, NTLM, zasad grupy i domeny stanowią dojrzałych oferty, które przetestowane i Uściślanie w różnych wersjach systemu Windows Server.

-   **Efektywne pod względem kosztów** — z usługami domeny w usłudze Azure AD, można uniknąć obciążenia infrastruktury i zarządzania skojarzonego z zarządzania infrastrukturą tożsamości do obsługi tradycyjnych aplikacji obsługującej katalogu. Można przenieść te aplikacje usług infrastruktury Azure i korzystać z niższych wydatki operacyjne.
