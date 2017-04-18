<properties
    pageTitle="Omówienie portalu Microsoft Azure"
    description="Dowiedz się, jak korzystać z portalu Microsoft Azure."
    services=""
    documentationCenter=""
    authors="davidwrede"
    manager="dwrede"
    editor="jimbe"/>

<tags
    ms.service="na"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="12/16/2015"
    ms.author="dwrede"/>

# <a name="microsoft-azure-portal-overview"></a>Omówienie portalu Microsoft Azure

Portal usługi Microsoft Azure to centralne miejsce, gdzie możesz obsługi administracyjnej i zarządzanie zasobami Azure.  Ten samouczek będzie zapoznanie się z portalem i pokazano, jak użyć tych najważniejszych funkcji:
- **Pełna marketplace** umożliwiające przeglądanie tysiące elementów firmy Microsoft i innych dostawców, które mogą być zakupione i/lub obsługi administracyjnej.
- **Środowisko Przeglądaj ujednolicony i skalowalna** który ułatwia znajdowanie zasobów istotnych informacji i wykonywać różne operacje zarządzania.
- **Spójne zarządzanie stron** (lub karty) których można zarządzać Azure przez szeroką gamę usług za pośrednictwem spójny sposób udostępnienia ustawienia, akcje rozliczeń informacji, danych monitorowania i zastosowania zdrowia i duża więcej.
- Umożliwia utworzenie ekran startowy niestandardowych, zawierający informacje, które mają być wyświetlane w każdym przypadku, gdy **obsługi osobistych** Zaloguj się.  Można także dostosować dowolne z karty zarządzania, które zawierają kafelków.

 ![Orientacja Azure portalu interfejsu użytkownika][UIOrientation]

## <a name="before-you-get-started"></a>Przed rozpoczęciem

