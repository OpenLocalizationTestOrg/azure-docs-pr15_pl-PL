<properties
    pageTitle="Ochrona tożsamości Azure Active Directory | Microsoft Azure"
    description="Dowiedz się, jak ochronę Azure AD tożsamości umożliwia ograniczenie możliwości atakującej wykorzystać tożsamości złamany lub urządzenia oraz zagwarantowanie tożsamości lub urządzenie, które poprzednio były podejrzane lub wiadomo, że zostać złamane."
    services="active-directory"
    keywords="ochrona tożsamości usługi Azure active directory chmury aplikacji odnajdowanie, zarządzanie aplikacji, zabezpieczeń, czynnik ryzyka, poziom ryzyka, luka w zabezpieczeniach, zasady zabezpieczeń"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="markvi"/>

#<a name="azure-active-directory-identity-protection"></a>Ochrona tożsamości Azure Active Directory 

Azure ochronę tożsamości Active Directory to funkcja wersji P2 Azure AD Premium zapewniająca skonsolidowany widok do zdarzenia ryzyka i słabych wpływu tożsamości Twojej organizacji. Microsoft ma zostały zabezpieczanie tożsamością opartej na chmurze przez dekadę, a z ochroną Azure AD tożsamości Microsoft jest udostępnienie tych samych systemów ochrony przedsiębiorstwa. Ochrona tożsamości wykorzystanie możliwości wykrywania anomalii istniejących Azure AD osoby (dostępne za pośrednictwem Azure AD Anomalous raporty aktywności) i wprowadza nowe typy zdarzeń ryzyka, które może wykryć różnic w odniesieniu w czasie rzeczywistym.



##<a name="getting-started"></a>Wprowadzenie

Większość naruszenia bezpieczeństwa odbywać się, gdy intruzów uzyskają dostęp do środowiska przez kradzież tożsamości użytkownika. Ataki stały się bardziej efektywne używanie naruszenia innej firmy, a za pomocą zaawansowanych wyłudzania. Po atakująca uzyskuje dostęp do jeszcze konta użytkownika uprzywilejowanych niska, jest stosunkowo proste dla nich uzyskanie dostępu do zasobów firmy ważne za pośrednictwem bocznej przepływu. Dlatego jest niezbędne do ochrony wszystkich tożsamości i, gdy tożsamości jest bezpieczne, wyprzedzeniem zapobiec złamany tożsamości zostanie użyte. 

Odkrywanie złamany tożsamości jest brak proste zadania. Na szczęście może ułatwić ochronę tożsamości: ochronę tożsamości używa algorytmów nauki adaptacyjne komputera i heurystykę wykrywanie różnic w odniesieniu do ryzyka zdarzenia, które mogą wskazywać, że jest już bezpieczne tożsamości.
 
Za pomocą tych danych, raportów i alertów, które umożliwia badanie te zdarzenia ryzyka, a następnie wykonaj odpowiednią naprawy lub akcję łagodzenia ochronę tożsamości generuje.
 
Ale Azure Active Directory ochronę tożsamości jest większa niż narzędzie monitorowania i raportowania. W zależności od zdarzenia ryzyka, ochronę tożsamości oblicza poziom ryzyka użytkownika dla każdego użytkownika, co umożliwia konfigurowanie zasad ryzyka automatycznie ochrony tożsamości organizacji.  Zasady ryzyka oprócz inne funkcje kontroli dostępu warunkowego dostarczony przez usługi Azure Active Directory i EMS, można automatycznie zablokować lub oferują adaptacyjne korygowania akcje, które obejmują resetowania hasła i uwierzytelnianie wieloskładnikowe wymuszania.  

####<a name="explore-identity-protections-capabilities"></a>Poznaj funkcje ochrony tożsamości 

**Wykrywanie zdarzenia ryzyka i ryzykowna kont:**  

- Wykrywanie 6 ryzyka typów zdarzeń maszynowego uczenia się i heurystyczne reguł 

- Obliczanie poziomów ryzyka użytkownika

- Dodawanie niestandardowych zalecenia dotyczące poprawienia ogólnego postawie zabezpieczeń wyróżniając luk



**Badanie zdarzenia ryzyka:**

- Wysyłanie powiadomienia o zdarzeniach ryzyka

