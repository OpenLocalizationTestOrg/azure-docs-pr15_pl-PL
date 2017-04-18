<properties
    pageTitle="Konfigurowanie rejestracji urządzenia automatycznego dla urządzeń domeny sprzężone Windows 8.1 | Microsoft Azure"
    description=" Czynności, aby skonfigurować zasady grupy dla systemu Windows 8.1 urządzenia domeny, aby automatycznie zarejestrować z usługą Azure Active Directory. "
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
    ms.author="Markvi"/>

# <a name="configure-automatic-device-registration-for-windows-81-domain-joined-devices"></a>Konfigurowanie rejestracji urządzenia automatycznego dla urządzeń domeny sprzężone Windows 8.1

Zasad grupy Active Directory umożliwia konfigurowanie urządzeń Windows 8.1 domeny sprzężone automatycznie zarejestrować z usługą Azure Active Directory. Aby skonfigurować zasady grupy, musisz mieć co najmniej jedną domenę przyłączonym Windows Server 2012 R2 lub Windows 8.1 komputerze za pomocą funkcji zarządzania zasadami grupy zainstalowany. Po włączeniu tych zasad grupy dla domeny każdy użytkownik domeny, który loguje się do komputera zostanie automatycznie i w trybie cichym zarejestrowany z obiektu urządzenia w Azure AD. Azure AD dla każdego użytkownika zarejestrowanych urządzenia fizycznego będzie się jeden obiekt urządzenia. Pamiętaj przeczytać i uzupełnić wymagania wstępne z automatycznej rejestracji urządzenia z urządzeniami Azure Active Directory for Windows Domain-Joined.

>[AZURE.NOTE]
 Aby uzyskać najnowsze instrukcje dotyczące konfigurowania rejestracji urządzenia automatycznego Zobacz, [jak skonfigurować automatyczne rejestracji domeny systemu Windows sprzężone urządzeń z usługą Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).



## <a name="configure-the-group-policy-for-your-windows-81-domain-joined-devices"></a>Konfigurowanie zasad grupy dla urządzenia domeny sprzężone Windows 8.1

1. Otwórz Menedżera serwera i przejdź do **Narzędzia** > **Zarządzania zasadami grupy**.
2. Z zarządzania zasadami grupy przejdź do węzła domeny, który odpowiada domeny, w której chcesz włączyć **Automatyczne dołączanie do obszaru roboczego**.
3. Kliknij prawym przyciskiem myszy **Obiekty zasad grupy** i wybierz pozycję **Nowy**. Nazwę obiektu zasad grupy, na przykład **Automatyczne dołączanie do obszaru roboczego**. Kliknij **przycisk OK**.
4. Kliknij prawym przyciskiem myszy nowy obiekt zasad grupy, a następnie kliknij przycisk **Edytuj**.
5. Przejdź do **konfiguracji komputera** > **zasady** > **Szablony administracyjne** > **składniki Windows** > **Dołączanie do obszaru roboczego**.
6. Kliknij prawym przyciskiem myszy automatycznie komputerów klienckich sprzężenia miejscu pracy, a następnie wybierz pozycję **Edytuj**.
7. Zaznacz opcję włączone, a następnie kliknij przycisk Zastosuj. Kliknij **przycisk OK**.
8. Obiekt zasad grupy może teraz połączyć wybraną lokalizację. Aby włączyć tych zasad dla wszystkich urządzeń Windows 8.1 domeny sprzężone w organizacji, łącza zasad grupy do domeny.

## <a name="unregistering-your-windows-81-domain-joined-devices"></a>Wyrejestrowywanie domeny Windows 8.1 sprzężone urządzeń

Możesz wybrać unregister domeny sprzężone urządzeniach Windows 8.1, wykonując następujące czynności: modyfikowanie ustawień zasad grupy dołączanie w miejscu pracy utworzone w poprzedniej sekcji. Ustawianie automatycznie pracy sprzężenia komputerach zasady klienta na wyłączone. Uniemożliwi nowych urządzeń automatycznego dołączania do obszaru roboczego.

Wyrejestrowywanie istniejących komputerów Windows 8.1 domeny sprzężone według następujących jedną z dwóch poniższych opcji:

* Opcja 1: **domeny Unregister Windows 8.1 sprzężone urządzenia przy użyciu ustawienia komputera**
  1. Na urządzeniu Windows 8.1, przejdź do **Obszaru Ustawienia komputera** > **sieci** > **pracy**
  2. Wybierz pozycję **Opuść**.
Ten proces należy powtórzyć dla każdego użytkownika domeny, który jest zalogowany na komputerze i została automatycznie sprzężone miejscu pracy.

* Opcja 2: Unregister Windows 8.1 domeny urządzenia sprzężonych, za pomocą skryptu
    1. Otwórz wiersz polecenia na komputerze z systemem Windows 8.1, a następnie wykonaj następujące polecenie:` %SystemRoot%\System32\AutoWorkplace.exe leave`
   
To polecenie musi być uruchamiane w kontekście każdego użytkownika domeny, który podpisał do komputera.

##<a name="event-viewer--errors-for-windows-81-domain-joined-devices"></a>Podgląd zdarzeń i błędów dla systemu Windows 8.1 domeny sprzężone urządzeń

Dziennik zdarzeń systemu Windows na komputerze z systemem Windows 8.1 wyświetla wiadomości związane Rejestracja urządzenia. Zarówno pomyślne, jak i niepowodzeniem zdarzenia znajdują się wiadomości. 

Dziennik zdarzeń można znaleźć w przypadku Podgląd w obszarze aplikacji i usług **Dzienniki** > **Microsoft** > **Windows > Dołącz do pracy**.

##<a name="additional-details"></a>Dodatkowe informacje

Zasady grupy umożliwia zaplanowane zadanie w systemie działa w kontekście użytkownika, który zostanie wywołana podczas logowania się użytkownika. Zadanie zostanie cichym rejestrować użytkownika i urządzenie Azure AD po logowania. Zaplanowane zadanie można znaleźć w systemie Windows 8.1 urządzeń w Biblioteka Harmonogramu zadań w **programie Microsoft** > **Windows** > **Dołączanie do obszaru roboczego**. Zadanie zostanie uruchomione i rejestrowanie wszystkich użytkowników usługi Active Directory tego logowania do. 

## <a name="additional-topics"></a>Dodatkowe tematy
- [Omówienie usługi Azure Active Directory Rejestracja urządzenia](active-directory-conditional-access-device-registration-overview.md)
- [Rejestracja urządzenia automatyczne z urządzeniami domeny Azure Active Directory dla systemu Windows 10](active-directory-conditional-access-automatic-device-registration.md)
- [Konfigurowanie rejestracji urządzenia automatycznego dla urządzeń domeny sprzężone systemu Windows 7](active-directory-conditional-access-automatic-device-registration-windows7.md)

