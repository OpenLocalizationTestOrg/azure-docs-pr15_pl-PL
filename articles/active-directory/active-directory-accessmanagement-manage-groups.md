<properties
    pageTitle="Zarządzanie grupami usługi Azure Active Directory | Microsoft Azure"
    description="Jak utworzyć i zarządzanie grupami do zarządzania użytkownikami Azure za pomocą usługi Azure Active Directory."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/29/2016"
    ms.author="curtand"/>


# <a name="managing-groups-in-azure-active-directory"></a>Zarządzanie grupami w usłudze Active Directory platformy Azure

> [AZURE.SELECTOR]
- [Azure portal](active-directory-groups-create-azure-portal.md)
- [Portal Azure klasyczny](active-directory-accessmanagement-manage-groups.md)
- [Programu PowerShell](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)


Jedna z funkcji zarządzania użytkownikami usługi Azure Active Directory (Azure AD) jest możliwość tworzenia grup użytkowników. Grupy umożliwiają wykonywanie zadań zarządzania, takie jak przypisywanie licencji lub uprawnień do kilku użytkowników jednocześnie. Grupy również służy do przypisywania uprawnień dostępu do

- Zasobów, takich jak obiekty w katalogu
- Zasoby zewnętrzne do katalogu, takich jak aplikacje władz akredytacji bezpieczeństwa, usług Azure, witryn programu SharePoint lub zasobów lokalnych

Ponadto właścicielem zasobu można także przypisać dostępu do zasobu do grupy w usłudze Azure AD należącą do innej osoby. Przydziale udziela członków tej grupy dostępu do tego zasobu. Następnie właściciela grupy zarządza członkostwem w grupie. Efektywne właściciel zasobu zobowiązuje właściciela grupy uprawnień można przypisać użytkowników do ich zasobów.

## <a name="how-do-i-create-a-group"></a>Jak utworzyć grupę?

W zależności od usług, do których subskrybowanych Twojej organizacji możesz utworzyć grupę przy użyciu jednej z następujących czynności:
- portal usługi Azure klasyczny
- portal konta usługi Office 365
- portal konta usługi Windows Intune

Firma Microsoft będzie opisano zadania, wykonana w portalu klasyczny Azure. Aby uzyskać więcej informacji na temat używania innych niż Azure portali do zarządzania katalogu Azure AD zobacz [Administrowanie katalogu Azure AD](active-directory-administer.md).

