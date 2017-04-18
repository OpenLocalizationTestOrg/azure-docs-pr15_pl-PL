<properties
   pageTitle="SSMS obsługę Azure AD MFA z bazy danych SQL i magazynu danych SQL | Microsoft Azure"
   description="Za pomocą uwierzytelniania uwzględnić wielu SSMS dla bazy danych SQL i magazynu danych SQL."
   services="sql-database"
   documentationCenter=""
   authors="BYHAM"
   manager="jhubbard"
   editor=""
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="10/04/2016"
   ms.author="rick.byham@microsoft.com"/>

# <a name="ssms-support-for-azure-ad-mfa-with-sql-database-and-sql-data-warehouse"></a>Obsługa SSMS Azure AD MFA z bazy danych SQL i magazynu danych SQL

Azure SQL Database i magazynu danych SQL Azure teraz obsługiwać połączenia z programu SQL Server Management Studio (SSMS) przy użyciu *Uniwersalny uwierzytelnianie usługi Active Directory*. Uniwersalny uwierzytelnianie usługi Active Directory jest przepływu pracy interakcyjnych, która obsługuje *Uwierzytelnianie wieloskładnikowe Azure* (MFA). Azure MFA ułatwia ochronę informacji dostęp do danych i aplikacji podczas spotkania żądanie użytkownika prosty proces logowania. Zapewnia silne uwierzytelnianie z zakresu opcje weryfikacji łatwe — rozmowy telefonicznej, wiadomości SMS, kart inteligentnych za pomocą numeru pin lub powiadomienie aplikacji dla urządzeń przenośnych — dzięki czemu użytkownicy mogą wybrać metodę preferują. Aby uzyskać opis uwierzytelnianie wieloskładnikowe zobacz [Uwierzytelnianie wieloskładnikowe](../multi-factor-authentication/multi-factor-authentication.md).

SSMS obsługuje teraz:

- Interakcyjne MFA z Azure AD przy użyciu możliwości sprawdzania poprawności okno podręczne okno dialogowe.
- -Interactive Active Directory hasła i zintegrowane uwierzytelnianie usługi Active Directory metody, które mogą być używane w wielu różnych aplikacjach (ADO.NET JDBC, ODBC, itp.). Te dwie metody nigdy nie powodują podręcznych oknach dialogowych.

Po skonfigurowaniu konta użytkownika do uwierzytelniania MFA przepływu pracy interakcyjnego uwierzytelniania dodatkowe, użytkownik musi za pośrednictwem podręcznych oknach dialogowych, użyj karty inteligentnej, itp. Po skonfigurowaniu konta użytkownika do uwierzytelniania MFA, użytkownik musi wybrać uniwersalny uwierzytelniania Azure nawiązania połączenia. Jeśli konto użytkownika nie wymaga MFA, użytkownik nadal możesz używać pozostałych dwóch opcji uwierzytelnianie usługi Azure Active Directory.

## <a name="universal-authentication-limitations-for-sql-database-and-sql-data-warehouse"></a>Uniwersalny ograniczenia uwierzytelniania dla bazy danych SQL i magazynu danych SQL

- SSMS to narzędzie tylko aktualnie włączone dla MFA za pośrednictwem uniwersalny uwierzytelnianie usługi Active Directory.
- Tylko dla jednego konta usługi Azure Active Directory można zalogować się w przypadku wystąpienia SSMS przy użyciu funkcji uwierzytelniania uniwersalny. Aby zalogować się jako inne konto Azure AD, należy użyć innego wystąpienia programu SSMS. (To ograniczenie jest ograniczone do uniwersalny uwierzytelnianie usługi Active Directory, możesz się zalogować do różnych serwerów przy użyciu uwierzytelnianie hasła usługi Active Directory, zintegrowane uwierzytelnianie usługi Active Directory lub uwierzytelnianie programu SQL Server).
- SSMS obsługuje uwierzytelnianie uniwersalny usługi Active Directory dla wizualizacji Eksplorator obiektów, edytora zapytań i magazynu kwerendy.
- DacFx ani projektanta schematu obsługuje uniwersalny uwierzytelniania.
- MSA konta nie są obsługiwane dla uniwersalny uwierzytelnianie usługi Active Directory.
- Uniwersalny uwierzytelnianie usługi Active Directory nie jest obsługiwana w SSMS dla użytkowników, którzy są importowane do bieżącej usługi Active Directory z innych katalogów Active Azure. Ci użytkownicy nie są obsługiwane, ponieważ będzie to wymagać Identyfikatora dzierżawy sprawdzić poprawność konta, a nie istnieje mechanizm zapewnienie, że.
- Istnieją nie dodatkowe wymagania dotyczące oprogramowania związane uniwersalny uwierzytelnianie usługi Active Directory, z wyjątkiem, że należy użyć obsługiwanej wersji programu SSMS.

