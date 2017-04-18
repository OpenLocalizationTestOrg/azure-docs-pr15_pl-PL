<properties
   pageTitle="Uwierzytelnianie i autoryzacji z Power BI osadzony"
   description="Uwierzytelnianie i autoryzacji z Power BI osadzony"
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

# <a name="authenticating-and-authorizing-with-power-bi-embedded"></a>Uwierzytelnianie i autoryzacji z Power BI osadzony

Usługa Power BI osadzony używa **klawiszy** i **Tokeny aplikacji** dla uwierzytelniania i autoryzacji, zamiast uwierzytelniania jawne użytkowników końcowych. W tym modelu aplikacji zarządza uwierzytelniania i autoryzacji dla użytkowników końcowych. W razie potrzeby aplikacji tworzy i wysyła tokeny aplikacji zachęcający z naszych usług do renderowania żądany raport. Ten projekt nie wymaga aplikacji do używania usługi Azure Active Directory dla uwierzytelniania użytkowników i autoryzacji, mimo że można nadal.

## <a name="two-ways-to-authenticate"></a>Dwie metody uwierzytelniania

**Klucz** — możesz użyć klawiszy dla wszystkich połączeń interfejsu API usługi REST osadzony Power BI. Klucze można znaleźć w **Azure portal** , klikając na **wszystkie ustawienia** , a następnie **klawisze dostępu**. Zawsze Traktuj klucza, tak jakby była hasła. Klucze mają uprawnienia do wprowadzania dowolnego interfejsu API usługi REST połączeń w zbiorze określonego obszaru roboczego.

W trakcie rozmowy pozostałych za pomocą klucza, Dodaj następujący nagłówek autoryzacji:            

    Authorization: AppKey {your key}

**Token aplikacji** — aplikacja tokeny są używane dla wszystkich żądań osadzania. Są one przeznaczone do uruchomienia po stronie klienta, aby ta osoba jest ograniczone do pojedynczego raportu i jest najlepiej ustawić czas wygaśnięcia.

Tokeny aplikacji są JWT (JSON Web Token) podpisaną przez jeden z kluczy.

Token aplikacji mogą zawierać następujące oświadczeniach:

| Roszczeń      | Opis        |
|--------------|------------|
| **Wersja**      | Wersja token aplikacji. 0.2.0 jest bieżącą wersję.       |
| **lub**      | Adresat tokenu. Do użytku Power BI osadzony: "https://analysis.windows.net/powerbi/api".  |
| **firmy**      |  Ciąg o wskazujący aplikacji, w której wystawiony token.    |
| **Typ**     | Typ tokenu aplikacji jest tworzony. Bieżący jedynego obsługiwanego typu jest **Osadzanie**.   |
| **WCN**      | Nazwa zbioru obszaru roboczego token został opublikowany dla.  |
| **szczególną uwagę**      | Identyfikator obszaru roboczego token został opublikowany dla.  |
| **Usuwanie**      | Identyfikator raportu token został opublikowany dla.     |
| **Nazwa użytkownika** (opcjonalnie) |  Używany z kontroli, to ciąg, który może pomóc w identyfikacji użytkownika w przypadku stosowania reguł kontroli. |
| **role** (opcjonalnie)   |   Ciąg zawierający role, aby określić, kiedy stosowanie zasad zabezpieczeń na poziomie wiersza. Jeśli przekazywanie więcej niż jedna rola, mają być przekazywane jako tablicę ciągu.    |
| **Exp** (opcjonalnie)    |   Wskazuje czas wygaśnięcia tokenu. Te mają być przekazywane w jako Unix sygnatury czasowe.   |
| **NBF** (opcjonalnie)    |   Wskazuje przy uruchamianiu w którym tokenu są prawidłowe. Te mają być przekazywane w jako Unix sygnatury czasowe.   |

Przykładowe tokenu aplikacji będzie wyglądać następująco:

![](media\power-bi-embedded-app-token-flow\power-bi-embedded-app-token-flow-sample-coded.png)


Gdy odczytany, będzie wyglądać podobnie do następującej:

![](media\power-bi-embedded-app-token-flow\power-bi-embedded-app-token-flow-sample-decoded.png)


## <a name="heres-how-the-flow-works"></a>Poniżej opisano, jak działa przepływ

1. Kopiowanie klawiszy interfejsu API do aplikacji. Możesz wyświetlić kluczy w **Azure Portal**.

    ![](media\powerbi-embedded-get-started-sample\azure-portal.png)

2. Token potwierdza roszczenia i zawiera czas wygaśnięcia.

    ![](media\powerbi-embedded-get-started-sample\power-bi-embedded-token-2.png)

3. Token otrzymuje zalogowani za pomocą interfejsu API klawiszy dostępu.

    ![](media\powerbi-embedded-get-started-sample\power-bi-embedded-token-3.png)

4. Żądania użytkowników, aby wyświetlić raport.

    ![](media\powerbi-embedded-get-started-sample\power-bi-embedded-token-4.png)

5.  Token sprawdzania poprawności przy użyciu interfejsu API klawiszy dostępu.

    ![](media\powerbi-embedded-get-started-sample\power-bi-embedded-token-5.png)

6.  Power BI osadzony wysyła raport do użytkownika.

    ![](media\powerbi-embedded-get-started-sample\power-bi-embedded-token-6.png)

Po **Power BI osadzony** wysyła raport do użytkownika, użytkownik może wyświetlić raport w niestandardową aplikację. Na przykład po zaimportowaniu [analizy sprzedaży PBIX danych przykładowych](http://download.microsoft.com/download/1/4/E/14EDED28-6C58-4055-A65C-23B4DA81C4DE/Analyzing_Sales_Data.pbix)aplikacji sieci web przykładowe wyglądałby następująco:

![](media\powerbi-embedded-get-started-sample\sample-web-app.png)

## <a name="see-also"></a>Zobacz też
- [Wprowadzenie do programu Microsoft Power BI osadzony próbki](power-bi-embedded-get-started-sample.md)
- [Typowe scenariusze Microsoft Power BI osadzony](power-bi-embedded-scenarios.md)
- [Wprowadzenie do programu Microsoft Power BI osadzony](power-bi-embedded-get-started.md)
