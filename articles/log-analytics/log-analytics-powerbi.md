<properties
   pageTitle="Eksportowanie danych analizy dziennika do usługi Power BI | Microsoft Azure"
   description="Power BI jest oparte na chmurze usługi analizy biznesowej firmy Microsoft, który zawiera zaawansowanych wizualizacji i raporty dotyczące analizy różnych zestawów danych.  Analizy dziennika może stale eksportować dane z repozytorium usługi OMS usługi Power BI, możesz korzystać z jego wizualizacji i narzędzi do analizy.  Ten artykuł zawiera opis sposobu konfigurowania kwerendy w analizy dziennika, który automatycznie eksportowanie do usługi Power BI w regularnych odstępach."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="bwren" />

# <a name="export-log-analytics-data-to-power-bi"></a>Eksportowanie danych analizy dziennika do usługi Power BI

[Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/) jest oparte na chmurze usługi analizy biznesowej firmy Microsoft, który zawiera zaawansowanych wizualizacji i raporty dotyczące analizy różnych zestawów danych.  Analizy dziennika można automatycznie eksportować dane z repozytorium usługi OMS usługi Power BI, można wykorzystać jej wizualizacji i narzędzi do analizy.

Po skonfigurowaniu usługi Power BI z dziennika analizy, możesz utworzyć kwerendy dziennika, które Eksportuj wyniki do odpowiednich zestawów danych w usłudze Power BI.  Kwerendy i Eksportuj w dalszym ciągu uruchamiane automatycznie zgodnie z harmonogramem definiowane w celu aktualność zestawu danych przy użyciu najnowszych danych zebranych przez analizy dziennika.

![Analizy dziennika do usługi Power BI](media/log-analytics-powerbi/overview.png)

## <a name="power-bi-schedules"></a>Power BI harmonogramów

*Power BI harmonogramu* zawiera wyszukiwania dziennika, który eksportuje zestaw danych z repozytorium usługi OMS do odpowiedniego zestawu danych w usłudze Power BI i harmonogram, która określa, jak często jest wykonywane to wyszukiwanie aby zapewnić aktualność zestawu danych.

Pola w zestawie danych będą zgodne właściwości rekordy zwrócone przez przeszukiwanie dziennika.  Jeśli wyszukiwanie zwraca rekordy różnych typów zestawu danych będzie zawierać wszystkie właściwości z każdego typu rekordu uwzględniane.  

