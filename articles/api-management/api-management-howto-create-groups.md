<properties 
    pageTitle="Jak utworzyć i używanie grup do zarządzania kontami dewelopera w zarządzaniu interfejsu API platformy Azure" 
    description="Dowiedz się, jak zarządzać kontami Deweloper przy użyciu grup w zarządzaniu interfejsu API platformy Azure" 
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

# <a name="how-to-create-and-use-groups-to-manage-developer-accounts-in-azure-api-management"></a>Jak utworzyć i używanie grup do zarządzania kontami Deweloper w zarządzaniu interfejsu API platformy Azure

W obszarze Zarządzanie interfejsu API grupy są używane do zarządzania widocznością produktów dla deweloperów. Produkty najpierw są one widoczne dla grupy, a następnie deweloperów w tych grup można wyświetlić i subskrybowanie produktów, które są skojarzone z grupy. 

Interfejs API zarządzania występują następujące grupy niezmienne systemu.

-   **Administratorzy** - Azure subskrypcji Administratorzy są członkami tej grupy. Administratorzy zarządzać wystąpień usługi zarządzania interfejsu API, tworzenia interfejsów API, operacje i produktów, które są używane przez deweloperów.
-   **Deweloperów** — dzięki portalowi deweloperów uwierzytelniony których użytkownicy należą do tej grupy. Klienci, którzy tworzyć aplikacje przy użyciu usługi interfejsy API się deweloperów. Deweloperzy uzyskają dostęp do portalu Deweloper i tworzyć aplikacje, które połączeń operacje interfejsu API.
-   **Goście** — Deweloper nieuwierzytelnionych użytkowników portalu, takich jak potencjalnych klientów odwiedzenie portalu Deweloper wystąpienia interfejsu API zarządzania należą do tej grupy. Ich można udzielić niektórych dostęp tylko do odczytu, takich jak możliwość wyświetlania interfejsów API, ale nie połączenie.

Oprócz tych grup systemu administratorzy mogą tworzyć niestandardowe grupy lub [wykorzystać zewnętrznych grup w skojarzony dzierżaw usługi Azure Active Directory][]. Niestandardowe i zewnętrznych grup mogą być używane razem z pakietem grupy systemu w określeniu widoczność deweloperów i dostęp do produktów interfejsu API. Na przykład możesz utworzyć jedną grupę niestandardową dla deweloperów powiązane z organizacją partnera i umożliwia im dostęp do interfejsów API z produktu zawierający tylko odpowiednie interfejsy API. Użytkownik może być członkiem więcej niż jednej grupy.

Ten przewodnik pokazano, jak dodawać nowe grupy i kojarzenie ich z produktami i deweloperów Administratorzy wystąpienia interfejsu API zarządzania.

