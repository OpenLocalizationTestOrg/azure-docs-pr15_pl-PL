<properties
    pageTitle="Uwierzytelniania i autoryzacji w aplikacji dla urządzeń przenośnych Azure | Microsoft Azure"
    description="Omówienie uwierzytelniania i koncepcyjny odwołanie / funkcji autoryzacji dla aplikacji Mobile Azure"
    services="app-service\mobile"
    documentationCenter=""
    authors="mattchenderson"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="mahender"/>

# <a name="authentication-and-authorization-in-azure-mobile-apps"></a>Uwierzytelniania i autoryzacji w Azure aplikacji dla urządzeń przenośnych

## <a name="what-is-app-service-authentication--authorization"></a>Co to jest aplikacja usługi uwierzytelniania i autoryzacji?

> [AZURE.NOTE] W tym temacie zostaną objęte migracją na skonsolidowaną [uwierzytelniania usługi aplikacji i autoryzacji](../app-service/app-service-authentication-overview.md) temacie opisano, jak w sieci Web, Mobile i interfejsu API aplikacji.

Aplikacja usługi uwierzytelniania / autoryzacji jest funkcją, dzięki czemu aplikacji do logowania użytkowników bez żadnych zmian kodu wymaganego do wewnętrznej bazy danych aplikacji. Umożliwia łatwy sposób ochrony aplikacji i pracę z danymi na użytkownika.

Usługa aplikacji używa tożsamości federacyjnych, w którym 3rd **dostawcy tożsamości** ("protokołu IDP") są przechowywane kont i uwierzytelnia użytkowników, a aplikacja używa tej tożsamości zamiast własnej. Usługa aplikacji obsługuje pięć dostawcy tożsamości okno: _Usługi Azure Active Directory_, _Facebook_, _Google_, _Konto Microsoft_i _serwisu Twitter_. Integracja innego dostawcy tożsamości lub rozwiązanie tożsamości niestandardowej można również zwiększyć ten pomocy technicznej dla aplikacji.

Aplikacji pozwalająca dowolną liczbę tych dostawców tożsamości, więc można udostępnić opcje dotyczące sposobu logowania użytkowników końcowych.

Jeśli chcesz rozpocząć pracę od razu, zobacz jeden z następujące samouczki:

- [Dodawanie uwierzytelniania do aplikacji w systemie iOS]
- [Dodawanie uwierzytelniania do aplikacji Xamarin.iOS]
- [Dodawanie uwierzytelniania do aplikacji Xamarin.Android]
- [Dodawanie uwierzytelniania do aplikacji systemu Windows]

## <a name="how-authentication-works"></a>Jak działa uwierzytelniania

W celu uwierzytelnienia przy użyciu jednego z dostawców tożsamości, najpierw należy skonfigurować dostawcy tożsamości wiedzieć o aplikacji. Dostawcy tożsamości następnie umożliwi identyfikatory i hasła, które zapewniają wrócić do aplikacji. Spowoduje to wykonuje relacji zaufania i umożliwia aplikacji usługi sprawdzić poprawność tożsamości przekazane.

Te kroki szczegółowo w następujących tematach:

- [Jak skonfigurować aplikację umożliwia logowania usługi Azure Active Directory]
- [Jak skonfigurować aplikacji umożliwia logowania w serwisie Facebook]
- [Jak skonfigurować aplikację umożliwia logowania usługi Google]
- [Jak skonfigurować aplikację do logowania się do programu Microsoft Account za pomocą]
- [Jak skonfigurować aplikację umożliwia logowania Twitter]

Gdy wszystko jest skonfigurowany do wewnętrznej bazy danych, można zmodyfikować klientowi Zaloguj się. Istnieją dwie metody poniżej:

- Za pomocą jednego wiersza kodu, powiadom aplikacji Mobile klienta SDK Zaloguj użytkowników.
- Korzystanie z zestawu SDK opublikowany przez dostawcę podanej tożsamości do ustalania tożsamości i uzyskać dostęp do aplikacji usługi.

>[AZURE.TIP] Większość aplikacji należy używać dostawcę SDK, aby uzyskać bardziej macierzystego wrażenie środowisko logowania i korzystać z pomocy technicznej odświeżania i inne korzyści wynikające z specyficzne dla dostawcy.

### <a name="how-authentication-without-a-provider-sdk-works"></a>Jak działa uwierzytelnianie bez dostawcę SDK

Jeśli nie chcesz skonfigurować dostawcę SDK, możesz zezwolić aplikacji Mobile wykonać logowania. Klient aplikacji Mobile SDK będzie Otwórz widok sieci web dostawcy wybrane i wypełnij Zaloguj się. Od czasu do czasu na blogi i fora, który będzie widać, że to określonej jako "serwer przepływu" lub "przepływ kierowany serwera" od serwera jest używana do zarządzania logowania, a klient SDK nigdy nie odbiera token dostawcy.

Samouczek uwierzytelniania dla każdej platformy omówiono kod potrzebne do uruchomienia tego przepływu. Na końcu przepływu klient SDK ma tokenu usługi aplikacji, a token są automatycznie dołączane do wszystkie żądania do wewnętrznej bazy danych.

