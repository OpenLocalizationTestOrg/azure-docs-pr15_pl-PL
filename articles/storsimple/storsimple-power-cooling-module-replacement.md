<properties 
   pageTitle="Zamienianie PCM na urządzeniu StorSimple | Microsoft Azure"
   description="Wyjaśniono, jak usunąć i zamienić na urządzeniu StorSimple dodatku i chłodzenia Moduł PCM)"
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/18/2016"
   ms.author="alkohli" />

# <a name="replace-a-power-and-cooling-module-on-your-storsimple-device"></a>Zamienianie Power i chłodzenia modułu na urządzeniu StorSimple

## <a name="overview"></a>Omówienie

Power i chłodzenia Moduł PCM () w urządzeniu Microsoft Azure StorSimple składa się z zasilania i chłodzenia wentylatory, które są kontrolowane przez podstawowy i EBOD załączniki. Istnieje tylko jeden model PCM, który ma certyfikat dla każdego załącznik. Podstawowy załącznik ma certyfikat dla 764 W PCM i załącznik EBOD ma certyfikat dla 580 W PCM. Mimo że PCMs załącznik podstawowego i załącznik EBOD są różne, procedura zastąpienie jest taka sama.

Ten samouczek w tym miejscu wyjaśniono, jak:

- Usuwanie PCM
- Instalowanie zastąpienie PCM

>[AZURE.IMPORTANT] Przed usuwania i zamieniania PCM, przejrzyj informacje bezpieczeństwa w [Wymiana składników sprzętu StorSimple](storsimple-hardware-component-replacement.md).

## <a name="before-you-replace-a-pcm"></a>Aby zamienić PCM

Należy pamiętać o następujących istotnych problemów przed Zamień usługi PCM:

- Jeśli zasilania PCM kończy się niepowodzeniem, pozostaw zainstalowany moduł uszkodzony, ale usuwać kabel. Wentylator nadal będą otrzymywać power z załącznik i nadal chłodzenia pisane z wielkiej litery. Jeśli wentylator kończy się niepowodzeniem, PCM musi zostać zastąpiona natychmiast.

- Przed usunięciem PCM, rozłączyć power PCM przez wyłączenie przełącznikiem głównym (o ile istnieje) lub usuwając fizycznie kabel. Udostępnia ostrzeżenie z systemem, że zamknięcie power jest bezpośrednie.

- Upewnij się, że innych PCM działa dalszego działania przed zamianą PCM uszkodzony. Uszkodzony PCM musi zostać zastąpiona pełną funkcjonalność PCM tak szybko, jak to możliwe.

- Wymiana Moduł PCM trwa tylko kilka minut, ale należy wykonać w ciągu 10 minut usuwania nie powiodło się PCM, aby zapobiec przegrzania.

- Zauważ, że wymiana 764 moduły W PCM wysłanych z fabryki nie zawierać modułu akumulatora kopii zapasowej. Należy usunąć baterii z Twojej uszkodzony PCM i wstawić go w module zamienny przed wykonaniem zastąpić. Aby uzyskać więcej informacji, zobacz jak [usunąć i wstawić moduł baterii kopii zapasowej](storsimple-battery-replacement.md).


## <a name="remove-a-pcm"></a>Usuwanie PCM

Wykonaj te instrukcje, jeśli chcesz usunąć z urządzenia Microsoft Azure StorSimple dodatku i chłodzenia Moduł PCM ().

>[AZURE.NOTE] Przed usunięciem usługi PCM należy sprawdzić, czy są poprawne zastępczego (764 T dla podstawowego załącznik) lub 580 W dla załącznik EBOD.

#### <a name="to-remove-a-pcm"></a>Aby usunąć PCM

1. W portalu klasyczny Azure kliknij pozycję **urządzenia** > **Konserwacja** > **Stanu sprzętu**. Sprawdzanie stanu składniki PCM w obszarze **Współużytkowanych składników** do określenia, na które PCM nie powiodło się:

     - Jeśli zostało zakończone niepowodzeniem, zasilania w PCM 0, stan **Power podanych w PCM 0** będzie czerwony.

     - Jeśli zasilania w PCM 1 nie powiodła się, stan **Power podanych w PCM 1** będzie czerwony.

     - Jeśli wentylator w PCM 1 nie powiodła się, stan **chłodzenia 0 dla PCM 0** lub **1 chłodzenia dla PCM 0** będzie czerwony.

2. Znajdź PCM nie powiodło się z tyłu podstawowego załącznik. Jeśli używasz 8600 model identyfikowanie podstawowego załącznik sprawdzając wyświetlane na panelu przedniego wyświetlania LED numer identyfikacyjny systemu jednostek. Domyślne jednostki identyfikator wyświetlany na podstawowy załącznik jest **00**domyślne identyfikator jednostki wyświetlane na załącznik EBOD jest **01**. Poniższy diagram i tabeli opisano panelu przedniego wyświetlaczu LED.

    ![Identyfikator systemu na panelu przedniego OPS](./media/storsimple-power-cooling-module-replacement/IC740991.png)

     **Rysunek 1** Przednia panel urządzenia  

  	|Etykieta|Opis|
  	|:---|:-----------|
  	|1|Przycisk Wycisz|
  	|2|Power systemu|
  	|3|Moduł błędów|
  	|4|Błąd logicznych.|
  	|5|Wyświetl identyfikator jednostki|

