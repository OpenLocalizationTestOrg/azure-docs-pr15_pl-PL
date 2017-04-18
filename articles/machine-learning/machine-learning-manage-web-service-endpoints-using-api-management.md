<properties
    pageTitle="Dowiedz się, jak zarządzać AzureML usług sieci web przy użyciu interfejsu API zarządzania | Microsoft Azure"
    description="Przewodnik po przedstawiający, jak zarządzać AzureML usług sieci web przy użyciu interfejsu API zarządzania."
    keywords="maszynowego uczenia interfejsu api zarządzania"
    services="machine-learning"
    documentationCenter=""
    authors="roalexan"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="roalexan" />


# <a name="learn-how-to-manage-azureml-web-services-using-api-management"></a>Dowiedz się, jak zarządzać AzureML usług sieci web przy użyciu interfejsu API zarządzania

##<a name="overview"></a>Omówienie

Ten przewodnik pokazano, jak szybko rozpocząć pracę przy użyciu interfejsu API zarządzania Zarządzanie AzureML usług sieci web.

##<a name="what-is-azure-api-management"></a>Co to jest zarządzanie interfejsu API Azure?

Azure Zarządzanie interfejsu API jest usługą Azure, która umożliwia zarządzanie punktów końcowych interfejsu API usługi REST, definiując dostępu użytkowników, ograniczania użycia i monitorowanie pulpitu nawigacyjnego. Kliknij [tutaj](https://azure.microsoft.com/services/api-management/) szczegółowe informacje na temat zarządzania interfejsu API Azure. Kliknij [tutaj](../api-management/api-management-get-started.md) przewodnika na temat wprowadzenie do zarządzania interfejsu API Azure. Ten innych podręcznik, na podstawie tego przewodnika, obejmuje więcej tematów, w tym konfiguracji powiadomień, ceny warstwy, Obsługa odpowiedzi, uwierzytelnianie użytkownika tworzenia produktów, subskrypcje Deweloper i zastosowania dashboarding.

##<a name="what-is-azureml"></a>Co to jest AzureML?

AzureML jest usługą Azure dla maszynowego uczenia, który umożliwia łatwe budowania, wdrażania i udostępnianie rozwiązań zaawansowanej analizy. Kliknij [tutaj](https://azure.microsoft.com/services/machine-learning/) szczegółowe informacje na temat AzureML.

##<a name="prerequisites"></a>Wymagania wstępne

Aby wykonać ten przewodnik, należy następująco:

* Konto Azure. Jeśli nie masz konta usługi Azure, kliknij [tutaj](https://azure.microsoft.com/pricing/free-trial/) Aby uzyskać szczegółowe informacje na temat tworzenia bezpłatne konto wersji próbnej.
* Konto AzureML. Jeśli nie masz konta AzureML, kliknij [tutaj](https://studio.azureml.net/) Aby uzyskać szczegółowe informacje na temat tworzenia bezpłatne konto wersji próbnej.
* Obszar roboczy, usługi i api_key dla doświadczenia AzureML rozmieszczony jako usługi sieci web. Kliknij [tutaj](machine-learning-create-experiment.md) Aby uzyskać szczegółowe informacje na temat tworzenia doświadczenia AzureML. Kliknij [tutaj](machine-learning-publish-a-machine-learning-web-service.md) Aby uzyskać szczegółowe informacje na temat instalacji doświadczenia AzureML jako usługi sieci web. Alternatywnie dodatku A zawiera instrukcje dotyczące tworzenia i przetestuj eksperyment AzureML prosty i wdrożyć go jako usługi sieci web.

##<a name="create-an-api-management-instance"></a>Tworzenie wystąpienia interfejsu API zarządzania

Poniżej przedstawiono czynności umożliwiające zarządzanie AzureML usługi sieci web za pomocą interfejsu API zarządzania. Najpierw utwórz wystąpienie usługi. Zaloguj się do [Portalu klasyczny](https://manage.windowsazure.com/) i kliknij przycisk **Nowy** > **Aplikacji usług** > **Interfejsu API zarządzania** > **Tworzenie**.

![Utwórz wystąpienie](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-instance.png)

Określ unikatową **adresu URL**. Ten przewodnik używa **demoazureml** — należy wybrać inny. Wybierz odpowiedni **subskrypcji** i **Region** wystąpienia usługi. Po wprowadzeniu odpowiednie opcje, kliknij przycisk Dalej.

![Tworzenie Usługa-1](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-service-1.png)

Określenie wartości w polu **Nazwa organizacji**. Ten przewodnik używa **demoazureml** — należy wybrać inny. Wprowadź swój adres e-mail w polu **adres e-mail administratora** . Ten adres e-mail jest używany powiadomienia z systemu zarządzania interfejsu API.

![Tworzenie usługa-2](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-service-2.png)

Kliknij pole wyboru, aby utworzyć wystąpienia usługi. *Trwa nową usługę, aby można utworzyć do 30 minut*.

##<a name="create-the-api"></a>Tworzenie API

Po utworzeniu wystąpienia usługi następnym krokiem jest utworzenie interfejsu API. Interfejs API składa się z zestawu działań, które mogą być wywoływane z aplikacji klienckiej. Operacje interfejsu API są proxy do istniejących usług sieci web. Ten przewodnik tworzy interfejsy API tego serwera proxy do istniejących zasobów AzureML i BES usług sieci web.

Interfejsy API są utworzona i skonfigurowana z portalu programu publisher interfejsu API jest dostępna za pośrednictwem portalu klasyczny Azure. Aby uzyskać dostęp do portalu programu publisher, wybierz pozycję wystąpienia usługi.

![Wybierz pozycję Usługa wystąpienia](./media/machine-learning-manage-web-service-endpoints-using-api-management/select-service-instance.png)

Kliknij przycisk **Zarządzaj** w portalu klasyczny Azure dotyczące usługi zarządzania interfejsu API.

![Usługa Zarządzanie](./media/machine-learning-manage-web-service-endpoints-using-api-management/manage-service.png)

Kliknij pozycję **interfejsy API** z **Interfejsu API zarządzania** menu po lewej stronie, a następnie kliknij **Dodaj interfejsu API**.

![Interfejs API zarządzania menu](./media/machine-learning-manage-web-service-endpoints-using-api-management/api-management-menu.png)

Wpisz nazwę **Interfejsu API pokaz AzureML** jako **nazwę interfejs API sieci Web**. Wpisz **https://ussouthcentral.services.azureml.net** jako adres **URL usługi sieci Web**. Wpisz **Pokaz azureml** jako **sufiks adresu URL interfejsu API sieci Web**. Zaznacz pole wyboru **HTTPS** jako schemat **Adres URL interfejsu API sieci Web** . Wybierz pozycję **Starter** jako **produktów**. Po zakończeniu kliknij pozycję **Zapisz** , aby utworzyć API.

![Dodawanie nowego — interfejs API języka](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-new-api.png)

##<a name="add-the-operations"></a>Dodawanie operacji

Kliknij pozycję **operacji dodawania** , aby dodać operacje do tego interfejsu API.

![Dodawanie operacji](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-operation.png)

Pojawi się okno **Nowa operacja** i na karcie **Podpis** zostanie wybrana domyślnie.

##<a name="add-rrs-operation"></a>Dodawanie Rekordów operacji

Najpierw należy utworzyć operacji usługi AzureML rekordy. Wybierz przycisk **OPUBLIKUJ** jako **Zlecenie HTTP**. Typ **/services/ /workspaces/ {obszaru roboczego} {usługi}-wersji execute?api = {apiversion} & Szczegóły = {Szczegóły}** jako **adres URL szablonu**. Wpisz nazwę **Wykonywanie RR** jako **Nazwa wyświetlana**.

![Dodawanie RR operacja podpisu](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-rrs-operation-signature.png)

Kliknij pozycję **odpowiedzi** > **dodać** z lewej i wybierz pozycję **200 OK**. Kliknij przycisk **Zapisz** , aby zapisać tej operacji.

![Dodawanie RR operacja odpowiedzi](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-rrs-operation-response.png)

##<a name="add-bes-operations"></a>Dodawanie operacji BES

Zrzuty ekranu nie są uwzględniane dla operacji BES, są bardzo podobne do tych dodawania operacji rekordy zasobów.

###<a name="submit-but-not-start-a-batch-execution-job"></a>Przesyłanie (ale nie zaczynają) zadanie wsadowe

Kliknij przycisk **operacji dodawania** , aby dodać operację AzureML BES API. Zaznacz **WPIS** **Zlecenie HTTP**. Typ **/services/ /workspaces/ {obszaru roboczego} {usługi}-wersji jobs?api = {apiversion}** **adres URL szablonu**. Wpisz **BES przesyłanie** jako **Nazwa wyświetlana**. Kliknij pozycję **odpowiedzi** > **dodać** z lewej i wybierz pozycję **200 OK**. Kliknij przycisk **Zapisz** , aby zapisać tej operacji.

###<a name="start-a-batch-execution-job"></a>Uruchom zadanie wsadowe

Kliknij przycisk **operacji dodawania** , aby dodać operację AzureML BES API. Zaznacz **WPIS** **Zlecenie HTTP**. Typ **/jobs/ /services/ {usługi} /workspaces/ {obszaru roboczego} {jobid}-wersji start?api = {apiversion}** **adres URL szablonu**. Wpisz **Uruchom BES** jako **Nazwa wyświetlana**. Kliknij pozycję **odpowiedzi** > **dodać** z lewej i wybierz pozycję **200 OK**. Kliknij przycisk **Zapisz** , aby zapisać tej operacji.

###<a name="get-the-status-or-result-of-a-batch-execution-job"></a>Uzyskiwanie stan lub wynik zadania wsadowe

Kliknij przycisk **operacji dodawania** , aby dodać operację AzureML BES API. Wybierz pozycję **Pobierz** **Zlecenie HTTP**. Typ **/workspaces/ {obszaru roboczego} /services/ {usługi} {jobid} /jobs/ ?api wersji = {apiversion}** **adres URL szablonu**. **Stan BES** wpisz **nazwę wyświetlaną**. Kliknij pozycję **odpowiedzi** > **dodać** z lewej i wybierz pozycję **200 OK**. Kliknij przycisk **Zapisz** , aby zapisać tej operacji.

###<a name="delete-a-batch-execution-job"></a>Usuwanie zadania wsadowe

Kliknij przycisk **operacji dodawania** , aby dodać operację AzureML BES API. Dla **zlecenia HTTP**, wybierz pozycję **Usuń** . Typ **/workspaces/ {obszaru roboczego} /services/ {usługi} {jobid} /jobs/ ?api wersji = {apiversion}** **adres URL szablonu**. Wpisz **BES Usuń** **nazwę wyświetlaną**. Kliknij pozycję **odpowiedzi** > **dodać** z lewej i wybierz pozycję **200 OK**. Kliknij przycisk **Zapisz** , aby zapisać tej operacji.

##<a name="call-an-operation-from-the-developer-portal"></a>Połączenie operacji z portalu — Deweloper

Operacje można wywołać bezpośrednio z poziomu portalu Deweloper, który zapewnia wygodny sposób, aby wyświetlić i przetestować operacje interfejsu API. W tym kroku przewodnik nawiąże połączenie **Wykonywanie RR** metody, dodany do **Interfejsu API pokaz AzureML**. Kliknij pozycję **dzięki portalowi deweloperów** z menu u góry prawej strony portalu klasyczny.

![portal dla programistów](./media/machine-learning-manage-web-service-endpoints-using-api-management/developer-portal.png)

Kliknij pozycję **interfejsy API** w górnym menu, a następnie kliknij **Interfejsu API pokaz AzureML** , aby wyświetlić dostępne operacje.

![Interfejs api demoazureml](./media/machine-learning-manage-web-service-endpoints-using-api-management/demoazureml-api.png)

Wybierz pozycję **Rr wykonywanie** operacji. Kliknij pozycję **Wypróbuj**.

![Spróbuj it](./media/machine-learning-manage-web-service-endpoints-using-api-management/try-it.png)

Parametrów żądania wpisz do **obszaru roboczego**, **usługi**, **2.0** dla **apiversion**i **PRAWDA** **Szczegóły**. Możesz znaleźć do **obszaru roboczego** i **usług** w AzureML usługi sieci web z pulpitu nawigacyjnego (patrz **testowych usługi sieci web** w dodatku A).

Dla nagłówków żądania, kliknij pozycję **Dodaj nagłówek** i wpisz **Typ zawartości** i **aplikacji i json**, a następnie kliknij pozycję **Dodaj nagłówek** i wpisz **autoryzacji** i **okaziciela <YOUR AZUREML SERVICE API-KEY> **. Możesz znaleźć swój **klucz interfejsu api** w AzureML usługi sieci web z pulpitu nawigacyjnego (patrz **Test usługi sieci web** w dodatku A).

Typ **{"Wartości wejściowych": {"input1": {"ColumnNames": ["Kol2"], "wartości": [["to jest dobrym dnia"]]}}, "GlobalParameters": {}}** dla treści żądania.

![azureml pokaz api](./media/machine-learning-manage-web-service-endpoints-using-api-management/azureml-demo-api.png)

Kliknij przycisk **Wyślij**.

![Wyślij](./media/machine-learning-manage-web-service-endpoints-using-api-management/send.png)

Po wywołaniu operacji, dzięki portalowi deweloperów Wyświetla **Zażądania adres URL** z usługi wewnętrznej, **Stan odpowiedzi**, **nagłówki odpowiedzi**i każda **zawartość odpowiedzi**.

![Stan odpowiedzi](./media/machine-learning-manage-web-service-endpoints-using-api-management/response-status.png)

##<a name="appendix-a---creating-and-testing-a-simple-azureml-web-service"></a>Dodatek A — tworzenie i testowanie proste AzureML usługi sieci web

###<a name="creating-the-experiment"></a>Tworzenie doświadczenia

Poniżej przedstawiono instrukcje dotyczące tworzenia prostej doświadczenia AzureML i wdrażanie go jako usługi sieci web. Ma usługi sieci web, jakie dane wejściowe kolumny dowolnego tekstu i zwraca zestaw funkcji, wyrażone są liczbami całkowitymi. Na przykład:

Tekst | Tekst mieszanych
--- | ---
Jest to dobre dnia | 1 1 2 2 0 2 0 1

Najpierw za pomocą przeglądarki wybranych przez użytkownika, przejdź do: [https://studio.azureml.net/](https://studio.azureml.net/) i wprowadź poświadczenia, aby się zalogować. Następnie utwórz nowy, pusty doświadczenia.

![Wyszukiwanie doświadczenia szablonów](./media/machine-learning-manage-web-service-endpoints-using-api-management/search-experiment-templates.png)

Zmień jego nazwę na **SimpleFeatureHashingExperiment**. Rozwiń **Zapisane zestawy danych** i przeciągnij **Przeglądy książki z Amazon** swojego doświadczenia.

![proste — funkcja mieszania doświadczenia](./media/machine-learning-manage-web-service-endpoints-using-api-management/simple-feature-hashing-experiment.png)

Rozwiń **Przekształcanie** i **manipulowania** i przeciągnij **Wybieranie kolumn w zestawie danych** do doświadczenia. **Przeglądy książki z Amazon** nawiązać **Zaznacz kolumny w zestawie danych**.

![Wybieranie kolumn](./media/machine-learning-manage-web-service-endpoints-using-api-management/project-columns.png)

Kliknij pozycję **Zaznacz kolumny w zestawie danych** , a następnie kliknij polecenie **Uruchamianie selektora kolumny** i wybierz **Kol2**. Kliknij znacznik wyboru, aby zastosować te zmiany.

![Wybieranie kolumn](./media/machine-learning-manage-web-service-endpoints-using-api-management/select-columns.png)

Rozwiń węzeł **Analizy tekstu** , a następnie przeciągnij na **Funkcji mieszania** doświadczenia. **Zaznacz kolumny w zestawie danych** połączyć się z **funkcją mieszania**.

![Łączenie projektu — kolumn](./media/machine-learning-manage-web-service-endpoints-using-api-management/connect-project-columns.png)

Wpisz **3** **Hashing bitsize**. Spowoduje to utworzenie 8 (23) kolumn.

![Mieszanie bitsize](./media/machine-learning-manage-web-service-endpoints-using-api-management/hashing-bitsize.png)

Na tym etapie warto kliknij polecenie **Uruchom** , aby przetestować doświadczenia.

![Uruchamianie](./media/machine-learning-manage-web-service-endpoints-using-api-management/run.png)

###<a name="create-a-web-service"></a>Tworzenie usługi sieci web

Teraz można utworzyć usługi sieci web. Rozwiń **Usługi sieci Web** i przeciągnij do doświadczenia **wprowadzania** . Nawiązywanie połączenia **danych wejściowych** z **funkcji mieszania**. Również przeciągnij **dane wyjściowe** z doświadczenia. **Dane wyjściowe** nawiązać **mieszania w funkcji**.

![dane wyjściowe do funkcji mieszania](./media/machine-learning-manage-web-service-endpoints-using-api-management/output-to-feature-hashing.png)

Kliknij przycisk **Publikuj usługi sieci web**.

![Publikowanie — Usługa sieci web](./media/machine-learning-manage-web-service-endpoints-using-api-management/publish-web-service.png)

Kliknij przycisk **Tak,** Aby opublikować doświadczenia.

![tak publikowanie](./media/machine-learning-manage-web-service-endpoints-using-api-management/yes-to-publish.png)

###<a name="test-the-web-service"></a>Testowanie usługi sieci web

Usługi sieci web AzureML składa się z RSS (usługa żądanie/odpowiedź) i punkty końcowe (usługa wykonanie partii) BES. RSS jest wykonywania synchroniczne. BES jest do wykonywania zadań asynchroniczne. Aby przetestować usługi sieci web ze źródłem Python przykładowe poniżej, może być konieczne Pobierz i zainstaluj zestaw SDK Azure dla Python (zobacz: [jak zainstalować Python](../python-how-to-install.md)).

Należy także **obszaru roboczego**, **usługi**i **api_key** z doświadczenia poniżej źródła próbki. Obszar roboczy i usług można znaleźć, klikając pozycję **Żądanie-odpowiedź** lub **Wsadowe** dla swojego doświadczenia na pulpicie nawigacyjnym usługi sieci web.

![Znajdź obszar roboczy i usługi](./media/machine-learning-manage-web-service-endpoints-using-api-management/find-workspace-and-service.png)

**Api_key** można znaleźć, klikając pozycję usługi doświadczenia na pulpicie nawigacyjnym usługi sieci web.

![Znajdowanie interfejsu api klucza](./media/machine-learning-manage-web-service-endpoints-using-api-management/find-api-key.png)

####<a name="test-rrs-endpoint"></a>Test RR końcowy

#####<a name="test-button"></a>Przycisk testu

Łatwym sposobem sprawdzenia punkt końcowy rr jest kliknij **Testowanie** na pulpicie nawigacyjnym usługi sieci web.

![Test](./media/machine-learning-manage-web-service-endpoints-using-api-management/test.png)

Wpisz **to dobre dnia** **Kol2**. Kliknij znacznik wyboru.

![wprowadzanie danych](./media/machine-learning-manage-web-service-endpoints-using-api-management/enter-data.png)

Pojawi się podobną do

![Przykładowe dane wyjściowe](./media/machine-learning-manage-web-service-endpoints-using-api-management/sample-output.png)

#####<a name="sample-code"></a>Przykładowy kod

Aby przetestować z Rekordów w inny sposób jest w kodzie klienta. Jeśli klikniesz **żądanie i odpowiedź** na pulpicie nawigacyjnym i przewiń do dołu, przykładowy kod będzie widoczny dla C#, Python i R. Pojawi się także na temat składni żądania rekordy, w tym identyfikator URI, żądania nagłówki i treści.

Ten przewodnik zawiera pracy przykład Python. Należy zmodyfikować go za pomocą **obszaru roboczego**, **usługi**i **api_key** z doświadczenia.

    import urllib2
    import json
    workspace = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE WORKSPACE ID>"
    service = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE SERVICE ID>"
    api_key = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE API KEY>"
    data = {
    "Inputs": {
        "input1": {
            "ColumnNames": ["Col2"],
            "Values": [ [ "This is a good day" ] ]
        },
    },
    "GlobalParameters": { }
    }
    url = "https://ussouthcentral.services.azureml.net/workspaces/" + workspace + "/services/" + service + "/execute?api-version=2.0&details=true"
    headers = {'Content-Type':'application/json', 'Authorization':('Bearer '+ api_key)}
    body = str.encode(json.dumps(data))
    try:
        req = urllib2.Request(url, body, headers)
        response = urllib2.urlopen(req)
        result = response.read()
        print "result:" + result
            except urllib2.HTTPError, error:
        print("The request failed with status code: " + str(error.code))
        print(error.info())
        print(json.loads(error.read()))

####<a name="test-bes-endpoint"></a>Test BES końcowy
Kliknij pozycję **wsadowe** pulpitu nawigacyjnego i przewiń w dół. Przykładowy kod będzie widoczny dla C#, Python i R. Pojawi się także na temat składni żądania BES Prześlij zadanie, rozpoczęcia zadania, się stanu lub wyników zadania i usuwanie zadania.

Ten przewodnik zawiera pracy przykład Python. Należy zmodyfikować go za pomocą **obszaru roboczego**, **usługi**i **api_key** z doświadczenia. Ponadto należy zmodyfikować **nazwę konta magazynu**, **miejsca do magazynowania klucz konta**i **nazwę kontenera magazynu**. Ponadto będzie konieczne modyfikowanie lokalizacji **pliku wejściowego** i lokalizacji **pliku wyjściowego**.

    import urllib2
    import json
    import time
    from azure.storage import *
    workspace = "<REPLACE WITH YOUR WORKSPACE ID>"
    service = "<REPLACE WITH YOUR SERVICE ID>"
    api_key = "<REPLACE WITH THE API KEY FOR YOUR WEB SERVICE>"
    storage_account_name = "<REPLACE WITH YOUR AZURE STORAGE ACCOUNT NAME>"
    storage_account_key = "<REPLACE WITH YOUR AZURE STORAGE KEY>"
    storage_container_name = "<REPLACE WITH YOUR AZURE STORAGE CONTAINER NAME>"
    input_file = "<REPLACE WITH THE LOCATION OF YOUR INPUT FILE>" # Example: C:\\mydata.csv
    output_file = "<REPLACE WITH THE LOCATION OF YOUR OUTPUT FILE>" # Example: C:\\myresults.csv
    input_blob_name = "mydatablob.csv"
    output_blob_name = "myresultsblob.csv"
    def printHttpError(httpError):
    print("The request failed with status code: " + str(httpError.code))
    print(httpError.info())
    print(json.loads(httpError.read()))
    return
    def saveBlobToFile(blobUrl, resultsLabel):
    print("Reading the result from " + blobUrl)
    try:
        response = urllib2.urlopen(blobUrl)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    with open(output_file, "w+") as f:
        f.write(response.read())
    print(resultsLabel + " have been written to the file " + output_file)
    return
    def processResults(result):
    first = True
    results = result["Results"]
    for outputName in results:
        result_blob_location = results[outputName]
        sas_token = result_blob_location["SasBlobToken"]
        base_url = result_blob_location["BaseLocation"]
        relative_url = result_blob_location["RelativeLocation"]
        print("The results for " + outputName + " are available at the following Azure Storage location:")
        print("BaseLocation: " + base_url)
        print("RelativeLocation: " + relative_url)
        print("SasBlobToken: " + sas_token)
        if (first):
            first = False
            url3 = base_url + relative_url + sas_token
            saveBlobToFile(url3, "The results for " + outputName)
    return

    def invokeBatchExecutionService():
    url = "https://ussouthcentral.services.azureml.net/workspaces/" + workspace +"/services/" + service +"/jobs"
    blob_service = BlobService(account_name=storage_account_name, account_key=storage_account_key)
    print("Uploading the input to blob storage...")
    data_to_upload = open(input_file, "r").read()
    blob_service.put_blob(storage_container_name, input_blob_name, data_to_upload, x_ms_blob_type="BlockBlob")
    print "Uploaded the input to blob storage"
    input_blob_path = "/" + storage_container_name + "/" + input_blob_name
    connection_string = "DefaultEndpointsProtocol=https;AccountName=" + storage_account_name + ";AccountKey=" + storage_account_key
    payload =  {
        "Input": {
            "ConnectionString": connection_string,
            "RelativeLocation": input_blob_path
        },
        "Outputs": {
            "output1": { "ConnectionString": connection_string, "RelativeLocation": "/" + storage_container_name + "/" + output_blob_name },
        },
        "GlobalParameters": {
        }
    }
        body = str.encode(json.dumps(payload))
    headers = { "Content-Type":"application/json", "Authorization":("Bearer " + api_key)}
    print("Submitting the job...")
    # submit the job
    req = urllib2.Request(url + "?api-version=2.0", body, headers)
    try:
        response = urllib2.urlopen(req)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    result = response.read()
    job_id = result[1:-1] # remove the enclosing double-quotes
    print("Job ID: " + job_id)
    # start the job
    print("Starting the job...")
    req = urllib2.Request(url + "/" + job_id + "/start?api-version=2.0", "", headers)
    try:
        response = urllib2.urlopen(req)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    url2 = url + "/" + job_id + "?api-version=2.0"

    while True:
        print("Checking the job status...")
        # If you are using Python 3+, replace urllib2 with urllib.request in the follwing code
        req = urllib2.Request(url2, headers = { "Authorization":("Bearer " + api_key) })
        try:
            response = urllib2.urlopen(req)
        except urllib2.HTTPError, error:
            printHttpError(error)
            return
        result = json.loads(response.read())
        status = result["StatusCode"]
        print "status:" + status
        if (status == 0 or status == "NotStarted"):
            print("Job " + job_id + " not yet started...")
        elif (status == 1 or status == "Running"):
            print("Job " + job_id + " running...")
        elif (status == 2 or status == "Failed"):
            print("Job " + job_id + " failed!")
            print("Error details: " + result["Details"])
            break
        elif (status == 3 or status == "Cancelled"):
            print("Job " + job_id + " cancelled!")
            break
        elif (status == 4 or status == "Finished"):
            print("Job " + job_id + " finished!")
            processResults(result)
            break
        time.sleep(1) # wait one second
    return
    invokeBatchExecutionService()
