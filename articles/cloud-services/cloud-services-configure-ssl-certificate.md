<properties 
    pageTitle="Konfigurowanie protokołu SSL dla usługi w chmurze (klasyczny) | Microsoft Azure" 
    description="Dowiedz się, jak określić punkt końcowy HTTPS dla ról w sieci web i jak przekazywanie certyfikatu SSL do zabezpieczania aplikacji." 
    services="cloud-services" 
    documentationCenter=".net" 
    authors="Thraka" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/04/2016"
    ms.author="adegeo"/>




# <a name="configuring-ssl-for-an-application-in-azure"></a>Konfigurowanie protokołu SSL dla aplikacji platformy Azure

> [AZURE.SELECTOR]
- [Azure portal](cloud-services-configure-ssl-certificate-portal.md)
- [Portal Azure klasyczny](cloud-services-configure-ssl-certificate.md)

Szyfrowanie Secure Socket Layer (SSL) jest najczęściej używane metody zabezpieczania danych przesyłanych w Internecie. Typowe zadania w tym artykule omówiono sposób określania punktu końcowego HTTPS dla ról w sieci web i jak przekazywanie certyfikatu SSL do zabezpieczania aplikacji.

> [AZURE.NOTE] Stosowanie procedury opisane w tym zadaniu z usługami w chmurze Azure; w przypadku aplikacji usług zobacz [ten](../app-service-web/web-sites-configure-ssl-certificate.md) artykuł.

To zadanie używa wdrożenia produkcji. Informacje na temat korzystania z tymczasowy wdrożenia są dostarczane na końcu tego tematu.

Najpierw przeczytaj [ten](cloud-services-how-to-create-deploy.md) artykuł, jeśli nie został jeszcze utworzony usługi w chmurze.

[AZURE.INCLUDE [websites-cloud-services-css-guided-walkthrough](../../includes/websites-cloud-services-css-guided-walkthrough.md)]


## <a name="step-1-get-an-ssl-certificate"></a>Krok 1: Uzyskaj certyfikat SSL

Aby skonfigurować protokołu SSL dla aplikacji, należy najpierw uzyskać certyfikat SSL, które zostały podpisane przez urząd certyfikacji, zaufanych innej firmy, która wydaje certyfikaty w tym celu. Jeśli nie masz już jeden, musisz uzyskać od firmy, która zostanie sprzedany certyfikatów SSL.

Certyfikat musi spełniać następujące wymagania odnośnie certyfikatów SSL platformy Azure:

-   Certyfikat musi zawierać klucz prywatny.
-   Certyfikat muszą zostać utworzone dla wymiany kluczy, można wyeksportować do pliku wymiana informacji osobistych (pfx).
-   Nazwa podmiotu certyfikatu musi odpowiadać domeny umożliwiające dostęp do usług w chmurze. Nie można uzyskać certyfikat SSL od urzędu certyfikacji (CA) dla domeny cloudapp.net. Należy uzyskać nazwę domeny niestandardowej, aby używać, gdy dostęp do tej usługi. Gdy żądania certyfikatu od urzędu certyfikacji, nazwa podmiotu certyfikatu musi odpowiadać niestandardowej nazwy domeny umożliwia dostęp do aplikacji. Na przykład jeśli niestandardowej nazwy domeny jest **contoso.com** możesz czy żądania certyfikatu od urzędu certyfikacji dla * **. contoso.com** lub * *www.contoso.com**.
-   Certyfikat, należy użyć co najmniej 2048-bitowego szyfrowania.

Do celów testowych, możesz [utworzyć](cloud-services-certs-create.md) i używanie certyfikatu z podpisem własnym. Certyfikat z podpisem własnym nie jest uwierzytelniony przez urząd certyfikacji i można użyć domeny cloudapp.net jako adres URL witryny sieci Web. Na przykład następujące zadanie używa certyfikatu z podpisem, w którym nazwa wspólna (CN) używane w certyfikatu to **sslexample.cloudapp.net**.

Następnie muszą zawierać informacje dotyczące certyfikatu w definicji usługi i plików konfiguracji usługi.

## <a name="step-2-modify-the-service-definition-and-configuration-files"></a>Krok 2: Modyfikowanie plików definicji i Konfiguracja usługi

