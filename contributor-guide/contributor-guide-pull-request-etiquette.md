# <a name="pull-request-etiquette-and-best-practices-for-microsoft-contributors-to-azure-documentation"></a>Pobierają etykietą żądania oraz najlepsze rozwiązania dla współautorów Microsoft Azure dokumentacji

Aby opublikować zmiany w dokumentacji, przesłaniu żądania pobieraj z Twojej rozwidlenie. Każde żądanie pobieraj ma do przejrzenia przed scalane. W tym artykule, aby dowiedzieć się, jak należy przetwarzania pobieraj żądanie recenzentów i sposób tworzenia żądania pobieraj którego są łatwiejsze i szybsze umożliwia przeglądanie, kolejki żądań pobieraj sprawdza się lepiej dla wszystkich osób.

## <a name="working-with-pull-request-reviewers"></a>Praca z pobieraj żądanie recenzentów

Oto podstawowe informacje, trzeba wiedzieć na temat pracy z recenzentów żądanie pobieraj. 

- <b>Opis roli recenzenta żądania pobieraj. Recenzent:</b>
  - Zapewnia podstawowe jakości zawartości
  - Zapobiega strat w repozytorium
  - Udostępnia opinii przed scalania

  Pobieraj żądanie recenzenci znajdują się w roli zarządzania zawartości. Przeznaczenie podstawowego nie jest po prostu korespondencji seryjnej niezależnie od przesłaniu tak szybko, jak to możliwe. Przewiduje opinii konieczne będzie wykonywać aktualizacji, szczególnie w przypadku nowych i intensywnie poprawiony artykuły.

- <b>Planowanie z recenzenta żądania pobieraj:</b>
  - Pobieraj wysoki priorytet wniosków o
  - Pobieraj żądań Przekroczono wydanych wersjach
  - Pobieraj zaproszenia, które zmienić lub dodać wiele plików

- <b>Umowa dotycząca poziomu usług dla pobieraj Zażądaj przeglądu</b>

  W repozytorium prywatne zawsze żądania pobieraj wprowadza kolejce pobieraj z etykietą gotowy do scalenia zespołu próbuje przeglądania żądania pobieraj w ciągu 12 godzin business (M-F, 8 AM do 5 PM) i przekazywanie opinii lub korespondencji seryjnej, jeśli wymagane jest opinii. Ten SLA dotyczy czynność przeglądania cena, nie scalania. PRs zostaną scalone, gdy spełniają [kryteria do scalenia](contributor-guide-pr-criteria.md). 

## <a name="make-the-pull-request-queue-work-better-for-everyone"></a>Tworzenie kolejki żądań pobieraj lepszą współpracę dla wszystkich osób

Istnieją dwa podstawowe realiów w kolejce PR:

- Pobieraj żądania są małe w zakresie, które zawierają bardzo podobne zmiany trwać krócej do przejrzenia. 
- Żądania pobieraj są dużego zakresu lub zawierają różne, mieszane typy zmian poświęcić nieco czasu na przeglądanie.

Można zwiększyć kolejce pobieraj lepszą współpracę, wykonując poniższe najważniejsze wskazówki:

- Osobne pomocniczych aktualizacje do istniejącego artykułów, z nowych artykułów lub ponownego głównych. Praca na tych zmian w oddzielnym gałęziami pracy. 

- Po usunięciu artykułów lub obrazy, nie można mieszać operację usunięcia z nowej zawartości dodatki lub aktualizacje. Uchwyt zmiany i nową zawartość w osobnym gałęzi pracy.

- W przypadku wersji lub Refaktoryzacja zawartości Planuj z recenzenta cena. Może być konieczne jej pomocy aby utworzyć Rozgałęzienie wersji lub koordynowanie razy korespondencji seryjnej z czasem publikowania, aby zawartość jest publikowana w odpowiednim czasie.

- Jeśli próbujesz koordynowanie aktualizacje wprowadzone w ACOM "repo" (ie, zmiany do nawigacji po lewej stronie wyładunku stron, przekierowania lub mapy nauki) ze zmianami tworzonego w repozytorium azure zawartości — pr, tę pracę związaną z wcześniejszym przygotowaniem musi będą dopasowane do recenzenta PR. W przeciwnym razie ryzyko o wiele przerwanych łączy.

## <a name="criteria-for-expedited-pull-requests"></a>Kryteria dla żądania przyspieszona pobieraj

- Skontaktuj się z azdocprs aby przyspieszyć PRs tylko wtedy, gdy konieczny. Możesz zażądać przyspieszona PR obsługi strefy czerwony, prywatność, prawnych i problemach zabezpieczeń; do zastosowań naprawdę przerwanego klienta; i odpowiedzi na pytania techniczne w kierownictwa. 
- Zawartość dla funkcji wersji nie kwalifikuje się do obsługi przyspieszona — funkcja udostępniania zawartości wymaga uprzedniego planowania lub musi być obsługiwane przez kolejki Standardowy priorytet.


## <a name="in-a-hurry-submit-prs-that-can-be-accepted-automatically"></a>W pośpiechu? Przesyłanie PRs, które mogą być akceptowane automatycznie

Za pomocą reguł automatyzacji PRMerger lepiej z codzienną PRs automatycznie scalane.

PRMerger może zaakceptować Twojej PR automatycznie, jeśli:
* Wpływa na 10 plików lub mniej.
* Zawiera on 15 zatwierdzenia lub mniej.
* Mniej niż 20% zmian w tekście.
* Selektory switchers nie została zaktualizowana.
* Żadne pliki są usuwane lub dodane.
* Nie obrazów są nowe, zmianie lub usunięciu.

Jeśli żądania pobieraj nie spełnia tych kryteriów, etykiety "PRmerger nie można scalić" zostanie automatycznie przypisany tak wiadomo, że wymaga przeglądu przez ludzi recenzenta cena.

### <a name="need-to-make-a-lot-of-little-changes"></a>Musisz wprowadzić licznych zmian mały?

Sporządzanie usługi sygnalizacji z powyższych reguł automatyzacji PRMerger i wykonaj następujące czynności:
* Przesyłanie artykułów zawierających uproszczonej zmiany w PR z plikami 10 lub mniej.
* Tworzenie osobnych PR artykułów, w które obrazy lub selektory zmiany. W tym celu ludzi Recenzja.
* Tworzenie osobnych PR artykuły nowe lub usunięte. W tym celu ludzi Recenzja.

## <a name="related"></a>Powiązane

- [Przejrzyj kryteria dotyczące jakości żądania pobieraj](contributor-guide-pr-criteria.md)

- [Pobierają żądanie automatyzacji komentarza](contributor-guide-pull-request-comments.md)
