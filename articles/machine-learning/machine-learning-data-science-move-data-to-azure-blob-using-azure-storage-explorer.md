<properties 
    pageTitle="Przenoszenie danych do i z magazynem obiektów Blob platformy Azure za pomocą Eksploratora magazynu Azure | Microsoft Azure" 
    description="Przenoszenie danych do i z magazynem obiektów Blob platformy Azure za pomocą Eksploratora magazynu platformy Azure" 
    services="machine-learning,storage" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/31/2016"
    ms.author="bradsev" />

# <a name="move-data-to-and-from-azure-blob-storage-using-azure-storage-explorer"></a>Przenoszenie danych do i z magazynem obiektów Blob platformy Azure za pomocą Eksploratora magazynu platformy Azure

Azure Eksplorator magazynu to bezpłatne narzędzie firmy Microsoft, który umożliwia pracę z danymi Azure miejsca do magazynowania w systemie Windows, macOS i Linux. W tym temacie opisano, jak używać tej funkcji do przekazywania i pobierania danych z magazynem obiektów blob Azure. Narzędzie można pobrać z [Eksploratora magazynu usługi Microsoft Azure](http://storageexplorer.com/).

Wytyczne dotyczące technologii umożliwia przenoszenie danych do i/lub z magazynem obiektów Blob platformy Azure są połączone poniżej:
 
[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]   

 
> [AZURE.NOTE] Jeśli używasz maszyn wirtualnych, który został skonfigurowany za pomocą skryptów dostarczony przez [maszyn wirtualnych nauki danych platformy Azure](machine-learning-data-science-virtual-machines.md)Azure Eksplorator magazynu jest już zainstalowany na maszyn wirtualnych.
 
> [AZURE.NOTE] Pełne wprowadzenie z magazynem obiektów blob platformy Azure zobacz [Podstawy obiektów Blob platformy Azure](../storage/storage-dotnet-how-to-use-blobs.md) i [Azure obiektów Blob usługi](https://msdn.microsoft.com/library/azure/dd179376.aspx).   

## <a name="prerequisites"></a>Wymagania wstępne

Ten dokument przyjęto założenie, że masz subskrypcję usługi Azure, konto miejsca do magazynowania i odpowiedni klucz miejsca do magazynowania dla tego konta. Przed przekazywania i pobierania danych, musisz znać Azure magazynowania nazwy i konta klucz konta. 

- Aby skonfigurować Azure subskrypcji, zobacz [bezpłatną wersję próbną jednego miesiąca](https://azure.microsoft.com/pricing/free-trial/).
- Aby uzyskać instrukcje dotyczące tworzenia konta miejsca do magazynowania i Uzyskiwanie konta i informacje o kluczu zobacz [temat Azure miejsca do magazynowania konta](../storage/storage-create-storage-account.md). Zanotuj klucz dostępu do konta miejsca do magazynowania niezbędnej ten klucz do nawiązania połączenia z kontem przy użyciu narzędzia Eksplorator magazynu Azure.
- Narzędzie Azure Eksploratora magazynu można pobrać z [Eksploratora magazynu usługi Microsoft Azure](http://storageexplorer.com/). Zaakceptuj ustawienia domyślne podczas instalacji.


<a id="explorer"></a>
## <a name="use-azure-storage-explorer"></a>Za pomocą Eksploratora magazynu platformy Azure 

Poniższe kroki dokumentu jak przekazywanie i pobieranie danych przy użyciu Eksploratora magazynu Azure. 

1.  Uruchom Eksploratora magazynu platformy Microsoft Azure.
2.  Aby wyświetlić kreatora **Zaloguj się do konta...** , wybierz ikonę **Ustawienia konta Azure** , a następnie **Dodaj konto** i wprowadź możesz poświadczeń. ![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/add-an-azure-store-account.png)
3.  Aby wyświetlić kreatora **Nawiązywanie połączenia z magazynem Azure** , wybierz ikonę **Nawiązywanie połączenia z magazynem Azure** . ![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/connect-to-azure-storage-1.png)
4. Wprowadź kod dostępu z konta usługi Azure miejsca do magazynowania na **Nawiązywanie połączenia z magazynem Azure** kreatora, a następnie **Dalej**. ![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/connect-to-azure-storage-2.png)
5. Wprowadź nazwę konta magazynu w polu **Nazwa konta** , a następnie wybierz przycisk **Dalej**. ![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/attach-external-storage.png)
6. Teraz powinny być wymienione dodane konto miejsca do magazynowania. Aby utworzyć kontener obiektów blob na koncie miejsca do magazynowania, kliknij prawym przyciskiem myszy węzeł **Kontenerów obiektów Blob** tego konta, wybierz pozycję **Tworzenie kontenera obiektów Blob**i wprowadź nazwę.
7. Aby przekazać danych do kontenera, zaznacz kontenera docelowego i kliknij przycisk **Przekaż** .![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/storage-accounts.png)
8. Kliknij na **...** po prawej stronie pola **pliki** , wybierz jeden lub wiele plików przekazać z systemu plików i kliknij przycisk **Przekaż** , aby rozpocząć przekazywanie plików.![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/upload-files-to-blob.png)
7. Aby pobrać dane, wybierając to w kontenerze odpowiednich, aby pobrać i kliknij przycisk **Pobierz**. ![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/download-files-from-blob.png)


