<properties
    pageTitle="Zarządzanie Azure interfejsu API — często zadawane pytania | Microsoft Azure"
    description="Dowiedz się odpowiedzi na często zadawane pytania, wzorców oraz najlepsze rozwiązania dotyczące zarządzania interfejsu API Azure."
    services="api-management"
    documentationCenter=""
    authors="miaojiang"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="mijiang"/>

# <a name="azure-api-management-faqs"></a>Interfejs API Azure zarządzania często zadawane pytania

Odpowiedzi na często zadawane pytania, wzorców i najlepszych rozwiązań Azure interfejsu API zarządzania.

## <a name="frequently-asked-questions"></a>Często zadawane pytania

-   [Jak można zadać zespołu Microsoft Azure interfejsu API zarządzania pytanie?](#how-can-i-ask-the-microsoft-azure-api-management-team-a-question)
-   [Co to znaczy, gdy funkcja znajduje się w wersji preview?](#what-does-it-mean-when-a-feature-is-in-preview)
-   [Jak można zabezpieczyć połączenie między brama zarządzania interfejsu API i moimi usługami wewnętrznej](#how-can-i-secure-the-connection-between-the-api-management-gateway-and-my-back-end-services)
-   [Jak skopiować Moje wystąpienie usługi zarządzania interfejsu API do nowego wystąpienia?](#how-do-i-copy-my-api-management-service-instance-to-a-new-instance)
-   [Zarządzać Moje wystąpienia interfejsu API zarządzania programowy?](#can-i-manage-my-api-management-instance-programmatically)
-   [Jak dodać użytkownika do grupy Administratorzy?](#how-do-i-add-a-user-to-the-administrators-group)
-   [Dlaczego jest zasady, które chcę dodać niedostępne w edytorze zasad?](#why-is-the-policy-that-i-want-to-add-unavailable-in-the-policy-editor)
-   [Jak korzystać z interfejsu API przechowywanie wersji w interfejsu API zarządzania?](#how-do-i-use-api-versioning-in-api-management)
-   [Jak skonfigurować wielu środowiskach w jeden interfejs API](#how-do-i-set-up-multiple-environments-in-a-single-api)
-   [Czy mogę korzystać z interfejsu API zarządzania SOAP?](#can-i-use-soap-with-api-management)
-   [Jest stałą adres IP bramy zarządzania interfejsu API? Czy można używać go w reguły zapory?](#is-the-api-management-gateway-ip-address-constant-can-i-use-it-in-firewall-rules)
-   [Serwer autoryzacji OAuth 2.0 można skonfigurować przy użyciu usług AD FS zabezpieczeń?](#can-i-configure-an-oauth-20-authorization-server-with-adfs-security)
-   [Jakie metody routingu zarządzania interfejsu API używa we wdrożeniach do kilku lokalizacji geograficznej?](#what-routing-method-does-api-management-use-in-deployments-to-multiple-geographic-locations)
-   [Do tworzenia wystąpienia usługi zarządzania interfejsu API można używać szablonu Menedżera zasobów Azure?](#can-i-use-an-azure-resource-manager-template-to-create-an-api-management-service-instance)
-   [Czy można używać certyfikatu z podpisem własnym protokołu SSL dla wewnętrznej?](#can-i-use-a-self-signed-ssl-certificate-for-a-back-end)
-   [Ustalanie przyczyny zwrócenia błędu uwierzytelniania, podczas próby klonowanie repozytorium CYFRA?](#why-do-i-get-an-authentication-failure-when-i-try-to-clone-a-git-repository)
-   [Interfejs API zarządzania działa Azure ExpressRoute?](#does-api-management-work-with-azure-expressroute)
-   [Czy można przenosić usługi zarządzania interfejsu API z jedną subskrypcję do innego?](#can-i-move-an-api-management-service-from-one-subscription-to-another)


### <a name="how-can-i-ask-the-microsoft-azure-api-management-team-a-question"></a>Jak można zadać zespołu Microsoft Azure interfejsu API zarządzania pytanie?

Można skontaktować się z nami, stosując jedną z następujących opcji:

-   Publikowanie na pytania w naszym [forum interfejsu API zarządzania w witrynie MSDN](https://social.msdn.microsoft.com/forums/azure/home?forum=azureapimgmt).
-   Wysyłanie wiadomości e-mail do <apimgmt@microsoft.com>.
-   Wyślij żądanie funkcji na [forum opinii Azure](https://feedback.azure.com/forums/248703-api-management).

### <a name="what-does-it-mean-when-a-feature-is-in-preview"></a>Co to znaczy, gdy funkcja znajduje się w wersji preview?

Gdy funkcja znajduje się na podglądzie, oznacza to, że firma Microsoft już aktywnie znalezienia opinii na temat pracy funkcji dla Ciebie. Funkcja podglądu jest funkcjonalny wykonane, ale jest możliwe, że będzie udzielamy najnowsze zmiany w odpowiedzi na opinie klientów. Zaleca się, że nie są zależne od funkcji, która znajduje się w wersji preview w środowisku produkcyjnym. Jeśli masz dowolnej opinii na temat funkcji podglądu, napisz nam za pośrednictwem jedną z opcji kontaktu w [jak można zadać zespołu Microsoft Azure interfejsu API zarządzania pytanie?](#how-can-i-ask-the-microsoft-azure-api-management-team-a-question).

### <a name="how-can-i-secure-the-connection-between-the-api-management-gateway-and-my-back-end-services"></a>Jak można zabezpieczyć połączenie między brama zarządzania interfejsu API i moimi usługami wewnętrznej

Istnieje kilka opcji bezpieczne połączenie między brama zarządzania interfejsu API i usług wewnętrznej. Można:

-   Za pomocą protokołu HTTP uwierzytelniania podstawowego. Aby uzyskać więcej informacji zobacz [Konfigurowanie interfejsu API ustawienia](api-management-howto-create-apis.md#configure-api-settings).
- Zgodnie z opisem w temacie [jak zabezpieczanie usług wewnętrznej za pomocą uwierzytelniania certyfikatów klientów w zarządzaniu interfejsu API Azure](api-management-howto-mutual-certificates.md)za pomocą protokołu SSL wzajemnego uwierzytelniania.
- Za pomocą IP whitelisting od usługi wewnętrznej. Jeżeli masz Standard lub Premium wystąpienia interfejsu API zarządzania warstwy, adres IP bramy pozostanie niezmieniona. Można ustawić z listy sprawdzonej, aby umożliwić ten adres IP. Na pulpicie nawigacyjnym w portalu Azure, możesz uzyskać adres IP wystąpienia interfejsu API zarządzania.
- Wystąpienia interfejsu API zarządzania nawiązać połączenia wirtualnej sieci Azure. Aby uzyskać więcej informacji zobacz [jak skonfigurować połączenia VPN w zarządzaniu interfejsu API Azure](api-management-howto-setup-vpn.md).

### <a name="how-do-i-copy-my-api-management-service-instance-to-a-new-instance"></a>Jak skopiować Moje wystąpienie usługi zarządzania interfejsu API do nowego wystąpienia?

Istnieje kilka opcji, jeśli chcesz skopiować wystąpienie zarządzania interfejsu API do nowego wystąpienia. Można:

-   Używanie narzędzia Kopia zapasowa i przywracanie funkcji zarządzania interfejsu API w. Aby uzyskać więcej informacji zobacz [Jak zaimplementować awarii przy użyciu usługi wykonywanie kopii zapasowych i przywracanie Azure interfejsu API zarządzania w](api-management-howto-disaster-recovery-backup-restore.md).
-   Tworzenie własnych kopii zapasowych i przywracanie funkcji za pomocą [Interfejsu API usługi REST zarządzania interfejsu API](https://msdn.microsoft.com/library/azure/dn776326.aspx). Zapisywanie i przywracanie obiektów z wystąpienia usługi, który ma za pomocą interfejsu API usługi REST.
-   Pobierz konfigurację usługi przy użyciu cyfra, a następnie przekazanie go do nowego wystąpienia. Aby uzyskać więcej informacji zobacz [Zapisywanie i konfigurowaniu konfiguracji zarządzania interfejsu API usługi przy użyciu cyfra](api-management-configuration-repository-git.md).

### <a name="can-i-manage-my-api-management-instance-programmatically"></a>Zarządzać Moje wystąpienia interfejsu API zarządzania programowy?

Tak, można zarządzać interfejsu API zarządzania programowo przy użyciu:

-   [Interfejs API zarządzania interfejsu API usługi REST](https://msdn.microsoft.com/library/azure/dn776326.aspx).
-   [Biblioteka zarządzania ApiManagement usług Microsoft Azure SDK](http://aka.ms/apimsdk).
-   Polecenia cmdlet [Wdrażanie usługi](https://msdn.microsoft.com/library/mt619282.aspx) i [Zarządzanie usługą](https://msdn.microsoft.com/library/mt613507.aspx) programu PowerShell.

### <a name="how-do-i-add-a-user-to-the-administrators-group"></a>Jak dodać użytkownika do grupy Administratorzy?

Poniżej opisano, jak dodać użytkownika do grupy Administratorzy:

1. Zaloguj się do [portalu Azure](https://portal.azure.com).
2. Przejdź do grupy zasobów, która ma wystąpienie interfejsu API zarządzania, które chcesz zaktualizować.
3. W obszarze Zarządzanie interfejsu API przypisać roli **Współautora zarządzania interfejsu Api** do użytkownika.

Teraz nowo dodany współautora użyć Azure programu PowerShell [poleceń cmdlet](https://msdn.microsoft.com/library/mt613507.aspx). Oto jak zalogować się jako administrator:

1. Używanie `Login-AzureRmAccount` polecenia cmdlet, aby się zalogować.
2. Ustaw kontekst subskrypcji, która jest usługa przy użyciu `Set-AzureRmContext -SubscriptionID <subscriptionGUID>`.
3. Uzyskiwanie adresu URL pojedynczego logowania jednokrotnego przy użyciu `Get-AzureRmApiManagementSsoToken -ResourceGroupName <rgName> -Name <serviceName>`.
4. Aby uzyskać dostęp do portalu administracyjnego za pomocą adresu URL.


### <a name="why-is-the-policy-that-i-want-to-add-unavailable-in-the-policy-editor"></a>Dlaczego jest zasady, które chcę dodać niedostępne w edytorze zasad?

Jeśli zasady, które chcesz dodać pojawia się wyszarzone lub cieniowania w edytorze zasad, upewnij się, że jest poprawny zakres zasady. Każdej deklaracji zasad jest przeznaczony do użycia w określonych i sekcje zasady. Aby zapoznać się z sekcji zasad i zakresów zasady, zobacz sekcję użycia zasad w [zasady zarządzania interfejsu API](https://msdn.microsoft.com/library/azure/dn894080.aspx).


### <a name="how-do-i-use-api-versioning-in-api-management"></a>Jak korzystać z interfejsu API przechowywanie wersji w interfejsu API zarządzania?

Istnieje kilka opcji za pomocą interfejsu API przechowywanie wersji w obszarze Zarządzanie interfejsu API:

-   W obszarze Zarządzanie interfejsu API można skonfigurować interfejsy API w celu odzwierciedlenia różnych wersji. Na przykład może być dwa różne interfejsy API, MyAPIv1 i MyAPIv2. Deweloper można wybrać wersję, którą chce za pomocą projektanta.
-   Możesz również skonfigurować do interfejsu API przy użyciu adresu URL usługi, który nie zawiera części wersji, na przykład https://my.api. Następnie skonfiguruj segment wersji na każdej operacji [Edycji adres URL](https://msdn.microsoft.com/library/azure/dn894083.aspx#RewriteURL) szablonu. Na przykład możesz mieć operacji za pomocą [adresu URL szablon](api-management-howto-add-operations.md#url-template) o nazwie /resource i [Ponownie zapisać adres URL](api-management-howto-add-operations.md#rewrite-url-template) szablonu o nazwie /v1/Resource. Możesz zmienić wartość części wersji osobno dla każdej operacji.
-   Jeśli chcesz zachować segment wersji "wartość domyślna" w polu adres URL interfejsu API usługi na wybranej operacji, ustaw zasadę, która używa zasady [Ustaw wewnętrznej bazy danych usługi](https://msdn.microsoft.com/library/azure/dn894083.aspx#SetBackendService) Aby zmienić ścieżkę żądanie wewnętrznej.

### <a name="how-do-i-set-up-multiple-environments-in-a-single-api"></a>Jak skonfigurować wielu środowiskach w jeden interfejs API

Aby skonfigurować wielu środowiskach, na przykład środowisku testowym i środowisku produkcyjnym w jeden interfejs API, masz dwie opcje. Można:

-   Host różne interfejsy API na tej samej dzierżawy.
-   Udostępniać samej interfejsy API na różnych dzierżaw.

### <a name="can-i-use-soap-with-api-management"></a>Czy mogę korzystać z interfejsu API zarządzania SOAP?

Obsługa [protokołu SOAP przekazująca](http://blogs.msdn.microsoft.com/apimanagement/2016/10/13/soap-pass-through/) jest już dostępny. Administratorzy mogą importować WSDL usługi protokołu SOAP i zarządzania interfejsu API Azure utworzy fronton protokołu SOAP. Dostępne dla usług SOAP są portalu Dokumentacja dewelopera, konsoli test, zasady i analizy.

### <a name="is-the-api-management-gateway-ip-address-constant-can-i-use-it-in-firewall-rules"></a>Jest stałą adres IP bramy zarządzania interfejsu API? Czy można używać go w reguły zapory?

Na poziomy Standard i Premium publiczny adres IP (VIP) dzierżawy zarządzania interfejsu API jest statyczne dla ważności dzierżawy, z pewnymi wyjątkami. Zmiany adresu IP w następujących przypadkach:

-   Usługa jest usuwana i ponownie utworzona.
-   Subskrypcja usługi jest zawieszone (na przykład w przypadku nonpayment), a następnie przywrócone.
-   Dodawanie lub usuwanie Azure wirtualnej sieci (tylko w warstwie Premium można wstawiać wirtualnej sieci).

W przypadku wdrożeń w przypadku zmiany adresu regionalne Jeśli regionu jest vacated, a następnie przywrócone (tylko w warstwie Premium można wstawiać w przypadku wdrożenia).

Dzierżaw warstwa Premium skonfigurowane na potrzeby wdrożenia wielu regionu są przypisywane jeden publiczny adres IP w rozbiciu na regiony.

Na stronie dzierżawy w portalu Azure, zostanie wyświetlony adres IP (lub adresy we wdrożeniu wielu regionów).

### <a name="can-i-configure-an-oauth-20-authorization-server-with-ad-fs-security"></a>Serwer autoryzacji OAuth 2.0 można skonfigurować przy użyciu usług AD FS zabezpieczeń?

Aby dowiedzieć się, jak skonfigurować serwer autoryzacji OAuth 2.0 z zabezpieczeniami usługi Active Directory Federation Services (AD FS), zobacz [Przy użyciu usług ADFS organizacji w zarządzaniu interfejsu API](https://phvbaars.wordpress.com/2016/02/06/using-adfs-in-api-management/).

### <a name="what-routing-method-does-api-management-use-in-deployments-to-multiple-geographic-locations"></a>Jakie metody routingu zarządzania interfejsu API używa we wdrożeniach do kilku lokalizacji geograficznej?

Interfejs API zarządzania używa [metody routingu ruchu wydajności](../traffic-manager/traffic-manager-routing-methods.md#performance-traffic-routing-method) we wdrożeniach do kilku lokalizacji geograficznej. Ruch przychodzący jest kierowane do najbliższego bramy interfejsu API. Jeśli jeden region przechodzi do trybu offline, ruch przychodzący jest automatycznie kierowane do następnego najbliższym bramy. Dowiedz się, dlaczego metody routingu w [Menedżer ruchu routingu metod](../traffic-manager/traffic-manager-routing-methods.md).

### <a name="can-i-use-an-azure-resource-manager-template-to-create-an-api-management-service-instance"></a>Do tworzenia wystąpienia usługi zarządzania interfejsu API można używać szablonu Menedżera zasobów Azure?

Wartość Tak. Zobacz szablony Szybki Start [Usługi zarządzania interfejsu API Azure](http://aka.ms/apimtemplate) .

### <a name="can-i-use-a-self-signed-ssl-certificate-for-a-back-end"></a>Czy można używać certyfikatu z podpisem własnym protokołu SSL dla wewnętrznej?

Wartość Tak. Poniżej przedstawiono sposób używania certyfikatu z podpisem Secure Sockets Layer (SSL) dla wewnętrznej:

1. Tworzenie jednostki [wewnętrznej bazy danych](https://msdn.microsoft.com/library/azure/dn935030.aspx) przy użyciu interfejsu API zarządzania.
2. Ustaw właściwość **skipCertificateChainValidation** na **PRAWDA**.
3. Jeśli już nie chcesz zezwolić certyfikatów z podpisem własnym, usunąć jednostki wewnętrznej bazy danych lub ustawić właściwość **skipCertificateChainValidation** na **wartość false**.

### <a name="why-do-i-get-an-authentication-failure-when-i-try-to-clone-a-git-repository"></a>Ustalanie przyczyny zwrócenia błędu uwierzytelniania, podczas próby klonowanie repozytorium cyfra?

Jeśli korzystasz z Menedżera poświadczeń cyfra lub jeśli próbujesz klonowanie repozytorium cyfra przy użyciu programu Visual Studio, mogą pojawić się znany problem z okna dialogowego poświadczeń systemu Windows. Okno dialogowe ogranicza długość hasła do 127 znaków, a jego obcina hasło wygenerowane przez firmy Microsoft. Pracujemy obecnie nad skrócenie hasło. Teraz Użyj imprezie cyfra na klonowanie repozytorium cyfra.

### <a name="does-api-management-work-with-azure-expressroute"></a>Interfejs API zarządzania działa Azure ExpressRoute?

Wartość Tak. Interfejs API zarządzania współdziała z Azure ExpressRoute.

### <a name="can-i-move-an-api-management-service-from-one-subscription-to-another"></a>Czy można przenosić usługi zarządzania interfejsu API z jedną subskrypcję do innego?

Wartość Tak. Aby dowiedzieć się, jak to zrobić, zobacz [Przenoszenie zasobów do nowej grupy zasobów lub subskrypcji](../resource-group-move-resources.md).
