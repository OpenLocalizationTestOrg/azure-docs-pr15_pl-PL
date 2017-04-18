<properties 
    pageTitle="Interfejs API zarządzania podstawowych pojęć" 
    description="Informacje na temat interfejsów API, produktów, ról, grupy i inne podstawowe pojęcia interfejsu API zarządzania." 
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
    ms.topic="hero-article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

#<a name="what-is-api-management"></a>Co to jest interfejs API zarządzania?

Interfejs API zarządzania ułatwia organizacjom publikują interfejsy API zewnętrznym, partnera i deweloperów wewnętrznych odblokowywanie możliwości ich dane i usługi. Firmy wszędzie są chcą rozszerzanie ich działania jako cyfrowego platformy, tworzenia nowych kanałów, znajdowanie nowych klientów i prowadzenie szczegółowego zaangażowania z istniejących. Interfejs API zarządzania zawiera kompetencjach zapewnienie powiedzie programu interfejsu API do zaangażowania Deweloper, analizy biznesowej, analizy, zabezpieczenia i ochrona.

W poniższym klipie wideo zawiera omówienie dotyczące zarządzania interfejsu API Azure i Dowiedz się, jak dodać wiele funkcji API, w tym Kontrola dostępu, ograniczanie szybkości monitorowania, rejestrowanie zdarzeń i buforowanie odpowiedzi z minimalnymi nakładu pracy za pomocą interfejsu API zarządzania.

> [AZURE.VIDEO azure-api-management-overview]

Aby korzystać z interfejsu API zarządzania, Administratorzy tworzyć interfejsy API. Każdy interfejs API składa się z jednego lub więcej operacji, a każdego interfejsu API można dodawać do jednego lub więcej produktów. Aby korzystać z interfejsu API, deweloperzy subskrybowanie produktu, który zawiera tego interfejsu API, a następnie ich Zadzwoń działania interfejsu API, objęte wszelkie zasady zastosowania, które mogą mieć znaczenie.

Ten temat zawiera omówienie podstawowych pojęć interfejsu API zarządzania.

