<properties 
    pageTitle="Usługa Bus brokered wiadomości samouczek .NET | Microsoft Azure"
    description="Samouczek .NET brokered wiadomości."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/27/2016"
    ms.author="sethm" />

# <a name="service-bus-brokered-messaging-net-tutorial"></a>Usługa Bus brokered wiadomości samouczek .NET

Bus usługi Azure udostępnia dwa pełna rozwiązania do obsługi wiadomości — jeden za pośrednictwem usługi scentralizowane "przekazywania" działa w chmurze, która obsługuje wiele różne protokoły transportu i w sieci Web usługi standardy, takie jak SOAP, WS-* i Zatrzymaj. Klient nie jest konieczne bezpośredniego połączenia z usługą lokalnego ani nie jest konieczne wiesz, gdzie znajduje się usługa, a usługa lokalnego nie musi dowolne porty przychodzące otwarty na zaporze.

Druga rozwiązanie wiadomości udostępnia funkcje wiadomości "brokered". Te można traktować jako asynchroniczne lub odłączona wiadomości funkcje, które publikowania — subskrypcji pomocy technicznej, rozdzielenie czasowy i scenariuszy przy użyciu infrastruktury wiadomości Bus usługi równoważenia obciążenia. Rozłączone komunikacji ma wiele zalet; na przykład klienci i serwery można połączyć zgodnie z potrzebami i ich operacji w sposób asynchroniczny.

Ten samouczek ma na celu zapewniają zawiera omówienie i zdobycie doświadczenia praktycznego z kolejek, jedną podstawowe składniki usługi Bus brokered wiadomości. Po zakończeniu pracy za pomocą sekwencji tematów w tym samouczku masz aplikację, która wypełnia listę wiadomości, tworzy kolejkę i wysyła wiadomości do tej kolejki. Na koniec aplikacji odbiera wiadomości z kolejki są wyświetlane, a następnie wyczyści jego zasobów i wyjścia. Samouczek odpowiednich opisano sposób tworzenia aplikacji, która korzysta z usługi przekazywania Bus zobacz [Bus usługi przekazywane samouczek wiadomości](../service-bus-relay/service-bus-relay-tutorial.md).

## <a name="introduction-and-prerequisites"></a>Wprowadzenie i wymagania wstępne

Kolejki oferują pierwszego w dostarczenia wiadomości pierwszego się (FIFO) dla jednego lub więcej konkurencyjnych konsumentów. FIFO oznacza, że wiadomości są zwykle oczekiwać odebrana i przetwarzana przez odbiorców w kolejności czasowy, w którym były dodawane do kolejki, a każda wiadomość zostanie odebrana i przetwarzana przez tylko jedną wiadomość dla klientów indywidualnych. Główną zaletą korzystania z kolejek jest osiągnięcie *czasowy rozdzielenie* składników aplikacji: innymi słowy, producentów i konsumentów nie potrzebujesz do wysyłania i odbierania wiadomości w tym samym czasie, ponieważ wiadomości są trwale przechowywane w kolejce. Powiązane korzyści jest *Wyrównywanie obciążenia*, co umożliwia producentów i konsumentów wysyłać i odbierać wiadomości na różne stopy.

Poniżej przedstawiono kilka czynności administracyjne i wstępne, które należy wykonać przed rozpoczęciem przerabiania samouczka. Pierwszym jest tworzenie nazw usługi oraz w celu uzyskania klucza podpisu (SA) udostępniania. Przestrzeń nazw zawiera granic aplikacji dla każdej aplikacji dostępne za pośrednictwem usługi Bus. Klucz skojarzeń zabezpieczeń jest generowane automatycznie przez system podczas tworzenia nazw usługi. Kombinacja nazw usługi i klucza skojarzeń zabezpieczeń zawiera poświadczenie, z którym usługa Bus uwierzytelnia dostęp do aplikacji.

### <a name="create-a-service-namespace-and-obtain-a-sas-key"></a>Tworzenie nazw usługi i uzyskać klucz skojarzenia zabezpieczeń

