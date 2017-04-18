<properties
    pageTitle="Wynik magazynu Lake danych analizy strumieniu | Microsoft Azure"
    description="Konfigurowanie uwierzytelniania i autoryzacji magazynu Azure danych Lake w zadaniu analizy strumieniu"
    keywords=""
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/26/2016"
    ms.author="jeffstok"
/>

# <a name="stream-analytics-data-lake-store-output"></a>Wynik magazynu Lake strumienia analizy danych

Zadania analizy strumieniu obsługuje kilka metod dane wyjściowe, z których jedna jest [Azure magazynu Lake danych](https://azure.microsoft.com/services/data-lake-store/). Magazyn Lake danych Azure to repozytorium wyraźny obejmujących obciążenia analityczny duży danych. Magazynu Lake danych pozwala na przechowywanie danych dowolny rozmiar, typ i spożyciu szybkość działania i badawczych analizy.

## <a name="authorize-a-data-lake-store-account"></a>Autoryzuj konto magazynu Lake danych

1.  Po zaznaczeniu magazynu Lake danych jako wynik w portalu zarządzania Azure pojawi się monit Aby autoryzować zastosowania sklepu Lake istniejących danych lub, aby zażądać dostępu do podglądu sklepu Lake danych za pośrednictwem portalu klasyczny Azure.

    ![](media/stream-analytics-data-lake-output/stream-analytics-data-lake-output-authorization.png)  

2.  Jeśli już masz dostęp do magazynu Lake danych, kliknij przycisk "Autoryzuj" i krótki czas strony będzie wyświetlana wskazująca "Przekierowanie do autoryzacji...". Strona zostanie zamknięta automatycznie i zostanie wyświetlona na stronie, która pozwala na konfigurowanie wynik magazynu Lake danych.

Jeśli użytkownik nie jest zarejestrowany dla Podgląd Sklepu Lake danych, możesz łącza "Utwórz konto teraz", aby zainicjować żądanie, lub wykonaj [instrukcje wprowadzenie](../data-lake-store/data-lake-store-get-started-portal.md).

## <a name="configure-the-data-lake-store-output-properties"></a>Konfigurowanie właściwości dane wyjściowe magazynu Lake danych

Po utworzeniu konta magazynu Lake danych uwierzytelniony można konfigurować właściwości do wyprowadzania danych Lake magazynu. W poniższej tabeli przedstawiono listę nazw właściwości i ich opisy, aby skonfigurować wyprowadzania magazynu Lake danych.

<table>
<tbody>
<tr>
<td><B>NAZWA WŁAŚCIWOŚCI</B></td>
<td><B>OPIS</B></td>
</tr>
<tr>
<td>Wyjściowy Alias</td>
<td>To jest używane w kwerendach w celu skierowania wyników kwerendy do tego magazynu Lake danych przyjazną nazwę.</td>
</tr>
<tr>
<td>Konto magazynu Lake danych</td>
<td>Nazwa konta miejsca do magazynowania, w której wysyłasz danych wyjściowych. Zostanie wyświetlona z listy rozwijanej listy kont magazynu Lake danych, do których użytkownik zalogowany do portalu ma dostęp do.</td>
</tr>
<tr>
<td>Ścieżka prefiks wzorzec [<I>opcjonalne</I>]</td>
<td>Ścieżka pliku do zapisywania plików w ramach określonego konta magazynu Lake dane używane. <BR>{date} {time}<BR>Przykład 1: folder1/dzienniki / {dat} / {time}<BR>Przykład 2: folder1/dzienniki / {dat}</td>
</tr>
<tr>
<td>Format daty [<I>opcjonalne</I>]</td>
<td>Jeśli token Data jest używana w ścieżce prefiks, można wybrać format daty, w której są zorganizowane plików. Przykład: RRRR MM-DD</td>
</tr>
<tr>
<td>Format godziny [<I>opcjonalne</I>]</td>
<td>Jeśli token czasu jest używany w ścieżce prefiks, określ format czasu, w którym są zorganizowane plików. Jedyna obsługiwana wartość jest obecnie HH.</td>
</tr>
<tr>
<td>Format szeregowania zdarzenia</td>
<td>Format szeregowania danych wyjściowych. JSON, CSV i Avro są obsługiwane.</td>
</tr>
<tr>
<td>Kodowanie</td>
<td>Jeśli format CSV lub JSON, musi być określony kodowania. UTF-8 jest obsługiwany tylko format kodowania w tej chwili.</td>
</tr>
<tr>
<td>Ogranicznika</td>
<td>Stosuje się tylko do szeregowania CSV. Analizy strumieniu obsługuje wiele typowych ograniczniki szeregowania dane w formacie CSV. Obsługiwane są następujące wartości przecinek, średnik, odstęp, karta i pionowy pasek.</td>
</tr>
<tr>
<td>Formatowanie</td>
<td>Stosuje się tylko do szeregowania JSON. Linia ciągła rozdzielając określa formatowania wynik przez każdy obiekt JSON rozdzielając nowy wiersz. Tablica Określa, że dane wyjściowe mają zostać sformatowane jako tablicę obiektów JSON.</td>
</tr>
</tbody>
</table>

## <a name="renew-data-lake-store-authorization"></a>Odnawianie autoryzacji magazynu Lake danych

Obecnie jest to ograniczenie miejsce, w którym token uwierzytelniania konieczne jest ręczne odświeżenie co 90 dni dla wszystkich zadań z magazynu Lake dane wyjściowe. Konieczne będzie również uwierzytelnienie konta magazynu Lake danych, jeśli hasło zostało zmienione, ponieważ utworzonej lub ostatnia uwierzytelniony zadania. Objawem tego problemu jest żadne dane wyjściowe zadania i komunikat o błędzie w dzienniku operacji wskazująca konieczność ponownego autoryzacji.

Aby rozwiązać ten problem, zatrzymanie uruchomionego zadania i przejdź do wydruku magazynu Lake danych. Kliknij łącze "Odnawianie autoryzacji" i krótki czas strony będzie wyświetlana wskazująca "Przekierowanie do autoryzacji...". Strona zostanie zamknięte automatycznie i w przypadku powodzenia wskaże "Autoryzacji pomyślnie odnowienia". Możesz następnie potrzebę u dołu strony, kliknij przycisk "Zapisz" i można kontynuować uruchamiając zadania z zatrzymana podczas ostatniego Aby uniknąć utraty danych.

![](media/stream-analytics-data-lake-output/stream-analytics-data-lake-output-renew-authorization.png)
