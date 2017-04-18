<properties
    pageTitle="Używanie usługi sieci web uczenia komputera za pomocą szablonu aplikacji sieci web | Microsoft Azure"
    description="Korzystanie z szablonu aplikacji sieci web w Azure Marketplace do korzystania z usługi sieci web przewidywanych Azure maszynowego uczenia."
    keywords="Usługa sieci Web, operationalization, interfejsu API usługi REST komputera nauki"
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
    ms.date="10/10/2016"
    ms.author="garye;raymondl"/>

# <a name="consume-an-azure-machine-learning-web-service-with-a-web-app-template"></a>Używanie usługi sieci web nauki maszynowego Azure za pomocą szablonu aplikacji sieci web

>[AZURE.NOTE] W tym temacie opisano techniki dotyczące usługi sieci web klasyczny. 

Po opracowanych przewidywanych modelu i wdrożyć go jako usługi Azure sieci web przy użyciu komputera nauki Studio lub za pomocą narzędzi, takich jak R lub Python, masz dostęp do modelu operationalized, za pomocą interfejsu API usługi REST.

Istnieje kilka sposobów używania interfejsu API usługi REST i uzyskać dostęp do usług sieci web. Na przykład można napisać aplikację w C#, R lub Python za pomocą kodu przykładowe wygenerowana po wdrożeniu usługi sieci web (dostępne na stronie Pomoc interfejsu API na pulpicie nawigacyjnym usługi sieci web w Studio nauki komputera). Lub można użyć przykładowego skoroszytu programu Microsoft Excel utworzenia (również dostępne na pulpicie nawigacyjnym usługi sieci web w Studio).

