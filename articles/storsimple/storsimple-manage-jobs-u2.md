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

# <a name="use-the-storsimple-manager-service-to-view-and-manage-storsimple-jobs-update-2"></a>Za pomocą usługi Menedżera StorSimple do wyświetlania i zarządzania zadaniami StorSimple (Aktualizuj 2)

[AZURE.INCLUDE [storsimple-version-selector-manage-jobs](../../includes/storsimple-version-selector-manage-jobs.md)]

## <a name="overview"></a>Omówienie

Strona **zadania** zawiera jednej centralnej portal do wyświetlania i zarządzania zadaniami, które zostały uruchomione na urządzeniach połączony z usługą Menedżera StorSimple. Można wyświetlać zadania według harmonogramu, uruchomionego złożonym, anulowane i nie powiodło się dla wielu urządzeń. Wyniki są przedstawione w formie tabeli. 

![Strona zadania](./media/storsimple-manage-jobs-u2/jobs.png)

Można szybko znaleźć zadania, które chcesz się przez filtrowanie do pól, takich jak:

- **Stan** — zadania może być uruchomiony, ukończony, anulowane, nie powiodło się, anulowania lub ukończone z błędami.
- **Od i do** — zadania mogą być filtrowane w oparciu o zakres dat i godzin.
- **Typ** — typ zadania może być kopii zapasowej, ręcznego wykonywania kopii zapasowej, przywracanie, klonowanie, urządzenia awaryjnym przeniesieniu utworzonego przypiętej lokalnie, modyfikowanie głośności, aktualizowanie i pomocy technicznej pakietu lub urządzenia wirtualnego inicjowania obsługi administracyjnej.

- **Urządzenia** — zadania są inicjowane na urządzeniu z niektórych połączony z usługą.
Filtrowanych zadań są następnie oznaczane znakami tabulacji na podstawie następujące atrybuty:

    - **Typ** — kopia zapasowa ręcznego wykonywania kopii zapasowej, przywracanie, klonowanie, urządzenia awaryjnym przeniesieniu utworzonego przypiętej lokalnie, modyfikowanie głośności, aktualizowanie i pomocy technicznej pakietu lub urządzenia wirtualnego inicjowania obsługi administracyjnej.

    - **Stan** — systemem złożonym, anulowane, nie powiodło się, anulowania lub ukończone z błędami.

    - **Jednostka** — zadania można skojarzyć z woluminu, zasady tworzenia kopii zapasowych lub urządzenie. Na przykład zadanie klonowanie jest skojarzony z woluminu, zadanie kopii zapasowej jest skojarzone z zasady tworzenia kopii zapasowych. Zadanie urządzenia zostanie utworzony w wyniku operacji przywracania lub odzyskiwanie (DR).

    - **Urządzenia** — nazwę urządzenia, na której zadanie zostało uruchomione.

    - **Wprowadzenie na** — czas rozpoczęcia zadania.

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
 
    ![Strona Szczegóły zadania](./media/storsimple-manage-jobs-u2/JobDetails.png)

## <a name="cancel-a-job"></a>Anulowanie zadania

Wykonaj poniższe czynności, aby anulować uruchomione zadanie.

>[AZURE.NOTE] Nie można anulować niektórych zadań, takich jak modyfikowanie głośność, aby zmienić typ głośność lub rozwijanie głośność.

### <a name="to-cancel-a-job"></a>Aby anulować zadanie

1. Na stronie **zadania** wyświetlane uruchomionego zadania, które chcesz anulować, uruchamiając kwerendy z odpowiednie filtry.

1. Zaznacz zadanie.

1. U dołu strony kliknij przycisk **Anuluj**.

1. Po wyświetleniu monitu o potwierdzenie, kliknij przycisk **Tak**.

To zadanie jest teraz anulowane.

## <a name="next-steps"></a>Następne kroki

- Dowiedz się, jak [zarządzać StorSimple zasad kopii zapasowej](storsimple-manage-backup-policies.md).

- Dowiedz się, jak [używać usługi Menedżera StorSimple administrowania urządzenia StorSimple](storsimple-manager-service-administration.md).
