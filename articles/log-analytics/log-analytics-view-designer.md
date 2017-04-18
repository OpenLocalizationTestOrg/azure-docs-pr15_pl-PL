<properties
    pageTitle="Zaloguj się Projektant widoków analizy | Microsoft Azure"
    description="Projektant widoków w dzienniku analizy umożliwia tworzenie niestandardowych widoków w konsoli usługi OMS, które zawierają różne wizualizacje danych w repozytorium usługi OMS. Ten artykuł zawiera omówienie Projektant widoków i procedur dotyczących tworzenia i edytowania widoków niestandardowych."
    services="log-analytics"
    documentationCenter=""
    authors="bwren"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="bwren"/>

# <a name="log-analytics-view-designer"></a>Projektant widoków analizy dziennika
Projektant widoków w dzienniku analizy umożliwia tworzenie niestandardowych widoków w konsoli usługi OMS, które zawierają różne wizualizacje danych w repozytorium usługi OMS. Ten artykuł zawiera omówienie Projektant widoków i procedur dotyczących tworzenia i edytowania widoków niestandardowych.

Inne artykuły, które są dostępne dla projektanta widoku są:

- [Odwołanie kafelku](log-analytics-view-designer-tiles.md) — odwołanie ustawienia dla każdego kafelków dostępny do użytku w sekcji Widoki niestandardowe. 
- [Odwołanie do wizualizacji części](log-analytics-view-designer-parts.md) — odwołanie ustawienia dla każdego kafelków dostępny do użytku w sekcji Widoki niestandardowe. 


## <a name="concepts"></a>Pojęcia
Widoki utworzone za pomocą projektanta widoku zawierają elementy w poniższej tabeli.

| Części | Opis |
|:--|:--|
| Kafelków | Wyświetlane na pulpicie nawigacyjnym głównym omówienie analizy dziennika.  Zawiera wizualne podsumowywania informacji zawartych w widoku niestandardowym.  Różne typy kafelków zapewniają różnych wizualizacji w repozytorium usługi OMS.  Kliknij Kafelek, aby otworzyć widok niestandardowy. |
| Widok niestandardowy | Wyświetlany, gdy użytkownik kliknie na kafelku.  Zawiera co najmniej jeden części wizualizacji. |
| Części wizualizacji | Wizualizacja danych w repozytorium usługi OMS według jednego lub więcej [wyszukiwań dziennika](log-analytics-log-searches.md).  Większość elementów będzie zawierać nagłówek, który zapewnia wysokiego poziomu wizualizacji i lista górny wyników.  Typy innej części zapewniają różnych wizualizacji w repozytorium usługi OMS.  Kliknij elementy w części do wyszukiwania dziennika, dostarczając szczegółowe rekordy. |

![Omówienie projektanta widoku](media/log-analytics-view-designer/overview.png)

## <a name="add-view-designer-to-your-workspace"></a>Dodawanie Projektant widoków do obszaru roboczego
Projektant widoków w trakcie Podgląd, należy dodać ją do obszaru roboczego, wybierając **Funkcje Preview** w sekcji **Ustawienia** portalu usługi OMS.

![Włącz podgląd](media/log-analytics-view-designer/preview.png)

## <a name="creating-and-editing-views"></a>Tworzenie i edytowanie widoków

### <a name="create-a-new-view"></a>Tworzenie nowego widoku
Otwórz nowy widok w **Projektancie widok** , klikając na kafelku projektanta widoku w głównym pulpitu nawigacyjnego usługi OMS.

![Widok projektanta kafelków](media/log-analytics-view-designer/view-designer-tile.png)

### <a name="edit-an-existing-view"></a>Edytowanie istniejącego widoku
Aby edytować istniejący widok w Projektancie widok, otwórz widok, klikając jego kafelka w głównym pulpitu nawigacyjnego usługi OMS.  Następnie kliknij przycisk **Edytuj** , aby otworzyć widok w Projektancie widoku.

![Edytowanie widoku](media/log-analytics-view-designer/menu-edit.png)

### <a name="clone-an-existing-view"></a>Klonowanie istniejącego widoku
Gdy klonowanie widoku, tworzy nowy widok i otwarcie go w Projektancie widoku.  Nowy widok będą mieć taką samą nazwę jak oryginał z "kopiowanie" dołączonych na koniec.  Aby klonowanie widoku, otwórz istniejący widok, klikając na jego kafelka w głównym pulpitu nawigacyjnego usługi OMS.  Następnie kliknij przycisk **klonowanie** , aby otworzyć widok w Projektancie widoku.

