<properties
  pageTitle="Zarządzanie dostępem do aplikacji za pomocą Azure AD |  Microsoft Azure"
  description="W tym artykule opisano, jak usługi Azure Active Directory umożliwia organizacjom Określ aplikacje, do których każdy użytkownik ma dostęp."
  services="active-directory"
  documentationCenter=""
  authors="femila"
  manager="femila"
  editor=""/>

 <tags
  ms.service="active-directory"
  ms.workload="identity"
  ms.tgt_pltfrm="na"
  ms.devlang="na"
  ms.topic="article"
  ms.date="10/13/2016"
  ms.author="femila"/>


# <a name="managing-access-to-apps"></a>Zarządzanie dostępem do aplikacji

Zarządzanie zapewnienia stałego dostępu, oceny zastosowania i raportowania w dalszym wezwanie po aplikacji jest zintegrowany z firmowym systemie tożsamości. W większości przypadków pozwala administratorom systemów informatycznych lub pracownikom pomocy technicznej wymagać trwających aktywną rolę w zarządzaniu dostępem do swoich aplikacji. Czasami przydziału jest wykonywane przez zespół informatycznej ogólne lub oddziałów. Często decyzja przydział ma przekazać maker decyzji biznesowych wymaganie zatwierdzenia przed IT ułatwia przydziału.  Inne organizacje inwestycji w integracji z istniejących automatyczną tożsamości i dostępu do zarządzania systemem, takie jak kontrola dostępu oparta na rolach (RBAC) lub na atrybutach kontroli dostępu (ABAC). Integracji i rozwoju reguły zwykle specjalistyczne i drogich. Monitorowania i raportowania albo metodę zarządzania jest inwestycji osobnych, kosztów i złożone.

## <a name="how-does-azure-active-directory-help"></a>W jaki sposób pomaga usługi Azure Active Directory

 Azure AD obsługuje zarządzanie doskonały dostęp do skonfigurowanych aplikacji umożliwia organizacjom łatwe osiągnięcia zasady dostępu prawej, od automatyczne opartej na atrybutach przydziału (scenariusze ABAC lub RBAC) za pośrednictwem przedstawicielstwa i tym administratora zarządzania. Z usługą Azure Active Directory można łatwo uzyskać złożonych zasady, łączenie wielu modeli zarządzania dla pojedynczej aplikacji i można ponownie użyć nawet zasady zarządzania w aplikacjach z tym samym odbiorców.

 - [Dodawanie nowych lub istniejących aplikacji](active-directory-sso-integrate-saas-apps.md)


 Przypisanie aplikacji Azure AD omówiono dwa tryby podstawowy przydział:

- **Danego przydziału** Administrator systemu informatycznego z uprawnieniami administratora globalnego katalogu można wybrać poszczególnych kont użytkowników i udzielić im dostęp do aplikacji.
- **Przydział oparte na grupach (płatność tylko Azure AD)** Administrator systemu informatycznego z uprawnieniami administratora globalnego katalogu można przypisywać grupy aplikacji. Określonym użytkownikom dostępu zależy od tego, czy są one członków grupy w czasie próbie uzyskania dostępu do aplikacji. Innymi słowy administrator może utworzyć skuteczne reguły przypisania informacją "bieżący członkom grupy przypisane ma dostęp do aplikacji". Przy użyciu tej opcji przydziału, Administratorzy mogą korzystać z jednej z opcji zarządzania grupy Azure AD, w tym [na atrybutach dynamiczne grup](active-directory-accessmanagement-manage-groups.md), grupy systemu zewnętrznego (na przykład w lokalnej usłudze Active Directory lub pracy) lub zarządzane przez administratora lub zarządzany eksploatacyjnych włączenie grup. Jednej grupy można łatwo przypisać do wielu aplikacji, zapewnianie udostępnić reguł przypisania aplikacji z koligacją przydziału zmniejszania złożoność ogólnego zarządzania. Pamiętaj, zagnieżdżonej grupy oparte na grupach przydziału dla aplikacji, w tym razem nie są obsługiwane członkostwa.

Korzystając z następujących trybów dwóch przydziału, Administratorzy można uzyskać jakiejkolwiek metody zarządzania pożądane przydziału.

Z Azure AD użycia i raportowania przydziałów jest w pełni zintegrowany, umożliwiające administratorom łatwe raport o stanie przydziału, błędy przydziałów i zastosowania nawet.

## <a name="complex-application-assignment-with-azure-ad"></a>Przydział złożonej aplikacji z usługą Azure Active Directory

