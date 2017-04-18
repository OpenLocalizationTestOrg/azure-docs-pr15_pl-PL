<properties
pageTitle="Źródła | Microsoft Azure"
description="Tworzenie aplikacji logika z usługą Azure aplikacji. Usługa Project Online jest elastyczne rozwiązanie online do zarządzania portfelami (PPM) i codziennych pracy od firmy Microsoft. Dostarczono za pośrednictwem usługi Office 365, usługi Project Online umożliwia organizacjom szybko Rozpocznij pracę z funkcji zarządzania projektami zaawansowanych do planowania, określanie priorytetów i zarządzanie projektami i inwestycji portfela projektu — z niemal dowolnego miejsca na niemal dowolnym urządzeniu."
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

# <a name="get-started-with-the-projectonline-connector"></a>Wprowadzenie do łącznika źródła

Usługa Project Online jest elastyczne rozwiązanie online do zarządzania portfelami (PPM) i codziennych pracy od firmy Microsoft. Dostarczono za pośrednictwem usługi Office 365, usługi Project Online umożliwia organizacjom szybko Rozpocznij pracę z funkcji zarządzania projektami zaawansowanych do planowania, określanie priorytetów i zarządzanie projektami i inwestycji portfela projektu — z niemal dowolnego miejsca na niemal dowolnym urządzeniu.

>[AZURE.NOTE] Tą wersją artykułu dotyczy wersji schematu 2015-08-01-podgląd aplikacji logicznych. 

