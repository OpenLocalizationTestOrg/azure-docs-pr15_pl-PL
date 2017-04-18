<properties
    pageTitle="Rozwiązywanie problemów, usługi Azure Active Directory access | Microsoft Azure"
    description="Dowiedz się, czynności, które można wykonać, aby rozwiązać problemy programu access z zasobów online firmy."
    services="active-directory"
    keywords="opartych na urządzeniach dostępu warunkowego, rejestracji urządzenia, Włącz rejestracji urządzenia, Rejestracja urządzenia i MDM"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/23/2016"
    ms.author="markvi"/>


# <a name="troubleshooting-for-azure-active-directory-access-issues"></a>Rozwiązywanie problemów dostępu usługi Azure Active Directory

Próby uzyskania dostępu do intranetu usługi SharePoint Online i zostanie wyświetlony komunikat o błędzie "odmowa dostępu". Co porabiasz?

W tym artykule opisano rozwiązywanie problemu czynności, które mogą ułatwić rozwiązywanie problemów z programu access z zasobów online firmy.

Pomocy w rozwiązaniu usługi Azure Active Directory (Azure AD) uzyskać dostęp do problemów, przejdź do sekcji w artykule opisano, jak posiadanej platformy urządzenia:

-   Urządzenie z systemem Windows
-   urządzenia z systemem iOS (sprawdzanie wstecz szybko uzyskać pomoc dotyczącą tablety Ipad i telefony iPhone.)
-   Urządzenie z systemem android (zaglądaj tu wkrótce Aby uzyskać pomoc dotyczącą Android telefonach i tabletach.)

## <a name="access-from-a-windows-device"></a>Dostęp za pomocą urządzeniu z systemem Windows

Jeśli urządzenie działa jednej z następujących platform, sprawdź w kolejnych sekcjach dla komunikat o błędzie, który jest wyświetlany podczas próby uzyskać dostęp do aplikacji lub usługi:

- Windows 10
- System Windows 8.1
- Windows 8
- Windows 7
- System Windows Server 2016
- Windows Server 2012 R2
- System Windows Server 2012
- Windows Server 2008 R2

### <a name="device-is-not-registered"></a>Urządzenie nie jest zarejestrowany.

Jeśli urządzenie nie jest zarejestrowany z usługą Azure Active Directory, a aplikacja jest chronione za pomocą zasad opartych na urządzeniach, może zostać wyświetlony strona zawierająca jeden z następujących komunikatów o błędzie:

!["Nie otrzymujesz dostępne w tym miejscu" wiadomości dla urządzeń wyrejestrowany] (./media/active-directory-conditional-access-device-remediation/01.png "Scenariusz")

Jeśli urządzenie jest domeny — dołączony do usługi Active Directory w Twojej organizacji, zrób tak:

1.  Upewnij się, że możesz zalogować się do systemu Windows przy użyciu konta programu pracy (konta usługi Active Directory).
2.  Nawiązywanie połączenia z siecią firmową za pośrednictwem wirtualną sieć prywatną (VPN) ani DirectAccess.
3.  Po połączeniu, naciśnij klawisze logo Windows + klawisz L, aby zablokować sesji systemu Windows.
4.  Wprowadź poświadczenia konta pracy, aby odblokować sesji systemu Windows.
5.  Poczekaj chwilę i spróbuj ponownie uzyskać dostęp do aplikacji lub usługi.
6.  Jeśli widzisz tej samej stronie, kliknij łącze **więcej szczegółów** , a następnie skontaktuj się z administratorem ze szczegółami.

Jeśli urządzenie jest domeny i uruchamiania systemu Windows 10, dostępne są dwie opcje:

- Uruchamianie Azure AD sprzężenia
- Dodawanie konta służbowego do systemu Windows

Aby dowiedzieć się, jak czym różnią się tych opcji zobacz [urządzenia za pomocą systemu Windows 10 w miejscu pracy](active-directory-azureadjoin-windows10-devices.md).

Aby uruchomić, Azure AD dołączanie, wykonaj następujące czynności na platformie urządzenie działa na. (Azure AD sprzężenia nie jest dostępna na telefonach Windows).

**Windows Update rocznicy 10**

