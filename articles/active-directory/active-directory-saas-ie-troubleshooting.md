<properties
    pageTitle="Rozwiązywanie problemów z rozszerzenie panelu programu Access dla programu Internet Explorer | Microsoft Azure"
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

#<a name="troubleshooting-the-access-panel-extension-for-internet-explorer"></a>Rozwiązywanie problemów z rozszerzenie panelu programu Access dla programu Internet Explorer

W tym artykule ułatwią rozwiązania następujących problemów:

- Nie możesz uzyskać dostęp do aplikacji za pośrednictwem portalu Moje aplikacje przy użyciu programu Internet Explorer.
- Zostanie wyświetlony komunikat "Instalowanie oprogramowania", nawet jeśli po zainstalowaniu oprogramowania.

Jeśli jesteś administratorem, zobacz też: [jak wdrożyć rozszerzenie panelu programu Access dla programu Internet Explorer za pomocą zasad grupy](active-directory-saas-ie-group-policy.md)

##<a name="run-the-diagnostic-tool"></a>Uruchom narzędzie diagnostyczne

Problemy z instalacją z rozszerzeniem panelu programu Access można diagnozowanie, pobierając i uruchamianie narzędzia diagnostyczne Panel dostępu:

1. [Kliknij tutaj, aby pobrać narzędzie diagnostyczne.](https://account.activedirectory.windowsazure.com/applications/AccessPanelExtensionDiagnosticTool/AccessPanelExtensionDiagnosticTool.zip)

2. Otwórz plik, a następnie naciśnij przycisk **wyodrębnić wszystkie** .

    ![Naciśnij klawisz wyodrębnić wszystkie](./media/active-directory-saas-ie-troubleshooting/extract1.png)

3. Naciśnij przycisk **Wyodrębnianie** , aby kontynuować.

    ![Naciśnij klawisz Wyodrębnij](./media/active-directory-saas-ie-troubleshooting/extract2.png)

4. Aby uruchomić narzędzie, kliknij prawym przyciskiem myszy plik o nazwie **AccessPanelExtensionDiagnosticTool**, a następnie wybierz pozycję **Otwórz za pomocą > Microsoft Windows Based Script Host**.

    ![Otwórz za pomocą > System Microsoft Windows podstawie hosta skryptów](./media/active-directory-saas-ie-troubleshooting/open_tool.png)

5. Następnie zostanie wyświetlony następujący oknie diagnostyczne w tym artykule opisano, co może być problem z instalacją pakietu.

    ![Przykładowe okna diagnostyczne](./media/active-directory-saas-ie-troubleshooting/tool_preview.png)

6. Kliknij przycisk "**Tak**" Aby pozwolić, aby program Rozwiązywanie problemów, które zostały odnalezione.

7. Aby zapisać te zmiany, zamknij okien programu Internet Explorer, a następnie otwórz go ponownie.<br />Jeśli nadal nie możesz uzyskać dostęp do aplikacji, spróbuj wykonać poniższe czynności.

##<a name="check-that-the-access-panel-extension-is-enabled"></a>Sprawdź, czy jest włączona rozszerzenie panelu programu Access

Aby sprawdzić, czy rozszerzenie panelu programu Access jest włączona w programie Internet Explorer:

1. W programie Internet Explorer kliknij **ikonę koła zębatego** w prawym górnym rogu okna. Wybierz pozycję **Opcje internetowe**.<br />(W starszych wersjach programu Internet Explorer można go znaleźć w obszarze **Narzędzia > Opcje internetowe**.

    ![Przejdź do polecenia Narzędzia > Opcje internetowe](./media/active-directory-saas-ie-troubleshooting/internetoptions.png)

2. Kliknij kartę **Programy** , a następnie kliknij przycisk **Zarządzaj dodatkami** .

    ![Kliknij pozycję Zarządzaj dodatkami](./media/active-directory-saas-ie-troubleshooting/internetoptions_programs.png)

3. W tym oknie dialogowym wybierz **Rozszerzenie Panelu programu Access** , a następnie kliknij przycisk **Włącz** .

    ![Kliknij pozycję Włącz](./media/active-directory-saas-ie-troubleshooting/enableaddon.png)

4. Aby zapisać te zmiany, zamykanie okien programu Internet Explorer, a następnie otwórz go ponownie.

##<a name="enable-extensions-for-inprivate-browsing"></a>Włącz rozszerzenia do przeglądania InPrivate

Jeśli korzystasz z trybu przeglądania InPrivate:

1. W programie Internet Explorer kliknij **ikonę koła zębatego** w prawym górnym rogu okna. Wybierz pozycję **Opcje internetowe**.<br />(W starszych wersjach programu Internet Explorer można go znaleźć w obszarze **Narzędzia > Opcje internetowe**.

    ![Przykładowe okna diagnostyczne](./media/active-directory-saas-ie-troubleshooting/inprivateoptions.png)

2. Przejdź na kartę **Prywatność** , następnie **Wyczyść** pole wyboru **Wyłącz paski narzędzi i rozszerzenia wraz z przeglądanie InPrivate**</p>

    ![Wyczyść pole wyboru Wyłącz paski narzędzi i rozszerzenia wraz z przeglądanie InPrivate](./media/active-directory-saas-ie-troubleshooting/enabletoolbars.png)

3. Aby zapisać te zmiany, zamykanie okien programu Internet Explorer, a następnie otwórz go ponownie.

##<a name="uninstall-the-access-panel-extension"></a>Odinstalowywanie rozszerzenie panelu programu Access

Aby odinstalować rozszerzenie panelu dostęp z komputera:

1. Na klawiaturze naciśnij **klawisz systemu Windows** , aby otworzyć Start menu. Po otwarciu menu możesz wpisać niczego do wyszukiwania. Wpisz "Panel sterowania", a następnie otwórz **Panel sterowania** , gdy zostanie ono wyświetlone w wynikach wyszukiwania.

    ![Wyszukiwanie w Panelu sterowania](./media/active-directory-saas-ie-troubleshooting/search_sm.png)

2. W prawym górnym rogu panelu sterowania należy zmienić opcję **Wyświetl według** **duże**ikony. Następnie znajdź i kliknij przycisk **Programy i funkcje** .

    ![Zmi widok, aby wyświetlić duże ikony](./media/active-directory-saas-ie-troubleshooting/control_panel.png)

3. Z listy wybierz **Rozszerzenie Panelu programu Access**i kliknij przycisk **Odinstaluj** .

    ![Kliknij przycisk Odinstaluj](./media/active-directory-saas-ie-troubleshooting/uninstall.png)

4. Następnie można spróbuj zainstalować rozszerzenia ponownie, aby sprawdzić, czy problem został rozwiązany.

Jeśli występują problemy z rozszerzeniem odinstalowanie, można też można usunąć za pomocą narzędzia [Microsoft rozwiązać go](https://go.microsoft.com/?linkid=9779673) .

## <a name="related-articles"></a>Artykuły pokrewne

- [Artykuł indeks do zarządzania aplikacjami w usłudze Azure Active Directory](active-directory-apps-index.md)
- [Dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md)
- [Jak wdrożyć rozszerzenie panelu programu Access dla programu Internet Explorer za pomocą zasad grupy](active-directory-saas-ie-group-policy.md)
