<properties 
    pageTitle="Więcej informacji na temat Enterprise Integracja z dodatkiem Service Pack dekodowanie X12 wiadomości Connctor | Microsoft Azure aplikacji usługi | Microsoft Azure" 
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

# <a name="get-started-with-decode-x12-message"></a>Wprowadzenie do dekodowania X12 wiadomości

Sprawdza, Edytuj i właściwości specyficzne dla partnerów, generuje dokumentu XML dla każdego zestawu transakcji i generuje potwierdzenia przetworzone transakcji.

## <a name="create-the-connection"></a>Utwórz połączenie

### <a name="prerequisites"></a>Wymagania wstępne

* Konto Azure; można utworzyć [bezpłatne konto](https://azure.microsoft.com/free)

* Aby użyć dekodowania X12 wiadomości łącznika jest wymagane konto integracji. Szczegółowe informacje na temat tworzenia [Konta integracji](./app-service-logic-enterprise-integration-create-integration-account.md), [partnerami](./app-service-logic-enterprise-integration-partners.md) i [X12 umowy](./app-service-logic-enterprise-integration-x12.md)

### <a name="connect-to-decode-x12-message-using-the-following-steps"></a>Nawiązywanie połączenia dekodowania X12 wiadomości, wykonaj następujące czynności:

1. [Tworzenie aplikacji logiczny](./app-service-logic-create-a-logic-app.md) zawiera przykład

2. Ten łącznik nie zawiera wszystkie wyzwalacze. Uruchom aplikację logicznych, takich jak wyzwalacz wezwanie na za pomocą innych wyzwalaczy.  W Projektancie aplikacji logika dodać wyzwalacz i Dodaj akcję.  Wybierz pozycję Pokaż Microsoft zarządzany interfejs API programu na liście rozwijanej listy, a następnie wprowadź "x12" w polu wyszukiwania.  Wybierz pozycję X12 — dekodowanie X12 wiadomości

    ![Wyszukiwanie x12](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage1.png)  

3. Jeśli nie utworzono jeszcze wszystkie połączenia z kontem integracji, zostanie wyświetlony monit o informacje dotyczące połączenia

    ![Integracja połączenia konta](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage4.png)    

4. Wprowadź szczegółowe informacje o koncie integracji.  Właściwości gwiazdką są wymagane

  	| Właściwość | Szczegóły |
  	| -------- | ------- |
  	| Nazwa połączenia * | Wpisz dowolną nazwę połączenia |
  	| Integracja z programem konta * | Wprowadź nazwę konta integracji. Upewnij się, że Twoje konto integracji i logiki aplikacji są w tej samej lokalizacji Azure |

    Po zakończeniu szczegóły swojego połączenia wyglądać podobnie do następującej
    
    ![Integracja połączenia konta utworzonego](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage5.png) 

5. Wybierz opcję **Utwórz**
    
6. Zwróć uwagę, że utworzono połączenie.

    ![Szczegóły połączenia konta integracji](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage6.png) 

7. Wybierz pozycję X12 płaska komunikat pliku dekodowanie

    ![Podaj wymagane pola](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage7.png) 

## <a name="x12-decode-does-following"></a>Dekodowanie X12 ma po

* Sprawdza koperty przed trading umowy partnera
* Umożliwia generowanie dokumentu XML dla każdego zestawu transakcji.
* Sprawdza, Edytuj i właściwości specyficzne dla partnerów
    * Edytuj strukturalnego sprawdzania poprawności i sprawdzania poprawności schematów rozszerzonego
    * Sprawdzanie poprawności struktury koperty wymiany.
    * Sprawdzanie poprawności schematu koperty schematem kontroli.
    * Sprawdzanie poprawności schematu elementów transakcji zestawu danych schematem wiadomości.
    * Edytuj sprawdzanie poprawności wykonywane na elementach transakcji zestawu danych 
* Sprawdza, czy numery kontroli zestawu wymiany, grupy i transakcji nie są duplikaty
    * Sprawdza, czy numer formantu wymiany przed wcześniej otrzymane skrzyżowania.
    * Sprawdza, czy numer sterowania grupy względem innych grupy liczb kontrolki w wymiany.
    * Sprawdza, czy transakcji Ustawianie formantu względem innych transakcji zestawu sterowania liczb w tej grupie.
* Konwertuje cały wymiany XML 
    * Podziel wymiany jako zestawy transakcji - zawieszenia zestawy transakcji wystąpi błąd: analizuje każdej transakcji w wymiany do innego dokumentu XML. Jeśli co najmniej jednej transakcji zestawów w wymiany niepowodzenie sprawdzania poprawności, X12 dekodowania zawiesza te zestawy transakcji.
    * Podziel wymiany jako zestawy transakcji - zawieszenia wymiany wystąpi błąd: analizuje każdej transakcji w wymiany do innego dokumentu XML.  Jeśli co najmniej jednej transakcji zestawów w wymiany niepowodzenie sprawdzania poprawności, X12 dekodowania zawiesza całego wymiany.
    * Zachowaj wymiany — zawieszenia zestawy transakcji w przypadku błędu: tworzy dokument XML całego wymiany wsadowej. X12 dekodowania zawiesza tylko te zestawy transakcji, których nie powiodło się sprawdzanie poprawności, pozostawiając przetwarzania inne transakcje zestawów
    * Zachowaj wymiany — zawiesić wymiany wystąpi błąd: tworzy dokument XML całego wymiany wsadowej. Jeśli co najmniej jednej transakcji zestawów w wymiany niepowodzenie sprawdzania poprawności, X12 dekodowania zawiesza całego wymiany 
* Umożliwia generowanie potwierdzenia techniczne i/lub funkcjonalne (jeśli został skonfigurowany).
    * Potwierdzanie techniczne generuje wyniku sprawdzania poprawności nagłówek. Potwierdzanie techniczne informuje o stanie przetwarzania nagłówka wymiany i przyczepy przez odbiorcę adres.
    * Potwierdzanie funkcjonalności generuje wyniku sprawdzania poprawności treści. Potwierdzanie funkcjonalności raportów każdego wystąpił błąd podczas przetwarzania otrzymany dokument

## <a name="next-steps"></a>Następne kroki

[Dowiedz się więcej o pakiecie integracji przedsiębiorstwa] (./app-service-logic-enterprise-integration-overview.md "Więcej informacji na temat Enterprise Integracja z dodatkiem Service Pack") 


