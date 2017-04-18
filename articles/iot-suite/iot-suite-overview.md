<properties
    pageTitle="Omówienie pakietu Microsoft Azure IoT Suite | Microsoft Azure"
    description="Podstawowe informacje o pakiecie IoT Azure udostępnia internet i rozwiązań wstępnie rzeczy zbieranie, analizowanie, przechowywania danych, podaj wizualizacji i integracja z innych systemów."
    services=""
    suite="iot-suite"
    documentationCenter=""
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-suite"
     ms.devlang="na"
     ms.topic="get-started-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/09/2016"
     ms.author="dobett"/>

# <a name="what-is-azure-iot-suite"></a>Co to jest pakiet IoT Azure?

Azure internet usług czynności (IoT) oferuje szeroką gamę możliwości. Te usługi kategorii enterprise umożliwiają:

- Zbierz dane od urządzenia
- Analizowanie danych strumieni w ruchu
- Przechowywanie i kwerendy dużych zestawów danych
- Wizualizowanie danych zarówno w czasie rzeczywistym i historyczne
- Integracja z systemami office Wstecz

Do przeprowadzania te funkcje, pakiety pakietu IoT Azure razem wielu usług Azure za pomocą rozszerzenia niestandardowe jako *wstępnie rozwiązań*. Te rozwiązania wstępnie są podstawowej implementacji typowe wzorce rozwiązanie IoT, które pomagają Aby skrócić czas, które można wykonać, aby usługi rozwiązań IoT. Za pomocą [IoT software development kit][lnk-sdks], można dostosować i rozszerzania tych rozwiązań do własnych wymagań. Umożliwia także te rozwiązania jako przykłady lub szablonów podczas tworzenia nowych rozwiązań IoT.

Poniższym klipie wideo przedstawiono wprowadzenie do pakietu IoT Azure:

> [AZURE.VIDEO azurecon-2015-introducing-the-microsoft-azure-iot-suite]

## <a name="azure-iot-services-in-azure-iot-suite"></a>Usług Azure IoT pakietu IoT Azure

Zwykle wstępnie rozwiązań przy użyciu następujących usług:

- Podstawowe w pakiecie IoT Azure jest [Azure IoT Centrum] [ lnk-iot-hub] usługi. Ta usługa zapewnia możliwości obsługi urządzeń w chmurze i chmury do urządzenia i działa jako bramę do chmury i innych najważniejszych usług IoT pakietu. Usługa umożliwia odbieranie wiadomości z urządzenia w skali, a następnie Wyślij polecenia do urządzenia.

- [Azure analizy strumieniu] [ lnk-asa] zawiera analiza danych w ruchu. Pakiet IoT wykorzystuje tę usługę do procesu telemetrycznego przychodzących, prowadzenie agregacji i wykrywanie zdarzeń. Wstępnie skonfigurowane rozwiązań też użyć analizy strumieniu przetwarzania komunikaty, które zawierają dane takie jak metadanych lub polecenia odpowiedzi od urządzeń. Rozwiązania za pomocą analizy strumieniu przetwarzania wiadomości od urządzenia i dostarczania tych wiadomości z innymi usługami.

- [Azure magazynowania] [ lnk-azure-storage] i [Azure DocumentDB] [ lnk-document-db] zapewniają możliwości przechowywania danych. Wstępnie skonfigurowane rozwiązań za pomocą magazyn obiektów blob do przechowywania telemetrycznego i udostępnić go na potrzeby analizy. Rozwiązania za pomocą DocumentDB do przechowywania metadanych urządzenia i Włącz funkcje zarządzania urządzenia rozwiązań.

- [Azure aplikacje sieci Web] [ lnk-web-apps] i [Microsoft Power BI] [ lnk-power-bi] zapewniają możliwości wizualizacji danych. Elastyczność Power BI umożliwia szybkie tworzenie własnych interakcyjne pulpity nawigacyjne, korzystających z pakietu IoT danych.

Omówienie architektury typowe rozwiązania IoT, zobacz [Microsoft Azure i Internet czynności (IoT)][iot-suite-what-is-azure-iot].

## <a name="preconfigured-solutions"></a>Wstępnie skonfigurowane rozwiązań

Pakiet IoT zawiera wstępnie rozwiązania, które pozwalają szybko rozpocząć pracę z i eksplorowanie typowe scenariusze IoT, takich jak *zdalnego monitorowania* i *konserwacji przewidywanych*, umożliwiają pakietu IoT Azure. Możesz wdrożyć te rozwiązania do subskrypcji usługi Azure, a następnie uruchom scenariusz IoT wykonane, zakończenia do końca.

## <a name="next-steps"></a>Następne kroki

Teraz, gdy masz Omówienie możliwości pakietu IoT i jakie są ich główne składniki, możesz dowiedzieć się więcej o wstępnie rozwiązań pakietu IoT, zobacz [Co to są Azure IoT wstępnie rozwiązań?][lnk-what-are-preconfig]

[lnk-sdks]: https://azure.microsoft.com/documentation/articles/iot-hub-sdks-summary/
[lnk-iot-hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-azure-storage]: https://azure.microsoft.com/documentation/services/storage/
[lnk-document-db]: https://azure.microsoft.com/documentation/services/documentdb/
[lnk-power-bi]: https://powerbi.microsoft.com/
[lnk-web-apps]: https://azure.microsoft.com/documentation/services/app-service/web/
[iot-suite-what-is-azure-iot]: iot-suite-what-is-azure-iot.md
[lnk-what-are-preconfig]: iot-suite-what-are-preconfigured-solutions.md
