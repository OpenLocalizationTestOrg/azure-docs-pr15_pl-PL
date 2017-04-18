<properties
    pageTitle="Przykłady typowych upodobania do analizy strumieniu kwerend | Microsoft Azure"
    description="Typowe wzorce zapytania analizy strumieniu Azure "
    keywords="Przykłady kwerend"
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
    ms.workload="big-data"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>


# <a name="query-examples-for-common-stream-analytics-usage-patterns"></a>Przykłady typowych upodobania analizy strumieniu kwerendy

## <a name="introduction"></a>Wprowadzenie

Zapytania w analizy strumieniu Azure wyrażone są w języku SQL przypominających kwerendy opisano w podręczniku [Dokumentacja języka kwerend analizy strumieniu](https://msdn.microsoft.com/library/azure/dn834998.aspx) .  W tym artykule omówiono rozwiązania kilka typowych wzorców kwerendy według rzeczywistych scenariuszy.  Jest w toku i nadal będą aktualizowane na podstawie nowych wzorców na bieżąco.

## <a name="query-example-data-type-conversions"></a>Przykład zapytania: konwersje typów danych
**Opis**: Definiowanie typów właściwości w strumieniu wprowadzania danych.
Na przykład samochodu grubość już wkrótce w strumieniu wprowadzania jako ciągi i musi zostać przekonwertowane na INT przeprowadzić SUMOWAĆ go.

**Danych wejściowych**:

| Wprowadź | Czas | Weight (waga) |
| --- | --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z | "1000" |
| Honda | 2015-01-01T00:00:02.0000000Z | "2000" |

**Wynik**:

| Wprowadź | Weight (waga) |
| --- | --- |
| Honda | 3000 |

**Rozwiązanie**:

    SELECT
        Make,
        SUM(CAST(Weight AS BIGINT)) AS Weight
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)

**Wyjaśnienie**: Użyj instrukcji CAST w polu Grubość, aby określić jego typ (zobacz listę urzędów certyfikacji obsługiwane typy danych [poniżej](https://msdn.microsoft.com/library/azure/dn835065.aspx)).


## <a name="query-example-using-likenot-like-to-do-pattern-matching"></a>Przykład zapytania: za pomocą podobne i nie chcesz dopasowanie do wzorca
**Opis**: Sprawdź, czy wartość pola dla zdarzenia zgodny niektórych wzorzec, na przykład zwrotu płyty licencji zaczynające A i kończy się 9

**Danych wejściowych**:

| Wprowadź | LicensePlate | Czas |
| --- | --- | --- |
| Honda | ABC 123 | 2015-01-01T00:00:01.0000000Z |
| Toyota | AAA 999 | 2015-01-01T00:00:02.0000000Z |
| Nissan | ABC 369 | 2015-01-01T00:00:03.0000000Z |

**Wynik**:

| Wprowadź | LicensePlate | Czas |
| --- | --- | --- |
| Toyota | AAA 999 | 2015-01-01T00:00:02.0000000Z |
| Nissan | ABC 369 | 2015-01-01T00:00:03.0000000Z |

**Rozwiązanie**:

    SELECT
        *
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LicensePlate LIKE 'A%9'

**Wyjaśnienie**: użyj LIKE instrukcji, aby sprawdzić, czy wartość pola LicensePlate zaczyna się od odpowiedzi, a następnie zawiera dowolny ciąg znaków zero lub więcej i kończy się 9. 

## <a name="query-example-specify-logic-for-different-casesvalues-case-statements"></a>Przykład zapytania: określanie logiki dla różnych przypadkach wartości (instrukcje wielkości liter)
**Opis**: Podaj różnych obliczeń dla pola na podstawie niektórych kryteriów.
Na przykład Podaj opis ciąg ile samochody przekazywane samej marki przy użyciu specjalnych przypadku 1.

**Danych wejściowych**:

| Wprowadź | Czas |
| --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z |
| Toyota | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:03.0000000Z |

**Wynik**:

| CarsPassed | Czas |
| --- | --- | --- |
| 1 Honda | 2015-01-01T00:00:10.0000000Z |
| 2 Toyotas | 2015-01-01T00:00:10.0000000Z |

**Rozwiązanie**:

    SELECT
        CASE
            WHEN COUNT(*) = 1 THEN CONCAT('1 ', Make)
            ELSE CONCAT(CAST(COUNT(*) AS NVARCHAR(MAX)), ' ', Make, 's')
        END AS CarsPassed,
        System.TimeStamp AS Time
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)

**Wyjaśnienie**: Klauzula litery pozwala na udostępnianie innym obliczenia na podstawie niektórych kryteriów (w naszym przypadku liczba samochodów w oknie agregacji).

## <a name="query-example-send-data-to-multiple-outputs"></a>Przykład zapytania: wysyłanie danych do wielu wyników
**Opis**: Wyślij dane do wielu elementów docelowych dane wyjściowe z pojedynczego zadania.
Na przykład analizowania danych opartych na progu alertu i archiwizowanie wszystkie zdarzenia magazynem obiektów blob

**Danych wejściowych**:

| Wprowadź | Czas |
| --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z |
| Honda | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:01.0000000Z |
| Toyota | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:03.0000000Z |

**Output1**:

| Wprowadź | Czas |
| --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z |
| Honda | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:01.0000000Z |
| Toyota | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:03.0000000Z |

**Output2**:

| Wprowadź | Czas | Liczba |
| --- | --- | --- |
| Toyota | 2015-01-01T00:00:10.0000000Z | 3 |

**Rozwiązanie**:

    SELECT
        *
    INTO
        ArchiveOutput
    FROM
        Input TIMESTAMP BY Time

    SELECT
        Make,
        System.TimeStamp AS Time,
        COUNT(*) AS [Count]
    INTO
        AlertOutput
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)
    HAVING
        [Count] >= 3

