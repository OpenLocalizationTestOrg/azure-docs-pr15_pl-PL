<properties 
   pageTitle="Zmienianie hasła StorSimple | Microsoft Azure" 
   description="Informacje dotyczące używania usługi Menedżera StorSimple Aby zmienić swoje hasło administratora Menedżera migawkę StorSimple i urządzenia." 
   services="storsimple" 
   documentationCenter="NA" 
   authors="alkohli" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD" 
   ms.date="08/17/2016"
   ms.author="alkohli"/>

# <a name="use-the-storsimple-manager-service-to-change-your-storsimple-passwords"></a>Zmienianie hasła StorSimple za pomocą usługi Menedżera StorSimple

## <a name="overview"></a>Omówienie 

Azure portal klasyczny strony **Konfigurowanie** zawiera wszystkie parametry urządzenia, które można zmienić na urządzeniu StorSimple, która zarządza usługa Menedżera StorSimple. Ten samouczek wyjaśniono, jak na stronie **Konfigurowanie** umożliwia zmienianie hasła StorSimple migawkę menedżera lub administratora urządzenia.

## <a name="change-the-device-administrator-password"></a>Zmienianie hasła administratora urządzenia

Korzystając z interfejsu programu Windows PowerShell uzyskać dostęp do urządzenia StorSimple, musisz wprowadzić hasło administratora urządzenia. Po pierwszym urządzeniu StorSimple jest zarejestrowany z usługą, domyślnym hasłem dla tego interfejsu jest *hasła1*. Zabezpieczania danych trzeba zmienić to hasło na końcu procesu rejestracji. Nie można zamknąć z procesu rejestracji, bez zmiany tego hasła. Aby uzyskać więcej informacji, zobacz [Krok 3: Konfigurowanie i zarejestrować urządzenie przy użyciu programu Windows PowerShell dla StorSimple](storsimple-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

Następnie można zmienić hasło, którego ustawiono najpierw za pośrednictwem interfejsu programu Windows PowerShell podczas rejestrowania za pośrednictwem portalu klasyczny Azure. Wykonaj poniższe czynności, aby zmienić hasło administratora urządzenia.

#### <a name="to-change-the-device-administrator-password"></a>Aby zmienić hasło administratora urządzenia

1. W portalu klasyczny kliknij pozycję **urządzenia** > **Konfiguruj** dla danego urządzenia.

2. Przewiń w dół do sekcji **Hasło administratora urządzenia** . Hasło administratora zawierający od 8 do 15 znaków. Hasło należy kombinacja 3 lub więcej znaków wielkie litery, małe litery, liczbowe i specjalnych.

3. Potwierdź hasło.

4. Kliknij pozycję **Zapisz** u dołu strony.

Teraz powinny być aktualizowane hasło administratora urządzenia. Za pomocą tego zmienione hasła dostępu do interfejsu programu Windows PowerShell.

## <a name="change-the-storsimple-snapshot-manager-password"></a>Zmienianie hasła Menedżer migawkę StorSimple

Oprogramowanie StorSimple migawkę Menedżera znajduje się na hoście systemu Windows i umożliwia administratorom zarządzanie kopie zapasowe urządzenia StorSimple w formularzu lokalnych i w chmurze migawek.

Podczas konfigurowania urządzenia w Menedżerze migawkę StorSimple, wyświetli się monit o Podaj adres IP urządzenia i hasło do uwierzytelnienia urządzenia miejsca do magazynowania. To hasło został skonfigurowany za pomocą interfejsu programu Windows PowerShell. Aby uzyskać więcej informacji, zobacz [Krok 3: Konfigurowanie i zarejestrować urządzenie przy użyciu programu Windows PowerShell dla StorSimple](storsimple-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

Następnie można zmienić hasło, którego ustawiono najpierw za pośrednictwem interfejsu programu Windows PowerShell podczas rejestrowania za pośrednictwem portalu klasyczny. Wykonaj poniższe czynności, aby zmienić hasło StorSimple migawkę menedżera.

#### <a name="to-change-the-storsimple-snapshot-manager-password"></a>Aby zmienić hasło Menedżer migawkę StorSimple

1. W portalu klasyczny kliknij pozycję **urządzenia** > **Konfiguruj** dla danego urządzenia.

2. Przewiń w dół do sekcji **StorSimple migawkę Manager** . Wprowadź hasło, 14 lub 15 znaków. Upewnij się, że hasło zawiera kombinacja 3 lub więcej znaków wielkie litery, małe litery, liczbowe i specjalnych.

3. Potwierdź hasło.

4. Kliknij pozycję **Zapisz** u dołu strony.

Teraz powinny być aktualizowane hasło StorSimple migawkę Manager.
 

## <a name="next-steps"></a>Następne kroki

- Dowiedz się więcej o [zabezpieczeniach StorSimple](storsimple-security.md).

- Dowiedz się więcej o [modyfikowaniu konfigurację urządzeń](storsimple-modify-device-config.md).

- Dowiedz się więcej o [korzystaniu z usługi Menedżera StorSimple administrowania urządzenia StorSimple](storsimple-manager-service-administration.md).
