<properties 
    pageTitle="Aktualizowanie usługi multimediów po stopniowych klawiszy dostępu miejsca do magazynowania | Microsoft Azure" 
    description="W tym artykule stanowić wskazówki dotyczące aktualizacji usługi multimediów po stopniowych klawiszy dostępu miejsca do magazynowania." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako"
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="milangada;cenkdin;juliako"/>

#<a name="update-media-services-after-rolling-storage-access-keys"></a>Aktualizowanie usługi multimediów po stopniowych klawiszy dostępu miejsca do magazynowania

Podczas tworzenia nowego konta usługi multimediów Azure również musiał wybierz konto Azure miejsca do magazynowania, który służy do przechowywania zawartości multimedialnej. Należy zauważyć, że można [dodać więcej niż jednego konta miejsca do magazynowania](meda-services-managing-multiple-storage-accounts.md) do swojego konta usługi multimediów.

Po utworzeniu nowego konta magazynu platformy Azure generuje dwa miejsca do magazynowania 512-bitowej klawiszy dostępu, które służą do uwierzytelniania dostępu do konta miejsca do magazynowania. Połączenia miejsca do magazynowania zwiększyć bezpieczeństwo, zaleca się okresowo ponownie wygenerować i obracanie klucza dostępu miejsca do magazynowania. Dwóch klawiszy dostępu (głównego i pomocniczego) znajdują się w celu umożliwienia można obsługiwać połączenia z kontem miejsca do magazynowania przy użyciu jednego klucza dostępu, podczas Generuj kod dostępu. Ta procedura jest nazywany "stopniowe klawiszy dostępu".

Usługi multimediów zależy od klawisza miejsca do magazynowania przekazane. W szczególności Locator, używane do strumienia lub pobrać aktywów zależą od określonego miejsca do magazynowania klawisz dostępu. Po utworzeniu konta AMS trwa zależność na klucz podstawowy programu access domyślnie, ale jako użytkownik możesz zaktualizować klucz miejsca do magazynowania, który ma AMS. Należy się upewnić umożliwić usługi multimediów wiesz, którego klucz za pomocą wykonując kroki opisane w tym temacie. Ponadto wdrażania klawiszy dostępu miejsca do magazynowania, musisz upewnij się, aby zaktualizować usługi Locator nie będą bez przerwy w usłudze przesyłanie strumieniowe (w tym kroku opisana w tym temacie).

>[AZURE.NOTE]Jeśli masz wiele kont miejsca do magazynowania, może wykonać tę procedurę z każdego miejsca do magazynowania konta.
>
>Przed wykonaniem kroków opisanych w tym temacie na koncie produkcji, upewnij się przetestować łącza na koncie przygotowań.


## <a name="step-1-regenerate-secondary-storage-access-key"></a>Krok 1: Generuj magazyn pomocniczy klawisz dostępu

Uruchom ponownego generowania klucza magazyn pomocniczy. Domyślnie klucz pomocnicza nie jest używany przez usługi multimediów.  Aby uzyskać informacje dotyczące sposobu użycia klawiszy miejsca do magazynowania, zobacz [jak: widok, Kopiuj i wyniku przestrzeni dyskowej, klawiszami dostępu](../storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).
  
##<a id="step2"></a>Krok 2: Aktualizacji usługi multimediów za pomocą nowego klucza magazyn pomocniczy

Aktualizowanie usługi multimediów za pomocą klucza dostępu magazyn pomocniczy. Jedną z dwóch poniższych metod umożliwia synchronizowanie klucza regenerowanej magazynowania danych z usługi multimediów.

- Azure portal za pomocą: Aby znaleźć nazwę i klucz wartości, przejdź do portalu Azure, a następnie wybierz konto. Po prawej stronie zostanie wyświetlone okno Ustawienia. W oknie Ustawienia wybierz pozycję klawiszy. W zależności od tego, który klucz miejsca do magazynowania dla usługi multimediów do synchronizacji z zaznacz Synchronizuj klucza podstawowego lub pomocnicza klucza przycisk Synchronizuj. W tym przypadku należy użyć klucza pomocniczego.

- Za pomocą usługi multimediów zarządzania interfejsu API usługi REST.

W poniższym przykładzie pokazano sposób tworzenia https://endpoint/***subscriptionId/usługi/mediaservices/konta/Nazwa konta*/StorageAccounts/*storageAccountName*żądania następujący/key było synchronizować klawisz do określonego miejsca do magazynowania z usługi multimediów. W tym przypadku jest używana wartość klucza magazyn pomocniczy. Aby uzyskać więcej informacji, zobacz [jak: interfejsu API usługi REST zarządzania usługi multimediów Użyj](http://msdn.microsoft.com/library/azure/dn167656.aspx).
    
    public void UpdateMediaServicesWithStorageAccountKey(string mediaServicesAccount, string storageAccountName, string storageAccountKey)
    {
        var clientCert = GetCertificate(CertThumbprint);
        
        HttpWebRequest request = (HttpWebRequest)WebRequest.Create(string.Format("{0}/{1}/services/mediaservices/Accounts/{2}/StorageAccounts/{3}/Key",
        Endpoint, SubscriptionId, mediaServicesAccount, storageAccountName));
        request.Method = "PUT";
        request.ContentType = "application/json; charset=utf-8";
        request.Headers.Add("x-ms-version", "2011-10-01");
        request.Headers.Add("Accept-Encoding: gzip, deflate");
        request.ClientCertificates.Add(clientCert);
        
        
        using (var streamWriter = new StreamWriter(request.GetRequestStream()))
        {
            streamWriter.Write("\"");
            streamWriter.Write(storageAccountKey);
            streamWriter.Write("\"");
            streamWriter.Flush();
        }
        
        using (var response = (HttpWebResponse)request.GetResponse())
        {
            string jsonResponse;
            Stream receiveStream = response.GetResponseStream();
            Encoding encode = Encoding.GetEncoding("utf-8");
            if (receiveStream != null)
            {
                var readStream = new StreamReader(receiveStream, encode);
                jsonResponse = readStream.ReadToEnd();
            }
        }
    }