**Wyjaśnienie**: klauzuli INTO informuje analizy strumieniu której wyjściowe tej instrukcji podczas zapisywania danych.
Pierwszy kwerenda jest przekazującą danych, które Otrzymaliśmy do wyników możemy o nazwie ArchiveOutput.
Kilka prostych agregacji czy drugiego zapytania i filtrowanie i wysyła wyniki z systemem alertów podrzędne.
*Uwaga*: można również ponownie użyć wyniki wyrażeń CTE (to znaczy z instrukcje) w wielu sprawozdaniach wyjściowy — ma zwiększenia korzyści otwierania mniej czytelnikom zapisanie się do źródła danych wejściowych.
Na przykład 

    WITH AllRedCars AS (
        SELECT
            *
        FROM
            Input TIMESTAMP BY Time
        WHERE
            Color = 'red'
    )
    SELECT * INTO HondaOutput FROM AllRedCars WHERE Make = 'Honda'
    SELECT * INTO ToyotaOutput FROM AllRedCars WHERE Make = 'Toyota'

## <a name="query-example-counting-unique-values"></a>Przykład zapytania: zliczanie wartości unikatowych
**Opis**: zliczania unikatowych wartości pól wyświetlanych w strumieniu poza przedziałem czasu.
Na przykład ile unikatowych tworzącej samochodów przeszła stoiska płatne w 2 drugie okno?

**Danych wejściowych**:

| Wprowadź | Czas |
| --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z |
| Honda | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:01.0000000Z |
| Toyota | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:03.0000000Z |

**Wynik:**

| Liczba | Czas |
| --- | --- |
| 2 | 2015-01-01T00:00:02.000Z |
| 1 | 2015-01-01T00:00:04.000Z |

**Rozwiązanie:**

    WITH Makes AS (
        SELECT
            Make,
            COUNT(*) AS CountMake
        FROM
            Input TIMESTAMP BY Time
        GROUP BY
              Make,
              TumblingWindow(second, 2)
    )
    SELECT
        COUNT(*) AS Count,
        System.TimeStamp AS Time
    FROM
        Makes
    GROUP BY
        TumblingWindow(second, 1)


**Wyjaśnienie:** Czynności początkowej agregacji uzyskanie unikatowych powoduje, że z ich liczba nad okno.
Firma Microsoft wykonaj agregacji ile powoduje, że masz firma Microsoft podane unikatowe wartości w oknie osób samej sygnatura czasowa, a następnie drugie okno agregacji powinien być minimalnymi nie agregacji 2 systemu windows z pierwszym krokiem.

## <a name="query-example-determine-if-a-value-has-changed"></a>Przykład zapytania: Określanie, czy wartość został zmieniony#
**Opis**: Przeglądanie poprzedniej wartości, aby ustalić, czy jest inny niż bieżącą wartość jest na przykład samochodu poprzedniego podróży płatne takie same wprowadź bieżącą samochodu?

**Danych wejściowych**:

| Wprowadź | Czas |
| --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z |
| Toyota | 2015-01-01T00:00:02.0000000Z |

**Wynik**:

| Wprowadź | Czas |
| --- | --- |
| Toyota | 2015-01-01T00:00:02.0000000Z |

**Rozwiązanie**:

    SELECT
        Make,
        Time
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LAG(Make, 1) OVER (LIMIT DURATION(minute, 1)) <> Make

