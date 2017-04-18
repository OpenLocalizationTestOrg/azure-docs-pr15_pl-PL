<properties 
    pageTitle="Tworzenie aplikacji logicznych w programie Visual Studio | Microsoft Azure" 
    description="Tworzenie projektu w programie Visual Studio, tworzenie i wdrażanie aplikacji logicznych." 
    authors="jeffhollan" 
    manager="erikre" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jehollan"/> 
    
# <a name="build-and-deploy-logic-apps-in-visual-studio"></a>Tworzenie i wdrażanie aplikacji logicznych w programie Visual Studio

Mimo że [Azure Portal](https://portal.azure.com/) umożliwia doskonały sposób na projektowanie i zarządzać swoimi aplikacjami logiczny, może także chcesz projektowanie i wdrażanie aplikacji logika z programu Visual Studio zamiast tego.  Logika aplikacje pochodzi z zaawansowanych narzędzi programu Visual Studio, który umożliwia tworzenie aplikacji logika za pomocą projektanta, skonfigurować szablony wdrożenia i automatyzacji i wdrażanie do każdego miejsca.  

## <a name="installation-steps"></a>Kroki instalacji

Poniżej znajdują się z instrukcjami, aby zainstalować i skonfigurować narzędzia Visual Studio dla aplikacji logicznych.

### <a name="prerequisites"></a>Wymagania wstępne

- [Visual Studio 2015 r.](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
- [Najnowsze SDK Azure](https://azure.microsoft.com/downloads/) (2.9.1 lub nowszego)
- [Azure programu PowerShell] (https://github.com/Azure/azure-powershell#installation)
- Dostęp do sieci web podczas korzystania z osadzonego projektanta

### <a name="install-visual-studio-tools-for-logic-apps"></a>Instalowanie narzędzia Visual Studio logiki aplikacji dla

Raz masz zainstalowane, wymagania wstępne 

1. Otwórz program Visual Studio 2015 do menu **Narzędzia** i wybierz pozycję **rozszerzenia i aktualizacje**
1. Wybierz kategorię **Online** do Wyszukaj w trybie online
1. Wyszukiwanie **Logiczny aplikacje** wyświetlić **Azure logiki aplikacji narzędzia programu Visual Studio**
1. Kliknij przycisk **Pobierz** , aby pobrać i zainstalować rozszerzenia
1. Uruchom ponownie program Visual Studio po zakończeniu instalacji

> [AZURE.NOTE] Możesz też pobrać rozszerzenia bezpośrednio z [tego łącza](https://visualstudiogallery.msdn.microsoft.com/e25ad307-46cf-412e-8ba5-5b555d53d2d9)

Po zainstalowaniu, będzie można użyć projektu grupa zasobów Azure przy użyciu projektanta aplikacji logika.

## <a name="create-a-project"></a>Tworzenie projektu

1. Przejdź do menu **plik** i wybierz pozycję **Nowy** >  **projektu** (lub mogą przejdź do pozycji **Dodaj** , a następnie wybierz **Nowy projekt** , aby dodać go do istniejącego rozwiązania):  ![menu Plik](./media/app-service-logic-deploy-from-vs/filemenu.png)

1. W oknie dialogowym Znajdowanie **chmurze**, a następnie wybierz **Grupę zasobów Azure**. Wpisz **nazwę** , a następnie kliknij **przycisk OK**.
    ![Dodawanie nowego projektu](./media/app-service-logic-deploy-from-vs/addnewproject.png)

1. Wybierz szablon **logiczny aplikacji** . Spowoduje to utworzenie szablonu wdrażania aplikacji pusty logiki zacząć.
    ![Wybierz szablon Azure](./media/app-service-logic-deploy-from-vs/selectazuretemplate.png)

1. Po wybraniu **szablonu**, naciśnij przycisk **OK**.

    Teraz projektu aplikacji logika jest dodawana do rozwiązania. Powinien zostać wyświetlony plik wdrożenia w Eksploratorze rozwiązań:  

    ![Wdrożenie](./media/app-service-logic-deploy-from-vs/deployment.png)

## <a name="using-the-logic-app-designer"></a>Korzystanie z projektanta aplikacji warunków logicznych

Po umieszczeniu projekt Azure grupa zasobów, który zawiera aplikację logiczny, możesz otworzyć projektanta w ramach programu Visual Studio ułatwiający tworzenie przepływu pracy.  Projektant wymaga połączenia internetowego, aby kwerenda łączniki w przypadku dostępne właściwości i danych (na przykład, jeśli program Dynamics CRM Online connector projektanta zwrócą wystąpienia CRM do wyświetlania listy niestandardowe dostępne i domyślne właściwości).

1. Kliknij prawym przyciskiem myszy `<template>.json` plik i wybierz polecenie **Otwórz z projektanta aplikacji logicznych** (lub `Ctrl+L`)
1. Wybierz subskrypcję, grupa zasobów i lokalizację szablonu rozmieszczenia
    - Należy pamiętać, że projektowanie aplikacji logika utworzy zasoby **Interfejsu API połączeń** dla właściwości kwerendy podczas projektowania.  Zaznaczona grupa zasobów będzie grupa zasobów, użyte do utworzenia tych połączeń w czasie projektowania.  Można wyświetlić lub zmodyfikować wszystkie połączenia interfejsu API, przechodząc do Azure Portal i przeglądania **Interfejsu API**połączeń.
    ![Selektor subskrypcji](./media/app-service-logic-deploy-from-vs/designer_picker.png)
1. Projektant powinny dostarczać na podstawie definicji w `<template>.json` pliku.
1. Teraz możesz tworzyć i projektowanie aplikacji logiczny, a zmiany zostaną zaktualizowane w szablonie wdrożenia.
    ![Projektant w programie Visual Studio](./media/app-service-logic-deploy-from-vs/designer_in_vs.png)

Pojawi się także `Microsoft.Web/connections` zasobów dodawanych do pliku zasobów dla każdego połączenia wymagane przez aplikację logiki do funkcji.  Te właściwości połączenia można ustawić podczas wdrażania i zarządzane po wdrożeniu w **Połączenia interfejsu API** w Azure Portal.

### <a name="switching-to-the-json-code-view"></a>Przełączanie do widoku Kod JSON

Możesz wybrać kartę **Widoku kodu** w dolnej części projektanta, aby przełączyć reprezentacją JSON aplikacji logicznych.  Aby wrócić do wszystkich zasobów JSON, kliknij prawym przyciskiem myszy `<template>.json` plik i wybierz polecenie **Otwórz**.

### <a name="saving-the-logic-app"></a>Zapisywanie aplikacji warunków logicznych

Możesz zapisać aplikację logiczny u w dowolnym momencie przy użyciu przycisku **Zapisz** lub `Ctrl+S`.  Jeśli istnieją jakiekolwiek błędy przy użyciu aplikacji logicznych w chwili zapisywania, ich będzie wyświetlana w oknie **wyjściowe** programu Visual Studio.

## <a name="deploying-your-logic-app"></a>Wdrażanie aplikacji warunków logicznych

Ponadto po skonfigurowaniu aplikacji należy wdrożyć bezpośrednio z programu Visual Studio w zaledwie kilku kroków. 

1. Kliknij prawym przyciskiem myszy nad projektem w Eksploratorze rozwiązań i przejdź do pozycji **Rozmieszczanie** > **Nowego wdrożenia...** 
     ![Nowe wdrażania](./media/app-service-logic-deploy-from-vs/newdeployment.png)

2. Zostanie wyświetlony monit, aby zalogować się do usługi Azure subskrypcji. 

3. Teraz należy wybrać szczegóły grupa zasobów, którą chcesz wdrożyć aplikację logiki do. 
    ![Wdrażanie do grupy zasobów](./media/app-service-logic-deploy-from-vs/deploytoresourcegroup.png)

     > [AZURE.NOTE]    Upewnij się zaznaczyć prawo pliki szablonu i parametrów dla grupy zasobów (na przykład w przypadku rozmieszczania środowisku produkcyjnym należy wybrać pliku parametrów produkcji). 
4. Wybierz przycisk rozmieszczania
 
    
6. Stan wdrażania pojawia się w oknie **dane wyjściowe** (może być konieczne wybierz **Azure inicjowania obsługi administracyjnej**. 
    ![Wynik](./media/app-service-logic-deploy-from-vs/output.png)

W przyszłości możesz poprawić aplikacji logicznych w formancie źródła i za pomocą programu Visual Studio do wdrażania nowsze wersje. 

> [AZURE.NOTE] Po zmodyfikowaniu definicji Azure Portal bezpośrednio, przy następnym wdrażanie z programu Visual Studio te zmiany zostaną zastąpione.

## <a name="next-steps"></a>Następne kroki

- Aby rozpocząć pracę z aplikacjami logiczny, wykonaj [Tworzenie aplikacji logiki](app-service-logic-create-a-logic-app.md) samouczka.  
- [Przykłady typowych widoku i scenariusze](app-service-logic-examples-and-scenarios.md)
- [Można zautomatyzować procesów biznesowych za pomocą aplikacji warunków logicznych](http://channel9.msdn.com/Events/Build/2016/T694) 
- [Dowiedz się, jak integrację systemów z aplikacjami warunków logicznych](http://channel9.msdn.com/Events/Build/2016/P462)