<properties
    pageTitle="Korzystanie z zestawów danych przykładowych w komputerze nauki Studio | Microsoft Azure"
    description="Opisy zestawów danych używane w modelach próbki zawarte w ML Studio. Za pomocą tych zestawów danych przykładowych do swojego doświadczeń."
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="garye"/>


# <a name="use-the-sample-data-sets-in-azure-machine-learning-studio"></a>Korzystanie z zestawów danych przykładowych w Azure maszynowego uczenia Studio

[top]: #machine-learning-sample-datasets

Po utworzeniu nowego obszaru roboczego w Azure maszynowego uczenia różne zestawy danych przykładowych i doświadczeń są domyślnie. Wiele z tych zestawów danych przykładowych są używane przez model próbki w [Galerii analizy Cortana Azure](http://gallery.cortanaintelligence.com/), a inne są dołączone jako przykłady różnych typów danych, zwykle używanych w nauki komputera.

Niektóre z tych zestawów danych są dostępne w magazynie obiektów Blob platformy Azure. Dla tych zestawów danych w poniższej tabeli przedstawiono bezpośredni link. Te zestawy danych można korzystać w Twojej doświadczeniach, przy użyciu [Importowanie danych] [ import-data] modułu.

Pozostałą część tych zestawów danych przykładowych wymienione w obszarze **Zestawy danych zapisanych** w palecie modułu z lewej strony obszaru roboczego doświadczenia podczas otwierania lub utworzyć nowe doświadczenia w ML Studio.
Umożliwia dowolną z następujących zestawów danych w własne doświadczeniu, przeciągając ją do obszaru roboczego programu doświadczenia.


<!--
For a list of sample experiments available in ML Studio, see [Machine Learning Sample Experiments][sample-experiments].

[sample-experiments]: machine-learning-sample-experiments.md
-->

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


<table>

<tr>
  <th align=left>Nazwa zestawu danych</th>
  <th align=left>Opis zestawu danych</th>
</tr>

<tr>
  <td valign=top>Dorosłego klasyfikacji dwójkową dochód spisu zestawu danych</td>
  <td valign=top>
Podzestaw 1994 spisu bazy danych przy użyciu dorosłych pracy przez wieku 16 z indeksem skorygowane dochód > 100.<p> </p><b>Sposób użycia:</b> klasyfikowania pracowników za pomocą demograficzne do przewidywania, czy osoby uzyskuje ponad 50K w roku.<p> </p><b>Poszukiwanie pokrewne:</b> Kohavi, R., Becker, B., (1996). UCI maszynowego uczenia repozytorium <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Dla studentów Kalifornii, szkoły informacji i nauki komputera </td>
</tr>

<tr ID=airport-codes-dataset>
  <td valign=top>Zestaw danych kodów lotnisk</td>
  <td valign=top>
Kodów lotnisk w Stanach Zjednoczonych.<p> </p>Tego zestawu danych zawiera jeden wiersz dla każdego lotnisk Stanów Zjednoczonych, dostarczając numeru Identyfikacyjnego lotnisk i nazwę oraz lokalizację miasto i województwo.
  </td>
</tr>

<tr>
  <td valign=top>Cena samochodów danych (Raw)</td>
  <td valign=top>
Informacji na temat samochodów, wprowadź i modelu, w tym ceny, funkcje, takie jak liczba butli i MPG, a także wynik ubezpieczenia ryzyka.<p> </p>Ocena ryzyka początkowo skojarzonego z automatycznego ceny i wówczas ustawiany rzeczywiste ryzyka w procesie wiadomo, że aktuariuszy jako symboling. Wartość + 3 wskazuje, że automatyczne jest ryzykowna, i wartość -3 że jest prawdopodobnie bardzo bezpieczne.<p> </p><b>Sposób użycia:</b> przewidywanie wynik ryzyka przez funkcje za pomocą regresji lub multivariate klasyfikacji. <p> </p><b>Poszukiwanie pokrewne:</b> Schlimmer, J.C. (1987). UCI maszynowego uczenia repozytorium <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Dla studentów Kalifornii, szkoły informacji i nauki komputera </td>
</tr>

<tr ID=bike-rental-uci-dataset>
  <td valign=top>Zestaw danych UCI dzierżawa rowerowe</td>
  <td valign=top>
Dzierżawa roweru UCI zestaw danych na podstawie rzeczywistych danych od firmy Bikeshare kapitału, która obsługuje sieci dzierżawa roweru w stanie Waszyngton kontrolera domeny.<p> </p>Zestaw danych zawiera jeden wiersz dla każdej godziny każdego dnia w 2011 i 2012, łącznie 17,379 wierszy. Zakres co godzinę roweru dzierżawy trwa od 1 do 977.

  </td>
</tr>

<tr ID=bill-gates-rgb-image>
  <td valign=top>Bill Gates obraz RGB</td>
  <td valign=top>
Plik obrazu publicznie dostępne są konwertowane na dane w formacie CSV.<p> </p>Kod do konwersji obrazu znajduje się na stronie szczegółów model <strong>kolorów quantization przy użyciu klaster oznacza K</strong> .
  </td>
</tr>

<tr>
  <td valign=top>Krwi darowizny danych</td>
  <td valign=top>
Podzestaw danych z bazy danych dawcy krwi Chu Centrum Hsin usługi Miasto, Tajwanu transfuzję krwi.<p> </p>Dawcy danych znajduje się miesiące od czasu ostatniego darowizny) i częstotliwość lub liczba pobrań, czas od ostatniej darowizny i kwota krwi Datki.<p> </p><b>Sposób użycia:</b> celem jest przewidywanie za pośrednictwem klasyfikacji, czy dawcy Datki krwi w marcu 2007, gdzie 1 wskazuje ofiarodawcę okresie docelowej i 0 nie dawcy. <p> </p><b>Poszukiwanie pokrewne:</b> Yeh, I.C., (2008). UCI maszynowego uczenia repozytorium <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Dla studentów Kalifornii, szkoły informacji i nauki komputera <p> </p>Yeh,-Cheng Korzun, Jang króla i notatki, naciśnij-Ming "odnajdowania wiedzy na modelu funkcjonalności za pomocą sekwencji Bernoulliego" systemy eksperta z aplikacjami, 2008, <a href="http://dx.doi.org/10.1016/j.eswa.2008.07.018">http://dx.doi.org/10.1016/j.eswa.2008.07.018</a>
  </td>
