<properties
    pageTitle="Nawiązywanie połączenia z usługi sieci Web uczenia | Microsoft Azure"
    description="C# lub Python nawiązywanie połączenia z usługą Azure maszynowego Learning w sieci Web przy użyciu klucza autoryzacji."
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016" 
    ms.author="garye" />


# <a name="connect-to-an-azure-machine-learning-web-service"></a>Nawiązywanie połączenia z usługą sieci Web uczenia Azure komputera

Deweloper Azure maszynowego szkoleniowej jest interfejs API usług sieci Web Aby przewidywań wprowadzania danych w czasie rzeczywistym lub w trybie partię. Azure maszynowego uczenia Studio umożliwia tworzenie przewidywań i wdrażanie usługi Azure maszynowego uczenia w sieci Web.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Aby uzyskać informacje o tworzenie i wdrażanie usługi komputera Learning w sieci Web przy użyciu komputera nauki Studio:

- Samouczek na temat tworzenia doświadczenia w komputerze nauki Studio zobacz [Tworzenie z pierwszym doświadczeniu](machine-learning-create-experiment.md).
- Aby uzyskać szczegółowe informacje na temat instalacji usługi sieci Web Zobacz temat [Deploy usługi sieci Web uczenia komputera](machine-learning-publish-a-machine-learning-web-service.md).
- Aby uzyskać więcej informacji na temat maszynowego uczenia, odwiedź [Centrum dokumentacji szkoleniowe komputera](https://azure.microsoft.com/documentation/services/machine-learning/).

## <a name="azure-machine-learning-web-service"></a>Usługa Azure maszynowego Learning w sieci Web ##

Za pomocą usługi sieci Web uczenia maszyny Azure zewnętrznej aplikacji komunikuje się z modelu wyników maszynowego uczenia przepływu pracy w czasie rzeczywistym. Połączenia usługi sieci Web uczenia maszyny zwraca wyniki przewidywania do aplikacji zewnętrznej. Aby usługa sieci Web uczenia maszynowego połączeń, należy przekazać klucz interfejsu API utworzonym przy umieszczaniu przewidywania. Usługi sieci Web uczenia maszynowego jest oparty na pozostałe wybranej popularne Architektura programowania projektów sieci Web.

Azure nauki komputera zawiera dwa typy usług:

- Żądanie odpowiedź usługi (RR) — krótki czas oczekiwania, wysoce skalowalna usługa, która stanowi interfejs do stateless modeli utworzone i wdrożone z Studio nauki komputera.
- Partia wykonanie usługi (BES) — asynchroniczne usługi tego wyniki w partii rekordów danych.

Aby uzyskać więcej informacji o usługach maszynowego Learning w sieci Web zobacz [Wdrażanie usługi sieci Web uczenia komputera](machine-learning-publish-a-machine-learning-web-service.md).

## <a name="get-an-azure-machine-learning-authorization-key"></a>Uzyskiwanie klucza autoryzacji nauki maszynowego Azure ##

Podczas wdrażania usługi doświadczenia generowane są klucze interfejsu API usługi sieci Web. Klucze można pobrać z kilku lokalizacjach.

## <a name="from-the-microsoft-azure-machine-learning-web-services-portal"></a>Z poziomu portalu usługi sieci Web nauki Microsoft Azure komputera

Zaloguj się do portalu [Usługi sieci Web nauki Microsoft Azure komputera](https://services.azureml.net) .

Aby pobrać klucz interfejsu API usługi nowego komputera Learning w sieci Web:

1. W portalu usługi sieci Web uczenia maszynowego Azure kliknij **Usług sieci Web** górnym menu.
2. Kliknij pozycję Usługa sieci Web, do której chcesz pobrać klucza.
3. W górnym menu kliknij pozycję **Consume**.
4. Skopiuj i Zapisz **Klucz podstawowy**.


Aby pobrać klucz interfejsu API usługi klasyczny maszynowego Learning w sieci Web:

1. W portalu usługi sieci Web uczenia maszynowego Azure kliknij **Klasyczny usług sieci Web** górnym menu.
2. Kliknij pozycję Usługa sieci Web, z którym pracujesz.
3. Kliknij punkt końcowy, dla której chcesz pobrać klucza.
3. W górnym menu kliknij pozycję **Consume**.
4. Skopiuj i Zapisz **Klucz podstawowy**.

## <a name="classic-web-service"></a>Klasyczny usługi sieci Web ##

 Można także pobrać klucz dla usługi sieci Web klasyczny z komputera nauki Studio lub Azure portal.

### <a name="machine-learning-studio"></a>Maszynowego uczenia Studio ###

1. W komputerze nauki Studio kliknij **Usług sieci WEB** po lewej stronie.
2. Kliknij pozycję usługi sieci Web. **Klucz interfejsu API** znajduje się na karcie **pulpitu NAWIGACYJNEGO** .

### <a name="azure-portal"></a>Azure portal ###

1. Kliknij pozycję **Nauki komputera** po lewej stronie.
2. Kliknij obszar roboczy, w którym znajduje się usługa sieci Web.
3. Kliknij pozycję **usług sieci WEB**.
4. Kliknij pozycję usługi sieci Web.
5. Kliknij punkt końcowy. "Klucz interfejsu API" nie działa w prawej dolnej części.

## <a id="connect"></a>Nawiązywanie połączenia z usługą maszynowego Learning w sieci Web

Umożliwia nawiązanie połączenia z usługą maszynowego Learning w sieci Web przy użyciu języka programowania, obsługującego żądania HTTP i odpowiedzi. Przykłady można wyświetlać na C#, Python i R ze strony sieci Web uczenia komputera usługi pomocy.

**Pomoc interfejsu API nauki komputera** Pomoc interfejsu API nauki komputera jest tworzona podczas wdrażania usługi sieci Web. Zobacz [Azure maszynowego uczenia Instruktaż wdrożenia usługi sieci Web](machine-learning-walkthrough-5-publish-web-service.md).
Pomoc interfejsu API nauki komputera zawiera szczegółowe informacje o przewidywania usługi sieci Web.

2. Kliknij pozycję Usługa sieci Web, z którym pracujesz.
3. Kliknij punkt końcowy, dla której chcesz wyświetlić stronę pomocy interfejsu API.
3. W górnym menu kliknij pozycję **Consume**.
3. Kliknij pozycję **Strona pomocy interfejsu API** pod żądanie odpowiedź lub wsadowe punkty końcowe.

**Do widoku interfejsu API maszynowego uczenia pomoc dotyczącą usługi sieci Web nowy**

Na komputerze Azure nauki portalu usługi sieci Web:

1. Kliknij przycisk **Usług sieci WEB** , w górnym menu.
2. Kliknij pozycję Usługa sieci Web, do której chcesz pobrać klucza.

Kliknij pozycję **Consume** uzyskać identyfikatory URI Reposonse żądania i usług wykonanie partii oraz przykładowy kod w C#, R i Python.

Kliknij pozycję **Swagger interfejsu API** uzyskiwania dokumentacji Swagger podstawie za pośrednictwem interfejsów API wezwaniem podanym identyfikatory URI.

### <a name="c-sample"></a>Przykładowe C# ###

Aby połączyć się z usługą maszynowego Learning w sieci Web, za pomocą **HttpClient** przechodzące ScoreData. ScoreData zawiera FeatureVector n wymiarowe wektorowa liczbowa funkcje reprezentujący ScoreData. Uwierzytelniania usługi szkoleniowe komputera przy użyciu klucza interfejsu API.

Aby połączyć się z usługą maszynowego Learning w sieci Web, musi być zainstalowany pakiet NuGet **Microsoft.AspNet.WebApi.Client** .

**Instalowanie Microsoft.AspNet.WebApi.Client NuGet w programie Visual Studio**

1. Publikowanie zestawów pobieranie danych z UCI: zestaw 2 dorosłego klasy danych usługi sieci Web.
2. Kliknij pozycję **Narzędzia** > **Menedżera pakietów NuGet** > **Konsoli Menedżera pakietów**.
2. Wybierz **pakiet instalacyjny Microsoft.AspNet.WebApi.Client**.

**Aby uruchomić przykładowy kod**

1. Publikowanie "Przykładowe 1: Pobierz zestaw danych ze UCI: zestaw danych klasy osoby 2" doświadczenia, część zbiór przykładowych nauki komputera.
2. Przypisywanie apiKey przy użyciu klucza z usługi sieci Web. Zobacz **Uzyskiwanie klucza autoryzacji Azure maszynowego uczenia** powyżej.
3. Przypisywanie serviceUri przy użyciu identyfikatora URI żądanie.


### <a name="python-sample"></a>Przykładowe Python ###

Aby połączyć się z usługą maszynowego Learning w sieci Web, użyć biblioteki **urllib2** przechodzące ScoreData. ScoreData zawiera FeatureVector n wymiarowe wektorowa liczbowa funkcje reprezentujący ScoreData. Uwierzytelniania usługi szkoleniowe komputera przy użyciu klucza interfejsu API.


**Aby uruchomić przykładowy kod**

1. Wdrażanie "Przykładowe 1: Pobierz zestaw danych ze UCI: zestaw danych klasy osoby 2" doświadczenia, część zbiór przykładowych nauki komputera.
2. Przypisywanie apiKey przy użyciu klucza z usługi sieci Web. Zobacz sekcję **uzyskania klucza autoryzacji Azure maszynowego uczenia** na początku tego artykułu.
3. Przypisywanie serviceUri przy użyciu identyfikatora URI żądanie.
