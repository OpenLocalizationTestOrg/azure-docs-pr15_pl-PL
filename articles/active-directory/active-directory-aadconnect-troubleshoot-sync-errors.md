<properties
    pageTitle="Narzędzie Azure AD Connect: Rozwiązywanie problemów z błędami podczas synchronizacji | Microsoft Azure"
    description="W tym miejscu wyjaśniono, jak rozwiązywać problemy napotykane podczas synchronizacji z Azure AD Connect."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>

# <a name="troubleshooting-errors-during-synchronization"></a>Rozwiązywanie problemów z błędami podczas synchronizacji
Mogą wystąpić błędy, gdy dane tożsamości są synchronizowane z systemu Windows Server usługi Active Directory (AD DS) usługi Azure Active Directory (Azure AD). Ten artykuł zawiera omówienie różnych rodzajów błędów synchronizacji, niektóre z możliwych scenariuszy, powodujące tych błędów i potencjalne sposoby błędów. Ten artykuł zawiera często używanych typów błędów i nie mogą obejmować możliwych błędów.

 W tym artykule założono, czytnik jest znajomość podstawowych [projektu Azure AD i Azure AD Connect](active-directory-aadconnect-design-concepts.md).

Z najnowszą wersją Azure AD Connect \(sierpnia 2016 lub wyższej\), raportu błędy synchronizacji jest dostępna w [Azure Portal](https://aka.ms/aadconnecthealth) jako część Azure AD łączenie kondycji do synchronizacji.


Uruchamianie 1 września 2016 Azure Active Directory duplikujące się atrybut funkcje [Ochrony](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md) będą dostępne dla wszystkich *nowych* Azure Active Directory dzierżaw domyślnie. Ta funkcja zostaną automatycznie włączone dla istniejących dzierżaw w nadchodzących miesiącach.

Narzędzie Azure AD Connect wykonuje 3 typy operacji z katalogów przechowuje zsynchronizowane: importowanie, synchronizacji i Eksportuj. Błędy może się odbywać w wszystkie operacje. W tym artykule omówiono głównie na błędy podczas eksportowania do Azure AD.

## <a name="errors-during-export-to-azure-ad"></a>Błędy podczas eksportowania do Azure AD
Po sekcji opisano różne typy błędy synchronizacji, które mogą wystąpić podczas operacji eksportowania do Azure AD przy użyciu łącznika Azure AD. Ten łącznik cechuje formatu nazw jest "contoso. *onmicrosoft.com*".
Błędy podczas eksportowania do Azure AD wskazują, że operację \(Dodawanie, aktualizowanie i usuwanie itd.\) próba przez narzędzie Azure AD Connect \(aparatu synchronizacji\) na usługi Azure Active Directory nie powiodło się.

![Eksportowanie omówienie błędów](.\media\active-directory-aadconnect-troubleshoot-sync-errors\Export_Errors_Overview_01.png)

## <a name="data-mismatch-errors"></a>Błędy niezgodności danych
### <a name="invalidsoftmatch"></a>InvalidSoftMatch
#### <a name="description"></a>Opis
- Gdy Azure AD Connect \(aparatu synchronizacji\) powoduje, że usługi Azure Active Directory, aby dodać lub zaktualizować obiektów, Azure AD odpowiada przychodzących obiekt w Azure AD przy użyciu atrybutu **sourceAnchor** z atrybutem **immutableId** obiektów. To dopasowanie jest określana mianem **Twardego zgodne**.
- Gdy Azure AD, **nie znajduje się** któremu atrybutów dowolnego obiektu, która jest zgodna z **immutableId** z atrybutem **sourceAnchor** przychodzących obiektu, przed inicjowania obsługi administracyjnej nowy obiekt, zmniejsza szybkość do znalezienia odpowiedniej za pomocą ProxyAddresses i UserPrincipalName atrybuty. Uwzględnij ten nosi nazwę **Wygładzone zgodne**. Uwzględnij wygładzone zaprojektowano zgodnie z obiektów, które już istnieje w Azure AD (które pochodzą w Azure AD) za pomocą nowych obiektów zostanie dodana/zaktualizowana podczas synchronizacji reprezentujące tej samej jednostki (użytkowników, grup) w środowisku lokalnym.
- **InvalidSoftMatch** błąd występuje, gdy słabo dopasowania nie znajduje pasujące obiekty **i** wygładzone zgodne znajduje pasujące obiektu, ale ten obiekt ma inną wartość *immutableId* niż przychodzących obiektu *SourceAnchor*, sugerujący, że pasujące obiekt został zsynchronizowany z innym obiektem z w wersji lokalnej usługi Active Directory.

Innymi słowy aby wygładzone pasuje do pracy, ma być wygładzone dopasowane do nie powinien mieć każda wartość argumentu *immutableId*. Jeśli dowolny obiekt *immutableId* z wartości jest awarii twardego dopasowania, ale spełniające kryteria dopasowania wygładzone, operacja spowoduje błąd synchronizacji InvalidSoftMatch.

Azure schematu usługi Active Directory nie zezwala na dwa lub więcej obiektów mieć taką samą wartość spośród następujących atrybutów. \(Nie jest pełna lista.\)

- ProxyAddresses
- UserPrincipalName
- onPremisesSecurityIdentifier
- Identyfikator obiektu

>[AZURE.NOTE] Funkcje [ochrony atrybut Duplikuj Azure AD atrybut](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md) jest również rozwijane jako domyślne zachowanie usługi Azure Active Directory.  Tworząc Azure AD bardziej elastyczne w sposób obsługi zduplikowanych ProxyAddresses i UserPrincipalName atrybuty w środowisku lokalnym AD zmniejszy liczbę błędów synchronizacji widoczne przez narzędzie Azure AD Connect (oraz innych klientach synchronizacji). Ta funkcja nie naprawia błędy duplikatów. Dlatego dane nadal należy naprawić. Ale pozwala inicjowania obsługi administracyjnej nowych obiektów, które w przeciwnym razie blokowane są obsługi administracyjnej ze względu na zduplikowane wartości w Azure AD. To będzie również zmniejszyć liczbę błędów synchronizacji zwracane do klienta synchronizacji.
Jeśli ta funkcja jest włączona dla dzierżawy, nie zobaczą błędy synchronizacji InvalidSoftMatch podczas inicjowania obsługi administracyjnej nowych obiektów.


#### <a name="example-scenarios-for-invalidsoftmatch"></a>Przykładowe scenariusze InvalidSoftMatch
1. W programie w wersji lokalnej usługi Active Directory występuje co najmniej dwa obiekty o tej samej wartości atrybutu ProxyAddresses. Tylko jeden jest wprowadzenie obsługi administracyjnej w Azure AD.
2. Dwa lub więcej obiektów o tej samej wartości userPrincipalName istnieje w w wersji lokalnej usługi Active Directory. Tylko jeden jest wprowadzenie obsługi administracyjnej w Azure AD.
3. Obiekt został dodany w wersji lokalnej usługi Active Directory z tej samej wartości atrybutu ProxyAddresses co istniejący obiekt w usłudze Azure Active Directory. Nie wprowadzenie zainicjowano obiekt dodane lokalnie w usłudze Azure Active Directory.
4. Obiekt został dodany w w wersji lokalnej usługi Active Directory z tej samej wartości atrybutu userPrincipalName co konto w usłudze Azure Active Directory. Obiekt nie jest wyświetlany przygotowana w usłudze Azure Active Directory.
5. Konto zsynchronizowanej został przeniesiony z las do las B. Azure AD Connect (synchronizacja aparat) została przy użyciu atrybutu ObjectGUID do obliczenia SourceAnchor. Po przeniesieniu las wartość SourceAnchor różni się. Nowy obiekt (od las B) nie może zsynchronizować z istniejącego obiektu w Azure AD.
6. Obiekt zsynchronizowanej masz przypadkowemu usunięciu z w wersji lokalnej usługi Active Directory i nowy obiekt został utworzony w usłudze Active Directory dla tej samej jednostki (na przykład użytkownika) bez usuwania konta w usłudze Azure Active Directory. Nowe konto nie można zsynchronizować z istniejącego obiektu Azure AD.
7. Narzędzie Azure AD Connect odinstalować i ponownie zainstalować. Podczas ponownej instalacji innym została wybrana jako SourceAnchor. Wszystkie obiekty, które już zostały zsynchronizowane zatrzymać synchronizację z powodu błędu InvalidSoftMatch.

#### <a name="example-case"></a>Przykład sprawy:
1. **Kit Robert** jest synchronizowana użytkownika usługi Azure Active Directory z w wersji lokalnej usługi Active Directory *contoso.com*
2. Roman Kit **UserPrincipalName** jest ustawiona jako **bobs@contoso.com**.
3. **"abcdefghijklmnopqrstuv =="** jest **SourceAnchor** wyliczane przez narzędzie Azure AD Connect przy użyciu Kit Robert **objectGUID** z na pomieszczenia usługi Active Directory, która jest **immutableId** Nowak Robert w usłudze Azure Active Directory.
4. Roman wprowadzono również następujące wartości atrybutu **proxyAddresses** :
    - smtp:bobs@contoso.com
    - smtp:bob.smith@contoso.com
    - **smtp:bob@contoso.com**
5.  Nowy użytkownik, **Taylora Roman**jest dodawany do w lokalnej usługi Active Directory.
6. Roman Taylora **UserPrincipalName** jest ustawiona jako **bobt@contoso.com**.
7. **"abcdefghijkl0123456789 ==" "** jest pomieszczenia **sourceAnchor** wyliczane przez narzędzie Azure AD Connect przy użyciu Taylora Robert **objectGUID** z w usłudze Active Directory. Robert Taylora obiekt nie ma jeszcze synchronizowane z usługi Azure Active Directory.
8. Roman Taylora występują następujące wartości atrybutu proxyAddresses
    - smtp:bobt@contoso.com
    - smtp:bob.taylor@contoso.com
    - **smtp:bob@contoso.com**
9. Podczas synchronizacji narzędzie Azure AD Connect rozpozna dodanie Taylora Robert w w wersji lokalnej usługi Active Directory i poproś Azure AD, aby wprowadzić te same zmiany.
10. Azure AD najpierw wykona słabo dopasowanie. Oznacza to, że będzie przeszukiwać, jeśli dowolny obiekt immutableId jest równa "abcdefghijkl0123456789 ==". Słabo dopasowania nie powiedzie się, jak żadnego innego obiektu w Azure AD będą mieć tego immutableId.
11. Azure AD spróbuje następnie dopasowania wygładzone Taylora Roman. Oznacza to, że będzie wyszukiwania, jeśli jest równa wartości trzech, łącznie z dowolnego obiektu z proxyAddressessmtp:bob@contoso.com
12. Azure AD spowoduje znalezienie Kit Robert obiektu zgodnie z wygładzone kryteriów. Ale ten obiekt ma wartość immutableId = "abcdefghijklmnopqrstuv ==". co oznacza ten obiekt został zsynchronizowany z innego obiektu z w wersji lokalnej usługi Active Directory. W związku z tym Azure AD nie wygładzone dopasowania tych obiektów i spowoduje błąd synchronizacji **InvalidSoftMatch** .

#### <a name="how-to-fix-invalidsoftmatch-error"></a>Jak naprawić błąd InvalidSoftMatch
Najczęstsze przyczyny błędów InvalidSoftMatch jest dwa obiekty z różnych SourceAnchor \(immutableId\) mają taką samą wartość ProxyAddresses i/lub UserPrincipalName atrybuty, które są używane podczas procesu wygładzone dopasowanie na Azure AD. Aby rozwiązać problem nieprawidłowe dopasowanie wygładzone

1.  Identyfikowanie zduplikowanych proxyAddresses, userPrincipalName lub inną wartość atrybut, który powoduje wystąpienie błędu. Również określenia, na które dwa \(lub więcej\) obiektów wiążą się z konfliktu. Raportu generowanych przez usługę [Azure AD łączenie kondycji dla synchronizacji](https://aka.ms/aadchsyncerrors) może ułatwić zidentyfikowanie dwa obiekty.
2. Identyfikowanie obiektu, który należy kontynuować zduplikowane wartości i obiektu, który nie powinien.
3. Usuwanie zduplikowanych wartości z obiektu, który nie jest konieczne tej wartości. Należy zauważyć, że należy wprowadzić zmiany w katalogu, w którym obiekt jest pochodzących z. W niektórych przypadkach może być konieczne usunięcie jednego z obiektów w konflikcie.
4. Jeśli wprowadzono zmiany w wersji lokalnej AD pozwolić Azure AD Connect synchronizować zmiany.

Zauważ, że raport o błędach synchronizacji w Azure AD łączenie kondycji do synchronizacji jest aktualizowany co 30 minut i zawiera błędy z najnowszych próba synchronizacji.

>[AZURE.NOTE] ImmutableId, zgodnie z definicją, nie należy zmieniać w polu Czas trwania obiektu. Jeśli narzędzie Azure AD Connect nie został skonfigurowany z scenariuszy pamiętać na powyższej liście, może się okazać w sytuacji, w której Azure AD Connect oblicza inną wartość SourceAnchor obiektu AD reprezentujący tak samo jednostki (samego użytkownika/grupy i kontaktu itd), która ma istniejący obiekt AD Azure, który chcesz nadal korzystać.

#### <a name="related-articles"></a>Artykuły pokrewne
- [Zduplikowane lub nieprawidłowe atrybutów uniemożliwia synchronizację katalogów w usłudze Office 365](https://support.microsoft.com/en-us/kb/2647098)

### <a name="objecttypemismatch"></a>ObjectTypeMismatch
#### <a name="description"></a>Opis
Gdy Azure AD próbuje Łagodna zgodne dwa obiekty, jest możliwe, że dwa obiekty inne "typ obiektu" (na przykład użytkownika, grupy, kontakt itp.) mieć takie same wartości atrybuty używane do wykonywania wygładzone dopasowanie. Jak kopiowanie atrybutów nie jest dozwolona w Azure AD, operacja może spowodować błąd synchronizacji "ObjectTypeMismatch".

#### <a name="example-scenarios-for-objecttypemismatch-error"></a>Przykładowe scenariusze ObjectTypeMismatch problemu
- Grupy zabezpieczeń poczty włączone zostanie utworzona w usłudze Office 365. Administrator powoduje dodanie nowego użytkownika lub kontakt w środowisku lokalnym AD (które nie są zsynchronizowane jeszcze Azure AD) o tej samej wartości atrybutu ProxyAddresses co grupy usługi Office 365.

#### <a name="example-case"></a>Przykład wielkość liter

1. Administrator tworzy nową grupę zabezpieczeń włączoną obsługę poczty w usłudze Office 365 dla działu podatku, a także adres e-mail jako tax@contoso.com. W ten sposób atrybutu ProxyAddresses dla tej grupy z wartością**smtp:tax@contoso.com**
2. Nowy użytkownik łączy Contoso.com i konto jest tworzone dla użytkownika lokalnego z proxyAddress jako**smtp:tax@contoso.com**
3. Gdy narzędzie Azure AD Connect zsynchronizuje nowe konto użytkownika, otrzymają komunikat o błędzie "ObjectTypeMismatch".

#### <a name="how-to-fix-objecttypemismatch-error"></a>Jak naprawić błąd ObjectTypeMismatch
Najczęstsze przyczyny błędów ObjectTypeMismatch jest dwa obiekty typu (użytkownika, grupy kontaktów itd.) mają taką samą wartość atrybutu ProxyAddresses. Aby rozwiązać problem ObjectTypeMismatch:

1.  Identyfikowanie zduplikowanych proxyAddresses (lub inne atrybuty) wartość, który powoduje wystąpienie błędu. Również określenia, na które dwa \(lub więcej\) obiektów wiążą się z konfliktu. Raportu generowanych przez usługę [Azure AD łączenie kondycji dla synchronizacji](https://aka.ms/aadchsyncerrors) może ułatwić zidentyfikowanie dwa obiekty.
2. Identyfikowanie obiektu, który należy kontynuować zduplikowane wartości i obiektu, który nie powinien.
3. Usuwanie zduplikowanych wartości z obiektu, który nie jest konieczne tej wartości. Należy zauważyć, że należy wprowadzić zmiany w katalogu, w którym obiekt jest pochodzących z. W niektórych przypadkach może być konieczne usunięcie jednego z obiektów w konflikcie.
4. Jeśli wprowadzono zmiany w wersji lokalnej AD pozwolić narzędzie Azure AD Connect synchronizować zmiany. Raport o błędach synchronizacji w Azure AD łączenie kondycji dla synchronizacji aktualizowany co 30 minut i zawiera błędy z najnowszych próba synchronizacji.


## <a name="duplicate-attributes"></a>Zduplikowane atrybuty
### <a name="attributevaluemustbeunique"></a>AttributeValueMustBeUnique
#### <a name="description"></a>Opis
Azure schematu usługi Active Directory nie zezwala na dwa lub więcej obiektów mieć taką samą wartość spośród następujących atrybutów. To, że każdy obiekt w Azure AD jest wymuszone dysponować unikatową wartość następujące atrybuty danego wystąpienia.

- ProxyAddresses
- UserPrincipalName

Jeśli narzędzie Azure AD Connect próby dodania nowego obiektu lub aktualizowanie istniejącego obiektu z wartością powyżej atrybutów, który jest już przypisany do innego obiektu usługi Azure Active Directory, operacja spowoduje błąd synchronizacji "AttributeValueMustBeUnique".
#### <a name="possible-scenarios"></a>Możliwe scenariusze:
1. Zduplikowane wartości przydzielono już synchronizowana obiektu, który koliduje z innym obiektem zsynchronizowane.

#### <a name="example-case"></a>Przykład sprawy:
1. **Kit Robert** jest synchronizowana użytkownika usługi Azure Active Directory z w wersji lokalnej usługi Active Directory contoso.com
2. Roman Kit **UserPrincipalName** w środowisku lokalnym jest ustawiona jako **bobs@contoso.com**.
3. Roman wprowadzono również następujące wartości atrybutu **proxyAddresses** :
    - smtp:bobs@contoso.com
    - smtp:bob.smith@contoso.com
    - **smtp:bob@contoso.com**
4. Nowy użytkownik, **Taylora Roman**jest dodawany do w lokalnej usługi Active Directory.
5. Roman Taylora **UserPrincipalName** jest ustawiona jako **bobt@contoso.com**.
6. **Roman Taylora** występują następujące wartości atrybutu **ProxyAddresses** i. smtp:bobt@contoso.comII. smtp:bob.taylor@contoso.com
7. Roman Taylora obiekt jest synchronizowane z usługą Azure Active Directory pomyślnie.
8. Administrator określono, aby zaktualizować atrybutu **ProxyAddresses** Taylora Robert z następującą wartość: i. **smtp:bob@contoso.com**
9. Azure AD spróbuje zaktualizować Taylora Robert obiektu w Azure AD na wartość powyżej, ale aby operacja zakończy się niepowodzeniem jako że ProxyAddresses wartość jest już przypisany do Kowalski Roman, uzyskując błąd "AttributeValueMustBeUnique".

#### <a name="how-to-fix-attributevaluemustbeunique-error"></a>Jak naprawić błąd AttributeValueMustBeUnique
Najczęstsze przyczyny błędów AttributeValueMustBeUnique jest dwa obiekty z różnych SourceAnchor \(immutableId\) mają tę samą wartość atrybutów ProxyAddresses i/lub UserPrincipalName. Aby naprawić błąd AttributeValueMustBeUnique

1.  Identyfikowanie zduplikowanych proxyAddresses, userPrincipalName lub inną wartość atrybut, który powoduje wystąpienie błędu. Również określenia, na które dwa \(lub więcej\) obiektów wiążą się z konfliktu. Raportu generowanych przez usługę [Azure AD łączenie zdrowia synchronizacji](https://aka.ms/aadchsyncerrors) może ułatwić zidentyfikowanie dwa obiekty.
2. Identyfikowanie obiektu, który należy kontynuować zduplikowane wartości i obiektu, który nie powinien.
3. Usuwanie zduplikowanych wartości z obiektu, który nie jest konieczne tej wartości. Należy zauważyć, że należy wprowadzić zmiany w katalogu, w którym obiekt jest pochodzących z. W niektórych przypadkach może być konieczne usunięcie jednego z obiektów w konflikcie.
4. Jeśli wprowadzono zmiany w wersji lokalnej AD pozwolić Azure AD Connect synchronizowanie zmian dotyczących błąd, aby przejść o stałej.

#### <a name="related-articles"></a>Artykuły pokrewne
-[Zduplikowane lub nieprawidłowe atrybutów uniemożliwia synchronizację katalogów w usłudze Office 365](https://support.microsoft.com/en-us/kb/2647098)


## <a name="data-validation-failures"></a>Błędy sprawdzania poprawności danych
### <a name="identitydatavalidationfailed"></a>IdentityDataValidationFailed
#### <a name="description"></a>Opis
Azure Active Directory wymusza różne ograniczenia danych przed zezwoleniem tych danych, do którego zostaną zapisane w katalogu. To, aby upewnić się, że użytkownicy końcowi uzyskać najlepsze środowiska podczas korzystania z aplikacji, które są zależne od tych danych.
#### <a name="scenarios"></a>Scenariusze
. Wartość atrybutu UserPrincipalName jest nieprawidłowa/nieobsługiwana znaków.
b. Atrybut UserPrincipalName nie wykonaj wymagany format.
#### <a name="how-to-fix-identitydatavalidationfailed-error"></a>Jak naprawić błąd IdentityDataValidationFailed

. Upewnij się, że atrybut userPrincipalName obsługiwał znaków i wymagany format.

#### <a name="related-articles"></a>Artykuły pokrewne
- [Przygotowywanie do zapewniania obsługi użytkowników w ramach synchronizacji katalogów do usługi Office 365]( https://support.office.com/en-us/article/Prepare-to-provision-users-through-directory-synchronization-to-Office-365-01920974-9e6f-4331-a370-13aea4e82b3e)


### <a name="datavalidationfailed"></a>DataValidationFailed
#### <a name="description"></a>Opis
To jest bardzo precyzyjne wielkość liter, które powoduje błąd synchronizacji **"DataValidationFailed"** po zmianie sufiks UserPrincipalName użytkownika z jednej domeny federacyjnych do innej domeny federacyjnych.

#### <a name="scenarios"></a>Scenariusze
Zsynchronizowane użytkownika sufiks UserPrincipalName została zmieniona z domenami federacyjnych do innej domeny federacyjnych w swojej siedzibie. Na przykład *UserPrincipalName = bob@contoso.com * została zmieniona na *UserPrincipalName = bob@fabrikam.com *.

#### <a name="example"></a>Przykład
1. Roman Kowalski, konto Contoso.com, aby dodać jako nowego użytkownika w usłudze Active Directory z UserPrincipalNamebob@contoso.com
2. Roman jest przenoszony do dzielenia różnych contoso.com o nazwie Fabrikam.com i jego UserPrincipalName został zamieniony nabob@fabrikam.com
3. Domeny domen contoso.com i fabrikam.com są domenach federacyjnych z usługą Azure Active Directory.
4. Roberta userPrincipalName nie zostanie zaktualizowany i powoduje błąd synchronizacji "DataValidationFailed".

#### <a name="how-to-fix"></a>Jak rozwiązać problem
Jeśli sufiks UserPrincipalName użytkownika został zaktualizowany z bob@ **contoso.com** do bob@ **fabrikam.com**, gdy zarówno **contoso.com** , jak i **fabrikam.com** są **domenach federacyjnych**, postępuj poniżej czynności, aby naprawić błąd synchronizacji

1. Aktualizowanie UserPrincipalName użytkownika w Azure AD z bob@contoso.com do bob@contoso.onmicrosoft.com. Za pomocą następującego polecenia programu PowerShell z modułem Azure AD programu PowerShell:`Set-MsolUserPrincipalName -UserPrincipalName bob@contoso.com -NewUserPrincipalName bob@contoso.onmicrosoft.com`

2. Zezwalaj na następnym cyklem synchronizacji próba synchronizacji. Synchronizacja czasu zakończy się pomyślnie i zostanie zaktualizowany UserPrincipalName Robert do bob@fabrikam.com zgodnie z oczekiwaniami.


## <a name="largeobject"></a>LargeObject
### <a name="description"></a>Opis
Gdy atrybut przekracza dozwolony limit rozmiaru, limit długości lub liczba określonym przez schematu usługi Azure Active Directory, Operacja synchronizacji spowoduje błąd synchronizacji **LargeObject** lub **ExceededAllowedLength** . Zazwyczaj ten błąd występuje dla następujących atrybutów

- certyfikatu użytkownika
- thumbnailPhoto
- proxyAddresses

### <a name="possible-scenarios"></a>Możliwych scenariuszy
1. Atrybut certyfikatu użytkownika Roberta jest przechowywana zbyt wiele certyfikatów przypisane do niego. Mogą one obejmować starsze, wygasłe certyfikaty.
2. ThmubnailPhoto Roberta ustawiona w usłudze Active Directory jest zbyt duża, aby można zsynchronizować w Azure AD.
3. Podczas automatycznego populacji atrybutu ProxyAddresses w usłudze Active Directory, masz przydzielone obiekt > 500 ProxyAddresses.

### <a name="how-to-fix"></a>Jak rozwiązać problem

1. Upewnij się, czy atrybut powoduje błąd w ramach dozwolonych ograniczenia.

## <a name="related-links"></a>Łącza pokrewne
- [Znajdź obiektów usługi Active Directory w Centrum administracyjne usługi Active Directory] (https://technet.microsoft.com/library/dd560661.aspx)
- [Jak wyszukiwać usługi Azure Active Directory obiektu przy użyciu programu PowerShell Azure Active Directory](https://msdn.microsoft.com/library/azure/jj151815.aspx)
