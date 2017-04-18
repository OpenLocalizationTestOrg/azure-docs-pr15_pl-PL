<properties 
    pageTitle="Jak dodać operacje do interfejsu API w zarządzaniu interfejsu API Azure | Microsoft Azure" 
    description="Dowiedz się, jak dodać operacje interfejsu API w zarządzaniu interfejsu API Azure." 
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

# <a name="how-to-add-operations-to-an-api-in-azure-api-management"></a>Jak dodać operacje do interfejsu API w zarządzaniu interfejsu API platformy Azure

Przed użyciem interfejs API interfejsu API zarządzania w operacji musi zostać dodana. Ten przewodnik pokazano, jak dodać i skonfigurować różnego rodzaju operacji API w zarządzaniu interfejsu API.

## <a name="add-operation"> </a>Operacji dodawania

Operacje są dodawane i skonfigurowany do interfejsu API w portalu programu publisher. Aby uzyskać dostęp do portalu programu publisher, kliknij przycisk **Zarządzaj** w portalu klasyczny Azure dotyczące usługi zarządzania interfejsu API.

![Portal programu Publisher][api-management-management-console]

>Jeśli wystąpienie usługi zarządzania interfejsu API nie został jeszcze utworzony, zobacz [Tworzenie wystąpienie usługi zarządzania interfejsu API][] samouczka [Wprowadzenie do zarządzania interfejsu API Azure][] .

Wybierz żądany interfejs API w portalu programu publisher, a następnie wybierz kartę **operacji** . 

![Operacje][api-management-operations]

Kliknij przycisk **Dodaj operacji** dodawania nowej operacji. Zostanie wyświetlona **Nowa operacja** i na karcie **Podpis** zostanie wybrana domyślnie.

![Dodawanie operacji][api-management-add-operation]

Określanie **Zlecenie HTTP** przez wybranie z listy rozwijanej.

![Metoda HTTP][api-management-http-method]

<a name="url-template"></a>

Definiowanie szablonu adresu URL przez wpisanie tekstu w fragment adresu URL, składający się z jednego lub więcej segmenty ścieżki adresu URL i zero lub więcej parametrów ciągu kwerendy. Adres URL szablonu, dołączone do podstawowego adresu URL interfejsu API, identyfikuje jednej operacji HTTP. Może zawierać jedną lub więcej o nazwie zmiennej części, które są oznaczane nawiasy klamrowe. Następujące segmenty zmiennej są nazywane parametrów szablonu i są przypisywane dynamicznie wartości wyodrębnionych z adresu URL żądania, gdy wniosek jest przetwarzana przez platformę interfejsu API zarządzania.

![Adres URL szablonu][api-management-url-template]

<a name="rewrite-url-template"></a>

W razie potrzeby określ **edycji adres URL szablonu**. Pozwala na szablon standardowy adres URL dla przetwarzania przychodzących wezwań na zewnętrzną, podczas nawiązywania połączeń z wewnętrzną za pomocą przekonwertowane adresu URL zgodnie z szablonem zmiana. Parametry szablonu w szablonie adresu URL stosuje się w szablonie zmiana. W poniższym przykładzie pokazano, jak zawartości typu kodowane jako segmentu ścieżki usługi sieci web z poprzedniego przykładu może być udostępniana jako parametru zapytania w interfejsie API opublikowany za pośrednictwem platformy interfejsu API zarządzania przy użyciu szablonów adresu URL.

![Zmiana szablonu adresu URL][api-management-url-template-rewrite]

Osób dzwoniących do operacji zależy od formatu `/customers?customerid=ALFKI` i to zostaną zamapowane `/Customers('ALFKI')` podczas wywoływania usług wewnętrznej.


**Wyświetlaną** nazwę i **Opis** Podaj opis działania i służą do tworzenia dokumentacji deweloperów przy użyciu tego interfejsu API w portalu Deweloper.

![Opis][api-management-description]

Opis działania można określić jako zwykły tekst lub HTML w polu tekstowym **Opis** .

## <a name="operation-caching"> </a>Operacji pamięci podręcznej

Buforowanie odpowiedzi zmniejsza opóźnienie traktowany interfejsu API konsumentów, obniża przepustowości i zmniejszenia obciążenia HTTP sieci web usługi implementacji API. 

Aby szybko i łatwo buforowanie operacji, wybierz kartę **buforowanie** , a następnie zaznacz pole wyboru **Włącz** .

![Pamięci podręcznej][api-management-caching-tab]

**Czas trwania** Określa przedział czasu, w którym odpowiedzi operacji pozostaje w pamięci podręcznej. Wartość domyślna to 3600 sekund lub 1 godzina.

Klucze pamięci podręcznej są używane do rozróżniania odpowiedzi tak, aby odpowiedzi odpowiadające każdej klucz różnych pamięci podręcznej otrzymają własnej osobnych wartość buforowana. Opcjonalnie wprowadź parametry ciągu kwerendy określonych i/lub nagłówki HTTP może być używany w przypadku komputerów odpowiednio wartości klucza pamięci podręcznej w polach tekstowych **różne według parametrów ciągu kwerendy** i **różne nagłówki** . W Brak żądania określonego, pełny adres URL, a następujące wartości nagłówka HTTP są używane w pamięci podręcznej generowania kluczy: **Zaakceptuj** i **Zaakceptuj znaków**.

>Aby uzyskać więcej informacji w pamięci podręcznej i buforowanie zasady zobacz [jak wyniki operacji pamięci podręcznej w zarządzaniu interfejsu API Azure][].


## <a name="request-parameters"> </a>Parametrów żądania

