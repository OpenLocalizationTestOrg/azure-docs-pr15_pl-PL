<properties
    pageTitle="Azure usługach domenowych AD: Tworzenie grupy Administratorzy kontrolera domeny AAD | Microsoft Azure"
    description="Wprowadzenie do usług domenowych Active Directory platformy Azure"
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

# <a name="get-started-with-azure-ad-domain-services"></a>Wprowadzenie do usług domenowych AD Azure

W tym artykule opisano zadania konfiguracji, trzeba włączyć usługi Azure AD domen dla dzierżawy usługi Azure AD.

## <a name="task-1-create-the-aad-dc-administrators-group"></a>Zadanie 1: Tworzenie grupy "Administratorzy AAD kontrolera domeny"
Pierwsze zadanie jest utworzenie grupy administracyjnej w dzierżawie usługi Azure Active Directory. Ten specjalne grupy administracyjnej nosi nazwę **AAD Administratorzy kontrolera domeny**. Członkowie tej grupy udzielono uprawnień administracyjnych na komputerach, które są domeny dołączony do Azure AD domenie usług domenowych zarządzane. W przypadku komputerów domeny ta grupa jest dodawany do grupy "Administratorzy". Ponadto Członkowie tej grupy mogą używać pulpitu zdalnego nawiązywanie połączeń zdalnych na komputerach domeny.  

> [AZURE.NOTE] Nie masz uprawnień administratora domeny lub Administrator przedsiębiorstwa w domenie zarządzanych utworzony przy użyciu usług domenowych AD Azure. W domenach zarządzane te uprawnienia są zarezerwowane przez usługę i nie będą dostępne dla użytkowników w dzierżawie. Jednak może zawierać grupy administratorów specjalne utworzone w tym zadaniu konfiguracji do wykonywania niektórych uprzywilejowanych operacji. Operacje te obejmują dołączanie komputerów do domeny, należący do grupy Administratorzy na komputerach domeny, konfigurowanie itp zasad grupy.

W tym zadaniu konfiguracji Tworzenie grupy administracyjnej i dodać jednego lub kilku użytkowników w katalogu do grupy. Wykonaj poniższe czynności, aby utworzyć grupy administracyjnej usługi Azure AD domeny:

1. Przejdź do **portalu klasyczny Azure** ([https://manage.windowsazure.com](https://manage.windowsazure.com))

2. Wybierz węzeł **Usługi Active Directory** w okienku po lewej stronie.

3. Wybierz pozycję dzierżawy Azure AD (katalog), dla którego chcesz włączyć usługi Azure AD domeny. Można utworzyć tylko jedną domenę dla każdego katalogu Azure AD.

    ![Wybierz katalog Azure AD](./media/active-directory-domain-services-getting-started/select-aad-directory.png)

4. Kliknij kartę **grupy** .

5. Aby dodać grupę do dzierżawy usługi Azure AD, kliknij pozycję **Dodaj grupę** z okienka zadań u dołu strony.

    ![Dodawanie przycisku grupy](./media/active-directory-domain-services-getting-started/add-group-button.png)

6. Utwórz grupę o nazwie **Administratorzy kontrolera domeny AAD**. Ustaw **Typ grupy** **zabezpieczeń**.

    > [AZURE.WARNING] Aby włączyć dostęp w ramach usługi Azure AD domeny zarządzane domeny, Utwórz grupę o tej nazwie dokładnie.

    ![Tworzenie grupy administratorów](./media/active-directory-domain-services-getting-started/create-admin-group.png)

7. Dodaj opis dla tej grupy, aby inne osoby zrozumieć, że ta grupa jest używana do udzielania uprawnień administracyjnych w usługach domenowych AD Azure.

8. Po utworzeniu grupy, kliknij nazwę grupy, aby wyświetlić właściwości tej grupy. Aby dodać użytkowników jako członkowie tej grupy, kliknij przycisk **Dodaj członków** panelu dołu.

    ![Dodawanie przycisku członków grupy](./media/active-directory-domain-services-getting-started/add-group-members-button.png)

9. W oknie dialogowym **Dodaj członków** Wybierz użytkowników, którzy należy Członkowie tej grupy i zaznacz pole wyboru, jeśli chcesz zatrzymać.

    ![Dodawanie użytkowników do grupy administratorów](./media/active-directory-domain-services-getting-started/add-group-members.png)

<br>

## <a name="task-2-create-or-select-an-azure-virtual-network"></a>Zadanie 2: Tworzenie lub wybieranie Azure wirtualnej sieci
Następnego zadania konfiguracji jest [Tworzenie lub wybieranie Azure wirtualnej sieci](active-directory-ds-getting-started-vnet.md).
