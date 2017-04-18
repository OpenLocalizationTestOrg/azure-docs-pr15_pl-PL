<properties
   pageTitle="Omówienie funkcji Azure | Microsoft Azure"
   description="Opis sposobu użycia funkcji Azure zoptymalizować asynchroniczne obciążeń pracą w minutach."
   services="functions"
   documentationCenter="na"
   authors="mattchenderson"
   manager="erikre"
   editor=""
   tags=""
   keywords="Azure funkcje, funkcje przetwarzania, webhooks, dynamiczne obliczeń, pliki architektura"/>

<tags
   ms.service="functions"
   ms.devlang="multiple"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/29/2016"
   ms.author="cfowler;mahender;glenga"/>
   
   
# <a name="azure-functions-overview"></a>Omówienie funkcji Azure

Funkcje Azure to rozwiązanie łatwe uruchamiania fragmenty kodu lub "funkcje" w chmurze. Można wpisać tylko kod, potrzebne do problemu, nie martwiąc się całej aplikacji lub infrastruktury, aby go uruchomić. To upewnij rozwoju bardziej wydajność, a służy język rozwoju wybranym, takich jak C#, F #, Node.js, Python lub PHP. Zapłacić tylko uruchomieniu kodu i zaufanie Azure przeskalować stosownie do potrzeb.

Ten temat zawiera omówienie funkcji Azure. Jeśli chcesz od razu w przejść i rozpoczynanie pracy z funkcjami Azure, Rozpocznij od [Tworzenie pierwszej funkcja Azure](functions-create-first-azure-function.md). Jeśli szukasz więcej informacji technicznych o funkcjach, zobacz [Dokumentacja dewelopera](functions-reference.md).

## <a name="features"></a>Funkcje

Poniżej przedstawiono niektóre kluczowe funkcje Azure funkcje:
    
