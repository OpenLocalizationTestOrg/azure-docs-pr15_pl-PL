<properties
   pageTitle="Azure Active Directory inspekcji zgłaszania zdarzeń | Microsoft Azure"
   description="Inspekcji zdarzeń, które są dostępne do przeglądania i pobierania z usługi Azure Active Directory"
   services="active-directory"
   documentationCenter=""
   authors="dhanyahk"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/19/2016"
   ms.author="dhanyahk"/>

# <a name="azure-active-directory-audit-report-events"></a>Zdarzenia raportu inspekcji w usłudze Azure Active Directory

*Niniejsza dokumentacja jest częścią [Azure Active Directory raportowania przewodnika] (aktywne — katalogu raportowania guide.md).*

Azure Active Directory raportu inspekcji pomagają zidentyfikować uprzywilejowanych akcje, które wystąpiły podczas ich usługi Azure Active Directory. Akcje uprzywilejowanych zawierać zmiany podniesienie uprawnień (na przykład tworzenie roli lub resetowanie hasła), zmienianie konfiguracji zasad (na przykład zasady haseł) lub zmiany w konfiguracji katalogu (na przykład zmiany ustawień Federacja domen). Raporty zapewniają rekordu inspekcji nazwę zdarzenia, aktor, która wykonała akcję zasobu docelowego wpływu zmiany oraz Data i godzina (w czasie UTC). Klienci będą mogli pobrać listę inspekcji zdarzeń dla ich Azure Active Directory za pośrednictwem [Azure Portal](https://portal.azure.com/), zgodnie z opisem w [widoku dzienników inspekcji](active-directory-reporting-azure-portal.md).


## <a name="list-of-audit-report-events"></a>Lista zdarzeń raportu inspekcji
<!--- audit event descriptions should be in the past tense --->

Zdarzenia                               | Opis zdarzenia
------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------
**Zdarzenia użytkownika**                      |
Dodawanie użytkownika                             | Dodać użytkownika do katalogu.
Usuwanie użytkownika                          | Usunąć użytkownika z katalogu.
Ustawianie właściwości licencji               | Ustawianie właściwości licencji do użytkownika w katalogu.
Resetowanie hasła użytkownika                  | Resetowanie hasła użytkownika w katalogu.
Zmienianie hasła użytkownika                 | Zmienić hasło dla użytkownika w katalogu.
Zmienianie licencji użytkownika                  | Zmienić licencji przypisanej do użytkownika w katalogu. Aby sprawdzić, jakie licencje zostały zaktualizowane, spójrz na poniżej właściwości [użytkownika aktualizacji](#update-user-attributes)
Aktualizuj użytkownika                          | Zaktualizowane użytkownika w katalogu. [Zobacz poniższą sekcję](#update-user-attributes) dla atrybuty, które można aktualizować.
Ustawianie życie Zmienianie hasła użytkownika       | Należy ustawić właściwość, który wymusza użytkownik może zmienić swoje hasło podczas logowania.
Aktualizowanie poświadczeń użytkownika        | Użytkownik zmienił hasło  
**Zdarzenia grupy**                     |
Dodawanie grupy                            | Utworzyć grupy w katalogu.
Aktualizacja grupy                         | Zaktualizowane grupy w katalogu. Aby zobaczyć, jakie właściwości grupy zostały zaktualizowane, zapoznaj się z [Grupy właściwości inspekcji](#update-group-attributes) w sekcji poniżej
Usuwanie grupy                         | Usunięte grupy z katalogu.
Dodawanie członka do grupy            | Dodać członka do grupy w katalogu.
Usuwanie członka z grupy         | Usunąć członka z grupy w katalogu.
CreateGroupSettings              | Ustawienia grupy utworzone
UpdateGroupSettings          | Zaktualizowane ustawienia grupy. Aby zobaczyć, jakie ustawienia grupy zostały zaktualizowane, zapoznaj się z [Grupy właściwości inspekcji](#update-group-attributes) w sekcji poniżej
DeleteGroupSettings            | Ustawienia grupy usunięte
SetGroupLicense                  | Ustawianie licencji grupy.
SetGroupManagedBy              | Ustawienia grupy do zarządzania przez użytkownika
AddGroupMember               | Dodano element członkowski do grupy
RemoveGroupMember            | Usuwanie członka z grupy
AddGroupOwner                | Dodano właściciela grupy
RemoveGroupOwner                 | Usunięto właściciel z grupy
**Zdarzenia aplikacji**               |
Dodawanie wystawcy usługi                | Wystawcy usługi dodawane do katalogu.
Usuwanie wystawcy usługi             | Usunięte wystawcy usługi z katalogu.
Dodaj poświadczenia głównej usługi    | Dodano poświadczeń w celu wystawcy usługi.
Usuwanie poświadczeń głównej usługi | Usunięto poświadczenia z głównej usługi.
Dodawanie pozycji delegowania                 | Utworzono [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permissiongrant-entity) w katalogu.
Ustawianie Delegowanie wpisu                 | Zaktualizowane [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permissiongrant-entity) w katalogu.
Usuwanie wpisu delegowania              | Usunięte [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permissiongrant-entity) w katalogu.
**Rola zdarzenia**                      |
Dodawanie roli członka do roli              | Dodać użytkownika do roli katalogu.
Usuwanie roli członka z roli         | Usunąć użytkownika z roli katalogu.
Ustawianie informacje kontaktowe firmy      | Ustawianie preferencji dotyczących kontaktu poziomie firmy. Ta opcja uwzględnia adresów e-mail dla powiadomienia marketingowych, a także techniczne dotyczące usług Microsoft Online Services.
Dodawanie pozycji delegowania                 | Utworzono [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#OAuth2PermissionGrantEntity) w katalogu.
Ustawianie Delegowanie wpisu                 | Zaktualizowane [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#OAuth2PermissionGrantEntity) w katalogu.
Usuwanie wpisu delegowania              | Usunięte [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#OAuth2PermissionGrantEntity) w katalogu.
AddSevicePrincipalOwner          | Dodano właściciela wystawcy usługi.
RemoveSevicePrincipalOwner   | Właściciel usunięta z wystawcy usługi.
AddApplication  | Dodawanie aplikacji.
UpdateApplication | Aktualizowanie aplikacji. Aby zobaczyć, jakie ustawienia aplikacji zostały zaktualizowane, zapoznaj się z [Aplikacji właściwości inspekcji](#update-application-attributes) w sekcji poniżej
DeleteApplication | Usuwanie aplikacji.
RestoreApplication|Przywracanie aplikacji.
AddApplicationOwner   |     Dodawanie właściciela do aplikacji.
RemoveApplicationOwner    |     Usuwanie właściciela z aplikacji.
**Rola zdarzenia**                         |
Dodawanie roli członka do roli                 | Dodać użytkownika do roli katalogu.
Usuwanie roli członka z roli            | Usunąć użytkownika z roli katalogu.
AddRoleDefinition               | Dodano rolę.
UpdateRoleDefinition                | Aktualizacja definicji roli. Aby zobaczyć, jakie ustawienia roli zostały zaktualizowane, odwołują się do [Roli definicji właściwości inspekcji](#update-role-definition-attributes) w sekcji poniżej
DeleteRoleDefinition                | Usunięte definicji roli.
AddRoleAssignmentToRoleDefinition | Przypisanie roli dodane do definicji roli.
RemoveRoleAssignmentFromRoleDefinition | Przypisanie roli usunięte z definicji roli.
AddRoleFromTemplate     | Dodano rola z szablonu.
UpdateRole  | Rola zaktualizowane.
AddRoleScopeMemberToRole    | Dodano zakresie element członkowski do roli.
RemoveRoleScopedMemberFromRole  | Usunąć zakresu członka z roli.
**Zdarzenia urządzenia (wszystkie zdarzenia)**                    |
AddDevice               | Dodane urządzenie.
UpdateDevice            | Zaktualizowane urządzenie. Aby zobaczyć, jakie właściwości urządzenia zostały zaktualizowane, należy zapoznać się z [Właściwości urządzenia Audited](#update-device-attributes) w poniższej sekcji
DeleteDevice            | Usunięte urządzenie.
AddDeviceConfiguration      | Konfiguracja dodane urządzenie.
UpdateDeviceConfiguration   | Konfiguracja urządzenia aktualizacją. Aby zobaczyć, jakie właściwości konfiguracji urządzenia zostały zaktualizowane, należy zapoznać się z [Właściwości konfiguracji urządzenia Audited](#update-device-configuration-attributes) w poniższej sekcji
DeleteDeviceConfiguration   | Konfiguracja urządzenia usunięte.
AddRegisteredOwner     | Dodano właścicielem do urządzenia.
AddRegisteredUsers     | Dodane zarejestrowanych użytkowników do urządzenia.
RemoveRegisteredOwner    | Usuwanie właścicielem z urządzenia.
RemoveRegisteredUsers    | Usuwanie zarejestrowanych użytkowników z urządzenia.
RemoveDeviceCredentials  | Usuwanie poświadczeń urządzenia.
**Zdarzenia B2B**                       |
Przekazane partii zaproszenia.              | Administrator ma przekazać plik zawierający zaproszeń wysyłanych do użytkowników partnera.
Zaproszenia partii przetwarzania.             | Plik zawierający zaproszeniami dla użytkowników partnerów została przetworzona.
Zapraszanie użytkowników zewnętrznych.                | Użytkownik zewnętrzny zostały zaproszone do katalogu.
Zrealizuj zapraszanie użytkowników zewnętrznych.         | Użytkownik zewnętrzny ma zaginął ich zaproszenie do katalogu.
Dodawanie użytkowników zewnętrznych do grupy.          | Użytkownik zewnętrzny przypisano członkostwa do grupy w katalogu.
Przypisywanie użytkowników zewnętrznych do aplikacji. | Użytkownik zewnętrzny przypisano bezpośredni dostęp do aplikacji.
Tworzenie wirusa dzierżawy.               | Realizacji zaproszenia utworzono nowej dzierżawy w Azure AD.
Tworzenie wirusa użytkownika.                 | Użytkownik utworzono w istniejącej dzierżawy w Azure AD przy wykupu zaproszenie.
**Jednostki administracyjne (wszystkie zdarzenia)**             |
AddAdministrativeUnit   |   Dodawanie jednostki administracyjnej.
UpdateAdministrativeUnit|   Aktualizowanie jednostki administracyjnej. Aby sprawdzić, jakie właściwości jednostki administracyjnej zostały zaktualizowane, odwołują się do [właściwości jednostki administracyjne inspekcji](#update-administrative-unit-attributes) w sekcji poniżej
DeleteAdministrativeUnit|   Usuwanie jednostki administracyjnej.
AddMemberToAdministrativeUnit|  Dodawanie członka do jednostki administracyjnej.
RemoveMemberFromAdministrativeUnit|     Usuwanie członka z jednostki administracyjnej.
**Katalog zdarzenia**                 |
Dodawanie partnera firmy               | Dodany partnera do katalogu.
Usuwanie partnera firmy          | Partner usunięta z katalogu.
DemotePartner   |   Obniżenie partnera.
Dodawanie domeny firmy                | Dodać domenę do katalogu.
Usuwanie domeny firmy           | Domena usunięta z katalogu.
Aktualizowanie domeny                        | Zaktualizowane domeny w katalogu. Aby sprawdzić, jakie właściwości domeny zostały zaktualizowane, odwołują się do [Właściwości domeny inspekcji](#update-domain-attributes) w sekcji poniżej
Konfigurowanie uwierzytelniania domeny            | Zmienić domyślne ustawienie domeny firmy.
Ustawianie informacje kontaktowe firmy      | Ustawianie preferencji dotyczących kontaktu poziomie firmy. Ta opcja uwzględnia adresów e-mail dla powiadomienia marketingowych, a także techniczne dotyczące usług Microsoft Online Services.
Konfigurowanie ustawień federacji w domenie    | Zaktualizowane ustawienia Federacji dla domeny.
Weryfikowanie domeny                        | Zweryfikować domenę w katalogu.
Sprawdź zweryfikowaną domeną poczty e-mail         | Zweryfikować domenę w katalogu za pomocą poczty e-mail weryfikacji.
Ustawianie flagi DirSyncEnabled w firmie   | Należy ustawić właściwość, która umożliwia katalogu dla Azure AD Sync.
Ustawianie zasad hasła                  | Ustawianie długości i znak ograniczenia haseł użytkowników.
Informacje o firmie zestawu              | Zaktualizowane informacje o poziomie firmy. Zobacz polecenia cmdlet [Get-MsolCompanyInformation](https://msdn.microsoft.com/library/azure/dn194126.aspx) programu PowerShell, aby uzyskać więcej informacji.
SetCompanyAllowedDataLocation  |    Ustawianie firmy dozwolone lokalizacja danych.
SetCompanyDirSyncEnabled    |   Należy ustawić flagę DirSyncEnabled.
SetCompanyDirSyncFeature |  Funkcja DirSync zestawu.
SetCompanyInformation |   Informacje o firmie zestawu.
SetCompanyMultiNationalEnabled    |     Ustawianie włączoną funkcję międzynarodowej firmy.
SetDirectoryFeatureOnTenant   |     Ustawianie funkcja katalogów w dzierżawie.
SetTenantLicenseProperties  |   Ustawianie właściwości licencji dzierżawy.
CreateCompanySettings   |   Tworzenie ustawienia w firmie
UpdateCompanySettings   |   Aktualizowanie ustawień firmy. Aby sprawdzić, jakie właściwości firmy zostały zaktualizowane, odwołują się do [Właściwości firmy inspekcji](#update-company-attributes) w sekcji poniżej
DeleteCompanySettings   |   Usuń ustawienia firmy
SetAccidentalDeletionThreshold   |  Należy ustawić próg przypadkowym usunięciom.
SetRightsManagementProperties   |   Ustaw właściwości zarządzania prawami.
PurgeRightsManagementProperties     |   Czyszczenie właściwości zarządzania prawami.
UpdateExternalSecrets   |   Aktualizowanie hasła zewnętrznych
**Zdarzenia zasad (wszystkie zdarzenia)**           |
AddPolicy   |   Dodawanie zasad.
UpdatePolicy    |   Aktualizowanie zasad.
DeletePolicy    |   Usuwanie zasad.
AddDefaultPolicyApplication     |   Dodawanie zasad do aplikacji.
AddDefaultPolicyServicePrincipal    |   Dodawanie zasad do wystawcy usługi.
RemoveDefaultPolicyApplication  |   Usuwanie zasad z aplikacji.
RemoveDefaultPolicyServicePrincipal     |   Usuwanie zasad z wystawcy usługi.
RemovePolicyCredentials     |   Usuwanie poświadczeń zasad.

## <a name="audit-report-retention"></a>Przechowywanie raportu inspekcji
Zdarzenia w raporcie Azure AD inspekcji są zachowywane przez 180 dni. Aby uzyskać więcej informacji na temat przechowywania w raportach zobacz [Zasady przechowywania raportu Active Directory Azure](active-directory-reporting-retention.md).

W przypadku klientów do przechowywania ich wydarzenia inspekcji przez dłuższy czas przechowywania API raportowania może służyć do regularnie wciąganie inspekcji zdarzeń do sklepu osobnych danych. Aby uzyskać szczegółowe informacje, zobacz [Wprowadzenie do interfejsu API raportów](active-directory-reporting-api-getting-started.md) .

## <a name="properties-included-with-each-audit-event"></a>Właściwości dołączone do każdego zdarzenia inspekcji

Właściwość      | Opis
------------- | --------------------------------------------------------------
Data i godzina | Data i godzina wystąpienia zdarzenia inspekcji
Aktor         | Użytkownika lub kapitału usługa, która wykonała akcję
Akcja        | Akcję, która została wykonana
Docelowy        | Użytkownika lub wystawcy usługi, wykonanej akcji


## <a name="update-user-attributes"></a>Atrybuty "Aktualizuj użytkownika"
Zdarzenia inspekcji "Aktualizacja użytkownika" zawiera dodatkowe informacje na temat jakie atrybuty użytkownika zostały zaktualizowane. Dla każdego atrybutu zarówno wartość poprzednia i nowa wartość jest dołączany.

Atrybut                           | Opis
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AccountEnabled                      | Użytkownik może Zaloguj się.
AssignedLicense                     | Wszystkie licencje, które zostały przypisane do użytkownika.
AssignedPlan                        | Plany usługi powstałe w wyniku licencji przypisana do użytkownika.
LicenseAssignmentDetail             | Szczegółowe informacje o licencji przypisana do użytkownika. Na przykład jeśli uczestniczyły oparte na grupach Licencjonowanie to dotyczy grupę, do której udzielił licencji.
Telefon komórkowy                              | Telefon komórkowy użytkownika.
OtherMail                           | Adres alternatywny adres e-mail użytkownika.
OtherMobile                         | Użytkownika dodatkowego telefonu komórkowego.
StrongAuthenticationMethod          | Lista metod weryfikacji skonfigurowany przez użytkownika uwierzytelniania wieloskładnikowego, takich jak kod połączeń głosowych, SMS lub weryfikacji z aplikacji dla urządzeń przenośnych.
StrongAuthenticationRequirement     | Jeśli uwierzytelnianie wieloskładnikowe jest wymuszane włączone lub wyłączone dla tego użytkownika.
StrongAuthenticationUserDetails     | Numer telefonu użytkownika, alternatywny telefonu i adresu e-mail na potrzeby weryfikacji Resetowanie uwierzytelnianie wieloskładnikowe i hasło.
StrongAuthenticationPhoneAppDetail  | Zarejestrowany do wykonywania 2FA logowania w aplikacjach dla szczegółów forof telefonu
TelephoneNumber                     | Numer telefonu użytkownika.
AlternativeSecurityId               | Identyfikator alternatywnych zabezpieczeń obiektu.
CreationType                        |Metoda tworzenia użytkownika (lub przez zaproszenie wirusa).
InviteTicket                        |Lista biletów zaproszenia dla użytkownika.
InviteReplyUrl                      |Lista adresów URL, aby odpowiedzieć na akceptację zaproszenia.
InviteResources                     |Lista zasobów, do których zostały zaproszone użytkownika.
LastDirSyncTime                     |Godzina ostatniej obiektu została zaktualizowana ze względu na synchronizowanie z autorytatywne katalogu (nabywca, lokalna).
MSExchRemoteRecipientType           |Mapy do MSO typów adresatów. Zapoznaj się z https://msdn.microsoft.com/library/microsoft.office.interop.outlook.recipient.type.aspx [MSO adresatów typy] typu adresatów
PreferredDataLocation               |Preferowany lokalizacji dla użytkownika, grupy, kontaktu, folder publiczny lub dane urządzenia.
ProxyAddresses                      |Adres, według którego obiektu adresatów programu Exchange Server został rozpoznany w systemie poczty obcego.
StsRefreshTokensValidFrom           |Powoduje odświeżanie informacji o odwołaniu token — wszystkie tokeny odświeżania STS wydane przed tym razem są traktowane jako wygasła.
UserPrincipalName                   |Nazwa UPN jest styl Internet nazwa logowania użytkownika.
UserState                           |Stan użytkownika oczekuje na zatwierdzenie PendingAcceptance zaakceptowane PendingVerification.
UserStateChangedOn                  |Sygnatura czasowa ostatniej zmiany UserState. Umożliwia wykonywanie przepływów pracy cyklu życia.
UserType                            |Typ użytkownika. Element członkowski (0), Gość (1), Viral(2).

## <a name="update-group-attributes"></a>Atrybuty "Grupę update"
Atrybut                           | Opis
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
Klasyfikacja            | Klasyfikacja dla grupy ujednolicony (HBI, MBI itp.).
Opis               | Zrozumiałą wyrażeń opisowa o obiekcie.  
DisplayName               |Nazwa wyświetlana obiektu
DirSyncEnabled            |Wskazuje, czy synchronizacja jest przeprowadzana z autorytatywny katalogu (nabywca, lokalna).
GroupLicenseAssignment    |Przydział licencji dla grupy
GroupType                 |Typ grupy, Unified (0)
IsMembershipRuleLocked    |Wskazuje, że właściwość MembershipRule jest ustawiona przez usługę zarządzania Samoobsługowe grupy i nie mogą być zmieniane przez użytkowników. Dotyczy tylko grup, którym GroupType zawiera GroupType.DynamicMembership
IsPublic                  |Flaga wskazująca, jeśli grupa jest publiczna/prywatna.
LastDirSyncTime           |Godzina ostatniej obiekt został zaktualizowane podczas synchronizowania z autorytatywne katalogu (lokalnego klienta).
Poczta                      |Podstawowy adres e-mail
MailEnabled               |Wskazuje, czy ta grupa ma obsługi poczty e-mail.
MailNickname              |Moniker obiektu książki adresowej, zwykle część nazwy e-mail poprzednim "@" symbol.   
MembershipRule            |Ciąg wyrażenia kryteria używane przez usługę zarządzania Samoobsługowe grupy do określenia, które elementy członkowskie powinna należeć do tej grupy. Zobacz też IsMembershipRuleLocked. Dotyczy tylko grup, którym GroupType zawiera GroupType.DynamicMembership.
MembershipRuleProcessingState| Wartość wyliczenia definiowane przez usługę Zarządzanie Samoobsługowe grupy definiujące stan członkostwa przetwarzania dla tej grupy. Dotyczy tylko grup, którym GroupType zawiera GroupType.DynamicMembership.
ProxyAddresses            |Adres, według którego obiektu adresatów programu Exchange Server został rozpoznany w systemie poczty obcego.
RenewedDateTime           |Rekord sygnatura czasowa kiedy ostatnio została odnowiona grupy.   
SecurityEnabled           |Wskazuje, czy członkostwo w grupie może mieć wpływ na decyzje dotyczące autoryzacji.
WellKnownObject           |Etykiety obiektu katalogu wyznaczające go jako jeden zestaw wstępnie zdefiniowanych.

## <a name="update-device-attributes"></a>Atrybuty "Aktualizowanie urządzenia"
Atrybut                           | Opis
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AccountEnabled | Wskazuje, czy podmiot zabezpieczeń może przeprowadzać uwierzytelnianie.
CloudAccountEnabled | Wskazuje, czy podmiot zabezpieczeń może przeprowadzać uwierzytelnianie. Napisane przez InTune, gdy urządzenie jest stosowany w wersji lokalnej.
CloudDeviceOSType | Typ urządzenia na podstawie system operacyjny np Windows RT, iOS. Gdy ustawiony przez usługi w chmurze (na przykład Intune), ten atrybut stanie się autorytatywne w katalogu DeviceOSType.
CloudDeviceOSVersion | Wersja systemu operacyjnego. Gdy ustawiony przez usługi w chmurze (na przykład Intune), ten atrybut stanie się autorytatywne w katalogu DeviceOSVersion.
CloudDisplayName | Wartość atrybutu LDAP displayName. Gdy ustawiony przez usługi w chmurze (na przykład Intune), ten atrybut staje się autorytatywne w katalogu displayName.
CloudCreated |Wskazuje, czy dany obiekt został utworzony przez usług w chmurze.
CompliantUntil | Jakie czasu zgodny z uważa urządzenia.
DeviceMetadata |Niestandardowe metadane dla urządzenia
DeviceObjectVersion | Ten atrybut służy do identyfikowania wersji schematu urządzenia.
DeviceOSType |Typ urządzenia na podstawie system operacyjny np Windows RT, iOS. Zapisywane przez usługę rejestracji i mają być aktualizowane przez MDM usługi zarządzania lub STS uproszczone usługi zarządzania.
DeviceOSVersion |Wersja systemu operacyjnego. Zapisywane przez usługę rejestracji i mają być aktualizowane przez MDM usługi zarządzania lub STS uproszczone usługi zarządzania.
DevicePhysicalIds |Atrybut wielowartościowy przeznaczone do przechowywania identyfikatorów urządzenia fizycznego. Może to BIOS identyfikatory, odciski palców TPM i sprzęt określonych identyfikatorów itp.
DirSyncEnabled | Wskazuje, czy synchronizacja jest przeprowadzana z autorytatywnych (nabywca, lokalnego) katalogu.
DisplayName |Nazwa wyświetlana obiektu
IsCompliant | Ten atrybut jest używany do zarządzania stan zarządzania urządzenia przenośnego urządzenia.
IsManaged |Ten atrybut jest używany, aby wskazać, że urządzenie jest zarządzane przez chmurę funkcją zarządzanie urządzeniami przenośnymi
LastDirSyncTime |Godzina ostatniej obiektu została zaktualizowana ze względu na synchronizowanie z autorytatywne katalogu (lokalnego klienta).

## <a name="update-device-configuration-attributes"></a>Atrybuty "Aktualizowanie konfiguracji urządzenia"
Atrybut                           | Opis
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
MaximumRegistrationInactivityPeriod |Maksymalna liczba dni, przez które urządzenia mogą być nieaktywne przed nim jest traktowany jako do usunięcia.
RegistrationQuota   |Zasady, można ograniczyć liczbę rejestracji urządzenia niedozwolone dla jednego użytkownika.

## <a name="update-service-principal-configuration-attributes"></a>Atrybuty "Zaktualizowania konfiguracji głównej usługi"
Atrybut                           | Opis
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AccountEnabled |Wskazuje, czy podmiot zabezpieczeń może przeprowadzać uwierzytelnianie.
AppPrincipalId | Zewnętrzne, zdefiniowane przez aplikację tożsamość podmiot zabezpieczeń.
DisplayName |Nazwa wyświetlana obiektu
ServicePrincipalName    | Główna nazwa usługi, zawierające "Nazwa urząd" gdzie nazwa określa wartość klasy aplikacji i urząd zawiera co najmniej hostname [: port] lub "Nazwa" określający identyfikator podmiotu usługi.

## <a name="update-app-attributes"></a>Atrybuty "Zaktualizować aplikację"
Atrybut                           | Opis
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AppAddress |Zestaw adresy (przekierowywanie adresów URL), które są przypisane do wystawcy usługi.
Identyfikator aplikacji                               |Identyfikator aplikacji aplikacji   
AppIdentifierUri | Aplikacja identyfikator URI, który identyfikuje aplikacji.  Zazwyczaj jest adres URL aplikacji programu access.
AppLogoUrl |Adres url obrazu logo aplikacji, które są przechowywane w sieci CDN.
AvailableToOtherTenants | PRAWDA aplikacja to wielu dzierżawy (to znaczy mogą być używane przez inne dzierżaw).
DisplayName | Nazwa wyświetlana nazwa aplikacji
Uprawnienia |Lista uprawnień do aplikacji.
ExternalUserAccountDelegationsAllowed |Flaga wskazująca, czy aplikacji zasobów jest zaufany i można utworzyć wpisów delegowania dla kont użytkowników zewnętrznych.
GroupMembershipClaims |Członkostwo w grupach oświadczeń zasad.
PublicClient | Wartość PRAWDA, jeśli klient nie może przechowywać poufne (to znaczy jawne klienta w OAuth2.0)
RecordConsentConditions | Typy zgody warunki określone warunki umowy: Brak (0), SilentConsentForPartnerManagedApp(1). Ta wartość będzie widoczna w schemacie interfejsu API wykresu i może zawierać tylko set zmieniane przez administratorów dzierżawy.
RequiredResourceAccess |Zawartość XML wartości właściwości RequiredResourceAccess.
Aplikacja sieci Web |Jeśli wartość true, wskazuje, że ta aplikacja jest aplikacji sieci web.
WwwHomepage |Podstawowy strona sieci Web.

## <a name="update-role-attributes"></a>Atrybuty "Zaktualizować roli"
Atrybut                           | Opis
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AppAddress |Zestaw adresy (przekierowywanie adresów URL), które są przypisane do wystawcy usługi.
BelongsToFirstLoginObjectSet |Jeśli wartość true, wskazuje, że ten obiekt należy do grupy obiektów, trzeba włączyć logowania pierwszego Administrator dzierżawy nowy.
Wbudowane |Wskazuje, czy ważności obiekt jest własnością systemu.
Opis | Zrozumiałą wyrażeń opisowa o obiekcie.  
DisplayName |Nazwa wyświetlana obiektu
MailNickname | Moniker obiektu książki adresowej, zwykle część nazwy e-mail poprzednim "@" symbol.
RoleDisabled |Wskazuje, czy roli powinny być ignorowane na potrzeby kontroli dostępu.
RoleTemplateId | Tożsamość szablonu roli.
ServiceInfo |Usługa obsługi administracyjnej informacje, które mogą być używane przez MOAC i/lub inne usługi wystąpienia (usługa takich samych lub różnych typów).
TaskSetScopeReference |Służy do identyfikowania TaskSet i zestaw zakresów skojarzonego z roli lub szablonów RoleTemplate.
ValidationError |Informacje publikowane przez usługę federacyjnych opisem innych niż zmiennych, specyficzne dla usługi błąd dotyczący właściwości lub łącze z obiektu administratora kroki w celu rozwiązania.
WellKnownObject |Etykiety obiektu katalogu wyznaczające go jako jeden zestaw wstępnie zdefiniowanych.

## <a name="update-role-definition-attributes"></a>Atrybuty "Aktualizowanie definicji roli"
Atrybut                           | Opis
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AssignableScopes    |Kolekcja zakresów autoryzacji, które można odwoływać się przypisywania RoleDefinition to podmiot zabezpieczeń.
DisplayName     |Nazwa wyświetlana obiektu
GrantedPermissions  |Uprawnienia tego RoleDefinition.


## <a name="update-administrative-unit-attributes"></a>Atrybuty "Aktualizowanie jednostki administracyjnej"
Atrybut                           | Opis
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
Opis |   Właściwość ta jest aktualizowana po zmianie opis jednostki administracyjne.
DisplayName |   Właściwość ta jest aktualizowana po zmianie nazwy jednostki administracyjnej.

## <a name="update-company-attributes"></a>Atrybuty "Aktualizacji firmy"
Atrybut                           | Opis
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AllowedDataLocation     | Lokalizacja, w której użytkownicy firmy mogą być obsługi administracyjnej.
AuthorizedServiceInstance   | Nazwy wystąpień usługi, do których można rozmieszczać planu.
DirSyncEnabled |Wskazuje, czy synchronizacja jest przeprowadzana z autorytatywnych (nabywca, lokalnego) katalogu.
DirSyncStatus |Wskazuje, czy synchronizacja obiektów książki adresowej w tym kontekście dzierżawy z autorytatywnych (nabywca, lokalnego) katalogu. rozszerzenia właściwości DirSyncEnabled obiektów firmy.
DirSyncFeatures |Flaga bitowa umożliwia śledzenie zestaw wbudowanych funkcji dirsync włączone i wyłączone dla dzierżawy.
DirectoryFeatures |Funkcje katalogu włączone/wyłączone.
DirSyncConfiguration |Zawiera wszystkie konfiguracji DirSync specyficzne dla bieżącej dzierżawy.
DisplayName |Nazwa wyświetlana obiektu
IsMnc |Ustaw flagę logiczną iff "true", firmy jest włączona funkcja międzynarodowej firmy.
ObjectSettings | Kolekcja ustawień zastosowanie do zakresu obiektu.
PartnerCommerceUrl |Adres URL witryny handlowego partnera.
PartnerHelpUrl |Adres URL witryny pomocy partnera.
PartnerSupportEmail | Adres URL partnera pomocy technicznej w wiadomości e-mail.
PartnerSupportTelephone |Adres URL do partnera pomocy technicznej telefonu.
PartnerSupportUrl |Adres URL witryny pomocy technicznej partnera.
StrongAuthenticationDetails |Szczegóły dotyczące StrongAuthentication.
StrongAuthenticationPolicy |Zasady silnego uwierzytelniania dla firmy.
TechnicalNotificationMail |Adres e-Mail, aby powiadomić kwestii technicznych dotyczących firmy.
TelephoneNumber |Numery telefonów, które są zgodne z E.123 rekomendacji ITU.
TenantType |Typ dzierżawy. Jeśli ten parametr na wartość nie jest określona, dzierżawy jest firmy. W przeciwnym razie możliwe są następujące wartości: MicrosoftSupport (0), SyndicatePartner (1), ValueAddedResellerPartnerDelegatedAdmin ResellerPartnerDelegatedAdmin (4) BreadthPartnerDelegatedAdmin (3) BreadthPartner (2) (5).
VerifiedDomain |Zestaw nazw domen DNS związane z firmy.

## <a name="update-domain-attributes"></a>Atrybuty "Zaktualizowania domeny"
Atrybut                           | Opis
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
Funkcje    |Bit flagi opisujące możliwości domeny, jeśli istnieją.
Domyślne |Wskazuje, czy domena jest wartością domyślną; na przykład UserPrincipalName domyślny sufiks gdy administrator tworzy nowego użytkownika w MOAC.
Początkowa |Wskazuje, czy domena jest domenę początkową dla firmy, przydzielony przez OCP. Początkowa domena jest unikatowy podrzędny domeny Microsoft Online; e.g.contoso3.microsoftonline.com.
LiveType    |Wpisz odpowiednie usługi Windows Live przestrzeń nazw, jeśli istnieją.
Nazwa    | Identyfikator punktu końcowego.
PasswordNotificationWindowDays  |Liczba dni przed wygaśnięciem hasła użytkownik jest powiadamiany.
PasswordValidityPeriodDays  | Można zmienić liczbę dni, które hasła będzie pasować do przed nim.

Wymagany formant dla wielu przepisów zgodności są rekordy inspekcji. Dla klientów korzystających z Azure Active Directory inspekcji raportu w celu dotrzymania ich zgodności z przepisami zaleca się, że klient przesyłanie kopię tego tematu Pomocy z kopią raportu inspekcji eksportowanego klienta, aby łatwiej wyjaśnić szczegółów raportu. Jeśli Inspektor chce opis przepisów zgodności, obecnie spełniające Azure, wskazać inspektora do [strony zgodność](https://azure.microsoft.com/support/trust-center/compliance/) Microsoft Azure Trust Center.
