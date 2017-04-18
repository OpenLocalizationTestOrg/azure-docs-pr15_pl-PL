<properties
    pageTitle="Ochrona z interfejsu API za pomocą usługi Zarządzanie Azure interfejsu API | Microsoft Azure"
    description="Dowiedz się, jak chronić z interfejsu API z przydziałów i ograniczania zasady (Ograniczanie szybkości)."
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

# <a name="protect-your-api-with-rate-limits-using-azure-api-management"></a>Ochrona z interfejsu API z granicami stawka za pomocą usługi zarządzania interfejsu API Azure

Ten przewodnik zawiera, jakie łatwe jest dodawanie ochrony do wewnętrznej bazy danych interfejsu API przez skonfigurowanie zasady limit i przydział stawka za pomocą usługi Zarządzanie interfejsu API Azure.

W tym samouczku utworzysz produktu "Bezpłatną wersję próbną" interfejs API, który umożliwia programistom dzwonić maksymalnie 10 na minutę i maksymalnie 200 połączeń tygodniowo połączenie z interfejsem API przy użyciu [Ogranicz szybkość połączenia na subskrypcję](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) i zasad [Przydział użycia zestawu na subskrypcję](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota) . Zostanie następnie opublikować API i przetestować zasady limitu stawka.

Dla bardziej zaawansowanych scenariuszy ograniczania, korzystając z zasad [stopa limit przez klucz](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) i [przydziału według klucza](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) zobacz [Zaawansowane żądanie ograniczania za pomocą usługi Zarządzanie interfejsu API Azure](api-management-sample-flexible-throttling.md).

## <a name="create-product"> </a>Tworzenie produktu

W tym kroku utworzysz produktu bezpłatnej wersji próbnej, który nie jest wymagane zatwierdzanie subskrypcji.

>[AZURE.NOTE] Masz już skonfigurowane produktu oraz ma zostać użyty do tego samouczka, można przejść do [Konfigurowanie połączeń stopa zasady limit i przydziału][] i postępuj zgodnie samouczek stamtąd przy użyciu produktu zamiast produktu bezpłatnej wersji próbnej.

Aby rozpocząć pracę, kliknij przycisk **Zarządzaj** w klasycznym Azure dotyczące usługi zarządzania interfejsu API. Powoduje przejście do portalu zarządzania interfejsu API programu publisher.

![Portal programu Publisher][api-management-management-console]

>Jeśli wystąpienie usługi zarządzania interfejsu API nie została jeszcze utworzona, zobacz [Tworzenie wystąpienie usługi zarządzania interfejsu API][] w samouczku [Zarządzanie do pierwszego interfejsu API zarządzania interfejsu API Azure w][] .

Kliknij pozycję **produkty** w menu **Zarządzania interfejsu API** po lewej stronie, aby wyświetlić stronę **produktów** .

![Dodawanie produktu][api-management-add-product]

Kliknij przycisk **Dodaj produktu** , aby wyświetlić okno dialogowe **Dodawanie nowego produktu** .

![Dodawanie nowego produktu][api-management-new-product-window]

W polu **Tytuł** wpisz **Bezpłatnej wersji próbnej**.

W polu **Opis** wpisz następujący tekst:  **subskrybenci będzie można uruchomić 10 minutę połączenia maksymalnie 200 połączeń/tydzień, po upływie którego odmowa dostępu.**

Produkty w zarządzaniu interfejsu API mogą być chronione lub Otwórz. Chroniony produktów musisz mieć subskrypcję przed mogą być używane. Otwieranie produktów może być używany bez subskrypcji. Upewnij się, że **wymaga subskrypcji** jest zaznaczona opcja do tworzenia chronionych produktów, która wymaga subskrypcji. Jest to ustawienie domyślne.

