<properties 
    pageTitle="Certyfikaty zarządzania i usług w chmurze | Microsoft Azure" 
    description="Dowiedz się, jak tworzyć i certyfikatów za pomocą Microsoft Azure" 
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
    ms.date="10/11/2016"
    ms.author="adegeo"/>

# <a name="certificates-overview-for-azure-cloud-services"></a>Omówienie certyfikatów dla usług w chmurze Azure
Certyfikaty są używane w Azure usług w chmurze ([certyfikaty usługi](#what-are-service-certificates)) oraz uwierzytelniania za pomocą usługi Zarządzanie interfejsu API ([certyfikaty zarządzania](#what-are-management-certificates) podczas korzystania z portal Azure klasyczny i nie ARM). W tym temacie przedstawiono ogólne omówienie oba typy certyfikatów, jak [tworzyć](#create) i [wdrażać](#deploy) je Azure.

Używane w Azure x.509 v3 certyfikatów i certyfikatów mogą być podpisywane przez innego zaufanego certyfikatu lub można je podpisem. Certyfikat z podpisem własnym podpisany przez własnej Kreator oraz ze względu na to, nie są domyślnie zaufany. Większości przeglądarek można zignorować to. Certyfikaty z podpisem własnym należy używać tylko przez siebie podczas tworzenia i testowania usług w chmurze. 

Certyfikaty używane przez Azure może zawierać prywatnych lub klucz publiczny. Certyfikaty mają odcisku palca, który umożliwia identyfikację w sposób mapy. Ten odcisku palca jest używane w Azure [pliku konfiguracji](cloud-services-configure-ssl-certificate.md) do identyfikowania certyfikatu, które należy używać usługi w chmurze. 

## <a name="what-are-service-certificates"></a>Co to są certyfikaty usługi?
Certyfikaty usługi dołączonych do usług w chmurze i komunikacja zabezpieczona do i z usługi. Na przykład jeśli wdrożono ról w sieci web, należy podać certyfikat, który może przeprowadzać uwierzytelnianie udostępnionej końcowego HTTPS. Certyfikaty usługi, z definicją usługi w są automatycznie wdrażane maszyn wirtualnych, uruchomionego wystąpienia roli użytkownika. 

Możesz przekazać certyfikaty usługi Azure klasyczny portalu za pomocą portalu klasyczny Azure lub za pomocą interfejsu API zarządzania usługi. Certyfikaty usługi są skojarzone z usługi w chmurze określonych i przypisane do wdrożenia w pliku definicji usługi.

Certyfikaty usługi mogą być zarządzane oddzielnie od usług i mogą być zarządzane przez inne osoby. Na przykład deweloper może przekazać pakietu usługi, który odwołuje się do certyfikat, który jest Menedżer występują przekazane wcześniej Azure. Kierownik można zarządzać i odnowienia zmieniania konfiguracji usługi bez konieczności Przekaż nowy pakiet usługi. Jest to możliwe, ponieważ nazwa logiczna dla certyfikatu i sklepu nazwy i lokalizacji są określane w postaci pliku definicji usługi podczas odcisku palca certyfikatu określonego w pliku konfiguracji usługi. Aby zaktualizować certyfikatu, należy tylko przekazywać nowego certyfikatu i zmień wartość w pliku konfiguracji usługi.

## <a name="what-are-management-certificates"></a>Co to są certyfikaty zarządzania?
Certyfikaty zarządzania umożliwiają uwierzytelnienia z interfejsem API usługi zarządzania dostarczony przez Azure klasyczny. Wiele programów i narzędzia (na przykład programu Visual Studio lub Azure SDK) przy użyciu tych certyfikatów zautomatyzować Konfiguracja i wdrożenie różnych usług Azure. Te nie są naprawdę związane z usług w chmurze. 

>[AZURE.WARNING] Ostrożnie! Poniższe typy certyfikatów zezwolić każdej osobie uwierzytelnia im Aby zarządzać subskrypcją, z którymi są skojarzone z. 

### <a name="limitations"></a>Ograniczenia
Istnieje ograniczenie liczby 100 certyfikatów zarządzania dla subskrypcji. Istnieje także ograniczenie 100 certyfikaty zarządzania dla wszystkich subskrypcji w obszarze identyfikatora użytkownika administratora określonej usługi. Jeśli zachodzi potrzeba więcej certyfikatów Identyfikatora użytkownika administratora konto jest już używana dodawać 100 certyfikaty zarządzania, możesz dodać uprawnienia Współtworzenie administratora, aby dodać dodatkowe certyfikaty. 

Zanim dodasz więcej niż 100 certyfikaty, zobacz Jeśli można użyć ponownie istniejącego certyfikatu. Za pomocą administratorów współpracujących sumuje potencjalnie niepotrzebne złożoność procesu zarządzania certyfikatu.


<a name="create"></a>
## <a name="create-a-new-self-signed-certificate"></a>Tworzenie nowego certyfikatu z podpisem własnym
Za pomocą dowolnego narzędzia można utworzyć certyfikat z podpisem własnym, jak przylegają do tych ustawień:

* Certyfikat X.509.
* Zawiera klucz prywatny.
* Utworzone dla wymiany kluczy (plik pfx).
* Nazwa musi odpowiadać domeny umożliwiające dostęp do usług w chmurze. 
    > Nie można pobrać SSL domeny certyfikat cloudapp.net (lub dowolną Azure powiązanych). Nazwa podmiotu certyfikatu musi odpowiadać niestandardowej nazwy domeny umożliwia dostęp do aplikacji. Na przykład **contoso.net**, **contoso.cloudapp.net**.
* Co najmniej 2048-bitowego szyfrowania.
* **Tylko certyfikatu usługi**: certyfikat klienta musi znajdować się w magazynie certyfikatów *osobistych* .

Istnieją dwa sposoby łatwe tworzenie certyfikatu w systemie Windows z `makecert.exe` narzędzia lub usług IIS.

### <a name="makecertexe"></a>MakeCert.exe

Narzędzie to została zastąpiona i nie jest już opisanych tutaj. Można znaleźć [w tym artykule w witrynie MSDN](https://msdn.microsoft.com/library/windows/desktop/aa386968) , aby uzyskać więcej informacji.

### <a name="powershell"></a>Programu PowerShell

```powershell
$cert = New-SelfSignedCertificate -DnsName yourdomain.cloudapp.net -CertStoreLocation "cert:\LocalMachine\My"
$password = ConvertTo-SecureString -String "your-password" -Force -AsPlainText
Export-PfxCertificate -Cert $cert -FilePath ".\my-cert-file.pfx" -Password $password
```

>[AZURE.NOTE] Przy użyciu certyfikatu z adresem IP zamiast domeny, należy użyć adresu IP w parametrze - Nazwa_dns.


Jeśli chcesz używać tego [certyfikatu za pomocą portalu zarządzania](../azure-api-management-certs.md), należy go wyeksportować do pliku **cer** :

```powershell
Export-Certificate -Type CERT -Cert $cert -FilePath .\my-cert-file.cer
```

### <a name="internet-information-services-iis"></a>Internetowe usługi informacyjne (IIS)

Istnieje wiele stron w Internecie, które obejmują jak to zrobić przy użyciu usług IIS. [W tym miejscu](https://www.sslshopper.com/article-how-to-create-a-self-signed-certificate-in-iis-7.html) jest doskonałe mam Pomyśl wyjaśniono to również. 

### <a name="java"></a>Java
Java umożliwia [Tworzenie certyfikatu](../app-service-web/java-create-azure-website-using-java-sdk.md#create-a-certificate).

### <a name="linux"></a>Linux
[W tym](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md) artykule opisano Tworzenie talonów z SSH.

## <a name="next-steps"></a>Następne kroki

[Przekazywanie certyfikatu usługi do portalu klasyczny Azure](cloud-services-configure-ssl-certificate.md) (lub [Azure portal](cloud-services-configure-ssl-certificate-portal.md)).

Przekaż [certyfikat interfejsu API zarządzania](../azure-api-management-certs.md) do portalu klasyczny Azure.

>[AZURE.NOTE] Azure portal nie korzysta z certyfikatów zarządzania Aby uzyskać dostęp do API, ale użyje kont użytkowników.
