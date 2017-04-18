<properties
   pageTitle="Implementacji usługi Azure Active Directory | Microsoft Azure"
   description="Jak wdrażać bezpiecznego hybrydowych architektury sieci przy użyciu usługi Azure Active Directory."
   services="guidance,virtual-network,active-directory"
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/28/2016"
   ms.author="telmos"/>

# <a name="implementing-azure-active-directory"></a>Implementacji usługi Azure Active Directory

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

W tym artykule opisano najważniejsze wskazówki dotyczące integrowanie lokalnej usługi Active Directory (AD) domeny i lasy z usługi Azure Active Directory, aby zapewnić uwierzytelnianie oparte na chmurze tożsamości.

> [AZURE.NOTE] Azure ma dwa różne wdrożenia modele: [Menedżer zasobów] [ resource-manager-overview] i klasyczny. Ta architektura odwołanie używa Menedżera zasobów, które firma Microsoft zaleca się w przypadku wdrożeń nowy.

Możesz użyć katalogu i tożsamości usługi, takie jak oferowane przez usług AD Directory (AD DS) do uwierzytelnienia tożsamości. Tych tożsamości mogą należeć do użytkowników, komputerów, aplikacji lub inne zasoby, które stanowią część granicę zabezpieczeń. Można obsługiwać katalogu i tożsamości usługi lokalnego, ale w scenariuszu hybrydowe, gdzie znajdują się elementy aplikacji w Azure może być bardziej efektywne, aby rozszerzyć tę funkcję w chmurze. Tej metody można ograniczyć opóźnienia spowodowane przez wysłanie uwierzytelniania i żądań autoryzacji lokalne z chmury do uruchomienia lokalnego usług katalogu i tożsamości.

Azure udostępnia dwa rozwiązania stosowania katalogu i tożsamości usługi w chmurze:

1. Możesz użyć [Azure Active Directory (AAD)] [ azure-active-directory] do tworzenia domeny AD w chmurze i połączyć go z lokalnego hasło. AAD umożliwia konfigurowanie logowania jednokrotnego (SSO) dla użytkowników korzystających z aplikacji dostępnym w chmurze. [Narzędzie Azure AD Connect] wykorzystuje AAD[ azure-ad-connect] uruchomiony lokalnego replikacji tożsamości i obiektów przechowywanych w repozytorium lokalnego w chmurze.

    Można także zaimplementować AAD bez korzystania z katalogu lokalnego. W tym przypadku AAD działa jako podstawowym źródłem wszystkich informacji o tożsamości zamiast zawierający dane replikowane z katalogu lokalnego.

    Należy zauważyć, że katalog w chmurze jest **nie** rozszerzenie katalogu lokalnego. Jest to raczej kopię, która zawiera tę samą obiektów i tożsamości. Zmiany wprowadzone do tych elementów lokalnego są kopiowane w chmurze, ale zmiany wprowadzone w chmurze, **nie** można replikować powrót do domeny lokalnej.

    >[AZURE.NOTE] Wersje Azure Active Directory Premium obsługuje zapisu tyłu haseł użytkowników, umożliwiając użytkownikom lokalnego wykonywanie samodzielnego resetowania hasła w chmurze. Jest to jedyne dane, które AAD synchronizuje powrót do katalogu lokalnego. Aby uzyskać więcej informacji na temat różnych wersjach AAD i ich funkcji zobacz [Azure Active Directory wersje][aad-editions].

2. Z centrum danych lokalnych, należy wdrożyć maszyny działającego usług AD DS jako kontrolera domeny w Azure, wydłużając istniejącą infrastrukturę AD (łącznie z usług AD DS i usług AD FS). Ta metoda jest najbardziej typowych scenariuszy, w której są połączone sieci lokalnej i Azure wirtualnych sieci przez połączenie VPN i/lub ExpressRoute. To rozwiązanie umożliwia dwukierunkową replikacji pozwoli Ci wprowadzanie zmian w chmurze i lokalnego, tam, gdzie jest najbardziej odpowiednie.

Ta architektura omówiono rozwiązanie 1. Aby uzyskać więcej informacji o drugim rozwiązaniem, zobacz [Rozszerzanie usługi Active Directory Azure][guidance-adds].

Typowe zastosowanie przypadkach dla tej architektury obejmują:

- Dostarczanie rejestracji Jednokrotnej dla użytkowników końcowych uruchomiony władz akredytacji bezpieczeństwa i inne aplikacje w chmurze, przy użyciu tych samych poświadczeń, które określają dla aplikacji lokalnej.

- Publikowanie lokalnych aplikacji sieci web do chmury do udostępnienia użytkownikom zdalnym należących do organizacji.

- Wdrażanie Samoobsługowe funkcje dla użytkowników końcowych, takich jak resetowanie haseł i delegowanie zarządzania grupy. 

    >[AZURE.NOTE] Te funkcje mogą być sterowane przez administratora za pomocą zasad grupy.

- Przy użyciu sieci VPN tunelem lub obwód ExpressRoute nie są bezpośrednio związane sytuacji, w której sieci lokalnej i Azure wirtualna sieć hostingu aplikacje w chmurze.

Należy zauważyć, że AAD nie zawiera wszystkie funkcje AD. Na przykład AAD obecnie obsługuje tylko uwierzytelniania użytkowników zamiast uwierzytelniania komputera. Niektóre aplikacje i usługi, takie jak programu SQL Server może wymagać uwierzytelnienia komputera w przypadku tego rozwiązania nie jest odpowiedni. Ponadto AAD może nie być odpowiednie dla systemów miejsce, w którym składniki można przeprowadzić migrację wielu obramowanie na lokalnej chmury, a nie tylko kopiowana.

> [AZURE.NOTE] Aby uzyskać szczegółowy opis działania usługi Azure Active Directory, obejrzyj [Microsoft Active Directory wyjaśniono][aad-explained].

## <a name="architecture-diagram"></a>Diagram architektury

Poniższy diagram wyróżnienie ważne składniki w tej architektury. Aby uzyskać więcej informacji o elementach obciążenie pracą w Azure, przeczytaj [Uruchomione maszyny wirtualne dla architektury N warstwowych Azure][implementing-a-multi-tier-architecture-on-Azure]:

> [AZURE.NOTE] Dla uproszczenia ten schemat tylko Pokazuje połączenia bezpośrednio związane z AAD i nie przedstawić przekierowania żądania przeglądarki sieci web lub inne protokół przesyłanych, który może wystąpić w ramach procesu federacji tożsamości i uwierzytelniania. Na przykład użytkownika (lokalnego lub zdalnego) zwykle uzyskuje dostęp do aplikacji sieci web za pomocą przeglądarki sieci i aplikacji sieci web może być przezroczysty przekierowywanie przeglądarki sieci web do uwierzytelniania żądania za pośrednictwem AAD. Po uwierzytelnieniu, żądania mogą być przekazywane do aplikacji sieci web wraz z informacjami odpowiednią tożsamość.

[! [0]][0]

- **Dzierżawy usługi azure Active Directory**. Jest to wystąpienie AAD utworzone przez organizację. Działa jako usługi katalogowej prostej dla aplikacje w chmurze (przechowuje obiekty kopiowany z lokalnego AD), a także tożsamości usług.

- **Podsieć poziomu sieci web**. Danej podsieci zawiera maszyny wirtualne, które zaimplementować niestandardową aplikację opartej na chmurze opracowanych przez organizację i którym AAD może działać jako broker tożsamości.

- **Lokalnego serwera usług AD DS**. Twoja organizacja może ma już istniejące usługi katalogowej, takie jak usług AD DS. W katalogu usług AD DS (na przykład informacji o użytkownikach i grupy) z AAD, aby włączyć AAD do uwierzytelnienia tożsamości za pomocą tych informacji można synchronizować liczby elementów.

- **Narzędzie azure AD Connect synchronizacją serwera**. Jest to lokalnego na komputerze, na którym działa narzędzie Azure AD Connect usługi synchronizacji. Za pomocą oprogramowania narzędzie Azure AD Connect jest Zainstaluj tę usługę. Usługa synchronizacji Azure AD Connect synchronizuje informacji (użytkowników, grupy, Kontakty itd.) przechowywanych w wersji lokalnej AD do AAD w chmurze. Na przykład możesz dodawać lub deprovision grup i użytkowników lokalnych i te zmiany są przenoszone do AAD. Usługi synchronizacji Azure AD Connect przekazuje informacje do dzierżawy AAD.

    >[AZURE.NOTE] Ze względów bezpieczeństwa hasła użytkownika nie są przechowywane bezpośrednio w AAD. Zamiast AAD zawiera skrótu każdego hasła. Jest to wystarczające do weryfikacji hasła użytkownika. Jeśli użytkownik wymaga Resetowanie hasła, musi to być wykonywana lokalnego i nowego skrótu wysyłane do AAD. Wersji AAD Premium zawiera funkcje, które można zautomatyzować to zadanie, aby umożliwić użytkownikom zresetować swoje własne hasła.

