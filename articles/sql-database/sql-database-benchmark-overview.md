<properties
    pageTitle="Omówienie testu bazy danych SQL Azure"
    description="W tym temacie opisano testowych bazy danych SQL Azure używane w pomiaru wydajności bazy danych SQL Azure."
    services="sql-database"
    documentationCenter="na"
    authors="CarlRabeler"
    manager="jhubbard"
    editor="monicar" />


<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="06/21/2016"
    ms.author="carlrab" />

# <a name="azure-sql-database-benchmark-overview"></a>Omówienie testu bazy danych SQL Azure

## <a name="overview"></a>Omówienie
Microsoft Azure SQL Database oferuje trzy [warstwy usługi](sql-database-service-tiers.md) z wieloma poziomami wydajności. Każdy poziom wydajności udostępnia rosnącymi zasobów lub "moc" formantem rosnącej złożoności wyższej przepustowości.

Ważne można było określenie sposobu rosnącymi możliwości poszczególnych poziomów wydajności przekłada się na wydajność lepszą bazy danych jest. W tym Microsoft opracowało testu bazy danych SQL Azure (ASDB). Wzorzec wykonuje podstawowe operacje w wszystkich obciążenia OLTP. Firma Microsoft Zmierz przepustowość osiągnięte uruchomione na poszczególnych poziomach wydajności baz danych.

