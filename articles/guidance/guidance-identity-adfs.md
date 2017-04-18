<properties
   pageTitle="Wdrażanie usług ADFS w Azure | Microsoft Azure"
   description="Jak wdrażać bezpiecznego hybrydowych architektury sieci przy użyciu autoryzacji usługi federacyjne Active Directory platformy Azure."
   services="guidance,vpn-gateway,expressroute,load-balancer,virtual-network,active-directory"
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/13/2016"
   ms.author="telmos"/>

# <a name="implementing-active-directory-federation-services-adfs-in-azure"></a>Wdrażanie usług federacyjnych Active Directory (ADFS) w Azure

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

W tym artykule opisano najważniejsze wskazówki dotyczące implementowania który rozszerza sieci lokalnej Azure i [Active Directory Federation Services (ADFS)] , który używa sieci hybrydowych bezpiecznego[ active-directory-federation-services] w celu wykonywania federacyjnych uwierzytelniania i autoryzacji dla składniki działające w chmurze. Ta architektura pozwala korzystać ze strukturą opisaną przez [Rozszerzanie usługi Active Directory Azure][implementing-active-directory].

> [AZURE.NOTE] Azure ma dwa różne wdrożenia modele: [Menedżer zasobów] [ resource-manager-overview] i klasyczny. Ta architektura odwołanie używa Menedżera zasobów, które firma Microsoft zaleca się w przypadku wdrożeń nowy.

ADFS może zostać uruchomiony lokalnego, ale w scenariuszu hybrydowe, gdzie aplikacji znajdują się w Azure może być bardziej efektywne zaimplementować tę funkcję w chmurze. Typowe zastosowanie przypadkach dla tej architektury obejmują:

- Aplikacje hybrydowych miejsce, w którym obciążenia Uruchom częściowo lokalnego i częściowo w Azure.

- Rozwiązania, które są używane federacyjnych autoryzacji udostępniania aplikacji sieci web organizacji partnera.

- Systemy, które obsługują dostęp za pomocą przeglądarki sieci web z spoza organizacji zapory.

- Systemy, które umożliwiają dostęp do aplikacji sieci web za pomocą połączenia z autoryzowanych urządzeń zewnętrznych, takich jak komputerów zdalnych, notesów i innych urządzeń przenośnych. 

Aby uzyskać więcej informacji na temat współdziałania usług ADFS organizacji, zobacz [Omówienie usług Active Directory Federation][active-directory-federation-services-overview]. Ponadto artykuł [wdrożenia ADFS w Azure] [ adfs-intro] zawiera szczegółowe wprowadzenie krok po kroku do wykonania ADFS Azure.

## <a name="architecture-diagram"></a>Diagram architektury

Poniższy diagram wyróżnienie ważne składniki, w tym architektury (*kliknij, aby powiększyć*). Aby uzyskać więcej informacji na temat dowolnego elementu nie związane z usługami ADFS, przeczytaj [implementacji architektury sieci bezpiecznego hybrydowych platformy Azure][implementing-a-secure-hybrid-network-architecture], [implementacji architektury sieci bezpiecznego hybrydowego z dostępem do Internetu w Azure][implementing-a-secure-hybrid-network-architecture-with-internet-access]i [implementowania bezpiecznego hybrydowych architektury sieci przy użyciu tożsamości usługi Active Directory platformy Azure][implementing-active-directory]:

[! [0]][0]

>[AZURE.NOTE] Ten diagram przedstawia następujące przypadki użycia:
>
>- Kod aplikacji działa w organizacji partnera uzyskuje dostęp do aplikacji sieci web hostowanej wewnątrz VNet usługi Azure.
>
>- Użytkowników zewnętrznych, zarejestrowanych (przy użyciu poświadczeń przechowywanych w DODAJE) uzyskiwania dostępu do aplikacji sieci web hostowanej wewnątrz VNet usługi Azure.
>
>- Nawiązywanie połączenia z VNet przez używane urządzenie autoryzowanych i uruchamianie aplikacji sieci web hostowanej wewnątrz VNet usługi Azure użytkownika.
>
>Ponadto ta architektura omówiono w stronie biernej Rosyjska, w którym serwerów federacyjnych zdecyduj, jak i kiedy do uwierzytelnienia użytkownika. Użytkownik ma podanie informacji logowania podczas uruchamiania aplikacji. To jest mechanizm najczęściej używane przez przeglądarki sieci web i obejmuje protokół, który przekierowuje przeglądarki do witryny miejsce, w którym użytkownik może Podaj swoje poświadczenia. ADFS obsługuje także Federację aktywne, według której aplikacja przejmuje odpowiedzialność za podanie poświadczeń bez dalszych interakcji użytkownika, ale tym przypadku wykracza poza zakres tej architektury.

- **DODAJE podsieć.** Serwery DODAJE są zawarte w ich podsieci. Reguły NSG ochrony serwerów DODAJE i umożliwiają Zapora przeciw ruchu z nieznanych źródeł.

- **DODAJE serwery.** Są to kontrolery działającego jako maszyny wirtualne w chmurze. Tych serwerach zapewnia uwierzytelnianie tożsamości lokalnych w tej domenie.

- **Podsieć ADFS.** Serwery usług ADFS organizacji mogą być umieszczane w ich podsieci z regułami NSG działającego jako zapora.

- **Serwery usług ADFS organizacji.** Serwery ADFS zapewniają federacyjnych autoryzacji i uwierzytelniania. W tym architektura im wykonywanie następujących zadań:

    - Mogą otrzymywać tokenów zabezpieczających zawierające oświadczeniach wprowadzone przez serwer federacyjny partnera w imieniu użytkownika partnera. ADFS można sprawdzić, czy tych tokenów są prawidłowe, przed przekazaniem oświadczeniach w programie Azure aplikacji sieci web. Aplikacji firmowej sieci web (w Azure) można użyć tych roszczeń Aby autoryzować żądania. W tym scenariuszu aplikacji firmowej sieci web jest *Polegaj firmy*i jest to odpowiedzialność serwer federacyjny partnera do wykonania oświadczeń zrozumie aplikacji firmowej sieci web. Serwerów federacyjnych partnerów są określane jako *partnerów kont* ponieważ przesyłania żądań dostępu imieniu konta uwierzytelnionego w organizacji partnera. Serwery ADFS są nazywane *partnerami zasobów* , ponieważ zapewniają dostęp do zasobów (w tym przypadku aplikacji sieci web).

    - Może przeprowadzać uwierzytelnianie (za pośrednictwem [Usługi rejestracji urządzenia w usłudze Active Directory]i DODAJE[ADDRS]) i Autoryzuj przychodzących żądań od użytkowników zewnętrznych z przeglądarki sieci web lub urządzenie, które musi mieć dostęp do aplikacji sieci web firmy. 

    Serwery usług ADFS organizacji są skonfigurowane jako farmy, dostępna za pośrednictwem usługi równoważenia obciążenia Azure. Ta struktura pomaga zwiększyć dostępność i skalowalność. Należy również zauważyć, że serwer usług ADFS organizacji nie są dostępne bezpośrednio do Internetu, zamiast cały ruch internetowy jest filtrowana przez serwery proxy aplikacji sieci web usług ADFS i strefy Zdemilitaryzowanej.

