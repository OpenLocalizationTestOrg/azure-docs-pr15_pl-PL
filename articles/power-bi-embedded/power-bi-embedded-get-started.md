<properties
   pageTitle="Wprowadzenie do programu Microsoft Power BI osadzony"
   description="Power BI osadzony, Dodaj interakcyjnych raportów usługi Power BI do aplikacji analizy biznesowej"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="get-started-with-microsoft-power-bi-embedded"></a>Wprowadzenie do programu Microsoft Power BI osadzony

**Power BI osadzony** jest usługą Azure, która umożliwia deweloperów aplikacji dodać interakcyjnych raportów usługi Power BI do własnych aplikacji. **Power BI osadzony** współdziała z istniejącymi aplikacjami bez konieczności zmianom lub zmienianie przez użytkowników Zaloguj się.

Zasoby dotyczące **Programu Microsoft Power BI osadzony** zainicjowano obsługę administracyjną za pośrednictwem [Interfejsów API ARM Azure](https://msdn.microsoft.com/library/mt712306.aspx). W tym przypadku zasobów, które można dodawać to **Kolekcja obszaru roboczego usługi Power BI**.

![](media\power-bi-embedded-get-started\introduction.png)

## <a name="create-a-workspace-collection"></a>Tworzenie zbioru obszaru roboczego
**Obszar roboczy zbioru** jest zasobów Azure najwyższego poziomu i kontenerem zawartości, która zostanie osadzony w aplikacji. **Kolekcja obszaru roboczego** można tworzyć na dwa sposoby:

   -    Ręcznie przy użyciu Azure Portal
   -    Programowo przy użyciu interfejsów API Manager(ARM) zasobów Azure

Przejdźmy kroki tworzenia **Zbioru obszaru roboczego** przy użyciu Azure Portal.

   1.   Otwieranie i zalogować się do **Portalu Azure**: [http://portal.azure.com](http://portal.azure.com).

   2.   Kliknij pozycję **+ Nowe** w górnym panelu.

       ![](media\power-bi-embedded-get-started\create-workspace-1.png)

   3.   W obszarze **danych + analizy** kliknij pozycję **Power BI osadzony**.
   4.   Na **Karta Tworzenie**wprowadź wymagane informacje. Dla **ceny**zobacz [Power BI osadzony ceny](http://go.microsoft.com/fwlink/?LinkID=760527).

       ![](media\power-bi-embedded-get-started\create-workspace-2.png)

   5. Kliknij przycisk **Utwórz**.

**Obszar roboczy zbioru** potrwa kilka chwil świadczenia. Po zakończeniu, zostaniesz przeniesiony do **Obszaru roboczego zbioru karta**.

   ![](media\power-bi-embedded-get-started\create-workspace-3.png)

**Karta Tworzenie** zawiera informacje potrzebne do połączeń interfejsów API tworzenie obszarów roboczych i wdrażanie ich zawartości.

<a name="view-access-keys"/>
## <a name="view-power-bi-api-access-keys"></a>Klawisze dostępu do wyświetlania Power BI interfejsu API

Jedna z najważniejszych informacji potrzebnych do połączeń API pozostałych Power BI są **Klawiszy dostępu**. Są one używane do generowania **tokenów aplikacji** służą do uwierzytelniania wezwaniach interfejsu API. Aby wyświetlić **Klawisze dostępu**, kliknij przycisk **Klawiszy dostępu** na **Karta Ustawienia**. Aby uzyskać więcej informacji o **tokeny aplikacji**zobacz [Authenticating i autoryzacji z Power BI osadzony](power-bi-embedded-app-token-flow.md).

   ![](media\power-bi-embedded-get-started\access-keys.png)

Po otwarciu "powiadomienie, że masz dwóch klawiszy.

   ![](media\power-bi-embedded-get-started\access-keys-2.png)

Skopiuj te klawisze i bezpieczne przechowywanie w aplikacji. Jest bardzo ważne, że są traktowane klucze jak hasła, ponieważ będzie zapewniają dostęp do całej zawartości w **Zbiorze obszaru roboczego**.

Gdy znajdują się dwa klucze, w określonym czasie jest potrzebny tylko jeden klucz. Drugiego klucza jest dostępna, więc można okresowo ponownie wygenerować klucze, bez przerywania dostępu do usługi.

Teraz, gdy masz wystąpienia usługi Power BI dla aplikacji i **Klawisze dostępu**, można zaimportować raportu do własnych aplikacji. Przed możesz dowiedzieć się, jak zaimportować raportu, następnej sekcji w tym artykule opisano tworzenie zestawów danych usługi Power BI i raporty do osadzania do aplikacji.

## <a name="create-power-bi-datasets-and-reports-to-embed-into-an-app"></a>Tworzenie zestawów danych usługi Power BI i raportów do osadzania do aplikacji

Teraz, gdy utworzono wystąpienie usługi Power BI dla aplikacji, a następnie **klawisze**, należy utworzyć zestawy danych usługi Power BI i raportów, które chcesz osadzić. Zestawy danych i raportów można utworzyć przy użyciu **Power BI Desktop**. Możesz pobrać [bezpłatne Power BI Desktop dla](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/). Lub, aby szybko rozpocząć pracę, można pobrać [detalicznej analizy próbki PBIX] (http://go.microsoft.com/fwlink/?LinkID=780547). Aby dowiedzieć się więcej o używaniu **Power BI Desktop**, zobacz [Wprowadzenie do programu Power BI Desktop](https://powerbi.microsoft.com/en-us/guided-learning/powerbi-learning-0-2-get-started-power-bi-desktop).

**Power BI Desktop**łączenie się ze źródłem danych przez importowanie **Power BI Desktop** kopię danych lub łączenia bezpośrednio do źródła danych za pomocą **zapytania bezpośredniego**.

Oto różnice między używaniem **importu** i **zapytania bezpośredniego**.

|Importowanie | Zapytania bezpośredniego
|---|---
|Tabel, kolumn *i dane* są importowane lub kopiowane do **Power BI Desktop**. Podczas pracy z wizualizacjami **Power BI Desktop** wysyła kopię danych. Aby zobaczyć zmiany, które wystąpiły danych źródłowych, możesz odświeżyć lub zaimportować wykonane, bieżący zestaw ponownie danych.|Tylko *tabel i kolumn* są importowane lub kopiowane do **Power BI Desktop**. Podczas pracy z wizualizacjami **Power BI Desktop** kwerendy źródła danych, co oznacza, że wyświetlasz zawsze aktualne dane.

Aby uzyskać więcej informacji o połączenie ze źródłem danych zobacz [Łączenie się ze źródłem danych](power-bi-embedded-connect-datasource.md).

Po zapisaniu pracy w **Power BI Desktop**jest tworzony plik PBIX. Ten plik zawiera raport. Ponadto w przypadku importowania danych PBIX zawiera pełny zestaw danych lub jeśli używasz **zapytania bezpośredniego**PBIX zawiera tylko schemat zestawu danych. Programowy wdrożyć PBIX do obszaru roboczego przy użyciu [Interfejsu API programu Power BI importu](https://msdn.microsoft.com/library/mt711504.aspx).

> [AZURE.NOTE] **Power BI osadzony** zawiera dodatkowe interfejsy API można zmienić serwer i bazę danych, który wskazuje swojego zestawu danych i ustawić poświadczenia konta usługi, który zestawu danych będzie używany do połączenia z bazą danych. Zobacz [SetAllConnections wpis](https://msdn.microsoft.com/library/mt711505.aspx) i [poprawki bramy w źródle danych](https://msdn.microsoft.com/library/mt711498.aspx).

## <a name="next-steps"></a>Następne kroki
W poprzednich krokach utworzono zbioru obszaru roboczego, a pierwszy raport i zestaw danych. Nadszedł czas, aby dowiedzieć się, jak pisać kod dla **Power BI osadzony**. Ułatwiające rozpoczęcie pracy, utworzonych aplikacji sieci web dla próbki: [Rozpoczynanie pracy z próbki](power-bi-embedded-get-started-sample.md). Próbki pokazano, jak do:

  - Obsługa administracyjna zawartości
      - Tworzenie obszaru roboczego
      - Importowanie pliku PBIX
      - Zaktualizuj parametry połączenia i Ustawianie poświadczeń dla swojego zestawy danych.

  - Bezpieczne osadzanie raportu

## <a name="see-also"></a>Zobacz też
- [Rozpoczynanie pracy z próbki](power-bi-embedded-get-started-sample.md)
- [Uwierzytelnianie i autoryzacji z Power BI osadzony](power-bi-embedded-app-token-flow.md)
- [Power BI desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)
