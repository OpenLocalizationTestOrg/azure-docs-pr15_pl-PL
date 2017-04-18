<properties
    pageTitle="Wprowadzenie do Azure DPM kopia zapasowa | Microsoft Azure"
    description="Wprowadzenie do wykonywania kopii zapasowych serwerów DPM przy użyciu usługi Azure kopii zapasowej"
    services="backup"
    documentationCenter=""
    authors="Nkolli1"
    manager="shreeshd"
    editor=""
    keywords="System Center Data Protection Manager, Menedżer ochrony danych, wykonywanie kopii zapasowych dpm"/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/21/2016"
    ms.author="trinadhk;giridham;jimpark;markgal"/>

# <a name="preparing-to-back-up-workloads-to-azure-with-dpm"></a>Przygotowanie do tworzenia kopii zapasowych obciążenia Azure z DPM

> [AZURE.SELECTOR]
- [Serwer Azure kopii zapasowej](backup-azure-microsoft-azure-backup.md)
- [SCDPM](backup-azure-dpm-introduction.md)
- [Azure kopii zapasowych serwera (klasyczny)](backup-azure-microsoft-azure-backup-classic.md)
- [SCDPM (klasyczny)](backup-azure-dpm-introduction-classic.md)


Ten artykuł zawiera wprowadzenie do korzystania z programu Kopia zapasowa Microsoft Azure ochrony serwerów systemu centrum danych Protection Manager (DPM) i obciążenia. Czytając go będzie opis:

- Jak działa kopii zapasowych serwera Azure DPM
- Wymagania wstępne dotyczące osiągnięcia wygładzonymi obsługi kopii zapasowej
- Typowe błędy napotkał i jak postępować z nimi
- Obsługiwane scenariusze

System Center DPM kopię zapasową danych plików i aplikacji. Dane kopie zapasowe DPM można przechowywane na taśmą na dysku, lub kopii zapasowej Azure z kopia zapasowa Microsoft Azure. DPM interakcji z kopii zapasowej Azure w następujący sposób:

- **DPM rozmieszczony jako fizycznie serwera lokalnego wirtualnych komputerze lub** — Jeśli DPM jest używany jako fizyczny serwer lub maszyn wirtualnych funkcji Hyper-V dla lokalnego może wykonywać kopie zapasowe danych do kopii zapasowej Azure magazynu oprócz dysku i taśmą kopii zapasowej.
- **DPM rozmieszczony jako Azure maszyn wirtualnych** — z systemu Centrum 2012 R2 z aktualizacji 3, DPM mogą być rozmieszczone jako Azure maszyn wirtualnych. Jeśli DPM jest używany jako Azure maszyn wirtualnych, które może wykonywać kopie zapasowe danych na dyskach Azure dołączone maszyn wirtualnych DPM Azure lub magazynowanie danych można offload przez jego kopię do magazynu kopii zapasowej Azure.

## <a name="why-backup-your-dpm-servers"></a>Dlaczego warto utworzyć kopię zapasową serwery DPM?

Firm przy użyciu kopii zapasowej Azure do tworzenia kopii zapasowych serwerów DPM zalety:

- Dla lokalnego wdrożenia programu DPM Azure kopia zapasowa służy jako alternatywa dla długotrwałych rozmieszczania taśmą.
- W przypadku wdrożeń DPM platformy Azure kopii zapasowej Azure umożliwia offload miejsca do magazynowania z dysku Azure umożliwia rozbudowy starsze dane są przechowywane w kopii zapasowej Azure i nowe dane na dysku.

## <a name="how-does-dpm-server-backup-work"></a>Jak działa narzędzia Kopia zapasowa DPM server?
Aby utworzyć kopię zapasową maszyny wirtualnej, najpierw migawkę w chwili danych jest potrzebny. Usługa Azure kopii zapasowej inicjuje zadania wykonywania kopii zapasowej zgodnie z harmonogramem i wyzwalane kopii zapasowej rozszerzenia zrób migawkę. Rozszerzenie kopii zapasowej współdziała z usługą VSS w gościa w celu osiągnięcia spójności i wywołuje interfejs API migawkę obiektów blob usługi Magazyn Azure po przekroczeniu spójności. Można to zrobić do uzyskania spójnego migawki dysków maszyny wirtualnej, bez konieczności zamykania go.

Po podjęciu migawki, dane są przenoszone przez usługę Azure kopii zapasowej do magazynu kopii zapasowej. Usługa trwa istotnych identyfikowania i przenoszenia bloki, które zmieniły się z ostatniej kopii zapasowej wprowadzania wydajną obsługę sieci i magazynowania kopii zapasowych. Po zakończeniu transferu danych migawki jest usuwana i punkt odzyskiwania jest tworzony. Ten punkt odzyskiwania są widoczne w portalu klasyczny Azure.

