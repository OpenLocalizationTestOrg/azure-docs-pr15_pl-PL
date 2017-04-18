<properties 
    pageTitle="Przewodnik: Monitorowanie systemu Microsoft Dynamics CRM z wniosków aplikacji" 
    description="Pobieranie telemetrycznego z Microsoft Dynamics CRM Online przy użyciu aplikacji wnioski. Przewodnik po instalacji, wprowadzenie danych, wizualizacji i Eksportuj." 
    services="application-insights" 
    documentationCenter=""
    authors="mazharmicrosoft" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/17/2015" 
    ms.author="awills"/>
 
# <a name="walkthrough-enabling-telemetry-for-microsoft-dynamics-crm-online-using-application-insights"></a>Przewodnik: Włączanie telemetrycznego dla programu Microsoft Dynamics CRM Online przy użyciu aplikacji wniosków

W tym artykule pokazano, jak uzyskać telemetrycznego danych z [Programu Microsoft Dynamics CRM Online](https://www.dynamics.com/) przy użyciu [Programu Visual Studio aplikacji wnioski](https://azure.microsoft.com/services/application-insights/). Przedstawimy każde Zakończ proces dodawania aplikacji wniosków skrypt do aplikacji, przechwytywanie danych i wizualizacji danych.

>[AZURE.NOTE] [Przeglądanie rozwiązanie próbki](https://dynamicsandappinsights.codeplex.com/).

## <a name="add-application-insights-to-new-or-existing-crm-online-instance"></a>Dodawanie aplikacji wniosków do nowego lub istniejącego wystąpienia CRM Online 

Monitorowanie aplikacji, możesz dodać SDK wniosków aplikacji do aplikacji. Zestaw SDK wysyła telemetrycznego do [portalu wniosków aplikacji](https://portal.azure.com), miejsce, w którym można użyć naszych Zaawansowana analiza i narzędzia diagnostyczne lub eksportowanie danych do magazynowania.

### <a name="create-an-application-insights-resource-in-azure"></a>Tworzenie zasób wniosków aplikacji platformy Azure

1. Uzyskiwanie [konta w programie Microsoft Azure](http://azure.com/pricing). 
2. Zaloguj się do [portalu Azure](https://portal.azure.com) i dodawanie nowego zasobu wniosków aplikacji. Jest to miejsce, w którym opracowywania i wyświetlania danych.

    ![Kliknij przycisk +, usług dla deweloperów, wniosków aplikacji.](./media/app-insights-sample-mscrm/01.png)

    Wybierz ASP.NET jako typ aplikacji.

3. Otwórz kartę Szybkie uruchamianie i otworzyć skrypt kodu.

    ![](./media/app-insights-sample-mscrm/03.png)

**Nie zamykaj strony kodu** podczas następnego kroku w innym oknie przeglądarki. Wkrótce musisz kodu. 

### <a name="create-a-javascript-web-resource-in-microsoft-dynamics-crm"></a>Utwórz zasób sieci web kodu JavaScript w programie Microsoft Dynamics CRM

1. Otwórz wystąpienie CRM Online i zaloguj się z uprawnieniami administratora.
2. Otwórz program Microsoft Dynamics CRM ustawienia, dostosowania, dostosowywanie systemu

    ![](./media/app-insights-sample-mscrm/04.png)
    
    ![](./media/app-insights-sample-mscrm/05.png)


    ![](./media/app-insights-sample-mscrm/06.png)

3. Utwórz zasób JavaScript.

    ![](./media/app-insights-sample-mscrm/07.png)

    Nadaj plikowi nazwę, wybierz **skrypt (JScript)** i Otwórz Edytor tekstu.

    ![](./media/app-insights-sample-mscrm/08.png)
    
4. Skopiuj kod z wniosków aplikacji. Podczas kopiowania upewnij się zignorować znaczników skryptów. Zapoznaj się poniżej zrzut ekranu:

    ![](./media/app-insights-sample-mscrm/09.png)

    Kod zawiera klucz oprzyrządowania, identyfikujący zasób wniosków aplikacji.

5. Zapisz i opublikuj.

    ![](./media/app-insights-sample-mscrm/10.png)

### <a name="instrument-forms"></a>Formularze dokumentu

1. W programie Microsoft CRM Online otwieranie formularza klienta

    ![](./media/app-insights-sample-mscrm/11.png)

2. Otwórz formularz właściwości

    ![](./media/app-insights-sample-mscrm/12.png)

3. Dodaj zasób sieci web JavaScript utworzone przez Ciebie

    ![](./media/app-insights-sample-mscrm/13.png)

    ![](./media/app-insights-sample-mscrm/14.png)

4. Zapisywanie i publikowanie wprowadzone dostosowania formularza.


## <a name="metrics-captured"></a>Metryki przechwycone

Teraz masz skonfigurowane przechwytywania telemetrycznego dla formularza. Zawsze, gdy są one używane, dane zostaną wysłane do zasobu wniosków aplikacji.

Poniżej przedstawiono przykłady danych, która zostanie wyświetlona.

#### <a name="application-health"></a>Kondycja aplikacji

![](./media/app-insights-sample-mscrm/15.png)

![](./media/app-insights-sample-mscrm/16.png)

Wyjątki w przeglądarce:

![](./media/app-insights-sample-mscrm/17.png)

Kliknij wykres, aby uzyskać więcej szczegółów:

![](./media/app-insights-sample-mscrm/18.png)

#### <a name="usage"></a>Użycie

![](./media/app-insights-sample-mscrm/19.png)

![](./media/app-insights-sample-mscrm/20.png)

![](./media/app-insights-sample-mscrm/21.png)

#### <a name="browsers"></a>Przeglądarki

![](./media/app-insights-sample-mscrm/22.png)

![](./media/app-insights-sample-mscrm/23.png)

#### <a name="geolocation"></a>Geolokalizacja

![](./media/app-insights-sample-mscrm/24.png)

![](./media/app-insights-sample-mscrm/25.png)

#### <a name="inside-page-view-request"></a>Wezwanie na środku strony widoku

![](./media/app-insights-sample-mscrm/26.png)

![](./media/app-insights-sample-mscrm/27.png)

![](./media/app-insights-sample-mscrm/28.png)

![](./media/app-insights-sample-mscrm/29.png)

![](./media/app-insights-sample-mscrm/30.png)

## <a name="sample-code"></a>Przykładowy kod

[Przeglądanie przykładowy kod](https://dynamicsandappinsights.codeplex.com/).

## <a name="power-bi"></a>Power BI

Możesz wykonywać nawet dokładniejszej analizy, jeśli [Eksportowanie danych do programu Microsoft Power BI](app-insights-export-power-bi.md).

## <a name="sample-microsoft-dynamics-crm-solution"></a>Przykładowe systemu Microsoft Dynamics CRM rozwiązanie

[Poniżej przedstawiono rozwiązanie przykładowych zaimplementowana w programie Microsoft Dynamics CRM] (https://dynamicsandappinsights.codeplex.com/).

## <a name="learn-more"></a>Dowiedz się więcej

* [Co to jest aplikacja wniosków?](app-insights-overview.md)
* [Wnioski aplikacji dla stron sieci web](app-insights-javascript.md)
* [Więcej przykładów i instruktaży](app-insights-code-samples.md)

 
