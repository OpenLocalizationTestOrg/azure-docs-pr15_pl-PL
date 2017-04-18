<properties
    pageTitle="Dodawanie uwierzytelniania na iOS przy użyciu aplikacji Mobile Azure"
    description="Dowiedz się, jak za pomocą uwierzytelniania użytkowników aplikacji iOS przy użyciu różnych dostawców tożsamości, w tym AAD, Google, Facebook, Twitter i Microsoft Azure aplikacji Mobile."
    services="app-service\mobile"
    documentationCenter="ios"
    authors="ysxu"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="add-authentication-to-your-ios-app"></a>Dodawanie uwierzytelniania do aplikacji w systemie iOS

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

W tym samouczku możesz dodać uwierzytelniania [iOS Szybkie rozpoczynanie] projektu za pośrednictwem dostawcy tożsamości obsługiwane. Ten samouczek jest oparty na samouczek [iOS szybki start] , które trzeba wykonać najpierw.

##<a name="register"></a>Zarejestruj się w aplikacji dla uwierzytelniania i konfigurowanie aplikacji usługi

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="permissions"></a>Ograniczanie uprawnień do uwierzytelnionych użytkowników

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

W Xcode naciśnij przycisk **Uruchom** , aby uruchomić aplikację. Będzie można wyjątek, ponieważ aplikacja próbuje uzyskać dostęp do wewnętrznej bazy danych jako użytkownik nieuwierzytelnionych, ale tabeli _TodoItem_ teraz wymaga uwierzytelniania.

##<a name="add-authentication"></a>Dodawanie uwierzytelniania do aplikacji

[AZURE.INCLUDE [app-service-mobile-ios-authenticate-app](../../includes/app-service-mobile-ios-authenticate-app.md)]


<!-- URLs. -->

[Szybki start systemu iOS]: app-service-mobile-ios-get-started.md

[Azure portal]: https://portal.azure.com
