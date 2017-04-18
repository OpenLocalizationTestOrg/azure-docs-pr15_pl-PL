<properties 
    pageTitle="Programowanie DocumentDB: procedur przechowywanych, wyzwalacze bazy danych i funkcji zdefiniowanych przez użytkownika | Microsoft Azure" 
    description="Dowiedz się, jak za pomocą DocumentDB Pisanie procedur składowanych, wyzwalacze bazy danych i funkcje zdefiniowane przez użytkownika (UDF) w języku JavaScript. Uzyskaj wskazówki programing bazy danych i nie tylko." 
    keywords="Wyzwalacze bazy danych, procedura składowana, procedura składowana, program bazy danych, sproc, documentdb, azure, platformy Microsoft azure"
    services="documentdb" 
    documentationCenter="" 
    authors="aliuy" 
    manager="jhubbard" 
    editor="mimig"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/14/2016" 
    ms.author="andrl"/>

# <a name="documentdb-server-side-programming-stored-procedures-database-triggers-and-udfs"></a>Programowania po stronie serwera DocumentDB: procedur przechowywanych, wyzwalacze bazy danych i funkcji zdefiniowanych przez użytkownika

Dowiedz się, jak zintegrowane języka Azure DocumentDB, transakcji wykonywanie kodu JavaScript umożliwia programistom pisanie **procedur składowanych**, **wyzwalaczami** i **funkcje (UDF) zdefiniowane przez użytkownika** oryginalnie w JavaScript. Pozwala na pisanie logiki aplikacji programu bazy danych, który może zostać wysłane i uruchamiane bezpośrednio na partycje miejsca do magazynowania bazy danych 

Zaleca się wprowadzenie obserwując poniższym klipie wideo, gdzie Liu osoby o imieniu Andrzej zawiera krótkie wprowadzenie do programowania modelu bazy danych po stronie serwera i DocumentDB. 

> [AZURE.VIDEO azure-demo-a-quick-intro-to-azure-documentdbs-server-side-javascript]

Następnie wróć do niniejszego artykułu, której poznasz odpowiedzi na poniższe pytania:  

- Jak pisać procedura składowana, wyzwalacz lub UDF za pomocą języka JavaScript?
- Jak DocumentDB zagwarantować kwasu?
- Jak działają transakcji w DocumentDB
- Co to są wyzwalane przed i po wyzwalane i jak jedną pisać?
- Jak zarejestrować i wykonać procedury przechowywanej, wyzwalacza lub UDF w sposób RESTful za pośrednictwem protokołu HTTP?
- Jakie są dostępne na utworzenie i wykonywanie SDK DocumentDB przechowywane procedur, wyzwalaczy i funkcji zdefiniowanych przez użytkownika?

## <a name="introduction-to-stored-procedure-and-udf-programming"></a>Wprowadzenie do programowania UDF i procedura składowana

Ta metoda *"JavaScript jako nowoczesne dzień T-SQL"* zwolnienia deweloperów aplikacji z złożoności niezgodności typów systemu i technologii mapowania relacyjnego obiektu. Ponadto wprowadzono numer wewnętrzny korzyści, które może być wykorzystana do utworzenia zaawansowanych aplikacji:  

-   **Procedurach logiczny:** JavaScript jako wysokiego poziomu język programowania, zawiera zaawansowanych, znanych funkcji do wyrażania warunków logicznych. Można wykonywać złożone sekwencji operacji bliżej do danych.

-   **Transakcje Atomowej:** DocumentDB gwarantuje, że operacje bazy danych wykonywane wewnątrz pojedynczej procedury składowanej lub wyzwalacza Atomowej. Dzięki temu aplikacja łączenie powiązanych operacji w ramach jednej partii, tak aby każdy z nich powiodła się albo żaden z nich nie powiodła się. 

-   **Wydajności:** Fakt, że JSON leżą są mapowane do systemu typu języka Javascript i jest również podstawowa jednostka przechowywania w DocumentDB umożliwia dla liczby optymalizacje, takich jak opóźnieniem materialization JSON dokumentów w puli buforów i nadawania dostępne na żądanie na wykonywanie kodu. Istnieje więcej zalet wydajności skojarzone z wysyłki reguł biznesowych do bazy danych:
    -   Tworzeniu partii — deweloperów można grupować operacje, takie jak zostanie wstawiony i przesyłanie ich zbiorczo. Opóźnienie ruchu sieciowego kosztów i ogólnych sklepu, aby utworzyć osobne transakcje zmniejszone. 
    -   Wstępna kompilacja — DocumentDB precompiles procedur składowanych, wyzwalaczami i funkcje zdefiniowane przez użytkownika (UDF), aby uniknąć JavaScript kompilacji koszt dla każdego wywołania. Ogólnych tworzenia kod bajt procedurach logiczny jest amortyzowanego na wartość minimalna.
    -   Harmonogram — wiele operacji potrzebna stronie efekt ("wyzwalacza") potencjalnie polega na wykonywaniu jednego lub wielu operacji pomocniczej ze sklepu. Oprócz niepodzielność to jest więcej performant po przeniesieniu na serwerze. 
-   **Hermetyzacji:** Procedury składowane może służyć do grupy reguł biznesowych w jednym miejscu. To daje dwie następujące korzyści:
    -   Dodaje warstwy abstrakcji u góry nieprzetworzonych danych, co umożliwia architektom danych są obsługiwane w swoich aplikacjach niezależnie od danych. Jest to szczególnie korzystnych, gdy dane są schematu bez, ze względu na łamliwa założeń, które może być konieczne wypiekany w aplikacji, mając na zajęcie się dane bezpośrednio.  
    -   Ten abstrakcji umożliwia przedsiębiorstw zabezpieczyć dane o jego usprawnienie dostęp za pomocą skryptów.  

