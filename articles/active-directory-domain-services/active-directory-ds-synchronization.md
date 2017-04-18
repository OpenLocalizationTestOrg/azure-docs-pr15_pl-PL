<properties
    pageTitle="Azure Active Directory Domain Services: Synchronizacja w domenach zarządzanych | Microsoft Azure"
    description="Opis synchronizacji w domenie zarządzanych Azure Active Directory Domain Services"
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
    ms.date="10/03/2016"
    ms.author="maheshu"/>

# <a name="synchronization-in-an-azure-ad-domain-services-managed-domain"></a>Synchronizacja w domenie usług domenowych AD Azure zarządzanych
Na poniższym diagramie przedstawiono, jak działa synchronizacja w w usługach domenowych AD Azure zarządzanych domen.

![Topologia synchronizacji w usługach domenowych AD Azure](./media/active-directory-domain-services-design-guide/sync-topology.png)

## <a name="synchronization-from-your-on-premises-directory-to-your-azure-ad-tenant"></a>Synchronizacja z katalogu lokalnego dzierżawy usługi Azure AD
Azure AD Connect synchronizacją umożliwia synchronizowanie konta użytkownika, grupy członkostwa i poświadczeń mieszania do Twojej dzierżawy Azure AD. Atrybuty użytkownika konta, takie jak nazwa UPN i lokalnych SID (identyfikatora zabezpieczeń) są synchronizowane. Jeśli korzystasz z usługi Azure AD domeny, mieszania starsze poświadczeń wymaganych do uwierzytelniania Kerberos i NTLM są synchronizowane z dzierżawcy usługi Azure AD.

Jeśli skonfigurujesz wstecz zapisu, są synchronizowane usługi Active Directory w lokalnej zmiany występujące w katalogu Azure AD. Na przykład, jeśli zmienisz hasło, za pomocą funkcji Zmień samodzielne hasła Azure AD zmiany hasła jest aktualizowana w swojej lokalnej hasło.

> [AZURE.NOTE] Zawsze używaj najnowszej wersji Azure AD Connect, aby upewnić się, że masz rozwiązania dla wszystkich znanych usterek.


## <a name="synchronization-from-your-azure-ad-tenant-to-your-managed-domain"></a>Synchronizacja z dzierżawy usługi Azure AD zarządzanych domeny
Konta użytkowników, członkostwa w grupach i mieszania poświadczenia są synchronizowane z dzierżawy usługi Azure AD zarządzanych domenie usług domenowych AD Azure. Ten proces synchronizacji jest automatyczne. Nie musisz skonfigurować, monitorować lub zarządzanie tego procesu synchronizacji. Proces synchronizacji jest również jeden-way/jednokierunkowa charakter. Zarządzane domeny jest głównie tylko do odczytu z wyjątkiem wszelkie niestandardowe organizacyjnych, możesz utworzyć. Dlatego nie można zmienić atrybuty użytkowników, hasła użytkowników i członkostwa w grupach w domenie zarządzane. W wyniku jest synchronizacja nie odwrotnej zmian z Twojej domeny zarządzanych powrót do dzierżawy usługi Azure AD.


## <a name="synchronization-from-a-multi-forest-on-premises-environment"></a>Synchronizacja z wielu las lokalnego środowiska
Wiele organizacji ma infrastrukturze tożsamości lokalnych dość złożone składający się z wieloma lasami konta. Narzędzie Azure AD Connect obsługuje synchronizowanych użytkowników, grupy i mieszania poświadczenia z las wielu środowiskach do Twojej dzierżawy Azure AD.

