<properties
    pageTitle="Ponowne próbkowanie maszynowego uczenia modelu | Microsoft Azure"
    description="Dowiedz się, jak przy przekwalifikowaniu modelu i usługi sieci Web używać modelu nowo przeszkolony w Azure maszynowego uczenia aktualizacji."
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="raymondl"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="v-donglo"/>

# <a name="retrain-a-machine-learning-model"></a>Ponowne próbkowanie maszynowego uczenia modelu

W ramach procesu operationalization maszynowego uczenia modeli w Azure maszynowego uczenia modelu jest szkolenia i zapisany. Następnie użycia jej do tworzenia predicative usługi sieci Web. Usługa sieci Web może być następnie wykorzystana w witryn sieci web, pulpitów nawigacyjnych i aplikacji dla urządzeń przenośnych. 

Modele utworzone za pomocą komputera nauki nie są zwykle statyczne. Nowe dane stał się dostępny lub po własne dane dla klientów indywidualnych interfejsu API modelu musi być retrained. 

Przeszkolenie może występować często. Za pomocą funkcji programowej interfejs API przeszkolenie można programowo przy przekwalifikowaniu modelu za pomocą interfejsów API przeszkolenie i aktualizować usługi sieci Web z modelem nowo przeszkolony. 

Ten dokument opisuje proces przekwalifikowania i przedstawiono sposób użycia interfejsów API przeszkolenie.

## <a name="why-retrain-defining-the-problem"></a>Dlaczego przy przekwalifikowaniu: Definiowanie problemu  

W ramach maszynowego uczenia procesu szkolenia model przygotowaniu przy użyciu zestawu danych. Modele utworzone za pomocą komputera nauki nie są zwykle statyczne. Nowe dane stał się dostępny lub po własne dane dla klientów indywidualnych interfejsu API modelu musi być retrained.

W poniższych scenariuszach programowej interfejs API zapewnia wygodny sposób, aby umożliwić możesz lub konsumenta do interfejsów API tworzenia klienta, który można na podstawie jednorazowego lub regularne przekwalifikować modelu przy użyciu własnych danych. Następnie mogą oceniać wyniki przeszkolenie i zaktualizuj interfejs API usług sieci Web używać nowo przeszkolony modelu.

>[AZURE.NOTE] Jeśli masz istniejące doświadczenia szkolenia i usługi sieci Web nowy, być może zechcesz zapoznaj się z ponownego próbkowania istniejącej usługi Web przewidywanych zamiast po Instruktaż wymienione w poniższej sekcji.

## <a name="end-to-end-workflow"></a>Koniec do zakończenia przepływu pracy 

Ten proces obejmuje następujące składniki: szkolenie doświadczenia i eksperyment przewidywanych opublikowany jako usługi sieci Web. Umożliwiające przeszkolenie modelu przeszkolony doświadczenia szkolenie musi być opublikowany jako usługi sieci Web na dane wyjściowe przeszkolony modelu. Dzięki temu interfejsu API dostępu do modelu dla przeszkolenie. 

Poniższe kroki dotyczą zarówno nowy i klasyczny usługi sieci Web:

Tworzenie początkowej usługi przewidywanych sieci Web:

* Tworzenie eksperyment szkolenia
* Tworzenie eksperyment przewidywanych sieci web
* Wdrażanie usługi sieci web przewidywanych

Przekwalifikować usługi sieci Web:

* Aktualizowanie doświadczenia szkolenie umożliwiające przeszkolenie
* Wdrażanie przekwalifikowania usługi sieci web
* Ponowne próbkowanie modelu przy użyciu kodu partii wykonanie usługi

Aby skorzystać z powyższych kroków zobacz [przy przekwalifikowaniu maszynowego uczenia modeli programowy](machine-learning-retrain-models-programmatically.md).

Jeśli wdrożono klasyczny usługi sieci Web:

* Utwórz nowy punkt końcowy w usłudze przewidywanych sieci Web
* POPRAWKA adresu URL i kodu
* Wskaż nowy punkt końcowy retrained modelu za pomocą adresu URL poprawki 

Aby skorzystać z powyższych kroków zobacz [przekwalifikować usługi sieci Web klasyczny](machine-learning-retrain-a-classic-web-service.md).

Jeśli wystąpią problemy przeszkolenie usługi sieci Web klasyczny, zobacz [Rozwiązywanie problemów ze przeszkolenie usługi sieci Web Azure maszynowego uczenia klasyczny](machine-learning-troubleshooting-retraining-models.md).

Wdrożenie usługi sieci Web nowy:

* Zaloguj się do konta Menedżera zasobów Azure
* Pobieranie definicji usługi sieci Web
* Eksportowanie definicji usługi sieci Web jako JSON
* Aktualizowanie odwołanie do `ilearner` obiektów blob w formacie JSON
* Importowanie JSON do definicji usługi sieci Web
* Aktualizowanie usługi sieci Web przy użyciu nowych definicji usługi sieci Web

Aby skorzystać z powyższych kroków zobacz [przekwalifikować usługi nowej sieci Web przy użyciu polecenia cmdlet programu PowerShell zarządzania nauki komputera](machine-learning-retrain-new-web-service-using-powershell.md).

Proces konfigurowania szkoleniem usługi sieci Web klasyczny obejmuje następujące kroki:

![Przeszkolenie Omówienie procesu][1]

Proces konfigurowania szkoleniem usługi sieci Web nowego obejmuje następujące kroki:

![Przeszkolenie Omówienie procesu][7]

## <a name="other-resources"></a>Inne zasoby

- [Przeszkolenie i aktualizowanie nauka maszynowego Azure modele z Factory danych Azure](https://azure.microsoft.com/blog/retraining-and-updating-azure-machine-learning-models-with-azure-data-factory/)
- [Tworzenie wielu modeli nauki komputera i sieci web usługi punkty końcowe z doświadczenia przy użyciu programu PowerShell](machine-learning-create-models-and-endpoints-with-powershell.md)
- Klip wideo [AML przeszkolenie modeli przy użyciu interfejsów API](https://www.youtube.com/watch?v=wwjglA8xllg) pokazano, jak przy przekwalifikowaniu modeli maszynowego uczenia utworzonych Azure maszynowego uczenia przy użyciu programu PowerShell i przeszkolenie interfejsów API.

<!--image links-->
[1]: ./media/machine-learning-retrain-machine-learning-model/machine-learning-retrain-models-programmatically-IMAGE01.png
[7]: ./media/machine-learning-retrain-machine-learning-model/machine-learning-retrain-models-programmatically-IMAGE07.png