3. Wskaźnik monitorowania LED z tyłu podstawowego załącznik można także do identyfikowania PCM uszkodzony. Zobacz poniższy diagram i tabeli, aby dowiedzieć się, jak za pomocą LED Znajdź PCM uszkodzony. Na przykład jeśli jest włączone LED odpowiadające **Wentylatora kończą się niepowodzeniem** , wentylator nie powiodło się. Analogicznie jeśli LED odpowiadające **AC błędów** jest włączone, zasilania nie powiodło się. 

    ![Tablica połączeń urządzenia PCM monitorowania wskaźnika LED](./media/storsimple-power-cooling-module-replacement/IC740992.png)

     **Rysunek 2** Tylnej strony PCM ze wskaźnikiem LED

  	|Etykieta|Opis|
  	|:---|:-----------|
  	|1|Błąd power AC|
  	|2|Błąd wentylatora|
  	|3|Uszkodzenia|
  	|4|PCM OK|
  	|5|Błąd power kontrolera domeny|
  	|6|Baterii prawidłowy|

4. Zapoznaj się z Poniższy diagram z tyłu urządzenia StorSimple, aby zlokalizować modułu PCM. PCM 0 jest po lewej stronie i PCM 1 jest po prawej stronie. Tabela poniżej opisano modułów.

     ![Tablica połączeń urządzenia podstawowego załącznik modułów](./media/storsimple-power-cooling-module-replacement/IC740994.png)

     **Rysunek 3** Tylnej strony urządzenia z wtyczki 

  	|Etykieta|Opis|
  	|:---|:-----------|
  	|1|PCM 0|
  	|2|PCM 1|
  	|3|Kontroler 0|
  	|4|Kontroler 1|

5. Wyłącz uszkodzony PCM i odłączyć kabel dostaw. Możesz teraz usunąć PCM.

6. Ujmij zatrzask i po stronie uchwyt PCM między kciuka i wskazującym, a ich ze sobą, aby otworzyć ten uchwyt zmieścić.

    ![Otwieranie uchwyt PCM](./media/storsimple-power-cooling-module-replacement/IC740995.png)

    **Rysunek 4** Otwieranie uchwytu PCM

7. Wygodne do trzymania uchwytu, a następnie usuń PCM.

    ![Usuwanie urządzenia PCM](./media/storsimple-power-cooling-module-replacement/IC740996.png)

    **Rysunek 5** Usuwanie PCM

## <a name="install-a-replacement-pcm"></a>Instalowanie zastąpienie PCM

Wykonaj te instrukcje, aby zainstalować PCM w urządzeniu StorSimple. Upewnij się, umieszczono modułu akumulatora kopii zapasowej, przed zainstalowaniem zamienny PCM (dotyczy tylko 764 PCMs W). Aby uzyskać więcej informacji, zobacz jak [usunąć i wstawić moduł baterii kopii zapasowej](storsimple-battery-replacement.md).

#### <a name="to-install-a-pcm"></a>Aby zainstalować PCM

1. Upewnij się, że masz poprawne przyklejanych PCM ten załącznik. Podstawowy załącznik musi 764 W PCM i załącznik EBOD musi 580 W PCM. Nie należy próbować za pomocą 580 PCM W w podstawowego załącznik lub 764 PCM W w załącznik EBOD. Na poniższej ilustracji przedstawiono lokalizację do identyfikowania informacji na etykiecie, która jest umieszczony PCM.

    ![Etykieta PCM urządzenia](./media/storsimple-power-cooling-module-replacement/IC740973.png)

    **Rysunek 6** Etykieta PCM

2. Sprawdź, czy uszkodzenie załącznik, zwracając szczególną uwagę łączników. 
                                        
    >[AZURE.NOTE] **Nie można zainstalować moduł Jeśli zgięte PIN dowolny łącznik.**

3. Przy użyciu uchwytu PCM w pozycji otwartej Przesuń modułu do załącznik.

    ![Instalowanie urządzenia PCM](./media/storsimple-power-cooling-module-replacement/IC740975.png)

    **Rysunek 7** Instalowanie PCM

4. Ręcznie Zamknij uchwytu PCM. Kliknij pozycję powinno być słychać jako uczestniczy zatrzaśnięć uchwyt. 
                                        
    >[AZURE.NOTE] Aby upewnić się, że PIN łącznik jest włączone, lekko można holownik uchwyt przytrzymując zatrzask. Slajdy PCM się oznacza, że zatrzask została zamknięta przed uwagi łączników.

5. Podłącz kable power zasilania i PCM.

6. Bezpieczny szczepu bele zwolnienia. 

7. Włącz PCM.

8. Sprawdź, czy zastąpić zakończyło się pomyślnie: w Azure klasyczny portalu usługi Menedżera StorSimple, przejdź do **urządzenia** > **Konserwacja** > **Stanu sprzętu**. W obszarze **Współużytkowanych składników**stanu PCM powinny być zielone. 
                                        
    >[AZURE.NOTE] Może potrwać kilka minut na zastąpienie PCM całkowicie zainicjować.

## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej na temat [Wymiana składników sprzętu StorSimple](storsimple-hardware-component-replacement.md).
