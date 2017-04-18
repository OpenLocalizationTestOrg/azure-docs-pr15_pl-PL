<properties
   pageTitle="Logika aplikacje lokalne połączenia danych bramy | Microsoft Azure"
   description="Informacje na temat tworzenia połączenia do lokalnego bramy danych za pomocą aplikacji logicznych."
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="07/05/2016"
   ms.author="jehollan"/>

# <a name="connect-to-the-on-premises-data-gateway-for-logic-apps"></a>Nawiązywanie połączenia z bramy danych lokalnych w przypadku aplikacji warunków logicznych

Logika obsługiwane aplikacje łączników umożliwiają skonfigurowania połączenia z danymi w lokalnej programu access za pomocą bramy danych lokalnych.  Następujące czynności przeprowadzi Cię przez proces zainstalować i skonfigurować bramę danych lokalnych do pracy z aplikacją logika.

## <a name="prerequisites"></a>Wymagania wstępne

* Należy przy użyciu służbowym lub szkolny adres e-mail w Azure skojarzyć bramy danych lokalnych z Twojego konta (usługi Azure Active Directory podstawie)
    * Jeśli korzystasz z Account Microsoft (np. @outlook.com, @live.com) konto Azure umożliwia tworzenie służbowym lub szkolny adres e-mail, wykonując [czynności opisane w tym miejscu](../virtual-machines/virtual-machines-windows-create-aad-work-id.md#locate-your-default-directory-in-the-azure-classic-portal)

> [AZURE.WARNING] Ma obecnie ograniczenie, które lokalnej instalacji bramy tylko zostanie ukończona, używając konta, które zostało zarejestrowane dzięki usłudze Power BI.  W międzyczasie Zarejestruj żadnego z kont w "Power BI bezpłatnego" Aby pomyślnie ukończyć instalację.

* Musi być lokalnego danych bramy [zainstalowany na komputerze lokalnym](app-service-logic-gateway-install.md).
* Brama musi nie zostały zastrzeżenia przez innego bramę danych lokalnych Azure ([ubiegać się dzieje z tworzenia krok 2 poniżej](#2-create-an-azure-on-premises-data-gateway-resource)) — instalacji może być skojarzony tylko do jednej bramy zasobu.

## <a name="installing-and-configuring-the-connection"></a>Instalowanie i konfigurowanie połączenia

### <a name="1-install-the-on-premises-data-gateway"></a>1. Instalowanie bramy dane lokalne

[W tym artykule](app-service-logic-gateway-install.md)znajdują się informacje na temat instalowania bramy danych lokalnych.  Brama musi być zainstalowany na komputerze lokalnym, zanim będziesz kontynuować pozostałe kroki.

### <a name="2-create-an-azure-on-premises-data-gateway-resource"></a>2. Tworzenie zasób bramy danych lokalnych Azure

Po zainstalowaniu subskrypcji Azure należy skojarzyć z bramą danych lokalnych.

1. Zaloguj się do Azure za pomocą samego pracy lub szkole adres e-mail, który został użyty podczas instalacji bramy
1. Kliknij przycisk **Nowy** zasób
1. Wyszukiwanie i wybierz **bramę danych lokalnych**
1. Wprowadź informacje, które chcesz skojarzyć swoje konto — takie jak wybór odpowiedniej **Nazwy instalacji** bramy

    ![Połączenie bramy danych lokalnych][1]
1. Kliknij przycisk **Utwórz** , aby utworzyć zasób

### <a name="3-create-a-logic-app-connection-in-the-designer"></a>3. Tworzenie połączenia aplikacji logicznych w Projektancie

Teraz, gdy Azure subskrypcji jest skojarzony z wystąpieniem bramy danych lokalnych, możesz utworzyć połączenie z tym z poziomu aplikacji logiczny.

1. Otwórz aplikację logiki i wybierz pozycję łącznik, który obsługuje połączenia lokalnego (od tej dokumentacji programu SQL Server)
1. Zaznacz pole wyboru dla **łączenia za pośrednictwem lokalnych danych bramy**

    ![Tworzenie bramy projektanta aplikacji warunków logicznych][2]
1. Wybierz pozycję **bramy** nawiązywanie i wykonaj wszelkie inne informacje o połączeniu wymagane
1. Kliknij przycisk **Utwórz** , aby utworzyć połączenie

Połączenie powinna teraz pomyślnie skonfigurowane do użycia w aplikacji logicznych.  

## <a name="next-steps"></a>Następne kroki
- [Typowe przykłady i scenariusze logiki aplikacji](app-service-logic-examples-and-scenarios.md)
- [Funkcje integracji przedsiębiorstwa](app-service-logic-enterprise-integration-overview.md)

<!-- Image references -->
[1]: ./media/app-service-logic-gateway-connection/createblade.PNG
[2]: ./media/app-service-logic-gateway-connection/blankconnection.PNG
[3]: ./media/app-service-logic-gateway-connection/checkbox.PNG