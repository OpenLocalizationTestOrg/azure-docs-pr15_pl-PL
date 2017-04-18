<properties
 pageTitle="Przewodnik dewelopera — języka kwerend | Microsoft Azure"
 description="Azure IoT Centrum deweloperów guide - opis języka kwerend używane do pobierania informacji o twins, metody i zadań z Twoim Centrum IoT"
 services="iot-hub"
 documentationCenter=".net"
 authors="fsautomata"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="elioda"/>

# <a name="reference---query-language-for-twins-and-jobs"></a>Odwołanie — języka kwerend twins i zadania

## <a name="overview"></a>Omówienie

Centrum IoT zawiera zaawansowanych języka SQL podobne do pobierania informacji dotyczących [twins urządzenia] [ lnk-twins] i [zadań][lnk-jobs]. W tym artykule przedstawiono:

* Wprowadzenie do główne funkcje języka kwerend IoT Centrum, i
* Szczegółowy opis języka.

## <a name="getting-started-with-twin-queries"></a>Wprowadzenie do kwerend podwójnego

[Urządzenie twins] [ lnk-twins] może zawierać obiekty, dowolnego JSON jako znaczników i właściwości. Centrum IoT umożliwia twins urządzenia kwerendy jako pojedynczy dokument JSON zawierający wszystkie informacje podwójnego.
Załóżmy, na przykład, że usługi twins Centrum IoT ma następującą strukturę:

        {                                                                      
            "deviceId": "myDeviceId",                                            
            "etag": "AAAAAAAAAAc=",                                              
            "tags": {                                                            
                "location": {                                                      
                    "region": "US",                                                  
                    "plant": "Redmond43"                                             
                }                                                                  
            },                                                                   
            "properties": {                                                      
                "desired": {                                                       
                    "telemetryConfig": {                                             
                        "configId": "db00ebf5-eeeb-42be-86a1-458cccb69e57",            
                        "sendFrequencyInSecs": 300                                          
                    },                                                               
                    "$metadata": {                                                   
                    ...                                                     
                    },                                                               
                    "$version": 4                                                    
                },                                                                 
                "reported": {                                                      
                    "connectivity": {                                                
                        "type": "cellular"                            
                    },                                                               
                    "telemetryConfig": {                                             
                        "configId": "db00ebf5-eeeb-42be-86a1-458cccb69e57",            
                        "sendFrequencyInSecs": 300,                                         
                        "status": "Success"                                            
                    },                                                               
                    "$metadata": {                                                   
                    ...                                                
                    },                                                               
                    "$version": 7                                                    
                }                                                                  
            }                                                                    
        }

Centrum IoT udostępnia twins urządzenia jako zbiór dokumentów o nazwie **urządzenia**.
Dlatego poniższe zapytanie pobiera cały zestaw twins urządzenia:

        SELECT * FROM devices

> [AZURE.NOTE] [SDK Centrum IoT] [ lnk-hub-sdks] obsługuje stronicowania dużych wyników.

Centrum IoT zapewnia pobrać twins filtrowanie z dowolnego warunków. Na przykład

        SELECT * FROM devices
        WHERE tags.location.region = 'US'

pobiera twins znacznikiem **location.region** Ustawianie z **NAMI**.
Operatory logiczne i arytmetyczne, porównania są obsługiwane także, np.

        SELECT * FROM devices
        WHERE tags.location.region = 'US'
            AND properties.reported.telemetryConfig.sendFrequencyInSecs >= 60

pobiera wszystkie twins znajduje się w Stanach Zjednoczonych skonfigurowany do wysyłania telemetrycznego rzadziej co minutę. Dla wygody również jest możliwe używanie stałych tablicowych w połączeniu z **w** i operatorów **nW** (nie w). Na przykład

        SELECT * FROM devices
        WHERE property.reported.connectivity IN ['wired', 'wifi']

pobiera wszystkie twins, które zgłoszone sieci Wi-Fi lub przewodowej łączności. Zapoznaj się z [klauzuli WHERE] [ lnk-query-where] sekcji dla odwołania do pełnego funkcje filtrowania.

Grupowanie i agregacje są również obsługiwane. Na przykład

        SELECT properties.reported.telemetryConfig.status AS status,
            COUNT() AS numberOfDevices
        FROM devices
        GROUP BY properties.reported.telemetryConfig.status