- Badanie zdarzenia ryzyka za pomocą przydatnych i kontekstowe informacji

- Dostarczanie podstawowych przepływów pracy umożliwia śledzenie dochodzenia

- Zapewniając łatwy dostęp do naprawy akcje, takie jak w przypadku resetowania hasła



**Zasady dostępu warunkowego ryzyka:**

- Zasady zmniejszeniu ryzykowna rejestrowaniu przez blokowanie rejestrowaniu lub wymaganie uwierzytelnianie wieloskładnikowe problemach.

- Blokowanie lub kont użytkowników ryzykowna bezpiecznego zasad

- Zasady, aby użytkownicy musieli zarejestrować uwierzytelniania wieloskładnikowego


## <a name="detection-and-risk"></a>Wykrywanie i ryzyka

### <a name="risk-events"></a>Zdarzenia ryzyka

Zdarzenia ryzyka to zdarzenia, które zostały oznaczone flagą jako podejrzane przez ochronę tożsamości i wskazują, że tożsamości mogły zostać złamane. Aby uzyskać pełną listę zdarzenia ryzyka zobacz [typy zdarzeń ryzyka wykrytych przez Azure Active Directory ochronę tożsamości](active-directory-identityprotection-risk-events-types.md). 


### <a name="risk-level"></a>Poziom ryzyka

Poziom ryzyka dla zdarzenia ryzyka jest wskazanie (wysoki, średni lub niski) ważności zdarzenia ryzyka. Poziom ryzyka ułatwia użytkownikom ochronę tożsamości ustalać ich priorytety akcje, które należy wykonać, aby zmniejszyć ryzyko swojej organizacji. Ważności zdarzenia ryzyka reprezentuje siłę sygnału jako predykcyjnych ujawnienia tożsamości, w połączeniu z hałasu, które zwykle wprowadza. 

- **Duży**: dużej pewności i zdarzenia ryzyka wysokiej ważności. Te zdarzenia są silne wskaźniki, które zostało naruszone tożsamość użytkownika, a wszystkie konta użytkowników, wpływ należy można wyeliminować natychmiast.

- **Średni**: wysokiej ważności, ale dolnym UFNOŚĆ zdarzenie ryzyka, lub odwrotnie. Te zdarzenia są potencjalnie ryzykowna, a wszystkie konta użytkowników, wpływ należy wyeliminować.

- **Niska**: Niski zaufania i zdarzenia ryzyka Niska ważność. To zdarzenie nie może wymagać natychmiastowego działania, ale w połączeniu z innymi zdarzenia ryzyka może dostarczyć silnych wskazania, że jest bezpieczne tożsamości. 


![Poziom ryzyka] (./media/active-directory-identityprotection/01.png "Poziom ryzyka")

 

Zdarzenia ryzyka albo są oznaczone w **czasie rzeczywistym**, lub w przetwarzanie końcowe po zdarzenia ryzyka miała już umieść (w trybie offline). Obecnie większość zdarzenia ryzyka w ochronę tożsamości są obliczane w trybie offline i są wyświetlane w ochronę tożsamości w 2-4 godzinach. Gdy obliczane w w czasie rzeczywistym, zdarzenia ryzyka w czasie rzeczywistym będą widoczne w konsoli ochrony tożsamości w 5-10 minut.

Kilka starszych klientów nie jest obecnie obsługiwany Wykrywanie zdarzenia w czasie rzeczywistym ryzyka i zapobiegania. W wyniku rejestrowaniu od tych klientów nie wykryto lub można zapisać w czasie rzeczywistym.


## <a name="investigation"></a>Badanie
Podróży za pośrednictwem ochronę tożsamości zwykle zaczyna się od pulpitu nawigacyjnego ochronę tożsamości. 

![Rozwiązywanie problemu] (./media/active-directory-identityprotection/1000.png "Rozwiązywanie problemu")

Pulpit nawigacyjny umożliwia dostęp do:
 
- Raporty, takich jak **Użytkownicy flagą ryzyka**, **zdarzenia ryzyka** i **usterek**
- Ustawień, takich jak konfiguracji **Zasad zabezpieczeń**, **powiadomień** i **uwierzytelnianie wieloskładnikowe rejestracji**
 

