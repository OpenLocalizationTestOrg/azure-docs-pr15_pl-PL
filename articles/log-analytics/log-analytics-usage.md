<properties
    pageTitle="Analizowanie danych użycia w dzienniku analizy | Microsoft Azure"
    description="Na stronie zastosowania w dzienniku analizy służy do wyświetlania ilości danych jest wysyłana do usługę."
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
    ms.topic="get-started-article"
    ms.date="08/11/2016"
    ms.author="banders"/>

# <a name="analyze-data-usage-in-log-analytics"></a>Analizowanie danych użycia w analizy dziennika

Dziennik analizy w pakiecie zarządzania operacje usługi (OMS) zbiera dane i przesyła go okresowo usługę.  Na stronie **zastosowania** służy do wyświetlania, ile danych jest wysyłana do usługę. Na stronie **zastosowania** również pokazano ilości danych jest codziennie wysyłane przez rozwiązań i jak często wysyłają serwery danych.

>[AZURE.NOTE] Jeśli masz bezpłatne konto utworzony przy użyciu [usługi OMS witryny sieci Web](http://www.microsoft.com/oms), użytkownik musi tylko wysyłanie 500 MB danych do usługę codziennie. Przekroczenie limitu dziennego analizy Zatrzymaj i wznowienie na początku następnego dnia. Musisz również ponowne wysyłanie dane, które nie zaakceptowane lub przetwarzanych przez usługi OMS.

Można wyświetlić użycie przy użyciu kafelka **zastosowania** na pulpit nawigacyjny **przeglądu** w usługi OMS.

![Użycie kafelków](./media/log-analytics-usage/usage-tile.png)

Jeśli Przekroczono limit dzienny zastosowania lub jest zbliżony do limitu, opcjonalnie można usunąć rozwiązanie w celu zmniejszenia ilości danych, które wysyłasz do usługę. Aby uzyskać więcej informacji na temat usuwania rozwiązań zobacz [Dodawanie analizy dziennika rozwiązań z galerii rozwiązań](log-analytics-add-solutions.md).

![Użycie pulpitu nawigacyjnego](./media/log-analytics-usage/usage-dashboard.png)

Na stronie **zastosowania** zostaną wyświetlone następujące informacje:

- Średnia użycie dzienne
- Korzystanie z danych dla każdego z rozwiązań w ciągu ostatnich 30 dni
- Ile danych serwerów w środowisku są wysyłane na usługę w ciągu ostatnich 30 dni
- Plan danych ceny warstwa oraz szacowany koszt
- Informacje o Twojej Umowa dotycząca poziomu usług (SLA), w tym, jak długo trwa usługi OMS przetwarzania danych

## <a name="to-work-with-usage-data"></a>Aby pracować z danych dotyczących użycia

1. Na stronie **Przegląd** kliknij **zastosowania** .
2. Na stronie **Użycie** wyświetlania kategorii zastosowania Pokaż obszary, że jest się dane o.
3. Jeśli masz rozwiązanie, które jest przez inne zbyt dużo dzienny przydziału przekazywania, należy rozważyć, usunięcie tego rozwiązania.

## <a name="to-view-your-estimated-cost-and-billing-information"></a>Aby wyświetlić usługi szacowany koszt i informacje rozliczeniowe
1. Na stronie **Przegląd** kliknij **zastosowania** .
2. Na stronie **Użycie** w obszarze **Użycie**kliknij Pagon (**>**) obok **Szacowany koszt**.
3. W rozwiniętej szczegóły **planu danych** widać usługi szacowany koszt, co miesiąc.  
    ![Plan danych](./media/log-analytics-usage/usage-data-plan.png)
4. Jeśli chcesz wyświetlić rozliczeniowym, kliknij pozycję **odczytywanie rachunku** , aby wyświetlić informacje o subskrypcji.
    - Na stronie subskrypcje kliknij subskrypcję, aby wyświetlić szczegółowe informacje oraz listę pozycji zastosowania.  
        ![subskrypcji](./media/log-analytics-usage/usage-sub01.png)
    - Na stronie Podsumowanie dla subskrypcji można wykonywać różne zadania, aby wyświetlić więcej szczegółów dotyczących subskrypcji i zarządzanie nimi.  
        ![Szczegóły subskrypcji](./media/log-analytics-usage/usage-sub02.png)

## <a name="to-view-data-batches-for-your-sla"></a>Aby wyświetlić dane partii dla swojego Umowa dotycząca poziomu usług
1. Na stronie **Przegląd** kliknij **zastosowania** .
2. W obszarze **Umowa dotycząca poziomu usług**kliknij pozycję **Pobierz SLA szczegóły**.
3. Umożliwia przeglądanie jest pobierany plik XLSX programu Excel.  
    ![Umowa dotycząca poziomu usług szczegóły](./media/log-analytics-usage/usage-sla-details.png)

## <a name="next-steps"></a>Następne kroki

- Zobacz [wyszukiwania dziennika analizy dziennika](log-analytics-log-searches.md) , aby wyświetlić szczegółowe informacje zgromadzone przez rozwiązań.
