<properties
   pageTitle="Uzyskać szczegółowy opis przy użyciu podglądu współpracy Azure Active Directory B2B | Microsoft Azure"
   description="Azure Active Directory B2B współpracy obsługuje relacji między firmy, włączając partnerów biznesowych selektywne dostępu do sieci firmowej aplikacji"
   services="active-directory"
   documentationCenter=""
   authors="viv-liu"
   manager="cliffdi"
   editor=""
   tags=""/>

<tags
   ms.service="active-directory"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="identity"
   ms.date="05/09/2016"
   ms.author="viviali"/>

# <a name="azure-ad-b2b-collaboration-preview-detailed-walkthrough"></a>Podgląd współpracy w usłudze Azure AD B2B: uzyskać szczegółowy opis

Ten instruktaż przedstawia sposób używania Azure AD B2B współpracy. Jako administrator IT firmy Contoso chcemy udostępniać aplikacje pracowników firm trzy partnera. Brak partnera firmy muszą być Azure AD.

- Alicja z organizacji prosty partnera
- Roman z organizacji partnera średni, musi mieć dostęp do zestawu aplikacji
- Aneta z złożonych organizacji partnera, musi mieć dostęp do zestaw aplikacji i członkostwo w grupach informatyczny firmy

Po wysłania zaproszeń do użytkowników partnera, firma Microsoft można skonfigurować je w Azure AD do udzielania dostępu do aplikacji oraz członkostwa grup za pośrednictwem portalu Azure. Zacznijmy od dodania Alicja.

## <a name="adding-alice-to-the-contoso-directory"></a>Dodanie Alicja do katalogu firmy Contoso
1. Tworzyć pliku CSV z nagłówków, jak pokazano, wypełniać tylko Alicji **poczty E-mail**, **DisplayName**i **InviteContactUsUrl**. **DisplayName** to nazwa wyświetlana w zaproszeniu, a także nazwy wyświetlanej w katalogu firmy Contoso Azure AD. **InviteContactUsUrl** udostępnia Alicja kontakt Contoso. W poniższym przykładzie InviteContactUsUrl określa profilu LinkedIn Contoso. Należy pisowni etykiety w pierwszym wierszu pliku CSV dokładnie określone w [odwołaniu formatu pliku CSV](active-directory-b2b-references-csv-file-format.md).  
![Przykładowy plik CSV dla Alicja](./media/active-directory-b2b-detailed-walkthrough/AliceCSV.png)

2. W portalu usługi Azure Dodawanie użytkownika do katalogu firmy Contoso (usługi Active Directory > Contoso > Użytkownicy > Dodaj użytkownika). Na liście "Typ użytkownika" rozwijanej wybierz "Użytkownicy w firmach partnera". Przekaż plik CSV. Upewnij się, że plik CSV jest zamknięte przed przekazaniem.  
![Przekazywanie plików CSV dla Alicja](./media/active-directory-b2b-detailed-walkthrough/AliceUpload.png)

3. Alicja teraz jest reprezentowane jako użytkownika zewnętrznego w katalogu firmy Contoso Azure AD.  
![Alicja znajduje się na Azure AD](./media/active-directory-b2b-detailed-walkthrough/AliceInAD.png)

4. Alicja otrzymuje następujące wiadomości e-mail.  
![Zaproszenie pocztą e-mail Alicja](./media/active-directory-b2b-detailed-walkthrough/AliceEmail.png)

5. Alicja kliknie łącze, a użytkownik zostanie wyświetlony monit, aby zaakceptować zaproszenie i zaloguj się przy użyciu poświadczeń swojej pracy. Jeśli Alicja nie znajduje się w katalogu Azure AD, Alicja jest monit o zalogowanie.  
![Utwórz konto po zaproszenie Alicja](./media/active-directory-b2b-detailed-walkthrough/AliceSignUp.png)

6. Alicja jest przekierowywana do aplikacji programu Access panelu, puste, aż użytkownik ma uprawnienia dostępu do aplikacji.  
![Panel dostępu dla Alicja](./media/active-directory-b2b-detailed-walkthrough/AliceAccessPanel.png)

Ta procedura umożliwia najłatwiejszym formy współpracy B2B. Jako użytkownik w katalogu firmy Contoso Azure AD Alicja można uzyskać dostęp do aplikacji i grup za pośrednictwem portalu Azure. Teraz Dodawanie Roman, który ma mieć dostęp do aplikacji Moodle i usług Salesforce.

## <a name="adding-bob-to-the-contoso-directory-and-granting-access-to-apps"></a>Dodawanie Robert do katalogu firmy Contoso i udzielanie dostępu do aplikacji
1. Użyj programu Windows PowerShell w Azure AD Module zainstalowany Znajdowanie identyfikatorów Moodle aplikacji i usług Salesforce. Identyfikatory mogą być pobierane przy użyciu polecenia cmdlet: `Get-MsolServicePrincipal | fl DisplayName, AppPrincipalId` spowoduje to wyświetlenie listy wszystkich dostępnych aplikacji firmy Contoso i ich AppPrincialIds.  
![Pobierz identyfikatory Robert](./media/active-directory-b2b-detailed-walkthrough/BobPowerShell.png)