Natomiast dzierżawy usługi Azure AD jest znacznie upraszcza i płaskiej przestrzeń nazw. Aby umożliwić użytkownikom w wiarygodny sposób dostępu do aplikacji zabezpieczone Azure AD, rozwiązywanie konfliktów UPN między kontami użytkowników w różnych lasach. Usługi niedźwiadki domeny zarządzanych usług domenowych AD Azure Zamknij podobieństwa do dzierżawy usługi Azure AD. W związku z tym Zobacz prostym struktury OU w zarządzanych domeny. Wszyscy użytkownicy i grupy są przechowywane w kontenerze "AADDC użytkownicy", niezależnie od domeny lokalnej i las, z którego zostały zsynchronizowane w. Skonfigurowano OU hierarchiczne struktury lokalnego. Jednak zarządzanych domeny nadal ma prostą strukturę OU prostym.


## <a name="exclusions---what-isnt-synchronized-to-your-managed-domain"></a>Wyłączenia — co nie jest zsynchronizowany z domeną zarządzanych
Następujące obiekty i atrybuty nie są synchronizowane do dzierżawy usługi Azure AD lub zarządzanych domeny:

- **Atrybuty wyłączone:** Można wykluczyć określone atrybuty z synchronizacją do Twojej dzierżawy Azure AD z domeny lokalnej za pomocą narzędzie Azure AD Connect. Następujące atrybuty wyłączone nie są dostępne w domenie zarządzane.

- **Zasad grupy:** Zasady grupy skonfigurowane w domenie lokalnej nie są synchronizowane z domeną zarządzane.

- **Udział SYSVOL:** Podobnie zawartość udziału SYSVOL w domenie lokalnej nie są synchronizowane z domeną zarządzane.

- **Obiektów komputera:** Obiekty komputera komputerów dołączonych do domeny lokalnej nie są synchronizowane z domeną zarządzane. Tych komputerów nie istnieje relacja zaufania z domeny zarządzanych i nie należy do domeny lokalnej tylko. W domenie zarządzane możesz znaleźć tylko w przypadku komputerów, które wyraźnie domeny — dołączono do domeny zarządzanych obiektów komputera.

- **Atrybuty SidHistory dla użytkowników i grup:** Podstawowy użytkowników i grup podstawowego identyfikatorów zabezpieczeń z domeny lokalnej są synchronizowane z domeną zarządzane. Jednak istniejące atrybuty SidHistory dla użytkowników i grup nie są synchronizowane z domeny w lokalnej domenie zarządzanych.

- **w strukturze organizacji jednostki (OU):** Jednostkami organizacyjnymi zdefiniowane w lokalnej domenie nie są synchronizowane z domeną zarządzane. Istnieją dwa organizacyjnych wbudowanych w zarządzanych domeny. Domyślnie zarządzanych domeny ma płaską strukturę Organizacyjna. Można jednak [tworzyć niestandardowe OU w domenie zarządzanych](./active-directory-ds-admin-guide-create-ou.md).


## <a name="how-specific-attributes-are-synchronized-to-your-managed-domain"></a>W jaki sposób określone atrybuty są synchronizowane z domeną zarządzanych
W poniższej tabeli wymieniono niektóre typowe atrybuty i w tym artykule opisano, jak są synchronizowane z domeną zarządzane.

