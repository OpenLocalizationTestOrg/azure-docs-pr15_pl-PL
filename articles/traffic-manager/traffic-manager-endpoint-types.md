<properties
    pageTitle="Typy punktu końcowego Menedżer ruchu | Microsoft Azure"
    description="W tym artykule opisano różne rodzaje punktów końcowych, które mogą być używane z menedżerem ruch Azure"
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="traffic-manager-endpoints"></a>Menedżer ruchu punkty końcowe

Menedżer ruch programu Microsoft Azure pozwala kontrolować sposób ruch sieciowy jest rozdzielana wdrażaniem aplikacji działa w różnych centrach danych. Konfigurowanie każdego wdrażanie aplikacji jako punkt końcowy w Menedżerze ruch. Gdy Menedżer ruchu odbiera żądania DNS, wybiera punktu końcowego dostępne do zwrócenia w odpowiedzi DNS. Menedżer ruchu Określa wybór na bieżący stan punktu końcowego i metody routingu ruchu. Aby uzyskać więcej informacji zobacz [Jak działa Menedżer ruchu](traffic-manager-how-traffic-manager-works.md).

Istnieją trzy typy punktu końcowego obsługiwane przez Menedżera ruchu:

- **Azure punkty końcowe** są używane do usługi obsługiwane platformy Azure.
- **Zewnętrzne punkty końcowe** są używane usługi hostowanej poza Azure, obsługiwanych lokalnie lub u innego dostawcy hostingu.
- **Zagnieżdżone punkty końcowe** są używane do łączenia profile Menedżera ruch, aby utworzyć bardziej elastyczne routingu ruchu programów do obsługi potrzeb większych i bardziej złożonych wdrożeń.

Nie ma żadnych ograniczeń na jak punkty końcowe różnych typów są łączone w jednym profilu Menedżer ruchu. Każdego profilu może zawierać dowolną kombinację typów punktu końcowego.

W poniższych sekcjach opisano każdy typ punktu końcowego szczegółowo większe.

## <a name="azure-endpoints"></a>Azure punkty końcowe

Azure punkty końcowe są używane usługi oparte na platformie Azure w Menedżerze ruch. Obsługiwane są następujące typy Azure zasobu:

- "Klasycznego" IaaS maszyny wirtualne i PaaS usług w chmurze.
- Aplikacje sieci Web
- Zasoby PublicIPAddress (który może być połączony z maszyny wirtualne bezpośrednio lub za pośrednictwem usługi równoważenia obciążenia Azure). PublicIpAddress musi mieć przypisane do użycia w profilu Menedżer ruchu nazwę DNS.

PublicIPAddress zasoby są zasobami Azure Menedżera zasobów. Nie istnieją w modelu Klasyczny wdrożenia. Dlatego są tylko obsługiwane w ruchu kierownika Menedżera zasobów Azure środowiska. Inne typy punktu końcowego są obsługiwane przez zarówno Menedżer zasobów i model klasyczny wdrożenia.

