<properties 
   pageTitle="Administracja menedżera StorSimple wirtualnych tablicy | Microsoft Azure"
   description="Dowiedz się, jak zarządzać Virtual StorSimple tablicy w lokalnej za pomocą usługi Menedżera StorSimple w portalu klasyczny Azure."
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
   ms.date="10/11/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-administer-your-storsimple-virtual-array"></a>Administrowanie macierzy Virtual StorSimple za pomocą usługi Menedżera StorSimple

![przebieg procesu konfiguracji](./media/storsimple-ova-manager-service-administration/manage4.png)

## <a name="overview"></a>Omówienie

W tym artykule opisano interfejs usługi Menedżera StorSimple, jak połączyć oraz różnych dostępnych opcji, w tym i łącza do konkretnych przepływów pracy, które mogą być wykonywane za pośrednictwem tego interfejsu użytkownika. 

Po zapoznaniu się w tym artykule, będziesz wiedzieć, jak:

- Nawiązywanie połączenia z usługą Menedżera StorSimple
- Nawigowanie interfejsu użytkownika Menedżer StorSimple
- Administrowanie macierzy Virtual StorSimple za pośrednictwem usługi Menedżera StorSimple

> [AZURE.NOTE] Aby wyświetlić opcje zarządzania dostępne w przypadku urządzenia serii StorSimple 8000, przejdź do tematu [Używanie usługę Menedżer StorSimple administrowania urządzenia StorSimple](storsimple-manager-service-administration.md).

## <a name="connect-to-the-storsimple-manager-service"></a>Nawiązywanie połączenia z usługą Menedżera StorSimple

Usługa Menedżera StorSimple działa w programie Microsoft Azure i łączy wiele tablic wirtualnych StorSimple. Zarządzanie tych urządzeń za pomocą centralnej portalu klasyczny Microsoft Azure działa w przeglądarce. Aby połączyć się z usługą Menedżera StorSimple, wykonaj następujące czynności.

#### <a name="to-connect-to-the-service"></a>Aby połączyć się z usługą