Aplikacja musi być skonfigurowany do używania certyfikatu, a musi zostać dodana punktu końcowego HTTPS. W wyniku definicji usługi i pliki konfiguracji usługi należy zaktualizować.

1.  W środowisku rozwoju otworzyć plik definicji usługi (CSDEF), dodawanie sekcji **certyfikatów** w sekcji **WebRole** i zawiera następujące informacje dotyczące certyfikatu (i certyfikatów pośrednich):

        <WebRole name="CertificateTesting" vmsize="Small">
        ...
            <Certificates>
                <Certificate name="SampleCertificate" 
                             storeLocation="LocalMachine" 
                             storeName="My"
                             permissionLevel="limitedOrElevated" />
                <!-- IMPORTANT! Unless your certificate is either
                self-signed or signed directly by the CA root, you
                must include all the intermediate certificates
                here. You must list them here, even if they are
                not bound to any endpoints. Failing to list any of
                the intermediate certificates may cause hard-to-reproduce
                interoperability problems on some clients.-->
                <Certificate name="CAForSampleCertificate"
                             storeLocation="LocalMachine"
                             storeName="CA"
                             permissionLevel="limitedOrElevated" />
            </Certificates>
        ...
        </WebRole>

    W sekcji **Certyfikaty** definiuje nazwę naszych certyfikatu, jego lokalizację i nazwę Sklepu, w którym znajduje się.
    
    Uprawnienia (`permisionLevel` atrybut) można ustawić jedną z następujących wartości:

  	| Wartość uprawnień  | Opis |
  	| ----------------  | ----------- |
  	| limitedOrElevated | **(Domyślne)** Wszystkie procesy roli dostęp do klucz prywatny. |
  	| podwyższonym poziomem uprawnień          | Tylko podwyższonym poziomem procesy dostęp do klucz prywatny.|

2.  W pliku definicji usługi Dodawanie elementu **InputEndpoint** w sekcji **punkty końcowe** , aby umożliwić HTTPS:

        <WebRole name="CertificateTesting" vmsize="Small">
        ...
            <Endpoints>
                <InputEndpoint name="HttpsIn" protocol="https" port="443" 
                    certificate="SampleCertificate" />
            </Endpoints>
        ...
        </WebRole>

3.  W pliku definicji usługi Dodaj element **oprawa** , w sekcji **witryny** . W tej sekcji dodaje powiązanie HTTPS do zamapować punkt końcowy do witryny:

        <WebRole name="CertificateTesting" vmsize="Small">
        ...
            <Sites>
                <Site name="Web">
                    <Bindings>
                        <Binding name="HttpsIn" endpointName="HttpsIn" />
                    </Bindings>
                </Site>
            </Sites>
        ...
        </WebRole>

    Wszystkie wymagane zmiany w pliku definicji usługi zostały już wykonane, ale nadal trzeba dodać informacje na temat certyfikatu do pliku konfiguracji usługi.

4.  W pliku konfiguracji usługi (CSCFG), ServiceConfiguration.Cloud.cscfg, Dodaj sekcję **certyfikatów** w obrębie sekcji **Rola** , zamieniając wartość przykładowych pokazano poniżej tego certyfikatu:

        <Role name="Deployment">
        ...
            <Certificates>
                <Certificate name="SampleCertificate" 
                    thumbprint="9427befa18ec6865a9ebdc79d4c38de50e6316ff" 
                    thumbprintAlgorithm="sha1" />
                <Certificate name="CAForSampleCertificate"
                    thumbprint="79d4c38de50e6316ff9427befa18ec6865a9ebdc" 
                    thumbprintAlgorithm="sha1" />
            </Certificates>
        ...
        </Role>

(W poprzednim przykładzie użyto **sha1** dla Algorytm odcisku palca. Określ odpowiednią wartość dla Algorytm odcisku palca certyfikatu).

Teraz, gdy definicji usługi i plików konfiguracji usługi zostały zaktualizowane, pakiet wdrożenia do przekazywania Azure. Jeśli korzystasz z **cspack**, nie używaj flagę **/generateConfigurationFile** , zgodnie z którą certyfikat zastąpi informacje znajdujące się wstawioną.