- **Podsieć serwera proxy usług ADFS organizacji.** Serwery proxy usług ADFS organizacji mogą być zawarte w obrębie własnej podsieci z regułami NSG ochronę. Serwery w danej podsieci są dostępne z Internetem za pośrednictwem zestawu urządzenia wirtualnej sieci, które zapewniają zapory między Azure wirtualna sieć i Internet.

- **ADFS serwerów proxy (WAP) aplikacji sieci web.** Tych komputerów działać jako serwery ADFS dla przychodzących żądań od partnera organizacji i urządzeń zewnętrznych. Serwery WAP działają jako filtr, ekran z serwerami ADFS z bezpośredni dostęp z publicznego Internetu. Podobnie jak w przypadku serwerów ADFS wdrażanie WAP serwerach w farmie z równoważenia obciążenia umożliwia większa dostępność i skalowalność niż wdrażanie zbiór serwerach autonomicznych.

    >[AZURE.NOTE] Aby uzyskać szczegółowe informacje o instalowaniu WAP serwerów Zobacz [Instalowanie i konfigurowanie serwera Proxy aplikacji sieci Web][install_and_configure_the_web_application_proxy_server]

- **Organizacja partnera.** To jest przykład organizacji partnera uruchamianej aplikacji sieci web, która zażąda dostępu do aplikacji sieci web z platformy Azure. Serwer federacyjny w organizacji partnera uwierzytelnia żądania lokalnie i przesyła tokenów zabezpieczających zawierające wniosków ADFS działa Azure. ADFS platformy Azure sprawdza tokenów zabezpieczających, i są ważne można przesłać oświadczeniach dla aplikacji sieci web z platformy Azure Aby autoryzować ich.

    >[AZURE.NOTE] Można również skonfigurować tunelem VPN, za pomocą bramy Azure zapewniające bezpośredni dostęp do usług ADFS zaufanych partnerów. Żądania otrzymane od partnerów nie przechodzą przez serwery WAP.

## <a name="recommendations"></a>Zalecenia

W tej sekcji przedstawiono zalecenia dotyczące implementowania ADFS w Azure, obejmujące:

- Zalecenia dotyczące maszyn wirtualnych.

- Zalecenia dotyczące sieci.

- Zalecenia dotyczące dostępności.

- Zalecenia dotyczące zabezpieczeń.

- Zalecenia dotyczące instalacji usług ADFS organizacji.

- Zalecenia dotyczące zaufania usług ADFS organizacji.

### <a name="vm-recommendations"></a>Zalecenia dotyczące maszyn wirtualnych

Utwórz maszyny wirtualne z odpowiednimi zasobami obsługę przewidywaną wielkość ruchu. Użyj rozmiaru maszyn istniejących hostingu ADFS w środowisku lokalnym jako punktu początkowego. Monitorowanie wykorzystania zasobów. Można zmieniać rozmiar maszyny wirtualne i skalowanie w dół, jeśli znajdują się zbyt duży.

Postępować zgodnie z zaleceniami wymienionych w [Uruchamianie maszyn wirtualnych systemu Windows Azure][vm-recommendations].

### <a name="networking-recommendations"></a>Zalecenia dotyczące sieci

Konfigurowanie interfejsu sieciowego dla każdej maszyny wirtualne, hostingu ADFS i WAP serwer za pomocą prywatnych adresów IP.

Nie ma maszyny wirtualne ADFS publicznych adresów IP. Aby uzyskać więcej informacji, zobacz [Zagadnienia dotyczące zabezpieczeń][security-considerations].

Ustawianie adresu IP preferowanego i pomocniczego serwera DNS dla interfejsów sieciowych dla każdego ADFS i maszyn wirtualnych WAP odwołać DODAJE maszyny wirtualne (która powinna być uruchomiona DNS). Ten krok jest niezbędne w celu umożliwienia poszczególnych maszyn wirtualnych dołączyć do domeny.

### <a name="availability-recommendations"></a>Zalecenia dotyczące dostępności

Tworzenie farmy ADFS z co najmniej dwóch serwerach, aby zwiększyć dostępność usługi ADFS.

Użyj magazynu innego konta dla każdego maszyn wirtualnych ADFS w farmie. Ta metoda pozwala upewnij się, że awarii z jednego miejsca do magazynowania konta nie powoduje całej farmy niedostępne.

Tworzenie zestawów osobnych dostępność Azure dla usług ADFS i maszyny wirtualne WAP. Upewnij się, że w każdym zestawie są co najmniej dwa maszyny wirtualne. Każdy zestaw dostępność musi mieć co najmniej dwie domeny aktualizacji i dwie domeny błędów.

Konfigurowanie urządzenia do równoważenia obciążenia maszyny wirtualne ADFS i maszyny wirtualne WAP w następujący sposób:

- Korzystanie z usługi równoważenia obciążenia Azure odbiorcom maszyny wirtualne WAP dostępu zewnętrznego i równoważenia obciążenia wewnętrznych do dystrybucji obciążenia na serwerach ADFS w farmie ADFS.

- Tylko przekazywać ruch znajdujących się na serwerze usług ADFS i WAP na porcie 443 (HTTPS).

- Nadaj równoważenia obciążenia statycznego adresu IP.

- Tworzenie sondy kondycji, za pomocą protokołu TCP zamiast HTTPS. Możesz wykonać polecenie ping na porcie 443, aby sprawdzić, czy na serwerze usług ADFS działa.

    >[AZURE.NOTE] Serwery usług ADFS organizacji za pomocą protokołu wskazanie nazwa serwera (SNI), dlatego próby sondy przy użyciu punktu końcowego HTTPS z równoważenia obciążenia kończy się niepowodzeniem.

- Dodawanie rekordu DNS, *A* do domeny dla usługi równoważenia obciążenia usług ADFS organizacji. 

    Określanie adresu IP usługi równoważenia obciążenia i nadaj mu nazwę domeny (na przykład adfs.contoso.com). Jest to nazwa, której klienci i serwery WAP dostęp do farmy serwerów usług ADFS organizacji.

### <a name="security-recommendations"></a>Zalecenia dotyczące zabezpieczeń

Zapobieganie bezpośrednie narażenie serwerze usług ADFS organizacji w Internecie. Serwery usług ADFS organizacji są domeny komputerach pełny autoryzacji udzielenia tokenów zabezpieczających. Jeśli na serwerze usług ADFS organizacji jest bezpieczne, złośliwy użytkownik może wydawać tokeny pełny dostęp do wszystkich aplikacji sieci web i serwerów federacyjnych, które są chronione za pomocą usług ADFS organizacji. Jeśli system musi obsługiwać żądania od użytkowników zewnętrznych, niekoniecznie nawiązywanie połączenia z witryn zaufanych partnerów, użyj WAP serwery te żądania obsługi. Aby uzyskać więcej informacji, zobacz [gdzie umieścić federacyjnych serwerów Proxy][where-to-place-an-fs-proxy].

