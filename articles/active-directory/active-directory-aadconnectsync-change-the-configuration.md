<properties
    pageTitle="Azure AD Connect synchronizacją: jak wprowadzanie zmian w domyślnej konfiguracji | Microsoft Azure"
    description="Przeprowadzi Cię przez proces zmiany w konfiguracji synchronizacji Azure AD Connect."
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
    ms.date="08/31/2016"
    ms.author="billmath"/>


# <a name="azure-ad-connect-sync-how-to-make-a-change-to-the-default-configuration"></a>Azure AD Connect synchronizacją: jak wprowadzanie zmian w domyślnej konfiguracji
Celem w tym temacie jest przeprowadził Cię przez proces wprowadzania zmian w konfiguracji domyślnej Azure AD Connect synchronizacji. Procedura przewiduje kilka typowych scenariuszy. Znając to powinny mieć możliwość wprowadzanie prostych zmian własną konfigurację na podstawie własnego reguł biznesowych.

## <a name="synchronization-rules-editor"></a>Edytor reguł synchronizacji
Edytor reguł synchronizacja jest używany do wyświetlić i zmienić domyślną konfigurację. Możesz go znaleźć w grupie **Azure AD Connect** , w Menu Start.  
![Menu Start z Edytor reguł synchronizacji](./media/active-directory-aadconnectsync-change-the-configuration/startmenu2.png)

Po otwarciu go, zobacz domyślne reguły w nowym polu.

![Edytor reguł synchronizacji](./media/active-directory-aadconnectsync-change-the-configuration/sre2.png)

### <a name="navigating-in-the-editor"></a>Nawigowanie w edytorze
Listy rozwijane w górnej części edytora umożliwiają szybkie znajdowanie na określoną regułę. Na przykład jeśli chcesz wyświetlić zasady miejsce, w którym znajduje się atrybutu proxyAddresses, możesz zmienić listy rozwijane do następującego:  
![Filtrowanie SRE](./media/active-directory-aadconnectsync-change-the-configuration/filtering.png)  
Aby zresetować filtrowania i załadować świeży konfiguracji, naciśnij klawisz **F5** na klawiaturze.

Do góry po prawej stronie znajduje się przycisk **Dodaj nową regułę**. Ten przycisk służy do tworzenia niestandardowych reguły.

U dołu Jeśli masz przyciski służące do działania na reguły synchronizacji zaznaczonego. **Edytowanie** i **Usuwanie** wykonaj co oczekiwaniami. **Eksportowanie** tworzy skrypt programu PowerShell do odtworzenia reguły synchronizacji. Ta procedura umożliwia przeniesienie reguły synchronizacji z jednego serwera do drugiego.

## <a name="create-your-first-custom-rule"></a>Tworzenie pierwszej reguły niestandardowej
Zmiana najczęściej jest zmiany przepływów atrybut. Dane w katalogu źródłowym może nie być tak jak Azure AD. W tym przykładzie w tej sekcji chcesz upewnij się, że podana nazwa użytkownika jest zawsze z **wielkiej litery**.

### <a name="disable-the-scheduler"></a>Wyłączony
Domyślnie [Harmonogram](active-directory-aadconnectsync-feature-scheduler.md) uruchamiane co 30 minut. Chcesz upewnij się, że nie jest uruchomiona podczas wprowadzania zmian i rozwiązywanie problemów z nowych reguł. Aby tymczasowo wyłączyć harmonogram, uruchom program PowerShell i uruchamianie`Set-ADSyncScheduler -SyncCycleEnabled $false`

![Wyłączony](./media/active-directory-aadconnectsync-change-the-configuration/schedulerdisable.png)  

### <a name="create-the-rule"></a>Tworzenie reguły

1. Kliknij pozycję **Dodaj nową regułę**.
2. Na stronie **Opis** wprowadź następujące informacje:  
![Ruch przychodzący reguły filtrowania](./media/active-directory-aadconnectsync-change-the-configuration/description2.png)  
    - Nazwa: Nadawanie regule nazwę opisową.
    - Opis: Jakieś wyjaśnienia tak innej osoby mogą zrozumieć, co to jest dla reguły.
    - Połączony system: system obiektu można znaleźć w. W tym przypadku możemy zaznacz łącznik usługi Active Directory.
    - Typ obiektu połączonego systemu-Metaverse: Zaznaczanie **użytkownika** i **osoba** .
    - Typ łącza: Zmień tę wartość, aby **dołączyć do**.
    - Hierarchia ważności: Podaj wartości, który jest unikatowy w systemie. Niższe wartości liczbowej wskazuje wyższy priorytet.
    - Tag: Pozostaw puste. Tylko reguły z gotowych do firmy Microsoft powinny mieć to pole wypełnione z wartością.
