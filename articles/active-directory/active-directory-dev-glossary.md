<properties
   pageTitle="Słownik Deweloper Azure Active Directory | Microsoft Azure"
   description="Lista postanowienia dotyczące często używanych pojęcia deweloperów usługi Azure Active Directory i funkcji."
   services="active-directory"
   documentationCenter=""
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/31/2016"
   ms.author="bryanla"/>

# <a name="azure-active-directory-developer-glossary"></a>Słownik deweloperów usługi Azure Active Directory
Ten artykuł zawiera definicje dla niektórych podstawowych koncepcji Deweloper Azure Active Directory (AD), które są przydatne, gdy informacje na temat tworzenia aplikacji dla usługi Azure AD.

## <a name="access-token"></a>token dostępu
Typ [tokenu zabezpieczającego](#security-token) wystawiony przez [serwer autoryzacji](#authorization-server)i używane przez [aplikacji klienckiej](#client-application) w celu uzyskania dostępu do [chronionych zasobów serwera](#resource-server). Zazwyczaj w postaci [JSON Token sieci Web (JWT)][JWT], token stanowią pełnomocnictwa do klienta przez [właściciela zasobów](#resource-owner), aby uzyskać żądany poziom dostępu. Token zawiera wszystkie stosowne [oświadczeniach](#claim) temat Włączanie aplikacji klienckiej używany jako formularz poświadczeń, podczas uzyskiwania dostępu do zasobu. Eliminuje to potrzebę właściciela zasobów udostępniania poświadczeń do klienta.

Tokeny dostępu są czasami określane jako "Użytkownika + aplikację" lub "Aplikacji tylko", w zależności od poświadczeń reprezentowanego. Na przykład, gdy aplikacja kliencka używa:

- [Udzielanie autoryzacji "Kodu autoryzacji"](#authorization-grant), użytkownik końcowy uwierzytelnia najpierw jako właściciel zasobu, delegowanie zezwolenia na kliencie dostępu do zasobu. Klient uwierzytelnia później podczas uzyskiwania token dostępu. Tokenu może być nazywane bardziej szczegółowo tokenu "Użytkownika + aplikację" jak przedstawia zarówno użytkownika, którego autoryzowanych aplikacji klienckich i aplikacji.
- [Udzielanie autoryzacji "Poświadczeń klienta"](#authorization-grant), klienta zawiera wyłącznie uwierzytelniania, działania bez właściciela zasobu uwierzytelniania i autoryzacji, więc token może czasami określane jako token "Tylko do aplikacji".

Zobacz [informacje dotyczące Azure AD Token] [ AAD-Tokens-Claims] uzyskać więcej szczegółowych informacji.

## <a name="application-manifest"></a>manifest aplikacji  
Funkcja dostarczony przez [portal Azure klasyczny][AZURE-classic-portal], który tworzy reprezentację JSON Konfiguracja tożsamości aplikacji, używane w charakterze mechanizmu aktualizacji skojarzona z nim [aplikacji] [ AAD-Graph-App-Entity] i [ServicePrincipal] [ AAD-Graph-Sp-Entity] jednostek. Zobacz [Opis manifest aplikacji usługi Azure Active Directory] [ AAD-App-Manifest] uzyskać więcej szczegółowych informacji.

## <a name="application-object"></a>obiekt aplikacji  
Gdy możesz rejestru/aktualizacji aplikacji w [portal Azure klasyczny][AZURE-classic-portal], portalu tworzy i aktualizacje odpowiedni [obiekt głównej usługi](#service-principal-object) i obiektu aplikacji dla dzierżawy. Obiekt aplikacji *definiuje* aplikacji jest Konfiguracja tożsamości globalnie (dotyczy wszystkich dzierżaw miejsce, w którym ma dostęp), definiując szablon z którego odpowiednie obiekty głównej usługi są *uzyskane* do użytku lokalnie w czasie wykonywania (w określonym dzierżawy).

Zobacz [aplikacji i usług kapitału obiektów] [ AAD-App-SP-Objects] uzyskać więcej informacji.

## <a name="application-registration"></a>Rejestrowanie aplikacji  
Aby zezwolić aplikacji na Integracja z i delegowanie funkcji tożsamości i zarządzanie dostępem do Azure AD, musi być zarejestrowany z usługi Azure AD [dzierżawy](#tenant). Po zarejestrowaniu aplikacji z usługą Azure Active Directory są podawane konfiguracji tożsamości dla aplikacji, umożliwiające jej Integracja z usługą Azure Active Directory i użyj funkcji, takich jak:

- Efektywne zarządzanie z logowania jednokrotnego przy użyciu Azure AD Zarządzanie tożsamościami i [Łączenie OpenID] [ OpenIDConnect] implementacji protokołu
- Brokered dostępu do [chronionych zasobów](#resource-server) w [aplikacjach klienckich](#client-application), za pomocą implementacji [autoryzacji serwera](#authorization-server) OAuth 2.0 Azure AD
- [Zgoda framework](#consent) zarządzania klientowi dostępu do chronionych zasobów według autoryzacji właściciela zasobów.

[Integracja] aplikacji w programie usługi Azure Active Directory[ AAD-Integrating-Apps] uzyskać więcej szczegółowych informacji.

## <a name="authentication"></a>uwierzytelnianie
Czynność trudne przyjęcie dla wiarygodnych poświadczeń, dostarczając podstawy tworzenia podmiot zabezpieczeń ma być używany dla tożsamości i kontroli dostępu. Na przykład podczas [udzielić autoryzacji uwierzytelnianie OAuth2](#authorization-grant) strona uwierzytelniania jest wypełnianie rolę [właściciela zasobu](#resource-owner) lub [aplikacji klienckiej](#client-application), w zależności od Udziel używane.

## <a name="authorization"></a>Autoryzacja
Czynność przyznawania uwierzytelniony kapitału uprawnień do czegoś. W modelu programowania Azure AD są dostępne dwa podstawowe użycie przypadki:

- Podczas przepływu [udzielić autoryzacji uwierzytelnianie OAuth2](#authorization-grant) : Jeśli [właściciel zasobu](#resource-owner) udzieli autoryzacji z [aplikacją kliencką](#client-application), umożliwiając klienta, aby uzyskać dostęp do zasobów właściciela zasobów.
- Podczas dostęp do zasobów przez klienta: wykonane przez [serwer zasobów](#resource-server), przy użyciu [rościć sobie](#claim) wartości obecne w [token dostępu](#access-token) do sterować dostępem na ich podstawie.

## <a name="authorization-code"></a>Kod autoryzacji
Krótki okres ważności "token" dostarczone z [aplikacją kliencką](#client-application) przez punkt [końcowy autoryzacji](#authorization-endpoint)w ramach przepływu "kodu autoryzacji" jedną z czterech uwierzytelnianie OAuth2 [udziela zezwolenia](#authorization-grant). Kod są zwracane do aplikacji klienckiej w odpowiedzi na uwierzytelniania [właściciela zasobu](#resource-owner), wskazująca, że właściciel zasobu delegował zezwolenia na dostęp do żądanego zasobów. W ramach przepływu kod jest później zaginął [token dostępu](#access-token).

## <a name="authorization-endpoint"></a>punkt końcowy autoryzacji
Jeden z punktów końcowych, realizowane przez [serwer autoryzacji](#authorization-server), umożliwiające interakcję z [właścicielem zasobu](#resource-owner) w celu udostępniania [udzielić autoryzacji](#authorization-grant) podczas autoryzacji uwierzytelnianie OAuth2 udzielanie przepływu. W zależności od używanych przepływ Udziel autoryzacji rzeczywistego przyznania opisane może być różna w tym [kodu autoryzacji](#authorization-code) lub [tokenu zabezpieczającego](#security-token).

Zobacz specyfikacja uwierzytelnianie OAuth2 [autoryzacji przyznać typy] [ OAuth2-AuthZ-Grant-Types] oraz [końcowy autoryzacji] [ OAuth2-AuthZ-Endpoint] sekcji i [Specyfikacja OpenIDConnect] [ OpenIDConnect-AuthZ-Endpoint] uzyskać więcej szczegółowych informacji.

## <a name="authorization-grant"></a>Udziel autoryzacji
Poświadczenie reprezentującą [właściciela zasobu](#resource-owner) [autoryzacji](#authorization) , aby uzyskać dostęp do chronionych zasobów, udzielił z [aplikacji klienckiej](#client-application). Aplikacja klienta można użyć jednej z [cztery typy udzielanie zdefiniowane przez Framework autoryzacji uwierzytelnianie OAuth2] [ OAuth2-AuthZ-Grant-Types] uzyskanie darowizny, w zależności od typu i wymagania dotyczące klienta: "Udziel kod autoryzacji", "przyznać poświadczeń klienta", "Udziel niejawne" i "przyznać zasobu właściciela hasła poświadczeń". [Token dostępu](#access-token)lub [kodu autoryzacji](#authorization-code) (wymiana później, aby token dostępu), zależnie od typu Udziel autoryzacji używany jest poświadczeń zwracane do klienta.

## <a name="authorization-server"></a>Autoryzacja serwera
Określone w [Ramach autoryzacji uwierzytelnianie OAuth2][OAuth2-Role-Def], odpowiedzialne wydawania dostępu do serwera tokeny do [klienta](#client-application) po pomyślnym uwierzytelniania [właściciela zasobów](#resource-owner) i uzyskania zezwolenie. [Aplikacji klienckiej](#client-application) współpracuje z serwerem autoryzacji w czasie wykonywania za pośrednictwem [autoryzacji](#authorization-endpoint) i [token](#token-endpoint) punkty końcowe, zgodnie z uwierzytelnianie OAuth2 zdefiniowane [udziela autoryzacji](#authorization-grant).

W przypadku integracja aplikacji Azure AD, Azure AD wykonuje rola serwera autoryzacji dla aplikacji Azure AD i usługi firmy Microsoft interfejsów API, na przykład [Programu Microsoft Graph API][Microsoft-Graph].

## <a name="claim"></a>Przejmowanie
[Tokenu zabezpieczającego](#security-token) zawiera roszczeń, które zapewniają potwierdzeń o jedną jednostkę (na przykład [aplikacji klienckiej](#client-application) lub [właściciel zasobu](#resource-owner)) do innego podmiotu (na przykład [serwer zasobów](#resource-server)). Roszczeń są pary nazwa wartość, które przekazywania fakty dotyczące tematu token (na przykład podmiot zabezpieczeń został uwierzytelniony przez [serwer autoryzacji](#authorization-server)). Roszczeń token zawiera danego zależą od wielu zmiennych, takich jak typ tokenu, typ poświadczeń używanych do uwierzytelniania temat konfiguracji aplikacji, itp.

Zobacz [informacje dotyczące tokenu Azure AD] [ AAD-Tokens-Claims] uzyskać więcej szczegółowych informacji.

## <a name="client-application"></a>Aplikacja klienta  
Określone w [Ramach autoryzacji uwierzytelnianie OAuth2][OAuth2-Role-Def], aplikacji, która powoduje, że chroniony żądania zasobu w imieniu [właściciela zasobów](#resource-owner). Termin "klient" oznacza wszelkie cechy wykonania określonego sprzętu (na przykład, czy aplikacja wykonuje na serwerze, pulpitu lub innych urządzeń).  

Aplikacja klienta żąda [autoryzacji](#authorization) od właściciela zasobów, aby wziąć udział w przepływie [udzielić autoryzacji uwierzytelnianie OAuth2](#authorization-grant) , a może uzyskać dostęp do interfejsów API i dane w imieniu właściciela zasobów. Uwierzytelnianie OAuth2 autoryzacji Framework [definiuje dwa typy klientów][OAuth2-Client-Types]"poufne" i "publicznej", oparte na możliwość klienta zachowania poufności swoje poświadczenia. Aplikacje można zaimplementować [Klient sieci web (poufnych)](#web-client) , który jest uruchomiony na serwerze sieci web z [klientami (publiczne)](#native-client) zainstalowanych na urządzeniu lub [systemem agenta użytkownika klienta (publiczne)](#user-agent-based-client) , który działa w przeglądarce urządzenia.

## <a name="consent"></a>zgody
Proces [właściciela zasobu](#resource-owner) udzielenia zezwolenia z [aplikacją kliencką](#client-application), określonych [uprawnień](#permissions) dostępu do chronionych zasobów imieniu właściciela zasobów. W zależności od uprawnień żądany przez klienta administrator lub użytkownik zostanie poproszony o zezwolenie dostępu do swoich danych organizacji i poszczególnych odpowiednio. Uwaga w scenariuszu [dzierżawy wielu](#multi-tenant-application) aplikacji [głównej usługi](#service-principal-object) jest zarejestrowany w dzierżawie consenting użytkownika.

## <a name="id-token"></a>Identyfikator tokenu
[Łączenie OpenID] [ OpenIDConnect-ID-Token] [tokenu zabezpieczającego](#security-token) dostarczony przez [autoryzacji serwera](#authorization-server) [autoryzacji punktu końcowego](#authorization-endpoint), zawierający [roszczeń](#claim) dotyczących uwierzytelnianie użytkownika końcowego [właściciela zasobów](#resource-owner). Przykład token dostępu identyfikator tokeny są przedstawiane także jako podpisane cyfrowo [JSON Token sieci Web (JWT)][JWT]. W odróżnieniu od token dostępu, token identyfikator roszczeń nie są używane do celów związanych z dostęp do zasobów i specjalnie kontroli dostępu.

Zobacz [informacje dotyczące tokenu Azure AD] [ AAD-Tokens-Claims] uzyskać więcej szczegółowych informacji.

## <a name="multi-tenant-application"></a>stosowanie wielu dzierżawy
Rodzaj [aplikacji klienckiej](#client-application) , która umożliwia logowanie się i [Zgoda](#consent) przez użytkowników obsługi administracyjnej w dowolnym Azure AD [dzierżawy](#tenant), łącznie z dzierżawami niż to, gdzie jest zarejestrowana klienta. Natomiast aplikacji zarejestrowany jako pojedyncze dzierżawy tylko umożliwia rejestrowaniu z kont użytkowników obsługi administracyjnej w tej samej dzierżawy, jak gdzie jest zarejestrowana aplikacji. Aplikacje [klienckie natywnych](#native-client) są wielu dzierżawy domyślnie aplikacje [klienckie web](#web-client) mieć możliwość wybrać jedno- i wielu dzierżawy.

Dowiedz się, [jak zalogować się każdy użytkownik Azure AD przy użyciu deseń wielu dzierżawy aplikacji] [ AAD-Multi-Tenant-Overview] uzyskać więcej szczegółowych informacji.

## <a name="native-client"></a>klientami
Typ [aplikacji](#client-application) , która jest oryginalnie zainstalowana na urządzeniu. Ponieważ cały kod jest wykonywana na urządzeniu, jest traktowany jako "publicznej" klienta z powodu niemożność przechowywanie poświadczeń prywatnie-jako poufne. Zobacz [typy klienta uwierzytelnianie OAuth2 i profile] [ OAuth2-Client-Types] uzyskać więcej szczegółowych informacji.

## <a name="permissions"></a>uprawnienia
[Aplikacja klienta](#client-application) uzyskuje dostęp do [serwera zasobów](#resource-server) poprzez zgłoszenie żądania uprawnień. Dostępne są dwa typy:

- "Delegowanej" uprawnienia, które żądanie [zakresu](#scopes) dostępu w obszarze delegowane zgody zalogować [właściciela zasobów](#resource-owner), są prezentowane do zasobu w czasie wykonywania jako [oświadczeniach "połączenia"](#claim) w kliencie programu [token dostępu](#access-token).
- Uprawnienia "Aplikacji", które zażądać dostępu [oparta na rolach](#roles) tożsamością i poświadczenia aplikacji klienckiej, są prezentowane do zasobu w czasie wykonywania jako [roszczenia "ról"](#claim) w token dostępu klienta.

Są również powierzchniowy w trakcie [zgodę](#consent) dadzą administratora lub właściciela zasobu udzielanie-odmówić dostępu klienta do zasobów w ich dzierżawy.

Żądania uprawnień są skonfigurowane na "Aplikacji" / "Konfigurowanie" karty w [portal Azure klasyczny][AZURE-classic-portal], w obszarze "Uprawnienia do innych aplikacji", przez wybranie odpowiedniej "delegowane uprawnienia" i "Uprawnienia aplikacji" (jego wymaga członkostwa w roli administratora globalnego). [Klient publicznej](#client-application) nie można zachować poświadczeń, go delegowane uprawnienia można zażądać tylko podczas [poufne klienta](#client-application) ma możliwość żądania delegowanej oraz uprawnienia aplikacji. Klienta [obiektu aplikacji](#application-object) przechowuje zadeklarowanych uprawnień w jego [Właściwość requiredResourceAccess][AAD-Graph-App-Entity].

## <a name="resource-owner"></a>właściciel zasobu
Określone w [Ramach autoryzacji uwierzytelnianie OAuth2][OAuth2-Role-Def], jednostka do udzielania dostępu do chronionych zasobów. Jeśli właściciel zasobu jest osobą, jego nosi nazwę użytkownika końcowego. Na przykład, gdy [aplikacja klienta](#client-application) chce uzyskać dostęp do skrzynki pocztowej użytkownika za pośrednictwem [Interfejsu API programu Microsoft Graph][Microsoft-Graph], wymaga uprawnienia właściciela zasobu skrzynki pocztowej.

## <a name="resource-server"></a>serwer zasobów
Określone w [Ramach autoryzacji uwierzytelnianie OAuth2][OAuth2-Role-Def], hosts ochronę zasobów, akceptując i odpowiadanie na serwerze chroniony żądania zasobu przez [aplikacje klienckie](#client-application) prezentowanie [token dostępu](#access-token). Nazywane także chronionego zasobu, aplikację serwera lub zasobu.

Serwer zasobów udostępnia interfejsy API i wymusza dostęp do jej zasobów chronionych za pośrednictwem [zakresów](#scopes) i [ról](#roles)przy użyciu struktury autoryzacji 2.0 OAuth. Jako przykład można wymienić interfejsu API Azure AD wykres, który umożliwia dostęp do danych dzierżawy Azure AD i interfejsów API 365 pakietu Office, aby uzyskać dostęp do danych, takich jak Poczta i kalendarz. Oba te są również dostępne za pośrednictwem [Interfejsu API programu Microsoft Graph][Microsoft-Graph].  

Podobnie jak w przypadku aplikacji klienckich konfiguracji tożsamości aplikacji zasobów nawiązano za pośrednictwem [rejestracji](#application-registration) w dzierżawie usługi Azure AD udostępniania aplikacji i kapitału obiektu usługi. Niektóre API udostępnionych przez firmę Microsoft, takich jak API Azure AD wykresu, wstępnie zarejestrowanych głównych usługi udostępnione w wszystkich dzierżaw podczas inicjowania obsługi administracyjnej.

## <a name="roles"></a>role
Jak [zakresy](#scopes)role umożliwiają dla [zasobów serwera](#resource-server) dotyczące dostępu do chronionych zasobów. Istnieją dwa typy: "rolę" wykonuje kontrola dostępu oparta na rolach dla użytkowników i grup, które wymagają dostępu do tego zasobu, natomiast rolę "aplikacji" wykonuje tak samo w [aplikacjach klienckich](#client-application) wymagają programu access.

Role są ciągami zdefiniowane zasobów (na przykład "osoba zatwierdzająca wydatków", "Tylko do odczytu", "Directory.ReadWrite.All") zarządzania w [portal Azure klasyczny] [ AZURE-classic-portal] za pośrednictwem tego zasobu [pojawiają aplikacji](#application-manifest)i przechowywane w zasobu [Właściwości appRoles][AAD-Graph-Sp-Entity]. Klasyczny portalu Azure służy także przypisywać użytkowników do ról "użytkownika" i skonfiguruj klienta [uprawnienia aplikacji](#permissions) , aby uzyskać dostęp do roli "aplikacji".

Szczegółowe omówienie ról aplikacji ujawnionego przez interfejs API wykresu Azure AD, zobacz [Zakresy uprawnień interfejsu API wykresu][AAD-Graph-Perm-Scopes]. Na przykład implementacji krok po kroku, zobacz [Kontrola dostępu w chmurze aplikacji przy użyciu Azure AD oparta na rolach][Duyshant-Role-Blog].

## <a name="scopes"></a>zakresy
Podobnie jak [role](#roles)zakresy umożliwiają dla [zasobów serwera](#resource-server) dotyczące dostępu do chronionych zasobów. Zakresy są używane do implementowania, [opartych na zakresach] [ OAuth2-Access-Token-Scopes] kontroli dostępu, dla [aplikacji klienckiej](#client-application) , która ma delegowanej dostęp do zasobu przez jego właściciela.

Zakresy są zasób zdefiniowany ciągami (na przykład "Mail.Read", "Directory.ReadWrite.All"), zarządzania w [portal Azure klasyczny] [ AZURE-classic-portal] za pośrednictwem tego zasobu [pojawiają aplikacji](#application-manifest)i przechowywane w zasobu [Właściwość oauth2Permissions][AAD-Graph-Sp-Entity]. Azure portalu klasyczny służy także do konfigurowania klienta aplikacji [przypisane uprawnienia](#permissions) dostępu do zakresu.

Najlepsze praktyki konwencji nazewnictwa, jest konieczne użycie formatu "resource.operation.constraint". Szczegółowe omówienie zakresów ujawnionego przez interfejs API wykresu Azure AD, zobacz [Zakresy uprawnień interfejsu API wykresu][AAD-Graph-Perm-Scopes]. Dla zakresów ujawnionego przez usługi Office 365, zobacz [informacje dotyczące uprawnień usługi Office 365 API][O365-Perm-Ref].

## <a name="security-token"></a>tokenu zabezpieczającego
Podpisanego dokumentu zawierającego roszczeń, takich jak uwierzytelnianie OAuth2 token lub potwierdzenia SAML 2.0. Uwierzytelnianie OAuth2 [udzielić autoryzacji](#authorization-grant), [token dostępu](#access-token) (uwierzytelnianie OAuth2) i [Token Identyfikatora](OpenID Connect) są typy tokenów zabezpieczających, które są wykonywane jako [JSON Token sieci Web (JWT)][JWT].

## <a name="service-principal-object"></a>Obiekt głównej usługi
Gdy możesz rejestru/aktualizacji aplikacji w [portal Azure klasyczny][AZURE-classic-portal], portalu tworzy i aktualizacje odpowiedni obiekt głównej usługi i [obiekt aplikacji](#application-object) dla dzierżawy. Obiekt aplikacji *definiuje* Konfiguracja tożsamości globalnie (dotyczy wszystkich dzierżaw miejsce, w którym skojarzonej aplikacji udzielono dostępu), i jest szablon, z którego odpowiednie obiekty głównej usługi są *uzyskane* do użytku lokalnie w czasie wykonywania (w określonym dzierżawy).

Zobacz [aplikacji i usług kapitału obiektów] [ AAD-App-SP-Objects] uzyskać więcej informacji.

## <a name="sign-in"></a>Logowanie
Proces [aplikacji klienckiej](#client-application) inicjowania uwierzytelnianie użytkownika końcowego i rejestrowania powiązanych stanu, w celu nabycia [tokenu zabezpieczającego](#security-token) i określanie zakresu sesji aplikacji do tego stanu. State mogą zawierać artefakty, takie jak informacje o profilach użytkowników i informacje pochodzące z token oświadczeń.

Funkcja rejestrowania aplikacji jest zazwyczaj używany do wykonania jedno logowania jednokrotnego (SSO). Go może również być poprzedzony funkcję "zapisów" jako punkt wejścia dla użytkownika końcowego uzyskać dostęp do aplikacji (tego, pierwszego logowania). Funkcja zapisów służy do zbierania i utrzymują dodatkowy stan specyficzne dla użytkownika, a może wymagać [użytkownik wyraża zgodę](#consent).

## <a name="sign-out"></a>wyrejestrowywania
Proces odinstalowywania uwierzytelniania użytkownika końcowego odłączenia stanu użytkownika skojarzonego z sesji [aplikacji klienckiej](#client-application) podczas [logowania](#sign-in)

## <a name="tenant"></a>dzierżawy
Wystąpienie katalogu Azure AD nosi nazwę dzierżawę Azure AD. Zapewnia szeroką gamę funkcji, takich jak:

- Usługa rejestru zintegrowanych aplikacji
- Uwierzytelnianie konta użytkowników i zarejestrowanych aplikacji
- Punkty końcowe pozostałe wymagane do obsługi różnych protokołów między innymi uwierzytelnianie OAuth2 i SAML, w tym punkt [końcowy autoryzacji](#authorization-endpoint), [token punktu końcowego](#token-endpoint) i punkt końcowy "typowych" używane przez [aplikacje wielu dzierżawy](#multi-tenant-application).

Dzierżawy również jest skojarzony z Azure AD lub innej subskrypcji usługi Office 365 podczas inicjowania obsługi administracyjnej subskrypcji, dostarczając funkcji tożsamości i zarządzanie dostępem dla subskrypcji. Dowiedz się, [jak uzyskać dzierżawę usługi Azure Active Directory] [ AAD-How-To-Tenant] szczegółowe informacje na różne sposoby, możesz uzyskać dostęp do dzierżawy. Zobacz, [jak Azure subskrypcje są skojarzone z usługi Azure Active Directory] [ AAD-How-Subscriptions-Assoc] szczegółowe informacje na temat relacji między subskrypcji i dzierżawę Azure AD.

## <a name="token-endpoint"></a>token punktu końcowego
Jeden z punktów końcowych, realizowane przez [serwer autoryzacji](#authorization-server) obsługuje uwierzytelnianie OAuth2 [udziela autoryzacji](#authorization-grant). W zależności od Udziel, go umożliwia uzyskanie [token dostępu](#access-token) (i powiązanych token "Odśwież") [klienta](#client-application)lub [token Identyfikatora](#ID-token) używanego z systemami [Łączenie OpenID] [ OpenIDConnect] Protocol (protokół).

## <a name="user-agent-based-client"></a>Systemem agenta użytkownika klienta
Typ [aplikacji](#client-application) , która pobiera kod z serwera sieci web i wykonywanego agenta użytkownika (na przykład przeglądarki sieci web), takich jak pojedynczej strony aplikacji (bezpieczne uwierzytelnianie HASŁA). Ponieważ cały kod jest wykonywana na urządzeniu, jest traktowany jako "publicznej" klienta z powodu niemożność przechowywanie poświadczeń prywatnie-jako poufne. Zobacz [typy klienta uwierzytelnianie OAuth2 i profile] [ OAuth2-Client-Types] uzyskać więcej szczegółowych informacji.

## <a name="user-principal"></a>główna użytkownika
Podobnie jak w odpowiedni sposób obiektem głównej usługi jest używany do przedstawiania wystąpienie aplikacji, obiekt kapitału użytkownika jest innego typu podmiot zabezpieczeń, która odpowiada użytkownik. Azure AD wykresu [obiektem użytkownik] [ AAD-Graph-User-Entity] definiuje schemat obiekcie użytkownika, w tym dotyczące użytkowników właściwości, takie jak imię i nazwisko, głównej nazwy użytkownika, członkostwo w roli katalogu itp. Dzięki temu konfiguracja tożsamość użytkownika dla Azure AD ustanawiania główna użytkownika w czasie wykonywania. Główna użytkownika jest używany do reprezentują uwierzytelnionego użytkownika dla rejestracji jednokrotnej, rejestrowanie delegowanie [Zgoda](#consent) , przez co sterować dostępem itp.

## <a name="web-client"></a>Klient sieci Web
Typ [aplikacji klienckiej](#client-application) wykonującego cały kod na serwerze sieci web i może działać jako klient "poufne" bezpieczne swoje poświadczenia są przechowywane na serwerze. Zobacz [typy klienta uwierzytelnianie OAuth2 i profile] [ OAuth2-Client-Types] uzyskać więcej szczegółowych informacji.

## <a name="next-steps"></a>Następne kroki
[Azure AD Przewodnik programisty] [ AAD-Dev-Guide] portalu używać rozwoju Azure AD temat, łącznie z omówieniem [integracji aplikacji] [ AAD-How-To-Integrate] i podstawy [Azure AD uwierzytelniania]i scenariusze obsługiwanego uwierzytelniania[AAD-Auth-Scenarios].

Użyj poniższej sekcji komentarze Disqus na przesyłanie opinii i Pomóż nam Uściślanie i kształtowanie naszych zawartości.

<!--Image references-->

<!--Reference style links -->
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AAD-Graph-User-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity
[AAD-How-Subscriptions-Assoc]: ./active-directory-how-subscriptions-associated-directory.md
[AAD-How-To-Integrate]: ./active-directory-how-to-integrate.md
[AAD-How-To-Tenant]: active-directory-howto-tenant.md
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Multi-Tenant-Overview]: active-directory-devhowto-multi-tenant-overview.md
[AAD-Security-Token-Claims]: ./active-directory-authentication-scenarios/#claims-in-azure-ad-security-tokens
[AAD-Tokens-Claims]: ./active-directory-token-and-claims.md
[AZURE-classic-portal]: https://manage.windowsazure.com
[Duyshant-Role-Blog]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/
[JWT]: https://tools.ietf.org/html/draft-ietf-oauth-json-web-token-32
[Microsoft-Graph]: https://graph.microsoft.io
[O365-Perm-Ref]: https://msdn.microsoft.com/en-us/office/office365/howto/application-manifest
[OAuth2-Access-Token-Scopes]: https://tools.ietf.org/html/rfc6749#section-3.3
[OAuth2-AuthZ-Endpoint]: https://tools.ietf.org/html/rfc6749#section-3.1
[OAuth2-AuthZ-Grant-Types]: https://tools.ietf.org/html/rfc6749#section-1.3
[OAuth2-Client-Types]: https://tools.ietf.org/html/rfc6749#section-2.1
[OAuth2-Role-Def]: https://tools.ietf.org/html/rfc6749#page-6
[OpenIDConnect]: http://openid.net/specs/openid-connect-core-1_0.html
[OpenIDConnect-AuthZ-Endpoint]: http://openid.net/specs/openid-connect-core-1_0.html#AuthorizationEndpoint
[OpenIDConnect-ID-Token]: http://openid.net/specs/openid-connect-core-1_0.html#IDToken
