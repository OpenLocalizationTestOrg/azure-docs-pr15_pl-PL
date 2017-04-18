
<properties
    pageTitle="Dodawanie HTTP + Swagger akcji w aplikacjach logiczny | Microsoft Azure"
    description="Omówienie HTTP + Swagger akcji i operacji"
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

# <a name="get-started-with-the-http--swagger-action"></a>Rozpoczynanie pracy z HTTP + Swagger akcji

Przy użyciu protokołu HTTP + Swagger akcji, możesz utworzyć najlepszych łącznik do dowolnego punktu końcowego pozostałych za pośrednictwem [Swagger dokumentu](https://swagger.io). Można również rozszerzanie aplikacji dla logiki połączenie dowolny punkt końcowy pozostałych najlepszych klasy logiczny projektanta aplikacji.

Aby rozpocząć pracę z HTTP + Swagger akcji w aplikacji logiczny, zobacz [Tworzenie nowej aplikacji logicznych](../app-service-logic/app-service-logic-create-a-logic-app.md).

---

## <a name="use-http--swagger-as-a-trigger-or-an-action"></a>Użyj protokołu HTTP + Swagger jako wyzwalacz lub akcji

HTTP + Swagger wyzwalacza i akcji, funkcja samo jak [Akcja HTTP](connectors-native-http.md) , ale działać lepiej projektu, wyświetlając kształt interfejsu API i wyników w Projektancie z [Swagger metadanych](https://swagger.io). Ponadto można użyć protokołu HTTP + Swagger jako wyzwalacz. Jeśli chcesz zaimplementować wyzwalacza ankieta, należy postępuj zgodnie ze wzorcem ankieta opisanej w [Tworzenie niestandardowego interfejsu API do użytku z logiki aplikacji](../app-service-logic/app-service-logic-create-api-app.md#polling-triggers).

[Dowiedz się więcej o logiki aplikacji wyzwalacze i akcje.](connectors-overview.md)

Oto przykładowy sposób do użycia HTTP + operacji Swagger jako akcji w przepływie pracy w aplikacji logicznych.

1. Kliknij przycisk **Nowy krok** .
2. Wybierz pozycję **Dodaj akcję**.
3. W polu wyszukiwania akcji wpisz **swagger** do listy HTTP + Swagger akcji.

    ![Wybierz pozycję HTTP + Swagger akcji](./media/connectors-native-http-swagger/using-action-1.png)

4. Wpisz adres URL dokumentu Swagger:
    - Aby pracować z projektanta aplikacji logiczny, adres URL musi być punktu końcowego HTTPS i CORS są włączone.
    - Jeśli dokument Swagger nie spełnia to wymaganie, umożliwia [Magazyn Azure z CORS włączone](#hosting-swagger-from-storage) przechowywanie dokumentu.
5. Kliknij przycisk **Dalej** do odczytu i renderowania z dokumentu Swagger.
6. Dodać parametry, które są wymagane do połączenia HTTP.

    ![Zakończenie działania HTTP](./media/connectors-native-http-swagger/using-action-2.png)

1. W lewym górnym rogu paska narzędzi i usługi logiki aplikacji zapisze obu kliknij przycisk **Zapisz** i opublikuj (aktywowania).

### <a name="host-swagger-from-azure-storage"></a>Swagger hosta z magazynu platformy Azure

Może być ma zostać utworzone odwołanie dokumentu Swagger nie jest obsługiwany lub który nie spełnia zabezpieczeń i origin krzyżowe wymagania dotyczące projektanta. Aby rozwiązać ten problem, można przechowywać dokument Swagger w magazynie Azure i Włącz CORS zostać utworzone odwołanie dokumentu.  

Poniżej przedstawiono czynności, aby utworzyć, konfigurować i przechowywanie dokumentów Swagger w magazynie Azure:

1. [Utwórz konto Azure miejsca do magazynowania z magazynem obiektów Blob platformy Azure](../storage/storage-create-storage-account.md). (W tym celu należy ustawić uprawnienia do **dostępie**).
2. Włączanie CORS na to. Za pomocą [tego skryptu programu PowerShell](https://github.com/logicappsio/EnableCORSAzureBlob/blob/master/EnableCORSAzureBlob.ps1) do konfigurowania tego ustawienia automatycznie.
3. Przekaż plik Swagger do obiektów blob. Można to zrobić z [Azure portal](https://portal.azure.com) lub narzędzie, takie jak [Eksplorator magazynu Azure](http://storageexplorer.com/).
1. Odwołuje się łącze HTTPS do dokumentu w magazynie obiektów Blob platformy Azure. (Łącze formacie `https://*storageAccountName*.blob.core.windows.net/*container*/*filename*`.)



## <a name="technical-details"></a>Szczegóły techniczne

Oto szczegóły wyzwalacze i akcje który ten HTTP + Swagger obsługuje łącznik.

## <a name="http--swagger-triggers"></a>HTTP + Swagger wyzwalaczy

Wyzwalacz to zdarzenie, które można uruchomić przepływ pracy, który jest definiowana w aplikacji logicznych. [Dowiedz się więcej na temat wyzwalaczy.](connectors-overview.md) HTTP + Swagger łącznik ma jeden wyzwalacz.

|Wyzwalacza|Opis|
|---|---|
|HTTP + Swagger|Nawiązywanie połączenia protokołu HTTP i zwrócić zawartości odpowiedzi|

## <a name="http--swagger-actions"></a>HTTP + akcje Swagger

Akcja jest operację, której zostało wykonane przez przepływ pracy, który jest definiowana w aplikacji logicznych. [Dowiedz się więcej na temat akcji.](connectors-overview.md) HTTP + Swagger łącznik ma jedną możliwych działań.

|Akcja|Opis|
|---|---|
|HTTP + Swagger|Nawiązywanie połączenia protokołu HTTP i zwrócić zawartości odpowiedzi|

### <a name="action-details"></a>Szczegóły akcji

HTTP + Swagger łącznik pochodzi z jednego możliwych działań. Poniżej znajdują się informacje o każdym akcje, ich wymaganych i opcjonalnych pól wprowadzania i odpowiadające im szczegóły dane wyjściowe, które są skojarzone z ich zastosowania.

#### <a name="http--swagger"></a>HTTP + Swagger

Wprowadź ruchu wychodzącego żądania HTTP pomoc Swagger metadanych.
Gwiazdka (*) oznacza, że wymagane pola.

|Nazwa wyświetlana|Nazwa właściwości|Opis|
|---|---|---|
|Metoda *|Metoda|Za pomocą zlecenie HTTP.|
|IDENTYFIKATOR URI *|Identyfikator URI|Identyfikator URI żądania HTTP.|
|Nagłówki|nagłówki|Obiekt JSON nagłówków HTTP, aby uwzględnić.|
|Treść|Treść|Żądania HTTP.|
|Uwierzytelnianie|uwierzytelnianie|Uwierzytelniania żądania. [Aby uzyskać więcej informacji, zobacz HTTP](./connectors-native-http.md#authentication).|

**Szczegóły wyników**

Odpowiedź HTTP

|Nazwa właściwości|Typ danych|Opis|
|---|---|---|
|Nagłówki|obiekt|Nagłówki odpowiedzi|
|Treść|obiekt|Obiekt odpowiedzi|
|Kod stanu|int|Kod stanu HTTP|

### <a name="http-responses"></a>Odpowiedzi HTTP

Podczas połączenia z różnych działań, może zostać wyświetlony określone odpowiedzi. Oto tabeli wokół odpowiednich odpowiedzi i opisy.

|Nazwa|Opis|
|---|---|
|200|Ok|
|202|Zaakceptowane|
|400|Nieprawidłowe żądanie|
|401|Brak autoryzacji|
|403|Dostęp zabroniony|
|404|Nie można odnaleźć|
|500|Wewnętrzny błąd serwera. Wystąpił nieznany błąd.|

---

## <a name="next-steps"></a>Następne kroki

Spróbuj platformy i [Tworzenie aplikacji logicznych](../app-service-logic/app-service-logic-create-a-logic-app.md) . Zapoznaj się dostępne łączniki w aplikacjach logiczny, sprawdzając naszą [listę interfejsów API](apis-list.md).
