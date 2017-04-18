<properties
   pageTitle="Za pomocą łączników SQL w aplikacjach logiczny | Microsoft Azure aplikacji usługi"
   description="Jak utworzyć i skonfigurować aplikację łącznika SQL lub interfejsu API i używać go w aplikacji dla logiki Azure aplikacji usługi"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="anuragdalmia"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="09/01/2016"
   ms.author="sameerch"/>


# <a name="get-started-with-the-microsoft-sql-connector-and-add-it-to-your-logic-app"></a>Wprowadzenie do programu Microsoft SQL Connector i dodać go do aplikacji warunków logicznych
>[AZURE.NOTE] Tą wersją artykułu dotyczy wersji schematu 2014-12-01-podgląd aplikacji logicznych. Wersja schematu Azure SQL 2015-08-01-podglądu kliknij [Interfejs API programu SQL Azure](../connectors/connectors-create-api-sqlazure.md).

Nawiązywanie połączenia do lokalnego programu SQL Server lub bazy danych SQL Azure, tworzyć i zmieniać usługi informacje lub dane. Łączniki może służyć w aplikacjach logiki do pobrania, proces, lub push danych jako część "przepływ pracy". Gdy używasz łącznik SQL w przepływie pracy można uzyskać wiele różnych scenariuszy. Na przykład można wykonywać następujące czynności:

- Uwidaczniają sekcji danych znajdujących się w bazie danych SQL przy użyciu sieci web lub w aplikacji mobilnej.
- Wstawianie danych do tabeli bazy danych SQL do przechowywania. Na przykład można wprowadzić rekordy pracowników, aktualizowanie zamówień sprzedaży i tak dalej.
- Pobieranie danych z programu SQL i używać go w procesie biznesowym. Na przykład możesz uzyskać rekordów klientów i umieścić rekordy klientów w usług SalesForce.

Możesz dodać łącznik SQL do przepływu pracy i proces danych biznesowych w ramach tego przepływu pracy w aplikacji logicznych. 

## <a name="triggers-and-actions"></a>Wyzwalacze i akcje
*Wyzwalacze* są o zdarzeniach. Na przykład jest dodawany po zaktualizowaniu rzędu lub nowym klientem. *Akcja* jest wynikiem wyzwalacza. Na przykład po zaktualizowaniu rzędu powiadomić do sprzedawcy. Lub dodanie nowego klienta wysyłanie powitalna wiadomość e-mail do nowego klienta.

Łącznik SQL może służyć jako wyzwalacz lub akcji w logiczny aplikacji oraz obsługuje dane w formatach XML i JSON. Dla każdej tabeli zawarte w opakowaniu ustawienia (więcej informacji na temat której w dalszej części tego tematu), jest JSON działań oraz działań XML.

Łącznik SQL występują następujące wyzwalaczami i dostępne akcje:

Wyzwalaczy | Akcje
--- | ---
Dane ankiety | <ul><li>Wstawianie tabeli</li><li>Aktualizuj spis</li><li>Wybierz pozycję z tabeli</li><li>Usuwanie z tabeli</li><li>Procedura składowana połączeń</li></ul>

## <a name="create-the-sql-connector"></a>Tworzenie łącznika SQL

Łącznik można utworzyć w aplikacji logicznych lub być tworzone bezpośrednio z usługi Azure Marketplace. Aby utworzyć łącznik z witryny Marketplace:  

1. W startboard Azure wybierz pozycję **Marketplace**.
2. Wyszukiwanie "SQL łącznik", zaznacz go i wybierz pozycję **Utwórz**.
3. Wprowadź nazwę, planowanie usługi aplikacji i inne właściwości.
4. Wprowadź następujące ustawienia pakietu:

    Nazwa | Wymagane |  Opis