>[AZURE.NOTE] Oprócz tworzenia i zarządzanie grupami w portalu programu publisher, można utworzyć i zarządzanie grup przy użyciu obiektu interfejsu API usługi REST zarządzania interfejsu API [grupy](https://msdn.microsoft.com/library/azure/dn776329.aspx) .

## <a name="create-group"> </a>Tworzenie grupy

Aby utworzyć nową grupę, kliknij przycisk **Zarządzaj** w portalu klasyczny Azure dotyczące usługi zarządzania interfejsu API. Powoduje przejście do portalu zarządzania interfejsu API programu publisher.

![Portal programu Publisher][api-management-management-console]

>Jeśli wystąpienie usługi zarządzania interfejsu API nie został jeszcze utworzony, zobacz [Tworzenie wystąpienie usługi zarządzania interfejsu API][] samouczka [Wprowadzenie do zarządzania interfejsu API Azure][] .

W menu **Zarządzania interfejsu API** po lewej stronie kliknij pozycję **grupy** , a następnie kliknij przycisk **Dodaj grupę**.

![Dodawanie nowej grupy][api-management-add-group]

Wpisz unikatową nazwę dla grupy i opcjonalny opis, a następnie kliknij przycisk **Zapisz**.

![Dodawanie nowej grupy][api-management-add-group-window]

Nowa grupa jest wyświetlana na karcie grupy. Aby edytować **nazwę** lub **Opis** grupy, kliknij nazwę grupy na liście. Aby usunąć grupę, kliknij przycisk **Usuń**.

![Grupa dodane][api-management-new-group]

Teraz, gdy grupa jest tworzona, może być skojarzone z produktami i deweloperów.

## <a name="associate-group-product"> </a>Skojarzyć grupy z produktem

Aby skojarzyć grupy z produktem, kliknij **produkty** z **Interfejsu API zarządzania** menu po lewej stronie, a następnie kliknij nazwę odpowiedniej produktu.

![Ustaw widoczność][api-management-add-group-to-product]

Wybierz kartę **widoczność** do dodawania i usuwania grup i wyświetlania bieżących grup dla tego produktu. Aby dodać lub usunąć grupy, zaznacz lub wyczyść pola wyboru obok odpowiedniej grupy, a następnie kliknij przycisk **Zapisz**.

![Ustaw widoczność][api-management-add-group-to-product-visibility]

>[AZURE.NOTE] Aby dodać grupy usługi Azure Active Directory, zobacz [jak Autoryzuj Deweloper konta za pomocą usługi Azure Active Directory w zarządzaniu interfejsu API Azure](api-management-howto-aad.md).
>
>Aby skonfigurować grupy z poziomu karty **widoczności** dla produktu, kliknij pozycję **Zarządzanie grupami**.

Gdy produkt jest skojarzone z grupą, deweloperzy w tej grupie można przeglądać i subskrybowanie produktu.

## <a name="associate-group-developer"> </a>Skojarzyć deweloperów grup

Aby skojarzyć grupy z deweloperów, w menu **Zarządzania interfejsu API** po lewej stronie kliknij pozycję **Użytkownicy** , a następnie zaznacz pole obok deweloperów, który chcesz skojarzyć z grupy.

![Dodawanie dewelopera do grupy][api-management-add-group-to-developer]

Po odpowiednim deweloperów są zaznaczone, kliknij odpowiednią grupę z listy rozwijanej **Dodaj do grupy** . Programista może być usuwany ze grup przy użyciu **Usuń z grupy** listy rozwijanej. 

![Deweloperów][api-management-add-group-to-developer-saved]

Po dodaniu skojarzenie między projektanta i grupy można wyświetlić go na karcie **użytkowników** .


## <a name="next-steps"> </a>Następne kroki

-   Po dodaniu Deweloper do grupy mogą wyświetlać i subskrybowanie produktów skojarzonych z tej grupy. Aby uzyskać więcej informacji zobacz [jak tworzenie i publikowanie produktu w zarządzaniu interfejsu API platformy Azure][]
-   Oprócz tworzenia i zarządzanie grupami w portalu programu publisher, można utworzyć i zarządzanie grup przy użyciu obiektu interfejsu API usługi REST zarządzania interfejsu API [grupy](https://msdn.microsoft.com/library/azure/dn776329.aspx) .


[api-management-management-console]: ./media/api-management-howto-create-groups/api-management-management-console.png
[api-management-add-group]: ./media/api-management-howto-create-groups/api-management-add-group.png
[api-management-add-group-window]: ./media/api-management-howto-create-groups/api-management-add-group-window.png
[api-management-new-group]: ./media/api-management-howto-create-groups/api-management-new-group.png
[api-management-add-group-to-product]: ./media/api-management-howto-create-groups/api-management-add-group-to-product.png
[api-management-add-group-to-product-visibility]: ./media/api-management-howto-create-groups/api-management-add-group-to-product-visibility.png
[api-management-add-group-to-developer]: ./media/api-management-howto-create-groups/api-management-add-group-to-developer.png
[api-management-add-group-to-developer-saved]: ./media/api-management-howto-create-groups/api-management-add-group-to-developer-saved.png

[api-management-]: ./media/api-management-howto-create-groups/api-management-.png

[Create a group]: #create-group
[Associate a group with a product]: #associate-group-product
[Associate groups with developers]: #associate-group-developer
[Next steps]: #next-steps

[Jak tworzyć i publikować produktu w zarządzaniu interfejsu API platformy Azure]: api-management-howto-add-products.md

[Wprowadzenie do zarządzania interfejsu API platformy Azure]: api-management-get-started.md
[Tworzenie wystąpienia usługi zarządzania interfejsu API]: api-management-get-started.md#create-service-instance
[korzystać z grup zewnętrznych w skojarzone dzierżaw usługi Azure Active Directory]: api-management-howto-aad.md#how-to-add-an-external-azure-active-directory-group
