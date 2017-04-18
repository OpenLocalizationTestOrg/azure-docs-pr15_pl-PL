<properties 
   pageTitle="Instalowanie aktualizacji w macierzy Virtual StorSimple | Microsoft Azure"
   description="Informacje dotyczące używania web tablicy Virtual StorSimple interfejsu użytkownika stosowania aktualizacji przy użyciu metody portal i poprawki"
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="09/07/2016"
   ms.author="alkohli" />

# <a name="install-updates-on-your-storsimple-virtual-array"></a>Instalowanie aktualizacji w macierzy wirtualnych StorSimple

## <a name="overview"></a>Omówienie

W tym artykule opisano czynności wymagane do zainstalowania aktualizacji na macierzy Virtual StorSimple za pośrednictwem lokalnych interfejs użytkownika sieci web i za pośrednictwem portalu klasyczny Azure. Należy zastosować aktualizacje oprogramowania lub poprawek, aby zapewnić aktualność macierzy Virtual StorSimple. 

Należy pamiętać, że instalowaniu aktualizacji lub poprawki ponownym uruchomieniu urządzenia. Zakładając, że tablica Virtual StorSimple jest urządzenie jednego węzła, wszelkie we/wy w toku jest zakłócenia i urządzenia napotkania przestoje. 

Przed zastosowaniem aktualizacji, zaleca się wykonanie wielkości lub akcji w trybie offline na hoście pierwszej, a następnie urządzenie. To minimalizuje możliwości uszkodzenie danych.

> [AZURE.IMPORTANT] Jeśli korzystasz z aktualizacji 0,1 lub wersje oprogramowania GA, musi użyj metody poprawki za pośrednictwem lokalnych sieci web interfejsu użytkownika, aby zainstalować aktualizację 0,3. Jeśli korzystasz z aktualizacji 0,2, zaleca się zainstalowanie aktualizacji za pośrednictwem portalu klasyczny Azure.

## <a name="use-the-local-web-ui"></a>Korzystanie z lokalnego interfejsu użytkownika w sieci web 
 
Istnieją dwa kroki podczas korzystania z lokalnego interfejsu użytkownika w sieci web:

- Pobieranie aktualizacji lub poprawki
- Zainstalowanie aktualizacji lub poprawki

### <a name="download-the-update-or-the-hotfix"></a>Pobieranie aktualizacji lub poprawki

Wykonaj następujące czynności w celu pobrania aktualizacji oprogramowania z wykazu usługi Microsoft Update.

#### <a name="to-download-the-update-or-the-hotfix"></a>Aby pobrać tę aktualizację lub poprawki

1. Uruchom program Internet Explorer i przejdź do [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).

2. Jeśli jest używany po raz pierwszy za pomocą wykazu usługi Microsoft Update na tym komputerze, kliknij przycisk **Zainstaluj** , po wyświetleniu monitu o zainstalować dodatek wykazu usługi Microsoft Update.
  
3. W polu wyszukiwania wykazu usługi Microsoft Update wprowadź numer bazy wiedzy Knowledge Base (KB) poprawki, które chcesz pobrać. Wprowadź **3182061** aktualizacji 0,3, a następnie kliknij przycisk **Wyszukaj**.

    Lista poprawki zostanie wyświetlone, na przykład **StorSimple wirtualnych tablicy aktualizacji 0,3**.

    ![Katalog wyszukiwania](./media/storsimple-ova-install-update-01/download1.png)

4. Kliknij przycisk **Dodaj**. Aktualizacja zostanie dodany do koszyka.

5. Kliknij łącze **Wyświetl koszyk**.

6. Kliknij przycisk **Pobierz**. Określanie lub **Przeglądanie** lokalne miejsce, w którym chcesz pobierania się pojawić. Aktualizacje są pobierane do określonej lokalizacji i umieszczony w podfolderze o takiej samej nazwie jak w przypadku aktualizacji. Ponadto można skopiować folderu w udziale sieciowym, który jest dostępny z urządzenia.

