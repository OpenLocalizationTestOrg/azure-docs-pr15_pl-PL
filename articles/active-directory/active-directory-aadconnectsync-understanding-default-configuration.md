<properties
    pageTitle="Azure AD Connect synchronizacją: opis konfiguracji domyślne | Microsoft Azure"
    description="Ten artykuł zawiera opis konfiguracji domyślnej Azure AD Connect synchronizacji."
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
    ms.date="09/01/2016"
    ms.author="billmath"/>

# <a name="azure-ad-connect-sync-understanding-the-default-configuration"></a>Azure AD Connect synchronizacją: opis konfiguracji domyślnej
W tym artykule opisano reguły konfiguracji w nowym polu. Dokumenty go reguł i wpływ tych reguł konfiguracji. On również przeprowadzi Cię przez domyślnej konfiguracji synchronizacji Azure AD Connect. Celem jest czytnik rozumie, jak model konfiguracji o nazwie deklaracyjnych inicjowania obsługi administracyjnej, pracuje w rzeczywisty przykład. W tym artykule założono, że został już zainstalowany i Konfigurowanie synchronizacji Azure AD Connect przy użyciu Kreatora instalacji.

Aby uzyskać szczegółowe informacje o modelu konfiguracji, przeczytaj [Opis deklaracyjnych-inicjowania obsługi administracyjnej](active-directory-aadconnectsync-understanding-declarative-provisioning.md).

## <a name="out-of-box-rules-from-on-premises-to-azure-ad"></a>W nowym polu reguł ze źródeł lokalnych Azure AD
Poniższe wyrażenia znajdują się w konfiguracji w nowym polu.

### <a name="user-out-of-box-rules"></a>Reguły z gotowych do użytkownika
Te zasady dotyczą również typ obiektu iNetOrgPerson.

Obiekt użytkownika musi spełniać następujące czynności w celu zsynchronizowania:

- Musi być sourceAnchor.
- Po utworzeniu obiektu w Azure AD sourceAnchor nie można zmienić. Jeśli wartość jest zmienione lokalnego, obiekt przestaje synchronizacji, dopóki nie zostanie zmieniony sourceAnchor, powrót do poprzedniej wartości.
- Musi mieć atrybut accountEnabled (kontroli konta użytkownika) wypełnione. Z lokalnej usługi Active Directory ten atrybut jest zawsze obecne i wypełnione.

Następujące obiekty użytkownika są **nie** jest synchronizowany Azure AD:

- `IsPresent([isCriticalSystemObject])`. Upewnij się, wiele obiektów w nowym polu w usłudze Active Directory, takich jak wbudowane konto administratora, nie są synchronizowane.
- `IsPresent([sAMAccountName]) = False`. Upewnij się, że obiektów użytkowników nie atrybut sAMAccountName nie są synchronizowane. Tym przypadku tylko praktycznie może wystąpić w domenie uaktualniony z NT4.
- `Left([sAMAccountName], 4) = "AAD_"`, `Left([sAMAccountName], 5) = "MSOL_"`. Nie są synchronizowane konta usługi używanego przez narzędzie Azure AD Connect synchronizacji i jego wcześniejszych wersji.
- Nie są synchronizowane konta programu Exchange, które nie będzie działać w usłudze Exchange Online.
    - `[sAMAccountName] = "SUPPORT_388945a0"`
    - `Left([mailNickname], 14) = "SystemMailbox{"`
    - `(Left([mailNickname], 4) = "CAS_" && (InStr([mailNickname], "}") > 0))`
    - `(Left([sAMAccountName], 4) = "CAS_" && (InStr([sAMAccountName], "}")> 0))`
- Nie są synchronizowane obiektów, które nie będzie działać w usłudze Exchange Online.
`CBool(IIF(IsPresent([msExchRecipientTypeDetails]),BitAnd([msExchRecipientTypeDetails],&H21C07000) > 0,NULL))`  
Ten maski (i H21C07000) czy odfiltrowywania następujących obiektów:
    - Folder publiczny z włączoną obsługą poczty
    - System Attendant skrzynki pocztowej
    - Skrzynka pocztowa bazy danych skrzynki pocztowej (Skrzynka pocztowa System)
    - Grup zabezpieczeń (nie stosowanie dla użytkownika, ale istnieje dla starszych powodów)
    - Grupa uniwersalna nie (nie stosowanie dla użytkownika, ale istnieje dla starszych powodów)
    - Plan skrzynki pocztowej
    - Skrzynki pocztowej odnajdowania