Serwery ADFS miejsce i serwerach WAP w osobnej podsieci z własnych zapory. Za pomocą reguł NSG do definiowania reguły zapory. Jeśli potrzebujesz bardziej zaawansowane ochrony można zaimplementować obwód zwiększenia bezpieczeństwa wokół serwery przy użyciu parę podsieci i NVAs, zgodnie z opisem w dokumencie [implementacji architektury sieci bezpiecznego hybrydowego z dostępem do Internetu w Azure][implementing-a-secure-hybrid-network-architecture-with-internet-access]. Należy zauważyć, że wszystkie zapory należy zezwalanie na ruch na porcie 443 (HTTPS).

Ograniczyć dostęp bezpośredni logowania na serwerze usług ADFS i WAP. Tylko DevOps personelu należy może się połączyć.

Nie dołączaj do serwerów WAP do domeny.

### <a name="adfs-installation-recommendations"></a>Zalecenia dotyczące instalacji usług ADFS organizacji

Artykuł [Wdrażanie farmy serwerów federacyjnych] [ Deploying_a_federation_server_farm] znajdują się szczegółowe instrukcje dotyczące instalowania i konfigurowania usług ADFS organizacji. Przed rozpoczęciem konfigurowania pierwszy serwer usług ADFS w farmie, należy wykonać następujące zadania:

1. Uzyskiwanie certyfikatu publicznym zaufaniem do uwierzytelniania serwera. *Nazwa* musi zawierać nazwę, według których klientów uzyskać dostęp do usług federacyjnych. Może to być nazwa DNS zarejestrowane dla równoważenia obciążenia, na przykład *adfs.contoso.com* (uniknąć przy użyciu nazwy symboli wieloznacznych, takich jak **. contoso.com*, ze względów bezpieczeństwa). Użyj tego samego certyfikatu na wszystkich maszyny wirtualne serwerze usług ADFS organizacji. Możesz kupić certyfikat z zaufanego urzędu certyfikacji, ale jeśli Twoja organizacja korzysta z usługi certyfikatów w usłudze Active Directory możesz utworzyć własne. 

    *Alternatywny nazwa* jest używana przez DRS umożliwiający dostęp za pomocą urządzeń zewnętrznych. To powinny być formularza *enterpriseregistration.contoso.com*.

    Aby uzyskać więcej informacji, zobacz [Uzyskiwanie i konfigurowanie certyfikatu SSL dla usług ADFS][adfs_certificates].

2. Na kontrolerze domeny Generuj nowy klucz główny usługi dystrybucji klucza. Ustaw wartość czasu skutecznych być bieżącą godzinę minus 10 godzin (tej konfiguracji zmniejsza opóźnienia, które mogą wystąpić w dystrybucji i synchronizowanie klawiszy w domenie). Ten krok jest niezbędne do obsługi tworzenia konta usługi grupy, które są używane do uruchamiania usługi ADFS. Następujące polecenia programu Powershell przedstawiono przykład jak to zrobić:

    ```powershell
    Add-KdsRootKey -EffectiveTime (Get-Date).AddHours(-10)
    ```

3. Dodaj każdego serwera ADFS maszyn wirtualnych do domeny.

>[AZURE.NOTE] Aby zainstalować ADFS, kontrolera domeny z systemem emulator podstawowego kontrolera domeny roli FSMO dla domeny musi być uruchomiony i były dostępne z usługami ADFS maszyny wirtualne.

### <a name="adfs-trust-recommendations"></a>Zalecenia dotyczące zaufania usług ADFS organizacji

Ustanowić zaufanie federacyjne między instalacji usług ADFS organizacji i serwerów federacyjnych dowolnej organizacji partnera. Konfigurowanie dowolnej oświadczeniach filtrowania i mapowania wymagane. Ten proces wymaga:

- DevOps personelu **w każdej organizacji partnera** Dodawanie uzależnionej zaufania firmy dla aplikacji sieci web dostępne za pośrednictwem serwery usług ADFS organizacji.

- DevOps personelu **w Twojej organizacji** skonfigurowanie dostawcy oświadczeń zaufania umożliwiające serwery ADFS, aby zaufać oświadczeń, które zapewniają organizacje partnerów.

- DevOps personelu **w Twojej organizacji** do konfigurowania usług ADFS organizacji w celu przekazania roszczeń do aplikacji sieci web Twojej organizacji.

    Aby uzyskać więcej informacji, zobacz [Ustanawianie zaufania federacji][establishing-federation-trust].

Publikowanie aplikacji sieci web organizacji i uzyskiwać do nich dostęp dla partnerów zewnętrznych za pomocą wstępne uwierzytelnienie przez serwery WAP. Aby uzyskać więcej informacji zobacz [Publikowanie aplikacji przy użyciu było wstępnie uwierzytelnić ADFS][publish_applications_using_AD_FS_preauthentication]

Należy zauważyć, że ADFS obsługuje token przekształcenie i powiększenia. Ta funkcja nie umożliwia Azure Active Directory. Z usługami ADFS podczas konfigurowania relacje zaufania, możesz wykonać następujące czynności:

- Konfigurowanie przekształcenia roszczeń dla reguły autoryzacji. Na przykład można mapować grupy zabezpieczeń z postać używane przez organizację partnera firmy Microsoft, aby coś, co tego DODAJE można autoryzować w Twojej organizacji.

- Przekształcanie wniosków z jednego formatu. Na przykład można mapować z SAML 2.0 do SAML 1.1 Jeśli aplikacja obsługuje tylko oświadczeniach SAML 1.1. 

## <a name="availability-considerations"></a>Zagadnienia dotyczące dostępności

SQL Server lub Windows wewnętrznej bazy danych (szczególną uwagę) służy do przechowywania informacji o konfiguracji usług ADFS organizacji. Szczególną uwagę nadmiarowości podstawowe. Zmiany są zapisywane bezpośrednio do tylko jednego ADFS baz danych w grupie ADFS, podczas gdy inne serwery na bieżąco ich bazy danych za pomocą replikacji. Za pomocą programu SQL Server umożliwiają nadmiarowości pełnej bazy danych i wysokiej dostępności za pomocą pracy awaryjnej klaster lub odzwierciedlające.

## <a name="security-considerations"></a>Zagadnienia dotyczące zabezpieczeń

ADFS używającego protokołu HTTPS, dlatego należy się upewnić, że zasady NSG podsieci zawierającej sieci web poziomu żądania HTTPS maszyny wirtualne o zezwolenie. Te żądania może prowadzić z sieci lokalnej podsieci zawierający warstwa, warstwa firm, warstwy danych, prywatne strefy Zdemilitaryzowanej, publicznej strefy Zdemilitaryzowanej i podsieci zawierającej serwery usług ADFS organizacji.

