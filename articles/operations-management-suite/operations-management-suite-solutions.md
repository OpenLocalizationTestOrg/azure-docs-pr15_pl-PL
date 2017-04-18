<properties
   pageTitle="Rozwiązania dla pakietu zarządzania operacje (usługi OMS) | Microsoft Azure"
   description="Rozwiązania rozszerzyć funkcje pakietu zarządzania operacje usługi (OMS), dostarczając scenariusze detaliczny zarządzania klientami można dodać do obszaru roboczego ich usługi OMS.  Ten artykuł zawiera informacje na temat niestandardowych rozwiązań utworzonych przez klientów i partnerów."
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/17/2016"
   ms.author="bwren" />

# <a name="management-solutions-in-operations-management-suite-oms-preview"></a>Rozwiązania do zarządzania w operacji zarządzania pakietu (usługi OMS) (wersja Preview)

>[AZURE.NOTE]Jest to wstępne dokumentacji rozwiązania do zarządzania w usługi OMS, które są obecnie w podglądzie.    

Rozwiązania do zarządzania rozszerzyć funkcje pakietu zarządzania operacje usługi (OMS), dostarczając scenariusze zarządzania detaliczny klientów można dodać do swojego środowiska.  Oprócz [rozwiązania oferowane przez firmę Microsoft](../log-analytics/log-analytics-add-solutions.md)partnerami i klientami można tworzyć rozwiązania do zarządzania może być używany w środowisku własne lub udostępnione klientom za pośrednictwem społeczności.

## <a name="finding-and-installing-management-solutions"></a>Znajdowanie i instalowanie rozwiązania do zarządzania
Istnieje wiele metod do lokalizowania i zainstalowanie rozwiązania do zarządzania, jak to opisano w poniższych sekcjach.

### <a name="azure-marketplace"></a>Azure Marketplace
Rozwiązania do zarządzania dostarczane przez firmę Microsoft i zaufanych partnerów może być zainstalowany z Azure Marketplace w portalu Azure.

