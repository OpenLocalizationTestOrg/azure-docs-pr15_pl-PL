<properties
   pageTitle="Obsługa wskazówek elastyczność | Microsoft Azure"
   description="Linki do awarii i aktywne wskazówki dotyczące usług Microsoft Azure elastyczność i dostępności."
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

# <a name="microsoft-azure-service-resiliency-guidance"></a>Wskazówki dotyczące ochrony usługi Microsoft Azure
Microsoft Azure służy do zapewnienia zasoby, których potrzebujesz, gdy są potrzebne. W ramach dobry projekt i praktyki działania należy wiedzieć, zarówno jak zaprojektować korzystania z usług Azure uzyskanie wysokiej dostępności, a także co należy zrobić, jeśli aplikacja jest wpływ awarii usługi. Do tego procesu, ten dokument zawiera łącza do wskazówek odzyskiwanie danych, a także wskazówki dotyczące projektowania dla różnych usług Azure.

##<a name="disaster-recovery-guidance"></a>Wskazówki dotyczące odzyskiwania po awarii
Wskazówki odzyskiwania danych, które są poniższe łącza można udostępnić informacje potrzebne do ułatwiają szybkie uzyskiwanie aplikacji trybu online, jeśli użytkownik ma wpływ awarii usługi Azure. Te łącza zostały utworzone pomocnych w odpowiedzi na pytanie, "I jestem jest wpływ awarii usługi Azure, co można zrobić?"

##<a name="design-guidance"></a>Wskazówki dotyczące projektowania
Poniższe łącza wskazówki dotyczące projektu to projektu i architektury wskazówek, który został utworzony, aby ułatwić zrozumienie jak najlepiej używać każdej usługi Azure w taki sposób, aby maksymalizuje czas pracy w aplikacji. Te łącza zostały utworzone pomocnych w odpowiedzi na pytanie "Jak I upewnij się, że błąd, błąd sprzętowy, awarii usługi lub inna awaria nie będą mieć wpływ na dostępności aplikacji pakietu?" W przypadku żadnej dokładnej wskazówki przez usługę, którą obecnie szukasz artykuł [wysokiej dostępności aplikacji utworzonych na platformy Microsoft Azure](./resiliency-high-availability-azure-applications.md) może być dodatkowe informacje, które mogą pomóc w projekcie. 

##<a name="service-guidance"></a>Wskazówki dotyczące usługi
| Usługa  | Wskazówki dotyczące odzyskiwania po awarii | Wskazówki dotyczące projektowania |
|:---------|:--------------------------:|:------------------:|
| [Usług w chmurze] (https://azure.microsoft.com/services/cloud-services/ "Azure usług w chmurze")       | [łącze] (../cloud-services/cloud-services-disaster-recovery-guidance.md "Wskazówki dotyczące odzyskiwania danych usług w chmurze Azure")   | Nie jest dostępna |
| [Kluczowe magazynu] (https://azure.microsoft.com/services/key-vault/ "Azure klucza magazynu")                      | [łącze] (../key-vault/key-vault-disaster-recovery-guidance.md "Wskazówki odzyskiwania systemu Azure klucza magazynu")        | Nie jest dostępna |
| [Miejsca do magazynowania] (https://azure.microsoft.com/services/storage/ "Azure miejsca do magazynowania")                            | [łącze] (../storage/storage-disaster-recovery-guidance.md "Wskazówki dotyczące odzyskiwania po awarii magazyn Azure")          | Nie jest dostępna |
| [Bazy danych SQL] (https://azure.microsoft.com/services/sql-database/ "Bazy danych programu SQL Azure")           | [łącze] (../sql-database/sql-database-disaster-recovery.md  "Wskazówki dotyczące odzyskiwania danych bazy danych SQL Azure")    | [łącze] (../sql-database/sql-database-business-continuity.md "Omówienie ciągłości z bazy danych SQL Azure") |
| [Maszyn wirtualnych] (https://azure.microsoft.com/services/virtual-machines/ "Azure maszyn wirtualnych") | [łącze] (../virtual-machines/virtual-machines-disaster-recovery-guidance.md "Wskazówki dotyczące odzyskiwania systemu Azure maszyn wirtualnych") | Nie jest dostępna |
| [Wirtualna sieć] (https://azure.microsoft.com/services/virtual-network/ "Azure wirtualnej sieci")    | [łącze] (../virtual-network/virtual-network-disaster-recovery-guidance.md "Wskazówki dotyczące odzyskiwania systemu Azure wirtualnej sieci")  | Nie jest dostępna |

##<a name="next-steps"></a>Następne kroki
Jeśli szukasz wskazówek, który omówiono szerokiego systemy i rozwiązania przeczytaj [Odzyskiwanie i wysoką dostępność dla aplikacji utworzonych na platformy Microsoft Azure](https://aka.ms/drtechguide).
