<properties 
    pageTitle="Jak używać wyników profilów w wyszukiwaniu Azure | Microsoft Azure | Usługa wyszukiwania hostowanej chmury" 
    description="Dostosowywanie wyszukiwania klasyfikowania za pośrednictwem wyników profile w wyszukiwaniu Azure, usługi wyszukiwania w chmurze obsługiwane w programie Microsoft Azure." 
    services="search" 
    documentationCenter="" 
    authors="HeidiSteen" 
    manager="mblythe" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="10/17/2016" 
    ms.author="heidist"/>

# <a name="how-to-use-scoring-profiles-in-azure-search"></a>Jak używać profile wyników wyszukiwania Azure

Profile punktowania są funkcją Microsoft Azure wyszukiwanie Dostosowywanie obliczeń w wyszukiwaniu, wpływające na jak są klasyfikowane elementów na liście wyników wyszukiwania. Możesz myśleć usunięto profile sposób istotności modelu przez zwiększenia elementy, które spełniają kryteria wstępnie zdefiniowanych. Załóżmy, że aplikacja jest witryna rezerwacji online hotel. Przez zwiększenia `location` pola wyszukiwania, które zawiera termin jak Seattle spowoduje wyższe wyniki dla elementów, które mają Seattle w `location` pola. Należy zauważyć, że można mieć więcej niż jeden profil wyników lub Brak, jeśli wyników domyślne wystarcza aplikacji.

Aby poeksperymentować z profilami wyników, możesz pobrać przykładową aplikację używa profilów wyników, aby zmienić kolejność klasyfikacji wyników wyszukiwania. Próbka jest jednak jako narzędzie nauki aplikacji konsoli — niekoniecznie bardzo rzeczywistych rozwoju rzeczywistych aplikacji — ale przydatne. 

Aplikacja przykładowa prezentuje zachowania wyników przy użyciu fikcyjnych danych o nazwie `musicstoreindex`. Prostota aplikacji przykładowych ułatwia modyfikowanie wyników profile i kwerendy, a następnie sprawdź bezpośredni wpływ na pozycję podczas wykonywania programu.

<a id="sub-1"></a>
## <a name="prerequisites"></a>Wymagania wstępne

Przykładowa aplikacja jest zapisywany w języku C# za pomocą programu Visual Studio 2013. Jeśli nie masz jeszcze kopii programu Visual Studio, spróbuj wykonać bezpłatne [Visual Studio 2013 Express edition](http://www.visualstudio.com/products/visual-studio-express-vs.aspx) .

Konieczne będzie subskrypcji usługi Azure i usługą Azure wyszukiwania do zakończenia tego samouczka. Aby uzyskać pomoc dotyczącą konfigurowania usługi, zobacz [Tworzenie usługi wyszukiwania w portalu](search-create-service-portal.md) .

[AZURE. Dołączanie [potrzebne jest konto Azure do użycia tego samouczka:](../../includes/free-trial-note.md)]

<a id="sub-2"></a>
## <a name="download-the-sample-application"></a>Pobieranie aplikacji przykładowej