Warto rozważyć użycie zestawu urządzeń wirtualnych sieci dzienniki szczegółowe informacje na temat ruchu Przechodzenie krawędzi wirtualnej sieci inspekcja celów.

## <a name="scalability-considerations"></a>Zagadnienia dotyczące skalowalność

Następujące kwestie podsumowane z dokumentu, [Planowanie wdrożenia ADFS][plan-your-adfs-deployment], nadaj punkt wyjścia do zmiany rozmiaru farmach ADFS:

- Jeśli masz mniej niż 1000 użytkowników, nie należy tworzyć dedykowane serwery usług ADFS organizacji, ale zamiast tego zainstalować ADFS na wszystkich serwerach DODAJE w chmurze (Upewnij się, że masz co najmniej dwa serwery DODAJE, aby zachować dostępność). Tworzenie pojedynczego serwera WAP.

- Jeśli masz między 1000 a 15000 użytkowników, należy utworzyć dwa serwery ADFS dedykowane i dwa serwery dedykowane WAP.

- Jeśli masz między użytkownikami 15000 i 60000, Utwórz między trzech do pięciu dedykowane ADFS serwerów i co najmniej dwa dedykowane WAP.

Te dane liczbowe założono, że używasz dwóch cztery podstawowe maszyny wirtualne (D4_v2 standardowy lub lepiej) do obsługi serwerów w Azure.

Zauważ, że jeśli korzystasz z systemu Windows wewnętrznej bazy danych do przechowywania danych konfiguracji usług ADFS organizacji, możesz są ograniczone do osiem serwerów ADFS w farmie. Jeśli przewidujesz wymagających więcej, należy użyć programu SQL Server. Aby uzyskać więcej informacji, zobacz [Roli bazy danych konfiguracji ADFS][adfs-configuration-database].

## <a name="management-considerations"></a>Uwagi dotyczące zarządzania

DevOps personelu należy przygotować się do wykonywania następujących zadań:

- Zarządzanie serwerów federacyjnych (Zarządzanie farmy ADFS, Zarządzanie zasadami zaufania na serwerach federacyjnych i zarządzanie certyfikaty używane przez usługi federacyjne).

- Zarządzanie serwerami WAP (Zarządzanie farmy WAP, zarządzanie certyfikaty WAP).

- Zarządzanie aplikacjami sieci web (konfigurowania strony ufającej, metod uwierzytelniania i mapowania oświadczeniach).

- Tworzenie kopii zapasowej składniki usług ADFS organizacji.

## <a name="monitoring-considerations"></a>Zagadnienia dotyczące monitorowania

[Microsoft System Center zarządzania Pack dla Active Directory Federacja usług 2012 R2] [ oms-adfs-pack] zawiera zarówno proaktywne i interaktywne monitorowanie wdrożenia ADFS na serwerze federacyjnym. Ten pakiet zarządzania monitoruje:

- Zdarzenia, która ADFS usług rekordów ADFS dzienniki zdarzeń.

- Dane dotyczące wydajności, który zbieranie liczników wydajności usług ADFS organizacji. 

- Ogólny stan aplikacji sieci web i system ADFS (uzależnionej strony), a także alerty dotyczące problemów krytycznych i ostrzeżeń.

## <a name="solution-components"></a>Składniki rozwiązania

Przykładowy skrypt rozwiązanie, [Rozmieszczanie ReferenceArchitecture.ps1][solution-script], jest dostępny, której można zaimplementować architektura znajdujący się zalecenia opisane w tym artykule. Ten skrypt wykorzystuje szablony Azure Menedżera zasobów. Szablony są dostępne jako zestaw podstawowych bloków konstrukcyjnych, z których każdy będzie wykonuje określonych akcji, takich jak tworzenie VNet lub konfigurowanie NSG. Przeznaczenie skrypt należy dodać akompaniament rozmieszczania szablonu.

Szablony są sparametryzowane z parametrami przechowywanych w oddzielnym pliku JSON. Możesz zmienić parametrów w tych plików, aby skonfigurować wdrożenie do własnych wymagań. Nie trzeba zmienić szablony się. Należy zauważyć, że nie można zmienić schematy obiektów w plikach parametru.

Podczas edytowania szablonów tworzyć obiektów, które należy wykonać konwencje nazewnictwa opisanego w [Zalecane konwencje nazewnictwa dla zasobów Azure][naming-conventions].

Rozwiązanie przykładowe tworzy i konfiguruje środowisko w chmurze obejmujących strefy Zdemilitaryzowanej podsieci i serwerów, podsieci usług ADFS organizacji i serwerów, podsieci serwera proxy usług ADFS organizacji i serwerów, DODAJE, warstwa, warstwa firm i składniki warstwy danych programu access, Brama VPN i zarządzania warstwy. Przykładowe rozwiązanie zawiera również opcjonalnym związane z tworzeniem symulowany lokalnego środowiska.

W poniższych sekcjach opisano elementy w lokalnej i w chmurze konfiguracji.

### <a name="on-premises-components"></a>Składniki lokalnego

>[AZURE.NOTE] Składniki są głównym celem architektura opisanych w tym dokumencie i znajdują się po prostu, aby mieć możliwość testowania środowiska chmury bezpiecznie, a nie przy użyciu środowisku produkcyjnym rzeczywistą. Z tego powodu w tej sekcji przedstawiono tylko pliki parametru klucza. Można modyfikować ustawienia, takie jak adresy IP lub rozmiarów maszyny wirtualne, ale zaleca się pozostawienie wiele innych parametrów bez zmian.

Las AD dla domeny o nazwie contoso.com składa się z tego środowiska. Domena zawiera dwa serwery DODAJE z adresami IP 192.168.0.4 i 192.168.0.5. Te dwa serwery również uruchomić usługę DNS. Lokalnego konta administratora na obu maszyny wirtualne jest nazywany `testuser` przy użyciu hasła `AweS0me@PW`. Ponadto konfiguracji konfiguruje bramą VPN do łączenia się z VNet w chmurze. Konfiguracja można modyfikować, edytując następujące pliki JSON znajduje się w [**parametrów onpremise**] [ on-premises-folder] folder:

- ** [virtualNetwork.parameters.json][on-premises-vnet-parameters]**. Ten plik definiuje przestrzeń adresów sieciowych w środowisku lokalnym.

- ** [virtualMachines adds.parameters.json][on-premises-virtualmachines-adds-parameters]**. Ten plik zawiera konfigurację maszyny wirtualne lokalnej usługi DODAJE obsługi. Domyślnie są tworzone dwa *standardowe-DS3-wersji 2* maszyny wirtualne.

- ** [virtualNetworkGateway.parameters.json] [ on-premises-virtualnetworkgateway-parameters] ** i ** [connection.parameters.json][on-premises-connection-parameters]**. Te pliki przytrzymaj ustawień połączenia VPN do bramy Azure VPN w chmurze, w tym klucz może być używany do ochrony ruchu przechodzenie bramy.

