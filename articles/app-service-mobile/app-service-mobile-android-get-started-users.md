<properties
    pageTitle="Dodawanie uwierzytelniania w systemie Android przy użyciu aplikacji Mobile | Azure aplikacji usługi"
    description="Dowiedz się, jak używać aplikacji Mobile w usłudze Azure aplikacji do uwierzytelniania użytkowników aplikacji Android przy użyciu różnych dostawców tożsamości, w tym Google, Facebook, Twitter i Microsoft."
    services="app-service\mobile"
    documentationCenter="android"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="add-authentication-to-your-android-app"></a>Dodawanie uwierzytelniania do aplikacji systemu Android

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

## <a name="summary"></a>Podsumowanie

W tym samouczku możesz dodać do listy zadań projektu Szybki Start na Android za pośrednictwem dostawcy tożsamości obsługiwane uwierzytelniania. Ten samouczek jest oparty na samouczek [Wprowadzenie do aplikacji Mobile] , które trzeba wykonać najpierw.

##<a name="register"></a>Zarejestruj się w aplikacji dla uwierzytelniania i konfigurowanie aplikacji usługi

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="permissions"></a>Ograniczanie uprawnień do uwierzytelnionych użytkowników

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

+ Android Studio Otwórz przewidywane wykonywane przez Ciebie projektu z samouczka [Wprowadzenie do aplikacji Mobile]. Z menu **Uruchamianie** kliknij pozycję **Uruchom aplikację** i sprawdź, czy powodu nieobsługiwanego wyjątku z kodem stanu 401 (Unauthorized) jest wywoływane po uruchomieniu aplikacji.

     Ten wyjątek spowodowany aplikacja próbuje uzyskać dostęp do wewnętrznej bazy danych jako użytkownik nieuwierzytelnionych, ale tabeli _TodoItem_ teraz wymaga uwierzytelniania.

Następnie możesz zaktualizować aplikację do uwierzytelniania użytkowników przed żądaniem zasobów w aplikacji Mobile wewnętrznej bazy danych.

## <a name="add-authentication-to-the-app"></a>Dodawanie uwierzytelniania aplikacji

[AZURE.INCLUDE [mobile-android-authenticate-app](../../includes/mobile-android-authenticate-app.md)]

## <a name="cache-tokens"></a>Pamięć podręczną tokenów uwierzytelniania na komputerze klienckim

[AZURE.INCLUDE [mobile-android-authenticate-app-with-token](../../includes/mobile-android-authenticate-app-with-token.md)]

##<a name="next-steps"></a>Następne kroki

Po wykonaniu tego samouczka uwierzytelnianie podstawowe, rozważ kontynuowaniem na jedno z następujące samouczki:

+ [Powiadomienia wypychane Dodaj do aplikacji systemu Android](app-service-mobile-android-get-started-push.md) Dowiedz się, jak skonfigurować do wewnętrznej bazy danych aplikacji Mobile umożliwia koncentratory powiadomienie Azure do wysyłania powiadomień wypychanych.

+ [Włączanie synchronizacja w trybie offline dla systemu Android aplikacji](app-service-mobile-android-get-started-offline-data.md) Dowiedz się, jak dodać Obsługa w trybie offline aplikacji przy użyciu aplikacji Mobile wewnętrznej bazie danych. Synchronizacja w trybie offline umożliwia użytkownikom końcowym korzystać z aplikacji dla urządzeń przenośnych&mdash;wyświetlania, dodawania i modyfikowania danych&mdash;nawet wtedy, gdy jest brak połączenia z siecią.



<!-- Anchors. -->
[Register your app for authentication and configure Mobile Services]: #register
[Restrict table permissions to authenticated users]: #permissions
[Add authentication to the app]: #add-authentication
[Store authentication tokens on the client]: #cache-tokens
[Refresh expired tokens]: #refresh-tokens
[Next Steps]:#next-steps


<!-- URLs. -->
[Wprowadzenie do aplikacji Mobile]: app-service-mobile-android-get-started.md
