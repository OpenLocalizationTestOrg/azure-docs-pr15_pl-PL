<properties
    pageTitle="Replikacja funkcji Hyper-V Odzyskiwanie witryny Azure | Microsoft Azure"
    description="Aby zrozumieć pojęcia techniczne, które ułatwiają pomyślnie instalowanie, konfigurowanie i zarządzanie Odzyskiwanie witryny Azure za pomocą w tym artykule."
    services="site-recovery"
    documentationCenter=""
    authors="Rajani-Janaki-Ram"
    manager="mkjain"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="09/12/2016"
    ms.author="rajanaki"/>  


# <a name="hyper-v-replication-with-azure-site-recovery"></a>Replikacja funkcji Hyper-V Odzyskiwanie witryny Azure

W tym artykule opisano pojęcia techniczne, które ułatwiają pomyślnie skonfigurować i zarządzanie witryną funkcji Hyper-V lub w witrynie System Center Virtual Machine Manager (VMM) do ochrony Azure za pomocą Odzyskiwanie witryny Azure.

## <a name="setting-up-the-source-environment-for-replication-between-on-premises-and-azure"></a>Konfigurowanie środowiska źródła replikacji między wdrożeniem lokalnym a Azure

W ramach konfigurowania awarii między wdrożeniem lokalnym a Azure upewnij się pobrać i zainstalować na serwerze VMM Azure witryny odzyskiwania dostawcy. Instalowanie agenta usługi Azure odzyskiwania na każdym hoście funkcji Hyper-V.

![Wdrażanie witryny VMM replikacji między wdrożeniem lokalnym a Azure](media/site-recovery-understanding-site-to-azure-protection/image00.png)

Konfigurowania środowiska źródłowego w infrastrukturze funkcji Hyper-V zarządzane jest podobne do konfigurowania środowiska źródła w infrastrukturze zarządzanych VMM. Jedyna różnica polega na tym, że dostawcy i agenta są zainstalowane na host funkcji Hyper-V. W środowisku VMM dostawca jest zainstalowany na serwerze VMM i agent jest zainstalowany na hosts funkcji Hyper-V (w przypadku replikacji Azure).

## <a name="workflows"></a>Przepływy pracy

### <a name="enable-protection"></a>Włącz ochronę
Po wprowadzeniu ochrony maszyny wirtualnej z Azure portal lub lokalnego, zostanie uruchomiony zadanie Odzyskiwanie witryny o nazwie **włączyć ochronę** . Na karcie **zadania** można monitorować go.

!["Włącz ochronę" zadania na liście zadań](media/site-recovery-understanding-site-to-azure-protection/image001.PNG)

Wymagania wstępne sprawdza zadania **Włącz ochronę** przed wywoływanie metody [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx) . Ta metoda tworzy replikacji Azure za pomocą danych wejściowych, skonfigurowane podczas ochrony.

**Włącz ochronę** zadanie rozpoczyna się replikacji początkowej ze źródeł lokalnych od wywoływanie metody [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx) . Ta metoda wysyła dysków wirtualnych maszyny wirtualnej Azure.

![Szczegóły zadania "Włącz ochronę"](media/site-recovery-understanding-site-to-azure-protection/IMAGE002.PNG)

