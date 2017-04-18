<properties
   pageTitle="Jak utworzyć aplikację, której można się zalogować dowolny użytkownik usługi Azure Active Directory | Microsoft Azure"
   description="Krok po kroku instrukcje dotyczące tworzenia aplikacji, które mogą logować się użytkownika z dowolnej dzierżawy usługi Azure Active Directory, nazywane także aplikacji wielu dzierżawy."
   services="active-directory"
   documentationCenter=""
   authors="skwan"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/11/2016"
   ms.author="skwan;bryanla"/>

# <a name="how-to-sign-in-any-azure-active-directory-ad-user-using-the-multi-tenant-application-pattern"></a>Jak zalogować się każdy użytkownik Azure Active Directory (AD) za pomocą deseń wielu dzierżawy aplikacji
Jeśli oprogramowanie jako aplikacji usługi w wielu organizacjach, można skonfigurować aplikację, aby zaakceptować rejestrowaniu z dowolnym dzierżawy Azure AD.  W Azure AD jest to, co Twojej dzierżawy wielu aplikacji.  Użytkownicy w dowolnym dzierżawy Azure AD będzie można się zalogować do aplikacji po wyrażając do swojego konta za pomocą aplikacji.  

Jeśli masz istniejącą aplikację, która ma swój własny system konta lub obsługuje inne rodzaje logowania od innych dostawców usług w chmurze, dodanie Azure AD logowanie z dowolnej dzierżawy jest prosty sposób rejestrowania aplikacji, dodając logowania w kodzie za pośrednictwem uwierzytelnianie OAuth2, łączenie OpenID lub SAML i umieszczania Zaloguj się przy użyciu przycisku Microsoft w aplikacji. Kliknij przycisk poniżej, aby dowiedzieć się więcej o znakowaniu aplikacji.

[! [Zaloguj się przycisk] [AAD-logowania]][AAD-App-Branding]


W tym artykule założono, że znasz już tworzenie jednej dzierżawy stosowania Azure AD.  Jeśli nie jesteś głowy wrócić do [strony głównej przewodnik dla deweloperów] [ AAD-Dev-Guide] i wypróbuj jedną z naszych Szybkie uruchamianie!

Istnieją cztery proste czynności, aby przekonwertować aplikacji do aplikacji wielu dzierżawy Azure AD:

1.  Aktualizowanie rejestracji aplikacji należy wielu dzierżawy
2.  Aktualizacji kodu wysyłanie wezwań do / Common punktu końcowego 
3.  Aktualizacji kodu do obsługi wielu wartości wystawcy
4.  Opis zgody użytkownik i administrator i wprowadź zmiany odpowiedni kod

Przyjrzyjmy się każdy krok szczegółowo. Również umożliwia bezpośrednie przejście bezpośrednio do [tej listy próbek wielu dzierżawy][AAD-Samples-MT].

## <a name="update-registration-to-be-multi-tenant"></a>Aktualizowanie rejestracji za wiele dzierżawy
Domyślnie rejestracji aplikacji i interfejsu API sieci web z Azure AD są jednej dzierżawy.  Można wprowadzić rejestracji wielu dzierżawy znajdując przełącznik "Aplikacji jest wiele dzierżawy" na stronie Konfiguracja rejestracji aplikacji w [portal Azure klasyczny] [ AZURE-classic-portal] i ustawienie "Tak".

Uwaga: Zanim aplikacja może się wiele dzierżawy, Azure AD wymaga identyfikatora URI identyfikator aplikacji stosowania globalnie unikatowa. Identyfikator URI aplikacji jest jednym ze sposobów zapewniających aplikacji jest identyfikowany w komunikatach protokołu.  W aplikacji dla jednej dzierżawy wystarczy dla identyfikatora URI identyfikator aplikacji być unikatowa w obrębie tej dzierżawy.  Stosowania wielu dzierżawy musi być globalnie unikatowa tak Azure AD można znaleźć aplikacji przez wszystkich dzierżaw.  Plik globalny są wymuszane przez wymaganie ma nazwy hosta, która jest zgodna z domeną zweryfikowaną dzierżawy Azure AD identyfikator URI identyfikator aplikacji.  Na przykład jeśli nazwa Twojej dzierżawy została contoso.onmicrosoft.com prawidłową aplikację identyfikator URI będzie `https://contoso.onmicrosoft.com/myapp`.  Gdyby dzierżawy usługi domeną zweryfikowaną `contoso.com`, a następnie prawidłowego identyfikatora URI identyfikator aplikacji może być także `https://contoso.com/myapp`.  Ustawianie aplikacji wielu dzierżawy zakończy się niepowodzeniem, jeśli identyfikator URI identyfikator aplikacji nie wykonaj tego wzorca.

