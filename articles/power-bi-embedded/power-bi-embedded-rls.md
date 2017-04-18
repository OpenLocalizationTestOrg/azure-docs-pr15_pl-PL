<properties
   pageTitle="Zabezpieczenia na poziomie wiersza z Power BI osadzony"
   description="Szczegóły dotyczące zabezpieczeń na poziomie wiersza z Power BI osadzony"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="row-level-security-with-power-bi-embedded"></a>Zabezpieczeń na poziomie wierszy z Power BI osadzony

Zabezpieczeń na poziomie wierszy (kontrola dostępu) może służyć do ograniczenia dostępu użytkowników do danych w raporcie lub zestawu danych, umożliwiająca wielu innym użytkownikom korzystanie z tym samym raportu podczas oglądanie różnych dane. Power BI osadzony obsługuje obecnie skonfigurowany ze kontroli zestawy danych.

![](media\power-bi-embedded-rls\pbi-embedded-rls-flow-1.png)

Aby móc skorzystać z kontroli, należy zapoznać się trzy główne pojęcia; Użytkownicy, role i reguły. Przyjrzyjmy się bliżej przyjrzeć się każdej:

**Użytkownicy** — są to rzeczywista użytkowników końcowych raportów. W Power BI osadzony użytkownicy są oznaczone za pomocą właściwości nazwy użytkownika w Token aplikacji.

**Role** — użytkownicy należeć do roli. Rola jest kontenera reguł i może być nazwana like "Menedżer sprzedaży" lub "Stanowisku Przedstawiciel handlowy". W Power BI osadzony użytkownicy są oznaczone za pomocą właściwości ról w Token aplikacji.

**Reguły** — role mają reguły, a te reguły są rzeczywiste filtry, które mają być stosowane do danych. Może to być tak proste, jak "Kraj = USA" lub coś bardziej dynamiczne.

### <a name="example"></a>Przykład

