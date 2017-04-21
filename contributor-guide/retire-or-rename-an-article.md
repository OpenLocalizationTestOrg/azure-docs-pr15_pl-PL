# <a name="steps-to-follow-when-you-retire-or-change-the-name-of-an-acom-technical-article"></a>Czynności, aby obserwować wycofać lub zmienić nazwę artykuł techniczny ACOM

Te wskazówki jest przeznaczony dla małych i średnich przedsiębiorstw, którzy są wyświetlane jako Autor artykułu, który ma być wycofana z sekcji dokumentacji technicznej azure.microsoft.com. Kroki też ubiegać się po zmianie nazwy pliku.

Jeśli jesteś członkiem naszej społeczności Azure i uważasz, że artykuł ma być wycofana dowolnego powodu, zostaw komentarz w strumieniu Disqus komentarza do tego artykułu umożliwić Autor wie, że czasami występują problemy z tego artykułu.

Autorzy małych muszą wykonać kilka czynności bezpiecznie zrezygnować zawartości, aby użytkownicy witryny sieci Web, które nie mają nieprawidłowe środowisko po możemy wycofać zawartość z witryny. Usuwanie tego artykułu lub zmieniając jego nazwy powinny być ostatnim, opcjonalnym się dzieje!

## <a name="step-1-set-the-article-to-no-indexno-follow-and-republish-it-recommended"></a>Krok 1: Ustaw artykuł nie obserwuj, nie indeksu w- i ponowne opublikowanie go (zalecane)

