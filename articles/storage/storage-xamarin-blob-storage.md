<properties
    pageTitle="Jak używać magazyn obiektów Blob z Xamarin | Microsoft Azure"
    description="Biblioteka Azure miejsca do magazynowania klienta do Xamarin umożliwia programistom tworzenie iOS, Android i aplikacje ze Sklepu Windows wraz z ich interfejsami użytkownika macierzystym. Ten samouczek pokazano, jak używać Xamarin do tworzenia aplikacji, która korzysta z magazynem obiektów Blob platformy Azure."
    services="storage"
    documentationCenter="xamarin"
    authors="micurd"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/08/2016"
    ms.author="micurd"/>

# <a name="how-to-use-blob-storage-from-xamarin"></a>Jak używać magazyn obiektów Blob z Xamarin

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

## <a name="overview"></a>Omówienie

Umożliwia Xamarin programistom za pomocą udostępnionego C# ścieżki bazowej kodu do tworzenia iOS, Android i aplikacje ze Sklepu Windows wraz z ich interfejsami użytkownika natywnych. Ten samouczek pokazano, jak magazyn obiektów Blob platformy Azure za pomocą aplikacji Xamarin. Jeśli chcesz dowiedzieć się więcej na temat przechowywania Azure, przed do nurkowania do kodu, zobacz [Wprowadzenie do magazyn Microsoft Azure](storage-introduction.md).

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.INCLUDE [storage-mobile-authentication-guidance](../../includes/storage-mobile-authentication-guidance.md)]

## <a name="create-a-new-xamarin-application"></a>Tworzenie nowej aplikacji Xamarin

Ten rozpoczynania pracy, możemy utworzyć aplikację, która jest przeznaczony dla systemu Android, iOS i systemu Windows. Ta aplikacja będzie po prostu Tworzenie kontenera i przekaż obiektów blob w tym kontenerze. Firma Microsoft obsługiwać za pomocą programu Visual Studio w systemie Windows to wprowadzenie, ale tym samym learnings mogą być stosowane podczas tworzenia aplikacji przy użyciu Xamarin Studio w systemie Mac OS.

Wykonaj poniższe czynności, aby utworzyć aplikację:

1. Jeśli jeszcze tego nie zrobiono, Pobierz i zainstaluj [Xamarin programu Visual Studio](https://www.xamarin.com/download).
2. Otwieranie programu Visual Studio i Tworzenie pustej aplikacji (udostępniony natywnych): **Plik > Nowy > projektu > między platformami > pusty App(Native Shared)**.
3. Kliknij prawym przyciskiem myszy rozwiązania w okienku Eksplorator rozwiązanie, a następnie wybierz pozycję **Zarządzanie pakietów NuGet rozwiązania**. Wyszukaj **WindowsAzure.Storage** i zainstalować najnowszą wersję stabilny dla wszystkich projektów w rozwiązaniu.
4. Tworzenie i uruchamianie projektu.

Teraz powinny mieć aplikacji, która umożliwia kliknij przycisk, który zwiększa licznik.

> [AZURE.NOTE] Biblioteka Azure miejsca do magazynowania klienta do Xamarin obsługuje obecnie poniższe typy projektów: natywne udostępniony, udostępnione Xamarin.Forms Xamarin.Android i Xamarin.iOS.

## <a name="create-container-and-upload-blob"></a>Tworzenie kontenera i przekazywanie obiektów blob

Następnie dodasz kodu do udostępnionego klasy `MyClass.cs` tworzy kontenera i wysyła obiektów blob w tym kontenerze. `MyClass.cs`powinna wyglądać następująco:

    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    using System.Threading.Tasks;

    namespace XamarinApp
    {
        public class MyClass
        {
            public MyClass ()
            {
            }

            public static async Task createContainerAndUpload()
            {
                // Retrieve storage account from connection string.
                CloudStorageAccount storageAccount = CloudStorageAccount.Parse("DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here");

                // Create the blob client.
                CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

                // Retrieve reference to a previously created container.
                CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

                // Create the container if it doesn't already exist.
                await container.CreateIfNotExistsAsync();

                // Retrieve reference to a blob named "myblob".
                CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

                // Create the "myblob" blob with the text "Hello, world!"
                await blockBlob.UploadTextAsync("Hello, world!");
            }
        }
    }

Upewnij się zamienić "your_account_name_here" i "your_account_key_here" Nazwa konta rzeczywiste i klucz konta. Możesz użyć tej klasy udostępnionego w systemie iOS, Android i Windows Phone aplikacji. Możesz po prostu dodać `MyClass.createContainerAndUpload()` do każdego projektu. Na przykład:

### <a name="xamarinappdroid--mainactivitycs"></a>XamarinApp.Droid > MainActivity.cs

    using Android.App;
    using Android.Widget;
    using Android.OS;

    namespace XamarinApp.Droid
    {
        [Activity (Label = "XamarinApp.Droid", MainLauncher = true, Icon = "@drawable/icon")]
        public class MainActivity : Activity
        {
            int count = 1;

            protected override async void OnCreate (Bundle bundle)
            {
                base.OnCreate (bundle);

                // Set our view from the "main" layout resource
                SetContentView (Resource.Layout.Main);

                // Get our button from the layout resource,
                // and attach an event to it
                Button button = FindViewById<Button> (Resource.Id.myButton);

                button.Click += delegate {
                    button.Text = string.Format ("{0} clicks!", count++);
                };

              await MyClass.createContainerAndUpload();
            }
        }
    }

### <a name="xamarinappios--viewcontrollercs"></a>XamarinApp.iOS > ViewController.cs

    using System;
    using UIKit;

    namespace XamarinApp.iOS
    {
        public partial class ViewController : UIViewController
        {
            int count = 1;

            public ViewController (IntPtr handle) : base (handle)
            {
            }

            public override async void ViewDidLoad ()
            {
                base.ViewDidLoad ();
                // Perform any additional setup after loading the view, typically from a nib.
                Button.AccessibilityIdentifier = "myButton";
                Button.TouchUpInside += delegate {
                    var title = string.Format ("{0} clicks!", count++);
                    Button.SetTitle (title, UIControlState.Normal);
                };

                await MyClass.createContainerAndUpload();
            }

            public override void DidReceiveMemoryWarning ()
            {
                base.DidReceiveMemoryWarning ();
                // Release any cached data, images, etc that aren't in use.
            }
        }
    }

### <a name="xamarinappwinphone--mainpagexaml--mainpagexamlcs"></a>XamarinApp.WinPhone > MainPage.xaml > MainPage.xaml.cs

    using Windows.UI.Xaml.Controls;
    using Windows.UI.Xaml.Navigation;

    // The Blank Page item template is documented at http://go.microsoft.com/fwlink/?LinkId=391641

    namespace XamarinApp.WinPhone
    {
        /// <summary>
        /// An empty page that can be used on its own or navigated to within a Frame.
        /// </summary>
        public sealed partial class MainPage : Page
        {
            int count = 1;

            public MainPage()
            {
                this.InitializeComponent();

                this.NavigationCacheMode = NavigationCacheMode.Required;
            }

            /// <summary>
            /// Invoked when this page is about to be displayed in a Frame.
            /// </summary>
            /// <param name="e">Event data that describes how this page was reached.
            /// This parameter is typically used to configure the page.</param>
            protected override async void OnNavigatedTo(NavigationEventArgs e)
            {
                // TODO: Prepare page for display here.

                // TODO: If your application contains multiple pages, ensure that you are
                // handling the hardware Back button by registering for the
                // Windows.Phone.UI.Input.HardwareButtons.BackPressed event.
                // If you are using the NavigationHelper provided by some templates,
                // this event is handled for you.
                Button.Click += delegate {
                    var title = string.Format("{0} clicks!", count++);
                    Button.Content = title;
                };

                await MyClass.createContainerAndUpload();
            }
        }
    }


## <a name="run-the-application"></a>Uruchamianie aplikacji

Teraz możesz uruchomić tej aplikacji w systemie Android lub Windows Phone emulatora. Możesz również uruchomić tę aplikację w emulatora iOS, ale wymaga komputera Mac. Aby uzyskać szczegółowe instrukcje dotyczące wykonywania tego zadania przeczytaj dokumentację [łączenia Visual Studio do komputera Mac](https://developer.xamarin.com/guides/ios/getting_started/installation/windows/connecting-to-mac/)

Po uruchomieniu aplikacji zostanie utworzony kontenerze `mycontainer` na Twoim koncie miejsca do magazynowania. Powinien zawierać obiektów blob, `myblob`, która zawiera tekst, `Hello, world!`. Można to sprawdzić, korzystając z [Eksploratora magazynu usługi Microsoft Azure](http://storageexplorer.com/).

## <a name="next-steps"></a>Następne kroki

W tym wprowadzenie przedstawiono sposób tworzenia aplikacji i platform w Xamarin, która używa magazynu Azure. Ten wprowadzenie specjalnie dotyczyła jeden scenariusz w magazynie obiektów Blob. Możesz jednak wykonać znacznie więcej z nie tylko magazyn obiektów Blob, ale również z tabeli, plików i magazyn kolejek. Sprawdź następujące artykuły, aby dowiedzieć się więcej:
- [Rozpoczynanie pracy z magazynem obiektów Blob platformy Azure za pomocą .NET](storage-dotnet-how-to-use-blobs.md)
- [Rozpoczynanie pracy z magazynem tabel platformy Azure za pomocą .NET](storage-dotnet-how-to-use-tables.md)
- [Wprowadzenie do magazynowania kolejki Azure za pomocą .NET](storage-dotnet-how-to-use-queues.md)
- [Wprowadzenie do przechowywania plików Azure w systemie Windows](storage-dotnet-how-to-use-files.md)

[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]