--- | --- | ---
Nazwa serwera | Tak | Wprowadź nazwę serwera SQL. Wpisz na przykład *SQL sqlexpress* lub *SQLserver.mydomain.com*.
Port | Brak | Domyślnym jest 1433.
Nazwa użytkownika | Tak | Wprowadź nazwę użytkownika, który może zalogować się do programu SQL Server. Jeśli połączenie do lokalnego programu SQL Server, wprowadź poświadczenia uwierzytelniania SQL.
Hasło | Tak | Wprowadź nazwę użytkownika i hasło.
Nazwa bazy danych | Tak | Wprowadź bazy danych, z którym masz połączenie. Na przykład można wprowadzić *klientów* lub *dbo i zamówienia*.
Lokalne | Tak | Domyślna to False. Wprowadź wartość FAŁSZ, jeśli połączenie z bazą danych programu Azure SQL. Wprowadź wartość True, jeśli połączenie z lokalnego serwera SQL.
Parametry połączenia Bus usługi | Brak | Jeśli łączysz się do lokalnego wprowadź parametry połączenia przekazywania Bus usługi.<br/><br/>[Korzystanie z Menedżera połączeń hybrydowego](app-service-logic-hybrid-connection-manager.md)<br/>[Usługi ceny Bus](https://azure.microsoft.com/pricing/details/service-bus/)
Nazwa serwera partnera | Brak | Jeśli podstawowy serwer jest niedostępny, możesz wprowadzić serwer partnera jako alternatywnego lub kopii zapasowych serwera.
Tabele | Brak | Listy tabel bazy danych, które mogą być aktualizowane przez łącznik. Na przykład wpisz *OrdersTable* lub *EmployeeTable*. Jeśli wprowadzono żadnych tabel, można użyć wszystkich tabel. Aby użyć tego łącznika jako akcji są wymagane prawidłowych tabel i/lub procedury składowane.
Procedury składowane | Brak | Wprowadź istniejące procedura przechowywana, która może być wywoływana przez łącznik. Na przykład wpisz *sp_IsEmployeeEligible* lub *sp_CalculateOrderDiscount*. Aby użyć tego łącznika jako akcji są wymagane prawidłowych tabel i/lub procedury składowane.
Dostępne kwerendy danych | Obsługa wyzwalacza | Instrukcję SQL, aby ustalić, czy wszystkie dane jest dostępna dla sondowanie tabeli bazy danych programu SQL Server. To powinna zwrócić wartość liczbową odpowiadającą liczbie wierszy danych dostępne. Przykład: SELECT COUNT(*) z nazwa_tabeli.
Kwerendy danych ankiety | Obsługa wyzwalacza | Instrukcja SQL, aby skorzystać z tabeli bazy danych programu SQL Server. Można wprowadzić dowolną liczbę instrukcji SQL, rozdzielając je średnikami. Tej instrukcji jest wykonywane transakcyjnie i tylko projekt zatwierdzony podczas bezpiecznie przechowywania danych w aplikacji logicznych. Przykład: Wybierz *z nazwa_tabeli; Usuwanie z nazwa_tabeli. <br/>**Note**<br/>Należy podać instrukcję ankiety, która pozwala uniknąć nieskończonej pętli, usuwając, przenosząc lub aktualizowanie zaznaczonych danych, aby upewnić się, że te same dane nie jest wysyłane ponownie.

5. Po zakończeniu ustawienia pakietu wyglądać podobnie do następującej:  
![][1]  

6. Wybierz polecenie **Utwórz**. 


## <a name="use-the-connector-as-a-trigger"></a>Używanie łącznika jako wyzwalacz
Przyjrzyjmy się aplikację prostych wyrażeń logicznych, która sprawdza dane z tabeli SQL, doda te dane w innej tabeli i aktualizuje dane.

Aby użyć łącznik SQL jako wyzwalacz, wprowadź wartości **Dostępne kwerendy danych** i **Ankiety kwerendy danych** . **Zapytanie dostępne danych** jest wykonywane zgodnie z harmonogramem, wprowadź i określa, czy wszystkie dane jest niedostępna. Ponieważ ta kwerenda zwraca tylko liczbę skalarne, można dostosowanych i zoptymalizowana pod kątem Wykonywanie częstych.

**Kwerendy danych ankiety** tylko jest wykonywane po kwerendę dostępna danych wskazuje, że dane są dostępne. Ta instrukcja wykonuje w obrębie transakcji i jest tylko projekt zatwierdzony podczas wyodrębnionej dane są trwale przechowywane w przepływie pracy. Należy unikać nieskończenie ponownie wyodrębnianie tych samych danych. Transakcji rodzaj to wykonanie można usunąć lub aktualizowanie danych, aby upewnić się, że nie jest zebrane przy następnym otrzymuje danych.

> [AZURE.NOTE] Schemat zwrócony przez ten instrukcję identyfikuje dostępne właściwości w wybrany łącznik. Musi mieć nazwę wszystkich kolumn.

#### <a name="data-available-query-example"></a>Przykład dostępne kwerendy danych

    SELECT COUNT(*) FROM [Order] WHERE OrderStatus = 'ProcessedForCollection'

#### <a name="poll-data-query-example"></a>Przykład zapytania danych ankietę

    SELECT *, GetData() as 'PollTime' FROM [Order]
        WHERE OrderStatus = 'ProcessedForCollection'
        ORDER BY Id DESC;
    UPDATE [Order] SET OrderStatus = 'ProcessedForFrontDesk'
        WHERE Id =
        (SELECT Id FROM [Order] WHERE OrderStatus = 'ProcessedForCollection' ORDER BY Id DESC)

### <a name="add-the-trigger"></a>Dodawanie wyzwalacza
1. Podczas tworzenia lub edytowania aplikacji logiczny, zaznacz łącznik SQL została utworzona jako wyzwalacz. Ta lista zawiera dostępne wyzwalacze: **Ankieta danych (JSON)** i **Dane ankiety (XML)**:  
![][5]

2. Wybierz wyzwalacz **Ankiety danych (JSON)** , wprowadź częstotliwość, a następnie kliknij przycisk ✓:  
![][6]

3. Wyzwalacz pojawi się zgodnie z konfiguracją w aplikacji logicznych. Output(s) wyzwalacza są wyświetlane i mogą być używane jako wartości wejściowych w każdej kolejnej akcji:  
![][7]

## <a name="use-the-connector-as-an-action"></a>Używanie łącznika jako akcji
Przy użyciu naszych scenariusza aplikacji prostych wyrażeń logicznych, która sprawdza dane z tabeli SQL doda te dane w innej tabeli i aktualizuje dane.

Aby użyć łącznika SQL jako akcji, wprowadź nazwę tabele i/lub procedury składowane wprowadzone po utworzeniu łącznika SQL:

1. Po wyzwalacz (lub wybierz pozycję "ręcznie uruchom tę logikę"), dodać łącznik SQL utworzone z galerii. Wybierz jedną z operacji, wstawianie, takich jak *Wstawić do TempEmployeeDetails (JSON)*:  
![][8]

2. Wprowadź wartości wejściowych rekord, który ma być wstawiony, a następnie kliknij polecenie ✓:  
![][9]

3. W galerii Wybierz utworzony łącznik SQL. Jako akcji wybierz akcję aktualizacja dla tej samej tabeli, takich jak *EmployeeDetails aktualizacji*:  
![][11]

4. Wprowadzanie wartości wejściowych dla akcję aktualizacji, a następnie kliknij polecenie ✓:  
![][12]

Istnieje możliwość przetestowania aplikacji logika przez dodanie nowego rekordu w tabeli, co to są wysyłane.

## <a name="what-you-can-and-cannot-do"></a>Tym, co można, a czego nie można zrobić

Kwerenda SQL | Obsługiwane | Brak obsługi
--- | --- | ---
Miejsce, w którym klauzula | <ul><li>Operatory: A, lub, = <> <, < =, >, > = a, takich jak</li><li>Wiele warunków sub można łączyć przy "(" i")"</li><li>Ciąg literałów, daty i godziny (ujęty w cudzysłów pojedynczy) liczb (powinna zawierać tylko cyfry)</li><li>Ściśle należy w formacie binarnym wyrażenie, takich jak ((operand operator operand) i/lub (operand operator operandu)) *</li></ul> | <ul><li>Operatory: Między w</li><li>Funkcji wbudowanych, takich jak ADD(), teraz() MAX(), POWER() i tak dalej</li><li>Operatory matematyczne, takie jak *, -, + i tak dalej</li><li>Za pomocą łączenia ciągu +.</li><li>Wszystkie sprzężenia</li><li>MA wartość NULL, a nie zawiera wartości Null</li><li>Wszelkie numery z wartością liczbową znaki, takie jak numery liczbę szesnastkową</li></ul>
Pola (kwerenda wybierająca) | <ul><li>Nazwy kolumn prawidłowy, rozdzielając je przecinkami. Bez prefiksów Nazwa tabeli może (działa łącznika na jedną tabelę naraz).</li><li>Nazwy można wstawić odwróconą "[" i "]"</li></ul> | <ul><li>Słowa kluczowe, takie jak GÓRNEJ, DISTINCT i tak dalej</li><li>Wygładzanie, takich jak ulicy, miasta + adres pocztowy jako</li><li>Wszystkie wbudowanych funkcji, takich jak ADD(), teraz() MAX(), POWER() i tak dalej</li><li>Operatory matematyczne, takie jak *, -, + i tak dalej</li><li>Za pomocą łączenia ciągu +</li></ul>

#### <a name="tips"></a>Porady

- Dla zapytania zaawansowane zaleca się tworzenie procedury składowanej i wykonywanie przy użyciu procedury wykonywania przechowywane interfejsu API.
- Używając kwerendy wewnętrzne, należy użyć ich w ramach procedur składowanych.
- W przypadku dołączania wielu warunków, można użyć "AND" i "Lub" operatorów.

## <a name="hybrid-configuration-optional"></a>Konfigurowanie środowiska hybrydowego (opcjonalnie)

> [AZURE.NOTE] Ten krok jest wymagany tylko wtedy, gdy korzystasz z programu SQL Server lokalnego za zaporą.

Usługa aplikacji używa Menedżer konfiguracji hybrydowych do bezpieczne łączenie się z systemem lokalnego. W przypadku zastosowania łącznik lokalnego serwera SQL, Menedżer połączeń hybrydowego jest wymagane.

> [AZURE.NOTE] Jeśli chcesz rozpocząć pracę z Azure logiki aplikacji przed utworzeniem konta dla konta Azure, przejdź do [Spróbuj logiki aplikacji](https://tryappservice.azure.com/?appservice=logic), miejsce, w którym możesz od razu utworzyć krótkotrwałe starter logiczny aplikację w aplikacji usługi. Nie kart kredytowych wymagane; nie zobowiązania.

Zobacz [Korzystanie z Menedżera połączeń hybrydowych](app-service-logic-hybrid-connection-manager.md).


## <a name="do-more-with-your-connector"></a>Więcej możliwości korzystania z tego łącznika
Teraz, gdy łącznik jest tworzony, możesz dodać go do przepływu pracy firm przy użyciu aplikacji logicznych. Zobacz [Co to są aplikacje logiczny?](app-service-logic-what-are-logic-apps.md).

Wyświetlanie odwołania Swagger interfejsu API usługi REST [łączników](http://go.microsoft.com/fwlink/p/?LinkId=529766)i interfejsu API aplikacje odwołania.

Można także przeglądać wydajności statystyki i kontroli zabezpieczeń do łącznika. Zobacz [Monitorowanie wbudowanych łączników i aplikacjami interfejsu API i zarządzanie nimi](app-service-logic-monitor-your-connectors.md).


<!--Image references-->
[1]: ./media/app-service-logic-connector-sql/Create.png
[5]: ./media/app-service-logic-connector-sql/LogicApp1.png
[6]: ./media/app-service-logic-connector-sql/LogicApp2.png
[7]: ./media/app-service-logic-connector-sql/LogicApp3.png
[8]: ./media/app-service-logic-connector-sql/LogicApp4.png
[9]: ./media/app-service-logic-connector-sql/LogicApp5.png
[10]: ./media/app-service-logic-connector-sql/LogicApp6.png
[11]: ./media/app-service-logic-connector-sql/LogicApp7.png
[12]: ./media/app-service-logic-connector-sql/LogicApp8.png
