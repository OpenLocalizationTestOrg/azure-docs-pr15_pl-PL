<properties 
   pageTitle="Administrowanie usługami Menedżera StorSimple | Microsoft Azure"
   description="Dowiedz się, jak zarządzać urządzenia StorSimple za pomocą usługi Menedżera StorSimple w portalu klasyczny Azure."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/21/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-administer-your-storsimple-device"></a>Administrowanie urządzenia StorSimple za pomocą usługi Menedżera StorSimple

## <a name="overview"></a>Omówienie

W tym artykule opisano interfejs usługi Menedżera StorSimple, w tym jak połączyć, różne opcje dostępne, oraz łączy się do konkretnych przepływów pracy, które mogą być wykonywane za pośrednictwem tego interfejsu użytkownika. Te wskazówki ma zastosowanie do obu; fizycznej StorSimple i urządzenia wirtualnego.

Po zapoznaniu się w tym artykule przedstawiono sposoby:

- Nawiązywanie połączenia z usługą Menedżera StorSimple
- Nawigowanie interfejsu użytkownika Menedżer StorSimple
- Administrowanie urządzenia StorSimple za pośrednictwem usługi Menedżera StorSimple


## <a name="connect-to-storsimple-manager-service"></a>Nawiązywanie połączenia z usługą Menedżera StorSimple

Usługa Menedżera StorSimple działa w programie Microsoft Azure i łączy do wielu urządzeń StorSimple. Zarządzanie tych urządzeń za pomocą centralnej portalu klasyczny Microsoft Azure działa w przeglądarce. Aby połączyć się z usługą Menedżera StorSimple, wykonaj następujące czynności.

#### <a name="to-connect-to-the-service"></a>Aby połączyć się z usługą

