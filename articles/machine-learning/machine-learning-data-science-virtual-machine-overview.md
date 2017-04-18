<properties
    pageTitle="Co to jest maszyny wirtualnej nauki danych? | Microsoft Azure"
    description="Dowiedz się, kluczowe scenariusze, funkcje i jak rozpocząć pracę z danymi nauki maszyn wirtualnych, środowiska i narzędzi gotowy do analizy."
    keywords="narzędzia do nauki danych, maszyn wirtualnych nauki danych, narzędzia do nauki danych, linux danych naukowych"
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="bradsev" />


# <a name="introduction-to-the-cloud-based-data-science-virtual-machine-for-linux-and-windows"></a>Wprowadzenie do opartej na chmurze danych nauki maszyny wirtualnej dla systemu Windows i Linux

Maszyny wirtualnej nauki danych jest dostosowany obraz maszyn wirtualnych na firmy Microsoft w chmurze Azure opracowane pod kątem wykonując nauki danych. Ma wiele popularnych danych naukowych i innych narzędzi wstępnie zainstalowany i wstępnie skonfigurowane do błyskawicznego rozpoczynania prac tworzenia inteligentnego aplikacji dla zaawansowanej analizy. Jest ona dostępna w systemie Windows Server 2012 lub wersjach OpenLogic 7.2 CentOS systemem Linux. 

W tym temacie w tym artykule omówiono, co można zrobić z maszyn wirtualnych nauki danych, przedstawiono niektóre kluczowe scenariusze dotyczące korzystania z maszyn wirtualnych itemizes kluczowe funkcje dostępne w wersji systemu Windows i Linux i instrukcje dotyczące rozpocząć korzystanie z nich.


## <a name="what-can-i-do-with-the-data-science-virtual-machine"></a>Co można zrobić z maszyny wirtualnej nauki danych?

Cel maszyny wirtualnej nauki danych jest zapewnienie specjalistów danych na wszystkich poziomach umiejętności oraz role ze środowiskiem nauki dane tarcia. Ten maszyn wirtualnych zapisuje wiele czasu, który chcesz poświęcać Jeśli środowisku porównywalna ma rozwijane własne. Zamiast tego Rozpocznij projektu nauki danych bezpośrednio w nowo utworzonych wystąpieniu maszyn wirtualnych. 

Maszyn wirtualnych nauki danych jest przeznaczona i skonfigurowany do pracy z scenariusze szeroki użycia. Środowiska można skalować w górę lub dół potrzeb projektu. Mogą być preferowany język do zadań nauki danych programu. Można zainstalować inne narzędzia i dostosować do potrzeb dokładnie system.
 
## <a name="key-scenarios"></a>Kluczowe scenariusze
W tej części podano niektóre kluczowe scenariusze, dla których mogą być rozmieszczone maszyn wirtualnych nauki danych.

### <a name="preconfigured-analytics-desktop-in-the-cloud"></a>Pulpit wstępnie analizy w chmurze

Maszyn wirtualnych nauki danych zawiera konfiguracji według planu bazowego dla zespołów nauki danych chcesz zastąpić pulpitami lokalne pulpitu chmury zarządzanych. Tego planu bazowego gwarantuje, że wszystkie naukowców danych w zespole spójne ustawienia umożliwiające weryfikowanie doświadczeń i promowanie współpracy. Ponadto obniża koszty zmniejszenia obciążenia grupy i zapisywania na czas potrzebny do oceny, instalowanie i obsługa różnych pakietów oprogramowania wymagane do wykonania zaawansowanej analizy.  

### <a name="data-science-training-and-education"></a>Szkolenie dotyczące nauki danych i instytucji edukacyjnych

Instruktorów przedsiębiorstwa i nauczycielom nauki który nauki danych klas udostępnia zwykle obraz maszyn wirtualnych, aby zapewnić spójne ustawienia swoich uczniów i właściwie pracy próbki. Maszyn wirtualnych nauki danych tworzy środowisku na żądanie instalację spójne, który ułatwia problemach niezgodności i pomocy technicznej. Miejsce, w którym tych środowiskach muszą być wbudowane często, szczególnie w przypadku krótszej spotkań szkoleniowych, przypadkach korzystać znacznie.