**Wyjaśnienie**: Użyj ZWŁOKI do wglądu do wejścia strumienia jedno zdarzenie ponownie i Pobierz tworzącej wartość. Następnie, porównując go przed rozpoczęciem na bieżącego zdarzenia wyjściowy zdarzenia, jeśli są różne.

## <a name="query-example-find-first-event-in-a-window"></a>Przykład zapytania: pierwszego zdarzenia Znajdź w oknie
**Opis**: Znajdź pierwszy samochodu w każdej 10 minut?

**Danych wejściowych**:

| LicensePlate | Wprowadź | Czas |
| --- | --- | --- |
| DXE 5291 | Honda | 2015-07-27T00:00:05.0000000Z |
| YZK 5704 | Ford | 2015-07-27T00:02:17.0000000Z |
| RMV 8282 | Honda | 2015-07-27T00:05:01.0000000Z |
| YHN 6970 | Toyota | 2015-07-27T00:06:00.0000000Z |
| VFE 1616 | Toyota | 2015-07-27T00:09:31.0000000Z |
| QYF 9358 | Honda | 2015-07-27T00:12:02.0000000Z |
| MDR 6128 | BMW | 2015-07-27T00:13:45.0000000Z |

**Wynik**:

| LicensePlate | Wprowadź | Czas |
| --- | --- | --- |
| DXE 5291 | Honda | 2015-07-27T00:00:05.0000000Z |
| QYF 9358 | Honda | 2015-07-27T00:12:02.0000000Z |

**Rozwiązanie**:

    SELECT 
        LicensePlate,
        Make,
        Time
    FROM 
        Input TIMESTAMP BY Time
    WHERE 
        IsFirst(minute, 10) = 1

Teraz wprowadzić zmiany problem i Znajdź pierwszy samochodu określonego w każdej 10 minut.

| LicensePlate | Wprowadź | Czas |
| --- | --- | --- |
| DXE 5291 | Honda | 2015-07-27T00:00:05.0000000Z |
| YZK 5704 | Ford | 2015-07-27T00:02:17.0000000Z |
| YHN 6970 | Toyota | 2015-07-27T00:06:00.0000000Z |
| QYF 9358 | Honda | 2015-07-27T00:12:02.0000000Z |
| MDR 6128 | BMW | 2015-07-27T00:13:45.0000000Z |

**Rozwiązanie**:

    SELECT 
        LicensePlate,
        Make,
        Time
    FROM 
        Input TIMESTAMP BY Time
    WHERE 
        IsFirst(minute, 10) OVER (PARTITION BY Make) = 1

## <a name="query-example-find-last-event-in-a-window"></a>Przykład zapytania: Znajdź ostatnie zdarzenie w oknie
**Opis**: znajdowanie ostatniego samochodu w każdej 10 minut.

**Danych wejściowych**:

| LicensePlate | Wprowadź | Czas |
| --- | --- | --- |
| DXE 5291 | Honda | 2015-07-27T00:00:05.0000000Z |
| YZK 5704 | Ford | 2015-07-27T00:02:17.0000000Z |
| RMV 8282 | Honda | 2015-07-27T00:05:01.0000000Z |
| YHN 6970 | Toyota | 2015-07-27T00:06:00.0000000Z |
| VFE 1616 | Toyota | 2015-07-27T00:09:31.0000000Z |
| QYF 9358 | Honda | 2015-07-27T00:12:02.0000000Z |
| MDR 6128 | BMW | 2015-07-27T00:13:45.0000000Z |

**Wynik**:

| LicensePlate | Wprowadź | Czas |
| --- | --- | --- |
| VFE 1616 | Toyota | 2015-07-27T00:09:31.0000000Z |
| MDR 6128 | BMW | 2015-07-27T00:13:45.0000000Z |

**Rozwiązanie**:

    WITH LastInWindow AS
    (
        SELECT 
            MAX(Time) AS LastEventTime
        FROM 
            Input TIMESTAMP BY Time
        GROUP BY 
            TumblingWindow(minute, 10)
    )
    SELECT 
        Input.LicensePlate,
        Input.Make,
        Input.Time
    FROM
        Input TIMESTAMP BY Time 
        INNER JOIN LastInWindow
        ON DATEDIFF(minute, Input, LastInWindow) BETWEEN 0 AND 10
        AND Input.Time = LastInWindow.LastEventTime

**Wyjaśnienie**: obejmuje dwa kroki w kwerendzie — pierwszego znajduje najnowszą sygnatury czasowej w systemie windows 10 minut. Drugim krokiem łączy wyniki kwerendy pierwszy ze strumieniem oryginalnego, aby znaleźć zdarzenia pasujące ostatniej sygnatury czasowe w każdym oknie. 

