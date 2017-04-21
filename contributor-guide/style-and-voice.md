<properties title="" pageTitle="Pisanie Azure dokumentacji - głosowych i styl cheat arkusza" description="Styl i głosu informacje ułatwiają tworzenie treść techniczna Centrum dokumentacji Azure." metaKeywords="" services="" solutions="" documentationCenter="" authors="tysonn" videoId="" scriptId="" manager="required" />

<tags ms.service="contributor-guide" ms.devlang="" ms.topic="article" ms.tgt_pltfrm="" ms.workload="" ms.date="12/16/2014" ms.author="glenga" />

#<a name="writing-azure-documentation---style-and-voice-cheat-sheet"></a>Pisanie Azure dokumentacji - głosowych i styl cheat arkusza

Poniżej przedstawiono arkusz podpowiedzi zawierający, który zawiera wskaźniki temat pisania artykuły techniczne usługi Azure i technologii. Poniższe wskazówki stosowanie czy podczas tworzenia nowych dokumentów lub aktualizowania istniejących dokumentów.

Co najmniej od zera, należy:

- Sprawdzanie pisowni i gramatyki Sprawdź tematów, nawet jeśli masz do wycinania i wklejania w programie Word, aby to zrobić.
- Za pomocą głosu nieformalnych i przyjazny — takie jak rozmowie do innej osoby Wykorzystaj.
- Za pomocą prostych zdań. Ta osoba jest łatwiejszy do zrozumienia, i są one łatwiejsze przetłumaczone przez tłumacze zarówno ludzi, jak i komputera.

W poniższych sekcjach zawarto szczegółowe informacje:

+ [Używanie głosu przyjazna dla klienta]
+ [Należy rozważyć, czy lokalizacji i tłumaczenia maszynowego]
+ [Inne problemy z stylu i głosowej do obserwowania]


##<a name="use-a-customer-friendly-voice"></a>Używanie głosu przyjazna dla klienta

Firma Microsoft Oczekuj wykonaj te zasady pisząc treść techniczna dla Azure. Firma Microsoft może nie zawsze się, ale należy zachować próbuje!

- **Użyj słów, codzienne**: spróbuj użyć języku naturalnym słów klienci za pomocą; mniej formalnych, ale nie mniej techniczne; zawierają przykłady opisują nowych koncepcji.

- **Pisanie ten**: nie tracą wyrazów i nie przekracza wyjaśnienia. Być potwierdzającego i nie używaj dodatkowe wyrazy lub wiele kwalifikatory. Zachowaj zdań, krótka i zwięzły. Zachowaj temat ograniczony. Jeśli zadanie ma kwalifikator, należy umieścić go na początku zdania lub akapitu. Ponadto Zachowaj liczby notatek do minimum. Za pomocą zrzut ekranu można zapisać wyrazów.

- **Łatwy zeskanować**: najpierw umieścić najważniejszych elementów. Za pomocą sekcji długiej procedury w bryłkach na łatwiejsze grupy czynności (procedur z więcej niż 12 czynności prawdopodobnie są zbyt długie). Gdy dodaje jasności za pomocą zrzut ekranu.

- **Pokazywanie empathy**: używanie sygnał wspomagających w artykule przy zachowaniu zastrzeżenia do minimum. Uczciwie wyróżnić obszarów, które mają być frustrujące dla klientów. Upewnij się, że artykuł poświęcony ma znaczenie co do klienta, po prostu nie dają techniczne wykładzie.

##<a name="consider-localization-and-machine-translation"></a>Należy rozważyć, czy lokalizacji i tłumaczenia maszynowego
Nasze artykuły techniczne są przekształcić w wiele innych języków, a niektóre mogą modyfikować określonego rynkach. Osoby mogą również korzystać z tłumaczenia maszynowego w sieci web do odczytu artykuły techniczne. Tak pisanie przy poniższe wskazówki pamiętać:

- **Upewnij się, że ten artykuł zawiera nie gramatyki, pisowni i błędów interpunkcyjnych**: to należy wykonać ogólnie. Konsola promocji cenowych 2.0 zawiera moduł sprawdzania pisowni podstawowych, ale należy również wkleić (renderowanych HTML) zawartość z tego artykułu do programu Word, który ma bardziej rozbudowany pisowni i gramatyki.

- **Tworzenie swojego zdania możliwie krótki**: zdania złożone lub łańcuchów klauzul utrudnić tłumaczenia. Jeśli użytkownik nie jest zbyt zbędne lub dziwne pomiarowe podziału zdań. Nie możemy naprawdę chcesz artykuły napisanym w języku nienaturalnie albo.

- **Użyj zdanie prosty i spójne budowy**: spójności jest lepsza tłumaczenia. Unikanie parentheticals i funduszy odłożonych, a temacie jako u początek zdania, jak to możliwe. Zapoznaj się z kilku tematy opublikowanych -, jeśli temat ma przyjaznej łatwe do odczytania stylu, użyj go jako model.

- **Użyj spójności terminologii i wielkości liter**: ponownie spójności jest klucz. Azure korzysta obudowy zdania tytułach, więc nigdy nie wielką wyrazu, jeśli nie jest początek zdania lub rzeczownikowe litery.

- **Dołącz "małe wyrazy"**: wyrazów, które możemy rozważ małych i ważne w języku angielskim, ponieważ są one zrozumiałe dla kontekstu (takie jak "," "", "that" i "jest") mają kluczowe znaczenie tłumaczenia maszynowego — upewnij się, możesz umieścić!

##<a name="other-style-and-voice-issues-to-watch-for"></a>Inne problemy z stylu i głosowej do obserwowania

- Nie należy podzielić kroki z komentarzem lub funduszy odłożonych.

- Instrukcje, które zawierają fragmenty kodu należy umieścić dodatkowe informacje o etapie do kodu jako komentarze. Zmniejsza fragment tekstu, które osoby mają czytać i informacje o kluczu kopiowane do programu project kodu do Przypomnij o kodzie czynności podczas odwołują się do niego później.

- Oficjalna nazwa produktu jest "Microsoft Azure", ale możemy prawie zawsze wystarczy powiedzieć "Azure", tak jak "Usługi Azure Mobile".

- Nie należy tworzyć skrótów, które zaczynają się od "MA" lub "A" Po prostu użyć "Azure" w pierwszym odwołanie przed nazwą usługa lub funkcja, a następnie upuść ją (np. "Usługi Azure Mobile" staje się "Mobile usług" po pierwszym użyciu). Należy unikać skrótów ogólnie — są tylko należy mylić osób.

- Azure używa obudowy zdania wszystkie tytuły.

- Korzystanie z "logowania" i nie "zalogować się w."

- Obejmować wyrazy "obserwowania" lub "w następujący sposób" wszystkie zdania, poprzedzającej listy lub kod wstawek.

- "Baza danych SQL" jest funkcja Azure. "Bazie danych SQL" to wystąpienie bazy danych z bazy danych SQL.

- Azure magazynowania zawiera kilka "danych zarządzania usługi" zawierające usługi tabel, usługa obiektów Blob i usługa kolejki. (Ta opcja nie nosi nazwę "Usługa magazynu tabel platformy Azure".)




###<a name="contributors-guide-links"></a>Współautorzy przewodnik łącza

- [Artykuł Omówienie](./../README.md)
- [Indeks artykułów ze wskazówkami dotyczącymi](./contributor-guide-index.md)



<!--Anchors-->
[Używanie głosu przyjazna dla klienta]: #use-a-customer-friendly-voice
[Należy rozważyć, czy lokalizacji i tłumaczenia maszynowego]: #consider-localization-and-machine-translation
[inne problemy z stylu i głosowej do obserwowania]: #other-style-and-voice-issues-to-watch-for