Zazwyczaj jest punktu początkowego do badania, czyli procesu recenzowania działań, dzienników i inne informacje związane ze zdarzeniem ryzyka zdecydować, czy naprawy lub łagodzenia czynności są niezbędne, i jak zostało naruszone tożsamości, a poznać zasady używania złamany tożsamości.

Można powiązać działań dochodzenia do [powiadomień](active-directory-identityprotection-notifications.md) , które Azure Active Directory ochrony wysyła na wiadomości e-mail.

W poniższych sekcjach przedstawiono więcej szczegółów i czynności, które są związane z dochodzeniem.  



## <a name="what-is-a-user-risk-level"></a>Co to jest poziom ryzyka użytkownika?

Poziom ryzyka użytkownika jest wskazanie (wysoki, średni lub niski) prawdopodobieństwo, że jest już bezpieczne tożsamość użytkownika. Go jest obliczana na podstawie zdarzenia ryzyka użytkownika, które są skojarzone z tożsamość użytkownika. 

Stan zdarzenia ryzyka jest **aktywne** czy **zamknięta**. Tylko zdarzenia ryzyka, które są **aktywne** przyczynić się do obliczania ryzyka użytkownika. 

Poziom ryzyka użytkownika jest obliczane za pomocą następujących danych wejściowych:

- Zdarzenia Active ryzyka wpływające na ochronę użytkownika
- Poziom ryzyka te zdarzenia 
- Czy podjęto akcji rozwiązywanie problemu 

![Czynniki ryzyka użytkownika] (./media/active-directory-identityprotection/1001.png "Czynniki ryzyka użytkownika")



Tworzenie zasady dostępu warunkowego, aby uniemożliwić użytkownikom ryzykowna logowanie się za pomocą poziomów ryzyka użytkownika lub zmuszaj ich do bezpiecznego zmieniać swoje hasła. 


## <a name="closing-risk-events-manually"></a>Ręczne zamykanie zdarzenia ryzyka

W większości przypadków potrwa akcje naprawy, takie jak bezpieczne hasło Resetowanie automatycznie zamknąć zdarzenia ryzyka. Jednak to może nie być możliwe.  
To, na przykład, wówczas, gdy:

- Usunięto użytkownika z wydarzeniami aktywnych czynników ryzyka
- Dochodzenia ujawnia, że zdarzenie zgłoszoną ryzyka zostały wykonywać przez godny zaufania użytkownika

Ponieważ zdarzenia ryzyka, które są **aktywne** przyczynić się do obliczania ryzyka użytkownika, może być konieczne ręczne obniżyć poziom ryzyka zamykając zdarzenia ryzyka ręcznie.  
W trakcie postępowania można wykonać dowolną z następujących akcji, aby zmienić stan zdarzenia ryzyka:

![Akcje] (./media/active-directory-identityprotection/34.png "Akcje")

- **Rozwiązywanie problemów** — Jeśli po bada zdarzenia ryzyka, zajęła akcji odpowiednie korygowania poza ochronę tożsamości i uważasz, że należy uwzględnić zdarzenia ryzyka zamknięta, oznaczanie wydarzenia jako rozwiązany. Rozpoznać zdarzeń będzie Ustaw stan zdarzenia ryzyka na zamknięte i zdarzenia ryzyka nie jest już umożliwi ryzyka użytkownika.

- **Oznacz jako wynik dodatni FAŁSZ** — w niektórych przypadkach może badanie zdarzenia ryzyka i odkrywanie, że niepoprawnie został oznaczony jako ryzykowna. Można ograniczyć liczbę wystąpień takie oznaczając zdarzenia ryzyka jako dodatnią wartość FAŁSZ. Dzięki temu maszynowego uczenia algorytmów, aby poprawić klasyfikacji podobnych zdarzeń w przyszłości. Stan zdarzenia dodatnią wartość FAŁSZ jest **zamknięte** i nie wpływają one ryzyka użytkownika.

- **Ignoruj** — Jeśli nie miały żadnych działań naprawy, ale chcesz zdarzenia ryzyka ma zostać usunięty z listą aktywną, możesz oznaczyć zdarzenia ryzyka Ignoruj i stan zdarzenia zostanie zamknięte. Ignorowane zdarzeń nie mogą współtworzyć ryzyka użytkownika. Tej opcji można używać tylko w przypadku nietypowego. 

