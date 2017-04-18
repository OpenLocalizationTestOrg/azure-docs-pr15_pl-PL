<properties
   pageTitle="Jak zarządzać danych składniki majątku | Microsoft Azure"
   description="Artykule wyróżnianie sposobie kontrolowania widoczność i własności aktywów danych zarejestrowane w wykazie danych Azure."
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


# <a name="how-to-manage-data-assets"></a>Jak zarządzać danych składników majątku

## <a name="introduction"></a>Wprowadzenie

**Wykaz danych Azure** zapewnia możliwości dla odnajdowania źródła danych, umożliwiając użytkownikom łatwe odnajdowanie i opis źródła danych, które są potrzebne do przeprowadzenia analizy i podejmowanie decyzji. Tych funkcji odnajdowania wprowadź największy wpływ, gdy wszyscy użytkownicy mogą znaleźć i opis najszerszym zakresie dostępnych źródeł danych. Pamiętając o tym domyślne zachowanie wykaz danych dla wszystkich źródeł danych zarejestrowanych będą widoczne dla — i odnajdowania przez — wykazu wszystkich użytkowników.

Wykaz danych nie udzielanie dostępu do samych danych. Dostęp do danych jest kontrolowane przez właściciela źródła danych. Wykaz danych umożliwia użytkownikom, aby odnaleźć źródła danych i wyświetlić metadane związane z źródła zarejestrowane w wykazie.

Czasami może występować sytuacjach, jednak źródeł danych należy tylko być widoczne dla określonych użytkowników lub członkom określonej grupy. W przypadku następujących scenariuszy wykaz danych umożliwia użytkownikom przejmowanie praw własności aktywów zarejestrowane dane w katalogu, a następnie kontrolki widoczność aktywów właścicielem.

> [AZURE.NOTE] Funkcje opisane w tym artykule są dostępne tylko w standardowej wersji programu Azure wykazu danych. Bezpłatna wersja nie oferuje możliwości własności i ograniczanie widoczność zawartości danych.

## <a name="managing-ownership-of-data-assets"></a>Zarządzanie własności aktywów danych
Domyślnie danych zarejestrowanych w wykazie danych są go; Każdy użytkownik z uprawnieniami dostępu do katalogu można wykryć i dodawać adnotacje te zasoby. Użytkowników może potrwać własności aktywów danych go, a następnie można ograniczyć widoczność środków trwałych, które są właścicielami.

Gdy zbiór danych w wykazie danych jest właścicielem, tylko użytkownicy autoryzowanych przez właścicieli można wykryć elementu i wyświetlić jego metadane i tylko właściciele można usunąć elementu z wykazu.

> [AZURE.NOTE] Własności w wykazie danych dotyczy tylko metadanych przechowywanych w katalogu. Nie przyznaje żadnych uprawnień w źródle danych.

### <a name="taking-ownership"></a>Przejmowanie na własność
Użytkownicy mogą przejmować własności aktywów danych przez wybranie opcji "Własność" w portalu wykazu danych. Nie specjalne uprawnienia są wymagane do przejmowanie praw własności środka trwałego go danych; Każdy użytkownik może potrwać własności środka trwałego go danych.

### <a name="adding-owners-and-co-owners"></a>Dodawanie właścicieli i Współtworzenie
Jeśli zbiór danych jest już przypisany, użytkownicy nie mogą po prostu przejmowanie praw własności — musi zostać dodana jako właściciele Współtworzenie przez właściciela istniejących. Wszelkie właściciel można dodać dodatkowych użytkowników lub grup zabezpieczeń jako właścicieli Współtworzenie.

> [AZURE.NOTE] Jest najlepszym rozwiązaniem jest co najmniej dwie osoby jako właścicieli dla wszystkich aktywów własnością danych.

### <a name="removing-owners"></a>Usuwanie właścicieli
Tak samo jak dowolną właściciela zawartości można dodać Współtworzenie właściciele, wszelkie właściciela zawartości można usunąć dowolne współwłaściciela.

Jeśli właściciela zawartości usuwa sobie właściciela, on już Zarządzanie elementu. Jeśli właściciel zawartości usuwa sobie jako właściciel i istnieją inne właściciele Współtworzenie, elementu zostaną przywrócone go do stanu.

## <a name="visibility"></a>Widoczność
Właściciele zawartości danych można sterować widocznością aktywów danych, które są właścicielami. Aby uniemożliwić widoczność default — miejsce, w którym wszyscy użytkownicy wykazu danych można wykryć i wyświetlanie zawartości danych — właściciel elementów zawartości, można przełączyć widoczność ustawienie z "Wszystkim" do "Właściciele i tym użytkownikom" w oknie dialogowym właściwości dla elementu. Właściciele można dodać określonych użytkowników i grup zabezpieczeń.

> [AZURE.NOTE] Jeśli to możliwe, trwałego własności i widoczność uprawnienia mają być przydzielane grupom zabezpieczeń, a nie do poszczególnych użytkowników.

## <a name="catalog-administrators"></a>Administratorzy wykazu
Administratorzy wykazu danych są niejawnie Współtworzenie właściciele wszystkich trwałych w wykazie. Właściciele zawartości nie można usunąć widoczności z wykazu Administratorzy i Administratorzy mogą zarządzać własności i widoczności dla wszystkich trwałych danych w katalogu.

## <a name="summary"></a>Podsumowanie
Wykaz danych modelu crowdsourcing metadanych i danych wykrywanie zasobów umożliwia użytkownikom wykaz wszystkich współtworzenia i Odkryj. Standard Edition wykazu danych zapewnia możliwości własności i zarządzania, aby ograniczyć widoczność oraz korzystanie z określonych danych składniki majątku.
