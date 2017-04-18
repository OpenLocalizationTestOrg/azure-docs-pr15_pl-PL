<properties
    pageTitle="Tworzenie planów odzyskiwania | Microsoft Azure" 
    description="Tworzenie planów odzyskiwania z Odzyskiwanie witryny Azure niepowodzenie nad i odzyskiwanie grupy maszyn wirtualnych i serwerów fizycznych." 
    services="site-recovery" 
    documentationCenter="" 
    authors="rayne-wiselman" 
    manager="jwhit" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery" 
    ms.date="10/05/2016"
    ms.author="raynew"/>

# <a name="create-recovery-plans"></a>Tworzenie planów odzyskiwania

Usługa Azure witryny odzyskiwania składa się na strategii odzyskiwania (BCDR) ciągłości i danych biznesowych przez orchestrating replikacji, pracy awaryjnej i odzyskiwanie maszyn wirtualnych i serwerów fizycznych. Urządzenia mogą być replikowane Azure lub centrum danych lokalnych pomocniczą. Krótki przegląd odczytywanie [Co to jest Odzyskiwanie witryny Azure?](site-recovery-overview.md).


## <a name="overview"></a>Omówienie

Ten artykuł zawiera informacje o tworzenia i dostosowywania planów odzyskiwania. 

Plany odzyskiwania składają się z jednej lub kilku grup numerowana, zawierające maszyn wirtualnych lub fizycznie serwerów, które chcesz zakończyć się niepowodzeniem przez siebie. Plany odzyskiwania wykonaj następujące czynności:

- Definiowanie grup komputerów, na których się nie powieść na, a następnie zacznij ze sobą.
- Model zależności między komputerami grupując je w grupie plan odzyskiwania. Przykład, jeśli chcesz zakończyć się niepowodzeniem nad i uzupełnić określonej aplikacji czy grupy maszyn wirtualnych dla tej aplikacji w tej samej grupie plan odzyskiwania.
- Automatyzowanie i rozszerzanie pracy awaryjnej. Plan odzyskiwania można uruchamiać test planowana, lub nieplanowanego przełączania awaryjnego. Możesz dostosować planów odzyskiwania przy użyciu skryptów, automatyzacji Azure i ręczne akcje.

Plany odzyskiwania są wyświetlane na **Plany odzyskiwania** w portalu Odzyskiwanie witryny.


Publikowanie jakieś komentarze lub pytania u dołu tego artykułu lub [Azure odzyskiwania usług Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)w witrynie.

## <a name="before-you-start"></a>Przed rozpoczęciem

Pamiętaj o następujących kwestiach:

- Plan odzyskiwania nie mieszać maszyny wirtualne z kartami jednej lub wielu sieci. Jest to spowodowane mieszanie te maszyny wirtualne nie jest dozwolona w usłudze w chmurze Azure.
- Można rozszerzyć planów odzyskiwania skrypty i ręczne akcje. Pamiętaj o następujących kwestiach:
    - Pisanie skryptów przy użyciu programu Windows PowerShell.
    - Upewnij się, że skryptów przy użyciu bloków efektywnej spróbuj tak, aby bezpiecznie obsługi wyjątków. Jeśli istnieje wyjątek skryptu przestaje działać i zadania pokazuje nie powiodło się.  Jeśli wystąpi błąd, pozostała część skrypt nie działa. Jeśli dzieje, gdy używasz nieplanowanego przełączania awaryjnego, nadal będzie planu odzyskiwania. W tej sytuacji po uruchomieniu planowane trybie awaryjnym przestanie planu odzyskiwania. W takim przypadku rozwiązać skrypt, upewnij się, że działa zgodnie z oczekiwaniami, a następnie ponownie uruchom planu odzyskiwania.
    - Polecenie zapisu hosta nie działa w obszarze skrypt planu odzyskiwania, a skrypt zakończy się niepowodzeniem. Jeśli chcesz utworzyć dane wyjściowe, utworzyć skrypt serwera proxy, który z kolei jest uruchamiany skrypt głównym i upewnij się, że wszystkie dane wyjściowe jest przekazywany w potoku się przy użyciu >> polecenia.
    - Limit czasu skryptu, jeśli nie zwraca w 600 sekund.
    - Jeśli nic jest zapisywany stderr, skrypt będą klasyfikowane jako nie powiodło się. Te informacje będą wyświetlane w sekcji szczegółów wykonywanie skryptów.
    - Jeśli używasz VMM wdrożenia zauważyć, że:

        - Uruchamianie skryptów w planie odzyskiwania w ramach konta usługi VMM. Upewnij się, że to konto ma uprawnienia do odczytu do udziału zdalnego, na którym znajduje się skrypt i przetestować skrypt do uruchomienia na poziomie uprawnień konta usługi VMM.
        - Polecenia cmdlet VMM są dostarczane w module programu Windows PowerShell. Moduł programu Windows PowerShell VMM jest instalowane podczas instalacji konsoli VMM. Moduł VMM mogą być ładowane do skrypt przy użyciu następującego polecenia skryptu: Import-Module-virtualmachinemanager nazwy. [Dowiedz się więcej](hhttps://technet.microsoft.com/library/hh875013.aspx).
        - Upewnij się, że masz co najmniej jeden serwer biblioteki w wdrożenia VMM. Domyślnie dla serwera VMM ścieżkę Udostępnij biblioteki znajduje się lokalnie na serwerze VMM za pomocą nazwę folderu MSCVMMLibrary.
        - W przypadku ścieżce Udostępnij biblioteki zdalnym (lub lokalnego, ale nie udostępniane MSCVMMLibrary, Udostępnij należy skonfigurować następująco (za pomocą \\libserver2.contoso.com\share\ jako przykład):
            - Otwórz Edytor rejestru i przejdź do Recovery\Registration witryny HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\Azure.
            -  Edytowanie wartości ScriptLibraryPath i umieść tę wartość, co \\libserver2.contoso.com\share\. określić pełną nazwę FQDN. Podaj uprawnienia do lokalizacji, Udostępnij.
            -  Upewnij się, przetestowanie skrypt przy użyciu konta użytkownika, które ma takie same uprawnienia jak konto usługi VMM zapewnienie działania autonomiczny sprawdzania skryptów w taki sam sposób, że będą one w planach odzyskiwania. Na serwerze VMM ustawić zasady wykonywania, aby pominąć w następujący sposób:
                -  Otwieranie konsoli programu Windows PowerShell 64-bitowej, używając podwyższonym poziomem uprawnień.
                -  Typ: **Pomijanie executionpolicy zestawu**. [Dowiedz się więcej](https://technet.microsoft.com/library/ee176961.aspx).

## <a name="create-a-recovery-plan"></a>Tworzenie planu odzyskiwania

Sposób, w którym została utworzona planu odzyskiwania zależy od wdrożenia Odzyskiwanie witryny.

- **Replikacja maszyny wirtualne VMware i serwerów fizycznych**— Tworzenie planu i dodawanie grupy replikacji, które zawierają maszyn wirtualnych i serwerów fizycznych do planu odzyskiwania.
- **Replikacja maszyny wirtualne funkcji Hyper-V (w chmury VMM)**— Tworzenie planu i dodać chronionego maszyn wirtualnych funkcji Hyper-V z chmury VMM do planu odzyskiwania.
- **Replikacja maszyny wirtualne funkcji Hyper-V (nie w chmury VMM)**— Tworzenie planu i dodawanie chronionego maszyn wirtualnych funkcji Hyper-V w grupie ochrona do planu odzyskiwania.
- **Replikacja SAN**— Tworzenie planu i Dodaj grupę replikacji, która zawiera maszyn wirtualnych do planu odzyskiwania. Ponieważ wszystkich maszyn wirtualnych w grupie replikacji musi zakończyć się niepowodzeniem nad razem wybierz grupę replikacji, a nie określonych maszyn wirtualnych (awaria wystąpiła w warstwie miejsca do magazynowania najpierw).


Tworzenie planu odzyskiwania w następujący sposób:

1. Kliknij kartę **Odzyskiwania plany** > **Utwórz Plan odzyskiwania**.
Określ nazwę dla planu odzyskiwania i źródłowej i docelowej. Serwer źródłowy musi mieć maszyn wirtualnych, które są włączone dla awaryjnego i odzyskiwania.

    - Jeśli masz replikacji z VMM do VMM wybierz **Typ źródła** > **VMM**i serwery VMM źródłowej i docelowej. Kliknij pozycję **Funkcji Hyper-V** , aby wyświetlić chmury, które zostały skonfigurowane do używania replice funkcji Hyper-V. 
    - Jeśli masz replikacji z VMM do VMM przy użyciu SAN wybierz **Typ źródła** > **VMM**i serwery VMM źródłowej i docelowej. Kliknij pozycję **SAN** , aby wyświetlić chmury, które zostały skonfigurowane dla replikacji SAN.
    - Jeśli masz replikacji z VMM Azure wybierz **Typ źródła** > **VMM**.  Wybierz serwer źródłowy VMM i **Azure** jako miejsce docelowe.
    - Jeśli masz replikacji z witryny funkcji Hyper-V wybierz **Typ źródła** > **witryny funkcji Hyper-V**. Wybierz witrynę jako źródła i **Azure **jako miejsce docelowe.
    - Jeśli z VMware lub fizycznie lokalnego serwera jest replikacji Azure, wybierz serwer konfiguracji jako źródła i **Azure** jako miejsce docelowe

2. W polu **Wybierz maszyn wirtualnych** wybierz maszyn wirtualnych (lub grupy replikacji), który ma zostać dodany do domyślnej grupy (Grupa 1) w planie odzyskiwania.

## <a name="customize-recovery-plans"></a>Dostosowywanie planów odzyskiwania

Po dodaniu chronione maszyn wirtualnych lub grup replikacji do domyślnej grupy planu odzyskiwania i utworzyć plan, który można dostosować tak:

- **Dodawanie nowych grup**— możesz dodać dodatkowe odzyskiwania plan grupy. Grupy dodawane są numerowane w kolejności ich dodać. Możesz dodać maksymalnie siedem grup. Możesz dodać więcej komputerów lub grup replikacji do tych nowych grup. Należy zauważyć, że maszyn wirtualnych lub grupy replikacji mogą być uwzględniane w grupie plan jeden odzyskiwania.
- **Dodawanie skryptu **— możesz dodać skrypty przed lub po odzyskiwania planowanie grupy. Dodawanie skryptu powoduje dodanie nowego zestawu akcji dla grupy. Na przykład o nazwie zostanie utworzony zestawu czynności wstępne dla grupy 1: 1 grupy: czynności wstępne. Zostaną wyświetlone wszystkie czynności wstępne wewnątrz tego zestawu. Należy zauważyć, że można dodawać tylko skrypt w witrynie podstawowego Jeśli serwer VMM wdrożony.
- **Dodawanie ręcznego akcji**— można dodać ręcznie akcje, które są uruchamiane przed lub po grupy plan odzyskiwania. Po uruchomieniu planu odzyskiwania zatrzymania w miejscu, w którym wstawiono ręcznego akcji i okno dialogowe zapyta zakończenie ręcznego akcji.

## <a name="extend-recovery-plans-with-scripts"></a>Rozszerzanie planów odzyskiwania za pomocą skryptów

Skrypt można dodać do planu odzyskiwania:

- Jeśli masz witryny źródłowej VMM można utworzyć skrypt na serwerze VMM i obejmować planu odzyskiwania.
- Jeśli masz replikacji Azure można zintegrować runbooks Azure automatyzacji planu odzyskiwania

### <a name="create-a-vmm-script"></a>Utworzyć skrypt VMM


Utworzenie skrypt w następujący sposób:

1. Tworzenie nowego folderu w udziale biblioteki, na przykład \<VMMServerName > \MSSCVMMLibrary\RPScripts. Umieszczanie go w źródle i docelowych VMM serwerów.
2. Tworzenie skryptu (na przykład RPScript) i upewnij się, że działa on zgodnie z oczekiwaniami.
3. Skrypt umieszczony w miejscu \<VMMServerName > \MSSCVMMLibrary na serwerach VMM źródłowej i docelowej.

### <a name="create-an-azure-automation-runbook"></a>Tworzenie działań aranżacji Azure automatyzacji

Można rozszerzyć planu odzyskiwania, uruchamiając działań aranżacji Azure automatyzacji w ramach planu. [Dowiedz się więcej](site-recovery-runbook-automation.md).


### <a name="add-custom-settings-to-a-recovery-plan"></a>Dodawanie niestandardowych ustawień do planu odzyskiwania

1. Otwórz planu odzyskiwania, który chcesz dostosować.
2. Kliknij, aby dodać maszyn wirtualnych lub nowej grupy.
3. Aby dodać skrypt lub ręczne akcji kliknij dowolny element na liście **krok** , a następnie kliknij **skrypt** lub **Ręczne akcji**. Określ, czy chcesz dodać skrypt lub akcji przed lub po zaznaczonego elementu. Za pomocą przycisków polecenie **Przenieś w górę** i **Przenieś w dół** , aby zmienić położenie skrypt w górę lub w dół.
4. Jeśli dodajesz skrypt VMM wybierz pozycję **przełączanie awaryjne do skryptu VMM**, a w ścieżce **Skryptu** wpisz ścieżkę względną do udziału. Tak, to na przykładzie której Udostępnij znajduje się w \\ <VMMServerName>\MSSCVMMLibrary\RPScripts, określ ścieżkę: \RPScripts\RPScript.PS1.
5. Jeśli dodajesz Azure automatyzacji Uruchamianie księgi określanie **Konta na platformie Azure automatyzacji** , w którym znajduje się działań aranżacji, a następnie wybierz odpowiedni **Skrypt działań aranżacji Azure**.
5. Wykonaj trybie awaryjnym planu odzyskiwania, aby upewnić się, że skrypt działa zgodnie z oczekiwaniami.


## <a name="next-steps"></a>Następne kroki

Różne rodzaje praca awaryjna można uruchamiać na planu odzyskiwania, w tym awarię test na sprawdzanie środowiska i praca awaryjna planowanego lub niezaplanowane. Aby [uzyskać więcej informacji](site-recovery-failover.md).


 