Jeśli chcesz, aby administrator, aby przejrzeć i zaakceptować lub odrzucić subskrypcji próby ten produkt, zaznacz opcję **Wymagaj zatwierdzania subskrypcji**. Jeśli pole wyboru nie jest zaznaczone, prób subskrypcji będzie zatwierdzony automatyczne. Subskrypcje automatycznie są zatwierdzone w tym przykładzie nie należy zaznaczać pola.

Aby umożliwić kont Deweloper subskrybować wielokrotnie nowy produkt, zaznacz pole wyboru **Zezwalaj na wiele subskrypcji jednoczesnego** . Ten samouczek nie wykorzystanie wiele subskrypcji jednoczesnego, aby pozostawić niezaznaczone.

Po wprowadzeniu wszystkich wartości kliknij pozycję **Zapisz** , aby utworzyć produktu.

![Produkt dodawany][api-management-product-added]

Domyślnie nowe produkty są widoczne dla użytkowników z grupy **administratorów** . Firma Microsoft będą dodawane grupy **projektantów** . Kliknij pozycję **Bezpłatnej wersji próbnej**, a następnie kliknij kartę **widoczność** .

>W obszarze Zarządzanie interfejsu API grupy są używane do zarządzania widocznością produktów dla deweloperów. Produkty Udziel widoczność do grup i deweloperów można wyświetlać i subskrybowanie produktów, które są widoczne dla grup, w których należą. Aby uzyskać więcej informacji zobacz [jak tworzenie i używanie grup w zarządzaniu interfejsu API Azure][].

![Dodawanie grupy deweloperów][api-management-add-developers-group]

Zaznacz pole wyboru **deweloperów** , a następnie kliknij przycisk **Zapisz**.

## <a name="add-api"> </a>Dodawanie interfejsu API do produktu

W tym kroku samouczka interfejsu API Echo będą dodawane do nowego produktu bezpłatnej wersji próbnej.

>Każde wystąpienie usługi zarządzania interfejsu API jest wstępnie skonfigurowana z interfejs API Echo, który może służyć do eksperymentować i informacje o zarządzaniu interfejsu API. Aby uzyskać więcej informacji zobacz [Zarządzanie do pierwszego interfejsu API w zarządzaniu interfejsu API Azure][].

Kliknij pozycję **produkty** z menu **Zarządzania interfejsu API** po lewej stronie, a następnie kliknij **Bezpłatna wersja próbna** , aby skonfigurować produktu.

![Konfigurowanie produktu][api-management-configure-product]

Kliknij przycisk **Dodaj interfejsu API do produktu**.

![Dodawanie interfejsu API do produktu][api-management-add-api]

Zaznacz **Interfejs API Echo**, a następnie kliknij przycisk **Zapisz**.

![Dodawanie interfejsu API Echo][api-management-add-echo-api]

## <a name="policies"> </a>Do konfigurowania zasad limit i stanie przydziału szybkość połączenia

Limity szybkości i przydziałów są skonfigurowane w edytorze zasad. W obszarze **Zarządzanie interfejsu API** menu po lewej stronie kliknij pozycję **zasady** . Na liście **produktów** kliknij **Bezpłatnej wersji próbnej**.

![Zasady produktu][api-management-product-policy]

Kliknij polecenie **Dodaj zasady** Importowanie szablonu zasad i rozpocząć tworzenie stopa zasady limit i przydziału.

![Dodawanie zasad][api-management-add-policy]

Aby wstawić zasady, umieść kursor w sekcji albo **przychodzących** i **wychodzących** szablonu zasad. Stopa limit i stanie przydziału zasady są zasady ruchu przychodzącego, tak ustaw kursor w odpowiednim elemencie przychodzących.

![Edytor zasad][api-management-policy-editor-inbound]

Dwie zasady, które dodajesz możemy w tym samouczku są zasady [Ogranicz szybkość połączenia na subskrypcję](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) i [Ustaw przydział użycia na subskrypcję](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota) .

![Zasady][api-management-limit-policies]