| Atrybut w domenie zarządzanych | Źródła | Notatki |
|:---|:---|:---|
|GŁÓWNEJ NAZWY UŻYTKOWNIKA|Atrybut UPN użytkownika w dzierżawie usługi Azure AD|Atrybut UPN z dzierżawy usługi Azure AD jest synchronizowane jako zarządzaną domeny. Dlatego najbezpieczniejszym sposobem Zaloguj się do domeny zarządzanych korzysta z usługi głównej nazwy użytkownika.|
|Atrybuty SAMAccountName|MailNickname użytkownika atrybutów w dzierżawie usługi Azure AD lub generowane automatycznie|Atrybut SAMAccountName jest pochodzących z atrybutu mailNickname w dzierżawie usługi Azure AD. Jeśli wiele kont użytkowników o takim samym mailNickname atrybucie, atrybuty SAMAccountName jest generowane automatycznie. Jeśli mailNickname lub prefiks UPN użytkownika jest dłuższa niż 20 znaków, atrybuty SAMAccountName jest generowane automatycznie spełnienia wymagań limit znaków: 20 atrybuty SAMAccountName.|
|Hasła|Hasło użytkownika z dzierżawy usługi Azure AD|Mieszania poświadczenia wymagane uwierzytelniania NTLM lub Kerberos (zwanych również dodatkowe poświadczeń) są synchronizowane z dzierżawy usługi Azure AD. Jeśli dzierżawy usługi Azure AD jest synchronizowana dzierżawy, te poświadczenia pochodzą z domeny lokalnej.|
|Podstawowy użytkownika lub grupy SID|Generowane automatycznie|Podstawowy identyfikator zabezpieczeń dla kont użytkownika lub grupy jest generowane automatycznie zarządzanych domeny. Ten atrybut nie ma odpowiednika identyfikatora podstawowego użytkownika/zabezpieczeń grupy obiektu w swojej lokalnej hasło. Niezgodność jest, ponieważ domena zarządzanych ma różnych nazw SID niż domeny lokalnej.|
|Historia SID dla użytkowników i grup|Użytkownik główny lokalnego i zabezpieczeń grupy|Atrybut SidHistory dla użytkowników i grup w domenie zarządzanych jest ustawiony zgodnie z odpowiednich użytkownika podstawowego lub zabezpieczeń w domenie lokalnej grupy. Ta funkcja pomaga ułatwić dźwigu i shift lokalnego aplikacjom zarządzanych domeny, ponieważ nie trzeba ODP ACL zasobów.|

> [AZURE.NOTE] **Zaloguj się do domeny zarządzane za pomocą formatu UPN:** Atrybut SAMAccountName może być generowane automatycznie dla niektórych kont użytkowników w domenie zarządzane. Jeśli wielu użytkowników ma tę samą atrybutu mailNickname lub użytkownicy mają zbyt długiej prefiksy głównej nazwy użytkownika, atrybuty SAMAccountName dla tych użytkowników może być generowane automatycznie. W związku z tym format SAMAccountName (na przykład "CONTOSO100\joeuser") nie zawsze jest najlepszym sposobem Zaloguj się do domeny. SAMAccountName generowane automatycznie użytkowników mogą różnić się od swoich prefiksów głównej nazwy użytkownika. Użyj formatu głównej nazwy użytkownika (na przykład 'joeuser@contoso100.com') do zalogowania się do domeny zarządzanych prawidłowo.


## <a name="objects-that-are-not-synchronized-to-your-azure-ad-tenant-from-your-managed-domain"></a>Obiekty, które nie są synchronizowane z dzierżawy usługi Azure AD z Twojej domeny zarządzanych
Zgodnie z opisem w poprzedniej sekcji tego artykułu, jest nie synchronizacji z Twojej domeny zarządzanych powrót do dzierżawy usługi Azure AD. Można utworzyć [Niestandardowe jednostkę organizacyjną (OU)](./active-directory-ds-admin-guide-create-ou.md) w zarządzanych domeny. Ponadto można utworzyć inne organizacyjnych, użytkowników, grup lub kont usług w tych niestandardowych organizacyjnych. Brak obiektów utworzonych w niestandardowych organizacyjnych są synchronizowane z powrotem dzierżawy usługi Azure AD. Obiekty te są dostępne tylko w swojej domenie zarządzane. W związku z tym tych obiektów nie są widoczne, przy użyciu Azure AD poleceń cmdlet, API Azure AD wykresu lub zarządzanie Azure AD interfejsu użytkownika.


## <a name="related-content"></a>Zawartość pokrewna
- [Funkcje — usługi Azure AD domeny](active-directory-ds-features.md)

- [Wdrożeń - usług domenowych AD Azure](active-directory-ds-scenarios.md)

- [Sieć zagadnienia związane z usług domenowych AD Azure](active-directory-ds-networking.md)

- [Wprowadzenie do usług domenowych AD Azure](active-directory-ds-getting-started.md)
