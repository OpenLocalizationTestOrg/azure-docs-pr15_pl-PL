<properties 
   pageTitle="Niepowodzenie wstecz maszyn wirtualnych VMware i serwerów fizycznych z platformy Azure VMware (starsza wersja) | Microsoft Azure" 
   description="W tym artykule opisano, jak ponownie niepowodzenie maszyny wirtualnej VMware replikowane Azure z Odzyskiwanie witryny Azure." 
   services="site-recovery" 
   documentationCenter="" 
   authors="ruturaj" 
   manager="mkjain" 
   editor=""/>

<tags
   ms.service="site-recovery"
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.topic="article"
   ms.workload="storage-backup-recovery" 
   ms.date="10/05/2016"
   ms.author="ruturajd@microsoft.com"/>

# <a name="fail-back-vmware-virtual-machines-and-physical-servers-from-azure-to-vmware-with-azure-site-recovery-legacy"></a>Niepowodzenie wstecz maszyn wirtualnych VMware i serwerów fizycznych z platformy Azure VMware z Odzyskiwanie witryny Azure (starsza wersja)

> [AZURE.SELECTOR]
- [Azure Portal](site-recovery-failback-azure-to-vmware.md)
- [Portal Azure klasyczny](site-recovery-failback-azure-to-vmware-classic.md)
- [Azure portalu klasyczny (starsza wersja)](site-recovery-failback-azure-to-vmware-classic-legacy.md)


Usługa Azure witryny odzyskiwania składa się na strategii odzyskiwania (BCDR) ciągłości i danych biznesowych przez orchestrating replikacji, pracy awaryjnej i odzyskiwanie maszyn wirtualnych i serwerów fizycznych. Urządzenia mogą być replikowane Azure lub centrum danych lokalnych pomocniczą. Krótki przegląd odczytywanie [Co to jest Odzyskiwanie witryny Azure?](site-recovery-overview.md)

## <a name="overview"></a>Omówienie

W tym artykule opisano, jak niepowodzenie wstecz maszyn wirtualnych VMware i serwerów fizycznych systemu Windows i Linux z platformy Azure do swojej witryny lokalnej po zostały replikowane z witryny lokalnej Azure.

>[AZURE.NOTE] Ten artykuł zawiera opis starsze scenariusza. Instrukcje należy używać w tym artykule tylko, jeśli replikowane Azure za pomocą [tych instrukcjach starsze](site-recovery-vmware-to-azure-classic-legacy.md). Jeśli możesz skonfigurować replikacji przy użyciu [rozszerzonego wdrożenia](site-recovery-vmware-to-azure-classic-legacy.md) , a następnie postępuj zgodnie z instrukcjami w [tym artykule](site-recovery-failback-azure-to-vmware-classic.md) , aby zakończyć się niepowodzeniem z powrotem. 


## <a name="architecture"></a>Architektura

Ten diagram przedstawia scenariusz awaryjnego i awarii. Niebieskie linie są połączenia używane podczas pracy awaryjnej. Czerwone linie są używane podczas awarii połączenia. Linie ze strzałkami przechodzenie przez internet.

![](./media/site-recovery-failback-azure-to-vmware/vconports.png)

## <a name="before-you-start"></a>Przed rozpoczęciem 

- Czy nie powiodło nad maszyny wirtualne VMware lub serwerów fizycznych i powinna działać w Azure.
- Należy zauważyć, że możesz tylko niepowodzenie wstecz maszyn wirtualnych VMware i systemu Windows i Linux oraz serwerów fizycznych z platformy Azure VMware maszyn wirtualnych w lokalnej witryny podstawowy.  Jeśli możesz już się niepowodzeniem ponownie fizyczny komputer, przełączanie awaryjne do Azure spowoduje przekonwertowanie go na maszyn wirtualnych Azure i powrotu do VMware spowoduje przekonwertowanie go na maszyny VMware.

Poniżej opisano, jak skonfigurować awarii:

1. **Konfigurowanie awarii składniki**: musisz skonfigurować serwer vContinuum lokalnego i wskaż serwer konfiguracji maszyn wirtualnych w Azure. Serwer proces będzie również skonfigurować jako maszyn wirtualnych Azure wysyłanie danych do lokalnego serwera docelowej wzorca. Zarejestruj się serwer procesów z serwera konfiguracji, który obsługiwany tym przełączeniu. Zainstaluj lokalnego serwera docelowej wzorca. W razie potrzeby systemu Windows server wzorca docelowej skonfigurowano ją automatycznie podczas instalacji vContinuum. W razie potrzeby Linux musisz go ręcznie skonfigurować na oddzielnym serwerze.
2. **Włączanie ochrony i powrotu**: po skonfigurowaniu składników w vContinuum musisz włączyć ochronę dla nie powiodło się nad maszyny wirtualne Azure. Będzie sprawdzać gotowości maszyny wirtualne i uruchom trybie awaryjnym z platformy Azure do witryny lokalnej. Po zakończeniu awarii reprotect maszyn lokalnego tak, aby uruchomić były replikowane Azure.