Zwraca liczbę urządzeń w każdym stanie konfiguracji telemetrycznego.

        [
            {
                "numberOfDevices": 3,
                "status": "Success"
            },
            {
                "numberOfDevices": 2,
                "status": "Pending"
            },
            {
                "numberOfDevices": 1,
                "status": "Error"
            }
        ]

W powyższym przykładzie przedstawiono sytuację miejsce, w którym trzech urządzeń zgłoszone pomyślnie konfiguracji, dwa są nadal stosowania konfiguracji i jedną zgłoszone komunikat o błędzie.

### <a name="c-example"></a>Przykład C#

Funkcje kwerend są wyświetlane przez [C# usługi SDK] [ lnk-hub-sdks] w klasie **RegistryManager** .
Oto przykład prostej kwerendy:

        var query = registryManager.CreateQuery("SELECT * FROM devices", 100);
        while (query.HasMoreResults)
        {
            var page = await query.GetNextAsTwinAsync();
            foreach (var twin in page) 
            {
                // do work on twin object
            }
        }

Należy zauważyć, jak obiektu **kwerendy** zostanie uruchomiony przy użyciu rozmiaru strony (maksymalnie 1000), a następnie wiele stron mogą być pobierane przez wywołanie metody **GetNextAsTwinAsync** wiele razy.
Należy pamiętać, że obiekt kwerendy udostępnia wiele **następnej\***, w zależności od opcji deserializacji wymagane przez kwerendę, to znaczy sobie podwójny lub zadania obiektów lub zwykły Json może być używany w przypadku korzystania z planów.

### <a name="node-example"></a>Przykład węzeł

Funkcje kwerend są wyświetlane przez [usługę węzeł SDK] [ lnk-hub-sdks] w obiekt **rejestru** .
Oto przykład prostej kwerendy:

        var query = registry.createQuery('SELECT * FROM devices', 100);
        var onResults = function(err, results) {
            if (err) {
                console.error('Failed to fetch the results: ' + err.message);
            } else {
                // Do something with the results
                results.forEach(function(twin) {
                    console.log(twin.deviceId);
                });

                if (query.hasMoreResults) {
                    query.nextAsTwin(onResults);
                }
            }
        };
        query.nextAsTwin(onResults);

Należy zauważyć, jak obiektu **kwerendy** zostanie uruchomiony przy użyciu rozmiaru strony (maksymalnie 1000), a następnie wiele stron mogą być pobierane przez wywołanie metody **nextAsTwin** wiele razy.
Należy pamiętać, że obiekt kwerendy udostępnia wiele **następnej\***, w zależności od opcji deserializacji wymagane przez kwerendę, to znaczy sobie podwójny lub zadania obiektów lub zwykły Json może być używany w przypadku korzystania z planów.

### <a name="limitations"></a>Ograniczenia

Obecnie prognozy są obsługiwane tylko w przypadku korzystania z agregacji, to znaczy niebędące agregacją kwerendy można używać tylko `SELECT *`. Ponadto agregacji są obsługiwane tylko w połączeniu z grupowania.

## <a name="getting-started-with-jobs-queries"></a>Wprowadzenie do kwerend zadania

[Zadania] [ lnk-jobs] umożliwia wykonywanie operacji na zestawach urządzeń. Każdy podwójnego urządzenia zawiera informacje o zadań, w których jest częścią w kolekcji o nazwie **zadania**.
Logicznie,

        {                                                                      
            "deviceId": "myDeviceId",                                            
            "etag": "AAAAAAAAAAc=",                                              
            "tags": {                                                            
                ...                                                              
            },                                                                   
            "properties": {                                                      
                ...                                                                 
            },
            "jobs": [
                { 
                    "deviceId": "myDeviceId",
                    "jobId": "myJobId",    
                    "jobType": "scheduleTwinUpdate",            
                    "status": "completed",                    
                    "startTimeUtc": "2016-09-29T18:18:52.7418462",
                    "endTimeUtc": "2016-09-29T18:20:52.7418462",
                    "createdDateTimeUtc": "2016-09-29T18:18:56.7787107Z",
                    "lastUpdatedDateTimeUtc": "2016-09-29T18:18:56.8894408Z",
                    "outcome": {
                        "deviceMethodResponse": null   
                    }                                         
                },
                ...
            ]                                                             
        }

Obecnie ta kolekcja jest queriable jako **devices.jobs** w języku kwerend IoT Centrum.

Na przykład aby zobaczyć wszystkie zadania (ostatnich i według harmonogramu), które mają wpływ na jedno urządzenie, można użyć następującej kwerendy:

        SELECT * FROM devices.jobs
        WHERE devices.jobs.deviceId = 'myDeviceId'

Należy pamiętać o tym, jak to zapytanie zawiera stan specyficzne dla urządzenia (i ewentualnie odpowiedzi metoda bezpośrednia) każdego zadania, zwracany.
Użytkownik może również filtrować przy użyciu dowolnego warunków logicznych na właściwości wszystkich obiektów w kolekcji **devices.jobs** .
Na przykład poniższa kwerenda:

        SELECT * FROM devices.jobs
        WHERE devices.jobs.deviceId = 'myDeviceId'
            AND devices.jobs.jobType = 'scheduleTwinUpdate'
            AND devices.jobs.status = 'completed'
            AND devices.jobs.createdTimeUtc > '2016-09-01'

pobiera wszystkie podwójnego ukończonego zadania aktualizacji dla urządzenia **myDeviceId** utworzonych po września 2016.

Użytkownik może również pobrać wyniki na urządzenie pojedynczego zadania.

        SELECT * FROM devices.jobs
        WHERE devices.jobs.jobId = 'myJobId'

### <a name="limitations"></a>Ograniczenia
Obecnie nie obsługują kwerend na **devices.jobs** :

* Prognozy, to znaczy tylko `SELECT *` jest możliwe;
* Warunki, które odwołują się do podwójnego urządzenia oprócz właściwości zadania, jak pokazano powyżej;
* Peforming agregacji, np. licznik, średnia, Grupuj według.

## <a name="basics-of-an-iot-hub-query"></a>Podstawowe informacje o Centrum IoT kwerendy

Co IoT Centrum zapytanie składa się z zaznacz i z klauzul i opcjonalnie WHERE i GROUP BY klauzule. Każdej kwerendy jest uruchamiany w zbiorze dokumentów JSON, np. twins urządzenia. Klauzula FROM wskazuje kolekcji dokumentów należy powtórzyć na (**urządzeń** lub **devices.jobs**). Następnie jest stosowany filtr w klauzuli WHERE. W przypadku agregacji, wyniki tego kroku są zgrupowane jako określone w klauzuli GROUP BY, a dla każdej grupy, jest generowany wiersz wyszczególnione w klauzuli SELECT.

        SELECT <select_list>
        FROM <from_specification>
        [WHERE <filter_condition>]
        [GROUP BY <group_specification>]

## <a name="from-clause"></a>Klauzula FROM

Klauzuli **FROM < from_specification >** może przyjmować tylko dwie wartości: **od urządzenia**do kwerendy urządzenia twins lub **od devices.jobs**do szczegółów na urządzenie zadania kwerendy.

## <a name="where-clause"></a>Klauzula WHERE

**Miejsce, w którym < filter_condition >** klauzula jest opcjonalna. Określa warunki, które dokumentów JSON w zbiorze FROM musi spełniać w celu zapewnienia częścią wyniku. Dowolny dokument JSON muszą być określone warunki na wartość "true" mają zostać uwzględnione w wyniku.

Dozwolone warunki opisane w sekcji [wyrażeń i warunków][lnk-query-expressions].

## <a name="select-clause"></a>Klauzula SELECT

Klauzula SELECT (**Wybierz < select_list >**) jest wymagana i umożliwia określenie wartości, jakie będą pobierane z kwerendy. Określa wartości JSON użyto w celu wyświetlenia nowych obiektów JSON dla każdego elementu filtrowanych (i opcjonalnie zgrupowanych) podzestaw zbioru od faza przewidywania generuje nowy obiekt JSON, wykonane z wartości określonych w klauzuli SELECT.

Jest to gramatyki w klauzuli SELECT:

        SELECT [TOP <max number>] <projection list>

        <projection_list> ::=
            '*'
            | <projection_element> AS alias [, <projection_element> AS alias]+

        <projection_element> :==
            attribute_name
            | <projection_element> '.' attribute_name
            | <aggregate>

        <aggregate> :==
            count(<projection_element>) | count()
            | avg(<projection_element>) | avg()
            | sum(<projection_element>) | sum()
            | min(<projection_element>) | min()
            | max(<projection_element>) | max()

gdzie **attribute_name** odwołuje się do dowolnej właściwości dokumentu JSON w zbiorze od. Przykłady klauzule SELECT można znaleźć w [Wprowadzenie do kwerend podwójnego] [ lnk-query-getstarted] sekcji.

Obecnie zaznaczenia klauzule innym niż **Wybierz \* ** są obsługiwane tylko w kwerendach agregujących na twins.

## <a name="group-by-clause"></a>Klauzula GROUP BY

Klauzula **GROUP BY < group_specification >** jest krok opcjonalny, który jest mogą być wykonywane po określony w klauzuli WHERE i przed rzut określonej w polu Wybierz filtr. Dokumenty oparte na wartość atrybutu go grup. Te grupy są używane do generowania zagregowane wartości określonych w klauzuli SELECT.

Przykład kwerendy przy użyciu GROUP BY jest:

        SELECT properties.reported.telemetryConfig.status AS status,
            COUNT() AS numberOfDevices
        FROM devices
        GROUP BY properties.reported.telemetryConfig.status

Posiadanie składnia GROUP BY jest następująca:

        GROUP BY <group_by_element>
        <group_by_element> :==
            attribute_name
            | < group_by_element > '.' attribute_name

gdzie **attribute_name** odwołuje się do dowolnej właściwości dokumentu JSON w zbiorze od. 

Obecnie klauzuli GROUP BY jest obsługiwana tylko podczas badania twins.

## <a name="expressions-and-conditions"></a>Wyrażeń i warunków

Na wysokim poziomie, *wyrażenie*:

* Wynikiem wystąpienia typu JSON (to znaczy wartość logiczna, liczba, ciąg, tablica lub obiektu), a
* Jest określona przez manipulowanie dane pochodzące z urządzenia JSON dokumentu i stałych przy użyciu wbudowanych operatorów i funkcji.

*Warunki* są wyrażenia, które jest wartością logiczną, to znaczy dowolnego stałą inny niż logiczna **PRAWDA** jest traktowana jako **wartość false** (z uwzględnieniem **null**, **Nieokreślone**dowolne wystąpienie obiektu lub w tablicy, dowolny ciąg i wyraźnie logiczna **FAŁSZ**).

Składnia wyrażeń jest następująca:

        <expression> ::=
            <constant> |
            attribute_name |
            unary_operator <expression> |
            <expression> binary_operator <expression> |
            <create_array_expression> |
            '(' <expression> ')'

        <constant> ::=
            <undefined_constant>
            | <null_constant> 
            | <number_constant> 
            | <string_constant> 
            | <array_constant> 

        <undefined_constant> ::= undefined
        <null_constant> ::= null
        <number_constant> ::= decimal_literal | hexadecimal_literal
        <string_constant> ::= string_literal
        <array_constant> ::= '[' <constant> [, <constant>]+ ']'

w przypadku gdy:

| Symbol | Definicja |
| -------------- | -----------------|
| attribute_name | Dowolnej właściwości dokumentu JSON w zbiorze od. |
| unary_operator | Dowolny operator jednoargumentowy zgodnie z sekcji operatorów. |
| binary_operator | Dowolny operator binarny zgodnie z sekcji operatorów. |
| decimal_literal | Ruchomości, wyrażony w notacji dziesiętne. |
| hexadecimal_literal | Liczba wyrażona w ciągu "0 x" następuje ciąg cyfr liczbę szesnastkową. |
| string_literaL | Literałów ciągów znaków są reprezentowane przez sekwencji zero lub więcej znaków Unicode lub sekwencji escape ciągów znaków Unicode. Ciąg literałów są ujęte w pojedynczy cudzysłów (apostrof: ") lub podwójny cudzysłów (cudzysłów:"). Dozwolone anuluje: `\'`, `\"`, `\\`, `\uXXXX` dla znaków Unicode zdefiniowanych przez 4 cyfry szesnastkowej. |

### <a name="operators"></a>Operatory

Obsługiwane są następujące operatory:

| Rodzina | Operatory |
| -------------- | -----------------|
| Średnia | +,-,*,/,% |
| Logicznych. | I, LUB NIE |
| Porównanie |  =,! =, <>;, <, > =, <> |

## <a name="next-steps"></a>Następne kroki

Dowiedz się, jak wykonywać kwerendy w aplikacji przy użyciu [SDK Centrum IoT][lnk-hub-sdks].

[lnk-query-where]: iot-hub-devguide-query-language.md#where-clause
[lnk-query-expressions]: iot-hub-devguide-query-language.md#expressions-and-conditions
[lnk-query-getstarted]: iot-hub-devguide-query-language.md#getting-started-with-twin-queries

[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-jobs]: iot-hub-devguide-jobs.md
[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md