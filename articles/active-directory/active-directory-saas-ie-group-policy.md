<properties
    pageTitle="Jak wdrożyć rozszerzenie panelu programu Access dla programu Internet Explorer za pomocą zasad grupy | Microsoft Azure"
    description="Jak wdrożyć dodatek programu Internet Explorer dla portalu Moje aplikacje za pomocą zasad grupy."
    services="active-directory"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>
<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/16/2016"
    ms.author="markvi"/>

#<a name="how-to-deploy-the-access-panel-extension-for-internet-explorer-using-group-policy"></a>Jak wdrożyć rozszerzenie panelu programu Access dla programu Internet Explorer za pomocą zasad grupy

Ten samouczek pokazano, jak za pomocą zasad grupy zdalnie Zainstaluj rozszerzenie panelu dostępu dla programu Internet Explorer na komputerach użytkowników. To rozszerzenie jest wymagana dla użytkowników programu Internet Explorer, musisz zalogować się do aplikacji, które są konfigurowane za pomocą [oparte na hasło logowania jednokrotnego](active-directory-appssoaccess-whatis.md#password-based-single-sign-on).

Zaleca się, że administratorzy automatyzowania wdrażania tego numeru wewnętrznego. W przeciwnym razie użytkownicy będą musieli Pobierz i zainstaluj rozszerzenie, który jest podatne na błąd użytkownika i wymaga uprawnień administratora. Ten samouczek obejmuje jedną metodę automatyzacji rozmieszczeń oprogramowania za pomocą zasad grupy. [Dowiedz się więcej na temat zasad grupy.](https://technet.microsoft.com/windowsserver/bb310732.aspx)

Rozszerzenie Panelu programu Access jest również dostępne dla [Chrome](https://go.microsoft.com/fwLink/?LinkID=311859) i [Firefox](https://go.microsoft.com/fwLink/?LinkID=626998), które wymagają uprawnień administratora do zainstalowania.

##<a name="prerequisites"></a>Wymagania wstępne

- Skonfigurowano [Usług domenowych Active Directory](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx)i komputerów użytkowników dołączyli do Twojej domeny.
- Wymagane są uprawnienia "Edytuj ustawienia" Aby można było edytować obiekty zasad grupy (GPO). Domyślnie członkowie grup zabezpieczeń następujące mieć to uprawnienie: Administratorzy domeny, Administratorzy przedsiębiorstwa i Twórcy-właściciele zasad grupy. [Dowiedz się więcej.](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx)

##<a name="step-1-create-the-distribution-point"></a>Krok 1: Tworzenie punktu dystrybucji

Najpierw należy umieścić pakiet Instalatora na lokalizacji sieciowej, która jest możliwy ze wszystkich komputerów, na których chcesz zdalnie zainstalować rozszerzenie na. Aby to zrobić, wykonaj następujące czynności:

1. Zaloguj się na serwerze jako administrator

2. W oknie **Menedżer serwerów** przejdź do **plików i usług magazynu**.

    ![Otwieranie plików i usług magazynu](./media/active-directory-saas-ie-group-policy/files-services.png)

3. Przejdź na kartę **Akcje** . Następnie kliknij pozycję **zadania** > **Nowy udział...**

    ![Otwieranie plików i usług magazynu](./media/active-directory-saas-ie-group-policy/shares.png)

4. **Kreator nowego udostępnianie** i ustawić uprawnienia, aby upewnić się, że będą one dostępne na komputerach użytkowników. [Dowiedz się więcej na temat akcji.](https://technet.microsoft.com/library/cc753175.aspx)

5. Pobierz następujące pakiet Instalatora systemu Microsoft Windows (plik msi): [Extension.msi Panel programu Access](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access Panel Extension.msi)

6. Skopiuj pakiet Instalatora do odpowiedniej lokalizacji w udziale.

    ![Kopiowanie pliku msi dla Ciebie już Udostępnij.](./media/active-directory-saas-ie-group-policy/copy-package.png)

8. Upewnij się, że programu komputerach klienckich będą mieli dostęp do pakiet Instalatora z udziału. 

##<a name="step-2-create-the-group-policy-object"></a>Krok 2: Tworzenie obiektu zasad grupy

1. Zaloguj się do serwera obsługującego instalacji usługach domenowych Active Directory (AD DS).

2. W Menedżerze serwera przejdź do pozycji **Narzędzia** > **Zarządzania zasadami grupy**.

    ![Przejdź do polecenia Narzędzia > zarządzania zasadami grupy](./media/active-directory-saas-ie-group-policy/tools-gpm.png)

3. W okienku po lewej stronie okna **Zarządzania zasadami grupy** umożliwia wyświetlanie hierarchii jednostkę organizacyjną (OU) i określić w zakresie, które chcesz zastosować zasady grupy. Na przykład konieczne może być wybierz małe OU wdrożenia kilku użytkowników do testowania lub może być wybierz najwyższego poziomu OU wdrożenia całej organizacji.

    > [AZURE.NOTE] Jeśli chcesz utworzyć lub edytować Twojej organizacji jednostek organizacyjnych, przełączać się ponownie do Menedżera serwera i przejdź do pozycji **Narzędzia** > **Użytkownicy usługi Active Directory i komputerami**.

4. Po wybraniu jednostkę Organizacyjną, kliknij prawym przyciskiem myszy jego i wybierz pozycję **Utwórz obiekt GPO w tej domenie i umieść tu łącze...**

    ![Utwórz nowy obiekt zasad grupy](./media/active-directory-saas-ie-group-policy/create-gpo.png)

5. W wierszu **Nowy obiekt zasad grupy** wpisz nazwę nowego obiektu zasad grupy.

    ![Nazwy nowego obiektu zasad grupy](./media/active-directory-saas-ie-group-policy/name-gpo.png)

6. Kliknij prawym przyciskiem myszy obiekt zasad grupy, która została właśnie utworzona, a następnie kliknij przycisk **Edytuj**.

    ![Edytuj nowy obiekt zasad grupy](./media/active-directory-saas-ie-group-policy/edit-gpo.png)

##<a name="step-3-assign-the-installation-package"></a>Krok 3: Przypisywanie pakiet instalacyjny

1. Określ, czy chcesz wdrożyć rozszerzenia na podstawie **Konfiguracji komputera** lub **Konfiguracja użytkownika**. Gdy używasz [Konfiguracja komputera](https://technet.microsoft.com/library/cc736413%28v=ws.10%29.aspx), rozszerzenie zostanie zainstalowana na komputerze, niezależnie od tego, które użytkownicy logują się go. Z drugiej strony, [Konfiguracja użytkownika](https://technet.microsoft.com/library/cc781953%28v=ws.10%29.aspx), użytkownicy będą mieć rozszerzenie zainstalowany dla nich bez względu na poszczególnych komputerach zalogują się do.

2. W lewym okienku okna **Edytor zarządzania zasadami grupy** przejdź do jednej z następujących ścieżki folderu, w zależności od typu konfiguracji wybierzesz:
    - `Computer Configuration/Policies/Software Settings/`
    - `User Configuration/Policies/Software Settings/`

3. Kliknij prawym przyciskiem myszy **Instalacja oprogramowania**, a następnie wybierz pozycję **Nowy** > **pakietu...**

    ![Tworzenie nowego pakietu instalacji oprogramowania](./media/active-directory-saas-ie-group-policy/new-package.png)

4. Przejdź do folderu udostępnionego, który zawiera pakiet Instalatora z [Krok 1: Tworzenie punktu dystrybucji](#step-1-create-the-distribution-point), wybierz plik msi i kliknij przycisk **Otwórz**.

    > [AZURE.IMPORTANT] Udostępnij znajduje się na tym samym serwerze, upewnij się, że uzyskujesz dostęp do msi za pośrednictwem sieci ścieżka pliku, a nie ścieżka pliku lokalnego.

    ![Wybierz pakiet instalacyjny w folderze udostępnionym.](./media/active-directory-saas-ie-group-policy/select-package.png)

5. W wierszu **Rozmieszczanie oprogramowania** wybierz **przypisane** dla metody wdrażania. Kliknij przycisk **OK**.

    ![Wybierz przypisane, a następnie kliknij przycisk OK.](./media/active-directory-saas-ie-group-policy/deployment-method.png)

Rozszerzenie został wdrożony do wybranego jednostkę Organizacyjną. [Dowiedz się, jak Instalacja oprogramowania zasad grupy.](https://technet.microsoft.com/library/cc738858%28v=ws.10%29.aspx)

##<a name="step-4-auto-enable-the-extension-for-internet-explorer"></a>Krok 4: Włączone automatycznie rozszerzenie dla programu Internet Explorer 

Oprócz uruchomiony Instalator, wszystkie rozszerzenia dla programu Internet Explorer musi być jawnie włączone zanim będzie można go używać. Wykonaj poniższe czynności, aby włączyć rozszerzenie panelu dostępu za pomocą zasad grupy:

1. W oknie **Edytor zarządzania zasadami grupy** przejdź do jednej z następujących ścieżek, w zależności od typu konfiguracji wybranego w [Krok 3: przypisywanie pakiet instalacyjny](#step-3-assign-the-installation-package):
    - `Computer Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`
    - `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`

2. Kliknij prawym przyciskiem myszy na **Liście dodatków**, a następnie kliknij przycisk **Edytuj**.
    ![Edytowanie listy dodatku.](./media/active-directory-saas-ie-group-policy/edit-add-on-list.png)

3. W oknie **Lista dodatków** wybierz opcję **włączone**. Następnie w sekcji **Opcje** kliknij pozycję **Pokaż...**.

    ![Kliknij przycisk Włącz, a następnie kliknij pozycję Pokaż...](./media/active-directory-saas-ie-group-policy/edit-add-on-list-window.png)

4. W oknie **Pokazywanie zawartości** wykonaj następujące czynności:

    1. Dla pierwszej kolumny (pole **Nazwa wartości** ) skopiuj i wklej następujący identyfikator klasy:`{030E9A3F-7B18-4122-9A60-B87235E4F59E}`

    2. Dla drugiej kolumny (pole **wartości** ) wpisz następujące wartości:`1`

    3. Kliknij **przycisk OK** , aby zamknąć okno **Pokazywanie zawartości** .

    ![Wypełnij wartości określonych powyżej.](./media/active-directory-saas-ie-group-policy/show-contents.png)

5. Kliknij **przycisk OK** , aby zastosować zmiany i zamknąć okno **Lista dodatków** .

Rozszerzenie teraz powinna być włączona na komputerach w wybrana jednostka Organizacyjna. [Dowiedz się więcej o używaniu zasad grupy, aby włączyć lub wyłączyć dodatki programu Internet Explorer.](https://technet.microsoft.com/library/dn454941.aspx)

##<a name="step-5-optional-disable-remember-password-prompt"></a>Krok 5 (opcjonalnie): Wyłącz monit "Zapamiętaj hasło"

Gdy użytkownicy logować się do witryn sieci Web przy użyciu rozszerzenie panelu programu Access, program Internet Explorer może wyświetlać następujący monit pytaniem "Czy chcesz przechowywać hasła?"

![](./media/active-directory-saas-ie-group-policy/remember-password-prompt.png)

Jeśli chcesz uniemożliwić użytkownikom wyświetlenia tego monitu, a następnie postępuj zgodnie z instrukcjami poniżej, aby zapobiec autouzupełniania z dokonywaniu haseł:

1. W oknie **Edytor zarządzania zasadami grupy** przejdź do ścieżki wymienione poniżej. Należy zauważyć, że to ustawienie konfiguracji jest dostępna tylko w obszarze **Konfiguracja użytkownika**.
    - `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/`

2. Znajdź ustawienie o nazwie **włączyć funkcję Autouzupełniania dla nazwy użytkownika i hasła w formularzach**.

    > [AZURE.NOTE] Poprzednie wersje usługi Active Directory może zawierać listę to ustawienie o nazwie **Autouzupełniania o zapisanie haseł nie jest możliwe**. Konfiguracja to ustawienie różni się od ustawienia opisane w tym samouczku.

    ![Pamiętaj ustalić to w obszarze Ustawienia użytkownika.](./media/active-directory-saas-ie-group-policy/disable-auto-complete.png)

3. Kliknij prawym przyciskiem myszy ustawienie powyżej, a następnie kliknij przycisk **Edytuj**.

4. W oknie tytuł, **Włącz funkcję Autouzupełniania dla nazwy użytkownika i hasła w formularzach**wybierz pozycję **wyłączone**.

    ![Wybierz przycisk Wyłącz](./media/active-directory-saas-ie-group-policy/disable-passwords.png)

5. Kliknij **przycisk OK** , aby zastosować te zmiany i zamknąć okno.

Użytkownicy nie będą już umożliwia przechowywanie ich poświadczeń lub Użyj Autouzupełniania, aby uzyskać dostęp do poprzednio przechowywanych poświadczeń. Jednak tych zasad umożliwia użytkownikom nadal korzystać z funkcji Autouzupełnianie dla pozostałych typów pól formularza, takich jak pola wyszukiwania.

> [AZURE.WARNING] Jeśli jest włączona po wybrano użytkowników do przechowywania niektórych poświadczeń, to będzie zasady *nie* wyczyść poświadczeń, które zostały już zapisane.

##<a name="step-6-testing-the-deployment"></a>Krok 6: Testowanie wdrażania

Postępuj zgodnie z instrukcjami poniżej, aby sprawdzić, jeśli wdrożenia rozszerzenia zakończyło się pomyślnie:

1. Jeśli wdrożono przy użyciu **Konfiguracja komputera**, zaloguj się do komputer kliencki należący do jednostkę Organizacyjną, do wybranej w [Krok 2: Tworzenie obiektu zasad grupy](#step-2-create-the-group-policy-object). Jeśli wdrożono przy użyciu **Konfiguracja użytkowników**, upewnij się zalogować się jako użytkownik, który należy do tej jednostce Organizacyjnej.

2. Może potrwać kilka logowania dodatki zasad grupy zmienia się na w pełni aktualizacji z tego komputera. Aby wymusić aktualizację, Otwórz okno **wiersza polecenia** , a następnie uruchom następujące polecenie:`gpupdate /force`

3. Będzie konieczne ponowne uruchomienie komputera instalacja być wykonywana. Uruchamiania może potrwać znacznie więcej niż instaluje stosowanego zwykle podczas rozszerzenia.

4. Po ponownym uruchomieniu komputera, otwórz **Program Internet Explorer**. W prawym górnym rogu okna kliknij menu **Narzędzia** (ikona koła zębatego), a następnie wybierz pozycję **Zarządzaj dodatkami**.

    ![Przejdź do polecenia Narzędzia > Zarządzaj dodatkami](./media/active-directory-saas-ie-group-policy/manage-add-ons.png)

5. W oknie **Zarządzaj dodatkami** Sprawdź zainstalowania **Rozszerzenie Panelu programu Access** i że jego **Stan** została ustawiona jako **włączone**.

    ![Sprawdź, czy rozszerzenie panelu programu Access jest zainstalowany i włączony.](./media/active-directory-saas-ie-group-policy/verify-install.png)

## <a name="related-articles"></a>Artykuły pokrewne

- [Artykuł indeks do zarządzania aplikacjami w usłudze Azure Active Directory](active-directory-apps-index.md)
- [Dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md)
- [Rozwiązywanie problemów z rozszerzenie panelu programu Access dla programu Internet Explorer](active-directory-saas-ie-troubleshooting.md)
