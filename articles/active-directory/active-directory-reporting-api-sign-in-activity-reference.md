<properties
    pageTitle="Raportu działania logowania w usłudze Azure Active Directory API reference | Microsoft Azure"
    description="Odwołanie do interfejsu API raportów aktywności logowania usługi Azure Active Directory"
    services="active-directory"
    documentationCenter=""
    authors="dhanyahk"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/25/2016"
    ms.author="dhanyahk;markvi"/>

# <a name="azure-active-directory-sign-in-activity-report-api-reference"></a>Azure Active Directory logowania raportu o działaniach interfejsu API odwołania


W tym temacie jest częścią zbiór tematów dotyczących usługi Azure Active Directory raportowania interfejsu API.  
Raportowanie Azure AD zawiera interfejs API, który umożliwia dostęp do danych z raportu aktywności logowania przy użyciu kodu lub pokrewnych narzędzi.
Zakres tego tematu ma dostarczać informacje na temat **logowania interfejsu API raportów aktywności**.

Zobacz:

- Więcej informacji koncepcyjnych [logowania działania](active-directory-reporting-azure-portal.md#sign-in-activities)
- Aby uzyskać więcej informacji na temat raportowania interfejsu API, [wprowadzenie Azure Active Directory raportowania API](active-directory-reporting-api-getting-started.md) .

Pytania, problemów lub opinii skontaktuj się z [AAD raportowania pomocy](mailto:aadreportinghelp@microsoft.com).



## <a name="who-can-access-the-api-data"></a>Kto może uzyskać dostęp do danych interfejsu API?

- Użytkownicy w roli administratora zabezpieczeń lub czytnika zabezpieczeń

- Administratorzy globalni

- Dowolną aplikację, która ma zezwolenia na dostęp do API (autoryzacji aplikacji można skonfigurować tylko na podstawie uprawnienia administratora globalnego)



## <a name="prerequisites"></a>Wymagania wstępne

Aby uzyskać dostęp do tego raportu za pośrednictwem raportowania interfejsu API, musisz mieć:

- Usługi [Azure Active Directory Premium P1 i P2 edition](active-directory-editions.md)

- Ukończony [wymagania wstępne dotyczące dostępu Azure AD raportowania interfejsu API](active-directory-reporting-api-prerequisites.md). 


##<a name="accessing-the-api"></a>Uzyskiwanie dostępu do API

Możesz albo skorzystać z tego interfejsu API za pomocą [Eksploratora wykresu](https://graphexplorer2.cloudapp.net) lub programowo przy użyciu, na przykład programu PowerShell. Aby programu PowerShell poprawnie interpretować składni filtr OData używanej w pozostałych wykresu AAD połączeń, należy użyć backtick (alias: akcent słaby) znaku "escape" znaku $. Znak backtick służy jako [znaku anulowania programu PowerShell i](https://technet.microsoft.com/library/hh847755.aspx)zezwolenia programu PowerShell wykonaj literałów interpretacji znaku $ i uniknąć mylące go jako nazwę zmiennej programu PowerShell (ie: $filter).

W tym temacie dotyczy w Eksploratorze wykresu. Na przykład programu PowerShell Zobacz tego [skryptu programu PowerShell](active-directory-reporting-api-sign-in-activity-samples.md#powershell-script).


## <a name="api-endpoint"></a>Interfejs API punktu końcowego

Można uzyskać dostęp do tego interfejsu API za pomocą następujących podstawowy identyfikator URI:  
    
    https://graph.windows.net/contoso.com/activities/signinEvents?api-version=beta  



Ze względu na ilości danych tego interfejsu API ma ograniczenie miliona zwrócone rekordy. 

To wywołanie zwraca dane partiami. Każdej partii może zawierać maksymalnie 1000 rekordów.  
Aby uzyskać następnej partii rekordów, użyj następnego łącza. Uzyskiwanie informacji [skiptoken](https://msdn.microsoft.com/library/dd942121.aspx) od pierwszego zestawu zwracane rekordy. Token Pomiń zostanie na końcu wynik ustawiony.  

    https://graph.windows.net/$tenantdomain/activities/signinEvents?api-version=beta&%24skiptoken=-1339686058


## <a name="supported-filters"></a>Obsługiwane filtrów

Można ograniczyć liczbę rekordów, które są zwracane przez interfejs API połączeń w formularzu filtru.  
Dla interfejsu API logowania powiązane dane, są obsługiwane następujące filtry:

- **$top =\<liczby rekordów, które mają zostać zwrócone\> ** — Aby ograniczyć liczbę zwracanych rekordów. To jest drogich operacji. Nie należy używać ten filtr, jeśli chcesz zwrócić tysiące obiektów.  
- **$filter =\<instrukcji filtr\> ** —, aby określić, na podstawie pól filtrów obsługiwanych typów rekordów istotnych informacji



## <a name="supported-filter-fields-and-operators"></a>Pola obsługiwane filtrów i operatorów

Aby określić typ rekordów, który Cię interesuje, można utworzyć instrukcję filtru, która może zawierać jedną lub kombinacja następujące pola filtru:

- [signinDateTime](#signindatetime) - określa datę lub zakres dat.

- [Nazwa użytkownika](#userid) — określa określonego użytkownika podstawie identyfikator użytkownika.

- [userPrincipalName](#userprincipalname) - definiuje określonego użytkownika podstawie głównej nazwy użytkownika użytkownika (UPN)

- [Identyfikator aplikacji](#appid) — określa konkretnej aplikacji podstawie identyfikator aplikacji

- [appDisplayName](#appdisplayname) - określa konkretnej aplikacji podstawie nazwę wyświetlaną tej aplikacji

- [loginStatus](#loginStatus) - Określa stan logowania (sukcesu-błąd)


> [AZURE.NOTE] Podczas używania Eksploratora wykresu, możesz potrzeby używania o odpowiedniej wielkości liter dla każdej litery jest pól filtru.


Aby zawęzić zakres zwrócone dane, można tworzyć połączenia obsługiwane filtry i pola filtru. Na przykład następująca instrukcja zwraca górny 10 rekordów między 2016 1 lipca i 2016 6 lipca:

    https://graph.windows.net/contoso.com/activities/signinEvents?api-version=beta&$top=10&$filter=signinDateTime+ge+2016-07-01T17:05:21Z+and+signinDateTime+le+2016-07-07T00:00:00Z


----------

### <a name="signindatetime"></a>signinDateTime

**Obsługiwane operatory**: eq, stronę, le, BT, lt

**Przykład**:

Przy użyciu określonej daty

    $filter=signinDateTime+eq+2016-04-25T23:59:00Z  



Za pomocą zakres dat    

    $filter=signinDateTime+ge+2016-07-01T17:05:21Z+and+signinDateTime+le+2016-07-07T17:05:21Z


**Notatki**:

Parametr daty i godziny powinien być w formacie UTC 


----------

### <a name="userid"></a>Nazwa użytkownika

**Obsługiwane operatory**: eq

**Przykład**:

    $filter=userId+eq+’00000000-0000-0000-0000-000000000000’

**Notatki**:

Wartość identyfikatora użytkownika jest wartość ciągu



----------

### <a name="userprincipalname"></a>userPrincipalName

**Obsługiwane operatory**: eq

**Przykład**:

    $filter=userPrincipalName+eq+'audrey.oliver@wingtiptoysonline.com' 


**Notatki**:

Wartość argumentu userPrincipalName jest wartość ciągu

----------

### <a name="appid"></a>Identyfikator aplikacji

**Obsługiwane operatory**: eq

**Przykład**:

    $filter=appId+eq+’00000000-0000-0000-0000-000000000000’



**Notatki**:

Wartość Identyfikator aplikacji jest wartość ciągu

----------


### <a name="appdisplayname"></a>appDisplayName

**Obsługiwane operatory**: eq

**Przykład**:

    $filter=appDisplayName+eq+'Azure+Portal' 


**Notatki**:

Wartość argumentu appDisplayName jest wartość ciągu

----------

### <a name="loginstatus"></a>loginStatus

**Obsługiwane operatory**: eq

**Przykład**:

    $filter=loginStatus+eq+'1'  


**Notatki**:

Istnieją dwie opcje loginStatus: 0 - Sukces, 1 - błąd

----------



## <a name="next-steps"></a>Następne kroki

- Czy chcesz wyświetlić przykłady odfiltrowanych działań logowania? Zapoznaj się z [próbki raportu interfejsu API logowania działania usługi Azure Active Directory](active-directory-reporting-api-sign-in-activity-samples.md).

- Czy chcesz dowiedzieć się więcej na temat Azure AD raportowania interfejsu API? Zobacz [Wprowadzenie do Azure Active Directory raportowania API](active-directory-reporting-api-getting-started.md).