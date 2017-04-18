<properties
   pageTitle="Konfigurowanie diagnostyki dla usług w chmurze Azure i maszyn wirtualnych | Microsoft Azure"
   description="W tym artykule opisano, jak skonfigurować informacje diagnostyczne dla debugowania usługi Azure cloude i wirtualnych maszyn w programie Visual Studio."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="configuring-diagnostics-for-azure-cloud-services-and-virtual-machines"></a>Konfigurowanie informacje diagnostyczne dla usług w chmurze Azure i maszyn wirtualnych

Gdy trzeba Rozwiązywanie problemów z usługi w chmurze Azure lub Azure maszyn wirtualnych Azure diagnostyki można skonfigurować łatwiej przy użyciu programu Visual Studio. Azure diagnostyki znajdują się dane systemu i rejestrowanie danych w środowisku maszyn wirtualnych systemu i wystąpienia maszyn wirtualnych, które uruchamianie usługi w chmurze i przesyła danych, który uwzględnia miejsca do magazynowania wybranych przez użytkownika. Aby uzyskać więcej informacji o diagnostyce rejestrowanie w Azure, zobacz [Włączanie diagnostyki rejestrowania aplikacji sieci web w usłudze Azure aplikacji](./app-service-web/web-sites-enable-diagnostic-log.md) .

W tym temacie opisano włączanie i konfigurowanie Azure diagnostyki w programie Visual Studio, przed i po wdrożeniu, a także w środowisku maszyn wirtualnych Azure. Ponadto możesz sposobu wybierz typy informacje diagnostyczne do zbierania i wyświetlania informacji po są zbierane.

Narzędzia diagnostyczne Azure można skonfigurować w następujący sposób:

- Możesz zmienić ustawienia diagnostyki za pomocą okna dialogowego **Konfiguracji diagnostyki** w programie Visual Studio. Ustawienia są zapisywane w pliku o nazwie diagnostics.wadcfgx (diagnostics.wadcfg w 2,4 SDK Azure lub starszym). Ewentualnie można bezpośrednio zmodyfikować plik konfiguracji. Jeśli ręcznie zaktualizować plik, zmiany konfiguracji zostaną wprowadzone przy następnym czasu wdrażanie chmury usługi Azure lub uruchomić usługę w emulatorze.

- Aby zmienić ustawienia diagnostyki uruchomionego usługi w chmurze lub maszyn wirtualnych za pomocą **Eksploratora chmury** lub **Eksploratora serwera** w programie Visual Studio.

## <a name="azure-26-diagnostics-changes"></a>Zmiany diagnostyki Azure 2.6

W przypadku projektów Azure SDK 2.6 w programie Visual Studio wprowadzono następujące zmiany. (Te zmiany także dotyczą nowszych wersjach systemu Azure SDK.)

- Lokalne emulatora obsługuje teraz diagnostyki. Oznacza to, czy zbieranie danych diagnostyki i upewnij się, że aplikacja tworzy prawo śledzenia podczas już opracowywania i testowania w programie Visual Studio. Parametry połączenia `UseDevelopmentStorage=true` umożliwia zbieranie danych diagnostyki, gdy korzystasz z chmury usługi projektu w programie Visual Studio przy użyciu emulatora Azure miejsca do magazynowania. Wszystkie dane diagnostyki są zbierane na koncie miejsca do magazynowania (opracowywania miejsca do magazynowania).

- Parametry połączenia konta diagnostyki przestrzeni dyskowej (Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString) znajduje się ponownie w pliku konfiguracji (.cscfg) usługi. W Azure SDK 2.5 konta miejsca do magazynowania diagnostyki określono w pliku diagnostics.wadcfgx.

Istnieje kilka godne uwagi różnic między jak parametry połączenia pracy w 2,4 SDK Azure lub nowszy i sposobu jej działania w Azure SDK 2.6 lub nowszym.

- W 2,4 SDK Azure i starszych parametry połączenia użyto jako środowisko uruchomieniowe przez wtyczkę diagnostyki Aby uzyskać informacje o koncie miejsca do magazynowania dla przenoszenia dzienniki diagnostyczne.

- W Azure SDK 2.6 lub nowszym parametry połączenia diagnostyki jest używany przez program Visual Studio do konfigurowania rozszerzenia diagnostyki magazynowania odpowiednie informacje o koncie podczas publikowania. Parametry połączenia pozwala na zdefiniowanie magazynu innego konta dla konfiguracji innej usługi, używające programu Visual Studio podczas publikowania. Jednak ponieważ dodatek diagnostyki nie jest już dostępna (po Azure SDK 2.5), plik .cscfg przez siebie nie można włączyć rozszerzenia diagnostyki. Musisz włączyć rozszerzenie oddzielnie za pomocą narzędzi, takich jak Visual Studio lub programu PowerShell.

- Aby uprościć proces konfigurowania rozszerzenia diagnostyki przy użyciu programu PowerShell, dane wyjściowe pakietu Visual Studio zawiera publicznej konfiguracji XML rozszerzenia diagnostyki dla poszczególnych ról. Programu Visual Studio umożliwia parametry połączenia diagnostyki Wypełnij informacje o koncie miejsca do magazynowania, które są zawarte w konfiguracji publicznej. Pliki konfiguracji publicznej są tworzone w folderze rozszerzenia i postępuj zgodnie ze wzorcem PaaSDiagnostics. &lt;RoleName >. PubConfig.xml. Wszelkie wdrożeń programu PowerShell podstawie można używać tego wzorca do zamapować każdej konfiguracji do roli.