- **Ponowne aktywowanie** - zdarzenia ryzyka, które zostały ręcznie zamknięte (przez wybranie pozycji **rozwiązać**, **wynik fałszywie dodatni**lub **Ignoruj**) można ponownie uaktywnić, ustawianie stanu zdarzenia na **aktywny**. Zdarzenia ryzyka uaktywnionej przyczynić się do obliczenia poziomu ryzyka użytkownika. Nie można ponownie uaktywnić zdarzenia ryzyka zamknięty za pośrednictwem naprawy (takie jak resetowania hasła bezpiecznego). 




**Aby otworzyć okno dialogowe elementami konfiguracji**:

1. Karta **ochronę Azure AD tożsamości** w obszarze **Zbadaj**polecenie **zdarzenia ryzyka**.

    ![Do resetowania hasła ręcznego] (./media/active-directory-identityprotection/1002.png "Do resetowania hasła ręcznego")

2. Na liście **zdarzenia ryzyka** kliknij ryzyka.

    ![Do resetowania hasła ręcznego] (./media/active-directory-identityprotection/1003.png "Do resetowania hasła ręcznego")

2. Na karta ryzyka kliknij prawym przyciskiem myszy użytkownika.

    ![Do resetowania hasła ręcznego] (./media/active-directory-identityprotection/1004.png "Do resetowania hasła ręcznego")



### <a name="closing-all-risk-events-for-a-user-manually"></a>Ręczne zamykanie wszystkie zdarzenia ryzyka dla użytkownika

Zamiast ręcznie pojedynczo zamykając zdarzenia ryzyka dla użytkownika, Azure Active Directory ochronę tożsamości również oferuje metody, aby zamknąć wszystkie zdarzenia dla użytkownika za pomocą jednego kliknięcia.


![Akcje] (./media/active-directory-identityprotection/2222.png "Akcje")

Po kliknięciu przycisku **Odrzuć wszystkie zdarzenia**wszystkie zdarzenia są zamknięte i danego użytkownika nie jest już na ryzyko.



## <a name="remediating-user-risk-events"></a>Korygując zdarzenia ryzyka użytkownika

Rozwiązywanie problemu jest akcją, aby zabezpieczyć tożsamości lub urządzenie, które poprzednio były podejrzane lub wiadomo, że zostać złamane. Akcja naprawy przywraca tożsamości lub urządzenia to bezpieczne i usuwa poprzednie zdarzenia ryzyka skojarzone z tożsamości lub urządzenia.

Aby korygowania zdarzenia ryzyka użytkownika, można wykonywać następujące czynności:

- Wykonywanie bezpieczne hasło przywrócenie ręcznie korygowania zdarzenia ryzyka użytkownika 

- Konfigurowanie zasad zabezpieczeń ryzyka użytkownika dla łagodzenia bądź automatycznego korygowania zdarzenia ryzyka użytkownika

- Ponownie utworzyć obraz zainfekowanego urządzenia  


### <a name="manual-secure-password-reset"></a>Resetowanie hasła bezpiecznego ręcznego

Resetowanie hasła bezpiecznego jest skuteczne naprawy w przypadku wielu zdarzeń ryzyka i podczas automatycznie zamyka te zdarzenia ryzyka i ich ponowne obliczanie ryzyka na poziomie użytkownika. Pulpit nawigacyjny ochronę tożsamości umożliwia inicjować resetowania hasła dla użytkownika ryzykowna. 

Okno dialogowe powiązanych udostępnia dwa sposoby, aby zresetować hasło:

**Resetowanie hasła** — wybierz pozycję **Wymagaj, aby zresetować hasło użytkownik** pozwala użytkownikom na własny odzyskać, gdy użytkownik został zarejestrowany uwierzytelniania wieloskładnikowego. Podczas jego następnego logowania użytkownik będzie wymagane pomyślnie rozwiązać wezwanie uwierzytelnianie wieloskładnikowe i następnie wymuszone Zmienianie hasła. Ta opcja jest niedostępna, jeśli konto użytkownika nie jest już zarejestrowane uwierzytelnianie wieloskładnikowe.

