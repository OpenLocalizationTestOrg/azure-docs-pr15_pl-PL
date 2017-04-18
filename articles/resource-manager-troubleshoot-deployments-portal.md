<properties
   pageTitle="Wyświetlanie operacji rozmieszczania z portalem | Microsoft Azure"
   description="Informacje dotyczące używania Azure portal wykrywanie błędów z wdrożenia Menedżera zasobów."
   services="azure-resource-manager,virtual-machines"
   documentationCenter=""
   tags="top-support-issue"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-multiple"
   ms.workload="infrastructure"
   ms.date="06/15/2016"
   ms.author="tomfitz"/>

# <a name="view-deployment-operations-with-azure-portal"></a>Wyświetlanie operacji wdrażania Portal Azure

> [AZURE.SELECTOR]
- [Portal](resource-manager-troubleshoot-deployments-portal.md)
- [Programu PowerShell](resource-manager-troubleshoot-deployments-powershell.md)
- [Polecenie Azure](resource-manager-troubleshoot-deployments-cli.md)
- [INTERFEJSU API USŁUGI REST](resource-manager-troubleshoot-deployments-rest.md)

Możesz wyświetlić operacji rozmieszczania za pośrednictwem portalu Azure. Może być najbardziej interesuje przeglądanie operacji, gdy masz otrzymujesz komunikat o błędzie podczas wdrażania, w tym artykule omówiono wyświetlanie operacji, które nie powiodło się. Portalu interfejs, który umożliwia łatwe znajdowanie błędów i określanie potencjalne rozwiązania.

[AZURE.INCLUDE [resource-manager-troubleshoot-introduction](../includes/resource-manager-troubleshoot-introduction.md)]

## <a name="use-deployment-operations-to-troubleshoot"></a>Rozwiązywanie problemów za pomocą operacji rozmieszczania

Aby wyświetlić operacji rozmieszczania, wykonaj następujące czynności:

1. Dla grupy zasobów biorących udział w ramach wdrożenia Zwróć uwagę, stan ostatniego wdrożenia. Możesz wybrać ten status, aby uzyskać więcej szczegółów.

    ![Stan wdrażania](./media/resource-manager-troubleshoot-deployments-portal/deployment-status.png)

2. Zostanie wyświetlona Najnowsza historia wdrożenia. Wybierz pozycję wdrożenia, który nie powiodło się.

    ![Stan wdrażania](./media/resource-manager-troubleshoot-deployments-portal/select-deployment.png)

3. Wybierz pozycję **nie powiodło się. Kliknij tutaj, aby uzyskać szczegółowe informacje** aby zobaczyć opis niepowodzenia wdrożenia. Na poniższej ilustracji rekord DNS nie jest unikatowy.  

    ![Wyświetlanie niepowodzenie wdrożenia](./media/resource-manager-troubleshoot-deployments-portal/view-error.png)

    Ten komunikat o błędzie powinny być wystarczające dla Ciebie rozpocząć rozwiązywanie problemów. Jeśli chcesz uzyskać więcej informacji o zadania, które zostały wykonane, możesz wyświetlić operacje, jak pokazano w następującej procedurze.

4. Można wyświetlić wszystkie operacje wdrożenia w karta **wdrożenia** . Wybierz dowolną operację, aby wyświetlić więcej szczegółów.

    ![Wyświetlanie operacji](./media/resource-manager-troubleshoot-deployments-portal/view-operations.png)

    W tym przypadku pojawi się, że konto miejsca do magazynowania, wirtualną sieć i zestaw dostępność zostały pomyślnie utworzone. Publiczny adres IP nie powiodło się, a inne zasoby nie została użyta.

5. Zdarzenia do wdrożenia można przeglądać, wybierając **zdarzeń**.

    ![Przeglądanie zdarzeń](./media/resource-manager-troubleshoot-deployments-portal/view-events.png)

6. Zobacz wszystkie zdarzenia do wdrożenia i zaznacz dowolny, aby uzyskać więcej informacji.

    ![Zobacz zdarzenia](./media/resource-manager-troubleshoot-deployments-portal/see-all-events.png)

