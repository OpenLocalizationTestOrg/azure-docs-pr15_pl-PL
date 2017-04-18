<properties
    pageTitle="Azure Active Directory inspekcji API reference | Microsoft Azure"
    description="Jak rozpocząć pracę z interfejsem API inspekcji usługi Azure Active Directory"
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
    ms.date="10/24/2016"
    ms.author="dhanyahk;markvi"/>

# <a name="azure-active-directory-audit-api-reference"></a>Azure Active Directory inspekcji interfejsu API odwołania

W tym temacie jest częścią zbiór tematów dotyczących usługi Azure Active Directory raportowania interfejsu API.  
Raportowanie Azure AD zawiera interfejs API, który umożliwia dostęp do danych inspekcji przy użyciu kodu lub pokrewnych narzędzi.
Zakres tego tematu ma dostarczać informacje na temat **inspekcji interfejsu API**.

Zobacz:

- [Dzienniki inspekcji](active-directory-reporting-azure-portal.md#audit-logs) , aby uzyskać więcej informacji koncepcyjnych
- Aby uzyskać więcej informacji na temat raportowania interfejsu API, [wprowadzenie Azure Active Directory raportowania API](active-directory-reporting-api-getting-started.md) .

Pytania, problemów lub opinii skontaktuj się z [AAD raportowania pomocy](mailto:aadreportinghelp@microsoft.com).


## <a name="who-can-access-the-data"></a>Kto może uzyskiwać dostęp do danych?

- Użytkownicy w roli administratora zabezpieczeń lub czytnika zabezpieczeń

- Administratorzy globalni

- Dowolną aplikację, która ma zezwolenia na dostęp do API (autoryzacji aplikacji można skonfigurować tylko na podstawie uprawnienia administratora globalnego)



## <a name="prerequisites"></a>Wymagania wstępne

Aby uzyskać dostęp do tego raportu za pośrednictwem interfejsu API raportów, należy najpierw:

- Usługi [Azure Active Directory bezpłatne lub lepiej edition](active-directory-editions.md)

- Ukończony [wymagania wstępne dotyczące dostępu Azure AD raportowania interfejsu API](active-directory-reporting-api-prerequisites.md). 
 

##<a name="accessing-the-api"></a>Uzyskiwanie dostępu do API

Możesz albo skorzystać z tego interfejsu API za pomocą [Eksploratora wykres](https://graphexplorer2.cloudapp.net) lub programowo przy użyciu, na przykład programu PowerShell. Aby programu PowerShell poprawnie interpretować składni filtr OData używanej w pozostałych wykresu AAD połączeń, należy użyć backtick (alias: akcent słaby) znaku "escape" znaku $. Znak backtick służy jako [znaku anulowania w programie PowerShell](https://technet.microsoft.com/library/hh847755.aspx), umożliwiając programu PowerShell wykonaj literałów interpretacji znaku $ i uniknąć kłopotliwe go jako nazwę zmiennej programu PowerShell (ie: $filter).

W tym temacie dotyczy w Eksploratorze wykresu. Na przykład programu PowerShell Zobacz tego [skryptu programu PowerShell](active-directory-reporting-api-audit-samples.md#powershell-script).

 
 

## <a name="api-endpoint"></a>Interfejs API punktu końcowego


Można uzyskać dostęp do tego interfejsu API za pomocą następujących identyfikatora URI:  

    https://graph.windows.net/contoso.com/activities/audit?api-version=beta

Nie istnieje limit liczby rekordów zwracanych przez interfejs API inspekcji Azure AD (za pomocą OData podziałem na strony).
Dla ograniczenia przechowywania danych raportowania zapoznaj się z [Zasad przechowywania raportowania](active-directory-reporting-retention.md).

To wywołanie zwraca dane partiami. Każdej partii może zawierać maksymalnie 1000 rekordów.  
Aby uzyskać następnej partii rekordów, użyj następnego łącza. Uzyskiwanie informacji skiptoken od pierwszego zestawu zwracane rekordy. Token Pomiń zostanie na końcu wynik ustawiony.  

    https://graph.windows.net/contoso.com/activities/audit?api-version=beta&%24skiptoken=-1339686058




## <a name="supported-filters"></a>Obsługiwane filtrów

Można ograniczyć liczbę rekordów, które są zwracane przez interfejs API połączenie w formularzu filtru.  
Dla interfejsu API logowania powiązane dane, są obsługiwane następujące filtry:

- **$top =\<liczby rekordów, które mają zostać zwrócone\> ** — Aby ograniczyć liczbę zwracanych rekordów. To jest drogich operacji. Nie należy używać ten filtr, jeśli chcesz zwrócić tysiące obiektów.     
- **$filter =\<instrukcji filtr\> ** —, aby określić, na podstawie pól filtrów obsługiwanych typów rekordów istotnych informacji



## <a name="supported-filter-fields-and-operators"></a>Pola obsługiwane filtrów i operatorów

Aby określić typ rekordów, który Cię interesuje, można utworzyć instrukcję filtru, która może zawierać jedną lub kombinacja następujące pola filtru:

- [daty](#activitydate) — określa datę lub zakres dat.
- [element activityType](#activitytype) - określa rodzaj działalności
- [działania](#activity) — określa działanie jako ciąg  
- [nazwisko i aktora](#actorname) - definiuje aktora w formularzu nazwisko aktora
- [Aktor-identyfikator obiektu](#actorobjectid) - definiuje aktora w formularzu ID aktora   
- [Aktor upn](#actorupn) - definiuje aktora w formularzu Aktor Nazwa zasady użytkownika (UPN) 
- [Nazwa/docelowej](#targetname) - definiuje obiekt docelowy w formularzu nazwisko aktora
- [Identyfikator docelowy obiektu](#targetobjectid) — definiuje obiekt docelowy w formularzu identyfikator elementu docelowego  
- [target-upn](#targetupn) - definiuje Aktor w formularzu Aktor Nazwa zasady użytkownika (UPN)   




----------

### <a name="activitydate"></a>Daty

**Obsługiwane operatory**: eq, stronę, le, BT, lt

**Przykład**:

    $filter=tdomain + 'activities/audit?api-version=beta&`$filter=eventTime gt ' + $7daysago    

**Notatki**:

Data/Godzina powinien być w formacie UTC

----------

### <a name="activitytype"></a>Element activityType

**Obsługiwane operatory**: eq

**Przykład**:

    $filter=activityType eq 'User'  

**Notatki**:

wielkość liter

----------

### <a name="activity"></a>działanie

**Obsługiwane operatory**: eq, zawiera startsWith

**Przykład**:

    $filter=activity eq 'Add application' or contains(activity, 'Application') or startsWith(activity, 'Add')   

**Notatki**:

wielkość liter

----------

### <a name="actorname"></a>nazwisko i aktora

**Obsługiwane operatory**: eq, zawiera startsWith

**Przykład**:

    $filter=actor/name eq 'test' or contains(actor/name, 'test') or startswith(actor/name, 'test')  

**Notatki**:

bez uwzględniania wielkości liter

    

----------
### <a name="actorobjectid"></a>Aktor-identyfikator obiektu

**Obsługiwane operatory**: eq

**Przykład**:

    $filter=actor/objectId eq 'e8096343-86a2-4384-b43a-ebfdb17600ba'    

----------
### <a name="targetname"></a>Nazwa/docelowej

**Obsługiwane operatory**: eq, zawiera startsWith

**Przykład**:

    $filter=targets/any(t: t/name eq 'some name')   

**Notatki**:

Bez uwzględniania wielkości liter

----------

### <a name="targetupn"></a>TARGET-głównej nazwy użytkownika

**Obsługiwane operatory**: eq, startsWith

**Przykład**:

    $filter=targets/any(t: startswith(t/Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity/userPrincipalName,'abc')) 

**Notatki**:

- Bez uwzględniania wielkości liter
- Musisz dodać pełną przestrzeń nazw podczas badania Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity

----------

### <a name="targetobjectid"></a>TARGET-identyfikator obiektu

**Obsługiwane operatory**: eq

**Przykład**:

    $filter=targets/any(t: t/objectId eq 'e8096343-86a2-4384-b43a-ebfdb17600ba')    

----------

### <a name="actorupn"></a>Aktor-głównej nazwy użytkownika

**Obsługiwane operatory**: eq, startsWith

**Przykład**:

    $filter=startswith(actor/Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity/userPrincipalName,'abc')  

**Notatki**:

- Bez uwzględniania wielkości liter 
- Musisz dodać pełną przestrzeń nazw podczas badania Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity

----------




## <a name="next-steps"></a>Następne kroki

- Czy chcesz zobaczyć przykłady działań filtrowanych systemu? Zapoznaj się z [usługi Azure Active Directory inspekcji interfejsu API próbki](active-directory-reporting-api-audit-samples.md).

- Czy chcesz dowiedzieć się więcej na temat Azure AD raportowania interfejsu API? Zobacz [Wprowadzenie do Azure Active Directory raportowania API](active-directory-reporting-api-getting-started.md).