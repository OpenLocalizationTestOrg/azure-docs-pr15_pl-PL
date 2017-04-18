<properties
   pageTitle="Samouczek: Procesu faktur EDIFACT przy użyciu usługi Azure BizTalk | Usługi BizTalk Microsoft Azure"
   description="Jak utworzyć i skonfigurować aplikację łącznika pola lub interfejsu API i używać go w aplikacji dla logiki Azure aplikacji usługi"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="msftman"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="05/31/2016"
   ms.author="deonhe"/>

# <a name="tutorial-process-edifact-invoices-using-azure-biztalk-services"></a>Samouczek: Proces EDIFACT faktury za pomocą usługi BizTalk Azure
Portal usługi BizTalk służy do konfigurowania i wdrażania X12 i EDIFACT umowy. W tym samouczku przyjrzymy się jak utworzyć umowę EDIFACT wymiany faktur między partnerami handlowymi. Ten samouczek jest zapisywany wokół rozwiązania biznesowego zakończenia do końca obejmujące dwóch partnerów handlowych Northwind i firmy Contoso, które wymiany EDIFACT wiadomości.  

## <a name="sample-based-on-this-tutorial"></a>Przykładowe według tego samouczka
Ten samouczek jest zapisywany wokół próbki, **Wysyłając EDIFACT faktury za pomocą BizTalk usług**, który jest dostępny do pobrania z [Galerii kodów MSDN](http://go.microsoft.com/fwlink/?LinkId=401005). Można użyć przykładowego i przejdź do tego samouczka, aby dowiedzieć się, jak próbki został utworzony. Można także użyć tego samouczka Tworzenie podstaw własne rozwiązanie. Druga metoda tego samouczka jest dobrany tak, aby dowiedzieć się, jak to rozwiązanie został utworzony. Ponadto możliwie, samouczka jest zgodna z próbki i używa tych samych nazw artefakty (na przykład schematów, przekształceń) jako używanych w próbce.  

>[AZURE.NOTE] Ponieważ to rozwiązanie polega na wysyłanie wiadomości z mostka EAI do mostka grupy użytkowników, ponownie próbce [mostka usług BizTalk łączenia próbki](http://code.msdn.microsoft.com/BizTalk-Bridge-chaining-2246b104) .  

## <a name="what-does-the-solution-do"></a>Do czego służy rozwiązanie?

W tym rozwiązaniu Northwind pobiera EDIFACT faktur z Contoso. Te faktury są w standardowym formacie lub grupy użytkowników. Tak przed wysłaniem faktury do Northwind, jego musi można przekształcić EDIFACT dokumentu faktury (zwanych również ZALICZKOWĄ). Po otrzymaniu Northwind musi procesu faktury EDIFACT i zwracać komunikat kontrolny (nazywane także CONTRL) do firmy Contoso.

![][1]  

Aby osiągnąć ten scenariusz firm, Contoso używa funkcji dostarczane z programem Microsoft Azure BizTalk Services.

*   Contoso umożliwia przekształcanie oryginalną fakturę ZALICZKOWĄ EDIFACT mosty EAI.

*   Mostek EAI wiadomość jest wysyłana do mostka Wyślij Edytuj wdrożony w ramach umowy skonfigurowane w portalu usługi BizTalk.

*   Edytuj mostka Wyślij przetwarza ZALICZKOWĄ EDIFACT i przekierowuje go do Northwind.

*   Po otrzymaniu faktury, zwraca Northwind CONTRL wiadomości do grupy użytkowników odbierane mostka wdrożony w ramach niniejszej Umowy.  

> [AZURE.NOTE] Opcjonalnie to rozwiązanie również przedstawiono sposób wysyłanie faktur partiami, zamiast wysyłania fakturami oddzielnie za pomocą tworzeniu partii.  

Do wykonania tego scenariusza, firma Microsoft korzysta z usługi Bus kolejkach Faktura z firmy Contoso do Northwind i odbieranie potwierdzenia z Northwind. Te kolejki mogą być tworzone przy użyciu aplikacji klienckiej, która jest dostępna do pobrania i znajduje się w pakiecie próbki, które są dostępne w ramach tego samouczka.  

## <a name="prerequisites"></a>Wymagania wstępne

*   Musi być nazw Bus usługi. Aby uzyskać instrukcje dotyczące tworzenia nazw, zobacz [jak: Tworzenie lub modyfikowanie Namespace usługi Bus usługi](https://msdn.microsoft.com/library/azure/hh674478.aspx). Załóżmy już przestrzeń nazw Bus usługi obsługi administracyjnej, o nazwie **edifactbts**.

*   Musisz mieć subskrypcję usługi BizTalk. Aby uzyskać instrukcje zobacz [Tworzenie usługi BizTalk za pomocą Azure portalu klasyczny](http://go.microsoft.com/fwlink/?LinkID=302280). Ten samouczek Powiadom nas założono, że masz subskrypcję usługi BizTalk, o nazwie **contosowabs**.

*   Zarejestruj swoją subskrypcję usługi BizTalk w portalu usługi BizTalk. Aby uzyskać instrukcje zobacz [Rejestrowanie BizTalk wdrażanie usługi w portalu usługi BizTalk](https://msdn.microsoft.com/library/hh689837.aspx)

*   Musi być Visual Studio zainstalowany.

*   Musi być BizTalk Services SDK zainstalowany. Zestaw SDK można pobrać z [http://go.microsoft.com/fwlink/?LinkId=235057](http://go.microsoft.com/fwlink/?LinkId=235057)  

## <a name="step-1-create-the-service-bus-queues"></a>Krok 1: Tworzenie kolejek Bus usługi  
To rozwiązanie wymaga usługi Bus kolejek do wymiany wiadomości między partnerami handlowymi. Contoso i Northwind wysyłać wiadomości do kolejek, z której mosty EAI i/lub Edytuj używanie ich. Dla tego rozwiązania jest potrzebny trzy kolejki Bus usługi:

*   **northwindreceive** — Northwind otrzymuje faktury z firmy Contoso w kolejce.

*   **contosoreceive** — Contoso otrzyma potwierdzenie z Northwind na niej.

*   **zawieszone** — wszystkie zawieszone wiadomości są przesyłane do niej. Wiadomości są zawieszone, jeżeli nie wpiszą podczas przetwarzania.

Te kolejki Bus usługi można utworzyć przy użyciu aplikacji klienckiej zawarte w pakiecie próbki.  

1.  Korzystając z lokalizacji, w której są pobierane próbki Otwórz **Samouczek wysłaniem faktury za pomocą BizTalk usług Edytuj Bridges.sln**.

2.  Naciśnij klawisz **F5** , aby utworzyć i uruchomić aplikację **Samouczek klienta** .

3.  Na ekranie wprowadź nazw usługi ACS Bus, nazwa wystawcy i klucza wystawcy.

    ![][2]  
4.  W oknie komunikatu zostanie wyświetlony monit, że trzy kolejki zostanie utworzony w obszarze nazw Bus usługi. Kliknij **przycisk OK**.

5.  Pozostaw klient samouczek uruchomiony. Otwieranie kliknij pozycję **Usługa Bus** > **_obszaru nazw Bus usługi_** > **kolejek**i sprawdź, czy są utworzone trzy kolejki.  

## <a name="step-2-create-and-deploy-trading-partner-agreement"></a>Krok 2: Tworzenie i wdrażanie handlowych Umowy partnera
Utwórz umowę partnerów handlowych między Contoso i Northwind. Umowę partnerów handlowych określa umowy handlu między partnerami dwóch firm, takich jak które schematu wiadomości, aby użyć, który protokół wiadomości za pomocą itd. Umowę partnerów handlowych zawiera dwa mosty grupy użytkowników, do wysyłania wiadomości do partnerów handlowych (nazywanych **Wysyłanie Edytuj mostka**) i jeden-do-otrzymywania wiadomości od partnerów handlowych (nazywanych **mostka odbieranie grupy użytkowników**).

W ramach tego rozwiązania mostka Wyślij Edytuj odpowiada stronie Wyślij układu i służy do wysyłania faktury EDIFACT z Contoso do Northwind. Podobnie mostka Odbierz Edytuj odpowiada stronie Odbierz układu i jest używany do odbierania potwierdzeń z Northwind.  

### <a name="create-the-trading-partners"></a>Tworzenie partnerów handlowych

Tworzenie zaczynać się partnerów handlowych firmy Contoso i Northwind.  

1.  W portalu usługi BizTalk na karcie **partnerów** kliknij przycisk **Dodaj**.

2.  Na stronie Nowa partnera wprowadź nazwy partnera **firmy Contoso** , a następnie kliknij **Zapisz**.

3.  Powtórz krok, aby utworzyć drugą partnera, **Northwind**.  

### <a name="create-the-agreement"></a>Tworzenie umowę
Między profile biznesowych partnerów handlowych tworzenia handlowych umów partnera. To rozwiązanie wymaga profile partnera domyślne, które są tworzone automatycznie po utworzonych partnerów.  

1.  W portalu usługi BizTalk kliknij **umów** > **Dodaj**.

2.  Na stronie **Ustawienia ogólne** nowej umowy określ wartości, jak pokazano na poniższej ilustracji, a następnie kliknij przycisk **Kontynuuj**.

    ![][3]  

    Kliknij przycisk **Kontynuuj**, karty **Ustawienia odbieranie** i **Wysyłanie** stanie się dostępna.

3.  Utwórz umowę Wyślij między Contoso i Northwind. Niniejsza Umowa decyduje o tym, jak Contoso wyśle odbiorcy faktury EDIFACT Northwind.

    1.  Kliknij przycisk **Wyślij ustawienia**.

    2.  Zachowanie domyślne wartości na kartach **Ruch przychodzący adres URL**, **Przekształcanie**i **Batching** .

    3.  Na karcie **Protokół** w sekcji **Schematy** Przekaż schematu **EFACT_D93A_INVOIC.xsd** . Ten schemat jest dostępna z pakietem próbki.

        ![][4]  
    4.  Na karcie **Transport** Określ szczegóły kolejek Bus usługi. Umowę stronie Wyślij używamy w kolejce **northwindreceive** faktura Northwind jest EDIFACT i kolejkę **wstrzymanych** , aby skierować wszystkie wiadomości, które kończą się niepowodzeniem podczas przetwarzania i są zawieszone. Utworzono tych kolejek w **Krok 1: tworzenie kolejek usługi Bus** (w tym temacie).

        ![][5]  

        W obszarze **Ustawienia transportu > transportu typu** i **Ustawienia zawieszenia wiadomości > transportu typu**, wybierz Bus usługi Azure i podaj wartości, jak pokazano na ilustracji.

4.  Utwórz umowę Odbierz między Contoso i Northwind. Niniejsza Umowa decyduje o tym, jak Contoso otrzyma potwierdzenie od Northwind.

    1.  Kliknij pozycję **Ustawienia odbierania**.

    2.  Zachowanie domyślne wartości na kartach **transportu** i **Przekształcanie** .

    3.  Na karcie **Protokół** w sekcji **Schematy** Przekaż schematu **EFACT_4.1_CONTRL.xsd** . Ten schemat jest dostępna z pakietem próbki.

    4.  Na karcie **rozsyłania** utworzyć filtr, aby upewnić się, że tylko potwierdzeń z Northwind są przesyłane do firmy Contoso. W obszarze **Ustawienia rozsyłania**kliknij przycisk **Dodaj** , aby utworzyć filtr routingu.

        ![][6]  
        1.  **Nazwa reguły**, **reguły rozsyłania**i **miejsce docelowe rozsyłania** należy podać wartości, jak pokazano na ilustracji.

        2.  Kliknij przycisk **Zapisz**.

    5.  Na karcie **rozsyłania** ponownie, określ, gdzie zawieszone potwierdzenia (potwierdzenia, których nie powiodło się podczas przetwarzania) są kierowane do. Ustaw typ transportu Bus usługi Azure, rozsyłanie docelowy typ **kolejki**, typ uwierzytelniania do **Udostępnionych podpis programu Access** (SA), odpowiednie parametry połączenia skojarzeń zabezpieczeń dotyczące nazw Bus usługi, a następnie wprowadź nazwę kolejki jako **zawieszone**.

5.  Na koniec kliknij przycisk **Deploy** wdrożenia umowy. Uwaga punkty końcowe miejsce, w którym wysyłanie i odbieranie są wdrażane umów.

    *   Na karcie **Ustawienia wysyłania** w obszarze **Adres URL ruchu przychodzącego**Uwaga punkt końcowy. Aby wysłać wiadomość z Contoso do Northwind przy użyciu mostka Wyślij grupy użytkowników, możesz wysłać wiadomość do tego punktu końcowego.

    *   Na karcie **Ustawienia odbierania** w obszarze **transportu**Uwaga punkt końcowy. Aby wysłać wiadomość z Northwind do firmy Contoso za pomocą Edytuj odbieranie mostka, musisz wysłać wiadomość do tego punktu końcowego.  

## <a name="step-3-create-and-deploy-the-biztalk-services-project"></a>Krok 3: Tworzenie i wdrażanie usług BizTalk projektu

W poprzednim kroku możesz wdrożyć Wyślij grupy użytkowników i odbierać umów przetwarzania EDIFACT faktur i potwierdzenia. Te umowy można tylko wiadomości procesów zgodnych ze standardowego schematu EDIFACT w wiadomości. Jednak dla tego scenariusza dla tego rozwiązania Contoso wyśle odbiorcy faktury Northwind w schemacie własnych we własnym zakresie. Tak zanim wiadomość jest wysyłana do mostka Wyślij grupy użytkowników, go musi można przekształcić w schemacie we własnym zakresie standardowy schematu faktury EDIFACT. Projekt EAI usług BizTalk robi to.

W projekcie usług BizTalk, **InvoiceProcessingBridge**, przekształcenia wiadomości jest również częścią pobranego próbki. Projekt obejmuje następujące artefakty:

*   **INHOUSEINVOICE. XSD** — schematu we własnym zakresie faktury, które są wysyłane do Northwind.

*   **EFACT_D93A_INVOIC. XSD** — schemat standardowy faktury EDIFACT.

*   **EFACT_4.1_CONTRL. XSD** — schematu EDIFACT potwierdzenia, że Northwind wysyła do firmy Contoso.

*   **INHOUSEINVOICE_to_D93AINVOIC. TRFM** — transformację mapy w schemacie we własnym zakresie faktury do standardowego schematu EDIFACT faktury.  

### <a name="create-the-biztalk-services-project"></a>Tworzenie projektu usług BizTalk
1.  W rozwiązania programu Visual Studio rozwiń projektu InvoiceProcessingBridge, a następnie otwórz plik **MessageFlowItinerary.bcs** .

2.  Kliknij dowolne miejsce w obszarze roboczym i ustaw **Adres URL usługi BizTalk** w polu właściwości, aby określić nazwę subskrypcji usługi BizTalk. Na przykład `https://contosowabs.biztalk.windows.net`.

    ![][7]  
3.  Z przybornika przeciągnij **Mostka One-Way Xml** do obszaru roboczego. Ustawianie właściwości **Obiektu nazwa** i **Adres względne** mostka do **ProcessInvoiceBridge**. Kliknij dwukrotnie **ProcessInvoiceBridge** , aby otworzyć powierzchni konfiguracji mostka.

4.  W polu **Typy wiadomości** , kliknij przycisk plus (**+**) przycisk, aby określić schemat wiadomości przychodzących. Ponieważ wiadomości przychodzącej mostka EAI jest zawsze we własnym zakresie faktury, Ustaw to **INHOUSEINVOICE**.

    ![][8]  
5.  Kliknij kształt, **Przekształcenie Xml** , a następnie w polu właściwości dla właściwości **mapy** , kliknij przycisk wielokropka (****...). W oknie dialogowym **Wybór mapy** wybierz plik transformacji **INHOUSEINVOICE_to_D93AINVOIC** , a następnie kliknij **przycisk OK**.

    ![][9]  
6.  Wróć do **MessageFlowItinerary.bcs**i z przybornika, przeciągnij **Punkt końcowy usługi zewnętrzne Two-Way** po prawej stronie **ProcessInvoiceBridge**. Ustaw właściwości **Obiektu nazwa** **EDIBridge**.

7.  W Eksploratorze rozwiązań rozwiń **MessageFlowItinerary.bcs** i kliknij dwukrotnie plik **EDIBridge.config** . Zamień zawartość **EDIBridge.config** następujące czynności.

    > [AZURE.NOTE] Dlaczego trzeba edytować plik .config? Punkt końcowy usługi zewnętrzne, dodawany do obszaru roboczego projektanta mostka reprezentuje mosty grupy użytkowników, które wcześniej wdrożone. Edytuj mosty są dwukierunkowe mosty, przy użyciu Wyślij i odbierać strony. Jednak mostka EAI dodawany do projektanta mostka jest jednokierunkowa mostka. Tak powinien obsługiwać wzorców programu exchange innej wiadomości dwóch mosty, korzystamy zachowanie niestandardowych mostka, dołączając konfigurację w pliku .config. Ponadto niestandardowej zachowania obsługuje również uwierzytelniania do punktu końcowego mostka Wyślij grupy użytkowników. To zachowanie niestandardowe jest dostępny jako osobne próbki [mostka usług BizTalk łączenia próbka - EAI do grupy użytkowników](http://code.msdn.microsoft.com/BizTalk-Bridge-chaining-2246b104). To rozwiązanie ponownie próbki.  
    
    ```
<?xml version="1.0" encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <extensions>
          <behaviorExtensions>
            <add name="BridgeAuthentication" 
                  type="Microsoft.BizTalk.Bridge.Behaviour.BridgeBehaviorElement, Microsoft.BizTalk.Bridge.Behaviour, Version=1.0.0.0, Culture=neutral, PublicKeyToken=ae58f69b69495c05" />
          </behaviorExtensions>
        </extensions>
        <behaviors>
          <endpointBehaviors>
            <behavior name="BridgeAuthenticationConfiguration">
              <!-- Enter the ACS namespace, issuer name and issuer secret of the BizTalk Services deployment -->
              <BridgeAuthentication acsnamespace="[YOUR ACS NAMESPACE]" 
                                    issuername="owner" 
                                    issuersecret="[YOUR ACS SECRET]" />
              <webHttp />
            </behavior>
          </endpointBehaviors>
        </behaviors>
        <bindings>
          <webHttpBinding>
            <binding name="BridgeBindingConfiguration">
              <security mode="Transport" />
            </binding>
          </webHttpBinding>
        </bindings>
        <client>
          <clear />
          <!--
            Go BizTalk Portal > Agreement > Send Settings > Inbound URL
            Copy the Endpoint URL and paste it in the below address field
          -->
          <endpoint name="TwoWayExternalServiceEndpointReference1" 
                    address="[YOUR EDI BRIDGE SEND URI]" 
                    behaviorConfiguration="BridgeAuthenticationConfiguration" 
                    binding="webHttpBinding" 
                    bindingConfiguration="BridgeBindingConfiguration" 
                    contract="System.ServiceModel.Routing.IRequestReplyRouter" />
        </client>
      </system.serviceModel>
    </configuration>

    ```
8.  Zaktualizować plik EDIBridge.config, aby uwzględnić szczegóły konfiguracji

    *   W obszarze _<behaviors>_, podaj ACS nazw i klucza skojarzonego z subskrypcją usługi BizTalk.

    *   W obszarze _<client>_, podaj punkt końcowy miejsce, w którym zostanie wdrożony umowę Wyślij grupy użytkowników.

    Zapisz zmiany i zamknij plik konfiguracji.

9.  Z przybornika kliknij **Łącznik** i dołączać składniki **ProcessInvoiceBridge** i **EDIBridge** . Zaznacz łącznik, a następnie w polu właściwości ustaw **Warunek filtru** **Dopasuj wszystkie**. Dzięki temu, że wszystkie wiadomości według mostka EAI są kierowane do mostka grupy użytkowników.

    ![][10]  
10.  Zapisz wprowadzone zmiany rozwiązanie.  

### <a name="deploy-the-project"></a>Wdrażanie projektu

1.  Na komputerze, na której utworzono projektu usług BizTalk Pobierz i zainstaluj certyfikat SSL dla subskrypcji usługi BizTalk. Kliknij pozycję **pulpit nawigacyjny**w obszarze usługi BizTalk, a następnie kliknij **Pobierz certyfikat SSL**. Kliknij dwukrotnie certyfikat i wykonaj monit, aby ukończyć instalację. Upewnij się, że zainstalować certyfikat w magazynie certyfikatów **Zaufane główne urzędy certyfikacji** .

2.  W Eksploratorze rozwiązań programu Visual Studio kliknij prawym przyciskiem myszy **InvoiceProcessingBridge** projektu, a następnie kliknij **Rozmieszczanie**.

3.  Zawierają wartości, jak pokazano na ilustracji, a następnie kliknij przycisk **Deploy**. Poświadczenia ACS usługi BizTalk można uzyskać, klikając pozycję **Informacje o połączeniu** z usługami BizTalk pulpitu nawigacyjnego.

    ![][11]  

    W okienku wynik, skopiuj punkt końcowy, gdzie mostka EAI zostanie wdrożony, na przykład `https://contosowabs.biztalk.windows.net/default/ProcessInvoiceBridge`. Ten adres URL punktu końcowego jest potrzebna później.  

## <a name="step-4-test-the-solution"></a>Krok 4: Testowanie rozwiązanie


W tym temacie przyjrzymy się opisano, jak sprawdzić rozwiązanie przy użyciu aplikacji **Samouczek klienta** w ramach próbki.  

1.  W programie Visual Studio naciśnij klawisz F5, aby uruchomić **Samouczek klienta**.

2.  Ekran musi być wartości wstępnie z kroku, gdzie utworzonych kolejek Bus usługi. Kliknij przycisk **Dalej**.

3.  W następnym oknie podanie poświadczeń ACS dla subskrypcji usługi BizTalk i punkty końcowe miejsce, w którym EAI i Edytuj (odbierania) są rozmieszczane mosty.

    Punkt końcowy mostka EAI skopiowany w poprzednim kroku. Edytuj otrzymuje końcowego mostka w portalu usługi BizTalk, przejdź do układu > Ustawienia odbierania > transportu > punkt końcowy.

    ![][12]  
4.  W następnym oknie w obszarze Contoso, kliknij przycisk **Wyślij fakturę we własnym zakresie** . W pliku okno dialogowe, otwórz plik INHOUSEINVOICE.txt. Sprawdzenie zawartości pliku, a następnie kliknij **przycisk OK** , aby wysłać faktury.

    ![][13]  
5.  W ciągu kilku sekund faktury są odbierane w Northwind. Kliknij łącze **Wyświetl wiadomość** , aby wyświetlić fakturę odebrana przez Northwind. Zwróć uwagę, jak faktury otrzymanej przez Northwind znajduje się w standardowej schematu EDIFACT podczas przesłaną przez Contoso jest schematu we własnym zakresie.

    ![][14]  
6.  Wybierz fakturę, a następnie kliknij pozycję **Wyślij potwierdzenia**. W oknie dialogowym, które pojawia się Zwróć uwagę, że identyfikator wymiany jest taka sama we otrzymanej faktury i potwierdzenia wysyłana. Kliknij przycisk OK w oknie dialogowym **Wysyłanie potwierdzenia** .

    ![][15]  
7.  W ciągu kilku sekund pomyślnie otrzymania potwierdzenia informatyczny firmy.

    ![][16]  

## <a name="step-5-optional-send-edifact-invoice-in-batches"></a>Krok 5 (opcjonalnie): wysyłanie EDIFACT faktury partiami 
Edytuj usług BizTalk mosty obsługuje także tworzeniu partii wiadomości wychodzących. Ta funkcja jest przydatne w przypadku otrzymywania partnerów, którzy preferują otrzymywanie partię wiadomości (spełniających określone kryterium) zamiast poszczególnych wiadomości.

Najważniejszym aspektem podczas pracy z partii jest rzeczywista wersją partii, nazywany kryteria wersji. Kryteria wersji mogą być oparte na jak partner-odbiorca chce otrzymywać wiadomości. Po włączeniu tworzeniu partii mostka Edytuj nie wysyła wiadomości wychodzącej do odbierania partnera do momentu wydania kryteria są spełnione. Na przykład łączenia we wsady kryteriów na podstawie wysyłki rozmiar wiadomości partię tylko wtedy, gdy "n" jest przetwarzany wsadowo wiadomości. Kryteriów partia można także opartych na czasie tak, aby partię są wysyłane jednocześnie stały każdego dnia. W tym rozwiązaniu próbie kryteria podstawie rozmiar wiadomości.

1.  W portalu usługi BizTalk kliknij umowę, utworzony wcześniej. Kliknij pozycję Wyślij Ustawienia > tworzeniu partii > Dodaj partię.

2.  W polu Nazwa partii wprowadź **InvoiceBatch**, podaj opis i kliknij przycisk **Dalej**.

3.  Określanie kryteriów partia, określające, które wiadomości należy przetwarzany wsadowo. W tym rozwiązaniu możemy partii wszystkich wiadomości. Tak zaznacz pole wyboru Użyj opcji definicje zaawansowanej, a następnie wprowadź wartość **1 = 1**. Jest to stan, który będzie zawsze wartość PRAWDA, a więc wszystkie wiadomości będzie się przetwarzany wsadowo. Kliknij przycisk **Dalej**.

    ![][17]  
4.  Określanie kryteriów wersji partię. W polu listy zaznacz **MessageCountBased**, a **Liczba**, określ **3**. Oznacza to, że partia trzy wiadomości będą wysyłane do Northwind. Kliknij przycisk **Dalej**.

    ![][18]  
5.  Zapoznaj się z podsumowaniem, a następnie kliknij przycisk **Zapisz**. Kliknij pozycję **Rozmieść** ponownie rozmieścić umowę.

6.  Wróć do **Samouczka klienta**, kliknij pozycję **Wyślij fakturę we własnym zakresie**i postępuj zgodnie z instrukcjami, aby wysłać faktury. Można zauważyć, że nie faktury są odbierane w Northwind, ponieważ rozmiar partii nie są spełnione. Powtórz ten krok dwa razy, dzięki czemu będziesz mieć trzy faktury wiadomości wysłane do Northwind. Spełnia on kryteria wprowadzenia partii 3 wiadomości i powinien zostać wyświetlony faktury na Northwind.


<!--Image references-->
[1]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-1.PNG  
[2]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-2.PNG  
[3]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-3.PNG
[4]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-4.PNG  
[5]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-5.PNG  
[6]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-6.PNG  
[7]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-7.PNG  
[8]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-8.PNG
[9]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-9.PNG  
[10]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-10.PNG  
[11]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-11.PNG  
[12]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-12.PNG  
[13]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-13.PNG
[14]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-14.PNG  
[15]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-15.PNG  
[16]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-16.PNG  
[17]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-17.PNG  
[18]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-18.PNG

