<properties
pageTitle="Wunderlist | Microsoft Azure"
description="Tworzenie aplikacji logika z usługą Azure aplikacji. Wunderlist podać Menedżer zadania lista i zadania, ułatwiające osobom, ich wykonywać zadania.  Czy lista zakupów udostępniono mapą rodzina, pracy nad projektem, lub planowanie urlopu Wunderlist ułatwia przechwytywanie, udostępniać i wykonać usługi to¬dos. Wunderlist natychmiast synchronizuje między swojego telefonu, tabletu i komputer, aby umożliwić dostęp do wszystkich zadań z dowolnego miejsca."
services="logic-apps"   
documentationCenter=".net,nodejs,java"  
authors="msftman"   
manager="erikre"    
editor=""
tags="connectors" />

<tags
ms.service="logic-apps"
ms.devlang="multiple"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="08/18/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-wunderlist-connector"></a>Wprowadzenie do łącznika Wunderlist

Wunderlist podać Menedżer zadania lista i zadania, ułatwiające osobom, ich wykonywać zadania.  Czy lista zakupów udostępniono mapą rodzina, pracy nad projektem, lub planowanie urlopu Wunderlist ułatwia przechwytywanie, udostępniać i wykonać usługi to¬dos. Wunderlist natychmiast synchronizuje między swojego telefonu, tabletu i komputer, aby umożliwić dostęp do wszystkich zadań z dowolnego miejsca.

>[AZURE.NOTE] Tą wersją artykułu dotyczy wersji schematu 2015-08-01-podgląd aplikacji logicznych. 

