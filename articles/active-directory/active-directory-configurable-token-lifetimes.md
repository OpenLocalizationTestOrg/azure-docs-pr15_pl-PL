<properties
   pageTitle="Można konfigurować istnienia Token w usłudze Azure Active Directory | Microsoft Azure"
   description="Ta funkcja jest używana przez administratorów i deweloperów Określ okresów tokeny wydawany przez Azure AD."
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
   ms.date="10/06/2016"
   ms.author="billmath"/>


# <a name="configurable-token-lifetimes-in-azure-active-directory-public-preview"></a>Można konfigurować istnienia Token w Azure Active Directory (publiczna wersja Preview)

>[AZURE.NOTE]
>Ta funkcja jest obecnie publicznej Podgląd.  Powinny być w stanie przywrócić lub usunąć wszelkie zmiany.  Firma Microsoft otwierają tę funkcję dla wszystkich osób spróbować podczas podglądu publicznej, jednak niektóre aspekty może wymagać [subskrypcji Azure AD Premium](active-directory-get-started-premium.md) , gdy ogólnodostępne.


## <a name="introduction"></a>Wprowadzenie
Ta funkcja jest używana przez administratorów i deweloperów Określ okresów tokeny wydawany przez Azure AD. Token istnienia można skonfigurować dla wszystkich aplikacji w dzierżawie, stosowania wielu dzierżawy lub dla określonych wystawcy usługi w dzierżawie.

W Azure AD obiektu zasad reprezentuje zestaw reguł wymuszane na poszczególnych aplikacji lub wszystkie aplikacje w dzierżawie.  Każdy typ zasad ma unikatową strukturę z zestawem właściwości, które zostaną zastosowane do obiektów, do których jest przydzielony.

Zasady może być wyznaczona jako domyślnego dla dzierżawy. Następnie tych zasad ma wpływ na dowolnej aplikacji, która znajduje się w którego dzierżawa, dopóki nie jest zastępowany przez zasady o wyższym priorytecie. Zasady można również przypisać do określonej aplikacji. Kolejności pierwszeństwa zależy od typu zasad.

Okres ważności tokenu zasad można skonfigurować dla tokeny odświeżania, tokeny dostępu tokeny sesji i tokeny identyfikator.


### <a name="access-tokens"></a>Tokeny dostępu
Token dostępu jest używany przez klienta, aby uzyskać dostęp do chronionego zasobu. Token dostępu można używać tylko dla określonych kombinacji użytkownika, klientów i zasobów. Tokeny dostępu nie odwołany i są ważne aż do ich wygaśnięcia. Złośliwy Aktor, który uzyskał token dostępu można go używać dla zakresu jej istnienia.  Dostosowywanie okres ważności tokenu dostępu jest zależność między zwiększanie wydajności systemu i zwiększenie czasu klient zachowuje programu access po wyłączeniu konta użytkownika.  Wydajność systemu ulepszone uzyskuje się ograniczając liczbę powtórzeń klienta musi uzyskać tokenu świeży dostępu. 

### <a name="refresh-tokens"></a>Odświeżanie tokenów
Gdy klient uzyskuje token dostępu, aby uzyskać dostęp do chronionego zasobu, otrzymuje zarówno tokenu odświeżania i token dostępu. Token odświeżania jest używany uzyskanie nowego programu access i Odśwież token pary po wygaśnięciu bieżącego tokenu dostępu. Odświeżanie tokeny są powiązane z kombinacji użytkownika i klienta. Można odwołać, a ich ważność jest sprawdzana za każdym razem, gdy są one używane.

Należy odróżnić klienci poufne i publicznej. Poufne klienci są aplikacje, które są umożliwia bezpieczne przechowywanie hasła klienta, co pozwala im udowodnić, że żądania są pochodzące z aplikacji klienckiej i nie złośliwy aktora. Jak przepływy są lepiej zabezpieczone czasu domyślny istnienia odświeżania tokeny wydawany przepływy są większe i nie można zmienić przy użyciu zasad.

Z powodu ograniczeń dla środowiska, które aplikacje są uruchamiane w klientów publicznych będą mogli bezpieczne przechowywanie hasła klienta. Zasady można ustawić zasobów uniemożliwienie tokenów odświeżania z klientów publicznych starsze niż za dany okres uzyskania nową parę token dostępu i Odśwież (odświeżanie Token Max nieaktywne czas).  Ponadto zasady można ustawić w okresie powyżej którego odświeżenie tokeny nie są już akceptowane (odświeżanie Token wiek Max).  Dostosowywanie okres ważności tokenu odświeżania pozwala kontrolować, kiedy i jak często użytkownika jest wymagana, aby ponownie wprowadzić poświadczenia zamiast bezgłośną ponownie uwierzytelniony podczas korzystania z aplikacji klienckiej publicznej.


### <a name="id-tokens"></a>Tokeny identyfikator
Identyfikator tokeny są przekazywane do witryny sieci web i klienci natywne i zawierają informacje profilu użytkownika. Token identyfikator jest związany z określoną kombinację użytkowników i klienta. Identyfikator tokeny są traktowane jako prawidłowe do czasu wygaśnięcia.  Zazwyczaj aplikacji sieci web odpowiada użytkownik istnienia sesji w aplikacji przez cały czas trwania token Identyfikatora wydanych dla użytkownika.  Dostosowywanie okres ważności tokenu identyfikator umożliwia sterowanie częstotliwość wygaśnie sesji aplikacji i użytkownik ponownie uwierzytelniania za pomocą Azure AD (bezgłośną lub interakcyjne) aplikacji sieci web.

