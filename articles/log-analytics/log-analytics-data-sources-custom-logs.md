<properties 
   pageTitle="Niestandardowe dzienniki w dzienniku analizy | Microsoft Azure"
   description="Analizy dziennika może zbierać zdarzenia z plików tekstowych na komputerach w systemach Windows i Linux.  Ten artykuł zawiera opis sposobu definiowania nowego dziennika niestandardowych i szczegóły rekordów, które tworzą w repozytorium usługi OMS."
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

# <a name="custom-logs-in-log-analytics"></a>Niestandardowe dzienników w analizy dziennika

Źródło danych dzienniki niestandardowe w analizy dziennika umożliwia zbieranie zdarzeń z plików tekstowych na komputerach w systemach Windows i Linux. Wiele aplikacji rejestrowanie informacji do plików tekstowych zamiast standardowej usług rejestrowania, takich jak dziennika zdarzeń systemu Windows lub Syslog.  Po zebrane, można analizować każdego rekordu w dzienniku do poszczególnych pól za pomocą funkcji [Pola niestandardowe](log-analytics-custom-fields.md) analizy dziennika.

![Zbieranie dzienników niestandardowe](media/log-analytics-data-sources-custom-logs/overview.png)

Pliki dziennika do pobrania musi odpowiadać następujące kryteria.

- Dziennik musisz mieć pojedynczy wpis w wierszu lub za pomocą sygnatura czasowa pasujących jedną z następujących formatów na początku każdego wpisu.

    RRRR MM-DD HH: MM: <br>
  -MM-DD YYYY HH: MM: SS AM/PM <br>
  Pn DD, YYYY HH: mm:
    
- Plik dziennika nie może zezwalać cykliczne aktualizacje miejsce, w którym plik jest zastępowany nowych wpisów. 

## <a name="defining-a-custom-log"></a>Definiowanie niestandardowego dziennika

Poniższa procedura umożliwia określenie niestandardowej pliku dziennika.  Przewiń na końcu tego artykułu Instruktaż próbki: Dodawanie niestandardowych dziennika.

### <a name="step-1-open-the-custom-log-wizard"></a>Krok 1. Otwieranie kreatora dziennika niestandardowego

Niestandardowe Kreator dziennika działa w portalu usługi OMS i umożliwia definiowanie nowego dziennika niestandardowego do zbierania.

1.  W portalu usługi OMS przejdź do pozycji **Ustawienia**.
2.  Kliknij na **danych** , a następnie **Dzienniki niestandardowe**.
3.  Domyślnie wszystkie zmiany konfiguracji są automatycznie przenoszone do wszystkich agentów.  Dla czynników Linux pliku konfiguracji są wysyłane do modułów zbierających dane Fluentd.  Jeśli chcesz zmodyfikować ten plik ręcznie w każdej agenta Linux, a następnie usuń zaznaczenie pola *Zastosuj poniżej konfiguracji na komputerach moje Linux*.
4.  Kliknij przycisk **Dodaj +** aby otworzyć Kreatora dziennika niestandardowe.

### <a name="step-2-upload-and-parse-a-sample-log"></a>Krok 2. Przekazywanie i przeanalizować przykładowego dziennika

Możesz uruchomić, przekazując próbki niestandardowe dziennika.  Kreator analizy i wyświetlanie wpisów w tym pliku umożliwiają sprawdzanie poprawności.  Dziennik analizy użyje ogranicznik, który można określić do identyfikowania każdego rekordu.

**Nowy wiersz** jest domyślnym ogranicznikiem i służy do plików dziennika, zawierających pojedyncze wpisy w poszczególnych wierszach.  Jeśli wiersz zaczyna się od daty i godziny w jeden z dostępnych formatów, można określić ogranicznika **sygnatury czasowej** , który obsługuje wpisów, które obejmują więcej niż jeden wiersz. 

Jeśli ogranicznika sygnatura czasowa jest używana, następnie właściwość TimeGenerated każdy rekord przechowywany w usługi OMS zostanie wypełniona data/godzina, określone dla tego wpisu w pliku dziennika.  Użycie nowego ogranicznika linii TimeGenerated jest wypełniane z datą i godziną, aby analizy dziennika pobrane wpisu. 

