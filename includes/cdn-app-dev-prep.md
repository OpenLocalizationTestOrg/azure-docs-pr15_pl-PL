## <a name="prerequisites"></a>Wymagania wstępne

Przed możemy można wpisać kod zarządzania CDN, trzeba wykonać kilka przygotowanie do włączenia naszych kodu wykonać czynność związaną z Menedżerem zasobów Azure.  Aby to zrobić, musisz:

* Tworzenie grupy zasobów zawiera profil CDN utworzone w tym samouczku
* Konfigurowanie usługi Azure Active Directory do uwierzytelniania naszych aplikacji
* Dodawanie uprawnień do grupy zasobów, tak aby tylko autoryzowani użytkownicy z naszych dzierżawy Azure AD można wchodzić w interakcje z naszych profilu sieci CDN

### <a name="creating-the-resource-group"></a>Tworzenie grupy zasobów

1. Logowanie do [portalu Azure](https://portal.azure.com).

2. Kliknij przycisk **Nowy** w lewym górnym rogu, a następnie **zarządzania**i **Grupa zasobów**.
    
    ![Tworzenie nowej grupy zasobów](./media/cdn-app-dev-prep/cdn-new-rg-1-include.png)

3. Zadzwoń do swojej grupy zasobów *CdnConsoleTutorial*.  Wybierz subskrypcję i wybierz lokalizację, w której najbliższym.  Jeśli chcesz, możesz kliknąć pozycję **Przypnij do pulpitu nawigacyjnego** pole wyboru, aby przypiąć grupa zasobów do pulpitu nawigacyjnego w portalu.  Spowoduje to ułatwiają znajdowanie w przyszłości.  Po dokonaniu wyboru, kliknij przycisk **Utwórz**.

    ![Nadawanie nazw grup zasobów](./media/cdn-app-dev-prep/cdn-new-rg-2-include.png)

4. Po utworzeniu grupy zasobów, jeśli nie przypiąć go do pulpitu nawigacyjnego, możesz znaleźć go, klikając pozycję **Wyszukaj**, a następnie **Grup zasobów**.  Kliknij pozycję Grupa zasobów, aby go otworzyć.  Zanotuj swój **Identyfikator subskrypcji**.  Będą potrzebne później.

    ![Nadawanie nazw grup zasobów](./media/cdn-app-dev-prep/cdn-subscription-id-include.png)

### <a name="creating-the-azure-ad-application-and-applying-permissions"></a>Tworzenie aplikacji Azure AD i stosowania uprawnień

Istnieją dwie metody uwierzytelniania aplikacji z usługą Azure Active Directory: poszczególnym użytkownikom lub wystawcy usługi. Głównej usługi jest podobne do konta usługi w systemie Windows.  Zamiast udzielanie uprawnień interakcję profile CDN określonego użytkownika, zamiast tego użytkownikowi uprawnienia do głównej usługi.  Główne usługi używa się zasadniczo do automatycznego, interakcyjnych procesów.  Mimo że ten samouczek zapisuje aplikacji konsoli interakcyjnych, poświęconym ujęciu głównej usługi.

Tworzenie wystawcy usługi składa się z kilku kroków, w tym tworzenie aplikacji usługi Azure Active Directory.  Aby to zrobić, możemy zacząć [skorzystać z tego samouczka](../articles/resource-group-create-service-principal-portal.md).

> [AZURE.IMPORTANT] Pamiętaj o postępuj zgodnie z instrukcjami [samouczka połączone](../articles/resource-group-create-service-principal-portal.md).  Jest *bardzo ważne* zakończyć ją dokładnie zgodnie z opisem.  Należy zwrócić uwagę swój **identyfikator dzierżawy**, **nazwy domeny dzierżawy** (często *. onmicrosoft.com* domeny przed określeniem domenę niestandardową), **identyfikator klienta**i **kluczy uwierzytelniania klienta**, jak są wymagane te później.  Uważaj bardzo zabezpieczenia swój **identyfikator klienta** i **kluczy uwierzytelniania klienta**, jak te poświadczenia może być użyty przez każdą osobę na wykonywanie operacji wystawcy usługi. 
>   
> Po przejściu do kroku o nazwie [Konfiguruj aplikację wielu dzierżawy](../articles/resource-group-create-service-principal-portal.md#configure-multi-tenant-application), wybierz opcję **nie**.
> 
> Po przejściu do kroku [Przypisywanie aplikacji do roli](../articles/resource-group-create-service-principal-portal.md#assign-application-to-role)grupa zasobów, możemy utworzony wcześniej, *CdnConsoleTutorial*za pomocą, ale zamiast roli **czytnika** przypisanie roli **Współautora profilu CDN** .  Po przypisaniu aplikacji rola **Współautora profilu CDN** na swojej grupy zasobów, wróć do tego samouczka. 

Po utworzeniu tej usługi głównych i przypisana rola **Współautora profilu CDN** , karta **użytkowników** z grupy zasobów powinna wyglądać podobnie do następującej.

![Karta użytkowników](./media/cdn-app-dev-prep/cdn-service-principal-include.png)


### <a name="interactive-user-authentication"></a>Uwierzytelnianie użytkownika interakcyjnego

Jeśli zamiast podmiot usługi wolisz, aby były uwierzytelniania interakcyjnych poszczególnych użytkowników, proces jest bardzo podobny do wystawcy usługi.  W rzeczywistości będzie konieczne postępuj zgodnie z procedurą, ale wprowadzić kilka zmian pomocnicze.

> [AZURE.IMPORTANT] Tylko wykonaj poniższe czynności, wybór uwierzytelniania użytkownika zamiast głównej usługi.

1. Podczas tworzenia aplikacji, a nie **Aplikacji sieci Web**, wybierz pozycję **natywnej aplikacji**. 
    
    ![Natywnej aplikacji](./media/cdn-app-dev-prep/cdn-native-application-include.png)
    
2. Na następnej stronie zostanie wyświetlony monit dla **przekierowania URI**.  Identyfikator URI nie można sprawdzić poprawności, ale pamiętaj, że wprowadzono.  Musisz go później. 

3. Istnieje bez konieczności tworzenia **kluczy uwierzytelniania klienta**.

4. Zamiast przypisywania wystawcy usługi do roli **Współautora profilu CDN** , możemy zacząć przypisywać poszczególnym użytkownikom lub grupom.  W tym przykładzie widać, że *CDN pokaz użytkownika* zostały przypisane do roli **Współautora profilu CDN** .  
    
    ![Dostępu poszczególnych użytkowników](./media/cdn-app-dev-prep/cdn-aad-user-include.png)

