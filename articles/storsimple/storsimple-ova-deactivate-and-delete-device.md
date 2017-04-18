<properties 
   pageTitle="Dezaktywowanie i usuwanie tablicę Virtual StorSimple | Microsoft Azure"
   description="Opisano sposób usuwania urządzenia StorSimple z usługi najpierw dezaktywowanie go, a następnie usuwając go."
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
   ms.date="06/20/2016"
   ms.author="alkohli" />

# <a name="deactivate-and-delete-a-storsimple-virtual-array"></a>Dezaktywowanie i usuwanie tablicę wirtualnych StorSimple

## <a name="overview"></a>Omówienie

Po zdezaktywowaniu tablicę Virtual StorSimple Server połączenia między urządzeniem i odpowiadające im usługi Menedżera StorSimple. Dezaktywacja jest operacji stałe i nie można cofnąć. Wyłączone urządzenia nie można zarejestrować w usłudze Menedżer StorSimple ponownie.

Może być konieczne Dezaktywuj i usuń urządzenie wirtualne StorSimple w następujących sytuacjach:


- Urządzenie jest w trybie online i planowanie niepowodzenie na tym urządzeniu. Konieczne może być to zrobić, jeśli planujesz przeprowadzić uaktualnienie do większych urządzenia. Po przesłaniu danych urządzenia i zakończeniu tym przełączeniu, można usunąć urządzenie.

- Urządzenie jest w trybie offline i planowanie niepowodzenie na tym urządzeniu. Może wystąpić, w przypadku awarii z powodu awarii w obrębie centrum danych, główne urządzenie w przypadku w dół. Planowanie niepowodzenie na urządzeniu pomocniczym na tym urządzeniu. Po przesłaniu danych urządzenia i tym przełączeniu zostanie zakończone, można usunąć urządzenie.

- Chcesz zlikwidować urządzenia i usuń go. 
 

Po zdezaktywowaniu urządzeniu dane, które są przechowywane lokalnie nie będzie już dostępne. Czy można odzyskać tylko tych danych, które są przechowywane w chmurze. Jeśli planujesz zachować dane urządzenia po dezaktywacji, następnie które należy wykonać migawkę chmury wszystkich danych przed dezaktywować urządzenia. Dzięki temu będzie można odzyskać wszystkie dane w późniejszym terminie.


Ten samouczek w tym miejscu wyjaśniono, jak:

- Dezaktywowanie urządzenia 
- Usuwanie urządzenia został dezaktywowany


## <a name="deactivate-a-device"></a>Dezaktywowanie urządzenia

Wykonaj poniższe czynności, aby zdezaktywować urządzenia.

#### <a name="to-deactivate-the-device"></a>Aby dezaktywować na urządzeniu   

1. Przejdź do strony **urządzenia** . Wybierz urządzenie, którego chcesz zdezaktywować.

    ![Wybierz urządzenie, aby zdezaktywować](./media/storsimple-ova-deactivate-and-delete-device/deactivate1m.png)

3. U dołu strony kliknij przycisk **Dezaktywuj**.

    ![Kliknij przycisk Dezaktywuj](./media/storsimple-ova-deactivate-and-delete-device/deactivate2m.png)

4. Pojawi się komunikat z potwierdzeniem. Kliknij przycisk **Tak,** aby kontynuować. 

    ![Upewnij się, Dezaktywuj](./media/storsimple-ova-deactivate-and-delete-device/deactivate3m.png)

    Proces Dezaktywuj uruchomi i potrwać kilka minut.

    ![Dezaktywowanie w toku](./media/storsimple-ova-deactivate-and-delete-device/deactivate4m.png)

3. Po dezaktywacji będzie można odświeżyć listę urządzeń. 

    ![Dezaktywowanie wykonania](./media/storsimple-ova-deactivate-and-delete-device/deactivate5m.png)

    Możesz teraz usunąć to urządzenie. 

## <a name="delete-the-device"></a>Usuwanie urządzenia

Urządzenie ma najpierw są nieaktywne Aby można było go usunąć. Urządzenie usunięcie go z listy urządzeń podłączonych do usługi. Usługę można zarządzać już usuniętego urządzenia. Dane skojarzone z urządzeniem pozostaną jednak w chmurze. Należy pamiętać, że te dane zostaną następnie Naliczanie opłat. 

Wykonaj poniższe czynności, aby usunąć urządzenie:

#### <a name="to-delete-the-device"></a>Aby usunąć urządzenie 

 1. Na stronie Menedżera StorSimple usługi **urządzeń** wybierz urządzenie został dezaktywowany, które chcesz usunąć.

    ![Wybierz urządzenie, aby usunąć](./media/storsimple-ova-deactivate-and-delete-device/deactivate5m.png)

 2. U dołu strony kliknij przycisk **Usuń**.
 
    ![Kliknij przycisk Usuń](./media/storsimple-ova-deactivate-and-delete-device/deactivate6m.png)

 3. Pojawi się monit o potwierdzenie. Wpisz nazwę urządzenia, aby potwierdzić usunięcie urządzenia. Należy zauważyć, że usunięcie urządzenia nie spowoduje usunięcia danych chmury skojarzone z urządzeniem. Kliknij ikonę wyboru, aby kontynuować.
 
    ![Potwierdzanie usunięcia](./media/storsimple-ova-deactivate-and-delete-device/deactivate7m.png) 

 5. Może potrwać kilka minut na urządzeniu, zostanie usunięta. 

    ![Usuwanie w toku](./media/storsimple-ova-deactivate-and-delete-device/deactivate8m.png)

    Po usunięciu urządzenia są odświeżane listę urządzeń.

    ![Usuń ukończone](./media/storsimple-ova-deactivate-and-delete-device/deactivate9m.png)


## <a name="next-steps"></a>Następne kroki

- Aby dowiedzieć się więcej na temat używania usługi Menedżera StorSimple, przejdź do [korzystania z usługi Menedżera StorSimple administrowania macierzy Virtual StorSimple](storsimple-ova-manager-service-administration.md). 