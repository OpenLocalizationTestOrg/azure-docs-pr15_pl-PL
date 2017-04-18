<properties
    pageTitle="Nawiązywanie połączenia z bazą danych SQL przy użyciu programu SQL Server Management Studio w Azure RemoteApp | Microsoft Azure"
    description="Użyj tego samouczka, aby dowiedzieć się, jak używać programu SQL Server Management Studio w Azure RemoteApp dotyczące zabezpieczeń i wydajności podczas nawiązywania połączenia z bazą danych SQL"
    services="sql-database"
    documentationCenter=""
    authors="adhurwit"
    manager="jhubbard"/>

<tags
    ms.service="sql-database"
    ms.workload="data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/05/2016"
    ms.author="adhurwit"/>

# <a name="use-sql-server-management-studio-in-azure-remoteapp-to-connect-to-sql-database"></a>Nawiązywanie połączenia z bazą danych SQL za pomocą programu SQL Server Management Studio w Azure RemoteApp

## <a name="introduction"></a>Wprowadzenie  
Tym samouczku pokazano, jak używać programu SQL Server Management Studio (SSMS) w Azure RemoteApp nawiązywania połączenia z bazą danych SQL. Go przeprowadzi Cię przez proces konfigurowania programu SQL Server Management Studio w Azure RemoteApp, opis korzyści i przedstawiono funkcje zabezpieczeń, które są dostępne w usłudze Azure Active Directory.

**Szacowany czas:** 45 minut

## <a name="ssms-in-azure-remoteapp"></a>SSMS w Azure RemoteApp

Funkcja RemoteApp Azure to usługa licencji platformy Azure, który zawiera aplikacje. Więcej informacji o nim tutaj: [Co to jest funkcja RemoteApp?](../remoteapp/remoteapp-whatis.md)

SSMS działa Azure RemoteApp umożliwia odbiór pliku w postaci uruchomionej SSMS na komputerze lokalnym.

![Zrzut ekranu przedstawiający SSMS działa funkcja RemoteApp platformy Azure][1]



## <a name="benefits"></a>Zalety

Istnieje wiele korzyści wynikające z używania SSMS w Azure RemoteApp, w tym:

- Port 1433 na serwerze SQL Azure ma nastąpić zewnętrznie (poza Azure).
- Bez konieczności zachować Dodawanie i usuwanie adresów IP w Zaporze Windows Azure SQL Server.
- Wszystkie połączenia Azure RemoteApp odbywa się za pomocą protokołu HTTPS na na porcie 443 przy użyciu szyfrowane protokołu pulpitu zdalnego
- Jest wielu użytkowników i można skalować.
- Występuje przyrost wydajności uzyskanie SSMS w tym samym regionie jako bazy danych SQL.
- Czy inspekcja użycia Azure RemoteApp z usługi Azure Active Directory, która ma raporty aktywności użytkownika w wersji Premium edition.
- Możesz włączyć uwierzytelnianie wieloskładnikowe (MFA).
- Dostęp SSMS dowolne miejsce w przypadku obsługiwanych klientów Azure RemoteApp, obejmujące iOS, Android, Mac, Windows Phone i komputera systemu Windows.


## <a name="create-the-azure-remoteapp-collection"></a>Tworzenie zbioru Azure RemoteApp

Poniżej przedstawiono procedurę tworzenia kolekcji Azure RemoteApp z SSMS:


### <a name="1-create-a-new-windows-vm-from-image"></a>1. Tworzenie nowych maszyn wirtualnych systemu Windows z obrazu
Używanie obrazu "Windows Server zdalnego pulpitu sesji hosta Windows Server 2012 R2" z galerii nawiązać z nowych maszyn wirtualnych.


### <a name="2-install-ssms-from-sql-express"></a>2. Zainstaluj SSMS z SQL Express