### <a name="on-demand-elastic-capacity-for-large-scale-projects"></a>Elastyczne możliwości dużych projektów na żądanie

Dane nauki hackathons zawodach lub modelowanie danych na dużą skalę i badań wymagają skalowania wydajność sprzętu zwykle przez krótki czas trwania. Maszyn wirtualnych nauki danych może ułatwić powielić środowiska nauki danych szybko na żądanie, na serwerach skalować się, umożliwiające doświadczeń Wymaganie silnych zasobów komputerowych do uruchomienia.

### <a name="short-term-experimentation-and-evaluation"></a>Krótkoterminowy eksperyment i oceny

Maszyn wirtualnych nauki danych mogą być używane do oceny lub więcej narzędzi, takich jak Microsoft R Server, SQL Server, narzędzia Visual Studio, Jupyter głębokich nauki / ML narzędzi i nowych narzędzi popularnych we Wspólnocie z minimalnymi konfiguracji nakładu. Ponieważ maszyn wirtualnych nauki danych można szybko skonfigurować, mogą być stosowane w innych krótkoterminowy scenariuszach zastosowania, takich jak replikacji opublikowanych doświadczeń, wykonywanie pokazy, następujące instruktaży w sesji online lub samouczki konferencji.


## <a name="whats-included-in-the-data-science-vm"></a>Co zawiera usługa maszyn wirtualnych nauki danych?

Maszyny wirtualnej nauki danych ma wielu narzędzi nauki popularne danych już zainstalowana i skonfigurowana. Zawiera również narzędzia, które ułatwiają do pracy z różnych Azure produktów danych i analizy. Można eksplorować i tworzenie modeli przewidywanych na dużych zestawów danych za pomocą serwera Microsoft R lub przy użyciu programu SQL Server 2016. Szereg innych narzędzi Wspólnoty Otwórz źródło i od firmy Microsoft są dołączone, a także przykładowy kod i notesów. Poniższa tabela itemizes i porównanie główne składniki zawarte w wersjach systemu Windows i Linux maszyny wirtualnej nauki danych.


|**Wersji systemu Windows** | **Linux Edition** |
|----------------|---------------|
|Microsoft R Server Developer Edition | Microsoft R Server Developer Edition |
|Anaconda Python 2.7, 3.5 | Anaconda Python 2.7, 3.5 |
|Notes Jupyter Server (R, Python) | JupyterHub: Jupyter wielu użytkowników notesów (R, Python, Julia) |
|Programu SQL Server 2016 Developer Edition: Skalowalna analizy w bazie danych z usługami R | Postgres, SQuirreL SQL (Narzędzia bazy danych), sterowniki programu SQL Server i wiersza polecenia (bcp, sqlcmd) |
|Edition społeczności programu Visual Studio 2015 (IDE) </br> — Usługa azure HDInsight (Hadoop) Lake danych narzędzia danych programu SQL Server </br> -Narzędzia Node.js, Python i R programu Visual Studio |IDEs i edytory </br> -Zaćmienie-przy użyciu wtyczki Azure zestaw narzędzi </br> -Emacs (z ESS, auctex) gedit |
|Power BI desktop | -- |
|Nauka obrabiarek </br> — Integracja z nauki Azure komputera </br> -CNTK (głębokości nauki na AI) </br> -Xgboost (popularne ML narzędzie w zawodach nauki danych) </br> -Wabbit Vowpal (uczeń szybkie online) </br> -Rattle (wizualne dane szybki start i narzędzia do analizy) </br> -Mxnet (głębokości nauki na AI) | Nauka obrabiarek </br> -Integracji z komputera Azure nauki </br> -CNTK (głębokości nauki na AI) </br> -Xgboost (popularne ML narzędzie w zawodach nauki danych) </br> -Wabbit Vowpal (uczeń szybkie online) </br> -Rattle (wizualne dane szybki start i narzędzia do analizy)  |
| SDK dostępu Azure i Cortana analizy pakiet usług | SDK dostępu Azure i Cortana analizy pakiet usług |
| Narzędzia do przenoszenia danych i zarządzanie zasobami Azure i Big Data: Eksplorator magazynu Azure, polecenie, programu PowerShell, AdlCopy (Lake danych Azure), AzCopy, dtui (w przypadku DocumentDB) bramy zarządzania danymi firmy Microsoft | Narzędzia do przenoszenia danych i zarządzanie zasobami Azure i Big Data: Eksplorator magazynu Azure, polecenie |
| Cyfra, dodatek programu Visual Studio Team Services | Cyfra |
| Port Windows najpopularniejszych Linux i narzędzi systemu Unix wiersza polecenia dostępne za pośrednictwem GitBash i poleceń | -- |



