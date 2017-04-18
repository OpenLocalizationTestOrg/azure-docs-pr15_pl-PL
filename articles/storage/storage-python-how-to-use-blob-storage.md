<properties
    pageTitle="Jak używać magazyn obiektów Blob platformy Azure (miejsca przechowywania obiektu) z Python | Microsoft Azure"
    description="Przechowywanie danych niestrukturalne w chmurze z magazynem obiektów Blob platformy Azure (miejsca przechowywania obiektu)."
    services="storage"
    documentationCenter="python"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="how-to-use-azure-blob-storage-from-python"></a>Jak używać magazyn obiektów Blob platformy Azure z Python

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Omówienie

Magazyn obiektów Blob platformy Azure to usługa, która przechowuje niestrukturalne dane w chmurze jako obiekty i obiektów blob. Magazyn obiektów blob mogą zawierać dowolnego typu tekst lub dane binarne, takich jak dokument, plik lub Instalator aplikacji. Magazyn obiektów blob jest również określane jako miejsca przechowywania obiektu.

W tym artykule pojawi się, jak wykonywać typowe scenariusze przy użyciu magazyn obiektów Blob. Przykłady są zapisane w Python i używanie [Microsoft Azure miejsca do magazynowania SDK dla Python]. Scenariusze, w których zawierać przekazywanie, wyświetlanie, pobieranie i usuwanie obiektów blob.

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-container"></a>Tworzenie kontenera

Zależności od typu obiektów blob, którego chcesz użyć, tworzenie obiektu **BlockBlobService**, **AppendBlobService**lub **PageBlobService** . Poniższy kod używa obiektu **BlockBlobService** . Dodaj następujący w górnej części dowolnego pliku Python, w którym chcesz programowy dostęp do magazyn obiektów Blob blok Azure.

    from azure.storage.blob import BlockBlobService

Poniższy kod tworzy obiekt **BlockBlobService** przy użyciu klucza konta i nazwę konta magazynu.  Zamień "Moje konto" i "klucze" Nazwa konta i klucza.

    block_blob_service = BlockBlobService(account_name='myaccount', account_key='mykey')

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

W poniższym przykładzie obiekt **BlockBlobService** umożliwia tworzenie kontenera, jeśli nie istnieje.

    block_blob_service.create_container('mycontainer')

Domyślnie nowe kontener jest prywatny, dlatego należy określić klucz dostępu miejsca do magazynowania (tak jak wcześniej) do pobrania obiektów blob z tego kontenera. Jeśli chcesz udostępnić BLOB w kontenerze wszystkim użytkownikom, można utworzyć kontener i przekazać poziom dostępu publicznego za pomocą następującego kodu.

    from azure.storage.blob import PublicAccess
    block_blob_service.create_container('mycontainer', public_access=PublicAccess.Container)

Można też kliknąć można modyfikować kontenera utworzonego za pomocą następującego kodu.

    block_blob_service.set_container_acl('mycontainer', public_access=PublicAccess.Container)

Po tej zmiany może obejrzeć każdy w Internecie obiektów blob w kontenerze publicznej, ale tylko modyfikowanie lub ich usuwanie.

## <a name="upload-a-blob-into-a-container"></a>Przekazywanie obiektów blob do kontenera

Aby utworzyć blob blok i przekazywanie danych, za pomocą **Tworzenie\_obiektów blob\_z\_ścieżkę**, **Tworzenie\_obiektów blob\_z\_strumienia**, **Tworzenie\_obiektów blob\_z\_bajtów** lub **Tworzenie\_obiektów blob\_z\_tekstu** metod. Są one wysokiego poziomu metod, które wykonują niezbędne segmentu gdy rozmiar danych przekracza 64 MB.

**Tworzenie\_obiektów blob\_z\_ścieżki** przesłane zawartość pliku z określonej ścieżce i **Tworzenie\_obiektów blob\_z\_strumienia** przesłane zawartość ze już otwarty plik/strumienia. **Tworzenie\_obiektów blob\_z\_bajtów** przekazywanie tablicę bajtów, a **Tworzenie\_obiektów blob\_z\_tekstu** przekazywanie wartość określony tekst przy użyciu określonego kodowania (domyślnie UTF-8).

