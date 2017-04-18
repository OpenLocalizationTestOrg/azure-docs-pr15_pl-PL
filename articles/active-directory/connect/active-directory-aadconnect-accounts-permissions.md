<properties
   pageTitle="Narzędzie Azure AD Connect: Kontach i uprawnieniach | Microsoft Azure"
   description="W tym temacie opisano konta używane i utworzone i wymagane uprawnienia."
   services="active-directory"
   documentationCenter=""
   authors="billmath"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"  
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="10/04/2016"
   ms.author="billmath"/>


# <a name="azure-ad-connect-accounts-and-permissions"></a>Narzędzie Azure AD Connect: Kontach i uprawnieniach
Kreatora instalacji Azure AD Connect oferuje dwie ścieżki innego:

- W obszarze Ustawienia Express kreator wymaga więcej uprawnień tak, aby je łatwo skonfigurować konfiguracji bez konieczności tworzenie użytkowników i konfigurowanie uprawnień oddzielnie.

- W niestandardowych ustawień kreatora oferuje więcej opcji i opcji, ale istnieje kilka sytuacji, w których należy upewnić się, że użytkownik ma odpowiednie uprawnienia.

## <a name="related-documentation"></a>Dokumentacja pokrewne
Jeśli nie przeczytaj dokumentację dotyczącą [Integrowanie tożsamości lokalnych z usługą Azure Active Directory](../active-directory-aadconnect.md), Poniższa tabela zawiera łącza do powiązanych tematów.

Temat |  
--------- | ---------
Instalowanie przy użyciu ustawień Express | [Instalacja ekspresowa Azure AD Connect](active-directory-aadconnect-get-started-express.md)
Instalowanie przy użyciu ustawień niestandardowe | [Instalacja niestandardowa: narzędzie Azure AD Connect](active-directory-aadconnect-get-started-custom.md)
Uaktualnienie DirSync | [Uaktualnienie narzędzie do synchronizacji usługi Azure AD (DirSync)](active-directory-aadconnect-dirsync-upgrade-get-started.md)


## <a name="express-settings-installation"></a>Ustawienia Instalacja ekspresowa
W obszarze Ustawienia Express Kreatora instalacji monituje o podanie poświadczeń AD DS Enterprise administratora, usługi Active Directory w lokalnej można skonfigurować z uprawnieniami wymaganymi dla Azure AD Connect. Jeśli przechodzisz z DirSync, poświadczenia Administratorzy przedsiębiorstwa usług AD DS są używane do resetowania hasła dla konta używane przez usługę DirSync. Należy również poświadczenia administratora globalnego Azure AD.

