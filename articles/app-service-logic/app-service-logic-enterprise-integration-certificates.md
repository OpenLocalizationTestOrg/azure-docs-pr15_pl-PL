
<properties
    pageTitle="Za pomocą certyfikatów pakietem integracji przedsiębiorstwa | Microsoft Azure"
    description="Informacje o sposobie korzystania z certyfikatów z Enterprise Integracja z dodatkiem Service Pack i aplikacje warunków logicznych"
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
    ms.date="09/06/2016"
    ms.author="deonhe"/>

# <a name="learn-about-certificates-and-enterprise-integration-pack"></a>Dowiedz się więcej o certyfikaty i Enterprise Integracja z dodatkiem Service Pack

## <a name="overview"></a>Omówienie
Integracja przedsiębiorstwa używa certyfikatów do zabezpieczania komunikacji B2B. Dwa typy certyfikatów można użyć w swojej aplikacji integracji przedsiębiorstwa:

- Certyfikaty publiczne, które muszą być zakupione od urzędu certyfikacji (CA).
- Prywatne certyfikatów, które można wydać samodzielnie. Te certyfikaty są czasami określane jako certyfikatów z podpisem własnym.


## <a name="what-are-certificates"></a>Co to są certyfikaty?
Certyfikaty są dokumentów cyfrowych, który zweryfikowania tożsamości uczestników łączności elektronicznej i które również bezpieczna komunikacja elektronicznych.

## <a name="why-use-certificates"></a>Dlaczego warto używać certyfikatów?
Czasami komunikacji B2B musi poufne. Integracja przedsiębiorstwa używa certyfikatów do zabezpieczania komunikacji te na dwa sposoby:

- Przez zaszyfrowanie treści wiadomości
- Przez cyfrowe podpisywanie wiadomości  

## <a name="how-do-you-upload-certificates"></a>Jak przekazać certyfikaty?

### <a name="public-certificates"></a>Certyfikaty publicznych
Aby użyć *certyfikatu publicznego* w logiczny aplikacji z funkcjami B2B, najpierw należy przekazać certyfikatu do konta usługi integracji. Używanie *certyfikatu z podpisem własnym*, z drugiej strony, należy najpierw wysłaniu [Azure klucza magazynu](../key-vault/key-vault-get-started.md "więcej informacji na temat klucza magazynu").

Po przekazaniu certyfikatu jest dostępna ułatwiające zabezpieczanie wiadomości B2B podczas definiowania ich właściwości w [umów](./app-service-logic-enterprise-integration-agreements.md) , które są tworzone.  

Poniżej przedstawiono szczegółowe informacje na temat przekazywania publicznej certyfikatów do konta usługi integracji, po zalogowaniu się do portalu Azure:

1. Wybierz pozycję **Przeglądaj**.  
    ![Kliknij pozycję Przeglądaj](./media/app-service-logic-enterprise-integration-overview/overview-1.png)  

2. Wprowadź **integracji** w polu wyszukiwania filtr, a następnie wybierz **Konta integracji** z listy wyników.     
    ![Wybierz pozycję konta integracji](./media/app-service-logic-enterprise-integration-overview/overview-2.png)

