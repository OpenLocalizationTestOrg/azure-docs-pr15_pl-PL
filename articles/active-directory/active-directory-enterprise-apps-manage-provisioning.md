<properties
    pageTitle="Użytkownik inicjowania obsługi administracyjnej zarządzania aplikacji przedsiębiorstwa w podglądzie usługi Azure Active Directory | Microsoft Azure"
    description="Dowiedz się, jak zarządzać Przypisywanie konta użytkowników dla aplikacji przedsiębiorstwa przy użyciu podglądu usługi Azure Active Directory"
    services="active-directory"
    documentationCenter=""
    authors="asmalser"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/12/2016"
    ms.author="asmalser"/>

#<a name="preview-managing-user-account-provisioning-for-enterprise-apps-in-the-new-azure-portal"></a>Podgląd: Zarządzanie inicjowania obsługi administracyjnej aplikacji przedsiębiorstwa w portalu Azure nowego konta użytkownika

W tym artykule opisano, jak za pomocą [Azure portal](https://portal.azure.com) Zarządzanie kontem użytkownika automatycznego inicjowania obsługi administracyjnej i Anuluj inicjowania obsługi administracyjnej aplikacji, które obsługują, szczególnie te, które zostały dodane w kategorii "polecane" [galerii aplikacji usługi Azure Active Directory](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery). Tego środowiska zarządzania w nowy portal Azure jest obecnie w podglądzie publiczne, a w tym artykule opisano nowe funkcje, a także kilka czasowe ograniczenia, które znajdują się w miejscu w okresie Podgląd. [Co to jest w podglądzie?](active-directory-preview-explainer.md)

Aby dowiedzieć się więcej na temat obsługi administracyjnej konta użytkownika automatyczne i jak to działa, zobacz [Przypisywanie użytkowników zautomatyzować i Deprovisioning do władz akredytacji bezpieczeństwa aplikacji z usługą Azure Active Directory](active-directory-saas-app-provisioning.md).

##<a name="finding-your-apps-in-the-new-portal"></a>Znajdowanie aplikacji w nowego portalu

Od września 2016 wszystkie aplikacje, które zostały skonfigurowane dla pojedynczego logowania jednokrotnego w katalogu, administrator katalogu przy użyciu [galerii aplikacji usługi Azure Active Directory](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery) wewnątrz [portal Azure klasycznego](https://manage.windowsazure.com), może teraz można wyświetlać i zarządzania nimi nowy portal Azure.

Te aplikacje można znaleźć w sekcji **Aplikacje dla przedsiębiorstw** nowy portal Azure są dostępne za pośrednictwem menu **Więcej usług** w obszarze nawigacji po lewej stronie. Aplikacji przedsiębiorstwa są aplikacje, które zostały wdrożone i są używane przez użytkowników w organizacji.

![Karta aplikacje dla przedsiębiorstw][0]

Kliknąć łącze **wszystkie aplikacje** po lewej stronie zawiera listę wszystkich aplikacji, które zostały skonfigurowane, łącznie z aplikacjami, które dodano z galerii. Wybieranie aplikacji ładuje karta zasobów dla aplikacji, której raporty mogą być wyświetlane dla tej aplikacji i mogą być zarządzane różne ustawienia.

Ustawienia inicjowania obsługi administracyjnej konta użytkownika można zarządzać, wybierając **obsługi** po lewej stronie.

![Karta zasobu aplikacji][1]


##<a name="provisioning-modes"></a>Tryby obsługi administracyjnej

Karta **obsługi** zaczyna się od menu **Tryb** , który pokazuje, jakie obsługi administracyjnej tryby są obsługiwane w przypadku aplikacji dla przedsiębiorstw i można je skonfigurować. Opcje dostępne są następujące:

* **Automatyczne** — ta opcja jest wyświetlany Azure AD obsługuje automatycznego opartych na interfejsu API inicjowania obsługi administracyjnej i/lub usuwania inicjowania obsługi administracyjnej kont użytkowników do tej aplikacji. Wybranie tego trybu wyświetla interfejs, który prowadzi administratorzy za pośrednictwem konfigurowania Azure AD nawiązywania połączenia z interfejsu API menedżera użytkownika aplikacji, tworzenie mapowania konta i przepływy pracy, które określają, jak dane konto użytkownika należy przepływ między Azure AD i aplikacji, a zarządzanie Azure AD inicjowania obsługi administracyjnej usługi.

* **Ręczne** — ta opcja jest wyświetlany, jeśli Azure AD nie obsługuje automatycznego inicjowania obsługi administracyjnej kont użytkowników do tej aplikacji. Ta opcja oznacza, że rekordy konta użytkowników przechowywanych w aplikacji muszą być zarządzane za pomocą proces zewnętrzny, oparte na możliwościach zarządzania i inicjowania obsługi administracyjnej użytkownika podany przez aplikację (który może obejmować SAML Just-In-Time inicjowania obsługi administracyjnej).


##<a name="configuring-automatic-user-account-provisioning"></a>Konfigurowanie konta użytkownika automatycznego inicjowania obsługi administracyjnej

Opcja **Automatyczne** zostanie wyświetlony ekran, który dzieli się na cztery sekcje:

###<a name="admin-credentials"></a>Poświadczenia administratora
Jest to, gdzie wymagane poświadczenia dla Azure AD nawiązywania połączenia z aplikacji zarządzania użytkownikami interfejsu API zostały wprowadzone. Wymagane dane wejściowe są różne w zależności od aplikacji. Aby uzyskać informacje poświadczeń i wymagania dotyczące określonej aplikacji, zobacz [Samouczek konfiguracji dla tej konkretnej aplikacji](active-directory-saas-app-provisioning.md#list-of-apps-that-support-automated-user-provisioning).

Wybierając przycisk **Testuj połączenie** umożliwia testowanie poświadczeń przez Azure AD próba nawiązania połączenia z aplikacji jest inicjowania obsługi administracyjnej aplikacji przy użyciu podanych poświadczeń.

###<a name="mappings"></a>Mapowania
To miejsce, w którym administratorzy można wyświetlać i edytować jakie blokowego atrybuty użytkownika między Azure AD i aplikację docelową, w przypadku kont użytkowników są obsługi administracyjnej lub aktualizacji.

Istnieje zestaw wstępnie mapowań między obiektami użytkownika Azure AD i każdej aplikacji władz akredytacji bezpieczeństwa użytkownika. Niektóre aplikacje Zarządzanie innych typów obiektów, takich jak grupy lub kontakty. Wybierając jeden z tych mapowań z tabelą edytora mapowania po prawej stronie, gdzie można je było wyświetlać i dostosowania.

![Karta zasobu aplikacji][2]

Obsługiwane dostosowania podczas pierwszego podglądu obejmują:

* Włączanie i wyłączanie mapowania dla określonych obiekty, takie jak obiekt użytkownika Azure AD do obiektu użytkownika aplikacji władz akredytacji bezpieczeństwa.

* Edytowanie, które atrybuty przepływ z obiektu użytkownika Azure AD do obiektu użytkownika tej aplikacji. Aby uzyskać więcej informacji na mapowanie atrybutu zobacz [Opis atrybut Mapowanie typów](active-directory-saas-customizing-attribute-mappings.md#understanding-attribute-mapping-types).

* Filtrowanie działania obsługi administracyjnej Azure AD należy wykonać w aplikacji docelowej, które są nowością w portalu Azure. Zamiast Azure AD w pełni zsynchronizować obiektów, można ograniczyć akcje wykonywane. Na przykład przez tylko wybieranie **aktualizacji**, aktualizacje tylko Azure AD istniejącego użytkownika konta w aplikacji, a nie tworzyć nowych plików. Po zaznaczeniu tylko **Tworzenie**, Azure tylko tworzy nowe konta użytkowników, ale nie wpływa na istniejące. Ta funkcja umożliwia administratorom tworzenie różnych mapowania do utworzenia konta i aktualizowanie przepływów pracy. Pełna możliwość tworzenia wielu mapowań na aplikacji jest planowane w dalszej części okresu Podgląd.

###<a name="settings"></a>Ustawienia
W tej sekcji umożliwia administratorom Rozpocznij i Zatrzymaj Azure AD inicjowania obsługi administracyjnej usługi dla wybranej aplikacji, a także Opcjonalnie wyczyść pamięć podręczną obsługi administracyjnej i ponownie uruchomić usługę.

Jeśli inicjowania obsługi administracyjnej jest włączonych po raz pierwszy dla aplikacji, należy włączyć usługę przez zmianę **Stanu inicjowania obsługi administracyjnej** **w**. To powoduje, że Azure AD inicjowania obsługi administracyjnej usługi przeprowadzić początkowej synchronizacji w przypadku tekstu użytkowników przydzielonych w sekcji **Użytkownicy i grupy** , kwerendy aplikacji docelowej dla nich, a następnie wykonuje obsługi administracyjnej akcje zdefiniowane w sekcji Azure AD **mapowania** . W trakcie tego procesu inicjowania obsługi administracyjnej usługi przechowuje zapisujące dane o kontach użytkowników, która zarządza, tak aby niezarządzanych konta w aplikacji docelowej, które zostały przeze mnie w zakresie przydziału nie mają wpływu Anuluj inicjowania obsługi administracyjnej operacji. Po początkowej synchronizacji inicjowania obsługi administracyjnej usługi automatycznie synchronizuje użytkowników i grupowanie obiektów na dziesięć minut.

Po prostu zmiana **Stanu inicjowania obsługi administracyjnej** **off** powoduje wstrzymanie obsługi administracyjnej usługi. W tym stanie Azure nie tworzenie, aktualizowanie lub usuwanie dowolnego użytkownika lub grupowanie obiektów w aplikacji. Zmiana stanu pozycji Wł powoduje usługi wybierz miejsce, w którym zostało przerwane.

Zaznaczenie pola wyboru **wyczyścić bieżący stan i ponownie uruchomić synchronizację** i zapisywanie zatrzymuje usługę obsługi administracyjnej zrzutów pamięci podręcznej dane dotyczące konta Azure AD jest używana do zarządzania, uruchamia usługi i wykonuje ponownie początkowej synchronizacji. Ta opcja umożliwia administratorom rozpoczęcie procesu wdrażania obsługi administracyjnej ponownie.

###<a name="synchronization-details"></a>Szczegóły synchronizacji
Ta sekcja zawiera dodawanie szczegółów operacji inicjowania obsługi administracyjnej usługi, w tym godziny imię i nazwisko, które usługa dostarczania uruchomiono przed aplikację i ile obiektów grup i użytkowników są zarządzane.

Łącza są dostarczane do **raportu działania obsługi**zawiera dziennik wszystkich użytkowników i grup utworzone, zaktualizowane oraz usunięte między Azure AD i docelowej aplikacji i **obsługi raport o błędach** , które znajdują się bardziej szczegółowe komunikaty o błędach dla użytkownika i grupowanie obiektów, które nie można odczytać utworzony, zaktualizowane lub usunięte. 

[0]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-blade.PNG
[1]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-provisioning.PNG
[2]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-provisioning-mapping.PNG