Parametry operacji są zarządzane na karcie Parametry. Parametry określone w **Szablonie adres URL** na karcie **Podpis** są automatycznie dodawane i mogą być zmieniane tylko przez edytowanie szablonu adresu URL. Dodatkowe parametry można wprowadzić ręcznie.

Aby dodać nowe parametry kwerendy, kliknij pozycję **Dodaj parametry kwerendy** i wprowadź następujące informacje:

-   **Nazwa** - Nazwa parametru.
-   **Opis** — krótki opis parametru (opcjonalnie).
-   **Typ** — typ parametru zaznaczony na liście w dół.
-   **Wartości** - wartości, które można przypisać do tego parametru. Jedną z wartości może zostać oznaczony jako domyślne (opcjonalnie).
-   **Wymagane** — że parametr jest obowiązkowe, zaznaczając pole wyboru. 

![Parametry żądania][api-management-request-parameters]

## <a name="request-body"> </a>Treści żądania

Jeśli zezwala operacji (położenie, WPIS) i wymaga podmiot może dostarczyć przykład go we wszystkich formatach obsługiwanych reprezentacja (np. json, XML). 

>Treść żądania jest używany tylko do celów dokumentacji i nie jest sprawdzana poprawność.

Aby wprowadzić treści żądania, przejdź na kartę **treści** .

Kliknij pozycję **Dodaj reprezentacja**, zacznij wpisywać nazwę odpowiedniego typu zawartości (np. aplikacji i json), wybierz go z listy rozwijanej i wklejanie w przykładzie treści wezwania na odpowiednie w wybranym formacie w polu tekstowym. 

![Treść żądania][api-management-request-body]

W dodatkowe reprezentacji, można również określić opcjonalny opis w polu tekstowym **Opis** .

## <a name="responses"> </a>Odpowiedzi

Jest dobrym rozwiązaniem jest zawierają przykłady odpowiedzi dla wszystkich kodów stanu, które mogą mieć tej operacji. Każdy kodu stanu może mieć więcej niż jedną odpowiedź treści przykład jednej dla każdego z obsługiwanych typów zawartości. 

Aby dodać odpowiedź, kliknij przycisk **Dodaj** , a następnie zacznij pisać kod żądany stan. W tym przykładzie jest kodu stanu **200 OK**. Gdy kod jest wyświetlany na liście rozwijanej, zaznacz go i kod odpowiedzi zostanie utworzone i dodane do operacji.

![Kod odpowiedzi][api-management-response-code]

Kliknij pozycję **Dodaj reprezentacja**, zacznij wpisywać nazwę odpowiedniego typu zawartości (np. aplikacji i json), a następnie zaznacz go na liście w dół.

![Typ zawartości treści][api-management-response-body-content-type]

Wklej przykład treści odpowiedzi w wybranym formacie, w polu tekstowym. 

![Treść odpowiedzi][api-management-response-body]

W razie potrzeby dodaj opcjonalny opis w polu tekstowym **Opis** .

Po skonfigurowaniu operacji, kliknij przycisk **Zapisz**.


## <a name="next-steps"> </a>Następne kroki

Po operacji są dodawane do interfejsu API, następnym krokiem jest skojarzyć API produktu w celu opublikowania go tak, aby mogą być wywoływane jej działalności.

-   [Jak tworzyć i publikować produktu][]

[api-management-management-console]: ./media/api-management-howto-add-operations/api-management-management-console.png
[api-management-operations]: ./media/api-management-howto-add-operations/api-management-operations.png
[api-management-add-operation]: ./media/api-management-howto-add-operations/api-management-add-operation.png
[api-management-http-method]: ./media/api-management-howto-add-operations/api-management-http-method.png
[api-management-url-template]: ./media/api-management-howto-add-operations/api-management-url-template.png
[api-management-url-template-rewrite]: ./media/api-management-howto-add-operations/api-management-url-template-rewrite.png
[api-management-description]: ./media/api-management-howto-add-operations/api-management-description.png
[api-management-caching-tab]: ./media/api-management-howto-add-operations/api-management-caching-tab.png
[api-management-request-parameters]: ./media/api-management-howto-add-operations/api-management-request-parameters.png
[api-management-request-body]: ./media/api-management-howto-add-operations/api-management-request-body.png
[api-management-response-code]: ./media/api-management-howto-add-operations/api-management-response-code.png
[api-management-response-body-content-type]: ./media/api-management-howto-add-operations/api-management-response-body-content-type.png
[api-management-response-body]: ./media/api-management-howto-add-operations/api-management-response-body.png


[api-management-contoso-api]: ./media/api-management-howto-add-operations/api-management-contoso-api.png

[api-management-add-new-api]: ./media/api-management-howto-add-operations/api-management-add-new-api.png
[api-management-api-settings]: ./media/api-management-howto-add-operations/api-management-api-settings.png
[api-management-api-settings-credentials]: ./media/api-management-howto-add-operations/api-management-api-settings-credentials.png
[api-management-api-summary]: ./media/api-management-howto-add-operations/api-management-api-summary.png
[api-management-echo-operations]: ./media/api-management-howto-add-operations/api-management-echo-operations.png

[Add an operation]: #add-operation
[Operation caching]: #operation-caching
[Request parameters]: #request-parameters
[Request body]: #request-body
[Responses]: #responses
[Next steps]: #next-steps

[Wprowadzenie do zarządzania interfejsu API platformy Azure]: api-management-get-started.md
[Tworzenie wystąpienia usługi zarządzania interfejsu API]: api-management-get-started.md#create-service-instance

[How to add operations to an API]: api-management-howto-add-operations.md
[Jak tworzyć i publikować produktu]: api-management-howto-add-products.md
[Jak wyniki operacji pamięci podręcznej w zarządzaniu interfejsu API platformy Azure]: api-management-howto-cache.md