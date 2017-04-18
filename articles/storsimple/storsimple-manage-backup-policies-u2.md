<properties 
   pageTitle="Zarządzanie zasadami kopii zapasowej usługi StorSimple | Microsoft Azure"
   description="Wyjaśniono, jak korzystać z usługę Menedżer StorSimple tworzyć i zarządzać nimi ręcznego tworzenia kopii zapasowych, kopii zapasowej harmonogramów i przechowywania kopii zapasowej."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor=""/>
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="05/10/2016"
   ms.author="v-sharos"/>

# <a name="use-the-storsimple-manager-service-to-manage-backup-policies-update-2"></a>Zarządzanie zasadami kopii zapasowej (Aktualizuj 2) za pomocą usługi Menedżera StorSimple

[AZURE.INCLUDE [storsimple-version-selector-manage-backup-policies](../../includes/storsimple-version-selector-manage-backup-policies.md)]

## <a name="overview"></a>Omówienie

Ten samouczek wyjaśniono, jak za pomocą strony **Kopii zapasowej zasad** usługi Menedżera StorSimple kontrolować procesów wykonywania kopii zapasowych i przechowywania kopii zapasowej dla swojego wielkości StorSimple. Zawiera również opis jak wykonać ręczną kopię zapasową.

Podczas wykonywania kopii zapasowej woluminu, możesz utworzyć migawkę lokalne lub migawki chmury. Jeśli wykonujesz lokalnie przypięty kreski, zalecamy określone migawkę chmury. Sporządzanie dużej liczby lokalne migawek wielkości lokalnie przypięty związane z zestawu danych, która zawiera wiele pochodząca spowoduje w sytuacji, w której można szybko uruchomić brakować miejsca lokalny. Jeśli wybierzesz migawek lokalnych, zalecamy migawek mniej dzienny kopia zapasowa ostatnio stanu, przechowywania ich na dzień, a następnie usuń je.

Zrób migawkę chmury lokalnie przypięty głośność, możesz skopiować tylko zmienionych danych w chmurze, gdy jest deduplicated i skompresowany. 

## <a name="the-backup-policies-page"></a>Na stronie zasady kopii zapasowej

Na stronie **Zasady kopii zapasowej** umożliwia zarządzanie zasadami kopii zapasowej i planowanie lokalnej i w chmurze migawek. (Kopii zapasowej zasady są używane do konfigurowania kopii zapasowej harmonogramów i przechowywania kopii zapasowej zbioru wielkości). Zasady kopii zapasowej umożliwiają jednoczesne zrób migawkę przypisaną. Oznacza to, że kopie zapasowe utworzone przez zasady tworzenia kopii zapasowych będzie spójne awarie kopii. Na stronie **Kopii zapasowej zasady** lista kopii zapasowej zasad, typy, wielkość skojarzone, liczba kopii zapasowych zachowane i opcję, aby włączyć te zasady.

Na stronie **Zasady kopii zapasowej** umożliwia filtrowanie istniejące zasady kopii zapasowej, przez co najmniej jedną z następujących pól:

- **Nazwa zasady** — nazwę skojarzonego z zasad. Różne typy zasad:

   - Zasady według harmonogramu, które wyraźnie są tworzone przez użytkownika.
   - Automatyczne zasady, które są tworzone kopii zapasowych domyślnej dla tej opcji głośności został włączony w czasie tworzenia głośność. Te zasady są nazywane jako *Nazwa woluminu*_Default miejsce, w którym *Nazwa woluminu* odwołuje się do nazwy woluminu StorSimple skonfigurowany przez użytkownika w portalu klasyczny Azure. Automatyczne zasady powodują dzienne migawki chmury, zaczynając od 22:30 czasu urządzenia.
   - Zaimportowane zasady, które zostały utworzone w Menedżerze migawkę StorSimple. Mają one znacznik, który zawiera opis hoście StorSimple migawkę Menedżera zasad zaimportowanych z.

