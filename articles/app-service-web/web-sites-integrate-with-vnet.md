<properties 
    pageTitle="Integracja aplikacji z Azure wirtualnej sieci" 
    description="Przedstawiono sposób nawiązywania połączenia z nowym lub istniejącym Azure wirtualnej sieci aplikacji w usłudze Azure aplikacji" 
    services="app-service" 
    documentationCenter="" 
    authors="ccompy" 
    manager="wpickett" 
    editor="cephalin"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="ccompy"/>

# <a name="integrate-your-app-with-an-azure-virtual-network"></a>Integracja aplikacji z Azure wirtualnej sieci #

Ten dokument zawiera opis funkcji integracji wirtualną sieć Azure aplikacji usługi i pokazano, jak skonfigurować przy użyciu aplikacji w [Usłudze Azure aplikacji](http://go.microsoft.com/fwlink/?LinkId=529714).  Jeśli użytkownik nie zna Azure wirtualnych sieci (VNETs), to funkcja, która pozwala umieszczać wiele Azure zasobów w sieci routeable innych niż internet i kontrolowanie dostępu do.  Następnie można łączyć sieci te do sieci w wersji lokalnej za pomocą różnych technologii VPN.  Aby dowiedzieć się więcej na temat wirtualnych sieci Azure zacząć z informacjami o tutaj: [Omówienie sieci wirtualne Azure][VNETOverview].  

Usługa Azure aplikacji ma dwie formy.  

1. Systemy wielu dzierżawy, które obsługują gamę ceny planów
1. Funkcję premium środowiska usługi aplikacji (ASE), która wdraża do swojego VNET.  

Ten dokument pokazując integracji VNET i nie środowisko usługi aplikacji.  Jeśli chcesz dowiedzieć się więcej na temat funkcji ASE, a następnie Rozpocznij tutaj informacje: [Wprowadzenie do środowiska usługi aplikacji][ASEintro].

Integracja VNET zapewnia dostęp aplikacji sieci web do zasobów w wirtualnej sieci, ale nie udziela prywatny dostęp do aplikacji sieci web z wirtualnej sieci.  Dostęp do witryn prywatnych jest dostępna tylko dla ASE skonfigurowane z wewnętrznych ładowania równoważenia (ILB).  Aby uzyskać szczegółowe informacje na temat korzystania z ASE ILB, rozpoczynać się następujący artykuł: [Tworzenie i używanie ASE ILB][ILBASE]. 

Typowy scenariusz użycia integracji VNET pozwala dostęp do bazy danych lub usługi sieci web, uruchomione na komputerze wirtualnych w sieci wirtualnych Azure z aplikacji sieci web.  W przypadku integracji VNET nie musisz uwidaczniają publicznej punkt końcowy dla aplikacji na swojej maszyn wirtualnych, ale może użyć prywatnych adresów routingu innych niż internet.  

Funkcja integracji VNET:

- wymaga standardowy lub Premium ceny planu 
- czy będą działały z Classic(V1) lub VNET Manager(V2) zasobów 
- Obsługa protokołu TCP i UDP
- Współpraca z aplikacji sieci Web, Mobile i interfejsu API
- Włącza aplikację nawiązać połączenie tylko 1 VNET naraz
- Umożliwia VNETs maksymalnie 5, które należy włączyć z planu usługi aplikacji 
- pozwala na tym samym VNET może być używany przez wiele aplikacji w aplikacji usługi Planowanie
- Obsługa 99,9% Umowa dotycząca poziomu usług ze względu na zależność od bramy VNET

Istnieje kilka rzeczy, których integracji VNET nie obsługuje, w tym:

- Instalowanie z dysku
- Integracja z programem AD 
- NetBios
- dostęp do witryn prywatnych

### <a name="getting-started"></a>Wprowadzenie ###
Oto niektóre rzeczy do zapamiętania przed nawiązaniem połączenia wirtualnej sieci aplikacji sieci web:

- Integracja VNET działa tylko z aplikacjami na **Standardowy** lub **Premium** ceny planu.  Włączanie funkcji, a następnie skalowanie planu usług aplikacji planu cennik nieobsługiwane aplikacji spowoduje utratę ich połączeń do VNETs używają.  
- Jeśli wirtualnej sieci docelowej już istnieje, musi być VPN punktu do witryny włączone z bramą routingu dynamicznego przed mogą być połączone z aplikacji.  Nie można włączyć punktu do witryny wirtualnej sieci prywatnej (VPN), jeśli Centrum skonfigurowano routing statyczny.
- VNET muszą znajdować się w tej samej subskrypcji jako usługi Plan(ASP) usługi aplikacji.  
- Aplikacje, które zostały zintegrowane z VNET będzie używał systemu DNS, którą określono dla tego VNET.
- Domyślnie całkującej aplikacji będzie tylko skierować ruch do Twojej VNET według zdefiniowanych w swojej VNET trasy.  


## <a name="enabling-vnet-integration"></a>Włączanie VNET integracji ##

Ten dokument jest ograniczony przede wszystkim przy użyciu Azure Portal integracji VNET.  Aby włączyć integrację VNET przy użyciu aplikacji przy użyciu programu PowerShell, postępuj zgodnie z instrukcjami poniżej: [Łączenie aplikacji do wirtualnej sieci przy użyciu programu PowerShell][IntPowershell].

Istnieje możliwość nawiązywania połączenia z nowym lub istniejącym wirtualną sieć aplikacji.  Po utworzeniu nowej sieci w ramach usługi integracji następnie oprócz tworzenie VNET dynamiczne bramy routingu będzie wstępnie skonfigurowane dla Ciebie, i wskaż VPN witryny będą dostępne.  

>[AZURE.NOTE] Konfigurowanie nowej integracji wirtualnej sieci może potrwać kilka minut.  

Aby włączyć integrację VNET Otwórz ustawienia aplikacji, a następnie wybierz pozycję sieci.  Interfejs użytkownika, który otwarty oferuje trzy opcje sieci.  Ten przewodnik jest tylko do integracji VNET mimo połączenia hybrydowych i środowiska usługi aplikacji zostały omówione w dalszej części tego dokumentu.  

Jeśli aplikacji nie jest prawidłowo ceny plan interfejs helpfully pozwoli przeskalować planu do planu cennik wyższą wybranych przez użytkownika.


![][1]
 
###<a name="enabling-vnet-integration-with-a-pre-existing-vnet"></a>Włączanie VNET Integracja z istniejącym VNET###
Interfejs użytkownika integracji VNET pozwala wybrać z listy programu VNETs.  Klasyczny VNETs będzie wskazuje, że są takie z wyrazem "Klasyczny" w nawiasach obok nazwy VNET.  Lista została posortowana tak, aby na początku wymieniono VNETs Menedżera zasobów.  Na ilustracji pokazano poniżej widać, można wybrać tylko jeden VNET.  Istnieje wiele powodów, że VNET będzie wyszarzona w tym:

- VNET znajduje się w innej subskrypcji, że Twoje konto ma dostęp do
- VNET nie ma punktu do witryny włączono
- VNET nie ma dynamicznego routingu bramy


![][2]

Aby włączyć integrację po prostu kliknij na VNET chcesz zintegrować z.  Po wybraniu VNET aplikacji zostanie automatycznie uruchomiony ponownie aby zmiany zostały wprowadzone.  

##### <a name="enable-point-to-site-in-a-classic-vnet"></a>Włączanie wskaż witryny w klasycznym VNET #####
Jeśli usługi VNET nie ma bramy ani nie ma punktu do witryny, następnie należy ustawić na pierwszym.  Aby to zrobić dla VNET klasycznego, przejdź do pozycji [Azure Portal] [ AzurePortal] i wyświetlić listę wirtualnych Networks(classic).  Z kliknij tutaj w sieci, dla których chcesz zintegrować z i kliknij duży pole w obszarze podstawowe informacje dotyczące o nazwie połączenia VPN.  W tym miejscu możesz utworzyć punktu witryny sieci VPN i nawet utworzyć bramę.  Po przejściu do punktu do witryny za pomocą środowiska tworzenia bramy będzie około 30 minut przed jest teraz gotowy.  

![][8]

##### <a name="enabling-point-to-site-in-a-resource-manager-vnet"></a>Włączanie wskaż witryny w VNET Menedżera zasobów #####

Aby skonfigurować VNET Menedżera zasobów z bramą i wskaż witrynę, należy użyć programu PowerShell opisane tutaj [skonfigurować połączenie punkt do witryny wirtualnej sieci przy użyciu programu PowerShell][V2VNETP2S].  Interfejs użytkownika do wykonania tej funkcji nie jest jeszcze dostępna. 

### <a name="creating-a-pre-configured-vnet"></a>Tworzenie wstępnie skonfigurowane VNET ###
Jeśli chcesz utworzyć nowy VNET, który skonfigurowano bramy i punkt do witryny aplikacji usługi sieci interfejsu użytkownika ma możliwość to zrobić, ale tylko dla Menedżera zasobów VNET.  Jeśli chcesz utworzyć klasyczny VNET z bramy i punkt do witryny należy zrobić to ręcznie za pośrednictwem interfejsu użytkownika sieci. 

Aby utworzyć VNET Menedżera zasobów, za pośrednictwem interfejsu użytkownika integracji VNET, po prostu wybierz pozycję **Utwórz nowy wirtualnej sieci** i podaj:

- Nazwa wirtualnej sieci
- Wirtualna sieć blok adresu
- Nazwa podsieci
- Blok adresu podsieci
- Blok adresu bramy
- Blok adresu punktu do witryny

Jeśli chcesz, aby ten VNET nawiązywania połączenia z jedną z innych sieci następnie należy unikać pobrania przestrzeń adresów IP, która nakłada się te sieci.  

>[AZURE.NOTE] Tworzenie VNET Menedżera zasobów z bramą trwa około 30 minut i obecnie będzie obsługują VNET aplikacji.  Po utworzeniu usługi VNET z bramą musisz wrócić do aplikacji interfejsu użytkownika integracji VNET i wybierz pozycję do nowej VNET.

![][3]

Azure VNETs zwykle są tworzone w sieci prywatnej adresu.  Domyślnie integracji VNET funkcji może kierować wszelkie przeznaczone dla tych zakresów adresów IP do swojego VNET.  Prywatne zakresy adresów IP są:

- 10.0.0.0/8 — to jest taka sama jak 10.0.0.0 - 10.255.255.255
- 172.16.0.0/12 — to jest taka sama jak 172.16.0.0 - 172.31.255.255 
- 192.168.0.0/16 - jest taka sama, jak 192.168.0.0 - 192.168.255.255
 
Przestrzeń adresów VNET należy określić w notacji CIDR.  Jeśli nie mają doświadczenia w notacji CIDR, jest to metoda określania bloki adresów przy użyciu adresu IP i liczba całkowita przedstawiająca maski sieci. Podręczna karta informacyjna zapewniającą że 10.1.0.0/24 byłyby 256 adresów i 10.1.0.0/25 będzie 128 adresów.  Adres IP protokołu IPv4 z /32 będzie tylko 1 adres.  

Jeśli ustawisz tutaj informacje o serwerze DNS następnie która zostanie ustawiona dla swojego VNET.  Po utworzeniu VNET możesz edytować te informacje z VNET środowiska użytkownika.

Po utworzeniu klasyczny VNET, przy użyciu interfejsu użytkownika integracji VNET utworzy VNET w tej samej grupy zasobów jako aplikacji. 

## <a name="how-the-system-works"></a>Jak działa system ##
W obszarze okładki ta funkcja tworzy u góry technologii VPN punktu do witryny, aby nawiązać połączenie z VNET aplikacji.   Aplikacji w usłudze Azure aplikacji ma wiele dzierżawy architekturę, która wykluczy inicjowania obsługi administracyjnej aplikacji bezpośrednio w VNET jako jest gotowy maszyn wirtualnych.  Tworząc na technologii punktu do witryny możemy ograniczyć dostęp do sieci tylko maszyn wirtualnych hostingu aplikacji.  Dostęp do sieci dodatkowo zależy od rodzaju tych hostów aplikacji tak, aby Twoje aplikacje są dostępne tylko skonfigurowane ich tak, aby uzyskać dostęp do sieci.  

![][4]
 
Jeśli serwer DNS nie konfigurowana wirtualnej sieci należy używać adresów IP.  Podczas korzystania z adresów IP, należy pamiętać, że główną zaletą ta funkcja jest umożliwia używanie prywatnych adresów w sieci prywatnej.  Jeśli ustawisz aplikacji do obsługi adresów IP publicznej dla jednej z pośrednictwem usługi SMS, a następnie nie korzystasz z funkcji integracji VNET a komunikują się przez internet.


##<a name="managing-the-vnet-integrations"></a>Zarządzanie integracji VNET##

Możliwość łączenie i odłączanie do VNET znajduje się na poziomie aplikacji.  Operacje, które mogą mieć wpływ integracji VNET w wielu aplikacjach znajdują się na poziomie ASP.  W interfejsie użytkownika, który jest wyświetlany na poziomie aplikacji pozwala na uzyskanie informacji o swojej VNET.  Większość tych samych informacji jest również wyświetlany na poziomie ASP.  

![][5]

Na stronie Stan funkcji sieci można zobaczyć, jeśli aplikacji jest połączony z VNET.  Jeśli Centrum VNET nie działa dla jakichkolwiek przyczyn następnie to samo Pokaż jako połączone nie.  

Informacje, które masz teraz dostępne w aplikacji, którą poziomu interfejsu użytkownika integracji VNET jest taka sama jak szczegółowe informacje, które otrzymujesz z ASP.  Poniżej przedstawiono te elementy:

- Nazwa VNET — to łącze powoduje otwarcie sieci interfejsu użytkownika
- Lokalizacja - odzwierciedla lokalizacji usługi VNET.  Istnieje możliwość integracji z VNET w innej lokalizacji.
- Stan certyfikatu - istnieje Certyfikaty umożliwia bezpieczne połączenie VPN między aplikacja i VNET.  Odzwierciedla test, aby upewnić się, że są one synchronizacji.
- Stan bramy - usługi bram należy w dół bez względu na powód następnie aplikacji nie może uzyskać dostęp do zasobów VNET.  
- Przestrzeń adresów VNET — jest to obszar adresów IP dla usługi VNET.  
- Wskaż miejsce dla adresu witryny — jest to punkt do witryny Obszar adresów IP dla usługi VNET.  Aplikacji zostanie wyświetlona komunikacji jako pochodząca z jednego adresy IP, w tym miejscu adres.  
- Witryny do obszaru adres witryny — sieci VPN witryny umożliwia łączenie z VNET na zasoby lokalna lub innych VNETs.  Czy masz skonfigurowanego zakresów adresów IP zdefiniowane za pomocą połączenie VPN wyświetli poniżej, a następnie.
- Serwery DNS — Jeśli masz serwery DNS konfigurowana do VNET, a następnie są wymienione tutaj.
- Adresy IP kierowane do VNET — dostępne są listy adresów IP, czy usługi VNET ma routingu zdefiniowanych dla.  Te adresy będą widoczne w tym miejscu.  

Tylko operacji, które można wykonać w widoku aplikacji usługi integracji VNET jest rozłączyć VNET jest podłączony do aplikacji.  Aby wykonać to wystarczy kliknąć Rozłącz u góry.  Ta akcja nie zmienia się z VNET.  VNET i jego konfiguracji, w tym bram pozostaje bez zmian.  Jeśli chcesz usunąć z VNET następnie należy najpierw usunąć w nim tym bram zasobów.  

Widok Planowanie usługi aplikacji są dostępne dodatkowe czynności.  Go jest także dostępny inaczej niż z tej aplikacji.  Do osiągnięcia interfejs sieci ASP po prostu otwórz ASP interfejsu użytkownika i przewiń w dół.  Istnieje element interfejsu użytkownika o nazwie stan funkcji sieci.  Pozwala niektóre informacje pomocnicze wokół usługi integracji VNET.  Kliknięcie polecenia w ten sposób interfejs użytkownika zostanie otwarty interfejs stan funkcji sieci.  Po kliknięciu na "Kliknij tutaj, aby zarządzać" otworzy się interfejsu użytkownika, który zawiera listę integracji VNET w tym ASP.

![][6]

Lokalizacja ASP jest warto pamiętać podczas wyszukiwania w lokalizacjach VNETs są Integracja z.  Gdy VNET znajduje się w innej lokalizacji jest bardziej może powodować wyświetlanie opóźnienie problemów.  

VNETs zintegrowany z jest przypomnienia o ile VNETs że aplikacje są zintegrowane z w tym ASP i ile możesz mieć.  

Aby wyświetlić szczegóły dodane na każdym VNET, wystarczy kliknąć VNET Cię interesuje.  Oprócz szczegóły którego zostały wspomniano wcześniej, pojawi się także wykaz aplikacji w tym ASP, które używają tej VNET.  

W odniesieniu do akcji istnieją dwa podstawowe akcje.  Pierwsza jest możliwość dodawania trasy, które sterują ruchu wychodzącego aplikacji do swojego VNET.  Druga akcja jest możliwość synchronizacji certyfikaty i informacje o sieci.

![][7]

**Routing** Jak wspomniano wcześniej trasy, które są definiowane w swojej VNET to, do czego służy kierujące ruch do Twojej VNET z Twojej aplikacji.  Istnieje kilka zastosowań, jeśli znajduje się miejsce, w którym klienci chcesz wysłać dodatkowe ruchu wychodzącego z aplikacji do VNET i ich tej funkcji.  Co się stanie z ruch będzie maksymalnie jak klienta konfiguruje ich VNET.  

**Certyfikaty** Stan certyfikatu odzwierciedla wyboru wykonywane przez usługę aplikacji, aby sprawdzić poprawność to nadal dobre certyfikatów, które użyto dla połączenia VPN.  Po włączeniu integracji VNET, jeśli jest to pierwszy integracji tego VNET z dowolnej aplikacji, w tym ASP występuje wymagane wymiany certyfikatów w celu zabezpieczenia połączenia.  Wraz z certyfikatów będziemy korzystać z konfiguracji systemu DNS, drogi i inne podobne rzeczy, które dobrze opisują sieci.
Po zmianie tych certyfikatów lub informacji dotyczących sieci będzie konieczne do kliknij pozycję "Synchronizuj sieć".  **Uwaga**: po kliknięciu przycisku "Synchronizuj sieci", a następnie spowoduje krótki awarii w łączność między aplikacji i usługi VNET.  Podczas aplikacji będzie nie można ponownie uruchomić utrata łączności może spowodować witryny nie będzie działał poprawnie.  

##<a name="accessing-on-premise-resources"></a>Uzyskiwanie dostępu do zasobów lokalna##

Jedną z zalet funkcji integracji VNET jest Jeśli Twojej VNET jest podłączony do sieci w wersji lokalnej za pomocą sieci VPN witryny, a następnie aplikacje mogą uzyskiwać dostęp do zasobów w wersji lokalnej z Twojej aplikacji.  Aby to zrobić, ale może być konieczne zaktualizowanie w wersji lokalnej bramy sieci VPN, z trasy dla punktu adres IP witryny zakresu.  Po pierwszym skonfigurowaniu witryny sieci VPN skryptów umożliwia konfigurowanie go należy skonfigurować pocztę trasy, łącznie z punktem na VPN witryny.  Po dodaniu punktu do witryny sieci VPN po swojej tworzenie sieci VPN witryny, a następnie należy ręcznie zaktualizować trasy.  Szczegółowe informacje o tym, jak to zrobić różnią się na bramy i nie zostały opisane w tym miejscu.  

>[AZURE.NOTE] Funkcja integracji VNET będzie współdziałanie z sieci VPN witryny, aby uzyskać dostęp do zasobów lokalna obecnie nie działa on z VPN ExpressRoute, aby zrobić to samo.  Jest to możliwe, podczas integracji z klasyczny lub Menedżer zasobów VNET.  Jeśli chcesz uzyskać dostęp do zasobów przez sieć VPN ExpressRoute można użyć ASE, który może zostać uruchomiony w swojej VNET. 

##<a name="pricing-details"></a>Szczegółowe informacje o cenach##
Istnieje kilka ceny drobne, których należy pamiętać podczas korzystania z funkcji integracji VNET.  Istnieją 3 opłat do korzystania z tej funkcji:

- ASP ceny wymagania warstwy
- Koszty przeniesienia danych
- Brama VPN koszty.

Dla aplikacji można było korzystać z tej funkcji muszą znajdować się w standardowy lub Plan usługi aplikacji Premium.  Więcej informacji można wyświetlić w tych kosztów tutaj: [Cennik usługi aplikacji][ASPricing]. 

Ze względu na sposób punktu VPN witryny są obsługiwane, zawsze masz opłatę dla ruchu wychodzącego danych za pośrednictwem połączenia integracji VNET nawet wtedy, gdy VNET w tym samym centrum danych.  Aby zobaczyć, jak je są zapoznaj się z tutaj: [Szczegóły ceny Transfer danych][DataPricing].  

Ostatni element jest koszt bram VNET.  Jeśli nie potrzebujesz bram dla czegoś innego, takich jak witryny sieci VPN dokonujesz płatności dla bramy do obsługi funkcji integracji VNET.  W tych kosztów w tym miejscu znajdują się szczegóły: [Ceny bramy sieci VPN][VNETPricing].  

##<a name="troubleshooting"></a>Rozwiązywanie problemów##

Gdy ta funkcja jest prosta nie oznacza to, że środowiska pracy użytkownika będzie bezpłatne problem.  Należy wystąpienia problemów z dostępem do punkt końcowy odpowiedniej są niektóre narzędzia, które umożliwia testowanie łączności z konsoli aplikacji.  Istnieją dwa środowiska konsoli, których można używać.  Jeden pochodzi z konsoli Kudu, a druga konsoli, który może osiągnąć w Azure Portal.  Aby przejść do konsoli Kudu z Twojej aplikacji przejdź do Narzędzia -> Kudu.  To jest taka sama, jak przechodząc do [nazwa witryny]. scm.azurewebsites.net.  Po której zostanie otwarty po prostu przejdź do karty konsoli debugowania.  Aby przejść do konsoli Azure hostowanej portalu, a następnie z Twojej aplikacji przejdź do Narzędzia -> konsoli.  


####<a name="tools"></a>Narzędzia####

Polecenie ping narzędzia nslookup i tracert nie będzie działać przy użyciu konsoli z powodu ograniczeń dotyczących zabezpieczeń.  Aby wypełnić void udzielenia zostały dwóch oddzielnych narzędzia dodany.  Aby przetestować funkcje DNS dodana narzędzie o nazwie nameresolver.exe.  Składnia jest następująca:

    nameresolver.exe hostname [optional: DNS Server]

Nameresolver umożliwia sprawdzanie nazwy hostów, która zależy od aplikacji.  W ten sposób można sprawdzić, jeśli masz nic nieprawidłowo skonfigurowane z DNS lub być może nie masz dostępu do swojego serwera DNS.

Narzędzie następnego umożliwia testowanie połączenia kombinację host i port TCP.  To narzędzie jest określane jako tcpping.exe i składnia jest następująca:

    tcpping.exe hostname [optional: port]

To narzędzie będzie informacją, że jeśli można osiągnięcia określonego hosta i portu, ale nie będzie wykonania tego samego zadania, które otrzymujesz w ramach ICMP podstawie narzędzie ping.  Narzędzie ping ICMP informuje, jeśli hosta jest uruchomiony.  Z tcpping użytkownik dowiaduje się, jeśli masz dostęp do określonego portu na hoście.  


####<a name="debugging-access-to-vnet-hosted-resources"></a>Debugowanie dostęp do VNET hostowanej zasobów####

Istnieje wiele czynności, które można zapobiec osiągnięciu określonego hosta oraz portu aplikacji.  W większości przypadków jest jednym z trzech czynności:

- **Jest w ten sposób**  Jeśli masz na komputerze zaporę w ten sposób będą trafienie limit czasu TCP.  W tym przypadku to 21 sekund.  Użyj narzędzia tcpping do testowania łączności.  Port TCP przekroczenia limitu czasu można ze względu na wiele rzeczy poza zapory, ale zacząć.  
- **System DNS jest niedostępna**  Limit czasu DNS wynosi 3 sekundy na serwera DNS.  Jeśli masz 2 serwerów DNS, które jest 6 sekund.  Aby sprawdzić, czy działa DNS za pomocą nameresolver.  Należy pamiętać, że nie można użyć narzędzia nslookup, jak nie używa DNS skonfigurowano usługi VNET.
- **Zakres nieprawidłowe IP P2S** Punktu do zakresu IP witryny musi się znajdować w zakresów adresów IP prywatne RFC 1918 (10.0.0.0-10.255.255.255-172.16.0.0-172.31.255.255-192.168.0.0-192.168.255.255) Jeśli zakres używane adresy IP poza który, a następnie co nie będzie działać.  

Jeśli te elementy nie odebrać problemu, sprawdź pierwszego podczas wykonywania prostych operacji, takich jak:  

- Brama jest wyświetlany jako należące do portalu?
- Certyfikaty są wyświetlane jako synchronizacji?
- Każda zmieniła się konfiguracja sieci bez konieczności wykonywania "Sieć Synchronizuj" w ASP którego dotyczy problem? 

Jeśli do bramy nie działa następnie wywoływanie go ponownie.  Jeśli certyfikatów są zsynchronizowane przejdź do widoku ASP usługi integracji VNET i trafień "Synchronizuj sieci".  Jeśli podejrzewasz, że doszło zmiany wprowadzone w konfiguracji VNET i nie jest wybrana synchronizacji jak usługi ASP, a następnie przejdź do widoku ASP usługi integracji VNET i trafienie Just "Synchronizuj sieci" jako przypomnienie, spowoduje to krótkie awarii z połączeniem VNET i aplikacji.  

Jeśli jest to wszystko prawidłowy należy wyświetlić nieco elementy:

- Czy istnieją inne aplikacje, korzystając z integracji VNET do osiągnięcia zasobów w tym samym VNET? 
- Można przejdź do konsoli aplikacji i używać tcpping do osiągnięcia inne zasoby w swojej VNET?  

Jeśli jest spełniony jeden z powyższych następnie usługi integracji VNET jest prawidłowy i że problem jest w innym miejscu.  Jest to miejsce, w którym otrzymuje się więcej wezwanie, ponieważ istnieje nie prosty sposób, aby sprawdzić, dlaczego nie można osiągnąć host: port.  Oto niektóre z powodów:

- masz zaporę w górę na hoście uniemożliwiające dostęp do portu aplikacji od punktu do zakresu IP witryny.  Często przekraczających podsieci wymaga dostępie.
- hosta docelowej nie działa
- Aplikacja nie działa
- masz problem IP lub nazwa hosta
- Aplikacja nasłuchują na porcie innym niż oczekiwano.  Można to sprawdzić, przechodząc na hoście i za pomocą "netstat - aon" w wierszu cmd.  Spowoduje to wyświetlenie procesu identyfikator oczekuje na co portu.  
- grup zabezpieczeń sieci są skonfigurowane w taki sposób, że one uniemożliwić dostęp do aplikacji hosta i portu punktu do zakresu IP witryny

Pamiętaj, że nie wiesz, jakiego IP w punktu do zakresu IP witryny, korzystające z aplikacji, należy zezwolić na dostęp z całego zakresu.  

Debuguj kolejne czynności to między innymi:

- Zaloguj się do innego maszyn wirtualnych w swojej VNET i próby osiągnięcia zasobów hosta: port stamtąd.  Istnieje kilka narzędzi ping port TCP można użyć w tym celu, lub nawet użyć telnet, jeśli taka potrzeba.  W tym miejscu celem jest tylko w celu ustalenia, czy połączenia jest to maszyn wirtualnych. 
- Wywoływanie aplikacji na inny maszyn wirtualnych i testowanie dostępu do tego host i port z poziomu konsoli z Twojej aplikacji  
####<a name="on-premise-resources"></a>Środowisko lokalne zasobów####
Jeśli programu nie można osiągnąć zasobów lokalnego, najpierw należy sprawdzić jest, jeśli zostanie wyświetlona zasobu w swojej VNET.  Jeśli która działa następne kroki są dosyć łatwe.  Z maszyn wirtualnych w swojej VNET należy wypróbować do osiągnięcia aplikacji w wersji lokalnej.  Można użyć telnet lub narzędzie ping TCP.  Jeśli do maszyn wirtualnych nie osiągnięcia na zasób lokalna, a następnie upewnij się, działa połączenie VPN witryny.  Jeżeli działa Sprawdź te same elementy wspomniano wcześniej, oraz w wersji lokalnej konfiguracji bramy i stan.  

Teraz Jeśli hostowanej usługi VNET mogą skorzystać z maszyn wirtualnych systemu w wersji lokalnej, ale nie aplikacji, a następnie przyczyny jest prawdopodobnie jedną z następujących:
- nie skonfigurowano usługi przekierowuje z punktem dla zakresów adresów IP witryn w Centrum w wersji lokalnej
- grup zabezpieczeń sieci blokują dostęp dla punktu do zakresu IP witryny
- usługi w wersji lokalnej zapory blokuje ruch od punktu do zakresu IP witryny
- masz Route(UDR) zdefiniowane przez użytkownika w Twojej VNET, który uniemożliwia punktu do witryny podstawie ruchu możliwość kontaktowania sieci w wersji lokalnej

## <a name="hybrid-connections-and-app-service-environments"></a>Połączenia hybrydowych i środowiskach aplikacji usługi##
Istnieją 3 funkcje, które umożliwiają dostęp do zasobów VNET hostowana.  Są to:

- Integracja z programem VNET
- Hybrydowe połączenia
- Środowiskach aplikacji usługi

Połączenia hybrydowych wymaga zainstalowania agenta przekazywania o nazwie Manager(HCM) połączenia hybrydowych w Twojej sieci.  HCM równa musi mieć możliwość utworzenia połączenia Azure, a także aplikacji.  To rozwiązanie jest szczególnie doskonałe z siecią zdalną, takich jak w sieci lokalna lub nawet innej chmury hostowanej sieci, ponieważ nie wymaga punktu końcowego dostępne internet.  HCM równa działa tylko w systemie Windows i nie może mieć maksymalnie 5 wystąpienia uruchomiony w celu zapewnienia wysokiej dostępności.  Mimo że hybrydowych połączeń TCP obsługuje tylko i każdy punkt końcowy HC ma w celu dopasowania do kombinacji określonego hosta: portu.  

Funkcję Środowisko usługi aplikacji pozwala na uruchamianie wystąpienie usługi aplikacji Azure w swojej VNET.  Dzięki temu zasoby aplikacji programu access w swojej VNET bez wszelkie dodatkowe czynności.  Niektóre z zalet środowisku usługi aplikacji są 14 GB pamięci RAM umożliwiają 8 pracowników core dedykowane.  Kolejną zaletą jest to, że można skalować system stosownie do potrzeb.  W przeciwieństwie do środowisk wielu dzierżawy, rozmiar, na którym strona ASP jest ograniczony w ASE sterowanie liczby zasobów, w której chcesz przekazać do systemu.  W odniesieniu do fokusu sieci tego dokumentu, jedną z czynności, które otrzymujesz w ramach ASE, który nie przy użyciu VNET integracji z jest czy można pracować z VPN ExpressRoute.  

Gdy znajduje się, że niektóre za pomocą zakładki wielkości liter, żadna z tych funkcji można zastąpić pozostałe.  Znajomość funkcji jakie jest powiązane z Twoimi wymaganiami i jak można używać tej funkcji.  Na przykład:

- Jeśli makr i po prostu chcesz uruchomić witryny w Azure i mieć dostęp do bazy danych na komputerze w obszarze biurku najłatwiejszym za pomocą jest hybrydowych połączenia.  
- W przypadku dużej organizacji, która chce umieszczenie dużej liczby właściwości sieci web w publicznej w chmurze i zarządzać nimi w swojej własnej sieci, a następnie chcesz przejść ze środowiskiem usługi aplikacji.  
- Jeśli masz wiele aplikacji usługi hostowanej aplikacji i po prostu chcesz uzyskać dostęp do swojego VNET zasobów, a następnie integracja VNET jest tak, aby przejść.  

Poza przypadkami użycia, istnieje kilka uproszczenia aspektach.  Jeśli do VNET jest już połączony z siecią w wersji lokalnej przy użyciu integracji VNET lub środowisku usługi aplikacji jest łatwym sposobem używanie lokalna zasobów.  Z drugiej strony Jeśli usługi VNET nie jest połączony z siecią w wersji lokalnej, a następnie jest o wiele więcej ogólnych, aby skonfigurować sieć VPN witryny z usługi VNET w porównaniu z instalacją HCM równa.  

Poza różnice funkcjonalne istnieją również ceny różnice.  Funkcja Środowisko usługi aplikacji jest premia oferująca usługi ale oferuje najbardziej sieci możliwości konfiguracji oprócz inne zalety.  Integracja VNET można używać z standardowy lub ASP Premium i doskonale umożliwiającego bezpieczne korzystanie z zasobów w swojej VNET z wielu dzierżawy usługi aplikacji.  Połączenia hybrydowych obecnie zależy od BizTalk konta, w której znajduje się ceny poziomów, które rozpocząć bezpłatne, a następnie przejść stopniowe wyższy zależą od ilości, które są potrzebne.  Gdy nadejdzie do pracy nad wielu sieci, ale nie ma żadnych innych funkcji hybrydowych połączenia, w którym można uzyskać dostęp do zasobów w oddzielnych sieciach również ponad 100.    


<!--Image references-->
[1]: ./media/web-sites-integrate-with-vnet/vnetint-upgradeplan.png
[2]: ./media/web-sites-integrate-with-vnet/vnetint-existingvnet.png
[3]: ./media/web-sites-integrate-with-vnet/vnetint-createvnet.png
[4]: ./media/web-sites-integrate-with-vnet/vnetint-howitworks.png
[5]: ./media/web-sites-integrate-with-vnet/vnetint-appmanage.png
[6]: ./media/web-sites-integrate-with-vnet/vnetint-aspmanage.png
[7]: ./media/web-sites-integrate-with-vnet/vnetint-aspmanagedetail.png
[8]: ./media/web-sites-integrate-with-vnet/vnetint-vnetp2s.png

<!--Links-->
[VNETOverview]: http://azure.microsoft.com/documentation/articles/virtual-networks-overview/ 
[AzurePortal]: http://portal.azure.com/
[ASPricing]: http://azure.microsoft.com/pricing/details/app-service/
[VNETPricing]: http://azure.microsoft.com/pricing/details/vpn-gateway/
[DataPricing]: http://azure.microsoft.com/pricing/details/data-transfers/
[V2VNETP2S]: http://azure.microsoft.com/documentation/articles/vpn-gateway-howto-point-to-site-rm-ps/
[IntPowershell]: http://azure.microsoft.com/documentation/articles/app-service-vnet-integration-powershell/
[ASEintro]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[ILBASE]: http://azure.microsoft.com/documentation/articles/app-service-environment-with-internal-load-balancer/
