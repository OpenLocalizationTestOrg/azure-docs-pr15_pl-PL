<properties
    pageTitle="Dodaj akcję kwerendy w aplikacjach logiczny | Microsoft Azure"
    description="Omówienie Akcja kwerendy do wykonywania operacji, takich jak tablicy filtrów."
    services=""
    documentationCenter=""
    authors="jeffhollan"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/20/2016"
   ms.author="jehollan"/>

# <a name="get-started-with-the-query-action"></a>Rozpoczynanie pracy z akcją kwerendy

Za pomocą akcji kwerendy, pracę z partii i tablice w celu przepływy pracy:

- Tworzenie zadania dla wszystkich wysoki priorytet rekordów z bazy danych.
- Zapisz wszystkie załączniki PDF do wiadomości e-mail do obiektów blob platformy Azure.

Aby rozpocząć pracę przy użyciu akcji kwerendy w aplikacji logiczny, zobacz [Tworzenie aplikacji logicznych](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="use-the-query-action"></a>Za pomocą akcji kwerendy

Akcja jest operację, która jest prowadzone przez przepływ pracy, który jest definiowana w aplikacji logicznych. [Dowiedz się więcej o akcje](connectors-overview.md).  

Akcja kwerendy ma obecnie jednej operacji o nazwie tablicy filtrów, która jest widoczna w projektancie. Pozwala na kwerendy tablicy i zwraca zestaw wyników filtrowania.

Poniżej opisano, jak można dodać go w aplikacji logiczny:

1. Kliknij przycisk **Nowy krok** .
2. Wybierz przycisk **Dodaj akcję**.
3. W polu wyszukiwania akcji wpisz **Filtr** do wyświetlania listy Akcja **tablicy filtrów** .

    ![Wybierz akcję, kwerendy](./media/connectors-native-query/using-action-1.png)

4. Wybierz pozycję tablicy do filtrowania. (Następujące zrzucie ekranu pokazano tablicę wyniki wyszukiwania Twitter).
5. Tworzenie warunku ma być obliczona dla każdego elementu. (Następujące zrzut ekranu filtry tweety od użytkowników, którzy mają więcej niż 100 obserwujących).

    ![Wykonywanie akcji kwerendy](./media/connectors-native-query/using-action-2.png)

    Akcja będzie wyjściowy nowej tablicy, które zawierają tylko wyników, które wymagania filtru.
6. Kliknij w lewym górnym rogu paska narzędzi, aby zapisać i aplikacji logiczny będzie zapisywanie i publikowanie (aktywowania).

## <a name="query-action"></a>Akcja kwerendy

Oto szczegóły dotyczące akcję, która obsługuje ten łącznik. Łącznik ma jedną możliwych działań.

|Akcja|Opis|
|---|---|
|Filtrowanie tablicy|Oblicza warunku dla każdego elementu w tablicy i zwraca wynik|

## <a name="action-details"></a>Szczegóły akcji

Akcja kwerendy jest zawarty w jednym możliwych działań. W poniższych tabelach opisano wymaganych i opcjonalnych pól wejściowych dla akcji i odpowiednie szczegóły dane wyjściowe, które są skojarzone z przy użyciu akcji.

### <a name="filter-array"></a>Tablicy filtrów
Poniżej przedstawiono wejściowych dla potrzeb dalszych działań, dzięki czemu żądania HTTP ruchu wychodzącego.
A * oznacza, że jest to pole wymagane.

|Nazwa wyświetlana|Nazwa właściwości|Opis|
|---|---|---|
|Z *|z|Tablica do filtrowania|
|Warunek *|gdzie|Warunek ma być obliczona dla każdego elementu|
<br>

### <a name="output-details"></a>Szczegóły wyników

Oto szczegóły wyników dla odpowiedzi HTTP.

|Nazwa właściwości|Typ danych|Opis|
|---|---|---|
|Filtrowane tablicy|Tablica|Tablicę zawierającą obiektu dla każdego wynik filtrowania|

## <a name="next-steps"></a>Następne kroki

Teraz wypróbować platformy i [Tworzenie aplikacji logicznych](../app-service-logic/app-service-logic-create-a-logic-app.md). Zapoznaj się dostępne łączniki w aplikacjach logiczny, sprawdzając naszej [listy interfejsów API](apis-list.md).
