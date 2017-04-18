<properties 
   pageTitle="Zarządzać swoim kontem miejsca do magazynowania StorSimple | Microsoft Azure"
   description="W tym miejscu wyjaśniono, jak można na stronie Konfigurowanie Menedżera StorSimple Dodawanie, edytowanie, usuwanie i obracanie kluczy zabezpieczeń konta magazynu skojarzone z tabeli wirtualnej StorSimple."
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
   ms.date="09/29/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-manage-storage-accounts-for-storsimple-virtual-array"></a>Zarządzanie kontami miejsca do magazynowania tablicy Virtual StorSimple za pomocą usługi Menedżera StorSimple

## <a name="overview"></a>Omówienie

Na stronie **Konfigurowanie** przedstawia parametry globalne usługi, utworzone w usłudze Menedżer StorSimple. Parametry te mogą być stosowane do wszystkich urządzeń podłączonych do usługi i obejmują:

- Konta miejsca do magazynowania 
- Rekordy kontrolki programu Access 

Ten samouczek wyjaśniono, jak umożliwia na stronie **Konfigurowanie** Dodawanie, edytowanie lub usuwanie konta miejsca do magazynowania dla macierzy Virtual StorSimple. Informacje zawarte w tym samouczku dotyczy tylko StorSimple tablicy wirtualnych oprogramowaniem wersji 2016 marca GA.

 ![Konfigurowanie strony](./media/storsimple-ova-manage-storage-accounts/configure_service_page.png)  

Konta magazynu zawierają poświadczenia używane urządzenie dostępu do konta miejsca do magazynowania z dostawcą usług w chmurze. W przypadku kont Microsoft Azure przestrzeni dyskowej są poświadczenia, takie jak nazwa konta i klucz podstawowy dostęp. 

Na stronie **Konfigurowanie** wszystkich kont miejsca do magazynowania, które są tworzone rozliczeń subskrypcji są wyświetlane w formacie tabelarycznym zawierający następujące informacje:

- **Nazwa** — unikatową nazwę przypisane do konta, jeśli został on utworzony.
- **Włączony protokół SSL** — czy SSL jest włączone, i jest urządzenia w chmurze komunikacja w bezpiecznym kanale.

Są najczęściej wykonywanych zadań związanych z kontami miejsca do magazynowania, które mogą być wykonywane na stronie **Konfigurowanie** :

- Dodawanie konta miejsca do magazynowania 
- Edytowanie konta miejsca do magazynowania 
- Usuwanie konta miejsca do magazynowania 


## <a name="types-of-storage-accounts"></a>Typy kont miejsca do magazynowania

Istnieją trzy typy kont miejsca do magazynowania, których można używać z urządzeniem StorSimple.

