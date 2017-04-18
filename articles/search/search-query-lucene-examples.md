<properties
    pageTitle="Przykłady Lucene kwerendy wyszukiwania Azure | Wyszukiwanie Microsoft Azure"
    description="Lucene składnię kwerendy wyszukiwania rozmyty, odległość wyszukiwania zwiększenia terminów, wyszukiwanie wyrażeń regularnych i wyszukiwania symboli wieloznacznych."
    services="search"
    documentationCenter=""
    authors="LiamCa"
    manager="pablocas"
    editor=""
    tags="Lucene query analyzer syntax"
/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="liamca"
/>

# <a name="lucene-query-syntax-examples-for-building-queries-in-azure-search"></a>Przykłady składni kwerendy Lucene dla konstruowania kwerend wyszukiwania Azure

Podczas tworzenia kwerend wyszukiwania Azure, można się domyślną [składnią kwerendy prostej](https://msdn.microsoft.com/library/azure/dn798920.aspx) lub alternatywny [Lucene parsera kwerendy wyszukiwania Azure](https://msdn.microsoft.com/library/azure/mt589323.aspx). Analizator kwerendy Lucene obsługuje bardziej złożone konstrukcji, kwerendy, takich jak występujące pól kwerendy, rozmyty wyszukiwania wyszukiwanie odległość, zwiększenia terminów i reqular wyrażenie wyszukiwania.

W tym artykule można przejrzeć przykłady wyświetlić Lucene składnię kwerendy i wyniki obok siebie. Przykłady uruchamiana wstępnie załadowanego indeksu wyszukiwania w [JSFiddle](https://jsfiddle.net/), edytora kodu online do testowania skryptu i HTML. 

Kliknij prawym przyciskiem myszy kwerendę adresy URL, aby otworzyć JSFiddle w osobnym oknie przeglądarki.

> [AZURE.NOTE] Poniższe przykłady wykorzystać indeks wyszukiwania składający się z zadania dostępne w oparciu o dostarczony przez initiative [Miasta Nowy Jork OpenData](https://nycopendata.socrata.com/) zestaw danych. Nie należy rozważyć te dane bieżącą lub ukończone. Indeks jest w trybie piaskownicy oferowane przez firmę Microsoft. Azure subskrypcji lub wypróbuj te kwerendy wyszukiwania Azure nie ma potrzeby.

## <a name="viewing-the-examples-in-this-article"></a>Wyświetlanie w przykładach w tym artykule

Określ wszystkich przykładach w tym artykule parsera kwerendy Lucene przez parametr**queryType** wyszukiwania. Gdy używasz parsera kwerendy Lucene w kodzie, będzie określ **queryType** na każde żądanie.  Prawidłowe wartości to **proste**|**Pełny**, przy użyciu **prostego** jako domyślny i **Pełny** dla analizatora kwerendy Lucene. Zobacz [Wyszukiwania dokumentów (Azure wyszukiwania usługi interfejsu API usługi REST)](https://msdn.microsoft.com/library/azure/dn798927.aspx) szczegółowe informacje na temat określania parametry kwerendy.

**Przykład 1** — kliknij prawym przyciskiem myszy poniższa kwerenda wstawek, aby go otworzyć na nowej stronie przeglądarki, która ładuje JSFiddle i uruchamia kwerendę:
- [& queryType = pełny i wyszukiwanie = *](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26searchFields=business_title%26$select=business_title%26queryType=full%26search=*)

Ta kwerenda zwraca dokumentów z indeksu zadań (załadowanego w usłudze piaskownicy)

W nowym oknie przeglądarki pojawi się źródłowy języka JavaScript i wyjściowe HTML obok siebie. Skrypt odwołuje się do kwerendy, który jest dostarczany przez adresy URL, w tym artykule. Na przykład na **przykład 1**, kwerendy źródłowej jest następująca:

    http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26searchFields=business_title%26$select=business_title%26queryType=full%26search=*

Powiadomienie o kwerendy użyto funkcji wstępnie indeks wyszukiwania Azure o nazwie nycjobs. Parametr **searchFields** ogranicza wyszukiwanie tylko z polem tytuł firm. **QueryType** jest ustawiona na **Pełny**, która powoduje, że wyszukiwanie Azure za pomocą parsera kwerendy Lucene dla tej kwerendy.

### <a name="fielded-query-operation"></a>Operacja fielded kwerendy

W przykładach w tym artykule można zmieniać, określając budowie **fieldname:searchterm** , aby zdefiniować fielded kwerendę, gdzie pole jest pojedynczego wyrazu, a wyszukiwany termin jest również pojedynczy wyraz lub frazę, opcjonalnie z operatorów logicznych. Oto kilka przykładów:

- business_title:(senior NOT junior)
- Stan: ("Warszawa" i "Nowy Jersey")

Pamiętaj umieścić wiele ciągi w cudzysłowie, jeśli chcesz, oba ciągi obliczane jako całość, tak jak w tym przypadku wyszukiwanie dwa różne miasta w polu Lokalizacja. Upewnij się również, operator jest litery, jak widać z nie i AND.

Pole określone w **fieldname:searchterm** musi być polem wyszukiwania. Zobacz [Create Index (Azure wyszukiwania usługi interfejsu API usługi REST)](https://msdn.microsoft.com/library/azure/dn798941.aspx) szczegółowe informacje na temat sposobu używania atrybuty indeksu w definicji pola.

## <a name="fuzzy-search"></a>Rozmyty wyszukiwania

Rozmyty wyszukiwania znajduje dopasowania w mające podobnego budowie. Na [dokumentacji Lucene](https://lucene.apache.org/core/4_10_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html)rozmyty wyszukiwania są oparte na [Odległość Damerau Levenshtein](https://en.wikipedia.org/wiki/Damerau%e2%80%93Levenshtein_distance).

Aby wykonać rozmyty wyszukiwania, użyj tylda "~" symbol na końcu jednego wyrazu z parametr opcjonalny, wartość od 0 do 2, która określa odległość edycji. Na przykład "niebieskie ~" lub "niebieski ~ 1" zwróci niebieski, niebieskie i przyklejanie.

**Przykład 2** — kliknij prawym przyciskiem myszy następujące wstawek kwerendy do spróbuj. Ta kwerenda wyszukuje tytuły biznesowych z wyższych terminów w nich, ale nie młodszych:

- [& queryType = pełny i wyszukiwanie = business_title:senior nie młodszych](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:senior+NOT+junior)

## <a name="proximity-search"></a>Odległość wyszukiwania

Odległość wyszukiwania są używane do znajdowania warunki, które są obok siebie w dokumencie. Wstawianie tyldy "~" symbol na końcu frazę obserwowane przez liczbę wyrazów, które tworzenie krawędzi odległość. Na przykład "hotel lotnisk" ~ 5 znajdzie hotel terminy i lotnisk w obrębie wyrazów 5 każdego z pozostałych w dokumencie.

**Przykład 3** — kliknij prawym przyciskiem myszy następujące wstawek kwerendy. Ta kwerenda wyszukuje zadania Skojarz terminów (miejsce, w którym go jest błędnie wpisana):

- [& queryType = pełny i wyszukiwanie = business_title:asosiate ~](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:asosiate~)

**Przykład 4** — kliknij prawym przyciskiem myszy kwerendę. Wyszukaj zadania, dla których termin "starszy analityka" gdzie oddzielone nie więcej niż jeden wyraz:

- [& queryType = pełny i wyszukiwanie = business_title: "starszy analityka" ~ 1](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:%22senior%20analyst%22~1)

**Przykład 5** — spróbuj go ponownie usuwanie wyrazów między terminów "starszy analityka".

- [& queryType = pełny i wyszukiwanie = business_title: "starszy analityka" ~ 0](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:%22senior%20analyst%22~0)

## <a name="term-boosting"></a>Terminów zwiększający

Zwiększenia terminów odwołuje się do klasyfikowania dokumentu wyższą, jeśli zawiera ono boosted terminów, względem dokumentów, które nie zawierają termin. To różni się od wyników profile, w tym wyników profile zwiększyć niektórych pól, a nie określonych warunków. Poniższy przykład pozwala zilustrować różnice.

Należy rozważyć, czy odpowiada wyników profil, który zwiększa w pewnym polu, takich jak **gatunek** w tym przykładzie musicstoreindex. Zwiększenia terminów można dodatkowo zwiększyć niektórych wyszukiwania terminów wyższymi niż inne osoby. Na przykład "skale ^ 2 elektronicznej" będzie zwiększyć dokumenty, które zawierają wyszukiwanych terminów w polu **gatunek** wyższymi niż inne pola wyszukiwania w indeksie. Ponadto dokumenty zawierające frazę "skale" będą wyżej niż inne frazę "elektronicznej" w wyniku wartość zwiększenie wydajności terminów (2).

Aby zwiększyć terminu, należy użyć daszka, "^", symbol ze wskaźnikiem zwiększenie wydajności (liczba) na końcu okresu podczas wyszukiwania. Wyższa współczynnika zwiększenie wydajności, trafniejsze termin będzie względem innych wyszukiwanych terminów. Domyślnie współczynnik zwiększenie wydajności wynosi 1. Mimo że współczynnik zwiększenie wydajności musi być dodatnia, może być mniejsza niż 1 (na przykład 0,2).

**Przykład 6** — kliknij prawym przyciskiem myszy kwerendę. Wyszukaj zadania, dla których termin "komputera analityka" miejsce, w którym widać, nie są wyniki z komputera wyrazów i analityka, ale analityk zadania są w górnej części wyników.

- [& queryType = pełny i wyszukiwanie = analityka business_title:computer](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:computer%5e2%20analyst)

**Przykład 7** – spróbuj go ponownie, ten czas zwiększenia wyników z komputerem terminów na analityka terminów, jeśli oba te wyrazy nie istnieje.

- [& queryType = pełny i wyszukiwanie = business_title:computer ^ 2 analityka](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:computer%5e2%20analyst)

## <a name="regular-expression"></a>Wyrażenie

Wyszukiwanie wyrażeń regularnych znajduje dopasowanie na podstawie zawartości między kreska ułamkowa "/", jako udokumentowane [klasy RegExp](http://lucene.apache.org/core/4_10_2/core/org/apache/lucene/util/automaton/RegExp.html).

**Przykład 8** — kliknij prawym przyciskiem myszy kwerendę. Wyszukaj zadania albo termin wyższych lub inny poziom.

- `&queryType=full&$select=business_title&search=business_title:/(Sen|Jun)ior/`

Adres URL w tym przykładzie nie będą renderowane prawidłowo na stronie. Obejść ten problem Skopiuj poniższy adres URL i wklej go w przeglądarce adres URL:    `http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26queryType=full%26$select=business_title%26search=business_title:/(Sen|Jun)ior/)`


## <a name="wildcard-search"></a>Symbol wieloznaczny wyszukiwania

Możesz użyć składni powszechnie dla wielu (\*) lub pojedynczego (?) znaków wieloznacznych wyszukiwania. Uwaga parsera kwerendy Lucene obsługuje stosowania tych symboli za pomocą pojedynczy termin, a nie jedno zdanie.

**Przykład 9** — kliknij prawym przyciskiem myszy kwerendę. Wyszukiwanie zadań, które zawiera prefiksu programu, które zawierałoby tytuły biznesowych z warunkami programowania programisty w nim.

- [& queryType = pełnej & $select = business_title & wyszukiwania = business_title:prog*](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26queryType=full%26$select=business_title%26search=business_title:prog*)

Nie można używać * lub? symbol jako pierwszy znak wyszukiwania.


## <a name="next-steps"></a>Następne kroki

Spróbuj określić parsera kwerendy Lucene w kodzie. Poniższe łącza wyjaśniono, jak skonfigurować kwerend wyszukiwania dla .NET i interfejsu API usługi REST. Linki za pomocą domyślną składnią proste, należy zastosować materiału z tego artykułu, aby określić **queryType**.

- [Kwerenda Azure indeksu wyszukiwania przy użyciu zestawu SDK .NET](search-query-dotnet.md)
- [Kwerenda indeksu wyszukiwania Azure za pomocą interfejsu API usługi REST](search-query-rest-api.md)


 