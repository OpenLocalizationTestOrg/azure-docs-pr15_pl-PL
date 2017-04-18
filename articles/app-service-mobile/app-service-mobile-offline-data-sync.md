<properties
    pageTitle="Synchronizowanie danych w trybie offline w aplikacji dla urządzeń przenośnych Azure | Microsoft Azure"
    description="Omówienie funkcji synchronizacji danych trybu offline dla aplikacji Mobile Azure i koncepcyjny odwołania"
    documentationCenter="windows"
    authors="adrianhall"
    manager="dwrede"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="offline-data-sync-in-azure-mobile-apps"></a>Synchronizowanie danych w trybie offline w aplikacji dla urządzeń przenośnych Azure

## <a name="what-is-offline-data-sync"></a>Co to jest synchronizowanie danych w trybie offline?

Synchronizacja danych w trybie offline jest klienta i serwera SDK funkcji aplikacji Mobile Azure, który ułatwia dla deweloperów do tworzenia aplikacji, które działają bez połączenia sieciowego.

Jeśli aplikacja jest w trybie offline, użytkownicy nadal można tworzyć i modyfikować dane, które zostaną zapisane w magazynu lokalnego. Po powrocie do trybu online aplikacja zsynchronizowania zmian lokalnych z Twojej aplikacji Mobile Azure wewnętrznej bazy danych. Ta funkcja umożliwia wykrywanie konfliktów, gdy tego samego rekordu zostanie zmieniona na klienta i wewnętrznej bazy danych. Następnie można rozwiązać konflikty, albo na serwerze lub klienta.

Synchronizacja w trybie offline ma wiele zalet:

* Ulepsz czas reakcji aplikacji, buforowanie danych serwera lokalnie na urządzeniu
* Tworzyć niezawodne aplikacje, które pozostają przydatne w przypadku problemów z siecią
* Zezwalaj na użytkowników końcowych do tworzenia i modyfikowania danych, nawet wtedy, gdy jest nie dostępu do sieci, pomocniczych scenariusze niewielki lub połączenia
* Synchronizowanie danych na wielu urządzeniach i wykrywanie konfliktów modyfikacji tego samego rekordu za pomocą dwóch urządzeń
* Ograniczyć użycie sieci w sieciach opóźnień wysoki lub taryfowych

Następujące samouczki pokazująca, jak dodać synchronizacja w trybie offline dla klientów przenośnych przy użyciu aplikacji Mobile Azure:

* [Android: Włącz synchronizacja w trybie offline]
* [iOS: Włączanie synchronizacja w trybie offline]
* [Xamarin iOS: Włączanie synchronizacja w trybie offline]
* [Xamarin Android: Włączanie synchronizacja w trybie offline]
* [Xamarin.Forms: Synchronizacja w trybie offline Włącz](app-service-mobile-xamarin-forms-get-started-offline-data.md)
* [Uniwersalny platformy systemu Windows: Włączanie synchronizacja w trybie offline]

## <a name="what-is-a-sync-table"></a>Co to jest tabela synchronizacji?

Aby uzyskać dostęp do punktu końcowego "/ tabele", klienta Azure Mobile SDK podać interfejsów takich jak `IMobileServiceTable` (klient .NET SDK) lub `MSTable` (iOS klienta). Te interfejsy API podłącz bezpośrednio do wewnętrznej bazy danych aplikacji Mobile Azure i zakończy się niepowodzeniem, jeśli na urządzeniu klienta nie ma połączenia sieciowego.

Do obsługi w trybie offline, aplikacji powinna Użyj zamiast tego w *tabeli synchronizacji* interfejsów API, takich jak `IMobileServiceSyncTable` (klient .NET SDK) lub `MSSyncTable` (iOS klienta). Te same OBSŁUGIWAŁ operacje (Tworzenie, Odczyt, Aktualizuj, Usuń) działa synchronizacja tabeli interfejsów API, z wyjątkiem teraz będzie odczytu lub zapisu do *magazynu lokalnego*. Zanim będzie można dokonać wszystkie operacje tabeli synchronizacji, musi być zainicjowany magazynu lokalnego.