## <a name="use-audit-logs-to-troubleshoot"></a>Używanie dzienników inspekcji rozwiązywać problemy

[AZURE.INCLUDE [resource-manager-audit-limitations](../includes/resource-manager-audit-limitations.md)]

Aby wyświetlić błędy wdrożenia, wykonaj następujące czynności:

1. Wyświetlanie dzienników inspekcji dla grupy zasobów, wybierając pozycję **Dzienniki inspekcji**.

    ![Wybierz pozycję Dzienniki inspekcji](./media/resource-manager-troubleshoot-deployments-portal/select-audit-logs.png)

2. W karta **Dzienników inspekcji** podsumowanie ostatniej operacji dla wszystkich grup zasobów będą widoczne w ramach subskrypcji. Zawiera reprezentację czas i stan operacji, a także listy operacji.

    ![Pokaż akcje](./media/resource-manager-troubleshoot-deployments-portal/audit-summary.png)

3. Można filtrować widok dzienników inspekcji w celu wyodrębnienia określonych warunków. Wybierz opcję **filtru** w górnej części karta **dzienników inspekcji** .

    ![Dzienniki filtru](./media/resource-manager-troubleshoot-deployments-portal/filter-logs.png)

4. Karta **Filtr** zaznacz warunki, aby ograniczyć widoku dzienników inspekcji do tylko te czynności, które mają być wyświetlane. Na przykład można filtrować działania w celu wyświetlenia tylko błędy dla grupy zasobów.

    ![Ustawianie opcji filtru](./media/resource-manager-troubleshoot-deployments-portal/set-filter.png)

5. Operacje można dodatkowo filtrować przez ustawienie zakresu czasu. Poniższa ilustracja filtrów widoku do określonego przedziału czasu 20 minut.

    ![Ustawianie czasu](./media/resource-manager-troubleshoot-deployments-portal/select-time.png)

6. Możesz wybrać żadnej czynności na liście. Wybierz operację, która zawiera błąd, który chcesz wyszukać.

    ![Wybierz operację](./media/resource-manager-troubleshoot-deployments-portal/select-operation.png)
  
7. Zostanie wyświetlona wszystkie zdarzenia dla tej operacji. Zwróć uwagę, **Identyfikatory korelacji** podsumowanie. Ten identyfikator jest używany do śledzenia zdarzeń pokrewnych. Może być przydatne podczas pracy z pomocą techniczną w celu rozwiązania problemu. Można wybrać dowolny zdarzenia, aby wyświetlić szczegółowe informacje dotyczące zdarzenia.

    ![Wybierz zdarzenie](./media/resource-manager-troubleshoot-deployments-portal/select-event.png)

8. Zostaną wyświetlone szczegółowe informacje dotyczące zdarzenia. Aby uzyskać informacje o błędzie, w szczególności zwrócić uwagę na **Właściwości** .

    ![Wyświetlanie szczegółowych informacji dziennika inspekcji](./media/resource-manager-troubleshoot-deployments-portal/audit-details.png)

Filtr, który został zastosowany do dziennika inspekcji jest zachowywana przy następnym wyświetlania, więc może być konieczna zmiana te wartości, aby rozszerzyć widoku działań.

## <a name="next-steps"></a>Następne kroki

- Aby uzyskać pomoc dotyczącą usuwania błędów danego wdrożenia zobacz [Usuwanie typowych błędów podczas wdrażania zasobów Azure przy użyciu Menedżera zasobów Azure](resource-manager-common-deployment-errors.md).
- Aby uzyskać informacje o za pomocą dzienników inspekcji monitorowanie innych działań, zobacz [Inspekcja operacji przy użyciu Menedżera zasobów](resource-group-audit.md).
- Aby sprawdzić poprawność wdrożenia przed jego wykonaniem, zobacz temat [Deploy grupa zasobów za pomocą szablonu Azure Menedżera zasobów](resource-group-template-deploy.md).
