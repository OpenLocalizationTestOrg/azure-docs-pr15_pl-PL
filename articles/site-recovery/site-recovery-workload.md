<properties
    pageTitle="Jakie obciążenia można chronić przy użyciu Odzyskiwanie witryny Azure?"
    description="Odzyskiwanie witryny Azure chroni obciążenia i aplikacji przez koordynowanie replikacji, pracy awaryjnej i odzyskiwanie maszyn wirtualnych lokalnego i serwerów fizycznych Azure lub do witryny lokalnej pomocniczej"
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="cfreeman"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="10/10/2016"
    ms.author="raynew"/>

# <a name="what-workloads-can-you-protect-with-azure-site-recovery"></a>Jakie obciążenia można chronić przy użyciu Odzyskiwanie witryny Azure?


Ten artykuł zawiera opis obciążenia i aplikacje, które można odtworzyć z usługą Azure Odzyskiwanie witryny.

Publikowanie jakieś komentarze lub pytania u dołu tego artykułu lub [Azure odzyskiwania usług Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)w witrynie.

## <a name="overview"></a>Omówienie

Organizacje muszą firm ciągłości i danych (BCDR) strategii odzyskiwania Aby zachować obciążenia i danych bezpiecznych i dostępne podczas zaplanowane i niezaplanowane przestoje i odzyskiwanie warunkom pracę zwykłą tak szybko, jak to możliwe.

Odzyskiwanie witryny to usługa Azure do strategii BCDR. Za pomocą Odzyskiwanie witryny, można wdrażać aplikacji replikacji w chmurze lub do witryny pomocniczą. Czy są Twoje aplikacje systemu Windows lub systemem Linux, z uruchomionymi na serwerach fizycznie, VMware lub funkcji Hyper-V, aby dodać akompaniament replikacji można użyć Odzyskiwanie witryny, wykonywanie testów odzyskiwania danych i wykonywania pracy awaryjnej i powrotu.


Odzyskiwanie witryny można zintegrować z aplikacji firmy Microsoft, w tym programu SharePoint, Exchange, Dynamics, SQL Server i usługi Active Directory. Microsoft również ściśle współpracuje z wiodących dostawców, w tym Oracle, SAP, IBM i funkcję czerwony. Możesz dostosować rozwiązania replikacji na podstawie aplikacji przez aplikację.

## <a name="why-use-site-recovery-for-application-replication"></a>Dlaczego warto używać Odzyskiwanie witryny replikacji aplikacji?

Odzyskiwanie witryny składa się na poziomie aplikacji ochrony i odzyskiwania w następujący sposób:

- Aplikacja-niezależne od, dostarczając replikacji dla dowolnego obciążenia uruchomione na komputerze obsługiwane.
- W pobliżu synchroniczne replikacji z RPOs możliwie jak 30 sekund do potrzeb najważniejszych aplikacji biznesowych.
- Spójne aplikacji migawek dla jednego lub wielu aplikacji.
- Integracja z programu SQL Server (AlwaysOn) i współpracy z innymi replikacji na poziomie aplikacji technologii, w tym replikacji AD SQL (AlwaysOn), grupy dostępność bazy danych programu Exchange (DAGs) i zabezpieczenia danych programu Oracle.
- Elastyczne odzyskiwania programów, które umożliwiają odzyskiwanie stos całą aplikację za pomocą jednego kliknięcia i dołączyć do uwzględnienia skryptów zewnętrznych i ręczne akcji w planie.
- Zarządzanie siecią Zaawansowane w Odzyskiwanie witryny i Azure uprościć wymagania sieci aplikacji, łącznie z możliwością rezerwowanie adresy IP, skonfigurować równoważenia obciążenia i integracji Azure ruch Menedżera switchovers sieci RTO niski.
-  Biblioteka sformatowanego automatyzacji zawierającego skryptów produkcji gotowe, specyficzne dla aplikacji, które można pobrać i zintegrowany z planów odzyskiwania.



## <a name="workload-summary"></a>Podsumowanie obciążenie pracą

Odzyskiwanie witryny można replikować dowolnej aplikacji uruchomione na komputerze obsługiwane. Ponadto współpracujemy w członkom zespołu produktu do przeprowadzania dodatkowe testując specyficzne dla aplikacji.

