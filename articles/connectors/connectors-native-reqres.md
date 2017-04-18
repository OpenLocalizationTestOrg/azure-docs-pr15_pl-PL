<properties
    pageTitle="Używanie akcji i odpowiadania na wezwania | Microsoft Azure"
    description="Omówienie wyzwalacza i odpowiadania na wezwania i akcji w aplikacji programu logiczny Azure"
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
   ms.date="07/18/2016"
   ms.author="jehollan"/>

# <a name="get-started-with-the-request-and-response-components"></a>Rozpoczynanie pracy z składniki i odpowiadania na wezwania

Ze składnikami i odpowiadania na wezwania w aplikacji logiczny można odpowiedzieć w czasie rzeczywistym zdarzeń.

Na przykład można wykonywać następujące czynności:

- Odpowiadanie na żądanie HTTP z danymi z lokalnej bazy danych za pomocą aplikacji logika.
- Wyzwalanie aplikacji logika ze zdarzenia webhook zewnętrznych.
- Nawiązywanie połączenia z aplikacji logiki z akcją i odpowiadania na wezwania z w innej aplikacji logiki.

Aby rozpocząć pracę przy użyciu akcji i odpowiadania na wezwania w aplikacji logika, zobacz [Tworzenie aplikacji logika](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="use-the-http-request-trigger"></a>Użyj wyzwalacza żądania HTTP

Wyzwalacz to zdarzenie, które można uruchomić przepływ pracy, który jest definiowana w aplikacji logicznych. [Dowiedz się więcej o wyzwalaczy](connectors-overview.md).

Oto przykładowa sekwencja konfigurowania żądania HTTP w Projektancie aplikacji logika.

1. Dodawanie wyzwalacza **żądanie — żądania HTTP po odebraniu** w aplikacji logicznych. Opcjonalnie można podać schematu JSON (za pomocą narzędzi, takich jak [JSONSchema.net](http://jsonschema.net)) dla zawartości żądania. Dzięki temu projektant do generowania tokenów dla właściwości w żądania HTTP.
2. Dodaj inną akcję, dzięki czemu możesz zapisać aplikację logicznych.
3. Po zapisaniu aplikacji logiczny, zostanie wyświetlony adresu URL żądania HTTP z karty wezwanie.
4. WPIS HTTP (służy narzędzie, takie jak [Postman](https://www.getpostman.com/)) do adresu URL uaktywnia aplikacji logicznych.

>[AZURE.NOTE] Jeśli nie zdefiniowano akcję odpowiedź `202 ACCEPTED` rozmówcy zwracana jest od razu odpowiedź. Za pomocą akcji odpowiedzi Dostosowywanie odpowiedź.

![Odpowiedź wyzwalacza](./media/connectors-native-reqres/using-trigger.png)

## <a name="use-the-http-response-action"></a>Za pomocą akcji odpowiedź HTTP

Akcja odpowiedź HTTP jest prawidłowa tylko w przypadku, gdy jest używany w przepływie pracy, który jest wyzwalane przez żądania HTTP. Jeśli nie zdefiniowano akcję odpowiedź `202 ACCEPTED` rozmówcy zwracana jest od razu odpowiedź.  Możesz dodać akcję odpowiedź na dowolnym z etapów w ramach przepływu pracy. Aplikacja logiczny tylko przechowuje przychodzące żądanie Otwórz minutę na odpowiedź.  Po jednej minucie, jeśli odpowiedź nie została wysłana z przepływu pracy (i akcja odpowiedź istnieje definicji) `504 GATEWAY TIMEOUT` są zwracane do rozmówcy.

Oto jak dodać akcję odpowiedź HTTP:

1. Kliknij przycisk **Nowy krok** .
2. Wybierz przycisk **Dodaj akcję**.
3. W polu wyszukiwania akcji wpisz **odpowiedź** na liście akcji odpowiedzi.

    ![Wybierz akcję, odpowiedź](./media/connectors-native-reqres/using-action-1.png)

4. Dodać parametry, które są wymagane do wiadomości odpowiedzi HTTP.

    ![Wykonywanie akcji odpowiedzi](./media/connectors-native-reqres/using-action-2.png)

5. Kliknij w lewym górnym rogu paska narzędzi, aby zapisać i aplikacji logiczny będzie zapisywanie i publikowanie (aktywowania).

## <a name="request-trigger"></a>Żądanie wyzwalacza

Oto szczegóły wyzwalacza obsługującego ten łącznik. Istnieje wyzwalacz pojedynczego żądania.

|Wyzwalacza|Opis|
|---|---|
|Żądanie|Występuje po otrzymaniu żądania HTTP|

## <a name="response-action"></a>Akcja odpowiedzi

Oto szczegóły dotyczące akcję, która obsługuje ten łącznik. Istnieje akcję pojedyncza odpowiedź, która może być używana tylko podczas towarzyszy wyzwalacza wezwanie.

|Akcja|Opis|
|---|---|
|Odpowiedź|Zwraca odpowiedź skorelowany żądanie HTTP|

### <a name="trigger-and-action-details"></a>Szczegóły wyzwalacza i akcji

W poniższych tabelach opisano wejściowych dla wyzwalacza i akcji, a odpowiadające im dane wyjściowe szczegóły.

#### <a name="request-trigger"></a>Żądanie wyzwalacza
Poniżej przedstawiono pole wyzwalacza z przychodzące żądanie HTTP.

|Nazwa wyświetlana|Nazwa właściwości|Opis|
|---|---|---|
|Schemat JSON|schematu|Schemat JSON żądania HTTP|
<br>

**Szczegóły wyników**

Oto szczegóły wyników dla żądania.

|Nazwa właściwości|Typ danych|Opis|
|---|---|---|
|Nagłówki|obiekt|Żądanie nagłówków|
|Treść|obiekt|Obiekt żądania|

#### <a name="response-action"></a>Akcja odpowiedzi

Poniżej przedstawiono wejściowych dla potrzeb dalszych działań odpowiedź HTTP. A * oznacza, że jest to pole wymagane.

|Nazwa wyświetlana|Nazwa właściwości|Opis|
|---|---|---|
|Kod stanu *|statusCode|Kod stanu HTTP|
|Nagłówki|nagłówki|Obiekt JSON wszystkie nagłówki odpowiedzi do uwzględnienia|
|Treść|Treść|Treść odpowiedzi|

## <a name="next-steps"></a>Następne kroki

Teraz wypróbować platformy i [Tworzenie aplikacji logicznych](../app-service-logic/app-service-logic-create-a-logic-app.md). Zapoznaj się dostępne łączniki w aplikacjach logiczny, sprawdzając naszej [listy interfejsów API](apis-list.md).
