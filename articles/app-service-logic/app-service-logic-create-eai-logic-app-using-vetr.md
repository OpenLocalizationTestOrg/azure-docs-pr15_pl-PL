<properties
   pageTitle="Tworzenie aplikacji logika EAI przy użyciu VETR w logiczny aplikacje Azure aplikacji usługi | Microsoft Azure"
   description="Sprawdzanie poprawności, kodowanie i przekształcanie funkcje usług BizTalk XML"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="rajeshramabathiran"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/20/2016"
   ms.author="rajram"/>


# <a name="create-eai-logic-app-using-vetr"></a>Tworzenie aplikacji logika EAI przy użyciu VETR

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

Większość scenariuszy integracji aplikacji przedsiębiorstwa (EAI) pośredniczy danych między źródłowej i docelowej. Scenariusze tego typu są często wspólny zestaw wymagania:

- Upewnij się, czy dane z różnych systemów są poprawnie sformatowany.
- Wykonywanie "wyszukiwania" na przychodzące dane do podejmowania decyzji.
- Konwertowanie danych z jednego formatu na inny. Na przykład konwertowanie danych z systemem CRM format danych na format danych w systemie ERP.
- Przesyłanie danych do odpowiedniej aplikacji lub systemu.

W tym artykule opisano typowe deseń integracji: "pośrednictwa jednokierunkowe komunikat" lub VETR (Sprawdzanie poprawności, Enrich, przekształcenie, rozsyłania). Wzór VETR przekazuje danych między encja źródłowa i jednostki docelowej. Źródłowa i docelowa są zwykle źródeł danych.

Należy rozważyć, czy witryny sieci Web, która akceptuje zamówienia. Użytkownicy publikują zamówienia, aby system przy użyciu protokołu HTTP. W tle system poprawności przychodzących danych poprawności, normalizowane i będzie nadal występował w kolejce Bus usługi potrzeby dalszego przetwarzania. System ma zamówienia wyłączanie kolejce otwieraj go w określonym formacie. W związku z tym przepływ kompleksowe jest następujący:

**HTTP** → **sprawdzić poprawność** → **Przekształcanie** → **Bus usługi**

![Przepływ VETR podstawowe][1]

Następujące aplikacje interfejsu API BizTalk ułatwia tworzenie tego wzorca:

* **Wyzwalacza HTTP** - źródło, aby zdarzenie wyzwalające wiadomości
* **Sprawdzanie poprawności** — sprawdza poprawności danych przychodzących
* **Przekształcanie** — umożliwia przekształcenie dane z przychodzące formacie do formatu wymagane przez system podrzędne
* **Łącznik usługi Bus** - jednostki docelowej miejsce, w którym dane są przesyłane


## <a name="constructing-the-basic-vetr-pattern"></a>Budowa podstawowy wzorzec VETR
### <a name="the-basics"></a>Podstawowe informacje

W portalu usługi Azure wybierz pozycję **+ Nowy**, wybierz opcję **Web + Mobile**, a następnie wybierz **Aplikację logicznych**. Wybierz nazwę, lokalizację, subskrypcji, grupa zasobów i lokalizacji, w której działa. Grupy zasobów pełnić rolę kontenerów dla aplikacji; wszystkie zasoby dla aplikacji przejdź do tej samej grupy zasobów.

Następnie dodawanie wyzwalacze i akcje.


## <a name="add-http-trigger"></a>Dodawanie wyzwalacza HTTP
1. W **Logiczny szablony aplikacji**wybierz opcję **Utwórz od podstaw**.
1. Wybierz pozycję **Odbiornik HTTP** z galerii, aby utworzyć nowy odbiornik. Nadaj jej **HTTP1**.
2. Ustawianie **automatycznie wysyłał odpowiedzi?** ustawienie ma wartość FAŁSZ. Skonfiguruj akcję wyzwalacza przez ustawienie _Metodę HTTP_ _wpis_ i ustawienie _Względny adres URL_ do _/OneWayPipeline_:  
    ![Wyzwalacza HTTP][2]
3. Wybierz pozycję zielony znacznik wyboru, aby ukończyć wyzwalacza.

## <a name="add-validate-action"></a>Dodawanie sprawdzania poprawności akcji

Teraz przejdźmy wprowadź akcje, które są uruchamiane przy każdym wyzwalacza, oznacza to, że zawsze, gdy Odebrano połączenie dla punktu końcowego HTTP.

1. Dodawanie **Sprawdzania poprawności XML BizTalk** z galerii i nadaj mu nazwę _(Validate1)_ , aby utworzyć wystąpienie.
2. Konfigurowanie schematu XSD, aby sprawdzić poprawność wiadomości przychodzących XML. Wybierz akcję, _Sprawdź poprawność_ i wybierz _triggers('httplistener').outputs. Zawartość_ jako wartość parametru _inputXml_ .