**Hasło tymczasowe** — wybierz pozycję **Generuj hasło tymczasowe** natychmiast powodować dotychczasowe hasło i Utwórz nowe hasło tymczasowe dla użytkownika. Wyślij nowe hasło tymczasowe do alternatywny adres e-mail dla użytkownika lub jego menedżerem. Ponieważ hasło tymczasowe, użytkownika zostanie wyświetlony monit, aby zmienić hasło podczas logowania.


![Zasady] (./media/active-directory-identityprotection/1005.png "Zasady")


**Aby otworzyć okno dialogowe elementami konfiguracji**:

1. Karta **ochronę Azure AD tożsamości** polecenie **użytkowników flagą ryzyka**.

    ![Do resetowania hasła ręcznego] (./media/active-directory-identityprotection/1006.png "Do resetowania hasła ręcznego")


2. Z listy użytkowników wybierz użytkownika z wydarzeniami co najmniej jeden ryzyka.

    ![Do resetowania hasła ręcznego] (./media/active-directory-identityprotection/1007.png "Do resetowania hasła ręcznego")


2. Karta użytkownika wybierz polecenie **Resetuj hasło**.

    ![Do resetowania hasła ręcznego] (./media/active-directory-identityprotection/1008.png "Do resetowania hasła ręcznego")





## <a name="user-risk-security-policy"></a>Zasady zabezpieczeń ryzyka użytkownika

Zasady zabezpieczeń ryzyka użytkownika jest zasadę dostępu warunkowego, która oblicza poziom ryzyka do określonego użytkownika i stosuje akcje naprawy i łagodzenia na podstawie wstępnie zdefiniowanego warunki i reguły.


![Zasady ridk użytkownika] (./media/active-directory-identityprotection/1009.png "Zasady ridk użytkownika")


Ochrona Azure AD tożsamości ułatwia zarządzanie łagodzenia i korygowanie flagą ryzyka, umożliwiając użytkownikom:

- Ustawianie użytkowników i grup, do których zasady dotyczy: 

    ![Zasady ridk użytkownika] (./media/active-directory-identityprotection/1010.png "Zasady ridk użytkownika")


- Ustaw użytkownika ryzyka poziomu progu (niski, średni lub wysoki) wyzwalające zasady: 

    ![Zasady ridk użytkownika] (./media/active-directory-identityprotection/1011.png "Zasady ridk użytkownika")


- Ustaw opcje nastąpiło, gdy wyzwalane zasad:

    ![Zasady ridk użytkownika] (./media/active-directory-identityprotection/1012.png "Zasady ridk użytkownika")


- Przełączanie stanu zasad:

    ![Zasady ridk użytkownika] (./media/active-directory-identityprotection/403.png "Rejestracja MFA")


- Przejrzyj i oceny wpływu zmiany przed aktywacji:

    ![Zasady ridk użytkownika] (./media/active-directory-identityprotection/1013.png "Zasady ridk użytkownika")


Wybieranie **progowa** ogranicza liczbę powtórzeń zasady zostanie wywołana i minimalizuje wpływ na użytkowników.
Jednak nie obejmuje użytkowników **niskiego** i **średniego** flagą ryzyka z zasady, które nie mogą zapewnić tożsamości lub urządzenia, które zostały wcześniej podejrzanych lub wiadomo, że zostać złamane.

Podczas ustawiania zasady

- Wykluczanie użytkowników, którzy mogą wygenerować wiele wyniki FAŁSZ — dodatnie (deweloperów, analityków zabezpieczeń)

- Wykluczanie użytkowników w regionach, gdzie Włączanie zasady nie jest praktyczne (na przykład brak dostępu do działu pomocy technicznej)

- Korzystanie z wdrożeniem **progowa podczas początkowej zasad** lub jeśli musisz zminimalizować problemach widoczne dla użytkowników końcowych.

- Jeśli Twoja organizacja wymaga większe bezpieczeństwo za pomocą progu **niskiego** . Wybieranie progu **niskiego** wprowadza dodatkowe użytkownika logowania problemach, ale większe bezpieczeństwo.

Zalecane ustawienia domyślne dla większości organizacji należy skonfigurować reguły dla próg **Średni** zrozumiałe użyteczności i zabezpieczenia.

