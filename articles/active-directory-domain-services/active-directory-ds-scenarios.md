<properties
    pageTitle="Azure Active Directory Domain Services: Wdrożeń | Microsoft Azure"
    description="Scenariusze wdrażania usługi Azure AD domeny"
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
    ms.date="09/21/2016"
    ms.author="maheshu"/>


# <a name="deployment-scenarios-and-use-cases"></a>Wdrożeń i przypadków użycia
W tej sekcji możemy Przyjrzyj się kilka scenariuszy i przypadków użycia, które korzystać z usług domenowych Azure Active Directory (AD).

## <a name="secure-easy-administration-of-azure-virtual-machines"></a>Bezpieczne, łatwe zarządzanie Azure maszyn wirtualnych
Azure Active Directory Domain Services umożliwia zarządzanie Azure maszyn wirtualnych w sposób usprawniony. Azure maszyn wirtualnych może być dołączony do zarządzanych domeny, dzięki czemu Zaloguj się przy użyciu poświadczeń AD firmy. Ta metoda pozwala uniknąć kłopotów zarządzania poświadczeń, takich jak zachowanie lokalne konta administratora na każdym Azure maszyn wirtualnych.

Maszyn wirtualnych serwera, które są połączone z domeną zarządzane można również zarządzane i chronione za pomocą zasad grupy. Możesz zastosować plany bazowe zabezpieczeń jest wymagana do Azure maszyn wirtualnych i zablokować zgodnie z wytycznymi firmy zabezpieczeń. Za pomocą funkcji zarządzania zasadami grupy można na przykład ograniczyć typy aplikacje, które można uruchomić w przypadku tych maszyn wirtualnych.

![Administracja usprawniony maszyn wirtualnych Azure](./media/active-directory-domain-services-scenarios/streamlined-vm-administration.png)

Serwery i innych infrastruktury osiągnie wycofanych z, Contoso jest przenoszenie wiele aplikacji obecnie hostowana w środowisku lokalnym w chmurze. Bieżący standardowego IT wymaga, że serwery obsługujące aplikacji korporacyjnych musi być domeny i zarządzanie nimi, za pomocą zasad grupy. Firmy Contoso informatykiem chce maszyn wirtualnych sprzężenia domeny wdrożony w Azure, aby ułatwić administrowanie. W wyniku administratorów i użytkowników można Zaloguj się przy użyciu poświadczeń firmy. W tym samym czasie można skonfigurować maszyny do wykonania plany bazowe zabezpieczeń za pomocą zasad grupy. Contoso wolisz nie mają do wdrażania, monitorowania i zarządzania kontrolerami domeny w Azure zapewnienie Azure maszyn wirtualnych. W związku z tym usług domenowych AD Azure to doskonałe dopasowanie do przypadków użycia.

**Wdrożenie notatek**

Należy rozważyć następujące ważne fakty w tym scenariuszu wdrażania:

- Zarządzane domen dostępne w usługach domenowych AD Azure stanowią pojedynczy płaską strukturę OU (jednostka organizacyjna) domyślnie. Wszystkie komputery domeny znajdują się w jednym prostym jednostce Organizacyjnej. Można jednak tworzyć niestandardowe organizacyjnych.

- Azure usług domenowych AD obsługuje prostych zasad grupy w formularzu wbudowanego obiektu zasad grupy poszczególnych użytkowników i komputerów kontenerów. Nie można docelowe zasad grupy przez OU/dział, wykonać filtrowanie WMI lub Utwórz niestandardowe obiekty zasad grupy.

- Azure usług domenowych AD obsługuje podstawowego schematu obiektu AD komputera. Nie można rozszerzyć schematu obiektu komputera.


## <a name="lift-and-shift-an-on-premises-application-that-uses-ldap-bind-authentication-to-azure-infrastructure-services"></a>Dźwigu i shift aplikacji lokalnej korzystającej z uwierzytelniania powiązania LDAP do usługi infrastruktury Azure

