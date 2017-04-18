<properties 
    pageTitle="Debugowanie usługi w chmurze Azure lub maszyn wirtualnych w programie Visual Studio | Microsoft Azure"
    description="Debugowanie usługi w chmurze lub maszyny wirtualnej w programie Visual Studio"
    services="visual-studio-online"
    documentationCenter="na"
    authors="TomArcher"
    manager="douge"
    editor="" />
<tags 
    ms.service="visual-studio-online"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="08/15/2016"
    ms.author="tarcher" />

# <a name="debugging-an-azure-cloud-service-or-virtual-machine-in-visual-studio"></a>Debugowanie usługi w chmurze Azure lub maszyn wirtualnych w programie Visual Studio

Program Visual Studio umożliwia różne opcje debugowania usług w chmurze Azure i maszyn wirtualnych.



## <a name="debug-your-cloud-service-on-your-local-computer"></a>Debugowanie usługi cloud na komputerze lokalnym

Można zaoszczędzić czas i pieniądze za pomocą Azure obliczyć emulatora debugowania usługi cloud na komputerze lokalnym. Przez debugowania usługi lokalnie, przed wdrożeniem tego, możesz poprawić niezawodności i wydajności bez opłaty raz obliczeń. Jednak niektóre mogą wystąpić błędy tylko po uruchomieniu usługi w chmurze platformy Azure się. Po włączeniu zdalnego debugowania podczas publikowania usługi, a następnie dołączyć debugowania do wystąpienia roli, można debugowania tych błędów.