</tr>

<tr ID=book-reviews-from-amazon>
  <td valign=top>Przeglądy książki z Amazon</td>
  <td valign=top>
Przeglądy książek w Amazon podjętych w witrynie sieci Web amazon.com przez pracowników badawczych dla studentów Pennsylvania (<a href="http://www.cs.jhu.edu/~mdredze/datasets/sentiment/">upodobania</a>). Zobacz dokument badań "Biographies, Bollywood, wysięgnik pól i mieszalni: dostosowanie domeny upodobania klasyfikacji" Blitzer Jan, oznacz Dredze i Fernando Pereira; Skojarzenie lingwistyczne obliczeniowe (ACL) 2007.<p> </p>Oryginalny zestaw danych zawiera przeglądy 975K o klasyfikacji 1, 2, 3, 4 i 5. Przeglądy zostały napisane w języku angielskim i pochodzą z przedziału czasu 1997-2007. Tego zestawu danych zostało pobrane w dół do recenzji 10K.
  </td>
</tr>

<tr>
  <td valign=top>Dane raka matki</td>
  <td valign=top>
Jeden z trzech zestawy dotyczącą raka danych, dostarczonych przez Instytut Oncology często występującego w materiały szkoleniowe komputera. Łączy informacje diagnostyczne z funkcjami analizy laboratoryjnej hodowlami około 300.<p> </p><b>Sposób użycia:</b> klasyfikować rodzaj raka, w zależności od 9 atrybuty, niektóre z nich są liniowe i niektóre są kategorii. <p> </p><b>Poszukiwanie pokrewne:</b> O.L. Wohlberg, W.H., ulicy, W.N. i Mangasarian, (1995). UCI maszynowego uczenia repozytorium <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Dla studentów Kalifornii, szkoły informacji i nauki komputera </td>
</tr>

