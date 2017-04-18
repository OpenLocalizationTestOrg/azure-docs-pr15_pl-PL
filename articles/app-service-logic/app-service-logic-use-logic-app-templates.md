<properties
 pageTitle="Szablony aplikacji logika | Microsoft Azure"
 description="Dowiedz się, jak używać wstępnie zaprojektowanych szablonów aplikacji logika ułatwiające rozpoczęcie pracy"
 authors="kevinlam1"
 manager="dwrede"
 editor=""
 services="app-service\logic"
 documentationCenter=""/>

<tags
    ms.service="app-service-logic"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="klam"/>

# <a name="logic-app-templates"></a>Szablony aplikacji warunków logicznych

## <a name="what-are-logic-app-templates"></a>Co to są szablony aplikacji logika

Szablon aplikacji logika jest aplikacji gotowych logiczny, którą można szybko rozpocząć tworzenie własnego przepływu pracy. 

Te szablony są dobrym sposobem identyfikować wzorce różnych, które mogą być wbudowane korzystanie z aplikacji logicznych. Możesz użyć tych szablonów jako- lub zmodyfikować ich tak, aby dopasować rozwiązania.

## <a name="overview-of-available-templates"></a>Omówienie dostępne szablony

Istnieje wiele dostępnych szablonów obecnie opublikowany w logiczny platformy aplikacji. Poniżej przedstawiono niektóre kategorie przykład, a także rodzaj łączników używane w nich.

### <a name="enterprise-cloud-templates"></a>Szablony przedsiębiorstw chmury
Szablony integrujących Dynamics CRM, usług Salesforce pole, obiektów Blob platformy Azure i innych łączników do potrzeb organizacji chmury. Oto kilka przykładów co należy zrobić w przypadku szablony zawierają organizowanie potencjalnych klientów i tworzenie kopii zapasowych danych firmowych pliku.

### <a name="enterprise-integration-pack-templates"></a>Szablony pack integracji przedsiębiorstwa
Konfiguracje VETER (Sprawdzanie poprawności, wyodrębnianie, przekształcanie, wzbogacanie, rozsyłanie) procesy otrzymywanie X12 Edytuj dokument na AS2 i przekształcania do formatu XML, jak również X12 i AS2 wiadomości obsługi.

### <a name="protocol-pattern-templates"></a>Szablony deseniu Protocol (protokół)
Te szablony składają się z aplikacji logiczny, które zawierają desenie protokół takich jak żądanie odpowiedź na HTTP, a także funkcje integracji przez FTP i SFTP. Skorzystaj z poniższych jakie istnieją lub jako podstawy do tworzenia bardziej złożonych wzorców Protocol (protokół).  

### <a name="personal-productivity-templates"></a>Szablony produktywność
Desenie, aby zwiększyć produktywność zawiera szablony, które Ustawianie przypomnień dzienny, ważnych elementach pracy są przekształcane w listy zadań do wykonania i zautomatyzować długich zadań w dół do kroku zatwierdzania jednego użytkownika.

### <a name="consumer-cloud-templates"></a>Dla klientów indywidualnych chmury szablonów
Szablony prosta Integracja z usługami społecznościowych, takich jak Twitter, zapas czasu i poczty e-mail, ostatecznie do wzmocnienia społecznościowych marketingowych inicjatywy; Te także szablony takie jak kopiowanie mętna umożliwiające zwiększenie wydajności, zapisując czasu poświęconego na zazwyczaj często wykonywanych zadań. 

## <a name="how-to-create-a-logic-app-using-a-template"></a>Jak utworzyć aplikację logiczny, przy użyciu szablonu 

Aby rozpocząć pracę przy użyciu szablonu aplikacji logiczny, przejdź do projektanta logiki aplikacji. Jeśli wprowadzane przez projektanta, otwierając istniejącej aplikacji logiczny, aplikacja logiczny automatycznie ładuje w widoku projektanta. Jednak jeśli tworzysz nowej aplikacji logika pojawi się ekran poniżej.  
 ![](../../includes/media/app-service-logic-templates/template7.png)  

Z tego ekranu albo możesz zacząć za pomocą aplikacji logika pustych lub gotowych szablonów. Jeśli wybierzesz jeden z szablonów, są dostarczane z dodatkowymi informacjami. W tym przykładzie firma Microsoft korzysta z szablonu *podczas tworzenia nowego pliku w usłudze Dropbox, skopiuj go do usługi OneDrive* .  
 ![](../../includes/media/app-service-logic-templates/template2.png)  

Jeśli użytkownik chce korzystać z szablonu, po prostu kliknij przycisk *za pomocą tego szablonu* . Zostanie wyświetlony monit do zalogowania się do konta oparte na które łączniki wykorzystuje szablon. Lub, jeśli wcześniej nawiązaniu połączenia z tych łączników, możesz wybrać kontynuowanie, jak pokazano poniżej.  
 ![](../../includes/media/app-service-logic-templates/template3.png)  

Po nawiązywania połączenia i wybraniu, *Kontynuuj*, aplikacja logiczny zostanie otwarta w widoku projektanta.  
 ![](../../includes/media/app-service-logic-templates/template4.png)  

W powyższym przykładzie tak jak w przypadku z wielu szablonów, niektóre pola wymagane właściwości mogą można wypełniać w łączników; Jednak niektóre mogą nadal wymagać wartości zanim będzie mógł prawidłowo wdrażanie aplikacji logicznych. Przy próbie wdrażanie bez wprowadzania część brakuje pól, zostanie wyświetlone powiadomienie z komunikatem o błędzie.

Jeśli chcesz powrócić do podglądu szablonu, kliknij przycisk *Szablony* na górnym pasku nawigacyjnym. Przełączanie z powrotem do podglądu szablonu, stracisz niezapisane postępu. Przed przełączania do podglądu szablonu, pojawi się komunikat z ostrzeżeniem powiadomienia o tym.  
 ![](../../includes/media/app-service-logic-templates/template5.png)  

## <a name="how-to-deploy-a-logic-app-created-from-a-template"></a>Jak wdrożyć aplikację logiczny, utworzone na podstawie szablonu

Po załadowano szablon i wszelkie zmiany wprowadzone wybierz pozycję Zapisz przycisk w lewym górnym rogu. To jest zapisywany i publikuje aplikacji logicznych.  
 ![](../../includes/media/app-service-logic-templates/template6.png)  

Aby uzyskać więcej informacji na jak możesz dodać kolejne kroki do istniejącego szablonu aplikacji logika lub utworzyć edytuje ogólnie, Dowiedz się więcej na [Tworzenie aplikacji logicznych](app-service-logic-create-a-logic-app.md).