Emulatorze symuluje usługi Azure obliczenia i wykonuje w środowisku lokalnym, dzięki czemu można przetestować i debugowania usługi w chmurze przed wdrożeniem tego. Emulatorze obsługuje do zarządzania cyklem życia wystąpienia usługi roli i zapewnia dostęp do symulowany zasoby, takie jak magazynu lokalnego. Podczas debugowania i uruchamiania usługi z programu Visual Studio, automatycznie uruchamia emulatorze jako aplikację tła, a następnie wdraża usługi w emulatorze. Emulatorze służy do wyświetlania usługi, gdy zostanie uruchomiony w środowisku lokalnym. Można uruchamiać pełną wersję lub wersję express emulatora. (Począwszy od Azure 2.3 express wersji emulatora jest domyślnie). Zobacz, [przy użyciu emulatora Express do uruchamiania i debugowania usługi w chmurze lokalnie](https://msdn.microsoft.com/library/dn339018.aspx).

### <a name="to-debug-your-cloud-service-on-your-local-computer"></a>Debugowanie usługi cloud na komputerze lokalnym

1. Na pasku menu wybierz pozycję **Debugowanie**, **Rozpocznij debugowanie** do uruchomienia projektu usługi Azure chmury. Alternatywnie naciśnij klawisz F5. Zostanie wyświetlony komunikat uruchamiany emulatora obliczenia. Po uruchomieniu emulatorze ikona na pasku zadań potwierdza go.

    ![Azure emulatora na pasku zadań](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC783828.png)

1. Wyświetlanie interfejsu użytkownika dla emulatora obliczeń przez otwarcie menu skrótów dla Azure ikony w obszarze powiadomień, a następnie wybierz **Pokaż obliczyć emulatora elementy interfejsu użytkownika**.

    W okienku po lewej stronie interfejsu użytkownika wyświetlane usług, które są obecnie wdrożony emulatora obliczeń i wystąpienia roli, które jest uruchomiony każdej usługi. Możesz wybrać usługi lub role, aby wyświetlić cyklu życia, rejestrowanie i informacje diagnostyczne w okienku po prawej stronie. Jeśli umieszczeniu fokusu na marginesie górnym okna uwzględniane rozszerza się na całą okienku po prawej stronie.

1. Krok za pośrednictwem aplikacji, wybierając polecenia w menu **Debugowanie** i punktów kontrolnych w kodzie. Podczas wykonywania kroków za pośrednictwem aplikacji w debugowania okienka zostaną zaktualizowane o bieżącym stanie aplikacji. Po zakończeniu debugowania, wdrażanie aplikacji zostanie usunięty. Jeśli aplikacja zawiera ról w sieci web i ustawieniu właściwości akcji uruchamiania, aby uruchomić przeglądarki sieci web, programu Visual Studio uruchamiania aplikacji sieci web w przeglądarce. Jeśli zmienisz liczbę wystąpień roli w konfiguracji usługi, możesz zatrzymać usługi w chmurze, a następnie ponownie uruchom debugowanie tak, aby umożliwić debugowanie tych nowych wystąpień roli.

    **Uwaga:** Po zakończeniu uruchamiania lub debugowania usługi emulatora lokalnym i emulatora miejsca do magazynowania nie są zatrzymane. Należy zatrzymać je bezpośrednio w obszarze powiadomień.


## <a name="debug-a-cloud-service-in-azure"></a>Debugowanie usługi w chmurze platformy Azure

Debugowanie usługi w chmurze z komputera zdalnego, należy włączyć funkcja jawnie podczas wdrażania usługi w chmurze, aby wymagane usługi (na przykład msvsmon.exe) są zainstalowane na maszyn wirtualnych, które uruchamiania wystąpień do roli. Jeśli nie zostanie włączone, zdalne debugowanie podczas publikowania usługi, musisz opublikować usługę z zdalne debugowanie włączone.

Jeśli włączysz zdalnego debugowania dla usługi w chmurze, nie powodować wystąpienie zmniejszoną wydajność i ponoszenia dodatkowych opłat. Zdalne debugowanie w usłudze produkcyjnej, nie należy używać, ponieważ klienci, którzy korzystają z usługi może mieć negatywny wpływ.

>[AZURE.NOTE] Podczas publikowania usługi w chmurze z programu Visual Studio, można włączyć **IntelliTrace** dla ról w tej usłudze, które współpracować .NET Framework 4 lub 4,5 .NET Framework. Za pomocą **IntelliTrace**, możesz sprawdzić zdarzeń występujących w przypadku ról w przeszłości i odtworzyć kontekstu, w tym czasie. Zobacz [Debugowanie usługi w chmurze opublikowanych z IntelliTrace i Visual Studio](http://go.microsoft.com/fwlink/?LinkID=623016) i [Przy użyciu IntelliTrace](https://msdn.microsoft.com/library/dd264915.aspx).

### <a name="to-enable-remote-debugging-for-a-cloud-service"></a>Aby włączyć zdalne debugowanie dla usługi w chmurze

1. Otwórz menu skrótów dla Azure projektu, a następnie wybierz pozycję **Publikuj**.

1. Wybierz pozycję środowiska **przemieszczania** i konfiguracji **Debugowanie** .

    Jest to tylko wskazówki. Możesz wybrać opcję Uruchom usługi środowiskach testowych w środowisku produkcyjnym. Jednak użytkownik może naruszać użytkowników po włączeniu zdalnego debugowania w środowisku produkcyjnym. Możesz wybrać konfigurację wersji, ale konfiguracji debugowania sprawia, że ułatwia debugowanie.

    ![Wybierz pozycję Konfiguracja debugowania](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746717.gif)

1. Postępuj zgodnie z instrukcjami normalny, ale zaznacz pole wyboru **Włącz zdalnego debugowania dla wszystkich ról** na karcie **Ustawienia zaawansowane** .

    ![Debugowanie konfiguracji](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746718.gif)

### <a name="to-attach-the-debugger-to-a-cloud-service-in-azure"></a>Aby dołączyć debugowania do usługi w chmurze platformy Azure

1. W Eksploratorze Server rozwiń węzeł usługi w chmurze.

1. Otwieranie menu skrótów dla roli lub wystąpienia roli, do którego chcesz dołączyć, a następnie wybierz **Dołączyć debugowania**.

    Jeśli debugowania roli debugowania Visual Studio podłączana do każdego wystąpienia tej roli. Debugowania podziały na przerwania po raz pierwszy roli działa wiersza kodu, które spełniają wszystkie warunki tego przerwania. Jeśli debugowanie wystąpienie dołącza debugowania tego wystąpienia i podziałami na przerwania tylko wtedy, gdy tego konkretnego wystąpienia działa wiersza kodu i spełniają warunki przerwania.

    ![Dołączanie debugowania](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746719.gif)

1. Po debugowania łączy się z wystąpieniem, debugowanie w zwykły sposób. Debugowania automatycznie dołącza do procesu odpowiedniego hosta dla roli użytkownika. W zależności od tego, co to jest roli debugowania dołącza do w3wp.exe, WaWorkerHost.exe lub WaIISHost.exe. Aby sprawdzić, do której dołączono debugowania procesu, rozwiń węzeł wystąpienie w Eksploratorze serwera. Aby uzyskać więcej informacji o procesach Azure, zobacz [Architektura roli Azure](http://blogs.msdn.com/b/kwill/archive/2011/05/05/windows-azure-role-architecture.aspx) .

    ![Wybierz typ kodu, okno dialogowe](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718346.png)

1. Aby zidentyfikować procesów, do których dołączono debugowania, Otwieranie okna dialogowego procesów, z menu, wybierając debugowania, Windows, procesami. (Klawiatury: klawisze Ctrl + Alt + Z) Aby odłączyć określonego procesu, otwórz menu skrótów, a następnie wybierz **Odłączanie proces**. Lub, odszukaj węzeł wystąpienie w Eksploratorze serwera, Znajdź proces, otwórz menu skrótów, a następnie wybierz **Odłączanie proces**.

    ![Debugowanie procesów](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC690787.gif)

>[AZURE.WARNING] Unikanie długie zatrzymaniem co punktów kontrolnych podczas zdalnego debugowania. Azure traktuje procesem, który jest zablokowany przez dłużej niż kilka minut odpowiada i zatrzymuje wysyłanie ruch do tego wystąpienia. Jeśli przerwiesz przez dłuższy czas, msvsmon.exe odłącza od procesu.

Aby odłączyć debugowania z procesów wszystkie wystąpienia lub roli, otwórz menu skrótów dla roli lub wystąpienia, które już debugowania, a następnie wybierz **Odłączanie debugowania**.

## <a name="limitations-of-remote-debugging-in-azure"></a>Ograniczenia dotyczące zdalnego debugowania platformy Azure

Z 2.3 SDK Azure zdalne debugowanie występują następujące ograniczenia.

- Z zdalne debugowanie włączone, nie można opublikować w którym wszelkie rola ma więcej niż 25 wystąpień usługi w chmurze.

- Debugowania używa portów 30400 do 30424, 31400 do 31424 i 32400 do 32424. Jeśli próbujesz użyć dowolnej z tych portów, nie będą mogli publikować usługi, a jeden z następujących komunikatów o błędach pojawi się w dzienniku aktywności Azure: 

    - Błąd sprawdzania poprawności pliku .cscfg w odniesieniu do pliku .csdef. 
    Zakres portu zastrzeżone "zakres" dla punktu końcowego Microsoft.WindowsAzure.Plugins.RemoteDebugger.Connector roli "roli" nakłada się na zdefiniowano portu lub zakresu.
    - Alokacja nie powiodła się. Spróbuj ponownie później, spróbuj zmniejszyć rozmiar pamięci Wirtualnej lub liczbę wystąpień roli lub spróbuj wdrażanie do innego obszaru.


## <a name="debugging-azure-virtual-machines"></a>Debugowanie Azure maszyn wirtualnych

Czy debugowanie programy, które działają w przypadku Azure maszyn wirtualnych przy użyciu Eksploratora serwera w programie Visual Studio. Włączenie zdalnego debugowania na Azure maszyn wirtualnych Azure instaluje zdalnego debugowania rozszerzenia na komputerze wirtualnych. Następnie możesz dołączyć do procesów na komputerze wirtualnych i debugowanie w normalny sposób.

>[AZURE.NOTE] Utworzony przez stos Menedżera zasobów Azure maszyn wirtualnych było zdalnie możliwe przy użyciu Eksploratora Cloud w Visual Studio 2015 r. Aby uzyskać więcej informacji zobacz [Zarządzanie zasobami Azure w Eksploratorze chmury](http://go.microsoft.com/fwlink/?LinkId=623031).

### <a name="to-debug-an-azure-virtual-machine"></a>Debugowanie Azure maszyn wirtualnych

1. W Eksploratorze Server rozwiń węzeł maszyn wirtualnych, a następnie wybierz węzeł maszyny wirtualnej, która ma zostać debugowanie.

1. Otwórz menu kontekstowe i wybierz pozycję **Włącz debugowanie**. Gdy zostanie wyświetlony monit, jeśli wiesz, czy chcesz włączyć debugowanie maszyny wirtualnej, wybierz przycisk **Tak**.

    Azure instaluje zdalnego debugowania rozszerzenia na komputerze wirtualnych, aby włączyć debugowanie.

    ![Maszyn wirtualnych włączyć polecenie debugowania](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746720.png)

    ![Dziennik Azure](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746721.png)

1. Po zakończeniu instalacji zdalnego debugowania rozszerzenia Otwórz menu kontekstowe maszyny wirtualnej i wybierz pozycję **Dołącz debugowania**

    Azure otrzymuje listę procesów na komputerze wirtualnych i wyświetla je w Dołącz, aby proces, okno dialogowe.

    ![Polecenie debugowania Dołącz](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746722.png)

1. W oknie dialogowym **Dołączanie do procesu** wybierz pozycję **Wybierz** , aby ograniczyć listy wyników, aby pokazać tylko typy kod, który ma zostać debugowanie. Czy debugowanie kodu 32 - lub 64-bitowa zarządzane i kodu macierzystego.

    ![Wybierz typ kodu, okno dialogowe](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718346.png)

1. Wybierz pozycję procesu, który chcesz debugowanie na komputerze wirtualnych, a następnie wybierz pozycję **Dołącz**. Na przykład możesz wybrać proces w3wp.exe, aby debugowanie aplikacji sieci web na komputerze wirtualnych. Aby uzyskać więcej informacji, zobacz [Debugowanie jeden lub więcej procesów w programie Visual Studio](https://msdn.microsoft.com/library/jj919165.aspx) i [Architektury roli Azure](http://blogs.msdn.com/b/kwill/archive/2011/05/05/windows-azure-role-architecture.aspx) .

## <a name="create-a-web-project-and-a-virtual-machine-for-debugging"></a>Tworzenie projektu sieci web i maszyny wirtualnej do debugowania

Przed opublikowaniem Azure projektu, mogą być przydatne go przetestować w środowisku zawartych, który obsługuje debugowanie i scenariuszy testowania i miejsce, w którym można zainstalować testowanie i monitorowanie programów. Jednym ze sposobów jest zdalne debugowanie aplikacji na komputerze wirtualnych.

Projekty Visual Studio ASP.NET oferuje opcję tworzenia przydatnego maszyny wirtualnej, który służy do testowania aplikacji. Maszyny wirtualnej obejmuje często potrzebna punkty końcowe, takie jak programu PowerShell, pulpitu zdalnego i WebDeploy.

### <a name="to-create-a-web-project-and-a-virtual-machine-for-debugging"></a>Tworzenie projektu sieci web i maszyny wirtualnej do debugowania

1. W programie Visual Studio Tworzenie nowej aplikacji sieci Web programu ASP.NET.

1. W oknie dialogowym Nowy projekt ASP.NET, w sekcji Azure wybierz **maszyn wirtualnych** w polu listy rozwijanej. Pozostaw zaznaczone pole wyboru **Utwórz zasobów zdalnych** . Wybierz **przycisk OK** , aby kontynuować.

    Zostanie wyświetlone okno dialogowe **Tworzenie maszyn wirtualnych w Azure** .


    ![Tworzenie projektu sieci web programu ASP.NET, okno dialogowe](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746723.png)

    **Uwaga:** Zostanie wyświetlony monit do zalogowania się do konta Azure, jeśli jeszcze nie jest zalogowany.

1. Wybieranie różnych ustawień maszyny wirtualnej, a następnie wybierz **przycisk OK**. Aby uzyskać więcej informacji, zobacz [maszyn wirtualnych]( http://go.microsoft.com/fwlink/?LinkId=623033) .

    Wprowadzona nazwa DNS nazwa będzie nazwa maszyny wirtualnej. 

    ![Tworzenie maszyn wirtualnych w oknie dialogowym Azure](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746724.png)

    Azure tworzy maszyn wirtualnych, a następnie postanowienia i konfiguruje punkty końcowe, takiej jak Pulpit zdalny i wdrażanie sieci Web



1. Po maszyny wirtualnej jest w pełni skonfigurowane, wybierz węzeł maszyny wirtualnej w Eksploratorze serwera.

1. Otwórz menu kontekstowe i wybierz pozycję **Włącz debugowanie**. Gdy zostanie wyświetlony monit, jeśli wiesz, czy chcesz włączyć debugowanie maszyny wirtualnej, wybierz przycisk **Tak**. 

    Azure instaluje zdalnego debugowania rozszerzenia maszyn wirtualnych, aby włączyć debugowanie.

    ![Maszyn wirtualnych włączyć polecenie debugowania](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746720.png)

    ![Dziennik Azure](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746721.png)

1. Publikowanie projektu w sposób opisany w [jak: Wdrażanie sieci Web za pomocą jednego kliknięcia publikowanie projektu w programie Visual Studio](https://msdn.microsoft.com/library/dd465337.aspx). Ponieważ chcesz debugowanie na komputerze wirtualnych, na stronie **Ustawienia** kreatora **Publikowania w sieci Web** , wybierz pozycję **Debugowanie** jako konfiguracji. Dzięki temu kod symbole są dostępne podczas debugowania.

    ![Ustawienia publikowania](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718349.png)

1. W obszarze **Opcje publikowania pliku**kliknij przycisk **Usuń dodatkowe pliki w miejscu docelowym** , jeśli projekt został już wdrożony w czasie wcześniejszych.

1. Gdy publikuje projektu, w menu kontekstowym maszyny wirtualnej w Eksploratorze serwera zaznacz **Dołączyć debugowania**

    Azure otrzymuje listę procesów na komputerze wirtualnych i wyświetla je w Dołącz, aby proces, okno dialogowe.

    ![Polecenie debugowania Dołącz](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746722.png)

1. W oknie dialogowym **Dołączanie do procesu** wybierz pozycję **Wybierz** , aby ograniczyć listy wyników, aby pokazać tylko typy kod, który ma zostać debugowanie. Czy debugowanie kodu 32 - lub 64-bitowa zarządzane i kodu macierzystego.

    ![Wybierz typ kodu, okno dialogowe](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718346.png)

1. Wybierz pozycję procesu, który chcesz debugowanie na komputerze wirtualnych, a następnie wybierz pozycję **Dołącz**. Na przykład możesz wybrać proces w3wp.exe, aby debugowanie aplikacji sieci web na komputerze wirtualnych. Aby uzyskać więcej informacji, zobacz [Debugowanie jeden lub więcej procesów w programie Visual Studio](https://msdn.microsoft.com/library/jj919165.aspx) .

## <a name="next-steps"></a>Następne kroki

- Zbieranie dzienników połączeń i wydarzeń z serwera wersji przy użyciu **Intellitrace** . Zobacz [Debugowanie usługi w chmurze opublikowanych z IntelliTrace i programu Visual Studio](http://go.microsoft.com/fwlink/?LinkID=623016).
- Rejestrować szczegółowe informacje z kodu działającego w role, czy role są uruchomione w środowisku programowania lub Azure za pomocą **Diagnostyki Azure** . Zobacz [Collecting rejestrowanie danych przy użyciu zestawu narzędzi Diagnostyka Azure](http://go.microsoft.com/fwlink/p/?LinkId=400450).
