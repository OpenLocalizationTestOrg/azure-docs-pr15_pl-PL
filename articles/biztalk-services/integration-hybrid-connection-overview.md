<properties
    pageTitle="Omówienie połączeń hybrydowych | Microsoft Azure"
    description="Więcej informacji o hybrydowych połączeń, zabezpieczenia porty TCP i obsługiwanych konfiguracji. MABS, WABS."
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
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="ccompy"/>


# <a name="hybrid-connections-overview"></a>Omówienie połączeń hybrydowego
Wprowadzenie do połączeń hybrydowych zawiera listę obsługiwanych konfiguracji i wyświetla wymagane porty TCP.


## <a name="what-is-a-hybrid-connection"></a>Co to jest połączenie hybrydowe

Połączenia hybrydowe to funkcja usługi BizTalk Azure. Połączenia hybrydowych zapewniają łatwy i wygodny sposób łączenia funkcje aplikacji sieci Web w usłudze Azure aplikacji (dawniej witryn sieci Web) i aplikacje Mobile w usłudze Azure aplikacji (dawniej Mobile Services) do zasobów lokalnych za zaporą.

![Połączenia hybrydowego][HCImage]

Zalety połączeń hybrydowych:

- Aplikacje sieci Web i aplikacji Mobile mogą uzyskiwać dostęp istniejących danych lokalnych, jak i usługi bezpiecznego.
- Wiele aplikacji Mobile lub aplikacji sieci Web można udostępniać połączenia hybrydowy dostęp do zasobów lokalnych.
- Porty TCP minimalnego są wymagane do uzyskania dostępu do sieci.
- Aplikacje przy użyciu połączeń hybrydowych dostęp do zasobów lokalnych określonych, publikowanej za pośrednictwem połączenia hybrydowych.
- Można nawiązać połączenia z każdego zasobu lokalnego, który używa statyczne port TCP, takich jak program SQL Server, MySQL HTTP sieci Web API i większość niestandardowych usług sieci Web.

    > [AZURE.NOTE] Oparte na TCP usług, które korzystają z portów dynamicznych (na przykład FTP pasywne używane w stronie biernej trybu lub) nie są obecnie obsługiwane. LDAP również nie jest obsługiwane. LDAP używa statyczne port TCP, ale można także stosować UDP. W wyniku go nie jest obsługiwane.

- Można używać z ram wszystkie obsługiwane przez aplikacje sieci Web (.NET, PHP, Java, Python, Node.js) i aplikacje Mobile (Node.js, .NET).
- Aplikacje sieci Web i aplikacji Mobile ma dostęp lokalnych zasobów w ten sam sposób, jakby sieci Web lub aplikacji Mobile znajduje się w sieci lokalnej. Na przykład sam połączenia ciąg używany lokalnego można również Azure.


Połączenia hybrydowych udostępniają administratorom przedsiębiorstwa sterowania i wgląd zasobów firmy dostęp do aplikacji hybrydowe, w tym:

- Używanie zasad grupy, Administratorzy mogą zezwalania na połączenia hybrydowych w sieci i również wyznaczyć zasoby, które mogą być udostępniane przez aplikacje hybrydowych.
- Dzienniki zdarzeń i inspekcji w sieci firmowej zapewniają wgląd zasobów dostęp do hybrydowego połączenia.


## <a name="example-scenarios"></a>Przykładowe scenariusze

Połączenia hybrydowych obsługuje następujących kombinacji framework i aplikacji:

- .NET framework dostępu do programu SQL Server
- .NET framework dostęp do usług protokołu HTTP/HTTPS z WebClient
- PHP dostęp do programu SQL Server, MySQL
- Java dostęp do programu SQL Server, MySQL i Oracle
- Java dostęp do usług protokołu HTTP/HTTPS

Podczas korzystania z połączeń hybrydowe, aby uzyskać dostęp do lokalnego programu SQL Server, należy rozważyć następujące kwestie:

- Wystąpień o nazwie Express programu SQL musi być skonfigurowany do używania statycznego porty. Domyślnie SQL Express o nazwie wystąpienia Użyj portów dynamicznych.
- SQL Express domyślne wystąpienia używać portu statycznego, ale musi być włączona funkcja TCP. Domyślnie TCP jest wyłączona.
- Podczas korzystania z klastrowanie lub grupy dostępności `MultiSubnetFailover=true` tryb nie jest obecnie obsługiwane.
- `ApplicationIntent=ReadOnly` Nie jest obecnie obsługiwane.
- Uwierzytelnianie programu SQL może być wymagane jako metodę autoryzacji zakończenia do końca obsługiwane przez aplikację Azure i lokalnego serwera SQL.