1. Przejdź do [https://manage.windowsazure.com/](https://manage.windowsazure.com/).

2. Przy użyciu poświadczeń konta Microsoft, zaloguj się do portalu Microsoft Azure klasyczny (znajdujące się w górnym rogu okienka).

3. Przewiń w dół w okienku nawigacji po lewej stronie, aby uzyskać dostęp do usług Menedżera StorSimple.

    ![Przewiń do usługi](./media/storsimple-ova-manager-service-administration/admin-scroll.png)

## <a name="navigate-the-storsimple-manager-service-ui"></a>Nawigowanie usługę Menedżer StorSimple interfejsu użytkownika

W poniższej tabeli przedstawiono hierarchii nawigacji w usłudze Menedżer StorSimple interfejsu użytkownika.

- Strona początkowa **Menedżera StorSimple** umożliwia przejście do strony poziom obsługi interfejsu użytkownika, które dotyczą wszystkich tablic wirtualnych w ramach usługi.

- Na stronie **urządzenia** umożliwia przejście do dotyczą tablicę wirtualnych określonych stron interfejsu użytkownika — na poziomie urządzenia.

#### <a name="storsimple-manager-service-navigational-hierarchy"></a>Hierarchia nawigacji usługi Menedżera StorSimple

|Strona główna|Poziom obsługi stron|Na poziomie urządzenia stron|
|---|---|---|
|Usługa Menedżera StorSimple|Pulpit nawigacyjny (usługa)|Pulpit nawigacyjny (urządzenie)|
||Urządzenia →|Monitorowanie|
||Wykaz kopii zapasowych|Akcje (serwer plików) lub </br>Ilości (serwerem)|
||Konfigurowanie (usługa)|Konfigurowanie (urządzenia)|
||Zadania|Konserwacja|
||Alerty|

## <a name="use-the-storsimple-manager-service-to-perform-management-tasks"></a>Do wykonywania zadań zarządzania za pomocą usługi Menedżera StorSimple

W poniższej tabeli przedstawiono podsumowanie wszystkich zadań administracyjnych i złożone przepływy pracy, które mogą być wykonywane w ramach usługi Menedżera StorSimple interfejsu użytkownika. Te zadania są zorganizowane oparte na stronach interfejsu użytkownika, na których są inicjowane.

Aby uzyskać więcej informacji na temat każdego przepływu pracy kliknij odpowiednią procedurę w tabeli.

#### <a name="storsimple-manager-workflows"></a>Menedżer StorSimple przepływów pracy

|Jeśli chcesz to zrobić...|Przejdź do tej strony interfejsu użytkownika...|Ta procedura|
|---|---|---|
|Tworzenie usługi</br>Usuwanie usługi</br>Uzyskiwanie klucza rejestru usługi</br>Ponownie wygenerować klucza rejestru usługi|Usługa Menedżera StorSimple|[Wdrażanie usługi Menedżera StorSimple](storsimple-ova-manage-service.md)|
|Zmienianie klucza szyfrowania danych usługi</br>Wyświetlanie dzienników operacji|Pulpit nawigacyjny StorSimple → usługi Menedżera|[Za pomocą pulpitu nawigacyjnego usług StorSimple](storsimple-ova-service-dashboard.md)|
|Dezaktywowanie tablicę wirtualny</br>Usuwanie wirtualnych tablicy|Menedżer StorSimple usługi → urządzeń|[Dezaktywowanie i usuwanie wirtualnych tablicy](storsimple-ova-deactivate-and-delete-device.md)|
|Pracy awaryjnej odzyskiwania i urządzenia po awarii</br>Wymagania wstępne dotyczące pracy awaryjnej</br>Przełączanie awaryjne do urządzenia wirtualne</br>Business ciągłości awarii (BCDR)</br>Błędy podczas awarii|Menedżer StorSimple usługi → urządzeń|[Po awarii odzyskiwania i urządzenia awaryjnego macierzy wirtualnych StorSimple](storsimple-ova-failover-dr.md)|
|Wykonywanie kopii zapasowej udział i ilości</br>Wykonać ręczną kopię zapasową</br>Zmienianie harmonogramu wykonywania kopii zapasowych</br>Wyświetlanie istniejących kopii zapasowych|Katalog kopii zapasowej → usługi Menedżera StorSimple|[Wykonywanie kopii zapasowej macierzy wirtualnych StorSimple](storsimple-ova-backup.md)|
|Przywracanie akcje z zestawu kopii zapasowych</br>Przywracanie wielkości z zestawu kopii zapasowych</br>Odzyskiwanie poziomie elementu (tylko w przypadku serwera plików)|Wykaz narzędzia Kopia zapasowa StorSimple → usługi Menedżera|[Przywracanie z kopii zapasowej macierzy wirtualnych StorSimple](storsimple-ova-restore.md)|
|Informacje o kontach miejsca do magazynowania</br>Dodawanie konta miejsca do magazynowania</br>Edytowanie konta miejsca do magazynowania</br>Usuwanie konta miejsca do magazynowania|Konfigurowanie → usługi Menedżera StorSimple|[Zarządzanie kontami miejsca do magazynowania dla macierzy wirtualnych StorSimple](storsimple-ova-manage-storage-accounts.md)|
|Informacje o rekordach kontroli dostępu</br>Dodawanie lub modyfikowanie rekord kontroli dostępu </br>Usuwanie rekordów kontroli dostępu|Konfigurowanie → usługi Menedżera StorSimple|[Zarządzanie rekordami kontrolki programu access tablicy wirtualnych StorSimple](storsimple-ova-manage-acrs.md)|
|Wyświetlanie szczegółów zadania|Menedżer StorSimple usługi → zadania| [Zarządzanie zadaniami StorSimple wirtualnych tablicy](storsimple-ova-manage-jobs.md)|
|Konfigurowanie ustawień alertów</br>Odbieranie alertów</br>Zarządzanie alertami</br>Przejrzyj alerty|Alerty usługi → Menedżera StorSimple|[Wyświetlanie i Zarządzanie alertami dla macierzy wirtualnych StorSimple](storsimple-ova-manage-alerts.md)|
|Modyfikowanie hasło administratora urządzenia|StorSimple Menedżer urządzeń usługi → skonfigurować →|[Zmienianie hasła administratora urządzenia StorSimple wirtualnych tablicy](storsimple-ova-change-device-admin-password.md)|
|Instalowanie aktualizacji oprogramowania|StorSimple Menedżer urządzeń usługi → → konserwacji|[Aktualizowanie wirtualnych macierzy](storsimple-ova-install-update-01.md)|

>[AZURE.NOTE] [Lokalne web interfejsu użytkownika](storsimple-ova-web-ui-admin.md) należy użyć dla następujących zadań:
>
>- [Pobieranie klucza szyfrowania danych usługi](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key)
>- [Tworzenie pakietu pomocy technicznej](storsimple-ova-web-ui-admin.md#generate-a-log-package)
>- [Zatrzymaj i uruchom ponownie tablicę wirtualny](storsimple-ova-web-ui-admin.md#shut-down-and-restart-your-device)

##<a name="next-steps"></a>Następne kroki
Uzyskać informacji na temat interfejs użytkownika w sieci web i jak używać tej funkcji przejdź do [Korzystanie z sieci web StorSimple interfejsu użytkownika umożliwiającego administrowanie macierzy Virtual StorSimple](storsimple-ova-web-ui-admin.md).
