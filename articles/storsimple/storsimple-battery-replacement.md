<properties 
   pageTitle="Wymiana baterii na urządzeniu StorSimple | Microsoft Azure"
   description="W tym artykule opisano, jak usuwanie, zamienianie i obsługa modułu baterii kopii zapasowej na urządzeniu StorSimple."
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
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="replace-the-backup-battery-module-on-your-storsimple-device"></a>Zastąpienie modułu baterii kopii zapasowej na urządzeniu StorSimple

## <a name="overview"></a>Omówienie

Podstawowy załącznik Power i chłodzenia Moduł PCM () na urządzeniu z systemem Microsoft Azure StorSimple ma pakiet akumulator dodatkowy. Ten pakiet zawiera zasilania tak, aby urządzenia StorSimple można zapisać dane w przypadku utraty możliwości AC podstawowego załącznik. Ten pakiet baterii nosi nazwę *modułu akumulatora kopii zapasowej*. Moduł baterią zapasową istnieje tylko w przypadku podstawowego załącznik w urządzeniu StorSimple (załącznik EBOD nie zawiera moduł kopii zapasowej baterii). 

Ten samouczek w tym miejscu wyjaśniono, jak:

- Usuwanie modułu akumulatora kopii zapasowej 
- Instalowanie nowego modułu akumulatora kopii zapasowej
- Obsługa modułu akumulatora kopii zapasowej

>[AZURE.IMPORTANT] Przed usuwania i zamieniania moduł baterii kopii zapasowej, należy zapoznać się z informacjami bezpieczeństwa [Wprowadzenie do wymiana składników sprzętu StorSimple](storsimple-hardware-component-replacement.md).

## <a name="remove-the-backup-battery-module"></a>Usuwanie modułu akumulatora kopii zapasowej

Moduł baterią zapasową dla swojego urządzenia StorSimple to pole-zamiennych. Przed zainstalowaniem w PCM modułu akumulatora powinny być przechowywane w oryginalnym opakowaniu. Wykonaj poniższe czynności, aby usunąć baterii kopii zapasowej.

#### <a name="to-remove-the-backup-battery-module"></a>Aby usunąć modułu akumulatora kopii zapasowej

1. W portalu klasyczny Azure, przejdź do pozycji **urządzenia** > **Konserwacja** > **Stanu sprzętu**. W obszarze **Współużytkowanych składników**odszukaj stan baterii.

2. Identyfikowanie PCM, w którym bateria nie powiodło się. Rysunek 1 pokazuje z tyłu urządzenia StorSimple.

    ![Tablica połączeń urządzenia podstawowego załącznik modułów](./media/storsimple-battery-replacement/IC740994.png)

    **Rysunek 1** Tylnej strony z modułach PCM i kontroler urządzenie podstawowe

  	|Etykieta|Opis|
  	|:----|:----------|
  	|1|PCM 0|
  	|2|PCM 1|
  	|3|Kontroler 0|
  	|4|Kontroler 1|

    Wskazywanych przez liczbę 3 na rysunku 2 monitorowania wskaźnik LED na PCM 0, która odpowiada na **Uszkodzenia** powinny być włączone.

    ![Tablica połączeń z urządzenia PCM monitorowania wskaźnik LED](./media/storsimple-battery-replacement/IC740992.png)

    **Rysunek 2** Tylnej strony PCM przedstawiający monitorowania wskaźnik LED

  	|Etykieta|Opis|
  	|:---|:-----------|
  	|1|Błąd power AC|
  	|2|Błąd wentylatora|
  	|3|Uszkodzenia|
  	|4|PCM OK|
  	|5|Błąd power kontrolera domeny|
  	|6|Baterii prawidłowy|