## <a name="step-3-upload-a-certificate"></a>Krok 3: Przekazywanie certyfikatu

Zaktualizowano pakietu wdrażania przy użyciu certyfikatu, a dodano punktu końcowego HTTPS. Możesz teraz przekazać pakiet i certyfikat Azure Portal Azure klasyczny.

1. Zaloguj się do [portalu klasyczny Azure][]. 
2. W okienku nawigacji po lewej stronie kliknij pozycję **Usług w chmurze** .
3. Kliknij pozycję Usługa w chmurze odpowiednie.
4. Kliknij kartę **Certyfikaty** .

    ![Kliknij kartę certyfikaty](./media/cloud-services-configure-ssl-certificate/click-cert.png)

5. Kliknij przycisk **Przekaż** .

    ![Przekazywanie](./media/cloud-services-configure-ssl-certificate/upload-button.png)
    
6. Wskaż **plik**, **hasło**, a następnie kliknij przycisk **Zakończ** (znacznik wyboru).

## <a name="step-4-connect-to-the-role-instance-by-using-https"></a>Krok 4: Połączyć się z wystąpieniem roli przy użyciu protokołu HTTPS

Teraz, gdy rozmieszczenia jest Azure i rozpocząć pracę, można połączyć z listą przy użyciu protokołu HTTPS.

1.  W portalu klasyczny Azure wybierz wdrożenia, a następnie kliknij łącze pod **Adres URL witryny**.

    ![Określanie adresu URL witryny][2]

2.  W przeglądarce sieci web zmodyfikuj łącze, aby użyć **https** zamiast **protokołu http**, a następnie odwiedź stronę.

    >[AZURE.NOTE] Jeśli korzystasz z certyfikat z podpisem własnym, po przejściu do punktu końcowego HTTPS, który jest skojarzony z certyfikatu z podpisem własnym można zobaczyć błąd certyfikatu w przeglądarce. Przy użyciu certyfikatu z podpisem zaufany urząd certyfikacji eliminuje ten problem. w międzyczasie możesz zignorować błąd. (Innym rozwiązaniem jest dodawanie certyfikatu z podpisem własnym do magazyn certyfikatów urząd zaufanym certyfikatem użytkownika).

    ![Przykład witryny sieci web SSL][3]

Jeśli chcesz używać protokołu SSL wdrożenia tymczasowy zamiast wdrażania produkcji, najpierw należy określić adres URL używany do wdrożenia tymczasowy. Wdrażanie usługi w chmurze środowiska wzorcowego bez certyfikatu lub dowolne informacje dotyczące certyfikatu. Po wdrożeniu można określić adres URL opartych na identyfikator GUID, która jest wyświetlana w polu **Adres URL witryny** Azure portalu klasyczny. Tworzenie certyfikatu z nazwą wspólnej (CN) równe oparte na GUID adresu URL (na przykład **32818777-6e77-4ced-a8fc-57609d404462.cloudapp.net**). Klasyczny portalu Azure umożliwia dodawanie certyfikatu w usłudze cloud etapowej. Następnie należy dodać informacje na temat certyfikatu do plików CSDEF i CSCFG, ponownie utworzyć pakiet aplikacji i aktualizowanie rozmieszczenia etapowej, aby użyć nowego pakietu.

## <a name="next-steps"></a>Następne kroki

* [Ogólne Konfiguracja usługi w chmurze](cloud-services-how-to-configure.md).
* Dowiedz się, jak [wdrożyć usługi w chmurze](cloud-services-how-to-create-deploy.md).
* Skonfiguruj [niestandardową nazwę domeny](cloud-services-custom-domain-name.md).
* [Zarządzanie usługi w chmurze](cloud-services-how-to-manage.md).


  [Portal Azure klasyczny]: http://manage.windowsazure.com
  [0]: ./media/cloud-services-configure-ssl-certificate/CreateCloudService.png
  [1]: ./media/cloud-services-configure-ssl-certificate/AddCertificate.png
  [2]: ./media/cloud-services-configure-ssl-certificate/CopyURL.png
  [3]: ./media/cloud-services-configure-ssl-certificate/SSLCloudService.png
  [4]: ./media/cloud-services-configure-ssl-certificate/AddCertificateComplete.png  
