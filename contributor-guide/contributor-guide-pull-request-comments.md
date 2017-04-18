# <a name="pull-request-comment-automation"></a>Pobierają żądanie automatyzacji komentarza

Firma Microsoft korzysta automatyzacji komentarz w komentarzach żądanie pobieraj zezwolić współautorom i autorów, aby przypisać etykiety, które sterują żądanie pobieraj Przejrzyj proces.

| Komentarz | Działanie | Dostępność|
| -------- |-------------|-------------|
|#podpisywania | Po Autor artykułu typy komentarz **sign poza #** w strumieniu komentarza, jest przypisywana etykieta **gotowy do korespondencji seryjnej** . | Publiczne i prywatne|
|#podpisywania | Jeśli Współpracownik, który nie jest wymienione Autor próbuje zalogować na żądanie pobieraj publicznej, używając komentarz **sign poza #** , wiadomość jest zapisywany pobieraj żądanie wskazuje, że etykiety można przypisywać tylko autor. | Publiczna |
|#wyłączanie blokady | Jeśli wpiszesz **hold poza #** w komentarzu żądanie pobieraj usuwa etykieta **gotowy do scalenia** — w przypadku, gdy zmienisz zdanie lub przez pomyłkę. W prywatnych repo w ten sposób etykiety **nie seryjnej** . | Publiczne i prywatne |
| #Zamknij zapoznaj się z | Autorzy mogą wpisz komentarz **Zamknij please #** w strumieniu komentarz żądanie pobieraj, aby go zamknąć, jeśli jednak nie chcesz scalić zmiany. | Publiczna |

##<a name="troubleshooting-sign-offs-in-the-public-repo"></a>Rozwiązywanie problemów z rozwiązań znak w publicznej repo

Znak publicznej repo wyłączanie automatyzacji jest umożliwia tylko autor logowania. Mogą być potrzebne niektóre ręcznego wyjątek podczas przetwarzania:

- **Autorzy artykuł**: umożliwia automatyzację komentarz repozytorium publicznej rzeczywisty konta GitHub muszą dokładnie odpowiadać konta GitHub wymieniony w artykule metadanych. Ma znaczenie wielkości liter swojego konta. Jeśli są zablokowane z zalogowaniem z powodu tego problemu, wysłać pocztę do azdocprs alias.

- **Inni pracownicy**: są pracownik, który jest podpisywanie imieniu autora są zablokowane przez automatyzacji, skontaktuj się z azdocprs z link wniosek o pobieraj. Wskazuje, kto jest — PMs na tym samym zespołu produktu, współpracownicy w zespołu pisania i pisania menedżerów zespołu są traktowane jako zaufanych źródeł.



## <a name="related"></a>Powiązane

- [Pobieraj wezwanie na etykiecie i najważniejsze wskazówki dotyczące programu Microsoft współautorów](contributor-guide-pull-request-etiquette.md)

- [Przejrzyj kryteria dotyczące jakości żądania pobieraj](contributor-guide-pr-criteria.md)
