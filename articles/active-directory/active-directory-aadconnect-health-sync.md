
<properties
    pageTitle="Azure AD kondycji nawiązywanie połączenia za pomocą synchronizacji | Microsoft Azure"
    description="Jest to strona Azure AD łączenie kondycja, która przedstawimy sposobów monitorowania Azure AD Connect synchronizacją."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>

# <a name="using-azure-ad-connect-health-for-sync"></a>Przy użyciu Azure AD łączenie kondycji dla synchronizacji
Poniższą dokumentację dotyczy monitorowania Azure AD Connect (synchronizacja) z Azure AD łączenie kondycją.  Aby uzyskać informacje dotyczące monitorowania usług AD FS z Azure AD łączenie kondycją zobacz [Przy użyciu Azure AD łączenie kondycji z usług AD FS](active-directory-aadconnect-health-adfs.md). Ponadto informacje dotyczące monitorowania usług domenowych Active Directory z Azure AD łączenie kondycją można znaleźć [Przy użyciu Azure AD łączenie kondycji z usługami AD DS](active-directory-aadconnect-health-adds.md).

![Azure AD Connect kondycji do synchronizacji](./media/active-directory-aadconnect-health-sync/sync-blade.png)

## <a name="alerts-for-azure-ad-connect-health-for-sync"></a>Alerty Azure AD łączenie kondycji do synchronizacji
Sekcji Azure AD łączenie kondycji alertów dla synchronizacji miejsce na liście aktywne alertów. Każdy alert zawiera odpowiednie informacje, kroki rozwiązywania i łączy się z dokumentacją pokrewne. Wybierając alertu aktywny ani rozwiązany zostanie wyświetlony nowy kart z dodatkowe informacje, a także czynności, które można wykonać, aby rozwiązać alert i łączy się z dokumentacją dodatkowe. Możesz także wyświetlić danych historycznych na alerty, które zostały rozwiązane w przeszłości.

Wybierając alertu, które zostaną wyświetlone dodatkowe informacje, jak również czynności można wykonać, aby rozwiązać alert i łączy się z dokumentacją dodatkowe.

![Błąd synchronizacji usługi Azure AD Connect](./media/active-directory-aadconnect-health-sync/alert.png)

### <a name="limited-evaluation-of-alerts"></a>Ograniczone oceny alertów
Jeśli narzędzie Azure AD Connect nie używa konfiguracji domyślnej (na przykład jeśli filtrowanie atrybut zmieniono z domyślnej konfiguracji niestandardowych konfiguracji), w agenta Azure AD łączenie kondycji będzie nie przekazywać zdarzenia błędów związanych z Azure AD Connect.

Ocena alerty ogranicza przez usługę. Czy zostanie wyświetlony transparent wskazuje za pomocą tego warunku w Portal Azure w obszarze tej usługi.

![Azure AD Connect kondycji do synchronizacji](./media/active-directory-aadconnect-health-sync/banner.png)

Można to zmienić, klikając pozycję "Ustawienia" i zezwalania agent Azure AD łączenie kondycji przekazywanie wszystkich dzienników błędów.

![Azure AD Connect kondycji do synchronizacji](./media/active-directory-aadconnect-health-sync/banner2.png)

## <a name="sync-insight"></a>Informacje o synchronizacji
Administratorzy często chcesz wiedzieć o czas potrzebny na ilość zmiany miejsce i synchronizowanie zmian Azure AD. Ta funkcja zapewnia łatwy sposób wizualizacji to za pomocą poniżej wykresów:   

- Czas oczekiwania operacji synchronizacji
- Trend zmiany obiektu

### <a name="sync-latency"></a>Opóźnienie synchronizacji
Ta funkcja zapewnia graficzne trend opóźnienia operacji synchronizacji (importowanie, eksportowanie itp.) dla łączników.  Zapewnia szybki i łatwy sposób opis nie tylko opóźnienie operacji (większe, jeśli masz duże zestawu zmiany występujące), ale również umożliwia wykrywanie różnic w odniesieniu opóźnienia, które mogą wymagać dalszego dochodzenia.

![Opóźnienie synchronizacji](./media/active-directory-aadconnect-health-sync/synclatency02.png)

Domyślnie są wyświetlane tylko opóźnienia operacji "Eksportowania" dla pozycji łącznik Azure AD.  Aby wyświetlić więcej operacji łącznika lub aby wyświetlić operacje z innych łączników, kliknij prawym przyciskiem myszy wykres, zaznacz wykres Edytuj lub kliknij przycisk "Edytuj opóźnienie wykresu" i wybierz pozycję określonej operacji i łączników.

### <a name="sync-object-changes"></a>Synchronizowanie zmian obiektu
Ta funkcja zapewnia trendu graficzne liczbę zmian, które są obliczane i eksportowane do Azure AD.  Obecnie próby gromadzenie te informacje z dzienników synchronizacji jest trudne.  Wykres umożliwia, nie tylko prostszy sposób monitorowania liczbę zmian, które występują w środowisku, ale widok graficzny błędów, które występują.

