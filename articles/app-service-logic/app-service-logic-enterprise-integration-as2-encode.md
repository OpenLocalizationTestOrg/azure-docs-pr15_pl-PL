<properties 
    pageTitle="Więcej informacji na temat Enterprise Integracja z dodatkiem Service Pack kodowanie AS2 wiadomości Connctor | Microsoft Azure aplikacji usługi | Microsoft Azure" 
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

# <a name="get-started-with-encode-as2-message"></a>Wprowadzenie do kodowania AS2 wiadomości

Podłącz do kodowania wiadomości AS2 ustanawiania zabezpieczeń i niezawodności podczas przesyłania wiadomości. Umożliwia podpisywaniu cyfrowym, szyfrowaniu i potwierdzeń za pośrednictwem wiadomości dyspozycji powiadomienia (MDN), która również prowadzi do pomocy technicznej dotyczącej Niemożność wyparcia się.

## <a name="create-the-connection"></a>Utwórz połączenie

### <a name="prerequisites"></a>Wymagania wstępne

* Konto Azure; można utworzyć [bezpłatne konto](https://azure.microsoft.com/free)

* Aby użyć AS2 kodowanie wiadomości łącznika jest wymagane konto integracji. Szczegółowe informacje na temat tworzenia [Konta integracji](./app-service-logic-enterprise-integration-create-integration-account.md), [partnerami](./app-service-logic-enterprise-integration-partners.md) i [AS2 umowy](./app-service-logic-enterprise-integration-as2.md)

### <a name="connect-to-encode-as2-message-using-the-following-steps"></a>Nawiązywanie połączenia wiadomości AS2 kodowanie, wykonując następujące czynności:

1. [Tworzenie aplikacji logiczny](./app-service-logic-create-a-logic-app.md) zawiera przykład

2. Ten łącznik nie zawiera wszystkie wyzwalacze. Uruchom aplikację logicznych, takich jak wyzwalacz wezwanie na za pomocą innych wyzwalaczy.  W Projektancie aplikacji logika dodać wyzwalacz i Dodaj akcję.  Wybierz pozycję Pokaż Microsoft zarządzany interfejs API programu na liście rozwijanej listy, a następnie wprowadź "AS2" w polu wyszukiwania.  Wybierz pozycję AS2 — kodowanie AS2 wiadomości

    ![Wyszukiwanie AS2](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage1.png)

3. Jeśli nie utworzono jeszcze wszystkie połączenia z kontem integracji, zostanie wyświetlony monit o informacje dotyczące połączenia
    
    ![Tworzenie połączenia z kontem integracji](./media/app-service-logic-enterprise-integration-AS2connector/as2encodeimage1.png)  

4. Wprowadź szczegółowe informacje o koncie integracji.  Właściwości gwiazdką są wymagane

  	| Właściwość   | Szczegóły |
  	| --------   | ------- |
  	| Nazwa połączenia *    | Wpisz dowolną nazwę połączenia |
  	| Integracja z programem konta * | Wprowadź nazwę konta integracji. Upewnij się, że Twoje konto integracji i logiki aplikacji są w tej samej lokalizacji Azure |

    Po zakończeniu szczegóły swojego połączenia wyglądać podobnie do następującej

    ![Integracja z programem połączeniu](./media/app-service-logic-enterprise-integration-AS2connector/as2encodeimage2.png)  

5. Wybierz opcję **Utwórz**

6. Zwróć uwagę, że utworzono połączenie.  Podaj AS2-z AS2-identyfikatorów (jako skonfigurowany Umowa) i szczegóły treści (zawartość wiadomości). 

    ![Podaj wymagane pola](./media/app-service-logic-enterprise-integration-AS2connector/as2encodeimage3.png)

## <a name="the-as2-encode-does-the-following"></a>Kodowanie AS2 wykonuje następujące czynności

* Stosuje nagłówków AS2/HTTP
* Znaki wychodzących wiadomości (jeśli został skonfigurowany)
* Szyfrowanie wiadomości wychodzących (jeśli został skonfigurowany)
* Kompresuje wiadomości (jeśli został skonfigurowany)

##<a name="try-it-for-yourself"></a>Wypróbuj ją dla siebie

Dlaczego nie spróbuj. Kliknij [tutaj](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/) aby wdrażanie aplikacji pełną funkcjonalność logiczny własne korzystania z funkcji logicznych AS2 aplikacji

## <a name="next-steps"></a>Następne kroki

[Dowiedz się więcej o pakiecie integracji przedsiębiorstwa] (./app-service-logic-enterprise-integration-overview.md "Więcej informacji na temat Enterprise Integracja z dodatkiem Service Pack") 