- Parametry połączenia w pliku .cscfg jest też używane przez [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) uzyskiwać dostęp do danych diagnostyki, może być wyświetlany na karcie **monitorowania** . Parametry połączenia jest potrzebne do skonfigurowania usługi umożliwia wyświetlanie pełnej monitorowania danych w portalu.

## <a name="migrating-projects-to-azure-sdk-26-and-later"></a>Migrowanie projektów Azure SDK 2.6 i nowszych

Podczas migracji z Azure SDK 2,5 Azure SDK 2.6 lub nowszym, jeśli masz konto miejsca do magazynowania diagnostyki określone w pliku .wadcfgx, następnie pozostanie on. Aby skorzystać z elastyczności przy użyciu magazynu innego konta dla konfiguracji innego miejsca do magazynowania, musisz ręcznie dodać parametry połączenia do projektu. Jeśli przeprowadzasz migrację projektu z 2,4 SDK Azure lub starszym do Azure SDK 2.6, parametry połączenia diagnostyki są zachowywane. Pamiętaj, jednak zmiany w jak parametry połączenia są traktowane w Azure SDK 2.6 określone w poprzedniej sekcji.

### <a name="how-visual-studio-determines-the-diagnostics-storage-account"></a>Jak Visual Studio Określa konto miejsca do magazynowania diagnostyki

- Jeśli określono diagnostyki parametry połączenia w pliku .cscfg, Visual Studio używa go skonfigurować rozszerzenia diagnostyki podczas publikowania i generowanie plików xml publicznej konfiguracji podczas pakowania.

- Jeśli określono parametry połączenia nie diagnostyczne w pliku .cscfg, następnie Visual Studio przechodzi do przy użyciu konta miejsca do magazynowania, określonego w pliku .wadcfgx skonfigurować rozszerzenia diagnostyki podczas publikowania i generowanie pliki xml publicznej konfiguracji podczas pakowania.

- Narzędzia diagnostyczne parametry połączenia w pliku .cscfg ma pierwszeństwo przed konto miejsca do magazynowania w pliku .wadcfgx. Jeśli określono diagnostyki parametry połączenia w pliku .cscfg, Visual Studio korzysta z którego i ignoruje konta miejsca do magazynowania w .wadcfgx.

### <a name="what-does-the-update-development-storage-connection-strings-checkbox-do"></a>Co oznacza "Aktualizacja opracowywania miejsca do magazynowania parametry połączenia..." pole wyboru zrobić?

Pole wyboru **Parametry połączenia aktualizacji opracowywania miejsca do magazynowania dla narzędzia diagnostyczne i buforowanie przy użyciu poświadczeń konta magazynu platformy Microsoft Azure podczas publikowania w programie Microsoft Azure** zapewnia wygodny sposób, aby zaktualizować wszystkie ciągi połączenia opracowywania miejsca do magazynowania konta za pomocą konta magazynu platformy Azure określony podczas publikowania.

Załóżmy na przykład, zaznacz to pole wyboru i określić parametry połączenia diagnostyki `UseDevelopmentStorage=true`. Podczas publikowania projektu Azure Visual Studio zostanie automatycznie zaktualizowany diagnostyki parametry połączenia z kontem miejsca do magazynowania w określonej w Kreatorze publikowania. Jednak jeśli konto rzeczywistą magazynowania określono jako parametry połączenia diagnostyki, następnie konta zamiast niej zostanie użyta.

## <a name="diagnostics-functionality-differences-between-azure-sdk-24-and-earlier-and-azure-sdk-25-and-later"></a>Narzędzia diagnostyczne funkcji różnice między 2,4 SDK Azure i wcześniej i Azure SDK 2.5 lub nowszy

W przypadku uaktualniania projektu Azure SDK 2,4 2,5 SDK Azure lub nowszy, możesz należy pamiętać następujące różnice funkcji Diagnostyka.

- **Interfejsy API konfiguracji są przestarzałe** — programowych konfiguracji diagnostyki jest dostępny w 2,4 SDK Azure i starszych wersjach, ale jest przestarzałe w 2,5 SDK Azure lub nowszym. Jeśli konfiguracji diagnostyki jest aktualnie zdefiniowana w kodzie, musisz skonfigurować te ustawienia od podstaw migracją projektu w kolejności diagnostyki pakietu, aby kontynuować pracę. Plik konfiguracji diagnostyki Azure SDK 2,4 jest diagnostics.wadcfg i diagnostics.wadcfgx dla 2,5 SDK Azure lub w nowszej wersji.

- **Informacje diagnostyczne dla aplikacji usług w chmurze można skonfigurować tylko na poziomie roli, nie na poziomie obiektu.**

- **Za każdym razem, wdrażanie aplikacji, konfiguracji diagnostyki jest aktualizowana** — to może powodować problemy z spójności, jeśli zmienić konfigurację diagnostyki Eksploratora serwera, a następnie ponowne wdrożenie aplikacji.

- **W 2,5 SDK Azure i nowszym, awarię zrzuty są skonfigurowane w pliku konfiguracji diagnostyki nie w kodzie** — Jeśli masz skonfigurowane w kodzie zrzuty awarię, musisz ręcznie przenieść konfiguracji od kodu do pliku konfiguracji, ponieważ zrzuty awarię nie są transferowane podczas migracji do Azure SDK 2.6.

## <a name="enable-diagnostics-in-cloud-service-projects-before-deploying-them"></a>Włączanie narzędzia diagnostyczne w projektach usługi w chmurze przed ich wdrożeniem

