<properties
    pageTitle="# Konfigurowanie rejestracji urządzenia automatycznego dla systemu Windows 7 domeny sprzężone urządzeń | Microsoft Azure"
    description="Kroki w celu skonfigurowania domeny systemu Windows 7 sprzężone urządzenia, aby automatycznie zarejestruj się z usługą Azure Active Directory. i czynności, aby wdrożyć pakiet oprogramowania rejestracji urządzenia z domeną systemu Windows 7 połączonych urządzeń z systemem dystrybucji oprogramowania takich jak Menedżera konfiguracji centrum systemowego."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="MarkVi"/>

# <a name="configure-automatic-device-registration-for-windows-7-domain-joined-devices"></a>Konfigurowanie rejestracji urządzenia automatycznego dla urządzeń domeny sprzężone systemu Windows 7

Jako administrator systemu informatycznego może skonfigurować urządzenia domeny sprzężone systemu Windows 7 automatycznie zarejestrować z usługą Azure Active Directory. W tym celu należy wdrożyć pakiet oprogramowania rejestracji urządzenia z domeną systemu Windows 7 sprzężone urządzeń z systemem dystrybucji oprogramowania takich jak Menedżera konfiguracji centrum systemowego. Pamiętaj przeczytać i uzupełnić wymagania wstępne wymienione w automatycznej rejestracji urządzenia z urządzeniami Azure Active Directory dla systemu Windows domeny.

>[AZURE.NOTE]
 Aby uzyskać najnowsze instrukcje dotyczące konfigurowania rejestracji urządzenia automatycznego Zobacz, [jak skonfigurować automatyczne rejestracji domeny systemu Windows sprzężone urządzeń z usługą Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).

##<a name="installing-the-device-registration-software-package-on-windows-7-domain-joined-devices"></a>Instalowanie pakietu oprogramowania rejestracji urządzenia w systemie Windows 7 domeny sprzężone urządzeń

Rejestracja urządzenia w systemie Windows 7 jest dostępna [do pobrania pakietu MSI](https://connect.microsoft.com/site1164). Pakiet musi być zainstalowana na komputerach systemu Windows 7, dołączonych do domeny usługi Active Directory. Należy wdrożyć pakiet przy użyciu systemu dystrybucji oprogramowania takich jak Menedżera konfiguracji centrum systemowego. Pakiet MSI obsługuje opcji Standardowy instalację za pomocą / quiet parametru.
Pakiet oprogramowania jest dostępna do pobrania w [witrynie sieci Web Microsoft Connect](https://connect.microsoft.com/site1164). Tutaj można wybrać, a następnie Pobierz obszar roboczy dołączanie dla systemu Windows 7.

![](./media/active-directory-conditional-access/device-registration-process-windows7.gif)

## <a name="workplace-join-with-azure-active-directory"></a>Dołączanie do pracy z usługą Azure Active Directory
Rejestracja urządzenia dla systemu Windows 7 domeny sprzężone urządzeń nie wymaga ani nie zawiera elementów interfejsu użytkownika. Po zainstalowaniu na komputerze, każdy użytkownik domeny, który loguje się do komputera zostanie automatycznie i w trybie cichym zarejestrowany z obiektu urządzenia w Azure AD. Azure AD dla każdego użytkownika zarejestrowanych urządzenia fizycznego będzie się jeden obiekt urządzenia.

Instalator tworzy zaplanowane zadanie w systemie działa w kontekście użytkownika, który zostanie wywołana podczas logowania się użytkownika. Zadanie powoduje ciche zarejestrowanie użytkownika i urządzenie z usługą Azure Active Directory po znaki w użytkownika zostanie zakończone.
Zaplanowane zadanie można znaleźć w bibliotece harmonogramu zadań w obszarze **Microsoft** > **Dołączanie do obszaru roboczego**.
Zadanie zostanie uruchomione i rejestrowania wszystkich użytkowników usługi Active Directory, których Zaloguj się do komputera.
Poniższej ilustracji przedstawiono procedury krok po kroku dla rejestracji urządzenia automatycznego.

![](./media/active-directory-conditional-access/automatic-device-registration-windows7.png)

1. (Informacje pracownik) loguje się na komputerze klienckim systemu Windows 7 przy użyciu poświadczeń domeny w usłudze Active Directory.
1. Dołączanie miejscu pracy zadania według harmonogramu zostanie uruchomione.
1. Bezgłośną uwierzytelnieniu użytkownika z usług AD FS przy użyciu zintegrowanego uwierzytelniania systemu Windows.
1. Azure AD zarejestrowanie użytkownikowi komputerze systemem Windows 7.
1. Obiekt urządzenia i certyfikat zostanie utworzona w Azure AD. Obiekt reprezentuje user@device.
1. Dołączanie do obszaru roboczego certyfikat jest przechowywany na komputerze.

## <a name="unregistering-your-windows-7-domain-joined-devices"></a>Wyrejestrowywanie domeny systemu Windows 7 sprzężone urządzeń

Możesz wybrać unregister urządzenia systemu Windows 7 domeny sprzężone, wykonując następujące czynności: Odinstalowywanie dołączanie pracy pakietu oprogramowania z domeny systemu Windows 7 połączonych za pomocą urządzenia z systemem dystrybucji oprogramowania takich jak Menedżera konfiguracji centrum systemu.

Następnie otwórz wiersz polecenia na komputerze z systemem Windows 7 i wykonać następujące polecenie, aby unregister na urządzeniu:

    %ProgramFiles%\Microsoft Workplace Join\AutoWorkplace.exe /leave

>[AZURE.NOTE]
>To polecenie musi być uruchamiane w kontekście każdego użytkownika domeny, który podpisał do komputera.
Przeglądarka zdarzeń i błędów dla systemu Windows 7 domeny sprzężone urządzenia.

Dziennika zdarzeń systemu Windows na komputerze z systemem Windows 7 będzie wyświetlana wiadomości związane dołączyć do obszaru roboczego. Wiadomości można znaleźć zarówno pomyślne, jak i niepowodzeniem zdarzenia dołączanie do obszaru roboczego. Dziennik zdarzeń można znaleźć w przypadku Podgląd w obszarze Dzienniki aplikacji i usług > Dołączanie miejscu pracy firmy Microsoft.

## <a name="additional-topics"></a>Dodatkowe tematy

- [Omówienie usługi Azure Active Directory Rejestracja urządzenia](active-directory-conditional-access-device-registration-overview.md)
- [Rejestracja urządzenia automatyczne z urządzeniami Azure Active Directory for Windows Domain-Joined](active-directory-conditional-access-automatic-device-registration.md)
- [Konfigurowanie rejestracji urządzenia automatycznego dla urządzeń domeny sprzężone Windows 8.1](active-directory-conditional-access-automatic-device-registration-windows-8-1.md)
- [Rejestracja urządzenia automatyczne z urządzeniami domeny Azure Active Directory dla systemu Windows 10](active-directory-azureadjoin-devices-group-policy.md)