Pierwszym krokiem jest utworzenie nazw usługi, aby uzyskać klucz [Udostępniony podpisu programu Access](service-bus-sas-overview.md) (SA). Przestrzeń nazw zawiera granic aplikacji dla każdej aplikacji dostępne za pośrednictwem usługi Bus. Klucz skojarzeń zabezpieczeń jest generowane automatycznie przez system podczas tworzenia nazw usługi. Połączenie usługi nazw i klucza skojarzeń zabezpieczeń zawiera poświadczenia dla usługi Bus do uwierzytelnienia dostęp do aplikacji.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

Następnym krokiem jest tworzenie projektu programu Visual Studio i pisanie dwóch funkcji Pomocnik ładowanie rozdzielany przecinkami listę wiadomości do obiektu wymagająca.NET [listy](https://msdn.microsoft.com/library/6sh2ey19.aspx) [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) .

### <a name="create-a-visual-studio-project"></a>Tworzenie projektu programu Visual Studio

1. Otwórz program Visual Studio jako administrator, klikając prawym przyciskiem myszy program w Start menu, a następnie klikając polecenie **Uruchom jako administrator**.

1. Utwórz nowy projekt aplikacji konsoli. Kliknij menu **plik** wybierz pozycję **Nowy**, a następnie kliknij pozycję **Projekt**. W oknie dialogowym **Nowy projekt** , kliknij przycisk **Visual C#** (Jeśli **Visual C#** nie jest wyświetlany, poszukaj w obszarze **Innych języków**), kliknij odpowiedni szablon **Aplikacji konsoli** i nadaj mu nazwę **QueueSample**. Użyj domyślnej **lokalizacji**. Kliknij przycisk **OK** , aby utworzyć projekt.

1. Dodawanie bibliotek usługi Bus do projektu za pomocą Menedżera pakietów NuGet:
    1. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projektu **QueueSample** , a następnie kliknij przycisk **Zarządzaj pakietów NuGet**.
    2. W oknie dialogowym **Zarządzanie pakietów Nuget** kliknij kartę **Przeglądaj** , wyszukaj **Bus usługi Azure**, a następnie kliknij przycisk **Zainstaluj**.
<br />
1. W oknie Eksplorator rozwiązań kliknij dwukrotnie plik Plik Program.cs, aby otworzyć go w Edytorze Visual Studio. Zmień nazwę przestrzeń nazw z jego domyślnej nazwy `QueueSample` do `Microsoft.ServiceBus.Samples`.

    ```
    Microsoft.ServiceBus.Samples
    {
        ...
    ```

1. Modyfikowanie `using` instrukcji, jak pokazano w poniższym kodzie.

    ```
    using System;
    using System.Collections.Generic;
    using System.Data;
    using System.IO;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.ServiceBus.Messaging;
    ```

1. Tworzenie pliku tekstowego o nazwie Data.csv i skopiowanie w następujący tekst rozdzielany przecinkami.

    ```
    IssueID,IssueTitle,CustomerID,CategoryID,SupportPackage,Priority,Severity,Resolved
    1,Package lost,1,1,Basic,5,1,FALSE
    2,Package damaged,1,1,Basic,5,1,FALSE
    3,Product defective,1,2,Premium,5,2,FALSE
    4,Product damaged,2,2,Premium,5,2,FALSE
    5,Package lost,2,2,Basic,5,2,TRUE
    6,Package lost,3,2,Basic,5,2,FALSE
    7,Package damaged,3,7,Premium,5,3,FALSE
    8,Product defective,3,2,Premium,5,3,FALSE
    9,Product damaged,4,6,Premium,5,3,TRUE
    10,Package lost,4,8,Basic,5,3,FALSE
    11,Package damaged,5,4,Basic,5,4,FALSE
    12,Product defective,5,4,Basic,5,4,FALSE
    13,Package lost,6,8,Basic,5,4,FALSE
    14,Package damaged,6,7,Premium,5,5,FALSE
    15,Product defective,6,2,Premium,5,5,FALSE
    ```

    Zapisz i zamknij plik Data.csv i pamiętaj lokalizacji, w której został zapisany.

1. W Eksploratorze rozwiązań, kliknij prawym przyciskiem myszy nazwę projektu (w tym przykładzie **QueueSample**), kliknij przycisk **Dodaj**, a następnie kliknij **Istniejący element**.

1. Przejdź do pliku Data.csv, który został utworzony w kroku 6. Kliknij plik, a następnie kliknij przycisk **Dodaj**. Upewnij się, że * *wszystkie pliki (*.*) ** jest zaznaczone na liście typów plików.

### <a name="create-a-method-that-parses-a-list-of-messages"></a>Utworzyć metodę analizowania listy wiadomości

1. W `Program` klasy przed `Main()` metoda zadeklarować dwóch zmiennych: jeden typ **DataTable**zawiera listę wiadomości w Data.csv. Drugi powinien być typu obiektu listy jednoznacznie określony do [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx). Te ostatnie jest lista brokered wiadomości, które będą używane kolejne kroki w samouczku.

    ```
    namespace Microsoft.ServiceBus.Samples
    {
        class Program
        {
    
            private static DataTable issues;
            private static List<BrokeredMessage> MessageList;
    ```

1. Poza `Main()`, definiowanie `ParseCSV()` metodę, która analizuje na liście wiadomości w Data.csv i załadowanie wiadomości do tabeli [DataTable](https://msdn.microsoft.com/library/azure/system.data.datatable.aspx) , jak pokazano na ilustracji. Metoda zwraca obiekt **DataTable** .

    ```
    static DataTable ParseCSVFile()
    {
        DataTable tableIssues = new DataTable("Issues");
        string path = @"..\..\data.csv";
        try
        {
            using (StreamReader readFile = new StreamReader(path))
            {
                string line;
                string[] row;
    
                // create the columns
                line = readFile.ReadLine();
                foreach (string columnTitle in line.Split(','))
                {
                    tableIssues.Columns.Add(columnTitle);
                }
    
                while ((line = readFile.ReadLine()) != null)
                {
                    row = line.Split(',');
                    tableIssues.Rows.Add(row);
                }
            }
        }
        catch (Exception e)
        {
            Console.WriteLine("Error:" + e.ToString());
        }
    
        return tableIssues;
    }
    ```

1. W `Main()` metody, Dodawanie instrukcji, która wymaga `ParseCSVFile()` metody:

    ```
    public static void Main(string[] args)
    {
    
        // Populate test data
        issues = ParseCSVFile();
    
    }
    ```

### <a name="create-a-method-that-loads-the-list-of-messages"></a>Tworzenie metodę ładowania na liście wiadomości

1. Poza `Main()`, definiowanie `GenerateMessages()` metodę, która ma obiekt **DataTable** zwrócony przez `ParseCSVFile()` i załadowanie tabeli do listy wymagająca brokered wiadomości. Następnie metoda zwróci obiekt **listy** , tak jak w poniższym przykładzie. 

    ```
    static List<BrokeredMessage> GenerateMessages(DataTable issues)
    {
        // Instantiate the brokered list object
        List<BrokeredMessage> result = new List<BrokeredMessage>();
    
        // Iterate through the table and create a brokered message for each row
        foreach (DataRow item in issues.Rows)
        {
            BrokeredMessage message = new BrokeredMessage();
            foreach (DataColumn property in issues.Columns)
            {
                message.Properties.Add(property.ColumnName, item[property]);
            }
            result.Add(message);
        }
        return result;
    }
    ```

1. W `Main()`, bezpośrednio po połączenie `ParseCSVFile()`, Dodawanie instrukcji, która wymaga `GenerateMessages()` metody wartość zwrócona przez `ParseCSVFile()` jako argument:

    ```
    public static void Main(string[] args)
    {
    
        // Populate test data
        issues = ParseCSVFile();
        MessageList = GenerateMessages(issues);
    }
    ```

### <a name="obtain-user-credentials"></a>Uzyskaj poświadczenia użytkownika

1. Najpierw utwórz trzy zmienne globalne ciąg do przechowywania tych wartości. Zadeklaruj zmienne bezpośrednio po poprzedniej deklaracji zmiennych; na przykład:

    ```
    namespace Microsoft.ServiceBus.Samples
    {
        public class Program
        {
    
            private static DataTable issues;
            private static List<BrokeredMessage> MessageList; 

            // Add these variables
            private static string ServiceNamespace;
            private static string sasKeyName = "RootManageSharedAccessKey";
            private static string sasKeyValue;
            …
    ```

1. Następnie należy utworzyć funkcję, która akceptuje i zapisuje obszar nazw usługi i klucza skojarzeń zabezpieczeń. Dodawanie tej metody można użyć poza `Main()`. Na przykład: 

    ```
    static void CollectUserInput()
    {
        // User service namespace
        Console.Write("Please enter the namespace to use: ");
        ServiceNamespace = Console.ReadLine();
    
        // Issuer key
        Console.Write("Enter the SAS key to use: ");
        sasKeyValue = Console.ReadLine();
    }
    ```

1. W `Main()`, bezpośrednio po połączenie `GenerateMessages()`, Dodawanie instrukcji, która wymaga `CollectUserInput()` metody:

    ```
    public static void Main(string[] args)
    {
    
        // Populate test data
        issues = ParseCSVFile();
        MessageList = GenerateMessages(issues);
        
        // Collect user input
        CollectUserInput();
    }
    ```

### <a name="build-the-solution"></a>Tworzenie rozwiązania

Z menu **Tworzenie** w programie Visual Studio można kliknij przycisk **Konstruuj rozwiązanie** lub naciśnij **Klawisze Ctrl + Shift + B** , aby potwierdzić dokładność pracy pory.

## <a name="create-management-credentials"></a>Tworzenie poświadczenia zarządzania

W tym kroku definiowania operacji zarządzania, używanego do tworzenia udostępniania poświadczeń podpisu (SA), z którymi będzie autoryzowanych aplikacji.

1. Ten samouczek jasności umieszcza wszystkie operacje kolejki w osobnym metody. Tworzenie asynchroniczna `Queue()` metoda `Program` klasy po `Main()` metody. Na przykład:
 
    ```
    public static void Main(string[] args)
    {
    …
    }
    static async Task Queue()
    {
    }
    ```

1. Następnym krokiem jest utworzenie skojarzeń zabezpieczeń poświadczenie za pomocą obiektu [Dostawca TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) . Sposób tworzenia pobiera nazwy klucza skojarzeń zabezpieczeń i uzyskać wartości w `CollectUserInput()` metody. Dodaj następujący kod do `Queue()` metody:

    ```
    static async Task Queue()
    {
        // Create management credentials
        TokenProvider credentials = TokenProvider.CreateSharedAccessSignatureTokenProvider(sasKeyName,sasKeyValue);
    }
    ```

2. Utwórz nowy obiekt zarządzania nazw, przy użyciu identyfikatora URI zawierającą nazwę nazw i poświadczenia zarządzania uzyskane w poprzednim kroku, jako argumenty. Dodaj ten kod bezpośrednio po kodzie dodane w poprzednim kroku. Upewnij się zamienić `<yourNamespace>` o nazwie obszaru nazw usługi:
    
    ```
    NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);
    ```

### <a name="example"></a>Przykład

W tym momencie kodu powinna wyglądać podobnie do następującej:

```
using System;
using System.Collections.Generic;
using System.Data;
using System.IO;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceBus.Messaging;

namespace Microsoft.ServiceBus.Samples
{
  public class Program
  {
    private static DataTable issues;
    private static List<BrokeredMessage> MessageList;
    private static string ServiceNamespace;
    private static string sasKeyName = "RootManageSharedAccessKey";
    private static string sasKeyValue;

    static void Main(string[] args)
    {
      // Populate test data
      issues = ParseCSVFile();
      MessageList = GenerateMessages(issues);

      // Collect user input
      CollectUserInput();

      // Add this call
      Task.WaitAll(Queue());
    }

    static async Task Queue()
    {
      // Create management credentials
      TokenProvider credentials = TokenProvider.CreateSharedAccessSignatureTokenProvider(sasKeyName, sasKeyValue);
      NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);
    }

    static DataTable ParseCSVFile()
    {
      DataTable tableIssues = new DataTable("Issues");
      string path = @"..\..\data.csv";
      try
      {
        using (StreamReader readFile = new StreamReader(path))
        {
          string line;
          string[] row;

          // create the columns
          line = readFile.ReadLine();
          foreach (string columnTitle in line.Split(','))
          {
            tableIssues.Columns.Add(columnTitle);
          }

          while ((line = readFile.ReadLine()) != null)
          {
            row = line.Split(',');
            tableIssues.Rows.Add(row);
          }
        }
      }
      catch (Exception e)
      {
        Console.WriteLine("Error:" + e.ToString());
      }

      return tableIssues;
    }

    static List<BrokeredMessage> GenerateMessages(DataTable issues)
    {
      // Instantiate the brokered list object
      List<BrokeredMessage> result = new List<BrokeredMessage>();

      // Iterate through the table and create a brokered message for each row
      foreach (DataRow item in issues.Rows)
      {
        BrokeredMessage message = new BrokeredMessage();
        foreach (DataColumn property in issues.Columns)
        {
          message.Properties.Add(property.ColumnName, item[property]);
        }
        result.Add(message);
      }
      return result;
    }

    static void CollectUserInput()
    {
      // User service namespace
      Console.Write("Please enter the service namespace to use: ");
      ServiceNamespace = Console.ReadLine();

      // Issuer key
      Console.Write("Please enter the issuer key to use: ");
      sasKeyValue = Console.ReadLine();
    }
  }
}
```

## <a name="send-messages-to-the-queue"></a>Wysyłanie wiadomości do kolejki

W tym kroku możesz utworzyć kolejkę, a następnie wysłać wiadomości, znajdujących się na liście brokered wiadomości do tej kolejki.

### <a name="create-queue-and-send-messages-to-the-queue"></a>Tworzenie kolejki i wysyłanie wiadomości do kolejki

1. Najpierw należy utworzyć kolejki. Na przykład, nadaj jej `myQueue`i deklarować go bezpośrednio po operacji zarządzania dodane w `Queue()` metody w ostatnim kroku:

    ```
    QueueDescription myQueue;

    if (namespaceClient.QueueExists("IssueTrackingQueue"))
    {
        namespaceClient.DeleteQueue("IssueTrackingQueue");
    }

    myQueue = namespaceClient.CreateQueue("IssueTrackingQueue");
    ```

1. W `Queue()` metody, tworzenie obiektu factory wiadomości przy użyciu nowo utworzone Bus identyfikator URI usługi jako argument. Dodaj następujący kod bezpośrednio po operacji zarządzania, które zostały dodane w ostatnim kroku. Upewnij się zamienić `<yourNamespace>` o nazwie obszaru nazw usługi:

    ```
    MessagingFactory factory = MessagingFactory.Create(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);
    ```

1. Następnie należy utworzyć obiekt kolejki za pomocą klasy [QueueClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx) . Dodaj następujący kod bezpośrednio po kodzie, które zostały dodane w ostatnim kroku:

    ```
    QueueClient myQueueClient = factory.CreateQueueClient("IssueTrackingQueue");
    ```

1. Następnie dodaj kod, który w pętli na liście wiadomości brokered, utworzonej wcześniej, wysyłanie każdego do kolejki. Dodaj następujący kod bezpośrednio po `CreateQueueClient()` instrukcji w poprzednim kroku:
    
    ```
    // Send messages
    Console.WriteLine("Now sending messages to the queue.");
    for (int count = 0; count < 6; count++)
    {
        var issue = MessageList[count];
        issue.Label = issue.Properties["IssueTitle"].ToString();
        await myQueueClient.SendAsync(issue);
        Console.WriteLine(string.Format("Message sent: {0}, {1}", issue.Label, issue.MessageId));
    }
    ```

## <a name="receive-messages-from-the-queue"></a>Odbieranie wiadomości z kolejki

W tym kroku można uzyskać na liście wiadomości z kolejki, który został utworzony w poprzednim kroku.

### <a name="create-a-receiver-and-receive-messages-from-the-queue"></a>Tworzenie odbiorcy i odbierać wiadomości z kolejki

W `Queue()` metoda obliczania, iteracji kolejki i odbierać wiadomości przy użyciu metody [QueueClient.ReceiveAsync](https://msdn.microsoft.com/library/azure/dn130423.aspx) , drukowanie każdej wiadomości do konsoli. Dodaj następujący kod bezpośrednio po kodzie dodanych w poprzednim kroku:

```
Console.WriteLine("Now receiving messages from Queue.");
BrokeredMessage message;
while ((message = await myQueueClient.ReceiveAsync(new TimeSpan(hours: 0, minutes: 1, seconds: 5))) != null)
    {
        Console.WriteLine(string.Format("Message received: {0}, {1}, {2}", message.SequenceNumber, message.Label, message.MessageId));
        message.Complete();
    
        Console.WriteLine("Processing message (sleeping...)");
        Thread.Sleep(1000);
    }
```

Należy zauważyć, że `Thread.Sleep` jest używane jedynie w celu zasymulowania wiadomość przetwarzania i prawdopodobnie nie trzeba go w aplikacji rzeczywistej wiadomości.

### <a name="end-the-queue-method-and-clean-up-resources"></a>Kończenie metody kolejki i oczyścić zasobów

Bezpośrednio po poprzedniej kodu Dodaj następujący kod, aby oczyścić zasoby factory i kolejki wiadomości:

```
factory.Close();
myQueueClient.Close();
namespaceClient.DeleteQueue("IssueTrackingQueue");
```

### <a name="call-the-queue-method"></a>Wywołaj metodę kolejki

Ostatnim krokiem jest dodanie instrukcji, która wymaga `Queue()` metody `Main()`. Dodaj następujący wiersz wyróżniony kodu na końcu Main():
    
```
public static void Main(string[] args)
{
    // Collect user input
    CollectUserInput();
    
    // Populate test data
    issues = ParseCSVFile();
    MessageList = GenerateMessages(issues);
    
    // Add this call
    Queue();
}
```

### <a name="example"></a>Przykład

Poniższy kod zawiera pełną aplikacji **QueueSample** .

```
using System;
using System.Collections.Generic;
using System.Data;
using System.IO;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceBus.Messaging;

namespace Microsoft.ServiceBus.Samples
{
    public class Program
    {
        private static DataTable issues;
        private static List<BrokeredMessage> MessageList;

        // Add these variables
        private static string ServiceNamespace;
        private static string sasKeyName = "RootManageSharedAccessKey";
        private static string sasKeyValue;

        static void Main(string[] args)
        {

            // Populate test data
            issues = ParseCSVFile();
            MessageList = GenerateMessages(issues);

            // Collect user input
            CollectUserInput();

            Queue();

        }

        static async Task Queue()
        {
            // Create management credentials
            TokenProvider credentials = TokenProvider.CreateSharedAccessSignatureTokenProvider(sasKeyName, sasKeyValue);
            NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);

            QueueDescription myQueue;

            if (namespaceClient.QueueExists("IssueTrackingQueue"))
            {
                namespaceClient.DeleteQueue("IssueTrackingQueue");
            }
            
            myQueue = namespaceClient.CreateQueue("IssueTrackingQueue");
            
            MessagingFactory factory = MessagingFactory.Create(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);

            QueueClient myQueueClient = factory.CreateQueueClient("IssueTrackingQueue");

            // Send messages
            Console.WriteLine("Now sending messages to the queue.");
            for (int count = 0; count < 6; count++)
            {
                var issue = MessageList[count];
                issue.Label = issue.Properties["IssueTitle"].ToString();
                await myQueueClient.SendAsync(issue);
                Console.WriteLine(string.Format("Message sent: {0}, {1}", issue.Label, issue.MessageId));
            }

            Console.WriteLine("Now receiving messages from Queue.");
            BrokeredMessage message;
            while ((message = await myQueueClient.ReceiveAsync(new TimeSpan(hours: 0, minutes: 1, seconds: 5))) != null)
            {
                Console.WriteLine(string.Format("Message received: {0}, {1}, {2}", message.SequenceNumber, message.Label, message.MessageId));
                message.Complete();

                Console.WriteLine("Processing message (sleeping...)");
                Thread.Sleep(1000);
            }

            factory.Close();
            myQueueClient.Close();
            namespaceClient.DeleteQueue("IssueTrackingQueue");


        }

        static void CollectUserInput()
        {
            // User service namespace
            Console.Write("Please enter the namespace to use: ");
            ServiceNamespace = Console.ReadLine();

            // Issuer key
            Console.Write("Enter the SAS key to use: ");
            sasKeyValue = Console.ReadLine();
        }

        static List<BrokeredMessage> GenerateMessages(DataTable issues)
        {
            // Instantiate the brokered list object
            List<BrokeredMessage> result = new List<BrokeredMessage>();

            // Iterate through the table and create a brokered message for each row
            foreach (DataRow item in issues.Rows)
            {
                BrokeredMessage message = new BrokeredMessage();
                foreach (DataColumn property in issues.Columns)
                {
                    message.Properties.Add(property.ColumnName, item[property]);
                }
                result.Add(message);
            }
            return result;
        }

        static DataTable ParseCSVFile()
        {
            DataTable tableIssues = new DataTable("Issues");
            string path = @"..\..\data.csv";
            try
            {
                using (StreamReader readFile = new StreamReader(path))
                {
                    string line;
                    string[] row;

                    // create the columns
                    line = readFile.ReadLine();
                    foreach (string columnTitle in line.Split(','))
                    {
                        tableIssues.Columns.Add(columnTitle);
                    }

                    while ((line = readFile.ReadLine()) != null)
                    {
                        row = line.Split(',');
                        tableIssues.Rows.Add(row);
                    }
                }
            }
            catch (Exception e)
            {
                Console.WriteLine("Error:" + e.ToString());
            }

            return tableIssues;
        }
    }
}
```

## <a name="build-and-run-the-queuesample-application"></a>Tworzenie i uruchamianie aplikacji QueueSample

Teraz, gdy zostały ukończone powyższych kroków, można utworzyć i uruchomić aplikację **QueueSample** .

### <a name="build-the-queuesample-application"></a>Tworzenie aplikacji QueueSample

W programie Visual Studio z menu **Tworzenie** kliknij przycisk **Konstruuj rozwiązanie**lub naciśnij **Klawisze Ctrl + Shift + B**. Jeśli wystąpią błędy, sprawdź, czy kod jest poprawny na podstawie wykonania przykładu przedstawione na końcu poprzedni krok.

## <a name="next-steps"></a>Następne kroki

Ten samouczek pokazano, jak tworzyć klienta usługi Bus, aplikacji i usług za pomocą usługi Bus brokered możliwości obsługi wiadomości. Samouczek podobne, która korzysta z usługi Bus [przekazywania](service-bus-messaging-overview.md#Relayed-messaging)zobacz [przekazywanie Bus usługi SMS samouczek](../service-bus-relay/service-bus-relay-tutorial.md).

Aby dowiedzieć się więcej na temat [Usługi Bus](https://azure.microsoft.com/services/service-bus/), zobacz następujące tematy.

- [Omówienie wiadomości Bus usługi](service-bus-messaging-overview.md)
- [Podstawy Bus usługi](service-bus-fundamentals-hybrid-solutions.md)
- [Architektura Bus usługi](service-bus-architecture.md)

