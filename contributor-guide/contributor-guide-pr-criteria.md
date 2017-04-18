# <a name="quality-criteria-for-pull-request-review"></a>Przejrzyj kryteria dotyczące jakości żądania pobieraj

Te kryteria są przeznaczone dla autorów, którzy tworzą i konserwują artykuły techniczne i pobieraj żądanie recenzentów, którzy Przeglądanie żądań pobieraj zawartości. Jeśli wezwanie pobieraj nie są objęte [automatycznego scalania](contributor-guide-pull-request-etiquette.md#in-a-hurry-submit-prs-that-can-be-accepted-automatically), będzie przeglądany przez ludzi pobieraj recenzenta żądania. Pobieraj żądanie recenzentów Przejrzyj zazwyczaj tylko co to jest nowe lub zmienione. Pobieraj żądanie recenzentów oceny zmian w żądanie pobieraj zgodnie z blokowaniem i bez blokowania elementów Recenzja jakości wymienione w tym artykule.

## <a name="blocking-content-quality-items"></a>Blokowanie zawartości jakości elementów

Aktualizacje w wezwaniu pobieraj muszą być zgodne z następującymi kryteriami do scalenia. Pobieraj żądanie recenzentów przekazywanie opinii w pobieraj żądanie komentarze dla tych elementów i typ `#hold-off` w wezwaniu pobieraj, aby przywrócić go do Ciebie (autor) opinii.

| Kategoria | Jakość przeglądanie elementu |
|----------|---------------------|
|Wymagania wstępne| "Gotowe do-korespondencji seryjnej" i "Sprawdzanie poprawności powiodło się" etykiety są przypisywane do PR.|
|Wymagania wstępne| Nie można zablokować żądania pobieraj konfliktem korespondencji seryjnej.|
|Integralność repo|    Żądanie pobieraj zawiera nie występują oczywiste strat zawartości.|
|Integralność repo|    Pobieraj żądanie nie zawiera osadzony repo lub nietypowe, zbędne pliki.|
|Integralność repo |Żądanie pobieraj zawiera mniej niż 100 plików zmienione, chyba że PR celowo aktualizuje oddział wersji ze wzorca. (Naprawdę, cena powinna zawierać daleko mniej niż, ale po 100 zmienionych plików, GitHub nie są wyświetlane diffs).|
|Nadawanie nazw |Nazwy plików dla nowych plików wykonaj [wskazówki dotyczące nazewnictwa plików](file-names-and-locations.md).|
|Nadawanie nazw |Nowe foldery wprowadzać Obserwuj repo [wskazówki dotyczące nazewnictwa folderu](file-names-and-locations.md#folder-names-in-the-repo).|
|Zawartość    |Artykuł jest dokumentem techniczne i w związku z tym poprawne zawartości kanału. Zobacz [co przeznaczonych wskazówki](content-channel-guidance.md).|
|Zawartość    |Przedmiotem dokumentu technicznego jest odpowiedni dla artykuł techniczny. Zobacz [co przeznaczonych wskazówki](content-channel-guidance.md).|
|Zawartość    |Ten artykuł zawiera akapit wprowadzający i proceduralnych lub koncepcyjna części zawartości. Artykuł musi zawierać pełną, wystarczające zawartości pozostawić własny jako artykułu. Nie należy małych fragment informacji. (Wyjątek: temat "Ograniczenia" znajduje się w kontekście dużych artykułu, który zawiera listę wszystkich ograniczenia usługi.)|
|Zawartość| Elementy, które powinny być nieuporządkowane list są listy punktowanej, elementy, które powinny być numerowane są numerowane. To jest podstawowy użyteczności.|
|Zawartość| Nietypowe lub nowej grafiki, architektura informacji lub struktur lub oczywiście niestandardowych projekty muszą być sprawdzane przy użyciu recenzenta PR potencjalnego klienta. Zespoły, które są eksperymentowanie z nowych elementów muszą mieć Kanwa problemu i rozwiązanie lub plan w celu oceny doświadczeń.|
|Projekt witryny i funkcje| Switchers są używane tylko w przypadku przełączania w różnych wersjach tego samego artykułu.|
|Projekt witryny i funkcje| Tytuły artykułów switchered zawierają informacje, które odróżnia każdy artykuł z innych artykułów w zestawie switchered.|
|Projekt witryny i funkcje| Ręcznie utworzony spisy treści nie są dozwolone. Artykuł musi Polegaj na H2s w celu jego spis treści na stronie.|
|Projekt witryny i funkcje| Jeśli istnieją H2 nagłówków, ten artykuł zawiera co najmniej dwoma nagłówkami H2. Za pomocą jednego nagłówek H2 tworzy artykuł jednoelementowego spisu treści. Nagłówki H2 należy przed nagłówki H3 mieć pewność, że jest tworzony spis treści.|
|Promocji cenowych| HTML: Źródło zawartości nie zawiera HTML na poziomie bloku — tekście pomocnicze, które HTML jest dozwolone — takie jak indeks górny, indeks dolny, znaków specjalnych i innymi pomocnicze, które nie można wykonać przy użyciu promocji cenowych. Tabel HTML są dozwolone tylko wtedy, gdy tabela zawiera list punktowanych lub numerowanych, ale zazwyczaj jest oznaczenie zawartości musi zostać uproszczone w celu źródła mogą być kodowane w promocji cenowych.|
|Promocji cenowych   |Niestandardowe promocji cenowych elementy są używane odpowiednio. Ex: Notatki są kodowane przy użyciu AZURE. Uwaga rozszerzenia, nie jako zwykły tekst.|
|FUNKCJA OPTYMALIZACJI APARATU WYSZUKIWANIA    |"& #124; Wymagane jest identyfikator witryny Microsoft Azure"|
|FUNKCJA OPTYMALIZACJI APARATU WYSZUKIWANIA    |Tytuł H1 zawiera wystarczających informacji w celu opisania zawartość tego artykułu, aby odróżnić go od innych artykułów Azure i do zamapować na słów kluczowych prawdopodobnych klienta. Na przykład "Przegląd" jako tytuł H1 jest błędu.|
|Terminologia| Korzystanie z akronim ARM, w wersji 1 lub w wersji 2 jako odwołania do klasycznego i modeli wdrażania Menedżera zasobów jest elementem terminologia blokowania.|
|Obrazy |Animowane pliki GIF nie są akceptowane do repo.|
|Obrazy | Obrazy wyczyść rozdzielczość, są błędnie napisane wyrazy i zawierają żadnych prywatnych informacji | 
|Organizowanie|Podgląd artykułu musi być przejrzysty na tymczasowego. Nie może zawierać dowolne oczywiste problemy związane z formatowaniem: <br><br>Listy numerowanej lub punktowanej, który jest wyświetlany jako akapitu <br> — Kod w blok kodu znajdujących się częściowo w bloku kodu i częściowo poza nią <br>-Ponumerowane kroki nieprawidłowy numer z powodu uszkodzony wcięcia|

## <a name="non-blocking-content-quality-items"></a>Blokowanie bez zawartości elementów jakości

Dla tych elementów recenzentów żądanie pobieraj Podaj opinii i instrukcje dotyczące autorowi Flaga monitująca przy użyciu poprawki późniejsze żądanie pobieraj. Jednak tych informacji nie blokuje decyzji o korespondencji seryjnej. Autorzy należy dalszych czynności do 3 dni roboczych przy użyciu poprawki.

| Kategoria | Jakość przeglądanie elementu |
|----------|---------------------|
|Zawartość|Artykuły powinny mieć "Następne kroki" na końcu z 1-3 odpowiednie i atrakcyjne następne kroki. Krótki tekst powinny podlegać pomagające klienta zrozumienie, dlaczego dotyczą następnych kroków. (Tylko nowych artykułów). Przykład: <https://azure.microsoft.com/en-us/documentation/articles/xplat-cli-install/><br>![](media/contributor-guide-pr-criteria/nextstepsexample.PNG)|
|Zawartość|Pisownia, gramatyka i innych problemów pisania - recenzentów żądanie pobieraj może przekazywać opinie na kilka drobne problemy jako bez blokowania opinii. W przypadku więcej niż kilka problemów redakcyjny recenzentów Zaloguj żądania edycji dla tego artykułu do edycji po opublikowaniu.|
|Obrazy|Obrazy poprawne objaśnienia styl i kolor, a zrzuty ekranu używasz poprawne stylu obramowania i symboli zastępczych. [Zobacz wskazówki obrazu](https://github.com/Azure/azure-content/blob/master/contributor-guide/create-images-markdown.md).|
|Obrazy|Obrazy zawierać tekst alternatywny. [Zobacz wskazówki obrazu](https://github.com/Azure/azure-content/blob/master/contributor-guide/create-images-markdown.md).|
|Projekt witryny i funkcje|Nagłówki H2 renderowane w spisie treści na stronie, najlepiej powinien być zawijany do linii nie więcej niż 2. Dłużej dzięki nagłówkom możesz trudniej skanowanie artykuł spisu treści.|
|Konwencje stylu|Wszystkie tytuły i nagłówki są zdaniu, na Azure styl.|
|Proces|Jeśli wezwanie pobieraj może mieć łatwo został ponownie skonfigurowany do korzystania z automatyzacji PRmerger, recenzentów żądanie pobieraj przekazywanie opinii do autora o używaniu gałęziami, więc zmiany mogą być scalane automatycznie. Zobacz [artykuł etykietą PR](https://github.com/Azure/azure-content/blob/master/contributor-guide/contributor-guide-pull-request-etiquette.md#in-a-hurry-submit-prs-that-can-be-accepted-automatically).|
|Proces|Gdy usunąć lub zmienić artykułu, upewnij się, że możesz wykonać instrukcje z procesu. Pobierają żądania recenzentów należy dodać poniższy komentarz i łącza w komentarzu:<br><br>*Sprawdź, czy zostały wykonane procesu w podręczniku przez współautorów usuwania artykułów: <https://github.com/Azure/azure-content/blob/master/contributor-guide/retire-or-rename-an-article.md> .*|

## <a name="related"></a>Powiązane

- [Pobieraj wezwanie na etykiecie i najważniejsze wskazówki dotyczące programu Microsoft współautorów](contributor-guide-pull-request-etiquette.md)

- [Pobierają żądanie automatyzacji komentarza](contributor-guide-pull-request-comments.md)
