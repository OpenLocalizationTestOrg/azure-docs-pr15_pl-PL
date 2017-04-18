<properties
    pageTitle="Zarządzanie dostępem anonimowym odczytu kontenerów i obiektów blob | Microsoft Azure"
    description="Dowiedz się, jak udostępnić kontenerów i obiektów blob do dostęp anonimowy i jak na programowy dostęp do."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="manage-anonymous-read-access-to-containers-and-blobs"></a>Zarządzanie dostępem anonimowym odczytu kontenerów i obiektów blob

## <a name="overview"></a>Omówienie

Domyślnie tylko właściciel konta miejsca do magazynowania może uzyskać dostęp do zasobów miejsca do magazynowania w ramach tego konta. Tylko magazyn obiektów Blob możesz ustawić uprawnień kontenera, aby umożliwić dostęp anonimowy odczytu kontenera i jego obiektów blob tak, aby można udzielić dostępu do tych zasobów bez udostępniania klucz konta.

Dostęp anonimowy jest najbardziej przydatna w przypadku scenariuszy, w którym chcesz niektórych obiektów blob, które zawsze będą dostępne dla anonimowego dostępu do odczytu. Precyzyjny system kontrolki możesz utworzyć podpis udostępniania, który umożliwia dostęp ograniczony pełnomocnictw przy użyciu różnych uprawnień i nad w określonym interwale czasu. Aby uzyskać więcej informacji na temat tworzenia podpisów udostępniania zobacz [Używanie podpisów dostępu udostępnionego (SA)](storage-dotnet-shared-access-signature-part-1.md).

## <a name="grant-anonymous-users-permissions-to-containers-and-blobs"></a>Udzielanie uprawnień użytkownikom anonimowym na kontenerów i obiektów blob

Domyślnie kontenera i wszelkie blob znajdujące się w nim można uzyskać dostęp tylko przez właściciela konta miejsca do magazynowania. Aby przekazać użytkownikom anonimowym odczyt kontenera i jego obiektów blob, można ustawić uprawnienia kontenera umożliwiają dostęp publicznej. Anonimowe użytkownicy mogą odczytywać blob znajdujące się w kontenerze publicznie bez uwierzytelniania wezwanie.

Kontenery zawierają następujące opcje Zarządzanie dostępem kontenera:

- **Pełny dostęp do odczytu publicznej:** Kontener i obiektów blob danych można odczytywać przy użyciu anonimowego żądania. Klientów można wyliczyć obiektów blob w kontenerze przy użyciu anonimowego żądania, ale nie można wyliczyć kontenerów w ramach konta miejsca do magazynowania.

- **Publicznej Odczyt dla obiektów blob tylko:** Obiektów blob danych w tym kontenerze można odczytywać przy użyciu anonimowego żądania, ale kontenera dane nie są dostępne. Klienci nie można wyliczyć obiektów blob w kontenerze przy użyciu anonimowego żądania.

- **Publicznych Odczyt:** Kontener i obiektów blob danych może być odczytywane przez tylko właściciel konta.

Kontener uprawnienia można ustawić w następujący sposób:

- [Azure Portal](https://portal.azure.com).
- Programistycznie korzystając z biblioteki klienta miejsca do magazynowania lub interfejsu API usługi REST.
- Przy użyciu programu PowerShell. Aby uzyskać informacje o ustawianiu uprawnień do kontenera z Azure programu PowerShell, zobacz [Przy użyciu programu PowerShell Azure z nośnikami Azure](storage-powershell-guide-full.md#how-to-manage-azure-blobs).

### <a name="setting-container-permissions-from-the-azure-portal"></a>Ustawianie uprawnień kontenera z Azure Portal

Aby ustawić uprawnienia kontener z [Azure Portal](https://portal.azure.com), wykonaj następujące kroki:

1. Przejdź do pozycji pulpit nawigacyjny dla Twojego konta miejsca do magazynowania.
2. Wybierz nazwę kontenera z listy. Kliknięcie nazwy udostępnia BLOB w wybranym kontenerze
3. Wybierz **zasadę dostępu** na pasku narzędzi.
4. W polu **Typ dostępu** wybierz żądany poziom uprawnień, jak pokazano poniżej ekranu.

    ![Edytowanie metadanych kontenera, okno dialogowe](./media/storage-manage-access-to-resources/storage-manage-access-to-resources-0.png)

### <a name="setting-container-permissions-programmatically-using-net"></a>Ustawianie uprawnień kontenera programowo przy użyciu programu .NET

Aby ustawić uprawnienia dla kontenera przy użyciu Biblioteka klienta .NET, najpierw pobrać kontenera istniejących uprawnień przez wywołanie metody **GetPermissions** . Następnie należy ustawić właściwość **PublicAccess** obiektu **BlobContainerPermissions** , który został zwrócony przez metodę **GetPermissions** . Na koniec wywołania metody **ustawiania** uprawnieniami zaktualizowane.

Poniższy przykład ustawia kontenera uprawnienia do odczytu publicznej. Aby ustawić uprawnienia publicznej dostęp do odczytu dla obiektów blob, ustawić właściwość **PublicAccess** **BlobContainerPublicAccessType.Blob**. Aby usunąć wszystkie uprawnienia dla użytkowników anonimowych, ustaw właściwość **BlobContainerPublicAccessType.Off**.

    public static void SetPublicContainerPermissions(CloudBlobContainer container)
    {
        BlobContainerPermissions permissions = container.GetPermissions();
        permissions.PublicAccess = BlobContainerPublicAccessType.Container;
        container.SetPermissions(permissions);
    }

## <a name="access-containers-and-blobs-anonymously"></a>Dostęp do kontenerów i obiektów blob anonimowe

Klient uzyskuje dostęp do kontenerów i obiektów blob anonimowo można użyć konstruktorów, które nie wymagają poświadczeń. W poniższych przykładach pokazano kilka różnych metod anonimowo odwołanie obiektów Blob usługi zasobów.

### <a name="create-an-anonymous-client-object"></a>Tworzenie obiektu klienta anonimowe

Możesz utworzyć nowy obiekt klienta usługi dla dostęp anonimowy, dostarczając punktu końcowego usługi obiektów Blob dla konta. Jednak trzeba również znasz nazwę kontenera tego konta, które są dostępne dla dostęp anonimowy.

    public static void CreateAnonymousBlobClient()
    {
        // Create the client object using the Blob service endpoint.
        CloudBlobClient blobClient = new CloudBlobClient(new Uri(@"https://storagesample.blob.core.windows.net"));

        // Get a reference to a container that's available for anonymous access.
        CloudBlobContainer container = blobClient.GetContainerReference("sample-container");

        // Read the container's properties. Note this is only possible when the container supports full public read access.
        container.FetchAttributes();
        Console.WriteLine(container.Properties.LastModified);
        Console.WriteLine(container.Properties.ETag);
    }

### <a name="reference-a-container-anonymously"></a>Dokumentacja dotycząca kontenera anonimowe

Jeśli masz adres URL do kontenera anonimowo dostępnej umożliwia go bezpośrednio odwołać kontenerze.

    public static void ListBlobsAnonymously()
    {
        // Get a reference to a container that's available for anonymous access.
        CloudBlobContainer container = new CloudBlobContainer(new Uri(@"https://storagesample.blob.core.windows.net/sample-container"));

        // List blobs in the container.
        foreach (IListBlobItem blobItem in container.ListBlobs())
        {
            Console.WriteLine(blobItem.Uri);
        }
    }


### <a name="reference-a-blob-anonymously"></a>Dokumentacja dotycząca obiektów blob anonimowe

Jeśli masz adres URL umożliwiający obiektów blob, który jest dostępny dla dostęp anonimowy, można odwoływać się do obiektów blob, bezpośrednio przy użyciu tego adresu URL:

    public static void DownloadBlobAnonymously()
    {
        CloudBlockBlob blob = new CloudBlockBlob(new Uri(@"https://storagesample.blob.core.windows.net/sample-container/logfile.txt"));
        blob.DownloadToFile(@"C:\Temp\logfile.txt", System.IO.FileMode.Create);
    }

## <a name="features-available-to-anonymous-users"></a>Funkcje dostępne dla użytkowników anonimowych

Poniższa tabela przedstawia operacje może zajść przez użytkowników anonimowych, gdy kontener ACL jest ustawiona na dostęp do publicznego.

| Operacja REST                                         | Uprawnienia do odczytu publicznej | Uprawnienia do odczytu publicznej dla obiektów blob tylko |
|--------------------------------------------------------|-----------------------------------------|---------------------------------------------------|
| Lista kontenerów                                        | Tylko właściciel                              | Tylko właściciel                                        |
| Tworzenie kontenera                                       | Tylko właściciel                              | Tylko właściciel                                        |
| Uzyskiwanie właściwości kontenera                               | Wszystkie                                     | Tylko właściciel                                        |
| Pobieranie metadanych kontenera                                 | Wszystkie                                     | Tylko właściciel                                        |
| Ustawianie kontenera metadanych                                 | Tylko właściciel                              | Tylko właściciel                                        |
| Uzyskiwanie ACL kontenera                                      | Tylko właściciel                              | Tylko właściciel                                        |
| Ustawianie ACL kontenera                                      | Tylko właściciel                              | Tylko właściciel                                        |
| Usuń kontener                                       | Tylko właściciel                              | Tylko właściciel                                        |
| Lista obiektów blob                                             | Wszystkie                                     | Tylko właściciel                                        |
| Umieszczanie obiektów Blob                                               | Tylko właściciel                              | Tylko właściciel                                        |
| Uzyskiwanie obiektów Blob                                               | Wszystkie                                     | Wszystkie                                               |
| Pobierz właściwości obiektów Blob                                    | Wszystkie                                     | Wszystkie                                               |
| Ustawianie właściwości obiektów Blob                                    | Tylko właściciel                              | Tylko właściciel                                        |
| Pobieranie metadanych obiektów Blob                                      | Wszystkie                                     | Wszystkie                                               |
| Ustawianie metadanych obiektów Blob                                      | Tylko właściciel                              | Tylko właściciel                                        |
| Umieszczenie bloku                                              | Tylko właściciel                              | Tylko właściciel                                        |
| Uzyskiwanie listy zablokowanych (Projekt zatwierdzony — blokuje tylko)                 | Wszystkie                                     | Wszystkie                                               |
| Uzyskiwanie listy zablokowanych (tylko niezatwierdzonym bloków lub wszystkich bloków) | Tylko właściciel                              | Tylko właściciel                                        |
| Umieszczenie Lista bloków                                         | Tylko właściciel                              | Tylko właściciel                                        |
| Usuwanie obiektów Blob                                            | Tylko właściciel                              | Tylko właściciel                                        |
| Kopiowanie obiektów Blob                                              | Tylko właściciel                              | Tylko właściciel                                        |
| Blob migawki                                          | Tylko właściciel                              | Tylko właściciel                                        |
| Blob dzierżawy                                             | Tylko właściciel                              | Tylko właściciel                                        |
| Umieszczenie strony                                               | Tylko właściciel                              | Tylko właściciel                                        |
| Pobieranie strony zakresów                                        | Wszystkie                                     | Wszystkie                                                  |
| Dołączanie obiektów Blob                                            | Tylko właściciel                              | Tylko właściciel                                                  |


## <a name="see-also"></a>Zobacz też

- [Uwierzytelnianie dla usługi Azure miejsca do magazynowania](https://msdn.microsoft.com/library/azure/dd179428.aspx)
- [Korzystanie z podpisów udostępniania (SA)](storage-dotnet-shared-access-signature-part-1.md)
- [Delegowanie dostępu za pomocą podpisu udostępniania](https://msdn.microsoft.com/library/azure/ee395415.aspx)
