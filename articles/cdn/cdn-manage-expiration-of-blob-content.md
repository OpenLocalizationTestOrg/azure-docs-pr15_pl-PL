<properties
 pageTitle="Zarządzanie wygasania Azure magazyn obiektów blob zawartości w sieci CDN Azure | Microsoft Azure"
 description="Zapoznaj się z opcjami sterowania czasu wygaśnięcia dla obiektów blob w pamięci podręcznej Azure CDN."
 services="cdn"
 documentationCenter=""
 authors="camsoper"
 manager="erikre"
 editor=""/>
<tags
 ms.service="cdn"
 ms.workload="media"
 ms.tgt_pltfrm="na"
 ms.devlang="multiple"
 ms.topic="article"
 ms.date="09/15/2016"
 ms.author="casoper"/>


# <a name="manage-expiration-of-azure-storage-blob-content-in-azure-cdn"></a>Zarządzanie wygasania Azure magazyn obiektów blob zawartości w sieci CDN Azure

> [AZURE.SELECTOR]
- [Usługi aplikacji chmura azure sieci Web, programu ASP.NET lub usług IIS](cdn-manage-expiration-of-cloud-service-content.md)
- [Azure magazyn obiektów blob usługi](cdn-manage-expiration-of-blob-content.md)

[Usługa obiektów blob](../storage/storage-introduction.md#blob-storage) w [Magazynie Azure](../storage/storage-introduction.md) jest jedną z kilku źródeł oparte na platformie Azure zintegrowany z Azure CDN.  Każda zawartość, publicznie obiektów blob mogą być buforowane w sieci CDN Azure, aż po jego time to live (TTL).  Wartość TTL jest określona przez [ *Cache-Control* nagłówka](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) w odpowiedzi HTTP z magazynu Azure.

>[AZURE.TIP] Można ustawić nie TTL obiektów blob.  W tym przypadku Azure CDN automatycznie stosuje domyślne TTL siedem dni.
>
>Aby uzyskać więcej informacji na temat współdziałania sieci CDN Azure aby przyspieszyć dostęp do obiektów blob i innych plików Zobacz [Omówienie CDN Azure](./cdn-overview.md).
>
>Aby uzyskać więcej informacji w usłudze Azure magazyn obiektów blob zobacz [Obiektów Blob usługi pojęcia](https://msdn.microsoft.com/library/dd179376.aspx). 

Ten samouczek przedstawiono kilka sposobów, które można ustawić wartość TTL obiektów blob w magazynie Azure.  

## <a name="azure-powershell"></a>Azure programu PowerShell

[Azure programu PowerShell](../powershell-install-configure.md) jest jednym ze sposobów najszybszą i najbardziej efektywny administrowania usługi Azure.  Używanie `Get-AzureStorageBlob` polecenia cmdlet, aby odwołać się do wartości blob, a następnie ustaw `.ICloudBlob.Properties.CacheControl` właściwości. 

```powershell
# Create a storage context
$context = New-AzureStorageContext -StorageAccountName "<storage account name>" -StorageAccountKey "<storage account key>"

# Get a reference to the blob
$blob = Get-AzureStorageBlob -Context $context -Container "<container name>" -Blob "<blob name>"

# Set the CacheControl property to expire in 1 hour (3600 seconds)
$blob.ICloudBlob.Properties.CacheControl = "public, max-age=3600"

# Send the update to the cloud
$blob.ICloudBlob.SetProperties()
```

>[AZURE.TIP] Za pomocą programu PowerShell do [zarządzania profilami CDN i punkty końcowe](./cdn-manage-powershell.md).

## <a name="azure-storage-client-library-for-net"></a>Biblioteka klienta Azure miejsca do magazynowania dla środowiska .NET

Aby ustawić TTL obiektów blob przy użyciu .NET, umożliwia ustawienie właściwości [CloudBlob.Properties.CacheControl](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.blobproperties.cachecontrol.aspx) [Azure Biblioteka klienta miejsca do magazynowania dla środowiska .NET](../storage/storage-dotnet-how-to-use-blobs.md) .

```csharp
class Program
{
    const string connectionString = "<storage connection string>";
    static void Main()
    {
        // Retrieve storage account information from connection string
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);
        
        // Create a blob client for interacting with the blob service.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
        
        // Create a reference to the container
        CloudBlobContainer container = blobClient.GetContainerReference("<container name>");

        // Create a reference to the blob
        CloudBlob blob = container.GetBlobReference("<blob name>");

        // Set the CacheControl property to expire in 1 hour (3600 seconds)
        blob.Properties.CacheControl = "public, max-age=3600";

        // Update the blob's properties in the cloud
        blob.SetProperties();
    }
}
```

>[AZURE.TIP] Istnieje wiele więcej przykłady kodu .NET dostępne w [Próbkach magazyn obiektów Blob Azure dla środowiska .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/).

## <a name="other-methods"></a>Inne metody

- [Azure interfejsu wiersza polecenia](../xplat-cli-install.md)

    Przekazując to zestaw przy użyciu właściwości *cacheControl* `-p` przełączanie.  W tym przykładzie ustawia wartość TTL godzinę (3600 sekund).

    ```text
    azure storage blob upload -c <connectionstring> -p cacheControl="public, max-age=3600" .\test.txt myContainer test.txt
    ```

- [Usługi Azure magazyn interfejsu API usługi REST](https://msdn.microsoft.com/library/azure/dd179355.aspx)

    Jawnie ustawione właściwości *x-ms-blob kontroli pamięci podręcznej* na żądanie [Umieszczenie Blob](https://msdn.microsoft.com/en-us/library/azure/dd179451.aspx), [Umieść listy zablokowanych](https://msdn.microsoft.com/en-us/library/azure/dd179467.aspx)lub [Właściwości zestawu obiektów Blob](https://msdn.microsoft.com/library/azure/ee691966.aspx) .

- Narzędzia do zarządzania innych firm miejsca do magazynowania

    Niektóre narzędzia do zarządzania magazyn Azure innych firm umożliwia ustawienie właściwości *CacheControl* na blob. 

## <a name="testing-the-cache-control-header"></a>Testowanie *Cache-Control* nagłówka

Można łatwo sprawdzić TTL blob usługi.  Za pomocą przeglądarki [narzędzi dla deweloperów](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/), przetestuj usługi obiektów blob łącznie z nagłówka odpowiedzi *Kontroli pamięci podręcznej* .  Za pomocą narzędzi, takich jak **wget**, [Postman](https://www.getpostman.com/)i [Fiddler](http://www.telerik.com/fiddler) przyjrzenie się nagłówki odpowiedzi.

## <a name="next-steps"></a>Następne kroki

- [Przeczytaj o *Cache-Control* nagłówka](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
- [Dowiedz się, jak zarządzać wygasania zawartości usługi w chmurze w sieci CDN Azure](./cdn-manage-expiration-of-cloud-service-content.md)