### <a name="single-sign-on-session-token"></a>Tokenu sesji rejestracji jednokrotnej
Jeśli użytkownik uwierzytelnia się z usługą Azure Active Directory, jednej sesji logowania jednokrotnego jest nawiązywane z przeglądarką i Azure AD użytkownika.  Pojedynczy logowania jednokrotnego sesji Token, w postaci pliku cookie, reprezentuje w tej sesji. Należy pamiętać, że tokenu sesji logowania jednokrotnego nie jest powiązany z aplikacją określonego zasobu i klienta. Czy należy odwołać tokeny sesji logowania jednokrotnego i ważność jest zaznaczone pole wyboru za każdym razem, gdy są one używane.

Istnieją dwa rodzaje tokeny sesji logowania jednokrotnego. Tokeny trwałych sesji są przechowywane jako stałe pliki cookie przeglądarki i tokeny nietrwałe sesji są przechowywane jako pliki cookie sesji (są to zniszczenie po zamknięciu przeglądarki).

Tokeny sesji trwałych osoby, które nie mają istnienia 24-godzinnym trwałych tokeny muszą istnienia 180 dni. Każdym razem, gdy token sesji logowania jednokrotnego jest używany w okresie jej ważności okres ważności jest używane w innej 24 godziny lub 180 dni. Jeśli sesji logowania jednokrotnego token nie jest używany w okresie jej ważności uznaje wygasła i nie są akceptowane. 

Zasady można ustawić w okresie po pierwszej sesji, które token został wystawiony powyżej którego tokeny sesji nie są już (sesji Token wiek Max).  Dostosowywanie okres ważności tokenu sesji pozwala kontrolować, kiedy i jak często użytkownik musi ponownie wprowadzać poświadczeń zamiast bezgłośną uwierzytelnianie podczas korzystania z aplikacji sieci web.

### <a name="token-lifetime-policy-properties"></a>Właściwości zasad okres ważności tokenu
Zasady okres ważności tokenu jest typu obiektu zasad, który zawiera reguły okres ważności tokenu.  Właściwości zasad służą do sterowania określonej tokenu istnienia.  Jeśli zasady nie jest ustawiona, system wymusza wartość domyślna istnienia.


### <a name="configurable-token-lifetime-properties"></a>Okres ważności tokenu można konfigurować właściwości
Właściwość|Ciąg właściwości zasad|Dotyczy|Domyślne|Minimalna|Maksymalna|
----- | ----- | ----- |----- | ----- | ----- |
Okres ważności tokenu dostępu|  AccessTokenLifetime|Tokeny dostępu, identyfikator tokenów, tokeny SAML2|1 godzina|10 minut|1 dzień|
Odświeżanie Token maksymalny czas nieaktywny|    MaxInactiveTime |Odświeżanie tokenów |14 dni|10 minut|    90 dni|
Wiek Max Token odświeżania pojedynczy czynnik|    MaxAgeSingleFactor  |Odświeżanie tokeny (dla wszystkich użytkowników) |90 dni|10 minut |Poczekaj, aż odwołany *|
Odświeżanie wieloskładnikowe Token wieku Max| MaxAgeMultiFactor|  Odświeżanie tokeny (dla wszystkich użytkowników)| 90 dni|10 minut|Poczekaj, aż odwołany *|
Wiek Max tokenu sesji pojedynczy czynnik |MaxAgeSessionSingleFactor **    |Tokeny sesji (trwałych i nietrwałe)| Poczekaj, aż odwołany   |10 minut |Poczekaj, aż odwołany *|
Wiek budynku sesji wieloskładnikowe Token Max| MaxAgeSessionMultiFactor ***|    Tokeny sesji (trwałych i nietrwałe)| Poczekaj, aż odwołany|  10 minut| Poczekaj, aż odwołany *



- * 365 dni jest maksymalna długość jawne, którą można ustawić dla następujących atrybutów.
- ** Jeśli MaxAgeSessionSingleFactor nie jest ustawiona wartość ma wartość MaxAgeSingleFactor. Jeśli żadna ustawiona właściwość przyjmuje wartość domyślna (do odwołany).
- Jeśli MaxAgeSessionMultiFactor nie jest ustawiona wartość ma wartość MaxAgeMultiFactor. Jeśli żadna ustawiona właściwość przyjmuje wartość domyślna (do odwołany).

### <a name="exceptions"></a>Wyjątki
Właściwość|Dotyczy|Domyślne|
----- | ----- | ----- |
Odświeżanie Token nieaktywne maksymalny czas (federacyjnych użytkownikom informacje o odwołaniach niewystarczająca)|Odświeżanie tokeny (wystawiony dla federacyjnych użytkownikom informacje o odwołaniach niewystarczające)|12 godzin|
Odświeżanie Token maksymalny czas nieaktywny (poufne klienci)| Odświeżanie tokeny (wystawiony dla klientów poufne)|90 dni|
Odświeżanie token wieku Max (wystawiony dla klientów poufne) |   Odświeżanie tokeny (wystawiony dla klientów poufne) |Poczekaj, aż odwołany