Pozostałe pliki w folderze zawierają informacje o konfiguracji użyte do utworzenia domeny lokalnej za pomocą tej infrastruktury. Używać ich do zainstalowania DODAJE, ustawień DNS, utworzyć las i konfigurowanie witryn replikacji dla las.

### <a name="cloud-components"></a>Składniki chmury

Składniki formularza podstawową tej architektury. [**Parametry azure**] [ azure-folder] folder zawiera następujące pliki parametru dotyczące konfigurowania tych składników:

- ** [virtualNetwork.parameters.json][vnet-parameters]**. Ten plik definiuje strukturę VNet maszyny wirtualne i innych składników w chmurze. Zawiera ustawień, takich jak nazwa, przestrzeni adresów, podsieci i adresy wszelkie wymagane serwery DNS. Należy zauważyć, że adresy DNS, w tym przykładzie odwołanie adresy IP lokalne serwery DNS, a także domyślnego serwera Azure DNS. Modyfikowanie te adresy, aby odwołać ustawień DNS, jeśli nie używasz przykładowe lokalnego środowiska:

    ```json
    {
        "virtualNetworkSettings": {
            "value": {
                "name": "ra-adfs-vnet",
                "resourceGroup": "ra-adfs-network-rg",
                "addressPrefixes": [
                    "10.0.0.0/16"
                ],
                "subnets": [
                    {
                        "name": "dmz-private-in",
                        "addressPrefix": "10.0.0.0/27"
                    },
                    {
                        "name": "dmz-private-out",
                        "addressPrefix": "10.0.0.32/27"
                    },
                    {
                        "name": "dmz-public-in",
                        "addressPrefix": "10.0.0.64/27"
                    },
                    {
                        "name": "dmz-public-out",
                        "addressPrefix": "10.0.0.96/27"
                    },
                    {
                        "name": "mgmt",
                        "addressPrefix": "10.0.0.128/25"
                    },
                    {
                        "name": "GatewaySubnet",
                        "addressPrefix": "10.0.255.224/27"
                    },
                    {
                        "name": "web",
                        "addressPrefix": "10.0.1.0/24"
                    },
                    {
                        "name": "biz",
                        "addressPrefix": "10.0.2.0/24"
                    },
                    {
                        "name": "data",
                        "addressPrefix": "10.0.3.0/24"
                    },
                    {
                        "name": "adds",
                        "addressPrefix": "10.0.4.0/27"
                    },
                    {
                        "name": "adfs",
                        "addressPrefix": "10.0.5.0/27"
                    },
                    {
                        "name": "proxy",
                        "addressPrefix": "10.0.6.0/27"
                    }
                ],
                "dnsServers": [
                    "192.168.0.4",
                    "192.168.0.5",
                    "168.63.129.16"
                ]
            }
        }
    }
    ```

- ** [virtualMachines adds.parameters.json ] [ virtualmachines-adds-parameters] **. Ten plik umożliwia skonfigurowanie maszyny wirtualne, uruchomiony DODAJE w chmurze. Konfiguracja składa się z dwóch maszyny wirtualne. Należy zmienić nazwę użytkownika administratora i hasło w `virtualMachineSettings` sekcję, a opcjonalnie można zmodyfikować rozmiar pamięci Wirtualnej zgodnie z wymaganiami dotyczącymi domeny:

    Aby uzyskać więcej informacji, zobacz [Rozszerzanie usługi Active Directory Azure][extending-ad-to-azure].

    ```json
            "virtualMachinesSettings": {
                "value": {
                    "namePrefix": "ra-adfs-ad",
                    "computerNamePrefix": "aad",
                    "size": "Standard_DS3_v2",
                    "osType": "Windows",
                    "adminUsername": "testuser",
                    "adminPassword": "AweS0me@PW",
                    "osAuthenticationType": "password",
                    "nics": [
                        {
                            "isPublic": "false",
                            "subnetName": "adds",
                            "privateIPAllocationMethod": "Static",
                            "startingIPAddress": "10.0.4.4",
                            "enableIPForwarding": false,
                            "dnsServers": [
                            ],
                            "isPrimary": "true"
                        }
                    ],
                    "imageReference": {
                        "publisher": "MicrosoftWindowsServer",
                        "offer": "WindowsServer",
                        "sku": "2012-R2-Datacenter",
                        "version": "latest"
                    },
                    "dataDisks": {
                        "count": 1,
                        "properties": {
                            "diskSizeGB": 127,
                            "caching": "None",
                            "createOption": "Empty"
                        }
                    },
                    "osDisk": {
                        "caching": "ReadWrite"
                    },
                    "extensions": [
                    ],
                    "availabilitySet": {
                        "useExistingAvailabilitySet": "No",
                        "name": "ra-adfs-as"
                    }
                }
            },
            "virtualNetworkSettings": {
                "value": {
                    "name": "ra-adfs-vnet",
                    "resourceGroup": "ra-adfs-network-rg"
                }
            },
            "buildingBlockSettings": {
                "value": {
                    "storageAccountsCount": 2,
                    "vmCount": 2,
                    "vmStartIndex": 1
                }
            }
        }
    ```

- ** [Dodawanie dodaje domeny controller.parameters.json][add-adds-domain-controller-parameters]**. Ten plik zawiera ustawienia dotyczące tworzenia domeny firmy CONTOSO, obejmujące serwery DODAJE. Używa rozszerzeń niestandardowych ustanawiania domeny i Dodaj serwery DODAJE do niego. Chyba że możesz utworzyć dodatkowe serwery DODAJE (w tym przypadku należy dodać je do `vms` tablica), zmienianie ich nazw w domyślnej lub chcesz utworzyć domenę pod inną nazwą, nie trzeba modyfikować ten plik.