1.  Otwórz aplikację **Ustawienia** .
2.  Kliknij pozycję **konta** > **dostępu służbowe**.
3.  Kliknij przycisk **Połącz**.
4.  Kliknij przycisk **Dołącz tego urządzenia w celu Azure AD**.
5.  Uwierzytelniania dla Twojej organizacji, Przekaż uwierzytelnianie wieloskładnikowe, jeśli zostanie wyświetlony monit, a następnie postępuj zgodnie z instrukcjami, które są wyświetlane.
6.  Wyloguj się, a następnie zaloguj się przy użyciu swojego konta służbowego.
7.  Spróbuj ponownie uzyskać dostęp do aplikacji.


**Windows Update 10 listopada 2015 r.**

1.  Otwórz aplikację **Ustawienia** .
2.  Kliknij pozycję **System** > **o**.
3.  Kliknij przycisk **Dołącz Azure AD**.
4.  Uwierzytelniania dla Twojej organizacji, Przekaż uwierzytelnianie wieloskładnikowe, jeśli zostanie wyświetlony monit, a następnie postępuj zgodnie z instrukcjami, które są wyświetlane.
5.  Wyloguj się, a następnie zaloguj się przy użyciu swojego konta służbowego (konto Azure AD).
6.  Spróbuj ponownie uzyskać dostęp do aplikacji.

Aby dodać swoje konto służbowe, wykonaj następujące czynności:

**Windows Update rocznicy 10**

1.  Otwórz aplikację **Ustawienia** .
2.  Kliknij pozycję **konta** > **dostępu służbowe**.
3.  Kliknij przycisk **Połącz**.
4.  Uwierzytelniania dla Twojej organizacji, Przekaż uwierzytelnianie wieloskładnikowe, jeśli zostanie wyświetlony monit, a następnie postępuj zgodnie z instrukcjami, które są wyświetlane.
5.  Spróbuj ponownie uzyskać dostęp do aplikacji.


**Windows Update 10 listopada 2015 r.**

1.  Otwórz aplikację **Ustawienia** .
2.  Kliknij pozycję **konta** > **konta**.
3.  Kliknij pozycję **Dodaj służbowe lub szkolne konto**.
4.  Uwierzytelniania dla Twojej organizacji, Przekaż uwierzytelnianie wieloskładnikowe, jeśli zostanie wyświetlony monit, a następnie postępuj zgodnie z instrukcjami, które są wyświetlane.
5.  Spróbuj ponownie uzyskać dostęp do aplikacji.

Jeśli urządzenie jest domeny i uruchamia Windows 8.1, dołączanie do pracy i zarejestrować się w programie Microsoft Intune, wykonaj następujące czynności:

1.  Otwórz **Ustawienia komputera**.
2.  Kliknij pozycję **sieć** > **miejscu pracy**.
3.  Kliknij przycisk **Dołącz**.
4.  Uwierzytelniania dla Twojej organizacji, Przekaż uwierzytelnianie wieloskładnikowe, jeśli zostanie wyświetlony monit, a następnie postępuj zgodnie z instrukcjami, które są wyświetlane.
5.  Kliknij pozycję **Włącz**.
6.  Spróbuj ponownie uzyskać dostęp do aplikacji.


### <a name="browser-is-not-supported"></a>Przeglądarka nie jest obsługiwana

Użytkownik może mieć dostępu Jeśli próbujesz uzyskać dostęp do aplikacji lub usługi, stosując jedną z następujących przeglądarek:

- Chrome, Firefox lub inne przeglądarki innej niż Microsoft Edge lub programu Microsoft Internet Explorer w systemie Windows 10 lub Windows Server 2016
- Firefox w systemie Windows 8.1, Windows 7, Windows Server 2012 R2, systemu Windows Server 2012 lub Windows Server 2008 R2

Pojawi się Strona błędów, która wygląda następująco:

![Komunikat "Nie otrzymujesz dostępne w tym miejscu" nieobsługiwane przeglądarki] (./media/active-directory-conditional-access-device-remediation/02.png "Scenariusz")

Tylko działań naprawczych jest za pomocą przeglądarki obsługującej aplikacji dla swojej platformy urządzenia.

## <a name="next-steps"></a>Następne kroki

[Azure Active Directory warunkowego dostępu](active-directory-conditional-access.md)