### <a name="priority-and-evaluation-of-policies"></a>Priorytet i oceny zasad

Token zasady istnienia można tworzyć i przypisane do określonej aplikacji, dzierżaw i głównych usługi. Oznacza to, że jest możliwe dla wielu zasad zastosować do określonej aplikacji. Zasady okres ważności tokenu literom obowiązują następujące reguły:


- Jeśli zasady jest jawnie przypisane do wystawcy usługi, będą wymuszane. 
- Jeśli zasady nie jest jawnie przypisane do wystawcy usługi, zasady jawnie przypisane do dzierżawy nadrzędnej głównej usługi będą wymuszane. 
- Jeśli zasady nie jest jawnie przypisane do głównej usługi lub dzierżawy, będą wymuszane dla zasad przypisanych do aplikacji. 
- Jeśli zasady nie została przypisana do wystawcy usługi, dzierżawy lub obiekt aplikacji, wartości domyślne będą wymuszane (zobacz tabelę powyżej).

Aby uzyskać więcej informacji na relacji między obiektami aplikacji i usług kapitału w Azure AD zobacz [aplikacji i usług obiektów kapitału w usługi Azure Active Directory](active-directory-application-objects.md).

Ważność token jest obliczane w chwili, gdy są one używane. Zasady z najwyższy priorytet na pasku aplikacji, który jest dostępny jest stosowane.


>[AZURE.NOTE]
>Przykład
>
>Użytkownik chce uzyskać dostęp do 2 aplikacje sieci web, A i B. 
>
>
>- Obie aplikacje znajdują się w tej samej dzierżawy nadrzędnej. 
>- Okres ważności tokenu zasad 1 z sesji Token Max wiek osiem godzin jest ustawiona jako domyślne dzierżawy nadrzędnej.
>- Aplikacja sieci Web, A zostanie aplikacji sieci web normalnego użytkowania i nie jest połączony z jakiekolwiek zasady. 
>- Aplikacji sieci Web B jest używana dla procesów bardzo ważne i jego głównych usługi jest połączony z zasad okres ważności tokenu 2 z w wieku Max tokenu sesji 30 minut.
>
>Na 12:00 PM użytkownik otwiera nową sesję przeglądarki i próbuje uzyskać dostęp do aplikacji sieci web A. użytkownik jest przekierowywana do Azure AD i jest zostanie wyświetlony monit o zalogowanie się. To pomija plików cookie z tokenu sesji w przeglądarce. Użytkownik zostanie przekierowany z powrotem do odpowiedzi aplikacji z token Identyfikatora, który umożliwia dostęp do aplikacji w sieci web.
>
>Godzinie 12:15 użytkownik próbuje uzyskać dostęp do aplikacji sieci web B. Przeglądarka przekierowuje Azure AD, które wykrywa cookie sesji. Wystawcy usługi B aplikacji sieci Web jest połączony z zasad 1, ale jest również częścią dzierżawy nadrzędnego z domyślną zasadę 2. Zasady 2 są stosowane ponieważ zasady połączone z głównych usługi mają wyższy priorytet niż zasady domyślne dzierżawy. Token sesji pierwotnie został wystawiony w ciągu ostatnich 30 minut, aby został oznaczony jako prawidłowe. Użytkownik zostanie przekierowany z powrotem do aplikacji sieci web B z token identyfikator udzielanie do nich dostępu.
>
>Godzinie 13:00 użytkownik spróbuje, przechodząc do odpowiedzi aplikacji sieci web Użytkownik jest przekierowywana do Azure AD. Aplikacja sieci Web, A nie jest połączony z wszelkie zasady, ale ponieważ jest to w dzierżawie z domyślnych zasad 1 tych zasad jest stosowane. Wykryto plików cookie pierwotnie wystawiony w ostatnim 8 godzin i użytkownika sesji jest trybie cichym przekierowany z powrotem do aplikacji sieci web odpowiedzi przy użyciu nowego tokenu identyfikator bez konieczności uwierzytelniania.
>
>Użytkownik natychmiast próbuje uzyskać dostęp do aplikacji sieci web B. Użytkownik jest przekierowywana do Azure AD. Jak wcześniej, zasady 2 stosowane. Token został wystawiony dłużej niż 30 minut wcześniej, następnie monit o ponowne wprowadzenie poświadczeń i wydawane są zupełnie nową sesję i token Identyfikatora. Użytkownik może uzyskać dostęp aplikacji sieci web B.

## <a name="configurable-policy-properties-in-depth"></a>Zasady można konfigurować właściwości: szczegółowo

### <a name="access-token-lifetime"></a>Okres ważności tokenu dostępu

**Ciąg:** AccessTokenLifetime

**Dotyczy:** Tokeny dostępu, identyfikator tokenów

**Podsumowanie:** Tych zasad określa, jak długo trwa dostępu i tokeny identyfikator dla tego zasobu są traktowane jako prawidłowe. Zmniejszenie okres ważności tokenu dostępu zmniejsza zagrożenie ryzyko związane z programu access lub token Identyfikatora używany przez złośliwy Aktor przez dłuższy czas (zgodnie z ich nie odwołany), ale również negatywnie wpływa na wydajność tokeny ma zostać zamieniona częściej.

