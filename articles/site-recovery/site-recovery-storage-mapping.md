<properties
    pageTitle="Mapowanie miejsca do magazynowania w Odzyskiwanie witryny Azure dla funkcji Hyper-V maszyn wirtualnych replikacji między centrami danych lokalnych | Microsoft Azure"
    description="Przygotowywanie mapowania miejsca do magazynowania dla funkcji Hyper-V maszyn wirtualnych replikacji między dwoma lokalnego centrach danych z Odzyskiwanie witryny Azure."
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
    ms.date="07/06/2016"
    ms.author="raynew"/>


# <a name="prepare-storage-mapping-for-hyper-v-virtual-machine-replication-between-two-on-premises-datacenters-with-azure-site-recovery"></a>Przygotowywanie mapowania miejsca do magazynowania dla funkcji Hyper-V maszyn wirtualnych replikacji między dwoma lokalnego centrach danych z Odzyskiwanie witryny Azure


Azure Odzyskiwanie witryny składa się na strategii odzyskiwania (BCDR) ciągłości i danych biznesowych przez orchestrating replikacji, pracy awaryjnej i odzyskiwanie maszyn wirtualnych i serwerów fizycznych. Ten artykuł zawiera opis mapowania miejsca do magazynowania, co pomaga wprowadzeniu optymalne wykorzystanie miejsca do magazynowania podczas korzystania z witryn odzyskiwania replikacji maszyn wirtualnych funkcji Hyper-V między centrami danych VMM dwóch lokalnego.

Publikowanie jakieś komentarze lub pytania u dołu tego artykułu lub [Azure odzyskiwania usług Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)w witrynie.

## <a name="overview"></a>Omówienie

Mapowanie miejsca do magazynowania działa tylko po już replikacji maszyn wirtualnych funkcji Hyper-V, które znajdują się w chmury VMM z podstawowego centrum danych pomocniczej centrum danych za pomocą funkcji Hyper-V replice lub SAN replikacji, w następujący sposób:


- **Lokalnego do lokalnego replikacji z replice funkcji Hyper-V)** — Konfigurowanie miejsca do magazynowania mapowania przez mapowanie klasyfikacje miejsca do magazynowania na źródle i docelowych VMM serwerów, wykonaj następujące czynności:

    - **Identyfikowanie docelowego miejsca do magazynowania dla maszyn wirtualnych replice**— maszyn wirtualnych będzie można replikować do elementu docelowego miejsca do magazynowania (SMB Udostępnij lub klaster udostępnionej ilości (CSVs)) wybranego przez użytkownika.
    - **Położenie maszyn wirtualnych replice**— mapowania miejsca do magazynowania jest używany do optymalnie umieść maszyn wirtualnych replice na serwerach hosta funkcji Hyper-V. Maszyn wirtualnych replice zostaną umieszczone na hostów, które można uzyskać dostęp do klasyfikacji zamapowanych miejsca do magazynowania.
    - **Brak miejsca do magazynowania mapowania**— Jeśli nie skonfigurowano mapowania miejsca do magazynowania, maszyn wirtualnych będzie można replikować do domyślnej lokalizacji przechowywania określony na serwerze hosta funkcji Hyper-V skojarzone z maszyny wirtualnej replice.

- **Lokalne do lokalnego replikacji z SAN**— Konfigurowanie miejsca do magazynowania mapowania przez mapowanie pul tablice miejsca do magazynowania na źródle i docelowych VMM serwerów.
    - **Określanie puli**— Określa, które puli miejsca do magazynowania pomocniczej pobierać dane replikacji z podstawowej puli.
    - **Identyfikowanie docelowego miejsca do magazynowania pul**— gwarantuje, że jednostek logicznych w grupie replikacji źródła są replikowane puli miejsca do magazynowania zamapowanych docelowej wybranych przez użytkownika.

## <a name="set-up-storage-classifications-for-hyper-v-replication"></a>Konfigurowanie klasyfikacji miejsca do magazynowania dla funkcji Hyper-V replikacji

Podczas korzystania z funkcji Hyper-V replice do replikacji z Odzyskiwanie witryny, można mapować między klasyfikacje miejsca do magazynowania na serwerach VMM źródłowej i docelowej lub na serwerze VMM Jeśli dwie witryny są zarządzane przez ten sam serwer VMM. Należy zauważyć, że:

