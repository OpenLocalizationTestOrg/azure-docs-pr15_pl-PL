<properties
   pageTitle="Narzędzie Azure AD Connect: Uaktualnianie z poprzedniej wersji | Microsoft Azure"
   description="Omówiono różne metody przeprowadzić uaktualnienie do najnowszej wersji Azure Active Directory połączyć, takich jak uaktualnienie w miejscu i zasięg migracji."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="Identity"
   ms.date="10/12/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-upgrade-from-a-previous-version-to-the-latest"></a>Narzędzie Azure AD Connect: Uaktualnianie z poprzedniej wersji do najnowszych
W tym temacie opisano różne metody, których możesz uaktualnić instalacji narzędzie Azure AD Connect do najnowszej wersji. Zaleca się pozostawienie samodzielnie bieżącego z wersjami Azure AD Connect. Kroki opisane w [kierunku migracji](#swing-migration) są również używane podczas tworzenia konfiguracji istotne zmiany.

Jeśli chcesz uaktualnić z DirSync, zobacz [uaktualnienie narzędzie do synchronizacji usługi Azure AD (DirSync)](./connect/active-directory-aadconnect-dirsync-upgrade-get-started.md) zamiast tego.

Istnieje kilka różnych strategii uaktualniania Azure AD Connect.

Metoda | Opis
--- | ---
[Uaktualnianie](active-directory-aadconnect-feature-automatic-upgrade.md) | W przypadku klientów z instalacją pakietu ekspresowe jest to metoda najprostszym.
[Uaktualnienie w miejscu](#in-place-upgrade) | Jeśli masz jeden serwer, Uaktualnij instalacji w miejscu na tym samym serwerze.
[Zasięg migracji](#swing-migration) | Przy użyciu dwóch serwerów można przygotować jeden z serwerów z nowej wersji lub konfiguracji i zmienić ASP, po zakończeniu.

Aby uzyskać wymagane uprawnienia Zobacz [uprawnienia wymagane do uaktualnienia](./connect/active-directory-aadconnect-accounts-permissions.md#upgrade).

## <a name="in-place-upgrade"></a>Uaktualnienie w miejscu
Uaktualnienie w miejscu działa w przypadku przenoszenia z Azure AD Sync lub narzędzie Azure AD Connect. Nie będzie działać dla DirSync lub rozwiązanie z kodu FIM + łącznik Azure AD.

Ta metoda jest zalecana jednego serwera i mniejsza niż około 100 000 obiektów. W przypadku zmiany zasad synchronizacji z gotowych do pełnego importowania i pełnej synchronizacji występują po uaktualnieniu. Dzięki temu nowej konfiguracji został zastosowany do wszystkich istniejących obiektów w systemie. To może potrwać kilka godzin, w zależności od liczby obiektów w zakresie aparat synchronizacji. Harmonogram synchronizacji różnicy normalny, domyślnie co 30 minut zostało zawieszone, ale nadal synchronizacji haseł. Warto wykonać uaktualnienie w miejscu podczas weekend. Jeśli wystąpią żadne zmiany w konfiguracji w nowym polu z nowej wersji Azure AD Connect, importowanie normalny delta-synchronizacji zostanie uruchomiona zamiast tego.  
![Uaktualnienie w miejscu](./media/active-directory-aadconnect-upgrade-previous-version/inplaceupgrade.png)

Jeśli do synchronizacji z gotowych do reguł zostały wprowadzone zmiany, zostaną one ponownie ustawione domyślnej konfiguracji w czasie uaktualniania. Aby upewnić się, że przechowywanej konfiguracji między uaktualnienie, upewnij się, że zostały wprowadzone zmiany, zgodnie z opisem w temacie [Najważniejsze wskazówki dotyczące zmieniania konfiguracji domyślnej](active-directory-aadconnectsync-best-practices-changing-default-configuration.md).

## <a name="swing-migration"></a>Zasięg migracji
Jeśli masz wdrażania złożonych lub bardzo wiele obiektów, może być niemożliwe do uaktualnienie w miejscu w systemie aktywnym. W przypadku niektórych klientów może to potrwać kilka dni i w tym czasie żadnych zmian różnicy będą przetwarzane. Ta metoda jest również używana, gdy ma być istotne zmiany w konfiguracji i chcesz spróbować je przed te są usuwane w chmurze.

Dla tych scenariuszy zaleca się używanie migracji zasięg. Potrzebujesz (co najmniej) dwa serwery, obecnie i jeden serwer tymczasowy. ASP (pełne niebieskie linie na ilustracji poniżej) jest odpowiedzialny za ładowanie aktywnej produkcji. Serwerze (kreskowane purpurowe linie w na poniższym obrazie) jest gotowa z nowej wersji lub konfiguracji i pełni gotowa, ten serwer się aktywne. Poprzedniego serwera aktywnego teraz za pomocą starszej wersji lub konfiguracji zainstalowany, jest wprowadzone na serwerze i uaktualnione.

Dwa serwery można używać różnych wersji. Na przykład ASP, które mają być zlikwidować służy Azure AD Sync i nowy serwer tymczasowy służy narzędzie Azure AD Connect. Jeśli korzystasz z migracji zasięg opracowywaniu nowej konfiguracji jest warto mieć tej samej wersji na dwa serwery.  
![Organizowanie serwera](./media/active-directory-aadconnect-upgrade-previous-version/stagingserver1.png)

Uwaga: Stwierdzono niektórzy klienci wolę trzy lub cztery serwerów dla tego scenariusza. Po uaktualnieniu serwera przemieszczenia nie masz kopii zapasowych serwera w przypadku [awarii](active-directory-aadconnectsync-operations.md#disaster-recovery). Z serwerami trzy lub cztery jeden zestaw serwerów podstawowy/gotowość z nową wersją można przygotować, zyskujemy pewność, że są zawsze serwerze przemieszczenia chcesz przejąć.

Te kroki ma zastosowanie również w celu przeniesienia między Azure AD Sync lub rozwiązanie z kodu FIM + łącznik Azure AD. Te kroki nie działają w przypadku DirSync, ale tym samym zasięg migracji (zwanych również równoległe wdrożenia) metody czynności DirSync znajdują się w [Uaktualnienie usługi Azure Active Directory synchronizowanie (DirSync)](./connect/active-directory-aadconnect-dirsync-upgrade-get-started.md).

### <a name="swing-migration-steps"></a>Zasięg kroków migracji

1. Jeśli używasz Azure AD Connect na obu serwerach i planowanie wykonaj tylko w przypadku zmiany konfiguracji, upewnij się, usługi active server, a przemieszczenia serwera są zarówno w tej samej wersji. Ułatwia to porównać później. Jeśli przechodzisz z Azure AD Sync, tych serwerach używają różnych wersji. Jeśli w przypadku uaktualniania ze starszej wersji Azure AD Connect jest dobrym pomysłem jest początkowych dwa serwery w tej samej wersji, ale nie jest wymagane.
2. Jeśli wprowadzono kilka Konfiguracja niestandardowa i serwera tymczasowy nie ma, postępuj zgodnie z instrukcjami w obszarze [Przenoszenie Konfiguracja niestandardowa z aktywna na serwerze](#move-custom-configuration-from-active-to-staging-server).
3. Jeśli przechodzisz z wcześniejszą wersją Azure AD Connect, Uaktualnij serwerze do najnowszej wersji. Jeśli chcesz przenieść z Azure AD Sync, zainstaluj narzędzie Azure AD Connect na serwerze tymczasowy.
4. Poinformuj aparatu synchronizacji, uruchom pełne importowanie i pełnej synchronizacji na serwerze tymczasowy.
5. Sprawdź konfigurację nowego nie był przyczyną nieoczekiwane zmiany w obszarze **Weryfikuj** przy użyciu [Sprawdź konfigurację serwera](active-directory-aadconnectsync-operations.md#verify-the-configuration-of-a-server). Jeśli coś nie jest jako oczekiwane, poprawne, Uruchom importowanie i synchronizowanie i sprawdź, aż dane odpowiada Twoim potrzebom. Poniższe czynności, można znaleźć w temacie połączone.
6. Przełączanie tymczasowy serwera active server. To jest ostatni krok **Przełączanie ASP** w [artykule Weryfikowanie konfiguracji serwera](active-directory-aadconnectsync-operations.md#verify-the-configuration-of-a-server).
7. W przypadku uaktualniania Azure AD Connect uaktualnienie serwera w tymczasowej tryb do najnowszej wersji. Wykonaj te same kroki jako przed, aby wyświetlić dane i konfigurację uaktualnione. Po uaktualnieniu systemu Azure AD Sync, możesz teraz wyłączyć i zlikwidować starego serwera.

### <a name="move-custom-configuration-from-active-to-staging-server"></a>Przenoszenie Konfiguracja niestandardowa z aktywne na serwerze
Jeśli dokonano zmian w konfiguracji serwera aktywnego, musisz upewnij się, że te same zmiany zostaną zastosowane do serwera przemieszczania.

Mogą być przenoszone reguły niestandardowe synchronizacji, które zostały utworzone przy użyciu programu PowerShell. Inne zmiany, należy zastosować ten sam sposób we wszystkich systemach i nie można migrować.

Należy upewnić się, co jest skonfigurowana tak samo na obu serwerach:

- Połączenie z tym samym lasów.
- Wszelkie domenę i jednostkę Organizacyjną, do filtrowania.
- Tym samym opcjonalne funkcje, takie jak synchronizacji haseł i zapisu hasła.

**Przenoszenie reguły synchronizacji**  
Aby przenieść regułę niestandardowych, wykonaj następujące czynności:

1. Otwórz **Edytor reguł synchronizacji** na serwerze aktywna.
2. Wybierz pozycję Tworzenie niestandardowej reguły. Kliknij przycisk **Eksportuj**. Spowoduje to wyświetlenie okna Notatnika. Zapisz plik tymczasowy z rozszerzeniem PS1. Dzięki temu skrypt programu PowerShell. Skopiuj plik ps1 do serwera przemieszczania.  
![Wyeksportowanie reguły synchronizacji](./media/active-directory-aadconnect-upgrade-previous-version/exportrule.png)
3. Identyfikator GUID łącznik różni się na serwerze i musi zostać zmieniony. Aby uzyskać identyfikator GUID, uruchomić **Edytor reguł synchronizacji**, wybierz jedną z reguł w nowym polu reprezentującą ten sam układ połączone i kliknij przycisk **Eksportuj**. Identyfikator GUID w pliku PS1 należy zastąpić GUID z serwera przemieszczenia.
4. W wierszu polecenia programu PowerShell Uruchom plik PS1. Spowoduje to utworzenie reguły niestandardowych na serwerze.
5. Jeśli masz wiele reguł niestandardowych, powtórz dla wszystkich niestandardowych reguł.

## <a name="next-steps"></a>Następne kroki
Dowiedz się więcej o [integrowaniu lokalnego tożsamości z usługą Azure Active Directory](active-directory-aadconnect.md).