W programie Visual Studio możesz wybrać zbieranie danych diagnostyki dla ról uruchamianych w Azure, podczas uruchamiania usługi w emulatorze przed wdrożeniem jej. Wprowadzenie wszystkich zmian ustawień diagnostyki w programie Visual Studio są zapisywane w pliku konfiguracji diagnostics.wadcfgx. Te ustawienia konfiguracji Określ konto miejsca do magazynowania, zapisywania danych diagnostyki podczas wdrażania usługi w chmurze.

### <a name="to-enable-diagnostics-in-visual-studio-before-deployment"></a>Aby włączyć diagnostyki w programie Visual Studio przed wdrożeniem

1. W menu skrótów dla roli interesującą Cię wybierz polecenie **Właściwości**, a następnie wybierz kartę **Konfiguracja** w oknie **Właściwości** roli.

1. W sekcji **Narzędzia diagnostyczne** upewnij się, że jest zaznaczone pole wyboru **Włącz diagnostyki** .

    ![Uzyskiwanie dostępu do opcji Włącz diagnostyki](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796660.png)

1. Wybierz przycisk wielokropka (...), aby określić miejsce, w którym chcesz umieścić dane diagnostyki mają być przechowywane konto miejsca do magazynowania. Konto miejsca do magazynowania, wybranym będzie lokalizacji, w której są przechowywane dane diagnostyki.

    ![Umożliwia określenie za pomocą konta miejsca do magazynowania](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796661.png)

1. W oknie dialogowym **Tworzenie parametrów połączenia miejsca do magazynowania** Określ, czy chcesz nawiązać połączenie za pomocą emulatorze miejsca do magazynowania Azure Azure subskrypcji, lub ręcznie wprowadzić poświadczenia.

    ![Okno dialogowe konta miejsca do magazynowania](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796662.png)

  - W przypadku wybrania firmy Microsoft Azure emulatora miejsca do magazynowania opcję Parametry połączenia jest ustawiona na UseDevelopmentStorage = true.

  - W przypadku wybrania opcji Twojej subskrypcji, możesz wybrać Azure subskrypcji, którego chcesz użyć i nazwę konta. Możesz wybrać przycisku Zarządzaj kontami do zarządzania subskrypcjami Azure.

  - Jeśli wybierzesz opcję poświadczeń wprowadzone ręcznie, zostanie wyświetlony monit o wprowadzenie nazwy i klucza Azure konta, dla którego chcesz użyć.

1. Wybierz przycisk **Konfiguruj** , aby wyświetlić okno dialogowe **Konfiguracja diagnostyki** . Każda karta (z wyjątkiem **Ogólne** i **Dziennika katalogów**) reprezentuje źródło danych diagnostyczne, które może zbierać. Karta domyślne **Ogólne**oferuje następujące opcje zbierania danych diagnostyki: **tylko błędy**, **wszystkie informacje**i **plan niestandardowy**. Domyślna opcja **tylko błędy**, wystarczy najmniejszą ilość miejsca do magazynowania, ponieważ nie przenosi, ostrzeżenia i śledzenie wiadomości. Opcji wszystkie informacje przekazuje większość informacje i dlatego, jest najbardziej drogich opcja pod względem miejsca do magazynowania.

    ![Włączanie Azure narzędzia diagnostyczne i konfiguracji](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758144.png)

1. W tym przykładzie wybierz opcję **plan niestandardowy** , możesz dostosować danych zebranych.

1. Pole **Przydział dysku w MB** Określa, ile miejsca chcesz przydzielić na koncie miejsca do magazynowania dla danych diagnostyki. Jeśli chcesz, możesz zmienić wartość domyślna.

1. Na każdej z kart narzędzia diagnostyczne danych do zebrania, zaznacz jego **Włączanie przenoszenia <log type> ** pole wyboru. Na przykład jeśli chcesz zebrać dzienniki aplikacji, zaznacz pole wyboru **Włącz przesyłanie Dzienniki aplikacji** na karcie **Dzienniki aplikacji** . Ponadto Podaj wszelkie inne informacje wymagane przez każdego typu danych diagnostyki. W sekcji **Konfigurowanie źródeł danych diagnostyki** w dalszej części tego tematu informacje dotyczące konfiguracji na każdej z kart.

1. Po włączeniu zbieranie danych diagnostyki chcesz, wybierz przycisk **OK** .

1. Uruchom projekt usługi Azure chmury w programie Visual Studio w zwykły sposób. Podczas korzystania z aplikacji informacji dziennika, który został włączony jest zapisany na koncie Azure miejsca do magazynowania, określonej.

## <a name="enable-diagnostics-in-azure-virtual-machines"></a>Włączanie narzędzia diagnostyczne w środowisku maszyn wirtualnych Azure

W programie Visual Studio możesz wybrać zbieranie danych diagnostyki dla Azure maszyn wirtualnych.

### <a name="to-enable-diagnostics-in-azure-virtual-machines"></a>Aby włączyć diagnostyki w środowisku maszyn wirtualnych Azure

1. W **Eksploratorze serwera**wybierz węzeł Azure, a następnie połączyć się z subskrypcji usługi Azure, jeśli jeszcze nie masz połączenie.

1. Rozwiń węzeł **maszyn wirtualnych** . Tworzenie nowej maszyny wirtualnej lub wybrać jedną, które zostało już dodane.

1. W menu skrótów dla maszyny wirtualnej interesujący Cię wybierz pozycję **Konfiguruj**. Spowoduje to wyświetlenie okna dialogowego konfiguracji maszyn wirtualnych.

    ![Konfigurowanie Azure maszyn wirtualnych](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796663.png)

