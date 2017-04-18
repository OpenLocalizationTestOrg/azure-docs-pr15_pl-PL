<properties
    pageTitle="Azure Active Directory Domain Services: Funkcje | Microsoft Azure"
    description="Funkcje Azure Active Directory Domain Services"
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

## <a name="features"></a>Funkcje
Następujące funkcje są dostępne w usługach domenowych AD Azure zarządzania domenami.

- **Proste wdrożenie obsługi:** Możesz włączyć usługi Azure AD domen dla dzierżawy usługi Azure AD za pomocą zaledwie kilku kliknięć. Niezależnie od tego, czy dzierżawy usługi Azure AD jest chmury dzierżawy lub synchronizowane z katalogu lokalnego zarządzanych domeny mogą być szybko przygotowana.

- **Obsługa Dołączanie domeny:** Możesz łatwo komputerów Dołączanie domeny w Azure wirtualnej sieci, które zarządzanych domeny jest dostępna w. Środowisko Dołączanie domeny na Windows klienta i serwera systemów operacyjnych działa bezproblemowo w stosunku do domen obsługiwane w usługach domenowych AD Azure. Można też użyć sprzężenia automatyczną domeny narzędzie w stosunku do tych domen.

- **Wystąpienia jedną domenę na katalogu Azure AD:** Możesz utworzyć pojedynczy domeną usługi Active Directory dla każdego katalogu Azure AD.

- **Tworzenie domen przy użyciu niestandardowej nazwy:** Możesz utworzyć domen z nazwami niestandardowej (na przykład "contoso100.com") przy użyciu usług domenowych AD Azure. Możesz użyć dowolnej zatwierdzonych lub niezweryfikowany nazw domen. Opcjonalnie możesz również utworzyć domeny sufiksu domeny wbudowane (to znaczy "*. onmicrosoft.com") oferowanych przez katalogu Azure AD.

- **Zintegrowany z usługą Azure Active Directory:** Nie trzeba konfigurować lub zarządzać replikacji usług domenowych AD Azure. Konta użytkowników, członkostwa w grupach i poświadczeń użytkownika (hasła) z katalogu Azure AD są automatycznie dostępne w usługach domenowych AD Azure. Nowi użytkownicy, grupy lub zmiany atrybutów z katalogu lokalnego lub dzierżawcy usługi Azure AD są synchronizowane automatycznie do usług domenowych AD Azure.

- **Uwierzytelnianie NTLM i Kerberos:** Obsługa uwierzytelniania NTLM i Kerberos należy wdrożyć aplikacje korzystające z zintegrowanego uwierzytelniania systemu Windows.

- **Za pomocą firmowego poświadczeń i haseł:** Hasła dla użytkowników w dzierżawie usługi Azure AD Praca z usług domenowych AD Azure. Użytkownicy mogą swoje poświadczenia firmowe na komputerach Dołączanie domeny za pomocą, zaloguj się interakcyjnie lub za pośrednictwem pulpitu zdalnego, a następnie uwierzytelnić zarządzanych domeny.

- **Powiązanie LDAP i LDAP więcej pomocy technicznej:** Możesz użyć aplikacji, które korzystają z wiązania LDAP do uwierzytelniania użytkowników w domenach obsługiwanych przez usługi Azure AD domeny. Ponadto aplikacje, które używają operacji odczytu LDAP atrybutów użytkownika/komputera kwerendy z katalogu również współpracy przed usług domenowych AD Azure.

- **Bezpieczny protokół LDAP (LDAPS):** Możesz włączyć dostęp do katalogu nad bezpiecznego protokołu LDAP (LDAPS). Bezpieczny dostęp LDAP jest dostępna w ramach wirtualnej sieci domyślnie. Można jednak także opcjonalnie włączyć bezpiecznego dostępu LDAP przez internet.

- **Zasady grupy:** Za pomocą pojedynczego wbudowanych GPO dla użytkowników i komputerów kontenerów, aby wymusić zgodność z wymagane zasad zabezpieczeń dla konta użytkowników i domeny komputerach.

- **Zarządzaj systemem DNS:** Członkowie grupy "Administratorzy kontrolera domeny AAD" można zarządzać DNS dla swojej domeny zarządzane za pomocą znanych narzędzi administracyjnych DNS, takich jak przystawki MMC administracji DNS.

- **Tworzenie niestandardowej jednostki organizacyjne (OU):** Członkowie grupy "Administratorzy AAD kontrolera domeny" mogą tworzyć niestandardowe organizacyjnych w domenie zarządzanych. Ci użytkownicy udzielono pełne uprawnienia administracyjne przez niestandardowe organizacyjnych, więc ich dodać lub usunąć konta usług, komputery, grupy itd., w tych niestandardowych organizacyjnych.

- **Dostępne w wielu regionach Azure:** Odwiedź stronę [usług Azure według regionów](https://azure.microsoft.com/regions/#services/) wiedzieć Azure regiony, w których Azure AD Domain Services jest dostępny.

- **Wysokiej dostępności:** Azure usług domenowych AD oferuje wysoką dostępność dla swojej domeny. Ta funkcja daje gwarancji wyższą czas działania usługi i odporność na awarie. Wbudowane kondycji monitorowanie ofert automatycznego korygowania z błędy przez Obracająca się nowych wystąpień, aby zamienić wystąpienia nie powiodło się i do świadczenia usługi ciągłej dla swojej domeny.

- **Za pomocą narzędzia do zarządzania znanych:** Za pomocą znanych narzędzi zarządzania usługi Active Directory systemu Windows Server, takich jak Centrum administracyjnego usługi Active Directory lub Active Directory programu PowerShell do zarządzania domenami zarządzane.
