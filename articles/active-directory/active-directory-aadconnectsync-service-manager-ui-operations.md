<properties
    pageTitle="Azure AD Connect synchronizacją: interfejs użytkownika Menedżera usługi synchronizacji | Microsoft Azure"
    description="Opis na karcie operacje w Menedżerze usługi synchronizacji dla Azure AD Connect."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/07/2016"
    ms.author="billmath"/>


# <a name="azure-ad-connect-sync-synchronization-service-manager"></a>Azure AD Connect synchronizacją: Menedżer usługi synchronizacji

[Operacje](active-directory-aadconnectsync-service-manager-ui-operations.md) | [Łączniki](active-directory-aadconnectsync-service-manager-ui-connectors.md) | [Projektant metaverse](active-directory-aadconnectsync-service-manager-ui-mvdesigner.md) | [Wyszukiwanie metaverse](active-directory-aadconnectsync-service-manager-ui-mvsearch.md)
--- | --- | --- | ---

![Menedżer usługi synchronizacji](./media/active-directory-aadconnectsync-service-manager-ui/operations.png)

Na karcie operacje pokazano wyniki z ostatnich operacji. Ta karta jest klucz, opis i rozwiązywanie problemów.

## <a name="understand-the-information-visible-in-the-operations-tab"></a>Zrozumienie informacji widoczne na karcie operacji
Górna połowa zawiera wszystkie elementy stały zamówienia. Domyślnie dziennik operacji zachowuje informacji na temat ostatnich siedmiu dni, ale można zmienić to ustawienie z [harmonogramu](active-directory-aadconnectsync-feature-scheduler.md). Chcesz wyszukać dowolne Uruchom, który nie jest wyświetlany stan sukcesu. Możesz zmienić sortowania, klikając nagłówki.

W kolumnie **Stan** jest najważniejsze informacje i zawiera najbardziej trudnych problem w przypadku uruchomienia. Oto krótkie podsumowanie najbardziej typowych stanów w kolejności pierwszeństwa, którzy chcą (gdzie * wskazać kilka ciągów możliwy błąd).

Stan | Komentarz
--- | ---
Zatrzymano-* | Nie można ukończyć Uruchom. Jeśli na przykład system zdalny nie działa i nie jest możliwy.
Zatrzymano błąd limit | Występują błędy ponad 5000. Uruchom automatycznie został zatrzymany z powodu dużej liczby błędów.
ukończony -\*-błędy | Uruchom wykonane, ale występują błędy (mniej niż 5000), które należy zbadać.
ukończony -\*-ostrzeżenia | Uruchom wykonane, ale niektóre dane nie jest w stanie oczekiwany. Jeśli masz błędy, następnie ten komunikat jest zazwyczaj tylko objawem. Do momentu usunąć błędy, możesz nie badanie ostrzeżeń.
sukcesu | Nie ma problemów.

Po wybraniu wiersza dołu aktualizuje Pokaż szczegóły uruchomienia. Na lewo od dołu może być listy informacją **krok #**. Ta lista jest wyświetlana tylko, jeśli masz wiele domen w sieci miejsce, w którym każda domena jest reprezentowany przez kroku. Nazwa domeny znajduje się pod nagłówkiem **partycją**. W obszarze **Statystyk synchronizacji**można znaleźć więcej informacji na temat zmiany, które zostały przetworzone. Kliknięcie łącza, aby uzyskać listę zmienione obiekty. Jeśli masz obiektów z błędami tych błędów jest widoczna w obszarze **Błędów synchronizacji**.

## <a name="troubleshoot-errors-in-operations-tab"></a>Rozwiązywanie problemów z błędami na karcie operacji
![Menedżer usługi synchronizacji](./media/active-directory-aadconnectsync-service-manager-ui/errorsync.png)  
Jeśli błędy, obiekt w błędu i komunikat o błędzie się są łącza, które znajdziesz więcej informacji.

Rozpocznij od kliknięcia ciąg błędu (pozycję**wyzwalane synchronizacji — reguły — błąd funkcji** na obrazie). Najpierw przedstawiono omówienie obiektu. Aby wyświetlić błędu, kliknij przycisk **Śledzenie stosu**. Ten śledzenia informacje debugowania poziomu błędu.

**Porada:** Możesz kliknąć prawym przyciskiem myszy w polu **informacje stosu wywołań** , wybierz pozycję **Zaznacz wszystko**, a **Kopia**. Następnie możesz skopiować stosu i przyjrzyj się komunikat o błędzie w edytorze Ulubione, takim jak Notatnik.

- Jeśli komunikat o błędzie pochodzi od **SyncRulesEngine**, informacje stosu wywołań najpierw zawiera listę wszystkich atrybutów obiektu. Przewiń w dół, aż zostanie wyświetlony nagłówek **InnerException = >**.  
![Menedżer usługi synchronizacji](./media/active-directory-aadconnectsync-service-manager-ui/errorinnerexception.png)  
Wiersz po zawiera błąd. Na powyższym obrazie błąd jest z utworzony niestandardowy Fabrikam reguły synchronizacji.

Jeśli błąd się nie dają wystarczających informacji, nadszedł czas, aby przyjrzeć się dane. Kliknięcie łącza przy użyciu identyfikatora obiektu i [Wykonaj obiektu i jego danych za pomocą systemu](active-directory-aadconnectsync-service-manager-ui-connectors.md#follow-an-object-and-its-data-through-the-system).

## <a name="next-steps"></a>Następne kroki
Dowiedz się, jak [zsynchronizować Azure AD Connect](active-directory-aadconnectsync-whatis.md) konfiguracji.

Dowiedz się więcej o [integrowaniu lokalnego tożsamości z usługą Azure Active Directory](active-directory-aadconnect.md).
