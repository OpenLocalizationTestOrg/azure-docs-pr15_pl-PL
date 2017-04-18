<properties
    pageTitle="Definiowanie Azure Active Directory hybrydowych tożsamości zagadnienia projektowe - strategię wdrażania tożsamości hybrydowych | Microsoft Azure"
    description="Za pomocą pola Kontrola dostępu warunkowego usługi Azure Active Directory sprawdza określone warunki wybierz podczas uwierzytelniania użytkownika i przed zezwoleniem na dostęp do aplikacji. Po tych warunki są spełnione, użytkownik uwierzytelniony i dostęp do aplikacji."
    documentationCenter=""
    services="active-directory"
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
    ms.date="08/08/2016"
    ms.author="billmath"/>


# <a name="define-a-hybrid-identity-adoption-strategy"></a>Definiowanie strategii wdrażania tożsamości hybrydowego

W tym zadaniu będzie zdefiniować strategii hybrydowych tożsamości wdrażania rozwiązania hybrydowego tożsamości do wymagań firm, które zostały omówione w:

- [Określanie potrzeb biznesowych](active-directory-hybrid-identity-design-considerations-business-needs.md)
- [Określenie wymagań dotyczących synchronizacji katalogów](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)
- [Określanie wymagania dotyczące uwierzytelniania wieloskładnikowego](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)

## <a name="define-business-needs-strategy"></a>Zdefiniowanie strategii potrzeb biznesowych
Pierwsze adresy zadania Określanie firm organizacji musi.  Może to być bardzo szeroki i grzbiet zakres może wystąpić, jeśli nie jesteś zachować ostrożność.  Na początku Zachowaj prostotę, ale należy pamiętać o do planowania projektu, który zezwalały i ułatwienia zmian w przyszłości.  Niezależnie od tego, czy jest projekt prosty lub jeden bardzo złożone usługi Azure Active Directory jest platformy Microsoft Identity, która obsługuje usługi Office 365, usługi Online firmy Microsoft i chmurę używającymi programów.

## <a name="define-an-integration-strategy"></a>Zdefiniowanie strategii integracji
Firma Microsoft udostępnia trzy scenariusze integracji głównym, które są tożsamości w chmurze, zsynchronizowane tożsamości i tożsamości federacyjnych.  Należy zaplanować na jeden z tych strategii integracji przyjęcia.  Strategia, wybrane może być różna i decyzji, wybierając jedną może obejmować, jakiego rodzaju chcesz podać, czy masz niektóre z istniejącej infrastruktury już w miejscu, i co to jest najbardziej kosztów efektywne środowisko użytkownika.  
 
![](./media/hybrid-id-design-considerations/integration-scenarios.png)

Dostępne są następujące scenariusze zdefiniowane na powyższym rysunku:

- **Tożsamości chmury**: są tożsamości, które istnieją wyłącznie w chmurze.  W przypadku Azure AD czy są one przechowywane w katalogu Azure AD.
- **Synchronized**: są tożsamości, znajdują się w lokalnej i w chmurze.  Przy użyciu Azure AD Connect, tych użytkowników są tworzone lub połączone z istniejącymi kontami Azure AD.  Skrótu hasła użytkownika są synchronizowane z lokalnym środowiskiem chmury w co nosi nazwę skrótu hasła.  Synchronizowane za pomocą jedno zastrzeżenie: to, że jeśli użytkownik jest wyłączona w środowisku lokalnym, może potrwać do 3 godziny dla tego stanu konta się pojawić Azure AD.  Jest to spowodowane przedział czasu synchronizacji.
- **Federacyjne**: tych tożsamości istnieje zarówno w lokalnej i w chmurze.  Przy użyciu Azure AD Connect, tych użytkowników są tworzone lub połączone z istniejącymi kontami Azure AD.  
 
>[AZURE.NOTE]
Aby uzyskać więcej informacji na temat synchronizacji opcje przeczytaj [Integrowanie tożsamości lokalnych z usługą Azure Active Directory](active-directory-aadconnect.md).

Poniższa tabela może pomóc w ustalaniu zalet i wad każdego z następujących strategii:

| Strategii         | Zalety                                                                                                                                                                                                                                                  | Wady                                                                                                                                                                                                                                                                                                                                                  |
|------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Chmura tożsamości** | Łatwiejsze zarządzanie dla małych organizacji. <br> Nic, aby zainstalować dodatkowego sprzętu na lokalnej — bez potrzeby<br>Jeśli użytkownik opuści firmy łatwo wyłączone                                                                                                   | Użytkownicy będą musieli logowania podczas uzyskiwania dostępu do obciążeń pracą w chmurze <br> Haseł może lub nie może być taka sama dla tożsamości w chmurze i lokalnych                                                                                                                                                                                                                     |
| **Synchronizowane**     | Hasło lokalnego zostaną służą do uwierzytelniania zarówno w lokalnej i w chmurze katalogów <br>Ułatwia zarządzanie dla małych, średnich i dużych organizacjach <br>Użytkownicy mogą mieć logowania jednokrotnego (SSO) dla niektórych zasobów <br> Metoda Microsoft preferowane synchronizacji <br> Łatwiejsze do zarządzania | Niektórzy klienci mogą niechętnie synchronizację katalogów ich z chmurą ukończenia policji określonej firmy                                                                                                                                                                                                                                                  |
| **Federacji**        | Użytkownicy mogą mieć logowania jednokrotnego (SSO) <br>Jeśli użytkownik zostanie zakończone lub pozostawia, konto można od razu wyłączone i dostęp odwołany,<br> Obsługa zaawansowanych scenariuszy, które można wykonywać przy użyciu synchronizowane                                           | Dodatkowe kroki w celu instalacji i konfiguracji <br> Wyższa konserwacji <br> Może wymagać dodatkowego sprzętu infrastruktury STS <br> Może wymagać dodatkowego sprzętu, aby zainstalować na serwerze federacyjnym. Dodatkowe oprogramowanie jest wymagane użycie usług AD FS <br> Wymaga instalacji rozległa logowania jednokrotnego <br> Punkt krytyczny błąd, jeśli serwer federacyjny jest wyłączony, użytkownicy nie będą mogli przeprowadzać uwierzytelniania |

### <a name="client-experience"></a>Środowisko klienta
Strategia, którego używasz, określają, obsługi logowania użytkowników.  Poniższe tabele zawierają informacje o co użytkownicy się spodziewać ich logowanie się.  Należy zauważyć, że nie wszystkie dostawców tożsamości federacyjnych obsługuje logowania jednokrotnego we wszystkich scenariuszach.

**Aplikacje sieciowe sprzężone domena i prywatne**:
 

|                              | Zsynchronizowane tożsamości      | Tożsamości federacyjnych                                           |
|------------------------------|----------------------------|--------------------------------------------------------------|
| Przeglądarki sieci Web                 | Uwierzytelnianie oparte na formularzach | rejestracji jednokrotnej, czasami wymagane do dostarczania identyfikator organizacji |
| Program Outlook                      | Monituj o poświadczenia     | Monituj o poświadczenia                                       |
| W programie Skype dla firm (Lync)    | Monituj o poświadczenia     | logowania jednokrotnego na programu Lync, zostanie wyświetlony monit poświadczeń programu Exchange   |
| Usługa SkyDrive Pro                 | Monituj o poświadczenia     | logowaniu jednokrotnym                                               |
| Pakiet Office Pro Plus subskrypcji | Monituj o poświadczenia     | logowaniu jednokrotnym                                               |

**Zewnętrznych lub niezaufanego źródła**:

|                              | Zsynchronizowane tożsamości      | Tożsamości federacyjnych                                           |
|------------------------------|----------------------------|--------------------------------------------------------------|
| Przeglądarki sieci Web                 | Uwierzytelnianie oparte na formularzach |  Uwierzytelnianie oparte na formularzach |
| Program Outlook, program Skype dla firm (Lync), usługi Skydrive Pro subskrypcji pakietu Office| Monituj o poświadczenia     | Monituj o poświadczenia                                       |
| Program Exchange ActiveSync    | Monituj o poświadczenia     | logowania jednokrotnego na programu Lync, zostanie wyświetlony monit poświadczeń programu Exchange   |
| Aplikacje dla urządzeń przenośnych                 | Monituj o poświadczenia     | Monituj o poświadczenia                                               |

Jeśli uważasz z zadania 1, że masz 3rd strony protokołu IdP lub są rozpoczęcie pracy za jego pomocą zapewniają federacji z usługą Azure Active Directory, musisz należy pamiętać o następujących obsługiwane możliwości:
- Dowolnego dostawcy SAML 2.0, który jest zgodny z SP Lite profilu mogą obsługiwać uwierzytelniania Azure AD i skojarzone aplikacje
- Obsługuje uwierzytelnianie pasywne, co ułatwia auth do aplikacji OWA, usługi SPO itp.
- Klienci usługi Exchange Online mogą być obsługiwane przez SAML 2.0 rozszerzony klienta profilu (ECP)

