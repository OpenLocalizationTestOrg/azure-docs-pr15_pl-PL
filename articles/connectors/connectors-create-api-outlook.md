<properties
pageTitle="Outlook.com | Microsoft Azure"
description="Tworzenie aplikacji logika z usługą Azure aplikacji. Łącznik Outlook.com umożliwia zarządzanie poczty, kalendarza i kontaktów. Można wykonywać różne akcje, takie jak Wyślij wiadomość e-mail, planowanie spotkań, dodać kontakty itd."
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

# <a name="get-started-with-the-outlookcom-connector"></a>Wprowadzenie do łącznika w usłudze Outlook.com

Łącznik usługi Outlook.com umożliwia zarządzanie poczty, kalendarza i kontaktów. Można wykonywać różne akcje, takie jak Wyślij wiadomość e-mail, planowanie spotkań, dodać kontakty itd.

>[AZURE.NOTE] Tą wersją artykułu dotyczy wersji schematu 2015-08-01-podgląd aplikacji logicznych. 

Można rozpocząć pracę, tworząc aplikację logiczny teraz, zobacz [Tworzenie aplikacji logicznych](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Wyzwalacze i akcje

Łącznik usługi Outlook.com mogą być używane jako akcji; ma trigger(s). Wszystkie łączniki obsługuje danych w formatach XML i JSON. 

 Łącznik usługi Outlook.com występują następujące akcje i/lub trigger(s) dostępne:

### <a name="outlookcom-actions"></a>Akcje usługi Outlook.com
Możesz wykonać następujące akcje:

|Akcja|Opis|
|--- | ---|
|[GetEmails](connectors-create-api-outlook.md#GetEmails)|Pobiera wiadomości e-mail z folderu|
|[WyślijWiadomośćEmail](connectors-create-api-outlook.md#SendEmail)|Wysyła wiadomość e-mail|
|[DeleteEmail](connectors-create-api-outlook.md#DeleteEmail)|Usuwa wiadomości e-mail za pomocą identyfikatora|
|[MarkAsRead](connectors-create-api-outlook.md#MarkAsRead)|Oznacza wiadomość e-mail jako przeczytanych|
|[Parametr from](connectors-create-api-outlook.md#ReplyTo)|Odpowiedzi na wiadomości e-mail|
|[GetAttachment](connectors-create-api-outlook.md#GetAttachment)|Pobiera e-mail załącznik według identyfikatorów|
|[SendMailWithOptions](connectors-create-api-outlook.md#SendMailWithOptions)|Wysyłanie wiadomości e-mail przy użyciu wielu opcji i poczekaj adresata reagować z jedną z opcji|
|[SendApprovalMail](connectors-create-api-outlook.md#SendApprovalMail)|Wysyłanie wiadomości e-mail zatwierdzanie i Oczekiwanie na odpowiedź od adresata|
|[CalendarGetTables](connectors-create-api-outlook.md#CalendarGetTables)|Pobiera kalendarzy|
|[CalendarGetItems](connectors-create-api-outlook.md#CalendarGetItems)|Pobiera elementy z kalendarza|
|[CalendarPostItem](connectors-create-api-outlook.md#CalendarPostItem)|Tworzy nowe zdarzenie|
|[CalendarGetItem](connectors-create-api-outlook.md#CalendarGetItem)|Pobiera określonego elementu z kalendarza|
|[CalendarDeleteItem](connectors-create-api-outlook.md#CalendarDeleteItem)|Usunięcie elementu kalendarza|
|[CalendarPatchItem](connectors-create-api-outlook.md#CalendarPatchItem)|Częściowo aktualizacji elementu kalendarza|
|[ContactGetTables](connectors-create-api-outlook.md#ContactGetTables)|Pobiera folderów kontaktów|
|[ContactGetItems](connectors-create-api-outlook.md#ContactGetItems)|Pobiera kontakty z folderu kontaktów|
|[ContactPostItem](connectors-create-api-outlook.md#ContactPostItem)|Tworzy nowy kontakt|
|[ContactGetItem](connectors-create-api-outlook.md#ContactGetItem)|Pobiera z określonym kontaktem z folderu kontaktów|
|[ContactDeleteItem](connectors-create-api-outlook.md#ContactDeleteItem)|Powoduje usunięcie kontaktu|
|[ContactPatchItem](connectors-create-api-outlook.md#ContactPatchItem)|Częściowo aktualizacje kontaktu|
### <a name="outlookcom-triggers"></a>Wyzwalacze Outlook.com
Można wykrywać te zdarzenia:

|Wyzwalacza | Opis|
|--- | ---|
|Na zdarzenie rozpoczynające się wkrótce|Uaktywnia przepływu uruchamiania Nadchodzące wydarzenia|
|W nowej wiadomości e-mail|Uaktywnia przepływu, gdy przychodzi nowa wiadomość e-mail|
|Na nowe elementy|Wyzwalane, gdy jest tworzony nowy element kalendarza|
|Na zaktualizowanych elementów|Wyzwalane modyfikacji elementu kalendarza|


## <a name="create-a-connection-to-outlookcom"></a>Tworzenie połączenia z usługi Outlook.com
Aby utworzyć logiki aplikacji z usługą Outlook.com, należy najpierw utworzyć **połączenie** następnie należy podać dane dla następujących właściwości: 

|Właściwość| Wymagane|Opis|
| ---|---|---|
|Tokenu|Tak|Poświadczenia usługi Outlook.com|
Po utworzeniu połączenia, można je wykonać czynności i odsłuchać wyzwalaczy opisane w tym artykule.

>[AZURE.INCLUDE [Steps to create a connection to Outlook.com](../../includes/connectors-create-api-outlook.md)] 

>[AZURE.TIP] To połączenie jest używane w innych aplikacjach logika.  

## <a name="reference-for-outlookcom"></a>Odwołanie do usługi Outlook.com
Dotyczy wersji: 1.0

## <a name="onupcomingevents"></a>OnUpcomingEvents
Na zdarzenie rozpoczynające się wkrótce: wyzwalane przepływu, uruchamiając Nadchodzące wydarzenia 

```GET: /Events/OnUpcomingEvents``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|Tabela|ciąg|tak|kwerendy|Brak|Unikatowy identyfikator kalendarza|
|lookAheadTimeInMinutes|Liczba całkowita|Brak|kwerendy|15|Czas (w minutach) do przodu poszukaj nadchodzących wydarzeniach|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Operacja zakończyła się pomyślnie|
|202|Operacja zakończyła się pomyślnie|
|400|BadRequest|
|401|Brak autoryzacji|
|403|Dostęp zabroniony|
|500|Wewnętrzny błąd serwera|
|domyślne|Operacja nie powiodła się.|


## <a name="getemails"></a>GetEmails
Pobieranie wiadomości e-mail: pobiera wiadomości e-mail z folderu 

```GET: /Mail``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|ścieżkafolderu|ciąg|Brak|kwerendy|Skrzynka odbiorcza|Ścieżka folderu do pobierania wiadomości e-mail (domyślny: "Skrzynka odbiorcza")|
|Do góry|Liczba całkowita|Brak|kwerendy|10|Liczba wiadomości e-mail w celu pobrania (domyślny: 10)|
|fetchOnlyUnread|wartość logiczna|Brak|kwerendy|wartość PRAWDA.|Pobieranie tylko nieprzeczytanych wiadomości e-mail?|
|includeAttachments|wartość logiczna|Brak|kwerendy|FAŁSZ|Jeśli ustawiono wartość true, załączniki pobierany wraz z wiadomości e-mail|
|searchQuery|ciąg|Brak|kwerendy|Brak|Kwerendy wyszukiwania do filtrowania wiadomości e-mail|
|Pomiń|Liczba całkowita|Brak|kwerendy|0|Liczba wiadomości e-mail, aby pominąć (domyślny: 0)|
|skipToken|ciąg|Brak|kwerendy|Brak|Przejdź token do pobrania Nowa strona|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Operacja zakończyła się pomyślnie|
|400|BadRequest|
|401|Brak autoryzacji|
|403|Dostęp zabroniony|
|500|Wewnętrzny błąd serwera|
|domyślne|Operacja nie powiodła się.|


## <a name="sendemail"></a>WyślijWiadomośćEmail
Wysyłanie wiadomości e-mail: wysyła wiadomość e-mail 

```POST: /Mail``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|Funkcja emailMessage| |tak|Treść|Brak|Adres e-mail|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Operacja zakończyła się pomyślnie|
|400|BadRequest|
|401|Brak autoryzacji|
|403|Dostęp zabroniony|
|500|Wewnętrzny błąd serwera|
|domyślne|Operacja nie powiodła się.|


## <a name="deleteemail"></a>DeleteEmail
Usuwanie wiadomości e-mail: usuwa wiadomości e-mail za pomocą identyfikatora 

```DELETE: /Mail/{messageId}``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|Identyfikator komunikatu|ciąg|tak|Ścieżka|Brak|Identyfikator wiadomości e-mail do usunięcia|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Operacja zakończyła się pomyślnie|
|400|BadRequest|
|401|Brak autoryzacji|
|403|Dostęp zabroniony|
|500|Wewnętrzny błąd serwera|
|domyślne|Operacja nie powiodła się.|


## <a name="markasread"></a>MarkAsRead
Oznaczanie jako przeczytane: oznacza wiadomości e-mail jako przeczytanych 

```POST: /Mail/MarkAsRead/{messageId}``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|Identyfikator komunikatu|ciąg|tak|Ścieżka|Brak|Czytanie wiadomości e-mail mają być oznaczane jako identyfikator|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Operacja zakończyła się pomyślnie|
|400|BadRequest|
|401|Brak autoryzacji|
|403|Dostęp zabroniony|
|500|Wewnętrzny błąd serwera|
|domyślne|Operacja nie powiodła się.|


## <a name="replyto"></a>Parametr from
Odpowiadanie na wiadomości e-mail: odpowiedzi na wiadomości e-mail 

```POST: /Mail/ReplyTo/{messageId}``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|Identyfikator komunikatu|ciąg|tak|Ścieżka|Brak|Identyfikator odpowiadania na wiadomości e-mail|
|komentarz|ciąg|tak|kwerendy|Brak|Odpowiedz komentarz|
|replyAll|wartość logiczna|Brak|kwerendy|FAŁSZ|Odpowiedzieć wszystkim adresatom|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Operacja zakończyła się pomyślnie|
|400|BadRequest|
|401|Brak autoryzacji|
|403|Dostęp zabroniony|
|500|Wewnętrzny błąd serwera|
|domyślne|Operacja nie powiodła się.|


## <a name="getattachment"></a>GetAttachment
Pobieranie załącznika: załącznik poczty e-mail pobiera według identyfikatorów 

```GET: /Mail/{messageId}/Attachments/{attachmentId}``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|Identyfikator komunikatu|ciąg|tak|Ścieżka|Brak|Identyfikator wiadomości e-mail|
|attachmentId|ciąg|tak|Ścieżka|Brak|Identyfikator załącznik, aby pobrać|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Operacja zakończyła się pomyślnie|
|400|BadRequest|
|401|Brak autoryzacji|
|403|Dostęp zabroniony|
|500|Wewnętrzny błąd serwera|
|domyślne|Operacja nie powiodła się.|


## <a name="onnewemail"></a>OnNewEmail
W nowej wiadomości e-mail: uruchamia przepływu, gdy przychodzi nowa wiadomość e-mail 

```GET: /Mail/OnNewEmail``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|ścieżkafolderu|ciąg|Brak|kwerendy|Skrzynka odbiorcza|Folder poczty e-mail, aby pobrać (domyślny: Skrzynka odbiorcza)|
|Aby|ciąg|Brak|kwerendy|Brak|Adresy e-mail adresatów|
|z|ciąg|Brak|kwerendy|Brak|Z adresu|
|znaczenie|ciąg|Brak|kwerendy|Normalny|Ważność wiadomości e-mail (wysoki, normalny, niski) (domyślny: normalny)|
|fetchOnlyWithAttachment|wartość logiczna|Brak|kwerendy|FAŁSZ|Pobieranie tylko wiadomości e-mail z załącznikiem|
|includeAttachments|wartość logiczna|Brak|kwerendy|FAŁSZ|Załączniki|
|subjectFilter|ciąg|Brak|kwerendy|Brak|Ciąg do wyszukiwania w temacie|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Operacja zakończyła się pomyślnie|
|202|Zaakceptowane|
|400|BadRequest|
|401|Brak autoryzacji|
|403|Dostęp zabroniony|
|500|Wewnętrzny błąd serwera|
|domyślne|Operacja nie powiodła się.|


## <a name="sendmailwithoptions"></a>SendMailWithOptions
Wysyłanie wiadomości e-mail z opcjami: wysyłanie wiadomości e-mail przy użyciu wielu opcji i poczekaj adresata reagować z jedną z opcji 

```POST: /mailwithoptions/$subscriptions``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|optionsEmailSubscription| |tak|Treść|Brak|Żądanie subskrypcji opcje wiadomości e-mail|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Ok|
|201|Utworzone subskrypcji|
|400|BadRequest|
|401|Brak autoryzacji|
|403|Dostęp zabroniony|
|500|Wewnętrzny błąd serwera|
|domyślne|Operacja nie powiodła się.|


## <a name="sendapprovalmail"></a>SendApprovalMail
Wysyłanie wiadomości e-mail zatwierdzenia: wysyłanie wiadomości e-mail zatwierdzanie i Oczekiwanie na odpowiedź od adresata 

```POST: /approvalmail/$subscriptions``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|approvalEmailSubscription| |tak|Treść|Brak|Żądanie subskrypcji do obsługi poczty e-mail zatwierdzenia|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Ok|
|201|Utworzone subskrypcji|
|400|BadRequest|
|401|Brak autoryzacji|
|403|Dostęp zabroniony|
|500|Wewnętrzny błąd serwera|
|domyślne|Operacja nie powiodła się.|


## <a name="calendargettables"></a>CalendarGetTables
Uzyskiwanie kalendarzy: pobiera kalendarzy 

```GET: /datasets/calendars/tables``` 

Nie ma żadnych parametrów dla tego połączenia
#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Ok|
|domyślne|Operacja nie powiodła się.|


## <a name="calendargetitems"></a>CalendarGetItems
Zdarzenia: pobiera elementy z kalendarza 

```GET: /datasets/calendars/tables/{table}/items``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|Tabela|ciąg|tak|Ścieżka|Brak|Unikatowy identyfikator kalendarza do pobrania|
|$filter|ciąg|Brak|kwerendy|Brak|Kwerenda filtru ODATA, aby ograniczyć liczbę wpisów|
|$orderby|ciąg|Brak|kwerendy|Brak|Kwerenda orderBy ODATA do określania kolejności wpisów.|
|$skip|Liczba całkowita|Brak|kwerendy|Brak|Liczba wpisów, aby pominąć (domyślny = 0)|
|$top|Liczba całkowita|Brak|kwerendy|Brak|Maksymalna liczba wpisów do pobierania (domyślny = 256)|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Ok|
|domyślne|Operacja nie powiodła się.|


## <a name="calendarpostitem"></a>CalendarPostItem
Tworzenie zdarzenia: umożliwia utworzenie nowego zdarzenia 

```POST: /datasets/calendars/tables/{table}/items``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|Tabela|ciąg|tak|Ścieżka|Brak|Unikatowy identyfikator kalendarza|
|element| |tak|Treść|Brak|Element kalendarza, aby utworzyć|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Ok|
|domyślne|Operacja nie powiodła się.|


## <a name="calendargetitem"></a>CalendarGetItem
Uzyskiwanie zdarzenia: pobiera określonego elementu z kalendarza 

```GET: /datasets/calendars/tables/{table}/items/{id}``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|Tabela|ciąg|tak|Ścieżka|Brak|Unikatowy identyfikator kalendarza|
|Identyfikator|ciąg|tak|Ścieżka|Brak|Unikatowy identyfikator elementu kalendarza do pobrania|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Ok|
|domyślne|Operacja nie powiodła się.|


## <a name="calendardeleteitem"></a>CalendarDeleteItem
Usuwanie zdarzenia: usuwa element kalendarza 

```DELETE: /datasets/calendars/tables/{table}/items/{id}``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|Tabela|ciąg|tak|Ścieżka|Brak|Unikatowy identyfikator kalendarza|
|Identyfikator|ciąg|tak|Ścieżka|Brak|Unikatowy identyfikator elementu kalendarza do usunięcia|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Ok|
|domyślne|Operacja nie powiodła się.|


## <a name="calendarpatchitem"></a>CalendarPatchItem
Aktualizowanie zdarzenia: częściowo aktualizacji elementu kalendarza 

```PATCH: /datasets/calendars/tables/{table}/items/{id}``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|Tabela|ciąg|tak|Ścieżka|Brak|Unikatowy identyfikator kalendarza|
|Identyfikator|ciąg|tak|Ścieżka|Brak|Unikatowy identyfikator elementu kalendarza, aby zaktualizować|
|element| |tak|Treść|Brak|Element kalendarza, aby zaktualizować|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Ok|
|domyślne|Operacja nie powiodła się.|


## <a name="calendargetonnewitems"></a>CalendarGetOnNewItems
Na nowe elementy: wyzwalane, gdy jest tworzony nowy element kalendarza 

```GET: /datasets/calendars/tables/{table}/onnewitems``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|Tabela|ciąg|tak|Ścieżka|Brak|Unikatowy identyfikator kalendarza|
|$filter|ciąg|Brak|kwerendy|Brak|Kwerenda filtru ODATA, aby ograniczyć liczbę wpisów|
|$orderby|ciąg|Brak|kwerendy|Brak|Kwerenda orderBy ODATA do określania kolejności wpisów.|
|$skip|Liczba całkowita|Brak|kwerendy|Brak|Liczba wpisów, aby pominąć (domyślny = 0)|
|$top|Liczba całkowita|Brak|kwerendy|Brak|Maksymalna liczba wpisów do pobierania (domyślny = 256)|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Ok|
|domyślne|Operacja nie powiodła się.|


## <a name="calendargetonupdateditems"></a>CalendarGetOnUpdatedItems
Zaktualizowane elementy: wyzwalane modyfikacji elementu kalendarza 

```GET: /datasets/calendars/tables/{table}/onupdateditems``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|Tabela|ciąg|tak|Ścieżka|Brak|Unikatowy identyfikator kalendarza|
|$filter|ciąg|Brak|kwerendy|Brak|Kwerenda filtru ODATA, aby ograniczyć liczbę wpisów|
|$orderby|ciąg|Brak|kwerendy|Brak|Kwerenda orderBy ODATA do określania kolejności wpisów.|
|$skip|Liczba całkowita|Brak|kwerendy|Brak|Liczba wpisów, aby pominąć (domyślny = 0)|
|$top|Liczba całkowita|Brak|kwerendy|Brak|Maksymalna liczba wpisów do pobierania (domyślny = 256)|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Ok|
|domyślne|Operacja nie powiodła się.|


## <a name="contactgettables"></a>ContactGetTables
Pobieranie folderów kontaktów: pobiera foldery kontaktów 

```GET: /datasets/contacts/tables``` 

Nie ma żadnych parametrów dla tego połączenia
#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Ok|
|domyślne|Operacja nie powiodła się.|


## <a name="contactgetitems"></a>ContactGetItems
Importowanie kontaktów: pobiera kontakty z folderu kontaktów 

```GET: /datasets/contacts/tables/{table}/items``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|Tabela|ciąg|tak|Ścieżka|Brak|Unikatowy identyfikator folderu kontaktów do pobrania|
|$filter|ciąg|Brak|kwerendy|Brak|Kwerenda filtru ODATA, aby ograniczyć liczbę wpisów|
|$orderby|ciąg|Brak|kwerendy|Brak|Kwerenda orderBy ODATA do określania kolejności wpisów.|
|$skip|Liczba całkowita|Brak|kwerendy|Brak|Liczba wpisów, aby pominąć (domyślny = 0)|
|$top|Liczba całkowita|Brak|kwerendy|Brak|Maksymalna liczba wpisów do pobierania (domyślny = 256)|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Ok|
|domyślne|Operacja nie powiodła się.|


## <a name="contactpostitem"></a>ContactPostItem
Tworzenie kontaktu: tworzy nowy kontakt 

```POST: /datasets/contacts/tables/{table}/items``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|Tabela|ciąg|tak|Ścieżka|Brak|Unikatowy identyfikator folderu kontaktów|
|element| |tak|Treść|Brak|Kontakt, aby utworzyć|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Ok|
|domyślne|Operacja nie powiodła się.|


## <a name="contactgetitem"></a>ContactGetItem
Uzyskiwanie kontakt: pobiera określonego kontaktu z folderu Kontakty 

```GET: /datasets/contacts/tables/{table}/items/{id}``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|Tabela|ciąg|tak|Ścieżka|Brak|Unikatowy identyfikator folderu kontaktów|
|Identyfikator|ciąg|tak|Ścieżka|Brak|Unikatowy identyfikator kontaktu do pobrania|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Ok|
|domyślne|Operacja nie powiodła się.|


## <a name="contactdeleteitem"></a>ContactDeleteItem
Usuwanie kontaktu: powoduje usunięcie kontaktu 

```DELETE: /datasets/contacts/tables/{table}/items/{id}``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|Tabela|ciąg|tak|Ścieżka|Brak|Unikatowy identyfikator folderu kontaktów|
|Identyfikator|ciąg|tak|Ścieżka|Brak|Unikatowy identyfikator kontaktu do usunięcia|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Ok|
|domyślne|Operacja nie powiodła się.|


## <a name="contactpatchitem"></a>ContactPatchItem
Aktualizuj kontakt: częściowo aktualizacje kontaktu 

```PATCH: /datasets/contacts/tables/{table}/items/{id}``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|Tabela|ciąg|tak|Ścieżka|Brak|Unikatowy identyfikator folderu kontaktów|
|Identyfikator|ciąg|tak|Ścieżka|Brak|Unikatowy identyfikator kontaktu, aby zaktualizować|
|element| |tak|Treść|Brak|Kontakt, aby zaktualizować|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Ok|
|domyślne|Operacja nie powiodła się.|


## <a name="object-definitions"></a>Definicje obiektów 

### <a name="triggerbatchresponseidictionarystringobject"></a>TriggerBatchResponse [IDictionary [ciąg, obiekt]]


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|wartość|Tablica|Brak |



### <a name="object"></a>Obiekt


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|



### <a name="sendmessage"></a>SendMessage


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|Załączniki|Tablica|Brak |
|Z|ciąg|Brak |
|DW|ciąg|Brak |
|Pole UDW|ciąg|Brak |
|Temat|ciąg|Tak |
|Treść|ciąg|Tak |
|Znaczenie|ciąg|Brak |
|IsHtml|wartość logiczna|Brak |
|Aby|ciąg|Tak |



### <a name="sendattachment"></a>SendAttachment


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|@odata.type|ciąg|Brak |
|Nazwa|ciąg|Tak |
|ContentBytes|ciąg|Tak |



### <a name="receivemessage"></a>ReceiveMessage


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|Identyfikator|ciąg|Brak |
|IsRead|wartość logiczna|Brak |
|HasAttachment|wartość logiczna|Brak |
|DateTimeReceived|ciąg|Brak |
|Załączniki|Tablica|Brak |
|Z|ciąg|Brak |
|DW|ciąg|Brak |
|Pole UDW|ciąg|Brak |
|Temat|ciąg|Tak |
|Treść|ciąg|Tak |
|Znaczenie|ciąg|Brak |
|IsHtml|wartość logiczna|Brak |
|Aby|ciąg|Tak |



### <a name="receiveattachment"></a>ReceiveAttachment


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|Identyfikator|ciąg|Tak |
|Typ zawartości|ciąg|Tak |
|@odata.type|ciąg|Brak |
|Nazwa|ciąg|Tak |
|ContentBytes|ciąg|Tak |



### <a name="digestmessage"></a>DigestMessage


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|Temat|ciąg|Tak |
|Treść|ciąg|Brak |
|Znaczenie|ciąg|Brak |
|Skrót|Tablica|Tak |
|Załączniki|Tablica|Brak |
|Aby|ciąg|Tak |



### <a name="triggerbatchresponsereceivemessage"></a>TriggerBatchResponse [ReceiveMessage]


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|wartość|Tablica|Brak |



### <a name="datasetsmetadata"></a>DataSetsMetadata


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|tabelaryczny|nie zdefiniowano|Brak |
|obiektów blob|nie zdefiniowano|Brak |



### <a name="tabulardatasetsmetadata"></a>TabularDataSetsMetadata


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|źródła|ciąg|Brak |
|displayName|ciąg|Brak |
|urlEncoding|ciąg|Brak |
|tableDisplayName|ciąg|Brak |
|tablePluralName|ciąg|Brak |



### <a name="blobdatasetsmetadata"></a>BlobDataSetsMetadata


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|źródła|ciąg|Brak |
|displayName|ciąg|Brak |
|urlEncoding|ciąg|Brak |



### <a name="tablemetadata"></a>TableMetadata


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|Nazwa|ciąg|Brak |
|Tytuł|ciąg|Brak |
|x-ms uprawnień|ciąg|Brak |
|x-ms możliwości|nie zdefiniowano|Brak |
|schematu|nie zdefiniowano|Brak |



### <a name="tablecapabilitiesmetadata"></a>TableCapabilitiesMetadata


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|sortRestrictions|nie zdefiniowano|Brak |
|filterRestrictions|nie zdefiniowano|Brak |
|filterFunctions|Tablica|Brak |



### <a name="tablesortrestrictionsmetadata"></a>TableSortRestrictionsMetadata


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|uwzględnianą w sortowaniu|wartość logiczna|Brak |
|unsortableProperties|Tablica|Brak |
|ascendingOnlyProperties|Tablica|Brak |



### <a name="tablefilterrestrictionsmetadata"></a>TableFilterRestrictionsMetadata


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|podatne|wartość logiczna|Brak |
|nonFilterableProperties|Tablica|Brak |
|Parametr requiredProperties|Tablica|Brak |



### <a name="optionsemailsubscription"></a>OptionsEmailSubscription


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|NotificationUrl|ciąg|Brak |
|Komunikat|nie zdefiniowano|Brak |



### <a name="messagewithoptions"></a>MessageWithOptions


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|Temat|ciąg|Tak |
|Opcje|ciąg|Tak |
|Treść|ciąg|Brak |
|Znaczenie|ciąg|Brak |
|Załączniki|Tablica|Brak |
|Aby|ciąg|Tak |



### <a name="subscriptionresponse"></a>SubscriptionResponse


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|Identyfikator|ciąg|Brak |
|zasób|ciąg|Brak |
|notificationType|ciąg|Brak |
|notificationUrl|ciąg|Brak |



### <a name="approvalemailsubscription"></a>ApprovalEmailSubscription


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|NotificationUrl|ciąg|Brak |
|Komunikat|nie zdefiniowano|Brak |



### <a name="approvalmessage"></a>ApprovalMessage


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|Temat|ciąg|Tak |
|Opcje|ciąg|Tak |
|Treść|ciąg|Brak |
|Znaczenie|ciąg|Brak |
|Załączniki|Tablica|Brak |
|Aby|ciąg|Tak |



### <a name="approvalemailresponse"></a>ApprovalEmailResponse


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|SelectedOption|ciąg|Brak |



### <a name="tableslist"></a>TablesList


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|wartość|Tablica|Brak |



### <a name="table"></a>Tabela


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|Nazwa|ciąg|Brak |
|DisplayName|ciąg|Brak |



### <a name="item"></a>Element


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|ItemInternalId|ciąg|Brak |



### <a name="calendaritemslist"></a>CalendarItemsList


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|wartość|Tablica|Brak |



### <a name="calendaritem"></a>CalendarItem


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|ItemInternalId|ciąg|Brak |



### <a name="contactitemslist"></a>ContactItemsList


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|wartość|Tablica|Brak |



### <a name="contactitem"></a>ContactItem


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|ItemInternalId|ciąg|Brak |



### <a name="datasetslist"></a>DataSetsList


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|wartość|Tablica|Brak |



### <a name="dataset"></a>Zestaw danych


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|Nazwa|ciąg|Brak |
|DisplayName|ciąg|Brak |


## <a name="next-steps"></a>Następne kroki
[Tworzenie aplikacji logiki](../app-service-logic/app-service-logic-create-a-logic-app.md)