<properties 
   pageTitle="Wyświetlanie i zarządzanie zadaniami tablicy Virtual StorSimple | Microsoft Azure"
   description="W tym artykule opisano stronę Menedżera StorSimple usługi zadań i jak go używać do śledzenia zadań ostatnich i bieżącej tablicy Virtual StorSimple."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor=""/>
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="06/07/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-view-jobs-for-the-storsimple-virtual-array"></a>Wyświetlanie zadań w tabeli wirtualnej StorSimple za pomocą usługi Menedżera StorSimple

## <a name="overview"></a>Omówienie

Na stronie **zadania** jest jednym portalu centralnej do przeglądania i zarządzania zadaniami, które są uruchamiane na tablic wirtualnych (nazywane także lokalnej wirtualną urządzenia) podłączonych do usługi Menedżera StorSimple. Możesz wyświetlać zadań uruchomiony, ukończone i nie powiodło się dla wielu urządzeń wirtualnych. Wyniki są przedstawione w formie tabeli. 

![Strona zadania](./media/storsimple-ova-manage-jobs/ovajobs1.png)

Można szybko znaleźć zadania, które chcesz się przez filtrowanie do pól, takich jak:

- **Stan** — możesz wyszukać wszystkich zadań uruchomionego, ukończone i nie powiodło się.
- **Od i do** — zadania mogą być filtrowane w oparciu o zakres dat i godzin.
- **Typ** — typ zadania mogą być wszystkie kopii zapasowej, Przywracanie pracy awaryjnej, Pobierz aktualizacje lub instalowanie aktualizacji.
- **Urządzenia** — zadania są inicjowane na urządzeniu określonego połączony z usługą. Filtrowanych zadań są następnie oznaczane znakami tabulacji na podstawie następujące atrybuty:

    - **Typ** — typ zadania mogą być wszystkie kopii zapasowej, Przywracanie pracy awaryjnej, Pobierz aktualizacje lub instalowanie aktualizacji.

    - **Stan** — zadania mogą być wszystkie uruchomione, ukończony lub nie powiodło się.

    - **Jednostka** — zadania można skojarzyć z głośność, Udostępnij lub urządzenia. 

    - **Urządzenia** — nazwę urządzenia, na której zadanie zostało uruchomione.

    - **Wprowadzenie na** — czas rozpoczęcia zadania.

    - **Informacje o postępie** — uzupełniania wartości procentowej uruchomione zadanie. Ukończone zadania to zawsze wynosić 100%.

Listy zadań są odświeżane co 30 sekund.

## <a name="view-job-details"></a>Wyświetlanie szczegółów zadania

Wykonaj poniższe czynności, aby wyświetlić szczegóły każde zadanie.

#### <a name="to-view-job-details"></a>Aby wyświetlić szczegóły zadania

1. Na stronie **zadania** wyświetlane zadania, które chcesz się uruchamiania kwerendy z odpowiednie filtry. Można wyszukiwać zadań wykonanych lub nie działa.

2. Wybierz zadanie z tabelarycznym listy zadań.

3. U dołu strony kliknij przycisk **Szczegóły**.

4. W oknie dialogowym **Szczegóły** możesz wyświetlać stan, szczegóły i Statystyki czasu. Na poniższej ilustracji przedstawiono przykład okno dialogowe **Szczegóły zadania kopii zapasowej** .
 
    ![Strona Szczegóły zadania](./media/storsimple-ova-manage-jobs/ovajobs2.png)

#### <a name="job-failures-when-the-virtual-machine-is-paused-in-the-hypervisor"></a>Błędy zadań po zatrzymaniu wskaźnika na monitor maszyny wirtualnej maszyny wirtualnej

Gdy zadanie jest postępów macierzy Virtual StorSimple i urządzenia (maszyna wirtualna obsługi administracyjnej w monitor maszyny wirtualnej) została wstrzymana dla większe niż 15 minut, zadanie zakończy się niepowodzeniem. To z powodu czasem StorSimple wirtualnych tablicy jest zsynchronizowany z godziną Microsoft Azure. Przykład awarię Przywróć zadania są wyświetlane w następujących zrzut ekranu.

![Przywracanie błąd zadania](./media/storsimple-ova-manage-jobs/restorejobfailure.png)

Te błędy zostaną zastosowane do zadania kopii zapasowej, Przywracanie aktualizacji i pracy awaryjnej. Jeśli komputer wirtualnych jest obsługi administracyjnej w funkcji Hyper-V, komputera po pewnym czasie synchronizowanie czasu przy użyciu usługi monitor maszyny wirtualnej. Po takiej sytuacji można ponownie uruchomić zadania. 

## <a name="next-steps"></a>Następne kroki

[Dowiedz się, jak korzystanie z lokalnego interfejsu użytkownika umożliwiającego administrowanie macierzy Virtual StorSimple w sieci web](storsimple-ova-web-ui-admin.md).
