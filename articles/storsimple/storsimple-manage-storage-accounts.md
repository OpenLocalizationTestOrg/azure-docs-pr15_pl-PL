<properties 
   pageTitle="Zarządzać swoim kontem miejsca do magazynowania StorSimple | Microsoft Azure"
   description="W tym artykule wyjaśniono, jak można strona Konfigurowanie StorSimple Manager umożliwia dodawanie, edytowanie i usuwanie lub obracanie kluczy zabezpieczeń konta miejsca do magazynowania."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="04/29/2016"
   ms.author="v-sharos" />

# <a name="use-the-storsimple-manager-service-to-manage-your-storage-account"></a>Aby zarządzać swoim kontem miejsca do magazynowania jest używana usługa Menedżer StorSimple

## <a name="overview"></a>Omówienie

Na stronie **Konfigurowanie** przedstawia wszystkie parametry globalne usługi utworzone w usłudze Menedżer StorSimple. Parametry te mogą być stosowane do wszystkich urządzeń podłączonych do usługi i obejmują:

- Konta miejsca do magazynowania 
- Szablony przepustowości 
- Rekordy kontrolki programu Access 

Ten samouczek wyjaśniono, jak korzystać z na stronie **Konfigurowanie** Dodawanie, edytowanie lub usuwanie kont miejsca do magazynowania i obracanie kluczy zabezpieczeń konta miejsca do magazynowania.

 ![Konfigurowanie strony](./media/storsimple-manage-storage-accounts/HCS_ConfigureService.png)  

Konta magazynu zawierają poświadczenia używane urządzenie dostępu do konta miejsca do magazynowania z dostawcą usług w chmurze. W przypadku kont Microsoft Azure przestrzeni dyskowej są poświadczenia, takie jak nazwa konta i klucz podstawowy dostęp. 

Na stronie **Konfigurowanie** wszystkich kont miejsca do magazynowania, które są tworzone rozliczeń subskrypcji są wyświetlane w formacie tabelarycznym zawierający następujące informacje:

- **Nazwa** — unikatowa nazwa przypisana do konta, jeśli został on utworzony.
- **Włączony protokół SSL** — czy SSL jest włączone, i jest urządzenia w chmurze komunikacja w bezpiecznym kanale.
- **Używane przez** — objętości przy użyciu konta miejsca do magazynowania.

Są najczęściej wykonywanych zadań związanych z kontami miejsca do magazynowania, które mogą być wykonywane na stronie **Konfigurowanie** :

- Dodawanie konta miejsca do magazynowania 
- Edytowanie konta miejsca do magazynowania 
- Usuwanie konta miejsca do magazynowania 
- Kluczowe obrotu kont miejsca do magazynowania 

## <a name="types-of-storage-accounts"></a>Typy kont miejsca do magazynowania

Istnieją trzy typy kont miejsca do magazynowania, których można używać z urządzeniem StorSimple.

