<properties
    pageTitle="Narzędzie Azure AD Connect: Obsługiwanych topologii | Microsoft Azure"
    description="W tym temacie opisano obsługiwanych i nieobsługiwanych topologii dla Azure AD Connect"
    services="active-directory"
    documentationCenter=""
    authors="AndKjell"
    manager="femila"
    editor=""/>
<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="billmath"/>

# <a name="topologies-for-azure-ad-connect"></a>Łączenie topologii dla Azure AD
Celem w tym temacie jest do opisu różnych lokalnych i topologii Azure AD dotyczące synchronizacji Azure AD Connect jako rozwiązanie integracji klucza. Opisuje zarówno obsługiwanych i nieobsługiwanych konfiguracji.

Legenda obrazów w dokumencie:

Opis | Ikona
-----|-----
Las usługi Active Directory w lokalnej| ![AD](./media/active-directory-aadconnect-topologies/LegendAD1.png)
Usługi Active Directory z filtrowanych importu| ![AD](./media/active-directory-aadconnect-topologies/LegendAD2.png)
Serwer synchronizacji usługi Azure AD Connect| ![Synchronizowanie](./media/active-directory-aadconnect-topologies/LegendSync1.png)
Azure AD Connect synchronizacji "tymczasowego tryb serwera"| ![Synchronizowanie](./media/active-directory-aadconnect-topologies/LegendSync2.png)
GALSync FIM2010 lub MIM2016| ![Synchronizowanie](./media/active-directory-aadconnect-topologies/LegendSync3.png)
Serwer synchronizacji usługi Azure AD Connect, szczegółowe| ![Synchronizowanie](./media/active-directory-aadconnect-topologies/LegendSync4.png)
Azure AD katalogu |![AAD](./media/active-directory-aadconnect-topologies/LegendAAD.png)
Nieobsługiwane scenariusz | ![Nieobsługiwane](./media/active-directory-aadconnect-topologies/LegendUnsupported.png)

## <a name="single-forest-single-azure-ad-tenant"></a>Jeden las, pojedyncze dzierżawy Azure AD
![Pojedynczy dzierżawy jeden las](./media/active-directory-aadconnect-topologies/SingleForestSingleDirectory.png)

Topologia najczęściej jest jeden las dzierżawa lokalnego, z domenami jednej lub wielu i pojedynczy Azure AD. W przypadku uwierzytelniania Azure AD służy synchronizacji haseł. Instalacja ekspresowa Azure AD Connect obsługuje tylko tej topologii.

### <a name="single-forest-multiple-sync-servers-to-one-azure-ad-tenant"></a>Jeden las wiele serwerów synchronizacji do jednej dzierżawy Azure AD
![Nieobsługiwane filtrowane jeden las](./media/active-directory-aadconnect-topologies/SingleForestFilteredUnsupported.png)

