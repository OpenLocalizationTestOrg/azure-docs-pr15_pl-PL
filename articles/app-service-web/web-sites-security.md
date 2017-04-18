<properties
    pageTitle="Zabezpieczanie aplikacji w usłudze Azure aplikacji"
    description="Dowiedz się, jak bezpiecznego aplikacji sieci web, wewnętrznej bazy danych aplikacji dla urządzeń przenośnych lub w usłudze Azure aplikacji interfejsu API aplikacji."
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="01/12/2016"
    ms.author="cephalin"/>


#<a name="secure-an-app-in-azure-app-service"></a>Zabezpieczanie aplikacji w usłudze Azure aplikacji

Ten artykuł ułatwia rozpoczęcie pracy o zabezpieczaniu aplikacji sieci web, wewnętrznej bazy danych aplikacji dla urządzeń przenośnych lub w usłudze Azure aplikacji interfejsu API aplikacji. 

Zabezpieczenia w usłudze Azure aplikacji występują dwa poziomy: 

- **Zabezpieczenia infrastruktury i platformy** — ufasz Azure usług, należy przeprowadzić faktycznie rzeczy bezpiecznie w chmurze.
- **Zabezpieczenia aplikacji** — należy więc projektować samej aplikacji bezpieczne. Ta opcja uwzględnia jak integracja z usługą Azure Active Directory, jak zarządzać certyfikaty i jak należy upewnić się, że możesz bezpiecznie porozmawiać z różnych usług. 

#### <a name="infrastructure-and-platform-security"></a>Zabezpieczenia infrastruktury i platformy
Ponieważ usługa aplikacji przechowuje maszyny wirtualne Azure, miejsca do magazynowania, połączeń sieciowych, struktury sieci web, zarządzania i funkcji integracji i wiele innych jest aktywnie zabezpieczone i kula przechodzi intensywnie zgodności i sprawdza w sposób ciągły, aby upewnić się, że:

- Są one odizolowane z obu Internet i z innymi klientami Azure zasoby są Twoje aplikacje aplikacji usługi.
- Przekazywanie hasła (przykład ciągi połączenia) między aplikacji aplikacji usługi i inne zasoby Azure (np. bazy danych SQL) w grupie zasobów w obrębie Azure i nie krzyżowe wszelkie ograniczenia sieciowe. Hasła są zawsze szyfrowane.
- Całą komunikację między aplikacji usługi aplikacji i zasobów zewnętrznych, takich jak zarządzania programu PowerShell, interfejs wiersza polecenia, SDK Azure, API pozostałych i połączeń hybrydowych prawidłowo są szyfrowane.
- zagrożenia 24-godzinnego zarządzania chroni zasoby aplikacji usługi przed złośliwym oprogramowaniem, distributed odmowa usługi (DDoS), w dolnym pośrednik (MITM) i innymi zagrożeniami. 

Aby uzyskać więcej informacji dotyczących zabezpieczeń infrastruktury i platformy w Azure zobacz [Centrum zaufania Azure](/support/trust-center/security/).

#### <a name="application-security"></a>Zabezpieczenia aplikacji

W trakcie odpowiedzialny za zabezpieczania infrastruktury i aplikacja działa na wersję platformy Azure odpowiada usługi bezpiecznego samej aplikacji. Innymi słowy potrzebne do opracowywania, wdrażania i zarządzania kod aplikacji i zawartości w bezpieczny sposób. Bez tego danego kodu aplikacji lub zawartości można nadal występować zagrożenia takich jak:

- Uruchomienie SQL
- Przejęcie kontroli sesji
- Wykonywanie skryptów witryny krzyżowe
- MITM poziomie aplikacji
- DDoS poziomie aplikacji