* **Wybór języka** - funkcji zapisu za pomocą C#, F #, Node.js, Python, PHP, partii, imprezie, Java lub dowolny plik wykonywalny.
* **Model cennik płatności za pomocą** — tylko za czas spędzony uruchamiania kodu. Zobacz opis opcji Planowanie dynamiczne aplikacji usług w [ceny sekcji](#pricing) poniżej.  
* **Przesuń do własnych zależności** — funkcje obsługuje NuGet i NPM, aby móc używać bibliotekach Ulubione.  
* **Zabezpieczenia zintegrowane** - wyzwalane ochrony HTTP funkcje za pomocą dostawców OAuth, takich jak usługi Azure Active Directory, Facebook, Google, Twitter i Account Microsoft.  
* **Integracja uproszczony** - łatwo korzystać z usługi Azure i ofert oprogramowanie jako z usługi (władz akredytacji bezpieczeństwa). Można znaleźć w [sekcji integracji](#integrations) poniżej kilka przykładów.  
* **Elastyczne rozwoju** – kodu swoich funkcji bezpośrednio w portalu lub skonfigurować integrację z ciągłym i wdrażanie kodu za pośrednictwem GitHub programu Visual Studio Team Services i innych [narzędzi do tworzenia obsługiwane](../app-service-web/web-sites-deploy.md#deploy-using-an-ide).  
* **Otwórz źródło** — środowisko uruchomieniowe funkcji jest Otwórz źródło i jest [dostępny na GitHub](https://github.com/azure/azure-webjobs-sdk-script).  

## <a name="what-can-i-do-with-functions"></a>Co można zrobić z funkcjami?

Funkcje Azure to doskonałe rozwiązanie w przypadku przetwarzania danych integracji systemów pracy z Internetem z czynności (IoT) i tworzenie prostych interfejsy API i microservices. Należy rozważyć, czy funkcje dla zadań, takich jak obraz lub kolejności przetwarzania, konserwacji pliku, długim zadania, które chcesz uruchomić w tle wątku lub dla wszystkich zadań, które chcesz uruchomić zgodnie z harmonogramem. 

Funkcje zawiera szablony ułatwiające rozpoczęcie pracy z kluczowe scenariusze, łącznie z następujących czynności:

* **BlobTrigger** - blob magazyn Azure proces po ich dodaniu do kontenerów. Można użyć tego zmiany rozmiaru obrazu.
* **EventHubTrigger** — odpowiedź na zdarzenia dostarczona do koncentratora zdarzenia Azure. Przydatna w przypadku oprzyrządowania aplikacji, przetwarzanie obsługi lub przepływu pracy użytkownika i scenariusze Internet czynności (IoT).
* **Ogólna webhook** - webhook proces HTTP żąda od obsługującego webhooks wszystkie inne usługi.
* **GitHub webhook** - odpowiadanie na zdarzenia występujące w sieci repozytoriach GitHub. Na przykład zobacz [Tworzenie webhook lub funkcji interfejsu API](functions-create-a-web-hook-or-api-function.md).
* **HTTPTrigger** - wyzwalacza wykonywanie kodu przy użyciu żądania HTTP.
* **QueueTrigger** - odpowiadanie na wiadomości przychodzące w kolejce magazyn Azure. Na przykład zobacz [Tworzenie funkcję Azure, która wiąże się z usługą Azure](functions-create-an-azure-connected-function.md).
* **ServiceBusQueueTrigger** - łączenie kodu z innych usług Azure lub w wersji lokalnej usługi przez słuchanie kolejki wiadomości. 
* **ServiceBusTopicTrigger** - łączenie kodu z innych usług Azure lub w wersji lokalnej usługi subskrypcja tematów. 
* **TimerTrigger** - wykonać czyszczenie lub innych zadań partii na wstępnie zdefiniowanego harmonogramu. Na przykład zobacz [Tworzenie wydarzenia przetwarzania funkcji](functions-create-an-event-processing-function.md).

Funkcje Azure obsługuje *wyzwalaczy*, które są sposoby uruchamiania wykonywanie kodu języka i *powiązań*, które są sposoby na upraszczanie kodowania dane wejściowe i wyjściowe. Aby uzyskać szczegółowy opis wyzwalaczami i powiązań, które udostępnia funkcje Azure zobacz [Funkcje Azure wyzwalaczami i dokumentacja dewelopera powiązań](functions-triggers-bindings.md).


## <a name="integrations"></a>Funkcje integracji

Funkcje Azure można zintegrować z różnych Azure i usługi 3rd firm. Umożliwia to aby wyzwolić funkcja i rozpoczęcia realizacji lub służyć jako dane wejściowe i wyjściowe kodu. Następujące funkcje integracji usługi są obsługiwane przez funkcje Azure. 

* Azure DocumentDB
* Koncentratory Azure zdarzenia 
* Azure aplikacji Mobile (tabel)
* Koncentratory Azure powiadomień
* Azure Bus usługi (kolejek i tematy)
* Azure miejsca do magazynowania (obiektów blob, kolejkach i tabel) 
* GitHub (webhooks)
* W lokalnym (za pomocą usługi Bus)

## <a name="pricing"></a>Ile kosztuje usługa działa koszt?

Funkcje Azure zawiera dwa rodzaje ceny plany, wybierz ten, który najlepiej pasuje do potrzeb: 

* **Dynamiczne hostingu plan** - po uruchomieniu funkcja, Azure zawiera wszystkie niezbędne zasoby obliczeniowe. Nie musisz się martwić dotyczących zarządzania zasobami i płacisz czas wykonywania kodu. Pełne szczegółowe informacje o cenach są dostępne na [ceny funkcji strony](/pricing/details/functions). 

* **Plan usług aplikacji** — uruchamiania usługi funkcji, tak jak w sieci web, mobile i aplikacje interfejsu API. Jeśli już korzystasz z aplikacji usługi dla innych aplikacji, można uruchamiać funkcje na tego samego planu, bez dodatkowych opłat. Szczegółowe informacje znajdują się na [stronie cennik usługi aplikacji](/pricing/details/app-service/).

Aby uzyskać więcej informacji na temat skalowania do funkcji, zobacz [jak skalowanie funkcje Azure](functions-scale.md).

##<a name="next-steps"></a>Następne kroki

+ [Tworzenie pierwszego funkcja Azure](functions-create-first-azure-function.md)  
Przeskakiwanie bezpośrednio w i tworzenie pierwszej funkcja przy użyciu funkcji Azure Szybki Start. 
+ [Dokumentacja dewelopera funkcje Azure](functions-reference.md)  
Więcej informacji technicznych dotyczących obsługi funkcji Azure i odwołanie przewiduje funkcje kodowanie i definiowanie wyzwalaczami i powiązań.
+ [Testowanie funkcji Azure](functions-test-a-function.md)  
W tym artykule opisano różne narzędzia i techniki testując funkcje.
+ [Jak skalowanie funkcje Azure](functions-scale.md)  
W tym artykule omówiono plany usługi dostępne za pomocą funkcji Azure wraz ze plan usług dynamiczne oraz wybierz plan, do prawej. 
+ [Dowiedz się więcej o usłudze Azure aplikacji](../app-service/app-service-value-prop-what-is.md)  
Funkcje Azure wykorzystuje platformy Azure aplikacji usług dla podstawowych funkcji, takich jak wdrożenia, zmienne środowiska i diagnostyki. 