Należy rozważyć, czy aplikacji, takich jak usługi Salesforce. W wielu organizacjach usługi Salesforce używane głównie przez organizacje marketingu i sprzedaży. Często członkowie zespołu marketingowego wiele masz uprawnień dostępu do usług Salesforce, gdy członkowie zespołu sprzedaży mają ograniczony dostęp do. W większości przypadków ogólne populacji pracownicy przetwarzający informacje ograniczono dostęp do aplikacji. Wyjątki od tych reguł skomplikować spraw. Często jest prawa poszczególnych członkom zespołu Zarządzanie marketingowe lub sprzedaży Udziel użytkownikowi dostępu lub zmienić ich role niezależnie od tych reguł ogólne.

Azure AD aplikacji, takich jak usługi Salesforce może być wstępnie skonfigurowane dla logowania jednokrotnego (SSO) i automatycznego inicjowania obsługi administracyjnej. Po skonfigurowaniu aplikacji Administrator może przejąć jednorazowego akcji do tworzenia i przypisywania odpowiednich grup. W tym przykładzie administrator wykonanie następujących przydziałów:

- [Dynamiczne grupy](active-directory-accessmanagement-manage-groups.md) można zdefiniować oznaczający automatycznie dla wszystkich członków zespołów marketingu i sprzedaży, przy użyciu atrybutów, takich jak dział lub ról:

    - Wszystkich członków grupy marketingowych może zostać przypisana rola "marketing" w usług Salesforce

    - Wszyscy członkowie zespołu sprzedaży, które grupy może zostać przypisana rola "sprzedaż" w usług Salesforce. Dalsze uściślenia można użyć wielu grup reprezentujących regionalnych zespoły przypisane do różnych ról usług Salesforce.

- Aby włączyć mechanizm wyjątku, Samoobsługowe grupy może zostać utworzony dla poszczególnych ról. Na przykład można utworzyć grupy "Usług Salesforce działań marketingowych wyjątku" grupowo Sklep internetowy. Grupy można przypisywać do roli marketingowych usług Salesforce i zespołu marketingowego zarządzanie mogą zostać wprowadzone właściciele. Członkom zespołu zarządzanie może Dodawanie lub usuwanie użytkowników, ustawianie zasad sprzężenia lub nawet Zatwierdź lub odrzucać żądania poszczególnych użytkowników, aby dołączyć do. Jest to obsługiwane przez środowisko odpowiednie informacje pracownika, niewymagający specjalistyczne szkolenie dla właścicieli lub członków.

W tym przypadku wszystkich użytkowników przydzielonych będzie automatycznie tak aby usług Salesforce, gdy są one dodawane do różnych grup, do których chcesz być aktualizowane ich przypisanie roli usług Salesforce. Użytkownicy będą mogli odnajdowanie i uzyskać dostęp do usług Salesforce, za pomocą panelu dostępu do aplikacji firmy Microsoft, klienci sieci web pakietu Office, lub nawet, przechodząc do strony logowania usługi Salesforce organizacji. Administratorzy będzie można łatwo wyświetlać stan użycia i przydziału, używając Azure AD raportowania.

Administratorzy mogą zastosować [dostępu warunkowego Azure AD](active-directory-conditional-access.md) , aby ustawić zasady dostępu dla określonych ról. Te zasady można dołączyć, czy dostęp jest dozwolone poza firmie i nawet uwierzytelnianie wieloskładnikowe lub urządzenia wymagania uzyskanie dostępu w różnych wielkości liter.

## <a name="how-can-i-get-started"></a>Jak mogę rozpocząć?

Pierwszy, jeśli nie używasz Azure AD i administrator systemu informatycznego:

 - [Przetestuj!](https://azure.microsoft.com/trial/get-started-active-directory/) -Możesz Załóż bezpłatną 30-dniową wersję próbną już dziś i wdrażanie rozwiązania pierwszej cloud w obszarze 5 minut, przy użyciu tego łącza

Azure AD funkcje, które umożliwiają udostępnianie konta:

- [Przypisanie grupy](active-directory-accessmanagement-self-service-group-management.md)
- Dodawanie aplikacji do Azure AD
- Wprowadzenie do przydziału
- Przypisanie aplikacji — często zadawane pytania
- [Raporty pulpitu nawigacyjnego zastosowania aplikacji](active-directory-passwords-get-insights.md)

## <a name="where-can-i-learn-more"></a>Gdzie można dowiedzieć się więcej?

- [Artykuł indeks do zarządzania aplikacjami w usłudze Azure Active Directory](active-directory-apps-index.md)
- [Ochrona aplikacji programu access warunkowe](active-directory-conditional-access.md)
- [Grupa samodzielne zarządzanie SSAA](active-directory-accessmanagement-self-service-group-management.md)