<tr ID=breast-cancer-features>
  <td valign=top>Funkcje raka matki <td valign=top>
Zestaw danych zawiera informacje regionów podejrzanych 102K (kandydatów) obrazy rentgenowskie każdy opisany przez funkcje 117. Funkcje są unikatowe i ich znaczenie nie jest wynika z twórcami zestawu danych (Siemens służba zdrowia). 
  </td>
</tr>

<tr ID=breast-cancer-info>
  <td valign=top>Informacje o raka matki</td>
  <td valign=top>
Zestaw danych zawiera dodatkowe informacje dla każdego regionu podejrzanych rentgenowskie obrazu. Każdy przykład informacje (np etykietę, pacjentów ID, współrzędne poprawki względem całego obrazu) o numer wiersza w zestawie danych funkcje raka matki. Każdy pacjentów ma wiele przykładów. Dla pacjentów, którzy mają raka kilka przykładów jest dodatnia, a niektóre są ujemne. W przypadku pacjentów, którzy nie mają raka, we wszystkich przykładach są ujemne. Zestaw danych zawiera przykłady 102K. Obciążona zestawu danych, 0,6% punktów jest dodatnia, pozostałe są ujemne. Służba zdrowia Siemens przypisano dostępne zestawu danych.
  </td>
</tr>

<tr ID=crm-appetency-labels-shared>
  <td valign=top>Etykiety Appetency CRM udostępnionych</td>
  <td valign=top>
Etykiety z wezwanie przewidywania relacji klienta 2009 Filiżanka KDD (<a href="http://www.sigkdd.org/site/2009/files/orange_small_train_appetency.labels">orange_small_train_appetency.labels</a>).
  </td>
</tr>

<tr ID=crm-churn-labels-shared>
  <td valign=top>Etykiety pochodząca CRM udostępnionych</td>
  <td valign=top>
Etykiety z wezwanie przewidywania relacji klienta 2009 Filiżanka KDD (<a href="http://www.sigkdd.org/site/2009/files/orange_small_train_churn.labels">orange_small_train_churn.labels</a>).
  </td>
</tr>

<tr ID=crm-dataset-shared>
  <td valign=top>Zestaw danych CRM udostępnionych</td>
  <td valign=top>
Te dane pochodzi z wezwanie przewidywania relacji klienta 2009 Filiżanka KDD (<a href="http://www.sigkdd.org/kdd-cup-2009-customer-relationship-prediction - orange_small_train.data.zip">orange_small_train.data.zip</a>).<p> </p>Zestaw danych zawiera 50K klientów firmy Telecom francuski pomarańczowy. Każdy klient ma 230 funkcje zastąpione anonimowymi 190, które są liczbowe i 40 są kategorii. Funkcje są bardzo rzadkie.
  </td>
</tr>

<tr ID=crm-upselling-labels-shared>
  <td valign=top>Etykiety grup CRM udostępnionych</td>
  <td valign=top>
Etykiety z wezwanie przewidywania relacji klienta 2009 Filiżanka KDD (<a href="http://www.sigkdd.org/site/2009/files/orange_large_train_upselling.labels">orange_large_train_upselling.labels</a>).
  </td>
</tr>

<tr>
  <td valign=top>Dane regresji efektywności energetycznej</td>
  <td valign=top>
Kolekcja profile symulowany energii, na podstawie 12 kształtów innego budynku. Budynki różnią się w odniesieniu do zróżnicowanych według 8 funkcje, takie jak szyb obszar, rozkład obszar szyby i orientację.<p> </p><b>Sposób użycia:</b> przewidywanie efektywności energetycznej ocena zależności w jednym z dwóch odpowiedzi rzeczywistych wartości za pomocą regresji lub klasyfikacji. Wiele klasy klasyfikacji w przypadku zaokrąglanie zmiennej odpowiedzi do najbliższej liczby całkowitej. <p> </p><b>Poszukiwanie pokrewne:</b> Xifara, odpowiedzi i Tsanas, A. (2012). UCI maszynowego uczenia repozytorium <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Dla studentów Kalifornii, szkoły informacji i nauki komputera </td>
</tr>

