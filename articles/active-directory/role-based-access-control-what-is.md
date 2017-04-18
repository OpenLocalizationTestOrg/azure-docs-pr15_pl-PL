<properties
    pageTitle="Kontrola dostępu oparta na rolach | Microsoft Azure"
    description="Wprowadzenie zarządzanie dostępem kontrola dostępu oparta na rolach Azure w Azure Portal. Aby przypisać uprawnienia w katalogu za pomocą przypisania roli."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/03/2016"
    ms.author="kgremban"/>

# <a name="get-started-with-access-management-in-the-azure-portal"></a>Wprowadzenie do zarządzania dostępu w portalu Azure

Zorientowane na zabezpieczeń firmy należy skoncentrować się na Dawanie pracownikom dokładnie uprawnienia, które są potrzebne. Zbyt wiele uprawnień udostępnia konta na ataki. Uprawnienia za mało oznacza, że pracownicy nie można pobrać wydajność pracy. Azure oparta na rolach programu Access Control (RBAC) pomaga rozwiązać ten problem, oferując zarządzania szerokiego dostępu dla Azure.

Przy użyciu RBAC, można oddzielnie różnic obowiązków w obrębie organizacji i udzielić tylko ilość dostępu użytkownikom, którzy potrzebują do wykonywania zadań. Zamiast określeniu wszystkich nieograniczony uprawnienia Azure subskrypcji lub zasoby, można zezwolić na używanie tylko niektórych akcji. Na przykład aby umożliwić jednego pracownika Zarządzanie maszyn wirtualnych w przypadku subskrypcji, podczas drugiego Zarządzanie baz danych programu SQL w tej samej subskrypcji za pomocą RBAC.

## <a name="basics-of-access-management-in-azure"></a>Podstawy zarządzania dostępu platformy Azure
Każdy Azure subskrypcji jest skojarzony z jednego katalogu Azure Active Directory (AD). Użytkownicy, grupy i aplikacji z tego katalogu umożliwia zarządzanie zasobami Azure subskrypcji. Przypisywanie tych praw dostępu przy użyciu Azure portal, Azure narzędzi wiersza polecenia i interfejsów API zarządzania Azure.

Udzielanie dostępu przypisując odpowiednią rolę RBAC do użytkowników, grupy i wniosków określonego zakresu. Zakres przypisania roli może być subskrypcji, grupa zasobów lub pojedynczego zasobu. Rola przypisana w zakresie nadrzędnej również udziela dostępu do elementów podrzędnych zawartych w nim. Na przykład użytkownika z dostępem do grupy zasobów można zarządzać wszystkie zasoby, które zawiera, takich jak witryny sieci Web, maszyn wirtualnych i podsieci.

![Relacja między elementami usługi Azure Active Directory — diagram](./media/role-based-access-control-what-is/rbac_aad.png)

Rola RBAC przypisujesz decyduje o zasobów użytkownikowi, grupie lub aplikacji można zarządzać w ramach tego zakresu.

## <a name="built-in-roles"></a>Role wbudowane
Azure RBAC obejmuje trzy role podstawowe, które dotyczą wszystkich typów zasobów:

- **Właściciel** ma pełny dostęp do wszystkich zasobów, w tym w prawo, aby pełnomocnictwa do innych osób.
- **Współautor** można tworzyć i zarządzać wszystkich typów zasobów Azure, ale nie może udzielić dostępu do innych osób.
- **Czytnik** można wyświetlać istniejące Azure zasobów.

Pozostałych ról RBAC Azure zezwolić na zarządzanie określonych Azure zasobów. Na przykład Rola współautora maszyn wirtualnych umożliwia tworzenie i zarządzanie nimi w środowisku maszyn wirtualnych systemu. Nie daje im dostępu do wirtualnej sieci lub podsieci, maszyna wirtualna połączony z.

[Role wbudowanych RBAC](role-based-access-built-in-roles.md) wyświetlane role dostępne w Azure. Określa operacje i zakres, który każdego wbudowanych rola zapewnia użytkownikom. Jeśli chcesz, aby określić własne ról zyskać więcej możliwości sterowania, zobacz, jak tworzyć [niestandardowe ról w Azure RBAC](role-based-access-control-custom-roles.md).

## <a name="resource-hierarchy-and-access-inheritance"></a>Dziedziczenie hierarchii i dostępu do zasobu
- Każdej **subskrypcji** w Azure należy tylko jednego katalogu.
- Każda **Grupa zasobów** należy tylko jedną subskrypcję.
- Każdy **zasób** należy do grupy tylko jeden zasób.

Dostęp dozwolony w zakresach nadrzędnej są dziedziczone na zakresy podrzędne. Na przykład:

- Rola czytnika można przypisać do grupy w usłudze Azure AD w zakresie subskrypcji. Członkowie tej grupy można wyświetlać każdej grupy zasobów i zasobów na subskrypcję.
- Rola współautora możesz przypisywać do aplikacji w zakresie grupy zasobów. Może ona zarządzać zasobów we wszystkich typach w danej grupy zasobów, ale nie inne grupy zasobów w subskrypcji.

## <a name="azure-rbac-vs-classic-subscription-administrators"></a>Azure RBAC a administratorzy klasyczny subskrypcji
Administratorzy klasyczny subskrypcji i administratorów współpracujących mieć pełny dostęp do Azure subskrypcji. Ich umożliwia zarządzanie zasobami [Azure portal](https://portal.azure.com) za pomocą interfejsów API Menedżera zasobów Azure lub modelu Klasyczny wdrożenia [Azure klasycznym portal](https://manage.windowsazure.com) i Azure. W modelu RBAC klasyczny Administratorzy przypisana rola właściciela w zakresie subskrypcji.

Tylko portal Azure i nowe składniki interfejsu API Menedżera zasobów Azure obsługują Azure RBAC. Użytkowników i aplikacje przypisanych ról RBAC nie można używać do portalu zarządzania klasyczny i model Azure wdrożenia klasyczny.

## <a name="authorization-for-management-vs-data-operations"></a>Zezwolenie na zarządzanie a operacje na danych
Azure RBAC obsługuje tylko operacje zarządzania zasobami Azure w Azure portal i interfejsów API Menedżera zasobów Azure. Nie można go Autoryzuj wszystkie operacje poziomu danych Azure zasobów. Na przykład można autoryzować innej osoby do zarządzania kontami miejsca do magazynowania, ale nie chcesz, aby blob lub tabel w ramach konta usługi przechowywania nie. Podobnie bazy danych SQL można zarządzać, ale nie tabele znajdujące się w nim.

## <a name="next-steps"></a>Następne kroki
- Rozpoczynanie pracy z [oparta na rolach kontrola dostępu w portalu Azure](role-based-access-control-configure.md).
- Zobacz [RBAC wbudowanych ról](role-based-access-built-in-roles.md)
- Definiowanie własnych [niestandardowych ról w Azure RBAC](role-based-access-control-custom-roles.md)
