<properties
    pageTitle="Azure Active Directory ochronę tożsamości playbook | Microsoft Azure"
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
    ms.date="08/22/2016"
    ms.author="markvi"/>

#<a name="azure-active-directory-identity-protection-playbook"></a>Azure Active Directory ochronę tożsamości playbook 

Ten playbook ułatwia:

- Wypełnianie danych w środowisku ochronę tożsamości imitującym zdarzenia ryzyka i usterek
- Konfigurowanie zasad dostępu warunkowego ryzyka i testowanie wpływu tych zasad


## <a name="simulating-risk-events"></a>Symulowanie zdarzenia ryzyka

Tej części podano kroki symulacji następujące typy zdarzeń ryzyka:

- Rejestrowaniu z anonimowych adresów IP (proste)
- Dodatki logowania z nieznanych lokalizacji (umiarkowane)
- Możliwe podróży do lokalizacji nietypowe (trudny)

Inne zdarzenia ryzyka nie symulowany w bezpieczny sposób.


### <a name="sign-ins-from-anonymous-ip-addresses"></a>Adresy IP rejestrowaniu z anonimowe

Ten typ zdarzenia ryzyka służy do identyfikowania użytkowników, którzy mają pomyślna z adresu IP, który wykryto za pomocą adresu IP serwera proxy anonimowe. Te serwery proxy są używane przez osoby, które chcesz ukryć adres IP swoich urządzeń i mogą być używane do złośliwych działań.

W **celu zasymulowania logowania z anonimowych IP, wykonaj następujące czynności**:

1.  Pobierz [Tor przeglądarki](https://www.torproject.org/projects/torbrowser.html.en).
2.  Za pomocą przeglądarki Tor, przejdź do [https://myapps.microsoft.com](https://myapps.microsoft.com).   
3.  Wprowadź poświadczenia konta, które mają być wyświetlane w raporcie **rejestrowaniu z anonimowych adresów IP** .

Rejestrowanie pojawi się na pulpicie nawigacyjnym ochronę tożsamości w ciągu 5 minut. 


###<a name="sign-ins-from-unfamiliar-locations"></a>Dodatki logowania z nieznanych lokalizacji

Ryzyko nieznanych lokalizacje jest mechanizm oceny w czasie rzeczywistym logowania, na który są uwzględnione w przeszłości lokalizacje logowania (IP, szerokości i długości geograficznej i WPW) do określenia lokalizacji nowych / nieznanych. System przechowuje poprzedniego adresy IP, szerokości i długości geograficznej i nich użytkownika i ich znanych lokalizacje. Lokalizacja logowania jest traktowany jako nieznanych, jeśli nie pasuje do istniejącego znanych miejscach lokalizacji logowania.

Ochrona tożsamości Azure Active Directory:  

 - ma okres nauki początkowej 14 dni, w których nie oznaczał nowych lokalizacji w jako nieznanych lokalizacji.
 - ignoruje rejestrowaniu ze znanych urządzeń i lokalizacji, które są geograficznie istniejącej lokalizacji znany.

Aby symulować nieznanych lokalizacji, musisz zalogować się z lokalizacji i urządzenie, które konto ma nie zalogowano z przed. 


W **celu zasymulowania logowania z nieznanych lokalizacji, wykonaj następujące czynności**:

1.  Wybierz konto, które ma co najmniej 14 dni historii logowania. 

2.  Wykonaj dowolną:
    
    . Podczas korzystania z sieci VPN, przejdź do [https://myapps.microsoft.com](https://myapps.microsoft.com) i wprowadź poświadczenia konta, które chcesz zasymulowania zdarzenie ryzyka.

    b. Poproś Skojarz w innej lokalizacji, aby zalogować się przy użyciu poświadczeń konta (niezalecane).

Rejestrowanie pojawi się na pulpicie nawigacyjnym ochronę tożsamości w ciągu 5 minut.
 
### <a name="impossible-travel-to-atypical-location"></a>Możliwe podróży nietypowe lokalizacji
Symulowanie warunek podróży niemożliwe jest trudne, ponieważ używa algorytmu maszynowego uczenia się chwastów się wyniki dodatnie false przykład niemożliwe podróży z urządzeń znanych lub rejestrowaniu z sieci VPN, które są używane przez innych użytkowników w katalogu. Ponadto algorytmu wymaga Historia logowania 3-14 dni dla użytkownika, zanim rozpocznie wygenerowaniem zdarzenia ryzyka.

W **celu zasymulowania niemożliwe podróży nietypowe lokalizacji, wykonaj następujące czynności**:

1.  Za pomocą przeglądarki standardowy, przejdź do [https://myapps.microsoft.com](https://myapps.microsoft.com).  

2.  Wprowadź poświadczenia konta, które chcesz wygenerować zdarzenie ryzyka niemożliwe podróży dla.

3.  Zmienianie swojego agenta użytkownika. Możesz zmienić agenta użytkownika w programie Internet Explorer z narzędzi dla deweloperów lub zmienianie swojego agenta użytkownika w programie Firefox lub Chrome przy użyciu dodatku przełącznik agenta użytkownika.

4.  Zmienianie swojego adresu IP. Możesz zmienić swojego adresu IP przy użyciu sieci VPN, dodatek Tor, lub wirujący się nowym komputerze platformy Azure w centrum danych.

5.  Logowanie do [https://myapps.microsoft.com](https://myapps.microsoft.com) jak poprzednio przy użyciu tych samych poświadczeń, a w kilka minut po poprzedniej logowania.

Rejestrowanie pojawi się na pulpicie nawigacyjnym ochronę tożsamości w 2-4 godzinach.<br>
Ze względu na komputerze złożonych nauki modeli związane jest szansy, których nie ma uzyskać pobierany w górę.<br> Można replikować te kroki dla wielu kont Azure AD.


## <a name="simulating-vulnerabilities"></a>Symulowanie luk 
Luk są słabych w środowisku Azure AD, które mogą być używane przez aktora ciągów bad. Obecnie 3 typy luk są udostępnione w ochronę Azure AD tożsamości, który korzystać z innych funkcji Azure AD. Tych luk będą wyświetlane na pulpicie nawigacyjnym ochronę tożsamości automatycznie po skonfigurowaniu te funkcje.

-   Azure AD [uwierzytelnianie wieloskładnikowe?](../multi-factor-authentication/multi-factor-authentication.md)
-   Azure AD [Odnajdowania aplikacji chmury](active-directory-cloudappdiscovery-whatis.md).
-   Azure AD [Zarządzanie tożsamościami uprzywilejowanych](active-directory-privileged-identity-management-configure.md). 



##<a name="user-compromise-risk"></a>Ryzyko złamania użytkownika

**Aby przetestować ryzyko złamania użytkownika, wykonaj następujące czynności**:

1.  Zaloguj się do [https://portal.azure.com](https://portal.azure.com) przy użyciu poświadczeń administratora globalnego dla dzierżawy.

2.  Przejdź do **ochronę tożsamości**. 

3.  Na głównym karta **ochronę Azure AD tożsamości** kliknij przycisk **Ustawienia**. 

4.  Karta **Ustawienia portalu** w obszarze **Zasady zabezpieczeń**, wybierz polecenie **użytkownika złamanie ryzyka**. 

5.  Na karta **Zaloguj ryzyka** wyłączyć **Włącz regułę** i kliknij przycisk **Zapisz** ustawienia.

6.  Dla danego konta zasymulowania nieznanych lokalizacji lub anonimowego wydarzenia ryzyka adresów IP. To będzie podwyższenie poziomu ryzyka użytkownika dla tego użytkownika do **nośnika**.

7.  Poczekaj kilka minut, a następnie sprawdź, czy na poziomie użytkownika dla użytkownika jest **Średni**.

8.  Przejdź do karta **Ustawienia portalu** .

9.  Karta **Ryzyko złamania użytkownika** , w obszarze, **Włącz regułę**wybierz **w** . 

10. Wybierz jedną z następujących opcji:

    . Aby zablokować, wybierz **Średni** w obszarze **Blokuj Zaloguj się**.

    b. Aby wymusić zmianę bezpiecznego hasła, wybierz **Średni** w obszarze **Wymagaj uwierzytelnianie wieloskładnikowe**.

13. Kliknij przycisk **Zapisz**.

14. Możesz teraz przetestować ryzyka dostępu warunkowego, logując się za pomocą użytkownika o poziomie podwyższonym poziomem ryzyka. Jeśli czynnik ryzyka użytkownika jest średni, w zależności od konfiguracji zasad, do logowania jest albo zablokowania lub wymuszeniem zmienić hasło. 
<br><br>
![Playbook](./media/active-directory-identityprotection-playbook/201.png "Playbook")
<br>

 
##<a name="sign-in-risk"></a>Logowania ryzyka

 
**Aby przetestować znak ryzyko, należy wykonać następujące czynności:**

1.  Zaloguj się do [https://portal.azure.com](https://portal.azure.com) przy użyciu poświadczeń administratora globalnego dla dzierżawy.

2.  Przejdź do **ochrony tożsamości**.

3.  Na głównym karta **ochronę Azure AD tożsamości** kliknij przycisk **Ustawienia**. 

4.  Karta **Ustawienia portalu** w obszarze **Zasady zabezpieczeń**, wybierz polecenie **Zaloguj się ryzyka**.

5.  Karta **Zaloguj ryzyka **wybierz **w** obszarze **Włącz regułę**. 

7.  Wybierz jedną z następujących opcji:

    . Aby zablokować, wybierz **Średni** w obszarze **Zaloguj bloku**

    b. Aby wymusić zmianę bezpiecznego hasła, wybierz **Średni** w obszarze **Wymagaj uwierzytelnianie wieloskładnikowe**.

8.  Aby zablokować, wybierz średni bloku w obszarze logowania.

9.  Aby wymusić uwierzytelnianie wieloskładnikowe, wybierz **Średni** w obszarze **Wymagaj uwierzytelnianie wieloskładnikowe**.

10. Wybierz polecenie **Zapisz**.

11. Możesz teraz przetestować ryzyka dostępu warunkowego symulowanie lokalizacje nieznanych lub anonimowych zdarzenia ryzyka IP, ponieważ są one obu zdarzenia ryzyka **Średni** .

<br>
![Playbook](./media/active-directory-identityprotection-playbook/200.png "Playbook")
<br>


## <a name="see-also"></a>Zobacz też

 - [Ochrona tożsamości Azure Active Directory](active-directory-identityprotection.md)