>[AZURE.NOTE]Dziennik analizy traktuje obecnie zebrane z dziennika za pomocą ogranicznika sygnatura czasowa jako UTC daty/godziny.  To szybko zmieni się używać strefę czasową na agenta. 
 
1.  Kliknij przycisk **Przeglądaj** i przejdź do przykładowy plik.  Należy zauważyć, że może to przycisk może być oznaczony **Wybierz plik** w niektórych przeglądarkach.
2.  Kliknij przycisk **Dalej**. 
3.  Kreator dziennika niestandardowe przekazać plik i listy rekordy, które identyfikuje.
4.  Zmienianie ogranicznik, który służy do identyfikowania nowego rekordu, a następnie wybierz pozycję ogranicznik, który najlepiej określającego rekordy w pliku dziennika.
5.  Kliknij przycisk **Dalej**.

### <a name="step-3-add-log-collection-paths"></a>Krok 3. Dodawanie ścieżki zbioru dziennika

Należy zdefiniować jedną lub więcej ścieżek agenta, gdzie można znaleźć niestandardowe dziennika.  Albo można udostępnić określonych ścieżkę i nazwę pliku dziennika, albo można określić ścieżkę przy użyciu symboli wieloznacznych dla nazwy.  Obsługuje aplikacje, które utworzenie nowego pliku każdego dnia lub gdy jeden plik osiągnie określony rozmiar.  Możesz także podać wielu ścieżek dla jednego pliku dziennika.

Na przykład aplikacja może utworzyć plik dziennika każdego dnia z datą uwzględniony w nazwie jak log20100316.txt. Wzór wypełnienia dla tych dziennika może być *dziennika\*txt* którego chcesz zastosować do dowolny plik dziennika, po zastosowaniu jest nazewnictwa schematu.

Poniższa tabela zawiera przykłady prawidłowych Aby określić inny dzien. 

| Opis | Ścieżka |
|:--|:--|
| Wszystkie pliki *C:\Logs* z rozszerzeniem txt agent systemu Windows | C:\Logs\\\*txt |
| Wszystkie pliki w *C:\Logs* pod nazwą, począwszy od dziennika i rozszerzeniem txt agenta systemu Windows | C:\Logs\log\*txt |
| Wszystkie pliki */var/log/audit* z rozszerzeniem txt Linux agenta | /var/log/audit/*.txt |
| Wszystkie pliki w */var/log/audit* pod nazwą, począwszy od dziennika i rozszerzeniem txt Linux agenta | /var/log/audit/log\*txt |
  

1.  Wybierz pozycję Windows i Linux oraz określ format ścieżki dodajesz.
2.  Wpisz ścieżkę i kliknij przycisk **+** przycisk.
3.  Powtórz ten proces dla wszelkie dodatkowe ścieżki.

### <a name="step-4-provide-a-name-and-description-for-the-log"></a>Krok 4. Podaj nazwę i opis w Dzienniku

Nazwa użytkownika będzie używana dla danego typu dziennika, zgodnie z powyższym opisem.  Zawsze zakończy się z _CL, aby odróżnić go jako niestandardowy dziennika.

1.  Wpisz nazwę dla dziennika.  ** \_CL** sufiks jest dostarczany automatycznie.
2.  Dodaj opcjonalny **Opis**.
3.  Kliknij przycisk **Dalej** do zapisania definicji dziennika niestandardowego.

### <a name="step-5-validate-that-the-custom-logs-are-being-collected"></a>Krok 5. Sprawdź poprawność pobierane niestandardowych dzienników
Może potrwać do godziny początkowej danych z nowego dziennika niestandardowego pojawiały się w dzienniku analizy.  Rozpocznie się zbierania wpisy z dzienników znaleziony w ścieżce określonej od punktu zdefiniowane przez dziennik niestandardowe.  Nie zachowa wpisów, które zostały przekazane podczas tworzenia niestandardowych dziennika, ale będzie zbierać już istniejące wpisy w plikach dziennika, które zlokalizują.

