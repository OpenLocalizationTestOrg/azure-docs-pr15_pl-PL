<properties
    pageTitle="Dodaj akcję HTTP w aplikacjach logiczny | Microsoft Azure"
    description="Omówienie akcji HTTP z właściwościami"
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
   ms.date="07/15/2016"
   ms.author="jehollan"/>

# <a name="get-started-with-the-http-action"></a>Rozpoczynanie pracy z akcją HTTP

Przy użyciu akcji HTTP można rozszerzanie przepływów pracy dla Twojej organizacji i powiadamianie dowolny punkt końcowy za pomocą protokołu HTTP.

Można:

- Tworzenie przepływów pracy aplikacji, które aktywować (wyzwalacza) po awarii witryny sieci Web, którą można zarządzać logiczny.
- Komunikowanie się do dowolnego punktu końcowego za pomocą protokołu HTTP, aby rozszerzyć przepływów pracy na inne usługi.

Aby rozpocząć pracę przy użyciu akcji HTTP w aplikacji logiczny, zobacz [Tworzenie aplikacji logicznych](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="use-the-http-trigger"></a>Użyj wyzwalacza HTTP

Wyzwalacz to zdarzenie, które można uruchomić przepływ pracy, który jest definiowana w aplikacji logicznych. [Dowiedz się więcej o wyzwalaczy](connectors-overview.md).

Oto przykładowa sekwencja konfigurowania wyzwalacza HTTP w Projektancie aplikacji logicznych.

1. Dodawanie wyzwalacza HTTP w aplikacji logicznych.
2. Wprowadź parametry punkt końcowy HTTP, który chcesz skorzystać.
3. Modyfikowanie interwału cyklu na jak często należy skorzystać.
4. Aplikacja logiczny teraz uruchamiana z dowolną zawartość, która jest zwracana podczas każdego sprawdzania.

![Wyzwalacza HTTP](./media/connectors-native-http/using-trigger.png)

### <a name="how-the-http-trigger-works"></a>Jak działa wyzwalacza HTTP

Wyzwalacza HTTP dzwoni do punktu końcowego HTTP na przedział cykliczne. Domyślnie odpowiedzi HTTP kod mniej niż 300 wyników w aplikacji dla logiki Uruchom. W widoku kodu, który oceni po połączenia HTTP do określenia, jeśli należy uruchomienie aplikacji logika można dodać warunku. Oto przykład wyzwalacza HTTP, który jest uruchamiana, gdy zwrócony kod stanu jest większe niż lub równe `400`.

```javascript
"Http":
{
    "conditions": [
        {
            "expression": "@greaterOrEquals(triggerOutputs()['statusCode'], 400)"
        }
    ],
    "inputs": {
        "method": "GET",
        "uri": "https://blogs.msdn.microsoft.com/logicapps/",
        "headers": {
            "accept-language": "en"
        }
    },
    "recurrence": {
        "frequency": "Second",
        "interval": 15
    },
    "type": "Http"
}
```

Szczegółowe informacje o parametrach wyzwalacza HTTP są dostępne w [witrynie MSDN](https://msdn.microsoft.com/library/azure/mt643939.aspx#HTTP-trigger).

## <a name="use-the-http-action"></a>Za pomocą akcji HTTP

Akcja jest operację, która jest prowadzone przez przepływ pracy, który jest definiowana w aplikacji logicznych. [Dowiedz się więcej o akcje](connectors-overview.md).

1. Kliknij przycisk **Nowy krok** .
2. Wybierz przycisk **Dodaj akcję**.
3. W polu wyszukiwania akcji wpisz **http** , aby wyświetlić listę akcji HTTP.

    ![Wybierz akcję, HTTP](./media/connectors-native-http/using-action-1.png)

4. Dodać parametry, które są wymagane na potrzeby połączeń HTTP.

    ![Wykonywanie akcji HTTP](./media/connectors-native-http/using-action-2.png)

5. Kliknij lewym górnym rogu paska narzędzi, aby zapisać. Aplikacji logiki będzie zapisywanie i publikowanie (aktywowania).

## <a name="http-trigger"></a>Wyzwalacza HTTP

Oto szczegóły wyzwalacza obsługującego ten łącznik. Łącznik HTTP ma jeden wyzwalacz.

|Wyzwalacza|Opis|
|---|---|
|HTTP|Umożliwia połączenie HTTP i zwraca zawartość odpowiedzi.|

## <a name="http-action"></a>Akcja HTTP

Oto szczegóły dotyczące akcję, która obsługuje ten łącznik. Łącznik HTTP ma jedną możliwych działań.

|Akcja|Opis|
|---|---|
|HTTP|Umożliwia połączenie HTTP i zwraca zawartość odpowiedzi.|

## <a name="http-details"></a>Szczegóły HTTP

W poniższych tabelach opisano wymaganych i opcjonalnych pól wejściowych dla akcji i odpowiednie szczegóły dane wyjściowe, które są skojarzone z przy użyciu akcji.


#### <a name="http-request"></a>Żądanie HTTP
Poniżej przedstawiono wejściowych dla potrzeb dalszych działań, dzięki czemu żądania HTTP ruchu wychodzącego.
A * oznacza, że jest to pole wymagane.

|Nazwa wyświetlana|Nazwa właściwości|Opis|
|---|---|---|
|Metoda *|Metoda|Zlecenie HTTP, aby użyć|
|IDENTYFIKATOR URI *|Identyfikator URI|Identyfikator URI żądania HTTP|
|Nagłówki|nagłówki|Obiekt JSON HTTP nagłówków do uwzględnienia|
|Treść|Treść|Żądania HTTP|
|Uwierzytelnianie|uwierzytelnianie|Szczegóły w sekcji [Uwierzytelnianie](#authentication)|
<br>

#### <a name="output-details"></a>Szczegóły wyników

Oto szczegóły wyników dla odpowiedzi HTTP.

|Nazwa właściwości|Typ danych|Opis|
|---|---|---|
|Nagłówki|obiekt|Nagłówki odpowiedzi|
|Treść|obiekt|Obiekt odpowiedzi|
|Kod stanu|int|Kod stanu HTTP|

## <a name="authentication"></a>Uwierzytelnianie

Funkcja aplikacje logiczny Azure aplikacji usługi umożliwia używają różnych typów uwierzytelniania protokołu HTTP punkty końcowe. Za pomocą tego uwierzytelniania przy użyciu **protokołu HTTP**, **[HTTP + Swagger](./connectors-native-http-swagger.md)**i **[HTTP Webhook](./connectors-native-webhook.md)** łączników. Można konfigurować są następujące typy uwierzytelniania:

* [Uwierzytelnianie podstawowe](#basic-authentication)
* [Uwierzytelnianie klienta na podstawie certyfikatu](#client-certificate-authentication)
* [Azure uwierzytelniania OAuth usługi Active Directory (Azure AD)](#azure-active-directory-oauth-authentication)

#### <a name="basic-authentication"></a>Uwierzytelnianie podstawowe

Następujący obiekt uwierzytelniania jest wymagany dla uwierzytelniania podstawowego.
A * oznacza, że jest to pole wymagane.

|Nazwa właściwości|Typ danych|Opis|
|---|---|---|
|Typ *|Typ|Typ uwierzytelniania (musi być `Basic` dla uwierzytelniania podstawowego)|
|Nazwa użytkownika *|Nazwa użytkownika|Nazwa użytkownika do uwierzytelnienia|
|Hasło *|hasło|Hasło do uwierzytelnienia|

>[AZURE.TIP] Jeśli chcesz używać hasło, którego nie można pobrać z definicji, użyj `securestring` parametru i `@parameters()` [Funkcja definicji przepływu pracy](http://aka.ms/logicappdocs).

Dlatego należy utworzyć w polu uwierzytelnianie obiektu w następujący sposób:

```javascript
{
    "type": "Basic",
    "username": "user",
    "password": "test"
}
```

#### <a name="client-certificate-authentication"></a>Uwierzytelnianie klienta na podstawie certyfikatu

Następujący obiekt uwierzytelniania jest wymagany dla uwierzytelnianie klienta na podstawie certyfikatu. A * oznacza, że jest to pole wymagane.

|Nazwa właściwości|Typ danych|Opis|
|---|---|---|
|Typ *|Typ|Typ uwierzytelniania (musi być `ClientCertificate` certyfikatów SSL klienta)|
|PFX *|PFX|Zakodowany Base64 zawartość pliku wymiany informacji osobistych (PFX)|
|Hasło *|hasło|Hasło, aby uzyskać dostęp do pliku PFX|

>[AZURE.TIP] Możesz użyć `securestring` parametru i `@parameters()` [Funkcja definicji przepływu pracy](http://aka.ms/logicappdocs) za pomocą parametru, którego nie będzie można odczytać w definicji po zapisaniu aplikacji logicznych.

Na przykład:

```javascript
{
    "type": "ClientCertificate",
    "pfx": "aGVsbG8g...d29ybGQ=",
    "password": "@parameters('myPassword')"
}
```

#### <a name="azure-ad-oauth-authentication"></a>Azure AD OAuth uwierzytelniania

Następujący obiekt uwierzytelniania jest wymagany dla uwierzytelniania Azure AD OAuth. A * oznacza, że jest to pole wymagane.

|Nazwa właściwości|Typ danych|Opis|
|---|---|---|
|Typ *|Typ|Typ uwierzytelniania (musi być `ActiveDirectoryOAuth` dla Azure AD OAuth)|
|Dzierżawy *|dzierżawy|Identyfikator dzierżawy dla dzierżawy Azure AD|
|Grupy odbiorców *|grupy odbiorców|Ustaw`https://management.core.windows.net/`|
|Klient identyfikator *|clientId|Identyfikator klienta na podstawie Azure AD|
|Tajny *|tajny|Tajny klienta, który żąda tokenu|

>[AZURE.TIP] Możesz użyć `securestring` parametru i `@parameters()` [Funkcja definicji przepływu pracy](http://aka.ms/logicappdocs) , należy użyć parametru, którego nie będzie można odczytać w definicji po zapisaniu.

Na przykład:

```javascript
{
    "type": "ActiveDirectoryOAuth",
    "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db47",
    "audience": "https://management.core.windows.net/",
    "clientId": "34750e0b-72d1-4e4f-bbbe-664f6d04d411",
    "secret": "hcqgkYc9ebgNLA5c+GDg7xl9ZJMD88TmTJiJBgZ8dFo="
}
```

## <a name="next-steps"></a>Następne kroki

Teraz wypróbować platformy i [Tworzenie aplikacji logicznych](../app-service-logic/app-service-logic-create-a-logic-app.md). Zapoznaj się dostępne łączniki w aplikacjach logiki, sprawdzając naszej [listy interfejsów API](apis-list.md).