![Opóźnienie synchronizacji](./media/active-directory-aadconnect-health-sync/syncobjectchanges02.png)

## <a name="object-level-synchronization-error-report-preview"></a>Raport o błędach synchronizacji poziomu obiektu (wersja Preview)
Ta funkcja zapewnia raportu o błędach synchronizacji, które mogą wystąpić podczas tożsamości dane są synchronizowane między Windows Server AD i Azure AD przy użyciu Azure AD Connect.

- Raport dotyczy błędy zarejestrowane przez klienta synchronizacji (narzędzie Azure AD Connect wersji 1.1.281.0 lub nowszy)
- Zawiera błędy, które wystąpiły w ostatniej operacji synchronizacji na aparat synchronizacji. ("Eksport" łącznika Azure AD.)
- Azure agent kondycji łączenie AD do synchronizacji musi być ruchu wychodzącego łączności z programem wymagane punkty końcowe raport, aby uwzględnić najnowszych danych. Zobacz [sekcję wymagania dotyczące](active-directory-aadconnect-health-agent-install.md#Requirements) , aby uzyskać szczegółowe informacje.
- Raport jest **zaktualizowana po co 30 minut** przy użyciu danych przekazane przez agenta Azure AD łączenie kondycji do synchronizacji.
Udostępnia następujące możliwości klucza

    - Kategoryzacja błędów
    - Lista obiektów z powodu błędu według kategorii
    - Wszystkie dane o błędach w tym samym miejscu
    - Porównanie porównawcze obiektów z powodu błędu ze względu na konflikt
    - Pobierz raport o błędach jako CSV (już wkrótce)

### <a name="categorization-of-errors"></a>Kategoryzacja błędów
Raport kategoryzuje istniejących błędami synchronizacji w następujących kategorii:

| Kategoria | Opis |
| -------------- | ----------- |
| Zduplikowany atrybut | Błędy podczas próby Azure AD Connect Tworzenie lub aktualizowanie obiektów z zduplikowane wartości jednego lub kilku atrybutów w Azure AD, który musi być unikatowa w dzierżawie, takich jak proxyAddresses, UserPrincipalName. |
| Niezgodność danych | Błędy podczas dopasowanie wygładzone kończy się niepowodzeniem, zgodnie z obiektów, które powodują błędy synchronizacji. |
| Błąd sprawdzania poprawności danych | Błędy spowodowane nieprawidłowych danych, takie jak znaki nieobsługiwane w krytycznych atrybuty, takie jak UserPrincipalName, formatowanie błędy, które kończą się niepowodzeniem sprawdzania poprawności przed zapisaniem w Azure AD.|
| Duży atrybut | Błędy podczas jednego lub kilku atrybutów są przekracza dozwolony rozmiar, długość lub liczność.|
| Inne | Wszystkie inne błędy, które nie mieszczą się w powyższych kategoriach. Na podstawie opinii, tej kategorii zostanie podzielona w pod kategorii.

![Synchronizowanie błędu raport Podsumowanie](./media/active-directory-aadconnect-health-sync/errorreport01.png)
![synchronizowanie kategorii raportów o błędzie](./media/active-directory-aadconnect-health-sync/errorreport02.png)

### <a name="list-of-objects-with-error-per-category"></a>Lista obiektów z powodu błędu według kategorii
Przechodzenie do każdej kategorii szczegółów zawiera listę obiektów o błędzie z tej kategorii.
![Lista raport błędów synchronizacji](./media/active-directory-aadconnect-health-sync/errorreport03.png)

### <a name="error-details"></a>Informacje o błędzie
Po danych jest dostępna w widoku szczegółowe dla każdego błędu

- Identyfikatory *Obiektu AD* związane
- Identyfikatory *Azure AD obiektu* związane (w stosownych przypadkach)
- Opis błędu i jak rozwiązać problem
- Artykuły pokrewne

![Szczegóły raportu o błędzie synchronizacji](./media/active-directory-aadconnect-health-sync/errorreport04.png)

### <a name="download-the-error-report-as-csv"></a>Pobierz raport o błędach jako CSV
Funkcja ta jest już wkrótce. Zamieścimy więcej aktualizacji.



## <a name="related-links"></a>Łącza pokrewne
* [Rozwiązywanie problemów z błędami podczas synchronizacji](active-directory-aadconnect-troubleshoot-sync-errors.md)
* [Atrybut zduplikowany elastyczności](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md)
* [Azure AD Connect kondycji](active-directory-aadconnect-health.md)
* [Azure AD Connect instalacji agenta kondycji](active-directory-aadconnect-health-agent-install.md)
* [Azure AD Connect operacje kondycji](active-directory-aadconnect-health-operations.md)
* [Podłącz za pomocą Azure AD kondycji z usług AD FS](active-directory-aadconnect-health-adfs.md)
* [Za pomocą Azure AD nawiązywanie połączenia z usługami AD DS kondycji](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect kondycji — często zadawane pytania](active-directory-aadconnect-health-faq.md)
* [Azure AD Connect kondycji Historia wersji](active-directory-aadconnect-health-version-history.md)
