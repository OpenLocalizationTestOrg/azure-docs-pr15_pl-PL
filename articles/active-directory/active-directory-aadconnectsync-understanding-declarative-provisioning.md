<properties
    pageTitle="Azure AD Connect synchronizacją: opis deklaracyjnych-inicjowania obsługi administracyjnej | Microsoft Azure"
    description="W tym artykule wyjaśniono deklaracyjnych modelu konfiguracji obsługi administracyjnej w narzędzie Azure AD Connect."
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
    ms.date="08/29/2016"
    ms.author="billmath"/>


# <a name="azure-ad-connect-sync-understanding-declarative-provisioning"></a>Azure AD Connect synchronizacją: opis deklaracyjnych-inicjowania obsługi administracyjnej
W tym temacie wyjaśniono modelu konfiguracji w narzędzie Azure AD Connect. Model nosi nazwę deklaracyjnych inicjowania obsługi administracyjnej i umożliwia Konfiguracja z łatwością. Wiele czynności opisane w tym temacie są zaawansowane i nie są wymagane dla większości scenariuszy.

## <a name="overview"></a>Omówienie
Deklaracyjnych inicjowania obsługi administracyjnej przetwarzania obiektów odbierane z katalogu połączonego źródła i określa, jak obiekt i atrybuty transformacji ze źródła do miejsca docelowego. Obiekt jest przetwarzana w potoku synchronizacji i proces jest taki sam dla reguły przychodzące i wychodzące. Reguły ruchu przychodzącego ma z obszaru łącznik metaverse a reguły wychodzącej jest metaverse do obszaru łącznik.

![Potok synchronizacji](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/sync1.png)  

Proces zawiera kilka różnych modułów. Każdy z nich jest odpowiedzialny za jeden koncepcja w synchronizacji obiektu.

![Potok synchronizacji](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/pipeline.png)  

