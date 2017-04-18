<properties
pageTitle="SendGrid | Microsoft Azure"
description="Tworzenie aplikacji logika z usługą Azure aplikacji. Dostawca połączeń SendGrid pozwala na wysyłanie wiadomości e-mail i zarządzanie nimi listy adresatów."
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

# <a name="get-started-with-the-sendgrid-connector"></a>Wprowadzenie do łącznika SendGrid

Dostawca połączeń SendGrid pozwala na wysyłanie wiadomości e-mail i zarządzanie nimi listy adresatów.

>[AZURE.NOTE] Tą wersją artykułu dotyczy wersji schematu 2015-08-01-podgląd aplikacji logicznych. 

Można rozpocząć pracę, tworząc aplikację logiczny teraz, zobacz [Tworzenie aplikacji logicznych](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Wyzwalacze i akcje

Łącznik SendGrid może być używany jako akcji; ma trigger(s). Wszystkie łączniki obsługuje danych w formatach XML i JSON. 

 Łącznik SendGrid ma następujące akcje dostępne. Istnieją wyzwalacze nie.

### <a name="sendgrid-actions"></a>Akcje SendGrid
Możesz wykonać następujące akcje:

|Akcja|Opis|
|--- | ---|
|[WyślijWiadomośćEmail](connectors-create-api-sendgrid.md#sendemail)|Wysyła wiadomości e-mail za pomocą interfejsu API SendGrid (tylko do 10 000 adresatów)|
|[AddRecipientToList](connectors-create-api-sendgrid.md#addrecipienttolist)|Dodawanie pojedynczego adresata do listy adresatów|


## <a name="create-a-connection-to-sendgrid"></a>Tworzenie połączenia z SendGrid
Do tworzenia aplikacji w logiczny z SendGrid, musisz najpierw utworzyć **połączenie** następnie należy podać dane dla następujących właściwości: 

|Właściwość| Wymagane|Opis|
| ---|---|---|
|ApiKey|Tak|Podaj swój klucz interfejsu Api SendGrid|
 

>[AZURE.INCLUDE [Steps to create a connection to SendGrid](../../includes/connectors-create-api-sendgrid.md)]

>[AZURE.TIP] To połączenie jest używane w innych aplikacjach logika.

Po utworzeniu połączenia, można je wykonać czynności i odsłuchać wyzwalaczy opisane w tym artykule.

## <a name="reference-for-sendgrid"></a>Odwołanie do SendGrid
Dotyczy wersji: 1.0

## <a name="sendemail"></a>WyślijWiadomośćEmail
Wysyłanie wiadomości e-mail: wysyła wiadomości e-mail za pomocą interfejsu API SendGrid (tylko do 10 000 adresatów) 

```POST: /api/mail.send.json``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|żądanie| |tak|Treść|Brak|Wyślij wiadomość e-mail|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Ok|
|400|Nieprawidłowe żądanie|
|401|Brak autoryzacji|
|403|Dostęp zabroniony|
|404|Nie można odnaleźć|
|429|Zbyt wiele żądanie|
|500|Wewnętrzny błąd serwera. Wystąpił nieznany błąd|
|domyślne|Operacja nie powiodła się.|


## <a name="addrecipienttolist"></a>AddRecipientToList
Dodaj adresata do listy: Dodawanie pojedynczego adresata do listy adresatów 

```POST: /v3/contactdb/lists/{listId}/recipients/{recipientId}``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|listId|ciąg|tak|Ścieżka|Brak|Unikatowy identyfikator listy adresatów|
|recipientId|ciąg|tak|Ścieżka|Brak|Unikatowy identyfikator adresata|

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

### <a name="emailrequest"></a>EmailRequest


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|z|ciąg|Tak |
|Nazwa od|ciąg|Brak |
|Aby|ciąg|Tak |
|Nazwa do|ciąg|Brak |
|Temat|ciąg|Tak |
|Treść|ciąg|Tak |
|ishtml|wartość logiczna|Brak |
|DW|ciąg|Brak |
|ccname|ciąg|Brak |
|pole UDW|ciąg|Brak |
|bccname|ciąg|Brak |
|Parametr from|ciąg|Brak |
|Data|ciąg|Brak |
|nagłówki|ciąg|Brak |
|pliki|Tablica|Brak |
|nazwy plików|Tablica|Brak |



### <a name="emailresponse"></a>EmailResponse


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|Komunikat|ciąg|Brak |



### <a name="recipientlists"></a>RecipientLists


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|list|Tablica|Brak |



### <a name="recipientlist"></a>RecipientList


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|Identyfikator|Liczba całkowita|Brak |
|Nazwa|ciąg|Brak |
|recipient_count|Liczba całkowita|Brak |



### <a name="recipients"></a>Adresaci


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|Adresaci|Tablica|Brak |



### <a name="recipient"></a>Adresat


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|Adres e-mail|ciąg|Brak |
|nazwisko|ciąg|Brak |
|Imię|ciąg|Brak |
|Identyfikator|ciąg|Brak |


## <a name="next-steps"></a>Następne kroki
[Tworzenie aplikacji warunków logicznych](../app-service-logic/app-service-logic-create-a-logic-app.md)