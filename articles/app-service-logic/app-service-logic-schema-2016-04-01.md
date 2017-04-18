<properties 
    pageTitle="Nową wersję schematu 2016-06-01 | Microsoft Azure" 
    description="Dowiedz się, jak napisać definicji JSON najnowszą wersję aplikacji warunków logicznych" 
    authors="jeffhollan" 
    manager="dwrede" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/25/2016"
    ms.author="jehollan"/>
    
# <a name="new-schema-version-2016-06-01"></a>Nową wersję schematu 2016-06-01

Nowy schemat i interfejsu API wersji dla aplikacji logika ma wiele udoskonaleń, które poprawić niezawodność i ułatwienia użycia logiki aplikacji. Istnieją 3 kluczowych różnic:

1. Dodanie zakresy, które są akcje, które zawierają zbiór akcji.
1. Warunki i pętle są najlepszych akcje
1. Wykonanie kolejnością więcej pełne za pośrednictwem `runAfter` właściwości (który zastępuje `dependsOn`)

Informacji na temat aktualizacji aplikacji logika ze schematu 2015-08-01-Podgląd w schemacie 2016-06-01 [zapoznaj się z poniższą sekcję uaktualnienia.](#upgrading-to-2016-06-01-schema)


## <a name="1-scopes"></a>1. zakresy

Jedną z najważniejszych zmian w tym schemacie jest dodanie zakresów i możliwość zagnieżdżania akcje w sobie.  Jest to pomocne podczas Konsolidacja zestawu akcji lub w przypadku konieczności zagnieździć akcje w sobie (na przykład warunek może zawierać inny warunek).  Więcej informacji na temat składni zakres może być znaleziony [w tym miejscu](app-service-logic-loops-and-scopes.md), ale przykład prostej zakres znajduje się poniżej:


```
{
    "actions": {
        "My_Scope": {
            "type": "scope",
            "actions": {                
                "Http": {
                    "inputs": {
                        "method": "GET",
                        "uri": "http://www.bing.com"
                    },
                    "runAfter": {},
                    "type": "Http"
                }
            }
        }
    }
}
```

## <a name="2-conditions-and-loops-changes"></a>2. warunki i pętle zmian

W poprzednich wersjach schematu warunków i pętli były parametry związane z jednej akcji.  To ograniczenie zniesieniu w tym schemacie, a teraz warunków i pętli się pojawić jako typ akcji.  Więcej informacji można znaleźć [w tym artykule](app-service-logic-loops-and-scopes.md), a poniżej przedstawiono prosty przykład akcji warunek:

```
{
    "If_trigger_is_foo": {
        "type": "If",
        "expression": "@equals(triggerBody(), 'foo')",
        "runAfter": { },
        "actions": {
            "Http_2": {
                "inputs": {
                    "method": "GET",
                    "uri": "http://www.bing.com"
                },
                "runAfter": {},
                "type": "Http"
            }
        },
        "else": 
        {
            "if_trigger_is_bar": "..."
        }      
    }
}
```

## <a name="3-runafter-property"></a>3. Właściwość RunAfter

Nowy `runAfter` zastępuje właściwość `dependsOn` aby umożliwić większą precyzją w kolejności uruchamiania.  `dependsOn`jest synonimem "Akcja uruchomiono i zakończyło się pomyślnie," jednak wielokrotnie trzeba wykonać akcję, jeśli poprzedniej akcji zakończyło się pomyślnie, nie powiodło się lub pominięte.  `runAfter`Umożliwia tej elastyczność.  Jest obiekt, który określa wszystkie pozycje akcji, które będzie działać po, a określa tablicę stan są dopuszczalne do wyzwalacza z.  Na przykład ma być uruchomiony po krok powiodła się A i B został pomyślnie lub nie powiodło się, czy utworzyć następujące `runAfter` właściwości:

```
{
    "...",
    "runAfter": {
        "A": ["Succeeded"],
        "B": ["Succeeded", "Failed"]
    }
}
```

## <a name="upgrading-to-2016-06-01-schema"></a>Uaktualnianie do schematu 2016-06-01

Tylko uaktualniania do nowego schematu 2016-06-01 trzeba wykonać kilka czynności.  [W tym artykule](app-service-logic-schema-2016-04-01.md)znajdują się szczegółowe informacje na temat zmian w schemacie.  Proces uaktualniania zawiera uruchamianie skryptu uaktualnienia, zapisywania jako nową aplikację logiki i potencjalnie zastępowania starą aplikację warunków logicznych w razie potrzeby.

1. Otwórz bieżącej aplikacji logicznych.
1. Kliknij przycisk **Aktualizuj schemat** na pasku narzędzi
   
    ![][1]
   
    Definicja uaktualnione zostaną zwrócone.  Można kopiowania i wklejania to do definicji zasobów, jeśli jest potrzebna, ale firma Microsoft **zaleca** , upewnij się, wszystkie połączenia za pomocą przycisku **Zapisz jako** odwołania są poprawne w aplikacji logika uaktualnione.
1. Kliknij przycisk **Zapisz jako** na pasku narzędzi karta uaktualnienia.
1. Wypełnij nazwę i logiki stan aplikacji, a następnie kliknij przycisk **Utwórz** , aby wdrażanie aplikacji uaktualnienia logiki.
1. Sprawdź, czy aplikacji uaktualnione logiczny działa zgodnie z oczekiwaniami.

    >[AZURE.NOTE] Jeśli używasz wyzwalacza ręcznie lub wezwania na zwrotnego adresu URL zostanie zostały zmienione w nowej aplikacji logicznych.  Aby sprawdzić działa end-to-end, a może klonowanie na istniejącej aplikacji logicznych w celu zachowania poprzedniego adresy URL, należy użyć nowy adres URL.

1. *Opcjonalne* Aby zastąpić poprzedni logika aplikacji z nową wersją schematu, użyj przycisku **klonowanie** na pasku narzędzi (przylegające do ikony **Aktualizacja schematu** na powyższym obrazie).  Jest to konieczne, tylko wtedy, gdy chcesz zachować ten sam identyfikator zasobu lub żądanie wyzwalacza adres URL aplikacji logicznych.

### <a name="upgrade-tool-notes"></a>Narzędzie uaktualniania notatek

#### <a name="condition-mapping"></a>Mapowanie warunku

Narzędzie spowoduje, że starań do grupy akcji PRAWDA i FAŁSZ gałąź razem w zakresie w definicji uaktualnione.  Specjalnie projektanta struktury `@equals(actions('a').status, 'Skipped')` powinien się pojawić jako `else` akcji.  Jednak jeśli narzędzie wykryje desenie go nie rozpozna potencjalnie utworzy osobne warunki dla PRAWDA i FAŁSZ gałęzi.  Czynności mogą być ponownie zamapowane opublikować uaktualnienie w razie potrzeby.

#### <a name="foreach-with-condition"></a>ForEach przy użyciu warunku
  
Pętli foreach z warunkiem według elementu poprzedniego wzorca mogą być replikowane w schemacie nowej akcji filtru.  Należy dzieje się automatycznie w czasie uaktualniania.  Warunek przestaje być akcję filtrowania przed pętli foreach (Aby przywrócić tylko tablicy elementów spełniających warunek), a tej tablicy jest przekazywany do akcji foreach.  Można wyświetlić przykład to [w tym artykule](app-service-logic-loops-and-scopes.md)

#### <a name="resource-tags"></a>Znaczniki zasobów

Znaczniki zasobu zostanie usunięta w czasie uaktualniania i trzeba będzie ponownie określić dla uaktualnionej przepływu pracy.

## <a name="other-changes"></a>Inne zmiany

### <a name="manual-trigger-renamed-to-request-trigger"></a>Ręczne wyzwalacza zmieniona na żądanie wyzwalacza

Typ `manual` przestarzałe i zmienić jego nazwę na `request` z tego typu z `http`.  Jest to bardziej zgodne z typem wzorca wykrytego przez wyzwalacz jest używany do utworzenia.

### <a name="new-filter-action"></a>Nowej akcji "filtr.

Jeśli pracujesz z szeroki wybór i potrzebę filtrowane w dół do pozycji mniejszy zestaw elementów, można nowego typu "filtru".  Akceptuje tablicy i warunku i będzie weryfikacji warunku dla każdego elementu i zwraca tablicę elementy, które spełniają warunek.

### <a name="foreach-and-until-action-restrictions"></a>ForEach i poczekaj, aż ograniczenia akcji

Foreach i poczekaj, aż pętli są ograniczone do jednej akcji.

### <a name="trackedproperties-on-actions"></a>TrackedProperties na akcje

Akcje teraz może mieć dodatkowe właściwości (równorzędne do `runAfter` i `type`) o nazwie `trackedProperties`.  Istnieje obiekt, który umożliwia określenie niektórych akcji nakładów lub wyników, które mają zostać uwzględnione w telemetrycznego Azure diagnostyczne, które są wysyłane w ramach przepływu pracy.  Na przykład:

```
{                
    "Http": {
        "inputs": {
            "method": "GET",
            "uri": "http://www.bing.com"
        },
        "runAfter": {},
        "type": "Http",
        "trackedProperties": {
            "responseCode": "@action().outputs.statusCode",
            "uri": "@action().inputs.uri"
        }
    }
}
```

## <a name="next-steps"></a>Następne kroki
- [Używanie definicję logiki aplikacji przepływu pracy](app-service-logic-author-definitions.md)
- [Tworzenie szablonu wdrażania aplikacji warunków logicznych](app-service-logic-create-deploy-template.md)


<!-- Image references -->
[1]: ./media/app-service-logic-schema-2016-04-01/upgradeButton.png
