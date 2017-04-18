<properties
    pageTitle="Ustawianie zasad opartych na urządzeniach dostępu warunkowego dla aplikacji związanych z usługi Azure Active Directory | Microsoft Azure"
    description="Ustawienia zasad oparte na urządzeniu dostępie warunkowym dla Azure AD połączonych aplikacji."
    services="active-directory"
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
    ms.date="09/14/2016"
    ms.author="markvi"/>


# <a name="set-device-based-conditional-access-policy-for-azure-active-directory-connected-applications"></a>Ustawianie zasad opartych na urządzeniach dostępu warunkowego dla aplikacji połączonych z usługi Azure Active Directory


Azure Active Directory (Azure AD) opartych na urządzeniach dostępu warunkowego umożliwiają ochronę zasobów organizacji z:

- Access pozwala podjąć próbę z nieznanych lub niezarządzanej urządzeń.
- Urządzenia, które nie spełniają zasad zabezpieczeń firmy.

Aby uzyskać omówienie dostępu warunkowego zobacz [dostępu warunkowego usługi Azure Active Directory](active-directory-conditional-access.md).

Można ustawić zasady chroniące te aplikacje oparte na urządzeniu dostępie warunkowym:

- Usługa Office 365 SharePoint Online ochrony organizacji witryn i dokumentów
- Usługa Office 365 Exchange Online ochrony poczty e-mail w Twojej organizacji
- Oprogramowanie jako aplikacjami usług (SaaS), które są połączone z Azure AD dla uwierzytelniania
- Aplikacje, które zostały opublikowane za pomocą Azure AD serwera Proxy aplikacji usług lokalnego

Aby ustawić zasady oparte na urządzeniu dostępie warunkowym, w portalu Azure, przejdź do określonej aplikacji w katalogu.


  ![Lista aplikacji w katalogu portal Azure] (./media/active-directory-conditional-access-policy-connected-applications/01.png "Aplikacje")


Wybierz aplikację, a następnie kliknij kartę **Konfiguruj** , aby ustawić zasady dostępu warunkowego.  


  ![Konfigurowanie aplikacji] (./media/active-directory-conditional-access-policy-connected-applications/02.png "Reguły dostępu oparty na urządzeniach")




Aby ustawić zasady oparte na urządzeniu dostępie warunkowym, w sekcji **reguły dostępu oparty na urządzeniach** w **Włącz reguły dostępu**, wybierz **na**.

Zasady oparte na urządzeniu dostępie warunkowym występują trzy elementy:

- **Stosowanie do**. Zakres użytkowników, których dotyczą zasady.

- **Reguły urządzenia**. Warunki urządzenie musi spełniać, aby umożliwić dostęp do aplikacji.

- **Wymuszanie stosowania**. Aplikacje klienckie (w trybie macierzystym i przeglądarki) dotyczy zasady.

  ![Trzy części zasady dostępu opartego na urządzeniu] (./media/active-directory-conditional-access-policy-connected-applications/03.png "Reguły dostępu oparty na urządzeniach")


## <a name="select-the-users-the-policy-applies-to"></a>Zaznacz użytkowników, których dotyczy zasady

W sekcji **Zastosuj do** można wybrać zakres użytkowników, których dotyczą te zasady.

Podczas tworzenia zakresów zasady dostępu dla użytkowników, masz dwie opcje:

- **Wszyscy użytkownicy**. Stosowanie zasad dla wszystkich użytkowników, którzy uzyskiwać dostęp do aplikacji.

- **Grupy**. Ogranicz zasad do użytkowników, którzy jest członkiem określonej grupy.

![Stosowanie zasad dla wszystkich użytkowników lub grupy] (./media/active-directory-conditional-access-policy-connected-applications/11.png "Stosowanie do")


 Aby wykluczyć użytkownika z zasady, zaznacz pole wyboru **z wyjątkiem** . Jest to pomocne w przypadku, gdy trzeba nadać uprawnienia do określonego użytkownika, aby tymczasowo uzyskać dostęp do aplikacji. Wybierz tę opcję, na przykład, jeśli masz niektórych użytkowników urządzeń, które nie są gotowe do dostępu warunkowego. Urządzenia mogą nie być zarejestrowana, lub są one zbliża się nie jest zgodny.


## <a name="select-the-conditions-that-devices-must-meet"></a>Wybierz odpowiednie warunki, które muszą spełniać urządzeń

Przy użyciu **Reguły urządzenia** Ustaw warunki urządzenie musi spełnić, aby uzyskać dostęp do aplikacji.

Oparte na urządzeniu dostępie warunkowym można ustawić dla następujących typów urządzeń:

- Aktualizacja rocznicy systemu Windows 10, Windows 8.1 i Windows 7
- System Windows Server 2016, systemu Windows Server 2012 R2, systemu Windows Server 2012 i Windows Server 2008 R2
- urządzeń z systemem iOS (iPad, iPhone)
- Urządzeń z systemem android

Pomoc techniczna dla komputerów Mac jest już wkrótce.

  ![Zastosuj zasady urządzenia] (./media/active-directory-conditional-access-policy-connected-applications/04.png "Aplikacje")

 >[AZURE.NOTE] Aby uzyskać informacje o różnicach między urządzeń domeny i Azure sprzężone AD zobacz [urządzenia za pomocą systemu Windows 10 w miejscu pracy](active-directory-azureadjoin-windows10-devices.md).

