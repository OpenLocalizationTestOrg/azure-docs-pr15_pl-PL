<properties 
   pageTitle="Wyświetlanie i zarządzanie zadaniami StorSimple | Microsoft Azure"
   description="W tym artykule opisano stronę Menedżera StorSimple usługi zadań i jak go używać do śledzenia ostatnio używane, bieżące i zaplanowanych zadań kopii zapasowej."
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
   ms.workload="TBD"
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-view-and-manage-storsimple-jobs"></a>Wyświetlanie i zarządzanie zadaniami StorSimple za pomocą usługi Menedżera StorSimple

[AZURE.INCLUDE [storsimple-version-selector-manage-jobs](../../includes/storsimple-version-selector-manage-jobs.md)]

## <a name="overview"></a>Omówienie

Strona **zadania** umożliwia jednej centralnej do wyświetlania i zarządzania zadaniami, które zostały uruchomione na urządzeniach połączony z usługą Menedżera StorSimple. Można wyświetlać zadania według harmonogramu, uruchomionego ukończone i nie powiodło się dla wielu urządzeń. Wyniki są przedstawione w formie tabeli. 

![Strona zadania](./media/storsimple-manage-jobs/HCS_JobsPage.png)

Można szybko znaleźć zadania, które chcesz się przez filtrowanie do pól, takich jak:

- **Stan** — zadania może być uruchomiony, według harmonogramu, nie powiodło się, ukończony, anulowania lub anulowane.

- **Typ** — zadania mogą być tworzone w wyniku według harmonogramu lub (**Sporządzanie kopii zapasowej**) kopii zapasowej na żądanie, klonowanie, przywracanie urządzenia lub operacji aktualizacji.

- **Urządzenia** — zadania są inicjowane na urządzeniu z niektórych połączony z usługą.

- **Od i do** — zadania mogą być filtrowane w oparciu o zakres dat i godzin.

Filtrowanych zadań są następnie oznaczane znakami tabulacji na podstawie następujące atrybuty:

- **Typ** — kopii zapasowej, klonowanie, przywracanie, pracy awaryjnej lub Aktualizuj.

- **Stan** — uruchomiony, według harmonogramu, nie powiodło się, ukończony, anulowania lub anulowane.

- **Jednostka** — zadania można skojarzyć z woluminu, zasady tworzenia kopii zapasowych lub urządzenie. Zadanie klonowanie jest skojarzony z woluminu, zadanie kopii zapasowej jest skojarzone z zasady tworzenia kopii zapasowych. Zadanie urządzenia zostanie utworzony w wyniku operacji przywracania lub odzyskiwanie (DR).

- **Urządzenia** — nazwę urządzenia, na której zadanie zostało uruchomione.

- **Pracę na** — czas rozpoczęcia zadania.

- **Informacje o postępie** — uzupełniania wartości procentowej uruchomione zadanie. Ukończone zadania to zawsze wynosić 100%.

Listy zadań są odświeżane co 30 sekund.

Na tej stronie można wykonywać następujące czynności związanych z pracą:

- Wyświetlanie szczegółów zadania

- Anulowanie zadania

## <a name="view-job-details"></a>Wyświetlanie szczegółów zadania

Wykonaj poniższe czynności, aby wyświetlić szczegóły każde zadanie.

#### <a name="to-view-job-details"></a>Aby wyświetlić szczegóły zadania

1. Na stronie **zadania** wyświetlane zadania, które chcesz się uruchamiania kwerendy z odpowiednie filtry. Można wyszukiwać ukończone, uruchomiona lub anulować zadania.

2. Wybierz zadanie.

3. U dołu strony kliknij przycisk **Szczegóły**.

4. W oknie dialogowym **Szczegóły zadania kopii zapasowej** możesz wyświetlić stan, szczegóły, statystyki czasu i statystyki danych.

## <a name="cancel-a-job"></a>Anulowanie zadania

Wykonaj poniższe czynności, aby anulować uruchomione zadanie.

### <a name="to-cancel-a-job"></a>Aby anulować zadanie

1. Na stronie **zadania** wyświetlane uruchomionego zadania, które chcesz anulować, uruchamiając kwerendy z odpowiednie filtry.

1. Zaznacz zadanie.

1. U dołu strony kliknij przycisk **Anuluj**.

1. Po wyświetleniu monitu o potwierdzenie, kliknij przycisk **Tak**.

To zadanie jest teraz anulowane.

## <a name="next-steps"></a>Następne kroki

- Dowiedz się, jak [zarządzać StorSimple zasad kopii zapasowej](storsimple-manage-backup-policies.md).

- Dowiedz się, jak [używać usługi Menedżera StorSimple administrowania urządzenia StorSimple](storsimple-manager-service-administration.md).
