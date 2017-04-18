<properties 
   pageTitle="Zmienianie hasła administratora urządzenia wirtualnego StorSimple | Microsoft Azure"
   description="W tym artykule opisano, jak zmienić hasło administratora urządzenia za pomocą portalu klasyczny Azure lub web tablicy Virtual StorSimple interfejsu użytkownika."
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
   ms.date="06/17/2016"
   ms.author="alkohli" />

# <a name="change-the-storsimple-virtual-array-device-administrator-password"></a>Zmienianie hasła administratora urządzenia StorSimple wirtualnych tablicy

## <a name="overview"></a>Omówienie

Podczas uzyskiwania dostępu do urządzenia wirtualnego StorSimple, korzystając z interfejsu programu Windows PowerShell, należy wprowadzić hasło administratora urządzenia. Gdy urządzenia StorSimple jest najpierw obsługi administracyjnej i pracę, domyślnym hasłem jest *hasła1*. Zabezpieczania danych domyślne hasło wygasa przy pierwszym zalogowaniu się, które są wymagane do zmiany tego hasła.

Aby zmienić hasło administratora urządzenia w dowolnym momencie, gdy urządzenie jest używany w środowisku produkcyjnym, można użyć interfejs użytkownika w sieci web lokalne lub portalu klasyczny Azure. Każde z tych procedur opisano w tym artykule.

## <a name="use-the-azure-classic-portal-to-change-the-password"></a>Zmienianie hasła za pomocą portalu klasyczny Azure

Wykonaj poniższe czynności, aby zmienić hasło administratora urządzenia za pośrednictwem portalu klasyczny Azure.

#### <a name="to-change-the-device-administrator-password-via-the-azure-classic-portal"></a>Aby zmienić hasło administratora urządzenia za pośrednictwem portalu klasyczny Azure

1. W portalu, kliknij pozycję **urządzenia** > **konfiguracji** dla danego urządzenia.

2. Przewiń w dół do sekcji **Hasło administratora urządzenia** . Hasło administratora zawierający od 8 do 15 znaków. Hasło musi być kombinacją wielkie litery, małe litery, liczbowe i znaki specjalne.

3. Potwierdź hasło.

4. Kliknij pozycję **Zapisz** u dołu strony.

Teraz powinny być aktualizowane hasło administratora urządzenia. To hasło modyfikacji umożliwia dostęp do lokalnie na urządzeniu.

## <a name="use-the-storsimple-virtual-array-web-ui-to-change-the-password"></a>Zmienianie hasła za pomocą tablicy Virtual StorSimple sieci web interfejsu użytkownika

Wykonaj poniższe czynności, aby zmienić hasło administratora urządzenia za pośrednictwem lokalnych sieci web interfejsu użytkownika.

#### <a name="to-change-the-device-administrator-password-via-the-local-web-ui"></a>Aby zmienić hasło administratora urządzenia za pośrednictwem lokalnych sieci web interfejsu użytkownika

1. Lokalne witryny interfejsu użytkownika, kliknij pozycję **Konserwacja** > **Zmienianie hasła** dla danego urządzenia.

    ![Zmienianie hasła1](./media/storsimple-ova-change-device-admin-password/image40.png)

2. Wpisz **bieżące hasło**.

3. Podaj **nowe hasło**. Hasło musi zawierać co najmniej 8 znaków. Musi zawierać 3, 4 z następujących czynności: wielkie litery, małe litery, liczbowe i znaki specjalne.

    Należy zauważyć, że hasło nie może być taka sama jak ostatnich 24 haseł.

3. Wprowadź ponownie hasło, aby je potwierdzić.

    ![Zmienianie Hasło2](./media/storsimple-ova-change-device-admin-password/image41.png)

4. U dołu strony kliknij przycisk **Zastosuj**. Następnie zostaną zastosowane nowe hasło. W przypadku zmiany hasła nie powiedzie, zobaczysz następujący komunikat o błędzie.

    ![Błąd hasła](./media/storsimple-ova-change-device-admin-password/image42.png)

    Po pomyślnym zaktualizowaniu hasła, otrzymasz powiadomienie. Następnie można to zmienione hasło dostępu lokalnie na urządzeniu.

## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej o [administrowaniu macierzy Virtual StorSimple](storsimple-ova-web-ui-admin.md).
