<properties
   pageTitle="Omówienie zabezpieczeń bazy danych SQL"
   description="Informacje dotyczące zabezpieczeń bazy danych SQL Azure i SQL Server, w tym różnice w chmurze i SQL Server lokalnego w przypadku uwierzytelniania, autoryzacji, zabezpieczenia połączeń, szyfrowania i zgodność."
   services="sql-database"
   documentationCenter=""
   authors="tmullaney"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management"
   ms.date="06/09/2016"
   ms.author="thmullan;jackr"/>


# <a name="securing-your-sql-database"></a>Zabezpieczanie bazy danych SQL

## <a name="overview"></a>Omówienie

W tym artykule opisano podstawy zabezpieczanie warstwie danych aplikację za pomocą narzędzia bazy danych SQL Azure. W szczególności w tym artykule spowoduje rozpoczęcie pracy z zasobów dla ograniczanie dostępu, ochrona danych, i monitorowanie aktywności w bazie danych utworzone w [rozpocząć pracę z bazą danych SQL samouczek](sql-database-get-started.md). Pełne omówienie funkcji zabezpieczeń dostępnych na wszystkie podtypy programu SQL Server zobacz [Centrum zabezpieczeń dla aparatu bazy danych programu SQL Server i bazy danych SQL Azure](https://msdn.microsoft.com/library/bb510589). Dodatkowe informacje są także dostępne w [zabezpieczeń i bazy danych SQL Azure techniczne oficjalny dokument](https://download.microsoft.com/download/A/C/3/AC305059-2B3F-4B08-9952-34CDCA8115A9/Security_and_Azure_SQL_Database_White_paper.pdf) (PDF).

## <a name="connection-security"></a>Zabezpieczenia połączeń

Zabezpieczenia połączeń odwołuje się do jak ograniczać i bezpiecznego połączenia z bazą danych przy użyciu reguły zapory i szyfrowania połączenia.

Reguły zapory są używane zarówno przez serwer i bazę danych do odrzucenia próby nawiązania połączenia z adresów IP, które nie zostały jawnie whitelisted. Aby umożliwić aplikacji lub publiczny adres IP komputera klienckiego nawiązania połączenia z nową bazą danych, musisz najpierw utworzyć reguły zapory na poziomie serwera przy użyciu klasycznej Portal Azure, interfejsu API usługi REST lub programu PowerShell. Zgodnie z zaleceniami dotyczącymi należy ograniczyć dozwolone przez zaporę serwera możliwie zakresów adresów IP. Aby uzyskać więcej informacji zobacz [Zapory bazy danych SQL Azure](https://msdn.microsoft.com/library/ee621782).

Wszystkie połączenia z bazą danych SQL Azure wymagają szyfrowania (SSL/TLS) w zawsze podczas danych jest "w drodze" oraz z bazy danych. W parametrach połączenia aplikacji, należy określić parametry szyfrowanie połączenie i *nie* można ufać certyfikat (jest to dla Ciebie po skopiowaniu ciąg połączenia z portalem klasyczny Azure), w przeciwnym razie połączenia nie sprawdza tożsamości serwera, a zostanie podatne na ataki "mężczyzna w środku". Sterownik ADO.NET, na przykład te ciąg parametry połączenia są **Szyfrowanie = True** i **TrustServerCertificate = False**. Aby uzyskać więcej informacji zobacz [szyfrowanie połączenia bazy danych SQL Azure i sprawdzanie poprawności certyfikatu](https://msdn.microsoft.com/library/azure/ff394108#encryption).


## <a name="authentication"></a>Uwierzytelnianie

Uwierzytelnianie odwołuje się do jak potwierdza tożsamość użytkownika podczas nawiązywania połączenia z bazą danych. Baza danych SQL obsługuje dwa typy uwierzytelniania:

 - **Uwierzytelnianie programu SQL**, która używa nazwy użytkownika i hasła
 - **Uwierzytelnianie usługi azure Active Directory**, która korzysta z tożsamości zarządzane przez usługi Azure Active Directory i jest obsługiwane w przypadku zarządzania oraz integracji domen

Po utworzeniu logiczne server dla bazy danych za pomocą nazwy użytkownika i hasła określić logowania "administrator serwera". Przy użyciu tych poświadczeń, użytkownik może przeprowadzać uwierzytelnianie do dowolnej bazy danych na tym serwerze jako właściciel bazy danych lub "dbo". Jeśli chcesz użyć uwierzytelnianie usługi Azure Active Directory, należy utworzyć inny administrator serwera o nazwie "Administrator Azure AD," co jest dozwolone administrowania Azure AD użytkownicy i grupy. Ten administrator może również wykonywać wszystkie operacje, które można zwykła server administrator. Aby skorzystać z przewodnika utworzyć administratorem Azure AD, aby włączyć uwierzytelnianie usługi Azure Active Directory, zobacz [Łączenie się SQL bazy danych przy użyciu Azure uwierzytelnianie usługi Active Directory](sql-database-aad-authentication.md) .

Zgodnie z zaleceniami dotyczącymi aplikacji powinna uwierzytelniania — w ten sposób można ograniczyć uprawnienia do aplikacji i zmniejszyć ryzyko złośliwych działań w przypadku, gdy występuje ataki uruchomienie SQL w kodzie aplikacji za pomocą innego konta. Zalecane podejście jest utworzenie, [zawarte użytkownika bazy danych](https://msdn.microsoft.com/library/ff929188), który umożliwia aplikacji do uwierzytelnienia bezpośrednio do jednej bazie danych. Możesz utworzyć użytkownika zawartych bazy danych, w której jest używane uwierzytelnianie SQL, wykonując następujące polecenie T-SQL podczas połączenia z bazą danych użytkownika jako logowania administrator serwera:

```
CREATE USER ApplicationUser WITH PASSWORD = 'strong_password'; -- SQL Authentication
```

Jeśli utworzono administratorem Azure AD, można utworzyć użytkownika bazy danych zawartych używającej uwierzytelnianie usługi Azure Active Directory, wykonując następujące polecenie T-SQL podczas połączenia z bazą danych użytkownika jako administrator usługi Azure AD:

```
CREATE USER [Azure_AD_principal_name | Azure_AD_group_display_name] FROM EXTERNAL PROVIDER; -- Azure Active Directory Authentication
```

W obu przypadkach parametry połączenia aplikacji należy określić tych poświadczeń użytkownika, zamiast logowania administrator serwera, połączenia z bazą danych.

Aby uzyskać więcej informacji na uwierzytelniania z bazą danych SQL zobacz [Zarządzanie bazami danych i logowania w bazie danych SQL Azure](sql-database-manage-logins.md).


## <a name="authorization"></a>Autoryzacja
Autoryzacja odwołuje się do co można zrobić w bazie danych SQL Azure, a to jest kontrolowane przez swoje konto użytkownika członkostwo ról i uprawnień. Zgodnie z zaleceniami dotyczącymi należy zezwolić użytkownikom co najmniej uprawnienia wymagane. Baza danych SQL Azure ułatwia zarządzanie rolami w T-SQL:

```
ALTER ROLE db_datareader ADD MEMBER ApplicationUser; -- allows ApplicationUser to read data
ALTER ROLE db_datawriter ADD MEMBER ApplicationUser; -- allows ApplicationUser to write data
```

Konta administratora serwera, którego łączysz się z jest członkiem db_owner, która ma uprawnienia do wykonywać żadnych w bazie danych. Zapisz to konto do wdrażania uaktualnień schematu i innych operacji zarządzania. Za pomocą konta "ApplicationUser" bardziej ograniczone uprawnienia do nawiązywania połączeń z poziomu aplikacji do bazy danych z co najmniej uprawnienia wymagane przez aplikację.

Istnieją sposoby zawęzić, co może wykonać użytkownik z bazy danych SQL Azure:

* [Role bazy danych](https://msdn.microsoft.com/library/ms189121) inne niż db_datareader i db_datawriter umożliwia tworzenie bardziej zaawansowanych kont użytkowników aplikacji lub mniej zaawansowanych zarządzania kontami.
* Szczegółowego [uprawnienia](https://msdn.microsoft.com/library/ms191291) umożliwiają sterowanie operacje możesz wykonać na poszczególnych kolumn, tabel, widoków, procedur i innych obiektów w bazie danych.
* [Personifikacja](https://msdn.microsoft.com/library/vstudio/bb669087) i [Podpisywanie modułów](https://msdn.microsoft.com/library/bb669102) umożliwia bezpieczne tymczasowo podniesienia poziomu uprawnień.
* Można używać [Zabezpieczeń na poziomie wiersza](https://msdn.microsoft.com/library/dn765131) limit, które wiersze użytkownika mogą uzyskiwać dostęp.
* [Maskowanie danych](sql-database-dynamic-data-masking-get-started.md) można ograniczyć narażenie ważnych danych.
* [Procedury przechowywane](https://msdn.microsoft.com/library/ms190782) umożliwia ograniczenie działań, które należy podjąć w bazie danych.

Zarządzanie bazami danych i serwerów logicznych z portalu klasyczny Azure lub za pomocą interfejsu API Menedżera zasobów Azure steruje przypisania roli konta użytkownika portalu. Aby uzyskać więcej informacji na ten temat zobacz [Kontrola dostępu oparta na rolach w Azure Portal](../active-directory./role-based-access-control-configure.md).


## <a name="encryption"></a>Szyfrowanie

Baza danych SQL Azure ułatwia ochronę danych przez szyfrowanie danych, gdy jest "w stanie spoczynku" lub przechowywane w plikach bazy danych i kopie zapasowe, używanie [Przejrzystego szyfrowania danych](http://go.microsoft.com/fwlink/?LinkId=526242). Aby zaszyfrować bazę danych, łączenie jako właściciel bazy danych i wykonać:

```
ALTER DATABASE [AdventureWorks] SET ENCRYPTION ON;
```

Inne sposoby szyfrowanie hasła do danych należy rozważyć:

* [Poziom komórek szyfrowania](https://msdn.microsoft.com/library/ms179331.aspx) do szyfrowania określone kolumny lub nawet komórek danych z innych kluczy szyfrowania.
* Jeśli potrzebujesz sprzętowego modułu zabezpieczeń lub witryny centralnego zarządzania hierarchii klucza szyfrowania, rozważ użycie [Magazynu klucza Azure z programem SQL Server w maszyn wirtualnych Azure](http://blogs.technet.com/b/kv/archive/2015/01/12/using-the-key-vault-for-sql-server-encryption.aspx).
* [Zawsze szyfrowane](https://msdn.microsoft.com/library/mt163865.aspx) (w wersji preview) powoduje szyfrowania przezroczysty aplikacji i umożliwia klientom do szyfrowania poufnych danych w aplikacjach klienckich bez udostępniania kluczy szyfrowania z bazą danych SQL.

## <a name="auditing"></a>Inspekcja

Inspekcja i śledzenia zdarzeń bazy danych może pomóc Obsługa zgodności z przepisami i zidentyfikować podejrzane działania. Inspekcja bazy danych SQL umożliwia rejestrowanie zdarzeń w bazie danych do inspekcji logowania na koncie magazyn Azure. Inspekcja bazy danych SQL integruje także Microsoft Power BI w celu ułatwienia rozwijania raportów i analiz. Aby uzyskać więcej informacji zobacz [Wprowadzenie do inspekcji bazy danych SQL](sql-database-auditing-get-started.md).

Wykrywanie zagrożenie bazy danych SQL zapewnia dodatkową warstwę zabezpieczeń na bieżąco inspekcja. Pozwala odpowiedzieć na zagrożenia występujące, dostarczając alertów zabezpieczeń anomalous działalności. Aby uzyskać więcej informacji zobacz [Wprowadzenie do wykrywania zagrożenie bazy danych SQL](sql-database-threat-detection-get-started.md).  

## <a name="compliance"></a>Zgodność

Oprócz powyższych funkcji i funkcji, które mogą ułatwić aplikacji spełniają różne zgodności wymagań dotyczących zabezpieczeń, bazy danych SQL Azure również uczestniczy w regularnych inspekcji i uzyskał certyfikat przed liczbą standardy zgodności. Aby uzyskać więcej informacji zobacz [Centrum zaufania programu Microsoft Azure](https://azure.microsoft.com/support/trust-center/), gdzie można znaleźć najbardziej aktualną listę [certyfikaty zgodności bazy danych SQL](https://azure.microsoft.com/support/trust-center/services/).