Istnieją dwie opcje reguły urządzenia:

- **Wszystkie urządzenia muszą być zgodne**. Wszystkie platformy urządzeń łączących się z aplikacji musi spełniać. Urządzenia uruchamiane na platformach, które nie obsługują dostępu warunkowego opartego na urządzeniu są dostępu.

- **Tylko zaznaczonego urządzenia musi spełniać**. Tylko konkretnego urządzenia platformy musi spełniać. Innych platformach lub innych platform, które mogą uzyskiwać dostęp do aplikacji mają dostęp.

  ![Ustawianie zakresu reguły urządzenia] (./media/active-directory-conditional-access-policy-connected-applications/05.png "Aplikacje")

Azure AD połączonych urządzeń są zgodne, jeśli są oznaczone jako **zgodne** z katalogu przez Intune lub systemu zarządzania urządzenia przenośnego innych firm, w którym można zintegrować z usługą Azure Active Directory.

Urządzenie domeny jest zgodny z jeśli:

- Jest zarejestrowany z usługą Azure Active Directory. Wiele organizacji Potraktuj domeny urządzeń jako zaufanej urządzenia.
- Nie została oznaczona jako **zgodności** w Azure AD przez System Centrum Menedżer konfiguracji 2016.

 ![Domeny urządzeń, które są zgodne z] (./media/active-directory-conditional-access-policy-connected-applications/06.png "Reguły urządzenia")

Osobiste urządzenia z systemem Windows są zgodne, jeśli są oznaczone jako **zgodne** z katalogu przez Intune lub systemu zarządzania urządzenia przenośnego innych firm, w którym można zintegrować z usługą Azure Active Directory.

Urządzenia z systemem Windows nie są zgodne, jeśli są oznaczone jako **zgodne** z katalogu przez Intune.

 >[AZURE.NOTE] Aby uzyskać więcej informacji na temat sposobu konfigurowania Azure AD w celu zapewnienia zgodności urządzenia w systemach zarządzania różnych zobacz [dostępu warunkowego usługi Azure Active Directory](active-directory-conditional-access.md).


Możesz wybrać jeden lub wiele platformy urządzeń zasady dostępu opartego na urządzeniu. Ta opcja uwzględnia Android, iOS, systemu Windows Mobile (Windows 8.1 telefonów i tabletów) i systemu Windows (wszystkie inne Windows urządzenia, w tym wszystkich urządzeń systemu Windows 10).
Ocenianie zasad występuje tylko na wybranych platform. Jeśli urządzenie, które próbuje uzyskać dostęp nie jest uruchomiony jeden z wybranych platform, urządzenie dostęp do aplikacji, gdy użytkownik ma dostęp. Nie zasad urządzeń jest obliczane.

![Wybierz pozycję platformy dla reguły urządzenia] (./media/active-directory-conditional-access-policy-connected-applications/07.png "Reguły urządzenia")


## <a name="set-policy-evaluation-for-a-type-of-application"></a>Ustawianie zasad oceny dla danego typu aplikacji

W sekcji **Wymuszania aplikacji** Ustaw typ aplikacji, których zasady oceni dla użytkownika lub dostęp za pomocą urządzeń.

Istnieją dwie opcje typ aplikacji, aby uwzględnić:

- Przeglądarki i natywnej aplikacji
- Tylko natywnej aplikacji

![Wybieranie przeglądarki lub macierzyste aplikacje] (./media/active-directory-conditional-access-policy-connected-applications/08.png "Aplikacje")

Aby wymusić zasady dostępu dla aplikacji, wybierz pozycję **dla przeglądarki i natywnej aplikacji**. Następnie można dołączać:

- Przeglądarki Safari w systemie iOS lub (na przykład Microsoft Edge w systemie Windows 10).
- Aplikacje używające biblioteki uwierzytelniania Active Directory (ADAL) na dowolnej platformie lub używający WebAccountManager (WAM) interfejsu API w systemie Windows 10.

>[AZURE.NOTE] Aby uzyskać więcej informacji na temat obsługi przeglądarki i inne uwagi dotyczące użytkownik uzyskuje dostęp do opartych na urządzeniach, aplikacji z ochroną urząd certyfikacji, zobacz [dostępu warunkowego usługi Azure Active Directory](active-directory-conditional-access.md).

## <a name="help-protect-email-access-from-exchange-activesync-based-applications"></a>Ochrona dostępu do poczty e-mail z aplikacji opartych na programie Exchange ActiveSync

W aplikacjach Office 365 Exchange Online możesz zablokować dostępu do poczty e-mail do aplikacji poczty programu Exchange ActiveSync za pomocą programu Exchange ActiveSync.

![Opcje zgodności w programie Exchange ActiveSync] (./media/active-directory-conditional-access-policy-connected-applications/09.png "Aplikacje")

![Wymaganie zgodności urządzenia, aby uzyskać dostęp do poczty e-mail] (./media/active-directory-conditional-access-policy-connected-applications/10.png "Aplikacje")


## <a name="next-steps"></a>Następne kroki

- [Azure Active Directory warunkowego dostępu](active-directory-conditional-access.md)