Przejdź do [Pokaz profile wyników wyszukiwania Azure](https://azuresearchscoringprofiles.codeplex.com/) w witrynie codeplex, aby pobrać aplikację przykładowych opisanych w tym samouczku.

Na karcie Kod źródłowy kliknij przycisk **Pobierz** w celu otrzymania pliku zip rozwiązanie. 

 ![][12]

<a id="sub-3"></a>
## <a name="edit-appconfig"></a>Edytowanie app.config

1. Po wyodrębnieniu plików, otwórz rozwiązanie w programie Visual Studio do edycji pliku konfiguracji.
1. W oknie Eksplorator rozwiązań kliknij dwukrotnie **app.config**. W tym pliku określono punktu końcowego usługi i `api-key` używane do uwierzytelnienia wezwanie. Te wartości można uzyskać z poziomu portalu klasyczny.
1. Zaloguj się do [portalu Azure](https://portal.azure.com).
1. Przejdź do pulpitu nawigacyjnego usługi Azure wyszukiwania.
1. Kliknij pole **Właściwości** , aby skopiować adres URL usługi
1. Kliknij Kafelek **klawiszy** , aby skopiować `api-key`.

Po zakończeniu dodawania adresu URL i `api-key` app.config, ustawienia aplikacji powinna wyglądać następująco:

   ![][11]


<a id="sub-4"></a>
## <a name="explore-the-application"></a>Przegląd aplikacji

Prawie gotowe do tworzenie i uruchamianie aplikacji, ale przed wykonaniem, zapoznaj się z pliki JSON służące do tworzenia i wypełnij indeks.

**Schema.JSON** określa wskaźnika, w tym wyników profilów, które są wyróżniono w tym pokazie. Powiadomienie o schemat definiuje wszystkie pola używane w indeksie, w tym pola nie można wyszukiwać, takich jak `margin`, używanej w profilu wyników. Wyników profilu składnią opisano w [Dodaj wyników profilu do indeksu wyszukiwania Azure](http://msdn.microsoft.com/library/azure/dn798928.aspx).

**Data1 3.json** znajdują się dane, albumy 246 w kilku gatunki. Dane to kombinacja rzeczywistej albumu i wykonawcy informacje z fikcyjnej pól, takich jak `price` i `margin` użyta w celu przedstawienia operacji wyszukiwania. Pliki danych są zgodne z indeksem i są przekazywane do usługi Azure wyszukiwania. Po przekazaniu i indeksowane danych może wydawać się kwerendy przed nim.

**Plik program.cs** wykonuje następujące operacje:

- Zostanie otwarte okno konsoli.

- Łączy do wyszukiwania Azure za pomocą adresu URL usługi i `api-key`.

- Usuwa `musicstoreindex` Jeśli istnieje.

- Tworzy nowy `musicstoreindex` za pomocą pliku schema.json.

- Wypełnia indeksu przy użyciu plików danych.

- Kwerendę indeksu przy użyciu czterech kwerendy. Zwróć uwagę, że wyników profile są określane jako parametru zapytania. Wszystkie kwerendy wyszukiwania na ten sam okres "najlepsze". Pierwszej kwerendy przedstawiono domyślne wyników. Pozostałe trzy zapytań za pomocą wyników profilu.

<a id="sub-5"></a>
## <a name="build-and-run-the-application"></a>Tworzenie i uruchamianie aplikacji

Aby wykluczyć łączności lub zestawu odwołanie problemów, tworzenie i uruchomić aplikację, aby upewnić się, że występują problemy pracy najpierw. Powinien zostać wyświetlony aplikacji konsoli otwórz w tle. Wszystkie cztery kwerendy wykonaj kolejno bez przerw. W wielu systemach całego programu wykonuje w obszarze 15 sekundach. Jeśli aplikacja konsoli zawiera komunikat "wykonane. Naciśnij klawisz enter, aby kontynuować", zostanie ukończona pomyślnie. 

Aby porównać zostanie uruchomiona kwerenda, można znacznik, kopiowanie i wklejanie wyniki kwerendy z konsoli i wkleić je do pliku programu Excel. 

Na poniższej ilustracji przedstawiono wyniki z pierwszej trzech kwerend przez siebie. Wszystkie kwerendy za pomocą samego wyszukiwany termin, najlepiej, który pojawia się w wielu tytuły albumu.

   ![][10]

Pierwsze zapytanie używa domyślnego wyników. Ponieważ wyszukiwany termin jest wyświetlany tylko w albumie tytuły i określono żadnych innych kryteriów, mające "najlepsze" w tytule album są zwracane w kolejności, w której usługa wyszukiwania znajduje się je. 

Drugiego zapytania używa wyników profilu, ale należy zauważyć, że profil miał nie działa. Wyniki są identyczne z pierwszego zapytania. Jest to spowodowane wyników profilu zwiększa pola (gatunek), które nie jest germane do danej kwerendy. Wyszukiwany termin "najlepsze" nie istnieje w dowolnym polu "gatunek" dowolny dokument. Profil wyników jest ignorowany, wyniki są taki sam, jak domyślne wyników.  

Trzecia kwerenda jest pierwszym dowodów wyników wpływ profilu. Nadal najlepiej jest wyszukiwany termin, więc możemy współpracownikowi albumy ten sam zestaw, ale ponieważ punktowania profilu zawiera dodatkowe kryteria, które zwiększa "ocena" i "ostatniej aktualizacji", niektóre elementy są napędzany wyżej na liście.

Na następnej ilustracji przedstawiono kwerendy czwartej zwiększane przez "Marża". Pole "Marża" nie można wyszukiwać i nie może zostać zwrócony w wynikach wyszukiwania. Wartość "Marża" ręcznie został dodany do arkusza kalkulacyjnego, aby przedstawić punktu elementów z marginesami wyższą są wyświetlane wyżej na liście wyników wyszukiwania. 

   ![][9]

Teraz, gdy możesz mieć badawcze, mające z profilami wyników, spróbuj zmienić używana składnia innej kwerendy wyników profile lub bardziej rozbudowane danych. Łącza w następnej sekcji podano więcej informacji.

<a id="next-steps"></a>
## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej na temat tworzenia wyników profile. Aby uzyskać szczegółowe informacje, zobacz temat [Dodawanie wyników profilu do indeksu wyszukiwania Azure](http://msdn.microsoft.com/library/azure/dn798928.aspx) .

Dowiedz się więcej na temat składni i kwerend parametrów wyszukiwania. Aby uzyskać szczegółowe informacje, zobacz [Wyszukiwania dokumentów (Azure wyszukiwania interfejsu API usługi REST)](http://msdn.microsoft.com/library/azure/dn798927.aspx) .

Musisz krok wstecz i Dowiedz się więcej o tworzeniu indeksu? [Obejrzyj ten klip wideo](http://channel9.msdn.com/Shows/Cloud+Cover/Cloud-Cover-152-Azure-Search-with-Liam-Cavanagh) , aby zrozumieć podstawy.

<!--Anchors-->
[Prerequisites]: #sub-1
[Download the sample application]: #sub-2
[Edit app.config]: #sub-3
[Explore the application]: #sub-4
[Build and run the application]: #sub-5
[Next steps]: #next-steps

<!--Image references-->
[12]: ./media/search-get-started-scoring-profiles/AzureSearch_CodeplexDownload.PNG
[11]: ./media/search-get-started-scoring-profiles/AzureSearch_Scoring_AppConfig.PNG
[10]: ./media/search-get-started-scoring-profiles/AzureSearch_XLSX1.PNG
[9]: ./media/search-get-started-scoring-profiles/AzureSearch_XLSX2.PNG 