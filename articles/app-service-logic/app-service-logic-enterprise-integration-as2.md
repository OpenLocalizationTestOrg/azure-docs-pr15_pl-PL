<properties 
    pageTitle="Dowiedz się, jak utworzyć umowę AS2 pakietu integracji przedsiębiorstwa" 
    description="Dowiedz się, jak utworzyć umowę AS2 pakietu integracji przedsiębiorstwa | Microsoft Azure aplikacji usługi" 
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
    ms.date="06/29/2016" 
    ms.author="deonhe"/>

# <a name="enterprise-integration-with-as2"></a>Integracja przedsiębiorstwa z AS2

## <a name="create-an-as2-agreement"></a>Utwórz umowę AS2
Aby można było korzystać z funkcji przedsiębiorstwa w aplikacjach logiczny, należy najpierw utworzyć umów. 

### <a name="heres-what-you-need-before-you-get-started"></a>Poniżej opisano, co jest potrzebne przed rozpoczęciem
- [Integracja z programem konta](./app-service-logic-enterprise-integration-accounts.md) zdefiniowane w ramach subskrypcji Azure  
- Co najmniej dwa [partnerów](./app-service-logic-enterprise-integration-partners.md) już określonych na koncie integracji  

>[AZURE.NOTE]Podczas tworzenia umowę, zawartości w pliku umowy musi odpowiadać typ umowy.    


Po wprowadzeniu [utworzone konto integracji](./app-service-logic-enterprise-integration-accounts.md) i [dodane partnerów](./app-service-logic-enterprise-integration-partners.md), można utworzyć umowę, wykonując następujące czynności:  

### <a name="from-the-azure-portal-home-page"></a>Na stronie Azure głównej portalu

Po zalogowaniu się do [portalu Azure](http://portal.azure.com "Azure portal"):  
1. Z menu po lewej stronie wybierz pozycję **Przeglądaj** .  

>[AZURE.TIP]Łącze **Przejdź** nie jest widoczne, konieczne może być najpierw rozwinąć menu. W tym celu kliknąć łącze **Pokaż menu** , znajdującego się w lewym górnym rogu menu zwinięte.  

![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    
2. Wpisz *integracji* w polu wyszukiwania filtr, a następnie wybierz pozycję **Konta integracji** z listy wyników.       
 ![](./media/app-service-logic-enterprise-integration-overview/overview-2.png)  
3. W kartę **Konta integracji** , która zostanie otwarta w górę wybierz konto integracji, w którym ma zostać utworzony umowę. Jeśli nie widzisz integracji konta list, [utworzyć pierwszy](./app-service-logic-enterprise-integration-accounts.md "All about integration accounts").  
![](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  
4.  Wybierz Kafelek **umów** . Jeśli nie widzisz umów kafelka, dodaj go najpierw.   
![](./media/app-service-logic-enterprise-integration-agreements/agreement-1.png)   
5. Wybierz przycisk **Dodaj** na kartę umowy, która zostanie otwarta.  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-2.png)  
6. Wprowadź **nazwę** dla umowy, a następnie wybierz **Hosta partnera**, **Tożsamości hosta**, **Partnera gościa**, **Tożsamości gościa**w kartę umowy, która zostanie otwarta.  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-3.png)  

Oto kilka informacji, które mogą być przydatne podczas konfigurowania ustawień Umowie: 
  
|Właściwość|Opis|
|----|----|
|Partner hosta|Umowa musi partnera zarówno hosta i gościa. Partner hosta reprezentuje organizacji, która jest konfigurowanie umowę.|
|Tożsamość hosta|Identyfikator partnera hosta. |
|Partner gościa|Umowa musi partnera zarówno hosta i gościa. Partner gościa reprezentuje organizacji, która jest kontynuowanie współpracy z partnerem hosta.|
|Tożsamość gościa|Identyfikator partnera gościa.|
|Ustawienia odbierania|Te właściwości mają zastosowanie do wszystkich wiadomości odebranych przez Umowę|
|Wysyłanie ustawienia|Właściwości te dotyczą wszystkie wiadomości wysyłane przez Umowę|  
Przejdź:  
7. Wybierz **Ustawienia odbierania** Konfigurowanie sposobu obsługi wiadomości otrzymywane przy użyciu tej Umowy.  
 
 - Opcjonalnie można zmienić właściwości w wiadomości przychodzących. Aby to zrobić, zaznacz pole wyboru **Zastąp właściwości wiadomości** .
  - Zaznacz pole wyboru **powinny być podpisane wiadomości** , jeśli chcesz wymagać wszystkich przychodzących wiadomości do podpisania. Jeśli wybierzesz tę opcję, konieczne będzie zaznacz **certyfikat** , który będzie używany do sprawdzania poprawności Podpis w wiadomości.
  - Opcjonalnie można zażądać także szyfrowanie wiadomości. Aby to zrobić, zaznacz pole wyboru **mają zostać zaszyfrowane wiadomości** . Następnie należy wybierz **certyfikat** , który będzie używany do dekodowanie wiadomości przychodzących.
  - Możesz również wymagać wiadomości mają być kompresowany. Aby to zrobić, zaznacz pole wyboru **powinny być kompresowany wiadomości** .  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-4.png)  

