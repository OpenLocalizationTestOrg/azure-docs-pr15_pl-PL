<properties
   pageTitle="Interakcyjna Obsługa raportów za pomocą interfejsu API języka JavaScript | Microsoft Azure"
   description="Power BI osadzony, interakcyjna Obsługa raportów za pomocą interfejsu API języka JavaScript"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="interact-with-power-bi-reports-using-the-javascript-api"></a>Interakcyjna Obsługa raportów usługi Power BI za pomocą interfejsu API języka JavaScript

Interfejs API języka JavaScript Power BI umożliwia łatwe osadzanie raportach usługi Power BI w aplikacji. Przy użyciu interfejsu API aplikacji programowo można wchodzić w interakcje z raportu różne elementy, takie jak stron i filtry. Ten interakcji sprawia, że raportach usługi Power BI bardziej zintegrowane część aplikacji.

Możesz osadzić raportu usługi Power BI w aplikacji przy użyciu iframe, który znajduje się jako część aplikacji. Elementu iframe działa jako granicy między aplikacji i raportem, jak widać na poniższej ilustracji. 

![Power BI osadzony iframe bez interfejsu API języka Javascript](media\powerbi-embedded-interact-with-reports\powerbi-embedded-interact-report-1.png)

Elementu iframe ułatwia proces osadzania znacznie, ale bez interfejsu API języka JavaScript raport i aplikacja nie można korzystać ze sobą. Ten brak interakcji można tworzyć wrażenie raport nie jest naprawdę część aplikacji. Raport i aplikacji naprawdę konieczne komunikowanie się i z powrotem, jak pokazano na poniższej ilustracji.

![Power BI osadzony iframe z interfejsem API języka Javascript](media\powerbi-embedded-interact-with-reports\powerbi-embedded-interact-report-2.png)

Interfejs API języka JavaScript Power BI umożliwia pisanie kodu, jaki bezpieczne może upłynąć do granicy iframe. Dzięki temu aplikacja programowy wykonywanie akcji w raporcie, a także zdarzeń z akcji, które użytkownicy wprowadzają w raporcie.

## <a name="what-can-you-do-with-the-power-bi-javascript-api"></a>Co można robić z interfejsem API języka JavaScript Power BI?
Przy użyciu interfejsu API języka JavaScript można zarządzać raportów, przejdź do strony w raporcie w formie, filtrowanie raportu i obsługiwać osadzanie zdarzeń. Na poniższym diagramie przedstawiono strukturę interfejsu API.

![Power BI API języka JavaScript diagramu](media\powerbi-embedded-interact-with-reports\powerbi-embedded-interact-report-3.png)


### <a name="manage-reports"></a>Zarządzanie raportami
Interfejs API języka Javascript umożliwia zarządzanie zachowaniem na poziomie raportu i strony:

- Osadzanie określonych raport programu Power BI bezpieczne w aplikacji — spróbuj [Osadzanie pokaz aplikacji](http://azure-samples.github.io/powerbi-angular-client/#/scenario1)
  - Ustawianie token dostępu
- Konfigurowanie raportu.
  - Włączanie i wyłączanie okienko filtru i okienko nawigacji między stronami — spróbuj [zaktualizować ustawienia pokaz aplikacji](http://azure-samples.github.io/powerbi-angular-client/#/scenario6)
  - Ustawianie wartości domyślnych dla stron i filtry - spróbuj, [Ustawianie wartości domyślnych pokaz](http://azure-samples.github.io/powerbi-angular-client/#/scenario5)
- Włączanie i wyłączanie trybu pełnoekranowego

[Dowiedz się więcej o osadzanie raportu](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embedding-Basics)


### <a name="navigate-to-pages-in-a-report"></a>Przejdź do strony w raporcie
Enbales interfejsu API języka JavaScript do znalezienia wszystkich stron w raporcie w formie i ustawić na bieżącej stronie. Wypróbuj [aplikację pokaz nawigacji](http://azure-samples.github.io/powerbi-angular-client/#/scenario3).

[Dowiedz się więcej o Nawigacja między stronami](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Page-Navigation)

### <a name="filter-a-report"></a>Filtrowanie raportu
Interfejs API języka JavaScript udostępnia podstawowe i zaawansowane możliwości osadzony raportów i stron raportu filtrowania. Spróbuj [filtrowania aplikacji pokaz](http://azure-samples.github.io/powerbi-angular-client/#/scenario4)i przejrzyj niektórych wprowadzający kodu.  


#### <a name="basic-filters"></a>Filtry podstawowe
Podstawowy filtr jest umieszczany na poziomie kolumny lub hierarchii i zawiera listę wartości, aby uwzględnić lub wykluczyć.

```
const basicFilter: pbi.models.IBasicFilter = {
  $schema: "http://powerbi.com/product/schema#basic",
  target: {
    table: "Store",
    column: "Count"
  },
  operator: "In",
  values: [1,2,3,4]
}
```


#### <a name="advanced-filters"></a>Filtry zaawansowane
Filtry zaawansowane przy użyciu operatora logicznego i albo lub i zaakceptuj warunki jednej lub dwóch, każdy z własną operator i wartość. Obsługiwane warunki są:

- Brak
- Mniejsze
- Równe
- Większe
- Równe
- Zawiera
- DoesNotContain
- StartsWith
- DoesNotStartWith
- Jest
- IsNot
- Czy.pusta
- IsNotBlank

```
const advancedFilter: pbi.models.IAdvancedFilter = {
  $schema: "http://powerbi.com/product/schema#advanced",
  target: {
    table: "Store",
    column: "Name"
  },
  logicalOperator: "Or",
  conditions: [
    {
      operator: "Contains",
      value: "Wash"
    },
    {
      operator: "Contains",
      value: "Park"
    }
  ]
}
```
[Dowiedz się więcej na temat filtrowania](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Filters)


### <a name="handling-events"></a>Obsługa zdarzeń
Oprócz wysyłania informacji do elementu iframe, aplikacja także otrzymać informacje dotyczące następujących zdarzeń pochodzące z elementu iframe:

- Osadź
  - Załadowano
  - Błąd
- Raporty
  - pageChanged
  - dataSelected (już wkrótce)

[Dowiedz się więcej na temat obsługi zdarzeń](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Handling-Events)


## <a name="next-steps"></a>Następne kroki
Aby uzyskać więcej informacji na temat interfejsu API języka JavaScript Power BI skorzystaj z następujących łączy:

- [Interfejs API języka JavaScript stron typu Wiki](https://github.com/Microsoft/PowerBI-JavaScript/wiki)
- [Dokumentacja modelu obiektowego](https://microsoft.github.io/powerbi-models/modules/_models_.html)
- Przykłady
  - [Kąta](http://azure-samples.github.io/powerbi-angular-client)
  - [Członkowskimi](https://github.com/Microsoft/powerbi-ember)
- [Pokaz programu Live](https://microsoft.github.io/PowerBI-JavaScript/demo/)
