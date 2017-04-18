<properties
    pageTitle="Azure AD Connect operacje kondycji."
    description="Ten artykuł zawiera opis dodatkowych operacji, które mogą być wykonywane po wdrożeniu Azure AD łączenie kondycji."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>

# <a name="azure-ad-connect-health-operations"></a>Azure AD Connect operacje kondycji

Poniższy temat zawiera opis różnych operacji, które można wykonywać przy użyciu Azure AD łączenie kondycji.

## <a name="enable-email-notifications"></a>Włącz powiadomienia E-mail
Można skonfigurować Azure AD łączenie kondycji usługi dotyczące wysyłania powiadomień e-mail po alerty są generowane wskazuje, że infrastruktury tożsamości nie działa prawidłowo. Taka sytuacja występuje, gdy alert jest generowany, a także podczas została oznaczona jako rozwiązane. Postępuj zgodnie z instrukcjami poniżej, aby skonfigurować powiadomienia e-mail.

![Azure AD Connect E-mail kondycji wykrywania powiadomień](./media/active-directory-aadconnect-health/email_noti_discover.png)

>[AZURE.NOTE] Powiadomienia e-mail są domyślnie wyłączone.


### <a name="to-enable-azure-ad-connect-health-email-notifications"></a>Aby włączyć Azure AD łączenie kondycji powiadomienia E-mail

1. Otwórz karta alerty dla usługi, dla których chcesz otrzymywać powiadomienia pocztą e-mail.
2. Kliknij przycisk "Ustawienia powiadomień" z paska akcji.
3. Wyłącz przełącznik powiadomienia pocztą E-mail włączone.
4. Zaznacz pole wyboru, aby skonfigurować wszystkich administratorów globalnych otrzymywania powiadomień e-mail.
5. Jeśli chcesz otrzymywać powiadomienia e-mail na inne adresy e-mail, można je określić w polu dodatkowego adresata wiadomości E-mail. Aby usunąć adres e-mail z listy, kliknij ją prawym przyciskiem myszy i wybierz polecenie Usuń.
6. Aby zakończyć zmiany kliknij przycisk "Zapisz". Wszystkie zmiany zaczną efektów, tylko po wybraniu opcji "Zapisz".

## <a name="delete-a-server-or-service-instance"></a>Usuń wystąpienie serwera lub usługi

### <a name="delete-a-server-from-azure-ad-connect-health-service"></a>Usuwanie serwera z Azure AD łączenie kondycji usługi
W niektórych przypadkach możesz usunąć serwer z monitorowane. Postępuj zgodnie z instrukcjami poniżej, aby usunąć serwer Azure AD łączenie kondycji usługi.

Podczas usuwania serwer, należy pamiętać o następujących czynności:

- Ta akcja ZATRZYMA zbierania wszelkie dodatkowe dane z tego serwera. Ten serwer zostanie usunięty z monitorowania usługi. Po tej akcji nie będzie można wyświetlać alerty nowej monitorowania lub użycie danych analytics dla tego serwera.
- Ta akcja nie będzie odinstalować lub usunąć Agent kondycji z serwera. Jeśli nie odinstalowano Agent kondycji przed wykonaniem tej czynności, można zobaczyć zdarzenia błędów na serwerze związanych z agentem kondycji.
- Ta akcja nie spowoduje usunięcia danych zebranych już z tego serwera. Dane zostaną usunięte zgodnie z zasadami przechowywania Microsoft Azure danych.
- Po wykonaniu tej akcji, jeśli chcesz uruchomić monitorowania ten sam serwer ponownie, będzie konieczne Odinstaluj i ponownie zainstalować agenta kondycji na tym serwerze.


#### <a name="to-delete-a-server-from-azure-ad-connect-health-service"></a>Aby usunąć serwer Azure AD łączenie kondycja usługi

Azure AD Connect zdrowia dla usług AD FS i Azure AD połączenia (synchronizacja):

1. Otwieranie karta serwera z serwera karta listy, wybierając nazwę serwera, który ma zostać usunięty.
2. Na karta serwera kliknij przycisk "Usuń" na pasku akcji.
3. Potwierdzenie do usunięcia na serwerze, wpisując nazwę serwera w oknie potwierdzenia.
4. Kliknij przycisk "Usuń".

Azure AD Connect kondycji w usługach AD DS:

1. Otwórz na pulpicie nawigacyjnym kontrolerów domeny.
2. Wybierz kontroler domeny do usunięcia.
3. Kliknij przycisk "Usuń wybrane" z paska akcji.
4. Potwierdzenie do usunięcia na serwerze.
5. Kliknij przycisk "Usuń".

