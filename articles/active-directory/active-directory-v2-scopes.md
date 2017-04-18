<properties
    pageTitle="Azure AD 2.0 zakresów, uprawnień i zgody | Microsoft Azure"
    description="Opis autoryzacji w punkt końcowy 2.0 Azure AD, takich jak zakresy, uprawnień i zgody."
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
    ms.date="09/30/2016"
    ms.author="dastrock"/>

# <a name="scopes-permissions--consent-in-the-v20-endpoint"></a>Zakresy, uprawnień i zgody na punkt końcowy 2.0

Aplikacje, które zostały zintegrowane z usługą Azure Active Directory wykonaj modelu szczególne zezwolenie, który pozwala kontrolować sposób aplikacji można uzyskiwać dostępu do danych.  Została zaktualizowana 2.0 stosowania tego modelu autoryzacji, zmienianie aplikacji należy interakcja z usługą Azure Active Directory.  W tym temacie opisano podstawowe pojęcia tego modelu autoryzacji, takich jak zakresy, uprawnień i zgody.

> [AZURE.NOTE]
    Nie wszystkie scenariusze usługi Azure Active Directory i funkcje są obsługiwane przez punkt końcowy 2.0.  Aby określić, należy użyć punktu końcowego 2.0, przeczytaj o [ograniczenia 2.0](active-directory-v2-limitations.md).

## <a name="scopes--permissions"></a>Zakresy i uprawnienia

Azure AD wykonuje protokołu autoryzacji [OAuth 2.0](active-directory-v2-protocols.md) , który jest metodą zezwolenia 3 aplikacji firmy na dostęp do sieci web hostowanej zasobów w imieniu użytkownika.  Każdy-internetowej zasób, którego można zintegrować z usługą Azure Active Directory będą mieć identyfikator zasobu lub **Aplikacji identyfikator URI**.  Na przykład firmy Microsoft w sieci web hostowanej zasobów należą:

- Office 365 Unified poczty interfejsu API:`https://outlook.office.com`
- Interfejs API Azure AD wykresu:`https://graph.windows.net`
- Program Microsoft Graph:`https://graph.microsoft.com`

To samo dotyczy dla wszystkich 3 zasobów firmy, które ma zintegrowane z usługą Azure Active Directory.  Dowolny z tych zasobów można także zdefiniować zestaw uprawnień, które mogą być używane do dzielenia się funkcji do mniejszych fragmenty tego zasobu.  Na przykład programu Microsoft Graph zdefiniowano kilka uprawnień:

- Przeczytaj kalendarz użytkownika
- Zapisywanie kalendarza użytkownika
- Wyślij wiadomość e-mail jako użytkownik
- [+ informacje dodatkowe](https://graph.microsoft.io)

Definiując tych uprawnień, zasób ma precyzyjne sterowanie wraz z danymi i jak są wyświetlane ze światem.  3 aplikacji firmy można zażądać te uprawnienia z dla użytkownika końcowego — i użytkownik końcowy musi zatwierdzić uprawnienia, przed aplikacji mogą działać w jego imieniu.  Według segmentu funkcji zasobu do mniejszych zestawów uprawnień, 3 aplikacje innych firm mogą być wbudowane żądania tylko określonych uprawnień, które są potrzebne w celu wykonania ich funkcji.  Umożliwia także użytkownikom wiedzieć, dokładnie tak jak aplikację użyje ich danych, tak aby były bardziej pewność, że aplikacja nie zachowuje się z zamiarem wyrządzenia szkód.

W Azure AD i OAuth, te uprawnienia są nazywane **zakresów**.  Można też zobaczyć ich określane jako **oAuth2Permissions**.  Zakres jest reprezentowany w Azure AD jako wartość ciągu.  Wartość zakres dla każdego uprawnienia kontynuowaniem przykład programu Microsoft Graph, jest:

- Przeczytaj kalendarza użytkownika:`Calendar.Read`
- Zapisywanie kalendarza użytkownika:`Mail.ReadWrite`
- Wysyłanie poczty jako użytkownika:`Mail.Send`

Aplikację można zażądać tych uprawnień, określając zakresy w żądania do punktu końcowego 2.0, zgodnie z poniższym opisem.

## <a name="openid-connect-scopes"></a>Łączenie OpenId zakresów

Łączenie OpenID wykonania 2.0 zawiera kilka dobrze zdefiniowanych zakresów, które nie mają zastosowania do dowolnego określonego zasobu — `openid`, `email`, `profile`, i `offline_access`.

#### <a name="openid"></a>OpenId

Jeśli aplikacja wykonuje logowania przy użyciu funkcji, [OpenID połączenie](active-directory-v2-protocols.md#openid-connect-sign-in-flow), musisz poprosić `openid` zakres.  `openid` Zakres pojawi się na ekranie pracy konto zgody uprawnienia "Zalogowanie użytkownika", a w osobistej ekran zgody konta Microsoft uprawnienia "Wyświetlić swój profil i łączenie się z aplikacji i usług przy użyciu konta Microsoft".  To uprawnienie umożliwia aplikacji do odbierania Unikatowy identyfikator użytkownika w formie `sub` przejmowania.  Go również zapewnia dostęp aplikacji do użytkownika końcowego informacje.  `openid` Zakres można również na punkt końcowy token 2.0 — umożliwia uzyskanie id_tokens, które mogą być używane do bezpiecznego połączenia HTTP między różne części aplikacji.

#### <a name="email"></a>Adres e-mail

`email` Zakres może być dołączone wzdłuż `openid` zakresów i innych.  Zapewnia on dostęp aplikacji do użytkownika podstawowy adres e-mail w postaci `email` rościć sobie.  `email` Roszczeń tylko zostaną uwzględnione w tokenów, jeśli adres e-mail jest skojarzony z konta użytkownika, które nie zawsze jest wielkość liter.  Jeśli przy użyciu `email` zakres aplikacji należy przygotować się do obsługi sprawy, w której `email` roszczeń nie istnieje w tokenu.

#### <a name="profile"></a>Profil

`profile` Zakres może być dołączone wzdłuż `openid` zakresów i innych.  Zapewnia on app uzyskiwania dostępu do wielu zasobów informacji o użytkowniku.  Zawiera, ale nie jest ograniczone do imię, nazwisko użytkownika, preferowany nazwy użytkownika, identyfikator obiektu i tak dalej.  Aby uzyskać pełną listę dostępnych w id_tokens roszczeń profilu dla danego użytkownika zapoznaj się z [odwołania do tokenu 2.0](active-directory-v2-tokens.md).

#### <a name="offlineaccess"></a>Offline_access

[ `offline_access` Zakres](http://openid.net/specs/openid-connect-core-1_0.html#OfflineAccess) umożliwia aplikacji dostępu do zasobów w imieniu użytkownika przez dłuższy czas.  Na ekranie pracy konto zgody tego zakresu zostaną wyświetleni uprawnień "W dowolnym momencie uzyskać dostęp do danych".  W osobistej ekranu zgody konta Microsoft pojawią się uprawnień "W dowolnym momencie uzyskać dostęp do informacji".  Gdy użytkownik akceptuje `offline_access` zakres aplikacji będą dostępne do odbierania tokeny odświeżania od punktu końcowego tokenu 2.0.  Odświeżanie tokeny są długotrwałe i zezwolić aplikacji umożliwia uzyskanie nowego tokeny dostępu, jak te starsze wygaśnie.

Jeśli aplikacji nie wymaga `offline_access` zakres, nie będziesz otrzymywać refresh_tokens.  Oznacza to, że po zrealizować authorization_code w [Przepływ kod autoryzacji OAuth 2.0](active-directory-v2-protocols.md#oauth2-authorization-code-flow), otrzymasz tylko ponownie access_token z `/token` punktu końcowego.  Tej access_token pozostanie obowiązujące przez krótki okres (zazwyczaj jedna godzina), ale po pewnym czasie wygaśnie.  W tym momencie, aplikacji należy przekierować użytkownika z powrotem do `/authorize` punktu końcowego do pobierania nowych authorization_code.  Podczas tego przekierowania użytkownika może być lub może nie być konieczne ponownie wprowadź swoje poświadczenia i ponownie wyraża zgodę na uprawnień, w zależności od typu aplikacji.

Aby uzyskać więcej informacji na temat Pobieranie i używanie odświeżanie tokenów, zajrzyj do [odwołania Protocol (protokół) 2.0](active-directory-v2-protocols.md).


## <a name="requesting-individual-user-consent"></a>Żądanie zgoda poszczególnych użytkowników

[Łączenie OpenID lub OAuth 2.0](active-directory-v2-protocols.md) żądania autoryzacji, aplikacji można zażądać uprawnień wymaganych przy użyciu `scope` kwerenda parametryczna.  Na przykład przy użytkownika do aplikacji, aplikacja może wysłać żądanie podobnej do następującej (z podziały wierszy w celu zwiększenia czytelności):

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&scope=
https%3A%2F%2Fgraph.microsoft.com%2Fcalendar.read%20
https%3A%2F%2Fgraph.microsoft.com%2Fmail.send
&state=12345
```

`scope` Parametr jest rozdzielany spacjami zakresów, które żąda aplikacji.  Każdy zakres poszczególnych jest wskazywana przez dodanie wartości zakresu do zasobu identyfikator aplikacji identyfikator URI.  Powyższy wniosek wskazuje, że aplikacja wymaga uprawnienia do kalendarza użytkownika czytania i wysyłania poczty jako użytkownik.

Gdy użytkownik wprowadzi swoje poświadczenia, punkt końcowy 2.0 sprawdza pasujące rejestr **użytkownik wyraża zgodę**.  Jeśli użytkownik nie ma zgodę na dowolny wymagane uprawnienia w przeszłości, punkt końcowy 2.0 — powoduje wyświetlenie monitu udzielenia wymagane uprawnienia.  

![Zrzut ekranu przedstawiający roboczego konta zgody](../media/active-directory-v2-flows/work_account_consent.png)

Gdy użytkownik zatwierdzi uprawnienia, zgody będą rejestrowane tak, aby użytkownik nie musi ponownie wyraża zgodę na kolejne dodatki logowania.

## <a name="requesting-consent-for-an-entire-tenant"></a>Żądanie zgody na całej dzierżawy

Często, gdy organizacja zakupów licencji lub subskrypcję usługi aplikacji, chcesz w pełni Inicjowanie obsługi dla pracowników.  W ramach tego procesu administrator przedsiębiorstwa może udzielić zgody na tę aplikację do działania w imieniu dowolnego pracownika.  Przyznając zgody na dzierżawę całego ekranu zgody dla tej aplikacji nie będzie działać z pracowników organizacji.

Aby zażądać zgody dla wszystkich użytkowników w dzierżawie, aplikacji można używać **administratora zgodę punktu końcowego**, opisane poniżej.

## <a name="admin-restricted-scopes"></a>Zakresy ograniczonym

Niektóre uprawnienia priviledge wysoki w ekosystem firmy Microsoft, może zostać oznaczona jako **ograniczonym**.  Przykłady takich zakresy:

- Czytanie organizaion katalogu danych:`Directory.Read`
- Zapisywanie danych w książce telefonicznej organizacji:`Directory.ReadWrite`
- Czytanie grup zabezpieczeń w książce telefonicznej organizacji:`Groups.Read.All`

Gdy użytkownik dla klientów indywidualnych może udzielić aplikacji dostęp do tych danych, organizacji użytkowników masz uprawnienia na udzielanie dostępu do tego samego zestawu poufnych danych firmy.  Jeśli aplikacja zażąda dostępu do jednego z tych uprawnień z organizacji użytkownika, zostanie wyświetlony komunikat o błędzie wskazujący, że są one autoryzowany do wyraża zgodę na uprawnień do aplikacji.

Jeśli aplikacji wymaga dostępu do tych zakresów ograniczonym dla organizacji, możesz przejąć je bezpośrednio z administrator firmy, również za pomocą **administratora zgodę punktu końcowego**, opisane poniżej.

Jeśli administrator udzieli tych uprawnień przez punkt końcowy zgody administratora, zgody zostanie udzielony dla wszystkich użytkowników w dzierżawie, zgodnie z powyższym opisem.

## <a name="using-the-admin-consent-endpoint"></a>Za pomocą punkt końcowy zgody administratora

Aplikacji, wykonując następujące kroki, będzie zebrać uprawnienia dla wszystkich użytkowników w danej dzierżawy, łącznie z ograniczonym zakresów.  Aby zobaczyć przykładowy kod, który wykonuje opisaną czynności poniżej, zapoznaj się z [próbki ograniczeniami zakresów administratora](https://github.com/Azure-Samples/active-directory-dotnet-admin-restricted-scopes-v2).

#### <a name="request-the-permissions-in-the-app-registration-portal"></a>Zażądaj uprawnień w portalu rejestracji aplikacji

- Przejdź do aplikacji w [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)lub [Tworzenie aplikacji](active-directory-v2-app-registration.md) , jeśli jeszcze tego nie zrobiono.
- Odszukaj sekcję **Uprawnieniach programu Microsoft Graph** i Dodaj uprawnienia, które wymagają aplikacji.
- Upewnij się, aby **zapisać** rejestracji aplikacji

#### <a name="recommended-sign-the-user-into-your-app"></a>Zalecane: zalogowanie się użytkownika do aplikacji

Zwykle podczas tworzenia aplikacji, która używa administrator zgodę punktu końcowego, aplikacja muszą mieć strony i widoku, który umożliwia administrator, aby zatwierdzić uprawnienia tej aplikacji.  Ta strona może być częścią aplikacji przepływu zapisów, ustawieniach aplikacji lub dedykowane przepływu "łączenie".  W większości przypadków jest to właściwe rozwiązanie dla aplikacji wyświetlić to "łączenie" widoku tylko wtedy, gdy użytkownik jest zalogowany przy użyciu służbowe konto Microsoft.

Logowanie się użytkownika do aplikacji umożliwia identyfikację organziation, do którego administrator należy przed prosząc o zatwierdzanie niezbędnych uprawnień.  Gdy nie niezbędne, ułatwiają tworzenie bardziej intuicyjny obsługi dla użytkowników organizacji.  Do zalogowania użytkownika, wykonaj naszych [samouczki Protocol (protokół) 2.0](active-directory-v2-protocols.md).

#### <a name="request-the-permissions-from-a-directory-admin"></a>Poproś uprawnienia administratora usługi katalogowej

Po zakończeniu uprawnieniom żądania od administratora firmy, można przekierowywać użytkownika 2.0 — **Administrator zgodę punktu końcowego**.

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/adminconsent?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&state=12345
&redirect_uri=http://localhost/myapp/permissions
```

```
// Pro Tip: Try pasting the below request in a browser!
```

```
https://login.microsoftonline.com/common/adminconsent?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&state=12345&redirect_uri=http://localhost/myapp/permissions
```

| Parametr | | Opis |
| ----------------------- | ------------------------------- | --------------- |
| dzierżawy | Wymagane | Dzierżawy katalogu, który ma być zażądaj uprawnień z.  Może być udostępniana w formacie przyjazną nazwę lub identyfikator guid. |
| client_id | Wymagane | Identyfikator aplikacji przypisania aplikacji portalu rejestracji ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)). |
| redirect_uri | Wymagane | Redirect_uri, w którym chcesz wstawić odpowiedź wysłany w celu aplikacji do obsługi.  Dokładnie musi odpowiadać jeden z redirect_uris, które zarejestrowane w portalu. |
| Województwo | zalecane | Wartość zawarte w żądaniu, która będzie również zwracana w odpowiedzi token.  Może być ciągiem dowolną zawartość, która ma.  Stan jest używany do kodowania informacji na temat stanu użytkownika w aplikacji, przed wystąpieniem żądanie uwierzytelnienia, takich jak strony lub widoku, w których znajdowały się na. |

W tym momencie Azure AD wymusić, że administrator dzierżawy zalogować się do wykonania żądania.  Administrator zostanie poproszony o zatwierdzać wszystkie uprawnienia, które żądanej aplikacji w portalu rejestracji. 

##### <a name="successful-response"></a>Pomyślną odpowiedź
Jeśli administrator zatwierdzi uprawnienia dla aplikacji, będzie pomyślną odpowiedź:

```
GET http://localhost/myapp/permissions?tenant=a8990e1f-ff32-408a-9f8e-78d3b9139b95&state=state=12345&admin_consent=True
```

| Parametr | Opis |
| ----------------------- | ------------------------------- | --------------- |
| dzierżawy | Dzierżawy katalogu, która przyznała uprawnienia aplikacji go wymagane, w formacie guid. |
| Województwo | Wartość zawarte w żądaniu, która będzie również zwracana w odpowiedzi token.  Może być ciągiem dowolną zawartość, która ma.  Stan jest używany do kodowania informacji na temat stanu użytkownika w aplikacji, przed wystąpieniem żądanie uwierzytelnienia, takich jak strony lub widoku, w których znajdowały się na. |
| admin_consent | Zostanie ustawiona na `True`. |



##### <a name="error-response"></a>Odpowiedzi komunikat o błędzie
Jeśli administrator nie zatwierdzić uprawnienia dla aplikacji, będzie zakończonego niepowodzeniem odpowiedzi:

```
GET http://localhost/myapp/permissions?error=permission_denied&error_description=The+admin+canceled+the+request
```

| Parametr | Opis |
| ----------------------- | ------------------------------- | --------------- |
| Błąd | Ciąg kodu błędu, który może służyć do klasyfikowania typów błędów występujących i może służyć do reagować na błędy. |
| error_description | Komunikat o błędzie określonych, które mogą być pomocne Deweloper Określ przyczynę błędu.  |

Po odebraniu odpowiedzi powiedzie od punktu końcowego zgody administratora, aplikacji uzyskanie go wymagane uprawnienia.  Teraz można przenieść na żąda tokenu do żądanego zasobu w sposób opisany poniżej.

## <a name="using-permissions"></a>Za pomocą uprawnień

Gdy użytkownik wyraża zgodę na uprawnień dla aplikacji, aplikacji mogą uzyskiwać tokeny dostępu, reprezentujących Twojej aplikacji uprawnień dostępu do zasobu w niektórych wydajność.  Token dostępu danego można używać tylko dla jednego resorce, ale zakodowany w nim będzie co uprawnienie, które przyznano aplikacji dla tego zasobu.  Uzyskanie token dostępu, aplikacji można wprowadzić żądanie punkt końcowy token 2.0:

```
POST common/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/json

{
    "grant_type": "authorization_code",
    "client_id": "6731de76-14a6-49ae-97bc-6eba6914391e",
    "scope": "https://outlook.office.com/mail.read https://outlook.office.com/mail.send",
    "code": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq..."
    "redirect_uri": "https://localhost/myapp",
    "client_secret": "zc53fwe80980293klaj9823"  // NOTE: Only required for web apps
}
```

Wynikowa token dostępu można w żądania HTTP do zasobu - on prawidłowo wskaże zasobu czy aplikacji ma odpowiednie uprawnienia do wykonania określonego zadania.  

Więcej szczegółów protokołu OAuth 2.0 oraz sposób uzyskiwania tokeny dostępu można znaleźć w [odwołania protokołu punktu końcowego 2.0](active-directory-v2-protocols.md).
