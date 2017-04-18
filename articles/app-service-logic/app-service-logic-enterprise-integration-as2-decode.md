<properties 
    pageTitle="Więcej informacji na temat Enterprise Integracja z dodatkiem Service Pack dekodowanie AS2 wiadomości Connctor | Microsoft Azure aplikacji usługi | Microsoft Azure" 
    description="Dowiedz się, jak używać partnerów za pomocą aplikacji Enterprise Integracja z dodatkiem Service Pack i układy logiczne" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="padmavc" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="padmavc"/>

# <a name="get-started-with-decode-as2-message"></a>Rozpoczynanie pracy z dekodowanie AS2 wiadomości

Połącz ze dekodowanie wiadomości AS2 w celu ustanowienia zabezpieczeń i niezawodności podczas przesyłania wiadomości. Umożliwia cyfrowe podpisywanie, odszyfrowywanie i potwierdzenia za pośrednictwem wiadomości powiadomienia dyspozycji (MDN).

## <a name="create-the-connection"></a>Utwórz połączenie

### <a name="prerequisites"></a>Wymagania wstępne

* Konto Azure; można utworzyć [bezpłatne konto](https://azure.microsoft.com/free)

* Aby użyć AS2 dekodowanie wiadomości łącznika jest wymagane konto integracji. Szczegółowe informacje na temat tworzenia [Konta integracji](./app-service-logic-enterprise-integration-create-integration-account.md), [partnerami](./app-service-logic-enterprise-integration-partners.md) i [AS2 umowy](./app-service-logic-enterprise-integration-as2.md)

### <a name="connect-to-decode-as2-message-using-the-following-steps"></a>Nawiązywanie połączenia wiadomości AS2 dekodowanie, wykonując następujące czynności:

1. [Tworzenie aplikacji logiczny](./app-service-logic-create-a-logic-app.md) zawiera przykład.

2. Ten łącznik nie zawiera wszystkie wyzwalacze. Uruchom aplikację logicznych, takich jak wyzwalacz wezwanie na za pomocą innych wyzwalaczy.  W Projektancie aplikacji logika dodać wyzwalacz i Dodaj akcję.  Wybierz pozycję Pokaż Microsoft zarządzany interfejs API programu na liście rozwijanej listy, a następnie wprowadź "AS2" w polu wyszukiwania.  Wybierz pozycję AS2 — dekodowanie AS2 wiadomości

    ![AS2 wyszukiwania](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage1.png)

3. Jeśli nie utworzono jeszcze wszystkie połączenia z kontem integracji, zostanie wyświetlony monit o informacje dotyczące połączenia

    ![Tworzenie połączenia integracji](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage2.png)

4. Wprowadź szczegółowe informacje o koncie integracji.  Właściwości gwiazdką są wymagane

  	| Właściwość   | Szczegóły |
  	| --------   | ------- |
  	| Nazwa połączenia *    | Wpisz dowolną nazwę połączenia |
  	| Integracja z programem konta * | Wprowadź nazwę konta integracji. Upewnij się, że Twoje konto integracji i logiki aplikacji są w tej samej lokalizacji Azure |

    Po zakończeniu szczegóły swojego połączenia wyglądać podobnie do następującej

    ![Integracja połączenia](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage3.png)

5. Wybierz opcję **Utwórz**
    
6. Zwróć uwagę, że utworzono połączenie.  Teraz kontynuować innych czynności w aplikacji warunków logicznych

    ![Integracja połączenia utworzonego](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage4.png) 

7. Wybierz z żądania wyjściowe treści i nagłówków

    ![Podaj wymagane pola](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage5.png) 

## <a name="the-as2-decode-does-the-following"></a>Dekodowanie AS2 wykonuje następujące czynności

* Przetwarza nagłówków AS2/HTTP
* Sprawdza podpis (jeśli został skonfigurowany)
* Odszyfrowuje wiadomości (jeśli został skonfigurowany)
* Dekompresuje wiadomości (jeśli został skonfigurowany)
* Druga otrzymanej MDN z oryginalnej wiadomości wychodzących
* Aktualizacje i uzależnia rekordów w bazie danych bez odrzucania
* Zapisuje rekordy AS2 tworzyć raporty o stanie
* Zawartość ładunku wynik jest zakodowany base64
* Określa, czy MDN jest wymagane i tego, czy MDN powinny być synchroniczne czy asynchroniczne na podstawie konfiguracji w niniejszej Umowie AS2
* Umożliwia generowanie MDN obraz lub asynchroniczna (w oparciu o konfiguracji umów)
* Ustawia właściwości i tokenów korelacji MDN

##<a name="try-it-for-yourself"></a>Wypróbuj ją dla siebie

Dlaczego nie spróbuj. Kliknij [tutaj](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/) aby wdrażanie aplikacji pełną funkcjonalność logiczny własne korzystania z funkcji logicznych AS2 aplikacji 

## <a name="next-steps"></a>Następne kroki

[Dowiedz się więcej o pakiecie integracji przedsiębiorstwa] (./app-service-logic-enterprise-integration-overview.md "Więcej informacji na temat Enterprise Integracja z dodatkiem Service Pack") 