Strona kreatora  | Poświadczenia zbierane | Wymagane uprawnienia| Na potrzeby
------------- | ------------- |------------- |-------------
N/D!|Uruchamianie Kreatora instalacji użytkownika| Administrator serwera lokalnego| <li>Tworzy lokalne konta, które jest używane jako [konto usługi aparatu synchronizacji](#azure-ad-connect-sync-service-account).
Nawiązywanie połączenia z Azure AD| Azure AD katalogu poświadczeń | Rola administratora globalnego w Azure AD | <li>Włączanie synchronizacji w katalogu Azure AD.</li>  <li>Tworzenie [konta Azure AD](#azure-ad-service-account) jest używana w operacji na rozpoczęcie synchronizacji w Azure AD.</li>
Nawiązywanie połączenia z usług AD DS | Poświadczenia usługi Active Directory w lokalnej | Członek grupy administratorów przedsiębiorstwa (EA) w usłudze Active Directory| <li>Umożliwia utworzenie [konta](#active-directory-account) w usłudze Active Directory i udziela uprawnienia do niego. Utworzono konto służy do odczytu i zapisu katalogu informacji podczas synchronizacji.</li>

### <a name="enterprise-admin-credentials"></a>Poświadczenia administratora przedsiębiorstwa
Te poświadczenia są używane tylko podczas instalacji i są używane po zakończeniu instalacji. Jest administrator przedsiębiorstwa, a nie administratora domeny, aby upewnić się, że można ustawić uprawnienia w usłudze Active Directory we wszystkich domenach.

### <a name="global-admin-credentials"></a>Poświadczenia administratora globalnego
Te poświadczenia są używane tylko podczas instalacji i nie są używane po zakończeniu instalacji. Służy do tworzenia [konta Azure AD](#azure-ad-service-account) używane do synchronizacji zmian Azure AD. Konto umożliwia synchronizowanie jako funkcji w Azure AD.

### <a name="permissions-for-the-created-ad-ds-account-for-express-settings"></a>Uprawnienia dla utworzonych usług AD DS uwzględniania Ustawienia ekspresowe
[Konto](#active-directory-account) utworzone do czytania i pisania w usługach AD DS mieć następujące uprawnienia podczas tworzenia express ustawienia:

Uprawnień | Na potrzeby
---- | ----
<li>Powielić zmiany katalogu</li><li>Replikacja katalogu spowoduje zmianę wszystkich | Synchronizacji haseł
Odczytu/zapisu wszystkich właściwości użytkownika | Importowanie i Exchange hybrydowego
Odczytu/zapisu wszystkich iNetOrgPerson właściwości | Importowanie i Exchange hybrydowego
Grupowanie wszystkich właściwości odczytu/zapisu | Importowanie i Exchange hybrydowego
Skontaktuj się z wszystkich właściwości odczytu/zapisu | Importowanie i Exchange hybrydowego
Resetowanie hasła | Przygotowanie do włączania zapisu hasłem

## <a name="custom-settings-installation"></a>Instalacja ustawień niestandardowych
Gdy przy użyciu niestandardowych ustawień, należy utworzyć konto używane do łączenia się z usługą Active Directory przed instalacją. Tego konta musi udzielić uprawnień można znaleźć w [Tworzenie konta usług AD DS](#create-the-ad-ds-account).

Strona kreatora  | Poświadczenia zebrane | Wymagane uprawnienia| Na potrzeby
------------- | ------------- |------------- |-------------
N/D! | Uruchamianie Kreatora instalacji użytkownika|<li>Administrator serwera lokalnego</li><li>Jeśli przy użyciu pełnego programu SQL Server, użytkownik musi być administratora systemu (SA) w języku SQL</li>| Domyślnie tworzy lokalne konta, które jest używane jako [konto usługi aparatu synchronizacji](#azure-ad-connect-sync-service-account). Konto jest tworzony tylko w sytuacji, gdy administrator nie określi określonego konta.
Instalowanie usługi synchronizacji, opcja Konto usługi | AD lub poświadczenia konta użytkowników lokalnych | Użytkownik, uprawnienia są udzielane przez Kreatora instalacji | Jeśli administrator określa konta, to konto jest używane jako konto usługi dla usługi synchronizacji.
Nawiązywanie połączenia z Azure AD | Azure AD katalogu poświadczeń| Rola administratora globalnego w Azure AD| <li>Włączanie synchronizacji w katalogu Azure AD.</li>  <li>Tworzenie [konta Azure AD](#azure-ad-service-account) jest używana w operacji na rozpoczęcie synchronizacji w Azure AD.</li>
Nawiązywanie połączenia z katalogów | Poświadczenia usługi Active Directory w lokalnej dla każdego las podłączonego do Azure AD | Uprawnienia są zależne od funkcji, które można włączyć i znajduje się na [koncie tworzenie AD DS](#create-the-ad-ds-account) |To konto jest używane do odczytu i zapisu katalogu informacji podczas synchronizacji.
Serwery AD FS | Dla każdego serwera na liście Kreator zbiera poświadczenia podczas logowania użytkownika kreatora są niewystarczające, aby połączyć | Administrator domeny | Instalowanie i konfigurowanie roli serwera usług AD FS.
Serwerów proxy aplikacji sieci Web |Dla każdego serwera na liście Kreator zbiera poświadczenia podczas logowania użytkownika kreatora są niewystarczające, aby połączyć | Lokalne administratora na komputerze docelowym | Instalowanie i konfigurowanie roli serwera WAP.
Poświadczenia konta zaufania serwera proxy |Poświadczenia zaufania usługi federacyjne (poświadczenia serwera proxy używa rejestracji certyfikatu zaufania za pomocą FS |Konta domeny, która jest administrator lokalny serwer usług AD FS | Rejestrowanie początkowe FS WAP zaufania certyfikatu.
AD FS konta usługi dla strony, "Użyj opcja konto użytkownika domeny" | Poświadczenia konta użytkownika AD | Użytkownik domeny | AD konto użytkownika, którego poświadczenia są dostarczane jest używany jako konto logowania usługi usług AD FS.

### <a name="create-the-ad-ds-account"></a>Tworzenie konta usług AD DS
Po zainstalowaniu Azure AD Connect konta, które można określić na stronie **Nawiązywanie połączenia z katalogów** musi znajdować się w usłudze Active Directory i masz wymagane uprawnienia. Kreator instalacji nie Sprawdź, czy uprawnienia i wszelkie problemy mogą znajdować się tylko podczas synchronizacji.

Uprawnienia, które jest wymagane w zależności od opcjonalne funkcje można włączyć. Jeśli masz wiele domen, muszą mieć uprawnienia dla wszystkich domen w las. Jeśli nie włączysz dowolnej z tych funkcji, wystarczające są domyślne uprawnienia **Użytkownika domeny** .

Funkcja | Uprawnienia
------ | ------
Synchronizacji haseł | <li>Powielić zmiany katalogu</li>  <li>Replikacja katalogu spowoduje zmianę wszystkich
Wdrożenie hybrydowe programu Exchange | Uprawnienia do zapisu atrybuty opisane w [zapisu hybrydowe programu Exchange](../active-directory-aadconnectsync-attributes-synchronized.md#exchange-hybrid-writeback) do użytkowników, grupy i kontakty.
Hasło wystornowaniem | Uprawnienia do zapisu atrybuty opisane w [Wprowadzenie do zarządzania hasłami](../active-directory-passwords-getting-started.md#step-4-set-up-the-appropriate-active-directory-permissions) do użytkowników.
Urządzenie wystornowaniem | Uprawnienia za pomocą skryptu programu PowerShell zgodnie z opisem w [zapisu urządzenia](../active-directory-aadconnect-feature-device-writeback.md).
Grupa wystornowaniem | Czytanie, tworzenie, aktualizowanie i usuwanie Grupuj obiekty w jednostce Organizacyjnej, gdzie powinien znajdować się grupy dystrybucyjne.

## <a name="upgrade"></a>Uaktualnianie
Po uaktualnieniu z jednej wersji Azure AD Connect do nowej wersji, potrzebne są następujące uprawnienia:

Główne | Wymagane uprawnienia | Na potrzeby
---- | ---- | ----
Uruchamianie Kreatora instalacji użytkownika | Administrator serwera lokalnego | Aktualizowanie plików binarnych.
Uruchamianie Kreatora instalacji użytkownika | Członek ADSyncAdmins | Wprowadź zmiany zasad synchronizacji i innych konfiguracji.
Uruchamianie Kreatora instalacji użytkownika | Jeśli korzystasz z pełną programu SQL server: DBO (lub podobnych) Aparat bazy danych synchronizacji | Wprowadź zmiany poziomu bazy danych, takich jak aktualizowanie tabel przy użyciu nowych kolumn.

## <a name="more-about-the-created-accounts"></a>Więcej informacji na temat utworzonych kont

### <a name="active-directory-account"></a>Konta Active Directory
Użycie opcji Ustawienia ekspresowe, konto jest tworzone w usłudze Active Directory, która służy do synchronizacji. Utworzone konto znajduje się w domeny głównej w kontenerze użytkowników i zawiera nazwy poprzedzony **MSOL_**. Konto zostanie utworzona i długi złożone hasło, które nie wygaśnie. Jeśli zasady haseł w Twojej domenie, upewnij się, że długie i czy jest dozwolony złożone hasła dla tego konta.

![Konta AD](./media/active-directory-aadconnect-accounts-permissions/adsyncserviceaccount.png)

### <a name="azure-ad-connect-sync-service-accounts"></a>Konta usługi synchronizacji usługi Azure AD Connect
Konto usługi lokalnej jest tworzone przez Kreatora instalacji (chyba że określone konto do użycia w ustawień niestandardowych). Konto jest prefiksem **AAD_** i używane przez usługę synchronizacji rzeczywisty do uruchamiania jako. Jeśli zainstalowano narzędzie Azure AD Connect na kontrolerze domeny, konto zostanie utworzone w domenie. Jeśli używasz na serwerze zdalnym z programem SQL server lub jeśli korzystasz z serwera proxy, który wymaga uwierzytelniania, konta usługi **AAD_** musi znajdować się w tej domenie.

![Konto usługi synchronizacji](./media/active-directory-aadconnect-accounts-permissions/syncserviceaccount.png)

Konto zostanie utworzona i długi złożone hasło, które nie wygaśnie.

To konto jest używane do przechowywania haseł do innych kont w bezpieczny sposób. Te hasła konta są przechowywane zaszyfrowane w bazie danych. Kluczy prywatnych kluczy szyfrowania są chronione za pomocą szyfrowania hasła klucza szyfrowania usługi przy użyciu systemu Windows danych ochrony interfejsu API (DPAPI). Nie należy resetować hasła dla konta usługi, ponieważ Windows zostanie następnie destroy kluczy szyfrowania ze względów bezpieczeństwa.

Użycie pełnego programu SQL Server, konto usługi jest DBO utworzonego bazy danych aparatu synchronizacji. Usługa nie będzie działać zgodnie z oczekiwaniami z inne uprawnienia. Logowanie SQL również zostanie utworzona.

Konto jest również udzielić uprawnień do plików, kluczy rejestru i innych obiektów związanych z aparatu synchronizacji.

### <a name="azure-ad-service-account"></a>Konta usługi Azure AD
Utworzono konto w Azure AD dla używania usługi synchronizacji. Według nazwy wyświetlanej można określić tego konta.

![Konta AD](./media/active-directory-aadconnect-accounts-permissions/aadsyncserviceaccount.png)

W drugiej części nazwę użytkownika można określić nazwę serwera, który ma zostać użyte na. Na obrazie nazwa serwera to FABRIKAMCON. Jeśli masz tymczasowej serwerów, każdy serwer ma własny rachunek. Azure AD jest limit 10 kont usługi synchronizacji.

Konto usługi jest tworzone długo złożone hasła, które nie wygaśnie. Udzielono specjalne uprawnienia **Konta synchronizacji katalogów** z uprawnieniami tylko do wykonywania zadania synchronizacji katalogów. Nie można udzielić tej roli wbudowanego specjalne spoza kreatora Azure AD Connect i Azure portal pokazuje tego konta z roli **użytkownika**.

![Konta AD roli](./media/active-directory-aadconnect-accounts-permissions/aadsyncserviceaccountrole.png)

## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej o [integrowaniu lokalnego tożsamości z usługą Azure Active Directory](../active-directory-aadconnect.md).