- Źródło, obiekt źródłowy
- [Zakres](#scope)umożliwia znalezienie wszystkich reguł synchronizacji, które znajdują się w zakresie
- [Dołączanie](#join), określa relację między obszar łączników i metaverse
- [Przekształcanie](#transform), jak można przekształcić atrybuty powoduje obliczenie i przepływ
- [Priorytet](#precedence), jest rozpoznawana jako konflikcie wpłaty atrybut
- TARGET, obiekt docelowy

## <a name="scope"></a>Zakres
Moduł zakres jest oceny obiektu i określa reguły, które znajdują się w zakresie i powinien zostać uwzględniony w przetwarzania. W zależności od wartości atrybuty obiektu synchronizacji różne reguły są obliczane w się w zakresie. Na przykład wyłączony użytkownik ze skrzynką pocztową nie Exchange ma inne reguły niż enabled użytkownika ze skrzynką pocztową.  
![Zakres](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/scope1.png)  

Zakres jest definiowana jako grupy i klauzul. Postanowienia znajdują się wewnątrz grupy. Logicznego i jest używany między wszystkie warunki w grupie. Na przykład (dział IT i kraju = = Dania). Logiczne lub jest używana między grupami.

![Zakres](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/scope2.png)  
Zakres na tym obrazie są odczytywane jako (dział = IT i kraju = Dania) lub (kraj = Szwecja). Jeśli albo grupy 1 lub 2 jest szacowana wartość PRAWDA, a następnie reguła jest w zakresie.

Moduł zakres obsługuje następujące operacje.

Operacja | Opis
--- | ---
RÓWNE, NOTEQUAL | Porównaj ciągu, którego wynikiem jest, jeśli wartość jest równa wartości atrybutu. Aby uzyskać wielowartościowe atrybuty Zobacz ISIN i ISNOTIN.
MNIEJSZE, LESSTHAN_OR_EQUAL | Porównaj ciąg, którego wynikiem jest, jeśli wartość jest mniej niż wartość atrybutu.
ZAWIERA, NOTCONTAINS | Porównaj ciąg, którego wynikiem jest, jeśli wartość znajdują się gdzieś wewnątrz wartość atrybutu.
STARTSWITH, NOTSTARTSWITH | Porównaj ciąg, którego wynikiem jest, jeśli wartość jest na początku wartość atrybutu.
ENDSWITH, NOTENDSWITH | Porównaj ciąg, którego wynikiem jest, jeśli wartość jest na końcu wartości atrybutu.
WIĘKSZE, GREATERTHAN_OR_EQUAL | Porównaj ciągu, którego wynikiem jest, jeśli wartość jest większa niż wartość atrybutu.
ISNULL, ISNOTNULL | Oblicza wartość, jeśli ten atrybut jest nieobecny z obiektu. Jeśli ten atrybut nie jest obecne i w związku z tym null, reguła jest w zakresie.
ISIN, ISNOTIN | Oblicza wartość, jeśli wartość jest zawarte w określonych atrybutów. Operacja jest wielowartościowych zmiany równości i NOTEQUAL. Atrybut ma być wielowartościowy atrybut, a jeśli wartość znajdują się w wartości atrybutu, następnie reguła jest w zakresie.
ISBITSET, ISNOTBITSET | Oblicza wartość, jeśli określony bit jest ustawiony. Na przykład może służyć do oceny bitów w kontroli konta użytkownika, aby sprawdzić, czy użytkownik jest włączona lub wyłączona.
ISMEMBEROF, ISNOTMEMBEROF | Wartości powinny zawierać nazwy domeny do grupy w obszarze łącznik. Jeśli obiekt jest członkiem grupy określonej, reguła jest w zakresie.

## <a name="join"></a>Dołączanie do
Moduł sprzężenia w potoku synchronizacji jest odpowiedzialny za znajdowanie relacji między obiekt w źródle i obiektu w docelowej. Na regułę ruchu przychodzącego tej relacji będzie obiektu w obszarze łącznik znajdowanie relacji do obiektu w metaverse.  
![Dołączanie do między KSW i mv](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/join1.png)  
Celem jest osiągnięcie, jeśli istnieje obiekt już w metaverse utworzone przez innego łącznika powinno zostać skojarzone z. Na przykład w las zasobów konta użytkownika z las konta powinny zostać połączone z danym użytkownikiem z las zasobów.

Sprzężenia są używane głównie na reguły przychodzące do łączenia obiektów miejsca łącznik do tego samego obiektu metaverse.

Sprzężenia są określane jako jedną lub więcej grup. W grupie masz klauzul. Logicznego i jest używany między wszystkie warunki w grupie. Logiczne lub jest używana między grupami. Grupy są przetwarzane w kolejności od góry do dołu. Po jednej grupy znalazł dokładnie jedną dopasowania obiektu w docelowej, żadne reguły sprzężenia są sprawdzane. Jeśli zero lub więcej niż jeden obiekt zostanie znaleziony, przetwarzanie dalszym następnej grupy reguł. Z tego powodu zasady powinny być utworzone w kolejności najbardziej jawnych najpierw lub bardziej rozmyty na końcu.  
![Dołączanie do definicji](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/join2.png)  
Sprzężenia na tym obrazie są przetwarzane od góry do dołu. Najpierw proces synchronizacji widzi, jeśli istnieje dopasowanie na pole Identyfikator pracownika. W przeciwnym razie drugą regułę widzi, jeśli nazwa konta umożliwia łączenie obiektów. Jeśli nie jest to dopasowanie albo, reguła trzecia i końcowe jest bardziej rozmyty dopasowanie przy użyciu nazwy użytkownika.

Jeśli zostały wszystkie reguły sprzężenia, a nie istnieje dokładnie jedno dopasowanie, **Typ łącza** na stronie **Opis** jest używany. Jeśli ta opcja jest ustawiona **udzielenia**, zostanie utworzony nowy obiekt w docelowej.  
![Obsługa administracyjna i dołączanie do](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/join3.png)  

Obiekt powinien mieć tylko jedna reguła synchronizacji jednego z regułami sprzężenia w zakresie. Jeśli istnieje wiele reguł synchronizacji miejsce, w którym jest definiowana sprzężenia, wystąpi błąd. Hierarchia ważności nie jest używane do rozwiązywania konfliktów sprzężenia. Obiekt musi mieć reguły sprzężenia w zakresie atrybuty przepływ z tym samym kierunku ruch przychodzący i wychodzący. Jeśli potrzebujesz atrybutów przepływu przychodzące i wychodzące do tego samego obiektu, musi mieć przychodzące i reguły ruchu wychodzącego synchronizacji z sprzężenia.

Dołącz ruchu wychodzącego ma szczególne zachowanie podczas próby obsługi administracyjnej obiektu do miejsca docelowego łącznik. Atrybut nazwy domeny jest używany do najpierw spróbuj odwrotna kolejność sprzężenia. Jeśli już istnieje obiekt w docelowej przestrzeni łącznik z tym samym nazwą domeny, obiekty są połączone.

Moduł sprzężenia tylko jest obliczane po gdy nowa reguła synchronizacji wejścia zakresu. Jeśli obiekt jest połączony, nie jest odłączanie nawet wtedy, gdy nie jest spełniony kryteria. Jeśli chcesz rozłączyć obiektu, reguła synchronizacji przyłączonym obiektów należy przejść do poza zakresem.

### <a name="metaverse-delete"></a>Usuń metaverse
Obiektu metaverse pozostanie niezmieniona dopóki zestawu jedna reguła synchronizacji w zakresie typu **Łącza** do **dostarczania** lub **StickyJoin**. StickyJoin jest używana podczas łącznik nie jest dozwolona do zapewniania obsługi nowy obiekt metaverse, ale jest dołączany, musi ono zostać usunięte w źródle przed usunięciem obiektu metaverse.

Po usunięciu obiektu metaverse wszystkich obiektów skojarzonych przy użyciu reguły wychodzącej synchronizacji oznaczone do **świadczenia** są oznaczane do usunięcia.

## <a name="transformations"></a>Przekształcenia
Przekształcenia służą do określania, jak atrybuty należy przepływ ze źródła do elementu docelowego. Strumienie może mieć jeden z następujących **typów przepływów**: bezpośrednich, stałą lub wyrażenie. Wartość atrybutu jako przepływał bezpośredni przepływ,-jest nie dodatkowych przekształceń. Stała wartość ustawia określonej wartości. Wyrażenie używa deklaracyjnych obsługi administracyjnej języka wyrażeń do wyrażania, jak powinny być transformacji. Szczegóły dotyczące języka wyrażeń można znaleźć w temacie [Opis deklaracyjnych obsługi administracyjnej wyrażenia języka](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) .

![Obsługa administracyjna i dołączanie do](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/transformations1.png)  

Pole wyboru **Zastosuj raz** Określa, czy atrybut należy można ustawić tylko podczas tworzenia obiektu. Na przykład tej konfiguracji można ustawić początkowe hasło dla nowego obiektu użytkownika.

### <a name="merging-attribute-values"></a>Scalanie wartości atrybutu
W przepływach atrybut istnieje ustawienie do określenia, jeśli atrybutów wielowartościowych powinny być scalane z kilku różnych łączników. Wartość domyślna to **aktualizacji**, co oznacza, że należy zdobywają reguła synchronizacji z najwyższy priorytet.

![Scalanie typów](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/mergetype.png)  

Istnieje także **korespondencji seryjnej** i **MergeCaseInsensitive**. Te opcje umożliwiają scalić wartości z różnych źródeł. Na przykład go może służyć do scalenia atrybut członka lub proxyAddresses z kilku różnych lasach. Tej opcji wszystkie reguły synchronizacji w zakresie obiektu należy użyć tego samego typu korespondencji seryjnej. Nie można definiować **aktualizacji** z jeden łącznik i **Scalanie** od drugiej. Jeśli spróbujesz, otrzymujesz komunikat o błędzie.

Różnica między **korespondencji seryjnej** i **MergeCaseInsensitive** jest sposobu przetwarzania atrybut zduplikowane wartości. Aparat synchronizacji służy do sprawdzenia, czy zduplikowane wartości nie są wstawiane do atrybutu docelowego. Z **MergeCaseInsensitive**zduplikowane wartości przy użyciu tylko różnica w przypadku, gdy nie mają występować. Na przykład powinien być niewidoczny oba "SMTP:bob@contoso.com" i "smtp:bob@contoso.com" atrybutu docelowego. **Scalanie** tylko przegląda dokładne wartości i wielu wartości w przypadku, gdy tylko różnią się w przypadku mogą występować.

Opcja **Zamień** jest taka sama jak **Aktualizacja**, ale nie jest używane.

### <a name="control-the-attribute-flow-process"></a>Sterowanie proces blokowy atrybut
Gdy wiele reguł Synchronizacja przychodząca zostały skonfigurowane do współtworzenia tę samą atrybutu metaverse, pierwszeństwa jest służące do ustalania na pierwszym miejscu. Reguła synchronizacji z najwyższy priorytet (najniższe wartości liczbowe) ma wartość do współtworzenia. W tym celu dla reguł ruchu wychodzącego. Synchronizacja reguły WINS najwyższy priorytet i współtworzyć wartość do katalogu połączonego.

W niektórych przypadkach zamiast przyczynić się do wartości, reguły synchronizacji należy określić zachowanie innych reguł. Istnieje kilka literałów specjalne używane dla tej sprawy.

Dla reguły przychodzące synchronizacji literałów **NULL** można oznaczającą, że przepływ nie ma wartości do współtworzenia. Inna reguła z niższy priorytet może przyczynić się wartości. Czy reguła nie przyczynić się wartości, atrybut metaverse jest usuwany. Dla reguły wychodzącej Jeśli **wartość NULL** jest końcowej po przetworzeniu wszystkie reguły synchronizacji następnie wartość zostanie usunięte w katalogu połączonego.

Literał **AuthoritativeNull** jest podobna do **wartości NULL** , ale z tą różnicą, że nie niższym reguły pierwszeństwa może przyczynić się wartości.

Przepływ atrybut można też użyć **IgnoreThisFlow**. Przypomina wartość null w znaczeniu, że wskazuje, że jest nic, aby pomóc. Różnica polega na jej w docelowej nie powoduje usunięcia już istniejącej wartości. Jest tak jak przepływ atrybut nigdy nie było dostępne.

Oto przykład:

Na *zewnątrz do AD - wdrożenia hybrydowe programu Exchange użytkownika* można znaleźć przepływu następujące czynności:  
`IIF([cloudSOAExchMailbox] = True,[cloudMSExchSafeSendersHash],IgnoreThisFlow)`  
Wyrażenia są odczytywane jako: Jeśli skrzynki pocztowej użytkownika znajduje się w Azure AD, przepływ atrybut z Azure AD do AD. W przeciwnym razie nie przepływ cokolwiek powrót do usługi Active Directory. W tym przypadku istniejącej wartości go czy Pamiętaj AD.

### <a name="importedvalue"></a>ImportedValue
Funkcja ImportedValue różni się od innych funkcji, ponieważ nazwa atrybutu musi być ujęta w cudzysłów, a nie w nawiasy kwadratowe:  
`ImportedValue("proxyAddresses")`.

Zazwyczaj podczas synchronizacji atrybut używa wartości oczekiwanej, nawet jeśli nie zostały wyeksportowane jeszcze lub błąd podczas eksportowania ("początek tower"). Ruch przychodzący synchronizacji założono, atrybut, który nie jeszcze osiągnięto połączonego katalogu po pewnym czasie osiągnie go. W niektórych przypadkach należy synchronizować tylko wartość potwierdzono przez połączonego katalog (hologram i różnicy importowanie tower").

Przykład tej funkcji można znaleźć w regule synchronizacji z gotowych do *w z usługi Active Directory — użytkownika typowych z programu Exchange*. W programie Exchange hybrydowe wartość dodana w wyniku Exchange online tylko mają być synchronizowane po potwierdzeniu, że wartość został pomyślnie wyeksportowany:  
`proxyAddresses` <- `RemoveDuplicates(Trim(ImportedValue("proxyAddresses")))`

## <a name="precedence"></a>Hierarchia ważności
Jeśli kilka reguł synchronizacji próbują Współtworzenie tę samą wartość atrybutu do elementu docelowego, wartość pierwszeństwa służy do określenia na pierwszym miejscu. Reguła z najwyższy priorytet, najniższa wartość liczbowa, ma Współtworzenie atrybutu konfliktu.

![Scalanie typów](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/precedence1.png)  

Ta kolejność może służyć do definiowania bardziej precyzyjne przepływów atrybut mały podzbiór obiektów. Na przykład w nowym oknie-z pole zasadami upewnij się, że pierwszeństwo z innych kont atrybuty z włączonym kontem (**AccountEnabled użytkownika**).

Pierwszeństwo można zdefiniować między łącznikami. Danymi lepiej się wartości, która umożliwia łączników.

### <a name="multiple-objects-from-the-same-connector-space"></a>Wiele obiektów z tego samego obszaru łącznika
Jeśli masz kilka obiektów w tym samym miejscu łącznik dołączony do tego samego obiektu metaverse pierwszeństwa musi zostać zmieniona. Jeśli kilka obiektów znajdują się w zakresie tej samej reguły synchronizacji, aparatu synchronizacji nie jest możliwe do określania pierwszeństwa. Jest niejednoznacznych obiekt źródłowy, które powinny przyczynić się wartość metaverse. Ta konfiguracja jest zgłoszone jako niejednoznacznych, nawet jeśli atrybuty w źródle mają tę samą wartość.  
![Wiele obiektów dołączonych do tego samego obiektu mv](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/multiple1.png)  

W tym scenariuszu należy zmienić zakres reguły synchronizacji, więc obiektów źródła synchronizacji różnych reguł w zakresie. Umożliwia określenie różnych pierwszeństwa.  
![Wiele obiektów dołączonych do tego samego obiektu mv](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/multiple2.png)  

## <a name="next-steps"></a>Następne kroki

- Dowiedz się więcej na temat języka wyrażeń w [Wyrażeniach opis deklaracyjnych inicjowania obsługi administracyjnej](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md).
- Zobacz, jak deklaracyjnych inicjowania obsługi administracyjnej jest używane z gotowych do w [Opis konfiguracji domyślnej](active-directory-aadconnectsync-understanding-default-configuration.md).
- Zobacz, jak zrobić praktyczne przy użyciu deklaracyjnych inicjowania obsługi administracyjnej w [jak wprowadzanie zmian w domyślnej konfiguracji](active-directory-aadconnectsync-change-the-configuration.md).
- Kontynuuj do czytania, jak użytkownicy i kontaktów wspólna praca w [Opis użytkowników i kontakty](active-directory-aadconnectsync-understanding-users-and-contacts.md).

**Przegląd tematów**

- [Azure AD Connect synchronizacją: opis i dostosowywanie synchronizacji](active-directory-aadconnectsync-whatis.md)
- [Integrowanie tożsamości lokalnych z usługi Azure Active Directory](active-directory-aadconnect.md)

**Tematy dodatkowe**

- [Azure AD Connect synchronizacją: funkcje, odwołania](active-directory-aadconnectsync-functions-reference.md)
