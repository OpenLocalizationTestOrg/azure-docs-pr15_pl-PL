<properties
    pageTitle="Zmieni się na punkt końcowy 2.0 Azure AD | Microsoft Azure"
    description="Opis zmiany wprowadzone do protokołów publicznej Podgląd 2.0 modelu aplikacji."
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

# <a name="important-updates-to-the-v20-authentication-protocols"></a>Ważne aktualizacje protokoły 2.0
Uwagi deweloperów! W następnych dwóch tygodniach firma Microsoft będzie prowadziła kilka aktualizacji do naszych protokoły 2.0, w których może oznaczać przerywanie zmiany dotyczące dowolnej aplikacji, które zostały zapisane w okresie Podgląd.  

## <a name="who-does-this-affect"></a>Kto to wpływa na?
Wszystkie aplikacje, które zostały zapisane w za pomocą 2.0 zbieżność końcowy uwierzytelniania

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
```

Więcej informacji na temat punktu końcowego 2.0 można znaleźć [tutaj](active-directory-appmodel-v2-overview.md).

Jeśli masz wbudowana aplikację, używając kodowania bezpośrednio do protokołu 2.0 — punkt końcowy 2.0, przy użyciu jednej z naszych OpenID połączenia lub OAuth middlewares sieci web lub przy użyciu innych bibliotek strona 3 do uwierzytelniania, możesz należy przygotować się do testowania projektów i wprowadzić zmiany w razie potrzeby.

## <a name="who-doesnt-this-affect"></a>Kto nie to wpływ?
Wszystkie aplikacje, które zostały zapisane przed punkt końcowy uwierzytelniania produkcji Azure AD

```
https://login.microsoftonline.com/common/oauth2/authorize
```

Niniejszy Protokół jest ustawiona w kamień i nie występują zmiany.

Ponadto jeśli do uwierzytelniania aplikacji z **tylko** korzysta z naszych ADAL biblioteki, nie musisz zmienić jakiekolwiek właściwości.  ADAL jest chroniony aplikacji od zmiany.  

## <a name="what-are-the-changes"></a>Co to są zmiany?
### <a name="removing-the-x5t-value-from-jwt-headers"></a>Usunięcie z nagłówkami JWT wartości x5t
Punkt końcowy 2.0 używa tokenów JWT szerokim zakresie, które zawierają sekcji parametrów nagłówka z odpowiednich metadanych dotyczących tokenu.  Jeśli dekodowanie nagłówek jednej z naszych bieżącego JWTs, czy znaleźć podobną do:

```
{ 
    "type": "JWT",
    "alg": "RS256",
    "x5t": "MnC_VZcATfM5pOYiJHMba9goEKY",
    "kid": "MnC_VZcATfM5pOYiJHMba9goEKY"
}
```

Miejsce, w którym właściwości "x5t" i "dla dzieci" Identyfikowanie klucz publiczny, który ma być używany do sprawdzania poprawności podpisu tokenu uzyskana z punkt końcowy łączenie OpenID metadanych.

Zmiana, która to najprostszy w tym miejscu jest usunięcie właściwości "x5t".  Może być nadal korzystać samej mechanizmy do sprawdzania poprawności tokenów, ale będą miały tylko we właściwościach "dla dzieci" pobrać prawidłowy klucz publiczny, zgodnie z protokołu OpenID połączenia. 

> [AZURE.IMPORTANT] **Zadanie: Upewnij się, aplikacji nie zależy od istnienia wartość x5t.**

### <a name="removing-profileinfo"></a>Usuwanie profile_info
Wcześniej, punkt końcowy 2.0 ma zostały zwracanie obiektu JSON base64 zakodowany w odpowiedziach token o nazwie `profile_info`.  Żądające token dostępu od punktu końcowego 2.0 przez wysłanie żądania w celu:

```
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

