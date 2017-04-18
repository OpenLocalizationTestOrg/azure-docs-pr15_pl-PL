<properties 
    pageTitle="Tworzenie rozwiązań B2B pakietem integracji przedsiębiorstwa | Microsoft Azure aplikacji usługi | Microsoft Azure" 
    description="Dowiedz się więcej o otrzymywanie danych przy użyciu funkcji B2B pakietu integracji przedsiębiorstwa" 
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

# <a name="learn-about-receiving-data-using-the-b2b-features-of-the-enterprise-integration-pack"></a>Dowiedz się więcej o odbierania danych przy użyciu funkcji B2B pakiet integracji przedsiębiorstwa#

## <a name="overview"></a>Omówienie ##

Ten dokument jest częścią pakietu integracji logiki aplikacji przedsiębiorstwa. Zapoznaj się z artykułem, aby dowiedzieć się więcej na temat [możliwości pakietu integracji przedsiębiorstwa](./app-service-logic-enterprise-integration-overview.md).

## <a name="prerequisites"></a>Wymagania wstępne ##

Aby używać AS2 i X12 akcje, które będą potrzebne jest konto integracji przedsiębiorstwa

[Jak utworzyć konto integracji przedsiębiorstwa](./app-service-logic-enterprise-integration-accounts.md)

## <a name="how-to-use-the-logic-apps-b2b-connectors"></a>Jak za pomocą łączników B2B aplikacje warunków logicznych ##

Po utworzeniu konta usługi integracji i dodane partnerów i umów do niego możesz przystąpić do tworzenia aplikacji logiczny, który zawiera przepływ pracy firma i firma (B2B).

W tym walkthru zobaczysz sposobu używania AS2 i X12 działania w celu utworzenia aplikacji logika firma i firma pobierającego dane z partnera handlowego.

1. Tworzenie nowego logiki aplikacji i [połączyć go z kontem integracji](./app-service-logic-enterprise-integration-accounts.md).  
2. Dodawanie wyzwalacza **żądanie — żądania HTTP po odebraniu** do aplikacji warunków logicznych  
![](./media/app-service-logic-enterprise-integration-b2b/flatfile-1.png)  
3. Dodaj akcję **Dekodowanie AS2** przez wybranie **Dodaj akcję**  
![](./media/app-service-logic-enterprise-integration-b2b/transform-2.png)  
4. W polu wyszukiwania wprowadź word **as2** w celu filtrowania wszystkie akcje do szablonu, który ma być używany  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-5.png)  
6. Wybierz akcję, **AS2 - AS2 dekodowanie wiadomości**  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-6.png)  
7. Jak widać, Dodaj **treść** , która ma być jako danych wejściowych. W tym przykładzie wybierz treści żądania HTTP, którego dotyczy aplikacji logiczny. Możesz także wprowadzić wyrażenia do wprowadzania nagłówków w polu**nagłówki** :

    @triggerOutputs()['headers']

8. Dodać **nagłówki** , które są wymagane na potrzeby AS2. Te będą widoczne w nagłówku żądania HTTP. W tym przykładzie należy zaznaczać nagłówków żądania HTTP, którego dotyczy aplikacji logiczny.
9. Teraz Dodaj akcję wiadomości dekodowania X12 ponownie wybranie pozycji **Dodaj akcję**  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-9.png)   
10. W polu wyszukiwania wprowadź word **x12** w celu filtrowania wszystkie akcje do szablonu, który ma być używany  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-10.png)  
11. Wybierz pozycję **X12-dekodowanie X12 wiadomości** akcji, aby dodać go do aplikacji logicznych  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-as2message.png)  
12. Teraz musisz określić dane wejściowe do tej akcji, które będą wynik działania AS2 powyżej. Treść wiadomości rzeczywista jest w obiekcie JSON i jest kodowane base64. W związku z tym, musisz podać wyrażenia jako danych wejściowych dlatego należy wprowadzić następujące wyrażenie w polu wprowadzania **X12 PŁASKA plik wiadomości do dekodowania**  

    @base64ToString(body('Decode_AS2_message')?['AS2Message']?['Content'])  

13. W tym kroku zostanie dekodowanie X12 dane otrzymane od partnera handlowego i będzie wyjściowy liczba elementów w obiekcie JSON. Aby umożliwić partnera wiedzieć o otrzymaniu danych można wysłać ponownie odpowiedź zawierającą powiadomienie usuwania wiadomości (MDN) AS2, w akcji odpowiedzi HTTP  
14. Dodawanie akcji **odpowiedzi** , wybierając pozycję **Dodaj akcję**   
![](./media/app-service-logic-enterprise-integration-b2b/b2b-14.png)  
15. Wpisz wyraz **odpowiedź** w polu wyszukiwania w celu filtrowania wszystkie akcje do szablonu, który ma być używany  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-15.png)  
16. Wybierz akcję **odpowiedzi** , aby dodać go  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-16.png)  
17. Ustawianie pola **treści** odpowiedzi przy użyciu następującego wyrażenia do nich dostęp MDN z wynik działania **dekodowania X12 wiadomości**  

    @base64ToString(body('Decode_AS2_message')?['OutgoingMdn']?['Content'])  

![](./media/app-service-logic-enterprise-integration-b2b/b2b-17.png)  
18. Zapisywanie wyników pracy  
![](./media/app-service-logic-enterprise-integration-b2b/transform-5.png)  

W tym momencie po zakończeniu konfigurowania aplikacji logika B2B. W aplikacji rzeczywistych, warto przechowywać zdekodowana X12 danych w magazynie aplikacji lub dane LOB. Można łatwo dodawać dalsze działania, wykonaj następujące czynności i pisanie niestandardowe interfejsy API nawiązać połączenie z aplikacji LOB i wykonaj te interfejsy API w aplikacji logicznych.

## <a name="features-and-use-cases"></a>Funkcje i przypadków użycia ##

- AS2 i X12 dekodowanie i kodowanie akcje umożliwiają pobieranie danych z i wysyłanie danych do partnerów przy użyciu standardowych protokołów branżowe korzystanie z aplikacji logika handlowych  
- Umożliwia AS2 i X12 z lub bez innych elementach wymiana danych z partnerami handlowymi odpowiednio do potrzeb
- Akcje B2B ułatwiają tworzenie partnerów i umów na koncie integracji i używanie ich w aplikacji dla warunków logicznych  
- Wydłużając aplikacji logika z innymi działaniami można wysyłać i odbierać dane do i z innych aplikacji i usług, takich jak usługi SalesForce  

## <a name="learn-more"></a>Dowiedz się więcej ##

[Dowiedz się więcej o pakiecie integracji przedsiębiorstwa](./app-service-logic-enterprise-integration-overview.md)  