**Obciążenie pracą** | **Powielić funkcji Hyper-V maszyny wirtualne do witryny pomocniczej** | **Powielić maszyny wirtualne funkcji Hyper-V Azure** | **Powielić VMware maszyny wirtualne do witryny pomocniczej** | **Powielić maszyny wirtualne VMware Azure**
---|---|---|---|---
Usługa Active Directory, DNS | Y | Y | Y | Y
Aplikacje sieci Web (IIS, SQL) | Y | Y | Y | Y
System Center Operations Manager | Y | Y | Y | Y
Programu SharePoint | Y | Y | Y | Y
SAP<br/><br/>Replikacja witryny SAP Azure-klaster | Y (sprawdzone przez firmę Microsoft) | Y (sprawdzone przez firmę Microsoft) | Y (sprawdzone przez firmę Microsoft) | Y (sprawdzone przez firmę Microsoft)
Exchange (innych niż AG) | Y | Wkrótce | Y | Y
Zdalny Pulpitów | Y | Y | Y | N/D!
Linux (system operacyjny i aplikacje) | Y (sprawdzone przez firmę Microsoft) | Y (sprawdzone przez firmę Microsoft) | Y (sprawdzone przez firmę Microsoft) | Y (sprawdzone przez firmę Microsoft)
Dynamics AX | Y | Y | Y | Y
Dynamics CRM | Y | Wkrótce | Y | Wkrótce
Oracle | Y (sprawdzone przez firmę Microsoft) | Y (sprawdzone przez firmę Microsoft) | Y (sprawdzone przez firmę Microsoft) | Y (sprawdzone przez firmę Microsoft)
Serwer plików systemu Windows | Y | Y | Y | Y


## <a name="replicate-active-directory-and-dns"></a>Replikacja usługi Active Directory i systemie DNS

Usługa Active Directory i infrastruktury DNS są niezbędne do większości aplikacji przedsiębiorstwa. Podczas odzyskiwania systemu musisz ochrona i odzyskiwanie tych elementów infrastruktury przed odzyskiwanie do obciążenia i aplikacje.

Odzyskiwanie witryny służy do tworzenia planu odzyskiwania pełną automatyczną danych usługi Active Directory i systemie DNS. Jeśli chcesz się nie powieść, na przykład przez SharePoint i rozwiązania SAP z głównego do pomocniczej witryny, możesz skonfigurować planu odzyskiwania, nie działa w usłudze Active Directory, a następnie plan dodatkowe odzyskiwania specyficzne dla aplikacji niepowodzenie przez inne aplikacje, które korzystają z usługi Active Directory.

[Aby uzyskać więcej informacji](site-recovery-active-directory.md) na temat ochrony usługi Active Directory i DNS.

## <a name="protect-sql-server"></a>Ochrona programu SQL Server

Program SQL Server stanowi podstawę usług danych usług danych dla wielu aplikacji biznesowych w centrum danych lokalnych.  Odzyskiwanie witryny może służyć razem z technologii programu SQL Server HA/DR ochrony aplikacji wielopoziomowymi przedsiębiorstwa, które używają programu SQL Server. Odzyskiwanie witryny zawiera:

- Rozwiązanie odzyskiwania systemu proste i efektywne pod względem kosztów dla programu SQL Server. Replikować wiele wersji i wersje programu SQL Server autonomicznych serwerów i klastrów Azure lub pomocnicza witryny.  
- Integracja z grupy dostępności (AlwaysOn) SQL, zarządzanie awaryjnego i powrotu z planami odzyskiwania Odzyskiwanie witryny Azure.
- Odzyskiwanie kompleksowe — plany dla wszystkich poziomów w aplikacji, w tym z baz danych programu SQL Server.
- Skalowanie programu SQL Server dla Szczyt ładuje z Odzyskiwanie witryny przez "którym następuje" ich do większych rozmiarów maszyn wirtualnych IaaS w Azure.
- Łatwe testowanie awarii programu SQL Server. Praca awaryjna test do analizowania danych i uruchom testy zgodności, bez wpływania środowisku produkcyjnym, może zostać uruchomiony.

[Aby uzyskać więcej informacji](site-recovery-sql.md) na temat ochrony programu SQL server.

##<a name="protect-sharepoint"></a>Ochrona programu SharePoint

Odzyskiwanie witryny Azure chroni wdrożeń programu SharePoint, w następujący sposób:

- Eliminuje konieczności i koszty skojarzone infrastruktury farmy gotowości awarii. Odzyskiwanie witryny za pomocą replikacji całej farmy (warstwy sieci Web, aplikacji i bazy danych), aby Azure lub pomocniczej witryny.
- Upraszcza zarządzanie i wdrażanie aplikacji. Aktualizacje używany do podstawowej witryny są automatycznie replikowane, a więc są dostępne po awaryjnego i odzyskiwania farmy w witrynie pomocniczą. Również obniża złożoność zarządzania i koszty skojarzone z aktualizowanie farmy gotowości.
- Upraszcza projektowanie aplikacji programu SharePoint i badania, tworząc środowisku replice na żądanie z kopią produkcji podobne do testowania i debugowania.
- Upraszcza przejścia w chmurze za pomocą Odzyskiwanie witryny migrowania wdrożeń programu SharePoint do Azure.

