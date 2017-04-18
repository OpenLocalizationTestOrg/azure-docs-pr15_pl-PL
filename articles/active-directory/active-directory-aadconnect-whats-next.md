<properties
    pageTitle="Narzędzie Azure AD Connect: Następne kroki i jak zarządzać narzędzie Azure AD Connect | Microsoft Azure"
    description="Dowiedz się, jak zwiększyć domyślnej konfiguracji i zadań operacyjnych dla Azure AD Connect."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="next-steps-and-how-to-manage-azure-ad-connect"></a>Następne kroki i jak zarządzać narzędzie Azure AD Connect
Następujące czynności są zaawansowane działania tematy, które umożliwiają dostosowanie Azure Active Directory nawiązywanie połączenia z spełnia potrzeb organizacji i wymagań.  

## <a name="add-additional-sync-administrators"></a>Dodawanie administratorów dodatkowe synchronizacji
Domyślnie tylko użytkownik, który został instalacji i Administratorzy będzie mieć możliwość zarządzania aparat zainstalowanych synchronizacji. Dla kolejnych osób można było dostępu i zarządzanie aparatu synchronizacji Znajdź grupę o nazwie ADSyncAdmins na serwerze lokalnym i dodaj je do tej grupy.

## <a name="assigning-licenses-to-azure-ad-premium-and-enterprise-mobility-users"></a>Przypisywanie licencji do użytkowników Azure AD Premium i mobilności przedsiębiorstwa

Teraz, gdy użytkownicy mają zostały zsynchronizowane z chmurą, należy je przypisać licencję, można Pracuj z chmury aplikacji, takich jak usługi Office 365.

### <a name="to-assign-an-azure-ad-premium-or-enterprise-mobility-suite-license"></a>Aby przypisać Azure AD Premium lub licencji pakietu mobilności Enterprise
--------------------------------------------------------------------------------
1. Zaloguj się do portalu Azure jako Administrator.
2. Po lewej stronie wybierz pozycję **Usługi Active Directory**.
3. Na stronie usługi Active Directory kliknij dwukrotnie w katalogu zawierającą użytkowników, dla których chcesz włączyć.
4. W górnej części strony katalogu wybierz pozycję **licencje**.
5. Na stronie licencje wybierz Active Directory Premium lub Enterprise mobilności pakiet Office, a następnie kliknij **przypisać**.
6. W oknie dialogowym Wybierz użytkowników, których chcesz przypisać licencje do, a następnie kliknij ikonę znacznika wyboru, aby zapisać zmiany.


## <a name="verifying-the-scheduled-synchronization-task"></a>Weryfikowanie zadania według harmonogramu synchronizacji
Jeśli chcesz sprawdzić stan synchronizacji możesz to zrobić, zaznaczając pole wyboru w portalu Azure.

### <a name="to-verify-the-scheduled-synchronization-task"></a>Aby sprawdzić zadania według harmonogramu synchronizacji
--------------------------------------------------------------------------------
1. Zaloguj się do portalu Azure jako Administrator.
2. Po lewej stronie wybierz pozycję **Usługi Active Directory**.
3. Na stronie usługi Active Directory kliknij dwukrotnie na katalog, w którym ma użytkowników, które chcesz włączyć.
4. W górnej części strony katalogu wybierz **Integracji katalogów**.
5. W obszarze Integracja z lokalną usługą active directory notatki godzina ostatniej synchronizacji.

<center>![Chmury](./media/active-directory-aadconnect-whats-next/verify.png)</center>

## <a name="starting-a-scheduled-synchronization-task"></a>Uruchamianie zadania według harmonogramu synchronizacji
Jeśli jest potrzebne do uruchamiania zadania synchronizacji możesz to zrobić, uruchamiając z kreatorem Azure AD Connect ponownie.  Konieczne będzie podanie poświadczeń Azure AD.  W kreatorze Wybierz zadanie, **Dostosuj opcje synchronizacji** , a następnie kliknij przycisk Dalej za pomocą kreatora. Na końcu upewnij się, że jest zaznaczone pole **Uruchom proces synchronizacji zaraz po zakończeniu początkowej konfiguracji** .

<center>![Chmury](./media/active-directory-aadconnect-whats-next/startsynch.png)</center>

Aby uzyskać więcej informacji na temat sync Azure AD Connect: harmonogram, zobacz [Azure AD łączenie harmonogram](active-directory-aadconnectsync-feature-scheduler.md)


## <a name="additional-tasks-available-in-azure-ad-connect"></a>Dodatkowe zadania dostępne w narzędzie Azure AD Connect
Po początkowej instalacji: narzędzie Azure AD Connect zawsze kreatora można uruchomić ponownie ze skrótu Azure AD Connect początku strony lub pulpitu.  Można zauważyć, że ponownie przejść za pomocą kreatora zawiera kilka nowych opcji w formularzu zadania dodatkowe.  

Poniższa tabela zawiera podsumowanie tych zadań i krótki opis każdej z nich.

![Dołączanie do reguły](./media/active-directory-aadconnect-whats-next/addtasks.png)


Dodatkowe zadania | Opis
------------- | ------------- |
Wyświetlanie wybranego scenariusza  |Umożliwia wyświetlanie bieżącego rozwiązania Azure AD Connect.  Ta opcja uwzględnia ustawienia ogólne, zsynchronizowane katalogów, ustawienia synchronizacji itp.
Dostosowywanie opcji synchronizacji | Umożliwia zmianę bieżącej konfiguracji, łącznie z dodawaniem dodatkowe lasy usługi Active Directory do konfiguracji lub włączenie opcji synchronizacji, takie jak użytkownika, grupy, urządzenie lub hasło zapisu Wstecz.
Włączanie trybu tymczasowego |  Pozwala to na etapie informacje, które będą później synchronizowane, ale nic się nie zostaną wyeksportowane Azure AD lub usługi Active Directory.  Umożliwia wyświetlanie podglądu synchronizacjami przed występują.

## <a name="next-steps"></a>Następne kroki
Dowiedz się więcej o [integrowaniu lokalnego tożsamości z usługą Azure Active Directory](active-directory-aadconnect.md).