- ** [loadBalancer adfs.parameters.json] [loadbalancer-adfs-parameters] ** Plik zawiera dwa zestawy parametrów konfiguracji. `virtualMachineSettings` Sekcja definiuje maszyny wirtualne obsługujące usługi ADFS w chmurze. Domyślnie skrypt tworzy dwóch z tych maszyny wirtualne w ten sam zestaw dostępności:

    ```json
        "virtualMachinesSettings": {
            "value": {
                "namePrefix": "ra-adfs-adfs",
                "computerNamePrefix": "adfs",
                "size": "Standard_DS1_v2",
                "osType": "windows",
                "adminUsername": "testuser",
                "adminPassword": "AweS0me@PW",
                "osAuthenticationType": "password",
                "nics": [
                    {
                        "isPublic": "false",
                        "subnetName": "adfs",
                        "privateIPAllocationMethod": "Static",
                        "startingIPAddress": "10.0.5.4",
                        "isPrimary": "true",
                        "enableIPForwarding": false,
                        "dnsServers": [ ]
                    }
                ],
                "imageReference": {
                    "publisher": "MicrosoftWindowsServer",
                    "offer": "WindowsServer",
                    "sku": "2012-R2-Datacenter",
                    "version": "latest"
                },
                "dataDisks": {
                    "count": 1,
                    "properties": {
                        "diskSizeGB": 128,
                        "caching": "None",
                        "createOption": "Empty"
                    }
                },
                "osDisk": {
                    "caching": "ReadWrite"
                },
                "extensions": [ ],
                "availabilitySet": {
                    "useExistingAvailabilitySet": "No",
                    "name": "ra-adfs-adfs-vm-as"
                }
            }
        }
        ...
        "buildingBlockSettings": {
            "value": {
                "storageAccountsCount": 2,
                "vmCount": 2,
                "vmStartIndex": 1
            }
        }
    ```

    `loadBalancerSettings` Sekcja zawiera opis usługi równoważenia obciążenia dla tych maszyny wirtualne. Usługi równoważenia obciążenia przekazuje ruch, który pojawia się na porcie 443 (HTTPS) do jednej lub inne maszyny wirtualne:

    ```json
        "loadBalancerSettings": {
            "value": {
                "name": "ra-adfs-adfs-lb",
                "frontendIPConfigurations": [
                    {
                        "name": "ra-adfs-adfs-lb-fe",
                        "loadBalancerType": "internal",
                        "internalLoadBalancerSettings": {
                            "privateIPAddress": "10.0.5.30",
                            "subnetName": "adfs"
                        }
                    }
                ],
                "backendPools": [
                    {
                        "name": "ra-adfs-adfs-lb-bep",
                        "nicIndex": 0
                    }
                ],
                "loadBalancingRules": [
                    {
                        "name": "https-rule",
                        "frontendPort": 443,
                        "backendPort": 443,
                        "protocol": "Tcp",
                        "backendPoolName": "ra-adfs-adfs-lb-bep",
                        "frontendIPConfigurationName": "ra-adfs-adfs-lb-fe",
                        "probeName": "https-probe",
                        "enableFloatingIP": false
                    }
                ],
                "probes": [
                    {
                        "name": "https-probe",
                        "port": 443,
                        "protocol": "Tcp",
                        "requestPath": null
                    }
                ],
                "inboundNatRules": [ ]
            }
        },
        "virtualNetworkSettings": {
            "value": {
                "name": "ra-adfs-vnet",
                "resourceGroup": "johns-adfs-network-rg"
            }
        }
    ```

- ** [adfs farmy domeny join.parameters.json ] [ adfs-farm-domain-join-parameters] **. Ten plik zawiera ustawienia używane do dodawania serwerów ADFS domeny firmy CONTOSO. Należy zmodyfikować ten plik, jeśli użytkownik utworzył dodatkowe serwery ADFS (aktualizowanie `vms` tablicy w tym przypadku), lub zmianie nazwy domeny.

- ** [gmsa.parameters.json][gmsa-parameters]**, ** [adfs farmy first.parameters.json][adfs-farm-first-parameters]**, i ** [adfs farmy rest.parameters.json][adfs-farm-rest-parameters]**. Skrypt używa ustawień te pliki do utworzenia farmy serwerów usług ADFS organizacji. 

    Plik *gmsa.parameters.json* zawiera ustawień konta usługi grupa zarządzane używane przez usługę ADFS. Można zmodyfikować ten plik, jeśli chcesz zmienić nazwę konta lub domeny.

    Plik *adfs farmy first.parameters.json* zawiera informacje potrzebne do utworzenia farmy serwerów usług ADFS organizacji oraz dodawanie pierwszy serwer. Jeśli zmienisz nazwę domeny lub grupy zarządzane konta usługi, należy zaktualizować ten plik.

    Plik *adfs farmy rest.parameters.json* służy do dodawania pozostałe serwery ADFS do farmy. Ponownie Jeśli zmienisz nazwę domeny lub grupy zarządzane konta usługi, należy zaktualizować ten plik. Aktualizacja `vms` tablicy, jeśli użytkownik utworzył dodatkowe serwery usług ADFS organizacji.

- ** [loadBalancer adfsproxy.parameters.json][loadBalancer-adfsproxy-parameters]**. Ten plik jest podobne w strukturze i zawartości do pliku *loadBalancer adfs.parameters.json* . Zawiera dane dotyczące tworzenia serwery proxy usług ADFS i równoważenia obciążenia.

- ** [adfsproxy — farmy — first.parameters.json][adfsproxy-farm-first-parameters]**, i ** [adfsproxy — farmy — rest.parameters.json][adfsproxy-farm-rest-parameters]**. Skrypt używa ustawień te pliki do utworzenia farmy serwerów proxy usług ADFS organizacji. 

- ** [virtualNetworkGateway.parameters.json][virtualnetworkgateway-parameters]**. Ten plik zawiera ustawienia służące do tworzenia bramy Azure VPN w chmurze używane do łączenia się z siecią w lokalnej. Należy zmodyfikować `sharedKey` wartość w `connectionsSettings` sekcji z urządzenia VPN lokalnego. Aby uzyskać więcej informacji, zobacz [implementacji hybrydowych architektury sieci przy użyciu Azure i lokalnych VPN][hybrid-azure-on-prem-vpn].

- ** [strefy zdemilitaryzowanej private.parameters.json] [ dmz-private-parameters] ** i ** [strefy zdemilitaryzowanej public.parameters.json ] [ dmz-public-parameters] **. Te pliki Konfigurowanie przychodzącej (publiczna) i ruchu wychodzącego strony (prywatnych) maszyny wirtualne, które składają się strefy Zdemilitaryzowanej, ochrona serwerów w chmurze. Aby uzyskać więcej informacji na temat tych elementów i ich konfiguracji, zobacz [implementacji strefy Zdemilitaryzowanej między Azure i Internet][implementing-a-secure-hybrid-network-architecture-with-internet-access].

- ** [loadBalancer-web.parameters-json][loadBalancer-web-parameters]**, ** [loadBalancer-biz.parameters-json][loadBalancer-biz-parameters]**, i ** [loadBalancer-data.parameters-json][loadBalancer-data-parameters]**. Te pliki parametry zawierają specyfikacje maszyn wirtualnych dla poziomów dostępu do sieci web, firm i danych, a następnie skonfiguruj urządzenia do równoważenia obciążenia dla każdego poziomu. Są to maszyny wirtualne, które obsługi aplikacji sieci web i baz danych i wykonać obciążenia firm dla organizacji. Można modyfikować rozmiary i liczby maszyny wirtualne w każdej warstwie, zgodnie z własnymi potrzebami.

- ** [virtualMachines mgmt.parameters.json][virtualMachines-mgmt-parameters]**. Ten plik zawiera konfigurację skoku pola i zarządzania maszyny wirtualne. Tylko istnieje możliwość uzyskania logowania i dostęp administracyjny do maszyny wirtualne w sieci web, firm i poziomów danych w polu skok. Domyślnie skrypt tworzy pojedynczy *Standard_DS1_v2* maszyn wirtualnych, ale można modyfikować ten plik, aby utworzyć maszyny wirtualne zwiększenie lub dodatkowe, jeśli obciążenie pracą zarządzania prawdopodobnie znacząca.