Najpierw musisz zrobić jest opublikować artykuł jako nie obserwuj, nie indeksu w-kilka tygodni przed usunięciem rzeczywistości. To jest traktowany jako najlepsze praktyki "przed pracy" dla emeryturę zawartości. W ten sposób usuwa artykuł z indeksy aparat wyszukiwania, aby inni użytkownicy mogą nie znaleźć artykuł w wyszukiwaniu. [Zobacz wewnętrznych typu wiki, aby uzyskać szczegółowe informacje.](https://microsoft.sharepoint.com/teams/azurecontentguidance/wiki/Pages/Remove%20published%20pages%20and%20request%20redirects.aspx)

## <a name="step-2-manage-inbound-links-required"></a>Krok 2: Zarządzanie łączami ruchu przychodzącego (wymagany)

Określa, czy łącza przychodzące firmy Microsoft do zawartości. Często blogi, fora i innej zawartości w sieci web wskazuje artykuły. Często możesz pracować z właściciele blogu, aby zmienić te łącza, a można usunąć lub Aktualizuj łącza forum wpisów. Narzędzia analizy sieci Web, można stwierdzić, czy występują problemy z łączach duży ruch przychodzący, potrzebne do zarządzania w ten sposób.

## <a name="step-3-remove-all-crosslinks-to-the-article-from-the-technical-content-repository-required"></a>Krok 3: Usuwanie wszystkich crosslinks do artykułu z techniczne repozytorium zawartości (wymagany)

Nie są oparte na przekierowaniami do przejmują crosslinks z innych artykułów. Aktualizowanie lub usuwanie krzyżyk odwołania do tego artykułu są usunięcie lub zmianę nazwy, łącznie w artykułach należące do innych osób.

1. Upewnij się, pracuje w aktualne gałęzi lokalne — Uruchom `git pull upstream master` (lub odpowiednią zmianę dla tego polecenia.

2.  Skanowanie folderu azure zawartości — pr artykuły i azure zawartości — pr zawiera folder dla każdego artykuły i zawiera łącze do tego artykułu, który chcesz wycofać, a albo usuń crosslinks lub zamień je odpowiednie crosslinks nowy. Można za pomocą funkcji wyszukiwania i zamienić narzędzie, aby znaleźć crosslinks, jeśli jest zainstalowany. Jeśli nie, możesz użyć programu Windows PowerShell bezpłatnie! Oto jak znaleźć crosslinks za pomocą programu PowerShell:

 . Uruchom program Windows PowerShell.

 b. W wierszu polecenia programu PowerShell zmienić do tego folderu azure zawartości — pr\articles:

 `cd azure-content-pr\articles`

 c. Wpisz następujące polecenie, które spowoduje wyświetlenie listy wszystkich plików, które zawierają odwołanie do tego artykułu, który chcesz usunąć:

 `Get-ChildItem -Recurse -Include *.md* | Select-String "<the name of the topic you are deleting>" | group path | select name`

  Jeśli wolisz wysłać listę nazw plików do pliku tekstowego (w tym psoutput.txt litery, nazwanego), możesz wykonać następujące czynności:

  `Get-ChildItem -Recurse -Include *.md* | Select-String "<the name of the topic you are deleting>" | group path | select name | Out-File C:\Users\<your account>\psoutput.txt`

3. Dodawanie i zatwierdzić wprowadzone zmiany, przekazać je do swojego rozwidlenia i Utwórz żądanie pobieraj przenieść zmiany z Twojej rozwidlenie wzorca gałęzi repozytorium głównym.

## <a name="step-4-update-the-fwlink-tool-required"></a>Krok 4: Aktualizowanie narzędzia łącze FW (wymagany)

Zaznacz narzędzie łącze FW dla dowolnego fwlink może do tego artykułu. Wskaż dowolny linki FW wymiana zawartości; Jeśli nie korzystasz z alias, który jest właścicielem łącza, dołączanie do. Jeżeli właściciele nie można zaktualizować łącze, plik biletów z MSCOM, aby zmienić łącze. Więcej informacji — [wewnętrzny typu wiki](http://sharepoint/sites/azurecontentguidance/wiki/Pages/Manage%20inbound%20links%20to%20retired%20topics.aspx).

## <a name="step-5-remove-all-crosslinks-to-the-article-from-other-pages-on-azuremicrosoftcom-and-create-a-redirect-for-the-retired-page-if-appropriate-required"></a>Krok 5: Usuwanie wszystkich crosslinks do artykułu z innych stron na azure.microsoft.com i tworzenie Przekieruj wycofanych strony, jeśli odpowiednie (wymagany)

Musisz pracować z osobą, która obsługuje i aktualizuje strona początkowa dokumentacji tej usługi dla tej części. Jeśli nie wiesz, kto jest danej osoby, skontaktuj się z partnerem zawartości zespołu. Osoba, która obsługuje i aktualizuje strona początkowa dokumentu należy wykonać dwie czynności:

1. W programie Visual Studio Przejrzyj **cały** ACOM web rozwiązanie dla odsyłaczy do pliku, aby wycofać. Usuwanie odsyłaczy lub Zamień zaktualizowane odsyłacz. Musisz usunąć linki HTML, jak również ciągów zasobów powiązanych łączy HTML. Więcej informacji — zobacz [wewnętrznych typu wiki](http://sharepoint/sites/azurecontentguidance/wiki/Pages/Create%20or%20edit%20a%20service%20landing%20page%20or%20left%20nav.aspx)

2. Jeśli istnieje artykuł zamienny, utworzyć przekierowanie. Więcej informacji — zobacz [wewnętrznych typu wiki](http://sharepoint/sites/azurecontentguidance/wiki/Pages/Remove%20published%20pages%20and%20request%20redirects.aspx).

3. Zaznacz zmiany do repozytorium.

## <a name="step-6-retire-the-article"></a>Krok 6: Wycofać tego artykułu

Po wykonaniu poprzednich kroków, i te zmiany są aktywne, możesz usunąć z repozytorium tego artykułu. 

**Ważne:** Podczas usuwania plików, należy użyć `git add --all` polecenia.

## <a name="step-7-remove-links-from-msdn-required"></a>Krok 7: Usuwanie łączy w witrynie MSDN (wymagany)

Przegląd zawartości narzędzia aparatu pytania i odpowiedzi w poszukiwaniu przerwanych łączy do tematu wycofanych lub o zmienionej nazwie i Usuń napraw łącza w wszystkich tematów w witrynie MSDN zależne.

## <a name="step-8-remove-cached-pages-from-search-engines-only-if-absolutely-necessary"></a>Krok 8: Usuwanie pamięci podręcznej stron z aparatów wyszukiwania (tylko wtedy, gdy konieczny)

Zrobić to tylko, jeśli zawartość trzeba szybko usunąć z powodu problemów z prawne lub trudnych klienta. Na najlepsze rozwiązania z Google usunięcia strony normalny priorytet powinny być traktowane tylko przez procesy aparat wyszukiwania naturalne. Przejdź do strony sieci web do usunięcia buforowane strony sieci web z aparatów wyszukiwania:

[Bing](https://www.bing.com/webmaster/tools/content-removal?rflid=1)
[Google](https://www.google.com/webmasters/tools/removals?pli=1)


### <a name="contributors-guide-links"></a>Współautorzy przewodnik łącza

- [Artykuł Omówienie](./../README.md)
- [Indeks artykułów ze wskazówkami dotyczącymi](./contributor-guide-index.md)