- **Generowane automatycznie magazynu kont** — jak wynika z nazwy, ten typ konta miejsca do magazynowania jest generowany automatycznie podczas tworzenia usługi. Aby uzyskać więcej informacji na temat tworzenia tego konta miejsca do magazynowania, zobacz [Krok 1: Tworzenie nowej usługi](storsimple-deployment-walkthrough-u1.md#step-1-create-a-new-service) w [Rozmieszczanie urządzenia StorSimple lokalnego](storsimple-deployment-walkthrough.md). 
- **Konta miejsca do magazynowania w subskrypcji usługi** — są to konta Azure miejsca do magazynowania, które są skojarzone z tej samej subskrypcji, jak usługa. Aby uzyskać więcej informacji o tych przestrzeni dyskowej są tworzone konta, zobacz [Temat Azure miejsca do magazynowania konta](../storage/storage-create-storage-account.md). 
- **Kont miejsca do magazynowania poza subskrypcja usługi** — są to konta Azure miejsca do magazynowania, które nie są skojarzone z usługą i prawdopodobnie sprzed, usługa została utworzona.

## <a name="add-a-storage-account"></a>Dodawanie konta miejsca do magazynowania

Można dodać konto miejsca do magazynowania, podając unikatową przyjazną nazwę i poświadczenia dostępu, które są połączone z kontem miejsca do magazynowania (z określonej chmurze usługodawcy). Masz również opcję włączenia tryb bezpieczny sockets layer (SSL), aby utworzyć bezpiecznego kanału komunikacji sieci między urządzeniem przenośnym a chmury.

Można utworzyć wiele kont dla dostawcy usług danego chmury. Należy jednak pamiętać, że po utworzeniu konta miejsca do magazynowania, nie można zmienić dostawcę usługi w chmurze.

Przy zapisywaniu konta miejsca do magazynowania, usługa próbuje komunikować się z dostawcą usług w chmurze. Poświadczenia i materiały programu access, które podano będzie uwierzytelniony w tej chwili. Konto miejsca do magazynowania jest tworzone tylko wtedy, gdy uwierzytelnianie zakończyło się powodzeniem. Jeśli uwierzytelnianie nie powiedzie się, następnie pojawi się odpowiedni komunikat o błędzie.

Menedżer zasobów magazynowania kont utworzonych w Azure portal są również obsługiwane w StorSimple. Menedżer zasobów kont miejsca do magazynowania nie pojawi się na liście rozwijanej wyboru podczas próby utworzenia kontenera głośność, tylko konta miejsca do magazynowania utworzony w portalu klasyczny Azure będą wyświetlane. Menedżer zasobów magazynowania konta muszą można dodać za pomocą procedury w celu dodania miejsca do magazynowania opisane poniżej.

> [AZURE.NOTE] Procedura dodawania konta miejsca do magazynowania różni się w zależności od używanej wersji oprogramowania StorSimple. Pamiętaj wykonać poprawną procedurę dla używanej wersji StorSimple.


[AZURE.INCLUDE [add-a-storage-account-update1](../../includes/storsimple-configure-new-storage-account-u1.md)]

[AZURE.INCLUDE [add-a-storage-account](../../includes/storsimple-configure-new-storage-account.md)]

## <a name="edit-a-storage-account"></a>Edytowanie konta miejsca do magazynowania

Można edytować konta miejsca do magazynowania, które są używane przez kontenera głośność. Jeśli edytujesz konta miejsca do magazynowania, który jest obecnie używana tylko pola dostępny do modyfikowania jest klucz dostępu do konta miejsca do magazynowania. Można podać nowy klucz miejsca do magazynowania w programie access i zapisać zaktualizowanych ustawień.

#### <a name="to-edit-a-storage-account"></a>Aby edytować konto miejsca do magazynowania

1. Strona główna usługi wybierz usługi, kliknij dwukrotnie nazwę usługi i kliknij przycisk **Konfiguruj**.

2. Kliknij pozycję **Dodawanie/edytowanie kont miejsca do magazynowania**.

3. W oknie dialogowym **Dodawanie/edytowanie kont miejsca do magazynowania** :

  1. Na liście rozwijanej **Miejsca do magazynowania**kont wybierz istniejące konto, które chcesz zmodyfikować. Może to być także konta miejsca do magazynowania, które zostały generowane automatycznie, gdy usługa najpierw została utworzona.
  2. W razie potrzeby można zmodyfikować wybór **Włącz tryb SSL** .
  3. Możesz obrócić kluczy dostępu do konta miejsca do magazynowania. Zobacz [Obrót klucz konta miejsca do magazynowania](#key-rotation-of-storage-accounts) , aby dowiedzieć się, jak wykonywać kluczowe obrotu.
  4. Kliknij ikonę wyboru ![zaznacz ikony](./media/storsimple-manage-storage-accounts/HCS_CheckIcon.png) Aby zapisać ustawienia. Ustawienia zostaną zaktualizowane na stronie **Konfigurowanie** . Kliknij przycisk **Zapisz** , aby zapisać zaktualizowane ustawienia.

     ![Edytowanie konta miejsca do magazynowania](./media/storsimple-manage-storage-accounts/HCs_AddEditStorageAccount.png)
  
## <a name="delete-a-storage-account"></a>Usuwanie konta miejsca do magazynowania

> [AZURE.IMPORTANT] Konto miejsca do magazynowania możesz usunąć tylko wtedy, gdy nie jest używana przez kontenera głośność. Jeśli konto miejsca do magazynowania jest używany przez kontenera głośność, najpierw usuń kontener głośności, a następnie usuń konto skojarzone miejsca do magazynowania.

#### <a name="to-delete-a-storage-account"></a>Aby usunąć konto miejsca do magazynowania

1. Na stronie Menedżera StorSimple usługi główna wybierz usługi, kliknij dwukrotnie nazwę usługi, a następnie kliknij **Konfiguruj**.

2. Na liście tabelaryczny kont miejsca do magazynowania umieść wskaźnik na konto, które chcesz usunąć.

3. Ikona Usuń (**x**) pojawi się w skrajnej prawej kolumnie dla tego konta miejsca do magazynowania. Kliknij ikonę **x** , aby usunąć poświadczenia.

4. Po wyświetleniu monitu o potwierdzenie, kliknij przycisk **Tak,** aby kontynuować usunięcie. Lista tabelaryczny zostanie zaktualizowana zgodnie ze zmianami.

## <a name="key-rotation-of-storage-accounts"></a>Kluczowe obrotu kont miejsca do magazynowania

Ze względów bezpieczeństwa klucza obrotu często jest wymagane w centrach danych. 

> [AZURE.NOTE] Następujące informacje kluczowe obrót i procedury obrót dotyczą tylko konta Microsoft Azure miejsca do magazynowania. Jeśli korzystasz z innego dostawcy usługi w chmurze, możesz zarządzać klawiszy konta miejsca do magazynowania za pośrednictwem tego dostawcy pulpitu nawigacyjnego.
 
Każdej subskrypcji Microsoft Azure może mieć co najmniej jednego konta skojarzonego miejsca do magazynowania. Dostęp do tych kont steruje klucze subskrypcji i dostęp dla każdego konta miejsca do magazynowania. 

Po utworzeniu konta magazynu Microsoft Azure generuje dwa miejsca do magazynowania 512-bitowej klawisze dostępu, które służą do uwierzytelniania podczas uzyskiwania dostępu do konta miejsca do magazynowania. O dwóch klawiszy dostępu miejsca do magazynowania umożliwia ponownie wygenerować klucze nie przerwy w usłudze miejsca do magazynowania lub dostęp do tej usługi. Klucz, który jest obecnie używana jest klucz *podstawowy* i klucz kopii zapasowej nosi nazwę klucza *pomocniczego* . Gdy urządzenia Microsoft Azure StorSimple uzyskuje dostęp do miejsca do magazynowania chmury usługodawcy należy podać jednego z tych dwóch kluczy.

## <a name="what-is-key-rotation"></a>Co to jest klucza obrotu?

Zazwyczaj aplikacje umożliwia tylko jeden z klawiszy dostępu do danych. Po upływie pewnego czasu możesz mieć aplikacji przełączyć się przy użyciu drugiego klucza. Po przełączeniu aplikacji do klucza pomocniczego, możesz Anuluj pierwszy klawisz, a następnie wygeneruj nowy klucz. Za pomocą dwóch klawiszy w ten sposób umożliwia aplikacji dostępu do danych bez konieczności dowolnego przestoje.

Klucze konta miejsca do magazynowania zawsze są przechowywane w usłudze zaszyfrowane. Te można jednak zresetować za pośrednictwem usługi Menedżera StorSimple. Usługa uzyskać klucz podstawowy i pomocniczy klucz dla wszystkich kont miejsca do magazynowania w tej samej subskrypcji, łącznie z kontami utworzone w Usługa magazynu, a także na przechowywanie domyślne utworzonej wygenerowane podczas pierwszego usługi StorSimple Menedżer kont. Usługa Menedżer StorSimple będzie zawsze pobiera klucze z portalem klasyczny Azure i przechowywać je w sposób zaszyfrowane.

## <a name="rotation-workflow"></a>Obrót przepływu pracy

Administrator usługi Microsoft Azure można odtworzyć lub zmienić klucz podstawowy i pomocniczy bezpośredni dostęp do konta miejsca do magazynowania (przy użyciu usługi Microsoft Azure magazynowania). Usługa Menedżera StorSimple niewidoczne ta zmiana automatycznie.

Informowanie usługę Menedżer StorSimple zmiany, możesz będzie potrzebny dostęp do usługi Menedżer StorSimple dostępu do konta miejsca do magazynowania, a następnie zsynchronizować klucz podstawowy i pomocniczy (w zależności od nich został zmieniony). Następnie usługa otrzymuje najnowszą klucza, są szyfrowane kluczy i wysyła zaszyfrowany klucz do urządzenia.

#### <a name="to-synchronize-keys-for-storage-accounts-in-the-same-subscription-as-the-service-azure-only"></a>Aby zsynchronizować klawiszy dla kont miejsca do magazynowania w tej samej subskrypcji usługi (tylko Azure)

1. Na stronie **usługi** kliknij kartę **Konfiguruj** .

2. Kliknij pozycję **Dodawanie/edytowanie kont miejsca do magazynowania**.

3. W oknie dialogowym wykonaj następujące czynności:

  1. Wybierz konto, miejsca do magazynowania przy użyciu klucza, która ma być synchronizowane. Klucze konta miejsca do magazynowania są szyfrowane, gdy są one wyświetlane.
  2. W usłudze Menedżer StorSimple musisz zaktualizować klucz, który wcześniej został zmieniony w usłudze Microsoft Azure miejsca do magazynowania. Jeśli klucz podstawowy dostęp został zmieniony (regenerowanej), kliknij przycisk **Synchronizuj klucza podstawowego**. Jeśli klucza pomocniczego został zmieniony, kliknij przycisk **Synchronizuj klucza pomocniczego**.

    ![Synchronizowanie klawiszy](./media/storsimple-manage-storage-accounts/HCS_KeyRotationStorageAccountSameSubscriptionAsService.png)

#### <a name="to-synchronize-keys-for-storage-accounts-outside-of-the-service-subscription"></a>Aby zsynchronizować klawiszy dla kont miejsca do magazynowania poza subskrypcja usługi

1. Na stronie **usługi** kliknij kartę **Konfiguruj** .

2. Kliknij pozycję **Dodawanie/edytowanie kont miejsca do magazynowania**.

3. W oknie dialogowym wykonaj następujące czynności:

  1. Wybierz konto, miejsca do magazynowania z kluczem dostępu, który chcesz zaktualizować.
  2. Należy zaktualizować kod dostępu miejsca do magazynowania w usłudze Menedżer StorSimple. W tym przypadku widać klawisz dostępu miejsca do magazynowania. Wprowadź nowy klucz w polu **Klawisz dostępu do konta miejsca do magazynowania**y. 
  3. Zapisz wprowadzone zmiany. Teraz powinny być aktualizowane klucz dostępu do konta miejsca do magazynowania.

## <a name="next-steps"></a>Następne kroki

- Dowiedz się więcej o [zabezpieczeniach StorSimple](storsimple-security.md).
- Dowiedz się więcej o [korzystaniu z usługi Menedżera StorSimple administrowania urządzenia StorSimple](storsimple-manager-service-administration.md).
