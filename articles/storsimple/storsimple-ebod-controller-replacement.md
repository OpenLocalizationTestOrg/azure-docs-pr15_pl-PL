<properties 
   pageTitle="Zamienianie kontroler StorSimple EBOD | Microsoft Azure"
   description="W tym miejscu wyjaśniono, jak usunąć i zamienić co najmniej obu kontroler EBOD na urządzeniu StorSimple 8600."
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

# <a name="replace-an-ebod-controller-on-your-storsimple-device"></a>Zamienianie kontroler EBOD na urządzeniu StorSimple

## <a name="overview"></a>Omówienie

Ten samouczek wyjaśniono, jak zastąpić uszkodzony moduł kontroler EBOD na urządzeniu z systemem Microsoft Azure StorSimple. Aby zamienić moduł kontrolera EBOD, należy:

- Usunąć uszkodzony kontroler EBOD
- Instalowanie nowego kontrolera EBOD

Przed rozpoczęciem należy rozważyć następujące informacje:

- Pusty moduły EBOD musi znajdować się na wszystkie nieużywane gniazda. Załącznik nie zostanie poprawnie ostudzić, jeśli przedział pozostanie otwarte.

- Kontroler EBOD jest wyłączania i można usunąć lub zastąpić. Nie usuwaj modułu nie powiodło się, dopóki nie zostaną dodane zamiennik. Po rozpoczęciu procesu zamienny musi zakończyć ją w ciągu 10 minut.

>[AZURE.IMPORTANT] Przed podjęciem próby usunięcia lub zastąpienia każdego składnika StorSimple, upewnij się, możesz przejrzeć [Konwencje ikona bezpieczeństwa](storsimple-safety.md#safety-icon-conventions) i inne [środki ostrożności](storsimple-safety.md).

## <a name="remove-an-ebod-controller"></a>Usuwanie kontrolera EBOD

Przed zamianą modułu kontroler EBOD w urządzeniu StorSimple, upewnij się, że inny moduł kontroler EBOD jest aktywna i uruchomiona. Poniższe procedury i tabela wyjaśniono, jak usunąć moduł kontrolera EBOD.

#### <a name="to-remove-an-ebod-module"></a>Aby usunąć moduł EBOD

1. Otwórz portal Azure klasyczny.

2. Przejdź do pozycji **urządzenia** > **Konserwacja** > **Stan sprzętu**i sprawdź, czy jest w stanie LED dla aktywnego modułu kontroler EBOD zielony i LED dla modułu kontroler EBOD jest czerwony.

3. Znajdź modułu kontroler EBOD z tyłu urządzenia.

4. Usuwanie kable łączące moduł kontrolera EBOD do kontrolera przed podjęciem moduł EBOD z systemu.

5. Zanotuj dokładnie portu skojarzeń zabezpieczeń Moduł kontrolera EBOD, który był podłączony do administratora. Trzeba będzie przywrócić system do tej konfiguracji po zamianie moduł EBOD. 

    >[AZURE.NOTE] Zazwyczaj są to A portu, który jest oznaczony jako **hosta w** poniższym diagramie.

    ![Kontroler tablica połączeń EBOD](./media/storsimple-ebod-controller-replacement/IC741049.png)

     **Rysunek 1** Tylnej strony EBOD modułu

  	|Etykieta|Opis|
  	|:----|:----------|
  	|1|Błąd LED|
  	|2|Power LED|
  	|3|Łączniki skojarzenia zabezpieczeń|
  	|4|LED skojarzenia zabezpieczeń|
  	|5|Porty szeregowe tylko do użytku factory|
  	|6|Port (hosta w)|
  	|7|Port B (hosta się)|
  	|8|Port C (tylko w trybie Factory)|

## <a name="install-a-new-ebod-controller"></a>Instalowanie nowego kontrolera EBOD

Poniższe procedury i tabela wyjaśniono, jak zainstalować moduł kontrolera EBOD w urządzeniu StorSimple.

#### <a name="to-install-an-ebod-controller"></a>Aby zainstalować kontroler EBOD

1. Sprawdzanie urządzenia EBOD za szkody, szczególnie do łącznika interfejsu. Nie można zainstalować nowy kontroler EBOD Jeśli zgięte dowolnego PIN.

2. Z zamknięcia w pozycji otwartej Przesuń modułu do załącznik do momentu zamknięcia uczestniczyć.

    ![Instalowanie kontrolera EBOD](./media/storsimple-ebod-controller-replacement/IC741050.png)

    **Rysunek 2**  Instalowanie moduł kontrolera EBOD

3. Zamknij zatrzask. Kliknij pozycję powinno być słychać jako uczestniczy zatrzask.

    ![Zwalnianie zatrzaśnięć EBOD](./media/storsimple-ebod-controller-replacement/IC741047.png)

    **Rysunek 3**  Zamykanie zatrzaśnięć moduł EBOD

4. Ponownie podłącz kable. Za pomocą dokładnie konfiguracji, która została użyta przed zastąpić. Zobacz poniższy diagram i tabeli, aby uzyskać szczegółowe informacje o sposobie łączenia kable.

    ![Kabel urządzenia 4U w celu power](./media/storsimple-ebod-controller-replacement/IC770723.png)

    **Rysunek 4**. Ponowne łączenie kable

  	|Etykieta|Opis|
  	|:----|:----------|
  	|1|Podstawowy załącznik|
  	|2|PCM 0|
  	|3|PCM 1|
  	|4|Kontroler 0|
  	|5|Kontroler 1|
  	|6|Kontroler EBOD 0|
  	|7|Kontroler EBOD 1|
  	|8|Załącznik EBOD|
  	|9|Jednostki dystrybucji zasilania|

## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej na temat [Wymiana składników sprzętu StorSimple](storsimple-hardware-component-replacement.md).
