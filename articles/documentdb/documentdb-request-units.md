<properties 
    pageTitle="Żądanie jednostki w DocumentDB | Microsoft Azure" 
    description="Informacje na temat zrozumieć, określ i ocenić wymagania jednostki żądania DocumentDB." 
    services="documentdb" 
    authors="syamkmsft" 
    manager="jhubbard" 
    editor="mimig" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/29/2016" 
    ms.author="syamk"/>

#<a name="request-units-in-documentdb"></a>Żądanie jednostki w DocumentDB
Teraz dostępna: DocumentDB [Kalkulator jednostek wezwanie](https://www.documentdb.com/capacityplanner). Dowiedz się, jak w [Estimating musi przepustowość sieci](documentdb-request-units.md#estimating-throughput-needs).

![Kalkulator przepustowości][5]

##<a name="introduction"></a>Wprowadzenie
Ten artykuł zawiera omówienie jednostek żądania w [Programie Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/). 

Po zapoznaniu się w tym artykule, będziesz mieć możliwość odpowiedzieć na następujące pytania:  

-   Co to są żądania jednostki i zażądać opłaty?
-   Jak określić zdolności jednostki żądania dla zbioru?
-   Jak oszacować jednostki żądania Moja aplikacja musi?
-   Co się stanie, gdy można przekraczać zdolności jednostki żądania dla zbioru?


##<a name="request-units-and-request-charges"></a>Żądanie jednostki i poproś opłat
DocumentDB zapewnia szybki, przewidywalne wydajność przez *rezerwowania* zasobów, aby spełnić musi przepustowość aplikacji.  Ponieważ aplikacja ładowanie i uzyskać dostęp do wzorców zmian w czasie, DocumentDB umożliwia łatwe zwiększenie lub zmniejszenie ilości zastrzeżone przepustowość dostępne w aplikacji.

Z DocumentDB zastrzeżone przepustowość jest określona w odniesieniu do jednostki żądania przetwarzania na sekundę.  Żądanie jednostki można traktować jako waluty przepustowość, według których *rezerwowanie* i ilość gwarantowana jednostki żądania dostępne w aplikacji na podstawie jednego drugiego.  Kolejne operacje w DocumentDB — zapisywania dokumentu, wykonywanie kwerendy, aktualizowanie dokumentu — powoduje zużycie Procesora, pamięci i operacji i/o na SEKUNDĘ.  Oznacza to, że kolejne operacje wiąże się *opłaty żądanie*, wyrażona w *jednostkach wezwanie*.  Opis czynników, które wpływają na żądanie jednostki opłat, wraz z wymagania dotyczące przepustowości aplikacji, umożliwia uruchomienie aplikacji jako koszt najbardziej skuteczny sposób. 

##<a name="specifying-request-unit-capacity"></a>Określanie pojemności jednostek żądanie
Tworząc zbiór DocumentDB, określ liczbę jednostek wezwanie na sekundę (RUs) ma być zarezerwowane dla zbioru.  Po utworzeniu kolekcji pełny przyznawania RUs określony jest zarezerwowana do użytku kolekcji.  Każda z kolekcji jest gwarantowana dedykowane i właściwości przepustowość samodzielnie.  

Należy pamiętać, że DocumentDB działa na modelu rezerwacji; oznacza to, że są wystawiona przez przepustowość *zastrzeżone* pobierania, niezależnie od tego, jaka część tej przepustowość jest aktywnie *używane*.  Należy pamiętać, jednak, która jako aplikacji ładowania danych i zastosowania desenie zmiany, którą można łatwo skalować liczbę w górę lub w dół zastrzeżone RUs za pośrednictwem SDK DocumentDB lub za pomocą [Azure Portal](https://portal.azure.com).  Aby uzyskać więcej informacji na przepustowość skala w górę i w dół zobacz [poziomy wydajności DocumentDB](documentdb-performance-levels.md).

##<a name="request-unit-considerations"></a>Zagadnienia dotyczące jednostki żądanie
Szacowanie liczby jednostek żądanie zarezerwować zbioru DocumentDB, należy wziąć pod uwagę następujące zmienne:

- **Rozmiar dokumentu**. Jednostki wykorzystana do odczytu lub zapisu danych zwiększa zwiększania rozmiaru dokumentu.
- **Licznik właściwości dokumentu**. Indeksowanie przyjęcia domyślne wszystkich właściwości jednostki zużyte do napisania dokumentu zwiększa wraz ze wzrostem liczby właściwości.
- **Spójność danych**. Podczas korzystania z poziomu spójności danych silne lub ograniczone Staleness dodatkowych jednostek będzie wykorzystana do czytanie dokumentów.
- **Właściwości indeksowane**. Indeks zasad każdego zbioru określa właściwości, które są indeksowane domyślnie. Można ograniczyć zużycie jednostek usługi żądanie, przez ograniczenie liczby właściwości indeksowanych lub włączyć indeksowanie z opóźnieniem.
- **Indeksowanie dokumentu**. Domyślnie każdy dokument jest automatycznie indeksowane Jeśli nie chcesz indeksować niektórych dokumentów będzie korzystanie ze mniej jednostek wezwanie.
- **Desenie kwerendy**. Złożoność kwerendy wpływa na liczbę jednostek żądanie są używane dla operacji. Liczba orzeczenia, rodzaj orzeczenia, planów, liczba funkcji zdefiniowanych przez użytkownika oraz rozmiar zbioru źródeł danych wszystkich wpływać koszt operacji kwerendy.
- **Użycie skryptu**.  Podobnie jak w przypadku kwerend, procedur składowanych wyzwalaczy zajmować jednostkach żądania w oparciu o złożoność operacji. Podczas opracowywania aplikacji inspekcja nagłówek opłaty żądanie, aby lepiej zrozumieć, jak każdorazowym jest przez inne możliwości jednostki wezwanie na.

##<a name="estimating-throughput-needs"></a>Wymaga planowania przepustowość
Jednostka żądania jest miarą znormalizowaną koszt przetwarzania żądania. Jednostka pojedynczego żądania reprezentuje możliwości przetwarzania wymagane do odczytu (przez siebie łącze lub identyfikator) pojedynczy dokument JSON 1KB składający się z 10 unikatowych wartości (z wyłączeniem właściwości systemu). Żądanie, aby utworzyć (Wstaw), zamienianie lub usuwanie tego samego dokumentu zajmie więcej przetwarzania przez usługę i w związku z tym większej liczby jednostek wezwanie.   

> [AZURE.NOTE] Plan bazowy 1 żądanie jednostki w dokumencie 1KB odpowiada prosty GET przez siebie łącze lub identyfikator dokumentu.

###<a name="use-the-request-unit-calculator"></a>Za pomocą kalkulatora jednostki żądanie
Aby pomóc klientów poprawnie dostosować ich oszacowaniem przepustowość, jest sieci web [kalkulatora jednostki żądania](https://www.documentdb.com/capacityplanner) ułatwiające oszacować wymagania jednostki żądania dotyczące typowych operacji, w tym:

- Tworzy dokument (zapisuje)
- Odczytuje dokumentu
- Usuwa dokument
- Aktualizacje dokumentu

Narzędzie obsługuje również szacowanie potrzeb miejsca do magazynowania danych według przykładowych dokumentów, podane.

Za pomocą narzędzia jest proste:

1. Przekazywanie jednego lub kilku przedstawiciela dokumentów JSON.

    ![Przekazywanie dokumentów do Kalkulatora jednostki żądanie][2]

2. Aby oszacować wymagania dotyczące przechowywania danych, wprowadź całkowitą liczbę dokumentów, których oczekujesz do przechowywania.

3. Wprowadź numer dokumentu tworzenie, odczytywanie, aktualizowanie i usuwanie operacje wymagają (na podstawie na sekundę). Aby oszacować opłat jednostek żądanie operacje aktualizacji dokumentu, Przekaż kopię przykładowy dokument z kroku 1 zawiera typowe pole aktualizacje.  Na przykład jeśli aktualizacje dokumentu zmodyfikować zazwyczaj dwie właściwości o nazwie lastLogin i userVisits, następnie po prostu skopiuj przykładowy dokument, aktualizowanie wartości dla tych dwóch właściwości i Przekaż dokument skopiowane.

    ![Wprowadź wymagania dotyczące przepustowości w kalkulatora jednostki żądanie][3]

4. Kliknij pozycję Obliczanie i przejrzeć wyniki.

    ![Żądanie wyniki Kalkulator jednostek][4]

>[AZURE.NOTE]Jeśli masz typów dokumentów, które będą różnią się znacznie pod względem rozmiaru i liczby właściwości indeksowane, a następnie przekazanie próbki każdego *typu* dokumentu typowy do narzędzia, a następnie pozycję Oblicz wyniki.

###<a name="use-the-documentdb-request-charge-response-header"></a>Za pomocą nagłówka odpowiedzi DocumentDB żądanie opłat
Każda odpowiedź z usługi DocumentDB zawiera niestandardowy nagłówek (x-ms żądanie-opłat za), który zawiera jednostki żądania zużyte żądania. Ten nagłówek jest również dostępne za pośrednictwem SDK DocumentDB. W zestawie SDK .NET RequestCharge jest właściwością obiektu ResourceResponse.  W przypadku kwerend DocumentDB Explorer kwerendy w portalu Azure informacje wezwanie na opłaty wykonane kwerend.

![Badanie opłaty RU w Eksploratorze kwerendy][1]

Mając to na uwadze, jedną z metod szacowanie, że ilość zarezerwowana przepustowość wymagane przez aplikację służy do rejestrowania opłatę jednostkową żądanie skojarzone z uruchomionymi typowych operacji przed przedstawiciela dokumentu używane przez aplikację, a następnie Szacowanie liczbę operacji przewiduje się wykonywanie sekundę.  Pamiętaj o Zmierz i uwzględnić typowych kwerend i DocumentDB skrypt także zastosowania.

>[AZURE.NOTE]Jeśli masz typów dokumentów, które będą różnią się znacznie pod względem rozmiaru i liczby właściwości indeksowane, następnie rekord skojarzone z każdego *typu* dokumentu typowy opłatę jednostkową dotyczy operacja wezwanie.

Na przykład:

1. Rejestrowanie opłatę jednostkową żądania tworzenia (Wstawianie) typowy dokumentu. 
2. Rekord opłaty jednostkowej żądanie czytania typowego dokumentu.
3. Rekord opłatę jednostkową żądania aktualizacji dokumentu typowy.
3. Rekord opłaty jednostkowej żądanie dokumentu typowy, typowych kwerend.
4. Rejestrowanie opłatę jednostkową żądanie wszelkie własnych skryptów (procedur składowanych, wyzwalaczy, funkcji zdefiniowanych przez użytkownika) osobie przez aplikację
5. Obliczanie jednostek wymagane żądania podanych liczbę operacji, które przewiduje się do uruchomienia sekundę.

##<a name="a-request-unit-estimation-example"></a>Przykład oszacowanie jednostki żądanie
Należy rozważyć następujące dokumentu ~ 1KB:

    {
     "id": "08259",
    "description": "Cereals ready-to-eat, KELLOGG, KELLOGG'S CRISPIX",
    "tags": [
        {
        "name": "cereals ready-to-eat"
        },
        {
        "name": "kellogg"
        },
        {
        "name": "kellogg's crispix"
        }
    ],
    "version": 1,
    "commonName": "Includes USDA Commodity B855",
    "manufacturerName": "Kellogg, Co.",
    "isFromSurvey": false,
    "foodGroup": "Breakfast Cereals",
    "nutrients": [
        {
        "id": "262",
        "description": "Caffeine",
        "nutritionValue": 0,
        "units": "mg"
        },
        {
        "id": "307",
        "description": "Sodium, Na",
        "nutritionValue": 611,
        "units": "mg"
        },
        {
        "id": "309",
        "description": "Zinc, Zn",
        "nutritionValue": 5.2,
        "units": "mg"
        }
    ],
    "servings": [
        {
        "amount": 1,
        "description": "cup (1 NLEA serving)",
        "weightInGrams": 29
        }
    ]
    }

>[AZURE.NOTE]Dokumenty są minified w DocumentDB, aby system obliczeniowe rozmiar powyżej dokumentu jest nieco mniejsza niż 1 KB.


W poniższej tabeli przedstawiono przybliżonego żądanie jednostki opłaty za typowych operacji dla tego dokumentu (opłatę jednostkową przybliżonego żądanie założono poziomu spójności konto jest równa "Sesji" i że wszystkie dokumenty są automatycznie indeksowane):

Operacja|Żądanie opłatę jednostkową 
---|---
Tworzenie dokumentu|~ 15 RU 
Przejrzyj dokument|~ 1 RU
Dokument kwerendy według identyfikatorów|~2.5 RU

Ponadto w tej tabeli opisano przybliżonego żądanie opłaty jednostki dotyczące typowych kwerend używanych w aplikacji:

Kwerendy|Żądanie opłatę jednostkową|# Zwrócone dokumentów
---|---|--- 
Wybierz pozycję jedzenie według identyfikatorów|~2.5 RU|1 
Zaznacz artykuły spożywcze przez producenta|~ 7 RU|7
Wybierz grupę jedzenie i kolejności wagi|~ 70 RU|100
Zaznacz górny 10 artykuły spożywcze w grupie żywność|~ 10 RU|10

>[AZURE.NOTE]Opłaty RU zależą od liczby zwracanych dokumentów.

Przy użyciu tych informacji firma Microsoft oszacować wymagania RU dla tej aplikacji danej liczby operacji i kwerendy Oczekujemy na sekundę:

Operacja/kwerendy|Liczbę na sekundę|Wymagane RUs 
---|---|--- 
Tworzenie dokumentu|10|150 
Przejrzyj dokument|100|100 
Zaznacz artykuły spożywcze przez producenta|25|175 
Wybierz pozycję według grupy żywność|10|700 
Wybierz pozycję 10 pierwszych|15|150 sumy|155|1275

W tym przypadku oczekujemy wymagania Średnia produktywność 1,275 RU/s.  Zaokrąglanie do najbliższej 100, firma Microsoft może zapewnić 1,300 RU-s kolekcji tej aplikacji.

##<a id="RequestRateTooLarge"></a>Przekraczającym ograniczenia przepustowości zastrzeżone
Odwoływanie, że zużycia żądania jest obliczana jako stopa na sekundę. Dla aplikacji, które wykraczają poza liczby jednostek ustanawianie żądań dla zbioru żądania do kolekcji będzie ograniczenie, aż wskaźnik pomija poniżej poziomu zastrzeżone. W przypadku wystąpienia ograniczenia serwer preemptively zakończyć wezwanie z RequestRateTooLargeException (kod stanu HTTP 429) i nagłówek x-ms-ponów próbę po ms wskazujący czas (w milisekundach), które użytkownik musi upłynąć interwał ponawiania żądania zwrotu.

    HTTP Status 429
    Status Line: RequestRateTooLarge
    x-ms-retry-after-ms :100

Jeśli korzystasz z kwerend .NET SDK klienta i LINQ, a następnie w większości przypadków, które nigdy nie trzeba postępować z tym wyjątkiem, zgodnie z bieżącej wersji programu .NET SDK klienta niejawnie połowy odpowiedź, uwzględnia określony serwer ponów próbę po nagłówka, a prób wezwanie. Chyba że Twoje konto jest dostępny jednocześnie przez wielu klientów, następną próbą powiedzie.

Jeśli masz więcej niż jeden klient łącznie operacyjnym powyżej stopy wezwanie na domyślne zachowanie ponów próbę mogą być niewystarczające, a klient będzie Zgłoś DocumentClientException z kodem stanu 429 do aplikacji. W przypadku takich jak można rozważyć Obsługa zachowanie ponów próbę i reguł w aplikacji błąd obsługi procedur lub zwiększenie zastrzeżone przepustowości dla zbioru.

##<a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej na temat przepustowości zastrzeżone z bazami danych Azure DocumentDB, zapoznaj się z następujących zasobów:
 
- [DocumentDB cennik](https://azure.microsoft.com/pricing/details/documentdb/)
- [Zarządzanie zdolności DocumentDB](documentdb-manage.md) 
- [Modelowanie danych w DocumentDB](documentdb-modeling-data.md)
- [Poziomy wydajności DocumentDB](documentdb-partition-data.md)

Aby dowiedzieć się więcej na temat DocumentDB, zobacz Azure DocumentDB [dokumentacji](https://azure.microsoft.com/documentation/services/documentdb/). 

Aby rozpocząć pracę przy użyciu skali i testowania z DocumentDB, zobacz [Wydajność i skali testowanie za pomocą Azure DocumentDB](documentdb-performance-testing.md).


[1]: ./media/documentdb-request-units/queryexplorer.png 
[2]: ./media/documentdb-request-units/RUEstimatorUpload.png
[3]: ./media/documentdb-request-units/RUEstimatorDocuments.png
[4]: ./media/documentdb-request-units/RUEstimatorResults.png
[5]: ./media/documentdb-request-units/RUCalculator2.png