### <a name="delete-a-service-instance-from-azure-ad-connect-health-service"></a>Usuń wystąpienie usługi Azure AD łączenie kondycji usługi

W niektórych przypadkach możesz usunąć wystąpienie usługi. Postępuj zgodnie z instrukcjami poniżej, aby usunąć wystąpienie usługi usłudze Azure AD łączenie zdrowia.

Podczas usuwania wystąpienie usługi, należy pamiętać o następujących czynności:

- Ta akcja spowoduje usunięcie w bieżącym wystąpieniu usług w usłudze monitorowania.
- Ta akcja nie będzie odinstalować lub usunąć Agent kondycji z dowolnego z serwerów, które były monitorowane w ramach tego wystąpienia usługi wyszukiwania. Jeśli nie odinstalowano Agent kondycji przed wykonaniem tej czynności, można zobaczyć zdarzenia błędów na serwerach związanych z agentem zdrowia.
- Wszystkie dane z tego wystąpienia usług zostaną usunięte zgodnie z zasadami przechowywania Microsoft Azure danych.
- Po wykonaniu tej akcji, jeśli chcesz rozpocząć monitorowanie usługi, odinstaluj i ponownie zainstalować agenta kondycji na wszystkich serwerach, które będą monitorowane. Po wykonaniu tej akcji, jeśli chcesz uruchomić monitorowania ten sam serwer ponownie, będzie konieczne Odinstaluj i ponownie zainstalować agenta kondycji na tym serwerze.


#### <a name="to-delete-a-service-instance-from-azure-ad-connect-health-service"></a>Aby usunąć wystąpienie usługi Azure AD łączenie kondycji usługi

1. Otwieranie karta usługi z karta listy usług, wybierając identyfikator usług (nazwa farmy), który chcesz usunąć.
2. Na karta serwera kliknij przycisk "Usuń" na pasku akcji.
3. Potwierdź nazwy usługi, wpisując je w oknie potwierdzenia. (na przykład: sts.contoso.com)
4. Kliknij przycisk "Usuń".
<br><br>