Pełne omówienie zagadnienia dotyczące zabezpieczeń dla aplikacji sieci web wykracza poza zakres tego dokumentu. Jako punktu startowego dalsze wskazówki dotyczące zabezpieczania aplikacji zobacz [Otwieranie sieci Web aplikacji zabezpieczeń programu Project (OWASP)](https://www.owasp.org/index.php/Main_Page), szczególnie [10 głównych projektu.](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project), która list wady bieżącego zabezpieczeń aplikacji górny 10 web krytyczne, określone przez członków OWASP.

## <a name="perform-penetration-testing-on-your-app"></a>Wykonywanie penetracyjne testowanie aplikacji

Aby wykonać skanowanie w aplikacji luka jednym kliknięciem jeden z najprostszych sposobów jeszcze wprowadzenie do badania luk w aplikacji usługi aplikacji jest [Integracja z zabezpieczeniami Tinfoil](/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/) . Można wyświetlić wyniki testu w raporcie łatwych do zrozumienia i Dowiedz się, jak rozwiązać problem każdego słabego z instrukcji krok po kroku.

Jeśli chcesz wykonać testy penetracyjne lub chcesz użyć innego dostawcy lub skanera pakietu, musi postępuj zgodnie z [penetracyjne Azure testowania proces zatwierdzania](https://security-forms.azure.com/penetration-testing/terms) i uzyskać pobieraniu przeprowadzić testy penetracyjne odpowiednie.

##<a name="https"></a>Bezpieczna komunikacja z klientami

Jeśli korzystasz z ** \*. azurewebsites.net** nazwy domeny utworzoną dla aplikacji usługi aplikacji, od razu może używać HTTPS, mowa certyfikat SSL dla wszystkich ** \*. azurewebsites.net** nazw domen. Jeśli witryny korzysta z [niestandardowej nazwy domeny](web-sites-custom-domain-name.md), możesz przekazać certyfikat SSL, aby [włączyć HTTPS](web-sites-configure-ssl-certificate.md) dla domeny niestandardowej.

Włączanie [HTTPS](https://en.wikipedia.org/wiki/HTTPS) ułatwia ochronę przed atakami MITM na komunikacji między aplikacji i jej użytkowników.

## <a name="secure-data-tier"></a>Zabezpieczanie warstwy danych

Aplikacji usługi wysoce zintegrowany z bazy danych SQL, tak, aby wszystkie parametry połączenia są szyfrowane w tablicy i odszyfrować tylko za na maszyn wirtualnych uruchamianej aplikacji na *i* tylko wtedy, gdy aplikacja jest uruchomiony. Ponadto bazy danych SQL Azure zawiera wiele funkcji zabezpieczeń ułatwiające bezpiecznego dane aplikacji przed zagrożeniami cyber, w tym [szyfrowania w pozostałych](https://msdn.microsoft.com/library/dn948096.aspx), [Zawsze szyfrowane](https://msdn.microsoft.com/library/mt163865.aspx), [Dynamiczne maskowanie danych](../sql-database/sql-database-dynamic-data-masking-get-started.md)i [Wykrywania zagrożenie](../sql-database/sql-database-threat-detection-get-started.md). Jeśli masz dane poufne lub wymagania dotyczące zgodności, zobacz [Zabezpieczanie bazy danych SQL](../sql-database/sql-database-security.md) Aby uzyskać więcej informacji na temat zabezpieczania danych.

Jeśli korzystasz z dostawcą bazy danych innych firm, takich jak ClearDB, skontaktuj się z dokumentacją dostawcy bezpośrednio na najważniejsze wskazówki dotyczące zabezpieczeń.  

##<a name="develop"></a>Zabezpieczanie projektowania i wdrażania

### <a name="publishing-profiles-and-publish-settings"></a>Publikowanie profilów i ustawienia publikowania

Podczas tworzenia aplikacji wykonywania zadań zarządzania lub Automatyzowanie zadań za pomocą narzędzi, takich jak **Visual Studio**, **Macierzy w sieci Web**, **Azure programu PowerShell** lub **interfejsu wiersza polecenia Azure (polecenie Azure)**, można użyć pliku *Ustawienia publikowania* lub *Publikowanie profilu*. Oba typy plików uwierzytelniania Azure i powinny być zabezpieczone do zapobieganie nieupoważnionemu dostępowi.

* Plik **ustawień publikowania** zawiera

    * Identyfikator subskrypcji Azure

    * Certyfikat zarządzania, która umożliwia wykonywanie zadań związanych z zarządzaniem dla Twojej subskrypcji *bez konieczności o podanie hasła lub nazwę konta*.

* Plik **profilu publikowania** zawiera

    * Informacje dotyczące publikowania aplikacji

Jeśli narzędzie jest używany plik ustawień publikowania lub plik profilu publikowania, zaimportować plik zawierający ustawienia publikowania lub profilu do narzędzia, a następnie **Usuń** go. Jeżeli należy pozostawić plik, aby udostępnić innym osobom pracować nad projektem, na przykład, należy ją zapisać w bezpiecznej lokalizacji, takich jak *zaszyfrowanych* katalogów z ograniczonymi uprawnieniami.

Ponadto należy upewnij się, że są zabezpieczone zaimportowanego poświadczeń. Na przykład **Azure programu PowerShell** i **interfejs wiersza polecenia Azure (polecenie Azure)** przechowywać importowanymi informacjami w **katalogu macierzystego** (*~* na */users/yourusername* w systemach Windows i Linux lub OS X systemów.) Dodatkowe zabezpieczenie możesz **Szyfrowanie** następujących lokalizacji za pomocą narzędzi szyfrowania, które są dostępne dla używanego systemu operacyjnego.

### <a name="configuration-settings-and-connection-strings"></a>Ustawienia konfiguracji i ciągów połączenia
Typowe rozwiązaniem jest wykonanie przechowywać parametry połączenia, poświadczeń uwierzytelniania i innych informacji poufnych w plikach konfiguracyjnych. Niestety te pliki mogą być dostępne w witrynie sieci Web lub w repozytorium publicznego ujawnienia informacji. Wyszukiwanie proste na [GitHub](https://github.com), na przykład, aby odsłonić wiele plików konfiguracji z udostępnionej hasła repozytoria publicznej.

Najlepszym rozwiązaniem jest do zachowania tych informacji z pliku konfiguracji aplikacji sieci. Aplikacji usługi pozwala na przechowywanie informacji o konfiguracji jako część środowiska wykonawczego jako **ustawień aplikacji** i **ciągów połączenia**. Wartości są dostępne w aplikacji w czasie wykonywania za pomocą *zmiennych środowiska* dla większości języków programowania. Aplikacji .NET wartości te są dodane do konfiguracji .NET w czasie rzeczywistym. Z wyjątkiem następujących sytuacji te ustawienia konfiguracji będą nadal zaszyfrowane, o ile nie można wyświetlić lub skonfigurować je za pomocą [Azure Portal](https://portal.azure.com) lub narzędzi, takich jak programu PowerShell lub interfejsu wiersza polecenia Azure. 

Przechowywanie informacji o konfiguracji w aplikacji usługi umożliwia administratorowi aplikacji zablokować poufnych informacji dla aplikacji produkcji. Deweloperzy mogą używać oddzielny zestaw ustawienia konfiguracji dla opracowywania aplikacji i ustawień może być automatycznie zastąpione skonfigurować w aplikacji usługi. Nawet deweloperów mieć na uwadze hasła skonfigurowane dla aplikacji produkcji. Aby uzyskać więcej informacji na temat konfigurowania ustawień aplikacji i ciągów połączenia w aplikacji usługi zobacz [Konfigurowanie aplikacji sieci web](web-sites-configure.md).

### <a name="ftps"></a>FTPS

Azure aplikacji usługi zapewnia bezpieczny dostęp FTP w systemie plików dla aplikacji za pomocą **FTPS**. Pozwala na bezpieczny dostęp do kodu aplikacji w aplikacji sieci web, a także diagnostyki dzienników. Zalecane jest zawsze używaj FTPS zamiast FTP. 

Łącze FTPS dla aplikacji znajdują się następujące czynności:

1. Otwórz [Azure Portal](https://portal.azure.com).
2. Wybierz pozycję **Przeglądaj wszystkie**.
3. Karta **Przeglądanie** zaznacz **Aplikacji usług**.
4. Karta **Aplikacji usług** zaznacz odpowiednią aplikację.
5. Karta tej aplikacji zaznacz **wszystkie ustawienia**.
6. Karta **Ustawienia** zaznacz **Właściwości**.
7. Łącza FTP i FTPS są zawarte w karta **Ustawienia** . 

Aby uzyskać więcej informacji o FTPS zobacz [File Transfer Protocol](http://en.wikipedia.org/wiki/File_Transfer_Protocol).

## <a name="next-steps"></a>Następne kroki

Uzyskać więcej informacji dotyczących zabezpieczeń platformy Azure, informacji na raportowanie **zdarzeniem bezpieczeństwa lub abuse**lub poinformowanie firmy Microsoft, że będziesz wykonywać **Testowanie przenikania** witryny, zobacz sekcję zabezpieczeń w [Centrum zaufania programu Microsoft Azure](https://azure.microsoft.com/support/trust-center/security/).

Aby uzyskać więcej informacji na plikach **web.config** lub **applicationhost.config** w aplikacji usługi aplikacji zobacz [Opcje konfiguracji odblokowywane w aplikacjach sieci web Azure aplikacji usługi](https://azure.microsoft.com/blog/2014/01/28/more-to-explore-configuration-options-unlocked-in-windows-azure-web-sites/).

Aby uzyskać informacje na informacji rejestrowania dla aplikacji usługi aplikacje, które mogą być przydatne wykrywanie atakami, zobacz [włączyć rejestrowanie diagnostyczne](web-sites-enable-diagnostic-log.md).

>[AZURE.NOTE] Jeśli chcesz rozpocząć pracę z Azure aplikacji usługi przed utworzeniem konta dla konta Azure, przejdź do [Spróbuj aplikacji usługi](http://go.microsoft.com/fwlink/?LinkId=523751), którym natychmiast można utworzyć aplikację krótkotrwałe starter w aplikacji usługi. Nie kart kredytowych wymagane; nie zobowiązania.

## <a name="whats-changed"></a>Informacje o zmianach

* Przewodnika do zmiany z witryn sieci Web do usługi aplikacji Zobacz: [Usługa Azure aplikacji i jego wpływ na istniejące usługi Azure](http://go.microsoft.com/fwlink/?LinkId=529714)
