<properties
    pageTitle="Używanie Azure miejsca do magazynowania w aplikacji ze Sklepu Windows | Microsoft Azure"
    description="Dowiedz się, jak utworzyć aplikację ze Sklepu Windows, która korzysta z magazynu obiektów Blob platformy Azure, kolejki, tabeli lub pliku."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>
    
# <a name="how-to-use-azure-storage-in-windows-store-apps"></a>Jak używać magazyn Azure w aplikacji ze Sklepu Windows

## <a name="overview"></a>Omówienie

Ten przewodnik pokazano, jak rozpocząć pracę z opracowywania aplikacji ze Sklepu Windows, która korzysta z magazynu Azure.

## <a name="download-required-tools"></a>Pobierz narzędzia potrzebne

- [Visual Studio](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx) ułatwia tworzenie, debugowanie, localize pakietu i wdrażanie aplikacji ze Sklepu Windows. Wymagany jest program Visual Studio 2012 lub nowszym.
- [Biblioteka klienta miejsca do magazynowania Azure](https://www.nuget.org/packages/WindowsAzure.Storage) udostępnia Biblioteka klas Windows Runtime do pracy z magazyn Azure.
- [WCF danych usług narzędzia dla systemu Windows aplikacje sklepu](http://www.microsoft.com/download/details.aspx?id=30714) rozszerza środowiska Dodawanie odwołanie do usługi z obsługą OData po stronie klienta w przypadku aplikacji ze Sklepu Windows w programie Visual Studio.

## <a name="develop-apps"></a>Można opracowywać aplikacje

### <a name="getting-ready"></a>Przygotowanie

Tworzenie nowego projektu aplikacji ze Sklepu Windows programu Visual Studio 2012 lub nowszego:

![Magazyn aplikacji — miejsca do magazynowania — w porównaniu z — z projektem][store-apps-storage-vs-project]

Następnie dodaj odwołanie do biblioteki klienta miejsca do magazynowania Azure prawym przyciskiem myszy **odwołania**, klikając **Dodaj odwołanie**, a następnie przechodząc do miejsca do magazynowania klienta biblioteki dla Windows Runtime pobranego:

![Magazyn aplikacji — miejsca do magazynowania — wybierz biblioteki][store-apps-storage-choose-library]

### <a name="using-the-library-with-the-blob-and-queue-services"></a>Korzystanie z biblioteki z usługami obiektów Blob i kolejki

W tym momencie aplikacji jest gotowy do połączeń usługi obiektów Blob platformy Azure i kolejki. Dodaj poniższe instrukcje **za pomocą** tak, aby typów magazynów Azure można odwoływać się bezpośrednio:

    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;

Następnie dodać przycisk do strony. Dodaj następujący kod do zdarzenia jego **kliknięciu** i modyfikowanie metodę obsługi zdarzeń przy użyciu [słów kluczowych asynchroniczne](http://msdn.microsoft.com/library/vstudio/hh156513.aspx):

    var credentials = new StorageCredentials(accountName, accountKey);
    var account = new CloudStorageAccount(credentials, true);
    var blobClient = account.CreateCloudBlobClient();
    var container = blobClient.GetContainerReference("container1");
    await container.CreateIfNotExistsAsync();

Kod założono, że ciągu dwóch zmiennych, *Nazwa konta* i *accountKey*. Reprezentują nazwę konta magazynu i klucz konta, który jest skojarzony z tym kontem.

Tworzenie i uruchamianie aplikacji. Klikając przycisk Sprawdź, czy kontenera o nazwie *container1* istnieje na koncie a następnie utworzyć, jeśli nie.

### <a name="using-the-library-with-the-table-service"></a>Korzystanie z biblioteki w usłudze tabeli

Typy, które są używane do komunikowania się z usługą Azure tabeli zależą od usługi danych WCF w bibliotece aplikacji ze Sklepu Windows. Następnie dodaj odwołania do bibliotek wymagane WCF przy użyciu konsoli Menedżera pakietu:

![aplikacje-miejsca do magazynowania — pakiet — kierownik sklepu][store-apps-storage-package-manager]

Użyj następującego polecenia do Menedżera pakiet punktu do lokalizacji na komputerze:

    Install-Package Microsoft.Data.OData.WindowsStore -Source "C:\Program Files (x86)\Microsoft WCF Data Services\5.0\bin\NuGet"

To polecenie spowoduje automatyczne dodanie wszystkie wymagane odwołania do projektu. Jeśli nie chcesz korzystać z konsoli Menedżera pakietów, możesz dodać folder NuGet usługi danych WCF na komputerze lokalnym do listy źródeł pakietu, a następnie dodaj odwołanie za pośrednictwem interfejsu użytkownika, zgodnie z opisem w [Zarządzaniu NuGet pakietów przy użyciu okna dialogowego](http://docs.nuget.org/docs/start-here/Managing-NuGet-Packages-Using-The-Dialog).

Gdy zdefiniowano odwołania pakietu NuGet usługi danych WCF, Zmień kod w przypadku **kliknij** przycisk swojego:

    var credentials = new StorageCredentials(accountName, accountKey);
    var account = new CloudStorageAccount(credentials, true);
    var tableClient = account.CreateCloudTableClient();
    var table = tableClient.GetTableReference("table1");
    await table.CreateIfNotExistsAsync();

Kod sprawdza, czy istnieje na koncie w tabeli o nazwie *tabela1* , a następnie utworzy, jeśli nie.

Możesz również dodać odwołanie do Microsoft.WindowsAzure.Storage.Table.dll, który jest dostępny w pakiecie, który został pobrany. Ta biblioteka zawiera dodatkowe funkcje, takie jak opartej odbicia szeregowania i ogólnego kwerendy. Należy zauważyć, że ta biblioteka nie obsługuje języka JavaScript.



[store-apps-storage-vs-project]: ./media/storage-use-store-apps/store-apps-storage-vs-project.png
[store-apps-storage-choose-library]: ./media/storage-use-store-apps/store-apps-storage-choose-library.png
[store-apps-storage-package-manager]: ./media/storage-use-store-apps/store-apps-storage-package-manager.png
