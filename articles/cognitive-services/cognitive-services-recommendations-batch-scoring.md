
<properties
    pageTitle="Wprowadzenie zalecenia partiami: komputer nauki interfejsu API zalecenia | Microsoft Azure"
    description="Azure maszynowego uczenia zalecenia — wprowadzenie zalecenia partiami"
    services="cognitive-services"
    documentationCenter=""
    authors="luiscabrer"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/17/2016"
    ms.author="luisca"/>

# <a name="get-recommendations-in-batches"></a>Uzyskaj zalecenia partiami

>[AZURE.NOTE] Pobieranie zalecenia partiami jest trudniejsze niż wprowadzenie zalecenia jednym naraz. Sprawdź interfejsy API, aby dowiedzieć się, jak uzyskać zalecenia dotyczące pojedynczego żądania:

> [Zalecenia dotyczące pozycji do pozycji](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3d4)<br>
> [Zalecenia element użytkownika](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3dd)
>
> Tylko wyników partii działa w przypadku kompilacjach, które zostały utworzone po 21 lipca 2016.


Istnieją sytuacje, w których jest potrzebne do uzyskania zalecenia dotyczące więcej niż jeden element naraz. Na przykład może Cię zainteresować tworzenia pamięci podręcznej zalecenia lub nawet analizowanie typy zaleceń, które występują.

Operacje wyników partię, nazywamy, są operacji asynchronicznych. Należy przesłać żądanie, poczekaj na zakończenie operacji i uzyskiwanie potrzebnych wyników.  

Bardziej precyzyjne, Oto Zrób tak:

1.  Tworzenie kontenera magazynu Azure, jeśli nie masz już.
2.  Przekaż plik wejściowy, który zawiera opis poszczególnych wezwaniach rekomendacji magazynem obiektów Blob platformy Azure.
3.  Zwiększ efektywność wyników zadaniu.
4.  Poczekaj na zakończenie operacji asynchronicznych.
5.  Po zakończeniu operacji gromadzenie wyników z magazynem obiektów Blob.

Przejdźmy przez jedno z następujących czynności.

## <a name="create-a-storage-container-if-you-dont-have-one-already"></a>Tworzenie kontenera miejsca do magazynowania, jeśli nie masz już