>[AZURE.NOTE] Aby uzyskać więcej informacji, zobacz [opartej na chmurze zarządzania interfejsu API: wykorzystanie Power API](http://j.mp/ms-apim-whitepaper) dokument PDF. Ten dokument wprowadzający dotyczący zarządzania interfejsu API, CITO badań obejmuje: 
>
> - Wspólne wymagania interfejsu API i problemach
>     - Rozdzielenie interfejsy API i prezentowanie fasad
>     - Wprowadzenie deweloperów w górę i szybkie uruchamianie
>     - Zabezpieczanie dostępu
>     - Analizy i miar
> - Uzyskanie kontroli i informacje z platformy zarządzania interfejsu API
> - Przy użyciu rozwiązania lokalnego w porównaniu z chmury
> - Zarządzanie API platformy Azure

## <a name="apis"> </a>Interfejsów API i operacji

Interfejsy API są podstawą wystąpienia usługi zarządzania interfejsu API. Każdy interfejs API reprezentuje zestaw operacji dostępne dla deweloperów. Każdy interfejs API zawiera odwołanie do usługi wewnętrznej, który wykonuje API i jego Mapa operacje, aby działania wykonywane przez usługę wewnętrznej. Operacje w zarządzaniu interfejsu API są bardzo konfigurowane, kontrolę nad adresu URL mapowanie, kwerendy i parametry ścieżki, żądanie i odpowiedź zawartości i buforowanie odpowiedzi operacji. Limit szybkości, przydziałów i zasady ograniczeń adresów IP można też zaimplementować poziomie interfejsu API lub poszczególnych działań.

Aby uzyskać więcej informacji zobacz [jak utworzyć interfejsy API][] i [jak dodać operacje, aby interfejs API][].


## <a name="products"></a> Produkty

Produkty są, jak interfejsy API są udostępnione deweloperów. Produkty w zarządzaniu interfejsu API mieć co najmniej jeden interfejsy API i skonfigurowano tytuł, opis i warunki użytkowania. Produkty może być **otwarty** lub **chroniony**. Produkty chronionego musisz mieć subskrypcję przed ich użyciem, otwartej produkty mogą być używane bez subskrypcji. Gdy produkt jest gotowa do użycia przez deweloperów może zostać opublikowany. Po opublikowaniu, można go wyświetlać (i w przypadku produktów chronionego subskrybuje) przez deweloperów. Zatwierdzanie subskrypcji jest skonfigurowane na poziomie produktu i można wymagają zatwierdzenia przez administratora lub uzyskiwać zatwierdzenie automatyczne.

Grupy służą do zarządzania widocznością produktów dla deweloperów. Produkty Udziel widoczność do grup i deweloperów można wyświetlać i subskrybowanie produktów, które są widoczne dla grup, w których należą. 

Aby uzyskać więcej informacji zobacz [jak tworzenie i publikowanie produktu][] i poniższym klipie wideo.

> [AZURE.VIDEO using-products]

## <a name="groups"></a> Grupy

Grupy służą do zarządzania widocznością produktów dla deweloperów. Interfejs API zarządzania występują następujące grupy niezmienne systemu.

-   **Administratorzy** - Azure subskrypcji Administratorzy są członkami tej grupy. Administratorzy zarządzać wystąpień usługi zarządzania interfejsu API, tworzenia interfejsów API, operacje i produktów, które są używane przez deweloperów.
-   **Deweloperów** — dzięki portalowi deweloperów uwierzytelniony których użytkownicy należą do tej grupy. Klienci, którzy tworzyć aplikacje przy użyciu usługi interfejsy API się deweloperów. Deweloperzy uzyskają dostęp do portalu Deweloper i tworzyć aplikacje, które połączeń operacje interfejsu API.
-   **Goście** — Deweloper nieuwierzytelnionych użytkowników portalu, takich jak potencjalnych klientów odwiedzenie portalu Deweloper wystąpienia interfejsu API zarządzania należą do tej grupy. Ich można udzielić niektórych dostęp tylko do odczytu, takich jak możliwość wyświetlania interfejsów API, ale nie połączenie.

Oprócz tych grup systemu administratorzy mogą tworzyć niestandardowe grupy lub [wykorzystać zewnętrznych grup w skojarzony dzierżaw usługi Azure Active Directory](api-management-howto-aad.md#how-to-add-an-external-azure-active-directory-group). Niestandardowe i zewnętrznych grup mogą być używane razem z pakietem grupy systemu w określeniu widoczność deweloperów i dostęp do produktów interfejsu API. Na przykład możesz utworzyć jedną grupę niestandardową dla deweloperów powiązane z organizacją partnera i umożliwia im dostęp do interfejsów API z produktu zawierający tylko odpowiednie interfejsy API. Użytkownik może być członkiem więcej niż jednej grupy.

Aby uzyskać więcej informacji zobacz [jak tworzenie i używanie grup][].

## <a name="developers"></a> Deweloperów

Deweloperów reprezentują kont użytkowników w wystąpieniu usług zarządzania interfejsu API. Deweloperów można utworzyć lub zaproszeni do przyłączenia się przez administratorów lub ich mogą tworzyć konta z [dzięki portalowi deweloperów][]. Każdy projektant jest członkiem jednej lub kilku grup i może być Subskrybowanie produktów, które przyznać widoczność do tych grup.

Gdy deweloperów subskrybowanie produktu otrzymują klucz podstawowy i pomocniczy dla tego produktu. Ten klucz jest używany podczas nawiązywania połączenia do interfejsów API produktu.

Aby uzyskać więcej informacji zobacz [jak utworzyć lub zapraszanie projektantów][] i [jak skojarzyć grup zawierających deweloperów][].

## <a name="policies"></a> Zasad

Zasady są zaawansowanych możliwości zarządzania interfejsu API, umożliwiająca programu publisher zmienić zachowanie interfejsu API przy użyciu konfiguracji. Zasady są to zbiór instrukcji, które są wykonywane kolejno na żądanie lub odpowiedź interfejsu API. Popularne instrukcje zawierają konwersję formatu z pliku XML do JSON i ograniczanie szybkości połączenia, aby ograniczyć liczbę połączeń przychodzących z Deweloper, a inne zasady są dostępne.

Zasady wyrażeń może służyć jako wartości atrybutu lub tekstu w każdym z zasad zarządzania interfejsu API, chyba że zasady określi inaczej. Pewne zasady, takie jak zasady [Przepływ sterowania](https://msdn.microsoft.com/library/azure/dn894085.aspx#choose) i [Ustaw zmienną](https://msdn.microsoft.com/library/azure/dn894085.aspx#set-variable) są oparte na wyrażeniach zasad. Aby uzyskać więcej informacji zobacz [Zaawansowane zasady](https://msdn.microsoft.com/library/azure/dn894085.aspx#AdvancedPolicies), [zasady wyrażeń](https://msdn.microsoft.com/library/azure/dn910913.aspx)i w poniższym klipie wideo.

> [AZURE.VIDEO policy-expressions-in-azure-api-management]

Aby uzyskać pełną listę zasad zarządzania interfejsu API zobacz [informacje dotyczące zasad][]. Aby uzyskać więcej informacji o używaniu i konfigurowaniu zasad zobacz [zasady zarządzania interfejsu API][]. Samouczek dotyczący tworzenia produktu z stawka limit i stanie przydziału zasad zobacz [jak utworzyć i skonfigurować ustawienia zaawansowane produktu][]. Aby uzyskać pokaz Zobacz poniższym klipie wideo.

> [AZURE.VIDEO rate-limits-and-quotas]

## <a name="developer-portal"></a> Dzięki portalowi deweloperów

Dzięki portalowi deweloperów to miejsce, w którym można więcej informacji na temat usługi operacje interfejsów API, wyświetlanie i połączeń deweloperów, a Subskrybowanie produktów. Potencjalni klienci mogą odwiedzić witrynę portalu Deweloper Wyświetlanie interfejsów API i operacje i utworzyć konto. Adres URL portalu sieci Deweloper znajduje się na pulpicie nawigacyjnym w portalu klasyczny Azure wystąpienia usługi zarządzania interfejsu API.

Możesz dostosować wygląd i działanie usługi dzięki portalowi deweloperów, dodając zawartość niestandardową, Dostosowywanie stylów i dodawanie znaku.

## <a name="api-management-and-the-api-economy"></a>Interfejs API zarządzania i zużycia interfejsu API

Aby uzyskać więcej informacji o zarządzaniu interfejsu API, obejrzyj następujące prezentacji z konferencji programu Microsoft Ignite 2015.

> [AZURE.VIDEO microsoft-ignite-2015-azure-api-management-and-the-api-economy]

[APIs and operations]: #apis
[Products]: #products
[Groups]: #groups
[Developers]: #developers
[Policies]: #policies
[Dzięki portalowi deweloperów]: #developer-portal

[Jak utworzyć interfejsy API]: api-management-howto-create-apis.md
[Jak dodać operacje do interfejsu API]: api-management-howto-add-operations.md
[Jak tworzyć i publikować produktu]: api-management-howto-add-products.md
[Jak utworzyć i używanie grup]: api-management-howto-create-groups.md
[Jak chcesz skojarzyć deweloperów grup]: api-management-howto-create-groups.md#associate-group-developer
[Jak utworzyć i skonfigurować ustawienia zaawansowane produktu]: api-management-howto-product-with-rules.md
[Jak utworzyć lub programista Zaproś]: api-management-howto-create-or-invite-developers.md
[Odwołanie do zasad]: api-management-policy-reference.md
[Zasady zarządzania interfejsu API]: api-management-howto-policies.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance



 
