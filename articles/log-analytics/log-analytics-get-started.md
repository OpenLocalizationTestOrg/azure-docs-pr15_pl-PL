<properties
    pageTitle="Wprowadzenie do analizy dziennika | Microsoft Azure"
    description="Zostanie wyświetlony i rozpocząć pracę z dziennika analizy w programie Microsoft operacje zarządzania pakietu (usługi OMS), w minutach."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/10/2016"
    ms.author="banders"/>


# <a name="get-started-with-log-analytics"></a>Wprowadzenie do analizy dziennika

Zostanie wyświetlony i rozpocząć pracę z dziennika analizy w programie Microsoft operacje zarządzania pakietu (usługi OMS), w minutach. Podczas wybierania sposobu tworzenia usługi OMS obszar roboczy, który jest podobny do konta istnieją dwie opcje:

- Pakiet zarządzania operacje Microsoft witryny sieci Web
- Subskrypcja Microsoft Azure

Korzystanie z witryny internetowej usługi OMS bezpłatne obszar roboczy usługi OMS można utworzyć. Lub subskrypcji Microsoft Azure umożliwia tworzenie obszaru roboczego usługi OMS. Oba obszary robocze działają tak samo, z wyjątkiem, że bezpłatnej usługi OMS obszaru roboczego można wysyłać tylko 500 MB danych codziennie do usługę. Jeśli korzystasz z subskrypcji usługi Azure, można uzyskać dostęp do innych usług Azure w tej subskrypcji. Bez względu na metodę, która umożliwia tworzenie obszaru roboczego utworzysz obszaru roboczego przy użyciu konta Microsoft lub konto organizacji.

Poniżej przedstawiono informacje o procesie:

![diagram ułatwiającej rozpoczęcie korzystania](./media/log-analytics-get-started/oms-onboard-diagram.png)

## <a name="log-analytics-prerequisites-and-deployment-considerations"></a>Wymagania wstępne dotyczące dziennika analizy i zagadnienia dotyczące rozmieszczania