- **Generowane automatycznie magazynu kont** — jak wynika z nazwy, ten typ konta miejsca do magazynowania jest generowany automatycznie podczas tworzenia usługi. Aby dowiedzieć się więcej na temat tworzenia tego konta miejsca do magazynowania, zobacz [Tworzenie nowej usługi](storsimple-ova-manage-service.md#create-a-service). 
- **Konta miejsca do magazynowania w subskrypcji usługi** — są to konta Azure miejsca do magazynowania, które są skojarzone z tej samej subskrypcji, jak usługa. Aby uzyskać więcej informacji o tych przestrzeni dyskowej są tworzone konta, zobacz [Temat Azure miejsca do magazynowania konta](../storage/storage-create-storage-account.md). 
- **Kont miejsca do magazynowania poza subskrypcja usługi** — są to konta Azure miejsca do magazynowania, które nie są skojarzone z usługą i prawdopodobnie sprzed, usługa została utworzona.

Tablic wirtualnych StorSimple tworzy kontenera (z prefiksem moduł hcs) w oknie konta skojarzonego miejsca do magazynowania. Ten kontener zawiera wszystkie dane chmury dla danego urządzenia. Nie usuwaj tego kontenera przez dostęp za pośrednictwem usługi Magazyn Azure, jak ta akcja spowoduje utratę danych.

## <a name="add-a-storage-account"></a>Dodawanie konta miejsca do magazynowania

Możesz dodać konto miejsca do magazynowania do konfiguracji usługi Menedżera StorSimple, dostarczając unikatową przyjazną nazwę i poświadczenia dostępu, które są połączone z kontem miejsca do magazynowania. Masz również opcję włączenia tryb bezpieczny sockets layer (SSL), aby utworzyć bezpiecznego kanału komunikacji sieci między urządzeniem przenośnym a chmury.

Można utworzyć wiele kont dla dostawcy usług danego chmury. Przy zapisywaniu konta miejsca do magazynowania, usługa próbuje komunikować się z dostawcą usług w chmurze. Poświadczenia i materiały programu access, które podano będzie uwierzytelniony w tej chwili. Konto miejsca do magazynowania jest tworzone tylko wtedy, gdy uwierzytelnianie zakończyło się powodzeniem. Jeśli uwierzytelnianie nie powiedzie się, następnie pojawi się odpowiedni komunikat o błędzie.

Menedżer zasobów magazynowania kont utworzonych w Azure portal są również obsługiwane w StorSimple. Menedżer zasobów kont miejsca do magazynowania nie będą widoczne na liście rozwijanej zaznaczenie, przechowywania kont utworzonych w portalu klasyczny Azure będą wyświetlane. Menedżer zasobów magazynowania konta muszą można dodać za pomocą procedury, aby dodać konto miejsca do magazynowania w sposób opisany poniżej.

Procedura dodawania konta usługi Azure klasyczny magazynowania jest wyszczególnione poniżej.

[AZURE.INCLUDE [add-a-storage-account](../../includes/storsimple-ova-configure-new-storage-account.md)]

## <a name="edit-a-storage-account"></a>Edytowanie konta miejsca do magazynowania

Można edytować konta miejsca do magazynowania, używane przez urządzenia. Jeśli edytujesz konta miejsca do magazynowania, który jest obecnie używana dostępny do modyfikowania pola są klawisz dostępu i tryb protokołu SSL dla konta miejsca do magazynowania. Można podać nowy klucz miejsca do magazynowania w programie access lub modyfikowanie wybór **trybu Enable SSL** i zapisać zaktualizowane ustawienia.

#### <a name="to-edit-a-storage-account"></a>Aby edytować konto miejsca do magazynowania

1. Strona główna usługi wybierz usługi, kliknij dwukrotnie nazwę usługi i kliknij przycisk **Konfiguruj**.

2. Kliknij pozycję **Dodawanie/edytowanie kont miejsca do magazynowania**.

3. W oknie dialogowym **Dodawanie/edytowanie kont miejsca do magazynowania** :

  1. Na liście rozwijanej **Miejsca do magazynowania**kont wybierz istniejące konto, które chcesz zmodyfikować. 
  2. W razie potrzeby można zmodyfikować wybór **Włącz tryb SSL** .
  3. Możesz ponownie wygenerować kluczy dostępu do konta miejsca do magazynowania. Aby uzyskać więcej informacji zobacz [ponownie wygenerować klucze konta miejsca do magazynowania](storage-create-storage-account.md#manage-your-storage-access-keys). Wprowadź nowy klucz konta miejsca do magazynowania. Dla konta Azure miejsca do magazynowania jest klucz podstawowy dostęp. 
  4. Kliknij ikonę wyboru ![zaznacz ikony](./media/storsimple-ova-manage-storage-accounts/checkicon.png) Aby zapisać ustawienia. Ustawienia zostaną zaktualizowane na stronie **Konfigurowanie** . 
  5. U dołu strony kliknij przycisk **Zapisz** , aby zapisać zaktualizowane ustawienia. 

     ![Edytowanie konta miejsca do magazynowania](./media/storsimple-ova-manage-storage-accounts/modifyexistingstorageaccount.png)
  
## <a name="delete-a-storage-account"></a>Usuwanie konta miejsca do magazynowania

> [AZURE.IMPORTANT] Tylko wtedy, gdy nie jest używany, można usunąć konto miejsca do magazynowania. Jeśli konto miejsca do magazynowania jest używany, będą otrzymywać powiadomień.

#### <a name="to-delete-a-storage-account"></a>Aby usunąć konto miejsca do magazynowania

1. Na stronie Menedżera StorSimple usługi główna wybierz usługi, kliknij dwukrotnie nazwę usługi, a następnie kliknij **Konfiguruj**.

2. Na liście tabelaryczny kont miejsca do magazynowania umieść wskaźnik na konto, które chcesz usunąć.

3. Ikona Usuń (**x**) pojawi się w skrajnej prawej kolumnie dla tego konta miejsca do magazynowania. Kliknij ikonę **x** , aby usunąć poświadczenia.

4. Po wyświetleniu monitu o potwierdzenie, kliknij przycisk **Tak,** aby kontynuować usunięcie. Lista tabelaryczny zostanie zaktualizowana zgodnie ze zmianami.

5. U dołu strony kliknij przycisk **Zapisz** , aby zapisać zaktualizowane ustawienia.


## <a name="next-steps"></a>Następne kroki

- Dowiedz się, sposobu [administrowania macierzy Virtual StorSimple](storsimple-ova-web-ui-admin.md).