Przejdź do [portalu Azure](https://portal.azure.com) i Utwórz nowe konto miejsca do magazynowania, jeśli nie masz już. Aby to zrobić, przejdź do **nowej** > **danych** + **miejsca do magazynowania** > **Konta miejsca do magazynowania**.

Po utworzeniu konta miejsca do magazynowania, musisz tworzyć kontenery obiektów blob, w którym będzie przechowywana wejście i wyjście wsadowe.

Przekazywanie pliku wprowadzania danych, który opisuje każdej usługi rekomendacji żądania magazynem obiektów Blob — Przejdźmy zadzwonić input.json pliku.
Po umieszczeniu kontenera, musisz przekazać plik, który zawiera opis poszczególnych żądań, które należy wykonać w usłudze zalecenia.

Partię można wykonywać tylko jeden typ żądania z określonym kompilacji. Firma Microsoft będzie wyjaśniono, jak zdefiniować te informacje w następnej sekcji. Na obecnym Załóżmy się, że firma Microsoft będzie wykonywać zalecenia element z określonym kompilacji. Plik wejściowy zawiera następnie wprowadzania informacji (w tym przypadku nasion elementów) dla każdego żądania.

Oto przykładowy wygląd pliku input.json:

    {
      "requests": [
      { "SeedItems": [ "C9F-00163", "FKF-00689" ] },
      { "SeedItems": [ "F34-03453" ] },
      { "SeedItems": [ "D16-3244" ] },
      { "SeedItems": [ "C9F-00163", "FKF-00689" ] },
      { "SeedItems": [ "F43-01467" ] },
      { "SeedItems": [ "BD5-06013" ] },
      { "SeedItems": [ "P45-00163", "FKF-00689" ] },
      { "SeedItems": [ "C9A-69320" ] }
      ]
    }

Jak widać, plik jest plikiem JSON, gdzie każda wniosków zawiera informacje, które trzeba wysłać żądanie zalecenia. Tworzenie pliku JSON podobne żądań, które należy spełnić, a następnie skopiować go do kontenera, która została właśnie utworzona w magazynie obiektów Blob.

## <a name="kick-start-the-batch-job"></a>Zwiększ efektywność zadaniu

Następnym krokiem jest, aby przesłać nowe zadanie wsadowe. Aby uzyskać więcej informacji zaznacz [odwołanie interfejsu API](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/).

Treść żądania interfejsu API należy zdefiniować lokalizacje miejsce, w którym dane wejściowe, dane wyjściowe i pliki błędów mają być przechowywane. Należy również zdefiniować poświadczeń, które są potrzebne uzyskać dostęp do tych lokalizacji. Ponadto musisz określić niektóre parametry, które dotyczą całej partii (typ zalecenia żądania modelu kompilacji liczba wyników na połączenia, informacje i tak dalej).

Oto przykład co treści żądania powinna wyglądać podobnie do:

    {
      "input": {
        "authenticationType": "PublicOrSas",
        "baseLocation": "https://mystorage1.blob.core.windows.net/",
        "relativeLocation": "container1/batchInput.json",
        "sasBlobToken": "?sv=2015-07_restofToken_…4Z&sp=rw"
      },
      "output": {
        "authenticationType": "PublicOrSas",
        "baseLocation": "https://mystorage1.blob.core.windows.net/",
        "relativeLocation": "container1/batchOutput.json ",
        "sasBlobToken": "?sv=2015-07_restofToken_…4Z&sp=rw"
      },
      "error": {
        "authenticationType": "PublicOrSas",
        "baseLocation": "https://mystorage1.blob.core.windows.net/",
        "relativeLocation": "container1/errors.txt",
        "sasBlobToken": "?sv=2015-07_restofToken_…4Z&sp=rw"
      },
      "job": {
        "apiName": "ItemRecommend",
        "modelId": "9ac12a0a-1add-4bdc-bf42-c6517942b3a6",
        "buildId": 1015703,
        "numberOfResults": 10,
        "includeMetadata": true,
        "minimalScore": 0.0
      }
    }

W tym polu kilka ważnych uwag:

-   Obecnie **authenticationType** zawsze należy skonfigurować do **PublicOrSas**.

-   Musisz uzyskać token podpisu udostępnionych programu Access (SA) umożliwia interfejsu API zalecenia do odczytu i zapisu z Twojego konta magazyn obiektów Blob. Więcej informacji na temat sposobu wygenerować tokeny skojarzeń zabezpieczeń można znaleźć na [stronie interfejsu API zalecenia](../storage/storage-dotnet-shared-access-signature-part-1.md).

-   Tylko **Nazwa_funkcji_api** , która jest obecnie obsługiwane jest **ItemRecommend**, używanej do pozycji do pozycji zalecenia. Tworzeniu partii obecnie nie obsługuje zalecenia element użytkownika.

## <a name="wait-for-the-asynchronous-operation-to-finish"></a>Poczekaj na zakończenie operacji asynchroniczne

Po uruchomieniu operacji partii odpowiedź zwraca nagłówek operacji lokalizacji, który udostępnia informacje, które są niezbędne do śledzenia operacji.
Operacja można śledzić przy użyciu [Pobrać API stan operacji]( https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3da), tak jak do śledzenia operację operacji kompilacji.

## <a name="get-the-results"></a>Uzyskiwanie wyników

Po zakończeniu operacji, przy założeniu, że nie wystąpiły błędy, wyniki można uzyskać z danych wyjściowych Blob miejsca do magazynowania.

W poniższym przykładzie pokazano, wynik może wyglądać następująco. W tym przykładzie zostanie wyświetlona wyników dla partię żądaniami tylko dwa (w celu jego skrócenia).

    {
      "results":
      [   
        {
          "request": { "seedItems": [ "DAF-00500", "P3T-00003" ] },
          "recommendations": [
            {
              "items": [
                {
                  "itemId": "F5U-00011",
                  "name": "L2 Ethernet Adapter-Win8Pro SC EN/XD/XX Hdwr",
                  "metadata": ""
                }
              ],
              "rating": 0.722,
              "reasoning": [ "People who like the selected items also like 'L2 Ethernet Adapter-Win8Pro SC EN/XD/XX Hdwr'" ]
            },
            {
              "items": [
                {
                  "itemId": "G5Y-00001",
                  "name": "Docking Station for Srf Pro/Pro2 SC EN/XD/ES Hdwr",
                  "metadata": ""
                }
              ],
              "rating": 0.718,
              "reasoning": [ "People who like the selected items also like 'Docking Station for Srf Pro/Pro2 SC EN/XD/ES Hdwr'" ]
            }
          ]
        },
        {
          "request": { "seedItems": [ "C9F-00163" ] },
          "recommendations": [
            {
              "items": [
                {
                  "itemId": "C9F-00172",
                  "name": "Nokia 2K Shell for Nokia Lumia 630/635 - Green",
                  "metadata": ""
                }
              ],
              "rating": 0.649,
              "reasoning": [ "People who like 'MOZO Flip Cover for Nokia Lumia 635 - White' also like 'Nokia 2K Shell for Nokia Lumia 630/635 - Green'" ]
            },
            {
              "items": [
                {
                  "itemId": "C9F-00171",
                  "name": "Nokia 2K Shell for Nokia Lumia 630/635 - Orange",
                  "metadata": ""
                }
              ],
              "rating": 0.647,
              "reasoning": [ "People who like 'MOZO Flip Cover for Nokia Lumia 635 - White' also like 'Nokia 2K Shell for Nokia Lumia 630/635 - Orange'" ]
            },
            {
              "items": [
                {
                  "itemId": "C9F-00170",
                  "name": "Nokia 2K Shell for Nokia Lumia 630/635 - Yellow",
                  "metadata": ""
                }
              ],
              "rating": 0.646,
              "reasoning": [ "People who like 'MOZO Flip Cover for Nokia Lumia 635 - White' also like 'Nokia 2K Shell for Nokia Lumia 630/635 - Yellow'" ]
            }       
          ]
        }
    ]}


## <a name="learn-about-the-limitations"></a>Więcej informacji na temat ograniczeń

-   Tylko jednego zadania może być wywołana na subskrypcję naraz.
-   Plik wsadowy wprowadzania zadania nie może być więcej niż 2 MB.
