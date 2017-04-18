<properties 
    pageTitle="Jak używać właściwości w zasady zarządzania interfejsu API platformy Azure" 
    description="Dowiedz się, jak używać właściwości w zasady zarządzania interfejsu API Azure." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>


# <a name="how-to-use-properties-in-azure-api-management-policies"></a>Jak używać właściwości w zasady zarządzania interfejsu API platformy Azure

Zasady zarządzania interfejsu API są zaawansowanych możliwości umożliwiająca programu publisher zmienić zachowanie interfejsu API przy użyciu konfiguracji systemu. Zasady są to zbiór instrukcji, które są wykonywane kolejno na żądanie lub odpowiedź interfejsu API. Zasady może być skonstruowane za pomocą wartości tekst dosłowny, zasady wyrażeń i właściwości. 

Każde wystąpienie usługi zarządzania interfejsu API ma kolekcji properties par klucz wartość globalnych wystąpienia usługi. Te właściwości mogą służyć do zarządzania stałe wartości ciągów we wszystkich konfiguracji interfejsu API i zasady. Każda właściwość ma następujące atrybuty.


| Atrybut | Typ            | Opis                                                                                             |
|-----------|-----------------|---------------------------------------------------------------------------------------------------------|
| Nazwa      | ciąg          | Nazwa właściwości. Może zawierać tylko litery, cyfry, okres, kreski i znaki podkreślenia. |
| Wartość     | ciąg          | Wartość właściwości. Nie może być puste lub zawierać tylko z odstępów.                           |
| Tajny    | wartość logiczna         | Określa, czy wartość jest tajne i mają zostać zaszyfrowane lub nie.                                |
| Znaczniki      | Tablica ciągu | Opcjonalnie znaczniki, które udostępniane może służyć do filtrowania listy właściwości.                               |

Właściwości są skonfigurowane w portalu programu publisher na karcie **Właściwości** . W poniższym przykładzie trzy właściwości są skonfigurowane.

![Właściwości][api-management-properties]