Rejestracji klientami są wielu dzierżawy domyślnie.  Nie musisz podejmować żadnych działań, aby wprowadzić macierzystego dzierżawy wielu rejestracji aplikacji klienta.

## <a name="update-your-code-to-send-requests-to-common"></a>Aktualizacji kodu wysyłanie wezwań do/Common
W aplikacji jednej dzierżawy Zaloguj się żądania są wysyłane do dzierżawy Zaloguj punktu końcowego.   Na przykład dla contoso.onmicrosoft.com, która będzie punkt końcowy:

    https://login.microsoftonline.com/contoso.onmicrosoft.com

Żądania wysyłane do punktu końcowego dzierżawy mogą logować się użytkownicy (lub Goście) w tym dzierżawie do aplikacji w tej dzierżawy.  Z aplikacją wielu dzierżawy aplikacji nie może ustalić na początku dzierżawy, jakie użytkownik jest, więc nie możesz wysyłać żądania do punktu końcowego dzierżawy.  Zamiast tego żądania są wysyłane do punktu końcowego, który multiplexes między dzierżawami wszystkich Azure AD:

    https://login.microsoftonline.com/common

Gdy Azure AD odbiera żądania / Common punkt końcowy, go podpisuje użytkownika i w konsekwencji zostanie odnaleziona dzierżawy, których użytkownik jest z.  / Typowych punktu końcowego działa ze wszystkimi protokoły obsługiwane przez Azure AD: łączenie OpenID, OAuth 2.0 SAML 2.0 i federacyjnych.

Zaloguj się odpowiedzi na następnie aplikacja zawiera token reprezentującą użytkownika.  Wartość wystawcy tokenu informuje aplikację dzierżawy, jakie użytkownik znajduje się z.  Jeśli odpowiedź zwraca znajdujący punkt końcowy, wartość wystawcy tokenu odpowiada dzierżawy użytkownika.  Ważne jest, aby Uwaga / Common punkt końcowy nie jest dzierżawy i nie jest wystawcy, jest po prostu multiplekser.  Gdy używasz/Common, reguł w aplikacji, aby sprawdzić poprawność tokeny należy można zaktualizować to brać pod uwagę. 

Jak wspomniano wcześniej, aplikacje wielu dzierżawy również powinien zawierać spójny wygląd logowania dla użytkowników, po zastosowaniu Azure AD związane ze znakowaniem wskazówki. Kliknij przycisk poniżej, aby dowiedzieć się więcej o znakowaniu aplikacji.

[! [Zaloguj się przycisk] [AAD-logowania]][AAD-App-Branding]

Spójrzmy na wykorzystanie / Common punktu końcowego i implementacji kodu bardziej szczegółowo.

## <a name="update-your-code-to-handle-multiple-issuer-values"></a>Aktualizacji kodu do obsługi wielu wartości wystawcy
Aplikacji sieci Web oraz interfejsy API sieci web otrzymasz i sprawdź poprawność tokenów z Azure AD.  

> [AZURE.NOTE] Podczas aplikacje klienckie natywnych żądania i otrzymywania tokenów z usługi Azure Active Directory, zrobią to, aby wysłać je do interfejsów API, gdzie mają oni.  Macierzyste aplikacje nie sprawdzaj tokeny i należy je traktować jako nieprzezroczysta.

Przyjrzyjmy się dostępnym w sposób tokenów przez aplikację otrzymuje z usługi Azure Active Directory.  Aplikacja jednej dzierżawy zajmie zwykle wartość punktu końcowego, takie jak:

    https://login.microsoftonline.com/contoso.onmicrosoft.com

i go używać do utworzenia adresu URL metadanych (w tym przypadku łączenie OpenID), takich jak:

    https://login.microsoftonline.com/contoso.onmicrosoft.com/.well-known/openid-configuration

Aby pobrać dwóch najistotniejsze informacji, które są używane do sprawdzania poprawności tokenów: dzierżawy podpisania klucze i wartości wystawcy.  Każdy dzierżawy Azure AD występują wartości unikatowe wystawcy formularza:

    https://sts.windows.net/31537af4-6d77-4bb9-a681-d2394888ea26/