Konieczne będzie ważna subskrypcja Azure do tego samouczka.  Jeśli nie masz, następnie [Załóż bezpłatną wersję próbną](https://azure.microsoft.com/pricing/free-trial/) dzisiaj.  Po umieszczeniu subskrypcji, masz dostęp do portalu pod adresem [https://portal.azure.com].

## <a name="how-to-create-a-resource"></a>Jak utworzyć zasób

Azure ma marketplace tysięcy elementów, które można tworzyć w jednym miejscu.  Załóżmy, że chcesz utworzyć nowy maszyn wirtualnych programu Windows Server 2012.  + NOWE Centrum jest punktu wejścia w zestawie curated polecane kategorii z witryny marketplace.  Każdej kategorii ma zbiór małych wyróżnionych elementów wraz z łączem do pełnego witryny marketplace, przedstawiający wszystkie kategorie i wyszukiwania. Aby utworzyć tego nowa systemu Windows Server 2012 maszyna wirtualna, wykonaj następujące czynności:  

1.  Windows Server 2012 wyróżniono, dzięki czemu można zaznaczyć z kategorii obliczeń.  
2.  Wypełnij niektóre podstawowe wartości wejściowych w formularzu.
3.  Kliknij przycisk "Utwórz" i usługi maszyn wirtualnych rozpocznie się do zapewnienia obsługi natychmiast.

Centrum powiadomienia będzie wyświetlane w przypadku zasobu został utworzony, karta zarządzania zostanie otwarty (można zawsze przejść do zasobów później).

![Kategorie portalu][PortalCategories]


## <a name="how-to-find-your-resources"></a>Jak znaleźć zasobów

Możesz zawsze przypiąć często używanych zasobów do Twojej startboard, ale może być konieczne przejść do innej, z którego często korzystasz nie.  Centrum Przeglądaj, jak pokazano poniżej jest jak uzyskać dostęp do wszystkich zasobów.  Można filtrować według subskrypcji, wybierz pozycję/Zmień rozmiar kolumn i przejdź do karty zarządzania klikając poszczególne elementy.

![Przeglądanie Centrum][BrowseHub]

## <a name="how-to-manage-and-delegate-access-to-a-resource"></a>Jak zarządzać i udzielanie pełnomocnictw do zasobu

Z tego karta można połączyć się z komputerem wirtualnych pulpitu zdalnego, monitorowanie kluczowe wskaźniki, kontrolowanie dostępu do tego maszyn wirtualnych za pomocą programu access oparte na roli (RBAC), konfigurowanie maszyn wirtualnych i innych zadań zarządzania ważne.  Delegowanie dostępu na podstawie roli jest krytyczne na potrzeby zarządzania w skali.  Kliknij [tutaj](./active-directory/role-based-access-control-configure.md) , aby uzyskać więcej informacji. Aby pełnomocnictwa do zasobu, wykonaj następujące czynności:

1.  Przejdź do zasobu.
2.  Kliknij pozycję wszystkie ustawienia, w sekcji Essentials.
3.  Na liście ustawienia, kliknij pozycję "Użytkownicy".
4.  Kliknij przycisk "Dodaj" na pasku poleceń.
5.  Wybierz użytkownika i roli.

![Zarządzanie zasobu][ManageResource]

## <a name="how-to-customize-a-resource-blade"></a>Jak dostosować karta zasobu

Azure nie skonfiguruje wstępnie karty dla zasobów, ale Kafelki te karty są Twoje do kontrolki.  Możesz łatwo przejść do dostosowywania trybu Dodawanie, usuwanie i zmienianie rozmiaru lub ponowne rozmieszczanie Kafelki. Aby dostosować kart, wykonaj następujące czynności:

1.  Przejdź do zasobu.
2.  Kliknij przycisk "..." w górnej części karta, który chcesz dostosować.
3.  Kliknij pozycję "Dodaj części".
4.  Zacznij przeciąganie i upuszczanie części.  

![Dostosowywanie karty][CustomizeBlades]

## <a name="how-to-get-help"></a>Jak uzyskać pomoc

Jeśli masz problemy, służymy dla Ciebie.  Portalu ma stronę Pomoc i obsługa techniczna, które wskazują w kierunku, w prawo.  W zależności od usługi [pomocy technicznej plan](https://azure.microsoft.com/support/plans/)można także tworzyć biletami pomocy technicznej bezpośrednio w portalu.  Po utworzeniu bilet pomocy technicznej, możesz zarządzać cyklem życia biletów z poziomu portalu. Aby przejść do pomocy i strony pomocy technicznej, przechodząc do przeglądania -> Pomoc + pomocy technicznej.  

![Pomoc i obsługa techniczna][HelpSupport]

## <a name="summary"></a>Podsumowanie

Przeanalizujmy materiału w ramach tego samouczka:
- Wiesz, jak utworzyć konto, Uzyskaj subskrypcji i przejdź do portalu
- Masz orientacji za pomocą portalu interfejsu użytkownika i materiału sposobu tworzenia i przeglądanie zasobów
- Wiesz, jak tworzyć zasobu i przeglądanie zasobów
- Dowiedział się o karty struktury lub zarządzania i jak spójne zarządzanie różnych typów zasobów
- Wiesz, jak dostosować portalu, aby wyświetlić informacje istotnych informacji do przodu i wyśrodkuj
- Wiesz, jak kontrolowanie dostępu do zasobów za pomocą programu access oparte na roli (RBAC)
- Wiesz, jak uzyskać pomoc i obsługa techniczna

Portal usługi Microsoft Azure ponoszonych przez tworzenie i zarządzanie aplikacjami usługi w chmurze.  Zapoznaj się [zarządzania blogu](https://azure.microsoft.com/blog/topics/management/) , aby zachować aktualność jako jesteśmy stale [Słuchanie opinii](https://feedback.azure.com/forums/223579-azure-preview-portal/) i wprowadzanie ulepszeń.  [Blog w ScottGu](http://weblogs.asp.net/scottgu) jest inny doskonałe miejsce, aby wyszukać wszystkie aktualizacje Azure.

[UIOrientation]: ./media/azure-portal-how-to-use/azure_portal_1.png
[PortalCategories]: ./media/azure-portal-how-to-use/azure_portal_2.png
[BrowseHub]: ./media/azure-portal-how-to-use/azure_portal_3.png
[ManageResource]: ./media/azure-portal-how-to-use/azure_portal_4.png
[CustomizeBlades]: ./media/azure-portal-how-to-use/azure_portal_5.png
[HelpSupport]: ./media/azure-portal-how-to-use/azure_portal_6.png