Dla dalszej części tego artykułu udostępniamy będzie przykład tworzenia kontroli i używające który aplikacji osadzony. Naszym przykładzie użyto [Detalicznej analizy przykładowy](http://go.microsoft.com/fwlink/?LinkID=780547) plik PBIX.

![](media\power-bi-embedded-rls\pbi-embedded-rls-scenario-2.png)

Nasze przykładowe analizy sprzedaży detalicznej przedstawia sprzedaż dla wszystkich sklepów w łańcuchu detalicznej określony. Bez kontroli, niezależnie od tego, które okręg Menedżer znaki w i widoków raportu, widoczne tych samych danych. Zarządzanie wyższych wykrył menedżerów okręg powinna być widoczna tylko sprzedaż w sklepach, które zarządzają, a w tym celu firma Microsoft korzysta kontroli.

Kontrola dostępu został utworzony w Power BI Desktop. Po otwarciu zestawu danych i raportów, możemy przejść do widoku diagramu, aby wyświetlić schemat:

![](media\power-bi-embedded-rls\pbi-embedded-rls-diagram-view-3.png)

Poniżej przedstawiono kilka sposobów Zwróć uwagę tego schematu:

-   Wszystkie miary, takich jak **Suma sprzedaży**, są przechowywane w tabeli faktów **sprzedaży** .
-   Istnieją cztery dodatkowy wymiar powiązanych tabel: **element**, **czasu** **sklepu**i **region**.
-   Strzałki na linii relacji wskazują, jaki sposób filtry przepływał z jednej tabeli do drugiego. Na przykład filtr jest umieszczony na **czas [Data]**, w bieżącym schemacie go chcesz filtrować tylko wartości w tabeli **Sales** . Żadnych innych tabel wpłynie ten filtr, ponieważ wszystkie strzałki na linii relacji wskazywać tabeli sprzedaży, a nie z dala od komputera.
-   Tabela **okręg** wskazuje, kto jest menedżerem dla każdego okręgu:

    ![](media\power-bi-embedded-rls\pbi-embedded-rls-district-table-4.png)

Na podstawie tego schematu, jeśli firma Microsoft zastosować filtr do kolumny **Kierownik okręg** w tabeli region, a w przypadku którego filtrowania dopasowania użytkownika raport wyświetlany ten filtr będzie również filtrować w dół tabeli **sklepu** i **Sprzedaż** celu wyświetlenia tylko dane dla tego określonego okręg menedżera.

Oto jak:

1.  Na karcie modelowania kliknij pozycję **Zarządzanie rolami**.  
![](media\power-bi-embedded-rls\pbi-embedded-rls-modeling-tab-5.png)

2.  Tworzenie nowej roli o nazwie **Menedżer**.  
![](media\power-bi-embedded-rls\pbi-embedded-rls-manager-role-6.png)

3.  W tabeli **okręg** należy wprowadzić następujące wyrażenie języka DAX: **[Region Manager] = USERNAME()**  
![](media\power-bi-embedded-rls\pbi-embedded-rls-manager-role-7.png)

4.  Aby upewnić się, że działają zasady, na karcie **modelowania** kliknij pozycję **Wyświetl jako role**, a następnie wprowadź następujące czynności:  
![](media\power-bi-embedded-rls\pbi-embedded-rls-view-as-roles-8.png)

    Raporty teraz będą wyświetlane dane, tak jakby były zalogować się jako **Osoby o imieniu Andrzej Ma**.

Stosowanie filtru zakresu, sposobu rozmieszczenia NAS tutaj będzie filtrowanie dół wszystkie rekordy z tabeli **region**, **Przechowywanie**i **Sprzedaż** . Jednak ze względu na kierunek filtru na relacje między **sprzedaży** i **czas**, tabele **sprzedaży** i **elementu**, a **element** i **czasu** będą nie filtrowane w dół.

![](media\power-bi-embedded-rls\pbi-embedded-rls-diagram-view-9.png)

Może to być ok tego wymogu, jednak jeśli firma Microsoft nie ma być menedżerów, aby wyświetlić elementy, dla których nie masz żadnej sprzedaży, firma Microsoft może włączyć dwukierunkowy filtrowaniu krzyżowym relacji i przepływ filtr zabezpieczeń w obu kierunkach. Można to zrobić, edytując relacji między **sprzedaży** i **element**, tak jak poniżej:

![](media\power-bi-embedded-rls\pbi-embedded-rls-edit-relationship-10.png)

Teraz filtry również przepływał z tabeli Sales do tabeli **elementu** :

![](media\power-bi-embedded-rls\pbi-embedded-rls-diagram-view-11.png)

**Uwaga** Jeśli używasz trybu zapytania bezpośredniego dla Twoich danych, należy włączyć filtrowanie krzyżowe dwukierunkowe, wybierając te dwie opcje:

1.  **Plik** -> **opcji i ustawień** -> **Funkcji podglądu** -> **Włącz krzyżowe filtrowanie w obu kierunków dla zapytania bezpośredniego**.
2.  **Plik** -> **opcji i ustawień** -> **DirectQuery** -> **umożliwić nieograniczony miary w trybie DirectQuery**.


Aby dowiedzieć się więcej o filtrowaniu krzyżowym dwukierunkowego, Pobierz dokument [dwukierunkowy filtrowanie krzyżowe w SQL Server Analysis Services 2016 i Power BI Desktop](http://download.microsoft.com/download/2/7/8/2782DF95-3E0D-40CD-BFC8-749A2882E109/Bidirectional cross-filtering in Analysis Services 2016 and Power BI.docx) .

To jest zawijany wszystkich pracy, które należy wykonać w Power BI Desktop, ale istnieje jeden więcej elementowi pracy, którą należy wykonać w celu kontroli reguł pracy firma Microsoft zdefiniowane w Power BI osadzony. Użytkownicy uwierzytelniania i autoryzacji przez aplikację i tokeny aplikacji są używane do udzielania dostępu tego użytkownika do określonych Power BI osadzonego raportu. Power BI osadzony nie ma dowolne informacje o na kto jest użytkownika. Kontrola dostępu do pracy musisz przekazać niektóre dodatkowy kontekst jako część token aplikacji:
-   **Nazwa użytkownika** (opcjonalnie) – używane z kontrola dostępu jest to ciąg, który może służyć do identyfikowania użytkownika w przypadku stosowania reguł kontroli. Zobacz Korzystanie z wiersza osadzony zabezpieczeń na poziomie dzięki usłudze Power BI
-   **role** — ciągu zawierającego role, aby określić, kiedy stosowanie zasad zabezpieczeń na poziomie wiersza. Jeśli przekazywanie więcej niż jedna rola, mają być przekazywane jako tablicę ciągów.

Jeśli ma właściwość username możesz również przekazać co najmniej jedną wartość ról.

Token pełnego aplikacja będzie wyglądał następująco:

![](media\power-bi-embedded-rls\pbi-embedded-rls-app-token-string-12.png)

Teraz z wszystkie wymagane elementy razem podczas logowania użytkownika do naszych aplikacji, aby wyświetlić raport, będzie tylko one widoczne dane, które będą mogły wyświetlić, zdefiniowane przez naszych zabezpieczeń na poziomie wiersza.

![](media\power-bi-embedded-rls\pbi-embedded-rls-dashboard-13.png)

## <a name="see-also"></a>Zobacz też
[Zabezpieczenia na poziomie wiersza (kontrola dostępu) z Power](https://powerbi.microsoft.com/en-us/documentation/powerbi-admin-rls/)
