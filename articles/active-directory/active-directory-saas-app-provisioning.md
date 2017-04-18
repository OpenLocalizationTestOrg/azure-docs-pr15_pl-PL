<properties
    pageTitle="Zautomatyzowany użytkownika aplikacji władz akredytacji bezpieczeństwa inicjowania obsługi administracyjnej w Azure AD | Microsoft Azure"
    description="Wprowadzenie do wykorzystania Azure AD zainicjować automatycznie, usuwanie i konta użytkowników są stale aktualizowane w wielu aplikacjach władz akredytacji bezpieczeństwa innych firm."
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="02/09/2016"
    ms.author="asmalser-msft"/>

#<a name="automate-user-provisioning-and-deprovisioning-to-saas-applications-with-azure-active-directory"></a>Automatyzowanie użytkownika ubezpieczenia i cofanie ubezpieczeń do władz akredytacji bezpieczeństwa aplikacji z usługą Azure Active Directory

##<a name="what-is-automated-user-provisioning-for-saas-apps"></a>Co to jest automatyczne przypisywanie użytkowników w przypadku aplikacji władz akredytacji bezpieczeństwa?

Azure Active Directory (Azure AD) umożliwia zautomatyzować tworzenie, konserwacja i usunięcie tożsamości użytkownika w chmurze ([władz akredytacji bezpieczeństwa](https://azure.microsoft.com/overview/what-is-saas/)) aplikacji, takich jak Dropbox, usług Salesforce, ServiceNow i innych elementów.

**Poniżej przedstawiono kilka przykładowych co ta funkcja umożliwia:**

- Automatyczne tworzenie nowych kont w prawo aplikacje władz akredytacji bezpieczeństwa dla nowych osób po dołączeniu do zespołu.
- Automatycznie dezaktywować konta z aplikacjami władz akredytacji bezpieczeństwa, gdy osoby opuszczanie uniknięcia zespołu.
- Upewnij się, że tożsamości w aplikacji władz akredytacji bezpieczeństwa są aktualizowane na podstawie zmian w katalogu.
- Inicjowanie obsługi nienależących do użytkowników obiekty, takie jak grupom w celu aplikacji władz akredytacji bezpieczeństwa, które obsługują je.

**Przypisywanie użytkowników automatyczną zawiera również następujące funkcje:**

- Możliwość zgodnie z istniejącymi tożsamościami między Azure AD i władz akredytacji bezpieczeństwa aplikacji.
- Opcje dostosowywania Pomocy Azure AD dopasowania bieżącej konfiguracji aplikacji władz akredytacji bezpieczeństwa, obecnie używane w Twojej organizacji.
- Opcjonalnie pocztą e-mail alerty dla błędy inicjowania obsługi administracyjnej.
- Dzienniki raportów i działania ułatwiające monitorowania i rozwiązywania problemów.

##<a name="why-use-automated-provisioning"></a>Dlaczego warto używać automatycznego inicjowania obsługi administracyjnej?

Niektóre typowe motywacji dla tej funkcji należą:

- Aby uniknąć kosztów, nieefektywność i ludzi błąd związany z ręcznego inicjowania obsługi administracyjnej procesów.
- Aby zabezpieczyć organizacji usuwając natychmiast tożsamość użytkownika z najważniejszych aplikacji władz akredytacji bezpieczeństwa Jeśli opuszczona przez organizację.
- Aby łatwo importować zbiorcze liczba użytkowników do określonej aplikacji władz akredytacji bezpieczeństwa.
- Słuchanie wygody konieczności obsługi administracyjnej rozwiązania uruchamianie poza zdefiniowane dla Azure AD rejestracji jednokrotnej zasady dostępu samej aplikacji.

##<a name="frequently-asked-questions"></a>Często zadawane pytania

**Jak często Azure AD zapisanie zmian katalogu do aplikacji władz akredytacji bezpieczeństwa?**

Azure AD sprawdza, czy zmiany każdej pięciu do dziesięciu minut. Jeśli aplikacja władz akredytacji bezpieczeństwa zwraca kilku błędów (takie jak w przypadku poświadczeń administratora nieprawidłowe), w Azure AD będzie stopniowo spowalniać jego częstotliwości maksymalnie raz dziennie, dopóki nie zostaną one usunięte.

**Jak długo trwa obsługi administracyjnej moich użytkowników?**

Zmiany przyrostowe wystąpić niemal natychmiast, ale jeśli chcesz zapewnić większą część katalogu, następnie to zależy od liczby użytkowników i grup, do których masz. Katalogi małych potrwać tylko kilka minut, średnie katalogów może potrwać kilka minut i bardzo duże katalogów może potrwać kilka godzin.

**Jak można śledzić postęp bieżącego zadania obsługi administracyjnej**

Raport inicjowania obsługi administracyjnej konta katalogu, w sekcji Raporty można przeglądać. Innym rozwiązaniem jest odwiedź karta Pulpit nawigacyjny aplikacji władz akredytacji bezpieczeństwa są inicjowania obsługi administracyjnej do i poszukaj w sekcji "Stan integracji" u dołu strony.

**Skąd będą wiedzieć, jeśli użytkownicy nie mogli uzyskać obsługi administracyjnej poprawnie?**

Na koniec obsługi administracyjnej konfiguracji Kreator jest opcję subskrybować powiadomienia e-mail dla błędy inicjowania obsługi administracyjnej. Możesz również sprawdzić raport błędy inicjowania obsługi administracyjnej, aby wyświetlić użytkowników, którzy nie może być przygotowana i dlaczego.

**Można Azure AD zapisywać zmian w aplikacji władz akredytacji bezpieczeństwa do katalogu?**

W przypadku większości aplikacji władz akredytacji bezpieczeństwa inicjowania obsługi administracyjnej jest ruchu wychodzącego tylko, co oznacza, że użytkownicy są zapisywane z katalogu do aplikacji, a zmiany z poziomu aplikacji nie można zapisać powrót do katalogu. Dla [Workday](https://msdn.microsoft.com/library/azure/dn762434.aspx)inicjowania obsługi administracyjnej jest jednak ruchu przychodzącego tylko, co oznacza, że użytkownicy zostaną zaimportowane do katalogu z pracy i Analogicznie, zmiany w katalogu nie zostać zapisane powrót do pracy.

**Jak można przesyłać opinie do zespołu inżynierów?**

Skontaktuj się z nami za pośrednictwem [usługi Azure Active Directory forum opinii](https://feedback.azure.com/forums/169401-azure-active-directory/).

##<a name="how-does-automated-provisioning-work"></a>Jak działa automatyczne obsługi administracyjnej?

Azure AD przepisy użytkownikom aplikacji władz akredytacji bezpieczeństwa przez nawiązanie połączenia z inicjowania obsługi administracyjnej punkty końcowe dostarczoną przez dostawcę każdej aplikacji. Te punkty końcowe Zezwalaj Azure AD programowy tworzenie, aktualizowanie i usuwanie użytkowników. Poniżej przedstawiono krótki przegląd różnych czynności, które Azure AD przetwarza zautomatyzować inicjowania obsługi administracyjnej.

1. Po włączeniu inicjowania obsługi administracyjnej aplikacji po raz pierwszy, wykonywane są następujące czynności:
 - Azure AD spróbuje zgodne istniejących użytkowników w aplikacji władz akredytacji bezpieczeństwa z odpowiednich tożsamości w katalogu. Jeśli użytkownik jest takie samo, są one *automatycznie niedostępny dla rejestracji jednokrotnej* . Aby użytkownik miał dostęp do aplikacji ta osoba musi mieć jawnie przypisane do aplikacji w Azure AD bezpośrednio lub za pośrednictwem członkostwa w grupach.
 - Jeśli określono już użytkowników, którzy mają być przydzielane do aplikacji i Azure AD kończy się niepowodzeniem znaleźć istniejących kont dla tych użytkowników, Azure AD dodawać nowe konta dla nich w aplikacji.
2. Po ukończeniu wstępnej synchronizacji, zgodnie z powyższym opisem, Azure AD sprawdzi 10 minut dla następujących zmian:
 - Jeśli nowych użytkowników zostały przypisane do aplikacji (bezpośrednio lub za pośrednictwem członkostwa w grupach), następnie będą one obsługi administracyjnej nowego konta w aplikacji władz akredytacji bezpieczeństwa.
 - Jeśli usunięto dostępu użytkownika, a następnie swojego konta w aplikacji władz akredytacji bezpieczeństwa zostanie oznaczona jako wyłączona (użytkownicy są nigdy nie pełni usuwane, która zapewnia ochronę przed utratą danych w przypadku konfiguracji).
 - Jeśli użytkownik ostatnio został przypisany do aplikacji, a one już konto w aplikacji władz akredytacji bezpieczeństwa, to konto zostanie oznaczone jako włączone, a niektóre właściwości użytkownika mogą być aktualizowane, jeśli są one nieaktualne w porównaniu z katalogu.
 - Jeśli informacje użytkownika (na przykład numer telefonu, lokalizację biura itp.) został zmieniony w katalogu, a następnie te informacje również zostaną zaktualizowane w aplikacji władz akredytacji bezpieczeństwa.

Aby uzyskać więcej informacji na temat odwzorowania atrybuty między Azure AD i aplikacji władz akredytacji bezpieczeństwa, zobacz artykuł w [Dostosowywania mapowań atrybutów](active-directory-saas-customizing-attribute-mappings.md).

##<a name="list-of-apps-that-support-automated-user-provisioning"></a>Wykaz aplikacji, które obsługują automatyczne przypisywanie użytkowników

Kliknij aplikację, aby wyświetlić samouczek na temat konfigurowania automatycznego inicjowania obsługi administracyjnej dla niego:

- [Pole](http://go.microsoft.com/fwlink/?LinkId=286016)
- [Citrix GoToMeeting](http://go.microsoft.com/fwlink/?LinkId=309580)
- [Cząstkowe](http://go.microsoft.com/fwlink/?LinkId=309575)
- [Docusign](http://go.microsoft.com/fwlink/?LinkId=403254)
- [Dropbox dla firm](http://go.microsoft.com/fwlink/?LinkId=309581)
- [Usługi Google Apps](http://go.microsoft.com/fwlink/?LinkId=309577)
- [Jive](http://go.microsoft.com/fwlink/?LinkId=309591)
- [Usługi SalesForce](http://go.microsoft.com/fwlink/?LinkId=286017)
- [Piaskownicy usług SalesForce](http://go.microsoft.com/fwlink/?LinkId=327869)
- [ServiceNow](http://go.microsoft.com/fwlink/?LinkId=309587)
- [Dzień roboczy](http://go.microsoft.com/fwlink/?LinkId=690250) (ruch przychodzący inicjowania obsługi administracyjnej)

Aby aplikację do obsługi użytkownika automatycznego inicjowania obsługi administracyjnej to przydzielać niezbędne punkty końcowe umożliwiające zewnętrznych programy, aby zautomatyzować tworzenie, konserwacja i usuwania użytkowników. Dlatego nie wszystkie aplikacje władz akredytacji bezpieczeństwa są zgodne z tą funkcją. W przypadku aplikacji, które obsługują to zespół inżynierów Azure AD będzie mógł tworzyć obsługi administracyjnej łącznika do tych aplikacji, a prac priorytety są przypisywane zgodnie z potrzebami aktualnych i potencjalnych klientów.

Aby skontaktować się zespół inżynierów Azure AD żądania obsługi administracyjnej obsługę dodatkowe aplikacje, Prześlij wiadomość za pośrednictwem [usługi Azure Active Directory forum opinii](https://feedback.azure.com/forums/169401-azure-active-directory/).

##<a name="related-articles"></a>Artykuły pokrewne

- [Artykuł indeks do zarządzania aplikacjami w usłudze Azure Active Directory](active-directory-apps-index.md)
- [Dostosowywanie mapowań atrybutów dla użytkownika inicjowania obsługi administracyjnej.](active-directory-saas-customizing-attribute-mappings.md)
- [Wyrażeń do mapowania atrybutów](active-directory-saas-writing-expressions-for-attribute-mappings.md)
- [Określanie zakresu filtry dla użytkownika inicjowania obsługi administracyjnej.](active-directory-saas-scoping-filters.md)
- [Aby włączyć automatyczne inicjowania obsługi administracyjnej użytkowników i grup z usługi Azure Active Directory do aplikacji przy użyciu SCIM](active-directory-scim-provisioning.md)
- [Konto inicjowania obsługi administracyjnej powiadomienia](active-directory-saas-account-provisioning-notifications.md)
- [Lista samouczki dotyczące integracja aplikacji władz akredytacji bezpieczeństwa](active-directory-saas-tutorial-list.md)