1. Jeśli jeszcze nie jest zainstalowany, należy dodać rozszerzenia diagnostyki monitorowania agenta firmy Microsoft. To rozszerzenie umożliwia zbieranie danych diagnostyki Azure maszyny wirtualnej. Na liście zainstalowane rozszerzenia wybierz pozycję Wybierz menu rozwijane dostępnych rozszerzeń, a następnie wybierz pozycję Diagnostyka monitorowania agenta firmy Microsoft.

    ![Instalowanie rozszerzenia Azure maszyn wirtualnych](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766024.png)

    >[AZURE.NOTE] Inne rozszerzenia diagnostyki są dostępne dla maszyn wirtualnych. Aby uzyskać więcej informacji zobacz funkcje i rozszerzenia maszyn wirtualnych Azure.

1. Wybierz przycisk **Dodaj** , aby dodać rozszerzenie i jego okno dialogowe **Konfiguracja diagnostyki** wyświetlanie.

1. Wybierz przycisk **Konfiguruj** , aby określić konto miejsca do magazynowania, a następnie wybierz przycisk **OK** .

    Każda karta (z wyjątkiem **Ogólne** i **Dziennika katalogów**) reprezentuje źródło danych diagnostyczne, które może zbierać.

    ![Włączanie Azure narzędzia diagnostyczne i konfiguracji](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758144.png)

    Karta domyślne **Ogólne**oferuje następujące opcje zbierania danych diagnostyki: **tylko błędy**, **wszystkie informacje**i **plan niestandardowy**. Domyślna opcja **tylko błędy**, wystarczy najmniejszą ilość miejsca do magazynowania, ponieważ nie przenosi, ostrzeżenia i śledzenie wiadomości. Opcja **wszystkie informacje** przekazuje większość informacje i jest w związku z tym, opcja najbardziej drogich pod względem miejsca do magazynowania.

1. W tym przykładzie wybierz opcję **plan niestandardowy** , możesz dostosować danych zebranych.

1. Pole **Przydział dysku w MB** Określa, ile miejsca chcesz przydzielić na koncie miejsca do magazynowania dla danych diagnostyki. Jeśli chcesz, możesz zmienić wartość domyślna.

1. Na każdej z kart narzędzia diagnostyczne danych do zebrania, zaznacz jego **Włączanie przenoszenia <log type> ** pole wyboru.

    Na przykład jeśli chcesz zebrać dzienniki aplikacji, zaznacz pole wyboru **Włącz przesyłanie Dzienniki aplikacji** na karcie **Dzienniki aplikacji** . Ponadto Podaj wszelkie inne informacje wymagane przez każdego typu danych diagnostyki. W sekcji **Konfigurowanie źródeł danych diagnostyki** w dalszej części tego tematu informacje dotyczące konfiguracji na każdej z kart.

1. Po włączeniu zbieranie danych diagnostyki chcesz, wybierz przycisk **OK** .

1. Zapisz zaktualizowany projekt.

    Pojawi się komunikat w oknie **Microsoft Azure dziennik** zaktualizowano maszyny wirtualnej.

## <a name="configure-diagnostics-data-sources"></a>Konfigurowanie diagnostyki źródeł danych

Po włączeniu narzędzia diagnostyczne zbierania danych, możesz wybrać dokładnie źródeł danych, które chcesz zebrać i jakie informacje są zbierane. Oto lista kart w oknie dialogowym **Konfiguracja narzędzia diagnostyczne** i oznacza poszczególnych opcji konfiguracji.

### <a name="application-logs"></a>Dzienniki aplikacji

**Dzienniki aplikacji** zawierają informacje diagnostyczne tworzone przez aplikację sieci web. Jeśli chcesz zarejestrować dzienniki aplikacji, zaznacz pole wyboru **Włącz przesyłanie Dzienniki aplikacji** . Można zwiększyć lub zmniejszyć liczbę minut po Dzienniki aplikacji są przenoszone do swojego konta miejsca do magazynowania, zmieniając wartość **Transfer okres (min)** . Możesz również zmienić ilość informacji rejestrowania w dzienniku, ustawiając wartość poziomu dziennika. Na przykład możesz wybrać **pełne** , aby uzyskać więcej informacji, lub wybierz **krytyczne** , aby przechwycić tylko błędy krytyczne. Jeśli masz dostawcy określonych diagnostyki, który emituje Dzienniki aplikacji, można przechwycić je, dodając identyfikator GUID dostawcy w polu **Identyfikator GUID dostawcy** .

  ![Dzienniki aplikacji](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758145.png)

  Aby uzyskać więcej informacji o dziennikach aplikacji, zobacz [Włączanie diagnostyki rejestrowania aplikacji sieci web w usłudze Azure aplikacji](./app-service-web/web-sites-enable-diagnostic-log.md) .

### <a name="windows-event-logs"></a>Dzienniki zdarzeń systemu Windows

Jeśli chcesz zarejestrować dzienniki zdarzeń systemu Windows, zaznacz pole wyboru **Włącz przesyłanie dzienniki zdarzeń systemu Windows** . Można zwiększyć lub zmniejszyć liczbę minut, gdy dzienniki zdarzeń są przenoszone do swojego konta miejsca do magazynowania, zmieniając wartość **Transfer okres (min)** . Zaznacz pola wyboru dla typów zdarzeń, które chcesz śledzić.

  ![Dzienniki zdarzeń](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796664.png)

Jeśli korzystasz z platformy Azure SDK 2.6 lub nowszej i chcesz określić niestandardowego źródła danych, wprowadź je w **<Data source name>** tekstu pola, a następnie wybierz przycisk **Dodaj** obok niej. Źródło danych jest dodawane do pliku diagnostics.cfcfg.

