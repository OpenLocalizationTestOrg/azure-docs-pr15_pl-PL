<properties
    pageTitle="Tworzenie niestandardowego pulpitu nawigacyjnego w dzienniku analizy | Microsoft Azure"
    description="Ten przewodnik pomaga zrozumieć, jak pulpity nawigacyjne analizy dziennika można wizualizowanie wszystkich wyszukiwań zapisany dziennik, umożliwiając pojedynczy obiektyw wyświetlanie środowiska."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="create-a-custom-dashboard-in-log-analytics"></a>Tworzenie niestandardowego pulpitu nawigacyjnego do analizy dziennika

Ten przewodnik pomaga zrozumieć, jak pulpity nawigacyjne analizy dziennika można wyświetlać wizualizację wszystkich wyszukiwań zapisany dziennik, umożliwiając pojedynczy obiektyw wyświetlanie środowiska.

![Przykładowy pulpit nawigacyjny](./media/log-analytics-dashboards/oms-dashboards-example-dash.png)

Wszystkie niestandardowe pulpitów nawigacyjnych, które zostały utworzone w portalu usługi OMS są również dostępne w aplikacji Mobile usługi OMS. Zobacz następujące strony, aby uzyskać więcej informacji na temat aplikacji.

- [Usługi OMS aplikacji dla urządzeń przenośnych z Microsoft Store](http://www.windowsphone.com/store/app/operational-insights/4823b935-83ce-466c-82bb-bd0a3f58d865)
- [Usługi OMS aplikacji dla urządzeń przenośnych z programu iTunes firmy Apple](https://itunes.apple.com/app/microsoft-operations-management/id1042424859?mt=8)

![pulpit nawigacyjny urządzeń przenośnych](./media/log-analytics-dashboards/oms-search-mobile.png)

## <a name="how-do-i-create-my-dashboard"></a>Jak utworzyć mój pulpit nawigacyjny?

Aby rozpocząć, przejdź do strony usługi OMS Przegląd. Zobaczysz kafelków **Moje pulpitu nawigacyjnego** po lewej stronie. Kliknij, aby przechodzić do pulpitu nawigacyjnego.

![Omówienie](./media/log-analytics-dashboards/oms-dashboards-overview.png)


## <a name="adding-a-tile"></a>Dodawanie kafelka

W pulpitach nawigacyjnych Kafelki są obsługiwane przez wyszukiwanie zapisany dziennik. Usługi OMS zawiera wiele wstępnie wprowadzone zapisany dziennik wyszukiwania, aby mógł zacząć od razu. Wykonaj następujące czynności, które w konspekcie, jak rozpocząć.

W widoku Moje pulpitu nawigacyjnego, po prostu kliknij przycisk **Dostosuj** , aby wprowadzić dostosowywanie tryb.

![Obrazkami](./media/log-analytics-dashboards/oms-dashboards-pictorial01.png)

 Panel, który zostanie otwarty w prawej części strony pokazuje wszystkie wyszukiwań zapisany dziennik obszaru roboczego. Wizualizować wyszukiwania dziennika zapisanych jako kafelka, umieść wskaźnik myszy na zapisanego wyszukiwania, a następnie kliknij odpowiedni symbol **plus** .

![Dodawanie kafelków 1](./media/log-analytics-dashboards/oms-dashboards-pictorial02.png)

Po kliknięciu przycisku **plus** symbol fragment jest wyświetlany w widoku Moje pulpitu nawigacyjnego.

![Dodawanie kafelków 2](./media/log-analytics-dashboards/oms-dashboards-pictorial03.png)


## <a name="edit-a-tile"></a>Edytowanie kafelka

W widoku Moje pulpitu nawigacyjnego, po prostu kliknij przycisk **Dostosuj** , aby wprowadzić dostosowywanie tryb. Kliknij Kafelek, który chcesz edytować. Zmiany prawym panelu, aby edytować i udostępnia wybór opcji:

![Edytowanie kafelka](./media/log-analytics-dashboards/oms-dashboards-pictorial04.png)

![Edytowanie kafelka](./media/log-analytics-dashboards/oms-dashboards-pictorial05.png)

### <a name="tile-visualizations"></a>Wizualizacje kafelków#
Istnieją trzy rodzaje wizualizacje kafelków do wyboru:

|Typ wykresu|działanie|
|---|---|
|![Wykres słupkowy](./media/log-analytics-dashboards/oms-dashboards-bar-chart.png)|Wyświetla oś czasu wyników wyszukiwania zapisany dziennik jako wykresu słupkowego lub listy wyników za pomocą pola w zależności od tego, jeżeli wyszukiwaniu dziennika agreguje wyniki za pomocą pola lub nie.
|![Metryka](./media/log-analytics-dashboards/oms-dashboards-metric.png)|Wyświetlanie odwołań wynik wyszukiwania usługi całkowita w postaci liczby na kafelku. Kafelki metryczne umożliwiają ustawianie próg wyróżni fragmentu po osiągnięciu progu.|
|![wiersz](./media/log-analytics-dashboards/oms-dashboards-line.png)|Wyświetla oś czasu z odwołań wynik wyszukiwania zapisany dziennik z wartościami jako wykres liniowy.|

### <a name="threshold"></a>Progowa.
Możesz utworzyć progu na kafelku, przy użyciu metryczne wizualizacji. Wybierz pozycję na tworzenie wartość progowa na kafelku. Określ, czy chcesz wyróżnić kafelku, gdy wartość jest powyżej lub poniżej progu wybranym, a następnie ustaw wartość progowa poniżej.

## <a name="organizing-the-dashboard"></a>Organizowanie pulpitu nawigacyjnego
Organizowanie pulpitu nawigacyjnego, przejdź do widoku Moje pulpitu nawigacyjnego i kliknij przycisk **Dostosuj** , aby wprowadzić dostosowywanie tryb. Kliknij i przeciągnij fragment, który chcesz przenieść i przeciągnij ją w odpowiednie miejsce kafelka, należy.

![Organizowanie pulpitu nawigacyjnego](./media/log-analytics-dashboards/oms-dashboards-organize.png)

## <a name="remove-a-tile"></a>Usuwanie kafelka
Usuwanie kafelka, przejdź do widoku Moje pulpitu nawigacyjnego i kliknij przycisk **Dostosuj** , aby wprowadzić dostosowywanie tryb. Wybierz Kafelek, który chcesz usunąć, a następnie w prawym panelu wybierz, **Usuwanie kafelków**.

![Usuwanie kafelka](./media/log-analytics-dashboards/oms-dashboards-remove-tile.png)

## <a name="next-steps"></a>Następne kroki

- Tworzenie [alertów](log-analytics-alerts.md) w analizy dziennika do generowania powiadomień i korygowania problemów.
