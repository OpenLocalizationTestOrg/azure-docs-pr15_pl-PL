<properties
    pageTitle="Omówienie interfejsu API eksportu zaangażowania urządzeń przenośnych"
    description="Informacje podstawowe informacje na temat eksportowania danych nieprzetworzonych wygenerowane przez użytkownika urządzenia, aby korzystać z jej własne narzędzia"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="kpiteira"
    manager="erikre"
    editor=""/>

<tags
    ms.service="mobile-engagement"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="mobile-multiple"
    ms.workload="mobile"
    ms.date="04/26/2016"
    ms.author="kpiteira"/>

# <a name="mobile-engagement-export-api-overview"></a>Omówienie interfejsu API eksportu zaangażowania urządzeń przenośnych

## <a name="introduction"></a>Wprowadzenie

W tym dokumencie dowiesz się, podstawowe informacje na temat eksportowania danych nieprzetworzonych wygenerowane przez użytkownika urządzenia, aby korzystać z jej własne narzędzia.

## <a name="pre-requisites"></a>Wymagania wstępne

Eksportowanie nieprzetworzonych danych z zaangażowania Mobile wymaga:

- Ustawienia uwierzytelniania interfejsu API aby można było używać interfejsów API (zobacz [Ręczna konfiguracja uwierzytelniania](mobile-engagement-api-authentication-manual.md)),
- Za pomocą interfejsów API pozostałych lub [.net SDK](mobile-engagement-dotnet-sdk-service-api.md)
- Konto Azure miejsca do magazynowania.

>[AZURE.NOTE] Również warto doskonałe [Eksplorator magazynu usługi Microsoft Azure](http://storageexplorer.com/)co najmniej fazie projektowania ponieważ zapewnia łatwy w obsłudze interfejs użytkownika interakcji z magazyn Azure.

## <a name="what-can-be-exported"></a>Co można eksportować?

Zaangażowania Mobile umożliwia użytkownikom zebrać wielu typów danych i w związku z tym zawiera kilka typów eksportu dostosowane do tych różne typy danych.
Istnieją 2 podstawowe typy eksportu:

- Migawka: używana zwykle eksportowania danych, który przedstawia stan i którego zaangażowania Mobile nie ma historii. Na przykład obejmuje znaczniki (info aplikacji), tokeny lub wypychanych kampanii opinie. W konsekwencji następujące typy eksportu nie są związane z datą.
- historyczne: ten typ eksportu jest używany dla danych, która sumuje w czasie, takich jak zdarzenia lub działania, na przykład.

W poniższej tabeli opisano wystarczająco wszystkie możliwe eksportowanie:

| Wyeksportuj typ | Typ danych | Opis                                                                                                                                 |
|-------------|-----------|---------------------------------------------------------------------------------------------------------------------------------------------|
| Migawka    | Powiadomienia push      | Umożliwia generowanie Eksport Push kampanii opinie na zasadzie na identyfikator urządzenia i nazwa użytkownika                                                              |
| Migawka    | Znacznik       | Umożliwia generowanie Eksportowanie znaczników (informacje o aplikacji) skojarzone z każdego urządzenia                                                                       |
| Migawka    | Urządzenie    | Umożliwia generowanie Eksport większość danych o urządzeniach, takich jak technicals (model, ustawienia regionalne, strefy czasowej,...), znaczniki, po raz pierwszy widoczna... |
| Migawka    | Tokenu     | Eksportuj wszystkie prawidłowe tokeny generuje                                                                                                 |
| Historyczne  | Działanie  | Umożliwia generowanie Eksportuj wszystkie działania dla każdego urządzenia w danym okresie                                                           |
| Historyczne  | Zdarzenia     | Umożliwia generowanie Eksportuj wszystkie działania dla każdego urządzenia w danym okresie                                                           |
| Historyczne  | Zadanie       | Umożliwia generowanie Eksportuj wszystkie zadania dla każdego urządzenia w danym okresie                                                                 |
| Historyczne  | Błąd     | Umożliwia generowanie Eksportuj wszystkie błędy dla poszczególnych urządzeń w danym okresie                                                               |

## <a name="how-does-it-work"></a>Jak to działa?

Eksportowanie są długie uruchamianie zadań, które mogą być bardzo duże pliki danych. Z tego powodu nie można wywołać natychmiast zwraca plik do pobrania.
Aby wyeksportować dane z zaangażowania Mobile, musisz utworzyć **Zadania eksportu** przy użyciu interfejsu API którym możesz określić, jak:

- Odpowiedni typ eksportu (migawki lub historycznych)
- Typ danych
- **Azure kontenera magazynu** (w tym prawidłowe skojarzenia zabezpieczeń z uprawnieniami do zapisu) miejsce, w którym zostanie zapisany wynik eksportowania.

Pamiętaj, że może potrwać kilka minut na zadanie do uruchomienia, a następnie może ją uruchom od kilku sekund w przypadku aplikacji bardzo mały do kilku godzin w przypadku aplikacji z dużą ilością użytkowników lub działania.

Po utworzeniu zadania jest możliwe sprawdzenie stanu, aby sprawdzić, czy jest nadal uruchomiony lub jeśli zostało ukończone.

Gdy zadanie jest zakończyła się pomyślnie, wynikowy plik danych jest dostępna w kontenerze dostarczonych miejsca do magazynowania.
