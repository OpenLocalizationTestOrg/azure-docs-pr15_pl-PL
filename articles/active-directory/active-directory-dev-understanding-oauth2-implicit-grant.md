<properties
   pageTitle="Opis uwierzytelnianie OAuth2 niejawne udzielić przepływ w usługi Azure Active Directory | Microsoft Azure"
   description="Dowiedz się więcej na temat wdrażania usługi Azure Active Directory uwierzytelnianie OAuth2 niejawne udzielić przepływ, oraz czy jest odpowiedni dla aplikacji."
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="vibronet"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/17/2016"
   ms.author="vittorib;bryanla"/>

# <a name="understanding-the-oauth2-implicit-grant-flow-in-azure-active-directory-ad"></a>Opis przepływu niejawne udzielanie uwierzytelnianie OAuth2 w Azure Active Directory (AD)

Udzielanie niejawne uwierzytelnianie OAuth2 jest odpowiedzialne za jest udzielanie najdłuższej wykaz zagadnienia dotyczące zabezpieczeń w specyfikacji uwierzytelnianie OAuth2. I jednak to podejście przez ADAL JS i który zaleca się podczas pisania aplikacji bezpieczne uwierzytelnianie HASŁA. Co daje? Jest to kwestia kompromisów: i okaże się niejawne udzielanie jest najlepszym rozwiązaniem, można wykonywać dla aplikacji, które używają interfejs API sieci Web przy użyciu języka JavaScript w przeglądarce.

## <a name="what-is-the-oauth2-implicit-grant"></a>Co to jest udzielanie niejawne uwierzytelnianie OAuth2?

