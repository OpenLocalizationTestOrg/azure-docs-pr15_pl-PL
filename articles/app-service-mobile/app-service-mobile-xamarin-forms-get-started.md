<properties
    pageTitle="Wprowadzenie do aplikacji Mobile przy użyciu Xamarin.Forms"
    description="Skorzystać z tego samouczka, aby rozpocząć korzystanie z aplikacji Mobile Azure rozwoju Xamarin.Forms"
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

#<a name="create-a-xamarinforms-app"></a>Tworzenie aplikacji Xamarin.Forms

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##<a name="overview"></a>Omówienie

Ten samouczek pokazano, jak dodać usługi opartej na chmurze wewnętrznej bazy danych do Xamarin.Forms aplikacji dla urządzeń przenośnych przy użyciu aplikacji Mobile Azure wewnętrznej bazy danych. Możesz utworzyć nowej aplikacji Mobile wewnętrznej bazie danych jak prosta _Lista zadania_ aplikacji Xamarin.Forms, w którym są przechowywane dane aplikacji platformy Azure.

Ten samouczek jest wymagane w przypadku wszystkich innych samouczków aplikacji Mobile dla Xamarin.Forms.

##<a name="prerequisites"></a>Wymagania wstępne

Aby użyć tego samouczka, są potrzebne następujące elementy:

* Konto Azure active. Jeśli nie masz konta, możesz utworzyć konto Azure wersji próbnej i uzyskać maksymalnie 10 bezpłatne aplikacje Mobile, który możesz korzystać nawet po zakończeniu okresu próbnego. Aby uzyskać szczegółowe informacje zobacz [Azure bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/).

* Program Visual Studio z Xamarin. Aby uzyskać instrukcje, zobacz [Instalacja i zainstaluj program Visual Studio i Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) . 

* Mac z Xcode 7.0 lub nowszy i Xamarin Studio społeczności zainstalowany. Zobacz [Instalacja i zainstaluj program Visual Studio i Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) i [konfiguracji, instalowanie i weryfikacji użytkownicy komputerów Mac](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN).
 
>[AZURE.NOTE] Jeśli chcesz rozpocząć pracę z Azure aplikacji usługi przed utworzeniem konta dla konta Azure, przejdź do [Spróbuj aplikacji usługi](https://tryappservice.azure.com/?appServiceName=mobile), której natychmiast można utworzyć krótkotrwałe początkowego aplikacji Mobile w aplikacji usługi. Nie kart kredytowych wymagane; nie zobowiązania.

## <a name="create-a-new-azure-mobile-app-backend"></a>Tworzenie nowej aplikacji Mobile Azure wewnętrznej bazie danych

Wykonaj poniższe czynności w celu utworzenia nowej aplikacji Mobile wewnętrznej bazie danych.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]


Teraz masz obsługi administracyjnej wewnętrznej bazy danych aplikacji Mobile Azure, który może być używany przez aplikacje klientów przenośnych. Następnie pobierze projektu server do prostej "list zadania" wewnętrznej bazy danych w celu opublikowania go w Azure.

## <a name="configure-the-server-project"></a>Konfigurowanie programu project server

Wykonaj poniższe czynności, aby skonfigurować project server umożliwia Node.js albo .NET wewnętrznej bazy danych.