Gdy używasz Azure punkty końcowe, Menedżer ruchu wykrywa maszyny IaaS "Klasyczny", usługa w chmurze lub aplikacji sieci Web jest zatrzymana i uruchomiona. Ten status jest widoczny w stan punktu końcowego. Aby uzyskać szczegółowe informacje, zobacz [Menedżer ruchu monitorowania punktu końcowego](traffic-manager-monitoring.md#endpoint-and-profile-status) . Po zatrzymaniu podstawową usługą Menedżer ruchu wykonuje sprawdzanie kondycji punktu końcowego lub skierowanie ruchu do punktu końcowego. Nie zdarzenia rozliczeń Menedżer ruchu występują przestał wystąpienia. Po uruchomieniu usługi życiorysy rozliczeń i punkt końcowy jest uprawniony do odbierać danych. Wykrywanie nie dotyczą PublicIpAddress punktów końcowych.

## <a name="external-endpoints"></a>Zewnętrznych punktów końcowych

Zewnętrzne punkty końcowe są używane dla usług poza Azure. Na przykład usługa obsługiwana lokalnego lub z innego dostawcy. Zewnętrznych punktów końcowych można używać oddzielnie lub połączone z Azure punkty końcowe w tym samym profilu Menedżer ruchu. Łączenie Azure punkty końcowe z zewnętrznych punktów końcowych udostępnia różnych scenariuszach:

- W obu modelu aktywny aktywny i aktywne pasywne pracy awaryjnej umożliwia uzyskanie zwiększenia nadmiarowości istniejącego lokalnej aplikacji Azure.
- Aby zmniejszyć opóźnienie aplikacji dla użytkowników na całym świecie, rozszerzyć istniejącą aplikację lokalnego na dodatkowe lokalizacje geograficzne w Azure. Aby uzyskać więcej informacji, zobacz [Menedżer ruchu "Wydajności" routingu ruchu](traffic-manager-routing-methods.md#performance-traffic-routing-method).
- Użyj Azure o podanie dodatkowych możliwości istniejącego lokalnego aplikacji, przez cały czas lub jako rozwiązanie "serii w chmurze" spotkania kolekcji na żądanie.

W niektórych przypadkach warto dokumentacja dotycząca usług Azure za pomocą zewnętrznych punktów końcowych (przykłady, zobacz [często zadawane pytania dotyczące](#faq)). W tym przypadku sprawdzanie kondycji są wystawiona kursie Azure punkty końcowe nie stopa zewnętrznych punktów końcowych. Jednak w przeciwieństwie do punktów końcowych Azure, zatrzymać lub usuniesz usługę źródłowych sprawdzanie kondycji rozliczeń będzie nadal występował, dopóki nie zostanie wyłączona lub usunąć punkt końcowy w Menedżerze ruch.

## <a name="nested-endpoints"></a>Zagnieżdżone punkty końcowe

Zagnieżdżone punkty końcowe połączyć profili Menedżer ruchu do tworzenia elastycznych schematy routingu ruchu i obsługiwać potrzeby większych, złożonych wdrożeń. Z punktów końcowych zagnieżdżone profilu "podrzędny" zostanie dodane jako punkt końcowy do profilu "parent". Profile dziecka i nadrzędnej mogą zawierać inne punkty końcowe dowolnego typu, w tym innych zagnieżdżonych profilów. Aby uzyskać więcej informacji zobacz [zagnieżdżonych profile Menedżer ruchu](traffic-manager-nested-profiles.md).

## <a name="web-apps-as-endpoints"></a>Aplikacje jako punkty końcowe w sieci Web

Niektóre dodatkowe kwestie podczas konfigurowania aplikacji sieci Web jako punkty końcowe w Menedżerze ruchu:

1. Tylko aplikacje sieci Web w wersji "Standard" lub powyżej przysługuje korzystanie z Menedżera ruch. Próby dodawania aplikacji sieci Web z dolnym SKU się niepowodzeniem. Obniżanie wersji SKU z istniejącej aplikacji sieci Web wyników w Menedżerze ruch nie jest już wysyłania ruchu do tej aplikacji sieci Web.

2. Punkt końcowy odbiera żądania HTTP, będą używały zmienionego nagłówek "hosta" w wezwaniu na ustalenie, której aplikacji sieci Web należy obsługi żądania. Nagłówek hosta zawiera nazwę DNS umożliwia inicjować żądanie, na przykład "contosoapp.azurewebsites.net". Aby użyć innej nazwy DNS z aplikacji sieci Web, nazwy DNS musi być zarejestrowany jako niestandardowej nazwy domeny dla aplikacji. Podczas dodawania punkt końcowy aplikacji sieci Web jako punkt końcowy Azure, nazwa DNS profilu Menedżer ruchu jest automatycznie rejestrowany dla aplikacji. Tej rejestracji są usuwane automatycznie, gdy punkt końcowy zostanie usunięty.

3. Każdy profil Menedżer ruchu może zawierać najwyżej jeden aplikacji sieci Web z punktów końcowych z każdego regionu Azure. Aby obejść to ograniczenie, można skonfigurować aplikację sieci Web jako zewnętrzny punkt końcowy. Aby uzyskać więcej informacji zobacz [często zadawane pytania](#faq).

## <a name="enabling-and-disabling-endpoints"></a>Włączanie i wyłączanie punkty końcowe

Wyłączanie punktu końcowego w Menedżerze ruch może być przydatne tymczasowe usuwanie ruchu z punktu końcowego, który znajduje się w trybie konserwacji lub rozmieszczany. Po ponownym uruchomieniu punkt końcowy można ponownie włączyć.

Punkty końcowe może być włączony i wyłączyć za pomocą portalu Menedżer ruchu interfejsu wiersza polecenia programu PowerShell i interfejsu API usługi REST, które są obsługiwane Menedżera zasobów i modelu Klasyczny wdrożenia.

>[AZURE.NOTE] Wyłączenie punktu końcowego Azure ma nic, aby zrobić z stanu rozmieszczania platformy Azure. Usługa Azure (takich jak maszyn wirtualnych lub aplikacji sieci Web będzie nadal uruchomiony i może odbierać ruch nawet wtedy, gdy wyłączona w Menedżerze ruch. Ruch można rozwiązać bezpośrednio do wystąpienia usługi, a nie za pomocą nazwy DNS profilu Menedżer ruchu. Aby uzyskać więcej informacji zobacz [jak działa Menedżer ruchu](traffic-manager-how-traffic-manager-works.md).

Bieżące uprawnienia do każdego punktu końcowego, aby odbierać danych zależy od następujących czynników:

- Stan profilu (włączone/wyłączone)
- Stan punktu końcowego (włączone/wyłączone)
- Wyniki kondycji sprawdza dla tego punktu końcowego

Aby uzyskać szczegółowe informacje zobacz [Menedżer ruchu monitorowania punktu końcowego](traffic-manager-monitoring.md#endpoint-and-profile-status).

>[AZURE.NOTE] Ponieważ Menedżer ruchu działa na poziomie DNS, nie może mieć wpływ na istniejące połączenia do dowolnego punktu końcowego. Gdy punkt końcowy jest niedostępna, Menedżer ruchu kieruje nowych połączeń do innej dostępne punktu końcowego. Jednak hosta za punktem końcowym wyłączone lub nieprawidłowe mogą w dalszym ciągu odbierać danych za pośrednictwem istniejące połączenia, dopóki nie zostaną zakończone tych sesji. Aplikacje należy ograniczyć czas trwania sesji, aby umożliwić ruchu Opróżnij z istniejące połączenia.

Jeśli wszystkie punkty końcowe w profilu, które zostały wyłączone lub profilu, sam jest wyłączona, Menedżer ruchu wysyła odpowiedź "NXDOMAIN" do nowej kwerendy DNS.

## <a name="faq"></a>FAQ

### <a name="can-i-use-traffic-manager-with-endpoints-from-multiple-subscriptions"></a>Do czego służy Menedżer ruchu z punktów końcowych z wiele subskrypcji?

Za pomocą punktów końcowych z wiele subskrypcji nie jest możliwe z aplikacjami Web Azure. Azure aplikacji sieci Web wymaga dowolna nazwa domeny niestandardowej używana w aplikacjach sieci Web jest używany tylko w jednym subskrypcji. Nie jest możliwe korzystanie z aplikacji sieci Web z wiele subskrypcji z tą samą nazwą domeny.

W przypadku innych typów punkt końcowy prawdopodobnie Menedżer ruchu za pomocą punktów końcowych z więcej niż jedną subskrypcję. Jak skonfigurować Menedżer ruchu zależy od tego, czy przy użyciu modelu Klasyczny wdrażania i obsługi Menedżera zasobów.

- W Menedżerze zasobów punkty końcowe w przypadku subskrypcji można dodawać do Menedżera ruch pod warunkiem, że osoba skonfigurowanie profilu Menedżer ruchu ma dostęp do odczytu do punktu końcowego. Tych uprawnień można udzielić za pomocą [Menedżera zasobów Azure kontrola dostępu oparta na rolach (RBAC)](../active-directory/role-based-access-control-configure.md).
- W interfejsie modelu Klasyczny wdrożenia Menedżer ruchu wymaga, aby usług w chmurze lub aplikacji sieci Web skonfigurowanego jako punkty końcowe Azure znajdowały się w tej samej subskrypcji, jak profil Menedżera ruch. Punkty końcowe usługi cloud w innych subskrypcji można dodawać do Menedżera ruch jako "zewnętrzne" punkty końcowe. Te zewnętrzne punkty końcowe są zafakturowane jako Azure punkty końcowe, a nie stopa zewnętrznych.

### <a name="can-i-use-traffic-manager-with-cloud-service-staging-slots"></a>Do czego służy Menedżer ruchu z gniazdami "Tymczasowej" usługi w chmurze?

Wartość Tak. Usługa w chmurze tymczasowej gniazda można skonfigurować w Menedżerze ruch jako zewnętrznych punktów końcowych. Sprawdzanie kondycji nadal są rozliczana według stawki Azure punktów końcowych. Ponieważ typ zewnętrzny punkt końcowy jest używany, zmiany w usłudze podstawowej nie są odczytywane automatycznie. Z zewnętrznych punktów końcowych Menedżer ruchu nie wykrywa po zatrzymaniu usługi w chmurze lub usunięte. W związku z tym Menedżer ruchu nadal rozliczeniami dla sprawdzanie kondycji, aż punkt końcowy zostały wyłączone lub usunięte.

### <a name="does-traffic-manager-support-ipv6-endpoints"></a>Menedżer ruchu obsługuje protokół IPv6 punkty końcowe?

Menedżer ruchu nie umożliwia obecnie IPv6 addressible serwerów nazw. Jednak Menedżer ruchu nadal mogą być używane przez klientów IPv6 łączących się punkty końcowe IPv6. Klient nie powoduje żądania DNS bezpośrednio do Menedżera ruch. Zamiast tego klient używa usługi DNS cykliczne. Tylko IPv6 klient wysyła żądania usługi DNS cykliczne za pomocą protokołu IPv6. Następnie usługę cykliczne powinny mieć możliwość kontaktowania się z serwerami nazwę Menedżer ruchu przy użyciu protokołu IPv4.

Menedżer ruchu odpowiada nazwie DNS punkt końcowy. Do obsługi protokołu IPv6 punktu końcowego, musi istnieć rekord DNS AAAA wskazująca nazwę DNS punkt końcowy adres IPv6. Sprawdzanie kondycji Menedżer ruchu obsługuje tylko adresy IP protokołu IPv4. Usługa musi uwidaczniane punktu końcowego IPv4 na tej samej nazwy DNS.

### <a name="can-i-use-traffic-manager-with-more-than-one-web-app-in-the-same-region"></a>Czy mogę korzystać z więcej niż jednej aplikacji sieci Web w tym samym regionie Menedżer ruchu?

Zazwyczaj Menedżer ruchu służy do kierowania ruchu do aplikacji wdrożonych w różnych regionach. Jednak może również służyć, której aplikacja ma więcej niż jeden wdrożenia w tym samym regionie. Punkty końcowe Azure Menedżer ruchu nie zezwalają na więcej niż jeden z punktów końcowych aplikacji sieci Web z tego samego regionu Azure mają zostać dodane do tego samego profilu Menedżer ruchu.

Poniższe czynności stanowią obejściu tego ograniczenia:

1. Sprawdź, czy usługi punkty końcowe są w innej witryny sieci web aplikacji "skali". Nazwa domeny musi zamapować na jednej witryny w jednostce danej skali. Dlatego obiema aplikacjami sieci Web w tej samej jednostki skali nie mogą współużytkować profilu Menedżer ruchu.
2. Dodaj nazwę domeny vanity jako niestandardowe hostname do każdej aplikacji sieci Web. Każdej aplikacji sieci Web musi być w jednostce inną skalę. Wszystkie aplikacje sieci Web musi należeć do tej samej subskrypcji.
3. Dodawanie jednego (i tylko jednego) punkt końcowy aplikacji sieci Web do swojego profilu Menedżer ruchu, jako punkt końcowy Azure.
4. Dodaj każdego dodatkowe punktu końcowego aplikacji sieci Web do swojego profilu Menedżer ruchu jako zewnętrzny punkt końcowy. Zewnętrznych punktów końcowych można dodawać tylko przy użyciu modelu wdrożenia Menedżera zasobów.
5. Utwórz rekord DNS CNAME w domenie vanity wskazującego na nazwie DNS profilu Menedżer ruchu (<>.... trafficmanager.net).
6. Uzyskiwać dostęp do witryny za pomocą vanity nazwy domeny, nie Menedżer ruchu profilu nazwę DNS.

## <a name="next-steps"></a>Następne kroki

- Dowiedz się, [jak działa Menedżer ruchu](traffic-manager-how-traffic-manager-works.md).
- Informacje na temat Menedżera ruch [monitorowania punktu końcowego i automatyczne przejście](traffic-manager-monitoring.md).
- Informacje na temat Menedżera ruch [metody routingu ruchu](traffic-manager-routing-methods.md).