3. Na stronie **Filtr Scoping** wprowadź **Imię ISNOTNULL**.  
![Reguła zakresu filtru ruchu przychodzącego](./media/active-directory-aadconnectsync-change-the-configuration/scopingfilter.png)  
Ta sekcja służy do określić, które obiekty, które należy zastosować regułę. Jeśli puste, reguła będzie zastosowane do wszystkich obiektów użytkowników. Ale czy zawierające sal konferencyjnych, konta usługi i innych obiektów użytkownika nie osób.
4. Na **Dołączanie reguł**pozostaw je puste.
5. Na stronie **przekształcenia** zmienić dla przepływu **wyrażenia**. Zaznacz docelowy atrybut **Imię**, a w źródle wprowadź `PCase([givenName])`.
![Reguła ruchu przychodzącego przekształcenia](./media/active-directory-aadconnectsync-change-the-configuration/transformations.png)  
Aparat synchronizacji jest uwzględniana wielkość liter, zarówno na nazwy funkcji i nazwa atrybutu. Jeśli wpiszesz występują problemy, zobacz ostrzeżenie podczas dodawania reguły. Edytor pozwala na Zapisz i Kontynuuj, więc musisz ponownie regułę i poprawić regułę.
6. Kliknij przycisk **Dodaj** , aby zapisać regułę.

Nowej reguły niestandardowej powinna być widoczna zasadom synchronizacji w systemie.

### <a name="verify-the-change"></a>Sprawdź zmiany
Z tej nowej zmiany chcesz działa zgodnie z oczekiwaniami i czy nie jest generowania błędy. W zależności od liczby obiektów, które masz istnieją dwa sposoby wykonywania tej czynności.

1. Uruchamianie pełnej synchronizacji wszystkich obiektów
2. Uruchom Podgląd i pełnej synchronizacji na jeden obiekt

Uruchom **Usługę synchronizacji** z start menu. Czynności opisane w tej sekcji są zapisane z tego narzędzia.

1. **Pełnej synchronizacji wszystkich obiektów**  
Zaznacz **Łączniki** u góry. Identyfikacja łącznika wprowadzone zmiany w poprzedniej sekcji, w tym przypadku usługach domenowych Active Directory, a następnie zaznacz go. Zaznacz pole wyboru **Uruchom** w akcje i wybierz **Pełnej synchronizacji** oraz **przycisk OK**.
![Pełnej synchronizacji](./media/active-directory-aadconnectsync-change-the-configuration/fullsync.png)  
Obiekty są teraz aktualizowane w metaverse. Teraz chcesz przejrzeć obiektu w metaverse.

2. **Wyświetl podgląd i pełnej synchronizacji na jeden obiekt**  
Zaznacz **Łączniki** u góry. Identyfikacja łącznika wprowadzone zmiany w poprzedniej sekcji, w tym przypadku usługach domenowych Active Directory, a następnie zaznacz go. Wybierz **Obszar łączników wyszukiwania**. Aby znaleźć obiekt, który chcesz przetestować zmiany, należy użyć zakresu. Zaznacz obiekt, a następnie kliknij pozycję **Podgląd**. Na ekranie nowe wybierz pozycję **Zatwierdzenie Podgląd**.
![Zatwierdź podglądu](./media/active-directory-aadconnectsync-change-the-configuration/commitpreview.png)  
Zmiana teraz poświęca metaverse.