>[AZURE.NOTE] Dla maszyn wirtualnych systemu Linux jest możliwe tylko plik spójną z kopii zapasowej.

## <a name="prerequisites"></a>Wymagania wstępne
Przygotowywanie kopii zapasowej Azure kopie zapasowe danych DPM w następujący sposób:

1. **Tworzenie magazynu kopii zapasowych** — Tworzenie magazynu w konsoli Azure kopii zapasowej.
2. **Plik do pobrania magazynu poświadczeń** — w kopii zapasowej Azure, przekazywanie certyfikatu zarządzania utworzono do magazyn.
3. **Instalowanie agenta kopii zapasowej Azure i zarejestrować serwer** — z Azure kopii zapasowej, zainstalować agenta na wszystkich serwerach DPM i zarejestrować serwer DPM w kopii zapasowej magazynu.

[AZURE.INCLUDE [backup-create-vault](../../includes/backup-create-vault.md)]

[AZURE.INCLUDE [backup-download-credentials](../../includes/backup-download-credentials.md)]

[AZURE.INCLUDE [backup-install-agent](../../includes/backup-install-agent.md)]


## <a name="requirements-and-limitations"></a>Wymagania dotyczące (i ograniczenia)

- DPM może działać jako fizyczny serwer lub zainstalowany w systemie Centrum 2012 z dodatkiem SP1 lub systemu Centrum 2012 R2 maszyny wirtualnej funkcji Hyper-V. Można również działać jako Azure maszyn wirtualnych uruchomionych System Centrum 2012 R2 z co najmniej DPM 2012 R2 aktualizacji nr 3 lub Windows maszyny wirtualnej VMWare uruchomionych System Centrum 2012 R2 z co najmniej pakietu zbiorczego aktualizacji 5.
- Jeśli używasz DPM System Centrum 2012 z dodatkiem SP1, należy zainstalować pakietem zbiorczym aktualizacji 2 dla System Center Data Protection Manager z dodatkiem SP1. Jest to wymagane, aby można było zainstalować agenta kopii zapasowej Azure.
- Serwer DPM powinien mieć programu Windows PowerShell i .net Framework 4,5 zainstalowany.
- DPM można wykonać kopię zapasową większość obciążenia Azure kopii zapasowej. Co to jest obsługiwane Zobacz pełną listę kopii zapasowej Azure obsługuje poniższych elementów.
- Nie można odzyskać danych przechowywanych w kopii zapasowej Azure z opcją "Kopiuj do taśmą".
- Musisz mieć konto Azure za pomocą funkcji Kopia zapasowa Azure włączone. Jeśli nie masz konta, możesz utworzyć bezpłatne konto wersji próbnej na kilka minut. Przeczytaj o [ceny Azure kopii zapasowej](https://azure.microsoft.com/pricing/details/backup/).
- Korzystanie z kopii zapasowych Azure wymaga agenta kopii zapasowej Azure musi być zainstalowany na serwerach, który chcesz utworzyć kopię zapasową. Każdy serwer musi mieć co najmniej 10% rozmiaru danych, który jest teraz kopię zapasową, dostępne jako lokalnej wolnego miejsca. Na przykład wykonywanie kopii zapasowej 100 GB danych wymaga co najmniej 10 GB wolnego miejsca w lokalizacji wymazywanie. Gdy minimalna wynosi 10%, zaleca się 15% wolnego miejsca w lokalnym miejsca ma być używany dla lokalizacji pamięci podręcznej.
- Dane są przechowywane w magazynie Azure magazynu. Nie jest ograniczona do ilości danych, które możesz można wykonać kopię zapasową kopii zapasowej Azure vault, ale rozmiar źródła danych (na przykład maszyn wirtualnych lub bazy danych) nie przekracza 54,400 GB.

Typy plików są obsługiwane wykonaj kopię zapasową Azure:

- Zaszyfrowane (pełne kopie zapasowe tylko)
- Skompresowany (przyrostowe kopie zapasowe obsługiwane)
- Rzadkie (przyrostowe kopie zapasowe obsługiwane)
- Skompresowany i rzadkie (traktowane jako Sparse)

I te nie są obsługiwane:

- Serwery w systemie plików uwzględniania wielkości liter nie są obsługiwane.
- Łącza stałe (pominięty)
- Punkty (pominięty) ponownej analizy
- Zaszyfrowanych i skompresowany (pominięty)
- Zaszyfrowane i rzadkie (pominięte)
- Skompresowany strumień
- Rzadkie strumienia

>[AZURE.NOTE] Z w systemie Centrum 2012 DPM z dodatkiem SP1 lub nowszym, można utworzyć kopię zapasową w górę obciążenia chroniony przez DPM Azure za pomocą programu Microsoft Azure kopii zapasowej.