<tr ID=flight-delays-data>
  <td valign=top>Opóźnienia lotów danych</td>
  <td valign=top>
Dane dotyczące wydajności na czas z lotu osobowych pobierany ze zbierania danych TranStats USA dział transportu (<a href="http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time">W czasie</a>).<p> </p>Zestaw danych obejmuje okres kwiecień-października 2013. Przed przekazaniem do Azure ML Studio zestawu danych został przetwarzana w następujący sposób:<ul><li>Zestaw danych został przefiltrowany dotyczyć tylko 70 Obciążenie najbardziej zajętej lotnisk kontynentalna część USA</li><li>Anulowane lotów zostały oznaczone jako opóźnione o więcej niż 15 minut</li><li>Zostały odfiltrowane w kierunku lotów</li><li>Wybrano następujące kolumny: roku, miesiąca, DayofMonth, DayOfWeek, przewoźnika, OriginAirportID, DestAirportID, CRSDepTime, DepDelay, DepDel15, CRSArrTime, ArrDelay, ArrDel15 i odwołania</li></ul>
</td>
</tr>

<tr>
  <td valign=top>Wydajność czasowa lotu (Raw)</td>
  <td valign=top>
Rekordy samolotem lotu przylotów i wylotów w Stanach Zjednoczonych z październik 2011.<p> </p><b>Sposób użycia:</b> przewidywanie opóźnienia lotów. <p> </p><b>Poszukiwanie pokrewne:</b> od działu USA z transportem <a href="http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time">http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time</a>.
  </td>
</tr>

<tr>
  <td valign=top>Las uruchamiana danych</td>
  <td valign=top>
Zawiera dane, takie jak wskaźniki temperatury i wilgotności i prędkość wiatru z obszaru północno-wschodnich Portugalia, połączone z rekordami uruchamiany las.<p> </p><b>Sposób użycia:</b> jest zadaniem trudne regresji miejsce, w którym celem jest przewidywanie obszaru nagrany uruchamiany las. <p> </p><b>Poszukiwanie pokrewne:</b> Cortez, P. i Morais, A. (2008). UCI maszynowego uczenia repozytorium <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Dla studentów Kalifornii, szkoły informacji i nauki komputera <p> </p>[Cortez i Morais, 2007] P. Cortez i A. Morais. Dane podejście wyszukiwania do prognozowania las uruchamiany przy użyciu danych meteorologicznych. W J. Neves, M. F. Santos i J. Machado Eds., nowy trendów w sztucznej analiz postępowania 13 2007 EPIA — portugalski konferencji sztucznego analiz grudnia, Guimarães, Portugalia, str. 512-523, 2007. APPIA, ISBN-13 978-989-95618-0 – 9. Dostępnego pod adresem: <a href="http://www.dsi.uminho.pt/~pcortez/fires.pdf">http://www.dsi.uminho.pt/~pcortez/fires.pdf</a>.
  </td>
</tr>

<tr ID=german-credit-card-uci-dataset>
  <td valign=top>Niemiecki karty kredytowej UCI zestawu danych</td>
  <td valign=top>
Statlog UCI (karta kredytowa niemiecki) zestawu danych (<a href="http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)">Statlog + niemiecki + kredytowej + danych</a>), za pomocą pliku german.data.<p> </p>Zestaw danych klasyfikuje osób opisanego przez zestaw atrybutów, jak małego lub dużego ryzyka. Każdy przykład reprezentuje osoby. Istnieją funkcje 20, zarówno danych liczbowych oraz kategorii i etykietę binarne (wartość ryzyka kredytowej). Wysoka kredytowej ryzyka zapisów etykieta = 2, niska kredytowej ryzyka zapisów Etykieta = 1. Koszt misclassifying przykład niskiego ryzyka, jak duży jest 1, koszt misclassifying przykład wysokim ryzykiem jako niski jest 5.
  </td>
</tr>

<tr ID=imdb-movie-titles>
  <td valign=top>ZAUFANI tytułów filmów</td>
  <td valign=top>