![Wiązanie LDAP](./media/active-directory-domain-services-scenarios/ldap-bind.png)

Contoso ma aplikacji lokalnej, która została zakupiona od niezależny wiele lat temu. Aplikacja jest obecnie w trybie konserwacji przez niezależny dostawca oprogramowania i zażądanie zmiany w aplikacji jest zbyt duży dla firmy Contoso. Ta aplikacja ma frontend oparte na sieci web, zbiera poświadczeń użytkownika przy użyciu formularza sieci web, która następnie uwierzytelnia użytkowników, wykonując wiązanie LDAP z firmową usługi Active Directory. Contoso przeprowadzić migrację ta aplikacja usługi infrastruktury Azure. Jest pożądane, czy aplikacja działa tak jak, bez konieczności zmiany. Ponadto powinno być możliwe do uwierzytelnienia przy użyciu poświadczeń firmową istniejących użytkowników i bez konieczności przy przekwalifikowaniu użytkowników do wykonywania operacji inaczej. Innymi słowy użytkownicy końcowi powinny być oblivious, z którym jest uruchomiona aplikacja i migracji powinny być przezroczyste do nich.

**Wdrożenie notatek**

Należy rozważyć następujące ważne fakty w tym scenariuszu wdrażania:

- Upewnij się, że aplikacja nie jest konieczne modyfikowanie/zapisu katalogu. LDAP zapisu zarządzanych domen dostępne w usługach domenowych AD Azure nie jest obsługiwane.

- Nie można zmienić hasła bezpośrednio przed zarządzanych domeny. Użytkownicy mogą zmieniać swoje hasła albo przy użyciu mechanizmu Zmień samodzielne hasła Azure AD lub z katalogu lokalnego. Te zmiany są automatycznie synchronizowane i dostępne w domenie zarządzane.


## <a name="lift-and-shift-an-on-premises-application-that-uses-ldap-read-to-access-the-directory-to-azure-infrastructure-services"></a>Dźwigu i shift aplikacji lokalnej, która korzysta z LDAP Czytaj, aby uzyskać dostęp do katalogu usługi infrastruktury Azure
Contoso ma opracowanym niemal dekadę temu aplikacji z LOB (LOB) lokalnego. Ta aplikacja jest katalog pamiętać i została przeznaczona do pracy z systemem Windows Server AD. Aplikacja używa LDAP (Lightweight Directory Access Protocol), aby odczytać informacje i atrybuty informacji o użytkownikach z usługi Active Directory. Aplikacja nie Modyfikuj atrybuty lub inny sposób zapisywania w katalogu. Contoso chcesz przeprowadzić migrację ta aplikacja usługi infrastruktury Azure i wycofać sprzętu lokalnego wiekowania aktualnie obsługujący tej aplikacji. Aplikacja nie można nagrywać ponownie używać katalogu nowoczesny interfejsy API, takich jak interfejsu API Azure AD wykresu opartego na pozostałych. W związku z tym opcja dźwigu i shift jest konieczne, zgodnie z którą aplikacji, możesz przeprowadzić migrację do działania w chmurze, bez modyfikowania kodu lub ponowne zapisywanie adresów aplikacji.

**Wdrożenie notatek**

Należy rozważyć następujące ważne fakty w tym scenariuszu wdrażania:

- Upewnij się, że aplikacja nie jest konieczne modyfikowanie/zapisu katalogu. LDAP zapisu zarządzanych domen dostępne w usługach domenowych AD Azure nie jest obsługiwane.

- Upewnij się, że aplikacja nie jest konieczne rozszerzonym niestandardowy schemat usługi Active Directory. Rozszerzenia schematu nie są obsługiwane w usługach domenowych AD Azure.