1. Przejdź do [https://manage.windowsazure.com/](https://manage.windowsazure.com/).

1. Przy użyciu poświadczeń konta Microsoft, zaloguj się do portalu Microsoft Azure klasyczny (znajdujące się w górnym rogu okienka).

1. Przewiń w dół w okienku nawigacji po lewej stronie, aby uzyskać dostęp do usług Menedżera StorSimple.


## <a name="navigate-storsimple-manager-service-ui"></a>Przejdź usługi Menedżera StorSimple interfejsu użytkownika

W poniższej tabeli przedstawiono hierarchii nawigacji w usłudze Menedżer StorSimple interfejsu użytkownika.

- Strona początkowa **Menedżera StorSimple** umożliwia przejście do strony poziom obsługi interfejsu użytkownika, które dotyczą wszystkich urządzeń w ramach usługi.

- Strona **urządzenia** umożliwia przejście do strony interfejsu użytkownika — na poziomie urządzenia, które dotyczą konkretnego urządzenia.

- Strony **Kontenerów głośności** umożliwia przejście do strony głośność, która zawiera wszystkie wielkość skojarzone z urządzeniem.


#### <a name="storsimple-manager-service-navigational-hierarchy"></a>Hierarchia nawigacji usługi Menedżera StorSimple

|Strona główna|Poziom obsługi stron|Na poziomie urządzenia stron|Na poziomie urządzenia stron|
|---|---|---|---|
|Usługa Menedżera StorSimple|Pulpit nawigacyjny usługi|Pulpit nawigacyjny urządzenia||
||Urządzenia →|Monitorowanie|
||Wykaz kopii zapasowych|Containers→ głośności|Ilości|
||Konfigurowanie (usługa)|Zasady kopii zapasowej||
||Zadania|Konfigurowanie (urządzenia)|
||Alerty|Konserwacja|

![Klip wideo dostępne](./media/storsimple-manager-service-administration/Video_icon.png) **wideo dostępne**

Aby obejrzeć klip wideo, który przeprowadzi Cię przez interfejs użytkownika usługi Menedżera StorSimple, kliknij [tutaj](https://azure.microsoft.com/documentation/videos/storsimple-manager-service-overview/).

## <a name="administer-storsimple-device-using-storsimple-manager-service"></a>Administrowanie urządzenia StorSimple przy użyciu usługi Menedżera StorSimple

W poniższej tabeli przedstawiono podsumowanie wszystkich zadań administracyjnych i złożone przepływy pracy, które mogą być wykonywane w ramach usługi Menedżera StorSimple interfejsu użytkownika. Te zadania są zorganizowane oparte na stronach interfejsu użytkownika, na których są inicjowane.

Aby uzyskać więcej informacji na temat każdego przepływu pracy kliknij odpowiednią procedurę w tabeli.

#### <a name="storsimple-manager-workflows"></a>Menedżer StorSimple przepływów pracy

|Jeśli chcesz to zrobić...|Przejdź do tej strony interfejsu użytkownika...|Wykonaj poniższą procedurę.|
|---|---|---|
|Tworzenie usługi</br>Usuwanie usługi</br>Uzyskiwanie klucza rejestru usługi</br>Ponownie wygenerować klucza rejestru usługi|Usługa Menedżera StorSimple|[Wdrażanie usługi Menedżera StorSimple](storsimple-manage-service.md)
|Zmienianie klucza szyfrowania danych usługi</br>Wyświetlanie dzienników operacji|Pulpit nawigacyjny StorSimple → usługi Menedżera|[Za pomocą pulpitu nawigacyjnego usług Menedżera StorSimple](storsimple-service-dashboard.md)|
|Dezaktywowanie urządzenia</br>Usuwanie urządzenia|Menedżer StorSimple usługi → urządzeń|[Dezaktywowanie i usuwanie urządzenia](storsimple-deactivate-and-delete-device.md)|
|Dowiedz się więcej o pracy awaryjnej odzyskiwania i urządzenia po awarii</br>Przełączanie awaryjne do urządzenia fizycznego</br>Przełączanie awaryjne do urządzenia wirtualne</br>Business ciągłości awarii (BCDR)|Menedżer StorSimple usługi → urządzeń|[Odzyskiwanie awaryjnego i danych dla swojego urządzenia StorSimple](storsimple-device-failover-disaster-recovery.md)|
|Lista kopie zapasowe dla woluminu</br>Wybierz zestaw kopii zapasowych</br>Usuwanie zestawu kopii zapasowych|Wykaz narzędzia Kopia zapasowa StorSimple → usługi Menedżera|[Zarządzanie wykonywania kopii zapasowych](storsimple-manage-backup-catalog.md)|
|Klonowanie woluminu|Wykaz narzędzia Kopia zapasowa StorSimple → usługi Menedżera|[Klonowanie woluminu](storsimple-clone-volume.md)|
|Przywracanie zestawu kopii zapasowych|Wykaz narzędzia Kopia zapasowa StorSimple → usługi Menedżera|[Przywracanie zestawu kopii zapasowych](storsimple-restore-from-backup-set.md)|
|Informacje o kontach miejsca do magazynowania</br>Dodawanie konta miejsca do magazynowania</br>Edytowanie konta miejsca do magazynowania</br>Usuwanie konta miejsca do magazynowania</br>Kluczowe obrotu kont miejsca do magazynowania|Konfigurowanie → usługi Menedżera StorSimple|[Zarządzanie kontami miejsca do magazynowania](storsimple-manage-storage-accounts.md)|
|Informacje na temat szablonów przepustowości</br>Dodawanie szablonu przepustowości</br>Edytowanie szablonu przepustowości</br>Usuwanie szablonu przepustowości</br>Użyj szablonu domyślnego przepustowości</br>Tworzenie szablonu przepustowości całodzienne rozpoczyna się w określonym czasie|Konfigurowanie → usługi Menedżera StorSimple|[Zarządzanie szablonami przepustowości](storsimple-manage-bandwidth-templates.md)|
|Informacje o rekordach kontroli dostępu</br>Tworzenie rekordu kontroli dostępu</br>Edytuj rekord kontroli dostępu</br>Usuwanie rekordów kontroli dostępu|Konfigurowanie → usługi Menedżera StorSimple|[Zarządzanie rekordami kontroli dostępu](storsimple-manage-acrs.md)|
|Wyświetlanie szczegółów zadania</br>Anulowanie zadania|Menedżer StorSimple usługi → zadania|[Zarządzanie zadaniami](storsimple-manage-jobs.md)
|Odbieranie alertów</br>Zarządzanie alertami</br>Przejrzyj alerty|Alerty usługi → Menedżera StorSimple|[Wyświetlanie i Zarządzanie alertami StorSimple](storsimple-manage-alerts.md)
|Wyświetlanie połączonego inicjator</br>Znajdź numer seryjny urządzenia</br>Znajdź element docelowy IQN|StorSimple Menedżer urządzeń usługi → → pulpitu nawigacyjnego|[Za pomocą pulpitu nawigacyjnego urządzenia StorSimple](storsimple-device-dashboard.md)|
|Tworzenie wykresów monitorowania|StorSimple Menedżer urządzeń usługi → → monitorowania|[Monitorowanie urządzenia StorSimple](storsimple-monitor-device.md)|
|Dodać kontener głośności</br>Modyfikowanie kontenera głośności</br>Usuwanie kontenera głośności|StorSimple → usługi Menedżer urządzeń → głośność kontenerów|[Zarządzanie kontenerów głośności](storsimple-manage-volume-containers.md)|
|Dodawanie głośności</br>Modyfikowanie głośności</br>Przełączanie woluminu do trybu offline</br>Usuwanie woluminu</br>Monitorowanie woluminu|Ilości StorSimple Menedżera usługi → urządzeń → kontenerów głośność →|[Zarządzanie wielkości](storsimple-manage-volumes.md)|
|Modyfikowanie ustawień urządzenia</br>Modyfikowanie ustawień czasu</br>Modyfikowanie ustawień DNS.md</br>Konfigurowanie interfejsów|StorSimple Menedżer urządzeń usługi → skonfigurować →|[Modyfikowanie konfiguracji urządzenia dla swojego urządzenia StorSimple](storsimple-modify-device-config.md)|
|Wyświetlanie ustawień serwera proxy w sieci web|StorSimple Menedżer urządzeń usługi → skonfigurować →|[Konfigurowanie serwera proxy sieci web dla swojego urządzenia](storsimple-configure-web-proxy.md)|
|Modyfikowanie hasło administratora urządzenia</br>Modyfikowanie StorSimple migawkę Menedżer haseł|StorSimple Menedżer urządzeń usługi → skonfigurować →|[Zmienianie hasła StorSimple](storsimple-change-passwords.md)|
|Konfigurowanie zdalnego zarządzania|StorSimple Menedżer urządzeń usługi → skonfigurować →|[Zdalne łączenie się z urządzeniem StorSimple](storsimple-remote-connect.md)|
|Konfigurowanie ustawień alertów|StorSimple Menedżer urządzeń usługi → skonfigurować →|[Wyświetlanie i Zarządzanie alertami StorSimple](storsimple-manage-alerts.md)|
|Konfigurowanie CHAP dla swojego urządzenia StorSimple|StorSimple Menedżer urządzeń usługi → skonfigurować →|[Konfigurowanie CHAP dla swojego urządzenia StorSimple](storsimple-configure-chap.md)|
|Dodawanie zasady tworzenia kopii zapasowych</br>Dodawanie lub modyfikowanie harmonogramu</br>Usuwanie zasady tworzenia kopii zapasowych</br>Wykonać ręczną kopię zapasową</br>Tworzenie niestandardowego zasady tworzenia kopii zapasowych przy użyciu wielu wielkości i harmonogramów|Menedżer StorSimple usługi → urządzeń → kopii zapasowej zasad|[Zarządzanie zasadami kopii zapasowej](storsimple-manage-backup-policies.md)|
|Zatrzymywanie kontrolery urządzeń</br>Ponownie uruchom kontrolery urządzeń</br>Zamknij kontrolery urządzeń</br>Przywrócić domyślne ustawienia fabryczne urządzenia</br>(Powyżej są tylko urządzenia lokalne)|StorSimple Menedżer urządzeń usługi → → konserwacji|[Zarządzanie kontroler urządzenia StorSimple](storsimple-manage-device-controller.md)|
|Dowiedz się więcej o StorSimple sprzętowej</br>Monitoruje stan sprzętu</br>(Powyżej są tylko urządzenia lokalne)|StorSimple Menedżer urządzeń usługi → → konserwacji|[Składniki sprzętowe monitora](storsimple-monitor-hardware-status.md)|
|Tworzenie pakietu pomocy technicznej|StorSimple Menedżer urządzeń usługi → → konserwacji|[Tworzenie i zarządzanie nimi pakiet pomocy technicznej](storsimple-create-manage-support-package.md)|
|Instalowanie aktualizacji oprogramowania|StorSimple Menedżer urządzeń usługi → → konserwacji|[Aktualizowanie urządzenia](storsimple-update-device.md)|


##<a name="next-steps"></a>Następne kroki
Jeśli napotkasz jakiekolwiek problemy z codziennym działaniem urządzenia StorSimple lub dowolny z jego składników sprzętu, zobacz:

- [Rozwiązywanie problemów z działającego urządzenia](storsimple-troubleshoot-operational-device.md)
- [Używanie StorSimple monitorowania wskaźników](storsimple-monitoring-indicators.md)

Jeśli nie możesz rozwiązać problemy, należy utworzyć żądanie obsługi dotyczą [Kontakt z pomocą techniczną firmy Microsoft](storsimple-contact-microsoft-support.md).
