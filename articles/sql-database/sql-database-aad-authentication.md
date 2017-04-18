<properties
   pageTitle="Łączenie bazy danych SQL lub magazynem danych SQL za pomocą uwierzytelniania usługi Azure Active Directory | Microsoft Azure"
   description="Dowiedz się, jak nawiązać połączenia z bazą danych SQL przy użyciu uwierzytelnianie usługi Azure Active Directory."
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
   ms.date="09/16/2016"
   ms.author="rick.byham@microsoft.com"/>

# <a name="connecting-to-sql-database-or-sql-data-warehouse-by-using-azure-active-directory-authentication"></a>Łączenie z bazą danych SQL lub magazynu danych SQL za pomocą uwierzytelniania usługi Azure Active Directory

Azure uwierzytelniania usługi Active Directory jest mechanizmu łączenia z platformy Microsoft Azure SQL Database i [Magazynu danych SQL](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) przy użyciu tożsamości w usłudze Azure Active Directory (Azure AD). Uwierzytelnianie usługi Azure Active Directory możesz centralnie zarządzać tożsamości bazy danych użytkowników i innych usług firmy Microsoft w jednej centralnej lokalizacji. Centralne zarządzanie identyfikator zawiera jednego miejsca do zarządzania użytkownikami bazy danych i upraszcza zarządzanie uprawnieniami. Następujące korzyści:

- Umożliwia zamiast uwierzytelnianie programu SQL Server.
- Ułatwia zatrzymanie mnożenia tożsamości użytkownika na serwerach bazy danych.
- Umożliwia obrotu hasła w jednym miejscu
- Klientów można zarządzać uprawnieniami bazy danych przy użyciu grup zewnętrznych (AAD).
- Można go wyeliminować przechowywania haseł, włączając zintegrowane uwierzytelnianie systemu Windows i innych rodzajów uwierzytelniania obsługiwane przez usługi Azure Active Directory.
- Azure Active Directory uwierzytelnianiu zawarte użytkowników bazy danych do uwierzytelnienia tożsamości na poziomie bazy danych.
- Azure Active Directory obsługuje tokenów uwierzytelniania dla aplikacji nawiązywanie połączenia z bazą danych SQL.
- Azure uwierzytelniania usługi Active Directory obsługuje ADFS (Federacja domen) lub uwierzytelniania natywnych użytkownika i hasła dla lokalnego Azure Active Directory bez domeny synchronizacji.  
- Azure Active Directory obsługuje korzystających z programu SQL Server Management Studio uniwersalny uwierzytelnianie usługi Active Directory, która zawiera uwierzytelnianie wieloskładnikowe (MFA).  MFA zawiera silnego uwierzytelniania do określonego zakresu opcje weryfikacji łatwe — rozmowy telefonicznej, wiadomości SMS, kart inteligentnych za pomocą numeru pin lub powiadomienie aplikacji dla urządzeń przenośnych. Aby uzyskać więcej informacji zobacz [SSMS obsługę Azure AD MFA z bazy danych SQL i magazynu danych SQL](sql-database-ssms-mfa-authentication.md).

Kroki konfiguracji obejmują poniższych procedur, aby skonfigurować i uwierzytelnianie usługi Azure Active Directory.

