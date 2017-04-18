<properties
    pageTitle="Alerty w czasie rzeczywistym Azure CDN | Microsoft Azure"
    description="W czasie rzeczywistym alerty w programie Microsoft Azure CDN. Alerty w czasie rzeczywistym stanowią powiadomienia dotyczące wydajności punkty końcowe w swoim profilu CDN."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="casoper"/>

# <a name="real-time-alerts-in-microsoft-azure-cdn"></a>W czasie rzeczywistym alerty w programie Microsoft Azure CDN

[AZURE.INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]


## <a name="overview"></a>Omówienie

W tym dokumencie omówiono alertów w czasie rzeczywistym w programie Microsoft Azure CDN. Ta funkcja zapewnia w czasie rzeczywistym powiadomienia dotyczące wydajności punkty końcowe w swoim profilu CDN.  Możesz skonfigurować adres e-mail lub alerty HTTP na podstawie:

* Przepustowości
* Kody stanu
* Statusy pamięci podręcznej
* Połączenia

## <a name="creating-a-real-time-alert"></a>Tworzenie alertu w czasie rzeczywistym

1. W [Azure Portal](https://portal.azure.com)przejdź do swojego profilu CDN.

    ![Karta profil sieci CDN](./media/cdn-real-time-alerts/cdn-profile-blade.png)

2. Z sieci CDN karta Profil kliknij przycisk **Zarządzaj** .

    ![Przycisk Zarządzaj CDN karta profilu](./media/cdn-real-time-alerts/cdn-manage-btn.png)

    Zostanie otwarty do portalu zarządzania CDN.

3. Umieść wskaźnik myszy na karcie **analizy** , a następnie umieść wskaźnik myszy na menu wysuwane **Statystykę w czasie rzeczywistym** .  Polecenie **alertów w czasie rzeczywistym**.

    ![Portal zarządzania CDN](./media/cdn-real-time-alerts/cdn-premium-portal.png)

    Zostanie wyświetlona lista istniejących konfiguracji alertu (jeśli istnieją).

4. Kliknij przycisk **Dodaj Alert** .

    ![Dodawanie przycisku alertów](./media/cdn-real-time-alerts/cdn-add-alert.png)

    Zostanie wyświetlony formularz do tworzenia nowego alertu.

    ![Nowy formularz alertu](./media/cdn-real-time-alerts/cdn-new-alert.png)

5. Jeśli chcesz, aby ten alert, aby była aktywna, po kliknięciu przycisku **Zapisz**, zaznacz pole wyboru **Włączone Alert** .

6. Wprowadź opisową nazwę dla alertu w polu **Nazwa** .

7. Na liście rozwijanej **Typ multimediów** wybierz **Obiekt dużych HTTP**.

    ![Typ multimediów zaznaczony obiekt dużych HTTP](./media/cdn-real-time-alerts/cdn-http-large.png)

    > [AZURE.IMPORTANT] Po zaznaczeniu **HTTP dużych obiektów** jako **Typ multimediów**.  Inne opcje nie są używane przez **CDN Azure z Verizon**.  Błąd, aby zaznaczyć **Obiekt dużych HTTP** spowoduje, że usługi alert, aby nigdy nie zostać wyzwolone.

8. Tworzenie **wyrażenia** do monitorowania, wybierając pozycję **metryczne**, **Operator**i **wartość wyzwalacza**.

    - **Metryka**wybierz typ warunku, który ma być monitorowane.  **MB/s przepustowości** jest kwotą wykorzystania przepustowości w MB na sekundę.  **Całkowita liczba połączeń** jest liczba równoległych połączeń HTTP do naszych serwerów krawędzi.  Definicje różnych statusy pamięci podręcznej i kody stanu zobacz [Kody stanu pamięci podręcznej CDN Azure](https://msdn.microsoft.com/library/mt759237.aspx) i [Kody stanu HTTP sieci CDN Azure](https://msdn.microsoft.com/library/mt759238.aspx)
    - **Operator** jest operatorów matematycznych, która określa relację między Metryka i wartość wyzwalacza.
    - **Wartość wyzwalacz** to wartość progowa, które muszą być spełnione, zanim zostanie wysłane powiadomienie.

    W poniższym przykładzie wyrażenie utworzonych przeze mnie wskazuje, czy chcę otrzymywać powiadomienia, gdy liczba 404 kodów stanu jest większa niż 25.

    ![Przykład alertu w czasie rzeczywistym wyrażenia](./media/cdn-real-time-alerts/cdn-expression.png)

9. **Interwał**wprowadź częstotliwości potem wyrażenie obliczane.

10. Na liście rozwijanej **powiadamiania na** wybierz, jeśli chcesz otrzymywać powiadomienia, gdy wyrażenie ma wartość PRAWDA.
    
    - **Rozpoczynanie warunek** wskazuje, że powiadomienie zostanie wysłany, gdy określony warunek najpierw zostanie wykryty.
    - **Koniec warunku** wskazuje, że powiadomienie zostanie wysłany, gdy określony warunek nie zostanie wykryty. To powiadomienie może być uruchomiona tylko po naszej sieci systemu monitorowania wykrył, że określony warunek miejsce.
    - **Ciągły** wskazuje, że powiadomienie zostanie wysłany w każdym że sieci systemu monitorowania wykrywa określony warunek. Należy pamiętać, który sieci systemu monitorowania sprawdza tylko raz na interwał określony warunek.
    - **Warunek początek i koniec** wskazuje, że zostanie wysłane powiadomienie po raz pierwszy że określony warunek zostanie wykryta i ponownie, gdy już nie zostanie wykryta warunek.

11. Jeśli chcesz otrzymywać powiadomienia pocztą e-mail, zaznacz pole wyboru **Powiadom pocztą E-mail** .  

    ![Powiadomienia o tym, w formularzu wiadomości E-mail](./media/cdn-real-time-alerts/cdn-notify-email.png)
    
    W polu **do** wprowadź adres e-mail, w którym chcesz powiadomienia o wysłaniu. **Temat** i **treść**można pozostawić domyślny lub można dostosować wiadomości za pomocą na liście **Dostępne słowa kluczowe** , aby dynamicznie wstawić dane alertów, gdy wiadomość jest wysyłana.

    > [AZURE.NOTE] Powiadomienie e-mail można sprawdzić, klikając przycisk **Testowanie powiadomienie** , ale dopiero po zapisaniu konfiguracji alertu.

12. Jeśli chcesz powiadomienia, która zostanie umieszczona na serwerze sieci web, zaznacz pole wyboru **Powiadom pocztą HTTP** .

    ![Powiadom przez HTTP Post formularza](./media/cdn-real-time-alerts/cdn-notify-http.png)

    W polu **adres Url** wprowadź adres URL, opublikowany w miejsce, w którym ma się znaleźć wiadomość HTTP. W polu tekstowym **nagłówki** wprowadź nagłówki HTTP do wysłania w wezwaniu.  Dla **treści** można dostosować wiadomości za pomocą na liście **Dostępne słowa kluczowe** , aby dynamicznie wstawić dane alertów, gdy wiadomość jest wysyłana.  **Nagłówki** i **treści** domyślne podobne do ładunku XML poniższym przykładzie.

    ```
    <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">
        <![CDATA[Expression=Status Code : 404 per second > 25&Metric=Status Code : 404 per second&CurrentValue=[CurrentValue]&NotificationCondition=Condition Start]]>
    </string>
    ```

    > [AZURE.NOTE] Powiadomienie o HTTP Post można sprawdzić, klikając przycisk **Testowanie powiadomienie** , ale dopiero po zapisaniu konfiguracji alertu.

13. Kliknij przycisk **Zapisz** , aby zapisać konfigurację alertu.  Jeśli w kroku 5 zaznaczone **Włączone Alert** , alertu jest obecnie aktywny.

## <a name="next-steps"></a>Następne kroki

- Analizowanie [statystykę w czasie rzeczywistym w sieci CDN Azure](cdn-real-time-stats.md)
- Wyświetlić elementy [zaawansowanych](cdn-advanced-http-reports.md) raportów HTTP
- Analizowanie [upodobania](cdn-analyze-usage-patterns.md)