- Potrzebujesz płatnej subskrypcji Microsoft Azure, aby w pełni wykorzystać analizy dziennika. Jeśli nie masz subskrypcji usługi Azure, należy utworzyć [bezpłatne konto](https://azure.microsoft.com/free/) , które umożliwia uzyskanie dostępu do jakichkolwiek usług Azure. Lub można utworzyć bezpłatne konto usługi OMS w [Pakiecie zarządzania operacje](http://microsoft.com/oms) w sieci Web i kliknij pozycję **Wypróbuj bezpłatnie**.
- Obszar roboczy usługi OMS
- Każdy komputer systemu Windows, którą chcesz zebrać dane z musi uruchomić systemu Windows Server 2008 z dodatkiem SP1 lub nowszym
- Adresy usługi sieci web [zaporą](log-analytics-proxy-firewall.md) dostęp do usługi OMS
- Serwer [Usługi OMS dziennika analizy usługi przesyłania dalej](https://blogs.technet.microsoft.com/msoms/2016/03/17/oms-log-analytics-forwarder) (bramy) do przesyłania ruchu z serwerów usługi OMS, jeśli dostęp do Internetu nie jest dostępny na komputerach
- Jeśli korzystasz z programu Operations Manager obsługuje analizy dziennika programu Operations Manager 2012 z dodatkiem SP1 UR6 oraz powyżej i Operations Manager 2012 R2 UR2 i powyżej. Obsługi serwera proxy został dodany w Operations Manager 2012 z dodatkiem SP1 UR7 i Operations Manager 2012 R2 UR3. Określ, jak będzie można zintegrować z usługi OMS.
- Sprawdzić, czy komputery się bezpośredni dostęp do Internetu. Jeśli nie wymagają serwer bramy, aby uzyskać dostęp do witryn usługi sieci web usługi OMS. Dostęp jest przy użyciu protokołu HTTPS.
- Określanie, które technologie i serwery będzie wysyłać dane do usługi OMS. Na przykład kontrolery, SQL Server, itd.
- Udzielanie uprawnień użytkownikom usługi OMS i Azure.
- Jeśli dane dotyczące użycia danych, wdrażanie każdego z rozwiązań pojedynczo i przetestuj wpływ na wydajność przed dodaniem dodatkowe rozwiązania.
- Przejrzyj wydajność i korzystanie z danych podczas dodawania rozwiązań i funkcji analizy dziennika. W tym zbiorze zdarzeń, zbieranie dzienników, zbieranie danych dotyczących wydajności itp. Lepiej jest zaczynać się minimalnymi zbioru do zastosowania danych lub wykryto wpływ na wydajność.
- Sprawdź, czy agenci systemu Windows nie są także zarządzane za pomocą programu Operations Manager w przeciwnym razie spowoduje zduplikowanych danych. Dotyczy to również Azure podstawie agentów mające diagnostyki Azure włączone.
- Po zainstalowaniu czynników, upewnij się, że agent działa poprawnie. Jeśli nie, sprawdź, upewnij się, że kryptograficzny interfejs API: izolacji klucza następnej generacji (CNG) nie jest wyłączona, za pomocą zasad grupy.
- Niektóre rozwiązania analizy dziennika mają dodatkowe wymagania



## <a name="sign-up-in-3-steps-using-the-operations-management-suite"></a>Utworzyć konto w krokach 3 przy użyciu pakietu zarządzania operacji

1. Przejdź do witryny sieci Web [Pakietu zarządzania operacji](http://microsoft.com/oms) , a następnie kliknij pozycję **Wypróbuj bezpłatnie**. Zaloguj się przy użyciu konta Microsoft, takich jak Outlook.com lub za pomocą konta organizacji dostarczony przez Twoją firmę lub instytucji edukacyjnej korzystać z usługi Office 365 lub innych usług firmy Microsoft.
2. Podaj nazwę unikatowe obszaru roboczego. Obszar roboczy jest kontenerem logiczne przechowywania danych zarządzania. Umożliwia umożliwia partycją danych między różnych zespołów w organizacji, jak danych jest zastrzeżona do obszaru roboczego. Określ adres e-mail i region, miejsce, w którym ma być przechowywane dane.  
    ![Tworzenie obszaru roboczego i połączyć subskrypcji](./media/log-analytics-get-started/oms-onboard-create-workspace-link01.png)
3. Następnie możesz utworzyć nowej subskrypcji Azure lub utworzyć łącze do istniejącej subskrypcji usługi Azure. Jeśli chcesz kontynuować, za pomocą bezpłatnej wersji próbnej, kliknij pozycję **Nie teraz**.  
  ![Tworzenie obszaru roboczego i połączyć subskrypcji](./media/log-analytics-get-started/oms-onboard-create-workspace-link02.png)

Możesz rozpocząć pracę z portalem pakiet administracyjny operacji.

Więcej informacji o konfigurowaniu obszaru roboczego i łączenie Azure istniejącego konta do obszarów roboczych utworzonych z pakietem zarządzania operacje na [Zarządzanie dostępem do analizy dziennika](log-analytics-manage-access.md).

## <a name="sign-up-quickly-using-microsoft-azure"></a>Zaloguj szybko przy użyciu Microsoft Azure

1. Przejdź do [portalu Azure](https://portal.azure.com) i zaloguj się, przejdź na liście usług, a następnie zaznacz **Usługi dziennika analizy (OMS)**.  
    ![Azure portal](./media/log-analytics-get-started/oms-onboard-azure-portal.png)
2. Kliknij przycisk **Dodaj**, a następnie wybierz opcje dla następujących elementów:
    - Nazwa **Usługi OMS obszaru roboczego**
    - **Subskrypcja** — Jeśli masz wiele subskrypcji, wybierz język, który chcesz skojarzyć z nowego obszaru roboczego.
    - **Grupa zasobów**
    - **Lokalizacja**
    - **Cennik warstwy**  
        ![Szybkie tworzenie](./media/log-analytics-get-started/oms-onboard-quick-create.png)
3. Kliknij pozycję **Utwórz,** Aby wyświetlić szczegóły obszaru roboczego w portalu Azure.       
    ![Szczegóły obszaru roboczego](./media/log-analytics-get-started/oms-onboard-workspace-details.png)         
4. Kliknij łącze **Portal usługi OMS** , aby otworzyć witrynę internetową pakiet administracyjny operacje z nowego obszaru roboczego.

Możesz rozpocząć korzystanie z portalu pakiet administracyjny operacji.

Więcej informacji o konfigurowaniu obszaru roboczego i łączenie istniejących obszarów roboczych utworzonych z pakietem zarządzania operacje do Azure subskrypcje w [Zarządzanie dostępem do analizy dziennika](log-analytics-manage-access.md).

## <a name="get-started-with-the-operations-management-suite-portal"></a>Wprowadzenie do portalu pakiet administracyjny operacji
Aby wybrać rozwiązań i połączyć serwerów, które chcesz zarządzać, kliknij kafelka **Ustawienia** , a następnie wykonaj czynności opisane w tej sekcji.  

![Rozpoczynanie pracy](./media/log-analytics-get-started/oms-onboard-get-started.png)  

1. **Dodawanie rozwiązań** - wyświetlanie zainstalowanych rozwiązanie.  
    ![rozwiązania](./media/log-analytics-get-started/oms-onboard-solutions.png)  
    Kliknij pozycję **można znaleźć w galerii** , aby dodać więcej rozwiązań.  
    ![rozwiązania](./media/log-analytics-get-started/oms-onboard-solutions02.png)  
    Wybierz rozwiązanie, a następnie kliknij przycisk **Dodaj**.
2. **Łączenie źródła** — wybierz sposób nawiązywania połączenia z środowiska serwera do zbierania danych:
    - Połącz ani Windows Server klienta bezpośrednio instalując agenta.
    - Łączenie serwerów Linux z agentem usługi OMS Linux.
    - Użyj konta magazynu platformy Azure skonfigurowane z systemu Windows i Linux oraz Azure diagnostyczne maszyn wirtualnych rozszerzeniem.
    - System Center Operations Manager umożliwia dołączanie grup zarządzania lub całego wdrożenia programu Operations Manager.
    - Włącz telemetrycznego Windows umożliwia uaktualnianie analizy.
        ![połączonych źródeł](./media/log-analytics-get-started/oms-onboard-data-sources.png)    

3. **Zbieranie danych** Skonfiguruj co najmniej jedno źródło danych do wypełnienia danych do obszaru roboczego. Po zakończeniu kliknij przycisk **Zapisz**.    

    ![zbieranie danych](./media/log-analytics-get-started/oms-onboard-logs.png)    


## <a name="optionally-connect-servers-directly-to-the-operations-management-suite-by-installing-an-agent"></a>Opcjonalnie możesz podłączyć serwery bezpośrednio w pakiecie zarządzania operacje instalując agenta

W poniższym przykładzie pokazano, jak zainstalować program Windows agent.

1. Kliknij **Ustawienia** , kliknij kartę **Połączenia źródła** , kliknij kartę dla typ źródła, które chcesz dodać, a albo pobierania agenta lub Dowiedz się, jak włączyć agenta. Na przykład kliknij przycisk **Pobierz Agent systemu Windows (wersja 64-bitowa)**. Agentami systemu Windows można zainstalować tylko agenta w systemie Windows Server 2008 z dodatkiem SP1 lub nowszym lub w systemie Windows 7 z dodatkiem SP1 lub nowszym.
2. Instalowanie agenta na jeden lub więcej serwerów. Możesz zainstalować agentów po kolei lub więcej zautomatyzowana metoda za pomocą [skryptu niestandardowego](log-analytics-windows-agents.md), lub można użyć istniejącej rozwiązanie dystrybucji oprogramowania, które może być.
3. Po akceptujesz Umowę licencyjną i Wybieranie folderu instalacji, zaznacz pole wyboru **Połącz agenta do usługi Azure analizy dziennika (OMS)**.   
    ![Konfigurowanie agenta](./media/log-analytics-get-started/oms-onboard-agent.png)

4. Na następnej stronie zostanie wyświetlony monit dla swojego Identyfikatora obszaru roboczego i klucza obszaru roboczego. Swoją nazwę obszaru roboczego i klucza są wyświetlane na ekranie, gdzie został pobrany plik agenta.  
    ![Agent klawiszy](./media/log-analytics-get-started/oms-onboard-mma-keys.png)  

    ![Dołączanie serwerów](./media/log-analytics-get-started/oms-onboard-key.png)
5. Podczas instalacji możesz kliknąć pozycję **Zaawansowane** , aby opcjonalnie Konfigurowanie serwera proxy i podać informacje uwierzytelniające. Kliknij przycisk **Dalej** , aby powrócić do ekranu informacji obszaru roboczego.
6. Kliknij przycisk **Dalej** do sprawdzania poprawności swoją nazwę obszaru roboczego i klucza. Jeśli nie zostaną znalezione jakiekolwiek błędy, możesz kliknąć pozycję **Wstecz** , aby wprowadzić poprawki. Po zatwierdzeniu swoją nazwę obszaru roboczego i klucza kliknij przycisk **Zainstaluj** do ukończenia instalacji agenta.
7. W Panelu sterowania kliknij pozycję Microsoft Agent monitorowania > karta usługi Azure dziennika analizy (OMS). Zostanie wyświetlona ikona zielony znacznik wyboru, gdy agentów komunikowania się z usługą pakiet administracyjny operacji. Początkowo trwa to około 5-10 minut.

>[AZURE.NOTE] Rozwiązania oceny zarządzania i konfigurowania możliwości nie są obecnie obsługiwane przez serwery podłączone bezpośrednio do pakietu zarządzania operacji.


W przypadku nawiązywania połączenia agenta, aby System Center Operations Manager 2012 z dodatkiem SP1 lub nowszym. Aby to zrobić, zaznacz pole wyboru **Połącz agenta System Center Operations Manager**. Po wybraniu że opcji Wyślij dane do usługi bez konieczności dodatkowego sprzętu lub Obciążenie grup zarządzania.

Więcej o nawiązywaniu połączeń agentów w pakiecie zarządzania operacje na [komputerach Łączenie systemu Windows do analizy dziennika](log-analytics-windows-agents.md).

## <a name="optionally-connect-servers-using-system-center-operations-manager"></a>Opcjonalnie można połączyć serwerów przy użyciu System Center Operations Manager

1. W konsoli programu Operations Manager zaznacz **administracji**.
2. Rozwiń węzeł **Operacyjne wniosków** i wybierz **Operacyjne połączenie wniosków**.

  >[AZURE.NOTE] W zależności od tego, jakie aktualizacji zestawienie z SCOM używasz można zobaczyć węzeł *Advisor Centrum systemu*, *Operacyjne wniosków*lub *Pakiet administracyjny operacji*.

3. Kliknij łącze **Zarejestruj, aby operacyjne wniosków** do góry po prawej stronie, a następnie postępuj zgodnie z instrukcjami.
4. Po zakończeniu Kreatora rejestracji, kliknij łącze **Dodaj komputera lub grupy** .
5. W oknie dialogowym **Wyszukiwania komputera** można wyszukiwać komputery lub grupy kontrolowane przez programu Operations Manager. Zaznacz komputery lub grupy, aby płycie je do analizy dziennika, kliknij przycisk **Dodaj**, a następnie kliknij **przycisk OK**. Można sprawdzić, czy usługę otrzymuje danych, przechodząc do kafelków **zastosowania** w portalu pakiet administracyjny operacji. Dane będą widoczne w około 5-10 minut.

Więcej o nawiązywaniu połączeń programu Operations Manager w pakiecie zarządzania operacje na [Łączenie analizy dziennika programu Operations Manager](log-analytics-om-agents.md).

## <a name="optionally-analyze-data-from-cloud-services-in-microsoft-azure"></a>Opcjonalnie analizowania danych z usług w chmurze w programie Microsoft Azure

Z pakietem zarządzania operacji możesz szybko wyszukać dzienniki usług IIS dla usług w chmurze i maszyn wirtualnych zdarzeń i, włączając diagnostyki dla usług w chmurze Azure. Dodatkowe informacje na temat technologii może również odbierać na potrzeby Azure maszyn wirtualnych, instalując program Microsoft Agent monitorowania. Więcej o konfigurowaniu środowiska usługi Azure używania pakietu zarządzania operacje w [Magazyn łączenie Azure analizy dziennika](log-analytics-azure-storage.md).


## <a name="next-steps"></a>Następne kroki

- [Dodawanie analizy dziennika rozwiązań z galerii rozwiązań](log-analytics-add-solutions.md) Dodawanie funkcji do zbierania danych.
- Zapoznaj się z [wyszukiwania dziennika](log-analytics-log-searches.md) wyświetlić szczegółowe informacje zgromadzone przez rozwiązań.
- Zapisywanie i wyświetlanie niestandardowych wyszukiwania przy użyciu [pulpitów nawigacyjnych](log-analytics-dashboards.md) .