Tworzenie i wykonywanie wyzwalacze bazy danych, procedura składowana i operatorów kwerendę niestandardową jest obsługiwane za pośrednictwem [Interfejsu API usługi REST](https://msdn.microsoft.com/library/azure/dn781481.aspx), [DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio/releases)i [klienta SDK](documentdb-sdk-dotnet.md) w wiele platform, takich jak .NET, Node.js i JavaScript.

**Samouczku [SDK Node.js z ze zobowiązania pytań](http://azure.github.io/azure-documentdb-node-q/) ** , aby przedstawić składni i zastosowania procedur składowanych, wyzwalaczami i funkcji zdefiniowanych przez użytkownika.   

## <a name="stored-procedures"></a>Procedury składowane

### <a name="example-write-a-simple-stored-procedure"></a>Przykład: Pisanie prostej procedury składowanej 
Zacznijmy od prostej procedury składowanej, która zwraca odpowiedź "Witaj świecie".

    var helloWorldStoredProc = {
        id: "helloWorld",
        body: function () {
            var context = getContext();
            var response = context.getResponse();
    
            response.setBody("Hello, World");
        }
    }


Procedury składowane są rejestrowane na zbiór i może działać na dokumentu i załącznik istnieje w tej kolekcji. Poniższy fragment pokazano, jak zarejestrować procedury helloWorld przechowywane w kolekcji. 

    // register the stored procedure
    var createdStoredProcedure;
    client.createStoredProcedureAsync('dbs/testdb/colls/testColl', helloWorldStoredProc)
        .then(function (response) {
            createdStoredProcedure = response.resource;
            console.log("Successfully created stored procedure");
        }, function (error) {
            console.log("Error", error);
        });


Po procedura składowana jest zarejestrowany, możemy ją wykonać kolekcji oraz Czytaj wyniki ponownie po stronie klienta. 

    // execute the stored procedure
    client.executeStoredProcedureAsync('dbs/testdb/colls/testColl/sprocs/helloWorld')
        .then(function (response) {
            console.log(response.result); // "Hello, World"
        }, function (err) {
            console.log("Error", error);
        });


Obiekt kontekstu zapewnia dostęp do wszystkich operacji, które mogą być wykonywane ilość miejsca do magazynowania DocumentDB, a także dostęp do obiektów i odpowiadania na wezwania. W tym przypadku obiektu odpowiedź możemy służy do ustawiania treści odpowiedź, która została wysłana do klienta. Aby uzyskać więcej informacji zapoznaj się z [serwera DocumentDB JavaScript dokumentacji SDK](http://azure.github.io/azure-documentdb-js-server/).  

Pozwól nam rozwiń w tym przykładzie i dodać więcej bazy danych pokrewnych funkcji procedury składowanej. Procedur składowanych można utworzyć, aktualizowanie, przeczytaj, kwerendy i usuwać dokumenty i załączniki w kolekcji.    

### <a name="example-write-a-stored-procedure-to-create-a-document"></a>Przykład: Pisanie procedura składowana, aby utworzyć dokument 
Wstawkę kodu następnej pokazano, jak za pomocą obiekt kontekstu interakcji z zasobami DocumentDB.

    var createDocumentStoredProc = {
        id: "createMyDocument",
        body: function createMyDocument(documentToCreate) {
            var context = getContext();
            var collection = context.getCollection();

            var accepted = collection.createDocument(collection.getSelfLink(),
                  documentToCreate,
                  function (err, documentCreated) {
                      if (err) throw new Error('Error' + err.message);
                      context.getResponse().setBody(documentCreated.id)
                  });
            if (!accepted) return;
        }
    }


Ta procedura składowana przyjmuje jako documentToCreate wprowadzania danych, treści dokumentu, który może zostać utworzone w bieżącym zbiorze. Wszystkie operacje są asynchroniczne i zależą od zwrotne funkcji języka JavaScript. Funkcja zwrotnego ma dwa parametry, jedną dla obiektu błędu w przypadku, gdy kończy się niepowodzeniem i jedną dla utworzony obiekt. Wewnątrz zwrotnego użytkowników można obsługiwać wyjątku lub błąd. W przypadku, gdy nie określono zwrotnego i występuje błąd, DocumentDB runtime zgłasza błąd.   

W powyższym przykładzie wywołanie zwrotne wygeneruje błąd, jeśli nie można operacji. W przeciwnym razie Ustawia identyfikator dokumentu utworzonego w treści odpowiedzi do klienta. Poniżej przedstawiono, jak ta procedura składowana jest wykonywane przy użyciu parametrów wejściowych.

    // register the stored procedure
    client.createStoredProcedureAsync('dbs/testdb/colls/testColl', createDocumentStoredProc)
        .then(function (response) {
            var createdStoredProcedure = response.resource;
    
            // run stored procedure to create a document
            var docToCreate = {
                id: "DocFromSproc",
                book: "The Hitchhiker’s Guide to the Galaxy",
                author: "Douglas Adams"
            };
    
            return client.executeStoredProcedureAsync('dbs/testdb/colls/testColl/sprocs/createMyDocument',
                  docToCreate);
        }, function (error) {
            console.log("Error", error);
        })
    .then(function (response) {
        console.log(response); // "DocFromSproc"
    }, function (error) {
        console.log("Error", error);
    });

    
Należy zauważyć, że ta procedura składowana można zmodyfikować, aby pobrać tablicę organów dokumentu jako danych wejściowych i są tworzone w tym samym wykonywanie procedury składowanej zamiast wielu żądania sieciowe Tworzenie każdej z nich osobno. To może służyć do wykonania importer wydajność zbiorcze dla DocumentDB (omówiono w dalszej części tego samouczka).   

Opisanym przykładzie pokazano, jak za pomocą procedur składowanych. Omówimy następujące zagadnienia wyzwalaczami i funkcje zdefiniowane przez użytkownika (UDF) w dalszej części samouczka.

## <a name="database-program-transactions"></a>Transakcje bazy danych programu
Transakcji w bazie danych typowe można zdefiniować za pomocą sekwencji operacji wykonywanych jako całość logiczne pracy. Każdej transakcji znajdują się **gwarancje kwasu**. KWAS jest znane akronim, zawiera cztery właściwości — niepodzielność, spójności, izolacji i odporność.  

Krótko, niepodzielność gwarantuje, że wszystkie prac wewnątrz transakcji jest traktowany jako całość miejsce, w którym albo wszystkie zostanie zatwierdzony lub Brak. Spójności służy do sprawdzenia, czy dane są zawsze dobry stan wewnętrzny na wielu transakcji. Izolacji gwarantuje, że żadne dwie transakcje utrudniać ze sobą — zazwyczaj, większość komercyjnych systemy zapewniają wiele poziomów izolacji, które mogą być używane w zależności od potrzeb aplikacji. Wytrzymałości zapewnia zmiany stara w bazie danych będzie zawsze obecne.   

W DocumentDB JavaScript jest obsługiwana w tym samym miejscu pamięci jako bazę danych. W związku z tym żądań w ramach procedur składowanych i wyzwalacze są wykonywane w tym samym zakresie sesji bazy danych. Dzięki temu DocumentDB na zagwarantowanie kwasu wszystkie operacje, które są częścią jednego przechowywane procedury wyzwalacza. Należy rozważyć następujące definicji procedura składowana:

    // JavaScript source code
    var exchangeItemsSproc = {
        name: "exchangeItems",
        body: function (playerId1, playerId2) {
            var context = getContext();
            var collection = context.getCollection();
            var response = context.getResponse();
    
            var player1Document, player2Document;
    
            // query for players
            var filterQuery = 'SELECT * FROM Players p where p.id  = "' + playerId1 + '"';
            var accept = collection.queryDocuments(collection.getSelfLink(), filterQuery, {},
                function (err, documents, responseOptions) {
                    if (err) throw new Error("Error" + err.message);
    
                    if (documents.length != 1) throw "Unable to find both names";
                    player1Document = documents[0];
    
                    var filterQuery2 = 'SELECT * FROM Players p where p.id = "' + playerId2 + '"';
                    var accept2 = collection.queryDocuments(collection.getSelfLink(), filterQuery2, {},
                        function (err2, documents2, responseOptions2) {
                            if (err2) throw new Error("Error" + err2.message);
                            if (documents2.length != 1) throw "Unable to find both names";
                            player2Document = documents2[0];
                            swapItems(player1Document, player2Document);
                            return;
                        });
                    if (!accept2) throw "Unable to read player details, abort ";
                });
    
            if (!accept) throw "Unable to read player details, abort ";
    
            // swap the two players’ items
            function swapItems(player1, player2) {
                var player1ItemSave = player1.item;
                player1.item = player2.item;
                player2.item = player1ItemSave;
    
                var accept = collection.replaceDocument(player1._self, player1,
                    function (err, docReplaced) {
                        if (err) throw "Unable to update player 1, abort ";
    
                        var accept2 = collection.replaceDocument(player2._self, player2,
                            function (err2, docReplaced2) {
                                if (err) throw "Unable to update player 2, abort"
                            });
    
                        if (!accept2) throw "Unable to update player 2, abort";
                    });
    
                if (!accept) throw "Unable to update player 1, abort";
            }
        }
    }
    
    // register the stored procedure in Node.js client
    client.createStoredProcedureAsync(collection._self, exchangeItemsSproc)
        .then(function (response) {
            var createdStoredProcedure = response.resource;
        }
    );

Ta procedura składowana używa transakcji w aplikacji dla gier do elementów handlu między dwoma uczestnikami jednej operacji. Procedura składowana próbuje czytanie dwóch dokumentów, które każdego odpowiadające identyfikatorom odtwarzacza przekazany jako argument. Jeśli oba dokumenty odtwarzacza znajdują się, a następnie procedura składowana aktualizuje dokumenty przez zamienianie ich elementów. Jeśli wystąpią błędy wzdłuż sposób, zgłasza wyjątek JavaScript, który niejawnie przerwana transakcji.

Zarejestrowanej zbioru procedura składowana przed to zbiór jedną partycją, a następnie transakcji jest ograniczone do wszystkich docuemnts w tym zbiorze. Jeśli jest podzielona kolekcji, procedur składowanych są wykonywane w zakresie transakcji klucza jedną partycją. Każdy przechowywaną wykonywanie procedury następnie musi zawierać wartość klucza partition odpowiadające zakres transakcji musi być uruchamiana. Aby uzyskać więcej informacji zobacz [DocumentDB podziału](documentdb-partition-data.md).

### <a name="commit-and-rollback"></a>Przekazywania i wycofywania
Transakcje są mocno i oryginalnie zintegrowane model programowania kodu JavaScript w DocumentDB. Wewnątrz funkcji JavaScript wszystkie operacje są automatycznie zawinięty w ramach jednej transakcji. Po zakończeniu JavaScript bez każdego wyjątku, operacje do bazy danych są zatwierdzony. W rzeczywistości instrukcje "Rozpocząć transakcji" i "Zatwierdzenie transakcji" w relacyjnych baz danych są zawarte w DocumentDB.  
 
W przypadku każdego wyjątku, który jest propagowana skryptu obsługi języka JavaScript w DocumentDB spowoduje przywrócenie całej transakcji. Jak pokazano w przykładzie wcześniejszych, zgłaszanie wyjątku odpowiada skuteczne "Transakcji WYCOFYWANIA" w DocumentDB.
 
### <a name="data-consistency"></a>Spójności danych
Procedur składowanych i wyzwalaczy są zawsze wykonywane w replice podstawowego kolekcji DocumentDB. Dzięki temu przechowywanie odczytuje z wewnątrz procedury oferty silnych spójności. Zapytań za pomocą funkcji zdefiniowanych przez użytkownika mogą być wykonywane na podstawowych lub dowolny pomocniczej replice, ale firma Microsoft upewnij się spotkać poziomu spójności wymagane przez wybranie odpowiednich replice.

## <a name="bounded-execution"></a>Wykonanie ograniczonych
Wszystkie operacje DocumentDB muszą wykonać na serwerze określony limit czasu żądania. To ograniczenie dotyczy również funkcji JavaScript (procedur składowanych, wyzwalaczami i funkcji zdefiniowanych przez użytkownika). Jeśli aplikacja nie zostanie ukończona z tego terminu, transakcja jest wycofana. Funkcje języka JavaScript musi Zakończ w terminie lub wdrożenia modelu kontynuacji podstawie do partii Życiorys wykonanie.  

W celu uproszczenia rozwoju procedur składowanych i wyzwalaczy obsługę terminów, wszystkie funkcje w obszarze obiektu kolekcji (w przypadku tworzenia, odczytu, zamienianie i usuwanie dokumentów i załączniki) zwracają wartość logiczną, która reprezentuje czy zakończy tej operacji. Jeśli ten parametr na wartość ma wartość FAŁSZ, jest wskazanie, że limit czasu jest zbliża się termin wygaśnięcia i że procedurę należy zawijanie się wykonanie.  Operacje kolejce przed pierwszym operacja niezaakceptowanych magazynu zapewniające wykonać, jeśli procedura składowana kończy w czasie, a nie w kolejce żądań więcej.  

Funkcje języka JavaScript jest również ograniczone na zużycie zasobów. DocumentDB zastrzega sobie przepustowość na zbiór na podstawie rozmiaru ustanawianie konta bazy danych. Przepustowość jest wyrażona za pomocą znormalizowaną jednostka Procesora i pamięci zużycie Jo o nazwie jednostki żądania lub RUs. Funkcje języka JavaScript potencjalnie korzystać z dużą liczbą RUs w krótkim czasie i może zostać wyświetlony stawek, ograniczone po osiągnięciu limitu kolekcji. Intensywnie procedur składowanych zasobów mogą być również poddana kwarantannie, zapewniające dostępność operacji pierwotny bazy danych.  

### <a name="example-bulk-importing-data-into-a-database-program"></a>Przykład: Zbiorcze importowanie danych do programu bazy danych
Poniżej przedstawiono przykładowy procedura przechowywana, która jest zapisywany importu zbiorczego dokumentów do kolekcji. Należy zauważyć, jak procedura składowana obsługuje ograniczone wykonanie, zaznaczając pole wyboru wartością logiczną zwrócenia wartości na podstawie createDocument, a następnie używa liczba dokumentów wstawiony w każdej wywołania procedury składowanej do śledzenia i wznowienie postępu w partii.

    function bulkImport(docs) {
        var collection = getContext().getCollection();
        var collectionLink = collection.getSelfLink();
    
        // The count of imported docs, also used as current doc index.
        var count = 0;
    
        // Validate input.
        if (!docs) throw new Error("The array is undefined or null.");
    
        var docsLength = docs.length;
        if (docsLength == 0) {
            getContext().getResponse().setBody(0);
        }
    
        // Call the create API to create a document.
        tryCreate(docs[count], callback);
    
        // Note that there are 2 exit conditions:
        // 1) The createDocument request was not accepted. 
        //    In this case the callback will not be called, we just call setBody and we are done.
        // 2) The callback was called docs.length times.
        //    In this case all documents were created and we don’t need to call tryCreate anymore. Just call setBody and we are done.
        function tryCreate(doc, callback) {
            var isAccepted = collection.createDocument(collectionLink, doc, callback);
    
            // If the request was accepted, callback will be called.
            // Otherwise report current count back to the client, 
            // which will call the script again with remaining set of docs.
            if (!isAccepted) getContext().getResponse().setBody(count);
        }
    
        // This is called when collection.createDocument is done in order to process the result.
        function callback(err, doc, options) {
            if (err) throw err;
    
            // One more document has been inserted, increment the count.
            count++;
    
            if (count >= docsLength) {
                // If we created all documents, we are done. Just set the response.
                getContext().getResponse().setBody(count);
            } else {
                // Create next document.
                tryCreate(docs[count], callback);
            }
        }
    }

## <a id="trigger"></a>Wyzwalacze bazy danych
### <a name="database-pre-triggers"></a>Wyzwalacze wstępnej bazy danych
DocumentDB zawiera wyzwalacze, które zostały wykonane lub wyzwalane przez operację w dokumencie. Na przykład można określić wyzwalacz przed, podczas tworzenia dokumentu — ten przed wyzwalacz będą uruchamiane przed dokument jest utworzony. Oto przykładowy sposób przed wyzwalaczy może służyć do sprawdzania poprawności właściwości dokumentu, który jest tworzony:

    var validateDocumentContentsTrigger = {
        name: "validateDocumentContents",
        body: function validate() {
            var context = getContext();
            var request = context.getRequest();
    
            // document to be created in the current operation
            var documentToCreate = request.getBody();
    
            // validate properties
            if (!("timestamp" in documentToCreate)) {
                var ts = new Date();
                documentToCreate["my timestamp"] = ts.getTime();
            }
    
            // update the document that will be created
            request.setBody(documentToCreate);
        },
        triggerType: TriggerType.Pre,
        triggerOperation: TriggerOperation.Create
    }


I odpowiedni kod rejestracji po stronie klienta Node.js wyzwalacza:

    // register pre-trigger
    client.createTriggerAsync(collection.self, validateDocumentContentsTrigger)
        .then(function (response) {
            console.log("Created", response.resource);
            var docToCreate = {
                id: "DocWithTrigger",
                event: "Error",
                source: "Network outage"
            };
    
            // run trigger while creating above document 
            var options = { preTriggerInclude: "validateDocumentContents" };
    
            return client.createDocumentAsync(collection.self,
                  docToCreate, options);
        }, function (error) {
            console.log("Error", error);
        })
    .then(function (response) {
        console.log(response.resource); // document with timestamp property added
    }, function (error) {
        console.log("Error", error);
    });


Wyzwalacze przed nie może mieć wszystkich jej parametrów wejściowych. Obiekt request może służyć do manipulowania komunikatu żądania skojarzonego z tej operacji. W tym miejscu przed wyzwalacza jest uruchamiany z tworzeniem dokumentu, a treść wiadomości żądania zawiera dokumentu do utworzenia w formacie JSON.   

Jeśli zarejestrowano wyzwalaczy, użytkownicy mogą określać operacji, które można uruchamiać z. Ten wyzwalacz został utworzony przy użyciu TriggerOperation.Create, co oznacza, że następujące czynności nie jest dozwolona.

    var options = { preTriggerInclude: "validateDocumentContents" };
    
    client.replaceDocumentAsync(docToReplace.self,
                  newDocBody, options)
    .then(function (response) {
        console.log(response.resource);
    }, function (error) {
        console.log("Error", error);
    });
    
    // Fails, can’t use a create trigger in a replace operation

### <a name="database-post-triggers"></a>Wyzwalacze po bazy danych
Po wyzwalacze, takich jak przed wyzwalacze są skojarzone z operacji nad dokumentem i nie ma żadnych parametrów wejściowych. Uruchom **po** wykonaniu operacji i masz dostęp do wiadomości odpowiedzi, które są wysyłane do klienta.   

W poniższym przykładzie pokazano po wyzwalaczy w działaniu:

    var updateMetadataTrigger = {
        name: "updateMetadata",
        body: function updateMetadata() {
            var context = getContext();
            var collection = context.getCollection();
            var response = context.getResponse();
    
            // document that was created
            var createdDocument = response.getBody();
    
            // query for metadata document
            var filterQuery = 'SELECT * FROM root r WHERE r.id = "_metadata"';
            var accept = collection.queryDocuments(collection.getSelfLink(), filterQuery,
                updateMetadataCallback);
            if(!accept) throw "Unable to update metadata, abort";
     
            function updateMetadataCallback(err, documents, responseOptions) {
                if(err) throw new Error("Error" + err.message);
                         if(documents.length != 1) throw 'Unable to find metadata document';
                         
                         var metadataDocument = documents[0];
                         
                         // update metadata
                         metadataDocument.createdDocuments += 1;
                         metadataDocument.createdNames += " " + createdDocument.id;
                         var accept = collection.replaceDocument(metadataDocument._self,
                               metadataDocument, function(err, docReplaced) {
                                      if(err) throw "Unable to update metadata, abort";
                               });
                         if(!accept) throw "Unable to update metadata, abort";
                         return;                    
            }                                                                                           
        },
        triggerType: TriggerType.Post,
        triggerOperation: TriggerOperation.All
    }


Jak pokazano w poniższym przykładzie można rejestrować wyzwalacza.

    // register post-trigger
    client.createTriggerAsync('dbs/testdb/colls/testColl', updateMetadataTrigger)
        .then(function(createdTrigger) { 
            var docToCreate = { 
                name: "artist_profile_1023",
                artist: "The Band",
                albums: ["Hellujah", "Rotators", "Spinning Top"]
            };
    
            // run trigger while creating above document 
            var options = { postTriggerInclude: "updateMetadata" };
        
            return client.createDocumentAsync(collection.self,
                  docToCreate, options);
        }, function(error) {
            console.log("Error" , error);
        })
    .then(function(response) {
        console.log(response.resource); 
    }, function(error) {
        console.log("Error" , error);
    });


Ten wyzwalacz kwerendy dla dokumentu metadanych i aktualizuje go ze szczegółami nowo utworzonego dokumentu.  

Jedyne, który należy zwrócić uwagę to realizacji **transakcji** wyzwalaczy w DocumentDB. Ten wyzwalacz po działa w ramach tej samej transakcji, jak tworzenie oryginalny dokument. W związku z tym jeśli firma Microsoft zostać zgłoszony wyjątek z po wyzwalacza (Powiedz, jeśli nie można zaktualizować dokument metadanych), cała transakcja zakończy się niepowodzeniem i wycofana. Dokument nie zostanie utworzony, a zostanie zwrócony wyjątek.  

##<a id="udf"></a>Funkcje zdefiniowane przez użytkownika
Funkcje zdefiniowane przez użytkownika (UDF) są używane do rozszerzenie gramatyki języka kwerend DocumentDB SQL i implementowanie niestandardowe reguły biznesowe. Może być tylko wywoływana z wewnątrz kwerendy. Ta osoba nie mają dostępu do obiektu kontekstu i są przeznaczone do użycia jako tylko do obliczeń języka JavaScript. W związku z tym funkcji zdefiniowanych przez użytkownika można uruchamiać na pomocniczym repliki usługi DocumentDB.  
 
Poniższy przykładowy tworzy UDF obliczyć podatek od dochodu oparte na stawkach różnych nawiasów dochód, a następnie używa go wewnątrz kwerendy, aby znaleźć wszystkie osoby, które spłacona więcej niż 20 000 zł w podatków.

    var taxUdf = {
        name: "tax",
        body: function tax(income) {
    
            if(income == undefined) 
                throw 'no input';
    
            if (income < 1000) 
                return income * 0.1;
            else if (income < 10000) 
                return income * 0.2;
            else
                return income * 0.4;
        }
    }


Następnie można UDF w kwerendach, podobnie jak w następującym przykładzie:

    // register UDF
    client.createUserDefinedFunctionAsync('dbs/testdb/colls/testColl', taxUdf)
        .then(function(response) { 
            console.log("Created", response.resource);
    
            var query = 'SELECT * FROM TaxPayers t WHERE udf.tax(t.income) > 20000'; 
            return client.queryDocuments('dbs/testdb/colls/testColl',
                   query).toArrayAsync();
        }, function(error) {
            console.log("Error" , error);
        })
    .then(function(response) {
        var documents = response.feed;
        console.log(response.resource); 
    }, function(error) {
        console.log("Error" , error);
    });

## <a name="javascript-language-integrated-query-api"></a>Kwerendy scałkowanej w przedziale języka JavaScript API
Oprócz wydaniem zapytań za pomocą gramatyki SQL w DocumentDB, SDK po stronie serwera umożliwia wykonywanie zoptymalizowane zapytań za pomocą fluent interfejs JavaScript bez dowolnego wiedzy programu SQL Server. Kwerenda JavaScript, którą interfejs API umożliwia programowy konstruowania kwerend przekazując predykatu funkcji w funkcji chainable połączeń z składni znanych built-ins tablicy i popularnych bibliotek JavaScript, takich jak lodash i ECMAScript5. Kwerendy pobieranych przez program obsługi języka JavaScript do wykonania efektywne za pomocą wskaźników DocumentDB osoby.

> [AZURE.NOTE]`__` (podwójne podkreślenie) jest to alias `getContext().getCollection()`.
> <br/>
> Innymi słowy, można użyć `__` lub `getContext().getCollection()` Aby uzyskać dostęp do zapytania interfejsu API języka JavaScript.

Obsługiwane funkcje obejmują:
<ul>
<li>
<b>Chain().... wartość ([zwrotnego] [, opcje])</b>
<ul>
<li>
Zaczyna się łańcuchowej połączenia, który musi zostać zakończone value().
</li>
</ul>
</li>
<li>
<b>Filtr (predicateFunction [, opcje] [, zwrotnego])</b>
<ul>
<li>
Umożliwia filtrowanie danych wejściowych przy użyciu funkcji predykatu, która zwraca wartość PRAWDA i FAŁSZ, aby przefiltrować wewnątrz i na zewnątrz wprowadzania dokumentów do Wynikowy zestaw. To zachowuje się podobnie do klauzuli WHERE w języku SQL.
</li>
</ul>
</li>
<li>
<b>Mapa (transformationFunction [, opcje] [, zwrotnego])</b>
<ul>
<li>
Stosuje rzut danej funkcji przekształcenie, która map każdego elementu wprowadzania danych do obiektu JavaScript lub wartości. To zachowuje się podobnie jak w klauzuli SELECT w języku SQL.
</li>
</ul>
</li>
<li>
<b>pluck ([propertyName] [, opcje] [, zwrotnego])</b>
<ul>
<li>
Jest to skrót mapy, które zwraca wartość pojedynczej właściwości z każdego elementu wprowadzania danych.
</li>
</ul>
</li>
<li>
<b>Spłaszcz ([isShallow] [, opcje] [, zwrotnego])</b>
<ul>
<li>
Łączy i spłaszcza tablic z każdego elementu wprowadzania do pojedynczej tablicy. To zachowuje się podobnie SelectMany w programie LINQ.
</li>
</ul>
</li>
<li>
<b>sortBy ([predicate] [, opcje] [, zwrotnego])</b>
<ul>
<li>
Sortowanie dokumentów w strumieniu dokument wejściowy rosnąco przy użyciu predykacie danego produktu nowego zestawu dokumentów. To zachowuje się podobnie jak klauzula ORDER BY w języku SQL.
</li>
</ul>
</li>
<li>
<b>sortByDescending ([predicate] [, opcje] [, zwrotnego])</b>
<ul>
<li>
Sortowanie dokumentów w strumieniu dokument wejściowy w porządku malejącym przy użyciu predykacie danego produktu nowego zestawu dokumentów. To zachowuje się podobnie jak klauzula ORDER BY x DESC w języku SQL.
</li>
</ul>
</li>
</ul>


Gdy otoczone predykat i/lub selektor funkcji, poniższych konstrukcji JavaScript uzyskiwanie automatycznie zoptymalizowane pod kątem uruchomić bezpośrednio na indeksy DocumentDB:

* Operatory prosty: = + - *-% | ^ &amp; == != === !=== &lt; &gt; &lt;= &gt;= || &amp;&amp; &lt;&lt; &gt;&gt; &gt;&gt;&gt;! ~
* Literałów, łącznie z obiektem literałów: {}
* Wariancja zwrotu

Poniższych konstrukcji języka JavaScript nie uzyskać zoptymalizowane dla wskaźników DocumentDB:

* Kontrolowanie przepływu (np. Jeśli, gdy)
* Funkcja połączeń

Aby uzyskać więcej informacji zobacz nasze [JSDocs po stronie serwera](http://azure.github.io/azure-documentdb-js-server/).

### <a name="example-write-a-stored-procedure-using-the-javascript-query-api"></a>Przykład: Pisanie procedura składowana przy użyciu kwerendy interfejsu API języka JavaScript

Poniższy przykład kodu przedstawiono przykładowy sposób użycia kwerendy API języka JavaScript w kontekście procedury składowanej. Procedura składowana wstawia dokumentu, podane przez parametru wejściowego oraz aktualizacji metadanych dokumentu za pomocą `__.filter()` metody z minSize, maxSize i totalSize na podstawie właściwości rozmiaru dokumentu wprowadzania danych.

    /**
     * Insert actual doc and update metadata doc: minSize, maxSize, totalSize based on doc.size.
     */
    function insertDocumentAndUpdateMetadata(doc) {
      // HTTP error codes sent to our callback funciton by DocDB server.
      var ErrorCode = {
        RETRY_WITH: 449,
      }

      var isAccepted = __.createDocument(__.getSelfLink(), doc, {}, function(err, doc, options) {
        if (err) throw err;

        // Check the doc (ignore docs with invalid/zero size and metaDoc itself) and call updateMetadata.
        if (!doc.isMetadata && doc.size > 0) {
          // Get the meta document. We keep it in the same collection. it's the only doc that has .isMetadata = true.
          var result = __.filter(function(x) {
            return x.isMetadata === true
          }, function(err, feed, options) {
            if (err) throw err;

            // We assume that metadata doc was pre-created and must exist when this script is called.
            if (!feed || !feed.length) throw new Error("Failed to find the metadata document.");

            // The metadata document.
            var metaDoc = feed[0];

            // Update metaDoc.minSize:
            // for 1st document use doc.Size, for all the rest see if it's less than last min.
            if (metaDoc.minSize == 0) metaDoc.minSize = doc.size;
            else metaDoc.minSize = Math.min(metaDoc.minSize, doc.size);

            // Update metaDoc.maxSize.
            metaDoc.maxSize = Math.max(metaDoc.maxSize, doc.size);

            // Update metaDoc.totalSize.
            metaDoc.totalSize += doc.size;

            // Update/replace the metadata document in the store.
            var isAccepted = __.replaceDocument(metaDoc._self, metaDoc, function(err) {
              if (err) throw err;
              // Note: in case concurrent updates causes conflict with ErrorCode.RETRY_WITH, we can't read the meta again 
              //       and update again because due to Snapshot isolation we will read same exact version (we are in same transaction).
              //       We have to take care of that on the client side.
            });
            if (!isAccepted) throw new Error("replaceDocument(metaDoc) returned false.");
          });
          if (!result.isAccepted) throw new Error("filter for metaDoc returned false.");
        }
      });
      if (!isAccepted) throw new Error("createDocument(actual doc) returned false.");
    }

## <a name="sql-to-javascript-cheat-sheet"></a>Kod SQL, aby arkusz podpowiedzi zawierający języka Javascript
W poniższej tabeli przedstawiono różne SQL i odpowiadające im kwerend JavaScript.

Z zapytania SQL klawiszy właściwość dokumentu (np. `doc.id`) jest uwzględniana wielkość liter.

<br/>
<table border="1" width="100%">
<colgroup>
<col span="1" style="width: 40%;">
<col span="1" style="width: 40%;">
<col span="1" style="width: 20%;">
</colgroup>
<tbody>
<tr>
<th>SQL</th>
<th>Kwerendy języka JavaScript API</th>
<th>Szczegóły</th>
</tr>
<tr>
<td>
<pre>
Wybierz pozycję * z dokumentów
</pre>
</td>
<td>
<pre>
__.map(Function(doc) {doc zwrotu;});
</pre>
</td>
<td>Wyniki we wszystkich dokumentach (podzielony na strony z token kontynuacji) jako jest.</td>
</tr>
<tr>
<td>
<pre>
Wybierz pozycję docs.id, docs.message jako msg, docs.actions z dokumentów
</pre>
</td>
<td>
<pre>
__.map(Function(doc) {zwracana {identyfikator: doc.id, msg: doc.message, akcje: doc.actions};});
</pre>
</td>
<td>Projekty identyfikator, wiadomość (alias do msg) i akcję z wszystkie dokumenty.</td>
</tr>
<tr>
<td>
<pre>
Wybierz pozycję * z dokumentów WHERE docs.id="X998_Y998"
</pre>
</td>
<td>
<pre>
__.Filter(Function(doc) {zwracana doc.id === "X998_Y998;"});
</pre>
</td>
<td>Kwerendy dla dokumentów przy użyciu predykacie: id = "X998_Y998".</td>
</tr>
<tr>
<td>
<pre>
Wybierz pozycję * z dokumentów miejsce, w którym ARRAY_CONTAINS(docs. Znaczniki, 123)
</pre>
</td>
<td>
<pre>
__.Filter(Function(x) {zwracana x.Tags & & x.Tags.indexOf(123) >-; 1});
</pre>
</td>
<td>Kwerendy dla dokumentów, które mają właściwość tagi i znaczników jest tablica zawierająca wartość 123.</td>
</tr>
<tr>
<td>
<pre>
Wybierz pozycję docs.id, docs.message jako dokumenty od msg WHERE docs.id="X998_Y998"
</pre>
</td>
<td>
<pre>
__.chain() .filter(function(doc) {zwracana doc.id === "X998_Y998;"}) .map(function(doc) {zwracana {identyfikator: doc.id, msg: doc.message};}) .value();
</pre>
</td>
<td>Kwerendy dla dokumentów przy użyciu predykatu, id = "X998_Y998", a następnie projektów identyfikatora i komunikatu (alias do msg).</td>
</tr>
<tr>
<td>
<pre>
Znakowanie dokumentów od znacznika wybierz wartość sprzężenia w dokumentach. Znaczniki ORDER BY docs._ts
</pre>
</td>
<td>
<pre>
.filter(function(doc) __.chain() {zwrotu dokumentu. Znaczniki & & Array.isArray (doc. Znaczniki); .sortBy(function(doc)}) {zwrotu doc._ts;}) .pluck("Tags") .flatten() .value()
</pre>
</td>
<td>Filtry dla dokumentów, które mają właściwości tablicy, znaczniki, a sortuje otrzymane dokumenty, właściwość systemu _ts sygnatura czasowa, a następnie projektów + spłaszcza tablicy znaczniki.</td>
</tr>
</tbody>
</table>

## <a name="runtime-support"></a>Środowisko uruchomieniowe pomocy technicznej
[Po stronie serwera DocumentDB JavaScript SDK](http://azure.github.io/azure-documentdb-js-server/) zapewnia obsługę najbardziej typowych funkcji języka JavaScript jako standardowych przez [ECMA 262](http://www.ecma-international.org/publications/standards/Ecma-262.htm).

### <a name="security"></a>Zabezpieczenia
Procedury składowane JavaScript i wyzwalacze są w trybie piaskownicy tak, aby skutków skrypt nie pamięci do drugiego bez pośrednictwa izolacji transakcji migawki na poziomie bazy danych. W środowiskach środowisko uruchomieniowe puli, ale czyszczone kontekstu po każdej serii. W związku z tym jest gwarantowana bezpieczne jakichkolwiek niezamierzonych skutków strony od siebie.

### <a name="pre-compilation"></a>Wstępna kompilacja
Procedur składowanych, wyzwalaczami i funkcji zdefiniowanych przez użytkownika są niejawnie wstępnie skompilowana bajt format kodu w celu uniknięcia koszt kompilacji podczas każdego wywołania skrypt. Dzięki temu wywołania procedury składowane są szybkie i masz za niski.

## <a name="client-sdk-support"></a>Obsługa SDK klienta
Oprócz klienta [Node.js](documentdb-sdk-node.md) DocumentDB obsługuje [.NET](documentdb-sdk-dotnet.md), [języka Java](documentdb-sdk-java.md), [JavaScript](http://azure.github.io/azure-documentdb-js/)i [Python SDK](documentdb-sdk-python.md). Procedury składowane, wyzwalaczami i funkcji zdefiniowanych przez użytkownika mogą być tworzone i wykonywane przy użyciu dowolnego z tych SDK także. W poniższym przykładzie pokazano sposób tworzenia i wykonaj procedurę przechowywaną przy użyciu klienta programu .NET. Zwróć uwagę, jak typy .NET są przekazywane do procedura przechowywana jako JSON i odczytywania.

    var markAntiquesSproc = new StoredProcedure
    {
        Id = "ValidateDocumentAge",
        Body = @"
                function(docToCreate, antiqueYear) {
                    var collection = getContext().getCollection();    
                    var response = getContext().getResponse();    
    
                    if(docToCreate.Year != undefined && docToCreate.Year < antiqueYear){
                        docToCreate.antique = true;
                    }
    
                    collection.createDocument(collection.getSelfLink(), docToCreate, {}, 
                        function(err, docCreated, options) { 
                            if(err) throw new Error('Error while creating document: ' + err.message);                              
                            if(options.maxCollectionSizeInMb == 0) throw 'max collection size not found'; 
                            response.setBody(docCreated);
                    });
            }"
    };
    
    // register stored procedure
    StoredProcedure createdStoredProcedure = await client.CreateStoredProcedureAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"), markAntiquesSproc);
    dynamic document = new Document() { Id = "Borges_112" };
    document.Title = "Aleph";
    document.Year = 1949;
    
    // execute stored procedure
    Document createdDocument = await client.ExecuteStoredProcedureAsync<Document>(UriFactory.CreateStoredProcedureUri("db", "coll", "sproc"), document, 1920);


W tym przykładzie pokazano, jak za pomocą [.NET SDK](https://msdn.microsoft.com/library/azure/dn948556.aspx) Tworzenie wyzwalacza wstępnej i utworzyć dokument za pomocą wyzwalacza włączone. 

    Trigger preTrigger = new Trigger()
    {
        Id = "CapitalizeName",
        Body = @"function() {
            var item = getContext().getRequest().getBody();
            item.id = item.id.toUpperCase();
            getContext().getRequest().setBody(item);
        }",
        TriggerOperation = TriggerOperation.Create,
        TriggerType = TriggerType.Pre
    };
    
    Document createdItem = await client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"), new Document { Id = "documentdb" },
        new RequestOptions
        {
            PreTriggerInclude = new List<string> { "CapitalizeName" },
        });


I w poniższym przykładzie pokazano, jak utworzyć funkcję zdefiniowaną przez użytkownika (UDF) i używać go w [języku SQL DocumentDB](documentdb-sql-query.md).

    UserDefinedFunction function = new UserDefinedFunction()
    {
        Id = "LOWER",
        Body = @"function(input) 
        {
            return input.toLowerCase();
        }"
    };
    
    foreach (Book book in client.CreateDocumentQuery(UriFactory.CreateDocumentCollectionUri("db", "coll"),
        "SELECT * FROM Books b WHERE udf.LOWER(b.Title) = 'war and peace'"))
    {
        Console.WriteLine("Read {0} from query", book);
    }

## <a name="rest-api"></a>INTERFEJSU API USŁUGI REST
Wszystkie operacje DocumentDB mogą być wykonywane w sposób RESTful. Procedur składowanych, wyzwalaczami i funkcji zdefiniowanych przez użytkownika można rejestrować w obszarze zbioru przy użyciu protokołu HTTP wpisu. Oto przykładowy sposób zarejestrować procedura składowana:

    POST https://<url>/sprocs/ HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:10 GMT
    
    
    var x = {
      "name": "createAndAddProperty",
      "body": function (docToCreate, addedPropertyName, addedPropertyValue) {
                var collectionManager = getContext().getCollection();
                collectionManager.createDocument(
                    collectionManager.getSelfLink(),
                    docToCreate,
                    function(err, docCreated) {
                      if(err) throw new Error('Error:  ' + err.message);
                      docCreated[addedPropertyName] = addedPropertyValue;
                      getContext().getResponse().setBody(docCreated);
                    });
            }
    }


Procedura składowana są rejestrowane przez wykonanie żądania POST przed URI dbs testowabd colls-testColl-sprocs z treści zawierający procedury przechowywanej w celu utworzenia. Wyzwalacze i funkcji zdefiniowanych przez użytkownika można rejestrować podobnie, wysyłając odpowiednio WPIS przed/wyzwalacze i /udfs.
To przechowywane procedury mogą następnie być wykonane przez WPIS żądania przed jego łącze zasobu:

    POST https://<url>/sprocs/<sproc> HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:20 GMT
    
    
    [ { "name": "TestDocument", "book": "Autumn of the Patriarch"}, "Price", 200 ]


W tym miejscu dane wejściowe procedura składowana jest przekazywany w treści wezwania. Zauważ, że dane wejściowe są przekazywane jako tablicę JSON parametrów wejściowych. Procedura składowana pobiera pierwszego wprowadzania dokumentu, który jest treść odpowiedzi. Odpowiedź, którą otrzymane jest następująca:

    HTTP/1.1 200 OK
     
    { 
      name: 'TestDocument',
      book: ‘Autumn of the Patriarch’,
      id: ‘V7tQANV3rAkDAAAAAAAAAA==‘,
      ts: 1407830727,
      self: ‘dbs/V7tQAA==/colls/V7tQANV3rAk=/docs/V7tQANV3rAkDAAAAAAAAAA==/’,
      etag: ‘6c006596-0000-0000-0000-53e9cac70000’,
      attachments: ‘attachments/’,
      Price: 200
    }


Wyzwalacze, w przeciwieństwie do procedur składowanych nie można wykonać bezpośrednio. Zamiast tego są one wykonywane w ramach operacji w dokumencie. Firma Microsoft można określić wyzwalacze do uruchomienia z żądaniem za pomocą nagłówków HTTP. Poniżej przedstawiono żądanie, aby utworzyć dokument.

    POST https://<url>/docs/ HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:10 GMT
    x-ms-documentdb-pre-trigger-include: validateDocumentContents 
    x-ms-documentdb-post-trigger-include: bookCreationPostTrigger
    
    
    {
       "name": "newDocument",
       “title”: “The Wizard of Oz”,
       “author”: “Frank Baum”,
       “pages”: 92
    }


W tym miejscu przed wyzwalacza przeprowadzić z żądaniem jest określona w nagłówku x-ms-documentdb-pre-trigger-include. Odpowiednio wszelkie po wyzwalacze znajdują się w nagłówku x-ms-documentdb-post-trigger-include. Należy zauważyć, że przed reformą i po wyzwalaczy można określić dla danego żądania.

## <a name="sample-code"></a>Przykładowy kod

Więcej przykładów kodu po stronie serwera (w tym [Usuwanie zbiorcze](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples/stored-procedures/bulkDelete.js)i [Aktualizowanie](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples/stored-procedures/update.js)) można znaleźć w naszym [repozytorium Github](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples).

Chcesz udostępnić klienci procedura składowana? Prześlij nam żądanie pobieraj! 

## <a name="next-steps"></a>Następne kroki

Po umieszczeniu co najmniej jeden procedur składowanych, wyzwalaczami i funkcji zdefiniowanych przez użytkownika utworzone można załadować je i wyświetlać je w Portal Azure za pomocą Eksploratora skrypt. Aby uzyskać więcej informacji zobacz [Wyświetlanie przechowywane procedur, wyzwalaczy i funkcji zdefiniowanych przez użytkownika, przy użyciu Eksploratora skrypt DocumentDB](documentdb-view-scripts.md).

Można również znaleźć następujące odwołania i zasoby przydatne w ścieżce, aby dowiedzieć się więcej na temat programowania po stronie serwera DocumentDB:

- [SDK Azure DocumentDB](https://msdn.microsoft.com/library/azure/dn781482.aspx)
- [DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio/releases)
- [JSON](http://www.json.org/) 
- [JavaScript ECMA 262](http://www.ecma-international.org/publications/standards/Ecma-262.htm)
- [JavaScript — JSON typ systemu](http://www.json.org/js.html) 
- [Rozszerzanie bezpiecznego i przenośnych bazy danych](http://dl.acm.org/citation.cfm?id=276339) 
- [Usługa kolumnowo Architektura bazy danych](http://dl.acm.org/citation.cfm?id=1066267&coll=Portal&dl=GUIDE) 
- [Hostingu Runtime .NET w programie Microsoft SQL server](http://dl.acm.org/citation.cfm?id=1007669)
