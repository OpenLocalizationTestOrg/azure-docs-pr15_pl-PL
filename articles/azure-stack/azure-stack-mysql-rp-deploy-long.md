<properties
    pageTitle="Wdrażanie dostawcy zasobów MySQL stosu Azure | Microsoft Azure"
    description="Szczegółowe kroki wdrażania dostawcy zasobów MySQL stos Azure."
    services="azure-stack"
    documentationCenter=""
    authors="Dumagar"
    manager="byronr"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="dumagar"/>


# <a name="deploy-the-mysql-resource-provider-on-azure-stack-to-use-with-webapps"></a>Wdrażanie dostawcy zasobów MySQL stosu Azure do użytku z używanie

> [AZURE.NOTE] Poniższe informacje dotyczą tylko wdrożeniach TP1 stos Azure.

Użyj tego artykułu, aby wykonaj uzyskać szczegółowe instrukcje dotyczące konfigurowania dostawcy zasobów MySQL na dowód stos Azure koncepcji tak, aby rozpocząć [Korzystanie z bazy danych MySQL stosu Azure, również za pomocą MySQL jako wewnętrznej bazy danych dla witryn WordPress](azure-stack-mysql-rp-deploy-short.md) utworzone za pomocą [Aplikacji sieci Web Azure](azure-stack-webapps-deploy.md).

## <a name="set-up-steps-before-you-deploy"></a>Procedura konfiguracji przed wdrożeniem

Przed wdrożeniem dostawcy zasobów, należy:

- Miał domyślny obraz systemu Windows Server z .NET 3.5
- Wyłączanie zwiększonych zabezpieczeń programu Internet Explorer (IE)
- Zainstaluj najnowszą wersję programu PowerShell Azure

### <a name="create-an-image-of-windows-server-including-net-35"></a>Tworzenie obrazu systemu Windows Server, w tym program .NET 3.5

Możesz pominąć ten krok, jeśli pobrany bitów stos Azure po 2016-2-23, ponieważ domyślne Windows Server 2012 R2 obrazu zawiera framework .NET 3.5 w tym artykule pobieranie lub w nowszej wersji.

Jeśli pobrano przed 2016-2-23, należy utworzyć systemu Windows Server 2012 R2 centrum danych wirtualnego dysku twardego z obrazem .NET 3.5 i zestaw jest jako domyślnego obrazu w repozytorium obraz platformy.

### <a name="turn-off-ie-enhanced-security-and-enable-cookies"></a>Wyłącz IE rozszerzony zabezpieczeń i Włącz pliki cookie

Aby wdrożyć dostawcy zasobów, należy uruchomić środowisko zintegrowane skryptów w PowerShell (ISE) jako administrator, więc należy zezwolić pliki cookie i JavaScript w profilu programu Internet Explorer, którego używasz, aby zalogować się do usługi Azure Active Directory dla zarówno administratorów i użytkowników dodatki logowania.

**Aby wyłączyć IE enhanced zabezpieczeń:**

1. Zaloguj się do komputera (aby zapewnić)-koncepcji stos Azure jako AzureStack-administrator, a następnie otwórz Menedżera serwera.

2. Wyłącz funkcję **Konfiguracja zwiększonych zabezpieczeń programu Internet Explorer** , zarówno dla administratorów i użytkowników.

3. Zaloguj się do maszyny wirtualnej **ClientVM.AzureStack.local** jako administrator, a następnie otwórz Menedżera serwera.

4. Wyłącz funkcję **Konfiguracja zwiększonych zabezpieczeń programu Internet Explorer** , zarówno dla administratorów i użytkowników.

**Aby włączyć pliki cookie:**

1. Na ekranie startowym systemu Windows kliknij polecenie **wszystkie aplikacje**, kliknij pozycję **Akcesoria systemu Windows**, kliknij prawym przyciskiem myszy **Program Internet Explorer**, wskaż pozycję **więcej**, a następnie wybierz polecenie **Uruchom jako administrator**.

2. Jeśli zostanie wyświetlony monit, sprawdź **zalecane zabezpieczeń**, a następnie kliknij **przycisk OK**.

3. W programie Internet Explorer kliknij pozycję **Narzędzia (ikona koła zębatego) ikona** &gt; **Opcje internetowe** &gt; kartę **Prywatność** .

4. Kliknij przycisk **Zaawansowane**, upewnij się, że wybierane są przyciski **Zaakceptuj** , kliknij **przycisk OK**i ponownie kliknij przycisk **OK** .

5. Zamknij program Internet Explorer i uruchom ponownie środowiska PowerShell ISE jako administrator.

### <a name="install-an-azure-stack-compatible-release-of-azure-powershell"></a>Zainstalować wersję zgodne stos Azure Azure programu PowerShell

1. Odinstaluj wszelkie istniejące Azure programu PowerShell z maszyn wirtualnych usługi klienta.

2. Zaloguj się do komputera Zapewnić stos Azure jako AzureStack-administrator.

