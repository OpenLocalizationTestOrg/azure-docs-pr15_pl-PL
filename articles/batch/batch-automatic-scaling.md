<properties
    pageTitle="Automatycznie skali obliczyć węzły w puli partii Azure | Microsoft Azure"
    description="Włącz automatyczne skalowanie w puli chmurze, aby dynamicznie dostosować liczby węzłów obliczeń w puli."
    services="batch"
    documentationCenter=""
    authors="mmacy"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="batch"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="multiple"
    ms.date="10/14/2016"
    ms.author="marsma"/>

# <a name="automatically-scale-compute-nodes-in-an-azure-batch-pool"></a>Automatyczne skalowanie węzły obliczeń w puli wsadowe Azure

Z automatyczne skalowanie usługi Azure partii może dynamiczne dodawanie lub usuwanie węzły obliczeń w puli na podstawie parametrów zdefiniowanych przez użytkownika. Automatycznie dostosowując wielkość potencjalnie można zapisać zaoszczędzić czas i pieniądze energii obliczeń używanych przez aplikację — Dodawanie węzły wzrostem potrzeb zadań z zadania i usuń je, podczas ich zmniejszyć.

Możesz włączyć automatyczne skalowanie puli węzłów obliczeń, kojarząc go z nim *autoscale formułę* , która zdefiniujesz, takich jak z [PoolOperations.EnableAutoScale] [ net_enableautoscale] metody w bibliotece [.NET partię](batch-dotnet-get-started.md) . Usługa wsadowe następnie używa następującej formuły do określenia liczby węzłów obliczeń, które są potrzebne do wykonania z pracą. Partia odpowiada metryki usługi próbki danych, które są okresowo pobierane i dopasowuje liczby węzłów obliczeń w puli interwałem można konfigurować oparte na formule.

Aby umożliwić automatyczne skalowanie po utworzeniu puli lub na istniejącej puli. Możesz również zmienić istniejącą formułę w puli, która jest jako "skalowanie automatyczne" włączona. Partia umożliwia Szacowanie formuły przed przydzielania ich do puli, a także monitorować stan automatyczne skalowanie zostanie uruchomiony.

## <a name="automatic-scaling-formulas"></a>Automatyczne skalowanie formuł

Automatyczne skalowanie formuła jest wartość ciągu zdefiniowanego zawiera jedną lub więcej instrukcji, która jest przypisana do puli [autoScaleFormula] [ rest_autoscaleformula] elementu (RESZTA partii) lub [CloudPool.AutoScaleFormula] [ net_cloudpool_autoscaleformula] właściwości (partii .NET). Jeśli przypisany do puli, usługa wsadowe używa formuły Określanie docelowej liczby węzłów obliczeń w puli dla następnego przedziału przetwarzania (więcej informacji na temat interwały później). Ciąg formuły nie może przekraczać 8 KB rozmiaru, może zawierać maksymalnie 100 instrukcji, które są oddzielone średnikami, a może zawierać podziały wiersza i komentarze.

Możesz myśleć automatyczne skalowanie formuł, jak przy użyciu autoscale partii "język". Instrukcje formuły są bezpłatne utworzonych wyrażeń, zawierające zarówno zmienne zdefiniowane przez usługę (zmienne zdefiniowane przez usługę partii), jak i zmienne zdefiniowane przez użytkownika (zmiennych zdefiniowanych przez użytkownika). Umożliwia im wykonywanie różnych operacji na tych wartości przy użyciu wbudowanych typów, operatory i funkcje. Na przykład instrukcją może zająć następującą postać:

```
$myNewVariable = function($ServiceDefinedVariable, $myCustomVariable);
```

Formuły zawierają zazwyczaj wielu instrukcji, które wykonują operacje na wartości, które są uzyskiwane w instrukcjach poprzedniego. Na przykład, najpierw możemy uzyskać wartość dla `variable1`, następnie przekazać je do funkcji wypełnianie `variable2`:

```
$variable1 = function1($ServiceDefinedVariable);
$variable2 = function2($OtherServiceDefinedVariable, $variable1);
```

