# <a name="azure-technical-documentation-contributor-guide"></a>Dokumentacja techniczna Azure przewodnik trybu współautora

Znalezieniu repozytorium GitHub, zawierającego źródło dokumentacji technicznych, w których jest opublikowany w [http://azure.microsoft.com/documentation](http://azure.microsoft.com/documentation)środku dokumentację Azure.

Ten repozytorium również zawiera wytyczne pomagające współtworzyć naszą dokumentację techniczną.  Aby uzyskać listę artykułów w podręczniku przez współautorów zapoznaj się [z indeksem](https://github.com/Azure/azure-content/blob/master/contributor-guide/contributor-guide-index.md).

## <a name="contribute-to-azure-documentation"></a>Współtworzenie się z dokumentacją Azure

Dziękujemy za zainteresowanie dokumentacji Azure!

* [Sposoby współtworzenia](#ways-to-contribute)
* [Kod postępowania](#code-of-conduct)
* [Informacje o programu Azure zawartości](#about-your-contributions-to-azure-content)
* [Organizacja repozytorium](#repository-organization)
* [Przy użyciu GitHub, cyfra i tego repozytorium](#use-github-git-and-this-repository)
* [Jak za pomocą promocji cenowych formatowanie temat](#how-to-use-markdown-to-format-your-topic)
* [Opinii, komentarze i pomocy technicznej](./contributor-guide/feedback-and-comments.md)
* [Więcej zasobów](#more-resources)
* [Indeks artykułów przewodnik dla wszystkich uczestników](./contributor-guide/contributor-guide-index.md) (powoduje otwarcie nowej strony)

## <a name="ways-to-contribute"></a>Sposoby współtworzenia 

Może przyczynić się do [dokumentacji Azure](http://azure.microsoft.com/documentation/) na kilka sposobów:

* Przyczynić się do [forum dyskusji](http://social.msdn.microsoft.com/Forums/windowsazure/home).
* Przesyłanie komentarzy Disqus u dołu artykuły.
* Łatwe może przyczynić się artykuły techniczne w interfejsie użytkownika GitHub. Znajdź artykuł z tego repozytorium lub zapoznaj się z artykułem na [http://azure.microsoft.com/documentation](http://azure.microsoft.com/documentation) i kliknij łącze w artykule prowadzące do źródła GitHub dla tego artykułu.
* Jeśli tworzysz istotnych zmian do istniejącego artykułu, dodawanie lub zmienianie obrazów lub tym utworzył nowy artykuł, musisz rozwidlenia tego repozytorium, instalowanie imprezie cyfra, konsoli promocji cenowych i Dowiedz się, niektóre polecenia cyfra.

##<a name="code-of-conduct"></a>Kod postępowania

Ten projekt przyjęła [Microsoft Otwórz źródło kodu postępowania](https://opensource.microsoft.com/codeofconduct/). Aby uzyskać więcej informacji, zobacz [Kod: przeprowadzanie — często zadawane pytania](https://opensource.microsoft.com/codeofconduct/faq/) lub kontakt [opencode@microsoft.com](mailto:opencode@microsoft.com) z odpowiedzi na dodatkowe pytania i komentarze.

##<a name="about-your-contributions-to-azure-content"></a>Informacje o programu Azure zawartości

###<a name="minor-corrections"></a>Nieznaczne korekty

Nieznaczne korekty lub wyjaśnienia przesyłanej dokumentacji i kod przykłady w tym repo są objęte [Azure witryny sieci Web warunki użytkowania (użytkowania)](http://azure.microsoft.com/support/legal/website-terms-of-use/).


###<a name="larger-submissions"></a>Większe przesłanych elementów

Po przesłaniu żądania pobieraj z nowych lub istotne zmiany w dokumentacji i przykłady kodu wyślemy komentarza w GitHub prośbą o przesyłanie online umowę licencyjną udział (CLA), jeśli znajdujesz się w jednej z tych grup:

* Członkowie grupy Otwórz technologii firmy Microsoft.
* Współpracowników, którzy nie działają w przypadku firmy Microsoft.

Administrator powinien wykonać formularz online, zanim firma Microsoft może zaakceptować Twoją prośbę pobieraj.

Szczegółowe informacje są dostępne w [http://azure.github.io/guidelines/#cla](http://azure.github.io/guidelines/#cla).

## <a name="repository-organization"></a>Organizacja repozytorium

Zawartość w repozytorium zawartości azure wykonuje organizacji dokumentacji na [Azure.Microsoft.com](http://azure.microsoft.com). To repozytorium zawiera dwa foldery główne:

### <a name="articles"></a>\articles

*\Articles* folder zawiera artykuły dokumentację w formacie promocji cenowych pliki z rozszerzeniem *.md* .

Artykuły w katalogu głównym publikowanych w Azure.Microsoft.com w ścieżce *http://azure.microsoft.com/documentation/articles/ {artykuł nazwa — bez md}-*.

* **Artykuł nazwy plików:** Zobacz [nasze nazw wskazówki plików](./contributor-guide/file-names-and-locations.md).

Artykuły w obrębie własnej folderu usługi publikowanych w Azure.Microsoft.com w ścieżce *http://azure.microsoft.com/documentation/articles/service-folder/ {artykuł nazwa — bez md}-*

* **Media podfoldery:** *\Articles* folder zawiera *\media* folder plików multimedialnych artykuł katalogu głównego, wewnątrz będących podfoldery z obrazami dla każdego artykułu.  Foldery usługi zawierają folder media osobnych artykuły w każdym folderze usługi. Artykuł folderów obrazów są nazywane identyczne do pliku artykuł minus rozszerzenie pliku *.md* .

### <a name="includes"></a>\Includes

Możesz utworzyć sekcje zawartości do ponownego użycia mają zostać uwzględnione w jeden lub więcej artykułów. Zobacz [Rozszerzenia niestandardowe używane w naszym techniczne zawartość](./contributor-guide/custom-markdown-extensions.md).

### <a name="markdown-templates"></a>Szablony \markdown

Ten folder zawiera naszych szablon standardowy promocji cenowych z formatowaniem podstawowe promocji cenowych, potrzebne do artykułu.

### <a name="contributor-guide"></a>\contributor-Guide

Ten folder zawiera artykułów, które są częścią przewodnika naszych współautorów.  

## <a name="use-github-git-and-this-repository"></a>Używanie GitHub, cyfra i tego repozytorium

Aby uzyskać informacje na temat przyczynić się, jak za pomocą interfejsu użytkownika GitHub Współtworzenie małych zmian i jak rozwidlenia i klonowanie repozytorium wpłaty bardziej znaczące, zobacz [Instalowanie i Konfigurowanie narzędzia do tworzenia w GitHub](./contributor-guide/tools-and-setup.md).

Jeśli po zainstalowaniu GitBash wybierz pozycję, aby pracować lokalnie, procedura tworzenia nowego lokalnego oddziału pracy, wprowadzanie zmian i przesłanie zmian z powrotem do głównego gałąź znajdują się w [cyfra polecenia służące do tworzenia nowego artykułu lub aktualizowania istniejącego artykułu](./contributor-guide/git-commands-for-master.md)

### <a name="branches"></a>Oddziałów

Zaleca się utworzenie lokalnego gałęziami pracy odwołujące się do określonego zakresu zmian. Gałęzie powinna być ograniczona do jednego koncepcji i artykułu zarówno w celu usprawnienia przepływu pracy i zmniejszanie możliwości konfliktów korespondencji seryjnej.  Odpowiedni zakres dla nowego oddziału są następujących działań:

* Nowy artykuł (i skojarzone obrazów)
* Edycja pisowni i gramatyki w artykule.
* Stosowanie jednego zmiany formatowania dużych zestawu artykułów (np. nowe stopka praw autorskich).

## <a name="how-to-use-markdown-to-format-your-topic"></a>Jak za pomocą promocji cenowych formatowanie temat

Wszystkie artykuły w tej repozytorium za pomocą GitHub flavored promocji cenowych.  Poniżej przedstawiono listę zasobów.

- [Podstawowe informacje o promocji cenowych](https://help.github.com/articles/markdown-basics/)

- [Cheatsheet drukowania promocji cenowych](./contributor-guide/media/documents/markdown-cheatsheet.pdf?raw=true)

- Nasze listy edytory promocji cenowych zobacz [Narzędzia i ustawienia tematu](./contributor-guide/tools-and-setup.md#install-a-markdown-editor).

## <a name="article-metadata"></a>Artykuł metadanych

Artykuł metadanych umożliwia niektórymi funkcjami w witrynie sieci web azure.microsoft.com, takich jak autor przyznawania przypisanie trybu współautora, łącza do stron nadrzędnych, opisy artykuł i funkcji optymalizacji aparatu wyszukiwania optymalizacje, a także raportowania programu Microsoft używa ma być obliczona wydajności zawartości. Ważne jest, metadane! [Poniżej przedstawiono wskazówki dotyczące upewnić się, że metadane odbywa się po prawej](./contributor-guide/article-metadata.md).

### <a name="labels"></a>Etykiety

Automatyczne etykiety są przypisywane aby uwzględniał żądania ułatwi zarządzanie przepływ pracy dla wniosku pobieraj i, aby informujące, co się dzieje z żądania pobieraj:

* Umowa licencyjna udział powiązanych
    * wymagane nie CLA: zmiana jest stosunkowo niewielki i nie wymaga logowania CLA.
    * wymagane CLA: zakres zmiana jest stosunkowo duży i wymaga zalogowania CLA.
    * podpisane CLA: Współautor podpisane CLA, więc żądania pobieraj możesz teraz przejść do przodu do recenzji.
* Filar etykiety: etykiety takie jak PnP, Nowoczesny aplikacje i TDC pomóc Kategoryzuj wezwań pobieraj przez organizację wewnętrzny, który należy sprawdzić żądanie pobieraj.
* Zmienianie wysyłane do Autor: Autor zostało powiadomione o żądaniu pobieraj oczekiwanie.

## <a name="more-resources"></a>Więcej zasobów

Zapoznaj się z [indeksu przewodnik naszych współautorów](./contributor-guide/contributor-guide-index.md) dla wszystkich tematów dotyczących naszych wskazówek.
