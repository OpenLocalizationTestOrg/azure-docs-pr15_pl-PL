<properties
    pageTitle="Azure przewodnik zabezpieczeń magazynowania | Microsoft Azure"
    description="Szczegóły wiele metod zabezpieczania magazyn Azure, w tym między innymi RBAC, szyfrowanie usługi przechowywania szyfrowania po stronie klienta, SMB 3.0 i szyfrowania dysku Azure."
    services="storage"
    documentationCenter=".net"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="robinsh"/>

#<a name="azure-storage-security-guide"></a>Przewodnik po zabezpieczeniach Azure miejsca do magazynowania

##<a name="overview"></a>Omówienie

Magazyn Azure oferuje pełnego zestawu funkcji zabezpieczeń, umożliwiające programistom na tworzenie bezpiecznych aplikacji. Samo konto miejsca do magazynowania mogą być chronione za pomocą kontrola dostępu oparta na rolach i usługi Azure Active Directory. Dane mogą być chronione podczas przesyłania między aplikacją i Azure za pomocą [Szyfrowania po stronie klienta](storage-client-side-encryption.md), HTTPS lub SMB 3.0. Dane można ustawić do automatycznie szyfrowane zapisywane magazyn Azure za pomocą [Przestrzeni dyskowej usługi szyfrowania SSE ()](storage-service-encryption.md). Można ustawić dysków systemu operacyjnego i dane używane przez maszyn wirtualnych zaszyfrowana przy użyciu [Szyfrowania dysku Azure](../security/azure-security-disk-encryption.md). Delegowanej dostęp do obiektów danych w magazynie Azure można udzielić używania [Podpisów dostępu udostępnione](storage-dotnet-shared-access-signature-part-1.md).

W tym artykule zawiera omówienie każdego z tych funkcji zabezpieczeń, które mogą być używane z magazyn Azure. Łącza są dostarczane do artykułów, które będą szczegółowy opis poszczególnych funkcji, można w prosty sposób dalszego dochodzenia w sprawie poszczególnych tematów.

Poniżej wymieniono tematy, które mają być uwzględnione w tym artykule:

-   [Zarządzanie płaszczyzny zabezpieczeń](#management-plane-security) — zabezpieczanie konta miejsca do magazynowania

    Płaszczyzny zarządzania składa się z zasobów służące do zarządzania kontem przestrzeni dyskowej. W tej sekcji będzie porozmawiamy o model wdrożenia Menedżera zasobów Azure i jak używać funkcji Kontrola dostępu oparta na rolach (RBAC) do kontrolowania dostępu do konta miejsca do magazynowania. Firma Microsoft będzie także porozmawiać o zarządzaniu kluczy konta miejsca do magazynowania i jak je odtworzyć.

-   [Dane płaszczyzny zabezpieczeń](#data-plane-security) — bezpieczeństwo dostępu do danych

    W tej sekcji opisano zezwolenia na dostęp do obiektów rzeczywistych danych na swoim koncie miejsca do magazynowania, takich jak obiektów blob, pliki kolejek i tabele, udostępniane podpisom dostępu i przechowywane zasady dostępu. Omówimy następujące zagadnienia zarówno skojarzeń zabezpieczeń poziomu usług i skojarzenia konta poziom zabezpieczeń. Widzimy będzie również sposobu ograniczyć dostęp do określonego adresu IP (lub zakres adresów IP), jak ograniczyć protokół używany do HTTPS i odwoływanie podpisu dostępu udostępnione nie czekając na wygaśnie.

-   [Szyfrowanie w drodze](#encryption-in-transit)

    W tej sekcji omówiono sposób zabezpieczania danych podczas przenoszenia do lub poza magazyn Azure. Będzie porozmawiamy o zalecane używanie HTTPS i szyfrowania używany przez SMB 3.0 dla Azure udziałach plików. Firma Microsoft będzie także zapoznaj się szyfrowanie po stronie klienta, co umożliwia szyfrowanie danych, zanim zostanie przekazany do miejsca do magazynowania w aplikacji klienckiej oraz odszyfrowywanie danych po przeniesieniu Brak miejsca.

-   [Szyfrowanie w pozostałych](#encryption-at-rest)

    Będzie porozmawiamy o przestrzeni dyskowej usługi szyfrowania SSE () oraz udostępnianie go dla konta miejsca do magazynowania, uzyskując z obiektami blob blok blob strony i dołączanie obiektów blob automatycznie są szyfrowane zapisywane magazyn Azure. Możemy również będzie wyglądać w sposób użycia szyfrowania dysku Azure i Poznaj podstawowe różnice i przypadkach szyfrowania dysku i SSE i szyfrowania po stronie klienta. Firma Microsoft krótko będzie wyglądać na FIPS zgodności dla komputerów Rządu Stanów Zjednoczonych.

-   Przeprowadź inspekcję dostępu do miejsca Azure za pomocą [Analizy miejsca do magazynowania](#storage-analytics)

    W tej sekcji omówiono sposób informacji można znaleźć w dziennikach analizy miejsca do magazynowania na żądanie. Firma Microsoft będzie zapoznaj się analizy rzeczywistą przechowywania danych dziennika i zobacz, jak wykrycia żądanie kluczem konta miejsca do magazynowania, podpisem udostępnionych programu Access, czy anonimowe i czy zakończyła się pomyślnie, czy nie.

-   [Włączanie klientów zgodny z przeglądarką za pomocą CORS](#Cross-Origin-Resource-Sharing-CORS)

    Ta sekcja zawiera informacje o jak zezwolić zasobów między origin udostępniania (CORS). Będzie porozmawiamy o dostęp z innych domen i sposobu obsługi go z funkcjami CORS wbudowane magazyn Azure.

##<a name="management-plane-security"></a>Zarządzanie płaszczyzny zabezpieczeń

Płaszczyzny zarządzania składa się z operacji, które mają wpływ na samo konto miejsca do magazynowania. Na przykład można utworzyć lub usunąć konto miejsca do magazynowania, uzyskać listę kont miejsca do magazynowania w subskrypcji, pobrać kluczy konta miejsca do magazynowania lub ponownie wygenerować klucze konta miejsca do magazynowania.

Podczas tworzenia nowego konta miejsca do magazynowania, możesz wybrać model wdrożenia klasyczny lub Menedżera zasobów. Model klasyczny tworzenia zasobów w Azure umożliwia tylko all-or-nothing dostęp do subskrypcji, a następnie kolejno konta miejsca do magazynowania.

Ten przewodnik omówiono modelu Menedżera zasobów, której zalecany sposób tworzenia kont miejsca do magazynowania. Przy użyciu konta magazynu Menedżera zasobów, zamiast dające dostęp do całej subskrypcji możesz sterować dostępem na poziomie bardziej ograniczone do płaszczyzny zarządzania przy użyciu kontrola dostępu oparta na rolach (RBAC).

###<a name="how-to-secure-your-storage-account-with-role-based-access-control-rbac"></a>Jak zabezpieczyć konto miejsca do magazynowania z kontrola dostępu oparta na rolach (RBAC)

Załóżmy porozmawiać na temat RBAC jest i jak go używać. Każdy Azure Subskrypcja obejmuje usługi Azure Active Directory. Użytkownicy, grupy i aplikacji z tego katalogu można udzielić dostępu do zarządzania zasobami Azure subskrypcji korzystające z modelu wdrożenia Menedżera zasobów. To jest określane jako kontrola dostępu oparta na rolach (RBAC). Aby zarządzać dostęp, służy [Azure portal](https://portal.azure.com/), [Azure polecenie narzędzia](../xplat-cli-install.md), [programu PowerShell](../powershell-install-configure.md)lub [Interfejsów API pozostałych dostawcy Azure miejsca do magazynowania zasobów](https://msdn.microsoft.com/library/azure/mt163683.aspx).

Z modelem Menedżera zasobów należy umieścić konta miejsca do magazynowania w grupy i kontrolować dostęp do zasobów do zarządzania linii tego konta określonego miejsca do magazynowania przy użyciu usługi Azure Active Directory. Na przykład można udzielić określonym użytkownikom możliwość dostępu klawiszy konta miejsca do magazynowania, gdy inni użytkownicy mogą wyświetlać informacje o koncie miejsca do magazynowania, ale nie można uzyskać dostęp do kluczy konta miejsca do magazynowania.

####<a name="granting-access"></a>Udzielanie dostępu

Dostęp jest przyznany przypisując odpowiednią rolę RBAC do użytkowników, grupy i aplikacjami, w tym zakresie prawo. Aby udzielić dostępu do całej subskrypcji, możesz przypisywać rolę na poziomie subskrypcji. Aby udzielić dostępu do wszystkich zasobów w grupie zasobów, należy udzielanie uprawnień grupy zasobów. Można także przypisywać role określonych do określonych zasobów, takich jak konta miejsca do magazynowania.

Oto główne punkty, które należy wiedzieć o dostęp do operacji zarządzania konta magazynu platformy Azure za pomocą RBAC:

-   Po przypisaniu dostępu zasadniczo przypisywanie roli do konta, które chcesz mieć dostęp. Możesz sterować dostępem do operacje, używane do zarządzania tego konta miejsca do magazynowania, ale nie obiekty danych w oknie konta. Na przykład może przyznać uprawnienia do pobrania właściwości konta miejsca do magazynowania (na przykład nadmiarowości), a nie do kontenera lub dane znajdujące się w kontenerze wewnątrz magazyn obiektów Blob.

-   Osoby mają uprawnienia dostępu do danych obiektów w oknie konta magazynu możesz udzielić mu uprawnienia do odczytu kluczy konta przestrzeni dyskowej, a użytkownik następnie klawiszy tych dostęp do obiektów blob, kolejki, tabele i pliki.

-   Można przypisywać role do określonego konta użytkownika, grupy użytkowników lub do określonej aplikacji.

-   Każda rola ma listę akcji i nie akcje. Na przykład Rola współautora maszyn wirtualnych ma akcji "listKeys", która umożliwia klawiszy konta miejsca do magazynowania do odczytu. Współautor zawiera "Nie akcje", na przykład aktualizowanie dostępu dla użytkowników w usłudze Active Directory.

-   Role do przechowywania obejmują (ale nie są ograniczone do) następujące czynności:

    -   Właściciel — ta osoba może zarządzać wszystko, łącznie z dostępem.

    -   — Ta osoba może wykonać Współautor nic właściciel oprócz przypisywanie dostępu. Osoby z tej roli umożliwia wyświetlanie i ponownie wygenerować klucze konta przestrzeni dyskowej. Za pomocą klawiszy konta miejsca do magazynowania mogą uzyskać dostęp obiekty danych.

    -   Czytnik — mogą wyświetlać informacje o koncie miejsca do magazynowania, z wyjątkiem hasła. Na przykład w przypadku przypisania roli z uprawnieniami czytnika na koncie miejsca do magazynowania osobie, mogą wyświetlać właściwości konta miejsca do magazynowania, ale nie można ich wprowadź odpowiednie zmiany we właściwościach lub wyświetlić kluczy konta miejsca do magazynowania.

    -   Współautora konta miejsca do magazynowania — ich Zarządzanie kontem miejsca do magazynowania — więcej grup zasobów i zasobów, subskrypcji i tworzenie i zarządzanie nimi wdrożeń grupa zasobów subskrypcji. Można także używać klawiszy konta miejsca do magazynowania, co z kolei oznacza, że mogą uzyskać dostęp do płaszczyzny danych.

    -   Administrator dostępu użytkowników — ta osoba Zarządzanie użytkownikowi dostępu do konta miejsca do magazynowania. Na przykład ta osoba może udzielić czytnika dostępu określonego użytkownika.

    -   Współautor maszyn wirtualnych — ich można zarządzać maszyn wirtualnych, ale nie na koncie miejsca do magazynowania, z którym jest połączony. Ta rola można wyświetlić kluczy konta miejsca do magazynowania, co oznacza, że użytkownik, któremu przypisanie tej roli można zaktualizować płaszczyzny danych.

        Użytkownika w celu utworzenia maszyny wirtualnej, muszą mieć możliwość tworzenia odpowiedniego pliku wirtualnego dysku twardego na koncie miejsca do magazynowania. Aby to zrobić, muszą mieć możliwość pobierania klucz konta miejsca do magazynowania i przekazać je do API tworzenia maszyn wirtualnych. W związku z tym muszą mieć to uprawnienie, można wyświetlić listę klawiszy konta miejsca do magazynowania.

- Możliwość definiowania ról niestandardowych jest funkcję, która umożliwia redagowanie zestawu akcji z listy dostępnych akcji, które mogą być wykonywane na Azure zasobów.

- Użytkownik ma zostać skonfigurowane w usłudze Active Directory platformy Azure, aby można było przypisać rolę do nich.

- Możesz utworzyć raport, który udzielił odwołany jakiego rodzaju programu access, z której i jakie zakres przy użyciu programu PowerShell lub interfejsu wiersza polecenia Azure.

####<a name="resources"></a>Zasoby

-   [Kontrola dostępu oparta na rolach Azure Active Directory](../active-directory/role-based-access-control-configure.md)

    W tym artykule wyjaśniono, kontrola dostępu oparta na rolach katalogowej Active Azure i sposobu jej działania.

-   [RBAC: Wbudowana ról](../active-directory/role-based-access-built-in-roles.md)

    W tym artykule opisano wszystkie dostępne w RBAC role wbudowane.

-   [Opis wdrażania Menedżera zasobów i wdrażania klasyczny](../resource-manager-deployment-model.md)

    W tym artykule omówiono wdrożenia Menedżera zasobów i modeli klasyczny wdrażania i wyjaśniono korzyści wynikające z używania grup zasobów i Menedżera zasobów

-   [Azure obliczeń, sieci i dostawców miejsca do magazynowania w obszarze Azure Menedżera zasobów](../virtual-machines/virtual-machines-windows-compare-deployment-models.md)

    W tym artykule wyjaśniono, jak obliczyć Azure, sieci i dostawców miejsca do magazynowania działają zgodnie z modelem Menedżera zasobów.

-   [Zarządzanie kontrola dostępu oparta na rolach przy użyciu interfejsu API usługi REST](../active-directory/role-based-access-control-manage-access-rest.md)

    W tym artykule pokazano, jak za pomocą interfejsu API usługi REST Zarządzanie RBAC.

-   [Wykaz interfejsu API pozostałych dostawcy zasobów Azure miejsca do magazynowania](https://msdn.microsoft.com/library/azure/mt163683.aspx)

    Jest to odwołanie do interfejsów API umożliwia programowo zarządzać swoim kontem miejsca do magazynowania.

-   [Przewodnik programisty do uwierzytelniania z interfejsem API Menedżera zasobów Azure](http://www.dushyantgill.com/blog/2015/05/23/developers-guide-to-auth-with-azure-resource-manager-api/)

    W tym artykule przedstawiono sposób służą do uwierzytelniania za pomocą interfejsów API Menedżera zasobów.

-   [Kontrola dostępu oparta na rolach dla usługi Microsoft Azure z Ignite](https://channel9.msdn.com/events/Ignite/2015/BRK2707)

    To łącze do klipu wideo w kanale 9 z konferencji Ignite 2015 MS. W tej sesji porozmawiać o dostęp do zarządzania i funkcje raportowania w Azure i Poznaj najlepsze rozwiązania wokół zabezpieczanie dostępu do Azure subskrypcji przy użyciu usługi Azure Active Directory.

###<a name="managing-your-storage-account-keys"></a>Zarządzanie kluczy konta miejsca do magazynowania

Klucze konta przestrzeni dyskowej są ciągami 512-bitowej utworzone przez Azure, której oraz nazwę konta magazynu, można uzyskać dostęp do obiektów danych przechowywanych w oknie konta miejsca do magazynowania, np. blob, podmioty w tabeli, wiadomości w kolejce i plików w udziale plików Azure. Kontrolowanie dostępu do miejsca do magazynowania konta klawiszy umożliwia sterowanie dostępem do płaszczyzny danych dla tego konta miejsca do magazynowania.

Każde konto miejsca do magazynowania ma dwa klucze określane jako "Klucz 1" i "Klucz 2" w [Azure portal](http://portal.azure.com/) i poleceń cmdlet programu PowerShell. Mogą one generowane ręcznie, wykonując jedną z kilku metod, w tym między innymi za pomocą programu PowerShell [Azure portal](https://portal.azure.com/), polecenie Azure lub programowo przy użyciu biblioteki klienta magazynowania .NET lub interfejsu API usługi REST Azure przestrzeni dyskowej usługi.

Istnieje wiele powodów do generowania kluczy konta miejsca do magazynowania.

-   Może je odtworzyć regularnie ze względów bezpieczeństwa.

-   Chcesz odtworzyć kluczy konta miejsca do magazynowania, jeśli ktoś zarządzane funkcja do aplikacji i pobierania klucz, który został ustalony lub zapisane w pliku konfiguracji nadanie im pełnego dostępu do konta przestrzeni dyskowej.

-   Innym przypadku ponownego wygenerowania klucza jest Jeśli Twój zespół korzysta aplikacji Eksplorator magazynu, który zachowuje klucz konta miejsca do magazynowania i pozostawia jednego z członków zespołu. Aplikacja będzie nadal pracować, udzielania dostępu do konta miejsca do magazynowania po ich nieobecności. Faktycznie tego powodu podstawowy, którym utworzone podpisów dostępu do udostępnionych poziomu kont — za pomocą skojarzenia zabezpieczeń na poziomie kont zamiast przechowywania klawiszy dostępu w pliku konfiguracji.

####<a name="key-regeneration-plan"></a>Ponowne generowanie klucza planu

Nie chcesz, aby ponownie wygenerować klucz używanego bez niektórych planowania. Dostęp może do obcinany, do tego konta miejsca do magazynowania, które mogą powodować zakłócenia głównych. Oto Dlaczego istnieją dwa klucze. Powinien odtworzyć jeden klawisz naraz.

Przed generowania kluczy, upewnij się, że masz listę wszystkich aplikacji, które są zależne od konta miejsca do magazynowania, a także innych usług, którego używasz platformy Azure. Na przykład jeśli korzystasz z usługi multimediów Azure, które są zależne od konta przestrzeni dyskowej, należy ponownie synchronizowane klawiszy dostępu za pomocą usługi multimediów po wygenerować klucz. Jeśli korzystasz z dowolnej aplikacji, takich jak Eksplorator magazynu, będzie konieczne udostępnia nowe klucze do tych aplikacji, a także. Należy zauważyć, że jeśli masz maszyny wirtualne, których wirtualnego dysku twardego pliki są przechowywane na koncie miejsca do magazynowania, ta osoba nie wpłynie polega na odtworzeniu klawiszy konta miejsca do magazynowania.

Można odtworzyć kluczy w portalu Azure. Gdy klucze są generowane mogą przyjmować maksymalnie 10 minut do zsynchronizowania w usługach miejsca do magazynowania.

Gdy wszystko będzie już gotowe, Oto ogólny proces określające, jak należy zmienić swój klucz. W tym przypadku jest założenie, że aktualnie używasz klucz 1 i użytkownik chce zmienić wszystkie elementy, aby użyć klucza 2.

1.  Odtworzyć 2 klucz, aby upewnić się, że są bezpieczne. Można to zrobić w Azure portal.

2.  We wszystkich aplikacji, w której jest przechowywany klucz magazynowania zmienić klucz miejsca do magazynowania do użycia klucza 2 nową wartość. Testowanie i publikowanie aplikacji.

3.  Po wszystkich aplikacji i usług są w górę i pomyślnie uruchomiona, ponownie wygenerować klucz 1. To gwarantuje, że każdy, kto komu nie wyraźnie mają nowy klucz będzie już dostępu do konta miejsca do magazynowania.

Jeśli obecnie korzystasz 2 klucza, wykonaj te same kroki, ale odwrócić nazw kluczy.

Można migrować przez kilka dni, zmieniając każdej aplikacji przy użyciu nowego klucza i opublikowanie go. Po zakończeniu każdy z nich należy powrócić i ponownie wygenerować starego klucza, ale nie działa.

Innym rozwiązaniem jest umieszczenie klucz konta miejsca do magazynowania w [Azure klucza magazynu](https://azure.microsoft.com/services/key-vault/) jako poufne i pobrać klucza stamtąd aplikacji. Następnie gdy ponownie wygenerować klucz i zaktualizować magazynu klucza Azure, aplikacje nie musisz ponownego wdrożenia, ponieważ ich wpłynie na nowy klucz z magazynu klucza Azure automatycznie. Zauważ, że możesz mieć aplikacji odczytać klucza za każdym razem, należy go lub możesz buforowania go w pamięci i jeśli go nie powiedzie się podczas korzystania z niego, klucz ponownego pobrania z magazynu klucza Azure.

Przy użyciu magazynu klucza Azure dodaje również inny poziom zabezpieczeń dla kluczy miejsca do magazynowania. Jeśli ta metoda nigdy nie trzeba ustalony klucza miejsca do magazynowania w pliku konfiguracji, co spowoduje usunięcie tego ścieżek ktoś uzyskiwanie dostępu do kluczy bez określonych uprawnień.

Inną zaletą Azure klucza magazynu jest można także sterowanie dostępem do kluczy przy użyciu usługi Azure Active Directory. Oznacza to, że można udzielić dostępu do kilku aplikacje, które należy pobrać klucze z magazynu klucza Azure i wiesz, że inne aplikacje nie będą mogli uzyskać dostęp do kluczy bez nadawania im uprawnienia specjalnie.

Uwaga: zalecane jest używanie tylko jeden z klawiszy we wszystkich aplikacjach, w tym samym czasie. Użycie 1 klucza w niektórych miejscach i 2 klucza w innych nie można obracać kluczy bez utraty dostępu aplikacji.

####<a name="resources"></a>Zasoby

-   [Informacje o kontach Azure miejsca do magazynowania](storage-create-storage-account.md#regenerate-storage-access-keys)

    Ten artykuł zawiera przegląd konta miejsca do magazynowania i w tym artykule omówiono wyświetlania, kopiując i ponownego generowania klawiszy dostępu miejsca do magazynowania.

-   [Wykaz interfejsu API pozostałych dostawcy zasobów Azure miejsca do magazynowania](https://msdn.microsoft.com/library/mt163683.aspx)

    Ten artykuł zawiera łącza do artykułów pobieranie klawiszy konta miejsca do magazynowania i ponownego generowania kluczy konta miejsca do magazynowania dla konto Azure za pomocą interfejsu API usługi REST. Uwaga: To dla kont miejsca do magazynowania Menedżera zasobów.

-   [Operacje na kontach miejsca do magazynowania](https://msdn.microsoft.com/library/ee460790.aspx)

    W tym artykule w odwołaniu interfejsu API usługi masową pozostałych zawiera łącza do określonych artykułów na pobieranie i ponownego generowania kluczy konta miejsca do magazynowania przy użyciu interfejsu API usługi REST. Uwaga: To dla kont miejsca do magazynowania klasyczny.

-   [Koniec z klucza zarządzania — Zarządzanie dostęp do danych magazyn Azure za pomocą Azure AD](http://www.dushyantgill.com/blog/2015/04/26/say-goodbye-to-key-management-manage-access-to-azure-storage-data-using-azure-ad/)

    W tym artykule przedstawiono sposoby kontrolowania dostępu do kluczy magazyn Azure w magazynu klucza Azure za pomocą usługi Active Directory. Pokazuje także jak za pomocą zadania automatyzacji Azure ponownie wygenerować klucze co godzinę.

##<a name="data-plane-security"></a>Bezpieczeństwo płaszczyzny danych

Bezpieczeństwo płaszczyzny danych odwołuje się do metod stosowanych do zabezpieczenia obiektów dane przechowywane w magazynie Azure — obiektów blob, kolejki, tabele i pliki. Firma Microsoft zobaczył metod do szyfrowania danych i zabezpieczeń podczas przesyłania danych, ale jak przejściu o zezwolenia na dostęp do obiektów?

Istnieją dwie metody kontrolowania dostępu do same obiekty danych. Pierwszym jest kontrolowanie dostępu do kluczy konta miejsca do magazynowania, a druga jest korzystanie z podpisów dostępu udostępnione, aby udzielić dostępu do określonych danych obiektów dla określonego przedziału czasu.

Jeden wyjątek Uwaga to, że ustawiając poziom dostępu dla kontener obiektów blob odpowiednio można zezwolić dostępie do obiektów blob. Jeśli ustawisz dostępu dla kontenera obiektów Blob lub kontener umożliwia publiczny dostęp do odczytu dla obiektów blob w danym kontenerze. Oznacza to, że każda osoba z adresem URL wskazujący na obiektów blob w danym kontenerze otwórz go w przeglądarce bez używania podpisu dostępu do udostępnionych lub posiadanie kluczy konta miejsca do magazynowania.

###<a name="storage-account-keys"></a>Klucze konta miejsca do magazynowania

Klucze konta przestrzeni dyskowej są ciągami 512-bitowej utworzone przez Azure, których oraz nazwę konta magazynu, można uzyskać dostęp do obiektów danych przechowywanych w oknie konta przestrzeni dyskowej.

Można na przykład więcej obiektów blob, zapisywania do kolejek, tworzyć tabele i modyfikowania plików. Wiele z tych czynności mogą być wykonywane za pośrednictwem portalu Azure lub przy użyciu jednego z wielu aplikacji Eksploratora magazynu. Można także napisać kod do wykonania tych operacji za pomocą interfejsu API usługi REST lub jeden z biblioteki klienta magazynu.

Opisane w sekcji [Zabezpieczenia płaszczyzny zarządzania](#management-plane-security), dostęp do kluczy miejsca do magazynowania dla konta miejsca do magazynowania klasyczny mogą być udzielane, otrzymują pełny dostęp do Azure subskrypcji. Dostęp do kluczy miejsca do magazynowania dla konta miejsca do magazynowania przy użyciu modelu Menedżera zasobów Azure można kontrolować za pośrednictwem kontrola dostępu oparta na rolach (RBAC).

###<a name="how-to-delegate-access-to-objects-in-your-account-using-shared-access-signatures-and-stored-access-policies"></a>Jak pełnomocnictwa do obiektów do konta za pomocą udostępnionego podpisy dostępu i przechowywane zasady dostępu

Podpis dostępu do udostępnionego jest ciąg zawierający tokenu zabezpieczeń, który może zostać dołączony identyfikatora URI, który umożliwia udzielanie pełnomocnictw do przechowywania obiektów i określanie ograniczeń, takich jak uprawnienia i Data/Godzina zakres programu access.

Można udzielić dostępu do obiektów blob, kontenerów wiadomości w kolejce, pliki i tabel. Tabel można faktycznie udzielone uprawnienia dostępu zakresu obiektów w tabeli, określając zakresy klucza partycją i wiersza do których ma użytkownik miał dostęp. Na przykład jeśli masz dane przechowywane przy użyciu klucza partition stanów geograficznych, można nadać osoby dostępu do tylko dane dla Kalifornii.

Inny przykład może nadaj aplikacji sieci web tokenu skojarzeń zabezpieczeń, który umożliwia tworzyć wpisy w kolejce i nadaj pracownikiem aplikacji roli tokenu skojarzenia zabezpieczeń do otrzymywania wiadomości z kolejki i ich przetwarzania. Lub może przekazać jeden klient token skojarzeń zabezpieczeń, można użyć do przekazania obrazów do kontenera w magazynie obiektów Blob i nadać uprawnienie aplikacji sieci web, aby odczytać te obrazy. W obu przypadkach jest odstęp problemów — każdej aplikacji może być podawana tylko takie uprawnienia dostępu, wymagające w celu wykonania swoich zadań. Można to zrobić za pomocą udostępnionego podpisów programu Access.

####<a name="why-you-want-to-use-shared-access-signatures"></a>Dlaczego ma być używany udostępnione podpisów programu Access

Dlaczego możesz chcieć używać skojarzeń zabezpieczeń zamiast tylko określeniu się klucz konta magazynu jest tak dużo łatwiejsze? Określeniu się klucz konta magazynu jest podobne do udostępniania kluczy usługi Królestwo przestrzeni dyskowej. Udziela jej pełny dostęp. Ktoś może użycie klawiszy i przekazywania ich całej biblioteki utworów muzycznych do swojego konta miejsca do magazynowania. One można także zastąpić zainfekowanych wirusami wersji plików lub kradzież danych. Przekazywanie nieograniczony dostęp do swojego konta miejsca do magazynowania jest czegoś, czego nie należy lekko.

Z podpisami dostępu do udostępnionego można zapewnić klienta tylko uprawnienia wymagane przez ograniczony czas. Na przykład jeśli ktoś przekazywania obiektów blob do swojego konta, możesz przyznać ich zapisu dla tylko wystarczającą ilość czasu przekazać blob (w zależności od rozmiaru obiektów blob, oczywiście). A jeśli zmienisz zdanie, możesz cofnąć przez program access.

Ponadto można określić, że żądania przy użyciu skojarzenia zabezpieczeń są ograniczone do określonych adres IP lub zewnętrzne Azure zakres adresów IP. Można również wymagają, że żądania są wykonywane przy użyciu określonego protokołu (HTTPS lub protokołu HTTP/HTTPS). Oznacza to, jeśli chcesz zezwolić ruchu HTTPS, można ustawić wymagane protokół HTTPS tylko i ruchu HTTP zostaną zablokowane.

####<a name="definition-of-a-shared-access-signature"></a>Definicja podpisu udostępniania

Podpis dostępu do udostępnionych to zbiór dołączane do adres URL wskazujący zasób parametry kwerendy

który zawiera informacje o dostęp dozwolony i okres, dla którego jest dozwolone programu access. Oto przykład; Ten identyfikator URI przewiduje odczyt obiektów blob pięć minut. Uwaga parametry zapytania skojarzeń zabezpieczeń musi być zakodowany adres URL, takie jak % 3A dwukropek (:) lub % 20 spację.

    http://mystorage.blob.core.windows.net/mycontainer/myblob.txt (URL to the blob)
    ?sv=2015-04-05 (storage service version)
    &st=2015-12-10T22%3A18%3A26Z (start time, in UTC time and URL encoded)
    &se=2015-12-10T22%3A23%3A26Z (end time, in UTC time and URL encoded)
    &sr=b (resource is a blob)
    &sp=r (read access)
    &sip=168.1.5.60-168.1.5.70 (requests can only come from this range of IP addresses)
    &spr=https (only allow HTTPS requests)
    &sig=Z%2FRHIX5Xcg0Mq2rqI3OlWTjEg2tYkboXr1P9ZUXDtkk%3D (signature used for the authentication of the SAS)

####<a name="how-the-shared-access-signature-is-authenticated-by-the-azure-storage-service"></a>Jak uwierzytelnieniu podpisu dostępu udostępnione przez usługę magazynowania Azure

Gdy Usługa magazynu odbiera żądania, trwa parametrów wejściowych kwerendy i tworzy podpis przy użyciu tej samej metody jako programu połączeń. Porównuje dwóch podpisów. Jeśli ta osoba zgoda, Usługa magazynu można sprawdzić wersję usługi miejsca do magazynowania w celu upewnij się, że jest prawidłowa, sprawdź, czy bieżącej daty i godziny w określonym okna, upewnij się, że dostęp zażądania odpowiada wniosek itp.

Na przykład z naszych powyżej adresu URL, jeśli adres URL wskazujący została do pliku zamiast obiektów blob to żądanie będzie działać, ponieważ określa, czy podpis dostępu udostępnione dla obiektów blob. Jeśli polecenie pozostałych wywoływana aktualizacji obiektów blob, czy się niepowodzeniem, ponieważ podpis dostępu udostępnione Określa, że dostęp tylko do odczytu jest dozwolone.

####<a name="types-of-shared-access-signatures"></a>Typy podpisów udostępniania

-   Poziom obsługi skojarzeń zabezpieczeń można używać do uzyskiwania dostępu określonych zasobów na koncie miejsca do magazynowania. Niektóre przykłady są pobierane listy obiektów blob w kontenerze pobierania obiektów blob, aktualizowanie jednostki w tabeli, dodawanie wiadomości do kolejki lub przekazywania pliku do udziału plików.

-   Skojarzenia zabezpieczeń na poziomie kont może służyć do wszystkie elementy, które skojarzeń zabezpieczeń na poziomie usługi może być używany do dostępu. Ponadto zapewnia opcji zasoby, które nie są dozwolone do skojarzeń zabezpieczeń poziomu usług, takich jak możliwość tworzenia kontenerów i tabele, kolejek oraz udziałach plików. Dostęp do wielu usług można określić także jednocześnie. Na przykład może przyznać im dostępu do zarówno obiektów blob, jak i plików na koncie miejsca do magazynowania.

####<a name="creating-an-sas-uri"></a>Tworzenie identyfikatora URI skojarzenia zabezpieczeń

1.  Spontaniczne URI można tworzyć na żądanie, definiując wszystkie parametry kwerendy zawsze.

    Jest to naprawdę elastyczne, ale jeśli masz zbiór logiczne parametry, które są podobne za każdym razem, zasadę dostępu przechowywane jest zorientować się.

2.  Możesz utworzyć zasadę dostępu przechowywane dla całego kontenera, udziału plików, tabeli lub kolejki. Następnie można następująco jako podstawy identyfikatory URI skojarzeń zabezpieczeń, możesz utworzyć. Uprawnienia na podstawie zasad przechowywania programu Access można łatwo odwołany. Możesz mieć maksymalnie 5 zasady zdefiniowane dla każdego kontenera, kolejki, tabeli lub udziale plików.

    Na przykład jeśli zostały będziesz mieć wiele osób więcej obiektów blob w określonym kontenerze, możesz utworzyć zasadę dostępu przechowywane informacją, że "udzielić dostępu do odczytu" i inne ustawienia, które będą takie same zawsze. Następnie możesz utworzyć identyfikator URI skojarzeń zabezpieczeń, przy użyciu ustawień zasady dostępu przechowywane i określanie wygasania Data/Godzina. Dzięki temu to, że nie musisz określić wszystkie parametry kwerendy zawsze.

####<a name="revocation"></a>Odwołania

Załóżmy, że zostało naruszone usługi skojarzeń zabezpieczeń lub chcesz je zmienić ze względu na zabezpieczeń firmowych lub wymagania dotyczące zgodności z przepisami. Jak odwołać dostęp do zasobu za pomocą tego skojarzenia zabezpieczeń? To zależy od sposobu utworzenia identyfikatora URI skojarzeń zabezpieczeń.

Jeśli używasz spontanicznych identyfikatora URI, masz trzy opcje. Można problemu tokeny skojarzenia zabezpieczeń z zasad wygasania krótki i po prostu poczekaj, aż skojarzeń zabezpieczeń wygaśnie. Można zmienić lub usunąć zasobów (przy założeniu, że tokenu zostało ograniczone do jednego obiektu). Można zmieniać klawisze konta przestrzeni dyskowej. Opcja Ostatnia mogą mieć duży wpływ, w zależności od tego, jak wiele usług są przy użyciu tego konta miejsca do magazynowania i prawdopodobnie nie jest coś, co chcesz zrobić bez niektórych planowania.

Jeśli korzystasz z skojarzeń zabezpieczeń pochodzące z zasadę dostępu przechowywane, można usunąć dostęp poprzez zasadę dostępu przechowywane odwołanie — zmienić ją po prostu, co już wygasła lub można je całkowicie usunąć. Stosowane natychmiast i unieważnia każdej skojarzenia zabezpieczeń utworzone za pomocą tej zasady przechowywania programu Access. Aktualizowanie lub usuwanie zasady dostępu przechowywane może tabela osób wpływ na uzyskiwanie dostępu do określonego kontenera, udziale plików lub kolejki za pośrednictwem skojarzeń zabezpieczeń, ale jeśli klienci są zapisywane, więc ich żądanie nowe skojarzenia zabezpieczeń starego stał się nieprawidłowe, to będzie działać prawidłowo.

Ponieważ przy użyciu skojarzenia zabezpieczeń pochodzące z zasadę dostępu przechowywane daje możliwość natychmiast Odwoływanie tej skojarzeń zabezpieczeń, jest zalecane, najlepszym rozwiązaniem jest zawsze użycie przechowywane zasady dostępu, jeśli to możliwe.

####<a name="resources"></a>Zasoby

Aby uzyskać szczegółowe informacje na temat korzystania z udostępnionych podpisy dostępu i przechowywane zasady dostępu, wraz z przykładami można znaleźć w następujących artykułach:

-   Są to artykuły odwołania.

    -   [Usługa skojarzenia zabezpieczeń](https://msdn.microsoft.com/library/dn140256.aspx)

        W tym artykule znajdują się przykłady skojarzenia zabezpieczeń na poziomie usługi przy użyciu obiektów blob, wiadomości w kolejce zakresów tabeli i pliki.

    -   [Budowa usługi skojarzenia zabezpieczeń](https://msdn.microsoft.com/library/dn140255.aspx)

    -   [Budowa konta skojarzenia zabezpieczeń](https://msdn.microsoft.com/library/mt584140.aspx)

-   Są to samouczki dotyczące korzystania z biblioteki klienta .NET do tworzenia podpisów dostępu udostępnione i przechowywane zasady dostępu.

    -   [Korzystanie z podpisów udostępniania (SA)](storage-dotnet-shared-access-signature-part-1.md)

    -   [Udostępniony podpisy programu Access, część 2: Tworzenie i używanie skojarzeń zabezpieczeń z usługą obiektów Blob](storage-dotnet-shared-access-signature-part-2.md)

        Ten artykuł zawiera wyjaśnienie modelu skojarzeń zabezpieczeń, przykłady udostępnione podpisów dostępu i zalecenia dotyczące najlepiej używać skojarzenia zabezpieczeń. Zostało to opisane jest odwołania uprawnienie.

-   Ograniczanie dostępu według adresu IP (ACL IP)

    -   [Co to jest punkt końcowy listy kontroli dostępu (ACL)?](../virtual-network/virtual-networks-acl.md)

    -   [Budowa usługi skojarzenia zabezpieczeń](https://msdn.microsoft.com/library/azure/dn140255.aspx)

        Ten artykuł odniesienia dla poziomu usług skojarzeń zabezpieczeń; jest składa się z przykładem ACLing adresów IP.

    -   [Budowa konta skojarzenia zabezpieczeń](https://msdn.microsoft.com/library/azure/mt584140.aspx)

        Ten artykuł odniesienia dla skojarzeń zabezpieczeń poziomu kont; jest składa się z przykładem ACLing adresów IP.

-   Uwierzytelnianie

    -    [Uwierzytelnianie dla usługi Azure miejsca do magazynowania](https://msdn.microsoft.com/library/azure/dd179428.aspx)

-   Udostępnione wprowadzenie samouczka podpisów programu Access

    -   [Wprowadzenie do samouczka skojarzenia zabezpieczeń](https://github.com/Azure-Samples/storage-dotnet-sas-getting-started)

##<a name="encryption-in-transit"></a>Szyfrowanie w drodze

###<a name="transport-level-encryption--using-https"></a>Szyfrowanie poziomu transportu — przy użyciu protokołu HTTPS

Inny krok, który należy wykonać w celu zapewnienia bezpieczeństwa danych magazyn Azure jest szyfrowanie danych między klientem i Magazyn Azure. Pierwszy zaleca się zawsze używaj protokołu [HTTPS](https://en.wikipedia.org/wiki/HTTPS) , który zapewnia komunikacyjne publicznie w Internecie.

Zawsze używaj HTTPS podczas wywoływania interfejsów API pozostałych lub dostęp do obiektów w magazynie. Ponadto **Udostępnione podpisów dostępu**, które mogą być używane do pełnomocnictwa do obiektów magazyn Azure, zawierać opcję, aby określić, że tylko protokołu HTTPS może być używany podczas korzystania z udostępnionych podpisy programu Access, zapewnienie, że każda wysyłanie łączy zawierających tokeny skojarzeń zabezpieczeń użyje odpowiedni protokół.

####<a name="resources"></a>Zasoby

-   [Włącz protokół HTTPS w aplikacji w usłudze Azure aplikacji](../app-service-web/web-sites-configure-ssl-certificate.md)

    W tym artykule pokazano, jak włączyć HTTPS dla aplikacji sieci Web programu Azure.

###<a name="using-encryption-during-transit-with-azure-file-shares"></a>Przy użyciu szyfrowania podczas przesyłania z udziały plików Azure

Azure magazyn plików obsługuje HTTPS, korzystając z interfejsu API usługi REST, ale więcej często używanych jako udziale plików SMB dołączony do maszyny. SMB 2.1 nie obsługuje szyfrowania, aby połączenia są dozwolone tylko w tym samym regionie platformy Azure. Jednak SMB 3.0 obsługuje szyfrowanie i mogą być używane z systemem Windows Server 2012 R2, Windows 8, Windows 8.1 i systemu Windows 10, zezwolenia na region krzyżowe i dostęp do nawet programu access na komputerze.

Należy zauważyć, że chociaż Azure udziały plików mogą być używane z systemem Unix, klient Linux SMB nie obsługuje jeszcze szyfrowania, więc jest tylko dostęp w regionie Azure. Obsługa szyfrowania Linux znajduje się na przewodnik odpowiedzialny za funkcji SMB deweloperów Linux. Podczas dodawania szyfrowania, masz same możliwości dostępu do udziału plików Azure w systemie Linux, podobnie jak w przypadku systemu Windows.

####<a name="resources"></a>Zasoby

-   [Jak korzystać z systemem Linux magazyn plików Azure](storage-how-to-use-files-linux.md)

    W tym artykule przedstawiono sposób instalowania Azure udziale plików w systemie Linux i przekazywania i pobierania plików.

-   [Wprowadzenie do przechowywania plików Azure w systemie Windows](storage-dotnet-how-to-use-files.md)

    Ten artykuł zawiera omówienie udziały plików Azure i instalacji i używania ich przy użyciu programu PowerShell i .NET.

-   [Magazyn plików w środku Azure](https://azure.microsoft.com/blog/inside-azure-file-storage/)

    W tym artykule informuje o ogólnodostępną magazyn plików Azure i szczegółowe informacje techniczne dotyczące szyfrowania SMB 3.0.

###<a name="using-client-side-encryption-to-secure-data-that-you-send-to-storage"></a>Zabezpieczanie danych, które wysyłasz do miejsca do magazynowania za pomocą szyfrowania po stronie klienta

Innym rozwiązaniem, który pomoże Ci upewnij się, że dane są bezpieczne podczas przesyłane między aplikacją kliencką i miejsca do magazynowania jest szyfrowania po stronie klienta. Dane są szyfrowane przed przesyłane do magazynu Azure. Podczas pobierania danych z magazynu Azure, dane są odszyfrowywane po odebraniu po stronie klienta. Mimo że dane są szyfrowane, przechodząc przez sieci, zalecamy również użyć HTTPS, ze względu testy integralności danych wbudowane pomocy zmniejszeniu błędów sieciowych wpływu integralności danych.

Szyfrowania po stronie klienta jest również szyfrowanie danych spoczynku, jak dane są przechowywane w postaci zaszyfrowanej. Firma Microsoft będzie rozmowy na ten temat bardziej szczegółowo w sekcji [szyfrowanie w pozostałych](#encryption-at-rest).

##<a name="encryption-at-rest"></a>Szyfrowanie w pozostałych

Istnieją trzy Azure funkcje, które zapewniają szyfrowanie w pozostałych. Azure szyfrowania dysku jest używany do szyfrowania dysków systemu operacyjnego i danych w IaaS środowisku maszyn wirtualnych systemu. Dwa pozostałe obszary — są szyfrowania po stronie klienta i SSE — zarówno służącego do szyfrowania danych w magazynie Azure. Załóżmy Spójrz na każdej z tych opcji, a następnie wykonaj porównanie i zobacz, gdy każdy z nich można używać.

Podczas korzystania z szyfrowania po stronie klienta, do szyfrowania danych podczas przesyłania (które również są przechowywane w postaci zaszyfrowanej w magazynie), możesz po prostu za pomocą HTTPS podczas przenoszenia, a niektóre sposób automatycznie szyfrowane są przechowywane dane. Istnieją dwa sposoby czynność — szyfrowanie dysków Azure i SSE. Zastosowano bezpośrednio szyfrowanie danych dysków systemu operacyjnego i danych za pośrednictwem SMS, a drugi służy do szyfrowania zapisanych magazynem obiektów Blob platformy Azure danych.

###<a name="storage-service-encryption-sse"></a>Szyfrowanie usługi przechowywania (SSE)

SSE pozwala na żądanie, że Usługa magazynu automatycznie szyfrować dane podczas zapisywania go do magazynu Azure. Po przeczytaniu dane z magazynu Azure będzie odszyfrowywane przez usługę Magazyn przed zwracanych. Umożliwia Zabezpieczanie danych bez konieczności modyfikowania kodu lub dodawanie kodu do dowolnych aplikacji.

To ustawienie ma zastosowanie do konta całego miejsca do magazynowania. Można włączyć i wyłączyć tę funkcję, zmieniając wartość ustawienia. Do tego służy Azure portal, programu PowerShell, polecenie Azure, interfejsu API usługi REST miejsca do magazynowania zasobu dostawcy lub biblioteka klienta .NET miejsca do magazynowania. SSE jest domyślnie wyłączona.

W tym momencie klawisze używane do szyfrowania są zarządzane przez firmę Microsoft. Firma Microsoft pierwotnie generowania kluczy i zarządzanie nimi bezpieczne przechowywanie klawisze, a także zwykłe obrót, zdefiniowane przez wewnętrznych zasad firmy Microsoft. W przyszłości będzie nadchodzi możliwość zarządzania kluczy szyfrowania, a następnie wprowadź ścieżkę migracji z kluczy zarządzany przez firmę Microsoft do klawiszy zarządzane przez klienta.

Ta funkcja jest dostępna w przypadku kont standardowe i miejsca do magazynowania Premium utworzony przy użyciu modelu wdrożenia Menedżera zasobów. SSE dotyczy tylko zablokować obiektów blob, blob strony, a następnie dołączyć obiektów blob. Inne typy danych, w tym tabele, kolejki i plików, nie będą szyfrowane.

Dane są szyfrowane tylko wtedy, gdy jest włączone SSE i zapisywania danych z magazynem obiektów Blob. Włączanie lub wyłączanie SSE nie ma wpływu na istniejące dane. Innymi słowy po włączeniu szyfrowania nie Przejdź wstecz i szyfrowania danych, który już istnieje; ani odszyfrowywanie danych, który już istnieje po wyłączeniu SSE.

Tej funkcji można korzystać przy użyciu konta z miejsca do magazynowania klasycznym należy można utworzyć nowe konto miejsca do magazynowania Menedżera zasobów i skopiuj dane do nowego konta za pomocą AzCopy. 

###<a name="client-side-encryption"></a>Szyfrowania po stronie klienta

Wspomniano szyfrowania po stronie klienta, gdy omawiane do szyfrowania danych podczas przesyłania. Ta funkcja pozwala na programowy szyfrowania danych w aplikacji klienckiej przed wysłaniem wielu sieci Azure miejsca do magazynowania w celu zapisania i programowy odszyfrowywanie danych po pobraniu go z magazynu Azure.

To zapewnia szyfrowania podczas przesyłania, ale umożliwia także funkcję szyfrowania spoczynku. Należy zauważyć, że chociaż dane są szyfrowane podczas przesyłania, nadal zaleca się przy użyciu protokołu HTTPS skorzystać z kontroli integralności danych wbudowane, które pomóc w zmniejszeniu błędów sieciowych wpływu integralności danych.

Przykład miejsce, w którym można to jest, jeśli masz aplikację sieci web przechowuje obiekty BLOB, który pobiera obiektów blob i ma być w najlepszy możliwy aplikacji i danych. W tym przypadku należy użyć szyfrowania po stronie klienta. Ruch między klientem a usługą Azure obiektów Blob zawiera zaszyfrowane zasobu, a nikt nie interpretowania danych podczas przesyłania i przywróć go do sieci prywatnych obiektów blob.

Szyfrowania po stronie klienta jest wbudowany w Java i bibliotekach klienta .NET miejsca do magazynowania, które z kolei przy użyciu Azure klucza magazynu interfejsów API, dzięki czemu dosyć łatwe do wykonania. Proces szyfrowania i odszyfrowywania danych jest używana technika koperty i przechowuje metadane używane przez szyfrowanie w obiekcie każdego miejsca do magazynowania. Na przykład dla obiektów blob, przechowywanych w niej go w metadanych obiektów blob, natomiast w przypadku kolejki, dodanie go do każdej wiadomości w kolejce.

Do szyfrowania samej można wygenerować i zarządzać kluczy szyfrowania. Możesz również użyć klawiszy wygenerowane przez Biblioteka klienta magazynowania Azure lub masz magazynu klucza Azure generowania kluczy. Kluczy szyfrowania można przechowywać w Twoim magazynie klucza lokalnego lub można je przechowywać w magazynu klucza Azure. Azure magazynu klucza umożliwia udzielić dostępu do hasła Azure klucza magazynu dla określonych użytkowników za pomocą usługi Azure Active Directory. Oznacza to, że nie tylko każda odczytywanie magazynu klucza Azure i pobrać kluczy, którego używasz do szyfrowania po stronie klienta.

####<a name="resources"></a>Zasoby

-   [Szyfrowanie i odszyfrowywanie obiektów blob w magazynie Azure firmy Microsoft przy użyciu magazynu klucza Azure](storage-encrypt-decrypt-blobs-key-vault.md)

    W tym artykule pokazano, jak używać szyfrowania po stronie klienta z magazynu klucza Azure, w tym sposobu tworzenia KEK i zapisać go w magazynu przy użyciu programu PowerShell.

-   [Magazyn klucza szyfrowania po stronie klienta i Azure magazynu platformy Microsoft Azure](storage-client-side-encryption.md)

    W tym artykule przedstawiono wyjaśnienie szyfrowania po stronie klienta i znajdują się przykłady używania biblioteki klienta miejsca do magazynowania do szyfrowania i odszyfrowywania zasoby z czterech usług miejsca do magazynowania. Również zawiera temat Azure klucza magazynu.

###<a name="using-azure-disk-encryption-to-encrypt-disks-used-by-your-virtual-machines"></a>Szyfrowanie dysków używane przez maszyn wirtualnych za pomocą szyfrowania dysku Azure

Azure szyfrowanie dysku jest nową funkcję, która jest obecnie w podglądzie. Ta funkcja umożliwia szyfrowanie dyski systemu operacyjnego i dane używane przez maszyn wirtualnych IaaS. Dla systemu Windows dyski są szyfrowane przy użyciu technologii szyfrowania standardowych funkcji BitLocker. Linux dysków są szyfrowane za pomocą technologii kryptograficznego DM. Jest to zintegrowany z magazynu klucza Azure pozwala na sterowanie i zarządzanie nimi kluczy szyfrowania dysku.

Rozwiązanie szyfrowanie dysków Azure obsługuje następujące trzy szyfrowania scenariuszy:

-   Włącz szyfrowanie dla nowych IaaS maszyny wirtualne utworzone na podstawie pliki zaszyfrowane klienta wirtualnego dysku twardego oraz klucze szyfrowania klienta, które są przechowywane w Azure klucza magazynu.

-   Włącz szyfrowanie dla nowych IaaS maszyny wirtualne utworzone na podstawie Azure Marketplace.

-   Włącz szyfrowanie dla istniejących IaaS maszyny wirtualne już uruchomiony w Azure.

>[AZURE.NOTE] W przypadku maszyny wirtualne Linux już uruchomiony w Azure lub nowe Linux maszyny wirtualne utworzone na podstawie obrazów w Azure Marketplace szyfrowania dysku system operacyjny nie jest obecnie obsługiwane. Szyfrowanie woluminu systemu operacyjnego dla maszyny wirtualne Linux jest obsługiwane tylko w przypadku maszyny wirtualne, które zostały zaszyfrowane lokalnego i przekazane do Azure. To ograniczenie dotyczy tylko na dysku z systemem operacyjnym; szyfrowanie ilości danych dla maszyny Linux jest obsługiwane.

Rozwiązanie obsługuje następujące maszyny wirtualne IaaS dla wersji preview publicznej włączenie platformy Microsoft Azure:

-   Integracja z magazynu klucza Azure

-   Standardowe [A, D i maszyny wirtualne IaaS serii G](https://azure.microsoft.com/pricing/details/virtual-machines/)

-   Włącz szyfrowanie dla maszyny wirtualne IaaS utworzony przy użyciu modelu [Azure Menedżera zasobów](../azure-resource-manager/resource-group-overview.md)

-   Wszystkie Azure publicznej [regionów](https://azure.microsoft.com/regions/)

Ta funkcja zapewnia, że wszystkie dane na dyskach maszyn wirtualnych są szyfrowane spoczynku w magazynie Azure.

####<a name="resources"></a>Zasoby

-   [Szyfrowanie dysków Azure dla systemu Windows i Linux oraz IaaS maszyn wirtualnych](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)

    W tym artykule omówiono wersji preview szyfrowania dysku Azure i łącza do pobrania oficjalny dokument.

###<a name="comparison-of-azure-disk-encryption-sse-and-client-side-encryption"></a>Porównanie szyfrowania dysku Azure, SSE i szyfrowania po stronie klienta

####<a name="iaas-vms-and-their-vhd-files"></a>Maszyny wirtualne IaaS i ich plików wirtualnego dysku twardego

Dla dysków za pośrednictwem SMS IaaS zaleca się przy użyciu szyfrowania dysku Azure. Możesz włączyć SSE szyfrowanie plików wirtualny dysk twardy, które są używane do tworzenia kopii tych dysków w magazynie Azure, ale są szyfrowane tylko nowo pisanych dane. Oznacza to, że jeśli utworzysz maszyny, a następnie włącz SSE na rachunku miejsca do magazynowania, w której znajduje się plik wirtualny dysk twardy, tylko zmiany będą zaszyfrowane, nie oryginalny plik wirtualnego dysku twardego.

Jeśli tworzysz maszyn wirtualnych przy użyciu obrazu z usługi Azure Marketplace Azure wykonuje [skrócona Kopiuj](https://en.wikipedia.org/wiki/Object_copying) obraz do swojego konta miejsca do magazynowania w magazynie Azure, a nie są szyfrowane, nawet jeśli użytkownik ma SSE włączone. Po maszyn wirtualnych tworzy i uruchamia aktualizowanie obrazu, SSE rozpocznie się dane. Z tego powodu najlepiej używać szyfrowania dysku Azure na maszyny wirtualne utworzone za pomocą obrazów w Azure Marketplace, jeśli chcesz całkowicie zaszyfrowany.

Jeśli wstępnie zaszyfrowanych maszyn wirtualnych do Azure ze źródeł lokalnych, można przekazać kluczy szyfrowania do magazynu klucza Azure i kontynuować przy użyciu szyfrowania dla tego maszyn wirtualnych, że zostały za pomocą lokalnego. Azure szyfrowanie dysków włączono obsługę w tym scenariuszu.

Jeśli masz tylko danych szyfrowanych wirtualnego dysku twardego ze źródeł lokalnych, możesz przekazać go do galerii jako obraz niestandardowy i obsługi administracyjnej maszyn wirtualnych z niego. Jeśli możesz to zrobić przy użyciu szablonów Menedżera zasobów, możesz poprosić go, aby włączyć szyfrowanie dysku Azure, gdy uruchomi się w górę maszyn wirtualnych.

Po dodaniu dysku danych i zainstalować go na maszyn wirtualnych, można włączyć szyfrowanie dysków Azure na nim danych. Go lokalnie dysku dane są szyfrowane najpierw, a następnie warstwy usług zarządzania będzie wykonaj zapis z opóźnieniem przed miejsca do magazynowania, więc zawartości miejsca do magazynowania jest zaszyfrowany.

####<a name="client-side-encryption"></a>Szyfrowania po stronie klienta####

Szyfrowanie po stronie klienta jest najbezpieczniejsza metoda szyfrowania danych, ponieważ są szyfrowane go przed przewozowe, a dane spoczynku są szyfrowane. Jednak wymaga, Dodaj kod aplikacji przy użyciu magazynu, co może chcesz zrobić. W tych przypadkach umożliwiają HTTPs danych na czas przesyłania i SSE szyfrowanie danych na pozostałych.

Szyfrowanie po stronie klienta można zaszyfrować jednostek tabeli, wiadomości w kolejce i obiektów blob. Z SSE można szyfrować tylko obiektów blob. Jeśli potrzebujesz tabeli i kolejki szyfrowania danych, należy używać szyfrowania po stronie klienta.

Po stronie klienta szyfrowania odbywa się całkowicie przez aplikację. Jest to najbezpieczniejsza metoda, ale wymagają zmian programistyczny do aplikacji i wprowadzić zarządzania kluczami procesów. Czy jest używany przy ma dodatkowe zabezpieczenia podczas przesyłania, a przechowywane dane były szyfrowane.

Szyfrowanie po stronie klienta jest większe obciążenie na komputerze klienckim, a masz konto w tym planów skalowalność, zwłaszcza jeśli są szyfrowania i przenoszenia wiele danych.

####<a name="storage-service-encryption-sse"></a>Szyfrowanie usługi przechowywania (SSE)

SSE zarządza Magazyn Azure. Za pomocą SSE nie przewiduje bezpieczeństwa danych podczas przesyłania, ale szyfrowania danych, podczas jego zapisywania do magazynu Azure. Nie ma żadnego wpływu na wydajność podczas korzystania z tej funkcji.

Można tylko szyfrowanie blob blok, Dołącz obiektów blob i strony przy użyciu SSE obiektów blob. Jeśli potrzebujesz szyfrowanie tabeli lub kolejki danych, należy rozważyć, przy użyciu szyfrowania po stronie klienta.

Jeśli masz archiwum lub w bibliotece plików wirtualny dysk twardy, których możesz używać jako podstawy dla tworzenia nowych maszyn wirtualnych, Utwórz nowe konto miejsca do magazynowania, Włącz SSE, a następnie przekaż pliki wirtualnego dysku twardego do tego konta. Tych plików wirtualnego dysku twardego będą szyfrowane przez Magazyn Azure.

Jeśli masz włączone w przypadku dysków w maszyn wirtualnych i SSE włączone dla konta magazynu ze swoich plików wirtualnego dysku twardego szyfrowanie dysku Azure działa prawidłowo; spowodują danych nowo zapisywane są szyfrowane dwa razy.

##<a name="storage-analytics"></a>Analizy miejsca do magazynowania

###<a name="using-storage-analytics-to-monitor-authorization-type"></a>Typ autoryzacji można monitorować za pomocą analizy miejsca do magazynowania

Dla każdego konta miejsca do magazynowania możesz włączyć Azure analizy miejsca do magazynowania do wykonywania rejestrowania i przechowywania danych metryki. To jest doskonałym narzędziem do chcesz sprawdzić wskaźniki konta przestrzeni dyskowej i konieczne rozwiązywanie problemów z kontem miejsca do magazynowania, ponieważ występują problemy z wydajnością przy użyciu.

Inny fragment danych wyświetlanych w dziennikach analizy miejsca do magazynowania jest metoda uwierzytelniania używana przez osobę, gdy będą uzyskiwać dostęp do miejsca do magazynowania. Na przykład z magazynem obiektów Blob jest widoczny użycie podpisu dostępu do udostępnionych lub klawiszy konta miejsca do magazynowania lub jeśli publicznej blob uzyskać do nich dostęp.

Może to być naprawdę przydatne, jeśli są ściśle ochrona dostępu do magazynu. Na przykład w magazynie obiektów Blob można ustawić opcję prywatne wszystkich kontenerów i implementowanie korzystania z usługi skojarzeń zabezpieczeń w całej aplikacji. Następnie możesz sprawdzić dzienniki regularnie, aby sprawdzić, czy z obiektami BLOB są dostępne za pomocą klawiszy konta miejsca do magazynowania, które mogą wskazywać naruszenia bezpieczeństwa, lub jeśli BLOB są publiczne, ale nie powinny być.

####<a name="what-do-the-logs-look-like"></a>Jak wyglądają dzienniki?

Po włączeniu metryki magazynowania konta i rejestrowanie za pośrednictwem portalu Azure analizy danych rozpocznie się szybko zebrać. Rejestrowanie i wskaźniki dla każdej usługi różni się; rejestrowanie jest zapisywany tylko, gdy istnieje działanie tego konta magazynu podczas metryki będą rejestrowane co minutę, co godzinę lub codziennie, w zależności od sposobu skonfigurowania.

Dzienniki są przechowywane w bloku BLOB w kontenerze o nazwie $logs na koncie miejsca do magazynowania. Ten kontener są tworzone po włączeniu analizy miejsca do magazynowania. Po utworzeniu ten kontener nie można usunąć, można usuwać jego zawartość.

W kontenerze $logs istnieje folder dla każdej usługi, a następnie istnieją podfolderów za rok miesiąc/dzień/godzinę. W obszarze godziny po prostu numerowania dzienniki. Oto jak będą wyglądały strukturę katalogu:

![Wyświetlanie plików dziennika](./media/storage-security-guide/image1.png)

Każdego żądania magazyn Azure jest rejestrowany. Oto migawki pliku dziennika, w której są wyświetlane pola kilka pierwszego.

![Migawka pliku dziennika](./media/storage-security-guide/image2.png)

Widać, że umożliwia dzienniki śledzenia dowolnego rodzaju połączenia z kontem miejsca do magazynowania.

####<a name="what-are-all-of-those-fields-for"></a>Co to są wszystkie te pola dla?

Istnieje wymienionym zasobów poniżej zawiera listę wiele pól w dzienniku i są używane. Oto lista pól w kolejności:

![Migawka pól w pliku dziennika](./media/storage-security-guide/image3.png)

Firma Microsoft interesuje pozycje GetBlob oraz sposób ich uwierzytelniania, potrzebujemy Szukaj pozycji typu operacji "Get-obiektów Blob" i sprawdź stan żądania (4<sup>tej</sup> kolumny) i typ autoryzacji (8<sup>tej</sup> kolumny).

Na przykład w pierwszych wierszach na liście powyżej, stan żądania jest "Sukces" i typ autoryzacji "uwierzytelnieniu". Oznacza to, że żądania wymaganego przy użyciu klucza konta miejsca do magazynowania.

####<a name="how-are-my-blobs-being-authenticated"></a>Jak są uwierzytelnianie Moje blob?

Udostępniamy trzy przypadki, w których firma Microsoft interesuje.

1.  To jest publiczna i jest dostępny, przy użyciu adresu URL bez podpisu dostępu udostępnione. W tym przypadku stan żądania jest "AnonymousSuccess" i "anonimowe" jest typ autoryzacji.

    1.0; 2015-11-17T02:01:29.0488963Z; GetBlob; **AnonymousSuccess**200 124; 37; **anonimowe**; mystorage...

2.  To jest prywatna i użyto podpisem dostępu udostępnione. W tym przypadku stan żądania jest "SASSuccess" i "skojarzeń zabezpieczeń" jest typ autoryzacji.

    1.0; 2015-11-16T18:30:05.6556115Z; GetBlob; **SASSuccess**200 416; 64; **skojarzenia zabezpieczeń**; mystorage...

3.  To jest prywatna i klucz magazynowania został użyty do nich dostęp. W tym przypadku stan żądania jest "**Sukces**" i typ autoryzacji jest "**uwierzytelniony**".

    1.0; 2015-11-16T18:32:24.3174537Z; GetBlob; **Powodzenia**206 59; 22; **Uwierzytelnianie**; mystorage...

Microsoft Analyzer wiadomości służy do wyświetlania i analizowania dzienników. Zawiera funkcje wyszukiwania i filtrowania. Na przykład, można wyszukać wystąpienia GetBlob czy obciążenie jest oczekiwanego, to znaczy aby się upewnić, że ktoś jest nie uzyskuje dostępu do konta miejsca do magazynowania niewłaściwie.

####<a name="resources"></a>Zasoby

-   [Analizy miejsca do magazynowania](storage-analytics.md)

    W tym artykule przedstawiono analizy miejsca do magazynowania i jak je włączyć.

-   [Format dziennika analizy miejsca do magazynowania](https://msdn.microsoft.com/library/azure/hh343259.aspx)

    W tym artykule przedstawiono Format dziennika analizy miejsca do magazynowania i szczegóły dostępnych pól, łącznie z — typ uwierzytelniania, który wskazuje typ uwierzytelniania żądania.

-   [Monitorowanie konto przestrzeni dyskowej w Azure portal](storage-monitor-storage-account.md)

    W tym artykule pokazano, jak skonfigurować monitorowanie miar i rejestrowanie konta miejsca do magazynowania.

-   [Rozwiązywanie problemów z końca do końca przy użyciu metryki magazynowania Azure i rejestrowanie, AzCopy i Analizator wiadomości](storage-e2e-troubleshooting.md)

    Ten artykuł zawiera informacje o Rozwiązywanie problemów za pomocą analizy miejsca do magazynowania i pokazano, jak za pomocą analizatora wiadomość firmy Microsoft.

-   [Przewodnik operacyjnym analizatora wiadomości firmy Microsoft](https://technet.microsoft.com/library/jj649776.aspx)

    Ten artykuł jest odwołaniem dla analizatora wiadomości firmy Microsoft i zawiera łącza do samouczka, szybki start i funkcji podsumowania.

##<a name="cross-origin-resource-sharing-cors"></a>Zasób Origin krzyżowe udostępniania (CORS)

###<a name="cross-domain-access-of-resources"></a>Dostęp z innych domen zasobów

W przypadku przeglądarki sieci web działa w jedną domenę żądania HTTP dla zasobu z innej domeny, jest to żądanie HTTP origin krzyżowe. Na przykład strony HTML z contoso.com żąda jpeg hostowana w fabrikam.blob.core.windows.net. Ze względów bezpieczeństwa przeglądarki ograniczyć żądania HTTP między origin zainicjowane z poziomu skryptów, takich jak JavaScript. Oznacza to, że gdy kodu JavaScript na stronie sieci web w domenie contoso.com żąda tego jpeg na fabrikam.blob.core.windows.net, przeglądarce uniemożliwi wezwanie.

Co to ma zrobić z magazynu platformy Azure Dobrze, jeśli przechowujesz statyczne zasoby, takie jak pliki danych JSON lub XML w magazynie obiektów Blob przy użyciu konta miejsca do magazynowania o nazwie Fabrikam, domena zasobów będzie fabrikam.blob.core.windows.net i contoso.com aplikacji sieci web nie będą mogli uzyskiwać do nich dostęp za pomocą języka JavaScript, ponieważ domeny są różne. Jest to również wartość PRAWDA, jeśli próbujesz jedną z usług przechowywania Azure — takich jak magazyn tabel — połączeń, które zwracają przetwarzanych przez klienta JavaScript danych JSON.

####<a name="possible-solutions"></a>Rozwiązanie

Jednym ze sposobów rozwiązania tego problemu jest przypisać fabrikam.blob.core.windows.net domenę niestandardową, na przykład "storage.contoso.com". Problem polega na tym, że można przypisać tylko tę domenę niestandardową do konta jednego miejsca do magazynowania. Co zrobić, jeśli zasoby są przechowywane w wielu kont miejsca do magazynowania?

Aby rozwiązać ten problem w inny sposób ma pełnić rolę serwera proxy dla połączeń magazynowania aplikacji sieci web. Oznacza to, jeśli plik jest przekazywany z magazynem obiektów Blob, aplikacji sieci web będzie albo zapisać go lokalnie, a następnie skopiuj go z magazynem obiektów Blob lub chcesz odczytać wszystkich go do pamięci, a następnie napisać magazynem obiektów Blob. Alternatywnie można napisać dedykowane aplikację sieci web (na przykład API sieci Web) przekazywania plików lokalnie i zapisuje je z magazynem obiektów Blob. W obu przypadkach trzeba kont dla tej funkcji, gdy Określanie skalowalność wymaga.

####<a name="how-can-cors-help"></a>Jak może pomóc CORS

Magazyn Azure umożliwia włączenie CORS — krzyżowe współużytkowanie zasobów pochodzenia. Dla każdego konta miejsca do magazynowania można określić domen, które ma dostęp do tego konta magazynu zasobów. Na przykład w naszym przypadku powyższym możemy można włączyć CORS na rachunku fabrikam.blob.core.windows.net miejsca do magazynowania i skonfigurować go, aby zezwolić na dostęp do contoso.com. Następnie contoso.com aplikacji sieci web można uzyskać dostęp do zasobów w fabrikam.blob.core.windows.net.

Jedyne pamiętać, to, że CORS umożliwia dostęp, ale nie wymaga uwierzytelniania, co jest wymagane dla wszystkich dostęp publicznej zasobów miejsca do magazynowania. Oznacza to, że są dostępne tylko obiektów blob jeśli są one publicznej lub dołączanie podpisu dostępu udostępnione umożliwiając odpowiednich uprawnień. Tabele, kolejki i plików nie mają publicznej dostępu, a wymagają skojarzeń zabezpieczeń.

Domyślnie CORS jest wyłączone na wszystkich usług. Możesz włączyć CORS za pomocą interfejsu API usługi REST lub biblioteka klienta miejsca do magazynowania nawiązywanie połączenia z jedną z metod, aby ustawić zasady usług. Po wykonaniu tej czynności, możesz umieścić regułę CORS, która jest w formacie XML. Oto przykład reguły CORS, który został ustawiony przy użyciu operacji Ustawianie właściwości usługi obiektów Blob usługi konta miejsca do magazynowania. Można wykonywać tej operacji za pomocą biblioteki klienta miejsca do magazynowania lub za pośrednictwem interfejsów API pozostałych magazyn Azure.

    <Cors>    
        <CorsRule>
            <AllowedOrigins>http://www.contoso.com, http://www.fabrikam.com</AllowedOrigins>
            <AllowedMethods>PUT,GET</AllowedMethods>
            <AllowedHeaders>x-ms-meta-data*,x-ms-meta-target*,x-ms-meta-abc</AllowedHeaders>
            <ExposedHeaders>x-ms-meta-*</ExposedHeaders>
            <MaxAgeInSeconds>200</MaxAgeInSeconds>
        </CorsRule>
    <Cors>

Oto, co oznacza każdego wiersza:

-   **AllowedOrigins** To informuje niepasujące domen można zażądać i odbioru danych z usługi Magazyn. Ten komunikat, że zarówno contoso.com, jak i fabrikam.com można zażądać danych z magazynem obiektów Blob konta określonego miejsca do magazynowania. Można także ustawić to symbol wieloznaczny (\*) umożliwia wszystkich domen do żądań dostępu.

-   **AllowedMethods** Jest to lista metod (zleceń żądania HTTP), których można używać podczas tworzenia żądania. W tym przykładzie są dozwolone tylko ODBIERZ i Wyślij. Można ustawić ten symbol wieloznaczny (\*) umożliwia wszystkie metody ma być używany.

-   **AllowedHeaders** Jest to nagłówków żądania, które domenie pochodzenia można określić, kiedy zgłaszającego żądanie. W tym przykładzie wszystkie nagłówki metadanych, zaczynając od x-ms metadane x-ms-meta docelowej i x-ms-meta-abc jest dozwolone. Symbol wieloznaczny (\*) wskazuje, że dowolny nagłówek począwszy od określonego prefiksu jest dozwolone.

-   **ExposedHeaders** To informuje, które nagłówki odpowiedzi należy dostępne w przeglądarce o wystawcy wezwanie. W tym przykładzie dowolny nagłówek, zaczynając od "x-ms - meta-" będą dostępne.

-   **MaxAgeInSeconds** Jest to maksymalnej ilości czasu przeglądarki będzie buforowania przez wstępnej żądanie opcje. (Aby uzyskać więcej informacji o żądaniu wstępnej Sprawdź pierwszego artykułu poniżej).

####<a name="resources"></a>Zasoby

Aby uzyskać więcej informacji na temat CORS i jak je włączyć sprawdź następujące zasoby.

-   [Zasób Origin krzyżowe udostępniania (CORS) obsługę usługi Azure miejsca do magazynowania na Azure.com](storage-cors-support.md)

    W tym artykule omówiono CORS oraz ustawiania reguł dla usług innego miejsca do magazynowania.

-   [Zasób Origin krzyżowe udostępniania (CORS) obsługę usług Azure miejsca do magazynowania w witrynie MSDN](https://msdn.microsoft.com/library/azure/dn535601.aspx)

    Jest to odwołanie dokumentacji CORS obsługę usługi Azure miejsca do magazynowania. Zawiera łącza do artykułów zastosowanie dla każdej usługi miejsca do magazynowania i przedstawiono przykład i wyjaśniono, każdy element w pliku CORS.

-   [Przechowywania platformy Microsoft Azure: Wprowadzenie CORS](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/02/03/windows-azure-storage-introducing-cors.aspx)

    To łącze do artykułu początkowej blogu informowanie o CORS i jak z niej korzystać.

##<a name="frequently-asked-questions-about-azure-storage-security"></a>Często zadawane pytania dotyczące zabezpieczeń magazyn Azure

1.  **Jak sprawdzić, integralności obiektów blob, który I jestem przenoszenia do lub poza magazyn Azure Jeśli nie można używać protokołu HTTPS?**

    Dowolnego powodu, które należy użyć protokołu HTTP zamiast HTTPS i pracy z obiektami blob blok, umożliwiają sprawdzanie MD5 do weryfikowania integralności blob przesyłane. Dzięki temu ochrona przed błędy warstwy sieci i transportu, ale niekoniecznie atakami pośredniczącego.

    Jeśli używasz protokołu HTTPS, która udostępnia zabezpieczeń na poziomie transportu, a następnie za pomocą sprawdzania MD5 jest zbędne i niepotrzebne.
    
    Więcej informacji zapoznaj się z [Omówieniem MD5 obiektów Blob platformy Azure](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/02/18/windows-azure-blob-md5-overview.aspx).

2.  **Co z otwieraniem FIPS zgodności dla Rządu Stanów Zjednoczonych?**

    Stany Zjednoczone informacji przetwarzania FIPS (Federal Standard) określa cryptographic algorytmów zatwierdzone do użycia przez US Federal systemy komputerowe dla instytucji rządowych, dla ochrony ważnych danych. Włączanie FIPS tryb na serwerze systemu Windows lub pulpitu informuje system operacyjny tylko zatwierdzone FIPS algorytmów szyfrowania powinien być używany. Jeśli aplikacja używa niezgodnych algorytmów, podziały aplikacje. Wersje With.NET Framework 4.5.2 lub nowszym, aplikacja automatycznie przełącza algorytmy kryptograficzny użycia algorytmów zgodnych z FIPS, gdy komputer jest w trybie FIPS.

    Microsoft pozostawia ją do każdego z klientów zdecydować, czy włączyć tryb FIPS. Firma Microsoft podejrzenie, że nie ma żadnego atrakcyjnej formie powodu dla klientów, którzy nie są objęte przepisów dla instytucji rządowych, aby włączyć tryb FIPS domyślne.

    **Zasoby**

-   [Dlaczego firma Microsoft już nie zalecenie "Tryb FIPS" już](http://blogs.technet.com/b/secguide/archive/2014/04/07/why-we-re-not-recommending-fips-mode-anymore.aspx)

    W tym artykule blogu zestawienie FIPS i wyjaśniono, dlaczego nie umożliwiają tryb FIPS domyślnie.

-   [FIPS 140 sprawdzania poprawności](https://technet.microsoft.com/library/cc750357.aspx)

    Ten artykuł zawiera informacje dotyczące sposobu produktów firmy Microsoft i moduły cryptographic zgodny ze standardem FIPS dla Stanów Zjednoczonych statystycznych.

-   ["Kryptograficzny system: Użyj FIPS algorytmów szyfrowania, mieszania i podpisywania" efekty ustawienia zabezpieczeń w systemie Windows XP i w nowszych wersjach systemu Windows](https://support.microsoft.com/kb/811833)

    Ten artykuł zawiera informacje o trybu FIPS w starszych komputerach z systemem Windows.
