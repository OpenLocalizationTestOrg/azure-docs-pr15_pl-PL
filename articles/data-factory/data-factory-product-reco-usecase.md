<properties 
    pageTitle="Diagram przypadków użycia Factory danych — zalecenia produktu" 
    description="Informacje na temat przypadków użycia wykonywane przy użyciu Factory danych Azure wraz z innymi usługami." 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/01/2016" 
    ms.author="shlo"/>

# <a name="use-case---product-recommendations"></a>Za pomocą liter — zalecenia produktu 

Azure Factory danych jest jednym z wielu usług używane do wdrożenia pakietu analizy Cortana solution Accelerator.  Zobacz [Pakiet analizy Cortana](http://www.microsoft.com/cortanaanalytics) strony szczegółowe informacje na ten pakiet. W tym dokumencie opisano typowe przypadków użycia, który Azure użytkowników już rozwiązany i zaimplementować za pomocą Factory danych Azure i innych usług składnik analizy Cortana.

## <a name="scenario"></a>Scenariusz

Sklepach internetowych chcesz często nakłonić ich klientom zakup produktów prezentując z produktami, które są najczęściej Cię zainteresować i w związku z tym najprawdopodobniej do zakupu. Aby to zrobić, sklepach internetowych trzeba dostosować ich użytkownika w trybie online za pomocą spersonalizowanych produktu zalecenia dla określonego użytkownika. Te spersonalizowane rekomendacje mają zostać wprowadzone oparte na ich bieżącej i poprzednich zakupów zachowanie danych, informacje o produkcie, nowo wprowadzonych marki i produktu i segmentacji danych klientów.  Ponadto może zapewnić zalecenia produktu użytkownika na podstawie analizy ogólnego zachowania zastosowania wszystkim użytkownikom ich połączone.

Celem tych sprzedawców jest optymalizacja na potrzeby konwersji kliknij pozycję do sprzedaży użytkownika i uzyskać wyższy przychód ze sprzedaży.  Konwersja one osiągnąć oferując zalecenia kontekstowe, oparte na zachowanie produktu na podstawie zainteresowań klienta i akcje. Dla tego przypadków użycia używamy sklepach internetowych na przykład firmy, które chcesz zoptymalizować dla swoich klientów. Jednak tych zasad dotyczą każdej firmy, którą chce uczestniczyć klientom wokół jego towarów i usług i rozszerza swoich klientów zakupu z zaleceniami spersonalizowanych produktu.

## <a name="challenges"></a>Wyzwania

Istnieje wiele problemy napotykane przez sklepach internetowych podczas próby wykonania tego typu przypadków użycia. 

Najpierw dane o różnych rozmiarach i kształty musi wchłonięte z wielu źródeł danych, zarówno w lokalnej i w chmurze. Te dane zawiera dane produktu, dane zachowanie historycznych klientów i dane użytkownika, jak użytkownik przegląda witryny Sklep internetowy. 

Zalecenia drugi, spersonalizowanych produktu musi być rozsądnie i precyzyjne obliczania i przewidziane. Oprócz produktów, marki i dane klientów zachowanie i przeglądarki sklepach internetowych muszą również zawierać opinii klientów w przeszłości zakupów do współczynnika ustalaniu zaleceniach produktu dla użytkownika. 

Trzecia zalecenia należy natychmiast elementu dostarczanego do użytkownika, aby podać Bezproblemowa przeglądania i zakupów środowiska i zalecenia najbardziej aktualne i istotne. 

Na koniec sprzedawców trzeba przeszukiwarki ich podejście przez śledzenie ogólny wzrost sprzedaży i sprzedaży sukcesów sprzedaży kliknij pozycję do konwersji i dostosować do ich przyszłych zalecenia.

## <a name="solution-overview"></a>Omówienie rozwiązania

Ten przykład przypadków użycia został rozwiązany i wykonywane przez użytkowników rzeczywistego Azure za pomocą Factory danych Azure i innych usługach składnik Cortana analizy, takich jak [HDInsight](https://azure.microsoft.com/services/hdinsight/) i [Usługi Power BI](https://powerbi.microsoft.com/).

Online u sprzedawcy detalicznego używa magazyn obiektów Blob platformy Azure, programu SQL server w lokalnej bazy danych SQL Azure i aj inteligentne danych relacyjnych jako ich opcje przechowywania danych w całej przepływu pracy.  Magazyn obiektów blob zawiera informacje o klientach, dane zachowanie klienta i produktu informacje o danych. Dane informacje produktu zawierają informacje o produkcie marki i produktu wykazu magazynowane lokalnie w magazynie danych SQL. 

Wszystkie dane są łączone i do system rekomendacji produktów do dostarczania spersonalizowanych zalecenia dotyczące zainteresowań klienta i akcji, gdy użytkownik przegląda produktów w katalogu w witrynie sieci Web. Klienci Zobacz też produktów, które są związane z produktem szukanych u według upodobania ogólnego witryny sieci Web, które nie są związane z dowolnego jednego użytkownika.

![diagram przypadków użycia](./media/data-factory-product-reco-usecase/diagram-1.png)

Gigabajty pliki dziennika nieprzetworzonych sieci web są generowane codziennie w witrynie sieci Web detalicznego online jako pliki półstrukturalnych. Pliki dziennika nieprzetworzonych sieci web i produktu i odbiorcy informacji o wykazie jest regularnego zasysanego do magazyn obiektów Blob platformy Azure, za pomocą przenoszenia danych globalnie rozmieszczonych Factory danych jako usługa. Pliki dziennika nieprzetworzonych w danym dniu oddzielone od siebie (według roku i miesiąca) w magazynie obiektów blob dla długotrwałego przechowywania.  [Usługa Azure HDInsight](https://azure.microsoft.com/services/hdinsight/) jest używany do partycje plików dziennika nieprzetworzonych w magazynie obiektów blob i procesu zasysanego dzienniki w skali skryptów zarówno gałęzi i świnka. Podzielony na partycje web rejestruje dane są następnie przetwarzane wyodrębnić potrzebnych danych wejściowych dla maszynowego uczenia rekomendacji system w celu wygenerowania zalecenia spersonalizowanych produktu.

System rekomendacji na potrzeby maszynowego uczenia się w tym przykładzie jest maszynowego Otwórz źródło uczenia platformy rekomendacji z [Apache Mahout](http://mahout.apache.org/).  Wszelkie [Azure maszynowego uczenia](https://azure.microsoft.com/services/machine-learning/) lub niestandardowe modelu można stosować do tego scenariusza.  Mahout model jest używany przewidywanie podobieństwa między elementami w witrynie sieci Web, na podstawie ogólnego wzorców użycia oraz generowanie spersonalizowane rekomendacje oparte na poszczególnych użytkowników.

Na koniec zestawu wyników zalecenia spersonalizowanych produktu jest przenoszony do aj inteligentne danych relacyjnych zużycia przez witrynę sieci Web u sprzedawcy detalicznego.  Zestaw wyników również może być dostępne bezpośrednio z magazynem obiektów blob w innej aplikacji lub przeniesiona do dodatkowych sklepów dla innych odbiorców i przypadków użycia.

## <a name="benefits"></a>Zalety

Optymalizacja ich strategii rekomendacji produktu i wyrównywania go do celów biznesowych, rozwiązanie spełnione promocji detalicznego online i marketingowych cele. Ponadto mieli operationalize i zarządzać nimi przepływu pracy rekomendacji produktów w sposób wydajność, wiarygodnych i skutecznej kosztów. Podejście wprowadzone łatwo aktualizować ich modelu oraz dostosowywanie skuteczność na podstawie miar sprzedaży sukcesów kliknij pozycję do konwersji. Za pomocą Factory danych Azure, mieli opuszczenie ich czasochłonne i drogich chmury ręcznego zarządzania zasobami i przechodzenie do zarządzania zasobami w chmurze na żądanie. W związku z tym były można zaoszczędzić czas, pieniądze i skrócenie czasu ich wdrażania rozwiązania. Dane dotyczące elementów nadrzędnych widoków i kondycji usługi operacyjne stał się ułatwia wizualizowanie i rozwiązywanie problemów z intuicyjny monitorowanie Factory danych i zarządzanie dostępne z poziomu portalu Azure interfejsu użytkownika. Można teraz zaplanowane ich rozwiązanie i zarządzane tak, aby na koniec danych jest prawidłowo wyprodukowano i dostarczane do użytkowników i danych i zależności przetwarzanie odbywa się automatycznie bez udziału rozmówcy.

Dostarcz ten spersonalizowanej zawartości zakupów online u sprzedawcy detalicznego utworzone konkurencyjności, przyciągają uwagę odbiorców daje i w związku z tym zwiększanie sprzedaży i ogólnego zadowolenia klientów.



  