Teraz akcji sprawdzania poprawności jest to pierwsza akcja po odbiornika protokołu HTTP: 

![Moduł sprawdzania poprawności BizTalk XML][3]

Podobnie możesz dodać pozostałe akcje. 

## <a name="add-transform-action"></a>Dodaj akcję przekształcenie
Załóżmy skonfigurować umożliwia przekształcenie, którą należy znormalizować przychodzących danych.

1. Dodawanie **Usługi Przekształcanie BizTalk** z galerii.
2. Aby skonfigurować lub przekształcenie, aby przekształcić wiadomości przychodzących XML, wybierz akcję, **Przekształcanie** jako akcji wykonywane podczas nosi nazwę tego interfejsu API. Wybierz pozycję ```triggers(‘httplistener’).outputs.Content``` jako wartość _inputXml_. *Mapa* jest opcjonalny parametr, ponieważ przychodzących danych jest dopasowany do wszystkich przekształceń skonfigurowane i zostaną zastosowane tylko te, które są zgodne z schematu.
3. Ponadto przekształcenia działa tylko wtedy, gdy sprawdzania poprawności zakończyło się powodzeniem. Aby skonfigurować ten warunek, wybierz ikonę koła zębatego w prawym górnym rogu, a następnie wybierz pozycję _Dodaj spełnienia warunku_. Warunek na ```equals(actions('xmlvalidator').status,'Succeeded')```:  

![Umożliwia przekształcenie BizTalk][4]


## <a name="add-service-bus-connector"></a>Dodawanie łącznika Bus usługi
Następnie dodawanie miejsce docelowe — kolejki Bus usługi — zapisać dane.

1. Dodawanie **Łącznika Bus usługi** z galerii. Ustaw **nazwę** _Servicebus1_, Ustaw ciąg połączenia, aby usługa wystąpienia bus **Parametry połączenia** , skonfiguruj **Nazwę obiektu** do _kolejki_i pominąć **nazwę subskrypcji**.
2. Wybierz akcję, **Wyślij wiadomość** i ustaw pole **zawartości** akcji _actions('transformservice').outputs. OutputXml_.
3. Ustawianie pola **Typ zawartości** do *kodu xml i aplikacji*:  

![Usługa Bus][5]


## <a name="send-http-response"></a>Wysyłanie odpowiedzi HTTP
Po zakończeniu przetwarzania potoku ponownie wysłać odpowiedź HTTP dla sukcesu i błąd poniższe czynności:

1. Dodawanie **Odbiornik HTTP** z galerii i wybierz akcję, **Wysłać odpowiedź HTTP** .
2. Ustaw **Identyfikator odpowiedzi** , aby wysłać *wiadomość*.
2. Ustaw **Odpowiedzi zawartości** do *procesu przetwarzania wykonane*.
3. **Kod stanu odpowiedzi** na *200* , aby wskazać HTTP 200 OK.
4. Wybierz rozwijane menu w prawym górnym rogu, a następnie wybierz pozycję **Dodaj spełnienia warunku**.  Warunek następujące wyrażenie:  
    ```@equals(actions('azureservicebusconnector').status,'Succeeded')```  <br/>
5. Powtórz te czynności, aby wysłać odpowiedź HTTP na także błąd. Zmienianie **warunku** następujące wyrażenie:  
```@not(equals(actions('azureservicebusconnector').status,'Succeeded'))``` <br/>
6. Wybierz **przycisk OK** , a następnie **utworzyć**.



## <a name="completion"></a>Zakończenie
Za każdym razem, ktoś wysyła wiadomość do punktu końcowego HTTP, wyzwalane aplikacji i wykonuje akcje, które właśnie utworzony. Aby zarządzać żadnych aplikacji logika tworzenie, wybierz pozycję **Przeglądaj** w Azure Portal i wybierz pozycję **Aplikacje logika**. Wybierz aplikację, aby uzyskać więcej informacji.

Niektóre tematy pomocne:

[Monitorowanie łączników i aplikacjami interfejsu API i zarządzanie nimi](app-service-logic-monitor-your-connectors.md)  <br/>
[Monitorowanie aplikacji warunków logicznych](app-service-logic-monitor-your-logic-apps.md)

<!--image references -->
[1]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/BasicVETR.PNG
[2]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/HTTPListener.PNG
[3]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/BizTalkXMLValidator.PNG
[4]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/BizTalkTransforms.PNG
[5]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/AzureServiceBus.PNG