7. Otwórz folder skopiowany, powinien zostać wyświetlony plik Microsoft Update autonomicznego pakietu `WindowsTH-KB3011067-x64`. Ten plik jest używany do instalowania aktualizacji lub poprawki.


### <a name="install-the-update-or-the-hotfix"></a>Zainstalowanie aktualizacji lub poprawki

Przed zainstalowaniem aktualizacji lub poprawki upewnij się, że zainstalowano aktualizację lub poprawki pobierane lokalnie na swoim hoście lub dostępny za pośrednictwem udziału sieciowego. 

Ta metoda umożliwia instalowanie aktualizacji na urządzeniu z systemem GA lub aktualizowanie 0.1 wersji oprogramowania. Ta procedura ma mniej niż 2 minut. Wykonaj poniższe czynności, aby zainstalować aktualizacji lub poprawki.


#### <a name="to-install-the-update-or-the-hotfix"></a>Aby zainstalować aktualizację lub poprawki

1. Lokalne witryny interfejsu użytkownika, przejdź do **utrzymania** > **Aktualizacji oprogramowania**.

    ![aktualizowanie urządzenia](./media/storsimple-ova-install-update-01/update1m.png)

2. W **zaktualizować ścieżkę pliku**wprowadź nazwę pliku aktualizacji lub poprawki. Można również przeglądać plik instalacji aktualizacji lub poprawki, jeśli umieszczony w udziale sieciowym. Kliknij przycisk **Zastosuj**.

    ![aktualizowanie urządzenia](./media/storsimple-ova-install-update-01/update2m.png)

3.  Wyświetlane jest ostrzeżenie. Podane to jest urządzeniem jednego węzła, po aktualizacji, ponownym uruchomieniu urządzenia i jest przestoje. Kliknij ikonę wyboru.

    ![aktualizowanie urządzenia](./media/storsimple-ova-install-update-01/update3m.png)

4. Zostanie uruchomiony aktualizacji. Po pomyślnym aktualizacji urządzenia ponownego uruchomienia. Lokalny interfejs użytkownika nie jest dostępny w tej długości.

    ![aktualizowanie urządzenia](./media/storsimple-ova-install-update-01/update5m.png)

5. Po zakończeniu po ponownym uruchomieniu komputera są pobierane na stronie **Zaloguj się** . Aby sprawdzić, czy oprogramowanie urządzenia zostały zaktualizowane w lokalnej sieci web interfejsu użytkownika, przejdź do **utrzymania** > **Aktualizacji oprogramowania**. Wersja oprogramowania wyświetlonej powinny być **10.0.0.0.0.10288.0** aktualizacji 0,3.

    > [AZURE.NOTE] Firma Microsoft raport wersje oprogramowania w nieco inny sposób w lokalnej sieci web interfejsu użytkownika i Azure portalu klasyczny. Na przykład interfejs użytkownika w sieci web lokalne raporty **10.0.0.0.0.10288** portalu klasyczny Azure raportach i **10.0.10288.0** dla tej samej wersji. 

    ![aktualizowanie urządzenia](./media/storsimple-ova-install-update-01/update6m.png)





## <a name="use-the-azure-classic-portal"></a>Za pomocą portalu klasyczny Azure

Jeśli z aktualizacji 0,2, zaleca się zainstalowanie aktualizacji za pośrednictwem portalu klasyczny Azure. Procedura portalu wymaga użytkownika do przeglądania, Pobierz i zainstaluj aktualizacje. Ta procedura ma około 7 minut do wykonania. Wykonaj poniższe czynności, aby zainstalować aktualizacji lub poprawki.

[AZURE.INCLUDE [storsimple-ova-install-update-via-portal](../../includes/storsimple-ova-install-update-via-portal.md)]

Po zakończeniu instalacji (co wskazuje stan zadania w skali 100%), przejdź do strony **urządzeń > konserwacja > aktualizacji oprogramowania**. Wersja oprogramowania wyświetlonej powinny być 10.0.10288.0.

![aktualizowanie urządzenia](./media/storsimple-ova-install-update-01/azupdate12m.png)

## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej o [administrowaniu macierzy Virtual StorSimple](storsimple-ova-web-ui-admin.md).