miejsce, w którym wartość identyfikator GUID jest wersja bezpiecznych Zmień nazwę identyfikator dzierżawy dzierżawy.  Jeśli klikniesz łącze metadanych powyżej dla `contoso.onmicrosoft.com`, zobaczysz wartość wystawcy w dokumencie.

Gdy aplikacji jednej dzierżawy sprawdza tokenu, sprawdza podpis token przed klucze podpisywania z dokumentu metadanych i służy do sprawdzenia, czy wartość wystawcy tokenu odpowiada ten, który został znaleziony w dokumencie metadanych.

Od / Common punktu końcowego nie odpowiada dzierżawy i nie jest wystawcy zbadać wartość wystawcy w metadanych dla / typowych ma adres URL szablonu zamiast rzeczywista wartość:

    https://sts.windows.net/{tenantid}/

W związku z tym, aplikacją wielu dzierżawy nie może sprawdzić poprawności tokenów tylko przez zgodne z wartością wystawcy w metadanych z `issuer` wartość token.  Stosowanie wielu dzierżawy musi logiczny określanie wartości wystawcy, które są prawidłowe i których nie, opartych na dzierżawy identyfikator część wartości wystawcy.  

Na przykład jeśli aplikację wielu dzierżawy tylko zezwala logowania z określonym dzierżaw, którzy zalogowali ich usługi, następnie powinien sprawdzić wartość wystawcy lub `tid` rościć sobie wartości w token, aby się upewnić, że dzierżawy się na jego liście subskrybentów.  Jeśli aplikacji wielu dzierżawy tylko dotyczy osoby, a nie decyzje dowolnego programu access oparte na dzierżaw, następnie ją zignorować wartość wystawcy całkowicie.

