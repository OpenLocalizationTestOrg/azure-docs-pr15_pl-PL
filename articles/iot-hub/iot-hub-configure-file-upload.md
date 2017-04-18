<properties
     pageTitle="Konfigurowanie przekazywania plików przy użyciu Azure portal | Microsoft Azure"
     description="Omówienie sposobu konfigurowania przekazywania plików za pomocą portalu Azure"
     services="iot-hub"
     documentationCenter=""
     authors="dominicbetts"
     manager="timlt"
     editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="na"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/30/2016"
     ms.author="dobett"/>

# <a name="configure-file-uploads-using-the-azure-portal"></a>Konfigurowanie przekazywania plików za pomocą portalu Azure

## <a name="file-upload"></a>Przekazywanie pliku

Aby użyć [funkcji w Centrum IoT przekazywanie plików][lnk-upload], najpierw należy skojarzyć konto Azure miejsca do magazynowania z Twoim Centrum. Wybierz ustawienia **przekazywanie plików** , aby wyświetlić listę właściwości przekazywania pliku do koncentratora IoT, który jest modyfikowany.

![][13]

**Kontenera magazynu**: Umożliwia wybranie kontenera obiektów blob na koncie magazyn Azure w bieżącej subskrypcji Azure powiązany z Twoim Centrum IoT Azure portal. W razie potrzeby możesz utworzyć konta magazynu platformy Azure na kontenerze karta i obiektów blob **kont miejsca do magazynowania** na karta **kontenerów** . Centrum IoT umożliwia automatyczne generowanie URI skojarzenia zabezpieczeń z uprawnieniami do zapisu z tym kontenerem obiektów blob dla urządzeń przydatnych podczas ich przekazywania plików.

![][14]

**Odbieranie powiadomień o przekazane pliki**: Włącz lub Wyłącz powiadomienia o przekazywania plików za pomocą przełącznika.

**TTL skojarzeń zabezpieczeń**: to ustawienie jest czas wygaśnięcia identyfikatorów skojarzeń zabezpieczeń zwracane na urządzeniu przez Centrum IoT. Domyślnie ustawiona na 1 godzinę, ale można dostosować do innych wartości za pomocą suwaka.

**Plik powiadomienie, że ustawienia domyślne pola TTL**: powiadomienie czasu wygaśnięcia pliku przekazywania przed wygasł. Domyślnie ustawiona na jeden dzień, ale można dostosować do innych wartości za pomocą suwaka.

**Liczba dostarczaniem maksymalny powiadomienie plików**: liczba prób Centrum IoT do przeprowadzania pliku powiadomienie przekazywania. Domyślnie do 10, ale można dostosować do innych wartości za pomocą suwaka.

![][15]

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji na temat możliwości przekazania pliku Centrum IoT, zobacz [przekazywanie plików z urządzenia] [ lnk-upload] w Podręczniku dewelopera.

Skorzystaj z poniższych łączy, aby dowiedzieć się więcej o zarządzaniu Centrum IoT Azure:

- [Zbiorcze zarządzanie urządzeniami IoT][lnk-bulk]
- [Metryki zastosowania][lnk-metrics]
- [Operacje monitorowania][lnk-monitor]

Aby jeszcze bardziej Poznawanie możliwości Centrum IoT, zobacz:

- [Przewodnik dewelopera][lnk-devguide]
- [Symulowanie urządzenia z IoT SDK bramy][lnk-gateway]
- [Zabezpieczanie rozwiązania IoT od podstaw w górę][lnk-securing]


  [13]: ./media/iot-hub-configure-file-upload/file-upload-settings.png
  [14]: ./media/iot-hub-configure-file-upload/file-upload-container-selection.png
  [15]: ./media/iot-hub-configure-file-upload/file-upload-selected-container.png

[lnk-upload]: iot-hub-devguide-file-upload.md

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
[lnk-securing]: iot-hub-security-ground-up.md