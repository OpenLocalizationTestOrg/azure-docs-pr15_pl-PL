<properties
    pageTitle="Konfigurowanie usługi Azure Active Directory do zarządzania dostępu aplikacji Sklep internetowy | Microsoft Azure"
    description="Zarządzanie grupami Samoobsługowe pozwala użytkownikom na tworzenie i zarządzanie grupami zabezpieczeń lub grup usługi Office 365 w usługi Azure Active Directory i oferuje możliwość grupy zabezpieczeń żądania lub członkostwa w grupach usługi Office 365"
    services="active-directory"
    documentationCenter=""
  authors="curtand"
    manager="femila"
    editor=""
    />

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/10/2016"
    ms.author="curtand"/>

# <a name="setting-up-azure-active-directory-for-self-service-group-management"></a>Konfigurowanie usługi Azure Active Directory do zarządzania Samoobsługowe grupy

Zarządzanie grupami Samoobsługowe pozwala użytkownikom na tworzenie i zarządzanie grupami zabezpieczeń lub grupy usługi Office 365 w usłudze Azure Active Directory (Azure AD). Użytkownicy mogą również zażądać grupy zabezpieczeń lub członkostwa w grupach usługi Office 365, a następnie właściciela grupy można zaakceptować lub odrzucić członkostwa. W ten sposób można przekazać codzienną kontrolę nad członkostwo w grupie osób, które opis kontekst firm członkostwo. Funkcje zarządzania Samoobsługowe grup są dostępne tylko w przypadku grup zabezpieczeń i grupami usługi Office 365, ale nie do grupy zabezpieczeń z włączoną obsługą poczty i listami dystrybucyjnymi.

Zarządzanie grupami Samoobsługowe obecnie obejmuje dwa podstawowe scenariusze: delegowanej grupy i zarządzanie Samoobsługowe grupy.

- **Delegowane Zarządzanie grupami** 
   przykładem jest administratorem, który jest używana do zarządzania dostęp do aplikacji władz akredytacji bezpieczeństwa, która korzysta z firmy. Zarządzanie tych praw dostępu staje się kłopotliwe, aby ten administrator prosi właściciel firmy, aby utworzyć nową grupę. Administrator przypisuje dostępu dla aplikacji do nowej grupy i dodaje do grupy wszystkich osób już uzyskiwania dostępu do aplikacji. Następnie właściciel firmy można dodać więcej użytkowników i tych użytkowników są automatycznie obsługi administracyjnej aplikacji. Właściciel firmy nie trzeba czekać z administratorem, aby zarządzać dostępem użytkowników. Jeśli administrator udziela to samo uprawnienie do menedżera w grupie innej firmy, a następnie osoba również zarządzać dostępem dla własnych użytkowników. Właściciel firmy ani Menedżera można przeglądać i zarządzanie użytkownikami innych osób. Administrator nadal widoczne wszystkich użytkowników, którzy mają dostęp do aplikacji i praw dostępu do bloku w razie potrzeby.

- **Zarządzanie grupami samodzielne** 
   przykładem w tym scenariuszu jest dwóch użytkowników zarówno z witryn usługi SharePoint Online, zdefiniowanych przez ich niezależnie. Chce nadać innych osób członkom zespołu dostęp do witryn. Aby to zrobić, można utworzyć jedną grupę w Azure AD i w usłudze SharePoint Online każdego z nich zaznacza tej grupy, aby umożliwić dostęp do swoich witryn. Gdy ktoś chce programu access, ich żądanie z poziomu panelu dostępu, a po zatwierdzeniu oni uzyskiwać dostęp do obu witryn usługi SharePoint Online automatycznie. Później jeden z nich zdecyduje, że wszystkie osoby z dostępem do witryny należy również uzyskać dostęp do określonej aplikacji władz akredytacji bezpieczeństwa. Administrator aplikacji władz akredytacji bezpieczeństwa można dodać prawa dostępu do aplikacji do witryny usługi SharePoint Online. Następnie żądań, które uzyskania zatwierdzenia udostępnia dwa witryn usługi SharePoint Online, a także do tej aplikacji władz akredytacji bezpieczeństwa.

## <a name="making-a-group-available-for-end-user-self-service"></a>Udostępnianie grupie dla użytkownika końcowego Sklep internetowy

1. Otwórz katalog Azure AD w [portal Azure klasyczny](https://manage.windowsazure.com).

2. Na karcie **Konfigurowanie** ustaw **Zarządzanie grupami delegowana** włączone.

3. Ustaw **Użytkownicy mogą tworzyć grupy zabezpieczeń** lub **Użytkownicy mogą tworzyć grupy pakietu Office** na włączone.

Po włączeniu **Użytkownicy mogą tworzyć grupy zabezpieczeń** wszystkich użytkowników w katalogu mogą tworzyć nowe grupy zabezpieczeń i dodawanie członków do grupy. Tych nowych grup również będą widoczne w Panel dostępu dla wszystkich innych użytkowników. Jeśli ustawienie zasad grupy na to zezwala, inni użytkownicy będą mogli tworzyć żądania, aby dołączyć do grupy. Jeśli **Użytkownicy mogą tworzyć grupy zabezpieczeń** jest wyłączone, użytkownicy nie można tworzyć grup i nie można zmienić istniejących grup, do których zostały właściciela. Jednak mogą nadal Zarządzanie członkostwami tych grup i zatwierdzanie wniosków od innych użytkowników, aby dołączyć do grup.

Za pomocą **użytkowników, których można używać samodzielnego dla grup zabezpieczeń** do uzyskania bardziej precyzyjne sterowanie w dostępu do zarządzania Samoobsługowe grupy przez użytkowników. Po włączeniu **Użytkownicy mogą tworzyć grupy** wszystkich użytkowników w katalogu mogą tworzyć nowe grupy i dodawanie członków do grupy. Ustawiając również **użytkowników, których można używać samodzielnego dla grup zabezpieczeń** do niektórych, masz ograniczony Zarządzanie ograniczone grupy użytkowników grupy. Gdy ta opcja jest ustawiona na niektóre, możesz dodawać użytkowników do grupy SSGMSecurityGroupsUsers przed ich tworzenie nowych grup i dodawanie członków do nich. Ustawiając dla wszystkich **użytkowników, których można używać samodzielnego dla grup zabezpieczeń** , można włączyć wszystkich użytkowników w katalogu, aby utworzyć nową grupę.

Aby określić niestandardową nazwę grupy, której członkowie mogą korzystać z samodzielnego umożliwia także pola **grupy, które używają samodzielnego dla grup zabezpieczeń** .

## <a name="additional-information"></a>Dodatkowe informacje

Te artykuły zawiera dodatkowe informacje dotyczące usługi Azure Active Directory.

* [Zarządzanie dostępem do zasobów z grupami usługi Azure Active Directory](active-directory-manage-groups.md)

* [Azure cmdlet usługi Active Directory do konfigurowania ustawień grupy](active-directory-accessmanagement-groups-settings-cmdlets.md)

* [Artykuł indeks do zarządzania aplikacjami w usłudze Azure Active Directory](active-directory-apps-index.md)

* [Co to jest Azure Active Directory?](active-directory-whatis.md)

* [Integrowanie tożsamości lokalnych z usługi Azure Active Directory](active-directory-aadconnect.md)