**Przyjrzyj się obiektu w metaverse**  
Teraz chcesz wybrać kilka obiektów próbki do upewnij się, że jest oczekiwana wartość i stosowanie reguły. Wybierz opcję **Wyszukiwanie Metaverse** od góry. Dodaj wszystkie filtry, trzeba znaleźć odpowiednich obiektów. Na liście wyników wyszukiwania Otwórz obiektu. Przeglądanie wartości atrybutu, a także Sprawdź, czy w kolumnie **Reguły Synchronizuj** oczekiwanego reguły stosowane jako.  
![Wyszukiwanie metaverse](./media/active-directory-aadconnectsync-change-the-configuration/mvsearch.png)  
### <a name="enable-the-scheduler"></a>Włącz harmonogram
Jeśli wszystkie ustawienia są zgodnie z oczekiwaniami, możesz ponownie włączyć harmonogram. Z programu PowerShell, uruchom `Set-ADSyncScheduler -SyncCycleEnabled $true`.

## <a name="other-common-attribute-flow-changes"></a>Inne typowe zmiany przepływu atrybutów
Poprzedniej sekcji opisano sposób wprowadzania zmian do przepływu atrybut. W tej sekcji przedstawiono kilka przykładowych. Instrukcje dotyczące tworzenia reguły synchronizacji jest skrót, ale pełny czynności można znaleźć w poprzedniej sekcji.

### <a name="use-another-attribute-than-the-default"></a>Za pomocą atrybutu inny niż domyślny
Na Fabrikam istnieje las używania lokalne alfabetu imię, nazwisko i nazwę wyświetlaną. Przedstawienie znaku łacińskiego następujące atrybuty można znaleźć w atrybutów rozszerzenia. Podczas tworzenia na globalnej liście adresowej w Azure AD i usługi Office 365 organizacji chce następujące atrybuty opcję.

Z konfiguracją domyślną obiekt z lokalnym las wygląda następująco:  
![Atrybut przepływu 1](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp1.png)

Aby utworzyć regułę z innych przepływów atrybut, wykonaj następujące czynności:

- Uruchom **Edytor reguł synchronizacji** z start menu.
- **Przychodzące** nadal zaznaczony w lewo kliknij przycisk **Dodaj nową regułę**.
- Nadawanie regule nazwę i opis. Wybierz w lokalnej usłudze Active Directory i typami odpowiedniego obiektu.  W polu **Typ łącza**wybierz pozycję **Dołącz**. Pierwszeństwa wybierz numer, który nie jest używany przez inną regułę. Reguły w nowym polu rozpoczyna się od 100, dlatego wartość 50 może być używana w tym przykładzie.
![Atrybut przepływu 2](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp2.png)
- Pozostaw puste zakresu (to znaczy, ma być stosowane do wszystkich obiektów użytkowników w las).
- Pozostaw puste reguły sprzężenia (czyli umożliwić reguły w nowym polu obsługiwać wszystkie sprzężenia).
- W przekształcenia Utwórz przepływów następujące czynności:  
![Atrybut przepływu 3](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp3.png)
- Kliknij przycisk **Dodaj** , aby zapisać regułę.
- Przejdź do **Menedżera usługi synchronizacji**. **Łączniki**zaznacz łącznik miejsce, w którym dodano regułę. Wybierz pozycję **Uruchom**, a **pełnej synchronizacji**. Wszystkie obiekty przy użyciu reguł bieżącej pełnej synchronizacji jest obliczana ponownie.

Jest to wynik dla tego samego obiektu z tej reguły niestandardowej:  
![Atrybut przepływu 4](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp4.png)

### <a name="length-of-attributes"></a>Długość atrybutów
Atrybuty ciągu są domyślnie ustawiona jako można indeksować i maksymalna długość wynosi 448 znaków. Jeśli pracujesz z atrybutami parametry, które mogą zawierać więcej, następnie upewnij się, w procesie atrybut, są następujące:  
`attributeName` <- `Left([attributeName],448)`

### <a name="changing-the-userprincipalsuffix"></a>Zmienianie userPrincipalSuffix
Atrybut userPrincipalName w usłudze Active Directory nie jest zawsze znana przez użytkowników i nie mogą być stosowane jako identyfikator logowania. Azure AD Connect Kreatora instalacji synchronizacji umożliwia wybranie innego atrybut, na przykład poczty. Ale w niektórych przypadkach można obliczyć atrybutu. Na przykład firmy Contoso ma dwa poziomy Azure AD, jeden produkcji a drugi do testowania. Mają użytkowników w dzierżawie ich test, aby użyć innego sufiks w identyfikator logowania.  
`userPrincipalName` <- `Word([userPrincipalName],1,"@") & "@contosotest.com"`

