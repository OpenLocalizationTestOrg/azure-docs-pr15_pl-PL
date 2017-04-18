<properties
  pageTitle="Wersji zestawu SDK klienta i serwera w aplikacji Mobile i usługach Mobile | Azure aplikacji usługi"
  description="Lista klientów SDK i zgodności z wersjami SDK serwera Mobile usług i aplikacji Mobile Azure"
  services="app-service\mobile"
  documentationCenter=""
  authors="adrianhall"
  manager="erikre"
  editor=""/>

<tags
  ms.service="app-service-mobile"
  ms.workload="mobile"
  ms.tgt_pltfrm="mobile-multiple"
  ms.devlang="dotnet"
  ms.topic="article"
  ms.date="10/01/2016"
  ms.author="adrianha"/>

# <a name="client-and-server-versioning-in-mobile-apps-and-mobile-services"></a>Przechowywanie wersji klienta i serwera w aplikacji Mobile i usługach Mobile

Najnowszej wersji usługi Azure Mobile jest funkcją **Aplikacji Mobile** Azure aplikacji usługi.

SDK klienta i serwera aplikacji Mobile pierwotnie są oparte na tych w usługach Mobile, ale są one *niezgodny ze sobą* .
Oznacza to, że należy użyć klient *Aplikacji Mobile* SDK z serwerem *Aplikacji Mobile* SDK i podobnie dla *Usług Mobile*. Niniejsza Umowa są realizowane przez wartość nagłówka specjalne używane przez klienta i serwera SDK, `ZUMO-API-VERSION`.

Uwaga: zawsze, gdy ten dokument odwołuje się do *Usługi Mobile* wewnętrznej bazie danych, go nie musi być obsługiwana w usługach Mobile. Obecnie istnieje możliwość migracji Usługa mobilna do uruchamiania aplikacji usługi bez żadnych zmian kodu, ale usługi będą nadal stosowane wersji SDK *Mobile Services* .

Aby dowiedzieć się więcej na temat migracji do usługi aplikacji bez żadnych zmian kodu, zobacz artykuł [migracji usługi mobilnej usłudze Azure w aplikacji].

## <a name="header-specification"></a>Specyfikacja nagłówka

Klucz `ZUMO-API-VERSION` można określić w nagłówku HTTP lub ciągu kwerendy. Wartość jest ciągiem wersji w formularzu **x.y.z**.

Na przykład:

Uzyskiwanie https://service.azurewebsites.net/tables/TodoItem

NAGŁÓWKI: W ZUMO WERSJA API: 2.0.0

Publikowanie https://service.azurewebsites.net/tables/TodoItem?ZUMO-API-VERSION=2.0.0

## <a name="opting-out-of-version-checking"></a>Rezygnowania z sprawdzania wersji

Można zaprzestać korzystania z wersji sprawdzania, ustawiając wartość **true** dla aplikacji ustawienie **MS_SkipVersionCheck**. Określ, w swojej web.config lub w sekcji Ustawienia aplikacji Azure portal.

> [AZURE.NOTE] Istnieje szereg zmiany działania między usługami Mobile i aplikacje telefon komórkowy, zwłaszcza w zakresie synchronizacja w trybie offline, uwierzytelnianie i powiadomienia wypychane. Należy tylko zaprzestać korzystania z wersji sprawdzanie po przetestowaniu ukończone, aby upewnić się, że te zmiany funkcjonalne nie podziału funkcjonalności aplikacji sieci.

## <a name="summary-of-compatibility-for-all-versions"></a>Podsumowanie zgodności dla wszystkich wersji

Wykres poniżej przedstawia zgodność między wszystkie typy klienta i serwera. **Usług** mobilnych lub **aplikacji** dla urządzeń przenośnych oparte na serwerze SDK, która jest używana jest klasyfikowane wewnętrznej bazie danych.

|                           | **Usług mobilnych** Node.js lub .NET | **Aplikacje dla urządzeń przenośnych** Node.js lub .NET |
| ----------                | -----------------------             |   ----------------              |
| [Klientów usług Mobile] | Ok                                  | Błąd\*                         |
| [Klienci aplikacji Mobile]     | Błąd\*                             | Ok                              |

\*Te można kontrolować, określając **MS_SkipVersionCheck**.


<!-- IMPORTANT!  The anchors for Mobile Services and Mobile Apps MUST be 1.0.0 and 2.0.0 respectively, since there is an exception error message that uses those anchors. -->

<!-- NOTE: the fwlink to this document is http://go.microsoft.com/fwlink/?LinkID=690568 -->

## <a name="1.0.0"></a>Telefon komórkowy usług klienta i serwera

Klient SDK w poniższej tabeli są zgodne z **Usługami Mobile**.

