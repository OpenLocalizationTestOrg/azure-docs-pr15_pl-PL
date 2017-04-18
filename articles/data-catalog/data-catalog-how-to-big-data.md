<properties
   pageTitle="Jak pracować ze źródłami danych "big data" | Microsoft Azure"
   description="Artykule wyróżnianie wzorców dla wykazu danych Azure za pomocą "big data" źródeł danych, takich jak magazyn obiektów Blob platformy Azure, Lake danych Azure i usługi Hadoop HDFS."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="10/04/2016"
   ms.author="maroche"/>


# <a name="how-to-work-with-big-data-sources-in-azure-data-catalog"></a>Jak pracować ze źródłami danych duży w wykazie danych Azure

## <a name="introduction"></a>Wprowadzenie
**Wykaz danych Microsoft Azure** to usługa w chmurze w pełni zarządzane, która pełni funkcję systemu rejestracji i system odnajdowania dla źródła danych przedsiębiorstwa. Innymi słowy **Wykazu danych Azure** to wszystko o osobach pomoc odnajdowanie, opis i używanie źródeł danych i organizacji pomoc jeszcze lepiej korzystać ze swoich istniejących źródeł danych, łącznie z danymi duży.

**Wykaz danych Azure** obsługuje rejestracji blob magazyn blogu Azure i katalogów oraz usługi Hadoop HDFS pliki i katalogi. Półstrukturalnych rodzaj te źródła danych zawiera elastyczność, ale oznacza to również, że użytkowników należy rozważyć, jak źródła danych są zorganizowane w celu uzyskanie najbardziej wartości zarejestrowanie ich z **Wykazem danych Azure**.

## <a name="directories-as-logical-data-sets"></a>Katalogi jako zestawy danych logicznych.

Wspólne deseniu w organizowaniu źródeł danych duży jest traktowania katalogów jako logiczne zestawy danych. Katalogi najwyższego poziomu są używane do Definiowanie zestawu danych, gdy podfoldery Definiowanie partycje i przechowywać pliki, które zawierają dane.

Przykład tego wzorca może wyglądać następująco:

    \vehicle_maintenance_events
        \2013
        \2014
        \2015
            \01
                \2015-01-trailer01.csv
                \2015-01-trailer92.csv
                \2015-01-canister9635.csv
                ...
    \location_tracking_events
        \2013
        ...

W tym przykładzie vehicle_maintenance_events i location_tracking_events reprezentują logiczne zestawy danych. Każdy z tych folderów, zawiera pliki danych, które są zorganizowane według roku i miesiąca w podfoldery. Każdy z tych folderów, potencjalnie może zawierać, setki lub tysiące plików.

W ten sposób rejestrowania pojedyncze pliki z **Wykazem danych Azure** prawdopodobnie nie miały znaczenie. Zamiast tego Zarejestruj katalogów, reprezentujących zestawów danych, które będą przydatności dla użytkowników, Praca z danymi.

## <a name="reference-data-files"></a>Pliki danych odwołania

Dopełniająca deseń jest przechowywanie zestawów danych odwołania jako pojedyncze pliki. Te zestawy danych może być traktować jako "małe" części danych duży, a często są podobne do wymiarów w modelu analizy danych. Pliki danych odwołania zawierają rekordy, które służą do tworzenia kontekstu zbiorczego plików danych przechowywanych w innym miejscu w magazynie duży danych.

Przykład tego wzorca może wyglądać następująco:

    \vehicles.csv
    \maintenance_facilities.csv
    \maintenance_types.csv

Pracując naukowca analityka lub dane z danych zawartych w większych struktur katalogu danych w plikach odwołanie może służyć do bardziej szczegółowe informacje dotyczące obiektów, które odwołuje się tylko nazwę lub identyfikator większy zestaw danych.

W ten sposób będą zrozumiałe zarejestrować odwołania pojedyncze pliki danych za pomocą **Wykazu danych Azure**. Każdy plik reprezentuje zestaw danych, a każdą z nich można dodawać adnotacje i wykryte pojedynczo.

## <a name="alternate-patterns"></a>Alternatywny wzorców

Desenie opisanych powyżej są tylko dwa różne sposoby, które mogą być uporządkowane magazynu duży danych, ale każda implementacja może się różnić. Niezależnie od tego, jak źródła danych mają strukturę, podczas rejestrowania źródła danych duży z **Wykazem danych Azure**skoncentrować się na rejestrowanie pliki i katalogi reprezentujących zestawów danych, które będą miały wartość do innych osób w organizacji. Rejestrowanie wszystkie pliki i katalogi czy folderu mało istotne katalogu, dzięki czemu trudniejsze użytkownikom znajdowanie potrzebnych informacji.

## <a name="summary"></a>Podsumowanie
Rejestrowanie źródeł danych dzięki **Wykazowi danych Azure** ułatwia ich odnajdowanie i opis. Rejestrowanie i adnotacje danych duże pliki i katalogi, reprezentujących logiczne zestawy danych, ułatwiają użytkownikom znajdowanie i używanie źródeł danych duży, które są potrzebne.
