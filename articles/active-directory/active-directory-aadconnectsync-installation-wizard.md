<properties
    pageTitle="Azure AD Connect synchronizacją: uruchomienie Kreatora instalacji po raz drugi | Microsoft Azure"
    description="W tym miejscu wyjaśniono, działania Kreatora instalacji na drugim uruchomieniu go."
    keywords="Kreator instalacji narzędzie Azure AD Connect umożliwia skonfigurowanie ustawień konserwacji uruchomienia po raz drugi"
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


# <a name="azure-ad-connect-sync-running-the-installation-wizard-a-second-time"></a>Azure AD Connect synchronizacją: uruchomienie Kreatora instalacji po raz drugi
Przy pierwszym uruchomieniu Kreatora instalacji narzędzie Azure AD Connect go przeprowadzi Cię przez sposobu konfigurowania instalacji. Jeśli ponownie uruchom Kreatora instalacji oferuje opcje obsługi.

Kreator instalacji można znaleźć w menu start o nazwie **Narzędzie Azure AD Connect**.

![Start menu](./media/active-directory-aadconnectsync-installation-wizard/startmenu.png)

Po uruchomieniu Kreatora instalacji zostanie wyświetlona strona z następujących opcji:

![Strona z listy zadania dodatkowe](./media/active-directory-aadconnectsync-installation-wizard/additionaltasks.png)

Jeśli zainstalowano ADFS z Azure AD Connect, masz dodatkowe opcje. Dodatkowe opcje, które mają dla usług ADFS opisano w [zarządzaniu ADFS](active-directory-aadconnect-federation-management.md#ad-fs-management).

Wybierz jedną z tych zadań, a następnie kliknij przycisk **Dalej** , aby kontynuować.

> [AZURE.IMPORTANT] Gdy masz Kreatora instalacji Otwórz zawieszone są wszystkie operacje w aparacie synchronizacji. Upewnij się, że Zamknij kreatora instalacji zaraz po zakończeniu zmiany w konfiguracji.

## <a name="view-current-configuration"></a>Konfiguracja bieżącego widoku
Ta opcja umożliwia szybki podgląd skonfigurowanego opcje.

![Strona z listą wszystkie opcje i ich stanu](./media/active-directory-aadconnectsync-installation-wizard/viewconfig.png)

Kliknij przycisk **Poprzedni** , aby wrócić do widoku. Jeśli wybierzesz **wyjścia**, możesz zamknąć kreatora instalacji.

## <a name="customize-synchronization-options"></a>Dostosowywanie opcji synchronizacji
Ta opcja służy do wprowadzania zmian w konfiguracji synchronizacji. Wyświetlenie podzestawu opcji z ścieżka instalacji niestandardowej konfiguracji. Ta opcja jest widoczna, nawet jeśli początkowo używane Instalacja ekspresowa.

- [Dodawanie katalogów](active-directory-aadconnect-get-started-custom.md#connect-your-directories). Aby usunięcie katalogu, zobacz [Usuwanie łącznika](active-directory-aadconnectsync-service-manager-ui-connectors.md#delete).
- [Zmień domenę i jednostkę Organizacyjną, do filtrowania](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering).
- Filtrowanie Usuń grupę.
- [Zmienianie opcjonalne funkcje](active-directory-aadconnect-get-started-custom.md#optional-features).

Inne opcje z początkowej instalacji nie można zmienić i nie są dostępne. Dostępne są następujące opcje:

- Zmieniaj atrybutu być userPrincipalName i sourceAnchor.
- Zmień metodę łączenia dla obiektów z różnych las.
- Włączanie, oparte na grupach filtrowania.

## <a name="refresh-directory-schema"></a>Odśwież schemat katalogu
Ta opcja jest używana, jeśli zmienisz schematu w jednym z lokalnego programu lasy usług AD DS. Na przykład może zainstalowaniu programu Exchange lub uaktualnienie do systemu Windows Server 2012 schematu z obiektami urządzenia. W tym przypadku należy poinstruuj narzędzie Azure AD Connect ponownie odczytać schematu z usług AD DS i zaktualizować pamięci podręcznej. Ta akcja generuje również reguły synchronizacji. Jeśli dodasz schematu programu Exchange jako przykład reguł synchronizacji dla programu Exchange są dodawane do konfiguracji.

Po wybraniu tej opcji znajdują się wszystkie katalogi w konfiguracji. Możesz zachować ustawieniem domyślnym i Odśwież wszystkie lasów lub usuwanie zaznaczenia niektóre z nich.

![Strona z listą wszystkich katalogów w środowisku](./media/active-directory-aadconnectsync-installation-wizard/refreshschema.png)

## <a name="configure-staging-mode"></a>Konfigurowanie trybu tymczasowy
Ta opcja umożliwia włączanie i wyłączanie tymczasowy tryb na serwerze. Więcej informacji na temat tymczasowych tryb i jak są one używane, które można znaleźć w [operacji](active-directory-aadconnectsync-operations.md#staging-mode).

Opcja pokazuje, czy tymczasowego jest obecnie włączona lub wyłączona:  
![Opcja, która również jest wyświetlany bieżący stan tymczasowy tryb](./media/active-directory-aadconnectsync-installation-wizard/stagingmodecurrentstate.png)

Aby zmienić stan, zaznacz tę opcję i zaznacz lub usuń zaznaczenie pola wyboru.  
![Opcja, która również jest wyświetlany bieżący stan tymczasowy tryb](./media/active-directory-aadconnectsync-installation-wizard/stagingmodeenable.png)

## <a name="change-user-sign-in"></a>Zmienianie logowania użytkownika
Ta opcja umożliwia zmianę z synchronizacji haseł do Federacji lub na odwrót. Nie można zmienić na **Nieskonfigurowane**.

Aby uzyskać więcej informacji na temat tej opcji zobacz [logowania użytkownika](active-directory-aadconnect-user-signin.md#changing-user-sign-in-method).

## <a name="next-steps"></a>Następne kroki

- Dowiedz się więcej na temat modelu konfiguracji używane przez narzędzie Azure AD Connect synchronizacją w [Opis deklaracyjnych-inicjowania obsługi administracyjnej](active-directory-aadconnectsync-understanding-declarative-provisioning.md).

**Przegląd tematów**

- [Azure AD Connect synchronizacją: opis i dostosowywanie synchronizacji](active-directory-aadconnectsync-whatis.md)
- [Integrowanie tożsamości lokalnych z usługi Azure Active Directory](active-directory-aadconnect.md)
