<properties 
    pageTitle="Więcej informacji na temat Enterprise Integracja z dodatkiem Service Pack kodowanie X12 wiadomości Connctor | Microsoft Azure aplikacji usługi | Microsoft Azure" 
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

# <a name="get-started-with-encode-x12-message"></a>Rozpoczynanie pracy z kodowanie X12 wiadomości

Sprawdza, Edytuj i właściwości specyficzne dla partnerów, konwertuje wiadomości w formacie XML Edytuj zestawów transakcji w wymiany i żądania potwierdzenia techniczne i/lub funkcjonalne

## <a name="create-the-connection"></a>Utwórz połączenie

### <a name="prerequisites"></a>Wymagania wstępne

* Konto Azure; można utworzyć [bezpłatne konto](https://azure.microsoft.com/free)

* Aby użyć kodowanie x12 wiadomości łącznika jest wymagane konto integracji. Szczegółowe informacje na temat tworzenia [Konta integracji](./app-service-logic-enterprise-integration-create-integration-account.md), [partnerami](./app-service-logic-enterprise-integration-partners.md) i [X12 umowy](./app-service-logic-enterprise-integration-x12.md)

### <a name="connect-to-encode-x12-message-using-the-following-steps"></a>Nawiązywanie połączenia kodowanie X12 wiadomości, wykonaj następujące czynności:

1. [Tworzenie aplikacji logiczny](./app-service-logic-create-a-logic-app.md) zawiera przykład

2. Ten łącznik nie zawiera wszystkie wyzwalacze. Uruchom aplikację logicznych, takich jak wyzwalacz wezwanie na za pomocą innych wyzwalaczy.  W Projektancie aplikacji logika dodać wyzwalacz i Dodaj akcję.  Wybierz pozycję Pokaż Microsoft zarządzany interfejs API programu na liście rozwijanej listy, a następnie wprowadź "x12" w polu wyszukiwania.  Wybierz pozycję X12-kodowanie X12 wiadomości według nazwy umowy lub X12-kodowanie X 12 wiadomość przez tożsamości.  

    ![Wyszukiwanie x12](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage1.png) 

3. Jeśli nie utworzono jeszcze wszystkie połączenia z kontem integracji, zostanie wyświetlony monit o informacje dotyczące połączenia

    ![Integracja połączenia konta](./media/app-service-logic-enterprise-integration-x12connector/x12encodeimage1.png) 


4. Wprowadź szczegółowe informacje o koncie integracji.  Właściwości gwiazdką są wymagane

  	| Właściwość | Szczegóły |
  	| -------- | ------- |
  	| Nazwa połączenia * | Wpisz dowolną nazwę połączenia |
  	| Integracja z programem konta * | Wprowadź nazwę konta integracji. Upewnij się, że Twoje konto integracji i logiki aplikacji są w tej samej lokalizacji Azure |

    Po zakończeniu szczegóły swojego połączenia wyglądać podobnie do następującej

    ![Integracja połączenia konta utworzonego](./media/app-service-logic-enterprise-integration-x12connector/x12encodeimage2.png) 


5. Wybierz opcję **Utwórz**

6. Zwróć uwagę, że utworzono połączenie.

    ![Szczegóły połączenia konta integracji](./media/app-service-logic-enterprise-integration-x12connector/x12encodeimage3.png) 

#### <a name="x12---encode-x12-message-by-agreement-name"></a>X12-kodowanie X12 wiadomości według nazwy umowy

7. Wybierz pozycję X12 umowy z listy rozwijanej i język xml wiadomości do kodowania.

    ![Podaj wymagane pola](./media/app-service-logic-enterprise-integration-x12connector/x12encodeimage4.png) 

#### <a name="x12---encode-x12-message-by-identities"></a>X12-kodowanie X12 wiadomości, tożsamości

7.  Podaj identyfikator nadawcy, Kwalifikator nadawcy, identyfikator odbiorcy i kwalifikator odbiorcy zgodnie z konfiguracją w X12 umowy.  Wybierz wiadomość xml do kodowania

    ![Podaj wymagane pola](./media/app-service-logic-enterprise-integration-x12connector/x12encodeimage5.png) 

## <a name="x12-encode-does-following"></a>Kodowanie X12 ma po:

* Umowa dotycząca rozdzielczość dopasowując właściwości kontekstu nadawcy i adresata.
* Serializes wymiany grupy użytkowników, konwertowania Edytuj zestawów transakcji w wymiany wiadomości w formacie XML.
* Stosuje segmentów nagłówek i przyczepy zestaw transakcji
* Umożliwia generowanie wymiany kontrolki, numer sterowania grupy oraz transakcji zestawu kontrolki numeru dla każdej wymiany poczty wychodzącej
* Zastępuje separatory w danych ładunku
* Sprawdza, Edytuj i właściwości specyficzne dla partnerów
    * Sprawdzanie poprawności schematu elementów transakcji zestawu danych dla komunikatu schematu
    * Sprawdzanie poprawności Edytuj wykonywane na elementach transakcji zestawu danych.
    * Rozszerzonego sprawdzanie poprawności wykonywane na elementach transakcji zestawu danych
* Żądania potwierdzenia techniczne i/lub funkcjonalne (jeśli został skonfigurowany).
    * Potwierdzanie techniczne generuje wyniku sprawdzania poprawności nagłówek. Potwierdzanie techniczne informuje o stanie przetwarzania nagłówek wymiany i przyczepy przez odbiorcę adresu
    * Potwierdzanie funkcjonalności generuje wyniku sprawdzania poprawności treści. Potwierdzanie funkcjonalności raportów każdego wystąpił błąd podczas przetwarzania otrzymany dokument

## <a name="next-steps"></a>Następne kroki

[Dowiedz się więcej o pakiecie integracji przedsiębiorstwa] (./app-service-logic-enterprise-integration-overview.md "Więcej informacji na temat Enterprise Integracja z dodatkiem Service Pack") 

