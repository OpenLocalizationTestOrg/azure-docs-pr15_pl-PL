<properties
    pageTitle="Azure Active Directory Domain Services: Administrowanie DNS w domenach zarządzanych | Microsoft Azure"
    description="Administrowanie DNS w domenach zarządzanych Azure Active Directory Domain Services"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="maheshu"/>

# <a name="administer-dns-on-an-azure-ad-domain-services-managed-domain"></a>Administrowanie DNS w domenie usług domenowych AD Azure zarządzanych
Azure Active Directory Domain Services zawiera serwera DNS (rozpoznawanie nazw domen), który zawiera rozpoznawania nazw DNS dla domeny zarządzane. Czasami może być konieczne skonfigurowania usługi DNS na zarządzanych domeny. Może być konieczne tworzenie rekordów DNS dla komputerów, które nie są połączone z domeną, konfigurowanie wirtualnych adresów IP dla urządzenia do równoważenia obciążenia lub konfigurowanie zewnętrznego serwera DNS. Z tego powodu użytkownicy, którzy należą do grupy "Administratorzy AAD kontrolera domeny" udzielono uprawnień administracyjnych DNS w domenie zarządzane.


## <a name="before-you-begin"></a>Przed rozpoczęciem
Aby wykonać zadania, wymienione w tym artykule, należy następująco:

1. Ważna **Subskrypcja Azure**.

2. Usługi **katalogowej Azure AD** -, albo zsynchronizowane z katalogu lokalnego lub w katalogu tylko do chmury.

3. **Usługi Azure AD domeny** musi być włączona dla katalogu Azure AD. Jeśli nie zostało to zrobione, wykonaj wszystkie zadania opisane w [Przewodnik wprowadzający dla](./active-directory-ds-getting-started.md).

4. **Domeny maszyn wirtualnych** z której administrować Azure AD domenie usług domenowych zarządzane. Jeśli nie masz maszyny wirtualnej, wykonaj wszystkie zadania opisane w artykuł [Dołączanie maszyny wirtualnej systemu Windows z domeną zarządzane](./active-directory-ds-admin-guide-join-windows-vm.md).

5. Potrzebujesz poświadczeń **konta użytkownika należącego do grupy "Administratorzy AAD kontrolera domeny"** w katalogu administrowania DNS dla domeny zarządzane.

<br>

## <a name="task-1---provision-a-domain-joined-virtual-machine-to-remotely-administer-dns-for-the-managed-domain"></a>Zadanie 1 - zapewnienie maszyny wirtualnej domeny zdalnego administrowania DNS dla domeny zarządzanych
Azure domen zarządzanych w usługach domenowych AD można zarządzać zdalnie przy użyciu znanych narzędzi administracyjnych usługi Active Directory takich jak Centrum administracyjne Active Directory (ADAC) lub AD programu PowerShell. Podobnie DNS dla domeny zarządzanych można administrować zdalnie przy użyciu narzędzi administracyjnych serwera DNS.

Administratorzy w katalogu Azure AD nie ma uprawnień do połączenia się z kontrolerami domeny w domenie zarządzane za pomocą pulpitu zdalnego. Członkowie grupy "Administratorzy AAD kontrolera domeny" mogą administrować DNS dla domen zarządzane zdalnie przy użyciu narzędzi serwera DNS klienta serwera systemu Windows z komputera z systemem przyłączonym do domeny zarządzane. Narzędzia serwera DNS można zainstalować jako część opcjonalną funkcją Narzędzia administracji zdalnej serwera (RSAT) na serwerze systemu Windows i komputerów klienckich dołączony do zarządzanych domeny.

Pierwsze zadanie jest do zapewniania obsługi maszyn wirtualnych systemu Windows Server o jest połączony z domeną zarządzane. Aby uzyskać instrukcje zapoznaj się z artykułem tytuł, [Dołącz do systemu Windows Server maszyny wirtualnej w domenie usług domenowych AD Azure zarządzane](active-directory-ds-admin-guide-join-windows-vm.md).