## <a name="query-example-detect-the-absence-of-events"></a>Przykład zapytania: wykrywanie braku zdarzenia
**Opis**: Sprawdź, czy strumień ma żadnej wartości spełniających określone kryteria.
Na przykład 2 samochody następujących po sobie z tym samym wybierz zostały wprowadzone w podróży płatne w ciągu 90 sekund?

**Danych wejściowych**:

| Wprowadź | LicensePlate | Czas |
| --- | --- | --- |
| Honda | ABC 123 | 2015-01-01T00:00:01.0000000Z |
| Honda | AAA 999 | 2015-01-01T00:00:02.0000000Z |
| Toyota | 987 ROZDZIELCZOŚCI | 2015-01-01T00:00:03.0000000Z |
| Honda | GHI 345 | 2015-01-01T00:00:04.0000000Z |

**Wynik**:

| Wprowadź | Czas | CurrentCarLicensePlate | FirstCarLicensePlate | FirstCarTime |
| --- | --- | --- | --- | --- |
| Honda | 2015-01-01T00:00:02.0000000Z | AAA 999 | ABC 123 | 2015-01-01T00:00:01.0000000Z |

**Rozwiązanie**:

    SELECT
        Make,
        Time,
        LicensePlate AS CurrentCarLicensePlate,
        LAG(LicensePlate, 1) OVER (LIMIT DURATION(second, 90)) AS FirstCarLicensePlate,
        LAG(Time, 1) OVER (LIMIT DURATION(second, 90)) AS FirstCarTime
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LAG(Make, 1) OVER (LIMIT DURATION(second, 90)) = Make

**Wyjaśnienie**: Użyj ZWŁOKI do wglądu do wejścia strumienia jedno zdarzenie ponownie i Pobierz tworzącej wartość. Następnie, porównując go przed rozpoczęciem na bieżącego zdarzenia wyjściowy zdarzenia, jeśli są takie same i użyj ZWŁOKI, aby uzyskać dane dotyczące poprzednich samochodu.

## <a name="query-example-detect-duration-between-events"></a>Przykład zapytania: wykrywanie czas trwania między zdarzeniami
**Opis**: znajdowanie czas trwania danego zdarzenia. Na przykład podane web clickstream określić czas poświęcony funkcji.

**Danych wejściowych**:  
  
| Użytkownika | Funkcja | Zdarzenia | Czas |
| --- | --- | --- | --- |
| user@location.com | RightMenu | Rozpoczynanie | 2015-01-01T00:00:01.0000000Z |
| user@location.com | RightMenu | Koniec | 2015-01-01T00:00:08.0000000Z |
  
**Wynik**:  
  
| Użytkownika | Funkcja | Czas trwania |
| --- | --- | --- |
| user@location.com | RightMenu | 7 |
  

**Rozwiązanie**

````
    SELECT
        [user], feature, DATEDIFF(second, LAST(Time) OVER (PARTITION BY [user], feature LIMIT DURATION(hour, 1) WHEN Event = 'start'), Time) as duration
    FROM input TIMESTAMP BY Time
    WHERE
        Event = 'end'
````

