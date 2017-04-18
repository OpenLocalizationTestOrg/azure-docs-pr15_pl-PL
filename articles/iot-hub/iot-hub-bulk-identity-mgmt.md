<properties
 pageTitle="Importowanie/eksportowanie tożsamości urządzenia Centrum IoT | Microsoft Azure"
 description="Pojęcia i .NET kod wstawki zbiorcze zarządzania Centrum IoT urządzenia tożsamości"
 services="iot-hub"
 documentationCenter=".net"
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/05/2016"
 ms.author="dobett"/>

# <a name="bulk-management-of-iot-hub-device-identities"></a>Zbiorcze zarządzania Centrum IoT urządzenia tożsamości

Każdy IoT jest rejestru tożsamości urządzenia, które umożliwiają tworzenie na urządzenie zasobów w usługi, takiej jak kolejki zawierającej lotu wiadomości w chmurze do urządzenia. Rejestr tożsamości urządzenia umożliwia również kontrolowanie dostępu do punktów końcowych skierowaną urządzenia. W tym artykule opisano sposoby importowania i eksportowania tożsamości urządzenia zbiorczo do i z rejestru tożsamości urządzenia.

Importowanie i eksportowanie operacje miejsce w kontekście *zadania* , które umożliwiają wykonywanie operacji usługi zbiorcze przed koncentratora IoT.

Klasa **RegistryManager** zawiera **ExportDevicesAsync** i **ImportDevicesAsync** metody, które używają framework **zadania** . Te metody umożliwiają eksportowanie, importowanie i synchronizowanie całości rejestru IoT Centrum urządzenia.

## <a name="what-are-jobs"></a>Co to są zadań?

Operacje rejestru tożsamości urządzenia w systemie **zadania** po tej operacji:

*  Zawiera potencjalnie długi czas wykonywania w porównaniu z operacjami runtime standardowy, lub
*  Zwraca dużych ilości danych dla użytkownika.

W tych przypadkach, a nie pojedyncze wywołanie interfejsu API Oczekiwanie lub blokowanie w wyniku operacji operacja asynchroniczne tworzy **zadanie** koncentratora IoT. Następnie operacja natychmiast zwraca obiekt **JobProperties** .

Następujące wstawkę kodu C# pokazano, jak utworzyć zadanie eksportu:

```
// Call an export job on the IoT Hub to retrieve all devices
JobProperties exportJob = await registryManager.ExportDevicesAsync(containerSasUri, false);
```

Następnie klasy **RegistryManager** umożliwia zbadać stanu **zadań** przy użyciu zwrócone metadane **JobProperties** .

Poniższy fragment kodu C# przedstawia sposób ankieta co pięć sekund, aby sprawdzić, czy zadanie zostało zakończone wykonywania:

```
// Wait until job is finished
while(true)
{
  exportJob = await registryManager.GetJobAsync(exportJob.JobId);
  if (exportJob.Status == JobStatus.Completed || 
      exportJob.Status == JobStatus.Failed ||
      exportJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}
```

## <a name="export-devices"></a>Eksportowanie urządzeń

Użyj metody **ExportDevicesAsync** , aby wyeksportować całość rejestru IoT Centrum urządzenia do kontenera obiektów blob [Magazyn Azure](https://azure.microsoft.com/documentation/services/storage/) , korzystając z [Podpisu dostępu do udostępnionych](https://msdn.microsoft.com/library/ee395415.aspx).

Ta metoda pozwala na tworzenie niezawodne kopie zapasowe danych urządzenia w kontenerze obiektów blob, który można sterować.

Metoda **ExportDevicesAsync** wymaga dwoma parametrami:

*  *Ciąg* zawierający identyfikator URI kontenera obiektów blob. Ten identyfikator URI musi zawierać tokenu skojarzeń zabezpieczeń, który udziela uprawnienia do zapisu w kontenerze. To zadanie tworzy blob bloku w tym kontenerze do przechowywania seryjne Eksportuj dane urządzenia. Token skojarzeń zabezpieczeń obejmuje następujące uprawnienia:
    
    ```
    SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Delete
    ```

*  *Logiczną* wskazującą, czy chcesz wykluczyć kluczy uwierzytelniania z eksportowania danych. Jeśli **FAŁSZ**, uwierzytelniania klawiszy są uwzględniane w Eksportuj dane wyjściowe; w przeciwnym razie klawiszy są eksportowane jako **wartość null**.

Poniższy fragment kodu C# pokazano, jak zainicjować zadanie eksportu, zawierający kluczy uwierzytelniania urządzenia w Eksportuj dane, a następnie ankieta na ukończenie:

```
// Call an export job on the IoT Hub to retrieve all devices
JobProperties exportJob = await registryManager.ExportDevicesAsync(containerSasUri, false);

// Wait until job is finished
while(true)
{
    exportJob = await registryManager.GetJobAsync(exportJob.JobId);
    if (exportJob.Status == JobStatus.Completed || 
        exportJob.Status == JobStatus.Failed ||
        exportJob.Status == JobStatus.Cancelled)
    {
    // Job has finished executing
    break;
    }

    await Task.Delay(TimeSpan.FromSeconds(5));
}
```

Zadanie przechowuje jej wyniki w kontenerze obiektów blob dostarczonych jako obiektu blob blok z nazwy **devices.txt**. Dane wyjściowe składa się z danych urządzenia JSON seryjnych, jedno urządzenie na wiersz.

W poniższym przykładzie pokazano danych wyjściowych:

```
{"id":"Device1","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device2","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device3","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device4","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device5","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
```

Aby uzyskać dostęp do tych danych w kodzie, możesz łatwo deserializować te dane za pomocą klasy **ExportImportDevice** . Następujące wstawkę kodu C# pokazano, jak odczytywać informacje urządzenia, które wcześniej wyeksportowano blob blok:

```
var exportedDevices = new List<ExportImportDevice>();

using (var streamReader = new StreamReader(await blob.OpenReadAsync(AccessCondition.GenerateIfExistsCondition(), RequestOptions, null), Encoding.UTF8))
{
  while (streamReader.Peek() != -1)
  {
    string line = await streamReader.ReadLineAsync();
    var device = JsonConvert.DeserializeObject<ExportImportDevice>(line);
    exportedDevices.Add(device);
  }
}
```

> [AZURE.NOTE]  Za pomocą metody **GetDevicesAsync** klasy **RegistryManager** pobrać listę urządzeń. Jednak to podejście ma słabo zakończenie 1000 na liczbę obiektów urządzenia, które są zwracane. W przypadku przewidywanego używania metody **GetDevicesAsync** dotyczy scenariuszy projektowych do pomocy debugowania i nie jest zalecane dla obciążenia produkcji.

## <a name="import-devices"></a>Importowanie urządzeń

Metoda **ImportDevicesAsync** klasy **RegistryManager** umożliwia wykonywanie operacji importu i synchronizacji zbiorczych w rejestrze IoT Centrum urządzenia. Podobnie jak metody **ExportDevicesAsync** metody **ImportDevicesAsync** stosowane w ramach **zadania** .

Zajmować się przy użyciu metody **ImportDevicesAsync** , ponieważ oprócz inicjowania obsługi administracyjnej nowych urządzeń w rejestrze tożsamości urządzenia, można również aktualizowanie i usuwanie istniejących urządzeniach.

> [AZURE.WARNING]  Nie można cofnąć operację importowania. Zawsze tworzyć kopie zapasowe istniejących danych przy użyciu metody **ExportDevicesAsync** do drugiego kontenera obiektów blob przed wprowadzeniem zmian zbiorczych do rejestru tożsamości urządzenia.

Metoda **ImportDevicesAsync** przyjmuje dwa parametry:

*  *Ciąg* zawierający identyfikatora URI kontenera obiektów blob [Platformy Azure magazynu](https://azure.microsoft.com/documentation/services/storage/) ma zostać użyte jako *wprowadzania danych* do zadania. Ten identyfikator URI musi zawierać token skojarzeń zabezpieczeń, zapewniającej dostęp do odczytu w kontenerze. Ten kontener musi zawierać obiektów blob z nazwy **devices.txt** , zawierający dane seryjne urządzenia zaimportować do rejestru tożsamości urządzenia. Importowanie danych musi zawierać informacje o urządzeniach w korzystającego z zadania **ExportImportDevice** podczas tworzenia obiektów blob **devices.txt** formacie JSON. Token skojarzeń zabezpieczeń obejmuje następujące uprawnienia:

    ```
    SharedAccessBlobPermissions.Read
    ```

*  *Ciąg* zawierający identyfikator URI kontenera obiektów blob [Magazyn Azure](https://azure.microsoft.com/documentation/services/storage/) umożliwia *Wyjście* z zadania. To zadanie tworzy blob bloku w tym kontenerze przechowywać informacje o błędzie z importu ukończonego **zadania**. Token skojarzeń zabezpieczeń obejmuje następujące uprawnienia:
    
    ```
    SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Delete
    ```

> [AZURE.NOTE]  Dwoma parametrami można wskazać na tym samym kontenerze obiektów blob. Parametry osobnych po prostu włączyć większą kontrolę nad danych kontenerze dane wyjściowe wymaga dodatkowych uprawnień.

Poniższy fragment kodu C# przedstawia sposób inicjowania zadania importu:

```
JobProperties importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);
```

## <a name="import-behavior"></a>Procesu importowania

Metoda **ImportDevicesAsync** pozwala wykonywać następujące operacje zbiorcze w rejestrze tożsamości urządzenia:

-   Zbiorcze rejestracji nowych urządzeń
-   Zbiorcze usuwanie istniejących urządzeń
-   Zbiorcze zmiany statusu (Włączanie lub wyłączanie urządzenia)
-   Zbiorcze Przypisywanie nowych kluczy uwierzytelniania urządzenia
-   Zbiorcze automatycznego ponownego wygenerowania kluczy uwierzytelniania urządzenia

Można wykonać dowolną kombinację poprzedniej operacji w ramach jednego połączenia **ImportDevicesAsync** . Można na przykład Zarejestruj nowe urządzenia i usuń lub zaktualizuj istniejących urządzeń w tym samym czasie. Stosowany razem z metody **ExportDevicesAsync** , możesz całkowicie migrację wszystkich swoich urządzeniach z jednym centrum IoT do innej.

Właściwość opcjonalne **parametrem importMode** w oknie importowanie danych szeregowania dla każdego urządzenia do sterowania importowanie proces na urządzenie. Właściwość **parametrem importMode** dostępne są następujące opcje:

| parametrem importMode |  Opis |
| -------- | ----------- |
| **createOrUpdate** | Jeśli urządzenie nie istnieje z określonym **identyfikator**, nowo jest zarejestrowany. <br/>Jeśli urządzenie już istnieje, istniejących informacji jest zastępowany dostarczonych danych wejściowych, bez względu na to wartość **ETag** . |
| **Tworzenie** | Jeśli urządzenie nie istnieje z określonym **identyfikator**, nowo jest zarejestrowany. <br/>Jeśli urządzenie już istnieje, komunikat o błędzie są zapisywane w pliku dziennika. |
| **Aktualizowanie** | Jeśli urządzenie jest już istnieje z określonym **identyfikator**, istniejących informacji jest zastępowany dostarczonych danych wejściowych, bez względu na to wartość **ETag** . <br/>Jeśli urządzenie nie istnieje, komunikat o błędzie są zapisywane w pliku dziennika. |
| **updateIfMatchETag** | Jeśli urządzenie jest już istnieje z określonym **identyfikator**, istniejących informacji jest zastępowany dostarczonych danych wejściowych tylko wtedy, gdy istnieje dopasowanie **ETag** . <br/>Jeśli urządzenie nie istnieje, komunikat o błędzie są zapisywane w pliku dziennika. <br/>W przypadku niezgodności **ETag** błędu są zapisywane w pliku dziennika. |
| **createOrUpdateIfMatchETag** | Jeśli urządzenie nie istnieje z określonym **identyfikator**, nowo jest zarejestrowany. <br/>Jeśli urządzenie już istnieje, istniejących informacji jest zastępowany dostarczonych danych wejściowych tylko wtedy, gdy istnieje dopasowanie **ETag** . <br/>W przypadku niezgodności **ETag** błędu są zapisywane w pliku dziennika. |
| **Usuwanie** | Jeśli urządzenie jest już istnieje z określonym **identyfikator**, zostanie usunięty bez względu na to wartość **ETag** . <br/>Jeśli urządzenie nie istnieje, komunikat o błędzie są zapisywane w pliku dziennika. |
| **deleteIfMatchETag** | Jeśli urządzenie jest już istnieje z określonym **identyfikator**, powoduje usunięcie go tylko wtedy, gdy istnieje dopasowanie **ETag** . Jeśli urządzenie nie istnieje, komunikat o błędzie są zapisywane w pliku dziennika. <br/>W przypadku niezgodności ETag błędu są zapisywane w pliku dziennika. |

> [AZURE.NOTE] Jeśli dane szeregowania jawnie definiuje flagi **parametrem importMode** dla urządzenia, domyślnie **createOrUpdate** podczas operacji importu.

## <a name="import-devices-example--bulk-device-provisioning"></a>Importowanie przykład urządzeń — zbiorczo urządzenia inicjowania obsługi administracyjnej. 

Poniższy przykład kodu C# pokazano, jak wygenerować wiele tożsamości urządzenia które:

- Zawierają klucze uwierzytelniania.
- Zapisać te informacje w urządzeniu blob blok.
- Importowanie urządzenia do tożsamości rejestru urządzenia.

```
// Provision 1,000 more devices
var serializedDevices = new List<string>();

for (var i = 0; i < 1000; i++)
{
// Create a new ExportImportDevice
  var deviceToAdd = new ExportImportDevice()
  {
    Id = Guid.NewGuid().ToString(),
    Status = DeviceStatus.Enabled,
    Authentication = new AuthenticationMechanism()
    {
      SymmetricKey = new SymmetricKey()
      {
        PrimaryKey = CryptoKeyGenerator.GenerateKey(32),
        SecondaryKey = CryptoKeyGenerator.GenerateKey(32)
      }
    },
    ImportMode = ImportMode.Create
  };

  // Add device to existing list
  serializedDevices.Add(JsonConvert.SerializeObject(deviceToAdd));
}

// Write this list to the blob
var sb = new StringBuilder();
serializedDevices.ForEach(serializedDevice => sb.AppendLine(serializedDevice));
await blob.DeleteIfExistsAsync();

using (CloudBlobStream stream = await blob.OpenWriteAsync())
{
  byte[] bytes = Encoding.UTF8.GetBytes(sb.ToString());
  for (var i = 0; i < bytes.Length; i += 500)
  {
    int length = Math.Min(bytes.Length - i, 500);
    await stream.WriteAsync(bytes, i, length);
  }
}

// Call import using the same blob to add new devices!
// This normally takes 1 minute per 100 devices the normal way
JobProperties importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);

// Wait until job is finished
while(true)
{
  importJob = await registryManager.GetJobAsync(importJob.JobId);
  if (importJob.Status == JobStatus.Completed || 
      importJob.Status == JobStatus.Failed ||
      importJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}
```

## <a name="import-devices-example--bulk-deletion"></a>Importowanie przykład urządzeń — usuwanie zbiorcze

Poniższy przykład kodu pokazano, jak usunąć urządzenia dodane za pomocą w poprzednim przykładzie kodu:

```
// Step 1: Update each device's ImportMode to be Delete
sb = new StringBuilder();
serializedDevices.ForEach(serializedDevice =>
{
  // Deserialize back to an ExportImportDevice
  var device = JsonConvert.DeserializeObject<ExportImportDevice>(serializedDevice);

  // Update property
  device.ImportMode = ImportMode.Delete;

  // Re-serialize
  sb.AppendLine(JsonConvert.SerializeObject(device));
});

// Step 2: Write the new import data back to the block blob
await blob.DeleteIfExistsAsync();
using (CloudBlobStream stream = await blob.OpenWriteAsync())
{
  byte[] bytes = Encoding.UTF8.GetBytes(sb.ToString());
  for (var i = 0; i < bytes.Length; i += 500)
  {
    int length = Math.Min(bytes.Length - i, 500);
    await stream.WriteAsync(bytes, i, length);
  }
}

// Step 3: Call import using the same blob to delete all devices!
importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);

// Wait until job is finished
while(true)
{
  importJob = await registryManager.GetJobAsync(importJob.JobId);
  if (importJob.Status == JobStatus.Completed || 
      importJob.Status == JobStatus.Failed ||
      importJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}

```

## <a name="getting-the-container-sas-uri"></a>Wprowadzenie kontenerze URI skojarzenia zabezpieczeń


Poniższy przykład kodu pokazano, jak wygenerować identyfikatora [URI skojarzenia zabezpieczeń](../storage/storage-dotnet-shared-access-signature-part-2.md) z Odczyt, zapis i Usuń uprawnienia dla kontenera obiektów blob:

```
static string GetContainerSasUri(CloudBlobContainer container)
{
  // Set the expiry time and permissions for the container.
  // In this case no start time is specified, so the
  // shared access signature becomes valid immediately.
  var sasConstraints = new SharedAccessBlobPolicy();
  sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
  sasConstraints.Permissions = 
    SharedAccessBlobPermissions.Write | 
    SharedAccessBlobPermissions.Read | 
    SharedAccessBlobPermissions.Delete;

  // Generate the shared access signature on the container,
  // setting the constraints directly on the signature.
  string sasContainerToken = container.GetSharedAccessSignature(sasConstraints);

  // Return the URI string for the container,
  // including the SAS token.
  return container.Uri + sasContainerToken;
}

```

## <a name="next-steps"></a>Następne kroki

W tym artykule przedstawiono sposób wykonywania operacji zbiorczej przed rejestru tożsamości urządzenia w Centrum IoT. Skorzystaj z poniższych łączy, aby dowiedzieć się więcej o zarządzaniu Centrum IoT Azure:

- [Metryki zastosowania][lnk-metrics]
- [Operacje monitorowania][lnk-monitor]

Aby dodatkowo Poznawanie możliwości Centrum IoT, zobacz:

- [Przewodnik dewelopera][lnk-devguide]
- [Symulowanie urządzenia z IoT SDK bramy][lnk-gateway]

[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md