Uwaga: z klienta usług Mobile SDK *nie* wysyłać wartość nagłówka `ZUMO-API-VERSION`. Jeśli usługa odbierze ten nagłówek lub wartość ciągu kwerendy, zostanie zwrócony błąd, chyba że jawnie wybrano w Pomniejsz zgodnie z powyższym opisem.

### <a name="MobileServicesClients"></a>Klienta przenośnego *usługi* SDK

| Platformy klienta                   | Wersja                                                                   | Wartość nagłówka wersji |
| -------------------               | ------------------------                                                  | -------------------  |
| Zarządzane klienta (Windows, Xamarin) | [1.3.2](https://www.nuget.org/packages/WindowsAzure.MobileServices/1.3.2) | n/d!                  |
| iOS                               | [2.2.2](http://aka.ms/gc6fex)                                             | n/d!                  |
| Android                           | [2.0.3](https://go.microsoft.com/fwLink/?LinkID=280126)                   | n/d!                  |
| HTML                              | [1.2.7](http://ajax.aspnetcdn.com/ajax/mobileservices/MobileServices.Web-1.2.7.min.js) | n/d!     |

### <a name="mobile-services-server-sdks"></a>Serwer *usług* mobilnych SDK

| Platformy serwera  | Wersja                                                                                                        | Nagłówek zaakceptowanych wersji |
| ---------------- | ------------------------------------------------------------                                                   | ----------------------- |
| .NET             | [Wersja WindowsAzure.MobileServices.Backend.* 1.0.x](https://www.nuget.org/packages/WindowsAzure.MobileServices.Backend/) | **Brak nagłówka wersji** |
| Node.js          | (już wkrótce)                        | **Brak nagłówka wersji** |

<!-- TODO: add Node npm version -->

### <a name="behavior-of-mobile-services-backends"></a>Zachowanie pomocą usług Mobile

| ZUMO INTERFEJSU API WERSJI | Wartość MS_SkipVersionCheck | Odpowiedź |
| ---------------- | ---------------------------- | -------- |
| Nie określono    | Dowolny                          | 200 - OK |
| Każda wartość        | PRAWDA                         | 200 - OK |
| Każda wartość        | Określona wartość FAŁSZ/nie          | 400 — Nieprawidłowe żądanie |

## <a name="2.0.0"></a>Azure aplikacji Mobile klienta i serwera

### <a name="MobileAppsClients"></a>Klientów przenośnych *aplikacji* SDK

Sprawdzanie wersji wprowadzono począwszy od następujących wersji klienta SDK dla **Aplikacji Mobile Azure**:

| Platformy klienta                   | Wersja                   | Wartość nagłówka wersji |
| -------------------               | ------------------------  | -----------------    |
| Zarządzane klienta (Windows, Xamarin) | [2.0.0](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/2.0.0) | 2.0.0 |
| iOS                               | [3.0.0](http://go.microsoft.com/fwlink/?LinkID=529823) | 2.0.0  |
| Android                           | [3.0.0](http://go.microsoft.com/fwlink/?LinkID=717033&clcid=0x409) | 3.0.0 |

<!-- TODO: add HTML version when released -->

### <a name="mobile-apps-server-sdks"></a>*Aplikacje* serwera SDK

Sprawdzanie wersji znajduje się w poniższej serwera SDK wersji:

| Platformy serwera  | ZESTAW SDK                                                                                                        | Nagłówek zaakceptowanych wersji |
| ---------------- | ------------------------------------------------------------                                                   | ----------------------- |
| .NET             | [Microsoft.Azure.Mobile.Server](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) | 2.0.0 |
| Node.js          | [aplikacje Azure-mobile)](https://www.npmjs.com/package/azure-mobile-apps)                         | 2.0.0 |

### <a name="behavior-of-mobile-apps-backends"></a>Zachowanie aplikacji Mobile wewnętrznych bazach danych

| ZUMO INTERFEJSU API WERSJI | Wartość MS_SkipVersionCheck | Odpowiedź |
| ---------------- | ---------------------------- | -------- |
| x.y.z albo wartość Null    | PRAWDA                         | 200 - OK |
| Wartości null             | Określona wartość FAŁSZ/nie          | 400 — Nieprawidłowe żądanie |
| 1.x.y            | Określona wartość FAŁSZ/nie          | 400 — Nieprawidłowe żądanie |
| 2.0.0-2.x.y      | Określona wartość FAŁSZ/nie          | 200 - OK |
| 3.0.0-3.x.y      | Określona wartość FAŁSZ/nie          | 400 — Nieprawidłowe żądanie |


## <a name="next-steps"></a>Następne kroki

- [Migrowanie usług mobilnych do Azure aplikacji usługi]


[Klientów usług Mobile]: #MobileServicesClients
[Klienci aplikacji Mobile]: #MobileAppsClients


[Mobile App Server SDK]: http://www.nuget.org/packages/microsoft.azure.mobile.server
[Migrowanie usług mobilnych do Azure aplikacji usługi]: app-service-mobile-migrating-from-mobile-services.md

