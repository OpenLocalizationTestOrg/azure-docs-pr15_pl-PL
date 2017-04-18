<properties 
    pageTitle="Omówienie integracji przedsiębiorstwa | Microsoft Azure aplikacji usługi | Microsoft Azure" 
    description="Za pomocą funkcji integracji przedsiębiorstwa można umożliwić scenariusze biznesowe, proces i integracji korzystanie z aplikacji warunków logicznych" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="msftman" 
    manager="erikre" 
    editor="cgronlun"/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="deonhe"/>

# <a name="overview-of-the-enterprise-integration-pack"></a>Omówienie pakietu integracji przedsiębiorstwa

## <a name="what-is-the-enterprise-integration-pack"></a>Co to jest pakiet integracji przedsiębiorstwa?
Pakiet integracji przedsiębiorstwa jest rozwiązania opartego na chmurze firmy Microsoft umożliwiających bezproblemowo firma firma (B2B) komunikacji. Pakiet używa standardowych protokołów branżowe tym [AS2](./app-service-logic-enterprise-integration-as2.md), [X12](./app-service-logic-enterprise-integration-x12.md)i [EDIFACT](./app-service-logic-enterprise-integration-edifact.md) do wymiany wiadomości między partnerami biznesowymi. Wiadomości można opcjonalnie zabezpieczyć przy użyciu szyfrowania i podpisy cyfrowe. 

Pakiet umożliwia organizacjom, które używają różnych protokołów i formaty do wymiany wiadomości drogą elektroniczną poprzez przekształcanie różnych formatów do formatu, który systemy obu organizacjach mogą interpretować i podejmij działanie. 

Użytkownicy zaznajomieni z serwera BizTalk lub Microsoft Azure BizTalk Services, będzie ona łatwy w użyciu funkcje integracji przedsiębiorstwa, ponieważ większość koncepcji są podobne. Jedna różnica głównych jest integracji przedsiębiorstwa używanego konta integracji ułatwiają zarządzanie artefaktów używane w komunikacji B2B i miejsca do magazynowania. 

Pod względem architektury pakiet integracji przedsiębiorstwa jest oparty na przechowywanie wszystkich artefaktów, których można używać do projektowania, wdrażania i obsługi aplikacji B2B **integracji konta** . Konta usługi integracji zasadniczo jest oparte na chmurze kontenera przechowywania artefakty, takich jak schematy partnerów, certyfikatów, mapy i umów. Tych efektów można w aplikacjach logiki do utworzenia B2B przepływy pracy. Zanim będzie można używać artefaktów w aplikacji logiczny, musisz połączyć konta integracji aplikacji logicznych. Po ich łączenie, aplikacji logiczny uzyskuje dostęp do konta integracji artefakty.  

## <a name="why-should-you-use-enterprise-integration"></a>Dlaczego należy użyć integracji przedsiębiorstwa?
- W przypadku integracji przedsiębiorstwa jest możliwe do przechowywania wszystkich usługi artefaktów w jednym miejscu, czyli konta integracji. 
- Można wykorzystać aparat aplikacje logiki i jego łączniki do tworzenia B2B przepływów pracy i integracja z 3 aplikacje władz akredytacji bezpieczeństwa firm, lokalnej aplikacji, a także aplikacje niestandardowe
- Możesz również korzystać z funkcji Azure

## <a name="how-to-get-started-with-enterprise-integration"></a>Jak rozpocząć pracę z integracji przedsiębiorstwa?
Możesz tworzyć i zarządzać aplikacjami B2B za pomocą pakietu integracji przedsiębiorstwa przy użyciu projektanta aplikacji logiczny na **Azure portal**.  

Za pomocą [programu PowerShell](https://msdn.microsoft.com/library/azure/mt652195.aspx "aplikacje logiczny tematy programu PowerShell") do zarządzania aplikacji logicznych. 

Poniżej przedstawiono kroki, które należy wykonać, aby można było tworzyć aplikacje w portalu Azure: ![obrazu — omówienie](./media/app-service-logic-enterprise-integration-overview/overview-0.png)  

## <a name="what-are-some-common-scenarios"></a>Co to są kilka typowych scenariuszy?

Integracja z programem Enterprise obsługuje tych standardami:   

- Edytuj - elektronicznej wymiany danych  
- EAI - integracji aplikacji dla przedsiębiorstw  

## <a name="heres-what-you-need-to-get-started"></a>Poniżej opisano, co jest potrzebne do rozpoczęcia
- Subskrypcję usługi Azure za pomocą konta integracji
- Visual Studio 2015 do tworzenia map i schematów
- [Integracja aplikacji przedsiębiorstwa logiczny platformy Microsoft Azure Tools for Visual Studio 2015 2.0](https://aka.ms/vsmapsandschemas)  

## <a name="try-it"></a>Wypróbuj
[Wypróbuj ją teraz](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-as2-send-receive) wdrażanie pełną funkcjonalność próbki AS2 Wyślij / Odbierz logiczny aplikację, która korzysta z funkcji B2B logiczny aplikacji.

## <a name="learn-more-about"></a>Dowiedz się więcej o:
- [Umów] (./app-service-logic-enterprise-integration-agreements.md "Więcej informacji na temat umów integracji przedsiębiorstwa")
- [Scenariusze biznesowe do biznesowe (B2B)] (./app-service-logic-enterprise-integration-b2b.md "Dowiedz się, jak tworzyć aplikacje logicznych z funkcjami B2B")  
- [Certyfikaty] (./app-service-logic-enterprise-integration-certificates.md "Więcej informacji na temat certyfikatów integracji przedsiębiorstwa")
- [Prosty plik Kodowanie dekodowanie] (./app-service-logic-enterprise-integration-flatfile.md "Dowiedz się, jak kodowanie i dekodowanie zawartości pliku prostego")  
- [Integracja z kont] (./app-service-logic-enterprise-integration-accounts.md "Więcej informacji na temat kont integracji")
- [Mapy] (./app-service-logic-enterprise-integration-maps.md "Więcej informacji na temat integracji przedsiębiorstwa map")
- [Partnerzy] (./app-service-logic-enterprise-integration-partners.md "Więcej informacji na temat partnerów integracji przedsiębiorstwa")
- [Schematy] (./app-service-logic-enterprise-integration-schemas.md "Więcej informacji na temat schematów integracji przedsiębiorstwa")
- [Sprawdzanie poprawności komunikatu XML] (./app-service-logic-enterprise-integration-xml.md "Dowiedz się, jak sprawdzać poprawność wiadomości XML z aplikacjami warunków logicznych")
- [Przekształcanie XML] (./app-service-logic-enterprise-integration-transform.md "Więcej informacji na temat integracji przedsiębiorstwa mapy")
- [Łączniki integracji przedsiębiorstwa] (../connectors/apis-list.md "Więcej informacji na temat łączników pack integracji przedsiębiorstwa")



