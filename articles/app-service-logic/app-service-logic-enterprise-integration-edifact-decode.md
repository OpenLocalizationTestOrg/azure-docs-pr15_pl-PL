<properties 
    pageTitle="Więcej informacji na temat Enterprise Integracja z dodatkiem Service Pack dekodowanie łącznik wiadomości EDIFACT | Microsoft Azure aplikacji usługi | Microsoft Azure" 
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

# <a name="get-started-with-decode-edifact-message"></a>Rozpoczynanie pracy z dekodowanie EDIFACT wiadomości

Sprawdza, Edytuj i właściwości specyficzne dla partnerów, generuje dokumentu XML dla każdego zestawu transakcji i generuje potwierdzenia przetworzone transakcji.

## <a name="create-the-connection"></a>Utwórz połączenie

### <a name="prerequisites"></a>Wymagania wstępne

* Konto Azure; można utworzyć [bezpłatne konto](https://azure.microsoft.com/free)

* Aby użyć EDIFACT dekodowanie wiadomości łącznika jest wymagane konto integracji. Szczegółowe informacje na temat tworzenia [Konta integracji](./app-service-logic-enterprise-integration-create-integration-account.md), [partnerami](./app-service-logic-enterprise-integration-partners.md) i [EDIFACT umowy](./app-service-logic-enterprise-integration-edifact.md)

### <a name="connect-to-decode-edifact-message-using-the-following-steps"></a>Nawiązywanie połączenia wiadomości EDIFACT dekodowanie, wykonując następujące czynności:

1. [Tworzenie aplikacji logika](./app-service-logic-create-a-logic-app.md) zawiera przykład.

2. Ten łącznik nie zawiera wszystkie wyzwalacze. Uruchom aplikację logicznych, takich jak wyzwalacz wezwanie na za pomocą innych wyzwalaczy.  W Projektancie aplikacji logika dodać wyzwalacz i Dodaj akcję.  Wybierz pozycję Pokaż Microsoft zarządzany interfejs API programu na liście rozwijanej listy, a następnie wprowadź "EDIFACT" w polu wyszukiwania.  Wybierz pozycję dekodowanie EDIFACT wiadomości

    ![Wyszukiwanie EDIFACT](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage1.png)
    
3. Jeśli nie utworzono jeszcze wszystkie połączenia z kontem integracji, zostanie wyświetlony monit o informacje dotyczące połączenia

    ![Tworzenie konta integracji](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage2.png)  

4. Wprowadź szczegółowe informacje o koncie integracji.  Właściwości gwiazdką są wymagane

  	| Właściwość | Szczegóły |
  	| -------- | ------- |
  	| Nazwa połączenia * | Wpisz dowolną nazwę połączenia |
  	| Integracja z programem konta * | Wprowadź nazwę konta integracji. Upewnij się, że Twoje konto integracji i logiki aplikacji są w tej samej lokalizacji Azure |

    Po zakończeniu szczegóły swojego połączenia wyglądać podobnie do następującej

    ![Integracja z programem konta](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage3.png)  

5. Wybierz opcję **Utwórz**

6. Należy zauważyć, że utworzono połączenie

    ![Szczegóły połączenia konta integracji](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage5.png)  

7. Wybierz wiadomość prosty plik EDIFACT dekodowanie

    ![Podaj wymagane pola](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage5.png)  

## <a name="edifact-decode-does-following"></a>Dekodowanie EDIFACT czy po

* Naprawianie umowę dopasowując kwalifikator nadawcy i identyfikator i kwalifikator odbiorcy i identyfikator
* Dzieli wielu skrzyżowania w jednej wiadomości w oddzielnym.
* Sprawdza koperty przed trading umowy partnera
* Rozkłada wymiany.
* Sprawdza, Edytuj i właściwości specyficzne dla partnerów zawiera
    * Sprawdzanie poprawności struktury koperty wymiany.
    * Sprawdzanie poprawności schematu koperty schematem kontroli.
    * Sprawdzanie poprawności schematu elementów transakcji zestawu danych schematem wiadomości.
    * Edytuj sprawdzanie poprawności wykonywane na elementach transakcji zestawu danych
* Sprawdza, czy numery kontroli zestawu wymiany, grupy i transakcji są nie duplikaty (jeśli został skonfigurowany) 
    * Sprawdza, czy numer formantu wymiany przed wcześniej otrzymane skrzyżowania. 
    * Sprawdza, czy numer sterowania grupy względem innych grupy liczb kontrolki w wymiany. 
    * Sprawdza, czy transakcji Ustawianie formantu względem innych transakcji zestawu sterowania liczb w tej grupie.
* Umożliwia generowanie dokumentu XML dla każdego zestawu transakcji.
* Konwertuje cały wymiany XML 
    * Podziel wymiany jako zestawy transakcji - zawieszenia zestawy transakcji wystąpi błąd: analizuje każdej transakcji w wymiany do innego dokumentu XML. Jeśli co najmniej jeden zestaw transakcji w wymiany nie sprawdzanie poprawności, a następnie dekodowanie EDIFACT zawiesza te zestawy transakcji. 
    * Podziel wymiany jako zestawy transakcji - zawieszenia wymiany wystąpi błąd: analizuje każdej transakcji w wymiany do innego dokumentu XML.  Jeśli co najmniej jeden zestaw transakcji w wymiany nie sprawdzanie poprawności, a następnie dekodowanie EDIFACT zawiesza całego wymiany.
    * Zachowaj wymiany — zawieszenia zestawy transakcji w przypadku błędu: tworzy dokument XML całego wymiany wsadowej. Dekodowanie EDIFACT zawiesza tylko te zestawy transakcji, których nie powiodło się sprawdzanie poprawności, pozostawiając procesu wszystkie inne zestawy transakcji
    * Zachowaj wymiany — zawiesić wymiany wystąpi błąd: tworzy dokument XML całego wymiany wsadowej. Jeśli co najmniej jeden zestaw transakcji w wymiany nie sprawdzanie poprawności, a następnie dekodowanie EDIFACT zawiesza całego wymiany 
* Umożliwia generowanie techniczne (formant) i/lub funkcjonalności potwierdzenia (jeśli został skonfigurowany).
    * Potwierdzenia technicznych lub CONTRL ACK informacjami dotyczącymi syntaktycznych wyboru ukończone wymiany odebrane.
    * Potwierdzanie funkcjonalności uznaje zaakceptować lub odrzucić otrzymanej wymiany lub grupy

## <a name="next-steps"></a>Następne kroki

[Dowiedz się więcej o pakiecie integracji przedsiębiorstwa] (./app-service-logic-enterprise-integration-overview.md "Więcej informacji na temat Enterprise Integracja z dodatkiem Service Pack") 