### <a name="refresh-token-max-inactive-time"></a>Odświeżanie token maksymalny czas nieaktywny

**Ciąg:** MaxInactiveTime

**Dotyczy:** Odświeżanie tokenów

**Podsumowanie:** Zasada ta kontroluje, jak stare token odświeżania może być, zanim klient można już go używać do pobierania nową parę token dostępu i Odśwież podczas próby dostępu do tego zasobu. Ponieważ nowy token odświeżania zazwyczaj zwracane jest token odświeżania jest używany, klienta musi nie osiągnięto dowolny zasób przy użyciu bieżącego tokenu odświeżania określony czas przed tych zasad uniemożliwi dostęp. 

Tych zasad spowoduje wymuszenie użytkowników, którzy nie są aktywne w swoich klientów uwierzytelnienie pobrać nowy token odświeżania. 

Należy pamiętać, że odświeżanie Token Max nieaktywne czas należy ustawić na mniejszą wartość niż wiek Max Token pojedynczy czynnik i wieloskładnikowe odświeżanie Token Max wiek.

### <a name="single-factor-refresh-token-max-age"></a>Odświeżanie pojedynczy czynnik token wieku max

**Ciąg:** MaxAgeSingleFactor

**Dotyczy:** Odświeżanie tokenów

**Podsumowanie:** To czasu określa zasady użytkownika nadal umożliwia uzyskiwanie dostępu do nowego programu access i Odśwież token pary po podczas ostatniego ich uwierzytelniony pomyślnie z pojedynczy czynnik tokeny odświeżania. Po użytkownika uwierzytelnia i otrzymuje nowy token odświeżania, będą mogli używać przepływu token odświeżania (pod warunkiem tokenu bieżącego odświeżania nie został odwołany i nie zostało nieużywane dłużej niż czas nieaktywne) dla określonego przedziału czasu. W tym momencie użytkownicy będą musieli uwierzytelnienie do odbierania nowy token odświeżania. 

Zmniejszenie maksymalny wiek spowoduje wymuszenie użytkownikom uwierzytelnianie częściej. Ponieważ jednym uwierzytelniania jest traktowany jako mniej bezpieczne niż uwierzytelnianie wieloskładnikowe, zalecane jest, że ten zasady są ustawione na wartość równe lub mniejsze niż wieloskładnikowe odświeżanie Token Max wieku zasady.

### <a name="multi-factor-refresh-token-max-age"></a>Odświeżanie wieloskładnikowe token wieku max

**Ciąg:** MaxAgeMultiFactor

**Dotyczy:** Odświeżanie tokenów

**Podsumowanie:** To czasu określa zasady użytkownika nadal umożliwia uzyskiwanie dostępu do nowego programu access i Odśwież token pary po podczas ostatniego ich uwierzytelniony pomyślnie z wielu tokeny odświeżania. Po użytkownika uwierzytelnia i otrzymuje nowy token odświeżania, będą mogli używać przepływu token odświeżania (pod warunkiem tokenu bieżącego odświeżania nie został odwołany i nie zostało nieużywane dłużej niż czas nieaktywne) dla określonego przedziału czasu. W tym momencie użytkownicy będą musieli uwierzytelnienie uzyskanie nowego tokenu odświeżania. 

Zmniejszenie maksymalny wiek spowoduje wymuszenie użytkownikom uwierzytelnianie częściej. Ponieważ jednym uwierzytelniania jest traktowany jako mniej bezpieczne niż uwierzytelnianie wieloskładnikowe, zaleca się tych zasad jest równa wartości równej lub większej niż pojedynczy czynnik odświeżanie Token Max wieku zasady.

### <a name="single-factor-session-token-max-age"></a>Wiek max tokenu sesji pojedynczy czynnik

**Ciąg:** MaxAgeSessionSingleFactor

**Dotyczy:** Tokeny sesji (trwałych i nietrwałe)

**Podsumowanie:** To określa zasady czasu, użytkownik może kontynuować za pomocą tokeny sesji uzyskać nowy identyfikator i sesji tokeny od czasu ostatniego uwierzytelnianie zakończyło się pomyślnie z pojedynczy czynnik. Po użytkownika uwierzytelnia i odbiera token nowej sesji, będą mogli używać przepływu token sesji (pod warunkiem tokenu bieżącej sesji jest odwołany ani nie wygasły) dla określonego przedziału czasu. W tym momencie użytkownicy będą musieli uwierzytelnienie do odbierania tokenu nowej sesji. 

Zmniejszenie maksymalny wiek spowoduje wymuszenie użytkownikom uwierzytelnianie częściej. Ponieważ jednym uwierzytelniania jest traktowany jako mniej bezpieczne niż uwierzytelnianie wieloskładnikowe, zaleca się tych zasad jest równa wartości równe lub mniejsze niż zasady wieloskładnikowe sesji do wieku Token Max.

### <a name="multi-factor-session-token-max-age"></a>Wiek max tokenu sesji wieloskładnikowe

**Ciąg:** MaxAgeSessionMultiFactor

