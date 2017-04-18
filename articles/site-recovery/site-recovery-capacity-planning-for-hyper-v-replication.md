<properties
    pageTitle="Uruchom narzędzie funkcji Hyper-V zdolności terminarz odzyskiwania witryny | Microsoft Azure"
    description="Ten artykuł zawiera instrukcje dotyczące korzystania z funkcji Hyper-V zdolności terminarz narzędzia odzyskiwania witryny Azure"
    services="site-recovery"
    documentationCenter="na"
    authors="rayne-wiselman"
    manager="jwhit"
    editor="" />
<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="07/12/2016"
    ms.author="raynew" />

# <a name="run-the-hyper-v-capacity-planner-tool-for-site-recovery"></a>Uruchom narzędzie funkcji Hyper-V zdolności terminarz odzyskiwania witryny

W ramach wdrożenia Odzyskiwanie witryny Azure musisz ustalanie replikacji i wymagania dotyczące przepustowości usługi. Narzędzie terminarz wydajność funkcji Hyper-V dla Odzyskiwanie witryny ułatwia ustalanie wymagań replikacji i przepustowość replikacji maszyn wirtualnych funkcji Hyper-V.


W tym artykule opisano, jak uruchomić narzędzie terminarz wydajność funkcji Hyper-V. To narzędzie powinna być używana razem z innych narzędzi planowania wydajności i informacji opisanych w [planowaniu odzyskiwania witryny](site-recovery-capacity-planner.md).


## <a name="before-you-start"></a>Przed rozpoczęciem

Węzeł serwer lub klaster funkcji Hyper-V w witrynie podstawowego uruchomić narzędzie. Aby uruchomić narzędzie Serwery hosta funkcji Hyper-V musi:

- System operacyjny: Windows Server® 2012 lub Windows Server® 2012 R2
- Pamięć: 20 MB (minimum)
- Procesor: 5 procent ogólnych (minimum)
- Miejsce na dysku: 5 MB (minimum)

Przed uruchomieniem narzędzia należy przygotować podstawowej witryny. Jeśli masz replikacji między dwoma lokalnej witryny ma zostać sprawdzony przepustowości, będzie potrzebne do przygotowania serwer replice także.


## <a name="step-1-prepare-the-primary-site"></a>Krok 1: Przygotowanie podstawowej witryny
1. Na stronie podstawowego utworzyć listę wszystkich funkcji Hyper-V maszyn wirtualnych, które mają być replikowane i funkcji Hyper-V hosts i klastrów na których są one umieszczone. Narzędzie można uruchamiać zawsze dla wielu hostów autonomicznej lub jeden klaster, ale nie obu razem. Należy również uruchamianie oddzielnie dla każdego systemu operacyjnego, należy zebrać i zanotuj serwery funkcji Hyper-V w następujący sposób:

  - Windows Server® 2012 autonomicznych serwerów
  - Klastrów systemu Windows Server® 2012
  - Windows Server® 2012 R2 autonomicznych serwerów
  - Windows Server® 2012 R2 klastrów

3. Włączanie dostępu zdalnego do WMI we wszystkich funkcji Hyper-V hosts i klastrów. Polecenia na każdym serwerze/klastrze, aby upewnić się, że reguły zapory i są ustawione uprawnienia użytkownika:

        netsh firewall set service RemoteAdmin enable

5. Włącz monitorowanie wydajności na serwerach i klastrów w następujący sposób:

  - Otwórz Zaporę systemu Windows z **Zabezpieczeniami zaawansowanymi** przystawki, a następnie włączyć następujące reguły przychodzące: **COM + dostęp do sieci (DCOM-IN)** i wszystkie reguły w **grupie zdalnego zarządzania dziennika zdarzeń**.

## <a name="step-2-prepare-a-replica-server-on-premises-to-on-premises-replication"></a>Krok 2: Przygotowywanie serwera replice (lokalnego do lokalnego replikacji)

Nie musisz to zrobić, jeśli masz replikacji Azure.

Zaleca się, że skonfigurowano jednego hosta funkcji Hyper-V jako serwera odzyskiwania tak, aby fikcyjna maszyn wirtualnych można replikować do go, aby sprawdzić przepustowości.  Możesz pominąć ten, ale nie będą mogli zmierzyć przepustowości dopiero po wykonaniu.

