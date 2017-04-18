<properties
    pageTitle="Lista zasobów magazynowania Azure z biblioteką klienta Microsoft Azure miejsca do magazynowania dla C++ | Microsoft Azure"
    description="Dowiedz się, jak używać listy interfejsów API w Biblioteka klienta programu Microsoft Azure miejsca do magazynowania dla C++ wyliczyć kontenerów, obiektów blob kolejek, tabel i obiektów."
    documentationCenter=".net"
    services="storage"
    authors="dineshmurthy"
    manager="jahogg"
    editor="tysonn"/>
<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="dineshm"/>

# <a name="list-azure-storage-resources-in-c"></a>Zasoby dla listy Azure miejsca do magazynowania w języku C++

Operacje na liście przedstawiono kluczowe do wielu scenariuszy opracowywania miejsca do magazynowania Azure. W tym artykule opisano sposoby najbardziej efektywne wyliczanie obiektów w magazynie Azure za pomocą listy interfejsów API przewidziane w bibliotece Microsoft Azure miejsca do magazynowania klienta C++.

>[AZURE.NOTE] Ten przewodnik jest przeznaczony dla biblioteki klienta Azure miejsca do magazynowania dla wersji C++ 2.x, który jest dostępny za pośrednictwem [NuGet](http://www.nuget.org/packages/wastorage) lub [GitHub](https://github.com/Azure/azure-storage-cpp).

Biblioteka klienta miejsca do magazynowania udostępnia szereg sposobów do listy lub kwerendy obiektów w magazynie Azure. Ten artykuł dotyczy następujących scenariuszy:

-   Lista kontenerów na koncie
-   Lista obiektów blob w kontenerze lub katalogu wirtualnego obiektów blob
-   Lista kolejkach konta
-   Lista tabel przy użyciu konta
-   Jednostki kwerendy w tabeli

Każdej z tych metod są wyświetlane przy użyciu różne przeciążenia dla różnych scenariuszach.

## <a name="asynchronous-versus-synchronous"></a>Asynchroniczne i obraz

Ponieważ biblioteka klienta miejsca do magazynowania dla C++ skonstruowaniu jest na wierzchu [pozostałych C++ biblioteki](https://github.com/Microsoft/cpprestsdk)założenia obsługujemy operacji asynchronicznych przy użyciu [pplx::task](http://microsoft.github.io/cpprestsdk/classpplx_1_1task.html). Na przykład:

    pplx::task<list_blob_item_segment> list_blobs_segmented_async(continuation_token& token) const;

Obraz operacji zawijać odpowiednich operacji asynchronicznych:

    list_blob_item_segment list_blobs_segmented(const continuation_token& token) const
    {
        return list_blobs_segmented_async(token).get();
    }

Jeśli pracujesz z wielu wątków aplikacji i usług, zaleca się użycie asynchroniczne interfejsy API bezpośrednio zamiast tworzenia wątku połączenie synchronizacji interfejsów API, co znacznie wpływa na wydajność.

## <a name="segmented-listing"></a>Lista segmentowany

Skala magazynu w chmurze wymaga segmentowany listę. Na przykład możesz mieć na milion BLOB w kontenerze obiektów blob platformy Azure lub podmioty miliardów tabeli Azure. Nie są one teoretyczna liczb, ale przypadków użycia rzeczywistą klienta.

Dlatego jest niemożliwe do listy wszystkich obiektów pojedyncza odpowiedź. Zamiast tego można wyświetlić listę obiektów przy użyciu nawigacji między stronami. Każda lista interfejsów API zawiera *części* przeciążeń.

Odpowiedź operacji segmentowany lista zawiera:

-   <i>_segment</i>, zawierający zestaw wyników dla jednego połączenia API lista jest zwracana.
-   *continuation_token*, który jest przekazywany do następnego połączenie, aby przejść do następnej strony wyników. W przypadku nie więcej wyników do zwrócenia token kontynuacji ma wartość null.

Na przykład typowego połączenia, aby wyświetlić listę wszystkich obiektów blob w kontenerze może wyglądać następujące wstawkę kodu. Kod jest dostępny w naszym [próbki](https://github.com/Azure/azure-storage-cpp/blob/master/Microsoft.WindowsAzure.Storage/samples/BlobsGettingStarted/Application.cpp):

    // List blobs in the blob container
    azure::storage::continuation_token token;
    do
    {
        azure::storage::list_blob_item_segment segment = container.list_blobs_segmented(token);
        for (auto it = segment.results().cbegin(); it != segment.results().cend(); ++it)
    {
        if (it->is_blob())
        {
            process_blob(it->as_blob());
        }
        else
        {
            process_diretory(it->as_directory());
        }
    }

        token = segment.continuation_token();
    }
    while (!token.empty());

Zwróć uwagę, że liczba wyników zwróconych na stronie mogą być sterowane przez parametr *max_results* w przeciążeń każdego interfejsu API, na przykład:

    list_blob_item_segment list_blobs_segmented(const utility::string_t& prefix, bool use_flat_blob_listing,
        blob_listing_details::values includes, int max_results, const continuation_token& token,
        const blob_request_options& options, operation_context context)

Jeśli nie określisz parametru *max_results* , Domyślna maksymalna wartość do 5000 wyników jest zwracana w pojedynczej strony.

Należy również zauważyć, że kwerenda magazyn tabel platformy Azure może zwracać żadnych rekordów lub mniejszej liczby rekordów niż wartość parametru *max_results* , który został wybrany, nawet jeśli token kontynuacji nie jest puste. Powodem może być kwerenda nie można ukończyć w pięć sekund. Jak token kontynuacji nie jest puste, należy kontynuować kwerendy, a kodu nie przyjęto założenie rozmiar segmentu wyników.

Zalecane wzorzec kodowania dla większości scenariuszy składa się z pozycje, które znajdują się jawne postępu listy lub kwerend i jak usługa odpowiada na każde żądanie. Szczególnie dla aplikacji C++ lub usług kontrolki niższego poziomu postępu lista może pomóc w pamięci sterowania i wydajności.

## <a name="greedy-listing"></a>Lista intensywnie

Starsze niż Biblioteka klienta miejsca do magazynowania dla C++ (Podgląd wersji 0.5.0 i starszych) dostępny-części listę interfejsów API dla tabel i kolejki, tak jak w poniższym przykładzie:

    std::vector<cloud_table> list_tables(const utility::string_t& prefix) const;
    std::vector<table_entity> execute_query(const table_query& query) const;
    std::vector<cloud_queue> list_queues() const;

Te metody zostały wprowadzone jako otoki segmentowany API. Dla każdej odpowiedzi segmentowany listy Kod dołączana wyniki wektor i zwracane wszystkie wyniki po pełny kontenery zostały zeskanowane.

Tej metody mogą działać, gdy konto miejsca do magazynowania lub tabela zawiera małą liczbę obiektów. Jednak wraz ze wzrostem liczby obiektów pamięci wymaganej można zwiększyć bez ograniczeń, ponieważ wszystkie wyniki pozostał w pamięci. Jeden operacja listy może zająć bardzo długo, w którym wywołujący ma żadnych informacji o postępie.

Te lista intensywnie interfejsy API w zestawie SDK istnieje w C#, Java lub środowiska JavaScript Node.js. Aby uniknąć potencjalnych problemach związanych z używaniem te intensywnie interfejsy API, usunięciu ich w wersji 0.6.0 Podgląd.

Jeśli kod wywołuje te intensywnie interfejsy API:

    std::vector<azure::storage::table_entity> entities = table.execute_query(query);
    for (auto it = entities.cbegin(); it != entities.cend(); ++it)
    {
        process_entity(*it);
    }

Następnie należy zmodyfikować swój kod, aby liście segmentowany API za pomocą:

    azure::storage::continuation_token token;
    do
    {
        azure::storage::table_query_segment segment = table.execute_query_segmented(query, token);
        for (auto it = segment.results().cbegin(); it != segment.results().cend(); ++it)
        {
            process_entity(*it);
        }

        token = segment.continuation_token();
    } while (!token.empty());

Określając parametr *max_results* segmentu, można saldo między liczby żądań i użycie pamięci w celu dotrzymania zagadnienia dotyczące wydajności aplikacji.

Ponadto, jeśli korzystasz z listy segmentowany API, ale przechowywania danych w zbiorze lokalnych w stylu "intensywnie", również zdecydowanie zaleca się że refactor kod powinien obsługiwać dane są przechowywane w lokalnej kolekcji dokładnie w skali.

## <a name="lazy-listing"></a>Lista opóźnieniem

Lista intensywnie podniesiona potencjalne problemy, ale jest wygodny Jeśli jest zbyt wiele obiektów w kontenerze.

Jeśli używasz również C# lub SDK Java Oracle, należy zapoznać z modelem programowania ustalony, która oferuje opóźnieniem stylu Lista, gdzie przy niektórych przesunięciu tylko pobieranych w razie potrzeby. W języku C++ szablonu opartego na iteracyjne zawiera podobne podejście.

Typowe listę opóźnieniem API, na przykład za pomocą **list_blobs** wygląda następująco:

    list_blob_item_iterator list_blobs() const;

Typowe wstawki, używającego wzorca opóźnieniem lista może wyglądać następująco:

    // List blobs in the blob container
    azure::storage::list_blob_item_iterator end_of_results;
    for (auto it = container.list_blobs(); it != end_of_results; ++it)
    {
        if (it->is_blob())
        {
            process_blob(it->as_blob());
        }
        else
        {
            process_directory(it->as_directory());
        }
    }

Należy zauważyć, że lista opóźnieniem jest dostępna tylko w trybie synchroniczne.

W porównaniu z listą intensywnie, lista opóźnieniem pobiera dane tylko wtedy, gdy jest to konieczne. W obszarze okładki jego pobiera dane z magazynu Azure tylko wtedy, gdy następnym iteracyjne przejdzie do następny odcinek. W związku z tym użycie pamięci jest określana za pomocą ograniczonych rozmiar, a operacja jest szybkie.

Lista opóźnieniem interfejsy API zawartych w bibliotece klienta miejsca do magazynowania dla C++ w wersji 2.2.0.

## <a name="conclusion"></a>Wnioski

W tym artykule omówiono możemy różne przeciążenia listą interfejsy API dla różnych obiektów w bibliotece klienta miejsca do magazynowania dla C++. Podsumowanie:

-   Interfejsy API asynchroniczne zaleca się w wielu scenariuszach wątków.
-   Lista segmentowany jest zalecane dla większości scenariuszy.
-   Lista opóźnieniem ma postać w bibliotece dogodnym opakowanie w scenariuszach synchroniczne.
-   Lista intensywnie nie jest zalecane i została usunięta z biblioteki.

##<a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji o przechowywaniu Azure i Biblioteka klienta C++ zobacz następujące zasoby.

-   [Jak używać magazyn obiektów Blob z C++](storage-c-plus-plus-how-to-use-blobs.md)
-   [Jak używać magazyn tabel z C++](storage-c-plus-plus-how-to-use-tables.md)
-   [Jak używać magazyn kolejek z C++](storage-c-plus-plus-how-to-use-queues.md)
-   [Biblioteka klienta Azure przechowywania dokumentacji interfejsu API C++.](http://azure.github.io/azure-storage-cpp/)
-   [Blog zespołu Azure miejsca do magazynowania](http://blogs.msdn.com/b/windowsazurestorage/)
-   [Dokumentacja Azure miejsca do magazynowania](https://azure.microsoft.com/documentation/services/storage/)