W tym wyrażeniu wykonać wszystko po lewej stronie pierwszego @-sign (Word) i ZŁĄCZ.teksty ze środka ciągu.

### <a name="convert-a-multi-value-to-a-single-value"></a>Konwertowanie wielu wartości na pojedynczej wartości
Niektóre atrybuty w usłudze Active Directory są wielowartościowe w schemacie, chociaż wyglądają pojedynczej wartości w użytkownicy usługi Active Directory i komputerach. Przykładem jest atrybut Opis.  
`description` <- `IIF(IsNullOrEmpty([description]),NULL,Left(Trim(Item([description],1)),448))`

W tym wyrażeniu w przypadku, gdy atrybut ma wartość, możemy wykonać pierwszy element (element) w atrybucie, Usuń (przycinanie) spacje wiodące i końcowe, a następnie przechowuj (po lewej) najpierw 448 znaki w ciągu.

### <a name="do-not-flow-an-attribute"></a>Nie przepływ atrybutu
Aby uzyskać podstawowe informacje dotyczące tego scenariusza dla tej sekcji zobacz [Kontrolowanie proces blokowy atrybut](active-directory-aadconnectsync-understanding-declarative-provisioning.md#control-the-attribute-flow-process).

Istnieją dwa sposoby na nie atrybut. Pierwszy jest dostępny w Kreatorze instalacji i umożliwia [Usuwanie wybranych atrybutów](active-directory-aadconnect-get-started-custom.md#azure-ad-app-and-attribute-filtering). Ta opcja ma zastosowanie atrybutu przed nigdy nie zostały zsynchronizowane. Jednak jeśli zostało rozpoczęte Synchronizuj ten atrybut, a później usunąć go z tą funkcją, następnie stopnie aparatu synchronizacji Zarządzanie atrybutu i istniejących wartości pozostają w Azure AD.

Jeśli chcesz usunąć wartość atrybutu i upewnij się, że w przyszłości przechodzi potrzebujesz zamiast tego utworzyć regułę niestandardową.

U Fabrikam możemy zostały zrealizowane, że niektóre atrybuty, które było synchronizować w chmurze nie powinny być dostępne. Chcemy upewnij się, że następujące atrybuty są usuwane z usługi Azure Active Directory.  
![Nieprawidłowe rozszerzenie atrybutów](./media/active-directory-aadconnectsync-change-the-configuration/badextensionattribute.png)

- Utwórz nową regułę ruchu przychodzącego synchronizacji i wypełnić opis ![opisy](./media/active-directory-aadconnectsync-change-the-configuration/syncruledescription.png)
- Tworzenie przepływów atrybut typu **wyrażeń** i ze źródłem **AuthoritativeNull**. Literał **AuthoritativeNull** wskazuje, że wartość powinna być pusta w MV nawet wtedy, gdy dolnym reguły synchronizacji pierwszeństwa próbuje wypełnić wartość.
![Transformacja rozszerzenia atrybutów](./media/active-directory-aadconnectsync-change-the-configuration/syncruletransformations.png)
- Zapisz regułę synchronizacji. Uruchamiania **Usługi synchronizacji**, znajdź łącznik, wybierz pozycję **Uruchom**, a **Pełnej synchronizacji**. Ten krok jest obliczana ponownie wszystkie przepływy atrybut.
- Upewnij się, że zamierzone zmiany zamiar można eksportować przeszukując obszar łączników.
![Usuń etapowej](./media/active-directory-aadconnectsync-change-the-configuration/deletetobeexported.png)

## <a name="next-steps"></a>Następne kroki

- Dowiedz się więcej o modelu konfiguracji w [Opis deklaracyjnych-inicjowania obsługi administracyjnej](active-directory-aadconnectsync-understanding-declarative-provisioning.md).
- Dowiedz się więcej na temat języka wyrażeń w [Wyrażeniach opis deklaracyjnych inicjowania obsługi administracyjnej](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md).

**Przegląd tematów**

- [Azure AD Connect synchronizacją: opis i dostosowywanie synchronizacji](active-directory-aadconnectsync-whatis.md)
- [Integrowanie tożsamości lokalnych z usługi Azure Active Directory](active-directory-aadconnect.md)