## <a name="what-is-a-local-store"></a>Co to jest magazynu lokalnego?

Magazynu lokalnego jest warstwa utrzymywanie danych na tym urządzeniu klienta. SDK klienta aplikacji Mobile Azure zapewniają domyślna Implementacja magazynu lokalnego. W systemie Windows, Xamarin i Android jest oparty na SQLite; w systemie iOS jest on oparty na podstawowych danych.

Aby używać implementacji oparte na SQLite w systemie Windows Phone lub Windows 8.1 magazynu, należy zainstalować rozszerzenie SQLite. Aby uzyskać więcej informacji, zobacz [uniwersalny platformy systemu Windows: Włączanie synchronizacja w trybie offline]. Android oraz iOS dostarczane z wersji SQLite w systemie operacyjnym urządzenia, więc nie jest konieczne odwołać własną wersję SQLite.

Deweloperów można także zaimplementować własne magazynu lokalnego. Na przykład jeśli chcesz zapisać dane w postaci zaszyfrowanej na klienta przenośnego, można zdefiniować magazynu lokalnego, która korzysta z szyfrowania SQLCipher.

## <a name="what-is-a-sync-context"></a>Co to jest kontekst synchronizacji?

*Kontekst synchronizacji* jest skojarzony z obiektem klienta przenośnego (takie jak `IMobileServiceClient` lub `MSClient`) i śledzenie zmian wprowadzanych z tabelami synchronizacji. Kontekst synchronizacji zachowuje że *kolejki operacji* zachowuje uporządkowana lista CUD operacji (Tworzenie, aktualizowanie, usuwanie), które później będą wysyłane na serwerze.

Magazynu lokalnego jest skojarzony z kontekstu synchronizacji, takich jak przy użyciu metody inicjowania `IMobileServicesSyncContext.InitializeAsync(localstore)` w [kliencie .NET SDK].

## <a name="how-sync-works"></a>Jak w trybie offline działa synchronizacja

Korzystając z tabel synchronizacji, kod klienta kontrolki, gdy lokalne zmiany zostaną zsynchronizowane z aplikacji Mobile Azure wewnętrznej bazy danych. Nic się nie są wysyłane do wewnętrznej bazy danych momentu połączenia do lokalnych zmian *push* . Podobnie magazynu lokalnego zostanie wypełniona nowych danych, tylko wtedy, gdy połączenie do *pobrania* danych.

* **Wypychanych**: operacji w ramach synchronizacji i wysyła wszystkie zmiany CUD od czasu ostatniego push. Należy pamiętać, że nie można wysyłać tylko tabeli programu zmiany, ponieważ w przeciwnym razie operacje mogła zostać wysłana kolejności. Wypychanych wykonuje szereg pozostałych połączenia do Twojej aplikacji Mobile Azure wewnętrznej bazy danych, która z kolei zmieni serwera bazy danych.

* **Pobierają**: pobieraj odbywa się na konkretnych tabel i można je dostosować za pomocą kwerendy w celu pobrania tylko podzestawu danych serwera. SDK klienta Azure Mobile wstawić danych do magazynu lokalnego.

* **Niejawne umieszcza**: Jeśli pobieraj jest wykonywane względem tabeli, która ma oczekujące aktualizacje lokalnych, pobieraj najpierw wykonywanie wypychanych w kontekście synchronizacji. Dzięki temu minimalizowanie konfliktów między zmiany, znajdujące się już w kolejce i nowe dane z serwera.

