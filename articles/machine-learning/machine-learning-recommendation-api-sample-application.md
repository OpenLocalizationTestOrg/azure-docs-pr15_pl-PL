<properties 
    pageTitle="Typowe operacje w interfejsie API maszynowego uczenia zalecenia | Microsoft Azure" 
    description="Azure ML rekomendacji przykładowej aplikacji" 
    services="machine-learning" 
    documentationCenter="" 
    authors="LuisCabrer" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="luisca"/> 


# <a name="recommendations-api-sample-application-walkthrough"></a>Przewodnik po aplikacji przykładowych interfejsu API zalecenia

>[AZURE.NOTE]Należy zacząć korzystać z usługi poznawcze interfejsu API zalecenia zamiast tej wersji. Usługa poznawcze zalecenia będzie zamieniania tej usługi, a opracowane zostaną wszystkie nowe funkcje. Ma nowych funkcji, takich jak tworzeniu partii pomocy technicznej lepiej interfejsu API Eksploratora, oczyszczania środowisko tworzenia konta i rozliczenia powierzchniowy, bardziej spójny interfejs API, itp.
> Dowiedz się więcej na temat [migrowania do nowej usługi poznawcze](http://aka.ms/recomigrate)

##<a name="purpose"></a>Cel

Ten dokument zawiera zastosowania API Azure maszynowego uczenia zalecenia za pośrednictwem [aplikacji przykładowej](https://code.msdn.microsoft.com/Recommendations-144df403).

Ta aplikacja nie jest przeznaczona do uwzględnienia pełną funkcjonalność ani nie używa wszystkich interfejsów API. Jego pokazano kilka typowych operacji do wykonania, gdy najpierw ma być odtwarzany za pomocą usługi rekomendacji nauki komputera. 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

##<a name="introduction-to-machine-learning-recommendation-service"></a>Wprowadzenie do usługi rekomendacji nauki komputera

Zalecenia za pośrednictwem usługi rekomendacji nauki maszynowego są włączone, podczas tworzenia modelu rekomendacji na podstawie następujących danych:

* Repozytorium elementów, które chcesz polecić, nazywane także wykaz
* Dane reprezentującą zastosowania elementów dla poszczególnych użytkowników lub sesji (to mogą pochodzić z czasem za pośrednictwem uzyskiwanie danych nie jako część aplikacji przykładowych)

Po utworzeniu modelu rekomendacji umożliwia jego przewidywanie elementy, które użytkownik może być zainteresowanych, zgodnie z zestawu elementów (lub pojedynczego elementu) użytkownik wybiera.

Aby włączyć w poprzednim scenariuszu, wykonaj następujące czynności w usłudze rekomendacji nauki komputera:

* Tworzenie modelu: jest to logiczne kontener danych (wykazu i zastosowania) i modele przewidywania. Każdy kontener modelu jest identyfikowany przez unikatowy identyfikator przydzielonego po jego utworzeniu. Ten identyfikator jest nazywany identyfikator modelu i jest używany przez większość interfejsów API. 
* Przekazywanie do wykazu: po utworzeniu kontenera modelu można skojarzyć do niego wykazu.

**Uwaga**: tworzenia modelu i przekazywaniu z wykazem są zazwyczaj wykonywane po cyklu życia modelu.

* Przekazywanie zastosowania: powoduje dodanie do kontenera modelu danych dotyczących użycia.
* Tworzenie modelu zalecenie: po umieszczeniu wystarczającą ilość danych, można tworzyć modelu rekomendacji. Operacja używa górny algorytmów nauki Machine do tworzenia modelu rekomendacji. Każdy kompilacji jest skojarzony z Unikatowy identyfikator. Potrzebne do zaktualizowania rekordu tego Identyfikatora, ponieważ jest on wymagany dla funkcji niektóre funkcje interfejsu API.
* Monitorowanie procesu konstrukcyjnych: Tworzenie modelu rekomendacji jest operacja asynchroniczna i może potrwać od kilku minut do kilku godzin, w zależności od ilości danych (wykazu i zastosowania) i parametrów kompilacji. Dlatego konieczne jest monitorowanie kompilacji. Model rekomendacji jest tworzona tylko wtedy, gdy jego skojarzony kompilacji zakończy się pomyślnie.
* (Opcjonalnie) Wybierz pozycję Konstruuj modelu rekomendacji aktywne: ten krok jest tylko niezbędne, jeśli masz więcej niż jeden model rekomendacji wbudowany w kontenerze z modelu. Dowolnego żądania, aby uzyskać zalecenia bez wskazująca modelu rekomendacji aktywne jest automatycznie przekierowywane przez system do kompilacji aktywne domyślne. 

**Uwaga**: model rekomendacji aktywnego jest gotowa i jest ona wbudowana obciążenia pracą produkcji. To różni się od modelu rekomendacji-aktywny, który pozostaje w środowisku przypominających test (nazywane tymczasowego).

* Uzyskaj zalecenia: po umieszczeniu modelu rekomendacji, może spowodować zalecenia dotyczące pojedynczego elementu lub listę elementów, które można wybrać. 

Zazwyczaj wywoła uzyskać zalecenia przez pewien okres czasu. W tym okresie można przekierowywać danych dotyczących użycia systemu rekomendacji maszynowego uczenia doda te dane do kontenera określonego modelu. Jeśli masz wystarczającą ilość danych dotyczących użycia, można tworzyć nowe modelu rekomendacji, który zawiera dane użycia dodatkowych. 

##<a name="prerequisites"></a>Wymagania wstępne

* Visual Studio 2013
* Dostęp do Internetu 
* Subskrypcja zaleceń interfejsu API (https://datamarket.azure.com/dataset/amla/recommendations).

##<a name="azure-machine-learning-sample-app-solution"></a>Azure rozwiązanie w postaci maszynowego uczenia przykładowych aplikacji

To rozwiązanie zawiera kod źródłowy, Przykładowe użycie, plik wykazu i dyrektywy, aby pobrać pakiety, które są wymagane do kompilacji.

##<a name="the-apis-used"></a>Interfejsów API używanych

Aplikacja używa funkcji rekomendacji maszynowego uczenia za pośrednictwem podzbiór dostępnych interfejsów API. Wspomniane są następujące:

* Tworzenie modelu: Tworzenie kontenera logicznych, aby pomieścić modeli danych i rekomendacji. Model jest identyfikowany przez nazwę, a nie można utworzyć więcej niż jeden model o takiej samej nazwie.
* Przekaż plik katalogu: umożliwia przekazywanie wykazu danych.
* Przekaż plik zastosowania: umożliwia przekazywanie danych dotyczących użycia.
* Wyzwalanie kompilacji: służy do tworzenia modelu rekomendacji.
* Monitorowanie wykonanie kompilacji: monitorowanie stanu kompilacji modelu rekomendacji za pomocą.
* Wybierz nowy model zalecenie: umożliwia wskazanie model rekomendacji, który ma być używany domyślnie kontenera modelu. Ten krok jest konieczny tylko wtedy, gdy masz więcej niż jeden model zalecenia i chcesz aktywować kompilacji-aktywny, jako model rekomendacji aktywne.
* Uzyskiwanie zalecenie: pobieranie zalecane elementy według określonej pojedynczego elementu lub zestawu elementów za pomocą. 

Aby uzyskać pełny opis za pośrednictwem interfejsów API można znaleźć w dokumentacji usługi Microsoft Azure Marketplace. 

**Uwaga**: modelu może mieć kilka kompilacjach w czasie (nie jednocześnie). Każdy kompilacji zostanie utworzona i takie same lub zaktualizowanych wykazu i danych dotyczących użycia dodatkowych.

## <a name="common-pitfalls"></a>Typowych problemów

* Należy podać nazwę użytkownika i klucz podstawowe konto usługi Microsoft Azure Marketplace, aby uruchomić aplikację próbki.
* Uruchamianie aplikacji przykładowych kolejno zakończy się niepowodzeniem. Działaniem aplikacji zawiera tworzenia, przekazywanie, tworzenie monitor i uzyskiwanie zalecenia z modelu wstępnie zdefiniowane; dlatego jej zakończy się niepowodzeniem na wykonanie następujących po sobie Jeśli zmienisz nazwę modelu między wywołania.
* Zalecenia dotyczące może zwrócić bez danych. Aplikacja przykładowa używa bardzo mały plik wykazu i zastosowania. Nie zalecane elementy zostaną więc niektórych elementów z katalogu.

## <a name="disclaimer"></a>Zastrzeżenie
Aplikację próbki nie mają być uruchamiane w środowisku produkcyjnym. Danych zawartych w wykazie jest bardzo mały, a nie zapewni modelu rekomendacji opisową. Dane są przedstawione jako pokaz. 
 
