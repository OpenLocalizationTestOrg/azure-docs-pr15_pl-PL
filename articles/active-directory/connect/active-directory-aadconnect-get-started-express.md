<properties
    pageTitle="Narzędzie Azure AD Connect: Rozpoczęcie pracy przy użyciu ustawień express | Microsoft Azure"
    description="Dowiedz się, jak pobrać, zainstalować i uruchomić Kreatora konfiguracji dla Azure AD Connect."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/13/2016"
    ms.author="billmath"/>

# <a name="getting-started-with-azure-ad-connect-using-express-settings"></a>Wprowadzenie do programu narzędzie Azure AD Connect przy użyciu ustawień express
Azure AD Connect **Ustawienia ekspresowe** jest używany jeden las topologii i [synchronizacji haseł](../active-directory-aadconnectsync-implement-password-synchronization.md) dla uwierzytelniania. **Ustawienia ekspresowe** jest domyślna opcja i jest używany do najczęściej wdrożonym scenariusza. Są tylko kilka kliknięć krótki z dala od komputera, aby rozszerzyć katalogu lokalnego w chmurze.

Przed rozpoczęciem instalacji narzędzie Azure AD Connect, upewnij się, aby [pobrać narzędzie Azure AD Connect](http://go.microsoft.com/fwlink/?LinkId=615771) i wykonano kroki warunki wstępne [Narzędzie Azure AD Connect: sprzęt i wymagania wstępne dotyczące](../active-directory-aadconnect-prerequisites.md).

Jeśli ustawienia ekspresowe nie ma odpowiednika topologii, zobacz [dokumentacji powiązanych](#related-documentation) innych sytuacjach.

## <a name="express-installation-of-azure-ad-connect"></a>Instalacja ekspresowa Azure AD Connect
Można zobaczyć poniższe czynności w działaniu w sekcji [klipy wideo](#videos) .

1. Zaloguj się jako administrator lokalny na serwerze, który chcesz zainstalować narzędzie Azure AD Connect na. Należy to zrobić na serwerze, który ma być serwera synchronizacji.
2. Przejdź do, a następnie kliknij dwukrotnie **AzureADConnect.msi**.
3. Na ekranie powitalnym zaznacz pole zgodę postanowienia licencyjne i kliknij przycisk **Kontynuuj**.  
4. Na ekranie Ustawienia Express kliknij opcję **Użyj ustawień express**.  
![Łączenie Azure AD — Zapraszamy!](./media/active-directory-aadconnect-get-started-express/express.png)
5. Na stronie Połącz Azure AD ekranu wprowadź nazwę użytkownika i hasło administratora globalnego usługi Azure AD. Kliknij przycisk **Dalej**.  
![Nawiązywanie połączenia z Azure AD](./media/active-directory-aadconnect-get-started-express/connectaad.png) Jeśli komunikat o błędzie i występują problemy z łącznością, zobacz [Rozwiązywanie problemów z łącznością](../active-directory-aadconnect-troubleshoot-connectivity.md).
6. Na stronie Połącz ekranu usług AD DS wprowadź nazwę użytkownika i hasło dla konta administratora organizacji. Możesz wprowadzić domeny w formacie NetBios lub nazwy FQDN, oznacza to, że FABRIKAM\administrator lub fabrikam.com\administrator. Kliknij przycisk **Dalej**.  
![Nawiązywanie połączenia z usług AD DS](./media/active-directory-aadconnect-get-started-express/connectad.png)
7. Strona [**Azure AD logowania konfiguracji**](../active-directory-aadconnect-user-signin.md#azure-ad-sign-in-configuration) zawiera tylko, jeśli nie zostaną wykonane w [wymagania wstępne dotyczące](../active-directory-aadconnect-prerequisites.md) [Weryfikowanie domeny](../active-directory-add-domain.md) .
![Niezweryfikowany domen](./media/active-directory-aadconnect-get-started-express/unverifieddomain.png)  
Jeśli widzisz tę stronę, następnie przejrzeć każdą domeną oznaczony **Nie zostały dodane** i **Nie został zweryfikowany**. Upewnij się, że w Azure AD zweryfikowane tych domen, którego używasz. Po zweryfikowaniu domeny usługi, kliknij symbol odświeżania.
8. Na stronie gotowy do skonfigurowania ekranu, kliknij przycisk **Zainstaluj**.
    - Opcjonalnie na karcie Gotowe, aby skonfigurować stronę, możesz usunąć zaznaczenie pola wyboru **Uruchom proces synchronizacji zaraz po zakończeniu konfiguracji** . Należy zaznaczenie tego pola wyboru, aby dodatkowej konfiguracji, takich jak [Filtrowanie](../active-directory-aadconnectsync-configure-filtering.md). Możesz usunąć zaznaczenie tej opcji, Kreator konfiguruje synchronizacji, ale pozostaną harmonogram wyłączona. Nie działa, dopóki nie zostanie włączona ręcznie ponownie [Kreatora instalacji](../active-directory-aadconnectsync-installation-wizard.md).
    - Mając programu Exchange w usłudze Active Directory w lokalnej możesz również wybrać opcję, aby włączyć [**wdrożenia hybrydowego programu Exchange**](https://technet.microsoft.com/library/jj200581.aspx). Włącz tę opcję, jeśli użytkownik planuje skrzynki pocztowe programu Exchange, zarówno w chmurze i lokalnych w tym samym czasie.
![Aby skonfigurować narzędzie Azure AD Connect](./media/active-directory-aadconnect-get-started-express/readytoconfigure.png)
9. Po zakończeniu instalacji, kliknij przycisk **Zakończ**.
10. Po zakończeniu instalacji Zaloguj i zaloguj się ponownie, zanim użyjesz Menedżerze usługi synchronizacji lub Edytor reguł synchronizacji.

## <a name="videos"></a>Klipy wideo

Dla klipu wideo na temat express instalacji zobacz:

>[AZURE.VIDEO azure-active-directory-connect-express-settings]

## <a name="next-steps"></a>Następne kroki
Teraz, gdy masz Azure AD Connect możesz [sprawdzić poprawność instalacji i przypisywanie licencji](../active-directory-aadconnect-whats-next.md).

Dowiedz się więcej o tych funkcjach, które zostały włączone z instalacją: [Uaktualnianie](../active-directory-aadconnect-feature-automatic-upgrade.md), [Zapobiegaj przypadkowym usunięciom](../active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)i [Azure AD łączenie kondycji](../active-directory-aadconnect-health-sync.md).

Dowiedz się więcej o tych typowych tematów: [Harmonogram i jak wyzwalanie synchronizacji](../active-directory-aadconnectsync-feature-scheduler.md).

Dowiedz się więcej o [integrowaniu lokalnego tożsamości z usługą Azure Active Directory](../active-directory-aadconnect.md).

## <a name="related-documentation"></a>Dokumentacja pokrewne

Temat |  
--------- | ---------
Omówienie Azure AD Connect | [Integrowanie tożsamości lokalnych z usługi Azure Active Directory](../active-directory-aadconnect.md)
Instalowanie przy użyciu ustawień niestandardowych | [Instalacja niestandardowa: narzędzie Azure AD Connect](active-directory-aadconnect-get-started-custom.md)
Uaktualnienie DirSync | [Uaktualnienie narzędzie do synchronizacji usługi Azure AD (DirSync)](active-directory-aadconnect-dirsync-upgrade-get-started.md)
Konta używane do instalacji | [Więcej informacji na temat Azure AD Connect kontach i uprawnieniach](active-directory-aadconnect-accounts-permissions.md)