Można rozpocząć pracę, tworząc aplikację logiczny teraz, zobacz [Tworzenie aplikacji logicznych](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Wyzwalacze i akcje

Łącznik źródła może służyć jako akcji; ma trigger(s). Wszystkie łączniki obsługuje danych w formatach XML i JSON. 

 Łącznik źródła występują następujące akcje i/lub trigger(s) dostępne:

### <a name="projectonline-actions"></a>Akcje źródła
Możesz wykonać następujące akcje:

|Akcja|Opis|
|--- | ---|
|[ListProjects](connectors-create-api-projectonline.md#listprojects)|Lista projektów w witrynie usługi project online|
|[CreateProject](connectors-create-api-projectonline.md#createproject)|Tworzy nowy projekt w witrynie usługi project online|
|[CreateTask](connectors-create-api-projectonline.md#createtask)|Utworzenie nowego zadania w programie project można|
|[CreateResource](connectors-create-api-projectonline.md#createresource)|Tworzy zasoby przedsiębiorstwa w witrynie usługi project online|
|[ListTasks](connectors-create-api-projectonline.md#listtasks)|Wyświetla listę opublikowanych zadań w projekcie|
|[CheckoutProject](connectors-create-api-projectonline.md#checkoutproject)|Sprawdza się w witrynie projektu|
|[PublishProject](connectors-create-api-projectonline.md#publishproject)|Ewidencjonowanie i publikowanie istniejącego projektu w witrynie|
### <a name="projectonline-triggers"></a>Wyzwalacze źródła
Można wykrywać te zdarzenia:

|Wyzwalacza | Opis|
|--- | ---|
|Po utworzeniu nowego projektu|Uaktywnia przepływu przy każdym utworzeniu nowego projektu|
|Po utworzeniu nowego zasobu|Uaktywnia nowy przepływ po utworzeniu nowego zasobu|
|Po utworzeniu nowego zadania|Uaktywnia przepływu po utworzeniu nowego zadania|


## <a name="create-a-connection-to-projectonline"></a>Tworzenie połączenia z źródła
Do tworzenia aplikacji w logiczny ze źródła, musisz najpierw utworzyć **połączenie** następnie należy podać dane dla następujących właściwości: 

|Właściwość| Wymagane|Opis|
| ---|---|---|
|Tokenu|Tak|Poświadczenia źródła|

>[AZURE.INCLUDE [Steps to create a connection to ProjectOnline](../../includes/connectors-create-api-projectonline.md)]

>[AZURE.TIP] To połączenie jest używane w innych aplikacjach logicznych.

## <a name="reference-for-projectonline"></a>Odwołania do źródła
Dotyczy wersji: 1.0

## <a name="onnewproject"></a>OnNewProject
Po utworzeniu nowego projektu: wyzwalane przepływu przy każdym utworzeniu nowego projektu 

```GET: /trigger/_api/ProjectData/Projects``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|siteUrl|ciąg|tak|kwerendy|Brak|Adres url witryny swojej witryny projektu głównego (przykład: https://sampletenant.sharepoint.com/teams/sampleteam)|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Ok|
|400|Nieprawidłowe żądanie|
|401|Brak autoryzacji|
|403|Dostęp zabroniony|
|404|Nie można odnaleźć|
|500|Wewnętrzny błąd serwera. Wystąpił nieznany błąd|
|domyślne|Operacja nie powiodła się.|


## <a name="onnewresource"></a>OnNewResource
Po utworzeniu nowego zasobu: uaktywnia nowy przepływ po utworzeniu nowego zasobu 

```GET: /trigger/_api/ProjectData/Resources``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|siteUrl|ciąg|tak|kwerendy|Brak|Adres url witryny swojej witryny projektu głównego (przykład: https://sampletenant.sharepoint.com/teams/sampleteam)|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Ok|
|400|Nieprawidłowe żądanie|
|401|Brak autoryzacji|
|403|Dostęp zabroniony|
|404|Nie można odnaleźć|
|500|Wewnętrzny błąd serwera. Wystąpił nieznany błąd|
|domyślne|Operacja nie powiodła się.|


## <a name="onnewtask"></a>OnNewTask
Po utworzeniu nowego zadania: uaktywnia przepływu po utworzeniu nowego zadania 

```GET: /trigger/_api/ProjectData/Tasks``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|siteUrl|ciąg|tak|kwerendy|Brak|Adres url witryny swojej witryny projektu głównego (przykład: https://sampletenant.sharepoint.com/teams/sampleteam)|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Ok|
|400|Nieprawidłowe żądanie|
|401|Brak autoryzacji|
|403|Dostęp zabroniony|
|404|Nie można odnaleźć|
|500|Wewnętrzny błąd serwera. Wystąpił nieznany błąd|
|domyślne|Operacja nie powiodła się.|


## <a name="listprojects"></a>ListProjects
Lista projekty: Lista projektów w witrynie usługi project online 

```GET: /_api/ProjectServer/Projects``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|siteUrl|ciąg|tak|kwerendy|Brak|Adres url witryny swojej witryny projektu głównego (przykład: https://sampletenant.sharepoint.com/teams/sampleteam)|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Ok|
|400|Nieprawidłowe żądanie|
|401|Brak autoryzacji|
|403|Dostęp zabroniony|
|404|Nie można odnaleźć|
|500|Wewnętrzny błąd serwera. Wystąpił nieznany błąd|
|domyślne|Operacja nie powiodła się.|


## <a name="createproject"></a>CreateProject
Tworzy nowy projekt: tworzy nowy projekt w witrynie usługi project online 

```POST: /_api/ProjectServer/Projects``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|siteUrl|ciąg|tak|kwerendy|Brak|Adres url witryny swojej witryny projektu głównego (przykład: https://sampletenant.sharepoint.com/teams/sampleteam)|
|Proj| |tak|Treść|Brak|Nowy projekt, aby utworzyć|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Ok|
|400|Nieprawidłowe żądanie|
|401|Brak autoryzacji|
|403|Dostęp zabroniony|
|404|Nie można odnaleźć|
|500|Wewnętrzny błąd serwera. Wystąpił nieznany błąd|
|domyślne|Operacja nie powiodła się.|


## <a name="createtask"></a>CreateTask
Tworzy nowe zadanie: utworzenie nowego zadania w programie project można 

```POST: /_api/ProjectServer/Projects('{project_id}')/Draft/Tasks/Add``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|siteUrl|ciąg|tak|kwerendy|Brak|Adres url witryny swojej witryny projektu głównego (przykład: https://sampletenant.sharepoint.com/teams/sampleteam)|
|PROJECT_ID|ciąg|tak|Ścieżka|Brak|Unikatowy identyfikator projektu, aby dodać zadanie do|
|zadanie| |tak|Treść|Brak|Nowe zadanie, aby dodać do projektu|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Ok|
|400|Nieprawidłowe żądanie|
|401|Brak autoryzacji|
|403|Dostęp zabroniony|
|404|Nie można odnaleźć|
|500|Wewnętrzny błąd serwera. Wystąpił nieznany błąd|
|domyślne|Operacja nie powiodła się.|


## <a name="createresource"></a>CreateResource
Tworzenie nowego zasobu: utworzenie zasobów przedsiębiorstwa w witrynie usługi project online 

```POST: /_api/ProjectServer/EnterpriseResources``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|siteUrl|ciąg|tak|kwerendy|Brak|Adres url witryny swojej witryny projektu głównego (przykład: https://sampletenant.sharepoint.com/teams/sampleteam)|
|zasób| |tak|Treść|Brak|Nowy zasób organizacji, aby dodać do projektu|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Ok|
|400|Nieprawidłowe żądanie|
|401|Brak autoryzacji|
|403|Dostęp zabroniony|
|404|Nie można odnaleźć|
|500|Wewnętrzny błąd serwera. Wystąpił nieznany błąd|
|domyślne|Operacja nie powiodła się.|


## <a name="listtasks"></a>ListTasks
Zawiera listę zadań: listy opublikowanych zadań w projekcie 

```GET: /_api/ProjectServer/Projects('{project_id}')/Tasks``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|siteUrl|ciąg|tak|kwerendy|Brak|Adres url witryny swojej witryny projektu głównego (przykład: https://sampletenant.sharepoint.com/teams/sampleteam)|
|PROJECT_ID|ciąg|tak|Ścieżka|Brak|Unikatowy identyfikator projektu do pobierania zadania|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Ok|
|400|Nieprawidłowe żądanie|
|401|Brak autoryzacji|
|403|Dostęp zabroniony|
|404|Nie można odnaleźć|
|500|Wewnętrzny błąd serwera. Wystąpił nieznany błąd|
|domyślne|Operacja nie powiodła się.|


## <a name="checkoutproject"></a>CheckoutProject
Wyewidencjonowywanie projektu: sprawdza się w witrynie projektu 

```POST: /_api/ProjectServer/Projects('{project_id}')/checkOut``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|siteUrl|ciąg|tak|kwerendy|Brak|Adres url witryny swojej witryny projektu głównego (przykład: https://sampletenant.sharepoint.com/teams/sampleteam)|
|PROJECT_ID|ciąg|tak|Ścieżka|Brak|Unikatowy identyfikator projektu, aby dodać zadanie do|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Ok|
|400|Nieprawidłowe żądanie|
|401|Brak autoryzacji|
|403|Dostęp zabroniony|
|404|Nie można odnaleźć|
|500|Wewnętrzny błąd serwera. Wystąpił nieznany błąd|
|domyślne|Operacja nie powiodła się.|


## <a name="publishproject"></a>PublishProject
Ewidencjonowanie i publikowanie projektu: ewidencjonowanie i publikowanie i istniejącego projektu w witrynie 

```POST: /_api/ProjectServer/Projects('{project_id}')/Draft/Publish(true)``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|siteUrl|ciąg|tak|kwerendy|Brak|Adres url witryny swojej witryny projektu głównego (przykład: https://sampletenant.sharepoint.com/teams/sampleteam)|
|PROJECT_ID|ciąg|tak|Ścieżka|Brak|Unikatowy identyfikator projektu zaewidencjonowywanie|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Ok|
|400|Nieprawidłowe żądanie|
|401|Brak autoryzacji|
|403|Dostęp zabroniony|
|404|Nie można odnaleźć|
|500|Wewnętrzny błąd serwera. Wystąpił nieznany błąd|
|domyślne|Operacja nie powiodła się.|


## <a name="object-definitions"></a>Definicje obiektów 

### <a name="triggerprojectswrapper"></a>TriggerProjectsWrapper


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|wartość|Tablica|Brak |



### <a name="triggerproject"></a>TriggerProject


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|Data rozpoczęcia projektu|ciąg|Brak |
|Data zakończenia projektu|ciąg|Brak |
|Data utworzenia projektu|ciąg|Brak |
|Identyfikator projektu|ciąg|Brak |
|ProjectModifiedDate|ciąg|Brak |
|Typ projektu|Liczba całkowita|Brak |
|NazwaProjektu|ciąg|Brak |



### <a name="triggerresourceswrapper"></a>TriggerResourcesWrapper


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|wartość|Tablica|Brak |



### <a name="triggerresource"></a>TriggerResource


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|ResourceId|ciąg|Brak |
|Kalendarz bazowy zasobu|ciąg|Brak |
|Typ rezerwacji zasobu|Liczba całkowita|Brak |
|Zasób można bilansować|wartość logiczna|Brak |
|Koszt użycia zasobu|Liczba|Brak |
|Data utworzenia zasobu|ciąg|Brak |
|Zasób dostępny najwcześniej od|ciąg|Brak |
|ResourceEmail|ciąg|Brak |
|Inicjały zasobu|ciąg|Brak |
|Zasób jest aktywny|wartość logiczna|Brak |
|Zasób jest ogólny|wartość logiczna|Brak |
|Zasób dostępny najpóźniej do|ciąg|Brak |
|Data zmodyfikowania zasobu|ciąg|Brak |
|ResourceName|ciąg|Brak |
|ResourceStatsuName|ciąg|Brak |
|Typu zasobu|Liczba całkowita|Brak |
|TypeDescription|ciąg|Brak |
|TypeName|ciąg|Brak |



### <a name="triggertaskswrapper"></a>TriggerTasksWrapper


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|wartość|Tablica|Brak |



### <a name="triggertask"></a>Zadanie wyzwalacza


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|Identyfikator projektu|ciąg|Brak |
|TaskId|ciąg|Brak |
|NazwaProjektu|ciąg|Brak |
|Nazwa_zadania|ciąg|Brak |
|Data utworzenia zadania|ciąg|Brak |
|Data modyfikacji zadania|ciąg|Brak |
|Data rozpoczęcia zadania|ciąg|Brak |
|Data zakończenia zadania|ciąg|Brak |
|Priorytet zadania|Liczba całkowita|Brak |
|Zadanie jest aktywne|wartość logiczna|Brak |



### <a name="newproject"></a>NewProject


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|Nazwa|ciąg|Tak |
|Opis|ciąg|Brak |
|Rozpoczynanie|ciąg|Brak |



### <a name="newreource"></a>NewReource


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|Nazwa|ciąg|Tak |
|IsBudget|wartość logiczna|Brak |
|IsGeneric|wartość logiczna|Brak |
|IsInactive|wartość logiczna|Brak |



### <a name="project"></a>Projektu


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|ApprovedStart|ciąg|Brak |
|ApprovedEnd|ciąg|Brak |
|CheckedOutDate|ciąg|Brak |
|CheckOutDescription|ciąg|Brak |
|CheckOutId|ciąg|Brak |
|W polach CreatedDate|ciąg|Brak |
|Identyfikator|ciąg|Brak |
|IsCheckedOut|wartość logiczna|Brak |
|LastPublishedDate|ciąg|Brak |
|Utworzony Data|ciąg|Brak |
|OptimizerDecision|Liczba całkowita|Brak |
|PlannerDecision|Liczba całkowita|Brak |
|Typ projektu|Liczba całkowita|Brak |
|Nazwa|ciąg|Brak |
|WinprojVersion|ciąg|Brak |



### <a name="projectswrapper"></a>ProjectsWrapper


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|wartość|Tablica|Brak |



### <a name="newtask"></a>Nowe zadanie


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|Parametry|nie zdefiniowano|Tak |



### <a name="taskparameters"></a>TaskParameters


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|Nazwa|ciąg|Tak |
|Notatki|ciąg|Brak |
|Rozpoczynanie|ciąg|Brak |
|Czas trwania|ciąg|Brak |



### <a name="enterpriseresource"></a>EnterpriseResource


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|CanLevel|wartość logiczna|Brak |
|Kod|ciąg|Brak |
|CostAccrual|Liczba całkowita|Brak |
|CostCenter|ciąg|Brak |
|Utworzone|ciąg|Brak |
|DefaultBookingType|Liczba całkowita|Brak |
|Adres e-mail|ciąg|Brak |
|ExternalId|ciąg|Brak |
|Grupa|ciąg|Brak |
|Rekrutacji|ciąg|Brak |
|Identyfikator|ciąg|Brak |
|Inicjały|ciąg|Brak |
|IsActive|wartość logiczna|Brak |
|IsBudget|wartość logiczna|Brak |
|IsCheckedOut|wartość logiczna|Brak |
|IsGeneric|wartość logiczna|Brak |
|IsTeam|wartość logiczna|Brak |
|MaterialLabel|ciąg|Brak |
|Zmodyfikowany|ciąg|Brak |
|Nazwa|ciąg|Brak |
|Fonetyka nazw|ciąg|Brak |
|Typu zasobu|Liczba całkowita|Brak |
|TerminationDate|ciąg|Brak |



### <a name="taskswrapper"></a>TasksWrapper


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|wartość|Tablica|Brak |



### <a name="task"></a>Zadanie


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|Utworzone|ciąg|Brak |
|Zmodyfikowany|ciąg|Brak |
|Rozpoczynanie|ciąg|Brak |
|Zakończenie|ciąg|Brak |
|Nazwa|ciąg|Brak |
|Identyfikator|ciąg|Brak |
|Priority (priorytet)|Liczba całkowita|Brak |
|ProcentWykonania|Liczba całkowita|Brak |
|Notatki|ciąg|Brak |
|Kontakt|ciąg|Brak |


## <a name="next-steps"></a>Następne kroki
[Tworzenie aplikacji warunków logicznych](../app-service-logic/app-service-logic-create-a-logic-app.md)