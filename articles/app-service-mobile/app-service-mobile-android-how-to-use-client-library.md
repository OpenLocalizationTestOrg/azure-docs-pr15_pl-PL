<properties
    pageTitle="Jak używać Android biblioteki klienta aplikacji Mobile"
    description="Jak w przypadku aplikacji Mobile Azure za pomocą klienta Android SDK."
    services="app-service\mobile"
    documentationCenter="android"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>


# <a name="how-to-use-the-android-client-library-for-mobile-apps"></a>Jak używać biblioteki Android klienta dla aplikacji Mobile

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Ten przewodnik pokazano, jak za pomocą klienta usługi Android zestaw SDK dla aplikacji Mobile do wykonania typowych scenariuszy, takich jak:

- Wyszukiwanie danych (Wstawianie, aktualizowanie i usuwanie).
- Uwierzytelnianie.
- Obsługa błędów.
- Dostosowywanie klienta. 

Powoduje także poszerzony do wspólnej kod klienta używany w aplikacji dla większości urządzeń przenośnych.

Ten przewodnik omówiono SDK Android po stronie klienta.  Aby dowiedzieć się więcej o SDK po stronie serwera dla aplikacji Mobile, zobacz [Praca z wewnętrznej bazy danych programu .NET SDK] [ 10] lub [sposobu używania wewnętrznej bazy danych Node.js SDK][11].

## <a name="reference-documentation"></a>Dokumentacji

[Odwołanie Javadocs interfejsu API] można znaleźć[ 12] dla biblioteki klienta Android w GitHub.

## <a name="supported-platforms"></a>Platformy obsługiwane

Android SDK aplikacje Mobile Azure obsługuje interfejs API poziomów 19 do 24 (KitKat przez nugacie).  

Uwierzytelnianie "serwer przepływu" za pomocą widoku sieci Web prezentowana interfejsu użytkownika. Jeśli urządzenie nie jest możliwe przedstawienie interfejsu użytkownika widoku sieci Web, inne metody uwierzytelniania są wymagane przez wykracza poza zakres produktu.  Zestaw SDK nie jest odpowiedni typ czujki lub podobnie urządzeń z ograniczonymi uprawnieniami.

## <a name="setup-and-prerequisites"></a>Instalacja i wymagania wstępne

Wykonywanie samouczka [aplikacji Mobile Szybki Start](app-service-mobile-android-get-started.md) .  To zadanie gwarantuje, że zostały spełnione wszystkie wymagania wstępne dotyczącej opracowywania aplikacji Mobile Azure.  Szybki Start ułatwia również skonfigurować to konto i tworzenie do pierwszej wewnętrznej bazy danych aplikacji Mobile.

Jeśli zdecydujesz się nie do zakończenia tego samouczka Szybki Start, należy wykonać następujące czynności:

- [Tworzenie aplikacji Mobile wewnętrznej bazie danych] [ 13] do użytku z systemem Android aplikacji.
- W Android Studio [Aktualizowanie plików kompilacji Gradle](#gradle-build).
- [Włączanie uprawnień internetowych](#enable-internet).

###<a name="gradle-build"></a>Aktualizowanie pliku kompilacji Gradle

Zmienianie oba pliki **build.gradle** :

1. Dodaj ten kod do pliku *projektu* poziomu **build.gradle** wewnątrz znacznika *buildscript* :

        buildscript {
            repositories {
                jcenter()
            }
        }

2. Dodaj kod do pliku *modułu aplikacji* poziomu **build.gradle** wewnątrz znacznika *zależności* :

        compile 'com.microsoft.azure:azure-mobile-android:3.1.0'

    Najnowsza wersja jest obecnie 3.1.0. Obsługiwane wersje są wymienione [tutaj][14].

###<a name="enable-internet"></a>Włączanie uprawnień internetowych

Aby uzyskać dostęp do Azure, aplikacji musi mieć włączone uprawnienie INTERNET. Jeśli jeszcze nie jest włączona, Dodaj poniższy wiersz kodu do pliku **AndroidManifest.xml** :

    <uses-permission android:name="android.permission.INTERNET" />

## <a name="the-basics-deep-dive"></a>Dive głębokości — informacje podstawowe

W tej sekcji opisano niektóre z kodu w aplikacji Szybki Start, które dotyczą przy użyciu aplikacji Mobile Azure.  

###<a name="data-object"></a>Definiowanie klas danych klienta

Aby uzyskać dostęp do danych z tabel platformy SQL Azure, definiowanie klas danych klienta, które odpowiadają do tabel w aplikacji Mobile wewnętrznej bazy danych. Przykłady w tym temacie założono tabeli o nazwie **ToDoItem**, który zawiera następujące kolumny:

- Identyfikator
- tekst
- Kończenie

Odpowiednie wpisać obiektów po stronie klienta:

    public class ToDoItem {
        private String id;
        private String text;
        private Boolean complete;
    }

Kod znajduje się w pliku o nazwie **ToDoItem.java**.

Jeśli tabela platformy SQL Azure zawiera więcej kolumn, należy dodać odpowiednich pól do tej klasy.  Na przykład jeśli DTO (obiekt transfer danych) zawiera kolumnę priorytet liczby całkowitej, a następnie można dodać to pole wraz z jego metody pobierające i ustawiające:

    private Integer priority;

    /**
    * Returns the item priority
    */
    public Integer getPriority() {
        return mPriority;
    }
    
    /**
    * Sets the item priority
    *
    * @param priority
    *            priority to set
    */
    public final void setPriority(Integer priority) {
        mPriority = priority;
    }

Aby dowiedzieć się, jak tworzyć dodatkowe tabele w swojej aplikacji Mobile wewnętrznej bazy danych, zobacz [jak: Definiowanie kontrolerze tabeli] [ 15] (.NET wewnętrznej bazy danych) lub [Definiowanie tabel przy użyciu schematu dynamiczne] [ 16] (Node.js wewnętrznej bazy danych). Dla Node.js wewnętrznej bazy danych można też użyć ustawienia **łatwe tabel** w [Azure portal].

###<a name="create-client"></a>Jak: tworzenie kontekstu klienta

Kod ten tworzy obiekt **MobileServiceClient** , który umożliwia dostęp do wewnętrznej bazy danych aplikacji Mobile. Kod przechodzi `onCreate` metody klasy **działania** określone w *AndroidManifest.xml* Akcja **główne** i **Uruchom** kategorii. W kodzie Szybki Start trafia w pliku **ToDoActivity.java** .

        MobileServiceClient mClient = new MobileServiceClient(
            "MobileAppUrl", // Replace with the Site URL
            this)

W tym kodzie Zamień `MobileAppUrl` przy użyciu adresu URL usługi aplikacji Mobile wewnętrznej bazy danych, które znajdują się w [Azure portal] w karta dla twojej aplikacji Mobile wewnętrznej bazy danych. Dla tego wiersza kodu skompilować należy dodać następującą instrukcję **Importowanie** :

    import com.microsoft.windowsazure.mobileservices.*;

###<a name="instantiating"></a>Jak: Tworzenie odwołania do tabeli

Najłatwiejszym sposobem kwerendy lub zmodyfikować dane w wewnętrznej bazy danych jest za pomocą *wpisany model programowania*, ponieważ jednoznacznie określonym języka Java. Ten model zawiera Bezproblemowa szeregowania JSON i deserializacji przy użyciu [gson] [ 3] biblioteki podczas wysyłania danych między obiektami klienta i tabelami w wewnętrznej bazie danych Azure SQL.

Aby uzyskać dostęp do tabeli, najpierw należy utworzyć [MobileServiceTable] [ 8] obiektu, dzwoniąc **getTable** metoda [MobileServiceClient][9].  Ta metoda wymaga dwóch overloads:

    public class MobileServiceClient {
        public <E> MobileServiceTable<E> getTable(Class<E> clazz);
        public <E> MobileServiceTable<E> getTable(String name, Class<E> clazz);
    }

Poniższy kod **mClient** jest to odwołanie do obiektu MobileServiceClient.  Pierwszy przeciążeń jest używany w miejsce, w którym nazwa klasy i nazwa tabeli są takie same, i jest język używany w szybki start:

    MobileServiceTable<ToDoItem> mToDoTable = mClient.getTable(ToDoItem.class);

Druga przeciążeń służy nazwa tabeli różni się od nazwy klasy: pierwszy parametr jest nazwą tabeli.

    MobileServiceTable<ToDoItem> mToDoTable = mClient.getTable("ToDoItemBackup", ToDoItem.class);

###<a name="binding"></a>Jak: dane są związane z interfejsu użytkownika

Wiązanie danych obejmuje trzy elementy:

- Źródła danych
- Układ ekranu
- Karta łączący dwa razem.

W naszym przykładowy kod możemy zwraca dane z tabeli platformy SQL Azure aplikacji Mobile **ToDoItem** do tablicy. To działanie jest typowych wzór wypełnienia dla aplikacji danych.  Kwerendy bazy danych często zwracają zbiór wierszy, które klient pobiera na liście lub w tablicy. W tym przykładzie tablicy jest źródłem danych.

Kod określa układ ekranu, która definiuje widok danych, który jest wyświetlany na urządzeniu.  Dwa są powiązane razem z karty, czyli w tym kodzie rozszerzenie z **ArrayAdapter&lt;ToDoItem&gt; ** zajęć.

#### <a name="layout"></a>Jak: definiowanie układu

Układ jest określona przez kilka wstawki kodu XML. Uwzględniając istniejący układ, poniższy kod reprezentuje **widoku listy** chcemy wypełnić z danymi serwera.

    <ListView
        android:id="@+id/listViewToDo"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        tools:listitem="@layout/row_list_to_do" >
    </ListView>

W poprzednim kodzie atrybutu *element listy* Określa identyfikator układu dla poszczególnych wierszy na liście. Ten kod określa pola wyboru i jego tekst i otrzymuje wystąpienia raz dla każdego elementu na liście. Ten układ nie są wyświetlane pola **identyfikator** i bardziej złożone układ chcesz określić dodatkowe pola wyświetlane na. Kod znajduje się w pliku **row_list_to_do.xml** .

    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="horizontal">
        <CheckBox
            android:id="@+id/checkToDoItem"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@string/checkbox_text" />
    </LinearLayout>


#### <a name="adapter"></a>Jak: Definiowanie karty

Ponieważ nasze widoku źródła danych jest tablicą **ToDoItem**, możemy klasy Nasze karty z **ArrayAdapter&lt;ToDoItem&gt; ** zajęć. Ten klasy tworzy widok dla każdej **ToDoItem** przy użyciu układu **row_list_to_do** .

W naszym kodzie zdefiniowano następujące klasy, która jest rozszerzeniem **ArrayAdapter&lt;E&gt; ** klasy:

    public class ToDoItemAdapter extends ArrayAdapter<ToDoItem> {

Zastąp metodę **getView** kart. Na przykład:

    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
        View row = convertView;

        final ToDoItem currentItem = getItem(position);

        if (row == null) {
            LayoutInflater inflater = ((Activity) mContext).getLayoutInflater();
            row = inflater.inflate(R.layout.row_list_to_do, parent, false);
        }

        row.setTag(currentItem);

        final CheckBox checkBox = (CheckBox) row.findViewById(R.id.checkToDoItem);
        checkBox.setText(currentItem.getText());
        checkBox.setChecked(false);
        checkBox.setEnabled(true);

        checkBox.setOnClickListener(new View.OnClickListener() {

            @Override
            public void onClick(View arg0) {
                if (checkBox.isChecked()) {
                    checkBox.setEnabled(false);
                    if (mContext instanceof ToDoActivity) {
                        ToDoActivity activity = (ToDoActivity) mContext;
                        activity.checkItem(currentItem);
                    }
                }
            }
        });


        return row;
    }

Tworzymy wystąpienie tej klasy w naszym aktywności w następujący sposób:

    ToDoItemAdapter mAdapter;
    mAdapter = new ToDoItemAdapter(this, R.layout.row_list_to_do);

Drugi parametr do konstruktora ToDoItemAdapter jest to odwołanie do układu. Firma Microsoft teraz utworzyć **Widok listy** i przydzielanie karta do **widoku listy**.

    ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
    listViewToDo.setAdapter(mAdapter);

### <a name="api"></a>Struktura interfejsu API

Operacje tabeli w aplikacji Mobile i niestandardowe interfejsu API są asynchroniczne. Używanie obiektów [przyszłości] oraz [AsyncTask] asynchroniczne metod dotyczących kwerend, wstawia, aktualizowanie i usuwanie. Za pomocą prognozy ułatwia do wykonywania wielu operacji w wątku tła bez konieczności zajmowania wielu zwrotne zagnieżdżonych.

Przejrzyj plik **ToDoActivity.java** w programie project Android Szybki Start w [Azure portal] , na przykład.

#### <a name="use-adapter"></a>Sposoby: za pomocą karty

Teraz możesz przystąpić do wiązania danych za pomocą. Poniższy kod przedstawia sposób pobierania elementów w tabeli i wypełniony karty lokalnej zwracane elementy.

    public void showAll(View view) {
        AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>(){
            @Override
            protected Void doInBackground(Void... params) {
                try {
                    final List<ToDoItem> results = mToDoTable.execute().get();
                    runOnUiThread(new Runnable() {

                        @Override
                        public void run() {
                            mAdapter.clear();
                            for (ToDoItem item : results) {
                                mAdapter.add(item);
                            }
                        }
                    });
                } catch (Exception exception) {
                    createAndShowDialog(exception, "Error");
                }
                return null;
            }
        };
        runAsyncTask(task);
    }

Połączeń karty w dowolnej chwili możesz zmodyfikować tabeli **ToDoItem** . Ponieważ modyfikacje są wykonywane na podstawie rekordu na podstawie, uchwyt się pojedynczego wiersza zamiast zbioru. Po wstawieniu elementu, **Dodaj** metodę połączenia na karcie; Podczas usuwania, wywołaj metodę **usunąć** .

##<a name="querying"></a>Jak: kwerendy danych z Twojej aplikacji Mobile wewnętrznej bazy danych

W tej sekcji opisano sposób wysyłania kwerend do aplikacji Mobile wewnętrznej bazy danych, która obejmuje następujące zadania:

- [Zwraca wszystkie elementy]
- [Filtrowanie danych zwróconych]
- [Sortowanie zwrócił dane.]
- [Zwróć dane na stronach]
- [Wybierz określone kolumny]
- [ZŁĄCZ.teksty metody zapytania](#chaining)

### <a name="showAll"></a>Jak: zwraca wszystkie elementy z tabeli

Poniższa kwerenda zwraca wszystkie elementy w tabeli **ToDoItem** .

    List<ToDoItem> results = mToDoTable.execute().get();

Zmienna *wyniki* zwraca zestaw z kwerendy jako listy wyników.

### <a name="filtering"></a>Jak: Filtr zwrócone dane

Następujące wykonywanie kwerendy zwraca wszystkie elementy z tabeli **ToDoItem** miejsce, w którym **wykonane** ma wartość **FAŁSZ**.

    List<ToDoItem> result = mToDoTable.where()
                                .field("complete").eq(false)
                                .execute().get();

**mToDoTable** to odwołanie do tabeli usługi mobilnej utworzone wcześniej.

Definiowanie filtru przy użyciu połączenia metody **miejsce, w którym** na odwołanie do tabeli. Metoda **miejsce, w którym** występuje **pola** metody następuje metodę, która określa predykacie logiczne. Możliwe metody predykatu obejmują **eq** (równa się), **no** (nie równa), **BT** (większe niż), **stronę** (większe niż lub równe), **lt** (mniejsze niż), **le** (mniejsze niż lub równe). Te metody umożliwiają porównanie pól typu Liczba i ciąg do określonej wartości.

Można filtrować według daty. Następujące metody umożliwiają porównywanie pola daty całego lub części daty: **rok**, **miesiąc**, **dzień**, **godziny**, **minuty**i **drugi**. W poniższym przykładzie dodawane filtr dla elementów, których *Data ukończenia* jest równe 2013.

    mToDoTable.where().year("due").eq(2013).execute().get();

Następujące metody obsługuje złożone filtry pola ciągów: **startsWith**, **endsWith** **"concat"**, **podciąg**, **indexOf**, **Zamień**, **toLower**, **toUpper**, **Przycinanie**i **długości**. Poniższy przykład filtry dla wiersze tabeli, w którym kolumny *tekstu* zaczyna się od "PRI0".

    mToDoTable.where().startsWith("text", "PRI0").execute().get();

Następujące metody operator są obsługiwane na pola liczbowe: **Dodawanie**, **sub** **mul**, **div**, **mod**, **powierzchnia**, **ceiling**i **zaokrąglić**. Poniższy przykład filtry dla wiersze tabeli, której wartość **czasu trwania** liczba jest parzysta.

    mToDoTable.where().field("duration").mod(2).eq(0).execute().get();

Orzeczenia można połączyć z tych metod logicznych: **, **lub** **i **nie**. Poniższy przykład łączy dwa poprzednich przykładach.

    mToDoTable.where().year("due").eq(2013).and().startsWith("text", "PRI0")
                .execute().get();

Grupowanie i zagnieździć operatorów logicznych:

    mToDoTable.where()
                .year("due").eq(2013)
                    .and
                (startsWith("text", "PRI0").or().field("duration").gt(10))
                .execute().get();

Aby uzyskać bardziej szczegółowe omówienie i przykłady filtrowania zobacz [poznawanie formatem modelu kwerendy Android klienta](http://hashtagfail.com/post/46493261719/mobile-services-android-querying).

### <a name="sorting"></a>Jak: sortowanie zwrócone dane

Poniższy kod zwraca wszystkie elementy z tabeli **ToDoItems** sortowaniem w kolejności rosnącej w polu *tekstowym* . *mToDoTable* to odwołanie do tabeli wewnętrznej bazy danych utworzonej poprzednio:

    mToDoTable.orderBy("text", QueryOrder.Ascending).execute().get();

Pierwszy parametr metody **orderBy** jest ciągiem równe nazwę pola, według którego chcesz sortować. Drugi parametr używa wyliczenie **QueryOrder** , aby określić, czy mają być sortowane w kolejności rosnącej lub malejącej.  Jeśli filtrujesz przy użyciu metody ***miejsce, w którym*** metody ***miejsce, w którym*** należy wywołany przed metody ***orderBy*** .

### <a name="paging"></a>Jak: zwracają dane na stronach

W pierwszym przykładzie pokazano sposób zaznacz pięć pierwszych wartości w tabeli. Kwerenda zwraca elementy z tabeli **ToDoItems**. **mToDoTable** to odwołanie do tabeli wewnętrznej bazy danych utworzonej poprzednio:

    List<ToDoItem> result = mToDoTable.top(5).execute().get();


Oto kwerendę, która umożliwia pomijanie pięć pierwszych elementów, a następnie zwraca następnych pięciu:

    mToDoTable.skip(5).top(5).execute().get();

### <a name="selecting"></a>Jak: wybierz określone kolumny

Poniższy kod przedstawia sposób przywrócić wszystkie elementy z tabeli **ToDoItems**, ale będzie wyświetlana tylko pola **Wykonano** i **tekst** . **mToDoTable** to odwołanie do tabeli wewnętrznej bazy danych utworzone wcześniej.

    List<ToDoItemNarrow> result = mToDoTable.select("complete", "text").execute().get();

Parametry funkcji wybierz są ciąg nazwy kolumn tabeli, które chcesz przywrócić.

**Wybierz** metodę musi wykonać metod, takich jak **miejsce** i **orderBy**. Może wystąpić metodami nawigacji między stronami, takie jak **góry**.

### <a name="chaining"></a>Jak: ZŁĄCZ.teksty metody zapytania

Może zostać dołączona metod wyszukiwania tabel wewnętrznej bazy danych. Tworzenie łańcucha metody zapytania umożliwia wybranie określonych kolumn wyfiltrowane wiersze, które są sortowane i stronicowanej. Można tworzyć złożone filtry logiczne.
Każda z metod kwerenda zwraca obiekt kwerendy. Aby zakończyć szereg metod i faktycznie uruchomić kwerendę, zadzwoń **Wykonywanie** metody. Na przykład:

    mToDoTable.where()
        .year("due").eq(2013)
        .and().startsWith("text", "PRI0")
        .or().field("duration").gt(10)
        .orderBy(duration, QueryOrder.Ascending)
        .select("id", "complete", "text", "duration")
        .top(20)
        .execute().get();

Metody łańcuchowej zapytania musi być sortowane w następujący sposób:

1. Metody filtrowania (**gdzie**).
2. Metody sortowania (**orderBy**).
3. Metody zaznaczenia (**Wybierz**).
4. stronicowanie metody (**Pomiń** i od **góry**).

##<a name="inserting"></a>Jak: wstawianie dane do wewnętrznej bazy danych

Utwórz wystąpienie wystąpienie klasy *ToDoItem* i ustawianie jej właściwości.

    ToDoItem item = new ToDoItem();
    item.text = "Test Program";
    item.complete = false;

Aby wstawić obiekt użyć **insert()** :

    ToDoItem entity = mToDoTable.insert(item).get();

Dopasowuje zwracane podmiotu danych umieszczonych w tabeli wewnętrznej bazy danych dostępne pola ID i innych wartości zestawu do wewnętrznej bazy danych.

Tabele w aplikacji Mobile wymagają kolumny klucza podstawowego o nazwie **identyfikator**. Domyślnie w tej kolumnie to ciąg. Wartość domyślna kolumny Identyfikator jest identyfikator GUID.  Można udostępnić innym unikatowych wartości, takich jak nazwy użytkowników lub adresy e-mail. Wartość Identyfikatora ciągu nie jest dostępne w wstawiony rekord, nowy identyfikator GUID generuje wewnętrznej bazy danych.

Wartości identyfikatorów ciąg mają następujące zalety:

+ Identyfikatory mogą być generowane bez wprowadzania podróż do bazy danych.
+ Rekordy są łatwiejsze do scalenia z różnych tabel lub baz danych.
+ Wartości identyfikatorów lepszej integracji z logiki aplikacji.

Wartości identyfikatorów ciągu są **wymagane** do pomocy technicznej synchronizacja w trybie offline.

##<a name="updating"></a>Jak: aktualizowanie danych w aplikacji dla urządzeń przenośnych

Aby zaktualizować dane w tabeli, należy przekazać nowy obiekt do metody **update()** .

    mToDoTable.update(item).get();

W tym przykładzie *elementem* jest odwołanie do wiersza w tabeli *ToDoItem* wprowadzono kilka zmian.
Wiersz z tym samym **identyfikator** zostanie zaktualizowany.

##<a name="deleting"></a>Jak: usuwanie danych w aplikacji dla urządzeń przenośnych

Poniższy kod pokazano, jak usunąć dane z tabeli, określając obiekt danych.

    mToDoTable.delete(item);

Możesz również usunąć element, określając pola **identyfikator** wiersza do usunięcia.

    String myRowId = "2FA404AB-E458-44CD-BC1B-3BC847EF0902";
    mToDoTable.delete(myRowId);

##<a name="lookup"></a>Jak: wyszukiwanie określonego elementu

Wyszukiwanie elementu z polem określony **identyfikator** przy użyciu metody **lookUp()** :

    ToDoItem result = mToDoTable
                        .lookUp("0380BAFB-BCFF-443C-B7D5-30199F730335")
                        .get();

##<a name="untyped"></a>Jak: Praca z danymi bez typu

Bez typu modelu programowania zapewnia dokładną kontrolę nad szeregowania JSON.  Istnieje kilka typowych scenariuszy, gdzie możesz używać bez typu modelu programowania. Jeśli na przykład tabeli wewnętrznej bazy danych zawiera wiele kolumn, należy odwołać podzbiór kolumn.  Wpisany modelu wymaga zdefiniowania wszystkich aplikacji dla urządzeń przenośnych kolumn tabeli w klasie danych.  

Większość interfejsu API do uzyskiwania dostępu do danych są podobne do wpisanego programowania połączeń. Podstawowa różnica polega na tym, że w modelu bez typu wywoływania metod obiektu **MobileServiceJsonTable** , zamiast obiektu **MobileServiceTable** .

### <a name="json_instance"></a>Jak: tworzenie wystąpienie bez typu tabeli

Podobnie jak w modelu wpisany, możesz uruchomić wprowadzenie odwołania do tabeli, ale w tym przypadku jest to obiekt **MobileServicesJsonTable** . Uzyskać odwołanie, dzwoniąc **getTable** metoda wystąpienie klienta:

    private MobileServiceJsonTable mJsonToDoTable;
    //...
    mJsonToDoTable = mClient.getTable("ToDoItem");

Po utworzeniu wystąpienie **MobileServiceJsonTable**ma on praktycznie tego samego interfejsu API dostępne jako z wpisanym modelu programowania. W niektórych przypadkach metody potrwać bez typu parametru zamiast wpisanego parametru.

### <a name="json_insert"></a>Jak: Wstawianie tabeli bez typu

Poniższy kod przedstawia sposób wykonania zadania wstawkę. Pierwszym krokiem jest utworzenie [JsonObject][1], która stanowi część [gson] [ 3] biblioteki.

    JsonObject jsonItem = new JsonObject();
    jsonItem.addProperty("text", "Wake up");
    jsonItem.addProperty("complete", false);

Następnie należy użyć **insert()** , aby wstawić obiekt bez typu do tabeli.

    mJsonToDoTable.insert(jsonItem).get();

Jeśli jest potrzebne do uzyskania identyfikator wstawionym obiekcie, użyj metody **getAsJsonPrimitive()** .

    jsonItem.getAsJsonPrimitive("id").getAsInt());

### <a name="json_delete"></a>Jak: usuwanie z tabeli bez typu

Poniższy kod pokazano, jak usunąć wystąpienie, w tym przypadku tego samego wystąpienia **JsonObject** został utworzony w tym przykładzie poprzednich *Wstawianie* . Kod jest taka sama jak z wpisanym wielkość liter, ale metoda ma inny podpis, ponieważ odwołuje się **JsonObject**.

         mToDoTable.delete(jsonItem);

Możesz również usunąć wystąpienie bezpośrednio przy użyciu Identyfikatora:

         mToDoTable.delete(ID);

### <a name="json_get"></a>Jak: zwraca wszystkie wiersze z tabeli bez typu

Poniższy kod pokazano, jak pobrać całej tabeli. Ponieważ używasz tabeli JSON, można selektywne można pobrać tylko niektóre kolumny tabeli.

    public void showAllUntyped(View view) {
        new AsyncTask<Void, Void, Void>() {
            @Override
            protected Void doInBackground(Void... params) {
                try {
                    final JsonElement result = mJsonToDoTable.execute().get();
                    final JsonArray results = result.getAsJsonArray();
                    runOnUiThread(new Runnable() {

                        @Override
                        public void run() {
                            mAdapter.clear();
                            for (JsonElement item : results) {
                                String ID = item.getAsJsonObject().getAsJsonPrimitive("id").getAsString();
                                String mText = item.getAsJsonObject().getAsJsonPrimitive("text").getAsString();
                                Boolean mComplete = item.getAsJsonObject().getAsJsonPrimitive("complete").getAsBoolean();
                                ToDoItem mToDoItem = new ToDoItem();
                                mToDoItem.setId(ID);
                                mToDoItem.setText(mText);
                                mToDoItem.setComplete(mComplete);
                                mAdapter.add(mToDoItem);
                            }
                        }
                    });
                } catch (Exception exception) {
                    createAndShowDialog(exception, "Error");
                }
                return null;
            }
        }.execute();
    }

Ten sam zestaw filtrowania, filtrowania i stronicowania metod, które są dostępne dla określonego modelu są dostępne z bez typu modelu.

##<a name="custom-api"></a>Jak: połączenie niestandardowego interfejsu API

Interfejs API niestandardowe pozwala na zdefiniowanie niestandardowych punktów końcowych z użyciem funkcji serwera, które nie zamapować na wstawianie, aktualizowanie, usuwanie lub operacja odczytu. Za pomocą niestandardowych interfejsu API, możesz mieć większą kontrolę nad wiadomości, w tym do czytania i ustawianie nagłówków wiadomości HTTP oraz definiowania formatem treści wiadomości, oprócz JSON.

W kliencie Android wywołania metody **invokeApi** połączenie niestandardowe końcowy interfejsu API. W poniższym przykładzie pokazano sposób wywoływania punktu końcowego interfejsu API o nazwie **completeAll**, która zwraca klasę zbioru o nazwie **MarkAllResult**.

    public void completeItem(View view) {

        ListenableFuture<MarkAllResult> result = mClient.invokeApi( "completeAll", MarkAllResult.class );

            Futures.addCallback(result, new FutureCallback<MarkAllResult>() {
                @Override
                public void onFailure(Throwable exc) {
                    createAndShowDialog((Exception) exc, "Error");
                }

                @Override
                public void onSuccess(MarkAllResult result) {
                    createAndShowDialog(result.getCount() + " item(s) marked as complete.", "Completed Items");
                    refreshItemsFromTable();
                }
            });
        }

Metoda **invokeApi** jest wywoływana na klienta, który wysyła żądanie WPIS do nowego interfejsu API usługi niestandardowe. Wynik zwrócony przez niestandardowe interfejsu API jest wyświetlany w oknie dialogowym wiadomości, jak są wszystkie błędy. Inne wersje **invokeApi** umożliwiają opcjonalnie Wysyłanie obiektu w treści żądania, określanie metody HTTP i wysyłanie parametry kwerendy z żądaniem. Dostępne są także bez typu wersji **invokeApi** .

##<a name="authentication"></a>Jak: Dodawanie uwierzytelniania do aplikacji

Samouczki już opisano szczegółowo dodać te funkcje.

Obsługa aplikacji usługi [uwierzytelniania użytkowników aplikacji](app-service-mobile-android-get-started-users.md) , za pomocą różnych dostawcy tożsamości zewnętrznych: Facebook, Google Account Microsoft, Twitter i usługi Azure Active Directory. Można ustawić uprawnienia w tabelach w celu ograniczenia dostępu dla określonych operacje, aby tylko uwierzytelnieni użytkownicy. Za pomocą tożsamości uwierzytelnionych użytkowników do wykonania reguł autoryzacji w sieci wewnętrznej bazy danych.

Obsługiwane są dwie przebiegu uwierzytelniania: **serwer** i przepływ **klienta** . Przepływ server zapewnia najłatwiejszym obsługę uwierzytelniania, zależy od interfejsu sieci web dostawcy tożsamości.  Nie dodatkowych SDK są wymagane do wdrożenia serwera przepływ uwierzytelniania. Uwierzytelnianie serwera przepływu nie ścisła integracja do urządzenia przenośnego i zaleca się tylko dowodu koncepcji scenariuszy.

Przepływ klienta umożliwia dla lepsza integracja z funkcje specyficzne dla urządzenia, takie jak logowania jednokrotnego zależy od SDK dostarczony przez dostawcę tożsamości.  Na przykład można zintegrować Facebook SDK urządzeń przenośnych aplikacji.  Klienta przenośnego zamienia do aplikacji Facebook i potwierdza usługi logowania jednokrotnego przed wymiany Twojej aplikacji dla urządzeń przenośnych.

Cztery czynności, aby włączyć uwierzytelnianie w aplikacji:

- Zarejestruj się w aplikacji dla uwierzytelniania z dostawcą tożsamości.
- Konfigurowanie sieci wewnętrznej bazy danych aplikacji usługi.
- Ograniczanie uprawnień tabeli uwierzytelnionych użytkowników tylko do wewnętrznej bazy danych aplikacji usługi.
- Dodawanie kodu uwierzytelniania do aplikacji.

Można ustawić uprawnienia w tabelach w celu ograniczenia dostępu dla określonych operacje, aby tylko uwierzytelnieni użytkownicy. Aby zmodyfikować żądania umożliwia także identyfikatory zabezpieczeń uwierzytelnionego użytkownika.  Aby uzyskać więcej informacji przejrzyj [rozpocząć pracę z uwierzytelnianiem] i dokumentację serwera SDK instrukcje.

### <a name="caching"></a>Jak: Dodawanie kodu uwierzytelniania do aplikacji

Poniższy kod rozpoczyna się proces logowania przepływu server przy użyciu dostawcy Google:

    MobileServiceUser user = mClient.login(MobileServiceAuthenticationProvider.Google);

Uzyskaj identyfikator zalogowany użytkownik od **MobileServiceUser** przy użyciu metody **getUserId** . Na przykład dotyczące używania prognozy połączenie asynchroniczne logowania interfejsy API zobacz [Wprowadzenie do uwierzytelniania].

### <a name="caching"></a>Jak: pamięci podręcznej tokenów uwierzytelniania

Buforowanie tokenów uwierzytelniania należy przechowywać identyfikator użytkownika i token uwierzytelniania lokalnie na urządzeniu. Przy następnym uruchomieniu aplikacji, możesz sprawdzić pamięci podręcznej, a jeśli istnieją następujące wartości, możesz pominąć dziennika w procedurze i rehydrate klienta przy użyciu tych danych. Jednak tych danych jest wielkość liter, a powinny być przechowywane, zaszyfrowane dla bezpieczeństwa w przypadku telefonu otrzymuje kradzież.

Zobaczysz pełny przykład jak do tokenów uwierzytelniania pamięci podręcznej w [pamięci podręcznej uwierzytelniania tokeny sekcji][7].

Podczas próby za pomocą token wygasłe, otrzymasz odpowiedź *401 — Brak autoryzacji* . Można obsługiwać błędy uwierzytelniania przy użyciu filtrów.  Filtry przechwycić żądania do wewnętrznej bazy danych aplikacji usługi. Kod filtru testują odpowiedzi na 401, proces logowania i zostało przywrócone żądania, który wygenerował 401. 

## <a name="adal"></a>Jak: uwierzytelniania użytkowników z Active Directory Authentication Library

Biblioteki uwierzytelniania Active Directory (ADAL) umożliwia użytkownikom zalogować się do aplikacji przy użyciu usługi Azure Active Directory. Za pomocą logowania przepływu klienta jest często używane zamiast `loginAsync()` metod, ponieważ zawiera więcej natywnych działanie interfejsu użytkownika i umożliwia dodatkowe dostosowania.

1. Konfigurowanie sieci wewnętrznej bazy danych aplikacji dla urządzeń przenośnych dla logowania AAD, wykonując samouczek [sposobu konfigurowania aplikacji usługi logowania usługi Active Directory](app-service-mobile-how-to-configure-active-directory-authentication.md) . Upewnij się ukończyć krok opcjonalny rejestrowania natywnej aplikacji.

2. Zainstaluj ADAL, modyfikując plik build.gradle, aby uwzględnić następujące definicje:

```
repositories {
    mavenCentral()
    flatDir {
        dirs 'libs'
    }
    maven {
        url "YourLocalMavenRepoPath\\.m2\\repository"
    }
}
packagingOptions {
    exclude 'META-INF/MSFTSIG.RSA'
    exclude 'META-INF/MSFTSIG.SF'
}
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile('com.microsoft.aad:adal:1.1.1') {
        exclude group: 'com.android.support'
    } // Recent version is 1.1.1
    compile 'com.android.support:support-v4:23.0.0'
}
```

3. Dodaj następujący kod w aplikacji, co poprawnej następujące:

* Zamień **Wstawianie urząd tutaj** nazwę dzierżawy, w którym możesz obsługi administracyjnej aplikacji. Format powinien być https://login.windows.net/contoso.onmicrosoft.com. Ta wartość można skopiować na karcie domeny w swojej usługi Azure Active Directory w [Azure portal klasyczny].

* Zamień **Identyfikator tutaj, wstawianie ZASOBU w-** identyfikator klienta do wewnętrznej bazy danych aplikacji dla urządzeń przenośnych. Identyfikator klienta można uzyskać na karcie **Zaawansowane** w obszarze **Ustawienia usługi Azure Active Directory** w portalu.

* Zamień **Identyfikator tutaj, wstawianie klienta w-** identyfikator klienta, skopiowane z klientami aplikacji.

* Zamień **Wstawianie PRZEKIERUJ-URI-tutaj** witryny _/.auth/login/done_ punktu końcowego, przy użyciu schematu HTTPS. Ta wartość powinna być podobna do _https://contoso.azurewebsites.net/.auth/login/done_.

        private AuthenticationContext mContext;

        private void authenticate() {
            String authority = "INSERT-AUTHORITY-HERE";
            String resourceId = "INSERT-RESOURCE-ID-HERE";
            String clientId = "INSERT-CLIENT-ID-HERE";
            String redirectUri = "INSERT-REDIRECT-URI-HERE";
            try {
                mContext = new AuthenticationContext(this, authority, true);
                mContext.acquireToken(this, resourceId, clientId, redirectUri, PromptBehavior.Auto, "", callback);
            } catch (Exception exc) {
                exc.printStackTrace();
            }
        }

        private AuthenticationCallback<AuthenticationResult> callback = new AuthenticationCallback<AuthenticationResult>() {
            @Override
            public void onError(Exception exc) {
                if (exc instanceof AuthenticationException) {
                    Log.d(TAG, "Cancelled");
                } else {
                    Log.d(TAG, "Authentication error:" + exc.getMessage());
                }
            }

            @Override
            public void onSuccess(AuthenticationResult result) {
                if (result == null || result.getAccessToken() == null
                        || result.getAccessToken().isEmpty()) {
                    Log.d(TAG, "Token is empty");
                } else {
                    try {
                        JSONObject payload = new JSONObject();
                        payload.put("access_token", result.getAccessToken());
                        ListenableFuture<MobileServiceUser> mLogin = mClient.login("aad", payload.toString());
                        Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
                            @Override
                            public void onFailure(Throwable exc) {
                                exc.printStackTrace();
                            }
                            @Override
                            public void onSuccess(MobileServiceUser user) {
                                Log.d(TAG, "Login Complete");
                            }
                        });
                    }
                    catch (Exception exc){
                        Log.d(TAG, "Authentication error:" + exc.getMessage());
                    }
                }
            }
        };

        @Override
        protected void onActivityResult(int requestCode, int resultCode, Intent data) {
            super.onActivityResult(requestCode, resultCode, data);
            if (mContext != null) {
                mContext.onActivityResult(requestCode, resultCode, data);
            }
        }

## <a name="how-to-add-push-notification-to-your-app"></a>Jak: Dodawanie powiadomień wypychanych do aplikacji

Możesz [zapoznanie się z informacjami] [ 6] , który opisuje jak Microsoft Azure powiadomienie koncentratorów obsługuje szeroką gamę powiadomienia wypychane.  W [tym samouczku][5], powiadomienia wypychane są wysyłane do wszystkich urządzeń każdorazowo po wstawieniu rekordu.

## <a name="how-to-add-offline-sync-to-your-app"></a>Jak: Dodawanie synchronizacja w trybie offline do aplikacji

Samouczek Szybki Start zawiera kod, który wykonuje synchronizacja w trybie offline. Poszukaj kodu prefiksem komentarzy:

    // Offline Sync

Uncommenting następujące wiersze kodu można zaimplementować synchronizacja w trybie offline, a następnie możesz dodać kod podobny do inny kod aplikacji Mobile.

##<a name="customizing"></a>Jak: dostosowywanie klienta

Istnieje kilka sposobów, którą można dostosować domyślne zachowanie klienta.

### <a name="headers"></a>Jak: dostosowywanie żądanie nagłówków

Konfigurowanie **ServiceFilter** dodać niestandardowy nagłówek HTTP do każdego żądania:

    private class CustomHeaderFilter implements ServiceFilter {

        @Override
        public ListenableFuture<ServiceFilterResponse> handleRequest(
                    ServiceFilterRequest request,
                    NextServiceFilterCallback next) {

            runOnUiThread(new Runnable() {

                @Override
                public void run() {
                    request.addHeader("My-Header", "Value");                    }
            });

            SettableFuture<ServiceFilterResponse> result = SettableFuture.create();
            try {
                ServiceFilterResponse response = next.onNext(request).get();
                result.set(response);
            } catch (Exception exc) {
                result.setException(exc);
            }
        }

### <a name="serialization"></a>Jak: dostosowywanie szeregowania

Klient zakłada, że nazw tabel, nazw kolumn i danych typów do wewnętrznej bazy danych wszystkie odpowiadać dokładnie obiekty danych zdefiniowane w kliencie programu. Może to być dowolna liczba powodów, dlaczego nazwy klienta i serwera mogą nie odpowiadać. W aktualnego scenariusza można wykonać następujące rodzaje dostosowania:

- Nazwy kolumn używanych w tabeli aplikacji usług nie zgodne z nazwami, używanym w kliencie.
- Za pomocą aplikacji usługi tabeli, która ma inną nazwę niż zajęć, które mapowania w kliencie.
- Włącz automatyczne właściwość wielkości liter.
- Dodawanie złożonej właściwości do obiektu.

### <a name="columns"></a>Jak: Mapa innego klienta i serwera nazw

Załóżmy, że kod języka Java klienta korzysta z standardowe nazwy stylu Java **ToDoItem** właściwości obiektu, takie jak następujące właściwości:

- mId
- mText
- mComplete
- Rocz.przych.m

Szeregować nazw klientów na nazwy JSON, które są zgodne z nazw kolumn w tabeli **ToDoItem** na serwerze. Poniższy kod korzysta z [gson] [ 3] biblioteki, aby dodać adnotację właściwości:

    @com.google.gson.annotations.SerializedName("text")
    private String mText;

    @com.google.gson.annotations.SerializedName("id")
    private int mId;

    @com.google.gson.annotations.SerializedName("complete")
    private boolean mComplete;

    @com.google.gson.annotations.SerializedName("duration")
    private String mDuration;

### <a name="table"></a>Jak: mapowanie nazw tabel różnych między klientem a wewnętrznej bazy danych

Mapowanie nazwy tabeli klienta na różnych usług mobilnych nazwę tabeli przy użyciu zastępowanie [getTable()] [ 4] metody:

    mToDoTable = mClient.getTable("ToDoItemBackup", ToDoItem.class);

### <a name="conversions"></a>Jak: zautomatyzować mapowaniami nazw kolumn

Możesz określić strategię konwersji zastosowany do wszystkich kolumn przy użyciu [gson] [ 3] interfejsu API. Biblioteka Android klienta używa [gson] [ 3] w tle szeregować obiektów Java JSON danych przed wysłaniem danych do Azure aplikacji usługi.  Poniższy kod używa metody **setFieldNamingStrategy()** , aby ustawić strategii. W tym przykładzie spowoduje usunięcie litery ("m"), a następnie małą następnego znaku dla każdej nazwy pola. Na przykład go chcesz przekształcić "mId" w "identyfikator".

    client.setGsonBuilder(
        MobileServiceClient
        .createMobileServiceGsonBuilder()
        .setFieldNamingStrategy(new FieldNamingStrategy() {
            public String translateName(Field field) {
                String name = field.getName();
                return Character.toLowerCase(name.charAt(1))
                    + name.substring(2);
                }
            });

Kod muszą być wykonane przed użyciem **MobileServiceClient**.

### <a name="complex"></a>Jak: przechowywanie właściwości obiektu lub tablicy do tabeli

Nasze przykładowe szeregowania mają pory, zaangażowanie typy pierwotne, takie jak liczby całkowite i ciągi.  Typy pierwotne szeregować łatwe do JSON.  Jeśli chcemy Dodawanie złożonej obiekt, który nie automatycznie szeregować do JSON, trzeba podać metodę szeregowania JSON.  Aby zobaczyć przykład o podanie niestandardowej szeregowania JSON, Przejrzyj wpis w blogu [szeregowania dostosowywanie przy użyciu biblioteki gson w kliencie Mobile usługi Android][2].

<!-- Anchors. -->

[What is Mobile Services]: #what-is
[Concepts]: #concepts
[How to: Create the Mobile Services client]: #create-client
[How to: Create a table reference]: #instantiating
[The API structure]: #api
[How to: Query data from a mobile service]: #querying
[Zwraca wszystkie elementy]: #showAll
[Filtrowanie danych zwróconych]: #filtering
[Sortowanie zwrócił dane.]: #sorting
[Zwróć dane na stronach]: #paging
[Wybierz określone kolumny]: #selecting
[How to: Concatenate query methods]: #chaining
[How to: Bind data to the user interface]: #binding
[How to: Define the layout]: #layout
[How to: Define the adapter]: #adapter
[How to: Use the adapter]: #use-adapter
[How to: Insert data into a mobile service]: #inserting
[How to: update data in a mobile service]: #updating
[How to: Delete data in a mobile service]: #deleting
[How to: Look up a specific item]: #lookup
[How to: Work with untyped data]: #untyped
[How to: Authenticate users]: #authentication
[Cache authentication tokens]: #caching
[How to: Handle errors]: #errors
[How to: Design unit tests]: #tests
[How to: Customize the client]: #customizing
[Customize request headers]: #headers
[Customize serialization]: #serialization
[Next Steps]: #next-steps
[Setup and Prerequisites]: #setup

<!-- Images. -->

<!-- URLs. -->
[Get started with Azure Mobile Apps]: app-service-mobile-android-get-started.md
[ASCII control codes C0 and C1]: http://en.wikipedia.org/wiki/Data_link_escape_character#C1_set
[Mobile Services SDK for Android]: http://go.microsoft.com/fwlink/p/?LinkID=717033
[Azure portal]: https://portal.azure.com
[Wprowadzenie do uwierzytelniania]: app-service-mobile-android-get-started-users.md
[1]: http://google-gson.googlecode.com/svn/trunk/gson/docs/javadocs/com/google/gson/JsonObject.html
[2]: http://hashtagfail.com/post/44606137082/mobile-services-android-serialization-gson
[3]: http://go.microsoft.com/fwlink/p/?LinkId=290801
[4]: http://go.microsoft.com/fwlink/p/?LinkId=296840
[5]: app-service-mobile-android-get-started-push.md
[6]: ../notification-hubs/notification-hubs-push-notification-overview.md#integration-with-app-service-mobile-apps
[7]: app-service-mobile-android-get-started-users.md#cache-tokens
[8]: http://azure.github.io/azure-mobile-apps-android-client/com/microsoft/windowsazure/mobileservices/table/MobileServiceTable.html
[9]: http://azure.github.io/azure-mobile-apps-android-client/com/microsoft/windowsazure/mobileservices/MobileServiceClient.html
[10]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[11]: app-service-mobile-node-backend-how-to-use-server-sdk.md
[12]: http://azure.github.io/azure-mobile-apps-android-client/
[13]: app-service-mobile-android-get-started.md#create-a-new-azure-mobile-app-backend
[14]: http://go.microsoft.com/fwlink/p/?LinkID=717034
[15]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#how-to-define-a-table-controller
[16]: app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations
[Przyszłość]: http://developer.android.com/reference/java/util/concurrent/Future.html
[AsyncTask]: http://developer.android.com/reference/android/os/AsyncTask.html