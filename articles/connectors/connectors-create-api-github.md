<properties
pageTitle="GitHub | Microsoft Azure"
description="Tworzenie aplikacji logika z usługą Azure aplikacji. GitHub jest usłudze hostingu repozytorium cyfra oparte na sieci web. Zapewnia on wszystkich poprawek rozłożone kontroli i źródła kodu zarządzania (SCM) funkcji cyfra, a także dodawanie własnych funkcji."
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

# <a name="get-started-with-the-github-connector"></a>Wprowadzenie do łącznika GitHub

GitHub jest usłudze hostingu repozytorium cyfra oparte na sieci web. Zapewnia on wszystkich poprawek rozłożone kontroli i źródła kodu zarządzania (SCM) funkcji cyfra, a także dodawanie własnych funkcji.

>[AZURE.NOTE] Tą wersją artykułu dotyczy wersji schematu 2015-08-01-podgląd aplikacji logicznych. 

Można rozpocząć pracę, tworząc aplikację logiczny teraz, zobacz [Tworzenie aplikacji logicznych](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Wyzwalacze i akcje

Łącznik GitHub może być używany jako akcji; ma trigger(s). Wszystkie łączniki obsługuje danych w formatach XML i JSON. 

 Łącznik GitHub występują następujące akcje i/lub trigger(s) dostępne:

### <a name="github-actions"></a>Akcje GitHub
Możesz wykonać następujące akcje:

|Akcja|Opis|
|--- | ---|
|[CreateIssue](connectors-create-api-github.md#createissue)|Tworzy problemu|
### <a name="github-triggers"></a>GitHub wyzwalaczy
Można wykrywać te zdarzenia:

|Wyzwalacza | Opis|
|--- | ---|
|Po otwarciu problemu|Problem zostanie otwarty.|
|Jeśli problem jest zamknięty|Problem jest zamknięty.|
|Podczas przypisywania problemu|Problem jest przydzielony.|


## <a name="create-a-connection-to-github"></a>Tworzenie połączenia z GitHub
Do tworzenia aplikacji w logiczny z GitHub, musisz najpierw utworzyć **połączenie** następnie należy podać dane dla następujących właściwości: 

|Właściwość| Wymagane|Opis|
| ---|---|---|
|Tokenu|Tak|Poświadczenia GitHub|
Po utworzeniu połączenia, można je wykonać czynności i odsłuchać wyzwalaczy opisane w tym artykule. 

>[AZURE.INCLUDE [Steps to create a connection to GitHub](../../includes/connectors-create-api-github.md)]

>[AZURE.TIP] To połączenie jest używane w innych aplikacjach logicznych.

## <a name="reference-for-github"></a>Odwołanie do GitHub
Dotyczy wersji: 1.0

## <a name="createissue"></a>CreateIssue
Tworzenie problem: tworzy problemu 

```POST: /repos/{repositoryOwner}/{repositoryName}/issues``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|repositoryOwner|ciąg|tak|Ścieżka|Brak|Właściciel repozytorium|
|repositoryName|ciąg|tak|Ścieżka|Brak|Nazwa repozytorium|
|issueBasicDetails| |tak|Treść|Brak|Szczegóły problemu|

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


## <a name="issueopened"></a>IssueOpened
Po otwarciu problem: problem zostanie otwarty. 

```GET: /trigger/issueOpened``` 

Nie ma żadnych parametrów dla tego połączenia
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


## <a name="issueclosed"></a>IssueClosed
Gdy jest zamknięte problem: problem jest zamknięty. 

```GET: /trigger/issueClosed``` 

Nie ma żadnych parametrów dla tego połączenia
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


## <a name="issueassigned"></a>IssueAssigned
Podczas przypisywania problemu: przypisywania problemu 

```GET: /trigger/issueAssigned``` 

Nie ma żadnych parametrów dla tego połączenia
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

### <a name="issuebasicdetailsmodel"></a>IssueBasicDetailsModel


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|Tytuł|ciąg|Tak |
|Treść|ciąg|Tak |
|osoby przydzielonej|ciąg|Tak |



### <a name="issuedetailsmodel"></a>IssueDetailsModel


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|Tytuł|ciąg|Tak |
|Treść|ciąg|Tak |
|osoby przydzielonej|ciąg|Tak |
|Liczba|ciąg|Brak |
|Województwo|ciąg|Brak |
|created_at|ciąg|Brak |
|repository_url|ciąg|Brak |


## <a name="next-steps"></a>Następne kroki
[Tworzenie aplikacji warunków logicznych](../app-service-logic/app-service-logic-create-a-logic-app.md)