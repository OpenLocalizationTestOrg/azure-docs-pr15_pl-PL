<properties 
   pageTitle="Dowiedz się, analizy Lake danych i U-SQL przy użyciu interakcyjnego Portal Azure samouczki | Azure" 
   description="Szybki start nauki analizy Lake danych i U-SQL. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="05/16/2016"
   ms.author="edmaca"/>


# <a name="use-azure-data-lake-analytics-interactive-tutorials"></a>Interaktywne podręczniki przedstawiają analizy Lake danych Azure za pomocą

Azure Portal zapewnia interakcyjny samouczek można rozpocząć pracę z danymi Lake analizy. W tym artykule pokazano, jak wykonywać samouczek do analizy dzienników witryny sieci Web.


>[AZURE.NOTE]Jeśli chcesz przejść na tym samym przewodnik przy użyciu programu Visual Studio, zobacz [Analiza dzienników witryny sieci Web przy użyciu analizy Lake danych](data-lake-analytics-analyze-weblogs.md).
>Więcej interakcyjne samouczki mają zostać dodane do portalu.


Inne samouczki Zobacz:

- [Wprowadzenie do analiz Lake danych za pomocą Azure Portal](data-lake-analytics-get-started-portal.md)
- [Wprowadzenie do analizy Lake danych przy użyciu programu PowerShell Azure](data-lake-analytics-get-started-powershell.md)
- [Wprowadzenie do analizy Lake danych przy użyciu zestawu SDK .NET](data-lake-analytics-get-started-net-sdk.md)
- [Można opracowywać skrypty U SQL za pomocą narzędzia Lake danych dla programu Visual Studio](data-lake-analytics-data-lake-tools-get-started.md) 

**Wymagania wstępne**

Przed rozpoczęciem tego samouczka, musisz mieć następujące czynności:

- **Konto analizy Lake danych**.  Zobacz [Wprowadzenie do analiz Lake danych Azure za pomocą Azure Portal](data-lake-analytics-get-started-portal.md).

##<a name="create-data-lake-analytics-account"></a>Tworzenie konta analizy Lake danych 

Przed uruchomieniem zadania, musisz mieć konto analizy Lake danych.

Każde konto analizy Lake danych ma zależność konto [Azure magazynu Lake danych](../data-lake-store/data-lake-store-overview.md) .  To konto jest określana jako konto domyślne magazynu Lake danych.  Możesz utworzyć konta magazynu Lake danych wcześniej, lub po utworzeniu konta analizy Lake danych. W tym samouczku spowoduje utworzenie konta magazynu Lake danych za pomocą konta analizy

**Aby utworzyć konto analizy Lake danych**