Należy również należy pamiętać o jakie funkcje nie są dostępne:

- Nie obsługują zaufania-federacyjnych podziały wszystkich klientów aktywne
 - Oznacza to, że nie klient programu Lync, klienta usługi OneDrive, subskrypcji pakietu Office, pakiet Office Mobile przed pakietu Office 2016
- Przejścia pakietu Office na uwierzytelnianie pasywne, aby umożliwić ich do obsługi wyłącznie SAML 2.0 IdPs, ale nadal będzie pomocy technicznej na podstawie klienta przez klienta


>[AZURE.NOTE]
Najbardziej zaktualizowanych list można znaleźć w artykule http://aka.ms/ssoproviders.

## <a name="define-synchronization-strategy"></a>Zdefiniowanie strategii synchronizacji
W tym zadaniu określi narzędzia, które będą używane do synchronizacji organizacji lokalnej danych do chmury i topologii należy używać.  Ponieważ większość organizacji korzystają z usługi Active Directory, informacji dotyczących używania Azure AD Connect do odpowiedzi na pytania powyżej podano niektóre szczegóły.  W środowiskach, które nie mają usługi Active Directory istnieje informacji na temat używania kodu FIM 2010 R2 lub 2016 z wieloma Uczestnikami ułatwiające planowanie tej strategii.  Jednak przyszłych wersjach narzędzie Azure AD Connect będzie obsługiwać katalogów LDAP, dlatego w zależności od osi czasu, te informacje może mieć możliwość pomocy.

###<a name="synchronization-tools"></a>Narzędzia do synchronizacji
Biegiem lat kilka narzędzi synchronizacji mają istniała przeznaczone i różnych scenariuszach.  Narzędzie Azure AD Connect jest obecnie przejdź do narzędzia dla wszystkich obsługiwanych scenariuszach.  AAD Sync DirSync są nadal wokół i może być zawarte w środowisku teraz. 

>[AZURE.NOTE]
Aby uzyskać najnowsze informacje dotyczące obsługiwane możliwości poszczególnych narzędzi przeczytaj artykuł [Porównanie narzędzia integracji katalogów](active-directory-hybrid-identity-design-considerations-tools-comparison.md) .  

### <a name="supported-topologies"></a>Obsługiwanych topologii
Podczas definiowania strategii synchronizację, należy ustalić topologii, która jest używana. W zależności od informacji, który został określony w kroku 2 można określić, które topologii jest pisane z wielkiej litery korzystać. Jeden las pojedynczej topologia Azure AD jest najczęściej i składa się z jednego las usługi Active Directory i jedno wystąpienie Azure AD.  Ma być używane w większości scenariuszy i jest oczekiwane topologii, gdy za pomocą Azure AD łączenie Express instalacji, jak pokazano na poniższej ilustracji.
 
![](./media/hybrid-id-design-considerations/single-forest.png)Scenariusz jeden las jest często bardzo dużych i małych nawet organizacji jest wiele lasów, jak pokazano na rysunku 5.

>[AZURE.NOTE]
Aby uzyskać więcej informacji o różnych lokalnych i topologii Azure AD przy użyciu Azure AD Connect synchronizacją przeczytaj artykuł [topologii dla Azure AD Connect](active-directory-aadconnect-topologies.md).


![](./media/hybrid-id-design-considerations/multi-forest.png) 

Scenariusz z wieloma las

Jeśli to wielkość liter, a następnie topologii wielu-forest-pojedynczy Azure AD należy rozważyć, gdy są spełnione następujące elementy:

- Użytkownicy mają tylko 1 tożsamości w lasach wszystkich — jednoznacznie identyfikujące użytkowników poniższej sekcji opisano to bardziej szczegółowo.
- Użytkownik uwierzytelnia się las, w którym znajduje się jego tożsamości
- UPN i kotwicy źródła (identyfikator niezmienne) będą pobierane z ten las
- Wszystkie lasów są dostępne narzędzie Azure AD Connect — oznacza to, że nie musi być domeny sprzężone i mogą być umieszczane w strefy Zdemilitaryzowanej, jeśli to ułatwia to.
- Użytkownicy mają tylko jedna skrzynka pocztowa
- Las, który obsługuje skrzynki pocztowej użytkownika ma najlepszej jakości danych atrybutów widoczne na globalnej liście adresowej (GAL) programu Exchange
- Jeśli użytkownik jest nie skrzynki pocztowej, następnie wszelkie las może służyć do współtworzenia tych wartości
- Jeśli masz skrzynki pocztowej połączonego występuje również inne konto w różnych las służący do logowania się.

