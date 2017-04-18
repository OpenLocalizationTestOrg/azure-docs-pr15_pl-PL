<properties
    pageTitle="Dodawanie nowego konta dzierżawy stos Azure w usłudze Azure Active Directory | Microsoft Azure"
    description="Po wdrożeniu Microsoft Azure stos Zapewnić, musisz utworzyć konto użytkownika co najmniej jeden dzierżawy, aby można było eksplorować portalu dzierżawy."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="erikje"/>

# <a name="add-a-new-azure-stack-tenant-account-in-azure-active-directory"></a>Dodawanie nowego konta dzierżawy stos Azure w usłudze Active Directory platformy Azure

Po [wdrażania Zapewnić stos Azure](azure-stack-run-powershell-script.md)musisz mieć konto użytkownika dzierżawy, można eksplorować portalu dzierżawy i przetestować ofert i planów. Możesz utworzyć konta dzierżawy za [pomocą portalu Azure](#create-an-azure-stack-tenant-account-using-the-azure-portal) lub przy [użyciu programu PowerShell](#create-an-azure-stack-tenant-account-using-powershell).

## <a name="create-an-azure-stack-tenant-account-using-the-azure-portal"></a>Tworzenie konta dzierżawy stos Azure za pomocą portalu Azure

Musisz mieć subskrypcję usługi Azure umożliwia Azure portal.

1. Zaloguj się do [Azure](http://manage.windowsazure.com).

2.  Na pasku nawigacyjnym po lewej stronie Microsoft Azure kliknij pozycję **Usługi Active Directory**.

3.  Na liście katalog kliknij katalog, który ma być używany dla stosu Azure, lub Utwórz nowy.

4.  Na tej stronie Katalog kliknij pozycję **Użytkownicy**.

5.  Kliknij pozycję **Dodaj użytkownika**.

6.  W kreatorze **Dodaj użytkownika** na liście **Typ użytkownika** wybierz **nowego użytkownika w Twojej organizacji**.

7.  W polu **Nazwa użytkownika** wpisz nazwę użytkownika.

8.  W **@** wybierz odpowiednią pozycję.

9.  Kliknij strzałkę w następnym.

10.  Na stronie **profilu użytkownika** w kreatorze wpisz **Imię**, **nazwisko**i **nazwę wyświetlaną**.

11. Na liście **ról** wybierz **użytkownika**.

12. Kliknij strzałkę w następnym.

13. Na stronie **Pobierz hasło tymczasowe** kliknij przycisk **Utwórz**.

14. Skopiuj **nowe hasło**.

15. Zaloguj się do programu Microsoft Azure z nowym kontem. Zmień hasło po wyświetleniu monitu.

16. Zaloguj się do `https://portal.azurestack.local` przy użyciu nowego konta, aby wyświetlić portalu dzierżawy.

## <a name="create-an-azure-stack-tenant-account-using-powershell"></a>Tworzenie konta dzierżawy stos Azure przy użyciu programu PowerShell

Jeśli nie masz subskrypcji usługi Azure, nie można dodać konto użytkownika dzierżawy za pomocą Azure portal. W tym przypadku należy użyć Azure Active Directory modułu dla programu Windows PowerShell.

> [AZURE.NOTE] Jeśli korzystasz z Account Microsoft (identyfikator Live ID) do wdrażania zapewnić stos Azure, nie umożliwia utworzenie konta dzierżawy AAD programu PowerShell. 

1.  Zainstalowanie [Asystenta logowania w usługach Online firmy Microsoft dla informatyków RTW](https://www.microsoft.com/en-us/download/details.aspx?id=41950).

2.  Instalowanie [Azure Active Directory modułu dla programu Windows PowerShell (wersja 64-bitowa)](http://go.microsoft.com/fwlink/p/?linkid=236297) , a następnie otwórz go.

3.  Uruchom następujące polecenia cmdlet:




    ```powershell
    # Provide the AAD credential you use to deploy Azure Stack PoC
   
            $msolcred = get-credential
    
    # Add a tenant account "Tenant Admin <username>@<yourdomainname>" with the initial password "<password>".
    
            connect-msolservice -credential $msolcred
            $user = new-msoluser -DisplayName "Tenant Admin" -UserPrincipalName <username>@<yourdomainname> -Password <password>
            Add-MsolRoleMember -RoleName "Company Administrator" -RoleMemberType User -RoleMemberObjectId $user.ObjectId
    
    ```

4.  Zaloguj się do programu Microsoft Azure z nowym kontem. Zmienianie hasła po wyświetleniu monitu.

5.  Zaloguj się do `https://portal.azurestack.local` przy użyciu nowego konta, aby wyświetlić portalu dzierżawy.



