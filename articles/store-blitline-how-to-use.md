<properties 
    pageTitle="Jak używać Blitline obrazu przetwarzanie — Azure funkcji przewodnika" 
    description="Dowiedz się, jak za pomocą usługi Blitline proces obrazów w aplikacji Azure." 
    services="" 
    documentationCenter=".net" 
    authors="blitline-dev" 
    manager="jason@blitline.com" 
    editor="jason@blitline.com"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/09/2014" 
    ms.author="support@blitline.com"/>
# <a name="how-to-use-blitline-with-azure-and-azure-storage"></a>Jak używać Blitline z Azure i Azure miejsca do magazynowania

Ten przewodnik będzie wyjaśniono, jak uzyskać dostęp do usług Blitline i sposób przesyłania zadań Blitline.

## <a name="what-is-blitline"></a>Co to jest Blitline?

Blitline jest oparte na chmurze obrazu Usługa, która zapewnia przetwarzania obrazów poziomu przedsiębiorstwa na ułamek cena, która będzie kosztować utworzyć samodzielnie.

Fakt, jest to, że przetwarzanie obrazu zostało wykonane wielokrotnie, zazwyczaj odbudowany od podstaw dla każdej witryny sieci Web. Firma Microsoft to pamiętać, ponieważ firma Microsoft po skonstruowaniu ich milionów razy za. Jeden dzień, który firma Microsoft zdecydowała, że być może nadszedł czas firma Microsoft po prostu to zrobić dla wszystkich osób. Firma Microsoft dowiedzieć się, jak to zrobić to szybkie i efektywne i zapisywanie wszystkich pracy w międzyczasie zrobić.

Aby uzyskać więcej informacji zobacz [http://www.blitline.com](http://www.blitline.com).

## <a name="what-blitline-is-not"></a>Co Blitline nie jest...

Wyjaśnienie, co jest przydatne w przypadku Blitline, jest zazwyczaj łatwiejsze do identyfikowania co Blitline nie przed przejściem do przodu.

- Blitline nie ma widżety HTML, aby przekazać obrazy. Musi być obrazów dostępnych publicznie lub z ograniczonymi uprawnieniami dostępne dla Blitline się skontaktować.

- Nie działa Blitline live przetwarzania obrazu, takie jak Aviary.com

- Blitline nie zaakceptuje przekazywania obrazów, nie można przekazać obrazy do Blitline bezpośrednio. Należy przekazać je do przechowywania Azure lub innych miejscach obsługuje Blitline, a następnie określić Blitline, gdzie można znaleźć je uzyskać.

- Blitline jest znacznym stopniu równoległa i wykonaj wszelkie Przetwarzanie synchroniczne. Co oznacza należy przekazać nam postback_url i firma Microsoft umożliwiają poznanie po możemy zakończeniu przetwarzania.

## <a name="create-a-blitline-account"></a>Tworzenie konta Blitline

[AZURE.INCLUDE [blitline-signup](../includes/blitline-signup.md)]

## <a name="how-to-create-a-blitline-job"></a>Jak utworzyć zadanie Blitline

Blitline używa JSON, aby zdefiniować akcje, które chcesz wykonać na obrazie. Ten JSON składa się z kilku prostych pól.

Najprostszy przykład jest następująca:

        json : '{
       "application_id": "MY_APP_ID",
       "src" : "http://cdn.blitline.com/filters/boys.jpeg",
       "functions" : [ {
           "name": "resize_to_fit",
           "params" : { "width": 240, "height": 140 },
           "save" : { "image_identifier" : "external_sample_1" }
       } ]
    }'

W tym miejscu mamy JSON, która ma być obraz "src" "... boys.jpeg", a następnie zmień rozmiar obrazu do 240 x 140.

Identyfikator aplikacji jest coś, co można znaleźć w swojej karcie **Informacji o połączeniu** lub **Zarządzanie** Azure. Jest identyfikatora tajne, która umożliwia wykonywanie zadań na Blitline.

