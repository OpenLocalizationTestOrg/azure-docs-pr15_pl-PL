<properties
    pageTitle="Dodawanie łącznika Dynamics CRM Online do aplikacji logiki | Microsoft Azure"
    description="Tworzenie aplikacji logika z usługą Azure aplikacji. Dostawcy Dynamics CRM Online połączenia zawiera interfejs API do pracy z obiektami na Dynamics CRM Online."
    services="logic-apps"    
    documentationCenter=""     
    authors="MandiOhlinger"    
    manager="erikre"    
    editor="" 
    tags="connectors" />

<tags
ms.service="logic-apps"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="08/15/2016"
ms.author="mandia"/>

# <a name="get-started-with-the-dynamics-crm-online-connector"></a>Wprowadzenie do programu Dynamics CRM Online łącznika
Łączenie programu Dynamics CRM Online, aby utworzyć nowy rekord, aktualizacji elementu i nie tylko. Za pomocą CRM Online możesz wykonać następujące czynności:

- Tworzenie usługi przepływu biznesowych na podstawie danych, które można uzyskać od CRM Online. 
- Użyj akcji, które usuwanie rekordu, Uzyskaj podmioty i nie tylko. Te akcje odpowiedzi, a następnie wprowadź dane wyjściowe dostępne dla innych akcji. Na przykład jeśli element zostanie zaktualizowany w CRM, można wysłać wiadomości e-mail za pomocą usługi Office 365.

W tym temacie pokazano, jak używać łącznik Dynamics CRM Online w aplikacji logiczny, a także listy wyzwalacze i akcje.

>[AZURE.NOTE] Tą wersją artykułu dotyczy aplikacji logiki ogólnodostępną (GA).

