<properties
    pageTitle="Kreator kopii Factory danych | Microsoft Azure"
    description="Informacje na temat sposobu używania Kreatora kopiowania Factory danych do skopiowania danych z obsługiwanych źródeł danych do pochłaniacze."
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="spelluru"/>

# <a name="data-factory-copy-wizard"></a>Kreator kopii Factory danych
Kreator kopii Factory Azure danych jest uprościć proces ingesting danych, który jest zazwyczaj pierwszy krok w scenariuszu integracji zakończenia do końca danych. Gdy przechodzące przez kreatora kopiowania Factory danych Azure, nie należy zrozumieć inne definicje JSON na usługi połączone, zestawy danych i procesy. Jednak po wykonaniu wszystkich kroków w Kreatorze Kreator automatycznie tworzy potok, aby skopiować dane z wybranego źródła danych do wybranego miejsca. Ponadto kreatora kopiowania ułatwia sprawdzania poprawności danych jest wchłonięte w momencie tworzenia, co powoduje zapisanie większość czasu, zwłaszcza gdy użytkownik są ingesting danych po raz pierwszy ze źródła danych. Aby uruchomić Kreatora kopiowania, kliknij **Kopiowanie danych** na stronie głównej firmie danych.

![Kreator kopii](./media/data-factory-copy-wizard/copy-data-wizard.png)


## <a name="an-intuitive-wizard-for-copying-data"></a>Intuicyjny kreatora kopiowania danych
Ten kreator pozwala na łatwe przenoszenie danych z wielu różnych źródeł do miejsc docelowych w minutach. Po wykonaniu kreatora, potok aktywnością Kopiuj jest tworzona automatycznie dla Ciebie wraz z zależne jednostek Factory dane (połączone usług i zestawy danych). Żadne dodatkowe czynności są wymagane do utworzenia proces.   

![Wybieranie źródła danych](./media/data-factory-copy-wizard/select-data-source-page.png)

> [AZURE.NOTE] Zobacz artykuł [Samouczek Kreatora kopiowania](data-factory-copy-data-wizard-tutorial.md) instrukcje krok po kroku utworzyć potok próbki, aby skopiować dane z Azure blob do tabeli bazy danych SQL Azure. 

Kreator jest przeznaczony danymi duży pamiętać od początku. To proste i efektywne tworzenie procesy Factory danych, które przenoszą setki folderów, pliki lub tabel za pomocą Kreatora kopiowania danych. Kreator obsługuje trzy następujące funkcje: Podgląd danych, schematu przechwytywania i mapowania i filtrowania danych. 

## <a name="automatic-data-preview"></a>Podgląd danych automatycznych 
Kreator kopii umożliwia przeglądanie część danych z wybranego źródła danych umożliwiają sprawdzanie, czy dane są właściwych danych, które chcesz skopiować. Ponadto w przypadku danych źródłowych w pliku tekstowym, Kreator kopii analizuje plik tekstowy, aby dowiedzieć się wierszy i kolumn ograniczniki oraz schematu automatycznie. 

![Ustawienia formatu pliku](./media/data-factory-copy-wizard/file-format-settings.png)

## <a name="schema-capture-and-mapping"></a>Przechwytywanie schematu i mapowania 
Schemat danych wyjściowych w niektórych przypadkach może różnić się schemat danych wejściowych. W tym scenariuszu należy mapowanie kolumny z schematu źródła do kolumn z schematu docelowego. 

Kreator kopii map automatycznie kolumn w schemacie źródła do kolumn w schemacie miejsca docelowego. Możesz zastąpić mapowania przy użyciu list rozwijanych (lub) określ, czy kolumna musi zostać pominięte podczas kopiowania danych.   

![Mapowanie schematu](./media/data-factory-copy-wizard/schema-mapping.png)

## <a name="filtering-data"></a>Filtrowanie danych  
Kreator umożliwia filtrowanie danych źródłowych, aby zaznaczyć tylko dane, które należy można skopiować do miejsca docelowego sink magazynu danych. Filtrowanie powoduje zmniejszenie ilości danych, które mają być kopiowane do magazynu danych sink i w związku z tym zwiększa przepustowość kopiowania. Umożliwia elastyczne umożliwia filtrowanie danych w relacyjnej bazy danych za pomocą SQL kwerendy języka (lub) plików w folderze obiektów blob platformy Azure za pomocą [funkcji Factory danych i zmiennych](data-factory-functions-variables.md).   