3. Za pomocą pulpitu zdalnego, zaloguj się do maszyny wirtualnej **ClientVM.AzureStack.local** jako administrator.

4. Otwórz Panel sterowania, kliknij pozycję **Odinstaluj program** &gt; kliknij **Azure programu PowerShell** &gt; kliknij polecenie **Odinstaluj**.

5. [Pobierz najnowszą PowerShell Azure, który obsługuje stos Azure](http://aka.ms/azstackpsh) i zainstaluj go.

    Po zainstalowaniu programu PowerShell, może zostać uruchomiony weryfikacji skrypt programu PowerShell, aby upewnić się, że możesz połączyć wystąpienia stos Azure (strona sieci web logowania powinien być wyświetlany).

## <a name="bootstrap-the-resource-provider-deployment-powershell"></a>Początkowego wdrażania dostawcy zasobów programu PowerShell

1. Nawiązywanie połączenia clientVm.AzureStack.Local pulpitu zdalnego Zapewnić stos Azure i zaloguj się jako azurestack\\azurestackuser.

2. [Pobieranie plików binarnych MySQL RP](http://aka.ms/masmysqlrp) plik i wyodrębnić je do D:\\MySQLRP.

3. Uruchamianie D:\\MySQLRP\\Bootstrap.cmd pliku jako administrator (azurestack\administrator).

    Spowoduje to otwarcie pliku Bootstrap.ps1 środowiska PowerShell ISE.

4. Po zakończeniu ładowania systemu windows PowerShell ISE kliknij przycisk "Odtwórz", lub naciśnij klawisz F5.

    Dwie główne karty pobierze, zawierające wszystkie skrypty i pliki należy wdrożyć dostawcy zasobów MySQL.

## <a name="prepare-prerequisites"></a>Przygotowywanie wymagania wstępne

Kliknij kartę **Wymagania wstępne przygotowywanie** do:

- Tworzenie certyfikatów wymagane
- Pobieranie plików binarnych MySQL do stosu Azure
- Przekazywanie artefaktów na koncie miejsca do magazynowania w stos Azure
- Publikowanie Galeria elementów

### <a name="create-the-required-certificates"></a>Tworzenie certyfikatów wymagane
Dodaje ten skrypt **Nowy SslCert.ps1** \_. Certyfikat AzureStack.local.pfx SSL D:\\MySQLRP\\wymagania wstępne dotyczące\\BlobStorage\\folder kontener. Certyfikat zabezpiecza komunikacji między dostawcy zasobów i lokalne wystąpienie programu Menedżer zasobów Azure.

1. Na karcie głównych **Przygotowywanie wymagania wstępne** kliknij kartę **SslCert.ps1 nowy** , a następnie uruchom go.

2. W wierszu, które zostanie wyświetlone wpisz hasło PFX chroniącego klucz prywatny i **zanotuj to hasło**. Musisz go później.

### <a name="download-mysql-binaries-to-your-azure-stack"></a>Pobieranie plików binarnych MySQL do stosu Azure

1. Wybierz kartę **MySqlServer.ps1 pobierania** , a następnie uruchom go.
2. Po wyświetleniu monitu kliknij przycisk **Tak** w oknie dialogowym Potwierdzanie Zaakceptuj Umowę licencyjną.

    To polecenie dodaje dwa pliki zip do folderu D:\MySql\Prerequisites\BlobStorage\Container.

### <a name="upload-all-artifacts-to-a-storage-account-on-azure-stack"></a>Przekazywanie wszystkich artefaktów na koncie miejsca do magazynowania w stos Azure

1. Kliknij kartę **Przekazywania-Microsoft.MySql-RP.ps1** i uruchom go.

2. W oknie dialogowym żądanie poświadczeń programu Windows PowerShell wpisz poświadczenia administratora usługi Azure stosu.

3. Po wyświetleniu monitu dla Azure Active Directory ID dzierżawy, wpisz nazwę domeny w pełni kwalifikowana dzierżawy usługi Azure Active Directory: na przykład microsoftazurestack.onmicrosoft.com.

    Okno podręczne zapytanie dotyczące poświadczeń.

    > [AZURE.TIP] Jeśli nie są wyświetlane w oknie podręcznym, które albo nie zostały wyłączone IE udoskonalone zabezpieczenia, aby włączyć język JavaScript w tym komputera i użytkownika lub nie zostały odebrane pliki cookie w programie Internet Explorer. Zobacz [Konfigurowanie kroki przed wdrożeniem](#set-up-steps-before-you-deploy).

4. Wpisz swoje poświadczenia administratora usługi stos Azure, a następnie kliknij przycisk **Zaloguj**.

### <a name="publish-gallery-items-for-later-resource-creation"></a>Publikowanie elementów galerii później tworzenie zasobów

Wybierz kartę **GalleryPackages.ps1 Publikuj** , a następnie uruchom go. Ten skrypt dodawane są dwa elementy marketplace do witryny marketplace portal Azure stos Zapewnić, używanego do wdrożenia zasoby bazy danych jako elementów marketplace.

## <a name="deploy-the-mysql-resource-provider-vm"></a>Wdrażanie dostawcy zasobów MySQL maszyn wirtualnych

Teraz, gdy już przygotowane zapewnić stos Azure z wymagane certyfikaty i elementami marketplace, należy wdrożyć dostawca zasobów programu SQL Server. Kliknij kartę **dostawcy wdrażanie MySQL** , aby:

   - Podaj wartości w pliku JSON, który odwołuje się do procesu wdrażania
   - Wdrażanie dostawcy zasobów
   - Aktualizacja DNS lokalne
   - Zarejestruj się karty dostawcy zasobów serwera SQL

### <a name="provide-values-in-the-json-file"></a>Podaj wartości w pliku JSON

Kliknij pozycję **Microsoft.MySqlprovider.Parameters.JSON**. Ten plik ma parametry, które szablonu Menedżera zasobów Azure musi poprawnie wdrażanie stos Azure.

1. Wypełnij **puste** parametry w pliku JSON:

    - Upewnij się, że podasz **adminusername** i **adminpassword** maszyn wirtualnych dostawcy zasobów MySQL.

    - Upewnij się, że hasło dla parametru **SetupPfxPassword** , które zostały wprowadzone w kroku [prequisites Przygotuj](#prepare-prerequisites) Zapisz.

    - Upewnij się, że podane parametry **basicAuthUserName** i **basicAuthPassword** . **Zanotuj te wartości.** Trzeba je później, aby zarejestrować dostawcy zasobów.

2. Kliknij przycisk **Zapisz**.

### <a name="deploy-the-resource-provider"></a>Wdrażanie dostawcy zasobów

1. Kliknij kartę **Rozmieszczanie-Microsoft.Mysql-provider.PS1** i uruchom skrypt.
2. Wpisz nazwę dzierżawy w usługi Azure Active Directory po wyświetleniu monitu.
3. W oknie podręcznym przesyłanie poświadczeń administratora usługi Azure stosu.

Pełne wdrożenie może potrwać od 15 do 45 minut na niektórych bardzo wykorzystywane POCs stos Azure.

### <a name="update-the-local-dns"></a>Aktualizacja DNS lokalne

1. Kliknij kartę **Register-Microsoft.MySQL-fqdn.ps1** i uruchom skrypt.
2. Po wyświetleniu monitu Azure Active Directory dzierżawy ID wprowadzania nazwy FQDN dzierżawy usługi Azure Active Directory: na przykład **microsoftazurestack.onmicrosoft.com**.

### <a name="register-the-sql-rp-resource-provider"></a>Rejestr dostawcy zasobów SQL RP

1. Kliknij kartę **Register-Microsoft.My-provider.ps1** i uruchom skrypt.

2. Gdy zostanie wyświetlony monit o poświadczenia, za pomocą zapisany jako parametrów **basicAuthUserName** i **basicAuthPassword** .

## <a name="verify-the-deployment-using-the-azure-stack-portal"></a>Sprawdź wdrażania za pomocą portalu stos Azure

1. Wylogowywanie się z ClientVM i zaloguj się ponownie jako **AzureStack\User**.

2. Na pulpicie kliknij **Portal Zapewnić stos Azure** i zaloguj się do portalu jako administrator usługi.

3. Upewnij się, że rozmieszczenia zakończyła się pomyślnie. Kliknij przycisk **Przeglądaj,** &gt; **Grup zasobów**, kliknij pozycję Grupa zasobów użyto (wartość domyślna to **MySQLRP**), a następnie upewnij się, że podstawowe części karta (górna połowa) odczytuje **wdrożenia zakończyła się pomyślnie**.


4. Upewnij się, że rejestracji zakończyła się pomyślnie. Kliknij przycisk **Przeglądaj,** &gt; **dostawców zasobów**, a następnie odszukaj **Lokalny MySQL**.

## <a name="create-your-first-mysql-database-to-test-your-deployment"></a>Tworzenie pierwszej bazy danych MySQL w celu przetestowania rozmieszczenia

1. Zaloguj się do portalu Zapewnić stos Azure jako administrator usługi.

2. Kliknij pozycję **+** przycisk &gt; **Niestandardowy** &gt; **serwer MySQL & bazy danych**.

3. Wypełnij formularz Szczegóły bazy danych.

    **Zanotuj "wprowadzoną nazwę serwera".** Parametry połączenia dla bazy danych zawiera "Nazwa serwera" jako część nazwy użytkownika: na przykład ** "user@ <ServerName>"**. Należy wprowadzić nazwy użytkownika w tym formacie podczas nawiązywania połączenia z bazą danych: na przykład przy wdrażaniu MySQL witryny za pomocą dostawcy zasobów Azure witryny sieci Web


## <a name="next-steps"></a>Następne kroki

Wypróbuj inne [PaaS usługi](azure-stack-tools-paas-services.md) , takie jak [dostawcy zasobów programu SQL Server](azure-stack-sql-rp-deploy-short.md) i [dostawcy zasobów aplikacji sieci Web](azure-stack-webapps-deploy.md).
