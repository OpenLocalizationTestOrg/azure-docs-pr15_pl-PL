<properties
    pageTitle="Azure Active Directory Domain Services: Przewodnik po administracji | Microsoft Azure"
    description="Tworzenie organizacji jednostek (OU) w usługach domenowych AD Azure zarządzania domenami"
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
    ms.date="09/21/2016"
    ms.author="maheshu"/>

# <a name="create-an-organizational-unit-ou-on-an-azure-ad-domain-services-managed-domain"></a>Tworzenie organizacji jednostek (OU) w domenie usług domenowych AD Azure zarządzanych
Azure domen zarządzanych w usługach domenowych AD obejmuje dwa kontenery Wbudowany odpowiednio o nazwie "Komputery AADDC" i "AADDC użytkowników". Kontener "Komputery AADDC" ma komputera obiektów na wszystkich komputerach dołączonych do domeny zarządzane. Kontener "Użytkownicy AADDC", który zawiera użytkowników i grup w dzierżawie Azure AD. Czasami może być konieczne do utworzenia konta usługi w domenie zarządzane wdrożenia obciążenia. W tym celu możesz utworzyć niestandardowe jednostkę organizacyjną (OU) w tej domenie zarządzanych i tworzenie kont usługi w tej jednostce Organizacyjnej. W tym artykule pokazano, jak utworzyć jednostkę Organizacyjną w domenie zarządzanych.


## <a name="install-ad-administration-tools-on-a-domain-joined-virtual-machine-for-remote-administration"></a>Narzędzia administracyjne AD zostaną zainstalowane na komputerze wirtualnych domeny dla administracji zdalnej
Azure domen zarządzanych w usługach domenowych AD można zarządzać zdalnie przy użyciu znanych narzędzi administracyjnych usługi Active Directory takich jak Centrum administracyjne Active Directory (ADAC) lub AD programu PowerShell. Administratorzy dzierżawy nie ma uprawnień do połączenia się kontrolerów domen na zarządzanych domeny za pomocą pulpitu zdalnego. Aby administrować zarządzanych domeny, zostaną zainstalowane na komputerze wirtualnych dołączony do domeny zarządzanych funkcji narzędzia AD Administracja. Zapoznaj się z artykułem zatytułowany [Administrowanie domenie usług domenowych AD Azure zarządzane](active-directory-ds-admin-guide-administer-domain.md) , aby uzyskać instrukcje.

## <a name="create-an-organizational-unit-on-the-managed-domain"></a>Utwórz jednostkę organizacyjną w domenie zarządzanych
Teraz, gdy AD narzędzia administracyjne są zainstalowane na domeny sprzężone maszyn wirtualnych, firma Microsoft korzysta z tych narzędzi do tworzenia jednostka organizacyjna w domenie zarządzane. Wykonaj następujące czynności:

> [AZURE.NOTE] Tylko członkowie grupy "Administratorzy AAD kontrolera domeny" masz uprawnień wymaganych do tworzenia niestandardowej jednostce Organizacyjnej. Upewnij się, wykonaj następujące czynności jako użytkownik, który należy do tej grupy.

1. Na ekranie startowym kliknij ikonę **Narzędzia administracyjne**. Narzędzia administracyjne AD zainstalowany na komputerze wirtualnych powinny być widoczne.

    ![Narzędzia administracyjne zainstalowane na serwerze](./media/active-directory-domain-services-admin-guide/install-rsat-admin-tools-installed.png)

2. Kliknij pozycję **Centrum administracyjne usługi Active Directory**.

    ![Centrum administracyjne usługi Active Directory](./media/active-directory-domain-services-admin-guide/adac-overview.png)

3. Aby wyświetlić domeny, kliknij nazwę domeny w okienku po lewej stronie (na przykład "contoso100.com").

    ![ADAC — widok domeny](./media/active-directory-domain-services-admin-guide/create-ou-adac-overview.png)

4. W okienku **zadań** po prawej stronie kliknij przycisk **Nowy** w węźle nazwę domeny. W tym przykładzie możemy kliknij przycisk **Nowy** w węźle "contoso100(local)" w okienku **zadań** po prawej stronie.

    ![ADAC — nowej jednostki Organizacyjnej](./media/active-directory-domain-services-admin-guide/create-ou-adac-new-ou.png)

5. Powinna być widoczna opcja tworzenia jednostka organizacyjna. Kliknij przycisk Uruchom okno dialogowe **Tworzenie jednostka organizacyjna** **Jednostka organizacyjna** .

6. W oknie dialogowym **Tworzenie jednostka organizacyjna** określ **nazwę** nowej jednostki organizacyjnej. Podaj krótki opis jednostki organizacyjnej. Można również ustawić pole **Zarządzany przez** jednostki organizacyjnej. Aby utworzyć niestandardowe jednostkę Organizacyjną, kliknij **przycisk OK**.

    ![ADAC - OU okno dialogowe Tworzenie](./media/active-directory-domain-services-admin-guide/create-ou-dialog.png)

7. Nowo utworzony OU powinien wyglądać w AD Centrum administracyjnego (ADAC).

    ![ADAC - OU utworzony](./media/active-directory-domain-services-admin-guide/create-ou-done.png)


## <a name="permissionssecurity-for-newly-created-ous"></a>Uprawnienia i zabezpieczeń dla nowo utworzonego organizacyjnych
Domyślnie użytkownika (członek grupy "Administratorzy AAD kontrolera domeny"), który utworzył niestandardowej jednostce Organizacyjnej udzielono uprawnień administracyjnych (Pełna kontrola) na jednostkę Organizacyjną. Użytkownik może następnie wtyczce i udzielić uprawnień do innych użytkowników lub grupy "Administratorzy AAD kontrolera domeny" w razie potrzeby. W następujących zrzut ekranu użytkownika 'bob@domainservicespreview.onmicrosoft.com' twórca nowej jednostki organizacyjnej "MyCustomOU" jest udzielona pełna kontrola nad nim.

 ![ADAC — nowe zabezpieczenie OU](./media/active-directory-domain-services-admin-guide/create-ou-permissions.png)


## <a name="notes-on-administering-custom-ous"></a>Uwagi o administrowaniu organizacyjnych niestandardowe
Teraz, gdy użytkownik utworzył niestandardowej jednostce Organizacyjnej, możesz teraz i utworzyć użytkownicy, grupy, komputery i konta usług w tej jednostce Organizacyjnej. Nie można przenosić użytkowników lub grup do "AAD kontrolera domeny użytkowników" OU niestandardowe organizacyjnych.

> [AZURE.WARNING] Konta użytkowników, grupy, konta usługi i obiektów komputera, utworzone w obszarze organizacyjnych niestandardowe nie są dostępne w dzierżawie usługi Azure AD. Innymi słowy obiekty te nie są wyświetlane przy użyciu interfejsu API Azure AD wykresu lub w Interfejsie użytkownika Azure AD. Obiekty te są dostępne tylko w usługach domenowych AD Azure domeny zarządzane.


## <a name="related-content"></a>Zawartość pokrewna

- [Administrowanie Azure AD domenie usług domenowych zarządzanych](active-directory-ds-admin-guide-administer-domain.md)

- [Centrum administracyjnego usługi Active Directory: Wprowadzenie](https://technet.microsoft.com/library/dd560651.aspx)

- [Przewodnik krok po kroku konta usług](https://technet.microsoft.com/library/dd548356.aspx)