### <a name="filtering-of-data-in-a-database"></a>Filtrowanie danych w bazie danych.  
W tym przykładzie użyto kwerendy SQL `Text.Format` funkcji i `WindowStart` zmiennej. 

![Sprawdź poprawność wyrażeń](./media/data-factory-copy-wizard/validate-expressions.png)

### <a name="filtering-of-data-in-an-azure-blob-folder"></a>Filtrowanie danych w folderze obiektów blob platformy Azure
Aby skopiować dane z folderu, który jest określona w czasie wykonywania na podstawie [systemu zmiennych](data-factory-functions-variables.md#data-factory-system-variables)można używać zmiennych w ścieżce folderu. Obsługiwane zmienne: **{rok}**, **{miesiąc}**, **{dnia}**, **{godzinę}**, **{minuta}**i **{niestandardowe}**. Przykład: inputfolder / {roku}-{miesiąc} {dzień}.

Załóżmy, że podania folderów w następującym formacie:

    2016/03/01/01
    2016/03/01/02
    2016/03/01/03
    ...

Kliknij przycisk **Przeglądaj** , **pliku**lub folderu, przejdź do jednej z tych folderów (na przykład 2016 -> 03 -> 01 -> 02) i kliknij przycisk **Wybierz**. Powinien zostać wyświetlony `2016/03/01/02` w polu tekstowym. Teraz zamienić **2016** **{roku}**, **03** z **{miesiącem}**, **01** z **{dnia}**i **02** z **{godzinę}**, a następnie naciśnij klawisz Tab. Powinien zostać wyświetlony list rozwijanych, aby wybrać format dla tych czterech zmiennych:

![Wykorzystywanie zmiennych systemu](./media/data-factory-copy-wizard/blob-standard-variables-in-folder-path.png)   

Jak pokazano na poniższej zrzut ekranu, umożliwia także zmienną **niestandardowych** i wszystkie [obsługiwane ciągi formatów](https://msdn.microsoft.com/library/8kb3ddd4.aspx). Aby wybrać folder o tej struktury, najpierw użyj przycisku **Przeglądaj** . Następnie zamienić wartości **{niestandardowe}**, a następnie naciśnij klawisz Tab, aby wyświetlić pole tekstowe, w którym można wpisać ciąg formatu.     

![Używanie niestandardowej zmiennej](./media/data-factory-copy-wizard/blob-custom-variables-in-folder-path.png)


## <a name="support-for-diverse-data-and-object-types"></a>Obsługa różnych danych i typy obiektów
Za pomocą Kreatora kopiowania, można przenieść efektywne setki folderów, pliki lub tabel.

![Wybierz tabele, z którego zostaną skopiowane dane](./media/data-factory-copy-wizard/select-tables-to-copy-data.png)

## <a name="scheduling-options"></a>Opcje harmonogramu
Operacja kopiowania można uruchamiać raz lub zgodnie z harmonogramem (co godzinę, codziennie, i tak dalej). Obu tych opcji może służyć do szerokości łączników lokalnego, chmura i pulpitu kopii lokalnej.

Operacja Kopiuj jednorazowy umożliwia przenoszenie danych ze źródła do miejsca docelowego tylko raz. Dotyczy danych dowolne rozmiary i dowolnego obsługiwanego formatu. Zaplanowane kopię umożliwia kopiowanie danych w określonym cyklu. Zaawansowanych ustawień (takich jak ponów próbę, limit czasu i alerty) umożliwia konfigurowanie kopii według harmonogramu.

![Planowanie właściwości](./media/data-factory-copy-wizard/scheduling-properties.png)


## <a name="next-steps"></a>Następne kroki
Aby krótki opis za pomocą Kreatora kopii Factory danych, aby utworzyć potok aktywnością kopii, zobacz [Samouczek: tworzenie potok przy użyciu Kreatora kopiowania](data-factory-copy-data-wizard-tutorial.md).
