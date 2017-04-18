<properties
    pageTitle="Punkt końcowy 2.0 Azure AD | Microsoft Azure"
    description="Porównanie oryginalnego Azure AD i punkty końcowe 2.0."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>

# <a name="whats-different-about-the-v20-endpoint"></a>Jakie są różnice dotyczące punkt końcowy 2.0?

Jeśli znasz usługi Azure Active Directory lub mieć zintegrowane aplikacje z usługą Azure Active Directory w przeszłości, mogą występować pewne różnice w punkt końcowy 2.0, którego nie można oczekiwać.  Ten dokument wymaga tych różnic zrozumienie.

> [AZURE.NOTE]
    Nie wszystkie scenariusze usługi Azure Active Directory i funkcje są obsługiwane przez punkt końcowy 2.0.  Aby określić, należy użyć punktu końcowego 2.0, przeczytaj o [ograniczenia 2.0](active-directory-v2-limitations.md).


## <a name="microsoft-accounts-and-azure-ad-accounts"></a>Konta Microsoft i Azure AD
punkt końcowy 2.0 — umożliwiają projektantom napisać aplikacje, które przyjmują logowania z kont programu Microsoft Accounts i Azure AD przy użyciu punkt końcowy jednej uwierzytelniania.  Uzyskasz możliwość zapisywania aplikacji całkowicie konta-niezależne od; może być zakresu typu konta, które użytkownik zaloguje się.  Oczywiście *można* poinformuj o tym, typ konta, które są używane w konkretnej sesji aplikacji, ale nie trzeba.

Na przykład jeśli aplikacji wymaga [Programu Microsoft Graph](https://graph.microsoft.io), niektóre dodatkowe funkcje i dane będą dostępne dla użytkowników w przedsiębiorstwach, takich jak ich witryn programu SharePoint lub dane katalogu.  Ale dla wielu akcji, takich jak [Czytanie poczty użytkownika](https://graph.microsoft.io/docs/api-reference/v1.0/resources/message), można napisać kod dokładnie tak samo w przypadku kont programu Microsoft Accounts i Azure AD.  

Integracja aplikacji z Accounts firmy Microsoft i Azure AD jest teraz jeden skomplikowanym procesem.  Jeden zestaw punkty końcowe, jednej biblioteki i rejestracji pojedynczej aplikacji umożliwia uzyskanie dostępu do względem zarówno dla klientów indywidualnych, jak i przedsiębiorstwa.  Aby dowiedzieć się więcej na temat punktu końcowego 2.0, zapoznaj się [z artykułem](active-directory-appmodel-v2-overview.md).


## <a name="new-app-registration-portal"></a>Nowy portal rejestracji aplikacji
punkt końcowy 2.0 można rejestrować tylko w nowej lokalizacji: [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).  Jest to miejsce, w którym można uzyskać identyfikator aplikacji portalu dostosować wygląd strona logowania do aplikacji i nie tylko.  Wystarczy, aby uzyskać dostęp do portalu jest konto Microsoft korzystająca — konto osobiste lub praca/Szkoła.  

Będziemy Dodawanie funkcji bardziej do tego portalu rejestracji aplikacji w czasie.  Zamiarem to, że ten portal będzie nowej lokalizacji, w której można przejść do zarządzania nic i wszystkich związanych z aplikacji Microsoft.


## <a name="one-app-id-for-all-platforms"></a>Jedną aplikację identyfikator dla wszystkich platform
W usłudze Active Directory platformy Azure oryginalny mogą zarejestrowane kilka różnych aplikacji dla pojedynczego projektu.  Zostały musieli używanie aplikacji oddzielnym rejestracji natywnych klientów i aplikacji sieci web:

![Rejestrowanie aplikacji starego interfejsu użytkownika](../media/active-directory-v2-flows/old_app_registration.PNG)

Na przykład jeśli skonstruowaniu zarówno witryny sieci Web i aplikacji w systemie iOS trzeba było zarejestrować je oddzielnie, przy użyciu dwóch różnych identyfikatorów aplikacji.  Jeśli masz witrynę sieci Web i wewnętrznej bazie danych sieci web interfejs api może być zarejestrowany jako osobne aplikacji w Azure AD.  Jeśli wcześniej używano aplikacji w systemie iOS i Android aplikacji, także mogą zarejestrowaniu dwóch różnych aplikacji.  

<!-- You may have even registered different apps for each of your build environments - one for dev, one for test, and one for production. -->

Teraz wystarczy jest rejestracji pojedynczej aplikacji i pojedynczy identyfikator aplikacji dla każdego z projektów.  Możesz dodać kilka "platformy" do każdego projektu i podaj odpowiednie dane dla poszczególnych platform, które można dodać.  Oczywiście można tworzyć możliwie jak najwięcej aplikacji stosownie do potrzeb, takich jak, ale dla większości przypadków tylko jeden identyfikator aplikacji powinna być konieczne.

<!-- You can also label a particular platform as "production-ready" when it is ready to be published to the outside world, and use that same Application Id safely in your development environments. -->

Naszym celem jest, że to będzie prowadzić do bardziej uproszczone zarządzanie aplikacji i proces tworzenia i tworzenie bardziej skonsolidowane widoku pojedynczego projektu, który może działać na.


## <a name="scopes-not-resources"></a>Zakresy, nie zasobów
W usłudze Azure AD oryginalnej aplikacji może działać jako **zasobu**lub adresata tokenów.  Zasób można określić liczbę **zakresów** lub **oAuth2Permissions** siebie, umożliwiając klienta aplikacje, aby zażądać tokeny do tego zasobu dla określonego zestawu zakresów.  Należy rozważyć interfejsu API Azure AD wykres, na przykład zasobu:

- Identyfikator zasobu lub `AppID URI`:`https://graph.windows.net/`
- Zakresy, lub `OAuth2Permissions`: `Directory.Read`, `Directory.Write`itp.  

Wszystkie te prawdziwe dla punktu końcowego 2.0.  Aplikacja nadal może działać jako zasobu, Definiowanie zakresów i oznaczane identyfikatora URI.  Aplikacje klienta nadal można zażądać dostępu do tych zakresów.  Jednak sposób, w której klient żąda uprawnienia uległ zmianie.  W przeszłości Autoryzuj 2.0 OAuth żądanie Azure AD może sprawdzono, takich jak:

```
GET https://login.microsoftonline.com/common/oauth2/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&resource=https%3A%2F%2Fgraph.windows.net%2F
...
```

Jeśli parametr **zasobu** wskazany zasobów aplikacji klienckiej żąda zezwolenia na.  Azure AD obliczona z uprawnieniami wymaganymi za pomocą aplikacji na podstawie konfiguracji statyczne w Azure Portal i odpowiednio wydanych tokeny.  Teraz samego 2.0 OAuth zezwalają na żądanie wygląda:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read%20https%3A%2F%2Fgraph.windows.net%2Fdirectory.write
...
```

miejsce, w którym parametr **zakres** wskazuje, który zasób i uprawnienia aplikacji żąda pozwolenia. Żądanego zasobu nadal jest bardzo prezentowanie w wezwaniu — po prostu jest kategorii w każdej z wartości parametru zakresu.  Przy użyciu parametru zakresu w ten sposób umożliwia punkt końcowy 2.0 — być bardziej zgodny ze specyfikacji OAuth 2.0 i lepiej wyrównane typowe rozwiązania branżowe.  Umożliwia także aplikacje, aby wykonać [zgody przyrostowe](#incremental-and-dynamic-consent), które zostało opisane w następnej sekcji.

## <a name="incremental-and-dynamic-consent"></a>Zgody przyrostowe i dynamicznych
Aplikacje zarejestrowane w ogólnodostępne Azure AD usługi, aby określić ich wymagane uprawnienia OAuth 2.0 w Azure Portal w czasie tworzenia aplikacji:

![Rejestracja uprawnienia interfejsu użytkownika](../media/active-directory-v2-flows/app_reg_permissions.PNG)

Uprawnienia aplikacji wymagane zostały skonfigurowane **statycznie**.  Konfiguracja aplikacji istnieć w Azure Portal dozwolonych i przechowywane kod i i proste, przedstawiono kilka problemów dla deweloperów:

- Aplikację trzeba wiedzieć, uprawnienia, które kiedykolwiek będą potrzebne podczas tworzenia aplikacji.  Dodawanie uprawnień w czasie jest trudne procesu.
- Aplikację musiała ustalić wszystkie zasoby, które kiedykolwiek dostęp do wyprzedzeniem.  Było trudne do tworzenia aplikacji, które można uzyskać dostępu do dowolnej liczby zasobów.
- Aplikację musiała zażądać uprawnienia, które kiedykolwiek będzie potrzebne na użytkownika pierwszego logowania.  W niektórych przypadkach doprowadziło to do bardzo dużej liczbie uprawnień, które nie zaleca się użytkowników końcowych ze zatwierdzania aplikacji programu access na początkowej logowania.

Z punktem końcowym 2.0 można określić uprawnienia aplikacji **dynamicznie**w czasie rzeczywistym, należy podczas normalnego użytkowania aplikacji.  Aby to zrobić, można określić zakresy aplikacji potrzebnych na dowolnym etapie w czasie, łącznie z nimi w `scope` parametr żądania autoryzacji:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read%20https%3A%2F%2Fgraph.windows.net%2Fdirectory.write
...
```

Powyższe uprawnienie żądania aplikację, aby odczytać użytkownika Azure AD katalogu danych, a także zapisywania ich katalogu danych.  Jeśli użytkownik ma zgodę na uprawnienia w przeszłości dla określonej aplikacji, po prostu będą wprowadzać poświadczeń i zalogować do aplikacji.  Jeśli użytkownik nie ma zgodę na dowolny z tych uprawnień, punkt końcowy 2.0 — powoduje wyświetlenie monitu o zgodę na te uprawnienia.  Aby uzyskać więcej informacji, można znaleźć w górę o [uprawnieniach, zgoda i zakresów](active-directory-v2-scopes.md).

Zezwolenia na aplikację, aby zażądać uprawnień dynamicznie za pośrednictwem `scope` parametru zapewnia pełną kontrolę nad przez użytkownika.  Jeśli chcesz, możesz do frontload Twojej zgody środowiska i poproś o wszystkie uprawnienia w jedno żądanie autoryzacji początkowej.  Jeśli aplikacji wymaga dużej liczby uprawnienia, możesz też wybrać zebrać te uprawnienia użytkownika stopniowo, jak próbie określonych funkcji aplikacji w czasie.

## <a name="well-known-scopes"></a>Znane zakresów

#### <a name="offline-access"></a>Dostęp w trybie offline
punkt końcowy 2.0 mogą wymagać stosowania nowe uprawnienie znanego aplikacje - `offline_access` zakres.  Wszystkie aplikacje należy poprosić to uprawnienie w razie potrzeby dostępu do zasobów w imieniu użytkownika przez okres dłuższy czas, nawet wtedy, gdy użytkownik może nie być aktywnie przy użyciu aplikacji.  `offline_access` Zakres będzie widoczna dla użytkownika w oknach dialogowych zgody jako "Dostęp do danych w trybie offline", który użytkownik musi zaakceptować.  Żądanie `offline_access` uprawnień umożliwi aplikacji sieci web do odbierania refresh_tokens OAuth 2.0 od punktu końcowego 2.0.  Refresh_tokens są długotrwałe i może zostać wymienione na nowe access_tokens OAuth 2.0 przez dłuższy czas programu access.  

Jeśli aplikacji nie wymaga `offline_access` zakres, nie będziesz otrzymywać refresh_tokens.  Oznacza to, że po zrealizować authorization_code w [Przepływ kod autoryzacji OAuth 2.0](active-directory-v2-protocols.md#oauth2-authorization-code-flow), otrzymasz tylko ponownie access_token z `/token` punktu końcowego.  Tej access_token pozostanie obowiązujące przez krótki okres (zazwyczaj jedna godzina), ale po pewnym czasie wygaśnie.  W tym momencie, aplikacji należy przekierować użytkownika z powrotem do `/authorize` punktu końcowego do pobierania nowych authorization_code.  Podczas tego przekierowania użytkownika może być lub może nie być konieczne ponownie wprowadź swoje poświadczenia i ponownie wyraża zgodę na uprawnień, w zależności od typu aplikacji.

Aby dowiedzieć się więcej o OAuth 2.0, refresh_tokens i access_tokens, zapoznaj się z [odwołań Protocol (protokół) 2.0](active-directory-v2-protocols.md).

#### <a name="openid-profile--email"></a>OpenID, profilu i poczty e-mail

W usłudze Active Directory platformy Azure oryginalny najprostszy łączenie OpenID logowania przepływ zapewni mnóstwo informacje o użytkowniku w celu utworzenia id_token.  Roszczeń w id_token może zawierać użytkownika nazwy, preferowany nazwy użytkownika, adres e-mail, identyfikator obiektu i inne.

Firma Microsoft są teraz ograniczanie informacje które `openid` zakres zapewnia dostęp do aplikacji.  Zakres "openid" będzie tylko zezwolić aplikacji do zalogowania użytkownika w i odbierać identyfikator aplikacji specyficzne dla użytkownika.  Jeśli chcesz uzyskać identyfikowalne dane osobowe o użytkowniku w aplikacji aplikacji należy poprosić dodatkowe uprawnienia użytkownika.  Firma Microsoft jest wprowadzenie dwa nowe zakresy — `email` i `profile` zakresów — zezwalające na to zgodę.

`email` Zakres jest bardzo proste — umożliwia dostęp aplikacji do użytkownika podstawowy adres e-mail za pomocą `email` rościć sobie w id_token.  `profile` Zakres zapewnia dostęp aplikacji do wszystkie inne podstawowe informacje o użytkowniku — ich nazwy, preferowany nazwa użytkownika, identyfikator obiektu i tak dalej.

Pozwala na kod aplikacji w sposób minimalnymi ujawnienie — można tylko poproś użytkownika dla zestawu informacji o aplikacji wymaga jego pracy.  Aby uzyskać więcej informacji o tych zakresów zapoznaj się z [Odwołanie zakres 2.0](active-directory-v2-scopes.md). 

## <a name="token-claims"></a>Token oświadczeń

Roszczeń w tokeny wydawany przez punkt końcowy 2.0 — nie będą identyczne tokeny wydawany przez ogólnie dostępne punkty końcowe Azure AD - aplikacje Migrowanie do nowej usługi należy zakłada się określonego roszczeń wystąpią w id_tokens lub access_tokens.   Tokeny wydawany przez punkt końcowy 2.0 są zgodne z warunkami OAuth 2.0 i łączenie OpenID, ale może wykonać znaczenie innego niż zazwyczaj dostępne usługi Azure AD.

Aby uzyskać informacje o określonych roszczeń emisji w tokeny 2.0, zobacz [odwołania do tokenu 2.0](active-directory-v2-tokens.md).

## <a name="limitations"></a>Ograniczenia
Istnieje kilka ograniczeń, o których warto pamiętać podczas korzystania z punktu 2.0.  Zajrzyj do [dokumentu ograniczenia 2.0 —](active-directory-v2-limitations.md) Aby wyświetlić, jeśli dowolny z tych ograniczeń dotyczy do scenariusza.
