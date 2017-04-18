<properties 
    pageTitle="Jak dostosować portalu Deweloper Azure interfejsu API zarządzania przy użyciu szablonów | Microsoft Azure" 
    description="Dowiedz się, jak dostosować portalu Deweloper Azure interfejsu API zarządzania przy użyciu szablonów." 
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


# <a name="how-to-customize-the-azure-api-management-developer-portal-using-templates"></a>Jak dostosować portalu Deweloper Azure interfejsu API zarządzania przy użyciu szablonów

Azure zarządzania interfejsu API udostępnia kilka funkcji dostosowywania Administratorzy mogą [dostosować wygląd i działanie dzięki portalowi deweloperów](api-management-customize-portal.md), jak dostosować zawartość stron portalu Deweloper, używając zestaw szablonów, które skonfigurować zawartość strony. Za pomocą składni [DotLiquid](http://dotliquidmarkup.org/) i dostarczonych zestawu zasoby zlokalizowanych ciągów, ikony i formanty strony, masz elastyczność, aby skonfigurować zawartości stron odpowiednio do potrzeb przy użyciu tych szablonów.

## <a name="developer-portal-templates-overview"></a>Omówienie szablonów portalu — Deweloper

Szablony portalu Deweloper zarządza się w portalu Deweloper przez administratorów wystąpienia zarządzania interfejsu API usługi. Zarządzanie szablonami Deweloper, przejdź do zarządzania interfejsu API wystąpienia usługi w portalu klasyczny Azure, a następnie kliknij przycisk **Przeglądaj**.

![Dzięki portalowi deweloperów][api-management-browse]

Jeśli jesteś już w portalu programu publisher, masz dostęp do portalu Deweloper, klikając pozycję **dzięki portalowi deweloperów**.

![Menu portalu — Deweloper][api-management-developer-portal-menu]

Aby uzyskać dostęp do szablonów portalu Deweloper, kliknij ikonę Dostosuj po lewej stronie, aby wyświetlić menu dostosowywania, a następnie kliknij przycisk **Szablony**.

![Szablony portalu — Deweloper][api-management-customize-menu]

Lista szablonów zawiera kilka kategorii szablonów obejmujących różnych stronach w portalu — Deweloper. Każdy szablon jest inny, ale kroki w celu ich edytowania i opublikować zmiany są takie same. Aby edytować szablon, kliknij nazwę szablonu.

![Szablony portalu — Deweloper][api-management-templates-menu]

Kliknij szablon umożliwia przejście do strony portalu Deweloper, który można dostosować za pomocą tego szablonu. Szablon zostanie wyświetlony w tym przykładzie **listy produktów** . Szablon **listy produktów** określa obszar ekranu wskazywana przez czerwony prostokąt. 

![Szablon listy produktów][api-management-developer-portal-templates-overview]

Niektóre szablony, takie jak szablony **Profilu użytkownika** , Dostosuj różne części tej samej stronie. 

![Szablony profilu użytkownika][api-management-user-profile-templates]

Edytor dla każdego szablonu portalu Deweloper ma dwie sekcje wyświetlane u dołu strony. Po lewej stronie umożliwia wyświetlenie okienka edytowania szablonu, a po prawej stronie zawiera modelu danych dla szablonu. 

Szablon edytowania okienko zawiera adiustacji, który steruje wygląd i działanie odpowiedniej strony w portalu Deweloper. Znaczniki w szablonie używa składni [DotLiquid](http://dotliquidmarkup.org/) . Jeden popularne edytorem DotLiquid jest [DotLiquid dla projektantów](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers). Zmiany wprowadzone w szablonie podczas edytowania są wyświetlane w czasie rzeczywistym w przeglądarce, ale nie są widoczne dla klientów poczekaj, aż zostanie [Zapisywanie](#to-save-a-template) i [Publikowanie](#to-publish-a-template) szablonu.

![Znaczniki szablonu][api-management-template]

W okienku **dane szablonu** zawiera prowadnicę do modelu danych dla obiektów, które są dostępne dla określonego szablonu. Ten przewodnik zawiera, wyświetlając dane dynamiczne, które obecnie są wyświetlane w portalu Deweloper. Możesz rozwinąć okienka szablon, klikając pozycję prostokąt w prawym górnym rogu okienka **danych szablonu** .

![Szablon modelu danych][api-management-template-data]

W poprzednim przykładzie są wyświetlane w portalu Deweloper dwóch produktów, które zostały pobrane z danych wyświetlanych w okienku **dane szablonu** , jak pokazano w poniższym przykładzie.

    {
        "Paging": {
            "Page": 1,
            "PageSize": 10,
            "TotalItemCount": 2,
            "ShowAll": false,
            "PageCount": 1
        },
        "Filtering": {
            "Pattern": null,
            "Placeholder": "Search products"
        },
        "Products": [
            {
                "Id": "56ec64c380ed850042060001",
                "Title": "Starter",
                "Description": "Subscribers will be able to run 5 calls/minute up to a maximum of 100 calls/week.",
                "Terms": "",
                "ProductState": 1,
                "AllowMultipleSubscriptions": false,
                "MultipleSubscriptionsCount": 1
            },
            {
                "Id": "56ec64c380ed850042060002",
                "Title": "Unlimited",
                "Description": "Subscribers have completely unlimited access to the API. Administrator approval is required.",
                "Terms": null,
                "ProductState": 1,
                "AllowMultipleSubscriptions": false,
                "MultipleSubscriptionsCount": 1
            }
        ]
    }

Znaczniki w szablonie **listy produktów** przetwarza dane, których chcesz przekazać odpowiednie dane wyjściowe iteracji w kolekcji produktów, aby wyświetlić informacje i łącza do każdego pojedynczego produktu. Uwaga `<search-control>` i `<page-control>` elementy w adiustację. Te sterowania wyświetlaniem wyszukiwanie i stronicowania formantów na stronie. `ProductsStrings|PageTitleProducts`jest to odwołanie zlokalizowany ciąg zawierający `h2` tekstu nagłówka na stronie. Aby uzyskać listę parametrów zasobów, formanty strony i ikon dostępnych do wykorzystania w szablonach portalu Deweloper, zobacz [Szablony portalu zarządzania interfejsu API dewelopera](https://msdn.microsoft.com/library/azure/mt697540.aspx).

    <search-control></search-control>
    <div class="row">
        <div class="col-md-9">
            <h2>{% localized "ProductsStrings|PageTitleProducts" %}</h2>
        </div>
    </div>
    <div class="row">
        <div class="col-md-12">
        {% if products.size > 0 %}
        <ul class="list-unstyled">
        {% for product in products %}
            <li>
                <h3><a href="/products/{{product.id}}">{{product.title}}</a></h3>
                {{product.description}}
            </li>   
        {% endfor %}
        </ul>
        <paging-control></paging-control>
        {% else %}
        {% localized "CommonResources|NoItemsToDisplay" %}
        {% endif %}
        </div>
    </div>

## <a name="to-save-a-template"></a>Aby zapisać szablon

Aby zapisać szablon, kliknij pozycję Zapisz w edytorze szablonu.

![Zapisywanie szablonu][api-management-save-template]

Zapisane zmiany nie są aktywne w portalu Deweloper, dopóki nie zostaną opublikowane.

## <a name="to-publish-a-template"></a>Publikowanie szablonu

Szablony zapisane można opublikować pojedynczo, lub jednocześnie. Aby opublikować poszczególnych szablon, kliknij pozycję Publikuj w edytorze szablonu.

![Publikowanie szablonu][api-management-publish-template]

Kliknij przycisk **Tak,** aby potwierdzić i szablon żywo w portalu Deweloper.

![Upewnij się, publikowanie][api-management-publish-template-confirm]

Aby opublikować wszystkie wersje obecnie wycofanych szablonu, kliknij pozycję **Publikuj** na liście szablony. Szablony nieopublikowane są oznaczone gwiazdką po nazwie szablonu. W tym przykładzie są publikowane szablonów **listy produktów** i **produktów** .

![Publikowanie szablonów][api-management-publish-templates]

Kliknij przycisk **Publikuj dostosowania** o potwierdzenie.

![Upewnij się, publikowanie][api-management-publish-customizations]

Nowo opublikowanej szablony są skuteczne bezpośrednio w portalu Deweloper.

## <a name="to-revert-a-template-to-the-previous-version"></a>Aby powrócić do poprzedniej wersji szablonu

Aby przywrócić szablonu do poprzedniej wersji opublikowanych, kliknij pozycję zostaną przywrócone w edytorze szablonu.

![Przywracanie szablonu][api-management-revert-template]

Kliknij przycisk **Tak,** aby potwierdzić.

![Upewnij się][api-management-revert-template-confirm]

Opublikowanej wersji szablonu jest live w portalu Deweloper, po zakończeniu operacji przywracania.

## <a name="to-restore-a-template-to-the-default-version"></a>Aby przywrócić szablonu do wersji domyślnej

Przywracanie szablonów do ich wersji domyślnej jest procesem dwuetapowym. Najpierw należy przywrócić szablony, a następnie przywrócone wersji musi być opublikowany.

Aby przywrócić jednego szablonu do wersji domyślnej, kliknij przycisk Przywróć w edytorze szablonu.

![Przywracanie szablonu][api-management-reset-template]

Kliknij przycisk **Tak,** aby potwierdzić.

![Upewnij się][api-management-reset-template-confirm]

Aby przywrócić wszystkie szablony do ich wersji domyślne, kliknij pozycję **Przywróć domyślne szablony** na liście szablon.

![Przywracanie szablonów][api-management-restore-templates]

Przywrócenie szablony następnie należy opublikować pojedynczo lub wszystkie naraz, wykonując kroki opisane w sekcji [Publikowanie szablonu](#to-publish-a-template).

## <a name="developer-portal-templates-reference"></a>Dokumentacja dewelopera portalu szablonów

Aby uzyskać informacje dla deweloperów portalu szablonów, zasobów ciągu, ikony i kontrolki strony, zobacz [Szablony portalu zarządzania interfejsu API dewelopera](https://msdn.microsoft.com/library/azure/mt697540.aspx).

## <a name="watch-a-video-overview"></a>Obejrzyj film wideo

W poniższym klipie wideo przedstawiono, jak dodać tablicy dyskusyjnej i oceny do stron i działanie interfejsu API w portalu Deweloper przy użyciu szablonów.

> [AZURE.VIDEO adding-developer-portal-functionality-using-templates-in-azure-api-management]


[api-management-customize-menu]: ./media/api-management-developer-portal-templates/api-management-customize-menu.png
[api-management-templates-menu]: ./media/api-management-developer-portal-templates/api-management-templates-menu.png
[api-management-developer-portal-templates-overview]: ./media/api-management-developer-portal-templates/api-management-developer-portal-templates-overview.png
[api-management-template]: ./media/api-management-developer-portal-templates/api-management-template.png
[api-management-template-data]: ./media/api-management-developer-portal-templates/api-management-template-data.png
[api-management-developer-portal-menu]: ./media/api-management-developer-portal-templates/api-management-developer-portal-menu.png
[api-management-browse]: ./media/api-management-developer-portal-templates/api-management-browse.png
[api-management-user-profile-templates]: ./media/api-management-developer-portal-templates/api-management-user-profile-templates.png
[api-management-save-template]: ./media/api-management-developer-portal-templates/api-management-save-template.png
[api-management-publish-template]: ./media/api-management-developer-portal-templates/api-management-publish-template.png
[api-management-publish-template-confirm]: ./media/api-management-developer-portal-templates/api-management-publish-template-confirm.png
[api-management-publish-templates]: ./media/api-management-developer-portal-templates/api-management-publish-templates.png
[api-management-publish-customizations]: ./media/api-management-developer-portal-templates/api-management-publish-customizations.png
[api-management-revert-template]: ./media/api-management-developer-portal-templates/api-management-revert-template.png
[api-management-revert-template-confirm]: ./media/api-management-developer-portal-templates/api-management-revert-template-confirm.png
[api-management-reset-template]: ./media/api-management-developer-portal-templates/api-management-reset-template.png
[api-management-reset-template-confirm]: ./media/api-management-developer-portal-templates/api-management-reset-template-confirm.png
[api-management-restore-templates]: ./media/api-management-developer-portal-templates/api-management-restore-templates.png







