<properties
    pageTitle="Zabezpieczenia OAUTH łączników władz akredytacji bezpieczeństwa i aplikacje interfejsu API | Azure"
    description="Przeczytaj informacje o zabezpieczeniach OAUTH w łączników i interfejsu API aplikacji w usłudze Azure aplikacji; Architektura microservices; władz akredytacji bezpieczeństwa"
    services="logic-apps"
    documentationCenter=""
    authors="MandiOhlinger"
    manager="dwrede"
    editor="cgronlun"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="mandia"/>


# <a name="learn-about-oauth-security-in-saas-connectors"></a>Informacje dotyczące zabezpieczeń OAUTH w władz akredytacji bezpieczeństwa łączników

>[AZURE.NOTE] Tą wersją artykułu dotyczy wersji schematu 2014-12-01-podgląd aplikacji logicznych.

Wiele programów jako łączniki usług (władz akredytacji bezpieczeństwa), takich jak Facebook, Twitter, DropBox i tak dalej wymagającego uwierzytelnienia za pomocą protokołu OAUTH.  Użycie tych łączników władz akredytacji bezpieczeństwa z logiki aplikacji, firma Microsoft udostępnia środowisko użytkownika uproszczony klikniętego "Autoryzuj" w Projektancie aplikacji logicznych. Gdy użytkownik **Autoryzuj**, zostanie wyświetlony monit o Zaloguj się w (jeśli jeszcze nie) i dostarczyć zgodę na nawiązanie połączenia z usługą władz akredytacji bezpieczeństwa w Twoim imieniu. Po dostarczyć zgodę i Autoryzuj, aplikacji logika następnie dostęp do tych usług władz akredytacji bezpieczeństwa.

## <a name="create-your-own-saas-app"></a>Tworzenie aplikacji władz akredytacji bezpieczeństwa
Ten uproszczone środowisko jest możliwe, ponieważ możemy poprzednio utworzone i zarejestrowane naszych aplikacji w tych usług władz akredytacji bezpieczeństwa.  W niektórych przypadkach może być do rejestrowania i używanie własnej aplikacji.  Jest to konieczne, na przykład gdy chcesz użyć tych łączników władz akredytacji bezpieczeństwa w aplikacjach niestandardowych. W tym przykładzie użyto łącznik skrzynki, ale ten proces jest taka sama dla wszystkich łączników zależne od OAUTH.

Nawet w kontekście logiki aplikacji zamiast przy użyciu domyślnej aplikacji, które udostępniamy można użyć własnej aplikacji. Jeśli przycisk "Autoryzuj" nie może nawiązać połączenia, możesz spróbować tworzenia własnych aplikacji. Poniżej wymieniono te kroki dla łącznika Twitter:

1. Otwórz wybrany łącznik Twitter w portal Azure preview. Przejdź do **przeglądania** > **interfejsu API aplikacje**. Zaznacz wybrany łącznik Twitter:  
    ![][1]

2. Wybierz pozycję **Ustawienia** > **uwierzytelniania**:  
    ![][2]

3. Skopiuj wartość **Identyfikatora URI przekierować** :  
    ![][3]

4. Przejdź do [serwisu Twitter](http://apps.twitter.com) i **utworzenia nowej aplikacji**. W polu właściwości **Zwrotnego adresu URL** Wklej wartość **Przekierowywać URI** kopiowane z tego łącznika Twitter: ![][4]  
5. Po utworzeniu aplikacji Twitter, zaznacz **klucz i tokeny dostępu**. Skopiuj następujące wartości.
6. W ustawieniach uwierzytelniania łącznik Twitter Wklej te wartości we właściwościach **Identyfikator klienta** i **Hasło klienta** :   
    ![][5]  
7. Zapisywanie ustawień łączników.  

Teraz należy mógł korzystać z tego łącznika z aplikacji logika. Gdy używasz tego łącznika z aplikacji logika używa aplikacji zamiast jako domyślną aplikację.  

> [AZURE.NOTE] Jeśli masz autoryzowanych aplikacji wcześniej, może być konieczne ponownie autoryzować aplikacji.


<!--Image references-->
[1]: ./media/app-service-logic-oauth-security/TwitterConnector.png
[2]: ./media/app-service-logic-oauth-security/Authentication.png
[3]: ./media/app-service-logic-oauth-security/RedirectURI.png
[4]: ./media/app-service-logic-oauth-security/TwitterApp.png
[5]: ./media/app-service-logic-oauth-security/TwitterKeys.png
