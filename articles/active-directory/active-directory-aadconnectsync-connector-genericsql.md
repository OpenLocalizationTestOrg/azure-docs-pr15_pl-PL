<properties
   pageTitle="Łącznik ogólnego SQL | Microsoft Azure"
   description="W tym artykule opisano, jak skonfigurować łącznik SQL ogólnego firmy Microsoft."
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

# <a name="generic-sql-connector-technical-reference"></a>Ogólne informacje techniczne łącznik SQL

Ten artykuł zawiera opis ogólnego łącznik SQL. Artykuł dotyczy następujących produktów:

- Menedżer tożsamości 2016 (MIM2016)
- Produkt Forefront Identity Manager 2010 R2 (FIM2010R2)
    -   Należy użyć poprawki 4.1.3671.0 lub nowszym [KB3092178](https://support.microsoft.com/kb/3092178).

MIM2016 i FIM2010R2 łącznik jest dostępna do pobrania z [Centrum pobierania Microsoft](http://go.microsoft.com/fwlink/?LinkId=717495).

Aby wyświetlić ten łącznik w działaniu, zobacz artykuł [Ogólnego łącznik SQL krok po kroku](active-directory-aadconnectsync-connector-genericsql-step-by-step.md) .

## <a name="overview-of-the-generic-sql-connector"></a>Omówienie łącznik ogólnego SQL

Rodzajowy łącznik SQL umożliwia Integracja usługi synchronizacji z systemu bazy danych, który oferuje połączenie ODBC.  

Z perspektywy wysokiego poziomu następujące funkcje są obsługiwane w bieżącej wersji łącznika:

Funkcja | Pomoc techniczna
--- | ---
Połączone źródło danych | Łącznik jest obsługiwana przez wszystkie sterowniki ODBC 64-bitowej. Przetestowano go z następujących czynności: <li>Program Microsoft SQL Server i platformy SQL Azure</li><li>Programu IBM DB2 10.x</li><li>Programu IBM DB2 9.x</li><li>Oracle 10 i 11 g</li><li>MySQL 5.x</li>
Scenariusze   | <li>Zarządzanie cyklu życia obiektu</li><li>Zarządzanie hasłami</li>
Operacje | <li>Pełne importowanie i różnicy importowanie i eksportowanie</li><li>Dla eksportowanie: Dodawanie, usuwanie aktualizacji i Zamień</li><li>Ustawianie hasła, zmienianie hasła</li>
Schematu | <li>Dynamiczne odnajdowanie atrybutów i obiektów</li>

### <a name="prerequisites"></a>Wymagania wstępne
Przed użyciem łącznika upewnij się, że masz na serwerze synchronizacji z następujących czynności:

- Program Microsoft .NET 4.5.2 Framework lub nowszy
- 64-bitowa POP klienta

### <a name="permissions-in-connected-data-source"></a>Uprawnienia w źródle danych połączonego
Aby utworzyć lub wykonywać żadnych obsługiwanych zadań w programie SQL ogólnego connector, musisz mieć:

- db_datareader
- db_datawriter

### <a name="ports-and-protocols"></a>Porty i protokoły
Wymagane porty dla sterownika ODBC do pracy można znaleźć w dokumentacji dostawcy bazy danych.

## <a name="create-a-new-connector"></a>Tworzenie nowego łącznika
Aby utworzyć łącznik ogólnego SQL, w **Usługi synchronizacji** wybierz **Agent zarządzania** i **Tworzenie**. Zaznacz łącznik **ogólnego SQL (Microsoft)** .

![CreateConnector](./media/active-directory-aadconnectsync-connector-genericsql/createconnector.png)

### <a name="connectivity"></a>Łączność
Łącznik używa pliku ODBC DSN łączności. Utwórz plik DSN korzystanie ze **Źródłami danych ODBC** dostępnej w menu start w obszarze **Narzędzia administracyjne**. W narzędziu administracyjnym Utwórz **Plik DSN** , aby może być udostępniana do łącznika.

![CreateConnector](./media/active-directory-aadconnectsync-connector-genericsql/connectivity.png)

Na ekranie łączności jest pierwszą podczas tworzenia nowego ogólnego łącznika SQL. Najpierw należy podać następujące informacje:

- Ścieżka pliku DSN
- Uwierzytelnianie
    - Nazwa użytkownika
    - Hasło

Bazy danych powinien obsługiwać jednej z następujących metod uwierzytelniania:

- **Uwierzytelnianie systemu Windows**: uwierzytelniających bazy danych używa poświadczeń systemu Windows do weryfikacji użytkownika. Nazwa użytkownika i hasła określony jest używane do uwierzytelniania z bazą danych. To konto wymaga uprawnienia do bazy danych.
- **Uwierzytelnianie programu SQL**: uwierzytelniania bazy danych przy użyciu nazwy użytkownika i hasło zdefiniowana łączności ekranu w celu połączenia z bazą danych. Jeśli nazwa użytkownika i hasło są przechowywane w pliku DSN, poświadczenia na ekranie łączności mają priorytet.
- **Uwierzytelnianie bazy danych SQL Azure**: Aby uzyskać więcej informacji, zobacz [Łączenie się SQL bazy danych przy użyciu Azure uwierzytelnianie usługi Active Directory](..\sql-database\sql-database-aad-authentication.md).

**Nazwy domeny jest kotwicy**: po wybraniu tej opcji, nazwy domeny jest również używana jako atrybut kotwicy. Może być używany do wykonania prostego, ale również ma następujące ograniczenie:

-   Łącznik obsługuje tylko jeden typ obiektu. W związku z tym atrybuty odwołanie może odwoływać się tylko do tego samego typu obiektu.

**Typ eksportu: obiekt Zamień**: podczas eksportowania, gdy tylko niektóre atrybuty, które zostały zmienione, cały obiekt ze wszystkimi atrybutami jest eksportowany i powoduje zastąpienie istniejącego obiektu.

### <a name="schema-1-detect-object-types"></a>Schematu 1 (typy obiektów Wykryj)
Na tej stronie zamierzasz skonfigurować sposób łącznik ma znaleźć typy różnych obiektów w bazie danych.

Każdy typ obiektu jest prezentowany jako i skonfigurowany dalej **skonfigurować partycje i hierarchie**.

![schema1a](./media/active-directory-aadconnectsync-connector-genericsql/schema1a.png)

**Metody wykrywania typu obiektu**: łącznik obsługuje następujące metody wykrywania typu obiektu.

- **Stała wartość**: Podaj listę typów obiektów z rozdzielaną średnikami listę. Na przykład: `User,Group,Department`.  
![schema1b](./media/active-directory-aadconnectsync-connector-genericsql/schema1b.png)
- **Procedury Tabela/Widok/przechowywanej**: Podaj nazwę tabeli i widok przechowywany procedury, a następnie nazwę kolumny, która zawiera listę typów obiektów. Jeśli używasz procedura składowana, następnie udostępniają parametrów dla niego w formacie **[nazwa]: [kierunek]: [wartość]**. Podaj każdego parametru w osobnym wierszu (używaj klawisze Ctrl + Enter, aby wstawić nowy wiersz).  
![schema1c](./media/active-directory-aadconnectsync-connector-genericsql/schema1c.png)
- **Kwerenda SQL**: Ta opcja umożliwia przekazanie zapytania SQL zwracające pojedynczą kolumnę z typów obiektów, na przykład `SELECT [Column Name] FROM TABLENAME`. Zwrócone kolumna musi być typu ciąg (varchar).

### <a name="schema-2-detect-attribute-types"></a>Schemat 2 (typy atrybut Wykryj)
Na tej stronie zamierzasz skonfigurować, jak nazwy atrybutów i typów mają wykrywania. Opcje konfiguracji są wyświetlane dla każdego typu obiektu wykryty na poprzedniej stronie.

![schema2a](./media/active-directory-aadconnectsync-connector-genericsql/schema2a.png)

**Metody wykrywania typu atrybutu**: łącznik obsługuje następujące metody wykrywania typu atrybut z każdego typu obiektu wykryto na ekranie schematu 1.

- **Procedury Tabela/Widok/przechowywanej**: Podaj nazwę tabeli i widok i przechowywany procedury, który ma być używany do znalezienia nazwy atrybutów. Jeśli używasz procedura składowana również dostarczać parametrów dla go w formacie **[nazwa]: [kierunek]: [wartość]**. Podaj każdego parametru w osobnym wierszu (używaj klawisze Ctrl + Enter, aby wstawić nowy wiersz). Wykrywanie nazwy atrybutów w atrybutów wielowartościowych, podaj przecinkami lista tabel lub widoków. Scenariusze wielowartościowe nie są obsługiwane podczas Tabela elementy nadrzędne i podrzędne mieć takie same nazwy kolumny.
- **Kwerenda SQL**: Ta opcja umożliwia przekazanie zapytania SQL, która zwraca pojedyncza kolumna zawierająca nazwy atrybutów, na przykład `SELECT [Column Name] FROM TABLENAME`. Zwrócone kolumna musi być typu ciąg (varchar).

### <a name="schema-3-define-anchor-and-dn"></a>Schemat 3 (Definiuj kontrolnych i nazwy domeny)
Ta strona umożliwia skonfigurowanie atrybutów nazwy domeny dla każdego typu obiektu Wykryto i kontrolnych. Można wybrać wielu atrybut aby kotwicy unikatowe.

![schema3a](./media/active-directory-aadconnectsync-connector-genericsql/schema3a.png)

- Atrybuty wielowartościowe i logiczne nie są widoczne.
- Samego atrybutu nie można używać nazwy domeny i punktów kontrolnych, o ile nie zaznaczono **nazwy domeny jest zakotwiczenia** na stronie łączność.
- Jeśli wybrano **nazwy domeny jest zakotwiczenia** na stronie łączność, ta strona wymaga atrybutu nazwy domeny. Ten atrybut może również jako atrybut kotwicy.
![schema3b](./media/active-directory-aadconnectsync-connector-genericsql/schema3b.png)

### <a name="schema-4-define-attribute-type-reference-and-direction"></a>Schemat 4 (Definiuj typ atrybutu, odwołania i kierunku)
Ta strona umożliwia skonfigurowanie typ atrybut, na przykład liczba całkowita, binarnego, lub wartość logiczna i kierunek dla każdego atrybutu. Wszystkie atrybuty ze strony **schematu 2** znajdują się wraz z atrybutami wiele wartości.

![schema4a](./media/active-directory-aadconnectsync-connector-genericsql/schema4a.png)

- **Typ danych**: używany do mapowania typu atrybutu tych typów, znane przy użyciu aparatu synchronizacji. Domyślnie jest używać tego samego typu, jak wykryto w schemacie SQL, ale nie są łatwo daty/godziny i odwołań. Dla osób musisz określić **daty/godziny** lub **Odwołanie**.
- **Kierunek**: rozmieszczanie atrybut importowanie, eksportowanie lub ImportExport. ImportExport to ustawienia domyślne.
![schema4b](./media/active-directory-aadconnectsync-connector-genericsql/schema4b.png)

Uwagi:

- Jeśli nie jest wykrywalna przez łącznik typu atrybut, to wybiera typ danych String.
- **Tabele zagnieżdżone** można uznać tabel bazy danych jedną kolumnę. Oracle przechowuje wiersze tabeli zagnieżdżonej w losowej kolejności. Jednak podczas pobierania tabelę zagnieżdżoną do zmiennej PL/SQL, wiersze są podane dolnego następujących po sobie, począwszy od 1. Który umożliwia tablicy przypominających dostęp do poszczególnych wierszy.
- **VARRYS** nie są obsługiwane w łącznik.

### <a name="schema-5-define-partition-for-reference-attributes"></a>Schemat 5 (Definiuj partycją atrybutów odwołanie)
Na tej stronie można skonfigurować dla wszystkich atrybutów odwołanie, które partition (typ obiektu) odwołuje się atrybut do.

![schema5](./media/active-directory-aadconnectsync-connector-genericsql/schema5.png)

Korzystając z **nazwy domeny jest kotwicy**, należy użyć tego samego typu obiektu, który odnosi się z. Nie można odwoływać się inny typ obiektu.

### <a name="global-parameters"></a>Globalne parametry
Na stronie parametry globalne służy do konfigurowania Import różnicy, format daty/godziny i metody hasła.

![globalparameters1](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters1.png)

Rodzajowy łącznik SQL obsługuje następujące metody importowania różnicy:

- **Wyzwalacza**: zobacz [Generowanie widoków Delta przy użyciu wyzwalaczy](https://technet.microsoft.com/library/cc708665.aspx).
- **Znak wodny**: ogólne podejście, które mogą być używane z dowolnej bazy danych. Kwerenda znak wodny jest wstępnie wypełnione według dostawcy bazy danych. Kolumny znak wodny musi znajdować się na każdej tabeli i widok używany. W tej kolumnie należy śledzić zostanie wstawiony i aktualizacje do tabel, jak i jego zależne od (wiele wartości lub podrzędny) tabel. Należy zsynchronizować zegary między usługą synchronizacji i serwer bazy danych. W przeciwnym razie niektóre wpisy w oknie importowanie różnicy może zostać pominięty.  
Ograniczenia:
    - Znak wodny strategii czy nie pomocy technicznej usunięte obiekty.
- **Migawka**: (działa tylko w przypadku programu Microsoft SQL Server) [generowania widoków różnicy za pomocą migawek](https://technet.microsoft.com/library/cc720640.aspx)
- **Śledzenie zmian**: (działa tylko w przypadku programu Microsoft SQL Server), [funkcja śledzenia zmian](https://msdn.microsoft.com/library/bb933875.aspx)  
Ograniczenia:
    - Kontrolnych i atrybut nazwy domeny musi być częścią klucza podstawowego dla zaznaczonego obiektu w tabeli.
    - Kwerenda SQL nie jest obsługiwany podczas importowania i eksportowania ze śledzeniem zmian.

**Dodatkowe parametry**: Określ strefa czasowa serwera bazy danych wskazujące, gdzie znajduje się serwer bazy danych. Ta wartość jest używana do obsługi różnych formatów atrybutów Data i godzina.

Łącznik zawsze przechowuje daty i godziny daty w formacie UTC. Aby można było poprawnie przekonwertować daty i godziny, strefa czasowa serwera bazy danych i format używany jest wymagane. Format powinna być wyrażona w formacie .net.

Podczas eksportowania każdego atrybutu czasu daty należy podać do łącznika w formacie czasu UTC.

![globalparameters2](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters2.png)

**Hasło konfiguracji**: łącznik zapewnia możliwości synchronizacji haseł i obsługuje ustawianie i zmienianie hasła.

Łącznik oferuje dwie metody do obsługi synchronizacji haseł:

- **Procedura składowana**: Ta metoda wymaga dwóch procedur składowanych do obsługi zestawu i Zmień hasło. Wpisz wszystkie parametry Dodawanie i zmienianie operacji hasła w **SP ustawić hasło** i **Zmienianie hasła SP** parametrów odpowiednio jak na poniższym przykładzie.
![globalparameters3](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters3.png)
- **Rozszerzenie hasła**: Ta metoda wymaga rozszerzenia hasło DLL (musisz podać nazwę DLL rozszerzenia, która jest implementacji interfejsu [IMAExtensible2Password](https://msdn.microsoft.com/library/microsoft.metadirectoryservices.imaextensible2password.aspx) ). Hasło rozszerzenia zestawu musi znajdować się w folderze rozszerzenia tak, aby łącznik można załadować DLL w czasie rzeczywistym.
![globalparameters4](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters4.png)

Musisz również włączyć zarządzanie hasłami na stronie **Konfigurowanie dodatkowej** .
![globalparameters5](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters5.png)

### <a name="configure-partitions-and-hierarchies"></a>Konfigurowanie partycje i hierarchie
Na stronie partycje i hierarchie wybierz pozycję wszystkie typy obiektów. Partycją własnej każdego typu obiektu.

![partitions1](./media/active-directory-aadconnectsync-connector-genericsql/partitions1.png)

Można także zastąpić wartości zdefiniowanych na stronie **łączności** lub **Globalne parametry** .

![partitions2](./media/active-directory-aadconnectsync-connector-genericsql/partitions2.png)

### <a name="configure-anchors"></a>Konfigurowanie kotwiczenie
Ta strona jest tylko do odczytu, ponieważ kotwicy został już zdefiniowany. Atrybut zaznaczonego kotwicy jest zawsze dołączony typ obiektu, aby upewnić się, że pozostaje unikatowe różnych typów obiektów.

![Kotwiczenie](./media/active-directory-aadconnectsync-connector-genericsql/anchors.png)

## <a name="configure-run-step-parameter"></a>Konfigurowanie parametrów wykonywania kroku
Te kroki są skonfigurowane na uruchamianie profilów łącznika. Tę konfigurację wykonaj praca rzeczywista importowania i eksportowania danych.

### <a name="full-and-delta-import"></a>Pełna i importowania różnicy
Ogólne wsparcie łącznik SQL pełnej i Import Delta przy użyciu następujących metod:

- Tabela
- Widok
- Procedura składowana
- Kwerenda SQL

![runstep1](./media/active-directory-aadconnectsync-connector-genericsql/runstep1.png)

**Tabela/Widok**  
Aby zaimportować wielowartościowe atrybuty obiektu, musisz podać nazwę przecinkami i widoku tabeli w **widoku tabeli nazwę wielowartościowe** i odpowiednich sprzężenia w **warunku sprzężenia** z tabeli nadrzędnej.

Przykład:, Który chcesz zaimportować obiektu pracownika i jego atrybuty wielowartościowe. Istnieją dwie tabele, o nazwie pracownika (Tabela głównym) i dział (wiele wartości).
Wykonaj następujące czynności:

- Typ **pracownika** w **Tabeli i widoku SP**.
- Wpisz dział w **widokach tabeli Nazwa wielowartościowe**.
- Wpisz warunek sprzężenia między pracownika & działu w **Warunku sprzężenia**, na przykład `Employee.DEPTID=Department.DepartmentID`.
![runstep2](./media/active-directory-aadconnectsync-connector-genericsql/runstep2.png)

**Procedury składowane**  
![runstep3](./media/active-directory-aadconnectsync-connector-genericsql/runstep3.png)

- Jeśli masz dużej ilości danych, zaleca się do wykonania podziałem na strony przy użyciu usługi procedury składowane.
- Dla procedury przechowywane do obsługi podziałem na strony należy podać rozpocząć zakończenia indeks i. Zobacz: [efektywne stronicowania za pośrednictwem dużych ilości danych](https://msdn.microsoft.com/library/bb445504.aspx).
- @StartIndexi @EndIndex są zastępowane w czasie wykonywania wartość rozmiaru strony odpowiednich skonfigurowane na stronie **Konfigurowanie kroku** . Na przykład, kiedy łącznik pobiera na pierwszej stronie i rozmiaru strony jest ustawiona 500, w takiej sytuacji @StartIndex 1 i @EndIndex 500. Te wartości Zwiększ po łącznik pobiera kolejnych stron i zmień @StartIndex & @EndIndex wartość.
- Aby wykonać parametryczne procedura przechowywana, podaj parametrów w `[Name]:[Direction]:[Value]` format. Wprowadź każdego parametru w osobnym wierszu (Użyj klawisza Ctrl + Enter, aby uzyskać nowy wiersz).
- Rodzajowy łącznik SQL obsługuje także operacji importowania z serwerów połączonych w programie Microsoft SQL Server. Jeśli informacje mają zostać pobrane z tabeli w polu Serwer połączony, tabeli należy przygotować w formacie:`[ServerName].[Database].[Schema].[TableName]`
- Rodzajowy łącznik SQL obsługuje tylko te obiekty, które mają strukturę podobne (zarówno alias nazwę i typ danych) między uruchomienie czynności wykrywania schematów i informacji. Zaznaczony obiekt ze schematem i dostarczone informacje podczas wykonywania czynności jest inny, łącznik SQL jest nie obsługuje tego typu scenariuszy.

**Kwerenda SQL**  
![runstep4](./media/active-directory-aadconnectsync-connector-genericsql/runstep4.png)

![runstep5](./media/active-directory-aadconnectsync-connector-genericsql/runstep5.png)

- Zestaw wielu wyników kwerend nie jest obsługiwane.
- Kwerenda SQL obsługuje podziałem na strony i podaj start zakończenia indeks i jako zmienna do obsługi podziałem na strony.

### <a name="delta-import"></a>Importowanie delta
![runstep6](./media/active-directory-aadconnectsync-connector-genericsql/runstep6.png)

Zmiana Importuj konfigurację wymaga więcej niektóre czynności konfiguracyjnych w porównaniu z pełne importowanie.

- Jeśli wybierzesz wyzwalacza lub migawki podejście do śledzenia zmian różnicy dostarczać tabeli historii lub migawki bazy danych w polu **tabeli historii lub migawki nazwę bazy danych** .
- Należy podać warunek sprzężenia między tabelami Historia i nadrzędnej, na przykład`Employee.ID=History.EmployeeID`
- Aby śledzić transakcji w tabeli nadrzędnej z tabeli historii, należy podać nazwę kolumny, zawierający informacje o operacji (Dodaj Aktualizuj/Usuń).
- Jeśli znak wodny, aby śledzić zmiany różnicy, podać nazwę kolumny, zawierający informacje o operacji w polu **Nazwa kolumny Oznacz wody**.
- Kolumna **Zmień typ atrybut** jest wymagane na potrzeby zmień typ. W tej kolumnie mapy zmianę odbywa się w podstawowej tabeli lub tabeli wielowartościowych Zmień typ w widoku delta. W tej kolumnie mogą zawierać Modify_Attribute Zmień typ Zmień poziom atrybutu lub dodawanie, modyfikowanie, lub usuń Zmień typ typu zmiany na poziomie obiektu. Jeśli jest innej niż wartość domyślna Dodawanie, modyfikowanie, lub Usuń, a następnie należy zdefiniować te wartości przy użyciu tej opcji.

### <a name="export"></a>Eksportowanie
![runstep7](./media/active-directory-aadconnectsync-connector-genericsql/runstep7.png)

Rodzajowy łącznik SQL obsługuje eksportowania przy użyciu czterech obsługiwanych metod, takich jak:

- Tabela
- Widok
- Procedura składowana
- Kwerenda SQL

**Tabela/Widok**  
Jeśli wybierzesz opcję tabeli/widoku, łącznik generuje odpowiednich zapytań do eksportowania.

**Procedury składowane**  
![runstep8](./media/active-directory-aadconnectsync-connector-genericsql/runstep8.png)

Jeśli wybierzesz opcję procedura przechowywana, eksportowanie wymaga trzy różne procedury przechowywane do wykonywania operacji Wstaw/Aktualizuj/Usuń.

- **Dodaj nazwę SP**: SP ten jest uruchamiana, gdy dowolny obiekt względem łącznika do wstawiania w odpowiedniej tabeli.
- **Aktualizowanie nazwy SP**: SP ten jest uruchamiana, gdy dowolny obiekt względem łącznik aktualizacji w odpowiedniej tabeli.
- **Usuwanie nazwy SP**: SP ten jest uruchamiana, gdy dowolny obiekt względem łącznik do usunięcia w odpowiedniej tabeli.
- Atrybut zaznaczone ze schematu używane jako wartości parametru do procedury składowanej. Na przykład `EmployeeName: INPUT: @EmployeeName` (EmployeeName jest zaznaczone w schemacie łącznik i łącznik zastępuje odpowiednią wartość podczas wykonywania eksportu)
- Aby uruchomić parametryczne procedura składowana, podaj parametrów w `[Name]:[Direction]:[Value]` format. Wprowadź każdego parametru w osobnym wierszu (Użyj klawisza Ctrl + Enter, aby uzyskać nowy wiersz).

**Kwerenda SQL**  
![runstep9](./media/active-directory-aadconnectsync-connector-genericsql/runstep9.png)

Jeśli wybierzesz opcję zapytania SQL, eksportowanie wymaga trzech różnych kwerend do wykonywania operacji Wstaw/Aktualizuj/Usuń.

- **Wstawianie zapytania**: Ta kwerenda jest uruchamiany, jeśli dowolny obiekt względem łącznika do wstawiania w odpowiedniej tabeli.
- **Aktualizowanie kwerendy**: Ta kwerenda jest uruchamiany, jeśli dowolny obiekt względem łącznik aktualizacji w odpowiedniej tabeli.
- **Usuń kwerendę**: Ta kwerenda jest uruchamiany, jeśli dowolny obiekt względem łącznik do usunięcia w odpowiedniej tabeli.
- Atrybut zaznaczone ze schematu używane jako wartości parametru do kwerendy, na przykład`Insert into Employee (ID, Name) Values (@ID, @EmployeeName)`

## <a name="troubleshooting"></a>Rozwiązywanie problemów

-   Aby uzyskać informacje dotyczące włączania rejestrowania rozwiązywać łącznik zobacz [jak włączyć ETW Tracing łączników](http://go.microsoft.com/fwlink/?LinkId=335731).