Po analizy dziennika uruchamiania zbierania w dzienniku niestandardowe swoje rekordy będą dostępne w przypadku wyszukiwania dziennika.  Za pomocą nazwa nadana dziennika niestandardowego jako **Typ** w kwerendzie.

>[AZURE.NOTE] Jeśli właściwość RawData brakuje wyszukiwania, może być konieczne zamknięcie i ponowne otwarcie przeglądarki.

### <a name="step-6-parse-the-custom-log-entries"></a>Krok 6. Analizowanie wpisy dziennika niestandardowego

Wpis dziennika całego będą przechowywane w jednej właściwości o nazwie **RawData**.  Najprawdopodobniej będzie oddzielanie różnych rodzajów informacji w każdej pozycji do poszczególnych właściwości przechowywanych w rekordzie.  Możesz to zrobić to korzystanie z funkcji [Pola niestandardowe](log-analytics-custom-fields.md) z analizy dziennika.

Szczegółowe informacje na temat analizowania wpis dziennika niestandardowe nie są dostarczane w tym miejscu.  Zapoznaj się z dokumentacją [Pola niestandardowe](log-analytics-custom-fields.md) informacje.

## <a name="disabling-a-custom-log"></a>Wyłączanie dziennika niestandardowego

Nie można usunąć definicji dziennika niestandardowego, gdy jest on utworzony, ale można ją wyłączyć, usuwając wszystkie jego ścieżki zbioru.

1.  W portalu usługi OMS przejdź do pozycji **Ustawienia**.
2.  Kliknij na **danych** , a następnie **Dzienniki niestandardowe**.
3.  Kliknij pozycję **Szczegóły** obok definicji dziennika niestandardowego, aby wyłączyć.
4.  Usuwanie wszystkich ścieżek zbioru definicja dziennika niestandardowego.


## <a name="data-collection"></a>Zbieranie danych

Dziennik analizy zapisze nowe wpisy z każdego niestandardowego dziennika co 5 minut.  Agentem rejestrowany jego położenie w każdy plik dziennika, zbierane z.  Jeśli agent przechodzi do trybu offline w okresie czasu, następnie analizy dziennika zapisze wpisy w miejsce, w którym go ostatnio przerwana, nawet jeśli te pozycje zostały utworzone podczas agent był w trybie offline.

Cała zawartość wpis dziennika są zapisywane do pojedynczej właściwości o nazwie **RawData**.  Można to analizować do wielu właściwości, które mogą być analizowane i przeszukiwać osobno, definiując [Pola niestandardowe](log-analytics-custom-fields.md) po utworzeniu niestandardowy dziennika.


## <a name="custom-log-record-properties"></a>Właściwości niestandardowe dziennika rekordu

Rekordy dziennika niestandardowego mają typ o nazwie dziennika, dostarczone i właściwości w poniższej tabeli.

| Właściwość | Opis |
|:--|:--|
| TimeGenerated | Data i godzina, rekord zebrane przy analizy dziennika.  Jeśli dziennik używany ogranicznika godzina podstawie to czas zbierane od wpisu. |
| SourceSystem  | Typ agenta zgromadzone rekord z. <br> Łączenie OpsManager — agent systemu Windows, bądź bezpośrednio lub SCOM <br> Linux — wszystkich agentów Linux  |
| RawData             | Pełny tekst zebrane wpisu. |
| ManagementGroupName | Nazwa grupy zarządzania SCOM agentów.  W przypadku innych czynników jest AOI -\<identyfikator obszaru roboczego\> |


## <a name="log-searches-with-custom-log-records"></a>Wyszukiwanie dziennika z rekordami dziennika niestandardowego

Rekordy z dzienników niestandardowych są przechowywane w repozytorium usługi OMS, tak jak rekordy z innego źródła danych.  Mają typu zgodnych z nazwą podawane podczas definiowania dziennika, aby móc używać właściwości Typ w wyszukiwaniu w celu pobrania rekordów zebrane z określonego dziennika.

Poniższa tabela zawiera różne przykłady wyszukiwania dziennika, które pobieranie rekordów z dzienników niestandardowe.

