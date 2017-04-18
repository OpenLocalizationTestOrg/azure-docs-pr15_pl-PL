<properties 
   pageTitle="Zmiana trybu urządzenia StorSimple | Microsoft Azure"
   description="W tym artykule opisano tryby urządzenia StorSimple i wyjaśniono, jak używać programu Windows PowerShell dla StorSimple Aby zmienić tryb urządzenia."
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
   ms.date="06/17/2016"
   ms.author="alkohli" />

# <a name="change-the-device-mode-on-your-storsimple-device"></a>Zmiana trybu urządzenie na urządzeniu StorSimple

Ten artykuł zawiera krótki opis różnych metod, w których może działać urządzenia StorSimple. Urządzenie StorSimple może działać w trzech trybach: Normalny, konserwacji i odzyskiwania. 

Po zapoznaniu się w tym artykule, będziesz wiedzieć:

- Jakie są tryby urządzenia StorSimple są
- Obliczanie używanego trybu urządzenia StorSimple znajduje się w
- Jak zmienić z normalny tryb konserwacji i *odwrotnie*


Powyższe zadania związane z zarządzaniem można przeprowadzić tylko za pośrednictwem interfejsu programu Windows PowerShell urządzenia StorSimple.

## <a name="about-storsimple-device-modes"></a>Informacje o trybach urządzenia StorSimple

Urządzenie StorSimple może działać w trybie normalnym, konserwacji lub odzyskiwania. Każdy z następujących trybów pokrótce opisano poniżej.

### <a name="normal-mode"></a>Tryb normalny

To jest definiowana jako normalny tryb operacyjny dla w pełni skonfigurowanego urządzenia StorSimple. Domyślnie urządzenia powinny być w trybie normalnym.

### <a name="maintenance-mode"></a>Tryb konserwacji

Czasami urządzenia StorSimple może być konieczne umieszczane w trybie konserwacji. W tym trybie umożliwia wykonywanie konserwacji na tym urządzeniu i zainstaluj aktualizacje kłopotliwe środki, takie jak Kontakty związane z dysku oprogramowania układowego.

System można umieszczać w trybie konserwacji tylko przy użyciu programu Windows PowerShell dla StorSimple. Wszystkie żądania We/Wy są wstrzymywane w tym trybie. Usług, takich jak pamięci RAM trwała (NVRAM) lub usługę klastrów także zostały zatrzymane. Oba kontrolery ponownego uruchomienia po wprowadzeniu lub opuścić ten tryb. Po zakończeniu tryb konserwacji wszystkich usług będzie życiorys i powinna być prawidłowy. To może potrwać kilka minut.

>[AZURE.NOTE]Tryb konserwacji **jest obsługiwana tylko na urządzeniu działa prawidłowo. Nie jest obsługiwane na urządzeniu, w którym jednej lub obu kontroler nie działają.**
</br>

### <a name="recovery-mode"></a>Tryb odzyskiwania

Tryb odzyskiwania można określić jako "Trybem awaryjnym dla systemu Windows z obsługą sieci". Tryb odzyskiwania uczestniczy zespołu Microsoft Support i umożliwia im wykonywanie Diagnostyka sieci. Podstawowym celem trybie odzyskiwania jest pobrać dzienniki systemu.

Jeśli system przechodzi do trybu odzyskiwania, należy skontaktować się Support firmy Microsoft dla następnych kroków. Aby uzyskać więcej informacji przejdź do [Kontakt z pomocą techniczną firmy Microsoft](storsimple-contact-microsoft-support.md).

>[AZURE.NOTE] **Urządzenie nie można umieścić w trybie odzyskiwania. Jeśli urządzenie jest w złym stanie, trybie odzyskiwania próbuje uzyskać urządzenia do stanu, w którym personelu Support firmy Microsoft, można sprawdzić go.**

## <a name="determine-storsimple-device-mode"></a>Określanie tryb urządzenia StorSimple

#### <a name="to-determine-the-current-device-mode"></a>Aby określić bieżący tryb urządzenia

1. Zaloguj się do konsoli szeregowego urządzenia, wykonując czynności opisane w [Kit Użyj nawiązywania połączenia z konsoli szeregowego urządzenia](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).

2. Przyjrzyj się komunikat w postaci transparentu w menu Konsola szeregowego urządzenia. Ten komunikat wyraźnie wskazuje, czy urządzenie jest w trybie konserwacji lub odzyskiwania. Jeśli wiadomość nie zawiera dowolne informacje o odnoszących się do trybu systemu, urządzenie jest w trybie normalnym.

## <a name="change-the-storsimple-device-mode"></a>Zmień tryb urządzenia StorSimple 

Urządzenie StorSimple można umieścić w tryb konserwacji (w trybie normalnym) do wykonywania konserwacji lub instalowanie aktualizacji tryb konserwacji. Wykonywanie poniższych procedur, aby wprowadzić lub zakończenie trybu konserwacji.

> [AZURE.IMPORTANT] Przed wprowadzeniem tryb konserwacji, sprawdź, czy oba kontrolery urządzeń są prawidłowy po zalogowaniu się do **Stanu sprzętu** na stronie **Obsługa** w portalu klasyczny Azure. Jeśli co najmniej obu kontroler nie jest prawidłowy, skontaktuj się z Support firmy Microsoft dla następnych kroków. Aby uzyskać więcej informacji przejdź do [Kontakt z pomocą techniczną firmy Microsoft](storsimple-contact-microsoft-support.md).

#### <a name="to-enter-maintenance-mode"></a>Aby przejść do trybu konserwacji

1. Zaloguj się do konsoli szeregowego urządzenia, wykonując czynności opisane w [Kit Użyj nawiązywania połączenia z konsoli szeregowego urządzenia](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).

2. W menu Konsola szeregowa wybierz opcję 1, **Zaloguj się przy użyciu pełnego dostępu**. Po wyświetleniu monitu należy podać **hasło administratora urządzenia**. Hasło domyślne to: `Password1`.

3. W wierszu polecenia wpisz 

    `Enter-HcsMaintenanceMode`

4. Zostanie wyświetlony komunikat ostrzegawczy informujący, że tryb konserwacji będzie przerwać wszystkie żądania We/Wy i Server połączenia z portalem klasyczny Azure i zostanie wyświetlony monit o potwierdzenie. Wpisz **Y** , aby przejść do trybu konserwacji.

5. Oba kontrolery uruchomi się ponownie. Po zakończeniu po ponownym uruchomieniu komputera innym pojawi się komunikat informujący, że urządzenie jest w trybie konserwacji.


#### <a name="to-exit-maintenance-mode"></a>Aby wyjść z trybu konserwacji

1. Zaloguj się do konsoli szeregowego urządzenia. Upewnij się, za pomocą wiadomości transparent, które urządzenie jest w trybie konserwacji.

2. W wierszu polecenia wpisz:

    `Exit-HcsMaintenanceMode`

3. Pojawi się komunikat ostrzegawczy i komunikat z potwierdzeniem. Wpisz **Y** , aby wyjść z trybu konserwacji.

4. Oba kontrolery uruchomi się ponownie. Po zakończeniu po ponownym uruchomieniu komputera innym pojawi się komunikat informujący, że urządzenie jest w trybie normalnym.


## <a name="next-steps"></a>Następne kroki

Dowiedz się, jak [zastosować aktualizacje trybie normalnym i konserwacji](storsimple-update-device.md) na urządzeniu StorSimple.

