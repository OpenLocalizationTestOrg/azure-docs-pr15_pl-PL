<properties 
    pageTitle="Konfiguracja zabezpieczeń korespondencji seryjnej podziału | Microsoft Azure" 
    description="Konfigurowanie x409 certyfikaty szyfrowania" 
    metaKeywords="Elastic Database certificates security" 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="torsteng"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="torsteng" />


# <a name="split-merge-security-configuration"></a>Konfiguracja zabezpieczeń korespondencji seryjnej podziału  

Aby korzystać z usługi podziału-korespondencji seryjnej, musi poprawnie skonfigurować zabezpieczenia. Usługa jest częścią funkcji elastyczne skali Microsoft Azure SQL Database. Aby uzyskać więcej informacji zobacz [elastyczne dzielenie skali i scalanie samouczek usługi](sql-database-elastic-scale-configure-deploy-split-and-merge.md).

## <a name="configuring-certificates"></a>Konfigurowanie certyfikatów

Certyfikaty są skonfigurowane na dwa sposoby. 

1. [Konfigurowanie certyfikatu SSL](#To-Configure-the-SSL#Certificate)
2. [Aby skonfigurować certyfikaty klienta](#To-Configure-Client-Certificates) 

## <a name="to-obtain-certificates"></a>Aby uzyskać certyfikat

Certyfikaty można uzyskać z publicznych urzędów certyfikacji (UC) lub z [Usługi Windows certyfikatu](http://msdn.microsoft.com/library/windows/desktop/aa376539.aspx). Są to preferowanej metody certyfikatów.

Jeśli te opcje nie są dostępne, możesz wygenerować **certyfikatów z podpisem własnym**.
 
## <a name="tools-to-generate-certificates"></a>Narzędzia do generowania certyfikatów

* [MakeCert.exe](http://msdn.microsoft.com/library/bfsktky3.aspx)
* [pvk2pfx.exe](http://msdn.microsoft.com/library/windows/hardware/ff550672.aspx)

### <a name="to-run-the-tools"></a>Uruchamianie narzędzia

* Z Deweloper wiersza polecenia dla programu Visual Studio, zobacz [Visual Studio wiersz polecenia](http://msdn.microsoft.com/library/ms229859.aspx) 

    Jeśli jest zainstalowany, przejdź do:

        %ProgramFiles(x86)%\Windows Kits\x.y\bin\x86 

* Uzyskiwanie WDK z [systemu Windows 8.1: Pobierz Kit i narzędzia](http://msdn.microsoft.com/windows/hardware/gg454513#drivers)

## <a name="to-configure-the-ssl-certificate"></a>Konfigurowanie certyfikatu SSL
Certyfikat SSL jest wymagane do szyfrowania komunikacji i służą do uwierzytelniania serwera. Wybierz najbardziej odpowiednią trzy scenariusze poniżej, a następnie wykonać jego kroków:

### <a name="create-a-new-self-signed-certificate"></a>Tworzenie nowego certyfikatu z podpisem własnym

1.    [Tworzenie certyfikatu z podpisem własnym](#Create-a-Self-Signed-Certificate)
2.    [Tworzenie pliku PFX dla certyfikatu SSL z podpisem własnym](#Create-PFX-file-for-Self-Signed-SSL-Certificate)
3.    [Przekazywanie certyfikatu SSL do usługi w chmurze](#Upload-SSL-Certificate-to-Cloud-Service)
4.    [Aktualizowanie certyfikatu SSL w pliku konfiguracji usługi](#Update-SSL-Certificate-in-Service-Configuration-File)
5.    [Importowanie urzędu certyfikacji SSL](#Import-SSL-Certification-Authority)

### <a name="to-use-an-existing-certificate-from-the-certificate-store"></a>Aby użyć istniejący certyfikat z magazynu certyfikatów
1. [Wyeksportuj certyfikat SSL z magazynu certyfikatów](#Export-SSL-Certificate-From-Certificate-Store)
2. [Przekazywanie certyfikatu SSL do usługi w chmurze](#Upload-SSL-Certificate-to-Cloud-Service)
3. [Aktualizowanie certyfikatu SSL w pliku konfiguracji usługi](#Update-SSL-Certificate-in-Service-Configuration-File)

### <a name="to-use-an-existing-certificate-in-a-pfx-file"></a>Aby skorzystać z istniejącego certyfikatu w pliku PFX

1. [Przekazywanie certyfikatu SSL do usługi w chmurze](#Upload-SSL-Certificate-to-Cloud-Service)
2. [Aktualizowanie certyfikatu SSL w pliku konfiguracji usługi](#Update-SSL-Certificate-in-Service-Configuration-File)

## <a name="to-configure-client-certificates"></a>Aby skonfigurować certyfikaty klienta
W celu uwierzytelnienia żądania usługi są wymagane certyfikaty klienta. Wybierz najbardziej odpowiednią trzy scenariusze poniżej, a następnie wykonać jego kroków:

### <a name="turn-off-client-certificates"></a>Wyłączanie certyfikaty klienta
1.    [Wyłączanie uwierzytelniania opartego na certyfikat klienta](#Turn-Off-Client-Certificate-Based-Authentication)

### <a name="issue-new-self-signed-client-certificates"></a>Nowe certyfikaty podpisem
1.    [Tworzenie urzędu certyfikacji podpisem](#Create-a-Self-Signed-Certification-Authority)
2.    [Przekazywanie certyfikatu urzędu certyfikacji do usługi w chmurze](#Upload-CA-Certificate-to-Cloud-Service)
3.    [Aktualizowanie certyfikat urzędu certyfikacji w pliku konfiguracji usługi](#Update-CA-Certificate-in-Service-Configuration-File)
4.    [Certyfikaty](#Issue-Client-Certificates)
5.    [Tworzenie plików PFX certyfikatów klienta](#Create-PFX-files-for-Client-Certificates)
6.    [Importowanie certyfikatu klienta](#Import-Client-Certificate)
7.    [Kopiowanie odciski palców certyfikat klienta](#Copy-Client-Certificate-Thumbprints)
8.    [Konfigurowanie klientów dozwolonych w pliku konfiguracji usługi](#Configure-Allowed-Clients-in-the-Service-Configuration-File)

### <a name="use-existing-client-certificates"></a>Używanie istniejących certyfikatów klienta
1.    [Znajdź klucz publiczny urząd certyfikacji](#Find-CA-Public Key)
2.    [Przekazywanie certyfikatu urzędu certyfikacji do usługi w chmurze](#Upload-CA-certificate-to-cloud-service)
3.    [Aktualizowanie certyfikat urzędu certyfikacji w pliku konfiguracji usługi](#Update-CA-Certificate-in-Service-Configuration-File)
4.    [Kopiowanie odciski palców certyfikat klienta](#Copy-Client-Certificate-Thumbprints)
5.    [Konfigurowanie klientów dozwolonych w pliku konfiguracji usługi](#Configure-Allowed-Clients-in-the-Service-Configuration File)
6.    [Konfigurowanie sprawdzanie odwołania certyfikatu klienta](#Configure-Client-Certificate-Revocation-Check)

## <a name="allowed-ip-addresses"></a>Dozwolonymi adresami IP

Dostęp do punktów końcowych usługi może być ograniczone do określonych zakresów adresów IP.

## <a name="to-configure-encryption-for-the-store"></a>Aby skonfigurować szyfrowanie magazynu

Certyfikat jest wymagane do szyfrowania poświadczeń, które są przechowywane w magazynie metadanych. Wybierz najbardziej odpowiednią trzy scenariusze poniżej, a następnie wykonać jego kroków:

### <a name="use-a-new-self-signed-certificate"></a>Użyj nowego certyfikatu z podpisem własnym

1.     [Tworzenie certyfikatu z podpisem własnym](#Create-a-Self-Signed-Certificate)
2.     [Tworzenie pliku PFX dla certyfikatu z podpisem własnym szyfrowania](#Create-PFX-file-for-Self-Signed-Encryption-Certificate)
3.     [Przekazywanie certyfikat szyfrowania do usługi w chmurze](#Upload-Encryption-Certificate-to-Cloud-Service)
4.     [Aktualizowanie certyfikatu szyfrowania pliku konfiguracji usługi](#Update-Encryption-Certificate-in-Service-Configuration-File)

### <a name="use-an-existing-certificate-from-the-certificate-store"></a>Używanie istniejącego certyfikatu z magazynu certyfikatów

1.     [Wyeksportuj certyfikat szyfrowania z magazynu certyfikatów](#Export-Encryption-Certificate-From-Certificate-Store)
2.     [Przekazywanie certyfikat szyfrowania do usługi w chmurze](#Upload-Encryption-Certificate-to-Cloud-Service)
3.     [Aktualizowanie certyfikatu szyfrowania pliku konfiguracji usługi](#Update-Encryption-Certificate-in-Service-Configuration-File)

### <a name="use-an-existing-certificate-in-a-pfx-file"></a>Używanie istniejącego certyfikatu w pliku PFX

1.     [Przekazywanie certyfikat szyfrowania do usługi w chmurze](#Upload-Encryption-Certificate-to-Cloud-Service)
2.     [Aktualizowanie certyfikatu szyfrowania pliku konfiguracji usługi](#Update-Encryption-Certificate-in-Service-Configuration-File)

## <a name="the-default-configuration"></a>Konfiguracja domyślna

Domyślna konfiguracja powoduje zablokowanie wszystkich dostępu do punktu końcowego HTTP. To jest zalecanego ustawienia, ponieważ żądania do te punkty końcowe może prowadzić informacje poufne, takie jak poświadczenia bazy danych.
Domyślna konfiguracja umożliwia dostęp do punktu końcowego HTTPS. To ustawienie może być ograniczone dalej.

### <a name="changing-the-configuration"></a>Zmiana konfiguracji

Grupy punktu końcowego i reguły dostępu, które dotyczą są skonfigurowane w **<EndpointAcls>** w sekcji w **pliku konfiguracji usługi**.

    <EndpointAcls>
      <EndpointAcl role="SplitMergeWeb" endPoint="HttpIn" accessControl="DenyAll" />
      <EndpointAcl role="SplitMergeWeb" endPoint="HttpsIn" accessControl="AllowAll" />
    </EndpointAcls>

Reguły w grupie kontroli dostępu są skonfigurowane w <AccessControl name=""> części pliku konfiguracji usługi. 

W dokumentacji listy kontroli dostępu sieci tłumaczy format.
Na przykład aby umożliwić tylko adresy IP w zakresie 100.100.0.0 do 100.100.255.255, aby uzyskać dostęp do punktu końcowego HTTPS, reguł wyglądałby następująco:

    <AccessControl name="Retricted">
      <Rule action="permit" description="Some" order="1" remoteSubnet="100.100.0.0/16"/>
      <Rule action="deny" description="None" order="2" remoteSubnet="0.0.0.0/0" />
    </AccessControl>
    <EndpointAcls>
    <EndpointAcl role="SplitMergeWeb" endPoint="HttpsIn" accessControl="Restricted" />

## <a name="denial-of-service-prevention"></a>Odmowa zapobieganie świadczeniu usługi

Istnieją dwa różne mechanizmy obsługiwane wykrywanie i zapobieganie ataki usługi:

*    Ogranicz liczbę równoczesne żądania na hoście zdalnym (domyślnie wyłączone)
*    Ograniczanie stopa programu access na hoście zdalnym (na domyślnie)

Te są oparte na funkcji opisano w dynamiczne zabezpieczeń adresów IP w programie IIS. Podczas zmieniania konfiguracji Uważaj następujące kwestie:

* Zachowanie serwery proxy i urządzeń translatora adresów sieciowych nad informacjami zdalnego hosta
* Każdego żądania do dowolnego zasobu w roli sieci web jest traktowany jako (np. ładowanie skryptów, obrazy, itp)

## <a name="restricting-number-of-concurrent-accesses"></a>Ograniczanie liczby równoczesne dostępu

Dostępne są następujące ustawienia, które skonfigurować to zachowanie:

    <Setting name="DynamicIpRestrictionDenyByConcurrentRequests" value="false" />
    <Setting name="DynamicIpRestrictionMaxConcurrentRequests" value="20" />

Zmienianie DynamicIpRestrictionDenyByConcurrentRequests wartość PRAWDA, aby włączyć tę ochronę.

## <a name="restricting-rate-of-access"></a>Ograniczanie stopa programu access

Dostępne są następujące ustawienia, które skonfigurować to zachowanie:

    <Setting name="DynamicIpRestrictionDenyByRequestRate" value="true" />
    <Setting name="DynamicIpRestrictionMaxRequests" value="100" />
    <Setting name="DynamicIpRestrictionRequestIntervalInMilliseconds" value="2000" />

## <a name="configuring-the-response-to-a-denied-request"></a>Konfigurowanie odpowiedź na żądanie odmowa

Odpowiedź na żądanie odmowa skonfigurowanie:

    <Setting name="DynamicIpRestrictionDenyAction" value="AbortRequest" />
Zapoznaj się z dokumentacją dla dynamicznego zabezpieczeń adresów IP w programie IIS dla innych obsługiwane wartości.

## <a name="operations-for-configuring-service-certificates"></a>Operacje dotyczące konfigurowania usługi certyfikatów
W tym temacie jest tylko w celach informacyjnych. Wykonaj kroki konfiguracji przedstawionych w:

* Konfigurowanie certyfikatu SSL
* Konfigurowanie certyfikatów klienta

## <a name="create-a-self-signed-certificate"></a>Tworzenie certyfikatu z podpisem własnym
Wykonaj:

    makecert ^
      -n "CN=myservice.cloudapp.net" ^
      -e MM/DD/YYYY ^
      -r -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.1" ^
      -a sha1 -len 2048 ^
      -sv MySSL.pvk MySSL.cer

Aby dostosować:

*    -n przy użyciu adresu URL usługi. Symbole wieloznaczne ("CN = * .cloudapp .net") i nazwy alternatywne ("CN=myservice1.cloudapp.net, CN=myservice2.cloudapp.net") są obsługiwane.
*    -e z Data wygaśnięcia certyfikatu Utwórz silne hasło i określ po wyświetleniu monitu.

## <a name="create-pfx-file-for-self-signed-ssl-certificate"></a>Utwórz plik PFX dla certyfikatu z podpisem własnym SSL

Wykonaj:

        pvk2pfx -pvk MySSL.pvk -spc MySSL.cer

Wprowadź hasło, a następnie wyeksportuj certyfikat z następujących opcji:
* Tak, Eksportuj klucz prywatny
* Eksportuj wszystkie właściwości rozszerzone

## <a name="export-ssl-certificate-from-certificate-store"></a>Wyeksportuj certyfikat SSL z magazynu certyfikatów

* Znajdowanie certyfikatu
* Kliknij pozycję Akcje -> Wszystkie zadania -> Eksportuj...
* Wyeksportuj certyfikat do. Plik PFX z następujących opcji:
    * Tak, Eksportuj klucz prywatny
    * Jeśli to możliwe, Dołącz wszystkie certyfikaty w ścieżce certyfikacji * wyeksportować wszystkie rozszerzone właściwości

## <a name="upload-ssl-certificate-to-cloud-service"></a>Przekazywanie certyfikatu SSL do usługi w chmurze

Przekazywanie certyfikatu z istniejącą lub wygenerowane. Plik PFX z pary kluczy SSL:

* Wprowadź hasło chroniące informacje o kluczu prywatnym

## <a name="update-ssl-certificate-in-service-configuration-file"></a>Aktualizowanie certyfikatu SSL w pliku konfiguracji usługi

Zaktualizuj wartość następujące ustawienia pliku konfiguracji usługi z odcisku palca certyfikatu przekazane do usługi w chmurze:

    <Certificate name="SSL" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="import-ssl-certification-authority"></a>Importowanie urzędu certyfikacji SSL

Wykonaj poniższe czynności w wszystkie konta komputera, która będzie komunikować się z usługą:

* Kliknij dwukrotnie. Plik CER w Eksploratorze Windows
* W oknie dialogowym certyfikat kliknij przycisk Zainstaluj certyfikat...
* Importowanie certyfikatu do magazynu Zaufane główne urzędy certyfikacji

## <a name="turn-off-client-certificate-based-authentication"></a>Wyłączanie uwierzytelniania opartego na certyfikat klienta

Jest obsługiwana tylko uwierzytelniania opartego na certyfikacie klienta i ją wyłączyć pozwoli publicznego dostępu do punktów końcowych usługi, o ile nie są inne mechanizmy w miejscu (np. Microsoft Azure wirtualna sieć).

Ma wartość FAŁSZ w pliku konfiguracji usługi, aby wyłączyć tę funkcję, należy zmienić te ustawienia:

    <Setting name="SetupWebAppForClientCertificates" value="false" />
    <Setting name="SetupWebserverForClientCertificates" value="false" />

Następnie skopiuj sam odcisku palca jako certyfikatu SSL w ustawieniu certyfikatu urzędu certyfikacji:

    <Certificate name="CA" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="create-a-self-signed-certification-authority"></a>Tworzenie urzędu certyfikacji podpisem
Wykonaj poniższe czynności, aby utworzyć certyfikat z podpisem własnym ma pełnić rolę urzędu certyfikacji:

    makecert ^
    -n "CN=MyCA" ^
    -e MM/DD/YYYY ^
     -r -cy authority -h 1 ^
     -a sha1 -len 2048 ^
      -sr localmachine -ss my ^
      MyCA.cer

Aby go dostosować

*    -e z daty wygaśnięcia certyfikacji


## <a name="find-ca-public-key"></a>Znajdź klucz publiczny urząd certyfikacji

Wszystkie certyfikaty klienta musi być wystawiony przez zaufany przez usługę Urząd certyfikacji. Znajdź klucz publiczny do urzędu certyfikacji, który wystawiony klienta certyfikatów, które mają być używane do uwierzytelniania Aby można było go przekazać do usługi w chmurze.

Jeśli plik z kluczem publicznym nie jest dostępna, można je wyeksportować z magazynu certyfikatów:

* Znajdowanie certyfikatu
    * Wyszukiwanie certyfikat klienta wystawiony przez ten sam urząd certyfikacji
* Kliknij dwukrotnie certyfikat.
* Wybierz kartę Ścieżka certyfikacji w oknie dialogowym certyfikatu.
* Kliknij dwukrotnie pozycję urzędu certyfikacji w ścieżce.
* Sporządzanie notatek właściwości certyfikatu.
* Zamknij okno dialogowe **certyfikat** .
* Znajdowanie certyfikatu
    * Wyszukiwanie powyższą urząd certyfikacji.
* Kliknij pozycję Akcje -> Wszystkie zadania -> Eksportuj...
* Wyeksportuj certyfikat do. CER z następujących opcji:
    * **Nie eksportuj klucz prywatny**
    * Jeśli to możliwe Dołącz wszystkie certyfikaty w ścieżce certyfikacji.
    * Eksportuj wszystkie właściwości rozszerzone.

## <a name="upload-ca-certificate-to-cloud-service"></a>Przekazywanie certyfikatu urzędu certyfikacji do usługi w chmurze

Przekazywanie certyfikatu z istniejącą lub wygenerowane. Plik CER z klucz publiczny urząd certyfikacji.

## <a name="update-ca-certificate-in-service-configuration-file"></a>Certyfikat urzędu certyfikacji aktualizacji w pliku konfiguracji usługi

Zaktualizuj wartość następujące ustawienia pliku konfiguracji usługi z odcisku palca certyfikatu przekazane do usługi w chmurze:

    <Certificate name="CA" thumbprint="" thumbprintAlgorithm="sha1" />

Z tym samym odcisku palca, należy zaktualizować wartość następujące ustawienia:

    <Setting name="AdditionalTrustedRootCertificationAuthorities" value="" />

## <a name="issue-client-certificates"></a>Certyfikaty

Każda osoba upoważniona do uzyskania dostępu do usługi powinna być klienta certyfikat wystawiony dla his/hers wyłączności za pomocą i należy wybrać, czy his/hers właścicielem silne hasło ochrony kluczem prywatnym. 

W tym samym komputerze, na którym generowane i umieszczane certyfikatu z podpisem własnym urzędu certyfikacji muszą zostać wykonane następujące czynności:

    makecert ^
      -n "CN=My ID" ^
      -e MM/DD/YYYY ^
      -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.2" ^
      -a sha1 -len 2048 ^
      -in "MyCA" -ir localmachine -is my ^
      -sv MyID.pvk MyID.cer

Dostosowywanie:

* -n z identyfikator klienta, którego będzie można uwierzytelnić z tym certyfikatem
* -e z Data wygaśnięcia certyfikatu
* MyID.pvk i MyID.cer z unikatowych nazw plików dla tego certyfikatu klienta.

To polecenie wyświetli monit o hasło ma być utworzony, a następnie użyty jeden raz. Za pomocą silnego hasła.

## <a name="create-pfx-files-for-client-certificates"></a>Tworzenie certyfikatów pliki PFX dla klienta

Dla każdego certyfikatu wygenerowane klienta należy wykonać:

    pvk2pfx -pvk MyID.pvk -spc MyID.cer

Dostosowywanie:

    MyID.pvk and MyID.cer with the filename for the client certificate

Wprowadź hasło, a następnie wyeksportuj certyfikat z następujących opcji:

* Tak, Eksportuj klucz prywatny
* Eksportuj wszystkie właściwości rozszerzone
* Osoby, dla którego został wystawiony ten certyfikat należy wybrać hasło eksportu

## <a name="import-client-certificate"></a>Importowanie certyfikatu klienta

Każda osoba, dla którego certyfikat klienta został wydany należy zaimportować pary kluczy w środowisku maszyn, które on umożliwia komunikowanie się za pomocą usługi:

* Kliknij dwukrotnie. Plik PFX w Eksploratorze Windows
* Importowanie certyfikatu do osobistych sklepu z co najmniej tę opcję:
    * Dołącz wszystkie właściwości rozszerzone zaznaczone pole wyboru

## <a name="copy-client-certificate-thumbprints"></a>Kopiowanie odciski palców certyfikat klienta
Każda osoba, dla którego został wystawiony certyfikat klienta, należy wykonać następujące kroki w celu uzyskania odcisk palca his/hers certyfikatu, które zostaną dodane do pliku konfiguracji usługi:
* Uruchamianie certmgr.exe
* Wybierz kartę osobiste.
* Kliknij dwukrotnie certyfikat klienta może być używany do uwierzytelniania
* W oknie dialogowym certyfikat, który zostanie otwarty wybierz kartę Szczegóły
* Upewnij się, że Pokaż są wyświetlane wszystkie
* Zaznacz pole o nazwie odcisku palca na liście
* Skopiuj wartość odcisku palca **Usuwanie niewidoczne znaków Unicode przed pierwszej cyfry** Usuń wszystkie spacje

## <a name="configure-allowed-clients-in-the-service-configuration-file"></a>Konfigurowanie klientów dozwolone w pliku konfiguracji usługi

Zaktualizuj wartość następujące ustawienia pliku konfiguracji usługi z listą przecinkami odciski palców certyfikatów klientów mogli uzyskiwać dostęp do usługi:

    <Setting name="AllowedClientCertificateThumbprints" value="" />

## <a name="configure-client-certificate-revocation-check"></a>Konfigurowanie sprawdzanie odwołania certyfikatu klienta

Ustawieniem domyślnym nie sprawdza się z urząd certyfikacji stanu odwołania certyfikatu klienta. Aby włączyć funkcję kontroli, jeśli urząd certyfikacji, który wydane certyfikaty klienta obsługuje kontrole, należy zmienić następujące ustawienia z jednym z wartości określonych w wyliczenia X509RevocationMode:

    <Setting name="ClientCertificateRevocationCheck" value="NoCheck" />

## <a name="create-pfx-file-for-self-signed-encryption-certificates"></a>Utwórz plik PFX dla certyfikaty szyfrowania podpisem

Aby uzyskać certyfikat szyfrowania należy wykonać:

    pvk2pfx -pvk MyID.pvk -spc MyID.cer

Dostosowywanie:

    MyID.pvk and MyID.cer with the filename for the encryption certificate

Wprowadź hasło, a następnie wyeksportuj certyfikat z następujących opcji:
*    Tak, Eksportuj klucz prywatny
*    Eksportuj wszystkie właściwości rozszerzone
*    Podczas przekazywania certyfikatu do usługi w chmurze, będzie potrzebne hasło.

## <a name="export-encryption-certificate-from-certificate-store"></a>Wyeksportuj certyfikat szyfrowania z magazynu certyfikatów

*    Znajdowanie certyfikatu
*    Kliknij pozycję Akcje -> Wszystkie zadania -> Eksportuj...
*    Wyeksportuj certyfikat do. Plik PFX z następujących opcji: 
  *    Tak, Eksportuj klucz prywatny
  *    Jeśli to możliwe, Dołącz wszystkie certyfikaty w ścieżce certyfikacji 
*    Eksportuj wszystkie właściwości rozszerzone

## <a name="upload-encryption-certificate-to-cloud-service"></a>Przekaż certyfikat szyfrowania do usługi w chmurze

Przekazywanie certyfikatu z istniejącą lub wygenerowane. Plik PFX z para kluczy:

* Wprowadź hasło chroniące informacje o kluczu prywatnym

## <a name="update-encryption-certificate-in-service-configuration-file"></a>Aktualizowanie certyfikatu szyfrowania pliku konfiguracji usługi

Zaktualizuj wartości odcisku palca następujące ustawienia pliku konfiguracji usługi z odcisku palca certyfikatu przekazane do usługi w chmurze:

    <Certificate name="DataEncryptionPrimary" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="common-certificate-operations"></a>Typowe operacje certyfikatu

* Konfigurowanie certyfikatu SSL
* Konfigurowanie certyfikatów klienta

## <a name="find-certificate"></a>Znajdowanie certyfikatu

Wykonaj następujące czynności:

1. Uruchom mmc.exe.
2. Plik -> Dodaj/Usuń przystawki...
3. Wybierz pozycję **Certyfikaty**.
4. Kliknij przycisk **Dodaj**.
5. Wybierz lokalizację, Magazyn certyfikatów.
6. Kliknij przycisk **Zakończ**.
7. Kliknij **przycisk OK**.
8. Rozwiń węzeł **Certyfikaty**.
9. Rozwiń węzeł magazyn certyfikatów.
10. Rozwiń węzeł podrzędny certyfikatu.
11. Wybierz certyfikat na liście.

## <a name="export-certificate"></a>Eksportowanie certyfikatu
**Kreator eksportu certyfikatów**:

1. Kliknij przycisk **Dalej**.
2. Wybierz pozycję **Tak**, **Eksportuj klucz prywatny**.
3. Kliknij przycisk **Dalej**.
4. Wybierz żądany format pliku.
5. Zaznacz żądane opcje.
6. Zaznacz pole wyboru **hasło**.
7. Wprowadź silne hasło i potwierdź je.
8. Kliknij przycisk **Dalej**.
9. Wpisz lub wybierz nazwę pliku miejsca przechowywania certyfikatu (za pomocą. Rozszerzenie PFX).
10. Kliknij przycisk **Dalej**.
11. Kliknij przycisk **Zakończ**.
12. Kliknij **przycisk OK**.

## <a name="import-certificate"></a>Importowanie certyfikatu

W Kreatorze importu certyfikatów:

1. Wybierz lokalizację magazynu.

    * Zaznaczanie **Bieżącego użytkownika** , jeśli tylko procesów uruchomionych w obszarze bieżący użytkownik będzie dostęp do usługi
    * Wybierz pozycję **Komputer lokalny** , jeśli inne procesy w tym komputerze zostanie dostęp do usługi
2. Kliknij przycisk **Dalej**.
3. W przypadku importowania z pliku, upewnij się, ścieżka do pliku.
4. W przypadku importowania. Plik PFX:
    1.     Wprowadź hasło chroniące klucz prywatny
    2.     Wybierz opcje importowania
5.     Wybierz pozycję "Miejsca" certyfikaty w następującym magazynie
6.     Kliknij przycisk **Przeglądaj**.
7.     Wybierz żądany magazyn.
8.     Kliknij przycisk **Zakończ**.
       
    * Jeśli wybrano magazynie zaufanego głównego urzędu certyfikacji, kliknij przycisk **Tak**.
9.     Kliknij przycisk **OK** na wszystkie okna dialogowe.

## <a name="upload-certificate"></a>Przekazywanie certyfikatu

W [portalu Azure](https://portal.azure.com/)

1. Wybierz pozycję **usług w chmurze**.
2. Wybierz pozycję Usługa w chmurze.
3. W górnym menu kliknij pozycję **Certyfikaty**.
4. Na dolnym pasku kliknij przycisk **Przekaż**.
5. Wybierz plik certyfikatu.
6. Jeśli jest. PFX plików, wprowadź hasło klucz prywatny.
7. Po zakończeniu, skopiuj odcisku palca certyfikatu z nową pozycję na liście.

## <a name="other-security-considerations"></a>Inne zagadnienia dotyczące zabezpieczeń
 
Ustawienia protokołu SSL opisanych w tym dokumencie szyfrowania komunikacji między swoich klientów i usługi, gdy punkt końcowy HTTPS jest używana. Jest to ważne, ponieważ poświadczenia dostępu do bazy danych i inne potencjalnie poufne informacje są zawarte w komunikacie. Zauważ, że usługa będzie nadal występował statusem poświadczeń, w tym w swoich wewnętrznych tabel w bazie danych programu Microsoft SQL Azure, podana magazynu metadanych w subskrypcji usługi Microsoft Azure. Zdefiniowano tej bazy danych jako część następujące ustawienie w pliku konfiguracji usługi (. Plik CSCFG): 

    <Setting name="ElasticScaleMetadata" value="Server=…" />

Poświadczeń przechowywanych w tej bazy danych są szyfrowane. Jednak zgodnie z zaleceniami dotyczącymi upewnij się, że zarówno w sieci web, jak i pracownik ról wdrażanie usługi są stale aktualizowane i bezpiecznego ich mają dostęp do bazy danych metadanych i certyfikatu służącego do szyfrowania i odszyfrowywania przechowywanych poświadczeń. 

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
