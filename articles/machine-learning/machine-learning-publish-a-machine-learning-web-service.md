<properties
    pageTitle="Wdrażanie usługi sieci web uczenie maszynowe | Microsoft Azure"
    description="Jak przekonwertować eksperyment szkolenia cennik, przygotowania go do wdrożenia, a następnie wdrożyć go jako usługi sieci web z usługą sieci Web."
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

# <a name="deploy-an-azure-machine-learning-web-service"></a>Wdrażanie usługi sieci web z usługą sieci Web

Uczenie maszynowe Azure pozwala na tworzenie, testowanie i wdrażanie przewidywanych rozwiązań analitycznych.

Z wysokiego poziomu punktu widzenia można to zrobić w trzech krokach:

- **[Utwórz doświadczenia szkolenia]** - omówienie jest środowiskiem współpracy na rzecz rozwoju visual, które służy do pociągu i przetestować predykcyjną modelu przy użyciu danych szkolenia, podana.
- **[Przekonwertuj go do przewidywanych doświadczenia]** - raz model został przeszkolony z istniejącymi danymi i możesz zacząć go używać, aby zdobyć nowych danych, przygotowanie i usprawnić eksperymentu dla prognoz.
- **Wdrożyć go jako usługi sieci web** - można wdrożyć eksperymentu predykcyjnej jako usługi sieci Azure web [nowych] lub [Klasyczny] . Użytkownicy mogą wysyłać dane do modelu i otrzymywać swój model prognozy.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="create-a-training-experiment"></a>Utwórz eksperyment szkolenia

Do pociągu modelu predykcyjną, umożliwia Studio uczenia maszynowego Azure Utwórz eksperyment szkolenia gdzie obejmują różne moduły, aby załadować dane szkolenia, przygotować dane w razie potrzeby, zastosowanie algorytmów uczenia maszynowego i oceny wyników. Można iteracyjne na doświadczenia i spróbować algorytmów uczenia różne stanowiska do porównania i oceny wyników.

Proces tworzenia i zarządzania nimi eksperymenty szkolenia jest objęty bardziej dokładnie gdzie indziej. Aby uzyskać więcej informacji zobacz następujące artykuły:

- [Tworzenie prostego doświadczenia w omówienie](machine-learning-create-experiment.md)
- [Zaprojektowanie rozwiązania predykcyjnej z usługą sieci Web](machine-learning-walkthrough-develop-predictive-solution.md)
- [Zaimportuj dane szkolenia do omówienie](machine-learning-data-science-import-data.md)
- [Zarządzanie iteracjami eksperymentów w omówienie](machine-learning-manage-experiment-iterations.md)

## <a name="convert-the-training-experiment-to-a-predictive-experiment"></a>Konwertuj eksperyment szkolenia do przewidywanych doświadczenia

Po już przeszkoleni modelu, możesz przystąpić do przeliczania eksperymentu szkolenia cennik zdobycie nowych danych.

Poprzez przekształcenie cennik, otrzymujesz wyszkolonych modelu gotowy do wdrożenia jako usługi sieci web punktacji. Użytkownicy usługi sieci web mogą wysyłać dane wejściowe do modelu i model będzie odesłać wyników przewidywania. Podczas konwersji do przewidywanych doświadczenia, należy pamiętać, jak oczekujesz, model stosowaną przez innych użytkowników.

Aby przekonwertować eksperymentu szkolenia cennik, kliknij polecenie **Uruchom** , w dolnej części obszaru roboczego eksperymentu, kliknij przycisk **Ustaw usługi sieci Web**, a następnie wybierz **Metodę usługi sieci Web**.

![Konwertuj na punktacji doświadczenia](./media/machine-learning-publish-a-machine-learning-web-service/figure-1.png)

Aby uzyskać więcej informacji na temat sposobu wykonania tej konwersji zobacz [Konwertowanie eksperyment szkolenia uczenia maszynowego do przewidywanych doświadczenia](machine-learning-convert-training-experiment-to-scoring-experiment.md).

Następujące kroki opisują, wdrażanie cennik jako nowa usługa sieci web. Można także wdrożyć eksperymentu jako klasyczna usługa sieci web.

## <a name="deploy-the-predictive-experiment-as-a-new-web-service"></a>Wdrażanie cennik jako nowe usługi sieci web

Teraz, cennik został przygotowany, można wdrożyć go jako usługi sieci web Azure. Przy użyciu usługi sieci web, użytkownicy mogą wysyłać dane do modelu i modelu zwróci jego prognoz.

