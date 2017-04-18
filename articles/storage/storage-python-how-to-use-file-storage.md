<properties
    pageTitle="Jak używać magazyn plików Azure z Python | Microsoft Azure"
    description="Dowiedz się, jak używać magazyn plików Azure z Python przekazywania, listy, pobieranie i usuwanie plików."
    services="storage"
    documentationCenter="python"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="robinsh"/>

# <a name="how-to-use-azure-file-storage-from-python"></a>Jak używać magazyn plików Azure z Python

[AZURE.INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-files](../../includes/storage-try-azure-tools-files.md)]

## <a name="overview"></a>Omówienie

W tym artykule pojawi się, jak wykonywać typowe scenariusze przy użyciu magazyn plików. Przykłady są zapisane w Python i używanie [Microsoft Azure miejsca do magazynowania SDK dla Python]. Scenariusze, w których zawierać przekazywanie, wyświetlanie, pobieranie i usuwanie plików.

[AZURE.INCLUDE [storage-file-concepts-include](../../includes/storage-file-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-share"></a>Tworzenie udziału

Obiekt **FileService** umożliwia pracę z akcji, katalogi i pliki. Poniższy kod tworzy obiekt **FileService** . Dodaj następujący w górnej części dowolnego pliku Python, w którym chcesz programowy dostęp do magazynowania Azure.

    from azure.storage.file import FileService

Poniższy kod tworzy obiekt **FileService** przy użyciu klucza konta i nazwę konta magazynu.  Zamień "Moje konto" i "klucze" Nazwa konta i klucza.

    file_service = **FileService** (account_name='myaccount', account_key='mykey')

W poniższym przykładzie można utworzyć udział, jeśli nie istnieje za pomocą obiektu **FileService** .

    file_service.create_share('myshare')

## <a name="upload-a-file-into-a-share"></a>Przekazywanie pliku do udziału

Udostępnianie miejsca do magazynowania plików Azure zawiera co najmniej, katalogu, w którym mogą znajdować się pliki. W tej sekcji dowiesz się, jak przekazywać plików z magazynu lokalnego do katalogu głównego udziału.

Aby utworzyć plik i przekaż danych, należy użyć **Tworzenie\_pliku\_z\_ścieżkę**, **Tworzenie\_pliku\_z\_strumienia**, **Tworzenie\_pliku\_z\_bajtów** lub **Tworzenie\_pliku\_z\_tekstu** metod. Są one wysokiego poziomu metod, które wykonują niezbędne segmentu gdy rozmiar danych przekracza 64 MB.

**Tworzenie\_pliku\_z\_ścieżki** przesłane zawartość pliku z określonej ścieżce i **Tworzenie\_pliku\_z\_strumienia** przesłane zawartość ze już otwarty plik/strumienia. **Tworzenie\_pliku\_z\_bajtów** przekazywanie tablicę bajtów, i **Tworzenie\_pliku\_z\_tekstu** przekazywanie wartość określony tekst przy użyciu określonego kodowania (domyślnie UTF-8).

Poniższy przykład przesłane zawartość pliku **sunset.png** do pliku **mój_plik** .

    from azure.storage.file import ContentSettings
    file_service.create_file_from_path(
        'myshare',
        None, # We want to create this blob in the root directory, so we specify None for the directory_name
        'myfile',
        'sunset.png',
        content_settings=ContentSettings(content_type='image/png'))

## <a name="how-to-create-a-directory"></a>Jak: Tworzenie katalogu

Można również uporządkować miejsca do magazynowania, wysyłanie plików wewnątrz podkatalogów zamiast je wszystkie w katalogu głównym. Usługa magazynu pliku Azure umożliwia tworzenie tyle katalogów, aby umożliwić konta. Kod poniżej utworzy podrzędne katalogu o nazwie **sampledir** w katalogu głównym.

    file_service.create_directory('myshare', 'sampledir')

## <a name="how-to-list-files-and-directories-in-a-share"></a>Jak: Lista pliki i katalogi w udziale

Aby wyświetlić pliki i katalogi w udziale, za pomocą **listy\_katalogów\_i\_pliki** metody. Ta metoda zwraca generator. Poniższy kod wyświetla **nazwę** każdego pliku i katalogu w udziale do konsoli.

    generator = file_service.list_directories_and_files('myshare')
    for file_or_dir in generator:
        print(file_or_dir.name)

## <a name="download-files"></a>Pobieranie plików

Aby pobrać dane z pliku, użyj **uzyskać\_pliku\_do\_ścieżkę**, **Uzyskiwanie\_pliku\_do\_strumienia**, **Uzyskiwanie\_pliku\_do\_bajtów**, lub **uzyskać\_pliku\_do\_tekstu**. Są one wysokiego poziomu metod, które wykonują niezbędne segmentu gdy rozmiar danych przekracza 64 MB.

W poniższym przykładzie pokazano przy użyciu **uzyskać\_pliku\_do\_ścieżki** pobrać zawartość pliku **mój_plik** i zapisać go w pliku **sunset.png w nowym oknie** .

    file_service.get_file_to_path('myshare', None, 'myfile', 'out-sunset.png')

## <a name="delete-a-file"></a>Usuwanie pliku

Na koniec aby usunąć plik, zadzwoń do **delete_file**.

    file_service.delete_file('myshare', None, 'myfile')

## <a name="next-steps"></a>Następne kroki

Teraz, gdy znasz już podstawy przechowywania plików, skorzystaj z poniższych łączy, aby dowiedzieć się więcej.

- [Centrum deweloperów Python](/develop/python/)
- [Usługi Azure magazyn interfejsu API usługi REST](http://msdn.microsoft.com/library/azure/dd179355)
- [Blog zespołu Azure miejsca do magazynowania]
- [Magazyn Microsoft Azure SDK dla Python]

[Blog zespołu Azure miejsca do magazynowania]: http://blogs.msdn.com/b/windowsazurestorage/
[Magazyn Microsoft Azure SDK dla Python]: https://github.com/Azure/azure-storage-python