Ale najszybszym i Najprostszym sposobem uzyskania dostępu do usługi sieci web jest dostępna w [Azure Web sklep z aplikacjami](https://azure.microsoft.com/marketplace/web-applications/all/)szablony aplikacji sieci Web.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="the-azure-machine-learning-web-app-templates"></a>Azure maszynowego uczenia szablony aplikacji sieci Web

Szablony aplikacji sieci web dostępna w Azure Marketplace można utworzyć niestandardowej aplikacji sieci web znający wprowadzania danych usługi sieci web i oczekiwanych wyników. Wszystko, co należy zrobić to udzielić dostępu aplikacji sieci web do usługi sieci web i danymi, a szablon reszta.

Dostępne są dwa szablony:

- [Szablon aplikacji sieci Web usługi Azure ML żądanie odpowiedź](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/)
- [Szablon aplikacji Azure partii ML wykonanie usługi sieci Web](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/)

Każdy szablon utworzy przykładowej aplikacji ASP.NET, korzystając z interfejsu API URI i klucza tej usługi sieci web i wdrożenie go jako witryny sieci web Azure. Szablon odpowiedzi żądanie usługi (RR) służy do tworzenia aplikacji sieci web, która umożliwia wysyłanie pojedynczy wiersz danych do usługi sieci web, aby uzyskać pojedynczy wynik. Szablon partii wykonanie usługi (BES) służy do tworzenia aplikacji sieci web, która pozwala na wysyłanie wielu wierszy danych w celu uzyskania wiele wyników.

Nie kodowania jest wymagany do używania tych szablonów. Wystarczy podać URI interfejsu API i klucza i na podstawie szablonu utworzyć aplikację.

## <a name="how-to-use-the-request-response-service-rrs-template"></a>Jak korzystać z szablonu odpowiedzi żądanie usługi (RR)

Gdy został wdrożony usługi sieci web, można wykonać poniższe czynności, aby użyć szablonu aplikacji sieci web rekordy zasobów, jak pokazano na poniższym diagramie.

![Proces używania szablonu sieci web rekordy zasobów][image1]

1. W komputerze nauki Studio Otwórz kartę **Usług sieci Web** , a następnie otwórz usługi sieci web, do którego chcesz uzyskać dostęp. Kopiowanie klucza na liście w obszarze **klucz interfejsu API** i zapisz go.

    ![Klucz interfejsu API][image3]

2. Otwórz stronę pomocy API **Żądanie/odpowiedź** . W górnej części strony pomocy w obszarze **żądanie**skopiuj wartość **Identyfikatora URI żądania** i zapisz go. Ta wartość będzie wyglądać następująco:

        https://ussouthcentral.services.azureml.net/workspaces/<workspace-id>/services/<service-id>/execute?api-version=2.0&details=true

    ![Identyfikator URI żądania][image4]

3. Przejdź do [Azure portal](https://portal.azure.com) **logowania**, kliknij pozycję **Nowy**, wyszukiwanie i wybierz pozycję **Azure ML odpowiedź żądania usługi w przeglądarce**, a następnie kliknij przycisk **Utwórz**. 

    - Określ unikatową nazwę dla aplikacji sieci web. Adres URL aplikacji sieci web będzie tej nazwy, a następnie `.azurewebsites.net.` na przykład`http://carprediction.azurewebsites.net.`

    - Wybierz subskrypcję Azure i usług, na których działa usługa sieci web.

    - Kliknij przycisk **Utwórz**.

    ![Tworzenie aplikacji sieci web][image5]

4. Po zakończeniu Azure wdrażanie aplikacji sieci web, kliknij adres **URL** na stronie Ustawienia aplikacji sieci web platformy Azure lub wprowadź adres URL w przeglądarce sieci web. Na przykład`http://carprediction.azurewebsites.net.`

5. Przy pierwszym uruchomieniu aplikacji sieci web go wyświetli monit dla **Adresu URL wpisu interfejsu API** oraz **Klucz interfejsu API**.
Wprowadź wartości, który został wcześniej zapisany:
    - **Adres URL żądania** ze strony pomocy interfejsu API dla **adresu URL wpisu interfejsu API**
    - **Klucz interfejsu API** z pulpitu nawigacyjnego usług sieci web dla **Klucz interfejsu API**.

    Kliknij przycisk **Prześlij**.

    ![Wprowadź klucz interfejsu API i wpis identyfikator URI][image6]

6. Aplikacji sieci web zostanie wyświetlona jego strona **Konfiguracja aplikacji sieci Web** z bieżącymi ustawieniami usługi sieci web. W tym miejscu można wprowadzać zmiany ustawienia używane przez aplikację sieci web.

    > [AZURE.NOTE] Opisane tutaj ustawienia tylko zmiana ich dla tej aplikacji sieci web. Go nie powoduje zmiany ustawień domyślnych usługi sieci web. Na przykład jeśli zmienisz **Opis** poniżej nie powoduje zmiany opis wyświetlane na pulpicie nawigacyjnym usługi sieci web w Studio nauki komputera.

    Po zakończeniu, kliknij przycisk **Zapisz zmiany**, a następnie kliknij przycisk **Przejdź do strony głównej**.

7. Na stronie głównej, które można wprowadzać wartości, aby wysłać do usługi sieci web, kliknij przycisk **Prześlij**, a zostanie zwrócony wynik.

Jeśli chcesz powrócić do strony **Konfiguracja** , przejdź do `setting.aspx` strony aplikacji sieci web. Na przykład: `http://carprediction.azurewebsites.net/setting.aspx.` pojawi się monit o wprowadzenie klucza interfejsu API ponownie — potrzebne, który umożliwia dostęp do strony i aktualizowanie ustawień.

Możesz zatrzymać, uruchom ponownie lub usuwanie aplikacji sieci web w portalu Azure, takich jak dowolnej innej aplikacji sieci web. Jak działa można przejdź do adresu internetowego narzędzia główne i wprowadź nowe wartości.

## <a name="how-to-use-the-batch-execution-service-bes-template"></a>Jak korzystać z szablonu partii wykonanie usługi (BES)

Szablon aplikacji sieci web BES służy w taki sam sposób jak w szablonie rekordy, z wyjątkiem, że aplikacji sieci web, która zostanie utworzona umożliwia przesyłanie wielu wierszy danych i odbierać wiele wyników.

Wyniki z usługi sieci web wykonanie partii są przechowywane w kontenerze Azure miejsca do magazynowania; wartości wejściowych mogą pochodzić z magazynu Azure lub plik lokalny.
Tak konieczne będzie kontenera Azure magazynu do przechowywania wyniki zwrócone przez aplikacji sieci web, a musisz przygotowywanie wprowadzania danych.

![Proces używania szablonu sieci web BES][image2]

1. Postępuj zgodnie z procedurą tworzenie aplikacji sieci web BES dla szablonu rekordy, z wyjątkiem:
    - Uzyskaj **Żądanie URI** na stronie Pomoc interfejsu API **WSADOWE** usługi sieci web.
    - Przejdź do [Szablonu aplikacji sieci Web usługi wykonanie Azure ML partii](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/) , aby otworzyć szablon BES w Azure Marketplace, a następnie kliknij pozycję **Tworzenie aplikacji sieci Web**.

2. Aby określić miejsce, w którym wyniki przechowywane, wprowadź informacje kontener docelowy na stronie głównej aplikacji sieci web. Także określić, gdzie znaleźć wartości wejściowych w pliku lokalnego lub kontenera magazynu Azure aplikacji sieci web.
Kliknij przycisk **Prześlij**.

    ![Informacje o miejsca do magazynowania][image7]

Aplikacji sieci web będzie wyświetlana stronę o stanie zadania.
Po ukończeniu zadania będziesz mieć lokalizacji wyników w magazynie obiektów blob platformy Azure. Masz również możliwość pobierania wyników do pliku lokalnego.

## <a name="for-more-information"></a>Aby uzyskać więcej informacji

Aby dowiedzieć się więcej na temat...

- Tworzenie eksperyment nauki komputera za pomocą komputera nauki Studio, zobacz [Tworzenie z pierwszym doświadczeniu w Azure maszynowego uczenia Studio](machine-learning-create-experiment.md)

- jak wdrożyć stopień opanowania materiału maszynowego poeksperymentować jako usługi sieci web, zobacz [Wdrażanie usługi sieci web nauki maszynowego Azure](machine-learning-publish-a-machine-learning-web-service.md)

- inne sposoby uzyskiwania dostępu do usługi sieci web, zobacz, [jak korzystać z usługi sieci web Azure maszynowego uczenia](machine-learning-consume-web-services.md)


[image1]: media\machine-learning-consume-web-service-with-web-app-template\rrs-web-template-flow.png
[image2]: media\machine-learning-consume-web-service-with-web-app-template\bes-web-template-flow.png
[image3]: media\machine-learning-consume-web-service-with-web-app-template\api-key.png
[image4]: media\machine-learning-consume-web-service-with-web-app-template\post-uri.png
[image5]: media\machine-learning-consume-web-service-with-web-app-template\create-web-app.png
[image6]: media\machine-learning-consume-web-service-with-web-app-template\web-service-info.png
[image7]: media\machine-learning-consume-web-service-with-web-app-template\storage.png
