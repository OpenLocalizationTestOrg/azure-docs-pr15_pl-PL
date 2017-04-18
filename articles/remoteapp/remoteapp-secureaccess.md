
<properties 
    pageTitle="Zabezpieczanie dostępu do Azure RemoteApp i poza | Microsoft Azure"
    description="Dowiedz się, jak bezpiecznego dostępu do Azure RemoteApp za pomocą warunkowego dostępu w usłudze Active Directory platformy Azure"
    services="remoteapp"
    documentationCenter="" 
    authors="piotrci" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />

# <a name="securing-access-to-azure-remoteapp-and-beyond"></a>Zabezpieczanie dostępu do Azure RemoteApp i poza

> [AZURE.IMPORTANT]
> Azure RemoteApp jest są przerywane. W tym artykule [anons](https://go.microsoft.com/fwlink/?linkid=821148) , aby uzyskać szczegółowe informacje.

W tym artykule, firma Microsoft udzieli omówienie jak administrator może skonfigurować kanału bezpiecznego dostępu, rozpoczynając od użytkownika przy użyciu funkcji RemoteApp Azure, a kończąc bezpiecznego zasobów, takich jak bazy danych SQL lub innej aplikacji wewnętrznej. Celem jest upewnij się, że tylko autoryzowani użytkownicy spełniających odpowiednie warunki mają dostęp do aplikacji zdalnego i że bezpiecznego wewnętrznej jest możliwy tylko z kontrolowaną środowiska Azure RemoteApp, a nie z innych lokalizacji.

Istnieją 3 głównych obszarów, których potrzebuje Administrator aby przyjrzeć się:

![Zagadnienia dostępu warunkowego RemoteApp Azure](./media/remoteapp-secureaccess/ra-conditionalenvironment.png)

Odczytywanie informacji i odpowiedzi na te pytania.

## <a name="who-can-access-the-collection"></a>Kto może uzyskać dostępu do kolekcji?
Administrator wybiera użytkowników, którzy mają dostęp do aplikacji zdalnego w kolekcji. Możesz użyć pracy usługi Azure Active Directory (Azure AD) lub konta służbowego (wcześniej nazywanego, "konta organizacji") lub konta Microsoft (np. @outlook.com). Większość scenariuszy przedsiębiorstwa za pomocą konta Azure AD; pozwalają za pomocą dostępu warunkowego funkcji omówiono w dalszej części i są również wybór tylko dla zbiorów domeny. Pozostałej części tego artykułu przyjęto założenie, że używasz konta Azure AD przy użyciu Azure RemoteApp.

**Co to jest firma Microsoft osiągnąć:**

Sterowanie dostępem do funkcji RemoteApp Azure za pomocą konta Azure AD daje dwie czynności:

1.  Zawsze wiedzieć uzyskiwać dostęp do aplikacji, możemy opublikowania i uzyskać dostępu do żadnych kończy się wstecz te aplikacje nawiązać połączenie.
2.  Sterować źródłowych Azure AD możemy utworzyć i usuń konta użytkowników, Ustaw zasady haseł, użyć uwierzytelnianie wieloskładnikowe itp. 

## <a name="how-is-the-collection-accessed-from-where"></a>Jak jest dostępny kolekcji Gdzie?
Często Administratorzy trzeba zdefiniować zasady dostępu do publicznych środowiska z Internetu, takich jak funkcja RemoteApp platformy Azure. Na przykład chcą zapewnić użytkownikom uzyskiwanie dostępu do środowiska spoza sieci firmowej za pomocą uwierzytelnianie wieloskładnikowe (MFA) dostęp; lub być może powinien być całkowicie zablokowany.

Administratorzy RemoteApp Azure umożliwia funkcji dostępnych za pośrednictwem Azure AD Premium ustawić zasady dostępu warunkowego dla jego środowiska Azure RemoteApp. Można też sformatowanego raportowania i alerty funkcje monitorowanie jak środowisko jest dostępny.

### <a name="how-to-set-up-conditional-access-for-azure-remoteapp"></a>Jak skonfigurować dostępu warunkowego dla Azure RemoteApp
Firma Microsoft zamiar szczegółową przykładowy scenariusz — RemoteApp Azure administrator chce zablokować dostęp do środowiska w przypadku użytkowników spoza sieci firmowej.

>[AZURE.NOTE] Przyjęto założenie, Azure AD została uaktualniona do poziomu Premium i że został utworzony co najmniej jeden zbiór Azure RemoteApp.

1.  Azure portal kliknij kartę **Usługi Active Directory** . Następnie kliknij pozycję katalogu, który chcesz skonfigurować.

    Pamiętaj: Dostępu warunkowego jest właściwością katalogu, a nie Azure RemoteApp, wszystkie konfiguracji odbywa się na poziomie katalogu. Oznacza to również, że musisz być administratorem katalogów, aby wprowadzić te zmiany.

2.  Kliknij pozycję **aplikacje**, a następnie kliknij **Microsoft Azure RemoteApp** konfigurowania dostępu warunkowego. Zauważ, że możesz skonfigurować dostępu warunkowego dla każdej aplikacji "oprogramowania jako usługa" w katalogu oddzielnie.
![Konfigurowanie dostępu warunkowego Azure RemoteApp](./media/remoteapp-secureaccess/ra-conditionalaccessscreen.png)
 

3.  Na karcie **Konfigurowanie** ustaw **Włącz reguły dostępu** włączone.
![Włącz reguły dostępu dla Azure RemoteApp](./media/remoteapp-secureaccess/ra-enableaccessrules.png)
 

4.  Teraz można skonfigurować różne reguły i wybierz osobę, której ma ich zastosowania w celu:

    1. Wybierz pozycję **zablokować dostęp, gdy nie pracujący** całkowicie uniemożliwić użytkownikom dostęp do Azure RemoteApp poza zadanej środowiska sieciowego.
    2. Kliknij opcję poniżej, aby zdefiniować zakresy adresów IP, które stanowią "zaufanych sieci". Wszystko poza te będą odrzucane.

5.  Przetestować konfigurację, uruchamiając klienta Azure RemoteApp z określonego adresu IP poza zakres, który wybrano. Po zalogowaniu się przy użyciu poświadczeń Azure AD powinien zostać wyświetlony następujący komunikat:

![Odmowa dostępu do Azure RemoteApp](./media/remoteapp-secureaccess/ra-accessdenied.png)
 

### <a name="future-conditional-access-features"></a>Funkcje dostępu warunkowego w przyszłości 
Nowe funkcje w programie Access warunkowe pracuje zespół usługi Azure Active Directory. Administratorzy będą mogli tworzyć nowe typy reguł znajdujących się poza siecią lokalizacji podstawie reguł. Podgląd publicznej nowe funkcje powinny być dostępne wkrótce.

### <a name="how-to-monitor-access-to-azure-remoteapp"></a>Jak można monitorować dostęp do Azure RemoteApp
Doskonałe funkcję, aby używać razem z pakietem dostępu warunkowego jest Azure Active Directory Premium funkcje raportowania. Można używać raportów do monitora, kto uzyskuje dostęp do środowiska i wykrywanie wszelkie podejrzane działania.

Na przykład widzisz nazwy użytkowników, którzy dostępne Azure RemoteApp, ile razy go jak i kiedy.

1.  W portalu Azure kliknij pozycję **Usługi Active Directory**, a następnie kliknij katalogu.

2.  Przejdź do karty **Raporty** .

3.  Na liście raportów zaznacz **użycie aplikacji** w obszarze **Zintegrowane aplikacje**.

    Zobaczysz niektóre zagregowane statystyki dla Azure RemoteApp. 
![Zagregowane statystykę dostępu Azure RemoteApp](./media/remoteapp-secureaccess/ra-accessstats.png)
 
5.  Kliknij aplikację, aby wyświetlić informacje o użytkownikach uzyskiwanie dostępu do Azure RemoteApp.
![Statystykę dostępu użytkownika dla Azure RemoteApp](./media/remoteapp-secureaccess/ra-userstats.png)
 
### <a name="summary"></a>Podsumowanie
Z Azure Active Directory Premium możesz skonfigurować reguły dostępu, aby Azure RemoteApp (i inne oprogramowanie jako aplikacje usług dostępne za pośrednictwem Azure AD). Reguły są obecnie ograniczone do zasad lokalizacji podstawie sieci, ale w przyszłości zostaną rozszerzone na inne aspekty zarządzania w przedsiębiorstwie.

Azure AD Premium oferuje, raportowanie i monitorowanie możliwości, jakie dalsze rozszerzanie kontrolki administrator ma w swoim środowisku Azure RemoteApp.

## <a name="how-do-i-make-sure-my-secure-resource-is-accessible-only-from-my-azure-remoteapp-environment"></a>Jak I upewnij się, że Moja bezpiecznego zasób jest dostępny tylko z moim środowiska Azure RemoteApp?
W poprzednich sekcjach tego artykułu, możemy ograniczony do zabezpieczania dostęp do środowiska Azure RemoteApp. Firma Microsoft zostały wykonane które, wybierając pozycję Użytkownicy, którzy mają dostęp i konfigurując reguły dostępu, aby dokładniej kontrolować sposób mogą używać usługi.

Typowy scenariusz w przypadku wdrożeń Azure RemoteApp jest zdalnego aplikacji potrzeby komunikować się z zasobem wewnętrznej, na przykład z bazą danych SQL. Ten zasób jest hostowana obsługiwanych lokalnie (na przykład w sieci firmowej) lub w chmurze (na przykład w Azure IaaS). Administratorzy chcą często upewnij się, że zasób wewnętrznej jest możliwy tylko przez aplikacje wdrażane za pomocą Azure RemoteApp, a nie na przykład przez aplikację z bezpośrednio na komputerze użytkownika i uzyskiwanie dostępu do publicznego Internetu. Azure RemoteApp często jest traktowany jako środowiska centralnie zarządzane i zabezpieczone i dlatego tylko ścieżkę za pomocą której użytkownicy interakcja z zasobem wewnętrznej.

Rozwiązaniem jest, aby umieścić środowiska Azure RemoteApp i bezpiecznego zasobu w tym samym Azure wirtualna sieć (VNET). W przypadku zasobu w innej witryny, możesz ustanowić połączenie VPN witryny do witryny, na przykład aby utworzyć VNet, obejmujące centrum danych Azure i lokalnego środowiska klienta.

Funkcja RemoteApp Azure obsługuje dwa typy wdrożeń zbioru, w której można wprowadzić własne VNET:

-   Osoby, które nie domeny — połączonych: aplikacje będą miały "linii wzroku" innych zasobów w VNET. Na przykład, to można łączyć aplikacje z bazą danych SQL używane uwierzytelnianie SQL (aplikacje uwierzytelnienia użytkownika bezpośrednio w bazie danych)

-   Sprzężone domeny: sprzężone maszyn wirtualnych używane przez Azure RemoteApp do kontrolera domeny w VNET. Jest to przydatne, gdy potrzebne do uwierzytelnienia kontrolera domeny systemu Windows, aby można było uzyskiwać dostęp do zasobu wewnętrznej.
![Kolekcja domeny w Azure RemoteApp](./media/remoteapp-secureaccess/ra-domainjoined.png)
 
### <a name="how-to-create-a-secure-connection-between-azure-and-my-on-premises-environment"></a>Jak utworzyć bezpiecznego połączenia między Azure i Moje lokalnego środowiska
Istnieje kilka opcji konfiguracji nawiązywania połączenia z środowiskach Azure i lokalnych. Omówienie opcji jest dostępna w tym miejscu.

Z Azure RemoteApp musisz najpierw skonfigurować usługi VNet, a następnie użyj go w trakcie procesu tworzenia zbioru. 

## <a name="the-complete-solution"></a>Kompleksowym rozwiązaniem
Na poniższym diagramie pokazano kompleksowym rozwiązaniem, w której firma Microsoft mają wbudowane kanału bezpiecznego dostępu z użytkownika końcowego, za pośrednictwem RemoteApp Azure (ARA), zasobów wewnętrznej bazy danych.
![Zabezpieczanie Azure RemoteApp](./media/remoteapp-secureaccess/ra-secureoverview.png) w etap 1 możemy wybranych użytkowników i tworzyć reguły dostępu, które określają, jak można uzyskać dostępu do ARA. W poniższym przykładzie możemy tylko Zezwalaj na dostęp dla użytkowników, rozpoczynając od sieci firmowej. Zgodny z innych niż użytkownicy nie będą mogli uzyskiwać dostęp do środowiska ARA wcale.
W "Etap 2" możemy mieć podsumowującymi zasobów wewnętrznej bazy danych tylko przy użyciu konfiguracji VNet/VPN, które sterować. Funkcja RemoteApp Azure został umieszczony w tym samym VNet. Wynik to, czy zasób jest możliwy tylko za pośrednictwem środowiska ARA.


