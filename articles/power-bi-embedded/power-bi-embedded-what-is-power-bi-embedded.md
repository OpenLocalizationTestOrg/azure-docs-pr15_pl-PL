<properties
   pageTitle="Co to jest Microsoft Power BI osadzonego?"
   description="Power BI osadzony umożliwia zintegrowanie raportach usługi Power BI w witrynie sieci web lub aplikacji dla urządzeń przenośnych, więc nie trzeba tworzyć niestandardowe rozwiązania wizualizowanie danych dla użytkowników"
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

# <a name="what-is-microsoft-power-bi-embedded"></a>Co to jest Microsoft Power BI osadzonego?

Z **Power BI osadzony**można zintegrować raportach usługi Power BI bezpośrednio w witrynie sieci web lub aplikacji dla urządzeń przenośnych.

![](media\powerbi-embedded-whats-is\what-is.png)

Power BI osadzony to **Usługa Azure** , która umożliwia niezależnych dostawców oprogramowania i deweloperów aplikacji powierzchniowy środowiska danych usługi Power BI w swoich aplikacjach. Projektant zostały wbudowane aplikacje, a te aplikacje mieć własnych użytkowników i odrębnych zestaw funkcji. Te aplikacje również może się zdarzyć, aby niektóre elementy wbudowane danych, takie jak wykresy i raporty, które mogą być teraz korzystająca z Microsoft Power BI osadzony. Użytkownicy nie będą konto usługi Power BI do korzystania z aplikacji. Pozostają można zalogować się do aplikacji, tak jak poprzednio i przeglądać i pracować z działanie funkcji raportowania bez konieczności dodatkowych licencji usługi Power BI.

## <a name="licensing-for-microsoft-power-bi-embedded"></a>Licencjonowanie Microsoft Power BI osadzony

**Microsoft Power BI osadzonego** modelu zastosowania Licencjonowanie usługi Power BI jest nie odpowiedzialność użytkownika końcowego.  Zamiast tego **są renderowane za pomocą** zakupionych przez dewelopera aplikacji, którą jest przez inne efekty wizualne i naliczania subskrypcję, do której należą zasoby.

## <a name="microsoft-power-bi-embedded-conceptual-model"></a>Microsoft Power BI osadzony Model koncepcyjny

![](media\powerbi-embedded-whats-is\model.png)

Podobnie jak inne usługi w Azure zasoby dla Power BI osadzony zainicjowano obsługę administracyjną za pośrednictwem [Interfejsów API ARM Azure](https://msdn.microsoft.com/library/mt712306.aspx). W tym przypadku zasób, którego możesz dodawać to **Kolekcja obszaru roboczego usługi Power BI**.

## <a name="workspace-collection"></a>Kolekcja obszaru roboczego

**Obszar roboczy zbioru** jest najwyższego poziomu Azure kontener dla zasobów zawierający 0 lub więcej **obszarów roboczych**.  **Obszar roboczy** **zbioru** zawiera wszystkie właściwości standardowe Azure, a także następujące czynności:

-   **Klawisze dostępu** — klawiszy są używane podczas bezpieczne połączenia interfejsów API Power BI (opisane w dalszej części).
-   **Użytkownicy** — Azure Active Directory (AAD) użytkowników, którzy mają uprawnienia administratora, aby zarządzać zbioru Power BI obszaru roboczego za pośrednictwem portalu Azure lub interfejsu API ARM.
-   **Region** — jako część inicjowania obsługi administracyjnej **Zbioru obszaru roboczego**, można wybrać ma zostać zastrzeżona w regionie. Aby uzyskać więcej informacji zobacz [Obszary Azure](https://azure.microsoft.com/regions/).

## <a name="workspace"></a>Obszar roboczy

**Obszar roboczy** jest kontenerem usługi Power BI zawartości, która może zawierać zestawy danych, raporty i pulpity nawigacyjne. **Obszar roboczy** jest puste, gdy utworzenia. W wersji Preview będzie Autor całej zawartości przy użyciu Power BI Desktop i będzie przekazać go do jednego z obszarów roboczych przy użyciu [Power BI pozostałych API](http://docs.powerbi.apiary.io/reference).

## <a name="using-workspace-collections-and-workspaces"></a>Za pomocą kolekcji obszaru roboczego i obszarów roboczych
**Kolekcje obszaru roboczego** i **obszary robocze** są kontenerami zawartości, które są używane i zorganizowane w bez względu na sposób najlepiej pasuje do Twojej projekt aplikacji do tworzonego. Będzie wiele różnych sposobów, że można rozmieścić ich zawartości. Można umieścić całą zawartość w ramach jednego obszaru roboczego, a następnie później użyć tokeny aplikacji do dalszego podziału zawartości między klientów. Można także umieścić wszystkich klientów w osobnych obszarach roboczych, aby była ona niektóre odstęp między nimi. Lub można uporządkować użytkowników według regionów, a nie przez klienta. Ten projekt elastyczne pozwala wybrać najlepszym sposobem organizowania zawartości.

## <a name="cached-datasets"></a>Tryb buforowanej zestawy danych

Tryb buforowanej zestawy danych można używać w podglądzie.  Jednak nie można odświeżyć buforowane po załadowaniu do **Programu Microsoft Power BI osadzony**.

## <a name="authentication-and-authorization-with-app-tokens"></a>Uwierzytelnianie i autoryzacji tokeny aplikacji

**Microsoft Power BI osadzony** różni się do aplikacji do wykonania wszystkich uwierzytelniania potrzeby użytkowników i autoryzacji. Nie jest jawnie wymagane użytkowników końcowych konieczności klientów usługi Azure Active Directory (Azure AD).  Zamiast tego aplikacja wyraża się do **Programu Microsoft Power BI osadzony** autoryzacji do renderowania raportu usługi Power BI przy użyciu **Tokenów uwierzytelniania aplikacji (tokeny aplikacji)**.  Te **Tokeny aplikacji** są tworzone, stosownie do potrzeb, gdy chce aplikacji do renderowania raportu.  Zobacz [tokeny aplikacji](power-bi-embedded-get-started-sample.md#key-flow).

![](media\powerbi-embedded-whats-is\app-tokens.png)

**Tokeny uwierzytelniania aplikacji (aplikacja tokeny)** służą do uwierzytelniania **Microsoft Power BI osadzony**.  Istnieją trzy typy tokenów **Aplikacji**:

1.  Obsługa administracyjna tokeny - używane podczas inicjowania obsługi administracyjnej nowego **obszaru roboczego** do **Pobierania obszaru roboczego**
2.  Tokeny rozwoju - używane podczas nawiązywania połączenia bezpośrednio do **Power BI pozostałych API**
3.  Osadzanie tokeny - używane podczas nawiązywania połączenia do renderowania raportu w osadzonego elementu iframe

Tokeny te są używane dla różnych faz kontaktów z **Programu Microsoft Power BI osadzony**.  Tokeny są zaprojektowane tak, aby możesz delegować uprawnienia z Twojej aplikacji usługi Power BI. Aby uzyskać więcej informacji zobacz [Przepływ Token aplikacji](power-bi-embedded-app-token-flow.md).

## <a name="see-also"></a>Zobacz też
- [Typowe scenariusze Microsoft Power BI osadzony](power-bi-embedded-scenarios.md)
- [Wprowadzenie do programu Microsoft Power BI osadzony](power-bi-embedded-get-started.md)