Gdy kursor zostanie umieszczony w elemencie zasad **ruchu przychodzącego** , kliknij strzałkę obok **Ogranicz szybkość połączenia na subskrypcję** Wstawianie szablonu zasad.

    <rate-limit calls="number" renewal-period="seconds">
    <api name="name" calls="number">
    <operation name="name" calls="number" />
    </api>
    </rate-limit>

**Ogranicz szybkość połączenia na subskrypcję** mogą być używane w poziomie produktu i można również na poziomie nazwy poszczególnych działań i interfejsu API. W tym samouczku używane są tylko poziom produktu zasady, więc usunąć elementy **interfejsu api** i **operacji** z elemencie **limit szybkości** tylko **limit szybkości** pozostają elementu, jak pokazano w poniższym przykładzie.

    <rate-limit calls="number" renewal-period="seconds">
    </rate-limit>

W produkcie bezpłatną wersję próbną stopa maksymalny dopuszczalne połączenia jest 10 połączeń minutę, dlatego wpisz **10** jako wartość atrybutu **połączeń** i **60** atrybutu **okres odnawiania** .

    <rate-limit calls="10" renewal-period="60">
    </rate-limit>

Aby skonfigurować zasady **Ustawianie przydział użycia na subskrypcję** , umieść kursor bezpośrednio pod element nowo dodany **limit szybkości** w elemencie **ruchu przychodzącego** , a następnie kliknij strzałkę znajdującą się po lewej stronie **Ustawianie przydział użycia na subskrypcję**.

    <quota calls="number" bandwidth="kilobytes" renewal-period="seconds">
    <api name="name" calls="number" bandwidth="kilobytes">
    <operation name="name" calls="number" bandwidth="kilobytes" />
    </api>
    </quota>

Ponieważ tych zasad również mają być na poziomie produktu, usuń elementy nazwiska i **działanie** **interfejsu api** , jak pokazano w poniższym przykładzie.

    <quota calls="number" bandwidth="kilobytes" renewal-period="seconds">
    </quota>

Przydziałów mogą być oparte na liczbie wywołań na interwału i przepustowość. W tym samouczku firma Microsoft nie są ograniczanie przepustowości na podstawie, aby usunąć atrybut **przepustowości** .

    <quota calls="number" renewal-period="seconds">
    </quota>

W produkcie bezpłatną wersję próbną przydziału jest 200 połączeń tygodniu. Określ **200** jako wartość atrybutu **połączeń** , a następnie określ **604800** jako wartość atrybutu **okres odnawiania** .

    <quota calls="200" renewal-period="604800">
    </quota>

>Interwałów zasady są określane w sekundach. Aby obliczyć interwał dla tygodnia, można pomnożyć liczbę dni (7) przez liczbę godzin w dniu (24) przez liczbę minut w godzina (60) przez liczbę sekund minuty (60): 7 *24* 60 * 60 = 604800.

Po zakończeniu konfigurowania zasady powinny być takie same poniższym przykładzie.

    <policies>
        <inbound>
            <rate-limit calls="10" renewal-period="60">
            </rate-limit>
            <quota calls="200" renewal-period="604800">
            </quota>
            <base />

    </inbound>
    <outbound>

        <base />

        </outbound>
    </policies>

Po odpowiednim zasady są skonfigurowane, kliknij przycisk **Zapisz**.

![Zapisywanie zasad][api-management-policy-save]

## <a name="publish-product"></a> Do opublikowania produktu

Teraz, gdy są dodawane za pośrednictwem interfejsów API i zasady są skonfigurowane, musi być opublikowany produktu, tak aby mogą być używane przez deweloperów. Kliknij pozycję **produkty** z menu **Zarządzania interfejsu API** po lewej stronie, a następnie kliknij **Bezpłatna wersja próbna** , aby skonfigurować produktu.

![Konfigurowanie produktu][api-management-configure-product]

Kliknij przycisk **Publikuj**, a następnie kliknij **opublikować go tak,** aby potwierdzić.

![Publikowanie produktu][api-management-publish-product]