Zawiera omówienie dotyczące obsługi użytkownikowi zobacz:

- [Przepływ odzyskiwania konta Compromised](active-directory-identityprotection-flows.md#compromised-account-recovery).  

- [Konto Compromised zablokowane przepływu](active-directory-identityprotection-flows.md#compromised-account-blocked).  


**Aby otworzyć okno dialogowe elementami konfiguracji**:

1. Wybierz polecenie karta **ochronę Azure AD tożsamości** , w sekcji **Konfigurowanie** **zasad ryzyka użytkownika**.

    ![Zasady ridk użytkownika] (./media/active-directory-identityprotection/1009.png "Zasady ridk użytkownika")






## <a name="mitigating-user-risk-events"></a>Łagodzenia zdarzenia ryzyka użytkownika
Administratorzy mogą ustawić zasady zabezpieczeń ryzyka użytkownika użytkownikom bloku na logowania w zależności od poziomu ryzyka. 

Blokowanie logowania:
 
- Zapobiega tworzenia nowego zdarzenia ryzyka użytkownika dla danego użytkownika

- Umożliwia administratorom ręcznie korygowania zdarzenia ryzyka wpływu tożsamość użytkownika i przywrócenie bezpieczny stan



## <a name="what-is-a-sign-in-risk-level"></a>Co to jest poziom ryzyka logowania?

Poziom ryzyka logowania jest wskazanie (wysoki, średni lub niski) prawdopodobieństwo, że dla określonych logowania, ktoś próbuje uwierzytelnienia przy użyciu tożsamości użytkownika. Poziom ryzyka logowania jest obliczane w momencie logowania i są uwzględnione zdarzenia ryzyka i wykryty wskaźników dla tego określonego logowania w czasie rzeczywistym. 

## <a name="mitigating-sign-in-risk-events"></a>Łagodzenia zdarzenia logowania ryzyka 
Łagodzenia jest akcją, aby ograniczyć możliwość atakującej wykorzystać tożsamości złamany lub urządzenia bez przywrócenia tożsamości lub urządzenia to bezpieczne. Łagodzenia nie rozpoznaje poprzednie zdarzenia logowania ryzyka skojarzonego z tożsamości lub urządzenia.

Za pomocą dostępu warunkowego ochronę Azure AD tożsamości automatycznie zmniejszeniu zdarzenia logowania ryzyka. Korzystając z tych zasad, warto rozważyć poziom ryzyka użytkownika lub logowania o blokowaniu ryzykowna rejestrowaniu lub użytkownik przeprowadzić uwierzytelnianie wieloskładnikowe. Te czynności mogą uniemożliwić wykorzystanie kradzież tożsamości spowodowało uszkodzenia oraz może odczekaj pewien czas, aby zabezpieczyć tożsamości. 


## <a name="sign-in-risk-security-policy"></a>Zasady zabezpieczeń logowania ryzyka

Zasady logowania ryzyka jest zasadę dostępu warunkowego, która oblicza czynnik ryzyka określonych logowania i dotyczy czynniki na podstawie wstępnie zdefiniowanego warunki i reguły.

![Logowanie zasad ryzyka] (./media/active-directory-identityprotection/1014.png "Logowanie zasad ryzyka")


Ochrona Azure AD tożsamości ułatwia zarządzanie łagodzenia ryzykowna rejestrowaniu umożliwiając:

- Ustawianie użytkowników i grup, do których zasady dotyczy: 

    ![Logowanie zasad ryzyka] (./media/active-directory-identityprotection/1015.png "Logowanie zasad ryzyka")


- Ustaw logowania ryzyka poziomu progu (niski, średni lub wysoki) wyzwalające zasady: 

    ![Logowania zasad ryzyka] (./media/active-directory-identityprotection/1016.png "Logowania zasad ryzyka")


- Ustawianie kontrolek w celu wymuszane, gdy wyzwalane zasad:  

    ![Logowania zasad ryzyka] (./media/active-directory-identityprotection/1017.png "Logowania zasad ryzyka")
    

- Przełączanie stanu zasad:

    ![Rejestracja MFA] (./media/active-directory-identityprotection/403.png "Rejestracja MFA")

- Przejrzyj i oceny wpływu zmiany przed aktywacji: 

    ![Logowanie zasad ryzyka] (./media/active-directory-identityprotection/1018.png "Logowanie zasad ryzyka")


### <a name="what-you-need-to-know"></a>Co należy wiedzieć

Możesz skonfigurować zasad zabezpieczeń logowania ryzyka wymaga uwierzytelniania wieloskładnikowego:

![Logowanie zasad ryzyka] (./media/active-directory-identityprotection/1017.png "Logowanie zasad ryzyka")

Jednak ze względów bezpieczeństwa to ustawienie działa tylko dla użytkowników, które już zostały zarejestrowane uwierzytelniania wieloskładnikowego. Jeśli jest spełniony warunek wymagać uwierzytelnianie wieloskładnikowe dla użytkownika, który nie został jeszcze zarejestrowany uwierzytelniania wieloskładnikowego, użytkownik jest zablokowany. 

Najlepiej Jeśli chcesz wymusić uwierzytelnianie wieloskładnikowe dla ryzykowna rejestrowaniu, należy:

1. Włącz [uwierzytelnianie wieloskładnikowe rejestracji zasad](#multi-factor-authentication-registration-policy) dla odpowiednich użytkowników.
2. Wymaganie odpowiednich użytkowników, aby logowania w sesji ryzykowna do rejestrowania MFA

Wykonanie tych kroków gwarantuje, że jest wymagane tego uwierzytelnianie wieloskładnikowe dla ryzykowna logowania. 


### <a name="best-practices"></a>Najważniejsze wskazówki

 
Wybieranie **progowa** ogranicza liczbę powtórzeń zasady zostanie wywołana i minimalizuje wpływ na użytkowników.  
 
Jednak nie obejmuje **niski** i **Średni** rejestrowaniu flagą ryzyka z zasady, które nie może blokować atakującej możliwości wykorzystania złamany tożsamości. 

Podczas ustawiania zasady 

- Wykluczanie użytkowników, którzy nie / nie mogą mieć uwierzytelnianie wieloskładnikowe

- Wykluczanie użytkowników w regionach, gdzie Włączanie zasady nie jest praktyczne (na przykład brak dostępu do działu pomocy technicznej)

- Wykluczanie użytkowników, którzy mogą wygenerować wiele wyniki FAŁSZ — dodatnie (deweloperów, analityków zabezpieczeń)

- Korzystanie z wdrożeniem **progowa podczas początkowej zasad** lub jeśli musisz zminimalizować problemach widoczne dla użytkowników końcowych.

- Jeśli Twoja organizacja wymaga większe bezpieczeństwo za pomocą progu **niskiego** . Wybieranie progu **niskiego** wprowadza dodatkowe użytkownika logowania problemach, ale większe bezpieczeństwo.

Zalecane ustawienia domyślne dla większości organizacji należy skonfigurować reguły dla próg **Średni** zrozumiałe użyteczności i zabezpieczenia.

 
Zasady logowania ryzyka są:

- Stosowane do całego ruchu w przeglądarce i dodatki logowania przy użyciu funkcji uwierzytelniania nowoczesny.
- Nie dotyczą aplikacji przy użyciu starszych protokołów zabezpieczeń, wyłączając punkt końcowy WS zaufania w federacyjnych protokołu IDP, takich jak ADFS.

Strona **Zdarzenia ryzyka** w konsoli ochronę tożsamości zawiera listę wszystkich zdarzeń:

- Zastosowano tych zasad
- Można przeglądać działania i określanie, czy akcja była właściwa 

Zawiera omówienie dotyczące obsługi użytkownikowi zobacz:

- [Ryzykowna odzyskiwania logowania](active-directory-identityprotection-flows.md#risky-sign-in-recovery) 

- [Ryzykowna logowania zablokowane](active-directory-identityprotection-flows.md#risky-sign-in-blocked)  

- [Uwierzytelnianie wieloskładnikowe rejestracji podczas ryzykowna logowania](active-directory-identityprotection-flows.md#multi-factor-authentication-registration-during-a-risky-sign-in)  





**Aby otworzyć okno dialogowe elementami konfiguracji**:

1. Wybierz polecenie karta **ochronę Azure AD tożsamości** , w sekcji **Konfigurowanie** **logowania zasady ryzyka**.

    ![Zasady ridk użytkownika] (./media/active-directory-identityprotection/1014.png "Zasady ridk użytkownika")





## <a name="multi-factor-authentication-registration-policy"></a>Uwierzytelnianie wieloskładnikowe zasady rejestracji

Uwierzytelnianie wieloskładnikowe Azure jest metodę weryfikacji tożsamości, która wymaga korzystania z więcej niż tylko nazwę użytkownika i hasło. Zawiera drugą warstwę zabezpieczeń dodatki logowania użytkownika i transakcji.  
Zaleca się Wymagaj Azure uwierzytelnianie wieloskładnikowe dla dodatki logowania użytkownika, ponieważ go:

- Ta aplikacja zapewnia silne uwierzytelnianie z zakresu opcje łatwe weryfikacji

- Odtwarzany kluczową rolę w przygotowaniu organizacji ochrona i odzyskiwanie z kompromisów konta

![Zasady ridk użytkownika] (./media/active-directory-identityprotection/1019.png "Zasady ridk użytkownika")



Aby uzyskać więcej informacji, zobacz [Co to jest Azure uwierzytelnianie wieloskładnikowe?](../multi-factor-authentication/multi-factor-authentication.md)


Ochrona Azure AD tożsamości ułatwia zarządzanie wdrożenie rejestracji uwierzytelnianie wieloskładnikowe przez skonfigurowanie zasady, które umożliwia: 



- Ustawianie użytkowników i grup, do których zasady dotyczy: 

    ![Zasady MFA] (./media/active-directory-identityprotection/1020.png "Zasady MFA")



- Ustaw opcje nastąpiło, gdy zasady wyzwalane::  

    ![Zasady MFA] (./media/active-directory-identityprotection/1021.png "Zasady MFA")


- Przełączanie stanu zasad:

    ![Zasady MFA] (./media/active-directory-identityprotection/403.png "Zasady MFA")

- Widok bieżący stan rejestru: 

    ![Zasady MFA] (./media/active-directory-identityprotection/1022.png "Zasady MFA")


Zawiera omówienie dotyczące obsługi użytkownikowi zobacz:

- [Uwierzytelnianie wieloskładnikowe rejestracji przepływu](active-directory-identityprotection-flows.md#multi-factor-authentication-registration).  

- [Uwierzytelnianie wieloskładnikowe rejestracji podczas ryzykowna logowania](active-directory-identityprotection-flows.md#multi-factor-authentication-registration-during-a-risky-sign-in).  





**Aby otworzyć okno dialogowe elementami konfiguracji**:

1. Wybierz polecenie karta **ochronę Azure AD tożsamości** , w sekcji **Konfigurowanie** **uwierzytelniania wieloskładnikowego rejestracji**.

    ![Zasady MFA] (./media/active-directory-identityprotection/1019.png "Zasady MFA")





## <a name="next-steps"></a>Następne kroki

 - [Kanał 9: Azure AD i Pokaż tożsamości: Podgląd ochrony tożsamości](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)
 - [Typy zdarzeń ryzyka wykryte przez Azure Active Directory ochronę tożsamości](active-directory-identityprotection-risk-events-types.md)
 - [Usterek wykrytych przez Azure Active Directory ochronę tożsamości](active-directory-identityprotection-vulnerabilities.md)
 - [Powiadomienia Azure Active Directory ochronę tożsamości](active-directory-identityprotection-notifications.md)
 - [Azure Active Directory ochronę tożsamości playbook](active-directory-identityprotection-playbook.md)
 - [Azure Active Directory ochronę tożsamości — słownik](active-directory-identityprotection-glossary.md)

 - [Środowiska logowania z ochroną Azure AD tożsamości](active-directory-identityprotection-flows.md)

 - [Włączanie ochrony tożsamości Azure Active Directory](active-directory-identityprotection-enable.md)
 - [Azure Active Directory ochronę tożsamości — jak odblokować użytkowników](active-directory-identityprotection-unblock-howto.md)

 - [Wprowadzenie do programu Microsoft Graph i Azure Active Directory ochronę tożsamości](active-directory-identityprotection-graph-getting-started.md)