W próbkach wielu dzierżawy, które znajdziesz w sekcji [Powiązane zawartość](#related-content) na końcu tego artykułu sprawdzania poprawności wystawcy jest wyłączone, aby włączyć wszelkie dzierżawy Azure AD, aby się zalogować.

Teraz Przyjrzyjmy się środowisko użytkownika dla użytkowników, którzy są logowanie się do aplikacji wielu dzierżawy.

## <a name="understanding-user-and-admin-consent"></a>Opis użytkowników i zgody administratora
W przypadku użytkownika do zalogowania się do aplikacji w Azure AD aplikacji musi być reprezentowana w dzierżawie użytkownika.  Dzięki temu organizacji do wykonywania operacji, takich jak stosowanie zasad unikatowe, gdy użytkownicy z ich dzierżawy logować się do aplikacji.  Stosowania jednego dzierżawy tej rejestracji jest proste; to, w którym się dzieje podczas rejestrowania aplikacji w [portal Azure klasyczny][AZURE-classic-portal].

Stosowania wielu dzierżawy pierwszej rejestracji aplikacji znajdują się w dzierżawie Azure AD używane przez dewelopera.  Gdy użytkownik z różnych dzierżawy zaloguje się do aplikacji po raz pierwszy, Azure AD monituje o wybranie wyraża zgodę na uprawnienia wymagane przez aplikację.  Jeśli one zgodę postać wniosku o nazwie *głównej usługi* jest tworzony w dzierżawie użytkownika i zaloguj się może kontynuować. Delegowanie również zostanie utworzona w katalogu, w którym rekordów zgody użytkownika do aplikacji. Zobacz [obiekty aplikacji i usług kapitału] [ AAD-App-SP-Objects] szczegółowe informacje na temat aplikacji aplikacji i ServicePrincipal obiekty i jak są powiązane ze sobą.

![Wyraża zgodę na jednowarstwowych aplikacji][Consent-Single-Tier] 

To doświadczenie zgody dotyczy uprawnienia wymagane przez aplikację.  Azure AD obsługuje dwa typy uprawnień tylko do aplikacji i delegowanej:

- Delegowanych uprawnień udziela aplikacji wykonać możliwość działania jako zalogowania użytkownika dla podzbioru czynności użytkownika.  Na przykład może udzielić aplikacji delegowane uprawnienia do odczytu kalendarz zalogowania użytkownika.
- Bezpośrednio do tożsamości aplikacji jest udzielić uprawnienia tylko do aplikacji.  Na przykład aplikacja może udzielić tylko do aplikacji uprawnienia do odczytu listy użytkowników w dzierżawie i będzie mógł w tym niezależnie od tego, kto jest zalogowany do aplikacji.

Niektóre uprawnienia można zgodę na przez zwykłego użytkownika, a inne wymagają zgody administratorem dzierżawy. 

### <a name="admin-consent"></a>Zgody administratora
Uprawnienia tylko do aplikacji zawsze wymagają zgody administratorem dzierżawy.  Jeśli aplikacja żądania uprawnień tylko do aplikacji i zwykły użytkownik próbuje zalogować się do aplikacji, aplikacja otrzymają komunikat o błędzie informujący, że użytkownik nie powiedzie się zgodę.

Niektóre delegowane uprawnienia wymagają również zgody administratorem dzierżawy.  Na przykład możliwość zapisania z powrotem do Azure AD jako zalogowania użytkownika wymaga zgody administratorem dzierżawy.  Przykład uprawnienia tylko do aplikacji Jeśli zwykłego użytkownika próbuje zalogować się do aplikacji, która zażąda delegowanej uprawnienie, które wymaga zgody administratora aplikacji wystąpi błąd.  Czy wymaga uprawnień administratora zgody jest określona przez projektanta publikowane zasobu, a następnie można znaleźć w dokumentacji dla zasobu.  Łącza do tematów opisujących uprawnienia dostępne dla interfejsu API programu Microsoft Graph i interfejsu API Azure AD wykresu znajdują się w sekcji [Powiązane zawartość](#related-content) tego artykułu.

Jeśli aplikacja używa uprawnień, które wymagają zgody administratora, należy następująco skonfigurować gest w aplikacji, takich jak przycisk lub łącze, gdy administrator może inicjowania akcji.  Żądanie aplikacji wysyła dla ta akcja jest zwykle łączenie uwierzytelnianie OAuth2-OpenID żądanie autoryzacji, ale również zawierającego `prompt=admin_consent` parametru ciągu kwerendy.  Gdy administrator wyraziła zgodę i wystawcy usługi jest tworzona w dzierżawie klienta, nie ma potrzeby logowania kolejne żądania `prompt=admin_consent` parametru.   Ponieważ administrator postanowił, że wymagane uprawnienia są dopuszczalne, nie innych użytkowników w dzierżawie zostanie wyświetlony monit o zgodę od tego momentu.

`prompt=admin_consent` Parametru również mogą być używane przez aplikacje żądające uprawnień, które nie wymagają zgody administratora, ale chcesz przekazać środowisko, w którym administrator dzierżawy "zarejestrowaniu", na podstawie jeden raz, a inni użytkownicy monit o wyrażenie zgody od tego momentu na.

Jeśli aplikacja wymaga zgody administratora i administrator znaki w aplikacji, ale `prompt=admin_consent` parametru nie jest wysyłana, administrator będzie mógł pomyślnie wyraża zgodę na aplikacji, ale tylko będzie wyraża zgodę dla swojego konta użytkownika.  Zwykli użytkownicy będą nadal nie można się zalogować i wyraża zgodę na aplikacji.  To jest przydatne, jeśli chcesz nadać administrator dzierżawy możliwość Eksplorowanie aplikacji przed innymi użytkownikami zezwolenia na dostęp.

Administrator dzierżawy można wyłączyć funkcji zwykła użytkownikom wyraża zgodę na aplikacje.  Jeśli ta funkcja jest wyłączona, administrator zgody jest zawsze wymagane na potrzeby aplikacji można skonfigurować w dzierżawie.  Jeśli chcesz przetestować aplikację z zgody zwykłego użytkownika wyłączone, możesz znaleźć Przełącz konfiguracji w dzierżawie Azure AD sekcji konfiguracji [portal Azure klasyczny][AZURE-classic-portal].

> [AZURE.NOTE] Niektóre aplikacje mają środowisko, w którym zwykli użytkownicy będą mogli początkowo zgoda, a później, aplikacja może wymagać administrator i żądania uprawnień, które wymagają zgody administratora.  Nie istnieje sposób w tym rejestracji jednej aplikacji w Azure AD dzisiaj.  Informacje o nadchodzących Azure AD końcowy w wersji 2, aby umożliwić aplikacjom zażądaj uprawnień w czasie wykonywania, zamiast w momencie rejestracji, które umożliwią w tym scenariuszu.  Aby uzyskać więcej informacji, zobacz [Przewodnik dewelopera w wersji 2 Azure AD aplikacji modelu][AAD-V2-Dev-Guide].

### <a name="consent-and-multi-tier-applications"></a>Zgody i wielu aplikacji
Aplikacja może mieć wiele poziomów, każda reprezentowana przez własnej rejestracji w Azure AD.  Na przykład natywnej aplikacji, która wywołuje interfejs API sieci web lub aplikacji sieci web wywołującym interfejsu API sieci web.  W obu przypadkach (natywnej aplikacji lub aplikacji sieci web) zażąda uprawnienia do połączeń zasobów (interfejsu API sieci web).  Aby klient się pomyślnie zgodę w dzierżawie klienta wszystkie zasoby, do których żądania uprawnień musi już istnieć w dzierżawa klienta.  Jeśli ten warunek nie jest spełniony, Azure AD zwróci błąd, że zasobu najpierw należy dodać.

Może to być problem, jeśli aplikacja logiczne składa się z dwóch lub więcej rejestracji aplikacji, na przykład osobnych klienta i zasobów.  Jak można uzyskać zasobu do dzierżawy klienta pierwszy?  Azure AD obejmuje sprawy, umożliwiając klienta i zasobów być zgodę w jednym kroku, gdzie zostanie wyświetlonych całkowita suma uprawnień zlecone przez klienta i zasobów na stronie wyrażania zgody.  Aby włączyć to zachowanie, rejestrowanie aplikacji zasobu musi zawierać identyfikator aplikacji klienta jako `knownClientApplications` w manifeście aplikacji.  Na przykład:

    knownClientApplications": ["94da0930-763f-45c7-8d26-04d5938baab2"]

Tej właściwości można aktualizować przy użyciu zasobu [manifestu aplikacji][AAD-App-Manifest]i przedstawiono w klientami wielu wywoływanie interfejsu API sieci web próbki w sekcji [Powiązane zawartość](#related-content) na końcu tego artykułu. Na poniższym diagramie omówiono zgody dla wielu aplikacji:

![Wyraża zgodę na wielu znanych klienta aplikacji][Consent-Multi-Tier-Known-Client] 

Podobne sprawy się dzieje, gdy różnych poziomów aplikacji są rejestrowane w różnych dzierżaw.  Na przykład można rozważyć w przypadku tworzenia aplikacji klientami, która wymaga usługi Office 365 Online interfejs API programu Exchange.  Aby przygotować macierzystego aplikacji lub nowszy dla natywnej aplikacji do uruchamiania w dzierżawie klienta, należy kapitału usługi Exchange Online.  W tym przypadku klienta musi zakupić usługę Exchange Online dla głównej ma być utworzony w ich dzierżawy usługi.  W przypadku interfejsu API utworzoną przez organizację innych niż Microsoft developer interfejsu API musi umożliwiają użytkownikom na zgodę ich stosowania w dzierżawie klienta, na przykład strona sieci web dyski zgody przy użyciu mechanizmu opisane w tym artykule.  Po utworzeniu wystawcy usługi w dzierżawie natywnej aplikacji można uzyskać API tokeny.

Na poniższym diagramie omówiono zgody dla aplikacji wielu zarejestrowane w różnych dzierżaw:

![Wyraża zgodę na wielu wielostronnej aplikacji][Consent-Multi-Tier-Multi-Party] 

### <a name="revoking-consent"></a>Odwoływanie zgody
Użytkownikom i administratorom odebrać zgody aplikacji w dowolnym momencie:

- Użytkownicy odebrać dostęp do poszczególnych aplikacji przez usunięcie ich z [Aplikacji programu Access Panel] [ AAD-Access-Panel] listy.
- Administratorzy odebrać dostęp do aplikacji, usuwając z Azure AD przy użyciu sekcji Zarządzanie Azure AD [portal Azure klasyczny][AZURE-classic-portal].

Jeśli administrator wyraża zgodę na aplikacji dla wszystkich użytkowników w dzierżawie, użytkownicy nie mogą odebrać dostęp pojedynczo.  Tylko administrator odebrać dostęp i tylko dla całej aplikacji.

### <a name="consent-and-protocol-support"></a>Obsługa protokołu i zgody
Zgody jest obsługiwana w Azure AD przy użyciu OAuth, OpenID połączyć, federacyjnych i protokoły SAML.  Nie obsługują protokoły SAML i federacyjnych `prompt=admin_consent` parametr, więc zgody administratora jest możliwe przy użyciu OAuth i łączenie OpenID tylko.

## <a name="multi-tenant-applications-and-caching-access-tokens"></a>Aplikacje wielu dzierżawy i buforowanie tokeny dostępu
Aplikacje wielu dzierżawy można również uzyskać tokeny dostępu, aby nawiązać połączenie interfejsów API, które są chronione za pomocą Azure AD.  Typowych błędów w przypadku biblioteki uwierzytelniania Active Directory (ADAL) za pomocą aplikacji dzierżawy wielu wstępnie żądania tokenu użytkownika przy użyciu/Common, otrzymujesz odpowiedź i Utwórz żądanie kolejnych tokenu dla tego samego użytkownika używającemu/Common.  Ponieważ pochodzi z dzierżawy, nie odpowiedź Azure AD/wspólne ADAL buforowanie token napisane z dzierżawy. Kolejne połączenie/Common uzyskać token dostępu dla użytkownika chybień wpis pamięci podręcznej i monit o Zaloguj się ponownie.  Aby uniknąć Brak pamięci podręcznej, upewnij się, że wezwań do już zalogowania użytkownika są przystosowane do punktu końcowego dzierżawy.

## <a name="related-content"></a>Zawartość pokrewna

- [Stosowanie wielu dzierżawy próbki][AAD-Samples-MT]
- [Znakowanie wskazówki dla aplikacji][AAD-App-Branding]
- [Przewodnik programisty usługi Azure AD][AAD-Dev-Guide]
- [Obiekty aplikacji i usług kapitału][AAD-App-SP-Objects]
- [Integracja aplikacji z usługą Azure Active Directory][AAD-Integrating-Apps]
- [Omówienie Framework zgody][AAD-Consent-Overview]
- [Program Microsoft Graph interfejsu API zakresów uprawnień][MSFT-Graph-AAD]
- [Azure AD wykresu interfejsu API zakresów uprawnień][AAD-Graph-Perm-Scopes]

Użyj sekcji komentarze Disqus poniżej, aby przesyłanie opinii i Pomóż nam Uściślanie i kształtowanie naszych zawartości.

<!--Reference style links IN USE -->
[AAD-Access-Panel]:  https://myapps.microsoft.com
[AAD-App-Branding]: ./active-directory-branding-guidelines.md
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Consent-Overview]: ./active-directory-integrating-applications.md#overview-of-the-consent-framework
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Overview]: https://azure.microsoft.com/en-us/documentation/articles/active-directory-graph-api/
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Samples-MT]: https://azure.microsoft.com/documentation/samples/?service=active-directory&term=multitenant
[AAD-Why-To-Integrate]: ./active-directory-how-to-integrate.md
[AZURE-classic-portal]: https://manage.windowsazure.com
[MSFT-Graph-AAD]: https://graph.microsoft.io/en-us/docs/authorization/permission_scopes

