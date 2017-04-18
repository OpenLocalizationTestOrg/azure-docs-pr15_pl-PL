<properties 
    pageTitle="Omówienie integracji kont i pakiet integracji przedsiębiorstwa | Microsoft Azure aplikacji usługi | Microsoft Azure" 
    description="Dowiedz się, wszystkie informacje o kontach integracji Enterprise Integracja z dodatkiem Service Pack i aplikacje warunków logicznych" 
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

# <a name="overview-of-integration-accounts"></a>Omówienie integracji kont

## <a name="what-is-an-integration-account"></a>Co to jest konto integracji?
Konta usługi integracji jest Azure konta, które umożliwia integracji przedsiębiorstwa aplikacje do zarządzania artefakty tym schematów, mapy, certyfikaty, partnerami i umów. Dowolnej aplikacji integracji, które można tworzyć, będzie konieczne w celu uzyskania dostępu do schematu, mapy lub certyfikatu, na przykład za pomocą konta usługi integracji.

## <a name="create-an-integration-account"></a>Tworzenie konta usługi integracji 
1. Wybierz pozycję **Przeglądaj**   
![](./media/app-service-logic-enterprise-integration-accounts/account-1.png)  
2. Wprowadź **integracji** w polu wyszukiwania filtr i wybierz pozycję **Konta integracji** z listy wyników     
 ![](./media/app-service-logic-enterprise-integration-accounts/account-2.png)  
3. Z menu u góry strony wybierz przycisk *Dodaj*      
![](./media/app-service-logic-enterprise-integration-accounts/account-3.png)  
4. Wprowadź **nazwę**, wybierz **subskrypcję** , którego chcesz użyć, tworzenie nowej **grupy zasobów** lub wybierz istniejącej grupy zasobów, wybierz **lokalizację** , gdzie konta integracji będą obsługiwane, wybierz **poziom ceny**, a następnie kliknij przycisk **Utwórz** .   

  W tym miejscu będzie obsługi administracyjnej konta integracji, w wybranej lokalizacji. To należy wykonać w ciągu 1 minuty.    
![](./media/app-service-logic-enterprise-integration-accounts/account-4.png)  
5. Odśwież stronę. Zostanie wyświetlona konta integracji na liście. Gratulacje!  
![](./media/app-service-logic-enterprise-integration-accounts/account-5.png) 

## <a name="how-to-link-an-integration-account-to-a-logic-app"></a>Jak łączenie klienta z integracji aplikacji warunków logicznych
Aby aplikacji logiczny na dostęp do mapy, schematy, umów i innych artefakty znajdujących się na Twoim koncie integracji musisz najpierw połączyć konta integracji aplikacji logicznych.

### <a name="here-are-the-steps-to-link-an-integration-account-to-a-logic-app"></a>Poniżej przedstawiono kroki umożliwiające łączenie klienta z integracji aplikacji warunków logicznych 

#### <a name="prerequisites"></a>Wymagania wstępne
- Konta usługi integracji
- Aplikacja warunków logicznych

>[AZURE.NOTE]Upewnij się, że Twoje konto integracji i logiki aplikacji są w **tym samym miejscu Azure** przed rozpoczęciem

1. Wybierz łącze **Ustawienia** z menu aplikacji warunków logicznych  
![](./media/app-service-logic-enterprise-integration-accounts/linkaccount-1.png)   
2. Wybierz element **Konta integracji** z karta Ustawienia  
![](./media/app-service-logic-enterprise-integration-accounts/linkaccount-2.png)   
3. Wybierz konto integracji, które chcesz połączyć logiki aplikacji ze **Wybierz konto integracji** rozwijane pole listy  
![](./media/app-service-logic-enterprise-integration-accounts/linkaccount-3.png)   
4. Zapisywanie wyników pracy  
![](./media/app-service-logic-enterprise-integration-accounts/linkaccount-4.png)   
5. Pojawi się powiadomienie, które wskazuje, że Twoje konto integracji została połączona z aplikacji logiki i wszystkich artefaktów na koncie integracji są teraz dostępne dla aplikacji logicznych.  
![](./media/app-service-logic-enterprise-integration-accounts/linkaccount-5.png)   

Teraz, gdy Twoje konto integracji jest połączone z aplikacji logiczny, można przejść do aplikacji logiki i za pomocą łączników B2B, takich jak sprawdzanie poprawności XML, Kodowanie/dekodowanie pliku prostego lub przekształcenie tworzyć aplikacje z funkcjami B2B.  
    
## <a name="how-to-delete-an-integration-account"></a>Jak usunąć konto integracji?
1. Wybierz pozycję **Przeglądaj**  
![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    
2. Wprowadź **integracji** w polu wyszukiwania filtr i wybierz pozycję **Konta integracji** z listy wyników     
 ![](./media/app-service-logic-enterprise-integration-overview/overview-2.png)  
3. Wybierz **konto integracji** , które chcesz usunąć  
![](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  
4. Wybierz pozycję **Usuń** łącze, które znajduje się w menu   
![](./media/app-service-logic-enterprise-integration-accounts/delete.png)  
5. Potwierdź    

## <a name="how-to-move-an-integration-account"></a>Jak przenieść konto integracji?
Konta usługi integracji można łatwo przenieść do nowej subskrypcji i nowej grupy zasobów. Jeśli trzeba przenieść konta integracji, wykonaj następujące czynności:

>[AZURE.IMPORTANT] Należy zaktualizować wszystkie skrypty do przeniesienia konta integracji za pomocą nowego zasobu identyfikatorów.

1. Wybierz pozycję **Przeglądaj**  
![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    
2. Wprowadź **integracji** w polu wyszukiwania filtr i wybierz pozycję **Konta integracji** z listy wyników     
 ![](./media/app-service-logic-enterprise-integration-overview/overview-2.png)  
3. Wybierz **konto integracji** , które chcesz usunąć  
![](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  
4. Zaznacz pole wyboru **Przenieś** łącze, znajdującego się w menu   
![](./media/app-service-logic-enterprise-integration-accounts/move.png)  
5. Potwierdź    

## <a name="next-steps"></a>Następne kroki
- [Dowiedz się więcej o pakiecie integracji przedsiębiorstwa] (./app-service-logic-enterprise-integration-overview.md "Więcej informacji na temat Enterprise Integracja z dodatkiem Service Pack")  
- [Dowiedz się więcej o umów] (./app-service-logic-enterprise-integration-agreements.md "Więcej informacji na temat umów integracji przedsiębiorstwa")  


 