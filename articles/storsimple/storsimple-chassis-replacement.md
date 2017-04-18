<properties 
   pageTitle="Zamienianie obudowy na urządzeniu StorSimple | Microsoft Azure"
   description="W tym artykule opisano, jak usunąć i zamienić obudowy StorSimple podstawowego załącznik lub załącznik EBOD."
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

# <a name="replace-the-chassis-on-your-storsimple-device"></a>Zamienianie obudowy na urządzeniu StorSimple

## <a name="overview"></a>Omówienie

Ten samouczek wyjaśniono, jak usunąć i zamienić podstawę w urządzeniu serii StorSimple 8000. Model StorSimple 8100 jest urządzenie pojedynczy załącznik (jeden podwozie), 8600 jest urządzenie podwójne załącznik (dwa podwozie). W przypadku modelu 8600 są potencjalnie dwóch obudowy, które może nie działać w urządzeniu: obudowa podstawowego załącznik lub ramie do załącznik EBOD.

W obu przypadkach podstawę zastąpienie, w której jest dostarczany przez firmę Microsoft jest pusta. Nie dodatku i chłodzenia modułów (PCMs), moduły kontroler dyski dysku stanie stałym SSD, dysków twardych (dysków twardych) lub modułów EBOD zostaną uwzględnione.

>[AZURE.IMPORTANT] Przed usuwania i zamieniania podwozia, przejrzyj informacje bezpieczeństwa w [Wymiana składników sprzętu StorSimple](storsimple-hardware-component-replacement.md).

## <a name="remove-the-chassis"></a>Usuwanie obudowy

Wykonaj poniższe czynności, aby usunąć obudowy na urządzeniu StorSimple.

#### <a name="to-remove-a-chassis"></a>Aby usunąć podstawę

1. Upewnij się, że urządzenie StorSimple jest zamknięty i odłączane od źródeł power.

2. Usuwanie wszystkich sieciowych i kable skojarzeń zabezpieczeń, w razie potrzeby.

3. Usuwanie jednostki z szafy.

4. Usuwanie wszystkich dysków i zanotuj gniazd, z których są usuwane. Aby uzyskać więcej informacji zobacz [Usuwanie na dysku twardym](storsimple-disk-drive-replacement.md#remove-the-disk-drive).

5. Na załącznik EBOD (jeśli jest to podstawę, która nie powiodło się) usuwać moduły kontroler EBOD. Aby uzyskać więcej informacji zobacz [Usuwanie kontroler EBOD](storsimple-ebod-controller-replacement.md#remove-an-ebod-controller). 

    Na podstawowy załącznik (jeśli jest to podstawę, która nie powiodło się) Usuń kontrolery i zanotuj gniazd, z których są usuwane. Aby uzyskać więcej informacji zobacz [Usuwanie kontrolera](storsimple-controller-replacement.md#remove-a-controller).

## <a name="install-the-chassis"></a>Instalowanie obudowy

Wykonaj poniższe czynności, aby zainstalować obudowy na urządzeniu StorSimple.

#### <a name="to-install-a-chassis"></a>Aby zainstalować podstawę

1. Zainstaluj urządzenie w stojaków. Aby uzyskać więcej informacji zobacz [stojaków instalacji urządzenia StorSimple 8100](storsimple-8100-hardware-installation.md#rack-mount-your-storsimple-8100-device) lub [stojaków instalacji urządzenia StorSimple 8600](storsimple-8600-hardware-installation.md#rack-mount-your-storsimple-8600-device).

2. Po obudowy jest zainstalowany w szafy, należy zainstalować moduł kontroler na tych samych pozycjach, które były wcześniej zainstalowane w.

3. Instalowanie dysków w tej samej pozycji i gniazda, które były wcześniej zainstalowane w.

    >[AZURE.NOTE] Zalecamy, aby najpierw zainstalować twarde SSD w gniazdach, a następnie zainstaluj dysków twardych.

2. Z urządzeniem zainstalowanych w szafy i składniki zainstalowane nawiązywania połączenia między urządzeniem źródeł odpowiednie uprawnienia, a następnie Włącz urządzenie. Aby uzyskać szczegółowe informacje zobacz [kabel urządzenia StorSimple 8100](storsimple-8100-hardware-installation.md#cable-your-storsimple-8100-device) lub [kabel urządzenia StorSimple 8600](storsimple-8600-hardware-installation.md#cable-your-storsimple-8600-device).

## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej na temat [Wymiana składników sprzętu StorSimple](storsimple-hardware-component-replacement.md).

