<properties
   pageTitle="Łącznik programu PowerShell | Microsoft Azure"
   description="W tym artykule opisano, jak skonfigurować firmy Microsoft Windows PowerShell łącznik."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="08/30/2016"
   ms.author="billmath"/>

# <a name="windows-powershell-connector-technical-reference"></a>Informacje techniczne dotyczące systemu Windows PowerShell łącznika
W tym artykule opisano łącznik programu PowerShell systemu Windows. Artykuł dotyczy następujących produktów:

- Menedżer tożsamości 2016 (MIM2016)
- Produkt Forefront Identity Manager 2010 R2 (FIM2010R2)
    -   Należy użyć poprawki 4.1.3671.0 lub nowszy [KB3092178](https://support.microsoft.com/kb/3092178).

MIM2016 i FIM2010R2 łącznik jest dostępna do pobrania z [Centrum pobierania Microsoft](http://go.microsoft.com/fwlink/?LinkId=717495).

## <a name="overview-of-the-powershell-connector"></a>Omówienie łącznik programu PowerShell
Łącznik programu PowerShell umożliwia Integracja usługi synchronizacji z systemów zewnętrznych, oferujących podstawą interfejsy API programu Windows PowerShell. Łącznik zawiera mostka między możliwości agenta zarządzania extensible łączności przy użyciu połączenia 2 framework (ECMA2) i programu Windows PowerShell. Aby uzyskać więcej informacji o strukturze ECMA zobacz [Extensible łączności 2.2 zarządzania agenta odwołania](https://msdn.microsoft.com/library/windows/desktop/hh859557.aspx).

### <a name="prerequisites"></a>Wymagania wstępne
Przed użyciem łącznika upewnij się, że masz na serwerze synchronizacji z następujących czynności:

- Program Microsoft .NET 4.5.2 Framework lub nowszy
- Środowiska Windows PowerShell w wersji 2.0, 3.0 lub 4.0

Zasady wykonanie na serwerze usług synchronizacji musi skonfigurowanym do obsługi łącznik, aby uruchomić skrypty środowiska Windows PowerShell. O ile skryptów zostanie uruchomiona łącznika są podpisane cyfrowo, konfigurowanie zasad wykonywania wykonanie tego polecenia:  
`Set-ExecutionPolicy -ExecutionPolicy RemoteSigned`

## <a name="create-a-new-connector"></a>Tworzenie nowego łącznika
Aby utworzyć łącznik programu Windows PowerShell w usługi synchronizacji, należy podać serię skrypty środowiska Windows PowerShell, które wykonywane czynności wymagane przez usługi synchronizacji. W zależności od źródła danych, które możesz połączyć się i funkcji, które są wymagane są różne skrypty, które należy zaimplementować. W tej sekcji opisano wszystkich skryptów, który można zaimplementować i gdy są one wymagane.

Łącznik programu Windows PowerShell służy do przechowywania wszystkich skryptów w bazie danych usługi synchronizacji. Gdy jest możliwe w celu uruchamiania skryptów, które są przechowywane w systemie plików, łatwiej Wstaw treść każdego skryptu bezpośrednio w konfiguracji łącznika.

Aby utworzyć łącznik programu PowerShell, w **Usługi synchronizacji** wybierz **Agent zarządzania** i **Tworzenie**. Zaznacz łącznik **programu PowerShell (Microsoft)** .

![Tworzenie łącznika](./media/active-directory-aadconnectsync-connector-powershell/createconnector.png)

### <a name="connectivity"></a>Łączność
Wprowadź parametry konfiguracji nawiązywania połączenia z systemem zdalnym. Te wartości są bezpieczne przechowywane przez usługę synchronizacji i udostępnione skrypty środowiska Windows PowerShell po uruchomieniu łącznik.

![Łączność](./media/active-directory-aadconnectsync-connector-powershell/connectivity.png)

Możesz skonfigurować następujące parametry połączenia:

**Łączność**

Parametr | Wartość domyślna | Cel
--- | --- | ---
Serwer | <Blank> | Nazwa serwera, który łącznik należy nawiązać połączenie.
Domeny | <Blank> | Domena poświadczeń do przechowywania do użytku po uruchomieniu łącznik.
Użytkownika | <Blank> | Nazwa użytkownika poświadczeń do przechowywania do użytku po uruchomieniu łącznik.
Hasło | <Blank> | Hasło poświadczeń do przechowywania do użytku po uruchomieniu łącznik.
Personifikacji konta łącznika | FAŁSZ | W przypadku wartości true Usługa synchronizacji działa skrypty środowiska Windows PowerShell w kontekście dostarczone poświadczenia. Jeśli to możliwe, zaleca się że parametr **$Credentials** jest przekazywany do każdej skrypt jest używany zamiast personifikacji. Aby uzyskać więcej informacji o dodatkowe uprawnienia, które są wymagane do Użyj tej opcji zobacz [Dodatkowa konfiguracja personifikacji](#additional-configuration-for-impersonation).
Ładowanie profilu użytkownika, gdy personifikacji | FAŁSZ | Instruuje system Windows, aby załadować profilu użytkownika poświadczeń łącznika podczas personifikacji. Jeśli personifikowanego użytkownika ma profil mobilny, łącznik nie ładuje profilu mobilnego. Aby uzyskać więcej informacji na temat dodatkowych uprawnień wymaganych do wykorzystania ten parametr, zobacz [Dodatkowa konfiguracja personifikacji](#additional-configuration-for-impersonation).
Typ logowania podczas personifikacji | Brak | Typ logowania podczas personifikacji. Więcej informacji można znaleźć w [dwLogonType] [ dw] dokumentacji.
Tylko skrypty podpisane | FAŁSZ | Jeśli wartość true, łącznik programu Windows PowerShell sprawdza, czy każdy skrypt ma prawidłowy podpis cyfrowy. Jeśli ma wartość FAŁSZ, upewnij się, że serwer usługi synchronizacji programu Windows PowerShell wykonanie zasad jest RemoteSigned lub bez ograniczeń.

**Typowe modułu**  
Łącznik umożliwia przechowywanie udostępnionego moduł programu Windows PowerShell w konfiguracji. Po uruchomieniu skrypt łącznik tak, aby go można zaimportować każdy skrypt moduł programu Windows PowerShell jest wyodrębnionych w systemie plików.

Importowanie, eksportowanie i synchronizacji haseł skryptów wspólnego modułu wyodrębnieniu do folderu MAData łącznika. Schemat, sprawdzania poprawności hierarchii i partycją skryptów odnajdowania wspólnego modułu jest wyodrębnione do folderu % TEMP %. W obu przypadkach wyodrębnionej skrypt wspólnego modułu nosi nazwę zgodnie z ustawieniami typowych nazwa skryptu modułu.

Aby załadować moduł o nazwie FIMPowerShellConnectorModule.psm1 z folderu MAData, należy użyć następującej instrukcji:`Import-Module (Join-Path -Path [Microsoft.MetadirectoryServices.MAUtils]::MAFolder -ChildPath "FIMPowerShellConnectorModule.psm1")`

Aby załadować moduł o nazwie FIMPowerShellConnectorModule.psm1 z folderu % TEMP %, należy użyć następującej instrukcji:`Import-Module (Join-Path -Path $env:TEMP -ChildPath "FIMPowerShellConnectorModule.psm1")`

**Sprawdzanie poprawności parametru**  
Skrypt sprawdzania poprawności jest opcjonalne skryptu programu Windows PowerShell, który może być używany, aby upewnić się, że są prawidłowe parametry konfiguracji łącznika określone przez administratora. Sprawdzanie poprawności serwera, poświadczeń połączenia i parametry połączenia są typowe zastosowania skryptu weryfikacji. Skrypt sprawdzania poprawności jest nazywany po następujących kartach i okna dialogowe zostaną zmienione:

- Łączność
- Globalne parametry
- Konfiguracja partition

Skrypt sprawdzania poprawności otrzymuje następujące parametry z łącznik:

Nazwa | Typ danych | Opis
--- | --- | ---
ConfigParameterPage | [ConfigParameterPage][cpp] | Karta konfiguracji lub okno dialogowe, którego dotyczy żądania sprawdzenia poprawności.
ConfigParameters | [Kolekcji KeyedCollection] [ keyk] [ciąg znaków, [ConfigParameter][cp]] | Tabela parametry konfiguracji dla łącznika.
Poświadczenia | [Parametr PSCredential][pscred] | Zawiera poświadczeń wprowadzone przez administratora na karcie połączenia.

Skrypt sprawdzania poprawności powinna zwrócić jeden obiekt ParameterValidationResult do nich.

**Odnajdowanie schematu**  
Skrypt odnajdowania schematu jest wymagane. Skrypt zwraca typy obiektów, atrybuty i ograniczeń atrybut, używane przez usługę synchronizacji konfigurując reguły przepływu atrybut. Skrypt odnajdowania schematu jest uruchamiane podczas tworzenia łącznik i wypełnia schematu łącznika. Jest również używana przez akcję odświeżanie schematu w Menedżerze usługi synchronizacji.

Skrypt odnajdowania schematu otrzymuje następujące parametry z łącznik:

Nazwa | Typ danych | Opis
--- | --- | ---
ConfigParameters | [Kolekcji KeyedCollection] [ keyk] [ciąg znaków, [ConfigParameter][cp]] | Tabela parametry konfiguracji dla łącznika.
Poświadczenia | [Parametr PSCredential][pscred] | Zawiera poświadczeń wprowadzone przez administratora na karcie połączenia.

Skrypt musi zwracać pojedynczym [schematu] [ schema] obiektu do nich. Obiekt schematu składa się z [SchemaType] [ schemaT] obiekty reprezentujące typów obiektów (na przykład: Użytkownicy i grupy). Obiekt SchemaType zawiera zbiór [SchemaAttribute] [ schemaA] obiekty reprezentujące atrybuty (na przykład: imię, nazwisko i adres pocztowy) tego typu.

**Dodatkowe parametry**  
Oprócz ustawienia konfiguracji standardowej można zdefiniować dodatkowe niestandardowych ustawień konfiguracji charakterystyczne dla danego wystąpienia łącznik. Tych parametrów można je określić w łącznik partition, lub uruchamianie krok poziomy i uzyskać do nich dostęp z odpowiednich skrypt programu Windows PowerShell. Niestandardowych ustawień konfiguracji mogą być przechowywane w bazie danych usługi synchronizacji w formacie zwykłego tekstu lub mogą być szyfrowane. Usługi synchronizacji automatycznie są szyfrowane i odszyfrowywane bezpiecznego ustawienia konfiguracji, gdy jest to wymagane.

Aby określić niestandardowych ustawień konfiguracji, oddzielić nazwę każdego parametru średnik (;).

Aby uzyskać dostęp do niestandardowych ustawień konfiguracji ze skryptu, musi sufiks nazwy podkreśleniem ( \_ ), a zakres parametru (globalnego, partycją lub RunStep). Na przykład aby uzyskać dostęp do parametr FileName globalnego, za pomocą poniższym przykładzie:`$ConfigurationParameters["FileName_Global"].Value`

### <a name="capabilities"></a>Funkcje
Karta możliwości projektanta agenta zarządzania definiuje zachowanie i funkcji łącznika. Nie można zmodyfikować wybranych na tej karcie, gdy został utworzony łącznik. W poniższej tabeli wymieniono ustawienia możliwości.

![Funkcje](./media/active-directory-aadconnectsync-connector-powershell/capabilities.png)

Funkcja | Opis |
--- | --- |
[Nazwa wyróżniająca stylu][dnstyle] | Wskazuje, jeśli łącznik obsługuje nazwy wyróżniające i jeśli tak, co styl.
[Wyeksportuj typ][exportT] | Określa typ obiektów, które są przedstawiane skrypt eksportu. <li>AttributeReplace — zawiera pełny zestaw wartości atrybutu wielowartościowego po zmianie atrybutu.</li><li>AttributeUpdate — zawiera tylko może atrybutów wielowartościowych po zmianie atrybutu.</li><li>MultivaluedReferenceAttributeUpdate - zawiera pełny zestaw wartości atrybutów wielowartościowych-referencyjny i może tylko atrybutów wielowartościowych odwołania.</li><li>ObjectReplace — zawiera wszystkie atrybuty obiektu, gdy dowolne zmiany atrybutów</li>
[Normalizacji danych][DataNorm] | Powoduje, że usługi synchronizacji, którą należy znormalizować atrybuty kontrolnych, zanim są one udostępniane skryptów.
[Potwierdzenie obiektu][oconf] | Konfiguruje procesu importowania Oczekiwanie w usługi synchronizacji. <li>Normalny — domyślne zachowanie oczekuje wyeksportowane wszystkie zmiany potwierdzone, przy użyciu importu</li><li>NoDeleteConfirmation — po usunięciu obiektu jest nie oczekującego importowania wygenerowane.</li><li>NoAddAndDeleteConfirmation — obiektu została utworzona lub usunięte, jest nie oczekującego importowania wygenerowane.</li>
Użyj nazwy domeny jako kontrolnych | Jeśli odpowiedni styl nazwa wyróżniająca LDAP atrybut kotwicy obszar łączników jest również nazwa wyróżniająca.
Równoczesne wykonywanie operacji kilka łączników | W przypadku zaznaczenia wielu łączników środowiska Windows PowerShell można uruchamiać jednocześnie.
Partycje | Po zaznaczeniu łącznik obsługuje wiele partycje i odnajdowanie partycją.
Hierarchia | Po zaznaczeniu łącznik obsługuje LDAP styl hierarchicznej struktury.
Włączanie importu | Po zaznaczeniu łącznik importowanie danych za pomocą skryptów importu.
Włączanie różnicy importu | Po zaznaczeniu łącznika można zażądać może ze skryptów importu.
Włącz eksportowanie | Po zaznaczeniu łącznik umożliwia wyeksportowanie danych za pomocą skryptów eksportu.
Włączanie pełnego eksportu | Po zaznaczeniu skryptów eksportu obsługuje eksportowanie miejsca cały łącznik. Aby użyć tej opcji, Włącz eksportowanie również należy sprawdzić.
Brak wartości odwołania w przebiegu eksportu pierwszego | Po zaznaczeniu atrybuty odwołania są eksportowane w drugim przebiegu eksportu.
Włącz zmiany nazwy obiektu | W przypadku zaznaczenia, którą można modyfikować nazwy wyróżniające.
Usuń Dodawanie zastępowania | W przypadku zaznaczenia, Usuń dodać operacji są eksportowane jako jednego zastąpienia.
Włączanie operacje hasła | Po zaznaczeniu skryptów synchronizacji hasła są obsługiwane.
Włączanie hasła eksportu w pierwszym przebiegu | Po zaznaczeniu hasła ustawione podczas inicjowania obsługi administracyjnej są eksportowane po utworzeniu obiektu.

### <a name="global-parameters"></a>Globalne parametry
Na karcie Parametry globalne w Projektancie agenta zarządzania umożliwia konfigurowanie skrypty środowiska Windows PowerShell, które są uruchamiane przez łącznik. Można również skonfigurować globalne wartości dla niestandardowych ustawień konfiguracji zdefiniowane na karcie połączenia.

**Odnajdowanie partition**  
Partycją jest osobnym nazw w jeden schemat udostępnionych. Na przykład w usłudze Active Directory każda domena jest partycją w jeden las. Partycją jest logiczna Grupa importowania i eksportowania operacji. Importowanie i eksportowanie obowiązują partition kontekście i wszystkie operacje się dzieje w tym kontekście. Partycje ma reprezentować hierarchii w LDAP. Nazwa wyróżniająca partycją jest używana podczas importowania Aby sprawdzić, czy wszystkie obiekty zwrócone w zakresie partycją. Nazwa wyróżniająca partition służy także podczas inicjowania obsługi administracyjnej z metaverse do obszaru łącznik do określenia partition, które podczas eksportowania obiektu powinny być skojarzone z.

Skrypt odnajdowania partition otrzymuje następujące parametry z łącznik:

Nazwa | Typ danych | Opis
--- | --- | ---
ConfigParameters  | [Kolekcji KeyedCollection][keyk][ciąg znaków, [ConfigParameter][cp]] | Tabela parametry konfiguracji dla łącznika.
Poświadczenia | [Parametr PSCredential][pscred] | Zawiera poświadczeń wprowadzone przez administratora na karcie połączenia.

Skrypt musi zwracać jedną [partycją] [ part] obiektu lub listę [T] Partition obiektów do nich.

**Odnajdowanie hierarchii**  
Skrypt odnajdowania hierarchii jest używane tylko w przypadku funkcja styl nazwa wyróżniająca LDAP. Skrypt jest używany do umożliwiają przeglądanie i wybierz zestaw kontenerów uznaje i pomniejszanie zakres importowanie i eksportowanie operacji. Skrypt powinien zawierać tylko listę węzłów, które są bezpośrednie dzieci węzeł główny dostarczane do skryptu.

Skrypt odnajdowanie hierarchii otrzymuje następujące parametry z łącznik:

Nazwa | Typ danych | Opis
--- | --- | ---
ConfigParameters | [Kolekcji KeyedCollection][keyk][ciąg znaków, [ConfigParameter][cp]] | Tabela parametry konfiguracji dla łącznika.
Poświadczenia | [Parametr PSCredential][pscred] | Zawiera poświadczeń wprowadzone przez administratora na karcie połączenia.
ParentNode | [HierarchyNode][hn] | Węzeł główny hierarchii, pod którym skrypt powinna zwrócić bezpośrednich elementów podrzędnych.

Skrypt musi zwracać jeden obiekt HierarchyNode pojedynczy podrzędny lub listy [T] obiektów HierarchyNode podrzędnych do nich.

#### <a name="import"></a>Importowanie
Łączniki, które obsługują operacji importowania musi spełnić trzy skryptów.

**Rozpocznij importowanie**  
Skrypt Importowanie początkowego jest uruchamiany na początku krok Uruchom importowanie. W tym kroku można nawiązać połączenie z w źródłowym systemie i wykonaj czynności przygotowawczych przed zaimportowaniem danych z połączonego systemu.

Rozpocznij Importuj skrypt otrzymuje następujące parametry z łącznik:

Nazwa | Typ danych | Opis
--- | --- | ---
ConfigParameters | [Kolekcji KeyedCollection][keyk][ciąg znaków, [ConfigParameter][cp]] | Tabela parametry konfiguracji dla łącznika.
Poświadczenia | [Parametr PSCredential][pscred] | Zawiera poświadczeń wprowadzone przez administratora na karcie połączenia.
OpenImportConnectionRunStep | [OpenImportConnectionRunStep][oicrs] | Skrypt informuje o typie partition Uruchom importowanie (delta lub pełnej), hierarchii, znak wodny i rozmiaru strony oczekiwany.
Typy | [Schematu][schema] | Schemat dla obszarze łącznik, który jest importowana.

Skrypt musi zwracać pojedynczy [OpenImportConnectionResults] [ oicres] obiektu do nich, na przykład:`Write-Output (New-Object Microsoft.MetadirectoryServices.OpenImportConnectionResults)`

**Importowanie danych**  
Skrypt importu danych jest wywoływana przez łącznik do momentu skrypt wskazuje, że nie jest już danych do importowania. Łącznik programu Windows PowerShell ma rozmiar strony 9999 obiektów. Jeśli skrypt zwraca więcej niż 9999 obiekty do zaimportowania, musi obsługiwać nawigacji między stronami. Umożliwia łącznik uzyskanie dostępu, gdy właściwości niestandardowych danych, której można do sklepu znak wodny, aby zawsze skrypt importowanie danych zostanie wywołana, skrypt życiorysy, importowanie obiektów, w którym zostało przerwane.

Skrypt importowanie danych otrzymuje następujące parametry z łącznik:

Nazwa | Typ danych | Opis
--- | --- | ---
ConfigParameters | [Kolekcji KeyedCollection][keyk][ciąg znaków, [ConfigParameter][cp]] | Tabela parametry konfiguracji dla łącznika.
Poświadczenia | [Parametr PSCredential][pscred] | Zawiera poświadczeń wprowadzone przez administratora na karcie połączenia.
GetImportEntriesRunStep | [ImportRunStep][irs] | Przechowuje wodnego (CustomData), który może być używany podczas stronicowanej importowanie i importuje różnicy.
OpenImportConnectionRunStep | [OpenImportConnectionRunStep][oicrs] | Skrypt informuje o typie partition Uruchom importowanie (delta lub pełnej), hierarchii, znak wodny i rozmiaru strony oczekiwanej.
Typy | [Schematu][schema] | Schemat dla obszarze łącznik, który jest importowana.

Skrypt importowanie danych należy zapisywać listy [[CSEntryChange][csec]] obiektu do nich. Ta kolekcja składa się z atrybutów CSEntryChange, reprezentujących każdy obiekt importowany. Podczas wykonywania pełne importowanie tego zbioru powinien mieć pełny zestaw CSEntryChange obiektów, które mają wszystkie atrybuty dla każdego obiektu. Podczas importowanie różnicy obiektu CSEntryChange albo powinien zawierać atrybut może poziomu dla każdego obiektu, aby zaimportować lub wykonane reprezentacja obiektów, które zostały zmienione (w trybie Zamień).

**Importowanie zakończenia**  
Po zakończeniu importu Uruchom skrypt importowanie zakończenia jest uruchamiany. Ten skrypt należy wykonać dowolne zadania oczyszczania niezbędne (na przykład, zamknij połączeń z systemami) i odpowiadać na błędy.

Koniec Importuj skrypt otrzymuje następujące parametry z łącznik:

Nazwa | Typ danych | Opis
--- | --- | ---
ConfigParameters | [Kolekcji KeyedCollection][keyk][ciąg znaków, [ConfigParameter][cp]] | Tabela parametry konfiguracji dla łącznika.
Poświadczenia | [Parametr PSCredential][pscred] | Zawiera poświadczeń wprowadzone przez administratora na karcie połączenia.
OpenImportConnectionRunStep | [OpenImportConnectionRunStep][oicrs] | Skrypt informuje o typie partition Uruchom importowanie (delta lub pełnej), hierarchii, znak wodny i rozmiaru strony oczekiwany.
CloseImportConnectionRunStep | [CloseImportConnectionRunStep][cecrs] | Skrypt informuje o przyczyny, dla której Importowanie zostało zakończone.

Skrypt musi zwracać pojedynczy [CloseImportConnectionResults] [ cicres] obiektu do nich, na przykład:`Write-Output (New-Object Microsoft.MetadirectoryServices.CloseImportConnectionResults)`

#### <a name="export"></a>Eksportowanie
Taka sama, jak architektura importowanie łącznika, łączniki, które obsługują eksportu musi spełnić trzy skryptów.

**Rozpocznij eksportowanie**  
Skrypt eksportu początkowy jest wykonywane na początku etapu uruchamiania eksportu. W tym kroku można nawiązać połączenie z w źródłowym systemie i przeprowadzanie wszelkie czynności przygotowawczych przed wyeksportowaniem danych do połączonego systemu.

Skrypt eksportu początkowego otrzymuje następujące parametry z łącznik:

Nazwa | Typ danych | Opis
--- | --- | ---
ConfigParameters | [Kolekcji KeyedCollection][keyk][ciąg znaków, [ConfigParameter][cp]] | Tabela parametry konfiguracji dla łącznika.
Poświadczenia | [Parametr PSCredential][pscred] | Zawiera poświadczeń wprowadzone przez administratora na karcie połączenia.
OpenExportConnectionRunStep | [OpenExportConnectionRunStep][oecrs] | Skrypt informuje o typie partition uruchamiania eksportu (delta lub pełnej), hierarchii i rozmiaru strony oczekiwany.
Typy | [Schematu][schema] | Schemat odstępu łącznik, który jest eksportowany.

Skrypt nie powinna zwrócić dowolne dane wyjściowe do nich.

**Eksportowanie danych**  
Usługi synchronizacji połączeń skrypt eksportu danych tyle razy, ile jest konieczne przetwarzanie wszystkich oczekujących operacji eksportowania. Jeśli obszar łączników ma oczekujących operacji eksportowania więcej niż rozmiar strony łącznika, skrypt eksportu danych mogą być nazywane wielokrotnie i ewentualnie wiele razy dla tego samego obiektu.

Skrypt eksportu danych otrzymuje następujące parametry z łącznik:

Nazwa | Typ danych | Opis
--- | --- | ---
ConfigParameters | [Kolekcji KeyedCollection][keyk][ciąg znaków, [ConfigParameter][cp]] | Tabela parametry konfiguracji dla łącznika.
Poświadczenia | [Parametr PSCredential][pscred] | Zawiera poświadczeń wprowadzone przez administratora na karcie połączenia.
CSEntries | IList[CSEntryChange][csec] | Lista wszystkich obszar łączników obiekty z oczekujących Eksport do przetwarzania w tym przebiegu.
OpenExportConnectionRunStep | [OpenExportConnectionRunStep][oecrs] | Skrypt informuje o typie partition uruchamiania eksportu (delta lub pełnej), hierarchii i rozmiaru strony oczekiwany.
Typy | [Schematu][schema] | Schemat odstępu łącznik, który jest eksportowany.

Skrypt eksportu danych musi zwracać [PutExportEntriesResults] [ peeres] obiektu do nich. Ten obiekt nie musi zawierać informacje wyników dla każdego eksportowanego łącznika, chyba że pojawi się komunikat o błędzie lub zmiany atrybutów kotwicy. Na przykład, aby przywrócić obiekt PutExportEntriesResults proces:`Write-Output (New-Object Microsoft.MetadirectoryServices.PutExportEntriesResults)`

**Eksportowanie zakończenia**  
Po zakończeniu eksportowania uruchomić, Eksportuj zakończenia skrypt do uruchomienia. Ten skrypt należy wykonać dowolne zadania oczyszczania niezbędne (na przykład, zamknij połączeń z systemami) i odpowiadać na błędy.

Skrypt eksportu zakończenia otrzymuje następujące parametry z łącznik:

Nazwa | Typ danych | Opis
--- | --- | ---
ConfigParameters | [Kolekcji KeyedCollection][keyk][ciąg znaków, [ConfigParameter][cp]] | Tabela parametry konfiguracji dla łącznika.
Poświadczenia | [Parametr PSCredential][pscred] | Zawiera poświadczeń wprowadzone przez administratora na karcie połączenia.
OpenExportConnectionRunStep | [OpenExportConnectionRunStep][oecrs] | Skrypt informuje o typie partition uruchamiania eksportu (delta lub pełnej), hierarchii i rozmiaru strony oczekiwany.
CloseExportConnectionRunStep | [CloseExportConnectionRunStep][cecrs] | Skrypt informuje o przyczyny, dla której zostało zakończone Eksportuj.

Skrypt nie powinna zwrócić dowolne dane wyjściowe do nich.

#### <a name="password-synchronization"></a>Synchronizacja haseł
Łączniki programu Windows PowerShell może służyć jako miejsce docelowe dla zmiany i resetowanie haseł.

Skrypt hasło otrzymuje następujące parametry z łącznik:

Nazwa | Typ danych | Opis
--- | --- | ---
ConfigParameters | [Kolekcji KeyedCollection][keyk][ciąg znaków, [ConfigParameter][cp]] | Tabela parametry konfiguracji dla łącznika.
Poświadczenia | [Parametr PSCredential][pscred] | Zawiera poświadczeń wprowadzone przez administratora na karcie połączenia.
Partition | [Partition][part] | Partycją katalogu, której CSEntry.
CSEntry | [CSEntry][cse] | Pozycja obszar łącznik obiektu, który jest odebrana zmiana hasła lub resetowanie.
Typ operacji | Ciąg | Wskazuje, czy operacja jest resetowania (**UstawianieHasła**) lub zmień (**Element ChangePassword**).
PasswordOptions | [PasswordOptions][pwdopt] | Flagi określające zamierzonego hasło resetować zachowania. Ten parametr jest dostępna tylko w przypadku typ operacji **UstawianieHasła**.
Stare_hasło | Ciąg | Wypełniony obiektu stare hasło dla zmiany hasła. Ten parametr jest dostępna tylko w przypadku typ operacji **Element ChangePassword**.
NewPassword | Ciąg | Wypełniony obiektu nowego hasła, ustawiany skrypt.

Skrypt hasła nie powinien zwrócić wyniki do nich programu Windows PowerShell. W przypadku wystąpienia błędu skryptu hasła, skrypt powinien zgłosić, jednym następujące wyjątki do informowania o tym problemie usługi synchronizacji:

- [PasswordPolicyViolationException] [ pwdex1] — wygenerowany, jeśli hasło nie spełnia zasady haseł w systemie połączonego.
- [PasswordIllFormedException] [ pwdex2] — wygenerowany, jeśli hasło nie jest do przyjęcia dotyczących połączonego systemu.
- [PasswordExtension] [ pwdex3] — wyjątek dla wszystkich innych błędów w obszarze skrypt hasła.

## <a name="sample-connectors"></a>Przykładowe łączników
Pełne omówienie łączników dostępne próbki, zobacz [Zbieranie łącznika w systemie Windows PowerShell łącznik próbki][samp].

## <a name="other-notes"></a>Inne uwagi

### <a name="additional-configuration-for-impersonation"></a>Dodatkowa konfiguracja personifikacji
Udzielanie, że użytkownik, który jest personalizacji następujące uprawnienia na serwerze usług synchronizacji:

Odczyt następujące klucze rejestru:

- HKEY_USERS\\\Software\Microsoft\PowerShell [SynchronizationServiceServiceAccountSID]
- HKEY_USERS\\\Environment [SynchronizationServiceServiceAccountSID]

Aby określić identyfikator zabezpieczeń (SID) konta usługi usługi synchronizacji, uruchom następujące polecenia programu PowerShell:

```
$account = New-Object System.Security.Principal.NTAccount "<domain>\<username>"
$account.Translate([System.Security.Principal.SecurityIdentifier]).Value
```

Dostęp do odczytu do następujących folderów systemu plików:

- %ProgramFiles%\Microsoft forefront tożsamości Manager\2010\Synchronization Service\Extensions
- %ProgramFiles%\Microsoft forefront tożsamości Manager\2010\Synchronization Service\ExtensionsCache
- %ProgramFiles%\Microsoft forefront tożsamości Manager\2010\Synchronization Service\MaData\\{ConnectorName}

Symbol zastępczy {ConnectorName} podstawić nazwę łącznika programu Windows PowerShell.

## <a name="troubleshooting"></a>Rozwiązywanie problemów

-   Aby uzyskać informacje dotyczące włączania rejestrowania rozwiązywać łącznik zobacz [jak włączyć ETW Tracing łączników](http://go.microsoft.com/fwlink/?LinkId=335731).

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[cpp]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.configparameterpage.aspx
[keyk]: https://msdn.microsoft.com/library/ms132438.aspx
[cp]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.configparameter.aspx
[pscred]: https://msdn.microsoft.com/library/system.management.automation.pscredential.aspx
[schema]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schema.aspx
[schemaT]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schematype.aspx
[schemaA]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schemaattribute.aspx
[dnstyle]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.madistinguishednamestyle.aspx
[exportT]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.maexporttype.aspx
[DataNorm]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.manormalizations.aspx
[oconf]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.maobjectconfirmation.aspx
[dw]: https://msdn.microsoft.com/library/windows/desktop/aa378184.aspx
[part]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.partition.aspx
[hn]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.hierarchynode.aspx
[oicrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openimportconnectionrunstep.aspx
[cecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeexportconnectionrunstep.aspx
[oicres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openimportconnectionresults.aspx
[cecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeexportconnectionrunstep.aspx
[cicres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeimportconnectionresults.aspx
[oecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openexportconnectionrunstep.aspx
[irs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.importrunstep.aspx
[cse]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.csentry.aspx
[csec]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.csentrychange.aspx
[peeres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.putexportentriesresults.aspx
[pwdopt]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordoptions.aspx
[pwdex1]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordpolicyviolationexception.aspx
[pwdex2]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordillformedexception.aspx
[pwdex3]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordextensionexception.aspx
[samp]: http://go.microsoft.com/fwlink/?LinkId=394291
