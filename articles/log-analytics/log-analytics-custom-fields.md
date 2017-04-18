<properties
   pageTitle="Pola niestandardowe do analizy dziennika | Microsoft Azure"
   description="Funkcję pola niestandardowe analizy dziennika pozwala utworzyć własne pola wyszukiwania z danych usługi OMS Dodawanie we właściwościach rekordu zbierane.  W tym artykule opisano proces, aby utworzyć pole niestandardowe, a także szczegółowe instrukcje ze zdarzeniem próbki."
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

# <a name="custom-fields-in-log-analytics"></a>Pola niestandardowe do analizy dziennika

Funkcja **Pola niestandardowe** analizy dziennika pozwala na rozszerzenie istniejących rekordów w repozytorium usługi OMS przez dodanie pola wyszukiwania.  Pola niestandardowe są automatycznie wypełniane na podstawie danych pobranych z innych właściwości w tym samym rekordzie.

![Omówienie pól niestandardowych](media/log-analytics-custom-fields/overview.png)

Na przykład przykładowego rekordu poniżej zawiera przydatne dane byłyby trudne do zauważenia w polu Opis zdarzenia.  Wyodrębnianie danych do oddzielnych właściwości udostępnia akcje, takie jak sortowanie i filtrowanie.

![Przycisk Wyszukaj dziennika](media/log-analytics-custom-fields/sample-extract.png)

>[AZURE.NOTE] Na podglądzie jest ograniczone do 100 pól w obszarze roboczym.  Ten limit zostaną rozwinięte, gdy ta funkcja osiągnie ogólnodostępną.

## <a name="creating-a-custom-field"></a>Tworzenie pola niestandardowego

Po utworzeniu pola niestandardowego dziennika analizy należy zrozumieć które dane użyte do wypełnienia jej wartość.  Jego korzysta z technologii Microsoft Research o nazwie FlashExtract szybkie identyfikowanie tych danych.  Zamiast konieczności podawania jawne instrukcje, analizy dziennika dowiaduje się o dane, które mają zostać wyodrębnione z przykładów, które są udostępniane.

W poniższych sekcjach przedstawiono procedury tworzenia pola niestandardowego.  W dolnej części tego artykułu jest skorzystać z wyodrębniania próbki.

> [!NOTE] Pole niestandardowe jest wypełniane podczas dodawania rekordów spełniających określone kryteria do magazynu danych usługi OMS, będą wyświetlane tylko w rekordów zbierane po utworzeniu pola niestandardowego.  Pole niestandardowe nie zostanie dodane do rekordów, które są już w magazynie danych po jego utworzeniu.

### <a name="step-1--identify-records-that-will-have-the-custom-field"></a>Krok 1 — zidentyfikować rekordy, które mają pola niestandardowego
Pierwszym krokiem jest zidentyfikować rekordy, które otrzymają pole niestandardowe.  [Przeszukiwanie dziennika standardowy](log-analytics-log-searches.md) , a następnie wybierz pozycję rekord ma pełnić rolę modelu analizy dziennika pokażemy z.  Po określeniu zamiar wyodrębnianie danych do pola niestandardowego, **Kreator wyodrębniania pole** zostanie otwarty, gdzie możesz sprawdzić poprawność i Uściślij kryteria.

2. Przejdź do **Wyszukiwania dziennika** i przy użyciu [kwerendy pobierane wszystkie rekordy](log-analytics-log-searches.md) zawierające pola niestandardowego.
2. Wybierz rekord, który użyje analizy dziennika ma pełnić rolę wzór wyodrębnianie danych, aby wypełnić pole niestandardowe.  Należy określić dane, które mają zostać wyodrębnione z tego rekordu, a analizy dziennika użyje tych informacji w celu określenia logiki do Wypełnij pola niestandardowego dla wszystkich rekordów podobne.
3. Kliknij przycisk, aby po lewej stronie w dowolnej właściwości tekstu rekordu i wybierz **wyodrębnić pola z**.
4. **Zostanie otwarty Kreator wyodrębniania pola**, a rekord, który wybrano jest wyświetlana w kolumnie **Przykładu głównego** .  Pole niestandardowe zostaną określone dla tych rekordów przy użyciu tych samych wartości w oknie dialogowym właściwości, które są zaznaczone.  
5. W przypadku zaznaczenia dokładnie to, czego potrzebujesz wybrać dodatkowe pola, aby zawęzić kryteria.  W celu zmiany wartości pól kryteria, możesz anulować i wybierz inny rekord spełniających odpowiednie kryteria.