## <a name="subscribe-account"> </a>Aby zasubskrybować konto Deweloper produktu

Teraz, gdy produkt jest opublikowany, jest dostępna dla subskrypcji i używane przez deweloperów.

>Administratorzy wystąpienia interfejsu API zarządzania automatycznie subskrybuje każdego produktu. W tym kroku samouczka możemy spowoduje subskrypcji jednego z kont Deweloper niebędący administratorami produktu bezpłatnej wersji próbnej. Jeśli Twojego konta dewelopera jest częścią roli Administratorzy, następnie można wykonać wraz z tego kroku, nawet jeśli Subskrybujesz już.

Z menu **Zarządzanie interfejsu API** po lewej stronie kliknij pozycję **Użytkownicy** , a następnie kliknij nazwę swojego konta Deweloper. W tym przykładzie użyto konta dewelopera **Borowik Gragg** .

![Konfigurowanie Deweloper][api-management-configure-developer]

Kliknij przycisk **Dodaj subskrypcji**.

![Dodawanie subskrypcji][api-management-add-subscription-menu]

Zaznacz **Bezpłatnej wersji próbnej**, a następnie kliknij przycisk **Subskrybuj**.

![Dodawanie subskrypcji][api-management-add-subscription]

>[AZURE.NOTE] W tym samouczku wiele subskrypcji jednoczesnego nie są włączone dla produktu bezpłatnej wersji próbnej. Gdyby, można będzie się monit o podanie nazwy subskrypcji, jak pokazano w poniższym przykładzie.

![Dodawanie subskrypcji][api-management-add-subscription-multiple]

Po kliknięciu przycisku **Subskrybuj**, produktu zostanie wyświetlona na liście **subskrypcji** dla użytkownika.

![Dodane subskrypcji][api-management-subscription-added]

## <a name="test-rate-limit"> </a>Połączeń operacji i przetestować limit szybkości

Teraz, gdy produkt bezpłatną wersję próbną jest skonfigurowany i opublikowanych, możemy połączenie niektóre operacje i przetestować zasady limitu stawka.
Przełącz się do portalu Deweloper, klikając pozycję **dzięki portalowi deweloperów** w prawym górnym menu.

![Dzięki portalowi deweloperów][api-management-developer-portal-menu]

Kliknij pozycję **interfejsy API** w górnym menu, a następnie kliknij **Interfejsu API Echo**.

![Dzięki portalowi deweloperów][api-management-developer-portal-api-menu]

Kliknij pozycję **Pobierz zasób**, a następnie kliknij **Spróbuj go**.

![Otwieranie konsoli][api-management-open-console]

Pozostaw domyślne wartości parametrów, a następnie wybierz subskrypcję klucz produktu bezpłatnej wersji próbnej.

![Klucz subskrypcji][api-management-select-key]

>[AZURE.NOTE] Jeśli masz wiele subskrypcji, należy wybrać klucz **Bezpłatnej**wersji próbnej, czyli zasady, które zostały skonfigurowane w poprzednich krokach będzie obowiązywać.

Kliknij przycisk **Wyślij**, a następnie Wyświetl odpowiedź. Uwaga **Stan odpowiedzi** **200 OK**.

![Wyniki operacji][api-management-http-get-results]

Kliknij przycisk **Wyślij** stawki większa niż stawka zasad limit 10 połączeń na minutę. Po przekroczeniu zasady limitu stawek, jest zwracany stan odpowiedzi **429 zbyt wiele** żądań.

![Wyniki operacji][api-management-http-get-429]

**Zawartość odpowiedź** wskazuje pozostała interwał przed ponowne próby zakończy się pomyślnie.

Kiedy obowiązuje stawka zasady limit 10 połączeń na minutę, wezwań zakończy się niepowodzeniem, przed upływem 60 sekund od pierwszego: 10 połączeń pomyślnego produktu przed Przekroczono limit szybkości. W tym przykładzie pozostała interwał jest 54 sekund.

