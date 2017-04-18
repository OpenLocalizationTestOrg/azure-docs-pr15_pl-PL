<properties
    pageTitle="Wprowadzenie do przechowywania Azure w pięć minut | Microsoft Azure"
    description="Szybko rozpocząć wydajną pracę w Microsoft Azure blob, tabel i kolejek przy użyciu szybkiego uruchamiania Azure miejsca do magazynowania, Visual Studio i emulatora Azure miejsca do magazynowania. Uruchom pierwszej aplikacji magazyn Azure w pięć minut."
    services="storage"
    documentationCenter=".net"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="get-started-with-azure-storage-in-five-minutes"></a>Wprowadzenie do przechowywania Azure w pięć minut

## <a name="overview"></a>Omówienie

Łatwiej wprowadzenie do opracowywania z nośnikami Azure. Ten samouczek pokazano, jak na uruchomienie aplikacji magazyn Azure się szybko. Za pomocą szybkiego startu szablonów wiadomości Azure SDK dla środowiska .NET. Te szybkiego uruchamiania zawierają kody gotowe Szybka instalacja, demonstrujący niektóre podstawowe scenariusze programowania z nośnikami Azure.

Aby uzyskać więcej informacji na temat przechowywania Azure przed do nurkowania do kodu, zobacz [Następne kroki](#next-steps).

## <a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem musisz następujące wymagania:

1. Gromadzenie i utworzenie aplikacji, musisz wersji programu [Visual Studio](https://www.visualstudio.com/) zainstalowany na Twoim komputerze.

2. Zainstaluj najnowszą wersję [Zestawu SDK Azure dla środowiska .NET](https://azure.microsoft.com/downloads/). Zestaw SDK zawiera projekty przykładowe Azure Szybki Start, emulatora Azure miejsca do magazynowania i [Biblioteka klienta Azure miejsca do magazynowania dla środowiska .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).

3. Upewnij się, że masz [.NET Framework 4,5](http://www.microsoft.com/download/details.aspx?id=30653) zainstalowany na Twoim komputerze, jak jest to wymagane przez projektów przykładowe Azure Szybki Start, które firma Microsoft będzie używane w tym samouczku.

    Jeśli nie masz pewności, która wersja programu .NET Framework jest zainstalowana na komputerze, zobacz [jak: określić, które .NET Framework są zainstalowane wersje](https://msdn.microsoft.com/vstudio/hh925568.aspx). Lub naciśnij klawisz systemu Windows, lub przycisk **Start** , wpisz **Frazę Panel sterowania**. Następnie kliknij pozycję **Programy** > **Programy i funkcje**i sprawdź, czy 4,5 .NET Framework znajduje się między zainstalowanych programów.

4. Musisz subskrypcji usługi Azure i konto Azure miejsca do magazynowania.

    - Aby uzyskać subskrypcję usługi Azure, zobacz [Bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/), [Opcje zakupu](https://azure.microsoft.com/pricing/purchase-options/)i [Zawiera element członkowski](https://azure.microsoft.com/pricing/member-offers/) (w przypadku Członkowie w witrynie MSDN, Microsoft Partner Network i BizSpark oraz innych programów firmy Microsoft).
    - Aby utworzyć konta magazynu platformy Azure, zobacz, [jak utworzyć konto miejsca do magazynowania](storage-create-storage-account.md#create-a-storage-account).

## <a name="run-your-first-azure-storage-application-against-azure-storage-in-the-cloud"></a>Wykonywane pierwszej aplikacji magazyn Azure Azure magazynu w chmurze

Po utworzeniu konta możesz utworzyć prostą aplikację magazyn Azure przy użyciu jednego z projektów przykładowe Azure szybkiego uruchamiania w programie Visual Studio. Ten samouczek omówiono projektów próbki do przechowywania Azure: **Magazyn Azure: obiektów blob**, **Magazyn Azure: pliki**, **Magazyn Azure: kolejek**, i **Magazyn Azure: tabele**:

1. Uruchom program Visual Studio.
2. W menu **plik** kliknij polecenie **Nowy projekt**.
3. W oknie dialogowym **Nowy projekt** kliknij **zainstalowane** > **szablonów** > **Visual C#** > **chmury** > **Przewodniki Szybki Start** > **Usług danych**.
    . Wybierz jedną z następujących szablonów: **Magazyn Azure: obiektów blob**, **Magazyn Azure: pliki**, **Magazyn Azure: kolejek**, lub **Magazyn Azure: tabele**.
    b. Upewnij się, że jest wybrany **program .NET Framework 4,5** framework docelowej.
    - 3.c. Określ nazwę dla projektu i tworzyć nowe rozwiązanie programu Visual Studio, jak pokazano:

    ![Azure Szybkie uruchamianie][Image1]

Warto przejrzeć kod źródłowy przed uruchomieniem aplikacji. Aby zapoznać się z kodu, wybierz pozycję **Eksplorator rozwiązań** w menu **Widok** w programie Visual Studio. Następnie kliknij dwukrotnie plik Plik Program.cs.

Następnie uruchom przykładowej aplikacji:

1.  W programie Visual Studio w menu **Widok** wybierz pozycję **Eksplorator rozwiązań** . Otwieranie pliku App.config i komentarz się parametry połączenia dla emulatora Azure miejsca do magazynowania:

    `<!--<add key="StorageConnectionString" value = "UseDevelopmentStorage=true;"/>-->`

2.  Usuń komentarze parametry połączenia dla usługi Azure miejsca do magazynowania i podaj konto nazwy i access klucz magazynowania w pliku App.config:`<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=[AccountName];AccountKey=[AccountKey]"`

    Aby pobrać klucz dostępu do konta miejsca do magazynowania, zobacz [Zarządzanie klawiszy dostępu do miejsca do magazynowania](storage-create-storage-account.md#manage-your-storage-access-keys).

3.  Po podać nazwę konta magazynu i klawisz dostępu w pliku App.config, w menu **plik** kliknij pozycję **Zapisz wszystko** zapisać wszystkie pliki projektu.
4.  W menu **Tworzenie** kliknij przycisk **Konstruuj rozwiązanie**.
5.  W menu **Debugowanie** naciśnij klawisz **F11** , aby uruchomić rozwiązanie krok po kroku lub naciśnij klawisz **F5** , aby uruchomić rozwiązanie.


## <a name="run-your-first-azure-storage-application-locally-against-the-azure-storage-emulator"></a>Uruchamiana pierwszej aplikacji magazyn Azure lokalnie emulatora miejsca do magazynowania Azure

[Emulator miejsca do magazynowania Azure](storage-use-emulator.md) udostępnia środowisku lokalnym, która emuluje usługi obiektów Blob platformy Azure, kolejki i tabeli na potrzeby projektowania. Emulator przestrzeni dyskowej służy do testowania aplikacji miejsca do magazynowania lokalnie, bez tworzenia Azure subskrypcji usługi lub konta miejsca do magazynowania i bez ponoszenia kosztami.

Aby wypróbować go, tworzenie prostej aplikacji magazyn Azure przy użyciu jednego z projektów przykładowe Azure szybkiego uruchamiania w programie Visual Studio. Ten samouczek omówiono projektów przykładowe **Magazyn obiektów Blob platformy Azure**, **Magazyn tabel platformy Azure**i **Magazyn kolejki Azure** :

1. Uruchom program Visual Studio.
2. W menu **plik** kliknij polecenie **Nowy projekt**.
3. W oknie dialogowym **Nowy projekt** kliknij **zainstalowane** > **szablonów** > **Visual C#** > **chmury** > **Przewodniki Szybki Start** > **Usług danych**.
    . Wybierz jedną z następujących szablonów: **Magazyn Azure: obiektów blob**, **Magazyn Azure: pliki**, **Magazyn Azure: kolejek**, lub **Magazyn Azure: tabele**.
    b. Upewnij się, że jest wybrany **program .NET Framework 4,5** framework docelowej.
    c. Określ nazwę dla projektu i tworzyć nowe rozwiązanie programu Visual Studio, jak pokazano:

    ![Azure Szybkie uruchamianie][Image1]

4.  W programie Visual Studio w menu **Widok** wybierz pozycję **Eksplorator rozwiązań** . Otwórz plik App.config i komentarz się parametry połączenia dla Twojego konta Azure miejsca do magazynowania, jeśli zostało już dodane. Następnie usuń komentarze parametry połączenia dla emulatora Azure miejsca do magazynowania:

    `<add key="StorageConnectionString" value = "UseDevelopmentStorage=true;"/>`

Warto przejrzeć kod źródłowy przed uruchomieniem aplikacji. Aby zapoznać się z kodu, wybierz pozycję **Eksplorator rozwiązań** w menu **Widok** w programie Visual Studio. Następnie kliknij dwukrotnie plik Plik Program.cs.

Następnie uruchomić aplikację próbki w emulatorze Azure miejsca do magazynowania:

1.  Naciśnij klawisz systemu Windows, wyszukaj *emulatora magazyn Microsoft Azure*, lub przycisk **Start** i uruchomić aplikację. Po uruchomieniu emulatorze pojawi się ikona i powiadomień w obszarze widoku zadań systemu Windows.
2.  W programie Visual Studio kliknij przycisk **Konstruuj rozwiązanie** w menu **Tworzenie** .
3.  W menu **Debugowanie** naciśnij klawisz **F11** , aby uruchomić rozwiązanie krok po kroku, lub naciśnij klawisz **F5** , aby uruchomić rozwiązanie od początku do końca.

## <a name="next-steps"></a>Następne kroki

Zobacz następujące zasoby, aby dowiedzieć się więcej o magazyn Azure:

* [Wprowadzenie do magazynu platformy Microsoft Azure](storage-introduction.md)
* [Rozpoczynanie pracy z Eksploratora magazynu platformy Azure](../vs-azure-tools-storage-manage-with-storage-explorer.md)
* [Rozpoczynanie pracy z magazynem obiektów Blob platformy Azure za pomocą .NET](storage-dotnet-how-to-use-blobs.md)
* [Rozpoczynanie pracy z magazynem tabel platformy Azure za pomocą .NET](storage-dotnet-how-to-use-tables.md)
* [Wprowadzenie do magazynowania kolejki Azure za pomocą .NET](storage-dotnet-how-to-use-queues.md)
* [Wprowadzenie do przechowywania plików Azure w systemie Windows](storage-dotnet-how-to-use-files.md)
* [Przesyłanie danych za pomocą narzędzia wiersza polecenia AzCopy](storage-use-azcopy.md)
* [Dokumentacja Azure miejsca do magazynowania](https://azure.microsoft.com/documentation/services/storage/)
* [Biblioteka klienta Microsoft Azure miejsca do magazynowania dla środowiska .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [Usługi Azure magazyn interfejsu API usługi REST](https://msdn.microsoft.com/library/azure/dd179355.aspx)

[Image1]: ./media/storage-getting-started-guide/QuickStart.png