### <a name="step-2---perform-initial-extract"></a>Krok 2 — wykonywanie początkowej Wyodrębnij.
Gdy została zidentyfikować rekordy, które będą mieć pole niestandardowe, należy określić dane, które mają zostać wyodrębnione.  Za pomocą tych informacji dziennika analizy będzie identyfikować wzorce podobne w rekordach podobne.  W kroku dzięki temu będzie można sprawdzić poprawność wyników i dostarczenia dalszych szczegóły dotyczące analizy dziennika do użycia w ich analizy.

1. Wyróżnianie tekstu w rekordzie próbki, który chcesz wypełnić pole niestandardowe.  Następnie zostanie wyświetlona przy użyciu okna dialogowego podaj nazwę w polu i wykonywanie początkowej Wyodrębnij.  Znaki ** \_CF** zostanie automatycznie dołączony.
2. Kliknij pozycję **Wyodrębnij** przeprowadzić analizę zbierane rekordów.  
3. Sekcje **Podsumowanie** i **Wyniki wyszukiwania** wyświetlić wyniki Wyodrębnij, można sprawdzić dokładność.  **Podsumowanie** Wyświetla używane do identyfikowania rekordów i zestawienia dla każdej z wartości danych określone kryteria.  **Wyniki wyszukiwania** zawiera szczegółowy wykaz rekordów spełniających kryteria.


### <a name="step-3--verify-accuracy-of-the-extract-and-create-custom-field"></a>Krok 3 — sprawdzić dokładność Wyodrębnij i tworzenie niestandardowego pola

Po wykonaniu początkowej Wyodrębnij analizy dziennika zostanie wyświetlony jego wyniki na podstawie danych, które już zostały zebrane.  Jeśli wynik wygląda na dokładną można utworzyć pole niestandardowe z nie dalszych działań.  Jeśli nie, następnie Uściślanie wyników, aby analizy dziennika można poprawić jego logiczny.

2.  Jeśli wartości w początkowej Wyodrębnij są poprawne, kliknij ikonę **Edytuj** obok niedokładne rekordu i zaznacz pozycję **Modyfikuj ten wyróżnienia** w celu modyfikowania zaznaczenia.
3.  Wpis jest kopiowana do sekcji **przykładowych** pod **Przykład głównego**.  Wyróżnienie w tym miejscu można dostosować w celu pomocy analizy dziennika opis zaznaczenia, które powinny być wprowadzone.
4.  Kliknij pozycję **Wyodrębnij** za pomocą nowe dane są interpretowane istniejących rekordów.  Wyniki mogą być zmieniane rekordów inną niż ta, którą właśnie modyfikacji oparte na ten nowy analizy.
5.  Kontynuuj dodać korekty, aż wszystkie rekordy Wyodrębnij poprawnie zidentyfikować dane do wypełnienia nowego pola niestandardowego.
6. Po zakończeniu pracy z wynikami, kliknij przycisk **Zapisz wyodrębnić** .  Pole niestandardowe jest teraz zdefiniowany, ale go nie będą dodawane do wszystkich rekordów jeszcze.
7.  Poczekaj, aż nowych rekordów spełniających określone kryteria, które mają być zbierane i ponownie uruchom wyszukiwania dziennika. Nowe rekordy powinny mieć pole niestandardowe.
8.  Użyj pola niestandardowego, takich jak inne właściwości rekordu.  Można go używać do agregacji i grupowanie danych i nawet go użyć do utworzenia nowe wnioski.


## <a name="viewing-custom-fields"></a>Wyświetlanie pola niestandardowe
W grupie Zarządzanie z kafelka **Ustawienia** usługi OMS pulpitu nawigacyjnego można wyświetlić listę wszystkich pól niestandardowych.  Wybierz **dane** , a następnie **pola niestandardowe** , aby wyświetlić listę wszystkich pól niestandardowych w obszarze roboczym.  

![Pola niestandardowe](media/log-analytics-custom-fields/list.png)

## <a name="removing-a-custom-field"></a>Usuwanie pola niestandardowego
Istnieją dwie metody usuwania pola niestandardowego.  Pierwsza jest opcja **Usuń** dla każdego pola, podczas przeglądania listy wykonane, zgodnie z powyższym opisem.  Inną metodą jest pobrać rekord i kliknij przycisk, aby po lewej stronie pola.  Menu ma odpowiednią opcję, aby usunąć pole niestandardowe.

## <a name="sample-walkthrough"></a>Przykładowe Instruktaż

Poniższej sekcji opisano pełny przykład tworzenia pola niestandardowego.  W tym przykładzie wyodrębnia nazwy usługi w zdarzeń systemu Windows, które wskazują usługę zmiana stanu.  To zależy od zdarzenia utworzonego przez Menedżera sterowania usługami w dzienniku systemu na komputerach z systemem Windows.  Jeśli ma się znaleźć w tym przykładzie, musi być [zbierania informacji zdarzeń dziennika systemu](log-analytics-data-sources-windows-events.md).

