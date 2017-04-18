<properties 
   pageTitle="Pulpit nawigacyjny z usługi Menedżera StorSimple — wirtualnej tablicy | Microsoft Azure"
   description="W tym artykule opisano pulpit nawigacyjny usługi Menedżera StorSimple i wyjaśniono, jak używać tej funkcji Monitorowanie kondycji macierzy Virtual StorSimple."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/07/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-dashboard-for-the-storsimple-virtual-array"></a>Pulpit nawigacyjny usługi Menedżera StorSimple za pomocą tablicy wirtualnych StorSimple

## <a name="overview"></a>Omówienie

Strona pulpitu nawigacyjnego usługi Menedżer StorSimple zawiera widoku podsumowania StorSimple wirtualnych tablic (nazywane także StorSimple lokalnej wirtualną urządzeń i urządzeń wirtualnych) podłączonych do usługę Menedżer StorSimple wyróżnienie tych, które wymagają uwagi administratorem systemu. Ten samouczek wprowadza strony pulpitu nawigacyjnego, omówiono zawartości pulpitu nawigacyjnego i funkcji i opisano zadania, które można wykonywać na tej stronie.

![Pulpit nawigacyjny usługi](./media/storsimple-ova-service-dashboard/dashboard1.png)

Pulpit nawigacyjny usługi Menedżera StorSimple są wyświetlane następujące informacje:

- W obszarze **wykresu** w górnej części strony można wyświetlić odpowiednie metryki dla urządzenia wirtualnego. Możesz wyświetlić podstawową pamięć używany na wszystkich urządzeniach wirtualnych, a także zużywanej przez urządzeń wirtualnych w okresie magazynu w chmurze. Aby określić względną lub bezwzględną zastosowania i wyświetlić skali czasu 1 tydzień, 1 miesiąc, 3 miesięcy lub lat 1 za pomocą kontrolek w prawym górnym rogu wykresu. Sterowanie odświeżaniem za pomocą ![Sterowanie odświeżaniem](./media/storsimple-ova-service-dashboard/refresh-control.png) odświeżyć na wykresie.

- **Omówienie zastosowania** przedstawia podstawową pamięć obsługi administracyjnej i zużywanej przez wszystkich urządzeń wirtualnych względem całkowita ilość miejsca dostępna na wszystkich urządzeniach wirtualną. **Provisioned** odwołuje się do ilości miejsca do magazynowania, który jest przygotowana przydzielonych do użycia, **używane** odwołuje się do zastosowania akcje lub wielkości jako wyświetlane przez inicjator, które są połączone z urządzenia wirtualne i **pojemność maks.** pokazuje maksymalną pojemność wszystkich urządzeń wirtualnych.

- Obszar **alerty** stanowi migawki wszystkie alerty aktywne na wszystkich urządzeniach wirtualnych pogrupowane według ważności alertu. Kliknięcie poziom ważności otwiera stronę **alertów** , występujące w celu pokazania alertów. Na stronie **alerty** możesz kliknąć poszczególne alert, aby wyświetlić dodatkowe informacje dotyczące tego alert, w tym zalecane akcji. Jeśli problem został rozwiązany, można także wyczyścić alert.

- Obszar **zadania** zawiera migawkę ostatnie zadań na wszystkich urządzeniach wirtualnych, które są połączone z tej usługi. Istnieją łącza, które umożliwiają przeglądanie zadań, które są obecnie w toku i tych, które zakończyła się pomyślnie lub Niepowodzenie w ciągu ostatnich 24 godzin. 

- Obszar **Szybkie rzut** po prawej stronie zawiera przydatne informacje, takie jak liczba wirtualnych urządzeń podłączonych do usługi, lokalizację usługi i szczegóły subskrypcji, który jest skojarzony z usługą stan usługi. Istnieje także łącza do dziennika operacji. Kliknij łącze, aby wyświetlić listę wszystkich operacji wykonanych usługi Menedżera StorSimple. 

Strony pulpitu nawigacyjnego usług Menedżera StorSimple umożliwia inicjować następujące zadania:

- Uzyskiwanie klucza rejestru usługi.
- Wyświetlanie dzienników operacji.

## <a name="get-the-service-registration-key"></a>Uzyskiwanie klucza rejestru usługi

Klucz rejestru usługi służy do zarejestrować urządzenie wirtualne StorSimple z usługą Menedżera StorSimple tak, aby pojawia się w portalu klasyczny Azure dla przyszłych zarządzania działań. Klucz jest tworzony na pierwszym urządzeniu wirtualnych i udostępnione dla pozostałych urządzeń wirtualnych. 

Kliknięcie **Klucza rejestru** (u dołu strony) powoduje otwarcie okna dialogowego **Klucza rejestru usługi** , miejsce, w którym można skopiować bieżącego klucza rejestru usługi do Schowka lub wygenerować klucza rejestru usługi.

![klucz rejestru](./media/storsimple-ova-service-dashboard/service-dashboard3.png)

Ponowne generowanie klucza nie wpływa na wcześniej zarejestrowanych urządzeń wirtualnych: dotyczy tylko urządzeń wirtualnych zarejestrowane w usłudze po klucz jest generowany ponownie.

Aby uzyskać więcej informacji na temat wprowadzenie klucza rejestru usługi przejdź do pozycji [Pobierz klucza rejestru usługi](storsimple-ova-manage-service.md#get-the-service-registration-key).

## <a name="view-the-operations-logs"></a>Wyświetlanie dzienników operacji

Dzienniki operacji można wyświetlić, klikając łącze dzienników operacji dostępna w okienku **Szybkie rzut** pulpitu nawigacyjnego. Spowoduje to przejście do karty usługi zarządzania: miejsce, w którym można filtrować i zobacz dzienniki określonym w usłudze Menedżer StorSimple.

![Dziennik działań](./media/storsimple-ova-service-dashboard/ops-log.png)

## <a name="next-steps"></a>Następne kroki

Dowiedz się, jak [używać lokalnego interfejsu użytkownika umożliwiającego administrowanie macierzy Virtual StorSimple sieci web](storsimple-ova-web-ui-admin.md).