2. Utwórz plik CSV zawierający Roberta poczty E-mail i DisplayName, **InviteAppID** **InviteAppResources**i InviteContactUsUrl. Wypełnianie **InviteAppResources** z AppPrincipalIds Moodle i usług Salesforce znaleźć z programu PowerShell, oddzielając je spacją. Wypełnij **InviteAppId** z tym samym AppPrincipalId Moodle brand wiadomości e-mail i zaloguj się stron za pomocą logo Moodle.  
![Przykładowy plik CSV dla niego](./media/active-directory-b2b-detailed-walkthrough/BobCSV.png)

3. Przekaż plik CSV za pośrednictwem portalu Azure, tak jak zostało wykonane dla Alicja. Roman jest teraz użytkowników zewnętrznych w katalogu firmy Contoso Azure AD.

4. Roman otrzymuje następujące wiadomości e-mail.  
![Zaproszenie pocztą e-mail Robert](./media/active-directory-b2b-detailed-walkthrough/BobEmail.png)

5. Roman kliknie łącze i zostanie wyświetlony monit o zaakceptowanie zaproszenia. Po zalogowaniu jest on on są kierowane do Panel dostępu i już można użyć Moodle i usług Salesforce.  
![Panel dostępu dla niego](./media/active-directory-b2b-detailed-walkthrough/BobAccessPanel.png)

Zostanie dodana oznaczone dalej, których potrzebuje dostępu do aplikacji, a także członkostwa w grupach w katalogu firmy Contoso.

## <a name="adding-carol-to-the-contoso-directory-granting-access-to-apps-and-giving-group-membership"></a>Dodawanie oznaczone do katalogu firmy Contoso, udzielanie dostępu do aplikacji i określeniu członkostwa w grupach

1. Za pomocą programu Windows PowerShell moduł Azure AD zainstalowany Znajdowanie identyfikatorów aplikacji i identyfikatory grupy w Contoso.
 - Pobieranie przy użyciu polecenia cmdlet AppPrincipalId `Get-MsolServicePrincipal | fl DisplayName, AppPrincipalId`, to samo jak w przypadku Robert
 - Pobieranie identyfikator obiektu dla grup przy użyciu polecenia cmdlet `Get-MsolGroup | fl DisplayName, ObjectId`. Spowoduje to wyświetlenie listy wszystkich grup w Contoso i ich identyfikatory obiektów. Identyfikatory grup mogą być pobierane także jako identyfikator obiektu na karcie właściwości grupy w portalu Azure.  
![Pobieranie identyfikatorów i grupy dla oznaczone](./media/active-directory-b2b-detailed-walkthrough/CarolPowerShell.png)

2. Utwórz plik CSV, wypełniać skrzynce poczty E-mail, DisplayName InviteAppID, InviteAppResources, **InviteGroupResources**i InviteContactUsUrl. **InviteGroupResources** jest wypełniany przy identyfikatory obiektów grup MyGroup1 i obiektów zewnętrznych, oddzielając je spacją.  
![Przykładowy plik CSV dla oznaczone](./media/active-directory-b2b-detailed-walkthrough/CarolCSV.png)

3. Przekaż plik CSV za pośrednictwem portalu Azure.

4. Oznaczone jest użytkownika w katalogu firmy Contoso i również jest członkiem grupy MyGroup1 i obiektów zewnętrznych, jak pokazano w portalu Azure.  
![Aneta znajduje się w grupie na Azure AD](./media/active-directory-b2b-detailed-walkthrough/CarolGroup.png)

5. Oznaczone, otrzymuje wiadomość e-mail zawierającą łącze do zaakceptować zaproszenie. Gdy użytkownik zaloguje się, Anna jest przekierowywana do panelu aplikacji programu Access mają dostęp do usług Salesforce i Moodle.  

To wszystko jest do dodawania użytkowników z partnera firmy Azure AD B2B współpracę w tym programie. W tym instruktażu pokazano, jak dodawać użytkowników Alicja, Robert i oznaczone przy użyciu trzech plików .csv osobnym katalogu firmy Contoso. Ten proces może łatwiejsze przez kondensacją plików .csv osobnym w jednym pliku.  
![Przykładowy plik CSV Alicja, Robert i oznaczone](./media/active-directory-b2b-detailed-walkthrough/CombinedCSV.png)

## <a name="related-articles"></a>Artykuły pokrewne
Przeglądanie naszych inne artykuły na Azure AD B2B współpracy:

- [Co to jest Azure AD B2B współpracy?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Jak to działa](active-directory-b2b-how-it-works.md)
- [Odwołanie do formatu pliku CSV](active-directory-b2b-references-csv-file-format.md)
- [Format token użytkowników zewnętrznych](active-directory-b2b-references-external-user-token-format.md)
- [Zmiany atrybutów obiektu użytkowników zewnętrznych](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [Ograniczenia w bieżącej wersji preview](active-directory-b2b-current-preview-limitations.md)
- [Artykuł indeks do zarządzania aplikacjami w usłudze Azure Active Directory](active-directory-apps-index.md)