Wiele serwerów Synchronizuj narzędzie Azure AD Connect podłączony do tej samej dzierżawy Azure AD, z wyjątkiem [przemieszczenia serwera](#staging-server)nie jest obsługiwana. Jest nieobsługiwane, nawet jeśli te są skonfigurowane do synchronizacji zestaw wzajemnie wykluczających się obiektów. Możesz może mieć traktowane jako to jeśli nie można uzyskać dostęp do wszystkich domen w las z jednego serwera lub do dystrybucji obciążenia przez kilka serwera.

## <a name="multiple-forests-single-azure-ad-tenant"></a>Wiele lasów pojedynczy dzierżawy Azure AD
![Wiele las jednej dzierżawy](./media/active-directory-aadconnect-topologies/MultiForestSingleDirectory.png)

Wiele organizacji mają środowiskach z wieloma lasami usługi Active Directory w lokalnej. Istnieją różne przyczyny mających więcej niż jeden las usługi Active Directory w lokalnej. Typowe przykłady są projekty z lasami konta zasobów i jako wynik po fuzji lub przejęcia.

Jeśli używasz wielu lasów, wszystkie lasów musi być dostępne przy jednym Azure AD Connect server synchronizacji. Nie musisz dołączyć serwer do domeny. W razie potrzeby do osiągnięcia las wszystkie serwera może znajdować się w sieci strefy Zdemilitaryzowanej.

Kreator instalacji narzędzie Azure AD Connect oferuje kilka opcji do skonsolidowania użytkowników reprezentowany w wielu lasach. Celem jest to, że użytkownik reprezentowany jest tylko raz w Azure AD. Istnieje kilka typowych topologii skonfigurowane w ścieżce instalacji niestandardowej w Kreatorze instalacji. Wybierz odpowiednią opcję odpowiadającą topologii na stronie **jednoznacznie identyfikujący typ przepływu pracy użytkowników**. Konsolidacji jest skonfigurowane tylko dla użytkowników. Zduplikowany grupy nie są skonsolidowane o domyślnej konfiguracji.

W następnej sekcji omówiono typowe topologii: [osobnych topologii](#multiple-forests-separate-topologies), [Pełna sieć](#multiple-forests-full-mesh-with-optional-galsync)i [Konta zasobów](#multiple-forests-account-resource-forest).

Domyślna konfiguracja w synchronizacji Azure AD Connect zakłada:

1. Użytkownicy mają tylko jedną włączonym kontem i las, w którym znajduje się to konto jest używane do uwierzytelnienia użytkownika. Jest to założenie dla obu synchronizacji haseł i federacji. UserPrincipalName i sourceAnchor-immutableID pochodzą z ten las.
2. Użytkownicy mają tylko jedna skrzynka pocztowa.
3. Las, który udostępnia skrzynkę pocztową do użytkownika ma najlepszej jakości danych atrybutów widoczne na globalnej liście adresowej (GAL) programu Exchange. Jeśli użytkownik jest nie skrzynki pocztowej, następnie wszelkie las może służyć do współtworzenia wartości tych atrybutów.
4. Jeśli masz połączonych skrzynki pocztowej, występuje również inne konto w różnych las używane do zalogowania się.

Jeśli środowiska jest niezgodny z tych założeń, są następujące akcje:

- Jeśli masz więcej niż jedno konto aktywne lub więcej niż jedna skrzynka pocztowa aparatu synchronizacji wybiera jedną i ignorowanie drugi.
- Skrzynki pocztowej połączonych za pomocą innego konta aktywnego nie są eksportowane do Azure AD. Konto użytkownika nie jest wyświetlana jako członek w dowolnej grupie. Zawsze jest przedstawiany skrzynki pocztowej połączonego w DirSync jako normalny skrzynki pocztowej. Ta zmiana celowo jest różny sposób, aby lepiej obsługuje wiele las scenariuszy.

Więcej informacji można znaleźć w [Opis konfiguracyjny domyślne](active-directory-aadconnectsync-understanding-default-configuration.md).

### <a name="multiple-forests-multiple-sync-servers-to-one-azure-ad-tenant"></a>Wiele lasów, wiele serwerów synchronizacji do jednej dzierżawy Azure AD
![Synchronizowanie wielu las wielokrotne nieobsługiwane](./media/active-directory-aadconnect-topologies/MultiForestMultiSyncUnsupported.png)

Nie jest obsługiwane ma więcej niż jeden serwer Azure AD Sync łączenie połączony z jednym Azure AD dzierżawy. Wyjątkiem jest korzystania z [przemieszczenia serwera](#staging-server).

### <a name="multiple-forests--separate-topologies"></a>Wiele lasów — osobnym topologii
**Użytkownicy są reprezentowany tylko raz przez katalogów**

![Raz las wielu użytkowników](./media/active-directory-aadconnect-topologies/MultiForestUsersOnce.png)

![Wiele las rozdzielonych topologii](./media/active-directory-aadconnect-topologies/MultiForestSeperateTopologies.png)

W tym środowisku wszystkich lasów lokalnego są traktowane jako odrębne osoby i żaden użytkownik nie może znajdować się w innych las.
Każdy las ma własnej organizacji programu Exchange, a istnieje nie GALSync między lasy. Ta topologia może być sytuację po łączenia i przejęcia lub w miejsce, w którym działa poszczególnych jednostek biznesowych organizacji odizolowane od siebie. Lasów są w tej samej organizacji w Azure AD i są wyświetlane z ujednolicony GAL.
Na tym obrazie każdego obiektu w każdy las jest reprezentowane raz w metaverse i zagregowane w dzierżawie docelowej Azure AD.

### <a name="multiple-forests--match-users"></a>Wiele lasów — dopasowanie użytkowników
**Tożsamość użytkownika istnieje w wielu katalogów**

Wspólne dla tych scenariuszy jest dystrybucyjnej i grupy zabezpieczeń mogą zawierać użytkowników, kontaktów i FSP (obcych podmiotów zabezpieczeń)

FSP są używane w DODAWANA reprezentujące członków z innych lasów w grupie zabezpieczeń. Wszystkie FSP są kojarzone rzeczywistego obiektu w Azure AD.

### <a name="multiple-forests--full-mesh-with-optional-galsync"></a>Wiele lasów — Pełna sieć z GALSync opcjonalne
**Istnieje tożsamości użytkowników w wielu katalogów. Uwzględnij przy użyciu: atrybut poczty**

![Las wielu użytkowników poczty](./media/active-directory-aadconnect-topologies/MultiForestUsersMail.png)

![Pełna sieć wielokrotne las](./media/active-directory-aadconnect-topologies/MultiForestFullMesh.png)

Topologia pełnej sieci umożliwia użytkownikami i zasobami znajdować się w dowolnym las i często może istnieć dwukierunkowe zaufanie między lasami.

Jeśli Exchange znajduje się w więcej niż jeden las, można opcjonalnie być lokalnego rozwiązania GALSync. Każdy użytkownik jest przedstawiany jako kontakt w innych lasach. GALSync często jest zaimplementowana przy użyciu Forefront Identity Manager 2010 lub Microsoft Identity Manager 2016. Narzędzie Azure AD Connect nie można używać dla lokalnego GALSync.

W tym scenariuszu tożsamości obiektów sprzężone przy użyciu atrybutu poczty. Użytkownik mający skrzynki pocztowej w jeden las jest połączony z kontaktami w innych lasach.

### <a name="multiple-forests--account-resource-forest"></a>Wiele lasów — las konta zasobów
**Istnieje tożsamości użytkowników w wielu katalogów. Uwzględnij przy użyciu: atrybuty ObjectSID i msExchMasterAccountSID**

![Las wielu użytkowników ObjectSID](./media/active-directory-aadconnect-topologies/MultiForestUsersObjectSID.png)

![Wiele las AccountResource](./media/active-directory-aadconnect-topologies/MultiForestAccountResource.png)

W topologii las konta zasobów masz jeden lub więcej lasy konta z kontami aktywnego użytkownika. Masz również jednym lub kilku lasach zasobów z kontami wyłączony.

W tym scenariuszu wszystkie **konta lasów**zaufania jednej (lub więcej) **las zasobów** . Las zasób ma zwykle rozszerzonego schematu AD przy użyciu programu Exchange i programu Lync. Wszystkie usługi programu Exchange i programu Lync, a także innych usług udostępnionych znajdują się w tym las. Użytkownicy mają wyłączone konto użytkownika, w tym las i Skrzynka pocztowa jest połączony z las konta.

## <a name="office-365-and-topology-considerations"></a>Usługa Office 365 i zagadnienia dotyczące topologii
Niektóre obciążeń pracą usługi Office 365, trzeba pewne ograniczenia obsługiwanych topologii. Jeśli zamierzasz użyć dowolnej z tych następnie przeczytaj temat obsługiwanych topologii obciążenie pracą.

Obciążenie pracą |  
--------- | ---------
Usługa Exchange Online | Jeśli istnieje więcej niż jedna Exchange organizacji lokalnej (czyli Exchange zostało rozmieszczone w więcej niż jeden las), należy użyć programu Exchange 2013 z dodatkiem SP1 lub nowszym. Szczegóły można znaleźć tutaj: [we wdrożeniach hybrydowych z wielu lasów usługi Active Directory](https://technet.microsoft.com/library/jj873754.aspx)
Program Skype dla firm | Jeśli używasz wielu lasów lokalnego topologia las konta zasobu jest obsługiwana. Szczegóły dla obsługiwanych topologii można znaleźć tutaj: [wymagania dotyczące programu Skype dla firm Server 2015 ochrony środowiska](https://technet.microsoft.com/library/dn933910.aspx)

## <a name="staging-server"></a>Organizowanie serwera
![Organizowanie serwera](./media/active-directory-aadconnect-topologies/MultiForestStaging.png)

Narzędzie Azure AD Connect obsługuje instalowanie drugi serwer w **trybie tymczasowego**. Serwera w tym trybie odczytuje dane ze wszystkich połączonych katalogów, ale nie zapisuje żadnych do połączonego katalogów. Korzysta z cyklu synchronizacji normalnym i w związku z tym ma zaktualizowaną kopię danych tożsamości. W danych, w którym podstawowy serwer nie możesz może nie na serwerze. Można to zrobić w Kreatorze Azure AD Connect. Ten drugi serwer najlepiej mogą znajdować się w innym centrum danych, ponieważ nie infrastruktury są udostępniane na serwerze podstawowym. Należy ręcznie skopiować wszelkie konfiguracji zmiany na serwerze podstawowego drugi serwer.

Aby przetestować nową konfigurację niestandardowych i efekt, który zawiera dane również można tymczasowy serwera. Można wyświetlić podgląd zmian i dostosowanie konfiguracji. Po zakończeniu edycji nowej konfiguracji, możesz wprowadzić serwerze active server i ustawić stare ASP w trybie tymczasowej.

Ta metoda umożliwia również zamienić synchronizowanie active server. Przygotowywanie nowego serwera i ustaw ją w trybie tymczasowej. Upewnij się, jest dobry stan, wyłącz tymczasowej tryb (uaktywnienie go) i zamknąć Aktywne serwera.

Użytkownik może mieć więcej niż jeden serwer tymczasową, jeśli chcesz wykonać wiele kopii zapasowych w centrach danych innym.

## <a name="multiple-azure-ad-tenants"></a>Kilka dzierżaw Azure AD
Firma Microsoft zaleca mających jednej dzierżawy w Azure AD dla organizacji.
Przed ma zostać użyta kilka dzierżaw Azure AD, poniższe tematy dotyczą typowych scenariuszy, co pozwala na korzystanie z jednej dzierżawy.

Temat |  
--------- | ---------
Delegowanie przy użyciu jednostek administracyjnych | [Zarządzanie jednostek administracyjnych w Azure AD](active-directory-administrative-units-management.md)

![Wiele las wielokrotne dzierżawy](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectory.png)

Istnieje powiązanie 1:1 serwer Azure AD Connect synchronizacją z dzierżawą usługi Azure AD. Dla każdego dzierżawy Azure AD konieczne jedną instalację serwera synchronizacji Azure AD Connect. Wystąpienia dzierżawy Azure AD są zamierzone samodzielnie, a użytkownicy w jednym nie widzą użytkownicy w innej dzierżawie. Jeśli jest przeznaczony ten odstęp, a następnie jest obsługiwana konfiguracja, ale w przeciwnym razie należy używać pojedynczy model dzierżawy Azure AD.

### <a name="each-object-only-once-in-an-azure-ad-tenant"></a>Każdy obiekt tylko raz w dzierżawie usługi Azure AD
![Jeden las filtrowane](./media/active-directory-aadconnect-topologies/SingleForestFiltered.png)

W tej topologii jeden Azure AD Connect synchronizacją serwer jest podłączony do każdego dzierżawy Azure AD. Serwery synchronizacji Azure AD Connect należy skonfigurować dla potrzeb filtrowania, więc każdy zestaw wzajemnie wykluczających obiektów do pracy na. Można na przykład ograniczyć każdego serwera do określonej domeny lub jednostkę Organizacyjną. Domeny DNS może być rejestrowane tylko w jednym Azure AD dzierżawy. UPN użytkowników w wersji lokalnej AD należy użyć także osobnych obszarów nazw. Na przykład na obrazie powyżej trzech oddzielnych UPN sufiksy są rejestrowane w lokalnym programie AD: contoso.com, fabrikam.com i nadrzędnych. Użytkownicy w każdy hasło lokalnego za pomocą różnych nazw.

Nie ma żadnych GALsync między wystąpieniami dzierżawy Azure AD. Książka adresowa w usłudze Exchange Online i programu Skype dla firm tylko użytkownicy pokazuje w tej samej dzierżawy.

Ta topologia występują następujące ograniczenia w inny sposób obsługiwane scenariusze:

- Tylko jeden z dzierżawami Azure AD umożliwiają wdrożenia hybrydowe programu Exchange z usługą Active Directory w lokalnej.
- Urządzeń z systemem Windows 10 może być skojarzony tylko z jednej dzierżawy Azure AD.

Wymagania wzajemnie wykluczających się zestawu obiektów dotyczy również wystornowania. Niektóre funkcje zapisu nie są obsługiwane przy tym topologii, ponieważ te funkcje założono konfigurację jednej lokalnego:

-   Grupa zapisu z domyślnej konfiguracji
-   Urządzenie wystornowaniem

### <a name="each-object-multiple-times-in-an-azure-ad-tenant"></a>Każdy obiekt wiele razy w dzierżawie usługi Azure AD
![Nieobsługiwane dzierżawy wielokrotne jeden las](./media/active-directory-aadconnect-topologies/SingleForestMultiDirectoryUnsupported.png) ![Nieobsługiwane jeden las wiele łączników](./media/active-directory-aadconnect-topologies/SingleForestMultiConnectorsUnsupported.png)

- Go nie jest obsługiwana zsynchronizować użytkownik będzie mógł kilka dzierżaw Azure AD.
- Jest nieobsługiwane, aby wprowadzić zmianę, aby pokazać użytkownikom w jednym Azure AD są wyświetlane jako kontaktów w innej dzierżawie Azure AD konfiguracji.
- Go nie jest obsługiwana osobom modyfikowanie Azure AD Connect synchronizacją nawiązywania połączenia z wieloma dzierżawami Azure AD.

### <a name="galsync-by-using-writeback"></a>GALsync przy użyciu zapisu
![MultiForestMultiDirectoryGALSync1Unsupported](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectoryGALSync1Unsupported.png) ![MultiForestMultiDirectoryGALSync2Unsupported](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectoryGALSync2Unsupported.png)

Azure AD dzierżaw są zamierzone samodzielnie.

- Go nie jest obsługiwana w celu zmiany konfiguracji synchronizacji Azure AD Connect do odczytywania danych z fuzji Azure AD.
- Jest obsługiwana do wyeksportowania użytkowników jako kontaktów do innego lokalnego AD przy użyciu Azure AD Connect synchronizacji.

### <a name="galsync-with-on-premises-sync-server"></a>GALsync z lokalnego serwera synchronizacji
![MultiForestMultiDirectoryGALSync](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectoryGALSync.png)

Jest obsługiwana umożliwia FIM2010-MIM2016 lokalnego użytkownikom GALsync między dwoma organizacji programu Exchange. Użytkownicy w organizacji jednego wyświetlane jako obcego użytkowników i kontaktów z innych organizacji. Te reklam różnych lokalnych następnie mogą być synchronizowane z dzierżawami własne Azure AD.

## <a name="next-steps"></a>Następne kroki
Aby dowiedzieć się, jak zainstalować narzędzie Azure AD Connect w następujących scenariuszach, zobacz [Instalacja niestandardowa: narzędzie Azure AD Connect](./connect/active-directory-aadconnect-get-started-custom.md).

Dowiedz się więcej na temat konfiguracji [synchronizacji Azure AD Connect](active-directory-aadconnectsync-whatis.md) .

Dowiedz się więcej o [integrowaniu lokalnego tożsamości z usługą Azure Active Directory](active-directory-aadconnect.md).