Zasoby i możliwości dla każdego poziomu warstwa i wydajności usługi są wyrażona za pomocą [Jednostki transakcji bazy danych (DTUs)](sql-database-technical-overview.md#understand-dtus). DTUs umożliwiają opisano możliwości względne poziom wydajności, na podstawie przejścia miary procesora i pamięci, czytać i zapisywać stawki oferowanych przez każdego poziomu wydajności. Podwajanie ocena DTU bazy danych jest równa Podwajanie power bazy danych. Wzorzec umożliwia ocenianie wpływ na wydajność bazy danych siły rosnącymi oferowanych przez każdy poziom wydajności, wykorzystując operacje rzeczywistej bazy danych podczas skalowania rozmiar bazy danych, liczby użytkowników i transakcji stawki proporcjonalnie do zasobów do bazy danych.

Przez wyrażenie przepustowość warstwie podstawowej usługi przy użyciu transakcje za godzinę, warstwie standardowa usługa przy użyciu transakcji na minutę i warstwa usług Premium przy użyciu transakcji na sekundę, go ułatwia szybkie tworzenie relacji między wydajność potencjalne każdej warstwy usługi wymagania aplikacji.

## <a name="correlating-benchmark-results-to-real-world-database-performance"></a>Korelacji wyniki testu do rzeczywistych wydajności bazy danych
Aby dowiedzieć się, że ASDB, takich jak wszystkich stawek, jest przedstawiciel oraz orientacyjnych tylko ważne jest. Wskaźników transakcji z aplikacją testu nie będą takie same jak, które mogą być osiągnięte z innymi aplikacjami. Wzorzec obejmuje zbiór różnych transakcji, typy uruchomienia ze schematem zawierający zakres tabel i typy danych. Podczas testu wykonuje podstawowych operacji, które są wspólne dla wszystkich obciążenia OLTP, nie reprezentuje dowolnego określonego rodzaju bazy danych lub aplikacji. Cel stawek jest zapewnienie odpowiednim przewodnika do przeprowadzenia względne bazę danych, którą można się spodziewać podczas skalowania w górę lub dół między poziomami wydajności. W rzeczywistości bazy danych o różnych rozmiarach i złożoność, wystąpić różne kombinacje obciążenia i będą odpowiadać na różne sposoby. Na przykład aplikacja intensywną Jo może trafienie progi Jo wcześniej lub aplikacją obciążenie Procesora może trafienie limity CPU wcześniej. Nie jest gwarantowana, który dowolnej konkretnej bazy danych zostanie skalować w taki sam sposób jak testowych w obszarze zwiększenie ładowania.

Wzorzec i jej metodologii opisano bardziej szczegółowo poniżej.

## <a name="benchmark-summary"></a>Podsumowanie stawek
ASDB środków wydajność operacje podstawowe bazy danych, najczęściej występujące w trybie online transakcji przetwarzania obciążenia (OLTP). Mimo że testu jest wyposażone w chmurze obliczeniowych w zdanie, schemat bazy danych, danych populacji, a transakcje zostały zaprojektowane jako ogólnie przedstawiciela podstawowych elementów, które są najczęściej używane w obciążenia OLTP.

## <a name="schema"></a>Schematu
Schemat jest oparty na za mało odmiany i złożoność w celu obsługi szeroką gamę operacji. Uruchomione testy w bazie danych, które składają się z sześciu tabel. Tabele podzielić na trzy kategorie: stałym rozmiarze, skalowanie i wzrostu. Istnieją dwie tabele stałym rozmiarze; trzy tabele skalowania; i jednej tabeli wzrostu. Tabele stałym rozmiarze mają stała liczba wierszy. Tabele skalowania mają kardynalności, jest proporcjonalny do wydajności bazy danych, ale nie powoduje zmiany podczas testów wydajności. Jak skalowania tabelę na obciążenie wstępne, a następnie zmiany kardynalności w trakcie działania stawek, jak wiersze są wstawianiu i usuwaniu rozmiarem wzrostu tabeli.

Schemat zawiera różnych typów danych, w tym liczbę całkowitą z zakresu liczbowych, znaków i Data/Godzina. Schemat zawiera kluczami podstawowymi i pomocniczymi, ale nie kluczy obcych — oznacza to, że istnieje bez ograniczeń więzów integralności między tabelami.

Program Generowanie danych generuje dane dotyczące początkowej bazy danych. Danych liczbowych i liczbą całkowitą, jest generowany z różnych strategii. W niektórych przypadkach wartości są rozdzielane losowo dla zakresu. W innych przypadkach zbiór wartości jest losowo cieniowania do zapewnienia utrzymania określonego rozkładu. Pola tekstowe są generowane przez ważonej listę wyrazów da rzeczywistych danych wyglądzie.

Rozmiarem bazy danych oparty na "współczynnik skalowania". Skala (skrót rz) określa kardynalności skalowanie i rosnących tabel. Opisane poniżej w sekcji Użytkownicy i Pacing, rozmiar bazy danych, liczby użytkowników i maksymalną wydajność wszystkich przeskalować proporcjonalnie względem siebie.

## <a name="transactions"></a>Transakcje
Obciążenie pracą składa się z dziewięciu typów transakcji, jak pokazano w poniższej tabeli. Każdej transakcji jest zaprojektowane w celu wyróżnienia określonego zestawu właściwości w bazie danych systemu i aparat sprzęt, wysoki kontrast z innych transakcji. Ta metoda ułatwia ocenić wpływ różnych składników do ogólnej wydajności. Na przykład "Odczytu intensywnie" transakcji daje znaczną liczbę operacji odczytu z dysku.

| Typ transakcji | Opis |
|---|---|
| Przeczytaj Lite | WYBÓR; w pamięci. tylko do odczytu |
| Średni odczytu | WYBÓR; przede wszystkim w pamięci; tylko do odczytu |
| Ciężkich odczytu | WYBÓR; przeważnie nie w pamięci; tylko do odczytu |
| Aktualizacja Lite | AKTUALIZACJI. w pamięci. odczytu i zapisu |
| Aktualizowanie ciężkich | AKTUALIZACJI. przeważnie nie w pamięci; odczytu i zapisu |
| Wstawianie Lite | WSTAWIANIE; w pamięci. odczytu i zapisu |
| Wstawianie ciężkich | WSTAWIANIE; przeważnie nie w pamięci; odczytu i zapisu |
| Usuwanie | USUWANIE; w pamięci i nie w pamięci; odczytu i zapisu |
| Procesor ciężkich | WYBÓR; w pamięci. stosunkowo dużym obciążeniu Procesora; tylko do odczytu |

## <a name="workload-mix"></a>Obciążenie pracą mix
Transakcje zostaną zaznaczone losowo rozkładu ważoną z następujące mix ogólne. Ogólna mix występują w stosunku do odczytu/zapisu około 2:1.

| Typ transakcji | % Mix |
|---|---|
| Przeczytaj Lite | 35 |
| Średni odczytu | 20 |
| Ciężkich odczytu | 5 |
| Aktualizacja Lite | 20 |
| Aktualizowanie ciężkich | 3 |
| Wstawianie Lite | 3 |
| Wstawianie ciężkich | 2 |
| Usuwanie | 2 |
| Procesor ciężkich | 10 |

## <a name="users-and-pacing"></a>Użytkownicy i nadawanie tempa
Obciążenie pracą testu poruszania się z narzędziem przesyła transakcji w zestawie połączeń symulować kilku użytkowników jednocześnie. Wszystkie połączenia i transakcje są generowane komputera, dla uproszczenia możemy odwołują się do tych połączeń jako "Użytkownicy". Mimo że każdy użytkownik działa niezależnie od innych użytkowników, wszyscy użytkownicy wykonywać tego samego cyklu czynności, jak pokazano poniżej:

1. Nawiązują połączenie z bazą danych.
2. Powtórz informowany opuszczanie:
    - Wybierz pozycję transakcji losowo (z ważonej rozkładu).
    - Wykonywanie wybranej transakcji i zmierzyć ich czas odpowiedzi.
    - Poczekaj, aż pacing opóźnienie.
3. Zamknij połączenie z bazą danych.
4. Zakończ.

Pacing opóźnienie (w kroku 2c) jest zaznaczone losowo, ale z rozkładem, który zawiera średnią drugiego 1.0. A więc każdego użytkownika na ogół wygenerować co najwyżej jednej transakcji na sekundę.

## <a name="scaling-rules"></a>Skalowanie reguł
Liczba użytkowników zależy od rozmiaru bazy danych (w jednostkach współczynnik skali). Ma jednego użytkownika w przypadku każdej pięciu jednostek współczynnik skalowania. Ze względu na pacing opóźnienie jednego użytkownika mogą powodować zwrócenie co najwyżej jednej transakcji na sekundę, średnio.

Na przykład skala czynniki 500 (rz = 500) bazy danych ma 100 użytkowników i można uzyskać maksymalna liczba 100 TPS. Do kierowania wyższą TPS kurs wymaga więcej użytkowników i większej bazy danych.

W poniższej tabeli pokazano liczbę użytkowników faktycznie poniesione dla każdego poziomu warstwa i wydajności usługi.

| Warstwa usług (poziom wydajności) | Użytkownicy | Rozmiar bazy danych |
|---|---|---|
| Podstawowe | 5 | 720 MB |
| Standardowe (S0) | 10 | 1 GB |
| Standardowe (S1) | 20 | 2.1 GB |
| Standardowe (S2) | 50 | 7.1 GB |
| Premium (P1) | 100 | 14 GB |
| Premium (P2) | 200 | 28 GB |
| Premium (P6-P3) | 800 | 114 GB |

## <a name="measurement-duration"></a>Czas trwania miar
Prawidłowe wzorca uruchamianie wymaga czas trwania miary stałej liczbie co najmniej jedną godzinę.

## <a name="metrics"></a>Metryki
Kluczowe wskaźniki w uruchomionym teście jest przepustowość i odpowiedzi.

- Przepustowość jest miary wydajności podstawowe w uruchomionym teście. Przepustowość jest zgłaszane w transakcji według jednostek z time, licząc wszystkie typy transakcji.
- Czas odpowiedzi jest miarą przewidywalności wydajności. Ograniczenie czasu odpowiedzi zależy od rodzaju usług z wyższych klas usługę, której bardziej rygorystyczne wymagania dotyczące czasu reakcji, tak jak pokazano poniżej.

| Klasa usług  | Miara przepustowość | Wymagania dotyczące czasu odpowiedzi |
|---|---|---|
| Premium | Transakcje na sekundę | 95. percentyl u 0,5 sekundy |
| Standardowe | Transakcje na minutę | percentyl 90 w sekundach 1.0 |
| Podstawowe | Transakcje na godzinę | 80. percentyl w sekundach 2.0 |

## <a name="conclusion"></a>Wnioski
Wzorzec bazy danych SQL Azure środków względne wyniki bazy danych SQL Azure działa w zakresie warstwy dostępne usługi i poziomy wydajności. Wzorzec wykonuje operacje podstawowe bazy danych, najczęściej występujące w trybie online transakcji przetwarzania obciążenia (OLTP). Za pomocą pomiaru wydajności rzeczywiste, testu zawiera bardziej zrozumiały ocena wpływu po przepustowość zmieniając poziom wydajności, nie jest możliwe, wyszczególniając wystarczy zasobów udostępnianych przez każdego poziomu, takich jak szybkość Procesora, rozmiar pamięci i operacji i/o na SEKUNDĘ. W przyszłości będziemy rozwijać testów wydajności, aby rozszerzyć zakres lub rozwinąć danych zawartych.

## <a name="resources"></a>Zasoby
[Wprowadzenie do bazy danych SQL](sql-database-technical-overview.md)

[Warstwy usługi i poziomy wydajności](sql-database-service-tiers.md)

[Wskazówki dotyczące wydajności dla pojedynczego baz danych](sql-database-performance-guidance.md)