Firma Microsoft wprowadź poniższe zapytanie zwraca wszystkie zdarzenia z Menedżera sterowania usługami zawierających Identyfikatorem zdarzenia 7036, czyli zdarzenia, która wskazuje usługę uruchamianie lub zatrzymywanie.

![Kwerendy](media/log-analytics-custom-fields/query.png)

Firma Microsoft, a następnie wybierz rekord ze zdarzeniem 7036 identyfikator.

![Źródło rekordów](media/log-analytics-custom-fields/source-record.png)

Chcemy nazwy usługi, który pojawia się we właściwości **RenderedDescription** i wybierz przycisk obok tej właściwości.

![Wyodrębnianie pola](media/log-analytics-custom-fields/extract-fields.png)

Zostaje otwarty **Kreator wyodrębniania pola** , a w kolumnie **Przykład główne** są zaznaczone pola **zdarzeń** i **identyfikator zdarzenia** .  Oznacza to, że pole niestandardowe zostaną określone dla zdarzenia w dzienniku systemu z Identyfikatorem zdarzenia 7036.  Jest to wystarczające, więc nie trzeba wybrać inne pola.

![Przykład głównym](media/log-analytics-custom-fields/main-example.png)

Firma Microsoft wyróżnij nazwę usługi we właściwości **RenderedDescription** i określ nazwę usługi za pomocą **usługi** .  Pole niestandardowe zostanie wywołana **Service_CF**.

![Pole tytułu](media/log-analytics-custom-fields/field-title.png)

Widać, że nazwa usługi określono poprawnie dla niektórych rekordów, ale nie do innych osób.   **Wyniki wyszukiwania** wskazują, że część nazwy **Karta wydajności WMI** nie został zaznaczony.  **Podsumowanie** pokazano, że cztery rekordy z usługą **DPRMA** niepoprawnie zawarte dodatkowego programu word, a dwa rekordy zidentyfikował **Instalator modułów** zamiast **Instalator modułów systemu Windows**.  

![Wyniki wyszukiwania](media/log-analytics-custom-fields/search-results-01.png)

Firma Microsoft rozpoczyna się **Karta wydajności WMI** rekordu.  Firma Microsoft kliknij jego ikonę Edytuj, a następnie **Modyfikuj ten wyróżnienia**.  

![Modyfikowanie wyróżnienia](media/log-analytics-custom-fields/modify-highlight.png)

Firma Microsoft zwiększanie wyróżnienie, aby wpisać **WMI** , a następnie uruchom ponownie wyodrębnij.  

![Przykład dodatkowe](media/log-analytics-custom-fields/additional-example-01.png)

Firma Microsoft widać, że zostały poprawione pozycje **Karta wydajności WMI** i analizy dziennika umożliwia również te informacje naprawianie rekordy dla **Instalatora Windows modułu**.  Firma Microsoft widoczne w sekcji **Podsumowanie** Chociaż tego **DPMRA** jest nadal nie są podane poprawnie.

![Wyniki wyszukiwania](media/log-analytics-custom-fields/search-results-02.png)

Firma Microsoft przejdź do rekordu z usługą DPMRA i wykonaj te same kroki, aby poprawić tego rekordu.

![Przykład dodatkowe](media/log-analytics-custom-fields/additional-example-02.png)

 Uruchomienie wyodrębnianie, możemy Zobacz, że teraz są wszystkie naszych wyniki odpowiednie.

![Wyniki wyszukiwania](media/log-analytics-custom-fields/search-results-03.png)

Widać, że **Service_CF** zostanie utworzona, ale jeszcze nie zostanie dodany do wszystkich rekordów.

![Liczba początkowa](media/log-analytics-custom-fields/initial-count.png)

Po upływie trochę czasu, więc nowego zdarzenia są zbierane, są wyświetlane, czy pole **Service_CF** jest teraz dodany do rejestruje pasujących naszych kryteriów.

![Ostateczne wyniki](media/log-analytics-custom-fields/final-results.png)

Teraz możemy użyć pola niestandardowego, takich jak inne właściwości rekordu.  Aby to zilustrować, możemy utworzyć kwerendę, która są grupowane według nowe pole **Service_CF** do sprawdzenia usługi, które są najbardziej aktywne.

![Grupowanie według kwerendy](media/log-analytics-custom-fields/query-group.png)

## <a name="next-steps"></a>Następne kroki

- Informacje na temat [wyszukiwania dziennika](log-analytics-log-searches.md) do tworzenia zapytań przy użyciu pól niestandardowych kryteriów.
- Monitorowanie [niestandardowych plików dziennika](log-analytics-data-sources-custom-logs.md) , które można przeanalizować za pomocą pól niestandardowych.