## <a name="migrate-an-on-premises-service-or-daemon-application-to-azure-infrastructure-services"></a>Migrowanie aplikacji usługa lub demon lokalnej usługi infrastruktury Azure
Niektóre aplikacje się składać z wielu poziomów, gdzie w warstwach musi wykonać uwierzytelnionego połączenia do poziomu wewnętrznej bazy danych, takich jak warstwa bazy danych. Konta usługi Active Directory są często używane dla tych przypadków użycia. Można dźwigu i shift aplikacjom usługi infrastruktury Azure i używanie Azure usługach domenowych AD dla potrzeb tożsamości te aplikacje. Możesz używać tego samego konta usługi Azure AD zsynchronizowanych z katalogu lokalnego. Alternatywnie można najpierw utworzyć niestandardowej jednostce Organizacyjnej, a następnie utworzyć konto usługi osobne w tej jednostce Organizacyjnej rozmieszczanie takich aplikacji.

![Konto usługi przy użyciu programu WIA](./media/active-directory-domain-services-scenarios/wia-service-account.png)

Contoso ma niestandardowej magazynu aplikacji zawierającego frontonu sieci web, programu SQL server i serwera wewnętrznej bazy danych FTP. Zintegrowany z systemem Windows uwierzytelnianie kont usługi jest używane do uwierzytelnienia frontonu sieci web na serwerze FTP. Frontonu sieci web jest skonfigurowana do uruchamiania jako konto usługi. Serwera wewnętrznej bazy danych jest skonfigurowany do Autoryzuj dostęp z konta usługi dla frontonu sieci web. Contoso nie chce trzeba wdrażać maszyny wirtualnej kontrolera domeny w chmurze, aby przenieść tę aplikację do usługi infrastruktury Azure. Firmy Contoso IT administrator może wdrożyć serwery obsługujące frontonu sieci web, program SQL server i serwer FTP, aby Azure maszyn wirtualnych. Następnie sprzężone tych urządzeń w domenie usług domenowych AD Azure zarządzane. Następnie mogą używać tego samego konta usługi w ich katalogu lokalnym na potrzeby uwierzytelniania tej aplikacji. Tego konta usługi Azure AD domenie usług domenowych zarządzane są synchronizowane i jest dostępny do użytku.

**Wdrożenie notatek**

Należy rozważyć następujące ważne fakty w tym scenariuszu wdrażania:

- Upewnij się, że aplikacja używa nazwy użytkownika i hasła dla uwierzytelniania. Uwierzytelnianie certyfikatu i karty inteligentnej podstawie nie jest obsługiwane w usługach domenowych AD Azure.

- Nie można zmienić hasła bezpośrednio przed zarządzanych domeny. Użytkownicy mogą zmieniać swoje hasła albo przy użyciu mechanizmu Zmień samodzielne hasła Azure AD lub z katalogu lokalnego. Te zmiany są automatycznie synchronizowane i dostępne w domenie zarządzane.


## <a name="azure-remoteapp"></a>Funkcja RemoteApp Azure
Funkcja RemoteApp Azure umożliwia administratorowi firmy Contoso Tworzenie zbioru domeny. Ta funkcja umożliwia zdalnego aplikacji obsługiwanych przez Azure RemoteApp uruchamianie na komputerach domeny i uzyskać dostęp do innych zasobów przy użyciu funkcji uwierzytelniania zintegrowany z systemem Windows. Za pomocą usług Azure AD domeny firmy Contoso możesz podać zarządzanych domeny używaną przez zbiory domeny Azure RemoteApp.

![Funkcja RemoteApp Azure](./media/active-directory-domain-services-scenarios/azure-remoteapp.png)

Aby uzyskać więcej informacji na temat ten scenariusz wdrażania, zobacz artykuł blogu usługi pulpitu zdalnego [dźwigu i shift z obciążeń pracą z Azure RemoteApp i usług domenowych w usłudze Azure AD](http://blogs.msdn.com/b/rds/archive/2016/01/19/lift-and-shift-your-workloads-with-azure-remoteapp-and-azure-ad-domain-services.aspx).