>[AZURE.NOTE]
Obiekty, które istnieją zarówno w wersji lokalnej i w chmurze są "połączone" za pośrednictwem Unikatowy identyfikator. W ramach synchronizacji katalogów ten unikatowy identyfikator nosi nazwę SourceAnchor. W kontekście z logowania jednokrotnego to nosi nazwę ImmutableId. [Projektowanie koncepcji Azure AD Connect](active-directory-aadconnect-design-concepts.md#sourceanchor) więcej zagadnienia dotyczące używania programu SourceAnchor.

Jeśli masz więcej niż jedno konto aktywne lub więcej niż jedna skrzynka pocztowa powyższego nie są spełnione, narzędzie Azure AD Connect wybierz jedną z nich i ignorowanie drugi.  Jeśli zostały połączone skrzynek pocztowych, ale żadne inne konto, tych kont nie zostaną wyeksportowane Azure AD i użytkownik nie będzie członkiem żadnej grupy.  Jest inne niż jak była w przeszłości z użyciem narzędzia DirSync i jest zamierzone, aby lepiej Obsługa scenariuszom wielu las. Scenariusz wielu las przedstawiono na poniższej ilustracji.
 
![](./media/hybrid-id-design-considerations/multiforest-multipleAzureAD.png) 
 
**Wiele las wielu scenariusz Azure AD**

Zaleca się mieć tylko jeden katalog w Azure AD dla organizacji, ale jest obsługiwana relacji 1:1 jest ona przechowywana między serwer Azure AD Connect synchronizacji i katalog Azure AD.  Dla każdego wystąpienia Azure AD konieczne będzie instalacji narzędzie Azure AD Connect.  Ponadto Azure AD zamierzone się odizolowane i użytkownicy w jedno wystąpienie Azure AD nie będą mogli widzieć użytkownicy w innym wystąpieniu.

Jest to możliwe i obsługiwane nawiązać jedno wystąpienie lokalnej usługi Active Directory do wielu katalogów Azure AD, jak pokazano na poniższej ilustracji:

![](./media/hybrid-id-design-considerations/single-forest-flitering.png) 
 

**Scenariusz filtrowania jeden las**

W tym celu następujących muszą być spełnione:

- Serwery synchronizacji usługi Azure AD Connect należy skonfigurować dla potrzeb filtrowania, mają zestaw wzajemnie wykluczających się obiektów.  To gotowe, na przykład według zakresu każdego serwera do określonej domeny lub jednostkę Organizacyjną.
- Domeny DNS może być rejestrowane tylko w jednym katalogu Azure AD Rozważmy UPN użytkowników w wersji lokalnej AD należy użyć osobnych obszarów nazw
- Użytkownicy w jedno wystąpienie Azure AD tylko będą mogli widzieć użytkowników z ich wystąpienia.  Nie będą mogli widzieć użytkownicy w innych przypadkach
- Tylko jeden z katalogów Azure AD umożliwiają wdrożenia hybrydowe programu Exchange z lokalnego AD
- Wzajemnego wyłączności dotyczy również zapisu Wstecz.  Dzięki temu niektóre funkcje wstecz zapisu nie są obsługiwane w tej topologii, ponieważ są one założono konfiguracji pojedynczego lokalnego.  Ta opcja uwzględnia:
 - Grupowanie wstecz zapisu z domyślnej konfiguracji
 - Urządzenie zapisu — Wstecz


Należy pamiętać, że następujące czynności nie jest obsługiwane i nie powinny zostać wybrane jako implementacja:

- Wiele serwerów Synchronizuj narzędzie Azure AD Connect łączenia do tego samego katalogu Azure AD, nawet jeśli są one skonfigurowane do synchronizacji zestaw wzajemnie wykluczających się obiekt nie jest obsługiwana
- Go nie jest obsługiwana zsynchronizować tego samego użytkownika do wielu katalogów Azure AD. 
- Nie jest również obsługiwana Aby wprowadzić zmianę, aby użytkownicy w jednym Azure AD się pojawić jako kontaktów w innym katalogu Azure AD konfiguracji. 
- Jest również nieobsługiwany modyfikowanie Azure AD Connect synchronizacją nawiązywania połączenia z wielu katalogów Azure AD.
- Azure AD katalogów są zamierzone samodzielnie. Jest nieobsługiwane w celu zmiany konfiguracji synchronizacji Azure AD Connect do odczytywania danych z innego katalogu Azure AD w celu tworzenia typowe i ujednolicony GAL między katalogów. Nie jest również obsługiwana do wyeksportowania użytkowników jako kontaktów do innego lokalnego AD przy użyciu Azure AD Connect synchronizacją.


>[AZURE.NOTE]
Jeśli Twoja organizacja ogranicza komputery w sieci z łączenia się z Internetem, w tym artykule wymieniono punkty końcowe (nazwy FQDN, IPv4 i zakresy adresów IP protokołu IPv6) że powinien zawierać w swojej ruchu wychodzącego umożliwia list i Internet Explorer Zaufane witryny klienta komputery zapewnienie komputerów pomyślnie mogą używać usługi Office 365. Aby uzyskać więcej informacji, przeczytaj [adresy URL usługi Office 365 i zakresy adresów IP](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2?ui=en-US&rs=en-US&ad=US).

## <a name="define-multi-factor-authentication-strategy"></a>Zdefiniowanie strategii uwierzytelnianie wieloskładnikowe
W tym zadaniu określi strategii uwierzytelnianie wieloskładnikowe korzystać.  Uwierzytelnianie wieloskładnikowe Azure składa się z dwóch różnych wersji.  Jeden jest oparte na chmurze, a druga lokalnego na podstawie przy użyciu serwera MFA Azure.  Na podstawie oceny powyżej, które można określić rozwiązania, które jest poprawne dla strategii.  Skorzystaj z poniższej tabeli do określenia opcji projektu najlepsze spełnienia wymagań dotyczących zabezpieczeń firmy:

Opcje wieloskładnikowe projektu:

| Trwały zapewnienie                                               | MFA w chmurze | Lokalna MFA |
|---------------------------------------------------------------|------------------|----------------|
| Aplikacje Microsoft                                                | tak              | tak            |
| Aplikacje władz akredytacji bezpieczeństwa w galerii aplikacji                                  | tak              | tak            |
| Są publikowane w usłudze Azure AD aplikacji Proxy aplikacji usług IIS         | tak              | tak            |
| Nie są publikowane w serwer Azure AD aplikacji Proxy aplikacji usług IIS | Brak               | tak            |
| Dostępu zdalnego sieci VPN, RDG                                     | Brak               | tak            |

Mimo że możesz może mieć rozliczenia na rozwiązanie dla strategii, nadal chcesz używać oceny z góry na miejsce, w którym znajdują się użytkowników.  Może to powodować rozwiązanie zmienić.  Skorzystaj z poniższej tabeli, aby pomóc Określanie to:

| Lokalizacja użytkownika                                                       | Opcja preferowanej projektu                 |
|---------------------------------------------------------------------|-----------------------------------------|
| Azure Active Directory                                              | Wielokrotne-FactorAuthentication w chmurze |
| Azure AD i lokalnych AD Federacji za pomocą usług AD FS             | Oba                                    |
| Azure AD i lokalnych AD przy użyciu Azure AD Connect nie synchronizacji haseł | Oba                                    |
| Azure AD i lokalnych Azure AD Connect za pomocą synchronizacji haseł  | Oba                                    |
| Lokalne AD                                                      | Uwierzytelnianie wieloskładnikowe serwera      |

>[AZURE.NOTE]
Należy upewnić się, że wybranej opcji Projekt uwierzytelnianie wieloskładnikowe obsługuje funkcje, które są wymagane do projektu.  Aby uzyskać więcej informacji przeczytaj [Wybierz rozwiązanie wieloskładnikowe zabezpieczeń dla Ciebie](../multi-factor-authentication-get-started.md#what-am-i-trying-to-secure).

## <a name="multi-factor-auth-provider"></a>Dostawca uwierzytelniania wieloskładnikowego
Uwierzytelnianie wieloskładnikowe jest domyślnie dostępne dla administratorów globalnego, którzy mają dzierżawy usługi Azure Active Directory. Jednak jeśli chcesz obejmowały uwierzytelnianie wieloskładnikowe wszystkich użytkowników i/lub umożliwia administratorom globalnej podjęcie zalet funkcji, takich jak portalu zarządzania, niestandardowe powitania i raportów, następnie trzeba kupić i skonfigurować dostawcę uwierzytelniania wieloskładnikowego.

>[AZURE.NOTE]
Należy upewnić się, że wybranej opcji Projekt uwierzytelnianie wieloskładnikowe obsługuje funkcje, które są wymagane do projektu. 

##<a name="next-steps"></a>Następne kroki
[Określenie wymagań dotyczących ochrony danych](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md)

## <a name="see-also"></a>Zobacz też
[Omówienie zagadnienia projektu](active-directory-hybrid-identity-design-considerations-overview.md)
