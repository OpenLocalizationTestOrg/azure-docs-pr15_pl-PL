<properties
    pageTitle="Azure AD Connect synchronizacją: interfejs użytkownika Menedżera usługi synchronizacji | Microsoft Azure"
    description="Opis na karcie łączniki w Menedżerze usługi synchronizacji dla Azure AD Connect."
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

![Menedżer usługi synchronizacji](./media/active-directory-aadconnectsync-service-manager-ui/connectors.png)

Karta łączników jest używana do zarządzania wszystkich systemach, którym jest połączona aparatu synchronizacji.

## <a name="connector-actions"></a>Akcje łącznika

Akcja | Komentarz
--- | ---
Tworzenie | Nie należy używać. Nawiązywanie połączenia z dodatkowych lasów AD, należy użyć Kreatora instalacji.
Właściwości | Używany do domeny i jednostkę Organizacyjną, do filtrowania.
[Usuwanie](#delete) | Używane do albo usunąć dane w obszarze, łącznika lub aby usunąć połączenie z las.
[Konfigurowanie profili uruchamiania](#configure-run-profiles) | Z wyjątkiem domeny filtrowanie nic, aby skonfigurować tutaj. Ta akcja umożliwia Zobacz już skonfigurowaną profili uruchamiania.
Uruchom | Użyty do uruchomienia jednorazowe Uruchom profilu.
Zatrzymywanie | Zatrzymuje łącznik obecnie uruchomione profilu.
Eksportowanie łącznika | Nie należy używać.
Importowanie łącznika | Nie należy używać.
Aktualizacja łącznika | Nie należy używać.
Odśwież schemat | Umożliwia odświeżenie pamięci podręcznej schematu. Zaleca należy korzystać z opcji w Kreatorze instalacji też od którego aktualizacje synchronizować reguły.
[Obszar łączników wyszukiwania](#search-connector-space) | Umożliwia znajdowanie obiektów i obserwować [obiektu i jego danych za pomocą systemu](#follow-an-object-and-its-data-through-the-system).

### <a name="delete"></a>Usuwanie
Akcja Usuń jest używana dla dwóch różnych operacji.
![Menedżer usługi synchronizacji](./media/active-directory-aadconnectsync-service-manager-ui/connectordelete.png)

Opcja **Usuń łącznik spację tylko** spowoduje usunięcie wszystkich danych, ale zachować konfiguracji.

Opcja **Usuń łącznik i obszar łączników** usuwa dane i konfigurację. Ta opcja jest używana, gdy nie chcesz już nawiązywanie połączenia z las.

Obie opcje synchronizacji wszystkich obiektów i zaktualizuj obiektów metaverse. Ta akcja jest długotrwałych operacji.

### <a name="configure-run-profiles"></a>Konfigurowanie profili uruchamiania
Ta opcja umożliwia wyświetlenie profili uruchamiania skonfigurowane dla łącznika.

![Menedżer usługi synchronizacji](./media/active-directory-aadconnectsync-service-manager-ui/configurerunprofiles.png)

### <a name="search-connector-space"></a>Obszar łączników wyszukiwania
Akcja miejsca łącznik wyszukiwania jest umożliwia znajdowanie obiektów i rozwiązywanie problemów z danymi.

![Menedżer usługi synchronizacji](./media/active-directory-aadconnectsync-service-manager-ui/cssearch.png)

Rozpocznij, wybierając **zakres**. Można przeszukiwać na podstawie danych (RDN, nazwy domeny, punktów kontrolnych, drzewa podrzędnego), lub obiektu (wszystkie pozostałe opcje).  
![Menedżer usługi synchronizacji](./media/active-directory-aadconnectsync-service-manager-ui/cssearchscope.png)  
Jeśli na przykład wykonywać wyszukiwanie podrzędny drzewa, możesz uzyskać wszystkich obiektów w jednej jednostce Organizacyjnej.
![Menedżer usługi synchronizacji](./media/active-directory-aadconnectsync-service-manager-ui/cssearchsubtree.png) w tym miejscu można wybrać obiekt, wybierz polecenie **Właściwości**i [po nim](#follow-an-object-and-its-data-through-the-system) z obszaru łącznik źródła, za pośrednictwem metaverse i do miejsca docelowego łącznik.

## <a name="follow-an-object-and-its-data-through-the-system"></a>Wykonaj obiektu i jego danych za pomocą systemu
Podczas rozwiązywania problemu z danymi, wykonaj obiektu z obszaru łącznik źródła do metaverse, a do elementu docelowego miejsca łącznik jest klucza procedury, aby dowiedzieć się, dlaczego danych nie ma oczekiwanych wartości.

### <a name="connector-space-object-properties"></a>Właściwości obiektu miejsca łącznika
**Importowanie**  
Po otwarciu obiektu cs, istnieje kilka kart u góry. Na karcie **Importowanie** zawiera dane, które są umieszczane po zaimportowaniu.
![Menedżer usługi synchronizacji](./media/active-directory-aadconnectsync-service-manager-ui/csimport.png) **Stara wartość** wskazuje, co obecnie znajduje się w systemie i **Nową wartość** co otrzymano w źródłowym systemie i nie została jeszcze pobrana. W tym przypadku ponieważ występuje błąd synchronizacji, nie można zastosować zmiany.

**Błąd**  
Strony błędu jest widoczna tylko jeśli występuje problem z obiektem. Zobacz szczegóły na stronie Operacje, aby uzyskać więcej informacji na temat rozwiązywania problemów z [błędami synchronizacji](active-directory-aadconnectsync-service-manager-ui-operations.md#troubleshoot-errors-in-operations-tab).

**Dotyczące elementów nadrzędnych**  
Na karcie dotyczące elementów nadrzędnych pokazano, jak obiekt miejsca łącznika jest powiązana z obiektu metaverse. Widać importowane łącznik ostatniej zmiany z połączonym systemem i które reguły stosowane do wypełnianie danych w metaverse.
![Menedżer usługi synchronizacji](./media/active-directory-aadconnectsync-service-manager-ui/cslineage.png) w kolumnie **Akcja** widoczny jest jedna reguła synchronizacji **przychodzące** z akcją **świadczenia**. Wskazująca, że dopóki ma ten obiekt miejsca łącznika, pozostaje obiektu metaverse. Lista reguł synchronizacji zamiast tego zawiera reguła synchronizacji z kierunku **wychodzące** i **dostarczanie**, wskazuje, że ten obiekt jest usunięte, gdy obiektu metaverse zostanie usunięty.
![Menedżer usługi synchronizacji](./media/active-directory-aadconnectsync-service-manager-ui/cslineageout.png) można też wyświetlić w kolumnie **PasswordSync** że miejsca łącznika ruchu przychodzącego może przyczynić się zmiany hasła, ponieważ jedna reguła synchronizacji ma wartość **PRAWDA**. To hasło następnie są wysyłane do Azure AD za pomocą reguły wychodzącej.

Na karcie dotyczące elementów nadrzędnych można przejść do metaverse, klikając pozycję [Właściwości obiektu Metaverse](#metaverse-object-properties).

U dołu wszystkich kartach znajdują się dwa przyciski: **Podgląd** i **dziennika**.

**Podgląd**  
Na stronie podglądu służy do synchronizowania jednego jeden obiekt. Jest on przydatny, gdy rozwiązywania niektóre reguły synchronizacji klienta i chcesz, aby zobaczyć efekt zmian na jeden obiekt. Możesz wybrać między **Pełnej synchronizacji** i **Delta synchronizacji**. Możesz również wybrać między **Generowanie podglądu**, który przechowuje tylko zmiany w pamięci i **Zatwierdzanie Podgląd**, które stany wszystkich zmieni się na spacje łącznik docelowej.
![Menedżer usługi synchronizacji](./media/active-directory-aadconnectsync-service-manager-ui/preview1.png) można sprawdzić obiekt i reguły, które stosowana do przepływu określonego atrybutu.
![Menedżer usługi synchronizacji](./media/active-directory-aadconnectsync-service-manager-ui/preview2.png)

**Log**  
Strona dziennika jest używana Aby wyświetlić stan synchronizacji haseł i historii, zobacz [Rozwiązywanie problemów z synchronizacją haseł](active-directory-aadconnectsync-implement-password-synchronization.md#troubleshoot-password-synchronization) , aby uzyskać więcej informacji.

### <a name="metaverse-object-properties"></a>Właściwości obiektu Metaverse
**Atrybuty**  
Na karcie atrybuty można zobaczyć wartości, a które łącznik przyczynić się go.
![Menedżer usługi synchronizacji](./media/active-directory-aadconnectsync-service-manager-ui/mvattributes.png)
**łączników**  
Na karcie łączników zawiera wszystkie spacje łącznika, które mają postać obiektu.
![Menedżer usługi synchronizacji](./media/active-directory-aadconnectsync-service-manager-ui/mvconnectors.png) ta karta umożliwia przechodzenie do [łącznika miejsca obiektu](#connector-space-object-properties).

## <a name="next-steps"></a>Następne kroki
Dowiedz się, jak [zsynchronizować Azure AD Connect](active-directory-aadconnectsync-whatis.md) konfiguracji.

Dowiedz się więcej o [integrowaniu lokalnego tożsamości z usługą Azure Active Directory](active-directory-aadconnect.md).
