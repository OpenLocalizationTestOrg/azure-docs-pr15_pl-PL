<properties
    pageTitle="Praca z biblioteką zarządzanego klienta Mobile usługi aplikacji (Windows | Xamarin) | Microsoft Azure"
    description="Dowiedz się, jak korzystać z klienta programu .NET dla aplikacji Mobile usługi Azure aplikacji z aplikacji systemu Windows i Xamarin."
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="how-to-use-the-managed-client-for-azure-mobile-apps"></a>Jak w przypadku aplikacji Mobile Azure za pomocą klienta usługi zarządzanych

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

##<a name="overview"></a>Omówienie

Ten przewodnik zawiera się, jak wykonywać typowe scenariusze korzystania z biblioteki zarządzanych klientów programu Azure aplikacji usługi Mobile aplikacje dla systemu Windows i aplikacji Xamarin. Jeśli jesteś nowym użytkownikiem aplikacji Mobile, warto najpierw wykonywania [aplikacji Mobile Azure Szybki Start] [ 1] samouczka. W tym przewodniku możemy skupić się na SDK zarządzanych po stronie klienta. Aby dowiedzieć się więcej o SDK po stronie serwera dla aplikacji Mobile, zapoznaj się z dokumentacją dla [.NET Server SDK] [ 2] lub [Node.js Server SDK][3].

## <a name="reference-documentation"></a>Dokumentacji

Dokumentacji dla klienta SDK znajduje się tutaj: [odwołanie klienta Azure Mobile aplikacji .NET][4].
Możesz również znaleźć kilka przykładów klienta w [repozytorium Azure próbki GitHub][5].

## <a name="supported-platforms"></a>Platformy obsługiwane

Platformy .NET obsługuje następujących platformach:

* Xamarin Android udostępniono 19 interfejsu API do 24 (KitKat przez nugacie)
* Xamarin iOS udostępniono iOS wersji 8.0 lub nowszy
* Platformy Windows uniwersalny
* Windows Phone 8.1
* Windows Phone 8.0 z wyjątkiem aplikacji Silverlight

Uwierzytelnianie "serwer przepływu" za pomocą widoku sieci Web prezentowanych interfejsu użytkownika.  Jeśli urządzenie nie jest możliwe przedstawienie interfejsu użytkownika widoku sieci Web, potrzebne są inne metody uwierzytelniania.  Zestaw SDK nie jest więc odpowiedni typ czujki, lub podobnie ograniczeniami urządzeń.

##<a name="setup"></a>Instalacja i wymagania wstępne

Przyjęto założenie, że już utworzone i opublikowane projektu wewnętrznej bazy danych aplikacji Mobile, który zawiera co najmniej jedną tabelę.  W kodzie używane w tym temacie tabeli o nazwie `TodoItem` i ma następujące kolumny: `Id`, `Text`, i `Complete`. Ta tabela jest tę samą tabelę utworzonych po zakończeniu [Szybki Start aplikacji Mobile Azure].

Odpowiadający typ klienta wpisany w języku C# jest następujące klasy:

    public class TodoItem
    {
        public string Id { get; set; }

        [JsonProperty(PropertyName = "text")]
        public string Text { get; set; }

        [JsonProperty(PropertyName = "complete")]
        public bool Complete { get; set; }
    }

[JsonPropertyAttribute] [ 6] służy do definiowania mapowania *PropertyName* typ klienta i w tabeli.

Aby dowiedzieć się, jak tworzyć tabele w swojej aplikacji Mobile wewnętrznej bazy danych, zobacz informacje przedstawione w [temacie .NET Server SDK] [ 7] lub [tematu Node.js Server SDK][8]. Jeśli utworzono do wewnętrznej bazy danych aplikacji Mobile w portalu Azure za pomocą Szybki Start, można także użyć ustawienie **łatwe tabel** w [Azure portal].

###<a name="how-to-install-the-managed-client-sdk-package"></a>Jak: zainstalować pakiet SDK zarządzanego klienta

Użyj jednej z następujących metod do zainstalowania pakietu SDK zarządzanego klienta dla aplikacji Mobile z [NuGet][9]:

+ **Programu Visual Studio** Kliknij prawym przyciskiem myszy projektu, kliknij pozycję **Zarządzaj pakiety NuGet**, wyszukaj `Microsoft.Azure.Mobile.Client` pakietu, a następnie kliknij przycisk **Zainstaluj**.

+ **Xamarin Studio** Kliknij prawym przyciskiem myszy projektu, kliknij przycisk **Dodaj** > **Dodawanie pakietów NuGet**, wyszukaj `Microsoft.Azure.Mobile.Client `pakietu, a następnie kliknij przycisk **Dodaj pakiet**.

W pliku głównego działalność Pamiętaj, aby dodać następującą instrukcję **przy użyciu** :

    using Microsoft.WindowsAzure.MobileServices;

###<a name="symbolsource"></a>Jak: Praca z symboli debugowania w programie Visual Studio

Symbole nazw Microsoft.Azure.Mobile są dostępne na [SymbolSource][10].  Zapoznaj się z [instrukcjami SymbolSource] [ 11] do integracji z programem Visual Studio SymbolSource.

##<a name="create-client"></a>Tworzenie aplikacji Mobile klienta

Poniższy kod tworzy [MobileServiceClient] [ 12] obiekt, który umożliwia dostęp do wewnętrznej bazy danych aplikacji Mobile.

    var client = new MobileServiceClient("MOBILE_APP_URL");

W poprzednim kodzie Zamień `MOBILE_APP_URL` z adresem URL aplikacji Mobile wewnętrznej bazy danych, która znajduje się w karta dla wewnętrznej bazy danych usługi aplikacji Mobile w [Azure portal]. Obiekt MobileServiceClient powinny być pojedyncza.

## <a name="work-with-tables"></a>Praca z tabelami

Poniżej szczegółowo opisano sposoby wyszukiwania i pobieranie rekordów i modyfikowania danych w tabeli.  Omówione są następujące tematy:

* [Tworzenie odwołania do tabeli](#instantiating)
* [Dane kwerendy](#querying)
* [Filtrowanie danych zwróconych](#filtering)
* [Sortowanie zwrócił dane.](#sorting)
* [Zwróć dane na stronach](#paging)
* [Wybierz określone kolumny](#selecting)
* [Wyszukiwanie rekordów za pomocą identyfikatora](#lookingup)
* [Praca z kwerendami bez typu](#untypedqueries)
* [Wstawianie danych](#inserting)
* [Aktualizowanie danych](#updating)
* [Usuwanie danych](#deleting)
* [Rozwiązywanie konfliktów i współbieżności Optymistyczny](#optimisticconcurrency)
* [Wiązanie z interfejsu użytkownika systemu Windows](#binding)
* [Zmienianie rozmiaru strony](#pagesize)

###<a name="instantiating"></a>Jak: Tworzenie odwołania do tabeli

Kod, uzyskuje dostęp do lub modyfikujące danych w tabeli wewnętrznej bazy danych połączeń funkcji na `MobileServiceTable` obiektu. Aby uzyskać odwołanie do tabeli, wywołującego metodę [Metoda GetTable] w następujący sposób:

    IMobileServiceTable<TodoItem> todoTable = client.GetTable<TodoItem>();

Zwracany obiekt używa modelu wpisany szeregowania. Obsługiwane jest też model szeregowania bez typu. Następujący przykład [tworzy odwołanie do tabeli bez typu]:

    // Get an untyped table reference
    IMobileServiceTable untypedTodoTable = client.GetTable("TodoItem");

W kwerendach bez typu musisz określić ciągu kwerendy źródłowej OData.

###<a name="querying"></a>Jak: kwerendy dane z Twojej aplikacji Mobile

W tej sekcji opisano sposób wysyłania kwerend do aplikacji Mobile wewnętrznej bazy danych, która zawiera następujące funkcje:

- [Filtrowanie danych zwróconych](#filtering)
- [Sortowanie zwrócił dane.](#sorting)
- [Zwróć dane na stronach](#paging)
- [Wybierz określone kolumny](#selecting)
- [Wyszukiwanie danych według identyfikatorów](#lookingup)

>[AZURE.NOTE]Aby zablokować wszystkie wiersze z zwracanych wymuszane jest rozmiar strony opartych na serwerze.  Stronicowanie zachowuje domyślne żądań dużych zestawów danych z negatywny wpływ na usługę.  Aby zwrócić więcej niż 50 wierszy, użyj `Skip` i `Take` metody, zgodnie z opisem w [zwracanych danych na stronach].

###<a name="filtering"></a>Jak: Filtr zwrócone dane

Poniższy kod przedstawia sposób do filtrowania danych według tym `Where` klauzulę w kwerendzie. Zwraca wszystkie elementy na `todoTable` którego `Complete` właściwość jest równa `false`. Funkcja [Jeżeli] dotyczy wiersza filtrowania predykat do kwerendy w odniesieniu do tabeli.

    // This query filters out completed TodoItems and items without a timestamp.
    List<TodoItem> items = await todoTable
       .Where(todoItem => todoItem.Complete == false)
       .ToListAsync();

Możesz wyświetlić identyfikator URI żądania wysłanego do wewnętrznej bazy danych za pomocą oprogramowania inspekcji wiadomości, takie jak narzędzi dla deweloperów przeglądarki lub [Fiddler]. Jeśli możesz obejrzeć identyfikator URI żądania, zwróć uwagę, modyfikacji ciągu kwerendy:

    GET /tables/todoitem?$filter=(complete+eq+false) HTTP/1.1

To żądanie OData przekształcić zapytania SQL przez zestaw SDK serwera:

    SELECT *
    FROM TodoItem
    WHERE ISNULL(complete, 0) = 0

Co wartość przekazywana do funkcji `Where` metoda może mieć dowolnej liczby warunków.

    // This query filters out completed TodoItems where Text isn't null
    List<TodoItem> items = await todoTable
       .Where(todoItem => todoItem.Complete == false && todoItem.Text != null)
       .ToListAsync();

W tym przykładzie czy można przekształcić w zapytania SQL przez zestaw SDK serwera:

    SELECT *
    FROM TodoItem
    WHERE ISNULL(complete, 0) = 0
          AND ISNULL(text, 0) = 0

Ta kwerenda również można podzielić na wiele klauzul:

    List<TodoItem> items = await todoTable
       .Where(todoItem => todoItem.Complete == false)
       .Where(todoItem => todoItem.Text != null)
       .ToListAsync();

Dwie metody są równoważne i mogą być używane zamiennie.  Opcja byłego&mdash;z łączenia wielu orzeczenia w jednej kwerendzie&mdash;jest bardziej zwarty i zalecane.

`Where` Klauzula obsługuje operacji, które można przekształcić w podzbiór OData. Operacje to między innymi:

* Operatory (==,! =, <, <, >, > =),
* Operatory arytmetyczne (+, -, /, *, %),
* Liczba dokładności (Math.Floor, Math.Ceiling)
* Funkcje tekstowe (długość, podciąg, Zamień, IndexOf, StartsWith, EndsWith)
* Właściwości daty (roku, miesiąca, dnia, godziny, minuty i sekundy),
* Dostęp do właściwości obiektu, a
* Żadnej z tych operacji łączenia wyrażeń.

Zastanawiając obsługuje SDK serwera, można rozważyć [OData v3 dokumentacji].

###<a name="sorting"></a>Jak: sortowanie zwrócone dane

Poniższy kod przedstawia sposób sortowania danych w kwerendzie w tym funkcję [OrderBy] lub [OrderByDescending] . Zwraca elementy z `todoTable` posortowane rosnąco według `Text` pola.

    // Sort items in ascending order by Text field
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .OrderBy(todoItem => todoItem.Text)
    List<TodoItem> items = await query.ToListAsync();

    // Sort items in descending order by Text field
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .OrderByDescending(todoItem => todoItem.Text)
    List<TodoItem> items = await query.ToListAsync();

###<a name="paging"></a>Jak: zwracają dane na stronach

Domyślnie wewnętrznej bazy danych zwraca tylko pierwszych 50 wierszy. Liczba zwracanych wierszy można zwiększyć przez wywołanie metody [potrwać] . Używanie `Take` wraz z metody [Pomiń] , aby zażądać od konkretnej "Strona" Suma zestawu danych zwróconych przez kwerendę. Poniższe zapytanie, podczas wykonywania zwraca pierwsze trzy elementów w tabeli.

    // Define a filtered query that returns the top 3 items.
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Take(3);
    List<TodoItem> items = await query.ToListAsync();

Poniższe zapytanie poprawiony pomija pierwsze trzy wyniki i zwraca wynik kolejne trzy. Ta kwerenda tworzy drugą "strony" danych, której rozmiar strony jest trzy elementy.

    // Define a filtered query that skips the top 3 items and returns the next 3 items.
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Skip(3)
                    .Take(3);
    List<TodoItem> items = await query.ToListAsync();

Metoda [IncludeTotalCount] żąda łączną liczbę elementów dla _wszystkich_ rekordów, które chcesz zwrócone zostały ignoruje klauzuli stronicowania limit określony:

    query = query.IncludeTotalCount();

W aplikacji rzeczywistych można użyć kwerendy, podobnie jak w powyższym przykładzie z formantu pager lub porównywalna interfejsu użytkownika przechodzenie między stronami.

>[AZURE.NOTE]Aby zastąpić limit 50 wierszy w aplikacji Mobile wewnętrznej bazie danych, możesz również dotyczą [EnableQueryAttribute] publicznej metody GET i określanie zachowania nawigacji między stronami. Po zastosowaniu metody następujące Ustawia maksymalną zwrócone wiersze do 1000:
>
>    [EnableQuery(MaxTop=1000)]

### <a name="selecting"></a>Jak: wybierz określone kolumny

Można określić, aby uwzględnić w wynikach, dodając do kwerendy klauzulę [Wybierz] zestaw właściwości. Na przykład poniższy kod przedstawia, jak zaznaczyć tylko jednego pola, a także jak zaznaczanie i formatowanie wielu pól:

    // Select one field -- just the Text
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Select(todoItem => todoItem.Text);
    List<string> items = await query.ToListAsync();

    // Select multiple fields -- both Complete and Text info
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Select(todoItem => string.Format("{0} -- {1}",
                        todoItem.Text.PadRight(30), todoItem.Complete ?
                        "Now complete!" : "Incomplete!"));
    List<string> items = await query.ToListAsync();

Wszystkie funkcje opisane dotąd są dodatku, więc możemy można zachować łączenia ich. Każdy łańcuchowej połączenia ma wpływ na więcej kwerendy. Przykład więcej:

    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Where(todoItem => todoItem.Complete == false)
                    .Select(todoItem => todoItem.Text)
                    .Skip(3).
                    .Take(3);
    List<string> items = await query.ToListAsync();

### <a name="lookingup"></a>Jak: wyszukiwanie danych według identyfikatorów

Funkcja [LookupAsync] może służyć do wyszukiwania obiektów z bazy danych za pomocą określonego identyfikatora.

    // This query filters out the item with the ID of 37BBF396-11F0-4B39-85C8-B319C729AF6D
    TodoItem item = await todoTable.LookupAsync("37BBF396-11F0-4B39-85C8-B319C729AF6D");

### <a name="untypedqueries"></a>Jak: wykonywać bez typu kwerendy

Wykonywanie kwerendy przy użyciu tabeli bez typu obiektu, należy jawnie określić ciągu kwerendy OData, dzwoniąc [ReadAsync], tak jak w poniższym przykładzie:

    // Lookup untyped data using OData
    JToken untypedItems = await untypedTodoTable.ReadAsync("$filter=complete eq 0&$orderby=text");

Można odzyskać JSON wartości, których można używać jak zbiór właściwości. Aby uzyskać więcej informacji na JToken i Newtonsoft Json.NET odwiedź witrynę [Json.NET] .

### <a name="inserting"></a>Jak: wstawianie dane do wewnętrznej bazy danych aplikacji Mobile

Wszystkie typy klienta musi zawierać element o nazwie **Id**, który jest domyślnie ciągu. Ten **identyfikator** jest wymagane do wykonywania operacji OBSŁUGIWAŁ i synchronizacja w trybie offline. Poniższy kod przedstawia sposób metoda [InsertAsync] umożliwia wstawianie nowych wierszy do tabeli. Parametr zawiera dane, które ma zostać wstawiony jako obiekt .NET.

    await todoTable.InsertAsync(todoItem);

Jeśli wartość Unikatowy identyfikator niestandardową jest niedostępna w `todoItem` podczas wstawiania identyfikator GUID jest generowany przez serwer.
Aby pobrać wygenerowanego Id, sprawdzanie obiekt, po powrocie połączenie.

Aby wstawić bez typu danych, może korzystać z Json.NET:

    JObject jo = new JObject();
    jo.Add("Text", "Hello World");
    jo.Add("Complete", false);
    var inserted = await table.InsertAsync(jo);

Oto przykład przy użyciu adresu e-mail jako identyfikator unikatowy ciąg:

    JObject jo = new JObject();
    jo.Add("id", "myemail@emaildomain.com");
    jo.Add("Text", "Hello World");
    jo.Add("Complete", false);
    var inserted = await table.InsertAsync(jo);

### <a name="working-with-id-values"></a>Praca z wartości identyfikatorów

Aplikacje Mobile obsługuje wartości unikatowe niestandardowy ciąg dla kolumny **identyfikator** tabeli. Wartość ciągu umożliwia aplikacjom używać niestandardowych wartości, takie jak adresy e-mail lub nazwy użytkowników dla identyfikatora.  Identyfikatory ciąg udostępnia następujące korzyści:

* Identyfikatory są generowane bez wprowadzania podróż do bazy danych.
* Rekordy są łatwiejsze do scalenia z różnych tabel lub baz danych.
* Wartości identyfikatorów można lepiej zintegrować z logiki aplikacji.

Wartość Identyfikatora ciągu nie jest ustawiona na wstawiony rekord, wewnętrznej bazy danych aplikacji Mobile generuje unikatowe wartości dla identyfikatora. Metoda [Guid.NewGuid] służy do generowania własne wartości identyfikatorów, na komputerze klienckim lub w wewnętrznej bazy danych.

    JObject jo = new JObject();
    jo.Add("id", Guid.NewGuid().ToString("N"));

###<a name="modifying"></a>Jak: zmodyfikować dane w aplikacji Mobile wewnętrznej bazie danych

Poniższy kod ilustruje sposób użycia metody [UpdateAsync] zaktualizować istniejący rekord z tym samym Identyfikatorem nowymi informacjami. Parametr zawiera dane, które mają być aktualizowane jako obiekt .NET.

    await todoTable.UpdateAsync(todoItem);

Aby zaktualizować dane bez typu, użytkownik może skorzystać z [Json.NET] w następujący sposób:

    JObject jo = new JObject();
    jo.Add("id", "37BBF396-11F0-4B39-85C8-B319C729AF6D");
    jo.Add("Text", "Hello World");
    jo.Add("Complete", false);
    var inserted = await table.UpdateAsync(jo);

`id` Musi być podane pole, podczas tworzenia aktualizacji. Używa wewnętrznej bazy danych `id` pola do identyfikowania które wiersza do aktualizacji. `id` Pól można uzyskać od wynik `InsertAsync` połączeń. `ArgumentException` Jest uruchamiany, jeśli podczas próby aktualizacji elementu bez `id` wartość.

###<a name="deleting"></a>Jak: usuwanie danych w aplikacji Mobile wewnętrznej bazie danych

Poniższy kod przedstawia sposób użycia metody [DeleteAsync] , aby usunąć wystąpienie istniejącego. Identyfikuje wystąpienie `id` pól zestawu na `todoItem`.

    await todoTable.DeleteAsync(todoItem);

Aby usunąć dane bez typu, użytkownik może skorzystać z Json.NET w następujący sposób:

    JObject jo = new JObject();
    jo.Add("id", "37BBF396-11F0-4B39-85C8-B319C729AF6D");
    await table.DeleteAsync(jo);

Po wprowadzeniu żądanie usunięcia należy określić identyfikator. Inne właściwości, nie są one przekazywane do usługi lub nie są uwzględniane w punkcie usług. Wynik `DeleteAsync` połączenia jest zazwyczaj `null`. Identyfikator, aby przekazać można uzyskać od wynik `InsertAsync` połączeń. A `MobileServiceInvalidOperationException` jest generowany podczas próby usunięcia elementu bez określania `id` pola.

###<a name="optimisticconcurrency"></a>Jak: używanie optymistyczny współbieżności dla Rozwiązywanie konfliktów

Dwie lub więcej klientami może tworzyć zmiany tego samego elementu w tym samym czasie. Bez wykrywanie konfliktów ostatniego zapisu może zastępować wszystkie poprzednie aktualizacje. **Opis pliku pomocy** założono, że każdej transakcji zatwierdzić i dlatego nie korzysta z dowolnym blokowania zasobów.  Przed zatwierdzeniem transakcji, opis pliku pomocy sprawdza modyfikacji w żadnych innych transakcji danych. Jeśli dane zostały zmodyfikowane, przekazywanie transakcja jest wycofana.

Aplikacje Mobile obsługuje opis pliku pomocy przez funkcję śledzenia zmian do korzystania z każdego elementu `version` system właściwości kolumny, którą zdefiniowano dla każdej tabeli w swojej aplikacji Mobile wewnętrznej bazy danych. Przy każdym zaktualizowaniu rekordu aplikacji Mobile ustawia `version` właściwości dla tego rekordu na nową wartość. Podczas każdego żądania aktualizacji `version` właściwości rekordu dołączony do żądania jest porównaniu do tej samej właściwości rekordu na serwerze. Jeśli wersja przekazywane z żądania nie ma odpowiednika wewnętrznej bazy danych, a następnie podnosi Biblioteka klienta `MobileServicePreconditionFailedException<T>` wyjątek. Typ włączone z wyjątkiem jest rekord z wewnętrznej bazy danych zawierającej wersji serwerów rekordu. Aplikacja następnie można zdecydować, czy wykonać żądania aktualizacji ponownie przy prawidłowej `version` wartość z wewnętrznej bazy danych wprowadzeniem zmian.

Definiowanie kolumny w klasie tabeli dla `version` właściwość system umożliwiający optymistyczny współbieżności. Na przykład:

    public class TodoItem
    {
        public string Id { get; set; }

        [JsonProperty(PropertyName = "text")]
        public string Text { get; set; }

        [JsonProperty(PropertyName = "complete")]
        public bool Complete { get; set; }

        // *** Enable Optimistic Concurrency *** //
        [JsonProperty(PropertyName = "version")]
        public string Version { set; get; }
    }


Aplikacje przy użyciu tabel bez typu włącza optymistyczny współbieżności przez ustawienie `Version` Flaga na `SystemProperties` tabeli w następujący sposób.

    //Enable optimistic concurrency by retrieving version
    todoTable.SystemProperties |= MobileServiceSystemProperties.Version;

Oprócz włączania współbieżności optymistycznych, możesz również efektywnej `MobileServicePreconditionFailedException<T>` wyjątek w kodzie podczas wywoływania [UpdateAsync].  Rozwiązywanie konfliktu przez zastosowanie odpowiedniego `version` zaktualizowany rekord i połączenia [UpdateAsync] z rekordem rozwiązany. Poniższy kod przedstawia sposób rozwiązać konflikt podczas zapisywania raz wykrywania:

    private async void UpdateToDoItem(TodoItem item)
    {
        MobileServicePreconditionFailedException<TodoItem> exception = null;

        try
        {
            //update at the remote table
            await todoTable.UpdateAsync(item);
        }
        catch (MobileServicePreconditionFailedException<TodoItem> writeException)
        {
            exception = writeException;
        }

        if (exception != null)
        {
            // Conflict detected, the item has changed since the last query
            // Resolve the conflict between the local and server item
            await ResolveConflict(item, exception.Item);
        }
    }


    private async Task ResolveConflict(TodoItem localItem, TodoItem serverItem)
    {
        //Ask user to choose the resoltion between versions
        MessageDialog msgDialog = new MessageDialog(
            String.Format("Server Text: \"{0}\" \nLocal Text: \"{1}\"\n",
            serverItem.Text, localItem.Text),
            "CONFLICT DETECTED - Select a resolution:");

        UICommand localBtn = new UICommand("Commit Local Text");
        UICommand ServerBtn = new UICommand("Leave Server Text");
        msgDialog.Commands.Add(localBtn);
        msgDialog.Commands.Add(ServerBtn);

        localBtn.Invoked = async (IUICommand command) =>
        {
            // To resolve the conflict, update the version of the item being committed. Otherwise, you will keep
            // catching a MobileServicePreConditionFailedException.
            localItem.Version = serverItem.Version;

            // Updating recursively here just in case another change happened while the user was making a decision
            UpdateToDoItem(localItem);
        };

        ServerBtn.Invoked = async (IUICommand command) =>
        {
            RefreshTodoItems();
        };

        await msgDialog.ShowAsync();
    }

Aby uzyskać więcej informacji zobacz temat [w trybie Offline synchronizacja danych w aplikacji Mobile Azure] .

###<a name="binding"></a>Jak: aplikacje Mobile wiązanie danych do interfejsu użytkownika systemu Windows

W tej sekcji przedstawiono sposób wyświetlania obiektów zwracanych danych przy użyciu elementów interfejsu użytkownika w aplikacji dla systemu Windows.  Poniższy przykład kodu wiąże się ze źródłem listy za pomocą kwerendy dla elementów niekompletna. [MobileServiceCollection] tworzy urządzeń przenośnych aplikacji obsługującej zbiór powiązanie.

    // This query filters out completed TodoItems.
    MobileServiceCollection<TodoItem, TodoItem> items = await todoTable
        .Where(todoItem => todoItem.Complete == false)
        .ToCollectionAsync();

    // itemsControl is an IEnumerable that could be bound to a UI list control
    IEnumerable itemsControl  = items;

    // Bind this to a ListBox
    ListBox lb = new ListBox();
    lb.ItemsSource = items;

Niektóre formanty w zarządzane środowisko uruchomieniowe obsługują interfejs o nazwie [ISupportIncrementalLoading]. Ten interfejs umożliwia na żądanie dodatkowych danych, gdy użytkownik przewija kontrolek. Ma wbudowanych funkcji dla tego interfejsu dla uniwersalny aplikacje systemu Windows za pośrednictwem [MobileServiceIncrementalLoadingCollection], automatycznie obsługujący połączenia z kontrolek. Używanie `MobileServiceIncrementalLoadingCollection` w aplikacjach systemu Windows w następujący sposób:

    MobileServiceIncrementalLoadingCollection<TodoItem,TodoItem> items;
    items = todoTable.Where(todoItem => todoItem.Complete == false).ToIncrementalLoadingCollection();

    ListBox lb = new ListBox();
    lb.ItemsSource = items;

Aby korzystać z nowej kolekcji na aplikacje systemu Windows Phone 8 i "Silverlight", użyj `ToCollection` metody rozszerzenia na `IMobileServiceTableQuery<T>` i `IMobileServiceTable<T>`. Aby załadować dane, zadzwoń do `LoadMoreItemsAsync()`.

    MobileServiceCollection<TodoItem, TodoItem> items = todoTable.Where(todoItem => todoItem.Complete==false).ToCollection();
    await items.LoadMoreItemsAsync();

Kiedy używać zbioru utworzone przez wywołanie `ToCollectionAsync` lub `ToCollection`, zostanie wyświetlony zbiór powiązanej z interfejsu użytkownika.  Ten zbiór jest pamiętać o nawigacji między stronami.  Ponieważ kolekcji ładowania danych z sieci, czasem ładowania zakończy się niepowodzeniem. Do obsługi tych błędów, zastępując `OnException` metoda `MobileServiceIncrementalLoadingCollection` obsługę wyjątki wynikające z połączenia do `LoadMoreItemsAsync`.

Należy rozważyć, jeśli tabela ma wiele pól, ale mają być wyświetlane niektóre z nich w kontrolę. Aby zaznaczyć określonych kolumn do wyświetlenia w interfejsie użytkownika można użyć porad w poprzedniej sekcji "[Wybierz określone kolumny](#selecting)".

###<a name="pagesize"></a>Zmienianie rozmiaru strony

Azure aplikacji Mobile zwraca więcej niż 50 elementów na żądanie domyślnie.  Można zmienić rozmiar stronicowanie, zwiększając maksymalny rozmiar strony na klienta i serwera.  Aby zwiększyć rozmiar żądanej strony, określ `PullOptions` podczas korzystania z `PullAsync()`:

    PullOptions pullOptions = new PullOptions
        {
            MaxPageSize = 100
        };

Przy założeniu wprowadzone `PageSize` równa lub większa niż 100 na serwerze, żądanie zwraca do 100 elementów.

##<a name="#offlinesync"></a>Praca z tabelami w trybie Offline

Tabele w trybie offline za pomocą lokalnych danych sklepu SQLite do użytku w trybie offline.  Wszystkie tabelę, którą operacji są wykonywane przed lokalnym SQLite zawierają zamiast w sklepie serwerem zdalnym.  Aby utworzyć tabelę trybu offline, najpierw należy przygotować projektu:

1. W programie Visual Studio, kliknij prawym przyciskiem myszy rozwiązanie > **Zarządzanie pakietów NuGet rozwiązania...**, a następnie wyszukaj i zainstaluj pakiet NuGet **Microsoft.Azure.Mobile.Client.SQLiteStore** dla wszystkich projektów w rozwiązaniu.

2. (Opcjonalnie) Do obsługi urządzeń z systemem Windows, zainstaluj jedną z następujących pakietach środowisko uruchomieniowe SQLite:

    * **Obsługi Windows 8.1:** Instalowanie [SQLite dla systemu Windows 8.1][3].
    * **Windows Phone 8.1:** Instalowanie [SQLite dla Windows Phone 8.1][4].
    * **Platformy Windows uniwersalny** Instalowanie [SQLite dla uniwersalny uniwersalny Windows][5].

3. (Opcjonalnie). Dla urządzeń z systemem Windows, kliknij pozycję **odwołania** > **Dodaj odwołanie**, rozwiń **Windows** folder > **rozszerzenia**, a następnie włącz odpowiednie **SQLite dla systemu Windows** SDK wraz z **Visual C++ 2013 Runtime for Windows** SDK.
    Nazwy SQLite SDK się nieco różnić z każdego platformy systemu Windows.

Przed utworzeniem odwołania do tabeli, należy przygotować magazynu lokalnego:

    var store = new MobileServiceSQLiteStore(Constants.OfflineDbPath);
    store.DefineTable<TodoItem>();

    //Initializes the SyncContext using the default IMobileServiceSyncHandler.
    await this.client.SyncContext.InitializeAsync(store);

Inicjowanie magazynu normalnie odbywa się natychmiast po utworzeniu klienta.  **OfflineDbPath** powinny być filename nadaje się do użytku na wszystkich platformach, które obsługujesz.  Jeśli ścieżka jest w pełni kwalifikowana (czyli zaczyna się od kreski ułamkowej), to zostanie użyta.  Jeśli ścieżka nie jest w pełni kwalifikowane, plik zostanie umieszczony w lokalizacji specyficzny dla platformy.

* Dla systemów iOS i urządzeń z systemem Android domyślna ścieżka to folder "Pliki osobiste".
* W przypadku urządzeń z systemem Windows domyślna ścieżka to folder "AppData" specyficzne dla aplikacji.

Odwołanie do tabeli można uzyskać za pomocą `GetSyncTable<>` metody:

    var table = client.GetSyncTable<TodoItem>();

Niepotrzebne do uwierzytelniania za pomocą tabeli trybu offline.  Wystarczy uwierzytelnienia komunikowania się z usługą wewnętrznej bazy danych.

###<a name="syncoffline"></a>Synchronizowanie tabeli trybu Offline

Tabele w trybie offline nie są synchronizowane z wewnętrznej bazy danych domyślnie.  Synchronizacja jest podzielony na dwie części.  Przekazać zmiany oddzielnie pobieranie nowych elementów.  Poniżej przedstawiono metodę typowe synchronizacji:

    public async Task SyncAsync()
    {
        ReadOnlyCollection<MobileServiceTableOperationError> syncErrors = null;

        try
        {
            await this.client.SyncContext.PushAsync();

            await this.todoTable.PullAsync(
                //The first parameter is a query name that is used internally by the client SDK to implement incremental sync.
                //Use a different query name for each unique query in your program
                "allTodoItems",
                this.todoTable.CreateQuery());
        }
        catch (MobileServicePushFailedException exc)
        {
            if (exc.PushResult != null)
            {
                syncErrors = exc.PushResult.Errors;
            }
        }

        // Simple error/conflict handling. A real application would handle the various errors like network conditions,
        // server conflicts and others via the IMobileServiceSyncHandler.
        if (syncErrors != null)
        {
            foreach (var error in syncErrors)
            {
                if (error.OperationKind == MobileServiceTableOperationKind.Update && error.Result != null)
                {
                    //Update failed, reverting to server's copy.
                    await error.CancelAndUpdateItemAsync(error.Result);
                }
                else
                {
                    // Discard local change.
                    await error.CancelAndDiscardItemAsync();
                }

                Debug.WriteLine(@"Error executing sync operation. Item: {0} ({1}). Operation discarded.", error.TableName, error.Item["id"]);
            }
        }
    }

Jeśli pierwszy argument `PullAsync` ma wartość null, a następnie synchronizacja przyrostowa nie jest używane.  Kolejne operacje synchronizacji pobiera wszystkie rekordy.

Zestaw SDK wykonuje niejawne `PushAsync()` przed pociągnięcie rekordów.

Obsługa konflikt się dzieje w `PullAsync()` metody.  Należy zająć się z konflikty w taki sam sposób jak tabele usługi online.  Powstaje konflikt podczas `PullAsync()` nosi nazwę zamiast podczas wstawiania, aktualizacji lub usunięcia. Jeśli wiele konfliktów przeprowadzona, są one połączone w pojedynczy MobileServicePushFailedException.  Uchwyt każdy błąd oddzielnie.

##<a name="#customapi"></a>Praca z niestandardowego interfejsu API

Interfejs API niestandardowe pozwala na zdefiniowanie niestandardowych punktów końcowych z użyciem funkcji serwera, które nie zamapować na wstawianie, aktualizowanie, usuwanie lub operacja odczytu. Za pomocą niestandardowych interfejsu API, możesz mieć większą kontrolę nad wiadomości, w tym do czytania i ustawianie nagłówków wiadomości HTTP oraz definiowania formatem treści wiadomości, oprócz JSON.

Interfejs API niestandardowe połączenia, dzwoniąc jedną z metod [InvokeApiAsync] na komputerze klienckim. Na przykład poniższy wiersz kodu przesyła żądanie POST do **completeAll** interfejsu API do wewnętrznej bazy danych:

    var result = await client.InvokeApiAsync<MarkAllResult>("completeAll", System.Net.Http.HttpMethod.Post, null);

Ten formularz połączeń wpisany metody i wymaga zdefiniowania zwracany typ **MarkAllResult** . Obsługiwane są zarówno wpisany i bez typu metody.

##<a name="authentication"></a>Uwierzytelniania użytkowników

Aplikacje Mobile obsługuje uwierzytelnianie i autoryzowanie użytkowników aplikacji przy użyciu różnych dostawcy tożsamości zewnętrzne: Facebook, Google Account Microsoft, Twitter i usługi Azure Active Directory. Można ustawić uprawnienia w tabelach w celu ograniczenia dostępu dla określonych operacje, aby tylko uwierzytelnieni użytkownicy. Za pomocą tożsamości uwierzytelnionych użytkowników do wykonania reguł autoryzacji w skrypty serwera. Aby uzyskać więcej informacji zobacz samouczek [uwierzytelniania Dodaj aplikację].

Obsługiwane są dwie przebiegu uwierzytelniania: _zarządzane przez klienta_ i _serwer zarządzany_ przepływ. Serwer zarządzany przepływu zapewnia najłatwiejszym obsługę uwierzytelniania, zależy od interfejs uwierzytelniania sieci web dostawcy. Przepływ zarządzane klienta umożliwia dla lepsza integracja z funkcje specyficzne dla urządzenia zależy od SDK specyficzne dla urządzenia specyficzne dla dostawcy.

>[AZURE.NOTE] Zalecamy korzystanie z przepływu zarządzane przez klienta w aplikacji produkcji.

Aby skonfigurować uwierzytelnianie, musisz zarejestrować aplikacji z jednego lub więcej dostawców tożsamości.  Dostawcy tożsamości generuje identyfikator klienta i hasła klienta dla aplikacji.  Te wartości są to ustaw w sieci wewnętrznej bazy danych umożliwiające Azure aplikacji usługi uwierzytelniania i autoryzacji.  Aby uzyskać więcej informacji postępuj zgodnie z instrukcjami szczegółowe samouczek [uwierzytelniania Dodaj aplikację].

W tej sekcji omówione są następujące tematy:

+ [Uwierzytelnianie zarządzane przez klienta](#clientflow)
+ [Serwer zarządzany uwierzytelniania](#serverflow)
+ [Buforowanie token uwierzytelniania](#caching)

###<a name="clientflow"></a>Uwierzytelnianie zarządzane przez klienta

Aplikacji można niezależne skontaktuj się z dostawcą tożsamości i podać zwrócona token podczas logowania przy użyciu sieci wewnętrznej bazy danych. Ten przepływ klienta umożliwia aby zapewnić użytkownikom jednego środowiska logowania jednokrotnego lub do pobierania danych użytkowników z dostawcy tożsamości. Uwierzytelnianie klienta na podstawie przepływu jest preferowana za pomocą przepływu serwera dostawcy tożsamości SDK zawiera więcej natywnych działanie interfejsu użytkownika i pozwala dostosowywania dodatkowe.

Przykłady są dostępne dla następujących wzorców uwierzytelniania przepływu klienta:

+ [Biblioteki uwierzytelniania usługi Active Directory](#adal)
+ [Facebook lub Google](#client-facebook)
+ [Live SDK](#client-livesdk)

#### <a name="adal"></a>Uwierzytelniania użytkowników z Active Directory Authentication Library

Za pomocą biblioteki uwierzytelniania Active Directory (ADAL) do uwierzytelniania użytkowników inicjowanie od klienta przy użyciu funkcji uwierzytelniania usługi Azure Active Directory.

1. Konfigurowanie sieci wewnętrznej bazy danych aplikacji dla urządzeń przenośnych dla AAD logowania jednokrotnego, wykonując samouczek [sposobu konfigurowania aplikacji usługi logowania usługi Active Directory] . Upewnij się ukończyć krok opcjonalny rejestrowania natywnej aplikacji.
2. W programie Visual Studio lub Xamarin Studio, otwórz projekt i Dodaj odwołanie do `Microsoft.IdentityModel.CLients.ActiveDirectory` NuGet pakiet. Podczas wyszukiwania, należy uwzględnić wersje wstępne.
3. Dodaj następujący kod do aplikacji, zgodnie z platformy, którego używasz. W każdej wprowadź następujące zamiany:

    * Zamień **Wstawianie urząd tutaj** nazwę dzierżawy, w którym możesz obsługi administracyjnej aplikacji. Format powinien być https://login.windows.net/contoso.onmicrosoft.com. Ta wartość można skopiować na karcie domeny w swojej usługi Azure Active Directory w [portal Azure klasyczny].
    * Zamień **Identyfikator tutaj, wstawianie ZASOBU w-** identyfikator klienta do wewnętrznej bazy danych aplikacji dla urządzeń przenośnych. Identyfikator klienta można uzyskać na karcie **Zaawansowane** w obszarze **Ustawienia usługi Azure Active Directory** w portalu.
    * Zamień **Identyfikator tutaj, wstawianie klienta w-** identyfikator klienta, skopiowane z klientami aplikacji.
    * Zamień **Wstawianie PRZEKIERUJ-URI-tutaj** witryny _/.auth/login/done_ punktu końcowego, przy użyciu schematu HTTPS. Ta wartość powinna być podobna do _https://contoso.azurewebsites.net/.auth/login/done_.

    Kod wymagane przez wszystkich platform następująco:

    **System Windows:**

        private MobileServiceUser user;
        private async Task AuthenticateAsync()
        {
            string authority = "INSERT-AUTHORITY-HERE";
            string resourceId = "INSERT-RESOURCE-ID-HERE";
            string clientId = "INSERT-CLIENT-ID-HERE";
            string redirectUri = "INSERT-REDIRECT-URI-HERE";
            while (user == null)
            {
                string message;
                try
                {
                    AuthenticationContext ac = new AuthenticationContext(authority);
                    AuthenticationResult ar = await ac.AcquireTokenAsync(resourceId, clientId,
                        new Uri(redirectUri), new PlatformParameters(PromptBehavior.Auto, false) );
                    JObject payload = new JObject();
                    payload["access_token"] = ar.AccessToken;
                    user = await App.MobileService.LoginAsync(
                        MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory, payload);
                    message = string.Format("You are now logged in - {0}", user.UserId);
                }
                catch (InvalidOperationException)
                {
                    message = "You must log in. Login Required";
                }
                var dialog = new MessageDialog(message);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }

    **Xamarin.iOS**

        private MobileServiceUser user;
        private async Task AuthenticateAsync(UIViewController view)
        {
            string authority = "INSERT-AUTHORITY-HERE";
            string resourceId = "INSERT-RESOURCE-ID-HERE";
            string clientId = "INSERT-CLIENT-ID-HERE";
            string redirectUri = "INSERT-REDIRECT-URI-HERE";
            try
            {
                AuthenticationContext ac = new AuthenticationContext(authority);
                AuthenticationResult ar = await ac.AcquireTokenAsync(resourceId, clientId,
                    new Uri(redirectUri), new PlatformParameters(view));
                JObject payload = new JObject();
                payload["access_token"] = ar.AccessToken;
                user = await client.LoginAsync(
                    MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory, payload);
            }
            catch (Exception ex)
            {
                Console.Error.WriteLine(@"ERROR - AUTHENTICATION FAILED {0}", ex.Message);
            }
        }

    **Xamarin.Android**

        private MobileServiceUser user;
        private async Task AuthenticateAsync()
        {
            string authority = "INSERT-AUTHORITY-HERE";
            string resourceId = "INSERT-RESOURCE-ID-HERE";
            string clientId = "INSERT-CLIENT-ID-HERE";
            string redirectUri = "INSERT-REDIRECT-URI-HERE";
            try
            {
                AuthenticationContext ac = new AuthenticationContext(authority);
                AuthenticationResult ar = await ac.AcquireTokenAsync(resourceId, clientId,
                    new Uri(redirectUri), new PlatformParameters(this));
                JObject payload = new JObject();
                payload["access_token"] = ar.AccessToken;
                user = await client.LoginAsync(
                    MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory, payload);
            }
            catch (Exception ex)
            {
                AlertDialog.Builder builder = new AlertDialog.Builder(this);
                builder.SetMessage(ex.Message);
                builder.SetTitle("You must log in. Login Required");
                builder.Create().Show();
            }
        }
        protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
        {
            base.OnActivityResult(requestCode, resultCode, data);
            AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);
        }

####<a name="client-facebook"></a>Rejestracji jednokrotnej przy użyciu tokenu z serwisu Facebook lub Google

Możesz użyć przepływu klienta, jak pokazano w tym wstawek w serwisie Facebook lub Google.

    var token = new JObject();
    // Replace access_token_value with actual value of your access token obtained
    // using the Facebook or Google SDK.
    token.Add("access_token", "access_token_value");

    private MobileServiceUser user;
    private async Task AuthenticateAsync()
    {
        while (user == null)
        {
            string message;
            try
            {
                // Change MobileServiceAuthenticationProvider.Facebook
                // to MobileServiceAuthenticationProvider.Google if using Google auth.
                user = await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook, token);
                message = string.Format("You are now logged in - {0}", user.UserId);
            }
            catch (InvalidOperationException)
            {
                message = "You must log in. Login Required";
            }

            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
    }

####<a name="client-livesdk"></a>Pojedynczy logowania przy użyciu programu Microsoft Account z Live SDK

Do uwierzytelniania użytkowników, musisz zarejestrować aplikacji na koncie Microsoft Centrum deweloperów. Konfigurowanie szczegółów rejestracji do wewnętrznej bazy danych aplikacji Mobile. Aby utworzyć rejestracja konta Microsoft i połączyć go z Twojej aplikacji Mobile wewnętrznej bazy danych, wykonaj czynności w [rejestrze aplikacji umożliwia logowania się do konta Microsoft]. Jeśli masz zarówno ze Sklepu Windows i Windows Phone 8-Silverlight wersji aplikacji, należy najpierw zarejestrować wersję ze Sklepu Windows.

Poniższy kod przy użyciu Live SDK i używa tokenu zwracane do logowania się do sieci wewnętrznej bazy danych aplikacji Mobile.

    private LiveConnectSession session;
    //private static string clientId = "<microsoft-account-client-id>";
    private async System.Threading.Tasks.Task AuthenticateAsync()
    {

        // Get the URL the Mobile App backend.
        var serviceUrl = App.MobileService.ApplicationUri.AbsoluteUri;

        // Create the authentication client for Windows Store using the service URL.
        LiveAuthClient liveIdClient = new LiveAuthClient(serviceUrl);
        //// Create the authentication client for Windows Phone using the client ID of the registration.
        //LiveAuthClient liveIdClient = new LiveAuthClient(clientId);

        while (session == null)
        {
            // Request the authentication token from the Live authentication service.
            // The wl.basic scope should always be requested.  Other scopes can be added
            LiveLoginResult result = await liveIdClient.LoginAsync(new string[] { "wl.basic" });
            if (result.Status == LiveConnectSessionStatus.Connected)
            {
                session = result.Session;

                // Get information about the logged-in user.
                LiveConnectClient client = new LiveConnectClient(session);
                LiveOperationResult meResult = await client.GetAsync("me");

                // Use the Microsoft account auth token to sign in to App Service.
                MobileServiceUser loginResult = await App.MobileService
                    .LoginWithMicrosoftAccountAsync(result.Session.AuthenticationToken);

                // Display a personalized sign-in greeting.
                string title = string.Format("Welcome {0}!", meResult.Result["first_name"]);
                var message = string.Format("You are now logged in - {0}", loginResult.UserId);
                var dialog = new MessageDialog(message, title);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
            else
            {
                session = null;
                var dialog = new MessageDialog("You must log in.", "Login Required");
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }
    }

Aby uzyskać więcej informacji zobacz dokumentację [Zestawu SDK systemu Windows Live] .

###<a name="serverflow"></a>Serwer zarządzany uwierzytelniania

Po zarejestrowaniu dostawcy tożsamości Wywołaj metodę [LoginAsync] na [MobileServiceClient] z wartością [MobileServiceAuthenticationProvider] dostawcy. Na przykład poniższy kod inicjuje serwera przepływu logowania przy użyciu Facebook.

    private MobileServiceUser user;
    private async System.Threading.Tasks.Task Authenticate()
    {
        while (user == null)
        {
            string message;
            try
            {
                user = await client
                    .LoginAsync(MobileServiceAuthenticationProvider.Facebook);
                message =
                    string.Format("You are now logged in - {0}", user.UserId);
            }
            catch (InvalidOperationException)
            {
                message = "You must log in. Login Required";
            }

            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
    }

Jeśli korzystasz z dostawcą tożsamości innych niż Facebook, zmień wartość [MobileServiceAuthenticationProvider] wartość dla swojego dostawcy.

W przepływie server Azure aplikacji usługi zarządza przepływ uwierzytelniania OAuth, wyświetlając na stronie logowania wybranego dostawcy.  Po zwraca dostawcy tożsamości, usługa Azure aplikacji generuje token uwierzytelniania aplikacji usługi. Metoda [LoginAsync] zwraca [MobileServiceUser], która udostępnia zarówno [identyfikator użytkownika] uwierzytelnionego użytkownika i [MobileServiceAuthenticationToken]jako token web JSON (JWT). Ten token można przechowywanych w pamięci podręcznej i ponownie wykorzystywać do momentu jej wygaśnięcia. Aby uzyskać więcej informacji zobacz [pamięci podręcznej token uwierzytelniania](#caching).

###<a name="caching"></a>Buforowanie token uwierzytelniania

W niektórych przypadkach można uniknąć wywołanie metody logowania po pierwszym pomyślnym uwierzytelnieniu przechowując token uwierzytelniania od dostawcy.  Aplikacje ze Sklepu Windows i UWP umożliwia [PasswordVault] pamięci podręcznej tokenu uwierzytelniania bieżącego po pomyślnym logowania, w następujący sposób:

    await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook);

    PasswordVault vault = new PasswordVault();
    vault.Add(new PasswordCredential("Facebook", client.currentUser.UserId,
        client.currentUser.MobileServiceAuthenticationToken));

Wartość identyfikatora użytkownika jest przechowywana jako nazwa użytkownika poświadczeń i token jest przechowywana jako hasło. W kolejnych nowych przedsiębiorstw możesz sprawdzić **PasswordVault** pamięci podręcznej poświadczeń. W poniższym przykładzie użyto poświadczenia z pamięci podręcznej, gdy znajdują się, a w przeciwnym razie próbuje ponownie uwierzytelniania wewnętrznej bazy danych:

    // Try to retrieve stored credentials.
    var creds = vault.FindAllByResource("Facebook").FirstOrDefault();
    if (creds != null)
    {
        // Create the current user from the stored credentials.
        client.currentUser = new MobileServiceUser(creds.UserName);
        client.currentUser.MobileServiceAuthenticationToken =
            vault.Retrieve("Facebook", creds.UserName).Password;
    }
    else
    {
        // Regular login flow and cache the token as shown above.
    }

Po wylogowaniu użytkownika należy również usunąć przechowywane poświadczenie w następujący sposób:

    client.Logout();
    vault.Remove(vault.Retrieve("Facebook", client.currentUser.UserId));

Aplikacje Xamarin umożliwia bezpieczne przechowywanie poświadczeń w obiekcie **konta** [Xamarin.Auth] interfejsów API. Przykład użycia tych interfejsów API Zobacz plik kodu [AuthStore.cs] [próbki do udostępniania fotografii ContosoMoments].

Jest stosowane uwierzytelnianie zarządzane klienta, można buforować token dostępu otrzymane od dostawcy, takich jak Facebook lub Twitter. Ten token może dostarczyć żądania nowy token uwierzytelniania od wewnętrznej bazy danych, w następujący sposób:

    var token = new JObject();
    // Replace <your_access_token_value> with actual value of your access token
    token.Add("access_token", "<your_access_token_value>");

    // Authenticate using the access token.
    await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook, token);

##<a name="pushnotifications"></a>Powiadomienia wypychane

Następujące tematy obejmują powiadomienia Push:

* [Zarejestruj się, aby powiadomienia wypychane](#register-for-push)
* [Uzyskaj pakiet SID ze Sklepu Windows](#package-sid)
* [Zarejestruj się przy użyciu szablonów i platform](#register-xplat)

###<a name="register-for-push"></a>Jak: rejestr powiadomienia wypychane

Klient aplikacji Mobile umożliwia zarejestrować powiadomień wypychanych z koncentratorów powiadomienie Azure. Rejestrowanie i spowoduje uzyskanie uchwyt uzyskać z specyficzne dla platformy wypychanych powiadomień usługi (PNS). Po utworzeniu rejestracji, następnie dołączyć tę wartość wraz z wszystkie znaczniki. Poniższy kod rejestruje powiadomienia push aplikacji systemu Windows z usługi powiadomień systemu Windows (WNS):

    private async void InitNotificationsAsync()
    {
        // Request a push notification channel.
        var channel =await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

        // Register for notifications using the new channel.
        await MobileService.GetPush().RegisterNativeAsync(channel.Uri, null);
    }

Jeśli użytkownik jest naciśnięcie do WNS, musisz najpierw [uzyskać pakiet ze Sklepu Windows SID](#package-sid).  Więcej informacji na temat aplikacje systemu Windows, w tym sposobu rejestrowania dla rejestracji szablonu zobacz [powiadomienia wypychane Dodaj do aplikacji].

Żądanie znaczników w kliencie nie jest obsługiwane.  Żądania znacznika bezgłośną są usuwane z rejestracji.
Jeśli chcesz zarejestrować urządzenie przy użyciu tagów, Utwórz niestandardowy interfejs API, korzystającego z interfejsu API koncentratory powiadomienie o przeprowadzenie rejestracji w Twoim imieniu.  [Nawiązywanie połączenia z interfejsu API niestandardowe](#customapi) zamiast `RegisterNativeAsync()` metody.

###<a name="package-sid"></a>Jak: uzyskać pakiet ze Sklepu Windows SID

Pakiet SID jest wymagany dla Włączanie powiadomień wypychanych w aplikacjach ze Sklepu Windows.  Aby uzyskać pakiet SID, zarejestrować aplikację ze Sklepu Windows.

Aby uzyskać ten parametr na wartość:

1. W Eksploratorze rozwiązań programu Visual Studio, kliknij prawym przyciskiem myszy projektu aplikacji ze Sklepu Windows, kliknij pozycję **Magazyn** > **Skojarzyć aplikacji ze sklepem...**.
2. W kreatorze kliknij przycisk **Dalej**, zaloguj się przy użyciu konta Microsoft, wpisz nazwę aplikacji w **minimalnej nową nazwę aplikacji**, a następnie kliknij pozycję **Minimalna**.
3. Po pomyślnym utworzeniu rejestracji aplikacji wybierz nazwę aplikacji, kliknij przycisk **Dalej**, a następnie kliknij, **Kojarzenie**.
4. Zaloguj się do [Centrum deweloperów systemu Windows] za pomocą Account Microsoft. W obszarze **Moje aplikacje**kliknij utworzony rejestracji aplikacji.
5. Kliknij pozycję **Zarządzanie aplikacji** > **tożsamości aplikacji**, a następnie przewiń w dół znajdowanie swojej **SID pakiet**.

Wielu możliwych zastosowań pakietu SID traktowania jej jako identyfikator URI, w tym przypadku należy użyć _ms-app: / /_ jako. Zanotuj wersję pakietu SID utworzone przez połączenie tę wartość jako prefiksu.

Aplikacje Xamarin wymagają niektórych dodatkowy kod, aby można było zarejestrować aplikacji uruchomionych dla systemu iOS lub Android platform. Aby uzyskać więcej informacji zobacz temat dla swojej platformy:

* [Xamarin.Android](app-service-mobile-xamarin-android-get-started-push.md#add-push)
* [Xamarin.iOS](app-service-mobile-xamarin-ios-get-started-push.md#add-push)

###<a name="register-xplat"></a>Jak: szablony wypychanych rejestru do wysyłania powiadomień i platform

Aby zarejestrować szablony, za pomocą `RegisterAsync()` metody z szablonami w następujący sposób:

        JObject templates = myTemplates();
        MobileService.GetPush().RegisterAsync(channel.Uri, templates);

Szablony powinny być `JObject` typy i mogą zawierać wiele szablonów w następującym formacie JSON:

        public JObject myTemplates()
        {
            // single template for Windows Notification Service toast
            var template = "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(message)</text></binding></visual></toast>";

            var templates = new JObject
            {
                ["generic-message"] = new JObject
                {
                    ["body"] = template,
                    ["headers"] = new JObject
                    {
                        ["X-WNS-Type"] = "wns/toast"
                    },
                    ["tags"] = new JArray()
                },
                ["more-templates"] = new JObject {...}
            };
            return templates;
        }

Metoda **RegisterAsync()** przyjmuje również pomocniczej Kafelki:

        MobileService.GetPush().RegisterAsync(string channelUri, JObject templates, JObject secondaryTiles);

Wszystkie znaczniki są usuwane z dala od komputera podczas rejestrowania zabezpieczeń. Aby dodać znaczniki do instalacji lub szablonów w ramach instalacji, zobacz [Praca z serwera wewnętrznej bazy danych programu .NET SDK dla aplikacji Mobile Azure].

Aby wysłać powiadomienia korzystanie z tych szablonów zarejestrowanych, odwołują się do [Interfejsów API koncentratory powiadomienie].

##<a name="misc"></a>Inne tematy

###<a name="errors"></a>Jak: obsługi błędów

W przypadku wystąpienia błędu w wewnętrznej bazie danych klienta SDK podnosi `MobileServiceInvalidOperationException`.  W poniższym przykładzie pokazano sposób obsługi wyjątków, który został zwrócony przez wewnętrznej bazy danych:

    private async void InsertTodoItem(TodoItem todoItem)
    {
        // This code inserts a new TodoItem into the database. When the operation completes
        // and App Service has assigned an Id, the item is added to the CollectionView
        try
        {
            await todoTable.InsertAsync(todoItem);
            items.Add(todoItem);
        }
        catch (MobileServiceInvalidOperationException e)
        {
            // Handle error
        }
    }

Inny przykład sposób postępowania z błędów można znaleźć w [Przykładowe pliki aplikacji Mobile]. Przykład [LoggingHandler] zawiera obsługi pełnomocnika rejestrowania (poniżej) Aby rejestrować żądania odniesienia do wewnętrznej bazy danych.

###<a name="headers"></a>Jak: dostosowywanie żądanie nagłówków

Aby obsługiwać rozwiązania konkretnej aplikacji, może być konieczne Dostosowywanie komunikowanie się z aplikacji Mobile wewnętrznej bazy danych. Na przykład można dodać niestandardowy nagłówek do każdego żądania wychodzących lub nawet kody stanu odpowiedzi. Można używać niestandardowej [DelegatingHandler], tak jak w poniższym przykładzie:

    public async Task CallClientWithHandler()
    {
        HttpResponseMessage[]
        MobileServiceClient client = new MobileServiceClient("AppUrl",
            new MyHandler()
            );
        IMobileServiceTable<TodoItem> todoTable = client.GetTable<TodoItem>();
        var newItem = new TodoItem { Text = "Hello world", Complete = false };
        await todoTable.InsertAsync(newItem);
    }

    public class MyHandler : DelegatingHandler
    {
        protected override async Task<HttpResponseMessage>
            SendAsync(HttpRequestMessage request, CancellationToken cancellationToken)
        {
            // Change the request-side here based on the HttpRequestMessage
            request.Headers.Add("x-my-header", "my value");

            // Do the request
            var response = await base.SendAsync(request, cancellationToken);

            // Change the response-side here based on the HttpResponseMessage

            // Return the modified response
            return response;
        }
    }


<!-- Anchors. -->


<!-- Images. -->

<!-- URLs. -->
[1]: app-service-mobile-windows-store-dotnet-get-started.md
[2]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[3]: app-service-mobile-node-backend-how-to-use-server-sdk.md
[4]: https://msdn.microsoft.com/en-us/library/azure/mt419521(v=azure.10).aspx
[5]: https://github.com/Azure-Samples
[6]: http://www.newtonsoft.com/json/help/html/Properties_T_Newtonsoft_Json_JsonPropertyAttribute.htm
[7]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#how-to-define-a-table-controller
[8]: app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations
[9]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/
[10]: http://www.symbolsource.org/
[11]: http://www.symbolsource.org/Public/Wiki/Using
[12]: https://msdn.microsoft.com/en-us/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient(v=azure.10).aspx

[Dodawanie uwierzytelniania do aplikacji]: app-service-mobile-windows-store-dotnet-get-started-users.md
[Synchronizowanie danych w trybie offline w aplikacji dla urządzeń przenośnych Azure]: app-service-mobile-offline-data-sync.md
[Dodawanie powiadomień wypychanych do aplikacji]: app-service-mobile-windows-store-dotnet-get-started-push.md
[Zarejestruj się w aplikacji umożliwia logowania się do konta Microsoft]: app-service-mobile-how-to-configure-microsoft-authentication.md
[Jak skonfigurować aplikacji usługi logowania usługi Active Directory]: app-service-mobile-how-to-configure-active-directory-authentication.md

<!-- Microsoft URLs. -->
[MobileServiceCollection]: https://msdn.microsoft.com/en-us/library/azure/dn250636(v=azure.10).aspx
[MobileServiceIncrementalLoadingCollection]: https://msdn.microsoft.com/en-us/library/azure/dn268408(v=azure.10).aspx
[MobileServiceAuthenticationProvider]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceauthenticationprovider(v=azure.10).aspx
[MobileServiceUser]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser(v=azure.10).aspx
[MobileServiceAuthenticationToken]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.mobileserviceauthenticationtoken(v=azure.10).aspx
[GetTable]: https://msdn.microsoft.com/en-us/library/azure/jj554275(v=azure.10).aspx
[tworzy odwołanie do tabeli bez typu]: https://msdn.microsoft.com/en-us/library/azure/jj554278(v=azure.10).aspx
[DeleteAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296407(v=azure.10).aspx
[IncludeTotalCount]: https://msdn.microsoft.com/en-us/library/azure/dn250560(v=azure.10).aspx
[InsertAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296400(v=azure.10).aspx
[InvokeApiAsync]: https://msdn.microsoft.com/en-us/library/azure/dn268343(v=azure.10).aspx
[LoginAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296411(v=azure.10).aspx
[LookupAsync]: https://msdn.microsoft.com/en-us/library/azure/jj871654(v=azure.10).aspx
[OrderBy]: https://msdn.microsoft.com/en-us/library/azure/dn250572(v=azure.10).aspx
[OrderByDescending]: https://msdn.microsoft.com/en-us/library/azure/dn250568(v=azure.10).aspx
[ReadAsync]: https://msdn.microsoft.com/en-us/library/azure/mt691741(v=azure.10).aspx
[Sporządzanie]: https://msdn.microsoft.com/en-us/library/azure/dn250574(v=azure.10).aspx
[Wybierz pozycję]: https://msdn.microsoft.com/en-us/library/azure/dn250569(v=azure.10).aspx
[Pomiń]: https://msdn.microsoft.com/en-us/library/azure/dn250573(v=azure.10).aspx
[UpdateAsync]: https://msdn.microsoft.com/en-us/library/azure/dn250536.(v=azure.10)aspx
[Nazwa użytkownika]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.userid(v=azure.10).aspx
[Gdzie]: https://msdn.microsoft.com/en-us/library/azure/dn250579(v=azure.10).aspx
[Azure portal]: https://portal.azure.com/
[Portal Azure klasyczny]: https://manage.windowsazure.com/
[EnableQueryAttribute]: https://msdn.microsoft.com/library/system.web.http.odata.enablequeryattribute.aspx
[Guid.NewGuid]: https://msdn.microsoft.com/en-us/library/system.guid.newguid(v=vs.110).aspx
[ISupportIncrementalLoading]: http://msdn.microsoft.com/library/windows/apps/Hh701916.aspx
[Centrum deweloperów systemu Windows]: https://dev.windows.com/en-us/overview
[DelegatingHandler]: https://msdn.microsoft.com/library/system.net.http.delegatinghandler(v=vs.110).aspx
[SDK usługi Windows Live]: https://msdn.microsoft.com/en-us/library/bb404787.aspx
[PasswordVault]: http://msdn.microsoft.com/library/windows/apps/windows.security.credentials.passwordvault.aspx
[ProtectedData]: http://msdn.microsoft.com/library/system.security.cryptography.protecteddata%28VS.95%29.aspx
[Interfejsy API koncentratory powiadomień]: https://msdn.microsoft.com/library/azure/dn495101.aspx
[Przykładowe pliki aplikacji dla urządzeń przenośnych]: https://github.com/Azure-Samples/app-service-mobile-dotnet-todo-list-files
[LoggingHandler]: https://github.com/Azure-Samples/app-service-mobile-dotnet-todo-list-files/blob/master/src/client/MobileAppsFilesSample/Helpers/LoggingHandler.cs#L63

<!-- External URLs -->
[OData v3 dokumentacji]: http://www.odata.org/documentation/odata-version-3-0/
[Fiddler]: http://www.telerik.com/fiddler
[Json.NET]: http://www.newtonsoft.com/json
[Xamarin.Auth]: https://components.xamarin.com/view/xamarin.auth/
[AuthStore.cs]: (https://github.com/azure-appservice-samples/ContosoMoments/blob/dev/src/Mobile/ContosoMoments/Helpers/AuthStore.cs)
[Przykładowe udostępniania fotografii ContosoMoments]: https://github.com/azure-appservice-samples/ContosoMoments