1. Jeśli chcesz użyć węzła replice Konfigurowanie funkcji Hyper-V replice brokera:

    - W oknie **Menedżer serwera**Otwórz **Menedżera klaster pracy awaryjnej**.
    - Połącz się z klastrem, wyróżnij nazwę klaster i kliknij pozycję **Akcje** > **Konfigurowanie roli** , aby otworzyć Kreatora wysokiej dostępności.
    - **Wybierz rolę** kliknij **Brokera replice funkcji Hyper-V**. W kreatorze podaj **nazwę NetBIOS** oraz **adres IP** , może być używany jako punkt połączenia z klastrem (nazywanych punkt dostępu klienta). **Broker replice funkcji Hyper-V** zostaną skonfigurowane, uzyskując nazwę punktu dostępu klienta, który należy pamiętać.
    - Sprawdź, czy roli Broker replice funkcji Hyper-V pomyślnie przechodzi do trybu online i może się nie powieść nad między wszystkie węzły klaster. Aby to zrobić, kliknij prawym przyciskiem myszy rolę, wskaż **Przejście**, a następnie kliknij **Wybierz węzeł**. Wybierz węzeł > **OK**.
    - Jeśli korzystasz z uwierzytelniania na podstawie certyfikatu, upewnij się, każdego węzła i uzyskać dostęp do punktu, że wszyscy mają zainstalowany certyfikat klienta.
2.  Włączanie serwera replice:

    - Dla klastrów, otwórz Menedżera klaster błąd, połącz się z klastrem i kliknij pozycję **role** > Wybierz roli > **Ustawienie replikacji**s > **Włącz ten klaster jako serwer replice**. Uwaga Jeśli używasz klastrze jako replice konieczne będzie mają Prezentuj ról brokera replice funkcji Hyper-V w klastrze w także podstawowej witryny.
    - Dla serwera autonomicznego Otwórz Menedżera funkcji Hyper-V. W okienku **Akcje** kliknij pozycję **Ustawienia funkcji Hyper-V** na serwerze, który chcesz włączyć, a w **Konfiguracji replikacji** kliknij opcję **Włącz ten komputer jako serwer replice**.
3. Konfigurowanie uwierzytelniania:

    - W **porty uwierzytelniania i** wybierz sposób uwierzytelniania podstawowego serwera i porty uwierzytelniania. Jeśli używasz certyfikatu kliknij przycisk **Wybierz certyfikat** , aby wybrać jedną. Użyj protokołu Kerberos, jeśli hosts funkcji Hyper-V podstawowego i odzyskiwania znajdują się w tej samej domeny lub domen zaufanych. Za pomocą certyfikatów dla różnych domenach lub wdrażania w grupie roboczej.
    - W sekcji **autoryzacji i miejsca do magazynowania** Zezwalaj **dowolnego** uwierzytelnionego serwera (podstawowe) wysyłanie replikacji danych do tego serwera replice. Kliknij **przycisk OK** lub **Zastosuj**.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image1.png)

    - Uruchom **netsh http Pokaż servicestate** Sprawdź, czy odbiornik jest uruchomiony dla protokół/port określonej:  
4. Konfigurowanie zapory. Podczas instalacji funkcji Hyper-V reguły zapory są tworzone do zezwalanie na ruch na domyślny porty (HTTPS na 443, Kerberos na 80). Włącz tych reguł w następujący sposób:

        - Certificate authentication on cluster (443): **Get-ClusterNode | ForEach-Object {Invoke-command -computername \$\_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTPS Listener (TCP-In)"}}**
        - Kerberos authentication on cluster (80): **Get-ClusterNode | ForEach-Object {Invoke-command -computername \$\_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTP Listener (TCP-In)"}}**
        - Certificate authentication on standalone server: **Enable-Netfirewallrule -displayname "Hyper-V Replica HTTPS Listener (TCP-In)"**
        - Kerberos authentication on standalone server: **Enable-Netfirewallrule -displayname "Hyper-V Replica HTTP Listener (TCP-In)"**

## <a name="step-3-run-the-capacity-planner-tool"></a>Krok 3: Uruchom narzędzie do planowania pojemności

Po utworzeniu przygotować witryny podstawowy i konfigurowanie serwera odzyskiwania można uruchomić narzędzie.

1. [Pobierz](https://www.microsoft.com/download/details.aspx?id=39057) narzędzie z Center Download firmy Microsoft.
2. Uruchom narzędzie z jednej z serwery podstawowe (lub jeden z węzłów z podstawowego klaster). Kliknij prawym przyciskiem myszy plik .exe, a następnie wybierz **polecenie Uruchom jako administrator**.
3. W **przed rozpoczęciem** Określ, jak długo ma zbieranie danych. Zalecamy, uruchom narzędzie godzinach produkcji, aby upewnić się, że dane są przedstawiciela. Jeśli próbujesz tylko do sprawdzania poprawności połączeń sieciowych, można zebrać tylko przez minutę.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image2.png)

4. W **podstawowej witryny szczegóły** określić nazwę serwera lub nazwy FQDN hosta autonomicznej lub w klastrze Określ nazwę FQDN klienta zaakceptować punkt, nazwę klaster lub dowolnego węzła w klastrze, a następnie kliknij przycisk **Dalej**. Narzędzie automatycznie wykrywa nazwę serwera, który jest uruchomiony na. Narzędzie przejmuje maszyny wirtualne, które można monitorować dla wybranych serwerów.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image3.png)

5. W **Replice szczegóły witryny** jest replikacji Azure lub jeśli masz replikacji pomocniczej centrum danych, a nie skonfigurowano serwera replice wybierz **pominąć testy witryny replice**. Jeśli są replikacji pomocniczej centrum danych i masz skonfigurowany typ replice w nazwie FQDN serwera autonomicznego lub punkt dostępu klienta dla klaster **Nazwa serwera (**lub) zakończenie brokera replice funkcji Hyper-V.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image4.png)