Po wykonaniu tego kroku zaktualizować istniejące Locator, (które zależności w starym kluczem miejsca do magazynowania), jak pokazano w poniższym kroku.

>[AZURE.NOTE]Poczekaj około 30 minut przed dowolnej operacji przy użyciu usługi multimediów (na przykład tworzenia Locator nowe) w celu zapobieżenia wpływu na oczekujące zadania.

##<a name="step-3-update-locators"></a>Krok 3: Aktualizowanie Locator

>[AZURE.NOTE]Wdrażania klawiszy dostępu miejsca do magazynowania, należy koniecznie zaktualizować do istniejącego Locator, dlatego nie przerwy w usłudze przesyłanie strumieniowe.

Poczekaj co najmniej 30 minut po zsynchronizowaniu nowy klucz miejsca do magazynowania z AMS. Następnie można odtworzyć usługi Locator na żądanie, aby zastosować współzależności klawisz do określonego miejsca do magazynowania i zachować istniejący adres URL.

Należy zauważyć, że po aktualizacji (lub odtworzenie) locator skojarzeń zabezpieczeń adres URL zawsze zmieni się.

>[AZURE.NOTE] Aby upewnić się, że zachować istniejące adresy URL usługi Locator na żądanie, musisz usunąć istniejące locator i Utwórz nowy za pomocą tego samego identyfikatora.

W poniższym przykładzie .NET pokazano, jak odtworzyć locator przy użyciu tego samego identyfikatora.

prywatne statyczne ILocator RecreateLocator(CloudMediaContext context, ILocator locator) {/ / Zapisz właściwości istniejącego locator.
Wariancja trwałego = locator. Zawartości; var accessPolicy = locator. AccessPolicy; var locatorId = locator. Identyfikator; Wariancja DataRozpoczęcia = locator. Czas rozpoczęcia; var locatorType = locator. Wpisz; var locatorName = locator. Nazwa;

Usuń stary locator.
Identyfikator. Delete();

Jeśli (locator. ExpirationDateTime < = DateTime.UtcNow) {zostać zgłoszony wyjątek nowego (String.Format ("nie można odtworzyć locator identyfikator = {0}, ponieważ jego czas wygaśnięcia locator jest w przeszłości", locator. Identyfikator)); }
    
        // Create new locator using saved properties.
        var newLocator = context.Locators.CreateLocator(
            locatorId,
            locatorType,
            asset,
            accessPolicy,
            startDate,
            locatorName);
    
    
    
        return newLocator;
    }


##<a name="step-5-regenerate--primary-storage-access-key"></a>Krok 5: Ponownie wygenerować klucz podstawowy dostępu

Odtworzyć klucz podstawowy programu access. Aby uzyskać informacje dotyczące sposobu użycia klawiszy miejsca do magazynowania, zobacz [jak: widok, Kopiuj i wyniku przestrzeni dyskowej, klawiszami dostępu](../storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).

##<a name="step-6-update-media-services-to-use-the-new-primary-storage-key"></a>Krok 6: Aktualizacji usługi multimediów za pomocą nowego klucza podstawowego miejsca do magazynowania
    
Użyj tej samej procedury zgodnie z opisem w [kroku 2](media-services-roll-storage-access-keys.md#step2) , tylko tym razem są synchronizowane nowy klucz podstawowy w dostęp z konta usługi multimediów.

>[AZURE.NOTE]Poczekaj około 30 minut przed dowolnej operacji przy użyciu usługi multimediów (na przykład tworzenia Locator nowe) w celu zapobieżenia wpływu na oczekujące zadania.

##<a name="step-7-update-locators"></a>Krok 7: Aktualizacja Locator  

Po 30 minut można odtworzyć usługi Locator na żądanie, aby zastosować współzależności nowy klucz podstawowy i zachować istniejący adres URL.

Użyj tej samej procedury, zgodnie z opisem w [kroku 3](media-services-roll-storage-access-keys.md#step-3-update-locators).


##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



###<a name="acknowledgments"></a>Potwierdzenia 

Chcemy Potwierdź następujące osoby, które przyczynić się do tworzenia tego dokumentu: Seva Titov Cenk Dingiloglu, Gada Mediolan.