Jeśli korzystasz z platformy Azure SDK 2,5 i chcesz określić niestandardowego źródła danych, możesz dodać go do `WindowsEventLog` sekcji diagnostics.wadcfgx pliku, takie jak w poniższym przykładzie.

```
<WindowsEventLog scheduledTransferPeriod="PT1M">
   <DataSource name="Application!*" />
   <DataSource name="CustomDataSource!*" />
</WindowsEventLog>
```
### <a name="performance-counters"></a>Liczniki wydajności

Informacje o wydajności licznik może pomóc w Znajdź gardeł systemu i dostosowywanie wydajności systemu i aplikacji. Aby uzyskać więcej informacji, zobacz [Tworzenie i używanie liczników wydajności w aplikacji Azure](https://msdn.microsoft.com/library/azure/hh411542.aspx) . Jeśli chcesz przechwycić liczniki wydajności, zaznacz pole wyboru **Włącz przesyłanie liczników wydajności** . Można zwiększyć lub zmniejszyć liczbę minut, gdy dzienniki zdarzeń są przenoszone do swojego konta miejsca do magazynowania, zmieniając wartość **Transfer okres (min)** . Zaznacz pola wyboru dla liczników wydajności, które chcesz śledzić.

  ![Liczniki wydajności](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758147.png)

Aby śledzić licznika, którego nie ma na liście, wprowadź je przy użyciu sugerowane składni, a następnie wybierz przycisk **Dodaj** . System operacyjny komputera wirtualnych Określa, które liczniki wydajności, można śledzić. Aby uzyskać więcej informacji na temat składni zobacz [Określanie ścieżki licznika](https://msdn.microsoft.com/library/windows/desktop/aa373193.aspx).

### <a name="infrastructure-logs"></a>Dzienniki infrastruktury

Jeśli chcesz przechwycić dzienniki infrastruktury, zawierających informacje o Azure infrastruktury diagnostyczne moduł dostęp zdalny i modułu RemoteForwarder zaznacz pole wyboru **Włącz przesyłanie dzienników infrastruktury** . Można zwiększyć lub zmniejszyć liczbę minut po dzienniki są przenoszone do swojego konta miejsca do magazynowania, zmieniając wartość Transfer okres (min).

  ![Narzędzia diagnostyczne infrastruktury dzienników](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758148.png)

  Aby uzyskać więcej informacji, zobacz [Zbieranie rejestrowania danych przy użyciu narzędzia diagnostyczne Azure](https://msdn.microsoft.com/library/azure/gg433048.aspx) .

### <a name="log-directories"></a>Katalogi dziennika

Jeśli chcesz przechwycić katalogi dziennika, które zawierają dane zebrane z katalogów dziennika żądania internetowe usługi informacyjne (IIS), nie powiodło się żądania lub foldery, które można wybrać, zaznacz pole wyboru **Włącz transferu katalogów dziennika** . Można zwiększyć lub zmniejszyć liczbę minut po dzienniki są przenoszone do swojego konta miejsca do magazynowania, zmieniając wartość **Transfer okres (min)** .

Można zaznaczyć pola dzienników, które chcesz zebrać, takich jak **Dzienniki programu IIS** i dzienniki **Failed żądanie** . Domyślne nazwy kontenera przestrzeni dyskowej są dostarczane, ale jeśli chcesz zmienić nazwy.

Ponadto można przechwycić dzienników z poziomu dowolnego folderu. Określa ścieżkę w sekcji **Dziennik z katalogu bezwzględne** , a następnie wybierz przycisk **Dodaj katalog** . Dzienniki będą przechwytywane do określonej kontenerów.

  ![Katalogi dziennika](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796665.png)

### <a name="etw-logs"></a>Dzienniki zdarzeń systemu Windows

Jeśli za pomocą [Śledzenia zdarzeń dla systemu Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803(v=vs.85).aspx) (ETW) i chcesz przechwytywać dzienniki zdarzeń systemu Windows, zaznacz pole wyboru **Włącz przesyłanie dzienniki zdarzeń systemu Windows** . Można zwiększyć lub zmniejszyć liczbę minut po dzienniki są przenoszone do swojego konta miejsca do magazynowania, zmieniając wartość **Transfer okres (min)** .

Zdarzenia są przechwytywane ze źródeł zdarzeń i manifesty zdarzenia, które można określić. Aby określić źródło zdarzenia, wprowadź nazwę w sekcji **Źródła zdarzenia** , a następnie wybierz przycisk **Dodaj źródło zdarzenia** . Podobnie można określić manifest zdarzenia w sekcji **Manifesty zdarzenia** , a następnie wybierz przycisk **Dodaj pojawiają zdarzenia** .

  ![Dzienniki zdarzeń systemu Windows](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766025.png)

  Struktura ETW jest obsługiwana w ASP.NET za pośrednictwem klas w [System.Diagnostics.aspx] (przestrzeń nazw https://msdn.microsoft.com/library/system.diagnostics (v=vs.110). Przestrzeń nazw Microsoft.WindowsAzure.Diagnostics, która dziedziczy i rozszerza standardowe [System.Diagnostics.aspx] (https://msdn.microsoft.com/library/system.diagnostics (v=vs.110) klasy, umożliwia korzystanie z [System.Diagnostics.aspx] (https://msdn.microsoft.com/library/system.diagnostics (v=vs.110) jako ramy rejestrowania w środowisku Azure. Aby uzyskać więcej informacji zobacz [wykonać sterowania z rejestrowania i śledzenia w programie Microsoft Azure](https://msdn.microsoft.com/magazine/ff714589.aspx) i [Włączanie diagnostyki w środowisku maszyn wirtualnych systemu i usług w chmurze Azure](./cloud-services/cloud-services-dotnet-diagnostics.md).

### <a name="crash-dumps"></a>Zrzuty awaryjne

Jeśli chcesz przechwycić informacji na temat kiedy ulega awarii w przypadku wystąpienia roli, zaznacz pole wyboru **Włącz przeniesienie ulec awarii dokonuje zrzutu** . (Ponieważ ASP.NET obsługuje większość wyjątków, to jest zazwyczaj przydatny tylko w przypadku role pracownika). Można zwiększyć lub zmniejszyć wartość procentową ilości miejsca do magazynowania poświęcona zrzuty awarię, zmieniając wartość **Przydziału katalogu (%)** . Możesz zmienić kontenera magazynu miejsce, w którym są przechowywane zrzutów awaryjnych i można określić, czy chcesz przechwycić **Pełny** lub **Mini** zrzutu.

Procesy aktualnie śledzone są widoczne. Zaznacz pola wyboru dla procesów, które chcesz przechwycić. Aby dodać inny proces do listy, wprowadź nazwę procesu, a następnie wybierz przycisk **Dodaj proces** .

  ![Zrzuty awaryjne](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766026.png)

  Zobacz [wykonać kontrolki: rejestrowanie i śledzenie w programie Microsoft Azure](https://msdn.microsoft.com/magazine/ff714589.aspx) i [Microsoft Azure diagnostyki część 4: niestandardowe składniki rejestrowania i zmiany 1.3 diagnostyki Azure](http://justazure.com/microsoft-azure-diagnostics-part-4-custom-logging-components-azure-diagnostics-1-3-changes/) uzyskać więcej informacji.

## <a name="view-the-diagnostics-data"></a>Wyświetlanie danych diagnostyki

Po zgromadzonych danych diagnostyki usługi w chmurze lub maszyny wirtualnej, można go wyświetlić.

### <a name="to-view-cloud-service-diagnostics-data"></a>Aby wyświetlić dane diagnostyki usługi cloud

1. Wdrażanie usługi cloud w zwykły sposób, a następnie uruchom go.

1. Na koncie miejsca do magazynowania można wyświetlać dane diagnostyki w raporcie generowany przez program Visual Studio lub tabel. Aby wyświetlić dane w raporcie, otwórz **Eksploratora chmury** lub **Eksploratora serwera**, otwieranie menu skrótów węzła dla roli interesującą Cię, a następnie wybierz **Widok danych diagnostycznych**.

    ![Wyświetlanie diagnostyki danych](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC748912.png)

    Zostanie wyświetlone raportu, który pokazuje dostępne dane.

    ![Raport diagnostyki platformy Microsoft Azure w programie Visual Studio](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796666.png)

    Jeśli nie widać najnowszych danych, może być konieczne Czekaj na okres przeniesienia.

    Wybierz link **Odśwież** , aby natychmiast zaktualizować dane lub interwał **Automatycznego odświeżania** lista rozwijana dane aktualizowane automatycznie. Aby wyeksportować dane błędu, wybierz przycisk **Eksportuj do CSV** , aby utworzyć plik wartości rozdzielanych przecinkami, który można otworzyć w arkuszu kalkulacyjnym.

    W **Eksploratorze chmury** lub **Server Explorer**Otwórz konta miejsca do magazynowania, który jest skojarzony z wdrożenia.

1. Otwórz tabele diagnostyki w oknie Podgląd tabeli, a następnie przejrzeć danych zebranych. Dzienniki programu IIS i dzienniki niestandardowej możesz otworzyć kontenera obiektów blob. Przeglądając w poniższej tabeli, możesz znaleźć tabeli lub obiektów blob kontener, który zawiera dane, które Cię. Oprócz danych dla tego pliku dziennika, wpisy tabeli zawiera EventTickCount, DeploymentId ról i RoleInstance ułatwiające Określanie, jakie maszyn wirtualnych i roli wygenerowane dane i kiedy. 

  	|Dane diagnostyczne|Opis|Lokalizacja|
  	|---|---|---|
  	|Dzienniki aplikacji|Dzienniki generowane przez wywołanie metody klasy System.Diagnostics.Trace kodu.|WADLogsTable|
  	|Dzienniki zdarzeń|Te dane są z dzienniki zdarzeń systemu Windows w środowisku maszyn wirtualnych systemu. System Windows przechowuje informacje w tych dziennikach, ale aplikacji i usług również używać ich do raportować błędy lub rejestrowanie informacji.|WADWindowsEventLogsTable|
  	|Liczniki wydajności|Na dowolnej licznika, który jest dostępny na komputerze wirtualnych może zbierać dane. System operacyjny zawiera liczniki wydajności, które obejmują wiele statystyki, takie jak godzina użycia i procesor pamięci.|WADPerformanceCountersTable|
  	|Dzienniki infrastruktury|Dzienniki te są generowane przez infrastruktury diagnostyki się.|WADDiagnosticInfrastructureLogsTable|
  	|Dzienniki programu IIS|Dzienniki te rekord żądania sieci web. Jeśli usługa w chmurze dużo ruchu, dzienniki te mogą być bardzo długie, więc należy gromadzenia i przechowywania tych danych, tylko wtedy, gdy jest potrzebny.|Można znaleźć dzienniki żądanie nie powiodło się w kontenerze obiektów blob w obszarze wad usług iis failedreqlogs w obszarze ścieżki dla tego wdrożenia, ról i wystąpienie. Można znaleźć pełną dzienniki w obszarze wad-usług iis-logfiles. Dla każdego pliku wpisów w tabeli WADDirectories.|
  	|Zrzuty awaryjne|Te informacje zawiera obrazy binarne procesu usługi cloud (zazwyczaj w roli pracownika).|kontener obiektów blob wad zgniatania zrzuty|
  	|Pliki dziennika niestandardowego|Dzienniki dane, które można wstępnie zdefiniowane.|Możesz określić w kodzie lokalizacji plików dziennika niestandardowego na koncie miejsca do magazynowania. Na przykład można określić kontenera niestandardowych obiektów blob.|

1. Jeśli wartość zostanie obcięta danych dowolnego typu, można spróbować zmniejszyć buforu dla danych typu lub skrócenia interwał między przekazywanie danych z komputera wirtualnych do Twojego konta miejsca do magazynowania.

1. (opcjonalnie) Czyszczenie danych z konta miejsca do magazynowania od czasu do czasu, aby zmniejszyć ogólny kosztów miejsca do magazynowania.

1. Po wykonaniu pełne wdrożenie plik diagnostics.cscfg (.wadcfgx dla 2,5 SDK Azure) zostanie zaktualizowany w Azure, a usługa w chmurze przejmuje wszelkie zmiany w konfiguracji diagnostyki. Jeśli istniejącego wdrożenia, należy zaktualizować, plik .cscfg nie jest aktualizowany platformy Azure. Ustawienia diagnostyki, można zmienić jednak, wykonując czynności opisane w następnej sekcji. Aby uzyskać więcej informacji na temat wykonywania pełne wdrożenie i aktualizowanie istniejącego wdrożenia zobacz [Kreatora publikowania aplikacji Azure](vs-azure-tools-publish-azure-application-wizard.md).

### <a name="to-view-virtual-machine-diagnostics-data"></a>Aby wyświetlić dane diagnostyki maszyn wirtualnych

1. W menu skrótów dla maszyny wirtualnej wybierz pozycję **Wyświetlanie diagnostyki danych**.

    ![Wyświetlanie danych diagnostyki w Azure maszyn wirtualnych](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766027.png)

    Spowoduje to otwarcie okna **Diagnostyka podsumowania** .

    ![Narzędzia diagnostyczne Azure maszyn wirtualnych podsumowania](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796667.png)  

    Jeśli nie widać najnowszych danych, może być konieczne Czekaj na okres przeniesienia.

    Wybierz link **Odśwież** , aby natychmiast zaktualizować dane lub interwał **Automatycznego odświeżania** lista rozwijana dane aktualizowane automatycznie. Aby wyeksportować dane błędu, wybierz przycisk **Eksportuj do CSV** , aby utworzyć plik wartości rozdzielanych przecinkami, który można otworzyć w arkuszu kalkulacyjnym.

## <a name="configure-cloud-service-diagnostics-after-deployment"></a>Konfigurowanie diagnostyki usługi w chmurze po wdrożeniu

Jeśli masz badania problemu z chmurą usługa tego już uruchomiona, można do zbierania danych nie określisz przed pierwotnie wdrożeniem roli. W takim przypadku możesz rozpocząć zbieranie danych za pomocą ustawień w Eksploratorze serwera. Informacje diagnostyczne dla jedno wystąpienie albo wszystkie wystąpienia można konfigurować w roli, w zależności od tego, czy otworzyć okno dialogowe Konfiguracja diagnostyki z menu skrótów dla tego wystąpienia lub roli. Jeśli skonfigurujesz węzeł roli, wszelkie zmiany dotyczą wszystkich wystąpień. Jeśli skonfigurujesz węzeł wystąpienie, wszelkie zmiany dotyczą tylko to wystąpienie.

### <a name="to-configure-diagnostics-for-a-running-cloud-service"></a>Aby skonfigurować informacje diagnostyczne dla uruchomionego usługi w chmurze

1. W Eksploratorze Server rozwiń węzeł **Usług w chmurze** , a następnie rozwiń węzły, aby zlokalizować roli lub wystąpienia, które chcesz zbadać lub oba.

    ![Konfigurowanie narzędzia diagnostyczne](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC748913.png)

1. W menu skrótów dla węzeł wystąpienia lub roli wybierz pozycję **Ustawienia diagnostyki aktualizacji**, a następnie wybierz diagnostyczne ustawień, które chcesz zebrać.

    Aby uzyskać informacje o ustawieniach konfiguracji zobacz **źródeł danych diagnostyki Konfiguruj** w tym temacie. Aby uzyskać informacji na temat sposobu wyświetlania danych diagnostyki zobacz **Wyświetlanie danych diagnostyki** w tym temacie.

    Jeśli zmienisz zbieranie danych w **Eksploratorze serwera**, te zmiany działają do momentu pełni ponownie wdróż usługi w chmurze. Jeśli korzystasz z domyślnego ustawienia publikowania, wprowadzone zmiany nie są zastępowane, ponieważ publikowanie domyślne ustawienie jest aktualizowanie istniejącego wdrożenia, a nie pełnego ponownego rozmieszczania. Aby upewnić się, że ustawienia wyczyść w czasie rozmieszczania, przejdź na kartę **Ustawienia zaawansowane** w Kreatorze publikowania, a następnie wyczyść pole wyboru **wdrażania aktualizacji** . Gdy ponownie wdróż się przy użyciu tego pola wyboru wyczyszczone, ustawienia przywrócić znajdującymi się w pliku .wadcfgx (lub .wadcfg) określonych za pomocą edytora właściwości dla roli. Po zaktualizowaniu wdrożenia Azure przechowywana stare ustawienia.

## <a name="troubleshoot-azure-cloud-service-issues"></a>Rozwiązywanie problemów z problemy z usługą Azure chmury

Występują problemy z chmury projektów usług, takich jak rolę, która pobiera zatrzymane w stan "zajęty", wielokrotnie odtwarza lub zgłasza błąd serwera wewnętrznego, istnieje narzędzi i technik używanych do diagnozowanie i rozwiązywanie tych problemów. Przykłady typowych problemów i rozwiązań, a także omówiono pojęcia i narzędzia używane do diagnozowania i naprawiania tych błędów zobacz [Azure PaaS obliczyć diagnostyki danych](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).

## <a name="q--a"></a>Pytania i odpowiedzi

**Co to jest rozmiar buforu i rozmiar, jaki ma być?**

W każdym wystąpieniu maszyn wirtualnych przydziałów ograniczyć ilość danych diagnostycznych mogą być przechowywane na lokalny system plików. Ponadto możesz określić rozmiar buforu dla każdego typu diagnostyczne dane, które są dostępne. Tego rozmiaru zachowuje się jak indywidualny przydział dla tego typu danych. Zaznaczając dolnej części okna dialogowego, można określić całkowitej ilości i ilości pamięci, która pozostaje. Jeśli użytkownik określi buforów większych lub więcej typów danych, będzie podejście do całkowitej ilości. Całkowitej ilości można zmienić, modyfikując plik konfiguracji diagnostics.wadcfg/.wadcfgx. Dane diagnostyki są przechowywane na tym samym plików jako dane aplikacji, więc jeśli aplikacja używa dużo miejsca na dysku, nie powinny zwiększyć ogólną przydział diagnostyki.

**Co to jest okres przeniesienia, i jak długo ma być?**

Okres przeniesienia jest ilość czasu, w którym upłynie między danymi. Po każdym okresie transfer danych jest przenoszona z lokalnego systemu plików na komputerze wirtualnych do tabel na koncie miejsca do magazynowania. Jeśli zakres danych, które są zbierane przed upływem okresu przeniesienia, starsze dane zostaną odrzucone. Można zmniejszyć okres przeniesienia, jeśli w przypadku utraty danych, ponieważ danych przekracza rozmiar buforu lub całkowitej ilości.

**Strefy czasowej są sygnatury czasowe w?**

Sygnatury czasowe znajdują się w lokalnej strefie czasowej centrum danych, który obsługuje usługa w chmurze. Są używane następujące trzy kolumny sygnatury czasowej w tabelach dziennika.

  - **PreciseTimeStamp** jest sygnatura czasowa ETW zdarzenia. Oznacza to, że czas to zdarzenie jest rejestrowane w kliencie.

  - **Sygnatura CZASOWA** jest zaokrąglana w dół do granicy częstotliwości przekazywania PreciseTimeStamp. Tak Jeśli częstotliwość przekazywania jest 5 minut i zdarzenia godzina 00:17:12, sygnatura CZASOWA będzie 00:15:00.

  - **Sygnatura czasowa** jest sygnatura czasowa, jaką jednostki został utworzony w tabeli Azure.

**Jak zarządzać kosztami, podczas zbierania informacji diagnostycznych?**

Ustawienia domyślne (**poziom rejestrowania** do **błędu** i **Przełączanie okres** równa **1 minuta**) są przeznaczone do Minimalizuj koszt. Koszty obliczeń zwiększa zbieranie większej ilości danych diagnostycznych lub Zmniejsz okresu przeniesienia. Nie zbieranie większej ilości danych, niż potrzebujesz, a nie zapomnij wyłączyć zbierania danych, gdy już nie potrzebujesz. Możesz zawsze włączyć ją ponownie, nawet w czasie wykonywania, jak pokazano w poprzedniej sekcji.

**Jak zbieranie dzienników żądanie nie powiodło się z programu IIS?**

Domyślnie usług IIS nie zbieranie dzienników żądanie nie powiodło się. Można skonfigurować usług IIS do zbierania ich edytowania pliku web.config dla ról w sieci web.

**Nie są zwracane informacje o śledzeniu z metod RoleEntryPoint, takich jak OnStart. Co jest nie tak?**

Metody RoleEntryPoint są nazywane w kontekście WAIISHost.exe, nie IIS. W związku z tym informacje o konfiguracji w pliku web.config, które zwykle nie dotyczy umożliwia śledzenie. Aby rozwiązać ten problem, dodawanie pliku .config do projektu roli sieci web i nazwę pliku, aby był zgodny zestaw danych wyjściowych, który zawiera kod RoleEntryPoint. W programie project roli sieci web domyślny nazwę pliku .config będzie WAIISHost.exe.config. Następnie dodaj następujące wiersze do tego pliku:

```
<system.diagnostics>
  <trace>
      <listeners>
          <add name “AzureDiagnostics” type=”Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitorTraceListener”>
              <filter type=”” />
          </add>
      </listeners>
  </trace>
</system.diagnostics>
```

Teraz w oknie **Właściwości** ustaw właściwość **Kopiuj do katalogu wyjściowego** **zawsze Kopiuj**.

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej o diagnostyce rejestrowanie w Azure, zobacz [Włączanie diagnostyki w środowisku maszyn wirtualnych systemu i usług w chmurze Azure](./cloud-services/cloud-services-dotnet-diagnostics.md) i [Włącz diagnostyki rejestrowania aplikacji sieci web w usłudze Azure aplikacji](./app-service-web/web-sites-enable-diagnostic-log.md).