Poniższy przykład przesłane zawartość pliku **sunset.png** do obiektów blob **myblob** .

    from azure.storage.blob import ContentSettings
    block_blob_service.create_blob_from_path(
        'mycontainer',
        'myblockblob',
        'sunset.png',
        content_settings=ContentSettings(content_type='image/png')
                )

## <a name="list-the-blobs-in-a-container"></a>Lista obiektów blob w kontenerze

Aby wyświetlić listę obiektów blob w kontenerze, użyj **listy\_obiektów blob** metody. Ta metoda zwraca generator. Poniższy kod wyświetla **nazwę** każdego obiektów blob w kontenerze do konsoli.

    generator = block_blob_service.list_blobs('mycontainer')
    for blob in generator:
        print(blob.name)

## <a name="download-blobs"></a>Pobieranie obiektów blob

Aby pobrać dane z obiektów blob, za pomocą **uzyskać\_obiektów blob\_do\_ścieżkę**, **Uzyskiwanie\_obiektów blob\_do\_strumienia**, **uzyskać\_obiektów blob\_do\_bajtów**, lub **uzyskać\_obiektów blob\_do\_tekstu**. Są one wysokiego poziomu metod, które wykonują niezbędne segmentu gdy rozmiar danych przekracza 64 MB.

W poniższym przykładzie pokazano przy użyciu **uzyskać\_obiektów blob\_do\_ścieżki** pobrać zawartość obiektów blob **myblob** i zapisać go w pliku **sunset.png w nowym oknie** .

    block_blob_service.get_blob_to_path('mycontainer', 'myblockblob', 'out-sunset.png')

## <a name="delete-a-blob"></a>Usuwanie obiektów blob

Na koniec aby usunąć obiektów blob, zadzwoń do **delete_blob**.

    block_blob_service.delete_blob('mycontainer', 'myblockblob')

## <a name="writing-to-an-append-blob"></a>Zapisywanie obiektów blob dołączającej

Dołączanie obiektów blob jest zoptymalizowana pod kątem dołączającej operacji, takich jak rejestrowanie. Jak blob blok obiektów blob dołączającej składa się z bloków, ale można dodać nowy blok do obiektów blob dołączającej, zawsze jest dołączany do końca obiektów blob. Nie można zaktualizować lub usuń istniejący blok w obiektów blob Dołącz. Jak są one dla obiektów blob blok nie dostępne są blok identyfikatorów dla obiektów blob Dołącz.

Każdy blok w obiektów blob dołączającej może być inny rozmiar, maksymalnie 4 MB i obiektów blob dołączającej może zawierać maksymalnie 50 000 bloków. Maksymalny rozmiar obiektów blob Dołącz w związku z tym jest nieco więcej niż 195 GB (blokuje 4 MB X 50 000).

W poniższym przykładzie tworzy nowe blob dołączającej i dołącza niektóre dane, symulowanie operacji rejestrowania prosty.

    from azure.storage.blob import AppendBlobService
    append_blob_service = AppendBlobService(account_name='myaccount', account_key='mykey')

    # The same containers can hold all types of blobs
    append_blob_service.create_container('mycontainer')

    # Append blobs must be created before they are appended to
    append_blob_service.create_blob('mycontainer', 'myappendblob')
    append_blob_service.append_blob_from_text('mycontainer', 'myappendblob', u'Hello, world!')

    append_blob = append_blob_service.get_blob_to_text('mycontainer', 'myappendblob')

## <a name="next-steps"></a>Następne kroki

Teraz, gdy znasz już podstawy magazyn obiektów Blob, skorzystaj z poniższych łączy, aby dowiedzieć się więcej.

- [Centrum deweloperów Python](/develop/python/)
- [Usługi Azure magazyn interfejsu API usługi REST](http://msdn.microsoft.com/library/azure/dd179355)
- [Blog zespołu Azure miejsca do magazynowania]
- [Magazyn Microsoft Azure SDK dla Python]

[Blog zespołu Azure miejsca do magazynowania]: http://blogs.msdn.com/b/windowsazurestorage/
[Magazyn Microsoft Azure SDK dla Python]: https://github.com/Azure/azure-storage-python
