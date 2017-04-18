<properties
   pageTitle="Rozpoczynanie pracy z magazynu Lake danych | Azure"
   description="Utworzenie konta magazynu Lake danych i wykonywania podstawowych operacji za pomocą programu PowerShell Azure"
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
   ms.date="10/04/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-azure-powershell"></a>Rozpoczynanie pracy z magazynu Lake danych Azure przy użyciu programu PowerShell Azure

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [Programu PowerShell](data-lake-store-get-started-powershell.md)
- [ZESTAW SDK PROGRAMU .NET](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [INTERFEJSU API USŁUGI REST](data-lake-store-get-started-rest-api.md)
- [Polecenie Azure](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Dowiedz się, jak utworzyć konto Azure magazynu Lake danych i wykonywania podstawowych operacji, takich jak tworzyć foldery, przekazywanie i pobieranie plików danych za pomocą programu PowerShell Azure, usuwanie swojego konta, itp. Aby uzyskać więcej informacji na temat magazynu Lake danych zobacz [Omówienie pakietu Lake magazynu danych](data-lake-store-overview.md).

## <a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem tego samouczka, musisz mieć następujące czynności:

* **Azure subskrypcji**. Zobacz [Azure pobrać bezpłatną wersję próbną](https://azure.microsoft.com/pricing/free-trial/).

* **Azure PowerShell wersji 1.0 lub nowszej**. Dowiedz się, [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md).

## <a name="authentication"></a>Uwierzytelnianie

Ten artykuł jest używana prostsze uwierzytelniania z magazynu Lake danych, gdy zostanie wyświetlony monit o podanie poświadczeń konta Azure. Poziom dostępu do konta i system plików podlega następnie poziom dostępu zalogowany użytkownik magazynu Lake danych. Istnieją inne metody także do uwierzytelnienia z magazynu Lake danych, które są **uwierzytelniania użytkowników końcowych** lub **do usługi**. Instrukcje i uzyskać więcej informacji na temat uwierzytelniania Zobacz [uwierzytelniania z magazynu Lake danych za pomocą usługi Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

## <a name="create-an-azure-data-lake-store-account"></a>Utwórz konto Azure magazynu Lake danych

1. Z pulpitu Otwórz nowe okno programu Windows PowerShell, a następnie wprowadź następujące wstawek, zaloguj się do konta Azure, ustaw subskrypcji i rejestrować dostawcy magazynu Lake danych. Gdy zostanie wyświetlony monit, aby się zalogować, upewnij się, że możesz zalogować się jako jeden z admininistrators właścicielem subskrypcji:

        # Log in to your Azure account
        Login-AzureRmAccount

        # List all the subscriptions associated to your account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Azure Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"


2. Konto Azure magazynu Lake danych jest skojarzony z grupą zasobów Azure. Rozpoczyna się od utworzenia Azure grupa zasobów.

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    ![Tworzenie grupy zasobów Azure] (./media/data-lake-store-get-started-powershell/ADL.PS.CreateResourceGroup.png "Tworzenie grupy zasobów Azure")

2. Utwórz konto Azure magazynu Lake danych. Nazwę, którą można określić musi zawierać tylko małe litery i cyfry.

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    ![Tworzenie konta magazynu Lake danych Azure] (./media/data-lake-store-get-started-powershell/ADL.PS.CreateADLAcc.png "Tworzenie konta magazynu Lake danych Azure")

3. Sprawdź, czy konto jest pomyślnie utworzone.

        Test-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

    Wynik to powinien być **spełnione**.

## <a name="create-directory-structures-in-your-azure-data-lake-store"></a>Tworzenie struktury katalogu w sklepie Lake danych Azure

Możesz utworzyć katalogów na koncie magazynu Lake Azure danych do przechowywania danych i zarządzanie nimi.

1. Określ katalog główny.

        $myrootdir = "/"

2. Tworzenie nowego katalogu o nazwie **mynewdirectory** w obszarze określony katalog główny.

        New-AzureRmDataLakeStoreItem -Folder -AccountName $dataLakeStoreName -Path $myrootdir/mynewdirectory

3. Upewnij się, pomyślnie utworzenia nowego katalogu.

        Get-AzureRmDataLakeStoreChildItem -AccountName $dataLakeStoreName -Path $myrootdir

    Należy go wyświetlić wyniki podobne do następujących:

    ![Sprawdź katalogu] (./media/data-lake-store-get-started-powershell/ADL.PS.Verify.Dir.Creation.png "Sprawdź katalogu")


## <a name="upload-data-to-your-azure-data-lake-store"></a>Przekazywanie danych do sklepu Lake danych Azure

Możesz przekazać danych do magazynu Lake danych bezpośrednio na poziomie głównym lub do katalogu, w którym został utworzony w ramach tego konta. Wstawki poniżej pokazano, jak przekazać kilka przykładowych danych do katalogu (**mynewdirectory**) została utworzona w poprzedniej sekcji.

Jeśli szukasz kilka przykładowych danych do przekazania zostanie wyświetlony folder **Pogotowie danych** z [Repozytorium cyfra Lake danych Azure](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData). Pobierz plik i zapisać go w katalogu lokalnym na komputerze, na przykład C:\sampledata\.

    Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "C:\sampledata\vehicle1_09142014.csv" -Destination $myrootdir\mynewdirectory\vehicle1_09142014.csv


## <a name="rename-download-and-delete-data-from-your-data-lake-store"></a>Zmienianie nazwy, pobieranie i usuwanie danych z magazynu Lake danych

Aby zmienić nazwę pliku, użyj następującego polecenia:

    Move-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014.csv -Destination $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

Aby pobrać plik, użyj następującego polecenia:

    Export-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv -Destination "C:\sampledata\vehicle1_09142014_Copy.csv"

Aby usunąć plik, użyj następującego polecenia:

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

Po wyświetleniu monitu wprowadź **Y** , aby usunąć element. Jeśli masz więcej niż jeden plik, aby usunąć można udostępnić wszystkie ścieżki, oddzielając je przecinkami.

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014.csv, $myrootdir\mynewdirectoryvehicle1_09142014_Copy.csv

## <a name="delete-your-azure-data-lake-store-account"></a>Usuwanie konta magazynu Lake danych Azure

Użyj następującego polecenia, aby usunąć konto magazynu Lake danych.

    Remove-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

Po wyświetleniu monitu wprowadź **Y** , aby usunąć konto.


## <a name="next-steps"></a>Następne kroki

- [Zabezpieczanie danych w magazynie Lake danych](data-lake-store-secure-data.md)
- [Używanie analizy Lake Azure danych z magazynu Lake danych](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Usługa Azure HDInsight za pomocą magazynu Lake danych](data-lake-store-hdinsight-hadoop-use-portal.md)
