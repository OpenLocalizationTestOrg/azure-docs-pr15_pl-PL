<properties
    pageTitle="Jak wykonać zadania administracyjne, np. zresetować hasło administratora | Microsoft Azure"
    description="W tym artykule opisano sposób wykonywania typowych zadań administracyjnych w bazie danych SQL. Na przykład Resetowanie hasła administratora, udzielanie i usuwanie dostępu."
    services="sql-database"
    documentationCenter=""
    authors="v-shysun"
    manager="felixwu"
    editor=""
    keywords="Resetowanie hasła administratora"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="v-shysun"/>

# <a name="how-to-perform-common-administrative-tasks-such-as-resetting-admin-password-in-azure-sql-database"></a>Sposób wykonywania typowych zadań administracyjnych, takich jak zresetować hasło administratora w bazie danych SQL Azure
W tym temacie dla szybkich kroków umożliwia udzielanie i usunąć dostęp do bazy danych programu Azure SQL. Aby uzyskać bardziej szczegółowe informacje Zobacz:

- [Zarządzanie bazami danych i logowania w bazie danych SQL Azure](sql-database-manage-logins.md)
- [Zabezpieczanie bazy danych SQL](sql-database-security.md)
- [Centrum zabezpieczeń dla aparatu bazy danych programu SQL Server i baza danych SQL Azure](https://msdn.microsoft.com/library/bb510589)


[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="to-reset-admin-password-for-a-logical-server"></a>Aby zresetować hasło administratora dla serwera logicznych.

- W [Azure Portal](https://portal.azure.com) kliknij **Serwerów SQL**, wybierz serwer z listy, a następnie kliknij przycisk **Resetuj hasło**.

## <a name="to-help-make-sure-only-authorized-ip-addresses-are-allowed-to-access-the-server"></a>Ułatwiające, upewnij się, że tylko IP autoryzowanych adresy będą mogli uzyskać dostęp do serwera
- Zobacz [jak: Konfigurowanie ustawień zapory w bazie danych SQL](sql-database-configure-firewall-settings.md).

## <a name="to-create-contained-database-users-in-the-user-database"></a>Aby utworzyć użytkowników bazy danych zawartych w bazie danych użytkownika
- Instrukcja [CREATE USER](https://msdn.microsoft.com/library/ms173463.aspx) i zobacz [Zawarte użytkowników bazy danych — nawiązywanie i bazy danych przenośny](https://msdn.microsoft.com/library/ff929188.aspx).

## <a name="to-authenticate-contained-database-users-by-using-your-azure-active-directory"></a>Uwierzytelnianie użytkowników zawartych bazy danych przy użyciu usługi Azure Active Directory
- Zobacz [łączenia z bazą danych SQL za pomocą uwierzytelniania usługi Azure Active Directory](sql-database-aad-authentication.md).

## <a name="to-create-additional-logins-for-high-privileged-users-in-the-virtual-master-database"></a>Aby utworzyć dodatkowe logowania wysokich uprawnieniach użytkowników w wirtualnej wzorca bazy danych
- Instrukcja [Tworzenie logowania](https://msdn.microsoft.com/library/ms189751.aspx) , a następnie w sekcji Zarządzanie logowania [Zarządzanie bazami danych](sql-database-manage-logins.md) i logowania w bazie danych SQL Azure więcej szczegółów.
