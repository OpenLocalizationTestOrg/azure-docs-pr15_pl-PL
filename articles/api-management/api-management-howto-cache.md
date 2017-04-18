<properties
    pageTitle="Dodawanie pamięci podręcznej w celu zwiększenia wydajności w zarządzaniu interfejsu API Azure | Microsoft Azure"
    description="Dowiedz się, jak zwiększyć opóźnienie przepustowości i obciążenia usługi sieci web do zarządzania interfejsu API usługi połączeń."
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
    ms.topic="get-started-article"
    ms.date="10/25/2016"
    ms.author="sdanie"/>

# <a name="add-caching-to-improve-performance-in-azure-api-management"></a>Dodawanie pamięci podręcznej w celu zwiększenia wydajności w zarządzaniu interfejsu API platformy Azure

Operacje w zarządzaniu interfejsu API można skonfigurować w pamięci podręcznej odpowiedzi. Buforowanie odpowiedzi można znacznie zmniejszyć opóźnienie interfejsu API, wykorzystania przepustowości i obciążenia usługi dla danych, które nie są często zmieniane w sieci web.

Ten przewodnik pokazano, jak dodać buforowanie odpowiedzi dla swojego interfejsu API i skonfigurować zasady działania interfejsu API Echo próbki. Następnie możesz zadzwonić operacji z portalu Deweloper, aby zweryfikować pamięci podręcznej w działaniu.

>[AZURE.NOTE] Uzyskać informacji na temat buforowania elementy według klucza przy użyciu wyrażeń zasad zobacz [niestandardowe pamięci podręcznej w zarządzaniu interfejsu API Azure](api-management-sample-cache-by-key.md).

## <a name="prerequisites"></a>Wymagania wstępne

Przed wykonaniem kroków w tym przewodniku, musi mieć wystąpienie usługi zarządzania interfejsu API z interfejsem API i skonfigurowana produktu. Jeśli wystąpienie usługi zarządzania interfejsu API nie został jeszcze utworzony, zobacz [Tworzenie wystąpienie usługi zarządzania interfejsu API][] samouczka [Wprowadzenie do zarządzania interfejsu API Azure][] .

## <a name="configure-caching"> </a>Konfigurowanie operacji w pamięci podręcznej

W tym kroku należy przejrzeć ustawień pamięci podręcznej operacji **Uzyskiwanie zasobów (buforowane)** próbki interfejsu API Echo.

>[AZURE.NOTE] Każde wystąpienie usługi zarządzania interfejsu API zawiera wstępnie skonfigurowanych z interfejs API Echo, który może służyć do eksperymentować i informacje o zarządzaniu interfejsu API. Aby uzyskać więcej informacji zobacz [Wprowadzenie do zarządzania interfejsu API Azure][].

Aby rozpocząć pracę, kliknij przycisk **Zarządzaj** w portalu klasyczny Azure dotyczące usługi zarządzania interfejsu API. Powoduje przejście do portalu zarządzania interfejsu API programu publisher.

![Portal programu Publisher][api-management-management-console]

Kliknij pozycję **interfejsy API** w menu **Zarządzania interfejsu API** po lewej stronie, a następnie kliknij **Interfejsu API Echo**.

![Interfejs API echo][api-management-echo-api]

Kliknij kartę **operacji** , a następnie kliknij operację **Uzyskiwanie zasobów (buforowane)** z listy **operacji** .

![Operacje interfejsu API echo][api-management-echo-api-operations]

Kliknij kartę **buforowanie** , aby wyświetlić ustawień pamięci podręcznej dla tej operacji.

![Karta pamięci podręcznej][api-management-caching-tab]

Aby włączyć buforowanie dla operacji, zaznacz pole wyboru **Włącz** . W tym przykładzie buforowanie jest włączone.

Poszczególne odpowiedzi operacji kluczem jest kolumna, na podstawie wartości w polach **różne według parametrów ciągu kwerendy** i **różne nagłówki** . Jeśli chcesz pamięci podręcznej wiele odpowiedzi na podstawie parametrów ciągu kwerendy lub nagłówków, można je skonfigurować w tych dwóch pól.

**Czas trwania** Określa interwał wygaśnięcia pamięci podręcznej odpowiedzi. W tym przykładzie interwał jest **3600** sekund, co oznacza godzinę.

Przy użyciu konfiguracji buforowania w tym przykładzie, pierwsze żądanie operacji **Uzyskiwanie zasobów (buforowane)** zwraca odpowiedź z usługi wewnętrznej bazy danych. Odpowiedź będą buforowane, wprowadzić według określonej nagłówki i parametrów ciągu kwerendy. Kolejnych zaproszeń do działania z parametrami pasujące uzyskuje odpowiedź buforowana zwrócona, dopóki nie wygasł interwał czasu trwania pamięci podręcznej.

## <a name="caching-policies"> </a>Przejrzeć zasady przechowywania

W tym kroku zapoznanie ustawień pamięci podręcznej operacji **Uzyskiwanie zasobów (buforowane)** próbki interfejsu API Echo.

Gdy ustawień pamięci podręcznej zostały skonfigurowane dla operacji na karcie **buforowanie** , zasady przechowywania są dodawane do tej operacji. Te zasady można wyświetlać i edytować w edytorze zasad.

