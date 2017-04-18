<properties
    pageTitle="Co to jest licencjonowania firmy Microsoft Azure Active Directory? | Microsoft Azure"
    description="Opis usługi Microsoft Azure Active Directory Licencjonowanie, jak to działa, jak rozpocząć pracę i najważniejsze wskazówki, włączając usługi Office 365, Microsoft Intune Azure Active Directory Premium i wersje podstawowe"
    services="active-directory"
      keywords="Licencjonowanie Azure AD"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/23/2016"
    ms.author="curtand"/>

# <a name="what-is-microsoft-azure-active-directory-licensing"></a>Co to jest licencjonowania firmy Microsoft Azure Active Directory?

##<a name="description"></a>Opis
Azure Active Directory (Azure AD) jest tożsamości firmy Microsoft jako rozwiązania usługi (IDaaS) i platformy. Azure AD jest oferowane w wielu wersjach funkcjonalne i techniczne od Azure AD wolny, która jest dostępna z dowolnej usługi firmy Microsoft, takich jak usługi Office 365 i Dynamics, Microsoft Intune Azure (Azure AD nie powoduje wygenerowania opłat zużycie w tym trybie), aby Azure AD spłacona wersjami na przykład pakiet mobilności przedsiębiorstwa (EMS), Azure AD Premium i podstawowy, a także uwierzytelnianie wieloskładnikowe Azure (MFA). Podobnie jak wiele innych usług Microsoft online services najbardziej Azure AD opłaconej wersji są udostępniane za pomocą uprawnień dla poszczególnych użytkowników, jak w usłudze Office 365, Microsoft Intune i Azure AD. W tych przypadkach zakupu usługi jest reprezentowane z jedną lub więcej subskrypcji, a każdego Subskrypcja obejmuje przed zakupu liczbę licencji w dzierżawie. Uprawnienia dla poszczególnych użytkowników zostaną osiągnięte za pomocą przypisania licencji, tworzenia łącza między użytkownikiem a produktu, włączanie składników usługi dla użytkownika i używające jednej z przedpłaconej licencji.

