
<properties
    pageTitle="Tworzenie zaawansowanych reguł członkostwa w grupach w wersji preview usługi Azure Active Directory za pomocą atrybuty | Microsoft Azure"
    description="Jak utworzyć reguły zaawansowane Dynamiczna grupa członkostwa, między innymi obsługiwane operatory reguły wyrażeń i parametry."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/12/2016"
    ms.author="curtand"/>


# <a name="using-attributes-to-create-advanced-rules-for-group-membership-in-azure-active-directory-preview"></a>Tworzenie zaawansowanych reguł członkostwa w grupach w podglądzie usługi Azure Active Directory za pomocą atrybutów

Azure portal pozwala utworzyć reguły zaawansowane, aby włączyć bardziej złożone na atrybutach dynamiczne członkostwa grup Podgląd usługi Azure Active Directory (Azure AD). [Co to jest w podglądzie?](active-directory-preview-explainer.md) W tym artykule wyszczególniono atrybuty regułę i składni, aby utworzyć reguły zaawansowane.

## <a name="to-create-the-advanced-rule"></a>Aby utworzyć reguły zaawansowane

1.  Zaloguj się do [portalu Azure](https://portal.azure.com) za pomocą konta, który jest administratorem globalnym katalogu.

2.  Wybierz pozycję **więcej usług**, wprowadź **użytkowników i grup** w polu tekstowym, a następnie naciśnij **klawisz Enter**.

  ![Otwieranie, zarządzanie użytkownikami](./media/active-directory-groups-dynamic-membership-azure-portal/search-user-management.png)

3.  Na karta **Użytkownicy i grupy** zaznacz **wszystkie grupy**.

  ![Otwieranie karta grup](./media/active-directory-groups-dynamic-membership-azure-portal/view-groups-blade.png)

4. Na karta **użytkowników i grup — we wszystkich grupach** wybierz polecenie **Dodaj** .

  ![Dodawanie nowej grupy](./media/active-directory-groups-dynamic-membership-azure-portal/add-group-type.png)

5. Na karta **grupy** wprowadź nazwę i opis nowej grupy. Wybierz pozycję **Wpisz członkostwa** **Dynamiki** lub **Dynamiczne urządzenia**, w zależności od tego, czy chcesz utworzyć regułę dla użytkowników i urządzeń, a następnie wybierz **Dodaj dynamiczne kwerendy**. Atrybuty używane do reguły urządzenia zobacz [Używanie atrybutów tworzyć reguły dla obiektów urządzenia](#using-attributes-to-create-rules-for-device-objects).

  ![Dodawanie reguły dynamicznego członkostwa](./media/active-directory-groups-dynamic-membership-azure-portal/add-dynamic-group-rule.png)

6. Na karta **reguły dynamicznego członkostwa** wprowadź regułę w polu **Dodaj członkostwa dynamicznych zaawansowane regułę** , naciśnij klawisz Enter, a następnie wybierz **Utwórz** u dołu karta.

7. Wybierz opcję **Utwórz** na karta **grupy** , aby utworzyć grupę.

## <a name="constructing-the-body-of-an-advanced-rule"></a>Budowa treści reguły zaawansowanej

Zaawansowane reguły, którą można tworzyć dynamiczne członkostwa grup jest zasadniczo binarne wyrażenie, które składa się z trzech części i wyniki w wyniku wartość PRAWDA lub FAŁSZ. Są trzy elementy:

- Parametr po lewej stronie
- Operator binarny
- Prawy stała

Ukończono reguły zaawansowanej będzie podobny do tego: (leftParameter binaryOperator "RightConstant"), miejsce, w którym otwierający i zamykający nawias są wymagane dla całego wyrażenia binarne, podwójne cudzysłowy są wymagane do prawej stałą i składnia parametru po lewej stronie jest user.property. Zaawansowanej reguły mogą zawierać więcej niż jednego binarne wyrażeń oddzielonych i, -, lub i - operatorów logicznych nie.

Poniżej przedstawiono przykłady właściwie skonstruowanych reguły zaawansowane:

- (user.department - eq "Sprzedaż")- lub (user.department - eq "sprzedaży")
- (user.department - eq "Sprzedaż")- i - nie (user.jobTitle-zawiera "Które integrują usługi")

Aby uzyskać pełną listę parametrów obsługiwanych i operatorów reguły wyrażenia Zobacz sekcjach poniżej. Atrybuty używane do reguły urządzenia zobacz [Używanie atrybutów tworzyć reguły dla obiektów urządzenia](#using-attributes-to-create-rules-for-device-objects).

Całkowitą długość treści reguły zaawansowane nie może przekraczać 2048 znaków.

> [AZURE.NOTE]
>Operacje ciągu oraz regex są uwzględniana wielkość liter. Można również wykonać testy wartość Null, przy użyciu $null jako stałą, na przykład, user.department - eq $null.
Ciągi zawierające oferty "powinny być wyjściowym za pomocą" znak, na przykład user.department - eq \`"Sprzedaż".

## <a name="supported-expression-rule-operators"></a>Operatory reguły obsługiwane wyrażenia
W poniższej tabeli wymieniono wszystkie obsługiwane wyrażenia operatorów reguły i ich składni, może być używany w treści reguły zaawansowane:

| Operator        | W składni         |
|-----------------|----------------|
| Nie równa się      | -n            |
| Równa się          | -eq            |
| Nie zaczyna się od | -notStartsWith |
| Zaczyna się od     | -startsWith    |
| Nie zawiera    | -notContains   |
| Zawiera        | -zawiera      |
| Niezgodne       | -notMatch      |
| Uwzględnij           | -zgodne         |


## <a name="query-error-remediation"></a>Naprawy błędów kwerendy
W poniższej tabeli wymieniono potencjalne błędy i jak je poprawić ewentualnych

| Błąd analizy kwerendy     | Błąd zastosowania       | Poprawiony zastosowania             |
|-----------------------|-------------------|-----------------------------|
| Błąd: Atrybut nie jest obsługiwane.                                      | (user.invalidProperty - eq "Wartość")       | (user.department - eq "wartość")<br/>Właściwości powinny być zgodne z [obsługiwane listy właściwości](#supported-properties).                          |
| Błąd: Operator nie jest obsługiwany dla atrybutu.                       | (user.accountEnabled-zawiera PRAWDA)                                                                               | (user.accountEnabled - eq PRAWDA)<br/>Właściwość jest typu boolean. Obsługiwane operatory (-eq lub - n) na typ boolean z powyższej listy.                                                                                                                                   |
| Błąd: Błąd kompilacji kwerendy.                                      | (user.department - eq "Sprzedaż")- i (user.department - eq "Marketing")(user.userPrincipalName-match"*@domain.ext") | (user.department - eq "Sprzedaż")- i (user.department - eq "sprzedaży")<br/>Operator logiczny powinny być zgodne z listy obsługiwanych właściwości powyżej. (user.userPrincipalName-zgodne ".*@domain.ext")or(user.userPrincipalName -zgodne "@domain.ext$")Error w wyrażeniu regularnym. |
| Błąd: Wyrażenie binarne nie jest w formacie prawo.                     | (user.department — eq "Sprzedaż") (user.department - eq "Sprzedaż") (user.department-eq "Sprzedaż")                             | (user.accountEnabled - eq PRAWDA)- i (user.userPrincipalName-zawiera"alias@domain")<br/>Kwerenda zawiera wiele błędów. Nawias okrągły nie w odpowiednim miejscu.                                                                                                                            |
| Błąd: Nieznany błąd wystąpił podczas konfigurowania członkostwa dynamiczne. | (user.userPrincipalName i user.accountEnabled - eq "prawda"-zawiera"alias@domain")                               | (user.accountEnabled - eq PRAWDA)- i (user.userPrincipalName-zawiera"alias@domain")<br/>Kwerenda zawiera wiele błędów. Nawias okrągły nie w odpowiednim miejscu.                                                                                                                            |

## <a name="supported-properties"></a>Właściwości obsługiwane
Poniżej przedstawiono wszystkich właściwości użytkownika, które można użyć w regule zaawansowane:

### <a name="properties-of-type-boolean"></a>Właściwości wpisz wartość logiczna

Operatory dozwolonych

* -eq


* -n


| Właściwości     | Dopuszczalne wartości  | Użycie                          |
|----------------|-----------------|--------------------------------|
| accountEnabled | PRAWDA FAŁSZ      | user.accountEnabled - eq PRAWDA)  |
| dirSyncEnabled | PRAWDA, FAŁSZ, wartość null | (user.dirSyncEnabled - eq PRAWDA) |

### <a name="properties-of-type-string"></a>Właściwości typu ciąg

Operatory dozwolonych

* -eq


* -n


* -notStartsWith


* -StartsWith


* -zawiera


* -notContains


* -zgodne


* -notMatch

| Właściwości                 | Dopuszczalne wartości                                                                                        | Użycie                                                     |
|----------------------------|-------------------------------------------------------------------------------------------------------|-----------------------------------------------------------|
| Miasto                       | Dowolna wartość ciągu lub $null                                                                           | (user.city - eq "wartość")                                   |
| kraj                    | Dowolna wartość ciągu lub $null                                                                            | (user.country - eq "wartość")                                |
| Dział                 | Dowolna wartość ciągu lub $null                                                                          | (user.department - eq "wartość")                             |
| displayName                | Każda wartość ciągu                                                                                 | (user.displayName - eq "wartość")                            |
| facsimileTelephoneNumber   | Dowolna wartość ciągu lub $null                                                                           | (user.facsimileTelephoneNumber - eq "wartość")               |
| Imię                  | Dowolna wartość ciągu lub $null                                                                           | (user.givenName - eq "wartość")                              |
| Stanowisko                   | Dowolna wartość ciągu lub $null                                                                           | (user.jobTitle - eq "wartość")                               |
| Poczta                       | Dowolna wartość ciągu lub $null (adres SMTP użytkownika)                                                  | (user.mail - eq "wartość")                                   |
| mailNickName               | Każda wartość ciągu (alias e-mail użytkownika)                                                            | (user.mailNickName - eq "wartość")                           |
| urządzeń przenośnych                     | Dowolna wartość ciągu lub $null                                                                           | (user.mobile - eq "wartość")                                 |
| Identyfikator obiektu                   | Identyfikator GUID obiektu użytkownika                                                                            | (user.objectId - eq "1111111-1111-1111-1111-111111111111") |
| passwordPolicies           | Brak DisableStrongPassword DisablePasswordExpiration DisablePasswordExpiration, DisableStrongPassword |   (user.passwordPolicies - eq "DisableStrongPassword")                                                      |
| physicalDeliveryOfficeName | Dowolna wartość ciągu lub $null                                                                            | (user.physicalDeliveryOfficeName - eq "wartość")             |
| kod_pocztowy                 | Dowolna wartość ciągu lub $null                                                                            | (user.postalCode - eq "wartość")                             |
| preferredLanguage          | Kod ISO 639-1                                                                                        | (user.preferredLanguage - eq "en US")                      |
| sipProxyAddress            | Dowolna wartość ciągu lub $null                                                                            | (user.sipProxyAddress - eq "wartość")                        |
| Województwo                      | Dowolna wartość ciągu lub $null                                                                            | (user.state - eq "wartość")                                  |
| Adres              | Dowolna wartość ciągu lub $null                                                                            | (user.streetAddress - eq "wartość")                          |
| nazwisko                    | Dowolna wartość ciągu lub $null                                                                            | (user.surname - eq "wartość")                                |
| telephoneNumber            | Dowolna wartość ciągu lub $null                                                                            | (user.telephoneNumber - eq "wartość")                        |
| usageLocation              | Kod kraju dwóch literami                                                                           | (user.usageLocation - eq "USA")                             |
| userPrincipalName          | Każda wartość ciągu                                                                                     | (user.userPrincipalName - eq"alias@domain")               |
| userType                   | element członkowski gościa $null                                                                                    | (user.userType - eq "Członka")                              |

### <a name="properties-of-type-string-collection"></a>Właściwości zbioru typu ciąg

Operatory dozwolonych

* -zawiera


* -notContains

| Poperties      | Dopuszczalne wartości                        | Użycie                                                |
|----------------|---------------------------------------|------------------------------------------------------|
| otherMails     | Każda wartość ciągu                      | (user.otherMails-zawiera"alias@domain")           |
| proxyAddresses | SMTP: alias@domain smtp:alias@domain | (user.proxyAddresses-zawiera "SMTP:alias@domain") |

## <a name="extension-attributes-and-custom-attributes"></a>Rozszerzenie atrybuty i atrybuty niestandardowe
Rozszerzenie atrybuty i atrybuty niestandardowe są obsługiwane w reguły dynamicznego członkostwa.

Rozszerzenie atrybuty są synchronizowane z lokalnego serwera okna AD i mieć format "ExtensionAttributeX", gdzie X jest równe 1-15.
Oto przykład regułę, która korzysta z atrybutem rozszerzenia

(user.extensionAttribute15 - eq "Marketing")

Niestandardowe atrybuty są synchronizowane z lokalnego serwera Windows AD lub z połączonego aplikacji władz akredytacji bezpieczeństwa i formatu "user.extension_[GUID]\__ [atrybut]", gdzie [identyfikator GUID] jest unikatowym identyfikatorem w AAD dla aplikacji, która utworzyła atrybutu w AAD i [atrybut] to nazwa atrybutu, która została utworzona.
Na przykład regułę, która korzysta z atrybutem niestandardowym

User.extension_c272a57b722d4eb29bfe327874ae79cb__OfficeNumber  

Nazwa atrybutu niestandardowej można znaleźć w katalogu przez badanie użytkownika atrybut za pomocą Eksploratora wykresu i wyszukiwanie nazwy atrybutu.

## <a name="direct-reports-rule"></a>Reguła bezpośredni podwładni
Teraz można wypełnić członków w grupie na podstawie atrybutu menedżera użytkownika.

**Aby skonfigurować grupy jako grupy "Menedżer"**

1. Wykonaj kroki 1-5 w [celu utworzenia reguły zaawansowane](#to-create-the-advanced-rule), a następnie wybierz **Typ członkostwa** **Dynamiki**.

2. Na karta **reguły dynamicznego członkostwa** wprowadź reguły przy użyciu następującej składni:

    Bezpośrednie raportów dla *bezpośredni podwładni dla {obectID_of_manager}*. Na przykład prawidłowych regułę bezpośredni podwładni

                    Direct Reports for "62e19b97-8b3d-4d4a-a106-4ce66896a863”

    gdzie "62e19b97-8b3d-4d4a-a106-4ce66896a863" jest identyfikator obiektu menedżera. Identyfikator obiektu można znaleźć w Azure AD na **karcie profil** strony użytkownika dla użytkownika, który jest menedżerem.

3. Podczas zapisywania tej reguły, wszystkich użytkowników, które spełniają reguły zostaną połączone w członków grupy. Może potrwać kilka minut w grupie początkowo wypełnianie.


## <a name="using-attributes-to-create-rules-for-device-objects"></a>Tworzenie reguł obiektów urządzenia za pomocą atrybutów

Można również utworzyć regułę, która umożliwia wybranie obiektów urządzenia w celu członkostwo w grupie. Można używać następujących atrybutów urządzenia:

| Właściwości           | Dopuszczalne wartości                  | Użycie                                                |
|----------------------|---------------------------------|------------------------------------------------------|
| displayName          | Każda wartość ciągu                | (device.displayName - eq "Piotr Iphone")                 |
| deviceOSType         | Każda wartość ciągu                | (device.deviceOSType - eq "IOS")                      |
| deviceOSVersion      | Każda wartość ciągu                | (urządzenie. Element OSVersion - eq "9.1")                         |
| isDirSynced          | PRAWDA, FAŁSZ, wartość null                 | (device.isDirSynced - eq "prawda")                      |
| isManaged            | PRAWDA, FAŁSZ, wartość null                 | (device.isManaged - eq "false")                       |
| isCompliant          | PRAWDA, FAŁSZ, wartość null                 | (device.isCompliant - eq "prawda")                      |


## <a name="additional-information"></a>Dodatkowe informacje
Te artykuły zawierają dodatkowe informacje o grupach w usłudze Azure Active Directory.

* [Wyświetlanie istniejących grup](active-directory-groups-view-azure-portal.md)
* [Tworzenie nowej grupy i dodawanie członków](active-directory-groups-create-azure-portal.md)
* [Zarządzanie ustawieniami grupy](active-directory-groups-settings-azure-portal.md)
* [Zarządzanie członkostwami grupy](active-directory-groups-membership-azure-portal.md)
* [Zarządzanie regułami dynamiczne dla użytkowników w grupie](active-directory-groups-dynamic-membership-azure-portal.md)
