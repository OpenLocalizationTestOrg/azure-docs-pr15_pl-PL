<properties
    pageTitle="Azure Active Directory Domain Services: Administrowanie zarządzanych domeny | Microsoft Azure"
    description="Administrowania domenami zarządzanych Azure Active Directory Domain Services"
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
    ms.date="10/02/2016"
    ms.author="maheshu"/>

# <a name="administer-an-azure-active-directory-domain-services-managed-domain"></a>Administrowanie domeną zarządzane usługi Azure Active Directory Domain Services
W tym artykule pokazano, jak administrowanie Azure Active Directory (AD) domenie usług domenowych zarządzane.


## <a name="before-you-begin"></a>Przed rozpoczęciem
Aby wykonać zadania, wymienione w tym artykule, należy następująco:

1. Ważna **Subskrypcja Azure**.

2. Usługi **katalogowej Azure AD** -, albo zsynchronizowane z katalogu lokalnego lub w katalogu tylko do chmury.

3. **Usługi Azure AD domeny** musi być włączona dla katalogu Azure AD. Jeśli nie zostało to zrobione, wykonaj wszystkie zadania opisane w [Przewodnik wprowadzający dla](./active-directory-ds-getting-started.md).

4. **Domeny maszyn wirtualnych** z której administrować Azure AD domenie usług domenowych zarządzane. Jeśli nie masz maszyny wirtualnej, wykonaj wszystkie zadania opisane w artykuł [Dołączanie maszyny wirtualnej systemu Windows z domeną zarządzane](./active-directory-ds-admin-guide-join-windows-vm.md).

5. Potrzebujesz poświadczeń **konta użytkownika należącego do grupy "Administratorzy AAD kontrolera domeny"** w katalogu administrowania zarządzanych domeny.

<br>


## <a name="administrative-tasks-you-can-perform-on-a-managed-domain"></a>Zadania administracyjne, które można wykonywać w domenie zarządzanych
Członkowie grupy "Administratorzy kontrolera domeny AAD" udzielono uprawnień w domenie zarządzane, które umożliwia im wykonywanie zadań, takich jak:

- Dołączanie komputerów zarządzanych domenę.

- Konfigurowanie wbudowanego obiektu zasad grupy dla kontenerów "Komputery AADDC" i "AADDC użytkowników" w domenie zarządzane.

- Administrowanie DNS w domenie zarządzane.

- Tworzenie i administrowanie niestandardowej jednostki organizacyjne (OU) w domenie zarządzane.

- Wzmocnienia dostęp administracyjny do komputerów dołączony do zarządzanych domeny.


## <a name="administrative-privileges-you-do-not-have-on-a-managed-domain"></a>Uprawnienia administracyjne nie masz w domenie zarządzanych
Domena jest zarządzany przez firmę Microsoft, w tym działań, takich jak poprawki, monitorowania i wykonywania kopii zapasowych. Dlatego domeny jest zablokowane i nie masz uprawnień do wykonywania określonych zadań administracyjnych w domenie. Kilka przykładów zadań, których nie można wykonać są poniżej.

- Nie udzielono uprawnień administratora domeny lub Administrator przedsiębiorstwa dla zarządzanych domeny.

- Nie można rozszerzyć schemat zarządzanych domeny.

- Nie możesz połączyć się kontrolery domeny zarządzane za pomocą pulpitu zdalnego.

- Nie można dodać kontrolery zarządzanych domeny.


## <a name="task-1---provision-a-domain-joined-windows-server-virtual-machine-to-remotely-administer-the-managed-domain"></a>Zadanie 1 - zapewnienie maszyny wirtualnej domeny Windows Server zdalnie administrować zarządzanych domeny
Azure domen zarządzanych w usługach domenowych AD można zarządzać za pomocą znanych narzędzi administracyjnych usługi Active Directory takich jak Centrum administracyjne Active Directory (ADAC) lub AD programu PowerShell. Administratorzy dzierżawy nie ma uprawnień do połączenia się kontrolerów domen na zarządzanych domeny za pomocą pulpitu zdalnego. W związku z tym członkowie grupy "Administratorzy AAD kontrolera domeny" mogą administrować domen zarządzane zdalnie przy użyciu narzędzia administracyjne AD z komputera klienta serwera systemu Windows, który jest połączony z domeną zarządzanych. Narzędzia administracyjne AD można zainstalować jako część opcjonalną funkcją Narzędzia administracji zdalnej serwera (RSAT) na serwerze systemu Windows i komputerów klienckich dołączony do zarządzanych domeny.

Pierwszym krokiem jest konfigurowanie maszyn wirtualnych systemu Windows Server o jest połączony z domeną zarządzane. Aby uzyskać instrukcje zapoznaj się z artykułem tytuł, [Dołącz do systemu Windows Server maszyny wirtualnej w domenie usług domenowych AD Azure zarządzane](active-directory-ds-admin-guide-join-windows-vm.md).

### <a name="remotely-administer-the-managed-domain-from-a-client-computer-for-example-windows-10"></a>Zdalne administrowanie zarządzanych domeny z komputera klienckiego (na przykład Windows 10)
Instrukcje w tym artykule maszyny wirtualnej Windows Server do zarządzania Zasadami AAD zarządza domeny. Jednak możesz również wybrać Aby to zrobić za pomocą maszyny wirtualnej klienta (na przykład Windows 10) systemu Windows.