## <a name="step-1-install-vcontinuum-on-premises"></a>Krok 1: Instalowanie vContinuum lokalnego

Musisz zainstalować serwer vContinuum w środowisku lokalnym, aby wskazywał na serwer konfiguracji.

1.  [Pobierz vContinuum](http://go.microsoft.com/fwlink/?linkid=526305). 
2.  Następnie pobrać wersję [vContinuum aktualizacji](http://go.microsoft.com/fwlink/?LinkID=533813) .
3. Zainstaluj najnowszą wersję programu vContinuum. Na stronie **powitalnej** kliknij przycisk **Dalej**.
    ![](./media/site-recovery-failback-azure-to-vmware/image2.png)
4.  Na pierwszej stronie kreatora określ adres IP serwera CX i CX port serwera. Zaznacz pole wyboru **Użyj protokołu HTTPS**.

    ![](./media/site-recovery-failback-azure-to-vmware/image3.png)

5.  Znajdź adres IP serwera konfiguracji na karcie **pulpitu nawigacyjnego** serwera konfiguracji maszyn wirtualnych w Azure.
    ![](./media/site-recovery-failback-azure-to-vmware/image4.png)

6.  Znajdowanie serwera konfiguracji publicznej port HTTPS na karcie **punkty końcowe** serwera konfiguracji maszyn wirtualnych w Azure.

    ![](./media/site-recovery-failback-azure-to-vmware/image5.png)

7. Na stronie **Szczegóły hasło CS** Określ hasło, które zauważyć w dół, po zarejestrowaniu serwer konfiguracji. Jeśli nie pamiętasz go sprawdzić **C:\\Program Files (x86)\\systemów InMage\\prywatne\\connection.passphrase** na serwerze konfiguracji maszyn wirtualnych.

    ![](./media/site-recovery-failback-azure-to-vmware/image6.png)

8.  Na stronie **Wybieranie lokalizacji docelowej** określ miejsce, w którym chcesz zainstalować na serwerze vContinuum i kliknij przycisk **Zainstaluj**.

    ![](./media/site-recovery-failback-azure-to-vmware/image7.png)

9. Po zakończeniu instalacji, możesz uruchomić vContinuum.
    ![](./media/site-recovery-failback-azure-to-vmware/image8.png)


## <a name="step-2-install-a-process-server-in-azure"></a>Krok 2: Instalowanie serwera proces platformy Azure 

Należy zainstalować serwer procesów platformy Azure tak, aby maszyny wirtualne platformy Azure można wysłać dane do lokalnego serwera docelowej wzorca. 

1.  Na stronie **Konfiguracja serwerów** Azure zaznacz, aby dodać nowy serwer proces.

    ![](./media/site-recovery-failback-azure-to-vmware/image9.png)

2.  Określ serwer proces nazwy oraz nazwę i hasło, aby połączyć maszyn wirtualnych jako administrator. Wybierz serwer konfiguracji, do którego chcesz zarejestrować serwer proces. Powinien to być ten sam serwer, którego używasz do ochrony i kończą się niepowodzeniem w środowisku maszyn wirtualnych systemu.
3.  Określanie Azure sieci, w którym należy wdrożyć serwer procesów. Należy tej samej sieci co serwer konfiguracji. Określanie unikatowej podsieci wybrany adres IP i zacznij wdrożenia.

    ![](./media/site-recovery-failback-azure-to-vmware/image10.png)

4.  Zadanie zostanie wywołana do wdrożenia serwera proces.

    ![](./media/site-recovery-failback-azure-to-vmware/image11.png)

5.  Po wdrożeniu serwera proces platformy Azure możesz zalogować się do serwera przy użyciu poświadczeń określonej. Zarejestruj serwer procesów w taki sam sposób rejestrowania lokalnego serwera proces. 

    ![](./media/site-recovery-failback-azure-to-vmware/image12.png)

>[AZURE.NOTE] Serwery zarejestrowana podczas awarii nie będą widoczne w obszarze właściwości maszyn wirtualnych w Odzyskiwanie witryny. Tylko będą one widoczne na karcie **Serwery** serwera konfiguracji, do którego zostanie zarejestrowany. Może potrwać około 10 do 15 minut do momentu ich wyświetlania na karcie Serwer procesów.


## <a name="step-3-install-a-master-target-server-on-premises"></a>Krok 3: Instalowanie lokalnego serwera docelowej wzorca

Zależnie od systemu operacyjnego w środowisku maszyn wirtualnych systemu źródła jest potrzebne do zainstalowania Linux lub lokalnego serwera wzorca docelowej systemu Windows.

### <a name="deploy-a-windows-master-target-server"></a>Wdrażanie serwera wzorca docelowej systemu Windows

Główny cel systemu Windows już jest dołączany do konfiguracji vContinuum. Po zainstalowaniu vContinuum główny jest również używany na tym samym komputerze i zarejestrowany na serwerze konfiguracji.

1.  Aby rozpocząć wdrożenia, utworzyć pusty komputera lokalnego na hoście ESX, na który chcesz odzyskać maszyny wirtualne z platformy Azure.

2.  Upewnij się, że są co najmniej dwa dyski dołączone do maszyn wirtualnych — jedna jest używana przez system operacyjny, a drugi służy do przechowywania dysk.

3.  Zainstaluj system operacyjny.

4.  Zainstaluj vContinuum na serwerze. To również wykonuje instalacji serwera docelowego wzorca.

### <a name="deploy-a-linux-master-target-server"></a>Wdrażanie serwera wzorca docelowej Linux

1.  Aby rozpocząć wdrożenia, utworzyć pusty komputera lokalnego na hoście ESX, na który chcesz odzyskać maszyny wirtualne z platformy Azure.

2.  Upewnij się, że są co najmniej dwa dyski dołączone do maszyn wirtualnych — jedna jest używana przez system operacyjny, a drugi służy do przechowywania dysk.

3.  Zainstaluj system operacyjny Linux. System docelowy wzorca Linux nie należy używać LVM głównego lub przechowywania spacji miejsca do magazynowania. Serwer wzorca docelowej Linux jest skonfigurowany w celu uniknięcia odnajdowania partycje i dyski LVM domyślnie.
4.  Partycje, które można utworzyć:

    ![](./media/site-recovery-failback-azure-to-vmware/image13.png)

5.  Przeprowadzić poniżej opublikować kroki instalacji przed rozpoczęciem instalacji wzorcowej docelowej.


#### <a name="post-os-installation-steps"></a>Publikowanie kroki instalacji systemu operacyjnego

Aby uzyskać identyfikatory SCSI dla każdego dysku twardego SCSI w maszyny wirtualnej Linux, Włącz parametru "dysk. EnableUUID = TRUE "w następujący sposób:

1. Zamknij komputer wirtualnych.
2. Kliknij prawym przyciskiem myszy pozycji maszyn wirtualnych w panelu po lewej stronie > **Zmień ustawienia**.
3. Kliknij kartę **Opcje** . Wybierz pozycję **Zaawansowane\>ogólne elementu** > **Parametry konfiguracji**. **Parametry konfiguracji** opcja jest dostępna tylko w przypadku, gdy komputer jest zamknięty.

    ![](./media/site-recovery-failback-azure-to-vmware/image14.png)

4. Sprawdza, czy wiersz z dysku **. EnableUUID** istnieje. Jeśli i nie jest ustawiona na **False** jest ustawiony na **wartość True** (bez rozróżniania wielkości liter). Jeśli istnieje i jest ustawiona na PRAWDA, kliknij przycisk **Anuluj** i przetestować polecenie SCSI w systemie operacyjnym gościa po uruchomieniu w górę. Jeśli nie istnieje kliknij pozycję **Dodaj wiersz**.
5. Dodawanie dysku. EnableUUID w kolumnie **Nazwa** . Ustaw wartość true. Nie dodawaj powyższych wartości wraz z podwójnych cudzysłowów.

    ![](./media/site-recovery-failback-azure-to-vmware/image15.png)

#### <a name="download-and-install-the-additional-packages"></a>Pobierz i zainstaluj dodatkowe pakiety

Uwaga: Upewnij się, że system ma połączenie z Internetem przed pobraniem i zainstalowaniem dodatkowe pakiety.

\#Instalowanie YUM -y xfsprogs perl lsscsi rsync wget kexec narzędzi

To polecenie do pobrania tych 15 pakietów z repozytorium CentOS 6.6 i instaluje je:

BC-1.06.95-1.el6.x86\_64. prędkości.

busybox-1.15.1-20.el6.x86\_64. prędkości.

elfutils bibliotekami-0.158-3.2.el6.x86\_64. prędkości.

kexec — narzędzia-2.0.0-280.el6.x86\_64. prędkości.

lsscsi 0,23 2.el6.x86\_64. prędkości.

LZO-2.03-3.1.el6\_5.1.x86\_64. prędkości.

Perl-5.10.1-136.el6\_6.1.x86\_64. prędkości.

Perl moduł-podłączany-3.90-136.el6\_6.1.x86\_64. prędkości.

Perl-Pod-anuluje-1.04-136.el6\_6.1.x86\_64. prędkości.
    
Perl-Pod-proste — 3.13-136.el6\_6.1.x86\_64. prędkości.

Perl bibliotekami-5.10.1-136.el6\_6.1.x86\_64. prędkości.

Perl wersję 0,77 136.el6\_6.1.x86\_64. prędkości.

rsync-3.0.6-12.el6.x86\_64. prędkości.

szałowe 1.el6.x86 1.1.0\_64. prędkości.

wget-1.12-5.el6\_6.1.x86\_64. prędkości.

Uwaga: Jeśli komputerem źródłowym używa plików Reiser lub XFS urządzenia głównego lub uruchamiania, następnie następujące pakiety powinny być pobrane i zainstalowane w docelowej wzorca Linux przed ochrony.

\#dysk CD /usr/local

\#wget <http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm>

\#wget <http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm>

\#obrotów na minutę - ivh kmod-reiserfs 0,0 1.el6.elrepo.x86\_64. obrotów na minutę reiserfs witryny-3.6.21-1.el6.elrepo.x86\_64. prędkości.

\#wget <http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_64.rpm>

\#obrotów na minutę - ivh xfsprogs-3.1.1-16.el6.x86\_64. prędkości.

#### <a name="apply-custom-configuration-changes"></a>Stosowanie zmian w konfiguracji niestandardowych

Przed zastosowaniem tych zmian upewnij się, że zostały wykonane poprzedniej sekcji, a następnie wykonaj następujące czynności:


1. Kopiowanie RHEL 6-64 Unified agentem binarne do nowo utworzonego system operacyjny.

2. Uruchom polecenie untar pliku binarnego: **tar - zxvf \<nazwę pliku\> **

3. Polecenie, aby nadać uprawnienia: \# **chmod 755./ApplyCustomChanges.sh**

4. Uruchom skrypt: ** \# ./ApplyCustomChanges.sh**. Uruchom skrypt tylko raz na serwerze. Po uruchomieniu skrypt, należy ponownie uruchomić serwer.



### <a name="install-the-linux-server"></a>Zainstaluj serwer Linux


1. [Pobierz](http://go.microsoft.com/fwlink/?LinkID=529757) plik instalacji.
2. Skopiuj plik maszyn wirtualnych wzorzec Linux docelowej przy użyciu narzędzia klienta sftp wybranych przez użytkownika. Alternatywnie Zaloguj się do maszyny wirtualnej wzorca docelowej Linux i Pobierz pakiet instalacyjny za pomocą łącza podanego za pomocą wget
3. Zalogowaniu Linux wzorcowej docelowej serwera maszyn wirtualnych przy użyciu ssh klienta wybranych przez użytkownika
4. Jeśli masz połączenie z siecią Azure, na którym jest używany serwer Linux docelowej głównego przez połączenie VPN, użyj wewnętrzny adres IP serwera, który można znaleźć w karta **pulpit nawigacyjny** maszyn wirtualnych i port 22, aby nawiązać połączenie docelowej wzorca Linux Server przy użyciu powłoki bezpiecznego.
5. Jeśli łączysz się z serwerem wzorca docelowej Linux przez publiczne połączenie internetowe za pomocą serwera docelowego wzorca Linux publicznej wirtualny adres IP (w środowisku maszyn wirtualnych systemu karta **pulpit nawigacyjny** ) i punkt końcowy publicznej tworzyć ssh zalogować do Linux servder.
6. Wyodrębnianie plików z archiwum tar Instalatora Server gzipped Linux docelowej wzorca, uruchamiając: *"tar — xvzf automatycznego odzyskiwania systemu Microsoft\_UC\_8.2.0.0\_RHEL6 64\"* z katalogu, w którym znajduje się plik Instalatora.

    ![](./media/site-recovery-failback-azure-to-vmware/image16.png)

7. Po wyodrębnieniu Instalatora plików do innego katalogu, przejdź do katalogu, do którego archiwizowanie zawartości tar zostały wyodrębnione. Z następującą ścieżkę katalogu uruchamianie "sudo. / install.sh".

    ![](./media/site-recovery-failback-azure-to-vmware/image17.png)

8. Po wyświetleniu monitu wybierz rolę podstawowego wybierz **2 (główny cel)**. Pozostaw inne opcje instalacji interakcyjnych na wartości domyślne.
9. Czekaj na celu kontynuowania instalacji i języka interfejsu konfiguracji hosta są wyświetlane. Narzędzie Konfiguracja hosta dla sarget wzorca Linux Server to narzędzie wiersza polecenia. Rozmiar nie zmienia się ssh okno Narzędzie klienta. Użyj klawiszy strzałek wybierz opcję w **obszarze globalne** , a następnie naciśnij klawisz ENTER na klawiaturze.

    ![](./media/site-recovery-failback-azure-to-vmware/image18.png)

    ![](./media/site-recovery-failback-azure-to-vmware/image19.png)


10. W polu **IP** wprowadź adres IP wewnętrznego serwera konfiguracji (możesz go znaleźć na karcie **pulpitu nawigacyjnego** serwera konfiguracji maszyn wirtualnych) i naciśnij klawisz ENTER. W polu **Port** **22** i naciśnięcie klawisza ENTER. 
11.  Pozostaw **HTTPS Użyj** jako **Tak**, a następnie naciśnij klawisz ENTER.
12.  Wprowadź hasło, który został wygenerowany na serwer konfiguracji. Jeśli korzystasz z klientem Kit z komputera systemu Windows w celu ssh maszyn wirtualnych Linux umożliwia Shift + Insert powoduje wklejenie zawartości Schowka. Skopiuj hasło do lokalnego Schowka za pomocą klawiszy Ctrl-C i wklej go za pomocą Shift + Insert. Naciśnij klawisz ENTER.
13.  Przejdź do Zamknij, a następnie naciśnij klawisz ENTER za pomocą klawisza Strzałka w prawo. Poczekaj, aż do ukończenia instalacji.

    ![](./media/site-recovery-failback-azure-to-vmware/image20.png)

Jeśli z jakiegoś powodu nie można zarejestrować serwer Linux docelowej wzorca do serwera konfiguracji należy więc ponownie, uruchamiając narzędzie konfiguracji hosta z /usr/local/ASR/Vx/bin/hostconfigcli (musisz najpierw ustawić uprawnienia dostępu do tego katalogu, uruchamiając chmod jako administratorów).


#### <a name="validate-master-target-registration"></a>Sprawdzanie poprawności rejestracji wzorcowej docelowej

Można sprawdzić, czy serwer docelowy wzorcowej została pomyślnie zarejestrowana do konfiguracji serwera magazynu Odzyskiwanie witryny Azure > **Serwer konfiguracji** > **Szczegóły serwera**.

>[AZURE.NOTE] Po zarejestrowaniu serwera docelowego wzorca, jeśli zostanie wyświetlony błędy konfiguracji maszyny wirtualnej została usunięta z platformy Azure lub punktów końcowych nie są poprawnie skonfigurowane, wynika to jednak konfiguracji wzorca docelowej zostanie wykryta przez Azure dndpoints po wdrożeniu wzorca docelowej platformy Azure, to nie dotyczy lokalnego wzorca docelowej lokalnego serwera. Nie dotyczy awarii i można te błędy ignorować. 



## <a name="step-4-protect-the-virtual-machines-to-the-on-premises-site"></a>Krok 4: Ochrona maszyn wirtualnych do lokalnej witryny

Musisz ochronę maszyny wirtualne do lokalnej witryny, zanim nie zostanie ponownie.

### <a name="before-you-begin"></a>Przed rozpoczęciem

Gdy maszyny to nie powiodło się nad Azure, dodaje dodatkowe temp stacji dysków w poszukiwaniu pliku strony. Jest to dodatkowe dysk, które nie są zwykle wymagane przez usługi nie powiodło się nad maszyn wirtualnych, ponieważ może być już dysk przeznaczony do pliku strony. Przed rozpoczęciem odwrotnej ochrony maszyn wirtualnych należy się upewnić, że ten dysk do trybu offline, aby nie uzyskać chroniony. Czynność w następujący sposób:

1.  Otwórz Zarządzanie komputerem i wybierz pozycję Zarządzanie magazynem, tak, aby Wyświetla listę dysków w trybie online i dołączone do komputera.
2.  Zaznacz dysku tymczasowym dołączone do komputera, a następnie wybierz pozycję, aby wyświetlić go w trybie offline. 

### <a name="protect-the-vms"></a>Ochrona maszyny wirtualne

1. W portalu usługi Azure Sprawdź stanów maszyny wirtualnej, aby upewnić się, że masz nie nad.

    ![](./media/site-recovery-failback-azure-to-vmware/image21.png)

2. Uruchom program vContinuum na tym komputerze.

    ![](./media/site-recovery-failback-azure-to-vmware/image8.png)

3. Kliknij **Nową funkcję ochrony** i wybierz typ systemu operacji 

4.  W nowym oknie, że Otwórz wybierz **Typ systemu operacyjnego** > **Uzyskać szczegółowe informacje** dotyczące maszyny wirtualne, który chcesz powrócić. **Szczegóły podstawowy serwer**identyfikowanie i wybierz maszyn wirtualnych, które mają być chronione. Maszyny wirtualne są wyświetlane w obszarze Nazwa hosta vCenter, które były na przed pracy awaryjnej.
5.  Po wybraniu maszyny wirtualnej ochrony (zawiera już nie nad Azure) okno podręczne zawiera dwa wpisy dla maszyny wirtualnej. To, ponieważ serwer konfiguracji wykrywa dwa wystąpienia maszyn wirtualnych zarejestrowany do niego. Należy usunąć wpis dla lokalnego maszyn wirtualnych tak, aby chronić poprawne maszyn wirtualnych. Aby zidentyfikować poprawne pozycja Azure maszyn wirtualnych w tym polu, możesz zalogować się do maszyn wirtualnych Azure i przejdź do \Microsoft Azure Site Recovery\Application Data\etc C:\Program Files (x86). W drscout.conf pliku określ identyfikator hosta. W oknie dialogowym vContinuum Zachowaj wpis, dla którego został znaleziony identyfikator hosta na maszyn wirtualnych. Usuwanie wszystkich pozostałych wpisów. Aby wybrać poprawny maszyn wirtualnych może dotyczyć jego adres IP. IP adres zakresu lokalnego będzie maszyn wirtualnych lokalnego.

    ![](./media/site-recovery-failback-azure-to-vmware/image22.png)

    ![](./media/site-recovery-failback-azure-to-vmware/image23.png)

6. Na serwerze vCenter zatrzymanie maszyny wirtualnej. Możesz również usunąć maszyny wirtualne lokalnego.
7. Następnie określ lokalnego serwera MT, do którego chcesz chronić maszyny wirtualne. W tym celu należy połączyć się z serwerem vCenter, do której chcesz powrócić.

    ![](./media/site-recovery-failback-azure-to-vmware/image24.png)

8. Wybierz serwer docelowej wzorca oparte na hoście, do którego chcesz odzyskać maszyn wirtualnych.

    ![](./media/site-recovery-failback-azure-to-vmware/image24.png)

9. Jest dostępna opcja replikacji dla każdej z nich maszyn wirtualnych. W tym celu należy zaznaczyć odzyskiwania magazynu danych po stronie, do której zostaną przywrócone maszyny wirtualne. W tabeli wymieniono różne opcje, które należy podać dla każdego maszyn wirtualnych.

    ![](./media/site-recovery-failback-azure-to-vmware/image25.png)

    **Opcja** | **Opcja zalecane wartości**
    ---|---
    Adres IP serwera procesu | Wybierz serwer proces wdrożony platformy Azure
    Rozmiar przechowywania w MB| 
    Wartość przechowywania | 1
    Dni i godzin | Dni
    Interwał spójności | 1
    Zaznacz docelowy magazynu danych | Magazynu danych dostępne w witrynie odzyskiwania. Do przechowywania danych należy miejsca i będzie dostępny do hosta ESX, na którym chcesz przywrócić maszyny wirtualnej.


10. Skonfiguruj odpowiednie właściwości uzyskiwanie maszyny wirtualnej po przełączeniu do lokalnej witryny. Właściwości przedstawiono w poniższej tabeli.

    ![](./media/site-recovery-failback-azure-to-vmware/image26.png)

    **Właściwość** | **Szczegóły**
    ---|---
    Konfiguracja sieci| Dla każdej karty sieciowej wykryte zaznacz go i kliknij pozycję **Zmień** , aby skonfigurować adres IP awarii dla maszyny wirtualnej. 
    Konfiguracja sprzętu| Określ Procesora i pamięci maszyn wirtualnych. Ustawienia można stosować do wszystkich pośrednictwem SMS chcesz chronić. Aby zidentyfikować poprawne wartości dla Procesora i pamięci, możesz odwołują się do rozmiaru roli IAAS maszyny wirtualne i wyświetlić liczbę rdzenie i pamięci przydzielone.
    Nazwa wyświetlana | Po powrotu do lokalnego możesz zmienić nazwę maszyn wirtualnych zostaną wyświetlone w magazynie vCenter. Domyślna nazwa jest nazwą hosta komputera maszyn wirtualnych. Aby określić nazwę maszyn wirtualnych, może dotyczyć listy maszyn wirtualnych w grupie ochrona.
    Konfiguracja NAT | Omawiane szczegółowo poniżej

    ![](./media/site-recovery-failback-azure-to-vmware/image27.png)

#### <a name="configure-nat-settings"></a>Konfigurowanie ustawień translatora adresów Sieciowych

1. Aby włączyć ochronę maszyn wirtualnych, kanały komunikacji muszą zostać ustanowione. Pierwszy kanał jest zawarte między maszyny wirtualnej i z serwera proces. Ten kanał zbiera dane z maszyn wirtualnych i wysyła go do serwera procesu, który przesyła dane do serwera docelowego wzorca. Jeśli serwer procesów i maszyny wirtualnej mają być chronione są w tej samej sieci wirtualnej Azure nie potrzebujesz za pomocą ustawień translatora adresów Sieciowych. W przeciwnym razie Określ ustawienia translatora adresów Sieciowych. Wyświetlanie publiczny adres IP serwera proces Azure. 

    ![](./media/site-recovery-failback-azure-to-vmware/image28.png)

2. Drugi kanał jest między serwera proces i serwer docelowy wzorca. Możliwość użycia translatora adresów Sieciowych, czy nie zależy od tego, czy przez połączenie VPN podstawie lub komunikowania się przez internet. Nie zaznaczaj translatora adresów sieciowych, jeśli korzystasz z sieci VPN, ale tylko wtedy, gdy używasz połączenia z Internetem.

    ![](./media/site-recovery-failback-azure-to-vmware/image29.png)

    ![](./media/site-recovery-failback-azure-to-vmware/image30.png)

3. Jeśli jeszcze nie usunięte maszyn wirtualnych lokalnego zgodnie z ustawieniami i magazynu danych, które występują powrót do nadal zawiera VMDK stare następnie należy upewnić się, że awarii, które maszyn wirtualnych zostanie utworzony w nowe miejsce. W tym celu wybierz pozycję Ustawienia **Zaawansowane** , a następnie określ alternatywny Folder, aby przywrócić **Ustawienia nazwę folderu**. Pozostaw innych opcji za pomocą ustawień domyślnych. Stosowanie ustawień nazwy folderu na wszystkich serwerach.

    ![](./media/site-recovery-failback-azure-to-vmware/image31.png)

4. Uruchom sprawdzanie gotowości do zapewnienia chcesz być chronione w lokalnym środowisku maszyn wirtualnych systemu. 

    ![](./media/site-recovery-failback-azure-to-vmware/image32.png)


5. Poczekaj na jego ukończenie. Jeśli pomyślnie znajduje się na wszystkie maszyny wirtualne można określić nazwę plan ochrony. Kliknij pozycję **Chroń** , aby rozpocząć.

    ![](./media/site-recovery-failback-azure-to-vmware/image33.png)


6. Można monitorować postęp w vContinuum.

    ![](./media/site-recovery-failback-azure-to-vmware/image34.png)

7. Po wykonaniu kroku, które kończy się **Aktywacji Plan ochrony** można monitorować ochrony maszyn wirtualnych w portalu Odzyskiwanie witryny.

    ![](./media/site-recovery-failback-azure-to-vmware/image35.png)

8. Można wyświetlić stan dokładnie klikając maszyn wirtualnych i monitorowanie postępu.

    ![](./media/site-recovery-failback-azure-to-vmware/image36.png)

## <a name="prepare-the-failback-plan"></a>Przygotowywanie planu powrotu

Aby przygotować plan awarii przy użyciu vContinuum, aplikacja może nie powrót do lokalnej witryny w dowolnym momencie. Plany te odzyskiwania są bardzo podobne do planów odzyskiwania w Odzyskiwanie witryny.

1.  Uruchamianie vContinuum i wybierz pozycję **Zarządzaj planów** > **odzyskać.** Można zobaczyć listę wszystkich planów używane niepowodzenie przez maszyny wirtualne. Za pomocą samego planów odzyskać.

    ![](./media/site-recovery-failback-azure-to-vmware/image37.png)

2. Wybierz plan ochrony i wszystkie maszyny wirtualne, aby przywrócić w nim. Po wybraniu poszczególnych maszyn wirtualnych można zobaczyć więcej szczegółowych informacji, takich jak serwer ESX docelowej i dysku maszyn wirtualnych źródłowego. Kliknij przycisk **Dalej** Aby uruchomić Kreatora odzyskać, a następnie wybierz maszyny wirtualne, który chcesz odzyskać.

    ![](./media/site-recovery-failback-azure-to-vmware/image38.png)

3. Można odzyskać w oparciu o wiele opcji, ale zaleca się za pomocą **Najnowszej znacznika** i wybierz pozycję **Zastosuj do wszystkich maszyny wirtualne** , aby upewnić się, stosowanie najnowszych danych ze maszyny wirtualnej.
4. Uruchamianie **sprawdzenie gotowości.** To sprawdza się, jeśli prawo parametrów są skonfigurowane do Włącz odzyskiwanie maszyn wirtualnych. Jeśli wszystkie testy się pomyślnie, kliknij przycisk **Dalej** . Jeśli nie, przejrzyj dziennik i naprawić błędy.

    ![](./media/site-recovery-failback-azure-to-vmware/image39.png)

8.  W **Konfiguracji maszyn wirtualnych** upewnij się, czy ustawienia odzyskiwania są poprawnie skonfigurowane. Jeśli chcesz, możesz zmienić ustawienia maszyn wirtualnych.

    ![](./media/site-recovery-failback-azure-to-vmware/image40.png)

9. Przejrzyj listę maszyn wirtualnych, które zostaną przywrócone i określić kolejność odzyskiwania. Należy zauważyć, że maszyny wirtualne są wyświetlane przy użyciu nazwy hosta komputera. Może być trudne do mapowania nazwy hosta komputera maszyn wirtualnych.
Aby zamapować imiona i nazwiska, przejdź do maszyn wirtualnych **pulpitu nawigacyjnego** w Azure i sprawdź nazwę hosta maszyn wirtualnych.

    ![](./media/site-recovery-failback-azure-to-vmware/image41.png)

10. Określ nazwę planu i wybierz **później odzyskać**. Zalecamy, aby odzyskać później, ponieważ początkowej ochrony mogą nie być ukończone. 
11. Polecenie **odzyskać** , aby zapisać plan lub wyzwalanie odzyskiwania, jeśli wybrano odzyskiwanie teraz, a nie później. Możesz sprawdzić stan odzyskiwania, aby sprawdzić, jeśli plan jest zostały zapisane.

    ![](./media/site-recovery-failback-azure-to-vmware/image42.png)

    ![](./media/site-recovery-failback-azure-to-vmware/image43.png)

## <a name="recover-virtual-machines"></a>Odzyskiwanie maszyn wirtualnych

Po utworzeniu w planie, można odzyskać maszyn wirtualnych. Przed Sprawdź, czy maszyn wirtualnych ukończenie synchronizacji. Jeśli stan replikacji jest wyświetlany przycisk OK ochrony zostanie zakończone, a próg RPO zostały spełnione. Można sprawdzić kondycję we właściwościach maszyn wirtualnych.

![](./media/site-recovery-failback-azure-to-vmware/image44.png)

Wyłącz funkcję Azure maszyn wirtualnych przed rozpoczęciem odzyskiwania. Dzięki temu jest nie podziału badania nad umysłem i czy użytkownicy będą tylko dostęp jedną kopię aplikacji. 


1.  Rozpocznij zapisanego planu. W vContinuum wybierz **Monitor** planów. Ta lista zawiera wszystkie plany, które zostało uruchomione.

    ![](./media/site-recovery-failback-azure-to-vmware/image45.png)

2.  Wybierz plan w **odzyskiwania** , a następnie kliknij przycisk **Start**. Można monitorować odzyskiwania. Po włączono maszyny wirtualne na można nawiązać je w vCenter.

    ![](./media/site-recovery-failback-azure-to-vmware/image46.png)

## <a name="protect-to-azure-again-after-failback"></a>Ochrona Azure ponownie po awarii

Po zakończeniu awarii będą prawdopodobnie zechcesz ponownie ochrony maszyn wirtualnych. Czynność w następujący sposób:

1.  Sprawdź, czy pracują maszyn wirtualnych lokalnego i że aplikacje są dostępne.
2.  W portalu Odzyskiwanie witryny Azure zaznacz maszyn wirtualnych i usuń je. Wybierz, aby wyłączyć ochronę maszyn wirtualnych. Dzięki temu, że maszyny wirtualne nie jest już chroniony.
3.  Platformy Azure usuwanie nie powiodło się nad Azure maszyn wirtualnych
4.  Usuń stary maszyn wirtualnych w vSpehere. Są to maszyny wirtualne, które wcześniej nie się nad Azure.
5.  W portalu Odzyskiwanie witryny chronić maszyny wirtualne, które ostatnio nie powiodło się nad. Po są one chronione możesz je dodać do planu odzyskiwania.
 
## <a name="next-steps"></a>Następne kroki



- [Przeczytaj o](site-recovery-vmware-to-azure-classic.md) replikacji maszyn wirtualnych VMware i serwerów fizycznych Azure za pomocą rozszerzonego.

 
