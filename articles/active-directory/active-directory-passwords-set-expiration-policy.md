<properties
    pageTitle="Ustawianie zasad wygasania hasła w usłudze Active Directory platformy Azure | Microsoft Azure"
    description="Dowiedz się, jak sprawdzić zasad wygasania i zmiana wygasania haseł użytkowników pojedynczo lub zbiorczo Azure Active directory haseł"
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="curtand"/>


# <a name="set-password-expiration-policies-in-azure-active-directory"></a>Ustawianie zasad wygasania hasła w usłudze Active Directory platformy Azure

> [AZURE.IMPORTANT] **Czy jesteś tutaj, ponieważ występują problemy z logowaniem?** Jeśli tak, [Oto jak można zmienić i zresetować swoje hasło](active-directory-passwords-update-your-own-password.md).

Jako administrator globalny usługi w chmurze firmy Microsoft Microsoft Azure Active Directory modułu dla programu Windows PowerShell umożliwia konfigurowanie haseł użytkowników, aby nie wygaśnie. Umożliwia także poleceń cmdlet programu Windows PowerShell do usunięcia nigdy nie wygasa konfiguracji lub aby sprawdzić, który użytkownik hasła są skonfigurowane nie wygaśnie. Ten artykuł zawiera pomoc dla usługami w chmurze, takiej jak Microsoft Intune i usługi Office 365, które korzystania z usługi Microsoft Azure Active Directory dla tożsamości i usług katalogowych.

  > [AZURE.NOTE] Nie, aby wygasało można skonfigurować tylko hasła dla kont użytkowników, które nie są synchronizowane w ramach synchronizacji katalogów. Aby uzyskać więcej informacji dotyczących synchronizacji katalogów Zobacz listę tematów w [Plan synchronizowania katalogu](https://msdn.microsoft.com/library/azure/hh967642.aspx).

Aby użyć poleceń cmdlet programu Windows PowerShell, należy najpierw zainstalować je.

## <a name="what-do-you-want-to-do"></a>Co chcesz zrobić?

- [Jak sprawdzić zasad wygasania hasła](#how-to-check-expiration-policy-for-a-password)

- [Konfigurowanie hasła tak, aby wygasało](#set-a-password-to-expire)

- [Konfigurowanie hasła tak, aby nie wygaśnie](#set-a-password-to-never-expire)

## <a name="how-to-check-expiration-policy-for-a-password"></a>Jak sprawdzić zasad wygasania hasła

1.  Połącz ze środowiska Windows PowerShell przy użyciu poświadczeń administratora firmy.

2.  Wykonaj jedną z następujących czynności:

    - Aby sprawdzić, czy hasło pojedynczego użytkownika jest ustawiony tak, aby nigdy nie wygasało, uruchom następujące polecenie cmdlet przy użyciu głównej nazwy użytkownika (UPN) (na przykład aprilr@contoso.onmicrosoft.com) lub Identyfikatora użytkownika, który chcesz sprawdzić:`Get-MSOLUser -UserPrincipalName <user ID> | Select PasswordNeverExpires`

    - Aby wyświetlić ustawienie "Hasło nigdy nie wygasa" dla wszystkich użytkowników, uruchom następujące polecenie cmdlet:`Get-MSOLUser | Select UserPrincipalName, PasswordNeverExpires`

## <a name="set-a-password-to-expire"></a>Konfigurowanie hasła tak, aby wygasało

1.  Połącz ze środowiska Windows PowerShell przy użyciu poświadczeń administratora firmy.

2.  Wykonaj jedną z następujących czynności:

    - Aby skonfigurować hasło pojedynczego użytkownika tak, aby hasło wygaśnie, uruchom następujące polecenie cmdlet przy użyciu głównej nazwy użytkownika (UPN) lub Identyfikatora użytkownika:`Set-MsolUser -UserPrincipalName <user ID> -PasswordNeverExpires $false`

    - Aby skonfigurować hasła wszystkich użytkowników w organizacji tak, aby były wygaśnie, użyj następującego polecenia cmdlet:`Get-MSOLUser | Set-MsolUser -PasswordNeverExpires $false`

## <a name="set-a-password-to-never-expire"></a>Konfigurowanie hasła tak, aby nigdy nie wygasało

1. Połącz ze środowiska Windows PowerShell przy użyciu poświadczeń administratora firmy.

2.  Wykonaj jedną z następujących czynności:

    - Aby skonfigurować hasło pojedynczego użytkownika tak, aby nigdy nie wygasało, uruchom następujące polecenie cmdlet przy użyciu głównej nazwy użytkownika (UPN) lub Identyfikatora użytkownika:`Set-MsolUser -UserPrincipalName <user ID> -PasswordNeverExpires $true`

    - Aby skonfigurować hasła wszystkich użytkowników w organizacji, aby nigdy nie wygasało, uruchom następujące polecenie cmdlet:`Get-MSOLUser | Set-MsolUser -PasswordNeverExpires $true`

## <a name="next-steps"></a>Następne kroki

* **Czy jesteś tutaj, ponieważ występują problemy z logowaniem?** Jeśli tak, [Oto jak można zmienić i zresetować swoje hasło](active-directory-passwords-update-your-own-password.md).