- **Ilości** — wielkość skojarzone z zasad. Wszystkie wielkość skojarzone z kopii zapasowej zasady są pogrupowane razem podczas tworzenia kopii zapasowych.

- **Ostatnia kopia zapasowa pomyślnego** — Data i godzina ostatniego pomyślnego kopii zapasowej, który został zrobione za pomocą tych zasad.

- **Następna kopia zapasowa** — Data i godzina następnego zaplanowanego kopii zapasowej, który będzie zainicjowane przez tych zasad.

- **Harmonogramy** — liczba harmonogramów skojarzone z kopii zapasowej zasad.

Często używane operacje, które można wykonywać na tej stronie są:

- Dodawanie zasady tworzenia kopii zapasowych 
- Dodawanie lub modyfikowanie harmonogramu 
- Usuwanie zasady tworzenia kopii zapasowych 
- Wykonać ręczną kopię zapasową 
- Tworzenie niestandardowego zasady tworzenia kopii zapasowych przy użyciu wielu wielkości i harmonogramów 

## <a name="add-a-backup-policy"></a>Dodawanie zasady tworzenia kopii zapasowych

Dodaj zasady tworzenia kopii zapasowych planować automatyczne kopie zapasowe. W portalu klasyczny Azure, aby dodać zasady tworzenia kopii zapasowych dla swojego urządzenia StorSimple, należy wykonać następujące czynności. Po dodaniu zasady, można zdefiniować serii rozłożonych w czasie (zobacz [Dodaj lub zmodyfikować harmonogram](#add-or-modify-a-schedule)).

[AZURE.INCLUDE [storsimple-add-backup-policy-u2](../../includes/storsimple-add-backup-policy-u2.md)]

![Klip wideo dostępne](./media/storsimple-manage-backup-policies-u2/Video_icon.png) **wideo dostępne**

Aby obejrzeć klip wideo pokazano, jak utworzyć lokalnie lub w chmurze zasad kopii zapasowej, kliknij [tutaj](https://azure.microsoft.com/documentation/videos/create-storsimple-backup-policies/).


## <a name="add-or-modify-a-schedule"></a>Dodawanie lub modyfikowanie harmonogramu

Możesz dodać lub zmodyfikować harmonogram dołączona do istniejącej zasady kopii zapasowej na urządzeniu StorSimple. W portalu klasyczny Azure, aby dodać lub zmodyfikować harmonogram, należy wykonać następujące czynności.

[AZURE.INCLUDE [storsimple-add-modify-backup-schedule](../../includes/storsimple-add-modify-backup-schedule-u2.md)]

## <a name="delete-a-backup-policy"></a>Usuwanie zasady tworzenia kopii zapasowych

W portalu klasyczny Azure, aby usunąć zasady tworzenia kopii zapasowych na urządzeniu StorSimple, należy wykonać następujące czynności.

[AZURE.INCLUDE [storsimple-delete-backup-policy](../../includes/storsimple-delete-backup-policy.md)]


## <a name="take-a-manual-backup"></a>Wykonać ręczną kopię zapasową

W portalu klasyczny Azure w celu utworzenia kopii zapasowej (ręczne) na żądanie dla jednego woluminu, należy wykonać następujące czynności.

[AZURE.INCLUDE [storsimple-create-manual-backup](../../includes/storsimple-create-manual-backup.md)]

## <a name="create-a-custom-backup-policy-with-multiple-volumes-and-schedules"></a>Tworzenie niestandardowego zasady tworzenia kopii zapasowych przy użyciu wielu wielkości i harmonogramów

W portalu klasyczny Azure, aby utworzyć niestandardowe zasady tworzenia kopii zapasowych wielu wielkości i harmonogramów, należy wykonać następujące czynności.

[AZURE.INCLUDE [storsimple-create-custom-backup-policy](../../includes/storsimple-create-custom-backup-policy-u2.md)]


## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej o [korzystaniu z usługi Menedżera StorSimple administrowania urządzenia StorSimple](storsimple-manager-service-administration.md).
