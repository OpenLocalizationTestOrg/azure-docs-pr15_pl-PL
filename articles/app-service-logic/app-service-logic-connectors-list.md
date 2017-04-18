<properties
    pageTitle="Listy dostępnych łączników i aplikacje interfejsu API | Microsoft Azure aplikacji usługi"
    description="Przeczytaj o łączników i aplikacje interfejsu API usługi Azure aplikacji"
    services="logic-apps"
    documentationCenter=""
    authors="MandiOhlinger"
    manager="erikre"
    editor="cgronlun"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/01/2016"
    ms.author="mandia"/>


# <a name="list-of-connectors-and-api-apps-to-use-in-your-logic-apps"></a>Lista łączników i aplikacje interfejsu API do użycia w aplikacji warunków logicznych
>[AZURE.NOTE] Tą wersją artykułu dotyczy wersji schematu 2014-12-01-podgląd aplikacji logicznych. Dla wersji logiki aplikacji (GA, General Availability) zobacz [Nową listę łączników](../connectors/apis-list.md).

Informacje na temat wszystkich dostępnych łączników i aplikacje interfejsu API utworzoną przez firmę Microsoft korzystać w logiczny aplikacji.

Ceny informacji oraz listę co to jest dołączany do każdego poziomu usług, zobacz [Azure aplikacji usługi ceny](https://azure.microsoft.com/pricing/details/app-service/).

> [AZURE.NOTE] Aby rozpocząć pracę z aplikacjami logiczny przed utworzeniem konta dla konta Azure, przejdź do [Spróbuj logiczny aplikacji](https://tryappservice.azure.com/?appservice=logic). Możesz od razu utworzyć aplikacji logika krótkotrwałe starter w aplikacji usługi. Nie kart kredytowych wymagane; nie zobowiązania.

## <a name="core-connectors"></a>Podstawowe łączników
W poniższej tabeli wymieniono wszystkie dostępne łączników i aplikacje interfejsu API utworzoną przez firmę Microsoft, są dostępne jako łączników podstawowe:

Nazwa | Opis
--- | ---
[Bing Translator](https://azure.microsoft.com/marketplace/partners/bing/microsofttranslator/) | Tłumaczenie tekstu w innym języku za pomocą usługi Bing.
[HTTP](app-service-logic-connector-http.md) | Odbiornik HTTP otwiera punktu końcowego, który działa jako serwer HTTP i wykrywa przychodzące żądania HTTP lub HTTPS. Akcja HTTP nie wymaga aplikacji interfejsu API i obsługiwaną pierwotnie logiki aplikacji.
[Zapas czasu](app-service-logic-connector-slack.md) | Nawiązywanie połączenia z zapas czasu i ogłaszać wiadomości w kanałach zapasu czasu.


## <a name="enterprise-integration-connectors"></a>Łączniki integracji przedsiębiorstwa
W poniższej tabeli wymieniono wszystkie dostępne łączniki i aplikacje interfejsu API utworzoną przez firmę Microsoft dostępne jako łączników integracji przedsiębiorstwa:

Nazwa  | Opis
------------- | -------------
[Reguły BizTalk](app-service-logic-use-biztalk-rules.md) | Definiowanie i kontrolowanie reguł biznesowych w obrębie organizacji przy użyciu reguł BizTalk. Zasady biznesowe można zaktualizować bez ponownego kompilowania lub ponowne rozmieszczanie skojarzone aplikacje.
[Wyodrębnianie XPath BizTalk](app-service-logic-xpath-extract.md) | Wyszukuje i wyodrębnia dane z zawartości XML według XPath możesz wybrać.
[Łącznik DB2](app-service-logic-connector-db2.md) | Łączy do programu IBM DB2 bazy danych lokalnych i na Azure maszyn wirtualnych z systemu operacyjnego Windows. Można mapować operacje interfejsu API sieci Web oraz OData API do poleceń programu Informix Structured Query Language. <br/><br/>Wyzwalacze nie. Działania obejmują tabeli wybierz pozycję, wstawianie, Aktualizuj, Usuń i niestandardowej instrukcji<br/><br/>Ten łącznik zawiera również program Microsoft Client dla DRDA do nawiązywania połączenia z serwerem programu Informix w sieci TCP/IP.
[Plik](app-service-logic-connector-file.md) | Za pomocą tego łącznika, można nawiązać systemu plików lokalnego lub sieci i zadania wykonane inny plik, w tym wysyłanie, usuwanie, listę plików i więcej.
[Programu Informix](app-service-logic-connector-informix.md) | Nawiązuje połączenie z bazą danych programu IBM Informix lokalnego i Azure maszyn wirtualnych systemu operacyjnego Windows. Można mapować operacje interfejsu API sieci Web oraz OData API do poleceń programu Informix Structured Query Language.<br/><br/>Wyzwalacze nie. Działania obejmują tabeli wybierz pozycję, wstawianie, Aktualizuj, Usuń i niestandardowej instrukcji.<br/><br/>Podczas korzystania z lokalnego, można używać sieci VPN lub Azure ExpressRoute. Ten łącznik zawiera również program Microsoft Client dla DRDA do nawiązywania połączenia z serwerem programu Informix w sieci TCP/IP.
[Program Microsoft SQL Server](app-service-logic-connector-sql.md) | Łączy do lokalnego programu SQL Server lub bazy danych Azure SQL. Można tworzenie, aktualizowanie, pobieranie i usuwanie wpisów w tabeli bazy danych SQL.
MQ | Łączy IBM WebSphere MQ Server w wersji 8 lokalnego i Azure maszyn wirtualnych systemu operacyjnego Windows. Podczas korzystania z lokalnego, można używać VPN lub Azure ExpressRoute. Łącznik także Microsoft Client dla MQ.<br/><br/>Wyzwalacze nie. Żadne akcje.<br/><br/>**Uwaga** Obecnie nie można używać z aplikacjami logicznych.

## <a name="connectors-as-triggers"></a>Łączniki jako wyzwalaczy
Kilka łączników zapewniają wyzwalaczy logiki aplikacji. Te wyzwalacze występują dwa typy:

1. Wyzwalacze ankiety: Te wyzwalacze ankieta usługi z określoną częstotliwością, aby sprawdzić, czy są nowe dane. Po udostępnieniu nowe dane nowego wystąpienia aplikacji logika działa z danymi jako danych wejściowych. Aby zapobiec są używane te same dane wiele razy wyzwalacz może oczyszczania danych czytanie i przekazywane do aplikacji logicznych. Przykłady takich łączników SQL, plik i Magazyn Azure.
2. Wyzwalacze wypychanych: Te wyzwalacze nasłuchują punktu końcowego lub zdarzenia. Następnie wyzwalanie nowego wystąpienia aplikacji logicznych. Przykłady takich łączników odbiornik HTTP i Twitter.

## <a name="connectors-as-actions"></a>Łączniki jako akcje
Można także łączników jako akcji w aplikacji logicznych. Akcje są przydatne w przypadku wyszukiwania danych w aplikacji logiki, którą można następnie używane w realizacji. Na przykład może być konieczne wyszukiwanie danych z bazy danych SQL, aby uzyskać dodatkowe informacje o kliencie podczas przetwarzania zamówienia. Lub może być konieczne pisanie, aktualizowanie i usuwanie danych w miejsca docelowego. Można to zrobić przy użyciu akcji udostępnianych przez łączników. Akcje zamapuj operacje w aplikacjach interfejsu API (zdefiniowana przez ich metadane Swagger).

## <a name="create-your-own-connectors-and-api-apps"></a>Tworzenie własnych łączników i aplikacje interfejsu API
[Łączniki i interfejsu API aplikacje odwołań](http://aka.ms/appservicesconnectorreference)  
[Azure interfejs API usługi aplikacji aplikacji wyzwalaczy](../app-service-api/app-service-api-dotnet-triggers.md)  
[Logika aplikacji odwołania](https://msdn.microsoft.com/library/azure/dn948510.aspx)

## <a name="more-on-connectors-and-api-apps"></a>Więcej informacji na temat interfejsu API aplikacje i łączników
[Co to są łączników i BizTalk interfejsu API aplikacje](app-service-logic-what-are-biztalk-api-apps.md)  
[Hybrydowe Connection Manager przy użyciu Azure aplikacji usługi](app-service-logic-hybrid-connection-manager.md)  
[Monitorowanie wbudowanych łączników i aplikacjami interfejsu API i zarządzanie nimi](app-service-logic-monitor-your-connectors.md)