## <a name="solution-deployment"></a>Wdrożenie rozwiązania

Rozwiązanie zakłada następujące wymagania:

- Masz istniejącej subskrypcji Azure, w którym można tworzyć grup zasobów.

- Zostały pobrane i zainstalowane najnowszej kompilacji programu Azure Powershell. Zobacz [tutaj] [ azure-powershell-download] instrukcje.

Aby uruchomić skrypt, który wdraża rozwiązanie:

1. Przenieś do folderu wygodny na komputerze lokalnym i Utwórz następujące podfoldery:

    - Skryptów

    - Skryptów i parametrów

    - Skryptów i parametrów/Onpremise

    - Azure skryptów i parametrów

2. Pobierz [Rozmieszczanie ReferenceArchitecture.ps1] [ solution-script] pliku do folderu skryptów

3. Pobierz zawartość [parametrów onpremise] [ on-premises-folder] folderu skryptów i parametrów/Onpremise:

4. Pobierz zawartość [parametrów azure] [ azure-folder] folderu skryptów i parametrów-Azure.

5. Edytowanie pliku ReferenceArchitecture.ps1 rozmieszczanie w folderze skryptów, a następnie Zmień następujące wiersze, aby określić grup zasobów, które powinny zostać utworzona lub służy do przechowywania zasobów utworzone przez skrypt:

    ```powershell
    # Azure Onpremise Deployments
        $onpremiseNetworkResourceGroupName = "ra-adfs-onpremise-rg"
        
        # Azure ADDS Deployments
        $azureNetworkResourceGroupName = "ra-adfs-network-rg"
        $workloadResourceGroupName = "ra-adfs-workload-rg"
        $securityResourceGroupName = "ra-adfs-security-rg"
        $addsResourceGroupName = "ra-adfs-adds-rg"
        $adfsResourceGroupName = "ra-adfs-adfs-rg"
        $adfsproxyResourceGroupName = "ra-adfs-proxy-rg"
    ```

6. Możesz edytować plików parametr w folderach skryptów i parametrów-Azure i skryptów i parametrów-Onpremise. Aktualizowanie odwołań grupa zasobów w tych plików, aby dopasować nazwy grup zasobów przydzielonych do zmiennych w pliku ReferenceArchitecture.ps1 rozmieszczanie. Poniższa tabela przedstawia pliki, które parametru odwołanie grupę zasobów. Zauważ, że grupy *Pomoc zdalna adfs-obciążenie pracą rg*, *Pomoc zdalna adfs zabezpieczeń rg* *Pomoc zdalna adfs dodaje rg*, *Pomoc zdalna adfs-adfs-rg*i *Pomoc zdalna adfs-proxy-rg* są używane tylko w skrypt programu PowerShell i nie występują w plikach parametru.

  	|Grupa zasobów|Parametr plików|
  	|--------------|--------------|
  	|Pomoc zdalna adfs-onpremise-rg|parameters\onpremise\connection.parameters.JSON<br /> parameters\onpremise\virtualMachines-ADDS.parameters.JSON<br />parameters\onpremise\virtualNetwork-ADDS-DNS.parameters.JSON<br />parameters\onpremise\virtualNetwork.parameters.JSON<br />parameters\onpremise\virtualNetworkGateway.parameters.JSON<br />parameters\azure\virtualNetworkGateway.parameters.JSON
  	|Pomoc zdalna adfs sieć rg|parameters\onpremise\connection.parameters.JSON<br />parameters\azure\dmz-Private.parameters.JSON<br />parameters\azure\dmz-public.parameters.JSON<br />parameters\azure\loadBalancer-ADFS.parameters.JSON<br />parameters\azure\loadBalancer-adfsproxy.parameters.JSON<br />parameters\azure\loadBalancer-Biz.parameters.JSON<br />parameters\azure\loadBalancer-Data.parameters.JSON<br />parameters\azure\loadBalancer-Web.parameters.JSON<br />parameters\azure\virtualMachines-ADDS.parameters.JSON<br />parameters\azure\virtualMachines-Mgmt.parameters.JSON<br />parameters\azure\virtualNetwork-with-onpremise-and-Azure-DNS.parameters.JSON<br />parameters\azure\virtualNetwork.parameters.JSON<br />parameters\azure\virtualNetworkGateway.parameters.JSON (*dwa wystąpienia*)

    Ponadto konfiguracji dla lokalnego i w chmurze składniki, zgodnie z opisem w sekcji [Components rozwiązanie] [składniki rozwiązania].

7. Otwórz okno programu PowerShell Azure, przejdź do folderu skryptów i uruchom następujące polecenie:

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> <mode>
    ```

    Zamienianie `<subscription id>` ze swojego identyfikatora Azure subskrypcji.

    Aby uzyskać `<location>`, określ Azure regionu, takich jak `eastus` lub `westus`.

    `<mode>` Parametr może mieć jedną z następujących wartości:

    - `Onpremise`, aby utworzyć symulowany lokalnego środowiska.

    - `Infrastructure`, tworzyć infrastruktury VNet i przejść pola w chmurze.

    - `CreateVpn`, aby utworzyć bramę Azure wirtualnej sieci i połączyć go do sieci lokalnej.

    - `AzureADDS`, sporządzenie maszyny wirtualne, działające jako serwery DODAJE wdrożenia usługi Active Directory, aby te maszyny wirtualne i tworzenia domeny w chmurze.

    - `AdfsVm`Aby utworzyć maszyny wirtualne ADFS i połączyć je do domeny w chmurze.

    - `ProxyVm`Aby utworzyć serwer proxy usług ADFS maszyny wirtualne i połączyć je do domeny w chmurze.

    - `Prepare`, który wykonuje powyższych zadań. **Jeśli tworzysz całkowicie nowych wdrożenia i nie mam istniejącej infrastruktury lokalnej jest zalecana opcja.**

    >[AZURE.NOTE] Możesz również uruchomić skrypt z `<mode>` parametr `Workload` do tworzenia sieci web, business, a warstwy danych maszyny wirtualne i sieci. To ustawienie nie jest częścią `Prepare` tryb.

    Jeśli korzystasz z `Prepare` opcji skrypt zajmie kilka godzin, a kończy z tą wiadomością *Przygotowanie zostanie zakończone. Zainstaluj certyfikat do wszystkich usług ADFS i serwer proxy maszyny wirtualne.*

8.  Aby umożliwić jego ustawienia DNS zostały zastosowane, uruchom ponownie polu skok (*Pomoc zdalna adfs zarządzania vm1* w grupie *zabezpieczeń rg, Pomoc zdalna adfs w-* ).

9.  [Uzyskaj certyfikat SSL dla usług ADFS] [ adfs_certificates] i zainstalować ten certyfikat na maszyny wirtualne ADFS. Zauważ, że możesz połączyć z maszyny wirtualne ADFS w ramce skok. Adresy IP są *10.0.5.4* i *10.0.5.5*. Domyślna nazwa użytkownika to *contoso\testuser* przy użyciu hasła *AweSome@PW*.

    >[AZURE.NOTE] Komentarze skryptu ReferenceArchitecture.ps1 rozmieszczanie na tym etapie zawiera szczegółowych instrukcji dotyczących tworzenia certyfikatu testowego podpisem i przy użyciu urząd `makecert` polecenia. Jednak nie należy używać certyfikatów generowanych przez makecert w środowisku produkcyjnym.

10. Uruchom następujące polecenie programu Powershell do konfigurowania farmy serwerów usług ADFS organizacji:

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Adfs
    ```