3. Wybierz konto integracji, do którego chcesz dodać certyfikatu.  
    ![Wybierz konto integracji, do którego chcesz dodać certyfikatu](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  

4.  Wybierz Kafelek **Certyfikaty** .  
    ![Wybierz Kafelek certyfikatów](./media/app-service-logic-enterprise-integration-certificates/certificate-1.png)

5. W kartę **Certyfikaty** , która zostanie otwarta kliknij przycisk **Dodaj** .
    ![Kliknij przycisk Dodaj](./media/app-service-logic-enterprise-integration-certificates/certificate-2.png)

6. Wprowadź **nazwę** dla certyfikatu, a następnie wybierz odpowiedni typ certyfikatu. (W tym przykładzie użyliśmy typ certyfikatu publicznego.) Następnie wybierz ikonę folderu po prawej stronie pola tekstowego **certyfikatu** .

7. Po otwarciu selektora pliku, Znajdź i wybierz plik certyfikatu, który chcesz przekazać do swojego konta integracji.

8. Wybierz certyfikat, a następnie wybierz **przycisk OK** w selektorze pliku. To sprawdza i przekazywanie certyfikatu do swojego konta integracji.

8. Na koniec ponownie na karta **Dodaj certyfikatu** , wybierz przycisk **OK** .  
    ![Wybierz przycisk OK](./media/app-service-logic-enterprise-integration-certificates/certificate-3.png)  

9. W około minuty, zobaczysz powiadomienie, które oznacza, że przekazywanie certyfikatu jest pełny.

10. Wybierz Kafelek **Certyfikaty** . Powinien zostać wyświetlony nowo dodany certyfikat.  
    ![Zobacz nowego certyfikatu.](./media/app-service-logic-enterprise-integration-certificates/certificate-4.png)  

### <a name="private-certificates"></a>Certyfikaty prywatnych
Możesz przekazać prywatne certyfikaty do konta usługi integracji, wykonując następujące czynności:  

1. [Przekazywanie klucz prywatny do magazynu klucza] (../key-vault/key-vault-get-started.md "Dowiedz się więcej o najważniejszych magazynu").  

    > [AZURE.TIP] Należy zezwolić funkcji aplikacji logika Azure aplikacji usługi do wykonywania operacji na klucz magazynu. Za udzielić dostępu i wystawcy usługi logiki aplikacji przy użyciu następującego polecenia programu PowerShell:`Set-AzureRmKeyVaultAccessPolicy -VaultName 'TestcertKeyVault' -ServicePrincipalName '7cd684f4-8a78-49b0-91ec-6a35d38739ba' -PermissionsToKeys decrypt, sign, get, list`  

2. Tworzenie certyfikatu z prywatne.  

3. Przekaż certyfikatów prywatnych do swojego konta integracji.

Po podjęciu powyższe kroki, umożliwia tworzenie umów przez osoby prywatne certyfikatu.

Poniżej przedstawiono szczegółowe informacje na temat przekazywania prywatnych certyfikatów do konta usługi integracji, po zalogowaniu się do portalu Azure:  

1. Wybierz pozycję **Przeglądaj**.  
    ![Przekaż prywatnych certyfikatów do konta usługi integracji](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    

2. Wprowadź **integracji** w polu wyszukiwania filtr, a następnie wybierz **Konta integracji** z listy wyników.     
    ![Wybierz pozycję konta integracji](./media/app-service-logic-enterprise-integration-overview/overview-2.png)  

3. Wybierz konto integracji, do którego chcesz dodać certyfikatu.  
    ![Wybierz konto integracji, do którego chcesz dodać certyfikatu](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  

4. Wybierz Kafelek **certyfikatów** .  
    ![Wybierz Kafelek certyfikatów](./media/app-service-logic-enterprise-integration-certificates/certificate-1.png)  

5. W kartę **Certyfikaty** , która zostanie otwarta kliknij przycisk **Dodaj** .
    ![Kliknij przycisk Dodaj](./media/app-service-logic-enterprise-integration-certificates/certificate-2.png)

6. Wprowadź **nazwę** dla certyfikatu, a następnie wybierz odpowiedni typ certyfikatu. (W tym przykładzie użyliśmy typ certyfikatu publicznego.) Następnie wybierz ikonę folderu po prawej stronie pola tekstowego **certyfikatu** .

7. Po otwarciu selektora pliku, Znajdź i wybierz plik certyfikatu, który chcesz przekazać do swojego konta integracji.

8. Po wybraniu certyfikatu, wybierz **przycisk OK** w selektorze pliku. Ta akcja sprawdza certyfikat i przekazuje go do swojego konta integracji.

9. Na koniec ponownie na karta **Dodaj certyfikatu** , wybierz przycisk **OK** .  
    ![Dodawanie certyfikatu](./media/app-service-logic-enterprise-integration-certificates/privatecertificate-1.png)  

10. W około minuty, zobaczysz powiadomienie, które oznacza, że przekazywanie certyfikatu jest pełny.

11. Wybierz Kafelek **Certyfikaty** . Powinien zostać wyświetlony nowo dodany certyfikat.
    ![Zobacz nowego certyfikatu.](./media/app-service-logic-enterprise-integration-certificates/privatecertificate-2.png)  

Po przekazaniu certyfikatu jest dostępna ułatwiające zabezpieczanie wiadomości B2B podczas definiowania ich właściwości w [układach](./app-service-logic-enterprise-integration-agreements.md).  

## <a name="next-steps"></a>Następne kroki
- [Tworzenie aplikacji logiki, która korzysta z funkcji B2B](./app-service-logic-enterprise-integration-b2b.md)  
- [Utwórz umowę B2B](./app-service-logic-enterprise-integration-agreements.md)  
- [Dowiedz się więcej o klucza magazynu] (../key-vault/key-vault-get-started.md "Dowiedz się więcej o najważniejszych magazynu")  