> [AZURE.NOTE] Jest najlepszym rozwiązaniem jest użycie dziennika kwerendę wyszukiwania, która zwraca dane w przeciwieństwie do wykonywania konsolidacji za pomocą poleceń, takich jak [miary](log-analytics-search-reference.md#measure).  Od nieprzetworzonych danych, można wykonać dowolną agregacji i obliczeń w usłudze Power BI.

## <a name="connecting-oms-workspace-to-power-bi"></a>Łączenie usługi OMS obszaru roboczego do usługi Power BI

Przed analizy dziennika można eksportować do usługi Power BI, należy połączyć usługi OMS obszaru roboczego do swojego konta usługi Power BI, wykonując poniższą procedurę.  

1. W konsoli usługi OMS kliknij kafelka **Ustawienia** .
2. Wybierz pozycję **konta**.
3. W sekcji **Informacje o obszarze roboczym** kliknij pozycję **Połącz z programem Power BI konta**.
4. Wprowadź poświadczenia konta usługi Power BI.

## <a name="create-a-power-bi-schedule"></a>Utworzenie harmonogramu Power BI

Utworzenie harmonogramu Power BI dla każdego zestawu danych, wykonując poniższą procedurę.

1. W konsoli usługi OMS kliknij **Przeszukiwanie dziennika** .
2. W nowej kwerendy wpisz lub wybierz zapisanego wyszukiwania, która zwraca dane, które chcesz wyeksportować do **Usługi Power BI**.  
3. Kliknij przycisk **Power BI** w górnej części strony, aby otworzyć okno dialogowe **Usługi Power BI** .
4. Wprowadź informacje w poniższej tabeli, a następnie kliknij przycisk **Zapisz**.

| Właściwość | Opis |
|:--|:--|
| Nazwa | Nazwę identyfikującą harmonogramu podczas wyświetlania listy harmonogramów Power BI. |
| Zapisane wyszukiwanie | Wyszukaj dziennika, aby uruchomić.  Możesz bieżącej kwerendy wybierającej, lub wybierz istniejące zapisane wyszukiwanie w polu listy rozwijanej. |
| Harmonogram | Jak często uruchamianie zapisanego wyszukiwania i eksportowanie do zestawu danych usługi Power BI.  Wartość musi należeć do przedziału 15 minut do 24 godzin. |
| Nazwa zestawu danych | Nazwa zestawu danych w usłudze Power BI.  Zostanie ono utworzony, jeśli nie istnieje i zaktualizowane, jeśli istnieje. |

## <a name="viewing-and-removing-power-bi-schedules"></a>Wyświetlanie i usuwanie Power BI harmonogramów

Wyświetlanie listy istniejących Power BI harmonogramów, wykonując poniższą procedurę.

1. W konsoli usługi OMS kliknij kafelka **Ustawienia** .
2. Wybierz pozycję **Power BI**.

Oprócz szczegółów harmonogramu liczba uruchamiane w ostatnim tygodniu harmonogram i stanu ostatniej synchronizacji są wyświetlane.  Synchronizacja napotkał błędy, po kliknięciu łącza do uruchomienia dziennika wyszukiwanie rekordów z szczegóły błędu.

Harmonogram można usunąć, klikając przycisk **X** w **Usuń kolumnę**.  Możesz wyłączyć harmonogram, wybierając pozycję **wyłączone**.  Aby zmodyfikować harmonogram, należy ją usunąć i utwórz go ponownie z nowymi ustawieniami.

![Power BI harmonogramów](media/log-analytics-powerbi/schedules.png)

## <a name="sample-walkthrough"></a>Przykładowe Instruktaż
Poniższej sekcji opisano przykład tworzenia harmonogramu Power BI i tworzenie prostego raportu za pomocą swojego zestawu danych.  W tym przykładzie wszystkie dane dotyczące wydajności zestawu komputerów jest eksportowany do usługi Power BI, a następnie wykres liniowy służy do wyświetlania wykorzystanie procesora.

### <a name="create-log-search"></a>Tworzenie dziennika wyszukiwania
Firma Microsoft rozpoczyna się od utworzenia dziennika możesz wyszukać dane, które chcemy, aby wysłać do zestawu danych.  W tym przykładzie użyjemy kwerendę, która zwraca wszystkie dane dotyczące wydajności dla komputerów o nazwie zaczynającej się *srv*.  

![Power BI harmonogramów](media/log-analytics-powerbi/walkthrough-query.png)

### <a name="create-power-bi-search"></a>Tworzenie Power BI wyszukiwania
Firma Microsoft kliknij przycisk **Power BI** , aby otworzyć okno dialogowe usługi Power BI i wprowadź wymagane informacje.  Potrzebne jest to wyszukiwanie, aby uruchomić raz na godzinę i Utwórz zestaw danych o nazwie *Contoso wydajności*.  Ponieważ już mamy wyszukiwania Otwórz tworzonych dane potrzebna, możemy zachować domyślny *Użyj bieżącej kwerendy wyszukiwania* **Zapisane**wyszukiwania.

![Power BI wyszukiwania](media/log-analytics-powerbi/walkthrough-schedule.png)

### <a name="verify-power-bi-search"></a>Sprawdź Power BI wyszukiwania
Aby sprawdzić, czy harmonogram możemy utworzony prawidłowo, możemy wyświetlić listę Power BI wyszukiwania w obszarze kafelka **Ustawienia** na pulpicie nawigacyjnym usługi OMS.  Firma Microsoft Poczekaj kilka minut i Odśwież ten widok, dopóki raporty, czy synchronizacja zostało uruchomione.

![Power BI wyszukiwania](media/log-analytics-powerbi/walkthrough-schedules.png)

### <a name="verify-the-dataset-in-power-bi"></a>Weryfikowanie zestawu danych w usłudze Power BI
Firma Microsoft Zaloguj się do naszego konta u [powerbi.microsoft.com](http://powerbi.microsoft.com/) i przewiń do **zestawów danych** u dołu lewego okienka.  Widać, że zestawu *Contoso wydajności* danych jest wyświetlana wskazującą, czy naszych eksportu zostało uruchomione pomyślnie.

![Power BI w zestawie danych](media/log-analytics-powerbi/walkthrough-datasets.png)

### <a name="create-report-based-on-dataset"></a>Tworzenie raportu według zestawu danych
Firma Microsoft wybierz zestaw danych **Wydajności firmy Contoso** , a następnie polecenie **wyników** w okienku **pola** po prawej stronie, aby wyświetlić pola, które są częścią tego zestawu danych.  Aby utworzyć wykres liniowy z wykorzystanie procesora na każdym komputerze, możemy wykonać następujące czynności.

1. Zaznacz wizualizację linii wykresu.
2. Przeciągnij **nazwa_obiektu** do **poziomu filtru raportu** , a następnie zaznacz **Procesor**.
3. Przeciągnij **CounterName** do **poziomu filtru raportu** , a następnie zaznacz **% czasu procesora**.
4. Przeciągnij **równowartości** **wartości**.
5. Przeciągnij **komputera** **legendy**.
6. Przeciągnij **TimeGenerated** **osi**.

Firma Microsoft może wyświetlać wyświetlania wyniku wykres liniowy z danymi z naszych zestawu danych.

![Wykres liniowy Power BI](media/log-analytics-powerbi/walkthrough-linegraph.png)

### <a name="save-the-report"></a>Zapisywanie raportu
Pracujemy zapisać raport, klikając przycisk Zapisz w górnej części ekranu i sprawdź, czy jest teraz wymienione w sekcji raporty w okienku po lewej stronie.

![Raportów programu Power BI](media/log-analytics-powerbi/walkthrough-report.png)

## <a name="next-steps"></a>Następne kroki

- Informacje na temat [wyszukiwania dziennika](log-analytics-log-searches.md) do tworzenia kwerend, które można eksportować do usługi Power BI.
- Dowiedz się więcej na temat [Usługi Power BI](http://powerbi.microsoft.com) tworzyć wizualizacje według Eksport analizy dziennika.
