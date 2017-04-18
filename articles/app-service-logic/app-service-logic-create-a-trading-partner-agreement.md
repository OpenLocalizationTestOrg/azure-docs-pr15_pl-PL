<properties 
   pageTitle="Tworzenie handlowych Umowy partnerów w usłudze Azure aplikacji | Microsoft Azure" 
   description="Tworzenie Trading umów partnera" 
   services="logic-apps" 
   documentationCenter=".net,nodejs,java" 
   authors="rajram" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
    ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="08/23/2016"
   ms.author="rajram"/>

# <a name="creating-a-trading-partner-agreement"></a>Tworzenie umowę partnerów handlowych   

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

Partnerami handlowymi są jednostek biorących udział w B2B komunikacji (Business — firma). Gdy partnerów ustanowienia relacji, to nosi nazwę *Umowy*. Umowę zdefiniowane jest oparty na komunikacji dwóch partnerów chęć osiągnięcia i protokół lub transportu określone. Różne protokoły B2B i transportu obsługiwanych przez usługę aplikacji Azure obejmują:

- AS2 (stosowanie instrukcja 2)
- EDIFACT (ONZ/elektronicznej wymiany danych dla administracji, handlu i transportu (NZ/EDIFACT))
- X12 (ASC X12)

### <a name="biztalk-api-apps-that-support-b2b-scenarios"></a>Aplikacje interfejsu API BizTalk, który obsługuje B2B scenariuszy
Następujące aplikacje interfejsu API włączyć te funkcje przy użyciu sformatowanego i intuicyjnej doświadczenia w Azure Portal:


## <a name="biztalk-trading-partner-management-tpm"></a>BizTalk Trading zarządzania partnera (TPM)
- Tworzenie i zarządzanie partnerów, profile i tożsamości
- Przechowywanie i zarządzanie schematów grupy użytkowników
- Przechowywanie i zarządzanie certyfikatów (używane w protokole AS2)
- Tworzenie i zarządzanie umów AS2
- Tworzenie i zarządzanie umów EDIFACT (obejmuje tworzeniu partii na stronie wysyłanie)
- Tworzenie i zarządzanie X12 umów (obejmuje tworzeniu partii na stronie wysyłanie)

![][1]


## <a name="as2-connector"></a>Łącznik AS2
- Wykonuje AS2 umów, zgodnie z definicją w powiązanych wystąpieniu TPM interfejsu API aplikacji
- Pobierający AS2 przetwarzania i śledzenie informacji dotyczących rozwiązywania problemów


## <a name="biztalk-edifact"></a>BizTalk EDIFACT
- Wykonuje EDIFACT umów, zgodnie z definicją w powiązanych wystąpieniu TPM interfejsu API aplikacji
- Pobierający EDIFACT przetwarzania i śledzenie informacji dotyczących rozwiązywania problemów
- Umożliwia zarządzanie stan partii (uruchomieniu i zatrzymaniu), zgodnie z postanowieniami Umowy EDIFACT w powiązanych wystąpieniu TPM interfejsu API aplikacji


## <a name="biztalk-x12"></a>BizTalk X12
- Wykonuje X12 umów zgodnie z definicją w powiązanych wystąpieniu TPM interfejsu API aplikacji 
- Wyświetla X12 przetwarzania i śledzenie informacji dotyczących rozwiązywania problemów
- Umożliwia zarządzanie stan partii (uruchomieniu i zatrzymaniu), zgodnie z definicją w X12 umowy w powiązanych wystąpieniu TPM interfejsu API aplikacji

Podaną wcześniej AS2, X 12 i aplikacje interfejsu API EDIFACT wymagają aplikację interfejsu API TPM, aby funkcja zgodnie z oczekiwaniami.


## <a name="getting-started"></a>Wprowadzenie
Aby utworzyć handlowych umów partnera:

1. Tworzenie wystąpienia łącznika **Zarządzania Trading partnera BizTalk** . Wymaga to pusta baza danych SQL do funkcji. Przed uruchamianie Pamiętaj mieć pusta baza danych, dostępne i gotowe do użycia.
2. Przekaż schematów i certyfikatów zgodnie z wymaganiami umów. W tym celu przeglądania wystąpienie TPM utworzone i przechodzenie do części "Schematy" lub "Certyfikaty"
3. Przejdź do wystąpienia TPM utworzone i krok do części **partnerów**
4. Tworzenie partnerów stosownie do potrzeb. Również edytować profile odpowiednio i Dodaj wymagane tożsamości
5. Teraz Utwórz umowy za pomocą part **umów** . Po utworzeniu umowę, należy wybrać protokół, który będzie używany. Pozostałe opcje konfiguracji zależą od wybranego protokołu.

![][2]

![][3]

<!--Image references-->
[1]: ./media/app-service-logic-create-a-trading-partner-agreement/TPMResourceView.png
[2]: ./media/app-service-logic-create-a-trading-partner-agreement/ProtocolSelection.png
[3]: ./media/app-service-logic-create-a-trading-partner-agreement/X12AgreementCreation.png
 
