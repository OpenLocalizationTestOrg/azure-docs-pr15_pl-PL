
<properties
   pageTitle="Scenariusze uwierzytelniania Azure AD | Microsoft Azure"
   description="Omówienie pięciu najbardziej typowe scenariusze uwierzytelniania dla Azure Active Directory (AAD)"
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin"/>

# <a name="authentication-scenarios-for-azure-ad"></a>Scenariusze uwierzytelniania Azure AD

Azure Active Directory (Azure AD) ułatwia uwierzytelniania dla deweloperów, zapewniając tożsamości jako źródła usługi z obsługą standardowych protokołów, takich jak OAuth 2.0 i łączenie OpenID, a także otwieranie bibliotek dla różnych platform ułatwiające rozpoczęcie kodowania szybko. Ten dokument pomaga opis różnych obsługuje scenariusze Azure AD i procedurach pokazano, jak rozpocząć pracę. Jest on podzielony na następujące sekcje:

- [Podstawowe informacje o uwierzytelniania w Azure AD](#basics-of-authentication-in-azure-ad)

- [Roszczeń w tokenów zabezpieczających Azure AD](#claims-in-azure-ad-security-tokens)

- [Podstawowe informacje o rejestrowaniu aplikacji w Azure AD](#basics-of-registering-an-application-in-azure-ad)

- [Typy aplikacji i scenariusze](#application-types-and-scenarios)

  - [Przeglądarki sieci Web do aplikacji sieci Web](#web-browser-to-web-application)

  - [Pojedyncza strona aplikacji (SPA)](#single-page-application-spa)

  - [Natywnej aplikacji do interfejsu API sieci Web](#native-application-to-web-api)

  - [Aplikacja sieci Web do interfejsu API sieci Web](#web-application-to-web-api)

  - [Demon lub aplikacji serwera interfejsu API sieci Web](#daemon-or-server-application-to-web-api)



## <a name="basics-of-authentication-in-azure-ad"></a>Podstawowe informacje o uwierzytelniania w Azure AD

Jeśli użytkownik nie zna podstawowe pojęcia uwierzytelniania w Azure AD, przeczytaj tę sekcję. W przeciwnym razie można przejść do pozycji [Typy aplikacji i scenariuszy](#application-types-and-scenarios).

Przypatrzmy najbardziej podstawowy scenariusz, których tożsamości jest wymagane: użytkownika w przeglądarce sieci web wymaga do uwierzytelniania aplikacji sieci web. W tym scenariuszu opisano bardziej szczegółowo w sekcji [Przeglądarki sieci Web do aplikacji sieci Web](#web-browser-to-web-application) , ale jest przydatne punkt początkowy, aby przedstawić możliwości Azure AD i conceptualize, jak działa tego scenariusza. Należy rozważyć następujące diagramu w tym scenariuszu:

![Omówienie logowania się w aplikacji sieci web](./media/active-directory-authentication-scenarios/basics_of_auth_in_aad.png)

Diagram powyżej pamiętać, Oto, co należy wiedzieć o jego różne składniki:

- Azure AD jest dostawcy tożsamości odpowiedzialny za weryfikowanie tożsamości użytkowników i aplikacje, które znajdują się w katalogu organizacji i ostatecznie wydawania tokenów zabezpieczających po pomyślnym uwierzytelnieniu tych użytkowników i aplikacji.


- Aplikacja, z której chce zewnętrzny uwierzytelniania Azure AD muszą być zarejestrowane w Azure AD, który rejestruje i jednoznacznie identyfikuje w katalogu aplikacji.


- Deweloperzy mogą używać biblioteki uwierzytelniania Otwórz źródło Azure AD, aby ułatwić uwierzytelniania przez obsługi szczegóły protokołu. Aby uzyskać więcej informacji, zobacz [Azure Active Directory uwierzytelniania bibliotek](active-directory-authentication-libraries.md) .


• Po uwierzytelnieniu użytkownika, aplikacja musi zostać sprawdzony tokenu zabezpieczającego użytkownika, aby upewnić się, że uwierzytelnianie zakończyło się pomyślnie zamierzonego stron. Deweloperzy mogą używać biblioteki uwierzytelniania przedstawionych obsługę poprawności dowolny token z usługi Azure Active Directory, tym JSON tokeny sieci Web (JWT) lub SAML 2.0. Jeśli chcesz ręcznie sprawdzana poprawność, zapoznaj się z dokumentacją [Programu obsługi tokenów JWT](https://msdn.microsoft.com/library/dn205065.aspx) .


> [AZURE.IMPORTANT] Azure AD używa szyfrowania kluczem publicznym do podpisania tokeny i sprawdź, czy są prawidłowe. Zobacz temat [Ważnych informacji o logowaniu klucza przerzucania w Azure AD](active-directory-signing-key-rollover.md) , aby uzyskać więcej informacji na potrzeby logiczny musi mieć w aplikacji w celu zapewnienia, że zawsze jest zaktualizowany przy użyciu najnowszych klawiszy.


• Przepływu zaproszenia i odpowiedzi w procesie uwierzytelniania jest określona przez protokół uwierzytelniania, który był używany, takich jak 2.0 OAuth OpenID połączyć, federacyjnych lub SAML 2.0. W bardziej szczegółowo w tym temacie [Azure Active Directory protokoły](active-directory-authentication-protocols.md) i w poniższych sekcjach omówiono te protokoły.

> [AZURE.NOTE] Azure AD obsługuje OAuth 2.0 i łączenie OpenID standardy składających rozległa wykorzystanie tokeny okaziciela, łącznie z tokeny okaziciela przedstawione w postaci JWTs. Token okaziciela jest tokenu zabezpieczającego lightweight, którego udziela dostępu "okaziciela" do chronionego zasobu. W tym znaczeniu "okaziciela", to każda strona, która może prezentować tokenu. Jeśli przyjęcie najpierw musi uwierzytelniania z usługą Azure Active Directory, aby otrzymywać token okaziciela, jeśli nie są podjąć kroki wymagane w celu bezpiecznego token na przekazywanie i miejsca do magazynowania, można przechwycone i używane przez niezamierzonych firmy. Gdy niektóre tokenów zabezpieczających ma wbudowane mechanizm uniemożliwia nieupoważnione korzystania z nich, tokeny okaziciela nie ma ten mechanizm i musi transportu w bezpiecznego kanału, takie jak transportu warstwy zabezpieczeń (HTTPS). Jeśli token okaziciela są przesyłane w Wyczyść, mężczyzna w środkowym atakiem mogą być używane przez złośliwy przyjęcie uzyskać tokenu i używać go uzyskania nieautoryzowanego dostępu do chronionych zasobów. Te same zasady zabezpieczeń Zastosuj podczas przechowywania lub pamięci podręcznej tokenów okaziciela do późniejszego użycia. Zawsze upewnij się, że aplikacja przesyła i przechowuje tokeny okaziciela w bezpieczny sposób. Aby uzyskać więcej zagadnienia dotyczące zabezpieczeń na okaziciela tokenów zobacz [RFC 6750 sekcji 5](http://tools.ietf.org/html/rfc6750).


Teraz, gdy masz omówiono podstawy więcej poniższych sekcjach, aby zrozumieć, jak działa inicjowania obsługi administracyjnej w Azure AD i obsługuje typowe scenariusze Azure AD.


## <a name="claims-in-azure-ad-security-tokens"></a>Roszczeń w tokenów zabezpieczających Azure AD

Wystawiony przez Azure AD tokenów zabezpieczających zawierają roszczenia i potwierdzeń informacje dotyczące tematu, który uwierzytelnieniu. Roszczenia te mogą być używane przez aplikację dla określonych zadań. Na przykład ta osoba może służyć do sprawdzania poprawności tokenu, identyfikowania podmiotu katalogu dzierżawy, wyświetlać informacje o użytkowniku, określić autoryzacji podmiotu i tak dalej. Roszczeń w dowolny token zabezpieczeń zależą od typu token, typ poświadczenia używane do uwierzytelnienia użytkownika i konfiguracji aplikacji. Krótki opis każdego typu roszczeń dostarczanych przez Azure AD znajduje się w poniższej tabeli. Aby uzyskać więcej informacji zobacz [obsługiwane Token i typów oświadczeń](active-directory-token-and-claims.md).


| Roszczeń | Opis |
|-------|-------------|
| Identyfikator aplikacji | Służy do identyfikowania aplikacji, która korzysta z tokenu.
| Grupy odbiorców | Służy do identyfikowania adresatów tokenu jest przeznaczony dla zasobu. |
| Aplikacja uwierzytelniania kontekstu zajęć | Wskazuje, jak klient został uwierzytelniony (publicznej klient a poufne klienta). |
| Błyskawiczne uwierzytelniania | Rekordy, Data i godzina wystąpienia uwierzytelniania. |
| Metody uwierzytelniania | Wskazuje, jak temat token został uwierzytelniony (hasło, certyfikat itp.). |
| Imię | Udostępnia podanej nazwie użytkownika określonych w Azure AD. |
| Grupy | Zawiera obiekt identyfikatorów Azure AD grup, do których użytkownik jest członkiem. |
| Dostawcy tożsamości | Rekordów dostawcy tożsamości, który uwierzytelniony temat tokenu. |
| Wydane w | Rejestruje czas co token został wystawiony, często używany do tokenu aktualność. |
| Wystawcy | Służy do identyfikowania usługi STS, które emisji token, a także dzierżawy Azure AD. |
| Nazwisko | Zawiera nazwisko użytkownika określonych w Azure AD. |
| Nazwa | Udostępnia ludzi czytelne wartość identyfikująca temat tokenu. |
| Identyfikator obiektu | Zawiera niezmienne, unikatowy identyfikator tematu w Azure AD. |
| Role | Zawiera przyjaznej nazwy Azure AD ról aplikacji, których przypisano użytkownika. |
| Zakres | Wskazuje uprawnienia do aplikacji klienckiej. |
| Temat | Wskazuje wartość kapitału o tokenu potwierdza informacji. |
| Identyfikator dzierżawy | Zawiera niezmienne, unikatowy identyfikator której wystawiony token dzierżawy katalogu. |
| Okres ważności tokenu | Określa przedział czasu, w którym tokenu jest prawidłowy. |
| Głównej nazwy użytkownika | Zawiera głównej nazwy użytkownika tematu. |
| Wersja | Zawiera numer wersji tokenu. |


## <a name="basics-of-registering-an-application-in-azure-ad"></a>Podstawowe informacje o rejestrowaniu aplikacji w Azure AD

Dowolnej aplikacji, która outsources uwierzytelniania Azure AD muszą być zarejestrowane w katalogu. Ten krok polega na sekretu Azure AD o aplikacji, w tym adres URL go zawiera lokalizację, adres URL do wysyłania odpowiedzi po uwierzytelnieniu URI do identyfikowania aplikacji i nie tylko. Te informacje są wymagane kilka najważniejszych powodów:

- Azure AD musi współrzędne komunikowanie się z aplikacji podczas obsługi logowania jednokrotnego lub wymianę tokeny. Dotyczy to następujących czynności:

  - Identyfikator URI identyfikator aplikacji: Identyfikator aplikacji. Ta wartość jest wysyłany do Azure AD podczas uwierzytelniania Aby wskazać, które aplikacje wywołujący chce tokenu do. Ponadto ta wartość jest dołączany do tokenu tak, aby aplikacja wie, że został on docelową.


  - Odpowiadanie adres URL i przekierować URI: w przypadku interfejsu API sieci web lub aplikacji sieci web, odpowiedź jest używany adres URL lokalizacji, do której Azure AD wyśle odpowiedzi uwierzytelniania, w tym tokenu, jeśli uwierzytelnianie zakończyło się pomyślnie. W przypadku natywnej aplikacji przekierowywać URI jest unikatowy identyfikator, do którego Azure AD przekieruje agenta użytkownika w żądaniu OAuth 2.0.


  - Identyfikator klienta: Identyfikator aplikacji, która jest generowany przez Azure AD, gdy aplikacja jest zarejestrowany. Żądające token lub kodu autoryzacji identyfikator klienta i klawiszy są wysyłane do Azure AD podczas uwierzytelniania.


  - Klucz: Klucz, który są wysyłane wraz z identyfikator klienta podczas uwierzytelniania do Azure AD połączenie interfejsu API sieci web.


- Azure AD należy upewnić się, że aplikacja ma wymagane uprawnienia dostępu do danych katalogu innych aplikacji w Twojej organizacji i tak dalej

Inicjowania obsługi administracyjnej staje się bardziej przejrzystym sposobem po zapoznaniu się, że istnieją dwie kategorie aplikacji, które mogą być rozwinięte, a zintegrowany z usługą Azure Active Directory:

- Pojedynczy aplikacji dzierżawy: aplikacji jednej dzierżawy jest przeznaczony do użytku w jednej organizacji. Są to zazwyczaj z LOB (LoB) aplikacje napisane przez dewelopera przedsiębiorstwa. Aplikacja jednej dzierżawy tylko musi być dostępne dla użytkowników z jednego katalogu, a w wyniku tylko musi być przygotowana z jednego katalogu. Te aplikacje zwykle są rejestrowane przez dewelopera w organizacji.


- Stosowanie wielu dzierżawy: aplikacji wielu dzierżawy jest przeznaczony do użytku w wielu organizacjach nie jednej organizacji. Są to zazwyczaj oprogramowania jako z usługi (władz akredytacji bezpieczeństwa) aplikacje napisane przez niezależny dostawca oprogramowania (Model). Aplikacje wielu dzierżawy muszą być przygotowana w każdym katalogu miejsce, w którym będą używane, wymaga zgody użytkownika lub administratora, aby zarejestrować je. Ten proces zgody rozpoczyna się po aplikacji zostało zarejestrowane w katalogu i otrzymuje dostęp do interfejsu API wykresu lub być może inny interfejsu API sieci web. Gdy użytkownik lub administrator z innej organizacji zarejestrowaniu korzystania z aplikacji, są przedstawione przy użyciu okna dialogowego, wyświetlającego aplikacja wymaga uprawnienia. Użytkownik lub administrator następnie można wyraża zgodę na aplikacji, która zapewnia dostęp do aplikacji do podanej danych, a na koniec rejestruje aplikacji w ich katalogu. Aby uzyskać więcej informacji zobacz [Omówienie Framework zgodę](active-directory-integrating-applications.md#overview-of-the-consent-framework).

Zagadnienia dodatkowe mogą pojawić się podczas tworzenia aplikacji wielu dzierżawy, zamiast aplikacji jednej dzierżawy. Na przykład jeśli udostępniasz aplikacji do użytkowników w wielu katalogach, należy mechanizmu, aby określić, które dzierżawy się znajdują. Aplikacja jednej dzierżawy musi tylko Szukaj w osobnym katalogu dla użytkownika, a aplikacją dzierżawy wielu należy zidentyfikować określonego użytkownika ze wszystkich katalogów w Azure AD. Do wykonania tego zadania, Azure AD zawiera typowe punktu końcowego uwierzytelniania miejsce, w którym dowolnej aplikacji wielu dzierżawy można kierować żądania zalogowania, a nie punkt końcowy specyficzne dla dzierżawy. Ten punkt końcowy jest https://login.microsoftonline.com/common dla wszystkich katalogów Azure AD punkt końcowy specyficzne dla dzierżawy mogą być https://login.microsoftonline.com/contoso.onmicrosoft.com. Punkt końcowy typowych jest szczególnie ważne brać pod uwagę podczas tworzenia aplikacji, ponieważ musisz logiki niezbędne do obsługi wielu dzierżaw podczas logowania, wyrejestrowywaniem i token sprawdzania poprawności.

Jeśli obecnie opracowywania aplikacji jednej dzierżawy, ale chcesz udostępnić wiele organizacji, łatwo umożliwia zmiany aplikacji i jego konfiguracji Azure AD, aby nadać mu dzierżawy wielu stanie. Ponadto Azure AD używa tego samego klucza podpisywania dla wszystkich tokenów we wszystkich katalogach, czy mają być udostępniane uwierzytelniania w dzierżawy jednego lub wielu dzierżawy aplikacji.

Każdy scenariusz wymienione w tym dokumencie obejmuje podrzędny sekcji opisującą jego wymagania dotyczące obsługi administracyjnej. Aby uzyskać więcej szczegółowych informacji o inicjowania obsługi administracyjnej aplikacji w Azure AD i różnice między aplikacjami jednej lub wielu dzierżawy zobacz [Integracji aplikacji z usługą Azure Active Directory](active-directory-integrating-applications.md) Aby uzyskać więcej informacji. Kontynuuj odczytu, aby poznać typowe scenariusze aplikacji w Azure AD.

## <a name="application-types-and-scenarios"></a>Typy aplikacji i scenariusze

Czy każdego z scenariuszy opisanych w tym dokumencie utworzony przy użyciu różnych języków i platformach. Ich wszystkich kopii, przykłady kodu wykonane, które są dostępne w naszym [przewodniku przykłady kodu](active-directory-code-samples.md)lub bezpośrednio z odpowiednich [repozytoria próbki Github](https://github.com/Azure-Samples?utf8=%E2%9C%93&query=active-directory). Ponadto jeśli aplikacja wymaga określone lub część scenariusz end-to-end, w większości przypadków tej funkcji można dodawać niezależnie. Na przykład jeśli masz natywnej aplikacji, która wymaga interfejsu API sieci web, łatwe dodawanie aplikacji sieci web, która wymaga także interfejsu API sieci web. Na poniższym diagramie przedstawiono scenariuszy i typów aplikacji i w jaki sposób można dodać różne składniki:

![Typy aplikacji i scenariusze](./media/active-directory-authentication-scenarios/application_types_and_scenarios.png)

Poniżej przedstawiono pięć scenariuszy podstawowy aplikacji, obsługiwany przez Azure AD:

- [Przeglądarki sieci Web do aplikacji sieci Web](#web-browser-to-web-application): użytkownik musi się zalogować do aplikacji sieci web, która jest zabezpieczone Azure AD.

- [Pojedynczej strony aplikacji (SPA)](#single-page-application-spa): użytkownik musi się zalogować do aplikacji pojedynczej strony, która jest zabezpieczone Azure AD.

- [Natywnej aplikacji sieci Web API](#native-application-to-web-api): natywnej aplikacji, działającą z telefonu, tabletu lub komputer należy do uwierzytelnienia użytkownika uzyskiwania zasobów z sieci web interfejs API, który jest zabezpieczone Azure AD.

- [Interfejs API sieci Web aplikacji sieci Web](#web-application-to-web-api): należy uzyskać zasoby z sieci web interfejsu API zabezpieczone Azure AD aplikacji sieci web.

- [Demon lub aplikacji serwera interfejs API sieci Web](#daemon-or-server-application-to-web-api): musi uzyskać zasoby z sieci web interfejsu API zabezpieczone Azure AD aplikację demona lub aplikacji serwera bez interfejsu użytkownika sieci web.

### <a name="web-browser-to-web-application"></a>Przeglądarki sieci Web do aplikacji sieci Web

W tej sekcji opisano aplikację, która uwierzytelnia użytkownika w przeglądarce sieci web do aplikacji sieci web. W tym scenariuszu aplikacji sieci web kieruje użytkownika przeglądarki, aby się zalogować do Azure AD. Azure AD zwraca odpowiedź logowania za pośrednictwem przeglądarki użytkownika, która zawiera roszczeń dotyczących użytkownika w tokenu zabezpieczającego. W tym scenariuszu obsługuje logowania jednokrotnego za pomocą protokołów federacyjnych, SAML 2.0 i łączenie OpenID.


#### <a name="diagram"></a>Diagram
![Przepływ uwierzytelniania dla przeglądarki w aplikacji sieci web](./media/active-directory-authentication-scenarios/web_browser_to_web_api.png)


#### <a name="description-of-protocol-flow"></a>Opis przepływu Protocol (protokół)


1. Gdy użytkownik wizyty aplikacji i musi się zalogować, będą przekierowywani przy użyciu żądania logowania do punktu końcowego uwierzytelniania w Azure AD.


2. Użytkownik zaloguje się na stronie logowania.


3. Jeśli uwierzytelnianie zakończyło się pomyślnie, Azure AD tworzy token uwierzytelniania i zwraca odpowiedź logowania do adresu URL odpowiedzi aplikacji, który został skonfigurowany w portalu zarządzania Azure. W przypadku aplikacji produkcji ten adres URL odpowiedzi powinny być HTTPS. Zwrócone token zawiera oświadczeniach o użytkownika i Azure AD, które są wymagane przez aplikację do sprawdzania poprawności tokenu.


4. Aplikacja sprawdza tokenu przy użyciu podpisywania kluczem publicznym i wystawcy informacji dostępnych w Federacji dokument metadanych dla Azure AD. Po aplikacji sprawdza tokenu, Azure AD zaczyna się nową sesję użytkownika. W tej sesji umożliwia użytkownikowi dostęp do aplikacji do momentu jej wygaśnięcia.


#### <a name="code-samples"></a>Przykłady kodu


Zobacz przykłady kodu przeglądarki sieci Web, aby scenariuszy aplikacji sieci Web. I zaglądaj tu często — dodajemy nowe przykłady cały czas. [Przeglądarki sieci web do aplikacji sieci Web](active-directory-code-samples.md#web-browser-to-web-application).


#### <a name="registering"></a>Rejestrowanie


- Pojedynczy dzierżawy: Jeśli tworzysz aplikację tylko dla Twojej organizacji, musi być zarejestrowana w katalogu firmy za pomocą portalu zarządzania Azure.


- Wiele dzierżawy: Jeśli tworzysz aplikację, który może być używany przez użytkowników spoza organizacji, to muszą być zarejestrowane w katalogu firmy, ale muszą być zarejestrowane w katalogu każdej organizacji, który będzie używany w aplikacji. Aby udostępnić w ich katalogu aplikacji, możesz umieścić procesu rejestracji dla klientów, które umożliwia im wyraża zgodę na aplikacji. Po zapisaniu się aplikacji one zostaną wyświetlone okno dialogowe przedstawiający aplikacja wymaga uprawnienia, a następnie opcję wyraża zgodę. W zależności od wymagane uprawnienia administratora w innej organizacji może wymagać zgody. Gdy zgadza się użytkownika lub administratora, aplikacja jest zarejestrowana w ich katalogu. Aby uzyskać więcej informacji zobacz [Integracja aplikacji z usługą Azure Active Directory](active-directory-integrating-applications.md).


#### <a name="token-expiration"></a>Wygaśnięcia tokenu

Po wygaśnięciu ważności token wystawiony przez Azure AD wygaśnięcia sesji użytkownika. Aplikacja skrócić tego okresu, jeśli to konieczne, takich jak wylogowywanie się użytkowników oparte na pewnym okresie nieaktywności. Gdy sesja wygasa, użytkownik wyświetli monit o Zaloguj się ponownie.





### <a name="single-page-application-spa"></a>Pojedyncza strona aplikacji (SPA)

W tej sekcji opisano uwierzytelniania dla jednej aplikacji stronę, która używa Azure AD i autoryzacji niejawne OAuth 2.0 udzielić bezpiecznego jego zakończenia interfejsu API z powrotem w sieci web. Poszczególne aplikacje strony są zwykle strukturze JavaScript warstwy prezentacji (fronton), która działa w przeglądarce i interfejs API sieci Web wewnętrznej jest uruchomiony na serwerze, który wykonuje reguł biznesowych aplikacji. Aby dowiedzieć się więcej o autoryzacji niejawne grant i pomocy możesz zdecydować, czy mają dla danego scenariusza aplikacji, zobacz [Opis przepływ niejawne udzielanie uwierzytelnianie OAuth2 w usługi Azure Active Directory](active-directory-dev-understanding-oauth2-implicit-grant.md).

W tym scenariuszu gdy użytkownik zaloguje się, kod JavaScript wierzch końcowe zastosowania [Active Directory Authentication Library dla języka JavaScript (ADAL. JS)](https://github.com/AzureAD/azure-activedirectory-library-for-js/tree/dev) i Udziel autoryzacji niejawne, aby uzyskać identyfikator tokenu (id_token) z usługi Azure Active Directory. Token jest przechowywanych w pamięci podręcznej i klienta dołącza go na żądanie jako token okaziciela podczas połączenia z jego interfejs API sieci Web wewnętrzna baza danych, które są chronione za pomocą pośredniczącym OWIN. 
#### <a name="diagram"></a>Diagram

![Pojedynczy diagram stron aplikacji](./media/active-directory-authentication-scenarios/single_page_app.png)

#### <a name="description-of-protocol-flow"></a>Opis przepływu Protocol (protokół)

1. Użytkownik przechodzi do aplikacji sieci web.


2. Aplikacja zwraca JavaScript fronton (prezentacji layer) do przeglądarki.


3. Użytkownik inicjuje logowania, na przykład, klikając pozycję Zaloguj się łącze. Przeglądarka wysyła GET do punktu końcowego autoryzacji Azure AD żądania token identyfikator. Żądanie zawiera adres URL identyfikator i odpowiedź klienta w parametry kwerendy.


4. Azure AD sprawdza adres URL odpowiedzi dla adresu URL zarejestrowanych odpowiedzi, który został skonfigurowany w portalu zarządzania Azure.


5. Użytkownik zaloguje się na stronie logowania.


6. Jeśli uwierzytelnianie zakończyło się pomyślnie, Azure AD tworzy token identyfikator i zwraca go jako fragment adresu URL (#) do adresu URL odpowiedzi aplikacji. W przypadku aplikacji produkcji ten adres URL odpowiedzi powinny być HTTPS. Zwrócone token zawiera oświadczeniach o użytkownika i Azure AD, które są wymagane przez aplikację do sprawdzania poprawności tokenu.


7. Kod klienta JavaScript działa w przeglądarce wyodrębnia tokenu z odpowiedzi za pomocą zabezpieczania połączeń w aplikacji sieci web, że zakończyć interfejsu API ponownie.


8. Przeglądarka nawiąże połączenie z aplikacji sieci web interfejsu API ponownie kończy się token dostępu w nagłówku autoryzacji.

#### <a name="code-samples"></a>Przykłady kodu


Zobacz przykłady kodu dla pojedynczej strony aplikacji (SPA) scenariuszy. Pamiętaj zaglądaj tu często — dodajemy nowe przykłady cały czas. [Pojedyncza strona aplikacji (bezpieczne uwierzytelnianie HASŁA)](active-directory-code-samples.md#single-page-application-spa).


#### <a name="registering"></a>Rejestrowanie


- Pojedynczy dzierżawy: Jeśli tworzysz aplikację tylko dla Twojej organizacji, musi być zarejestrowana w katalogu firmy za pomocą portalu zarządzania Azure.


- Wiele dzierżawy: Jeśli tworzysz aplikację, który może być używany przez użytkowników spoza organizacji, to muszą być zarejestrowane w katalogu firmy, ale muszą być zarejestrowane w katalogu każdej organizacji, który będzie używany w aplikacji. Aby udostępnić w ich katalogu aplikacji, możesz umieścić procesu rejestracji dla klientów, które umożliwia im wyraża zgodę na aplikacji. Po zapisaniu się aplikacji one zostaną wyświetlone okno dialogowe przedstawiający aplikacja wymaga uprawnienia, a następnie opcję wyraża zgodę. W zależności od wymagane uprawnienia administratora w innej organizacji może wymagać zgody. Gdy zgadza się użytkownika lub administratora, aplikacja jest zarejestrowana w ich katalogu. Aby uzyskać więcej informacji zobacz [Integracja aplikacji z usługą Azure Active Directory](active-directory-integrating-applications.md).

Po zarejestrowaniu aplikacji musi być skonfigurowany do używania protokołu udzielanie niejawne OAuth 2.0. Domyślnie ten protokół jest wyłączony dla aplikacji. Aby włączyć uwierzytelnianie OAuth2 niejawne udzielanie Protocol (protokół) dla aplikacji, Pobierz jego manifest aplikacji za pomocą portalu zarządzania Azure, ustaw wartość "oauth2AllowImplicitFlow" na wartość PRAWDA, a następnie przekaż manifestu wstecz do portalu. Aby uzyskać szczegółowe instrukcje zobacz [Włączanie OAuth 2.0 niejawne Udziel dla pojedynczej strony aplikacji](active-directory-integrating-applications.md).


#### <a name="token-expiration"></a>Wygaśnięcia tokenu

Użycie ADAL.js do zarządzania uwierzytelnianiem z usługą Azure Active Directory, możesz korzystać z kilka funkcji ułatwiających odświeżania token wygasłe, a także uzyskiwanie tokeny dla zasobów interfejsu API dodatkowe sieci web, które mogą być wymagane przez aplikację. Jeśli użytkownik pomyślnie uwierzytelnia z usługą Azure Active Directory, sesji zabezpieczone pliki cookie jest nawiązywane dla użytkownika między przeglądarką i Azure AD. Należy pamiętać, że sesja istnieją między użytkownikiem a Azure AD nie między użytkownikiem i uruchamiania na serwerze aplikacji sieci web. Po wygaśnięciu tokenu ADAL.js używa tej sesji bezgłośną uzyskać inny token. Jest to za pomocą ukrytych iFrame do wysyłania i odbierania żądania przy użyciu protokołu udzielanie niejawne OAuth. ADAL.js można także użyć ten sam mechanizm w trybie cichym uzyskać tokeny dostępu z Azure AD dla innych sieci web zasobów interfejsu API, które połączenia aplikacji jak te zasoby pomocy technicznej zasobów między origin udostępniania (CORS) są rejestrowane w katalogu użytkownika, a wszystkie wymagane zgody podano przez użytkownika podczas logowania.


### <a name="native-application-to-web-api"></a>Natywnej aplikacji do interfejsu API sieci Web


W tej sekcji opisano natywnej aplikacji, która wymaga interfejsu API sieci web w imieniu użytkownika. W tym scenariuszu jest oparty na typ OAuth 2.0 autoryzacji kod udzielanie z klientem publicznej, zgodnie z opisem w sekcji 4.1 [specyfikacji OAuth 2.0](http://tools.ietf.org/html/rfc6749). Natywnej aplikacji uzyskuje token dostępu dla użytkownika przy użyciu protokołu OAuth 2.0. Ten token dostępu są następnie wysyłane w wezwaniu do interfejsu API, która zezwala na użytkownika i zwraca żądanego zasobu w sieci web.

#### <a name="diagram"></a>Diagram

![Natywnej aplikacji do diagramu interfejsu API w sieci Web](./media/active-directory-authentication-scenarios/native_app_to_web_api.png)

#### <a name="authentication-flow-for-native-application-to-api"></a>Przepływ uwierzytelniania dla aplikacji macierzystej API

#### <a name="description-of-protocol-flow"></a>Opis przepływu Protocol (protokół)


Jeśli używasz biblioteki uwierzytelniania AD najczęściej szczegóły protokołu opisane poniżej są obsługiwane, takich jak menu podręczne przeglądarki, token pamięci podręcznej i obsługi tokenów odświeżania.

1. W przeglądarce podręcznym natywnej aplikacji powoduje, że żądania do punktu końcowego autoryzacji w Azure AD. Żądanie zawiera identyfikator klienta i Przekieruj identyfikator URI natywnej aplikacji, jak pokazano w portalu zarządzania i aplikacji identyfikator URI dla interfejsu API sieci web. Jeśli użytkownik jeszcze nie zalogowano, są monitowani o Zaloguj się ponownie


2. Azure AD uwierzytelnia użytkownika. Jeśli jest to aplikacja wielu dzierżawy i zgody jest wymagane do korzystania z aplikacji, użytkownik będą musieli zgoda, jeśli ich jeszcze tego nie zrobiono. Po udzielanie zgody i po pomyślnym uwierzytelnieniu Azure AD problemy autoryzacji kod odpowiedź z aplikacją kliencką przekierowania URI.


3. Azure AD problemy autoryzacji kod odpowiedź przekierowania URI, z aplikacją kliencką zatrzyma interakcji z przeglądarką i wyodrębnia kod autoryzacji z odpowiedzi. Przy użyciu następującego kodu autoryzacji, z aplikacją kliencką przesyła żądanie Azure AD token punktu końcowego, który zawiera kod autoryzacji, informacje o aplikacji klienckiej (identyfikator klienta i przekierowania URI) i zasobem (aplikacja identyfikator URI dla interfejsu API sieci web).


4. Kod autoryzacji i informacji na temat interfejsu API sieci web i aplikacji klienta do sprawdzania poprawności przez Azure AD. Po sprawdzeniu poprawności Azure AD zwraca dwa tokeny: token dostępu JWT i JWT tokenu odświeżania. Ponadto Azure AD zwraca podstawowe informacje o użytkowniku, takie jak ich wyświetlania nazwy i dzierżawy identyfikatora.


5. Za pomocą protokołu HTTPS z aplikacją kliencką używa zwracane token dostępu JWT dodać ciąg JWT z oznaczeniem "Okaziciela" w nagłówku autoryzacji żądania do interfejsu API sieci web. Interfejsu API sieci web sprawdza JWT token i jeśli sprawdzanie poprawności zakończyło się pomyślnie, zwraca żądanego zasobu.


6. Po wygaśnięciu token dostępu z aplikacją kliencką otrzymają komunikat o błędzie, który wskazuje, że użytkownik chce ponownie uwierzytelniania. Aplikacja ma token odświeżania prawidłowy, może służyć umożliwia uzyskanie nowego token dostępu bez monitowania użytkownika, aby zalogować się ponownie. Po wygaśnięciu token odświeżania, aplikacja będzie konieczne interakcyjnie ponownie uwierzytelnienia użytkownika.


> [AZURE.NOTE] Token odświeżania wydawany przez Azure AD można uzyskać dostęp do wielu zasobów. Na przykład jeśli masz aplikacji klienckiej, która ma uprawnienia do połączeń web dwa interfejsy API token odświeżania może służyć uzyskanie dostępu do tokenu do innych sieci web interfejsu API także.


#### <a name="code-samples"></a>Przykłady kodu


Zobacz przykłady kodu do natywnej aplikacji scenariuszy interfejs API sieci Web. I zaglądaj tu często — dodajemy nowe przykłady cały czas. [Natywnej aplikacji do interfejsu API sieci Web](active-directory-code-samples.md#native-application-to-web-api).


#### <a name="registering"></a>Rejestrowanie


- Pojedynczy dzierżawy: Obu macierzystych aplikacji i interfejsu API sieci web muszą być zarejestrowane w tym samym katalogu w Azure AD. Aby udostępnić zestaw uprawnień, które są używane do ograniczania natywnej aplikacji dostępu do jego zasobów można skonfigurować interfejsu API sieci web. Aplikacja klienta wybiera odpowiednie uprawnienia z menu rozwijanego "Uprawnienia do innych aplikacji" w portalu zarządzania Azure.


- Wiele dzierżawy: Po raz pierwszy, natywnej aplikacji tylko zarejestrowane w katalogu programu publisher lub dewelopera. Drugi natywnej aplikacji jest skonfigurowany do wskazać uprawnienia wymagane do działać. Tej listy wymagane uprawnienia są wyświetlane w oknie dialogowym, gdy użytkownik lub administrator w katalogu docelowym powoduje zgody aplikacji, która udostępnia do organizacji. Niektóre aplikacje wymagają po prostu poziomie użytkownika uprawnienia, które mogą przyznać każdy użytkownik w organizacji. Inne aplikacje wymaga uprawnienia na poziomie administratora, które użytkownik w organizacji nie wyraża zgodę na. Tylko administrator katalogów może powodować zgody aplikacji wymagających tego poziomu uprawnień. Gdy użytkownik lub administrator wyraża zgodę, tylko interfejsu API sieci web jest zarejestrowana w ich katalogu. Aby uzyskać więcej informacji zobacz [Integracja aplikacji z usługą Azure Active Directory](active-directory-integrating-applications.md).


#### <a name="token-expiration"></a>Wygaśnięcia tokenu


Gdy natywnej aplikacji używa kodu autoryzacji uzyskanie dostępu do JWT token, również otrzymuje tokenu odświeżania JWT. Po wygaśnięciu token dostępu, token odświeżania może służyć do ponownego uwierzytelnienia użytkownika bez konieczności ich tak, aby zalogować się ponownie. Ten token odświeżania następnie jest używany do uwierzytelnienia użytkownika, które powoduje nowy token dostępu i token odświeżania.





### <a name="web-application-to-web-api"></a>Aplikacja sieci Web do interfejsu API sieci Web


W tej sekcji opisano aplikacji sieci web, który należy pobrać zasoby z interfejsu API sieci web. W tym scenariuszu istnieją dwa typy tożsamości, które umożliwiają aplikacji sieci web do uwierzytelniania i połączeń interfejsu API sieci web: tożsamość aplikacji lub tożsamość użytkownika delegowanej.

*Tożsamości aplikacji:* W tym scenariuszu używa udzielanie poświadczeń klienta OAuth 2.0 w celu uwierzytelnienia jako aplikacja i uzyskać dostęp do interfejsu API sieci web. Podczas korzystania z tożsamości aplikacji sieci web interfejsu API tylko wykrywa, że wywołuje aplikacji sieci web, co sieć web interfejsu API nie otrzymuje wszelkie informacje o użytkowniku. Jeśli po otrzymaniu informacje o użytkowniku, zostanie ona wysłana za pomocą protokołu aplikacji, a nie jest podpisany przez Azure AD. Interfejsu API sieci web zaufania, że aplikacji sieci web uwierzytelnianie użytkownika. Z tego powodu tego wzorca jest nazywany zaufanych podsystemu.

*Delegowanymi tożsamość użytkownika:* W tym scenariuszu można zrobić na dwa sposoby: łączenie OpenID i udzielanie kod autoryzacji OAuth 2.0 z klientem poufne. Aplikacja sieci web uzyskuje token dostępu dla użytkownika, którego okaże się interfejsu API, że użytkownik uwierzytelniony pomyślnie dla aplikacji sieci web i aplikacji sieci web udało się uzyskać tożsamość użytkownika delegowanego połączenie interfejsu API sieci web w sieci Web. Ten token dostępu są wysyłane w wezwaniu do interfejsu API, która zezwala na użytkownika i zwraca żądanego zasobu w sieci web.

#### <a name="diagram"></a>Diagram

![Aplikacja sieci Web do diagramu interfejs API sieci Web](./media/active-directory-authentication-scenarios/web_app_to_web_api.png)



#### <a name="description-of-protocol-flow"></a>Opis przepływu Protocol (protokół)

Tożsamość aplikacji i typów tożsamości użytkownika delegowanego omówiono w procesie poniżej. Kluczowe różnice między nimi to, że tożsamość użytkownika delegowanych muszą kupić kod autoryzacji przed użytkownik może logowania i uzyskać dostęp do interfejsu API sieci web.

##### <a name="application-identity-with-oauth-20-client-credentials-grant"></a>Udzielanie poświadczeń tożsamości aplikacji z klientem OAuth 2.0

1. Użytkownik jest zalogowany Azure AD w aplikacji sieci web (zobacz [Przeglądarki sieci Web do aplikacji sieci Web](#web-browser-to-web-application) powyżej).


2. Aplikacja sieci web musi uzyskać token dostępu, który można uwierzytelnić interfejsu API sieci web i pobrać żądanego zasobu. Jest to żądanie token punktu końcowego Azure AD, podanie poświadczeń, identyfikator klienta i aplikacji sieci web interfejsu API identyfikator URI.


3. Azure AD uwierzytelnia aplikacji i zwraca token dostępu JWT, używanym do połączeń interfejsu API sieci web.


4. Za pomocą protokołu HTTPS aplikacji sieci web używa zwracane token dostępu JWT dodać ciąg JWT z oznaczeniem "Okaziciela" w nagłówku autoryzacji żądania do interfejsu API sieci web. Interfejsu API sieci web sprawdza JWT token i jeśli sprawdzanie poprawności zakończyło się pomyślnie, zwraca żądanego zasobu.

##### <a name="delegated-user-identity-with-openid-connect"></a>Łączenie tożsamość użytkownika delegowanego z OpenID

1. Użytkownik jest zalogowany do aplikacji sieci web przy użyciu Azure AD (patrz powyżej sekcja [Przeglądarki sieci Web do aplikacji sieci Web](#web-browser-to-web-application) ). Jeśli użytkownik aplikacji sieci web nie ma jeszcze zgodę na oferujący aplikacji sieci web nawiązać połączenie interfejsu API sieci web w jego imieniu, użytkownik musi zgodę. Aplikacja wyświetli wymaga uprawnienia, a jeśli dowolną z następujących uprawnień na poziomie administratora, normalnego użytkownika w katalogu nie będą mogli zgodę. Ten proces zgody dotyczy tylko wielu dzierżawy, aplikacje nie pojedynczy dzierżawy jako aplikacja będzie już masz niezbędne uprawnienia. Gdy użytkownik zalogowany aplikacji sieci web odebrana token identyfikator informacje o użytkownika, a także kod autoryzacji.


2. Za pomocą kodu autoryzacji wydawany przez Azure AD, aplikacji sieci web przesyła żądanie Azure AD token punktu końcowego, który zawiera kod autoryzacji, informacje o aplikacji klienckiej (identyfikator klienta i przekierowania URI) i zasobem (aplikacja identyfikator URI dla interfejsu API sieci web).


3. Kod autoryzacji oraz informacje dotyczące aplikacji sieci web i interfejs API sieci web do sprawdzania poprawności przez Azure AD. Po sprawdzeniu poprawności Azure AD zwraca dwa tokeny: token dostępu JWT i JWT tokenu odświeżania.


4. Za pomocą protokołu HTTPS aplikacji sieci web używa zwracane token dostępu JWT dodać ciąg JWT z oznaczeniem "Okaziciela" w nagłówku autoryzacji żądania do interfejsu API sieci web. Interfejsu API sieci web sprawdza JWT token i jeśli sprawdzanie poprawności zakończyło się pomyślnie, zwraca żądanego zasobu.

##### <a name="delegated-user-identity-with-oauth-20-authorization-code-grant"></a>Tożsamość użytkownika delegowanego z udzielanie kod autoryzacji OAuth 2.0

1. Użytkownik wcześniej zalogować się do aplikacji sieci web, w których mechanizm uwierzytelniania zależy od Azure AD.


2. Aplikacja sieci web wymaga kodu autoryzacji uzyskać token dostępu, więc emituje żądanie za pomocą przeglądarki Azure AD autoryzacji punktu końcowego, dostarczając identyfikator klienta i przekierowywanie identyfikatora URI dla aplikacji sieci web po pomyślnym uwierzytelnieniu. Użytkownik rejestruje się Azure AD.


3. Jeśli użytkownik aplikacji sieci web nie ma jeszcze zgodę na oferujący aplikacji sieci web nawiązać połączenie interfejsu API sieci web w jego imieniu, użytkownik musi zgodę. Aplikacja wyświetli wymaga uprawnienia, a jeśli dowolną z następujących uprawnień na poziomie administratora, normalnego użytkownika w katalogu nie będą mogli zgodę. Ten proces zgody dotyczy tylko wielu dzierżawy, aplikacje nie pojedynczy dzierżawy jako aplikacja będzie już masz niezbędne uprawnienia.


4. Gdy użytkownik ma zgodę, aplikacji sieci web odbiera kodu autoryzacji wymagającego umożliwia uzyskanie token dostępu.


5. Za pomocą kodu autoryzacji wydawany przez Azure AD, aplikacji sieci web przesyła żądanie Azure AD token punktu końcowego, który zawiera kod autoryzacji, informacje o aplikacji klienckiej (identyfikator klienta i przekierowania URI) i zasobem (aplikacja identyfikator URI dla interfejsu API sieci web).


6. Kod autoryzacji oraz informacje dotyczące aplikacji sieci web i interfejs API sieci web do sprawdzania poprawności przez Azure AD. Po sprawdzeniu poprawności Azure AD zwraca dwa tokeny: token dostępu JWT i JWT tokenu odświeżania.


7. Za pomocą protokołu HTTPS aplikacji sieci web używa zwracane token dostępu JWT dodać ciąg JWT z oznaczeniem "Okaziciela" w nagłówku autoryzacji żądania do interfejsu API sieci web. Interfejsu API sieci web sprawdza JWT token i jeśli sprawdzanie poprawności zakończyło się pomyślnie, zwraca żądanego zasobu.

#### <a name="code-samples"></a>Przykłady kodu

Zobacz przykłady kodu dla aplikacji sieci Web scenariuszy interfejs API sieci Web. I zaglądaj tu często — dodajemy nowe przykłady cały czas. Sieci Web [aplikacji interfejsu API sieci Web](active-directory-code-samples.md#web-application-to-web-api).


#### <a name="registering"></a>Rejestrowanie

- Pojedynczy dzierżawy: Zarówno dla tożsamości aplikacji i przypadkach tożsamość użytkownika delegowanych, aplikacji sieci web oraz interfejsu API sieci web musi być zarejestrowany w tym samym katalogu w Azure AD. Aby udostępnić zestaw uprawnień, które są używane do ograniczania dostępu aplikacji sieci web do jego zasobów można skonfigurować interfejsu API sieci web. Jeśli jest używany typ tożsamości użytkownika delegowanych, aplikacji sieci web trzeba wybierz odpowiednie uprawnienia z menu rozwijanego "Uprawnienia do innych aplikacji" w portalu zarządzania Azure. Ten krok nie jest wymagany, jeśli jest używany typ tożsamości aplikacji.


- Wiele dzierżawy: Najpierw aplikacji sieci web jest skonfigurowany do wskazać uprawnienia wymagane do działać. Tej listy wymagane uprawnienia są wyświetlane w oknie dialogowym, gdy użytkownik lub administrator w katalogu docelowym powoduje zgody aplikacji, która udostępnia do organizacji. Niektóre aplikacje wymagają po prostu poziomie użytkownika uprawnienia, które mogą przyznać każdy użytkownik w organizacji. Inne aplikacje wymaga uprawnienia na poziomie administratora, które użytkownik w organizacji nie wyraża zgodę na. Tylko administrator katalogów może powodować zgody aplikacji wymagających tego poziomu uprawnień. Gdy zgadza się użytkownika lub administratora, aplikacji sieci web oraz interfejsu API sieci web są oba zarejestrowane w ich katalogu.

#### <a name="token-expiration"></a>Wygaśnięcia tokenu

Gdy aplikacja sieci web używa kodu autoryzacji uzyskanie dostępu do JWT token, również otrzymuje tokenu odświeżania JWT. Po wygaśnięciu token dostępu, token odświeżania może służyć do ponownego uwierzytelnienia użytkownika bez konieczności ich tak, aby zalogować się ponownie. Ten token odświeżania następnie jest używany do uwierzytelnienia użytkownika, które powoduje nowy token dostępu i token odświeżania.


### <a name="daemon-or-server-application-to-web-api"></a>Demon lub aplikacji serwera interfejsu API sieci Web


W tej sekcji opisano demon lub serwera aplikacji, która musi uzyskać zasoby z interfejsu API sieci web. Istnieją dwa scenariusze podrzędne, które dotyczą w tej sekcji: demon, który musi wywołać web API, oparty na OAuth 2.0 typu udzielanie poświadczeń klienta. i aplikacji serwera (na przykład interfejsu API sieci web), która wymaga połączenia interfejsu API, oparty na OAuth 2.0 On-Behalf-Of wersja robocza Specyfikacja w sieci web.

Scenariusz aplikację demona reagować na połączenie sieci web API, należy pamiętać o kilku kwestiach. Najpierw interakcji użytkownika nie jest możliwe przy użyciu aplikacji demon, która wymaga wniosek o jego tożsamość. Przykład aplikację demona jest zadanie wsadowe lub usługa systemu operacyjnego uruchomione w tle. Ten typ aplikacji żądania token dostępu za pomocą jego tożsamości aplikacji i prezentowanie klienta identyfikator, poświadczeń (hasła lub certyfikatu) i stosowanie identyfikator URI do Azure AD. Po pomyślnym uwierzytelnieniu demona odbiera token dostępu z Azure AD, który jest następnie używany do połączeń interfejsu API sieci web.

Dla tego scenariusza aplikacji serwera reagować na połączenie sieci web API, warto użycie przykładu. Załóżmy, że użytkownik uwierzytelniony w natywnej aplikacji i ten natywnej aplikacji należy zadzwonić interfejsu API sieci web. Azure AD problemy token dostępu JWT, aby nawiązać połączenie interfejsu API sieci web. Jeśli interfejs API sieci web wymaga połączeń innej podrzędne interfejsu API sieci web, go za pomocą przepływu w imieniu z pełnomocników tożsamość użytkownika i uwierzytelniania interfejsu API sieci web podrzędne.

#### <a name="diagram"></a>Diagram

![Demon lub aplikacja do diagramu interfejs API sieci Web](./media/active-directory-authentication-scenarios/daemon_server_app_to_web_api.png)

#### <a name="description-of-protocol-flow"></a>Opis przepływu Protocol (protokół)

##### <a name="application-identity-with-oauth-20-client-credentials-grant"></a>Udzielanie poświadczeń tożsamości aplikacji z klientem OAuth 2.0

1. Najpierw aplikacji serwer wymaga uwierzytelnienia przez Azure AD jako, bez interakcji ludzi takich jak interakcyjne okno logowania jednokrotnego. Jest to żądanie tokenu punktu końcowego Azure AD, dostarczając poświadczeń, identyfikator klienta i stosowanie identyfikator URI.


2. Azure AD uwierzytelnia aplikacji i zwraca token dostępu JWT, używanym do połączeń interfejsu API sieci web.


3. Za pomocą protokołu HTTPS aplikacji sieci web używa zwracane token dostępu JWT dodać ciąg JWT z oznaczeniem "Okaziciela" w nagłówku autoryzacji żądania do interfejsu API sieci web. Interfejsu API sieci web sprawdza JWT token i jeśli sprawdzanie poprawności zakończyło się pomyślnie, zwraca żądanego zasobu.


##### <a name="delegated-user-identity-with-oauth-20-on-behalf-of-draft-specification"></a>Tożsamość użytkownika delegowanego z OAuth 2.0 Specyfikacja w imieniu z wersji roboczej

Przepływ omówiony poniżej założono, że użytkownik został uwierzytelniony w innej aplikacji (na przykład natywnej aplikacji) i jego tożsamości użytkownika został użyty do uzyskania token dostępu do interfejsu API sieci web pierwszego poziomu.

1. Natywnej aplikacji wysyła token dostępu do interfejsu API sieci web pierwszego poziomu.


2. Interfejs API sieci web pierwszego poziomu wysyła żądanie token punktu końcowego Azure AD adres klienta, identyfikator i poświadczenia, a także tokenu dostępu. Ponadto żądanie jest wysyłane z on_behalf_of parametr, który wskazuje sieci web interfejsu API żąda nowe tokeny połączenie podrzędne interfejsu API sieci web imieniu oryginalnej użytkownika.


3. Azure AD sprawdza, czy interfejsu API sieci web pierwszego poziomu uprawnień dostępu do sieci web podrzędne interfejsu API oraz weryfikowanych przez żądanie, zwracanie token dostępu JWT i JWT odświeżanie token interfejsu API sieci web pierwszego poziomu.


4. Za pomocą protokołu HTTPS interfejsu API sieci web pierwszego poziomu wywołuje interfejsu API sieci web podrzędne token ciąg w nagłówku autoryzacji w wezwaniu. Interfejsu API sieci web pierwszego poziomu nadal połączeń interfejsu API sieci web podrzędne, jak długo token dostępu i Odśwież tokeny są prawidłowe.

#### <a name="code-samples"></a>Przykłady kodu

Zobacz przykłady kodu demon lub aplikacji serwera scenariuszy interfejs API sieci Web. I zaglądaj tu często — dodajemy nowe przykłady cały czas. [Serwer lub aplikacją demon interfejsu API sieci Web](active-directory-code-samples.md#server-or-daemon-application-to-web-api)

#### <a name="registering"></a>Rejestrowanie

- Pojedynczy dzierżawy: Zarówno dla tożsamości aplikacji i przypadkach tożsamość użytkownika delegowanych, aplikację demona lub serwera musi być zarejestrowany w tym samym katalogu w Azure AD. Aby udostępnić zestaw uprawnień, które są używane do ograniczania demona lub serwera dostępu do jego zasobów można skonfigurować interfejsu API sieci web. Jeśli jest używany typ tożsamości użytkownika delegowanych, aplikacja serwera musi wybierz odpowiednie uprawnienia z menu rozwijanego "Uprawnienia do innych aplikacji" w portalu zarządzania Azure. Ten krok nie jest wymagany, jeśli jest używany typ tożsamości aplikacji.


- Wielu dzierżawy: Najpierw aplikacji demon lub serwer jest skonfigurowany do wskazać uprawnienia wymagane do działać. Ta lista wymagane uprawnienia są wyświetlane w oknie dialogowym podczas przez użytkownika lub administratora w katalogu docelowym powoduje zgody aplikacji, która udostępnia do organizacji. Niektóre aplikacje wymagają po prostu poziomie użytkownika uprawnienia, które mogą przyznać każdy użytkownik w organizacji. Inne aplikacje wymaga uprawnienia na poziomie administratora, które użytkownik w organizacji nie wyraża zgodę na. Tylko administrator katalogów może powodować zgody aplikacji wymagających tego poziomu uprawnień. Zgadza się użytkownika lub administratora, oba interfejsy API sieci web zarejestrowane w ich katalogu.

#### <a name="token-expiration"></a>Wygaśnięcia tokenu

Podczas pierwszej aplikacji używa jego kod autoryzacji uzyskanie dostępu do JWT token, również otrzymuje tokenu odświeżania JWT. Po wygaśnięciu token dostępu, token odświeżania może służyć do ponownego uwierzytelnienia użytkownika bez monitowania o poświadczenia. Ten token odświeżania następnie jest używany do uwierzytelnienia użytkownika, które powoduje nowy token dostępu i token odświeżania.

## <a name="see-also"></a>Zobacz też

[Przewodnik programisty Azure Active Directory](active-directory-developers-guide.md)

[Przykłady kodu Azure Active Directory](active-directory-code-samples.md)

[Ważne informacje dotyczące logowania przerzucania klucza się Azure AD](active-directory-signing-key-rollover.md)

[OAuth 2.0 w Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx)
