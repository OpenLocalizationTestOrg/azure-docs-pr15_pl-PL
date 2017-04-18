<properties 
    pageTitle="Zasady zarządzania Azure interfejsu API w | Microsoft Azure" 
    description="Dowiedz się, jak tworzenie, edytowanie i skonfigurować zasady zarządzania interfejsu API." 
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


#<a name="policies-in-azure-api-management"></a>Zasady zarządzania Azure interfejsu API w

W obszarze Zarządzanie interfejsu API Azure zasady są zaawansowanych możliwości umożliwiająca programu publisher zmienić zachowanie interfejsu API przy użyciu konfiguracji systemu. Zasady to zbiór instrukcji, które są wykonywane kolejno na żądanie lub odpowiedź interfejsu API. Instrukcje popularne uwzględnić konwersję formatu z pliku XML do formatu JSON i połączeń tempa ograniczania, aby ograniczyć liczbę połączeń przychodzących z Deweloper. Wiele więcej zasad są dostępne po ich otrzymaniu.

Zobacz [Odwołanie do zasad][] , aby uzyskać pełną listę instrukcji zasad i ustawień.

Zasady są stosowane w bramie umieszczonym między interfejsu API dla klientów indywidualnych i zarządzanego interfejsu API. Brama odbiera żądania wszystkich i zazwyczaj przekazuje je niezmieniony API źródłowych. Jednak zasady można zastosować zmiany do żądanie przychodzące i wychodzące odpowiedzi.

Zasady wyrażeń może służyć jako wartości atrybutu lub tekstu w każdym z zasad zarządzania interfejsu API, chyba że zasady określi inaczej. Pewne zasady, takie jak zasady [Przepływ sterowania][] i [Ustaw zmienną][] są oparte na wyrażeniach zasady. Aby uzyskać więcej informacji zobacz [Zaawansowane zasady][] i [wyrażeń zasady][].

## <a name="scopes"> </a>Jak skonfigurować zasady
Zasady można skonfigurować globalnie, albo w zakresie [produktu][], [interfejsu API][] lub [operacji][]. Aby skonfigurować zasady, przejdź do edytora zasad w portalu programu publisher.

![Menu zasad][policies-menu]

Edytor zasady składa się z trzech głównych sekcji: zakres zasady (u góry), definicji zasady, gdzie zasady są edytowane (po lewej) i instrukcje listy (po prawej):

![Edytor zasad][policies-editor]

Aby rozpocząć konfigurowanie zasad, należy najpierw zaznaczyć zakres, w którym ma być stosowane zasady. Zrzucie **Starter** ekranu produktu jest zaznaczone. Zauważ, że kwadratowa symbol obok nazwy zasad wskazuje, że zasady jest już stosowane na tym poziomie.

![Zakres][policies-scope]

Ponieważ zasady została już zainstalowana, konfiguracji jest wyświetlana w widoku definicji.

![Konfigurowanie][policies-configure]

Zasady jest wyświetlany tylko do odczytu na początku. Aby przeprowadzić edycję definicji kliknij akcję **Konfigurowanie zasad** .

![Edytowanie][policies-edit]

Definicja zasad jest proste dokumentu XML, który opisuje sekwencji instrukcji przychodzących i wychodzących. XML można edytować bezpośrednio w oknie definicji. Lista instrukcji znajduje się w prawo i instrukcje dotyczące bieżącego zakresu są włączone i wyróżniony; jak pokazano w instrukcji **Ogranicz szybkość połączenia** ekranu powyżej.

Instrukcja enabled kliknięcie spowoduje dodanie odpowiednie XML w lokalizacji kursora w definicji widoku. 

>[AZURE.NOTE] Zasady, które chcesz dodać, nie jest włączona, upewnij się, że jesteś w odpowiedniego zakresu dla tej zasady. Każdy deklaracji zasad jest przeznaczony do użytku w niektórych sekcjach zasad i zakresów. Aby zapoznać się z sekcji zasad i zakresów zasady, zaznacz sekcji **zastosowania** dla tej zasady [Odwołanie do zasad][].

Pełna lista zasady i ich ustawienia są dostępne w [Odwołanie do zasad][].

Na przykład, aby dodać nowe zestawienie Aby ograniczyć przychodzących wezwań do określonych adresów IP, umieść kursor wewnątrz zawartość `inbound` elementu XML i kliknij pozycję instrukcji **rozmówcy ograniczenia adresy IP** .

![Zasady ograniczeń][policies-restrict]

Spowoduje to dodanie fragmentu kodu XML do `inbound` element, który zawiera wskazówki dotyczące konfigurowania instrukcji.

    <ip-filter action="allow | forbid">
        <address>address</address>
        <address-range from="address" to="address"/>
    </ip-filter>

Aby ograniczyć ruch przychodzący żądania i zaakceptować tylko te z adresem IP 1.2.3.4 zmodyfikować XML w następujący sposób:

    <ip-filter action="allow">
        <address>1.2.3.4</address>
    </ip-filter>