<!--Image references-->
[AAD-Sign-In]: ./media/active-directory-devhowto-multi-tenant-overview/sign-in-with-microsoft-light.png
[Consent-Single-Tier]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-single-tier.png
[Consent-Multi-Tier-Known-Client]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-multi-tier-known-clients.png
[Consent-Multi-Tier-Multi-Party]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-multi-tier-multi-party.png

<!--Reference style links -->
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AAD-Graph-User-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity
[AAD-How-To-Integrate]: ./active-directory-how-to-integrate.md
[AAD-Security-Token-Claims]: ./active-directory-authentication-scenarios/#claims-in-azure-ad-security-tokens
[AAD-Tokens-Claims]: ./active-directory-token-and-claims.md
[AAD-V2-Dev-Guide]: ./active-directory-appmodel-v2-overview.md
[AZURE-classic-portal]: https://manage.windowsazure.com
[Duyshant-Role-Blog]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/
[JWT]: https://tools.ietf.org/html/draft-ietf-oauth-json-web-token-32
[O365-Perm-Ref]: https://msdn.microsoft.com/en-us/office/office365/howto/application-manifest
[OAuth2-Access-Token-Scopes]: https://tools.ietf.org/html/rfc6749#section-3.3
[OAuth2-AuthZ-Code-Grant-Flow]: https://msdn.microsoft.com/library/azure/dn645542.aspx
[OAuth2-AuthZ-Grant-Types]: https://tools.ietf.org/html/rfc6749#section-1.3 
[OAuth2-Client-Types]: https://tools.ietf.org/html/rfc6749#section-2.1
[OAuth2-Role-Def]: https://tools.ietf.org/html/rfc6749#page-6
[OpenIDConnect]: http://openid.net/specs/openid-connect-core-1_0.html
[OpenIDConnect-ID-Token]: http://openid.net/specs/openid-connect-core-1_0.html#IDToken














