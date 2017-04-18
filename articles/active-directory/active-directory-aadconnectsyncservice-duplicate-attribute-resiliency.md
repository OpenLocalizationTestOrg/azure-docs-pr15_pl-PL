<properties
    pageTitle="Elastyczność atrybut synchronizacji i duplikat tożsamość | Microsoft Azure"
    description="Nowe zachowanie sposobu obsługi obiekty z głównej nazwy użytkownika lub ProxyAddress konfliktów podczas synchronizacji katalogów przy użyciu Azure AD Connect."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="markusvi"/>



# <a name="identity-synchronization-and-duplicate-attribute-resiliency"></a>Elastyczność atrybut synchronizacji i Duplikuj tożsamości
Zduplikowane elastyczność atrybut to funkcja usługi Azure Active Directory, która wyeliminuje tarcie spowodowane przez **UserPrincipalName** i **ProxyAddress** konfliktów podczas uruchamiania jednego z narzędzi synchronizacji firmy Microsoft.

Te atrybuty są zazwyczaj wymagane do być unikatowa we wszystkich **użytkownika**, **grupy**lub **kontaktu** obiektów w danej dzierżawy usługi Azure Active Directory.

> [AZURE.NOTE] Tylko użytkownicy mogą mieć UPN.

Nowe zachowanie, które ta funkcja umożliwia w chmurze części proces synchronizacji, dlatego jest klienta o niesprecyzowanym i odpowiednie dla każdego produktu synchronizacji firmy Microsoft, w tym narzędzie Azure AD Connect, DirSync i z wieloma Uczestnikami + łącznik. Uniwersalny termin "klienta synchronizacji" jest używany w tym dokumencie oznaczającą dowolny z tych produktów.

## <a name="current-behavior"></a>Zachowanie bieżącej
W przypadku próby obsługi administracyjnej nowy obiekt z wartością UPN lub ProxyAddress naruszający to ograniczenie unikatowości usługi Azure Active Directory blokuje dany obiekt utworzenie. Podobnie jeśli obiekt jest aktualizowany przy użyciu głównej nazwy użytkownika nie są unikatowe lub ProxyAddress, aktualizacja zakończy się niepowodzeniem. Próba zastrzeżenia lub aktualizacja jest po ponownych próbach przez klienta synchronizacji na każdym cyklu eksportu i znów się nie powiedzie, dopóki nie zostanie rozwiązany. Wiadomość e-mail raport o błędzie jest generowany przy każdej próbie i komunikat o błędzie jest rejestrowany przez klienta synchronizacji.

## <a name="behavior-with-duplicate-attribute-resiliency"></a>Zachowanie atrybut zduplikowany elastyczności
Zamiast całkowicie braku obsługi administracyjnej lub zaktualizować obiekt z atrybutem zduplikowanych, usługi Azure Active Directory "poddaje kwarantannie" zduplikowane atrybut, który chcesz naruszają ograniczenia unikatowości. Jeśli ten atrybut jest wymagany do obsługi administracyjnej, takich jak UserPrincipalName, usługa przypisuje wartość symbol zastępczy. Format te wartości tymczasowe jest  
"***<OriginalPrefix>+<4DigitNumber>@<InitialTenantDomain>. onmicrosoft.com***".  
Atrybut nie jest wymagane, takich jak **ProxyAddress**, usługi Azure Active Directory po prostu poddaje kwarantannie atrybutu konflikt i będzie kontynuować tworzenie obiektów lub aktualizacji.

Po quarantining atrybut, informacje dotyczące konfliktu są wysyłane w tym samym błędu raportu wiadomości e-mail używanych w stare zachowanie. Te informacje o jest wyświetlana tylko w raporcie o błędach jeden raz, sytuacji kwarantanny nie nadal zalogowania się w przyszłych wiadomościach e-mail. Ponadto ponieważ pomyślnym Eksportuj do tego obiektu klienta synchronizacji nie rejestruje błąd i nie ponów próbę Tworzenie / aktualizowanie operacji na cykli kolejnych synchronizacji.

