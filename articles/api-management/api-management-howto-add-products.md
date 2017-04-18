<properties 
    pageTitle="Jak można tworzyć i publikować produktu w zarządzaniu interfejsu API platformy Azure" 
    description="Dowiedz się, jak tworzyć i publikować produkty w zarządzaniu interfejsu API Azure." 
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

# <a name="how-to-create-and-publish-a-product-in-azure-api-management"></a>Jak tworzyć i publikować produktu w zarządzaniu interfejsu API platformy Azure

W zarządzaniu interfejsu API Azure produkt zawiera jeden lub więcej interfejsów API, a także przydział użycia i warunki użytkowania. Po opublikowaniu produktu deweloperów można zasubskrybować produktu i zacząć przy użyciu interfejsów API produktu. Temat zawiera przewodnik dotyczący tworzenia produktu, dodając interfejs API i opublikowanie go dla deweloperów.

## <a name="create-product"> </a>Tworzenie produktu

Operacje są dodawane i skonfigurowany do interfejsu API w portalu programu publisher. Aby uzyskać dostęp do portalu programu publisher, kliknij przycisk **Zarządzaj** w portalu klasycznym Azure dotyczące usługi zarządzania interfejsu API.

![Portal programu Publisher][api-management-management-console]

>Jeśli wystąpienie usługi zarządzania interfejsu API nie został jeszcze utworzony, zobacz [Tworzenie wystąpienie usługi zarządzania interfejsu API][] samouczka [Wprowadzenie do zarządzania interfejsu API Azure][] .

Kliknij pozycję **produktów** w menu po lewej stronie, aby wyświetlić stronę **produktów** , a następnie kliknij przycisk **Dodaj produkt**.

![Produkty][api-management-products]

![Nowy produkt][api-management-add-new-product]

Wprowadź opisową nazwę dla tego produktu w polu **Nazwa** i opis produktu w polu **Opis** .

Produkty w zarządzaniu interfejsu API może być **otwarty** lub **chroniony**. Produkty chronionego musisz mieć subskrypcję przed ich użyciem, otwartej produkty mogą być używane bez subskrypcji. Sprawdź **wymaga subskrypcji** do tworzenia chronionych produktów, która wymaga subskrypcji. Jest to ustawienie domyślne.

Zaznacz **Wymaganie zatwierdzenia subskrypcji** , jeśli chcesz, aby administrator, aby przejrzeć i zaakceptować lub odrzucić prób subskrypcji tego produktu. Jeśli pole jest zaznaczona, prób subskrypcji będzie zatwierdzony automatyczne. Aby uzyskać więcej informacji o subskrypcji zobacz [Wyświetlanie subskrybentów produktu][].

Aby umożliwić kontom Deweloper subskrypcji wielokrotnie produktu, zaznacz pole wyboru **Zezwalaj na wiele subskrypcji** . Jeśli to pole nie jest zaznaczone, każdy konta dewelopera subskrybować tylko jeden raz produktu.

![Nieograniczoną wiele subskrypcji][api-management-unlimited-multiple-subscriptions]

Aby ograniczyć liczbę wiele subskrypcji jednocześnie, zaznacz pole wyboru **Ogranicz liczbę jednoczesnego subskrypcje** i wprowadź limit subskrypcji. W poniższym przykładzie jednoczesnego subskrypcje są ograniczone do czterech na konta dewelopera.

![Cztery wiele subskrypcji][api-management-four-multiple-subscriptions]

Po wszystkich nowych opcji produktu są skonfigurowane, kliknij przycisk **Zapisz** , aby utworzyć nowy produkt.

![Produkty][api-management-products-page]

>Domyślnie nowych produktów wycofanych i jest widoczny tylko dla grupy **administratorów** .

Aby skonfigurować produkt, kliknij nazwę produktu na karcie **produkty** .

## <a name="add-apis"> </a>Dodawanie interfejsów API z produktem

Strona **produkty** zawiera cztery łącza konfiguracji: **Podsumowanie**, **Ustawienia** **widoczności**i **subskrybentów**. Na karcie **Podsumowanie** to miejsce, w którym można dodać interfejsy API i publikowanie lub cofanie publikowania produktu.

![Podsumowanie][api-management-new-product-summary]

Przed opublikowaniem pakietu, musisz dodać jeden lub więcej interfejsów API. Aby to zrobić, kliknij przycisk **Dodaj interfejsu API do produktu**.

![Dodawanie interfejsów API][api-management-add-apis-to-product]

Wybierz odpowiednie interfejsy API, a następnie kliknij przycisk **Zapisz**.

## <a name="add-description"> </a>Można umieścić informacje opisujące z produktem

Karta **Ustawienia** umożliwia zawierają szczegółowe informacje o produkcie, takich jak celowi, zapewnia dostęp do interfejsów API i inne przydatne informacje. Zawartość jest przeznaczona dla deweloperów, którzy będą wywołanie API i mogą być zapisywane w formacie zwykłego tekstu lub języka HTML.

![Ustawienia produktu][api-management-product-settings]

Zaznacz pole wyboru **wymaga subskrypcji** , aby utworzyć chronionego produktu, która wymaga subskrypcji do użytku, lub wyczyść pole wyboru Utwórz otwieranie produktu, który można wywołać bez subskrypcji.