Zestaw danych zawiera informacje na temat filmów, które zostały sklasyfikowane w serwisie Twitter tweety: ZAUFANI filmu ID, nazwę filmu i gatunek, roku produkcji. Istnieje filmy 17K w zestawie danych. Zestaw danych został wprowadzony w papieru "S. Dooms, T. De Pessemier i Martens L. MovieTweetings: filmu ocena zestawu danych zebrane z Twitter. Warsztat na Crowdsourcing i ludzi obliczeń systemów Recommender CrowdRec na RecSys 2013".
  </td>
</tr>

<tr>
  <td valign=top>Dane klasy soczewki dwóch</td>
  <td valign=top>
Być może jest znaną bazy danych można znaleźć w materiały rozpoznawania deseniem. Zestaw danych jest niewielka, zawierający 50 przykłady pomiarów Motyw Płatek z trzech odmian soczewki.<p> </p><b>Sposób użycia:</b> przewidywanie typ soczewki z wymiarów.  <p> </p><b>Poszukiwanie pokrewne:</b> ROZKŁAD.FISHER, R.A. (1988). UCI maszynowego uczenia repozytorium <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Dla studentów Kalifornii, szkoły informacji i nauki komputera </td>
</tr>

<tr ID=movie-tweets>
  <td valign=top>Tweety filmu</td>
  <td valign=top>
Zestaw danych jest to rozszerzona wersja zestawu danych Tweetings filmu. Zestaw danych zawiera oceny 170K w przypadku filmów należy wyodrębnionych z dobrze tweety w serwisie Twitter. Każde wystąpienie przedstawia tweet i jest krotki: identyfikator użytkownika, identyfikator filmu ZAUFANI, klasyfikacji, sygnatura czasowa, numer Ulubione dla tego tweet i liczba retweets tym serwisie Twitter. Zestaw danych została udostępniona A. Said, S. Dooms, B. Loni i D. Tikk Recommender systemów wezwanie w 2014 r.
  </td>
</tr>

<tr>
  <td valign=top>MPG danych dla różnych samochodów</td>
  <td valign=top>
Tego zestawu danych jest nieco zmodyfikowaną wersję zestawu danych dostępne w bibliotece StatLib Carnegie Mellon University. Zestaw danych użyto w 1983 American statystyczne skojarzenia specyfikacji.<p> </p>Zużycie paliwo dla różnych samochodów km / galon wraz z informacjami takie liczbę butli, przesunięcie aparat Angielski koń parowy, waga całkowita i przyspieszenia wykazy danych.<p> </p><b>Sposób użycia:</b> przewidywanie zużycia paliwo na podstawie 3 wielowartościowe atrybuty osobne i 5 atrybuty ciągły. <p> </p><b>Poszukiwanie pokrewne:</b> StatLib, Carnegie Mellon University (1993). UCI maszynowego uczenia repozytorium <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Dla studentów Kalifornii, szkoły informacji i nauki komputera </td>
</tr>

<tr>
  <td valign=top>Zestaw danych Pima Indian cukrzyca binarne klasyfikacji</td>
  <td valign=top>
Podzestaw danych z krajowych Instytut cukrzyca i przewód i choroby nerek bazy danych. Zestaw danych został przefiltrowany skoncentrować się na żeńskich pacjentów dziedzictwa indyjskich Pima. Dane zawierają medyczny danych, takich jak glukozy i inulinowego poziomów, a także czynników stylu życia.<p> </p><b>Sposób użycia:</b> przewidywanie, czy podmiot ma cukrzyca (Klasyfikacja binarne). <p> </p><b>Poszukiwanie pokrewne:</b> Sigillito przeciw (1990). UCI maszynowego uczenia repozytorium <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml "</a>. Irvine, CA: Dla studentów Kalifornii, szkoły informacji i nauki komputera </td>
</tr>

<tr>
  <td valign=top>Dane klientów restauracji</td>
  <td valign=top>
Zestaw metadanych dotyczących klientów, w tym demograficzne i preferencji.<p> </p><b>Sposób użycia:</b> za pomocą tego zestawu danych w połączeniu z innymi restauracji dwa zestawy danych, szkolenie i przetestowanie systemu recommender. <p> </p><b>Poszukiwanie pokrewne:</b> Bache, K. i Lichman, M. (2013). UCI maszynowego uczenia repozytorium <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Dla studentów Kalifornii, szkoły informacje i nauki komputera.
  </td>
