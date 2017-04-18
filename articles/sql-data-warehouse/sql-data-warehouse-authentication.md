<properties
   pageTitle="Uwierzytelnianie do magazynu danych Azure SQL | Microsoft Azure"
   description="Azure Active Directory (AAD) i SQL Server uwierzytelnianie do magazynu danych SQL Azure."
   services="sql-data-warehouse"
   documentationCenter=""
   authors="byham"
   manager="barbkess"
   editor=""
   tags=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/24/2016"
   ms.author="rickbyh;barbkess;sonyama"/>

# <a name="authentication-to-azure-sql-data-warehouse"></a>Uwierzytelnianie do magazynu danych Azure SQL

> [AZURE.SELECTOR]
- [Omówienie zabezpieczeń](sql-data-warehouse-overview-manage-security.md)
- [Uwierzytelnianie](sql-data-warehouse-authentication.md)
- [Szyfrowanie (Portal)](sql-data-warehouse-encryption-tde.md)
- [Szyfrowanie (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)

Aby połączyć się magazynu danych SQL, należy przekazać w poświadczeń zabezpieczeń na potrzeby uwierzytelniania. Podczas nawiązywania połączenia, niektóre ustawienia połączenia są skonfigurowane w ramach ustanowić sesję kwerendy.  

Aby uzyskać więcej informacji o zabezpieczeń i włączanie połączenia z magazynu danych zobacz [bezpiecznego bazy danych w magazynie danych SQL][].

## <a name="sql-authentication"></a>Uwierzytelnianie programu SQL
Aby połączyć się magazynu danych SQL, należy podać następujące informacje:

- W pełni kwalifikowana nazwa_serwera
- Określanie uwierzytelniania SQL
- Nazwa użytkownika
- Hasło
- Domyślna baza danych (opcjonalnie)

Domyślnie nawiązaniu połączenia z bazą danych *wzorca* i nie bazy danych użytkowników. Nawiązać połączenie z bazą danych użytkownika, można wykonać jedną z dwóch czynności:

- Podczas rejestrowania serwera w Eksploratorze obiektów serwera SQL w SSDT, SSMS, lub w ciągu połączenia aplikacji, określ domyślną bazę danych. Na przykład dodać parametr InitialCatalog dla połączenia ODBC.
- Wyróżnij bazy danych użytkownika przed utworzeniem sesji w SSDT.

> [AZURE.NOTE] Instrukcji Transact-SQL **Mojabazadanych UŻYJ;** nie jest obsługiwane umożliwiające zmianę połączenia bazy danych. Nawiązywanie połączenia z magazynem danych SQL z SSDT wskazówek można znaleźć w artykule [kwerendy przy użyciu programu Visual Studio][] .

## <a name="azure-active-directory-aad-authentication"></a>Uwierzytelnianie usługi Azure Active Directory (AAD)

[Azure Active Directory] [ What is Azure Active Directory] uwierzytelnianie jest mechanizmu łączenia się magazynu danych Microsoft Azure SQL przy użyciu tożsamości w usłudze Azure Active Directory (Azure AD). Uwierzytelnianie usługi Azure Active Directory możesz centralnie zarządzać tożsamości bazy danych użytkowników i innych usług firmy Microsoft w jednej centralnej lokalizacji. Centralne zarządzanie identyfikator zawiera jednego miejsca do zarządzania użytkownikami magazynu danych SQL i upraszcza zarządzanie uprawnieniami. 

### <a name="benefits"></a>Zalety

Azure Active Directory zalety:

- Stanowi alternatywę dla uwierzytelniania programu SQL Server.
- Ułatwia zatrzymanie mnożenia tożsamości użytkownika na serwerach bazy danych.
- Umożliwia obrotu hasła w jednym miejscu
- Zarządzanie uprawnieniami bazy danych przy użyciu grup zewnętrznych (AAD).
- Eliminuje przechowywania haseł, włączając zintegrowane uwierzytelnianie systemu Windows i innych rodzajów uwierzytelniania obsługiwane przez usługi Azure Active Directory.
- Użytkownicy bazy danych przy użyciu zawarte do uwierzytelnienia tożsamości na poziomie bazy danych.
- Obsługuje tokenów uwierzytelniania dla aplikacji nawiązywanie połączenia z magazynem danych SQL.
- Obsługuje uwierzytelnianie wieloskładnikowe za pośrednictwem uniwersalny uwierzytelnianie usługi Active Directory dla programu SQL Server Management Studio. Aby uzyskać opis uwierzytelnianie wieloskładnikowe zobacz [SSMS obsługę Azure AD MFA z bazy danych SQL i magazynu danych SQL](../sql-database/sql-database-ssms-mfa-authentication.md).

> [AZURE.NOTE] Azure Active Directory jest nadal stosunkowo nowy i ma pewne ograniczenia. W celu zapewnienia właściwie dobierane w środowisku usługi Azure Active Directory, zobacz [Azure AD funkcje i ograniczenia][], w szczególności uwagi dodatkowe.

### <a name="configuration-steps"></a>Kroki konfiguracji

Wykonaj poniższe czynności, aby skonfigurować uwierzytelnianie usługi Azure Active Directory.

1. Tworzenie i wypełnij usługi Azure Active Directory
2. Opcjonalnie: Kojarzenie lub zmienianie usługi active directory, która jest aktualnie skojarzona z subskrypcją usługi Azure
3. Tworzenie administratora usługi Azure Active Directory dla magazynu danych SQL Azure.
4. Konfigurowanie komputerów klienckich
5. Tworzenie użytkowników bazy danych zawartych w bazie danych zamapowane tożsamości Azure AD
6. Połącz się z magazynu danych przy użyciu tożsamości Azure AD

Obecnie użytkownicy usługi Azure Active Directory nie są wyświetlane w Eksploratorze obiektów SSDT. Aby obejść ten problem należy wyświetlić użytkowników w [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx).
  
### <a name="find-the-details"></a>Znajdowanie szczegółów
- Wykonywanie uzyskać szczegółowe instrukcje. Szczegółowo opisano konfigurowanie i używanie uwierzytelniania usługi Azure Active Directory są niemal identyczne dla bazy danych SQL Azure i magazynu danych SQL Azure. Postępuj zgodnie z instrukcjami szczegółowe w temacie [Łączenie się z bazą danych SQL lub SQL danych magazynu przy użyciu Azure uwierzytelnianie usługi Active Directory](../sql-database/sql-database-aad-authentication.md).
- Tworzenie niestandardowego bazy danych i dodawanie użytkowników do ról. Następnie udzielanie uprawnień szczegółowego do ról. Aby uzyskać więcej informacji zobacz [Wprowadzenie do uprawnień aparat bazy danych](https://msdn.microsoft.com/library/mt667986.aspx).

## <a name="next-steps"></a>Następne kroki

Aby rozpocząć, kwerenda magazynu danych przy użyciu programu Visual Studio i inne aplikacje, zobacz [kwerendy przy użyciu programu Visual Studio][].

<!-- Article references -->
[Zabezpieczanie bazy danych w magazynie danych SQL]: ./sql-data-warehouse-overview-manage-security.md
[Kwerenda z programu Visual Studio]: ./sql-data-warehouse-query-visual-studio.md
[What is Azure Active Directory]: ../active-directory/active-directory-whatis.md
[Azure AD funkcje i ograniczenia]: ../sql-database/sql-database-aad-authentication.md#azure-ad-features-and-limitations
