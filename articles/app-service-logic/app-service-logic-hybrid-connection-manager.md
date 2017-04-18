<properties 
    pageTitle="Korzystanie z Menedżera połączeń hybrydowych | Microsoft Azure" 
    description="Zainstaluj i skonfiguruj hybrydowych Connection Manager oraz nawiązywanie połączenia z lokalnego łączniki w aplikacjach warunków logicznych" 
    services="app-service\logic" 
    documentationCenter=".net,nodejs,java"
    authors="MandiOhlinger" 
    manager="anneta" 
    editor=""/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="mandia"/>

# <a name="connect-to-on-premises-connectors-using-the-hybrid-connection-manager"></a>Nawiązywanie połączenia przy użyciu Menedżera połączeń hybrydowych łączników lokalnego

>[AZURE.NOTE] Tą wersją artykułu dotyczy wersji schematu 2014-12-01-podgląd aplikacji logicznych. Logika aplikacje ogólnodostępną (GA) używa bramy połączenia lokalnego. Dowiedz się więcej o nowych [bramy](app-service-logic-gateway-connection.md) i [Logikę GA aplikacje](https://azure.microsoft.com/documentation/services/logic-apps/).

W systemie lokalnego logikę aplikacji używa hybrydowych Connection Manager. Niektóre łączniki można połączyć się z systemu lokalnego, takich jak programu SQL Server, SAP, programu SharePoint i tak dalej. 

Menedżer połączeń hybrydowych HCM (równa) jest kliknięcie — raz Instalatora, który jest zainstalowany na serwerze usług IIS w sieci za zaporą. Przy użyciu przekazywania Bus usługi Azure, HCM równa uwierzytelnia lokalny system łącznikiem platformy Azure. 

> [AZURE.NOTE] Hybrydowe Connection Manager jest wymagana tylko wtedy, gdy łączysz się zasobów lokalnych za zaporą. Jeśli nie łączysz się z systemu lokalnego, nie potrzebujesz hybrydowych Connection Manager.

Aby rozpocząć pracę, należy następująco:

- Azure Bus usługi przekazywania nazw skojarzenia zabezpieczeń parametry połączenia. Zobacz [Usługa Bus ceny](https://azure.microsoft.com/pricing/details/service-bus/) do określenia, które warstwa zawiera przekaźniki.
- Lokalne systemu logowania informacji, takich jak nazwa użytkownika i hasło. Na przykład jeśli łączysz się do lokalnego programu SQL Server potrzebne konto logowania programu SQL Server i hasło.
- Lokalnego serwera informacji, takich jak nazwa serwera i numer portu. Na przykład jeśli łączysz się do lokalnego programu SQL Server, należy nazwę programu SQL Server i numer portu TCP.

## <a name="get-the-service-bus-connection-string"></a>Pobieranie parametrów połączenia Bus usługi

W portalu Azure skopiuj pierwiastek Bus usługi parametry połączenia skojarzeń zabezpieczeń. Ten ciąg połączenia łączy usługi Azure łącznika na komputerze lokalnym. 

1. W [portalu klasyczny Azure](http://go.microsoft.com/fwlink/p/?LinkID=213885)wybierz obszaru nazw Bus usługi, a następnie wybierz **Informacje o połączeniu**:

    ![][SB_ConnectInfo]

2. Skopiuj parametry połączenia skojarzenia zabezpieczeń:

    ![][SB_SAS]

## <a name="install-the-hybrid-connection-manager"></a>Instalowanie hybrydowych Connection Manager

1. W [portalu Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040)wybierz utworzony łącznik. Aby go otworzyć, wybierz pozycję **Przeglądaj**, wybierz pozycję **Aplikacje interfejsu API**, a następnie wybrać łącznik lub aplikacji interfejsu API. 
<br/><br/>
W obszarze **Połączenia hybrydowego**jest **niekompletny**konfiguracji:
<br/>
![][2] 

2. Wybierz **połączenie hybrydowych**. Parametry połączenia usługi Bus wcześniej wprowadzonych jest wyświetlany.
3. Skopiuj **ciąg konfiguracji podstawowej**:
<br/>
![][PrimaryConfigString]

4. W obszarze **Lokalnego hybrydowych Connection Manager**można pobrać Menedżera połączenia hybrydowego lub zainstalować go bezpośrednio w portalu. 
<br/><br/>
Aby zainstalować bezpośrednio z poziomu portalu, przejdź do lokalnego serwera programu IIS, przejdź do portalu, a następnie wybierz **plik do pobrania i konfigurowanie**.
<br/><br/>
Aby pobrać hybrydowych Connection Manager, przejdź do lokalnego serwera usług IIS i przejdź do **aplikacji ClickOnce** (http://hybridclickonce.azurewebsites.net/install/Microsoft.Azure.BizTalk.Hybrid.ClickOnce.application). Instalacja jest uruchamiany automatycznie, więc można go uruchamiać.

5. W oknie **Ustawienia odbiornika** wprowadź **Ciąg konfiguracji podstawowej** wklejony (w kroku 3), a następnie wybierz pozycję **Zainstaluj**.

Po zakończeniu konfiguracji, wyświetla następujące czynności:
<br/>
![][3] 

Teraz po przejściu do łącznika ponownie, stan połączenia hybrydowego jest **połączony**. Może być konieczne zamknięcie łącznik i ponownym otwarciu dokumentu:
<br/>
![][4] 

> [AZURE.NOTE] Aby przełączyć się parametry połączenia pomocniczym, ponownie uruchom Instalatora hybrydowych połączenie i wprowadź **Ciąg konfiguracji pomocniczą**.


## <a name="tcp-ports-and-security"></a>Porty TCP i zabezpieczeń

Po utworzeniu połączenia hybrydowych witryny sieci Web zostanie utworzona na serwerze usług IIS lokalne lokalnego. Serwer usług IIS może być w strefy Zdemilitaryzowanej. Wymagań dotyczących portów TCP na serwerze usług IIS obejmują:

TCP Port | Dlaczego
--- | ---
 | Porty TCP nie przychodzące są wymagane.
9350 - 9354 | Te porty są używane do transmisji danych. Menedżer przekazywania Bus usługi sondy port 9350 ustalenie, jeśli jest dostępna łączność TCP. Jeśli jest dostępny, następnie przyjęto założenie, że port 9352 jest również dostępne. Ruch omówiono port 9352. <br/><br/>Zezwalanie na połączenia wychodzące do tych portów.
5671 | Użycie portów 9352 ruchu danych port 5671 jest używany jako kanału kontroli. <br/><br/>Zezwalanie na połączenia wychodzące do tego portu. 
porty 80, 443 | Jeśli porty 9352 i 5671 są użyteczne, *następnie* porty 80 i 443 są alternatywnych porty używane do przesyłania danych i kanału kontroli.<br/><br/>Zezwalanie na połączenia wychodzące do tych portów.
Port system lokalna | Na komputerze lokalnym otwórz port używany przez system. Na przykład programu SQL Server zwykle używa portu 1433. Otwórz ten port TCP.

[Hostingu za zaporą z Bus usługi](http://msdn.microsoft.com/library/azure/ee706729.aspx)

## <a name="troubleshooting"></a>Rozwiązywanie problemów

![][HCMFlow]

### <a name="on-premises-troubleshooting"></a>Rozwiązywanie problemów z lokalnego

1. Na serwerze usług IIS Potwierdź ról w sieci web usług IIS jest zainstalowany i wszystkich usług IIS są uruchomione.
2. Na serwerze usług IIS upewnij się, że hybrydowych Connection Manager jest zainstalowana i uruchomiona:
 - W Menedżerze IIS (inetmgr) w witrynie sieci Web ***MicrosoftAzureBizTalkHybridListener*** powinny znajdować się na liście i działać. 
 - Ta witryna sieci Web używa ***HybridListenerAppPool*** naśladujący *sieciowa* lokalnego wbudowane konta użytkownika. Ponadto należy uruchomić tej puli.
3. Na serwerze usług IIS upewnij się, że łącznik jest zainstalowana i uruchomiona: 
 - Witryny sieci Web zostanie utworzona dla tego łącznika. Na przykład jeśli masz utworzony łącznik SQL, istnieje ***MicrosoftSqlConnector_nnn*** witryny sieci Web. W Menedżerze usług IIS (inetmgr), upewnij się, ta witryna sieci Web jest wyświetlane i pracę. 
 - Ta witryna sieci Web używa własnej puli aplikacji usług IIS o nazwie ***HybridAppPoolnnn***. Tej puli działa *Usługa sieciowa* lokalnego wbudowane konta użytkownika. Ta witryna sieci Web i puli oba, należy uruchomić. 
 - Przeglądaj lokalne łącznik. Na przykład jeśli witryny sieci Web łącznika korzysta z portu 6569, przejdź do http://localhost:6569. Dokument domyślny nie jest skonfigurowany tak `HTTP Error 403.14 - Forbidden error` jest planowane.
4. W zaporze upewnij się, są otwarte porty TCP wymienionych w tym temacie.
5. Zapoznaj się z systemem źródłowej lub docelowej:
 - Niektóre systemy lokalnego wymagane pliki dodatkowe zależności. Na przykład łączysz SAP lokalnego, musi być zainstalowany na serwerze usług IIS niektóre dodatkowe pliki SAP.
 - Sprawdź łączność z systemem przy konto logowania. Na przykład port TCP używany przez system musi być otwarty, podobnie jak port 1433 dla programu SQL Server. Konto logowania, wprowadzone w portalu Azure musi mieć dostęp do systemu.
6. Na serwerze usług IIS sprawdź dzienniki zdarzeń występują błędy. 
7. Oczyszczanie i ponownie zainstaluj hybrydowych Connection Manager: 
 - W programie IIS należy ręcznie usunąć łącznik witryny sieci Web i jej puli aplikacji. 
 - Uruchom ponownie Menedżera połączeń hybrydowych i upewnij się, że wprowadzania poprawny **Podstawowy ciąg konfiguracji** dla tego łącznika.



### <a name="in-the-azure-classic-portal"></a>W portalu klasyczny Azure

1. Upewnij się, że przestrzeń nazw Bus usługi ma stan **aktywny** .
2. Po utworzeniu łącznika, wprowadź parametry połączenia skojarzeń zabezpieczeń Bus usługi. Nie wprowadzaj ACS parametry połączenia.


## <a name="faq"></a>FAQ

**Pytanie**: istnieją dwa Menedżerowie połączeń hybrydowych. Jaka jest różnica? 

**Odpowiedzi**: jest technologia [Hybrydowych połączenia](../biztalk-services/integration-hybrid-connection-overview.md) , która jest używana przede wszystkim przez aplikacje sieci Web (dawniej witryn sieci Web) i aplikacje Mobile (dawniej urządzeń przenośnych usługi) nawiązywania połączenia z lokalnym. Ten Menedżer połączeń hybrydowego jest jej własne [Ustawienia](../biztalk-services/integration-hybrid-connection-create-manage.md) i korzysta z usługi Azure BizTalk (w tle). Obsługuje tylko protokoły TCP i HTTP.

Z łącznikami Azure aplikacji usługi mamy są również hybrydowych Connection Manager.  Czy ten hybrydowych Connection Manager *usługi Azure BizTalk usługi (w tle)* i obsługuje więcej niż protokoły TCP i HTTP. Zobacz [łączników i interfejsu API listę aplikacji](app-service-logic-connectors-list.md).

Bus usługi Azure oba służy do łączenia się z lokalnym systemem.

**Pytanie**: podczas tworzenia niestandardowej aplikacji interfejsu API, do czego służy aplikacji usługi hybrydowych Connection Manager nawiązywania połączenia z lokalnym? 

**Odpowiedzi**: nie z pobieraniem do. Możesz za pomocą wbudowanych łączników, konfigurowanie aplikacji usługi hybrydowych Connection Manager do łączenia się z lokalnym systemem. Następnie za pomocą tego łącznika niestandardową aplikację interfejsu API, prawdopodobnie przy użyciu aplikacji logicznych. Obecnie nie można opracowywać lub Utwórz własny hybrydowych aplikacji interfejsu API (na przykład łącznik SQL lub łącznik pliku).

Jeśli swojego niestandardowego interfejsu API korzysta z portu TCP lub HTTP, służy [Hybrydowych połączenia](../biztalk-services/integration-hybrid-connection-overview.md) i jego hybrydowych Connection Manager. W tym scenariuszu jest używana usługa Azure BizTalk. [Nawiązywanie połączenia z lokalnego programu SQL Server za pomocą aplikacji sieci web](../app-service-web/web-sites-hybrid-connection-connect-on-premises-sql-server.md) może pomóc.  


## <a name="read-more"></a>Dowiedz się więcej

[Monitorowanie aplikacji warunków logicznych](app-service-logic-monitor-your-logic-apps.md)<br/>
[Usługi ceny Bus](https://azure.microsoft.com/pricing/details/service-bus/)



[SB_ConnectInfo]: ./media/app-service-logic-hybrid-connection-manager/SB_ConnectInfo.png
[SB_SAS]: ./media/app-service-logic-hybrid-connection-manager/SB_SAS.png
[PrimaryConfigString]: ./media/app-service-logic-hybrid-connection-manager/PrimaryConfigString.png
[HCMFlow]: ./media/app-service-logic-hybrid-connection-manager/HCMFlow.png
[2]: ./media/app-service-logic-hybrid-connection-manager/BrowseSetupIncomplete.jpg
[3]: ./media/app-service-logic-hybrid-connection-manager/HybridSetup.jpg
[4]: ./media/app-service-logic-hybrid-connection-manager/BrowseSetupComplete.jpg

 