1. Logowanie do [portalu Azure](https://portal.azure.com/signin/index/?Microsoft_Azure_Kona=true&Microsoft_Azure_DataLake=true&hubsExtension_ItemHideKey=AzureDataLake_BigStorage%2cAzureKona_BigCompute).
2. Kliknij pozycję **Microsoft Azure** w lewym górnym rogu, aby otworzyć StartBoard.
3. Kliknij Kafelek **witryny Marketplace** .  
3. Wpisz **Azure danych Lake analizy** w polu wyszukiwania w karta **wszystko** , a następnie naciśnij klawisz **ENTER**. **Azure danych Lake analizy** jest wyświetlany na liście.
4. Kliknij pozycję **Analizy Lake danych Azure** na liście.
5. Kliknij przycisk **Utwórz** w dolnej części karta.
6. Wpisz lub wybierz następujące czynności:

    ![Karta portal Azure danych Lake analizy](./media/data-lake-analytics-get-started-portal/data-lake-analytics-portal-create-adla.png)

    - **Nazwa**: Nazwa konta analizy.
    - **Magazyn Lake danych**: Konto każdego analizy Lake danych ma zależne konta magazynu Lake danych. Konto analizy Lake danych oraz zależne magazynu Lake danych musi znajdować się w tym samym centrum danych Azure. Postępuj zgodnie z instrukcjami, aby utworzyć nowe konto magazynu Lake danych, lub wybierz istniejący.
    - **Subskrypcja**: Wybierz subskrypcję Azure dla konta analizy.
    - **Grupa zasobów**. Wybierz istniejącą grupą zasobów Azure lub Utwórz nową. Aplikacje zwykle składają się wiele elementów, na przykład aplikacji sieci web, bazy danych, serwer bazy danych, magazynowania i 3 usług innych firm. Menedżer zasobów Azure (ARM) umożliwia pracę z zasobami w aplikacji grupowo, określane jako grupa zasobów Azure. Możesz wdrożyć, aktualizowanie, monitorować lub usunąć wszystkie zasoby aplikacji w jednym, skoordynowanego operacji. Używanie szablonu do wdrożenia i tego szablonu można pracować w różnych środowiskach takich jak testowania, organizowanie i produkcji. Może zawierać wyjaśnienie rozliczenia dla Twojej organizacji, wyświetlając rzutowane koszty dla całej grupy. Aby uzyskać więcej informacji zobacz [Omówienie Menedżera zasobów Azure](azure-resource-manager/resource-group-overview.md). 
    - **Lokalizacja**. Wybierz pozycję Centrum danych Azure konta analizy Lake danych. 
7. Wybierz pozycję **Przypnij do Startboard**. Jest to wymagane do obserwowania tego samouczka.
8. Kliknij przycisk **Utwórz**. Przejście do portalu StartBoard. Fragment jest dodawany do strony głównej z etykietą przedstawiający "Wdrażanie Azure danych Lake analizy". Wystarczy kilka minut, aby utworzyć konto analizy Lake danych. Po utworzeniu konta portalu zostanie wyświetlona na koncie dla nowych kart.

    ![Karta portal Azure danych Lake analizy](./media/data-lake-analytics-get-started-portal/data-lake-analytics-portal-blade.png)

##<a name="run-website-log-analysis-interactive-tutorial"></a>Uruchamianie interaktywny samouczek analizy dziennika witryny sieci Web

**Aby otworzyć witrynę sieci Web Analytics dziennika interaktywny samouczek**

1. Z poziomu portalu kliknij pozycję **Microsoft Azure** z menu po lewej stronie, aby otworzyć StartBoard.
2. Kliknij Kafelek, które jest połączone z Twoim kontem analizy Lake danych.
3. Kliknij przycisk **Eksploruj interaktywne podręczniki przedstawiają** z paska **Essentials** .

    ![Interaktywne podręczniki przedstawiają Lake danych analizy](./media/data-lake-analytics-use-interactive-tutorials/data-lake-analytics-explore-interactive-tutorials.png)

4. Jeśli zobaczysz pomarańczowego informacją o tym ostrzeżenia "próbki nie konfiguracji, kliknij przycisk...", kliknij przycisk **Kopiuj przykładowych danych** do skopiowania przykładowych danych do domyślnego konta magazynu Lake danych. Interaktywny samouczek potrzebuje danych do uruchomienia.
5. Karta **Interaktywne podręczniki przedstawiają** kliknij **Analizy dziennika witryny sieci Web**. Portalu spowoduje otwarcie samouczka w nowej karta portalu.
5. Kliknij pozycję **Wprowadzenie 1** , a następnie postępuj zgodnie z instrukcjami

##<a name="see-also"></a>Zobacz też

- [Omówienie analizy danych Lake bazy wiedzy Microsoft Azure](data-lake-analytics-overview.md)
- [Wprowadzenie do analiz Lake danych za pomocą Azure Portal](data-lake-analytics-get-started-portal.md)
- [Wprowadzenie do analizy Lake danych przy użyciu programu PowerShell Azure](data-lake-analytics-get-started-powershell.md)
- [Można opracowywać skrypty U SQL za pomocą narzędzia Lake danych dla programu Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
- [Analizowanie dzienniki witryny sieci Web przy użyciu analizy Lake danych Azure](data-lake-analytics-analyze-weblogs.md)