## <a name="next-steps"> </a>Następne kroki

-   Obejrzyj pokaz ustawienia limity szybkości i przydziałów w poniższym klipie wideo.

> [AZURE.VIDEO rate-limits-and-quotas]


[api-management-management-console]: ./media/api-management-howto-product-with-rules/api-management-management-console.png
[api-management-add-product]: ./media/api-management-howto-product-with-rules/api-management-add-product.png
[api-management-new-product-window]: ./media/api-management-howto-product-with-rules/api-management-new-product-window.png
[api-management-product-added]: ./media/api-management-howto-product-with-rules/api-management-product-added.png
[api-management-add-policy]: ./media/api-management-howto-product-with-rules/api-management-add-policy.png
[api-management-policy-editor-inbound]: ./media/api-management-howto-product-with-rules/api-management-policy-editor-inbound.png
[api-management-limit-policies]: ./media/api-management-howto-product-with-rules/api-management-limit-policies.png
[api-management-policy-save]: ./media/api-management-howto-product-with-rules/api-management-policy-save.png
[api-management-configure-product]: ./media/api-management-howto-product-with-rules/api-management-configure-product.png
[api-management-add-api]: ./media/api-management-howto-product-with-rules/api-management-add-api.png
[api-management-add-echo-api]: ./media/api-management-howto-product-with-rules/api-management-add-echo-api.png
[api-management-developer-portal-menu]: ./media/api-management-howto-product-with-rules/api-management-developer-portal-menu.png
[api-management-publish-product]: ./media/api-management-howto-product-with-rules/api-management-publish-product.png
[api-management-configure-developer]: ./media/api-management-howto-product-with-rules/api-management-configure-developer.png
[api-management-add-subscription-menu]: ./media/api-management-howto-product-with-rules/api-management-add-subscription-menu.png
[api-management-add-subscription]: ./media/api-management-howto-product-with-rules/api-management-add-subscription.png
[api-management-developer-portal-api-menu]: ./media/api-management-howto-product-with-rules/api-management-developer-portal-api-menu.png
[api-management-open-console]: ./media/api-management-howto-product-with-rules/api-management-open-console.png
[api-management-http-get]: ./media/api-management-howto-product-with-rules/api-management-http-get.png
[api-management-http-get-results]: ./media/api-management-howto-product-with-rules/api-management-http-get-results.png
[api-management-http-get-429]: ./media/api-management-howto-product-with-rules/api-management-http-get-429.png
[api-management-product-policy]: ./media/api-management-howto-product-with-rules/api-management-product-policy.png
[api-management-add-developers-group]: ./media/api-management-howto-product-with-rules/api-management-add-developers-group.png
[api-management-select-key]: ./media/api-management-howto-product-with-rules/api-management-select-key.png
[api-management-subscription-added]: ./media/api-management-howto-product-with-rules/api-management-subscription-added.png
[api-management-add-subscription-multiple]: ./media/api-management-howto-product-with-rules/api-management-add-subscription-multiple.png

[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: ../api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Zarządzanie do pierwszego interfejsu API w zarządzaniu interfejsu API platformy Azure]: api-management-get-started.md
[Jak utworzyć i używanie grup w zarządzaniu interfejsu API platformy Azure]: api-management-howto-create-groups.md
[View subscribers to a product]: api-management-howto-add-products.md#view-subscribers
[Get started with Azure API Management]: api-management-get-started.md
[Tworzenie wystąpienia usługi zarządzania interfejsu API]: api-management-get-started.md#create-service-instance
[Next steps]: #next-steps

[Create a product]: #create-product
[Konfigurowanie zasad limit i stanie przydziału szybkość połączenia]: #policies
[Add an API to the product]: #add-api
[Publish the product]: #publish-product
[Subscribe a developer account to the product]: #subscribe-account
[Call an operation and test the rate limit]: #test-rate-limit

[Limit call rate]: https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate
[Set usage quota]: https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota
