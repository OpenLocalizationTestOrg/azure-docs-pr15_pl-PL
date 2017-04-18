<properties
    pageTitle="Przekazywanie plików z urządzeń za pomocą Centrum IoT | Microsoft Azure"
    description="Skorzystać z tego samouczka, aby dowiedzieć się, jak przekazywać pliki na urządzeniach za pomocą Centrum IoT Azure C#."
    services="iot-hub"
    documentationCenter=".net"
    authors="fsautomata"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="dotnet"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="06/21/2016"
     ms.author="elioda"/>

# <a name="tutorial-how-to-upload-files-from-devices-to-the-cloud-with-iot-hub"></a>Samouczek: Jak przekazywać pliki z urządzeń z chmurą przy użyciu Centrum IoT

## <a name="introduction"></a>Wprowadzenie

Azure koncentratora IoT jest w pełni zarządzane usługa, która pozwala zaufanego i bezpieczna komunikacja dwukierunkowa między miliony IoT urządzeń i aplikacji wewnętrzna baza danych. Samouczki poprzedniego ([Wprowadzenie do Centrum IoT] i [Wysyłanie wiadomości chmury do urządzenia z Centrum IoT]) przedstawić podstawowy urządzenia w chmurze i chmury do urządzenia z obsługą wiadomości koncentratora IoT i samouczek [proces urządzenia w chmurze wiadomości] w tym artykule opisano sposób wiarygodny sposób przechowywania wiadomości urządzenia w chmurze w magazynie obiektów blob platformy Azure. Jednak w niektórych scenariuszach nie można łatwo mapowania danych wysyłanych do niewielkich wiadomości urządzenia w chmurze, które akceptuje Centrum IoT urządzenia. Jako przykład można wymienić dużych plików, która zawiera obrazy, klipy wideo, dane wibracje pobrane w wysokiej częstotliwości lub zawierają niektóre formularza wstępnie przetworzony format danych. Są to zazwyczaj partii przetwarzania w chmurze za pomocą narzędzi, takich jak [Fabryki danych Azure] lub stos [Hadoop] . Podczas przekazywania plików z urządzenia są preferowane wysyłanie zdarzenia, nadal możesz używać funkcji IoT Centrum zabezpieczeń i niezawodności.

Ten samouczek opiera się na kod w [wiadomości chmury do urządzenia z Centrum IoT] samouczka, aby pokazano, jak skorzystać z funkcji przekazywania pliku IoT Centrum. Przedstawiono sposób bezpieczne zapewnienie urządzeniu Azure blob identyfikatora URI dla przekazywanie pliku i jak używać powiadomienia o przekazywania plików Centrum IoT wyzwalać przetwarzania pliku w swojej aplikacji wewnętrznej.

Na końcu tego samouczka uruchomieniu dwóch aplikacji konsoli systemu Windows:

* **SimulatedDevice**, zmodyfikowaną wersję aplikacji utworzony w [chmurze do urządzenia wiadomości z Centrum IoT] samouczek, której przekazywania pliku do magazynu przy użyciu identyfikatora URI skojarzeń zabezpieczeń dostępne w Twoim Centrum IoT.
* **ReadFileUploadNotification**, który otrzymuje powiadomienia o przekazywania plików z Twoim Centrum IoT.

> [AZURE.NOTE] Centrum IoT obsługuje wiele platformy urządzeń i języków (w tym C, Java i Javascript) za pomocą urządzenia Azure IoT SDK. Zapoznaj się z [Centrum deweloperów IoT Azure] , aby uzyskać instrukcje krok po kroku dotyczące nawiązywania połączenia między urządzeniem kod przedstawiona w tym samouczku oraz ogólnie Centrum IoT Azure.

Aby można było użyć tego samouczka, są potrzebne następujące elementy:

+ Program Microsoft Visual Studio 2015 r.,

+ Konto Azure active. (Jeśli nie masz konta, możesz utworzyć [bezpłatne konto] [ lnk-free-trial] na kilka minut.)

## <a name="associate-an-azure-storage-account-to-iot-hub"></a>Kojarzenie konta magazynu platformy Azure do koncentratora IoT

Ponieważ urządzenia symulowany przekazywania pliku do obiektów blob, muszą mieć konta [Magazynu platformy Azure] związanych z Centrum IoT. Kojarzenie konta magazynu platformy Azure z koncentratora IoT, Centrum może wygenerować identyfikatora URI skojarzeń zabezpieczeń, który umożliwia bezpieczne przekazywanie pliku do kontenera obiektów blob urządzenia. Usługa Centrum IoT i SDK urządzenia będą dopasowane proces, który generuje identyfikator URI skojarzeń zabezpieczeń i udostępnia na urządzeniu, aby przekazać plik za pomocą.

Postępuj zgodnie z instrukcjami [Wysyłanie plików konfigurowanie za pomocą portalu Azure] [ lnk-configure-upload] skojarzyć konta magazynu platformy Azure do Twoim Centrum IoT.

## <a name="upload-a-file-from-a-simulated-device"></a>Przekazywanie pliku z urządzeniem symulowany

W tej sekcji można zmodyfikować aplikację symulacji utworzone w [chmurze do urządzenia wiadomości z Centrum IoT] otrzymywać wiadomości w chmurze do urządzenia z poziomu Centrum IoT.

1. W programie Visual Studio kliknij prawym przyciskiem myszy **SimulatedDevice** projektu, kliknij przycisk **Dodaj**, a następnie kliknij **Istniejący element**. Przejdź do pliku obrazu, a następnie dołączyć go do projektu. Tego samouczka przyjęto założenie nosi nazwę obrazu `image.jpg`.

2. Kliknij prawym przyciskiem myszy obraz, a następnie kliknij polecenie **Właściwości**. Upewnij się, że **skopiować do katalogu wyjściowego** ustawiono **zawsze Kopiuj**.

    ![][1]

3. W pliku **Plik Program.cs** Dodaj następujące instrukcje u góry pliku:

        using System.IO;

4. Dodaj następujące metody klasy **programu** :
         
        private static async void SendToBlobAsync()
        {
            string fileName = "image.jpg";
            Console.WriteLine("Uploading file: {0}", fileName);
            var watch = System.Diagnostics.Stopwatch.StartNew();

            using (var sourceData = new FileStream(@"image.jpg", FileMode.Open))
            {
                await deviceClient.UploadToBlobAsync(fileName, sourceData);
            }

            watch.Stop();
            Console.WriteLine("Time to upload file: {0}ms\n", watch.ElapsedMilliseconds);
        }

    `UploadToBlobAsync` Metoda pobiera plik źródłowy nazwę i strumienia pliku do przekazania i obsługuje przekazywania miejsca do magazynowania. Aplikacja konsoli Wyświetla czas potrzebny na Przekaż plik.

5. Dodaj następujące metody w metodzie **główne** tuż przed `Console.ReadLine()` wiersza:

        SendToBlobAsync();

> [AZURE.NOTE] Sake i uproszczenia tego samouczka nie zaimplementować wszelkie zasady ponów próbę. W kodzie produkcyjnym wykonała zasady ponów próbę (na przykład wykładniczego wycofywania), sugerowanej w artykule w witrynie MSDN [Przejściowych obsługi błędów].

## <a name="receive-a-file-upload-notification"></a>Powiadomienie przekazywania plików

W tej sekcji możesz zapisać aplikację konsoli systemu Windows, która odbiera wiadomości powiadomień przekazywania plików z Centrum IoT.

1. W bieżącym rozwiązaniu Visual Studio należy utworzyć nowy projekt Visual C# systemu Windows za pomocą **Aplikacji konsoli** szablon projektu. Nazwa projektu **ReadFileUploadNotification**.

    ![Nowy projekt w programie Visual Studio][2]

2. W oknie Eksplorator rozwiązań projektu **ReadFileUploadNotification** kliknij prawym przyciskiem myszy, a następnie kliknij **Zarządzanie pakietów NuGet**.

    Spowoduje to wyświetlenie okna Zarządzanie pakietów NuGet.

2. Wyszukiwanie `Microsoft.Azure.Devices`, kliknij przycisk **Zainstaluj**i zaakceptuj warunki użytkowania. 

    Pliki do pobrania, instaluje i dodaje odwołanie do [IoT Azure - pakiet NuGet SDK usługi] w programie project **ReadFileUploadNotification** .

3. W pliku **Plik Program.cs** Dodaj następujące instrukcje u góry pliku:

        using Microsoft.Azure.Devices;

4. Dodawanie pola do klasy **programu** . Podstawiania wartości symbolu zastępczego przy użyciu parametrów połączenia Centrum IoT z [Wprowadzenie do Centrum IoT]:

        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";
        
5. Dodaj następujące metody klasy **programu** :
   
        private async static Task ReceiveFileUploadNotificationAsync()
        {
            var notificationReceiver = serviceClient.GetFileNotificationReceiver();

            Console.WriteLine("\nReceiving file upload notification from service");
            while (true)
            {
                var fileUploadNotification = await notificationReceiver.ReceiveAsync();
                if (fileUploadNotification == null) continue;

                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received file upload noticiation: {0}", string.Join(", ", fileUploadNotification.BlobName));
                Console.ResetColor();

                await notificationReceiver.CompleteAsync(fileUploadNotification);
            }
        }

    Należy zauważyć, że wzór Odbierz ten sam używany do odbierania wiadomości w chmurze do urządzenia z aplikacji urządzenia.

6. Na koniec dodać kolejne wiersze do metody **główne** :

        Console.WriteLine("Receive file upload notifications\n");
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
        ReceiveFileUploadNotificationAsync().Wait();
        Console.ReadLine();

## <a name="run-the-applications"></a>Uruchamianie aplikacji

Teraz możesz przystąpić do uruchamiania aplikacji.

1. W programie Visual Studio kliknij prawym przyciskiem myszy rozwiązanie, a następnie wybierz **uruchamiania zestawu projektów**. Zaznacz **wiele projektów uruchamiania**, a następnie wybierz akcję **rozpocząć** **ReadFileUploadNotification** i **SimulatedDevice**.

2. Naciśnij klawisz **F5**. Nazwa powinna rozpoczynać się obie aplikacje. Powinien zostać wyświetlony przekazywanie wykonane w jednej aplikacji konsoli i Przekaż wiadomość z powiadomieniem odbierane przez aplikację konsoli. [Azure portal] lub program Visual Studio Server Explorer służy do sprawdzenia obecności przekazanego pliku na swoim koncie magazyn Azure.

  ![][50]


## <a name="next-steps"></a>Następne kroki

W tym samouczku wiesz, jak wykorzystać możliwości przekazania pliku Centrum IoT w celu uproszczenia przekazywania plików z urządzeń. Możesz kontynuować Poznaj funkcje Centrum IoT i scenariusze z następujących artykułów:

- [Programistyczne tworzenie Centrum IoT][lnk-create-hub]
- [Wprowadzenie do C SDK][lnk-c-sdk]
- [Centrum IoT SDK][lnk-sdks]

Aby jeszcze bardziej Poznawanie możliwości Centrum IoT, zobacz:

- [Symulowanie urządzenia z IoT SDK bramy][lnk-gateway]

<!-- Images. -->

[50]: ./media/iot-hub-csharp-csharp-file-upload/run-apps1.png
[1]: ./media/iot-hub-csharp-csharp-file-upload/image-properties.png
[2]: ./media/iot-hub-csharp-csharp-file-upload/create-identity-csharp1.png

<!-- Links -->

[Azure portal]: https://portal.azure.com/

[Factory Azure danych]: https://azure.microsoft.com/documentation/services/data-factory/
[Hadoop]: https://azure.microsoft.com/documentation/services/hdinsight/

[Wysyłanie wiadomości chmury do urządzenia z Centrum IoT]: iot-hub-csharp-csharp-c2d.md
[Przetwarzanie wiadomości urządzenia w chmurze]: iot-hub-csharp-csharp-process-d2c.md
[Wprowadzenie do Centrum IoT]: iot-hub-csharp-csharp-getstarted.md
[Centrum deweloperów IoT Azure]: http://www.azure.com/develop/iot

[Obsługa przejściowych błędów]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure miejsca do magazynowania]: ../storage/storage-create-storage-account.md#create-a-storage-account
[lnk-configure-upload]: iot-hub-configure-file-upload.md
[Azure IoT - pakiet NuGet SDK usługi]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md