**Dotyczy:** Tokeny sesji (trwałych i nietrwałe)

**Podsumowanie:** To określa zasady czasu, użytkownik może kontynuować za pomocą tokeny sesji uzyskać nowy identyfikator i sesji tokeny od czasu ostatniego uwierzytelnianie zakończyło się pomyślnie z wielu. Po użytkownika uwierzytelnia i odbiera token nowej sesji, będą mogli używać przepływu tokenu sesji (pod warunkiem tokenu bieżącej sesji jest odwołany ani nie wygasły) dla określonego przedziału czasu. W tym momencie użytkownicy będą musieli uwierzytelnienie do odbierania tokenu nowej sesji. 

Zmniejszenie maksymalny wiek spowoduje wymuszenie użytkownikom uwierzytelnianie częściej. Ponieważ jednym uwierzytelniania jest traktowany jako mniej bezpieczne niż uwierzytelnianie wieloskładnikowe, zaleca się tych zasad jest równa wartości równej lub większej niż zasady pojedynczy czynnik sesji do wieku Token Max.

## <a name="sample-token-lifetime-policies"></a>Przykładowe zasady okres ważności tokenu

Możliwość tworzenia i zarządzania nimi token istnienia dla aplikacji, głównych usługi i dzierżawy usługi ogólnego udostępnia wszystkie rodzaje nowych scenariuszy w Azure AD.  Firma Microsoft zacząć szczegółową kilka typowych scenariuszy zasady, które ułatwi wyznaczenia nowych zasad:


- Token istnienia
- Nieaktywne razy token Max
- Token wieku Max

Przedstawimy każde kilka scenariuszy, w tym:


- Zarządzanie dzierżawy domyślnych zasad
- Tworzenie zasad dla logowania w sieci Web
- Tworzenie zasad dla natywnej aplikacji nawiązywania połączeń z interfejs API sieci Web
- Zarządzanie zasadami zaawansowane 

### <a name="prerequisites"></a>Wymagania wstępne
W przykładowe scenariusze firma Microsoft będzie się tworzenie, aktualizowanie, łączenia i usuwanie zasad aplikacji, podmioty usługi i dzierżawy usługi ogólne.  Jeśli jesteś nowym użytkownikiem Azure AD wyewidencjonowania [w tym artykule](active-directory-howto-tenant.md) ułatwiające rozpoczęcie pracy przed kontynuowaniem próbki.  


