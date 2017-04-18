<properties
    pageTitle="Wdrażanie szablonów z programu Visual Studio w stos Azure | Microsoft Azure"
    description="Dowiedz się, jak wdrażanie szablonów z programu Visual Studio w stos Azure."
    services="azure-stack"
    documentationCenter=""
    authors="HeathL17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="helaw"/>

# <a name="deploy-templates-in-azure-stack-using-visual-studio"></a>Wdrażanie szablonów w stos Azure za pomocą programu Visual Studio

Wdrażanie szablonów Azure Menedżera zasobów, aby Zapewnić stos Azure za pomocą programu Visual Studio.

Menedżer zasobów szablony wdrażania i obsługi administracyjnej wszystkich zasobów dla aplikacji w jednym, skoordynowanego operacji.

1.  Otwieranie programu Visual Studio aktualizacja 2015 1.

2.  Kliknij kartę **plik**, kliknij pozycję **Nowy**, a w oknie dialogowym **Nowy projekt** kliknij pozycję **Grupa zasobów Azure**.

3.  Wprowadź **nazwę** dla nowego projektu, a następnie kliknij **przycisk OK**.

4.  W oknie dialogowym **Wybierz szablon Azure** kliknij **maszyn wirtualnych systemu Windows**, a następnie kliknij **przycisk OK**.

  W nowym projekcie zostanie wyświetlona lista szablonów dostępnych po rozwinięciu węzła **Szablony** w okienku **Eksplorator rozwiązań** .

5.  W okienku **Eksplorator rozwiązań** kliknij prawym przyciskiem myszy nazwę projektu, kliknij pozycję **Rozmieść**, a następnie kliknij pozycję **Nowy wdrożenie**.

6.  W oknie dialogowym **Rozmieszczanie do grupy zasobów** w **subskrypcji** listę rozwijaną wybierz subskrypcji usługi Microsoft Azure stosu.

7.  Na liście **Grupa zasobów** wybierz istniejącej grupy zasobów lub Utwórz nową.

8.  Na liście **lokalizacji grupa zasobów** wybierz lokalizację, a następnie kliknij **Rozmieszczanie**.

9.  W oknie dialogowym **Edytuj parametry** wprowadź wartości parametrów (które zależy od szablonu), a następnie kliknij przycisk **Zapisz**.

## <a name="next-steps"></a>Następne kroki

[Wdrażanie szablonów z poziomu wiersza polecenia](azure-stack-deploy-template-command-line.md)
