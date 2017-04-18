<properties
    pageTitle="Dodawanie opóźnienie w aplikacjach logiczny | Microsoft Azure"
    description="Omówienie opóźnienie i opóźnienie — do akcji i jak używać ich z aplikacji logika Azure."
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

# <a name="get-started-with-the-delay-and-delay-until-actions"></a>Rozpoczynanie pracy z opóźnienie i opóźnienie — do akcji

Za pomocą opóźnienie i "opóźnienie — do momentu" akcje, można wykonać scenariusze przepływu pracy.

Na przykład można wykonywać następujące czynności:

- Poczekaj, aż do dnia tygodnia przesyłanie aktualizacji stanu w wiadomości e-mail.
- Opóźnianie przepływu pracy, dopóki połączenia HTTP czasu na zakończenie przed wznawianie i pobierania wyników.

Aby rozpocząć pracę przy użyciu akcji opóźnienie w aplikacji logiczny, zobacz [Tworzenie aplikacji logicznych](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="use-the-delay-actions"></a>Używanie akcji opóźnienie

Akcja jest operację, która jest prowadzone przez przepływ pracy, który jest definiowana w aplikacji logicznych. [Dowiedz się więcej o akcje](connectors-overview.md).

Oto przykładowa sekwencja jak za pomocą krok opóźnienie w aplikacji logiczny:

1. Po dodaniu wyzwalacza, kliknij przycisk **Nowy krok** , aby dodać akcję.
2. Wyszukaj **opóźnienia** wyświetlić akcje opóźnienie. W tym przykładzie, firma Microsoft wybierze **opóźnienie**.

    ![Opóźnienie akcje](./media/connectors-native-delay/using-action-1.png)

3. Wykonaj dowolną z właściwości akcji, aby skonfigurować opóźnienie.

    ![Opóźnienie konfiguracji](./media/connectors-native-delay/using-action-2.png)

4. Kliknij przycisk **Zapisz** opublikować i aktywować tę aplikację logicznych.


## <a name="action-details"></a>Szczegóły akcji

Wyzwalacza cyklu zawiera następujące właściwości, które można skonfigurować.

### <a name="delay-action"></a>Opóźnienie akcji

Ta akcja opóźnienia budżet w określonym przedziale czasu.
A * oznacza, że jest to pole wymagane.

|Nazwa wyświetlana|Nazwa właściwości|Opis|
|---|---|---|
|Zliczanie *|Liczba|Liczba jednostek czasu opóźnienia|
|Jednostka *|jednostki|Jednostka czasu: `Second`, `Minute`, `Hour`, lub`Day`|
<br>

### <a name="delay-until-action"></a>Opóźnienie — do akcji

Ta akcja opóźnienia Uruchom do określonej daty/godziny.
A * oznacza, że jest to pole wymagane.

|Nazwa wyświetlana|Nazwa właściwości|Opis|
|---|---|---|
|Rok *|Sygnatura czasowa|Od początku roku do opóźnienia do (GMT)|
|Miesiąc *|Sygnatura czasowa|Miesiąc opóźnienia do (GMT)|
|Dzień *|Sygnatura czasowa|Dzień opóźnienia do (GMT)|
<br>


## <a name="next-steps"></a>Następne kroki

Teraz wypróbować platformy i [Tworzenie aplikacji logicznych](../app-service-logic/app-service-logic-create-a-logic-app.md). Zapoznaj się dostępne łączniki w aplikacjach logiczny, sprawdzając naszej [listy interfejsów API](apis-list.md).