Możesz [zainstalować narzędzia administracji zdalnej serwera (RSAT)](http://social.technet.microsoft.com/wiki/contents/articles/2202.remote-server-administration-tools-rsat-for-windows-client-and-windows-server-dsforum2wiki.aspx) na komputerach wirtualnych systemu Windows, postępując zgodnie z instrukcjami w witrynie TechNet.


## <a name="task-2---install-active-directory-administration-tools-on-the-virtual-machine"></a>Zadanie 2 — narzędzia administracyjne usługi Active Directory instalacji na komputerze wirtualnych
Wykonaj poniższe czynności, aby zainstalować narzędzia administracyjne usługi Active Directory na tym komputerze wirtualnych domeny sprzężone. Aby uzyskać więcej [informacji na temat instalowania i korzystania z narzędzia administracji zdalnej serwera](https://technet.microsoft.com/library/hh831501.aspx), zobacz witrynę Technet.

1. Przejdź do węzła **maszyn wirtualnych** w portalu klasyczny Azure. Zaznacz maszyny wirtualnej utworzone w zadaniu 1, a następnie kliknij przycisk **Połącz** na pasku poleceń w dolnej części okna.

    ![Nawiązywanie połączenia z maszyn wirtualnych systemu Windows](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)

2. Portalu klasycznym wyświetli monit o otwarcie lub zapisanie pliku z rozszerzeniem ".rdp", który jest używany do łączenia maszyn wirtualnych. Kliknij, aby otworzyć plik po zakończeniu pobierania.

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

9. Na stronie **Funkcje** kliknij, aby rozwinąć węzeł **Narzędzia administracji zdalnej serwera** , a następnie kliknij pozycję do rozwiń węzeł **Narzędzia administracyjne roli** . Funkcja **usług AD DS i narzędzia do tych usług** na liście Narzędzia administracyjne roli.

    ![Funkcje strony](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-ad-tools.png)

10. Na stronie **potwierdzenia** kliknij przycisk **Zainstaluj** instalowanie AD i funkcji narzędzia tych usług na komputerze wirtualnych. Podczas instalacji funkcji zakończy się pomyślnie, kliknij przycisk **Zamknij** , aby zakończyć działanie kreatora **dodawania ról i funkcje** .

    ![Strona potwierdzenia](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-confirmation.png)


## <a name="task-3---connect-to-and-explore-the-managed-domain"></a>Zadanie 3 — nawiązywanie połączenia i eksplorowanie zarządzanych domeny
Teraz, gdy AD narzędzia administracyjne są zainstalowane na domeny sprzężone maszyn wirtualnych, firma Microsoft korzysta z tych narzędzi eksplorować i administrowanie zarządzanych domeny.

> [AZURE.NOTE] Musisz być członkiem grupy "Administratorzy AAD kontrolera domeny", administrowania zarządzanych domeny.

1. Na ekranie startowym kliknij ikonę **Narzędzia administracyjne**. Narzędzia administracyjne AD zainstalowany na komputerze wirtualnych powinny być widoczne.

    ![Narzędzia administracyjne zainstalowane na serwerze](./media/active-directory-domain-services-admin-guide/install-rsat-admin-tools-installed.png)

2. Kliknij pozycję **Centrum administracyjne usługi Active Directory**.

    ![Centrum administracyjne usługi Active Directory](./media/active-directory-domain-services-admin-guide/adac-overview.png)

3. Aby poznać domeny, kliknij nazwę domeny w okienku po lewej stronie (na przykład "contoso100.com"). Zwróć uwagę, dwoma kontenerami odpowiednio o nazwie "Komputery AADDC" i "AADDC użytkowników".

    ![ADAC — widok domeny](./media/active-directory-domain-services-admin-guide/adac-domain-view.png)

4. Kliknij kontener o nazwie **AADDC użytkowników** , aby wyświetlić wszystkich użytkowników i grupy należące do zarządzanych domeny. Powinien zostać wyświetlony konta użytkowników i grup z usługi Azure AD dzierżawa Pokaż w górę w tym kontenerze. Powiadomienie o w tym przykładzie, konto użytkownika dla użytkownika o nazwie "Robert" i grupę o nazwie "Administratorzy AAD kontrolera domeny" są dostępne w tym kontenerze.

    ![ADAC - użytkownicy domeny](./media/active-directory-domain-services-admin-guide/adac-aaddc-users.png)

5. Kliknij kontener o nazwie **Komputerach AADDC** komputerach dołączonych do tej domeny zarządzane. Powinien zostać wyświetlony wpis dla bieżącej maszyny wirtualnej jest dołączony do domeny. Konta komputerów na wszystkich komputerach dołączonych do Azure AD domenie usług domenowych zarządzane są przechowywane w tym kontenerze "Komputery AADDC".

    ![ADAC - domeny połączonych komputerach](./media/active-directory-domain-services-admin-guide/adac-aaddc-computers.png)

<br>

## <a name="related-content"></a>Zawartość pokrewna

- [Azure usługach domenowych AD - przewodnik wprowadzenie](./active-directory-ds-getting-started.md)

- [Dołączanie do maszyny wirtualnej systemu Windows Server w domenie usług domenowych AD Azure zarządzanych](active-directory-ds-admin-guide-join-windows-vm.md)

- [Wdrażanie narzędzia administracji zdalnej serwera](https://technet.microsoft.com/library/hh831501.aspx)
