<properties 
   pageTitle="Rozpoczynanie pracy z magazynu Lake danych | Azure" 
   description="Utworzenie konta magazynu Lake danych i wykonywania podstawowych operacji w magazynie Lake danych za pomocą portalu" 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/13/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-the-azure-portal"></a>Rozpoczynanie pracy z magazynu Lake danych Azure za pomocą portalu Azure

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [Programu PowerShell](data-lake-store-get-started-powershell.md)
- [ZESTAW SDK PROGRAMU .NET](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [INTERFEJSU API USŁUGI REST](data-lake-store-get-started-rest-api.md)
- [Polecenie Azure](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Dowiedz się, jak utworzyć konto Azure magazynu Lake danych i wykonywania podstawowych operacji, takich jak tworzyć foldery, przekazywanie i pobieranie plików danych za pomocą Azure Portal, usuwanie swojego konta, itp. Aby uzyskać więcej informacji na temat magazynu Lake danych zobacz [Omówienie z Azure Lake magazynu danych](data-lake-store-overview.md).

## <a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem tego samouczka, musisz mieć następujące czynności:

- **Azure subskrypcji**. Zobacz [Azure pobrać bezpłatną wersję próbną](https://azure.microsoft.com/pricing/free-trial/).

## <a name="do-you-learn-fast-with-videos"></a>Czy informacje szybko z klipów wideo?

Obejrzyj następujące klipy wideo, aby rozpocząć pracę z magazynu Lake danych.

* [Tworzenie konta magazynu Lake danych](https://mix.office.com/watch/1k1cycy4l4gen)
* [Zarządzanie danymi w magazynie Lake danych za pomocą Eksploratora danych](https://mix.office.com/watch/icletrxrh6pc)

## <a name="create-an-azure-data-lake-store-account"></a>Utwórz konto Azure magazynu Lake danych

1. Logowanie się do nowego [Azure Portal](https://portal.azure.com).

2. Kliknij przycisk **Nowy**, kliknij pozycję **dane + miejsca do magazynowania**, a następnie kliknij **Magazynu Lake danych Azure**. Przeczytaj informacje w karta **Magazynu Lake Azure danych** , a następnie kliknij przycisk **Utwórz** w lewym dolnym rogu karta.

3. W karta **Nowy magazyn Lake dane** zawierają wartości, jak pokazano poniżej przechwycony ekran:

    ![Utwórz nowe konto Azure magazynu Lake danych] (./media/data-lake-store-get-started-portal/ADL.Create.New.Account.png "Utwórz nowe konto Azure Lake danych")

    - **Subskrypcji**. Wybierz subskrypcję, w którym chcesz utworzyć nowe konto magazynu Lake danych.
    - **Grupa zasobów**. Wybierz istniejącą grupę zasobów, lub kliknij pozycję **Utwórz grupę zasobów** , aby utworzyć. Grupa zasobów to kontener zasoby pokrewne dla aplikacji. Aby uzyskać więcej informacji zobacz [Grup zasobów w Azure](azure-resource-manager/resource-group-overview.md#resource-groups).
    - **Lokalizacja**: Wybierz lokalizację, w której chcesz utworzyć konto magazynu Lake danych.

4. Wybierz pozycję **Przypnij do Startboard** , jeśli chcesz konta magazynu Lake danych mają być dostępne z Startboard.

5. Kliknij przycisk **Utwórz**. Jeśli chcesz przypiąć klienta do startboard spowoduje powrót do startboard i można wyświetlić postęp konfiguracji konta magazynu Lake danych. Gdy konta magazynu Lake danych jest przygotowana, karta konta wyświetlane.

6. Rozwijanie **podstawowe informacje dotyczące** listy rozwijanej, aby sprawdzić, czy informacje o Twoim koncie magazynu Lake danych, takich jak grupie zasobu jest należeć do lokalizacji, itp. Kliknij ikonę **Szybki Start** , aby zobaczyć łącza do dodatkowych zasobów związanych z magazynu Lake danych.

    ![Konto Sklepem Lake danych Azure] (./media/data-lake-store-get-started-portal/ADL.Account.QuickStart.png "Konta i Lake danych Azure")

## <a name="createfolder"></a>Tworzenie folderów konta magazynu Lake danych Azure

Można tworzyć foldery w obszarze konta magazynu Lake danych do przechowywania danych i zarządzanie nimi.

1. Otwórz konto magazynu Lake danych, która została właśnie utworzona. W okienku po lewej stronie kliknij przycisk **Przeglądaj**, kliknij pozycję **Magazyn Lake danych**, a następnie z karta magazynu Lake danych kliknij nazwę konta, w którym chcesz utworzyć foldery. Jeśli przypięte klienta do startboard, kliknij pozycję kafelka tego konta.

2. W swojej karta konta magazynu Lake danych kliknij pozycję **Eksplorator danych**.

    ![Tworzenie folderów w magazynie Lake danych konta] (./media/data-lake-store-get-started-portal/ADL.Create.Folder.png "Tworzenie folderów w magazynie Lake danych konta")

3. W swojej karta konta magazynu Lake danych kliknij pozycję **Nowy Folder**, wprowadź nazwę nowego folderu, a następnie kliknij **przycisk OK**.
    
    ![Tworzenie folderów w magazynie Lake danych konta] (./media/data-lake-store-get-started-portal/ADL.Folder.Name.png "Tworzenie folderów w magazynie Lake danych konta")
    
    Nowo utworzonego folderu zostaną wyświetlone w karta **Eksplorator danych** . Możesz tworzyć folderów zagnieżdżonych prowadzący dowolnego poziomu.

    ![Tworzenie folderów na koncie Lake danych] (./media/data-lake-store-get-started-portal/ADL.New.Directory.png "Tworzenie folderów na koncie Lake danych")


## <a name="uploaddata"></a>Przekazywanie danych do konta magazynu Lake danych Azure

Możesz przekazać danych do konta usługi Azure magazynu Lake danych bezpośrednio na poziomie głównym lub do folderu, który został utworzony w ramach tego konta. W przechwycony ekran poniżej postępuj zgodnie z instrukcjami, aby przekazać plik podfolder z karta **Eksplorator danych** . W tym przechwycony ekran plik zostanie przekazany podfolder pokazano łącza do stron nadrzędnych (oznaczonych w polu czerwony).

Jeśli szukasz kilka przykładowych danych do przekazania zostanie wyświetlony folder **Pogotowie danych** z [Repozytorium cyfra Lake danych Azure](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData).

![Przekazywanie danych] (./media/data-lake-store-get-started-portal/ADL.New.Upload.File.png "Przekazywanie danych")


## <a name="properties"></a>Właściwości i akcji dostępnych na przechowywane dane

Kliknij nowo dodany plik, aby otworzyć karta **Właściwości** . Właściwości skojarzone z pliku i akcje, które można wykonywać na plik są dostępne w tym karta. Możesz również skopiować pełną ścieżkę do pliku na koncie magazynu Lake danych Azure wyróżnione w polu czerwony przechwycony ekran poniżej.

![Właściwości na karcie dane] (./media/data-lake-store-get-started-portal/ADL.File.Properties.png "Właściwości na karcie dane")

* Kliknij przycisk **Podgląd** , aby wyświetlić podgląd pliku, bezpośrednio z poziomu przeglądarki. Możesz określić formatowanie także podglądu. Kliknij pozycję **Podgląd**, kliknij przycisk **Format** w karta **Podgląd pliku** i w karta **Format Podgląd pliku** Określ opcje, takie jak liczba wierszy, aby wyświetlić, kodowanie, ogranicznika za pomocą itd.

  ![Podgląd formatu] (./media/data-lake-store-get-started-portal/ADL.File.Preview.png "Podgląd formatu")

* Kliknij przycisk **Pobierz** , aby pobrać plik na swój komputer.

* Kliknij przycisk **Zmień nazwę pliku** , aby zmienić nazwę pliku.

* Kliknij przycisk **Usuń plik** , aby usunąć ten plik.


## <a name="secure-your-data"></a>Zabezpieczanie danych

Można zabezpieczyć dane przechowywane w konta magazynu Lake Azure danych przy użyciu usługi Azure Active Directory i kontroli dostępu (ACL). Aby uzyskać instrukcje, jak to zrobić zobacz [Zabezpieczanie danych w magazynie Lake danych Azure](data-lake-store-secure-data.md).


## <a name="delete-azure-data-lake-store-account"></a>Magazyn Lake danych Azure Usuń konto

Aby usunąć konto Azure magazynu Lake danych z usługi karta magazynu Lake danych, kliknij przycisk **Usuń**. Aby potwierdzić akcję, zostanie wyświetlony monit o wprowadzenie nazwy konta, które chcesz usunąć. Wprowadź nazwę konta, a następnie kliknij polecenie **Usuń**.

![Usuń dane Lake konta] (./media/data-lake-store-get-started-portal/ADL.Delete.Account.png "Usuń dane Lake konta")


## <a name="next-steps"></a>Następne kroki

- [Zabezpieczanie danych w magazynie Lake danych](data-lake-store-secure-data.md)
- [Używanie analizy Lake Azure danych z magazynu Lake danych](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Usługa Azure HDInsight za pomocą magazynu Lake danych](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Dzienniki diagnostyczne dostępu dla magazynu Lake danych](data-lake-store-diagnostic-logs.md)