[//]: # (Start of RBAC section)
## <a name="manage-access-with-role-based-access-control"></a>Zarządzanie dostępem z oparte na roli kontroli dostępu
### <a name="overview"></a>Omówienie
[Kontrola dostępu oparta roli](role-based-access-control-configure.md) Azure AD łączenie kondycji zapewnia dostęp do usługi Azure AD łączenie kondycji do użytkowników lub grup spoza administratorów globalnych. To uzyskuje się przez przypisywanie ról do grup i\lub docelowi użytkownicy, a także mechanizm ograniczyć administratorów globalnych w katalogu.

#### <a name="roles"></a>Role
Azure AD łączenie kondycji obsługuje następujące role wbudowane.

| Rola | Uprawnienia |
| ----------- | ---------- |
| Właściciel | Właściciele mogą ***zarządzać dostępem*** (np. przypisywanie roli do użytkownika lub grupy), ***Wyświetlanie wszystkich informacji*** (np. Wyświetl alerty) za pomocą portalu i ***zmienić ustawienia*** (np. powiadomienia e-mail) w ramach Azure AD łączenie kondycji. <br>Domyślnie administratorów globalnego Azure AD są przypisani do tej roli i nie można zmienić.  |
|Trybu współautora|  Współautorzy mogą być ***Wyświetlanie wszystkich informacji*** (np. Wyświetl alerty) za pomocą portalu i ***zmienić ustawienia*** (np. powiadomienia e-mail) w ramach Azure AD łączenie kondycji.|
|Czytnik| Czytelnicy mogą ***Wyświetlanie wszystkich informacji*** (np. widoku alerty) z portalu w Azure AD łączenie kondycji.|

Wszystkie inne, czy role (na przykład "Administratorzy dostępu użytkowników" lub "Użytkownicy Labs DevTest"), nawet jeśli są dostępne w portalu środowisko, nie mają wpływu Aby udostępnić w Azure AD łączenie kondycji.

#### <a name="access-scope"></a>Zakres programu Access

Narzędzie Azure AD Connect obsługuje zarządzania programu access na dwóch poziomach:

- ***Wszystkie wystąpienia usługi***: to jest zalecane ścieżkę dla większości klientów i sterowanie dostępem dla wszystkich wystąpień usług (np. farmy ADFS) przez wszystkie typy roli, które są monitorowane Azure AD łączenie kondycji.

- ***Wystąpienie usługi***: W niektórych przypadkach może być konieczne oddzielnie różnic na podstawie typów roli lub przez wystąpienie usługi programu access. W takim przypadku możesz zarządzać dostępem na poziomie wystąpienia usługi.  

Uprawnienia Jeśli użytkownik ma mieć dostęp na poziomie katalogu lub wystąpienia usługi.


### <a name="how-to-allow-users-or-groups-access-to-azure-ad-connect-health"></a>Jak udostępnić użytkownikom lub grupom Azure AD łączenie kondycji
#### <a name="steps-1-select-the-appropriate-access-scope"></a>Kroki od 1: Wybierz zakres odpowiedniego dostępu
Aby umożliwić użytkownikom dostępu na poziomie *wszystkie wystąpienia usług* w Azure AD łączenie kondycji, otwórz głównym karta w Azure AD łączenie kondycji.<br>
#### <a name="step-2-add-users-groups-and-assign-roles"></a>Krok 2: Dodawanie użytkowników, grupy i przypisywanie ról
1. Kliknij w części "Użytkownicy" w sekcji Konfigurowanie.<br>
![Azure AD Connect karta głównym RBAC kondycji](./media/active-directory-aadconnect-health/RBAC_main_blade.png)
2. Wybierz pozycję "Dodaj"
3. Wybierz "Rolę", takie jak "Właściciel"<br>
![Azure AD Connect kondycji RBAC Dodawanie użytkownika](./media/active-directory-aadconnect-health/RBAC_add.png)
4. Wpisz nazwę lub identyfikator wybranych użytkowników lub grup. W tym samym czasie, możesz wybrać jeden lub więcej użytkowników lub grup. Kliknij przycisk "Wybierz".
![Azure AD Connect zdrowia RBAC wybierz użytkownika](./media/active-directory-aadconnect-health/RBAC_select_users.png)
5. Wybierz pozycję "Ok".<br>

6. Po zakończeniu przypisanie roli użytkowników lub grup pojawi się na liście.<br>
![Azure AD Connect listy użytkowników RBAC zdrowia](./media/active-directory-aadconnect-health/RBAC_user_list.png)

Poniższe czynności umożliwi wyświetlonych użytkowników i grupy programu access zgodnie z ich przypisanych ról.
>[AZURE.NOTE]
- Globalne Administratorzy mają zawsze pełny dostęp do wszystkich działań, ale nie są obecne na powyższej liście konta administratora globalnego.
- Funkcja "Zapraszania użytkowników" nie jest obsługiwana w Azure AD łączenie zdrowia.

#### <a name="step-3-share-the-blade-location-with-users-or-groups"></a>Krok 3: Udostępnianie lokalizacji karta użytkowników lub grup
1. Po przypisaniu uprawnień, użytkownik może uzyskać dostęp Azure AD łączenie kondycji, przechodząc do [http://aka.ms/aadconnecthealth](http://aka.ms/aadconnecthealth).
2. Raz na karta, użytkownik przypięcie karta lub innej części do pulpitu nawigacyjnego, po prostu klikając pozycję "Przypnij do pulpitu nawigacyjnego"<br>
![Azure AD łączenie kondycji RBAC numeru pin karta](./media/active-directory-aadconnect-health/RBAC_pin_blade.png)


>[AZURE.NOTE] Użytkownik z roli "Czytnika" przypisane nie będzie operacji "Tworzenie", aby uzyskać rozszerzenia Azure AD kondycji nawiązywanie połączenia z usługi Azure Marketplace. Ten użytkownik nadal mają dostęp do karta, przechodząc do tego łącza. Do późniejszego użycia użytkownik może przypiąć karta do pulpitu nawigacyjnego.

### <a name="remove-users-andor-groups"></a>Usuwanie użytkowników lub grup
Możesz usunąć użytkownika lub grupę dodana do części Azure AD łączenie zdrowia rolę podstawie kontrola dostępu przez kliknięcie prawym przyciskiem myszy i wybierając pozycję Usuń.<br>
![Azure AD Connect zdrowia RBAC Usuń użytkownika](./media/active-directory-aadconnect-health/RBAC_remove.png)

[//]: # (End of RBAC section)

## <a name="related-links"></a>Łącza pokrewne

* [Azure AD Connect kondycji](active-directory-aadconnect-health.md)
* [Azure AD Connect instalacji agenta kondycji](active-directory-aadconnect-health-agent-install.md)
* [Podłącz za pomocą Azure AD kondycji z usług AD FS](active-directory-aadconnect-health-adfs.md)
* [Przy użyciu Azure AD łączenie kondycji dla synchronizacji](active-directory-aadconnect-health-sync.md)
* [Za pomocą Azure AD nawiązywanie połączenia z usługami AD DS kondycji](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect kondycji — często zadawane pytania](active-directory-aadconnect-health-faq.md)
* [Azure AD Connect kondycji Historia wersji](active-directory-aadconnect-health-version-history.md)
