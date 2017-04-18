<properties
    pageTitle="Azure AD Connect synchronizacją: opis deklaracyjnych inicjowania obsługi administracyjnej wyrażenia | Microsoft Azure"
    description="W tym artykule wyjaśniono deklaracyjnych wyrażeń obsługi administracyjnej."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/31/2016"
    ms.author="markusvi;andkjell"/>


# <a name="azure-ad-connect-sync-understanding-declarative-provisioning-expressions"></a>Azure AD Connect synchronizacją: opis deklaracyjnych inicjowania obsługi administracyjnej wyrażeń
Azure AD Connect synchronizacją opiera się na deklaracyjnych inicjowania obsługi administracyjnej po raz pierwszy wprowadzone w programie Forefront Identity Manager 2010. Umożliwia wdrożenie logiki biznesowej integracji pełną tożsamości bez konieczności pisać kod skompilowany.

Podstawowe części deklaracyjnych inicjowania obsługi administracyjnej jest język wyrażeń w przepływach atrybut. Język używany jest podzbiorem języka Visual Basic for Applications (VBA) Microsoft®. Ten język jest używany w programie Microsoft Office i użytkownikom obsługi języka VBScript również rozpozna ją. Wyrażenie języka deklaracyjnych inicjowania obsługi administracyjnej tylko przy użyciu funkcji i nie są strukturalnych języka. Nie ma żadnych metod lub instrukcji. Funkcje są zagnieżdżone zamiast tego przepływu express programu.

Aby uzyskać więcej informacji zobacz [Witamy w języku Visual Basic for Applications dokumentacja języka dla pakietu Office 2013](https://msdn.microsoft.com/library/gg264383.aspx).

Atrybuty są jednoznacznie określone typy. Funkcja akceptuje tylko atrybuty typu poprawna. Ponadto jest uwzględniana wielkość liter. Zarówno funkcja nazw i atrybutu musi być z.wielkiej lub jest generowany błąd.

## <a name="language-definitions-and-identifiers"></a>Identyfikatory i definicje języka

- Funkcje mają nazwę po przez argumenty w nawiasach: NazwaFunkcji (argument 1, argument N).
- Atrybuty są oznaczane w nawiasach kwadratowych: [Nazwa_atrybutu]
- Parametry są oznaczane procentu: Nazwa parametru %
- Stałe w postaci ciągów są ujęte w cudzysłowy: na przykład "Contoso" (Uwaga: należy użyć cudzysłowów prostych "", a nie drukarskie "")
- Wartości liczbowe są wyrażone bez cudzysłowów i powinna być dziesiętne. Wartości szesnastkowej są prefiksem & h Na przykład 98052 & HFF
- Wartości logiczne, wyrażone są ze stałymi: PRAWDA, FAŁSZ.
- Wbudowane stałych i literałów wyrażone są tylko jego nazwą: IgnoreThisFlow wartość NULL, CRLF,

### <a name="functions"></a>Funkcje
Deklaracyjnych inicjowania obsługi administracyjnej używa wiele funkcji umożliwiających możliwość przekształcenia wartości atrybutu. Te funkcje mogą być zagnieżdżane, więc wynik z jednej funkcji jest przekazywana do innej funkcji.

`Function1(Function2(Function3()))`

Pełną listę funkcji można znaleźć w [Kompendium](active-directory-aadconnectsync-functions-reference.md).

### <a name="parameters"></a>Parametry
Parametr jest zdefiniowane przez łącznik lub przez administratora przy użyciu programu PowerShell. Parametry zazwyczaj zawierają wartości, które różnią się od systemu, na przykład nazwy domeny użytkownika znajduje się w. Można użyć tych parametrów w przepływach atrybut.

Łącznik usługi Active Directory przewidziano następujące parametry reguły przychodzące synchronizacji.

| Nazwa parametru | Komentarz |
| --- | --- |
| Domain.Netbios | Format NetBIOS domeny obecnie importowany, na przykład FABRIKAMSALES |
| Domain.FQDN | Format nazwy FQDN domeny obecnie importowany, na przykład sales.fabrikam.com |
| Domain.LDAP | Format LDAP domeny obecnie importowany, na przykład kontrolera domeny = sprzedaż, kontrolera domeny = fabrikam, kontrolera domeny = com |
| Forest.Netbios | Format NetBIOS nazwy las obecnie importowany, na przykład FABRIKAMCORP |
| Forest.FQDN | Format nazwy FQDN nazwy las obecnie importowany, na przykład fabrikam.com |
| Forest.LDAP | Format LDAP nazwy las obecnie importowany, na przykład kontrolera domeny = fabrikam, kontrolera domeny = com |

System udostępnia następujące parametr, który można uzyskać identyfikator łącznik uruchomiony:  
`Connector.ID`

Oto przykład, w którym domeny atrybutów metaverse nazwą netbios domeny, w której znajduje się użytkownik wypełnia:  
`domain` <- `%Domain.Netbios%`

### <a name="operators"></a>Operatory
Można używać następujących operatorów:

- **Porównanie**: <, < =, <>, >, > =
- **Matematycznych**: +, -, \*,
- **Ciąg**: & (ZŁĄCZ.teksty)
- **Logiczne**: & & (i) || (lub)
- **Kolejność obliczeń**:)

Operatory są wykonywane od lewej do prawej i mają ten sam priorytet oceny. Oznacza to, że \* (współczynnik) nie są sprawdzane przed - (odejmowanie). 2\*(5 + 3) nie jest taka sama jak 2\*5 + 3. (Nawiasy kwadratowe) służą do zmiany kolejności oceny, gdy od lewej do prawej oceny zamówienia nie jest odpowiedni.

## <a name="multi-valued-attributes"></a>Atrybuty wielowartościowe
Funkcje mogą działać zarówno jedno- i wielowartościowe atrybuty. Atrybuty wielowartościowe funkcja działa przez wszystkich wartości i dotyczy wszystkich wartości tę samą funkcję.

Na przykład:  
`Trim([proxyAddresses])`Wykonaj Trim każda wartość atrybutu proxyAddress.  
`Word([proxyAddresses],1,"@") & "@contoso.com"`Dla każdej wartości z @-sign, zamienić domeny z @contoso.com.  
`IIF(InStr([proxyAddresses],"SIP:")=1,NULL,[proxyAddresses])`Odszukaj adres SIP i usunąć go z wartości.

## <a name="next-steps"></a>Następne kroki

- Dowiedz się więcej o modelu konfiguracji w [Opis deklaracyjnych-inicjowania obsługi administracyjnej](active-directory-aadconnectsync-understanding-declarative-provisioning.md).
- Zobacz, jak deklaracyjnych inicjowania obsługi administracyjnej jest używane z gotowych do w [Opis konfiguracji domyślnej](active-directory-aadconnectsync-understanding-default-configuration.md).
- Zobacz, jak zrobić praktyczne przy użyciu deklaracyjnych inicjowania obsługi administracyjnej w [jak wprowadzanie zmian w domyślnej konfiguracji](active-directory-aadconnectsync-change-the-configuration.md).

**Przegląd tematów**

- [Azure AD Connect synchronizacją: opis i dostosowywanie synchronizacji](active-directory-aadconnectsync-whatis.md)
- [Integrowanie tożsamości lokalnych z usługi Azure Active Directory](active-directory-aadconnect.md)

**Tematy dodatkowe**

- [Azure AD Connect synchronizacją: funkcje, odwołania](active-directory-aadconnectsync-functions-reference.md)
