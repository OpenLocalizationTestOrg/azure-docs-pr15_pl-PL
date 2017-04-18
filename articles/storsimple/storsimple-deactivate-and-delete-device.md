<properties 
   pageTitle="Dezaktywowanie i usuwanie urządzenia StorSimple | Microsoft Azure"
   description="Opisano sposób usuwania urządzenia StorSimple z usługi najpierw dezaktywowanie go, a następnie usuwając go."
   services="storsimple"
   documentationCenter=""
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="anoobbacker" />

# <a name="deactivate-and-delete-a-storsimple-device"></a>Dezaktywowanie i usuwanie urządzenia StorSimple

## <a name="overview"></a>Omówienie

Możesz wykonać urządzeniu StorSimple wylogowywanie się z usługi (na przykład, jeśli są zastępowanie lub uaktualnianie urządzenia lub nie są już używasz StorSimple). Jeśli jest to możliwe, będzie konieczne dezaktywować na urządzeniu, aby można było go usunąć. Dezaktywowanie serwerów połączenia między urządzeniem i odpowiadające im usługi Menedżera StorSimple. Ten samouczek wyjaśniono, jak usunąć urządzenie StorSimple z usługi dezaktywowanie go, a następnie usuwając go. 

Po zdezaktywowaniu urządzeniu dane, które są przechowywane lokalnie na urządzeniu nie będzie już dostępne. Czy można odzyskać tylko tych danych, które są skojarzone z urządzeniem, które są przechowywane w chmurze.  

>[AZURE.WARNING] Dezaktywacja jest operacji stałe i nie można cofnąć. Nie można zarejestrować urządzenie dezaktywowany usłudze Menedżer StorSimple, o ile zostanie najpierw zresetowane przez fabryki. 
>
>Fabryki Resetowanie proces usuwa wszystkie dane, które są przechowywane lokalnie na urządzeniu. Dlatego istotne jest zrób migawkę chmury wszystkich danych, zanim dezaktywować urządzenia. Dzięki temu będzie można odzyskać wszystkie dane w późniejszym terminie.

Ten samouczek w tym miejscu wyjaśniono, jak:

- Dezaktywowanie urządzenia i usuwania danych
- Dezaktywowanie urządzenia i zachować dane

Ponadto wyjaśniono sposób zdezaktywowaniu i usuwania działania na urządzenie wirtualne StorSimple.

>[AZURE.NOTE] Przed dezaktywowaniu urządzenie fizycznej lub wirtualnej StorSimple upewnij się zatrzymać lub usunąć klientów i hostów, które są zależne od urządzenia.

## <a name="deactivate-and-delete-data"></a>Dezaktywowanie i usuwanie danych

Jeśli interesuje całkowite usunięcie urządzenia i nie chcesz, aby zachować dane na tym urządzeniu, wykonaj następujące czynności.

#### <a name="to-deactivate-the-device-and-delete-the-data"></a>Aby dezaktywować urządzenia i usuwania danych  

1. Przed dezaktywowanie urządzeniu, musisz usunąć wszystkie wielkość kontenerów (i wielkość) skojarzone z urządzeniem. Możesz usunąć kontenerów głośność dopiero po usunięciu skojarzone kopie zapasowe.

2. Dezaktywowanie urządzenia w następujący sposób:

    1. Na stronie Menedżera StorSimple usługi **urządzeń** wybierz urządzenie, które chcesz dezaktywować, a następnie u dołu strony kliknij przycisk **Dezaktywuj**.

    2. Pojawi się komunikat z potwierdzeniem. Kliknij przycisk **Tak,** aby kontynuować. Proces Dezaktywuj uruchomi i potrwać kilka minut.

3. Po dezaktywacji można całkowicie usunąć urządzenie. Urządzenie usunięcie go z listy urządzeń podłączonych do usługi. Usługę można zarządzać już usuniętego urządzenia. Aby usunąć urządzenie, wykonaj następujące czynności:

    1. Na stronie Menedżera StorSimple usługi **urządzeń** wybierz urządzenie został dezaktywowany, które chcesz usunąć.

    2. U dołu strony kliknij przycisk **Usuń**.

    3. Pojawi się monit o potwierdzenie. Kliknij przycisk **Tak,** aby kontynuować.

    Może potrwać kilka minut na urządzeniu, zostanie usunięta.

## <a name="deactivate-and-retain-data"></a>Dezaktywowanie i przechowywania danych

Jeśli interesuje podczas usuwania urządzenia, ale chcesz zachować dane, wykonaj następujące czynności.

####<a name="to-deactivate-a-device-and-retain-the-data"></a>Aby dezaktywować urządzenia i zachować dane 

1. Dezaktywowanie urządzenia. Wszystkie kontenery głośności i migawki urządzenia pozostaną.

    1. Na stronie Menedżera StorSimple usługi **urządzeń** wybierz urządzenie, które chcesz dezaktywować, a następnie u dołu strony kliknij przycisk **Dezaktywuj**.

    2. Pojawi się komunikat z potwierdzeniem. Kliknij przycisk **Tak,** aby kontynuować. Proces Dezaktywuj uruchomi i potrwać kilka minut.

2. Teraz można się niepowodzeniem nad kontenerów głośności i skojarzone migawki. Aby uzyskać procedury przejdź do [odzyskiwania awaryjnego i danych dla swojego urządzenia StorSimple](storsimple-device-failover-disaster-recovery.md).

3. Po dezaktywacji i pracy awaryjnej można całkowicie usunąć urządzenie. Urządzenie usunięcie go z listy urządzeń podłączonych do usługi. Usługę można zarządzać już usuniętego urządzenia. Wykonaj poniższe czynności, aby usunąć urządzenie:
 
    1. Na stronie Menedżera StorSimple usługi **urządzeń** wybierz urządzenie został dezaktywowany, które chcesz usunąć.

    2. U dołu strony kliknij przycisk **Usuń**.

    3. Pojawi się monit o potwierdzenie. Kliknij przycisk **Tak,** aby kontynuować.

    Może potrwać kilka minut na urządzeniu, zostanie usunięta.

## <a name="deactivate-and-delete-a-virtual-device"></a>Dezaktywowanie i usuwanie urządzenie wirtualne

Dla urządzenie wirtualne StorSimple dezaktywacji zwalnia maszyny wirtualnej. Następnie można usunąć maszyny wirtualnej i zasoby utworzenia został zainicjowany. Po zdezaktywowaniu urządzenia wirtualnego nie można przywrócić do poprzedniego stanu. 

Dezaktywacja powoduje następujące działania:

- Urządzenia wirtualnego StorSimple zostanie usunięty.

- OSDisk i dyski danych utworzona dla urządzenia wirtualnego StorSimple zostaną usunięte.

- Usługi hostowanej i wirtualnej sieci, które zostały utworzone podczas inicjowania obsługi administracyjnej są zachowywane. Jeśli nie korzystasz z tych obiektów, należy usunąć je ręcznie.

- Migawki chmury utworzone przez urządzenia wirtualnego StorSimple są zachowywane.

## <a name="next-steps"></a>Następne kroki
- Aby przywrócić domyślne na urządzeniu został dezaktywowany, przejdź do [Resetuj domyślne ustawienia fabryczne urządzenia](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings).

- Aby uzyskać pomoc techniczną, [skontaktuj się z pomocą techniczną firmy Microsoft](storsimple-contact-microsoft-support.md).

- Aby dowiedzieć się więcej na temat używania usługi Menedżera StorSimple, przejdź do [korzystania z usługi Menedżera StorSimple administrowania urządzenia StorSimple](storsimple-manager-service-administration.md). 
