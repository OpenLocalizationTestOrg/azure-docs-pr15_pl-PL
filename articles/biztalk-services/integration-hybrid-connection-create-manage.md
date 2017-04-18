<properties 
    pageTitle="Tworzenie i zarządzanie połączeniami hybrydowych | Microsoft Azure" 
    description="Dowiedz się, jak utworzyć połączenie hybrydowe, połączenie, instalowanie i zarządzanie nimi hybrydowych Connection Manager. MABS, WABS" 
    services="biztalk-services" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="biztalk-services" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="ccompy"/>


# <a name="create-and-manage-hybrid-connections"></a>Tworzenie i zarządzanie połączeniami hybrydowego


## <a name="overview-of-the-steps"></a>Omówienie czynności
1. Tworzenie połączenia funkcji hybrydowych wprowadzając **Nazwa hosta** lub **nazwy FQDN** zasobów lokalnych w sieci prywatnej.
2. Łącze do hybrydowego połączenia z aplikacjami Azure web lub Azure aplikacji dla urządzeń przenośnych.
3. Instalowanie hybrydowych Connection Manager na zasób lokalnego i Połącz z określonego połączenia hybrydowych. Azure portal zapewnia jednym kliknięciem zainstalować i połączyć.
4. Zarządzanie połączeniami hybrydowych i klucze połączenia.

W tym temacie opisano następujące kroki. 

> [AZURE.IMPORTANT] Istnieje możliwość Ustaw punkt końcowy hybrydowych połączenia z adresem IP. Jeśli używasz adresu IP, możesz lub nie może osiągnąć zasobów lokalnych w zależności od klienta. Połączenie hybrydowych zależy od klienta, wykonując wyszukiwanie DNS. W większości przypadków __klienta__ jest kodzie aplikacji. Jeśli klient wykonuje wyszukiwanie DNS (go nie próbuje rozpoznać adresu IP, tak jakby była nazwę domeny (x.x.x.x)), a następnie ruch nie są wysyłane za pośrednictwem połączenia hybrydowych.
>
> Na przykład (pseudocode) **10.4.5.6** zdefiniowanych jako hosta lokalnego:
> 
> **Sprawdza się następującym scenariuszu:**  
> `Application code -> GetHostByName("10.4.5.6") -> Resolves to 127.0.0.3 -> Connect("127.0.0.3") -> Hybrid Connection -> on-prem host`
> 
> **Następującym scenariuszu nie działa:**  
> `Application code -> Connect("10.4.5.6") -> ?? -> No route to host`


## <a name="CreateHybridConnection"></a>Tworzenie połączenia funkcji hybrydowych

Połączenie hybrydowych mogą być tworzone w portalu Azure za pomocą aplikacji sieci Web **lub** korzystanie z usług BizTalk. 

**Tworzenie połączeń hybrydowych przy użyciu aplikacji sieci Web**, zobacz [Łączenie aplikacji sieci Web Azure do zasobu w środowisku lokalnym](../app-service-web/web-sites-hybrid-connection-get-started.md). Można również zainstalować Menedżera połączenia hybrydowych HCM (równa) w aplikacji sieci web jest preferowana metoda. 

**Do tworzenia połączeń hybrydowych w usługach BizTalk**:

1. Zaloguj się do [portalu klasyczny Azure](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. W okienku nawigacji po lewej stronie wybierz pozycję **Usługi BizTalk** , a następnie wybierz pozycję usługi BizTalk. 

    Jeśli nie masz Istniejąca usługa BizTalk, możesz [Tworzenie usługi BizTalk](biztalk-provision-services.md).
3. Wybierz kartę **Hybrydowych połączenia** :  
![Karta połączenia hybrydowego][HybridConnectionTab]

4. Wybierz opcję **Utwórz połączenie hybrydowe** , lub kliknij przycisk **Dodaj** na pasku zadań. Wprowadź następujące informacje:

    Właściwość | Opis
--- | ---
Nazwa | Nazwa połączenia hybrydowych musi być unikatowa i nie może mieć taką samą nazwę jak usługa BizTalk. Można wprowadzić dowolną nazwę, ale zależeć od jej cel. Przykłady:<br/><br/>Listy płac*SQL*<br/>SupplyList*SharepointServer*<br/>Klienci*OracleServer*
Nazwa hosta | Wprowadź nazwę hosta w pełni kwalifikowaną nazwę hosta lub adres IP protokołu IPv4 zasobów lokalnych. Przykłady:<br/><br/>mySQLServer<br/>*mySQLServer*. *Domena*. .com*twojafirma*corp.<br/>*myHTTPSharePointServer*<br/>*myHTTPSharePointServer*. .com *twojafirma*<br/>10.100.10.10<br/><br/>Jeśli używasz adresu IP protokołu IPv4 Uwaga kodzie klienta lub aplikacji może nie rozwiązać adres IP. Zobacz ważne Uwaga w górnej części tego tematu.
Port | Wprowadź numer portu zasobów lokalnych. Na przykład jeśli korzystasz z aplikacji sieci Web, wprowadź portu 80 lub 443. Jeśli korzystasz z programu SQL Server, wpisz port 1433.

5. Zaznacz pole wyboru, aby zakończyć konfigurację. 

#### <a name="additional"></a>Dodatkowe

- Można utworzyć wiele połączeń hybrydowych. Zobacz [usług BizTalk: wykres wersje](biztalk-editions-feature-chart.md) dla liczby połączeń. 
- Każdego połączenia hybrydowego jest tworzone parę parametry połączenia: aplikacja klucze tego klucze WYSYŁANIE i lokalnych, które ODSŁUCHAĆ. Każdej pary ma podstawowy i klucz pomocniczą. 


## <a name="LinkWebSite"></a>Łącze do usługi Azure aplikacji Web App lub aplikacji dla urządzeń przenośnych

Łącza w przeglądarce lub aplikacji Mobile w usłudze Azure aplikacji do istniejącego połączenia hybrydowe, zaznacz **Korzystanie z istniejącego połączenia hybrydowych** w karta hybrydowych połączeń. Zobacz [dostępu do lokalnych zasobów za pomocą połączenia hybrydowych w usłudze Azure aplikacji](../app-service-web/web-sites-hybrid-connection-get-started.md).

## <a name="InstallHCM"></a>Instalowanie Menedżera połączeń hybrydowych lokalnego

Po utworzeniu połączenia hybrydowych zainstalować hybrydowych Connection Manager na zasobów lokalnych. Można pobrać z aplikacji Azure web lub z usługi BizTalk. Kroki BizTalk usług: 

1. Zaloguj się do [portalu klasyczny Azure](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. W okienku nawigacji po lewej stronie wybierz pozycję **Usługi BizTalk** , a następnie wybierz pozycję usługi BizTalk. 
3. Wybierz kartę **Hybrydowych połączenia** :  
![Karta połączenia hybrydowego][HybridConnectionTab]
4. Na pasku zadań wybierz pozycję **Konfiguracja lokalnego**:  
![Ustawienia lokalne][HCOnPremSetup]
5. Wybierz pozycję **Instalowanie i konfigurowanie** uruchomienia lub pobrać hybrydowych Connection Manager na komputerze lokalnym. 
6. Zaznacz pole wyboru, aby rozpocząć instalację. 

<!--
You can also download the Hybrid Connection Manager MSI file and copy the file to your on-premises resource. Specific steps:

1. Copy the on-premises primary Connection String. See [Manage Hybrid Connections](#ManageHybridConnection) in this topic for the specific steps.
2. Download the Hybrid Connection Manager MSI file. 
3. On the on-premises resource, install the Hybrid Connection Manager from the MSI file. 
4. Using Windows PowerShell, type: 
> Add-HybridConnection -ConnectionString “*Your On-Premises Connection String that you copied*” 
--> 

#### <a name="additional"></a>Dodatkowe
- Hybrydowe Connection Manager można zainstalować w następujących systemach operacyjnych:

    - Windows Server 2008 R2 (.NET Framework 4,5 + i Windows Management Framework 4.0 + wymagane)
    - Windows Server 2012 (Windows Management Framework 4.0 + wymagane)
    - Windows Server 2012 R2


- Po zainstalowaniu hybrydowych Connection Manager są następujące operacje: 

    - Połączenie hybrydowych hostowanej Azure automatycznie jest skonfigurowany do za pomocą podstawowego parametry połączenia aplikacji. 
    - Zasób w środowisku lokalnym automatycznie jest skonfigurowany do używania podstawowego parametry połączenia w środowisku lokalnym.

- Hybrydowe Connection Manager należy użyć lokalnych prawidłowe parametry połączenia dla autoryzacji. Aplikacje Mobile lub aplikacji sieci Web Azure należy użyć parametrów połączenia prawidłową aplikacją o zezwolenie.
- Połączenia hybrydowego można skalować instalując inne wystąpienie Menedżera połączeń hybrydowych na innym serwerze. Konfigurowanie odbiornika lokalnego, aby użyć tego samego adresu jako pierwszy odbiornika lokalnego. W tej sytuacji ruch jest przypadkowo rozmieszczone (okrężnego) między detektory aktywnego lokalnego. 


## <a name="ManageHybridConnection"></a>Zarządzanie połączeniami hybrydowego
Aby zarządzać połączenia hybrydowe, można wykonywać następujące czynności:

- Azure portal za pomocą i przejdź do usługi BizTalk. 
- Używanie [interfejsów API pozostałych](http://msdn.microsoft.com/library/azure/dn232347.aspx).

#### <a name="copyregenerate-the-hybrid-connection-strings"></a>Kopiowanie i Regeneruj hybrydowych parametry połączenia

1. Zaloguj się do [portalu klasyczny Azure](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. W okienku nawigacji po lewej stronie wybierz pozycję **Usługi BizTalk** , a następnie wybierz pozycję usługi BizTalk. 
3. Wybierz kartę **Hybrydowych połączenia** :  
![Karta połączenia hybrydowego][HybridConnectionTab]
4. Wybierz połączenie hybrydowych. Na pasku zadań wybierz pozycję **Zarządzanie połączenie**:  
![Zarządzanie opcjami][HCManageConnection]

    **Zarządzanie połączenia** zawiera listę aplikacji i lokalnych parametry połączenia. Można kopiować parametry połączenia lub Generuj kod dostępu używane w parametrach połączenia. 

    **Jeśli wybierzesz Regeneruj**, używane w parametrach połączenia kod dostępu udostępnione został zmieniony. Wykonaj następujące czynności:
    - W portalu klasyczny Azure wybierz **Klawiszy synchronizacji** w aplikacji Azure.
    - Ponownie uruchom **Instalatora lokalnego**. Po uruchomieniu ponownie konfiguracji w środowisku lokalnym, zasobów lokalnych jest automatycznie skonfigurowany do używania parametry połączenia podstawowego zaktualizowane.


#### <a name="use-group-policy-to-control-the-on-premises-resources-used-by-a-hybrid-connection"></a>Zastosowanie zasad grupy do kontrolowania zasobów lokalnych używane przez połączenie hybrydowego

1. Pobieranie [szablonów administracyjnych hybrydowych Connection Manager](http://www.microsoft.com/download/details.aspx?id=42963).
2. Wyodrębnianie plików.
3. Na komputerze, na którym modyfikuje zasad grupy wykonaj następujące czynności:  

    - Kopiuj. Pliki ADMX do folderu *%WINROOT%\PolicyDefinitions* .
    - Kopiuj. ŚILS plików do folderu *%WINROOT%\PolicyDefinitions\en-us* .

Po skopiowaniu można zmienić zasady za pomocą Edytora zasad grupy.




## <a name="next"></a>Następny

[Łączenie aplikacji sieci Web Azure do zasobu lokalnego](../app-service-web/web-sites-hybrid-connection-get-started.md)  
[Nawiązywanie połączenia z lokalnego programu SQL Server z aplikacjami Azure Web](../app-service-web/web-sites-hybrid-connection-connect-on-premises-sql-server.md)   
[Omówienie połączeń hybrydowego](integration-hybrid-connection-overview.md)


## <a name="see-also"></a>Zobacz też

[Interfejsu API usługi REST związanych z zarządzaniem BizTalk usługi Microsoft Azure](http://msdn.microsoft.com/library/azure/dn232347.aspx)  
[Usługi BizTalk: Wykres wersji](biztalk-editions-feature-chart.md)  
[Tworzenie usługi BizTalk za pomocą portalu klasyczny Azure](biztalk-provision-services.md)  
[BizTalk usługi: Karty pulpitu nawigacyjnego, monitorowanie i skali](biztalk-dashboard-monitor-scale-tabs.md)


[HybridConnectionTab]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionTab.png
[HCOnPremSetup]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionOnPremSetup.png
[HCManageConnection]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionManageConn.png 