Aby wdrożyć eksperymentu predykcyjnej, kliknij przycisk **Uruchom** w dolnej części obszaru roboczego eksperymentu. Po doświadczeniu zakończy działanie, kliknij **Wdrażania usługi sieci Web** i wybierz **wdrażania usługi sieci Web [Nowy]**.  Zostanie otwarta strona wdrażania usługi sieci Web Learning maszyny portalu.

### <a name="machine-learning-web-service-portal-deploy-experiment-page"></a>Maszyny usługi sieci Web Learning portal Wdroż stronę doświadczenia

Na stronie wdrożenie eksperymentu wprowadź nazwę usługi sieci web.
Wybierz plan taryfowy. Jeśli masz istniejącego planu taryfowego, że można go wybrać, w przeciwnym razie należy utworzyć nowy plan ceny dla usługi.

1.  W **Stałej cenie** listy rozwijanej, wybierz istniejący plan lub wybierz opcję **Wybierz nowy plan** .
2.  W polu **Nazwa planu**wpisz nazwę, która będzie identyfikować planu na rachunku.
3.  Wybierz jeden z **Poziomów Plan miesięczny**. Domyślne poziomy planu do planów dla danego regionu domyślne oraz usługi sieci web jest wdrażany do tego regionu.

Kliknij przycisk **Deploy** a stronie **Szybki Start** otwiera usługi sieci web.

Strony sieci web usługi szybkiego startu daje wskazówki i dostęp do najbardziej typowe zadania, jakie będzie wykonywać po utworzeniu usługi sieci web. W tym miejscu można łatwy dostęp do strony testowej i zużywają strony.

<!-- ![Deploy the web service](./media/machine-learning-publish-a-machine-learning-web-service/figure-2.png)-->

### <a name="test-your-web-service"></a>Testowanie usługi sieci web

Aby przetestować nowej usługi sieci web, w obszarze Typowe zadania kliknij przycisk **Test usługi sieci web** . Na stronie testowej można przetestować usługi sieci web jako usługa typu żądanie-odpowiedź (RR) lub usługi Batch Execution (BEF).

Strona testowa rekordy zasobów wyświetla wejść, wyjść i globalne parametry, które zostały zdefiniowane dla potrzeb eksperymentu. Aby przetestować usługi sieci web, można ręcznie wprowadź odpowiednie wartości dla danych wejściowych lub podać oddzielone przecinkami wartości (CSV) plik sformatowany, zawierające wartości testowe.

Aby przetestować przy użyciu rekordów zasobów, z trybu widoku listy, wprowadź odpowiednie wartości dla danych wejściowych, a następnie kliknij przycisk **Test typu żądanie-odpowiedź**. Twój przewidywania wyniki są wyświetlane w kolumnie wyników w lewo.

![Wdrażanie usługi sieci web](./media/machine-learning-publish-a-machine-learning-web-service/figure-5-test-request-response.png)

Aby przetestować swój BES, kliknij **partii**. Na stronie testowej wsadowego kliknij Przeglądaj w obszarze dane wejściowe i wybierz plik CSV zawierający przykładowe odpowiednie wartości. Jeśli nie masz pliku CSV, a utworzony eksperymentu predykcyjnej przy użyciu Studio uczenia maszynowego, można pobrać zestawu danych dla eksperymentu predykcyjnych i używać go.

Aby pobrać zestaw danych, należy otworzyć Studio uczenia maszynowego. Otwórz eksperymentu predykcyjnych i kliknij prawym przyciskiem myszy dane wejściowe dla eksperymentu. Z menu kontekstowego wybierz opcję **zestawu danych** , a następnie wybierz **Pobierz**.

![Wdrażanie usługi sieci web](./media/machine-learning-publish-a-machine-learning-web-service/figure-7-mls-download.png)

Kliknij przycisk **Testuj**. Wyświetla stan zadania wsadowe w prawo w obszarze **Badań zadań wsadowych**.

![Wdrażanie usługi sieci web](./media/machine-learning-publish-a-machine-learning-web-service/figure-6-test-batch-execution.png)

<!--![Test the web service](./media/machine-learning-publish-a-machine-learning-web-service/figure-3.png)-->

Na stronie **konfiguracji** można zmienić opis, tytuł, zaktualizować magazyn kluczy i włączyć przykładowe dane do usługi sieci web.