[AZURE.INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

##<a name="download-and-run-the-xamarinforms-solution"></a>Pobierz i uruchom rozwiązanie Xamarin.Forms

W tym miejscu masz kilka opcji. Możesz pobrać rozwiązanie do komputera Mac i otwórz go w Xamarin Studio lub można pobrać rozwiązanie na komputerze z systemem Windows i otwórz go w programie Visual Studio przy użyciu Mac podłączonym do tworzenia aplikacji w systemie iOS. Jeśli potrzebujesz bardziej szczegółowe instrukcje dotyczące scenariuszy instalacji Xamarin, zobacz [Instalacja i zainstaluj program Visual Studio i Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) .

Wykonaj poniższe możliwości:

 1. Na komputerze Mac lub na komputerze z systemem Windows otwórz [Azure Portal] w oknie przeglądarki.
 2. Karta Ustawienia dla aplikacji Mobile, wybierz polecenie **Rozpoczęcie pracy** (w obszarze komórkowy) > **Xamarin.Forms**. W kroku 3 kliknij przycisk **Utwórz nową aplikację** , jeśli jeszcze nie jest wybrana.  Następnie kliknij przycisk **Pobierz** .

    Spowoduje to pobranie projekt, który zawiera aplikacji klienckiej, która jest połączony z aplikacji dla urządzeń przenośnych. Zapisz plik skompresowany projektu na komputerze lokalnym i zanotuj miejsce, w którym można go zapisać.

 3. Wyodrębnianie projekt, który został pobrany, a następnie otwórz go w Xamarin Studio lub Visual Studio.

    ![][9]

    ![][8]


##<a name="optional-run-the-ios-project"></a>(Opcjonalnie) Uruchamianie programu project iOS

Ta sekcja jest uruchamiania projektu iOS Xamarin dla urządzeń z systemem iOS. Jeśli nie pracujesz z urządzeniami z systemem iOS, możesz pominąć tę sekcję.

####<a name="in-xamarin-studio"></a>W Xamarin Studio

1. Kliknij prawym przyciskiem myszy projektu iOS, a następnie kliknij **Jako projekt startowy**.
2. W menu **Uruchamianie** kliknij pozycję **Rozpocznij debugowanie** do tworzenia projektu i uruchom aplikację w emulatorze iPhone.

####<a name="in-visual-studio"></a>W programie Visual Studio
1. Kliknij prawym przyciskiem myszy projektu iOS, a następnie kliknij **Ustaw jako projekt startowy**.
2. W menu **Tworzenie** kliknij pozycję **Menedżer konfiguracji**.
3. W oknie dialogowym **Menedżer konfiguracji** zaznacz pola wyboru **Tworzenie** i **Wdrażanie** projektu iOS.
4. Naciśnij klawisz **F5** , aby utworzyć projektu i uruchamianie aplikacji w emulatorze iPhone.

    >[AZURE.NOTE] Jeśli masz problemy z tworzenie, uruchamianie NuGet Menedżera pakietów aktualizacji do najnowszej wersji Xamarin pomocy technicznej i pakietów. Czasami projektów Szybki Start może zwłoki w do najnowszych aktualizacji.    

W aplikacji, wpisz opisową tekstu, takie jak _Xamarin Dowiedz się,_ a następnie kliknij pozycję **+** przycisk.

![][10]

To przesyła żądanie POST do nowej aplikacji dla urządzeń przenośnych wewnętrznej bazy danych obsługiwane w Azure. Dane z żądania zostanie wstawiona do tabeli TodoItem. Elementy przechowywane w tabeli są zwracane przez wewnętrznej bazy danych aplikacji dla urządzeń przenośnych, a dane są wyświetlane na liście.

>[AZURE.NOTE]
> Można znaleźć kod, który uzyskuje dostęp do wewnętrznej bazy danych usługi aplikacji dla urządzeń przenośnych w pliku TodoItemManager.cs C# projektu biblioteki portable klasy rozwiązania.

##<a name="optional-run-the-android-project"></a>(Opcjonalnie) Uruchamianie Android projektu

Ta sekcja jest uruchamiania projektu telefon droid Xamarin dla systemu Android. Jeśli nie pracujesz z urządzeń z systemem Android, możesz pominąć tę sekcję.

####<a name="in-xamarin-studio"></a>W Xamarin Studio

1. Kliknij prawym przyciskiem myszy Android projektu, a następnie kliknij **Jako projekt startowy**.
2. W menu **Uruchamianie** kliknij pozycję **Rozpocznij debugowanie** do tworzenia projektu i uruchom aplikację w Android emulatora.

####<a name="in-visual-studio"></a>W programie Visual Studio
1. Kliknij prawym przyciskiem myszy projektu Android (telefon Droid), a następnie kliknij **Ustaw jako projekt startowy**.
4. W menu **Tworzenie** kliknij pozycję **Menedżer konfiguracji**.
5. W oknie dialogowym **Menedżer konfiguracji** zaznacz pola wyboru **Utwórz** i **Wdróż** Android projektu.
6. Naciśnij klawisz **F5** , aby utworzyć projektu i uruchamianie aplikacji w Android emulatora.

    >[AZURE.NOTE] Jeśli masz problemy z tworzenie, uruchamianie NuGet Menedżera pakietów aktualizacji do najnowszej wersji Xamarin pomocy technicznej i pakietów. Czasami projektów Szybki Start może zwłoki w do najnowszych aktualizacji.    


W aplikacji, wpisz opisową tekstu, takie jak _Xamarin Dowiedz się,_ a następnie kliknij pozycję **+** przycisk.

![][11]

To przesyła żądanie POST do nowej aplikacji dla urządzeń przenośnych wewnętrznej bazy danych obsługiwane w Azure. Dane z żądania zostanie wstawiona do tabeli TodoItem. Elementy przechowywane w tabeli są zwracane przez wewnętrznej bazy danych aplikacji dla urządzeń przenośnych, a dane są wyświetlane na liście.

> [AZURE.NOTE]
> Można znaleźć kod, który uzyskuje dostęp do wewnętrznej bazy danych usługi aplikacji dla urządzeń przenośnych w pliku TodoItemManager.cs C# projektu biblioteki portable klasy rozwiązania.


##<a name="optional-run-the-windows-project"></a>(Opcjonalnie) Uruchamianie projektu systemu Windows


Ta sekcja jest uruchamiania projektu Xamarin aplikacji dla urządzeń z systemem Windows. Jeśli nie pracujesz z urządzenia z systemem Windows, możesz pominąć tę sekcję.


####<a name="in-visual-studio"></a>W programie Visual Studio
1. Kliknij prawym przyciskiem myszy dowolną projektów systemu Windows, a następnie kliknij **Ustaw jako projekt startowy**.
4. W menu **Tworzenie** kliknij pozycję **Menedżer konfiguracji**.
5. W oknie dialogowym **Menedżer konfiguracji** zaznacz pola wyboru **Utwórz** i **Wdróż** wybranego projektu systemu Windows.
6. Naciśnij klawisz **F5** , aby utworzyć projektu i uruchamianie aplikacji w emulatora systemu Windows.

    >[AZURE.NOTE] Jeśli masz problemy z tworzenie, uruchamianie NuGet Menedżera pakietów aktualizacji do najnowszej wersji Xamarin pomocy technicznej i pakietów. Czasami projektów Szybki Start może zwłoki w do najnowszych aktualizacji.    


W aplikacji, wpisz opisową tekstu, takie jak _Xamarin Dowiedz się,_ a następnie kliknij pozycję **+** przycisk.

To przesyła żądanie POST do nowej aplikacji dla urządzeń przenośnych wewnętrznej bazy danych obsługiwane w Azure. Dane z żądania zostanie wstawiona do tabeli TodoItem. Elementy przechowywane w tabeli są zwracane przez wewnętrznej bazy danych aplikacji dla urządzeń przenośnych, a dane są wyświetlane na liście.

![][12]

> [AZURE.NOTE]
> Można znaleźć kod, który uzyskuje dostęp do wewnętrznej bazy danych usługi aplikacji dla urządzeń przenośnych w pliku TodoItemManager.cs C# projektu biblioteki portable klasy rozwiązania.

##<a name="next-steps"></a>Następne kroki

* [Dodawanie uwierzytelniania do aplikacji](app-service-mobile-xamarin-forms-get-started-users.md)  
Dowiedz się, jak uwierzytelniania użytkowników aplikacji z dostawcą tożsamości.

* [Dodawanie powiadomień wypychanych do aplikacji](app-service-mobile-xamarin-forms-get-started-push.md)  
Dowiedz się, jak dodać wypychanych powiadomień wsparcie dla aplikacji i konfigurowanie usługi wewnętrznej bazy danych aplikacji Mobile umożliwia koncentratory powiadomienie Azure do wysyłania powiadomień wypychanych.

* [Włączanie synchronizacja w trybie offline dla aplikacji](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  Dowiedz się, jak dodać Obsługa w trybie offline aplikacji przy użyciu aplikacji Mobile wewnętrznej bazy danych. Synchronizacja w trybie offline umożliwia użytkownikom końcowym współdziałanie z aplikacji dla urządzeń przenośnych&mdash;wyświetlania, dodawania i modyfikowania danych&mdash;nawet wtedy, gdy jest brak połączenia z siecią.

* [Jak w przypadku aplikacji Mobile Azure za pomocą klienta usługi zarządzanych](app-service-mobile-dotnet-how-to-use-client-library.md)  
Dowiedz się, jak pracować z zarządzanego klienta SDK w aplikacji Xamarin. 


<!-- Anchors. -->
[Getting started with mobile app backends]:#getting-started
[Create a new mobile app backend]:#create-new-service
[Next Steps]:#next-steps


<!-- Images. -->
[6]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart.png
[8]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-vs.png
[9]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-xs.png
[10]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-ios.png
[11]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-android.png
[12]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-windows.png


<!-- URLs. -->
[Visual Studio Professional 2013]: https://go.microsoft.com/fwLink/p/?LinkID=257546
[Mobile app SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[Azure Portal]: https://portal.azure.com/