Przejdź do nowej maszyn wirtualnych i przejdź do strony ten plik do pobrania: [Microsoft® SQL Server® 2014 Express](https://www.microsoft.com/en-us/download/details.aspx?id=42299)

Istnieje możliwość pobierać tylko SSMS. Po pobraniu przejdź do katalogu Zainstaluj i uruchom Instalatora, aby zainstalować SSMS.

Należy zainstalować dodatek Service Pack 1 dla programu SQL Server 2014 r. Możesz pobrać ją tutaj: [dodatek Service Pack 1 (SP1) programu Microsoft SQL Server 2014 r.](https://www.microsoft.com/en-us/download/details.aspx?id=46694)

Dodatek Service Pack 1 dla programu SQL Server 2014 zawiera podstawowe funkcje do pracy z bazą danych SQL Azure.


### <a name="3-run-validate-script-and-sysprep"></a>3. Uruchom skrypt sprawdzania poprawności oraz Sysprep

Na pulpicie maszyn wirtualnych skrypt programu PowerShell nosi nazwę sprawdzania poprawności. Uruchamiać, klikając dwukrotnie. Zweryfikuje, że maszyn wirtualnych jest gotowa do użycia dla zdalnego hosting aplikacji. Po zakończeniu weryfikacji zostanie wyświetlone pytanie do narzędzia Sysprep — wybierz pozycję, aby go uruchomić.

Po zakończeniu działania narzędzia sysprep wyłączy maszyn wirtualnych.

Aby dowiedzieć się więcej na temat tworzenia obrazu Azure RemoteApp, zobacz: [jak utworzyć obraz szablonu RemoteApp platformy Azure](http://blogs.msdn.com/b/rds/archive/2015/03/17/how-to-create-a-remoteapp-template-image-in-azure.aspx)


### <a name="4-capture-image"></a>4. przechwycić obraz

Po maszyn wirtualnych został zatrzymany, znajdź ją w bieżącym portalu i przechwycona.

Aby dowiedzieć się więcej na temat Przechwytywanie obrazu, zobacz [przechwycić obraz maszyn wirtualnych systemu Windows Azure utworzone za pomocą modelu Klasyczny wdrażania](../virtual-machines/virtual-machines-windows-classic-capture-image.md)


### <a name="5-add-to-azure-remoteapp-template-images"></a>5. Dodawanie do obrazów szablonów RemoteApp Azure

W sekcji Azure RemoteApp portalu bieżącego przejdź na kartę obrazów szablonów, a następnie kliknij przycisk Dodaj. W oknie podręcznym wybierz pozycję "Importowanie obrazu z biblioteki maszyn wirtualnych", a następnie wybierz obraz, która została właśnie utworzona.



### <a name="6-create-cloud-collection"></a>6. utworzyć zbiór chmury

W portalu bieżącego Utwórz nowy zbiór chmury RemoteApp Azure. Wybierz obraz szablonu zostały zaimportowane z zainstalowanym SSMS.

![Tworzenie nowej kolekcji chmury][2]


### <a name="7-publish-ssms"></a>7. SSMS publikowanie

Na publikowanie nowej kolekcji chmury, wybierz na karcie publikowanie aplikacji w Start Menu i wybierz pozycję SSMS z listy.

![Publikowanie aplikacji][5]

### <a name="8-add-users"></a>8. Dodawanie użytkowników

Na karcie dostępu użytkowników można wybrać użytkowników, którzy będą mieli dostęp do tej kolekcji Azure RemoteApp, który zawiera tylko SSMS.

![Dodawanie użytkownika][6]


### <a name="9-install-the-azure-remoteapp-client-application"></a>9. Zainstaluj z aplikacją kliencką Azure RemoteApp

Możesz pobrać i zainstalować klienta Azure RemoteApp tutaj: [Pobierz | Funkcja RemoteApp Azure](https://www.remoteapp.windowsazure.com/en/clients.aspx)



## <a name="configure-azure-sql-server"></a>Konfigurowanie serwera Azure SQL

Konfiguracja tylko potrzebne jest zapewnienie, że usługi Azure jest włączone dla zapory. Jeśli używasz tego rozwiązania, nie musisz dodać adresy IP, aby otworzyć zapory. Ruch sieciowy, które mają dostęp do programu SQL Server jest z innych usług Azure.


![Zezwalaj na Azure][4]



## <a name="multi-factor-authentication-mfa"></a>Uwierzytelnianie wieloskładnikowe (MFA)

MFA można włączyć dla tej aplikacji, w szczególności. Przejdź na kartę aplikacji usługi Azure Active Directory. Microsoft Azure RemoteApp znajdują się wpisu. Jeśli klikniesz tej aplikacji, a następnie skonfiguruj, zostanie wyświetlona strona poniżej miejsce, w którym można włączyć MFA dla tej aplikacji.

![Włączanie MFA][3]



## <a name="audit-user-activity-with-azure-active-directory-premium"></a>Przeprowadzanie inspekcji aktywności użytkownika z Azure Active Directory Premium

Jeśli nie masz Azure AD Premium, następnie należy włączyć w sekcji licencje katalogu. Z Premium włączone można przypisać użytkowników do poziomu Premium.

Po przejściu do użytkownika w usłudze Active Directory platformy Azure możesz następnie przejdź do karty aktywności, aby wyświetlić informacje o logowaniu do Azure RemoteApp.



## <a name="next-steps"></a>Następne kroki

Po wykonaniu powyższych czynności, będzie można uruchomić klienta Azure RemoteApp i zaloguj się przy użyciu przypisany użytkownik. Zostanie wyświetlona z SSMS jako jedna z aplikacji, a można go uruchamiać, jak w przypadku, jeśli zostały zainstalowane na komputerze z dostępu do serwera SQL Azure.

Aby uzyskać więcej informacji na temat do nawiązania połączenia z bazą danych SQL, zobacz [Nawiązywanie połączenia z bazą danych SQL z programu SQL Server Management Studio i wykonać przykładowa kwerenda T-SQL](sql-database-connect-query-ssms.md).


To wszystko jest teraz. Ciesz się!



<!--Image references-->
[1]: ./media/sql-database-ssms-remoteapp/ssms.png
[2]: ./media/sql-database-ssms-remoteapp/newcloudcollection.png
[3]: ./media/sql-database-ssms-remoteapp/mfa.png
[4]: ./media/sql-database-ssms-remoteapp/allowazure.png
[5]: ./media/sql-database-ssms-remoteapp/publish.png
[6]: ./media/sql-database-ssms-remoteapp/user.png