## <a name="security-and-ports"></a>Zabezpieczenia i porty

Połączenia hybrydowych bezpiecznego połączenia z aplikacji Azure i lokalnych hybrydowych Connection Manager do połączenia hybrydowych za pomocą autoryzacji podpisu udostępnionych programu Access (SA). Klucze oddzielnego połączenia są tworzone stosowania i Connection Manager hybrydowych lokalnego. Klucze połączenia można wycofane i odwołany niezależnie.

Połączenia hybrydowych zapewniają bezproblemowe i bezpieczne dystrybucji klawisze aplikacje oraz lokalnego hybrydowych Connection Manager.

Zobacz [Tworzenie i zarządzanie połączeniami hybrydowych](integration-hybrid-connection-create-manage.md).

*Autoryzacja aplikacji różni się od połączenia hybrydowych*. Można używać dowolnej metody odpowiednich uprawnień. Metoda autoryzacji zależy od metody autoryzacji zakończenia do końca obsługiwane w chmurze Azure i składników lokalnego. Na przykład aplikacja Azure uzyskuje dostęp do lokalnego programu SQL Server. W tym scenariuszu autoryzacji SQL mogą zostać metody autoryzacji obsługiwane zakończenia do końca.

#### <a name="tcp-ports"></a>Porty TCP
Połączenia hybrydowych wymagają tylko ruchu wychodzącego TCP i HTTP łączność z sieci prywatnej. Nie ma potrzeby otwierania żadnych portów zapory lub modyfikowania konfiguracji obwód kształtu sieci, aby umożliwić wszelkie przychodzące połączenia w sieci.

Następujące porty TCP są używane przez połączenia hybrydowych:

Port | Dlaczego jest potrzebny
--- | ---
9350 - 9354 | Te porty są używane do transmisji danych. Menedżer przekazywania Bus usługi sondy port 9350 ustalenie, jeśli jest dostępna łączność TCP. Jeśli jest dostępny, następnie przyjęto założenie, że port 9352 jest również dostępne. Ruch omówiono port 9352. <br/><br/>Zezwalanie na połączenia wychodzące do tych portów.
5671 | Użycie portu 9352 ruchu danych port 5671 jest używany jako kanał sterowania. <br/><br/>Zezwalanie na połączenia wychodzące do tego portu.
porty 80, 443 | Te porty są używane dla niektórych żądań danych Azure. Ponadto jeśli porty 9352 i 5671 nie są użyteczne, *następnie* porty 80 i 443 są alternatywnych porty używane do przesyłania danych i kanału kontroli.<br/><br/>Zezwalanie na połączenia wychodzące do tych portów. <br/><br/>**Uwaga** Skorzystaj z poniższych jako alternatywnych porty zamiast porty TCP nie jest zalecane. HTTP/WebSocket jest używany jako protokół zamiast natywnych TCP dla kanałów danych. Może powodować mniejszą wydajność.



## <a name="next-steps"></a>Następne kroki

[Tworzenie i zarządzanie połączeniami hybrydowego](integration-hybrid-connection-create-manage.md)<br/>
[Łączenie aplikacji sieci Web Azure do zasobu lokalnego](../app-service-web/web-sites-hybrid-connection-get-started.md)<br/>
[Nawiązywanie połączenia z lokalnego programu SQL Server z aplikacji Azure web](../app-service-web/web-sites-hybrid-connection-connect-on-premises-sql-server.md)<br/>


## <a name="see-also"></a>Zobacz też

[Interfejsu API usługi REST związanych z zarządzaniem BizTalk usługi Microsoft Azure](http://msdn.microsoft.com/library/azure/dn232347.aspx)
[usług BizTalk: wersje wykresu](biztalk-editions-feature-chart.md)<br/>
[Tworzenie usługi BizTalk przy użyciu Azure portal](biztalk-provision-services.md)<br/>
[BizTalk usługi: Karty pulpitu nawigacyjnego, monitorowanie i skali](biztalk-dashboard-monitor-scale-tabs.md)<br/>

[HCImage]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionImage.png
[HybridConnectionTab]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionTab.png
[HCOnPremSetup]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionOnPremSetup.png
[HCManageConnection]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionManageConn.png
