<properties
    pageTitle="Azure AD Connect synchronizacją: konfigurowania filtrowania | Microsoft Azure"
    description="Opisano sposób konfigurowania filtrowania Azure AD Connect synchronizacji."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="andkjell;markvi"/>


# <a name="azure-ad-connect-sync-configure-filtering"></a>Azure AD Connect synchronizacją: Konfigurowanie filtrowania
Za pomocą filtrowania, można kontrolować, które obiekty będą widoczne w Azure AD z katalogu lokalnego. Domyślna konfiguracja przejście wszystkich obiektów we wszystkich domenach w lasach skonfigurowane. Na ogół jest zalecana konfiguracja. Użytkownicy końcowi przy użyciu obciążeń pracą usługi Office 365, takich jak usługi Exchange Online i program Skype dla firm, korzystać z pełną globalnej listy adresowej, aby mogli wysyłać wiadomości e-mail i połączenie ze wszystkimi. O domyślnej konfiguracji jak samej obsługi, które jak ze stosowania lokalnego serwera Exchange lub programu Lync.

W niektórych przypadkach trzeba wprowadzić zmiany w konfiguracji domyślnej. Oto kilka przykładów:

- Zamierzasz użyć [wielu Azure AD katalogu topologii](active-directory-aadconnect-topologies.md#each-object-only-once-in-an-azure-ad-directory). Następnie należy zastosować filtr, aby kontrolować, które obiekty mają być synchronizowane do określonego katalogu Azure AD.
- Uruchamianie wdrożenia pilotażowego dla Azure lub usługi Office 365 i chcesz tylko podzbiór użytkowników w Azure AD. W małych pilotażowego nie jest istotne pełną globalnej listy adresowej w celu zademonstrowania funkcji.
- Masz wiele kont usług i innych kont-osobiste, które nie mają Azure AD.
- Dla zachowania zgodności nie powoduje usunięcia dowolnego użytkownika konta lokalnego. Możesz tylko je wyłączyć. Ale w Azure AD mają jedynie aktywne konta obecności.

W tym artykule opisano, jak skonfigurować różne metody filtrowania.

> [AZURE.IMPORTANT]Microsoft nie obsługuje zmiany lub operacji synchronizacji Azure AD Connect poza działaniami formalnego opisane. Wszystkie te akcje może spowodować niespójne lub nieobsługiwane stan synchronizacji Azure AD Connect i w wyniku firma Microsoft nie zapewnia pomocy technicznej związane z takimi wdrożeniami.

## <a name="basics-and-important-notes"></a>Podstawowe informacje i ważnych notatek
Narzędzie Azure AD Connect synchronizacji możesz włączyć filtrowania w dowolnym momencie. Jeśli początkowych domyślnej konfiguracji synchronizacji katalogów, a następnie konfigurowania filtrowania, obiekty, które są odfiltrowywane są już synchronizowane Azure AD. Aby uwzględnić tę zmianę dowolnych obiektów Azure AD, które już zostały zsynchronizowane, ale następnie zostały przefiltrowane są usuwane Azure AD.

Przed wprowadzeniem zmian do filtrowania, upewnij się, że [wyłączyć zaplanowane zadanie](#disable-scheduled-task) , nie przypadkowo Eksportowanie zmiany, które nie została jeszcze zweryfikowana są prawidłowe.

Ponieważ filtrowania można usunąć wiele obiektów w tym samym czasie, chcesz upewnij się, że nowe filtry są poprawne, przed rozpoczęciem eksportowania wszelkie zmiany do Azure AD. Po wykonaniu kroków konfiguracji, zaleca się przed eksportowanie i wprowadzanie zmian w Azure AD możesz wykonać [kroki weryfikacji](#apply-and-verify-changes) .

Aby zabezpieczyć się przed typowymi usuwanie wiele obiektów przypadkowo, funkcja [zapobiec przypadkowym usuwa](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) jest domyślnie włączona. Jeśli usuniesz wiele obiektów z powodu filtrowania (500 domyślnie), należy wykonaj czynności opisane w tym artykule, aby umożliwić usuwa obsłużone do Azure AD.

Użycie kompilacji przed 2015 listopada ([1.0.9125](active-directory-aadconnect-version-history.md#1091250)), wprowadzanie zmian w konfiguracji filtru i używanie synchronizacji haseł należy wyzwalanie pełnej synchronizacji wszystkich haseł po wykonaniu kroków konfiguracji. Instrukcje na temat uruchamiania hasła pełnej synchronizacji, zobacz [Wyzwalanie pełnej synchronizacji wszystkich haseł](active-directory-aadconnectsync-implement-password-synchronization.md#trigger-a-full-sync-of-all-passwords). Jeśli jesteś w 1.0.9125 lub nowszym, to akcji zwykła **pełnej synchronizacji** również oblicza Jeśli hasła mają być synchronizowane i tej dodatkowych czynności nie jest już potrzebne.

Jeśli obiektów **użytkowników** przypadkowo zostały usunięte Azure AD ze względu na błąd filtrowania, możesz ponownie utworzyć obiektów użytkowników w Azure AD, usuwając konfiguracji filtrowania i ponownie zsynchronizować z katalogów. Ta akcja przywraca użytkowników z Kosza w Azure AD. Jednak można cofnąć usunięcia innych obiektów. Na przykład jeśli przypadkowo Usuwanie grupy zabezpieczeń i został użyty do list ACL zasób, grupy i jej ACL nie można odzyskać.

Narzędzie Azure AD Connect usuwa tylko obiekty, które raz uwzględniono w zakresie. Jeśli ma Azure AD obiekty, które zostały utworzone przez inny aparat synchronizacji i tych obiektów nie są w zakresie, dodając filtrowania nie je usunąć. Na przykład rozpoczynać serwer DirSync i on utworzony pełną kopię całego katalogu Azure AD, zainstaluj nowy serwer Azure AD Connect synchronizacją równolegle z filtrowaniem włączonym od początku nie powoduje usunięcia dodatkowe obiekty utworzone przez usługę DirSync.

Konfiguracja filtrowania jest zachowywana po Instalowanie i uaktualnianie do nowszej wersji Azure AD Connect. Zawsze jest najlepszym rozwiązaniem zweryfikować, że konfiguracji nie został przypadkowo zmieniony po uaktualnieniu do nowszej wersji przed uruchomieniem Przechodzenie do pierwszej synchronizacji.

Jeśli masz więcej niż jeden las filtrowania konfiguracji opisanych w tym temacie musi zastosowane do każdy las (przy założeniu, że mają taką samą konfigurację dla wszystkich pozostałych).

### <a name="disable-scheduled-task"></a>Wyłącz zadanie zaplanowane
Aby wyłączyć harmonogram wbudowanych, powodujące cykl synchronizacji co 30 minut, wykonaj następujące czynności:

1. Przejdź do programu PowerShell monitu.
2. Uruchamianie `Set-ADSyncScheduler -SyncCycleEnabled $False` do wyłączony.
3. Wprowadź odpowiednie zmiany w sposób opisany w tym temacie.
4. Uruchamianie `Set-ADSyncScheduler -SyncCycleEnabled $True` Aby ponownie włączyć harmonogram.

**Jeśli korzystasz z kompilacji Azure AD Connect przed 1.1.105.0**  
Aby wyłączyć zaplanowane zadanie wyzwalające cykl synchronizacji co 3 godziny, wykonaj następujące czynności:

1. **Harmonogram zadań** można uruchomić z start menu.
2. Bezpośrednio w obszarze **Biblioteka Harmonogramu zadań**, Znajdź zadania o nazwie **Harmonogram Azure AD Sync**, kliknij prawym przyciskiem myszy i wybierz pozycję **Wyłącz**.  
![Harmonogram zadań](./media/active-directory-aadconnectsync-configure-filtering/taskscheduler.png)  
3. Możesz teraz wprowadzania zmian w konfiguracji i ręczne uruchamianie aparat synchronizacji z konsoli **Menedżer usługi synchronizacji** .

Po zakończeniu wszystkich zmian filtrowania nie zapomnij pochodzić Wstecz i **Włącz** zadanie ponownie.

## <a name="filtering-options"></a>Opcje filtrowania
Następujące typy konfiguracji filtrowania można stosować do narzędzia do synchronizacji katalogów:

- [**Grupa oparta**](active-directory-aadconnect-get-started-custom.md#sync-filtering-based-on-groups): filtrowanie według jednej grupy można skonfigurować tylko na początkowej instalacji przy użyciu Kreatora instalacji. Go jest nie dodatkowo omówione w tym temacie.

- [**Oparte na domenie**](#domain-based-filtering): Ta opcja umożliwia wybranie synchronizowanych do Azure AD domen. Umożliwia także dodawanie i usuwanie domen z konfiguracji aparatu synchronizacji, jeśli wprowadzisz zmiany w lokalnej infrastrukturze po zainstalowaniu Azure AD Connect synchronizacji.

- [**Oparte na jednostce organizacyjnej**](#organizational-unitbased-filtering): Ta opcja filtrowania umożliwia wybranie której organizacyjnych synchronizacją z usługą Azure AD. Ta opcja jest dla wszystkich typów obiekt w wybrane organizacyjnych.

- [**Oparte na atrybut**](#attribute-based-filtering): Ta opcja umożliwia filtrowanie obiektów według wartości atrybutu obiektów. Można także używać różnych filtrów dla typów innego obiektu.

Za pomocą wielu opcji filtrowania w tym samym czasie. Na przykład umożliwia oparte na jednostkę Organizacyjną filtrowania tylko umieszczać obiekty w jednej jednostce Organizacyjnej i tego samego czasu na atrybutach filtrowania do filtrowania obiekty. Jeśli używasz wielu metod filtrowania, filtrów za pomocą logicznego i między filtrów.

## <a name="domain-based-filtering"></a>Filtrowanie oparte na domenie
Ta sekcja zawiera czynności, aby skonfigurować filtr domeny. Jeśli zostały dodane lub usunięte domenach w sieci, po zainstalowaniu Azure AD Connect, masz aktualizacji konfiguracji filtrowania.

Preferowany sposób zmienić opartych na domenie filtrowania jest po uruchomieniu instalacji Kreator i zmień [domenę i organizacyjnych filtrowania](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering). Kreator instalacji jest automatyzacja wszystkie zadania opisane w niniejszym dokumencie.

Należy tylko wykonaj poniższe czynności w przypadku jakiejś przyczyny nie można uruchomić Kreatora instalacji.

Oparte na domenie konfiguracji filtrowania składa się z następujących czynności:

- [Wybrać domeny,](#select-domains-to-be-synchronized) który powinien zostać uwzględniony w synchronizacji.
- Dla każdej domeny dodane lub usunięte Dostosuj [Uruchamianie profilów](#update-run-profiles).
- [Zastosuj i sprawdź, czy zmiany](#apply-and-verify-changes).

### <a name="select-domains-to-be-synchronized"></a>Wybierz pozycję domeny, które mają być synchronizowane
**Aby ustawić filtr domeny, wykonaj następujące czynności:**

1. Zaloguj się do serwera, na którym jest uruchomiony Azure AD Connect synchronizacją przy użyciu konta, które jest członkiem grupy zabezpieczeń **ADSyncAdmins** .
2. Uruchom **Usługę synchronizacji** z start menu.
3. Zaznacz **Łączniki** i na liście **łączników** zaznacz łącznik z typem **Usług domenowych Active Directory**. Z **akcji**wybierz opcję **Właściwości**.  
![Właściwości łącznika](./media/active-directory-aadconnectsync-configure-filtering/connectorproperties.png)  
4. Kliknij przycisk **Konfiguruj partycje katalogu**.
5. Na liście **Wybierz partycje katalogu** zaznacz, a następnie usuń zaznaczenie domen, stosownie do potrzeb. Upewnij się, że zaznaczono tylko partycje, które chcesz zsynchronizować.  
![Partycje](./media/active-directory-aadconnectsync-configure-filtering/connectorpartitions.png)  
Jeśli zmienisz swoje lokalnie infrastruktury usługi Active Directory i domen dodane lub usunięte z las, a następnie kliknij przycisk **Odśwież** , aby pobrać zaktualizowaną listę. Po odświeżeniu, zostanie wyświetlony monit o poświadczenia. Poświadczenia dowolnego z dostępem do odczytu do usługi Active Directory w lokalnej. Nie ma być użytkownika, który jest wstępnie wypełnione w oknie dialogowym.  
![Wymagane odświeżenie](./media/active-directory-aadconnectsync-configure-filtering/refreshneeded.png)  
6. Gdy skończysz, zamknij okno dialogowe **Właściwości** , klikając **przycisk OK**. Po usunięciu domen z las, wyskakujące okienko komunikat informujący, że domena została usunięta i konfiguracji będzie można oczyścić.
7. Kontynuuj dostosować [Uruchamianie profilów](#update-run-profiles).

### <a name="update-run-profiles"></a>Aktualizowanie profili uruchamiania
Po zaktualizowaniu filtr domeny, musisz także zaktualizować profili uruchamiania.

1. Na liście **łączników** upewnij się, że jest wybrany łącznik zmienione w poprzednim kroku. **Akcje**zaznacz **Konfigurowanie profili uruchamiania**.  
![Łącznik uruchamianie profilów](./media/active-directory-aadconnectsync-configure-filtering/connectorrunprofiles1.png)  

Należy dopasować następujące profile:

- Pełne importowanie
- Pełnej synchronizacji
- Importowanie różnicy
- Zmiana synchronizacji
- Eksportowanie

Dla każdego z pięciu profile należy wykonać następujące czynności dla każdej domeny **dodane** :

1. Wybierz profil, uruchom, a następnie kliknij przycisk **Nowy krok**.
2. Na stronie **Konfigurowanie krok** w listy rozwijanej **Typ** , wybierz typ kroku o takiej samej nazwie jako profilu, którą konfigurujesz. Następnie kliknij przycisk **Dalej**.  
![Łącznik uruchamianie profilów](./media/active-directory-aadconnectsync-configure-filtering/runprofilesnewstep1.png)  
3. Na stronie **Konfiguracja łącznika** w **Partition** listę rozwijaną, wybierz nazwę domeny, dodanych do filtru domeny.  
![Łącznik uruchamianie profilów](./media/active-directory-aadconnectsync-configure-filtering/runprofilesnewstep2.png)  
4. Aby zamknąć okno dialogowe **Konfigurowanie profilu uruchamiania** , kliknij przycisk **Zakończ**.

Dla każdego z pięciu profile należy wykonać następujące czynności dla każdej domeny **usunięte** :

1. Wybierz profil, uruchom.
2. Jeśli **wartość** atrybutu **Partition** jest identyfikator GUID, wybierz krok, uruchamianie, a następnie kliknij przycisk **Usuń krok**.  
![Łącznik uruchamianie profilów](./media/active-directory-aadconnectsync-configure-filtering/runprofilesdeletestep.png)  

Wynik powinien być należy wymieniony każdej domeny, którą chcesz zsynchronizować krokiem w każdym uruchamianie profilu.

Aby zamknąć okno dialogowe **Konfigurowanie profili uruchamiania** , kliknij **przycisk OK**.

- Aby zakończyć konfigurację, [Zastosuj i sprawdź, czy zmiany](#apply-and-verify-changes).

## <a name="organizational-unitbased-filtering"></a>Oparte na jednostce organizacyjnej filtrowania
Preferowany sposób zmienić oparte na jednostkę Organizacyjną filtrowania jest po uruchomieniu instalacji kreatora i zmień [domenę i organizacyjnych filtrowania](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering). Kreator instalacji jest automatyzacja wszystkie zadania opisane w niniejszym dokumencie.

Należy tylko wykonaj poniższe czynności w przypadku jakiejś przyczyny nie można uruchomić Kreatora instalacji.

**Aby skonfigurować organizacji jednostek — oparte na filtrowanie, wykonaj następujące czynności:**

1. Zaloguj się do serwera, na którym jest uruchomiony Azure AD Connect synchronizacją przy użyciu konta, które jest członkiem grupy zabezpieczeń **ADSyncAdmins** .
2. Uruchom **Usługę synchronizacji** z start menu.
3. Zaznacz **Łączniki** i na liście **łączników** zaznacz łącznik z typem **Usług domenowych Active Directory**. Z **akcji**wybierz opcję **Właściwości**.  
![Właściwości łącznika](./media/active-directory-aadconnectsync-configure-filtering/connectorproperties.png)  
4. Kliknij pozycję **Konfigurowanie partycje katalogu**, zaznacz domenę, którą chcesz skonfigurować, a następnie kliknij **kontenerów**.
5. Po wyświetleniu monitu udostępnić poświadczeń dostęp do odczytu do usługi Active Directory w lokalnej. Nie ma być użytkownika, który jest wstępnie wypełnione w oknie dialogowym.
6. W oknie dialogowym **Wybieranie kontenerów** wyczyść organizacyjnych, które nie mają być synchronizowane z katalogu chmury, a następnie kliknij **przycisk OK**.  
![OU](./media/active-directory-aadconnectsync-configure-filtering/ou.png)  
  - Kontener **komputery** powinny być zaznaczone dla komputerów systemu Windows 10 do pomyślnie synchronizacji Azure AD. Jeśli dołączono domeny komputery znajdują się w innych jednostkach organizacyjnych, upewnij się, że są wybrane.
  - Kontener **ForeignSecurityPrincipals** powinny być zaznaczone, jeśli masz wiele lasów za pomocą relacji zaufania. Ten kontener umożliwia członkostwa w grupach zabezpieczeń krzyżowe las jest rozpoznawana.
  - **RegisteredDevices** OU powinny być zaznaczone, jeśli włączono funkcję zapisu urządzenia. Jeśli korzystasz z innej funkcji zapisu, takich jak zapisu grupy, upewnij się, że zaznaczono następujących lokalizacji.
  - Wybierz inne OU miejsce, w którym znajdują się użytkowników, obiektów iNetOrgPersons, grupy, kontakty i komputerach. Na obrazie all znajdują się w jednostce Organizacyjnej ManagedObjects.
7. Gdy skończysz, zamknij okno dialogowe **Właściwości** , klikając **przycisk OK**.
8. Aby zakończyć konfigurację, [Zastosuj i sprawdź, czy zmiany](#apply-and-verify-changes).

## <a name="attribute-based-filtering"></a>Na atrybutach filtrowania
Upewnij się, są 2015 listopada ([1.0.9125](active-directory-aadconnect-version-history.md#1091250)) lub nowszym tworzą te kroki, aby pracować.

Filtrowanie atrybut podstawie jest najbardziej elastyczna sposobem Filtrowanie obiektów. Możliwości [deklaracyjnych inicjowania obsługi administracyjnej](active-directory-aadconnectsync-understanding-declarative-provisioning.md) służy do sterowania niemal wszystkie aspekty, kiedy mają być synchronizowane obiektu do Azure AD.

Filtrowanie można stosować zarówno w [ruchu przychodzącego](#inbound-filtering) z usługi Active Directory do metaverse i [wychodzącego](#outbound-filtering) z metaverse do Azure AD. Zalecane jest stosowanie filtrowania w ruchu przychodzącego, ponieważ jest najłatwiej Obsługa. Filtrowania ruchu wychodzącego należy używać tylko, jeśli jest to wymagane, aby dołączyć do obiektów z więcej niż jeden las przed oszacowanie może być wykonywana.

### <a name="inbound-filtering"></a>Ruch przychodzący filtrowania
Ruch przychodzący podstawie filtrowania korzysta z domyślnej konfiguracji miejsce, w którym obiektów, przechodząc do Azure AD musi mieć cloudFiltered atrybut metaverse nie ustawiono wartość mają być synchronizowane. Jeśli ten atrybut wartość jest ustawiona na **wartość PRAWDA**, obiekt nie jest synchronizowane. Go należy nie można ustawić na **wartość False** zgodnie z projektem. Aby upewnić się, że innych reguł mogli współtworzyć wartość, ten atrybut tylko powinien mieć wartości **PRAWDA** lub **wartość NULL** (Brak).

W filtrowanie ruchu przychodzącego, można użyć do określenia, które obiekty należy lub nie mają być synchronizowane z możliwości **zakresu** . Jest to miejsce, w którym można dostosować do wymagań własnej organizacji. Moduł zakres ma **grupy** i **Klauzula** ustalenie, jeśli reguły synchronizacji powinien znajdować się w zakresie. **Grupa** zawiera jeden lub wiele **klauzuli**. Istnieje logicznego i między wiele klauzul i logiczne lub wiele grup.

Pozwól nam wyglądać na przykład:  
![Zakres](./media/active-directory-aadconnectsync-configure-filtering/scope.png) to odczytywane jako **(dział = IT) lub (dział = sprzedaży i c = US)**.

W próbki i poniższych instrukcji jako przykładu użyto obiekcie użytkownika, ale można to wszystkie typy obiektów.

W poniższych przykładach wartość pierwszeństwa rozpoczyna się 500. Ta wartość gwarantuje, że te reguły są obliczane po reguły w nowym polu (niższy priorytet, wyższą wartość liczbową).

#### <a name="negative-filtering-do-not-sync-these"></a>Wartości ujemne filtrowania "synchronizacji tych"
W poniższym przykładzie zostaną odfiltrowane (nie są synchronizowane) wszystkich użytkowników, którym **extensionAttribute15** ma wartość **NoSync**.

1. Zaloguj się do serwera, na którym jest uruchomiony Azure AD Connect synchronizacją przy użyciu konta, które jest członkiem grupy zabezpieczeń **ADSyncAdmins** .
2. Uruchom **Edytor reguł synchronizacji** z start menu.
3. Upewnij się, że **przychodzące** jest zaznaczone, a następnie kliknij przycisk **Dodaj nową regułę**.
4. Nadaj nazwę opisową reguły takie jak "*w z usługi Active Directory — DoNotSyncFilter użytkownika*". Wybierz poprawny las **użytkownika** jako **Typ obiektu CS**i **osoba** jako **Typ obiektu MV**. Jako **Typ łącza**wybierz pozycję **Dołącz do** i kolejności pierwszeństwa wpisz wartość nie są obecnie używane przez inną regułę synchronizacji (na przykład 500), a następnie kliknij przycisk **Dalej**.  
![Ruch przychodzący opis 1](./media/active-directory-aadconnectsync-configure-filtering/inbound1.png)  
5. W **Scoping filtr**kliknij pozycję **Dodaj grupę**, kliknij **Dodaj klauzulę**i wybierz atrybut **ExtensionAttribute15**. Upewnij się, że Operator jest ustawiona na **równe** i w polu wartość wpisz wartość **NoSync** . Kliknij przycisk **Dalej**.  
![Ruch przychodzący 2 zakres](./media/active-directory-aadconnectsync-configure-filtering/inbound2.png)  
6. Pozostaw puste **Dołączanie** reguły, a następnie kliknij przycisk **Dalej**.
7. Kliknij pozycję **Dodaj przekształcenie**, wybierz **dla przepływu** do **stałych**, wybierz atrybut docelowy **cloudFiltered** i w polu tekstowym źródło wpisz **wartość True**. Kliknij przycisk **Dodaj** , aby zapisać regułę.  
![Ruch przychodzący transformacja 3](./media/active-directory-aadconnectsync-configure-filtering/inbound3.png)
8. Aby zakończyć konfigurację, [Zastosuj i sprawdź, czy zmiany](#apply-and-verify-changes).

#### <a name="positive-filtering-only-sync-these"></a>Wynik dodatni filtrowania "tylko synchronizowanie tych"
Wyrażanie dodatnia filtrowanie może być bardziej trudne, ponieważ należy również wziąć pod uwagę obiektów, które nie są oczywiste do zsynchronizowania, takich jak sal konferencyjnych.

Dodatnia opcję filtrowania wymaga dwóch reguł synchronizacji. Jeden (lub kilka) z właściwy zakres obiektów do synchronizacji, a drugi synchronizacji wszystkich efektywnej reguły tego odfiltrowywanie wszystkich obiektów, które nie zostały jeszcze rozpoznane jako obiekt, które mają być synchronizowane.

W poniższym przykładzie tylko synchronizować obiektów użytkowników, których atrybut działu ma wartość **sprzedaży**.

1. Zaloguj się do serwera, na którym jest uruchomiony Azure AD Connect synchronizacją przy użyciu konta, które jest członkiem grupy zabezpieczeń **ADSyncAdmins** .
2. Uruchom **Edytor reguł synchronizacji** z start menu.
3. Upewnij się, że **przychodzące** jest zaznaczone, a następnie kliknij przycisk **Dodaj nową regułę**.
4. Nadawanie regule opisową nazwę, na przykład "*w z usługi Active Directory — sprzedaż użytkownika synchronizowanie*". Wybierz poprawny las **użytkownika** jako **Typ obiektu CS**i **osoba** jako **Typ obiektu MV**. Jako **Typ łącza**wybierz pozycję **Dołącz do** i kolejności pierwszeństwa wpisz wartość nie są obecnie używane przez inną regułę synchronizacji (na przykład 501), a następnie kliknij przycisk **Dalej**.  
![Ruch przychodzący opis 4](./media/active-directory-aadconnectsync-configure-filtering/inbound4.png)  
5. W **Scoping filtr**kliknij pozycję **Dodaj grupę**, kliknij pozycję **Dodaj klauzulę**i atrybut wybierz **działu**. Upewnij się, że Operator jest ustawiona na **równe** i w polu wartość wpisz wartość **Sprzedaż** . Kliknij przycisk **Dalej**.  
![Ruch przychodzący zakres 5](./media/active-directory-aadconnectsync-configure-filtering/inbound5.png)  
6. Pozostaw puste **Dołączanie** reguły, a następnie kliknij przycisk **Dalej**.
7. Kliknij pozycję **Dodaj przekształcenie**, wybierz **dla przepływu** do **stałych**, wybierz atrybut docelowy **cloudFiltered** i w polu tekstowym źródło wpisz **wartość False**. Kliknij przycisk **Dodaj** , aby zapisać regułę.  
![Ruch przychodzący transformacja 6](./media/active-directory-aadconnectsync-configure-filtering/inbound6.png)  
Jest to szczególny przypadek miejsce, w którym możesz ustawić cloudFiltered jawnie FAŁSZ.

    Mamy teraz Utwórz regułę synchronizacji wszystkich efektywnej.

8. Nadaj nazwę opisową reguły takie jak "*w z usługi Active Directory — filtr użytkownika efektywnej all*". Wybierz poprawny las **użytkownika** jako **Typ obiektu CS**i **osoba** jako **Typ obiektu MV**. Jako **Typ łącza**wybierz pozycję **Dołącz do** i kolejności pierwszeństwa wpisz wartość nie są obecnie używane przez inną regułę synchronizacji (na przykład 600). Masz zaznaczony pierwszeństwa wartość wyższa (niższy priorytet) niż poprzedniego reguły synchronizacji, ale również od lewej niektórych pokoju, więc dodamy dalszych reguł filtrowania synchronizacji później umożliwia Rozpocznij synchronizowanie dodatkowe działy. Kliknij przycisk **Dalej**.  
![Ruch przychodzący opis 7](./media/active-directory-aadconnectsync-configure-filtering/inbound7.png)  
9. Pozostaw puste **Scoping filtr** , a następnie kliknij przycisk **Dalej**. Pusty filtr wskazuje, że zasady powinny być stosowane do wszystkich obiektów.
10. Pozostaw puste **Dołączanie** reguły, a następnie kliknij przycisk **Dalej**.
11. Kliknij pozycję **Dodaj przekształcenie**, wybierz **dla przepływu** do **stałych**, wybierz atrybut docelowy **cloudFiltered** i w polu tekstowym źródło wpisz **wartość True**. Kliknij przycisk **Dodaj** , aby zapisać regułę.  
![Ruch przychodzący transformacja 3](./media/active-directory-aadconnectsync-configure-filtering/inbound3.png)  
12. Aby zakończyć konfigurację, [Zastosuj i sprawdź, czy zmiany](#apply-and-verify-changes).

W razie potrzeby można utworzyć więcej reguł typu pierwsze miejsce, w którym możesz umieścić więcej obiektów w naszym synchronizacji.

### <a name="outbound-filtering"></a>Filtrowanie ruchu wychodzącego
W niektórych przypadkach należy filtrowanie tylko wtedy, gdy obiekty dołączyli w metaverse. Na przykład może być wymagane Aby przyjrzeć się atrybut poczty z las zasobów i atrybut userPrincipalName z las konta do określenia, jeśli obiekt mają być synchronizowane. W takim przypadku możesz utworzyć wyszukującego reguły wychodzącej.

W tym przykładzie Zmienianie filtrowania, dlatego tylko użytkownicy miejsce, w którym oba poczty i userPrincipalName kończy się @contoso.com są synchronizowane:

1. Zaloguj się do serwera, na którym jest uruchomiony Azure AD Connect synchronizacją przy użyciu konta, które jest członkiem grupy zabezpieczeń **ADSyncAdmins** .
2. Uruchom **Edytor reguł synchronizacji** z start menu.
3. W obszarze **Typ reguły**kliknij pozycję **wychodzące**.
4. Znajdź regułę o nazwie **zewnątrz do AAD — SOAInAD Dołączanie użytkownika**. Kliknij przycisk **Edytuj**.
5. W oknie podręcznym Odpowiedz **Tak,** Aby utworzyć kopię reguły.
6. Na stronie **Opis** zmienić pierwszeństwem nieużywanymi wartość, na przykład 50.
7. Kliknij przycisk **Filtr Scoping** w obszarze nawigacji po lewej stronie. Kliknij pozycję **Dodaj klauzulę**, wybierz atrybut **poczty**, wybierz Operator **ENDSWITH**i typ wartości **@contoso.com**. Kliknij pozycję **Dodaj klauzulę**, wybierz atrybut **userPrincipalName**, wybierz Operator **ENDSWITH**i typ wartości **@contoso.com**.
8. Kliknij przycisk **Zapisz**.
9. Aby zakończyć konfigurację, [Zastosuj i sprawdź, czy zmiany](#apply-and-verify-changes).

## <a name="apply-and-verify-changes"></a>Stosowanie i sprawdź, czy zmiany
Po wprowadzeniu zmian w konfiguracji muszą one stosowane do obiektów już istnieje w systemie. Możliwe także, że powinny być przetwarzane obiektów nie są obecnie w aparat synchronizacji i aparat synchronizacji należy przeczytać w źródłowym systemie ponownie, aby sprawdzić zawartość.

Zmiana konfiguracji przy użyciu **domeny** lub **Jednostka organizacyjna** filtrowania, należy wykonać **pełne importowanie** następuje **Zmiana synchronizacji**.

Jeśli zmienisz konfiguracji przy użyciu **atrybut** filtrowania, należy wykonać **pełnej synchronizacji**.

Wykonaj następujące kroki:

1. Uruchom **Usługę synchronizacji** z start menu.
2. Zaznacz **Łączniki** i na liście **łączników** zaznacz łącznik miejsce, w którym możesz wprowadził zmianę wcześniej konfiguracji. Z **akcji**wybierz polecenie **Uruchom**.  
![Uruchamianie łącznika](./media/active-directory-aadconnectsync-configure-filtering/connectorrun.png)  
3. W polu **Uruchom profile**wybierz operację wymienionych w poprzedniej sekcji. Jeśli jest potrzebne do uruchamiania dwóch działań, uruchom drugi po zakończeniu pierwszego (kolumnie **Stan** jest **bezczynny** wybranego łącznika).

Po synchronizacji wszystkie zmiany są umieszczane do wyeksportowania. Przed faktycznie wprowadź odpowiednie zmiany w Azure AD, który chcesz sprawdzić, czy te zmiany są poprawne.

1. Uruchom wiersz cmd i przejdź do`%Program Files%\Microsoft Azure AD Sync\bin`
2. Uruchom:`csexport "Name of Connector" %temp%\export.xml /f:x`  
Nazwa łącznika można znaleźć w usługi synchronizacji. Ma nazwę podobną do "contoso.com — AAD" dla Azure AD.
3. Uruchom:`CSExportAnalyzer %temp%\export.xml > %temp%\export.csv`
4. Teraz masz pliku w % temp % o nazwie export.csv, który może sprawdzić w programie Microsoft Excel. Ten plik zawiera wszystkie zmiany, które mają być eksportowane.
5. Wprowadź niezbędne zmiany w danych lub konfiguracji i uruchom poniższe czynności ponownie (Importuj, Synchronizuj i sprawdź) do momentu zmiany, które mają być eksportowane są poprawnie.

Po zakończeniu, wyeksportować zmiany do Azure AD.

1. Zaznacz **Łączniki** i na liście **łączników** zaznacz łącznik Azure AD. Z **akcji**wybierz polecenie **Uruchom**.
2. W oknie **Uruchamianie profilów**zaznacz opcję **Eksportuj**.
3. Jeśli zmiany w konfiguracji usunąć wiele obiektów, następnie zobaczysz komunikat o błędzie na eksportowania gdy liczba jest większa niż skonfigurowana wartość progowa (domyślnie 500). Jeśli zostanie wyświetlony ten błąd, należy tymczasowo wyłączyć funkcję [Zapobiegaj przypadkowym usunięciom](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md).

Nadszedł czas, aby ponownie włączyć harmonogram.

1. **Harmonogram zadań** można uruchomić z start menu.
2. Bezpośrednio w obszarze **Biblioteka Harmonogramu zadań**, Znajdź zadania o nazwie **Harmonogram Azure AD Sync**, kliknij prawym przyciskiem myszy i wybierz pozycję **Włącz**.

## <a name="next-steps"></a>Następne kroki
Dowiedz się, jak [zsynchronizować Azure AD Connect](active-directory-aadconnectsync-whatis.md) konfiguracji.

Dowiedz się więcej o [integrowaniu lokalnego tożsamości z usługą Azure Active Directory](active-directory-aadconnect.md).
