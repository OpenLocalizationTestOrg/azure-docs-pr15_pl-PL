<properties
    pageTitle="Skalowanie licznik wystąpień ręcznie lub automatycznie | Microsoft Azure"
    description="Dowiedz się, jak skalowanie usług Azure."
    authors="rboucher"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2015"
    ms.author="robb"/>

# <a name="scale-instance-count-manually-or-automatically"></a>Skalowanie licznik wystąpień ręcznie lub automatycznie

W [Azure Portal](https://portal.azure.com/)wystąpienia liczba tej usługi można ustawić ręcznie lub można ustawić parametry ona automatycznie skali bazującej na żądanie. To zazwyczaj jest określane jako *Skala się* lub *Skala w*.

Przed skalowania na podstawie licznik wystąpień, należy rozważyć, czy skalowania zależy od **poziomu ceny** oprócz licznik wystąpień. Różne poziomy cennik może mieć różną rdzenie i pamięci, a więc będą oni mieli lepszą wydajność dla taką samą liczbę wystąpień (czyli *Skala w górę* lub *Skala w dół*). W tym artykule opisano specjalnie *skalę* i *wychodzącego*.

Można skalować w portalu, a także umożliwia [Interfejsu API usługi REST](https://msdn.microsoft.com/library/azure/dn931953.aspx) lub [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) dostosować skalę ręcznie lub automatycznie.

> [AZURE.NOTE] W tym artykule opisano, jak utworzyć ustawienie autoscale w portalu pod adresem [http://portal.azure.com](http://portal.azure.com). Ustawienia Autoscale utworzonego w tym portalu nie może być edytowany portalu klasyczny ([http://manage.windowsazure.com](http://manage.windowsazure.com)).

## <a name="scaling-manually"></a>Ręczne skalowania

1. [Azure Portal](https://portal.azure.com/)kliknij przycisk **Przeglądaj**, a następnie przejdź do zasobu, który chcesz skalować, takich jak **plan usług aplikacji**.

2. Kafelka **Skala** w **operacji** informuje o stanie skalowania: **wyłączone** po możesz są skalowania ręcznie, **na** kiedy są skalowania przez jeden lub więcej wskaźniki.
    ![Skala kafelków](./media/insights-how-to-scale/Insights_UsageLens.png)

3. Kliknięcie w polu spowoduje przejście do karta **Skala** . W górnej części karta Skala widać Historia akcje autoscale usługę.  
    ![Karta Skala](./media/insights-how-to-scale/Insights_ScaleBladeDayZero.png)

>[AZURE.NOTE] Tylko akcje wykonywane przez autoscale będą widoczne na tym wykresie. Jeśli ręcznie dopasować licznik wystąpień, zmiany nie zostaną uwzględnione na tym wykresie.

4. Liczba **wystąpień** suwak ręcznie można dostosować.
5. Kliknij polecenie **Zapisz** i można będzie można skalować do tego liczbę wystąpień niemal natychmiast.

## <a name="scaling-based-on-a-pre-set-metric"></a>Skalowanie według wstępnie ustawione jednostki metryczne

Liczba wystąpień, aby automatycznie dopasować w zależności od metryki, wybierz pozycję metryczne, który ma na liście rozwijanej **skali przez** . Na przykład **plan usług aplikacji** można skalować procentu **Procesora**.

1. Po wybraniu metryki zostanie wyświetlony suwaka lub pól tekstowych, aby wprowadzić liczbę wystąpień, który chcesz skalować między:

    ![Karta Skala z wartościami procentowymi Procesora](./media/insights-how-to-scale/Insights_ScaleBladeCPU.png)

    Autoscale nigdy nie będą miały usługi poniżej lub powyżej granic, które zostanie ustawiona, niezależnie od tego, do ładowania.

2. Drugi możesz wybrać zakres docelowej metryki. Na przykład w przypadku wybrania **wartości procentowej Procesora**można ustawić obiekt docelowy procesora i średnia wszystkich wystąpień w usłudze. Skalę się wydarzy gdy średnia Procesora przekracza maksymalny zdefiniowanych, podobnie, skali w stanie zawsze, gdy średnia Procesora mniejsza niż wartość minimalna.

3. Kliknij polecenie **Zapisz** . Autoscale sprawdzi co kilka minut, aby się upewnić, że są w zakresie wystąpienia i docelowej dla swojego metryki. Gdy usługi otrzymuje dodatkowy ruch, zostanie wyświetlony więcej wystąpień bez wprowadzania.

## <a name="scale-based-on-other-metrics"></a>Skala na podstawie innych metryk

Można skalować metryki na podstawie innego niż ustawienia wstępne, które są wyświetlane na liście rozwijanej **Skalowanie przez** i można nawet złożony zestaw skali się i skalowanie w regułach.

### <a name="adding-or-changing-a-rule"></a>Dodawanie lub zmienianie reguły

1. Wybierz pozycję **reguły harmonogram i wydajności** na liście rozwijanej **skali przez** : ![reguły wydajności](./media/insights-how-to-scale/Insights_PerformanceRules.png)

2. Jeśli wcześniej używano autoscale, na pojawi się widoku dokładnie reguły, które trzeba było.

3. Przeskalować według innej metryczne kliknij pozycję Wiersz **Dodaj regułę** . Możesz też kliknąć jedną z istniejących wierszy, aby zmienić jednostki metryczne poprzednio do metryki, który chcesz skalować przez.
![Dodawanie reguły](./media/insights-how-to-scale/Insights_AddRule.png)

4. Teraz należy wybrać metryczne, które chcesz przeskalować przez. Podczas wybierania metryczne, który istnieje kilka kwestie do rozważenia:
    * *Zasób* Metryka pochodzi z. Zazwyczaj są to samo jak zasobów, które są skalowania. Jeśli chcesz skalować przez głębokość kolejki miejsca do magazynowania zasobu jest jednak kolejki, który chcesz skalować przez.
    * *Metryka nazwy* samej.
    * *Agregacja czasu* metryki. To, jak dane są łączenie okresie *czasu trwania*.

5. Po wybraniu usługi metryki możesz wybrać próg Metryka i operator. Można na przykład powiedzieć **większe niż** **80%**.

6. Następnie wybierz akcję, którą chcesz wykonać. Istnieje kilka różnych rodzajów akcje:
    * Zwiększanie lub zmniejszanie — spowoduje to dodanie lub usunięcie **liczbę wystąpień zdefiniowanych**
    * Zwiększanie lub zmniejszanie procent — spowoduje to zmianę licznik wystąpień przez wartość procentową. Na przykład można umieścić 25 w polu **wartość** , a gdyby obecnie wystąpienia 8 czy można dodać 2.
    * Zwiększanie lub zmniejszanie czcionki do — spowoduje to ustawienie Zliczanie wystąpień **wartości** definiowania.

7. Na koniec możesz chłodzenia — jak długo ta reguła powinien czekać po poprzedniej akcji skali przeskalować ponownie.

8. Po skonfigurowaniu reguły naciśnij przycisk **OK**.

9. Po skonfigurowaniu wszystkich reguł, które mają należy koniecznie kliknij polecenie **Zapisz** .

### <a name="scaling-with-multiple-steps"></a>Skalowanie przy użyciu wielu kroków

Przykładach są bardzo proste. Jednak jeśli ma być skuteczniejsze o skalowania w górę (lub w dół), nawet dodać wiele reguł skali dla tę samą metrykę. Na przykład można zdefiniować dwie reguły skali CPU procentowa:

1. Możliwość skalowania przez wystąpienie 1 w przypadku CPU procentowa powyżej 60%
2. Możliwość skalowania przez wystąpienia 3 w przypadku CPU procentowa powyżej 85%

![Wiele reguł skali](./media/insights-how-to-scale/Insights_MultipleScaleRules.png)

Z tej reguły dodatkowe Jeśli usługi obciążenie przekracza 85% przed akcją skali, zostanie wyświetlony dwa kolejne wystąpienia zamiast jednej.

## <a name="scale-based-on-a-schedule"></a>Skala zgodnie z harmonogramem


Domyślnie podczas tworzenia reguły skali będzie zawsze stosować. Możesz zobaczyć który po kliknięciu w nagłówku profilu:

![Profil](./media/insights-how-to-scale/Insights_Profile.png)

Można mieć skuteczniejsze skalowania podczas dnia lub tygodnia, niż na weekend. Można nawet Zamknij usługi całkowicie wyłączyć godzin pracy.

1. W tym celu na profil, który ma wybierz **cyklu** zamiast **zawsze** i wybierz godziny, które mają profil, aby zastosować.

2. Na przykład aby profil, który ma zastosowanie w tygodniu, na liście rozwijanej **dni** wyczyść pole wyboru **sobotę** i **niedzielę**.

3. Aby profil, który ma zastosowanie w dziennych, ustaw **czas rozpoczęcia** pora dnia, który ma być uruchamiany w.
    ![Cykl domyślne](./media/insights-how-to-scale/Insights_ProfileRecurrence.png)

4. Kliknij **przycisk OK**.

5. Następnie należy dodać profil, który chcesz zastosować w innym czasie. Kliknij wiersz **Dodawanie profilu** .
    ![Poza pracą](./media/insights-how-to-scale/Insights_ProfileOffWork.png)

6. Nadaj nazwę nowej, druga, profilu, na przykład może połączenia go **poza pracą**.

7. Następnie ponownie wybierz **Cykl** , a następnie wybierz zakres liczba wystąpienia, które mają w tym czasie.

8. Jako profil domyślny wybierz **dni** ma tego profilu, aby zastosować do i **czas rozpoczęcia** w ciągu dnia.

>[AZURE.NOTE] Niezależnie od **strefy czasowej** , możesz wybrać Autoscale będzie używać reguł oszczędność czasu. Jednak podczas czasu letniego przesunięcie czasu UTC zostanie wyświetlona przesunięcie strefy czasowej podstawowy, nie letni przesunięcie oszczędność czasu UTC.

9. Kliknij **przycisk OK**.

10. Teraz należy dodać niezależnie od reguły, które mają być stosowane podczas drugiego profilu. Kliknij przycisk **Dodaj regułę**, a następnie można skonstruować tej samej reguły, które masz w profilu domyślnego.
    ![Dodawanie reguły do pozycji Wył pracy](./media/insights-how-to-scale/Insights_RuleOffWork.png)

11. Pamiętaj utworzyć obu regułę dla skali się i skala w lub podczas profilu licznik wystąpień tylko będą powiększanie (lub zmniejszyć).

12. Na koniec kliknij przycisk **Zapisz**.

## <a name="next-steps"></a>Następne kroki

* [Monitorowanie usługi metryki](insights-how-to-customize-monitoring.md) upewnij się, że usługi jest dostępny i odpowiada.
* [Włączanie monitorowania i diagnostyki](insights-how-to-use-diagnostics.md) zbieranie szczegółowe metryki wysokiej częstotliwości o tej usługi.
* [Odbieranie alertów](insights-receive-alert-notifications.md) przy każdym operacyjne zdarzeń lub metryki krzyżowe progu.
* [Monitorowanie wydajności aplikacji](../application-insights/app-insights-azure-web-apps.md) , jeśli chcesz poznać dokładnie tak, jak kod działa w chmurze.
* [Wyświetlanie zdarzeń i dzienników inspekcji](insights-debugging-with-events.md) Aby dowiedzieć się, wszystkie elementy podczas tej usługi.
* [Monitor dostępności i czas reakcji dowolnej strony sieci web](../application-insights/app-insights-monitor-web-app-availability.md) przy użyciu aplikacji wniosków, więc można znaleźć w przypadku strony nie działa.