## <a name="recommendations"></a>Zalecenia

W tej sekcji przedstawiono zalecenia dotyczące implementowania AAD w następujący sposób.

- AD Connect
- Zabezpieczenia

### <a name="azure-ad-connect-sync-service-recommendations"></a>Rekomendacje usług synchronizacji usługi Azure AD Connect

Synchronizacja dotyczy zapewnia, że informacje o tożsamości użytkownicy w chmurze jest zgodny z formatem przechowywanych lokalnie. Celem usługi synchronizacji Azure AD Connect jest aby zachować spójność ten. Następujące punkty podsumowanie zalecenia dotyczące implementowania synchronizacji usługi Azure AD Connect:

- Przed wdrożeniem Azure AD Connect synchronizacji, należy sprawdzić wymagania synchronizacji Twojej organizacji (co do synchronizacji, z którymi domenami i częstotliwości. Artykuł [Określanie wymagania synchronizacji katalogów] [ aad-sync-requirements] w tym artykule opisano punktów, które należy wziąć pod uwagę.

- Można uruchomić usługę synchronizacji Azure AD Connect przy użyciu maszyny lub hostowanej komputera lokalnego. W zależności od zmienności informacje znajdujące się w katalogu AD po wykonaniu początkowej synchronizacji z AAD obciążenie usługi synchronizacji Azure AD Connect najprawdopodobniej będzie wysoka. Przy użyciu maszyny pozwala na łatwiejsze skalowanie na serwerze (monitor aktywności, zgodnie z opisem w sekcji [zagadnienia monitorowania](#monitoring-considerations) do określenia, czy jest to konieczne).

- Jeśli masz wiele domen lokalnego w las można przechowywać i synchronizować informacje o cały las do jednej dzierżawy AAD (jest to zalecane podejście). Informacje o filtrach dla tożsamości, które występują w więcej niż jedną domenę, dzięki czemu każdej tożsamości pojawia się tylko raz w AAD nie jest duplikowany, gdyż może prowadzić do niespójności podczas synchronizowania danych. Ta metoda wymaga wykonania wielu serwerów Synchronizuj narzędzie Azure AD Connect. Aby uzyskać więcej informacji zobacz scenariusz wielu AAD w sekcji [Uwagi dotyczące topologii](#topology-considerations) . 

- Umożliwia filtrowanie ograniczenie danych przechowywanych w AAD do tylko, które są niezbędne. Na przykład Twoja organizacja nie może być przechowywanie informacji o kontach nieaktywny lub -osobiste w AAD. Filtrowanie może być oparte na grupach, oparty na domenie, oparte na jednostkę Organizacyjną lub opartą na atrybutach i można połączyć filtrów w celu wygenerowania bardziej złożonych reguł. Można na przykład umożliwia synchronizowanie tylko obiekty przechowywanych w domenie, które mają określoną wartość w zaznaczony atrybut. Aby uzyskać szczegółowe informacje, zobacz [synchronizacji Azure AD Connect: Konfigurowanie filtrowania][aad-filtering].

- Aby wdrożyć wysoką dostępność usługi synchronizacji AD Connect, uruchom serwer tymczasowy pomocniczy. Aby uzyskać więcej informacji zobacz sekcję [zagadnienia topologii](#topology-considerations) .

### <a name="security-recommendations"></a>Zalecenia dotyczące zabezpieczeń

Następujące elementy podsumowanie zalecenia dotyczące podstawowego zabezpieczeń stosowania AAD, w zależności od potrzeb organizacji:

- Zarządzanie haseł użytkowników.

    Jeśli używasz premium edition AAD hasło zapisu zwrotne od AAD do katalogu lokalnego można włączyć, wykonując instalację niestandardową Azure AD Connect:

    [! [9]][9]

    Ta funkcja umożliwia użytkownikom zresetować swoje własne hasła z poziomu portalu Azure, ale powinna być włączone tylko po przejrzeniu zasadami zabezpieczeń hasła w organizacji. Na przykład mogą ograniczyć, którzy użytkownicy mogą zmieniać swoje hasła, a następnie można dostosować środowisko zarządzania hasła. Aby uzyskać więcej informacji, zobacz [Dostosowywanie Zarządzanie hasłami do potrzeb organizacji][aad-password-management].

- Zachowywanie ochrony dla aplikacji lokalnej, które są dostępne zewnętrznie.

    Serwer Proxy aplikacji usługi Azure AD umożliwia uzyskanie kontroli dostępu w aplikacjach sieci web w lokalnej od użytkowników zewnętrznych przez AAD. Tej metody chroni aplikacji, umożliwiając nie je bezpośrednio do Internetu. Tylko użytkownicy, którzy mają prawidłowe poświadczenia w katalogu Azure są dostęp do aplikacji. Aby uzyskać więcej informacji, zobacz artykuł [Włączanie serwera Proxy aplikacji w portalu Azure][aad-application-proxy].

- Aktywnie monitoruje AAD objawów podejrzane działania.

    Warto rozważyć użycie AAD Premium P2 edition, który zawiera ochronę tożsamości AAD. Ochrona tożsamości używa algorytmów adaptacyjne maszynowego uczenia i heurystykę wykrywanie różnic w odniesieniu do ryzyka zdarzenia, które mogą wskazywać, że jest już bezpieczne tożsamości. Na przykład może wykryć potencjalnie anomalous działania, na przykład o nieregularnym kształcie logowania działania rejestrowaniu z nieznanych źródeł lub adresy IP z działaniem podejrzanych lub rejestrowaniu z urządzeń, które mogą być zainfekowane. Za pomocą tych danych, raportów i alertów, które umożliwia badanie te zdarzenia ryzyka, a następnie wykonaj odpowiednią naprawy lub akcję łagodzenia ochronę tożsamości generuje. Aby uzyskać więcej informacji, zobacz [Azure Active Directory ochronę tożsamości][aad-identity-protection].

    Za pomocą funkcji raportowania AAD w portalu Azure monitorowanie podejrzanych i innych zabezpieczeń działań związanych z występujące w systemie. AAD można wygenerować serii raportów:

    [! [17]][17]

    Aby uzyskać więcej informacji na temat korzystania z tych raportów, zobacz [Azure Active Directory raportowania przewodnik][aad-reporting-guide].

## <a name="topology-considerations"></a>Zagadnienia dotyczące topologii

Jeśli są Integracja katalogu lokalnego z AAD, skonfiguruj narzędzie Azure AD Connect do wykonania topologii, który najlepiej pasuje do wymagań danej organizacji. Topologii obsługiwanych przez narzędzie Azure AD Connect są następujące:

- **Jeden las, jednego katalogu AAD**. W tej topologii Azure AD Connect umożliwia synchronizowanie obiektów i informacje o tożsamości w jednej lub kilku domen w jednym las z jednej dzierżawy AAD lokalnego. To jest wykonywane przez Instalacja ekspresowa Azure AD Connect domyślnej topologii.

    [! [1]][1]

    Nie należy tworzyć wiele serwerów Synchronizuj narzędzie Azure AD Connect nawiązania różnych domenach w tym samym las lokalnego tej samej dzierżawy AAD, chyba że używasz serwer Azure AD Connect synchronizacji w trybie tymczasowej (zobacz tymczasowej Server poniżej).

- **Wiele lasów, jednego katalogu AAD**. Jeśli masz więcej niż jeden las lokalnego za pomocą tej topologii. Informacje o tożsamości można skonsolidować tak, aby każdy użytkownik unikatowe reprezentowany jest raz w katalogu AAD, nawet jeśli użytkownik istnieje w więcej niż jeden las. Wszystkie lasów Użyj tego samego serwera synchronizacji Azure AD Connect. Serwer Azure AD Connect synchronizacją nie musi należeć do dowolnej domeny, ale musi to być dostępne z wszystkich lasów:

    [! [2]][2]

    W tej topologii nie używaj osobnych Azure AD Connect zsynchronizować serwery nawiązywania połączenia z jednej dzierżawy AAD każdy las lokalnego. Może to spowodować tożsamości zduplikowane informacje w AAD Jeśli użytkowników znajdują się w więcej niż jeden las.

- **Wiele lasów, oddzielnych topologii**. Tej metody można scalić informacje tożsamości z osobny las jednej dzierżawy AAD. Strategia ta jest przydatne, jeśli polega na łączeniu lasami z innych organizacji (po ocena fuzji lub przejęcia, na przykład), i informacje o tożsamości dla każdego użytkownika jest przechowywana w tylko jeden las:

    Jeśli GAL w każdym las są zsynchronizowane, a następnie użytkownika w jeden las może znajdować się w innej jako kontakt. Może to występować, jeśli na przykład organizacja została dokonana GALSync Forefront Identity manager 2010 lub Microsoft Identity Manager 2016. W tym scenariuszu można określić, że użytkownicy powinny być oznakowane ich atrybut *poczty* . Można również dopasować tożsamości przy użyciu atrybutów *ObjectSID* i *msExchMasterAccountSID* . Jest to przydatne, jeśli masz jeden lub więcej lasów zasobów z konta wyłączone.

- **Przemieszczenia serwera**. W tej konfiguracji można uruchomić drugiego wystąpienia serwera synchronizacji Azure AD Connect równolegle z pierwszej. Ta struktura obsługuje scenariuszy, takich jak:

    - Wysokiej dostępności.

    - Testowanie i wdrażanie nowej konfiguracji serwera synchronizacji Azure AD Connect.

    - Wprowadzenie do nowego serwera i likwidować stare konfiguracji. 

    W poniższych scenariuszach drugiego wystąpienia jest uruchamiany w *trybie*. Serwer rekordów zaimportowanych obiektów i synchronizacja danych w bazie danych, ale nie przekazuje dane do AAD. Tylko wtedy, gdy wyłączyć tryb tymczasowy serwer zacznij wpisywać danych AAD i uruchamia wykonywanie hasło zapisu-wykonuje kopię do katalogów lokalnych odpowiednio:

    [! [4]][4]

    Aby uzyskać więcej informacji, zobacz [synchronizacji Azure AD Connect: zadań operacyjnych i zagadnienia][aad-connect-sync-operational-tasks].

- **Wiele AAD katalogów**. Zalecane jest, aby utworzyć jeden katalog AAD dla organizacji, ale mogą wystąpić sytuacje, w którym ma być partycją informacji w oddzielnych katalogach AAD. W tym przypadku należy zagwarantować, że każdy obiekt z las lokalnego pojawia się tylko w jednym katalogu AAD, aby uniknąć problemów wstecz zapisu synchronizacji i hasło.

    Aby wdrożyć w tym scenariuszu, skonfigurować osobne Azure AD Connect synchronizowanie serwery dla każdego katalogu AAD i korzystania z filtrowania, każdy serwer synchronizacji Azure AD Connect działa na wzajemnie wykluczających się zestawu obiektów: 

    [! [5]][5]

Aby uzyskać więcej informacji na temat tych topologii zobacz [topologii dla Azure AD Connect][aad-topologies].

## <a name="user-sign-in-considerations"></a>Zagadnienia logowania użytkownika

Domyślnie usługa AAD zakłada logowania użytkowników, dostarczając tego samego hasła, że mogą używać lokalnego i serwer Azure AD Connect synchronizacją konfiguruje synchronizacji haseł między lokalną domeną i AAD. W przypadku wielu organizacji jest to właściwe rozwiązanie, ale należy rozważyć istniejących zasad i infrastruktury organizacji. Na przykład:

- Zasady zabezpieczeń organizacji może być uniemożliwiają synchronizowanie hasła w chmurze.

- Użytkownicy odczuwać Bezproblemowa logowania jednokrotnego (bez dodatkowe hasła zostanie wyświetlony monit) może wymagać podczas uzyskiwania dostępu do chmury zasobów z domeny komputerach połączonych za pomocą sieci firmowej.

- Twoja organizacja może być już ADFS lub innego Federacji dostawcy wdrożony. Możesz skonfigurować AAD, aby użyć tej infrastruktury Implementowanie uwierzytelniania i logowania jednokrotnego, a nie za pomocą hasła informacji przechowywanych w chmurze.

Aby uzyskać więcej informacji, zobacz [Azure AD łączenie użytkownika logowania na temat opcji][aad-user-sign-in].

## <a name="azure-ad-application-proxy-considerations"></a>Uwagi dotyczące Azure AD aplikacji serwera proxy

Azure AD umożliwia uzyskanie dostępu do aplikacji lokalnej.

- Udostępnianie aplikacji sieci web w lokalnej za pomocą serwera proxy aplikacji, której łączników zarządzane przez składnik Azure AD aplikacji serwera proxy. Łącznik serwera proxy aplikacji zostanie wyświetlona połączenia wychodzącego z serwera proxy aplikacji Azure AD. Użytkownicy zdalnego żądania są kierowane z AAD przez to połączenie do aplikacji sieci web. Ten mechanizm usuwa musisz otwierać porty przychodzące w zaporze lokalnego zmniejszenie powierzchni atakiem udostępniane przez organizację.

Aby uzyskać więcej informacji, zobacz [Publikowanie aplikacji przy użyciu serwera proxy aplikacji Azure AD][aad-application-proxy].

## <a name="object-synchronization-considerations"></a>Zagadnienia dotyczące synchronizacji obiektu

Domyślna konfiguracja Azure AD Connect synchronizuje obiekty z katalogu lokalnego AD na podstawie zestawu reguł określonych w artykule [synchronizacji Azure AD Connect: opis konfiguracji domyślnej][aad-connect-sync-default-rules]. Tylko obiekty, które spełniają te reguły są synchronizowane, inne osoby nie są uwzględniane. Na przykład obiektów użytkownika musi mieć atrybut unikatowe *sourceAnchor* i atrybutu *accounEnabled* , muszą być wypełnione. Obiektów użytkowników, które nie mają atrybut *sAMAccountName* i rozpocząć z tekstem *AAD_* lub *MSOL_* nie są synchronizowane. Narzędzie Azure AD Connect dotyczy kilka reguł obiektów użytkownika, kontaktu, grupa, ForeignSecurityPrincipal i komputera. Jeśli chcesz zmodyfikować domyślny zestaw reguł przy użyciu edytora reguły synchronizacji instalowane z Azure AD Connect (również opisane w [synchronizacji Azure AD Connect: opis konfiguracji domyślnej][aad-connect-sync-default-rules]).

Można zdefiniować własne filtry, aby ograniczyć obiekty, które mają być synchronizowane przez domenę lub jednostkę Organizacyjną. Lub wykonania bardziej złożone filtrowanie niestandardowe, zgodnie z opisem w [synchronizacji Azure AD Connect: Konfigurowanie filtrowania][aad-filtering].

## <a name="security-considerations"></a>Zagadnienia dotyczące zabezpieczeń

Kontrola dostępu warunkowego umożliwiają odrzucać żądania uwierzytelniania z nieznanych źródeł:

- Wyzwalanie [Azure uwierzytelnianie wieloskładnikowe (MFA)] [ azure-multifactor-authentication] Jeśli użytkownik próbuje nawiązać połączenie z wartością zaufanej lokalizacji (takich jak w Internecie) zamiast zaufana sieć.

- Umożliwia określenie zasady dostępu do aplikacji i funkcji typ platformy urządzenia użytkownika (iOS, Android, systemu Windows Mobile w systemie Windows).

- Rejestrowanie włączone/wyłączone stanu użytkowników urządzeń i włączać te informacje do testy zasady dostępu. Na przykład jeśli telefonu użytkownika zostanie utracony lub kradzież powinno być rejestrowane jako wyłączona, aby zapobiec używanego do uzyskiwania dostępu.

- Kontrolowanie poziomu dostępu do użytkownika na podstawie członkostwa grup. Przy użyciu [reguł członkostwa dynamiczne AAD] [ aad-dynamic-membership-rules] w celu uproszczenia administracji grupy. Krótkie omówienie jak to działa, zobacz [Wprowadzenie do dynamicznego członkostwa dla grup][aad-dynamic-memberships].

- Za pomocą dostępu warunkowego ryzyka zasady ochrony tożsamości AAD chroniące Zaawansowane na podstawie nietypowych działania logowania lub innych zdarzeń.

Aby uzyskać więcej informacji, zobacz [usługi Azure Active Directory dostępu warunkowego][aad-conditional-access].

## <a name="scalability-considerations"></a>Zagadnienia dotyczące skalowalność

Skalowalność dotyczy usługa AAD i konfiguracji serwera synchronizacji Azure AD Connect:

- W usłudze AAD nie trzeba konfigurować odpowiednie opcje w celu wdrożenia skalowalność. Usługa AAD obsługuje skalowalność według repliki. AAD wykonuje się pojedynczego replice podstawowego, które uchwyty operacji zapisu, i wielu repliki pomocniczej tylko do odczytu. AAD przekierowuje przezroczysty próby zapisy wprowadzone przed pomocniczej repliki replice podstawowego. AAD zapewnia spójność ostatecznego. Wszystkie zmiany wprowadzone w replice podstawowego są przenoszone do pomocniczej replik. Większość operacji przed AAD są Odczyt, a nie zapisuje, ta architektura skale również.

    Aby uzyskać więcej informacji, zobacz [Azure AD: Pokaż ustawienia zaawansowane naszych katalogu chmury zbędne geo, wysokiej dostępności, rozłożone][aad-scalability].

- Na serwerze narzędzie Azure AD Connect synchronizacją należy określić, jak wiele obiektów najprawdopodobniej będzie synchronizować z katalogu lokalnego. Jeśli masz mniej niż 100 000 obiektów, używając programu SQL Server Express LocalDB domyślne wyposażone narzędzie Azure AD Connect. Jeśli masz wiele obiektów, należy zainstalować wersję produkcji programu SQL Server i przeprowadzić instalację niestandardową: narzędzie Azure AD Connect określającą, że należy używać istniejącego wystąpienia programu SQL Server.

## <a name="availability-considerations"></a>Zagadnienia dotyczące dostępności

Podobnie jak w przypadku wątpliwości skalowalność dostępność obejmuje usługę AAD i konfigurację Azure AD Connect:

- Usługa AAD służy do zapewnienia wysokiej dostępności. Dostępne są opcje nie można konfigurować użytkownika dostępności. Jest rozkładana geo i zostanie uruchomiona w wielu danych wyśrodkowanie widoku na świecie z automatyzacji awaryjnego przełączania. Jeśli centrum danych jest niedostępny, AAD sprawia, że dane katalogu jest dostępna dla wystąpienia w co najmniej dwa więcej centrach danych regionalnie rozproszone.

    >[AZURE.NOTE] Umowa dotycząca poziomu usług AAD podstawowe i Premium dostępność % gwarancje co najmniej 99,9 usług. Istnieje umowa dotycząca poziomu usług dla warstwie bezpłatne AAD. Aby uzyskać więcej informacji, zobacz [Umowa dotycząca poziomu usług dla usługi Azure Active Directory][sla-aad].

- Aby zwiększyć dostępność serwera synchronizacji Azure AD Connect drugiego wystąpienia można uruchamiać w trybie tymczasowej zgodnie z opisem w sekcji [Uwagi dotyczące topologii](#topology-considerations) . 

    Ponadto jeśli nie używasz wystąpienie programu SQL Server Express LocalDB dostępnego w usłudze Azure AD Connect, następnie należy rozważyć wysoką dostępność dla programu SQL Server. Należy zauważyć, że obsługiwane tylko wysoce zalecane jest klaster SQL. Rozwiązania, takie jak odzwierciedlające i zawsze na nie są obsługiwane przez narzędzie Azure AD Connect.

    Aby uzyskać dodatkowe informacje na temat utrzymywania dostępności serwera synchronizacji Azure AD Connect i jak odzyskać po awarii, zobacz [synchronizacji Azure AD Connect: zadań operacyjnych i zagadnienia — odzyskiwanie][aad-sync-disaster-recovery].

## <a name="management-considerations"></a>Uwagi dotyczące zarządzania

Istnieją dwa aspekty zarządzania AAD:

1. Administrowanie AAD w chmurze.

2. Obsługa serwerów Synchronizuj narzędzie Azure AD Connect.

AAD udostępnia następujące opcje służące do zarządzania domenami i katalogi w chmurze:

- [Moduł usługi Azure Active Directory programu PowerShell][aad-powershell]. Jeśli musisz skrypt typowych zadań administracyjnych Azure AD takich jak zarządzanie użytkownikami, zarządzania domeną i konfigurowania rejestracji jednokrotnej za pomocą tego modułu.

- Karta Zarządzanie Azure AD w portalu Azure. Ta karta zawiera widoku zarządzania interakcyjnych katalogu i pozwala na sterowanie i skonfigurować większość aspektów AAD.

    [! [10]][10]

Narzędzie Azure AD Connect instaluje następujące narzędzia, które służy do przechowywania Azure AD Connect usług synchronizacji z komputery lokalnego:

- Konsola Microsoft Azure Active Directory połączenia. To narzędzie umożliwia modyfikowanie konfiguracji serwera Azure AD Sync, dostosowywanie, jak synchronizacja jest przeprowadzana, włączanie lub wyłączanie trybu tymczasowy i przełączanie logowania tryb użytkownika (można włączyć AD FS logowania przy użyciu infrastruktury lokalnego).

- Menedżer usługi synchronizacji. Karta *operacje* w tym narzędziu do zarządzania procesu synchronizacji i wykrywanie, czy wszystkie części tego procesu nie powiodła się. Może wyzwolić synchronizacji ręcznie za pomocą tego narzędzia. 

    [! [12]][12]

    Na karcie *Łączniki* umożliwia sterowanie połączeniami dla domen (lokalnej i w chmurze) do jest dołączona aparat synchronizacji:

    [! [13]][13]

-  Edytor reguł synchronizacji. Aby dostosować sposób, w której obiekty są przekształcane podczas kopiowania między katalogu lokalnego i AAD w chmurze za pomocą tego narzędzia. To narzędzie umożliwia określenie dodatkowych atrybutów i obiektów do synchronizacji i filtrów w celu określenia, które wystąpienia powinny lub nie mają być synchronizowane.

    Aby uzyskać więcej informacji, zobacz sekcję Edytor reguł synchronizacji w dokumencie [synchronizacji Azure AD Connect: opis konfiguracji domyślnej][aad-connect-sync-default-rules].

Strony [synchronizacji Azure AD Connect: Najważniejsze wskazówki dotyczące zmieniania konfiguracji domyślnej] [ aad-sync-best-practices] zawiera dodatkowe informacje i wskazówki dotyczące zarządzania Azure AD Connect.

## <a name="monitoring-considerations"></a>Zagadnienia dotyczące monitorowania

Monitorowanie kondycji jest wykonywane za pomocą serii lokalne agentów zainstalowany:

- Narzędzie Azure AD Connect instaluje agenta znajdują się informacje dotyczące synchronizacji. Monitorowanie kondycji i wydajności Azure AD Connect za pomocą karta Azure Active Directory łączenie kondycji w portalu Azure. Aby uzyskać więcej informacji, zobacz [Przy użyciu Azure AD łączenie zdrowia synchronizacji][aad-health].

- Monitorowanie kondycji domeny usług AD DS i katalogów z platformy Azure, zainstaluj Azure AD łączenie kondycji agenta usług AD DS na komputerze w ramach domeny lokalnej. Karta Azure Active Directory łączenie kondycji za pomocą w portalu Azure Monitor usług AD DS. Aby uzyskać więcej informacji zobacz [Używanie Azure AD łączenie kondycji z usługami AD DS][aad-health-adds]

- Zainstaluj Azure AD łączenie kondycji agenta usług AD FS monitoruje kondycję usług AD FS uruchomionych dla lokalnego i monitorowanie usług AD DS za pomocą karta Azure Active Directory łączenie kondycji w portalu Azure. Aby uzyskać więcej informacji zobacz [Przy użyciu Azure AD łączenie kondycji z usług AD FS][aad-health-adfs]

Aby uzyskać więcej informacji na temat instalowania agentów kondycji łączenie AD i ich wymagania zobacz [Azure AD łączenie kondycji instalacji agenta][aad-agent-installation].

## <a name="sample-solution"></a>Przykładowe rozwiązanie

W tej sekcji opisano instrukcje dotyczące tworzenia rozwiązanie próbki, którego można używać do testowania konfiguracji AAD. Rozwiązanie obejmuje następujące elementy:

- Aplikacja web n warstwowych zgodnie ze strukturą opisaną za [Pośrednictwem systemem SMS dla architektury N warstwowych Azure][implementing-a-multi-tier-architecture-on-Azure].

- Komputer kliencki test. Zaleca się przy użyciu innego maszyn wirtualnych na tym komputerze. Konfiguracja tego maszyn wirtualnych jest ważne, ile masz uprawnienia administratora i można nawiązać aplikacji sieci web n warstwowych.

- Symulowany lokalnego środowiska, która zawiera własnej domeny utworzony przy użyciu usług AD DS.

Scenariusze, pokazujących, jak te kroki są:

- Włączanie dostępu do aplikacji sieci web n warstwowych uruchomiony w chmurze dla użytkowników zewnętrznych z AAD uwierzytelniania hasła.

- Włączanie dostępu do aplikacji sieci web n warstwowych uruchomiony w chmurze użytkownikom w organizacji, systemie AAD uwierzytelniania hasła.

### <a name="prerequisites"></a>Wymagania wstępne

Czynności, które należy wykonać założono następujące wymagania:

- Masz istniejącej subskrypcji Azure, w którym można tworzyć grup zasobów.

- Zainstalowano [interfejs wiersza polecenia Azure][azure-cli].

- Zostały pobrane i zainstalowane najnowszej kompilacji. Zobacz [tutaj] [ azure-powershell-download] instrukcje.

- Zainstalowano kopię [makecert] [ makecert] narzędzia na komputerze klienckim.

- Masz dostęp do komputera z programu Visual Studio zainstalowany.

### <a name="create-the-n-tier-web-application-architecture"></a>Tworzenie Architektura aplikacji sieci web n warstwowych

1. Utwórz folder o nazwie `Scripts` zawierający podfolder `Parameters`.

2. Pobierz [Rozmieszczanie ReferenceArchitecture.ps1] [ solution-script] skrypt programu PowerShell do folderu skryptów.

3. W folderze parametrów Utwórz podfolder innego systemu Windows.

4. Pobierz następujące pliki do folderu parametrów/Windows:

    - [virtualNetwork.parameters.json][vnet-parameters-windows]

    - [networkSecurityGroup.parameters.json][nsg-parameters-windows]

    - [webTierParameters.json][webtier-parameters-windows]

    - [businessTierParameters.json][businesstier-parameters-windows]

    - [dataTierParameters.json][datatier-parameters-windows]

    - [managementTierParameters.json][managementtier-parameters-windows]

5. Edytowanie pliku **ReferenceArchitecture.ps rozmieszczanie**1 w folderze skryptów, a następnie zmień następujący wiersz, aby określić grupa zasobów, która powinna zostać utworzona lub służy do przechowywania maszyn wirtualnych i zasoby utworzone przez skrypt:

        ```powershell
        # PowerShell
        $resourceGroupName = "ra-aad-ntier-rg"
        ```

6. Edytuj następujące pliki w folderze s **parametrów okno**i ustaw `resourceGroup` wartość w `virtualNetworkSettings` sekcji w każdej z tych plików, aby być taka sama jak określona w pliku skryptu **ReferenceArchitecture.ps1 rozmieszczanie** .

    - networkSecurityGroup.parameters.json

    - webTierParameters.json

    - businessTierParameters.json

    - dataTierParameters.json

    - managementTierParameters.json

7. Otwórz okno programu PowerShell Azure, przejdź do folderu skryptów i uruchom następujące polecenie:

        ```powershell
        .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Windows ntier
        ```

    Zamienianie **<subscription id>** ze swojego identyfikatora Azure subskrypcji.

    Aby uzyskać **<location>**, określ region Azure, na przykład *eastus* lub *westus*.

8. Po zakończeniu skryptu, Azure portal umożliwia uzyskanie publiczny adres IP równoważenia obciążenia warstwy sieci web (*Pomoc zdalna aad-ntier-sieci web — kg*):

    [! [18]][18]

9. Zaloguj się do badania klienta komputera (lub maszyn wirtualnych) i sprawdź, czy mają dostęp warstwie sieci web przy użyciu programu Internet Explorer przejdź do publiczny adres IP usługi równoważenia obciążenia warstwy sieci web. Powinien zostać wyświetlony domyślnej strony usług IIS.

### <a name="simulate-configuration-of-a-public-web-site"></a>Symulacja konfiguracji publicznej witryny sieci web

>[AZURE.NOTE] Poniższe kroki symuluje kojarzenie warstwie sieci web z www.contoso.com nazw DNS, modyfikując plik hosts na komputerze klienckim. Ponadto maszyny wirtualne poziomu sieci web są skonfigurowane z certyfikatów z podpisem własnym i sfałszowana obsługującego urząd. Jest to do testowania wyłącznie, a **nie należy używać tych technik w środowisku produkcyjnym**.

1. Na komputerze klienckim testowych edytować plik **C:\Windows\System32\drivers\etc\hosts** za pomocą Notatnika, a następnie dodać wpis, który przypisuje www.contoso.com nazwę publiczny adres IP usługi równoważenia obciążenia warstwy sieci web:

        ```text
        # Copyright (c) 1993-2009 Microsoft Corp.
        #
        # This is a sample HOSTS file used by Microsoft TCP/IP for Windows.
        #
        # This file contains the mappings of IP addresses to host names. Each
        # entry should be kept on an individual line. The IP address should
        # be placed in the first column followed by the corresponding host name.
        # The IP address and the host name should be separated by at least one
        # space.
        #
        # Additionally, comments (such as these) may be inserted on individual
        # lines or following the machine name denoted by a '#' symbol.
        #
        # For example:
        #
        #      102.54.94.97     rhino.acme.com          # source server
        #       38.25.63.10     x.acme.com              # x client host
        
        # localhost name resolution is handled within DNS itself.
        #   127.0.0.1       localhost
        #   ::1             localhost
        
        52.165.38.64    www.contoso.com
        ```

2. Upewnij się, że teraz można przejść do www.contoso.com na komputerze klienckim. Tej samej stronie usług IIS domyślne powinien pojawić się, jak poprzednio.

3. Na komputerze klienckim otwórz wiersz polecenia jako Administrator, a za pomocą narzędzia makecert-c
4. Twórz fałszywych głównego urzędu certyfikacji:

        ```
        makecert -sky exchange -pe -a sha256 -n "CN=MyFakeRootCertificateAuthority" -r -sv MyFakeRootCertificateAuthority.pvk MyFakeRootCertificateAuthority.cer -len 2048
        ```

    Sprawdź, czy są tworzone następujące pliki:

    - MyFakeRootCertificateAuthority.cer

    - MyFakeRootCertificateAuthority.pvk

4. Uruchom następujące polecenie, aby zainstalować główny urząd certyfikacji test:

        ```
        certutil.exe -addstore Root MyFakeRootCertificateAuthority.cer
        ```

5. Urząd certyfikacji test służy do generowania certyfikat www.contoso.com:

        ```
        makecert -sk pkey -iv MyFakeRootCertificateAuthority.pvk -a sha256 -n "CN=www.contoso.com" -ic MyFakeRootCertificateAuthority.cer -sr localmachine -ss my -sky exchange -pe
        ```

6. Uruchamianie polecenia **konsoli mmc** i Dodawanie przystawki Certyfikaty dla konta komputera na komputerze lokalnym.

7. W */Certificates (komputer lokalny) / osobiste/certyfikatu i* przechowywania, wyeksportuj certyfikat www.contoso.com z kluczem prywatnym w pliku o nazwie www.contoso.com.pfx:

    [! [20]][20]

8. Wróć do portalu Azure i Połącz z poziomu zarządzania maszyn wirtualnych (*Pomoc zdalna aad-ntier zarządzania vm1*). Domyślna nazwa użytkownika to *testuser* przy użyciu hasła *AweS0me@PW*:

    [! [21]][21]
    
9. Z poziomu zarządzania maszyn wirtualnych nawiązywanie połączenia z każdego z poziomu sieci web maszyny wirtualne z kolei za pomocą nazwy użytkownika *testuser* i hasła *AweS0me@PW*i wykonać następujące zadania. Należy zauważyć, że maszyny wirtualne prywatne adresy IP 10.0.1.4, 10.0.1.5 i 10.0.1.6:

    >[AZURE.NOTE] Warstwa maszyny wirtualne mieć tylko prywatnych adresów IP. Możesz można łączyć się je przy użyciu warstwie zarządzania maszyn wirtualnych.

    1. Kopiowanie plików *www.contoso.com.pfx* i *MyFakeRootCertificateAuthority.cer* na komputerze klienckim.
    
    2. Otwórz okno wiersza polecenia jako Administrator, przejdź do folderu zablokowanych plików, które zostały skopiowane i uruchom następujące polecenia:
    
        ```
        certutil.exe -privatekey -importPFX my www.contoso.com.pfx NoExport

        certutil.exe -addstore Root MyFakeRootCertificateAuthority.cer
        ```

    3. Uruchamianie `mmc` polecenia, Dodawanie przystawki Certyfikaty dla konta komputera na komputerze lokalnym i sprawdź, czy zostały zainstalowane następujące certyfikaty:

        - Www.contoso.com \Personal\Certificates\ \Certificates (komputer lokalny)

        - Authorities\Certificates\MyFakeRootCertificateAuthority certyfikacji głównego \Trusted \Certificates (komputer lokalny)

    4. Uruchom konsolę menedżera informacji usług internetowych i przejdź do *witryny sieci Web Sites\Default* na tym komputerze.

    5. W okienku *akcji* kliknij *powiązań*i dodać powiązanie https za pomocą certyfikatu SSL www.contoso.com:

        [! [22]][22]

10. Wróć do komputerze klienckim i sprawdź, czy możesz teraz przejść do https://www.contoso.com.

### <a name="create-an-azure-active-directory-tenant"></a>Tworzenie dzierżawy usługi Azure Active Directory

1. Za pomocą portalu Azure tworzenie dzierżawy usługi Azure Active Directory, takich jak *myaadname*. onmicrosoft.com, gdzie *myaadname* jest nazwą katalogu, zaznaczone przez Ciebie.

2. Dodawanie użytkownika o nazwie *administratora* z roli GlobalAdmin do katalogu. Zanotuj wygenerowanym hasła.

3. Przy użyciu programu Internet Explorer, przejdź do https://account.activedirectory.windowsazure.com/ i zaloguj się jako admin@ *myaadname*. onmicrosoft.com. Zmień hasło po wyświetleniu monitu.

### <a name="create-and-deploy-a-test-web-application"></a>Tworzenie i wdrażanie aplikacji sieci web test

1. Przy użyciu programu Visual Studio na komputerze rozwoju, tworzenie aplikacji sieci Web programu ASP.NET o nazwie ContosoWebApp1 (Użyj .NET Framework 4.5.2):

    [! [23]][23]

2. Wybierz szablon *MVC* , zmienić uwierzytelnianie do *pracy i szkolnych kont*i określić nazwę swojej dzierżawy AAD. Nie należy tworzyć aplikacji usługi w chmurze.

    [! [24]][24]

3. Tworzenie i uruchamianie aplikacji, aby przetestować uwierzytelniania. Na stronie logowania wprowadź nazwę konta admin@ *myaadname*. onmicrosoft.com, hasło i kliknij pozycję *Zaloguj się*:

    [! [25]][25]

4. Sprawdź, czy AAD zapytanie dotyczące uprawnień do zalogowanie użytkownika i czytanie swój profil, a następnie uruchamiania aplikacji sieci web przy użyciu tożsamości Administrator.

5. Zamknij program Internet Explorer i wróć do programu Visual Studio.

6. Otwórz plik web.config oraz `<appSettings>` sekcji, zmień wartość klucza *ida: PostLogoutRedirectUri* do *https://www.contoso.com:443-*. Zapisz plik.

7. W oknie *Eksploratora rozwiązanie* kliknij prawym przyciskiem myszy projektu ContosoWebApp1, kliknij pozycję *Publikuj*.

8. W oknie *Publikowanie sieci Web* kliknij pozycję *niestandardowe*. Tworzenie niestandardowego profilu o nazwie *ContosoWebApp1*.

9. Na stronie *połączenie* *metody Publikuj* ustawić system *Plików* i określ folder o nazwie *ContosoWebApp*, znajduje się w odpowiednim miejscu na komputerze dewelopera.

10. Na stronie *Ustawienia* ustawienie *konfiguracji* do *wersji*.

11. Na stronie *podglądu* kliknij pozycję *Publikuj*.

12. Nawiązywanie połączenia z każdym serwerze sieci web z kolei (za pośrednictwem warstwie zarządzania maszyn wirtualnych) i wykonywać następujące zadania:

    1. Skopiuj *ContosoWebApp* folder i jego zawartość z komputera programowania do folderu *C:\inetpub* .

    2. Przy użyciu konsoli menedżera informacji usług internetowych, przejdź do *witryny sieci Web Sites\Default* na tym komputerze.

    3. W okienku *Akcje* kliknij pozycję *Ustawienia podstawa*i zmienić fizyczny ścieżkę witryny sieci web do *%SystemDrive%\inetpub\ContosoWebApp*:

        [! [26]][26]

### <a name="publish-the-test-web-application-through-aad"></a>Publikowanie aplikacji sieci web test za pomocą AAD

1. Zaloguj się do portalu Azure i przejdź do katalogu AAD.

2. Na karcie *aplikacje* kliknij aplikację, ContosoWebApp1.

3. Sprawdź, czy aplikacja jest pomyślnie dodana do katalogu, a następnie kliknij kartę *Konfigurowanie* .

4. Zmień adres *URL logowania na* do https://www.contoso.com:443 i ustawić adres *URL odpowiedź* https://www.contoso.com:443 (ten sam adres URL).

5. Zapisywanie konfiguracji.

6. Na komputerze klienckim przejdź do https://www.contoso.com. Sprawdź, czy AAD zostanie wyświetlony monit o podanie poświadczeń, a następnie zaloguj się.

>[AZURE.NOTE] To jest punkt końcowy dla pierwszego scenariusza: Włączanie dostępu do aplikacji sieci web n warstwowych uruchomiony w chmurze dla użytkowników zewnętrznych z AAD uwierzytelniania hasła.

### <a name="create-a-simulated-on-premises-environment-with-active-directory"></a>Tworzenie symulowany lokalnego środowiska z usługą Active Directory

Środowisko lokalne zawiera dwa kontrolery domeny dla `contoso.com` domeny i dwa serwery usługi Azure AD Connect usługi synchronizacji. Serwery usługi Azure AD Connect nie są domeny — połączone.

1. W Eksploratorze plików wróć do folderu skryptów zawierającego skrypt używany do tworzenia aplikacji sieci web N warstwowych.

2. W folderze parametry Dodaj innego folderu sub o nazwie `Onpremise`.

3. Pobierz następujące pliki do folderu parametrów/Onpremise:

    - [Dodawanie dodaje domeny — controller.parameters.json][add-adds-domain-controller-parameters]

    - [Tworzenie dodaje las extension.parameters.json][create-adds-forest-extension-parameters]

    - [virtualMachines adc.parameters.json][virtualMachines-adc-parameters]

    - [virtualMachines-adc-joindomain.parameters.json][virtualMachines-adc-joindomain-parameters]

    - [virtualMachines adds.parameters.json][virtualMachines-adds-parameters]

    - [virtualNetwork.parameters.json][virtualNetwork-parameters]

    - [virtualNetwork dodaje dns.parameters.json][virtualNetwork-adds-dns-parameters]

5. Edytowanie pliku ReferenceArchitecture.ps1 rozmieszczanie w folderze skryptów, a następnie zmień następujący wiersz, aby określić grupa zasobów, która powinna zostać utworzona lub służy do przechowywania maszyn wirtualnych i zasoby utworzone przez skrypt:

        ```powershell
        # PowerShell
        $resourceGroupName = "ra-aad-onpremise-rg"
        ```

6. Edytuj następujące pliki w folderze parametrów/Onpremise i ustaw `resourceGroup` wartość w `virtualNetworkSettings` sekcji w każdej z tych plików, aby być taka sama jak określona w pliku skryptu ReferenceArchitecture.ps1 rozmieszczanie.

    - virtualMachines adds.parameters.json

    - virtualMachines adc.parameters.json

    - virtualNetwork.parameters.json

    - virtualNetwork — powoduje dodanie — dns.parameters.json

7. Otwórz okno programu PowerShell Azure, przejdź do folderu skryptów i uruchom następujące polecenie:

        ```powershell
        .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Windows onpremise
        ```

    Zamienianie `<subscription id>` ze swojego identyfikatora Azure subskrypcji.

    Aby uzyskać `<location>`, określ Azure regionu, takich jak `eastus` lub `westus`.

### <a name="install-and-configure-the-azure-ad-connect-sync-service"></a>Instalowanie i konfigurowanie usługi synchronizacji Azure AD Connect

Konfiguracja przedstawionych w tych krokach składa się z dwóch wystąpień Azure AD Connect usługi synchronizacji. Pierwszy jest aktywny, gdy drugi jest skonfigurowane w trybie tymczasowy, aby zapewnić szybkie przełączanie awaryjne i wysokiej dostępności, jeśli pierwszy serwer ulegnie awarii.

1. Za pomocą portalu Azure, przejdź do grupy zasobów, przytrzymując maszyny wirtualne dla usług synchronizacji Azure AD Connect (*Pomoc zdalna aad-onpremise-rg* domyślnie) i nawiązać *Pomoc zdalna aad-onpremise-adc-vm1* maszyn wirtualnych. Zaloguj się jako *testuser* przy użyciu hasła *AweS0me@PW*.

2. Pobierz narzędzie Azure AD Connect [tutaj][aad-connect-download].

3. Uruchom Instalatora Azure AD Connect i przeprowadzić instalację za pomocą opcji *Dostosuj* .

    >[AZURE.NOTE] Nie można użyć opcji *Ustawienia ekspresowe* , ponieważ maszyn wirtualnych usługi synchronizacji Azure AD Connect hostingu nie należy do domeny.

4. Na stronie *Instalowanie wymagane składniki* pozostaw puste, aby zaakceptować domyślne opcje Ustawienia opcjonalnym.

5. Na stronie *logowania użytkownika* wybierz *Synchronizacji haseł*.

6. Na stronie *Nawiązywanie połączenia z Azure AD* wprowadź admin@ *myaadname*. onmicrosoft.com, gdzie *myaadname* to nazwa Twojej dzierżawy AAD. Wprowadź hasło do konta administratora.

7. Na stronie *Nawiązywanie połączenia z katalogów* określić contoso.com dla las (wpisz wartość, ponieważ nie jest on wyświetlany na liście rozwijanej), wprowadź CONTOSO\testuser dla nazwy użytkownika, określ AweS0me@PW hasło, a następnie kliknij przycisk *Dodaj katalog*.

8. Na stronie *Azure AD logowania Konfiguracja* zaakceptować wartość domyślną dla głównej nazwy użytkownika. Sprawdź *Kontynuuj bez żadnych zweryfikowanych domen*.

    >[AZURE.NOTE] Katalog contoso.com są wyświetlane jako *Niezweryfikowany*. Aby sprawdzić nazwę domeny, musisz utworzyć rekord TXT przez rejestratora nazw domen. W tym przykładzie domeny lokalnej nie jest zarejestrowany zewnętrznie. Aby uzyskać więcej informacji, zobacz [Dodawanie niestandardowej nazwy domeny do usługi Azure Active Directory][aad-custom-directory].

9. Na stronie *domeny i OU filtrowania* wybierz *Synchronizowanie wszystkich domen i organizacyjnych* (ustawienie domyślne).

10. Na stronie *jednoznacznie identyfikujący typ przepływu pracy użytkowników* zaakceptuj wartości domyślne.

11. Na stronie *Filtrowanie użytkowników i urządzeń* wybierz *Synchronizowanie wszystkich użytkowników i urządzeń* (ustawienie domyślne).

12. Na stronie *funkcje opcjonalne* wybierz *zapisu hasła*.

13. Na stronie *gotowy do skonfigurowania* zaznacz opcję *Rozpocznij proces synchronizacji po zakończeniu konfiguracji*, ale pozostawić, *Włącz tryb tymczasowy* usunięto zaznaczenie.

14. Sprawdź, że proces konfiguracji zostanie zakończony bez błędów, a następnie zamknij Instalatora.

15. Portal Azure Połącz się *Pomoc zdalna — aad — onpremise — adc — vm2* maszyn wirtualnych. Zaloguj się jako *testuser* przy użyciu hasła *AweS0me@PW*.

16. Pobierz narzędzie Azure AD Connect, a następnie uruchom Instalatora.

17. Kroków kreatora i odpowiadanie zgodnie z opisem w kroki od 3 do 12.

18. Na stronie *gotowy do skonfigurowania* wybierz *rozpocząć proces synchronizacji po zakończeniu konfiguracji*i **również zaznaczyć pole wyboru** *Włącz tryb tymczasowy*. Powoduje to narzędzie Azure AD Connect usługi synchronizacji pobrać szczegółowe informacje o kontach i obiektów z domeny contoso.com i przechowywać je w bazie danych, ale nie przekazują te szczegóły dzierżawy usługi AAD.

14. Sprawdź, że proces konfiguracji zostanie zakończony bez błędów, a następnie zamknij Instalatora.

### <a name="test-the-aad-configuration"></a>Przetestuj konfigurację AAD

1. Za pomocą portalu Azure, przejdź do katalogu AAD, otwórz karta usługi Azure Active Directory, kliknij pozycję *Użytkownicy i grupy*, a następnie kliknij *wszystkich użytkowników* , aby wyświetlić listę użytkowników i grup synchronizowane z katalogu. Powinien zostać wyświetlony użytkowników dla następujących kont:

    - Administrator (admin@ *myaadname*. onmicrosoft.com)

    - Konto usługi synchronizacji katalogu lokalnego (Sync_ADC1_*nnnnnnnnnnnn*@*myaadname*. onmicrosoft.com)

    - Konto usługi synchronizacji katalogu lokalnego (Sync_ADC2_*nnnnnnnnnnnn*@*myaadname*. onmicrosoft.com)

2. W portalu Azure przejdź do grupy zasobów, przytrzymując maszyny wirtualne dotyczące usług AD DS kontrolerów (*Pomoc zdalna aad-onpremise-rg* domyślnie) i nawiązać *Pomoc zdalna aad-onpremise-ad-vm1* maszyn wirtualnych. Zaloguj się jako *testuser* przy użyciu hasła *AweS0me@PW*.

3. Za pomocą konsoli *Użytkownicy usługi Active Directory i komputerach* , dodawanie nowego użytkownika domeny o nazwie Tibbot Adrianna o nazwie logowania dianet@contoso.com. Określ hasło wybranych przez użytkownika. Tworzenie użytkownika jest członkiem lokalnej grupy Administratorzy (dotyczy tylko, więc można zalogować się lokalnie jako temu użytkownikowi później — tylko komputery w domenie są DCs).

4. Przełączanie do *Pomoc zdalna aad-onpremise-adc-vm1* maszyn wirtualnych, Otwórz okno programu PowerShell i uruchom następujące polecenia:

        ```[powershell]
        Import-Module ADSync
        Start-ADSyncSyncCycle -PolicyType Delta
        ```

    Upewnij się, że polecenie zwraca *sukcesu*.

    >[AZURE.NOTE] Ten krok jest to konieczne, ponieważ domyślnie procesu synchronizacji jest zaplanowane do uruchomienia co 30 minut. Te polecenia wymusić synchronizację natychmiast.

5. Wróć do portalu Azure, przejdź do katalogu AAD, otwórz karta usługi Azure Active Directory, kliknij pozycję *Użytkownicy i grupy*, a następnie kliknij *wszystkich użytkowników*. Powinien zostać wyświetlony Tibbot Adrianna (dianet@ *myaadname*. onmicrosoft.com) są wyświetlane na liście użytkowników.

6. W karta usługi Azure Active Directory kliknij pozycję *Przedsiębiorstwo aplikacje*, a następnie kliknij *wszystkie aplikacje*. Kliknij aplikację, *ContosoWebApp1* , a następnie kliknij pozycję *Użytkownicy i grupy*. Na pasku narzędzi kliknij przycisk *Dodaj*. Kliknij pozycję *Użytkownicy i grupy*, a następnie wybierz *Tibbot Adrianna*. Kliknij przycisk *Przypisz*.

7. Przy użyciu programu Internet Explorer, przejdź do witryny https://account.activedirectory.windowsazure.com. Zaloguj się jako dianet@ *myaadname*. onmicrosoft.com przy użyciu określonej wcześniej hasła.

    >[AZURE.NOTE] Nie klikaj ikonę ContosoWebApp na liście aplikacji. AAD próbuje znaleźć aplikacji sieci web z adresem DNS publicznie wymienionych dla www.contoso.com, który różni się od adresu usługi równoważenia obciążenia warstwy sieci web.

8. Kliknij kartę *profilu* . Szczegóły użytkownika (Jeśli wybrano dowolny podczas jej tworzenia) powinny być wyświetlane.

    >[AZURE.NOTE] Kliknięcie przycisku *Zmień hasło*nie są dozwolone do wykonywania tego zadania, jak operacja musi być włączona przez administratora najpierw. Aby uzyskać więcej informacji, zobacz [Wprowadzenie do zarządzania hasłami][aad-password-management].

### <a name="enable-on-premises-users-to-run-the-application-by-using-authentication-through-aad"></a>Włączanie użytkowników lokalnych uruchomić aplikację za pomocą uwierzytelniania za pośrednictwem AAD

1. Wróć do kontrolera domeny *Pomoc zdalna aad-onpremise-ad-vm1* maszyn wirtualnych.

2. Otwieranie konsoli *Menedżer DNS* i Dodaj nowy rekord hosta dla www.contoso.com. Określ publiczny adres IP usługi równoważenia obciążenia warstwy sieci web.

3. Skopiuj plik *MyFakeRootCertificateAuthority.cer* na komputerze klienckim (utworzono to pliki w procedurze [konfiguracji Simulate publicznej witryny sieci web](#simulate-configuration-of-a-public-web-site)
    
4. Otwórz okno wiersza polecenia jako Administrator, przejdź do folderu, przytrzymując plik, który został właśnie skopiowany i uruchom następujące polecenie:

        ```
        certutil.exe -addstore Root MyFakeRootCertificateAuthority.cer
        ```

5. Przy użyciu programu Internet Explorer, przejdź do https://www.contoso.com. Upewnij się, że zostanie wyświetlona strona logowania AAD ContosoWebApp1 aplikacji sieci web.

6. Zaloguj się jako dianet@ *myaadname*. onmicrosoft.com. Uruchom aplikację i poprawnie Zaloguj się.

## <a name="next-steps"></a>Następne kroki

- Dowiedz się, najważniejsze wskazówki dotyczące [rozszerzenia domeny lokalnej DODAJE do Azure][adds-extend-domain]

- Dowiedz się, najważniejsze wskazówki dotyczące [tworzenia las zasobów DODAJE] [ adds-resource-forest] platformy Azure

<!-- links -->
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[script]: #sample-solution-script
[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-n-tier-vm.md
[active-directory-domain-services]: https://technet.microsoft.com/library/dd448614.aspx
[active-directory-federation-services]: https://technet.microsoft.com/windowsserver/dd448613.aspx
[azure-active-directory]: ../active-directory-domain-services/active-directory-ds-overview.md
[azure-ad-connect]: ../active-directory/active-directory-aadconnect.md
[ad-azure-guidelines]: https://msdn.microsoft.com/library/azure/jj156090.aspx
[aad-explained]: https://youtu.be/tj_0d4tR6aM
[aad-editions]: ../active-directory/active-directory-editions.md
[guidance-adds]: ./guidance-iaas-ra-secure-vnet-ad.md
[sla-aad]: https://azure.microsoft.com/support/legal/sla/active-directory/v1_0/
[azure-multifactor-authentication]: ../multi-factor-authentication/multi-factor-authentication.md
[aad-conditional-access]: ../active-directory//active-directory-conditional-access.md
[aad-dynamic-membership-rules]: ../active-directory/active-directory-accessmanagement-groups-with-advanced-rules.md
[aad-dynamic-memberships]: https://youtu.be/Tdiz2JqCl9Q
[aad-user-sign-in]: ../active-directory/active-directory-aadconnect-user-signin.md
[aad-sync-requirements]: ../active-directory/active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md
[aad-topologies]: ../active-directory/active-directory-aadconnect-topologies.md
[aad-filtering]: ../active-directory/active-directory-aadconnectsync-configure-filtering.md
[aad-scalability]: https://blogs.technet.microsoft.com/enterprisemobility/2014/09/02/azure-ad-under-the-hood-of-our-geo-redundant-highly-available-distributed-cloud-directory/
[aad-connect-sync-default-rules]: ../active-directory/active-directory-aadconnectsync-understanding-default-configuration.md
[aad-identity-protection]: ../active-directory/active-directory-identityprotection.md
[aad-password-management]: ../active-directory/active-directory-passwords-customize.md
[aad-application-proxy]: ../active-directory/active-directory-application-proxy-enable.md
[aad-connect-sync-operational-tasks]: ../active-directory/active-directory-aadconnectsync-operations.md#staging-mode
[aad-custom-domain]: ../active-directory/active-directory-add-domain.md
[aad-powershell]: https://msdn.microsoft.com/library/azure/mt757189.aspx
[aad-sync-disaster-recovery]: ../active-directory/active-directory-aadconnectsync-operations.md#disaster-recovery
[aad-sync-best-practices]: ../active-directory/active-directory-aadconnectsync-best-practices-changing-default-configuration.md
[aad-adfs]: ../active-directory/active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs
[aad-health]: ../active-directory/active-directory-aadconnect-health-sync.md
[aad-health-adds]: ../active-directory/active-directory-aadconnect-health-adds.md
[aad-health-adfs]: ../active-directory/active-directory-aadconnect-health-adfs.md
[aad-agent-installation]: ../active-directory/active-directory-aadconnect-health-agent-install.md
[aad-reporting-guide]: ../active-directory/active-directory-reporting-guide.md
[azure-cli]: ../virtual-machines-command-line-tools.md
[azure-powershell-download]: ../powershell-install-configure.md
[solution-script]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/Deploy-ReferenceArchitecture.ps1
[vnet-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/virtualNetwork.parameters.json
[nsg-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/networkSecurityGroups.parameters.json
[webtier-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/webTier.parameters.json
[businesstier-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/businessTier.parameters.json
[datatier-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/dataTier.parameters.json
[managementtier-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/managementTier.parameters.json
[makecert]: https://msdn.microsoft.com/library/windows/desktop/aa386968(v=vs.85).aspx
[add-adds-domain-controller-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/add-adds-domain-controller.parameters.json
[create-adds-forest-extension-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/create-adds-forest-extension.parameters.json
[virtualMachines-adds-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/virtualMachines-adds.parameters.json
[virtualNetwork-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/virtualNetwork.parameters.json
[virtualNetwork-adds-dns-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/virtualNetwork-adds-dns.parameters.json
[virtualMachines-adc-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/virtualMachines-adc.parameters.json
[virtualMachines-adc-joindomain-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/virtualMachines-adc-joindomain.parameters.json
[aad-connect-download]: http://www.microsoft.com/download/details.aspx?id=47594
[aad-custom-directory]: ../active-directory/active-directory-add-domain.md
[aad-password-management]: ../active-directory/active-directory-passwords-getting-started.md#enable-users-to-reset-their-azure-ad-passwords
[adds-extend-domain]: ./guidance-identity-adds-extend-domain.md
[adds-resource-forest]: ./guidance-identity-adds-resource-forest.md
[0]: ./media/guidance-identity-aad/figure1.png "Architektura tożsamości chmurze przy użyciu usługi Azure Active Directory"
[1]: ./media/guidance-identity-aad/figure2.png "Jeden las, pojedyncze AAD katalogu topologii"
[2]: ./media/guidance-identity-aad/figure3.png "Wiele lasów pojedynczy AAD katalogu topologii"
[4]: ./media/guidance-identity-aad/figure5.png "Tymczasowej topologii serwerów"
[5]: ./media/guidance-identity-aad/figure6.png "Wiele topologii katalogów AAD"
[6]: ./media/guidance-identity-aad/figure7.png "Wybieranie instalację niestandardową Azure AD łączenie synchronizacja z konkretnego wystąpienia programu SQL Server"
[7]: ./media/guidance-identity-aad/figure8.png "Określanie metody logowania jednokrotnego logowania użytkownika"
[8]: ./media/guidance-identity-aad/figure9.png "Określanie domeny i OU opcje filtrowania"
[9]: ./media/guidance-identity-aad/figure10.png "Włączanie hasła zapisu — Wstecz"
[10]: ./media/guidance-identity-aad/figure11.png "Karta Zarządzanie Azure AD w portalu"
[11]: ./media/guidance-identity-aad/figure12.png "Konsola Azure AD Connect"
[12]: ./media/guidance-identity-aad/figure13.png "Na karcie operacje w Menedżerze usługi synchronizacji"
[13]: ./media/guidance-identity-aad/figure14.png "Na karcie łączniki w Menedżerze usługi synchronizacji"
[14]: ./media/guidance-identity-aad/figure15.png "Edytor reguł synchronizacji"
[15]: ./media/guidance-identity-aad/figure16.png "Karta Azure Active Directory łączenie kondycji w portalu Azure przedstawiający kondycji synchronizacji"
[16]: ./media/guidance-identity-aad/figure17.png "Karta Azure Active Directory łączenie kondycji w portalu Azure przedstawiający kondycja usług AD DS"
[17]: ./media/guidance-identity-aad/figure18.png "Raporty dotyczące zabezpieczeń dostępne w portalu Azure"
[18]: ./media/guidance-identity-aad/figure19.png "Portal Azure, wyróżnianie publiczny adres IP usługi równoważenia obciążenia warstwy sieci web"
[19]: ./media/guidance-identity-aad/figure20.png "Przejdź do publiczny adres IP usługi równoważenia obciążenia warstwy sieci web przy użyciu programu Internet Explorer"
[20]: ./media/guidance-identity-aad/figure21.png "Przystawki Certyfikaty przedstawiający certyfikat www.contoso.com"
[21]: ./media/guidance-identity-aad/figure22.png "Nawiązywanie połączenia z poziomu zarządzania maszyn wirtualnych"
[22]: ./media/guidance-identity-aad/figure23.png "Tworzenie powiązania HTTPS domyślnej witryny sieci web"
[23]: ./media/guidance-identity-aad/figure24.png "Tworzenie aplikacji sieci web ContosoWebApp1"
[24]: ./media/guidance-identity-aad/figure25.png "Ustawianie właściwości uwierzytelniania aplikacji sieci web ContosoWebApp1"
[25]: ./media/guidance-identity-aad/figure26.png "Logowanie się do Azure AAD z ContosoWebApp1 aplikacji sieci web"
[26]: ./media/guidance-identity-aad/figure27.png "Zmienianie folderu domyślnej witryny sieci web"