[Wypróbuj Azure AD premium.](https://portal.office.com/Signup/Signup.aspx?OfferId=01824d11-5ad8-447f-8523-666b0848b381&ali=1#0)

> [AZURE.NOTE] Azure AD portalu administracyjnego jest częścią portalu klasyczny Azure. Podczas przy użyciu Azure AD nie wymaga żadnych zakupów Azure, uzyskiwanie dostępu do tego portalu wymaga aktywną subskrypcję Azure lub usługi [Azure subskrypcji wersji próbnej](https://azure.microsoft.com/pricing/free-trial/).

Ogólne omówienie możliwości usług Azure AD zobacz [Co to jest Azure AD](active-directory-whatis.md).
[Dowiedz się więcej o poziomach usługi Azure AD](https://azure.microsoft.com/support/legal/sla/)

> [AZURE.NOTE]  Subskrypcje Azure płatność przy zakupie są różne: podczas również przedstawione w katalogu, te subskrypcje umożliwiają tworzenie Azure zasobów i zamapowanie ich na metodę płatności. W takim przypadku nie istnieje bez liczby licencji skojarzone z subskrypcją. Stowarzyszenie użytkowników z subskrypcją dostępu użytkowników do zarządzania zasobami subskrypcji, uzyskuje się przyznając im uprawnienia do działania Azure zasobów mapowane z subskrypcją.


##<a name="how-does-azure-ad-licensing-work"></a>Jak działa Licencjonowanie Azure AD?

(W oparciu o uprawnienia) oparte na licencji Azure AD usługi pracy przez aktywowanie subskrypcji w dzierżawie usługi katalogowej-Azure AD. Gdy subskrypcja jest aktywna możliwości usług można zarządzane przez administratorów usługi katalogowej- i używane przez liczby licencjonowanych użytkowników.

Podczas zakupu lub uaktywnienie Enterprise Suite mobilności, Azure AD Premium lub Azure AD podstawowe katalogu zostanie zaktualizowany zgodnie z subskrypcji, łącznie z jego okres ważności i przedpłaconej licencji. Informacje o Twojej subskrypcji w tym stanu, następne zdarzenie cykl życia i liczba licencji przypisanych lub dostępne jest dostępna za pośrednictwem portalu klasyczny Azure na karcie licencji dla określonego katalogu. Jest to także najlepiej do zarządzania przypisaniami licencji.

Każdej subskrypcji składa się z co najmniej jeden plan usługi każdego mapowania uwzględniane poziom funkcjonalności typu usług; na przykład Azure AD Azure MFA, Intune firmy Microsoft, usługi Exchange Online lub SharePoint Online. Zarządzanie licencjami w usłudze Azure AD nie wymaga zarządzania poziomu plan usług. Różni się od usługi Office 365, która zależy od tego trybu konfiguracji zaawansowanej zarządzać dostępem do dołączonych usług. Azure AD zależy od w konfiguracji usługi Włączanie funkcji i zarządzać nimi z poszczególnych uprawnień.

Zazwyczaj informacje o subskrypcji Azure AD odbywa się za pośrednictwem portalu klasyczny Azure, na karcie licencji dla określonego katalogu. Azure AD subskrypcji, z wyjątkiem Azure AD Premium, nie są wyświetlane w portalu usługi Office.

> [AZURE.IMPORTANT] Azure AD Premium i podstawowe, a także subskrypcje Enterprise Suite mobilności są ograniczone do ich ustanawianie katalogu/dzierżawy. Subskrypcji nie można podzielić między katalogów lub użyć do uprawnia użytkowników w innych katalogów. Przenoszenie subskrypcji między katalogów jest możliwe, ale wymaga przesyłania bilet pomocy technicznej lub anulowania i ponownie zakupu w przypadku bezpośrednich zakupów.

> Po zakupieniu Azure AD lub Enterprise Suite mobilności za pośrednictwem licencjonowania zbiorowego Aktywacja subskrypcji zostanie przeprowadzona automatycznie po Umowa obejmuje innych usług Microsoft Online, na przykład usługi Office 365.

Płatna pomoc telefoniczna Azure AD funkcje zakresu szerokość katalogu. Przykłady:
- Oparte na grupach przypisanie do aplikacji, które włączono w obszarze określonej aplikacji, którą zarządzasz.
- Zaawansowane i funkcje zarządzania Samoobsługowe grupy są dostępne w obszarze Konfiguracja katalogu lub określonej grupy.
- Raporty dotyczące zabezpieczeń Premium znajdują się na karcie raportowania
- Chmura odnajdowania aplikacji wyświetlane w portalu Azure tożsamością.

###<a name="assigning-licenses"></a>Przypisywanie licencji
Podczas uzyskiwania subskrypcji to wszystko, musisz skonfigurować możliwości płatnej, za pomocą usługi Azure AD spłacona funkcje wymaga rozpowszechniania licencji osobom prawo. Zazwyczaj każdy użytkownik, kto ma dostęp do lub który odbywa się za pośrednictwem Azure AD spłacona funkcji musi mieć przypisane licencji. Przypisanie licencji jest mapowanie między użytkownikiem a kupowane usługi, takiej jak Azure AD Premium, Basic lub Enterprise Suite mobilności.

Zarządzanie, którzy użytkownicy w katalogu należy dysponować licencją jest proste. Można wykonać, przypisując do grupy do tworzenia reguł przydziału za pośrednictwem portalu administracyjnego Azure AD lub przypisując licencji bezpośrednio do prawej osób za pośrednictwem portalu, programu PowerShell lub interfejsów API. Podczas przypisywania licencji do grupy, wszystkich członków grupy zostanie przypisana licencja. Jeśli użytkownicy są dodawane lub usuwane z grupy zostanie przypisany lub usunąć odpowiednią licencję. Przypisanie grupy mogą korzystać wszystkie dostępne Zarządzanie grupami i jest zgodna z oparte na grupach przydziału dla aplikacji. W ten sposób można skonfigurować reguły w taki sposób, że wszyscy użytkownicy w katalogu są automatycznie przypisywane, upewnij się, że każda osoba mająca odpowiednie stanowisko ma licencji lub nawet delegować decyzji o innych menedżerów w organizacji.

Z przydziałem oparte na grupach licencji każdy użytkownik Brak lokalizacja użytkowania dziedziczy lokalizację katalogu podczas przydziału. Tej lokalizacji mogą być zmieniane przez administratora w dowolnym momencie. W przypadku której automatycznego przydziału nie powiodło się ze względu na błąd informacje o użytkowniku w obszarze tego typu licencji zostaną zastosowane Państwa.

##<a name="getting-started-with-azure-ad-licensing"></a>Rozpoczynanie pracy z licencjonowania Azure AD

Wprowadzenie do programu Azure AD jest proste. zawsze możesz utworzyć katalogu jako część tworzący konto Azure bezpłatnej wersji próbnej. [Dowiedz się więcej o tworzący konto w organizacji](sign-up-organization.md). Poniższy może ułatwić upewnij się, że katalogu najlepiej jest wyrównana z innych usług firmy Microsoft mogą korzystać lub planuje używanie i cele w uzyskaniu usługę.

Poniżej przedstawiono kilka wskazówek:
- Jeśli już używasz organizacji usług firmy Microsoft, masz już katalogiem Azure AD. W tym przypadku należy nadal używać tego samego katalogu dla innych usług, tak, aby Zarządzanie tożsamościami podstawowych, w tym inicjowania obsługi administracyjnej i hybrydowego wdrożenia logowania jednokrotnego, może być wykorzystana przez usługę. Użytkownicy będą mieć możliwości logowania jednokrotnego i będą korzystać z większe możliwości w usługach. W wyniku Jeśli zdecydujesz się na zakup Azure AD opłaconej usługi dla pracowników, zalecamy używanie tego samego katalogu w tym celu.
- Jeśli planujesz używać Azure AD dla innego zestawu użytkowników (partnerów, klientów i tak dalej), lub jeśli chcesz ocenić usługi Azure AD i chcesz zrobić w izolacji usługi produkcji lub jeśli szukasz konfiguracji środowiska piaskownicy dla usług, zaleca się najpierw utworzyć nowy katalog za pośrednictwem portalu klasyczny Azure Azure. [Dowiedz się więcej na temat tworzenia nowego katalogu Azure AD w portalu klasyczny Azure](active-directory-licensing-directory-independence.md). Nowy katalog zostanie utworzony przy użyciu konta jako użytkownika zewnętrznego z uprawnieniami administratora globalnego. Po zalogowaniu się do portalu klasyczny Azure z tym kontem można wyświetlić ten katalog i uzyskać dostęp do wszystkich zadań administracyjnych w katalogu. Zaleca się utworzenie lokalnego konta z odpowiednimi uprawnieniami do zarządzania innych usług firmy Microsoft, (te nie są dostępne za pośrednictwem portalu klasyczny Azure). [Aby uzyskać więcej informacji o tworzeniu kont użytkowników w Azure AD](active-directory-create-users.md).

> [AZURE.NOTE] Azure AD obsługuje "użytkowników zewnętrznych", które są kontami użytkowników w przypadku Azure AD, które zostały utworzone za pomocą konta Microsoft (MSA) lub tożsamości Azure AD z innego katalogu. Są nam zajęty rozszerzenie tej funkcji w całej organizacji usług firmy Microsoft, teraz tych kont nie są obsługiwane w niektórych środowiska usługi; na przykład portalu administracyjnego usługi Office 365 nie obsługuje obecnie tych użytkowników. W wyniku użytkowników zewnętrznych z kontami Microsoft nie będzie mógł uzyskać dostęp do portalu administracyjnego usługi Office 365, gdy użytkowników zewnętrznych z innych katalogów Azure AD będą ignorowane. W drugim przypadku tylko lokalne konta użytkownika, Azure AD lub katalogów usługi Office 365, gdzie użytkownik został utworzony, będzie dostępne za pośrednictwem funkcji.

Jak wskazano, Azure AD ma różnych wersji płatnej. Tych wersji niewielkimi różnicami w ich dostępność zakupu:


| Produktu   | EA/LICENCJONOWANIA ZBIOROWEGO     | Otwieranie  |   DOSTAWCY |   Prawa dotyczące użytkowania MPN  |   Bezpośrednie zakupu | Wersja próbna |
|---|---|---|---|---|---|---|
| Pakiet mobilności przedsiębiorstwa |   X | X | X | X |  |      X |
| Azure AD Premium  | X | X | X |   | X | X |
| Azure AD Basic    | X | X | X | X |  <br /> |  <br />  |

###<a name="select-one-or-more-license-trials"></a>Zaznacz jeden lub więcej licencji wersji próbnych
 We wszystkich przypadkach można aktywować Azure AD Premium lub Enterprise Suite mobilności subskrypcji wersji próbnej, wybierając określonej wersji próbnej, odpowiedni na karcie licencji w katalogu. Obu wersji próbnej zawiera subskrypcję 30-dniową 100 licencji.

![Planów licencja na wersję próbną usługi Azure Active Directory](./media/active-directory-licensing-what-is/trial_plans.png)

![Plany licencja na wersję próbną pakietu mobilności przedsiębiorstwa](./media/active-directory-licensing-what-is/EMS_trial_plan.png)

![Plany aktywne licencję wersji próbnej](./media/active-directory-licensing-what-is/active_license_trials.png)

###<a name="assign-licenses"></a>Przypisywanie licencji
Gdy subskrypcji jest aktywne, należy przypisać licencję do siebie i Odśwież przeglądarkę, aby upewnić się, że są wyświetlane wszystkie funkcje. Następnym krokiem jest, aby przypisać licencje do użytkowników, którzy będą musieli dostępu lub zostaną uwzględnione na spłacona Azure AD funkcje. Jak wspomniano powyżej w "Przypisywanie licencji" najlepszym sposobem w tym celu jest identyfikowanie grupy odbiorców reprezentujący i przypisać licencję; w ten sposób użytkowników, którzy są dodawane lub usuwane z grupy na jej cyklu życia przypisane do lub usunięte z licencją.

Aby przypisać licencję do poszczególnych użytkowników lub grupy, wybierz plan licencji, do którego chcesz przypisać, a następnie kliknij polecenie **Przypisz** na pasku poleceń.

![Plany aktywne licencję wersji próbnej](./media/active-directory-licensing-what-is/assign_licenses.png)

Raz w oknie dialogowym przydziału dla wybranego planu, możesz wybrać użytkowników i dodawanie ich do **przypisywania** kolumny po prawej stronie. Możesz Przejrzyj listę użytkowników lub z wyszukiwania dla określonych osób za pomocą wyszukiwania szkło u góry rogu siatki użytkownika. Aby przypisać grupy, wybierz pozycję "Grupy" z menu **Pokaż** , a następnie kliknij przycisk Sprawdź po prawej stronie, aby odświeżyć przydziałów, które są wyświetlane.

![Przypisywanie licencji do grup](./media/active-directory-licensing-what-is/assign_licenses_to_groups.png)

Teraz można przeszukiwać lub przejrzyj grupy i dodać je do kolumny **przypisać** w ten sam sposób. Te umożliwia przypisać kombinację użytkowników i grup w jednej operacji. Aby ukończyć proces przydziału, kliknij przycisk wyboru w prawym dolnym rogu strony.

![Komunikat postępu przypisania licencji](./media/active-directory-licensing-what-is/license_assignment_progress_message.png)

Po przypisaniu grupy jej członków dziedziczą licencji na 30 minut, ale zwykle w ciągu kilku minut 1 i 2.

Błędy przypisania może wystąpić podczas Azure AD przypisania licencji, ale są stosunkowo rzadkie. Potencjalne błędy przydziałów są ograniczone do:
- Konflikt przydziału — gdy użytkownik wcześniej przypisano licencji, która nie jest zgodna z bieżącą licencją. W tym przypadku przypisania licencji nowe wymaga Usuwanie poprzedniego slajdu.
- Przekroczono dostępne licencje - liczba użytkowników w grupach przydzielonych przekracza dostępne licencje, stan przypisania użytkowników zostaną zastosowane niewykonanie Przypisz ze względu na Brak licencji.

###<a name="view-assigned-licenses"></a>Wyświetl przypisane licencje

Widok podsumowania przypisane licencje tym zdarzenia cyklu życia subskrypcji dostępne, przydzielonych i następny są wyświetlane na karcie **licencji** .

![Wyświetl liczbę przypisane licencje](./media/active-directory-licensing-what-is/view_assigned_licenses.png)

Szczegółowy wykaz przypisane użytkowników i grup, takich jak stan przypisania i ścieżkę (bezpośrednio lub dziedziczone z jednej lub kilku grup) jest dostępna, podczas nawigowania do planu licencji.

![Wyświetlanie szczegółów licencji przypisane do planu licencji](./media/active-directory-licensing-what-is/assigned_licenses_detail.png)

Usuwanie licencji jest tak proste, jak przypisywanie ich. Jeśli użytkownik został przypisany bezpośrednio lub przydzielonych grupy, możesz usunąć licencję, wybierając typu licencji, **Usuwanie**, dodając do listy Usuń użytkownika lub grupy, i potwierdzenie akcji. Można także Otwórz typu licencji, wybierz określonego użytkownika lub grupy i wybierz polecenie **Usuń** na pasku poleceń. Aby zakończyć dziedziczenie użytkownika licencji z grupy, po prostu usunąć użytkownika z grupy.

###<a name="extending-trials"></a>Rozszerzanie wersji próbnych

Wersji próbnej rozszerzenia dla klientów są dostępne jako samodzielne za pośrednictwem portalu usługi Office 365. Administrator klienta można przejść do [portalu Office](https://portal.office.com/#Billing) (access zależy od uprawnień do portalu usługi Office) i wybierz wersję próbną Azure AD Premium. Kliknij link **rozszerzona wersji próbnej** i postępuj zgodnie z instrukcjami. Konieczne będzie wprowadzenia danych karty kredytowej, ale nie zostanie obciążona.

![Rozszerzanie wersji próbnej licencji w portalu Office](./media/active-directory-licensing-what-is/extend_license_trial.png)

Po przesłaniu żądania obsługi klientów można przesyłać z rozszerzeniem wersji próbnej. Administrator klienta można przejść do usługi Office 365 portalu [stronę pomocy technicznej](http://aka.ms/extendAADtrial) (access zależy od uprawnień do strony pomocy technicznej pakietu Office). Na tej stronie wybierz pozycję "Subskrypcje i prób" w obszarze funkcje pytaniami "wersja próbna" w obszarze Symptom. Na koniec wprowadź informacje na temat okoliczności

![Rozszerzanie wersji próbnej licencji przy użyciu prośbę o pomoc techniczną](./media/active-directory-licensing-what-is/alternate_office_aad_trial_extension.png)

## <a name="next-steps"></a>Następne kroki

Teraz można przystąpić do konfigurowania i korzystać z niektórych funkcji Azure AD Premium.

- [Resetowanie hasła Sklep internetowy](active-directory-manage-passwords.md)
- [Zarządzanie grupami Sklep internetowy](active-directory-accessmanagement-self-service-group-management.md)
- [Azure AD Connect zdrowia](active-directory-aadconnect-health.md)
- [Przypisanie grupy do aplikacji](active-directory-manage-groups.md)
- [Uwierzytelnianie wieloskładnikowe Azure](../multi-factor-authentication/multi-factor-authentication.md)
- [Bezpośrednie zakupu licencji Azure AD Premium](http://aka.ms/buyaadp)
