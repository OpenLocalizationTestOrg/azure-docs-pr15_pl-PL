Organizacje używają więcej [usług (władz akredytacji bezpieczeństwa)](https://azure.microsoft.com/overview/what-is-saas/) aplikacje wydajności, ponieważ narzędzi i technologii chmury stają się bardziej dostępny. W miarę liczba władz akredytacji bezpieczeństwa aplikacji staje się trudne, Administratorzy mogą zarządzać kontami i praw dostępu i użytkowników do różnych haseł. Zarządzanie te aplikacje pojedynczo tworzy dodatkowej pracy i jest mniej bezpieczne.


- Pracowników, którzy mają umożliwia śledzenie wielu haseł zwykle metodami bezpiecznego mniej zapamiętać, zapisanie hasła albo przy użyciu tych samych haseł w wielu kont.

- Po otrzymaniu nowego pracownika lub jedną pozostawia, swoje konta musi być indywidualnie obsługi administracyjnej lub usuwane.

- Ponadto pracowników może zostać uruchomiony za pomocą aplikacji władz akredytacji bezpieczeństwa dla swojej pracy bez pośrednictwa IT, co oznacza będą tworzyć własne konta na komputerach pozwala administratorom systemów informatycznych nie jeszcze zatwierdzony i nie są monitorowania.  

Rozwiązanie dla wszystkich tych problemów jest logowania jednokrotnego (SSO). Jest najłatwiejszym sposobem na zarządzanie wieloma aplikacjami i umożliwić użytkownikom spójny wygląd logowania jednokrotnego. Azure Active Directory (Azure AD) zawiera niezawodne rozwiązanie rejestracji Jednokrotnej i zawiera wiele dostępnych wstępnie zintegrowanych aplikacji, z samouczki dla administratorów szybko skonfigurować nową aplikację i Rozpoczynanie inicjowania obsługi administracyjnej użytkowników.


## <a name="how-does-azure-active-directory-integrate-apps"></a>Jak usługi Azure Active Directory zintegrować aplikacje?  

Azure AD umożliwia integrację ustanawianie kont i aplikacjami. Można to zrobić, korzystając z jednej z dwóch metod.

- Jeśli aplikacja jest wstępnie zintegrowane w galerii aplikacji, można przejść przez przeszukiwanej skonfigurować aplikacje i skonfiguruj ustawienia, aby zezwolić logowania jednokrotnego. Dla dowolnej aplikacji galerii ułatwi Ci rozpoczęcie pracy, wykonując proste instrukcje krok po kroku przedstawione w galerii aplikacji i Azure portal umożliwiające rejestracji jednokrotnej.

- Jeśli aplikacji nie znajduje się w galerii, możesz nadal skonfigurować aplikacje większości w Azure AD jako niestandardową aplikację. Wymaga to nieco bardziej techniczny specjalizacji w celu skonfigurowania. Możesz dodać dowolnej aplikacji obsługującej SAML 2.0 jako aplikacji federacyjnych lub dowolnej aplikacji, która ma opartych na języku HTML strona logowania jako hasło logowania jednokrotnego aplikacji.

W przypadku miejsce, w którym utworzono kont dla aplikacji władz akredytacji bezpieczeństwa, które nie są zarządzane przez użytkowników IT, narzędzie [Odnajdowania aplikacji chmury](../articles/active-directory/active-directory-cloudappdiscovery-whatis.md) rozwiązaniem. To narzędzie monitoruje ruchu w sieci web do identyfikowania, które aplikacje są używane w całej organizacji i liczba osób korzystających z każdego z nich. IT można użyć te informacje, aby dowiedzieć się, jakie aplikacje użytkowników wolisz i zdecyduj, które do integracji Azure AD dla logowania jednokrotnego.  

Gdy aplikacji integrowanie Azure AD, można mapować tożsamości aplikacji ustanowionych użytkowników do odpowiednich Azure AD tożsamości.  
