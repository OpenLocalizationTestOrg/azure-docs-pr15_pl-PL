<properties
   pageTitle="Osadzone Microsoft Power BI — połączenie ze źródłem danych"
   description="Power BI osadzony, połączenia ze źródłami danych"
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

# <a name="connect-to-a-data-source"></a>Nawiązywanie połączenia ze źródłem danych

Z **Power BI osadzony**można osadzić raportów do własnych aplikacji. Podczas raportu usługi Power BI możesz osadzić w aplikacji, raport łączy do danych źródłowych, **importując** kopię danych lub za pomocą **połączenia bezpośrednio** do źródła danych za pomocą **zapytania bezpośredniego**.

Oto różnice między używaniem **importu** i **zapytania bezpośredniego**.

|Importowanie | Zapytania bezpośredniego
|---|---
|Tabel, kolumn *i dane* są importowane lub kopiowane do zestawu danych w raporcie. Aby zobaczyć zmiany, które wystąpiły danych źródłowych, możesz odświeżyć lub zaimportować wykonane, bieżący zestaw ponownie danych.|Tylko *tabel i kolumn* są importowane lub kopiowane do zestawu danych w raporcie. Zawsze możesz wyświetlić najbardziej aktualne dane.
Z Power BI osadzony możesz za pomocą zapytania bezpośredniego źródła danych w chmurze, ale nie lokalnych źródeł danych w tej chwili.

## <a name="benefits-of-using-directquery"></a>Korzyści wynikające z używania zapytania bezpośredniego

Istnieją dwie główne korzyści wynikające podczas korzystania z **zapytania bezpośredniego**:

   -    **Zapytania bezpośredniego** umożliwia tworzenie wizualizacji na bardzo dużych zestawów danych, gdzie w przeciwnym razie będzie będzie niemożliwe do pierwszego importu wszystkie dane.
   -    Podstawowych zmian w danych może wymagać odświeżania danych, a w przypadku niektórych raportów potrzeba wyświetlania bieżących danych mogą wymagać przesyłania dużych danych, co ponownie importowania danych będzie niemożliwe. Natomiast **zapytania bezpośredniego** raportów zawsze bieżące dane dotyczące użycia.

## <a name="limitations-of-directquery"></a>Ograniczenia dotyczące zapytania bezpośredniego

   Istnieje kilka ograniczeń za pomocą **zapytania bezpośredniego**:

   -    Wszystkie tabele muszą pochodzić z jednej bazie danych.
   -    Jeśli kwerenda jest zbyt złożona, wystąpi błąd. Do usunięcia błędu musi refactor kwerendę, więc jest mniej skomplikowana. Quuery musi być złożone, należy zaimportować dane zamiast **zapytania bezpośredniego**.
   -    Filtrowanie relacji jest ograniczone do jednym kierunku zamiast obu kierunków.
   -    Nie można zmienić typu danych kolumny.
   -    Domyślnie ograniczenia są umieszczane na wyrażenia języka DAX dozwolone w miary. Zobacz [zapytania bezpośredniego i miar](#measures).

<a name="measures"/>
## <a name="directquery-and-measures"></a>Zapytania bezpośredniego i miar

Upewnij się, że wydajności kwerend wysyłanych do źródła danych, są ograniczenia nałożone na miary. Podczas korzystania z **Power BI Desktop**, zaawansowanych użytkowników można wybrać pominąć to ograniczenie, wybierając pozycję **Plik > Opcje i Ustawienia > Opcje**. W oknie dialogowym **Opcje** wybierz **zapytania bezpośredniego**, a następnie wybierz opcję **Zezwalaj na nieograniczony miar w trybie DirectQuery**. Po wybraniu tej opcji można dowolne wyrażenie języka DAX, który jest nieprawidłowy dla miary. Użytkownicy muszą mieć świadomość; jednak że niektóre wyrażenia które wykonują dobrze po zaimportowaniu danych może spowodować bardzo wolno zapytania w źródle wewnętrznej bazy danych w trybie **zapytania bezpośredniego** . 

## <a name="see-also"></a>Zobacz też
- [Wprowadzenie do programu Microsoft Power BI osadzony](power-bi-embedded-get-started.md)
- [Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)