3. Aby usunąć PCM z uszkodzonej baterii, wykonaj czynności opisane w [Usuwanie PCM](storsimple-power-cooling-module-replacement.md#remove-a-pcm).

4. Za pomocą konwertera PCM usunięte Podnieś baterii modułu uchwyt obrotu w górę wskazywane na poniższym rysunku i pobrania ich do Usuń baterii.

    ![Usuwanie baterii z PCM](./media/storsimple-battery-replacement/IC741019.png)

    **Rysunek 3** Usuwanie baterii z PCM

5. Umieść moduł w jednostce zastąpienia pole pakowania.

6. Zwraca wadliwy jednostkę do firmy Microsoft dla obsługi i obsługi pisane z wielkiej litery.

## <a name="install-a-new-backup-battery-module"></a>Instalowanie nowego modułu akumulatora kopii zapasowej

Wykonaj poniższe czynności, aby zainstalować moduł baterii zastępczego w PCM w załącznik podstawowego urządzenia StorSimple.

#### <a name="to-install-the-battery-module"></a>Aby zainstalować moduł baterii

1. Zaznacz odpowiednią orientację w PCM modułu akumulatora kopii zapasowej.

2. Naciśnij uchwyt modułu baterii maksymalnie miejsca łącznik.

3. Zamień PCM w podstawowego załącznik, zgodnie z wytycznymi zawartymi w polu [Zamień dodatku i chłodzenia modułu na urządzeniu StorSimple](storsimple-power-cooling-module-replacement.md).

4. Po zakończeniu zastąpienie przejdź do pozycji **urządzenia** > **Konserwacja** > **Stanu sprzętu** w portalu klasyczny Azure. Sprawdź stan do upewnij się, że instalacja zakończyła się pomyślnie poziomu. Zielony stan wskazuje, że bateria jest uszkodzony.

## <a name="maintain-the-backup-battery-module"></a>Obsługa modułu akumulatora kopii zapasowej

W urządzeniu StorSimple moduł kopii zapasowej baterii zapewnia możliwości kontroler podczas zdarzeń utraty power. Umożliwia zapisywanie ważnych danych przed zamknięciem w sposób kontroli na tym urządzeniu StorSimple. Z dwóch baterii naładowaną w PCMs systemu mogą obsługiwać dwa kolejne utratę zdarzenia.

W portalu klasyczny Azure **Stanu sprzętu** na stronie **Obsługa** wskazuje, czy bateria działa poprawnie, czy zbliża się zakończenia życia. Stan baterii jest oznaczany **baterii w PCM 0** lub **baterii w PCM 1** w obszarze **Współużytkowanych składników**. Ta strona zostanie wyświetlona Państwie **OBNIŻONY** na końcu życia zbliżenie się do, a **nie powiodło się** dla wycofanych z Tobą. 

>[AZURE.NOTE] Bateria zgłosić **nie powiodła się** podczas należy po prostu naliczane.
 
Jeśli zostanie wyświetlony stan **OBNIŻONY** , zaleca się kursu następujące działania:

- System może wystąpić ostatnie utratę zasilania lub baterie może być przeprowadzana okresowa Obsługa. Obserwować systemu na 12 godzin przed kontynuowaniem.

    - Jeśli stan jest nadal **OBNIŻONY** po 12 godzin stałego połączenia do potęgi AC z kontrolerami i PCMs uruchomiony, akumulator musi zostać zastąpiona. Sprawdź, [skontaktuj się z pomocą techniczną firmy Microsoft](storsimple-contact-microsoft-support.md) , aby moduł baterią zapasową zamienny.

    - Jeśli OK po 12 godzin staje się województwo, działa baterii i tylko potrzebne opłatę konserwacji.

- Jeśli nie jest skojarzone utratę zasilania Sieciowego i PCM jest włączony i podłączony do potęgi AC, bateria musi zostać zastąpiona. [Kontakt z pomocą techniczną firmy Microsoft](storsimple-contact-microsoft-support.md) dla zamówień moduł baterią zapasową zamienny.

>[AZURE.IMPORTANT] Dysponować uszkodzonej baterii zgodnie z przepisami narodowych i regionalnych. 

## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej na temat [Wymiana składników sprzętu StorSimple](storsimple-hardware-component-replacement.md).
