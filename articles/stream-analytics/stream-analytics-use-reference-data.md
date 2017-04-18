<properties
    pageTitle="Za pomocą tabel danych i wyszukiwania odwołań do analizy strumieniu | Microsoft Azure"
    description="Używanie danych źródłowych w kwerendzie analizy strumieniu"
    keywords="tabeli odnośników, danych źródłowych"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>

# <a name="using-reference-data-or-lookup-tables-in-a-stream-analytics-input-stream"></a>Za pomocą tabel danych lub Wyszukaj odwołań w strumieniu wprowadzania analizy strumieniu

Dane źródłowe (nazywane także tabeli odnośników) jest ograniczone zestawu danych, który jest statyczny lub spowalniając zmieniania charakter, używane do wyszukiwania lub być zgodne z strumienia danych. Aby korzystać z danych źródłowych w swojej pracy Azure analizy strumieniu zazwyczaj użyje [Odwołanie dołączanie danych do](https://msdn.microsoft.com/library/azure/dn949258.aspx) w kwerendzie. Analizy strumieniu używa magazyn obiektów Blob platformy Azure jako Warstwa przechowywania danych źródłowych i z odwołaniem Factory danych Azure danych może być przekształcania i/lub skopiowany do magazyn obiektów Blob platformy Azure, do użycia jako dane odwołania, na [podstawie dowolnej liczby chmury i magazynów lokalnego](../data-factory/data-factory-data-movement-activities.md). Za pomocą sekwencji obiektów blob (zdefiniowane w konfiguracji wprowadzania) rosnąco Data/Godzina, określonego w polu Nazwa obiektów blob w modelu danych źródłowych. Ją obsługuje **tylko** dodawanie do końca sekwencji za pomocą Data/Godzina **większa** niż określona przez ostatnią obiektów blob w sekwencji.

## <a name="configuring-reference-data"></a>Konfigurowanie danych źródłowych

Aby skonfigurować danych odwołania, najpierw należy utworzyć wprowadzenia typu **Danych źródłowych**. W poniższej tabeli opisano każdej właściwości, którą należy podać podczas tworzenia wprowadzania danych źródłowych z jej opis:

<table>
<tbody>
<tr>
<td>Nazwa właściwości</td>
<td>Opis</td>
</tr>
<tr>
<td>Alias wprowadzania danych</td>
<td>Przyjazna nazwa będzie używana w kwerendzie zadania zostać utworzone odwołanie tych danych wejściowych.</td>
</tr>
<tr>
<td>Konto miejsca do magazynowania</td>
<td>Nazwa konta miejsca do magazynowania, gdzie znajdują się pliki obiektów blob. Znajduje się w tej samej subskrypcji jako zadanie analizy strumieniu, można go wybrać z listy w dół.</td>
</tr>
<tr>
<td>Klucz konta miejsca do magazynowania</td>
<td>Klucz tajny, skojarzone z kontem miejsca do magazynowania. Otrzymuje to wypełniane automatycznie, jeśli konto miejsca do magazynowania znajduje się w tej samej subskrypcji jako zadanie analizy strumieniu.</td>
</tr>
<tr>
<td>Kontener miejsca do magazynowania</td>
<td>Kontenery zapewniają logiczną grupę dla obiektów blob przechowywane w usłudze Microsoft Azure Blob. Gdy wysyłasz obiektów blob usługi obiektów Blob należy określić kontener dla tego obiektów blob.</td>
</tr>
<tr>
<td>Ścieżka wzorzec</td>
<td>Ścieżka pliku używane do znajdowania z obiektami BLOB w określonym kontenerze. W ścieżce można określić wystąpienia jeden lub więcej z następujących zmiennych 2:<BR>{date} {time}<BR>Przykład 1: products/{date}/{time}/product-list.csv<BR>Przykład 2: products/{date}/product-list.csv
</tr>
<tr>
<td>Format daty [opcjonalne]</td>
<td>{Date} użycie wewnątrz ścieżki określony format daty, w której są zorganizowane plików można wybrać z listy rozwijanej obsługiwane formaty. Przykład: RRRR MM-DD</td>
</tr>
<tr>
<td>Format godziny [opcjonalne]</td>
<td>{Time} użycie wewnątrz ścieżki określony format czasu, w którym są zorganizowane plików można wybrać z listy rozwijanej obsługiwane formaty. Przykład: HH</td>
</tr>
<tr>
<td>Format szeregowania zdarzenia</td>
<td>Aby upewnić się, że zapytań działają w ten sposób można się spodziewać, analizy strumieniu musi wiedzieć, który format szeregowania używasz dla poczty przychodzącej strumieni danych. Dane referencyjne obsługiwane formaty są CSV i JSON.</td>
</tr>
<tr>
<td>Kodowanie</td>
<td>Obecnie UTF-8 jest obsługiwany tylko format kodowania</td>
</tr>
</tbody>
</table>

## <a name="generating-reference-data-on-a-schedule"></a>Generowanie danych referencyjnych zgodnie z harmonogramem

Jeśli dane odwołanie jest powoli Zmienianie zestawu danych, następnie obsługę odświeżanie odwołanie, które danych jest włączona, określając ścieżka wzorzec w konfiguracji wprowadzania danych przy użyciu {date} i {godziny} tokeny podstawiania. Analizy strumieniu użyje definicji danych zaktualizowane odwołania, oparty na tym deseniu ścieżki. Na przykład wzorzec `sample/{date}/{time}/products.csv` w formacie daty ilości czasu i **"RRRR-MM-DD"** format **"Hh: mm"** powoduje, że analizy strumieniu do pobrania zaktualizowanej obiektów blob `sample/2015-04-16/17:30/products.csv` godzinie 5:30 kwietnia 2015 16 UTC czas strefy.

> [AZURE.NOTE] Obecnie analizy strumieniu zadań poszukaj odświeżania obiektów blob tylko wtedy, gdy godzina na komputerze przechodzi do czasu zakodowany w nazwie obiektów blob. Na przykład zadanie będzie wyglądać na `sample/2015-04-16/17:30/products.csv` jak to możliwe, ale nie wcześniej niż 5:30 PM kwiecień 2015 16 UTC czas strefy. Wpłynie to *nigdy nie* odszukaj plik z wcześniejszą niż ostatnią, która zostanie wykryta zakodowany czasu.
> 
> Np. po zadaniu umożliwia znalezienie wyrazów to `sample/2015-04-16/17:30/products.csv` będzie ignorować wszystkie pliki z wcześniejszych niż 5:30 PM kwiecień 2015 16 zakodowany daty, gdy nadchodzi opóźnione `sample/2015-04-16/17:25/products.csv` obiektów blob zostanie utworzony w tym samym kontenerze zadania nie będzie używać go.
> 
> Podobnie jeśli `sample/2015-04-16/17:30/products.csv` dostarczać tylko godzinie 10:03 kwiecień 2015 16, ale nie obiektów blob z datą wcześniejszą znajduje się w kontenerze, zadanie będzie korzystać z tego pliku, zaczynając od 10:03 PM kwiecień 2015 16 i przy użyciu poprzedniej danych odwołania do tego czasu.
> 
> Wyjątek to zadanie musi ponownie przetwarzanie danych w czasie lub po pierwszym zadaniu uruchomiono. Podczas uruchamiania, które zadania szuka ostatnio obiektów blob wyprodukowano przed zadania Uruchom określony czas. Można to zrobić w celu zapewnienia zbioru danych odwołania **niepustych** podczas uruchamiania zadania. Jeśli nie można odnaleźć jednego, zadanie zostanie wyświetlone następujące diagnostic: `Initializing input without a valid reference data blob for UTC time <start time>`.


[Azure Factory danych](https://azure.microsoft.com/documentation/services/data-factory/) można dodać akompaniament zadania tworzenia zaktualizowanych obiektów blob wymagane przez analizy strumieniu w celu zaktualizowania definicji danych odwołania. Factory danych to usługa integracji danych opartych na chmurze, która orchestrates i zautomatyzować poruszania się i przekształcania danych. Factory danych obsługuje [łączenia dużej liczby oparte na chmurze i magazynów danych lokalnych](../data-factory/data-factory-data-movement-activities.md) i przenoszenie danych łatwo regularnego określonym przez użytkownika. Aby uzyskać więcej informacji i wskazówki krok po kroku dotyczące sposobu konfigurowania potok Factory danych w celu wygenerowania danych źródłowych dla analizy strumieniu, który odświeża wstępnie zdefiniowanych zgodnie z harmonogramem zapoznaj się z [próbki GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ReferenceDataRefreshForASAJobs).

## <a name="tips-on-refreshing-your-reference-data"></a>Porady na temat odświeżania danych odwołania ##

1. Zastępowania danych typu blob odwołanie nie spowoduje, że analizy strumieniu ponownie załadować to i w niektórych przypadkach może powodować to zadanie nie powiedzie się. Zalecany sposób Zmienianie danych źródłowych jest dodanie nowych obiektów blob przy użyciu samej kontenera i ścieżka wzorzec zdefiniowane w zadania wejście i używanie Data/Godzina **większa** niż określona przez ostatnią obiektów blob w sekwencji.
2.  Dane odwołanie, które obiektów blob nie są ułożone chronologicznie obiektów blob "Ostatnia modyfikacja", ale tylko godziny i daty określonej w to nazwa, za pomocą {date} i {czasu} podstawianie.
3.  Kilka razy, które zadania musisz wrócić w czasie w związku z tym odwołania danych typu BLOB musi nie należy zmieniać ani usuwać.

## <a name="get-help"></a>Uzyskiwanie pomocy
Aby uzyskać dodatkową pomoc spróbuj nasze [forum analizy strumieniu Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Następne kroki
Zostały wprowadzone do analizy strumieniu, zarządzanych usług do przesyłania strumieniowego analizy danych z Internetu rzeczy. Aby dowiedzieć się więcej o tej usłudze, zobacz:

- [Wprowadzenie do korzystania z analizy strumieniu Azure](stream-analytics-get-started.md)
- [Skalowanie Azure analizy strumieniu zadania](stream-analytics-scale-jobs.md)
- [Dokumentacja języka kwerend analizy strumieniu Azure](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Strumień Azure analizy zarządzania pozostałych interfejsu API odwołania](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
