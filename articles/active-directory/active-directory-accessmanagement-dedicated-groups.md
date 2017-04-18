<properties
    pageTitle="Dedykowane grup w usłudze Active Directory platformy Azure | Microsoft Azure"
    description="Przegląd grup jak dedykowane pracować w usługi Azure Active Directory i jak są tworzone."
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
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="curtand"/>

# <a name="dedicated-groups-in-azure-active-directory"></a>Dedykowane grup w usłudze Active Directory platformy Azure

W usługi Azure Active Directory (Azure AD) funkcję dedykowany grupy automatycznie tworzy i wypełnia członkostwo Azure AD wstępnie zdefiniowane grupy. Członkowie grup dedykowane nie można dodać lub usunąć za pomocą portal Azure klasyczny, poleceń cmdlet programu Windows PowerShell, lub programowo.

>[AZURE.NOTE] Grupy dedykowanej Wymagaj przypisania licencji usługi Azure AD Premium do
>- administrator zarządzający reguły w grupie
>- Wszyscy użytkownicy, którzy są zaznaczone przez regułę należeć do grupy

**Aby włączyć dedykowane grup**

1. W [portalu klasyczny Azure](https://manage.windowsazure.com)wybierz pozycję **Usługi Active Directory**, a następnie otwórz katalogu organizacji.

2. Wybierz kartę **grupy** , a następnie otwórz grupę, którą chcesz edytować.

3. Wybierz kartę **Konfigurowanie** , a następnie ustaw **Włączanie grup przeznaczonych** na wartość **Tak**.

Po przełącznik Włącz dedykowany grupy jest ustawiony na wartość **Tak**, możesz włączyć dodatkowo katalogu automatycznie utworzyć grupę dedykowany wszystkich użytkowników, ustawiając **użytkownikom "wszystkie" grupy** przełączanie na wartość **Tak**. Następnie można również edytować nazwę tej grupy, dedykowane, wpisując je w **Nazwa wyświetlana dla "Wszystkich użytkowników" grupy** pola.

Grupy Wszyscy użytkownicy można przypisać te same uprawnienia do wszystkich użytkowników w katalogu. Na przykład możesz przyznać wszystkich użytkowników w katalogu dostęp do aplikacji władz akredytacji bezpieczeństwa, przypisywanie dostępu dla grupy dedykowane wszystkich użytkowników do tej aplikacji.

Dedykowane grupy Wszyscy użytkownicy zawiera wszystkich użytkowników w katalogu, w tym gości i użytkowników zewnętrznych. W razie potrzeby grupy który wyłącza użytkowników zewnętrznych, a następnie można to osiągnąć przez tworzenie grupy przy użyciu opartą na atrybutach reguły dynamiczne następujące:

                (user.userPrincipalName -notContains "#EXT#@")

W grupie, z wyłączeniem wszystkich gości za pomocą reguły następujące:

                (user.userType -ne "Guest")

Aby dowiedzieć się o tworzeniu reguły *Zaawansowane* (reguły, które mogą zawierać wiele porównania) dla członkostwa w grupach dynamiczne, zobacz [Używanie atrybuty, aby utworzyć reguły zaawansowane](active-directory-accessmanagement-groups-with-advanced-rules.md).


Te artykuły zawiera dodatkowe informacje dotyczące usługi Azure Active Directory.

* [Zarządzanie dostępem do zasobów z grupami usługi Azure Active Directory](active-directory-manage-groups.md)
* [Artykuł indeks do zarządzania aplikacjami w usłudze Azure Active Directory](active-directory-apps-index.md)
* [Co to jest Azure Active Directory?](active-directory-whatis.md)
* [Integrowanie tożsamości lokalnych z usługi Azure Active Directory](active-directory-aadconnect.md)