1. Aby rozpocząć, Pobierz najnowszą [Podgląd polecenia Cmdlet Azure AD programu PowerShell](https://www.powershellgallery.com/packages/AzureADPreview). 
2.  Po umieszczeniu cmdlet Azure AD programu PowerShell, uruchom polecenie Połącz, aby zalogować się do konta administratora Azure AD. Musisz to zrobić przy każdym uruchomieniu nowej sesji.
        
        Connect-AzureAD -Confirm

3.  Uruchom następujące polecenie, aby wyświetlić wszystkie zasady, które zostały utworzone w dzierżawie.  Tego polecenia należy użyć po większość operacji w następujących scenariuszach.  Również ułatwi to uzyskać **Identyfikator obiektu** zasad. 
        
        Get-AzureADPolicy

### <a name="sample-managing-a-tenants-default-policy"></a>Przykład: Zarządzanie dzierżawy domyślnych zasad

W tym przykładzie zostanie utworzony zasady, które umożliwia użytkownikom rzadziej Zaloguj się w Twojej całej dzierżawy. 

Aby to zrobić, tworzymy zasady okres ważności tokenu odświeżanie tokeny pojedyncze czynniki, zastosowane przez dzierżawy usługi. Tych zasad zostaną zastosowane do wszystkich aplikacji w dzierżawie i każdej głównej usługi którym nie ma jeszcze zasady zestaw. 

1.  **Tworzenie zasad okres ważności tokenu.** 

Ustaw Token odświeżanie pojedynczy czynnik znaczeniu "do odwołany" nie wygaśnie, aż odebraniu dostępu.  Definicja zasad poniżej to, co możemy zostanie utworzony:
        
        @("{
          `"TokenLifetimePolicy`":
              {
                 `"Version`":1, 
                 `"MaxAgeSingleFactor`":`"until-revoked`"
              }
        }")

Następnie uruchom następujące polecenie, aby utworzyć tych zasad. 

        
    New-AzureADPolicy -Definition @("{`"TokenLifetimePolicy`":{`"Version`":1, `"MaxAgeSingleFactor`":`"until-revoked`"}}") -DisplayName TenantDefaultPolicyScenario -IsTenantDefault $true -Type TokenLifetimePolicy
        
Aby wyświetlić nowych zasad i uzyskać jej identyfikator obiektu, uruchom następujące polecenie.

    Get-AzureADPolicy
&nbsp;&nbsp;2. **Aktualizacja zasad**

Ustaleniu pierwszej zasady nie jest bardzo jako ściśle usługi wymaga i danego potrzebne do tokeny odświeżanie pojedynczy czynnik wygaśnie w ciągu 2 dni. Uruchom następujące polecenie. 
        
    Set-AzureADPolicy -ObjectId <ObjectID FROM GET COMMAND> -DisplayName TenantDefaultPolicyUpdatedScenario -Definition @("{`"TokenLifetimePolicy`":{`"Version`":1,`"MaxAgeSingleFactor`":`"2.00:00:00`"}}")
        
&nbsp;&nbsp;3. **Gotowe!** 

### <a name="sample-creating-a-policy-for-web-sign-in"></a>Przykład: Tworzenie zasad logowania w sieci web

W tym przykładzie zostanie utworzony zasad, która wymaga użytkowników do uwierzytelnienia częściej w aplikacji sieci Web. Tych zasad będzie wartość ważności tokeny dostępu i identyfikator i wiek Max tokenu sesji wieloskładnikowe główna usługi aplikacji sieci web.

1.  **Tworzenie zasad okres ważności tokenu.**

Tych zasad logowania w sieci Web będzie wartość Token dostępu i identyfikator długotrwałe i wiek tokenu sesji Max pojedynczy czynnik 2 godziny.

    New-AzureADPolicy -Definition @("{`"TokenLifetimePolicy`":{`"Version`":1,`"AccessTokenLifetime`":`"02:00:00`",`"MaxAgeSessionSingleFactor`":`"02:00:00`"}}") -DisplayName WebPolicyScenario -IsTenantDefault $false -Type TokenLifetimePolicy

Aby wyświetlić nowych zasad i uzyskać jej identyfikator obiektu, uruchom następujące polecenie.

    Get-AzureADPolicy
&nbsp;&nbsp;2. **przypisać zasady do swojego wystawcy usługi.**

Firma Microsoft zacząć łącze nowe zasady z wystawcy usługi.  Konieczne będzie również sposobem uzyskania dostępu do **identyfikator obiektu** głównej usługi. Można kwerend [Programu Microsoft Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity) lub przejdź do naszego [Narzędzie Eksploratora wykres](https://graphexplorer.cloudapp.net/) i zaloguj się do konta usługi Azure AD Aby wyświetlić wszystkie Twojej dzierżawy usługi podmiotów. 

Po umieszczeniu **identyfikator obiektu**, uruchom następujące polecenie.
        
    Add-AzureADServicePrincipalPolicy -ObjectId <ObjectID of the Service Principal> -RefObjectId <ObjectId of the Policy>
&nbsp;&nbsp;3. **Gotowe!** 

 

### <a name="sample-creating-a-policy-for-native-apps-calling-a-web-api"></a>Przykład: Tworzenie zasad natywnej aplikacji nawiązywania połączeń z interfejs API sieci Web

>[AZURE.NOTE]
>Łączenie zasad aplikacjom jest wyłączony.  Pracujemy nad tym wkrótce włączenie.  Ta strona zostanie zaktualizowana, jak ta funkcja jest dostępna.

W tym przykładzie pracujemy utworzy zasady, które wymagają użytkownikom uwierzytelnianie mniejsze i będzie wydłużyć czas, który można je nieaktywne bez konieczności uwierzytelniania ponownie. Zasady zostaną zastosowane do interfejsu API sieci Web, w ten sposób, gdy natywnej aplikacji zażąda go jako zasobu, do którego tych zasad zostaną zastosowane.

1.  **Tworzenie zasad okres ważności tokenu.** 

To polecenie utworzy ścisłych zasad dla interfejs API sieci Web. 
        
    New-AzureADPolicy -Definition @("{`"TokenLifetimePolicy`":{`"Version`":1,`"MaxInactiveTime`":`"30.00:00:00`",`"MaxAgeMultiFactor`":`"until-revoked`",`"MaxAgeSingleFactor`":`"180.00:00:00`"}}") -DisplayName WebApiDefaultPolicyScenario -IsTenantDefault $false -Type TokenLifetimePolicy
         
Aby wyświetlić nowych zasad i uzyskać jej identyfikator obiektu, uruchom następujące polecenie.

    Get-AzureADPolicy

&nbsp;&nbsp;2. **przypisać zasady interfejsu API usług sieci Web**.

Firma Microsoft zacząć łącze nowe zasady za pomocą aplikacji.  Konieczne będzie również sposobem uzyskania dostępu do **identyfikator obiektu** aplikacji. [Azure Portal](https://portal.azure.com/)za pomocą jest najlepszy sposób na znalezienie Twojej aplikacji **identyfikator obiektu** . 

Po umieszczeniu **identyfikator obiektu**, uruchom następujące polecenie.

    Add-AzureADApplicationPolicy -ObjectId <ObjectID of the App> -RefObjectId <ObjectId of the Policy>

&nbsp;&nbsp;3. **Gotowe!** 

### <a name="sample-managing-an-advanced-policy"></a>Przykład: Zarządzanie zasadami zaawansowane 

W tym przykładzie zostanie utworzony kilka zasad wykazać, jak działa system priorytet i jak można zarządzać wieloma zasadami zastosowany do kilku obiektów. Udostępnia kilka wgląd priorytet zasady wyjaśniono powyżej i będzie także ułatwiają zarządzanie bardziej złożonych zadań. 

1.  **Tworzenie zasad okres ważności tokenu**

Pory bardzo proste. Firma Microsoft została utworzona dzierżawy domyślnych zasad, który ustawia pojedynczy czynnik odświeżanie Token istnienia do 30 dni. 

    New-AzureADPolicy -Definition @("{`"TokenLifetimePolicy`":{`"Version`":1,`"MaxAgeSingleFactor`":`"30.00:00:00`"}}") -DisplayName ComplexPolicyScenario -IsTenantDefault $true -Type TokenLifetimePolicy
Aby wyświetlić nowych zasad i wykonywanie jest identyfikator obiektu, uruchom następujące polecenie.
 
    Get-AzureADPolicy

&nbsp;&nbsp;2. **Przypisywanie zasad wystawcy usługi**

Teraz mamy zasady na całej dzierżawy.  Załóżmy, że chcemy zachować tych zasad 30 dni podmiotu określonej usługi, ale zmienić domyślną zasadę dzierżawy za górną granicę "do odwołany". 

Najpierw możemy zacząć łącze nowe zasady z naszych wystawcy usługi.  Konieczne będzie również sposobem uzyskania dostępu do **identyfikator obiektu** głównej usługi. Można kwerend [Programu Microsoft Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity) lub przejdź do naszego [Narzędzie Eksploratora wykres](https://graphexplorer.cloudapp.net/) i zaloguj się do konta usługi Azure AD Aby wyświetlić wszystkie Twojej dzierżawy usługi podmiotów. 

Po umieszczeniu **identyfikator obiektu**, uruchom następujące polecenie.

    Add-AzureADServicePrincipalPolicy -ObjectId <ObjectID of the Service Principal> -RefObjectId <ObjectId of the Policy>

&nbsp;&nbsp;3. **ustawić flagę IsTenantDefault się FAŁSZ przy użyciu następującego polecenia**. 

    Set-AzureADPolicy -ObjectId <ObjectId of Policy> -DisplayName ComplexPolicyScenario -IsTenantDefault $false
&nbsp;&nbsp;4. **Utwórz nową zasadę domyślne dzierżawy**

    New-AzureADPolicy -Definition @("{`"TokenLifetimePolicy`":{`"Version`":1,`"MaxAgeSingleFactor`":`"until-revoked`"}}") -DisplayName ComplexPolicyScenarioTwo -IsTenantDefault $true -Type TokenLifetimePolicy

&nbsp;&nbsp;5. **Gotowe!** 

Teraz masz zasad oryginalnych związane z Twojej wystawcy usługi i nowych zasad, ustawianie jako domyślnych zasad dzierżawy.  Należy pamiętać, że zasady zastosowane do głównych usługi mają priorytet przez zasady domyślne dzierżawy. 


## <a name="cmdlet-reference"></a>Informacje dotyczące poleceń cmdlet

### <a name="manage-policies"></a>Zarządzanie zasadami
Następujące polecenia cmdlet może służyć do zarządzania zasadami.</br></br>



#### <a name="new-azureadpolicy"></a>Nowy AzureADPolicy
Tworzy nową zasadę.

    New-AzureADPolicy -Definition <Array of Rules> -DisplayName <Name of Policy> -IsTenantDefault <boolean> -Type <Policy Type> 

Parametry|Opis|Przykład|
-----| ----- |-----|
-Definicja| Tablica stringified JSON, zawierającego wszystkie reguły zasad.|-Definition @("{`"TokenLifetimePolicy`":{`"Version`":1,`"MaxInactiveTime`":`"20:00:00`"}}") 
-DisplayName|Ciąg o nazwie zasad|MyTokenPolicy - DisplayName
-IsTenantDefault|Jeśli wartość PRAWDA ustawia zasady jako dzierżawcy domyślnych zasad, jeśli ma wartość FAŁSZ nie działają|-IsTenantDefault $true
— Typ|Typ zasady, zawsze token istnienia za pomocą "TokenLifetimePolicy"|— TokenLifetimePolicy typ
-AlternativeIdentifier [opcjonalne]|Ustawia alternatywny identyfikator zasady.|MyAltId - AlternativeIdentifier
</br></br>

#### <a name="get-azureadpolicy"></a>Get-AzureADPolicy         
Pobiera wszystkie zasady AzureAD lub określonej zasady 
        
    Get-AzureADPolicy 

Parametry|Opis|Przykład|
-----| ----- |-----|
Identyfikator obiektu-[opcjonalne]|Obiekt identyfikator zasad chcesz przejść. |Identyfikator obiektu - &lt;identyfikator obiektu zasad&gt; 
</br></br>

#### <a name="get-azureadpolicyappliedobject"></a>Get-AzureADPolicyAppliedObject         
Pobiera wszystkie aplikacje i usługi podmioty połączone z zasady
        
    Get-AzureADPolicyAppliedObject -ObjectId <object id of policy> 

Parametry|Opis|Przykład|
-----| ----- |-----|
-Identyfikator obiektu|Obiekt identyfikator zasad chcesz przejść.|Identyfikator obiektu - &lt;identyfikator obiektu zasad&gt;
</br></br>

#### <a name="set-azureadpolicy"></a>Ustawianie AzureADPolicy
Aktualizacje istniejących zasad
        
    Set-AzureADPolicy -ObjectId <object id of policy> -DisplayName <string> 
 
Parametry|Opis|Przykład|
-----| ----- |-----|
-Identyfikator obiektu|Obiekt identyfikator zasad chcesz przejść.|Identyfikator obiektu - &lt;identyfikator obiektu zasad&gt;
-DisplayName|Ciąg o nazwie zasad|MyTokenPolicy - DisplayName
-Definicja [opcjonalne]|Tablica stringified JSON, zawierającego wszystkie reguły zasad.|-Definition @("{`"TokenLifetimePolicy`":{`"Version`":1,`"MaxInactiveTime`":`"20:00:00`"}}") 
-IsTenantDefault [opcjonalne]|Jeśli wartość PRAWDA ustawia zasady jako dzierżawcy domyślnych zasad, jeśli ma wartość FAŁSZ nie działają|-IsTenantDefault $true
— Typ [opcjonalne]|Typ zasady, zawsze token istnienia za pomocą "TokenLifetimePolicy"|— TokenLifetimePolicy typ
-AlternativeIdentifier [opcjonalne]|Ustawia alternatywny identyfikator zasady.|MyAltId - AlternativeIdentifier
</br></br>

#### <a name="remove-azureadpolicy"></a>Usuń AzureADPolicy         
Usuwa określoną zasadę

     Remove-AzureADPolicy -ObjectId <object id of policy>

Parametry|Opis|Przykład|
-----| ----- |-----|
-Identyfikator obiektu|Obiekt identyfikator zasad chcesz przejść.|Identyfikator obiektu - &lt;identyfikator obiektu zasad&gt;
</br></br>

### <a name="application-policies"></a>Zasady aplikacji
Następujące polecenia cmdlet może być używany do zasad aplikacji.</br></br>

#### <a name="add-azureadapplicationpolicy"></a>Dodawanie AzureADApplicationPolicy         
Łącza określonych zasad aplikacji

    Add-AzureADApplicationPolicy -ObjectId <object id of application> -RefObjectId <object id of policy>

Parametry|Opis|Przykład|
-----| ----- |-----|
-Identyfikator obiektu|Identyfikator aplikacji obiektu.|Identyfikator obiektu - &lt;identyfikator obiektu aplikacji&gt; 
-RefObjectId|Identyfikator zasad obiektu. |-RefObjectId &lt;identyfikator obiektu zasad&gt;
</br></br>

#### <a name="get-azureadapplicationpolicy"></a>Get-AzureADApplicationPolicy        
Otrzymuje zasad przydzielonych do aplikacji

    Get-AzureADApplicationPolicy -ObjectId <object id of application>

Parametry|Opis|Przykład|
-----| ----- |-----|
-Identyfikator obiektu|Identyfikator aplikacji obiektu.|Identyfikator obiektu - &lt;identyfikator obiektu aplikacji&gt; 
</br></br>

#### <a name="remove-azureadapplicationpolicy"></a>Usuń AzureADApplicationPolicy        
Usuwa zasady z aplikacji

    Remove-AzureADApplicationPolicy -ObjectId <object id of application> -PolicyId <object id of policy>

Parametry|Opis|Przykład|
-----| ----- |-----|
-Identyfikator obiektu|Identyfikator aplikacji obiektu.|Identyfikator obiektu - &lt;identyfikator obiektu aplikacji&gt; 
-PolicyId| Identyfikator obiektu zasad.|-PolicyId &lt;identyfikator obiektu zasad&gt;
</br></br>

### <a name="service-principal-policies"></a>Podstawowe zasady usługi
Następujące polecenia cmdlet może służyć zasady głównej usługi.</br></br>

#### <a name="add-azureadserviceprincipalpolicy"></a>Dodawanie AzureADServicePrincipalPolicy         
Linki do określonej zasady do wystawcy usługi

    Add-AzureADServicePrincipalPolicy -ObjectId <object id of service principal> -RefObjectId <object id of policy>

Parametry|Opis|Przykład|
-----| ----- |-----|
-Identyfikator obiektu|Identyfikator aplikacji obiektu.|Identyfikator obiektu - &lt;identyfikator obiektu aplikacji&gt; 
-RefObjectId|Identyfikator zasad obiektu. |-RefObjectId &lt;identyfikator obiektu zasad&gt;
</br></br>

#### <a name="get-azureadserviceprincipalpolicy"></a>Get-AzureADServicePrincipalPolicy        
Pobiera wszystkie zasady połączone z kapitału określonej usługi

    Get-AzureADServicePrincipalPolicy -ObjectId <object id of service principal>
 
Parametry|Opis|Przykład|
-----| ----- |-----|
-Identyfikator obiektu|Identyfikator aplikacji obiektu.|Identyfikator obiektu - &lt;identyfikator obiektu aplikacji&gt; 
</br></br>

#### <a name="remove-azureadserviceprincipalpolicy"></a>Usuń AzureADServicePrincipalPolicy         
Usuwa zasady z określonej głównej usługi.

    Remove-AzureADServicePrincipalPolicy -ObjectId <object id of service principal>  -PolicyId <object id of policy>
 
Parametry|Opis|Przykład|
-----| ----- |-----|
-Identyfikator obiektu|Identyfikator aplikacji obiektu.|Identyfikator obiektu - &lt;identyfikator obiektu aplikacji&gt; 
-PolicyId| Identyfikator obiektu zasad.|-PolicyId &lt;identyfikator obiektu zasad&gt;