- Podczas mapowania jest poprawnie skonfigurowany i replikacja jest włączona, wirtualnego dysku twardego maszyny wirtualnej w lokalizacji podstawowej będzie można replikować do miejsca do magazynowania w lokalizacji docelowej zamapowane.
- Klasyfikacje miejsca do magazynowania musi być dostępny grupom hosta znajduje się w źródłowej i docelowej chmury.
- Klasyfikacje nie muszą być tego samego typu miejsca do magazynowania. Na przykład można mapować Klasyfikacja źródła, która zawiera udziały SMB w celu klasyfikacji docelowej, która zawiera CSVs.
- Przeczytaj więcej w temacie [jak utworzyć klasyfikacje miejsca do magazynowania w VMM](https://technet.microsoft.com/library/gg610685.aspx).

## <a name="example"></a>Przykład

Jeśli klasyfikacje są poprawnie skonfigurowane w VMM po wybraniu źródłowej i docelowej serwera VMM podczas mapowania miejsca do magazynowania, będą wyświetlane klasyfikacje źródłowej i docelowej. Oto przykład udziałach plików miejsca do magazynowania i klasyfikacje dla organizacji z dwóch miejscach w Nowy Jork i Chicago.

**Lokalizacja** | **Serwer VMM** | **Udostępnianie pliku (źródło)** | **Klasyfikacja (źródło)** | **Zamapowane** | **Udostępnianie pliku (docelowa)**
---|---|--- |---|---|---
Nowy Jork | VMM_Source| SourceShare1 | ZŁOTY | GOLD_TARGET | TargetShare1
 |  | SourceShare2 | SILVER | SILVER_TARGET | TargetShare2
 | | SourceShare3 | BRĄZOWY | BRONZE_TARGET | TargetShare3
Chicago | VMM_Target |  | GOLD_TARGET | Niezamapowane |
| | | SILVER_TARGET | Niezamapowane |
 | | | BRONZE_TARGET | Niezamapowane

Czy można skonfigurować na karcie **Magazynowanie Server** na stronie **zasoby** Odzyskiwanie witryny portalu.

![Skonfiguruj mapowanie miejsca do magazynowania](./media/site-recovery-storage-mapping/storage-mapping1.png)

Z tym przykładzie:
- Po utworzeniu maszyny wirtualnej replice dla maszyn wirtualnych ZŁOTY ilość miejsca do magazynowania (SourceShare1), będzie można replikować do magazynu GOLD_TARGET (TargetShare1).
- Po utworzeniu maszyny wirtualnej replice dla maszyn wirtualnych SREBRNY ilość miejsca do magazynowania (SourceShare2) będzie można replikować do magazynu SILVER_TARGET (TargetShare2) i tak dalej.

Udziały plików rzeczywistych i ich przypisane klasyfikacje w VMM są wyświetlane na następnym ekranie zrzut.

![Klasyfikacje miejsca do magazynowania w VMM](./media/site-recovery-storage-mapping/storage-mapping2.png)

## <a name="multiple-storage-locations"></a>Wiele lokalizacji przechowywania

Klasyfikacja docelowej przydzielenia wielu akcji SMB lub CSVs lokalizacji przechowywania optymalnego zostanie automatycznie zaznaczone, gdy maszyny wirtualnej jest chroniony. Jeśli odpowiednie docelowej bez miejsca do magazynowania jest dostępna z określonej klasyfikacji, domyślnej lokalizacji przechowywania określony na hoście funkcji Hyper-V jest używany do umieść replice wirtualnych dyskach twardych.

Poniższej tabeli pokazano, jak klasyfikacji miejsca do magazynowania i ilości klaster udostępnione są skonfigurowane w naszym przykładzie.

**Lokalizacja** | **Klasyfikacja** | **Skojarzony miejsca do magazynowania**
---|---|---
Nowy Jork | ZŁOTY | <p>C:\ClusterStorage\SourceVolume1</p><p>\\FileServer\SourceShare1</p>
 | SILVER | <p>C:\ClusterStorage\SourceVolume2</p><p>\\FileServer\SourceShare2</p>
Chicago | GOLD_TARGET | <p>C:\ClusterStorage\TargetVolume1</p><p>\\FileServer\TargetShare1</p>
 | SILVER_TARGET| <p>C:\ClusterStorage\TargetVolume2</p><p>\\FileServer\TargetShare2</p>

W poniższej tabeli podsumowano zachowanie po włączeniu ochrony dla maszyn wirtualnych (VM1 - VM5) w tym przykładzie środowisku.

**Maszyn wirtualnych** | **Źródło miejsca do magazynowania** | **Źródło klasyfikacji** | **Zamapowane docelowego miejsca do magazynowania**
---|---|---|---
VM1 | C:\ClusterStorage\SourceVolume1 | ZŁOTY | <p>C:\ClusterStorage\SourceVolume1</p><p>\\\FileServer\SourceShare1</p><p>Obie GOLD_TARGET</p>
VM2 | \\FileServer\SourceShare1 | ZŁOTY | <p>C:\ClusterStorage\SourceVolume1</p><p>\\FileServer\SourceShare1</p> <p>Obie GOLD_TARGET</p>
VM3 | C:\ClusterStorage\SourceVolume2 | SILVER | <p>C:\ClusterStorage\SourceVolume2</p><p>\FileServer\SourceShare2</p>
VM4 | \FileServer\SourceShare2 | SILVER |<p>C:\ClusterStorage\SourceVolume2</p><p>\\FileServer\SourceShare2</p><p>Obie SILVER_TARGET</p>
VM5 | C:\ClusterStorage\SourceVolume3 | N/D! | Mapowania, dlatego używane są domyślnej lokalizacji przechowywania hosta funkcji Hyper-V

## <a name="next-steps"></a>Następne kroki

Teraz, gdy masz lepszego zrozumienia mapowanie miejsca do magazynowania, [Przygotowywanie do wdrożenia Odzyskiwanie witryny Azure](site-recovery-best-practices.md).
