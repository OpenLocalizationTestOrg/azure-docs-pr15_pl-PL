<properties
    pageTitle="Logika limity aplikacji i konfiguracji | Microsoft Azure"
    description="Przegląd ograniczenia usług i wartości konfiguracji dostępnych w przypadku aplikacji logicznych."
    services="logic-apps"
    documentationCenter=".net,nodejs,java"
    authors="jeffhollan"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jehollan"/>

# <a name="logic-app-limits-and-configuration"></a>Logika limity aplikacji i konfiguracji

Poniżej znajdują się informacje na temat bieżące limity i szczegóły konfiguracji Azure logiczny aplikacji.

## <a name="limits"></a>Ograniczenia

### <a name="http-request-limits"></a>Limity żądania HTTP

Są limity dla pojedynczego żądania i/lub łącznik połączenia HTTP

#### <a name="timeout"></a>Limit czasu

|Nazwa|Limit|Notatki|
|----|----|----|
|Limit czasu żądania|1 minuta|[Asynchroniczna deseniu](app-service-logic-create-api-app.md) lub [do pętli](app-service-logic-loops-and-scopes.md) zadośćuczynienia stosownie do potrzeb|

#### <a name="message-size"></a>Rozmiar wiadomości

|Nazwa|Limit|Notatki|
|----|----|----|
|Rozmiar wiadomości|50 MB|Niektóre łączniki i interfejsów API może nie obsługuje 50MB.  Żądanie wyzwalacza obsługuje maksymalnie 25MB|
|Limit obliczeń przy użyciu wyrażenia|znaki 131 072|`@concat()`, `@base64()`, `string` nie może przekraczać to|

#### <a name="retry-policy"></a>Spróbuj ponownie zasad

|Nazwa|Limit|Notatki|
|----|----|----|
|Ponowne próby|4|Można skonfigurować [Ponów próbę parametr zasad](https://msdn.microsoft.com/en-us/library/azure/mt643939.aspx)|
|Opóźnienie max|1 godzina|Można skonfigurować [Ponów próbę parametr zasad](https://msdn.microsoft.com/en-us/library/azure/mt643939.aspx)|
|Opóźnienie min|20 min|Można skonfigurować [Ponów próbę parametr zasad](https://msdn.microsoft.com/en-us/library/azure/mt643939.aspx)|

### <a name="run-duration-and-retention"></a>Uruchamianie czas trwania i przechowywania

Są limity dla aplikacji pojedynczy logiczny Uruchom.

|Nazwa|Limit|Notatki|
|----|----|----|
|Czas trwania zadania|90 dni||
|Przechowywanie miejsca do magazynowania|90 dni|To jest uruchamianie rozpoczęcia od godziny|
|Interwał cyklu min|s 15||
|Maksymalna liczba interwału cyklu|500 dni||


### <a name="looping-and-debatching-limits"></a>Powtarzanie i debatching ograniczenia

Są limity dla aplikacji pojedynczy logiczny Uruchom.

|Nazwa|Limit|Notatki|
|----|----|----|
|Elementy ForEach|5000|[Akcja kwerendy](../connectors/connectors-native-query.md) umożliwia filtrowanie większe tablice stosownie do potrzeb|
|Poczekaj, aż liczba iteracji|10 000||
|Elementy SplitOn|5000||
|Równoległości ForEach|20|Można ustawić sekwencyjne foreach, dodając `"operationOptions": "Sequential"` do `foreach` akcji|


### <a name="throughput-limits"></a>Ograniczenia przepustowości

Są limity dla logika jednego wystąpienia aplikacji. 

|Nazwa|Limit|Notatki|
|----|----|----|
|Wyzwalacze na sekundę|100|Rozpowszechniania przepływów pracy w wielu aplikacjach stosownie do potrzeb|

### <a name="definition-limits"></a>Limity definicji

Są limity dla pojedynczego logika definicji aplikacji.

|Nazwa|Limit|Notatki|
|----|----|----|
|Akcje w ForEach|1|Możesz dodać zagnieżdżone przepływów pracy, aby rozszerzyć tak, stosownie do potrzeb|
|Akcje na przepływu pracy|60|Możesz dodać zagnieżdżone przepływów pracy, aby rozszerzyć tak, stosownie do potrzeb|
|Dozwolone głębokości zagnieżdżania akcji|5|Możesz dodać zagnieżdżone przepływów pracy, aby rozszerzyć tak, stosownie do potrzeb|
|Przepływów w rozbiciu na regiony na subskrypcję|1000.||
|Wyzwalaczy na przepływu pracy|10||
|Maksymalna liczba znaków wyrażenia|8192||
|Maksymalna liczba `trackedProperties` rozmiar przy użyciu znaków|16000|
|`action`/`trigger`limit nazwy|80||
|`description`limit długości|256||
|`parameters`limit|50||
|`outputs`limit|10||

## <a name="configuration"></a>Konfiguracja

### <a name="ip-address"></a>Adres IP

Połączenia utworzonego na podstawie [Łącznik](../connectors/apis-list.md) zostanie pochodzą z określonego poniżej adresu IP.

Wywołań za pomocą logiki aplikacji bezpośrednio (to znaczy za pośrednictwem [protokołu HTTP](../connectors/connectors-native-http.md) lub [HTTP + Swagger](../connectors/connectors-native-http-swagger.md)) mogą pochodzić z dowolnej z [Zakresów adresów IP centrum danych Azure](https://www.microsoft.com/en-us/download/details.aspx?id=41653).

|Logika aplikacji Region|Wychodzącego|
|-----|----|
|Wschód Australia|40.126.251.213|
|Australia kopiec|40.127.80.34|
|Brazylia Południowej|191.232.38.129|
|Indie centralnej|104.211.98.164|
|Centralny Stany Zjednoczone|40.122.49.51|
|Wschodnioazjatyckie|23.99.116.181|
|Wschodniej Stany Zjednoczone|191.237.41.52|
|Wschodniej USA 2|104.208.233.100|
|Wschód Japonia|40.115.186.96|
|Japonia zachód|40.74.130.77|
|Ameryka Północna centralnej w Stanach Zjednoczonych|65.52.218.230|
|Europa Północna|104.45.93.9|
|Południowej centralnej Stany Zjednoczone|104.214.70.191|
|Kraje Azji Południowo|13.76.231.68|
|Indie Płd.|104.211.227.225|
|Europa Zachodnia|40.115.50.13|
|Indie zachód|104.211.161.203|
|Zachód Stany Zjednoczone|104.40.51.248|


## <a name="next-steps"></a>Następne kroki  

- Aby rozpocząć pracę z aplikacjami logiczny, wykonaj [Tworzenie aplikacji logiki](app-service-logic-create-a-logic-app.md) samouczka.  
- [Przykłady typowych widoku i scenariusze](app-service-logic-examples-and-scenarios.md)
- [Można zautomatyzować procesów biznesowych za pomocą aplikacji warunków logicznych](http://channel9.msdn.com/Events/Build/2016/T694) 
- [Dowiedz się, jak integrację systemów z aplikacjami warunków logicznych](http://channel9.msdn.com/Events/Build/2016/P462)