Aby dowiedzieć się więcej na temat aplikacji logiczny, zobacz [Co to są aplikacje logiki](../app-service-logic/app-service-logic-what-are-logic-apps.md) i [Tworzenie aplikacji logiczny](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="connect-to-dynamics-crm-online"></a>Nawiązywanie połączenia z Dynamics CRM Online

Przed aplikacji logika uzyskać dostęp do dowolnej usługi, należy najpierw utworzyć *połączenie* z usługą. Połączenia umożliwia łączność aplikacji logiki i innej usługi. Na przykład aby połączyć się Dynamics, należy najpierw Dynamics CRM Online *połączenia*. Aby utworzyć połączenie, wprowadź poświadczenia, zwykle używanych do uzyskania dostępu do usługi, którą chcesz nawiązać połączenie. Dlatego z dynamiki, wprowadź poświadczenia konta Dynamics CRM Online, aby utworzyć połączenie.


### <a name="create-the-connection"></a>Utwórz połączenie

>[AZURE.INCLUDE [Steps to create a connection to Dynamics CRM Online Connection Provider](../../includes/connectors-create-api-crmonline.md)]

## <a name="use-a-trigger"></a>Użyj wyzwalacza

Wyzwalacz to zdarzenie, które może służyć do uruchamiania przepływu pracy, zdefiniowane w aplikacji logicznych. Wyzwalacze "ankieta" usługi interwału i częstotliwość. [Dowiedz się więcej o wyzwalaczy](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

1. W aplikacji logiczny, wpisz "dynamics" w celu uzyskania listy wyzwalacze:  

    ![](./media/connectors-create-api-crmonline/dynamics-triggers.png)

2. Wybierz pozycję **Dynamics CRM Online — po utworzeniu rekordu**. Jeśli połączenie już istnieje, następnie wybierz organizacji i jednostki z listy rozwijanej.

    ![](./media/connectors-create-api-crmonline/select-organization.png)

    Jeśli zostanie wyświetlony monit, aby się zalogować, wprowadź znak w szczegóły, aby utworzyć połączenie. [Utwórz połączenie](connectors-create-api-crmonline.md#create-the-connection) w tym temacie przedstawiono kroki. 

    > [AZURE.NOTE] W tym przykładzie aplikacji logika uruchamia po utworzeniu rekordu. Aby wyświetlić wyniki tego wyzwalacza, Dodaj inną akcję, która wysyła wiadomość e-mail. Na przykład dodać akcję usługi Office 365, *Wysyłanie wiadomości e-mail* wiadomości e-mail, możesz podczas dodawania nowego rekordu. 

3. Wybierz przycisk **Edytuj** i ustaw wartości **częstotliwości** i **Interwał** . Na przykład jeśli chcesz wyzwalacza ankieta co 15 minut, następnie ustawić **Częstotliwość** **minuty**i ustawianie **interwału** **15**. 

    ![](./media/connectors-create-api-crmonline/edit-properties.png)

4. **Zapisz** zmiany (lewym górnym rogu paska narzędzi). Logika aplikacji są zapisywane i automatycznie włączona.


## <a name="use-an-action"></a>Za pomocą akcji

Akcja jest czynność wykonaną przez przepływ pracy zdefiniowane w aplikacji logicznych. [Dowiedz się więcej o akcje](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

1. Kliknij znak plus. Zobacz ustawienia kilku opcji: **Dodaj akcję**, **Dodaj warunek**lub jeden z **większą liczbą** opcji.

    ![](./media/connectors-create-api-crmonline/add-action.png)

2. Wybierz przycisk **Dodaj akcję**.

3. W polu tekstowym wpisz "dynamics" w celu uzyskania listy dostępnych akcji.

    ![](./media/connectors-create-api-crmonline/dynamics-actions.png)

4. W naszym przykładzie wybierz pozycję **Dynamics CRM Online — aktualizacji rekordu**. Jeśli połączenie już istnieje, a następnie wybierz pozycję **Nazwa organizacji**, **Nazwa jednostki**i inne właściwości:  

    ![](./media/connectors-create-api-crmonline/sample-action.png)

    Jeśli zostanie wyświetlony monit o informacje o połączeniu, wprowadź szczegółowe informacje, aby utworzyć połączenie. [Utwórz połączenie](connectors-create-api-crmonline.md#create-the-connection) w tym temacie opisano następujące właściwości. 

    > [AZURE.NOTE] W tym przykładzie możemy aktualizowanie istniejącego rekordu w CRM Online. Dane wyjściowe następny wyzwalacz służy do zaktualizowania rekordu. Na przykład dodać wyzwalacza programu SharePoint *modyfikacji istniejącego elementu* . Następnie dodaj CRM Online *aktualizacji rekordu* akcję, która korzysta z pól programu SharePoint w celu zaktualizowania istniejącego rekordu w CRM Online. 

5. **Zapisz** zmiany (lewym górnym rogu paska narzędzi). Logika aplikacji są zapisywane i automatycznie włączona.


## <a name="technical-details"></a>Szczegóły techniczne

## <a name="triggers"></a>Wyzwalaczy

|Wyzwalacza | Opis|
|--- | ---|
|[Po utworzeniu rekordu](connectors-create-api-crmonline.md#when-a-record-is-created)|Uaktywnia przepływu obiekt jest tworzony w programie CRM.|
|[Po zaktualizowaniu rekordu](connectors-create-api-crmonline.md#when-a-record-is-updated)|Uaktywnia przepływu modyfikacji obiektu w programie CRM.|
|[Po usunięciu rekordu](connectors-create-api-crmonline.md#when-a-record-is-deleted)|Uaktywnia przepływu obiektu zostanie usunięty w CRM.|


## <a name="actions"></a>Akcje

|Akcja|Opis|
|--- | ---|
|[Lista rekordów](connectors-create-api-crmonline.md#list-records)|Tej operacji są pobierane rekordy obiektu.|
|[Utwórz nowy rekord](connectors-create-api-crmonline.md#create-a-new-record)|Operacja tworzy nowy rekord jednostki.|
|[Pobieranie rekordu](connectors-create-api-crmonline.md#get-record)|Operacja otrzymuje określony rekord obiektu.|
|[Usuwanie rekordu](connectors-create-api-crmonline.md#delete-a-record)|Operacja usuwa rekord z kolekcji jednostek.|
|[Zaktualizuj rekord](connectors-create-api-crmonline.md#update-a-record)|Operacja aktualizacji istniejącego rekordu obiektu.|

### <a name="trigger-and-action-details"></a>Szczegóły wyzwalacza i akcji

W tej sekcji Zobacz szczegółowe informacje o każdej wyzwalacza i działań, w tym wszystkie wymagane lub opcjonalne właściwości wprowadzania i dowolne odpowiednie dane wyjściowe skojarzone z łącznik.

#### <a name="when-a-record-is-created"></a>Po utworzeniu rekordu
Uaktywnia przepływu obiekt jest tworzony w programie CRM. 

|Nazwa właściwości| Nazwa wyświetlana|Opis|
| ---|---|---|
|zestaw danych *|Nazwa organizacji|Nazwa organizacji CRM, takich jak Contoso|
|Tabela *|Nazwa obiektu|Nazwa obiektu|
|$skip|Pomiń zliczanie|Liczba wpisów, aby pominąć (domyślny = 0)|
|$top|Uzyskiwanie maksymalna liczba|Maksymalna liczba wpisów uzyskanie (domyślny = 256)|
|$filter|Filtrowanie kwerendy|Kwerendę filtru ODATA, aby ograniczyć wpisy zwrócone|
|$orderby|Uporządkuj według|Kwerenda orderBy ODATA do określania kolejności pozycji|

Gwiazdka (*) oznacza, że są one wymagane.

##### <a name="output-details"></a>Szczegóły wyników
ItemsList

| Nazwa właściwości | Typ danych |
|---|---|
|wartość|Tablica|


#### <a name="when-a-record-is-updated"></a>Po zaktualizowaniu rekordu
Uaktywnia przepływu modyfikacji obiektu w programie CRM. 

|Nazwa właściwości| Nazwa wyświetlana|Opis|
| ---|---|---|
|zestaw danych *|Nazwa organizacji|Nazwa organizacji CRM, takich jak Contoso|
|Tabela *|Nazwa obiektu|Nazwa obiektu|
|$skip|Pomiń zliczanie|Liczba wpisów, aby pominąć (domyślny = 0)|
|$top|Uzyskiwanie maksymalna liczba|Maksymalna liczba wpisów uzyskanie (domyślny = 256)|
|$filter|Filtrowanie kwerendy|Kwerendę filtru ODATA, aby ograniczyć wpisy zwrócone|
|$orderby|Uporządkuj według|Kwerenda orderBy ODATA do określania kolejności pozycji|

Gwiazdka (*) oznacza, że są one wymagane.

##### <a name="output-details"></a>Szczegóły wyników
ItemsList

| Nazwa właściwości | Typ danych |
|---|---|
|wartość|Tablica|


#### <a name="when-a-record-is-deleted"></a>Po usunięciu rekordu
Uaktywnia przepływu obiektu zostanie usunięty w CRM. 

|Nazwa właściwości| Nazwa wyświetlana|Opis|
| ---|---|---|
|zestaw danych *|Nazwa organizacji|Nazwa organizacji CRM, takich jak Contoso|
|Tabela *|Nazwa obiektu|Nazwa obiektu|
|$skip|Pomiń zliczanie|Liczba wpisów, aby pominąć (domyślny = 0)|
|$top|Uzyskiwanie maksymalna liczba|Maksymalna liczba wpisów uzyskanie (domyślny = 256)|
|$filter|Filtrowanie kwerendy|Kwerendę filtru ODATA, aby ograniczyć wpisy zwrócone|
|$orderby|Uporządkuj według|Kwerenda orderBy ODATA do określania kolejności pozycji|

Gwiazdka (*) oznacza, że są one wymagane.

##### <a name="output-details"></a>Szczegóły wyników
ItemsList

| Nazwa właściwości | Typ danych |
|---|---|
|wartość|Tablica|


#### <a name="list-records"></a>Lista rekordów
Tej operacji są pobierane rekordy obiektu. 

|Nazwa właściwości| Nazwa wyświetlana|Opis|
| ---|---|---|
|zestaw danych *|Nazwa organizacji|Nazwa organizacji CRM, takich jak Contoso|
|Tabela *|Nazwa obiektu|Nazwa obiektu|
|$skip|Pomiń zliczanie|Liczba wpisów, aby pominąć (domyślny = 0)|
|$top|Uzyskiwanie maksymalna liczba|Maksymalna liczba wpisów uzyskanie (domyślny = 256)|
|$filter|Filtrowanie kwerendy|Kwerendę filtru ODATA, aby ograniczyć wpisy zwrócone|
|$orderby|Uporządkuj według|Kwerenda orderBy ODATA do określania kolejności pozycji|

Gwiazdka (*) oznacza, że są one wymagane.

##### <a name="output-details"></a>Szczegóły wyników
ItemsList

| Nazwa właściwości | Typ danych |
|---|---|
|wartość|Tablica|


#### <a name="create-a-new-record"></a>Utwórz nowy rekord
Operacja tworzy nowy rekord jednostki. 

|Nazwa właściwości| Nazwa wyświetlana|Opis|
| ---|---|---|
|zestaw danych *|Nazwa organizacji|Nazwa organizacji CRM, takich jak Contoso|
|Tabela *|Nazwa obiektu|Nazwa obiektu|

Gwiazdka (*) oznacza, że są one wymagane.

##### <a name="output-details"></a>Szczegóły wyników
Brak.


#### <a name="get-record"></a>Pobieranie rekordu
Operacja otrzymuje określony rekord obiektu. 

|Nazwa właściwości| Nazwa wyświetlana|Opis|
| ---|---|---|
|zestaw danych *|Nazwa organizacji|Nazwa organizacji CRM, takich jak Contoso|
|Tabela *|Nazwa obiektu|Nazwa obiektu|
|Identyfikator *|Identyfikator elementu|Określ identyfikator rekordu|

Gwiazdka (*) oznacza, że są one wymagane.

##### <a name="output-details"></a>Szczegóły wyników
Brak.


#### <a name="delete-a-record"></a>Usuwanie rekordu
Operacja usuwa rekord z kolekcji jednostek. 

|Nazwa właściwości| Nazwa wyświetlana|Opis|
| ---|---|---|
|zestaw danych *|Nazwa organizacji|Nazwa organizacji CRM, takich jak Contoso|
|Tabela *|Nazwa obiektu|Nazwa obiektu|
|Identyfikator *|Identyfikator elementu|Określ identyfikator rekordu|

Gwiazdka (*) oznacza, że są one wymagane.


#### <a name="update-a-record"></a>Zaktualizuj rekord
Operacja aktualizacji istniejącego rekordu obiektu. 

|Nazwa właściwości| Nazwa wyświetlana|Opis|
| ---|---|---|
|zestaw danych *|Nazwa organizacji|Nazwa organizacji CRM, takich jak Contoso|
|Tabela *|Nazwa obiektu|Nazwa obiektu|
|Identyfikator *|Identyfikator rekordu|Określ identyfikator rekordu|

Gwiazdka (*) oznacza, że są one wymagane.

##### <a name="output-details"></a>Szczegóły wyników
Brak.


## <a name="http-responses"></a>Odpowiedzi HTTP

Akcje i wyzwalaczy może zwracać jedną lub więcej z poniższych kodów stanu HTTP: 

|Nazwa|Opis|
|---|---|
|200|Ok|
|202|Zaakceptowane|
|400|Nieprawidłowe żądanie|
|401|Brak autoryzacji|
|403|Dostęp zabroniony|
|404|Nie można odnaleźć|
|500|Wewnętrzny błąd serwera. Wystąpił nieznany błąd.|
|domyślne|Operacja nie powiodła się.|


## <a name="next-steps"></a>Następne kroki

[Tworzenie aplikacji logicznych](../app-service-logic/app-service-logic-create-a-logic-app.md). Poznaj dostępne łączniki w aplikacjach logiki w naszej [listy interfejsów API](apis-list.md).