- `CBool(InStr(DNComponent(CRef([dn]),1),"\\0ACNF:")>0)`. Obiekty ofiarą replikacja nie są synchronizowane.

Stosowane następujące reguły atrybut:

- `sourceAnchor <- IIF([msExchRecipientTypeDetails]=2,NULL,..)`. Atrybut sourceAnchor nie jest zamieszczanie z połączonych skrzynki pocztowej. Przyjmuje się, że jeśli znaleziono skrzynki pocztowej połączonego konta rzeczywista jest z nim połączony później.
- Atrybuty są synchronizowane tylko, jeśli wartość atrybutu **mailNickName** związane z programu Exchange.
- W przypadku wielu lasów atrybuty są używane w następującej kolejności:
    1. Atrybuty związanych z logowaniem (na przykład userPrincipalName) są tworzone z las z włączonym kontem.
    2. Atrybuty, które można znaleźć w GAL programu Exchange (globalna lista adresowa) są tworzone z las ze skrzynką pocztową programu Exchange.
    3. Jeśli nie skrzynki pocztowej można znaleźć, następujące atrybuty mogą pochodzić z dowolnej las.
    4. Exchange związane z atrybutów (nie są widoczne na globalnej liście adresowej techniczne atrybuty) są tworzone z las miejsce, w którym `mailNickname ISNOTNULL`.
    5. W przypadku wielu lasów, które spełniają jednej z tych reguł kolejność tworzenia (Data/godzina) łączników (lasów) jest używana do określenia, które las składa się atrybuty.

### <a name="contact-out-of-box-rules"></a>Skontaktuj się z gotowych do reguł
Obiekt kontaktu musi spełniać następujące czynności w celu zsynchronizowania:

- Kontakt musi być włączona poczta. Sprawdza z następującymi regułami:
    - `IsPresent([proxyAddresses]) = True)`. Można wypełnić atrybutu proxyAddresses.
    - Podstawowy adres e-mail można znaleźć w atrybucie proxyAddresses lub atrybut poczty. Obecność @ jest używany do weryfikowania, że ich zawartość jest adres e-mail. Jedną z tych dwóch reguł należy przyjąć wartość PRAWDA.
        - `(Contains([proxyAddresses], "SMTP:") > 0) && (InStr(Item([proxyAddresses], Contains([proxyAddresses], "SMTP:")), "@") > 0))`. Czy istnieje wpis z "SMTP:", a jeśli istnieje, mogą @ można znaleźć w ciągu?
        - `(IsPresent([mail]) = True && (InStr([mail], "@") > 0)`. Jest atrybut poczty wypełnione oraz jeśli jest to, czy @ można znaleźć w ciągu?

Następujące obiekty kontaktów są **nie** jest synchronizowany Azure AD:

- `IsPresent([isCriticalSystemObject])`. Upewnij się, nie kontaktom oznaczone jako krytyczne są synchronizowane. Nie powinny być dowolna z konfiguracją domyślną.
- `((InStr([displayName], "(MSOL)") > 0) && (CBool([msExchHideFromAddressLists])))`.
- `(Left([mailNickname], 4) = "CAS_" && (InStr([mailNickname], "}") > 0))`. Tych obiektów nie działają w usłudze Exchange Online.
- `CBool(InStr(DNComponent(CRef([dn]),1),"\\0ACNF:")>0)`. Obiekty ofiarą replikacja nie są synchronizowane.

### <a name="group-out-of-box-rules"></a>Grupowanie w nowym polu reguł
Obiektu grupy musi spełniać następujące czynności w celu zsynchronizowania:

- Musi być mniejsza niż 50 000 członków. Liczba ta jest liczba członków w grupie lokalnej.
    - Jeśli ma więcej członków przed synchronizacją zostanie uruchomiony po raz pierwszy, grupy nie jest synchronizowane.
    - Jeśli Zwiększ liczbę członków z gdy został utworzony, następnie po osiągnięciu 50 000 członków zatrzymania synchronizacji, dopóki ponownie statystykę członkostwo jest mniejsza niż 50 000.
    - Uwaga: Liczba 50 000 członkostwo jest również nałożonych przez Azure AD. Nie jest możliwe do synchronizacji z więcej członków grupy, nawet jeśli modyfikowanie lub usuwanie tej reguły.
- Jeśli grupa jest **Grupą dystrybucyjną**, również go muszą być włączona poczta. Zobacz [kontakt z gotowych do reguły](#contact-out-of-box-rules) dla tej reguły są wymuszane.

Są następujące Grupuj obiekty **nie** jest synchronizowany Azure AD:

- `IsPresent([isCriticalSystemObject])`. Upewnij się, wiele obiektów w nowym polu w usłudze Active Directory, takich jak wbudowanej grupy Administratorzy, nie są synchronizowane.
- `[sAMAccountName] = "MSOL_AD_Sync_RichCoexistence"`. Grupa starszych używane przez usługę DirSync.
- `BitAnd([msExchRecipientTypeDetails],&amp;H40000000)`. Grupy ról.
- `CBool(InStr(DNComponent(CRef([dn]),1),"\\0ACNF:")>0)`. Obiekty ofiarą replikacja nie są synchronizowane.

### <a name="foreignsecurityprincipal-out-of-box-rules"></a>Reguły w nowym polu ForeignSecurityPrincipal
Sprzężone FSP do"" (\*) obiektu w metaverse. W rzeczywistości tego sprzężenia występuje tylko dla użytkowników i grup zabezpieczeń. Ta konfiguracja zapewnia, że członkostwa las krzyżowe są rozpoznawania i poprawnie przedstawione w Azure AD.

### <a name="computer-out-of-box-rules"></a>Komputer z gotowych do reguł
Obiekt komputera musi spełniać następujące czynności w celu zsynchronizowania:

- `userCertificate ISNOTNULL`. Komputery systemu Windows 10 Wypełnij ten atrybut. Wszystkie obiekty komputera z wartością w tym atrybucie są synchronizowane.

## <a name="understanding-the-out-of-box-rules-scenario"></a>Opis tego scenariusza z gotowych do reguł
W tym przykładzie użyto wdrażania z jednego konta las (A), jeden las zasobów (R) i jednego katalogu Azure AD.

![Obraz z opisem scenariusz](./media/active-directory-aadconnectsync-understanding-default-configuration/scenario.png)

W tej konfiguracji zakłada się, że jest włączone konto w las konta i wyłączone konto w las zasobów z połączonych skrzynką pocztową.

Naszej sieci z domyślną konfigurację jest:

- Atrybuty związane z logowaniem są synchronizowane z las z włączonym kontem.
- Atrybuty, które znajdują się na globalnej liście adresowej (globalna lista adresowa) są synchronizowane z las ze skrzynki pocztowej. Jeśli nie skrzynki pocztowej można znaleźć, innych las jest używana.
- Jeśli zostanie znaleziony połączonych skrzynki pocztowej, można znaleźć połączonego konta włączono obiekt do wyeksportowania do Azure AD.

### <a name="synchronization-rule-editor"></a>Edytor reguł synchronizacji
Konfiguracja można było wyświetlać i modyfikować za pomocą narzędzia Edytor reguł synchronizacji (SRE) i skrót do niego znajdują się w start menu.

![Edytor reguł ikony](./media/active-directory-aadconnectsync-understanding-default-configuration/sre.png)

SRE to narzędzie zestawu zasobów i jest instalowany z synchronizacji Azure AD Connect. Aby można było go uruchomić, musi być członkiem grupy ADSyncAdmins. Podczas uruchamiania, zobacz mniej więcej tak:

![Reguły synchronizacji ruch przychodzący](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulesinbound.png)

W tym okienku zobaczysz wszystkie reguły synchronizacji utworzone dla danej konfiguracji. Każdy wiersz w tabeli jest jedna reguła synchronizacji. Po lewej stronie w obszarze typy reguł, znajdują się dwa różne typy: przychodzących i wychodzących. Ruch przychodzący i wychodzący jest w widoku metaverse. Przede wszystkim będą fokusu na reguły przychodzące w tym omówienie. Rzeczywista lista reguł synchronizacji zależy od schematu wykryto w AD. Na powyższym obrazie las konta (fabrikamonline.com) nie ma jakichkolwiek usług, takich jak Exchange i programu Lync, a żadne reguły synchronizacji zostały utworzone dla tych usług. Jednak w las zasobów (res.fabrikamonline.com) możesz znaleźć reguły synchronizacji dla tych usług. Zawartość reguł różni się w zależności od wersji wykryte. Na przykład we wdrożeniu z programem Exchange 2013 istnieje więcej przepływów atrybut skonfigurowane niż w programie Exchange 2010 i 2007.

### <a name="synchronization-rule"></a>Reguła synchronizacji
Reguła synchronizacji jest obiekt konfiguracji przy użyciu zestawu atrybutów ułożony, gdy warunek jest spełniony. Służy także do opisano, jak obiekt w obszarze łącznik jest powiązana z obiektu w metaverse znana pod nazwą **join** lub **zgodne**. Reguły synchronizacji mają wartość pierwszeństwa wskazująca, jak są powiązane ze sobą. Reguła synchronizacji z wartością liczbową dolnym ma pierwszeństwo, a konflikt przepływu atrybut pierwszeństwo wins Rozwiązywanie konfliktów.

Na przykład przeglądać reguły synchronizacji **w z usługi Active Directory — AccountEnabled użytkownika**. Zaznacz ten wiersz w SRE i kliknij przycisk **Edytuj**.

Ponieważ ta reguła jest reguły w nowym polu, pojawi się ostrzeżenie podczas otwierania reguły. Nie należy wszelkie [zmiany wprowadzone w regułach w nowym polu](active-directory-aadconnectsync-best-practices-changing-default-configuration.md), zostanie wyświetlony monit, co to są zamiaru. W tym przypadku tylko chcesz wyświetlić regułę. Wybierz opcję **nie**.

![Synchronizacja zasady ostrzeżenia](./media/active-directory-aadconnectsync-understanding-default-configuration/warningeditrule.png)

Reguła synchronizacji ma czterema sekcjami konfiguracji: opis, zakresy, filtrowanie, Dołącz reguł i przekształcenia.

#### <a name="description"></a>Opis
Pierwsza sekcja zawiera podstawowe informacje, takie jak nazwa i opis.

![Opis Karta Edytor reguł synchronizacji ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncruledescription.png)

Można również znaleźć informacje o który połączonego system dotyczące tej reguły, które typ systemu połączonego it ma zastosowanie do i typ obiektu metaverse obiektów. Typ obiektu metaverse jest zawsze osoby bez względu na to, w przypadku typ obiektu źródłowego użytkownika, iNetOrgPerson lub kontakt. Typ obiektu metaverse nigdy nie należy zmienić, aby została utworzona jako typ rodzajowy. Typ łącza można ustawić sprzężenia, StickyJoin lub przepisu. To ustawienie współdziała z sekcji dołączanie reguły i jest objęta później.

Można też wyświetlić, że ta reguła synchronizacji jest używany do synchronizacji haseł. Jeśli użytkownik jest w zakresie dla tej reguły synchronizacji, hasło jest synchronizowane ze źródeł lokalnych w chmurze (przy założeniu, że została włączona funkcja synchronizacji hasła).

#### <a name="scoping-filter"></a>Filtr zakresu
W sekcji Filtr zakresu jest używany do konfigurowania, kiedy ma być stosowane reguły synchronizacji. Ponieważ nazwy reguły synchronizacji patrzy się wskazuje powinny być stosowane tylko dla użytkowników włączone, zakres skonfigurowano tak atrybut AD **kontroli konta użytkownika** nie może mieć bit 2 Ustaw. Gdy aparat synchronizacji znajdzie użytkownika w AD, dotyczy ta reguła synchronizacji podczas **kontroli konta użytkownika** jest ustawiona na wartość dziesiętną 512 (włączone użytkownika normalny). Nie dotyczy reguła, gdy użytkownik ma **kontroli konta użytkownika** , ustaw 514 (wyłączone użytkownika normalny).

![Określanie zakresu karty w edytorze reguły synchronizacji ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulescopingfilter.png)

Filtr zakresu zawiera grupy i klauzule, które mogą być zagnieżdżone. Dla reguły synchronizacji zastosować muszą być spełnione wszystkie warunki w grupie. Po zdefiniowaniu wiele grup co najmniej jedną grupę, muszą być spełnione dla zastosować regułę. Oznacza to, że logiczne lub jest obliczana między grupami i logicznych i jest obliczana w grupie. Przykład tej konfiguracji można znaleźć w ruchu wychodzącego reguły synchronizacja **zewnątrz do AAD — dołączanie do grupy**. Istnieje kilka grup filtr synchronizacji, na przykład jedną dla grup zabezpieczeń (`securityEnabled EQUAL True`), a drugi do grup dystrybucyjnych (`securityEnabled EQUAL False`).

![Określanie zakresu karty w edytorze reguły synchronizacji ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulescopingfilterout.png)

Ta reguła służy do Definiowanie grupy, które powinny być tak, aby Azure AD. Grup dystrybucyjnych musi być włączone do synchronizacji z usługą Azure Active Directory poczty, ale dla grup zabezpieczeń wiadomości e-mail nie jest wymagane.

#### <a name="join-rules"></a>Dołączanie do reguł
Trzecia sekcja służy do konfigurowania obiektów w obszarze łącznik się relacjom między obiektami w metaverse. Reguła sprawdzono wcześniej nie ma więc zamiast tego zamierzasz przeglądać dowolne konfiguracji dołączanie reguł **w z usługi Active Directory — dołączanie do użytkownika**.

![Dołączanie do karty reguły w edytorze reguły synchronizacji ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulejoinrules.png)

Zawartość reguły sprzężenie zależy od pasujące opcji w Kreatorze instalacji. Dla reguły przychodzące oceny zaczyna się od obiektu w przestrzeni łącznik źródła i każdej grupy w regułach sprzężenia jest obliczana w sekwencji. Jeśli obiekt źródłowy jest obliczane zgodnie z dokładnie jeden obiekt w metaverse przy użyciu jednej z reguł sprzężenia, obiekty są połączone. Jeśli zostały wszystkie reguły, a nie, jest używany typ łącza na stronie opis. Jeśli ta konfiguracja jest ustawiona na **świadczenia**, nowy obiekt zostanie utworzona w celu metaverse. Aby zainicjować obsługę nowy obiekt metaverse jest nazywany do **projektu** obiektu metaverse.

Reguły sprzężenia są szacowane jedynie raz. Sprzężone obiektu miejsca łącznik i obiektu metaverse pozostaną sprzężonych pod warunkiem zakres reguły synchronizacji będzie nadal spełnione.

Podczas obliczania reguły synchronizacji, tylko jedna reguła synchronizacja z regułami sprzężenia zdefiniowane musi być w zakresie. Jeśli wiele reguł synchronizacji z regułami sprzężenia znajdują się dla jednego obiektu, jest generowany błąd. Z tego powodu najlepszym rozwiązaniem jest mają tylko jedna reguła synchronizacji z sprzężenia zdefiniowane w przypadku wielu reguł synchronizacji w zakresie obiektu. W konfiguracji z gotowych do synchronizacji Azure AD Connect tych reguł można można znaleźć, wyświetlając w nazwie i Znajdź zawierających wyraz **Dołączanie** na końcu nazwy. Reguły synchronizacji bez reguł sprzężenia zdefiniowane dotyczy przepływów atrybut inna reguła synchronizacji obiekty połączone i obsługi administracyjnej nowy obiekt w docelowej.

Podczas przeglądania na powyższym obrazie widać, że reguły próbuje dołączyć **objectSID** z **msExchMasterAccountSid** (Exchange) i **msRTCSIP OriginatorSid** (Lync), czyli Oczekujemy w topologii las konta zasobów. Możesz znaleźć tej samej reguły na wszystkich lasy. Zakłada się, że każdy las może być las konto lub zasobu. Ta konfiguracja działa również wtedy, gdy konta, które są przechowywane w jeden las i nie mają być połączone.

#### <a name="transformations"></a>Przekształcenia
Sekcja transformacja definiuje wszystkie przepływy atrybutów, które dotyczą obiekt docelowy, gdy obiekty są połączone i Filtr zakresu jest spełniony. Przechodzenie wstecz do **w z usługi Active Directory — AccountEnabled użytkownika** regułę synchronizacji, możesz znaleźć przekształcenia następujące czynności:

![Przekształcenia Karta Edytor reguł synchronizacji ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncruletransformations.png)

Umieść tę konfigurację w kontekście wdrożeniem las konta zasobu jest planowane znalezienie włączonym kontem w las konta i wyłączone konto las zasobów przy użyciu ustawień programu Exchange i programu Lync. Reguła synchronizacji patrzy się zawiera atrybuty wymagane do logowania i następujące atrybuty należy przepływ od las przypadku włączonym kontem. Wszystkie przepływy atrybut łączone są w jedną regułę synchronizacji.

Przekształcenie może zawierać różne typy: stałą, bezpośrednich i wyrażenia.

- Stały przepływ przepływa zawsze wartość ustalony. W przypadku powyżej, zawsze ustawia wartość **True** metaverse atrybut o nazwie **accountEnabled**.
- Bezpośrednie przepływu przepływał zawsze wartość atrybutu w źródle atrybutu docelowej jako-jest.
- Trzeci typ przepływu jest wyrażenie i umożliwia bardziej zaawansowane konfiguracji.

Język wyrażeń jest VBA (Visual Basic for Applications), dlatego osobom mającym obsługi pakietu Microsoft Office lub VBScript rozpozna format. Atrybuty są ujęte w nawiasy kwadratowe [Nazwa_atrybutu]. Atrybut nazw i nazw funkcji jest uwzględniana wielkość liter, ale edytor reguł synchronizacji wynikiem wyrażenia i podaj ostrzeżenie, jeśli wyrażenie nie jest prawidłowe. Wszystkie wyrażenia są wyrażone w jednym wierszu z zagnieżdżonych funkcji. Aby wyświetlić możliwości języka konfiguracji, Oto przepływu pwdLastSet, ale z dodać kolejne komentarze wstawione:

```
// If-then-else
IIF(
// (The evaluation for IIF) Is the attribute pwdLastSet present in AD?
IsPresent([pwdLastSet]),
// (The True part of IIF) If it is, then from right to left, convert the AD time format to a .Net datetime, change it to the time format used by Azure AD, and finally convert it to a string.
CStr(FormatDateTime(DateFromNum([pwdLastSet]),"yyyyMMddHHmmss.0Z")),
// (The False part of IIF) Nothing to contribute
NULL
)
```

Aby uzyskać więcej informacji na języku wyrażeń dla atrybutu przepływów, zobacz [Opis deklaracyjnych inicjowania obsługi administracyjnej wyrażeń](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) .

### <a name="precedence"></a>Hierarchia ważności
Teraz sprawdzono niektórych poszczególnych reguł synchronizacji, ale reguł wspólna praca w konfiguracji. W niektórych przypadkach wartość atrybutu jest przyczynić się z wielu reguł synchronizacji do samego atrybutu docelowego. W tym przypadku pierwszeństwo atrybutów służy do określenia, jakie atrybuty wins. Na przykład Spójrz na sourceAnchor atrybut. Ten atrybut jest atrybutem ważne, aby można było zalogować się do Azure AD. Znajdują się przepływu atrybut ten atrybut w dwóch różnych reguły synchronizacji **w z usługi Active Directory — AccountEnabled użytkownika** i **w z usługi Active Directory — użytkownika typowych**. Ze względu na pierwszeństwa reguł synchronizacji atrybut sourceAnchor jest przyczynić się z las z włączonym kontem najpierw po kilku obiektów dołączonych do obiektu metaverse. Jeśli nie ma żadnych obsługą kont, a następnie aparatu synchronizacji używa reguły synchronizacji wszystkich efektywnej **w z usługi Active Directory — użytkownika typowych**. Ta konfiguracja gwarantuje, że nawet w przypadku kont, które są wyłączone, jest nadal sourceAnchor.

![Reguły synchronizacji ruch przychodzący](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulesinbound.png)

Pierwszeństwa reguł synchronizacja jest ustawiona w grupach przez Kreatora instalacji. Wszystkie reguły w grupie mają taką samą nazwę, ale są one podłączone do różnych katalogach połączonego. Kreator instalacji udostępnia reguły **w z usługi Active Directory — dołączanie do użytkownika** najwyższy priorytet i iteracje, na wszystkich połączonych AD katalogów. Następnie jest używany do następnej grupy reguł w wstępnie zdefiniowanej kolejności. W grupie reguły są dodawane w odpowiedniej kolejności łączników zostały dodane w kreatorze. Jeśli inny łącznik zostanie dodany za pomocą kreatora, reguły synchronizacji są zamówić ponownie, a łącznik nowej reguły są wstawiane ostatnio w każdej grupie.

### <a name="putting-it-all-together"></a>Składanie wszystkiego razem
Pracujemy obecnie zapoznaniu się informacji o regułach synchronizacji, aby można było zrozumieć, jak działa Konfiguracja z różnych reguły synchronizacji. Jeśli Przyjrzyj się użytkownika i atrybuty, które są tworzone do metaverse, reguły są stosowane w następującej kolejności:

Nazwa | Komentarz
:------------- | :-------------
W z usługi Active Directory — dołączanie do użytkownika | Reguła dołączania do łącznika rozmieścić obiekty z metaverse.
W z usługi Active Directory — kontoużytkownika włączone | Atrybuty wymagane do logowania się do Azure AD i usługi Office 365. Chcemy następujące atrybuty z obsługą konta.
W z usługi Active Directory — wspólny użytkownika z programu Exchange | Atrybuty znaleziony w globalnej listy adresowej. Przyjęto założenie, że jakość danych jest zalecane w las, w którym mają znaleźć skrzynki pocztowej użytkownika.
W z usługi Active Directory — wspólny użytkownika | Atrybuty znaleziony w globalnej listy adresowej. W przypadku, gdy nie znaleziono skrzynki pocztowej, innych obiektów połączonych może przyczynić się wartość atrybutu.
W z usługi Active Directory — użytkownik programu Exchange | Istnieje tylko jeśli wykryto programu Exchange. Przepływał wszystkich atrybutów programu Exchange infrastruktury.
W z usługi Active Directory — Lync użytkownika | Istnieje tylko jeśli wykryto programu Lync. Przepływał wszystkich atrybutów programu Lync infrastruktury.

## <a name="next-steps"></a>Następne kroki

- Dowiedz się więcej o modelu konfiguracji w [Opis deklaracyjnych-inicjowania obsługi administracyjnej](active-directory-aadconnectsync-understanding-declarative-provisioning.md).
- Dowiedz się więcej na temat języka wyrażeń w [Wyrażeniach opis deklaracyjnych inicjowania obsługi administracyjnej](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md).
- Kontynuuj czytanie, jak działa Konfiguracja w nowym polu w [Opis użytkowników i kontaktów](active-directory-aadconnectsync-understanding-users-and-contacts.md)
- Zobacz, jak zrobić praktyczne przy użyciu deklaracyjnych inicjowania obsługi administracyjnej w [jak wprowadzanie zmian w domyślnej konfiguracji](active-directory-aadconnectsync-change-the-configuration.md).

**Przegląd tematów**

- [Azure AD Connect synchronizacją: opis i dostosowywanie synchronizacji](active-directory-aadconnectsync-whatis.md)
- [Integrowanie tożsamości lokalnych z usługi Azure Active Directory](active-directory-aadconnect.md)