Quintessential [udzielić kod autoryzacji uwierzytelnianie OAuth2](https://tools.ietf.org/html/rfc6749#section-1.3.1) jest Udziel autoryzacji, który używa dwóch oddzielnych punktów końcowych. Punkt końcowy autoryzacji jest używana na etapie interakcji użytkownika, co powoduje kodu autoryzacji. Punkt końcowy token są używane przez klienta do wymiany kod token dostępu, a często także tokenu odświeżania. Aplikacje sieci Web są wymagane do prezentowania ich poświadczeń aplikacji do punktu końcowego tokenu, dzięki czemu serwer autoryzacji może przeprowadzać uwierzytelnianie klienta.

[Udzielanie uwierzytelnianie OAuth2 niejawne](https://tools.ietf.org/html/rfc6749#section-1.3.2) jest wartość typu Wariant oferujący kliencie uzyskanie token dostępu (i id_token w przypadku [Połączenia OpenId](http://openid.net/specs/openid-connect-core-1_0.html)) bezpośrednio z punktu końcowego autoryzacji bez kontaktowania się z punktu końcowego tokenu ani uwierzytelniania aplikacji klienckiej. Tego wariantu został są zaprojektowane specjalnie dla aplikacji działa w przeglądarce sieci Web opartych na JavaScript: w pierwotnej specyfikacji uwierzytelnianie OAuth2 tokeny są zwracane w fragmentu identyfikatora URI. Dzięki temu token bitów dostępne dla kodu JavaScript w kliencie, ale gwarantuje, że nie będą uwzględniane w przekierowania kierunku serwera. Zwracanie tokeny za pomocą przeglądarki przekierowuje bezpośrednio z punktu końcowego autoryzacji. Ponadto wprowadzono zaletą usuwaniu wszelkie wymagania krzyżowe origin połączeń, które są niezbędne, jeśli aplikacji JavaScript jest wymagane do kontaktów token punktu końcowego.

Ważne cechy udzielanie niejawne uwierzytelnianie OAuth2 jest fakt, że takie przepływał tokeny odświeżania nigdy nie zwrotnego do klienta. Jak widać będzie w następnej sekcji, która nie jest naprawdę potrzebne, a następnie w rzeczywistości będzie problem dotyczący zabezpieczeń.

## <a name="suitable-scenarios-for-the-oauth2-implicit-grant"></a>Odpowiednie scenariusze Udziel niejawne uwierzytelnianie OAuth2

Jako specyfikacji uwierzytelnianie OAuth2 się oświadcza, ma zostały wzmiankę niejawne udzielanie umożliwiają aplikacjom agenta użytkownika — to znaczy aplikacji JavaScript wykonywania w przeglądarce. Definiowanie cechy takich wniosków to, że kod JavaScript jest używana do uzyskiwania dostępu do zasobów serwera (zwykle API sieci Web) i w związku z tym aktualizowanie aplikacji interfejsu użytkownika. Wyobrazić aplikacji, takich jak Gmail lub programu Outlook Web Access: po wybraniu wiadomości ze skrzynki odbiorczej panelu wizualizacji wiadomości zmieniony, wyświetlając nowego zaznaczenia podczas pozostałej części strony pozostanie niezmieniona. W odróżnieniu od tradycyjnych oparte na przekierowanie aplikacji Web apps, to, gdzie wyniki każdej interakcji użytkownika w odświeżania całej strony i renderowanie pełnej strony nowej odpowiedzi serwera.

Aplikacje, które przyjmują metodę JavaScript podstawie do jego skrajnej są nazywane pojedynczej strony aplikacji lub źródła: ma to, że te aplikacje tylko służyć początkowej strony HTML i JavaScript skojarzone, z wszystkie kolejne interakcje prowadzone przez interfejsu API sieci Web wykonywane za pomocą języka JavaScript. Jednak hybrydowych metod, której aplikacji jest głównie sterowanych odświeżenie strony, ale wykonuje rzadkie połączeń JS, nie są nietypowych — omówienie zastosowania przepływu niejawne są odpowiednie dla osób, jak również.

Aplikacje oparte na przekierowanie bezpiecznego zwykle ich wysyłane za pomocą plików cookie, jednak metoda nie działa także JavaScript aplikacji. Pliki cookie działają tylko w domenie, które były wygenerowane, podczas połączenia JavaScript mogą być kierowane do innych domen. W rzeczywistości, które często będzie w przypadku: wyobrazić wniosków odwołujących się do programu Microsoft Graph API, API pakietu Office, Azure interfejsu API — wszystkie znajdujących się poza domeną, z której aplikacji jest obsługiwana. Trendu wzrostu dla aplikacji JavaScript ma nie wewnętrznej bazy danych, opierając się 100% na przyjęcie 3 interfejsy API sieci Web w celu zaimplementowania ich działalności.

Obecnie preferowaną metodą ochrony połączenia do interfejs API sieci Web jest użycie metody token okaziciela uwierzytelnianie OAuth2 miejsce, w którym każdego wywołania towarzyszy token dostępu uwierzytelnianie OAuth2. Interfejs API sieci Web sprawdza przychodzące token dostępu i, jeśli znajdzie w niej niezbędne zakresy, umożliwia dostęp do żądanej operacji. Niejawne przepływu zapewnia wygodny mechanizm JavaScript aplikacje, aby uzyskać tokeny dostępu dla interfejs API sieci Web oferuje wiele zalet w odniesieniu do plików cookie:

- Tokeny niezawodne uzyskuje się bez konieczności krzyżowe połączeń origin — obowiązkowe rejestracji Przekieruj, które URI tokeny są zwrotu gwarantuje, że tokeny nie są wypartej
- Aplikacje JavaScript można uzyskać możliwie jak najwięcej tokeny dostępu potrzebują, dla możliwie jak najwięcej API sieci Web są docelowe — bez ograniczeń domen
- Funkcje HTML5, takie jak sesji lub magazynu lokalnego udzielić pełną kontrolę nad token pamięci podręcznej i zarządzania istnienia zarządzania pliki cookie jest nieprzezroczystości do aplikacji
- Tokeny dostępu nie są podatne na ataki fałszowaniu (CSRF) żądania między witrynami

Przepływ udzielanie niejawne nie wydaje tokeny odświeżania, głównie ze względów bezpieczeństwa. Token odświeżania jako węższa nie jest ograniczone jako tokeny dostępu, udzielanie znacznie więcej możliwości w związku z tym inflicting znacznie więcej szkód w przypadku, gdy jest wyciekiem. W procesie niejawne tokeny są dostarczane w adresie URL, dlatego ryzyka przechwycenie jest większy niż u udzielanie kodu autoryzacji.

Jednak zauważyć, że aplikacja JavaScript znajduje innego mechanizmu dyspozycji na odnowienie tokeny dostępu bez wielokrotnie monitowania użytkownika o poświadczenia. Aplikacja umożliwia wykonywanie nowych wezwań token względem punktu końcowego autoryzacji Azure AD ukrytych iframe: jak przeglądarka nadal jest aktywnej sesji (przeczytaj: występują cookie sesji) w domenie Azure AD żądanie uwierzytelnienia pomyślnie mogą występować bez konieczności interakcji użytkownika. 

Ten model możliwość aplikacji JavaScript niezależne odnowić tokeny dostępu, a nawet uzyskiwanie nowych dla nowego interfejsu API (pod warunkiem, że użytkownik wcześniej zgodę dla nich. Pozwala to uniknąć dodano obciążenia podczas uzyskiwania, utrzymanie i ochrona artefaktu wysokiej wartości, takie jak token odświeżania. Artefaktu, co umożliwia odnowienie odbiorcze, plików cookie sesji Azure AD, odbywa się poza aplikacji. Zaletą tej metody jest użytkownik może Wyloguj się z usługi Azure Active Directory, przy użyciu aplikacji zalogowany na Azure AD działa w dowolnej z przeglądarki. Powoduje usunięcie cookie sesji Azure AD, a aplikacja JavaScript automatycznie zostaną utracone możliwość odnawiania tokenów dla podpisanego się użytkownika.

## <a name="is-the-implicit-grant-suitable-for-my-app"></a>Udzielanie niejawne nadaje się do aplikacji?

Udzielanie niejawne przedstawia ryzyka więcej niż inne dotacje. Obszary należy zwrócić uwagę na również opisano (na przykład wyświetlić [Niewłaściwe użycie programu Access Token personifikacji właściciela zasobu w niejawne przepływ] [ OAuth2-Spec-Implicit-Misuse] i [OAuth 2.0 zagrożenie modelu i zagadnienia dotyczące zabezpieczeń][OAuth2-Threat-Model-And-Security-Implications]). Wyższa profilu ryzyka jest jednak głównie fakt, że ma włączyć aplikacje, których wykonanie aktywnego kodu, obsługiwane przez zdalnego zasobu w przeglądarce. Jeśli planujesz architektura bezpieczne uwierzytelnianie HASŁA nie składniki wewnętrznej bazy danych lub zamierzają wywołania interfejs API sieci Web przy użyciu języka JavaScript, zaleca się użycie niejawne przepływu uzyskania tokenu.

Jeśli aplikacja jest klientami, niejawne przepływu nie jest doskonałe dopasowanie. Brak plików cookie sesji Azure AD w kontekście klientami pozbawia aplikacji środki utrzymania długo okresów ważności sesji. Co oznacza aplikacja będzie wielokrotnie monity podczas uzyskiwania tokeny dostępu dla nowych zasobów.

Jeśli tworzysz aplikacji sieci Web, która zawiera wewnętrznej bazie danych i zajmowanie interfejs API z jego kod wewnętrznej bazy danych niejawne przepływu jest właściwie dobierane. Inne dotacje umożliwiają znacznie więcej możliwości. Na przykład udzielanie poświadczeń klienta uwierzytelnianie OAuth2 umożliwia uzyskanie tokeny odzwierciedlić uprawnienia przypisane do aplikacji, a nie delegowania użytkownika. Oznacza to, że klient ma możliwość obsługi programowy dostęp do zasobów, nawet gdy użytkownik nie jest aktywnie zajmujący się sesji i tak dalej. Nie tylko że, ale takich przyznanych udostępnia wyższą gwarancje zabezpieczeń. Na przykład tokeny dostępu tranzytu przeze mnie za pośrednictwem przeglądarki użytkownika, nie ryzykuj są zapisywane w historii przeglądania i tak dalej. Podczas żądania token z aplikacją kliencką można wykonać silnego uwierzytelniania.

## <a name="next-steps"></a>Następne kroki

- Aby uzyskać pełną listę zasoby dla deweloperów w tym informacje dotyczące protokołów i uwierzytelnianie OAuth2 autoryzacji udzielanie pomocy technicznej przepływów przez Azure AD, poszukaj [Azure AD dewelopera Przewodnik po][AAD-Developers-Guide]
- Zobacz, [jak integracja aplikacji z usługą Azure Active Directory]  [ ACOM-How-To-Integrate] dodatkowe długości procesu integracja aplikacji.

<!--Image references-->

<!--Reference style links in use-->
[AAD-Developers-Guide]: active-directory-developers-guide.md
[ACOM-How-And-Why-Apps-Added-To-AAD]: active-directory-how-applications-are-added.md
[ACOM-How-To-Integrate]: active-directory-how-to-integrate.md
[OAuth2-Spec-Implicit-Misuse]: https://tools.ietf.org/html/rfc6749#section-10.16 
[OAuth2-Threat-Model-And-Security-Implications]: https://tools.ietf.org/html/rfc6819

