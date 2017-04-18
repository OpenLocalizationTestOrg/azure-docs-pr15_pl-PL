<properties
   pageTitle="Rozwiązywanie problemów z wolno kopia zapasowa plików i folderów w narzędziu Azure kopia zapasowa | Microsoft Azure"
   description="Ten artykuł zawiera wskazówki dotyczące rozwiązywania problemów ułatwiają diagnozowanie przyczyn występowania problemów z wydajnością kopii zapasowej Azure"
   services="backup"
   documentationCenter=""
   authors="genlin"
   manager="jimpark"
   editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="genli"/>

# <a name="troubleshoot-slow-backup-of-files-and-folders-in-azure-backup"></a>Rozwiązywanie problemów z wolniej kopia zapasowa plików i folderów w kopii zapasowej Azure

Ten artykuł zawiera wskazówki dotyczące rozwiązywania problemów pomagające określić spowodować spadek wydajności kopii zapasowych dotyczące plików i folderów podczas korzystania z kopii zapasowej Azure. Gdy wykonywać kopie zapasowe plików za pomocą agenta kopii zapasowej Azure, wykonywania kopii zapasowej może potrwać dłużej niż oczekiwano. To opóźnienie może być spowodowane co najmniej jedną z następujących czynności:

-   [Istnieje gardła wydajności na komputerze, na którym jest teraz kopię zapasową.](#cause1)
-   [Inny proces lub oprogramowanie antywirusowe jest zaburzania proces Azure kopii zapasowej.](#cause2)
-   [Agent kopii zapasowej nie działa na Azure maszyn wirtualnych (maszyn wirtualnych).](#cause3)  
-   [W przypadku wykonywania kopii zapasowych dużą liczbą plików (miliony).](#cause4)

Przed rozpoczęciem rozwiązywania problemów, firma Microsoft zaleca pobranie i zainstalowanie [najnowszej agent kopii zapasowej Azure](http://aka.ms/azurebackup_agent). Udzielamy częste aktualizacje do agenta kopii zapasowej Rozwiązywanie problemów dotyczących różnych, dodawanie funkcji i zwiększyć wydajność.

Również zalecamy zapoznanie się [Usługa Azure kopii zapasowych — często zadawane pytania](backup-azure-backup-faq.md) , aby upewnić się, że nie występują wymienionych typowe problemy z konfiguracją.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

<a id="cause1"></a>
## <a name="cause-performance-bottlenecks-on-the-computer"></a>Przyczyna: Gardła wydajności na komputerze

Utrudnień na komputerze, na którym jest teraz kopię zapasową mogą powodować opóźnienia. Na przykład komputera możliwość odczytu lub zapisu na dysku lub przepustowość wysyłanie danych z sieci, mogą powodować gardła.

System Windows udostępnia wbudowane narzędzia, które jest określana mianem [Monitora wydajności](https://technet.microsoft.com/magazine/2008.08.pulse.aspx) (monitora) wykrywanie tych gardła.

Poniżej przedstawiono niektóre liczniki wydajności i zakresy, które mogą być pomocne w diagnozowania gardeł kopii zapasowych optymalnych.

| Licznik  | Stan  |
|---|---|
|Dysk logiczny (dysk fizyczny) — % bezczynne   | • 100% idle o 50% bezczynne = dobrej kondycji</br>• 49% idle 20% bezczynne = ostrzeżenia lub monitora</br>• 19% idle na 0% bezczynne = krytyczne lub poza Specyfikacja|
|  Dysk logiczny (dysk fizyczny) — średni % S odczytu lub zapisu na dysku |  • 0,001 ms 0,015 MS = dobrej kondycji</br>• 0,015 ms 0,025 MS = ostrzeżenia lub monitora</br>• 0.026 ms lub już = krytyczne lub poza Specyfikacja|
|  Dysk logiczny (dysk fizyczny) — bieżąca długość kolejki dysku (dla wszystkich wystąpień) | 80 żądań więcej niż 6 minut |
| Pamięć — nie pula stronicowana bajtów|• Mniej niż 60% puli zużyte = dobrej kondycji<br>• 61 do 80% puli zużyte = ostrzeżenia lub monitora</br>• Większe niż 80% puli zużyte = krytyczne lub poza Specyfikacja|
| Pamięć — Bajty puli stronicowanej |• Mniej niż 60% puli zużyte = dobrej kondycji</br>• 61 do 80% puli zużyte = ostrzeżenia lub monitora</br>• Większe niż 80% puli zużyte = krytyczne lub poza Specyfikacja|
| Pamięć — dostępne megabajtów| • 50% pamięci lub bardziej = dobrej kondycji</br>• 25% pamięci = monitora</br>• 10% pamięci = ostrzeżenia</br>• Mniej niż 100 MB lub 5% pamięci = krytyczne lub poza Specyfikacja|
|Procesor —\%czasu procesora (wszystkie wystąpienia)|• Mniej niż 60% zużyte = dobrej kondycji</br>• od 61 do 90% zużyte = Monitor lub ostrzeżenia</br>• od 91 do 100% zużyte = krytyczne|


> [AZURE.NOTE] Jeśli okaże się, że infrastruktury jest dziedziczonej z istotnymi elementami, zalecamy defragmentowania dysków regularnie w celu zwiększenia wydajności.

<a id="cause2"></a>
## <a name="cause-another-process-or-antivirus-software-interfering-with-azure-backup"></a>Przyczyna: Inny proces lub oprogramowanie antywirusowe zaburzania kopii zapasowej Azure

Firma Microsoft zobaczył kilka wystąpień miejsce, w którym inne procesy w systemie Windows mieć negatywny wpływ wydajność procesu agenta kopii zapasowej Azure. Na przykład jeśli używasz zarówno agent Azure Backup, jak i innego programu do tworzenia kopii zapasowych danych lub jeśli oprogramowanie antywirusowe jest uruchomiony i zablokował pliki do wykonania kopii zapasowej, wielokrotności blokuje na pliki może spowodować konfliktu. W tej sytuacji kopii zapasowej może się nie powieść lub zadanie może potrwać dłużej niż oczekiwano.

Najlepsze w tym scenariuszu zaleca się wyłączyć drugi program kopii zapasowej, aby sprawdzić, czy wykonywania kopii zapasowej dla agenta kopii zapasowej Azure zmieni. Zazwyczaj upewniając się, że wielu zadań kopii zapasowej nie są uruchomione w tym samym czasie jest wystarczające, aby zapobiec ich siebie.

Programy antywirusowe zaleca się wykluczenie następujących plików i lokalizacji:

- C:\Program Files\Microsoft Azure odzyskiwania usług Agent\bin\cbengine.exe jako proces
- C:\Program Files\Microsoft Azure odzyskiwania usług Agent\ folderów
- Zapasowej lokalizację (Jeśli nie używasz standardowej lokalizacji)

<a id="cause3"></a>
## <a name="cause-backup-agent-running-on-an-azure-virtual-machine"></a>Przyczyna: Agent Backup uruchomionych Azure maszyn wirtualnych

Jeśli korzystasz z agentem kopii zapasowej na maszyny, wydajność będzie mniejsza niż po uruchomieniu go na komputerze fizycznym. Jest to planowane z powodu ograniczeń operacji i/o na SEKUNDĘ.  Jednak możesz optymalizować działanie, możesz je przełączyć dysków z danymi, które są teraz kopię zapasową do magazynu Premium Azure. Pracujemy nad rozwiązywanie tego problemu, a poprawki będą dostępne w przyszłej wersji.

<a id="cause4"></a>
## <a name="cause-backing-up-a-large-number-millions-of-files"></a>Przyczyna: Wykonywanie kopii zapasowej dużą liczbą plików (miliony)

Przenoszenie dużej ilości danych trwa dłużej niż przenoszenia mniejsze ilości danych. W niektórych przypadkach wykonywania kopii zapasowej dotyczy nie tylko rozmiar danych, ale również liczbę plików lub folderów. Jest to zwłaszcza wtedy, gdy tworzona jest kopia zapasowa miliony małych plików (kilka bajtów do kilku kilobajtów).

Dzieje się tak, ponieważ jest kopia zapasowa danych i przeniesienie go do Azure, Azure jest jednocześnie katalogowaniu plików. W niektórych scenariuszach rzadkich operację wykazu może potrwać dłużej niż oczekiwano.

Następujących wskaźników mogą ułatwić zrozumienie gardło i w związku z tym pracować nad następne kroki:

- **Interfejs użytkownika jest wyświetlany postęp aby transmisja danych**. Dane są nadal przesyłane. Przepustowość sieci lub rozmiar danych może powodować opóźnienia.

- **Interfejs użytkownika nie jest wyświetlany postęp przesyłania danych**. Otwórz Dzienniki znajdują się w C:\Microsoft Azure odzyskiwania usług Agent\Temp, a następnie zaznacz wpis FileProvider::EndData w dzienniku. Ten wpis oznacza, że zakończone transfer danych i operacji wykazu się dzieje. Nie Anuluj zadania kopii zapasowej. Zamiast tego poczekaj nieco już na zakończenie operacji wykazu. Jeśli problem będzie nadal występował, skontaktuj się z [Azure obsługi](https://portal.azure.com/#create/Microsoft.Support).