* **Synchronizacja przyrostowa**: pierwszy parametr operacji pobieraj jest *nazwę kwerendy* , która jest używana tylko na komputerze klienckim. Jeśli używasz nazwę kwerendy różne od null Azure Mobile SDK wykona *Synchronizacja przyrostowa*.
  Zawsze operacji pobieraj zwraca zestaw wyników, najnowszą `updatedAt` sygnatura czasowa z tego zestawu wyników są przechowywane w tabelach system lokalny SDK. Pobieraj kolejne operacje pobierze tylko rekordy po ten znacznik.

  Na synchronizację przyrostowe serwera musi zwracać zrozumiałej `updatedAt` wartości, a także musi obsługiwać sortowanie według tego pola. Jednak ponieważ SDK doda własny sortowania w polu updatedAt, nie można używać pobieraj kwerendę, która ma swój własny `$orderBy$` klauzuli.

  Nazwa kwerendy można dowolny ciąg, który możesz wybrać, ale musi być unikatowa dla każdego logiczne zapytania w aplikacji.
  W przeciwnym razie pobieraj różnych operacji można zastąpić samej sygnatury czasowej synchronizacja przyrostowa i zapytań można zwracać niepoprawne wyniki.

  Jeśli kwerenda ma parametr, jednym ze sposobów Tworzenie nazwy unikatowe kwerendy jest uwzględnienie wartości parametru.
  Na przykład jeśli filtrujesz na nazwę użytkownika, nazwę kwerendy można następująco (w języku C#):

        await todoTable.PullAsync("todoItems" + userid,
            syncTable.Where(u => u.UserId == userid));

  Jeśli chcesz zaprzestać korzystania z synchronizacja przyrostowa przekazać `null` jako identyfikator kwerendy. W tym przypadku pobierane wszystkie rekordy przy każdym wywołaniu do `PullAsync`, która jest potencjalnie nieefektywne.

* **Purging**: możesz wyczyścić zawartości przy użyciu magazynu lokalnego `IMobileServiceSyncTable.PurgeAsync`.
  Może to być konieczne, jeśli masz stare dane w bazie danych klienta lub jeśli chcesz odrzucić wszystkie zmiany oczekiwanie.

  Wyczyścić wyczyści tabeli z magazynu lokalnego. W przypadku operacji oczekujące synchronizacji z bazy danych serwera czyszczenia Zgłoś wyjątek, chyba że *Czyszczenie życie* parametr jest ustawiony.

  Przykład starych danych na komputerze klienckim Załóżmy, że w tym przykładzie "list zadania" Device1 pobiera tylko elementy, które nie zostaną ukończone. Następnie todoitem "Zakupić mleko" jest oznaczony jako ukończonego na serwerze przez inne urządzenie. Device1 będzie nadal jednak todoitem "Kup mleka" w lokalnym magazynie ponieważ tylko jest pobierania danych elementów, które nie są oznaczone jako ukończone. Wyczyścić wyczyści tego elementu starych.

## <a name="next-steps"></a>Następne kroki

* [iOS: Włączanie synchronizacja w trybie offline]
* [Xamarin iOS: Włączanie synchronizacja w trybie offline]
* [Xamarin Android: Włączanie synchronizacja w trybie offline]
* [Uniwersalny platformy systemu Windows: Włączanie synchronizacja w trybie offline]

<!-- Links -->
[Klient programu .NET SDK]: app-service-mobile-dotnet-how-to-use-client-library.md
[Android: Włącz synchronizacja w trybie offline]: app-service-mobile-android-get-started-offline-data.md
[iOS: Włączanie synchronizacja w trybie offline]: app-service-mobile-ios-get-started-offline-data.md
[Xamarin iOS: Włączanie synchronizacja w trybie offline]: app-service-mobile-xamarin-ios-get-started-offline-data.md
[Xamarin Android: Włączanie synchronizacja w trybie offline]: app-service-mobile-xamarin-ios-get-started-offline-data.md
[Uniwersalny platformy systemu Windows: Włączanie synchronizacja w trybie offline]: app-service-mobile-windows-store-dotnet-get-started-offline-data.md