## <a name="how-to-get-started-with-the-windows-data-science-vm"></a>Jak rozpocząć pracę z maszyn wirtualnych nauki danych systemu Windows

- Tworzenie wystąpienie maszyn wirtualnych w systemie Windows, przechodząc do [tej strony](https://azure.microsoft.com/marketplace/partners/microsoft-ads/standard-data-science-vm/) i wybierając pozycję zielony przycisk **Tworzenie maszyn wirtualnych** .
- Zaloguj się do maszyn wirtualnych z pulpitu zdalnego przy użyciu poświadczeń, określonym przez Ciebie podczas tworzenia maszyn wirtualnych.
- Aby odnaleźć i uruchamianie narzędzia dostępne, kliknij **Start** menu.


## <a name="get-started-with-the-linux-data-science-vm"></a>Wprowadzenie do maszyn wirtualnych nauki danych Linux

- Utwórz wystąpienie maszyn wirtualnych w systemie Linux (oparte na OpenLogic CentOS), przechodząc do [tej strony](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm/) i wybierając przycisk **Tworzenie maszyn wirtualnych** .
- Zaloguj się do maszyn wirtualnych w kliencie SSH, takich jak Kit lub SSH polecenia, przy użyciu poświadczeń określonej podczas tworzenia maszyn wirtualnych.
- W wierszu powłoki wprowadź dsvm więcej info.
- Dla komputera stacjonarnego graficzne, pobrać klienta X2Go dla swojej platformy klienta [tutaj](http://wiki.x2go.org/doku.php/doc:installation:x2goclient) i postępuj zgodnie z instrukcjami w dokumencie maszyn wirtualnych nauki danych Linux [świadczenia maszyny wirtualnej nauki danych Linux](machine-learning-data-science-linux-dsvm-intro.md#installing-and-configuring-x2go-client).


## <a name="next-steps"></a>Następne kroki

### <a name="for-the-windows-data-science-vm"></a>Do nauki danych maszyn wirtualnych systemu Windows

- Aby uzyskać więcej informacji na temat uruchamiania określonych narzędzi dostępnych w wersji systemu Windows, zobacz [Obsługa administracyjna nauki wirtualna maszyna danych firmy Microsoft](machine-learning-data-science-provision-vm.md) i
-  Aby uzyskać więcej informacji o tym, jak wykonywać różne zadania potrzebną do danych projektu nauki na maszyn wirtualnych systemu Windows zobacz [dziesięć rzeczy, które można wykonywać nauki danych maszyn wirtualnych](machine-learning-data-science-vm-do-ten-things.md).

### <a name="for-the-linux-data-science-vm"></a>Do nauki danych Linux maszyn wirtualnych

- Aby uzyskać więcej informacji na temat uruchamiania określonych narzędzi dostępnych w wersji Linux zobacz [Obsługa administracyjna maszyny wirtualnej nauki danych Linux](machine-learning-data-science-linux-dsvm-intro.md).
- Aby uzyskać Instruktaż, w którym pokazano, jak wykonać kilka typowych zadań nauki danych za pomocą maszyn wirtualnych Linux zobacz [nauki danych na Linux danych nauki maszyny wirtualnej](machine-learning-data-science-linux-dsvm-walkthrough.md).