1. Zaloguj się do portalu Azure.
2. W okienku po lewej stronie wybierz pozycję **więcej usług**.
3. Przewiń w dół do **rozwiązania** albo wpisz *rozwiązania* w oknie dialogowym **Filtr** .
4. Kliknij przycisk **+ Dodaj** .
5. Wyszukaj rozwiązania, które Cię interesują albo przeglądania, klikając przycisk **Filtr** lub wpisując tekst w polu **Wyszukiwania Everthing** .
6. Kliknij element marketplace, aby wyświetlić szczegółowe informacje.
4. Kliknij przycisk **Utwórz** , aby otworzyć okienko **Dodawanie rozwiązania** .
5. Pojawi się wymagane informacje, takie jak [usługi OMS obszaru roboczego i konto automatyzacji](#oms-workspace-and-automation-account) oprócz wartości parametrów w rozwiązaniu.
6. Kliknij przycisk **Utwórz** , aby zainstalować rozwiązania.

### <a name="oms-portal"></a>Portal usługi OMS
Rozwiązania do zarządzania dostarczane przez firmę Microsoft mogą być zainstalowane z galerii rozwiązań w portalu usługi OMS.

1. Zaloguj się do portalu usługi OMS.
2. Kliknij Kafelek **Galerii rozwiązań** .
2. Na stronie galerii rozwiązań usługi OMS informacje na temat poszczególnych dostępne rozwiązanie. Kliknij nazwę rozwiązanie, które chcesz dodać do usługi OMS.
3. Na stronie wybranego rozwiązania są wyświetlane szczegółowe informacje na temat rozwiązania. Kliknij przycisk **Dodaj**.
4. Nowy Kafelek rozwiązanie, które zostały dodane, pojawi się na przegląd strony w usługi OMS i można rozpoczęcie korzystania z niego po przetworzeniu danych przez usługę.

### <a name="azure-quickstart-templates"></a>Szablony Azure Szybki Start
Członkowie społeczności można przesłać rozwiązania do zarządzania do szablonów Szybki Start Azure.  Możesz pobrać tych szablonów dla instalacji później lub sprawdzanie ich tak, aby dowiedzieć się, jak [tworzyć własne rozwiązań](#creating-a-solution).

1. Postępuj zgodnie z procesu opisanego w [usługi OMS obszaru roboczego i konto automatyzacji](#oms-workspace-and-automation-account) łącze obszaru roboczego i konta.
2. Przejdź do [szablonów Azure Szybki Start](https://azure.microsoft.com/documentation/templates/).  
3. Wyszukaj rozwiązanie, które Cię interesują.
4. Wybierz rozwiązanie z wyników, aby wyświetlić jego szczegóły.
5. Kliknij przycisk **Deploy Azure** .
6. Użytkownik zostanie wyświetlony monit o podanie informacji, na przykład grupa zasobów i położenie oprócz wartości parametrów w rozwiązaniu.
7. Kliknij przycisk **zakupu** , aby zainstalować rozwiązania.

### <a name="deploy-azure-resource-manager-template"></a>Wdrażanie szablonu Azure Menedżera zasobów
Rozwiązania można uzyskać od społeczności lub że [Samodzielne tworzenie](#creating-a-solution) są wykonywane jako szablon Menedżera zasobów i użyć dowolnej z standardowe metody wdrażania [szablonu](../resource-group-template-deploy-portal.md).  Należy zauważyć, że przed zainstalowaniem rozwiązanie, należy utworzyć i łączenie [obszaru roboczego usługi OMS i konto automatyzacji](#oms-workspace-and-automation-account).

## <a name="oms-workspace-and-automation-account"></a>Obszar roboczy usługi OMS i konto automatyzacji
Większość rozwiązania do zarządzania wymagają [usługi OMS obszaru roboczego](../log-analytics/log-analytics-manage-access.md) , zawierają widoki i [konta automatyzacji](../automation/automation-security-overview.md#automation-account-overview) runbooks i zasoby pokrewne. Obszar roboczy i konta musi spełniać następujące wymagania.

- Rozwiązanie można używać tylko jednej usługi OMS obszaru roboczego i jedno konto automatyzacji.  
- Obszar roboczy usługi OMS i konto automatyzacji używane przez rozwiązanie muszą być połączone ze sobą. Obszar roboczy usługi OMS mogą być połączone tylko z jednym kontem automatyzacji, a konto automatyzacji tylko mogą być połączone z jednego obszaru roboczego usługi OMS.
- Połączone, obszar roboczy usługi OMS i automatyzacji konta muszą być w tej samej grupy zasobów i regionu.  Wyjątkiem jest z obszarem roboczym usługi OMS regionu wschodniego Stanów Zjednoczonych i i konto automatyzacji wschodniego 2 w USA.

### <a name="creating-a-link-between-an-oms-workspace-and-automation-account"></a>Tworzenie łącza między obszarem roboczym usługi OMS i konta automatyzacji
Jak określić obszar roboczy usługi OMS i automatyzacji konta zależy od metoda instalacji rozwiązania.

- Po zainstalowaniu rozwiązania Microsoft za pośrednictwem portalu usługi OMS jest zainstalowany w bieżący obszar roboczy usługi OMS i wymagane jest konto nie automatyzacji.

- Po zainstalowaniu rozwiązania w ramach usługi Azure Marketplace, zostanie wyświetlony monit o usługi OMS obszaru roboczego i konto automatyzacji, a łącze między nimi jest tworzona automatycznie.  

- Rozwiązania poza Azure Marketplace należy połączyć obszaru roboczego usługi OMS i konta automatyzacji przed zainstalowaniem rozwiązanie.  Możesz to zrobić, wybierając dowolną rozwiązanie Azure Marketplace i zaznaczenie obszaru roboczego usługi OMS i konta automatyzacji.  Nie masz faktycznie zainstalować rozwiązanie, ponieważ łącze zostanie utworzony po zainicjowaniu usługi OMS obszaru roboczego i konta automatyzacji są zaznaczone.  Po utworzeniu łącze, a następnie za pomocą tego obszaru roboczego usługi OMS i automatyzacji konta dla każdego rozwiązania. 

### <a name="verifying-the-link-between-an-oms-workspace-and-automation-account"></a>Weryfikowanie łącza między usługi OMS obszaru roboczego i konta automatyzacji
Łącza między z obszarem roboczym usługi OMS i konta automatyzacji przy użyciu poniższej procedury, można sprawdzić.

1. Wybierz konto, automatyzacji w portalu Azure.
2. Przewiń do dołu okienka **Ustawienia** .
3. W przypadku sekcji o nazwie **Zasoby usługi OMS** w okienku **Ustawienia** tego konta jest dołączony do obszaru roboczego usługi OMS.
4. Zaznacz **obszar roboczy** , aby wyświetlić szczegóły obszaru roboczego usługi OMS połączony z tym klientem automatyzacji.


## <a name="listing-management-solutions"></a>Wyświetlanie rozwiązania do zarządzania
Za pomocą poniższej procedury do wyświetlanie rozwiązania do zarządzania w obszarach roboczych połączone z subskrypcji usługi Azure.

1. Zaloguj się do portalu Azure.
2. W okienku po lewej stronie wybierz pozycję **więcej usług**.
3. Przewiń w dół do **rozwiązania** albo wpisz *rozwiązania* w oknie dialogowym **Filtr** .
4. Zostaną wyświetlone rozwiązań zainstalowanych w Twoich obszarów roboczych.

Należy zauważyć, że można wyświetlać tylko rozwiązania Microsoft zainstalowanych w bieżący obszar roboczy za pomocą portalu usługi OMS.

## <a name="removing-a-management-solution"></a>Usuwanie rozwiązania do zarządzania
Po usunięciu rozwiązanie do zarządzania usuwane są również wszystkie zasoby w rozwiązaniu.  

1. Znajdź rozwiązanie w portalu Azure przy użyciu procedury w [pozycje rozwiązań](#listing-solutions).
2. Wybierz rozwiązanie, które chcesz usunąć.
3. Kliknij przycisk **Usuń** .

## <a name="creating-a-management-solution"></a>Tworzenie rozwiązania do zarządzania
Ukończono wskazówki na temat tworzenia rozwiązania do zarządzania są dostępne na [Tworzenie rozwiązań usługi pakietu w operacji zarządzania (OMS)](operations-management-suite-solutions-creating.md). 


## <a name="next-steps"></a>Następne kroki

- Wyszukiwanie [Szablonów Szybki Start Azure](https://azure.microsoft.com/documentation/templates) dla próbki różnych szablonów Menedżera zasobów.
- Tworzenie własnego [rozwiązania do zarządzania](operations-management-suite-solutions-creating.md).
 