Zobacz poniższą tabelę, jeśli chcesz dowiedzieć się więcej na temat jakie odbierania ustawienia Włącz.  

|Właściwość|Opis|
|----|----|
|Zastępowanie właściwości wiadomości|Zaznacz tę opcję, aby wskazać, że może zostać zastąpiona właściwości w otrzymanej wiadomości |
|Powinny być podpisane wiadomości|Włącz tę opcję, aby wymagać wiadomości podpisane cyfrowo|
|Mają zostać zaszyfrowane wiadomości|Włącz tę opcję, aby wymagać szyfrowanie wiadomości. Wiadomości zaszyfrowanych przez osoby, które nie będą odrzucane.|
|Powinny być kompresowany wiadomości|Włącz tę opcję, aby wymagać wiadomości mają być kompresowany. Skompresowany bez wiadomości zostaną odrzucone.|
|Tekst MDN|Jest to domyślne MDN były wysyłane do nadawcy wiadomości|
|Wysyłanie MDN|Włącz tę opcję, aby umożliwić MDNs do wysłania.|
|Wysyłanie podpisanej MDN|Włącz tę opcję, aby wymagać MDNs do podpisania.|
|Algorytm Mikrofonu||
|Wysyłanie asynchroniczne MDN|Włącz tę opcję, aby wymagać asynchroniczne przesyłanie wiadomości.|
|ADRES URL|To jest adres URL, do którego będą wysyłane wiadomości.|
Teraz przejdź:  
8. Wybierz **Ustawienia wysyłania** do konfigurowania sposobu obsługi wiadomości wysyłane za pośrednictwem niniejszej Umowy.  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-5.png)  

Zobacz poniższą tabelę, jeśli chcesz dowiedzieć się więcej na temat jakie Wyślij ustawienia Włącz.  

|Właściwość|Opis|
|----|----|
|Włączanie podpisywania wiadomości|Zaznacz to pole wyboru, aby włączyć wszystkie wiadomości wysyłane przez umowę do podpisania.|
|Algorytm Mikrofonu|Wybierz algorytm ma być używany w podpisywanie wiadomości|
|Certyfikat|Wybierz certyfikat do użycia w podpisywanie wiadomości|
|Włącz szyfrowanie wiadomości|Zaznacz to pole wyboru szyfrowanie wszystkich wiadomości wysłanych z tej Umowy.|
|Algorytm szyfrowania|Wybierz algorytm szyfrowania ma być używany w szyfrowanie wiadomości|
|Ujawniać nagłówki HTTP|Zaznaczenie tego pola wyboru ujawniać nagłówku typ zawartości HTTP w jednym wierszu.|
|Żądanie MDN|Włącz to pole wyboru zażądać MDN dla wszystkich wiadomości wysyłanych z niniejszej Umowy|
|Żądanie MDN podpisane|Włącz żądania, że korzystasz z MDNs wszystkie wysyłane do tej umowy|
|Żądanie asynchroniczne MDN|Włącz żądania asynchroniczne MDN były wysyłane do tej umowy|
|ADRES URL|Adres URL, do którego będą wysyłane MDNs|
|Włączanie NRR|Zaznacz to pole wyboru, aby włączyć Niemożność wyparcia potwierdzenia|
Firma Microsoft prawie gotowe!  
9. Wybierz Kafelek **umów** na karta Integracja konta, a zobaczysz umowę dodane na liście.  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-6.png)

