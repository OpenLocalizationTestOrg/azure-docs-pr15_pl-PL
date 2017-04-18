<properties 
   pageTitle="Zarządzania rekordami kontrolki programu access w tabeli wirtualnej StorSimple | Microsoft Azure"
   description="Informacje dotyczące zarządzania rekordami kontroli dostępu (ACRs) do określenia, które hosty można nawiązać woluminu na tablicy Virtual StorSimple."
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
   ms.date="05/03/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-manage-access-control-records-for-the-storsimple-virtual-array"></a>Za pomocą usługi StorSimple Manager do zarządzania rekordami kontroli dostępu dla macierzy Virtual StorSimple 

## <a name="overview"></a>Omówienie

Rekordy kontrolki programu Access (ACRs) umożliwiają określenie, które hosty można nawiązać woluminu na tablicy Virtual StorSimple (nazywane także StorSimple lokalnej wirtualną urządzenia). ACRs są ustawione na określonej objętości i zawierać kwalifikowaną nazwy iSCSI (nazw IQN) hostów. Jeśli host próbuje nawiązać woluminu, urządzenie sprawdza awaryjnego skojarzone z tym głośność nazwa IQN, i w przypadku dopasowanie, połączenie jest nawiązywane. W sekcji **rekordów kontrolki programu access** na stronie **Konfigurowanie** Wyświetla wszystkie rekordy sterowania dostęp z odpowiednich nazw IQN hostów.

Ten samouczek opisano następujące typowe zadania związane z awaryjnego.

- Uzyskiwanie IQN
- Dodawanie rekordu kontroli dostępu 
- Edytuj rekord kontroli dostępu 
- Usuwanie rekordów kontroli dostępu 

> [AZURE.IMPORTANT] 
> 
> - Przypisując awaryjnego woluminu, zwrócić uwagę, że głośność nie jest jednocześnie dostępna przez więcej niż jednego hosta grupowany ponieważ to może spowodować uszkodzenie głośność. 
> - Podczas usuwania awaryjnego z woluminu, upewnij się, że odpowiednie hosta nie korzysta wielkość ponieważ usunięcie może spowodować zakłócenia odczytu i zapisu.

## <a name="get-the-iqn"></a>Uzyskiwanie IQN

Wykonaj poniższe czynności, aby uzyskać IQN hosta systemu Windows z systemem Windows Server 2012.

[AZURE.INCLUDE [storsimple-get-iqn](../../includes/storsimple-get-iqn.md)]

## <a name="add-an-acr"></a>Dodawanie awaryjnego

Strona **Konfiguracja** usługi StorSimple Manager umożliwia dodawanie ACRs. Zazwyczaj jeden awaryjnego zostanie skojarzony z jednego woluminu.

Aby dowiedzieć się, jak kojarzenie awaryjnego z woluminu przejdź do [artykułu Dodawanie woluminu](storsimple-ova-deploy3-iscsi-setup.md#step-3-add-a-volume).

>[AZURE.IMPORTANT] 
> 
>Przypisując awaryjnego woluminu, zwrócić uwagę, że głośność nie jest jednocześnie dostępna przez więcej niż jednego hosta grupowany ponieważ to może spowodować uszkodzenie głośność.
 
Wykonaj poniższe czynności, aby dodać awaryjnego.

#### <a name="to-add-an-acr"></a>Aby dodać awaryjnego

1. Strona główna usługi wybierz usługi, kliknij dwukrotnie nazwę usługi, a następnie kliknij kartę **konfiguracji** .

    ![Karta Konfiguracja](./media/storsimple-ova-manage-acrs/acr1.png)

2. Na liście tabelarycznego w obszarze **rekordy kontroli dostępu**, wprowadź **nazwę** dla swojego awaryjnego.

3. W obszarze **Nazwa inicjator iSCSI**należy podać nazwę IQN hosta systemu Windows. 

4. Kliknij pozycję **Zapisz** u dołu strony zapisać nowo utworzony awaryjnego. Zobaczysz następujący komunikat z potwierdzeniem.

    ![komunikat z potwierdzeniem](./media/storsimple-ova-manage-acrs/acr2.png)

5. Kliknij ikonę wyboru ![Zaznacz ikony](./media/storsimple-ova-manage-acrs/check-icon.png). Lista tabelaryczny zostaną zaktualizowane, aby odzwierciedlała to dodawanie.

## <a name="edit-an-acr"></a>Edytowanie awaryjnego

Aby edytować ACRs za pomocą strona **Konfiguracja** w portalu klasyczny Azure. 

> [AZURE.NOTE] Należy zmodyfikować tylko ACRs, które są obecnie używane. Aby edytować awaryjnego skojarzone z woluminu, który jest obecnie używana, należy najpierw podjąć głośności w trybie offline.

Wykonaj poniższe czynności, aby edytować awaryjnego.

#### <a name="to-edit-an-acr"></a>Aby edytować awaryjnego

1. Strona główna usługi wybierz usługi, kliknij dwukrotnie nazwę usługi, a następnie kliknij kartę **konfiguracji** .

2. Na liście tabelaryczny rekordów kontrolki programu access, umieść wskaźnik na awaryjnego, który chcesz zmodyfikować.

3. Podaj nową nazwę i/lub IQN dla awaryjnego.

4. Kliknij pozycję **Zapisz** u dołu strony do zapisania zmodyfikowanego awaryjnego. Zostanie wyświetlony komunikat potwierdzenia. 

5. Kliknij ikonę wyboru ![Zaznacz ikony](./media/storsimple-ova-manage-acrs/check-icon.png). Liście tabelaryczny zostaną zaktualizowane w celu odzwierciedlenia tej zmiany.

## <a name="delete-an-access-control-record"></a>Usuwanie rekordów kontroli dostępu

Aby usunąć ACRs za pomocą strony **konfiguracji** w portalu klasyczny Azure. 

> [AZURE.NOTE] 
> 
> - Należy usunąć tylko ACRs, które są obecnie używane. Aby usunąć awaryjnego skojarzone z woluminu, który jest obecnie używana, należy najpierw podjąć głośności w trybie offline.
> - Podczas usuwania awaryjnego z woluminu, upewnij się, że odpowiednie hosta nie korzysta wielkość ponieważ usunięcie może spowodować zakłócenia odczytu i zapisu.

Wykonaj poniższe czynności, aby usunąć rekord kontroli dostępu.

#### <a name="to-delete-an-access-control-record"></a>Aby usunąć rekord kontroli dostępu

1. Strona główna usługi wybierz usługi, kliknij dwukrotnie nazwę usługi, a następnie kliknij kartę **konfiguracji** .

2. Na liście tabelaryczny rekordów kontrolki programu access (ACRs), umieść wskaźnik na awaryjnego, który chcesz usunąć.

3. Ikona Usuń (**x**) pojawią się w skrajnej prawej kolumnie awaryjnego, wybrana. Kliknij ikonę **x** , aby usunąć awaryjnego. Zobaczysz następujący komunikat z potwierdzeniem.

    ![komunikat z potwierdzeniem](./media/storsimple-ova-manage-acrs/acr3.png)

5. Kliknij ikonę wyboru ![Zaznacz ikony](./media/storsimple-ova-manage-acrs/check-icon.png). Lista tabelaryczny zostaną zaktualizowane, aby odzwierciedlała usunięcie.

## <a name="next-steps"></a>Następne kroki

- Dowiedz się więcej na temat [dodawania wielkości i konfigurowania ACRs](storsimple-ova-deploy3-iscsi-setup.md#step-3-add-a-volume).
