<properties
   pageTitle="Kolekcje niezawodne | Microsoft Azure"
   description="Usługi stanowe tkaninie usługi zapewniają zaufanego kolekcje, które umożliwiają pisanie aplikacje w chmurze wysoce dostępne, skalowalna i małym opóźnieniem."
   services="service-fabric"
   documentationCenter=".net"
   authors="mcoskun"
   manager="timlt"
   editor="masnider,vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="10/18/2016"
   ms.author="mcoskun"/>

# <a name="introduction-to-reliable-collections-in-azure-service-fabric-stateful-services"></a>Wprowadzenie do kolekcji zaufanego w usługach stanowe tkaninie usługi Azure

Kolekcje zaufanego umożliwiają pisanie aplikacje w chmurze wysoce dostępne, skalowalna i małym opóźnieniem, tak jakby podczas pisania aplikacji na jednym komputerze. Klas w obszarze nazw **Microsoft.ServiceFabric.Data.Collections** zawierają zestaw zbiorów w nowym polu, które automatycznie swój stan wysokiej dostępności. Deweloperzy muszą programu tylko do zaufanego API zbioru i Zezwól zaufanego zbiory Zarządzanie stanem replikowane i lokalny.

Klucza różnicę między zbiorami niezawodne i inne technologie wysokiej dostępności (na przykład Redis, usłudze Azure tabeli i usługa Azure kolejki) to, że stan przechowywanej lokalnie w wystąpieniu usług podczas także udostępniane na wysoce. Oznacza to, że:

- Wszystkie operacje odczytu są lokalne, które powoduje krótki czas oczekiwania i odczytuje wysokiej przepustowości.
- Zapisuje wszystkie ponoszenia minimalna liczba IOs sieci, co spowoduje krótki czas oczekiwania i zapisuje wysokiej przepustowości.

![Obraz zmiany kolekcji.](media/service-fabric-reliable-services-reliable-collections/ReliableCollectionsEvolution.png)

Kolekcje zaufanego można traktować jako naturalne kształtowanie klas **System.Collections** : nowy zestaw kolekcji, które są przeznaczone dla aplikacji chmury i wieloosobowe bez powiększania złożoność dla deweloperów. Jako takie zaufanego kolekcje są:

- Replikowane: Zmian stanu są replikowane wysokiej dostępności.
- Trwałe: Danych jest zachowywane na dysku dla wytrzymałości przed dużych dostawie (na przykład awaria zasilania centrum danych).
- Asynchroniczne: Interfejsy API są asynchroniczne zapewnienie wątków nie są blokowane po ponoszenia Jo.
- Transakcji: Pozyskiwania transakcje używają interfejsów API, więc łatwe zarządzanie wielu zbiorów zaufanego w ramach usługi.

Kolekcje zaufanego Podaj gwarancje silnych spójności poza pole w celu ułatwienia uzasadnienie o stanie aplikacji.
Silne spójności uzyskuje się poprzez zapewnienie transakcji, które zatwierdzenia zakończenie tylko wtedy, gdy cała transakcja zalogował kworum Większość repliki, w tym podstawowych.
W celu osiągnięcia spójności słabsze, aplikacje mogą Potwierdź powrót do klienta i nadawcy przed zwraca asynchroniczne Zatwierdź.

Niezawodne API zbiory są kształtowanie równoczesne zbiory interfejsów API (znajdujący się w obszarze nazw **System.Collections.Concurrent** ):

- Asynchroniczne: Zwraca zadania, ponieważ w odróżnieniu od zbiorów równoczesne operacje są replikowane i zachowywane.
- Nie parametry wyjściowe: używa `ConditionalValue<T>` zwraca wartość logiczna i wartości zamiast parametry wyjściowe. `ConditionalValue<T>`przypomina `Nullable<T>` , ale nie wymaga T, aby być struktury.
- Transakcje: Używa obiektu transakcji, aby umożliwić użytkownikowi grupa akcje na wielu zbiorów niezawodne w transakcji.

Obecnie **Microsoft.ServiceFabric.Data.Collections** zawiera dwie kolekcje:

- [Słownik zaufanego](https://msdn.microsoft.com/library/azure/dn971511.aspx): reprezentuje kolekcję replikowane, transakcji i asynchroniczne par klucz wartość. Podobnie jak **ConcurrentDictionary**, zarówno klucz i wartość mogą być dowolnego typu.
- [Niezawodne kolejki](https://msdn.microsoft.com/library/azure/dn971527.aspx): reprezentuje replikowane, transakcji i asynchroniczne ściśle pierwszego w, first-out (FIFO) kolejki. Podobnie jak **ConcurrentQueue**, wartość może być dowolnego typu.

## <a name="isolation-levels"></a>Poziomów izolacji
Poziom izolacji określa stopień, do którego transakcji muszą zostać odizolowane od zmian wprowadzonych przez inne transakcje.
Istnieją dwa poziomy izolacji, obsługiwane w kolekcji zaufanego:

- **Przeczytaj powtarzalnych**: Określa, że instrukcje odczytanie danych, które zostały zmodyfikowane, ale nie zostały jeszcze zatwierdzone przez inne transakcje i że żadne inne transakcje można zmodyfikować dane, które przeczytana przez bieżącej transakcji, dopóki nie zakończy się bieżącej transakcji. Aby uzyskać więcej informacji zobacz [https://msdn.microsoft.com/library/ms173763.aspx](https://msdn.microsoft.com/library/ms173763.aspx).
- **Migawka**: Określa, że dane odczytane przy każdej instrukcji w transakcji będzie transakcyjnie spójne wersję danych na początku transakcji.
Transakcji można rozpoznać tylko modyfikacji danych, które zostały zatwierdzone przed rozpoczęciem transakcji.
Modyfikacji danych wprowadzonych przez inne transakcje po rozpoczęciu bieżącej transakcji nie są widoczne dla instrukcji wykonywania w bieżącej transakcji.
Efekt jest tak, jakby instrukcje w transakcji uzyskać migawkę danych Projekt zatwierdzony — znajdowały się na początku transakcji.
Migawki są zgodne między zbiorami niezawodne.
Aby uzyskać więcej informacji zobacz [https://msdn.microsoft.com/library/ms173763.aspx](https://msdn.microsoft.com/library/ms173763.aspx).

Kolekcje zaufanego automatycznie wybierz żądany poziom izolacji dla danego operacja odczytu, w zależności od operacji i roli replice w momencie tworzenia transakcji.
Poniżej znajduje się tabela przedstawiający izolacji poziomu ustawień domyślnych dla zaufanego słownika i kolejki czynności.

| Operacja \ roli      | Podstawowy          | Pomocnicze        |
| --------------------- | :--------------- | :--------------- |
| Całość odczytu    | Przeczytaj powtarzalnych  | Migawka         |
| Wyliczenie \ liczba   | Migawka         | Migawka         |

>[AZURE.NOTE] Przykłady typowych dla jednej operacji jednostki `IReliableDictionary.TryGetValueAsync`, `IReliableQueue.TryPeekAsync`.

Słownik zaufanego oraz wiarygodnych kolejki obsługuje odczytu i zapisu.
Innymi słowy wszelkie zapisu w obrębie transakcji będą widoczne na następujących odczytywaną należący do tej samej transakcji.

## <a name="locking"></a>Blokowanie
W obszarze zbiory niezawodne wszystkich transakcji są stopniowo dwóch: transakcji zwolnienia blokad uzyskał aż do zakończenia transakcji z przerwania lub zatwierdzenia.

Słownik zaufanego używa blokowania dla wszystkich operacji całość na poziomie wiersza.
Niezawodne kolejki zajmują wyłączanie współbieżności ściśle transakcji właściwości FIFO.
Operacja blokady poziomu umożliwiające utworzenie jednej transakcji korzysta z zaufanego kolejki `TryPeekAsync` i/lub `TryDequeueAsync` i jednej transakcji z `EnqueueAsync` w danej chwili.
Zauważyć, że w celu zachowania FIFO, jeśli `TryPeekAsync` lub `TryDequeueAsync` kiedykolwiek przestrzega, że zaufanego kolejka jest pusta, również powoduje zablokowanie `EnqueueAsync`.

Pisanie, że operacje zawsze pobierają blokady z wyłącznością.
Operacji odczytu blokowania zależy od kilku czynników.
Wszelkie operacja odczytu wykonywane przy użyciu izolacji migawki jest zablokowana bezpłatne.
Wszelkie operacja odczytu powtarzalnych domyślnie trwa blokady udostępnione.
Jednak dla dowolnej operacji odczytu obsługującego powtarzalnych odczytu użytkownika można uzyskać blokady aktualizacji zamiast blokady udostępnione.
Blokada aktualizacji jest blokada asymetrycznym zapobiegania wspólnej formy zakleszczenia wykonywana w przypadku wielu transakcji blokowania zasobów potencjalne aktualizacje w późniejszym czasie.

Macierz zgodności blokad znajduje się poniżej:

| Żądanie \ udzielił | Brak         | Udostępnione       | Aktualizacja      | Z wyłącznością    |
| ----------------- | :----------- | :----------- | :---------- | :----------- |
| Udostępnione            | Nie konfliktów  | Nie konfliktów  | Konflikt    | Konflikt     |
| Aktualizacja            | Nie konfliktów  | Nie konfliktów  | Konflikt    | Konflikt     |
| Z wyłącznością         | Nie konfliktów  | Konflikt     | Konflikt    | Konflikt     |

Należy zauważyć, że limit czasu argumentem zaufanego API zbiorów jest używany dla wykrywanie zakleszczenia.
Na przykład dwie transakcje (T1 i T2) próbujesz czytanie i aktualizowanie K1.
Możliwe jest ich zakleszczenie, ponieważ obie o blokady udostępnione.
W tym przypadku limit czasu będzie jednej lub obu działań.

Należy zauważyć, że w powyższej sytuacji zakleszczenia doskonałe przykładem jak zapobiec zakleszczenia blokada aktualizacji.

## <a name="persistence-model"></a>Utrzymywanie modelu
Niezawodne Menedżer stanu i zaufanego zbiorów wykonaj utrzymywanie modelu, który jest nazywany dziennika i punktów kontrolnych.
To jest model miejsce, w którym każdej zmianie stanu jest zalogowany na dysku i stosowane tylko w pamięci.
Państwo wykonania jest zachowywane tylko od czasu do czasu (alias Punkt kontrolny).
Korzyścią jest to, że może są przekształcane w kolejnych zapisuje tylko dołączania na dysku zwiększyć wydajność.

Aby lepiej zrozumieć modelu dziennika i punktów kontrolnych, najpierw Przyjrzyjmy się scenariusz nieskończonej dysku.
Niezawodne Menedżer stanu dzienniki każdej operacji przed są replikowane.
Dzięki temu zaufanego zbioru stosowanie tylko te operacje w pamięci.
Ponieważ dzienniki są zachowywane nawet wtedy, gdy replice kończy się niepowodzeniem, należy ponownie uruchomić, zaufanego Menedżer stanu zawiera wystarczających informacji w jego dzienniki w celu ponownego odtworzenia wszystkie operacje, które są tracone replice.
Dysk jest nieograniczony, rekordy dziennika nigdy nie muszą zostać usunięte i zaufanego zbioru musi zarządzać tylko stan w pamięci.

Teraz Przyjrzyjmy się scenariusz skończonej dysku.
Jak zebrać rekordów dziennika, zaufanego Menedżer stanu zostaną brakować miejsca na dysku.
Zanim takiej sytuacji zaufanego Menedżer stanu musi obcinania dziennika, aby zwolnić miejsce na nowsze rekordów.
Go zażąda zaufanego zbiorów do punktu kontrolnego ich stanu w pamięci na dysk.
Odpowiada te niezawodne zbiory utrzymują stanu do tego punktu.
Po zaufanego zbiory ukończyć ich punktów kontrolnych, zaufanego Menedżer stanu dziennik można obciąć, w celu zwolnienia miejsca na dysku.
Dzięki temu podczas replice konieczne jest ponowne uruchomienie zaufanego zbiory będzie odzyskania ich użyciu stanu, a zaufanego Menedżer stanu będzie odzyskać i odtwarzanie wszystkich zmian stanu, które wystąpiły od punktu kontrolnego.

>[AZURE.NOTE] Inną wartość dodać punkt kontrolny jest zwiększa wydajność odzyskiwania w typowe przypadki.
Jest to spowodowane punktów kontrolnych zawierają tylko najnowsze wersje.

## <a name="recommendations"></a>Zalecenia

- Nie należy modyfikować obiekt typu niestandardowego zwracane przez operacji odczytu (np `TryPeekAsync` lub `TryGetValueAsync`). Kolekcje zaufanego, tak jak równoczesne zbiorów zwraca odwołanie do obiektów, a nie kopia.
- Wykonaj kopię głębokości zwracany obiekt typu niestandardowego przed modyfikowaniem go. Ponieważ typy wbudowane i struktur przebiegu według wartości, nie musisz wykonać kopię głębokości nad nimi.
- Nie należy używać `TimeSpan.MaxValue` dla limity czasu. Limity czasu należy wykrywanie zakleszczenia.
- Nie należy używać transakcji, po jej została przekazana, przerwane lub usunięte.
- Nie należy używać wyliczenie spoza zakresu transakcji, który został utworzony.
- Nie twórz transakcji w innej transakcji `using` instrukcji ponieważ mogą powodować zakleszczenia.
- Upewnij się, że usługi `IComparable<TKey>` wykonania jest poprawny. System przejmuje współzależności to przeznaczonego do scalenia punktów kontrolnych.
- Umożliwia blokadą aktualizacji podczas czytania elementu zamiaru aktualizowanie uniemożliwia klasy zakleszczenia.
- Warto rozważyć użycie kopii zapasowych i przywracanie funkcjonalności mają awarii.
- Unikaj mieszania operacje całość i operacje wiele jednostek (np. `GetCountAsync`, `CreateEnumerableAsync`) w tej samej transakcji z powodu poziomów izolacji różnych.

Oto kilka rzeczy, których warto pamiętać:

- Domyślny limit czasu wynosi 4 sekundy dla wszystkich niezawodne zbioru interfejsów API. Większość użytkowników nie należy zastąpić to.
- Token anulowania domyślny jest `CancellationToken.None` w wszystkich zaufanego interfejsów API zbiorów.
- Parametr typ klucza (*TKey*) zaufanego słownika poprawnie musi wykonać `GetHashCode()` i `Equals()`. Klucze muszą być niezmienne.
- Uzyskanie szybkiej zaufanego zbiorów, każdej usługi powinny mieć co najmniej docelowej i replice minimalny rozmiar 3.
- Operacje odczytu na pomocniczej może odczytać wersji, które nie są kworum Projekt zatwierdzony.
Oznacza to, że wersję dane odczytywane z jednym pomocniczej może FAŁSZ postępem.
Odczyt z głównego są zawsze oczywiście stabilny: może nie być FAŁSZ postępem.

## <a name="next-steps"></a>Następne kroki

- [Niezawodne usług szybki start](service-fabric-reliable-services-quick-start.md)
- [Praca z kolekcjami niezawodne](service-fabric-work-with-reliable-collections.md)
- [Niezawodne powiadomienia usług](service-fabric-reliable-services-notifications.md)
- [Wykonywanie kopii zapasowej niezawodne usług i ich przywracanie (odzyskiwanie)](service-fabric-reliable-services-backup-restore.md)
- [Stan niezawodny Menedżera konfiguracji](service-fabric-reliable-services-configuration.md)
- [Rozpoczynanie pracy z usługami interfejs API usługi tkaninie sieci Web](service-fabric-reliable-services-communication-webapi.md)
- [Zaawansowane zastosowania niezawodne modelu programowania usług](service-fabric-reliable-services-advanced-usage.md)
- [Dokumentacja dewelopera niezawodne zbierania danych](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