Aby obsługują tę funkcję nowy atrybut został dodany do użytkowników i grup oraz kontakt klas obiektów:  
**DirSyncProvisioningErrors**

To jest wielowartościowe atrybut, który służy do przechowywania konflikcie atrybuty, które chcesz naruszają ograniczenia unikatowości powinny one zostać dodane normalnie. Zadania czasomierza tła został włączony w usłudze Active Directory platformy Azure uruchamianej co godzinę, aby wyszukać konflikty atrybut zduplikowany, które zostały rozwiązane, a wątpliwe atrybuty są automatycznie usuwane z kwarantanny.

### <a name="enabling-duplicate-attribute-resiliency"></a>Włączanie ochrony atrybut zduplikowany
Zduplikowane elastyczność atrybut będzie nowe zachowanie przez wszystkich dzierżaw usługi Azure Active Directory. Będzie widoczny na domyślnie dla wszystkich dzierżawami, które włączono synchronizację po raz pierwszy w 22 sierpnia 2016 lub nowszym. Dzierżaw, które włączona synchronizacja przed tej daty ma włączoną partiami funkcją. Ten rozmieszczenia rozpocznie się w 2016 wrzesień i wiadomości e-mail z powiadomieniem będą wysyłane do każdej dzierżawcy techniczne powiadomienie o kontakt z określonej daty, gdy zostanie włączona funkcja.

Po włączono duplikowanie elastyczność atrybut nie można wyłączyć.

Aby sprawdzić, jeśli funkcja jest włączona dla dzierżawy, możesz to zrobić, pobierając najnowszą wersję modułu Azure Active Directory programu PowerShell i uruchamiania:

`Get-MsolDirSyncFeatures -Feature DuplicateUPNResiliency`

`Get-MsolDirSyncFeatures -Feature DuplicateProxyAddressResiliency`

Jeśli chcesz z wyprzedzeniem włączyć funkcję przed jest włączona dla dzierżawy, możesz to zrobić, pobierając najnowszą wersję modułu Azure Active Directory programu PowerShell i uruchamiania:

`Set-MsolDirSyncFeature -Feature DuplicateUPNResiliency -Enable $true`

`Set-MsolDirSyncFeature -Feature DuplicateProxyAddressResiliency -Enable $true`

## <a name="identifying-objects-with-dirsyncprovisioningerrors"></a>Identyfikowanie obiektów z DirSyncProvisioningErrors
Istnieje obecnie dwie metody do identyfikowania obiektów, które mają tych błędów z powodu zduplikowaną właściwość konflikty, Azure Active Directory programu PowerShell i portalu administracyjnego usługi Office 365. Ma planów rozszerzenie dodatkowe portalu podstawie raportowania w przyszłości.

### <a name="azure-active-directory-powershell"></a>PowerShell Azure Active Directory
Dla poleceń cmdlet środowiska PowerShell w tym temacie są spełnione następujące warunki:

- Wszystkie następujące polecenia cmdlet jest uwzględniana wielkość liter.
- **— ErrorCategory PropertyConflict** zawsze musi być włączona. Obecnie są inne typy **ErrorCategory**, ale to może zostać wydłużony w przyszłości.

Najpierw wprowadzenie uruchomiony **MsolService Połącz** i wpisując poświadczeń administrator dzierżawy.

Następujące polecenia cmdlet i operatorów za pomocą następnie, aby wyświetlić błędy na różne sposoby:

1. [Zobacz wszystkie](#see-all)

2. [Według typu właściwości](#by-property-type)

3. [Według wartości powodujące konflikt](#by-conflicting-value)

4. [Używanie funkcji wyszukiwania ciągu](#using-a-string-search)

5. [Sortowania](#sorted)

6. [W ograniczoną ilość lub wszystkich](#in-a-limited-quantity-or-all)


#### <a name="see-all"></a>Zobacz wszystkie
Po połączeniu, aby wyświetlić listę Ogólne atrybut inicjowania obsługi administracyjnej błędy w dzierżawie Uruchom:

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict`

To powstaje podobnej do następującej:  
 ![Get-MsolDirSyncProvisioningError](./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/1.png "Get-MsolDirSyncProvisioningError")  


#### <a name="by-property-type"></a>Według typu właściwości
Aby wyświetlić błędy typu właściwości, dodać flagę **- PropertyName** argumentu **UserPrincipalName** lub **ProxyAddresses** :

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyName UserPrincipalName`

Lub

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyName ProxyAddresses`

#### <a name="by-conflicting-value"></a>Według wartości powodujące konflikt
Aby wyświetlić błędy związane z określoną właściwość dodać flagę **- Wartość_właściwości** (**- PropertyName** należy używać także podczas dodawania ta flaga):

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyValue User@domain.com -PropertyName UserPrincipalName`


#### <a name="using-a-string-search"></a>Używanie funkcji wyszukiwania ciągu
Do wykonania wyszukiwania ciągu ogólne użyć flagi **Parametru-Wyszukiwany_ciąg** . To mogą być używane niezależnie ze wszystkich powyższych flagi, z wyjątkiem **ErrorCategory PropertyConflict**, która jest zawsze wymagane:

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -SearchString User`

#### <a name="in-a-limited-quantity-or-all"></a>W ograniczoną ilość lub wszystkich
1. **MaxResults <Int> ** może służyć do ograniczania kwerend do określonej liczby wartości.

2. **Wszystkie** można mieć pewność, że wszystkie wyniki są pobierane w przypadku gdy istnieje dużej liczby błędów.

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -MaxResults 5`

## <a name="office-365-admin-portal"></a>Portalu administracyjnego usługi Office 365

W Centrum administracyjnym usługi Office 365, możesz wyświetlić błędy synchronizacji katalogów. Raport w portalu usługi Office 365 będzie wyświetlana tylko obiektów **użytkowników** , których tych błędów. Nie są wyświetlane informacje o konfliktach między **grupami** i **Kontakty**.


![Aktywni użytkownicy] (./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/1234.png "Aktywni użytkownicy")

Aby uzyskać instrukcje dotyczące sposobu wyświetlania błędów synchronizacji katalogów w Centrum administracyjnym usługi Office 365 zobacz [Identyfikowanie błędy synchronizacji katalogów w usłudze Office 365](https://support.office.com/en-us/article/Identify-directory-synchronization-errors-in-Office-365-b4fc07a5-97ea-4ca6-9692-108acab74067).


### <a name="identity-synchronization-error-report"></a>Raport o błędach synchronizacji tożsamości
Gdy obiekt z konfliktem atrybut zduplikowany jest obsługiwany z to nowe zachowanie powiadomienie znajduje się w standardowej raport o błędach synchronizacji tożsamości wiadomość e-mail jest wysyłana do technicznych powiadomienie kontaktów dla dzierżawy. Istnieje jednak ważne zmiany tego zachowania. W przeszłości informacji na temat konflikt atrybut zduplikowany czy uwzględniane w raporcie wszystkich kolejnych błędu, aż konflikt został rozwiązany. Z to nowe zachowanie powiadomienia o błędach dla danego konfliktu wyświetlona dopiero raz — w czasie konflikcie atrybut jest kwarantannie.

Poniżej przedstawiono przykładowy wygląd powiadomień e-mail dla konfliktu ProxyAddress:  
    ![Aktywni użytkownicy](./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/6.png "Active Users")  

## <a name="resolving-conflicts"></a>Rozwiązywanie konfliktów
Rozwiązywanie problemów z taktyk strategii i rozdzielczość dla tych błędów powinny różnić się od sposób błędy atrybut zduplikowany były obsługiwane w przeszłości. Jedyna różnica polega na tym, że zadania czasomierza wymazywanie za pośrednictwem dzierżawy na stronie usługi, aby automatycznie dodawać atrybutu w danym do obiektu pisane z wielkiej litery, po konflikt został rozwiązany.

Następujący artykuł zawiera opis różnych strategii Rozwiązywanie problemów i rozpoznawanie: [duplikat lub błąd atrybutów uniemożliwia synchronizację katalogów w usłudze Office 365](https://support.microsoft.com/kb/2647098).

## <a name="known-issues"></a>Znane problemy
Żadna z tych znane problemy dotyczące powoduje utratę lub usługi rozkładu danych. Niektóre z nich są estetycznych, innym powodują standardowej "*ochrony przed*" Atrybut zduplikowany zostanie wygenerowany zamiast quarantining atrybutu konflikt i innego powoduje, że w przypadku niektórych błędów wymagać dodatkowego ręcznego konfigurowania.

**Zachowanie podstawowe:**

1. Obiekty z konfiguracji konkretnego atrybutu nadal błędów eksportu zamiast zduplikowanych atrybutów, które są kwarantannie.  
Na przykład:

    . Nowy użytkownik zostanie utworzona w AD za pomocą nazwy UPN **Joe@contoso.com** i ProxyAddress**smtp:Joe@contoso.com**

    b. Właściwości obiektu w konflikcie z istniejącą grupą, gdzie jest ProxyAddress **SMTP:Joe@contoso.com**.

    c. Podczas eksportu, jest generowany błąd **konfliktu ProxyAddress** zamiast atrybuty konflikt kwarantannie. Operacja jest wykonana ponownie po cyklu kolejnych synchronizacji, jak mogą być przed włączeniem funkcje ochrony.

2. Jeśli z tym samym adresem SMTP lokalnej są tworzone dwie grupy, jedną zakończy się niepowodzeniem udzielenia przy pierwszej próbie z błędem standardowy **ProxyAddress** zduplikowane. Jednak zduplikowanych wartości jest prawidłowo kwarantannie po następnym cyklem synchronizacji.

**Raport portalu office**:

1. Komunikat o błędzie szczegółowe dla dwóch obiektów w zestawie konflikt UPN jest taka sama. Oznacza to, że oba miały ich UPN zmienione / kwarantannie, gdy w rzeczywistości tylko jedno z nich ma jakiekolwiek dane zmienione.

2. Szczegółowy komunikat o błędzie konfliktu UPN zawiera nieprawidłowe displayName dla użytkownika, który ma ich UPN zmienione kwarantannie. Na przykład:

    . **Użytkownik A** synchronizuje pierwszy z **UPN = User@contoso.com **.

    b. **Użytkownik B** jest próba można zsynchronizować w górę do następnego poziomu **UPN = User@contoso.com **.

    c. **Przez użytkownika B** UPN został zamieniony na **User1234@contoso.onmicrosoft.com** i **User@contoso.com** jest dodawana do **DirSyncProvisioningErrors**.

    d. Komunikat o błędzie do **Użytkownika B** powinny wskazywać, że **użytkownika A** ma już **User@contoso.com** jako głównej nazwy użytkownika, ale zawiera displayName własny **B użytkownika** .



**Raport o błędach synchronizacji tożsamości**:

Łącze do *instrukcji dotyczących rozwiązania tego problemu* jest niepoprawna:  
    ![Aktywni użytkownicy](./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/6.png "Active Users")  

Powinny wskazywać [https://aka.ms/duplicateattributeresiliency](https://aka.ms/duplicateattributeresiliency).


## <a name="see-also"></a>Zobacz też

- [Synchronizowanie Azure AD Connect](active-directory-aadconnectsync-whatis.md)

- [Integrowanie tożsamości lokalnych z usługi Azure Active Directory](active-directory-aadconnect.md)

- [Identyfikowanie błędy synchronizacji katalogów w usłudze Office 365](https://support.office.com/en-us/article/Identify-directory-synchronization-errors-in-Office-365-b4fc07a5-97ea-4ca6-9692-108acab74067)