| Kwerendy | Opis |
|:--|:--|
| Typ = MyApp_CL | Wszystkie zdarzenia w niestandardowy Zaloguj nazwany MyApp_CL. |
| Typ = MyApp_CL Severity_CF = błąd | Wszystkie zdarzenia z dziennika niestandardowego o nazwie MyApp_CL o wartości *błędu* w pola niestandardowego o nazwie *Severity_CF*. |




## <a name="sample-walkthrough-of-adding-a-custom-log"></a>Przykładowe Instruktaż: Dodawanie niestandardowych dziennika

Poniższej sekcji opisano przykładem utworzenia dziennika niestandardowego.  Przykładowego dziennika są zbierane jest pojedynczy wpis w każdym wierszu, począwszy od daty i godziny oraz następnie przecinek rozdzielanym kodu, stanu i wiadomości.  Poniżej przedstawiono kilka przykładowych wpisów.

    2016-03-10 01:34:36 207,Success,Client 05a26a97-272a-4bc9-8f64-269d154b0e39 connected
    2016-03-10 01:33:33 208,Warning,Client ec53d95c-1c88-41ae-8174-92104212de5d disconnected
    2016-03-10 01:35:44 209,Success,Transaction 10d65890-b003-48f8-9cfc-9c74b51189c8 succeeded
    2016-03-10 01:38:22 302,Error,Application could not connect to database
    2016-03-10 01:31:34 303,Error,Application lost connection to database

### <a name="upload-and-parse-a-sample-log"></a>Przekazywanie i przeanalizować przykładowego dziennika

Firma Microsoft udostępnia jednego z plików dziennika i może wyświetlać zdarzenia, które będzie zbierać.  W tym przypadku nowy wiersz jest wystarczające ogranicznika.  Jeśli pojedynczy wpis w dzienniku może obejmować wiele wierszy, a następnie ogranicznika sygnatura czasowa musi być używany.

![Przekazywanie i przeanalizować przykładowego dziennika](media/log-analytics-data-sources-custom-logs/delimiter.png)

### <a name="add-log-collection-paths"></a>Dodawanie ścieżki zbioru dziennika

Pliki dziennika zostanie umieszczony w *C:\MyApp\Logs*.  Każdego dnia o nazwie, który uwzględnia datę w deseniu *appYYYYMMDD.log*zostanie utworzony nowy plik.  Wzorzec wystarczające dla tego dziennika będzie *C:\MyApp\Logs\\\*.log*.

![Ścieżka kolekcji dziennika](media/log-analytics-data-sources-custom-logs/collection-path.png)

### <a name="provide-a-name-and-description-for-the-log"></a>Podaj nazwę i opis w Dzienniku

Firma Microsoft korzysta z *MyApp_CL* i wpisz nazwę w polu **Opis**.

![Nazwa dziennika](media/log-analytics-data-sources-custom-logs/log-name.png)


### <a name="validate-that-the-custom-logs-are-being-collected"></a>Sprawdź poprawność pobierane niestandardowych dzienników

Firma Microsoft korzysta z kwerendy z *Typ = MyApp_CL* zwraca wszystkie rekordy z zebranych dziennika.

![Kwerenda dziennika bez pól niestandardowych](media/log-analytics-data-sources-custom-logs/query-01.png)

### <a name="parse-the-custom-log-entries"></a>Analizowanie wpisy dziennika niestandardowego

Firma Microsoft korzysta z pól niestandardowych do definiowania *EventTime*, *Kod*, *Stan*, a pola *wiadomości* i firma Microsoft może wyświetlać różnica w rekordach, które są zwracane przez zapytanie.

![Kwerenda dziennika z polami niestandardowymi](media/log-analytics-data-sources-custom-logs/query-02.png)

## <a name="next-steps"></a>Następne kroki

- Za pomocą [Pól niestandardowych](log-analytics-custom-fields.md) do analizowania wpisów w dzienniku niestandardowych do poszczególnych pól.
- Informacje na temat [wyszukiwania dziennika](log-analytics-log-searches.md) do analizowania danych zebranych ze źródła danych i rozwiązań. 