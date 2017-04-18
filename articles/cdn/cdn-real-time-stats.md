<properties
    pageTitle="Real-jednorazowej-statystykę w Azure CDN | Microsoft Azure"
    description="W czasie rzeczywistym statystyki zawiera dane dotyczące wydajności sieci CDN Azure w czasie rzeczywistym, gdy dostarczania zawartości dla klientów."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>

# <a name="real-time-stats-in-microsoft-azure-cdn"></a>W czasie rzeczywistym statystykę w programie Microsoft Azure CDN

[AZURE.INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Omówienie

W tym dokumencie omówiono statystykę w czasie rzeczywistym w programie Microsoft Azure CDN.  Ta funkcja zapewnia danych w czasie rzeczywistym, takich jak przepustowość, stany pamięci podręcznej i równoczesne połączenia do swojego profilu CDN, kiedy dostarczania zawartości dla klientów. Dzięki temu ciągłe monitorowanie kondycji usługi w dowolnym momencie, w tym live Przejdź zdarzeń.

Dostępne są następujące wykresów:

* [Przepustowości](#bandwidth)
* [Kody stanu](#status-codes)
* [Statusy pamięci podręcznej](#cache-statuses)
* [Połączenia](#connections)


## <a name="accessing-real-time-stats"></a>Uzyskiwanie dostępu do statystykę w czasie rzeczywistym

1. W [Azure Portal](https://portal.azure.com)przejdź do swojego profilu CDN.

    ![Karta profil sieci CDN](./media/cdn-real-time-stats/cdn-profile-blade.png)

2. Z sieci CDN karta profilu kliknij przycisk **Zarządzaj** .

    ![Przycisk Zarządzaj CDN karta profilu](./media/cdn-real-time-stats/cdn-manage-btn.png)

    Zostanie otwarty do portalu zarządzania CDN.

3. Umieść wskaźnik myszy na karcie **analizy** , a następnie umieść wskaźnik myszy na menu wysuwane **Statystykę w czasie rzeczywistym** .  Kliknij **obiekt dużych HTTP**.

    ![Portal zarządzania CDN](./media/cdn-real-time-stats/cdn-premium-portal.png)

    Są wyświetlane na wykresach statystykę w czasie rzeczywistym.
    
Każda wykresów wyświetla w czasie rzeczywistym statystyki dla wybranego okresu, rozpoczynając po załadowaniu strony.  Wykresy są aktualizowane automatycznie co kilka sekund.  Przycisk **Odśwież wykres** , jeśli istnieją, wyczyści wykresu, po upływie którego zostaną wyświetlone tylko zaznaczonych danych.

## <a name="bandwidth"></a>Przepustowości

![Wykres przepustowości](./media/cdn-real-time-stats/cdn-bandwidth.png)

Na wykresie **przepustowości** jest wyświetlana ilość przepustowość dla bieżącej platformy w wybranym okresie. Zacieniowany część wykresu wskazuje wykorzystania przepustowości. Na określonej liczbie przepustowości obecnie używana jest wyświetlany bezpośrednio pod wykres liniowy.

## <a name="status-codes"></a>Kody stanu

![Wykres kodu stanu](./media/cdn-real-time-stats/cdn-status-codes.png)

Na wykresie **Kody stanu** wskazuje, ile razy występuje niektóre kody odpowiedzi HTTP w wybranym okresie.

> [AZURE.TIP]  Aby uzyskać opis poszczególnych opcji kodu stanu HTTP zobacz [Kody stanu HTTP sieci CDN Azure](https://msdn.microsoft.com/library/mt759238.aspx).

Zostanie wyświetlona lista kodów stanu HTTP bezpośrednio powyżej wykresu. Tej listy wskazuje każdego kodu stanu, zawierającego wykres liniowy i bieżącej liczba wystąpień na sekundę kodu stanu. Domyślnie zostanie wyświetlony wiersz dla każdego z tych kodów stanu na wykresie. Jednak można monitorować tylko kody stanu, które mają specjalne znaczenie dla danej konfiguracji sieci CDN. W tym celu należy sprawdzić kody żądany stan wyczyść wszystkie pozostałe opcje, a następnie kliknij pozycję **Odśwież wykresu**. 

Możesz tymczasowo ukryć zarejestrowane dane dla określony kod stanu.  W legendzie bezpośrednio poniżej wykresu kliknij kodu stanu, który chcesz ukryć. Kod stanu będą od razu ukryte z wykresu. Ponowne kliknięcie tego kodu stanu spowoduje tę opcję, aby ponownie wyświetlane.

## <a name="cache-statuses"></a>Statusy pamięci podręcznej

![Wykres statusy pamięci podręcznej](./media/cdn-real-time-stats/cdn-cache-status.png)

Na wykresie **Statusy pamięci podręcznej** wskazuje, ile razy występuje niektórych rodzajów statusy pamięci podręcznej w wybranym okresie. 

> [AZURE.TIP]  Aby uzyskać opis poszczególnych opcji kodu stanu pamięci podręcznej zobacz [Kody stanu pamięci podręcznej CDN Azure](https://msdn.microsoft.com/library/mt759237.aspx).

Zostanie wyświetlona lista kodów stanu pamięci podręcznej bezpośrednio powyżej wykresu. Tej listy wskazuje każdego kodu stanu, zawierającego wykres liniowy i bieżącej liczba wystąpień na sekundę kodu stanu. Domyślnie zostanie wyświetlony wiersz dla każdego z tych kodów stanu na wykresie. Jednak można monitorować tylko kody stanu, które mają specjalne znaczenie dla danej konfiguracji sieci CDN. W tym celu należy sprawdzić kody żądany stan wyczyść wszystkie pozostałe opcje, a następnie kliknij pozycję **Odśwież wykresu**. 

Możesz tymczasowo ukryć zarejestrowane dane dla określony kod stanu.  W legendzie bezpośrednio poniżej wykresu kliknij kodu stanu, który chcesz ukryć. Kod stanu będą od razu ukryte z wykresu. Ponowne kliknięcie tego kodu stanu spowoduje tę opcję, aby ponownie wyświetlane.

## <a name="connections"></a>Połączenia

![Wykres połączenia](./media/cdn-real-time-stats/cdn-connections.png)

Ten wykres wskazuje, ile połączenia zostały utworzone w celu serwery krawędzi. Każdy wniosek o środka trwałego, które przechodzi przez naszych CDN wyniki w przypadku połączenia.

## <a name="next-steps"></a>Następne kroki

- Bądź na bieżąco z [alertów w czasie rzeczywistym w sieci CDN Azure](cdn-real-time-alerts.md)
- Wyświetlić elementy [zaawansowanych](cdn-advanced-http-reports.md) raportów HTTP
- Analizowanie [upodobania](cdn-analyze-usage-patterns.md)

