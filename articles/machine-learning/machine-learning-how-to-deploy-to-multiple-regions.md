<properties
    pageTitle="Jak wdrożyć usługi sieci Web do wielu regionów | Microsoft Azure"
    description="Czynności, aby wdrożyć regionów (kopii) Nowa usługa sieci Web z innymi."
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="raymondl"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="v-donglo"/>

# <a name="how-to-deploy-a-web-service-to-multiple-regions"></a>Jak wdrożyć usługi sieci Web do wielu regionów

Nowe usługi Azure w sieci Web umożliwiają łatwe wdrażanie usługi sieci web do wielu regionów bez konieczności wiele subskrypcji bądź obszarów roboczych. 

Cennik to region określonym, dlatego należy zdefiniować rozliczeń planu dla każdego regionu, w którym będzie wdrażanie usługi sieci web.

## <a name="to-create-a-plan-in-another-region"></a>Aby utworzyć plan w innym regionie

1. Zaloguj się do [komputera Microsoft Azure nauki usług sieci Web](https://services.azureml.net/).
2. Kliknij opcję menu **planów** .
3. Na plany na stronie widoku kliknij przycisk **Nowy**.
4. Z menu rozwijanego **Subskrypcja** Wybierz subskrypcję, w którym będzie znajdować się nowy plan.
5. Z menu rozwijanego **Region** wybierz region w nowym planie. Opcje planu dla wybranego regionu są wyświetlane w sekcji **Opcje planu** strony.
6. Z menu rozwijanego **Grupa zasobów** wybierz grupę zasobów dla planu. Aby uzyskać więcej informacji dotyczących grup zasobów zobacz [Zarządzanie Azure zasoby portalu](../azure-portal/resource-group-portal.md).
7. W polu **Nazwa Plan** wpisz nazwę planu.
8. W obszarze **Opcje planu**kliknij poziom rozliczeń w nowym planie.
9. Kliknij przycisk **Utwórz**.


## <a name="deploying-the-web-service-to-another-region"></a>Wdrażanie usługi sieci web do innego regionu

1. Kliknij opcję menu **Usług sieci Web** .
2. Wybierz usługę sieci Web w przypadku rozmieszczania nowy region.
3. Kliknij przycisk **Kopiuj**.
4. W polu **Nazwa usługi sieci Web**wpisz nową nazwę dla usługi sieci web.
5. W polu **opisu usługi sieci Web**wpisz opis usługi sieci web.
6. Z menu rozwijanego **Subskrypcja** Wybierz subskrypcję, w którym będzie znajdować się nowa usługa sieci web.
7. Z menu rozwijanego **Grupa zasobów** wybierz grupę zasobów usługi sieci web. Aby uzyskać więcej informacji dotyczących grup zasobów zobacz [Zarządzanie Azure zasoby portalu](../azure-portal/resource-group-portal.md).
8. Z menu rozwijanego **regionu** zaznacz obszar, w którym chcesz wdrożyć usługę sieci web.
9. Z menu rozwijanego **miejsca do magazynowania konta** wybierz konto miejsca do magazynowania, w którym chcesz przechowywać usługi sieci web.
10. Z menu rozwijanego **Cena Plan** wybierz plan w regionie wybranym w kroku 8.
11. Kliknij przycisk **Kopiuj**.