### <a name="how-authentication-with-a-provider-sdk-works"></a>Jak działa uwierzytelnianie za pomocą dostawcy SDK

Praca z dostawcą SDK umożliwia możliwości obsługi dziennika więcej ściśle współdziałanie z platformy systemu operacyjnego jest uruchomione aplikacji. Zapewnia to również token dostawcy i niektóre informacje o użytkowniku na klienta, który ułatwia używanie wykresu interfejsy API do dostosowywania środowiska użytkownika. Od czasu do czasu na blogi i fora będą widoczne to określane jako "przepływu klienta" lub "Przekierowanie klienta przepływu" od kodu na komputerze klienckim obsługuje logowania i kod klienta ma dostęp do tokenu dostawcy.

Po uzyskaniu tokenu dostawcy musi być przesyłane aplikacji usługi sprawdzania poprawności. Na końcu przepływu klient SDK ma tokenu usługi aplikacji, a token są automatycznie dołączane do wszystkie żądania do wewnętrznej bazy danych. Deweloper również zachować odwołanie do token dostawcy ich wyboru.

## <a name="how-authorization-works"></a>Jak działa autoryzowanie

Aplikacja usługi uwierzytelniania / autoryzacji udostępnia kilka możliwości dla **akcję do wykonania po żądanie nie jest uwierzytelniony**. Zanim kodzie otrzyma danego żądania, możesz mieć Sprawdź aplikacji usługi, jeśli uwierzytelnieniu żądania i jeżeli nie go odrzucić i spróbuj zalogować się przed ponowną użytkownik.

Jedna opcja ma mieć nieuwierzytelniony żądania przekierowanie do jednego z dostawców tożsamości. W przeglądarce sieci web zajmie to faktycznie użytkownika do nowej strony. Jednak nie nastąpi przekierowanie do klienta przenośnego w ten sposób i nieuwierzytelnionych odpowiedzi zostanie wyświetlony komunikat odpowiedzi HTTP _401 Unauthorized_ . Mieć to, zawsze należy pierwszego żądania przez klienta do punktu końcowego logowania, a następnie możesz nawiązywać połączenia do innych interfejsów API. Podczas próby połączenia innego interfejsu API przed zalogowaniem się klient otrzyma komunikat o błędzie.

Jeśli chcesz mieć bardziej szczegółowego sterowania, na które punkty końcowe wymaga uwierzytelniania, możesz również wybrać "żadnej akcji (Zezwalaj na żądanie)" dla nieuwierzytelnionych żądań. W tym przypadku wszystkie decyzje uwierzytelniania są wstrzymana w kodzie aplikacji. To również pozwala na Zezwalaj na dostęp do określonych użytkowników na podstawie reguł autoryzacji niestandardowe.

## <a name="documentation"></a>Dokumentacja

Następujące samouczki pokazująca, jak dodać uwierzytelniania dla klientów przenośnych przy użyciu aplikacji usługi:

- [Dodawanie uwierzytelniania do aplikacji w systemie iOS]
- [Dodawanie uwierzytelniania do aplikacji Xamarin.iOS]
- [Dodawanie uwierzytelniania do aplikacji Xamarin.Android]
- [Dodawanie uwierzytelniania do aplikacji systemu Windows]

Następujące samouczki pokazująca, jak skonfigurować usługę aplikacji je wykorzystać dostawców uwierzytelniania inny:

- [Jak skonfigurować aplikację umożliwia logowania usługi Azure Active Directory]
- [Jak skonfigurować aplikację do logowania Facebook za pomocą]
- [Jak skonfigurować aplikację umożliwia logowania usługi Google]
- [Jak skonfigurować aplikację do logowania się do programu Microsoft Account za pomocą]
- [Jak skonfigurować aplikację umożliwia logowania Twitter]

Jeśli chcesz używać system tożsamości niż opisane w tym miejscu, można również wykorzystać [Podgląd pomocy technicznej niestandardowego uwierzytelniania na serwerze .NET SDK](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth).

[Dodawanie uwierzytelniania do aplikacji w systemie iOS]: app-service-mobile-ios-get-started-users.md
[Dodawanie uwierzytelniania do aplikacji Xamarin.iOS]: app-service-mobile-xamarin-ios-get-started-users.md
[Dodawanie uwierzytelniania do aplikacji Xamarin.Android]: app-service-mobile-xamarin-android-get-started-users.md
[Dodawanie uwierzytelniania do aplikacji systemu Windows]: app-service-mobile-windows-store-dotnet-get-started-users.md

[Jak skonfigurować aplikację umożliwia logowania usługi Azure Active Directory]: app-service-mobile-how-to-configure-active-directory-authentication.md
[Jak skonfigurować aplikacji umożliwia logowania w serwisie Facebook]: app-service-mobile-how-to-configure-facebook-authentication.md
[Jak skonfigurować aplikację umożliwia logowania usługi Google]: app-service-mobile-how-to-configure-google-authentication.md
[Jak skonfigurować aplikację do logowania się do programu Microsoft Account za pomocą]: app-service-mobile-how-to-configure-microsoft-authentication.md
[Jak skonfigurować aplikację umożliwia logowania Twitter]: app-service-mobile-how-to-configure-twitter-authentication.md