6. **Rozszerzone** informacje dotyczące replice włączyć **pominąć testy witryny rozszerzonego replice**. Ta osoba nie jest obsługiwana przez Odzyskiwanie witryny.
7. W **Wybierz pozycję pośrednictwem SMS do skopiowanymi** narzędzia nawiązuje połączenie z serwerem lub klaster i wyświetla maszyny wirtualne i dyskach na serwerze podstawowego, zgodnie z ustawieniami określonej na stronie **Szczegóły witryny podstawowy** . Należy zauważyć, że nie jest uruchomiony maszyny wirtualne, które już są włączone dla replikacji lub że nie będą wyświetlane. Wybierz maszyny wirtualne, dla których chcesz zebrać metryki. Automatyczne zaznaczanie wirtualnych dysków twardych zbiera dane dla maszyny wirtualne zbyt.
9. Jeśli skonfigurowano serwer replice lub klaster w **informacji dotyczących sieci** Określ przybliżonego przepustowości sieci WAN, które Twoim zdaniem będą używane między witrynami podstawowego i replice i wybierz pozycję certyfikaty, jeśli skonfigurowano uwierzytelnianie na podstawie certyfikatu.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image5.png)

10. **Podsumowanie** Sprawdź ustawienia i kliknij przycisk **Dalej** do rozpoczęcia zbierania metryki. Narzędzie postęp i stan jest wyświetlany na stronie **Obliczanie pojemności** . Po zakończeniu narzędzia do uruchamiania kliknij pozycję **Wyświetl raport** , aby zacząć wynik. Domyślnie dzienniki i raporty są przechowywane w **%systemdrive%\Users\Public\Documents\Capacity terminarz**.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image6.png)


## <a name="step-4-interpret-the-results"></a>Krok 4: Interpretowania wyników
Poniżej przedstawiono ważne metryki. Możesz zignorować metryk, które nie są wymienione tutaj. Nie są odpowiednie dla Odzyskiwanie witryny.

### <a name="on-premises-to-on-premises-replication"></a>Lokalne do lokalnego replikacji
  - Wpływ replikacji na hoście podstawowych obliczeń, pamięci
  - Wpływ replikacji na podstawową, hosts odzyskiwania miejsca na dysku miejsca do magazynowania, operacji i/o na SEKUNDĘ
  - Całkowita przepustowość wymagane dla replikacji delta (MB/s)
  - Przepustowość sieci obserwowanych między podstawowy host i hosta odzyskiwania (MB/s)
  - Sugestie dotyczące idealny liczba aktywne transferów równoległe między dwoma hosts i klastrów

### <a name="on-premises-to-azure-replication"></a>Lokalne Azure replikacji
  - Wpływ replikacji na hoście podstawowych obliczeń, pamięci
  - Wpływ replikacji podstawowy host miejsca do magazynowania miejsca na dysku, operacji i/o na SEKUNDĘ
  - Całkowita przepustowość wymagane dla replikacji delta (MB/s)

## <a name="more-resources"></a>Więcej zasobów

- Aby uzyskać szczegółowe informacje o narzędziu czytanie dokumentu, który towarzyszy pobierania narzędzia.
- Obejrzyj skorzystać z narzędzia Mayer Jan [blogu w witrynie TechNet](http://blogs.technet.com/b/keithmayer/archive/2014/02/27/guided-hands-on-lab-capacity-planner-for-windows-server-2012-hyper-v-replica.aspx).
- [Uzyskiwanie wyników](site-recovery-performance-and-scaling-testing-on-premises-to-on-premises.md) z naszych testowania lokalnego do lokalnego funkcji Hyper-V replikacji



## <a name="next-steps"></a>Następne kroki

Po zakończeniu planowania pojemności możesz rozpocząć wdrażanie Odzyskiwanie witryny:

- [Powielić maszyny wirtualne funkcji Hyper-V w chmury VMM Azure](site-recovery-vmm-to-azure.md)
- [Powielić maszyny wirtualne funkcji Hyper-V (bez VMM) Azure](site-recovery-hyper-v-site-to-azure.md)
- [Powielić maszyny wirtualne funkcji Hyper-V między witrynami VMM](site-recovery-vmm-to-vmm.md)
- [Powielić maszyny wirtualne funkcji Hyper-V między witrynami VMM z SAN](site-recovery-vmm-san.md)
- [Powielić funkcji hyper-V maszyny wirtualne na jednym serwerze VMM](site-recovery-single-vmm.md)