1. Utwórz i wypełnij usługi Azure Active Directory.
2. Upewnij się, że baza danych znajduje się w wersji 12 bazy danych SQL Azure. (Nie wymagany do magazynu danych SQL.)
3. Opcjonalnie: Kojarzenie lub zmienianie usługi active directory, która jest aktualnie skojarzona z subskrypcją usługi Azure.
4. Tworzenie administratora usługi Azure Active Directory dla programu SQL Server Azure lub [Magazynu danych SQL Azure](https://azure.microsoft.com/services/sql-data-warehouse/).
5. Konfigurowanie komputerów klienckich.
6. Tworzenie użytkowników bazy danych zawartych w bazie danych zamapowane Azure AD tożsamości.
7. Nawiązywanie połączenia z bazą danych przy użyciu tożsamości Azure AD.


## <a name="trust-architecture"></a>Ufaj architektura

Poniższy diagram wysokiego poziomu przedstawiono architekturę rozwiązania przy użyciu funkcji uwierzytelniania Azure AD przy użyciu bazy danych SQL Azure. Zastosuj te same pojęcia do magazynu danych SQL. Do pomocy technicznej usługi Azure Active Directory natywnych hasła, jest traktowany jako tylko część chmury i bazy danych SQL Azure AD-Azure. Do obsługi uwierzytelniania Sfederowane (lub użytkownika i hasło dla poświadczeń systemu Windows) jest wymagana komunikacja z usługami ADFS blokiem. Strzałki wskazują ścieżek komunikacji.

![AAD auth diagram][1]

Poniższy diagram przedstawia Federacji, zaufania i hostingu relacji, umożliwiające klienta do połączenia z bazą danych przesyłając tokenu. Token jest uwierzytelnianie przez Azure AD i jest zaufany w bazie danych. Odbiorcy 1 może oznaczać usługi Azure Active Directory użytkownikom natywnych lub usługi Azure Active Directory z użytkownikami federacyjnymi. Odbiorcy 2 reprezentuje możliwe rozwiązanie, łącznie z zaimportowanych użytkowników. w tym przykładzie pochodzące z federacyjnych usługi Azure Active Directory z usługami ADFS synchronizowane z usługą Azure Active Directory. Należy zrozumieć, że dostęp do bazy danych przy użyciu funkcji uwierzytelniania Azure AD wymaga, że hostingu subskrypcji jest skojarzony z usługą Azure Active Directory. Tej samej subskrypcji należy utworzyć programu SQL Server bazy danych SQL Azure lub magazynu danych SQL.

![Relacja subskrypcji][2]

## <a name="administrator-structure"></a>Struktura administratora

Przy użyciu funkcji uwierzytelniania Azure AD, istnieją dwa konta administratora na serwerze bazy danych SQL; oryginalny administratora programu SQL Server i administratora Azure AD. Zastosuj te same pojęcia do magazynu danych SQL. Tylko administrator oparte na konto Azure AD może utworzyć pierwszego użytkownika bazy danych Azure AD zawarte w bazie danych użytkownika. Identyfikator logowania administratora Azure AD może być Azure AD użytkownika lub grupy w usłudze Azure AD. Gdy administrator jest konto grupy, mogą być używane przez dowolnego członka grupy Włączanie wielu administratorów Azure AD dla wystąpienie programu SQL Server. Przy użyciu konta grupy jako administrator usprawnia zarządzania, co umożliwia centralne Dodawanie i usuwanie członków grupy w Azure AD bez zmieniania użytkowników i uprawnień w bazie danych SQL. W dowolnym momencie można skonfigurować tylko jeden administrator Azure AD (użytkownika lub grupy).

![Struktura administratora][3]

## <a name="permissions"></a>Uprawnienia

Aby utworzyć nowych użytkowników, musisz mieć `ALTER ANY USER` uprawnień w bazie danych. `ALTER ANY USER` Mogą być przyznane uprawnienia innych użytkowników bazy danych. `ALTER ANY USER` Uprawnień odbywa się również konta administratora serwera, a użytkownicy bazy danych z `CONTROL ON DATABASE` lub `ALTER ON DATABASE` uprawnień dla tej bazy danych, a także przez członków `db_owner` roli bazy danych.

Aby utworzyć użytkownika bazy danych zawartych w bazy danych SQL Azure lub magazynu danych SQL, umożliwia nawiązanie połączenia z bazą danych przy użyciu tożsamości Azure AD. Aby utworzyć pierwszy użytkownik zamkniętego bazy danych, należy połączenia z bazą danych przy użyciu administratora usługi Azure Active Directory (który jest właściciel bazy danych). Jest to pokazano w kroku 4 i 5 poniżej. Uwierzytelniania usługi Azure Active Directory jest możliwe tylko jeśli administrator usługi Azure Active Directory została utworzona dla bazy danych SQL Azure lub magazynu danych programu SQL server. Jeśli administrator usługi Azure Active Directory została usunięta z serwera, istniejących użytkowników usługi Azure Active Directory utworzone wcześniej wewnątrz programu SQL Server można już połączenia z bazą danych przy użyciu poświadczeń usługi Azure Active Directory.

## <a name="azure-ad-features-and-limitations"></a>Azure AD funkcje i ograniczenia

Następujące składniki usługi Azure Active Directory można obsługi administracyjnej, Azure SQL Server lub SQL Data Warehouse:

- Natywne członków: członka utworzone w Azure AD w tej domenie zarządzanych lub w domenie klienta. Aby uzyskać więcej informacji zobacz [Dodawanie własnej nazwy domeny, aby Azure AD](../active-directory/active-directory-add-domain.md).

- Federacji członków domeny: członka utworzone w Azure AD przy użyciu federacyjnych domeny. Aby uzyskać więcej informacji zobacz [Microsoft Azure teraz obsługuje Federację z usługą Active Directory systemu Windows Server](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/).

- Zaimportowany członków z innych katalogów Active Azure, którzy są członkami domeny natywnych lub federacyjnych.

- Grupy usługi Active Directory tworzone jako grupy zabezpieczeń.

Konta Microsoft (na przykład outlook.com, hotmail.com i live.com) lub inne konta gościa (na przykład gmail.com, yahoo.com) nie są obsługiwane. Jeśli możesz się zalogować do [https://login.live.com](https://login.live.com) przy użyciu konta i hasła, są za pomocą konta Microsoft, które nie jest obsługiwane dla uwierzytelniania Azure AD dla bazy danych SQL Azure lub magazynu danych SQL Azure.

### <a name="additional-considerations"></a>Uwagi dodatkowe

- Aby zwiększyć zarządzania, zaleca się, że świadczenie dedykowane grupy usługi Azure Active Directory jako administrator.
- W dowolnym momencie dla Azure SQL Server lub magazynu danych SQL Azure można skonfigurować tylko jeden administrator Azure AD (użytkownika lub grupy).
- Tylko administrator usługi Azure Active Directory dla programu SQL Server początkowo łączyć się Azure SQL Server lub magazynu danych SQL Azure za pomocą konta usługi Azure Active Directory. Administrator usługi Active Directory może skonfigurować kolejnych użytkowników bazy danych usługi Azure Active Directory.
- Zaleca się ustawianie limitu czasu połączeń do 30 sekund.
- SQL Server 2016 Management Studio i SQL Server Data Tools dla programu Visual Studio 2015 (14.0.60311.1April wersji 2016 lub nowszej) obsługuje uwierzytelnianie usługi Azure Active Directory. (Uwierzytelnianie azure Active Directory jest obsługiwane przez **Dostawca danych programu .NET Framework dla serwera SQL**; na co najmniej wersji .NET Framework 4.6). W związku z tym uwierzytelniania usługi Azure Active Directory można używać najnowszej wersji tych narzędzi i aplikacji warstwy danych (DAC i .bacpac).
- [Wersja ODBC 13.1](https://www.microsoft.com/download/details.aspx?id=53339) obsługuje uwierzytelnianie usługi Azure Active Directory, jednak `bcp.exe` nie można połączyć przy użyciu funkcji uwierzytelniania usługi Azure Active Directory, ponieważ korzysta ona z dostawcą starsze ODBC.
- `sqlcmd`obsługuje usługi Azure Active Directory uwierzytelniania począwszy od wersji 13.1 dostępne w [Centrum pobierania](http://go.microsoft.com/fwlink/?LinkID=825643).  
- Program SQL Server Data Tools dla programu Visual Studio 2015 wymaga co najmniej wersji 2016 kwietnia narzędzia danych (wersja 14.0.60311.1). Obecnie użytkownicy usługi Azure Active Directory nie są wyświetlane w Eksploratorze obiektów SSDT. Aby obejść ten problem należy wyświetlić użytkowników w [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx).
- [Program Microsoft JDBC sterownik 6.0 dla programu SQL Server](https://www.microsoft.com/en-us/download/details.aspx?id=11774) obsługuje uwierzytelnianie usługi Azure Active Directory. Zobacz też [Ustawienie właściwości połączenia](https://msdn.microsoft.com/library/ms378988.aspx).
- PolyBase nie uwierzytelniania za pomocą uwierzytelniania usługi Azure Active Directory.
- Kilka narzędzi, takich jak funkcje analizy Biznesowej i programu Excel nie są obsługiwane.
- Azure uwierzytelniania usługi Active Directory jest obsługiwane bazy danych SQL Azure portalu karty **Baz danych importu** i **Eksportu** . Importowanie i eksportowanie przy użyciu funkcji uwierzytelniania usługi Azure Active Directory obsługiwane jest też polecenia programu PowerShell.


## <a name="1-create-and-populate-an-azure-ad"></a>1. Tworzenie i wypełnić Azure AD

Tworzenie usługi Azure Active Directory i wypełnij go użytkownicy i grupy. Usługi Azure Active Directory może być domeną zarządzanych domenę początkową Azure AD. Usługi Azure Active Directory można także lokalnej usługi Active Directory federacji z usługą Azure Active Directory.

Aby uzyskać więcej informacji zobacz [Integrowanie tożsamości lokalnych z usługą Azure Active Directory](../active-directory/active-directory-aadconnect.md), [Dodaj własną nazwę domeny do Azure AD](../active-directory/active-directory-add-domain.md) [platformy Microsoft Azure teraz obsługuje Federację z usługą Active Directory systemu Windows Server](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), [Administrowanie katalogu Azure AD](https://msdn.microsoft.com/library/azure/hh967611.aspx)i [Zarządzanie Azure AD przy użyciu programu Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx).

## <a name="2-ensure-your-sql-database-is-version-12"></a>2. Upewnij się, że bazy danych SQL jest wersja 12

Azure uwierzytelniania usługi Active Directory są obsługiwane w najnowszej wersji 12 bazy danych SQL. Aby uzyskać informacje o wersji 12 bazy danych SQL i, aby dowiedzieć się, czy jest on dostępny w danym regionie zobacz [Co nowego w najnowszej wersji 12 aktualizacji bazy danych SQL](sql-database-v12-whats-new.md). Ten krok nie jest konieczne dla magazynu danych SQL Azure, ponieważ program SQL Data Warehouse jest dostępna tylko w wersji 12.

Jeśli masz istniejącej bazy danych, należy zweryfikować, że jest ona hostowana w wersji 12 bazy danych SQL za pomocą połączenia z bazą danych (na przykład za pomocą programu SQL Server Management Studio) i nadzorowaniem `SELECT @@VERSION;`. Oczekiwany wynik dla bazy danych w bazie danych SQL w wersji 12 jest co najmniej **Microsoft SQL Azure (RTM) - 12.0**. Jeśli bazy danych nie jest obsługiwany w wersji 12 bazy danych SQL, zobacz [Planowanie i przygotować przeprowadzić uaktualnienie do wersji 12 bazy danych SQL](sql-database-v12-plan-prepare-upgrade.md), a następnie odwiedź Portal klasyczny Azure, aby przeprowadzić migrację bazy danych do bazy danych SQL w wersji 12.

Alternatywnie możesz utworzyć nowej bazy danych w bazie danych SQL w wersji 12, wykonując kroki opisane w sekcji [Tworzenie pierwszej bazy danych Azure SQL](sql-database-get-started.md). **Porada**: czytanie następnej czynności przed wybraniem subskrypcji dla nowej bazy danych.

## <a name="3-optional-associate-or-change-the-active-directory-that-is-currently-associated-with-your-azure-subscription"></a>3. opcjonalne: Kojarzenie lub zmienianie usługi active directory, która jest aktualnie skojarzona z subskrypcją usługi Azure

Aby skojarzyć bazy danych z katalogu Azure AD dla Twojej organizacji, katalogu zaufanych katalog był Azure subskrypcji obsługującym bazę danych. Aby uzyskać więcej informacji zobacz [jak Azure subskrypcje są skojarzone z usługą Azure Active Directory](https://msdn.microsoft.com/library/azure/dn629581.aspx).

**Informacje dodatkowe:** Co Azure Subskrypcja obejmuje relacji zaufania z wystąpieniem Azure AD. Oznacza to, zaufaną tego katalogu do uwierzytelniania użytkowników, usług i urządzeń. Wiele subskrypcji można ufać tego samego katalogu, ale subskrypcji zaufania tylko jednego katalogu. Widać katalogu jest zaufany subskrypcji na karcie **Ustawienia** w [https://manage.windowsazure.com/](https://manage.windowsazure.com/). Tej relacji zaufania, która subskrypcji z katalogu różni się od relacji, która ma subskrypcję z wszystkich innych zasobów w Azure (witryny sieci Web, bazy danych i tak dalej), które przypominają zasobów podrzędne subskrypcji. Jeśli subskrypcja wygaśnie, następnie dostęp do tych innych zasobów skojarzone z subskrypcją również zatrzymuje. Ale katalogu pozostanie w Azure i można skojarzyć innej subskrypcji ten katalog i przejdź do zarządzania użytkownikami katalogu. Aby uzyskać więcej informacji o zasobach zobacz [Opis dostęp do zasobów w Azure](https://msdn.microsoft.com/library/azure/dn584083.aspx).

Poniższe procedury zawierają instrukcje krok po kroku dotyczące sposobu zmieniania katalogu skojarzone dla danej subskrypcji.

1. Łączenie do sieci [Klasyczny Portal Azure](https://manage.windowsazure.com/) za pomocą administrator Azure subskrypcji.
2. Na banerze po lewej stronie wybierz pozycję **Ustawienia**.
3. Subskrypcji jest wyświetlany na ekranie ustawienia. Jeśli odpowiedniej subskrypcji nie jest wyświetlana, kliknij pozycję **Subskrypcje** u góry, listy rozwijanej **Filtru w katalogu** i wybierz katalog zawierający subskrypcji, a następnie kliknij **Zastosuj**.

    ![Wybierz subskrypcję][4]
4. W obszarze **Ustawienia** kliknij subskrypcję, a następnie kliknij **Edytuj katalogu** w dolnej części strony.

    ![AD ustawienia portalu][5]
5. W oknie dialogowym **Edytowanie katalogu** usługi Azure Active Directory, który jest skojarzony z programu SQL Server lub SQL Data Warehouse zaznacz, a następnie kliknij strzałkę, aby dalej.

    ![Edycja katalogu zaznacz][6]
6. W oknie dialogowym **Potwierdzanie** katalogu mapowania potwierdzić, że "**wszystkich administratorów współpracujących zostaną usunięte.**"

    ![Potwierdź katalogu edycji][7]
7. Kliknij pozycję Sprawdź ponownie załadować portalu.

> [AZURE.NOTE] Po zmianie katalogu, dostęp do wszystkich administratorów współpracujących, Azure AD użytkowników i grup oraz użytkowników katalog kopii zasobów są usuwane i już nie mają dostęp do tej subskrypcji lub jego zasobów. Tylko, jako administrator usługi, można skonfigurować dostępu do głównych według nowego katalogu. Tej zmiany może potrwać znacznych ilość czasu propagowanie wszystkie zasoby. Zmiana katalogu, również zmieni administrator Azure AD dla bazy danych SQL i magazynu danych SQL i nie zezwalaj na dostęp do bazy danych dla wszystkich istniejących użytkowników Azure AD. Administrator usługi Azure AD musi być resetowanie (opisane poniżej) i nowe Azure AD użytkownicy muszą zostać utworzone.

## <a name="4-create-an-azure-ad-administrator-for-azure-sql-server"></a>4. Tworzenie administrator Azure AD dla programu SQL Server Azure

Każdy Azure SQL Server (który obsługuje bazy danych SQL lub magazynu danych SQL) zostanie uruchomiony przy użyciu konta administratora pojedynczego serwera jest administratorem całego serwera SQL Azure. Druga administratora programu SQL Server muszą zostać utworzone, który jest konto Azure AD. Tego podmiotu jest tworzony jako użytkownik bazy danych zawartych w bazie danych wzorca. Jako administratorzy konta administratora serwera są członkami **db_owner we wszystkich bazy danych użytkowników** i wprowadź każdy użytkownik bazy danych jako użytkownik **dbo** . Aby uzyskać więcej informacji na temat kont administratora serwera zobacz [Zarządzanie bazami danych i logowania w bazie danych SQL Azure](sql-database-manage-logins.md) i z logowania użytkowników sekcji **i** [wskazówki dotyczące zabezpieczeń bazy danych SQL Azure i ograniczenia](sql-database-security-guidelines.md).

Korzystając z usługi Azure Active Directory z replikacją Geo, administrator usługi Azure Active Directory musi być skonfigurowany zarówno podstawowych i pomocniczych serwerów. Jeśli serwer nie ma administratora usługi Azure Active Directory, następnie logowania do usługi Azure Active Directory, aby użytkownicy odbierane "nie można nawiązać połączenia" Błąd serwera.

> [AZURE.NOTE] Użytkownicy, którzy nie są oparte na konto Azure AD (w tym konto administratora serwera SQL Azure), nie można utworzyć Azure AD użytkownicy, ponieważ nie masz uprawnień do sprawdzania poprawności proponowane bazy danych użytkownikom Azure AD.

### <a name="provision-an-azure-active-directory-administrator-for-your-azure-sql-server-by-using-the-azure-portal"></a>Inicjowanie obsługi usługi Azure Active Directory administratorem serwera SQL Azure za pomocą portalu Azure

1. W [portalu Azure](https://portal.azure.com/), w prawym górnym rogu kliknij pozycję połączenie, aby rozwinąć listę możliwych katalogów Active. Wybierz poprawny usługi Active Directory jako domyślne Azure AD. W tym kroku łączy skojarzenia subskrypcji z usługą Active Directory z serwerem SQL Azure, upewniając się, że samej subskrypcji jest używany w obu Azure AD i SQL Server. (Program SQL Server Azure można hostingu, bazy danych SQL Azure lub magazynu danych SQL Azure.)

    ![Wybierz pozycję ad][8]
2. Na transparencie po lewej stronie wybierz pozycję **serwerów SQL**, wybierz pozycję z **programu SQL server**, a następnie w karta **Programu SQL Server** u góry kliknij **Ustawienia**.

    ![Ustawienia AD][9]
3. Karta **Ustawienia** kliknij ** Administrator usługi Active Directory.
4. W karta **administratora usługi Active Directory** kliknij pozycję **Administrator usługi Active Directory**i u góry, kliknij przycisk **Ustaw administratora**.
5. W karta **Dodaj administratora** , wyszukiwania dla użytkownika, zaznacz użytkownika lub grupę jako administrator, a następnie kliknij **Wybierz**. (Karta administratora usługi Active Directory zawiera wszystkie elementy członkowskie i grup usługi Active Directory. Nie można wybrać użytkowników lub grup, które są wyszarzone, ponieważ nie są obsługiwane jako administratorzy Azure AD. (Zobacz listę obsługiwanych administratorów w **Azure AD funkcje i ograniczenia** powyżej). Kontrola dostępu oparta na rolach (RBAC) dotyczy tylko portal usługi i nie jest propagowana do programu SQL Server.
6. W górnej części karta **administratora usługi Active Directory** kliknij przycisk **ZAPISZ**.
    ![Wybierz pozycję administrator][10]

    Proces zmiany administrator może potrwać kilka minut. Nowy administrator pojawia się w polu **Administrator usługi Active Directory** .

> [AZURE.NOTE] Podczas konfigurowania administrator Azure AD, nową nazwę administratora (użytkownika lub grupy) nie można już znajdować się w wirtualnej wzorca bazy danych jako użytkownik uwierzytelnianie programu SQL Server. Jeśli istnieją, ustawienia administracyjne Azure AD zakończy się niepowodzeniem; Wycofywanie jej tworzenia i wskazująca tego takich uprawnień administratora (nazwa) już istnieje. Ponieważ taki użytkownik uwierzytelnianie programu SQL Server nie jest częścią Azure AD, wszelkie starań, aby połączyć się z serwerem przy użyciu funkcji uwierzytelniania Azure AD zakończy się niepowodzeniem.

Aby później usunąć administrator u góry karta **administratora usługi Active Directory** , kliknij przycisk **Usuń administrator**, a następnie kliknij przycisk **Zapisz**.

### <a name="provision-an-azure-ad-administrator-for-azure-sql-server-by-using-powershell"></a>Inicjowanie obsługi administrator Azure AD dla programu SQL Server Azure przy użyciu programu PowerShell

Aby uruchomić polecenia cmdlet programu PowerShell, należy mieć Azure zainstalować i uruchomić programu PowerShell. Aby uzyskać szczegółowe informacje zobacz [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md).

Do zapewniania obsługi administratorem Azure AD, wykonaj następujące polecenia programu PowerShell Azure:

- Dodawanie AzureRmAccount
- Wybierz pozycję AzureRmSubscription


Inicjowanie obsługi i zarządzać nimi, administrator Azure AD użyć polecenia cmdlet:

| Nazwa polecenia cmdlet                                       | Opis                                                                                                     |
|---------------------------------------------------|-----------------------------------------------------------------------------------------------------------------|
| [Ustawianie AzureRmSqlServerActiveDirectoryAdministrator](https://msdn.microsoft.com/library/azure/mt603544.aspx)    | Inicjuje obsługę administracyjną administrator usługi Azure Active Directory dla programu SQL Server Azure lub magazynu danych SQL Azure. (Musi być z bieżącej subskrypcji). |
| [Usuń AzureRmSqlServerActiveDirectoryAdministrator](https://msdn.microsoft.com/library/azure/mt619340.aspx) | Usuwa administrator usługi Azure Active Directory dla programu SQL Server Azure lub magazynu danych SQL Azure. |
| [Get-AzureRmSqlServerActiveDirectoryAdministrator](https://msdn.microsoft.com/library/azure/mt603737.aspx)    | Zwraca informacje na temat administratora usługi Azure Active Directory skonfigurowana dla tego serwera SQL Azure lub magazynu danych SQL Azure. |

Aby wyświetlić więcej szczegółów dla każdej z tych poleceń, na przykład za pomocą polecenia programu PowerShell get-help ``get-help Set-AzureRmSqlServerActiveDirectoryAdministrator``.

Następujące przepisy skrypt grupy administratorów Azure AD o nazwie **DBA_Group** (identyfikator obiektu `40b79501-b343-44ed-9ce7-da4c8cc7353f`) **demo_server** serwera w grupie zasobów nazwę **grupy-23**:

```
Set-AzureRmSqlServerActiveDirectoryAdministrator –ResourceGroupName "Group-23"
–ServerName "demo_server" -DisplayName "DBA_Group"
```

Parametru wejściowego **DisplayName** akceptuje Azure AD nazwy wyświetlanej lub głównej nazwy użytkownika. Na przykład ``DisplayName="John Smith"`` i ``DisplayName="johns@contoso.com"``. Azure AD grup tylko Azure AD Nazwa wyświetlana jest obsługiwana.

> [AZURE.NOTE] Polecenia programu Azure PowerShell ```Set-AzureRmSqlServerActiveDirectoryAdministrator``` uniemożliwi inicjowania obsługi administracyjnej Administratorzy Azure AD dla użytkowników nieobsługiwane. Nieobsługiwane użytkownik może obsługi administracyjnej, ale nie można nawiązać połączenia z bazą danych. (Zobacz listę obsługiwanych administratorów w **Azure AD funkcje i ograniczenia** powyżej).

W poniższym przykładzie użyto opcjonalne **identyfikator obiektu**:

```
Set-AzureRmSqlServerActiveDirectoryAdministrator –ResourceGroupName "Group-23"
–ServerName "demo_server" -DisplayName "DBA_Group" -ObjectId "40b79501-b343-44ed-9ce7-da4c8cc7353f"
```

> [AZURE.NOTE] Azure AD **identyfikator obiektu** jest wymagane, gdy **DisplayName** nie jest unikatowy. Do pobierania wartości **identyfikator obiektu** i **DisplayName** , użyj sekcji klasyczny Portal Azure Active Directory i Wyświetl właściwości użytkownika lub grupy.

W poniższym przykładzie zwracany informacje o bieżącej administrator Azure AD dla programu SQL Server Azure:

```
Get-AzureRmSqlServerActiveDirectoryAdministrator –ResourceGroupName "Group-23" –ServerName "demo_server" | Format-List
```

W poniższym przykładzie usuwane administrator Azure AD:
```
Remove-AzureRmSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23" –ServerName "demo_server"
```

Umożliwia także obsługę Azure Active Directory Administrator przy użyciu interfejsów API pozostałych. Aby uzyskać więcej informacji zobacz [odwołanie interfejsu API usługi zarządzania pozostałych i operacje dla operacji bazy danych SQL Azure dla baz danych programu SQL Azure](https://msdn.microsoft.com/library/azure/dn505719.aspx)

## <a name="5-configure-your-client-computers"></a>5. Konfigurowanie komputerów klienckich

Na wszystkich komputerach klienckich, z których aplikacje lub użytkowników nawiązywanie połączenia z bazą danych SQL Azure lub magazynu danych SQL Azure za pomocą Azure AD tożsamości, trzeba zainstalować następujące oprogramowanie:

- .NET framework 4.6 lub nowszy z [https://msdn.microsoft.com/library/5a4x27ek.aspx](https://msdn.microsoft.com/library/5a4x27ek.aspx).
- Biblioteki uwierzytelniania usługi Azure Active Directory dla programu SQL Server (**ADALSQL. Biblioteka DLL**) jest dostępny w wielu językach (x86 i amd64) z Centrum pobierania w witrynie [Microsoft Active Directory Authentication Library dla programu Microsoft SQL Server](http://www.microsoft.com/download/details.aspx?id=48742).

### <a name="tools"></a>Narzędzia

- Zainstalowanie [Programu SQL Server 2016 Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) lub [SQL Server Data Tools dla programu Visual Studio 2015](https://msdn.microsoft.com/library/mt204009.aspx) spełnia wymagania .NET Framework 4.6.
- Wersja instalacje x86 SSMS **ADALSQL. Biblioteka DLL**.
- SSDT instaluje wersję amd64 **ADALSQL. Biblioteka DLL**.
- Najnowsze Visual Studio z [Programu Visual Studio do pobrania](https://www.visualstudio.com/downloads/download-visual-studio-vs) spełnia wymagania .NET Framework 4.6, ale nie można zainstalować wersję wymagane amd64 **ADALSQL. Biblioteka DLL**.

## <a name="6-create-contained-database-users-in-your-database-mapped-to-azure-ad-identities"></a>6. Tworzenie użytkowników bazy danych zawartych w bazie danych zamapowane tożsamości Azure AD

### <a name="about-contained-database-users"></a>O użytkownikach zawartych bazy danych

Azure uwierzytelniania usługi Active Directory wymaga użytkowników bazy danych ma zostać utworzony jako użytkowników zawarte bazy danych. Użytkownika bazy danych zawartych w oparciu tożsamości Azure AD jest użytkownika bazy danych, którego nie ma identyfikator logowania w bazie danych wzorca i które mapy do tożsamości w katalogu Azure AD, który jest skojarzony z bazą danych. Tożsamość Azure AD może być indywidualne konto użytkownika lub grupy. Aby uzyskać więcej informacji o użytkownikach zawartych bazy danych zobacz [Tworzenie zawarte użytkowników bazy danych i przenośnych bazy danych](https://msdn.microsoft.com/library/ff929188.aspx).

> [AZURE.NOTE] Nie można utworzyć użytkowników bazy danych (z wyjątkiem Administratorzy) za pomocą portalu. Role RBAC nie są przenoszone do programu SQL Server, bazy danych SQL lub magazynu danych SQL. Role RBAC Azure są używane do zarządzania zasobami Azure, a nie dotyczą uprawnień do bazy danych. Na przykład rola **Współautora serwera SQL** nie udzielić dostępu do łączenia się z bazą danych SQL lub magazynu danych SQL. Uprawnienia dostępu, musisz mieć udzielony bezpośrednio w bazie danych przy użyciu instrukcji Transact-SQL.

### <a name="connect-to-the-user-database-or-data-warehouse-by-using-sql-server-management-studio-or-sql-server-data-tools"></a>Łączenie w magazynie użytkownika bazy danych lub danych za pomocą programu SQL Server Management Studio lub SQL Server Data Tools

Aby potwierdzić administrator Azure AD jest poprawnie skonfigurowany, nawiązywanie połączenia z **wzorcem** bazy danych przy użyciu konta administratora Azure AD.
Aby zainicjować obsługę Azure AD zawarte bazy danych użytkownika (innych niż administrator serwera, który jest właścicielem bazy danych), połączenia z bazą danych przy użyciu tożsamości Azure AD, która ma dostęp do bazy danych.

> [AZURE.IMPORTANT] Obsługa uwierzytelniania usługi Azure Active Directory są dostępne w programie [SQL Server 2016 Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) i [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx) w Visual Studio 2015. Wersji 2016 sierpnia SSMS obsługuje również uniwersalny uwierzytelnianie usługi Active Directory, co pozwala administratorom wymaga uwierzytelniania wieloskładnikowego przy użyciu telefonu, wiadomości tekstowej, kart inteligentnych za pomocą numeru pin lub powiadomienie aplikacji dla urządzeń przenośnych.

#### <a name="connect-using-active-directory-integrated-authentication"></a>Nawiązywanie połączenia przy użyciu zintegrowanego uwierzytelniania usługi Active Directory

Ta metoda jest zalogowany do systemu Windows przy użyciu swoich poświadczeń usługi Azure Active Directory z domeny sfederowanej.

1. Uruchom Management Studio lub narzędzia danych i w oknie dialogowym **połączenie z serwerem** (lub **Nawiązywanie połączenia z aparatu bazy danych**) w oknie dialogowym **uwierzytelniania** wybierz **Zintegrowane uwierzytelnianie usługi Active Directory**. Hasło nie jest potrzebna lub można wprowadzić, ponieważ zostanie wyświetlona poświadczenia istniejące połączenia.
    ![Wybierz opcję zintegrowanego uwierzytelniania AD][11]

2. Kliknij przycisk **Opcje** , a następnie na stronie **Właściwości połączenia** w oknie dialogowym **połączenie z bazą danych** wpisz nazwę bazy danych, którą chcesz się połączyć.
    ![Wybierz nazwę bazy danych][13]


#### <a name="connect-using-active-directory-password-authentication"></a>Nawiązywanie połączenia przy użyciu funkcji uwierzytelniania hasła usługi Active Directory

Ta metoda połączenia z głównej nazwy Azure AD przy użyciu Azure AD zarządzane domeny. Można również użyć dla kont federacyjnych bez dostępu do domeny, na przykład podczas pracy zdalnej.

Tej metody można użyć, jeśli użytkownik jest zalogowany do systemu Windows przy użyciu poświadczeń z domeny, która nie jest Federacji Azure lub przy użyciu funkcji uwierzytelniania Azure AD przy użyciu Azure AD na podstawie wstępnego lub domena klienta.

1. Uruchom Management Studio lub narzędzia danych i w oknie dialogowym **połączenie z serwerem** (lub **Nawiązywanie połączenia z aparatu bazy danych**) w oknie dialogowym **uwierzytelniania** wybierz **Uwierzytelnianie hasła usługi Active Directory**.
2. W polu **Nazwa użytkownika** wpisz nazwę użytkownika usługi Azure Active Directory w formacie **username@domain.com**. Musi to być konta z usługi Azure Active Directory lub konta z domeny utworzyć Federację z usługi Azure Active Directory.
3. W polu **hasło** wpisz hasło użytkownika dla konta usługi Azure Active Directory lub federacyjnych konta domeny.
    ![Wybierz opcję uwierzytelniania hasła AD][12]

4. Kliknij przycisk **Opcje** , a następnie na stronie **Właściwości połączenia** w oknie dialogowym **połączenie z bazą danych** wpisz nazwę bazy danych, którą chcesz się połączyć. (Zobacz grafiki w poprzedniej opcji).


### <a name="create-an-azure-ad-contained-database-user-in-a-user-database"></a>Utwórz użytkownika bazy danych Azure AD zawarte w bazie danych użytkownika

Aby utworzyć Azure AD zawarte bazy danych użytkownika (innych niż administrator serwera, który jest właścicielem bazy danych), połączenia z bazą danych przy użyciu tożsamości Azure AD jako użytkownik mający co najmniej uprawnienie **ALTER USER dowolny** . Następnie należy użyć następującej składni języka Transact-SQL:

    CREATE USER <Azure_AD_principal_name>
    FROM EXTERNAL PROVIDER;


*Azure_AD_principal_name* może być głównej nazwy użytkownika w usłudze Azure AD użytkownika lub nazwę wyświetlaną dla grupy w usłudze Azure AD.

**Przykłady:** Aby utworzyć bazę danych zawartych użytkownika reprezentującą Azure AD Federacji lub zarządzane użytkownika domeny:

    CREATE USER [bob@contoso.com] FROM EXTERNAL PROVIDER;
    CREATE USER [alice@fabrikam.onmicrosoft.com] FROM EXTERNAL PROVIDER;

Aby utworzyć użytkownika bazy danych zawartych przedstawiający Azure AD lub federacyjnych grupy domeny, wprowadź nazwę wyświetlaną grupę zabezpieczeń:

    CREATE USER [ICU Nurses] FROM EXTERNAL PROVIDER;

Aby utworzyć użytkownika bazy danych zawartych odpowiadającą aplikacji, która łączy przy użyciu tokenu Azure AD:

    CREATE USER [appName] FROM EXTERNAL PROVIDER;

Aby uzyskać więcej informacji o tworzeniu użytkowników bazy danych zawartych w oparciu tożsamości usługi Azure Active Directory zobacz [Tworzenie użytkownika (Transact-SQL)](http://msdn.microsoft.com/library/ms173463.aspx).


> [AZURE.NOTE] Usuwanie administratora usługi Azure Active Directory dla programu SQL Server Azure powoduje, że każdy użytkownik uwierzytelniania Azure AD nawiązywania połączenia z serwerem. Jeśli to konieczne, nie będzie można używać Azure AD użytkowników można usunąć ręcznie przez administratora bazy danych SQL.

Po utworzeniu użytkownika bazy danych tego użytkownika otrzyma uprawnień **POŁĄCZ** i można nawiązać połączenie bazy danych jako członek roli **PUBLIC** . Początkowo uprawnienia tylko dostępne dla użytkownika są uprawnienia do roli **publicznej** lub uprawnienia do każdej grupy systemu Windows są one członkiem. Po postanowień Azure AD oparte na zawarte użytkownika bazy danych, możesz przyznać użytkownika dodatkowe uprawnienia, tak samo jak udzielanie uprawnień do innego typu użytkownika. Zazwyczaj udzielanie uprawnień do ról bazy danych, a następnie dodać użytkowników do roli. Aby uzyskać więcej informacji zobacz [Podstawy uprawnień aparat bazy danych](http://social.technet.microsoft.com/wiki/contents/articles/4433.database-engine-permission-basics.aspx). Aby uzyskać więcej informacji na temat specjalne role bazy danych SQL zobacz [Zarządzanie bazami danych i logowania w bazie danych SQL Azure](sql-database-manage-logins.md).
Użytkownik domeny federacyjnych importowanego do domeny zarządzanie, należy użyć tożsamości zarządzanych domeny.

> [AZURE.NOTE] Azure AD użytkowników są oznaczone w metadanych bazy danych typu E (EXTERNAL_USER) i grup typu X (EXTERNAL_GROUPS). Aby uzyskać więcej informacji zobacz [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx).


## <a name="7-connect-by-using-azure-ad-identities"></a>7. nawiązywanie połączenia przy użyciu tożsamości Azure AD

Azure uwierzytelniania usługi Active Directory obsługuje następujące metody nawiązywania połączenia z bazą danych przy użyciu tożsamości Azure AD:

- Przy użyciu zintegrowanego uwierzytelniania systemu Windows
- Przy użyciu Azure AD głównej nazwy i hasła
- Przy użyciu funkcji uwierzytelniania token aplikacji

### <a name="71-connecting-using-integrated-windows-authentication"></a>7.1. Nawiązywanie połączenia za pomocą zintegrowanego uwierzytelniania (Windows)

Aby użyć zintegrowanego uwierzytelniania systemu Windows, domeny usługi Active Directory musi być Federacji usługi Azure Active Directory. Aplikację klienta (lub usługę) nawiązywanie połączenia z bazą danych musi być zainstalowany na komputerze domeny w obszarze poświadczenia domeny użytkownika.

Nawiązać połączenie z bazą danych przy użyciu zintegrowanego uwierzytelniania i tożsamości Azure AD, słowo kluczowe uwierzytelniania w parametrach połączenia bazy danych musi być równa zintegrowane Active Directory. W poniższym przykładzie kodu C# użyto ADO .NET.

    string ConnectionString =
    @"Data Source=n9lxnyuzhv.database.windows.net; Authentication=Active Directory Integrated; Initial Catalog=testdb;";
    SqlConnection conn = new SqlConnection(ConnectionString);
    conn.Open();

Notatki, że połączenie ciągu słowo kluczowe ``Integrated Security=True`` dla nawiązywanie połączenia z bazą danych SQL Azure nie jest obsługiwane.
Należy zauważyć, że podczas nawiązywania połączenia ODBC należy usunąć spacje i ustawianie uwierzytelniania "ActiveDirectoryIntegrated".

### <a name="72-connecting-with-an-azure-ad-principal-name-and-a-password"></a>7.2. Nawiązywanie połączenia z Azure AD głównej nazwy i hasła
Nawiązać połączenie z bazą danych przy użyciu zintegrowanego uwierzytelniania i tożsamości Azure AD, słowo kluczowe uwierzytelniania musi być równa Active Directory hasło. Parametry połączenia musi zawierać identyfikator użytkownika i UID i hasła-PWD słów kluczowych i wartości. W poniższym przykładzie kodu C# użyto ADO .NET.

    string ConnectionString =
      @"Data Source=n9lxnyuzhv.database.windows.net; Authentication=Active Directory Password; Initial Catalog=testdb;  UID=bob@contoso.onmicrosoft.com; PWD=MyPassWord!";
    SqlConnection conn = new SqlConnection(ConnectionString);
    conn.Open();

Dowiedz się więcej na temat metod uwierzytelniania Azure AD przy użyciu przykłady kodu pokaz dostępna w [Azure AD uwierzytelniania GitHub pokaz](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/security/azure-active-directory-auth).


### <a name="73-connecting-with-an-azure-ad-token"></a>7.3 połączenia z token Azure AD
Ta metoda uwierzytelniania umożliwia średniego poziomu usług nawiązać połączenie z bazą danych SQL Azure ani magazynu danych SQL Azure uzyskując tokenu z Azure Active Directory (AAD). Umożliwia zaawansowanych scenariuszy, w tym na podstawie certyfikatu uwierzytelniania. Należy wykonać cztery podstawowe kroki uwierzytelniania token Azure AD:

1. Zarejestrować aplikację usługi Azure Active Directory i Uzyskaj identyfikator klienta kodu. 
2. Utwórz użytkownika bazy danych odpowiadającą aplikacji. (Wykonane wcześniej w kroku 6).
3. Tworzenie certyfikatu na komputer klienta jest aplikacji.
4. Dodawanie certyfikatu jako klucz aplikacji.

Przykładowe parametry połączenia:

```
string ConnectionString =@"Data Source=n9lxnyuzhv.database.windows.net; Initial Catalog=testdb;"
SqlConnection conn = new SqlConnection(ConnectionString);
connection.AccessToken = "Your JWT token"
conn.Open();
```

Aby uzyskać więcej informacji zobacz [Blog zabezpieczeń programu SQL Server](https://blogs.msdn.microsoft.com/sqlsecurity/2016/02/09/token-based-authentication-support-for-azure-sql-db-using-azure-ad-auth/).

### <a name="connecting-with-sqlcmd"></a>Nawiązywanie połączenia z sqlcmd  
Poniższe instrukcje nawiązywanie połączenia przy użyciu wersji 13.1 sqlcmd, który jest dostępny w [Centrum pobierania](http://go.microsoft.com/fwlink/?LinkID=825643).

```
sqlcmd -S Target_DB_or_DW.testsrv.database.windows.net  -G  
sqlcmd -S Target_DB_or_DW.testsrv.database.windows.net -U bob@contoso.com -P MyAADPassword -G -l 30
```

## <a name="see-also"></a>Zobacz też

[Zarządzanie bazami danych i logowania w bazie danych Azure SQL](sql-database-manage-logins.md)

[Użytkownicy zawartych bazy danych](https://msdn.microsoft.com/library/ff929071.aspx)

[Utwórz użytkownika (Transact-SQL)](http://msdn.microsoft.com/library/ms173463.aspx)


<!--Image references-->

[1]: ./media/sql-database-aad-authentication/1aad-auth-diagram.png
[2]: ./media/sql-database-aad-authentication/2subscription-relationship.png
[3]: ./media/sql-database-aad-authentication/3admin-structure.png
[4]: ./media/sql-database-aad-authentication/4select-subscription.png
[5]: ./media/sql-database-aad-authentication/5ad-settings-portal.png
[6]: ./media/sql-database-aad-authentication/6edit-directory-select.png
[7]: ./media/sql-database-aad-authentication/7edit-directory-confirm.png
[8]: ./media/sql-database-aad-authentication/8choose-ad.png
[9]: ./media/sql-database-aad-authentication/9ad-settings.png
[10]: ./media/sql-database-aad-authentication/10choose-admin.png
[11]: ./media/sql-database-aad-authentication/11connect-using-int-auth.png
[12]: ./media/sql-database-aad-authentication/12connect-using-pw-auth.png
[13]: ./media/sql-database-aad-authentication/13connect-to-db.png