1. W [portalu klasyczny Azure](https://manage.windowsazure.com)wybierz pozycję **Usługi Active Directory**, a następnie wybierz nazwę katalogu dla Twojej organizacji.

2. Wybierz kartę **grupy** .

3. Wybierz pozycję **Dodaj grupę**.

4. W oknie **Dodawanie grupy** Określ nazwę i opis grupy.


## <a name="how-do-i-add-or-remove-individual-users-in-a-security-group"></a>Jak dodać lub usunąć poszczególni użytkownicy w grupie zabezpieczeń

**Dodawanie poszczególnych użytkowników do grupy**

1. W [portalu klasyczny Azure](https://manage.windowsazure.com)wybierz pozycję **Usługi Active Directory**, a następnie wybierz nazwę katalogu dla Twojej organizacji.

2. Wybierz kartę **grupy** .

3. Otwórz grupę, do której chcesz dodać członków. Otwórz kartę **członków** zaznaczonej grupy, jeśli go nie jest już wyświetlana.

4. Wybierz pozycję **Dodaj członków**.

5. Na stronie **Dodawanie członków** wybierz nazwę użytkownika lub grupy, do której chcesz dodać jako członek tej grupy. Upewnij się, że ta nazwa został dodany do okienka **zaznaczone** .


**Aby usunąć poszczególnych użytkowników z grupy**

1. W [portalu klasyczny Azure](https://manage.windowsazure.com)wybierz pozycję **Usługi Active Directory**, a następnie wybierz nazwę katalogu dla Twojej organizacji.

2. Wybierz kartę **grupy** .

3. Otwieranie grupy, z której chcesz usunąć członków.

4. Wybierz kartę **członków** , wybierz nazwę członka, który chcesz usunąć z tej grupy, a następnie kliknij przycisk **Usuń**.

6. Upewnij się w wierszu, że chcesz usunąć ten element z grupy.


## <a name="how-can-i-manage-the-membership-of-a-group-dynamically"></a>Jak zarządzać członkostwem grupy dynamicznie?

W Azure AD bardzo łatwo można ustawić Prosta reguła do określenia, którzy użytkownicy mają być członków grupy. Prosta reguła jest bardziej tylko jednego porównanie. Na przykład jeśli grupa jest przypisana do aplikacji władz akredytacji bezpieczeństwa, możesz skonfigurować reguły dodawać użytkowników, którzy mają stanowisko "Stanowisku Przedstawiciel handlowy." Ta reguła następnie udziela dostępu do tej aplikacji władz akredytacji bezpieczeństwa dla wszystkich użytkowników z tej stanowisko w katalogu.

Podczas atrybutów zmiany użytkownika systemu wszystkie reguły dynamiczne grupy w katalogu, aby sprawdzić, jeśli zmianę atrybutu użytkownika spowoduje dowolnej grupie dodaje lub usuwa. Jeśli użytkownik spełnia reguły w grupie, dodanych jako członka do tej grupy. Jeśli nie jest już spełniają zasady grupy, do której są one członkiem, są usuwane jako członków z tej grupy.

> [AZURE.NOTE] Możesz skonfigurować reguły dla dynamicznego członkostwa na grupy zabezpieczeń lub grup usługi Office 365. Członkostwa w grupach zagnieżdżonych nie są obecnie obsługiwane oparte na grupach przydziału dla aplikacji.
>
> Dynamiczne członkostwa grupy wymagają licencji Azure AD Premium ma być przypisane do
>
> - Administrator zarządzający reguły w grupie
> - Dla wszystkich członków grupy

**Aby umożliwić dynamiczne członkostwo w grupie**

1. W [portalu klasyczny Azure](https://manage.windowsazure.com)wybierz pozycję **Usługi Active Directory**, a następnie wybierz nazwę katalogu dla Twojej organizacji.

2. Wybierz kartę **grupy** , a następnie otwórz grupę, którą chcesz edytować.

3. Wybierz kartę **Konfigurowanie** , a następnie ustaw **Włączyć dynamiczne członkostwa** na wartość **Tak**.

4. Konfigurowanie prostego pojedynczą regułę dla grupy, aby kontrolować sposób dynamiczne członkostwa dla tej funkcji grupy. Upewnij się, **Dodawanie użytkowników miejsce, w którym** opcja jest zaznaczona, a następnie wybierz właściwości użytkownika z listy (na przykład dział, stanowisko, itp.)

5. Następnie wybierz warunek (nie równa się równa się, nie rozpoczyna się od, rozpoczyna się od, nie zawiera, zawiera, nie pasują do siebie, Uwzględnij).

6. Określ wartość porównania dla właściwości wybranego użytkownika.

Aby dowiedzieć się o tworzeniu reguły *Zaawansowane* (reguły, które mogą zawierać wiele porównania) dla członkostwa w grupach dynamiczne, zobacz [Używanie atrybuty, aby utworzyć reguły zaawansowane](active-directory-accessmanagement-groups-with-advanced-rules.md).

## <a name="additional-information"></a>Dodatkowe informacje

Te artykuły zawiera dodatkowe informacje dotyczące usługi Azure Active Directory.

* [Zarządzanie dostępem do zasobów z grupami usługi Azure Active Directory](active-directory-manage-groups.md)

* [Azure cmdlet usługi Active Directory do konfigurowania ustawień grupy](active-directory-accessmanagement-groups-settings-cmdlets.md)

* [Artykuł indeks do zarządzania aplikacjami w usłudze Azure Active Directory](active-directory-apps-index.md)

* [Co to jest Azure Active Directory?](active-directory-whatis.md)

* [Integrowanie tożsamości lokalnych z usługi Azure Active Directory](active-directory-aadconnect.md)