Kliknij polecenie **zasady** **Zarządzania interfejsu API** menu po lewej stronie, a następnie wybierz pozycję **interfejsu API Echo / pobieranie zasobów (buforowane)** z listy rozwijanej **Operacja** .

![Operacja z zakresem zasady][api-management-operation-dropdown]

W edytorze zasad zostaną wyświetlone zasad dla tej operacji.

![Edytor zasad zarządzania interfejsu API][api-management-policy-editor]

Definicja zasad dla tej operacji zawiera zasady definiujące buforowania konfiguracji, które zostały przejrzeć, korzystając z karty **buforowanie** w poprzednim kroku.

    <policies>
        <inbound>
            <base />
            <cache-lookup vary-by-developer="false" vary-by-developer-groups="false">
                <vary-by-header>Accept</vary-by-header>
                <vary-by-header>Accept-Charset</vary-by-header>
            </cache-lookup>
            <rewrite-uri template="/resource" />
        </inbound>
        <outbound>
            <base />
            <cache-store caching-mode="cache-on" duration="3600" />
        </outbound>
    </policies>

>[AZURE.NOTE] Zmiany wprowadzone w zasadach buforowania w edytorze zasad zostaną uwzględnione również na karcie **buforowanie** działania i na odwrót.

## <a name="test-operation"> </a>Połączeń operacji i przetestować buforowanie

Aby wyświetlić pamięci podręcznej w działaniu, możemy zadzwonić operacji z portalu Deweloper. Kliknij pozycję **dzięki portalowi deweloperów** w prawym górnym menu.

![Dzięki portalowi deweloperów][api-management-developer-portal-menu]

Kliknij pozycję **interfejsy API** w górnym menu, a następnie wybierz **Interfejs API Echo**.

![Interfejs API echo][api-management-apis-echo-api]

>Jeśli masz tylko jeden interfejs API skonfigurowane lub widoczny dla Twojego konta, klikając pozycję interfejsy API przejście bezpośrednio z operacji dla tego interfejsu API.

Wybierz operację **Uzyskiwanie zasobów (buforowane)** , a następnie kliknij **Otwartej konsoli**.

![Otwieranie konsoli][api-management-open-console]

Konsola umożliwia wywołania operacji bezpośrednio z poziomu portalu Deweloper.

![Konsoli][api-management-console]

Pozostaw domyślne wartości **parametr1** i **parametr2**.

Wybierz inny klawisz z listy rozwijanej **klucza subskrypcji** . Jeśli Twoje konto zawiera tylko jedną subskrypcję, już być zaznaczone.

Wprowadź **sampleheader:value1** w polu tekstowym **żądanie nagłówków** .

Kliknij pozycję **HTTP Get** i zanotuj nagłówków odpowiedzi.

Wprowadź **sampleheader:value2** w polu tekstowym **żądanie nagłówki** , a następnie kliknij **HTTP Get**.

Zauważ, że wartość **sampleheader** jest nadal **wartość1** w odpowiedzi. Spróbuj wykonać kilka różnych wartości i zanotuj, zwracany jest odpowiedź buforowana z pierwszego połączenia.

Wprowadzanie **25** w polu **parametr2** , a następnie kliknij **HTTP Get**.

Należy zauważyć, że wartość **sampleheader** w odpowiedzi jest teraz **wartość2**. Ponieważ wyniki operacji są wprowadzić w ciągu kwerendy, nie została zwrócona poprzedniego odpowiedź buforowana.

## <a name="next-steps"> </a>Następne kroki

-   Aby uzyskać więcej informacji o zasadach buforowania zobacz [pamięci podręcznej zasad][] [odwołanie do zasad zarządzania interfejsu API][].
-   Uzyskać informacji na temat buforowania elementy według klucza przy użyciu wyrażeń zasad zobacz [niestandardowe pamięci podręcznej w zarządzaniu interfejsu API Azure](api-management-sample-cache-by-key.md).

[api-management-management-console]: ./media/api-management-howto-cache/api-management-management-console.png
[api-management-echo-api]: ./media/api-management-howto-cache/api-management-echo-api.png
[api-management-echo-api-operations]: ./media/api-management-howto-cache/api-management-echo-api-operations.png
[api-management-caching-tab]: ./media/api-management-howto-cache/api-management-caching-tab.png
[api-management-operation-dropdown]: ./media/api-management-howto-cache/api-management-operation-dropdown.png
[api-management-policy-editor]: ./media/api-management-howto-cache/api-management-policy-editor.png
[api-management-developer-portal-menu]: ./media/api-management-howto-cache/api-management-developer-portal-menu.png
[api-management-apis-echo-api]: ./media/api-management-howto-cache/api-management-apis-echo-api.png
[api-management-open-console]: ./media/api-management-howto-cache/api-management-open-console.png
[api-management-console]: ./media/api-management-howto-cache/api-management-console.png


[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Wprowadzenie do zarządzania interfejsu API platformy Azure]: api-management-get-started.md

[Odwołanie do zasad zarządzania interfejsu API]: https://msdn.microsoft.com/library/azure/dn894081.aspx
[Zasady przechowywania]: https://msdn.microsoft.com/library/azure/dn894086.aspx

[Tworzenie wystąpienia usługi zarządzania interfejsu API]: api-management-get-started.md#create-service-instance

[Configure an operation for caching]: #configure-caching
[Review the caching policies]: #caching-policies
[Call an operation and test the caching]: #test-operation
[Next steps]: #next-steps
