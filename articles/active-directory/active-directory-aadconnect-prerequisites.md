<properties
   pageTitle="Narzędzie Azure AD Connect: Wymagania wstępne i sprzęt | Microsoft Azure"
   description="W tym temacie opisano wymagania wstępne i wymagania sprzętowe usługi Azure AD Connect"
   services="active-directory"
   documentationCenter=""
   authors="andkjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="10/12/2016"
   ms.author="billmath"/>

# <a name="prerequisites-for-azure-ad-connect"></a>Wymagania wstępne dotyczące Azure AD nawiązywanie połączenia
W tym temacie opisano wymagania wstępne i wymagania sprzętowe usługi Azure AD Connect.

## <a name="before-you-install-azure-ad-connect"></a>Przed rozpoczęciem instalacji narzędzie Azure AD Connect
Przed rozpoczęciem instalacji narzędzie Azure AD Connect, istnieje kilka rzeczy, które są potrzebne.

### <a name="azure-ad"></a>Azure AD
- Subskrypcję usługi Azure lub usługi [Azure subskrypcji wersji próbnej](https://azure.microsoft.com/pricing/free-trial/). Ta opcja jest wymagane do uzyskiwania dostępu do portalu Azure — nie za pomocą narzędzie Azure AD Connect. Jeśli korzystasz z programu PowerShell lub usługi Office 365 nie muszą Azure subskrypcję Azure AD Connect. Jeśli masz licencję usługi Office 365 można także korzystać z portalu usługi Office 365. Za pomocą płatnej licencji usługi Office 365 możesz również wyświetlić w Azure portal z portalu usługi Office 365.
    - Za pomocą funkcji Podgląd Azure AD w [Azure portal](https://portal.azure.com). Ten portal nie wymaga licencji usługi Azure.
- [Dodaj i sprawdź, czy domeny](active-directory-add-domain.md) ma zostać użyta w Azure AD. Na przykład jeśli zamierzasz użyć contoso.com dla użytkowników, a następnie upewnij się, ta domena została zweryfikowana i nie używasz tylko domenę domyślną contoso.onmicrosoft.com.
- Umożliwia dzierżawę Azure AD przez obiekty domyślnej 50k. Po weryfikacji domeny, limit zostaje zwiększona do 300 obiektów k. Jeśli potrzebujesz więcej obiektów w Azure AD, musisz otworzyć przypadek pomocy technicznej, aby limitu zwiększonym jeszcze bardziej. Jeśli potrzebujesz więcej niż 500 obiektów k potrzebujesz więcej licencji, takich jak usługi Office 365, Azure AD podstawowe, Azure AD Premium lub pakietu mobilności przedsiębiorstwa.

### <a name="prepare-your-on-premises-data"></a>Przygotowywanie danych lokalnych
- Przejrzyj [funkcji synchronizacji opcjonalne, które można włączyć w Azure AD](active-directory-aadconnectsyncservice-features.md) i ocena funkcji, które należy włączyć.

### <a name="on-premises-servers-and-environment"></a>Lokalne serwery i środowiska
- AD schematu wersji i las poziomu funkcjonalności musi być Windows Server 2003 lub nowszym. Jak są spełnione wymagania poziomu schematu i las kontrolerów może zostać uruchomiony dowolna wersja.
- Jeśli zamierzasz użyć funkcji **zapisu hasło** kontrolery domeny musi być w systemie Windows Server 2008 (z najnowszych SP) lub nowszym. W przypadku usługi DCs 2008 (sprzed R2) należy również zastosować [poprawkę KB2386717](http://support.microsoft.com/kb/2386717).
- Używane przez Azure AD kontrolera domeny musi być zapisywalny dysk. Nie jest obsługiwane umożliwia RODC (kontrolera domeny tylko do odczytu) i narzędzie Azure AD Connect wykonywania wszelkie przekierowania zapisu.
- Nie można zainstalować narzędzie Azure AD Connect na Small Business Server lub podstawowe informacje dotyczące systemu Windows Server. Serwer musi korzystać z systemu Windows Server standardowy lub lepiej.
- Serwer Azure AD Connect musi mieć pełny graficzny zainstalowany. Instalowanie na core serwera nie jest obsługiwana.
- Narzędzie Azure AD Connect musi być zainstalowany w systemie Windows Server 2008 lub nowszym. Ten serwer może być kontrolera domeny lub serwera członkowskiego przy użyciu ustawień express. Jeśli korzystasz z ustawieniami niestandardowymi, serwer może być również autonomiczne i nie musi być połączony z domeną.
- Po zainstalowaniu Azure AD Connect w systemie Windows Server 2008, upewnij się zastosować najnowszych poprawek z witryny Windows Update. Instalacja nie jest można uruchomić z serwerem bez poprawki.
- Jeśli zamierzasz użyć funkcji **synchronizacji haseł**serwer Azure AD Connect musi być w systemie Windows Server 2008 R2 z dodatkiem SP1 lub nowszym.
- Serwer Azure AD Connect musi mieć [.NET Framework 4.5.1](#component-prerequisites) lub nowszego i [Microsoft programu PowerShell 3.0](#component-prerequisites) lub nowszy.
- Jeśli wdrożeniu usług federacyjnych Active Directory serwery zainstalowanego usług AD FS lub serwera Proxy aplikacji sieci Web musi być Windows Server 2012 R2 lub nowszy. [Zdalne zarządzanie systemem Windows](#windows-remote-management) musi być włączona na tych serwerach instalacji zdalnej.
- Jeśli wdrażania usługi federacyjne Active Directory, należy [Certyfikatów SSL](#ssl-certificate-requirements).
- Jeśli wdrażania usługi federacyjne Active Directory, należy skonfigurować [Rozpoznawanie nazw](#name-resolution-for-federation-servers).
- Narzędzie Azure AD Connect wymaga bazy danych programu SQL Server do przechowywania danych tożsamości. Domyślnie jest zainstalowany program SQL Server 2012 Express LocalDB (uproszczonej wersji programu SQL Server Express) i konto usługi dla usługi jest tworzone na komputerze lokalnym. SQL Server Express ma limit rozmiaru 10GB, który umożliwia zarządzanie około 100 000 obiektów. Jeśli potrzebne do zarządzania większą ilość obiektów katalogu, należy wskazać polecenie Kreator instalacji innej instalacji programu SQL Server.
- Jeśli korzystasz z osobnych programu SQL Server, te wymagania stosowanie:
    - Narzędzie Azure AD Connect obsługuje wszystkie podtypy programu Microsoft SQL Server z programu SQL Server 2008 (z SP4) do programu SQL Server 2014. Microsoft Azure SQL Database nie jest **obsługiwane** jako bazy danych.
    - Należy użyć bez uwzględniania wielkości liter sortowania bazy danych SQL. Są one identyfikowane z \_CI_ w ich nazwy. Jest **nieobsługiwane** liter oznaczonej numerem \_CS_ w ich nazwy.
    - Może mieć tylko jeden aparat synchronizacji dla każdego wystąpienia bazy danych. Jest **nieobsługiwane** udostępnianie wystąpienie bazy danych za pomocą synchronizacji FIM-z wieloma Uczestnikami, DirSync lub Azure AD Sync.

### <a name="accounts"></a>Konta
- Konto administratora globalnego Azure AD dla katalogu Azure AD chcesz zintegrować z. Musi być **konta szkoły lub organizacji** , a nie może być **konta Microsoft**.
- Jeśli za pomocą ustawień ekspresowa lub uaktualnienie DirSync, musisz mieć konto administratora przedsiębiorstwa lokalną usługą Active Directory.
- [Konta w usłudze Active Directory](./connect/active-directory-aadconnect-accounts-permissions.md) , jeśli korzystasz z ustawieniami niestandardowymi ścieżka instalacji.

### <a name="azure-ad-connect-server-configuration"></a>Azure AD Connect konfiguracji serwera
- Jeśli administratorami globalnej MFA włączone, adres URL **https://secure.aadcdn.microsoftonline-p.com** musi być na liście zaufanych witryn. Zostanie wyświetlony monit o to Dodaj do listy zaufanych witryn, jeśli nie zostanie dodany, zanim zostanie wyświetlony monit o wezwanie MFA. Aby dodać go do listy zaufanych witryn, można użyć programu Internet Explorer.

### <a name="connectivity"></a>Łączność
- Serwer Azure AD Connect wymaga rozpoznawania nazw DNS dla sieci intranet i internet. Serwer DNS muszą mieć możliwość rozpoznawania nazw zarówno w usłudze Active Directory w lokalnej, a także Azure AD punktów końcowych.
- Jeśli masz zapory w intranecie i musisz otworzyć porty między serwerami Azure AD Connect i kontrolerach domeny, zobacz [Azure AD Connect porty](active-directory-aadconnect-ports.md) , aby uzyskać więcej informacji.
- Jeśli musi być otwarta serwera proxy lub zapory limit których może uzyskać dostęp adresy URL, adresy URL w [zakresy adresów IP i adresy URL usługi Office 365](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) .
    - Jeśli korzystasz z Cloud firmy Microsoft w Niemcy lub w chmurze dla instytucji rządowych Microsoft Azure, zobacz [Uwagi dotyczące wystąpienia usługi synchronizacji Azure AD Connect](active-directory-aadconnect-instances.md) dla adresów URL.
- Narzędzie Azure AD Connect jest domyślnie komunikowanie się za pomocą Azure AD przy użyciu TLS 1.0. Można zmienić tę na TLS 1.2, wykonując czynności opisane w [Włącz 1.2 TLS dla Azure AD Connect](#enable-tls-12-for-azure-ad-connect).
- Jeśli korzystasz z serwer proxy ruchu wychodzącego dla łączenia się z Internetem, następujące ustawienia w pliku **C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config** musi zostać dodana do Kreatora instalacji i Azure AD Connect synchronizacji móc połączyć się z Internetem i Azure AD. Ten tekst należy wprowadzać w dolnej części pliku. W tym kodzie &lt;PROXYADRESS&gt; reprezentuje proxy rzeczywisty adres IP lub host nazwę.

```
    <system.net>
        <defaultProxy>
            <proxy
            usesystemdefault="true"
            proxyaddress="http://<PROXYADDRESS>:<PROXYPORT>"
            bypassonlocal="true"
            />
        </defaultProxy>
    </system.net>
```

- Jeśli serwer proxy wymaga uwierzytelniania, [konta usługi](./connect/active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-accounts) musi znajdować się w domenie, a ścieżka instalacji dostosowane ustawienia należy użyć, aby określić [konto usługi niestandardowe](./connect/active-directory-aadconnect-get-started-custom.md#install-required-components). Należy również różne zmiany machine.config. Dzięki temu zmiany w pliku machine.config Kreatora instalacji i aparat synchronizacji odpowiadać na żądania uwierzytelniania serwera proxy. W Kreatorze instalacji wszystkich stron z wyjątkiem strony **Konfigurowanie** podpisanych w poświadczenia użytkownika są używane. Na stronie **Konfigurowanie** na końcu kreatora instalacji kontekst jest przełączenie do [konta usługi](./connect/active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-accounts) , które zostało utworzone przez Ciebie. W sekcji machine.config powinna wyglądać następująco.

```
    <system.net>
        <defaultProxy enabled="true" useDefaultCredentials="true">
            <proxy
            usesystemdefault="true"
            proxyaddress="http://<PROXYADDRESS>:<PROXYPORT>"
            bypassonlocal="true"
            />
        </defaultProxy>
    </system.net>
```

Zobacz MSDN, aby uzyskać więcej informacji o [domyślnym serwerze proxy elementu](https://msdn.microsoft.com/library/kd3cf2ex.aspx).

Jeśli masz problemy z łącznością, zobacz [Rozwiązywanie problemów z łącznością](active-directory-aadconnect-troubleshoot-connectivity.md).

### <a name="other"></a>Inne
- Opcjonalnie: Test konto użytkownika do weryfikacji synchronizacji.

## <a name="component-prerequisites"></a>Wymagania wstępne dotyczące składników

### <a name="powershell-and-net-framework"></a>Programu PowerShell i .net Framework
Narzędzie Azure AD Connect zależy od firmy Microsoft PowerShell i .NET Framework 4.5.1. Potrzebujesz tej wersji lub nowszy na serwerze. W zależności od wersji systemu Windows Server wykonaj następujące czynności:

- Windows Server 2012R2
  - Microsoft PowerShell jest zainstalowany domyślnie, jest wymagane żadne działanie.
  - .NET framework 4.5.1 i późniejszych wersjach są dostępne za pośrednictwem usługi Windows Update. Upewnij się, że zainstalowano najnowszych aktualizacji do systemu Windows Server w Panelu sterowania.
- Windows Server 2008R2 i Windows Server 2012
  - Najnowszą wersję programu PowerShell firmy Microsoft jest dostępna w systemie **Windows Management Framework 4.0**dostępne w [Centrum pobierania Microsoft](http://www.microsoft.com/downloads).
  - .NET framework 4.5.1 i późniejszych wersjach są dostępne w [Centrum pobierania Microsoft](http://www.microsoft.com/downloads).
- System Windows Server 2008
  - Najnowsza wersja obsługiwanej wersji programu PowerShell jest dostępna w **systemie Windows Management Framework 3.0**dostępny w [Centrum pobierania Microsoft](http://www.microsoft.com/downloads).
 - .NET framework 4.5.1 i późniejszych wersjach są dostępne w [Centrum pobierania Microsoft](http://www.microsoft.com/downloads).

### <a name="enable-tls-12-for-azure-ad-connect"></a>Włączyć TLS 1.2 dla Azure AD Connect
Narzędzie Azure AD Connect używa TLS 1.0 domyślnie do szyfrowania komunikacji między serwerem aparatu synchronizacji i Azure AD. Można to zmienić, konfigurując .net aplikacji do użytku TLS 1.2 domyślnie na serwerze. Więcej informacji na temat TLS 1.2 można znaleźć w [2960358 doradcze zabezpieczeń firmy Microsoft](https://technet.microsoft.com/security/advisory/2960358).

1. Nie można włączyć TLS 1.2 dla systemu Windows Server 2008. Potrzebujesz 2008R2 Windows Server lub nowszym. Upewnij się, masz po zainstalowaniu systemu operacyjnego poprawki .net 4.5.1, zobacz [2960358 doradcze zabezpieczeń firmy Microsoft](https://technet.microsoft.com/security/advisory/2960358). Może być to lub nowszej wersji już zainstalowane na serwerze.
2. Jeśli używasz systemu Windows Server 2008R2, upewnij się, że jest włączony TLS 1.2. Na serwerze Windows Server 2012 i nowsze wersje TLS 1.2 już powinna być włączona.
```
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2]
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
```
3. We wszystkich systemach operacyjnych Ustaw ten klucz rejestru i ponownie uruchom serwer.
```
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319
"SchUseStrongCrypto"=dword:00000001
```
4. Jeśli chcesz również włączyć TLS 1.2 między serwerem aparatu synchronizacji i zdalnego programu SQL Server, upewnij się, że zainstalowano wymagane wersje zainstalowane [TLS 1.2 obsługę programu Microsoft SQL Server](https://support.microsoft.com/kb/3135244).

## <a name="prerequisites-for-federation-installation-and-configuration"></a>Wymagania wstępne dotyczące Federacji instalacja i Konfiguracja

### <a name="windows-remote-management"></a>Zdalne zarządzanie systemem Windows
Wdrażanie usług federacyjnych Active Directory lub serwera Proxy aplikacji sieci Web za pomocą narzędzie Azure AD Connect, sprawdź wymagania poniżej, aby upewnić się, że powiedzie łączności i konfiguracji:

- Jeśli serwer docelowy jest domeną sprzężone, upewnij się, że zdalnego zarządzania systemu Windows jest włączona
    - W oknie polecenia podwyższonym poziomem Psh — za pomocą polecenia`Enable-PSRemoting –force`
- Jeśli na docelowym serwerze jest wartością domeny przyłączonym komputerze WAP, istnieje kilka dodatkowych wymagań
    - Na komputerze docelowym (WAP komputer):
         - Upewnij się, winrm (zdalnego zarządzania systemem Windows i zdalnym) jest uruchomiona za pomocą przystawki usług
         - W oknie polecenia podwyższonym poziomem Psh — za pomocą polecenia`Enable-PSRemoting –force`
    - Na komputerze, na którym działa Kreator (jeśli komputerze docelowym jest domeny połączonych lub niezaufanych nienależących do domeny):
        - W oknie polecenia podwyższonym poziomem Psh — za pomocą polecenia`Set-Item WSMan:\localhost\Client\TrustedHosts –Value <DMZServerFQDN> -Force –Concatenate`
        - W Menedżerze serwera:
             - Dodaj hosta WAP strefy Zdemilitaryzowanej do puli komputera (Menedżer serwera -> Zarządzaj... -> Dodawanie serwerów za pomocą karty DNS)
             - Karta Menedżera serwera wszystkie serwery: kliknij prawym przyciskiem myszy serwer WAP i wybierz pozycję Zarządzaj jako..., wprowadź poświadczenia lokalnych (nie domeny) na komputerze WAP
             - Aby sprawdzić poprawność połączenia Psh — zdalnego, na karcie Menedżer serwera wszystkie serwery: kliknij prawym przyciskiem myszy serwer WAP i wybierz pozycję programu Windows PowerShell.  Zdalnej sesji Psh — należy otworzyć, aby upewnić się, że można ustalić zdalnej sesji programu PowerShell.

### <a name="ssl-certificate-requirements"></a>Wymagania dotyczące certyfikatu SSL
**Ważne:** Stanowczo zalecamy używanie tego samego certyfikatu SSL w wszystkie węzły farmy usług AD FS, a także wszystkich serwerów proxy aplikacji sieci Web.

- Certyfikat musi być X509 certyfikatu.
- Za pomocą certyfikatu z podpisem własnym na serwerach federacyjnych w testowych. Dla środowisku produkcyjnym, zalecamy uzyskać certyfikat z publicznego urzędu certyfikacji.
    - Jeśli przy użyciu certyfikatu, który nie jest publicznym zaufaniem, upewnij się, że certyfikat zainstalowany na poszczególnych serwerach Proxy aplikacji sieci Web jest zaufany na serwerze lokalnym i na wszystkich serwerach federacyjnych
- Tożsamość certyfikat musi odpowiadać nazwie usługi federacyjne (na przykład sts.contoso.com).
    - Tożsamość jest albo temat alternatywny (SAN) rozszerzenie Nazwa_dns typu lub, jeśli nie ma żadnych wpisów SAN, nazwę podmiotu określona jako nazwa wspólna.  
    - Wiele wpisów SAN może znajdować się w certyfikatu, pod warunkiem jeden z nich jest zgodna z nazwą usługi federacyjne.
    - Jeśli planujesz używać dołączanie do obszaru roboczego, dodatkowe SAN jest wymagane z wartością **enterpriseregistration.** Po sufiksu głównej nazwy użytkownika (UPN) organizacji, na przykład **enterpriseregistration.contoso.com**.
- Certyfikaty na podstawie CryptoAPI następnej generacji (CNG) klawiszy i dostawców magazynów kluczy nie są obsługiwane. Oznacza to, że należy użyć certyfikatu na podstawie dostawcy (usługodawca kryptograficzny) i nie KSP (Dostawca magazynu kluczy).
- Certyfikaty przy użyciu symboli wieloznacznych są obsługiwane.

### <a name="name-resolution-for-federation-servers"></a>Rozpoznawanie nazw dla serwerów federacyjnych
- Skonfiguruj rekordy DNS dla usług AD FS nazwa usługi federacyjne (np. sts.contoso.com) dla sieci intranet (wewnętrznego serwera DNS) i ekstranetu (publicznej DNS za pomocą rejestratora domen). Dla sieci intranet DNS rekord upewnij się, że używasz A rekordów i nie rekordów CNAME. Jest to wymagane na potrzeby uwierzytelniania systemu windows do działa prawidłowo z komputera sprzężonych domeny.
- Wdrażając więcej niż jeden serwer usług AD FS lub serwera Proxy aplikacji sieci Web, następnie upewnij się, które skonfigurowano usługi równoważenia obciążenia i że rekordy DNS dla nazwy usługi federacyjne usług AD FS (na przykład sts.contoso.com) odwołują się do równoważenia obciążenia.
- Dla zintegrowanego uwierzytelniania systemu windows do pracy aplikacji przeglądarki przy użyciu programu Internet Explorer w sieci intranet upewnij się, że nazwa usługi federacyjne usług AD FS (na przykład sts.contoso.com) jest dodawana do strefy intranet w programie Internet Explorer. Można to kontrolowane przez zasady grupy i używany na wszystkich komputerach domeny sprzężone.

## <a name="azure-ad-connect-supporting-components"></a>Narzędzie Azure AD Connect pomocniczych składników
Poniżej przedstawiono listę składników instalowane narzędzie Azure AD Connect na serwerze, na którym zainstalowano narzędzie Azure AD Connect. Ta lista jest dla podstawowe Instalacja ekspresowa.  Jeśli wybierzesz na stronie Instalowanie synchronizacji usług za pomocą innego programu SQL Server, następnie SQL Express LocalDB nie jest zainstalowany lokalnie.

- Azure AD Connect kondycji
- Microsoft Online Services Asystenta logowania w dla informatyków (zainstalowany, ale nie zależność)
- Narzędzia wiersza polecenia programu Microsoft SQL Server 2012
- Microsoft SQL Server 2012 Express LocalDB
- Microsoft SQL Server 2012 Native Client
- Microsoft Visual C++ 2013 ponownej dystrybucji pakietu

## <a name="hardware-requirements-for-azure-ad-connect"></a>Wymagania sprzętowe usługi Azure AD Connect
Poniższa tabela zawiera minimalne wymagania dotyczące systemu Azure AD Connect synchronizacji.

| Liczba obiektów w usłudze Active Directory | PROCESOR | Pamięci | Rozmiar dysku twardego |
| ------------------------------------- | --- | ------ | --------------- |
| Mniej niż 10 000 | 1,6 GHz | 4 GB | 70 GB |
| 10 000-50 000 | 1,6 GHz | 4 GB | 70 GB |
| 50 000-100 000 | 1,6 GHz | 16 GB | 100 GB |
| Dla 100 000 lub więcej obiektów jest wymagane pełną wersję programu SQL Server|  |  |  |
| 100000 — 300 000 | 1,6 GHz | 32 GB | 300 GB |
| 300 000 — 600,000 | 1,6 GHz | 32 GB | 450 GB |
| Więcej niż 600,000 | 1,6 GHz | 32 GB | 500 GB |

Minimalne wymagania dotyczące komputerów z systemem usług AD FS lub serwerów aplikacji sieci Web jest następująca:

- Procesor: Dwa podstawowe 1,6 GHz lub wyższej
- PAMIĘCI: 2GB lub nowszy
- Azure maszyn wirtualnych: A2 konfiguracji lub nowszy

## <a name="next-steps"></a>Następne kroki
Dowiedz się więcej o [integrowaniu lokalnego tożsamości z usługą Azure Active Directory](active-directory-aadconnect.md).
