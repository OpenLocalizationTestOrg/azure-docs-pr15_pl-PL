<properties
pageTitle="SMTP | Microsoft Azure"
description="Tworzenie aplikacji logika z usługą Azure aplikacji. Połącz ze SMTP do wysyłania wiadomości e-mail."
services="logic-apps"   
documentationCenter=".net,nodejs,java"  
authors="msftman"   
manager="erikre"    
editor=""
tags="connectors" />

<tags
ms.service="app-service-logic"
ms.devlang="multiple"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="07/15/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-smtp-connector"></a>Rozpoczynanie pracy z łącznik protokołu SMTP

Połącz ze SMTP do wysyłania wiadomości e-mail.

Aby użyć [dowolny łącznik](./apis-list.md), najpierw należy utworzyć aplikację logicznych. Można rozpocząć, [tworząc aplikację logiczny, który jest teraz](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="connect-to-smtp"></a>Nawiązywanie połączenia z SMTP

Przed aplikacji logika uzyskać dostęp do dowolnej usługi, należy najpierw utworzyć *połączenie* z usługą. [Połączenia](./connectors-overview.md) umożliwia łączność aplikacji logiki i innej usługi. Na przykład aby połączyć się SMTP, należy najpierw protokołu SMTP *połączenia*. Aby utworzyć połączenie, należy podać poświadczenia są zwykle używanych do uzyskania dostępu do usługi, którą chcesz nawiązać połączenie. Tak w tym przykładzie SMTP, może być konieczne poświadczenia, aby nazwa połączenia, adres serwera SMTP i informacje logowania użytkownika w celu utworzenia połączenia SMTP. [Dowiedz się więcej o połączenia]()  

### <a name="create-a-connection-to-smtp"></a>Tworzenie połączenia z SMTP

>[AZURE.INCLUDE [Steps to create a connection to SMTP](../../includes/connectors-create-api-smtp.md)]

## <a name="use-an-smtp-trigger"></a>Za pomocą wyzwalacza SMTP

Wyzwalacz to zdarzenie, który może być używany do uruchamiania przepływu pracy, zdefiniowane w aplikacji logika. [Dowiedz się więcej o wyzwalaczy](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

W tym przykładzie ponieważ SMTP nie ma własną, wyzwalacz użyjemy wyzwalacza **usług Salesforce — po utworzeniu obiektu** . Ten wyzwalacz będzie Aktywuj po utworzeniu nowego obiektu w usług Salesforce. W naszym przykładzie firma Microsoft konfigurować go tak, aby każdorazowo po utworzeniu nowego potencjalnego klienta w usług Salesforce akcję *Wysyłanie wiadomości e-mail* odbywa się za pośrednictwem łącznik SMTP powiadomienie nowego potencjalnego klienta tworzony.

1. Wprowadzanie *usług salesforce* w polu wyszukiwania w Projektancie aplikacji logiczny, a następnie wybierz wyzwalacza **usług Salesforce — po utworzeniu obiektu** .  
 ![](../../includes/media/connectors-create-api-salesforce/trigger-1.png)  

2. **Po utworzeniu obiektu** formant jest wyświetlany.
 ![](../../includes/media/connectors-create-api-salesforce/trigger-2.png)  

3. Wybierz **Typ obiektu** , a następnie wybierz pozycję *potencjalnego klienta* z listy obiektów. W tym kroku są wskazujące, tworzysz wyzwalacz, który będzie wyświetlane powiadomienie o aplikacji logika przy każdym utworzeniu nowego potencjalnego klienta w usług Salesforce.  
 ![](../../includes/media/connectors-create-api-salesforce/trigger3.png)  

4. Utworzono wyzwalacza.  
 ![](../../includes/media/connectors-create-api-salesforce/trigger-4.png)  

## <a name="use-an-smtp-action"></a>Za pomocą akcji SMTP

Akcja jest czynność wykonaną przez przepływ pracy zdefiniowane w aplikacji logicznych. [Dowiedz się więcej o akcje](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

Teraz, gdy wyzwalacz został dodany, wykonaj poniższe czynności, aby dodać akcję SMTP, która wystąpi po utworzeniu nowego potencjalnego klienta usług Salesforce.

1. Wybierz pozycję **+ Nowy krok** Aby dodać akcję, którą chcesz wykonać po utworzeniu nowego potencjalnego klienta.  
 ![](../../includes/media/connectors-create-api-salesforce/trigger4.png)  

2. Wybierz pozycję **Dodaj akcję**. Ten zostanie otwarty pola wyszukiwania, gdzie możesz wyszukać żadnych działań możesz chcesz wykonać.  
 ![](../../includes/media/connectors-create-api-smtp/using-smtp-action-2.png)  

3. Wprowadź *smtp* , aby wyszukać działania związane z SMTP.  

4. Wybierz akcję do wykonania po utworzeniu nowego potencjalnego klienta **SMTP - Wyślij wiadomość E-mail** . Zostanie wyświetlona bloku sterowania akcji. Należy nawiązać połączenie smtp w bloku projektanta, jeśli jeszcze nie tego wcześniej.  
 ![](../../includes/media/connectors-create-api-smtp/smtp-2.png)    

5. Wprowadzania informacji odpowiedniej poczty e-mail w bloku **SMTP - Wyślij wiadomość E-mail** .  
 ![](../../includes/media/connectors-create-api-smtp/using-smtp-action-4.PNG)  

6. Zapisz swoją pracę w celu aktywowania przepływu pracy.  

## <a name="technical-details"></a>Szczegóły techniczne

Poniżej przedstawiono szczegółowe informacje dotyczące wyzwalacze, akcje i odpowiedzi, które obsługuje to połączenie:

## <a name="smtp-triggers"></a>Wyzwalacze SMTP

SMTP nie ma wyzwalaczy. 

## <a name="smtp-actions"></a>Akcje SMTP

SMTP obejmuje następujące czynności:


|Akcja|Opis|
|--- | ---|
|[Wysyłanie wiadomości E-mail](connectors-create-api-smtp.md#send-email)|Operacja wysyła wiadomość e-mail do jednego lub wielu adresatów.|

### <a name="action-details"></a>Szczegóły akcji

Oto szczegóły akcji tego łącznika, wraz z jej odpowiedzi:


### <a name="send-email"></a>Wysyłanie wiadomości E-mail
Operacja wysyła wiadomość e-mail do jednego lub wielu adresatów. 


|Nazwa właściwości| Nazwa wyświetlana|Opis|
| ---|---|---|
|Aby|Aby|Określanie adresów e-mail, rozdzielając je średnikami, takich jakrecipient1@domain.com;recipient2@domain.com|
|DW|DW|Określanie adresów e-mail, rozdzielając je średnikami, takich jakrecipient1@domain.com;recipient2@domain.com|
|Temat|Temat|Temat wiadomości e-mail|
|Treść|Treść|Treść wiadomości e-mail|
|Z|Z|Adres e-mail nadawcy, takich jaksender@domain.com|
|IsHtml|Jest Html|Wyślij wiadomość e-mail w formacie HTML (PRAWDA i FAŁSZ)|
|Pole UDW|pole UDW|Określanie adresów e-mail, rozdzielając je średnikami, takich jakrecipient1@domain.com;recipient2@domain.com|
|Znaczenie|Znaczenie|Ważność wiadomości e-mail (wysoki, normalny lub niski)|
|ContentData|Załączniki danych zawartości|Zawartość danych (kodowane base64 dla strumieni i jako-jest ciągu)|
|Typ zawartości|Typ zawartości załączników|Typ zawartości|
|ContentTransferEncoding|Kodowanie transferu zawartości załączników|Przeniesienie zawartości kodowanie (base64 lub brak)|
|Nazwa pliku|Nazwy plików załączników|Nazwa pliku|
|ContentId|Identyfikator zawartości załączników|Identyfikator zawartości|

* Wskazuje, że właściwość jest wymagane


## <a name="http-responses"></a>Odpowiedzi HTTP

Akcje i wyzwalaczy powyżej może zwracać jedną lub więcej z poniższych kodów stanu HTTP: 

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
[Tworzenie aplikacji warunków logicznych](../app-service-logic/app-service-logic-create-a-logic-app.md)