**Wyjaśnienie**: Użyj funkcji OSTATNIEJ do pobierania ostatnią wartość czasu, gdy typ zdarzenia był "Start". Należy zauważyć, że OSTATNIEJ funkcja PARTITION BY [użytkownik] oznaczającą, że wynik oblicza dla poszczególnych unikatowych użytkowników.  Kwerenda zawiera 1 godzina maksymalnej wartości progowej dla różnicę czasu między zdarzeniami "Start" i "Zatrzymaj", ale można skonfigurować jako wymaganego (LIMIT DURATION(hour, 1).

## <a name="query-example-detect-duration-of-a-condition"></a>Przykład zapytania: wykrywanie czas trwania warunku
**Opis**: Sprawdzanie, ile czasu dla wystąpił warunku.
Załóżmy, że błąd, które spowodowały wszystkie samochody ciężar niepoprawne (powyżej 20 000 jednostkach funt) — chcemy obliczyć czas trwania błędu.

**Danych wejściowych**:

| Wprowadź | Czas | Weight (waga) |
| --- | --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z | 2000 |
| Toyota | 2015-01-01T00:00:02.0000000Z | 25000 |
| Honda | 2015-01-01T00:00:03.0000000Z | 26000 |
| Toyota | 2015-01-01T00:00:04.0000000Z | 25000 |
| Honda | 2015-01-01T00:00:05.0000000Z | 26000 |
| Toyota | 2015-01-01T00:00:06.0000000Z | 25000 |
| Honda | 2015-01-01T00:00:07.0000000Z | 26000 |
| Toyota | 2015-01-01T00:00:08.0000000Z | 2000 |

**Wynik**:

| StartFault | EndFault |
| --- | --- |
| 2015-01-01T00:00:02.000Z | 2015-01-01T00:00:07.000Z |

**Rozwiązanie**:

````
    WITH SelectPreviousEvent AS
    (
    SELECT
    *,
        LAG([time]) OVER (LIMIT DURATION(hour, 24)) as previousTime,
        LAG([weight]) OVER (LIMIT DURATION(hour, 24)) as previousWeight
    FROM input TIMESTAMP BY [time]
    )

    SELECT 
        LAG(time) OVER (LIMIT DURATION(hour, 24) WHEN previousWeight < 20000 ) [StartFault],
        previousTime [EndFault]
    FROM SelectPreviousEvent
    WHERE
        [weight] < 20000
        AND previousWeight > 20000
````

**Wyjaśnienie**: używanie ZWŁOKI do wyświetlania strumienia wejściowego 24 godzin i poszukaj wystąpienia, w którym StartFault i StopFault są objęte waga < 20000.

## <a name="query-example-fill-missing-values"></a>Przykład zapytania: wypełnianie brakujące wartości
**Opis**: dla strumienia zdarzenia, które nie mają wartości warzywa strumienia zdarzeń z regularnych odstępach.
Na przykład wygenerować zdarzenie co 5 sekund, które zgłasza najbardziej ostatnio widoczna punktu danych.

**Danych wejściowych**:

| t | wartość |
|--------------------------|-------|
| "2014-01-01T06:01:00" | 1 |
| "2014-01-01T06:01:05" | 2 |
| "2014-01-01T06:01:10" | 3 |
| "2014-01-01T06:01:15" | 4 |
| "2014-01-01T06:01:30" | 5 |
| "2014-01-01T06:01:35" | 6 |

**Wyjściowy (10 pierwszych wierszy)**:

| windowend | lastevent.t | lastevent.Value |
|--------------------------|--------------------------|--------|
| 2014-01-01T14:01:00.000Z | 2014-01-01T14:01:00.000Z | 1 |
| 2014-01-01T14:01:05.000Z | 2014-01-01T14:01:05.000Z | 2 |
| 2014-01-01T14:01:10.000Z | 2014-01-01T14:01:10.000Z | 3 |
| 2014-01-01T14:01:15.000Z | 2014-01-01T14:01:15.000Z | 4 |
| 2014-01-01T14:01:20.000Z | 2014-01-01T14:01:15.000Z | 4 |
| 2014-01-01T14:01:25.000Z | 2014-01-01T14:01:15.000Z | 4 |
| 2014-01-01T14:01:30.000Z | 2014-01-01T14:01:30.000Z | 5 |
| 2014-01-01T14:01:35.000Z | 2014-01-01T14:01:35.000Z | 6 |
| 2014-01-01T14:01:40.000Z | 2014-01-01T14:01:35.000Z | 6 |
| 2014-01-01T14:01:45.000Z | 2014-01-01T14:01:35.000Z | 6 |

    
**Rozwiązanie**:

    SELECT
        System.Timestamp AS windowEnd,
        TopOne() OVER (ORDER BY t DESC) AS lastEvent
    FROM
        input TIMESTAMP BY t
    GROUP BY HOPPINGWINDOW(second, 300, 5)


**Wyjaśnienie**: Ta kwerenda wygeneruje wydarzeń każdego drugiego 5 i będzie wyjściowe ostatniego zdarzenia, które otrzymano przed. [Skaczące okno] Czas trwania (https://msdn.microsoft.com/library/dn835041.aspx "Skaczące okno - Azure analizy strumieniu") Określa, jak daleko wstecz kwerenda będzie wyglądać Znajdowanie najnowszych zdarzeń (300 sekund w tym przykładzie).


## <a name="get-help"></a>Uzyskiwanie pomocy
Aby uzyskać dodatkową pomoc spróbuj nasze [forum analizy strumieniu Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Następne kroki

- [Wprowadzenie do analizy strumieniu Azure](stream-analytics-introduction.md)
- [Wprowadzenie do korzystania z analizy strumieniu Azure](stream-analytics-get-started.md)
- [Skalowanie Azure analizy strumieniu zadania](stream-analytics-scale-jobs.md)
- [Dokumentacja języka kwerend analizy strumieniu Azure](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Strumień Azure analizy zarządzania pozostałych interfejsu API odwołania](https://msdn.microsoft.com/library/azure/dn835031.aspx)
 
