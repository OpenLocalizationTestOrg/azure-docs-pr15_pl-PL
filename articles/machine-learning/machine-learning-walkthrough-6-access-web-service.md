<properties
    pageTitle="Krok 6: Dostęp do usługi sieci web maszynowego uczenia | Microsoft Azure"
    description="Krok 6 opracowanie skorzystać z przewidywanych rozwiązanie: dostęp do usługi sieci web usługi active Azure maszynowego uczenia."
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="garye"/>


# <a name="walkthrough-step-6-access-the-azure-machine-learning-web-service"></a>Przewodnik krok 6: Dostęp do usługi sieci web nauki maszynowego Azure

To jest ostatni krok Instruktaż [opracowanie rozwiązania przewidywanych analizy w programie nauki maszynowego Azure](machine-learning-walkthrough-develop-predictive-solution.md)


1.  [Tworzenie obszaru roboczego nauki komputera](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [Przekaż istniejące dane](machine-learning-walkthrough-2-upload-data.md)
3.  [Tworzenie nowego doświadczenia](machine-learning-walkthrough-3-create-new-experiment.md)
4.  [Szkolenie i ocenianie modeli](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  [Wdrażanie usługi sieci web](machine-learning-walkthrough-5-publish-web-service.md)
6.  **Dostęp do usługi sieci web**

----------

W poprzednim kroku, w tym instruktażu wdrożone usługi sieci web, korzystającego z naszego modelu przewidywania ryzyka kredytowej. Teraz użytkownicy muszą mieć możliwość wysyłają do niego dane i otrzymywać wyniki. 

Usługa sieci web jest usługi Azure sieci web, który może odbierać i zwrócić danych przy użyciu interfejsów API pozostałych w jeden z dwóch sposobów:  

-   **Żądanie/odpowiedź** — użytkownik wysyła jeden lub więcej wierszy danych kredytową z usługą przy użyciu protokołu HTTP, a usługa odpowiada z co najmniej jeden zestaw wyników.
-   **Wsadowe** — użytkownik przechowuje jeden lub więcej wierszy danych kredytowej w obiektów blob platformy Azure, a następnie wysyła lokalizacji obiektów blob usługi. Usługa widoczne wszystkie wiersze danych w wprowadzania obiektów blob, przechowuje wyniki w innej obiektów blob i zwraca adres URL w danym kontenerze.  

Najszybszym i Najprostszym sposobem uzyskania dostępu do usługi sieci web jest dostępna w [Azure Web sklep z aplikacjami](https://azure.microsoft.com/marketplace/web-applications/all/)szablony aplikacji sieci Web.
Te szablony aplikacji sieci web można utworzyć niestandardowej aplikacji sieci web znający wprowadzania danych usługi sieci web i co może zwrócić. Wszystko, co należy zrobić to dostęp do usługi sieci web i danymi, a szablon reszta.

Aby uzyskać więcej informacji na temat korzystania z szablonów aplikacji sieci web zobacz [Consume usługi sieci web nauki maszynowego Azure za pomocą szablonu aplikacji sieci web](machine-learning-consume-web-service-with-web-app-template.md).

Można również zaprojektować aplikacji niestandardowej do uzyskania dostępu do usługi sieci web przy użyciu kodu starter dostarczany w R, C# i Python języków programowania.
Pełne szczegóły można znaleźć w [temacie jak do korzystania z usługi sieci web Azure maszynowego uczenia, który został opublikowany na komputerze nauki doświadczenia](machine-learning-consume-web-services.md).
