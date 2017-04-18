<properties
   pageTitle="Azure AD Connect synchronizacją: zapobiec przypadkowym usuwa | Microsoft Azure"
   description="W tym temacie opisano funkcję Zapobieganie przypadkowego usunięcia (zapobieganie przypadkowego usunięcia) w narzędzie Azure AD Connect."
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
   ms.workload="identity"
   ms.date="09/01/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-sync-prevent-accidental-deletes"></a>Azure AD Connect synchronizacją: Zapobiegaj przypadkowym usunięciom
W tym temacie opisano funkcję Zapobieganie przypadkowe usunąć (zapobieganie przypadkowe usunięcia) w narzędzie Azure AD Connect.

Po zainstalowaniu narzędzie Azure AD Connect, zapobiec przypadkowym usuwa jest domyślnie włączone i skonfigurowane tak, aby nie Eksportuj z więcej niż 500 usuwa. Ta funkcja służy do ochronę przed konfiguracji przypadkowe zmiany i do katalogu lokalnego, który może mieć wpływ na wielu użytkowników i innych obiektów.

Typowe scenariusze po wyświetleniu wiele usuwa obejmują:

- Zmieniony w celu [filtrowania](active-directory-aadconnectsync-configure-filtering.md) , gdzie cały [OU](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering) lub [domena](active-directory-aadconnectsync-configure-filtering.md#domain-based-filtering) nie jest zaznaczona.
- Wszystkie obiekty w jednostce Organizacyjnej zostaną usunięte.
- Jednostka Organizacyjna jest zmieniana tak wszystkich obiektów w nim są uznawane za poza zakresem synchronizacji.

Wartość domyślna 500 obiektów można zmienić przy użyciu programu PowerShell przy użyciu `Enable-ADSyncExportDeletionThreshold`. Należy skonfigurować tę wartość w celu dopasowania do rozmiaru organizacji. Ponieważ Harmonogram synchronizacji uruchamia co 30 minut, wartość jest liczba usuwa widoczne w ciągu 30 minut.

W przypadku zbyt wiele usuwa umieszczane w można eksportować do Azure AD następnie przestaje eksportowania i odbieranie wiadomości e-mail w następujący sposób:

![Zapobieganie przypadkowemu usuwa wiadomości e-mail](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/email.png)

> *Witaj (kontakt techniczne). Godzinie usługi synchronizacji tożsamości wykrył, że liczba operacji usunięcia Przekroczono próg usunięcie skonfigurowane dla (nazwa organizacji). Suma (liczba) obiektów zostały wysłane do usunięcia w tym synchronizacja tożsamości. To osiągnięcia lub przekroczenia progu skonfigurowaną usunięcie obiektów (liczba). Administrator powinien potwierdź że te usunięcia powinny być przetwarzane, aby można było będzie kontynuować. Zobacz zapobieganie usunięcia przypadkowe, aby uzyskać więcej informacji o błędzie wymienionych w tej wiadomości e-mail.*

Można też wyświetlić stan `stopped-deletion-threshold-exceeded` podczas przeglądania w **Menedżerze usługi synchronizacji** interfejsu użytkownika dla profilu eksportu.
![Zapobiec przypadkowym usuwa interfejs użytkownika Menedżera usługi synchronizacji](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/syncservicemanager.png)

Jeśli to oczekiwano badanie oraz podejmowania działań naprawczych. Aby zobaczyć, które obiekty mają zostać usunięte, wykonaj następujące czynności:

1. Uruchom **usługę synchronizacji** z Start Menu.
2. Przejdź do **łączników**.
3. Zaznacz łącznik typu **Usługi Azure Active Directory**.
4. W obszarze **Akcje** w prawo zaznacz **Obszar łączników wyszukiwania**.
5. W oknie podręcznym w obszarze **zakres**wybierz **Odłączona od** , a następnie wybierz czas w przeszłości. Kliknij przycisk **Wyszukaj**. Ta strona zawiera widok wszystkie obiekty, które ma zostać usunięta. Klikając poszczególne elementy, możesz uzyskać dodatkowe informacje o obiekcie. Możesz również kliknąć **Ustawienie kolumny** , aby dodać dodatkowe atrybuty, które mają być widoczne w siatce.

![Obszar łączników wyszukiwania](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/searchcs.png)

Jeśli wszystkie usuwa są konieczne, wykonaj następujące czynności:

1. Tymczasowe wyłączanie ochrony i umożliwić tych usuwa przejść, uruchom polecenie cmdlet programu PowerShell: `Disable-ADSyncExportDeletionThreshold`. Podaj nazwę i hasło konta administratora globalnego Azure AD.
![Poświadczenia](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/credentials.png)
2. Azure łącznika usługi Active Directory nadal zaznaczony wybierz akcję, **Uruchom** i wybierz pozycję **Eksportuj**.
3. Aby ponownie włączyć ochronę, uruchom polecenie cmdlet programu PowerShell: `Enable-ADSyncExportDeletionThreshold`.

## <a name="next-steps"></a>Następne kroki

**Przegląd tematów**

- [Azure AD Connect synchronizacją: opis i dostosowywanie synchronizacji](active-directory-aadconnectsync-whatis.md)
- [Integrowanie tożsamości lokalnych z usługi Azure Active Directory](active-directory-aadconnect.md)