Parametr "Zapisz" służy do identyfikowania informacji o miejsce, w którym chcesz umieścić obraz, gdy mamy zostały przetworzone go. W tym przypadku uproszczony możemy jeszcze zdefiniowana. Jeśli lokalizacja nie jest definiowana Blitline będą przechowywane go lokalnie (i tymczasowo) w lokalizacji w chmurze unikatowe. Można pobrać tej lokalizacji z JSON zwracane przez Blitline po wprowadzeniu Blitline. Identyfikator "obraz" jest wymagane i są zwracane po do identyfikowania tych danych zapisany obraz.

Można znaleźć więcej informacji na temat *funkcji* obsługujemy tutaj: <http://www.blitline.com/docs/functions>

Możesz również znaleźć dokumentację dotyczącą Opcje zadania tutaj: <http://www.blitline.com/docs/api>

Po umieszczeniu usługi JSON zrobić wystarczy **WPIS** go do`http://api.blitline.com/job`

Zostanie wyświetlony ponownie JSON, który wygląda mniej więcej tak:

    {
     "results":
         {"images":
            [{
              "image_identifier":"external_sample_1",
              "s3_url":"https://s3.amazonaws.com/dev.blitline/2011110722/YOUR_APP_ID/CK3f0xBF_2bV6wf7gEZE8w.jpg"
            }],
          "job_id":"4eb8c9f72a50ee2a9900002f"
         }
    }


To informuje, że Blitline otrzymaniu żądania, ma umieszczanie go w kolejce przetwarzania i po zakończeniu obraz będzie dostępna w: **https://s3.amazonaws.com/dev.blitline/2011110722/YOUR\_aplikacji\_ID/CK3f0xBF_2bV6wf7gEZE8w.jpg**

## <a name="how-to-save-an-image-to-your-azure-storage-account"></a>Jak zapisać obraz do swojego konta magazynu platformy Azure

Jeśli masz konto Azure miejsca do magazynowania, mogą być Blitline push obraz usługi Azure kontener. Dodając "azure_destination" do definiowania lokalizację i uprawnień dla Blitline do.

Oto przykład:

    job : '{
      "application_id": "YOUR_APP_ID",
      "src" : "http://www.google.com/logos/2011/houdini11-hp.jpg",
         "functions" : [{
         "name": "blur",
         "save" : {
             "image_identifier" : "YOUR_IMAGE_IDENTIFIER",
             "azure_destination" : {
                 "account_name" : "YOUR_AZURE_CONTAINER_NAME",
                 "shared_access_signature" : "SAS_THAT_GIVES_BLITLINE_PERMISSION_TO_WRITE_THIS_OBJECT_TO_CONTAINER",
               }
           }
         }]
       }'


Wypełniając wartości CAPITALIZED na własny, można przesłać ten JSON do http://api.blitline.com/job i obraz "src" będą przetwarzane przy użyciu filtru Rozmycie i następnie przypisany do Azure docelowego.

###<a name="please-note"></a>Uwaga:

Skojarzenia zabezpieczeń musi zawierać pełnego adresu url skojarzenia zabezpieczeń, łącznie z nazwą pliku w pliku docelowym.

Przykład:

    http://blitline.blob.core.windows.net/sample/image.jpg?sr=b&sv=2012-02-12&st=2013-04-12T03%3A18%3A30Z&se=2013-04-12T04%3A18%3A30Z&sp=w&sig=Bte2hkkbwTT2sqlkkKLop2asByrE0sIfeesOwj7jNA5o%3D


Można również przeczytać najnowszym dokumentów magazyn Azure Blitline w [tym miejscu](http://www.blitline.com/docs/azure_storage).


## <a name="next-steps"></a>Następne kroki

Odwiedź stronę blitline.com, aby uzyskać informacje o wszystkich naszych innych funkcji:

* Punkt końcowy interfejsu API Blitline dokumenty <http://www.blitline.com/docs/api>
* Funkcje Blitline interfejsu API <http://www.blitline.com/docs/functions>
* Przykłady Blitline interfejsu API <http://www.blitline.com/docs/examples>
* Trzecia część biblioteki Nuget <http://nuget.org/packages/Blitline.Net>