Jeśli chcesz ręcznie zatwierdzić wszystkie żądania subskrypcji produktu, wybierz opcję **Wymagaj zatwierdzania subskrypcji** . Domyślnie wszystkie subskrypcje produktu są przydzielane automatycznie.

Aby umożliwić kontom Deweloper subskrypcji wielokrotnie produktu, zaznacz pole wyboru **Zezwalaj na wiele subskrypcji** i opcjonalnie Określ limit. Jeśli to pole nie jest zaznaczone, każdy konta dewelopera subskrybować tylko jeden raz produktu.

Opcjonalnie Wypełnij pole **warunki użytkowania** opisem warunki użytkowania produktu, którzy użytkownicy muszą zaakceptować, aby używać produktu.

## <a name="publish-product"> </a>Publikowanie produktu

Interfejs API programu produktu może być wywołana, należy opublikować produktu. Na karcie **Podsumowanie** produktu kliknij przycisk **Publikuj**, a następnie kliknij **opublikować go tak,** aby potwierdzić. Aby ustawić jako prywatny opublikowanej produktu, kliknij przycisk **Cofnij publikowanie**.

![Publikowanie produktu][api-management-publish-product]

## <a name="make-visible"> </a>Być widoczny dla deweloperów produktu

Na karcie **widoczność** można wybrać role są w stanie zobaczyć produktu w portalu Deweloper i subskrybowanie produktu.

![Widoczność produktu][api-management-product-visiblity]

Aby włączyć lub wyłączyć widoczność produktu dla deweloperów w grupy, zaznacz lub usuń zaznaczenie pola wyboru obok grupy, a następnie kliknij przycisk **Zapisz**.

>Aby uzyskać więcej informacji zobacz [jak tworzenie i używanie grup do zarządzania kontami dewelopera w zarządzaniu interfejsu API Azure][].

## <a name="view-subscribers"> </a>Wyświetlanie subskrybentów produktu

Na karcie **subskrybentów** wymienione deweloperów, którzy zasubskrybowano produktu. Szczegóły i ustawienia dla wszystkich deweloperów można wyświetlić, klikając nazwę dewelopera. W tym przykładzie nie deweloperów jeszcze zasubskrybowano produktu.

![Deweloperów][api-management-developer-list]

## <a name="next-steps"> </a>Następne kroki

Po odpowiednim interfejsy API zostaną dodane i opublikowany produktu, deweloperzy można zasubskrybować produktu i zacznij nawiązać połączenie za pośrednictwem interfejsów API. Samouczek demonstrujący te elementy, a także konfiguracji zaawansowanej produktu zobacz [jak utworzyć i skonfigurować ustawienia zaawansowane produktu w zarządzaniu interfejsu API Azure][].

Aby uzyskać więcej informacji na temat pracy z produktami Zobacz poniższym klipie wideo.

> [AZURE.VIDEO using-products]

[Create a product]: #create-product
[Add APIs to a product]: #add-apis
[Add descriptive information to a product]: #add-description
[Publish a product]: #publish-product
[Make a product visible to developers]: #make-visible
[Widok subskrybentów produktu]: #view-subscribers
[Next steps]: #next-steps

[api-management-management-console]: ./media/api-management-howto-add-products/api-management-management-console.png
[api-management-add-product]: ./media/api-management-howto-add-products/api-management-add-product.png
[api-management-add-new-product]: ./media/api-management-howto-add-products/api-management-add-new-product.png
[api-management-unlimited-multiple-subscriptions]: ./media/api-management-howto-add-products/api-management-unlimited-multiple-subscriptions.png
[api-management-four-multiple-subscriptions]: ./media/api-management-howto-add-products/api-management-four-multiple-subscriptions.png
[api-management-products-page]: ./media/api-management-howto-add-products/api-management-products-page.png
[api-management-new-product-summary]: ./media/api-management-howto-add-products/api-management-new-product-summary.png
[api-management-add-apis-to-product]: ./media/api-management-howto-add-products/api-management-add-apis-to-product.png
[api-management-product-settings]: ./media/api-management-howto-add-products/api-management-product-settings.png
[api-management-publish-product]: ./media/api-management-howto-add-products/api-management-publish-product.png
[api-management-product-visiblity]: ./media/api-management-howto-add-products/api-management-product-visibility.png
[api-management-developer-list]: ./media/api-management-howto-add-products/api-management-developer-list.png



[api-management-products]: ./media/api-management-howto-add-products/api-management-products.png
[api-management-]: ./media/api-management-howto-add-products/
[api-management-]: ./media/api-management-howto-add-products/


[How to add operations to an API]: api-management-howto-add-operations.md
[How to create and publish a product]: api-management-howto-add-products.md
[Wprowadzenie do zarządzania interfejsu API platformy Azure]: api-management-get-started.md
[Tworzenie wystąpienia usługi zarządzania interfejsu API]: api-management-get-started.md#create-service-instance
[Next steps]: #next-steps
[Jak utworzyć i używanie grup do zarządzania kontami Deweloper w zarządzaniu interfejsu API platformy Azure]: api-management-howto-create-groups.md
[Jak utworzyć i skonfigurować ustawienia zaawansowane produktu w zarządzaniu interfejsu API platformy Azure]: api-management-howto-product-with-rules.md 