Odpowiedź będzie wyglądało obiekt JSON następujące czynności:
```
{ 
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https://outlook.office.com/mail.read",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "profile_info": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

`profile_info` Wartość informacje o użytkowniku, który podpisał do aplikacji — ich nazwy wyświetlanej, imię, nazwisko, adres e-mail, identyfikator i tak dalej.  Przede wszystkim `profile_info` została użyta w pamięci podręcznej tokenu i wyświetlić celów.

Teraz zostaje usunięta `profile_info` wartość — ale nie martw się, możemy nadal podaje te informacje do deweloperów w nieco inną lokalizację.  Zamiast `profile_info`, punkt końcowy 2.0 zwróci teraz `id_token` w każdej token odpowiedzi:

```
{ 
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https://outlook.office.com/mail.read",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

Możesz dekodowanie i przeanalizować id_token, aby pobrać te same informacje otrzymane od profile_info.  Id_token jest JSON Web Token (JWT), o treści określonej przez łączenie OpenID.  Kod może powinien być tak bardzo podobne — wystarczy wyodrębnić środkowej części (treść) id_token i base64 dekodowanie go, aby uzyskać dostęp do obiektu JSON w obrębie.

W następnych dwóch tygodniach, należy kod aplikacji pobierania informacji dotyczących użytkownika z jednej `id_token` lub `profile_info`; zależnie od występuje.  Dzięki temu podczas wprowadzania zmian aplikacji bezproblemowo mogą obsługiwać przejścia od `profile_info` do `id_token` bez zakłóceń.

> [AZURE.IMPORTANT] **Zadanie: Upewnij się, aplikacji nie zależy od istnienia `profile_info` wartość.**

### <a name="removing-idtokenexpiresin"></a>Usuwanie id_token_expires_in
Podobne do `profile_info`, możemy również usunięcie `id_token_expires_in` parametru z odpowiedziami.  Wcześniej punkt końcowy 2.0 zwróci wartość `id_token_expires_in` wraz z każdym odpowiedzi id_token, na przykład w odpowiedzi Autoryzuj:

```
https://myapp.com?id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...&id_token_expires_in=3599...
```

Lub w token odpowiedzi:

```
{ 
    "token_type": "Bearer",
    "id_token_expires_in": 3599,
    "scope": "openid",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "profile_info": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

`id_token_expires_in` Wartość wskazuje liczbę sekund id_token czy są ważne.  Teraz możemy usuwanie `id_token_expires_in` wartość całkowicie.  Zamiast tego można użyć standardowego łączenie OpenID `nbf` i `exp` roszczeń sprawdzenie ważności id_token.  Zobacz [odwołania do tokenu 2.0 —](active-directory-v2-tokens.md) Aby uzyskać więcej informacji o tych roszczeń.

> [AZURE.IMPORTANT] **Zadanie: Upewnij się, aplikacji nie zależy od istnienia `id_token_expires_in` wartość.**


### <a name="changing-the-claims-returned-by-scopeopenid"></a>Zmienianie oświadczeniach zwracane przez zakres = openid
Ta zmiana będzie najważniejsze — w rzeczywistości, ma wpływ na niemal każdego aplikację, która używa punkt końcowy 2.0.  Wiele aplikacji wysyłać żądania do przy użyciu punktu końcowego 2.0 — `openid` zakresu, takich jak:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=...
&redirect_uri=...
&response_mode=form_post
&response_type=id_token
&scope=openid offline_access https://outlook.office.com/mail.read
```

Dzisiaj, gdy użytkownik udziela zgody na `openid` zakres, aplikacji otrzyma mnóstwo informacje o użytkowniku w wyniku id_token.  Roszczenia te mogą zawierać nazwy, preferowany nazwy użytkownika, adres e-mail i Identyfikatora obiektu.

W tej aktualizacji firma Microsoft zmieniania informacji o którą `openid` zakres zapewnia dostęp aplikacji do, aby lepiej comform z specyfikacji łączenie OpenID.  `openid` Zakres będzie tylko zezwolić aplikacji do zalogowania użytkownika w i odbierać identyfikator aplikacji specyficzne dla użytkownika w `sub` rościć sobie z id_token.  Roszczeń w id_token tylko z `openid` zakres udzielił będzie się pozbawione wszelkie informacje osobiste.  Przykład id_token roszczeń są:

```
{ 
    "aud": "580e250c-8f26-49d0-bee8-1c078add1609",
    "iss": "https://login.microsoftonline.com/b9410318-09af-49c2-b0c3-653adc1f376e/v2.0 ",
    "iat": 1449520283,
    "nbf": 1449520283,
    "exp": 1449524183,
    "nonce": "12345",
    "sub": "MF4f-ggWMEji12KynJUNQZphaUTvLcQug5jdF2nl01Q",
    "tid": "b9410318-09af-49c2-b0c3-653adc1f376e",
    "ver": "2.0",
}
```

Jeśli chcesz uzyskać identyfikowalne dane osobowe o użytkowniku w aplikacji aplikacji należy poprosić dodatkowe uprawnienia użytkownika.  Firma Microsoft są wprowadzenie obsługę dwie nowe zakresy z Specyfikacja łączenie OpenID — `email` i `profile` zakresów — zezwalające na to zgodę.

- `email` Zakres jest bardzo proste — umożliwia dostęp aplikacji do użytkownika podstawowy adres e-mail za pomocą `email` rościć sobie w id_token.  Należy zauważyć, że `email` roszczeń nie zawsze są obecne w id_tokens — też tylko zostanie on uwzględniony jeśli są dostępne w profilu użytkownika.
- `profile` Zakres zapewnia dostęp aplikacji do wszystkie inne podstawowe informacje o użytkowniku — ich nazwy, preferowany nazwa użytkownika, identyfikator obiektu i tak dalej.

Pozwala na kod aplikacji w sposób minimalnymi ujawnienie — poproś użytkownika po prostu zestaw informacji aplikacji wymaga jego pracy.  Jeśli chcesz kontynuować pobieranie pełny zestaw informacje o użytkowniku, która obecnie odbiera aplikacji, powinny zawierać wszystkie trzy zakresy w wezwaniach na autoryzacji:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=...
&redirect_uri=...
&response_mode=form_post
&response_type=id_token
&scope=openid profile email offline_access https://outlook.office.com/mail.read
```

Aplikację można rozpocząć wysyłanie `email` i `profile` natychmiast zakresów i punkt końcowy 2.0 będzie zaakceptować tych dwóch zakresów i rozpocząć żąda uprawnień użytkowników w razie potrzeby.  Jednak zmiany w interpretacji `openid` zakres nie zostaną wprowadzone przez kilka tygodni.

> [AZURE.IMPORTANT] **Zadanie: dodawanie `profile` i `email` zakresów, jeśli aplikacji wymaga informacje o użytkowniku.**  Należy zauważyć, że ADAL będzie zawierać oba te uprawnienia żądania domyślnie. 

### <a name="removing-the-issuer-trailing-slash"></a>Usuwanie wystawcy końcowe ułamkową.
Wcześniej wartość wystawcy, która pojawia się w tokeny od punktu końcowego 2.0 zajęła formularza

```
https://login.microsoftonline.com/{some-guid}/v2.0/
```

Miejsce, w którym identyfikator guid został tenantId dzierżawy Azure AD, wydające tokenu.  Wprowadzone zmiany staje się wartość wystawcy

```
https://login.microsoftonline.com/{some-guid}/v2.0 
```

w obu tokeny i w dokumencie odnajdywania łączenie OpenID.

> [AZURE.IMPORTANT] **Zadanie: Upewnij się, aplikacji akceptuje wartości wystawcy zarówno z i bez ukośnika podczas sprawdzania poprawności wystawcy.**

## <a name="why-change"></a>Dlaczego warto zmienić?
Podstawowy powód wprowadzenie tych zmian jest zgodny ze standardem specyfikacji OpenID połączenia.  Jest OpenID łączenie zgodności, możemy dotyczy zminimalizowania różnic między integracji z usługami firmy Microsoft tożsamości i z innymi usługami tożsamości w branży.  Chcemy programistom przy użyciu bibliotek uwierzytelniania ich Ulubione Otwórz źródło bez konieczności zmiany bibliotek, aby zezwalały na różnice firmy Microsoft.

## <a name="what-can-you-do"></a>Co można robić?
Dziś można rozpocząć tworzenie wszystkich zmian opisanych powyżej.  Należy natychmiast:

1.  **Usuń wszelkie zależności na `x5t` parametr nagłówek.**
2.  **Obsługiwać przejścia od `profile_info` do `id_token` w odpowiedziach token.**
3.  **Usuń wszelkie zależności na `id_token_expires_in` parametr odpowiedzi.**
3.  **Dodawanie `profile` i `email` zakresów do aplikacji, jeśli aplikacji wymaga informacji o użytkownikach podstawowy.**
4.  **Zaakceptuj wartości wystawcy tokenów zarówno z i bez ukośnika.**

Aby wprowadzić te zmiany, więc możesz go użyć jako odwołanie w ułatwianie aktualizacji kodu już zaktualizowano naszą [dokumentację Protocol (protokół) 2.0](active-directory-v2-protocols.md) .

Jeśli masz dalsze pytania dotyczące zakresu zmian, należy zachęcamy do bliższy kontakt z nami w serwisie Twitter u @AzureAD.

## <a name="how-often-will-protocol-changes-occur"></a>Jak często wystąpi zmiany Protocol (protokół)
Nie możemy przewidzieć, dodatkowo przerywanie o zmianach protokoły uwierzytelniania.  Te zmiany w jednej wersji celowo są zostać grupowania, dzięki czemu nie trzeba obsłużone tego typu proces aktualizacji ponownie dowolnej chwili wkrótce.  Oczywiście będziemy Dodawanie funkcji do Usługa uwierzytelniania zbieżnych 2.0, który można korzystać z, ale te zmiany należy dodatku i nie podział istniejący kod.

Ponadto chcemy powiedzieć Dziękujemy za testujesz rzeczy w okresie Podgląd.  Więcej informacji i doświadczeń naszych pilotażowe zostały cenne w dotychczasowych i firma Microsoft Życzymy nadal będziesz do udostępniania pomysłów i opinie.

Radosna kodowania!

Dział Microsoft tożsamości
