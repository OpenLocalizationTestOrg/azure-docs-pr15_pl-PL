<properties
   pageTitle="Informacje o wersji Azure wykaz danych | Microsoft Azure"
   description="Informacje o wersji dla wykazu danych Azure."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="09/21/2016"
   ms.author="maroche"/>

# <a name="azure-data-catalog-release-notes"></a>Wykaz danych Azure informacje o wersji

## <a name="notes-for-the-november-20-2015-release-of-azure-data-catalog"></a>Zwolnij notatek do 20 listopada 2015 wykazu danych Azure

### <a name="opening-data-sources-in-power-bi-desktop"></a>Otwieranie źródła danych w Power BI Desktop

Podczas korzystania z opcji "Otwórz w Power BI Desktop" z portalu **Azure wykazu danych** , użytkownicy mogą wystąpić jeden z dwóch problemów w aplikacji Power BI Desktop:

- Zostanie wyświetlone okno dialogowe z tytułem "Nie można do otwartego dokumentu"
- Zostanie otwarty w aplikacji Power BI Desktop, ale plik może być puste

W różnych sytuacjach ten problem można rozwiązać przez pobranie i zainstalowanie najnowszej wersji programu Power BI Desktop z [PowerBI.com](https://powerbi.com).

## <a name="notes-for-the-november-13-2015-release-of-azure-data-catalog"></a>Zwolnij notatek do 13 listopada 2015 wykazu danych Azure

### <a name="registering-and-connecting-to-teradata"></a>Rejestrowanie i nawiązywanie połączeń z Teradata

Podczas łączenia się użytkowników źródeł danych programu Teradata musi być zainstalowany odpowiedni sterownik Teradata ODBC, która odpowiada liczbie bitów (32-bitowej lub 64-bitowa) oprogramowania.

Tej daty ADC wersji, to najnowszy [sterownik Teradata ODBC dla systemu windows (wersja 15.10)](http://downloads.teradata.com/download/connectivity/odbc-driver/windows) jest zgodny z pakietem Office 2013, ale nie w pakiecie Office 2016.

## <a name="notes-for-the-july-13-2015-release-of-azure-data-catalog"></a>Zwolnij notatek do 13 lipca 2015 wykazu danych Azure

### <a name="registering-and-connecting-to-oracle-database"></a>Rejestrowanie i nawiązywanie połączenia z bazą danych Oracle

Podczas nawiązywania połączenia ze źródłami danych bazy danych programu Oracle użytkownicy mają zainstalowany poprawne sterowniki programu Oracle, które odpowiada liczbie bitów (32-bitowej lub 64-bitowa) oprogramowania.

-   Podczas rejestrowania źródła danych programu Oracle na komputerze z systemem Windows w wersji 32-bitowej, będzie używana 32-bitowych sterowników programu Oracle
-   Podczas rejestrowania źródła danych programu Oracle na komputerze z 64-bitowej wersji systemu Windows, będzie używana 64-bitowych sterowników programu Oracle
-   Podczas nawiązywania połączenia ze źródłami danych programu Oracle za pomocą programu Excel na komputerze z systemem 32-bitowej wersji pakietu Microsoft Office, łącznie z 64-bitowego systemu Windows, 32-bitowych sterowników Oracle będą używane
-   Podczas nawiązywania połączenia ze źródłami danych programu Oracle za pomocą programu Excel na komputerze z 64-bitowej wersji pakietu Microsoft Office, będzie używana 64-bitowych sterowników programu Oracle

### <a name="registering-and-connecting-to-sql-server-reporting-services"></a>Rejestrowanie i nawiązywanie połączeń z usług SQL Server Reporting Services

Obsługa źródeł danych programu SQL Server Reporting Services (SSRS) jest obecnie dostępna jedynie w trybie natywnym serwerów. Obsługa SSRS w trybie programu SharePoint zostanie dodany w nowszej wersji.

### <a name="opening-data-assets-in-excel"></a>Otwieranie elementów danych w programie Excel

Podczas otwierania aktywów danych w programie Microsoft Excel z poziomu portalu **Azure wykazu danych** , użytkowników może zostać wyświetlony monit z oknem dialogowym **Hasło zabezpieczeń programu Microsoft Excel** . Jest to standardowy, oczekiwane zachowanie i użytkowników można wybrać **włączyć** , aby kontynuować.

Aby uzyskać więcej informacji zobacz [Włączanie lub wyłączanie alertów zabezpieczeń dotyczących łączy oraz plików pochodzących z podejrzanych witryn sieci Web](https://support.office.com/article/Enable-or-disable-security-alerts-about-links-and-files-from-suspicious-websites-A1AC6AE9-5C4A-4EB3-B3F8-143336039BBE).

### <a name="proxy-and-policy-configuration-and-data-source-registration"></a>Serwer proxy i zasad konfiguracji i danych rejestracji źródła

Użytkownicy mogą wystąpić sytuacja miejsce, w którym można zalogować się do portalu Azure wykazu danych, ale przy próbie Zaloguj się do narzędzia rejestracji źródła danych przez nich problemach komunikat o błędzie, który uniemożliwia logowania.

Istnieją dwa potencjalne przyczyny tego zachowania problemu:

**Przyczyny 1: Active Directory Federation Services konfigurację** Narzędzie do rejestracji źródła danych użyto uwierzytelniania formularzy, aby sprawdzić poprawność logowania użytkowników w usłudze Active Directory. Pomyślne logowanie uwierzytelniania formularzy musi być włączone w globalnej zasady uwierzytelniania przez administratora usługi Active Directory.

W niektórych sytuacjach zachowanie ten błąd może wystąpić tylko wtedy, gdy użytkownik znajduje się w sieci firmowej lub tylko wtedy, gdy użytkownik nawiązuje połączenie z spoza sieci firmowej. Globalne zasady uwierzytelniania umożliwia metody uwierzytelniania należy włączyć osobno dla sieci intranet i ekstranetu połączenia. Błędy logowania może wystąpić, jeśli nie włączono uwierzytelnianie formularzy dla sieci, z której użytkownik nawiązuje połączenie.

Aby uzyskać więcej informacji zobacz [Konfigurowanie zasad uwierzytelniania](https://technet.microsoft.com/library/dn486781.aspx).

**2 Przyczyna: konfiguracji serwera proxy sieci** Jeśli sieci firmowej używa serwera proxy, narzędzie do rejestracji nie można nawiązać połączenia z usługą Azure Active Directory za pośrednictwem serwera proxy. Użytkownicy mogą upewnij się, że narzędzie do rejestracji, edytując plik konfiguracji narzędzia, dodanie do pliku w tej sekcji:


      <system.net>
        <defaultProxy useDefaultCredentials="true" enabled="true">
          <proxy usesystemdefault="True"
                          proxyaddress="http://<your corporate network proxy url>"
                          bypassonlocal="True"/>
        </defaultProxy>
      </system.net>


Aby zlokalizować plik RegistrationTool.exe.config, uruchom narzędzie do rejestracji, a następnie otwórz narzędzie Menedżera zadań systemu Windows. Na karcie Szczegóły w Menedżerze zadań kliknij prawym przyciskiem myszy RegistrationTool.exe i wybierz polecenie Otwórz lokalizację pliku z menu podręcznego.