[Aby uzyskać więcej informacji](https://gallery.technet.microsoft.com/SharePoint-DR-Solution-f6b4aeae) na temat ochrony programu SharePoint.


## <a name="protect-dynamics-ax"></a>Ochrona systemu Dynamics AX

Odzyskiwanie witryny Azure chroni rozwiązanie Dynamics AX ERP przez:

- Orchestrating replikacji całe Dynamics AX środowisko (sieci Web i AOS warstwy, poziomów bazy danych, program SharePoint) Azure lub do witryny pomocniczą.
- Upraszczanie migracji wdrożeniach systemu Dynamics AX w chmurze (Azure).
- Upraszczanie opracowywania aplikacji Dynamics AX i sprawdzanie przez utworzenie kopii przypominających produkcji na żądanie, testowanie i debugowanie.

[Aby uzyskać więcej informacji](https://gallery.technet.microsoft.com/Dynamics-AX-DR-Solution-b2a76281) o ochronie AX dynamiczne.

## <a name="protect-rds"></a>Ochrona licencji

Usługami pulpitu zdalnego (licencji) umożliwia virtual desktop infrastruktury Pulpitów wirtualnych, oparte na sesji pulpitów i aplikacji, dzięki czemu użytkownicy mogą Praca w dowolnym miejscu. Odzyskiwanie witryny Azure umożliwiają:

- Replikowane zarządzane lub niezarządzanej puli pulpitów wirtualnych pomocniczej witryny i zastosowania zdalnej sesji do witryny pomocniczą lub Azure.
- Oto, co można odtworzyć:

**LICENCJI** | **Powielić funkcji Hyper-V maszyny wirtualne do witryny pomocniczej** | **Powielić maszyny wirtualne funkcji Hyper-V Azure** | **Powielić VMware maszyny wirtualne do witryny pomocniczej** | **Powielić maszyny wirtualne VMware Azure** | **Replikacja serwerów fizycznych do witryny pomocniczej** | **Replikacja serwerów fizycznych Azure**
---|---|---|---|---|---|---
**Puli pulpit wirtualny (niezarządzanych)** | Tak | Brak | Tak | Brak | Tak | Brak
**Puli pulpit wirtualny (zarządzanych i bez UDP)** | Tak | Brak | Tak | Brak | Tak | Brak
**Zdalne programy, a następnie sesji pulpitu (bez UDP)** | Tak | Tak | Tak | Tak | Tak | Tak


[Aby uzyskać więcej informacji](https://gallery.technet.microsoft.com/Remote-Desktop-DR-Solution-bdf6ddcb) na temat ochrony RDS.


## <a name="protect-exchange"></a>Ochrona programu Exchange

Odzyskiwanie witryny chroni programu Exchange, w następujący sposób:

- W przypadku małych wdrożeń programu Exchange, takie jak pojedyncze lub autonomicznego serwerów Odzyskiwanie witryny można replikować i się nie powieść nad Azure lub do witryny pomocniczej.
- W przypadku dużych wdrożeń Odzyskiwanie witryny można zintegrować z DAGS programu Exchange.
- DAGs programu Exchange jest zalecane rozwiązanie programu Exchange awarii w przedsiębiorstwie.  Plan zagospodarowania terenu odzyskiwania odzyskiwania może zawierać DAGs, aby dodać akompaniament AG pracy awaryjnej w witrynach.


[Aby uzyskać więcej informacji](https://gallery.technet.microsoft.com/Exchange-DR-Solution-using-11a7dcb6) na temat ochrony programu Exchange.

## <a name="protect-sap"></a>Ochrona SAP

Odzyskiwanie witryny za pomocą ochrony rozmieszczenia SAP w następujący sposób:

- Włącz ochronę całego wdrożenia SAP przez replikacji warstwy rozmieszczania Azure lub do witryny pomocniczą.
- Uproszczenia migracji chmurze przy użyciu Odzyskiwanie witryny do migrowania wdrożenia SAP Azure.
- Rozwoju SAP w celu uproszczenia i badania, tworząc produkcji przypominających skopiuj na żądanie do testowania i debugowania aplikacji.

[Aby uzyskać więcej informacji](http://aka.ms/asr-sap) na temat ochrony SAP.

## <a name="next-steps"></a>Następne kroki

[Przygotowywanie do wdrożenia Odzyskiwanie witryny](site-recovery-best-practices.md) 
