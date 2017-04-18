<properties
    pageTitle="Kopii lokalnej maszyn wirtualnych VMware lub serwerów fizycznych do witryny pomocniczej | Microsoft Azure"
    description="Powielić maszyny wirtualne VMware i systemu Windows i Linux oraz serwerów fizycznych do witryny pomocniczej z Odzyskiwanie witryny Azure za pomocą tego artykułu."
    services="site-recovery"
    documentationCenter=""
    authors="nsoneji"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="nisoneji"/>


# <a name="replicate-on-premises-vmware-virtual-machines-or-physical-servers-to-a-secondary-site"></a>Powielić VMware w lokalnym środowisku maszyn wirtualnych systemu lub serwerów fizycznych do witryny pomocniczej


## <a name="overview"></a>Omówienie

InMage Scout w Odzyskiwanie witryny Azure udostępnia w czasie rzeczywistym replikacji między witrynami VMware lokalnego. InMage Scout jest dołączany do subskrypcji usługi Azure Odzyskiwanie witryny.


## <a name="prerequisites"></a>Wymagania wstępne

**Konto Azure**: musisz mieć konto [Microsoft Azure](https://azure.microsoft.com/) . Możesz rozpocząć z [bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/). [Aby uzyskać więcej informacji](https://azure.microsoft.com/pricing/details/site-recovery/) na temat ceny Odzyskiwanie witryny.


## <a name="step-1-create-a-vault"></a>Krok 1: Tworzenie magazynu
1. Zaloguj się do [portalu Azure](https://portal.azure.com).
2. Kliknij pozycję Nowy > Zarządzanie > Wykonywanie kopii zapasowych i odzyskiwanie witryny (usługi OMS). Można także kliknąć przycisk Przeglądaj > magazynu usługi odzyskiwania > Dodaj.
3. W polu **Nazwa** Określ przyjazną nazwę identyfikującą magazyn. Jeśli masz więcej niż jedną subskrypcję, wybierz jeden z nich.
4. W **grupie zasobów** Utwórz nową grupę zasobów, lub wybierz istniejący. Określanie obszaru Azure i wypełnij wymagane pola. 
5.  W polu **Lokalizacja**wybierz regionu geograficznego dla magazyn. Aby sprawdzić obsługiwanych regionów, zobacz [Azure witryny odzyskiwania ceny](https://azure.microsoft.com/pricing/details/site-recovery/).
5. Jeśli chcesz szybko dostępu magazynu z pulpitu nawigacyjnego kliknij polecenie Przypnij do pulpitu nawigacyjnego, a następnie kliknij przycisk Utwórz.
6. Nowy magazyn pojawi się na pulpicie nawigacyjnym > wszystkie zasoby, a w głównym usługi odzyskiwania magazynów karta.

## <a name="step-2-configure-the-vault-and-download-inmage-scout-components"></a>Krok 2: Konfigurowanie magazyn i Pobierz składniki InMage Scout
7. W usługach odzyskiwania karta magazynami wybierz z magazynu i kliknij pozycję Ustawienia.
8. W obszarze **Ustawienia** > **Wprowadzenie** kliknij pozycję **Odzyskiwanie witryny** > krok 1: **Przygotowywanie infrastruktury** > **cel ochrony**.
9. W **celu ochrony** zaznacz do odzyskiwania witryny, a wybierz pozycję Tak, VMware vSphere monitor maszyny wirtualnej. Kliknij przycisk OK.
10. W oknie **Ustawienia Scout**kliknij przycisk klucz oprogramowania i rejestracji GA InMage Scout 8.0.1 pobierania do pobrania. Pliki Instalatora dla wszystkich wymaganych składników znajdują się w pliku zip pobrany.


## <a name="step-3-install-component-updates"></a>Krok 3: Instalowanie aktualizacji składnika

Przeczytaj o najnowsze [aktualizacje](#updates). Będzie zainstalować aktualizacji plików na serwerach w następującej kolejności:

1. Serwer ODBIERANIA, jeśli istnieje
2. Serwer konfiguracji
3. Proces serwerów
3. Serwerów docelowych wzorca
4. serwery vContinuum
5. Serwer źródłowy (Windows i Linux Server)

Instalowanie aktualizacji w następujący sposób:

1. Pobierz plik zip [aktualizacji](https://aka.ms/asr-scout-update4) . Ten plik zip zawiera następujące pliki:

    - RX_8.0.4.0_GA_Update_4_8725872_16Sep16.tar.GZ
    - CX_Windows_8.0.4.0_GA_Update_4_8725865_14Sep16.exe
    - UA_Windows_8.0.4.0_GA_Update_4_9035261_27Sep16.exe
    - UA_RHEL6 64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz
    - vCon_Windows_8.0.4.0_GA_Update_4_8921562_16Sep16.exe
    - UC update4 bitów dla RHEL5 OL5, OL6, SUSE 10, SUSE 11: UA_<Linux OS>_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz 
    
2. Wyodrębnianie plików zip.<br>
3. **Do ODBIERANIA serwera**: kopiowanie **RX_8.0.4.0_GA_Update_4_8725872_16Sep16.tar.gz** na serwerze ODBIERANIA i wyodrębnij ją. W folderze wyodrębnionej uruchamianie **Zainstaluj**.<br>
4. **Na serwerze serwera proces konfiguracji**: kopiowanie **CX_Windows_8.0.4.0_GA_Update_4_8725865_14Sep16.exe** do konfiguracji serwera i serwera proces. Kliknij dwukrotnie, aby go uruchomić.<br>
5. **Serwer docelowy wzorcowe dla systemu Windows**: Aby zaktualizować ujednolicony agenta, skopiuj **UA_Windows_8.0.4.0_GA_Update_4_9035261_27Sep16.exe** na serwerze docelowej wzorca. Kliknij ją dwukrotnie, aby go uruchomić. Należy zauważyć, że ujednolicony agenta dotyczy również z serwerem źródłowym. Należy zainstalować go na serwerze źródłowym, jak również wymienione w dalszej części tej listy.<br>
7. **Na serwerze vContinuum**: kopiowanie **vCon_Windows_8.0.4.0_GA_Update_4_8921562_16Sep16.exe** na serwerze vContinuum.  Upewnij się, że została zamknięta kreatora vContinuum. Kliknij dwukrotnie plik, aby go uruchomić.<br>
8. **Serwer docelowy wzorca dla Linux**: Aby zaktualizować ujednolicony agenta, skopiuj **UA_RHEL6 64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz** na serwerze docelowej wzorca i wyodrębnij ją. W folderze wyodrębnionej uruchamianie **Zainstaluj**.<br>
9. **Źródło dla systemu Windows server**: Aby zaktualizować ujednolicony agenta, skopiuj **UA_Windows_8.0.4.0_GA_Update_4_9035261_27Sep16.exe** z serwerem źródłowym. Kliknij ją dwukrotnie, aby go uruchomić.<br>
10. **Serwer źródłowy dla Linux**: Aby zaktualizować ujednolicony agenta, skopiuj odpowiedniej wersji pliku UC na serwerze Linux i wyodrębnij ją. W folderze wyodrębnionej uruchamianie **Zainstaluj**.  Przykład: Dla serwera 6,7 x 64 RHEL, skopiuj **UA_RHEL6 64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz** na serwerze i wyodrębnij ją. W folderze wyodrębnionej uruchamianie **Zainstaluj**.

## <a name="step-4-set-up-replication"></a>Krok 4: Konfigurowanie replikacji
1. Konfigurowanie replikacji między źródła i docelowych witrynach VMware.
2. Aby uzyskać instrukcje zapoznaj się z dokumentacją InMage Scout, pobranych z produktem. Można również dostęp do dokumentacji w następujący sposób:

    - [Informacje o wersji](https://aka.ms/asr-scout-release-notes)
    - [Macierz zgodności](https://aka.ms/asr-scout-cm)
    - [Przewodnik użytkownika](https://aka.ms/asr-scout-user-guide)
    - [Przewodnik użytkownika ODBIERANIA](https://aka.ms/asr-scout-rx-user-guide)
    - [Przewodnik szybkiej instalacji](https://aka.ms/asr-scout-quick-install-guide)


## <a name="updates"></a>Aktualizacje

### <a name="azure-site-recovery-scout-801-update-4"></a>Aktualizacja Scout 8.0.1 odzyskiwania Azure witryny 4
Aktualizacja Scout 4 jest aktualizacja zbiorcza. Ma wszystkie poprawki update1 do update3 i następujące nowe poprawki i rozszerzenia.

**Obsługa nowej platformy** 

- Dodano obsługę dla vCenter-vSphere 6.0, 6.1 i 6.2
- Dodano obsługę dla następujących systemów operacyjnych Linux
    - Czerwone funkcję Enterprise Linux (RHEL) 7.0, 7.1 i 7.2 
    - CentOS 7.0, 7.1 i 7.2
    - Czerwone funkcję Enterprise Linux (RHEL) 6.8
    - CentOS 6.8

>[AZURE.NOTE]
>
> RHEL-CentOS 7 64-bitowej wersji **InMage_UA_8.0.1.0_RHEL7-64_GA_06Oct2016_release.tar.gz** jest dostarczana z pakietem Scout GA podstawowego **InMage_Scout_Standard_8.0.1 GA.zip**. Pobierz pakiet Scout GA z portalu, jak wspomniano w [step1](site-recovery-vmware-to-vmware.md#Step 1: Create a vault).

**Poprawki i ulepszenia** 

- Zamknięcie Ulepszona obsługa dla następujących systemów operacyjnych Linux i klony zapobiec występowaniu niechcianych ponownie zsynchronizuj problemów.
    - Czerwone funkcję Enterprise Linux (RHEL) 6.x
    - Linux Oracle (OL) 6.x
- Linux należy wykonać dostęp do folderu, którego uprawnienia w katalogu instalacji agenta ujednolicony teraz są ograniczone tylko do użytkowników lokalnych.
- W systemie Windows traci synchronizację problem podczas intensywnie wydawania typowych rozłożone spójności zakładka na załadować rozłożone aplikacji, takich jak SQL i punkt udostępniania klastrów.
- Dodano dziennika powiązanych fix CX podstawowej Instalatora pakietu.
- Łącze pobierania vCLI 6.0 VMware jest dodawany do podstawowej Instalatora docelowej wzorzec systemu Windows.
- Dodać więcej kontroli i dzienników zmiany konfiguracji sieci podczas awaryjnego i ćwiczenia DR.
- CX nie zgłoszono niekiedy także przechowywania informacji.  
- Klaster fizycznie głośność zmieniać rozmiar operacji za pomocą Kreatora vContinuum się niepowodzeniem, jeśli stało zmniejszania głośności źródła.
- Ochrona klaster nie powiodło się "Nie można odnaleźć podpisu dysku" podczas klaster dysk jest PRDM.
- cxps transportu serwera awarii z powodu wyjątku spoza zakresu. 
- Nazwa serwera i IP kolumn są teraz rozmiar wypychanych instalacji stronie kreatora vContinuum.
- Ulepszenia interfejsu API ODBIERANIA
    - Zawiera pięć najnowszych dostępne typowe spójności punktów (tylko gwarantowana znaczniki).
    - Udostępnia wydajność i szczegóły wolnego miejsca dla wszystkich urządzeń chroniony.
    - Zawiera stan sterownika Scout na serwerze źródłowym. 
    
>[AZURE.NOTE] 
>
>- Teraz pakiet podstawowy **InMage_Scout_Standard_8.0.1_GA.zip** zostały zaktualizowane CX podstawowej Instalator **InMage_CX_8.0.1.0_Windows_GA_26Feb2015_release.exe** i docelowej wzorzec Windows podstawowej Instalator **InMage_Scout_vContinuum_MT_8.0.1.0_Windows_GA_26Feb2015_release.exe**. Dla wszystkich nowych instalacji za pomocą nowego bitów GA CX i docelowej wzorzec systemu Windows.
>- Aktualizacja 4 mogą być stosowane bezpośrednio na 8.0.1 GA.
>- Serwer konfiguracji i ODBIERANIA aktualizacji nie można obniżyć po ich są stosowane w systemie.

### <a name="azure-site-recovery-scout-801-update-3"></a>Azure witryny odzyskiwania Scout 8.0.1 aktualizacji 3
Aktualizacja 3 zawiera następujące poprawki i ulepszenia:

- Serwer konfiguracji i ODBIERANIA się nie powieść zarejestrować do magazynu Odzyskiwanie witryny, gdy znajdują się za serwerem proxy.
- Liczba godzin w celu punktu odzyskiwania (RPO) nie jest spełniony nie jest wyświetlany aktualizowane w raport dotyczący kondycji.
- Serwer konfiguracji nie można zsynchronizować z ODBIERANIA podczas ESX sprzętu szczegółów lub szczegóły sieci bez żadnych znaków UTF-8.
- Kontrolery domeny systemu Windows Server 2008 R2 nie można uruchomić po odzyskiwania.
- Synchronizacja w trybie offline nie działa zgodnie z oczekiwaniami.
- Po przełączeniu maszyn wirtualnych (maszyn wirtualnych) usunięcie pary replikacji otrzymuje zablokowany w Interfejsie użytkownika CX od dłuższego czasu, a użytkownik nie może wykonać awarii i wznowić działanie.
- Ogólna migawki, które zostały tak zoptymalizowane operacji, które są wykonywane przez zadanie spójności, aby zmniejszyć aplikacji odłączenie takich jak klienci SQL.
- Działanie narzędzia spójności (VACP.exe) została ulepszona, zmniejszając użycie pamięci, wymagany do tworzenia migawek w systemie Windows.
- Naciśnięcie Zainstaluj awarie usługi, gdy hasło jest większa niż 16 znaków.
- vContinuum nie jest sprawdzanie i monitowanie o podanie poświadczeń vCenter nowy po zmianie poświadczeń.
- W systemie Linux Menedżera pamięci podręcznej wzorca docelowej (cachemgr) jest pliki nie są pobierane z serwera proces, co spowoduje ograniczania pary replikacji.
- Gdy kolejność dysku fizycznym pracy awaryjnej klaster (MSCS) nie jest taka sama na wszystkich węzłach, replikacja nie jest ustawiony dla niektórych wielkości klaster.
<br/>Należy zauważyć, że klaster musi być reprotected skorzystać z tej poprawki.  
- Funkcja SMTP nie działa zgodnie z oczekiwaniami po uaktualnieniu ODBIERANIA od Scout 7.1 do Scout 8.0.1.
- Więcej statystykę zostały dodane w dzienniku operacja wycofywania do śledzenia czasu, którą podjętych w celu jego wykonaniem.
- Dodano obsługę dla systemów operacyjnych Linux na serwerze źródłowym:
    - Aktualizacja czerwony funkcję Enterprise Linux (RHEL) 6 7
    - Aktualizowanie centOS 6 7
- CX i ODBIERANIA interfejs użytkownika umożliwia teraz wyświetlanie powiadomień dla pary, które przechodzi do trybu mapy bitowej.
- Następujące poprawki zabezpieczeń zostały dodane ODBIERANIA:

**Opis problemu**|**Procedury wykonania**
---|---
Autoryzacja pomijać za pośrednictwem manipulowania parametru|Ograniczony dostęp do innych niż odpowiednich użytkowników.
Fałszowaniu żądanie między witrynami|Zaimplementowana koncepcja token strony, generowany losowo dla każdej strony. <br/>Dzięki temu zostaną wyświetlone: <li> Istnieje tylko jedno logowania wystąpienie dla tego użytkownika.</li><li>Nie działa odświeżania strony — nastąpi przekierowanie do pulpitu nawigacyjnego.</li>
Przekazywanie pliku złośliwy|Ograniczone pliki do określonych rozszerzeń plików. Dozwolone rozszerzenia są: 7z, aiff, asf, avi, bmp, csv, doc, docx, fla, flv, gif, gz, gzip, jpeg, jpg, dziennika, mid, mov, mp3, mp4, mpc, mpeg, mpg, ods, odt, plik pdf, png, ppt, pptx, pxd, qt, pamięci ram, rar, Menedżera zasobów, rmi, rmvb, rtf, sdc, sitd, swf, sxc, sxw, tar, tgz, tif, tiff, txt, vsd, wav, wma, wmv, xls, xlsx, xml i zip.
Trwałe skryptów krzyżowych | Dodane wprowadzania reguły sprawdzania poprawności.


>[AZURE.NOTE]
>
>-  Wszystkie aktualizacje Odzyskiwanie witryny kumulują się. Aktualizacja 3 zawiera wszystkie poprawki aktualizacji 1 i 2 aktualizacji. Aktualizacja 3 mogą być stosowane bezpośrednio na 8.0.1 GA.
>-  Serwer konfiguracji i ODBIERANIA aktualizacji nie można obniżyć po ich są stosowane w systemie.

### <a name="azure-site-recovery-scout-801-update-2-update-03dec15"></a>Azure witryny odzyskiwania Scout 8.0.1 aktualizacja 2 (wymagana aktualizacja 03 15 gru)

Poprawki w pakiecie 2 obejmują:

- **Serwer konfiguracji**: Rozwiązywanie problemu, który uniemożliwia bezpłatne funkcję pomiarowe 31 dni roboczych zgodnie z oczekiwaniami, gdy serwer konfiguracji została zarejestrowana Odzyskiwanie witryny.
- **Agent ujednolicony**: Rozwiązywanie problemu z aktualizacji 1, które spowodowały aktualizacji nie są instalowane na serwerze docelowym wzorca po została uaktualniona z wersji 8.0 lub 8.0.1.


### <a name="azure-site-recovery-scout-801-update-1"></a>Aktualizacja Scout 8.0.1 odzyskiwania Azure witryna 1

Aktualizacja 1 zawiera następujące poprawki i nowe funkcje:

- 31 dni wolne ochrony dla każdego wystąpienia serwera. Dzięki temu można przetestować funkcje lub skonfigurować dowodu koncepcji.
    - Wszystkie operacje na serwerze, w tym awaryjnego i powrotu, są bezpłatne dla pierwszej 31 dni, począwszy od momentu serwer najpierw chronionej za pomocą Scout odzyskiwania witryny.
    - Z 32 dnia roku każdy serwer chronionych jest rozliczana według stawki standardowej wystąpienie ochrony Azure Odzyskiwanie witryny do witryny należące do klienta.
    - W dowolnej chwili liczba chronionego serwerów, które są aktualnie pobieranych jest dostępna na stronie pulpitu nawigacyjnego magazynu Odzyskiwanie witryny Azure.
- Pomoc techniczna dla vSphere interfejsu wiersza polecenia (vCLI) dodawać 5,5 Aktualizacja 2.
- Pomoc techniczna dla systemów operacyjnych Linux na serwerze źródłowym dodawać:
    - Aktualizowanie RHEL 6 6
    - Aktualizowanie RHEL 5 11
    - Aktualizacja centOS 6 6
    - Aktualizacja centOS 5 11
- Błąd poprawek w celu rozwiązania następujących problemów:
    - Rejestracja magazynu konfiguracji serwera lub serwera ODBIERANIA zakończy się niepowodzeniem.
    - Klaster ilości nie są wyświetlane zgodnie z oczekiwaniami podczas grupowany maszyn wirtualnych są reprotected, gdy one.
    - Awarii zakończy się niepowodzeniem, kiedy serwer docelowy wzorca znajduje się na innym serwerze ESXi z maszyn wirtualnych produkcji lokalnej.
    - Uprawnienia pliku konfiguracji zostaną zmienione po uaktualnieniu do 8.0.1, który ma wpływ na ochronę i operacji.
    - Próg ponowne synchronizowanie nie są wymuszane zgodnie z oczekiwaniami, która prowadzi do niespójne replikacją.
    - Ustawienia RPO nie są prawidłowo wyświetlane w interfejsie konfiguracji serwera. Wartość kompresji danych wyświetlana niepoprawnie skompresowany wartość.
    -  Operacji usuwania nie powoduje usunięcia zgodnie z oczekiwaniami w Kreatorze vContinuum i replikacja nie jest usuwany z interfejsu konfiguracji serwera.
    -  W Kreatorze vContinuum dysk jest automatycznie niezaznaczone po kliknięciu **szczegółów** w widoku dysku podczas ochrony maszyn wirtualnych MSCS.
    - Podczas fizycznej do wirtualnego scenariusz (P2V) wymagane usługi HP, takich jak CIMnotify i CqMgHost, nie są przenoszone do ręcznego w odzyskiwania maszyn wirtualnych. Efektem w czasie uruchamiania dodatkowe.
    - Ochrona maszyn wirtualnych Linux zakończy się niepowodzeniem, gdy istnieje więcej niż 26 dysków na serwerze docelowym wzorca.

## <a name="next-steps"></a>Następne kroki

Publikowanie pytania, na które masz na [forum usługi Azure odzyskiwania](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).
