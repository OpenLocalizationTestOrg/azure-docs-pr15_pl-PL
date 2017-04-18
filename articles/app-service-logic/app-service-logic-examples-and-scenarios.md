<properties
   pageTitle="Aplikacje logiki przykładów i scenariusze | Microsoft Azure"
   description="Przykłady typowych warunków logicznych aplikacje i Dowiedz się, jak wdrażać typowe scenariusze"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>

# <a name="logic-apps-examples-and-common-scenarios"></a>Przykłady aplikacji logiki i typowych scenariuszy

Ten dokument szczegóły typowe scenariusze oraz przykłady, aby ułatwić zrozumienie niektóre sposoby możesz umożliwia aplikacji logika automatyzowanie procesów biznesowych. 

## <a name="custom-triggers-and-actions"></a>Niestandardowe wyzwalacze i akcje

Istnieje kilka sposobów może uruchamiać aplikację logiczny z innej aplikacji. Oto kilka typowych przykładów:

- [Tworzenie niestandardowego wyzwalacza lub akcji](app-service-logic-create-api-app.md)
- [Długotrwałe akcje](app-service-logic-create-api-app.md)
- [Wyzwalacza żądania HTTP (POST)](app-service-logic-http-endpoint.md)
- [Wyzwalacze Webhook i akcje](app-service-logic-create-api-app.md)
- [Ankieta wyzwalaczy](app-service-logic-create-api-app.md)

### <a name="scenarios"></a>Scenariusze

- [Żądanie synchroniczne odpowiedź](app-service-logic-http-endpoint.md)
- [Żądanie odpowiedź wraz z programem SMS](https://channel9.msdn.com/Blogs/Windows-Azure/Azure-Logic-Apps-Walkthrough-Webhook-Functions-and-an-SMS-Bot)

## <a name="error-handling-and-logging"></a>Rejestrowanie i obsługi błędów

- [Wyjątku i obsługi błędów](app-service-logic-exception-handling.md)
- [Konfigurowanie alertów Azure i diagnostyki](app-service-logic-monitor-your-logic-apps.md)

### <a name="scenarios"></a>Scenariusze

- [Przypadków użycia: Błąd i obsługi wyjątków](app-service-logic-scenario-error-and-exception-handling.md)

## <a name="deploying-and-managing"></a>Wdrażanie i zarządzanie nimi

- [Tworzenie automatycznego wdrażania](app-service-logic-create-deploy-template.md)
- [Tworzenie i wdrażanie aplikacji logika z programu Visual Studio](app-service-logic-deploy-from-vs.md)
- [Monitorowanie logiki aplikacji](app-service-logic-monitor-your-logic-apps.md)

## <a name="content-types-conversions-and-transformations"></a>Typy zawartości, konwersji i przekształcenia

Logika aplikacje [języka definicji przepływu pracy](http://aka.ms/logicappsdocs) zawiera wiele funkcji umożliwia konwertowanie i pracy z różnymi typami zawartości.  Ponadto aparat wykona wszystkie może je zachować typy zawartości danych będzie przepływał przez przepływ pracy.

- [Obsługa typów zawartości](app-service-logic-content-type.md) takich jak aplikacji i json, kod xml i aplikacji i zwykły i tekst
- [Do tworzenia definicji przepływu pracy](app-service-logic-author-definitions.md)
- [Dokumentacja języka definicji przepływu pracy](http://aka.ms/logicappsdocs)

## <a name="batches-and-looping"></a>Partie i pętle

- [SplitOn](app-service-logic-loops-and-scopes.md)
- [ForEach](app-service-logic-loops-and-scopes.md)
- [Poczekaj, aż](app-service-logic-loops-and-scopes.md)

## <a name="integrating-with-azure-functions"></a>Integracja z funkcjami Azure

- [Integrację funkcji Azure](app-service-logic-azure-functions.md)

### <a name="scenarios"></a>Scenariusze

- [Funkcja Azure jako wyzwalacz Bus usługi](app-service-logic-scenario-function-sb-trigger.md)

## <a name="http-rest-and-soap"></a>HTTP, REST i SOAP

 - [Wywoływanie SOAP](https://blogs.msdn.microsoft.com/logicapps/2016/04/07/using-soap-services-with-logic-apps/)


Firma Microsoft będzie dodawaj przykłady i scenariusze do tego dokumentu. Aby poinformować nas o tym, jakie przykłady i scenariusze chcesz tutaj za pomocą sekcji komentarze poniżej.