## <a name="configuration-steps"></a>Kroki konfiguracji

Uwierzytelnianie wieloskładnikowe implementacji wymaga cztery podstawowe kroki.

1. **Konfigurowanie usługi Azure Active Directory** — Aby uzyskać więcej informacji, zobacz [Integrowanie tożsamości lokalnych z usługą Azure Active Directory](../active-directory/active-directory-aadconnect.md), [Dodaj własną nazwę domeny do Azure AD](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), [platformy Microsoft Azure teraz obsługuje Federację z usługą Active Directory systemu Windows Server](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), [Administrowanie katalogu Azure AD](https://msdn.microsoft.com/library/azure/hh967611.aspx)i [Zarządzanie Azure AD przy użyciu programu Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx).

2. **Konfigurowanie MFA** — Aby uzyskać instrukcje krok po kroku, zobacz [Konfigurowanie uwierzytelniania wieloskładnikowego Azure](../multi-factor-authentication/multi-factor-authentication-whats-next.md). 

3. **Konfigurowanie bazy danych SQL lub magazynu danych SQL Azure AD uwierzytelniania** — Aby uzyskać instrukcje krok po kroku, zobacz [Nawiązywanie połączenia z bazą danych SQL lub SQL danych magazynu przy użyciu Azure uwierzytelnianie usługi Active Directory](sql-database-aad-authentication.md).

4. **Pobierz SSMS** — na komputerze klienckim, Pobierz najnowszą SSMS (co najmniej 2016 sierpień), z [Pobierz program SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx).

## <a name="connecting-by-using-universal-authentication-with-ssms"></a>Nawiązywanie połączenia przy użyciu uwierzytelniania uniwersalny SSMS

Poniższe kroki pokazują sposób nawiązywania połączenia z bazą danych SQL lub magazynu danych SQL przy użyciu najnowszych SSMS.

1. Nawiązywanie połączenia przy użyciu uniwersalny uwierzytelniania, w oknie dialogowym **Łączenie z serwerem** , wybierz pozycję **Uniwersalny uwierzytelnianie usługi Active Directory**.
![Łączenie uniwersalny 1mfa][1]

2. Jak zwykle dla bazy danych SQL i magazynu danych SQL należy kliknij pozycję **Opcje** i podać nazwę bazy danych w oknie dialogowym **Opcje** . Następnie kliknij przycisk **Połącz**.
3. Gdy pojawi się okno dialogowe **Zaloguj się do swojego konta** , podaj konto i hasło Twojej tożsamości usługi Azure Active Directory.
![2mfa-logowania][2]

    > [AZURE.NOTE] Uniwersalny uwierzytelniania za pomocą konta, które nie wymagają MFA umożliwia nawiązanie połączenia w tym momencie. Dla użytkowników wymagających MFA wykonaj następujące czynności.
 
4. Dwa MFA ustawienia okna dialogowe mogą być wyświetlane. Jednorazowo operacja zależy od administratora MFA ustawienie i w związku z tym mogą być opcjonalne. Dla domeny MFA włączone ten krok jest czasami wstępnie zdefiniowane (na przykład domeny wymaga użytkowników przy użyciu karty inteligentnej i numeru pin).  
![Ustawienia 3mfa][3]

5. Możliwe drugi raz, które okno dialogowe pozwala wybrać szczegóły metody uwierzytelniania. Możliwości są skonfigurowane przez administratora.
![4mfa-Sprawdź-1][4]
 
6. Usługi Azure Active Directory przesyła potwierdzenie informacje do Ciebie. Gdy zostanie wyświetlony kod weryfikacyjny, wprowadź je w polu **Wprowadź kod weryfikacyjny** , a następnie kliknij przycisk **Zaloguj**.
![5mfa-Sprawdź-2][5]

Po zakończeniu weryfikacji SSMS łączy, zwykle przy założeniu prawidłowe poświadczenia i dostęp przez zaporę.

##<a name="next-steps"></a>Następne kroki  

Udzielanie innym dostępu do bazy danych: [Uwierzytelnianie bazy danych SQL i autoryzacji: udzielanie dostępu](sql-database-manage-logins.md)  
Upewnij się, inne osoby mogą nawiązywanie połączenia przez zaporę: [Konfigurowanie reguły zapory na poziomie serwera bazy danych SQL Azure za pomocą portalu Azure](sql-database-configure-firewall-settings.md)


[1]: ./media/sql-database-ssms-mfa-auth/1mfa-universal-connect.png
[2]: ./media/sql-database-ssms-mfa-auth/2mfa-sign-in.png
[3]: ./media/sql-database-ssms-mfa-auth/3mfa-setup.png
[4]: ./media/sql-database-ssms-mfa-auth/4mfa-verify-1.png
[5]: ./media/sql-database-ssms-mfa-auth/5mfa-verify-2.png