11. W oknie dialogowym skoku przejdź do *https://adfs.contoso.com/adfs/ls/idpinitiatedsignon.htm* , aby przetestować instalację ADFS (może pojawić się ostrzeżenie o certyfikat, który można zignorować ten test). Upewnij się, że zostanie wyświetlona strona logowania Contoso Corporation. Zaloguj się jako *contoso\testuser* przy użyciu hasła *AweS0me@PW*.

12. Instalowanie certyfikatu SSL na serwerze proxy usług ADFS maszyny wirtualne. Adresy IP są *10.0.6.4* i *10.0.6.5*.

13. Uruchom następujące polecenie programu Powershell, aby skonfigurować pierwszy serwer proxy usług ADFS organizacji:

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Proxy1
    ```

14. Wykonaj instrukcje wyświetlane przez skrypt testowanie instalacji pierwszego serwera proxy usług ADFS organizacji.

15. Uruchom następujące polecenie programu Powershell, aby skonfigurować drugi pierwszy serwer proxy usług ADFS organizacji:

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Proxy2
    ```

16. Postępuj zgodnie z instrukcjami wyświetlane przez skrypt do testowania zakończenie konfiguracji serwera proxy usług ADFS organizacji.

## <a name="next-steps"></a>Następne kroki

- [Azure Active Directory][aad].

- [Azure Active Directory B2C][aadb2c].

<!-- links -->

[vm-recommendations]: ./guidance-compute-single-vm.md#Recommendations
[naming-conventions]: ./guidance-naming-conventions.md
[implementing-active-directory]: ./guidance-identity-adds-extend-domain.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[implementing-a-secure-hybrid-network-architecture]: ./guidance-iaas-ra-secure-vnet-hybrid.md
[implementing-a-secure-hybrid-network-architecture-with-internet-access]: ./guidance-iaas-ra-secure-vnet-dmz.md
[DRS]: https://technet.microsoft.com/library/dn280945.aspx
[where-to-place-an-fs-proxy]: https://technet.microsoft.com/library/dd807048.aspx
[ADDRS]: https://technet.microsoft.com/library/dn486831.aspx
[plan-your-adfs-deployment]: https://msdn.microsoft.com/library/azure/dn151324.aspx
[ad_network_recommendations]: #network_configuration_recommendations_for_AD_DS_VMs
[domain_and_forests]: https://technet.microsoft.com/library/cc759073(v=ws.10).aspx
[adfs_certificates]: https://technet.microsoft.com/library/dn781428(v=ws.11).aspx
[create_service_account_for_adfs_farm]: https://technet.microsoft.com/library/dd807078.aspx
[import_server_authentication_certificate]: https://technet.microsoft.com/library/dd807088.aspx
[adfs-configuration-database]: https://technet.microsoft.com/en-us/library/ee913581(v=ws.11).aspx
[active-directory-federation-services]: https://technet.microsoft.com/windowsserver/dd448613.aspx
[security-considerations]: #security-considerations
[recommendations]: #recommendations
[claims-aware applications]: https://msdn.microsoft.com/en-us/library/windows/desktop/bb736227(v=vs.85).aspx
[active-directory-federation-services-overview]: https://technet.microsoft.com/en-us/library/hh831502(v=ws.11).aspx
[establishing-federation-trust]: https://blogs.msdn.microsoft.com/alextch/2011/06/27/establishing-federation-trust/
[Deploying_a_federation_server_farm]: https://technet.microsoft.com/library/dn486775.aspx
[install_and_configure_the_web_application_proxy_server]: https://technet.microsoft.com/library/dn383662.aspx
[publish_applications_using_AD_FS_preauthentication]: https://technet.microsoft.com/library/dn383640.aspx
[managing-adfs-components]: https://technet.microsoft.com/library/cc759026.aspx
[oms-adfs-pack]: https://www.microsoft.com/download/details.aspx?id=41184
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[aad]: https://azure.microsoft.com/documentation/services/active-directory/
[aadb2c]: https://azure.microsoft.com/documentation/services/active-directory-b2c/
[adfs-intro]: ../active-directory/active-directory-aadconnect-azure-adfs.md
[solution-script]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/Deploy-ReferenceArchitecture.ps1
[on-premises-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-identity-adfs/parameters/onpremise
[on-premises-vnet-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/onpremise/virtualNetwork.parameters.json
[on-premises-virtualmachines-adds-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/onpremise/virtualMachines-adds.parameters.json
[on-premises-virtualnetworkgateway-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/onpremise/virtualNetworkGateway.parameters.json
[on-premises-connection-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/onpremise/connection.parameters.json
[azure-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-identity-adfs/parameters/azure
[vnet-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/virtualNetwork.parameters.json
[dmz-private-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/dmz-private.parameters.json
[dmz-public-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/dmz-public.parameters.json
[virtualnetworkgateway-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/virtualNetworkGateway.parameters.json
[hybrid-azure-on-prem-vpn]: ./guidance-hybrid-network-vpn.md
[virtualmachines-adds-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/virtualMachines-adds.parameters.json
[extending-ad-to-azure]: ./guidance-identity-adds-extend-domain.md
[loadbalancer-adfs-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/loadBalancer-adfs.parameters.json
[add-adds-domain-controller-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/add-adds-domain-controller.parameters.json
[gmsa-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/gmsa.parameters.json
[adfs-farm-first-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/adfs-farm-first.parameters.json
[adfs-farm-rest-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/adfs-farm-rest.parameters.json
[adfs-farm-domain-join-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/adfs-farm-domain-join.parameters.json
[loadBalancer-web-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/loadBalancer-web.parameters.json
[loadBalancer-biz-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/loadBalancer-biz.parameters.json
[loadBalancer-data-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/loadBalancer-data.parameters.json
[virtualMachines-mgmt-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/virtualMachines-mgmt.parameters.json
[loadBalancer-adfsproxy-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/loadBalancer-adfsproxy.parameters.json
[adfsproxy-farm-first-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/adfsproxy-farm-first.parameters.json
[adfsproxy-farm-rest-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/adfsproxy-farm-rest.parameters.json
[0]: ./media/guidance-iaas-ra-secure-vnet-adfs/figure1.png "Zabezpieczanie hybrydowych architektury sieci z usługą Active Directory"