![Klonowanie widoku](media/log-analytics-view-designer/edit-menu-clone.png)

### <a name="delete-an-existing-view"></a>Usuń istniejący widok
Aby usunąć istniejący widok, otwórz widok, klikając jego kafelka w głównym pulpitu nawigacyjnego usługi OMS.  Następnie kliknij przycisk **Edytuj** , aby otworzyć widok w Projektancie widok, a następnie kliknij przycisk **Usuń widok**.

![Usuwanie widoku](media/log-analytics-view-designer/edit-menu-delete.png)

### <a name="export-an-existing-view"></a>Eksportowanie istniejącego widoku
Widok można eksportować do pliku JSON, który można zaimportować do innego obszaru roboczego lub przy użyciu [szablonu Azure Menedżera zasobów](../resource-group-authoring-templates.md).  Aby wyeksportować istniejący widok, otwórz widok, klikając jego kafelka w głównym pulpitu nawigacyjnego usługi OMS.  Następnie kliknij przycisk **Eksportuj** , aby utworzyć plik w folderze pobierania w przeglądarce.  Nazwa pliku będzie nazwą widoku z rozszerzeniem *omsview*.

![Eksportowanie widoku](media/log-analytics-view-designer/edit-menu-export.png)

### <a name="import-an-existing-view"></a>Importowanie istniejącego widoku
Możesz zaimportować plik *omsview* wyeksportowany z innej grupy zarządzania.  Aby zaimportować istniejący widok, należy najpierw utworzyć nowy widok.  Kliknij przycisk **Importuj** i wybierz plik *omsview* .  Konfiguracji w pliku zostaną skopiowane do istniejącego widoku.

![Eksportowanie widoku](media/log-analytics-view-designer/edit-menu-import.png)

## <a name="working-with-view-designer"></a>Praca z Projektant widoków
Projektant widoków występują trzy okienka.  Okienko **projektu** reprezentuje widoki niestandardowe.  Po dodaniu kafelków i elementy w panelu **sterowania** do okienka **Projektowanie** dodanych do widoku.  W okienku **Właściwości** zostaną wyświetlone właściwości sąsiadująco lub wybranej części.

![Projektant widoków](media/log-analytics-view-designer/view-designer-screenshot.png)

### <a name="configure-view-tile"></a>Konfigurowanie widoku kafelków
Widok niestandardowy może zawierać tylko pojedynczego fragmentu.  Wybierz kartę **kafelka** w panelu **sterowania** , aby wyświetlić bieżący fragment lub wybierz alternatywny.  W okienku **Właściwości** zostaną wyświetlone właściwości dla bieżącego kafelka.  Konfigurowanie właściwości kafelków zgodnie z szczegółowe informacje w [Kafelka odwołania](log-analytics-view-designer-tiles.md) , a następnie kliknij przycisk **Zastosuj** , aby zapisać zmiany.

### <a name="configure-visualization-parts"></a>Konfigurowanie części wizualizacji
Widok może zawierać dowolną liczbę części wizualizacji.  Wybierz kartę **Widok** , a następnie część wizualizacji, aby dodać do widoku.  W okienku **Właściwości** zostaną wyświetlone właściwości wybranej części.  Konfigurowanie właściwości widoku zgodnie z szczegółowe informacje w [odwołanie do części wizualizacji](log-analytics-view-designer-parts.md) i kliknij przycisk **Zastosuj** , aby zapisać zmiany.

### <a name="delete-a-visualization-part"></a>Usuwanie części wizualizacji
Część wizualizacji można usunąć z widoku, klikając przycisk **X** w prawym górnym rogu obszaru.

### <a name="rearrange-visualization-parts"></a>Zmienianie rozmieszczenia części wizualizacji
Widoki mieć tylko jeden wiersz części wizualizacji.  Zmienianie rozmieszczenia istniejące składniki w widoku, klikając i przeciągając je do nowej lokalizacji.


## <a name="next-steps"></a>Następne kroki

- Dodawanie [kafelków](log-analytics-view-designer-tiles.md) do widoku niestandardowym.
- Dodawanie [Składników wizualizacji](log-analytics-view-designer-parts.md) do widoku niestandardowym.
