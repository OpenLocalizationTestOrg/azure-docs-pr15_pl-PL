<properties 
    pageTitle="Omówienie sprawdzania poprawności XML w pakiecie integracji przedsiębiorstwa | Microsoft Azure aplikacji usługi | Microsoft Azure" 
    description="Dowiedz się, jak sprawdzanie poprawności działa w aplikacji Enterprise Integracja z dodatkiem Service Pack i układy logiczne" 
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
    ms.date="07/08/2016" 
    ms.author="deonhe"/>

# <a name="enterprise-integration-with-xml-validation"></a>Integracja przedsiębiorstwa ze sprawdzaniem poprawności XML

## <a name="overview"></a>Omówienie
Często w scenariuszach B2B partnerów umowy muszą sprawdzić poprawność które komunikatów wymienianych między sobą obowiązują przed rozpoczęciem przetwarzania danych. Łącznik sprawdzania poprawności XML pakietu integracji przedsiębiorstwa umożliwia sprawdzania poprawności dokumentów ze schematem wstępnie zdefiniowanych.  

## <a name="how-to-validate-a-document-with-the-xml-validation-connector"></a>Jak sprawdzić poprawność dokumentu przy użyciu łącznika sprawdzania poprawności XML
1. Tworzenie aplikacji logiki i [połączyć go z kontem integracji](./app-service-logic-enterprise-integration-accounts.md "Dowiedz się, aby połączyć konto integracji aplikacji logiczny") zawiera schemat, których używasz do sprawdzania poprawności danych XML.
2. Dodawanie wyzwalacza **żądanie — żądania HTTP po odebraniu** do aplikacji warunków logicznych  
![](./media/app-service-logic-enterprise-integration-xml/xml-1.png)    
3. Dodaj akcję **Sprawdzania poprawności XML** przez wybranie **Dodaj akcję**  
4. W polu wyszukiwania wprowadź *xml* w celu filtrowania wszystkie akcje do szablonu, który ma być używany 
5. Wybierz pozycję **Sprawdzanie poprawności XML**     
![](./media/app-service-logic-enterprise-integration-xml/xml-2.png)   
6. Zaznacz pole tekstowe **zawartości**  
![](./media/app-service-logic-enterprise-integration-xml/xml-1-5.png)
7. Zaznacz znacznik treści jako wartości, które będą sprawdzane.   
![](./media/app-service-logic-enterprise-integration-xml/xml-3.png)  
8. Zaznacz pole **Nazwa SCHEMATU** listy i wybierz schemat, który ma być używany do sprawdzania poprawności wprowadzania *zawartości* powyżej     
![](./media/app-service-logic-enterprise-integration-xml/xml-4.png) 
9. Zapisywanie wyników pracy  
![](./media/app-service-logic-enterprise-integration-xml/xml-5.png) 

W tym momencie po zakończeniu konfigurowania tego łącznika sprawdzania poprawności. W aplikacji rzeczywistych można przechowywać zatwierdzonych danych w aplikacji LOB, takich jak usługi SalesForce. Można łatwo dodawać akcji do wysłania wyniku sprawdzania poprawności usług Salesforce. 

Teraz możesz przetestować działanie sprawdzania poprawności dokonując żądania HTTP punktu końcowego.  

## <a name="next-steps"></a>Następne kroki

[Dowiedz się więcej o pakiecie integracji przedsiębiorstwa] (./app-service-logic-enterprise-integration-overview.md "Więcej informacji na temat Enterprise Integracja z dodatkiem Service Pack")   