![Konfigurowanie usługi sieci web](./media/machine-learning-publish-a-machine-learning-web-service/figure-8-arm-configure.png)

Po wdrożeniu usługi sieci web, możesz:

- **Dostęp** przez interfejs API usługi sieci web.
- **Zarządzaj** go za pośrednictwem portalu usługi usługą sieci web lub wersji zapoznawczej.
- **Aktualizacja** to zmiana modelu.

### <a name="access-the-web-service"></a>Dostęp do usługi sieci web

Po rozmieszczeniu usługi sieci web z Studio uczenia maszynowego, można wysłać dane do usługi i otrzymywać odpowiedzi programowo.

Strona **zużywają** zawiera wszystkie informacje potrzebne do połączenia się z usługą sieci web. Na przykład klucz API zapewnia Zezwalaj autoryzowany dostęp do usługi.

Aby uzyskać więcej informacji dotyczących uzyskiwania dostępu do usługi sieci web uczenie maszynowe zobacz [sposób wykorzystywania wdrożonej usługą sieci Web usługi sieci web](machine-learning-consume-web-services.md).

### <a name="manage-your-new-web-service"></a>Zarządzanie nowej usługi sieci web

Można zarządzać portalu usług sieci Web Learning komputera usługi sieci web klasyczny. Ze [strony głównej portalu](https://services.azureml-test.net/)kliknij opcję **Usługi sieci Web**. Ze strony usługi sieci web można usunąć lub skopiować usługi. Aby monitorować określonej usługi, kliknij usługę, a następnie kliknij **pulpit nawigacyjny**. Aby monitorować zadania wsadowe związane z usługą sieci web, kliknij przycisk **Dziennik żądania partii**.

## <a name="deploy-the-predictive-experiment-as-a-classic-web-service"></a>Wdrażanie cennik jako usługi sieci web Classic

Teraz, gdy cennik został wystarczająco przygotowany, można wdrożyć go jako usługi sieci web Azure. Przy użyciu usługi sieci web, użytkownicy mogą wysyłać dane do modelu i modelu zwróci jego prognoz.

Aby wdrożyć eksperymentu predykcyjnej, kliknij polecenie **Uruchom** , w dolnej części obszaru roboczego eksperymentu, a następnie kliknij przycisk **Wdrożyć usługę sieci Web**. Usługa sieci web jest skonfigurowany i są umieszczane na pulpicie nawigacyjnym usługi sieci web.

![Wdrażanie usługi sieci web](./media/machine-learning-publish-a-machine-learning-web-service/figure-2.png)

Możesz sprawdzić tę usługę sieci web w portalu usług sieci Web Learning maszyny lub Studio uczenia maszynowego.

Aby przetestować odpowiedź żądania usługi sieci web, kliknij przycisk **Test** na pulpicie nawigacyjnym usługi sieci web. Okno dialogowe pojawi się monit o podanie danych wejściowych dla usługi. Są to kolumny oczekiwanego przez punktów końcowych. Wprowadź zestaw danych, a następnie kliknij przycisk **OK**. Wyniki generowane przez usługę sieci web są wyświetlane w dolnej części pulpitu nawigacyjnego.

Kliknięcie Podgląd łącza **Test** do testowania usługi w portalu Azure Machine Learning w sieci Web usług, jak pokazano wcześniej w sekcji usługi sieci web.

Aby przetestować usługa wykonywania partii, kliknij łącze Podgląd **Test** . Na stronie testowej wsadowego kliknij Przeglądaj w obszarze dane wejściowe i wybierz plik CSV zawierający przykładowe odpowiednie wartości. Jeśli nie masz pliku CSV, a utworzony eksperymentu predykcyjnej przy użyciu Studio uczenia maszynowego, można pobrać zestawu danych dla eksperymentu predykcyjnych i używać go.

![Test usługi sieci web](./media/machine-learning-publish-a-machine-learning-web-service/figure-3.png)

Na stronie **Konfiguracja** można zmienić nazwę wyświetlaną usługi i nadać jej opis. Nazwa i opis jest wyświetlany w [wersji zapoznawczej](http://manage.windowsazure.com/) gdzie zarządzania usługami internetowymi.

Można podać opis dla danych wejściowych, danych wyjściowych i parametry usługi sieci web, wpisując ciąg dla każdej kolumny w **SCHEMACIE WEJŚCIOWYM**, **Schemat danych wyjściowych**i **PARAMETR usługi sieci Web**. Opisy te są używane w dokumentacji kodu próbki świadczone usługi sieci web.

Można włączyć rejestrowanie, aby zdiagnozować wszelkie błędy, które są wyświetlane podczas uzyskiwania dostępu do usługi sieci web. Aby uzyskać więcej informacji zobacz [Włączanie rejestrowania dla usługi sieci web uczenie maszynowe](machine-learning-web-services-logging.md).

![Konfigurowanie usługi sieci web](./media/machine-learning-publish-a-machine-learning-web-service/figure-4.png)

Można również skonfigurować punkty końcowe dla usługi sieci web w portalu usług sieci Web Azure Machine Learning podobna do procedury przedstawione wcześniej w sekcji usługi sieci web. Opcje są różne, można dodać lub zmienić opis usługi, Włącz rejestrowanie i Włącz przykładowe dane do testowania.

### <a name="access-the-web-service"></a>Dostęp do usługi sieci web

Po rozmieszczeniu usługi sieci web z Studio uczenia maszynowego, można wysłać dane do usługi i otrzymywać odpowiedzi programowo.

Pulpit nawigacyjny zawiera wszystkie informacje potrzebne do połączenia się z usługą sieci web. Na przykład klucz API jest zapewniane w celu umożliwienia autoryzowany dostęp do usługi i stron pomocy API są dostarczane w celu możesz rozpocząć pisanie kodu.

Aby uzyskać więcej informacji dotyczących uzyskiwania dostępu do usługi sieci web uczenie maszynowe zobacz [sposób wykorzystywania wdrożonej usługą sieci Web usługi sieci web](machine-learning-consume-web-services.md).

### <a name="manage-the-web-service"></a>Zarządzanie usługą sieci web

Istnieją różne akcje możesz wykonać do monitorowania usługi sieci web. Można zaktualizować ją i usuń go. Można również dodać dodatkowe punkty końcowe do usługi sieci web Classic oprócz domyślnego punktu końcowego, który jest tworzony podczas jej wdrażania.

Aby uzyskać więcej informacji zobacz [Zarządzanie usługą sieci Web obszaru roboczego](machine-learning-manage-workspace.md) i [Zarządzaj usługi sieci web przy użyciu usługi sieci Web Azure Machine Learning portal](machine-learning-manage-new-webservice.md).

<!-- When this article gets published, fix the link and uncomment
For more information on how to manage Azure Machine Learning web service endpoints using the REST API, see **Azure machine learning web service endpoints**.
-->

## <a name="update-the-web-service"></a>Aktualizacja usługi sieci web

Można wprowadzić zmiany do usługi sieci web, takich jak aktualizowania modelu z danymi dodatkowe szkolenie i wdrażać go ponownie, zastąpienie oryginalnego usługi sieci web.

Aby zaktualizować usługę sieci web, otwórz oryginalny cennik użyta podczas wdrażania usługi sieci web i wprowadź edytowalnej kopii, klikając przycisk **ZAPISZ jako**. Wprowadź żądane zmiany, a następnie kliknij przycisk **Wdrożyć usługę sieci Web**.

Ponieważ zostały rozmieszczone tego doświadczenia przed, są pytanie, czy chcesz zastąpić (klasyczny Web Service) lub istniejącej usługi aktualizacji (Nowa usługa sieci web). Klikając przycisk **Tak** lub **Aktualizacja** zatrzymuje istniejącej usługi sieci web i wdraża nowy cennik jest wdrożony w jej miejsce.

> [AZURE.NOTE] Jeśli wprowadzono zmiany w konfiguracji w oryginalnym usługi sieci web, na przykład wpisać nową nazwę wyświetlaną lub opis, trzeba będzie ponownie wprowadzić te wartości.

Jedną z opcji aktualizowania usługi sieci web jest przekwalifikowaniu modelu programowo. Aby uzyskać więcej informacji zobacz [Ponowne próbkowanie uczących modele programowo](machine-learning-retrain-models-programmatically.md).


<!-- internal links -->
[Utwórz eksperyment szkolenia]: #create-a-training-experiment
[Przekonwertuj go na cennik]: #convert-the-training-experiment-to-a-predictive-experiment
[Nowy]: #deploy-the-predictive-experiment-as-a-new-Web-service
[klasyczny]: #deploy-the-predictive-experiment-as-a-new-Web-service
[Access]: #access-the-Web-service
[Manage]: #manage-the-Web-service-in-the-azure-management-portal
[Update]: #update-the-Web-service