</tr>

<tr>
  <td valign=top>Danych funkcji restauracji</td>
  <td valign=top>
Zestaw metadanych dotyczących restauracje i ich funkcje, takie jak żywność typ, styl miejsc spożywania i lokalizacji.<p> </p><b>Sposób użycia:</b> za pomocą tego zestawu danych w połączeniu z innymi restauracji dwa zestawy danych, szkolenie i przetestowanie systemu recommender. <p> </p><b>Poszukiwanie pokrewne:</b> Bache, K. i Lichman, M. (2013). UCI maszynowego uczenia repozytorium <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Dla studentów Kalifornii, szkoły informacje i nauki komputera.
  </td>
</tr>

<tr>
  <td valign=top>Oceny restauracji</td>
  <td valign=top>
Zawiera oceny podane przez użytkowników dla restauracji w skali od 0 do 2.<p> </p><b>Sposób użycia:</b> za pomocą tego zestawu danych w połączeniu z innymi restauracji dwa zestawy danych, szkolenie i przetestowanie systemu recommender. <p> </p><b>Poszukiwanie pokrewne:</b> Bache, K. i Lichman, M. (2013). UCI maszynowego uczenia repozytorium <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Dla studentów Kalifornii, szkoły informacje i nauki komputera.
  </td>
</tr>

<tr>
  <td valign=top>Stali zestawu danych z wielu klasy Annealing</td>
  <td valign=top>
