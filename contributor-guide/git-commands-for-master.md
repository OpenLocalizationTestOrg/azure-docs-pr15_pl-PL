<properties pageTitle="Cyfra polecenia służące do tworzenia nowego artykułu lub aktualizowania istniejącego artykułu" description="Instrukcje dotyczące pracy z Azure techniczne zawartości repozytoria GitHub." metaKeywords="" services="" solutions="" documentationCenter="" authors="tysonn" videoId="" scriptId="" manager="carolz" />

<tags ms.service="contributor-guide" ms.devlang="" ms.topic="article" ms.tgt_pltfrm="" ms.workload="" ms.date="01/16/2015" ms.author="tysonn" />

# <a name="git-commands-for-creating-a-new-article-or-updating-an-existing-article"></a>Cyfra polecenia służące do tworzenia nowego artykułu lub aktualizowania istniejącego artykułu


## <a name="standard-process-working-from-master"></a>Proces standardowy (Praca ze wzorca)
Wykonaj kroki opisane w tym artykule, aby utworzyć lokalne gałąź pracy na komputerze, dzięki czemu możesz utworzyć nowy artykuł sekcji dokumentacji technicznej azure.microsoft.com lub aktualizowanie istniejącego artykułu.

1. Uruchom imprezie cyfra (lub narzędzie wiersza polecenia, których cyfra).

 **Uwaga:** Jeśli pracujesz w publicznej repozytorium, zmienić azure zawartości — pr zawartości azure w polu polecenia.

2. Zmień azure zawartości — pr:

        cd azure-content-pr
3. Zapoznaj się z głównego gałąź:

        git checkout master

4. Należy utworzyć nowy lokalne gałąź pracy pochodzące z głównego gałąź:

        git pull upstream master:<working branch>


5. Przenoszenie do nowego oddziału pracy:

        git checkout <working branch>

6. Poinformuj swojego rozwidlenie ustalić, czy utworzono lokalne gałęzią pracy:

        git push origin <working branch>

7. Tworzenie nowego artykułu lub wprowadzić zmiany do istniejącego artykułu. Otwieranie i tworzyć nowe pliki promocji cenowych oraz Atom (http://atom.io) jest używany jako edytor promocji cenowych za pomocą Eksploratora Windows. Po utworzone lub zmodyfikowane do artykułu i obrazów, przejdź do następnego kroku.

8. Dodawanie i zatwierdzić wprowadzone zmiany.

        git add .
        git commit –m "<comment>"
        
   Lub, aby dodać tylko określonych pliki zmodyfikowane:

        git add <file path>
        git commit –m "<comment>"

   Jeśli usuniesz pliki, należy za pomocą tej funkcji:
   
        git add --all
        git commit -m "<comment>"

9. Aktualizacja do lokalnych gałąź pracy ze zmianami nadrzędny:

        git pull upstream master

10. Przekazać zmiany do swojego rozwidlenie na GitHub:

        git push origin <working branch>

12. Po zakończeniu przesyłania zawartości do nadrzędnego gałęzi wzorca dla tymczasowej sprawdzanie poprawności, i/lub publikowanie, w Interfejsie GitHub tworzenie pobieraj zlecenia z Twojej rozwidlenie gałąź wzorca.

13. W przypadku pracownika pracy w repozytorium prywatne, zmian, które można przesłać są automatycznie umieszczane i tymczasowy łącza jest zapisywany na żądanie pobieraj. Przejrzyj zawartość etapowej i zaloguj się komentarze żądanie pobieraj przez dodanie komentarza **sign poza #** .  Oznacza to, że zmiany są gotowe do zostać przeniesiony na żywo.  Jeśli nie chcesz żądanie pobieraj zaakceptowane -, jeśli są tylko tymczasowej zmiany — Dodaj notatkę **hold poza #** na żądanie pobieraj.

14. Pobieraj akceptanta żądanie Recenzuje żądania pobieraj, udostępnia opinii lub akceptuje żądania pobieraj. 

15. Opcjonalnie Sprawdź swoje opublikowany artykuł lub zmiany w

 http://Azure.microsoft.com/Documentation/articles/*name-of-your-article-without-the-MD-extension*

## <a name="publishing"></a>Publikowanie

- Artykuły są publikowane w przybliżeniu 10:00 i 3:00 PM czasu pacyficznego, od poniedziałku do piątku. Może potrwać do 30 minut artykuły są wyświetlane online po opublikowaniu. Należy pamiętać, że żądania pobieraj musi być scalane przez recenzenta żądania pobieraj, zanim zmiany mogą być zawarte w następnej planowanej publikacji, uruchom. Musisz pracować z pobieraj recenzenta żądania wyprzedzeniem mieć pewność, że żądania pobieraj scalone określonych publikowania, uruchom. W przeciwnym razie PRs są przeglądane w odpowiedniej kolejności ich złożenia.

- Jeśli korzystasz z pracowników zatrudnionych w repozytorium prywatne, wszystkie żądania pobieraj podlegają reguł sprawdzania poprawności, które należy uwzględnić przed żądanie pobieraj mogą być scalane. 

## <a name="working-with-release-branches"></a>Praca z oddziałów wersji

Podczas pracy z gałęzią wersji, najlepszym sposobem tworzenia lokalnych gałąź pracę od gałęzi wersji jest należy użyć następującej składni polecenia:

    git checkout upstream/<upstream branch name> -b <local working branch name>

Spowoduje to utworzenie lokalnego gałąź bezpośrednio z Oddział nadrzędny, można uniknąć lokalne scalania.