![Zapisywanie][policies-save]

Po ukończeniu konfigurowania instrukcje dotyczące zasad, kliknij przycisk **Zapisz** i zmiany będą przekazywane do bramy zarządzania interfejsu API natychmiast.

##<a name="sections"> </a>Opis konfiguracji zasad

Zasady jest serię instrukcji, które wykonują aby i odpowiadania na wezwania. Konfiguracja jest prawidłowo podzielony na `inbound`, `backend`, `outbound`, i `on-error` sekcje, jak pokazano w następującej konfiguracji.

    <policies>
      <inbound>
        <!-- statements to be applied to the request go here -->
      </inbound>
      <backend>
        <!-- statements to be applied before the request is forwarded to 
             the backend service go here -->
      </backend>
      <outbound>
        <!-- statements to be applied to the response go here -->
      </outbound>
      <on-error>
        <!-- statements to be applied if there is an error condition go here -->
      </on-error>
    </policies> 

Jeśli występuje błąd podczas przetwarzania żądania, wszelkie pozostałe kroki `inbound`, `backend`, lub `outbound` sekcje są pomijane i wykonanie przechodzi do instrukcji w `on-error` sekcji. Przez umieszczenie zasad instrukcji w `on-error` sekcji, możesz przejrzeć błąd przy użyciu `context.LastError` właściwości do sprawdzania i dostosowywanie przy użyciu odpowiedzi błędu `set-body` zasady i skonfigurować, co się stanie, jeśli wystąpi błąd. Istnieją kody błędów instrukcje wbudowane i pod kątem błędów, które mogą wystąpić podczas przetwarzania zasady. Aby uzyskać więcej informacji zobacz [Błąd obsługi w zasady zarządzania interfejsu API](https://msdn.microsoft.com/library/azure/mt629506.aspx).

Ponieważ zasady można określić na różnych poziomach (globalnego, produktu i interfejsu api operacji) konfiguracji umożliwia w celu określenia kolejności, w której instrukcje definicji zasad wykonywanie w odniesieniu do zasad nadrzędnej. 

Zakresy zasady są uwzględniane w następującej kolejności.

1. Globalne
2. Zakres produktu
3. Interfejs API zakresu
4. Zakres działania

Instrukcje w nich są obliczane według położenia `base` elementu, jeśli jest zainstalowany.

Na przykład jeśli zasady na poziomie globalnym i skonfigurowane dla interfejsu API zasady, następnie zawsze, gdy używa się tego określonego interfejsu API obu zasady zostaną zastosowane. Interfejs API zarządzania umożliwia deterministyczne kolejnością instrukcji zasad połączony za pośrednictwem element podstawowy. 

    <policies>
        <inbound>
            <cross-domain />
            <base />
            <find-and-replace from="xyz" to="abc" />
        </inbound>
    </policies>

W przykładzie definicji zasad powyżej `cross-domain` instrukcji wykonywany przed nastąpić wyższą zasad dotyczących, które z kolei, `find-and-replace` zasad.

Jeśli te same zasady pojawi się dwa razy w deklaracji zasad, najbardziej ostatnio szacowane zasady są stosowane. To umożliwia zastępowanie zasady, które zostały zdefiniowane na wyższym zakresie. Aby wyświetlić zasady w bieżącym zakresie w edytorze zasad, kliknij przycisk **Oblicz ponownie skutecznych zasad dla zaznaczonego zakresu**.

Zauważyć, że globalne zasady znajduje nie zasad nadrzędnej i za pomocą `<base>` elementu w nim jest ignorowany. 

## <a name="next-steps"></a>Następne kroki

Zapoznaj się z po wyrażeń zasad wideo.

> [AZURE.VIDEO policy-expressions-in-azure-api-management]

[Odwołanie do zasad]: api-management-policy-reference.md
[Produktu]: api-management-howto-add-products.md
[INTERFEJS API]: api-management-howto-add-products.md#add-apis 
[Operacja]: api-management-howto-add-operations.md

[Zasady zaawansowane]: https://msdn.microsoft.com/library/azure/dn894085.aspx
[Przepływ sterowania]: https://msdn.microsoft.com/library/azure/dn894085.aspx#choose
[Ustaw zmienną]: https://msdn.microsoft.com/library/azure/dn894085.aspx#set_variable
[Zasady wyrażeń]: https://msdn.microsoft.com/library/azure/dn910913.aspx

[policies-menu]: ./media/api-management-howto-policies/api-management-policies-menu.png
[policies-editor]: ./media/api-management-howto-policies/api-management-policies-editor.png
[policies-scope]: ./media/api-management-howto-policies/api-management-policies-scope.png
[policies-configure]: ./media/api-management-howto-policies/api-management-policies-configure.png
[policies-edit]: ./media/api-management-howto-policies/api-management-policies-edit.png
[policies-restrict]: ./media/api-management-howto-policies/api-management-policies-restrict.png
[policies-save]: ./media/api-management-howto-policies/api-management-policies-save.png
