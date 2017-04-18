<properties
    pageTitle="Dowiedz się, jak kodowanie lub dekodowanie prostym pliki przy użyciu aplikacji Enterprise Integracja z dodatkiem Service Pack i logiki | Microsoft Azure aplikacji usługi | Microsoft Azure"
    description="Korzystanie z funkcji aplikacji Enterprise Integracja z dodatkiem Service Pack i logiki do kodowania lub dekodowanie plików prostych"
    services="app-service\logic"
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

# <a name="enterprise-integration-with-flat-files"></a>Integracja przedsiębiorstwa z plików prostych

## <a name="overview"></a>Omówienie

Być może zechcesz kodowanie zawartości XML przed wysłaniem go do partnera biznesowego w scenariuszu firma firma (B2B). Prosty plik kodowanie łącznik aplikacji dla logiki wprowadzone przez funkcję logiki aplikacji usługi aplikacji Azure umożliwia wykonaj następujące czynności. Logika aplikacja utworzony można pobrać jego XML zawartości z różnych źródeł, w tym z wyzwalacza żądania HTTP, z innej aplikacji lub nawet z jednej z wielu [łączników](../connectors/apis-list.md). Aby uzyskać więcej informacji na temat aplikacji logika zapoznaj się z [dokumentacji aplikacji logika](./app-service-logic-what-are-logic-apps.md "Dowiedz się więcej o aplikacjach logiczny").  

## <a name="how-to-create-the-flat-file-encoding-connector"></a>Jak utworzyć prosty plik kodowanie łącznika

Wykonaj poniższe czynności, aby dodać prosty plik kodowanie łącznika do aplikacji logicznych.

1. Tworzenie aplikacji logiki i [połączyć go z kontem integracji](./app-service-logic-enterprise-integration-accounts.md "Dowiedz się, aby połączyć konto integracji aplikacji logika"). To konto zawiera schemat, używanego do kodowania danych XML.  
2. Dodawanie wyzwalacza **żądanie — żądania HTTP po odebraniu** do aplikacji logika.  
![Zrzut ekranu przedstawiający wyzwalacza, aby zaznaczyć](./media/app-service-logic-enterprise-integration-flatfile/flatfile-1.png)    
3. Dodawanie pliku prostego kodowanie akcji, w następujący sposób:

    . Wybierz znak **plus** .

    b. Wybierz łącze **Dodaj akcję** (pojawia się po wybraniu znak plus).

    c. W polu wyszukiwania wpisz *prostym* do filtrowania wszystkie akcje do szablonu, który ma być używany.

    d. Wybierz opcję **Płaskie kodowanie pliku** z listy.   
![Zrzut ekranu przedstawiający prostym plik opcji kodowanie](./media/app-service-logic-enterprise-integration-flatfile/flatfile-2.png)   
4. W oknie dialogowym **Prostym kodowanie pliku** zaznacz pole tekstowe **zawartości** .  
![Zrzut ekranu przedstawiający zawartości pola tekstowego](./media/app-service-logic-enterprise-integration-flatfile/flatfile-3.png)  
5. Zaznacz znacznik treści jako zawartości, która ma zostać zakodowany. Znacznik Treść zostanie Wypełnij pole zawartości.     
![Zrzut ekranu przedstawiający znacznik treści](./media/app-service-logic-enterprise-integration-flatfile/flatfile-4.png)  
6. Zaznacz pole **Nazwa schematu** listy, a następnie wybierz schemat, który ma być używany do kodowania wprowadzania zawartości.    
![Zrzut ekranu Nazwa schematu pole listy](./media/app-service-logic-enterprise-integration-flatfile/flatfile-5.png)  
7. Zapisz swoją pracę.   
![Zrzut ekranu przedstawiający zapisywanie ikony](./media/app-service-logic-enterprise-integration-flatfile/flatfile-6.png)  

W tym momencie po zakończeniu konfigurowania tego łącznika kodowania pliku prostego. W aplikacji rzeczywistych może być przechowywane dane zakodowany w aplikacji programu LOB, takich jak usługi Salesforce. Lub można wysłać, że dane kodowane do obrotu partnera. Można łatwo dodawać do nich akcję wysłać dane wyjściowe Akcja kodowania usług Salesforce lub partnera handlowego, korzystając z jednej łączników, pod warunkiem.

Teraz możesz przetestować wybrany łącznik wybierając żądanie do punktu końcowego HTTP, a tym zawartości XML w treści wezwania.  

## <a name="how-to-create-the-flat-file-decoding-connector"></a>Jak utworzyć prosty plik dekodowanie łącznika

>[AZURE.NOTE] Aby wykonać te kroki, należy pliku schematu już przekazane do integracji konta.

1. Dodawanie wyzwalacza **żądanie — żądania HTTP po odebraniu** do aplikacji logicznych.  
![Zrzut ekranu przedstawiający wyzwalacza, aby zaznaczyć](./media/app-service-logic-enterprise-integration-flatfile/flatfile-1.png)    
2. Dodawanie pliku prostego dekodowanie akcji, w następujący sposób:

    . Wybierz znak **plus** .

    b. Wybierz łącze **Dodaj akcję** (pojawia się po wybraniu znak plus).

    c. W polu wyszukiwania wpisz *prostym* do filtrowania wszystkie akcje do szablonu, który ma być używany.

    d. Wybierz opcję **Płaskie dekodowanie pliku** z listy.   
![Zrzut ekranu przedstawiający prostym plik dekodowanie opcji](./media/app-service-logic-enterprise-integration-flatfile/flatfile-2.png)   
- Zaznacz kontrolkę **zawartości** . Opcja zapewnia listę zawartości z poprzednich krokach, które jako zawartość umożliwia dekodowanie. Zauważ, że *treść* z przychodzące żądanie HTTP jest dostępny do użycia jako zawartość odszyfrować. Można także wprowadzić zawartości do dekodowanie bezpośrednio do formantu **zawartości** .     
- Wybierz polecenie Oznakuj *treści* . Należy zauważyć, że tag treści znajduje się teraz w formant **zawartości** .
- Wybierz nazwę schematu, który ma być używany do dekodowanie zawartości. Następujące zrzucie ekranu pokazano, że *OrderFile* jest nazwą wybrany schemat. Ta nazwa schematu była wcześniej przekazane do konta usługi integracji.

 ![Zrzut ekranu przedstawiający prostym plik dekodowanie okno dialogowe](./media/app-service-logic-enterprise-integration-flatfile/flatfile-decode-1.png)    
- Zapisz swoją pracę.  
![Zrzut ekranu przedstawiający zapisywanie ikony](./media/app-service-logic-enterprise-integration-flatfile/flatfile-6.png)    

W tym momencie po zakończeniu konfigurowania pliku prostym dekodowanie łącznik. W aplikacji rzeczywistych można przechowywać zdekodowane dane w aplikacji z LOB, takich jak usługi Salesforce. Można łatwo dodawać akcji do wysłania wyniku działań dekodowania usług Salesforce.

Teraz możesz przetestować wybrany łącznik żądania HTTP punktu końcowego i łącznie z zawartości XML, który chcesz dekodowanie w treści wezwania.  

## <a name="next-steps"></a>Następne kroki
- [Dowiedz się więcej o pakiecie integracji przedsiębiorstwa] (./app-service-logic-enterprise-integration-overview.md "Więcej informacji na temat Enterprise Integracja z dodatkiem Service Pack").  
