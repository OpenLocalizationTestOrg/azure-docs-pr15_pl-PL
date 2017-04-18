<properties
    pageTitle="Dodawanie wyzwalacza cyklu w aplikacjach logiczny | Microsoft Azure"
    description="Przegląd wyzwalacza cyklu i jak z niego korzystać z aplikacji logika Azure."
    services=""
    documentationCenter=""
    authors="jeffhollan"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/18/2016"
   ms.author="jehollan"/>

# <a name="get-started-with-the-recurrence-trigger"></a>Wprowadzenie do wyzwalacza cyklu

Za pomocą wyzwalacza cyklu, mogą tworzyć zaawansowane przepływy pracy w chmurze.

Na przykład można wykonywać następujące czynności:

- Planowanie przepływ pracy ma uruchomić procedurę Przechowywaną każdego dnia.
- Wyślij wiadomość e-mail podsumowanie wszystkich tweety w ostatnim tygodniu o niektórych hashtagu.

Aby rozpocząć pracę, za pomocą wyzwalacza cyklu w aplikacji logiczny, zobacz [Tworzenie aplikacji logicznych](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="use-a-recurrence-trigger"></a>Użyj wyzwalacza cyklu

Wyzwalacz to zdarzenie, które można uruchomić przepływ pracy, który jest definiowana w aplikacji logicznych. [Dowiedz się więcej o wyzwalaczy](connectors-overview.md).

Oto przykładowa sekwencja jak skonfigurować wyzwalacz cyklu w aplikacji logiczny:

1. Dodawanie wyzwalacza **cyklu** jako pierwszy krok w aplikacji logicznych.
2. Wprowadź parametry interwału cyklu.

Aplikacja logiczny rozpocznie się teraz Uruchom po każdym przedziale czasu.

![Wyzwalacza HTTP](./media/connectors-native-recurrence/using-trigger.png)

## <a name="trigger-details"></a>Szczegóły wyzwalacza

Wyzwalacza cyklu zawiera następujące właściwości, które można skonfigurować.

Aplikacja logiczny jest uruchamiana po w określonym interwale czasu.
A * oznacza, że jest to pole wymagane.

|Nazwa wyświetlana|Nazwa właściwości|Opis|
|---|---|---|
|Częstotliwość *|częstość|The unit of time: `Second`, `Minute`, `Hour`, `Day`, or `Year`.|
|Interwał *|Interwał|Interwał danego częstotliwość cyklu.|
|Strefa czasowa|Strefa czasowa|Czas rozpoczęcia są dostarczane bez przesunięcie czasu UTC, zostanie użyty tej strefie czasowej.|
|Czas rozpoczęcia|czas rozpoczęcia|Godzina rozpoczęcia w [formacie ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations).|
<br>


## <a name="next-steps"></a>Następne kroki

Teraz wypróbować platformy i [Tworzenie aplikacji logicznych](../app-service-logic/app-service-logic-create-a-logic-app.md). Zapoznaj się dostępne łączniki w aplikacjach logiczny, sprawdzając naszej [listy interfejsów API](apis-list.md).
