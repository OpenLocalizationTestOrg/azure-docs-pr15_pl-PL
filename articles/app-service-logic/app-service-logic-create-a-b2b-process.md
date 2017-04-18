<properties 
   pageTitle="Tworzenie procesu B2B w usłudze Azure aplikacji | Microsoft Azure" 
   description="Omówienie sposobu tworzenia procesów biznesowych do biznesowych" 
   services="logic-apps" 
   documentationCenter=".net,nodejs,java" 
   authors="rajram" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="04/20/2016"
   ms.author="rajram"/>

# <a name="creating-a-b2b-process"></a>Tworzenie procesu B2B

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]


## <a name="business-scenario"></a>Scenariusza biznesowego 
Contoso i Northwind są dwie partnerów biznesowych. Contoso (detalicznego) wysyła zamówienia zakupu do Northwind (dostawca) w branży transportu poziomu, takie jak AS2. Northwind przechowuje wszystkich przychodzących zamówień w jego magazynu w chmurze. Zamówienia zakupu są wiadomości XML między tymi dwoma partnerami. Gdy wiadomość jest przechowywana w magazynu w chmurze firmy Northwind następnie firmy Northwind wewnętrzne procesy uchwyt kolejności od tego momentu na.
 
Celem tego samouczka jest ustalenie sposobu Northwind ustanowić procesów biznesowych za pomocą którego można odbierać wiadomości (zamówień w formacie XML) od partnera firmy Contoso przez AS2 i utrzymują go w jego magazynu w chmurze.


## <a name="capabilities-demonstrated"></a>Możliwości wykazać 
Ten samouczek ułatwia zaprezentowania następujące możliwości: 

- **Transportem wiadomości**: u sprzedawcy detalicznego i dostawcy może być na innych platformach, ale nadal optymalną komunikacji między nimi. W tym samouczku komunikują się nad AS2 (stosowanie instrukcji 2). AS2 jest popularnym sposobem transportu danych między partnerów handlowych w komunikacji firm do firmy.
- **Utrzymywanie danych**: po wiadomość została odebrana przez AS2, a następnie Northwind chce ją przed dalszego przetwarzania. Może używać łącznika do innej witryny wiadomości w jego magazynu w chmurze. W tym samouczku obiektów blob platformy Azure jest podejmowana użyć jako magazynu w chmurze dla Northwind.
- **Tworzenie procesów biznesowych**: W przepływie wielu aplikacji interfejsu API może być ze sobą wartość uzyskanie wyników biznesowych, jak pokazano poniżej.


## <a name="before-you-begin"></a>Przed rozpoczęciem
Tego samouczka przyjęto założenie, że masz już podstawy usługi aplikacji Azure, dowiedzieć się, jak tworzyć aplikacje interfejsu API i łączenie przepływu.


## <a name="steps-to-achieve-the-business-scenario"></a>Czynności, aby osiągnąć scenariusza biznesowego
**Tworzenie i konfigurowanie wymagane aplikacje interfejsu API**

1. Tworzenie wystąpienia **Azure magazyn obiektów Blob łącznik**. W tym celu poświadczenia konta usługi Azure miejsca do magazynowania. Upewnij się, że jest gotowy przed rozpoczęciem tworzenia to.
2. Utwórz wystąpienie **Zarządzania Trading partnera BizTalk**. Wymaga to pusta baza danych SQL do funkcji. Upewnij się, że jest gotowy przed rozpoczęciem tworzenia to.
3. Tworzenie wystąpienia **AS2 łącznik**. Wymaga to pusta baza danych SQL do funkcji. Upewnij się, że jest gotowy przed rozpoczęciem tworzenia to. Ponadto jeśli chcesz zarchiwizować wiadomości jako część AS2 przetwarzania, możesz może zapewnić poświadczeń do obiektów Blob platformy Azure podczas jej tworzenia.
4. Konfigurowanie usługi TPM (Trading partnera Zarządzanie) utworzony:  
    1. Przejdź do wystąpienie usługi TPM utworzonych jako część powyższe czynności.
    2. Używanie opcji **partnerów** w obszarze *składniki* do sekcji **Dodawanie** nowego partnera o nazwie **Contoso** i w profilu Dodaj wymagane tożsamości AS2.
    3. Używanie opcji **partnerów** w obszarze *składniki* do sekcji **Dodawanie** nowego partnera o nazwie **Northwind** i w profilu Dodaj wymagane tożsamość AS2.
    4. Za pomocą opcji **umów** w obszarze *składniki* do układu **Dodaj** nowe AS2 między Northwind i Contoso. Northwind będzie tutaj hostowanej partnera, a Contoso będzie partnerem gościa. Odpowiednio skonfigurować podpisywania, szyfrowania kompresji i potwierdzeń podczas tworzenia tej Umowy. W przypadku certyfikatów konieczne może być używany, ich można przekazywać za pomocą opcji **Certyfikaty** podczas przeglądania usługi TPM, który jest tworzony.


## <a name="create-a-flow--business-process"></a>Tworzenie przepływu / procesów biznesowych
1. Utwórz nowy przepływ, w którym pierwszym krokiem jest AS2. Przeciągnij i upuść **AS2 łącznik** i wybierz wystąpienie już utworzone. Wybierz pozycję wyzwalacz jako funkcji:  
    ![][1]  
2. Następnie przeciągnij i upuść **Azure magazyn obiektów Blob łącznik** i wybierz wystąpienie już utworzone. Wybierz akcję jako funkcji oraz w ramach, wybierz pozycję **Przekaż obiektów Blob** jako wymaganej funkcjonalności. Konfigurowanie zależnie od potrzeb.
3. Teraz tworzenie i wdrażanie przepływu.


## <a name="message-processing--troubleshooting"></a>Przetwarzanie wiadomości i rozwiązywanie problemów
1. Nadszedł czas na przetestowanie przepływ jest wdrożone. Wysyłanie wiadomości XML zawinięty w AS2 (zgodnie z umową AS2 utworzony w poprzednim przykładzie) do punktu końcowego AS2 udostępnione przez wystąpienie AS2Connector utworzone przez Ciebie. Konieczne może być Konfigurowanie uwierzytelniania dla punktu końcowego, tak aby była publicznie.
2. Wykonanie informacji o procesie jest wyświetlana, przechodząc do przepływu, a następnie przechodzenie do wystąpienia przepływu, które masz wykonane
3. Dla AS2 przetwarzanie informacji przejdź do związane wystąpienie AS2Connector, a następnie wykonaj podczas przeglądania w ramach śledzenia. Można użyć filtrów biorących udział informacje, które chcesz ograniczyć możliwość wyświetlania.

![][2]

<!--Image references-->
[1]: ./media/app-service-logic-create-a-b2b-process/Flow.png
[2]: ./media/app-service-logic-create-a-b2b-process/Tracking.png
 