### <a name="finalize-protection-on-the-virtual-machine"></a>Finalizowanie ochronę komputera wirtualnych
Podczas replikacji początkowej zostanie wywołana, jest pobierana [migawki maszyn wirtualnych funkcji Hyper-V](https://technet.microsoft.com/library/dd560637.aspx) . Wirtualnych dyskach twardych są przetwarzane po jednym, aż wszystkie dyski są przekazywane do Azure. Zwykle trwa to trochę czasu, aby zakończyć, na podstawie rozmiaru dysku i przepustowość. Aby zoptymalizować wykorzystanie Twojej sieci, zobacz [jak zarządzać lokalnego do wykorzystania przepustowości sieci Azure ochrony](https://support.microsoft.com/kb/3056159).

Po zakończeniu replikacji początkowej zadania **Finalize ochronę komputera wirtualnych** konfiguruje ustawienia sieci i po replikacji. Gdy trwa replikacji początkowej:

- Wszystkie zmiany na dyskach są śledzone. 
- Ilość dodatkowego miejsca do magazynowania jest zużyte migawki i plików dziennika replice funkcji Hyper-V (HRL).

Po zakończeniu replikacji początkowej zostanie usunięty migawki maszyn wirtualnych funkcji Hyper-V. To usunięcie powoduje scalanie zmian danych po początkowej replikacji na dysku nadrzędnego.

!["Finalizowanie ochronę komputera wirtualnych" zadania na liście zadań](media/site-recovery-understanding-site-to-azure-protection/image03.png)

### <a name="delta-replication"></a>Zmiana replikacji
Śledzenie replikacji replice funkcji Hyper-V, który jest częścią aparat replikacji funkcji Hyper-V replice, ścieżek zmiany do wirtualnego dysku twardego jako pliki dziennika replice funkcji Hyper-V (*.hrl). HRL pliki znajdują się w tym samym katalogu co skojarzone dysków.

Każdego dysku, który jest skonfigurowany do replikacji ma skojarzony plik HRL. Dziennik są wysyłane do miejsca do magazynowania klienta po zakończeniu replikacji początkowej. Gdy dziennik znajduje się w drodze Azure, zmiany w podstawowych są śledzone w innym pliku dziennika, w tym samym katalogu.

Podczas początkowej replikacji lub różnicy można monitorować stanu replikacji maszyn wirtualnych w widoku maszyn wirtualnych wymieniony w [Monitorowanie kondycji replikacji dla maszyn wirtualnych](./site-recovery-monitoring-and-troubleshooting.md#monitor-replication-health-for-virtual-machine).  

### <a name="resynchronization"></a>Ponowne synchronizowanie
Maszyny wirtualnej zostało oznaczone do ponowne synchronizowanie, gdy zarówno replikacji różnicy kończy się niepowodzeniem i replikacji początkowej pełnej będzie kosztów pod względem przepustowości sieci lub czasu. Na przykład gdy rozmiar pliku HRL zbiory 50 procent całkowitego rozmiaru dysku, maszyny wirtualnej zostało oznaczone do ponowne synchronizowanie. Ponowne synchronizowanie minimalizuje ilość danych przesyłania przez sieć obliczanie sum kontrolnych dysków maszyn wirtualnych źródłowej i docelowej i wysyłając tylko różnicy.

Po zakończeniu pracy ponowne synchronizowanie replikacji normalny różnicy należy wznowić. Możesz wznowić ponowne synchronizowanie awaria sieci lub innej awarii.

Domyślnie zaplanowane automatycznie ponowna synchronizacja jest skonfigurowany do wystąpić poza godzin pracy. Jeśli maszyny wirtualnej musi być zsynchronizowane ręcznie, wybierz maszyny wirtualnej z portalem i kliknij **ponownie zsynchronizować**.

![Ponowne ręczne synchronizowanie](media/site-recovery-understanding-site-to-azure-protection/image04.png)

Ponowne synchronizowanie używa algorytmu segmentu o stałej blok miejsce, w którym pliki źródłowej i docelowej są podzielone na stałe fragmentów. Sumy kontrolne dla każdego fragmentu są generowane i porównywane w celu określenia, która blokuje ze źródła mają być stosowane do elementu docelowego.

### <a name="retry-logic"></a>Ponów próbę logicznych
Ma wbudowane ponów próbę logicznych błędy replikacji. Tę logikę można podzielić na dwie kategorie:

| Kategoria                  | Scenariusze                                    |
|---------------------------|----------------------------------------------|
| Użytkownicy niebędący odzyskania błędu     | Nie ponów próbę jest podejmowana próba nawiązania. Stan replikacji maszyn wirtualnych jest **krytyczne**i wymagana jest interwencja administratora. Przykłady: <ul><li>Przerwane łańcuch wirtualnego dysku twardego</li><li>Nieprawidłowy stan maszyny wirtualnej replice</li><li>Błąd uwierzytelniania sieciowego</li><li>Błąd autoryzacji</li><li>Maszyny wirtualnej, która nie została odnaleziona, w przypadku serwera autonomicznego funkcji Hyper-V</li></ul>|
| Odwracalny błąd         | Wykonywane co interwał replikacji, przy użyciu wykładniczego wstecz wyłączenia który zwiększa interwał ponawiania od początku pierwszej próby (1, 2, 4, 8, 10 minut). Jeśli błąd będzie nadal występował, spróbuj co 30 minut. Przykłady: <ul><li>Błąd sieciowy</li><li>Brak miejsca na dysku</li><li>Warunek małą ilością pamięci</li></ul>|

## <a name="hyper-v-virtual-machine-protection-and-recovery-life-cycle"></a>Cykl życia ochrony i odzyskiwania maszyn wirtualnych funkcji Hyper-V

![Cykl życia ochrony i odzyskiwania maszyn wirtualnych funkcji Hyper-V](media/site-recovery-understanding-site-to-azure-protection/image05.png)

## <a name="other-references"></a>Pozostałe odwołania

- [Monitorowanie i rozwiązywanie problemów z ochroną VMware, VMM, funkcji Hyper-V i fizycznym witryn](./site-recovery-monitoring-and-troubleshooting.md)
- [Możliwość kontaktowania się z for Microsoft Support](./site-recovery-monitoring-and-troubleshooting.md#reaching-out-for-microsoft-support)
- [Typowe błędy Odzyskiwanie witryny Azure i ich rozwiązania](./site-recovery-monitoring-and-troubleshooting.md#common-asr-errors-and-their-resolutions)