Wartości właściwości mogą zawierać ciągi tekstowe i [zasad wyrażeń](https://msdn.microsoft.com/library/azure/dn910913.aspx). W poniższej tabeli przedstawiono poprzednie właściwości trzy przykłady i ich atrybutów. Wartość `ExpressionProperty` to wyrażenie zasad, która zwraca wartość typu Ciąg zawierającą bieżącą datę i godzinę. Właściwość `ContosoHeaderValue` jest oznaczony jako hasło, aby jej wartość nie jest wyświetlane.

| Nazwa               | Wartość                      | Tajny | Znaczniki    |
|--------------------|----------------------------|--------|---------|
| ContosoHeader      | TrackingId                 | FAŁSZ  | Contoso |
| ContosoHeaderValue | ••••••••••••••••••••••     | PRAWDA   | Contoso |
| ExpressionProperty | @(DateTime.Now.ToString()) | FAŁSZ  |         |

## <a name="to-use-a-property"></a>Aby użyć właściwości

Aby użyć właściwości zasady, umieść nazwę właściwości wewnątrz parę podwójne nawiasy klamrowe, takich jak `{{ContosoHeader}}`, jak pokazano w poniższym przykładzie.

    <set-header name="{{ContosoHeader}}" exists-action="override">
      <value>{{ContosoHeaderValue}}</value>
    </set-header>

W tym przykładzie `ContosoHeader` jest używany jako nazwa nagłówka w `set-header` zasady, a `ContosoHeaderValue` jest używany jako wartość nagłówka. Gdy szacowana jest funkcja tych zasad podczas żądanie lub odpowiedź do bramy zarządzania interfejsu API `{{ContosoHeader}}` i `{{ContosoHeaderValue}}` są zamieniane na ich odpowiednich wartości.

Właściwości mogą być używane jako ukończone atrybut lub wartości elementów, jak pokazano w poprzednim przykładzie, ale ich można także wstawić do lub połączone z części wyrażenia tekst dosłowny, jak pokazano w poniższym przykładzie:`<set-header name = "CustomHeader{{ContosoHeader}}" ...>`

Właściwości mogą także zawierać zasady wyrażeń. W poniższym przykładzie `ExpressionProperty` jest używana.

    <set-header name="CustomHeader" exists-action="override">
        <value>{{ExpressionProperty}}</value>
    </set-header>

Gdy szacowana jest funkcja tych zasad, `{{ExpressionProperty}}` jest zastępowany jej wartość: `@(DateTime.Now.ToString())`. Ponieważ wartość to wyrażenie zasad, zostanie obliczone wyrażenie i zasady jest kontynuowana z jego wykonanie.

Istnieje możliwość przetestowania ten limit w portalu Deweloper, dzwoniąc operację, która ma zasady z właściwościami w zakresie. W poniższym przykładzie operacji nosi nazwę dwóch poprzedniego przykładu `set-header` zasady z właściwości. Należy zauważyć, że odpowiedzi zawiera dwa niestandardowe nagłówki, które zostały skonfigurowane zasady za pomocą właściwości.

![Dzięki portalowi deweloperów][api-management-send-results]

Jeśli obejrzeć [śledzenia Inspektor interfejsu API](api-management-howto-api-inspector.md) dla zawierający dwie poprzedniego zasady przykładowe z właściwości połączenia, możesz sprawdzić dwóch `set-header` zasady z wartości nieruchomości wstawione oraz wyrażenia zasad właściwości zawierającą wyrażenie zasad.

![Interfejs API Inspektor śledzenia][api-management-api-inspector-trace]

Należy zauważyć, że podczas wartości właściwości mogą zawierać zasad wyrażeń, wartości nieruchomości nie może zawierać innych właściwości. Jeśli tekst zawierający odwołanie właściwość jest używana dla wartości właściwości, takie jak `Property value text {{MyProperty}}`, nie będzie można zastąpić tej właściwości, a zostaną uwzględnione wartość właściwości.

## <a name="to-create-a-property"></a>Aby utworzyć właściwość

Aby utworzyć właściwość, kliknij pozycję **Dodaj właściwość** na karcie **Właściwości** .

![Dodaj właściwość][api-management-properties-add-property-menu]

**Nazwy** i **wartości** są wymagane wartości. Jeśli wartość tej właściwości jest hasło, zaznacz pole wyboru **to hasło** . Wprowadź jeden lub więcej znaczników opcjonalne ułatwiające organizowanie właściwości, a następnie kliknij przycisk **Zapisz**.

![Dodaj właściwość][api-management-properties-add-property]

Po zapisaniu nowej właściwości pola tekstowego **wyszukiwania właściwości** zostanie wypełniona nazwę nowej właściwości, a nowa właściwość zostanie wyświetlona. Aby wyświetlić wszystkie właściwości, wyczyść pole tekstowe **wyszukiwania właściwości** , a następnie naciśnij klawisz enter.

![Właściwości][api-management-properties-property-saved]

Aby uzyskać informacje na temat tworzenia właściwość za pomocą interfejsu API usługi REST zobacz [Tworzenie właściwość za pomocą interfejsu API usługi REST](https://msdn.microsoft.com/library/azure/mt651775.aspx#Put).

## <a name="to-edit-a-property"></a>Aby edytować właściwość

Aby edytować właściwość, kliknij przycisk **Edytuj** obok właściwości, aby edytować.

![Edytowanie właściwości][api-management-properties-edit]

Wprowadź odpowiednie zmiany, a następnie kliknij przycisk **Zapisz**. Jeśli zmienisz nazwę właściwości, wszelkie zasady, które odwołują się do tej właściwości są automatycznie aktualizowane korzystać z nową nazwą.

![Edytowanie właściwości][api-management-properties-edit-property]

Aby uzyskać informacje dotyczące edytowania właściwość za pomocą interfejsu API usługi REST zobacz [Edytowanie właściwości za pomocą interfejsu API usługi REST](https://msdn.microsoft.com/library/azure/mt651775.aspx#Patch).

## <a name="to-delete-a-property"></a>Aby usunąć właściwość

Aby usunąć właściwość, kliknij przycisk **Usuń** obok właściwości do usunięcia.

![Usuwanie właściwości][api-management-properties-delete]

Kliknij przycisk **Tak, usuń ją** , aby potwierdzić.

![Potwierdzanie usunięcia][api-management-delete-confirm]

>[AZURE.IMPORTANT] Jeśli właściwość odwołuje się wszelkie zasady, będzie mógł pomyślnie usunąć, aż usuniesz właściwość z wszystkie zasady, które z niej korzystać.

Aby uzyskać informacje o usuwaniu właściwość za pomocą interfejsu API usługi REST zobacz [Usuwanie właściwości za pomocą interfejsu API usługi REST](https://msdn.microsoft.com/library/azure/mt651775.aspx#Delete).

## <a name="to-search-and-filter-properties"></a>Aby wyszukiwanie i filtrowanie właściwości

Na karcie **Właściwości** znajdują się wyszukiwanie i filtrowanie funkcje ułatwiające zarządzanie właściwości. Aby filtrować listę właściwości według nazwy właściwości, wprowadź wyszukiwany termin w polu tekstowym **wyszukiwania właściwości** . Aby wyświetlić wszystkie właściwości, wyczyść pole tekstowe **wyszukiwania właściwości** , a następnie naciśnij klawisz enter.

![Wyszukiwanie][api-management-properties-search]

Aby filtrować listę właściwości według wartości znacznika, wprowadź jeden lub więcej znaczników w polu tekstowym **Filtruj według znaczników** . Aby wyświetlić wszystkie właściwości, wyczyść pole tekstowe **Filtruj według znaczników** i naciśnij klawisz enter.

![Filtr][api-management-properties-filter]

## <a name="next-steps"></a>Następne kroki

-   Dowiedz się więcej o pracy z zasad
    -   [Zasady zarządzania interfejsu API w](api-management-howto-policies.md)
    -   [Odwołanie do zasad](https://msdn.microsoft.com/library/azure/dn894081.aspx)
    -   [Zasady wyrażeń](https://msdn.microsoft.com/library/azure/dn910913.aspx)

## <a name="watch-a-video-overview"></a>Obejrzyj film wideo

> [AZURE.VIDEO use-properties-in-policies]

[api-management-properties]: ./media/api-management-howto-properties/api-management-properties.png
[api-management-properties-add-property]: ./media/api-management-howto-properties/api-management-properties-add-property.png
[api-management-properties-edit-property]: ./media/api-management-howto-properties/api-management-properties-edit-property.png
[api-management-properties-add-property-menu]: ./media/api-management-howto-properties/api-management-properties-add-property-menu.png
[api-management-properties-property-saved]: ./media/api-management-howto-properties/api-management-properties-property-saved.png
[api-management-properties-delete]: ./media/api-management-howto-properties/api-management-properties-delete.png
[api-management-properties-edit]: ./media/api-management-howto-properties/api-management-properties-edit.png
[api-management-delete-confirm]: ./media/api-management-howto-properties/api-management-delete-confirm.png
[api-management-properties-search]: ./media/api-management-howto-properties/api-management-properties-search.png
[api-management-send-results]: ./media/api-management-howto-properties/api-management-send-results.png
[api-management-properties-filter]: ./media/api-management-howto-properties/api-management-properties-filter.png
[api-management-api-inspector-trace]: ./media/api-management-howto-properties/api-management-api-inspector-trace.png