Tego zestawu danych zawiera szereg rekordy z stali termiczne odprężanie prób fizycznie atrybutów (szerokość, grubość, typów typu (cewki, Arkusz itp.) wynikowym stali.<p> </p><b>Sposób użycia:</b> przewidywanie dowolnej z dwóch atrybutów klasy liczbowe; twardość lub siły. Może też analizowanie korelacji między atrybutów.<p> </p>Stali wykonaj standardowy zestaw zdefiniowanych przez SAE i innych organizacji. Szukasz konkretnej "Klasa" (zmienna klasy) i chcesz, aby zrozumieć wartości wymagane. <p> </p><b>Poszukiwanie pokrewne:</b> szterlingi, D. i Buntine W. (Brak). UCI maszynowego uczenia repozytorium <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Dla studentów Kalifornii, szkoły informacji i nauki komputera <p> </p>Przewodnik przydatne do stali można znaleźć tutaj: <a href="http://www.outokumpu.com/SiteCollectionDocuments/Outokumpu-steel-grades-properties-global-standards.pdf">http://www.outokumpu.com/SiteCollectionDocuments/Outokumpu-steel-grades-properties-global-standards.pdf</a>
  </td>
</tr>

<tr>
  <td valign=top>Dane teleskopu</td>
  <td valign=top>
Rekordów wysokiej energii gamma cząstek seria wraz z szum w tle, oba symulowane przy użyciu Monte Carlo procesu.<p> </p>Zamiarem symulacji było zwiększyć dokładność funkcji opartych na ziemi powietrza Cherenkov gamma teleskopy, przy użyciu metod statystycznych do rozróżniania odpowiedniej sygnału (promieniowania Cherenkov natryski) oraz szum w tle (hadronic prysznice zainicjowana promieniami cosmic w górnym atmosfery).<p> </p>Dane zostały wstępnie przetworzone na tworzenie wydłużona klaster długie osi jest ustawiony w kierunku do środka kamery. Właściwości tego elipsa (często nazywane parametrów Hillas) należą parametrów obrazu, które mogą być używane do dyskryminacji.<p> </p><b>Sposób użycia:</b> przewidywanie, czy obraz przyjęcie reprezentuje szum w tle lub sygnału.<p> </p><b>Uwagi:</b> dokładność prostej klasyfikacji nie ma znaczenie dla tych danych od klasyfikacji zdarzenia tło, jak sygnału jest większa niż klasyfikacji zdarzenie sygnału jako tła. Porównanie różnych klasyfikatorów na wykresie ROC powinien być używany. Prawdopodobieństwo akceptować zdarzenia tła jako sygnału musi być poniżej jednego z następujących progów: 0,01, 0,02, 0,05, 0,1 lub 0,2.<p> </p>Należy również zauważyć, że liczba zdarzeń tła (h, natryski hadronic) jest zaniżone, na rzeczywiste wymiary klasy h lub hałasu stanowi większość zdarzeń. <p> </p><b>Poszukiwanie pokrewne:</b> Bock, R.K. (1995). UCI maszynowego uczenia repozytorium <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Dla studentów Kalifornia, szkoły informacji </td>
</tr>

<tr ID=weather-dataset>
  <td valign=top>Zestaw danych pogody</td>
  <td valign=top>
Co godzinę uwag naziemnej pogody NOAA (<a href="http://cdo.ncdc.noaa.gov/qclcd_ascii/, merged data from 201304 to 201310">scalonych danych z 201304 do 201310</a>).<p> </p>Dane obejmują uwagi utworzonego na podstawie stacji pogody lotnisk, obejmujące okres kwiecień-października 2013. Przed przekazaniem do Azure ML Studio zestawu danych został przetwarzana w następujący sposób:<ul><li>Identyfikatory stacji pogody zostały zamapowane na odpowiadające im lotnisk identyfikatory</li><li>Nie jest skojarzony z 70 lotnisk Obciążenie najbardziej zajętej stacji pogody zostały odfiltrowane.</li><li>Kolumna Data podzielono na osobne kolumny rok, miesiąc i dzień</li><li>Wybrano następujące kolumny: AirportID, rok, miesiąc, dzień, godzina, strefy czasowej, SkyCondition, widoczność, WeatherType, DryBulbFarenheit, DryBulbCelsius, WetBulbFarenheit, WetBulbCelsius, DewPointFarenheit, DewPointCelsius, RelativeHumidity, prędkość wiatru, WindDirection, ValueForWindCharacter, StationPressure, PressureTendency, PressureChange, SeaLevelPressure, RecordType, HourlyPrecip, wysokościomierza</li></ul>
  </td>
</tr>

<tr ID=wikipedia-sp-500-dataset>
  <td valign=top>Wikipedia SP 500 zestawu danych</td>
  <td valign=top>
Dane pochodzi z serwisu Wikipedia (<a href="http://www.wikipedia.org/">http://www.wikipedia.org/</a>) oparte na artykuły każdej S & P 500 firmy przechowywane jako danych XML.<p> </p>Przed przekazaniem do Azure ML Studio zestawu danych został przetworzony w następujący sposób:<ul><li>Wyodrębnianie zawartości tekstowej dla każdej firmy</li><li>Usuwanie formatowania typu wiki</li><li>Usuń znaki niealfanumeryczne</li><li>Konwertowanie całego tekstu na małe litery.</li><li>Dodano kategorii znanej firmy</li></ul><p> </p>Nie można odnaleźć należy zauważyć, że niektóre firmy artykułu, więc liczby rekordów jest mniejsze niż 500.
  </td>
</tr>





<tr ID=direct-marketing>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/direct_marketing.csv">direct_marketing.csv</a></td>
  <td valign=top>
Zestaw danych zawiera dane klienta i wskazówki dotyczące swoją odpowiedź do bezpośredniego kampanii. Każdy wiersz reprezentuje klienta. Zestaw danych zawiera funkcje 9 o demograficzne użytkownika oraz zachowanie i 3 etykietę kolumny (odwiedź konwersji i poświęcają).  Odwiedź witrynę jest kolumnę danych binarnych, która wskazuje, że klient odwiedzonej po kampanii marketingowej, konwersji wskazuje klienta zakupionych coś, poświęcają jest kwota, która została wydana.  Zestaw danych została udostępniona przez Hillstrom Tomasz dla MineThatData E-Mail analizy i danych wyszukiwania wezwanie.
  </td>
</tr>

<tr ID=lyrl2004-tokens-test>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/lyrl2004_tokens_test.csv">lyrl2004_tokens_test.csv</a></td>
  <td valign=top>
Funkcje przykładów test w zestawie danych wiadomości Reuters RCV1 w wersji 2. Zestaw danych zawiera artykuły 781K wraz z ich identyfikatory (pierwszej kolumny zestawu danych). Każdy artykuł jest plikach stopworded i była raczej. Zestaw danych została udostępniona przez David. D. Nowak.
  </td>
</tr>

<tr ID=lyrl2004-tokens-train>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/lyrl2004_tokens_train.csv">lyrl2004_tokens_train.csv</a></td>
  <td valign=top>
Funkcje przykładów szkoleniowych w zestawie danych wiadomości Reuters RCV1 w wersji 2. Zestaw danych zawiera artykuły 23K wraz z ich identyfikatory (pierwszej kolumny zestawu danych). Każdy artykuł jest plikach stopworded i była raczej. Zestaw danych została udostępniona przez David. D. Nowak.
  </td>
</tr>

<tr ID=intrusion-detection>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/network_intrusion_detection.csv">network_intrusion_detection.csv</a><br></td>
  <td valign=top>
Zestaw danych z KDD Cup 1999 wiedzy odnajdowania i wyszukiwania narzędzia konkurencji (<a href="http://kdd.ics.uci.edu/databases/kddcup99/kddcup99.html">kddcup99.html</a>) danych.<p> </p>Zestaw danych został pobrany i przechowywane w magazynie obiektów Blob platformy Azure (<a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/network_intrusion_detection.csv">network_intrusion_detection.csv</a>) i zawiera zarówno szkoleń i testowania zestawy danych. Szkolenie dotyczące zestawu danych ma około 126K kolumn i wierszy 43, łącznie z etykietami; 3 kolumny są częścią informacji z etykiety, a 40 kolumny, składający się z funkcji liczbowych i ciąg/kategorii, są dostępne dla szkolenia modelu. Dane z badań ma około 22,5 K testowania przykładów z 43 kolumn tak jak dane szkolenia.

  </td>
</tr>

<tr ID=rcv1-v2-topics-qrels>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/rcv1-v2.topics.qrels.csv">rcv1 v2.topics.qrels.csv</a></td>
  <td valign=top>
Temat przydziałów dla artykuły w zestawie danych wiadomości Reuters RCV1 w wersji 2. Artykuł informacyjny można przypisywać do kilku tematów. Format każdy wiersz jest "<topic name>  <document id> 1". Zestaw danych zawiera 2.6M tematu przydziałów. Zestaw danych została udostępniona przez David. D. Nowak.
  </td>
</tr>

<tr ID=student-performance>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/student_performance.txt">student_performance.txt</a></td>
  <td valign=top>
Te dane pochodzi z wezwanie oceny wydajności KDD Cup 2010 uczniów (<a href="http://www.kdd.org/kdd-cup-2010-student-performance-evaluation">oceny działania uczniów</a>). Dane użyte jest zestaw szkoleniowy Algebra_2008_2009 (Stamper, J., Niculescu-Mizil, S. A., Ritter, Gordon, G.J. i Koedinger, K.R. (2010). Algebraiczną I 2009 roku 2008. Test z zestawem danych KDD Cup 2010 edukacyjnych danych wyszukiwania wezwanie. Znajdowanie go w <a href="http://pslcdatashop.web.cmu.edu/KDDCup/downloads.jsp">downloads.jsp</a> lub <a href="http://www.kdd.org/sites/default/files/kddcup/site/2010/files/algebra_2008_2009.zip">algebra_2008_2009.zip</a>.<p> </p>Zestaw danych został pobrany i przechowywane w magazynie obiektów Blob platformy Azure (<a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/student_performance.txt">student_performance.txt</a>) i zawiera pliki dziennika uczeń Korepetycje systemu. Funkcji podanym zawiera identyfikator problemu i jego krótki opis, identyfikator uczniów, sygnatura czasowa i liczbę prób uczniów przed rozwiązywania problemu w prawo sposób. Oryginalny zestaw danych zawiera rekordy 8,9 M, tego zestawu danych zostało pobrane w dół do pierwszych wierszy 100 KB. Zestaw danych zawiera 23 kolumn tabulatorami różnych typów: numerycznym kategorii i sygnatura czasowa.

  </td>
</tr>




</table>


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