## <a name="task-2---install-dns-server-tools-on-the-virtual-machine"></a>Zadanie 2 — narzędzia serwera DNS instalacji na komputerze wirtualnych
Wykonaj poniższe czynności, aby zainstalować narzędzia administracyjne DNS na tym komputerze wirtualnych domeny sprzężone. Aby uzyskać więcej informacji o [instalowaniu i przy użyciu narzędzia administracji zdalnej serwera](https://technet.microsoft.com/library/hh831501.aspx)zobacz witrynę Technet.

1. Przejdź do węzła **maszyn wirtualnych** w portalu klasyczny Azure. Zaznacz maszyny wirtualnej utworzone w zadaniu 1, a następnie kliknij przycisk **Połącz** na pasku poleceń w dolnej części okna.

    ![Nawiązywanie połączenia z maszyn wirtualnych systemu Windows](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)

2. Portalu klasycznym wyświetli monit o otwarcie lub zapisanie pliku z rozszerzeniem ".rdp", który jest używany do łączenia maszyn wirtualnych. Kliknij plik, po zakończeniu pobierania.

3. W wierszu logowania Użyj poświadczeń użytkownika należącego do grupy "Administratorzy AAD kontrolera domeny". Na przykład firma Microsoft korzysta z 'bob@domainservicespreview.onmicrosoft.com' w naszym przypadku.

4. Na ekranie startowym Otwórz **Menedżera serwera**. Kliknij pozycję **Dodaj role i funkcje** w okienku centralnej strony w oknie Menedżer serwera.

    ![Uruchamianie Menedżera serwera maszyn wirtualnych](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager.png)

5. Na stronie **Przed rozpoczęciem** **dodawania ról i funkcje kreatora**kliknij przycisk **Dalej**.

    ![Przed rozpoczęciem strony](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-begin.png)

6. Na stronie **Typ instalacji** pozostaw wybraną opcję **Instalacja oparta na rolach lub funkcji** zaznaczone pole wyboru, a następnie kliknij przycisk **Dalej**.

    ![Strona Typ instalacji](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-type.png)

7. Na stronie **Wybieranie serwera** wybierz bieżącej maszyny wirtualnej z puli serwera, a następnie kliknij przycisk **Dalej**.

    ![Strona wyboru serwera](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-server.png)

8. Na stronie **Role serwerów** kliknij przycisk **Dalej**. Ponieważ nie możemy instalacji ról na serwerze, pominąć tę stronę.

9. Na stronie **Funkcje** kliknij, aby rozwinąć węzeł **Narzędzia administracji zdalnej serwera** , a następnie kliknij pozycję do rozwiń węzeł **Narzędzia administracyjne roli** . Funkcja **Narzędzia serwera DNS** z listy narzędzia administracyjne roli.

    ![Funkcje strony](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-dns-tools.png)

10. Na stronie **potwierdzenia** kliknij przycisk **Zainstaluj** , aby zainstalować funkcję narzędzia serwera DNS na tym komputerze wirtualnych. Podczas instalacji funkcji zakończy się pomyślnie, kliknij przycisk **Zamknij** , aby zakończyć działanie kreatora **dodawania ról i funkcje** .

    ![Strona potwierdzenia](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-dns-confirmation.png)


## <a name="task-3---launch-the-dns-management-console-to-administer-dns"></a>Zadanie 3 — Uruchom konsolę zarządzania DNS, aby administrowanie DNS
Teraz, gdy funkcja narzędzia serwera DNS jest zainstalowana na domeny sprzężone maszyn wirtualnych, możemy administrowanie DNS w domenie zarządzane za pomocą narzędzia systemu DNS.

> [AZURE.NOTE] Musisz być członkiem grupy "Administratorzy AAD kontrolera domeny", administrowania DNS w domenie zarządzane.

1. Na ekranie startowym kliknij ikonę **Narzędzia administracyjne**. Powinien zostać wyświetlony konsoli **DNS** zainstalowany na komputerze wirtualnych.

    ![Narzędzia administracyjne — konsoli DNS](./media/active-directory-domain-services-admin-guide/install-rsat-dns-tools-installed.png)

2. Kliknij pozycję **DNS** Uruchom konsolę zarządzania systemem DNS.

3. W oknie dialogowym **Łączenie z serwerem DNS** kliknij opcję tytuł **następujący komputer**, a następnie wpisz nazwę domeny DNS zarządzanych domeny (na przykład "contoso100.com").

    ![Konsoli DNS - połączyć się z domeną](./media/active-directory-domain-services-admin-guide/dns-console-connect-to-domain.png)

4. Konsoli DNS łączy zarządzanych domeny.

    ![Konsoli DNS - administrowania domeną](./media/active-directory-domain-services-admin-guide/dns-console-managed-domain.png)

5. Za pomocą konsoli DNS można teraz dodać wpisy DNS dla komputerów w wirtualnej sieci, w której został włączony usług domenowych AAD.

> [AZURE.WARNING] Należy zachować ostrożność przy administrowaniu DNS dla domeny zarządzane przy użyciu narzędzi administracyjnych DNS. Upewnij się, że możesz **nie usuwać ani modyfikować wbudowanych rekordy DNS, które są używane przez usługi domeny w domenie**. Wbudowane rekordy DNS należą rekordy DNS domeny, rekordów serwera nazw i innych rekordów lokalizacji kontrolera domeny. Po zmodyfikowaniu tych rekordów, usług domenowych są zakłócenia w wirtualnej sieci.


Zobacz [artykuł narzędzia DNS w witrynie Technet](https://technet.microsoft.com/library/cc753579.aspx) , aby uzyskać więcej informacji o zarządzaniu DNS.


## <a name="related-content"></a>Zawartość pokrewna

- [Azure usługach domenowych AD - przewodnik wprowadzenie](./active-directory-ds-getting-started.md)

- [Dołączanie do maszyny wirtualnej systemu Windows Server w domenie usług domenowych AD Azure zarządzanych](active-directory-ds-admin-guide-join-windows-vm.md)

- [Administrowanie Azure AD domenie usług domenowych zarządzanych](active-directory-ds-admin-guide-administer-domain.md)

- [Narzędzia administracyjne DNS](https://technet.microsoft.com/library/cc753579.aspx)
