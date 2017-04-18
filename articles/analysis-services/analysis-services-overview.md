<properties
   pageTitle="Co to jest Azure Analysis Services | Microsoft Azure"
   description="Uzyskaj przeglądu usług Analysis Services w Azure."
   services="analysis-services"
   documentationCenter=""
   authors="minewiskan"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="analysis-services"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="owend"/>

# <a name="what-is-azure-analysis-services"></a>Co to jest Azure Analysis Services?
![Azure Analysis Services](./media/analysis-services-overview/aas-overview-aas-icon.png)

Azure usług Analysis Services jest oparty na aparat sprawdzone analizy w programie Microsoft SQL Server Analysis Services, i pozwala klasy korporacyjnej modelowanie danych w chmurze.

> [AZURE.IMPORTANT] Azure usług Analysis Services jest w **podglądzie**. Istnieje kilka rzeczy, które po prostu nie działają jeszcze. Pamiętaj wyewidencjonować [Podgląd oczekiwania](#preview-expectations) w dalszej części tego artykułu. I upewnij się, w celu kontroli w naszym [blogu Azure usług Analysis Services](https://go.microsoft.com/fwlink/?linkid=830920) , aby uzyskać najnowsze informacje.

## <a name="built-on-sql-server-analysis-services"></a>Graficzny oparty na usług SQL Server Analysis Services
Azure usług Analysis Services jest zgodny z tym samym SQL Server 2016 Analysis Services Enterprise Edition znasz już. Azure usług Analysis Services obsługuje modeli tabelarycznych na poziomie zgodności 1200. Zapytania bezpośredniego, partycje zabezpieczeń na poziomie wiersza, dwukierunkowego relacje i tłumaczenie są obsługiwane.

## <a name="use-the-tools-you-already-know"></a>Korzystanie z narzędzi, które znasz już
![Narzędzia do analizy BIZNESOWEJ — Deweloper](./media/analysis-services-overview/aas-overview-dev-tools.png)

Tworząc modele danych usług Analysis Services Azure, użyj tych samych narzędzi dla usług SQL Server Analysis Services. Tworzenie i wdrażanie modeli tabelarycznych za pomocą najnowszej wersji programu [SQL Server danych narzędzia (SSDT)](https://msdn.microsoft.com/library/mt204009.aspx) lub przy użyciu [Programu Powershell Azure](../powershell-install-configure.md) i [Menedżera zasobów Azure](../azure-resource-manager/resource-group-overview.md) szablonów programu [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx).

## <a name="connect-to-data-sources"></a>Połączenia ze źródłami danych
Modele danych wdrażane na serwerach w obsłudze Azure nawiązywanie połączeń danych źródła lokalnego w organizacji lub w chmurze. Łączenie danych z obu lokalnego i źródła danych dla hybrydowych rozwiązanie analizy Biznesowej w chmurze.

![Źródła danych](./media/analysis-services-overview/aas-overview-data-sources.png)

Ponieważ serwer jest w chmurze, połączenia ze źródłami danych w chmurze jest bezproblemowa. Podczas łączenia z lokalnych źródeł danych, [bramy danymi lokalnego](analysis-services-gateway.md) zapewnia szybki, bezpiecznego połączenia z serwerem usług Analysis Services w chmurze.  

 \*Niektóre źródła danych nie są jeszcze obsługiwane w oknie podglądu. Aby uzyskać więcej informacji, zobacz [oczekiwań Podgląd](#preview-expectations) w dalszej części tego artykułu.

## <a name="explore-your-data-from-anywhere"></a>Eksplorowanie danych z dowolnego miejsca
Nawiązywanie połączenia i [Pobieranie danych](analysis-services-connect.md) z serwerów z o dowolnego miejsca. Azure usług Analysis Services obsługuje nawiązywanie połączenia z Power BI Desktop, Excel, niestandardowe aplikacje i narzędzi przeglądarki.

![Wizualizacje danych](./media/analysis-services-overview/aas-overview-visualization.png)

 \*Power BI osadzony nie jest jeszcze obsługiwane w oknie podglądu.

## <a name="secure"></a>Zabezpieczanie

#### <a name="user-authentication"></a>Uwierzytelnianie użytkownika
Uwierzytelnianie użytkownika dla usług Azure Analysis services jest obsługiwany przez [Azure Active Directory (AAD)](../active-directory/active-directory-whatis.md). Podczas próby logowania do bazy danych usług Analysis Services Azure, użytkowników za pomocą tożsamości konta organizacji dostęp do bazy danych, którą chce uzyskać dostęp. Tych tożsamości użytkownika muszą być członkami domyślne usługi Azure Active Directory dla subskrypcji, gdzie znajduje się serwer Azure usług Analysis Services. [Integracja katalogu](https://technet.microsoft.com/library/jj573653.aspx) między AAD i usługi Active Directory w lokalnej to doskonały sposób, aby przejść do lokalnego użytkownikom dostęp do bazy danych usług Analysis Services Azure, ale nie jest wymagane dla wszystkich scenariuszy.

Użytkownicy, zaloguj się głównej nazwy użytkownika (UPN) swojego konta i swoje hasło. Synchronizowane z lokalnej usługi Active Directory, UPN użytkownika jest zazwyczaj adres e-mail organizacji.

Uprawnienia związane z zarządzaniem zasobów serwera Azure Analysis Services są obsługiwane przez przypisywanie użytkowników do ról w ramach subskrypcji usługi Azure. Domyślnie subskrypcji Administratorzy mają uprawnienia właściciela zasobów serwera platformy Azure. Dodatkowych użytkowników można dodawać za pomocą Menedżera zasobów Azure.

#### <a name="data-security"></a>Bezpieczeństwo danych
Azure usług Analysis Services wykorzystuje magazyn obiektów Blob platformy Azure pozostać miejsca do magazynowania i metadanych dla baz danych usług Analysis Services. Pliki danych w ramach obiektów Blob są szyfrowane przy użyciu szyfrowania po stronie serwera Azure obiektów Blob (SSE). W trybie zapytania bezpośredniego znajduje się tylko metadane; rzeczywistych danych jest dostępny ze źródła danych w czasie kwerendy.

#### <a name="on-premises-data-sources"></a>Lokalnych źródeł danych
Bezpieczny dostęp do danych przechowywanych lokalnie w organizacji można osiągnąć, instalowanie i konfigurowanie [bramy danymi lokalnego](analysis-services-gateway.md). Bram zapewniają dostęp do danych dla zapytania bezpośredniego i trybów w pamięci. Gdy model usług Analysis Services Azure łączy się z lokalnego źródła danych, kwerendy zostanie utworzona wraz z zaszyfrowane poświadczenia dla lokalnego źródła danych. Usługa w chmurze brama analizuje kwerendy i umieszcza żądania do Bus usługi Azure. Brama lokalnego sprawdza Bus usługi Azure dla oczekujących żądań. Następnie bramy otrzymuje kwerendę, odszyfrowuje poświadczenie za i nawiązuje połączenie ze źródłem danych do wykonania. Powrót do bramy, a następnie do bazy danych usług Analysis Services Azure, wyniki następnie są wysyłane ze źródła danych.

Azure usług Analysis Services podlega [Postanowienia dotyczące usług Online firmy Microsoft](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) i [Microsoft Online Services zasady zachowania poufności informacji](https://www.microsoft.com/privacystatement/OnlineServices/Default.aspx).
Aby dowiedzieć się więcej o zabezpieczeniach Azure, zobacz [Centrum zaufania programu Microsoft](https://www.microsoft.com/trustcenter/Security/AzureSecurity).

## <a name="get-help"></a>Uzyskiwanie pomocy
Azure usług Analysis Services jest proste, aby skonfigurować i zarządzać. Można znaleźć wszystkie informacje potrzebne do tworzenia i obsługi serwera. Podczas tworzenia modelu danych do wdrażania na serwerze, jest tak samo podobnie jak w przypadku tworzenia modelu danych, które Wdroż lokalnego serwera. Istnieje rozległa biblioteki koncepcyjny, procedurach, samouczki i artykuły odwołanie w [Usług Analysis Services w witrynie MSDN](https://msdn.microsoft.com/library/bb522607.aspx).

Również mamy wiele klipów wideo pomocne w [Azure usług Analysis Services na kanału 9](https://channel9.msdn.com/series/Azure-Analysis-Services).

Co zmieniasz szybko. Zawsze można uzyskać najnowsze informacje o [blogu Azure usług Analysis Services](https://go.microsoft.com/fwlink/?linkid=830920).

## <a name="community"></a>Społeczności
Usług Analysis Services zawiera dynamiczne społeczności użytkowników. Dołączanie do konwersacji na [forum Azure usług Analysis Services](https://aka.ms/azureanalysisservicesforum).

## <a name="feedback"></a>Opinie
Masz sugestie lub funkcji? Pamiętaj o komentarz na [Azure Analysis Services opinii](https://aka.ms/azureanalysisservicesfeedback).

Masz sugestie dotyczące dokumentacji? Możesz dodać komentarze, przy użyciu Disqus u dołu każdego artykułu.

## <a name="preview-expectations"></a>Podgląd oczekiwań
Azure usług Analysis Services jest obecnie w podglądzie. Istnieje kilka rzeczy, które należy pamiętać o.

##### <a name="server-modes"></a>Tryby serwera
Azure usług Analysis Services obsługuje obecnie tryb tabelaryczny modelach tabelarycznych na poziomie zgodności 1200. Tryb wielowymiarowe i wyszukiwania danych i dodatku Power Pivot w trybie programu SharePoint nie są obsługiwane.

##### <a name="data-sources"></a>Źródła danych
W polu Podgląd następujących źródeł danych są obsługiwane w 1200 modelach tabelarycznych wdrożonych serwer Azure usług Analysis Services.

| **Chmury** | **Lokalne** |
|--------------|------------|
| Baza danych SQL | Program SQL Server |
| Magazyn danych SQL | DOSTĘPU |
|      | Oracle |
|       | Teradata |


### <a name="data-source-providers"></a>Dostawców źródeł danych
Modele danych w usługach Analysis Services Azure może wymagać dostawców innych danych w celu połączenia ze źródłami danych niż modele danych w programie SQL Server Analysis Services. Wymagania dostawcy danych zależą od tego, jest źródła danych w chmurze lub lokalnego i typu modelu danych; Kwerenda w pamięci lub bezpośrednich. Aby uzyskać więcej informacji, zobacz [połączenia źródła danych](analysis-services-datasource.md).


### <a name="client-connections"></a>Połączeń klienta
Power BI osadzony nie jest jeszcze obsługiwane w oknie podglądu.

Skoroszyty programu Excel z live połączenia serwer usług Analysis Services Azure i zapisane w usłudze OneDrive lub SharePoint Online nie są obsługiwane.



## <a name="next-steps"></a>Następne kroki
Po zapoznaniu się więcej na temat usług Analysis Services Azure, nadszedł czas na rozpoczęcie pracy. Dowiedz się, jak [utworzyć serwer](analysis-services-create-server.md) Azure i [Wdrażanie modelu tabelarycznego](analysis-services-deploy.md) do niego.