Można rozpocząć pracę, tworząc aplikację logiczny teraz, zobacz [Tworzenie aplikacji logicznych](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Wyzwalacze i akcje

Łącznik Wunderlist może być używany jako akcji; ma trigger(s). Wszystkie łączniki obsługuje danych w formatach XML i JSON. 

 Łącznik Wunderlist występują następujące akcje i/lub trigger(s) dostępne:

### <a name="wunderlist-actions"></a>Akcje Wunderlist
Możesz wykonać następujące akcje:

|Akcja|Opis|
|--- | ---|
|[RetrieveLists](connectors-create-api-wunderlist.md#retrievelists)|Pobieranie listy skojarzonego z kontem.|
|[CreateList](connectors-create-api-wunderlist.md#createlist)|Tworzenie listy.|
|[ListTasks](connectors-create-api-wunderlist.md#listtasks)|Pobieranie zadań z określonej listy.|
|[CreateTask](connectors-create-api-wunderlist.md#createtask)|Tworzenie zadania|
|[ListSubTasks](connectors-create-api-wunderlist.md#listsubtasks)|Pobieranie podzadań z określonej listy lub z określonych zadań.|
|[CreateSubTask](connectors-create-api-wunderlist.md#createsubtask)|Tworzenie podzadań w określonych zadań|
|[ListNotes](connectors-create-api-wunderlist.md#listnotes)|Pobieranie notatek do określonej listy lub określonych zadań.|
|[CreateNote](connectors-create-api-wunderlist.md#createnote)|Dodawanie uwagi do konkretnego zadania|
|[ListComments](connectors-create-api-wunderlist.md#listcomments)|Pobieranie zadań komentarze do określonej listy lub określonych zadań.|
|[CreateComment](connectors-create-api-wunderlist.md#createcomment)|Dodawanie komentarza do konkretnego zadania|
|[RetrieveReminders](connectors-create-api-wunderlist.md#retrievereminders)|Pobieranie przypomnienia dla określonej listy lub określonych zadań.|
|[CreateReminder](connectors-create-api-wunderlist.md#createreminder)|Ustawianie przypomnienia.|
|[RetrieveFiles](connectors-create-api-wunderlist.md#retrievefiles)|Pobieranie plików do określonej listy lub określonych zadań.|
|[GetList](connectors-create-api-wunderlist.md#getlist)|Pobiera określonej listy|
|[DeleteList](connectors-create-api-wunderlist.md#deletelist)|Usuwa listę|
|[UpdateList](connectors-create-api-wunderlist.md#updatelist)|Aktualizowanie określonej listy|
|[GetTask](connectors-create-api-wunderlist.md#gettask)|Pobiera konkretnego zadania|
|[UpdateTask](connectors-create-api-wunderlist.md#updatetask)|Aktualizacje konkretnego zadania|
|[DeleteTask](connectors-create-api-wunderlist.md#deletetask)|Usuwa konkretnego zadania|
|[GetSubTask](connectors-create-api-wunderlist.md#getsubtask)|Pobiera określonych podzadania|
|[UpdateSubTask](connectors-create-api-wunderlist.md#updatesubtask)|Aktualizacje określone podzadania|
|[DeleteSubTask](connectors-create-api-wunderlist.md#deletesubtask)|Usuwa określone podzadania|
|[GetNote](connectors-create-api-wunderlist.md#getnote)|Pobieranie określoną notatkę|
|[AktualizacjęUwaga](connectors-create-api-wunderlist.md#updatenote)|Aktualizowanie określoną notatkę|
|[DeleteNote](connectors-create-api-wunderlist.md#deletenote)|Usuwanie określoną notatkę|
|[GetComment](connectors-create-api-wunderlist.md#getcomment)|Pobieranie komentarza konkretnego zadania|
|[UpdateReminder](connectors-create-api-wunderlist.md#updatereminder)|Aktualizowanie określonego przypomnienia|
|[DeleteReminder](connectors-create-api-wunderlist.md#deletereminder)|Usuwanie określonych przypomnienia|
### <a name="wunderlist-triggers"></a>Wunderlist wyzwalaczy
Można wykrywać te zdarzenia:

|Wyzwalacza | Opis|
|--- | ---|
|Kiedy zadanie jest ukończenia|Nowy przepływ uaktywnia przypada zadania na liście|
|Po utworzeniu nowego zadania|Uaktywnia nowy przepływ po utworzeniu nowego zadania na liście|
|Gdy wystąpi przypomnienia|Nowy przepływ uaktywnia występuje przypomnienia|


## <a name="create-a-connection-to-wunderlist"></a>Tworzenie połączenia z Wunderlist
Do tworzenia aplikacji w logiczny z Wunderlist, musisz najpierw utworzyć **połączenie** następnie należy podać dane dla następujących właściwości: 

|Właściwość| Wymagane|Opis|
| ---|---|---|
|Tokenu|Tak|Poświadczenia Wunderlist|
Po utworzeniu połączenia, można je wykonać czynności i odsłuchać wyzwalaczy opisane w tym artykule. 


>[AZURE.INCLUDE [Steps to create a connection to Wunderlist](../../includes/connectors-create-api-wunderlist.md)] 


>[AZURE.TIP] To połączenie jest używane w innych aplikacjach logika.

## <a name="reference-for-wunderlist"></a>Odwołanie do Wunderlist
Dotyczy wersji: 1.0

## <a name="triggertaskdue"></a>TriggerTaskDue
Kiedy zadanie jest ukończenia: nowy przepływ uaktywnia przypada zadania na liście 

```GET: /trigger/tasksdue``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|list_id|Liczba całkowita|tak|kwerendy|Brak|Identyfikator listy|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Operacja ukończona pomyślnie|


## <a name="triggertasknew"></a>TriggerTaskNew
Po utworzeniu nowego zadania: uaktywnia nowy przepływ po utworzeniu nowego zadania na liście 

```GET: /trigger/tasksnew``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|list_id|Liczba całkowita|tak|kwerendy|Brak|Identyfikator listy|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Operacja ukończona pomyślnie|


## <a name="triggerreminder"></a>TriggerReminder
Gdy wystąpi przypomnienia: nowy przepływ uaktywnia występuje przypomnienia 

```GET: /trigger/reminders``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|list_id|Liczba całkowita|tak|kwerendy|Brak|Identyfikator listy|
|TASK_ID|Liczba całkowita|Brak|kwerendy|Brak|Identyfikator zadania|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Operacja ukończona pomyślnie|


## <a name="retrievelists"></a>RetrieveLists
Uzyskiwanie listy: pobieranie listy skojarzonego z kontem. 

```GET: /lists``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Operacja ukończona pomyślnie|
|400|Nieprawidłowe żądanie|
|500|Wewnętrzny błąd serwera. Wystąpił nieznany błąd|
|domyślne|Operacja nie powiodła się.|


## <a name="createlist"></a>CreateList
Tworzenie listy: Tworzenie listy. 

```POST: /lists``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|wpis| |tak|Treść|Brak|Nowa lista ma zostać utworzony|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Operacja ukończona pomyślnie|
|domyślne|Operacja nie powiodła się.|


## <a name="listtasks"></a>ListTasks
Pobieranie zadań: pobieranie zadań z określonej listy. 

```GET: /tasks``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|list_id|Liczba całkowita|tak|kwerendy|Brak|Identyfikator listy|
|ukończone|wartość logiczna|Brak|kwerendy|Brak|Ukończone|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Operacja ukończona pomyślnie|
|400|Nieprawidłowe żądanie|
|500|Wewnętrzny błąd serwera. Wystąpił nieznany błąd|
|domyślne|Operacja nie powiodła się.|


## <a name="createtask"></a>CreateTask
Utwórz zadanie: Tworzenie zadania 

```POST: /tasks``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|wpis| |tak|Treść|Brak|Można utworzyć nowe zadanie|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|201|Utworzone|


## <a name="listsubtasks"></a>ListSubTasks
Podzadania: pobieranie podzadań z określonej listy lub z określonych zadań. 

```GET: /subtasks``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|list_id|Liczba całkowita|tak|kwerendy|Brak|Identyfikator listy|
|TASK_ID|Liczba całkowita|Brak|kwerendy|Brak|Identyfikator zadania|
|ukończone|wartość logiczna|Brak|kwerendy|Brak|Ukończone|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Operacja ukończona pomyślnie|
|400|Nieprawidłowe żądanie|
|500|Wewnętrzny błąd serwera. Wystąpił nieznany błąd|
|domyślne|Operacja nie powiodła się.|


## <a name="createsubtask"></a>CreateSubTask
Tworzenie podzadania: tworzenie podzadań w określonych zadań 

```POST: /subtasks``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|wpis| |tak|Treść|Brak|Nowe podzadania do utworzenia|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|201|Utworzone|


## <a name="listnotes"></a>ListNotes
Pobieranie notatek: pobieranie notatek do określonej listy lub określonych zadań. 

```GET: /notes``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|list_id|Liczba całkowita|tak|kwerendy|Brak|Identyfikator listy|
|TASK_ID|Liczba całkowita|Brak|kwerendy|Brak|Identyfikator zadania|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Operacja ukończona pomyślnie|
|400|Nieprawidłowe żądanie|
|500|Wewnętrzny błąd serwera. Wystąpił nieznany błąd|
|domyślne|Operacja nie powiodła się.|


## <a name="createnote"></a>CreateNote
Tworzenie notatki: Dodawanie uwagi do konkretnego zadania 

```POST: /notes``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|wpis| |tak|Treść|Brak|Nową notatkę do utworzenia|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|201|Utworzone|


## <a name="listcomments"></a>ListComments
Umieszczanie zadań komentarzy: pobieranie zadań komentarze do określonej listy lub określonych zadań. 

```GET: /task_comments``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|list_id|Liczba całkowita|tak|kwerendy|Brak|Identyfikator listy|
|TASK_ID|Liczba całkowita|Brak|kwerendy|Brak|Identyfikator zadania|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Operacja ukończona pomyślnie|
|400|Nieprawidłowe żądanie|
|500|Wewnętrzny błąd serwera. Wystąpił nieznany błąd|
|domyślne|Operacja nie powiodła się.|


## <a name="createcomment"></a>CreateComment
Dodawanie komentarza do zadania: Dodawanie komentarza do konkretnego zadania 

```POST: /task_comments``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|wpis| |tak|Treść|Brak|Nowy komentarz zadań do utworzenia|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|201|Utworzone|


## <a name="retrievereminders"></a>RetrieveReminders
Otrzymywanie przypomnień: pobieranie przypomnienia dla określonej listy lub określonych zadań. 

```GET: /reminders``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|list_id|Liczba całkowita|tak|kwerendy|Brak|Identyfikator listy|
|TASK_ID|Liczba całkowita|Brak|kwerendy|Brak|Identyfikator zadania|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Operacja ukończona pomyślnie|
|400|Nieprawidłowe żądanie|
|500|Wewnętrzny błąd serwera. Wystąpił nieznany błąd|
|domyślne|Operacja nie powiodła się.|


## <a name="createreminder"></a>CreateReminder
Ustawianie przypomnienia: Ustawianie przypomnienia. 

```POST: /reminders``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|wpis| |tak|Treść|Brak|Monit do utworzenia|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Operacja ukończona pomyślnie|
|domyślne|Operacja nie powiodła się.|


## <a name="retrievefiles"></a>RetrieveFiles
Pobieranie plików: pobieranie plików do określonej listy lub określonych zadań. 

```GET: /files``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|list_id|Liczba całkowita|tak|kwerendy|Brak|Identyfikator listy|
|TASK_ID|Liczba całkowita|Brak|kwerendy|Brak|Identyfikator zadania|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Operacja ukończona pomyślnie|
|400|Nieprawidłowe żądanie|
|500|Wewnętrzny błąd serwera. Wystąpił nieznany błąd|
|domyślne|Operacja nie powiodła się.|


## <a name="getlist"></a>GetList
Uzyskiwanie listy: pobiera określonej listy 

```GET: /lists/{id}``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|Identyfikator|ciąg|tak|Ścieżka|Brak|Identyfikator listy|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Ok|


## <a name="deletelist"></a>DeleteList
Usuwanie listy: Usuwa listę 

```DELETE: /lists/{id}``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|Identyfikator|Liczba całkowita|tak|Ścieżka|Brak|Identyfikator listy|
|poprawki|Liczba całkowita|tak|kwerendy|Brak|Poprawki|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|204|Brak zawartości|


## <a name="updatelist"></a>UpdateList
Aktualizowanie listy: aktualizowanie określonej listy 

```PATCH: /lists/{id}``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|Identyfikator|Liczba całkowita|tak|Ścieżka|Brak|Identyfikator listy|
|wpis| |tak|Treść|Brak|Szczegóły listy|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Ok|


## <a name="gettask"></a>GetTask
Pobieranie zadań: pobiera konkretnego zadania 

```GET: /tasks/{id}``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|list_id|Liczba całkowita|tak|kwerendy|Brak|Identyfikator listy|
|Identyfikator|Liczba całkowita|tak|Ścieżka|Brak|Identyfikator zadania|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Ok|


## <a name="updatetask"></a>UpdateTask
Zaktualizuj zadania: aktualizuje konkretnego zadania 

```PATCH: /tasks/{id}``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|list_id|Liczba całkowita|tak|kwerendy|Brak|Identyfikator listy|
|Identyfikator|Liczba całkowita|tak|Ścieżka|Brak|Identyfikator zadania|
|wpis| |tak|Treść|Brak|Szczegóły zadania|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Ok|


## <a name="deletetask"></a>DeleteTask
Usuwanie zadania: usuwa konkretnego zadania 

```DELETE: /tasks/{id}``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|list_id|Liczba całkowita|tak|kwerendy|Brak|Identyfikator listy|
|Identyfikator|Liczba całkowita|tak|Ścieżka|Brak|Identyfikator zadania|
|poprawki|Liczba całkowita|tak|kwerendy|Brak|Poprawki|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|204|Brak zawartości|


## <a name="getsubtask"></a>GetSubTask
Uzyskiwanie podzadań: pobiera określonych podzadania 

```GET: /subtasks/{id}``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|Identyfikator|ciąg|tak|Ścieżka|Brak|Identyfikator podzadania|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Ok|


## <a name="updatesubtask"></a>UpdateSubTask
Aktualizowanie podzadania: aktualizuje określonych podzadania 

```PATCH: /subtasks/{id}``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|Identyfikator|Liczba całkowita|tak|Ścieżka|Brak|Identyfikator podzadania|
|wpis| |tak|Treść|Brak|Szczegóły podzadań|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Ok|


## <a name="deletesubtask"></a>DeleteSubTask
Usuwanie podzadania: usuwa określone podzadania 

```DELETE: /subtasks/{id}``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|Identyfikator|Liczba całkowita|tak|Ścieżka|Brak|Identyfikator podzadania|
|poprawki|Liczba całkowita|tak|kwerendy|Brak|Poprawki|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|204|Brak zawartości|


## <a name="getnote"></a>GetNote
Uzyskiwanie Uwaga: pobieranie określoną notatkę 

```GET: /notes/{id}``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|Identyfikator|ciąg|tak|Ścieżka|Brak|Identyfikator skryptu|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Ok|


## <a name="updatenote"></a>AktualizacjęUwaga
Aktualizowanie Uwaga: aktualizowanie określoną notatkę 

```PATCH: /notes/{id}``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|Identyfikator|Liczba całkowita|tak|Ścieżka|Brak|Identyfikator skryptu|
|wpis| |tak|Treść|Brak|Szczegóły notatki|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Ok|


## <a name="deletenote"></a>DeleteNote
Usuwanie notatki: usuwanie określoną notatkę 

```DELETE: /notes/{id}``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|Identyfikator|Liczba całkowita|tak|Ścieżka|Brak|Identyfikator skryptu|
|poprawki|Liczba całkowita|tak|kwerendy|Brak|Poprawki|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|204|Brak zawartości|


## <a name="getcomment"></a>GetComment
Uzyskiwanie komentarz zadania: pobieranie komentarza określonego zadania 

```GET: /task_comments/{id}``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|Identyfikator|ciąg|tak|Ścieżka|Brak|Identyfikator komentarza|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Ok|


## <a name="updatereminder"></a>UpdateReminder
Aktualizowanie przypomnienia: aktualizowanie określonego przypomnienia 

```PATCH: /reminders/{id}``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|Identyfikator|Liczba całkowita|tak|Ścieżka|Brak|Identyfikator przypomnienie|
|wpis| |tak|Treść|Brak|Szczegóły przypomnienia|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Ok|


## <a name="deletereminder"></a>DeleteReminder
Usuwanie przypomnienia: usuwanie określonych przypomnienia 

```DELETE: /reminders/{id}``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|Identyfikator|Liczba całkowita|tak|Ścieżka|Brak|Identyfikator przypomnienie.|
|poprawki|Liczba całkowita|tak|kwerendy|Brak|Poprawki|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|204|Brak zawartości|


## <a name="object-definitions"></a>Definicje obiektów 

### <a name="list"></a>Lista


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|Identyfikator|Liczba całkowita|Brak |
|created_at|ciąg|Brak |
|Tytuł|ciąg|Brak |
|list_type|ciąg|Brak |
|Typ|ciąg|Brak |
|poprawki|Liczba całkowita|Brak |



### <a name="createdlist"></a>CreatedList


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|Identyfikator|Liczba całkowita|Brak |
|created_at|ciąg|Brak |
|Tytuł|ciąg|Brak |
|poprawki|Liczba całkowita|Brak |
|Typ|ciąg|Brak |



### <a name="task"></a>Zadanie


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|Identyfikator|Liczba całkowita|Brak |
|assignee_id|Liczba całkowita|Brak |
|assigner_id|Liczba całkowita|Brak |
|created_at|ciąg|Brak |
|created_by_id|Liczba całkowita|Brak |
|due_date|ciąg|Brak |
|list_id|Liczba całkowita|Brak |
|poprawki|Liczba całkowita|Brak |
|starred|wartość logiczna|Brak |
|Tytuł|ciąg|Brak |



### <a name="subtask"></a>Podzadań


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|Identyfikator|Liczba całkowita|Brak |
|TASK_ID|Liczba całkowita|Brak |
|created_at|ciąg|Brak |
|created_by_id|Liczba całkowita|Brak |
|poprawki|ciąg|Brak |
|Tytuł|ciąg|Brak |



### <a name="note"></a>Uwaga


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|Identyfikator|Liczba całkowita|Brak |
|TASK_ID|Liczba całkowita|Brak |
|zawartość|ciąg|Brak |
|created_at|ciąg|Brak |
|updated_at|ciąg|Brak |
|poprawki|Liczba całkowita|Brak |



### <a name="comment"></a>Komentarz


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|Identyfikator|Liczba całkowita|Brak |
|TASK_ID|Liczba całkowita|Brak |
|poprawki|Liczba całkowita|Brak |
|tekst|ciąg|Brak |
|Typ|ciąg|Brak |
|created_at|ciąg|Brak |



### <a name="reminder"></a>Przypomnienia


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|Identyfikator|Liczba całkowita|Brak |
|Data|ciąg|Brak |
|TASK_ID|Liczba całkowita|Brak |
|poprawki|Liczba całkowita|Brak |
|Typ|ciąg|Brak |
|created_at|ciąg|Brak |
|updated_at|ciąg|Brak |



### <a name="createdreminder"></a>CreatedReminder


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|Identyfikator|Liczba całkowita|Brak |
|Data|ciąg|Brak |
|TASK_ID|Liczba całkowita|Brak |
|poprawki|Liczba całkowita|Brak |
|created_at|ciąg|Brak |
|updated_at|ciąg|Brak |



### <a name="file"></a>Plik


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|Identyfikator|Liczba całkowita|Brak |
|adres URL|ciąg|Brak |
|TASK_ID|Liczba całkowita|Brak |
|list_id|Liczba całkowita|Brak |
|user_id|Liczba całkowita|Brak |
|nazwa_pliku|ciąg|Brak |
|CONTENT_TYPE|ciąg|Brak |
|file_size|Liczba całkowita|Brak |
|local_created_at|ciąg|Brak |
|created_at|ciąg|Brak |
|updated_at|ciąg|Brak |
|Typ|ciąg|Brak |
|poprawki|Liczba całkowita|Brak |



### <a name="newtask"></a>Nowe zadanie


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|list_id|Liczba całkowita|Tak |
|Tytuł|ciąg|Tak |
|assignee_id|Liczba całkowita|Brak |
|ukończone|wartość logiczna|Brak |
|recurrence_type|ciąg|Brak |
|recurrence_count|Liczba całkowita|Brak |
|due_date|ciąg|Brak |
|starred|wartość logiczna|Brak |



### <a name="newlist"></a>NewList


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|Tytuł|ciąg|Tak |



### <a name="newsubtask"></a>NewSubtask


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|list_id|Liczba całkowita|Tak |
|TASK_ID|Liczba całkowita|Tak |
|Tytuł|ciąg|Tak |
|ukończone|wartość logiczna|Brak |



### <a name="newnote"></a>NewNote


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|list_id|Liczba całkowita|Tak |
|TASK_ID|Liczba całkowita|Tak |
|zawartość|ciąg|Tak |



### <a name="newcomment"></a>Nowy_komentarz


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|list_id|Liczba całkowita|Tak |
|TASK_ID|Liczba całkowita|Tak |
|tekst|ciąg|Tak |



### <a name="newreminder"></a>NewReminder


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|list_id|Liczba całkowita|Tak |
|TASK_ID|Liczba całkowita|Tak |
|Data|ciąg|Tak |



### <a name="updatetask"></a>UpdateTask


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|poprawki|Liczba całkowita|Brak |
|Tytuł|ciąg|Brak |
|assignee_id|Liczba całkowita|Brak |
|ukończone|wartość logiczna|Brak |
|recurrence_type|ciąg|Brak |
|recurrence_count|Liczba całkowita|Brak |
|due_date|ciąg|Brak |
|starred|wartość logiczna|Brak |



### <a name="updatelist"></a>UpdateList


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|poprawki|Liczba całkowita|Brak |
|Tytuł|ciąg|Brak |



### <a name="updatesubtask"></a>UpdateSubtask


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|poprawki|Liczba całkowita|Brak |
|Tytuł|ciąg|Brak |
|ukończone|wartość logiczna|Brak |



### <a name="updatenote"></a>AktualizacjęUwaga


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|poprawki|Liczba całkowita|Brak |
|zawartość|ciąg|Brak |



### <a name="updatereminder"></a>UpdateReminder


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|Data|ciąg|Brak |
|poprawki|Liczba całkowita|Brak |


## <a name="next-steps"></a>Następne kroki
[Tworzenie aplikacji logiki](../app-service-logic/app-service-logic-create-a-logic-app.md)