W tych instrukcjach w formule celem jest obciążony liczbę węzłów obliczeń skalowania puli do — **docelowej** liczby **węzłów dedykowane**. Ten numer może być większy, małe lub taka sama jak bieżącej liczby węzłów w puli. Partia wynikiem formuły autoscale puli w określonych odstępach czasu ([Automatyczne skalowanie interwały](#automatic-scaling-interval) omówiony poniżej). Następnie dopasuje docelowej liczby węzłów w puli na liczbę, która określa formuły autoscale w czasie oceny.

Jako przykład szybkie ta formuła autoscale dwóch wierszy Określa, że liczby węzłów należy dostosować według liczby aktywnych zadań maksymalnie 10 węzły obliczeń:

```
$averageActiveTaskCount = avg($ActiveTasks.GetSample(TimeInterval_Minute * 15));
$TargetDedicated = min(10, $averageActiveTaskCount);
```

Kilka kolejnych sekcjach w tym artykule omówiono różne jednostki tworzące formuły autoscale, w tym zmienne, operatorów, operacje i funkcje. Można znaleźć informacje dotyczące sposobu uzyskiwania różnych obliczeń zasobów i zadaniami metryki w partii. Za pomocą tych metryki dostosować sposób inteligentny statystyka węzeł puli użytkownika na podstawie stanu użycia i zadań zasobów. Jak utworzyć formułę i włączyć automatyczne skalowanie puli przy użyciu API .NET i pozostałych partii następnie dowiesz się. Firma Microsoft będzie zakończenia z kilku formuł przykładowych.

> [AZURE.IMPORTANT] Każde konto Azure partii jest ograniczona do maksymalnie rdzenie (i w związku z tym węzły obliczeń) służącej do przetwarzania. Usługa wsadowe utworzy węzły tylko do tego limitu core. W związku z tym nie może ona osiągnąć docelowej liczby obliczeń węzłów określonego przez formułę. Zobacz [przydziałów i limity dotyczące usługi Azure wsadowe](batch-quota-limit.md) informacji na temat wyświetlania i zwiększanie przydziałami konta.

## <a name="variables"></a>Zmienne

W formułach autoscale, można używać zmiennych **określonych usług** , a także **zdefiniowane przez użytkownika** . Usługa zdefiniowana zmiennych jest wbudowany w usłudze partii — niektóre są odczytu i zapisu, a niektóre są tylko do odczytu. Zmienne zdefiniowane przez użytkownika są zmienne *należy* zdefiniować. W przypadku formuły przykład dwóch wierszy `$TargetDedicated` jest usługa zdefiniowana zmienna podczas `$averageActiveTaskCount` jest zmienną zdefiniowane przez użytkownika.

W poniższej tabeli przedstawiono zmienne zarówno odczytu i zapisu i tylko do odczytu, które są definiowane przez usługę partii.

Możesz **pobrać** i **Ustaw** wartości zmienne zdefiniowane przez usługę Zarządzanie liczby węzłów obliczeń w puli:

| Zmienne zdefiniowane przez usługę odczytu i zapisu | Opis |
| --- | --- |
| $TargetDedicated | Numer **docelowej** **dedykowane węzły obliczeń** dla puli. To liczby węzłów obliczeń, które skalowania do puli. Jest to liczba "docelowy", ponieważ istnieje możliwość puli nie do osiągnięcia docelowej liczby węzłów. Przyczyną może być zmodyfikowany docelowej liczby węzłów jest ponownie przez oceny kolejnych autoscale przed puli dotarło początkowej docelowej. Również może się zdarzyć po osiągnięciu przydziału partii konta węzeł lub core przed osiągnięciem docelowej liczby węzłów. |
| $NodeDeallocationOption | Akcja wykonywana w przypadku węzły obliczeń zostaną usunięte z puli. Możliwe wartości są następujące:<ul><li>**requeue**— natychmiast kończy zadań i umieszcza je ponownie w kolejce zadań, tak, aby były zmiany harmonogramu.<li>**Zakończenie**- natychmiast kończy zadań i usunąć je z kolejki zadań.<li>**taskcompletion**— czeka obecnie uruchomione zadania do końca, a następnie usuwa węzeł z puli.<li>**retaineddata**— czeka na wszystkich danych lokalnych zachowane zadania na węzeł, który ma zostać wyczyszczone przed usunięciem węzeł z puli.</ul> |

Możesz **uzyskać** wartości zmienne zdefiniowane przez usługę, aby wprowadzić zmiany, opartych na metryki w usłudze partii:

| Tylko do odczytu zmienne zdefiniowane przez usługę | Opis |
| --- | --- |
| $CPUPercent | Średnia procentowe użycie Procesora. |
| $WallClockSeconds | Liczba sekund zużyte. |
| $MemoryBytes | Średnia liczba megabajtów używane. |
| $DiskBytes | Średnia liczba gigabajtów używane na lokalnych dyskach. |
| $DiskReadBytes | W tym artykule liczby bajtów. |
| $DiskWriteBytes | Liczba zapisanych bajtów. |
| $DiskReadOps | Liczba wykonywanych operacji odczytu dysku. |
| $DiskWriteOps | Liczba operacji dysku zapisu. |
| $NetworkInBytes | Liczba bajtów przychodzących. |
| $NetworkOutBytes | Liczba bajtów wychodzących. |
| $SampleNodeCount | Liczba węzłów obliczeń. |
| $ActiveTasks | Liczba zadań do stanu aktywnego. |
| $RunningTasks | Numer zadania w stanie uruchomienia. |
| $PendingTasks | Suma $ActiveTasks i $RunningTasks. |
| $SucceededTasks | Liczba zadań, które zostało zakończone pomyślnie. |
| $FailedTasks | Liczba zadań, które nie powiodło się. |
| $CurrentDedicated | Bieżąca liczba dedykowane obliczyć węzły. |

> [AZURE.TIP] Zmienne tylko do odczytu, zdefiniowane przez usługę, wymienione powyżej są *obiekty* , które zapewniają różnych metod, aby uzyskać dostęp do danych skojarzonych z każdym. Aby uzyskać więcej informacji, zobacz [Uzyskiwanie przykładowych danych](#getsampledata) poniżej.

## <a name="types"></a>Typy

W formule są obsługiwane następujące **typy** .

- podwójne
- doubleVec
- doubleVecList
- ciąg
- Sygnatura czasowa — sygnatura czasowa jest złożonego strukturą, która zawiera następujące elementy:

    - rok
    - miesiąc (1 – 12)
    - dzień (1 – 31)
    - Weekday (w formacie liczby, np. 1 w poniedziałek)
    - Godzina (w formacie liczb 24-godzinny, na przykład 13 oznacza PM; 1)
    - MINUTE (od 00 do 59)
    - Second (od 00 do 59)
- parametru TimeInterval

    - TimeInterval_Zero
    - TimeInterval_100ns
    - TimeInterval_Microsecond
    - TimeInterval_Millisecond
    - TimeInterval_Second
    - TimeInterval_Minute
    - TimeInterval_Hour
    - TimeInterval_Day
    - TimeInterval_Week
    - TimeInterval_Year

## <a name="operations"></a>Operacje

Te **operacje** są dozwolone na typy, które zostały wymienione powyżej.

| Operacja                             | Obsługiwane operatory   | Typ wyniku   |
| ------------------------------------- | --------------------- | ------------- |
| podwójne *operator* podwójne              | +, -, *, /            | podwójne            |
| podwójne *operator* parametru timeinterval        | *                     | parametru TimeInterval      |
| Podwójna doubleVec *operatora*           | +, -, *, /            | doubleVec         |
| doubleVec *operator* doubleVec        | +, -, *, /            | doubleVec         |
| podwójne parametru TimeInterval *operatora*        | *, /                  | parametru TimeInterval      |
| parametru parametru TimeInterval *operator* timeinterval  | +, -                  | parametru TimeInterval      |
| Sygnatura czasowa *operator* parametru TimeInterval     | +                     | Sygnatura czasowa         |
| parametru timeinterval *operator* sygnatura czasowa     | +                     | Sygnatura czasowa         |
| Sygnatura czasowa *operator* sygnatura czasowa        | -                     | parametru TimeInterval      |
| *operator*podwójne                      | -, !                  | podwójne            |
| *operator*parametru timeinterval                | -                     | parametru TimeInterval      |
| podwójne *operator* podwójne              | < < =, ==, > =, >,! =  | podwójne            |
| ciąg *operatora* ciągu              | < < =, ==, > =, >,! =  | podwójne            |
| Sygnatura czasowa *operator* sygnatura czasowa        | < < =, ==, > =, >,! =  | podwójne            |
| parametru parametru TimeInterval *operator* timeinterval  | < < =, ==, > =, >,! =  | podwójne            |
| podwójne *operator* podwójne              | & &, & #124; & #124;      | podwójne            |

Podczas testowania dwukrotnie z trójargumentowy (`double ? statement1 : statement2`), niezerową jest **prawdziwy**, a zero jest **fałszywe**.

## <a name="functions"></a>Funkcje

Wstępnie zdefiniowane te **Funkcje** są dostępne do użycia w określeniu automatyczne skalowanie formuły.

| Funkcja                          | Zwracany typ   | Opis
| --------------------------------- | ------------- | --------- |
| AVG(doubleVecList)                | podwójne        | Zwraca średnią wartość dla wszystkich wartości w doubleVecList.
| Len(doubleVecList)                | podwójne        | Zwraca długość wektora utworzonego na podstawie doubleVecList.
| LG(Double)                        | podwójne        | Zwraca dziennik o podstawie 2 podwójnego.
| LG(doubleVecList)                 | doubleVec     | Zwraca componentwise dziennika o podstawie 2 doubleVecList. Vec(double) muszą być przekazywane jawnie parametru. W przeciwnym razie przyjmowana jest wersja lg(double) podwójnych.
| ln(Double)                        | podwójne        | Zwraca logarytm naturalny podwójnego.
| ln(doubleVecList)                 | doubleVec     | Zwraca componentwise dziennika o podstawie 2 doubleVecList. Vec(double) muszą być przekazywane jawnie parametru. W przeciwnym razie przyjmowana jest wersja lg(double) podwójnych.
| log(Double)                       | podwójne        | Zwraca dziennik podwójnego o podstawie 10.
| log(doubleVecList)                | doubleVec     | Zwraca dziennik componentwise doubleVecList o podstawie 10. Vec(double) muszą być przekazywane jawnie dla pojedynczego parametru podwójnych. W przeciwnym razie przyjmowana jest wersja log(double) podwójnych.
| MAX(doubleVecList)                | podwójne        | Zwraca maksymalną wartość w doubleVecList.
| min(doubleVecList)                | podwójne        | Zwraca minimalną wartość w doubleVecList.
| norm(doubleVecList)               | podwójne        | Zwraca dwa normę wektorowa, który został utworzony na podstawie doubleVecList.
| percentyl (v doubleVec podwójnych p) | podwójne        | Zwraca element percentyl wektora v.
| RAND()                            | podwójne        | Zwraca wartość losowe od 0.0 do 1.0.
| Range(doubleVecList)              | podwójne        | Zwraca różnicę między wartości minimalne i maksymalne doubleVecList.
| STD(doubleVecList)                | podwójne        | Zwraca odchylenie standardowe próbki wartości w doubleVecList.
| stop()                            |               | Ocena stopnie wyrażenia autoscaling.
| sum(doubleVecList)                | podwójne        | Zwraca sumę wszystkie składniki doubleVecList.
| Godzina (dateTime ciąg = "")          | Sygnatura czasowa     | Jeśli zostanie przekazany zwraca sygnatura czasowa bieżącą godzinę, jeśli są przekazywane żadne parametry lub sygnaturą czasową ciągu daty i godziny. Formaty daty i godziny obsługiwane są W3C DTF i RFC 1123.
| Val (v doubleVec i podwójne)        | podwójne        | Zwraca wartość elementu, który jest w lokalizacji i w wektorze v z indeksem początkowe zero.

Niektóre funkcje opisane w powyższej tabeli Zaakceptuj listy jako argument. Rozdzielaną średnikami listę jest dowolną kombinację *podwójne* i *doubleVec*. Na przykład:

`doubleVecList := ( (double | doubleVec)+(, (double | doubleVec) )* )?`

Wartość *doubleVecList* jest konwertowana na jednym *doubleVec* przed oceny. Na przykład jeśli `v = [1,2,3]`, następnie nawiązywania połączeń z `avg(v)` jest równoważna dzwonisz `avg(1,2,3)`. Wywoływanie `avg(v, 7)` jest równoważna dzwonisz `avg(1,2,3,7)`.

## <a name="getsampledata"></a>Uzyskiwanie przykładowych danych

Formuły Autoscale działają na danych metryki (przykłady) dostarczonego przez usługę partię. Formuła powiększa się lub zmniejsza rozmiar puli na podstawie wartości uzyskiwanych z usługi. Zmienne zdefiniowane przez usługę, opisanych powyżej są obiekty, które zapewniają różnych metod, aby uzyskać dostęp do danych, który jest skojarzony z tego obiektu. Na przykład następujące wyrażenie zawiera żądanie, aby uzyskać ostatnie pięć minut użycie Procesora:

```
$CPUPercent.GetSample(TimeInterval_Minute * 5)
```

| Metoda | Opis |
| --- | --- |
| GetSample() | `GetSample()` Metoda zwraca wektor próbek danych.<br/><br/>Próbki wynosi 30 sekund warte metryki danych. Innymi słowy próbki są uzyskiwane co 30 sekund. Ale zaznaczono poniżej, występuje opóźnienie między zbierania próbki i kiedy jest ona dostępna do formuły. Jako takie nie wszystkie próbki w danym okresie mogą być dostępne do oceny za pomocą formuły.<ul><li>`doubleVec GetSample(double count)`<br/>Określa liczbę próbki, aby uzyskać najnowsze próbkach, które zostały pobrane.<br/><br/>`GetSample(1)`Zwraca ostatnią przykładowe dostępne. Dla miar, takich jak `$CPUPercent`, jednak to nie może być używana, ponieważ nie jest możliwe wiedzieć, *Gdy* zebrane próbki. Może to być ostatnio używane lub z powodu problemów z systemu, może być znacznie starsze. Najlepiej w takich przypadkach, aby użyć przedział czasu, tak jak pokazano poniżej.<li>`doubleVec GetSample((timestamp or timeinterval) startTime [, double samplePercent])`<br/>Określa przedział czasu do zbierania danych przykładowych. Opcjonalnie można również określa procent próbki, które muszą być dostępne w ramach czasowych wymagane.<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10)`zwróci 20 próbek, jeśli wszystkie próbki dla ostatniego dziesięć minut znajdują się w sekcji Historia CPUPercent. Jeśli ostatniej chwili Historia nie jest dostępna, jednak zwracana będzie tylko 18 próbki. W tym przypadku:<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10, 95)`może się zakończyć niepowodzeniem, ponieważ dostępne są tylko 90 procent próbki.<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10, 80)`powiedzie się.<li>`doubleVec GetSample((timestamp or timeinterval) startTime, (timestamp or timeinterval) endTime [, double samplePercent])`<br/>Określa przedział czasu do zbierania danych z czas rozpoczęcia i czas zakończenia.<br/><br/>Jak wcześniej wspomniano, ma opóźnienia między zbierania próbki i kiedy jest ona dostępna do formuły. To należy uwzględnić podczas używania `GetSample` metody. Zobacz `GetSamplePercent` poniżej.|
| GetSamplePeriod() | Zwraca okres próbek zrobionych w zestawie danych historycznych próbki. |
| Count() | Zwraca całkowitą liczbę próbki w historii metryczne. |
| HistoryBeginTime() | Zwraca sygnatura czasowa najstarszych przykładowych danych dostępne metryki. |
| GetSamplePercent() |Zwraca wartość procentową próbki, które są dostępne dla danego okresu. Na przykład:<br/><br/>`doubleVec GetSamplePercent( (timestamp or timeinterval) startTime [, (timestamp or timeinterval) endTime] )`<br/><br/>Ponieważ `GetSample` metody zakończy się niepowodzeniem, jeśli wartość procentową próbki jest mniejsza od `samplePercent` określone, możesz użyć `GetSamplePercent` , aby sprawdzić, najpierw. Następnie można wykonać akcję alternatywną, jeśli próbki niewystarczające, bez zatrzymywanie automatycznej oceny skalowania.|

### <a name="samples-sample-percentage-and-the-getsample-method"></a>Przykłady, procent próbki i metody *GetSample()*

Operacja podstawowe formuły autoscale jest pobierają dane metryczne, zadań i zasobów, a następnie dopasuj rozmiar puli na podstawie tych danych. Jako takie jest ważne informacje o interakcja autoscale formuł z danych metryki lub "próbki."

**Przykłady**

Usługa partii okresowo trwa *próbki* metryki zadań i zasobów i udostępnia je do formuł autoscale. Te przykłady są rejestrowane co 30 sekund przez usługę partię. Istnieje jednak zwykle niektóre opóźnienie, który powoduje opóźnienia między kiedy zarejestrowano próbkach i po ich są udostępniane na (i mogą być odczytywane przez) formuły autoscale. Ponadto ze względu na różnych czynników, takich jak sieci lub inne problemy z infrastruktury, próbki może nie być zarejestrowana dla określonego zakresu. Powoduje próbki "Brak".

**Przykładowa wartość procentową**

Gdy `samplePercent` przekazywana do funkcji `GetSample()` metody lub `GetSamplePercent()` metoda jest wywoływana, "%" odnosi się do porównania między liczbą całkowitą *możliwe* próbek, które są rejestrowane przez usługę partii i liczba próbek, które są *dostępne* do formuły autoscale.

Przyjrzyjmy się przedziału 10 minut czasu jako przykład. Ponieważ próbki są rejestrowane co 30 sekund, w ramach przedziału czasu 10 minut, maksymalna liczba próbek, które są rejestrowane w partiach będzie 20 próbek (2 na minutę). Jednak ze względu na opóźnienie związane mechanizm raportowania lub niektórych innego problemu w ramach infrastruktury Azure, może istnieć tylko 15 próbek dostępnych do formuły autoscale do czytania. Oznacza to, że dla tego okresu 10 minut tylko **75 procent** całkowitej liczby próbki rejestrowane są rzeczywiście dostępne do formuły.

**GetSample() i przykładowe zakresy**

Formuły autoscale mają wzrostu i zmniejszanie pul — węzły Dodawanie lub usuwanie węzłów. Ponieważ węzły kosztów pieniądze, chcesz mieć pewność, że formuły za pomocą inteligentnego metody analizy, oparty na odpowiednie dane. W związku z tym firma Microsoft zaleca używanie analizy typu popularny w formułach. Tego typu będą powiększanie i zmniejszanie usługi pul na podstawie *zakresu* próbek zebranych.

W tym celu należy użyć `GetSample(interval look-back start, interval look-back end)` zwraca **wektorowa** próbek:

```
$runningTasksSample = $RunningTasks.GetSample(1 * TimeInterval_Minute, 6 * TimeInterval_Minute);
```

Gdy wiersz powyżej jest obliczane przez partię, zwróci zakres próbki jako wektor wartości. Na przykład:

```
$runningTasksSample=[1,1,1,1,1,1,1,1,1,1];
```

Po zgromadzonych w wektorze próbki, możesz następnie używać funkcji, takich jak `min()`, `max()`, i `avg()` do uzyskania zrozumiałej wartości z zakresu zbierane.

Celu zwiększenia bezpieczeństwa można wymusić formuły kończy się *niepowodzeniem* , jeśli jest mniejsza niż wartość procentową próbki dostępna przez określony czas. Po wymuszeniu formuły niepowodzenie poinstruuj partii zaprzestać dodatkowo oceny formuły, jeśli określony procent próbek nie jest dostępna — i zostaną wprowadzone żadne zmiany rozmiaru puli. Aby określić wymagane procent próbek do oceny powiodła się, określ go jako trzeciego parametru do `GetSample()`. W tym miejscu jest określony wymóg 75 procent próbki:

```
$runningTasksSample = $RunningTasks.GetSample(60 * TimeInterval_Second, 120 * TimeInterval_Second, 75);
```

Ważne jest również, z powodu wcześniej wspomniano opóźnienie w dostępność próbki, aby zawsze określić zakres czasu czas rozpoczęcia wstecz wygląd starszych niż minutę. Dzieje się tak dlatego trwa około minuty dla próbki propagowanie za pomocą systemu, dlatego próbki w zakresie `(0 * TimeInterval_Second, 60 * TimeInterval_Second)` często nie będzie dostępna. Ponownie, można użyć parametru procent z `GetSample()` wymusić wymagane wartości procentowej określonej próbki.

> [AZURE.IMPORTANT] Firma Microsoft **zaleca** tego można * *uniknąć, opierając się *tylko* na `GetSample(1)` w formułach autoscale **. Dzieje się tak dlatego `GetSample(1)` zasadniczo komunikat z usługą partii "Podaj ostatniej próbki masz, niezależnie od tego, jak długo trwa temu masz." Ponieważ jest pojedynczej próbki i może być starsze próbki, może być przedstawiciela większy obraz ostatnie zadania lub stanu zasobu. Jeśli korzystasz z `GetSample(1)`, upewnij się, że jest częścią większych poufności i nie zależy od formuły punktu tylko dane.

## <a name="metrics"></a>Metryki

Podczas definiowania formuły, możesz użyć metryki zarówno **zasobów** i **zadaniami** . Możesz dostosować docelowej liczby węzłów dedykowane w puli na podstawie danych metryki uzyskać i oceny. Zobacz sekcję [zmienne](#variables) powyżej, aby uzyskać więcej informacji na każdej jednostki metryczne.

<table>
  <tr>
    <th>Metryka</th>
    <th>Opis</th>
  </tr>
  <tr>
    <td><b>Zasób</b></td>
    <td><p><b>Metryki zasobów</b> są oparte na użycie Procesora, przepustowości i pamięci węzły obliczeń, jak również liczby węzłów.</p>
        <p> Zmienne zdefiniowane przez usługę są przydatne w przypadku wprowadzanie korekt liczy węzeł:</p>
    <p><ul>
      <li>$TargetDedicated</li>
            <li>$CurrentDedicated</li>
            <li>$SampleNodeCount</li>
    </ul></p>
    <p>Zmienne zdefiniowane usługi są przydatne do wprowadzanie korekt według węzeł użycie zasobu:</p>
    <p><ul>
      <li>$CPUPercent</li>
      <li>$WallClockSeconds</li>
      <li>$MemoryBytes</li>
      <li>$DiskBytes</li>
      <li>$DiskReadBytes</li>
      <li>$DiskWriteBytes</li>
      <li>$DiskReadOps</li>
      <li>$DiskWriteOps</li>
      <li>$NetworkInBytes</li>
      <li>$NetworkOutBytes</li></ul></p>
  </tr>
  <tr>
    <td><b>Zadanie</b></td>
    <td><p><b>Zadania metryki</b> na podstawie stanu zadań, takich jak aktywne, oczekujące i wykonane. Następujące zmienne zdefiniowane usługi są przydatne do wprowadzanie korekt rozmiar puli na podstawie metryk zadania:</p>
    <p><ul>
      <li>$ActiveTasks</li>
      <li>$RunningTasks</li>
      <li>$PendingTasks</li>
      <li>$SucceededTasks</li>
            <li>$FailedTasks</li></ul></p>
        </td>
  </tr>
</table>

## <a name="write-an-autoscale-formula"></a>Pisanie formuły autoscale

Możesz utworzyć formułę autoscale przez tworzenie instrukcji używających powyżej składniki, a następnie połączyć te instrukcje do całej formuły. W tej sekcji utworzymy autoscale przykład formuły, które może wykonać kilku decyzji skalowania rzeczywistych.

Najpierw Przyjrzyjmy Definiowanie wymagań dotyczących naszych nową formułę autoscale. Formuła powinna:

1. **Zwiększanie** docelowej liczby węzłów obliczeń w puli, jeśli użycie Procesora jest wysoki.
2. **Zmniejszanie** docelowej liczby węzłów obliczeń w puli, gdy użycie Procesora jest niska.
3. Zawsze należy ograniczyć **maksymalną** liczbę węzłów do 400.

Aby *zwiększyć* liczby węzłów podczas wysoka użycie Procesora, możemy Definiowanie instrukcji, które wypełnia zmiennej zdefiniowane przez użytkownika (`$totalNodes`) jest 110 procent bieżącej docelowej liczby węzłów, ale tylko wtedy, gdy wartość minimalną średnią użycie Procesora podczas ostatniego 10 minut była powyżej 70 procent. W przeciwnym razie firma Microsoft korzysta z bieżącej wartości dedykowanego.

```
$totalNodes =
    (min($CPUPercent.GetSample(TimeInterval_Minute * 10)) > 0.7) ?
    ($CurrentDedicated * 1.1) : $CurrentDedicated;
```

*Zmniejszanie* liczby węzłów podczas niskie użycie Procesora, następnej instrukcji w naszym formuły zestawów tak samo `$totalNodes` zmiennych 90 procent bieżącej docelowej liczby węzłów, jeżeli średnia użycie Procesora w ciągu ostatnich 60 minut w obszarze 20 procent. W przeciwnym razie użyj bieżącej wartości `$totalNodes` którą możemy wypełnione podanych instrukcji.

```
$totalNodes =
    (avg($CPUPercent.GetSample(TimeInterval_Minute * 60)) < 0.2) ?
    ($CurrentDedicated * 0.9) : $totalNodes;
```

Teraz Ogranicz docelowej liczby węzłów dedykowane obliczeń **Maksymalna** 400:

```
$TargetDedicated = min(400, $totalNodes)
```

Oto całej formuły:

```
$totalNodes =
    (min($CPUPercent.GetSample(TimeInterval_Minute * 10)) > 0.7) ?
    ($CurrentDedicated * 1.1) : $CurrentDedicated;
$totalNodes =
    (avg($CPUPercent.GetSample(TimeInterval_Minute * 60)) < 0.2) ?
    ($CurrentDedicated * 0.9) : $totalNodes;
$TargetDedicated = min(400, $totalNodes)
```

## <a name="create-an-autoscale-enabled-pool"></a>Tworzenie puli autoscale — informacje

Aby utworzyć nową pulę z autoscaling włączone, użyj jednej z następujących technik:

**Wsadowe .NET**

1. Tworzenie puli z [BatchClient.PoolOperations.CreatePool](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx).
1. Ustawienie właściwości [CloudPool.AutoScaleEnabled](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscaleenabled.aspx) `true`.
1. Należy ustawić właściwość [CloudPool.AutoScaleFormula](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscaleformula.aspx) przy użyciu formuły autoscale.
1. (Opcjonalnie) Należy ustawić właściwość [CloudPool.AutoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoScaleevaluationinterval.aspx) (wartość domyślna to 15 minut).
1. Przekaż puli z [CloudPool.Commit](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.commit.aspx) lub [CommitAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.commitasync.aspx).

**Partia interfejsu API usługi REST**

* [Dodawanie puli do konta](https://msdn.microsoft.com/library/azure/dn820174.aspx): Określ `enableAutoScale` i `autoScaleFormula` elementy w wezwaniu interfejsu API usługi REST skonfigurować automatyczne skalowanie puli po jego utworzeniu.

Poniższy fragment kodu tworzy puli obsługą autoscale przy użyciu [.NET partii] [ net_api] biblioteki. Formuła autoscale w puli ustawia docelowej liczby węzłów na 5 w poniedziałki i 1 dla każdego dnia tygodnia. [Interwał automatycznego skalowania](#automatic-scaling-interval) wynosi 30 minut. W tym i innych C# wstawki w tym artykule "myBatchClient" jest prawidłowo zainicjowany wystąpienia [BatchClient][net_batchclient].

```csharp
CloudPool pool = myBatchClient.PoolOperations.CreatePool("mypool", "3", "small");
pool.AutoScaleEnabled = true;
pool.AutoScaleFormula = "$TargetDedicated = (time().weekday == 1 ? 5:1);";
pool.AutoScaleEvaluationInterval = TimeSpan.FromMinutes(30);
pool.Commit();
```

Oprócz interfejsu API usługi REST partii i .NET SDK służy innych [SDK partię](batch-technical-overview.md#batch-development-apis), [partii poleceń cmdlet](batch-powershell-cmdlets-get-started.md)i [Polecenie partii](batch-cli-get-started.md) do pracy z autoscaling.

> [AZURE.IMPORTANT] Po utworzeniu puli obsługą autoscale należy **nie** określ `targetDedicated` parametru. Ponadto, jeśli chcesz ręcznie zmienić rozmiar puli autoscale — informacje (na przykład z [BatchClient.PoolOperations.ResizePool][net_poolops_resizepool]), musisz najpierw pierwszy, **Wyłącz** automatyczne skalowanie w puli, zmienić jego rozmiar.

### <a name="automatic-scaling-interval"></a>Interwał automatycznego skalowania

Domyślnie usługa partii dopasowuje rozmiar puli zgodnie z odpowiednim wzorem autoscale co **15 minut**. Interwał ten jest konfigurowane, jednak za pomocą puli następujące właściwości:

* [CloudPool.AutoScaleEvaluationInterval] [ net_cloudpool_autoscaleevalinterval] (wsadowe .NET)
* [autoScaleEvaluationInterval] [ rest_autoscaleinterval] (interfejsu API usługi REST)

Minimalny interwał jest pięć minut, a wartość maksymalna jest 168 godzin. Jeśli jest określony interwał poza ten zakres, usługę partii zwróci błąd nieprawidłowe żądanie (400).

> [AZURE.NOTE] Autoscaling nie ma obecnie odpowiedzieć na zmiany w ciągu mniej niż minuty, ale zamiast ma Dopasowywanie rozmiaru puli stopniowo podczas pracy obciążenie pracą.

## <a name="enable-autoscaling-on-an-existing-pool"></a>Włączanie autoscaling na istniejącej puli

Jeśli masz już utworzony puli z ustalonej liczby węzłów obliczeń przy użyciu parametru *targetDedicated* , możesz włączyć autoscaling w puli. Każdy zestaw SDK partii zawiera operacji "jako Włącz skalowanie automatyczne", na przykład:

* [BatchClient.PoolOperations.EnableAutoScale] [ net_enableautoscale] (wsadowe .NET)
*  [Włącz automatyczne skalowanie w puli] [ rest_enableautoscale] (interfejsu API usługi REST)

Po włączeniu autoscaling na istniejącej puli stosuje się:

* Jeśli automatyczne skalowanie jest obecnie **wyłączona** w puli wpisanie żądanie "jako Włącz skalowanie automatyczne", *musi* Określ formułę autoscale prawidłowe w przypadku problemu żądania. *Opcjonalnie* można określić przedział oceny autoscale. Jeśli nie określisz interwału, zostanie użyta wartość domyślna 15 minut.

* Jeśli autoscale nie jest obecnie **włączone** w puli, można określić formułę autoscale i interwału oceny. Nie można pominąć obie właściwości.

  * Jeśli użytkownik określi przedział oceny autoscale, zatrzymaniu istniejący harmonogram oceny i uruchomieniu nowego harmonogramu. Czas rozpoczęcia nowego harmonogramu jest czas, w którym został wydany wezwanie "Włącz autoscale".

  * W przypadku pominięcia formuły autoscale lub oceny interwału, usługa partii nadal użyć bieżącej wartości tego ustawienia.

> [AZURE.NOTE] Jeśli wartość został określony w parametrze *targetDedicated* podczas tworzenia puli, jest to ignorowany, gdy szacowana jest funkcja Automatyczne skalowanie formuły.

Ten wstawkę kodu C# używa [.NET partii] [ net_api] biblioteki w celu umożliwienia autoscaling na istniejącej puli:

```csharp
// Define the autoscaling formula. This formula sets the target number of nodes
// to 5 on Mondays, and 1 on every other day of the week
string myAutoScaleFormula = "$TargetDedicated = (time().weekday == 1 ? 5:1);";

// Set the autoscale formula on the existing pool
myBatchClient.PoolOperations.EnableAutoScale(
    "myexistingpool",
    autoscaleFormula: myAutoScaleFormula);
```

### <a name="update-an-autoscale-formula"></a>Zaktualizuj formułę autoscale

Używanie samego żądania *aktualizacji* formułę "Włącz autoscale" na istniejącej puli obsługą autoscale (na przykład z [EnableAutoScale] [ net_enableautoscale] w partii .NET). Istnieje bez specjalnych operacji "Aktualizowanie autoscale". Na przykład jeśli autoscaling jest już włączone dla "myexistingpool", gdy jest wykonywane poniższy kod, formuła autoscale zostanie zastąpiony zawartość `myNewFormula`.

```csharp
myBatchClient.PoolOperations.EnableAutoScale(
    "myexistingpool",
    autoscaleFormula: myNewFormula);
```

### <a name="update-the-autoscale-interval"></a>Interwał autoscale aktualizacji

Jak o aktualizowaniu formułę autoscale, korzystasz z tym samym [EnableAutoScale] [ net_enableautoscale] metodę, aby zmienić interwał oceny autoscale istniejącej puli obsługą autoscale. Na przykład, aby ustawić interwał oceny autoscale 60 minut puli, który jest już włączone autoscale:

```csharp
myBatchClient.PoolOperations.EnableAutoScale(
    "myexistingpool",
    autoscaleEvaluationInterval: TimeSpan.FromMinutes(60));
```

## <a name="evaluate-an-autoscale-formula"></a>Szacowanie formuły autoscale

Przed zastosowaniem w puli, możesz oszacować formuły. W ten sposób można wykonywać "wykonanie testowe" formułę, aby zobaczyć, jak sprawozdanie oceny przed wprowadzane formułę do produkcji.

Aby Szacuj formułę autoscale, musisz najpierw pierwszy **włączyć autoscaling** w puli z **prawidłową formułę**. Jeśli chcesz przetestować formułę w puli, która jeszcze nie zawiera autoscaling włączone, możesz użyć formuły jednego wiersza `$TargetDedicated = 0` po raz pierwszy włączyć autoscaling. Następnie należy użyć do Szacuj formułę, którą chcesz przetestować jedną z następujących czynności:

* [BatchClient.PoolOperations.EvaluateAutoScale](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.evaluateautoscale.aspx) lub [EvaluateAutoScaleAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.evaluateautoscaleasync.aspx)

    Te metody .NET partii wymagają Identyfikatora istniejącej puli i ciąg zawierający formułę autoscale ma być obliczona. Wyniki obliczeń są zawarte w zwracanych wystąpienia [AutoScaleEvaluation](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscaleevaluation.aspx) .

* [Szacowanie formuły skalowania automatycznego](https://msdn.microsoft.com/library/azure/dn820183.aspx)

    W tym żądaniu interfejsu API usługi REST Określ puli Identyfikatora URI i formuła autoscale w elemencie *autoScaleFormula* treści żądania. Odpowiedź operacji zawiera informacje o błędzie, który może być związany z formułą.

W tej [Partii .NET] [ net_api] wstawkę kodu, możemy Szacuj formułę przed zastosowaniem go w [CloudPool][net_cloudpool]. Jeśli puli nie ma autoscaling włączone, możemy włączyć najpierw.

```csharp
// First obtain a reference to an existing pool
CloudPool pool = batchClient.PoolOperations.GetPool("myExistingPool");

// If autoscaling isn't already enabled on the pool, enable it.
// You can't evaluate an autoscale formula on non-autoscale-enabled pool.
if (pool.AutoScaleEnabled == false)
{
    // We need a valid autoscale formula to enable autoscaling on the
    // pool. This formula is valid, but won't resize the pool:
    pool.EnableAutoScale(
        autoscaleFormula: $"$TargetDedicated = {pool.CurrentDedicated};",
        autoscaleEvaluationInterval: TimeSpan.FromMinutes(5));

    // Batch limits EnableAutoScale calls to once every 30 seconds.
    // Because we want to apply our new autoscale formula below if it
    // evaluates successfully, and we *just* enabled autoscaling on
    // this pool, we pause here to ensure we pass that threshold.
    Thread.Sleep(TimeSpan.FromSeconds(31));

    // Refresh the properties of the pool so that we've got the
    // latest value for AutoScaleEnabled
    pool.Refresh();
}

// We must ensure that autoscaling is enabled on the pool prior to
// evaluating a formula
if (pool.AutoScaleEnabled == true)
{
    // The formula to evaluate - adjusts target number of nodes based on
    // day of week and time of day
    string myFormula = @"
        $curTime = time();
        $workHours = $curTime.hour >= 8 && $curTime.hour < 18;
        $isWeekday = $curTime.weekday >= 1 && $curTime.weekday <= 5;
        $isWorkingWeekdayHour = $workHours && $isWeekday;
        $TargetDedicated = $isWorkingWeekdayHour ? 20:10;
    ";

    // Perform the autoscale formula evaluation. Note that this does not
    // actually apply the formula to the pool.
    AutoScaleRun eval =
        batchClient.PoolOperations.EvaluateAutoScale(pool.Id, myFormula);

    if (eval.Error == null)
    {
        // Evaluation success - print the results of the AutoScaleRun.
        // This will display the values of each variable as evaluated by the
        // autoscale formula.
        Console.WriteLine("AutoScaleRun.Results: " +
            eval.Results.Replace("$", "\n    $"));

        // Apply the formula to the pool since it evaluated successfully
        batchClient.PoolOperations.EnableAutoScale(pool.Id, myFormula);
    }
    else
    {
        // Evaluation failed, output the message associated with the error
        Console.WriteLine("AutoScaleRun.Error.Message: " +
            eval.Error.Message);
    }
}
```

Pomyślnego obliczonego wyniku formuły w tym wstawek spowoduje dane wyjściowe podobne do następujących czynności:

```
AutoScaleRun.Results:
    $TargetDedicated=10;
    $NodeDeallocationOption=requeue;
    $curTime=2016-10-13T19:18:47.805Z;
    $isWeekday=1;
    $isWorkingWeekdayHour=0;
    $workHours=0
```

## <a name="get-information-about-autoscale-runs"></a>Uzyskiwanie informacji na temat zostanie uruchomiona autoscale

Aby upewnić się, że formuła działa zgodnie z oczekiwaniami, zaleca się, że okresowo sprawdzić wyniki autoscaling "działa" wsadowe wykonuje w puli usługi. Tak, pobieranie (lub Odśwież) odwołanie do puli i sprawdź właściwości swojej ostatniego autoscale uruchamianie.

W partii .NET właściwość [CloudPool.AutoScaleRun](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscalerun.aspx) ma niektóre właściwości, dostarczając informacji o najnowsze automatyczne skalowanie uruchamiając wykonywanych w puli usługi partii.

* [AutoScaleRun.Timestamp](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscalerun.timestamp.aspx)
* [AutoScaleRun.Results](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscalerun.results.aspx)
* [AutoScaleRun.Error](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscalerun.error.aspx)

W interfejsie API usługi REST wezwanie na [Uzyskiwanie informacji na temat puli](https://msdn.microsoft.com/library/dn820165.aspx) zwraca informacje o puli, która obejmuje najnowsze automatyczne skalowanie uruchomić informacji w [autoScaleRun](https://msdn.microsoft.com/library/dn820165.aspx#bk_autrun).

Poniższy fragment kodu C# korzysta z biblioteki .NET partii do wydrukowania informacji na temat ostatniego autoscaling, uruchom w puli "myPool":

```csharp
Cloud pool = myBatchClient.PoolOperations.GetPool("myPool");
Console.WriteLine("Last execution: " + pool.AutoScaleRun.Timestamp);
Console.WriteLine("Result:" + pool.AutoScaleRun.Results.Replace("$", "\n  $"));
Console.WriteLine("Error: " + pool.AutoScaleRun.Error);
```

Przykładowe wyniki poprzedniego wstawki kodu:

```
Last execution: 10/14/2016 18:36:43
Result:
  $TargetDedicated=10;
  $NodeDeallocationOption=requeue;
  $curTime=2016-10-14T18:36:43.282Z;
  $isWeekday=1;
  $isWorkingWeekdayHour=0;
  $workHours=0
Error:
```

## <a name="example-autoscale-formulas"></a>Przykładowe formuły autoscale

Spójrzmy na kilka formuł, które przedstawiono różne sposoby dostosować ilości zasobów obliczeń w puli.

### <a name="example-1-time-based-adjustment"></a>Przykład 1: Opartych na czasie korekty

Być może chcesz dopasować rozmiar puli na podstawie dzień tygodnia i godzinę, aby zwiększyć lub zmniejszyć odpowiednio liczby węzłów w puli.

Ta formuła najpierw uzyskuje bieżącą godzinę. Jeśli jest weekday (1-5) oraz w godzinach pracy (8 AM do 6 PM), rozmiar puli docelowej wynosi 20 węzły. W przeciwnym razie jest ustawiony na 10 węzły.

```
$curTime = time();
$workHours = $curTime.hour >= 8 && $curTime.hour < 18;
$isWeekday = $curTime.weekday >= 1 && $curTime.weekday <= 5;
$isWorkingWeekdayHour = $workHours && $isWeekday;
$TargetDedicated = $isWorkingWeekdayHour ? 20:10;
```

### <a name="example-2-task-based-adjustment"></a>Przykład 2: Opartym na zadaniu korekty

W tym przykładzie rozmiar puli jest określany na liczbę zadań w kolejce podstawie. Należy zauważyć, że zarówno komentarze, jak i podziały wierszy są akceptowane w ciągach formuły.

```csharp
// Get pending tasks for the past 15 minutes.
$samples = $ActiveTasks.GetSamplePercent(TimeInterval_Minute * 15);
// If we have fewer than 70 percent data points, we use the last sample point,
// otherwise we use the maximum of last sample point and the history average.
$tasks = $samples < 70 ? max(0,$ActiveTasks.GetSample(1)) : max( $ActiveTasks.GetSample(1), avg($ActiveTasks.GetSample(TimeInterval_Minute * 15)));
// If number of pending tasks is not 0, set targetVM to pending tasks, otherwise
// half of current dedicated.
$targetVMs = $tasks > 0? $tasks:max(0, $TargetDedicated/2);
// The pool size is capped at 20, if target VM value is more than that, set it
// to 20. This value should be adjusted according to your use case.
$TargetDedicated = max(0, min($targetVMs, 20));
// Set node deallocation mode - keep nodes active only until tasks finish
$NodeDeallocationOption = taskcompletion;
```

### <a name="example-3-accounting-for-parallel-tasks"></a>Przykład 3: Uwzględniania równoległe zadania

Jest to kolejny przykład dopasowuje rozmiar puli na podstawie liczby zadań. Ta formuła uwzględnia również [MaxTasksPerComputeNode] [ net_maxtasks] wartości, która została ustawiona dla puli. To jest szczególnie przydatne w sytuacjach, gdzie została włączona [Wykonywanie zadań równoległych](batch-parallel-node-tasks.md) na użytkownika.

```csharp
// Determine whether 70 percent of the samples have been recorded in the past
// 15 minutes; if not, use last sample
$samples = $ActiveTasks.GetSamplePercent(TimeInterval_Minute * 15);
$tasks = $samples < 70 ? max(0,$ActiveTasks.GetSample(1)) : max( $ActiveTasks.GetSample(1),avg($ActiveTasks.GetSample(TimeInterval_Minute * 15)));
// Set the number of nodes to add to one-fourth the number of active tasks (the
// MaxTasksPerComputeNode property on this pool is set to 4, adjust this number
// for your use case)
$cores = $TargetDedicated * 4;
$extraVMs = (($tasks - $cores) + 3) / 4;
$targetVMs = ($TargetDedicated + $extraVMs);
// Attempt to grow the number of compute nodes to match the number of active
// tasks, with a maximum of 3
$TargetDedicated = max(0,min($targetVMs,3));
// Keep the nodes active until the tasks finish
$NodeDeallocationOption = taskcompletion;
```

### <a name="example-4-setting-an-initial-pool-size"></a>Przykład 4: Ustawianie rozmiaru początkowego puli

Ten przykład przedstawia wstawkę kodu C# za pomocą formuły autoscale, który ustawia rozmiar puli do liczby węzłów na okres pierwszego okresu. A następnie go dopasowuje rozmiar puli, oparte na ilości pracy i aktywne zadania po upływie początkowego okresu.

Formuła w następujący fragment kodu:

- Ustawia rozmiar puli początkowej czterech węzłów.
- Nie Dopasowywanie rozmiaru puli w ciągu pierwszych 10 minut w puli cyklu życia.
- Po 10 minutach pobiera wartość maksymalna liczba uruchomiony i aktywne zadania w ciągu ostatnich 60 minut.
  - Jeśli obie wartości są równe 0 (co oznacza, że żadne zadania były uruchomione lub aktywna w ciągu ostatnich 60 minut), rozmiar puli jest równa 0.
  - Jeśli wartość któregokolwiek jest większa od zera, są wprowadzane żadne zmiany.

```csharp
string now = DateTime.UtcNow.ToString("r");
string formula = string.Format(@"
    $TargetDedicated = {1};
    lifespan         = time() - time(""{0}"");
    span             = TimeInterval_Minute * 60;
    startup          = TimeInterval_Minute * 10;
    ratio            = 50;

    $TargetDedicated = (lifespan > startup ? (max($RunningTasks.GetSample(span, ratio), $ActiveTasks.GetSample(span, ratio)) == 0 ? 0 : $TargetDedicated) : {1});
    ", now, 4);
```

## <a name="next-steps"></a>Następne kroki

* [Maksymalizowanie partii Azure obliczyć użycie zasobu z zadania równoczesne węzeł](batch-parallel-node-tasks.md) zawiera szczegółowe informacje dotyczące sposobu można wykonywać wielu zadań jednocześnie w węzłach obliczeń w puli użytkownika. Oprócz autoscaling ta funkcja może pomóc zmniejszyć czas trwania zadania dla niektórych obciążeń pracą, możesz oszczędności.

* Dla innego detonatora efektywności upewnij się, że aplikacja partii korzysta z usługi partii w najbardziej optymalny sposób. W [efektywne kwerendy usługi Azure partię](batch-efficient-list-queries.md)dowiesz się, jak ograniczyć ilość danych w sieci kwerendy stanu potencjalnie tysięcy węzły obliczeń lub zadania.

[net_api]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_batchclient]: http://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_cloudpool_autoscaleformula]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscaleformula.aspx
[net_cloudpool_autoscaleevalinterval]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscaleevaluationinterval.aspx
[net_enableautoscale]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.enableautoscale.aspx
[net_maxtasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.maxtaskspercomputenode.aspx
[net_poolops_resizepool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.resizepool.aspx

[rest_api]: https://msdn.microsoft.com/library/azure/dn820158.aspx
[rest_autoscaleformula]: https://msdn.microsoft.com/library/azure/dn820173.aspx
[rest_autoscaleinterval]: https://msdn.microsoft.com/library/azure/dn820173.aspx
[rest_enableautoscale]: https://msdn.microsoft.com/library/azure/dn820173.aspx
